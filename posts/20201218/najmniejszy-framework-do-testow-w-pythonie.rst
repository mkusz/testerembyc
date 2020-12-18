.. title: Najmniejszy framework do testów w Pythonie
.. slug: najmniejszy-framework-do-testow-w-pythonie
.. date: 2020-12-18 15:12:26 UTC+01:00
.. tags: python, code16challenge, requests, unittest
.. category: python
.. link: 
.. description: Potęga Pythona w 16 linijkach kodu
.. type: text
.. previewimage: /images/posts/20201218/bobcat.jpg
.. template: newsletter.tmpl

.. figure:: /images/posts/20201218/bobcat.jpg
    :align: center
    :target: /posts/20201218/najmniejszy-framework-do-testow-w-pythonie/

    Zdjęcie zaczerpnięte z portalu `Pexels <https://www.pexels.com/>`_


Jeśli interesujesz się choć trochę programowaniem, to zapewne trafiłeś kiedyś na artykuły porównujące ile linii kodu potrzebnych jest do stworzenia uwielbianego *hello world* w różnych językach programowania. Sam zresztą pokazywałem, w pierwszym wpisie na tym blogu ile linii kodu potrzebne jest w Pythonie, aby wyświetlić proste `witaj świecie </posts/20191024/witaj-swiecie/>`_. A co by się stało, gdybyśmy w tych rozważaniach poszli ciut dalej i zastanowili się, ile potrzebnych jest linii kodu, aby stworzyć działający framework do testów?

.. more

Ale skąd w ogóle ten pomysł?
============================

Historia tego pomysłu sięga początków tegorocznej pandemii i sławnego `#hot16challenge <https://www.siepomaga.pl/hot16challenge>`_ oraz pomysłu o jakim opowiedział mi wtedy **Bartosz Kita** z bloga `Tester Programuje <https://testerprogramuje.pl/>`_. Sam pomysł był prosty i do tej pory mocno żałuję, że pomimo gotowego kodu, nie znalazłem czasu na nagranie krótkiego filmu, gdzie bym o tym opowiedział (wybacz Bartek, bo pomysł był przedni). Ale o co chodzi? Najlepiej niech sam autor o tym opowie:

.. youtube:: jLV1VaneJWs

Wiem, że akcja już dawno przestała być modna, ale skoro kod powstał, a wiele osób prosiło mnie o ciut bardziej techniczne wpisy, to pobawmy się wspólnie troszkę Pythonem i pokażę Ci jego siłę. Jak zapewne domyślasz się po treści powyższego filmu, spróbuję pokazać Ci, że w **16 linijkach kodu** da się stworzyć działający **framework do testów**.

Zanim jednak to zrobię, miej na uwadze, że zostanie tu złamane kilka reguł formatowania kodu Pythona, które są opisane w dokumencie `PEP-8 - Style Guide for Python Code <https://www.python.org/dev/peps/pep-0008/>`_. Bez tych drobnych, celowych *wypaczeń*, nie udało mi się upakować kodu w tak małej ilości linijek.

Założenia
=========

Aby cała powyższa zabawa w ogóle nam się udała, musimy przyjąć pewne założenia i mieć świadomość, że stworzony framework będzie miał dosyć ograniczoną funkcjonalność (choć bardzo łatwo będzie można ją rozszerzyć). Założenia jakie przyjąłem, prezentują się następująco:

* zajmiemy się testami REST API - wysyłając zapytanie do REST API, w odpowiedzi zawsze otrzymamy `kod statusu odpowiedzi <https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml>`_, a tworzony framework będzie sprawdzał właśnie ten kod,
* dane testowe będą przechowywane poza kodem Pythona - takie podejście pomoże nam zaoszczędzić kilka linijek kodu, ale również umożliwi tworzenie kolejnych testów tylko poprzez edycję zewnętrznego pliku oraz nie będzie wymagało znajomości Pythona,
* framework powinien wygenerować jakąś formę raportu - co to za testy, po których uruchomieniu nie dostaniemy jakiegoś raportu?

Założenia są proste ale na pierwszy rzut oka wydaje się to praktycznie niemożliwe. A gdzieś napisałem, że tworzony kod będzie prosty i że to zadanie będzie łatwe do wykonania?

