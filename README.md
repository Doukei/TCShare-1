# 暂时停更
安装教程（虚拟主机）
应该会比readme详细点吧。

整一个虚拟主机或者vps。需要php7，建议php7.3。有php7.4更好。

下载代码。你可以直接git clone到空间里，也可以下载到本地。

准备好composer。切换到程序目录，然后执行composer install。

提醒x 如果你的服务器在国内，这里你可能需要设置一下国内镜像源：

composer config repo.packagist composer https://mirrors.aliyun.com/composer/
如果你是在本地安装的依赖，现在把程序包上传到服务器。

设置伪静态规则：
Nginx用户请完整填写以下规则。Apache用户可以无需理会，程序已经包含开箱即用的.htaccess文件。

try_files $uri $uri/ /index.php$is_args$args;
location ~ /\.env {
    deny all;
}
创建.env文件，并按配置填写如下内容：

#   XS 是前缀
#   | -KEY 是配置种类，可选KEY，APP，SEC
#   | | - -ct是key的ID（类似config.php）
#   | | - | - something是配置名称
#   | | - | - | - - - - value在等号右边
#   XS_KEY_ct_something=value

    XS_KEY_ct=ctyun   #必填，值为ctyun
    XS_KEY_ct_FD=     #应用文件夹名
    XS_KEY_ct_AK=     #AK
    XS_KEY_ct_SK=     #SK

#   这里APP后面的可以是任意值，一般就123456下去
#          ↓
    XS_APP_1=/              #挂载路径
    XS_APP_1_NAME=TCShare   #网盘名称
    XS_APP_1_THEME=mdui     #界面主题
    XS_APP_1_BASE=/         #网盘内路径
    XS_APP_1_KEY=ct         #对应上面Key的ID
这里的AK、SK、FD需要你自己获取。

如果你需要多盘挂载，你可以在上述配置之后继续追加：

XS_KEY_ct2=ctyun   #必填，值为ctyun
XS_KEY_ct2_FD=     #应用文件夹名
XS_KEY_ct2_AK=     #AK
XS_KEY_ct2_SK=     #SK

XS_APP_2=/disk2         #挂载路径
XS_APP_2_NAME=TCSecond  #网盘名称
XS_APP_2_THEME=mdui     #界面主题
XS_APP_2_BASE=/         #网盘内路径
XS_APP_2_KEY=ct2        #对应上面Key的ID
这里我们将第二个网盘挂载到/disk2，但是你只能通过/disk2访问。
一个小技巧是：在第一个网盘里新建一个disk2文件夹，就能点击进入了。
注意：除非你知道自己在做什么，不要把两个网盘挂载到相同路径。

访问你的服务器地址，点击链接授权。

如果你是多盘用户，请为你的每个网盘单独授权。
如，先访问http://tcshare.website/授权第一个，再访问http://tcshare.website/disk2/授权第二个。
如果您是按本文进行配置（且并非运行在SCF上），只需要登录账号就能完成授权。

请每个月手动为每个网盘的token续期。如，你的网盘安装在http://tcshare.website/，你需要每个月访问一次http://tcshare.website/-renew。

请不要使用计划任务访问，这不会起到任何作用。

这就结束了。































