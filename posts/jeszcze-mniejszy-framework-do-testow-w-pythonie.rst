.. title: Jeszcze mniejszy framework do testów w Pythonie
.. slug: jeszcze-mniejszy-framework-do-testow-w-pythonie
.. date: 2021-01-06 20:53:56 UTC+01:00
.. tags: python, requests, unittest, lambda
.. category: python
.. link: 
.. description: Potęgi Pythona ciąg dalszy
.. type: text
.. previewimage: /images/posts/jeszcze-mniejszy-framework-do-testow-w-pythonie.jpg
.. template: newsletter.tmpl

.. figure:: /images/posts/jeszcze-mniejszy-framework-do-testow-w-pythonie.jpg
    :align: center
    :target: /posts/jeszcze-mniejszy-framework-do-testow-w-pythonie/

    Zdjęcie zaczerpnięte z portalu `Pexels <https://www.pexels.com/>`_

Poniższy wpis jest kontynuacją poprzedniego wpisu, gdzie próbowałem pokazać Ci jak w Pythonie można stworzyć `najmniejszy framework do testów </posts/20201218/najmniejszy-framework-do-testow-w-pythonie/>`_.

Czym się dzisiaj wspólnie zajmiemy?

Otóż odpowiemy sobie na 2 pytania:

* Czy aby na pewno 16 linijek kodu, to najmniejsza możliwa ilość?
* Czy da się w tych 16 linijkach kodu stworzyć bardziej rozbudowany framework?

Odpowiedzi na te pytania zapewne się domyślasz, ale jeśli chcesz wiedzieć jak, to nie pozostaje Ci nic innego jak zagłębić się w dalszą część tego wpisu.

.. more

Gdzie skończyliśmy?
===================

Dla małego przypomnienia zacznijmy w miejscu, gdzie skończyliśmy. Kod naszego mini frameworka wygląda tak:

.. code:: python

    import unittest, xmlrunner, json, requests

    class Tests(unittest.TestCase): pass

    def add_test(cls, name, data):
        def abstract_test(self):
            self.assertEqual(requests.request(**data['request']).status_code, data['assert']['statusCode'])
        setattr(cls, name, abstract_test)

    with open('tests.json', 'r') as json_file:
        for test_name, test_data in json.load(json_file).items():
            add_test(Tests, test_name, test_data)

    if __name__ == '__main__':
        unittest.main(testRunner=xmlrunner.XMLTestRunner())

W pogoni za rozumem
===================

Kosmetyka
---------

Póki co nasz kod jest w miarę czytelny, pomimo złamania kilku reguł. Skoro i tak je łamiemy, to idźmy ciut głębiej i uprośćmy ten kod do poniższej postaci.

.. code:: python

    import unittest, xmlrunner, json, requests

    def add_test(cls, name, data):
        def abstract_test(self):
            self.assertEqual(requests.request(**data['request']).status_code, data['assert']['statusCode'])
        setattr(cls, name, abstract_test)

    with open('tests.json', 'r') as json_file:
        class Tests(unittest.TestCase): pass
        for test_name, test_data in json.load(json_file).items():
            add_test(Tests, test_name, test_data)

    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Zmian jest niewiele i to tylko kosmetyka:

* usunęliśmy sprawdzanie z jakiego skryptu jest uruchamiany nasz kod,
* przerzuciliśmy definicję klasy :code:`Test` do wnętrza bloku :code:`with open(...)`.

Powyższa kosmetyka redukuję ilość linii kodu do 13.

Patrząc na kod, możemy zauważyć, że jednym z większych bloków jest definicja funkcji :code:`add_test`. Zastanówmy się czy da się coś z tym tematem zrobić.

Wyrażenia Lambda
----------------

Zanim jednak będziemy bardziej odchudzać kod frameworku, musimy poznać czym są tzw. **wyrażenia Lambda** w Pythonie.

**Wyrażenia Lmabda** to krótki, anonimowe funkcje (opisane w `PEP 312 <https://www.python.org/dev/peps/pep-0312/>`_ oraz `dokumentacji <https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions>`_). Funkcje takie są określane jako anonimowe ponieważ zdefiniowane są w miejscu ich użycia. **Funkcje Lambda** mogą przyjmować dowolną liczbę argumentów ale mogą zawierać tylko jedno wyrażenie. Ale jak działa Lambda?

