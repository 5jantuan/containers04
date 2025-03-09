# Использование контейнеров как среды выполнения
## Цель работы
Данная лабораторная работа призвана напомнить основные команды ОС Debian/Ubuntu. Также она позволит познакомиться с Docker и его основными командами.

## Задание
Запустить контейнер Ubuntu, установить Web-сервер Apache и вывести в браузере страницу с текстом "Hello, World!".

## Подготовка
Для выполнения данной работы необходимо иметь установленный на компьютере Docker.

## Шаг первый
Создаем репозиторий containers04 и клонируем его себе на компьютер.
  

### **1. Запуск контейнера с Ubuntu**
```bash
docker run -ti -p 8000:80 --name containers04 ubuntu bash
```
**Что делает команда?**  
- `docker run` — запускает новый контейнер.  
- `-ti` — означает, что мы запускаем контейнер в интерактивном режиме (`-t` — терминал, `-i` — ввод/вывод).  
- `-p 8000:80` — пробрасывает порт 8000 на хосте в порт 80 контейнера (где будет работать Apache).  
- `--name containers04` — даёт контейнеру имя `containers04`.  
- `ubuntu` — используемый базовый образ (`Ubuntu`).  
- `bash` — запуск командного интерпретатора Bash внутри контейнера.  

**Вывод в консоли:**  
После запуска контейнера появится приглашение командной строки, что означает, что мы вошли внутрь контейнера:
```bash
root@<container_id>:/#
```

---

### **2. Обновление списка пакетов**
```bash
apt update
```
**Что делает команда?**  
- Обновляет информацию о доступных пакетах и их версиях в `Ubuntu`.  

**Вывод в консоли:**  
Будет загружена информация о доступных обновлениях, примерно так:
```bash
Hit:1 http://archive.ubuntu.com/ubuntu focal InRelease
Get:2 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]
...
Reading package lists... Done
```

---

### **3. Установка Apache2**
```bash
apt install apache2 -y
```
**Что делает команда?**  
- Устанавливает веб-сервер Apache (`apache2`).  
- Флаг `-y` автоматически отвечает «да» на запросы подтверждения установки.  

**Вывод в консоли:**  
После загрузки и установки пакетов будет выведено:
```bash
...
Setting up apache2 (2.4.41-4ubuntu3.17) ...
Created symlink /etc/systemd/system/multi-user.target.wants/apache2.service → /lib/systemd/system/apache2.service.
...
```

---

### **4. Запуск Apache**
```bash
service apache2 start
```
**Что делает команда?**  
- Запускает веб-сервер Apache в контейнере.  

**Вывод в консоли:**  
```bash
service apache2 status
```
Видим примерно такое:
```bash
* apache2 is running
```

---

### **5. Проверка работы Apache**
Открываем браузер и вводим:
```
http://localhost:8000
```
**Что увидим?**  
Отобразиться стандартная страница Apache:  
*«Apache2 Ubuntu Default Page»*

---

### **6. Проверка содержимого веб-директории**
```bash
ls -l /var/www/html/
```
**Что делает команда?**  
- Показывает список файлов в каталоге `/var/www/html/`, который используется Apache для размещения веб-страниц.  

**Вывод в консоли:**  
```bash
-rw-r--r-- 1 root root 10701 Mar 09  2025 index.html
```

---

### **7. Замена содержимого `index.html`**
```bash
echo '<h1>Hello, World!</h1>' > /var/www/html/index.html
```
**Что делает команда?**  
- Заменяет содержимое файла `index.html` на простой HTML с текстом «Hello, World!».  

---

### **8. Обновление страницы в браузере**
Открываем `http://localhost:8000` и теперь видим:  
```
Hello, World!
```

---

### **9. Просмотр конфигурации виртуального хоста**
```bash
cd /etc/apache2/sites-enabled/
cat 000-default.conf
```
**Что делает команда?**  
- `cd /etc/apache2/sites-enabled/` — переходит в каталог с активными конфигурациями виртуальных хостов Apache.  
- `cat 000-default.conf` — выводит содержимое конфигурационного файла.  

**Вывод в консоли:**  
В файле должно быть что-то вроде:
```apache
<VirtualHost *:80>
    DocumentRoot /var/www/html
    ...
</VirtualHost>
```
Это означает, что Apache обслуживает веб-страницы из `/var/www/html` на порту `80`.

---

### **10. Выход из контейнера**
```bash
exit
```
**Что делает команда?**  
- Завершает работу терминала контейнера и выходит в обычный терминал.

---

### **11. Просмотр списка контейнеров**
```bash
docker ps -a
```
**Что делает команда?**  
- Показывает список всех контейнеров, включая остановленные.  

**Вывод в консоли:**  
```bash
CONTAINER ID   IMAGE    COMMAND   CREATED         STATUS                      NAMES
a1b2c3d4e5f6   ubuntu   "bash"    10 minutes ago Exited (0) 2 minutes ago    containers04
```
Наш контейнер `containers04` виден в списке.

---

### **12. Удаление контейнера**
```bash
docker rm containers04
```

**Вывод в консоли:**  
Если всё прошло успешно, контейнер больше не будет отображаться в `docker ps -a`.

---
