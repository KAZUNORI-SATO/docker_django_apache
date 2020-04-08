
【１】Webコンテナ（Django + Apache + mod_wsgi）とDBコンテナ（PostgreSQL）の作成と起動までの手順

①事前準備
　1.作業用マシンに以下をインストール

　（Docker関連）
　　　Windows10 Proの場合 ：Docker Desktop for Windows、Docker Compose
　　　　　　　　　　　　　　※マシンのHyper-Vを有効にする

　　　Windows10 HOMEの場合：Docker Tools 
　　　　　　　　　　　　　　※インストールの途中でVirtualBoxもインストールする

　（開発環境）
　　　Git
　　　VisualStudioCode

　2.Gitのワークスペース作成とローカルリポジトリの設定、VSCodeとの連携を確認する

　3.GitのリモートリポジトリからPullする

　（リモートリポジトリ）
　　　https://github.com/KAZUNORI-SATO/docker_django_apache


②ホスト（Windows側）とコンテナで共有するフォルダの設定
　・①で準備したGitのワークスペースを設定する
　　（フォルダ内にはGitHUBからダウンロードしたファイル一式が格納されていること）

　　　Docker Desktop for Windowsの場合　：DockerのGUIの設定項目

　　　Docker Toolsの場合　：VirtualBox 設定項目の共有フォルダのパス設定を正しくする


③Dockerの仮想マシン起動

　　　Docker Desktop for Windowsの場合　：Docker for Windowsを起動（MobyLinuxVM）

　　　Docker Toolsの場合　：VirtualBox を起動する
　　　　　　　　　　　　　　Docker Quickstart Terminal を起動する（VirtualBoxの仮想マシンが起動する）


④各状態の確認（これ以降、ホストOSのコマンドプロンプトで実行）

■dockerの仮想マシン情報の表示
　docker-machine ls

　NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER     ERRORS
　default   *        virtualbox   Running   tcp://192.168.99.100:2376           v19.03.5
                                            　　    ★仮想マシンのIPアドレスがわかる

■dockerイメージの一覧を確認（初回は何も表示されない）
　docker images


⑤カレントディレクトリをDockerfileがあるフォルダ（Gitのワークスペースのフォルダ）にする
　cd c:\docker_django_apache


⑥コンテナをビルドして起動する
　docker-compose up -d --build

■dockerイメージの一覧を確認
　docker images

　REPOSITORY                            TAG                 IMAGE ID            CREATED             SIZE
　docker_django_apache_django-apache2   latest              c094e9a10271        15 minutes ago      610MB
　postgres                              11                  aa8042237034        7 days ago          283MB
　ubuntu                                latest              4e5021d210f6        2 weeks ago         64.2MB

　　※docker_django_apache_django-apache2は「Django + Apache + mod_wsgi」コンテナのイメージ
　　※ubuntuは「Django + Apache + mod_wsgi」コンテナの元コンテナのイメージ

■コンテナの一覧を確認
　docker container ls [-a]　

　CONTAINER ID        IMAGE                                 COMMAND                  CREATED             STATUS              PORTS                  NAMES
　56ea328775fd        docker_django_apache_django-apache2   "apache2ctl -D FOREG…"   19 minutes ago      Up 19 minutes       0.0.0.0:8005->80/tcp   django-apache2
　8787910a754e        postgres:11                           "docker-entrypoint.s…"   19 minutes ago      Up 19 minutes       5432/tcp               docker_django_apache_db_1

または、docker-compose ps

　          Name                         Command              State          Ports
　----------------------------------------------------------------------------------------
　django-apache2              apache2ctl -D FOREGROUND        Up      0.0.0.0:8005->80/tcp
　docker_django_apache_db_1   docker-entrypoint.sh postgres   Up      5432/tcp

　　※Webコンテナ（django-apache2）はApacheの80ポートで待ち受け、ホスト側からはポートフォワードで8005番ポートを使用する
　　※docker_django_apache_db_1 はPostgreSQLのDBコンテナ


【２】DjangoのアプリケーションとDB構築手順
　・WebコンテナにはDjangoの基本アプリである管理 (admin) サイトの環境が配置されており、
　　DBコンテナにはPostgreSQLのDjango用データベースが作成されている

