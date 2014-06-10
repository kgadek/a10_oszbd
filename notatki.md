1. Tworzenie instancji

	>> IFIX=/c/Program\ Files/IBM/Informix/11.70

   * Utwórz nową instancję serwera bazodanowego:
   * Nazwa instancji (DBSERVERNAME) ol2014_xx, gdzie xx – twoje inicjały (dotyczy to całego zadania, podczas wykonywania kolejnych operacji xx pojawiające się w dokumencie powinno być zastąpione inicjałami autora)
   * Plik tworzący rootdbs powinien znajdować się w katalogu c:\ol_xx_data (gdzie xx – twoje inicjały), rozmiar rootdbs 200 MB
   * Włącz mirroring dla rootdbs, pliki mirrorujące rootdbs powinny znajdować się w katalogu c:\ol_xx_mirror (gdzie xx – twoje inicjały)
   * (dopuszczalna jest konfiguracja pozostawiająca pliki tworzące rootdbs w domyślnym miejscu)

	>> mkdir C:\ol_kg_data
	>> mkdir C:\ol_kg_mirror
	>> type nul >>C:\ol_kg_data\rootdbs_dat.000
	>> type nul >>C:\ol_kg_mirror\rootdbs_dat_mirror.000

   * Skonfiguruj interfejs dla połączeń sieciowych dla portu 9092
   * Zweryfikuj poprawność utworzenia instancji (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranów pokazujących rezultaty tych poleceń)

2. Konfiguracja połączeń

   * Skonfiguruj dodatkowy interfejs dla połączeń sieciowych dla poru 9094

	>> onmode -ky
	>> starts ol2014_kg

   * Zweryfikuj czy serwer jest w trybie online, jeśli nie to przejdź do trybu online. (wykonaj odpowiednie polecenie, umieść w sprawozdaniu zrzut ekranu pokazujący rezultat tego polecenia)
   * Zweryfikuj możliwość połączenia przy pomocy obu skonfigurowanych interfejsów sieciowych (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)

3. Zarządzanie przestrzenią dyskową

   * Stwórz przestrzeń (dbspace) o nazwie danexx1. Rozmiar przestrzeni to 70 MB w dwóch chunkach o nazwach danexx1_1, danexx1_2 o rozmiarach 50MB, 20MB. Pliki umieść w katalogu c:\ol_xx_data. Nie mirroruj tej przestrzeni.

	>> type nul >>C:\ol_kg_data\danekg1_1.000
	>> type nul >>C:\ol_kg_data\danekg1_2.000
	>> onspaces -c -d danekg1 -p C:\ol_kg_data\danekg1_1.000 -o 0 -s 51200
	>> onspaces -a danekg1 -p C:\ol_kg_data\danekg1_2.000 -o 0 -s 20400

   * Stwórz przestrzeń (dbspace) o nazwie danexx2. Rozmiar przestrzeni to 70 MB w dwóch chunkach o nazwach danexx2_1, danexx2_2 o rozmiarach 40MB, 30MB. Pliki umieść w katalogu c:\ol_xx_data. Włącz mirroring dla tej przestrzeni. Pliki mirrorujące umieść w katalogu c:\ol_xx_mirror

	>> type nul >>C:\ol_kg_data\danekg2_1.000
	>> type nul >>C:\ol_kg_data\danekg2_2.000
	>> type nul >>C:\ol_kg_mirror\danekg2_1_mirror.000
	>> type nul >>C:\ol_kg_mirror\danekg2_2_mirror.000
	>> onspaces -c -d danekg2 -p C:\ol_kg_data\danekg2_1.000 -o 0 -s 40960 -m C:\ol_kg_mirror\danekg2_1_mirror.000 0
	>> onspaces -a danekg2 -p C:\ol_kg_data\danekg2_2.000 -o 0 -s 30720 -m C:\ol_kg_mirror\danekg2_2_mirror.000 0

   * Stwórz przestrzeń (dbspace) przeznaczoną na logi o nazwie logxx. Rozmiar przestrzeni to 30 MB w jednym chunku o nazwie nazwie logxx. Pliki umieść w katalogu c:\ol_xx_log. Włącz mirroring dla tej przestrzeni.

	>> mkdir C:\ol_kg_log\
	>> type nul >>C:\ol_kg_log\logkg.000
	>> type nul >>C:\ol_kg_mirror\logkg_mirror.000
	>> onspaces -c -d logkg -p C:\ol_kg_log\logkg.000 -o 0 -s 30720 -m C:\ol_kg_mirror\logkg_mirror.000 0

   * Stwórz dwie przerzenie (dbspace) przeznaczone na dane tymczasowe o nazwach tmp1_xx, tmp2_xx. Rozmiar każdej przestrzeni to 30 MB w jednym chunku o nazwie nazwach , tmp1_xx, tmp2_xx. Pliki umieść w katalogu c:\ol_xx_tmp.

	>> mkdir C:\ol_kg_tmp
	>> type nul >>C:\ol_kg_tmp\tmp1_kg.000
	>> type nul >>C:\ol_kg_tmp\tmp2_kg.000
	>> onspaces -c -d tmp1_kg -t -p C:\ol_kg_tmp\tmp1_kg.000 -o 0 -s 30720
	>> onspaces -c -d tmp2_kg -t -p C:\ol_kg_tmp\tmp2_kg.000 -o 0 -s 30720

   * Stwórz bazę danych db1_xx w przestrzeni danexx1
   * Stwórz bazę danych db2_xx w przestrzeni danexx2

	>> #via dbaccess

   * Zweryfikuj poprawność konfiguracji (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)
   * Podaj ilość wolnego miejsca w każdej przestrzeni

	>> #http://www.tek-tips.com/viewthread.cfm?qid=776550
	>> select
	     sysdbstab.name,
	     sum(syschktab.chksize*4096) total,
	     sum(chksize - nfree)*4096 nused,
	     sum(syschktab.nfree*4096) free
	   from
	     syschktab, sysdbstab
	   where
	     syschktab.dbsnum = sysdbstab.dbsnum
	     group by 1 ;

	>> #wyniki w B

	<< name   danekg2
	<< total  73400320
	<< nused  4399104
	<< free   69001216

	<< name   rootdbs
	<< total  209715200
	<< nused  134258688
	<< free   75456512

	<< name   tmp1_kg
	<< total  31457280
	<< nused  217088
	<< free   31240192

	<< name   danekg1
	<< total  73318400
	<< nused  4399104
	<< free   68919296

	<< name   logkg
	<< total  31457280
	<< nused  217088
	<< free   31240192

	<< name   tmp2_kg
	<< total  31457280
	<< nused  217088
	<< free   31240192



