.. title: Jak dorobić do etatu, czyli tester tworzy RPA
.. slug: jak-dorobic-do-etatu-czyli-tester-tworzy-rpa
.. date: 2020-08-13 20:25:40 UTC+02:00
.. tags: rpa, howto
.. category: felieton
.. link: 
.. description: 
.. type: text
.. previewimage: /images/posts/20200813/rpa_preview.jpeg
.. template: newsletter.tmpl

.. image:: /images/posts/20200813/rpa_picture.jpeg
    :align: center
    :target: https://muscat.pl

Zapewne zauważyłeś mój drogi czytelniku, że od pewnego czasu nic nie napisałem. Powodów tego stanu rzeczy jest kilka, a o jednym z nich przeczytasz za chwilę. Zanim to zrobisz, zadam Ci kilka pytań:

* Czy jako tester oprogramowania jesteś w stanie dorobić sobie do etatu?
* Czy takie okazje trafiają się w trakcie pandemii?
* Jak znaleźć okazję?
* Jak podejść do współpracy ze zleceniodawcą pracując na etacie?
* Na co zwrócić uwagę przy omawianiu współpracy?
* Jak wycenić swoją pracę?

Jeśli choć na jedno pytanie nie znasz odpowiedzi, to zapraszam do poniższego artykułu, w którym postaram się odpowiedzieć na powyższe pytania.

.. more

Uprzedzając pytania, artykuł nie jest w żaden sposób sponsorowany, a jest po prostu swoistego rodzaju case-study tego, co już zostało zrobione oraz pewnego rodzaju formalizacją wniosków ze współpracy, do których i mi będzie łatwiej wrócić w przyszłości. Dodatkowo na końcu znajdziesz małą niespodziankę, którą przygotowaliśmy wspólnie z firmą `Muscat Eyewear <https://muscat.pl>`_, z którą miałem okazję współpracować i która zainspirowała mnie do napisania tego artykułu.

.. image:: /images/posts/20200813/muscat.png
    :align: center
    :width: 320
    :target: https://muscat.pl


Czy jako tester oprogramowania jesteś w stanie dorobić sobie do etatu?
----------------------------------------------------------------------

Na to pytanie zapewne znasz odpowiedź i zapewne jest ona twierdząca. I z mojej strony mogą tylko potwierdzić. Nic odkrywczego. Opisany poniżej sposób wydaje się jednak najbardziej naturalnym sposobem, szczególnie dla testerów automatyzujących. Zadaję sobie sprawę, że nie koniecznie jest to droga dla wszystkich i jednocześnie chciałbym Was przestrzec przed porywaniem się z motyką na słońce. Dlaczego? O tym za moment.

Czy takie okazje trafiają się w czasach kryzysu/pandemii?
---------------------------------------------------------

Skoro zadaję takie pytanie, to zapewne domyślasz się odpowiedzi, a więc tylko potwierdzę Twoje domysły i odpowiem: **tak**. Zdziwilibyście się ile firm, pomimo kryzysu stawia na rozwój i inwestuję w projekty, które w krótszej lub dłuższej perspektywie albo przyniosą oszczędności, albo pozwolą na szybszy rozwój.

Jak znaleźć okazję?
-------------------

Tu nie ma jednoznacznej odpowiedzi, ale ostatnio odnoszę wrażenie, że bardzo wiele tego typu akcji przenosi się do mediów społecznościowych. Dlaczego tak się dzieje? Ponieważ bardzo łatwo tam zaistnieć oraz nasza sieć kontaktów przeważnie jest tam dosyć spora i łatwo kogoś przywołać do jakiejś dyskusji. W moim przypadku dokładnie tak to wyglądało i odbyło się trochę przypadkiem. Całość historii rozpoczęła się na Facebooku na grupie `SysOps/DevOps Polska <https://www.facebook.com/groups/sysopspolska/>`_, gdzie CTO w firmie `Muscat Eyewear <https://muscat.pl>`_ umieścił temat dotyczący "poszukiwania osoby, która byłaby w stanie napisać skrypt automatyzujący pewne operacje" (niemal dosłowny cytat z dyskusji). Pomimo że jestem członkiem tej grupy, temat jakoś umknął mojej uwadze. Do dyskusji przywołał mnie kolega będący DevOpsem w firmie, w której kilka lat wcześniej pracowałem (chyba dobrze jest utrzymywać jednak dobre kontakty z ludźmi z innych działów w firmie). Cała reszta dogadywania się to miks rozmów telefonicznych, wideokonferencji oraz wymiana maili. O co w ogóle chodziło z tym skryptem?

