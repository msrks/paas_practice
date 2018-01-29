## Flask+Postgresql on Heroku

#### install postgresql on mac

```bash
$ brew update
$ brew install postgresql
$ brew services start postgresql
$ brew services list
```

#### postgresql の動作確認

```bash
$ createdb testdb
$ psql -l
$ dropdb testdb
$ psql -l
```

#### flask + postgresql

app.py

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
import os

app = Flask(__name__)

app.config['SQLALCHEMY_DATABASE_URI'] = os.environ.get( "DATABASE_URL",
                                         "postgresql://localhost/<local-db-name>")
..
..
```

heroku で postgresの addonを使うと、DATABASE_URL環境変数に URLがセットされるので、上のように書くといい。

### ローカルでの動作確認

```
$ python
>>> from app import db
>>> db.create_all()
$ gunicorn app:app
```

### herokuへのデプロイ

```bash
$ git init
$ echo web: gunicorn app:app --log-file=- > Procfile
$ echo python-3.6.2 > runtime.txt
$ git add -A
$ git commit -m "init commit"
$ heroku create <heroku-app-name>
$ heroku addons:add heroku-postgresql
$ git push heroku master
$ heroku run python
>>> from app import db
>>> db.create_all()
$ heroku open
```
