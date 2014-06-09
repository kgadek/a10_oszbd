Tworzenie instancji

	>> IFIX=/c/Program\ Files/IBM/Informix/11.70

   * Utwórz nową instancję serwera bazodanowego:
   * Nazwa instancji (DBSERVERNAME) ol2014_xx, gdzie xx – twoje inicjały (dotyczy to całego zadania, podczas wykonywania kolejnych operacji xx pojawiające się w dokumencie powinno być zastąpione inicjałami autora)
   * Plik tworzący rootdbs powinien znajdować się w katalogu c:\ol_xx_data (gdzie xx – twoje inicjały), rozmiar rootdbs 200 MB
   * Włącz mirroring dla rootdbs, pliki mirrorujące rootdbs powinny znajdować się w katalogu c:\ol_xx_mirror (gdzie xx – twoje inicjały)
   * (dopuszczalna jest konfiguracja pozostawiająca pliki tworzące rootdbs w domyślnym miejscu)
   * Skonfiguruj interfejs dla połączeń sieciowych dla portu 9092
   * Zweryfikuj poprawność utworzenia instancji (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranów pokazujących rezultaty tych poleceń)

Konfiguracja połączeń

   * Skonfiguruj dodatkowy interfejs dla połączeń sieciowych dla poru 9094
   * Zweryfikuj czy serwer jest w trybie online, jeśli nie to przejdź do trybu online. (wykonaj odpowiednie polecenie, umieść w sprawozdaniu zrzut ekranu pokazujący rezultat tego polecenia)
   * Zweryfikuj możliwość połączenia przy pomocy obu skonfigurowanych interfejsów sieciowych (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)

Zarządzanie przestrzenią dyskową

   * Stwórz przestrzeń (dbspace) o nazwie danexx1. Rozmiar przestrzeni to 70 MB w dwóch chunkach o nazwach danexx1_1, danexx1_2 o rozmiarach 50MB, 20MB. Pliki umieść w katalogu c:\ol_xx_data. Nie mirroruj tej przestrzeni.
   * Stwórz przestrzeń (dbspace) o nazwie danexx2. Rozmiar przestrzeni to 90 MB w dwóch chunkach o nazwach danexx2_1, danexx2_2 o rozmiarach 40MB, 30MB. Pliki umieść w katalogu c:\ol_xx_data. Włącz mirroring dla tej przestrzeni. Pliki mirrorujące umieść w katalogu c:\ol_xx_mirror
   * Stwórz przestrzeń (dbspace) przeznaczoną na logi o nazwie logxx. Rozmiar przestrzeni to 30 MB w jednym chunku o nazwie nazwie logxx. Pliki umieść w katalogu c:\ol_xx_log. Włącz mirroring dla tej przestrzeni.
   * Stwórz dwie przerzenie (dbspace) przeznaczone na dane tymczasowe o nazwach tmp1_xx, tmp2_xx. Rozmiar każdej przestrzeni to 30 MB w jednym chunku o nazwie nazwach , tmp1_xx, tmp2_xx. Pliki umieść w katalogu c:\ol_xx_tmp.
   * Stwórz bazę danych db1_xx w przestrzeni danexx1
   * Stwórz bazę danych db2_xx w przestrzeni danexx2
   * Zweryfikuj poprawność konfiguracji (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)
   * Podaj ilość wolnego miejsca w każdej przestrzeni

 Zarządzanie logami

   * Przenieś log fizyczny (phisical log) do przestrzeni log_xx
   * Przenieś log logiczny (logical log) do przestrzeni log_xx, utwórz dodatkowe elementy logu (logi cal log files) tak aby log logiczny miał w sumie nie mniej niż 40 MB. Jeśli jest taka potrzeba to powiększ rozmiar przestrzeni na logi
   * Zweryfikuj poprawność konfiguracji (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)

