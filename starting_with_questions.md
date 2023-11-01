Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

SELECT a.country, SUM(a.productprice_cleaned * p.orderedquantity) as total_revenue
FROM all_sessions_clean as a
JOIN products as p
ON a.productsku = p.sku
WHERE a.country != '(not set)'
GROUP BY country
ORDER BY total_revenue DESC
LIMIT 3;

SELECT a.city, SUM(a.productprice_cleaned * p.orderedquantity) as total_revenue
FROM all_sessions_clean as a
JOIN products as p
ON a.productsku = p.sku
WHERE a.city != '(not set)' AND a.city != 'not available in demo dataset'
GROUP BY city
ORDER BY total_revenue DESC
LIMIT 1;


Answer:
Countries: 
1. United States: $126685107.36

Cities
1. Mountain View	$29929759.55




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
-- Countries
WITH country_with_quantity AS(
SELECT 
	a.country as country, 
	p.orderedquantity
FROM all_sessions_clean as a
JOIN products as p
ON a.productsku = p.sku)

SELECT 
	country, 
	ROUND(AVG(orderedquantity),2) as avg_products_ordered
FROM country_with_quantity
WHERE country != '(not set)'
GROUP BY country
ORDER BY avg_products_ordered DESC

-- Cities

WITH city_with_quantity AS(
SELECT 
	a.city as city, 
	p.orderedquantity
FROM all_sessions_clean as a
JOIN products as p
ON a.productsku = p.sku)

SELECT 
	city, 
	ROUND(AVG(orderedquantity),2) as avg_products_ordered
FROM city_with_quantity
WHERE city != '(not set)' AND city != 'not available in demo dataset'
GROUP BY city
ORDER BY avg_products_ordered DESC



Answer:
"Montenegro"	3786.00
"Mali"	3786.00
"Papua New Guinea"	2558.00
"Réunion"	2538.00
"Georgia"	2506.40
"Côte d’Ivoire"	1928.50
"Moldova"	1893.00
"Tanzania"	1429.00
"Trinidad & Tobago"	1379.50
"Armenia"	1351.00
"Oman"	1330.00
"Ethiopia"	1239.00
"Bulgaria"	1231.73
"Portugal"	1219.86
"Morocco"	1203.43
"Sint Maarten"	1100.00
"Puerto Rico"	1099.83
"Uganda"	1033.00
"Lebanon"	935.00
"Sudan"	935.00
"Chile"	930.84
"Laos"	886.67
"Zimbabwe"	844.00
"Dominican Republic"	829.55
"Italy"	758.98
"San Marino"	757.00
"Romania"	751.28
"Tunisia"	748.00
"Taiwan"	732.08
"Algeria"	720.50
"Macau"	701.67
"Honduras"	680.50
"Russia"	663.42
"Czechia"	662.41
"Kazakhstan"	617.00
"Israel"	605.41
"Guatemala"	603.22
"Serbia"	602.23
"Brazil"	593.07
"Panama"	588.67
"Mexico"	587.40
"Ireland"	586.66
"Denmark"	579.52
"Hong Kong"	577.80
"South Korea"	576.60
"Costa Rica"	573.40
"United Kingdom"	568.80
"Nepal"	568.00
"Japan"	558.14
"Thailand"	556.65
"Slovenia"	553.88
"Peru"	551.31
"Myanmar (Burma)"	543.67
"Pakistan"	522.25
"Australia"	520.92
"Ukraine"	519.67
"United Arab Emirates"	514.65
"United States"	514.14
"Spain"	510.88
"Slovakia"	510.56
"Albania"	510.33
"Macedonia (FYROM)"	505.00
"Bahamas"	498.00
"Singapore"	497.77
"Indonesia"	495.79
"Canada"	494.81
"Saudi Arabia"	492.50
"Germany"	492.42
"India"	481.58
"Lithuania"	479.93
"Philippines"	475.52
"Malta"	469.50
"Vietnam"	462.10
"Argentina"	460.03
"Croatia"	454.87
"Greece"	452.40
"New Zealand"	446.30
"Netherlands"	444.39
"Kosovo"	433.00
"Colombia"	432.60
"France"	420.52
"Switzerland"	406.57
"Poland"	391.72
"Malaysia"	390.88
"Latvia"	387.63
"Bosnia & Herzegovina"	386.67
"Norway"	385.57
"Uruguay"	367.64
"Cambodia"	362.20
"Turkey"	360.84
"Venezuela"	341.89
"Hungary"	340.00
"Egypt"	322.45
"Austria"	319.09
"Sweden"	317.56
"Finland"	313.77
"Sri Lanka"	307.67
"Jordan"	295.00
"Nigeria"	280.14
"South Africa"	279.44
"Nicaragua"	268.00
"Belgium"	249.25
"Bangladesh"	242.74
"Belarus"	231.00
"Bahrain"	226.33
"Kuwait"	221.17
"China"	219.32
"Botswana"	215.00
"Ghana"	197.00
"Jamaica"	178.00
"Palestine"	155.00
"Estonia"	151.20
"Iraq"	150.67
"Martinique"	138.50
"Ecuador"	136.73
"Kyrgyzstan"	123.00
"Mauritius"	104.50
"Bolivia"	90.00
"Gibraltar"	79.00
"Cyprus"	78.25
"Jersey"	59.00
"Somalia"	50.00
"Brunei"	45.50
"Qatar"	41.33
"Maldives"	33.00
"Barbados"	25.00
"El Salvador"	19.00
"Kenya"	16.50
"Paraguay"	15.00
"Luxembourg"	12.00
"Haiti"	1.00
"Rwanda"	0.00
"Iceland"	0.00

