长话短说：现在Docker改为基于YY.MM的版本（像Ubuntu），用户可以选择Stable（发布较慢）或者Edge（发布较快）版本。
Docker Engine改为Docker CE（社区版） 
它包含了CLI客户端、后台进程/服务以及API。用户像以前以同样的方式获取。
Docker Data Center改为Docker EE（企业版） 
在Docker三个定价层增加了额外的支付产品和支持
这些修改并不影响Docker Compose以及Docker Machine
Docker版本现在基于YY.MM 
使用基于月份的发行版本，17.03 的第一版就指向17.03.0，如果有bug/安全修复需要发布，那么将会指向17.03.1等等。
"Edge"与"Stable"两个版本发行
Edge版本每月发布，提供一个月支持。
Stable版本每季度发布，提供4个月支持。
你可以通过Docker EE订阅 延长Stable版本支持以及补丁修复。
