# EthCanRouter
Tento software je třeba nahrát na převodník. Byl navržen pro PEAK Ethernet Gateway, ale teoreticky by měl fungovat s každým zařízením, na kterém běží Linux s verzí jádra minimálně 2.6.31 a ovladačem SocketCAN.

## Spuštění
Po tom, co je software nahrán do převodníku pomocí EthCanConfigu, je umístěn v adresáři /mnt/user v souboru jménem eth-can-config. Měl by být nastaven jako spustitelný. Navíc v adresáři /etc/init.d/ je přítomen spouštěcí skript, který jej spustí po nabootování převodníku.

## Manuální spuštění
Pokud si přejete tento software testovat, připojte se k převodníku pomocí SSH (např. [PuTTy](https://www.putty.org/)) a příkazem `cd /mnt/user/` vstupde do adresáře s programem, který spustíte pomocí příkazu `./eth-can-router`

Pokud jej spustíte bez argumentů, pokusí se načíst konfigurační soubor se jménem `default.json` z aktuálního pracovního adresáře.
### Argumenty
- `-s` Místo spuštění proběhne instalace spouštěcího skriptu do /etc/init.d/
- `-v` Místo spuštění vypíše verzi softwaru
- `-c [jméno souboru.json]` Nastaví cestu k souboru, ze kterého se čte konfigurace 
- `-d` Dry run - nepoužívat skutečné CAN rozhraní, pouze ho simulovat
- `-t` Testování zadáváním vstupu na STDIN, viz dále
- `-r [x]` Číslo routy, která se bude testovat
- `-l [x]` Číslo listeneru, který se bude testovat  
- `-x` Testování bude probíhat se vstupem v hexadecimálním tvaru
- `-f` Vynutit průchod filterm listeneru

Po spuštění je možné kdykoliv program ukončit zadáním slova `exit`.

## Kompilace
Pokud si přejete tento software skompilovat, přejděte na [návod zde](Compilation.md)