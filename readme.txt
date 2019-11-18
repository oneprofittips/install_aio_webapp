Первым делом  создадим для приложения папку для приложения

Создаим вирт окружение
pip install virtualenv
python3 -m venv env
активация source env/bin/activate
деактивация deactivate

pip install uvloop

Установка gunicorn
pip install gunicorn
Пробный запуск 
1. Создаем конфиг файл gunicorn.conf.py 
2. Строка запуска 
  1.1 gunicorn main_api:my_web_app --bind localhost:8080 --worker-class aiohttp.GunicornUVLoopWebWorker --workers 2
  или с помощью конфиг файла
  1.2 gunicorn main_api:my_web_app -c gunicorn.conf.py

Установка Nginx 
apt install nginx
Создадим в папке /etc/nginx/sites-available/   файл для сайта 
и впишем туда конфигурацию 

server {
    listen 80;
    server_name 111.222.333.44; # здесь прописать или IP-адрес или доменное имя сервера
    access_log  /var/log/nginx/example.log;
 
    location /static/ {
        root /home/user/myprojectenv/myproject/myproject/;
        expires 30d;
    }
 
    location / {
        proxy_pass http://127.0.0.1:8000; 
        proxy_set_header Host $server_name;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

Создадим  ссылку 
ln -s /etc/nginx/sites-available/<ИМЯ ФАЙЛА> /etc/nginx/sites-enabled/

Проверка на синтаксические ошибки
nginx -t

Команды управления Nginx
systemctl stop nginx
systemctl restart nginx
systemctl start nginx
systemctl reload nginx
systemctl disable nginx
systemctl enable nginx

Настройка супервизора
apt-get install supervisor

В файле cd /etc/supervisor/supervisord.conf  добавим в конец
[program:app]
command=/root/app/appenv/bin/gunicorn app:inst_app --bind 0.0.0.0:5000 --worker-class aiohttp.GunicornUVLoopWebWorker --workers=4
directory=/root/app
user=root
autorestart=true
redirect_stderr=true

Включим супервизор
update-rc.d supervisor enable

Запустим
service supervisor start
/etc/init.d/supervisor restart
Основные команды супервизора
supervisorctl reread
supervisorctl update
supervisorctl status <имя файла конф>
supervisor restart <имя файла конф>

Убирть процесс killall gunicorn


PG
apt-get install postgresql
su - postgres
psql

CREATE DATABASE yourdbname;
CREATE USER youruser WITH ENCRYPTED PASSWORD 'yourpass';
GRANT ALL PRIVILEGES ON DATABASE yourdbname TO youruser;
