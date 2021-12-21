#

## Mapa pamięci

Atari XE/XL

Kompilator korzysta ze strony zerowej w przedziale $0080 .. $00D7

W przypadku korzystania z pamięci dodatkowej tablica z kodami banków dla `PORTB` umieszczona zostaję w przestrzeni adresowej $0101 .. $0140

Dodatkowy 256 bajtowy bufor pamięci wykorzystywany do operacji na ciągach znakowych ulokowany jest w przedziale $0400 .. $04FF

+ STACKORIGIN

+ STATICDATA

+ RUNTIME LIBRARY

+ UNIT INITIALIZATIONS

+ MAIN PROGRAM
	+ .LOCAL MAIN (all procedures / functions)
	+ .LOCAL @DEFINES
	+ .LOCAL @RESOURCE

+ DATAORIGIN

+ PROGRAMSTACK