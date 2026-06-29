# Лабораторная работа №04: Настройка пайплайна непрерывной интеграции (CI) с помощью GitHub Actions

## Ход выполнения работы

### Шаг 1. Подготовка директории и инициализация репозитория
Очистка папки четвертой лабораторной работы от старых конфигураций, перенос чистой кодовой базы из предыдущей работы, переход в рабочую папку и сброс старого Git-дерева для исключения конфликтов.

**Команды:**
```bash
rm -rf ~/workspace/projects/lab04
cp -r ~/workspace/projects/lab03 ~/workspace/projects/lab04
cd ~/workspace/projects/lab04
rm -rf .git
git init -b main

```

**Вывод:**

```bash
Initialized empty Git repository in /Users/mac/workspace/projects/lab04/.git/

```

---

### Шаг 2. Первичная привязка репозитория и создание конфигурации CI

Добавление ссылки на удаленный репозиторий четвертой лабы, создание служебных директорий и написание файла сценария `ci.yml` для автоматической сборки проекта роботом GitHub Actions.

**Команды:**

```bash
git remote add origin https://${GITHUB_TOKEN}@[github.com/g4l0p3r1d0l/lab04.git](https://github.com/g4l0p3r1d0l/lab04.git)
mkdir -p .github/workflows
cat << 'EOF' > .github/workflows/ci.yml
name: CMake CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Configure CMake
      run: cmake -B _build -DCMAKE_BUILD_TYPE=Release
    - name: Build
      run: cmake --build _build --config Release
EOF

```

---

### Шаг 3. Создание файла документации

Создание первичного файла описания лабораторной работы.

**Команда:**

```bash
cat << 'EOF' > README.md
# Lab 04: Continuous Integration
Пайплайн автоматической сборки настроен.
EOF

```

---

### Шаг 4. Фиксация изменений в локальном репозитории

Индексация всех созданных файлов проекта и выполнение первого корневого коммита.

**Команды:**

```bash
git add .
git commit -m "feat: setup github actions workflow"

```

**Вывод:**

```bash
[main (root-commit) c19b52f] feat: setup github actions workflow
 7 files changed, 38 insertions(+)
 create mode 100644 .github/workflows/ci.yml
 create mode 100644 .gitignore
 create mode 100644 CMakeLists.txt
 create mode 100644 README.md
 create mode 100644 examples/example1.cpp
 create mode 100644 include/print.hpp
 create mode 100644 sources/print.cpp

```

---

### Шаг 5. Ошибка авторизации при попытке отправки кода

Попытка выполнения команды `git push` в новой сессии терминала, приведшая к ошибке авторизации из-за отсутствия переменной окружения токена в текущем контексте окружения.

**Команда:**

```bash
git push origin main --force

```

**Вывод:**

```bash
Password for '[https://github.com](https://github.com)': 
remote: No anonymous write access.
fatal: Authentication failed for '[https://github.com/g4l0p3r1d0l/lab04.git/](https://github.com/g4l0p3r1d0l/lab04.git/)'

```

---

### Шаг 6. Восстановление токена доступа и успешная публикация на GitHub

Экспорт персонального токена доступа в текущую сессию терминала, принудительное обновление URL удаленного репозитория с валидными учетными данными и успешная отправка ветки `main` на удаленный сервер.

**Команды:**

```bash
export GITHUB_TOKEN="my_token"
git remote set-url origin https://${GITHUB_TOKEN}@[github.com/g4l0p3r1d0l/lab04.git](https://github.com/g4l0p3r1d0l/lab04.git)
git push origin main --force

```

**Вывод:**

```bash
Enumerating objects: 14, done.
Counting objects: 100% (14/14), done.
Delta compression using up to 10 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (14/14), 1.48 KiB | 1.48 MiB/s, done.
Total 14 (delta 0), reused 0 (delta 0), pack-reused 0
To [https://github.com/g4l0p3r1d0l/lab04.git](https://github.com/g4l0p3r1d0l/lab04.git)
 * [new branch]      main -> main

```

---

## Вывод

В ходе лабораторной работы были изучены практические аспекты настройки непрерывной интеграции (Continuous Integration) на базе облачной платформы **GitHub Actions**. Был создан конфигурационный файл в формате YAML, описывающий этапы развертывания виртуальной машины на базе Ubuntu, конфигурирования C++ проекта через CMake и последующей автоматической сборки. В процессе выполнения работы были успешно решены проблемы с контекстом переменных окружения (`GITHUB_TOKEN`) в Unix-терминале, и проект был опубликован в удаленной ветке `main`.
