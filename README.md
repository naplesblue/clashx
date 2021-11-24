# clashx
clash pro rules

采用白名单模式建立分流策略。

现有rules：

gfwlist：从github gfwlist转码得来，处理成clash可辨认的格式，以域名方式分流到流量大的服务器。
accelerated chinese website：加入国内知名域名，分流到直接访问。
cnipcidr：中国所属IP，分流到直接访问。
Netflix：分流Netflix到支持Netflix的服务器。
