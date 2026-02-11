#Infrastructure Acess & Monitoring Gateway

> **Automated Infrastructure as Code (IaC) solution for secure remote access and monitoring.**

![Ansible](https://img.shields.io/badge/Ansible-E00-red?style=flat&logo=ansible)
![Docker](https://img.shields.io/badge/Docker-2496ED-blue?style=flat&logo=docker)
![Nginx](https://img.shields.io/badge/Nginx-009639-green?style=flat&logo=nginx)
![License](https://img.shields.io/badge/License-MIT-green)

## О проекте

Этот репозиторий содержит инструменты автоматизации для развертывания защищенного шлюза доступа к инфраструктуре (на базе **Xray/Marzban**) и системы мониторинга (**Grafana**). 

Проект реализует принцип **"One-Click Deployment"**: полная настройка "голого" сервера (Ubuntu/Debian) выполняется одной командой с использованием **Ansible** и **Makefile**.

### Ключевые возможности:

* **Security First:** Автоматическая генерация криптографических ключей `x25519` и `ShortId` для протокола VLESS Reality прямо в процессе деплоя. Ключи не хранятся в репозитории.
* **Reverse Proxy & SSL:** Настройка **Nginx** в качестве фронтенда с автоматическим выпуском и обновлением сертификатов Let's Encrypt.
* **Monitoring:** Автоматическое развертывание **Grafana** в Docker-контейнере.
* **Dynamic Configuration:** Использование Jinja2 шаблонов для подстановки доменов, ключей и email-адресов.



## Инструкция по установке


### 1. Подготовка (что вам нужно)
* **Сервер:** Купленный VPS с чистой ОС Ubuntu (рекомендуется 24.04).
* **Домен:** Направьте ваш домен (или бесплатный поддомен DuckDNS) на IP вашего сервера.

### 2. Установка одной командой
Подключитесь к вашему серверу через терминал (SSH) и вставьте следующую команду:

```bash
apt update && apt install -y git make ansible && \
git clone https://github.com/chikamint/infra-gateway-automation.git && \
cd infra-gateway-automation && \
make install-local
```
Для установки удалённо используйте:
```bash
apt update && apt install -y git make ansible && \
git clone https://github.com/chikamint/infra-gateway-automation.git && \
cd infra-gateway-automation && \
make init
make setup
```


После установки обязательно нужно создать учётную запись с помощью команды

```bash
marzban cli admin create --sudo
```
 
---

## Технический стек

| Компонент | Роль в системе |
|-----------|----------------|
| **Ansible** | Оркестрация, управление конфигурациями, генерация ключей |
| **Docker & Compose** | Контейнеризация сервисов (Marzban, Grafana) |
| **Nginx** | Reverse Proxy, SSL Termination |
| **Xray-core** | Ядро шлюза (VLESS + Reality + TCP) |
| **Make** | Интерактивный интерфейс (Wizard) для запуска |

---

## Структура проекта
```text
.
├── Makefile                # Интерактивный мастер установки
├── marzban_fixed.sh        # Скрипт установки Marzban
├── ansible/
│   ├── deploy.yml          # Основной плейбук
│   └── templates/
│       ├── nginx.conf.j2       # Шаблон конфига Nginx
│       └── xray_config.json.j2 # Шаблон конфига Xray
```
