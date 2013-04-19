unionpay
========

UNION PAY
[UNION PAY]
https://online.unionpay.com/
requests:
https://online.unionpay.com/mer/pages/merser/helper.jsp?pid=1&cid=1
information for technique:
https://online.unionpay.com/mer/pages/merser/helper.jsp?pid=1&cid=1
2.Documents:

《中国银联在线支付业务商户接入技术改造指南》
《互联网商户接入接口规范》
《中国银联在线支付业务商户接入技术改造指南》
《16-02M：互联网商户接入接口规范》第七章。

3、商户进行接口开发和改造的注意事项
（1）消费、交易信息查询两个交易功能是每个接入商户必须开发的基本功能。在交易过程中，由于银行返回交易应答可能发生延迟，商户可以通过交易查询功能多次查询未及时收到的交易结果。
（2）商户可根据退款的频率，选择消费撤消和退货交易通过后台交易发起或通过银联在线支付商户服务网站发起。如选择通过后台交易发起，则须按照技术改造指南完成相应的开发和测试。
（3）担保支付相关交易。如商户选择支持担保支付功能，担保消费和交易信息查询两个交易功能必须开发，担保消费撤销、担保消费完成、担保消费完成撤销可选择开发。
4、商户如何对接口开发情况进行验证？
为保障技术改造的成功，商户可利用中国银联提供的联调测试环境、测试卡等进行联调测试。联调测试环境、测试卡信息，以及离线联调测试的注意事项等详见《中国银联在线支付业务商户接入技术改造指南》。
5、银联在线支付的开发过程具有哪些技术环境？地址是什么？
中国银联为商户的接口开发提供三个环境：开发联调环境、PM测试环境（准生产环境）、生产环境，请求地址如下：
（1）开发联调环境地址：
    前台支付：http://58.246.226.99/UpopWeb/api/Pay.action
    后台交易：http://58.246.226.99/UpopWeb/api/BSPay.action
    查询请求：http://58.246.226.99/UpopWeb/api/Query.action
（2）PM测试环境（准生产环境）地址：
    前台支付：https://www.epay.lxdns.com/UpopWeb/api/Pay.action
    后台交易：https://www.epay.lxdns.com/UpopWeb/api/BSPay.action
    查询请求：https://www.epay.lxdns.com/UpopWeb/api/Query.action
（3）生产环境地址：
    前台支付：https://unionpaysecure.com/api/Pay.action
    后台交易：https://besvr.unionpaysecure.com/api/BSPay.action
    查询请求：https://query.unionpaysecure.com/api/Query.action
6、银联在线支付的三个技术环境提供测试卡吗？分别是什么？
中国银联为商户的接口开发提供三个环境：开发联调环境、PM测试环境（准生产环境）、生产环境，请求地址如下：
（1）开发联调环境测试卡：
    借记卡卡号：6212341111111111111，密码：111111，手机号：13888888888，短信验证码测试环境不校验，可输入任意6位数字。
    贷记卡卡号：6212341111111111111，年份：12，月份：12，CVN2：123，短信验证码测试环境不校验，可任意输入6位数字。
（2）PM测试环境（准生产环境）测试卡：
    借记卡卡号：6222023602899998371，密码：888888，手机号：13924254496 ，短信验证码：111111。
    贷记卡卡号：5309900599078555，CVN2：214，有效期：02月15年，手机号：13602958691，短信验证码：111111。
8. About front & Back:
1.Front: 前台通知在持卡人支付成功后点击“返回商户”时调用.(can test local)
2.Back: 后台通知在交易成功后由服务器自动回调，商户需以后台通知为准(Test: need a outside domain)
后台通知在开发联调环境中的通知地址使用外网可以访问的ip地址+端口80，不支持域名；
PM测试环境（准生产环境）与生产环境的后台通知地址只需外网能够访问即可。前台通知无上述要求。
**********************************************
The "Back" is same as notify_url in taobao!
**********************************************
10、调用数次查询接口，交易一直为处理中状态，该如何处理？
网银支付模式：跳转到银行网银页面后，若订单状态为正在处理中，查询结果为正在处理中。在银行响应后，UPOP会根据银行的应答，修改订单状态为成功或失败。如果持卡人在网银页面直接关闭浏览器，则查询结果一直显示为处理中。若超过1小时，查询结果仍为处理中状态，一般可认为持卡人关闭了浏览器，当作失败交易处理。若产生单边账，可在第二天通过对账退款给持卡人。
无卡支付模式：UPOP可控制交易超时时间，一般在持卡人完成支付2分钟后交易会有明确的状态，查询结果为成功或失败。失败的交易允许持卡人重新进行支付，即如果持卡人在12点钟支付失败，12点05分再次支付成功，则商户在12点03分查询到的结果为交易失败，12点06分查询到的结果为成功。
That means: need to recheck!
11、防钓鱼接口有哪些特殊的技术要求？
（1）商户需在入网申请表中填写真实交易域名，如有多个，需全部填写。
（2）交易报文中customIp字段需上送客户真实IP，详情请参考接口规范文档中对customIp域的说明。
（3）UPOP会根据当前服务器时间减掉商户上送的订单时间来计算订单超时时间，所得时间间隔如果大于上送的transTimeout字段，则认为该笔订单超时。商户需对transTimeout字段上送一个合理值，建议300000毫秒。
12、customIp字段上送要求
目前UPOP只支持上送单一IP（IPV4或IPV6），不支持上送多个IP，且不支持上送null或由空格、逗号隔开的多个值。
若客户使用代理服务器上网，商户可能获取到多个客户端IP。若为非匿名代理，请尽量上送客户端真实IP；若为匿名代理，则上送代理服务器IP。
13、得到非00响应码，商户该如何处理？
相关处理可参考以下伪代码：
      if(respCode==00){
          if(queryResult==0){
              交易成功
          }
          else if(queryResult==2){
              交易处理中，继续调查询接口
          }
      }
      else{
          交易失败
      }
14、UPOP返回的系统保留域cupReserved字段有什么作用？
此保留域供银联在应答报文中向商户返回系统特殊信息，具体描述参见《互联网商户接入接口规范》。
UPOP会根据需要添加该字段内容项，格式保持为：
cupReserved=或cupReserved={key1=value1&key2=value2&…}，如cupReserved={orderAmount=100&orderCurrency=156&payMode=LitePay }，
商户需要支持兼容该字段内容不定的情况。

reference:

[UNION PAY]