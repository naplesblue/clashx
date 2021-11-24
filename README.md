# clashx
clash pro rules

Clash Pro下载：

[Clash Pro下载地址](https://install.appcenter.ms/users/clashx/apps/clashx-pro/distribution_groups/public)

### 采用白名单模式建立分流策略

### 现有rules：

```yaml

# 分流交易所到不受限的地区。
# lancidr：局域网IP直连。
# gfwlist：从github gfwlist转码得来，处理成clash可辨认的格式，以域名方式分流到流量大的服务器。
# accelerated chinese website：加入国内知名域名，分流到直接访问。
# cnipcidr：中国所属IP，分流到直接访问。
# Netflix：分流Netflix到支持Netflix的服务器。
# 所有其他未匹配境外流量，一概转发到v2ray服务器。

# 策略组
rule-providers:
lancidr:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/naplesblue/clashx@master/yaml/lancidr.yaml"
    path: ./ruleset/lancidr.yaml
    interval: 86400
    
  gfwlist:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/naplesblue/clashx@master/yaml/gfwlist_domain.yaml"
    path: ./ruleset/gfwlist.yaml
    interval: 86400

  accelchina:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/naplesblue/clashx@master/yaml/accelerated-domains.china.yaml"
    path: ./ruleset/accelchina.yaml
    interval: 86400

  cnip:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/naplesblue/clashx@master/yaml/china_ip_list.yaml"
    path: ./ruleset/cnipcidr.yaml
    interval: 86400
  
  netflix:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/naplesblue/clashx/master/yaml/netflix.yaml"
    path: ./ruleset/netflix.yaml
    interval: 86400

#策略    
rules:
  - RULE-SET,lancidr,DIRECT
  - DOMAIN-SUFFIX,huobi.com,Singapore
  - DOMAIN-SUFFIX,binance.com,HK
  - DOMAIN-SUFFIX,ftx.com,Singapore
  - RULE-SET,accelchina,DIRECT
  - RULE-SET,cnip,DIRECT
  - GEOIP,CN,DIRECT
  - RULE-SET,netflix,US
  - RULE-SET,gfwlist,v2ray
  - MATCH,v2ray

```