4. Zarządzanie logami

   1.* Przenieś log fizyczny (phisical log) do przestrzeni log_xx

	>> onparams -p -s 30000 -d logkg -y
	>> #fix config
	>> onmode -ky
	>> starts ol2014_kg
	>> ontape -s

   2.* Przenieś log logiczny (logical log) do przestrzeni log_xx, utwórz dodatkowe elementy logu (logi cal log files) tak aby log logiczny miał w sumie nie mniej niż 40 MB. Jeśli jest taka potrzeba to powiększ rozmiar przestrzeni na logi

	>> onparams -a -d logkg -s 10240
	<< spodziewamy się ERRORa
	>> type nul >>C:\ol_kg_log\logkg.001
	>> type nul >>C:\ol_kg_mirror\logkg_mirror.001
	>> onspaces -a logkg -p C:\ol_kg_log\logkg.001 -o 0 -s 51200 -m C:\ol_kg_mirror\logkg_mirror.001 0
	>> onparams -a -d logkg -s 10240
	>> onparams -a -d logkg -s 10240
	>> onparams -a -d logkg -s 10240
	>> onparams -a -d logkg -s 10240
	>> onmode -c #odpowiednią ilość razy
	>> onmode -l
	>> onparams -d -l N #dla N <- [1..6]
	>> oncheck -pe logkg


   3.* Zweryfikuj poprawność konfiguracji (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)

