## ArgoSB一键无交互小钢炮脚本💣：极简 + 轻量 + 快速

---------------------------------------

<img width="636" height="238" alt="0cbc3f82134b4fc99afd6cee37e98be" src="https://github.com/user-attachments/assets/a76ca418-badb-4e9a-a771-6682ec713e06" />

### 【ArgoSB当前版本：V25.8.18】

---------------------------------------

#### 1、基于Sing-box + Xray + Cloudflared-Argo 三内核自动分配

#### 2、支持Docker Image镜像部署，公开镜像库：```ygkkk/argosb```

#### 3、支持Linux类主流VPS系统，SSH脚本支持非root，几乎无需依赖，无脑一次回车搞定

#### 4、支持NIX容器系统，特别推荐IDX-Google、Clawcloud爪云、CloudCat的服务器

#### 5、可选内核Wireguard-WARP全局出站模式，更换落地IP为IPV4+IPV6的WARP的IP，解锁流媒体

#### 6、可选IPV4或者IPV6的配置输出；可选四档IP优先级的切换

#### 7、所有代理协议都无需域名（除了argo固定隧道），支持单个或多个代理协议任意组合并快速重置更换
【已支持：AnyTLS、Vless-xhttp-reality、Vless-reality-vision、Shadowsocks-2022、Vmess-ws、Hysteria2、Tuic、Argo临时/固定隧道】



----------------------------------------------------------

## 一、自定义变量参数说明：

| 变量意义 | 变量名称| 在变量值""之间填写| 删除变量 | 在变量值""之间留空 | 变量要求及说明 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1、启用vless-reality-vision | vlpt | 端口指定 | 关闭vless-reality-vision | 端口随机 | 必选之一 【xray内核：TCP】 |
| 2、启用vless-xhttp-reality | xhpt | 端口指定 | 关闭vless-xhttp-reality | 端口随机 | 必选之一 【xray内核：TCP】 |
| 3、启用Shadowsocks-2022 | sspt | 端口指定 | 关闭Shadowsocks-2022 | 端口随机 | 必选之一 【xray内核：TCP】 |
| 4、启用anytls | anpt | 端口指定 | 关闭anytls | 端口随机 | 必选之一 【singbox内核：TCP】 |
| 5、启用vmess-ws | vmpt | 端口指定 | 关闭vmess-ws | 端口随机 | 必选之一 【xray/singbox内核：TCP】 |
| 6、启用hysteria2 | hypt | 端口指定 | 关闭hy2 | 端口随机 | 必选之一 【singbox内核：UDP】 |
| 7、启用tuic | tupt | 端口指定 | 关闭tuic | 端口随机 | 必选之一 【singbox内核：UDP】 |
| 8、warp开关 | warp | 填写s或者x | 关闭warp | 所有内核协议启用warp | 可选，s表示singbox所有协议启用warp，x表示xray所有协议启用warp |
| 9、argo开关 | argo | 填写y | 关闭argo隧道 | 关闭argo隧道 | 可选，填写y时，vmess变量vmpt必须启用，且固定隧道必须填写vmpt端口 |
| 10、argo固定隧道域名 | agn | 托管在CF上的域名 | 使用临时隧道 | 使用临时隧道 | 可选，argo填写y才可激活固定隧道|
| 11、argo固定隧道token | agk | CF获取的ey开头的token | 使用临时隧道 | 使用临时隧道 | 可选，argo填写y才可激活固定隧道 |
| 12、uuid密码 | uuid | 符合uuid规定格式 | 随机生成 | 随机生成 | 可选 |
| 13、reality域名（仅支持reality类协议） | reym | 符合reality域名规定 | yahoo | yahoo | 可选，使用CF类域名时，可用作ProxyIP/客户端地址反代IP（建议高位端口或纯IPV6下使用，以防被扫泄露）|
| 14、vmess客户端host地址 | cdnym | IP解析的CF域名 | vmess为直连 | vmess为直连 | 可选，使用80系CDN或者回源CDN时可设置，否则客户端host地址需手动更改为IP解析的CF的域名|
| 15、切换ipv4或ipv6配置 | ippz | 填写4或者6 | 自动识别IP配置 | 自动识别IP配置 | 可选，4表示IPV4配置输出，6表示IPV6配置输出 |
| 16、切换ipv4或ipv6出站优先 | ipyx | 填写4或6或46或64 | 系统默认优先 | 系统默认优先 | 可选，46表示IPV4出站优先，64表示IPV6出站优先，4表示仅IPV4出站，6表示仅IPV6出站 |
| 17、添加所有节点名称前缀 | name | 任意字符 | 默认协议名前缀 | 默认协议名前缀 | 可选 |
| 18、【仅容器类docker】监听端口，网页查询 | PORT | 端口指定 | 3000 | 3000 | 可选 |
| 19、【仅容器类docker】启用vless-ws-tls | DOMAIN | 服务器域名 | 关闭vless-ws-tls | 关闭vless-ws-tls | 可选，vless-ws-tls可独立存在，uuid变量必须启用 |

