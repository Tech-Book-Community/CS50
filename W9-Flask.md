<iframe width="640" height="360" src="https://www.youtube.com/embed/-aqUek49iL8?si=mPdWuyBP8Qcj3clX" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# 目錄

- [1. 前言](#1-前言)
- [2. 參考資料](#2-參考資料)
- [3. Poetry](#3-poetry)
  - [3-1. Poetry 簡介](#3-1-poetry簡介)
  - [3-2. Poetry 安裝與設定](#3-2-poetry安裝與設定)
  - [3-3 Poetry init 專案](#3-3-poetry-init-專案)
  - [3.4 `pyproject.toml`](#34-pyprojecttoml)
- [4. Flask](#4-flask)
  - [4.1 安裝 flask](#41-安裝flask)
  - [4.2 設定 Flask](#42-設定flask)
  - [4.3 設定 `route`](#43-設定-route)
  - [4.4 template](#44-template)
  - [4.5 static](#45-static)
  - [4.5 jinja](#45-jinja)
  - [4.5 Child Template](#45-child-template)
- [5. 登入實做](#5-登入實做)
  - [5.1 SQLite](#51-sqlite)
  - [5.2 Register](#52-register)
  - [5.3 Login](#53-login)

---

# 1. 前言

在 CS50(2024 Spring) Class 9 中, David 向我們介紹如何使用 Python 的 Flask 框架與 Html, CSS, JavaScript 搭建全端框架。本文將介紹如何使用 Poetry 架設 Python 虛擬環境， 並使用 Flask 依照 Folder Structure 搭建全端專案, 並使用 SQLite 作為資料庫。

本文為 [CS50 Spring Class 9 Problem C$50 Finance](https://cs50.harvard.edu/college/2024/spring/psets/9/finance/)的延伸，可以先下載原始檔案，裡面已經有原始的 Html。

---

# 2. 參考資料

- [CS50 2024 Spring Week 9 Flask](https://cs50.harvard.edu/college/2024/spring/weeks/9/)
- [CS50 2024 Spring Week 9 Flask 課程筆記](https://cs50.harvard.edu/college/2024/spring/notes/9/)
- [Python 套件管理器——Poetry 完全入門指南](https://blog.kyomind.tw/python-poetry/)
- [Poetry + pyenv 教學：常用指令與注意事項](https://blog.kyomind.tw/poetry-pyenv-practical-tips/)
- [NarayanAdithya/Flask-Poetry](https://github.com/NarayanAdithya/Flask-Poetry)
- [TinyMurky/CS50_Week9_Finance_Hw_Poetry](https://github.com/TinyMurky/CS50_Week9_Finance_Hw_Poetry)

---

# 3. Poetry

關於 Poetry 如何安裝建議直接閱讀 kyo 大大的[Python 套件管理器——Poetry 完全入門指南](https://blog.kyomind.tw/python-poetry/), 裡面有完整的 Poetry 介紹, 以下則是為了啟動 Flask 專案所需要的步驟。

## 3-1. Poetry 簡介

Poetry 在其[官網](https://python-poetry.org/)和[GitHub](https://github.com/python-poetry/poetry#poetry-dependency-management-for-python)上被定義為「**讓 Python 套件打包與依賴管理變得簡單**」。Poetry 的主要目的是簡化 Python 的套件管理。過去，我們通常使用 Pip 或 Anaconda 來管理套件，Pip 會將套件安裝在全域環境中，而 Anaconda 的操作較為複雜。Poetry 則提供了類似於 Npm 的方式，透過`pyproject.toml`檔案來管理套件，並且將虛擬環境(`venv`)設定在專案資料夾內部，所有套件都安裝在此處。此外，Poetry 允許使用`poetry run xxx`來運行定義在`pyproject.toml`中的指令，例如使用`poetry run dev`來啟動`dev = "app.main:start_dev"`，作為專案的入口點。

## 3-2. Poetry 安裝與設定

> 安裝 Poetry

Poetry 可以用[pipx](https://python-poetry.org/docs/#installing-with-pipx)或是[official installer](https://python-poetry.org/docs/#installing-with-the-official-installer), 這裡我使用後者。輸入以下指令，如果遇到問題的話可以先檢查 Python 版本，我發現 3.7 版本以下會無法安裝。

```
curl -sSL https://install.python-poetry.org | python3 -
```

接著在 `.zshrc`, `.bashrc` 或是 `.bash_profile`內加上以下指令，這是因為 Poetry 會被安裝在`~/.local/share/pypoetry`

```
export PATH=$PATH:$HOME/.local/bin
```

接著要用`source`讓 shell reload 我們剛剛輸入的指令(依照檔名更換`.bashrc`的部份)：

```
source ~/.bashrc
```

> 設定 Poetry

kyo 大大的[Python 套件管理器——Poetry 完全入門指南](https://blog.kyomind.tw/python-poetry/) 中有許多設定 Poetry 的技巧，我認為最重要的設定是以下這個指令(請直接打在 command line):

```
poetry config virtualenvs.in-project true
```

接著用 `poetry config --list`查看，如果看到`virtualenvs.in-project = true`就代表以後虛擬環境的`.venv` 會裝在 專案裡面而不會像是`pyenv`一樣裝在專案資料夾的外面。

```
poetry config --list

> ...省略
> virtualenvs.in-project = true
> ...省略
```

## 3-3 Poetry init 專案

> init 專案

在想要生成的資料夾路徑中，於 command line 中輸入以下指令，另外需要注意：

- `fincanc_2`: 是我的專案名稱，可以自由選擇
- `--src`: 這一段不一定要加，但是加的話會生成 `src`資料夾，並且將與專案名稱相同的資料夾創建在 `src` 裡面，變成 `finance_2/src/finance_2`的結構

```
poetry new finance_2 --src
```

> 以下是生成後的 folder structure

![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240826211235882.png>)

> 產生虛擬環境

接著使用以下指令生成`.venv`在專案資料夾裡面, 也就是虛擬環境 。但要注意如果有特別選擇版本，該版本的 python 在 global 環境中也要有相對應的版本。

```
poetry env use python

or

poetry env use python3.11
```

輸入完之後可以看到專案資料夾內出現`.venv`，並且在`lib` 中看到虛擬的 python 環境`python3.11`
![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240826212108386.png>)

> 進入到`.venv`環境

使用下面的指令讓 shell 進入到 `.venv`

```
poetry shell
```

接著你的 command line 應該會看到`venv` 的字樣，但是我裝了 `oh my bash`套件所以看起來像是下面這個樣子，這樣我們的 python 就進入到虛擬環境中。

![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240826212502035.png>)

> 設定 VSCode

按下 `ctrl + shift + p`進入到以下畫面，選擇 `Python: Select Interpreter`(找不到的話可以輸入`python`)。

![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240826213039526.png>)

選擇`./venv/bin/python` 的這個 path, 它會指向我們裝在專案裡面的`.venv`內的 python。這樣 vscode 就不會在 import 的時候找不到各種 package 而出現紅底線警告。
![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240826213215323.png>)

## 3.4 `pyproject.toml`

進入到專案內部，會有一個`pyproject.toml`, 如果常在用 `npm`或是`rust cargo`等套件管理器的對此一定不陌生，這邊是管理整個專案版本與套件的[TOML](https://zh.wikipedia.org/zh-tw/TOML)文件。其中專案都會在`[tool.poetry.dependencies]` 更詳細的內容可以看官網: [The pyproject.toml file](https://python-poetry.org/docs/pyproject/)

```toml
[tool.poetry]
name = "finance-2"
version = "0.1.0"
description = ""
authors = ["Tiny_Murky <murky0830@gmail.com>"]
readme = "README.md"
packages = [{include = "finance_2", from = "src"}]

[tool.poetry.dependencies]
python = "^3.11"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

> scripts

`pyproject.toml` [scripts](https://python-poetry.org/docs/pyproject/#scripts) 中只要設定 script，就可以直接啟動專案中某個某個資料集內的`py`檔中的一個 function, 也就是說可以用來當作我們啟動專案的入口。但是這個專案需要出現在 `[tool.poetry]` 的 package array 之中。

假設我今天把專案中的 `financial_2/src/financial_2`改名叫做`financial_2/src/app`，我的 toml 檔也要要成：

```toml
[tool.poetry]
name = "finance-2"
version = "0.1.0"
description = ""
authors = ["Tiny_Murky <murky0830@gmail.com>"]
readme = "README.md"
packages = [{include = "app", from = "src"}]

[tool.poetry.dependencies]
python = "^3.11"
```

![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240826221357232.png>)

接著在`financial_2/src/app` 新增 `main.py` 然後貼入以下程式碼：

```python
"""
This is entry point
"""
def start():
    """
    project start here
    """
    print("Hello Word")
```

回到`pyproject.toml`，在最下面貼上 以下部份，在這裡可以看到 `app`是因為我們在`package` 中寫入`{include = "app", from = "src"}` 我們才可以從 app 開始，如果只有`{include = "src"}` 就會需要`src.app.main:start`。

而資料夾路徑與`py`檔用`.`連接，`py`檔要用哪個 function 在`:`之後

```toml
[tool.poetry.scripts]
start = "app.main:start"
```

完成之後需要先輸入以下指令

```
poetry install
```

然後在確定有使用`poetry shell` 在虛擬環境的情況下輸入:

```
poetry run start
```

就可以看到以下畫面，注意`run start`的 `start`指的是`start = "app.main:start"`等號左邊的名稱, 而不是在`main.py`檔中的 function 名稱

![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240826222423535.png>)

# 4. Flask

flask 是 python 的輕量化 web 框架，裡面搭建了`Werkzeug`資料庫的**Python Web Server Gateway Interface**以及`jinja2`的模板引擎,讓我們不需要前端框架也可以從後端生成 html。

## 4.1 安裝 flask

在 poetry 專安裝套件很簡單，只需要輸入以下指定就可以安裝：

```
poetry add flask
```

## 4.2 設定 Flask

以下的部份我是參考 Github:[NarayanAdithya/Flask-Poetry](https://github.com/NarayanAdithya/Flask-Poetry)的程式碼架構的。
首先先在 `src`底下新增 `src/template` 和 `src/static`，如下圖：

![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240827202926760.png>)

`template`用來放置 `html`檔案，而`static`則是放 Public 文件像是`css`或是圖檔。

完成後新增`src/libs`資料夾後，於內部新增 `__init__.py`與`common.py`兩個檔案，`__init__.py`會讓`libs`資料夾變成一個 module 讓我們可以用 python 的 import 方法 import module 內的 function(可以參考：[Python Basics: Why use **init**.py?](https://sarangsurve.medium.com/python-basics-why-use-init-py-c88589e44c91))。

接著在`common.py`檔中貼入以下 function, 這個可以讓我們輸入相對路進之後，轉變成會從`src`向下的絕對路徑，他的意思是從 `src/libs/common.py`路徑出發，往上兩層的資料夾，也就是`src`，再和相對路徑組合。

```python
"""
Common.py included functions that can't categorize into special py file
"""
import os
from pathlib import Path


def get_abs_path(path: str):
    """
    get absolute path start from src
    """
    abs_path = Path(__file__).absolute().parent.parent

    return os.path.join(abs_path, path)

```

接著回到`src/app/__init__.py`，在這裡我們要設定 flask 的本體 `app`物件。在`src/app/__init__.py`貼入以下程式碼：

```python
"""
init flask here, and set session and other config
"""
from flask import Flask
from src.libs.common import get_abs_path

# Setting up flask
template_path = get_abs_path("template")
static_path = get_abs_path("static")
app = Flask(__file__, template_folder=template_path, static_folder=static_path)

print("__file__: ", __file__)

```

上面這段程式碼是使用絕對路徑的方式告訴 Flask `template` 和 `static` 資料夾在哪裡。

而`Flask`則是 init 整個 app, 其中`__file__`這變變數會是`src/app/__init__.py`的地址如下：

```
__file__： /home/tinymurky/my_code/cs50/finance_2/src/app/__init__.py
```

我們也可以使用下面這種絕對路徑也可以執行。

```python
app_absolute = get_abs_path("app/__init__.py")

app = Flask(app_absolute, template_folder=template_path, static_folder=static_path)
```

接著回到`src/app/main.py` 修改下面的程式碼，在這裡`dev`會啟動 debug 模式，[flask 官網](https://flask.palletsprojects.com/en/2.3.x/tutorial/deploy/#run-with-a-production-server)有提到：

> When running publicly rather than in development, you should not use the built-in development server

flask 內建的 run 僅適合用於 development, 不適合 Production 版本，等等我們會在 start 加上 Production 版本。

第一行`from src.app import app`的 app 其實就是 `src/app/__init__.py`裡面的`app`，在 python 中寫在 file 的 Variable 或 function 不需要特別 export 就可以被其他程式碼 import 使用了(我花了好多時間才搞懂@@)

```python
from src.app import app

PORT = 3000

def start():
	print("Hello World")

def dev():
    """
    Start dev mode
    """
    app.run(debug=True, port=PORT)
```

接著在 Poetry 加上 dev

```toml
[tool.poetry.scripts]
start = "app.main:start"
dev = "app.main:dev"
```

並於 command line 啟動

```shell
poetry run dev
```

會出現以下文字

```
Warning: 'dev' is an entry point defined in pyproject.toml, but it's not installed as a script. You may get improper `sys.argv[0]`.

The support to run uninstalled scripts will be removed in a future release.

Run `poetry install` to resolve and get rid of this message.

__file__:  /home/tinymurky/my_code/cs50/finance_2/src/app/__init__.py
__file__:  /home/tinymurky/my_code/cs50/finance_2/src/app/__init__.py
 * Serving Flask app '/home/tinymurky/my_code/cs50/finance_2/src/app/__init__.py'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:3000
Press CTRL+C to quit
```

在瀏覽器輸入`127.0.0.1:3000`就可以啟動了，`ctrl+c`可以跳出，如果不想看到`Warning`字樣，可以輸入以下指令之後再啟動。

```
poetry install
```

## 4.3 設定 `route`

起動`dev` 後進入`127.0.0.1:3000` 會發現出現 `Not found`, 這是因為我們還沒有設定`route`，以下介紹要如何使用資料夾結構來整理 flask 的 router。

新建 `src/app/routes`以及下面兩個檔案 `src/app/routes/__init__.py`與`src/app/routes/route.py`
![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240827215505532.png>)

並在`src/app/routes/__init__.py`貼入以下程式碼，Blueprint 的更詳細內容可以看[Python Web Flask — Blueprints 解決大架構的網站](https://medium.com/seaniap/python-web-flask-blueprints-%E8%A7%A3%E6%B1%BA%E5%A4%A7%E6%9E%B6%E6%A7%8B%E7%9A%84%E7%B6%B2%E7%AB%99-1f9878312526)或是[flask 官網](https://flask.palletsprojects.com/en/3.0.x/blueprints/)(看完之後才發現原來 Blueprint 也可以有自己的 `template`和`static`，但本篇先暫時都只用 app 的 template)

```python
"""
init router
"""
from flask import Blueprint

print("__name__: ", __name__) # __name__:  src.app.routes
root = Blueprint("root", __name__, url_prefix="/")

# 下面這行一定要放在最下面
from . import route # 這個route指的是src/app/routes/route.py

```

Blueprint 我自己的想像就像是一個比較小的`Flask() app`，它負責網頁中的一個`route`，root 負責 根目錄`/`, 只要是從 `127.0.0.1/` 進入的都會經過這個 route 處理。

接著在`src/app/routes/route.py`貼入以下程式碼：
在這裡的 `@root.route("/")`是`decorator`，在裡面我們設定 是進入 root 下的 `/`路徑會走進`index()` function，這樣講比敬混搖，舉個例子來說，如果設定`test = Blueprint("test", __name__, url_prefix="/test_route")`, `decorator` 設定成`@test.route("/haha", methods=["GET"])`的話，就要從`127.0.0.1/test_route/haha`來進入 index function。

```python
"""
This file contains all
"""
from src.app.routes import root

@root.route("/", methods=["GET"])
def index():  # function 名稱可以隨便取名
    """
    Root router
    """
    return "Hello World!"

```

另外如果在 import root 的時候下面的 code 出現紅底線(`pylint error`)的話，可以`poetry install`之後重起 vscode

```
from src.app.routes import root
```

最後還需要在 app 上面 register 我們才能正式使用這個 route, 前往`src/app/main.py` 修改以下程式碼

```python
"""
init flask here, and set session and other config
"""
from flask import Flask
from src.libs.common import get_abs_path

# 新增下面這行
from src.app.routes import root

# Setting up flask
template_path = get_abs_path("template")
static_path = get_abs_path("static")
app_absolute = get_abs_path("app/__init__.py")
app = Flask(__file__, template_folder=template_path, static_folder=static_path)

# 新增下面這行
app.register_blueprint(root)


```

最後啟動 flask，然後在瀏覽器輸入`http://127.0.0.1:3000`，就會進入以下畫面，恭喜你成功做出第一隻 api 了

```
poetry run dev
```

![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240827224629950.png>)

## 4.4 template

Flask 內建`jinja2`進行後端渲染 html 之後傳給前端，flask 可以從`template`資料夾拿出 html 模板並渲染，也就是下面這段中的`template_folder`設定的 path，也就是`src/template`

```python
template_path = get_abs_path("template")

app = Flask(__file__, template_folder=template_path, static_folder=static_path)
```

我們先從 CS50 Week 9 Finance 的 zip 檔中將 `templates`下的`layout.html`並複製到 Poetry 專案的`src/template/layout.html`，如果你還沒下載的話可以看下面的這個指令下載。

```shell
wget https://cdn.cs50.net/2024/spring/psets/9/finance.zip
```

接著進入 `src/app/routes/route.py`增加下面的程式碼，並使用`poetry run dev`之後進入`localhost:3000`

```python
"""
This file contains all
"""

from flask import render_template # 增加此行
from src.app.routes import root

@root.route("/", methods=["GET"])
def index():  # function 名稱可以隨便取名
    """
    Root router
    """
    # return "Hello World!" # 刪除此行
    return render_template("layout.html") # 增加此行
```

如果你看到下面這個畫面就成功了。

這是因為我們使用 `render_template`將`template_folder`裡面的`layout.html`拿出來渲染，需要注意的是`layout.html`是從`template_folder`當作 root，意思指如果你是放在 `template/folder1/layout.html`裡面，你就需要寫成`return render_template("layout.html")`
![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240903203459773.png>)

## 4.5 static

`static` 和 template 的概念一樣，是用來放靜態檔案像是圖片或是`css`檔案，我們把 CS50 Week 9 Finance 的 zip 檔中將 把 static 裡的檔案都放到 `src/static`裡面。

![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240903205114905.png>)

接著清除瀏覽器的快取之後然後重整`localhost:3000`，如果你看到一個錢的小圖案就成功了
![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240903205835794.png>)

也可以在瀏覽器中輸入 `http://localhost:3000/static/favicon.ico`，可以看到下面的圖片
![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240903210834654.png>)

原理一樣是因為我們在`Flask()`中有設定

```python
static_path = get_abs_path("static")

app = Flask(__file__, template_folder=template_path, static_folder=static_path)
```

## 4.5 jinja

[jinja](https://jinja.palletsprojects.com/en/3.1.x/) 是一個模板引擎，它可以用 html 架構搭建網站，但又在其中加上一些特殊 syntex, 接著可以從`render_template()` function 傳入值去渲染這個畫面。

例如在`layout.html`找到 `<main>`之後

```html
<main class="container py-5 text-center h-75">
  {% block main %}{% endblock %}
</main>
```

改成下面這樣

```html
<main class="container py-5 text-center h-75">
  {{ input_some_stuff }} {% block main %}{% endblock %}
</main>
```

接著回到`src/app/routes/route.py`更改下面兩行後 重新整理`localhost:3000`

```python
@root.route("/", methods=["GET"])
def index():  # function 名稱可以隨便取名
    """
    Root router
    """

    some_cool_stuff = "So Cool!" # 更新這裡
    return render_template("layout.html", input_some_stuff=some_cool_stuff) # 更新這裡
```

看到出現 `So cool!` 就成功了。
在這裡的`{{ input_some_stuff }}` 會提示給 jinja, 看我們在 `render_template()`的`input_some_stuff` 中 取出資料放在`html`渲染。
![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240903212718224.png>)

> 常用的 jinja syntax

jinja 裡面有像是 python 裡面的 `for` 和`if` 的寫法，可以參考這邊[Template Designer Documentation](https://jinja.palletsprojects.com/en/3.0.x/templates/)<在此不詳細說明

> if:

```html
<div>{% if True %} yay {% endif %}</div>
```

> for

```html
<ul>
  {% for item in seq %}
  <li>{{ item }}</li>
  {% endfor %}
</ul>
```

> Child Template

我們前面編輯的`layout.html`之所以叫做`layout`是一位它其實要當作其他 html 的模板，我們可以觀察`<title>` 與`<main>`裡面的 `{% block %}`部份

```html
<head>
  <!-- 中間省略 -->
  <title>C$50 Finance: {% block title %}{% endblock %}</title>
</head>
```

```html
<main class="container py-5 text-center h-75">
  {% block main %}{% endblock %}
</main>
```

接著我們在 `template`內新增一個`home.html` 然後貼上

```html
{% extends "layout.html" %} {% block title %} Home {% endblock %} {% block main
%} {{ input_some_stuff }} {% endblock %}
```

然後記得把 layout 裡面的`{{ input_some_stuff }}`刪除

接著到`src/app/routes/route.py`修改`render_template`。

```python
@root.route("/", methods=["GET"])
def index():  # function 名稱可以隨便取名
    """
    Root router
    """
    some_cool_stuff = "So Cool!"
    return render_template("home.html", input_some_stuff=some_cool_stuff) # 修改 layput.html
```

接著重新整理`localhost:3000` 會出現以下畫面， 可以看到標題出現了 `Home`的字樣，並且`So Cool!`還是維持在畫面上。

這是因為在 `home.html` 中 `{% extends "layout.html" %}`表`home.html`繼承`layout.html`的模板，layout 中在`title`的`{% block title %}{% endblock %}`代表 `home.html`可以使用 `title`名稱的 block, 把值從`home.html`放到 layout 的`title` block 裡面，`{% block main %}{% endblock %}` 也是一樣，下方的`{{ input_some_stuff }}`會整個被放在 layout 的`main` block。

```html
{% block main %} {{ input_some_stuff }} {% endblock %}
```

這樣的好處是不同頁面可以共享同一個 模板，就不需要每個頁面都要重新寫`<nav>`或是`<footer>`等重複的物件。
![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240903221454066.png>)

# 5. 登入實做

本文將實做登入的功能(其他功能礙於篇幅暫時不會寫不好意思@@，想要看其他功的實做可以參考我的[Github](https://github.com/TinyMurky/CS50_Week9_Finance_Hw_Poetry))

登入功能將會使用 `SQLite3` (更詳細的 SQLite 介紹可以看 [CS50 Week8 SQL](https://cs50.harvard.edu/college/2024/spring/weeks/7/))儲存用戶資料 並使用 Session 紀錄登入狀態，並會實做簡單的 Error handler

## 5.1 SQLite

[SQLite](https://www.sqlite.org/) 不像是其他的 **RDBMS** 關聯是資料庫系統一樣是 client/server 結構的資料庫引擎，而是一個與程式碼整合的資料庫。

Python 提供 [sqlite3](https://docs.python.org/3/library/sqlite3.html) Package，讓我們不用在本地端安裝 SQLite 也可以使用。

> 備註：以下的 SQLite 我是使用 Singleton Pattern, 讓整個程式碼只會使用同一個 instance, 但不一定要這樣寫

首先先在`src`資料夾向下創立 `src/sql`，並在其中創立 `src/sql/schema.sql`、`src/sql/sqlite.py` 兩個檔案。其中`src/sql/schema.sql` 會用來 create database 的 table, 而`src/sql/sqlite.py` 可以用 python 來操控 SQLite。

我們在 `src/sql/sqlite.py`

```python
import sqlite3

from src.libs.common import get_abs_path

class SQL:
    """
    Singleton to access database by sqlite
    """

    _db_path = get_abs_path("sql/finance.db")
    _instance = None
    _connect = None
    _cursor = None

    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super(SQL, cls).__new__(cls, *args, **kwargs)
            cls._connect = sqlite3.connect(cls._db_path, check_same_thread=False)
            cls._connect.row_factory = SQL.dict_factory
            cls._cursor = cls._connect.cursor()
        return cls._instance

    @staticmethod
    def dict_factory(cursor: sqlite3.Cursor, row):
        """
        auto mapping sql data to dictionary
        """
        fields = [column[0] for column in cursor.description]
        return {key: value for key, value in zip(fields, row)}

    @staticmethod
    def migrate():
        """
        migrate database
        """
        if SQL._connect is None or SQL._cursor is None:
            SQL._connect = sqlite3.connect(SQL._db_path, check_same_thread=False)
            SQL._cursor = SQL._connect.cursor()

        sql_schema_path = get_abs_path("sql/schema.sql")

        with open(sql_schema_path, "r", encoding="utf-8") as f:
            sql_query = f.read()

        SQL._cursor.executescript(sql_query)
        SQL._connect.commit()
```

簡單解說一下，首先是最上段，這裡的`__new__`可以用來生成 Singleton, 可以參考：[用**new**来实现单例](https://xxhs-blog.readthedocs.io/zh-cn/latest/how_to_be_a_rich_man.html#id2)

在這裡的首先先看到 `_db_path = get_abs_path("sql/finance.db")`，在這裡是設定資料庫存放的位置，如果該位置沒有`finance.db`就會自己在該位置生成一個，此處我設定在 `src/sql/finance.db`。

接著有`_connect`和`_cursor`，connect 是一個[Connection](https://docs.python.org/3/library/sqlite3.html#sqlite3.Connection) object, 他代表 SQLite connect 到硬碟上面的資料庫。[Cursor](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor)則是用來對資料庫下指令並取得指令後回傳的資料。像是 `cur.execute("CREATE TABLE movie(title, year, score)")`就會創造該 table, 但需要注意，一般 read 的時候只需要使用 cursor, create、delete、update 都需要在`cursor.execute`之後另外加一個`connect.commit()`，才會對 database 進行操作。

```python
class SQL:
    """
    Singleton to access database by sqlite
    """

    _db_path = get_abs_path("sql/finance.db")
    _instance = None
    _connect = None
    _cursor = None

    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super(SQL, cls).__new__(cls, *args, **kwargs)
            cls._connect = sqlite3.connect(cls._db_path, check_same_thread=False)
            cls._connect.row_factory = SQL.dict_factory
            cls._cursor = cls._connect.cursor()
        return cls._instance
```

接下來是`dict_factory` (可以參考: [How to create and use row factories](https://docs.python.org/3/library/sqlite3.html#how-to-create-and-use-row-factories))。

```python
    @staticmethod
    def dict_factory(cursor: sqlite3.Cursor, row):
        """
        auto mapping sql data to dictionary
        """
        fields = [column[0] for column in cursor.description]
        return {key: value for key, value in zip(fields, row)}
```

在沒有設置 row factory 之前，資料庫中的一個 Row 被拿出來的時候，會是一個 tuple, 假設一個 row 有 `id`和`username` 兩個欄位，如果拿出一個 row 就會像是 `(1, "tinymurky)` 要選擇其中的值就須使用 index 去抓欄位, row factory 會在一個 row 被抓出來之後先做預處理，在這裡是依照 column 的名稱去 mapping, mapping 後會變成下面這樣的 dictionary

```json
{
  "id": 1,
  "username": "tinymurky"
}
```

最後是 `migrate` function, 使用這個 function 可以生成 `finance.db` 資料庫(在上面的`_db_path`設定)

```python
    @staticmethod
    def migrate():
        """
        migrate database
        """
        if SQL._connect is None or SQL._cursor is None:
            SQL._connect = sqlite3.connect(SQL._db_path, check_same_thread=False)
            SQL._cursor = SQL._connect.cursor()

        sql_schema_path = get_abs_path("sql/schema.sql")

        with open(sql_schema_path, "r", encoding="utf-8") as f:
            sql_query = f.read()

        SQL._cursor.executescript(sql_query)
        SQL._connect.commit()
```

在使用這個 function 前，需要先新增 schema sql 檔案:`/src/sql/schema.sql`。並在其中貼入以下程式碼：

```sql
PRAGMA foreign_keys = OFF;
DROP TABLE IF EXISTS users;
PRAGMA foreign_keys = ON;

CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
    username TEXT NOT NULL,
    hash TEXT NOT NULL,
    cash NUMERIC NOT NULL DEFAULT 10000.00
);

CREATE UNIQUE INDEX username ON users (username);
```

這會建立一個 users table 並且把 `username`設定為`UNIQUE`也就是不能有重複的。其中的

```sql
PRAGMA foreign_keys = OFF;
DROP TABLE IF EXISTS users;
PRAGMA foreign_keys = ON;
```

可以在每次重建的時候都先刪除舊的 table, `foreign_keys`可以無視不同 table 間的 foreign_key 的關聯直接刪掉。

在`migrate`檔之中，由於他是 `static` 的 function, 需要先假設他在使用的時候 `sql.connect`和`sql.cursor`都還沒有 initialize 所以才會有：

```python
        if SQL._connect is None or SQL._cursor is None:
            SQL._connect = sqlite3.connect(SQL._db_path, check_same_thread=False)
            SQL._cursor = SQL._connect.cursor()
```

而下面這部份則是從 loacl 的資料夾中讀出剛剛寫好的`schema.db`然後執行。

```python
        sql_schema_path = get_abs_path("sql/schema.sql")

        with open(sql_schema_path, "r", encoding="utf-8") as f:
            sql_query = f.read()

        SQL._cursor.executescript(sql_query)
        SQL._connect.commit() # create 操作需要呼叫commit 才會執行
```

接著到`pyproject.toml` 新增 `db_migrate` 之後，執行 `poetry run db_migrat`, 就會看到 `financial_db` 出現在 `/src/sql/financial_db`！

```toml
[tool.poetry.scripts]
db_migrate = "sql.sqlite:SQL.migrate" # <-新增這裡
start = "app.main:start_prod"
dev = "app.main:start_dev"
```

> Create/Find user

回到 `src/sql/sqlite.py`, 貼入以下程式碼，首先會先用 assert 檢查是否存在 `self._connect`和 `self._cursor`, 如果存在則新增用戶，這邊的密碼會先用 `generate_password_hash` hash 過之後才存入，這樣就算資料庫被破解也不會知道。

接著使用 `INSERT` query 將 `username`和`hashed_password`放進資料庫，它會在`self._cursor.execute(query, (username, hashed_password))` 自動補在 `query`的兩個`?`位置。使用 `?`主要是防止 [sql injection](https://zh.wikipedia.org/zh-tw/SQL%E6%B3%A8%E5%85%A5), 最後的`self._cursor.lastrowid`可以拿出剛剛 insert 進去的 id。

```python
from werkzeug.security import generate_password_hash # 這個請放在檔案最上方
    def create_user(self, username: str, password: str):
        """
        create user
        """
        assert self._connect is not None
        assert self._cursor is not None

        if not isinstance(username, str) or not isinstance(password, str):
            raise Exception('InvalidDevInputArgument')

        hashed_password = generate_password_hash(password=password)
        query = """
        INSERT INTO users (username, hash)
        VALUES
        (?, ?)
        """

        self._cursor.execute(query, (username, hashed_password))
        self._connect.commit()

        return self._cursor.lastrowid
```

接著往下繼續貼入下面這個 function, 這個 function 可以用 `username`或是`id`來搜尋都可以，主要是看使用的時候的 input type

```python
    def find_unique_user(self, identifier: Union[str, int]) -> dict[str, Any]:
        """
        get unique users from users table
        """
        assert self._cursor is not None
        if isinstance(identifier, str):
            query = """
            SELECT * FROM users
            WHERE username = ?
            LIMIT 1;
            """
        elif isinstance(identifier, int):
            query = """
            SELECT * FROM users
            WHERE id = ?
            LIMIT 1;
            """
        else:
            raise Exception('InvalidDevInputArgument')

        self._cursor.execute(query, (identifier,))
        user = self._cursor.fetchone()
        return user
```

這樣設定之後，就有一個可以創造新用戶和取出用戶的資料的 function 了！

最後在整個 file 的最下方貼上以下程式碼，就可以有一個單一的 class instance 給大家使用了。

```python
sql_client = SQL() # 最左邊不要有任何空格
```

## 5.2 Register

接著要來創造用戶，在`src/templates`中新增`register.html` ，

```html
{% extends "layout.html" %} {% block title %} Log In {% endblock %} {% block
main %}
<form action="/users/register" method="post">
  <div class="mb-3">
    <input
      autocomplete="off"
      autofocus
      class="form-control mx-auto w-auto"
      name="username"
      placeholder="Username"
      type="text"
    />
  </div>
  <div class="mb-3">
    <input
      class="form-control mx-auto w-auto"
      name="password"
      placeholder="Password"
      type="password"
    />
  </div>
  <div class="mb-3">
    <input
      class="form-control mx-auto w-auto"
      name="confirm_password"
      placeholder="Confirm Your Password"
      type="password"
    />
  </div>
  <button class="btn btn-primary" type="submit">Create Account</button>
</form>
<br />
<div>
  <a href="/users/login">Login</a>
</div>
{% endblock %}
```

接著到`src/app/routes`下面新增資料夾 `users`，往下新增 `users/__init__.py`和`users/route.py`

![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240911213401515.png>)

在 `users/route.py`中貼上：

```python
from flask import Blueprint

users = Blueprint("users", __name__)

from . import route
```

接著回到 `route/__init__.py`中註冊`users`這個 blueprint, 現在當 url 是 `/users`時就會走到`src/app/routes/users/routes.py` 裡面

```python
from flask import Blueprint
from src.app.routes.users import users # 加這一行

root = Blueprint("root", __name__)
root.register_blueprint(users, url_prefix="/users") # 加這一行

from . import route
```

接著貼入以下 function，在 `/users/register`的這個 api 中，如果是用 `GET`的方法進入，則會渲染上面設定的`html`，而如果是 post, 則會新增一個 user。

`request.form.get` 可以從前端 post 的 form 中取出資料，這裡會確認 `password`和`confirm_password` 是不是一樣

```python
from flask import request, render_template
from src.app.routes.users import users
from src.sql.sqlite import sql_client

@users.route("/register", methods=["GET", "POST"])
def register():
    """
    POST to submit register stuff
    GET to go to register page
    """
    if request.method == "POST":
        username = request.form.get("username")

        if not username:
            raise Except('NotProvideUserName')

        password = request.form.get("password")
        confirm_password = request.form.get("confirm_password")

        if not password or not confirm_password:
            raise Except('NotProvidePassword')

        if password != confirm_password:
            raise  Except('InvalidUserNameOrPassword')

        sql_client.create_user(username=username, password=password)
        return redirect("/users/login")

    else:
        return render_template("register.html")
```

我們可以啟動 `poetry run dev`之後，前往 `/users/register` 可以看到剛剛的頁面。可以實際狀個 user 看看
![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240911214631765.png>)

## 5.3 Login

接下來終於要實做登入功能了! 在登入時會需要使用 cookie session 機制，首先先安裝以下套電

```shell
poety add flask-session
```

接著回到 `src/app/__init__.py`，讓 app 可以使用 session, cookie session 可以把特定資訊放在 client 端。加入下面打上 `# 增加這行`的地方

```python
"""
init flask here, and set session and other config
"""
import sys
for path in sys.path:
    print(path, "\n")

from flask import Flask
from flask_session import Session # 增加這行
from src.libs.common import get_abs_path
from src.app.routes import root


# Setting up flask
template_path = get_abs_path("templates")
static_path = get_abs_path("static")
app = Flask(__file__, template_folder=template_path, static_folder=static_path)


# Configure session to use filesystem (instead of signed cookies)
app.config["SESSION_PERMANENT"] = False # 增加這行, 這邊也可以設成為True, session在關機之後還會保留,下次重開的時候還會在
app.config["SESSION_TYPE"] = "filesystem"# 增加這行

Session(app) # 增加這行

app.register_blueprint(root)


```

接著在 `src/templates`新增`login.html`

```html
{% extends "layout.html" %} {% block title %} Log In {% endblock %} {% block
main %}
<form action="/users/login" method="post">
  <div class="mb-3">
    <input
      autocomplete="off"
      autofocus
      class="form-control mx-auto w-auto"
      name="username"
      placeholder="Username"
      type="text"
    />
  </div>
  <div class="mb-3">
    <input
      class="form-control mx-auto w-auto"
      name="password"
      placeholder="Password"
      type="password"
    />
  </div>
  <button class="btn btn-primary" type="submit">Log In</button>
</form>
<br />
<div>
  <a href="/users/register">Register</a>
</div>
{% endblock %}
```

回到`src/app/routes/users/routes.py` 貼入下面的程式碼， 首先是 `/users/login`, 如果是`GET` 就會進入上面設定的 `html`, 如果是`POST`, 就先用`username`從資料庫裡面把`user`找出來然後使用 `check_password_hash` 確認用戶入的密碼 `hash`之後會不會我們存在 database 中 hash 的密碼是一樣的，如果一樣就在 `seesion`裡面放入`user_id`，就可以登入了！

而`logout` 則是單純把 session 清空，就當作 user 被網站登出

```python
from werkzeug.security import check_password_hash

@users.route("/login", methods=["GET", "POST"])
def login():
    """
    POST to submit login stuff
    GET to go to login page
    """
    if request.method == "POST":
        session.clear()

        username = request.form.get("username")

        if not username:
            raise Exception('NotProvideUserName')

        password = request.form.get("password")

        if not password:
            raise Exception('NotProvidePassword')

        user = sql_client.find_unique_user(username)

        if not user or not check_password_hash(
            hashed_password=user["hash"], password=password
        ):
            raise Exception('InvalidUserNameOrPassword')

        session["user_id"] = user["id"]

        return redirect("/")

    else:
        return render_template("login.html")


@users.route("/logout", methods=["GET"])
def logout():
    """
    removed session and logout
    """

    session.clear()

    return redirect("/users/login")
```

> login 畫面如下
> ![](<./W9/CS50%202024%20Spring%20Week%209%20Flask%20(%20Poetry,%20Python,%20Flask,%20SQLite)-20240911223122340.png>)

登入後會被跳轉到 `/`(home 頁面)，但是其實我們也可以直接進入 `/`，因此我們需要擋住未登入的用戶進入 home 頁面，這邊直接使用 flask 推薦的 [Login Required Decorator](https://flask.palletsprojects.com/en/latest/patterns/viewdecorators/)，建立新檔案`src/libs/decorator.py`並貼入以下程式碼。

```python
from functools import wraps
from flask import session, redirect
from src.sql.sqlite import sql_client


def login_required(f):
    """
    Decorate routes to require login.

    https://flask.palletsprojects.com/en/latest/patterns/viewdecorators/
    """

    @wraps(f)
    def decorated_function(*args, **kwargs):
        USER_LOGIN_URL = "/users/login"

        user_id = session.get("user_id")
        if not user_id or not isinstance(user_id, int):
            return redirect(USER_LOGIN_URL)

        user = sql_client.find_unique_user(user_id)

        if not user:
            return redirect(USER_LOGIN_URL)
        return f(*args, **kwargs)

    return decorated_function


```

接著來到 `src/app/routes/route.py`中，在 `@root.route("/")` 下面加入`@login_required`，這樣在進入 `/`的時候就會先檢查有沒有該 user(還有該 user 的 id 是不是存在資料庫)，只有存在 session 才可以正確進入 home, 不然就會被導回到 login

```python
from flask import render_template, session, redirect # 加session
from src.libs.decorator import login_required # 加這行 import

@root.route("/")
@login_required # 加這一行
def home_page():
    """
    home page for dash board
    """
    user_id = session.get("user_id") # 可以用 get從 session中拿東西出來用

    if not user_id:
        return redirect("/users/login")
```

這樣我們就是做完登入功能了！也謝謝看到這邊的你！
