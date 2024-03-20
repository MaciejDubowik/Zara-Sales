# Analiza danych sprzedażowych sklepu ZARA


![image](https://imageio.forbes.com/specials-images/imageserve/6480707a48a543086e30c0ae/0x0.jpg?format=jpg&crop=3500,1968,x0,y171,safe&height=900&width=1600&fit=bounds)

## Opis Projektu
Ten projekt dotyczy analizy danych sprzedażowych za pomocą narzędzia Power BI oraz bazy danych MySQL. Dane pochodzą z zestawu danych zawierającego informacje o sprzedaży produktów, w tym identyfikatorach produktów, kategoriach, cenach, ilościach sprzedanych produktów i wielu innych.

Dane pozyskane są ze strony [kaggle.com](https://www.kaggle.com/datasets/xontoloyo/data-penjualan-zara/data). 

Wykorzystane narzędzia: PowerBI i MySQL

## Pytania Analizy Danych
- Jaka jest łączna sprzedaż oraz dochód?
- Jaka jest sprzedaż produktów w poszczególnych kategoriach?
- Czy produkty na promocji mają większą sprzedaż niż te, które nie są na promocji?
- Jakie są najczęstsze pozycje w układzie sklepu?
- Które produkty sprzedają się najlepiej?

## Etapy Analizy
**1. Ładowanie danych**

Dane zostały wczytane z pliku źródłowego w formacie .csv do bazy danych MySQL.

**2. Czyszczenie danych**

Usuwanie niepotrzebnych wierszy oraz kolumn oraz dostosowywanie projektu do późniejszej analizy

**3. Analiza Eksploracyjna**

Wykonanie podstawowych zapytań dla lepszego zrozumienia danych.

Przykładowe zapytania:

Łączna sprzedaż:
```
SELECT SUM(sales_volume) AS total_sales
FROM zara_sales.sales; 

```

Całkowity dochód:
```
SELECT SUM(sales_volume * price) as total_revenue
FROM zara_sales.sales; 
```

Sprzedaż produktów w poszczególnych kategoriach:
```
SELECT
	terms,
    SUM(sales_volume) AS total_sales
FROM zara_sales.sales
GROUP BY terms
ORDER BY total_sales DESC;
```

Porównanie produktów będących oraz nie będących objętych promocją:
```
SELECT
	promotion,
    SUM(sales_volume) AS total_sales
FROM zara_sales.sales
GROUP BY promotion;
```

Najczęstsze pozycje w układzie sklepu:
```
WITH total_products_categories AS (
	SELECT COUNT(product_id) AS total FROM zara_sales.sales
)
SELECT
		s.product_position,
		ROUND((COUNT(s.product_position)/t.total)*100, 2) as products_percentage
FROM 
	total_products_categories t, 
    zara_sales.sales s
GROUP BY s.product_position, t.total;
```
Analiza cen:
```
SELECT
	terms,
    MIN(price) AS lowest_price,
    MAX(price) AS highest_price,
    ROUND(AVG(price),2) AS average_price
FROM zara_sales.sales
GROUP BY terms
ORDER BY average_price DESC;
```

Bestsellery mężczyźni:
```
SELECT
	name,
    sales_volume
FROM zara_sales.sales
WHERE section = 'MAN'
ORDER BY sales_volume DESC
LIMIT 10;
```

**4. Wizualizacja**

Utworzenie interaktywnego dashboardu w programie PowerBI (zara_sales_dashboard.pbix).
![image](https://github.com/MaciejDubowik/Zara-Sales/assets/77201172/1670e2ff-8605-4b2b-9a6b-4aab1575e208)


## Załączniki
Plik z danymi początkowymi:
``` 
zara_sales.csv
```
Plik po przeprowadzeonej analizie:
```
zara_sales_dashboard.pbix
``` 


