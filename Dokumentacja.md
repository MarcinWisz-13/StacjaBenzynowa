

  
  

<!-- <style>

p,li {

font-size: 12pt;

}

</style> -->

  

<!-- <style>

pre {

font-size: 8pt;

}

</style> -->

  
  

---

  <br>
  

**Temat:** System Stacji Benzynowej “Zielona Krówka”

  <br>

**Autorzy:** 
- Emilia Bania
- Janusz Szczepanik
- Marcin Wisz
- Artur Wlezień
- Jan Żbik



<br>

---

<br>

# 1. Zakres i krótki opis systemu

  <br>

System obsługi stacji paliw, obejmujący zarządzanie magazynem paliw oraz systemem sprzedażowym. Pozwala na sprzedaż, magazynowanie, przyjmowanie, raportowanie o aktualnych stanach, zamówieniach, pracownikach.

  <br>
  

# 2. Wymagania i funkcje systemu

  <br>

Jako [persona] [chcę], [aby]

- Jako klient chcę móc kupić dowolne paliwo w dowolnej ilości którą zmieszczę do baku/kanistra

- Jako klient chce móc zatankować z dowolnego, aktualnie wolnego dystrybutora, który jest aktywny

- Jako klient chcę zrobić zakup paliw na paragon

- Jako klient firmowy chcę zrobić zakup na fakturę VAT

- Jako pracownik chce móc wystawiać paragony/faktury bez względu jakie paliwa się na nim znajdą

-  Jako pracownik, chcę móc widzieć, czy dany dystrybutor jest aktywny oraz czy nie wystąpiła awaria

- Jako pracownik chce móc zobaczyć aktualną ilość wskazywaną poprzez licznik dystrybutora

- Jako pracownik chce móc aktywować / zablokować dany dystrybutor

- Jako pracownik chcę móc odblokować dystrybutor w przypadku “ucieczki” klienta bez płacenia

- Jako pracownik, chcę móc podejrzeć aktualne stany magazynowe paliw

- Jako właściciel/pracownik chce móc wpisać ilość dostarczonego paliwa i od którego dostawcy ono pochodzi

- Jako właściciel chce mieć możliwość podglądu  ile paliwa zostało dostarczone przez każdego dostawce  w dowolnym okresie

- Jako właściciel/pracownik chce mieć możliwość obejrzenia historii zamówień

- Jako właściciel/pracownik chce mieć możliwość obejrzenia historii transakcji

- Jako właściciel chcę móc sprawdzić dane pracowników

- Jako właściciel/pracownik chce mieć możliwość zobaczenia który pracownika finalizował daną transakcję

Funkcjonalność:

-   Resetowanie stanu aktualnego na dystrybutorze po wykonaniu transakcji
    
-   W momencie transakcji automatyczna aktualizacja stanów magazynowych
    
-   Automatyczne dodanie ilości paliwa po odbiorze dostawy


  

# 3. Projekt bazy danych

  
<br>

## Schemat bazy danych

