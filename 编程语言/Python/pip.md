# pip

## pip show

查询已安装 python 包的信息（版本、位置、依赖等）

```shell
$ pip show requests
Name: requests
Version: 2.24.0
Summary: Python HTTP for Humans.
Home-page: https://requests.readthedocs.io
Author: Kenneth Reitz
Author-email: me@kennethreitz.org
License: Apache 2.0
Location: /home/endruz/code/htsz/Natural/venv/lib/python3.8/site-packages
Requires: chardet, urllib3, idna, certifi
Required-by:
```

## pip download

下载 python 包及其依赖的离线包（whl、tar.gz 等）

```shell
pip download -d lib -r requirements.txt
```

```shell
$ pip download -d lib requests==2.24.0
Collecting requests==2.24.0
  Using cached requests-2.24.0-py2.py3-none-any.whl (61 kB)
Collecting certifi>=2017.4.17
  Using cached certifi-2021.10.8-py2.py3-none-any.whl (149 kB)
Collecting chardet<4,>=3.0.2
  Using cached chardet-3.0.4-py2.py3-none-any.whl (133 kB)
Collecting idna<3,>=2.5
  Using cached idna-2.10-py2.py3-none-any.whl (58 kB)
Collecting urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1
  Using cached urllib3-1.25.11-py2.py3-none-any.whl (127 kB)
Saved ./lib/requests-2.24.0-py2.py3-none-any.whl
Saved ./lib/certifi-2021.10.8-py2.py3-none-any.whl
Saved ./lib/chardet-3.0.4-py2.py3-none-any.whl
Saved ./lib/idna-2.10-py2.py3-none-any.whl
Saved ./lib/urllib3-1.25.11-py2.py3-none-any.whl
Successfully downloaded requests certifi chardet idna urllib3
```

## pip install

使用本地离线包批量安装 python 包

```shell
$ pip install --no-index -f file:/home/lib/ -r requirements.txt
Looking in links: file:///home/lib/
Processing ./lib/jieba-0.42.1.tar.gz
Processing ./lib/requests-2.24.0-py2.py3-none-any.whl
Processing ./lib/idna-2.10-py2.py3-none-any.whl
Processing ./lib/urllib3-1.25.11-py2.py3-none-any.whl
Processing ./lib/certifi-2021.10.8-py2.py3-none-any.whl
Processing ./lib/chardet-3.0.4-py2.py3-none-any.whl
Building wheels for collected packages: jieba
  Building wheel for jieba (setup.py) ... done
  Created wheel for jieba: filename=jieba-0.42.1-py3-none-any.whl size=19314476 sha256=4b56058d21f3f71db360f31d720461750d9928ca74b7933d131ea69abb38443c
  Stored in directory: /home/endruz/.cache/pip/wheels/44/ae/e0/589e7a445bb3b99ae8c4137d367979af84790df28cf4e7a319
Successfully built jieba
Installing collected packages: urllib3, idna, chardet, certifi, requests, jieba
Successfully installed certifi-2021.10.8 chardet-3.0.4 idna-2.10 jieba-0.42.1 requests-2.24.0 urllib3-1.25.11
```
