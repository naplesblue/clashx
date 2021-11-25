# Clashx Pro规则

自用Clash Pro分流规则

Clash Pro下载：

[Clash Pro下载地址](https://install.appcenter.ms/users/clashx/apps/clashx-pro/distribution_groups/public)

### 采用白名单模式建立分流策略

### 现有rules：

dns部分直接搬运了[ClashX使用入门](https://bfchengnuo.com/2021/06/23/ClashX%E4%BD%BF%E7%94%A8%E5%85%A5%E9%97%A8/)中的配置，仅修改了fall-back中name server的地址。这篇文章对clash pro的规则解释相当详细，特别是dns部分。

是不是使用dns fall-back，要看实际网络环境。fall-back主要解决dns污染，使用国外dns加密服务来解析正确的域名结果。但这样一来，又会将使用cdn服务的服务器挡在外面，使得原本按照请求方访问速度最快分配的ip地址反而解析到国外去。目前的使用中，更多需要考虑跨国公司在国内设立cdn服务器之后的访问效果。比如Apple，icloud服务，这些会影响到软件更新服务。影响不仅是速度，甚至无法找到更新。所以，对域名分流的规则要谨慎测试。

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

  applications:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/naplesblue/clashx@master/yaml/applications.yaml"
    path: ./ruleset/applications.yaml
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
    url: "https://cdn.jsdelivr.net/gh/naplesblue/clashx@master/yaml/netflix.yaml"
    path: ./ruleset/netflix.yaml
    interval: 86400

#策略    
rules:
  - RULE-SET,lancidr,DIRECT
  - RULE-SET,applications,DIRECT
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