RPA, czyli Zrobotyzowana Automatyzacja Procesów
-----------------------------------------------

Skrypt, który miał zostać napisany, miał za zadanie wspomagać pewien proces biznesowy, a więc defakto chodziło o stworzenie *"szytego na miarę"* narzędzia typu RPA (Robotic Process Automation), czyli Zrobotyzowanej Automatyzacji Procesów. `Wikipedia <https://pl.wikipedia.org/wiki/Zrobotyzowana_Automatyzacja_Procesów>`_ opisuje to jako technologię automatyzacji powtarzalnych procesów biznesowych w oparciu o programy komputerowych, które symulują pracę człowieka. Jeśli jesteś testerem (lub chcesz nim zostać) to czy nie widzisz tu pewnej analogi do testów automatycznych? Przecież dokładnie to one robią, tzn. testy automatyczne symulują manualne operacje wykonywane przez testów oprogramowania. Dlatego powyżej napisałem, że ten sposób dorobienia do etatu wydaje się najbardziej naturalnym dla testerów automatyzujących, ponieważ to właśnie ta grupa w większości ma wymagany zestaw umiejętności, aby tego typu narzędzia budować. Fakt, poza prostym pisaniem testów, wymagana jest pewna wiedza z zakresu pisania oprogramowania, ale tego typu rzeczy, każdy szanujący się senior powinien być w stanie zrobić bez większego problemu.

Niestety nie mogę Wam podać wszystkich szczegółów dotyczących tego do czego ten skrypt służy (tak jest już wdrożony produkcyjnie), ale to co poniżej przeczytacie, pozwoli Wam na wyrobienie sobie ogólnego obrazu.

W ogłoszeniu na Facebooku Łukasz napisał, że skrypt ma za zadanie przenosić pewne dane pomiędzy 2 systemami, z których jeden posiada API, a do drugiego można się dostać jedynie przez stronę www. Co prawda w ogłoszeniu była sugestia dotycząca używania JavaScriptu i dowolnego framework wykorzystywanego do testowania www, ale chyba w pierwszej prywatnej rozmowie, okazało się, że technologia nie ma znaczenia i równie dobrze może to być Python. Zanim jednak dojdziemy do szczegółów technicznych, omówmy jeszcze jeden istotny temat, czyli

Jak podejść do współpracy pracując na etacie?
---------------------------------------------

Zdecydowanie najwygodniejszą formą dla zleceniodawcy byłaby umowa w oparciu o model tzw. *B2B*, czyli gdybyś prowadził działalność gospodarczą, jednak to nie jedyna możliwa forma współpracy (dlaczego to już zupełnie oddzielny temat). W moim przypadku stanęło na spisaniu zwykłej umowy o dzieło. Pamiętaj jednak, że w każdej umowie o współpracę znajdą się pewne zapisy, które nakładają na Ciebie, jako wykonawcy pewne obowiązki oraz ewentualne kary. Dlatego pisałem, żebyś nie porywał się z motyką na słońce, ponieważ może Cię to słono kosztować. Przeczytaj taką umowę kilka razy i jeśli cokolwiek wydaje Ci się nie jasne, skonsultujcie to z prawnikiem (takie porady nie są drogie, a mogą Cię ustrzec przed późniejszymi problemami). Prześledź też wypisany zakres prac oraz zakres Waszej odpowiedzialności, gwarancji itp. Jeśli uważacie, że coś jest nie tak, albo wychodzi poza wcześniej ustalony zakres, nie obawiajcie się prosić o zmianę (pamiętajcie, aby to sensownie uzasadnić) oraz w razie rozszerzenia zakresu obowiązków renegocjujcie wysokość wynagrodzenia. Obie strony tego układu, czyli Ty jako zleceniobiorca oraz Twój zleceniodawca musicie być zadowoleni. Jeśli, któraś ze stron poczuje się "wykorzystana" może to prowadzić do frustracji i niepotrzebnych przepychanek, co przeważnie kończy się źle. Pamiętaj również, że po drugiej stronie też są tylko ludzie i nie zakładaj nigdy z góry, że ktoś umyślnie działa na Twoją niekorzyść.