Zacznijmy od czegoś prostego, czyli suma 2 liczb:

.. code:: python

    add = lambda a, b: a + b
    add(1, 3)

>>> 4

Nic nadzwyczajnego, ale żeby tradycji stało się zadość, kilka słów wyjaśnienia jak to działa:

1. Tworzymy wyrażenie Lambda, które przyjmuje dwie zmienne: :code:`a` i :code:`b` oraz wykonuje operację dodawania na tych zmiennych.
2. Wyrażenie to przypisujemy do zmiennej :code:`add`. W tym momencie zmienna :code:`add` staje się funkcją, której ciałem jest wyrażenie Lambda (opisywałem ten mechanizm we wpisie dotyczącym `dekoratorów </posts/20200109/dekoratory-w-pythonie/>`_, gdzie tłumaczyłem, że `funkcja jest obiektem </posts/20200109/dekoratory-w-pythonie/#funkcja-jest-obiektem>`_).
3. Wywołujemy tak stworzoną funkcję z 2 parametrami. W wyniku wywołania otrzymujemy wynik dodawania.

Odpowiednikiem takiego wyrażenia będzie poniższy kod:

.. code:: python

    def add(a, b):
        return a + b

    add(1, 3)

>>> 4

Zysk niby niewielki, ale zawsze coś. A co, gdybyśmy chcieli osiągnąć coś ciekawszego? Zobaczmy co się stanie, jak tradycyjna metoda, w wyniku będzie zwracała wyrażenie lambda:

.. code:: python

    def multiplier(n):
        return lambda a: a * n

    multiplier_1k = multiplier(1000)

    multiplier_1k(3)

>>> 3000

Zaczyna się robić ciekawiej. Zauważ, że w ten sposób moglibyśmy bardzo szybko zdefiniować inne metody np. :code:`multiplier_10k = multiplier(10000)`, czy też :code:`doubler = multiplier(2)`. Zaczynasz rozumieć z jak potężnym narzędziem mamy do czynienia?

Wszystko super, ale jak z tego skorzystać w naszym frameworku?

Lambda po raz pierwszy
----------------------

Skoro już wiemy, że przy użyciu wyrażenia lambda, można zastąpić definicję jakiejś meto, to spróbujmy to zaimplementować w naszym kodzie. Jak? Zerknij poniżej:

.. code:: python

    import unittest, xmlrunner, json, requests

    def add_test(cls, name, data):
        setattr(cls, name, lambda self: self.assertEqual(requests.request(**data['request']).status_code, data['assert']['statusCode']))

    with open('tests.json', 'r') as json_file:
        class Tests(unittest.TestCase): pass
        for test_name, test_data in json.load(json_file).items():
            add_test(Tests, test_name, test_data)

    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Myślę, że zmiany nie wymagają większego komentarza, bo jedyne co tutaj zrobiliśmy to zastąpiliśmy definicję oraz wywołanie metody :code:`abstract_test` wyrażeniem lambda. W wyniku, odchudziliśmy nasz kod do 11 linii kodu. Jest nieźle, ale może da się lepiej?

Lambda po raz drugi
-------------------

Skoro udało nam się pozbyć definiowania jednej metody, to może uda nam się wyrzucić jeszcze jedną?

.. code:: python

    import unittest, xmlrunner, json, requests

    add_test = lambda cls, name, data: setattr(cls, name, lambda self: self.assertEqual(requests.request(**data['request']).status_code, data['assert']['statusCode']))

    with open('tests.json', 'r') as json_file:
        class Tests(unittest.TestCase): pass
        for test_name, test_data in json.load(json_file).items():
            add_test(Tests, test_name, test_data)

    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Zauważ, że dalej podążamy utartym schematem. Wykonajmy jeszcze jeden drobny zabieg, a więc przenieśmy przypisanie Lambdy do zmiennej w inne miejsce:

