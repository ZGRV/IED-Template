# IED-Template

Шаблон проекта прошивки интеллектуального электронного устройства (ИЭУ) по стандарту **IEC 61850** на языке C. Стек протокола — [libiec61850](https://github.com/mz-automation/libiec61850), подключённый как git-сабмодуль.

## Структура

```
├── src/main.c                  # точка входа (заполняется в рамках заданий)
├── include/                    # заголовочные файлы проекта
├── cmake/toolchain.cmake       # тулчейн кросс-компиляции (aarch64-buildroot, sysroot /opt/arm_sysroot)
├── third_party/libiec61850/    # сабмодуль: стек IEC 61850 + примеры (goose_publisher, goose_observer, ...)
└── docs/task-01-initial.md     # учебное задание 1: форк, сборка, локальный запуск GOOSE-публикатора
```

## Требования

- Linux (Ubuntu 22.04+) или WSL2;
- git, cmake ≥ 3.10, gcc, gdb;
- для кросс-сборки: тулчейн aarch64-buildroot в `/opt/arm_sysroot`;
- для реальной отправки GOOSE: права root (raw-сокеты).

## Сборка

Клонирование (важен флаг сабмодулей):

```bash
git clone --recurse-submodules <url> && cd IED-Template
```

Нативная сборка (x86_64, для локальной отладки):

```bash
cmake -B build -DCMAKE_BUILD_TYPE=Debug
cmake --build build -j
./build/ied_exe eth0        # имя своего интерфейса; для GOOSE — sudo
```

Кросс-сборка под целевую ARM-плату (aarch64):

```bash
cmake -B build-aarch64 -DCMAKE_BUILD_TYPE=Debug -DCROSS_COMPILE=ON
cmake --build build-aarch64 -j
```

## Отладка в VS Code

Конфигурации уже подготовлены в `.vscode/`:

| Задача / конфигурация | Назначение |
|---|---|
| `Build (host x86_64)` (F7) | сборка под локальную отладку |
| `Debug (host x86_64)` (F5) | локальная отладка gdb |
| `Build (target aarch64)` | кросс-сборка под плату |
| `Deploy & gdbserver (target)` | загрузка бинарника на плату + gdbserver :1234 |
| `Debug (target aarch64, gdbserver)` (F5) | удалённая отладка на плате (IP задать в `.vscode/gdb-remote.gdb`) |
| `QEMU + cross-gdb (terminal)` | проверка aarch64-бинарника в эмуляторе без железа |

## Лицензия

[GPLv3](LICENSE)