Jedną z najważniejszych rzeczy, którą ja od samego początku podkreślałem, to fakt, że pracuję na etacie, a więc wszelkie prace związane z tworzeniem skryptu, będą prowadzone po moich normalnych godzinach pracy (przeważnie wieczorami oraz w weekendy). Trzeba o takich rzeczach uprzedzić już na samym początku, ponieważ nie raz w trakcie Twojej pracy dotrzesz do miejsca, które będzie wymagało bądź doprecyzowania jakichś szczegółów, bądź jakiejś interwencji po stronie waszego zleceniodawcy.

Jednak zanim podpiszesz samą umowę, musisz określić wspólnie z klientem:

* zakres projektu
* czego potrzebujesz od klienta
* zakres gwarancji
* czas i koszt

Zakres projektu
===============


To chyba jeden z najtrudniejszych punktów w całej tej "zabawie". Powód tego jest bardzo prozaiczny. W normalnej pracy testera często musisz testować proces, którego nie rozumiesz i musisz się go od kogoś nauczyć. Tu jest bardzo podobnie z drobną różnicą: poza pracą analityka biznesowego musisz też stać się architektem systemu. Musisz więc przemyśleć masę spraw, o których zwykły tester, a czasem również i klient, nie mają pojęcia.  Czasem trafisz na bardzo konkretnego klienta, który będzie wiedział, czego potrzebuje. Czasem jednak będziesz musiał go wspomóc. Pewne rzeczy, musisz przemyśleć przed przystąpieniem do projektu, a pewne zaimplementować dla swojej wygody. Lista najważniejszych elementów, na które należy zwrócić uwagę to:

1. **Dokładne ustalenie procesu biznesowego** - pamiętaj, że klient zna swój proces biznesowy i dla niego to, co Ty masz zrobić, może być czymś "trywialnym", ale to, co jest trywialne dla klienta i tylko wymaga dużego nakładu manualnej pracy, dla kogoś, kto to zautomatyzuje już nie koniecznie takie będzie. Pamiętaj, że może pojawić się w procesie masa warunków, dodatkowych danych oraz sam proces może przebiegać różnie, w zależności jakie dane zostaną wprowadzone na samym początku jego przebiegu. Taka automatyzacja to już nie jest prosty skrypt testów, który podąża od punktu A do punktu B i jak wydarzy się coś nieoczekiwanego, to po prostu zgłosi błąd. Tutaj bardzo często w grę wchodzą rzeczywiste pieniądze i jeśli proces zostanie błędnie obsłużony, to może wiązać się ze stratami finansowymi po stronie klienta.

2. **Z jakimi systemami będziesz współpracował** - proces biznesowy (mocno upraszczając) polega na przepływie i obróbce jakichś danych. Skoro tak, to należałoby określić punkty wejścia i wyjścia z takiego procesu biznesowego. Bardzo często może się okazać, że punktem wejścia i wyjścia z procesu biznesowego będzie ten sam system, ale w trakcie jego procesowania część danych musi zostać "przeniesiona" do drugiego systemu, z którego z kolei jakieś dane należy pobrać. Żeby sobie uzmysłowić, o co chodzi, wyobraźmy sobie proces zakupowy w jednej z platform aukcyjnych (nie będę żadnej faworyzował, więc tym razem bez linków). Mocno upraszczając, klient chce coś kupić. Po znalezieniu produktu oraz opłaceniu zamówienia trafia ono do sprzedawcy, a więc dane przepływają od platformy do sprzedawcy. Sprzedawca wysyła towar i przesyła status realizacji zamówienia do klienta. Tutaj następuje przepływ danych od sprzedawcy do platformy. Cały ten przepływ danych nastąpił w ramach jednego procesu biznesowego, jakim jest zakup towaru (tak wiem, że można go rozbić na mniejsze elementy. ale nie o tą analizę tutaj chodzi). Znów dochodzimy do wniosku, że to, co proste do zrobienia manualnie, nie koniecznie będzie takie proste do automatyzacji.

