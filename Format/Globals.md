# 🧺 Globální úložistě
Někdy je potřeba uložit výstup z vícero zpráv (vícero *rout*) na jedno místo, a počkat na správnou chvíli, kdy ho bude možné najednou odeslat. V takovém případě je možné použít v parametrech jakékoliv akční jednotky název "global názevúložiště" a tak výstup nebude ukládán do vst. jednotky ale do tzv. globálního úložiště s názvem `názevúložiště`.  
Aby bylo možné počkat na správnou chvíli a pak odeslat "celou zprávu", můžete u některé `Výstpní jednotky` použít parametr `waitFor`, který vám dovolí čekat na vztyčení *vlajek*, které můžou vztyčit Listenery pomocí parametru `setFlag`. (viz [Routy](/Format/Routes.md))  
Vlajka je vždy nastavena "na nějakém úložišti", to znamená, že kromě `setFlag` je nutné také specifikovat `flagStore` - úložiště, ke kterému vlajka přísluší.

Výstupní jednotka, která obsahuje parametr `waitFor` pak čeká na požadouvanou vlajku právě **na globálním úložišti**, které je u ní specifikováno (větišnou parametrem `inputName`).

Filozofie vlajek je odvozena od bitového maskování (každá vlajka je jiná mocnina dvojky). Pokud tedy chcete získat data ze tří rout, v jedné může některá jednotka nastavit vlajku `1`, v druhé `2` a ve třetí `4`. V parametru `waitFor` pak bude třeba čekat na číslo `7` (1+2+4).
<table>
    <thead>
        <tr>
            <th>Název parametru</th><th>Význam</th><th>Kódové označení</th>
        </tr>
    </thead>
    <tr>
        <th>Vztyč vlajku</th><td>Číslo vlajky, mocnina dvojky - tento paramter se objevuje u listeneru</td>
        <td><code>setFlag</code> (<tt>number</tt>)</td>
    </tr>
    <tr>
        <th>Název globálního úložiště</th><td>Název globálního úložiště, kterému se vlajka nastaví - tento paramter se objevuje u listeneru</td>
        <td><code>flagStore</code> (<tt>string</tt>)</td>
    </tr>
    <tr>
        <th>Čekej na vlajky</th><td>Součet hodnot vlajek, ne které chcete čekat - parametr se objevuje u výst. jednotky</td>
        <td><code>waitFor</code> (<tt>number</tt>)</td>
    </tr>
</table>