<img width="926" height="602" alt="fbb5de8b838df475c10561d47ad9202b" src="https://github.com/user-attachments/assets/33935ca5-6724-492e-9fe7-8d4237acb2b4" />

----------------------------------------------------------

## 二、SSH一键变量脚本模版说明：

### 脚本以 ```变量名称="变量值"的单个或多个组合 + 主脚本``` 的形式运行

主脚本：```bash <(curl -Ls https://raw.githubusercontent.com/yonggekkk/argosb/main/argosb.sh)```

必选其一的协议端口变量：```vmpt=""```、```vmpt="" argo="y"```、```vlpt=""```、```xhpt=""```、```anpt=""```、```hypt=""```、```tupt=""```、```sspt=""```

可选的功能类变量：```warp=""```、```uuid=""```、```reym=""```、```cdnym=""```、```argo=""```、```agn=""```、```agk=""```、```ippz=""```、```ipyx=""```、```name=""```

请参考```一、自定义变量参数说明```中变量的作用说明，变量值填写在```" "```之间，变量之间空一格，不用的变量可以删除

* ### 模版1：多个任意协议组合运行
```
sspt="" vlpt="" vmpt="" hypt="" tupt="" xhpt="" anpt="" bash <(curl -Ls https://raw.githubusercontent.com/yonggekkk/argosb/main/argosb.sh)
```

* ### 模版2：主流TCP或UDP单个协议运行

Vless-Reality-Vision协议节点
```
vlpt="" bash <(curl -Ls https://raw.githubusercontent.com/yonggekkk/argosb/main/argosb.sh)
```

Vless-Xhttp-Reality协议节点
```
xhpt="" bash <(curl -Ls https://raw.githubusercontent.com/yonggekkk/argosb/main/argosb.sh)
```

Shadowsocks-2022协议节点
```
sspt="" bash <(curl -Ls https://raw.githubusercontent.com/yonggekkk/argosb/main/argosb.sh)
```

AnyTLS协议节点
```
anpt="" bash <(curl -Ls https://raw.githubusercontent.com/yonggekkk/argosb/main/argosb.sh)
```

Vmess-ws协议节点
```
vmpt="" bash <(curl -Ls https://raw.githubusercontent.com/yonggekkk/argosb/main/argosb.sh)
```

Hysteria2协议节点
```
hypt="" bash <(curl -Ls https://raw.githubusercontent.com/yonggekkk/argosb/main/argosb.sh)
```

Tuic协议节点
```
tupt="" bash <(curl -Ls https://raw.githubusercontent.com/yonggekkk/argosb/main/argosb.sh)
```

* ### 模版3：仅Argo临时/固定隧道运行

类似无公网的IDX-Google-VPS容器推荐使用此脚本，快速一键内网穿透获取节点

仅argo临时隧道节点
```
vmpt="" argo="y" bash <(curl -Ls https://raw.githubusercontent.com/yonggekkk/argosb/main/argosb.sh)
```

仅argo固定隧道节点，必须填写端口(vmpt)、域名(agn)、token(agk)
```
vmpt="端口" argo="y" agn="解析的CF域名" agk="CF获取的token" bash <(curl -Ls https://raw.githubusercontent.com/yonggekkk/argosb/main/argosb.sh)
```

---------------------------------------------------------

## 三、多功能SSH快捷方式命令组

#### 说明：首次安装成功后需重连SSH，```agsb 命令```的快捷方式才可生效；如未生效，请使用```主脚本 命令```的快捷方式

1、查看Argo的固定域名、固定隧道的token、临时域名、当前已安装的节点信息命令：```agsb list``` 或者 ```主脚本 list```

2、更换代理协议变量组命令：```自定义各种协议变量组 agsb rep``` 或者 ```自定义各种协议变量组 主脚本 rep```

3、更新脚本命令(建议卸载重装)：```原已安装的自定义各种协议变量组 主脚本 rep``` 

4、重启脚本命令：```agsb res``` 或者 ```主脚本 res```

5、卸载脚本命令：```agsb del``` 或者 ```主脚本 del```

6、临时切换IPV4/IPV6节点配置 (双栈VPS专享)：

显示IPV4节点配置：```ippz=4 agsb list```或者```ippz=4 主脚本 list```

显示IPV6节点配置：```ippz=6 agsb list```或者```ippz=6 主脚本 list```

----------------------------------------------------------