3. **Dodatkowe dane** - może się okazać, że proces biznesowy wymaga jakichś dodatkowych danych, które aktualnie są przetrzymywane np. w arkuszach kalkulacyjnych, a więc należy przemyśleć przechowywanie takich danych. Takie dane powinny być łatwe do edycji, a więc należy przewidzieć jakiś format przechowywania, który będzie spójny z punktu widzenia języka, w którym będzie tworzyć narzędzie RPA, ale jednocześnie zrozumiały i łatwy do edycji dla Waszego klienta. Niestety pliki arkusza kalkulacyjnego nie są dobrym formatem do przechowywania danych, gdyż łatwo "zepsuć" taki arkusz, np. poprzez omyłkowe scalenie komórek lub pozostawienie pustych rzędów, co wymaga obsłużenia większej liczny przypadków/wyjątków w kodzie.

4. **Logowanie zdarzeń** - pamiętaj też o odpowiednim sposobie logowania akcji, które budowane narzędzie wykonuje. Obsługa logowania ułatwi 2 rzeczy: proces tworzenia aplikacji (mniej debugowania kodu) oraz łatwiejszą naprawę błędów pojawiających się u klienta po wdrożeniu "na produkcji".

5. **Raportowanie wyników** - każdy proces biznesowy w jakimś miejscu się kończy, a więc należy gdzieś przedstawić status działania takiego procesu. Pamiętaj, że nie zawsze proces biznesowy zakończy się sukcesem. Czasem mogą wydarzyć się jakieś nieoczekiwane błędy w samych danych wprowadzonych do procesu, co może skutkować niemożliwością jego zakończenia. Jeżeli jest to proces kluczowy dla funkcjonowania waszego klienta, może się również okazać, że takie dane będą musiały zostać ręcznie przetworzone lub zmodyfikowane i ponownie wprowadzone do tworzonego narzędzia RPA. Dobrą praktyką byłoby więc, poinformowanie klienta, nie tylko o sukcesie, ale również o porażce oraz wskazaniu jej przyczyn, np. poprzez poinformowanie jakie dane nie mogły zostać odczytane lub przetworzone. W ten sposób ułatwicie klientowi sposób postępowania z konkretnym zestawem danych, a więc pomimo porażki zredukujecie czas manualnej ich obsługi.

6. **Opcje ustawień** - narzędzie RPA, to oprogramowanie i jak prawie każde oprogramowanie może wymagać czasem zmiany jakichś parametrów, np. adresu bazy danych, do której się łączy lub modyfikacji nazwy użytkownika i/lub hasła. Warto, aby takie parametry były łatwo modyfikowalne np. poprzez zmianę zmiennych środowiskowych lub edycję jakich plików konfiguracyjnych.

7. **Dokumentacja** - jak każdy projekt, tak i narzędzie RPA powinno być dostarczone do klienta z dokumentacją. Zakres oraz formę takiej dokumentacji należy również określić z klientem, gdyż może się okazać, że wytworzenie dokumentacji będzie równie czasochłonne co napisanie kodu. Czasem jako dokumentacją wystarczą odpowiednie komentarze w kodzie i/lub np. pliki README.md, które będą znajdowały się w repozytorium kodu tworzonego narzędzia.

8. **Testy** - coś co tygryski lubią najbardziej😉. Tutaj sprawa jest bardzo otwarta i jeżeli klient Wam tego nie narzuci, sprawa pozostaje na Twoich barkach. Możesz tworzyć testy jednostkowe, funkcjonalne lub poprzestać na testach integracyjnych opierając się na danych dostarczonych przez klienta. Może to być również miks powyższych lub jeszcze inne podejście, jednak na pewno przyjdzie taki moment, kiedy Wasza aplikacja będzie musiała zacząć działać na danych produkcyjnych. Dojdziecie więc do tzw. testów akceptacyjnych. Jak dojść w miarę bezpiecznie do tego etapu, tak aby nie okazało się, że "nic nie działa", opiszę trochę niżej.

