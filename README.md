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

# DNS 服务器配置(可选；若不配置，程序内置的 DNS 服务会被关闭)
dns:
  enable: true
  listen: 127.0.0.1:53
  ipv6: true # 当此选项为 false 时, AAAA 请求将返回空

  # 以下填写的 DNS 服务器将会被用来解析 DNS 服务的域名
  # 仅填写 DNS 服务器的 IP 地址
  default-nameserver:
    - 223.5.5.5
    - 114.114.114.114
  enhanced-mode: fake-ip # 或 redir-host
  fake-ip-range: 198.18.0.1/16 # Fake IP 地址池 (CIDR 形式)
  # use-hosts: true # 查询 hosts 并返回 IP 记录

  # 在以下列表的域名将不会被解析为 fake ip，这些域名相关的解析请求将会返回它们真实的 IP 地址
  fake-ip-filter:
    # 以下域名列表参考自 vernesong/OpenClash 项目，并由 Hackl0us 整理补充
    # === LAN ===
    - '*.lan'
    # === Linksys Wireless Router ===
    - '*.linksys.com'
    - '*.linksyssmartwifi.com'
    # === Apple Software Update Service ===
    - 'swscan.apple.com'
    - 'mesu.apple.com'
    # === Windows 10 Connnect Detection ===
    - '*.msftconnecttest.com'
    - '*.msftncsi.com'
    # === NTP Service ===
    - 'time.*.com'
    - 'time.*.gov'
    - 'time.*.edu.cn'
    - 'time.*.apple.com'

    - 'time1.*.com'
    - 'time2.*.com'
    - 'time3.*.com'
    - 'time4.*.com'
    - 'time5.*.com'
    - 'time6.*.com'
    - 'time7.*.com'

    - 'ntp.*.com'
    - 'ntp.*.com'
    - 'ntp1.*.com'
    - 'ntp2.*.com'
    - 'ntp3.*.com'
    - 'ntp4.*.com'
    - 'ntp5.*.com'
    - 'ntp6.*.com'
    - 'ntp7.*.com'

    - '*.time.edu.cn'
    - '*.ntp.org.cn'
    - '+.pool.ntp.org'

    - 'time1.cloud.tencent.com'
    # === Music Service ===
    ## NetEase
    - '+.music.163.com'
    - '*.126.net'
    ## Baidu
    - 'musicapi.taihe.com'
    - 'music.taihe.com'
    ## Kugou
    - 'songsearch.kugou.com'
    - 'trackercdn.kugou.com'
    ## Kuwo
    - '*.kuwo.cn'
    ## JOOX
    - 'api-jooxtt.sanook.com'
    - 'api.joox.com'
    - 'joox.com'
    ## QQ
    - '+.y.qq.com'
    - '+.music.tc.qq.com'
    - 'aqqmusic.tc.qq.com'
    - '+.stream.qqmusic.qq.com'
    ## Xiami
    - '*.xiami.com'
    ## Migu
    - '+.music.migu.cn'
    # === Game Service ===
    ## Nintendo Switch
    - '+.srv.nintendo.net'
    ## Sony PlayStation
    - '+.stun.playstation.net'
    ## Microsoft Xbox
    - 'xbox.*.microsoft.com'
    - '+.xboxlive.com'
    # === Other ===
    ## QQ Quick Login
    - 'localhost.ptlogin2.qq.com'
    ## Golang
    - 'proxy.golang.org'
    ## STUN Server
    - 'stun.*.*'
    - 'stun.*.*.*'

  # 支持 UDP / TCP / DoT / DoH 协议的 DNS 服务，可以指明具体的连接端口号。
  # 所有 DNS 请求将会直接发送到服务器，不经过任何代理。
  # Clash 会使用最先获得的解析记录回复 DNS 请求
  nameserver:
    - 'https://doh.pub/dns-query'
    - 'https://dns.alidns.com/dns-query'
     
  # 当 fallback 参数被配置时, DNS 请求将同时发送至上方 nameserver 列表和下方 fallback 列表中配置的所有 DNS 服务器.
  # 当解析得到的 IP 地址的地理位置不是 CN 时，clash 将会选用 fallback 中 DNS 服务器的解析结果。
  fallback:
    - 'tls://dns.google:853'
    - 'https://1.1.1.1/dns-query'

  # 如果使用 nameserver 列表中的服务器解析的 IP 地址在下方列表中的子网中，则它们被认为是无效的，
  # Clash 会选用 fallback 列表中配置 DNS 服务器解析得到的结果。
  #
  # 当 fallback-filter.geoip 为 true 且 IP 地址的地理位置为 CN 时，
  # Clash 会选用 nameserver 列表中配置 DNS 服务器解析得到的结果。
  #
  # 当 fallback-filter.geoip 为 false, 如果解析结果不在 fallback-filter.ipcidr 范围内，
  # Clash 总会选用 nameserver 列表中配置 DNS 服务器解析得到的结果。
  #
  # 采取以上逻辑进行域名解析是为了对抗 DNS 投毒攻击。
  fallback-filter:
    geoip: false
    ipcidr:
      - 240.0.0.0/4
      - 0.0.0.0/32


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
