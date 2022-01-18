### [返回上层](../POC.md)


# 目录

* [PoC 命名规范](#PoC命名规范)
* [PoC 漏洞类型](#PoC漏洞类型说明)

# PoC命名规范

PoC命名主要由三部分组成，漏洞应用名称-漏洞类型-（当前日期+同类型报告序数）（漏洞类型请参考* [PoC 漏洞类型](#PoC漏洞类型说明)）；

文件命名应遵守命名规范，三部分首字母大写，禁止特殊字符输入；三部分内容之间使用‘-’进行连接，三部分中多字段由“_”进行连接，且首字母需大写。PoC命名举例如下：

<pre>Weblogic-RCE-CVE-2011-1122.py</pre> 
Weblogic组件（Weblogic），远程命令执行类型（RCE），漏洞编号信息

<pre>Weblogic-Info_Leak-210113.py</pre>

Weblogic组件（Weblogic），信息泄露漏洞（Info_Leak），无CVE编号信息，则编写漏洞爆发时间

### 注意
（当前日期+同类型报告序数）解释如下
如两个Weblogic漏洞都是RCE，无CVE编号，且都是同一天编写
<pre>Weblogic-RCE-210113.py</pre> 
<pre>Weblogic-RCE-2101131.py</pre> 

# PoC漏洞类型说明

具体漏洞类型主要如下：

<table border=1 style="text-align:center">
    <tr style="background-color:rgb(164, 164, 208)"><td>中文名称</td><td>缩写</td></tr>
    <tr><td> 跨站脚本攻击 </td><td> XSS</td></tr>
    <tr><td> 跨站请求伪造 </td><td> CSRF</td></tr>
    <tr><td> SQL注入 </td><td> SQL_Inj</td></tr>
    <tr><td> 命令执行 </td><td> Cmd_Exec</td></tr>
    <tr><td> 代码执行 </td><td> Code_Exec</td></tr>
    <tr><td> 远程文件包含 </td><td> RFI</td></tr>
    <tr><td> 本地文件包含 </td><td> LFI</td></tr>
    <tr><td> 溢出漏洞 </td><td> Overflow</td></tr>
    <tr><td> 权限提升 </td><td> Privilege</td></tr>
    <tr><td> 暴力破解 </td><td> Brute_Force</td></tr>
    <tr><td> 弱密码 </td><td> Weak_Pass</td></tr>
    <tr><td> 信息泄漏 </td><td> Info_Leak</td></tr>
    <tr><td> 认证绕过 </td><td> Auth_Bypass</td></tr>
    <tr><td> 越权漏洞 </td><td> Over_Permission</td></tr>
    <tr><td> 认证绕过 </td><td> Auth_Bypass</td></tr>
    <tr><td> 任意文件下载 </td><td> File_Download</td></tr>
    <tr><td> 任意文件删除 </td><td> File_Deletion</td></tr>
    <tr><td> 任意文件读取 </td><td> File_Read</td></tr>
    <tr><td> 文件上传 </td><td> File_Upload</td></tr>
    <tr><td> 密码重置 </td><td> Rest_pwd</td></tr>
    <tr><td> 后门 </td><td> Backdoor</td></tr>
    <tr><td> 目录穿越 </td><td> Dir_Traversal</td></tr>
    <tr><td> 配置错误 </td><td> Setup_Error</td></tr>
    <tr><td> 会话漏洞 </td><td> Session_Vuln</td></tr>
    <tr><td> 重定向漏洞 </td><td> Url_Redirect</td></tr>
  	<tr><td> 拒绝服务 </td><td> Dos</td></tr>
    <tr><td> 其它绕过 </td><td> Other_Bypass</td></tr>
    <tr><td> 其它注入 </td><td> Other_Inj</td></tr>
  	<tr><td> 其它逻辑漏洞 </td><td> Logical_vuln</td></tr>
    <tr><td> 其它漏洞 </td><td> Logical_vuln</td></tr>
  	<tr><td> 通用型漏洞 </td><td> Generic_vuln</td></tr>
    <tr><td> 事件型漏洞 </td><td> Event_vuln</td></tr>
</table>

如提交的漏洞不在以下提供的列表内，可自定义漏洞类型，漏洞类型命名规范需满足命名规范，如首字母进行大写，如漏洞类型存在多个字段则使用“_”进行连接。

### [返回上层](../POC.md)