Zarządzanie przestrzenią dyskową 2

   * Jeśli baza db1_xx nie była utworzona w „trybie logowania”, zmień tryb logowania bazy na unbuffered (co oznaczają poszczególne „tryby logowania” baz)
   * Stwórz w bazie danych db1_xx tabelę tab1_xx o kolumcach: id int (klucz główny), nazwa char 1200
   * Wstaw 30 000 wierszy do tej tabeli
   * Zaobserwuj proces wypełniania logu logicznego
   * Pokaż raporty wykorzystania przestrzeni dyskowej, skomentuj istotne parametry które można odczytać z tych raportów
   * Ile jest wolnego miejsca w poszczególnych przestrzeniach dyskowych, ile w poszczególnych chunkach
   * Ile miejsca zajmuje tabela tab1_xx, w ilu extent’ach, jaki jest rozmiar poszczególnych extent’ów
   * Stwórz w bazie danych db1_xx tabelę tab2_xx o kolumcach: id int (klucz główny), nazwa char 2400
   * Wstaw 10 wierszy do tej tabeli tab2_xx
   * Wstaw 5000 kolejnych wierszy do tabeli tab1_xx
   * Ile miejsca zajmują tabele tab1_xx, tab2_xx, w ilu stronach, w ilu extent’ach, jaki jest rozmiar poszczególnych extent’ów
   * Pokaż zawartość strony zawierającej 5 wiersz tabeli tab1_xx, ile wierszy znajduje się na tej stronie
   * Pokaż zawartość strony zawierającej 7 wiersz tabeli tab2_xx, ile wierszy znajduje się na tej stronie
   * Usuń wiersze o id z przedziału [100 .. 20 000] z tabeli tab1_xxx 
   * Pokaż raporty wykorzystania przestrzeni dyskowej, , skomentuj istotne parametry które można odczytać z tych raportów
   * Ile jest wolnego miejsca w poszczególnych przestrzeniach dyskowych, ile w poszczególnych chunkach
   * Ile miejsca zajmuje tabela tab1_xx, w ilu stronach, w ilu extent’ach, jaki jest rozmiar poszczególnych extent’ów itp.
   * Usuń wszystkie dane z tab1_xxx
   * Pokaż raporty wykorzystania przestrzeni dyskowej
   * Ile jest wolnego miejsca w poszczególnych przestrzeniach dyskowych, ile w poszczególnych chunkach
   * Ile miejsca zajmuje tabela tab1_xx, w ilu stronach, w ilu extent’ach, jaki jest rozmiar poszczególnych extent’ów itp.
   * (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)
   * Stwórz w bazie danych db1_xx tabelę tab_frag1_xx o kolumcach: id int (klucz główny), nazwa char 200, kod char(1)
   * Pofragmentuj tabelę pomiędzy przestrzenie danexx1, danexx2 – fragmentacja według wartości pola kod – A,B – przestrzeń danexx1, C – przestrzeń danexx2
   * Wstaw 10 000 wierszy do tej tabeli o wartości pola kod = A, 50 000 wierszy o wartości pola kod = B, 7 000 wierszy do tej tabeli o wartości pola kod = C,
   * Pokaż w których dbspaceach/chunkach zostały umieszczone dane
   * Zmień strategię fragmentacji, – fragmentacja według wartości pola kod – A – przestrzeń danexx1, B,C – przestrzeń danexx2
   * Pokaż w których dbspaceach/chunkach zostały umieszczone dane

Archiwizacja i odtwarzanie

   * Skonfiguruj i wykonaj backup danych
   * Skonfiguruj i wykonaj backup logów
   * Wstaw 10 000 wierszy do tabeli tab2_xx, pamiętaj o zakomitowaniu transakcji
   * Wstaw 5 000 wierszy do tabeli tab1_xx, nie komituj transakcji
   * Wykonaj checkpoint (full checkpoint), wszystkie dane puli buforów powinny zostać zapisane na dysk, zweryfikuj fakt wykonania checkpointu w „message logu”, zweryfikuj stan kolejek LRU
   * (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)
   * Pamiętaj o backupowaniu logów
   * Zasymuluj awarię (przejdź do trybu ofline, usuń pliki tworzące przestrzeń dane1_xx)
   * Spróbuj przejść do trybu online
   * Sprawdź status dbspace’ów
   * Odtwórz dane z backupu
   * Wiersze wstawione do tabeli tab2_xx powinny zostać odtworzone
   * Wiersze wstawione do tabeli tab1_xx powinny zostać wycofane
   * (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)
