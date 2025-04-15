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

## 💾 Хранение данных

MariaDB будет сохранять все данные в **`/var/lib/db`** на хосте.

Создай каталог и выдай права:

```bash
sudo mkdir -p /var/lib/db
sudo chown -R 999:999 /var/lib/db
```

---

## 📦 Сборка и запуск

```bash
cd /path/to/project
sudo docker-compose up --build
```

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