Co będziemy testować?
=====================

Skoro już wiemy, że chcemy testować jakieś REST API, to przydało by się nam takie. Po adresem https://reqres.in/ znajduje się przykładowe i dosyć proste API. Pomimo swoich ograniczeń na nasze potrzeby jest w zupełności wystarczające.

Potrzebne biblioteki
====================

Skoro znamy już założenia zastanówmy się z czego możemy skorzystać, aby maksymalnie ograniczyć potrzebną ilość linii kodu.

request
-------

Najpopularniejszą i najbardziej rozbudowaną biblioteką do wysyłania zapytań do REST API jest biblioteka `requests <https://requests.readthedocs.io/en/master/>`_. Biblioteka wymaga instalacji, gdyż nie jest częścią biblioteki standardowej i możemy to zrobić wydając następującą komendę:

.. code:: bash

    python -m pip install requests

unittest
--------

`Unittest <https://docs.python.org/3/library/unittest.html>`_ to wbudowany w bibliotekę standardową framework do przeprowadzania testów, a więc nie wymaga dodatkowej instalacji. W naszym kodzie posłuży nam jako podstawa budowanego frameworka.

xmlrunner
---------

`Xmlrunner <https://unittest-xml-reporting.readthedocs.io/en/latest/>`_ jest tzw. runnerem dla frameworka *unittest*, który umożliwia generowanie raportów w formacie XML, zgodnym ze `schematem JUnit <https://github.com/windyroad/JUnit-Schema>`_. Format ten akceptowany jest przez większość narzędzi CI/CD, a więc budowany framework można bardzo prosto z takimi narzędziami zintegrować oraz przeglądać raport z przeprowadzonych testów. W celu instalacji biblioteki, należy wydać następującą komendę:

.. code:: bash

    pip install unittest-xml-reporting


json
----

Standardowa biblioteka Pythona, a więc nie wymaga instalacji. Biblioteka ta umożliwia obsługę danych w formacie `JSON <https://www.json.org/json-en.html>`_ i posłuży do odczytu danych testowych z zewnętrznego pliku.

Kodujemy
========

Skoro mamy już wszystkie potrzebne elementy tej całej układanki to zacznijmy w końcu pisać ten kod. Na początek stwórzmy sobie 2 pliki: :code:`tests.py` i :code:`tests.json`. Jak już będziemy je mieli to przejdźmy do pliku `tests.py`.

import
------

Każdy szanujący się programista Pythona, postępuje zgodnie z regułami i na początku pliku importuje wszystkie biblioteki. Dobry obyczaj mówi, że import każdej biblioteki powinie znajdować się w oddzielnej linijce, jednakże, ze względu na ograniczone miejsce, w naszym kodzie wszystkie importy zostaną wykonane w jednej linii:

.. code:: python

    import unittest, xmlrunner, json, requests

Kolejność importów jest dowolna (choć i tutaj są pewne reguły, które warto stosować).

Pierwszy test
-------------

Wiemy, że chcemy sprawdzać status odpowiedzi na wysłane żądanie, a więc zacznijmy od czegoś prostego: wyślemy proste żądanie typu GET na url https://reqres.in/api/users i sprawdzimy kod statusu odpowiedzi.

.. code:: python

    response = requests.get("https://reqres.in/api/users")
    print(response.status_code)

>>> 200

Super. Wiemy, że endpoint działa a kod :code:`200` mówi nam, że wszystko przebiegło bez problemów (:code:`200` oznacza :code:`OK`).

No ale gdzie tu test? No faktycznie nie ma go. Więc przeróbmy troszeczkę ten kod.

.. code:: python

    response = requests.get("https://reqres.in/api/users")
    assert response.status_code == 200

Po uruchomieniu tego kodu nic się nie wyświetli, gdyż wszystko jest w porządku. W ramach samdzielnego ćwiczenia sprawdź co się stanie jak podmienisz :code:`200` na :code:`202`.

Czy to już koniec? Na razie mamy 4 linie kodu (po importach zostawiamy jedną linię przerwy) a mamy do dyspozycji ich aż 16. No więc co dalej?

Test w unittest
---------------

Przeróbmy teraz kod tak, aby wykorzystać dobrodziejstwa unittest.

