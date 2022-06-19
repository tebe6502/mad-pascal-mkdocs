#

## [Pliki zasobów](/skladnia/#r-resource)

### Składnia plików RC

Pliki `RC` są zwykłymi plikami tekstowymi. Zawierają listę zasobów, które należy włączyć do kompilowanego pliku.
Podstawowy element składni wygląda następująco:

	NazwaZasobu  TypZasobu  NazwaPliku  [0 0 0 0 0 0 0 0]

	RCLABEL      RCTYPE     RCFILE      [PAR0, PAR1, PAR2, PAR3, PAR4, PAR5, PAR6, PAR7]

W treści pliku `RC` mogą się znaleźć komentarze, poprzedzone znakiem ';' lub '#'. Przykład pliku `RC`:

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
| EXTMEM     | Dowolny typ danych ładowany do pamięci dodatkowej `PORTB`, adres ładowania ustalany jest na podstawie `RCLABEL`|
| RCASM      | Plik w assemblerze, który zostanie dołączony i zasemblowany.                                           |
| DOSFILE    | Plik z nagłówkiem **Atari DOS**, adres ładowania takiego pliku powinien być identyczny jak `RCLABEL`       |
| RELOC      | Plik relokowalny w formacie **Mad Assemblera**, plik zostanie poddany relokacji pod wskazany adres `RCLABEL`|
| RMT        | Plik modułu **Raster Music Tracker-a**, plik zostanie poddany relokacji pod wskazany adres `RCLABEL`       |
| MPT        | Plik modułu **Music ProTracker-a**, plik zostanie poddany relokacji pod wskazany adres `RCLABEL`           |
| CMC        | Plik modułu **Chaos Music Composer-a**, plik zostanie poddany relokacji pod wskazany adres `RCLABEL`       |
| RMTPLAY    | Player dla modułu **RMT**, jako `RCFILE` podajemy plik *.FEAT oraz dodatkowo `PAR0` tryb playera 0..3:     |
|            | 0 => compile RMTplayer for 4 tracks mono                                                               |
|            | 1 => compile RMTplayer for 8 tracks stereo                                                             |
|            | 2 => compile RMTplayer for 4 tracks stereo L1 R2 R3 L4                                                 |
|            | 3 => compile RMTplayer for 4 tracks stereo L1 L2 R3 R4                                                 |
| MPTPLAY    | Player dla modułu **MPT**, nie wymaga podawania nazwy pliku `RCFILE`                                       | 
| CMCPLAY    | Player dla modułu **CMC**, nie wymaga podawania nazwy pliku `RCFILE`                                       |
| XBMP       | Plik **Windows Bitmap** (8 BitsPerPixel) ładowany do pamięci **VBXE** pod wskazany adres `RCLABEL` od indeksu koloru `PAR0` w palecie kolorów **VBXE** nr 1|

&nbsp;
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


### Dostęp do zasobów.

Zasoby umieszczane są pod wskazanymi `RCLABEL` adresami w pamięci. Jedynym wyjątkiem jest typ zasobu `RCDATA` dla którego możliwe jest pominięcie definicji `RCLABEL` w kodzie programu.

W przypadku braku definicji `RCLABEL` zasób zostaje dołączony do kompilowanego programu, dostęp możliwy jest poprzez procedurę `GetResourceHandle`.

	GetResourceHandle(pointer, 'rclabel');

Procedura `GetResourceHandle` ustala wartość wskaźnika POINTER dla zasobu 'RCLABEL'.