.. code:: python

    import unittest, xmlrunner, json, requests

    with open('tests.json', 'r') as json_file:
        class Tests(unittest.TestCase): pass
        add_test = lambda cls, name, data: setattr(cls, name, lambda self: self.assertEqual(requests.request(**data['request']).status_code, data['assert']['statusCode']))
        for test_name, test_data in json.load(json_file).items():
            add_test(Tests, test_name, test_data)

    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Odchudziliśmy już nasz kod do 9 linii przy zachowaniu tej samej funkcjonalności. Czy to odchudzanie kiedyś się skończy? Tak, ale jeszcze nie w tym momencie. Zauważ, że sama definicja klasy :code:`Tests`, metody testowej :code:`add_test` oraz jej wielokrotne wywołanie zajmują aż 4 linijki kodu. Może damy radę jeszcze coś tutaj pokombinować?

type
----

Metoda :code:`type` to jedna z metod wbudowanych Pythona i nie wymaga importu (jest zawsze dostępna, podobnie jak używana przez nas metoda :code:`setattr`). Większość osób programujących w Pythonie używa tej metody do sprawdzania typu obiektu. Nie każdy jednak zdaje sobie sprawę, że :code:`type` można wykorzystać też w innym celu. `type <https://docs.python.org/3/library/functions.html?highlight=type#type>`_ możemy również wykorzystać do dynamicznego tworzenia obiektów w Pythonie. Jak to działa? To nic skomplikowanego. W obecnym kodzie mamy następującą definicję klasy: :code:`class Tests(unittest.TestCase): pass`. Jest to klasa, dziedzicząca po :code:`unittest.TestCase` oraz nie posiadająca żadnych zmiennych oraz metod. Przy użyciu :code:`type` powyższa definicja wyglądałaby tak: :code:`Tests = type("Tests", (unittest.TestCase,), {})`. Co tu się podziało?

1. Przypisanie do zmiennej już znasz.
2. Jako pierwszy argument podaliśmy nazwę nowego obiektu.
3. Jako drugi argument podaliśmy tuple z obiektami, po jakich dziedziczy nowo tworzony obiekt.
4. Jako trzeci argument podaliśmy pusty słownik, ponieważ nasza klasa jest 'pusta' (to nie do końca prawda, bo dziedziczy, po innym obiekcie, ale nie wnikajmy w to tutaj).

Po podstawieniu do naszego kodu, cały framework wyglądałby następująco:

.. code:: python

    import unittest, xmlrunner, json, requests

    with open('tests.json', 'r') as json_file:
        Tests = type("Tests", (unittest.TestCase,), {})
        add_test = lambda cls, name, data: setattr(cls, name, lambda self: self.assertEqual(requests.request(**data['request']).status_code, data['assert']['statusCode']))
        for test_name, test_data in json.load(json_file).items():
            add_test(Tests, test_name, test_data)

    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Na razie zysku brak. Ciągle mamy 9 linii kodu. Jednak dla wprawnego oka, przyzwyczajonego do kodu pisanego w Pythonie, widać, że coś tutaj możemy teraz uprościć.

dict comprehension
------------------

Zwrot **dict comprehension** nie posiada sensownego tłumaczenia (dlatego będę używał go w oryginale). Czym jest **dict comprehension**? Jest to sposób na uproszczenie zapisu, tworzenia słownika z wykorzystaniem pętli :code:`for`, który opisany jest w `PEP 271 <https://www.python.org/dev/peps/pep-0274/>`_ (istnieje również bardzo podobny mechanizm jak **list comprehension**, który opisany jest w `PEP 202 <https://www.python.org/dev/peps/pep-0202/>`_). W skrócie zapis:

.. code:: python

    keys = ["a", "b", "c", "d"]
    values = [1, 2, 3, 4]

    new_dict = {}

    for i, k in enumerate(keys):
        new_dict[k] = values[i]

    new_dict

>>> {'a': 10, 'b': 20, 'c': 30, 'd': 40}

możemy zastąpić poprzez zapis:

.. code:: python

    keys = ["a", "b", "c", "d"]
    values = [1, 2, 3, 4]

    new_dict = {k: values[i] for i, k in enumerate(keys)}

    new_dict

