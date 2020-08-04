# Konvertery
Konverter je jednotka, která dostane jako vstup řetězec, který mu předá Listener, a jako výstup odesílá zprávy do opačného typu sítě.
Konvertery specifikují, jak vstupní řetězec rozdělit, jaké operace s těmito částmi provést, a jak je vložit do odesílané zprávy.

Konvertery obsahují vstupní, akční a výstupní jednotky.

## Vstupní jednotka
TODO: tohle platí jen pro Ethernet->CAN

U každé z nich specifikujte, jak zacházet se syrovým vstupem z CAN kanálu/Internetového protokolu. Typicky rozděluje souvislou zprávu na *více podčástí* = seznam fragmentů. Můžete použít více vstupních jednotek konverteru, v tom případě je nutné je pojmenovat. (`name`)  
Vstupní jednotky konverteru můžou být těchto typů:
- Prostá - nerozděluje zprávu, seznam bude obsahovat jen jeden *fragment*
- scanf - interpretuje data jako ASCII text a rozdělí jej pomocí formátovcího řetězce, který je typický pro funkci `scanf` jazyka C.
- separator - interpretuje jako text a rozdělí ho podle specifikovaného rozdělovacího znaku
- regex - extrahuje z textu části odpovídající zachytávacím skupinám (capture groups) z regulárního výrazu ([pro testování použijte třeba regex101](https://regex101.com/))
- bits - vybere z přijaté zprávy určité skupiny bitů a uspořádá je podle potřeby. Každá skupina bitů (fragment) je brána jako jedno číslo.
    - Pořadí bajtů - endianita = Big Endian/Little Endian (`byteOrder`)
    - Pořadí bitů v jednom bajtu - specifikuje, který bit je přečten jako "nultý" (LSB -> nultý bude bit nejvíce vpravo) (`bitOrder`)

Navíc můžete u každého typu vst. jednotky zadat vlastní pořadí, podle kterého se přeskládají různé extrahované podčásti zprávy. (Vlastnost `shuffle`, pro přeskládání přesně v opačném pořadí zadejte její hodnotu `Inf`)

### Datové typy fragmentů
Datové typy fragmentů se mohou lišit podle použitého typu vstupní jednotky, která jej vyprodukovala.
Rozlišují se datové typy `string` (řetězec), `int` (znaménkové číslo) a `uint` (bezznaménkové číslo).

Vst. jednotky produkují:
- Prostá - celá zpráva je `string`
- scanf - fragmenty obdrží datové typy specifikované formátovacím řetězcem
- separator - u každého fragmentu je proveden pokus o převedení na `int`, a pokud selže, fragment bude typu `string`.
- regex - stejně jako u **separator**
- bits - u každé skupiny bitů je specifikováno, jestli se mají interpretovat jako `int`, nebo `uint`. (V konfiguračním souboru je pro použití `uint` uvedeno u každého rozmezí bitů písmeno `u`)
> Poznámka: u jednotky může být uvedena maximální velikost jednoho `string` fragmentu. Pokud není uvedena, je použita výchozí hodnota 256.

## Akce
Akční jednotky umožňují provádět různé operace nad daty extrahovanými ze zprávy. Jedna extrahovaná část = `fragment`. Výsledky operací jsou opět uloženy do seznamů fragmentů v jednotlivých vst. jednotkách. Pokud index fragmentu, specifikovaný jako výstupní, neexistuje, je vytvořen (jsou použity dynamické seznamy).  
#### Obvyklé paramtery
Tyto paramtery se objevují u většiny akčních jednotek.
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Název zdroje</th><td>Název vstupní jednotky, ze které se data berou. Pokud je použita je jedna, nepojmenovaná, tak je tento parametr nepotřebný.</td>
        <td><code>inputName</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Index zdroje</th><td>Index v seznamu fragmentů ze vst. jednotky</td>
        <td><code>inputIndex</code> (<tt>number</tt>)</td>
    </tr>
</table>

----------

### Not (`not`)
Neguje veškeré bity ve fragmentu.
#### Parametry
Bere jen *obvyklé parametry*.

### Bitová maska (`mask`)
Provede logický součin nebo součet nad bity v jednom fragmentu.
#### Parametry
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Typ operace</th><td>AND - součin / OR - součet</td>
        <td><code>op</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Maska</th><td>Hexadecimální číslo, se kterým bude provedena požadovaná operace</td>
        <td><code>mask</code> (<tt>string</tt>)</td>
    </tr>
</table>

### Posun (`lshift`,`rshift`)
Provede aritmetický nebo logický posun všech bitů ve fragmentu doleva nebo doprava.
#### Parametry
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Velikost posunu</th><td>O kolik bitů</td>
        <td><code>amount</code> (<tt>number</tt>)</td>
    </tr>
    <tr>
        <th>Aritmetický posun</th><td>(dobrovolný) Zachovat znaménkovost fragmentu. Pokud tento paramtetr není nastaven, je použit vždy posun adekvátní k datovému typu fragmentu.</td>
        <td><code>arithmetic</code> (<tt>boolean</tt>)</td>
    </tr>
</table>

### Spojení fragmentů (`concat`)
Používá se u fragmentů typu `string`. Spojí dva řetězce dohromady a uloží je do jakéhokoliv z fragmentů.
#### Parametry
*Nebere žádný z obvyklých parametrů.*
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Název 1. vst. j.</th><td>Název vstupní jednotky, ze které se vezme první polovina výsledného řetězce</td>
        <td><code>firstName</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Index 1. vst. fragmentu</th><td>Index v seznamu fragmentů 1. vst. jednotky</td>
        <td><code>firstIndex</code> (<tt>number</tt>)</td>
    </tr>
    <tr>
        <th>Název 2. vst. j.</th><td>Název vstupní jednotky, ze které se vezme druhá polovina výsledného řetězce</td>
        <td><code>secondName</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Index 2. vst. fragmentu</th><td>Index v seznamu fragmentů 2. vst. jednotky</td>
        <td><code>secondIndex</code> (<tt>number</tt>)</td>
    </tr>
    <tr>
        <th>Název vst. j. pro výstup</th><td>Název vstupní jednotky, do které se uloží výsledný řetězec</td>
        <td><code>destinationName</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Index fragmentu pro výstup</th><td>Index v seznamu fragmentů výstupní. vst. jednotky</td>
        <td><code>destinationIndex</code> (<tt>number</tt>)</td>
    </tr>
</table>

### Seřazení fragmentů (`shuffle`)
Zpřehází aktuální seznam fragmentů jedné vstupní jednotky.
#### Parametry
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Přehození</th><td>Seznam indexů - v jakém pořadí budou fragmenty jedné vst. jednotky seřazeny po provedení příkazu. (Při "pozpátku" bude v konf. souboru hodnota <code>Inf</code>)</td>
        <td><code>shuffle</code> (<tt>array[number]</tt>)</td>
    </tr>
</table>

### Záměna fragmentů (`swap`)
Zamění jeden fragment jedné vst. jednotky za jiný fragment jiné vst. jednotky. (popř. stejné vst. jednotky).
#### Parametry
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Název 1. vst. jedn.</th><td></td>
        <td><code>firstName</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Index 1. fragmentu</th><td></td>
        <td><code>firstIndex</code> (<tt>number</tt>)</td>
    </tr>
    <tr>
        <th>Název 2. vst. jedn.</th><td></td>
        <td><code>secondName</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Index 2. fragmentu</th><td></td>
        <td><code>secondIndex</code> (<tt>number</tt>)</td>
    </tr>
</table>

### Formátovaný výpis (`printf`)
Interpretuje data z každého specifikovaného fragmentu jako pole 8-bitových ASCII znaků a použije funkci `printf` pro výpis do jiného fragmentu.
#### Parametry
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Indexy vst. fragmentů</th><td>Seznam indexů fragmetů, které budou předány funkci <code>printf</code>. (tento parametr nahrazuje běžný parametr <code>inputIndex</code>)</td>
        <td><code>inputIndexes</code> (<tt>array[number]</tt>)</td>
    </tr>
    <tr>
        <th>Formát</th><td>Formátovací řetězec</td>
        <td><code>format</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Cílová vst. jednotka</th><td></td>
        <td><code>destinationName</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Cílový index fragmentu</th><td></td>
        <td><code>destinationIndex</code> (<tt>number</tt>)</td>
    </tr>
</table>

### Slovníkový překlad (`dictionary`)
Převede data z číselného formátu na ASCII znaky nebo obráceně.
#### Parametry
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Indexy vst. fragmentů</th><td>Seznam indexů fragmetů, které budou přeloženy. (tento parametr nahrazuje běžný parametr <code>inputIndex</code>)</td>
        <td><code>inputIndexes</code> (<tt>array[number]</tt>)</td>
    </tr>
    <tr>
        <th>Slovník</th><td>Pole řetězců ve formátu *číslo mezera znak* nebo *znak mezera číslo*, např. "1 Y". Nelze kombinovat oba případy v jednom poli. Jeden slovník umožňuje překlad jen jedním směrem</td>
        <td><code>dictionary</code> (<tt>array[string]</tt>)</td>
    </tr>
</table>

### Provedení příkazu sed (`sed`)
Spustí příkaz [sed](https://www.gnu.org/software/sed/manual/sed.html) nad uvedeným fragmentem.
#### Parametry
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Argumenty</th><td>Řetězec argumentů, který bude doslovně předán příkazu <code>sed</code>.<br>Argumenty, které souvisí s výpisem do souborů budou samozřejmě ignorovány.</td>
        <td><code>arguments</code> (<tt>string</tt>)</td>
    </tr>
</table>

### Regulární výraz (`regex`)
Spustí regulární výraz nad uvedeným fragmentem, který interpretuje jako pole 8-bitových ASCII znaků. Každá celá jedna z výsledných odpovídajících "zachytávacích skupin" (capture groups) je uložena do fragmentu na příslušném indexu (podle jedné z hodnot parametru `destinationIndexes`) do seznamu fragmentů specifikované cílové vstupní jednotky.
#### Parametry
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Výraz</th><td>Regulární výraz</td>
        <td><code>regex</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Cílová vst. jednotka</th><td></td>
        <td><code>destinationName</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Indexy cílových fragmentů</th><td></td>
        <td><code>destinationIndexes</code> (<tt>array[number]</tt>)</td>
    </tr>
</table>

### Korekční kód NMEA (`nmeacc`)
Vypočítá korekční kód uvedených fragmentů pomocí logické funkce XOR. Pokud *obvyklý parametr* "Index zdroje" není zadán, funkce je spuštěna na celém seznamu fragmentů vybrané vstupní jednotky.
#### Parametry
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Cílová vst. jednotka</th><td></td>
        <td><code>destinationName</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Indexy cílových fragmentů</th><td></td>
        <td><code>destinationIndex</code> (<tt>number</tt>)</td>
    </tr>
</table>

## Výstupní jednotky
Výstupní jednotka má za úkol posbírat fragmenty z jednotlivých vstupních jednotek a poskládat je do nových zpráv, které budou odeslány do CAN nebo Ethernet sítě (podle typu jednotky). Typ jednotky je vybrán podle typu [routy](/Format/Routes.md) ve které, aktuální konverter používáte.  
>Výstupní jednotky používají stejné *obvyklé parametry* jako jednotky akce.

### Formátovaný výpis do TCP/UDP (`printf`)
Interpretuje data z každého specifikovaného fragmentu jako pole 8-bitových ASCII znaků a použije funkci `printf` pro výpis do jiného fragmentu.
#### Parametry
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Indexy vst. fragmentů</th><td>Seznam indexů fragmetů, které budou předány funkci <code>printf</code>. (tento parametr nahrazuje běžný parametr <code>inputIndex</code>)</td>
        <td><code>inputIndexes</code> (<tt>array[number]</tt>)</td>
    </tr>
    <tr>
        <th>Formát</th><td>Formátovací řetězec</td>
        <td><code>format</code> (<tt>string</tt>)</td>
    </tr>
</table>

### Odeslání na CAN (`cansend`)
#### Parametry
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>PGN</th><td>Hexadecimální číslo, PGN výstupní CAN zprávy</td>
        <td><code>pgn</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Index prvního bitu</th><td></td>
        <td><code>firstBitIndex</code> (<tt>number</tt>)</td>
    </tr>
    <tr>
        <th>Datový typ</th><td>Datový typ výstupu (ovlivní celkový počet bitů). Pokud je zadán <tt>char</tt>, vstupní znak bude převeden na ASCII kód.</td>
        <td><code>dataType</code> (<tt>uchar | uint4 | int4 | uint8 | int8 | uint16 | int16 | uint32 | int32 | str</tt>)</td>
    </tr>
</table>

## 🧺 Globální úložistě
Někdy je potřeba uložit výstup z vícero zpráv (vícero *rout*) na jedno místo, a počkat na správnou chvíli, kdy ho bude možné odeslat. V takovém případě je možné použít v parametrech jakékoliv akční jednotky název "global názevúložiště" a tak nebude výstup ukládán do vst. jednotky ale do globálního úložiště s názvem `názevúložiště`.  
Aby bylo možné počkat na správnou chvíli a pak odeslat "celou zprávu", můžete u některé `Výstpní jednotky` použít parametr `waitfor`, který vám dovolí čekat na zápis vlajky, které můžou akční nebo vstupní jednotky zapisovat pomocí parametru `setflag`.

Filozofie vlajek je odvozena od bitového maskování (každá vlajka je jiná mocnina dvojky). Pokud tedy chcete získat data ze tří rout, v jedné může některá jednotka nastavit vlajku `1`, v druhé `2` a ve třetí `4`. V parametru `waitfor` pak bude třeba čekat na číslo `7` (1+2+4).
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Vztyč vlajku</th><td>Číslo vlajky, mocnina dvojky</td>
        <td><code>setflag</code> (<tt>number</tt>)</td>
    </tr>
    <tr>
        <th>Čekej na vlajky</th><td>Součet hodnot vlajek, ne které chcete čekat</td>
        <td><code>waitfor</code> (<tt>number</tt>)</td>
    </tr>
</table>


## Formát scanf a printf
Použijte wikipedii
- [printf - CZ](https://cs.wikipedia.org/wiki/Printf#Form%C3%A1tovac%C3%AD_%C5%99et%C4%9Bzec)
- [scanf - EN](https://en.wikipedia.org/wiki/Scanf_format_string#Format_string_specifications)
> POZOR! Funkce scanf nedokáže rozpoznat jiné oddělovače následující za `řetězcem`, než mezeru. např. `%s, %d` se nebude chovat podle očekávání. Použijte raději jednotky typu `separator` nebo `regex`.