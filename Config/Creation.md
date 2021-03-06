# Vytváření konfiguračních souborů
Soubory jsou formátovány v jazyce JSON, můžete je psát ručně, nebo k tomu použít vizuální editor EthCanConfig.

## Otevírání souborů
Pro otevření *existujícího* konfiguračního souboru použijte položku nabídky "File->Open".
Pro vytvoření *nového* konfiguračního souboru použijte "File->New". Soubor nebude nikdy automaticky uložen. Pro **uložení** použijte položku "File->Save" nebo klávesovou zkraktu <kbd>Ctrl+S</kbd>.

## Filozofie
EthCanRouter dokáže
- přijmout vstup z některého z vstupních rozhraní
- filtrovat jen chtěné zprávy
- provést na vstupní zprávě různé sekvenční operace (tzv. konverze)
- uložit ji pro pozdější použití
- čekat na výskyt jiné zprávy
- odeslat

## Struktura
Konfigurace obsahuje **strom** konfiguračních *nastavení*, z nichž každé nese informaci o určitém aspektu činnosti převodníku.

![Screenshot](screenshot.png)
Aktuálně přidaná nastavení vidíte jako klikatelný strom v levé části okna programu.
Pro přidání *nastavení* použijte řádky se symbolem **+**.

Ze začátku uvidíte jen *"vyšší"* nastavení, která specifikují vlastnosti převodu jen hrubě. Vždy napravo od jména nastavení se nachází textové/rozbalovací pole, do kterého můžete napsat požadovanou hodnotu.

Až budete mít *"vyšší"* nastavení nastavena, můžete rozkliknout ta, která mají nalevo od názvu šipku **>** a projít podrobnější vlastnosti.

[Seznam možných *nastavení* najdete zde](/Format/Main.md)