# prometheus_bot for Telegram

Данный бот является допилинной версией бота [inCaller_prometheus_bot](https://github.com/inCaller/prometheus_bot)

## Сборка

```bash
git clone https://github.com/inCaller/prometheus_bot.git
cd prometheus_bot
go build
```

или можем использовать готовый бинарник с последней версией сборки:

```bash
git clone https://github.com/inCaller/prometheus_bot.git
cd prometheus_bot
./prometheus_bot
```


## Использование

1. Создайте бота [BotFather](https://t.me/BotFather), он вернет вам токен

2. Укажите токен бота и файл с шаблоном оповещений в файле ```config.yaml```:

```yml
telegram_token: "TOKEN GOES HERE"
# ONLY IF YOU USING TEMPLATE required for test

template_path: "template.tmpl" 
time_zone: "Europe/Moscow"
split_token: "|"    

# ONLY IF YOU USING DATA FORMATTING FUNCTION, NOTE for developer: important or test fail
time_outdata: "02/01/2006 15:04:05" 
split_msg_byte: 4000
```

3. Получите ID чата одним из двух способов:
    1. Если бота используете только вы, то просто напишите ему любое сообщение и он вернёт ID.
    2. Если бот будет использоваться в группе, то добавьте его в группу. Для получения ID напишите любое сообщение, [начинающееся со знака `/`](https://core.telegram.org/bots#privacy-mode)


4. Откройте конфиг Alertmanager-а(например: /etc/alertmanager/alertmanager.yml) и укажите в разделе ```receivers:```:

```yml
- name: prometheus_bot
  webhook_configs:
  - send_resolved: True
    url: http://127.0.0.1:9087/alert/-chat_id
```

где вместо ```-chat_id``` необходимо указать ID чата, если ID начинается с ```-``` то его нужно указать с дефисом, например: ```url: http://127.0.0.1:9087/alert/-5858$


5. Отредактируйте и скопируйте файл prometheus_bot.service. Для просмотра ключей запуска используйте: ```prometheus_bot --help```:

```bash
cp prometheus_bot.servic /etc/systemd/system/prometheus_bot.service
systemctl daemon-reload
```

6. Запустите бота:

```bash
systemctl start prometheus_bot
systemctl status prometheus_bot
```

## Формат сообщения

Формат сообщений, отправляемых в Telegram можно задать в шаблоне production_example.tmpl

