# Raport Końcowy - Beniamin Jankowski, Marcel Trzaskawka

## Spis treści

- [Raport Końcowy - Beniamin Jankowski, Marcel Trzaskawka](#raport-końcowy---beniamin-jankowski-marcel-trzaskawka)
  - [Spis treści](#spis-treści)
  - [Role](#role)
    - [Opis Widoków](#opis-widoków)
    - [Opis Triggerów](#opis-triggerów)
    - [Opis Funkcji i Procedur](#opis-funkcji-i-procedur)
  - [**Schemat bazy**](#schemat-bazy)
    - [1. Categories](#1-categories)
    - [2. Meals](#2-meals)
    - [3. Menus](#3-menus)
    - [4. Menu Details](#4-menu-details)
    - [5. Staff](#5-staff)
    - [6. Tables](#6-tables)
    - [7. Customers](#7-customers)
    - [8. Bills](#8-bills)
    - [9. Orders](#9-orders)
    - [10. TablesReservation](#10-tablesreservation)
    - [11. ReservationDetails](#11-reservationdetails)
    - [12. OrderDetails](#12-orderdetails)
    - [13. NamesFromCompanies](#13-namesfromcompanies)
    - [14. PaymentTypes](#14-paymenttypes)
    - [15. PaymentDetails](#15-paymentdetails)
  - [**Kod DDL wraz z warunkami integralnościowymi**](#kod-ddl-wraz-z-warunkami-integralnościowymi)
  - [Zapełnianie bazy danymi](#zapełnianie-bazy-danymi)
  - [Widoki](#widoki)
    - [ViewAllClientsAllOrdersValue](#viewallclientsallordersvalue)
    - [ViewIndividualCLients](#viewindividualclients)
    - [ViewBuisnessClients](#viewbuisnessclients)
    - [ViewEachOrderValue](#vieweachordervalue)
    - [ViewAllOrdersValue](#viewallordersvalue)
    - [ViewAllOnlineReservationsDetails](#viewallonlinereservationsdetails)
    - [ViewAllCanceledReservationsDetails](#viewallcanceledreservationsdetails)
    - [ViewAllMenus](#viewallmenus)
    - [ViewCurrentMenu](#viewcurrentmenu)
  - [Triggery](#triggery)
    - [MaximumOrdersPerWaiter](#maximumordersperwaiter)
  - [Funkcje i Procedury](#funkcje-i-procedury)
    - [CheckAvailableTables](#checkavailabletables)
    - [Reservation](#reservation)
    - [MakeOrderAtPlace](#makeorderatplace)
    - [MakeOrderTakeAway](#makeordertakeaway)
    - [CancelOrder](#cancelorder)
    - [AddToOrder](#addtoorder)
    - [RemoveFromOrder](#removefromorder)
    - [MakeMenu](#makemenu)
    - [AddNewMeal](#addnewmeal)
    - [InsertMealToMenu](#insertmealtomenu)
    - [ConfirmMenuChange](#confirmmenuchange)
    - [ChangeMealPrice](#changemealprice)
    - [Payment](#payment)
    - [IssueBill](#issuebill)
    - [PrintBill](#printbill)
    - [IssueInvoice](#issueinvoice)
    - [PrintInvoice](#printinvoice)
  - [Raporty](#raporty)
    - [GeneratePrivateClientReport](#generateprivateclientreport)
    - [GeneratePrivateClientMonthlyReport](#generateprivateclientmonthlyreport)
    - [GenerateBuisnessClientReport](#generatebuisnessclientreport)
    - [GenerateBuisnessClientMonthlyReport](#generatebuisnessclientmonthlyreport)
    - [GenerateAdminReport](#generateadminreport)

## Role

- Administrator
- System
- Menedżer
- Szef Kuchni
- Kucharz
- Kelner
- Użytkownik

### Opis Widoków

1. ViewAllClientsAllOrdersValue
    - Wyświetla wartości wszystkich zamówień dla poszczególnych klientów
  
2. ViewIndividualCLients
    - Wyświetla wszystkich klientów Prywatnych (*IsCompany* = 0)
  
3. ViewBuisnessClients
    - Wyświetla wszystkich klientów Fizycznych (*IsCompany* = 1)

4. ViewEachOrderValue
    - Wyświetla łączną wartość poszczególnych zamówień

5. ViewAllOrdersValue
    - Wyświetla łączną wartość wszystkich zamówień

6. ViewAllOnlineReservationsDetails
    - Wyświetla detale rezerwacji online

7. ViewAllCanceledReservationsDetails
    - Wyświetla detale anulowanych rezerwacji

8. ViewAllMenus
    - Wyświetla wszystkie informacje na temat wszystkich menu wraz z daniami

9. ViewCurrentMenu
    - Wyświetla informacje i dania aktualnego menu

### Opis Triggerów

1. MaximumOrdersPerWaiter  
    - Sprawdzamy, czy dany kelner nie jest zbyt obciążony - może mieć aktualnych naraz maksymalnie 5 dań na godzinę

### Opis Funkcji i Procedur

1. CheckAvailableTables
    - Podając konretną datę możemy zobaczyć, ile stolików jest dostępnych

2. Reservation - należy najpierw sprawdzić aktualne menu widokiem *ViewCurrentMenu*, z którego to klienci mogą wybierać dania na zadany termin
    - Przy rezerwacji należy zamówić dania, które klienci chcą zjeść.
    - Dania muszą być w aktualnym menu.
    - Bez złożenia zamówienia rezerwacja nie będzie możliwa.
    - klienci indywidualni - wymagana jest wcześniejsza liczba zamówień (tzn. minimum *5*). Po spełnieniu tego warunku zamówienie również **musi** przekroczyć kwotę *50 zł*
    - klienci firmowi - brak wymaganych warunków do spełnienia w celu rezerwacji. Możliowść również rezerwacji imiennych - tzn. firma może wymienić osoby, które będą uczestniczyły w posiłku.
    - istnieje możliwość zarezerwowania więcej stolików niż 1.
    - Rezerwację poszczególnych stolików można anulować.
    - Czas trwania rezerwacji domyślnie trwa 240 min (4h) dla stolika. W szczególnych przypadkach, czas można ustawić na inny.
    - Nie można zarezerwować stolika na datę, kiedy menu nie jest jeszcze znane.
    - Owoce morza-  na dni: **czwartek**, **piątek** oraz **sobota** istnieje możliwość zamówienia owoców morza. Taką rezerwację należy złożyć co najwyżej do poniedziałku poprzedzającego rezerwację.

3. MakeOrderAtPlace
    - Stwórz zamówienie na miejscu oraz 'zarezerwuj stolik'

4. MakeOrderTakeAway
    - Złóż zamówienie na miejscu lub przez internet, które będzie na wynos

5. CancelOrder
    - Anuluj zamówienie

6. AddToOrder
    - Dodaj dania do zamówienia

7. RemoveFromOrder
    - Usuń danie z zamówienia

8. MakeMenu
    - Stwórz nowe menu

9. AddNewMeal
    - Dodaje nowy posiłek do danego menu

10. InsertMealToMenu
    - Dodaje posiłek do danego menu

11. ConfirmMenuChange
    - Aktualizacja menu wraz ze sprawdzeniem, czy co najmniej połowa się zmieniła

12. ChangeMealPrice
    - Zmień cenę danego posiłku w danym menu
  
13. Payment
    - Dokonaj płatności za zamówienie

14. IssueBill
    - Wystawia rachunek

15. PrintBill
    - Wyświetla szczegóły rachunku

16. IssueInvoice
    - Wystawia fakturę

17. PrintInvoice
    - Wyświeta szczegóły faktury

18. GeneratePrivateClientReport
    - Generuje raport dla klientów prywatnych
    - Znajdują się w nim detale dotyczące rachunków, zamówień i kwot z podanego okresu
    - Argumenty
      - CustomerID
      - StartingDate
      - EndingDate
19. GeneratePrivateClientMonthlyReport
    - Generuje raport miesięczny dla klientów prywatnych
    - Znajdują się w nim detale dotyczące rachunków, zamówień i kwot dla podanego miesiąca
    - Argumenty
      - CustomerID
      - StartingMonth (*Rok ma znaczenie, podać w formacie 'RRRR-MM-DD')
20. GenerateBuisnessClientReport
    - Generuje raport dla klientów fizycznych
    - Znajdują się w nim detale dotyczące rachunków, zamówień i kwot z podanego okresu
    - Argumenty
      - CustomerID
      - StartingDate
      - EndingDate
21. GenerateBuisnessClientMonthlyReport
    - Generuje raport miesięczny dla klientów fizycznych
    - Znajdują się w nim detale dotyczące rachunków, zamówień i kwot dla podanego miesiąca
    - Argumenty
      - CustomerID
      - StartingMonth (*Rok ma znaczenie, podać w formacie 'RRRR-MM-DD')
22. GenerateAdminReport
    - Generuje statystki dotyczące zamówień, rachunków, klientów i przychodów dla podanego okresu
    - Argumenty
      - StartDate
      - EndDate

## **Schemat bazy**

![Schemat bazy](images/schemat.png)

---

### 1. Categories

- CategoryID - klucz główny dla tabeli
- CategoryName - nazwa kategorii
- Description - opis kategorii

### 2. Meals

<!-- Tabela zajmuje mało miejsca, bo tylko przetrzymuje informacje o daniach.  
W przypadku zmiany ceny, obecne danie będzie oznaczone jako Discontinued, a następnie wprowadzone będzie to samo danie ze zmienioną ceną. -->

- MealID - klucz główny dla tabeli
- CategoryID - przynależna kategoria
- MealName - nazwa posiłku
- Discontinued - informacja, czy dany posiłek jest dostępny do zamówienia:

  - 0 -> posiłek jest dostępny
  - 1 -> posiłek jest niedostępny

### 3. Menus

<!-- Ograniczyliśmy zajmowane miejsce w odniesieniu do starego rozwiązania, ponieważ tabela nie przetrzymuje niepotrzebnych informacji. -->

- MenuID - klucz główny dla tabeli
- MenuStart - data rozpoczęcia obowiązywania menu
- MenuEnd - data zakończenia obowiązywania menu
- Discontinued - informacja, czy dane menu jest dostępne:

  - 0 -> menu jest dostępne
  - 1 -> menu jest niedostępne

### 4. Menu Details

- MenuID - pierwszy klucz główny dla tabeli
- MealID - drugi klucz główny dla tabeli
- Price - cena za dany posiłek w danym menu

### 5. Staff

- EmployeeID - klucz główny dla tabeli
- Firstname - imię pracownika
- Lastname - nazwisko pracownika
- Address - adres zamieszkania
- DateOfBirth - data urodzenia
- Role - rola pracownika w restauracji

### 6. Tables

- TableID - klucz główny dla tabeli
- ChairCount - liczba krzeseł przy stoliku
- Discontinued - informacja, czy dany stolik jest dostępny jako możliwy do użycia (w razie zepsucia/naprawy itp.):

  - 0 -> stolik jest dostępny
  - 1 -> stolik nie jest niedostępny

### 7. Customers

- CustomerID - klucz główny dla tabeli
- Name - imię klienta lub nazwa firmy
- Lastname - nazwisko klienta
- NIP - numer NIP firmy
- Address - adres firmy
- PhoneNumber - numer kontaktowy do klienta/firmy
- IsComponay - informacja, czy dany rekord jest firmą, czy osobą prywatną:

  - 0 -> klient jest klientem prywatnym
  - 1 -> klient jest firmą

### 8. Bills

- BillID - klucz główny dla tabeli
- DateOfIssue - data wystawienia rachunku (dla wartości NULL rachunek nie jest jeszcze domknięty)
- Refund - informacja o tym, czy dany rachunek jest zwrotny:

  - 0 -> zwykły rachunek
  - 1 -> rachunek zwrotny

### 9. Orders

- OrderID - klucz główny dla tabeli
- CustomerID - ID klienta
- WaiterID - (*EmployeeID* dla tabeli **Staff**) informacja, kto obsługiwał dane zamówienie
- OrderDate - data złożenia zamówienia
- ServedDate - data zaserwowania/odbioru dania (dla wartości **NULL** zamówienie nie zostało jeszcze zrealizowane)
- TakeAway - informacja, czy dane zamówienie jest brane na wynos:

  - 0 -> zamówienie na miejscu
  - 1 -> zamówienie na wynos

- Canceled - informacja o anulowaniu danego zamówienia:

  - 0 -> zamówienie aktualne
  - 1 -> zamówienie anulowane

- ExpectedDate:

  - na wynos przez internet: oczekiwana data przyjścia po posiłek,
  - na wynos dla zamówienia złożonego w restauracji: przewidywana data podana przez kuchnię
  - na miejscu z rezerwacją - start rezerwacji (dzięki temu nie musimy wprowadzać osobnej kolumny w tabelach rezerwacji)
  - na miejscu bez rezerwacji - przewidywana data podana przez kuchnię

- IsOnline - informacja o tym, czy dane zamówienie zostało złożone przez internet:

  - 0 -> zamówienie złożone na miejscu
  - 1 -> zamówienie złożone przez internet

- BillID - informacja o numerze rachunku, pod które podpięte jest zamówienie

### 10. TablesReservation

- OrderID - klucz główny dla tabeli
- TimeSpan - przewidywany czas rezerwacji w minutach (domyślnie - 4h (240 min))

### 11. ReservationDetails

- OrderID - pierwszy klucz główny dla tabeli
- TableID - drugi klucz główny dla tabeli
- Canceled - informacja o anulowaniu danej rezerwacji dla danego stolika:

  - 0 -> rezerwacja stolika aktualna
  - 1 -> anulowanie rezerwacji stolika

### 12. OrderDetails

- OrderID - pierwszy klucz główny dla tabeli
- MealID - drugi klucz główny dla tabeli
- MenuID - informacja, do jakiego menu należy dany posiłek w danym zamówieniu
- Amount - informacja o liczbie złożonych konretnych dań

### 13. NamesFromCompanies

- NFC - klucz główny dla tabeli
- OrderID - numer zamówienia
- FirstName - imię osoby, która uczestniczy w zamówieniu
- LastName - nazwisko osoby, która uczestniczy w zamówieniu

### 14. PaymentTypes

- PaymentTypeID - klucz główny dla tabeli
- PaymentName - rodzaj płatności

### 15. PaymentDetails

- PaymentID - klucz główny dla tabeli
- OrderID - numer zamówienia
- PaymentTypeID - rodzaj płatności
- Sum - zapłacona część zamówienia
- DateOfPayment - data wpłaty
- Refund - czy dana płatność ma zostać wycofana:

  - 0 -> normalna płatność
  - 1 -> płatność do zwrócenia

---

## **Kod DDL wraz z warunkami integralnościowymi**

1. Categories

    ```sql
    CREATE TABLE [dbo].[Categories] (
       [CategoryID] [int] IDENTITY(1, 1) NOT NULL,
       [CategoryName] [varchar](50) NOT NULL,
       [Description] [text] NULL,
    CONSTRAINT [PK_Categories] PRIMARY KEY CLUSTERED 
    (
        [CategoryID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
    GO
    ```

2. Meals

    ```sql
    CREATE TABLE [dbo].[Meals] (
        [MealID] [int] IDENTITY(1, 1) NOT NULL,
        [CategoryID] [int] NOT NULL,
        [MealName] [varchar](50) NOT NULL,
        [Discontinued] [bit] NOT NULL,
    CONSTRAINT [PK_Meals] PRIMARY KEY CLUSTERED 
    (
        [MealID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO

    ALTER TABLE [dbo].[Meals] ADD  CONSTRAINT [DF_Meals_Discontinued]  DEFAULT ((0)) FOR [Discontinued]
    GO


    ALTER TABLE [dbo].[Meals]  WITH CHECK ADD  CONSTRAINT [FK_Meals_Categories] FOREIGN KEY([CategoryID])
    REFERENCES [dbo].[Categories] ([CategoryID])
    GO

    ALTER TABLE [dbo].[Meals] CHECK CONSTRAINT [FK_Meals_Categories]
    GO
    ```

3. Menus

    ```sql
    CREATE TABLE [dbo].[Menus] (
        [MenuID] [int] IDENTITY(1, 1) NOT NULL,
        [MenuStart] [date] NOT NULL,
        [MenuEnd] [date] NOT NULL,
        [Discontinued] [bit] NOT NULL,
    CONSTRAINT [PK_Menu] PRIMARY KEY CLUSTERED 
    (
        [MenuID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO

    ALTER TABLE [dbo].[Menus] ADD  CONSTRAINT [DF_Menus_Discontinued]  DEFAULT ((0)) FOR [Discontinued]
    GO

    ALTER TABLE [dbo].[Menus]  WITH CHECK ADD  CONSTRAINT [CK_Menus] CHECK  (([MenuStart]<[MenuEnd]))
    GO

    ALTER TABLE [dbo].[Menus] CHECK CONSTRAINT [CK_Menus]
    GO
    ```

4. MenuDetails

    ```sql
    CREATE TABLE [dbo].[MenuDetails] (
        [MenuID] [int] NOT NULL,
        [MealID] [int] NOT NULL,
        [Price] [money] NOT NULL,
    CONSTRAINT [PK_MenuDetails] PRIMARY KEY CLUSTERED 
    (
        [MenuID] ASC,
        [MealID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO

    ALTER TABLE [dbo].[MenuDetails]  WITH CHECK ADD  CONSTRAINT [FK_MenuDetails_Meals] FOREIGN KEY([MealID])
    REFERENCES [dbo].[Meals] ([MealID])
    GO

    ALTER TABLE [dbo].[MenuDetails] CHECK CONSTRAINT [FK_MenuDetails_Meals]
    GO

    ALTER TABLE [dbo].[MenuDetails]  WITH CHECK ADD  CONSTRAINT [FK_MenuDetails_Menu] FOREIGN KEY([MenuID])
    REFERENCES [dbo].[Menus] ([MenuID])
    GO

    ALTER TABLE [dbo].[MenuDetails] CHECK CONSTRAINT [FK_MenuDetails_Menu]
    GO

    ALTER TABLE [dbo].[MenuDetails]  WITH CHECK ADD  CONSTRAINT [CK_MenuDetails] CHECK  (([Price]>(0)))
    GO

    ALTER TABLE [dbo].[MenuDetails] CHECK CONSTRAINT [CK_MenuDetails]
    GO
    ```

5. Staff

    ```sql
    CREATE TABLE [dbo].[Staff] (
        [EmployeeID] [int] IDENTITY(1, 1) NOT NULL,
        [Firstname] [varchar](50) NOT NULL,
        [Lastname] [varchar](50) NOT NULL,
        [Address] [varchar](200) NOT NULL,
        [DateOfBirth] [date] NOT NULL,
        [Role] [varchar](50) NOT NULL,
    CONSTRAINT [PK_Staff] PRIMARY KEY CLUSTERED 
    (
        [EmployeeID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO
    ```

6. Tables

    ```sql
    CREATE TABLE [dbo].[Tables] (
        [TableID] [int] IDENTITY(1, 1) NULL,
        [ChairCount] [int] NOT NULL,
        [Discontinued] [bit] NOT NULL,
    CONSTRAINT [PK_Tables] PRIMARY KEY CLUSTERED 
    (
        [TableID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO

    ALTER TABLE [dbo].[Tables] ADD  CONSTRAINT [DF_Tables_Discontinued]  DEFAULT ((0)) FOR [Discontinued]
    GO

    ALTER TABLE [dbo].[Tables]  WITH CHECK ADD  CONSTRAINT [CK_Tables] CHECK  (([ChairCount]>(0)))
    GO

    ALTER TABLE [dbo].[Tables] CHECK CONSTRAINT [CK_Tables]
    GO
    ```

7. Customers

    ```sql
    CREATE TABLE [dbo].[Customers] (
        [CustomerID] [int] IDENTITY(1,1) NOT NULL,
        [Name] [varchar](50) NULL,
        [Lastname] [varchar](50) NULL,
        [NIP] [varchar](20) NULL,
        [Address] [varchar](200) NULL,
        [PhoneNumber] [varchar](24) NULL,
        [IsCompany] [bit] NOT NULL,
    CONSTRAINT [PK_Customers] PRIMARY KEY CLUSTERED 
    (
        [CustomerID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO
    ```

8. Bills

    ```sql
    CREATE TABLE [dbo].[Bills] (
        [BillID] [int] IDENTITY(1,1) NOT NULL,
        [DateOfIssue] [datetime] NOT NULL,
        [Refund] [bit] NOT NULL,
    CONSTRAINT [PK_TMPBills] PRIMARY KEY CLUSTERED 
    (
        [BillID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO
    ```

9. Orders

    ```sql
    CREATE TABLE [dbo].[Orders] (
       [OrderID] [int] IDENTITY(1,1) NOT NULL,
       [CustomerID] [int] NOT NULL,
       [WaiterID] [int] NOT NULL,
       [OrderDate] [datetime] NOT NULL,
       [ServedDate] [datetime] NULL,
       [TakeAway] [bit] NOT NULL,
       [Canceled] [bit] NOT NULL,
       [ExpectedDate] [datetime] NOT NULL,
       [IsOnline] [bit] NOT NULL,
       [BillID] [int] NULL,
    CONSTRAINT [PK_Orders] PRIMARY KEY CLUSTERED 
    (
        [OrderID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO

    ALTER TABLE [dbo].[Orders] ADD  CONSTRAINT [DF_Orders_OrderDate]  DEFAULT (getdate()) FOR [OrderDate]
    GO

    ALTER TABLE [dbo].[Orders] ADD  CONSTRAINT [DF_Orders_TakeAway]  DEFAULT ((0)) FOR [TakeAway]
    GO

    ALTER TABLE [dbo].[Orders] ADD  CONSTRAINT [DF_Orders_Canceled]  DEFAULT ((0)) FOR [Canceled]
    GO

    ALTER TABLE [dbo].[Orders] ADD  CONSTRAINT [DF_Orders_ExpectedDate]  DEFAULT (dateadd(hour,(1),getdate())) FOR [ExpectedDate]
    GO

    ALTER TABLE [dbo].[Orders] ADD  CONSTRAINT [DF_Orders_IsOnline]  DEFAULT ((0)) FOR [IsOnline]
    GO

    ALTER TABLE [dbo].[Orders]  WITH CHECK ADD  CONSTRAINT [FK_Orders_Bills] FOREIGN KEY([BillID])
    REFERENCES [dbo].[Bills] ([BillID])
    GO

    ALTER TABLE [dbo].[Orders] CHECK CONSTRAINT [FK_Orders_Bills]
    GO

    ALTER TABLE [dbo].[Orders]  WITH CHECK ADD  CONSTRAINT [FK_Orders_Customers] FOREIGN KEY([CustomerID])
    REFERENCES [dbo].[Customers] ([CustomerID])
    GO

    ALTER TABLE [dbo].[Orders] CHECK CONSTRAINT [FK_Orders_Customers]
    GO

    ALTER TABLE [dbo].[Orders]  WITH CHECK ADD  CONSTRAINT [FK_Orders_Staff] FOREIGN KEY([WaiterID])
    REFERENCES [dbo].[Staff] ([EmployeeID])
    GO

    ALTER TABLE [dbo].[Orders] CHECK CONSTRAINT [FK_Orders_Staff]
    GO

    ALTER TABLE [dbo].[Orders]  WITH CHECK ADD  CONSTRAINT [CK_Orders] CHECK  (([ServedDate]>[OrderDate] OR [ServedDate] IS NULL))
    GO

    ALTER TABLE [dbo].[Orders] CHECK CONSTRAINT [CK_Orders]
    GO

    ALTER TABLE [dbo].[Orders]  WITH CHECK ADD  CONSTRAINT [CK_Orders_1] CHECK  (([CustomerID]>(0) AND [WaiterID]>(0)))
    GO

    ALTER TABLE [dbo].[Orders] CHECK CONSTRAINT [CK_Orders_1]
    GO
    ```

10. TablesReservations

    ```sql
    CREATE TABLE [dbo].[TablesReservations] (
        [OrderID] [int] NOT NULL,
        [TimeSpan] [int] NOT NULL,
    CONSTRAINT [PK_TablesReservation_1] PRIMARY KEY CLUSTERED 
    (
        [OrderID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO

    ALTER TABLE [dbo].[TablesReservations] ADD  CONSTRAINT [DF_TablesReservations_TimeSpan]  DEFAULT ((240)) FOR [TimeSpan]
    GO

    ALTER TABLE [dbo].[TablesReservations]  WITH CHECK ADD  CONSTRAINT [FK_TablesReservation_Orders1] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[Orders] ([OrderID])
    GO

    ALTER TABLE [dbo].[TablesReservations] CHECK CONSTRAINT [FK_TablesReservation_Orders1]
    GO

    ALTER TABLE [dbo].[TablesReservations]  WITH CHECK ADD  CONSTRAINT [CK_TablesReservations] CHECK  (([TimeSpan]>(0)))
    GO

    ALTER TABLE [dbo].[TablesReservations] CHECK CONSTRAINT [CK_TablesReservations]
    GO
    ```

11. ReservationDetails

    ```sql
    CREATE TABLE [dbo].[ReservationDetails] (
        [OrderID] [int] NOT NULL,
        [TableID] [int] NOT NULL,
        [Canceled] [bit] NOT NULL,
    CONSTRAINT [PK_ReservationDetails] PRIMARY KEY CLUSTERED 
    (
        [OrderID] ASC,
        [TableID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO

    ALTER TABLE [dbo].[ReservationDetails] ADD  CONSTRAINT [DF_ReservationDetails_Canceled]  DEFAULT ((0)) FOR [Canceled]
    GO

    ALTER TABLE [dbo].[ReservationDetails]  WITH CHECK ADD  CONSTRAINT [FK_ReservationDetails_Tables] FOREIGN KEY([TableID])
    REFERENCES [dbo].[Tables] ([TableID])
    GO

    ALTER TABLE [dbo].[ReservationDetails] CHECK CONSTRAINT [FK_ReservationDetails_Tables]
    GO

    ALTER TABLE [dbo].[ReservationDetails]  WITH CHECK ADD  CONSTRAINT [FK_ReservationDetails_TablesReservation1] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[TablesReservations] ([OrderID])
    GO

    ALTER TABLE [dbo].[ReservationDetails] CHECK CONSTRAINT [FK_ReservationDetails_TablesReservation1]
    GO
    ```

12. OrderDetails

    ```sql
    CREATE TABLE [dbo].[OrderDetails] (
        [OrderID] [int] NOT NULL,
        [MealID] [int] NOT NULL,
        [MenuID] [int] NOT NULL,
        [Amount] [int] NOT NULL,
        [PaymentType] [varchar](20) NULL,
        [DateOfPayment] [smalldatetime] NULL,
    CONSTRAINT [PK_OrderDetails_1] PRIMARY KEY CLUSTERED 
    (
        [OrderID] ASC,
        [MealID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO

    ALTER TABLE [dbo].[OrderDetails]  WITH CHECK ADD  CONSTRAINT [FK_OrderDetails_MenuDetails] FOREIGN KEY([MenuID], [MealID])
    REFERENCES [dbo].[MenuDetails] ([MenuID], [MealID])
    GO

    ALTER TABLE [dbo].[OrderDetails] CHECK CONSTRAINT [FK_OrderDetails_MenuDetails]
    GO

    ALTER TABLE [dbo].[OrderDetails]  WITH CHECK ADD  CONSTRAINT [FK_OrderDetails_Orders1] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[Orders] ([OrderID])
    GO

    ALTER TABLE [dbo].[OrderDetails] CHECK CONSTRAINT [FK_OrderDetails_Orders1]
    GO

    ALTER TABLE [dbo].[OrderDetails]  WITH CHECK ADD  CONSTRAINT [CK_OrderDetails] CHECK  (([amount]>(0)))
    GO

    ALTER TABLE [dbo].[OrderDetails] CHECK CONSTRAINT [CK_OrderDetails]
    GO
    ```

13. NamesFromCompanies

    ```sql
    CREATE TABLE [dbo].[NamesFromCompanies] (
        [NFC] [int] IDENTITY(1,1) NOT NULL,
        [OrderID] [int] NULL,
        [FirstName] [varchar](50) NULL,
        [LastName] [varchar](50) NULL,
    CONSTRAINT [PK_NamesFromCompanies] PRIMARY KEY CLUSTERED 
    (
        [NFC] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO

    ALTER TABLE [dbo].[NamesFromCompanies]  WITH CHECK ADD  CONSTRAINT [FK_NamesFromCompanies_Orders] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[Orders] ([OrderID])
    GO

    ALTER TABLE [dbo].[NamesFromCompanies] CHECK CONSTRAINT [FK_NamesFromCompanies_Orders]
    GO

    ALTER TABLE [dbo].[NamesFromCompanies]  WITH CHECK ADD  CONSTRAINT [CK_NamesFromCompanies] CHECK  (([OrderID]>(0)))
    GO

    ALTER TABLE [dbo].[NamesFromCompanies] CHECK CONSTRAINT [CK_NamesFromCompanies]
    GO
    ```

14. PaymentTypes

    ```sql
    CREATE TABLE [dbo].[PaymentTypes] (
        [PaymentTypeID] [int] IDENTITY(1,1) NOT NULL,
        [PaymentName] [varchar](20) NOT NULL,
    CONSTRAINT [PK_PaymentTypes] PRIMARY KEY CLUSTERED 
    (
        [PaymentTypeID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO
    ```

15. PaymentDetails

    ```sql
    CREATE TABLE [dbo].[PaymentDetails] (
        [PaymentID] [int] IDENTITY(1,1) NOT NULL,
        [OrderID] [int] NOT NULL,
        [PaymentTypeID] [int] NOT NULL,
        [Sum] [money] NOT NULL,
        [DateOfPayment] [datetime] NOT NULL,
        [Refund] [bit] NOT NULL,
    CONSTRAINT [PK_PaymentDetails] PRIMARY KEY CLUSTERED 
    (
        [PaymentID] ASC
    ) WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
    ) ON [PRIMARY]
    GO

    ALTER TABLE [dbo].[PaymentDetails]  WITH CHECK ADD  CONSTRAINT [FK_PaymentDetails_Orders] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[Orders] ([OrderID])
    GO

    ALTER TABLE [dbo].[PaymentDetails] CHECK CONSTRAINT [FK_PaymentDetails_Orders]
    GO

    ALTER TABLE [dbo].[PaymentDetails]  WITH CHECK ADD  CONSTRAINT [FK_PaymentDetails_PaymentTypes] FOREIGN KEY([PaymentTypeID])
    REFERENCES [dbo].[PaymentTypes] ([PaymentTypeID])
    GO

    ALTER TABLE [dbo].[PaymentDetails] CHECK CONSTRAINT [FK_PaymentDetails_PaymentTypes]
    GO
    ```

## Zapełnianie bazy danymi

Wszelkie dane, które dodaliśmy do bazy były pierwotnie dodawanie ręczne bez procedur tak, aby same procedury póżniej testować i analizować. Końcowy raport zawiera dane, które zostały utworzone dzięki procedurom, poza *stałymi danymi*, które przez długi czas pozostają niezmienne, takie jak np. *Categories* czy *PaymentType*.

## Widoki

Lista widoków:

- ViewAllClientsAllOrdersValue
- ViewIndividualCLients
- ViewBuisnessClients
- ViewEachOrderValue
- ViewAllOrdersValue
- ViewAllOnlineReservationsDetails
- ViewAllCanceledReservationsDetails
- ViewAllMenus
- ViewCurrentMenu

### ViewAllClientsAllOrdersValue

```sql
CREATE VIEW ViewAllClientsAllOrdersValue
AS
SELECT c.CustomerID, c.Name, c.Lastname, c.NIP, SUM(od.Amount * md.Price) AS AllOrdersValue
FROM Customers c
JOIN Orders o ON o.CustomerID = c.CustomerID
JOIN OrderDetails od ON od.OrderID = o.OrderID
JOIN Meals m on m.MealId = od.MealID
JOIN MenuDetails md ON md.MealID = m.MealID
GROUP BY c.CustomerID, c.Name, c.Lastname, c.NIP
```

### ViewIndividualCLients

```sql
CREATE VIEW ViewIndividualCLients
AS
SELECT *
FROM Customers
WHERE IsCompany = 0
```

### ViewBuisnessClients

```sql
CREATE VIEW ViewBuisnessClients
AS
SELECT *
FROM Customers
WHERE IsCompany = 1
```

### ViewEachOrderValue

```sql
CREATE VIEW ViewEachOrderValue
AS
SELECT od.OrderID, o.OrderDate, o.ServedDate, o.ExpectedDate, SUM(od.Amount * md.Price) AS OrderTotalValue
FROM OrderDetails od
JOIN Meals m ON m.MealID = od.MealID
JOIN MenuDetails md ON md.MealID = m.MealID
JOIN Orders o ON o.OrderID = od.OrderID
GROUP BY od.OrderID, o.OrderDate, o.ServedDate, o.ExpectedDate
```

### ViewAllOrdersValue

```sql
CREATE VIEW ViewAllOrdersValue
AS
SELECT SUM(od.Amount * md.Price) AS OrderTotalValue
FROM OrderDetails od
JOIN Meals m ON m.MealID = od.MealID
JOIN MenuDetails md ON md.MealID = m.MealID
```

### ViewAllOnlineReservationsDetails

```sql
CREATE VIEW ViewAllOnlineReservationsDetails
AS
SELECT tr.OrderId, o.OrderDate, o.ServedDate, o.ExpectedDate, o.IsOnline, tr.TimeSpan, rd.Canceled, t.ChairCount, t.Discontinued
FROM TablesReservations tr
JOIN ReservationDetails rd ON rd.OrderID = tr.OrderID
JOIN Tables t ON t.TableID = rd.TableID
JOIN Orders o ON o.OrderID = tr.OrderID AND o.IsOnline = 1
WHERE rd.Canceled = 0
```

### ViewAllCanceledReservationsDetails

```sql
CREATE VIEW ViewAllCanceledReservationsDetails
AS
SELECT tr.OrderId, t.TableID, o.OrderDate, o.ServedDate, o.ExpectedDate, tr.TimeSpan, rd.Canceled, t.ChairCount, t.Discontinued
FROM TablesReservations tr
JOIN ReservationDetails rd ON rd.OrderID = tr.OrderID
JOIN Tables t ON t.TableID = rd.TableID
JOIN Orders o ON o.OrderID = tr.OrderID
WHERE rd.Canceled = 1
```

### ViewAllMenus

```sql
CREATE VIEW ViewAllMenus
AS
SELECT m.*, md.MealID, ml.MealName, md.Price
FROM Menus m
JOIN MenuDetails md ON md.MenuID = m.MenuID
JOIN Meals ml ON ml.MealID = md.MealID
```

### ViewCurrentMenu

```sql
CREATE VIEW ViewCurrentMenus
AS
SELECT m.*, md.MealID, md.Price
FROM Menus m
JOIN MenuDetails md ON md.MenuID = m.MenuID
WHERE GETDATE() BETWEEN MenuStart AND MenuEnd
```

## Triggery

### MaximumOrdersPerWaiter

```sql

ALTER TRIGGER MaximumOrdersPerWaiter ON Orders
    AFTER INSERT, UPDATE
AS
BEGIN
    IF (
        SELECT COUNT(*) FROM Orders AS O
        INNER JOIN inserted AS i ON O.WaiterID = i.WaiterID
        WHERE O.WaiterID = i.WaiterID
            AND O.ExpectedDate BETWEEN DATEADD(hour, -1, GETDATE())
            AND GETDATE()
        ) > 5
    BEGIN
        RAISERROR('To many orders per one waiter', 16, 1)
        ROLLBACK
        RETURN
    END
END;
```

## Funkcje i Procedury

### CheckAvailableTables

```sql
CREATE FUNCTION CheckAvailableTables (
    @date DATETIME
)
RETURNS TABLE
AS
RETURN
    SELECT T.TableID FROM Tables AS T
    LEFT JOIN (
        SELECT DISTINCT RD.TableID FROM ReservationDetails AS RD
        JOIN TablesReservations AS TR
        ON TR.OrderID = RD.OrderID
        JOIN Orders AS O
        ON O.OrderID = TR.OrderID
        WHERE DATEADD(minute, TR.TimeSpan, @date) > O.ExpectedDate 
        ANd @date < DATEADD(minute, TR.TimeSpan, O.ExpectedDate) 
    ) AS OT ON T.TableID = OT.TableID
    WHERE OT.TableID IS NULL
```

### Reservation

```sql
CREATE PROCEDURE Reservation (
    @CustomerID INT,
    @WaiterID INT,
    -- @OrderDate AS GETDATE()
    -- @Canceled AS 0
    @ExpectedDate DATETIME,
    @MealIDString VARCHAR(MAX),
    @AmountString VARCHAR(MAX),

    @NameList VARCHAR(MAX),
    @SurnameList VARCHAR(MAX),

    @TableString VARCHAR(MAX),
    @PaymentTypeID INT,
    @MoneyAmount Money,
    @BillID INT,
    @MenuID INT
)
AS
BEGIN
    
    DECLARE @MealIIDTable TABLE (ID INT IDENTITY(1, 1), MealID INT)
    INSERT INTO @MealIIDTable
    SELECT * FROM STRING_SPLIT(@MealIDString, ',')

    DECLARE @AmountTable TABLE (ID INT IDENTITY(1, 1), AmountID INT)
    INSERT INTO @AmountTable
    SELECT * FROM STRING_SPLIT(@AmountString, ',')

    DECLARE @TablesID TABLE (ID INT IDENTITY(1, 1), TableID INT)
    INSERT INTO @TablesID
    SELECT * FROM STRING_SPLIT(@TableString, ',')


    IF (SELECT COUNT(MealID) FROM @MealIIDTable) <> (SELECT COUNT(AmountID) FROM @AmountTable)
    BEGIN
        RAISERROR('The number of amounts and meals are not the same', 16, 10)
        RETURN
    END


    IF (
        SELECT COUNT(RD.TableID) FROM ReservationDetails AS RD
        JOIN TablesReservations AS TR
        ON TR.OrderID = RD.OrderID
        JOIN Orders AS O
        ON O.OrderID = TR.OrderID
        JOIN @TablesID AS TID
        ON TID.TableID = RD.TableID
        WHERE DATEADD(minute, TR.TimeSpan, @ExpectedDate) > O.ExpectedDate 
        ANd @ExpectedDate < DATEADD(minute, TR.TimeSpan, O.ExpectedDate)
        )
        > 0 OR  
        (
        SELECT COUNT(TID.TableID) FROM @TablesID AS TID
        JOIN Tables AS T
        ON T.TableID = TID.TableID
        ) = 0
    BEGIN
        RAISERROR('Some tables have been already reserved or the tables do not exist', 16, 11)
        RETURN
    END
    

    IF EXISTS (
        SELECT MID.MealID FROm @MealIIDTable AS MID
        LEFT JOIN (SELECT M.MealID FROM MenuDetails AS MD
            JOIN Meals AS M 
            ON M.MealID = MD.MealID) AS NM
        ON NM.MealID = MID.MealID
        WHERE NM.MealID IS NULL
    )
    BEGIN
        RAISERROR('Some meals does not exist in menu', 16, 12)
        RETURN
    END


    IF (
        SELECT COUNT(*) FROM @MealIIDTable AS MID
        JOIN Meals AS M ON M.MealID = MID.MealID
        JOIN Categories AS C ON C.CategoryID = M.CategoryID
        WHERE C.CategoryID = 1  
        ) > 0
    BEGIN
        IF (
            SELECT DATEPART(WEEKDAY, @ExpectedDate)
        ) NOT IN (5,6,7)
        BEGIN
            RAISERROR('The Fishes can be served only in Thursday, Friday or Saturday', 16, 13)
            RETURN
        END

        DECLARE @MaxDate DATETIME
        SET @MaxDate = DATEADD(DAY, -DATEDIFF(DAY, '19000101', @ExpectedDate) % 7, @ExpectedDate);

        IF (@MaxDate <= GETDATE())
        BEGIN 
            RAISERROR('You can reserve up to Monday before reservation date', 16, 14)
            RETURN
        END
    END

    DECLARE @WZ MONEY, @WK INT
    SET @WZ = 50
    SET @WK = 5 

    DECLARE @Cost MONEY
    SET @Cost = (
            SELECT SUM(MD.Price * A.AmountID) FROM @MealIIDTable AS MID
            JOIN @AmountTable AS A
            ON A.ID = MID.ID
            JOIN MenuDetails AS MD
            ON MD.MealID = MID.MealID
            WHERE MD.MenuID = @MenuID
            )

    IF @CustomerID IN (SELECT CustomerID FROM Customers WHERE IsCompany = 1) AND @MoneyAmount > @Cost 
        OR @CustomerID IN (SELECT CustomerID FROM Customers WHERE IsCompany = 0) AND @MoneyAmount <> @Cost
    BEGIN
        RAISERROR('Incorrect amount of payment', 16, 17)
        RETURN
    END

    IF (@CustomerID) IN 
        (
            SELECT CustomerID FROM Customers
            WHERE IsCompany = 0
        )
    BEGIN
        IF (
            SELECT COUNT(OrderID) FROM Orders
            WHERE CustomerID = @CustomerID
            ) < 0
        BEGIN
            RAISERROR('Customer has less than 5 orders', 16, 15)
            RETURN
        END

        IF @Cost < 50
        BEGIN
            RAISERROR('Not enough sum', 16, 16)
            RETURN
        END

    END

    DECLARE @LatestBill INT

    IF @BillID IS NULL
    BEGIN
        INSERT INTO Bills (Refund)
        VALUES (0)

        SET @LatestBill = (
            SELECT TOP 1 BillID FROM Bills
            ORDER BY BillID DESC
            )
    END
    ELSE
    BEGIN
        SET @LatestBill = @BillID

    END

    INSERT INTO Orders (CustomerID, WaiterID, OrderDate, TakeAway, Canceled, ExpectedDate, IsOnline, BillID)
    VALUES (@CustomerID, @WaiterID, GETDATE(), 0, 0, @ExpectedDate, 1, @LatestBill) 

    DECLARE @LatestOrder INT
    SET @LatestOrder = (
        SELECT TOP 1 OrderID
        FROM Orders
        ORDER BY OrderID DESC
        )


    INSERT INTO OrderDetails (OrderID, MealID, MenuID, Amount)
    SELECT @LatestOrder, MID.MealID, @MenuID, AMT.AmountID FROM @MealIIDTable AS MID
    JOIN @AmountTable AS AMT
    ON AMT.ID = MID.ID

    
    IF @TableString IS NOT NULL
    BEGIN
        INSERT INTO TablesReservations (OrderID)
        VALUES (@LatestOrder)

        INSERT INTO ReservationDetails (OrderID, TableID, Canceled)
        SELECT @LatestOrder, TableID, 0 FROM @TablesID

    END


    DECLARE @NameTable TABLE (ID INT IDENTITY(1, 1), Name VARCHAR(20))
    INSERT INTO @NameTable
    SELECT * FROM STRING_SPLIT(@NameList, ',')

    DECLARE @SurnameTable TABLE (ID INT IDENTITY(1, 1), Surname VARCHAR(20))
    INSERT INTO @SurnameTable
    SELECT * FROM STRING_SPLIT(@SurnameList, ',')


    IF @NameList IS NOT NULL AND @SurnameList IS NOT NULL AND @CustomerID IN (SELECT CustomerID FROM Customers WHERE IsCompany = 1)
    BEGIN
        INSERT INTO NamesFromCompanies (OrderID, FirstName, LastName)
        SELECT @LatestOrder, NT.Name, ST.Surname FROM @NameTable AS NT
        JOIN @SurnameTable AS ST
        ON ST.ID = NT.ID
    END

    

    INSERT INTO PaymentDetails (OrderID, PaymentTypeID, [Sum], DateOfPayment, Refund)
    VALUES (@LatestOrder, @PaymentTypeID, @MoneyAmount, GETDATE(), 0)

END
```

### MakeOrderAtPlace

```sql
CREATE PROCEDURE MakeOrderAtPlace (
    @CustomerID INT,
    @WaiterID INT,
    -- @OrderDate AS GETDATE()
    -- @Canceled AS 0
    @MealIDString VARCHAR(MAX),
    @AmountString VARCHAR(MAX),
    
    
    @TableString VARCHAR(MAX),
    
    @PaymentTypeID INT,
    @MoneyAmount Money,
    @BillID INT,
    @MenuID INT
)
AS
BEGIN
    
    DECLARE @MealIIDTable TABLE (ID INT IDENTITY(1, 1), MealID INT)
    INSERT INTO @MealIIDTable
    SELECT * FROM STRING_SPLIT(@MealIDString, ',')

    DECLARE @AmountTable TABLE (ID INT IDENTITY(1, 1), AmountID INT)
    INSERT INTO @AmountTable
    SELECT * FROM STRING_SPLIT(@AmountString, ',')

    DECLARE @TablesID TABLE (ID INT IDENTITY(1, 1), TableID INT)
    INSERT INTO @TablesID
    SELECT * FROM STRING_SPLIT(@TableString, ',')

    IF (SELECT COUNT(MealID) FROM @MealIIDTable) <> (SELECT COUNT(AmountID) FROM @AmountTable)
    BEGIN
        RAISERROR('The number of amounts and meals are not the same', 16, 20)
        RETURN
    END

    IF (
        SELECT COUNT(RD.TableID) FROM ReservationDetails AS RD
        JOIN TablesReservations AS TR
        ON TR.OrderID = RD.OrderID
        JOIN Orders AS O
        ON O.OrderID = TR.OrderID
        JOIN @TablesID AS TID
        ON TID.TableID = RD.TableID
        WHERE DATEADD(minute, TR.TimeSpan, @ExpectedDate) > O.ExpectedDate 
        ANd @ExpectedDate < DATEADD(minute, TR.TimeSpan, O.ExpectedDate)
        )
        > 0 OR  
        (
        SELECT COUNT(TID.TableID) FROM @TablesID AS TID
        JOIN Tables AS T
        ON T.TableID = TID.TableID
        ) = 0
    BEGIN
        RAISERROR('Some tables have been already reserved or the tables do not exist', 16, 11)
        RETURN
    END


    IF EXISTS (
        SELECT MID.MealID FROm @MealIIDTable AS MID
        LEFT JOIN (SELECT M.MealID FROM MenuDetails AS MD
            JOIN Meals AS M 
            ON M.MealID = MD.MealID) AS NM
        ON NM.MealID = MID.MealID
        WHERE NM.MealID IS NULL
    )
    BEGIN
        RAISERROR('Some meals does not exist in menu', 16, 21)
        RETURN
    END

    
    IF (
        SELECT COUNT(*) FROM @MealIIDTable AS MID
        JOIN Meals AS M ON M.MealID = MID.MealID
        JOIN Categories AS C ON C.CategoryID = M.CategoryID
        WHERE C.CategoryID = 1  
        ) > 0
    BEGIN
        RAISERROR('Not serving fish on menu without reservation', 16, 22)
        RETURN
    END

    DECLARE @Cost MONEY
    SET @Cost = (
            SELECT SUM(MD.Price * A.AmountID) FROM @MealIIDTable AS MID
            JOIN @AmountTable AS A
            ON A.ID = MID.ID
            JOIN MenuDetails AS MD
            ON MD.MealID = MID.MealID
            WHERE MD.MenuID = @MenuID
            )

    IF @CustomerID IN (SELECT CustomerID FROM Customers WHERE IsCompany = 1) AND @MoneyAmount > @Cost 
        OR @CustomerID IN (SELECT CustomerID FROM Customers WHERE IsCompany = 0) AND @MoneyAmount <> @Cost
    BEGIN
        RAISERROR('Incorrect amount of payment', 16, 23)
        RETURN
    END

    DECLARE @LatestBill INT

    IF @BillID IS NULL
    BEGIN
        INSERT INTO Bills (Refund)
        VALUES (0)

        SET @LatestBill = (
            SELECT TOP 1 BillID FROM Bills
            ORDER BY BillID DESC
            )
    END
    ELSE
    BEGIN
        SET @LatestBill = @BillID

    END

    INSERT INTO Orders (CustomerID, WaiterID, BillID)
    VALUES (@CustomerID, @WaiterID, @LatestBill) 

    DECLARE @LatestOrder INT
    SET @LatestOrder = (
        SELECT TOP 1 OrderID
        FROM Orders
        ORDER BY OrderID DESC
        )


    INSERT INTO OrderDetails (OrderID, MealID, MenuID, Amount)
    SELECT @LatestOrder, MID.MealID, @MenuID, AMT.AmountID FROM @MealIIDTable AS MID
    JOIN @AmountTable AS AMT
    ON AMT.ID = MID.ID


   INSERT INTO PaymentDetails (OrderID, PaymentTypeID, [Sum], DateOfPayment, Refund)
   VALUES (@LatestOrder, @PaymentTypeID, @MoneyAmount, GETDATE(), 0)


    IF @TableString IS NOT NULL
    BEGIN
        INSERT INTO TablesReservations (OrderID)
        VALUES (@LatestOrder)

        INSERT INTO ReservationDetails (OrderID, TableID, Canceled)
        SELECT @LatestOrder, TableID, 0 FROM @TablesID
    END

END
```

### MakeOrderTakeAway

```sql
CREATE PROCEDURE MakeOrderTakeAway (
    @CustomerID INT,
    @WaiterID INT,
    -- @OrderDate AS GETDATE()
    -- @Canceled AS 0
    @ExpectedDate DATETIME,
    @MealIDString VARCHAR(MAX),
    @AmountString VARCHAR(MAX),
    @IsOnline BIT,

    @PaymentTypeID INT,
    @MoneyAmount Money,
    @BillID INT,
    @MenuID INT
)
AS
BEGIN
    
    DECLARE @MealIIDTable TABLE (ID INT IDENTITY(1, 1), MealID INT)
    INSERT INTO @MealIIDTable
    SELECT * FROM STRING_SPLIT(@MealIDString, ',')

    DECLARE @AmountTable TABLE (ID INT IDENTITY(1, 1), AmountID INT)
    INSERT INTO @AmountTable
    SELECT * FROM STRING_SPLIT(@AmountString, ',')

    
    IF (SELECT COUNT(MealID) FROM @MealIIDTable) <> (SELECT COUNT(AmountID) FROM @AmountTable)
    BEGIN
        RAISERROR('The number of amounts and meals are not the same', 16, 10)
        RETURN
    END


    IF EXISTS (
        SELECT MID.MealID FROm @MealIIDTable AS MID
        LEFT JOIN (SELECT M.MealID FROM MenuDetails AS MD
            JOIN Meals AS M 
            ON M.MealID = MD.MealID) AS NM
        ON NM.MealID = MID.MealID
        WHERE NM.MealID IS NULL
    )
    BEGIN
        RAISERROR('Some meals does not exist in menu', 16, 12)
        RETURN
    END

   
    DECLARE @Cost MONEY
    SET @Cost = (
            SELECT SUM(MD.Price * A.AmountID) FROM @MealIIDTable AS MID
            JOIN @AmountTable AS A
            ON A.ID = MID.ID
            JOIN MenuDetails AS MD
            ON MD.MealID = MID.MealID
            WHERE MD.MenuID = @MenuID
            )

    IF @CustomerID IN (SELECT CustomerID FROM Customers WHERE IsCompany = 1) AND @MoneyAmount > @Cost 
        OR @CustomerID IN (SELECT CustomerID FROM Customers WHERE IsCompany = 0) AND @MoneyAmount <> @Cost
    BEGIN
        RAISERROR('Incorrect amount of payment', 16, 17)
        RETURN
    END


    DECLARE @LatestBill INT

    IF @BillID IS NULL
    BEGIN
        INSERT INTO Bills (Refund)
        VALUES (0)

        SET @LatestBill = (
            SELECT TOP 1 BillID FROM Bills
            ORDER BY BillID DESC
            )
    END
    ELSE
    BEGIN
        SET @LatestBill = @BillID

    END


    IF @IsOnline = 1
    BEGIN
        INSERT INTO Orders (CustomerID, WaiterID, TakeAway, ExpectedDate, IsOnline, BillID)
        VALUES (@CustomerID, @WaiterID, 1, @ExpectedDate, @IsOnline, @LatestBill) 
    END
    ELSE
    BEGIN
        INSERT INTO Orders (CustomerID, WaiterID, TakeAway, IsOnline, BillID)
        VALUES (@CustomerID, @WaiterID, 1, @IsOnline, @LatestBill) 
    END


    DECLARE @LatestOrder INT
    SET @LatestOrder = (
        SELECT TOP 1 OrderID
        FROM Orders
        ORDER BY OrderID DESC
        )


    INSERT INTO OrderDetails (OrderID, MealID, MenuID, Amount)
    SELECT @LatestOrder, MID.MealID, @MenuID, AMT.AmountID FROM @MealIIDTable AS MID
    JOIN @AmountTable AS AMT
    ON AMT.ID = MID.ID


    INSERT INTO PaymentDetails (OrderID, PaymentTypeID, [Sum], DateOfPayment, Refund)
    VALUES (@LatestOrder, @PaymentTypeID, @MoneyAmount, GETDATE(), 0)

END
```

### CancelOrder

```sql
CREATE PROCEDURE CancelOrder (
    @OrderID INT
)
AS
BEGIN
    UPDATE Orders
    SET Canceled = 1
    WHERE OrderID = @OrderID

    UPDATE ReservationDetails
    SET Canceled = 1
    WHERE OrderID = @OrderID

    UPDATE PaymentDetails
    SET Refund = 1
    WHERE OrderID = @OrderID
END
```

### AddToOrder

```sql
CREATE PROCEDURE AddToOrder (
    @OrderID INT,
    @MealIDString VARCHAR(MAX),
    @AmountString VARCHAR(MAX),
    @MenuID INT
)
AS
BEGIN
    DECLARE @MealIIDTable TABLE (ID INT IDENTITY(1, 1), MealID INT)
    INSERT INTO @MealIIDTable
    SELECT * FROM STRING_SPLIT(@MealIDString, ',')

    DECLARE @AmountTable TABLE (ID INT IDENTITY(1, 1), AmountID INT)
    INSERT INTO @AmountTable
    SELECT * FROM STRING_SPLIT(@AmountString, ',')

    
    IF (SELECT COUNT(MealID) FROM @MealIIDTable) <> (SELECT COUNT(AmountID) FROM @AmountTable)
    BEGIN
        RAISERROR('The number of amounts and meals are not the same', 16, 10)
        RETURN
    END


    IF EXISTS (
        SELECT MID.MealID FROm @MealIIDTable AS MID
        LEFT JOIN (SELECT M.MealID FROM MenuDetails AS MD
            JOIN Meals AS M 
            ON M.MealID = MD.MealID) AS NM
        ON NM.MealID = MID.MealID
        WHERE NM.MealID IS NULL
    )
    BEGIN
        RAISERROR('Some meals does not exist in menu', 16, 12)
        RETURN
    END

    IF @OrderId NOT IN (SELECT OrderID FROM Orders)
    BEGIN 
        RAISERROR('The order does not exist', 16, 1)
        RETURN
    END


    INSERT INTO OrderDetails (OrderID, MealID, MenuID, Amount)
    SELECT @OrderID, MID.MealID, @MenuID, AMT.AmountID FROM @MealIIDTable AS MID
    JOIN @AmountTable AS AMT
    ON AMT.ID = MID.ID
END
```

### RemoveFromOrder

```sql
CREATE PROCEDURE RemoveFromOrder (
    @OrderID INT,
    @MealID INT,
    @MenuID INT
)
AS
BEGIN
    IF @MealID NOT IN (SELECT MealID FROM OrderDetails WHERE OrderID = @OrderID AND MenuID = @MenuID)
    BEGIN
        RAISERROR('Meal not in order', 16, 1)
    END

    DELETE FROM OrderDetails
    WHERE OrderID = @OrderID AND MenuID = @MenuID
END
```

### MakeMenu

```sql
CREATE PROCEDURE MakeMenu (
    @StartDate DATE, 
    @EndDate DATE
)
AS
BEGIN
    INSERT INTO Menus (
        MenuStart, 
        MenuEnd,
        Discontinued
    )
    VALUES (
        @StartDate,
        @EndDate,
        1
    )
END
```

### AddNewMeal

```sql
CREATE PROCEDURE AddNewMeal (
    @MealName VARCHAR(50),
    @CategoryID INT
)
AS
BEGIN
    IF @MealName NOT IN (
        SELECT MealName
        FROM Meals
    )
    BEGIN
        INSERT INTO Meals (
            MealName, 
            CategoryID
        )
        VALUES (
            @MealName,
            @CategoryID
        )
    END
    ELSE
    BEGIN
        RAISERROR('There has been already the meal ', 16, 2)
    END
END
```

### InsertMealToMenu

```sql
CREATE PROCEDURE InsertMealToMenu (
    @MenuID INT,
    @MealID INT,
    @Price MONEY
)
AS
BEGIN
    IF @MenuID IN (
        SELECT MenuID
        FROM Menus
    ) AND @MealID NOT IN (
        SELECT MealID
        FROM MenuDetails
        WHERE MenuID = @MenuID
    )
    BEGIN
    INSERT INTO MenuDetails (
        MenuID,
        MealID,
        Price
    )
    VALUES (
        @MenuID,
        @MealID,
        @Price
    )
    END
    ELSE
    BEGIN
        RAISERROR('There is no such category or Meal already in Menu', 16, 2)
    END
END
```

### ConfirmMenuChange

```sql
CREATE PROCEDURE ConfirmMenuChange (
    @NewMenu INT
)
AS
BEGIN
    DECLARE @PreviousMenuID INT;
    SET @PreviousMenuID = (
        SELECT TOP 1 MenuID FROM Menus
        WHERE Discontinued = 0
        ORDER BY MenuEnd DESC)

    DECLARE @CountNew INT
    SET @CountNew = (SELECT COUNT(MealID) FROM MenuDetails
        WHERE MenuID = @NewMenu)

    DECLARE @CountCommon INT
    SET @CountCommon  = (
        SELECT COUNT(*) AS CC
        FROM (
            SELECT MealID
            FROM MenuDetails
            WHERE MenuID = @PreviousMenuID
            INTERSECT
            SELECT MealID
            FROM MenuDetails
            WHERE MenuID = @NewMenu
        ) AS CC
    )


    IF @CountCommon / @NewMenu > 0.5
    BEGIN
        RAISERROR('Half meals did not change', 16, 3)
        RETURN
    END

    UPDATE Menus
    SET Discontinued = 0
    WHERE MenuID = @NewMenu

END
```

### ChangeMealPrice

```sql
CREATE PROCEDURE ChangeMealPrice (
    @MenuID INT,
    @MealID INT,
    @Price INT
)
AS
BEGIN
    IF (
        SELECT COUNT(MenuID) FROM MenuDetails
        WHERE MenuID = @MenuID AND MealID = @MealID
        ) = 0
    BEGIN
        RAISERROR('Position does not exist', 16, 1)
    END

    UPDATE MenuDetails
    SET Price = @Price
    WHERE MenuID = @MenuID AND MealID = @MealID
END
```

### Payment

```sql
CREATE PROCEDURE Payment (
    @OrderID INT,
    @PaymentTypeID INT,
    @AmountMoney Money
)
AS 
BEGIN
    INSERT INTO PaymentDetails (OrderID, PaymentTypeID, [Sum], DateOfPayment, Refund)
    VALUES (@OrderID, @PaymentTypeID, @AmountMoney, GETDATE(), 0)
END
```

### IssueBill

```sql
CREATE PROCEDURE IssueBill (
    @BillID INT
)
AS
BEGIN
    IF (
        SELECT SUM(Sum) - SUM(Price * Amount)
        FROM OrderDetails od
        JOIN Orders o ON o.OrderID = od.OrderID
        JOIN PaymentDetails pd ON pd.OrderID = o.OrderID
        JOIN Meals ml ON ml.MealID = od.MealID
        JOIN MenuDetails md ON md.MealID = ml.MealID
        WHERE BillID = @BillID 
        AND od.MenuID = md.MenuID
    ) = 0
    BEGIN
        UPDATE Bills
        SET DateOfIssue = GETDATE()
    END

    ELSE
    BEGIN
        RAISERROR('You have to pay first', 16, 1)
    END
END
```

### PrintBill

```sql
CREATE FUNCTION PrintBill (
    @BillID INT
)
RETURNS TABLE
AS
RETURN (
    SELECT 
        b.BillID, 
        o.OrderID, 
        Name,
        Lastname,
        OrderDate, 
        DateOfIssue,
        DateOfPayment,
        TakeAway,
        IsOnline,
        Canceled,
        MealName,
        Amount,
        Price,
        PaymentName,
        [Total Price]

    FROM Bills b
    JOIN Orders o ON o.BillID = b.BillID
    JOIN OrderDetails od ON od.OrderID = o.OrderID
    JOIN Customers c ON c.CustomerID = o.CustomerID
    JOIN Meals ml ON ml.MealID = od.MealID
    JOIN MenuDetails md ON md.MealID = ml.MealID
    JOIN PaymentDetails pd ON pd.OrderID = o.OrderID
    JOIN PaymentTypes pt ON pt.PaymentTypeID = pd.PaymentTypeID
    JOIN (
        SELECT b.BillID, SUM(md.Price * Amount) AS [Total Price]
        FROM Bills b
        JOIN Orders o ON o.BillID = b.BillID
        JOIN OrderDetails od ON od.OrderID = o.OrderID
        JOIN MenuDetails md ON md.MealID = od.MealID
        WHERE Canceled = 0
        AND md.MenuID = od.MenuID
        GROUP BY b.BillID
    ) tp ON tp.BillID = b.BillID
    WHERE IsCompany = 0 
    AND b.BillID = @BillID
    AND md.MenuID = od.MenuID
)
```

### IssueInvoice

```sql
CREATE PROCEDURE IssueInvoice (
    @BillID INT
)
AS
BEGIN
    UPDATE Bills
    SET DateOfIssue = GETDATE()
END
```

### PrintInvoice

```sql
CREATE FUNCTION PrintInvoice (
    @CustomerID INT,
    @StartDate SMALLDATETIME,
    @EndDate SMALLDATETIME
)
RETURNS TABLE
AS
RETURN (
    SELECT 
        b.BillID, 
        o.OrderID, 
        Name,
        NIP,
        OrderDate, 
        DateOfIssue,
        DateOfPayment,
        TakeAway,
        IsOnline,
        Canceled,
        MealName,
        Amount,
        Price,
        PaymentName,
        [Total Price],
        [Total Price] - [Price Paid] AS [Price To Pay]

    FROM Bills b
    JOIN Orders o ON o.BillID = b.BillID
    JOIN PaymentDetails pd ON pd.OrderID = o.OrderID
    JOIN PaymentTypes pt oN pt.PaymentTypeID = pd.PaymentTypeID
    JOIN OrderDetails od ON od.OrderID = o.OrderID
    JOIN Meals ml ON ml.MealID = od.MealID
    JOIN MenuDetails md ON md.MealID = ml.MealID
    JOIN Customers c ON c.CustomerID = o.CustomerID

    JOIN (
        SELECT CustomerID, SUM(Price * Amount) [Total Price]
        FROM OrderDetails od
        JOIN Meals ml ON ml.MealID = od.MealID
        JOIN MenuDetails md ON md.MealID = ml.MealID
        JOIN Orders o ON o.OrderID = od.OrderID
        WHERE o.CustomerID = @CustomerID
        AND OrderDate BETWEEN @StartDate AND @EndDate
        AND md.MenuID = od.MenuID
        AND Canceled = 0
        GROUP BY CustomerID
    ) tp ON tp.CustomerID = o.CustomerID

    JOIN (
        SELECT CustomerID, SUM(Sum) AS [Price Paid]
        FROM PaymentDetails pd
        JOIN Orders o ON o.OrderID = pd.OrderID
        WHERE o.CustomerID = @CustomerID
        AND OrderDate BETWEEN @StartDate AND @EndDate
        AND Canceled = 0
        GROUP BY CustomerID
    ) pp ON pp.CustomerID = o.CustomerID

    WHERE c.CustomerID = @CustomerID
    AND md.MenuID = od.MenuID
)
```

## Raporty

### GeneratePrivateClientReport

```sql
ALTER FUNCTION [dbo].[GeneratePrivateClientReport] (
    @CustomerID INT,
    @StartingDate DATETIME,
    @EndingDate DATETIME   
)
RETURNS TABLE
AS
RETURN (
    SELECT o.CustomerID, 
        Name, 
        Lastname,
        OrderDate, 
        ExpectedDate, 
        ServedDate, 
        Canceled, 
        TakeAway, 
        IsOnline, 
        b.BillID, 
        DateOfIssue, 
        o.OrderID, 
        tr.TimeSpan AS [Reservation TimeSpan], 
        PaymentID, 
        DateOfPayment, 
        PaymentName, 
        Sum AS [Payment Value], 
        m.MenuID, 
        ml.MealID, 
        MealName, 
        CategoryName, 
        Amount, Price, 
        [Total Value].[Total Value]

    FROM Orders o
    JOIN Customers c ON c.CustomerID = o.CustomerID
    JOIN Bills b ON b.BillID = o.BillID
    JOIN PaymentDetails pd ON pd.OrderID = o.OrderID
    JOIN PaymentTypes pt ON pt.PaymentTypeID = pd.PaymentTypeID
    JOIN OrderDetails od ON od.OrderID = o.OrderID
    JOIN Meals ml ON ml.MealID = od.MealID
    JOIN MenuDetails md ON md.MealID = ml.MealID
    JOIN Menus m ON m.MenuID = md.MenuID
    JOIN Categories cat ON cat.CategoryID = ml.CategoryID
    JOIN TablesReservations tr ON tr.OrderID = o.OrderID
    JOIN (
        SELECT CustomerID, SUM(Price * Amount) [Total Value]
        FROM OrderDetails od
        JOIN Meals ml ON ml.MealID = od.MealID
        JOIN MenuDetails md ON md.MealID = ml.MealID
        JOIN Orders o ON o.OrderID = od.OrderID
        WHERE o.CustomerID = @CustomerID 
        AND OrderDate BETWEEN @StartingDate AND @EndingDate 
        GROUP BY CustomerID
    ) [Total Value] ON [Total Value].CustomerID = o.CustomerID
    WHERE o.CustomerID = @CustomerID 
    AND OrderDate BETWEEN @StartingDate AND @EndingDate 
    AND IsCompany = 0
)
```

### GeneratePrivateClientMonthlyReport

```sql
ALTER FUNCTION [dbo].[GeneratePrivateClientMonthlyReport] (
    @CustomerID INT,
    @StartingMonth DATETIME
)
RETURNS TABLE
AS
RETURN (
    SELECT o.CustomerID, 
        Name, 
        Lastname,
        OrderDate, 
        ExpectedDate, 
        ServedDate, 
        Canceled, 
        TakeAway, 
        IsOnline, 
        b.BillID, 
        DateOfIssue, 
        o.OrderID, 
        tr.TimeSpan AS [Reservation TimeSpan], 
        PaymentID, 
        DateOfPayment, 
        PaymentName, 
        Sum AS [Payment Value], 
        m.MenuID, 
        ml.MealID, 
        MealName, 
        CategoryName, 
        Amount, Price, 
        [Total Value].[Total Value]

    FROM Orders o
    JOIN Customers c ON c.CustomerID = o.CustomerID
    JOIN Bills b ON b.BillID = o.BillID
    JOIN PaymentDetails pd ON pd.OrderID = o.OrderID
    JOIN PaymentTypes pt ON pt.PaymentTypeID = pd.PaymentTypeID
    JOIN OrderDetails od ON od.OrderID = o.OrderID
    JOIN Meals ml ON ml.MealID = od.MealID
    JOIN MenuDetails md ON md.MealID = ml.MealID
    JOIN Menus m ON m.MenuID = md.MenuID
    JOIN Categories cat ON cat.CategoryID = ml.CategoryID
    JOIN TablesReservations tr ON tr.OrderID = o.OrderID
    JOIN (
        SELECT CustomerID, SUM(Price * Amount) [Total Value]
        FROM OrderDetails od
        JOIN Meals ml ON ml.MealID = od.MealID
        JOIN MenuDetails md ON md.MealID = ml.MealID
        JOIN Orders o ON o.OrderID = od.OrderID
        WHERE o.CustomerID = @CustomerID 
        AND MONTH(OrderDate) = MONTH(@StartingMonth)
        AND YEAR(OrderDate) = YEAR(@StartingMonth)
        GROUP BY CustomerID
    ) [Total Value] ON [Total Value].CustomerID = o.CustomerID
    WHERE o.CustomerID = @CustomerID 
    AND MONTH(OrderDate) = MONTH(@StartingMonth)
    AND YEAR(OrderDate) = YEAR(@StartingMonth)
    AND IsCompany = 0
)
```

### GenerateBuisnessClientReport

```sql
ALTER FUNCTION [dbo].[GenerateBuisnessClientReport] (
    @CustomerID INT,
    @StartingDate DATETIME,
    @EndingDate DATETIME   
)
RETURNS TABLE
AS
RETURN (
    SELECT o.CustomerID, 
        Name, 
        NIP,
        OrderDate, 
        ExpectedDate, 
        ServedDate, 
        Canceled, 
        TakeAway, 
        IsOnline, 
        b.BillID, 
        DateOfIssue, 
        o.OrderID, 
        tr.TimeSpan AS [Reservation TimeSpan], 
        PaymentID, 
        DateOfPayment, 
        PaymentName, 
        Sum AS [Payment Value], 
        m.MenuID, 
        ml.MealID, 
        MealName, 
        CategoryName, 
        Amount, Price, 
        tv.[Total Value],
        pp.[Price Paid],
        tv.[Total Value] - pp.[Price Paid] AS [Price to Pay]

    FROM Orders o
    JOIN Customers c ON c.CustomerID = o.CustomerID
    JOIN Bills b ON b.BillID = o.BillID
    JOIN PaymentDetails pd ON pd.OrderID = o.OrderID
    JOIN PaymentTypes pt ON pt.PaymentTypeID = pd.PaymentTypeID
    JOIN OrderDetails od ON od.OrderID = o.OrderID
    JOIN Meals ml ON ml.MealID = od.MealID
    JOIN MenuDetails md ON md.MealID = ml.MealID
    JOIN Menus m ON m.MenuID = md.MenuID
    JOIN Categories cat ON cat.CategoryID = ml.CategoryID
    JOIN TablesReservations tr ON tr.OrderID = o.OrderID
    JOIN (
        SELECT CustomerID, SUM(Price * Amount) [Total Value]
        FROM OrderDetails od
        JOIN Meals ml ON ml.MealID = od.MealID
        JOIN MenuDetails md ON md.MealID = ml.MealID
        JOIN Orders o ON o.OrderID = od.OrderID
        WHERE o.CustomerID = @CustomerID 
        AND OrderDate BETWEEN @StartingDate AND @EndingDate 
        GROUP BY CustomerID
    ) tv ON tv.CustomerID = o.CustomerID

    JOIN (
        SELECT CustomerID, SUM(Sum) AS [Price Paid]
        FROM PaymentDetails pd
        JOIN Orders o ON o.OrderID = pd.OrderID
        WHERE o.CustomerID = @CustomerID 
        AND OrderDate BETWEEN @StartingDate AND @EndingDate 
        GROUP BY CustomerID
    ) pp ON pp.CustomerID = o.CustomerID

    WHERE o.CustomerID = @CustomerID 
    AND OrderDate BETWEEN @StartingDate AND @EndingDate 
    AND IsCompany = 1
)
```

### GenerateBuisnessClientMonthlyReport

```sql
ALTER FUNCTION [dbo].[GenerateBuisnessClientMonthlyReport] (
    @CustomerID INT,
    @StartingMonth DATETIME
)
RETURNS TABLE
AS
RETURN (
    SELECT o.CustomerID, 
        Name, 
        NIP,
        OrderDate, 
        ExpectedDate, 
        ServedDate, 
        Canceled, 
        TakeAway, 
        IsOnline, 
        b.BillID, 
        DateOfIssue, 
        o.OrderID, 
        tr.TimeSpan AS [Reservation TimeSpan], 
        PaymentID, 
        DateOfPayment, 
        PaymentName, 
        Sum AS [Payment Value], 
        m.MenuID, 
        ml.MealID, 
        MealName, 
        CategoryName, 
        Amount, Price, 
        tv.[Total Value],
        pp.[Price Paid],
        tv.[Total Value] - pp.[Price Paid] AS [Price to Pay]

    FROM Orders o
    JOIN Customers c ON c.CustomerID = o.CustomerID
    JOIN Bills b ON b.BillID = o.BillID
    JOIN PaymentDetails pd ON pd.OrderID = o.OrderID
    JOIN PaymentTypes pt ON pt.PaymentTypeID = pd.PaymentTypeID
    JOIN OrderDetails od ON od.OrderID = o.OrderID
    JOIN Meals ml ON ml.MealID = od.MealID
    JOIN MenuDetails md ON md.MealID = ml.MealID
    JOIN Menus m ON m.MenuID = md.MenuID
    JOIN Categories cat ON cat.CategoryID = ml.CategoryID
    JOIN TablesReservations tr ON tr.OrderID = o.OrderID

    JOIN (
        SELECT CustomerID, SUM(Price * Amount) [Total Value]
        FROM OrderDetails od
        JOIN Meals ml ON ml.MealID = od.MealID
        JOIN MenuDetails md ON md.MealID = ml.MealID
        JOIN Orders o ON o.OrderID = od.OrderID
        WHERE o.CustomerID = @CustomerID 
        AND MONTH(OrderDate) = MONTH(@StartingMonth)
        AND YEAR(OrderDate) = YEAR(@StartingMonth)
        GROUP BY CustomerID
    ) tv ON tv.CustomerID = o.CustomerID

    JOIN (
        SELECT CustomerID, SUM(Sum) AS [Price Paid]
        FROM PaymentDetails pd
        JOIN Orders o ON o.OrderID = pd.OrderID
        WHERE o.CustomerID = @CustomerID 
        AND MONTH(OrderDate) = MONTH(@StartingMonth)
        AND YEAR(OrderDate) = YEAR(@StartingMonth) 
        GROUP BY CustomerID
    ) pp ON pp.CustomerID = o.CustomerID

    WHERE o.CustomerID = @CustomerID 
    AND MONTH(OrderDate) = MONTH(@StartingMonth)
    AND YEAR(OrderDate) = YEAR(@StartingMonth)
    AND IsCompany = 1
)
```

### GenerateAdminReport

```sql
CREATE FUNCTION GenerateAdminReport (
    @StartDate DATETIME,
    @EndDate DATETIME
)
RETURNS TABLE
AS RETURN (
    SELECT DISTINCT 
        [Total Orders], 
        [Total Customers], 
        [Total Private Clients Count], 
        [Total Buisness Clients Count], 
        [Total TakeAway Orders], 
        [Total Online Orders], 
        [Canceled Orders], 
        [Refunded Orders],
        [Refund Value],
        [Total Reservations], 
        [Canceled Reservations],
        [Total Orders Value],
        [Total Revenue],
        [Total Orders Value] - [Total Revenue] AS [Not Paid Orders]
    FROM Orders o
    CROSS JOIN (
        SELECT COUNT(DISTINCT OrderID) AS [Total Orders]
        FROM Orders
    ) AS OrderCount

    CROSS JOIN (
        SELECT COUNT(DISTINCT c.CustomerID) AS [Total Customers]
        FROM Customers c
        JOIN Orders o ON o.CustomerID = c.CustomerID
    ) AS CustomerCount

    CROSS JOIN (
        SELECT COUNT(DISTINCT o.OrderID) AS [Total Reservations]
        FROM Orders o
        JOIN TablesReservations tr ON tr.OrderID = o.OrderID
    ) AS ReservationCount

    CROSS JOIN (
        SELECT COUNT(Distinct o.OrderID) AS [Canceled Reservations]
        FROM Orders o
        JOIN TablesReservations tr ON tr.OrderID = o.OrderID
        WHERE Canceled = 1
    ) CanceledReservationsCount

    CROSS JOIN (
        SELECT COUNT(Distinct o.OrderID) AS [Canceled Orders]
        FROM Orders o
        WHERE Canceled = 1
    ) CanceledOrdersCount

    CROSS JOIN (
        SELECT COUNT(DISTINCT OrderID) AS [Total TakeAway Orders]
        FROM Orders
        WHERE TakeAway = 1
    ) AS TakeAwayCount

    CROSS JOIN (
        SELECT COUNT(DISTINCT OrderID) AS [Total Online Orders]
        FROM Orders
        WHERE IsOnline = 1
    ) AS OnlineCount

    CROSS JOIN (
        SELECT COUNT(DISTINCT c.CustomerID) AS [Total Buisness Clients Count]
        FROM Customers c
        JOIN Orders o ON o.CustomerID = c.CustomerID
        WHERE IsCompany = 1
    ) AS BuisnessCount

    CROSS JOIN (
        SELECT COUNT(DISTINCT c.CustomerID) AS [Total Private Clients Count]
        FROM Customers c
        JOIN Orders o ON o.CustomerID = c.CustomerID
        WHERE IsCompany = 0
    ) AS PrivateCount

    CROSS JOIN (
        SELECT ISNULL(SUM(Price * Amount), 0) AS [Total Orders Value]
        FROM OrderDetails od
        JOIN Meals ml ON ml.MealID = od.MealID
        JOIN MenuDetails md ON md.MealID = ml.MealID
        JOIN Orders o ON o.OrderID = od.OrderID
        JOIN Bills b ON b.BillID = o.BillID
        JOIN PaymentDetails pd ON pd.OrderID = o.OrderID
        WHERE od.MenuID = md.MenuID AND b.Refund = 0 AND pd.Refund = 0
    ) AS OrdersValue

    CROSS JOIN (
        SELECT ISNULL(SUM(CONVERT(money, Sum)), 0) AS [Total Revenue] 
        FROM PaymentDetails
        WHERE Refund = 0
    ) AS Revenue
    
    CROSS JOIN (
        SELECT COUNT(DISTINCT OrderID) AS [Refunded Orders]
        FROM Orders o
        JOIN Bills b ON b.BillID = o.BillID
        WHERE b.Refund = 1
    ) AS RefundedOrdersCount

    CROSS JOIN (
        SELECT ISNULL(SUM(CONVERT(money, Sum)), 0) AS [Refund Value]
        FROM PaymentDetails
        WHERE Refund = 1
    ) AS RefundValue

    WHERE OrderDate BETWEEN @StartDate AND @EndDate
)
```