9. **Inne** - być może okaże się, że poza napisaniem samego narzędzia, będzie należało przeprowadzić jakieś dodatkowe prace uzupełniające w trakcie wdrożenia, np. konfiguracja systemu tak, aby program automatycznie się uruchamiał. Może się również okazać, że poza dostarczeniem kodu programu, klient będzie wymagał np. stworzenia obrazu dockerowego z tworzonym narzędziem lub wytworzenia odpowiednich skryptów w narzędziu CI/CD, którym zarządza klient.

Jak więc widzisz ilość elementów, które należy uwzględnić w samym zakresie projektu, zdecydowanie wykracza poza prostą analizę procesu biznesowego, dla którego tworzone jest narzędzie RPA.

Pamiętaj również, że klient może zażądać od Ciebie podpisania umowy `NDA <https://pl.wikipedia.org/wiki/Umowa_poufności>`_, czyli tzw. umowy poufności. Nie bój się takiej umowy. Klient stara się tylko obronić swoje interesy, abyś intencjonalnie nie udostępnił komuś niepowołanemu procesów biznesowych lub innych danych stanowiących własność intelektualną klienta, a które bardzo często stanowią o przewadze konkurencyjnej. Pamiętaj również, że jeśli masz wątpliwości co do treści takiej umowy, warto po prostu zasięgnąć porady prawnika. Pamiętaj jednak, że nie przewidzenie któregoś z powyższych elementów może doprowadzić do sytuacji, że poświęcisz na projekt dodatkowe godziny, za które nikt Ci nie zapłacić, bo z własnej winy lub niewiedzy nie przewidziałeś ich w kosztorysie.

Czego potrzebujesz od klienta?
==============================

Odpowiedź na to pytanie nie jest oczywista i bardzo mocno zależy od tego, jaki jest zakres projektu. Przejdźmy więc bardzo szybko po punktach z zakresu projektu i zastanówmy się, co może nam dostarczyć klient:

1. **Proces biznesowy** - tu sprawa jest jasna, czyli potrzebujemy jakiejś formy opisu tego procesu. Forma jest w zasadzie dowolna, ale nie wymagajcie od klienta tworzenia skomplikowanych dokumentów, jeśli do tej pory ich nie wytworzył, bo możecie trafić na ścianę nie do przejścia. Pamiętaj, że wytworzenie takiej dokumentacji może wymagać dużego nakładu czasu po stronie klienta, a skoro potrzebuje on narzędzia RPA, to bardzo często potrzebuje właśnie tego czasu zaoszczędzić. Jeśli jest taka możliwość, poproś o inną formę, która jest mniej czasochłonna, np. nagranie filmów wraz z komentarzem dotyczącym całego procesu. Jeśli coś będzie niejasne, poproś później o doprecyzowanie.

2. **Inne systemy** - skoro mówimy o jakichś systemach (lub aplikacjach) to na pewno będziemy potrzebowali do nich jakichś danych dostępowych. Najczęściej będzie to jakiś login i hasło. Ideałem byłoby, gdyby w początkowej fazie klient dysponował jakimś środowiskiem testowym. Dostęp do takiego środowiska ułatwi Ci proces tworzenia narzędzia, gdyż zarówno Ty, jak i klient będziecie spokojniejsi, że nic nie popsujesz. Dodatkowo będziesz miał możliwość samemu odtworzyć manualnie proces biznesowy, który tworzone narzędzie będzie automatyzować.

3. **Dodatkowe dane** - po otrzymaniu dokumentacji z punktu 1, po prostu zorientujesz się, czy takie dane są potrzebne, czy nie. Również w późniejszych etapach może się okazać, że potrzebujesz jakichś dodatkowych danych, o których istnieniu w początkowym etapie nie zdawałeś sobie sprawy. Nie wahaj się i po prostu o nie zapytaj.

4. **Logowanie zdarzeń** - ustalenie formatu logowanie zdarzeń w większości wypadków będzie zależało od Ciebie. Warto jednak z klientem się w tej kwestii po prostu dogadać, gdyż w razie późniejszych problemów, to klient będzie musiał Ci te logi jakoś dostarczyć.

