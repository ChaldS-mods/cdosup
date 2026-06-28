# Contributing to CDOSUP

Спасибо, что хотите добавить пакет в CDOSUP! Это очень просто.

## Как добавить пакет

### 1. Создайте CDOSBUILD

Сделайте форк репозитория и создайте файл:

```
packages/<имя-пакета>/CDOSBUILD
```

### 2. Заполните метаданные

Минимальные требования:

```bash
pkgname="имя-пакета"          # обязательно
pkgver="1.0.0"                # обязательно
pkgdesc="Что это за пакет"
arch="x86_64"
license="GPL-3.0-only"
source=("URL исходников")
depends=""
makedepends=""
```

### 3. Напишите build() и package()

**build()** — сборка пакета из исходников.
Запускается в директории `$srcdir`.

**package()** — установка собранных файлов.
Файлы нужно скопировать в `$pkgdir`.
Внутри `$pkgdir` должна быть структура как в `/`:

```
$pkgdir/
└── usr/
    ├── bin/
    ├── lib/
    └── share/
```

### 4. Проверьте локально

```bash
chaldos-pkg -C ваш-пакет
```

### 5. Отправьте Pull Request

## Правила

- **Безопасность**: не включайте бинарники в CDOSBUILD. Только ссылки на официальные источники.
- **Лицензия**: указывайте реальную лицензию пакета.
- **Зависимости**: указывайте ВСЕ зависимости.
- **Версионирование**: используйте версию как в оригинальном пакете.
- **Одна команда = один пакет**: `chaldos-pkg -C <пакет>` должен устанавливать одну программу.

## Пример: простой пакет

```bash
pkgname="hello-chaldos"
pkgver="1.0.0"
pkgdesc="Простой hello world для ChaldOS"
arch="x86_64"
license="MIT"
source=("https://example.com/hello-chaldos.tar.gz")

build() {
    gcc -o hello hello.c
}

package() {
    mkdir -p "$pkgdir/usr/bin"
    cp hello "$pkgdir/usr/bin/"
}
```

## Пример: meson-проект (fuzzel)

```bash
pkgname="fuzzel"
pkgver="1.10.2"
pkgdesc="Wayland application launcher"
arch="x86_64"
license="MIT"
source=("https://codeberg.org/dnkl/fuzzel/releases/download/${pkgver}/fuzzel-${pkgver}.tar.gz")
depends="wayland, pixman, fcft, freetype"
makedepends="meson, ninja, scdoc"

build() {
    meson setup build --prefix=/usr --buildtype=release
    ninja -C build
}

package() {
    DESTDIR="$pkgdir" ninja -C build install
}
```

## Вопросы?

Открывайте Issue в репозитории!