.. code:: python

    class Tests(unittest.TestCase):
        def test_get_all_users(self):
            response = requests.get("https://reqres.in/api/users")
            self.assertEqual(response.status_code, 200)

Odpowiedzmy sobie, co tu się wydarzyło:

* stworzyliśmy klasę testową :code:`Tests`, która dziedziczy po :code:`unittest.TestCase`
* przenieśliśmy nasz test do metody :code:`test_get_all_employment` (w unittest, wszystkie metody testowe, muszą zaczynać się od słowa *test*)
* podmieniliśmy :code:`assert` na :code:`assertEqual`

Niestety przy próbie uruchomienia, nic się nie wydarzy.

Naprawmy to poprzez dodanie poniższego kodu na końcu pliku oraz go uruchommy:

.. code:: python

    if __name__ == '__main__':
        unittest.main()

>>> .
----------------------------------------------------------------------
Ran 1 test in 0.281s
OK

Wygląda to już zdecydowanie lepiej, ale to nie koniec naszej zabawy. Zajmijmy się teraz przechowywaniem danych testowych w pliku.

tests.json
----------

Zanim jednak dojdziemy do samego pliku, zmieńmy jeszcze jedną rzecz w naszym kodzie, tak abyś lepiej zrozumiał dlaczego pewne rzeczy działają. Zauważ, że w naszym kodzie, do tej pory używaliśmy :code:`requests.get`. Czy da się to jakoś sparametryzować? Jak to mawiają `ciekawość to pierwszy stopień do piekła` to poszukajmy do niego drzwi. Jeśli do edycji kodu, używasz PyCharma to klikając w :code:`get` z przytrzymanym klawiszem :code:`CTRL` przejdziesz to implementacji metody :code:`requests.get`. I cóż tam widzimy (pominąłem komentarze)?

.. code:: python

    def get(url, params=None, **kwargs):
        kwargs.setdefault('allow_redirects', True)
        return request('get', url, params=params, **kwargs)

Pomijając pobieranie domyślnych wartości dla parametru :code:`allow_redirects` widzimy, że tak naprawdę metoda :code:`requests.get` to wywołanie metody :code:`requests.requst` z odpowiednimi parametrami, gdzie pierwszy parametr określa nam metodę wysyłki żądania (listę wszystkich parametrów można znaleźć w `dokumentacji <https://requests.readthedocs.io/en/master/api/>`_).

No więc skoro samo biblioteka :code:`requests` tak robi, to dlaczego nie możemy my tak postąpić? Nasz kod po zmianach będzie wyglądał tak:

.. code:: python

    import unittest, xmlrunner, json, requests

    class Tests(unittest.TestCase):
        def test_get_all_users(self):
            response = requests.request(
                method='GET',
                url="https://reqres.in/api/users"
            )
            self.assertEqual(response.status_code, 200)

    if __name__ == '__main__':
        unittest.main()

Zauważ, że podałem wprost nazwy parametrów przekazywanych do :code:`requests.request`.

Przejdźmy zatem do przeniesienia danych testowych do pliku :code:`tests.json`. W pliku musimy przechować tak na prawdę 3 informacje dla pojedynczego testu (a dokładniej to 4, ale o tym będę mówił troskę dalej):

- metoda do wysyłki żądania
- url endpointu, na który wysyłamy żądanie
- spodziewany kod statusu odpowiedzi

Zawartość pliku :code:`tests.json` prezentuje się tak:

.. code:: json

    {
      "request": {
        "method": "GET",
        "url": "https://reqres.in/api/users"
      },
      "assert": {
        "statusCode": 200
      }
    }

Przeróbmy teraz nasz kod, tak aby skorzystał z tych danych:

.. code:: python

    import unittest, xmlrunner, json, requests

    data = json.load(open('tests.json', 'r'))

    class Tests(unittest.TestCase):
        def test_get_all_users(self):
            response = requests.request(
                method=data['request']['method'],
                url=data['request']['url'],
            )
            self.assertEqual(response.status_code, data['assert']['statusCode'])

    if __name__ == '__main__':
        unittest.main()

Co tu się zmieniło? Do zmiennej :code:`data` wczytaliśmy zawartość pliku :code:`tests.json` oraz podmieniliśmy wszystkie wartości testu na te odczytane z pliku. Zauważ, że dane pobrane z pliku i umieszczone w zmiennej :code:`data` tworzą słownik.

