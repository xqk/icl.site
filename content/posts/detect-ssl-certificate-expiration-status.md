---
title: "检测SSL证书到期状态"
description: ""
summary: "检测SSL证书到期状态shell脚本，然后就可以通过心跳检测和企业微信机器人组合来实现自动告警"
date: 2022-05-02T01:12:46+08:00
slug: ""
tags: ["ssl"]
draft: false
pin: true
---

直接上干货！

### 配置文件

`domain_ssl.info`
```text
www.icl.site:443
```

### shell文件：

`check_domain_time.sh`
```shell
#!/bin/bash
################ Version Info ##################
# Create Date: 2022-04-28
# Author:      xqk
# Mail:        xiaqiankun@outlook.com
# Version:     1.0
# Attention:   通过域名获取证书的过期时间
################################################

# 加载环境变量
. /etc/profile
. ~/.bash_profile
. /etc/bashrc

# 脚本所在目录即脚本名称
script_dir=$( cd "$( dirname "$0"  )" && pwd )
script_name=$(basename ${0})

# 当前时间
CURRENT_TIME=$(date "+%Y-%m-%d %H:%M:%S")

# 声明报警数组
declare -A warningDomains=()

function main(){
    warningDomainsStr=""
    readFile="${script_dir}/domain_ssl.info"

    i=0
    while read line
    do
        if [[ $line =~ "#" ]];then
            continue
        fi

        get_domain=$(echo "${line}" | awk -F ':' '{print $1}')
        get_port=$(echo "${line}" | awk -F ':' '{print $2}')

        END_TIME=$(echo | openssl s_client -servername ${get_domain}  -connect ${get_domain}:${get_port} 2>/dev/null | openssl x509 -noout -dates |grep 'After'| awk -F '=' '{print $2}'| awk -F ' +' '{print $1,$2,$4 }' )
        END_TIME1=$(date +%s -d "$END_TIME") #将日期转化为时间戳
        NOW_TIME=$(date -d "$CURRENT_TIME" +%s) #将目前的日期也转化为时间戳

        RST=$(($(($END_TIME1-$NOW_TIME))/(60*60*24))) # 到期时间减去目前时间再转化为天数
        END_TIME_S=$(date -d @$END_TIME1 "+%Y-%m-%d")

        # echo "${get_domain} 还剩${RST}天 ${END_TIME_S}"

        # 找出剩余不到20天的域名证书
        if [[ $RST -lt 20 ]]; then
            warningDomains[$i]="${get_domain}-还剩${RST}天-${END_TIME_S} "
            let i++
        fi

    done < $readFile

    num=${#warningDomains[@]}
    if [[ $num -gt 0 ]]; then
        # 这些就是需要告警的域名
        echo ${warningDomains[@]}
        
        # exit 1 
    fi
}

# 执行函数
main

```

