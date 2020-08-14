.. title: Cała prawda o testach oprogramowania i czym one są
.. slug: cala-prawda-o-testach-oprogramowania-i-czym-one-sa
.. date: 2019-10-29 22:00:00 UTC+01:00
.. tags: testy, definicja, natura testów
.. category: definicja
.. link: 
.. description: Opis natury testów oprogramowania oraz krótka informacja o reperkusjach z tego wynikających
.. type: text
.. previewimage: /images/posts/testerembyc_600x600.png

Tytuł szumny i jeśli zwrócił Twoją uwagę, to bardzo się cieszę, bo pewnie tak jak ja swego czasu nie znalazłeś odpowiedzi na pytanie, czym są testy oprogramowania? Zapewne przeglądając Wikipedię i inne szanowane źródła wiedzy dla testerów, widziałeś mój czytelniku, definicję dotyczącą testowania oprogramowania. Mówi ona m.in. że testowanie oprogramowania jest jednym z procesów zapewnienia jakości wytwarzanego oprogramowania, który ma na celu jego weryfikację (sprawdzenie zgodności ze specyfikacją) oraz walidację (sprawdzenie zgodności z oczekiwaniami użytkownika). Super definicja tylko, że nie mówi nic na temat tego, czym są testy? Dlaczego to takie ważne i co wiedza o tym może nam dać?

.. more

Wiedza o naturze testów
-----------------------

Odpowiedzmy sobie najpierw na drugie pytanie, czyli co wiedza o tym, czym są testy może nam dać? Odpowiedź jest nieoczywista i może wydawać się ciut dziwna, bo pomaga rozwiązać bardzo dużo różnych problemów, np.:

- w przypadku kodu testów automatycznych, odpowiada na pytanie, czy kod testów powinien mieć swoje oddzielne repozytorium, czy może powinien być częścią kodu aplikacji,
- w jakim języku należy pisać testy automatyczne,
- tester kontra programista, kto ma rację i dlaczego tak często testerzy są tak mało doceniani przez programistów.

Dlaczego większość punktów odnosi się do testów automatycznych? Bo z tym typem testów jest zawsze najwięcej problemów i jeśli my, jako tester, musimy podjąć pewne decyzje, aby wdrożyć w naszym projekcie testy automatyczne, im bardziej będziemy świadomi potencjalnych problemów i ich przyczyn, tym łatwiej będzie nam zacząć oraz lepsze będą nasze testy. Wróćmy jednak do sedna tematu, czyli:

Czym są testy?
--------------

Odpowiem, krótko i zwięźle: **testy są produktem**.

Pewnie pytasz teraz: ale dlaczego? Zacznijmy od początku, czyli czym w ogóle jest produkt. Według Wikipedii produktu to obiekt rynkowej wymiany i może być zarówno dobrem materialnym, usługą, miejscem, organizacją lub ideą. Widać, że w przypadku testów, możemy mówić o usłudze, a testerzy w zamian za jej dostarczenie dostają wynagrodzenie. Widać więc, że określenie testów produktem jest jak najbardziej poprawne.

Skoro już wiemy, czym są testy, to teraz zastanówmy się nad tym, kim jest klient, który kupuje usługę testowania.

Klient usługi testowania
------------------------

Znów posłużę się definicją z Wikipedii. Klient to nabywca towaru na własny użytek, a więc ostatnie ogniwo w łańcuchu ekonomicznym, czyli ktoś, kto korzysta z towaru i nie odsprzedaje go dalej (wtedy byłby sprzedawcą lub pośrednikiem).

Zastanówmy się, kto najwięcej korzysta z produktu, jakim są testy: projekt menadżer, dział marketingu, a może programiści? Prześledźmy to po kolei, komu może na tym najbardziej zależeć.

Na pierwszy ogień weźmy *projekt menadżera*? Czy jemu w ogóle potrzebne są testy? Raczej nie, bo dla projekt menadżera najważniejsze jest wytworzenie oprogramowania w zadanym terminie i przy zadanym budżecie. Z tej perspektywy, testy są wręcz dodatkową przeszkodą i kosztem. Tak przynajmniej sądzi część PM-ów (choć widzę na tym polu zdecydowaną poprawę na przestrzeni ostatnich kilku lat), którzy nie rozumieją, co dają testy. A testy polepszają jakość produktu oraz potrafią uchronić od kosztownych błędów, które mogą nadszarpnąć reputację firmy, a w niektórych przypadkach również doprowadzić do utraty życia przez ludzi (przykładem niech będzie, `błąd w oprogramowaniu samochodów marki Toyota <http://www.safetyresearch.net/blog/articles/toyota-unintended-acceleration-and-big-bowl-“spaghetti”-code>`_ , który odpowiadał za obsługę pedału gazu).

No to może *dział marketingu*? Ich zadaniem jest przedstawienie zalet sprzedawanego oprogramowania i argument o tym, że oprogramowanie zostało dobrze przetestowane, nie jest czymś, na co zwraca uwagę klient. Zastanów się, czy kiedykolwiek pytałeś sprzedawcy w sklepie AGD, czy lodówka lub pralka, którą kupujesz, została należycie przetestowana? Jako klient, wychodzisz z założenia, że kupowany produkt będzie po prostu działał poprawnie.

W takim razie może *programiści*? W końcu to oni piszą kod programu i jeśli będzie on miał jakieś błędy, to będą musieli je poprawić. To oni najbardziej ucierpią, gdy będą musieli poprawiać błędy kosztem implementacji nowych funkcjonalności, które powinny byś wdrożone zgodnie z harmonogramem. Również oni będą musieli się tłumaczyć, dlaczego są obsuwy w terminach itp. Wychodzi więc na to, że tak naprawdę to właśnie programiści są głównym odbiorcą produktu, jakim są testy. Niestety wciąż wielu programistów uważa, że testerze przeszkadzają im w pracy, bo ciągle zgłaszają błędy i ogólnie rzecz biorąc, "czepiają się". Na szczęście takie podejście spotykane jest coraz rzadziej, szczególnie wtedy, gdy tworzone są interdyscyplinarne zespoły, gdzie zarówno tester, jak i programista jest członkiem tego samego zespołu, a tzw. dowiezienie implementowanej funkcjonalności zależy również od tego, czy została ona przetestowana.

Podsumowanie
------------

Testy są produktem i to nie zależnie od tego, czy są to testy automatyczne, manualne, penetracyjne, wydajnościowe czy dowolny inny ich rodzaj. Nawet testy jednostkowe (z ang. unit testy) również są produktem, choć w tym przypadku, jest on wytworem wewnętrznym zespołu programistów. Ich cel jest taki sam, czyli dostarczyć informacji na temat oprogramowania i poprawności jego implementacji.

Podejście do testów, jako produktu niesie za sobą bardzo dużo reperkusji i daje testerom, bardzo mocny argument w sporach pomiędzy testerami a programistami, którzy czasem zbyt bardzo próbują ingerować w testy (np. próbując narzucić, w jakim języku programowania należy implementować testy automatyczne). Podniesienie argumentów dotyczących tego, że to testerzy są wytwórcą testów i mają odpowiednią wiedzę oraz umiejętności do ich wytworzenia, a programiści są klientem, który może co najwyżej podpowiadać, w jakim kierunku produkt się rozwija, powinno kończyć wszelkie spory. Tak zdaję sobie sprawę, że nie do każdego takie argumenty mogą trafić oraz że narzucają na testerów odpowiedzialność za testy, ale jak to mówił pewien klasyk "with great power, comes great responsibility".
