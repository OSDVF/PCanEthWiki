# Vedlejší instrukce pro programu
- Log Soubor - ukládají se do něj akce, které program provádí
- Log Level - počet zpráv, které v logu budou
    - Nic (`no`)
    - Normální - jen informace o změnách konfigurace programů (`normal`)
    - Všechno - ukládá záznam o přeposlání každé zprávy  (`all`)
- Síťová konfigurace
    - IP
    - Maska podsítě
    - Výchozí brána
```json
{
    "logFile":"relativeOrAbsolutePathToFile.log",
    "logLevel":"normal",
    "net":{
        "ip":"10.10.53.200",
        "mask":"255.0.0.0",
        "gateway":"10.255.255.254"
    }
}
```
## Poznámka
Pozor! Změna síťové konfigurace může mít za následek odpojení od sítě, ke které je převodník aktuálně připojen. Tuto konfiguraci si zapamatujte, protože jinak nebude možné se k převodníku opětovně připojit.

Pokud bude síťová konfigurace zadána chybně, převodník bude mít výchozí IP Adresu `192.168.1.10`.
