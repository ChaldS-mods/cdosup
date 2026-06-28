# 📦 CDOSUP — ChaldOS User Package Repository

**CDOSUP** — это AUR-подобный репозиторий пакетов для **ChaldOS Linux**.

Пользователи могут устанавливать пакеты одной командой:
```bash
chaldos-pkg -C название_пакета
```

А любой желающий может добавить свой пакет — просто отправьте Pull Request с CDOSBUILD!

## Как установить пакет

```bash
# Обновить базу пакетов
chaldos-pkg -Sy

# Установить бинарный пакет (если есть в pool/)
chaldos-pkg -S fuzzel

# Собрать и установить из исходников (CDOSUP)
chaldos-pkg -C nvidia-firmware
```

## Как добавить свой пакет

1. Сделайте форк этого репозитория
2. Создайте `packages/<имя-пакета>/CDOSBUILD`
3. Отправьте Pull Request

Подробнее: [CONTRIBUTING.md](CONTRIBUTING.md)

## Структура репозитория

```
cdosup/
├── packages/              # CDOSBUILD рецепты (как AUR)
│   ├── nvidia-firmware/   # Прошивки для NVIDIA GPU
│   │   └── CDOSBUILD
│   ├── fuzzel/            # Wayland лаунчер (меню пуска)
│   │   └── CDOSBUILD
│   └── example/           # Шаблон для новых пакетов
│       └── CDOSBUILD
├── pool/                  # Бинарные .cdos пакеты (pre-built)
├── packages.db            # База данных пакетов
├── README.md
├── CONTRIBUTING.md
└── LICENSE
```

## Формат CDOSBUILD

```bash
pkgname="мой-пакет"
pkgver="1.0.0"
pkgdesc="Описание пакета"
arch="x86_64"
license="MIT"
source=("https://example.com/исходники.tar.gz")
depends="зависимость1, зависимость2"
makedepends="инструменты-сборки"

build() {
    # Сборка (запускается в $srcdir)
    make
}

package() {
    # Установка в $pkgdir (создать $pkgdir/usr/)
    make DESTDIR="$pkgdir" install
}
```

## Связанные проекты

- [ChaldOS-mini](https://github.com/ChaldS-mods/ChaldOS-mini) — Minimal Linux distribution
- [chaldos-pkg](https://github.com/ChaldS-mods/ChaldOS-mini/blob/main/rootfs/usr/bin/chaldos-pkg) — Package manager

## Лицензия

CC0 — Public domain. Рецепты пакетов не являются кодом.