5. **Raportowanie wyników** - podobnie jak w punkcie 4, warto ustalić z klientem: gdzie, w jakiej formie oraz jakie dane taki raport powinien zawierać.

6. **Opcje ustawień** - część z tych opcji może wynikać ze wszystkich wcześniejszych punktów, część może wynikać z Twoich potrzeb (np. będzie włączać dodatkowe elementy przydatne podczas debugowania lub testowania rozwiązania), a część może wynikać z jakich potrzeb Twojego klienta (np. jakieś opóźnienie w procesowaniu lub cykliczne uruchamianie narzędzia).

7. **Dokumentacja** - podobnie jak w punkcie 4, warto ustalić format i zakres dokumentacji.

8. **Testy** - i znów warto ustalić z klientem, czy testy akceptacyjne będą wystarczające, czy wymagane jest coś więcej.

9. **Inne** - no i po raz ostatni warto spytać się, czy będzie coś jeszcze potrzebne.

Pamiętaj, że skoro klient nawiązał z Tobą współpracę, to znaczy, że zależy mu na tym narzędziu oraz na czasie, który dzięki niemu zaoszczędzi (pieniądze przy okazji również). Będzie się więc starał oraz dopingował Cię, aby narzędzie powstało jak najszybciej, a więc dostarczy Ci wszystkich potrzebnych Ci informacji. Pamiętaj również, że u klienta też pracują normalni ludzie, którzy mają swoje prywatne życie i problemy i jeśli są jakieś opóźnienia, to grzecznie przypomnijcie, że o coś pytaliście. Nie róbcie afery tam, gdzie jej nie ma. Każdy może o czymś zapomnieć i jeśli Ty będziesz do swojego klienta podchodził rozsądnie, to i on podjedzie do Ciebie podobnie.

Zakres gwarancji
================

Tutaj sprawa wydaje się w miarę oczywista. Jako wykonawca, będziesz zobowiązany do naprawy błędów w oprogramowaniu. Może się również okazać, że klient przewidzi jakieś kary za nie dotrzymanie terminów lub będziesz musiał w pewnych przypadkach dojechać do klienta. Warto pewne rzeczy przewidzieć i dobrze zastanowić się co wchodzi w zakres gwarancji.

Czas i koszt
============

Znając zakres projektu, możesz przystąpić do oszacowania czasu potrzebnego do wykonania poszczególnych elementów oraz całościowego kosztu.

Nie podam Ci tutaj żadnej złotej formuły. Czas potrzebny na wytworzenie każdego z elementów wchodzących w zakres projektu może być mocno zróżnicowany. Zależeć on będzie zarówno od samego stopnia skomplikowania, jak i również od Waszych umiejętności. Pamiętaj jednak, że większości mamy mocną tendencję do błędnego szacowania czasu potrzebnego na wykonanie danego zadania i przeważnie go zaniżamy. Wynika to bardzo często z błędnego zakresu projektu, bo ktoś nam czegoś nie powiedział lub o coś zapomnieliśmy zapytać oraz zbytniej wiary w nasze własne umiejętności. Pamiętaj również o uwzględnieniu buforu bezpieczeństwa na jakieś nieprzewidziane zdarzenia, np. choroba lub błędne oszacowanie zakresu pracy. Taki bufor powinien stanowić minimum 20% ogólnego czasu, a jeszcze lepiej, jeśli to będzie nawet 50%, gdy nie masz pewności co do kluczowych elementów zakresu projektu. Jeśli jest to Twój pierwszy projekt takiego typu to taki bufor możne nawet rozszerzyć do 100%, a i tak jest duża szansa, że okaże się on zbyt mały.

Jak już uda Ci się oszacować potrzebny czas to jedyne co pozostaje to określenie wysokości stawki za Twoją roboczo godzinę oraz przedstawienie tego wyliczenia swojemu klientowi. Jak określić stawkę za roboczo godzinę? Można pójść kilkoma drogami:

