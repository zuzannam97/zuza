#! /usr/bin/env python
# -*- coding: utf-8 -*-

import os.path  # modu³ udostêpniaj¹cy funkcjê isfile()

print """Podaj dane w formacie:
wyraz obcy: znaczenie1, znaczenie2
Aby zakoñczyæ wprowadzanie danych, podaj 0.
"""

sFile = "slownik.txt"  # nazwa pliku zawieraj¹cego wyrazy i ich t³umaczenia
slownik = {}  # pusty s³ownik
# wobce = set() # pusty zbiór wyrazów obcych


def otworz(plik):
    if os.path.isfile(sFile):  # czy istnieje plik s³ownika?
        with open(sFile, "r") as sTxt:  # otwórz plik do odczytu
            for line in sTxt:  # przegl¹damy kolejne linie
                # rozbijamy liniê na wyraz obcy i t³umaczenia
                t = line.split(":")
                wobcy = t[0]
                # usuwamy znaki nowych linii
                znaczenia = t[1].replace("\n", "")
                znaczenia = znaczenia.split(",")  # tworzymy listê znaczeñ
                # dodajemy do s³ownika wyrazy obce i ich znaczenia
                slownik[wobcy] = znaczenia
    return len(slownik)  # zwracamy iloœæ elementów w s³owniku


def zapisz(slownik):
    # otwieramy plik do zapisu, istniej¹cy plik zostanie nadpisany(!)
    file1 = open(sFile, "w")
    for wobcy in slownik:
        # "sklejamy" znaczenia przecinkami w jeden napis
        znaczenia = ",".join(slownik[wobcy])
        # wyraz_obcy:znaczenie1,znaczenie2,...
        linia = ":".join([wobcy, znaczenia])
        print >>file1, linia  # zapisujemy w pliku kolejne linie
    file1.close()  # zamykamy plik


def oczysc(str):
    str = str.strip()  # usuñ pocz¹tkowe lub koñcowe bia³e znaki
    str = str.lower()  # zmieñ na ma³e litery
    return str

# zmienna oznaczaj¹ca, ¿e u¿ytkownik uzupe³ni³ lub zmieni³ s³ownik
nowy = False
ileWyrazow = otworz(sFile)
print "Wpisów w bazie:", ileWyrazow

# g³ówna pêtla programu
while True:
    dane = raw_input("Podaj dane: ")
    t = dane.split(":")
    wobcy = t[0].strip().lower()  # robimy to samo, co funkcja oczysc()
    if wobcy == 'koniec':
        break
    elif dane.count(":") == 1:  # sprawdzamy poprawnoœæ wprowadzonych danych
        if wobcy in slownik:
            print "Wyraz", wobcy, " i jego znaczenia s¹ ju¿ w s³owniku."
            op = raw_input("Zast¹piæ wpis (t/n)? ")
        # czy wyrazu nie ma w s³owniku? a mo¿e chcemy go zast¹piæ?
        if wobcy not in slownik or op == "t":
            znaczenia = t[1].split(",")  # podane znaczenia zapisujemy w liœcie
            znaczenia = map(oczysc, znaczenia)  # oczyszczamy elementy listy
            slownik[wobcy] = znaczenia
            nowy = True
    else:
        print "B³êdny format!"

if nowy:
    zapisz(slownik)

print "=" * 50
print "{0: <15}{1: <40}".format("Wyraz obcy", "Znaczenia")
print "=" * 50
for wobcy in slownik:
    print "{0: <15}{1: <40}".format(wobcy, ",".join(slownik[wobcy]))