
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

![image](https://github.com/MarcinWisz-13/StacjaBenzynowa/blob/main/aaa.png)


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

### Nazwa Tabeli: contractorsArchive

<br>

- Opis: Tabela historyczna odpowiedzialna za przechowywanie zmian na kontrachentach

  <br>

| Nazwa atrybutu | Typ | Długość | Opis/Uwagi |
| -------------- | --- | ------- | ---------- |
| idArchive | int | | id zmiany |
| idContractorNIP | varchar | 15 | PK - id/NIP firmy  |
| operationType | varchar | 6 | Typ operacji |
| operationDate | datetime |  | data wykonania operacji |
| NEW_contractorName | varchar | 60 | Nowa nazwa firmy |
| NEW_address | varchar | 60 | Nowy adres firmy |
| NEW_address2 | varchar | 60 | Nowy dalszy adres firmy |
| NEW_postalCode | varchar | 10 | Nowy kod pocztowy |
| NEW_contactNumber | varchar | 15 | Nowy numer telefonu |
| NEW_email |varchar | 40 | Nowy email |
| NEW_contractorType | int |  | Nowy czy dostawca czy odbiorca |
| OLD_contractorName | varchar | 60 | Starsza nazwa firmy |
| OLD_address | varchar | 60 | Starszy adres firmy |
| OLD_address2 | varchar | 60 | Starszy dalszy adres firmy |
| OLD_postalCode | varchar | 10 | Starszy kod pocztowy |
| OLD_contactNumber | varchar | 15 | Starszy numer telefonu |
| OLD_email |varchar | 40 | Starszy email  |
| OLD_contractorType | int |  | Starsza czy dostawca czy odbiorca |

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

### Nazwa tabeli: fuelStorageArchive

<br>

- Opis: tabela odpowiedzialna za zbieranie danych o zmianach w magazynie paliw

<br>

| Nazwa atrybutu | Typ |  Długość | Opis/Uwagi |
| -------------- | ---- | ---------- | - |
| idArchive | int | | PK - id zmiany |
| fuelCode | varchar | 6 | PK - kod paliwa |
| operationType | varchar | 20 | Typ operacji |
| operationDate | datetime |  | data wykonania operacji |
| NEW_fuelCurrLevel | float | | aktualny poziom paliwa w zbiorniku |
| OLD_fuelCurrLevel | float | | stary poziom paliwa w zbiorniku |
| NEW_tankNumber | integer | | numer zbiornika w którym znajduje się paliwo |
| OLD_tankNumber | integer | | stary numer zbiornika w którym znajduje się paliwo |
| NEW_fuelMaxLevel | float | | nowa maksymalna pojemność zbiornika |
| OLD_fuelMaxLevel | float | | stara maksymalna pojemność zbiornika |

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

### tabela "contractors" <br>
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

### tabela "contractorsArchive"
```sql
CREATE TABLE [dbo].[contractorsArchive](
	[idArchive] [int] IDENTITY(1,1) NOT NULL,
	[idContractorNIP] [varchar](15) NOT NULL,
	[operationType] [varchar](6) NOT NULL,
	[operationDate] [datetime] NOT NULL,
	[NEW_contractorName] [varchar](60) NULL,
	[NEW_address] [varchar](60) NULL,
	[NEW_address2] [varchar](60) NULL,
	[NEW_postalCode] [varchar](10) NULL,
	[NEW_contactNumber] [varchar](15) NULL,
	[NEW_email] [varchar](40) NULL,
	[NEW_contractorType] [int] NULL,
	[OLD_contractorName] [varchar](60) NOT NULL,
	[OLD_address] [varchar](60) NOT NULL,
	[OLD_address2] [varchar](60) NULL,
	[OLD_postalCode] [varchar](10) NOT NULL,
	[OLD_contactNumber] [varchar](15) NULL,
	[OLD_email] [varchar](40) NULL,
	[OLD_contractorType] [int] NULL,
 CONSTRAINT [PK_contractorsArchive] PRIMARY KEY CLUSTERED 
(
	[idArchive] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO

ALTER TABLE [dbo].[contractorsArchive] ADD  DEFAULT (getdate()) FOR [operationDate]
GO
```

<br>

### tabela "distStation"
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

### tabela "docNumeration"
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

### tabela "employees"
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

### tabela "fuelingHistory"
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

### tabela "fuelPriceHistory"
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

### tabela "fuelStorage"
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

### tabela "fuelStorageArchive"
```sql
CREATE TABLE [dbo].[fuelStorageArchive](
	[idArchive] [int] IDENTITY(1,1) NOT NULL,
	[fuelCode] [varchar](6) NOT NULL,
	[operationDate] [datetime] NOT NULL,
	[operationType] [varchar](20) NOT NULL,
	[NEW_fuelCurrLevel] [float] NULL,
	[OLD_fuelCurrLevel] [float] NULL,
	[NEW_tankNumber] [int] NULL,
	[OLD_tankNumber] [int] NULL,
	[NEW_fuelMaxLevel] [float] NULL,
	[OLD_fuelMaxLevel] [float] NULL,
 CONSTRAINT [PK_fuelStorageArchive] PRIMARY KEY CLUSTERED 
(
	[idArchive] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO

ALTER TABLE [dbo].[fuelStorageArchive] ADD  DEFAULT (getdate()) FOR [operationDate]
GO

ALTER TABLE [dbo].[fuelStorageArchive]  WITH CHECK ADD  CONSTRAINT [FK_fuelStorageArchive_fuelStorage] FOREIGN KEY([fuelCode])
REFERENCES [dbo].[fuelStorage] ([fuelCode])
GO

ALTER TABLE [dbo].[fuelStorageArchive] CHECK CONSTRAINT [FK_fuelStorageArchive_fuelStorage]
GO

```
<br>

### tabela "losesHistory"
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

### tabela "orderDetails"
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

### tabela "orders"
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

### tabela "pumpGuns"
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

### tabela "transactionDocuments"
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

### Widok: v_contractorsOrders - widok pokazuję szczegółowe dane zamówień

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

### Widok: v_employeePerformance - pokazuję wydajność sprzedażową danego pracownika

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

### Widok: v_fuelingDetails - pokazuje aktalną sprzedaż każdego paliwa (które najszybciej się sprzedaję)

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

### Widok: v_FuelTypeSummary - pokazuje aktualną sprzedaż w podziale na dane paliwo

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

### Widok: v_invoiceSummary - widok pokazujący wszystkie faktury wraz z wartością

```sql
CREATE VIEW [dbo].[v_invoiceSummary]
AS
WITH p as (SELECT idFueling, fh.pricePerLiter * fh.amountOfFuel as totalBrutto FROM fuelingHistory as fh)

SELECT
dn.prefix + '/' + CAST(td.docTitleNumber as varchar)+  '/' + RIGHT(CONVERT(varchar,td.date , 103),7)  +
	( CASE WHEN sufix LIKE '' THEN ''
		ELSE '/'+sufix END 
		 )  as docTitle,
c.idContractorNIP as NIP,
c.contractorName as contractorName,
pg.fuelCode,
fh.amountOfFuel , 
FORMAT(ROUND(fh.pricePerLiter * (1 - (CAST(td.taxPTU as FLOAT) / 100)) ,2) , 'N2') as nettoPerLiter,
FORMAT(ROUND(fh.pricePerLiter , 2), 'N2') as bruttoPerLiter,
FORMAT(ROUND( p.totalBrutto * (1 - (CAST(td.taxPTU as FLOAT) / 100)) , 2), 'N2')  as Netto, 
FORMAT(ROUND((p.totalBrutto) - (p.totalBrutto* (1 - (CAST(td.taxPTU as FLOAT) / 100))),2),'N2') AS TotalTax,
FORMAT(ROUND(p.totalBrutto , 2 ), 'N2') as Brutto,
CONVERT(varchar,td.date,104) as docDate
FROM transactionDocuments as td
INNER JOIN fuelingHistory as fh ON td.idFueling = fh.idFueling
INNER JOIN pumpGuns as pg ON fh.idPumpGun = pg.idPumpGun
INNER JOIN docNumeration as dn ON td.docType = dn.id
INNER JOIN contractors as c ON td.idContractorNIP= c.idContractorNIP
INNER JOIN p ON fh.idFueling = p.idFueling
WHERE td.docType = 2

GO
```

<br>

### Widok: v_orderSummaryView - pokazuje podsumowanie zamówień

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

### Widok: v_receiptSummary - pokazuje zestawienie wszystkich paragonów

```sql
CREATE VIEW [dbo].[v_receiptSummary]
AS
WITH p as (SELECT idFueling, fh.pricePerLiter * fh.amountOfFuel as totalBrutto FROM fuelingHistory as fh)
SELECT
dn.prefix + '/' + CAST(td.docTitleNumber as varchar)+  '/' + RIGHT(CONVERT(varchar,td.date , 103),7)  +
	( CASE WHEN sufix LIKE '' THEN ''
		ELSE '/'+sufix END 
		 )  as docTitle,
pg.fuelCode ,
fh.amountOfFuel , 
FORMAT(ROUND(fh.pricePerLiter , 2), 'N2') as bruttoPerLiter,
FORMAT(ROUND((p.totalBrutto) - (p.totalBrutto * (1 - (CAST(td.taxPTU as FLOAT) / 100))),2),'N2') AS TotalTax,
FORMAT(ROUND(p.totalBrutto , 2 ), 'N2') as Brutto,
CONVERT(varchar,td.date,104) as docDate
FROM transactionDocuments as td
INNER JOIN fuelingHistory as fh ON td.idFueling = fh.idFueling
INNER JOIN pumpGuns as pg ON fh.idPumpGun = pg.idPumpGun
INNER JOIN docNumeration as dn ON td.docType = dn.id
INNER JOIN p ON fh.idFueling = p.idFueling
WHERE td.docType = 1
GO
```
<br><br><br>

## Procedury/funkcje

<br>

### Procedura: p_addContractor - dodaje kontrachenta z weryfikacją podanych danych

```sql
CREATE   PROCEDURE [dbo].[p_addContractor]
	@ContractorNIP varchar(15),
	@ContractorName varchar(60),
	@Address varchar(60),
	@Address2 varchar(60) = NULL,
	@PostalCode varchar(10),
	@ContactNumber varchar(15) = NULL,
	@Email varchar(40) = NULL,
	@ContractorType int = NULL
AS
BEGIN
    -- Rozpoczęcie transakcji
    BEGIN TRANSACTION;

    BEGIN TRY

	DECLARE @IsValidNIP bit;
	DECLARE @IsValidNameAddress bit;
	DECLARE @IsValidPostalCode bit;
	DECLARE @IsValidContactNumber bit;
	DECLARE @IsValidEmail bit;


	----------isValidNIP
	
	 IF LEN(@ContractorNIP) = 10 AND ISNUMERIC(@ContractorNIP) = 1
	 BEGIN
		SET @IsValidNIP = 1;
	 END
	 ELSE
		BEGIN
			SET @IsValidNIP = 0;
		    PRINT 'NIP Niepoprawny';
            THROW 50001, 'NIP Niepoprawny', 1;
		END


	-----------isValidNameAddress

	 IF LEN(@Address) <= 60 AND LEN(@ContractorName) <= 60
	 BEGIN
		SET @IsValidNameAddress = 1;
	 END
	 ELSE
		BEGIN
			SET @IsValidNameAddress = 0;
		    PRINT 'Nazwa lub adres niepoprawne';
            THROW 50002, 'Nazwa lub adres niepoprawne', 1;
		END

	-----------isValidPostalCode

	 IF @PostalCode LIKE '[0-9][0-9]-[0-9][0-9][0-9]' AND LEN(@PostalCode) <= 10
	 BEGIN
		SET @IsValidPostalCode = 1;
	 END
	 else
		BEGIN
			SET @IsValidPostalCode = 0;
			PRINT 'Kod pocztowy niepoprawny';
			THROW 50003, 'Kod pocztowy niepoprawny', 1;
		END


	-----------isValidContactNumber

	IF ((LEN(@ContactNumber) BETWEEN 9 AND 15) AND ISNUMERIC(@ContactNumber) = 1) OR @ContactNumber is NULL
	BEGIN
		SET @IsValidContactNumber = 1;
	END
	else
		BEGIN
			SET @IsValidContactNumber = 0;
			PRINT 'Numer kontaktowy niepoprawny';
            THROW 50004, 'Numer kontaktowy niepoprawny', 1;
		END

	-----------isValidEmail

 IF @Email LIKE '%_@__%.__%' AND						--sprawdza pierwszą formę maila - " układ"
       CHARINDEX(' ', @Email) = 0 AND					--sprawdza czy email nie zawiera spacji
       CHARINDEX('..', @Email) = 0 AND					--sprawdza czy email nie zawiera podwójnej kropki
       LEFT(@Email, 1) NOT IN ('@', '.') AND			--sprawdza czy nie zaczyna sie lub konczy @ albo .
       RIGHT(@Email, 1) NOT IN ('@', '.') AND
       LEN(@Email) <= 254 AND
       LEN(@Email) - LEN(REPLACE(@Email, '@', '')) = 1
	BEGIN
		SET @IsValidEmail = 1;
	END
    ELSE
		BEGIN
			SET @IsValidEmail = 0;
			PRINT 'Email niepoprawny';
            THROW 50005, 'Email niepoprawny', 1;
		END


	--jeśli wszystko poprawne, insert do tabeli
	IF @IsValidContactNumber = 1 AND 
	@IsValidNameAddress = 1 AND 
	@IsValidEmail = 1 AND 
	@IsValidNIP = 1 AND 
	@IsValidPostalCode = 1
		BEGIN
			insert into contractors (idContractorNIP, contractorName, address, address2, postalCode, contactNumber, email, contractorType) values
			(@ContractorNIP, @ContractorName, @Address, @Address2, @PostalCode, @ContactNumber, @Email, @ContractorType);
		END
	else
	BEGIN
		PRINT 'ERROR NA KONCU';
		THROW 50010, 'ERROR NA KONCU',1;
	END

	

        -- Jeśli wszystkie operacje zakończą się sukcesem, zatwierdź transakcję
        COMMIT;
    END TRY
    BEGIN CATCH
        -- W przypadku błędu, cofnij transakcję
        ROLLBACK;

        -- Opcjonalnie, obsłuż błąd
        DECLARE @ErrorMessage NVARCHAR(4000);
        DECLARE @ErrorSeverity INT;
        DECLARE @ErrorState INT;

        SELECT 
            @ErrorMessage = ERROR_MESSAGE(),
            @ErrorSeverity = ERROR_SEVERITY(),
            @ErrorState = ERROR_STATE();

        -- Wyrzuć błąd ponownie
        RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;
```

<br>

### Procedura: p_addEmptyOrder - dodaj zamówienie - tworzy "koszyk"

```sql
CREATE   PROCEDURE [dbo].[p_addEmptyOrder]
	@ContractorNIP varchar(15)
AS
BEGIN
    -- Rozpoczęcie transakcji
    BEGIN TRANSACTION;

    BEGIN TRY
	IF EXISTS (SELECT 1 FROM contractors where idContractorNIP = @ContractorNIP)
	BEGIN
		insert into orders (contractorNIP) OUTPUT inserted.idOrder values (@ContractorNIP);
		
	END
	ELSE
		BEGIN
		PRINT 'Kontrachent nie istnieje, dodaj kontrachenta';
		THROW 50006, 'Kontrachent nie istnieje, dodaj kontrachenta', 1;
		END
	

        -- Jeśli wszystkie operacje zakończą się sukcesem, zatwierdź transakcję
        COMMIT;
    END TRY
    BEGIN CATCH
        -- W przypadku błędu, cofnij transakcję
        ROLLBACK;

        -- Opcjonalnie, obsłuż błąd
        DECLARE @ErrorMessage NVARCHAR(4000);
        DECLARE @ErrorSeverity INT;
        DECLARE @ErrorState INT;

        SELECT 
            @ErrorMessage = ERROR_MESSAGE(),
            @ErrorSeverity = ERROR_SEVERITY(),
            @ErrorState = ERROR_STATE();

        -- Wyrzuć błąd ponownie
        RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;
```

<br>


### Procedura: p_addFuelToOrder - dodaj paliwo do utworzonego wcześniej zamówienia - "koszyka"

```sql
CREATE    PROCEDURE [dbo].[p_addFuelToOrder]
	@IDOrder int, 
	@FuelCode varchar(6),
	@Amount float,
	@Price float
AS
BEGIN
    -- Rozpoczęcie transakcji
    BEGIN TRANSACTION;

    BEGIN TRY
	IF @IDOrder != 0 AND @FuelCode != '' AND @Amount != 0 AND @Price != 0
	BEGIN
		insert into orderDetails ( idOrder, fuelCode, amountFuel, pricePerLiter) values
								 (@IDOrder, @FuelCode, @Amount, @Price);
	END
	ELSE 
		BEGIN
		PRINT 'Niepoprawne dane';
        THROW 50001, 'Niepoprawne dane', 1;
		END
	

        -- Jeśli wszystkie operacje zakończą się sukcesem, zatwierdź transakcję
        COMMIT;
    END TRY
    BEGIN CATCH
        -- W przypadku błędu, cofnij transakcję
        ROLLBACK;

        -- Opcjonalnie, obsłuż błąd
        DECLARE @ErrorMessage NVARCHAR(4000);
        DECLARE @ErrorSeverity INT;
        DECLARE @ErrorState INT;

        SELECT 
            @ErrorMessage = ERROR_MESSAGE(),
            @ErrorSeverity = ERROR_SEVERITY(),
            @ErrorState = ERROR_STATE();

        -- Wyrzuć błąd ponownie
        RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;
```

<br>

### Procedura: p_collectOrder - odbierz zamówienie - potwierdzamy dostawe paliw - aktualizuje stany magazynowe

```sql
CREATE PROCEDURE [dbo].[p_collectOrder]
	@IDOrder int
AS
BEGIN
    -- Rozpoczęcie transakcji
    BEGIN TRANSACTION;

    BEGIN TRY
	DECLARE @tmp_IdOrderDetails int;
	DECLARE @tmp_AmountFuel float;
	DECLARE @tmp_FuelCode varchar(6);
	DECLARE @tmp_OrderDetailsTable TABLE (
    id int,
	amountFuel float,
	fuelCode varchar(6)
    );

	IF EXISTS (SELECT 1 FROM orders where idOrder = @IDOrder)
	BEGIN

	update orders					--ustawienie daty dostarczenia, potwierdzamy odbiór zamówienia
	set deliveryDate = GETDATE()
	where idOrder = @IDOrder;


	--cały proces tworzenia tabeli chwilowej oraz kursora który iteruje po tej tabeli z pozycjami order details wywołując funkcję p_fuelLevelUpdate
	insert into @tmp_OrderDetailsTable(id, amountFuel, fuelCode)
	select id, amountFuel, fuelCode
	from orderDetails
	where idOrder = @IDOrder;

	DECLARE orders_cursor CURSOR FOR
	select id, amountFuel, fuelCode
	from @tmp_OrderDetailsTable

	OPEN orders_cursor;

	FETCH NEXT FROM orders_cursor INTO 
	@tmp_IdOrderDetails,
    @tmp_AmountFuel,
	@tmp_FuelCode;

	WHILE @@FETCH_STATUS = 0
	BEGIN
		EXEC p_fuelLevelUpdate 
		@FuelCode = @tmp_FuelCode,
		@AmountOfFuel = @tmp_AmountFuel,
		@isDelivery = 1;

		FETCH NEXT FROM orders_cursor INTO 
		@tmp_IdOrderDetails,
	    @tmp_AmountFuel,
		@tmp_FuelCode;
	END

	CLOSE orders_cursor;
    DEALLOCATE orders_cursor;

	END
	ELSE
		BEGIN
		PRINT 'Zamówienie nie istnieje';
        THROW 50007, 'Zamówienie nie istnieje', 1;
		END
	


	
	

        -- Jeśli wszystkie operacje zakończą się sukcesem, zatwierdź transakcję
        COMMIT;
    END TRY
    BEGIN CATCH
        -- W przypadku błędu, cofnij transakcję
        ROLLBACK;

        -- Opcjonalnie, obsłuż błąd
        DECLARE @ErrorMessage NVARCHAR(4000);
        DECLARE @ErrorSeverity INT;
        DECLARE @ErrorState INT;

        SELECT 
            @ErrorMessage = ERROR_MESSAGE(),
            @ErrorSeverity = ERROR_SEVERITY(),
            @ErrorState = ERROR_STATE();

        -- Wyrzuć błąd ponownie
        RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;
```

<br>


### Procedura: p_confirmOrder - potwierdzamy wysłanie zamówienia - czyli zamykamy koszyk ale bez potwierdzenia odbioru

```sql
CREATE     PROCEDURE [dbo].[p_confirmOrder]
	@IDOrder int
AS
BEGIN
    -- Rozpoczęcie transakcji
    BEGIN TRANSACTION;

    BEGIN TRY
	IF EXISTS (SELECT 1 FROM orders where idOrder = @IDOrder)
	BEGIN
		update orders
		set orderDate = GETDATE()
		where idOrder = @IDOrder;
	END
	ELSE
		BEGIN
		PRINT 'Zamówienie nie istnieje';
		THROW 50007, 'Zamówienie nie istnieje', 1;
		END

	
	

        -- Jeśli wszystkie operacje zakończą się sukcesem, zatwierdź transakcję
        COMMIT;
    END TRY
    BEGIN CATCH
        -- W przypadku błędu, cofnij transakcję
        ROLLBACK;

        -- Opcjonalnie, obsłuż błąd
        DECLARE @ErrorMessage NVARCHAR(4000);
        DECLARE @ErrorSeverity INT;
        DECLARE @ErrorState INT;

        SELECT 
            @ErrorMessage = ERROR_MESSAGE(),
            @ErrorSeverity = ERROR_SEVERITY(),
            @ErrorState = ERROR_STATE();

        -- Wyrzuć błąd ponownie
        RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;
```

<br>


### Procedura: p_endFueling - koniec tankowania - wyzwalane odłożeniem pistoletu przy dystrybutorze na swoje miejscie

```sql
CREATE     PROCEDURE [dbo].[p_confirmOrder]
	@IDOrder int
AS
BEGIN
    -- Rozpoczęcie transakcji
    BEGIN TRANSACTION;

    BEGIN TRY
	IF EXISTS (SELECT 1 FROM orders where idOrder = @IDOrder)
	BEGIN
		update orders
		set orderDate = GETDATE()
		where idOrder = @IDOrder;
	END
	ELSE
		BEGIN
		PRINT 'Zamówienie nie istnieje';
		THROW 50007, 'Zamówienie nie istnieje', 1;
		END

	
	

        -- Jeśli wszystkie operacje zakończą się sukcesem, zatwierdź transakcję
        COMMIT;
    END TRY
    BEGIN CATCH
        -- W przypadku błędu, cofnij transakcję
        ROLLBACK;

        -- Opcjonalnie, obsłuż błąd
        DECLARE @ErrorMessage NVARCHAR(4000);
        DECLARE @ErrorSeverity INT;
        DECLARE @ErrorState INT;

        SELECT 
            @ErrorMessage = ERROR_MESSAGE(),
            @ErrorSeverity = ERROR_SEVERITY(),
            @ErrorState = ERROR_STATE();

        -- Wyrzuć błąd ponownie
        RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;
```

<br>


### Procedura: p_fuelLevelUpdate - procedura obsługujaca ruch magazynowy w magazynie paliw

```sql
CREATE PROCEDURE [dbo].[p_fuelLevelUpdate]
    @FuelCode VARCHAR(6),
    @AmountOfFuel float,
	@isDelivery bit
	AS
	BEGIN
    -- Rozpoczęcie transakcji
    BEGIN TRANSACTION;

    BEGIN TRY
       
	DECLARE @FuelCurrLevel float;
	DECLARE @FuelMaxLevel float;

	select @FuelCurrLevel = fuelCurrLevel
	from fuelStorage
	where fuelCode = @FuelCode;

	select @FuelMaxLevel = fuelMaxLevel
	from fuelStorage 
	where fuelCode = @FuelCode;

	if @isDelivery = 0										  -- jeśli 0 to znaczy że ktoś pobiera paliwo
		BEGIN												  
		SET @FuelCurrLevel = @FuelCurrLevel - @AmountOfFuel;  -- ustawienie poziomu paliwa po odjęciu
		
		if @fuelCurrLevel < 0								  -- sprawdzenie czy poziom nie jest za niski
			BEGIN
				PRINT 'Za niski poziom paliwa';					  -- jeśli jest za niski to błąd
				THROW 50008, 'Za niski poziom paliwa', 1;
			END
		else
			BEGIN											  -- jeśli jest ok, to update na tabeli
				update fuelStorage
				set fuelCurrLevel = @FuelCurrLevel
				where fuelCode = @FuelCode;
			END

		END
	else
		BEGIN													--else czyli 1 - dostawa paliwa, zwiększamy poziom w zbiorniku
			BEGIN												  
			SET @FuelCurrLevel = @FuelCurrLevel + @AmountOfFuel;  -- ustawienie poziomu paliwa po odjęciu
			if @fuelCurrLevel > @FuelMaxLevel								  -- sprawdzenie czy poziom nie jest za wysoki
				BEGIN
					PRINT 'Za wysoki poziom paliwa';					  -- jeśli jest za wysoki - błąd 
					THROW 50008, 'Za wysoki poziom paliwa', 1;
				END
			else
				BEGIN											  -- jeśli jest ok, to update na tabeli
					update fuelStorage
					set fuelCurrLevel = @fuelCurrLevel
					where fuelCode = @FuelCode;
				END

		END
		END

        -- Jeśli wszystkie operacje zakończą się sukcesem, zatwierdź transakcję
        COMMIT;
    END TRY
    BEGIN CATCH
        -- W przypadku błędu, cofnij transakcję
        ROLLBACK;

        -- Opcjonalnie, obsłuż błąd
        DECLARE @ErrorMessage NVARCHAR(4000);
        DECLARE @ErrorSeverity INT;
        DECLARE @ErrorState INT;

        SELECT 
            @ErrorMessage = ERROR_MESSAGE(),
            @ErrorSeverity = ERROR_SEVERITY(),
            @ErrorState = ERROR_STATE();

        -- Wyrzuć błąd ponownie
        RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;



```

<br>


### Procedura: p_fuelPriceUpdate - aktualizacja ceny dla danego paliwa

```sql
CREATE PROCEDURE [dbo].[p_fuelPriceUpdate]
    @fuelCode VARCHAR(6),
    @NewPrice float
AS
BEGIN
    -- Rozpoczęcie transakcji
    BEGIN TRANSACTION;

    BEGIN TRY

		IF @NewPrice > 0 AND EXISTS (SELECT 1 FROM fuelStorage where fuelCode = @fuelCode)
		BEGIN
			insert into fuelPriceHistory (date, fuelCode, price)
			values (GETDATE(), @fuelCode, @NewPrice)
		END
		ELSE
			BEGIN
			PRINT 'Błąd parametrów';
			THROW 50008, 'Błąd parametrów', 1;
			END

        -- Jeśli wszystkie operacje zakończą się sukcesem, zatwierdź transakcję
        COMMIT;
    END TRY
    BEGIN CATCH
        -- W przypadku błędu, cofnij transakcję
        ROLLBACK;

        -- Opcjonalnie, obsłuż błąd
        DECLARE @ErrorMessage NVARCHAR(4000);
        DECLARE @ErrorSeverity INT;
        DECLARE @ErrorState INT;

        SELECT 
            @ErrorMessage = ERROR_MESSAGE(),
            @ErrorSeverity = ERROR_SEVERITY(),
            @ErrorState = ERROR_STATE();

        -- Wyrzuć błąd ponownie
        RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;


```

<br>


### Procedura: p_genLose - generowanie dokumentu strat - powiązane z konkretnym tankowaniem z fuelingHistory

```sql
CREATE   PROCEDURE [dbo].[p_genLose]
    @IDFueling int,
	@CarID varchar (20) = NULL
AS
BEGIN
    -- Rozpoczęcie transakcji
    BEGIN TRANSACTION;

    BEGIN TRY

	DECLARE @StationNumber int;

	--select @PumpGun = idPumpGun							--pobranie numeru pistoletu z idFueling
	--from fuelingHistory
	--where idFueling = @IDFueling;

	--select @StationNumber = pg.idStationNumber				--pobranie do zmiennej station number na podstawie numeru pistoletu
    --from pumpGuns pg
	--join distStation ds ON ds.idStationNumber = pg.idStationNumber 
   	--where pg.idPumpGun = @PumpGun

	select @StationNumber = pg.idStationNumber				--pobranie do zmiennej station number na podstawie numeru tankowania
    from pumpGuns pg
	join fuelingHistory fh ON fh.idPumpGun = pg.idPumpGun
	join distStation ds ON ds.idStationNumber = pg.idStationNumber 
   	where fh.idFueling = @IDFueling


	insert into losesHistory (idFueling, carID) values
	(@IDFueling, @CarID);

	update distStation
	set transactionFinished = 1, activeGun = 0
	where idStationNumber = @StationNumber;



        -- Jeśli wszystkie operacje zakończą się sukcesem, zatwierdź transakcję
        COMMIT;
    END TRY
    BEGIN CATCH
        -- W przypadku błędu, cofnij transakcję
        ROLLBACK;

        -- Opcjonalnie, obsłuż błąd
        DECLARE @ErrorMessage NVARCHAR(4000);
        DECLARE @ErrorSeverity INT;
        DECLARE @ErrorState INT;

        SELECT 
            @ErrorMessage = ERROR_MESSAGE(),
            @ErrorSeverity = ERROR_SEVERITY(),
            @ErrorState = ERROR_STATE();

        -- Wyrzuć błąd ponownie
        RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;
```

<br>


### Procedura: p_genTransactionDoc - generowanie dokumentu sprzedażowego

```sql
CREATE   PROCEDURE [dbo].[p_genTransactionDocument]
	@DocType int,
	@IDContractorNIP varchar (15),
	@TaxPTU int,
	@PaymentMethod int,
	@Distributor int,
	@IDDocNumeration int,
	@IDEmployee int
AS
BEGIN
    -- Rozpoczęcie transakcji
    BEGIN TRANSACTION;

    BEGIN TRY

	DECLARE @LastTitleNumber int;		--pobieranie ostatniej wartości licznika dla danego dokumentu
	DECLARE @LastDate datetime;
	DECLARE @IDFueling int;				--przechowywanie id fueling pobrane po @Distributor

	IF (@DocType = 1 or @DocType = 2) AND 
	EXISTS (SELECT 1 FROM contractors where idContractorNIP = @IDContractorNIP) AND 
	(@PaymentMethod BETWEEN 1 AND 3)  AND 
	EXISTS (SELECT 1 FROM distStation where idStationNumber = @Distributor) AND
	EXISTS (SELECT 1 FROM employees where idEmployee = @IDEmployee)
	BEGIN

	select top 1 @LastTitleNumber = docTitleNumber	--wczytywanie ostatniego numeru dokumentu i inkrementacja
	from transactionDocuments
	where docType = @DocType
	order by docTitleNumber DESC;

	select @LastDate = date							--pobieranie daty ostatniego dokumentu
	from transactionDocuments
	where docType = @DocType AND docTitleNumber = @LastTitleNumber;

	if (MONTH(GETDATE()) = MONTH(@LastDate))		--jeśli miesiąc na ostatnim dokumencie jest inny, zeruj licznik do 1
	BEGIN
		SET @LastTitleNumber = @LastTitleNumber + 1; 
		PRINT @LastTitleNumber;
	END
	else
	BEGIN
		SET @LastTitleNumber = 1;
		PRINT @LastTitleNumber;
	END
	

	select @IDFueling = idFueling							--wyciąganie idFueling po numerze dystrybutora
	from distStation ds
	join pumpGuns pg ON ds.idStationNumber = pg.idStationNumber
	join fuelingHistory fh ON pg.idPumpGun = fh.idPumpGun
	where ds.idStationNumber = @Distributor AND activeGun != 0



	--utworzenie dokumentu
	insert into transactionDocuments (docType, docTitleNumber, date, idContractorNIP, taxPTU, paymentMethod, idFueling, idDocNumeration, idEmployee) values
	(@DocType, @LastTitleNumber, GETDATE(), @IDContractorNIP, @TaxPTU, @PaymentMethod, @IDFueling, @IDDocNumeration, @IDEmployee);

	update distStation								--konczenie transakcji
	set activeGun = 0, transactionFinished = 1
	where idStationNumber = @Distributor;

	END
	ELSE
		BEGIN
		PRINT 'Błąd parametrów';
		THROW 50008, 'Błąd parametrów', 1;
		END

        -- Jeśli wszystkie operacje zakończą się sukcesem, zatwierdź transakcję
        COMMIT;
    END TRY
    BEGIN CATCH
        -- W przypadku błędu, cofnij transakcję
        ROLLBACK;

        -- Opcjonalnie, obsłuż błąd
        DECLARE @ErrorMessage NVARCHAR(4000);
        DECLARE @ErrorSeverity INT;
        DECLARE @ErrorState INT;

        SELECT 
            @ErrorMessage = ERROR_MESSAGE(),
            @ErrorSeverity = ERROR_SEVERITY(),
            @ErrorState = ERROR_STATE();

        -- Wyrzuć błąd ponownie
        RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;
```

<br>


### Procedura: p_startFueling - rozpoczęcie tankowania - aktywowane podniesiem pistoletu przy dystrybutorze

```sql
CREATE PROCEDURE [dbo].[p_startFueling]
    @ActiveGun int
AS
BEGIN
    -- Rozpoczęcie transakcji
    BEGIN TRANSACTION;

    BEGIN TRY

		IF ((select transactionFinished from distStation where activeGun = @ActiveGun) = 0) 
		BEGIN
			PRINT 'Dystrybutor zajęty';
			THROW 50011, 'Dystrybutor zajęty', 1;
		END
		ELSE
		BEGIN
		   DECLARE @FuelCode varchar(6);
		   DECLARE @IDActualPrice int;
		   DECLARE @StationNumber int;

		   select @FuelCode = fuelCode				--pobieranie FuelCode
		   from pumpGuns 
		   where idPumpGun = @ActiveGun;

		   select @IDActualPrice = id				--pobieranie ceny
		   from v_actualPrice
		   where fuelCode = @FuelCode;


		   select @StationNumber = pg.idStationNumber		--ustalenie dystrybutora
		   from pumpGuns pg
		   join distStation ds ON ds.idStationNumber = pg.idStationNumber 
   		   where pg.idPumpGun = @ActiveGun

		   update distStation 
		   set amountOfFuel = 0, idActualPrice = @IDActualPrice, activeGun = @ActiveGun
		   where idStationNumber = @StationNumber
		END



	

        -- Jeśli wszystkie operacje zakończą się sukcesem, zatwierdź transakcję
        COMMIT;
    END TRY
    BEGIN CATCH
        -- W przypadku błędu, cofnij transakcję
        ROLLBACK;

        -- Opcjonalnie, obsłuż błąd
        DECLARE @ErrorMessage NVARCHAR(4000);
        DECLARE @ErrorSeverity INT;
        DECLARE @ErrorState INT;

        SELECT 
            @ErrorMessage = ERROR_MESSAGE(),
            @ErrorSeverity = ERROR_SEVERITY(),
            @ErrorState = ERROR_STATE();

        -- Wyrzuć błąd ponownie
        RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;
```

<br><br><br>






## Triggery

<br><br>

### Tabela: contractors; Trigger: tgr_Contractors_Changed

Opis: Zapisuje w tabeli contractorsArchive zmiany wykonane na kontraktorach

```sql
CREATE TRIGGER [dbo].[trg_Contractors_Changed]
ON [dbo].[contractors]
AFTER UPDATE, DELETE
AS
BEGIN
    SET NOCOUNT ON;

    -- Obsługa operacji UPDATE
    IF EXISTS (SELECT * FROM inserted) AND EXISTS (SELECT * FROM deleted)
    BEGIN
        IF UPDATE(contractorName) OR
           UPDATE(address) OR
           UPDATE(address2) OR
           UPDATE(postalCode) OR
           UPDATE(contactNumber) OR
           UPDATE(email) OR
           UPDATE(contractorType)
        BEGIN
            INSERT INTO dbo.contractorsArchive (
                idContractorNIP,
                operationType,
                NEW_contractorName,
                NEW_address,
                NEW_address2,
                NEW_postalCode,
                NEW_contactNumber,
                NEW_email,
                NEW_contractorType,
                OLD_contractorName,
                OLD_address,
                OLD_address2,
                OLD_postalCode,
                OLD_contactNumber,
                OLD_email,
                OLD_contractorType
            )
            SELECT 
                i.idContractorNIP,
                'UPDATE',
                CASE WHEN i.contractorName <> d.contractorName THEN i.contractorName ELSE NULL END,
                CASE WHEN i.address <> d.address THEN i.address ELSE NULL END,
                CASE WHEN i.address2 <> d.address2 THEN i.address2 ELSE NULL END,
                CASE WHEN i.postalCode <> d.postalCode THEN i.postalCode ELSE NULL END,
                CASE WHEN i.contactNumber <> d.contactNumber THEN i.contactNumber ELSE NULL END,
                CASE WHEN i.email <> d.email THEN i.email ELSE NULL END,
                CASE WHEN i.contractorType <> d.contractorType THEN i.contractorType ELSE NULL END,
                d.contractorName,
                d.address,
                d.address2,
                d.postalCode,
                d.contactNumber,
                d.email,
                d.contractorType
            FROM inserted i
            INNER JOIN deleted d ON i.idContractorNIP = d.idContractorNIP
            WHERE i.contractorName <> d.contractorName OR
                  i.address <> d.address OR
                  i.address2 <> d.address2 OR
                  i.postalCode <> d.postalCode OR
                  i.contactNumber <> d.contactNumber OR
                  i.email <> d.email OR
                  i.contractorType <> d.contractorType;
        END
    END

    -- Obsługa operacji DELETE
    IF EXISTS (SELECT * FROM deleted) AND NOT EXISTS (SELECT * FROM inserted)
    BEGIN
        INSERT INTO dbo.contractorsArchive (
            idContractorNIP,
            operationType,
            OLD_contractorName,
            OLD_address,
            OLD_address2,
            OLD_postalCode,
            OLD_contactNumber,
            OLD_email,
            OLD_contractorType
        )
        SELECT 
            d.idContractorNIP,
            'DELETE',
            d.contractorName,
            d.address,
            d.address2,
            d.postalCode,
            d.contactNumber,
            d.email,
            d.contractorType
        FROM deleted d;
    END
END;

```

<br>

### Tabela: fuelStorage; Trigger: tgr_fuelStorage_Change

Opis: Zapisuje w tabeli fuelStorageArchive zmiany wykonane w magazynie paliw

```sql
CREATE TRIGGER [dbo].[trg_FuelStorage_Change]
ON [dbo].[fuelStorage]
AFTER UPDATE
AS
BEGIN
	SET NOCOUNT ON;
	IF UPDATE(fuelCurrLevel)
	BEGIN

	INSERT INTO fuelStorageArchive(
		fuelCode,
		operationType,
		NEW_fuelCurrLevel,
		OLD_fuelCurrLevel
	)
	SELECT 
		i.fuelCode,
		CASE WHEN i.fuelCurrLevel < d.fuelCurrLevel THEN 'REFUELING' ELSE 'SUPPLY' END,
		i.fuelCurrLevel,
		d.fuelCurrLevel
	   FROM inserted i
            INNER JOIN deleted d ON i.fuelCode= d.fuelCode
	END

	IF UPDATE(fuelMaxLevel)
	BEGIN
	
	INSERT INTO fuelStorageArchive(
		fuelCode,
		operationType,
		NEW_fuelMaxLevel,
		OLD_fuelMaxLevel
	)
	SELECT 
		i.fuelCode,
		'CHANGE_MAX_LEVEL',
		i.fuelMaxLevel,
		d.fuelMaxLevel
	   FROM inserted i
            INNER JOIN deleted d ON i.fuelCode= d.fuelCode
	END

		IF UPDATE(tankNumber)
	BEGIN
	
	INSERT INTO fuelStorageArchive(
		fuelCode,
		operationType,
		NEW_tankNumber,
		OLD_tankNumber
	)
	SELECT 
		i.fuelCode,
		'CHANGE_TANK',
		i.tankNumber,
		d.tankNumber
	   FROM inserted i
            INNER JOIN deleted d ON i.fuelCode= d.fuelCode
	END
END;
```

  

(dla każdego triggera należy wkleić kod polecenia definiującego trigger wraz z komentarzem)


## Constrains

fuelPriceHistory - price > 0

distStation - amountOfFuel >= 0
distStation - activeGun <= 16

fuelStorage - fuelCurrLevel < fuelMaxLevel
fuelStorage - fuelCurrLevel > 0

orderDetails - amountOfFuel > 0
oderdDetails - pricePerLiter >= 0