## 请务必更新最新版 修复了一个可能导致越权读取文件夹的隐患
# TCShare v3
多云盘目录列表程序  
支持Onedrive 国际版和世纪互联 天翼云API 和彩云  
【配置文件可视化编辑器绝赞咕咕咕中】  
[安装教程：这里](https://xylog.cn/2020/03/03/tcshare.html)

### License
本项目使用[LGPL](https://github.com/xytoki/TCShare/blob/v3/LICENSE)协议开源。  
#### 用户协议
1. 凡以任何方式安装、使用本项目者，均被视为自愿接受[LGPL](https://github.com/xytoki/TCShare/blob/v3/LICENSE)与此协议。  
2. 使用本项目挂载各大云盘，均被视为自愿接受**各云盘服务协议**及**相关法律法规**。
3. 请勿使用本项目**分发违反云盘服务协议或相关法律法规的文件**
4. 因不当使用本软件而导致的任何意外、疏忽、合约毁坏、诽谤、版权或其他知识产权侵犯及其所造成的任何损失（如以上第三条），本项目**概不负责**，亦**不承担任何法律责任**。

### Demo

[天翼云](https://xia.st/)  
[和彩云](https://xia.st/caiyun/)  
[OneDrive国际](https://xia.st/d/)  
[OneDrive世纪互联](https://xia.st/c/)  

### 安装方式：

若你要挂载一个网盘到`/`：

1. 获取一个API，并设置环境变量(如SCF等)或填入`.env`或`.env.local`中
2. 上传到服务器，用composer安装依赖
3. 配置伪静态
4. 访问`/-install`进行账号授权，获取token。
5. 记得每个月访问`/-renew`续期一次。续期的时候不需要重新填写token。
6. OneDrive不需要手动续期。

若将网盘挂载到`/disk`，则授权地址会变成`/disk/-install`、`/disk/-renew`，回调地址会变成`/disk/-callback`，请注意区分。

##### v3版本仅支持使用.env文件或环境变量配置。
请一定禁止访问env文件！
```shell
#   XS 是前缀
#   | -KEY 是配置种类，可选KEY，APP，SEC
#   | | - -ct是key的ID（类似config.php）
#   | | - | - something是配置名称
#   | | - | - | - - - - value在等号右边
#   XS_KEY_ct_something=value

    #天翼云配置
    XS_KEY_ct=ctyun   #必填，值为ctyun
    XS_KEY_ct_FD=     #应用文件夹名
    XS_KEY_ct_AK=     #AK
    XS_KEY_ct_SK=     #SK
    #Onedrive配置
    XS_KEY_od=onedrive
    #世纪互联配置
    XS_KEY_od=onedriveCN

#   这里APP后面的可以是任意值，一般就123456下去
#          ↓
    XS_APP_1=/              #挂载路径
    XS_APP_1_NAME=TCShare   #网盘名称
    XS_APP_1_THEME=mdui     #界面主题
    XS_APP_1_BASE=/         #网盘内路径
    XS_APP_1_KEY=ct         #对应上面Key的ID

```
#### OneDrive特别说明
程序已经内置了一组OneDrive的Client ID和Secret，正常情况下不需要手动设置。  
你的授权会经过`https://tcshare-r.now.sh`中转。该网页为纯静态页面，源码位于`_app/redirect/index.html`，不会获取您的个人信息。如仍有疑虑，你可以配置自己的应用:
```bash
    XS_KEY_od_AK=client_id
    XS_KEY_od_SK=client_secret
    XS_KEY_od_FD=redirect_uri  #格式：http://domain/_app/redirect
```
#### SharePoint使用方式
前期配置与onedrive相同，只需要在app配置处加上
```bash
    XS_APP_<id>_MODE=sharepoint
    XS_APP_<id>_SITE=
    #sharepoint的网站名，若地址是http://xxx.sharepoint.cn/site/<name>，只要填写<name>这部分
```
也就是说你可以一个key挂一个od和多个sharepoint。

#### 和彩云使用方式
1. 正常登录和彩云，记得勾选【下次自动登录】。  
2. 打开[这个地址](https://caiyun.feixin.10086.cn/Mcloud/sso/getCyToken.action)。 
3. 复制里面所有内容，**两边加上单引号**（重要！），填入到配置文件/环境变量的`XS_KEY_<name>_TOKEN`字段中  
4. 理论上可用。和彩云cookie有效期理论一年，够用了。
5. 为什么不做自动登录？因为有验证码。  
配置示例：  
```bash
XS_KEY_cm=caiyun
XS_KEY_cm_TOKEN='{"cyToken":"******|*|RCS|******|******","encryPhone":"******"}'
XS_APP_<id>_NAME="TCShare 和彩云"
XS_APP_<id>_THEME=mdui
XS_APP_<id>_BASE=/
XS_APP_<id>_KEY=cm
XS_APP_<id>=/caiyun
```
由于和彩云服务端接口限制，在线播放视频将默认播放经过和彩云服务器转码后的文件，**这将导致视频分辨率降至`720p`(1280x720)，码率降至`1Mbps`**。  
这不会影响文件下载（包括外链播放）。如果遇到外链播放失败的问题，可以尝试在链接后加上`?TC_transcode`请求转码链接。  
你也可以在配置文件中加入`XS_APP_<id>_NO_TRANSCODE=true`禁用转码功能，但这可能导致部分情况下视频播放失败（如目前已知iOS设备无法播放）。  
如仅希望对iOS设备启用转码，可以设置`XS_APP_<id>_NO_TRANSCODE=except_ios`。

### 功能

#### 已支持
 - 文件下载
 - 文件夹打包下载（仅和彩云）
 - 视频播放 (mp4,webm,mkv)，PC使用[ArtPlayer](https://github.com/zhw2590582/ArtPlayer)，手机使用[DPlayer](https://dplayer.js.org)
 - 视频外挂字幕，需为同名在同目录下。目前支持ass特效字幕（[ASS.js](https://github.com/weizhenye/ASS/)），PC支持srt（artplayer自带）
 - 音频播放 (mp3,aac,m4a,flac,ogg,wav) 使用[APlayer](https://aplayer.js.org)
 - Office预览 (doc(x),ppt(x),xls(x),pdf)
 - 图片预览 (bmp,jpg,jpeg,png,gif,webp)，使用[glightbox](https://github.com/biati-digital/glightbox)
 - 多盘
 - 腾讯云函数（SCF）
 - 数据缓存（文件，memcache，redis）
 - 密码加密
 - 防盗链
 - Token鉴权
 - 服务器直接输出（1M以下，`?TC_direct`）
 - header,readme
 - WebDav

#### TODO
 - 更多文件类型的预览

### Rewrite规则：

Apache：
```ApacheConf
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)$ index.php [QSA,L]
</IfModule>
<FilesMatch "^\.">
    Order allow,deny
    Deny from all
</FilesMatch>
```
Nginx:
```nginx
try_files $uri $uri/ /index.php$is_args$args;
location ~ /\.env {
    deny all;
}
```
### WebDav  
TCShare现已支持只读的WebDav，但需要在配置文件里手动启用：
```shell
XS_APP_<id>_DAV=true
```
之后即可通过`https://tcshare/`或者`https://tcshare/-dav`使用WebDAV了。
若需访问加密文件夹，请设置WebDAV独立密码：
```shell
XS_APP_<id>_DAV_AUTH=username:password
```
你还可以设置如下参数，这样`https://tcshare/`将无密码且无法访问加密文件夹，`https://tcshare/-dav`将有密码且可以访问加密文件夹。
```shell
XS_APP_<id>_DAV_AUTH_ONLY_STANDALONE=true
```
#### Nginx配置问题
若使用WebDAV时Nginx报`405 Method Not Allowed`错误，请在Rewrite规则里增加如下：
```nginx
set $sep "?";
if ( $is_args = "?" ){
    set $sep "&";
}
error_page  405 =200 $uri${sep}_FORCE_METHOD=$request_method;
```
### 配置缓存  
默认情况下，TCShare将使用文件缓存数据，您可以设置如下配置而是用memcache或Redis：
```bash
  XS_CACHE_MODE=memcached
  XS_CACHE_PATH=memcached://127.0.0.1:11211
# XS_CACHE_PATH=memcached:///tmp/memcached.sock
# XS_CACHE_PATH=memcached://127.0.0.1:11211;memcached://127.0.0.12:11211
# https://symfony.com/doc/current/components/cache/adapters/memcached_adapter.html

  XS_CACHE_MODE=redis
  XS_CACHE_PATH=redis://127.0.0.1:6379
# XS_CACHE_PATH=redis:///tmp/redis.sock
# https://symfony.com/doc/current/components/cache/adapters/redis_adapter.html
```

### 安全规则
支持密码保护、Token鉴权、Referrer防盗链三种方式。
该功能仅支持在环境变量中配置，不支持config.php。
配置如下：
```bash
XS_SEC_1=/dir1/* 
# 路径规则，参照http://flightphp.com/learn/#routing
# 例如，/dir1 会匹配/dir1 /dir1/
# /dir1/* 匹配 /dir1 /dir1/ 和dir1之下的所有文件
XS_SEC_1_TYPE=referrer
# 模式
XS_SEC_1_MODE=black
# 如果是referrer，设置黑白名单
XS_SEC_1_VAL=baidu.com,google.com
# 黑白名单的域名，逗号分隔
XS_SEC_1_EMPTY=true
# 允许空referrer

# Token鉴权，需要和密码配合使用
# 若token正确优先级在token之后的所有规则都将被跳过
# token不正确将继续下一条规则
# Token计算方式见下
XS_SEC_2=/dir2/*
XS_SEC_2_TYPE=token
XS_SEC_2_VAL=tcshare_demo_key #secret值

# 密码保护
XS_SEC_3=/dir2/*
XS_SEC_3_TYPE=password
XS_SEC_3_VAL=password123
#      ↑
# 此数字决定优先级，优先级高的规则将先执行。
# 除了Token之外，其他规则返回失败时将终止程序
# Token失败会跳到下一条规则 争取会忽略下面所有
# 所以目前如需使用token必须在下面放置password。

XS_SEC_<id>_IGNORE=file    #此规则对文件无效
XS_SEC_<id>_IGNORE=folder  #此规则对文件夹无效
XS_SEC_<id>_IGNORE=mp4;mkv #此规则对mp4 mkv后缀文件无效
```
#### Token计算方式
可用于附件CDN等高级用途。
```php
function token($path,$secret,$exps){
    $baseTime=1580646002;        
    // Magic Number, 实际上是2020-02-02 20:20:02 CST, 别管我为啥这么写
    $exptime=time()+$exps-$baseTime;
    // 过期时间减去上面的magic number
    $usign=substr(md5($path.$secret.$exptime),0,16);
    // 16位MD5
    return $usign.dechex($exptime);
    // 16进制的过期时间
}
$path   = "/dir1/1.jpg?query_string=should_be_calcuated";
    // querystring也需要计算
$secret = "tcshare_demo_key";
$expire = 300;
echo $path."&_tcshare=",token($path,$secret,$expire);
// returns /dir1/1.jpg?query_string=should_be_calcuated&_tcshare=1d435a917e04e8c824eb21
```

### 其他配置项
```shell
    XS_SUBTITLE_FIND='${dir}subs/${name}.srt;${dir}subs/${name}.ass'
    # 设置播放器如何搜索字幕文件。仅PC版。记得写配置文件要有两侧单引号。
```

### 在腾讯云云函数(SCF)运行

1. 下载程序
2. 使用`composer`安装依赖
3. 上传到腾讯云，函数名是`index.main_handler`
4. 设置环境变量，然后安装
5. 若绑定域名，请增加一条环境变量：`scf_base=/`，其中`/`是绑定的路径。
6. 设置环境变量的时候**注意不需要两侧引号！！！**
### 付费定制等

本程序开发纯属个人需求及爱好，故何时更新、更新什么内容也岁个人兴趣。  
如果你喜欢此项目，或需要安装指导等服务，亦或是极度需要某个功能，欢迎赞助。    
另可根据需求进行付费定制，如二次开发整合等。

[PayToki](https://paytoki.now.sh)
