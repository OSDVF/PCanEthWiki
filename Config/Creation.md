# Vytváření konfiguračních souborů
Soubory jsou formátovány v jazyce JSON, můžete je psát ručně, nebo k tomu použít vizuální editor EthCanConfig.

## Otevírání souborů
Pro otevření existujícího konfiguračního souboru použijte položku nabídky "File->Open".
Pro vytvoření nového konfiguračního souboru použijte "File->New". Soubor nebude nikdy automaticky uložen. Pro **uložení** použijte položku "File->Save" nebo klávesovou zkraktu <kbd>Ctrl+S</kbd>.

## Struktura
Konfigurace obsahuje **strom** konfiguračních *nastavení*, z nichž každé nese informaci o určitém aspektu činnosti převodníku. Začněte tím, že vytvoříte *"vyšší" nastavení*, které vše specifikují jen hrubě a postupně vytvářením jejich vnořených *nastavení* popíšete celou činnost převodníku.

Aktuální nastavení vidíte v různých sloupcích v hlavní části okna programu.
Pro přidání *nastavení* použijte řádky se symbolem **+**.

Když kliknete na jedno existující nastavení, v dalším sloupci uvidíte konfigurační hodnoty a vnořená *nastavení*, které obsahuje.

[Seznam existujících *nastavení* najdete zde](/Format/Main.md)