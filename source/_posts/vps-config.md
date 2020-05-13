---
title: VPS相关配置，Trojan及V2RAY脚本
toc: true
category:
  - 科学上网
tags:
  - VPS
  - Trojan
  - V2Ray
abbrlink: 7a1be4a0
date: 2020-05-08 13:26:19
---

 目前在用的科学上网工具有两款：**Trojan**和**V2Ray**。V2Ray的使用方式有很多，这里不作详细记录。Trojan的优势在于配置相对简单、采用的科学上网的方法不易被发现、速度快。V2Ray的优势在于功能齐全，搭配其他工具可以实现从简单到复杂的各种科学上网方式，缺点是某些方式速度较慢。综合两者的优缺点，我一般将两者搭配使用，Trojan负责科学上网，V2Ray负责路由功能。

 ### 安装Trojan和V2Ray
是否在Debian/Ubuntu官方仓库中
> - `Trojan`: ✔ (ubuntu从19.04开始)
> - `V2Ray`:  ✘

`V2Ray`的官方安装脚本在国内下载速度很慢，经常失败。结合上面信息，应选择优先安装`Trojan`，然后在终端下开启科学上网模式，再安装`V2Ray`。***安装流程全自动，一切都很Nice。***

```bash
function trojan_environment_varibles() {
    TROJAN_PREFIX=/directory/to/trojan/real/config.json
    TROJAN_CERT=$TROJAN_PREFIX/cert.pem
    TROJAN_CERT_PATH_IN_CONFIG=$TROJAN_CERT
    TROJAN_CONFIG=$TROJAN_PREFIX/config.json
    # trojan模板配置文件
    TROJAN_CONFIG_EXAMPLE=$TROJAN_PREFIX/config_example.json
    
    TROJAN_SERVER=$SERVER_NAME
}

function trojan_install_for_old_distributor() {
    # 当官方仓库中不含trojan时安装流程
    # 证书 位于 /usr/local/etc/ssl/cert.pem
    # 配置文件 位于 /usr/local/etc/trojan/config.json
    # 新建trojan用户，由trojan用户开启服务
}

function trojan_install() {
    # 设置trojan相关环境变量
    trojan_environment_varibles
    # 若trojan配置文件不存在，则从模板配置文件复制一份
    [ ! -f $TROJAN_CONFIG ] && cp $TROJAN_CONFIG_EXAMPLE $TROJAN_CONFIG
    if ! command -v trojan > /dev/null 2>&1; then
        TROJAN_CONFIG=/path/to/模板配置文件
        TROJAN_CERT=/path/to/本地证书
        if 满足条件：Ubuntu及版本在19.04及以上; then
            sudo apt install trojan
            sudo ln -sf $TROJAN_CONFIG /etc/trojan/config.json
        else
            trojan_install_for_old_distributor
            TROJAN_CERT_PATH_IN_CONFIG=/usr/local/etc/ssl/cert.pem
        fi
    fi
    # 设置配置文件中的证书路径
    if $TROJAN_CONFIG中不含$TROJAN_CERT_PATH_IN_CONFIG; then
        sed -i "s|\"cert.*|\"cert\": \"$TROJAN_CERT_PATH_IN_CONFIG\",|" $TROJAN_CONFIG
    fi
    trojan_first_start
}
```


### 配置文件
这里只讨论Trojan在本地的配置以及相关脚本。Trojan的客户端配置中有三项在切换VPS时需要修改，分别是

|配置选项        | VPS        |
|:--------------:|:-----------:|
| `remote_addr` | `IP/域名`  |
| `sni`         | `域名`     |
| `cert`        | `本地证书路径` |

因为使用了`CloudFlare`家的泛域名证书，因此在多个VPS上可以使用同一套证书，意味着即使在配置文件中**本地证书路径**使用了软链接地址，也无需对其指向的证书文件做任何修改。因此在切换VPS时，真正需要修改的只有两个地方：`remote_addr`和`sni`。

`remote_addr`选择的规则为

|               | DNS | Proxy |
|:--------------:|:---------------:|:------:|
| `remote_addr` | `IP`或者`域名` | `IP`  |

原因在于`remote_addr`必须解析到VPS自己的`IP`。

### 切换VPS并重启服务的脚本
根据上面的分析，采用模板配置文件的方式，选取特定的VPS后，用`sed`直接修改配置文件中上述两个选项的值，再用`service`命令重启服务。

#### VPS信息
VPS的相关信息，都存放在文件`server_info.ini`中，如下所示：
```ini
[cloudcone]
ip = xxx.xxx.xxx.xxx
domain = www.example.com
dns_proxy = true

[racknerd]
ip = xxx.xxx.xxx.xxx
domain = www.example2.com
dns_proxy = false
```
借助`awk`定义两个函数用于读取`ini`文件中的服务器信息。
```bash
SERVER_INFO_FILE=/path/to/server_info.ini
# 收集VPS入口信息
function read_section_ini() {
    INI_FILE=$1
    awk -F '=' '{
        if ($1 ~ /\[/) {
            gsub(/[\[\]]/, "", $1)
            print $1
        }
    }' $INI_FILE
}    

# 获取特定VPS的信息
function read_ini() {
    INI_FILE=$1
    SECTION=${2:-cloudcone}
    KEY=${3:-ip}
    awk -F '=' '{
    if ($1 ~ /\[/)
        if ($1 ~ /\['$SECTION'/) {
            a = 1
        } else {
            a = 0
        }
    else
        if (a == 1 && $1 ~ /'$KEY'/) {
            gsub(/[[:blank:]]/, "", $2)
            print $2
        }
    }' $INI_FILE
}

VPS_SERVERS=($(read_section_ini $SERVER_INFO_FILE))
```

#### 切换VPS脚本
接下来就涉及到`Trojan`和`V2Ray`相关的脚本了，同样定义两个函数

```bash
# 可用的VPS都存放在数组变量 VPS_SERVERS 中，传入的参数为数组下标，
# 用于选取特定 VPS
SERVER_ID=${1:-0}
SERVER_NAME=${VPS_SERVERS[$SERVER_ID]}

function choose_server_for_trojan() {
    TROJAN_SERVER=$SERVER_NAME
    TROJAN_CONFIG=/path/to/trojan的模板配置文件
    # 获取选定VPS的具体信息
    dns_proxy=$(read_ini $SERVER_INFO_FILE $TROJAN_SERVER dns_proxy)
    ip=$(read_ini $SERVER_INFO_FILE $TROJAN_SERVER ip)
    sni=$(read_ini $SERVER_INFO_FILE $TROJAN_SERVER sni)
    if $dns_proxy; then
        remote_addr=$ip
    else
        remote_addr=$sni
    fi
    # 修改模板配置文件
    sed -i "s|\"remote_addr.*|\"remote_addr\": \"$remote_addr\",|" $TROJAN_CONFIG
    sed -i "s|\"sni.*|\"sni\": \"$sni\",|" $TROJAN_CONFIG
    # 重启Trojan服务
    sudo service trojan restart
    sudo service trojan status
}

function choose_server_for_v2ray() {
    # V2Ray相关配置修改
}

function show_servers_info() {
# 输出VPS相关信息
    echo "可用的服务器有："
    for i in "${!VPS_SERVERS[@]}"; do
    echo "  $i ): ${VPS_SERVERS[i]}"
    done
    echo ""
}

# 核心逻辑部分
if [ $SERVER_ID -ge 0 ] && [ $SERVER_ID -lt ${#VPS_SERVERS[*]} ]; then
    choose_server_for_trojan
    choose_server_for_v2ray
else
    # 显示VPS相关信息
    show_servers_info
fi
```