>>> {'a': 10, 'b': 20, 'c': 30, 'd': 40}

Oczywiście taka konstrukcja może być wykorzystywana też w połączeniu z warunkami itp. Ale jak to może pomóc w odchudzeniu naszego kodu? Trzymaj się krzesła.

Najmniejszy framework
---------------------

Poskładajmy więc to wszystko w całość:

.. code:: python

    import unittest, xmlrunner, json, requests

    with open('tests.json', 'r') as json_file:
        tests_dict = {name: (lambda data: lambda self: self.assertEqual(
            requests.request(**data['request']).status_code, data['assert']['statusCode']))(data)
                      for name, data in json.load(json_file).items()
        }
        Tests = type("Tests", (unittest.TestCase,), tests_dict)

    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Jak to działa? Przy wykorzystaniu **dict comprehension** stworzyliśmy słownik :code:`tests_dict`, który zawiera wszystkie metody testowe, które wcześniej tworzone były wewnątrz pętli :code:`for`. Pominęliśmy również krok dotyczący definicji oraz wywołania metody :code:`add_test`. Nie jest ona w tym momencie konieczna, ponieważ przypisujemy metodę bezpośrednio do zmiennej, która jest wartością słownika, przypisaną do odpowiedniego klucza w tym słowniku. Tak zdefiniowany słownik wstawiamy do dynamicznie tworzonej klasy.

Powyższy kod został przedstawiony w ten sposób, aby choć trochę zachować jego czytelność. Jeśli jednak pozbędziemy się niepotrzebnego formatowania oraz przypisania słownika do zmiennej :code:`tests_dict`, nasz kod będzie wyglądał następująco:

.. code:: python

    import unittest, xmlrunner, json, requests

    with open('tests.json', 'r') as json_file:
        Tests = type("Tests", (unittest.TestCase,), {name: (lambda data: lambda self: self.assertEqual(requests.request(**data['request']).status_code, data['assert']['statusCode']))(data) for name, data in json.load(json_file).items()})

    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Tak dobrze widzisz, że nasz framework mieści się w **6 linijkach kodu** (pomijając puste linie, moglibyśmy odchudzić ten kod do zaledwie 4 linii kodu, co było by już tylko kosmetyczną zmianą). Nie wiem jak dla Ciebie, ale dla mnie to lekki obłęd.

Więcej funkcjonalności
======================

Skoro odchudziliśmy kod frameworku do zaledwie 6 linijek kodu, to do pierwotnie zakładanych 16 linijek, trochę nam brakuje. Spróbujmy wykorzystać ten zapas, do stworzenia testów, które testują coś więcej.

Ale co tak na prawdę możemy dodać do naszego frameworka, aby był bardziej rozbudowany? Do głowy przychodzą mi 3 rzeczy:

1. Dodanie weryfikacji poprawności struktury danych jakimi odpowiada testowany endpoint poprzez weryfikację listy kluczy.
2. Dodanie weryfikacji czasu odpowiedzi danego endpointa.
3. Wsparcie dla wielu plików json, co da nam możliwość rozbicia testów do testowania mniejszych funkcjonalności lub grupowania testów jako suity testów.

Abyśmy mogli wprowadzić powyższe rozszerzenia, potrzebujemy delikatnie zmodyfikować obecny kod poprzez wydzielenie metody z testem z wnętrza słownika. Poniższy kod prezentuje jak tego dokonać:

.. code:: python

    import unittest, xmlrunner, json, requests

    def abstract_test(self, data):
        response = requests.request(**data['request'])
        self.assertEqual(response.status_code, data['assert']['statusCode'])

    with open('tests.json', 'r') as json_file:
        test = lambda data: lambda self: abstract_test(self, data)
        Tests = type("Tests", (unittest.TestCase,), {name: test(data) for name, data in json.load(json_file).items()})

    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Zmiany, które zostały wprowadzone nie powinny być zaskoczeniem, gdyż bardzo podobny kod był w akapicie dotyczącym wykorzystania `lambdy </posts/jeszcze-mniejszy-framework-do-testow-w-pythonie/#lambda-po-raz-pierwszy>`_.

