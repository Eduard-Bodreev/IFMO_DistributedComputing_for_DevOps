
---

# HW3: Monitoring Docker Cluster with Grafana, Prometheus, cAdvisor

### Автор: Eduard Bodreev

## Описание

Этот проект автоматически разворачивает систему мониторинга Docker-контейнеров с помощью Ansible, Docker, Prometheus, Grafana и cAdvisor.
После выполнения playbook'а:

* Grafana доступна по порту `3000`
* Prometheus — на `9090`
* cAdvisor — на `8180`
* Автоматически добавляется Grafana-дэшборд и подключается источник Prometheus
* Создаётся алерт при превышении CPU > 2.5% на контейнере `db-master`

---

## Требования

* Установленный Ansible (на вашей локальной машине)
* SSH-доступ к удалённому серверу
* Docker уже установлен или будет установлен playbook'ом

---

1. Склонировать репозиторий:

   ```bash
   git clone https://github.com/Eduard-Bodreev/IFMO_DistributedComputing_for_DevOps
   ```

2. Отредактировать `inventory.ini`, указав IP-адрес и пользователя удалённого сервера:

   ```
   [all]
   158.160.137.228 ansible_user=ubuntu
   ```

3. Запустить playbook:

   ```bash
   ansible-playbook -i inventory.ini playbook3.yml
   ```

---
* Зайти на `http://<IP>:3000`
* Логин/пароль: `admin / adminpass`
* Перейти в Dashboards → Docker monitoring
* Убедиться, что отображаются метрики CPU, Memory, Network

### Алерты:

* Зайти в `Alerting > Alert rules`
* Убедиться, что есть правило: **CPU > 2.5%**
* Состояние можно проверить вручную, нагружая контейнер