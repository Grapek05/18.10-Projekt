# Polecenie paste

Polecenie paste służy do poziomego łączenia plików (scalanie równoległe). Jeśli nie określono żadnego pliku lub wstawiono myślnik („-”) zamiast nazwy pliku, paste odczyta ze standardowego wejścia i wyświetli wynik w niezmienionej postaci, aż do wydania polecenia przerwania [Ctrl-c]. 

- __Składnia:__ 

```
					paste [OPTION]... [FILES]...
```

Rozważmy trzy pliki posiadające nazwę __state__, __capital__ i __number__. Pliki __state__ i __capital__ zawierają odpowiednio 5 nazw stanów i stolic Indii. Plik __number__ zawiera 5 cyfr.

```
$ cat state  
Arunachal Pradesh  
Assam  
Andhra Pradesh  
Bihar  
Chhattisgrah  
  
$ cat capital  
Itanagar  
Dispur  
Hyderabad  
Patna  
Raipur
```

Bez żadnej opcji, polecenie paste łączy pliki równolegle.

```
$ paste number state capital
1       Arunachal Pradesh       Itanagar  
2       Assam   Dispur  
3       Andhra Pradesh  Hyderabad  
4       Bihar   Patna  
5       Chhattisgrah    Raipur
```

W powyższym poleceniu trzy pliki są łączone poleceniem wklejenia. Polecenie paste zapisuje na terminalu odpowiednie linie z plików z użyciem tabulatora jako oddzielenia

## Opcje

1. Opcja -d, -delimiters umożliwia określenie listy znaków, które mają być używane jako oddzielenie zamiast domyślnego separatora TAB.

- __Tylko jeden znak jest określony:__

```
$ paste -d "|" number state capital
1|Arunachal Pradesh|Itanagar  
2|Assam|Dispur  
3|Andhra Pradesh|Hyderabad  
4|Bihar|Patna  
5|Chhattisgrah|Raipur
```

- __Określonych znaków jest więcej niż jeden:__

```
$ paste -d "|," number state capital  
1|Arunachal Pradesh,Itanagar  
2|Assam,Dispur  
3|Andhra Pradesh,Hyderabad  
4|Bihar,Patna  
5|Chhattisgrah,Raipur
```
Pierwszy i drugi plik są oddzielone znakiem „|” a drugi i trzeci są oddzielone znakiem „,”.

2. Opcja -s, --serial. Możemy łączyć pliki sekwencyjnie, używając opcji -s. Odczytuje wszystkie linie z pojedynczego pliku i łączy je w jedną linię, przy czym każda linia jest oddzielona tabulatorem.

```
$ paste -s number state capital
1       2       3       4       5  
Arunachal Pradesh       Assam   Andhra Pradesh  Bihar   Chhattisgrah  
Itanagar        Dispur  Hyderabad       Patna   Raipur
```

W powyższym poleceniu najpierw odczytuje dane z pliku __number__ i łączy je w jedną linię, przy czym każda linia jest oddzielona tabulatorem. Po przejściu do nowego wiersza zaczyna odczytywać następny plik (__state__). Proces powtarza się od nowa, aż do odczytania wszystkich plików.

- __Kombinacja -d i -s:__

```
$ paste -s -d ":" number state capital
1:2:3:4:5  
Arunachal Pradesh:Assam:Andhra Pradesh:Bihar:Chhattisgrah  
Itanagar:Dispur:Hyderabad:Patna:Raipur
```

3. –version: Ta opcja służy do wyświetlenia wersji paste, która jest aktualnie uruchomiona w twoim systemie.

```
$ paste --version  
paste (GNU coreutils) 8.26  
Packaged by Cygwin (8.26-2)  
Copyright (C) 2016 Free Software Foundation, Inc.  
License GPLv3+: GNU GPL version 3 or later .  
This is free software: you are free to change and redistribute it.  
There is NO WARRANTY, to the extent permitted by law.
```

## Zastosowania polecenia paste

1. __Łączenie kolejnych linii:__ Polecenie paste może być również użyte do połączenia kolejnych linii z pliku w jedną linię poprzez wpisywanie łączników (-)

- __Z dwoma łącznikami:__

```
$ cat capital | paste - -
Itanagar        Dispur  
Hyderabad       Patna  
Raipur
```

- __Z trzema łącznikami:__

```
$ paste - - - < capital
Itanagar        Dispur  Hyderabad  
Patna   Raipur
```

2. __Połączenie z innymi poleceniami:__ Mimo że paste wymaga co najmniej dwóch plików do łączenia linii, dane z jednego pliku można przekazać z powłoki. Podobnie jak w przykładzie poniżej, polecenie cut jest używane z opcją -f do wycinania pierwszego pola pliku stanu, a dane wyjściowe są przesyłane za pomocą polecenia paste mającego jedną nazwę pliku i zamiast nazwy drugiego pliku podany jest łącznik. Jeśli nie określono łącznika, dane wejściowe z powłoki nie zostaną wklejone.

- __Bez łącznika:__

```
$ cut -d " " -f 1 state | paste number
1  
2  
3  
4  
5
```

- __Z łącznikiem:__

```
$ cut -d " " -f 1 state | paste number -
1       Arunachal  
2       Assam  
3       Andhra  
4       Bihar  
5       Chhattisgrah
```

Kolejność wklejania można zmienić zmieniając położenie łącznika:

```
$ cut -d " " -f 1 state | paste - number
Arunachal       1  
Assam   2  
Andhra  3  
Bihar   4  
Chhattisgrah    5
```