　　　DB名　　　: django_db
      ユーザー名: django_db_user
      パスワード: password1234

　①Djangoの基本アプリ用のテーブルをデータベースに作成
　docker-compose run --rm django-apache2 python3 /var/www/html/django_app/my_site/manage.py migrate

　Starting docker_django_apache_db_1 ... done                                                                                                                                                                Operations to perform:
  　Apply all migrations: admin, auth, contenttypes, sessions
　Running migrations:
　  Applying contenttypes.0001_initial... OK
　  Applying auth.0001_initial... OK
　  Applying admin.0001_initial... OK
　  Applying admin.0002_logentry_remove_auto_add... OK
　  Applying admin.0003_logentry_add_action_flag_choices... OK
　  Applying contenttypes.0002_remove_content_type_name... OK
　  Applying auth.0002_alter_permission_name_max_length... OK
　  Applying auth.0003_alter_user_email_max_length... OK
　  Applying auth.0004_alter_user_username_opts... OK
　  Applying auth.0005_alter_user_last_login_null... OK
　  Applying auth.0006_require_contenttypes_0002... OK
　  Applying auth.0007_alter_validators_add_error_messages... OK
　  Applying auth.0008_alter_user_username_max_length... OK
　  Applying auth.0009_alter_user_last_name_max_length... OK
　  Applying auth.0010_alter_group_name_max_length... OK
　  Applying auth.0011_update_proxy_permissions... OK
　  Applying sessions.0001_initial... OK


　②管理ユーザーを作成する
　docker-compose run --rm django-apache2 python3 /var/www/html/django_app/my_site/manage.py createsuperuser

　（設定例）
　　User:admin
　　Pass:adminadmin123


　③ブラウザでhttp://192.168.99.101:8005/admin/
  　の管理 (admin) サイトにアクセスし、②で設定したユーザーとパスワードでログイン可能であることを確認する


【３】参考
　・docker関連のコマンドとdocker-compose関連のコマンド

＜dockerコマンド＞
■コンテナ削除
　docker container rm <コンテナIDの先頭から一意に識別可能な部分まで>

■タグなしイメージの一括削除（事前にコンテナを削除しておく）
　docker image prune

■イメージの削除
　docker rmi [image ID]

■コンテナ上でコマンドを実行する
 docker exec -it [コンテナ名] ps auxf

■コンテナに入りLinuxシェルコマンド実行
 docker exec -it [コンテナ名] bash


＜docker-composeコマンド＞
■Djangoバージョンの確認
　docker-compose run --rm [コンテナ名] python3 -m django --version

■アプリケーション（ここではpollsアプリ）の構造作成（アプリ生成時の１回のみ）
　docker-compose run --rm [コンテナ名] python3 manage.py startapp polls

■Djangoにアプリのファイルに変更があったことを伝え、変更内容をマイグレーションの形で保存する
  docker-compose run --rm [コンテナ名] python3 manage.py makemigrations polls

■データベースのマイグレーション
  docker-compose run --rm [コンテナ名] python3 manage.py sqlmigrate polls 0001

■アプリ用のテーブルをデータベースに作成
　docker-compose run --rm [コンテナ名] python3 manage.py migrate

■Pythonシェルを起動
  docker-compose run --rm [コンテナ名] python3 manage.py shell

■Django簡易サーバーの起動
　docker-compose run --rm [コンテナ名] python3 manage.py runserver

■Djangoadmin基本アプリで使用されているcssを取得する（各アプリの「/static/」を「STATIC_ROOT」に集めます。）
（アプリ生成時の１回のみ）
　docker-compose run --rm [コンテナ名] python3 manage.py collectstatic

■プロジェクトの作成（初回の実行時のみ）
　docker-compose run --rm [コンテナ名] django-admin startproject [プロジェクト名] . 

■特定のコンテナ起動（依存関係のコンテナも起動）
　docker-compose up -d [コンテナ名]

　※「-d」はバックグラウンドで起動

■PostgreSQLの確認（DBにログイン）
　docker-compose exec [コンテナ名] psql -U [ユーザー名] [DB名]

　★ユーザー名、DB名はdocker-compose.ymlで定義したものを使用

■起動中のコンテナをすべて終了する
　docker-compose down -v


