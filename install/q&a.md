常见问题剖析
===================
1. 怎么确认f2etest-client安装成功了？
    http:/127.0.0.1/setuser.asp?username=test&password=hello123&key=xxx（请用site.json文件中设置的apikey替换xxx） 返回ok即可。
2. 新的windows server登录不成功？
    需要了访问http://IP:3000/syncAllRemoteUsers进行账户同步
3. 如果chrome安装到windows server 2008 r2上，用网站提供的脚本会黑屏?
    需要添加以下参数
    ```bash
    start /MAX "" "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --allow-no-sandbox-job %url% %proxyParam%
    ```
4. 远程访问Linux上Mysql报错10061
    解决方案：修改user表。
    ```bash
        mysql -u root -p 密码
        use mysql
        update user set host="%" where user="root" and host="localhost";
        service mysql restart
    ```
5. windows server 2008至少为sp1版本才能安装IE10和IE11。
    如果版本较低，请安装sp1补丁。
    补丁地址：https://www.microsoft.com/zh-cn/download/details.aspx?id=5842
7. 初次使用某版本IE浏览器时，如发现无法登录等异常，请确保cookie是非禁止状态，设置步骤如下：
    浏览器右侧->工具->隐私->将上下滑块设置为"中"即可
8. 怎么配置浏览器hosts？
    点击hosts图标，配置格式如下：
    ```bash
    10.58.24.110 www.gome.com.cn
    10.58.24.110 g.gome.com.cn
    ```
    配置完后，先打开Firefox浏览器，测试一下，如果通过，各浏览器会共享host。
9. 360安全浏览器、360极速浏览器、猎豹浏览器安装后总是重启
    (1)360安全浏览器、360极速浏览器需要安装到windows server 2003的 c 盘的 360  文件夹中，并且 360 文件夹给Users角色授权所有
    (2)猎豹浏览器安装到Windows server 2003的 c盘的 liebao 文件夹中，并且将
    liebao文件夹给Users角色授权所有