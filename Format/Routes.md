# Routy
Routa je komplexní definice, jak převádět a přeposílaat určitá data, která přijdou do převodníku.
Vlastnosti:
- Jméno
- Typ - směr přeposílání
    - Ethernet -> CAN (`canout`)
    - CAN -> Ethernet (`canin`)
- Listenery (viz dále)
```json
{
    "routes":[
        {
            "name":"firstRoute",
            "type":"canout",
            "listeners":[
                ...
            ]
        },
        {
            "name":"secondRoute",
            "type":"canin",
            "listeners":[
                ...
            ]
        },
    ]
}
```
## Listenery
Listener je jednotka, která naslouchá na určitém kanále na určitý typ zprávy, a pokud přijde, přepošle ji dál.

### Ethernet -> CAN Listenery
(`listeners`)  
Je potřeba specifikovat:
- **port**, na kterém bude naslouchat
- **protokol**, který bude pro naslouchání použit *(TCP/UDP)*
- **Začátek zprávy** - řetězec/[regex](Regex.md), který označuje místo, kde začíná jedna zpráva (`startsWith`)
- **Konec zprávy** - řetězec/[regex](Regex.md), který označuje místo, kde končí jedna zpráva (`endsWith`)
TODO: aby stačil jen začátek nebo konec
- **Konverter** - viz dále

Dobrovolné vlastnosti
- Zahrnout řetězce označující začátek a konec zprávy do vstupu konverteru [*výchozí: ano*] (`includeBorders`)
- Filtr - může být řetězec nebo regex
    - pokud zadáte řetězec, budou zpracovány jen zprávy, které jej obsahují
    - pokud zadáte regex, budou zpracovány jen zprávy, které mu odpovídají
```json
{
    "routes":[
        {
            "name":"firstRoute",
            "type":"canout",
            "listeners":[
                {
                    "port":"1234",
                    "protocol":"udp",
                    "startsWith":"$PMACO",
                    "endsWith":"/\\*[0-9a-fA-F]+\r\n/",
                    "includeBorders":true,
                    "filter":"/^[a-Z],(.*)$/",
                    "converters":[
                        ...
                    ]
                }
            ]
        },
    ]
}
```

### CAN -> Ethernet Listenery
(`listeners`)  
Je třeba specifikovat:
- **Kanál**, který bude použit pro naslouchání
- **Konverter** - viz dále

Dobrovolné vlastnosti
- Filtr - řetězec ve tvaru hexadecimálního čísla
- Maska filtru - také řetězec v hexa tvaru

Maska určuje, které bity filtru budou použity při filtrování. Filtr bude s maskou logicky vynásoben.
```json
{
    "routes":[
        {
            "name":"secondRoute",
            "type":"canin",
            "listeners":[
                {
                    "channel":"can0",
                    "filter":"",
                    "filterMask":"",
                    "converters":[
                        ...
                    ]
                }
            ]
        },
    ]
}
```

## Konvertery
Konverter je jednotka, která dostane jako vstup řetězec, který mu předá Listener, a jako výstup odesílá zprávy do opačného typu sítě.
Konvertery specifikují, jak vstupní řetězec rozdělit, jaké operace s těmito částmi provést, a jak je vložit do odesílané zprávy. *[Více zde](/Format/Converters/Main.md)*