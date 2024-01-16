# build_tools

## Overview

## How to use - Linux

**Note**: The solution has been tested on **Ubuntu 22.04**.

### Installing dependencies

You might need to install **Python**, depending on your version of Ubuntu:

```bash
sudo apt-get install -y python
```

### Building ONLYOFFICE products source code

1. install node

```
apt nodejs
```

```
npm cache clean -f //清除npm缓存，执行命令
npm install -g n //n模块是专门用来管理nodejs的版本，安装n模块
n 16.15.1 // 指定node安装版本
npm install -g npm@8.12.1 // 指定npm安装版本
node -v //查看node版本
npm -v //查看npm版本
```

2. proxy set

```
sudo vim /etc/profile
export HTTP_PROXY=http://127.0.0.1:8080
export HTTPS_PROXY=http://127.0.0.1:8080
source /etc/profile
```

```
sudo npm config set proxy http://127.0.0.1:8080
sudo npm config set https-proxy http://127.0.0.1:8080
npm config set registry https://registry.npm.taobao.org
```

3. Go to the `build_tools/tools/linux` directory:

    ```bash
    cd build_tools/tools/linux
    ```

4. Run the `automate.py` script:

    ```bash
    ./automate.py
    ```

If you run the script without any parameters this allows to build **ONLYOFFICE
Document Server**, **Document Builder** and **Desktop Editors**.

The result will be available in the `./out` directory.

To build **ONLYOFFICE** products separately run the script with the parameter
corresponding to the necessary product.

It’s also possible to build several products at once as shown in the example
below.

**Example**: Building **Desktop Editors** and **Document Server**

```bash
./automate.py desktop server
```

## QA

### [fetch & build]: icu

svn: E170013: Unable to connect to a repository at URL 'https://github.com/unicode-org/icu/tags/release-58-2/icu4c'
svn: E000104: Error running context: Connection reset by peer

解决方案：


1. Manual download

```
cd /build_tools/scripts/../../core/Common/3dParty/icu
wget https://github.com/unicode-org/icu/releases/download/release-58-2/icu4c-58_2-src.tgz
tar -zxvf icu4c-58_2-src.tgz
```

2. Modify file

```
vim build_tools/scripts/core_common/modules/icu.py
```

3. Delete two lines of code and save it.

```
if not base.is_dir("icu"):
    base.cmd("svn", ["export", "https://github.com/unicode-org/icu/tags/release-" + icu_major + "-" + icu_minor + "/icu4c", "./icu", "--non-interactive", "--trust-server-cert"])
```
