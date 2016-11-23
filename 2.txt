#! /usr/bin/env python
# -*- coding: utf-8 -*-

# importujemy funkcje z modu³u ocenyfun zapisanego w pliku ocenyfun.py
from ocenyfun import drukuj
from ocenyfun import srednia
from ocenyfun import mediana
from ocenyfun import odchylenie

przedmioty = set(['polski', 'angielski'])  # definicja zbioru
drukuj(przedmioty, "Lista przedmiotów zawiera: ")

print "\nAby przerwaæ wprowadzanie przedmiotów, naciœnij Enter."
while True:
    przedmiot = raw_input("Podaj nazwê przedmiotu: ")
    if len(przedmiot):
        if przedmiot in przedmioty:  # czy przedmiot jest w zbiorze?
            print "Ten przedmiot ju¿ mamy :-)"
        przedmioty.add(przedmiot)  # dodaj przedmiot do zbioru
    else:
        drukuj(przedmioty, "\nTwoje przedmioty: ")
        przedmiot = raw_input("\nZ którego przedmiotu wprowadzisz oceny? ")
        if przedmiot not in przedmioty:  # je¿eli przedmiotu nie ma w zbiorze
            print "Brak takiego przedmiotu, mo¿esz go dodaæ."
        else:
            break  # wyjœcie z pêtli

oceny = []  # pusta lista ocen
ocena = None  # zmienna steruj¹ca pêtl¹ i do pobierania ocen
print "\nAby przerwaæ wprowadzanie ocen, podaj 0 (zero)."

while not ocena:
    try:
        ocena = int(raw_input("Podaj ocenê (1-6): "))
        if (ocena > 0 and ocena < 7):
            oceny.append(float(ocena))
        elif ocena == 0:
            break
        else:
            print "B³êdna ocena."
        ocena = None
    except ValueError:
        print "B³êdne dane!"

drukuj(oceny, przedmiot.capitalize() + " - wprowadzone oceny: ")
s = srednia(oceny)  # wywo³anie funkcji z modu³u ocenyfun
m = mediana(oceny)  # wywo³anie funkcji z modu³u ocenyfun
o = odchylenie(oceny, s)  # wywo³anie funkcji z modu³u ocenyfun
print "\nŒrednia: {0:5.2f}\nMediana: {1:5.2f}\nOdchylenie: {2:5.2f}".format(s, m, o)