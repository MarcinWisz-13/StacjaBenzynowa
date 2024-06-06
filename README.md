
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
- dostep

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

- Jako pracownik, chcę móc widzieć, czy dany dystrybutor jest aktywny oraz czy nie wystąpiła awaria

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

![image](https://github.com/MarcinWisz-13/StacjaBenzynowa/assets/33297453/5bd828e1-7bbc-46bd-b322-9a27d5b5e6a2)


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

### Nazwa tabeli: distStations

<br>

- Opis: tabela odpowiedzialna za spis wszystkich dystrybutorów

<br>

| Nazwa atrybutu | Typ | Długość | Opis/Uwagi |
| -------------- | --- | ------- | ---------- |
| idStationNumber | int | | PK - id dystrybutora |
| activeGun | int | | numer aktualnie uzywanego pistoletu |
| transactionFinished | bit | | wartosc pokazująca czy transakcja zostala zakonczona pomyslnie |
| amountOfFuel | float | | ilosc zatankowanego paliwa |
| idActualPrice | int | | aktualna cena z tabeli fuelPriceHistory |
  
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
| idFueling | integer | | PK - id tankowania |
| idPumpGun | integer | | id pistoletu z którego tankowano |
| pricePerLiter | float | | Aktualna cena za którą tankowano|
| amountOfFuel| float | | Ile paliwa zatankowano w litrach |
| date | datetime | | data tankowania |

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

### Nazwa tabeli: losesHistory

<br>

- Opis: tabela odpowiedzialna za spis niezapłaconych tankowań

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
| idLose | int | | PK - id straty |
| idFueling | int | | id rekordu tankowania |
| carID | varchar | 20 | rejestracja samochodu który odjechał bez płacenia |

<br>

### Nazwa tabeli: orderDetails

<br>

- Opis: tabela odpowiedzialna za składowanie pozycji zamówień (ile, jakiego paliwa oraz w jakiej cenie)

<br>

| Nazwa atrybutu | Typ  |Długość | Opis/Uwagi |
|----------------|------|--------|------------|
| id | integer | | PK - klucz sztuczny, numer pojedynczej pozycji na zamówieniu |
| idOrder | integer | | Numer zamówienia |
| fuelCode | varchar| 6| Kod paliwa |
| amountFuel | float  | | Ilość paliwa w litrach |
| pricePerLiter | float | | Cena za litr paliwa |

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

<br>

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

<br>

tabela "contractors" <br>
```sql
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

<br>

tabela "distStation"
```sql
CREATE TABLE [dbo].[distStation](
	[idStationNumber] [int] NOT NULL,
	[activeGun] [int] NOT NULL,
	[transactionFinished] [int] NOT NULL,
	[amountOfFuel] [float] NOT NULL,
	[idActualPrice] [int] NOT NULL,
 CONSTRAINT [PK_distStation] PRIMARY KEY CLUSTERED 
(
	[idStationNumber] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

<br>

tabela "docNumeration"
```sql
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

<br>

tabela "employees"
```sql
CREATE TABLE [dbo].[employees](
	[idEmployee] [int] NOT NULL,
	[firstName] [varchar](30) NOT NULL,
	[surname] [varchar](50) NOT NULL,
	[positionTitle] [varchar](30) NOT NULL,
 CONSTRAINT [PK_employees] PRIMARY KEY CLUSTERED 
(
	[idEmployee] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

<br>

tabela "fuelingHistory"
```sql
CREATE TABLE [dbo].[fuelingHistory](
	[idFueling] [int] IDENTITY(1,1) NOT NULL,
	[idPumpGun] [int] NOT NULL,
	[pricePerLiter] [float] NOT NULL,
	[amountOfFuel] [float] NOT NULL,
	[date] [datetime] NOT NULL,
 CONSTRAINT [PK_fuelingHistory] PRIMARY KEY CLUSTERED 
(
	[idFueling] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

<br>

tabela "fuelPriceHistory"
```sql
CREATE TABLE [dbo].[fuelPriceHistory](
	[id] [int] IDENTITY(1,1) NOT NULL,
	[date] [datetime] NOT NULL,
	[fuelCode] [varchar](6) NOT NULL,
	[price] [float] NOT NULL,
 CONSTRAINT [PK_fuelPriceHistory_1] PRIMARY KEY CLUSTERED 
(
	[id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

<br>

tabela "fuelStorage"
```sql
CREATE TABLE [dbo].[fuelStorage](
	[fuelCode] [varchar](6) NOT NULL,
	[fuelName] [varchar](30) NOT NULL,
	[tankNumber] [int] NOT NULL,
	[fuelCurrLevel] [float] NOT NULL,
	[fuelMaxLevel] [float] NOT NULL,
 CONSTRAINT [PK_fuelStorage] PRIMARY KEY CLUSTERED 
(
	[fuelCode] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

<br>

tabela "losesHistory"
```sql
CREATE TABLE [dbo].[losesHistory](
	[idLose] [int] NOT NULL,
	[idFueling] [int] NOT NULL,
	[carID] [varchar](20) NULL,
 CONSTRAINT [PK_losesHistory] PRIMARY KEY CLUSTERED 
(
	[idLose] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

<br>

tabela "orderDetails"
```sql
CREATE TABLE [dbo].[orderDetails](
	[id] [int] NOT NULL,
	[idOrder] [int] NOT NULL,
	[fuelCode] [varchar](6) NOT NULL,
	[amountFuel] [float] NOT NULL,
	[pricePerLiter] [float] NOT NULL,
 CONSTRAINT [PK_orderDetails] PRIMARY KEY CLUSTERED 
(
	[id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

<br>

tabela "orders"
```sql
CREATE TABLE [dbo].[orders](
	[idOrder] [int] NOT NULL,
	[idOrderDoc] [int] NOT NULL,
	[contractorNIP] [varchar](15) NULL,
	[orderDate] [datetime] NULL,
	[deliveryDate] [datetime] NULL,
 CONSTRAINT [PK_orders] PRIMARY KEY CLUSTERED 
(
	[idOrder] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

<br>

tabela "pumpGuns"
```sql
CREATE TABLE [dbo].[pumpGuns](
	[idPumpGun] [int] NOT NULL,
	[idStationNumber] [int] NOT NULL,
	[fuelCode] [varchar](6) NOT NULL,
 CONSTRAINT [PK_pumpGuns] PRIMARY KEY CLUSTERED 
(
	[idPumpGun] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

<br>

tabela "transactionDocuments"
```sql
CREATE TABLE [dbo].[transactionDocuments](
	[id] [int] NOT NULL,
	[docType] [int] NOT NULL,
	[docTitleNumber] [int] NOT NULL,
	[date] [datetime] NOT NULL,
	[idContractorNIP] [varchar](15) NULL,
	[taxPTU] [int] NULL,
	[paymentMethod] [int] NOT NULL,
	[idFueling] [int] NOT NULL,
	[idDocNumeration] [int] NOT NULL,
	[idEmployee] [int] NOT NULL,
 CONSTRAINT [PK_transactionDocuments] PRIMARY KEY CLUSTERED 
(
	[id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

<br>

## Widoki

<br>

Widok: v_actualPrice - pokazuję najnowszą cenę paliw wraz z datą aktualizacji 

```sql
CREATE VIEW [dbo].[v_actualPrice]
AS
SELECT        date, fuelCode, price, id
FROM            dbo.fuelPriceHistory AS fph
WHERE        (date =
                    (SELECT MAX(date) AS Expr1
                    FROM dbo.fuelPriceHistory
                    WHERE (fuelCode = fph.fuelCode)))
```
<br>

Widok: v_contractorsOrders - widok pokazuję szczegółowe dane zamówień

 ```sql
CREATE VIEW [dbo].[v_contractorsOrders] AS
SELECT 
    o.idOrder,
    o.idOrderDoc,
    o.orderDate,
    o.deliveryDate,
    c.idContractorNIP,
    c.contractorName,
    c.address,
    c.address2,
    c.postalCode,
    c.contactNumber,
    c.email,
    c.contractorType
FROM 
    orders o
JOIN 
    contractors c ON o.contractorNIP = c.idContractorNIP;
```
<br>

Widok: v_employeePerformance - pokazuję wydajność sprzedażową danego pracownika

 ```sql
CREATE VIEW [dbo].[v_employeePerformance] AS
SELECT 
    e.idEmployee,
    e.firstName,
    e.surname,
    e.positionTitle,
    COUNT(td.id) AS transactionsCount,
    SUM(fh.amountOfFuel) AS totalFuelSold,
    SUM(fh.pricePerLiter * fh.amountOfFuel) AS totalSalesAmount
FROM employees e
JOIN transactionDocuments td ON e.idEmployee = td.idEmployee
JOIN fuelingHistory fh ON td.idFueling = fh.idFueling
GROUP BY e.idEmployee, e.firstName, e.surname, e.positionTitle;
```
<br>

Widok: v_fuelingDetails - pokazuje aktalną sprzedaż każdego paliwa (które najszybciej się sprzedaję)

```sql
CREATE VIEW [dbo].[v_fuelingDetails]
AS
SELECT
	fh.idFueling,
	fh.idPumpGun,
	fh.pricePerLiter,
	fh.amountOfFuel,
	fh.date,
	pg.fuelCode,
	pg.idStationNumber
FROM dbo.fuelingHistory fh
INNER JOIN dbo.pumpGuns pg ON fh.idPumpGun = pg.idPumpGun
```

<br>

Widok: v_FuelTypeSummary - pokazuje aktualną sprzedaż w podziale na dane paliwo

```sql
CREATE VIEW [dbo].[v_FuelTypeSummary] AS
SELECT 
    pg.fuelCode,
    fs.fuelName,
    SUM(fh.amountOfFuel) AS totalAmountOfFuel
FROM fuelingHistory fh
JOIN pumpGuns pg ON fh.idPumpGun = pg.idPumpGun
JOIN fuelStorage fs ON pg.fuelCode = fs.fuelCode
GROUP BY pg.fuelCode, fs.fuelName;
```

<br>

Widok: v_invoiceSummary - widok pokazujący wszystkie faktury wraz z wartością

```sql
CREATE   VIEW [dbo].[v_invoiceSummary]
AS
SELECT
dn.prefix + '/' + CAST(td.docTitleNumber as varchar)+  '/' + RIGHT(CONVERT(varchar,td.date , 103),7)  +
	( CASE WHEN sufix LIKE '' THEN '' ELSE '/'+sufix END )  as docTitle,
c.idContractorNIP as NIP,
c.contractorName ,
pg.fuelCode,
fh.amountOfFuel , 
FORMAT(ROUND(fh.pricePerLiter * (1 - (CAST(td.taxPTU as FLOAT) / 100)) ,2) , 'N2') as nettoPerLiter,
FORMAT(ROUND(fh.pricePerLiter , 2), 'N2') as bruttoPerLiter,
FORMAT(ROUND( fh.pricePerLiter * fh.amountOfFuel * (1 - (CAST(td.taxPTU as FLOAT) / 100)) , 2), 'N2')  as Netto, 
FORMAT(ROUND((fh.pricePerLiter * fh.amountOfFuel) - (fh.pricePerLiter * fh.amountOfFuel * (1 - (CAST(td.taxPTU as FLOAT) / 100))),2),'N2') AS TotalTax,
FORMAT(ROUND(fh.pricePerLiter * fh.amountOfFuel , 2 ), 'N2') as Brutto
FROM transactionDocuments as td
INNER JOIN fuelingHistory as fh ON td.idFueling = fh.idFueling
INNER JOIN pumpGuns as pg ON fh.idPumpGun = pg.idPumpGun
INNER JOIN docNumeration as dn ON td.idDocNumeration = dn.id
INNER JOIN contractors as c ON td.idContractorNIP= c.idContractorNIP
```

<br>

Widok: v_orderSummaryView - pokazuje podsumowanie zamówień

```sql
CREATE VIEW [dbo].[v_orderSummaryView]
AS
SELECT 
    o.idOrder,
    o.idOrderDoc,
    o.contractorNIP,
    o.orderDate,
    o.deliveryDate,
    SUM(od.amountFuel * od.pricePerLiter) AS TotalPrice
FROM dbo.orders o
JOIN dbo.orderDetails od ON o.idOrder = od.idOrder
GROUP BY 
    o.idOrder,
    o.idOrderDoc,
    o.contractorNIP,
    o.orderDate,
    o.deliveryDate;
```

<br>

Widok: v_receiptSummary - pokazuje zestawienie wszystkich paragonów

```sql
CREATE VIEW [dbo].[v_receiptSummary]
AS

SELECT
dn.prefix + '/' + CAST(td.docTitleNumber as varchar)+  '/' + RIGHT(CONVERT(varchar,td.date , 103),7)  +
	( CASE WHEN sufix LIKE '' THEN '' ELSE '/'+sufix END )  as docTitle,
pg.fuelCode ,
fh.amountOfFuel , 
FORMAT(ROUND(fh.pricePerLiter * (1 - (CAST(td.taxPTU as FLOAT) / 100)) ,2) , 'N2') as nettoPerLiter,
FORMAT(ROUND(fh.pricePerLiter , 2), 'N2') as bruttoPerLiter,
FORMAT(ROUND(fh.pricePerLiter * fh.amountOfFuel * (1 - (CAST(td.taxPTU as FLOAT) / 100)) , 2), 'N2')  as Netto, 
FORMAT(ROUND((fh.pricePerLiter * fh.amountOfFuel) - (fh.pricePerLiter * fh.amountOfFuel * (1 - (CAST(td.taxPTU as FLOAT) / 100))),2),'N2') AS TotalTax,
FORMAT(ROUND(fh.pricePerLiter * fh.amountOfFuel , 2 ), 'N2') as Brutto
FROM transactionDocuments as td
INNER JOIN fuelingHistory as fh ON td.idFueling = fh.idFueling
INNER JOIN pumpGuns as pg ON fh.idPumpGun = pg.idPumpGun
INNER JOIN docNumeration as dn ON td.idDocNumeration = dn.id
WHERE td.docType = 1
```
<br><br><br>

## Procedury/funkcje

<br>

Procedura: p_addContractor - dodaje kontrachenta z weryfikacją podanych danych

```sql

```

<br>

Procedura: p_addEmptyOrder - dodaj zamówienie - tworzy "koszyk"

```sql

```

<br>


Procedura: p_addFuelToOrder - dodaj paliwo do utworzonego wcześniej zamówienia - "koszyka"

```sql

```

<br>

Procedura: p_collectOrder - odbierz zamówienie - potwierdzamy dostawe paliw - aktualizuje stany magazynowe

```sql

```

<br>


Procedura: p_confirmOrder - potwierdzamy wysłanie zamówienia - czyli zamykamy koszyk ale bez potwierdzenia odbioru

```sql

```

<br>


Procedura: p_endFueling - koniec tankowania - wyzwalane odłożeniem pistoletu przy dystrybutorze na swoje miejscie

```sql

```

<br>


Procedura: p_fuelLevelUpdate - procedura obsługujaca ruch magazynowy w magazynie paliw

```sql

```

<br>


Procedura: p_fuelPriceUpdate - aktualizacja ceny dla danego paliwa

```sql

```

<br>


Procedura: p_genLose - generowanie dokumentu strat - powiązane z konkretnym tankowaniem z fuelingHistory

```sql

```

<br>


Procedura: p_genTransactionDoc - generowanie dokumentu sprzedażowego

```sql

```

<br>


Procedura: p_startFueling - rozpoczęcie tankowania - aktywowane podniesiem pistoletu przy dystrybutorze

```sql

```

<br><br><br>






## Triggery


  

(dla każdego triggera należy wkleić kod polecenia definiującego trigger wraz z komentarzem)


## Constrains

fuelPriceHistory - price > 0

distStation - amountOfFuel >= 0
distStation - activeGun <= 16

fuelStorage - fuelCurrLevel < fuelMaxLevel
fuelStorage - fuelCurrLevel > 0

orderDetails - amountOfFuel > 0
oderdDetails - pricePerLiter >= 0
