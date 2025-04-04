# SMTP to Telegram

[![Docker Hub](https://img.shields.io/docker/pulls/kostyaesmukov/smtp_to_telegram.svg?style=flat-square)][Docker Hub]
[![Go Report Card](https://goreportcard.com/badge/github.com/KostyaEsmukov/smtp_to_telegram?style=flat-square)][Go Report Card]
[![License](https://img.shields.io/github/license/KostyaEsmukov/smtp_to_telegram.svg?style=flat-square)][License]

[Docker Hub]:      https://hub.docker.com/r/kostyaesmukov/smtp_to_telegram
[Go Report Card]:  https://goreportcard.com/report/github.com/KostyaEsmukov/smtp_to_telegram
[License]:         https://github.com/KostyaEsmukov/smtp_to_telegram/blob/main/LICENSE

`smtp_to_telegram` is a simple program that listens for SMTP and forwards
all incoming Email messages to Telegram.

Say you have a software that can send Email notifications via SMTP.
You can use `smtp_to_telegram` as an SMTP server so
the notification mail would be sent to the chosen Telegram chats.

## Getting started

1. Create a new Telegram bot: https://core.telegram.org/bots#creating-a-new-bot.
2. Open that bot account in the Telegram account that should receive
   the messages, press `/start`.
3. Retrieve a chat id with `curl https://api.telegram.org/bot<BOT_TOKEN>/getUpdates`.
4. Repeat steps 2 and 3 for each Telegram account that should receive the messages.
5. Start a docker container:

```
docker run \
    --name smtp_to_telegram \
    -e ST_TELEGRAM_CHAT_IDS=<CHAT_ID1>,<CHAT_ID2> \
    -e ST_TELEGRAM_BOT_TOKEN=<BOT_TOKEN> \
    kostyaesmukov/smtp_to_telegram
```

Assuming that your Email-sending software is running in docker as well,
you can use `smtp_to_telegram:2525` as the target SMTP address.
No TLS or authentication is required.

The default Telegram message format is:

```
From: {from}\\nTo: {to}\\nSubject: {subject}\\n\\n{body}\\n\\n{attachments_details}
```

A custom format can be specified as well:

```
docker run \
    --name smtp_to_telegram \
    -e ST_TELEGRAM_CHAT_IDS=<CHAT_ID1>,<CHAT_ID2> \
    -e ST_TELEGRAM_BOT_TOKEN=<BOT_TOKEN> \
    -e ST_TELEGRAM_MESSAGE_TEMPLATE="Subject: {subject}\\n\\n{body}" \
    kostyaesmukov/smtp_to_telegram
```