Skoro mamy już podwaliny do dalszej zabawy, rozszerzmy kod o dodatkowe sprawdzenia.

Więcej testów w teście
----------------------

Ponieważ wiemy już co jeszcze chcemy testować, musimy znaleźć sposób na pobranie potrzebnych informacji. Okazuje się, że wszystko tak na prawdę już w naszym kodzie jest, ale jeszcze z tych informacji nie robimy użytku. Te i inne dodatkowe informacje otrzymujemy w `odpowiedzi na wysłane żądanie <https://requests.readthedocs.io/en/master/api/#requests.Response>`_. Wartości, z których możemy skorzystać w teście to:

- :code:`resposne.json()` - zwraca słownik z danymi, którymi odpowiedział endpoint,
- :code:`response.elapsed.total_seconds()` - zawiera czas pomiędzy wysłaniem żądania, a otrzymaniem odpowiedzi.

Istnieją również inne ciekawe wartości, z których można skorzystać, np. :code:`response.headers`, ale nie zmieścilibyśmy się w wymaganej ilości kodu oraz musielibyśmy przechowywać dużo więcej informacji w pliku **json**.

Abyśmy mogli z tych informacji skorzystać, musimy dołożyć pewne dane do naszego pliku :code:`tests.json`:

.. code:: json

    {
      "test_get_all_users": {
        "request": {
          "method": "GET",
          "url": "https://reqres.in/api/users"
        },
        "assert": {
          "statusCode": 200,
          "responseKeys": ["page", "total", "per_page", "total_pages", "data", "support"],
          "responseTime": 0.300
        }
      },
      "test_get_users_id_2": {
        "request": {
          "method": "GET",
          "url": "https://reqres.in/api/users/2"
        },
        "assert": {
          "statusCode": 200,
          "responseKeys": ["data", "support"],
          "responseTime": 0.300
        }
      },
      "test_get_non_existing_user": {
        "request": {
          "method": "GET",
          "url": "https://reqres.in/api/users/23"
        },
        "assert": {
          "statusCode": 404,
          "responseKeys": [],
          "responseTime": 0.300
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
          "statusCode": 201,
          "responseKeys": ["name", "jon", "id", "createdAt"],
          "responseTime": 0.300
        }
      }
    }

Przeróbmy teraz nasz kod, tak aby skorzystał z tych danych:

.. code:: python

    import unittest, xmlrunner, json, requests, glob

    def abstract_test(self, data):
        response: requests.Response = requests.request(**data['request'])
        self.assertEqual(response.status_code, data['assert']['statusCode'])
        self.assertSetEqual(set(response.json().keys()), set(data['assert']['responseKeys']))
        self.assertLessEqual(response.elapsed.total_seconds(), data['assert']['responseTime'])

    with open('tests.json', 'r') as json_file:
        test = lambda data: lambda self: abstract_test(self, data)
        Tests = type("Tests", (unittest.TestCase,), {name: test(data) for name, data in json.load(json_file).items()})

    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Sprawdzenie czasu odpowiedzi nie powinno być zaskoczeniem. Natomiast jeśli chodzi o porównanie struktury danych odpowiedzi wymaga drobnej ekwilibrystyki na danych:

- wyciągamy same klucze z odpowiedzi,
- następnie listę zamieniamy na :code:`set`,
- listę oczekiwanych kluczy również zamieniamy na :code:`set`.

Zapytasz się po co to wszystko? Powód jest prosty, tzn. lista jest zbiorem uporządkowanych wartości i aby 2 listy były uznane za identyczne, obie listy poza zawieraniem tych samych wartości, muszą mieć jeszcze te wartości ułożone w takiej samej kolejności. :code:`set` natomiast w tym względzie jest mniej restrykcyjny i wymaga tylko posiadania takich samych wartości, nie przejmując się zupełnie ich kolejnością. Dzięki temu, zdecydowanie ułatwiamy sobie wprowadzanie danych testowych oraz eliminujemy fałszywe błędy spowodowane przez inną kolejność zwracanych przez endpoint wartości.

