---
layout: post
title:  "运行和安装wiki.js"
---

1. 下载最新的wiki发布的源码
https://github.com/Requarks/wiki/releases
如https://github.com/Requarks/wiki/archive/v1.0.11.zip
解压之后，在目录中从config.sample.yml中拷贝一个config.js文件，
并做相应的设置

2. 安装npm包
```
npm install
```

3. 运行配置
```
npm run dev-configure
```

按步骤，将会生成一个配置文件config.js

```
title: Wiki
host: 'http://localhost:8080'
port: 8080
paths:
  repo: ./repo
  data: ./data
uploads:
  maxImageFileSize: 3
  maxOtherFileSize: 100
lang: en
langRtl: false
public: false
auth:
  defaultReadAccess: false
  local:
    enabled: true
  google:
    enabled: true
    clientId: GOOGLE_CLIENT_ID
    clientSecret: GOOGLE_CLIENT_SECRET
  microsoft:
    enabled: true
    clientId: MS_APP_ID
    clientSecret: MS_APP_SECRET
  facebook:
    enabled: false
    clientId: FACEBOOK_APP_ID
    clientSecret: FACEBOOK_APP_SECRET
  github:
    enabled: false
    clientId: GITHUB_CLIENT_ID
    clientSecret: GITHUB_CLIENT_SECRET
  slack:
    enabled: false
    clientId: SLACK_CLIENT_ID
    clientSecret: SLACK_CLIENT_SECRET
  ldap:
    enabled: false
    url: 'ldap://serverhost:389'
    bindDn: cn='root'
    bindCredentials: BIND_PASSWORD
    searchBase: 'o=users,o=example.com'
    searchFilter: '(uid={{username}})'
    tlsEnabled: false
    tlsCertPath: 'C:\example\root_ca_cert.crt'
  azure:
    enabled: false
    clientId: APP_ID
    clientSecret: APP_SECRET_KEY
    resource: 00000002-0000-0000-c000-000000000000
    tenant: YOUR_TENANT.onmicrosoft.com
sessionSecret: 13ede35dcdc4d254c44cc79685302cccd34aa707a4b81a4cb2b04f1eebc3635c
db: 'mongodb://<user>:<pass>@ds040877.mlab.com:40877/wikidb'
git:
  url: 'git@github.com:YYY-TEAM/wiki.git'
  branch: master
  auth:
    type: ssh
    username: ''
    password: ''
    privateKey: /Users/yangyibang/.ssh/id_rsa
    sslVerify: true
  showUserEmail: true
  serverEmail: yvan.yang@outlook.com
features:
  linebreaks: true
  mathjax: true
externalLogging:
  bugsnag: false
  loggly: false
  papertrail: false
  rollbar: false
  sentry: false
theme:
  primary: indigo
  alt: blue-grey
  viewSource: all
  footer: blue-grey
  code:
    dark: true
    colorize: true

```

4. 最后就可以运行了
http://localhost:8080


参考：https://docs.requarks.io/wiki/dev-guide