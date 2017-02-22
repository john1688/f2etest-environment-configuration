Windows Server 2008 安装步骤
===================

3号机：Server 2008: IE8
4号机：Server 2008: IE9
5号机：Server 2008: IE10
6号机：Server 2008: IE11

1. 安装windows Server机群
    - 3号机：Server 2008: IE8
    - 4号机：Server 2008: IE9
    - 5号机：Server 2008: IE10
    - 6号机：Server 2008: IE11
    - 注意事项:  
        (1)将来如果出现新的浏览器，可以即时增加新的服务器，用来部署新浏览器或软件。  
        (2)如果在现有运行一段时间的系统中部署新服务器，请访问以下URL来同步所有账号：[http://f2etest.xxx.com/syncAllRemoteUsers](http://f2etest.xxx.com/syncAllRemoteUsers)  
        (3)由于Server 2008可安装的最低IE版本是8，因此IE6和IE7只能安装在Server 2003系统中。
2. 修改组策略管理器
    - 运行->gpedit.msc  
    - [计算机配置/管理模板/系统]，右侧的 [显示“关闭事件跟踪程序”] 设为禁用  
    - [计算机配置/Windows 设置/安全设置/账户策略/密码策略]，禁用 [密码必须符合复杂性要求]、将 [密码最长使用期限] 设为 0  
    - 运行->gpupdate /force
3. 修改电源选项
    - [控制面板 / 电源选项 / 更改计划设置 / 关闭显示器] 设为 [从不]。
4. 添加功能
    - 管理工具 / 服务器管理器 / 添加功能/选择Telnet 客户端、桌面体验
5. 添加角色
    - 管理工具 / 服务器管理器 / 添加角色：
    - 远程桌面服务：
    - 角色服务：远程桌面会话主机、远程桌面授权
    - 身份验证方法：不需要使用网络级别身份认证
    - 授权模式：每用户
    - 用户组：添加 Authenticated Users
    Web 服务器（IIS）：
    - 角色服务：全选[常见HTTP功能]、全选[应用程序开发]、全选[安全性]、全选[管理工具]
    - 安装完后重启并完成配置。
6. 禁用IE ESC
    - 管理工具 / 服务器管理器/配置 IE ESC，禁用
7. 激活远程桌面授权
    - 管理工具 / 远程桌面服务 / 远程桌面授权管理器，在打红叉的服务器上右键/激活服务器
    ```bash
    公司信息：随便填
    许可证安装向导：
    许可证计划：企业协议
    协议号码：6565792
    产品版本：“Windows Server 2008或Windows Server 2008 R2”
    许可证类型：“TS或RDS每用户CAL”
    数量：输入你想允许的最大远程连接数量，如100
    激活后，红叉变绿。
    ```
8. 远程桌面会话主机配置
    - 管理工具 / 远程桌面服务 / 远程桌面会话主机配置：
    ```bash
    双击连接，“会话” 选择5分钟“结束已断开的会话”，“空闲会话限制”设为6小时；“客户端设置限制最大颜色深度”为24位
    编辑设置/授权/授权服务器，添加许可证服务器为自己。
    ```
9. 安装curl
    - f2etest-client/curl 下的 exe 和 dll 文件复制到 C:\Windows 文件夹下。
10. 在 Win 上部署 f2etest-client
    - 配置如下：
    ```bash
    将 app 文件夹复制到 c 盘根目录下（配置在前面的 app.json 中的），
    将需要的 bat 配置到 [管理工具/远程桌面服务/Remote App] 中：
    如果是2008操作系统，添加时请选择：允许任何命令行参数
    部署 www 下的 setuser.asp
    复制到 c:\interpub\wwwroot 下，并修改：
    apiKey 为前面 conf/site.json 中配置的 clientApiKey。
    [管理工具 / IIS 管理器]，点开 [网站 / Default Web Site]，点击 [身份验证]，右键编辑，[特定用户] IUSR 修改为 Administrator 并输入密码。
    否则 setuser.asp 无法正确执行（没有权限创建用户或修改密码）
    如果遇到iis服务器访问asp提示403错误，说明iis服务器安装asp.net服务没有安装完全。
    ```
11. 在 Win 上部署 hostsShare-client
    - 将 hostsShare-client 下的 build 文件夹命名为 hostsShare，放到服务器 f2etest-ie11（app.json中配置的） 的 C 盘根目录下。
    - 将 C:\app\hostsshare.bat 添加到 [管理工具/远程桌面服务/Remote App] 中，并允许任何命令行参数。
12. 下载 IE 并安装（或者通过操作系统自动更新来安装）
    - http://windows.microsoft.com/zh-cn/internet-explorer/ie-9-worldwide-languages --> IE9-Windows7-x64-chs.exe
    - http://windows.microsoft.com/zh-cn/internet-explorer/ie-11-worldwide-languages --> IE11-Windows6.1-x64-zh-cn.exe
    - IE 10 的官网找不到，可以从其他地方下载 --> IE10-Windows6.1-x64-zh-cn.exe
    - 注意去掉IE的 关于 菜单中的 “自动安装新版本”