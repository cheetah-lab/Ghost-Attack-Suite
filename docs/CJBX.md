  ### [返回首页](../README.md)
  # GAS插件框架介绍

**注: 目前插件加载支持HTML编写的插件** 

##  GAS插件的特点

插件的特点:

- 跨平台 可移植性
- 节省系统的资源成本, 可以达到随时用随时下载
- GAS插件框架只作为容器载体, 使用者编写的插件可以快速的应用及加载,不依赖于框架的API, 耦合性低

- 使用json作为配置文件, 配置简单

## 插件目录结构介绍


tree 
.

├── CHANGELOG.md   // 记录插件的变更日志

├── README.md  // 插件的使用说明

├── package.json  // 插件的配置文件

└── src  // 编写好的HTML文件的存放位置

  ├── ...



## package.json 配置说明

例如:

```json
{
  "plugin" :{
    "name" : "Windows提权辅助",
    "author" : "猎豹",
    "desc": "Windows提权辅助工具",
    "version" : "0.1.0",
    "icons" : "src/tiquan.png",
    "main_program" : "src/index.html",
    "scripts": {},
		"devDependencies": {},
		"dependencies": {}
	}
}
```

- 关键字说明

  - "plugin" 定义此为插件配置

  - "name" 定义插件名称

  - "author" 插件作者名称

  - "desc" 插件简短描述

  - "version" 插件版本

  - "icons" 插件图标 **以package.json存放目录为根目录**

  - "main_program" 插件入口html文件

  - "scripts"  `存放插件依赖的外界服务的启动命令 例如 ./nmap...` 目前功能暂未开放

## 插件提交
可联系 tian_jingxin@venustech.com.cn 提交插件，优秀插件会推送到插件市场

  ### [返回首页](../README.md)
    












