# WordPress + MySQL Master-Slave Deployment via Ansible  
### Автор: Eduard Bodreev

## Описание

Проект разворачивает мультиконтейнерное приложение с WordPress и MySQL (в режиме Master-Slave репликации) с помощью Ansible и Docker:  
Также реализованы тестовые сценарии проверки репликации и очистка данных после теста.

Все действия выполняются на удалённом сервере с доступом по SSH.

---

## Требования

- Установленный Ansible (на вашей машине)
- SSH-доступ к серверу с root-доступом или sudo без пароля
- Docker и docker-compose на удалённом сервере (автоматически устанавливаются)
- Настроенный `inventory.ini` с IP-адресом сервера

---

## Содержимое

- `playbook2.yml` - основной плейбук: развертывание MySQL Master-Slave и WordPress
- `playbook2test.yml` - тест репликации: создание БД и таблицы, вставка и проверка данных
- `playbook2aftertest.yml` - удаление тестовых данных и проверка синхронизации
- `inventory.ini` - список серверов
- `docker-compose.yml` - генерируется автоматически на сервере

---

## Установка и запуск

### 1. Клонировать репозиторий
```bash
git clone https://github.com/Eduard-Bodreev/IFMO_DistributedComputing_for_DevOps.git
cd IFMO_DistributedComputing_for_DevOps
git checkout HW2
```
### 2. Указать сервер в `inventory.ini`

```ini
[wordpress_server]
<IP_сервера> ansible_user=<пользователь>
```

### 3. Запустить основной плейбук

```bash
ansible-playbook -i inventory.ini playbook2.yml
```

### 4. Проверить корректность репликации

```bash
ansible-playbook -i inventory.ini playbook2test.yml
```

Плейбук создаёт таблицу, вставляет данные и проверяет, что данные из мастера появились в реплике даже после выключения мастера.


### 5. Удалить тестовые записи и убедиться, что они исчезли и на реплике

```bash
ansible-playbook -i inventory.ini playbook2aftertest.yml
```