"Council Bluffs"	7589.00
"Bellflower"	3786.00
"Cork"	3786.00
"Santiago"	3607.00
"Bellingham"	2836.00
"Detroit"	2748.00
"Westville"	2299.00
"Santa Fe"	1932.50
"Nashville"	1886.00
"Brno"	1548.00
"Zhongli District"	1492.00
"Ghent"	1351.00
"Gurgaon"	1335.86
"Rome"	1310.40
"Mississauga"	1305.25
"Moscow"	1291.80
"Madrid"	1286.47
"Prague"	1216.50
"Belo Horizonte"	1184.00
"Sakai"	1148.00
"Riyadh"	1138.00
"Saint Petersburg"	1106.75
"San Bruno"	1106.43
"San Diego"	1093.52
"Milan"	1091.55
"Athens"	1088.40
"Perth"	1035.80
"Minato"	1033.70
"Wrexham"	1033.00
"Munich"	865.29
"Kirkland"	847.72
"Longtan District"	845.00
"Santa Clara"	826.83
"Sao Paulo"	826.68
"Sherbrooke"	812.67
"Dubai"	798.14
"Menlo Park"	791.00
"Indianapolis"	769.00
"Charlotte"	749.13
"Montreuil"	726.50
"Berlin"	708.77
"Palo Alto"	703.44
"Mexico City"	700.84
"Bucharest"	697.44
"Phoenix"	694.83
"Indore"	689.00
"Seoul"	670.55
"Redmond"	661.13
"Dublin"	661.06
"Singapore"	661.03
"Melbourne"	659.38
"Chicago"	647.29
"Cambridge"	627.13
"Hyderabad"	602.33
"Kolkata"	593.22
"Bangkok"	588.00
"Quebec City"	578.14
"Mountain View"	575.65
"Austin"	570.69
"Edmonton"	569.50
"Toronto"	566.81
"Shinjuku"	565.29
"Hong Kong"	564.67
"Vladivostok"	551.00
"Tel Aviv-Yafo"	547.76
"Pozuelo de Alarcon"	541.33
"Pittsburgh"	533.73
"The Dalles"	516.50
"San Francisco"	503.88
"Dallas"	503.28
"Bhubaneswar"	499.00
"Chennai"	497.20
"London"	492.80
"Sunnyvale"	491.94
"Bogota"	488.00
"New Delhi"	487.69
"Atlanta"	483.09
"Seattle"	478.03
"Calgary"	472.20
"Mumbai"	470.31
"San Jose"	468.22
"Cupertino"	466.70
"La Victoria"	465.25
"Warsaw"	463.52
"Bengaluru"	463.43
"Tempe"	447.25
"Salem"	446.28
"Oakland"	445.43
"Hamburg"	437.09
"Redwood City"	435.17
"Buenos Aires"	434.50
"Osaka"	434.33
"Rosario"	433.00
"New York"	431.50
"Karachi"	428.00
"Istanbul"	418.17
"Los Angeles"	403.94
"Kalamazoo"	403.00
"Fremont"	401.50
"Kiev"	400.56
"Zurich"	399.67
"Sydney"	396.09
"Monterrey"	393.00
"Pune"	386.56
"Kuala Lumpur"	384.50
"Houston"	380.25
"Irvine"	379.89
"Ho Chi Minh City"	378.33
"San Antonio"	368.86
"Ahmedabad"	364.64
"Ipoh"	362.00
"Boulder"	361.00
"Denver"	355.83
"Amsterdam"	355.64
"Taguig"	355.43
"Courbevoie"	348.00
"Jakarta"	347.78
"Hanoi"	347.00
"Paris"	345.00
"Timisoara"	344.00
"Jaipur"	339.50
"Avon"	330.50
"Pleasanton"	330.00
"Vancouver"	329.17
"Patna"	327.00
"Jacksonville"	326.50
"Kitchener"	321.40
"Philadelphia"	315.00
"Montreal"	313.89
"Boston"	313.32
"Rexburg"	308.67
"Lisbon"	299.00
"Sacramento"	299.00
"Montevideo"	292.50
"Ann Arbor"	290.56
"Santa Monica"	276.00
"Brisbane"	274.50
"Druid Hills"	269.00
"Wellesley"	269.00
"Oviedo"	260.00
"Barcelona"	241.53
"Villeneuve-d'Ascq"	230.00
"Vienna"	228.50
"Culiacan"	220.00
"Lahore"	218.00
"University Park"	215.00
"Lake Oswego"	215.00
"Stuttgart"	214.00
"Medellin"	208.00
"Burnaby"	200.67
"Washington"	191.11
"Ashburn"	187.50
"Richardson"	185.00
"Budapest"	184.17
"Oxford"	183.00
"Sandton"	183.00
"Glasgow"	183.00
"Rio de Janeiro"	179.67
"Stanford"	178.00
"Thessaloniki"	171.50
"Salford"	171.00
"Goose Creek"	168.00
"Tampa"	168.00
"Stockholm"	163.47
"South San Francisco"	159.50
"Columbia"	154.00
"Shibuya"	152.00
"Colombo"	138.67
"Hayward"	137.00
"South El Monte"	137.00
"Antwerp"	134.50
"Manila"	128.75
"Maracaibo"	119.50
"Norfolk"	102.00
"Orlando"	100.17
"Madison"	92.00
"Yokohama"	82.29
"Iasi"	79.00
"Poznan"	73.50
"Minneapolis"	70.00
"Quezon City"	66.83
"Milpitas"	66.38
"Appleton"	66.00
"East Lansing"	66.00
"Frankfurt"	65.00
"Chuo"	65.00
"Cluj-Napoca"	62.00
"Doha"	62.00
"Brussels"	62.00
"Copenhagen"	61.50
"Eau Claire"	59.64
"Kovrov"	58.00
"Izmir"	58.00
"Neipu Township"	58.00
"Zagreb"	54.00
"Adelaide"	54.00
"Petaling Jaya"	53.50
"Nanded"	53.00
"Bandung"	53.00
"Auckland"	52.00
"Makati"	51.50
"Kharkiv"	51.00
"Manchester"	50.50
"Egham"	50.00
"San Salvador"	49.00
"Kharagpur"	46.00
"Piscataway Township"	40.00
"Vilnius"	40.00
"Oslo"	40.00
"San Mateo"	36.00
"St. John's"	34.00
"Lucknow"	33.00
"Helsinki"	31.00
"Marseille"	31.00
"Las Vegas"	26.67
"Greer"	26.00
"Ottawa"	25.50
"Cape Town"	25.00
"Berkeley"	25.00
"Fortaleza"	22.00
"Chico"	22.00
"Coventry"	22.00
"Nagoya"	22.00
"Jersey City"	18.33
"Parsippany-Troy Hills"	18.00
"Beijing"	16.00
"Portland"	15.00
"Asuncion"	15.00
"Akron"	15.00
"Chandigarh"	15.00
"LaFayette"	15.00
"St. Louis"	13.50
"Bilbao"	13.50
"Sapporo"	12.00
"Columbus"	8.50
"Waterloo"	8.00
"Nairobi"	6.00
"Forest Park"	5.00
"Kansas City"	4.00
"El Paso"	2.00
"Chiyoda"	0.00
"Rotterdam"	0.00
"Bratislava"	0.00
"Westlake Village"	0.00
"Antalya"	0.00
"Watford"	0.00
"Marlboro"	0.00
"Fort Worth"	0.00
"Bordeaux"	0.00
"Panama City"	0.00


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
-- Countries: 
SELECT country, mainproductcategory_cleaned, count(all_sessions_clean.fullvisitorid_cleaned) as total_visitors
FROM all_sessions_clean
WHERE country != '(not set)'
GROUP BY mainproductcategory_cleaned, country
ORDER BY total_visitors DESC, country