W tym momencie mamy 13 linii kodu, więc teoretycznie moglibyśmy coś tutaj jeszcze dorzucić, ale musimy pamiętaj jeszcze o kwestii związanej z obsługą dodatkowy plików **json**. Zanim to zrobimy, musimy wprowadzić dodatkową bibliotekę oraz omówić jedną dodatkową metodę, których użyjemy do tego zadania.

glob
----

Biblioteka `glob <https://docs.python.org/3/library/glob.html>`_ w dużym uproszczeniu służy do wyszukiwania plików i katalogów. Na nasze potrzeby wykorzystamy tylko jedną metodę. a dokładniej `glob.iglob <https://docs.python.org/3/library/glob.html>`_. Wykorzystamy ja do wyszukiwania plików **json**.

globals()
---------

`globals() <https://docs.python.org/3/library/functions.html#globals>`_ to kolejna z wbudowanych metod Pythona, która przechowuje zmienne globalne dla danego modułu. Na tym etapie, gdybyśmy w naszym kodzie wyświetli zawartość, którą zwraca :code:`globals()`, zauważylibyśmy m.in. coś takiego:

>>> 'Tests': <class '__main__.Tests'>

Jest to zmienna, w której przechowywana jest klasa z testami z pojedynczego pliku **json**. Po co nam to wiedzieć? Zapraszam dalej.

Obsługa wielu plików json
-------------------------

Wiemy już czego będziemy używać, a wiec do dzieła. Rozdzielmy najpierw obecny plik :code:`tests.json` na dwa mniejsze.

Pierwszy plik nazwiemy :code:`users_get.json` będzie testował API dotyczące pobierania danych użytkowników z testowanej aplikacji:

.. code:: json

    {
      "test_get_all_users": {
        "request": {
          "method": "GET",
          "url": "https://reqres.in/api/users"
        },
        "assert": {
          "statusCode": 200,
          "responseKeys": ["page", "total", "per_page", "total_pages", "data", "support"],
          "responseTime": 0.300
        }
      },
      "test_get_users_id_2": {
        "request": {
          "method": "GET",
          "url": "https://reqres.in/api/users/2"
        },
        "assert": {
          "statusCode": 200,
          "responseKeys": ["data", "support"],
          "responseTime": 0.300
        }
      },
      "test_get_non_existing_user": {
        "request": {
          "method": "GET",
          "url": "https://reqres.in/api/users/23"
        },
        "assert": {
          "statusCode": 404,
          "responseKeys": [],
          "responseTime": 0.300
        }
      }
    }

Drugi plik nazwiemy :code:`users_create.json` będzie testował API dotyczące tworzenia nowych użytkowników w testowanej aplikacji:

.. code:: json

    {
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
          "statusCode": 201,
          "responseKeys": ["name", "jon", "id", "createdAt"],
          "responseTime": 0.300
        }
      }
    }

Zauważ, że te pliki to tylko prosty podział i nie nastąpiła żadna modyfikacja danych, które oryginalnie zawarte były w pliku :code:`tests.json`.

Przejdźmy teraz do wprowadzenia zmian w kodzie:

.. code:: python

    import unittest, xmlrunner, json, requests, glob

    def abstract_test(self, data):
        response: requests.Response = requests.request(**data['request'])
        self.assertEqual(response.status_code, data['assert']['statusCode'])
        self.assertSetEqual(set(response.json().keys()), set(data['assert']['responseKeys']))
        self.assertLessEqual(response.elapsed.total_seconds(), data['assert']['responseTime'])

    for file_name in glob.iglob("*.json"):
        with open(file_name, 'r') as json_file:
            test = lambda data: lambda self: abstract_test(self, data)
            suite_name = file_name.split('.')[0]
            globals()[suite_name] = type(suite_name, (unittest.TestCase,), {name: test(data) for name, data in json.load(json_file).items()})

    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Wprowadziliśmy kilka drobnych zmian:

