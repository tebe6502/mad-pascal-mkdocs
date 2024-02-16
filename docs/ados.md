# Alternatywa dla DOS-a

## [foxDOS](https://github.com/tebe6502/Mad-Assembler/blob/master/examples/foxdos.asm)

* instaluje urządzenie D: jak każdy inny DOS Atari
* przy starcie wczytuje plik uruchamialny o nazwie AUTORUN
* obsługuje standardowy system plików DOS 2
* obsługiwany rozmiar sektora (128 lub 256 bajtów) jest ustalany na etapie kompilacji foxDOS-a
* foxDOS umożliwia odczyt pliku przez D:
* obsługuje funkcję BINARY LOAD, np. XIO 40,#1,0,0,”D:FILE.EXE”
* foxDOS mieści się w całości w sektorach odczytu wstępnego (boot sectors)
* foxDOS nie ustawia MEMLO, ale zajmuje tylko obszar pamięci $0700..$097F
* foxDOS nie wyłącza ROM-u podczas transmisji

ograniczenia:

* jednocześnie można czytać tylko jeden plik, ale może on być dowolnej długości
* umożliwia nadpisanie istniejącego pliku mieszczącego się w tylko jednym sektorze
* inne operacje, jak odczyt katalogu, kasowanie, zmiana nazwy itd. nie są obsługiwane

```
copy filename.obx disk\autorun.

dir2atr.exe -md -B foxdos.obx disk.atr disk\ 
```

## [xBootDOS](https://xxl.atari.pl/xbootdos/)

* MEMLO = $980, nie używa strony $04xx,$05xx ani $06x
* po wczytaniu automatycznie uruchamia program zapisany pod nazwą „AUTO”
* automatycznie konfiguruje się do odpowiedniej wielkości sektora (128/256) SD/ED/DD oraz wersji systemu plików DOS2 (rozmiar obrazu dysku do 16 MB)
* obsługuje także systemy BiboDOS i TopDOS z 64/128 wpisami katalogowymi
* pozwala nadpisywać istniejące pliki
* obsługuje funkcję BINARY LOAD, np. XIO 40,#1,0,0,”D:FILE.EXE”
* dodatkowe komendy NOTE i POINT znajdują się w pliku xBDext (mogą być załadowane automatycznie – zmień nazwę na AUTO)

ograniczenia:

* jednocześnie można czytać tylko jeden plik, ale może on być dowolnej długości
* inne operacje, jak odczyt katalogu, kasowanie, zmiana nazwy itd. nie są obsługiwane

```
copy filename.obx disk\autorun.

dir2atr.exe -md -B xBootDOS.obx disk.atr disk\ 
```

## [dir2atr](https://www.horus.com/~hias/atari/)

Program dla Windows przy pomocy którego można tworzyć obrazy dyskietek **Atari XE/XL** `ATR`
