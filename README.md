
�y�P�zWeb�R���e�i�iDjango + Apache + mod_wsgi�j��DB�R���e�i�iPostgreSQL�j�̍쐬�ƋN���܂ł̎菇

�@���O����
�@1.��Ɨp�}�V���Ɉȉ����C���X�g�[��

�@�iDocker�֘A�j
�@�@�@Windows10 Pro�̏ꍇ �FDocker Desktop for Windows�ADocker Compose
�@�@�@�@�@�@�@�@�@�@�@�@�@�@���}�V����Hyper-V��L���ɂ���

�@�@�@Windows10 HOME�̏ꍇ�FDocker Tools 
�@�@�@�@�@�@�@�@�@�@�@�@�@�@���C���X�g�[���̓r����VirtualBox���C���X�g�[������

�@�i�J�����j
�@�@�@Git
�@�@�@VisualStudioCode

�@2.Git�̃��[�N�X�y�[�X�쐬�ƃ��[�J�����|�W�g���̐ݒ�AVSCode�Ƃ̘A�g���m�F����

�@3.Git�̃����[�g���|�W�g������Pull����

�@�i�����[�g���|�W�g���j
�@�@�@https://github.com/KAZUNORI-SATO/docker_django_apache


�A�z�X�g�iWindows���j�ƃR���e�i�ŋ��L����t�H���_�̐ݒ�
�@�E�@�ŏ�������Git�̃��[�N�X�y�[�X��ݒ肷��
�@�@�i�t�H���_���ɂ�GitHUB����_�E�����[�h�����t�@�C���ꎮ���i�[����Ă��邱�Ɓj

�@�@�@Docker Desktop for Windows�̏ꍇ�@�FDocker��GUI�̐ݒ荀��

�@�@�@Docker Tools�̏ꍇ�@�FVirtualBox �ݒ荀�ڂ̋��L�t�H���_�̃p�X�ݒ�𐳂�������


�BDocker�̉��z�}�V���N��

�@�@�@Docker Desktop for Windows�̏ꍇ�@�FDocker for Windows���N���iMobyLinuxVM�j

�@�@�@Docker Tools�̏ꍇ�@�FVirtualBox ���N������
�@�@�@�@�@�@�@�@�@�@�@�@�@�@Docker Quickstart Terminal ���N������iVirtualBox�̉��z�}�V�����N������j


�C�e��Ԃ̊m�F�i����ȍ~�A�z�X�gOS�̃R�}���h�v�����v�g�Ŏ��s�j

��docker�̉��z�}�V�����̕\��
�@docker-machine ls

�@NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER     ERRORS
�@default   *        virtualbox   Running   tcp://192.168.99.100:2376           v19.03.5
                                            �@�@    �����z�}�V����IP�A�h���X���킩��

��docker�C���[�W�̈ꗗ���m�F�i����͉����\������Ȃ��j
�@docker images


�D�J�����g�f�B���N�g����Dockerfile������t�H���_�iGit�̃��[�N�X�y�[�X�̃t�H���_�j�ɂ���
�@cd c:\docker_django_apache


�E�R���e�i���r���h���ċN������
�@docker-compose up -d --build

��docker�C���[�W�̈ꗗ���m�F
�@docker images

�@REPOSITORY                            TAG                 IMAGE ID            CREATED             SIZE
�@docker_django_apache_django-apache2   latest              c094e9a10271        15 minutes ago      610MB
�@postgres                              11                  aa8042237034        7 days ago          283MB
�@ubuntu                                latest              4e5021d210f6        2 weeks ago         64.2MB

�@�@��docker_django_apache_django-apache2�́uDjango + Apache + mod_wsgi�v�R���e�i�̃C���[�W
�@�@��ubuntu�́uDjango + Apache + mod_wsgi�v�R���e�i�̌��R���e�i�̃C���[�W

���R���e�i�̈ꗗ���m�F
�@docker container ls [-a]�@

�@CONTAINER ID        IMAGE                                 COMMAND                  CREATED             STATUS              PORTS                  NAMES
�@56ea328775fd        docker_django_apache_django-apache2   "apache2ctl -D FOREG�c"   19 minutes ago      Up 19 minutes       0.0.0.0:8005->80/tcp   django-apache2
�@8787910a754e        postgres:11                           "docker-entrypoint.s�c"   19 minutes ago      Up 19 minutes       5432/tcp               docker_django_apache_db_1

�܂��́Adocker-compose ps

�@          Name                         Command              State          Ports
�@----------------------------------------------------------------------------------------
�@django-apache2              apache2ctl -D FOREGROUND        Up      0.0.0.0:8005->80/tcp
�@docker_django_apache_db_1   docker-entrypoint.sh postgres   Up      5432/tcp

�@�@��Web�R���e�i�idjango-apache2�j��Apache��80�|�[�g�ő҂��󂯁A�z�X�g������̓|�[�g�t�H���[�h��8005�ԃ|�[�g���g�p����
�@�@��docker_django_apache_db_1 ��PostgreSQL��DB�R���e�i


�y�Q�zDjango�̃A�v���P�[�V������DB�\�z�菇
�@�EWeb�R���e�i�ɂ�Django�̊�{�A�v���ł���Ǘ� (admin) �T�C�g�̊����z�u����Ă���A
�@�@DB�R���e�i�ɂ�PostgreSQL��Django�p�f�[�^�x�[�X���쐬����Ă���

�@�@�@DB���@�@�@: django_db
      ���[�U�[��: django_db_user
      �p�X���[�h: password1234

�@�@Django�̊�{�A�v���p�̃e�[�u�����f�[�^�x�[�X�ɍ쐬
�@docker-compose run --rm django-apache2 python3 /var/www/html/django_app/my_site/manage.py migrate

�@Starting docker_django_apache_db_1 ... done                                                                                                                                                                Operations to perform:
  �@Apply all migrations: admin, auth, contenttypes, sessions
�@Running migrations:
�@  Applying contenttypes.0001_initial... OK
�@  Applying auth.0001_initial... OK
�@  Applying admin.0001_initial... OK
�@  Applying admin.0002_logentry_remove_auto_add... OK
�@  Applying admin.0003_logentry_add_action_flag_choices... OK
�@  Applying contenttypes.0002_remove_content_type_name... OK
�@  Applying auth.0002_alter_permission_name_max_length... OK
�@  Applying auth.0003_alter_user_email_max_length... OK
�@  Applying auth.0004_alter_user_username_opts... OK
�@  Applying auth.0005_alter_user_last_login_null... OK
�@  Applying auth.0006_require_contenttypes_0002... OK
�@  Applying auth.0007_alter_validators_add_error_messages... OK
�@  Applying auth.0008_alter_user_username_max_length... OK
�@  Applying auth.0009_alter_user_last_name_max_length... OK
�@  Applying auth.0010_alter_group_name_max_length... OK
�@  Applying auth.0011_update_proxy_permissions... OK
�@  Applying sessions.0001_initial... OK


�@�A�Ǘ����[�U�[���쐬����
�@docker-compose run --rm django-apache2 python3 /var/www/html/django_app/my_site/manage.py createsuperuser

�@�i�ݒ��j
�@�@User:admin
�@�@Pass:adminadmin123


�@�B�u���E�U��http://192.168.99.101:8005/admin/
  �@�̊Ǘ� (admin) �T�C�g�ɃA�N�Z�X���A�A�Őݒ肵�����[�U�[�ƃp�X���[�h�Ń��O�C���\�ł��邱�Ƃ��m�F����


�y�R�z�Q�l
�@�Edocker�֘A�̃R�}���h��docker-compose�֘A�̃R�}���h

��docker�R�}���h��
���R���e�i�폜
�@docker container rm <�R���e�iID�̐擪�����ӂɎ��ʉ\�ȕ����܂�>

���^�O�Ȃ��C���[�W�̈ꊇ�폜�i���O�ɃR���e�i���폜���Ă����j
�@docker image prune

���C���[�W�̍폜
�@docker rmi [image ID]

���R���e�i��ŃR�}���h�����s����
 docker exec -it [�R���e�i��] ps auxf

���R���e�i�ɓ���Linux�V�F���R�}���h���s
 docker exec -it [�R���e�i��] bash


��docker-compose�R�}���h��
��Django�o�[�W�����̊m�F
�@docker-compose run --rm [�R���e�i��] python3 -m django --version

���A�v���P�[�V�����i�����ł�polls�A�v���j�̍\���쐬�i�A�v���������̂P��̂݁j
�@docker-compose run --rm [�R���e�i��] python3 manage.py startapp polls

��Django�ɃA�v���̃t�@�C���ɕύX�����������Ƃ�`���A�ύX���e���}�C�O���[�V�����̌`�ŕۑ�����
  docker-compose run --rm [�R���e�i��] python3 manage.py makemigrations polls

���f�[�^�x�[�X�̃}�C�O���[�V����
  docker-compose run --rm [�R���e�i��] python3 manage.py sqlmigrate polls 0001

���A�v���p�̃e�[�u�����f�[�^�x�[�X�ɍ쐬
�@docker-compose run --rm [�R���e�i��] python3 manage.py migrate

��Python�V�F�����N��
  docker-compose run --rm [�R���e�i��] python3 manage.py shell

��Django�ȈՃT�[�o�[�̋N��
�@docker-compose run --rm [�R���e�i��] python3 manage.py runserver

��Djangoadmin��{�A�v���Ŏg�p����Ă���css���擾����i�e�A�v���́u/static/�v���uSTATIC_ROOT�v�ɏW�߂܂��B�j
�i�A�v���������̂P��̂݁j
�@docker-compose run --rm [�R���e�i��] python3 manage.py collectstatic

���v���W�F�N�g�̍쐬�i����̎��s���̂݁j
�@docker-compose run --rm [�R���e�i��] django-admin startproject [�v���W�F�N�g��] . 

������̃R���e�i�N���i�ˑ��֌W�̃R���e�i���N���j
�@docker-compose up -d [�R���e�i��]

�@���u-d�v�̓o�b�N�O���E���h�ŋN��

��PostgreSQL�̊m�F�iDB�Ƀ��O�C���j
�@docker-compose exec [�R���e�i��] psql -U [���[�U�[��] [DB��]

�@�����[�U�[���ADB����docker-compose.yml�Œ�`�������̂��g�p

���N�����̃R���e�i�����ׂďI������
�@docker-compose down -v


