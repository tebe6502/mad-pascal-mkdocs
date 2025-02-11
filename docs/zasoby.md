#

## [Pliki zasobów](/skladnia/#r-resource)

### Składnia plików RC

Pliki `RC` są zwykłymi plikami tekstowymi. Zawierają listę zasobów, które należy włączyć do kompilowanego pliku.
Podstawowy element składni wygląda następująco:

	NazwaZasobu  TypZasobu  NazwaPliku  [0 0 0 0 0 0 0 0]

	RCLABEL      RCTYPE     RCFILE      [PAR0, PAR1, PAR2, PAR3, PAR4, PAR5, PAR6, PAR7]

W treści pliku `RC` mogą znaleźć się komentarze, poprzedzone znakiem `;` lub `#`. Przykład pliku `RC`:

```delphi
	; to jest player MPT
	mpt_player	MPTPLAY
	
	# to jest modul
	mpt_modul	MPT	'porazka.mpt'

	dane	RCDATA 'dane.xex' 6
```

Typ zasobu określa format włączanego pliku.

| Typ zasobu | Opis                                                                                                   |
|:----------:|--------------------------------------------------------------------------------------------------------|
| RCDATA     | Dowolny typ danych, np.:                                                                               |
|            | label RCDATA 'filename'                                                                                |
|            | label RCDATA 'filename' OFFSET                                                                         |
| EXTMEM     | Dowolny typ danych ładowany do pamięci dodatkowej `PORTB`, adres ładowania ustalany <br> jest na podstawie `RCLABEL`|
| RCASM      | Plik w assemblerze, który zostanie dołączony i zasemblowany (makra nie są dozwolone).                  |
| DOSFILE    | Plik z nagłówkiem **Atari DOS**, adres ładowania takiego pliku powinien być identyczny <br> jak `RCLABEL`       |
| LIBRARY    | Skompilowany moduł `LIBRARY`, ładowany do banku `RCLABEL`   |
| RELOC      | Plik relokowalny w formacie **Mad Assemblera**, plik zostanie poddany relokacji pod <br> wskazany adres `RCLABEL`|
| RMT        | Plik modułu **Raster Music Tracker-a**, plik zostanie poddany relokacji pod wskazany <br> adres `RCLABEL`       |
| MPT        | Plik modułu **Music ProTracker-a**, plik zostanie poddany relokacji pod wskazany <br> adres `RCLABEL`           |
| CMC        | Plik modułu **Chaos Music Composer-a**, plik zostanie poddany relokacji pod wskazany <br> adres `RCLABEL`       |
| SAPR       | Plik z danymi SAP-R, ładowany pod wskazany adres `RCLABEL`    |
| PP         | Plik spakowny **Power Packer**-em, ładowany pod wskazany adres `RCLABEL`  |
| RMTPLAY    | Player dla modułu **RMT**, ładowany na początek strony pamięci od adresu `RCLABEL`,<br>jako `RCFILE` podajemy plik `.FEAT`, `PAR0` tryb playera 0..3:  |
|            | 0 => compile RMTplayer for 4 tracks mono                                                               |
|            | 1 => compile RMTplayer for 8 tracks stereo                                                             |
|            | 2 => compile RMTplayer for 4 tracks stereo L1 R2 R3 L4                                                 |
|            | 3 => compile RMTplayer for 4 tracks stereo L1 L2 R3 R4                                                 |
|            | oraz opcjonalnie `PAR1` jako adres dla zmiennych na stronie zerowej (domyślnie `$E0`)                  |
|            | 
| RMTPLAY2   | Player dla modułu **RMT**, ładowany na początek strony pamięci od adresu `RCLABEL`,<br>jako `RCFILE` podajemy plik `.FEAT`, `PAR0` tryb playera 0..3:  |
|            | 0 => compile RMTplayer for 4 tracks mono                                                               |
|            | 1 => compile RMTplayer for 8 tracks stereo                                                             |
|            | 2 => compile RMTplayer for 4 tracks stereo L1 R2 R3 L4                                                 |
|            | 3 => compile RMTplayer for 4 tracks stereo L1 L2 R3 R4                                                 |
|            | oraz opcjonalnie `PAR1` jako adres dla zmiennych na stronie zerowej (domyślnie `$E0`)                  |
| SAPRPLAY   | Player SAP-R LZSS, nie wymaga podawania nazwy pliku `RCFILE`, <br> adres `RCLABEL` tylko od początku strony |
| MPTPLAY    | Player dla modułu **MPT**, nie wymaga podawania nazwy pliku `RCFILE`                                       | 
| CMCPLAY    | Player dla modułu **CMC**, nie wymaga podawania nazwy pliku `RCFILE`                                       |
| XBMP       | Plik **BMP** **`Windows Bitmap`** (8 BitsPerPixel) ładowany do pamięci **VBXE** <br> pod wskazany adres `RCLABEL`, <br> od indeksu koloru `PAR0` -> `color select`, <br> w palecie kolorów **VBXE** `PAR1` -> `palette select` |

