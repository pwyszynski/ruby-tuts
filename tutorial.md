# Obsługa plików w języku Ruby


## Spis treści

 * tworzenie plików
 * otwieranie istniejących plików
 * zmiana nazwy i usuwanie plików
 * pobieranie informacji o plikach
 * czytanie i pisanie do plików

## Tworzenie nowych plików

Nowe pliki w Ruby tworzone są przez użycie metody `new` klasy `File`. Przyjmuje ona dwa argumenty, pierwszym jest nazwa pliku, drugim tryb w jakim ten plik jest obsługiwany. Poniższa tabelka pokazuje dostępne tryby:

| Tryb | Opis                  |
| :---:|-----------------------|
|   r  | odczyt (read)         |
|   w  | zapis (write)         |
|   a  | dostęp (access)       |
|   b  | wykonywanie (WIN/DOS) |

Mając to na uwadze, możemy utworzyć nowy plik w trybie do zapisu:

```ruby
File.new('foo.txt', 'w')
=> #<File:foo.txt>
```

## Otwieranie istniejących plików

Istniejące pliki mogą być otwarte za pomocą metody `open` klasy `File`:
```ruby
file = File.open('foo.txt')
=> #<File:foo.txt>
```

Zwróć uwagę, że istniejące pliki mogą być otwarte w różnych trybach, tak jak to wyróżniono w tabeli. Na przykład, możemy otworzyć plik w trybie tylko do odczytu:

```ruby
file = File.open('foo.txt', 'r')
=> #<File:temp.txt>
```

Możemy również sprawdzić, czy plik jest już otwarty za pomocą metody `closed?`:

```ruby
file.closed?
=> false
```

## Zamykanie pliku
Zamykamy plik używając metody `close`:

```ruby
file = File.open('foo.txt', 'r')
=> #<File:foo.txt>
file.close
=> nil
```

## Zmiana nazwy i usuwanie plików

Pliki w Ruby mają zmieniane nazwy i są usuwane odpowiednio za pomocą metod `rename` i `close`. Na przykład, możemy utworzyć nowy plik, zmienić jego nazwę, po czym usunąć go.

```ruby
File.new("foo.txt", "w")
=> #<File:foo.txt>

File.rename("foo.txt", "bar.txt")
=> 0

File.delete("bar.txt")
=> 1
```

## Pobieranie informacji o plikach
Operacje na plikach często wymagają więcej jak tylko ich otwieranie. Czasem musimy poznać więcej informacji na temat samego pliku zanim w ogóle zostanie on otwarty.

#### Sprawdzanie, czy plik istnieje

Żeby sprawdzić, czy plik istnieje, używamy metody `exists?`:

```ruby
File.exists?("foo.txt")
=> true
```

#### Czy plik to na pewno plik/katalog

Żeby sprawdzić, czy plik jest na pewno plikiem a nie np. katalogiem, używamy metody `file?`:

```ruby
File.file?("ruby")
=> false
```

Podobnie, żeby sprawdzić, czy jest to katalog używamy metody `directory?`:

```ruby
File.directory?("ruby")
=> true
```

#### Czy plik jest do odczytu, zapisywalny lub wykonywalny
Żeby sprawdzić, czy plik jest do odczytu, zapisywalny lub wykonywalny używamy odpowiednio metod `readable?`, `writable?`, `executable?`: 


```ruby
File.readable?("foo.txt")
=> true

File.writable?("bar.txt")
=> true

File.executable?("foo.txt")
=> false
```

#### Sprawdzanie rozmiaru pliku:

Do poznania rozmiaru pliku (w bajtach) służy metoda `size?`:

```ruby
File.size("temp.txt")
=> 99
```

#### Sprawdzanie, czy plik jest pusty
Sprawdzanie czy plik jest pusty (zerowa długość) wykonujemy metodą `zero?`:

```ruby
File.zero?("temp.txt")
=> false
```

#### Poznanie typu pliku
Metoda `ftype` zwraca typ pliku na którym jest użyta:

```ruby
File.ftype("temp.txt")
=> "file"

File.ftype("../ruby")
=> "directory"

File.ftype("/dev/sda5")
=> "blockSpecial"
```

#### Poznanie daty utworzenia, modyfikacji i dostępu

Odpowiednio za pomocą metod `ctime`, `mtime`, `atime`:

```ruby
File.ctime("temp.txt")
=> Thu Nov 29 10:51:18 EST 2007

File.mtime("temp.txt")
=> Thu Nov 29 11:14:18 EST 2007

File.atime("temp.txt")
=> Thu Nov 29 11:14:19 EST 2007
```


## Czytanie i pisanie do pliku

Po otwarciu już istniejącego pliku lub utworzeniu nowego jesteśmy w stanie go odczytać lub zapisywać do niego. Odczytywać możemy za pomocą metod `readline` lub `each`:

```ruby
myfile = File.open("foo.txt")
=> #<File:temp.txt>

myfile.readline
=> "To jest testowy plik\n"

myfile.readline
=> "Zawiera kilka testowych linijek\n"
```

Alternatywnie, możemy użyć metody `each`, żeby odczytać cały plik na raz:
```ruby
myfile = File.open("temp.txt")
=> #<File:temp.txt>

myfile.each {|line| print line }
To jest testowy plik
Zawiera kilka testowych linijek
Ale poza tym
Nie robi nic
```


Podobnie wygląda zapisywanie do pliku
```ruby
myfile = File.new("write.txt", "w+")    # otwarcie pliku w trybie read i write
=> #<File:write.txt>

myfile.puts("This test line 1")         # dopisanie linijki
=> nil

myfile.puts("This test line 2")         # dopisanie drugiej linijki
=> nil

myfile.rewind                           # przesuń wskaźnik na początek pliku, żeby można było odczytać cały
=> 0

myfile.readline
=> "This test line 1\n"

myfile.readline
=> "This test line 2\n"
```