Zanim przejdziemy dalej, popatrz na wartości wstawiane do argumentów wywołania metody `:code:requests.request`. Nie zauważasz tam pewnej prawidłowości?

Podpowiem: porównaj nazwę argumentu, do którego wstawiane są dane, z nazwą klucza z jakiego te dane są pobierane.

Może da się to jakoś wykorzystać na naszą korzyść i zaoszczędzić ciut miejsca w kodzie? Przecież w tym momencie mamy już 14 linii kodu, a nie mamy jeszcze ani, większej ilości testów, ani raportów.

Rozpakowywanie słownika
-----------------------

Jeśli czytałeś mój artykuł dotyczący `dekoratorów w Pythonie </posts/20200109/dekoratory-w-pythonie/>`_ to wspominam w nim o 2 sposobach przekazywania argumentów do funkcji: przez `args i kwargs </posts/20200109/dekoratory-w-pythonie/index.html#args-i-kwargs>`_ (jeśli nie wiesz o co chodzi to zanim przejdziesz dalej, polecam się z tym zapoznać). W naszym kodzie, przekazanie argumentów do metody :code:`requests.request` wykonaliśmy właśnie przy użyciu :code:`kwargs`, a więc defakto jako słownik, gdzie kluczem jest nazwa argumentu, a wartością danego klucza, wartość argumentu. Mówię o tym kawałku kodu:

.. code:: python

    response = requests.request(
        method=data['request']['method'],
        url=data['request']['url'],
    )

W Pythonie istnieje mechanizm tzw. `rozpakowywania słownika`, który można wykorzystać do przekazania wartości do wywoływanej metody. Przyjrzyj się poniższemu zapisowi:

.. code:: python

    response = requests.request(**data['request'])

Zauważ proszę, że wykorzystałem w nim zapis :code:`**` przed nazwą zmiennej, która jest słownikiem. Jak to działa? Zmienna :code:`data['request']` przechowuje słownik z 2 kluczami: :code:`method` i :code:`url`. Zapis :code:`**` powoduje *rozpakowanie* słownika, a więc w przypadku wywołania metody, powoduje przypisanie konkretnym argumentów, wartości z odpowiadających ich nazwom kluczy ze słownika. Dlatego też, oba powyższe zapisy są ze sobą równoważne. Jak więc teraz prezentuje się nasz kod?

.. code:: python

    import unittest, xmlrunner, json, requests

    data = json.load(open('tests.json', 'r'))

    class Tests(unittest.TestCase):
        def test_get_all_users(self):
            response = requests.request(**data['request'])
            self.assertEqual(response.status_code, data['assert']['statusCode'])

    if __name__ == '__main__':
        unittest.main()

Zauważ, że z 14 linii kodu, zredukowaliśmy zapis do 11 linii. Można tutaj jeszcze jedną rzecz uprościć, a mianowicie pozbyć się zmiennej pomocniczej :code:`response` i nasz kod będzie się prezentował w następujący sposób:

.. code:: python

    import unittest, xmlrunner, json, requests

    data = json.load(open('tests.json', 'r'))

    class Tests(unittest.TestCase):
        def test_get_all_users(self):
            self.assertEqual(requests.request(**data['request']).status_code, data['assert']['statusCode'])

    if __name__ == '__main__':
        unittest.main()

Zeszliśmy tym samym do 10 linii kodu. Na co wykorzystamy pozostałe 6 linii?

Generator testów z danych testowych
-----------------------------------

Dochodzimy do najfajniejszej części tego wpisu, czyli jeszcze większej *magii* niż zapis z :code:`**`. Przeróbmy teraz nasz kod tak, aby metoda z testem nie była zdefiniowana bezpośrednio w klasie testów, ale umieszczona w niej w sposób dynamiczny. Zerknij na poniższy kod:

.. code:: python

    import unittest, xmlrunner, json, requests

    data = json.load(open('tests.json', 'r'))

    class Tests(unittest.TestCase):
        pass

    def abstract_test(self):
        self.assertEqual(requests.request(**data['request']).status_code, data['assert']['statusCode'])

    setattr(Tests, 'test_get_all_users', abstract_test)

    if __name__ == '__main__':
        unittest.main()

