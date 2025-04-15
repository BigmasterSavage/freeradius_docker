# FreeRADIUS + MariaDB + daloRADIUS в Docker

Этот проект разворачивает полную систему авторизации на базе **FreeRADIUS**, с веб-интерфейсом **daloRADIUS** и СУБД **MariaDB** с инициализацией схем.

---

## 🧱 Структура

```
project/
├── docker-compose.yml
├── Dockerfile.apache
├── Dockerfile.freeradius
├── Dockerfile.mariadb
└── README.md
```

---

## 🚀 Установка

Подходит для **Debian 11** и выше.

### 1. Установи Docker

```bash
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl gnupg lsb-release

curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo   "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg]   https://download.docker.com/linux/debian   $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

### 2. Установи Docker Compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.6/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

---

## 💾 Хранение данных

MariaDB будет сохранять все данные в **`/var/lib/db`** на хосте.

Создай каталог и выдай права:

```bash
sudo mkdir -p /var/lib/db
sudo chown -R 999:999 /var/lib/db
```

---

## 📦 Сборка и запуск

Перейди в каталог с проектом:

```bash
cd /path/to/project
sudo docker-compose up --build
```

---

## 🔍 Сервисы и доступ

| Сервис       | URL/Порт               | Описание                       |
|--------------|------------------------|--------------------------------|
| MariaDB      | `localhost:3306`       | База данных `radius`           |
| FreeRADIUS   | `1812/udp`, `1813/udp` | Порты RADIUS                   |
| daloRADIUS   | `http://localhost/`    | Веб-интерфейс пользователей    |
| daloRADIUS   | `http://localhost:8000/` | Интерфейс операторов         |

---

## 📂 Доступы к БД

- **root**: `12345`
- **radius**: `12345`
- **База**: `radius`

---

## ❌ Остановка

```bash
sudo docker-compose down
```

Если хочешь удалить все тома и данные:

```bash
sudo docker-compose down -v
```

---

## ✅ Проверка FreeRADIUS

Пример теста изнутри контейнера FreeRADIUS:

```bash
docker exec -it freeradius radtest testuser testpass localhost 0 testing123
```

---

## 🛠 Примечания

- Все образы собираются **локально** из предоставленных `Dockerfile`.
- daloRADIUS автоматически подключается к базе и готов к использованию после запуска.
- SQL-схемы автоматически загружаются в MariaDB при первом запуске.

---

Happy RADIUS-ing! 😊