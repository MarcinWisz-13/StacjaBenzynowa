
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
  
# <p align='center'> System Stacji Benzynowej </p>
## <p align='center'> “Zielona Krówka” </p>

  <br>

### **Autorzy:** 
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

### Wymagania:

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

 <br>

### Funkcjonalność:



-   Resetowanie stanu aktualnego na dystrybutorze po wykonaniu transakcji
    
-   W momencie transakcji automatyczna aktualizacja stanów magazynowych
    
-   Automatyczne dodanie ilości paliwa po odbiorze dostawy


  <br>
  <br>

# 3. Projekt bazy danych

  
<br>

## Schemat bazy danych

![obraz](C:\Users\Administrator\Desktop\schemat.jpg)

<br>
  



<br>

## Opis poszczególnych tabel

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

### Nazwa tabeli: docNumeration
<br>

- Opis: tabela typu słownikowego w której określamy styl szablon numeracji dokumentów poprzez prefix oraz sufix np: FS 01/2024/SPRZ gdzie FS jest prefixem a /SPRZ jest sufixem

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
| id | integer | | PK - id schematu |
| prefix | varchar | 6 | Prefix szablonu |
| sufix | varchar | 6 | Sufix szablonu |

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
| positionTitle | varchar | 30 | Nazwa stanowiska funkcyjnego pracownika|

<br>


### Nazwa tabeli: fuelingHistory

<br>

- Opis: tabela odpowiedzialna za składowanie historii tankowań

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
|idFueling | integer | | PK - id tankowania |
| idPumpGun | integer | | id pistoletu z którego tankowano |
|pricePerLiter | float | | Aktualna cena za którą tankowano|
|amountOfFuel| float | | Ile paliwa zatankowano w litrach |


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
| price | float | | Cena za litr paliwa |

<br>

### Nazwa tabeli: fuelStorage

<br>

- Opis: tabela odpowiedzialna za aktualne zasoby paliw

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
| fuelCode | varchar | 6 | PK - kod paliwa |
| fuelName | varchar | 30 | Nazwa paliwa |
| tankNumber | integer | | numer zbiornika w którym znajduje się paliwo |
| fuelCurrLevel | float | | aktualny poziom paliwa w zbiorniku |
| fuelMaxLevel | float | | maksymalna pojemność zbiornika |

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
| idOrderDoc | integer | | Numer dokumentu zamówienia |
| contractorNIP | varchar | 15 | NIP dostawcy |
| orderDate | datetime | | data zamówienia |
| deliveryDate | datetime | | data otrzymania dostawy |

<br>



### Nazwa tabeli: pumpGuns

<br>

- Opis: tabela składująca informację o konkretnych pistoletach do paliwa na danym dystrybutorze

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| - | - | - | - |
| idPumpGun | integer | | PK - id pistoletu |
| idStationNumber | integer | | Numer dystrybutora |
| fuelCode | varchar | 6 | dostępne paliwo dla danego pistoletu |
| inUse | bit | | czy pistolet w użyciu - podniesiony
| transactionFinished | bit | | czy transakcja dla danego tankowania zakończona 1 - tak |
| amountOfFuel | float | | Ilość zatankowanego paliwa |
| idActualPrice | int | | id aktualnej ceny paliwa pobranej z tabeli fuelPriceHistory |


### Nazwa tabeli: transactionDocuments

<br>

- Opis: tabela przechowująca wszystkie dokumenty transakcji

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| - | - | - | - |
| id | integer | | PK - id dokumentu |
| docType | integer | | Typ dokumentu 1 - paragon, 2 - faktura |
| docTitleNumber | integer |  | Kolejny numer w numeracji dla danego typu dokumentu |
| date | datetime | | Data wystawienia dokumentu |
| idContractorNIP | varchar | 15 | NIP kontrachenta |
| taxPTU | integer | | Stawka VAT wyrażona w % |
| idFueling | integer | | id tankowania | 
| idDocNumeration | integer | | id schematu numeracji dokumentu |
| idEmployee | integer | | id pracownika wystawiającego dokument |

<br>







  

# 4. Implementacja

  

## Kod poleceń DDL

  
tabela "contractors"
```
CREATE TABLE [dbo].[contractors](
	[idContractorNIP] [varchar](15) NOT NULL,
	[contractorName] [varchar](60) NOT NULL,
	[address] [varchar](60) NOT NULL,
	[address2] [varchar](60) NULL,
	[postalCode] [varchar](10) NOT NULL,
	[contactNumber] [varchar](15) NULL,
	[email] [varchar](40) NULL,
	[contractorType] [int] NULL,
 CONSTRAINT [PK_contractors] PRIMARY KEY CLUSTERED 
(
	[idContractorNIP] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

tabela "docNumeration"

```
CREATE TABLE [dbo].[docNumeration](
	[id] [int] NOT NULL,
	[prefix] [varchar](6) NULL,
	[sufix] [varchar](6) NULL,
 CONSTRAINT [PK_docNumeration] PRIMARY KEY CLUSTERED 
(
	[id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

tabela "employees"

```

```
  

## Widoki

  

(dla każdego widoku należy wkleić kod polecenia definiującego widok wraz z komentarzem)

  

## Procedury/funkcje

  

(dla każdej procedury/funkcji należy wkleić kod polecenia definiującego procedurę wraz z komentarzem)

  

## Triggery

  

(dla każdego triggera należy wkleić kod polecenia definiującego trigger wraz z komentarzem)