* przyjąć wysokość stawki, jaką masz na etacie,
* przyjąć wysokość stawki programisty języka, w jakim będziecie tworzyć narzędzie,
* przyjąć dowolną stawkę, która zadowoliłaby Was, gdybyście pracowali na swoim,
* dowolny inny sposób.

Co się wydarzy po przedstawieniu wyceny projektu klientowi, to już różnie bywa. Może się okazać, że klient przyjmie tę kwotę bez jakiejkolwiek rozmowy, co będzie znaczyło, że miał dużo większy budżet, a Ty będziesz się zastanawiał czy nie warto było powiedzieć więcej. Może się również okazać, że zacznie się targować. W tym 2 przypadku to od Ciebie będzie zależało, czy po dalszych pertraktacjach zgodzisz się na proponowaną cenę (nigdy nie gódź się na 1 proponowaną kwotę, chyba że Cię zadowala), czy też podziękujesz i odmówisz wykonania projektu.

Zanim podpiszesz umowę
======================

Jak już masz powyższe rzeczy ustalone z klientem, weź jeszcze raz głęboki oddech i przeczytaj wszystko jeszcze raz. Sprawdź dobrze, czy czegoś nie pominąłeś, czegoś nie brakuje lub jakieś zapisy nie powinny zostać skorygowane, lub zmienione. Jeśli masz wątpliwości, to tak jak już wspominałem wcześniej, skonsultuj się z prawnikiem. Nie pomoże on w kwestiach technicznych i wycenie, ale może zwrócić uwagę na rzeczy, o których Ty zupełnie nie pomyślałeś, a co może uchronić Cię przez dużymi kosztami w przyszłości.

Pamiętaj również o najważniejszej rzeczy. Znając powyższe aspekty i wymagania klienta, powinieneś już mieć w głowie choćby nakreślony ogólny zarys, jak projekt będzie wyglądał, jakich bibliotek będziesz używał i czy w ogóle jesteś w stanie wykonać pewne elementy.

Podpisałem umowę i co dalej?
============================

Jak to, co dalej? **Do roboty!**

Od czego zacząć?
================

Tak jak napisałem powyżej, przed podpisaniem umowy, powinieneś mieć już w głowie ogólny zarys oraz listę bibliotek, których użyjesz podczas tworzenia narzędzia RPA dla Twojego klienta. Fakt posiadania wizji to jedno, a życie to drugie. Może się okazać, że np. nie masz dostępu do środowiska testowego i bez ingerencji w środowisko produkcyjne, możesz tylko pobierać z niego jakieś dane. Mogą wystąpić jakieś inne trudności.

Jak więc podejść do tematu?
===========================

"Dziel i rządź". Podziel pracę na mniejsze i częściowo od siebie niezależne obiekty/klasy:

* do interakcji z systemem, z którego będziesz pobierał dane,
* do interakcji z systemem, do którego dane będziesz dostarczał,
* struktura danych (będzie wykorzystywany przez oba obiekty powyżej),
* odpowiadający za logowanie zdarzeń,
* odpowiadający za odczyt konfiguracji i/lub dodatkowych danych.

Podchodząc do tego w ten sposób, jesteś w stanie pracować nad pewnymi jego elementami niezależnie od innych i chwilowe trudności w dostępie do np. środowiska testowego, lub oczekiwanie na doprecyzowanie jakichś kwestii ze strony klienta, nie będą tak wpływały na opóźnienie projektu. Pamiętaj również, że klient jest żywo zainteresowany postępem prac, więc jeśli jesteś w stanie zaprezentować mu jakaś część narzędzia, zrób to. Pamiętaj, aby klientowi powiedzieć, co mniej więcej zobaczy i czego może brakować. Takie podejście, poza uspokojeniem klienta, może również wpłynąć na wczesne wykrycie błędów lub nieścisłości w samym procesie. Często może się okazać, że Twoje rozumienie automatyzowanego procesu biznesowego było błędne lub pojawią się jakieś dodatkowe warunki, które dla klienta były oczywiste, a o których nie wspomniał w trakcie spisywania wymagań (dlatego bufor czasu przy wycenie projektu jest tak ważny). Co jednak zrobić, jeśli okaże się, że bardzo mocno pewne rzeczy ulegają zmianie względem pierwotnego zakresu projektu?