-- City
SELECT city, mainproductcategory_cleaned, count(all_sessions_clean.fullvisitorid_cleaned) as total_visitors
FROM all_sessions_clean
WHERE city != '(not set)' AND city != 'not available in demo dataset'
GROUP BY mainproductcategory_cleaned, city
ORDER BY total_visitors DESC, city

Answer:
The top category for each country/city is Apparel. Shop by Brand and Electronics are the most common for locations with large visitor counts. Lower visitor count locations have their own unique trends likely due to the small sample sizes. 



**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
-- Countries
WITH productsalesbycountry AS(
SELECT 
	a.country as country,
	SUM(p.orderedquantity * a.productprice_cleaned) as toprevenue,
	a.productname_cleaned
FROM all_sessions_clean as a
JOIN products as p
ON a.productsku = p.sku
GROUP BY a.country, a.productname_cleaned)

, maxrevenues AS(
SELECT 
	country,
	MAX(toprevenue) as max_revenue
FROM productsalesbycountry
GROUP BY country)

SELECT 
	c.country, 
	c.max_revenue,
	d.productname_cleaned
FROM maxrevenues as c
JOIN productsalesbycountry as d
ON c.country = d.country AND c.max_revenue = d.toprevenue 

WITH productsalesbycity AS(
SELECT 
	a.city as city,
	SUM(p.orderedquantity * a.productprice_cleaned) as toprevenue,
	a.productname_cleaned
FROM all_sessions_clean as a
JOIN products as p
ON a.productsku = p.sku
GROUP BY a.city, a.productname_cleaned)

