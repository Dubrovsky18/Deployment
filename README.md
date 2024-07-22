# Deploy APP

## Подготовка

Создайте виртуальное окружение

```sh
python3.12 -m venv venv
```

Активируйте виртуальное окружение

```sh
. ./venv/bin/activate
```

Установите ansible и другие необходимые для работы python пакеты в виртуальное окружение

```sh
pip install -r requirements.txt
```

Задеплоем старую версию

```sh
ansible-playbook -e "app_version=old" deploy.yml -t deploy
```

## Прописывание creds 

Для общего доступа я создал api ключи для взаимодействия с сервисом staytus.
Для отслеживания состояний процесса переходим сюда -> http://62.84.117.224:8787/

В файле roles/app/vars/main.yml 
Стоит указать все необходимые значения

```yml
staytus_api_key: 9de0c7a8-acab-4959-8e3a-1a7ffa6917fd
staytus_api_secret: YhwihiteAr4tKjClRGy6PwFFu3dgoH
service_name: "web-application" 
maintenance_status: "maintenance" 
operational_status: "operational"
server_staytus: 62.84.117.224
staytus_url: http://{{ server_staytus }}:8787/api/v1
```

Если нужно подкорректировать сведения о машине, где это будет запускаться, тогда стоит подредактировать inventory.yml
```yml
---
all:
  hosts:
    server:
      ansible_connection: "local"
```

Если вам необходимо помимо деплоя создать инфраструктуру из `Staytus` тогда стоит запустить 

```sh
ansible-playbook deploy.yml -t infra
```

Если мы начинаем деплоить наш сервис, тогда 

```sh
ansible-playbook -e "app_version=new" deploy.yml -t deploy
``` 
Как только мы успешно 

Обязательно передать через опцию версию приложения, которую хотим задеплоить.
Если требуется пароль на sudo, то следует добавить ключ `--ask-become-pass` для ввода пароля через консоль.