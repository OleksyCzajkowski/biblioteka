biblioteka = []
licznik_id = 1  


def generuj_id():
    global licznik_id
    identyfikator = f"id{licznik_id}"
    licznik_id += 1
    return identyfikator


def wczytaj_int(tekst):
    while True:
        liczba = int(input(tekst))
        if liczba > 0:
            return liczba
        else:
            print("Podaj liczbę większą od zera.")


def znajdz_po_id(identyfikator):
    for ksiazka in biblioteka:
        if ksiazka["id"] == identyfikator:
            return ksiazka
    return None


def znajdz_po_tytule(tytul):
    dopasowane = []
    for ksiazka in biblioteka:
        tytul_ksiazki = ksiazka["nazwa"].lower()
        if tytul.lower() in tytul_ksiazki:
            dopasowane.append(ksiazka)
    return dopasowane


def pokaz_menu():
    print()
    print(" MENU BIBLIOTEKI ")
    print("1 - Dodaj książkę")
    print("2 - Wyświetl wszystkie książki")
    print("3 - Usuń książkę po ID")
    print("4 - Wyszukaj książkę po tytule")
    print("5 - Wyszukaj książkę po ID")
    print("6 - Pokaż najgrubsze książki")
    print("7 - Policz łączną liczbę stron")
    print("8 - Filtrowanie książek po liczbie stron")
    print("0 - Zakończ program")
    print("===========================")


def dodaj_ksiazke():
    print()
    print(" Dodawanie nowej książki ")
    tytul = input("Podaj tytuł książki: ")
    liczba_stron = wczytaj_int("Podaj liczbę stron: ")
    nowa_ksiazka = {
        "id": generuj_id(),
        "nazwa": tytul,
        "liczbaStron": liczba_stron
    }
    biblioteka.append(nowa_ksiazka)
    print("Książka została dodana do biblioteki.")


def wyswietl_ksiazki():
    print()
    print(" Lista książek ")
    if not biblioteka:
        print("Brak książek w bibliotece.")
        return
    numer = 1
    for ksiazka in biblioteka:
        print()
        print("Książka numer:", numer)
        print("ID:", ksiazka["id"])
        print("Tytuł:", ksiazka["nazwa"])
        print("Liczba stron:", ksiazka["liczbaStron"])
        print("------------------------")
        numer += 1


def usun_ksiazke():
    print()
    print(" Usuwanie książki ")
    identyfikator = input("Podaj ID książki: ")
    ksiazka = znajdz_po_id(identyfikator)
    if ksiazka:
        biblioteka.remove(ksiazka)
        print("Książka została usunięta.")
    else:
        print("Nie znaleziono książki o podanym ID.")


def szukaj_po_tytule():
    print()
    print(" Wyszukiwanie po tytule ")
    tytul = input("Podaj tytuł : ")
    wyniki = znajdz_po_tytule(tytul)
    if wyniki:
        print()
        print("Znaleziono", len(wyniki), "książek:")
        for ksiazka in wyniki:
            print("Tytuł:", ksiazka["nazwa"], "- ID:", ksiazka["id"])
    else:
        print("Nie znaleziono książek z podanym tytułem.")


def szukaj_po_id():
    print()
    print(" Wyszukiwanie po ID ")
    identyfikator = input("Podaj ID książki: ")
    ksiazka = znajdz_po_id(identyfikator)
    if ksiazka:
        print()
        print("Znaleziono książkę:")
        print("Tytuł:", ksiazka["nazwa"])
        print("Liczba stron:", ksiazka["liczbaStron"])
    else:
        print("Nie znaleziono książki o podanym ID.")


def pokaz_najgrubsze():
    print()
    print(" Najgrubsze książki ")
    if not biblioteka:
        print("Brak książek w bibliotece.")
        return
    najwiecej_stron = max(ksiazka["liczbaStron"] for ksiazka in biblioteka)
    for ksiazka in biblioteka:
        if ksiazka["liczbaStron"] == najwiecej_stron:
            print("Tytuł:", ksiazka["nazwa"], "- ID:", ksiazka["id"])


def policz_strony():
    print()
    print(" Obliczanie łącznej liczby stron ")
    suma_stron = 0
    for ksiazka in biblioteka:
        suma_stron += ksiazka["liczbaStron"]
    print("Łączna liczba stron we wszystkich książkach:", suma_stron)


def filtruj_po_stronach():
    print()
    print(" Filtrowanie książek ")
    minimalna_liczba_stron = wczytaj_int("Pokaż książki z większą liczbą stron niż: ")
    znalezione = []
    for ksiazka in biblioteka:
        if ksiazka["liczbaStron"] > minimalna_liczba_stron:
            znalezione.append(ksiazka)
    if znalezione:
        print()
        print("Książki z więcej niż", minimalna_liczba_stron, "stronami:")
        for ksiazka in znalezione:
            print("Tytuł:", ksiazka["nazwa"], "-", ksiazka["liczbaStron"], "stron")
    else:
        print("Nie znaleziono książek spełniających ten warunek.")


def start():
    while True:
        pokaz_menu()
        wybor = input("Wybierz numer opcji: ")
        if wybor == "1":
            dodaj_ksiazke()
        elif wybor == "2":
            wyswietl_ksiazki()
        elif wybor == "3":
            usun_ksiazke()
        elif wybor == "4":
            szukaj_po_tytule()
        elif wybor == "5":
            szukaj_po_id()
        elif wybor == "6":
            pokaz_najgrubsze()
        elif wybor == "7":
            policz_strony()
        elif wybor == "8":
            filtruj_po_stronach()
        elif wybor == "0":
            print()
            print("Zamykanie programu. Do zobaczenia!")
            break
        else:
            print("Niepoprawny wybór. Spróbuj ponownie.")


start()
