<p align="center">
 <img width="100px" src="https://github.com/Dreamacro/clash/raw/master/docs/logo.png" align="center" alt="GitHub Readme Stats" />
 <h2 align="center">Clash Rules Lite</h2>
 
 <p align="center">🍒 自定义代理规则，精简匹配规则 (<b>代理匹配速度明显提升</b>)。</p>
 
 <p align="center">
  <a href="https://github.com/zhanyeye/clash-rules-lite/blob/master/.github/workflows/release.yml">
   <img src="https://github.com/zhanyeye/clash-rules-lite/actions/workflows/release.yml/badge.svg" />
  </a>
 </p>
 
 <p>
  <pre align="center">https://cdn.jsdelivr.net/gh/zhanyeye/clash-rules-lite@release/rules.txt</pre>
 </p>
 <p align="center"><a href="https://github.com/zhanyeye/clash-rules-lite/blob/master/rules.txt">代理规则列表</a></p>
</p>



#### 使用方法
1. fork 本仓库：[Fork zhanyeye/clash-rules-lite](https://github.com/zhanyeye/clash-rules-lite/fork) 

2. 鼠标右击订阅的配置文件选中“复制”，将复制的文件命名为`local`（因为更新订阅链接时会覆盖你的修改）

<div>
    <img width="750" src="https://user-images.githubusercontent.com/35565811/184479698-dbc0f06b-7313-4448-a694-cad3d9d5dbe3.png">
</div>


3. 在你复制的 `local` 配置中，修改配置如下，注意 `proxies`, `proxy-groups` 和 `{YOUR-GITHUB-USERNAME}` 修改为你的配置（加粗的部分）


<pre><code> 
mixed-port: 7890
allow-lan: true
bind-address: '*'
mode: rule
log-level: silent
external-controller: '127.0.0.1:9090'
proxies:
    <b>- { name: '1-香港', type: *, server: **, port: *, cipher: **, password: **, udp: true }</b>
    <b>- { name: '2-香港', type: *, server: **, port: *, cipher: **, password: **, udp: true }</b>
proxy-groups:
    <b>- { name: 'PROXY', type: select, proxies: ['1-香港', '2-香港'] }</b>
rules:
  - DOMAIN-KEYWORD,github,PROXY
  - DOMAIN-KEYWORD,google,PROXY
  - RULE-SET,pac,PROXY
rule-providers:
  pac:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/<b>{YOUR-GITHUB-USERNAME}</b>/clash-rules-lite@release/rules.txt"
    path: ./rules/pac.yaml
    interval: 86400
script:
  code: |
    def main(ctx, metadata):
        keywords = ["google", "github"]
        for key in keywords:
            if key in metadata["host"]:
                return "PROXY"
        if ctx.rule_providers["pac"].match(metadata):
            return "PROXY"
        else:
            return "DIRECT"

</code></pre>


3. 运行修改后的 `local` 配置，再切换成 `Rule` 或 `Script` 模式
<div>
    <img width="750" src="https://user-images.githubusercontent.com/35565811/184479791-6e2c12ca-d28f-4009-839a-e9a06bdcff00.png">
</div>


#### 自定义代理规则
修改仓库中`rules.txt`，修改完后会自动更新并邮件提醒，然后在 Clash 上刷新 providers
<div>
    <img width="750" src="https://user-images.githubusercontent.com/35565811/184480450-c24dd895-2b8a-4cfb-8f9e-77843c3df5af.png">
</div>

#### 高阶玩法
+ 使用python脚本解析订阅链接，自动生成改配置文件，需要你在 Python 代码中设置好你的订阅链接，代码见：[generate_config_quickly.py](https://github.com/zhanyeye/clash-rules-lite/blob/main/generate_config_quickly.py)
+ 注意该解析脚本是基于本人的订阅链接编写的，不一定适合所有机场，你可以自己修改代码，比较容易
