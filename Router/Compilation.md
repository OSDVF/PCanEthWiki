# Kompilace EthCanRouteru
Tento návod je určen pro kompilaci [EthCanRouter](/Router/Main.md) na systému Debian 10.

## 1. Crosstool-NG
Nejdříve potřebujeme nástroj Crosstool-NG, který skompiluje a připraví kompilátor GCC a standartní knihovny, které poté použijeme ke kompilaci našeho programu. Nejdříve ale stáhneme tyto balíčky:
```bash
sudo apt-get install build-essential flex libncurses-dev ncurses-base ncurses-bin libtool libtool-bin python python-dev gdb make automake autoconf texinfo unzip help2man gawk bison gettext man wget 
```
Poté samotný [Crosstool-NG 1.24](http://crosstool-ng.org/download/crosstool-ng/crosstool-ng-1.24.0.tar.bz2) a rozbalíme jej do složy `~/crosstool/`. Vznikne složka `~/crosstool/crosstool-ng-1.24.0`, do které vstoupíme příkazem `cd` a pak provedeme
```bash
./configure
make
./ct-ng menuconfig
```
V menu nastavíme všechno podle [tohoto návodu](http://unisim-vp.org/site/crosstool-arm-926ejs-linux-gnueabi-how-to.html) a k tomu ještě v sekci `Paths and misc options` vypneme možnost `Install licenses`. Dalším krokem je
```bash
./ct-ng build
```
Toto může trvat i přes hodinu.


## 2. Knihovny
Kromě správného kompilátoru je nutné mít v systému nainstalovány také [potřebné knihovny](Libs.md)