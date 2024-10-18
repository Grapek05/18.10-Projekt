# Komenda awk

## Działanie
Służy do wyszukiwania wierszy w plikach, które są zgodne ze wzorcem i wykonuje określone działania na tych wierszach.

## Opis
Komenda **awk** używa zestawu instrukcji dostarczonych przez użytkownika w celu porównania zestawu plików, jednego wiersza w danym momencie, do rozszerzonych wyrażeń regularnych dostarczanych przez użytkownika. Następnie działania są wykonywane na dowolnym wierszu, który jest zgodny z rozszerzonym wyrażeniem regularnym.


## Dane wejściowe dla komendy awk
Komenda **awk** pobiera dwa typy danych wejściowych: wejściowe pliki tekstowe i instrukcje programu.

**Wejściowe pliki tekstowe**

Wyszukiwanie i działania są wykonywane na wejściowych plikach tekstowych. Pliki są określone przez:

-   Określenie zmiennej _Plik_ w wierszu komend.
-   Modyfikowanie zmiennych specjalnych **ARGV** i **ARGC**.
-   Podanie standardowego wejścia w przypadku braku zmiennej _Plik_ .

Jeśli za pomocą zmiennej _Plik_ określono wiele plików, pliki są przetwarzane w podanej kolejności.

**Instrukcje dotyczące programu**

Instrukcje udostępniane przez użytkownika sterują działaniami komendy **awk** . Instrukcje te pochodzą z zmiennej _` Program_' w wierszu komend lub z pliku określonego za pomocą opcji **-f** razem z zmienną _ProgramFile_ . Jeśli określono wiele plików programu, pliki są konkatenowane w podanej kolejności, a wynikowa kolejność instrukcji jest używana.

## Dane wyjściowe komendy awk

Komenda **awk** generuje trzy typy danych wyjściowych z danych wejściowych w wejściowym pliku tekstowym:

-   Wybrane dane mogą być drukowane na standardowym wyjściu, bez zmiany pliku wejściowego.
-   Wybrane fragmenty pliku wejściowego mogą zostać zmienione.
-   Wybrane dane można zmieniać i drukować na standardowe wyjście, z modyfikacją lub bez zmiany zawartości pliku wejściowego.

Wszystkie te typy danych wyjściowych mogą być wykonywane w tym samym pliku. Język programowania rozpoznawany przez komendę **awk** pozwala użytkownikowi na przekierowanie danych wyjściowych.

## Przetwarzanie plików z rekordami i polami

Pliki są przetwarzane w następujący sposób:

1.  Komenda **awk** skanuje swoje instrukcje i wykonuje wszystkie działania określone w celu wystąpienia przed odczytaniem pliku wejściowego.
    
    Instrukcja **POCZĄTEK** w języku programowania **awk** umożliwia użytkownikowi określenie zestawu instrukcji, które mają zostać wykonane przed przeczytaniem pierwszego rekordu. Jest to szczególnie przydatne przy inicjowaniu zmiennych specjalnych.
    
2.  Jeden rekord jest odczytyowany z pliku wejściowego.
    
    Rekord jest zestawem danych oddzielonych separatorem rekordów. Wartością domyślną dla separatora rekordów jest znak nowego wiersza, który powoduje, że każdy wiersz w pliku jest osobnym rekordem. Separator rekordów można zmienić, ustawiając specjalną zmienną **RS** .
    
3.  Rekord jest porównywany z każdym wzorcem określonym przez instrukcje komendy **awk** .
    
    W instrukcji komend można określić, że w rekordzie porównywane są konkretne pole. Domyślnie pola są rozdzielane białymi znakami (odstępami lub tabulatorami). Każde pole jest przywołane przez zmienną pola. Do pierwszego pola w rekordzie przypisywany jest zmienna **$1** , drugim polem jest przypisywany zmienna **$2** , itd. Cały rekord jest przypisywany do zmiennej **$0** . Separator pól można zmienić, korzystając z opcji **-F** w wierszu komend lub ustawiając **System plików** zmienna specjalna=rozszerzone wyrażenie regularne
    
4.  Jeśli rekord jest zgodny ze wzorcem, wszystkie działania powiązane z tym wzorcem są wykonywane w rekordzie.
5.  Gdy rekord jest porównywany z każdym wzorcem i wykonywane są wszystkie określone działania, następny rekord jest odczytyowany z danych wejściowych. Proces jest powtarzany do momentu, gdy wszystkie rekordy zostaną odczytane z pliku wejściowego.
6.  Jeśli określono wiele plików wejściowych, następny plik zostanie otwarty, a proces zostanie powtórzony do momentu odczytania wszystkich plików wejściowych.
7.  Po odczytaniu ostatniego rekordu w ostatnim pliku komenda **awk** wykonuje wszystkie określone instrukcje, które mają zostać wykonane po przetwarzaniu danych wejściowych.
    
    Instrukcja **KONIEC** w języku programowania **awk** umożliwia użytkownikowi określenie działań, które mają zostać wykonane po odczytaniu ostatniego rekordu. Jest to szczególnie przydatne przy wysyłaniu komunikatów o tym, jakie prace zostały wykonane za pomocą komendy **awk** .
   
## Przykłady
-   Aby wyświetlić wiersze pliku o długości większej niż 72 znaki, należy wpisać:
    
    ```plaintext-ibm
    awk  'length  >72'  chapter1
    ```
    
-   Powoduje wybranie każdej linii.chapter1Plik ten jest dłuższy niż 72 znaki i zapisuje te wiersze na standardowym wyjściu, ponieważ nie określono _Działanie_ . Znak tabulacji jest liczony jako 1 bajt.
    
-   Aby wyświetlić wszystkie wiersze między słowami startzapewnienia odpornościstop,włączanie "start"zapewnienia odporności "stop",wprowadź:
    
    ```plaintext-ibm
    awk  '/start/,/stop/'  chapter1
    ```
    

-   Aby uruchomić program komendy **awk** ,sum2.awk,który przetwarza plik,chapter1, wpisz:
    
    ```plaintext-ibm
    awk  -f  sum2.awk  chapter1
    ```
    

Następujący program:sum2.awk, wylicza sumę i średnią liczb w drugiej kolumnie pliku wejściowego,chapter1:

```plaintext-ibm
    {
       sum += $2
    }
