# Cégek és Kapcsolatok (Company / Contact)

A MiniCRM-ben az adatlapok (opportunity cards) kapcsolatokhoz, a kapcsolatok pedig cégekhez vannak rendelve.

## Új cég / kapcsolat adatlap létrehozása
Ha API-n keresztül egy teljesen új kártyát szeretne létrehozni céggel és kapcsolattal együtt, 3 külön API hívásra van szükség:

1. **Cég létrehozása.**
2. **Kapcsolat (Contact) létrehozása** és hozzárendelése az előbb létrehozott céghez.
3. **Kártya (Opportunity card) létrehozása** és hozzárendelése a létrehozott kapcsolathoz.

## Keresés (Search)
Lehetőség van cégek és kapcsolatok keresésére különböző paraméterek alapján:
* Név alapján
* E-mail cím alapján
* Telefonszám alapján
* Frissítés dátuma alapján
* Egyedi mezők alapján