Zmiana zakresu projektu
=======================

Nie bój się głośno na ten temat powiedzieć i zakomunikować, że takie zmiany wpłyną na czas implementacji rozwiązania oraz wiążą się z wykonaniem dodatkowych prac, które nie zostały uwzględnione w umowie i jej wycenie. Poproś o aneks do umowy i uzgodnij z klientem, ile za tą dodatkową pracę powinien zapłacić. Być może klient wycofa się z tych zmian lub ograniczy ich zakres.

Testy akceptacyjne
==================

Udostępnianie tworzonego narzędzia na prezentacjach lub do testów w trakcie jego pisania, poza powyżej opisanymi aspektami, ułatwi również bardzo mocno kwestie odbioru tego narzędzia przez klienta. Dzieje się tak, dlatego że klient nie będzie zaskoczony tym, jak narzędzie wygląda i działa oraz będzie ono już dawno sprawdzone, a większość błędów będzie już naprawiona. Nie znaczy to, że w momencie obioru, narzędzie będzie wolne od błędów. Na pewno znajdą się jakieś przypadki danych, których ani Ty, ani klient nie przewidział i jeśli się pojawią, będziesz musiał coś poprawić. Nie wpłynie to jednak na sam proces odbioru aplikacji. W ten sposób można w zasadzie pominąć lub raczej rozciągnąć proces testów akceptacyjnych na prawie cały czas rozwijania narzędzia.

Gwarancja
=========

Oczywiście po testach akceptacyjnych i wdrożeniu aplikacji u klienta, następuje okres gwarancyjny. Warto szybko odpowiadać na zgłoszenia, ponieważ:

* po 1 jesteście do tego zobligowani zapisami w umowie,
* po 2 warto z takim klientem dobrze żyć, bo być może w przyszłości będziecie mogli jeszcze w innych kwestiach mu pomóc, a co za tym idzie dodatkowo zarobić (jak to mawiał pewien klasyk: "zadowolony klient, który kupuje, to zadowolony klient, który kupuje).

Pamiętaj również, że jeśli wyrobisz sobie dobrą markę, to może się okazać, że zadowolony klient przy okazji luźnej rozmowy z innym przedsiębiorcą poleci Was, bo był zadowolony z efektu pracy i współpracy z Wami. Dzięki temu szukanie okazji przestanie być problemem.

Podsumowanie
------------

W powyższym artykule nie poruszyłem w ogóle szczegółów dotyczących tego, jakich bibliotek użyłem podczas implementacji tworzonego narzędzia RPA. Jest to celowy zabieg, ponieważ każdy ma swoje preferencje i nie do każdego zadania, wybrane przeze mnie biblioteki będą się nadawały. Jeśli jednak jesteś tym zainteresowany, to proszę, daj znać, a postaram się zaspokoić Twoją ciekawość.

Mam jednak nadzieję, że tak bardzo przekrojowy opis, który bardziej skupia się na podejściu do problemu oraz ewentualnych pułapkach będzie dla Ciebie przydatny. Wiele z elementów, które omówiłem powyżej, sprawdzi się również przy rozpoczynaniu wszelkich testerskich projektów, z którymi na co dzień masz styczność w swojej zawodowej pracy. Szczególnie jeśli dostaniesz zadanie rozpoczęcia automatycznego testowania jakiegoś projektu.

Niespodzianka
-------------

Jak wspomniałem na początku artykułu, wspólnie z firmą `Muscat Eyewear <https://muscat.pl>`_ przygotowaliśmy niespodziankę. Każdego czytelnika mojego bloga, który zapisze się do mojego newslettera (formularz poniżej), otrzyma w mailu **kod zniżkowy na 50 zł na zakup dowolnych okularów korekcyjnych lub przeciwsłonecznych**. Kod jest ważny do *końca września* więc macie jeszcze chwilę, aby również uzyskać dofinansowanie zakupu okularów korekcyjnych u Waszego pracodawcy.

.. image:: /images/posts/20200813/muscat.png
    :align: center
    :width: 320
    :target: https://muscat.pl