5. Zarządzanie przestrzenią dyskową 2

   1.* Jeśli baza db1_xx nie była utworzona w „trybie logowania”, zmień tryb logowania bazy na unbuffered (co oznaczają poszczególne „tryby logowania” baz)

	>> SELECT * FROM sysdatabases WHERE name='db1_kg';
	>> ondblog unbuf
	>> ontape -s

   2.* Stwórz w bazie danych db1_xx tabelę tab1_xx o kolumcach: id int (klucz główny), nazwa char 1200

	>> CREATE TABLE tab1_kg (id int, nazwa char(1200), PRIMARY KEY(id));
	>> CREATE PROCEDURE blah()
		DEFINE i INTEGER;
		FOR i=1 TO 30000
			INSERT INTO tab1_kg (id,nazwa) VALUES (i, 'Blah Guy');
		END FOR;
	END PROCEDURE;
	EXECUTE PROCEDURE blah();
	DROP PROCEDURE blah;

   3.* Wstaw 30 000 wierszy do tej tabeli
   4.* Zaobserwuj proces wypełniania logu logicznego

	>> onstat -l

   5.* Pokaż raporty wykorzystania przestrzeni dyskowej, skomentuj istotne parametry które można odczytać z tych raportów

	>> onstat -d

   6.* Ile jest wolnego miejsca w poszczególnych przestrzeniach dyskowych, ile w poszczególnych chunkach
   7.* Ile miejsca zajmuje tabela tab1_xx, w ilu extent’ach, jaki jest rozmiar poszczególnych extent’ów
   8.* Stwórz w bazie danych db1_xx tabelę tab2_xx o kolumcach: id int (klucz główny), nazwa char 2400

	>> CREATE TABLE tab2_kg (id int, nazwa char(2400), PRIMARY KEY(id));
	>> CREATE PROCEDURE bleh()
		DEFINE i INTEGER;
		FOR i=1 TO 10
			INSERT INTO tab2_kg (id,nazwa) VALUES (i, 'Watermelon');
		END FOR;
	END PROCEDURE;
	EXECUTE PROCEDURE bleh();
	DROP PROCEDURE bleh;
	>> CREATE PROCEDURE blih()
		DEFINE i INTEGER;
		FOR i=1 TO 5000
			INSERT INTO tab1_kg (id,nazwa) VALUES (30000+i, 'H-Bomb');
		END FOR;
	END PROCEDURE;
	EXECUTE PROCEDURE blih();
	DROP PROCEDURE blih;


   9.* Wstaw 10 wierszy do tej tabeli tab2_xx
   10.* Wstaw 5000 kolejnych wierszy do tabeli tab1_xx
   11.* Ile miejsca zajmują tabele tab1_xx, tab2_xx, w ilu stronach, w ilu extent’ach, jaki jest rozmiar poszczególnych extent’ów
   12.* Pokaż zawartość strony zawierającej 5 wiersz tabeli tab1_xx, ile wierszy znajduje się na tej stronie
   13.* Pokaż zawartość strony zawierającej 7 wiersz tabeli tab2_xx, ile wierszy znajduje się na tej stronie
   14.* Usuń wiersze o id z przedziału [100 .. 20 000] z tabeli tab1_xxx 
   15.* Pokaż raporty wykorzystania przestrzeni dyskowej, , skomentuj istotne parametry które można odczytać z tych raportów
   16.* Ile jest wolnego miejsca w poszczególnych przestrzeniach dyskowych, ile w poszczególnych chunkach
   17.* Ile miejsca zajmuje tabela tab1_xx, w ilu stronach, w ilu extent’ach, jaki jest rozmiar poszczególnych extent’ów itp.
   18.* Usuń wszystkie dane z tab1_xxx
   19.* Pokaż raporty wykorzystania przestrzeni dyskowej
   20.* Ile jest wolnego miejsca w poszczególnych przestrzeniach dyskowych, ile w poszczególnych chunkach
   21.* Ile miejsca zajmuje tabela tab1_xx, w ilu stronach, w ilu extent’ach, jaki jest rozmiar poszczególnych extent’ów itp.
   22.* (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)
   23.* Stwórz w bazie danych db1_xx tabelę tab_frag1_xx o kolumcach: id int (klucz główny), nazwa char 200, kod char(1)
   24.* Pofragmentuj tabelę pomiędzy przestrzenie danexx1, danexx2 – fragmentacja według wartości pola kod – A,B – przestrzeń danexx1, C – przestrzeń danexx2
   25.* Wstaw 10 000 wierszy do tej tabeli o wartości pola kod = A, 50 000 wierszy o wartości pola kod = B, 7 000 wierszy do tej tabeli o wartości pola kod = C,
   26.* Pokaż w których dbspaceach/chunkach zostały umieszczone dane
   27.* Zmień strategię fragmentacji, – fragmentacja według wartości pola kod – A – przestrzeń danexx1, B,C – przestrzeń danexx2
   28.* Pokaż w których dbspaceach/chunkach zostały umieszczone dane

6. Archiwizacja i odtwarzanie

   1.* Skonfiguruj i wykonaj backup danych
   2.* Skonfiguruj i wykonaj backup logów
   3.* Wstaw 10 000 wierszy do tabeli tab2_xx, pamiętaj o zakomitowaniu transakcji
   4.* Wstaw 5 000 wierszy do tabeli tab1_xx, nie komituj transakcji
   5.* Wykonaj checkpoint (full checkpoint), wszystkie dane puli buforów powinny zostać zapisane na dysk, zweryfikuj fakt wykonania checkpointu w „message logu”, zweryfikuj stan kolejek LRU
   6.* (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)
   7.* Pamiętaj o backupowaniu logów
   8.* Zasymuluj awarię (przejdź do trybu ofline, usuń pliki tworzące przestrzeń dane1_xx)
   9.* Spróbuj przejść do trybu online
   10.* Sprawdź status dbspace’ów
   11.* Odtwórz dane z backupu
   12.* Wiersze wstawione do tabeli tab2_xx powinny zostać odtworzone
   13.* Wiersze wstawione do tabeli tab1_xx powinny zostać wycofane
   14.* (wykonaj odpowiednie polecenia, umieść w sprawozdaniu zrzuty ekranu pokazujące rezultaty tych poleceń)
