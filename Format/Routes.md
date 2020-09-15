# Routy
Routa je komplexní nastavení, jak převádět a přeposílat určitá data, která přijdou do převodníku.
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
            "endpoint": "10.10.53.205",
            "port":25555,
            "protocol":"udp",
            "listeners":[
                ...
            ]
        },
    ]
}
```

## Listenery
Listener je jednotka, která naslouchá na určitém kanále na určitý typ zprávy, a pokud přijde, přepošle ji do *konverterů*.

### Ethernet -> CAN Listenery
(`listeners`)  
Je potřeba specifikovat:
- **port**, na kterém bude naslouchat
- **protokol**, který bude pro naslouchání použit *(TCP/UDP)*
- **Začátek zprávy** - řetězec/[regex](Regex.md), který označuje místo, kde začíná jedna zpráva (`startsWith`)
- **Konec zprávy** - řetězec/[regex](Regex.md), který označuje místo, kde končí jedna zpráva (`endsWith`)
- **Konvertery** - viz dále

> **Důležité**: Na konci zprávy můžou být znaky konce řádku CR a LF ale nemusí - budou při filtraci ignorovány.

Dobrovolné vlastnosti
- Zahrnout řetězce označující začátek a konec zprávy do vstupu konverteru [*výchozí: ano*] (`includeBorders`)
- Filtr - může být řetězec nebo regex
    - pokud zadáte řetězec, budou zpracovány jen zprávy, které jej obsahují
    - pokud zadáte regex, budou zpracovány jen zprávy, které mu odpovídají
- Vzyč vlajku (`setFlag`)- nastaví vlajku pro pozdější použití (viz [Globální úložiště](/Format/Globals.md))
- Globální ůložiště (`flagStore`)- Název globálního úložiště, kterému se vlajka nastaví (např <kbd>global mojeúložiště</kbd>)
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
                    "endsWith":"/\\*[0-9a-fA-F]+/",
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
- **Vstupní CAN kanál**
- **Konvertery** - viz dále

Veškerý výstup z tohoto typu routy půjde do jednoho místa v IP síti - toto specifikujete těmito vlastnostmi:
- Koncový bod (`endpoint`) - IP adresa zařízení, na který se bude odesílat výstup
- port
- protokol - který bude pro odeslání použit *(TCP/UDP)*

Tyto vlastnosti **nemusíte** specifikovat, pokud si nepřejete, aby tato routa měla přímý výstup, ale např. se její výstup jen ukládal do globálního úložiště. Výsledek pak může být odeslán pomocí jiné routy (viz viz [Globální úložiště](/Format/Globals.md)).
> Pro Ethernet -> CAN listenery je výstupní kanál specifikován ve výstupní jednotce konverteru, viz [Konvertery](/Format/Converters/Main.md).

Dobrovolné vlastnosti
- Filtr - řetězec ve tvaru hexadecimálního čísla
- Maska filtru - také řetězec v hexa tvaru
- Vzyč vlajku (`setFlag`) - nastaví vlajku pro pozdější použití (viz [Globální úložiště](/Format/Globals.md))
- Globální ůložiště (`flagStore`)- Název globálního úložiště, kterému se vlajka nastaví (např <kbd>global mojeúložiště</kbd>)

Maska určuje, které bity filtru budou použity při filtrování. Filtr bude s maskou logicky vynásoben a porovnán s PGN příchozí zprávy.
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