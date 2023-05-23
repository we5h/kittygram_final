#  Проект «Kittygram»

## Описание проекта:

- #### Проект Kittygram позволяет пользователям поделиться своим пушистым счастьем и рассказать о его достижениях!
---

### *Собаки обходят стороной и ищут свой проект!*
<div id="header" align="left">  <img src="https://99px.ru/sstorage/86/2022/08/image_861608220218568351111.gif" width="200"/> </div>

---
## У нас вы можете:
- #### Зарегистрироваться.
- #### Добавить кота!
- #### Убрать (своего) кота!
- #### Подобрать цвет, на который его окрас больше всего походит!
- #### Не забудьте счастливую фотографию любимца!
- #### Рассказть всем чем прославился ваш кот!
- #### Указать возраст любимца.
---
## Как запустить проект:

``` Настраиваем Docker
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin;
# Переходим в директорию проекта.
cd kittygram_final/
# Создаем и редактируем файл .env, в котором нужно указать данные
sudo nano .env
# 
DJANGO_KEY=django-insecure-cg6*%6d51ef8f#4!r3*$vmxm4)abgjw8mo!4y-q*uq1!4$-88$
POSTGRES_DB=<Желаемое_имя_базы_данных>
POSTGRES_USER=<Желаемое_имя_пользователя_базы_данных>
POSTGRES_PASSWORD=<Желаемый_пароль_пользователя_базы_данных>
DB_HOST=db
DB_PORT=5432
# Сохраняем файл. Ключ DJANGO_KEY рекомендуется заменить
# Далее выполняем последовательно
sudo docker compose -f docker-compose.production.yml pull
sudo docker compose -f docker-compose.production.yml down
sudo docker compose -f docker-compose.production.yml up -d
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collect_static/. /static_backend/static/
# Создаем суперпользователся. Следуем инструкциям при выполнении.
sudo docker compose -f docker-compose.production.yml exec backend python manage.py createsuperuser

# Генерируем новый секретный ключ Django
sudo docker compose -f docker-compose.production.yml exec backend python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'
# В ответ будет примерно такая строка: cg6*%6d51ef8f#4!r3*$vmxm4)abgjw8mo!4y-q*uq1!4$-88$
# Вставляем в .env файл. Должно получиться примерно такое:
DJANGO_KEY=django-insecure-cg6*%6d51ef8f#4!r3*$vmxm4)abgjw8mo!4y-q*uq1!4$-88$
```

Устанавливаем и настраиваем NGINX

```bash
# Устанавливаем NGINX
sudo apt install nginx -y
# Запускаем
sudo systemctl start nginx
# Настраиваем firewall
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
# Включаем firewall
sudo ufw enable
# Открываем конфигурационный файл NGINX
sudo nano /etc/nginx/sites-enabled/default
# Полностью удаляем из него все и пишем новые настройки

server {
    listen 80;
    server_name example.com;
    
    location / {
        proxy_set_header HOST $host;
        proxy_pass http://127.0.0.1:9000;

    }
}

# Сохраняем изменения и выходим из редактора
# Проверяем корректность настроек
sudo nginx -t
# Запускаем NGINX
sudo systemctl start nginx
```
Настраиваем HTTPS

```bash
# Установка пакетного менеджера snap.
# У этого пакетного менеджера есть нужный вам пакет — certbot.
sudo apt install snapd
# Установка и обновление зависимостей для пакетного менеджера snap.
sudo snap install core; sudo snap refresh core
# При успешной установке зависимостей в терминале выведется:
# core 16-2.58.2 from Canonical✓ installed 

# Установка пакета certbot.
sudo snap install --classic certbot
# При успешной установке пакета в терминале выведется:
# certbot 2.3.0 from Certbot Project (certbot-eff✓) installed

# Создание ссылки на certbot в системной директории,
# чтобы у пользователя с правами администратора был доступ к этому пакету.
sudo ln -s /snap/bin/certbot /usr/bin/certbot

# Получаем сертификат и настраиваем NGINX следуя инструкциям
sudo certbot --nginx
# Перезапускаем NGINX
sudo systemctl reload nginx
```

---
## Попробовать демо-версию:
* [Kittygram](https://wkittygram.ddns.net)
## В проекте были использованы технологии:
* #### Django REST
* #### Python 3.9
* #### Gunicorn
* #### Nginx
* #### JS
* #### Node.js
* #### PostgreSQL
* #### Docker
---
### Над проектом работал:
* [Dmitrij Gribkov(Backend)](https://github.com/we5h)