1. Dodaliśmy pętle :code:`for`, która iteruje po znalezionych plikach **json**.
2. Dodaliśmy zmienną :code:`suite_name`, która przechowuje nazwę suity testów, a która jest nazwą pliku **json** bez jego rozszerzenia.
3. W dynamiczny sposób dodajemy zmienne globalne, które są oddzielnymi klasami testów, gdzie każdemu plikowi **json** odpowiada jedna klasa z testami.

Gdybyśmy teraz w naszym kodzie wyświetli zawartość, którą zwraca :code:`globals()`, zauważylibyśmy m.in. coś takiego:

>>> 'user_create': <class '__main__.user_create'>, 'user_get': <class '__main__.user_get'>

Ile mamy linii kodu? **15**. Uff. Dalej mieścimy się w zakładanych 16 linijkach kodu.

Podsumowanie
============

To była dosyć długa przygoda (aż dwa i to dosyć rozbudowane wpisy na blogu). Sporo Pythonowych *trików*, które na pierwszy rzut oka nie wydają się oczywiste, ale pokazują potęgę tego języka.  Mam nadzieję, że ta podróż zachęci Cię do poznawania zarówno bibliotek Pythonowych jakie można wykorzystać w testach oraz wewnętrznych mechanizmów, jakie są w ten język wbudowane.

Bonus
=====

Jeśli jesteś purystą i najważniejszą sprawą dla Ciebie jest zgodność kodu z **PEP-8** to poniżej wersja tego frameworka, która jest z nim zgodna oraz w dalszym ciągu mieści się w 16 linijkach kodu (sprawdza status odpowiedzi oraz wspiera wiele plików **json**):

.. code:: python

    import unittest
    import xmlrunner
    import json
    import requests
    import glob

    for file_name in glob.iglob("*.json"):
        with open(file_name, 'r') as json_file:
            tests_dict = {name: (lambda data: lambda self: self.assertEqual(
                requests.request(**data['request']).status_code, data['assert']['statusCode']))(data)
                          for name, data in json.load(json_file).items()}
            suite_name = file_name.split('.')[0]
            globals()[suite_name] = type(suite_name, (unittest.TestCase,), tests_dict)

    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Dodatkowo linki do repozytorium z najciekawszymi (według mnie) wersjami kodu, który omawialiśmy w tej mini serii znajdziesz poniżej:

* `pierwszy artykuł <https://github.com/mkusz/the_smallest_rest_api_testing_framework/tree/first_article>`_,
* `więcej funkcjonalności <https://github.com/mkusz/the_smallest_rest_api_testing_framework/tree/more_functionality>`_,
* `tylko 6 linii kodu <https://github.com/mkusz/the_smallest_rest_api_testing_framework/tree/6_lines>`_,
* `zgodność z PEP-8 <https://github.com/mkusz/the_smallest_rest_api_testing_framework/tree/pep8_valid>`_.

Bonus 2
=======

Po raz kolejny **Jakub Spórna** z bloga https://sporna.dev/ podrzucił jeszcze mniejszą wersję frameworka. Tym razem wziął na tapetę wersję najbardziej rozbudowaną funkcjonalność i postanowił ją troszeczkę bardziej pomniejszyć. Poniżej jego wersja (z minimalną modyfikacją dotyczącej nazw plików):

.. code:: python

    import unittest, xmlrunner, json, requests, glob

    def abstract_test(self, data, response):
        self.assertEqual(response.status_code, data['assert']['statusCode'])
        self.assertSetEqual(set(response.json().keys()), set(data['assert']['responseKeys']))
        self.assertLessEqual(response.elapsed.total_seconds(), data['assert']['responseTime'])

    test = lambda data: lambda self: abstract_test(self, data, requests.request(**data['request']))
    globals().update({file_name: type(file_name, (unittest.TestCase, ), {name: test(data) for name, data in json.loads(open(file_name, 'r').read()).items()}) for file_name in glob.iglob("*.json")})
    unittest.main(testRunner=xmlrunner.XMLTestRunner())

Nie będę już analizował zmian, gdyż pozostawię to dla Ciebie w ramach treningu. A może Ty również pokusisz się o jakaś wersję tego kodu, np. rozbudowaną o jakieś dodatkowe sprawdzenia?
