# Сборка OpenWrt для Cudy WR1300 v4 из исходников

↩️ [English version (BUILD_EN.md)](BUILD_EN.md)

Патч добавляет поддержку Cudy WR1300 v4. Автор патча: Jan Brudna, [страница патча (v2) на Patchwork](https://patchwork.ozlabs.org/project/openwrt/patch/20260513183938.242137-1-jan.brudna@gmail.com/). Проверено на OpenWrt `v25.12.6`.

Перед началом подготовьте систему сборки: [список зависимостей](https://openwrt.org/docs/guide-developer/toolchain/install-buildsystem). Собирать нужно не от root, и нужно достаточно свободного места на диске.

1. Клонируйте OpenWrt и переключитесь на нужную версию:
```bash
git clone https://github.com/openwrt/openwrt.git
cd openwrt
git checkout v25.12.6
```

2. Обновите feeds:
```bash
./scripts/feeds update -a
./scripts/feeds install -a
```

3. Задайте имя и почту для git (локально, для `git am`) и примените патч (авторство сохраняется):
```bash
git config user.name "Your Name"
git config user.email "you@example.com"
curl -L https://raw.githubusercontent.com/LinSwitch/cudy-wr1300-v4-openwrt/refs/heads/main/v2-ramips-add-support-for-Cudy-WR1300-v4.patch | git am
```

4. Скачайте официальный конфиг для архитектуры. Версия в ссылке должна совпадать с тегом из шага 1:
```bash
curl -L https://downloads.openwrt.org/releases/25.12.6/targets/ramips/mt7621/config.buildinfo -o .config
```

5. Выберите профиль устройства и проверьте конфигурацию.
Target System: (`MediaTek Ralink MIPS`) и Subtarget (`MT7621 based boards`) уже должны быть выбраны; измените только Target Profile на `Cudy WR1300 v4`:
```bash
make menuconfig   # Target Profile: Cudy WR1300 v4
make defconfig
```

6. Скачайте исходники заранее и запустите сборку:
```bash
make download
make -j$(nproc) V=s
```
   Если загрузка зависла, запустите `make download V=s`, чтобы увидеть проблемный файл. Скачайте его вручную с [sources.openwrt.org](https://sources.openwrt.org/) в папку `dl`, например:
```bash
curl -L -O --output-dir dl https://sources.openwrt.org/bzip2-1.0.8.tar.gz
```
   После этого снова запустите `make download`, а когда загрузка завершится, команду сборки.

Готовые образы лежат в `bin/targets/ramips/mt7621/`.


### Возможная ошибка при `make menuconfig` или `make defconfig`

При сборке OpenWrt 25.12.6 может возникнуть ошибка циклической зависимости, связанная с пакетами WeeChat:

```text
error: recursive dependency detected!
symbol PACKAGE_luci-app-weechat depends on PACKAGE_weechat-full
symbol PACKAGE_weechat-full depends on PACKAGE_weechat-minimal
symbol PACKAGE_weechat-minimal is selected by PACKAGE_luci-app-weechat
```

Если столкнулись с этой ошибкой, откройте файл:

`package/feeds/luci/luci-app-weechat/Makefile`

Найдите строку:

```make
LUCI_DEPENDS:=+luci-base +weechat +rpcd-mod-ucode
```

Замените её на:

```make
LUCI_DEPENDS:=+luci-base +weechat-full +rpcd-mod-ucode
```

После этого удалите временные файлы конфигурации и повторно выполните `make defconfig`:

```bash
rm -rf tmp/
make defconfig
```

Если ошибка исчезла, можно продолжать сборку. Это изменение требуется только в том случае, если указанная ошибка действительно возникает.