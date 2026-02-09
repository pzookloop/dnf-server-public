# dnf-server-public

## 推广
一个好玩的大模型聚合平台: [https://gpt.0101.run](https://gpt.0101.run)
便宜且稳定的大模型API分发平台 [https://0101.run](https://0101.run)

## 介绍
所有代码均可随意查看或修改。此端前后端代码都在此仓库中, 前端在webui2目录下。

配套的登录器在这里:
https://github.com/onlyGuo/DNFLogin

## 预览
### 门户页
自带一个前台页面, 页面内容可以在这个目录下更改: `webui2/src/views/custom/legacy/HomePage.vue`
包括首页banner, 版本下载地址等均可自定义修改.
![QQ20251030-091648.png](doc/home.png)
### 后台登录
后台登录页访问地址: `http://你的服务器IP/login`, 页面看起来更加现代化.
![admin-login.png](doc/admin-login.png)
### 玩家管理-账号管理
目前支持查看玩家账号信息, 以及针对账号手动充值点券等功能.
![player-account.png](doc/player-account.png)
### 玩家管理-角色管理
支持查看玩家角色信息, 包括角色的基础属性, 等级信息等, 并且增加了两个小功能, 可以给指定角色发送物品, 可以修改角色属性.
![player-role.png](doc/player-role.png)
#### 发送物品
可以给指定角色发送物品, 物品可以是游戏内的任何物品, 比如装备, 消耗品等. 前提是需要上传Script.pvf文件, 上传好后会自动解析.
![send-email.png](doc/send-email.png)
#### 修改属性
可以修改角色的基础属性, 包括力量, 智力, 体力 等基础属性, 也可以修改HP, MP等, 伤害/防御等战斗属性.
![role-info.png](doc/role-info.png)
### PVF管理
支持上传Script.pvf文件, 上传后会自动解析, 并且可以查看PVF文件中的物品信息, 包括物品ID, 名称等. 本系统中的物品发送功能, 以及物品名称显示功能均依赖于Script.pvf文件.
![pvf.png](doc/pvf.png)
### 登录器管理
支持配置登录器的版本信息, 包括登录器样式, 下载地址等. 玩家在登录时, 会自动下载游戏本体.
![login.png](doc/login.png)
### 版本管理
支持配置游戏版本信息, 包括版本号, 下载地址等. 玩家在登录时, 会自动检查自动下载需要更新的升级包.
![version.png](doc/version.png)
### 辅助配置
支持配置游戏客户端内的辅助功能, 包括自动拾取, 自动翻牌等. 在后台配置好后, 客户端会自动同步这些配置.
![client.png](doc/client.png)


本想直接编译出一个现成的jar包给大家用，但实际考虑后还是决定开源出来，毕竟即使从源码出发，整体的安装过程也不算复杂，并且某些时候水很深。

如果你是第一次使用本项目, 你需要在DOF数据库中执行以下SQL语句来初始化必要的数据表结构:

```sql
ALTER TABLE `d_taiwan`.`accounts`
ADD COLUMN `admin` INT(10) COMMENT '是否是超管',
ADD COLUMN `parent_uid` INT(10) COMMENT '上级用户ID'
ADD COLUMN `isRobot` TINYINT(3) COMMENT '是否是机器人' DEFAULT 0;

create database if not exists `dnf_service` default character set utf8 collate utf8_general_ci;
use `dnf_service`;
/*
 Navicat Premium Data Transfer

 Source Server         : DNF
 Source Server Type    : MySQL
 Source Server Version : 50095 (5.0.95-community-log)
 Source Host           : 100.70.77.33:3000
 Source Schema         : dnf_service

 Target Server Type    : MySQL
 Target Server Version : 50095 (5.0.95-community-log)
 File Encoding         : 65001

 Date: 28/01/2026 16:58:58
*/

SET NAMES utf8;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for ADMIN_TEMP_PASSWORD
-- ----------------------------
DROP TABLE IF EXISTS `ADMIN_TEMP_PASSWORD`;
CREATE TABLE `ADMIN_TEMP_PASSWORD` (
                                      `id` int(10) NOT NULL auto_increment,
                                      `PASSWORD` varchar(64) default NULL,
                                      PRIMARY KEY  (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=2 DEFAULT CHARSET=latin1 COMMENT='超管临时密码表,用于免登录快速操作的授权';

-- ----------------------------
-- Records of ADMIN_TEMP_PASSWORD
-- ----------------------------
BEGIN;
INSERT INTO `ADMIN_TEMP_PASSWORD` (`id`, `PASSWORD`) VALUES (1, 'DNFnb@123.');
COMMIT;

-- ----------------------------
-- Table structure for ASSIST_CONFIG
-- ----------------------------
DROP TABLE IF EXISTS `ASSIST_CONFIG`;
CREATE TABLE `ASSIST_CONFIG` (
                                `ID` int(10) NOT NULL auto_increment,
                                `CONFIG_JSON` longtext NOT NULL,
                                `UPDATE_TIME` datetime default NULL,
                                PRIMARY KEY  (`ID`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of ASSIST_CONFIG
-- ----------------------------
BEGIN;
INSERT INTO `ASSIST_CONFIG` (`ID`, `CONFIG_JSON`, `UPDATE_TIME`) VALUES (1, '{\"服务器地址\":\"49.232.12.79\",\"角色等级上限\":70,\"一键卖分品级\":2,\"含宠物装备\":0,\"SSS评分开关\":1,\"本地GM开关\":0,\"史诗自动确认开关\":0,\"英雄级开关\":1,\"物品图标开关\":1,\"name2开关\":1,\"品级文本开关\":1,\"连发按键组\":[\"X\"],\"快捷键前置\":\"Ctrl\",\"无损画质\":16,\"难度命名\":[\"普通级\",\"冒险级\",\"王者级\",\"地狱级\",\"英雄级\"],\"品级命名\":[\"普通\",\"高级\",\"稀有\",\"神器\",\"史诗\",\"勇者\",\"传说\",\"神话\"],\"简体PVF\":0,\"隐藏功能\":0,\"自动拾取\":{\"拾取模式\":4,\"自定义拾取代码组\":[0,6515]},\"自动翻牌\":{\"上\":0,\"下\":0},\"史诗闪光\":{\"闪光开关\":1,\"闪光代码\":9413},\"补丁信息\":{\"补丁名称\":\"DOF补丁大合集V7\",\"补丁声明\":\"本软件永久免费！用途仅限于测试实验、研究学习为目的，请勿用于商业途径及非法运营，严禁将本软件用于与中国现行法律相违背的一切行为！否则，请停止使用，若坚持使用，造成的一切法律责任及所有后果均由使用方承担，与作者无关，特此声明！\"}}', '2026-01-28 16:54:58');
COMMIT;

-- ----------------------------
-- Table structure for CLIENT_LAUNCHER_BANNER
-- ----------------------------
DROP TABLE IF EXISTS `CLIENT_LAUNCHER_BANNER`;
CREATE TABLE `CLIENT_LAUNCHER_BANNER` (
                                         `ID` int(11) NOT NULL auto_increment,
                                         `TITLE` varchar(255) NOT NULL,
                                         `IMAGE_URL` varchar(1024) NOT NULL,
                                         `SORT_NO` int(11) NOT NULL default '0',
                                         `ENABLED` tinyint(1) NOT NULL default '1',
                                         `CREATE_TIME` datetime default NULL,
                                         PRIMARY KEY  (`ID`),
                                         KEY `IDX_CLIENT_LAUNCHER_BANNER_ENABLED_SORT` (`ENABLED`,`SORT_NO`,`ID`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of CLIENT_LAUNCHER_BANNER
-- ----------------------------
BEGIN;
INSERT INTO `CLIENT_LAUNCHER_BANNER` (`ID`, `TITLE`, `IMAGE_URL`, `SORT_NO`, `ENABLED`, `CREATE_TIME`) VALUES (1, '70版本, 经典重现', 'https://oss.icoding.ink/.inner/dnf/login_1.png', 0, 1, '2026-01-25 21:36:06');
INSERT INTO `CLIENT_LAUNCHER_BANNER` (`ID`, `TITLE`, `IMAGE_URL`, `SORT_NO`, `ENABLED`, `CREATE_TIME`) VALUES (2, '重燃国服经典 回归最初的感动', 'https://oss.icoding.ink/.inner/dnf/login_2.png', 1, 1, '2026-01-25 21:36:08');
COMMIT;

-- ----------------------------
-- Table structure for CLIENT_LAUNCHER_CONFIG
-- ----------------------------
DROP TABLE IF EXISTS `CLIENT_LAUNCHER_CONFIG`;
CREATE TABLE `CLIENT_LAUNCHER_CONFIG` (
                                         `ID` int(11) NOT NULL auto_increment,
                                         `VERSION` varchar(64) character set utf8 NOT NULL,
                                         `DOWNLOAD_URL` varchar(1024) character set utf8 NOT NULL,
                                         `MD5` varchar(64) character set utf8 NOT NULL,
                                         `REMARK` varchar(1024) character set utf8 NOT NULL,
                                         `UPDATE_TIME` datetime default NULL,
                                         `TITLE` varchar(255) character set utf8 NOT NULL,
                                         PRIMARY KEY  (`ID`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=latin1;

-- ----------------------------
-- Records of CLIENT_LAUNCHER_CONFIG
-- ----------------------------
BEGIN;
INSERT INTO `CLIENT_LAUNCHER_CONFIG` (`ID`, `VERSION`, `DOWNLOAD_URL`, `MD5`, `REMARK`, `UPDATE_TIME`, `TITLE`) VALUES (1, '1.0.0', 'https://oss.icoding.ink/.inner/dnf/install-70.zip', '', '', '2026-01-27 16:21:01', '地下城与勇士');
COMMIT;

-- ----------------------------
-- Table structure for CLIENT_LAUNCHER_VERSION
-- ----------------------------
DROP TABLE IF EXISTS `CLIENT_LAUNCHER_VERSION`;
CREATE TABLE `CLIENT_LAUNCHER_VERSION` (
                                          `ID` int(11) NOT NULL auto_increment,
                                          `VERSION` varchar(64) character set utf8 NOT NULL,
                                          `DOWNLOAD_URL` varchar(1024) character set utf8 NOT NULL,
                                          `DESCRIPTION` mediumtext character set utf8 NOT NULL,
                                          `FORCE_UPDATE` tinyint(1) NOT NULL default '0',
                                          `ENABLED` tinyint(1) NOT NULL default '1',
                                          `CREATE_TIME` datetime default NULL,
                                          PRIMARY KEY  (`ID`),
                                          KEY `IDX_CLIENT_LAUNCHER_VERSION_ENABLED_TIME` (`ENABLED`,`CREATE_TIME`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=latin1;

-- ----------------------------
-- Records of CLIENT_LAUNCHER_VERSION
-- ----------------------------
BEGIN;
COMMIT;

-- ----------------------------
-- Table structure for GM_REGIST_AUTH_CODE
-- ----------------------------
DROP TABLE IF EXISTS `GM_REGIST_AUTH_CODE`;
CREATE TABLE `GM_REGIST_AUTH_CODE` (
                                      `id` int(10) NOT NULL auto_increment,
                                      `AUTH_CODE` varchar(64) default NULL COMMENT '授权码',
                                      `USE_COUNT` int(11) default NULL COMMENT '使用次数',
                                      `MAX_COUNT` int(11) default NULL COMMENT '最大使用次数',
                                      PRIMARY KEY  (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=8 DEFAULT CHARSET=latin1;

-- ----------------------------
-- Table structure for recharge_key
-- ----------------------------
DROP TABLE IF EXISTS `recharge_key`;
CREATE TABLE `recharge_key` (
                               `id` int(11) NOT NULL auto_increment,
                               `content` varchar(255) NOT NULL COMMENT '卡密',
                               `type` int(11) NOT NULL COMMENT '0 = 点券，1 = 装备',
                               `face` int(11) NOT NULL COMMENT '面值（type = 0时表示点券数量， type = 1时表示装备编号）',
                               `face_name` varchar(255) default NULL COMMENT '装备名称(type = 1时有效)',
                               `status` int(11) NOT NULL COMMENT '0 = 未使用，1 = 已使用',
                               `use_account` varchar(255) default NULL COMMENT '使用账号',
                               `use_uid` int(11) default NULL COMMENT '使用账号ID',
                               `create_time` datetime NOT NULL COMMENT '创建时间',
                               `use_time` datetime default NULL COMMENT '使用时间',
                               PRIMARY KEY  (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8 COMMENT='充值卡密表';

-- ----------------------------
-- Records of recharge_key
-- ----------------------------

SET FOREIGN_KEY_CHECKS = 1;

```

#### 软件架构
spring-boot


#### 安装过程

1. 安装JDK24环境和Maven编译工具并配置环境变量
2. 拉取本仓库代码。
3. 修改你的数据库配置和短信配置：`src/main/resources/application.yml`:
```` yml
server:
  port: 9001
spring:
  application:
    name: DNF Service Web Application
  datasource:
    url: jdbc:mysql://这里改成你的数据库地址:端口号?useUnicode=true&characterEncoding=utf8
    password: 这里填写你的数据库密码
    username: 这里填写你的数据库账号
    driver-class-name: com.mysql.jdbc.Driver
  freemarker:
    suffix: .ftl
mybatis:
  mapper-locations: classpath*:mappers/*.xml
  type-aliases-package: com.aiyi.game.dnfserver.entity
logging:
  file: logs/dnf.log
  level:
    com.aiyi: debug

aliyun:
  access-key-id: 这里填写你从阿里云申请的access-key-id 否则注册时无法发送短信
  access-secret: 这里填写你从阿里云申请的access-secret 否则注册时无法发送短信
  oss:
    endpoint: 这里不用管，预留，后续制作全自动更新版本时可能用得上
    bucket-name: 这里不用管，预留，后续制作全自动更新版本时可能用得上
````


4. 生成独属于自己的公私钥（长度:`2048bit` 格式:`PKCS#8`）,可以在这里在线生成：http://www.metools.info/code/c80.html 如下图:

   -- 为什么不把公私钥直接内置到程序中而是强制你手动生成？因为公私钥时DOF服务端与客户端通信加密的基础，每个人用不同的密钥可以提高安全性，若都用统一的密钥，那么别有用心的人稍微使点手段就可以跳过密码验证直接登录你的私服，导致玩家账号密码形同虚设。
   
  注意： 如果登录游戏卡在频道页面尝试多次依然无法进入的话，则将格式`PKCS#8`换成`PKCS#1`重新生成。
   
![rsa](https://github.com/onlyGuo/dnf-server-public/raw/main/doc/rsa.png)

5. 用`notpad++`或`vs code`等一类的工具将公钥保存为`publickey.pem`并上传到服务器game目录.

![rsa](https://github.com/onlyGuo/dnf-server-public/raw/main/doc/rsa-pub.png)
![rsa](https://github.com/onlyGuo/dnf-server-public/raw/main/doc/rsa-pub-upload.png)

6. 复制私钥内容（不要复制`-----BEGIN PRIVATE KEY-----`和`-----END PRIVATE KEY-----`这一类的头尾标识）到文件`src/main/resources/private.key` 替换里面的content，如下如:

![rsa](https://github.com/onlyGuo/dnf-server-public/raw/main/doc/rsa-pri.png)

7. 在根目录下执行cmd命令`mvn clean package`进行编译（初次编译会下载各种依赖包，会很慢，约30分钟，耐心等待，网络越好速度越快，若编译失败可以使用‘你懂得的上网方案’）。

8. 观察日志出现如下内容标识编译完成，编译成品为：`target/dnf-server-0.0.1-SNAPSHOT.jar`
````
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  7.395 s
[INFO] Finished at: 2021-04-29T16:14:55+08:00
[INFO] ------------------------------------------------------------------------
````
9. 服务器安装JDK24环境，将`target/dnf-server-0.0.1-SNAPSHOT.jar`上传至服务器
10. 在`dnf-server-0.0.1-SNAPSHOT.jar`同目录下, 上传Script.pvf.
11. 在服务器jar包目录下执行: `nohup java -jar dnf-server-0.0.1-SNAPSHOT.jar &`, 端口默认9001
12. 配置前端页面，确保本机已经安装了Node.js, 以`v22.21.1`为例, 进入到webui2目录中
13. 依次执行下面命令:
```` bash
npm install
npm run build
````
14. 务器创建站点，将本项目源码编译文件放到站点目录中，并配置反向代理防止本项目与后台交互产生跨域问题，以nginx为例。
```nginx
server{
    listen       9002;        # 页面访问端口
    listen  [::]:9002;        # 页面访问端口
    server_name  localhost;
    
    # 站点目录， “/home/html/dnf”改为你自己的目录
    location / {
        root   /home/html/dnf;
        try_files $uri $uri/ /index.html;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # 后台服务代理，“http://172.18.0.3:9001;”改为你自己的服务路径和端口
    location ~/api/v1 {
        proxy_pass http://172.18.0.3:9001;
        proxy_set_header Host $host:$server_port;
    }
}
```

#### 使用说明

1. 启动后在当前目录执行`tail -f logs/dnf.log`可以查看日志以便于排错。
2. 结束本服务进程执行`pkill java`
3. 联系QQ：719348277

### 本地调试说明
需将Script.pvf文件放到项目根目录, 方可完整调试, 打包后, 也需要将Script.pvf于Jar包放到一起部署(打包时, 不会直将Script.pvf打包到Jar包, 要手动放到同一个目录中, 此举是方便在不重新编译的情况下快速更新Script.pvf, 更新完Pvf文件后, 需要重启).
