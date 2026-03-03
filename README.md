# Laborant_02
Отчёт по второй лабораторной работе.

## Laboratory work II

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

## Tutorial

1. Настройка глобальных переменных среды GitHub и установка редактора по умолчанию (В моём случае `nano`):
```sh
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_EMAIL=<адрес_почтового_ящика>
$ export GITHUB_TOKEN=<сгенирированный_токен>
$ alias edit=<nano|vi|vim|subl>
```

2. Активируем виртуальное окружение:
```sh
$ cd ${GITHUB_USERNAME}/workspace
$ source scripts/activate
```

3. Создаём директорию в домашнем каталоге пользователя, формируем конфигурационный файл с данными от нашего аккаунта и настраиваем протокол связи:
```sh
$ mkdir ~/.config
$ cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
$ git config --global hub.protocol https
```

4. Создаётся проектная директория, инициализируется репозиторий Git, устанавливаются имя и email пользователя, проверяются глобальные настройки, добавляется удалённый репозиторий на GitHub, загружаются последние изменения, создаётся файл `README.md`, фиксируется его внесение в первом коммите и отправляются изменения на удалённый сервер:
```sh
$ mkdir projects/lab02 && cd projects/lab02
$ git init
$ git config --global user.name ${GITHUB_USERNAME}
$ git config --global user.email ${GITHUB_EMAIL}
# check your git global settings
$ git config -e --global
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git
$ git pull origin master
$ touch README.md
$ git status
$ git add README.md
$ git commit -m"added README.md"
$ git push origin master
```

5. Добавляем на сервисе **GitHub** в репозитории **lab02** файл **.gitignore** со следующем содержимом:
```sh
*build*/
*install*/
*.swp
.idea/
```

6. Скачиваем свежие изменения с удалённого репозитория и делаем проверку журнала изменений:
```sh
$ git pull origin master
$ git log
```

7. Cоздаём структуру проекта, состоящую из трёх директорий (`sources`, `include`, `examples`), а затем поместили в директорию `sources` файл `print.cpp`:
```sh
$ mkdir sources
$ mkdir include
$ mkdir examples
$ cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```

8. Создаём заголовочный файл `print.hpp`:
```sh
$ cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF
```

9. Создаём пример программы `example1.cpp`:
```sh
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

10. Создаём пример программы `example2.cpp`
```sh
$ cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

11. Редактируем файл `README.md`:
```sh
$ edit README.md
```

12. Проверяем состояние репозитория, добавляем все изменения, фиксируем их коммитом с сообщением и отправляем на удалённый сервер:
```sh
$ git status
$ git add .
$ git commit -m"added sources"
$ git push origin master
```