### Możliwość ładowania zasobów pod ROM

        CMC             RAM / ROM
        CMCPLAY         RAM / ROM
        DOSFILE         RAM / ROM
        EXTMEM
        LIBRARY         PORTB
        MPT             RAM / ROM
        MPTPLAY         RAM / ROM
        PP              RAM / ROM
        RCASM           RAM / ROM
        RCDATA          RAM / ROM
        RELOC           RAM
        RMT             RAM / ROM
        RMTPLAY         RAM
        RMTPLAY2        RAM / ROM
        XBMP
        SAPR            RAM / ROM
        SAPRPLAY        RAM / ROM

### Włączenie do aplikacji pliku RC

W kodzie źródłowym programu należy wpisać dyrektywę kompilatora (np. na początku sekcji implementacji):

	{$R mojezasoby.rc}

Dodatkowo należy podać w kodzie programu wartość dla etykiet `RCLABEL` odpowiednich zasobów, np.:

```delphi
const

    mpt_player = $8000;

    mpt_modul = $9000;
```

Włączenie pliku `RC` następuje w momencie kompilacji programu.

Jeśli adres zasobu wskazuje adres pod ``ROM`` (``$C000..$FFFF``) wówczas wyłączany jest ``ANTIC``. Przy uruchomieniu programu należy wpisać odpowiednią wartość do rejestru ``DMACTL``
aby włączyć obraz z powrotem.


### Dostęp do zasobów.

Zasoby umieszczane są pod wskazanymi `RCLABEL` adresami w pamięci. Wyjątkiem są zasoby `RCDATA`, `SAPR`, `PP` dla których możliwe jest pominięcie definicji `RCLABEL` w kodzie programu.

W przypadku braku definicji `RCLABEL` zasób zostaje dołączony do kompilowanego programu, dostęp możliwy jest poprzez procedurę `GetResourceHandle`.

	GetResourceHandle(pointer, 'rclabel');

Procedura `GetResourceHandle` ustala wartość wskaźnika **POINTER** dla zasobu `RCLABEL`.

	SizeOfResource(variable, 'rclabel');

Procedura `SizeOfResource` zwraca długość zasobu `RCLABEL` w zmiennej **VARIABLE**.

### RCASM

> _W zasobach `ASM` nie ma możliwości używania makr._

### SAPRPLAY

Player SAP-R LZSS wymaga podania adresu od początku strony pamięci. Główny kod playera zajmuje **$0300** bajtów, bufory **$0900**, w sumie **$0C00** bajtów.

**SAP-R** jest to plik z kolejnymi wartościami rejestrów **POKEY**-a (`$D200..$D208`). Zapis pliku **SAP-R** umożliwia emulator **Altirra**, lub program [**RMT2LZSS**](https://forums.atariage.com/topic/315537-rmt2lzss-convert-rmt-tunes-to-lzss-for-fast-playback).