Tak na prawdę w dalszym ciągu pod względem funkcjonalnym oraz końcowego wynika, powyższy kod jest tym samym co poprzedni, gdzie metoda :code:`test_get_all_users` był zdefiniowa wewnątrz klasy :code:`Tests`.

Jak to działa?

1. Klasa :code:`Tests` w kodzie została zaimplementowana tak, że nic poza dziedziczeniem po klasie :code:`unittest.TestCase` nie robi nic poza tym. Jest po prostu pustą definicją.

2. Metoda służąca do wysyłania żądania do endpointu, znajdują się teraz poza ciałem klasy oraz została nazwana :code:`abstract_test`. Sam sposób wysyłania żądania się nie zmienił.

3. Następnie wywołujemy metodę `setattr <https://docs.python.org/3/library/functions.html#setattr>`_, która jest metodą wbudowaną w język Python. Pozwala ona na wstawienie do obiektu, nowego atrybutu oraz przypisania mu wartości (o tym również wspominałem w artykule dotyczącym dekoratorów w sekcji `funkcja jest obiektem </posts/20200109/dekoratory-w-pythonie/index.html#funkcja-jest-obiektem>`_. Zauważ, że jej wywołanie przyjęło 3 argumenty:

* obiekt do którego wstawiamy - u nas jest to klasa :code:`Tests`,
* nazwę atrybutu pod jakim będzie znajdowała się wstawiona wartość - u nas jest to :code:`test_get_all_users`,
* wartość jaka będzie przypisana do atrybutu - nas jest to adres w pamięci metody :code:`abstract_test` (widać to po braku :code:`()` na końcu).

Jeśli wywołamy powyższy kod to w dalszym ciągu otrzymujemy taki sam wynik.

No dobra. Umiemy już dynamicznie wstawić metodę z testem do obiektu, ale to jeszcze nie do końca jest generator. Żeby nasz kod umiał coś więcej musimy dokonać jeszcze małych przeróbek w obu naszych plikach.

Zacznijmy od pliku :code:`tests.json`:

.. code:: json

    {
      "test_get_all_users": {
        "request": {
          "method": "GET",
          "url": "https://reqres.in/api/users"
        },
        "assert": {
          "statusCode": 200
        }
      }
    }

Tu zmiany są niewielkie, bo tak na prawdę, *nazwaliśmy* tylko już istniejące dane jako :code:`test_get_all_users`.

Teraz kolej na plik :code:`main.py`:

.. code:: python

    import unittest, xmlrunner, json, requests

    data = json.load(open('tests.json', 'r'))

    class Tests(unittest.TestCase):
        pass

    def add_test(cls, name):
        def abstract_test(self):
            self.assertEqual(requests.request(**data[name]['request']).status_code, data[name]['assert']['statusCode'])
        setattr(cls, name, abstract_test)

    for test_name in data.keys():
        add_test(Tests, test_name)

    if __name__ == '__main__':
        unittest.main()

Co zmieniliśmy?

1. Metoda :code:`abstract_test` oraz wywołanie metody :code:`setattr` ukryte zostało w metodzie :code:`add_test`. Zauważ, że metoda ta przyjmuje 2 atrybuty:

* :code:`cls` - to klasa do której będziemy dodawać test,
* :code:`name` - to nazwa testu jaki będziemy dodawać.

2. W metodzie :code:`abstract_test` zmienił się sposób dotarcia do danych testowych w słowniku przechowywanym w zmiennej :code:`data`. Doszedł tam po prostu dodatkowy poziom zagnieżdżenia wynikający ze zmiany struktury danych w pliku :code:`tests.json`. Zauważ również, że zmienna :code:`name` nie jest argumentem wywołania metody :code:`abstract_test`, a metody nadrzędnej, czyli :code:`add_test`. Jak to możliwe, że to działa? Otóż zmienna :code:`name` staje się dla metody :code:`add_test` zmienną globalną, ze względu na jej zagnieżdżenie wewnątrz metody :code:`add_test`.

3. Wywołanie :code:`settatr` korzysta teraz ze zmienne :code:`name`, a nie bezpośredniej nazwy.

4. Dodaliśmy pętlę :code:`for` iterującą po kluczach słownika ze zmiennej :code:`data`. Te klucze to tak na prawdę nazwy testów z pliku `tests.json` (w tym momencie mamy tylko jeden klucz o wartości :code:`test_get_all_users`).

Czy to wszystko?

Mamy 3 problemy:

1. Mamy tylko 1 test.
3. Brakuje nam jeszcze raportów.
3. Mamy 17 linii kodu (o 1 za dużo).

Więcej testów
-------------

Skoro tyle się napracowaliśmy to dorzućmy więcej testów. Jak możesz się domyślić, aby dopisać nowe testy, wystarczy odpowiednie dane umieścić w pliku :code:`tests.json`. Poniżej przykładowy zestaw testów:

.. code:: json

    {
      "test_get_all_users": {
        "request": {
          "method": "GET",
          "url": "https://reqres.in/api/users"
        },
        "assert": {
          "statusCode": 200
        }
      },
      "test_get_users_id_2": {
        "request": {
          "method": "GET",
          "url": "https://reqres.in/api/users/2"
        },
        "assert": {
          "statusCode": 200
        }
      },
      "test_get_non_existing_user": {
        "request": {
          "method": "GET",
          "url": "https://reqres.in/api/users/23"
        },
        "assert": {
          "statusCode": 404
        }
      },
      "test_create_new_user": {
        "request": {
          "method": "POST",
          "url": "https://reqres.in/api/users",
          "json": {
            "name": "testerembyc",
            "jon": "tester"
          }
        },
        "assert": {
          "statusCode": 201
        }
      }
    }


Raporty i 16 linii kodu
-----------------------

To zadanie to w zasadzie już tylko drobna formalność. Spójrz na poniższy kod:

.. code:: python

    import unittest, xmlrunner, json, requests

    data = json.load(open('tests.json', 'r'))

    class Tests(unittest.TestCase): pass

    def add_test(cls, name):
        def abstract_test(self):
            self.assertEqual(requests.request(**data[name]['request']).status_code, data[name]['assert']['statusCode'])
        setattr(cls, name, abstract_test)

    for test_name in data.keys():
        add_test(Tests, test_name)

    if __name__ == '__main__':
        unittest.main(testRunner=xmlrunner.XMLTestRunner())

Co się zmieniło:

1. Implementacja klasy :code:`Tests` mieści się w jednej linii (tak wiem znów naginam dobre zasady formatowania kodu).
2. W wywołaniu metody :code:`unittest.main` jako argument :code:`testRunner` podałem do tej pory nie wykorzystywany :code:`xmlrunner`. Dzięki temu po uruchomieniu testów w konsoli zobaczymy poniższy tekst:

.. code:: bash

    Running tests...
    ----------------------------------------------------------------------
    ....
    ----------------------------------------------------------------------
    Ran 4 tests in 0.609s

    OK

    Generating XML reports...

Dodatkowo w katalogu z naszymi plikami, pojawi się plik o rozszerzeniu :code:`.xml`, który jest naszym *raportem* z przeprowadzonych testów.

Podsumowanie
============

Zmieściliśmy się w 16 linijkach kodu?

Chyba nam się udało, choć nagięliśmy przy okazji kilka reguł dotyczących formatowania kodu w Pythonie, ale udało nam się zachować względną czytelność i dosyć sporą funkcjonalność.

Mam nadzieję, że ten wpis pokaz Ci jak potężnym narzędziem potrafi być Python.

Czy da się coś więcej z tego kodu wykrzesać?

Oczywiście, że tak, ale wtedy nie zmieścimy się w 16 linijkach kodu. Jako ćwiczenie dla Ciebie mogę podpowiedzieć, że przy niewielkim nakładzie pracy, można dodać dodatkowe sprawdzenia, np. czy dane, które otrzymujemy w odpowiedzi na wysłane żądanie, są danymi jakich się spodziewamy. Jak to zrobić, to już zostawiam Tobie jako dalsze ćwiczenie swoich szarych komórek (ten kod dla mnie był takim właśnie ćwiczeniem).

PS. Cały powyższy kod znajdziesz również w poniższym `repozytorium w GitHubie <https://github.com/mkusz/the_smallest_rest_api_testing_framework>`_.