, maxrevenues AS(
SELECT 
	city,
	MAX(toprevenue) as max_revenue
FROM productsalesbycity
GROUP BY city)

SELECT 
	c.city, 
	c.max_revenue,
	d.productname_cleaned
FROM maxrevenues as c
JOIN productsalesbycity as d
ON c.city = d.city AND c.max_revenue = d.toprevenue 






Answer:

Outside of the US journals are one of the top selling products. With the US being the top country in terms of overall sales, cities within the US are purchasing the Cam Indoor Security Camera. 



**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

cities
SELECT 
	a.city as city,
	SUM(p.orderedquantity * a.productprice_cleaned) as revenue,
	CASE 
		WHEN p.stocklevel = 0 THEN 0
		ELSE CAST(p.restockingleadtime as numeric)/CAST(p.stocklevel as NUMERIC)
	END AS restocking_ratio
FROM all_sessions_clean as a
JOIN products as p
ON a.productsku = p.sku
GROUP BY a.city, restocking_ratio
HAVING SUM(p.orderedquantity * a.productprice_cleaned) > 0 and city != 'not available in demo dataset' AND city != '(not set)'
ORDER BY revenue DESC

country

SELECT 
	a.country as country,
	SUM(p.orderedquantity * a.productprice_cleaned) as revenue,
	CASE 
		WHEN p.stocklevel = 0 THEN 0
		ELSE CAST(p.restockingleadtime as numeric)/CAST(p.stocklevel as NUMERIC)
	END AS restocking_ratio
FROM all_sessions_clean as a
JOIN products as p
ON a.productsku = p.sku
GROUP BY a.country, restocking_ratio
HAVING SUM(p.orderedquantity * a.productprice_cleaned) > 0 and country != 'not available in demo dataset' AND country != '(not set)'
ORDER BY revenue DESC


Answer:
The locations with the highest revenue are taking a shorter time to restock likely because the company wants to prioritize their top locations. 






