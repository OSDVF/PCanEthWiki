# Nahrání softwaru
Pokud jste již [připojeni k převodníku](Connecting.md), tlačítkem **Device Info** zobrazíte informace o operačním systému a verzi EthCanRouteru, který je právě na převodníku přítomný.

Pokud na něm ještě EthCanRouter není, nebo je zastaralý (vzhledem k verzi, která je ukázaná v horním panelu vedle nápisu *Current version*), klikněte na tlačítko **Update Software**, čímž se do převodníku nahraje EthCanRouter a jeho spouštěcí skript. Pak proběhne restart převodníku (počkejte 60 sekund na opětovné připojení).

# Nahrání konfiguračního souboru
Konfigurační soubor je klasický textový soubor obsahující text ve formátu JSON. Můžete jej nahrát i manuálně (přes SSH a SCP), ale doporučujeme použít program EthCanConfig.

Pokud jste již [připojeni k převodníku](Connecting.md), tlačítkem **Upload Configuration File** nahrajete soubor do převodníku.
Po nahrání bude převodník restartován, počkejte asi 60 sekund.