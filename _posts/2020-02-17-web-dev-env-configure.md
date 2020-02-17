---
layout: post
title: Install Python && Microsoft Build Tools && env config
---

![sample post]({{site.baseurl}}/images/image-2.png)


# Python install && env config

## Install latest version of python
![download python]({{site.baseurl}}/images/2020-2-17/python_download.png)

## Config Python environment
![python env]({{site.baseurl}}/images/2020-2-17/sys_env.png)

# Install Microsoft Build Tools
You need to **install the latest version of Visual Studio** including the "**Desktop development with C++**" workload.

1. install [Microsoft Build Tools 2019](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools)

![c++]({{site.baseurl}}/images/2020-2-17/c_plus_plus.png)

2. run `npm config set msvs_version 2019 --global`

## ref
https://stackoverflow.com/questions/57879150/how-can-i-solve-error-gypgyp-errerr-find-vsfind-vs-msvs-version-not-set-from-c