END {
       print "Sum: ", sum;
       print "Average:", sum/NR;
    }
```

-   Pierwsze działanie powoduje dodanie wartości drugiego pola każdego wiersza do zmiennej.sum. Wszystkie zmienne są inicjowane do wartości liczbowej 0 (zero), gdy jest ona przywoływana po raz pierwszy. Wzorzec **KONIEC** przed drugim działaniem powoduje, że działania te zostaną wykonane po odczytaniu wszystkich plików wejściowych. Zmienna specjalna **Brak odpowiedzi** , używana do obliczania średniej, jest zmienną specjalną określającą liczbę odczytanych rekordów.
    
-   Aby wydrukować dwa pierwsze pola w odwrotnej kolejności, wpisz:
    
    ```plaintext-ibm
    awk '{ print $2, $1 }' chapter1
    ```
    

-   Następujący program **awk**
    
    ```plaintext-ibm
    awk -f sum3.awk chapter2
    ```
    

drukuje pierwsze dwa pola zbioruchapter2z polami wejściowymi oddzielonymi przecinkami i/lub spacjami i tabulatorami, a następnie dodaje pierwszą kolumnę, a następnie drukuje sumę i średnią:

```plaintext-ibm
BEGIN  {FS = ",|[ \t]+"}
       {print $1, $2}
       {s += $1}
END    {print "sum is",s,"average is", s/NR }               
```
****Przykład2:****

Przyjmij poniższy plik tekstowy jako plik wejściowy dla wszystkich poniższych przypadków:
```
$cat > employee.txt
```
```
konto menedżera ajay 45000   
konto urzędnika sunil 25000   
konto menedżera varun 50000   
konto menedżera amit 47000 konto urzędnika   
tarun 15000   
sprzedaż urzędnika deepak 23000   
sprzedaż urzędnika sunil 13000   
zakup dyrektora satvik 80000
```
****1. Domyślne zachowanie Awk:**** Domyślnie Awk drukuje wszystkie wiersze danych ze wskazanego pliku.
```
$ awk '{print}' pracownik.txt
```
****Wyjście:****
```
konto menedżera ajay 45000   
konto urzędnika sunil 25000   
konto menedżera varun 50000   
konto menedżera amit 47000 konto urzędnika   
tarun 15000   
sprzedaż urzędnika deepak 23000   
sprzedaż urzędnika sunil 13000   
zakup dyrektora satvik 80000
```
W powyższym przykładzie nie podano żadnego wzorca. Akcje są więc stosowane do wszystkich wierszy. Akcja print bez żadnego argumentu domyślnie drukuje cały wiersz, więc drukuje wszystkie wiersze pliku bez błędu.

****Przykład3:****
****Użycie wbudowanych zmiennych NR (Wyświetl numer wiersza)****
```
$ awk '{print NR,$0}' pracownik.txt
```
****Wyjście:****
```
1 konto menedżera ajay 45000   
2 konto urzędnika sunil 25000   
3 konto menedżera varun 50000   
4 konto menedżera amit 47000   
5 konto pracownika tarun 15000   
6 konto urzędnika deepak 23000   
7 konto pracownika sunil 13000   
8 zakup dyrektora satvik 80000
```
W powyższym przykładzie polecenie awk z NR drukuje wszystkie wiersze wraz z numerem wiersza.

****Użycie wbudowanych zmiennych NF (Wyświetl ostatnie pole)****
```
$ awk '{print $1,$NF}' pracownik.txt
```
****Wyjście:****
```
ajay 45000   
sunil 25000   
varun 50000   
amit 47000   
tarun 15000   
deepak 23000   
sunil 13000   
satvik 80000
```
W powyższym przykładzie $1 reprezentuje Name, a $NF reprezentuje Salary. Salary możemy uzyskać za pomocą $NF, gdzie $NF reprezentuje ostatnie pole.

****Inne zastosowanie wbudowanych zmiennych NR (Wyświetlanie wierszy od 3 do 6)****
```
$ awk 'NR==3, NR==6 {print NR,$0}' employee.txt
```
****Wyjście:****
```
3 varun manager sprzedaż 50000   
4 amit manager konto 47000   
5 tarun peon sprzedaż 15000   
6 deepak clerk sprzedaż 23000
```

***Link do przykładów z zastosowania AWK w PDF:***

- [https://ai.ia.agh.edu.pl/_media/pl:dydaktyka:unix:awk1.pdf](https://nayma.pl)