![obraz](https://github.com/MarcinWisz-13/StacjaBenzynowa/assets/131010262/c26c1818-188d-4c0a-876c-946904f1434d)

<br>
  



<br>

## Opis poszczególnych tabel

<br>

### Nazwa tabeli: avalFuelInStation

<br>

- Opis: tabela odpowiedzialna za dostępność paliw w dystrybutorach 

<br>

| Nazwa atrybutu | Typ | Długość | Opis/Uwagi |
| -------------- | --- | ------- | ---------- |
| fuelStation | integer |  | PK - numer dystrybutora  |
| fuelCode | varchar | 6 | PK - kod paliwa |

<br>

### Nazwa tabeli: contractors

<br>

- Opis: tabela odpowiedzialna za zgromadzenie informacji dostawców i klientów firmowych

<br>

| Nazwa atrybutu | Typ | Długość | Opis/Uwagi |
| -------------- | --- | ------- | ---------- |
| idContractorNIP | varchar | 15 | PK - id/NIP firmy  |
| contractorName | varchar | 60 | nazwa firmy |
| address | varchar | 60 | adres firmy |
| address2 | varchar | 60 | dalszy adres firmy |
| postalCode | varchar | 10 | kod pocztowy |
| contactNumber | varchar | 15 | numer telefonu |
| email |varchar | 40 | email  |
| contractorType | int |  | czy dostawca czy odbiorca |

<br>

### Nazwa tabeli: employees
<br>

- Opis: tabela odpowiedzialna za zgromadzenie informacji o pracownikach

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
| idEmployee | integer | | PK - id pracownika |
| firstName | varchar | 30 | Imie pracownika |
| surname | varchar | 50 | Nazwisko pracownika |
| idPosition | integer | | id stanowiska funkcyjnego pracownika|

<br>

### Nazwa tabeli: fuelingDocuments

<br>

- Opis: tabela odpowiedzialna za przypisywanie dokumentu potwierdzającego płatność do każdej transakcji

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
| idFueling | integer | | PK - id dokumentu tankowania |
| isInvoice | bit | | pole decydujące, czy pozycja przynależy do faktury czy paragonu ( 1 - faktura, 0 - paragon )
| idDoc | integer | | Odwołanie do id dokumentu handlowego który zawiera daną pozycję dokumentu tankowania |

<br>

### Nazwa tabeli: fuelingHistory

<br>

- Opis: tabela odpowiedzialna za składowanie historii tankowań

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
|idFueling | integer | | PK - id tankowania |
| stationNumber | integer| | Dystrybutor z którego tankowano |
| fuelCode | varchar | 6 | Kod paliwa które zatankowano |
|pricePerLiter | float | | Aktualna cena za którą tankowano|
|amountOfFuel| float | | Ile paliwa zatankowano w litrach |
| isDisable | bit | | ???Czy zablokowany??? |

<br>

### Nazwa tabeli: fuelNames

<br>

- Opis: tabela odpowiedzialna za składowanie nazw paliw

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
| fuelCode | varchar | 6 | PK - kod paliwa |
| fuelName | varchar | 30 | nazwa paliwa |

<br>

### Nazwa tabeli: fuelPriceHistory

<br>

- Opis: tabela odpowiedzialna za składowanie historii cen paliw

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
| id | integer | | PK - klucz sztuczny, historia cen tankowania |
| date | datetime | | Data zmiany ceny |
| fuelCode | varchar | 6 | Kod paliwa |
| actualPrice | float | | Cena za litr paliwa |

<br>

### Nazwa tabeli: fuelStorage

<br>

- Opis: tabela odpowiedzialna za składowanie nazw paliw

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
| fuelCode | varchar | 6 | PK - kod paliwa |
| tankNumber | integer | | numer zbiornika w którym znajduje się paliwo |
| fuelCurrLevel | float | | aktualny poziom paliwa w zbiorniku |
| fuelMaxLevel | float | | maksymalna pojemność zbiornika |

<br>

### Nazwa tabeli: invoices

<br>

- Opis: tabela odpowiedzialna za składowanie wystawionych faktur

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
| idInvoice | integer | | PK - id faktury ( nie jej numer )|
| invoiceNumber | varchar | 20 | Numer faktury w rozliczeniu rocznym |
| date | datetime | | Data wystawienia dokumentu |
| idCustomer | varchar | 15 | Numer nabywcy/odbiorcy (NIP)|
|totalPriceNetto| float | | Wartość faktury netto |
| totalTax | float | | Podatek VAT |
| totalPriceBrutto | float | | Wartość faktury brutto |
|idEmployee | integer | | Kod pracownika wystawiającego dokument | 

<br>

### Nazwa tabeli: orderDetails

<br>

- Opis: tabela odpowiedzialna za składowanie pozycji zamówień (ile, jakiego paliwa oraz w jakiej cenie)

<br>

| Nazwa atrybutu | Typ  |Długość | Opis/Uwagi |
|----------------|------|--------|------------|
| id   		 | integer    |		| PK - klucz sztuczny, numer pojedynczej pozycji na zamówieniu            |
| idOrder    | integer    |	 | Numer zamówienia           |
| fuelCode   | varchar| 6| Kod paliwa           |
| amountFuel |  float    || Ilość paliwa w litrach           |
| pricePerLiter | float    || Cena za litr paliwa         |

<br>

### Nazwa tabeli: orders

<br>

- Opis: tabela odpowiedzialna za składowanie zamówień (od kogo, kiedy zamówione, kiedy przyszło)

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| - | - | - | - |
| idOrder | integer | | PK - id zamównienia |
| contractorNIP | varchar | 15 | NIP dostawcy |
| orderDate | datetime | | data zamówienia |
| deliveryDate | datetime | | data otrzymania dostawy |

<br>

### Nazwa tabeli: positionNames

<br>

- Opis: tabela odpowiedzialna za składowanie pozycji pracowników

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| - | - | - | - |
| idPosition | integer | | PK - id stanowiska funkcyjnego pracownika |
| title | varchar | 30 | tytuł pracownika |

<br>

### Nazwa tabeli: receipts

<br>

- Opis: tabela odpowiedzialna za składowanie wystawionych paragonów

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| - | - | - | - |
| idReceipt | integer | | PK - id paragonu |
| date | datetime | | data wystawienia paragonu
| totalTax | float | | wartość PTU |
| totalPriceBrutto | float | | wartość brutto
| idEmployee | integer | | id pracownika |








  

# 4. Implementacja

  

## Kod poleceń DDL

  

(dla każdej tabeli należy wkleić kod DDL polecenia tworzącego tabelę)

  

```sql

create  table  tab1 (

a int,

b varchar(10)

)

```

  

## Widoki

  

(dla każdego widoku należy wkleić kod polecenia definiującego widok wraz z komentarzem)

  

## Procedury/funkcje

  

(dla każdej procedury/funkcji należy wkleić kod polecenia definiującego procedurę wraz z komentarzem)

  

## Triggery

  

(dla każdego triggera należy wkleić kod polecenia definiującego trigger wraz z komentarzem)
