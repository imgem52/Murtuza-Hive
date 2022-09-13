1. Download vechile sales data -> https://github.com/shashank-mishra219/Hive-Class/blob/main/sales_order_data.csv

2. Store raw data into hdfs location

3. Create a internal hive table "sales_order_csv" which will store csv data sales_order_csv .. make sure to skip header row while creating table

# create csv table for sales data

create table sales_csv
(
ORDERNUMBER int,
qtyordered int,
priceeach float,
orderlinenumber int,
sales float,
status string,
qtr_id int,
month_id int,
year_id int,
productline string,
msrp int,
productcode string,
phone string,
city string,
state string,
postalcode string,
country string,
territory string,
contactlastname string,
contactfirstname string,
dealsize string
)
row format delimited
fields terminated by ','
tblproperties("skip.header.line.count"="1")
; 

4. Load data from hdfs path into "sales_order_csv"

load local data inpath "file:///tmp/sales_order_data_csv" into table sales_csv;

5. Create an internal hive table which will store data in ORC format "sales_order_orc"

create table sales_orc
(
ORDERNUMBER int,
qtyordered int,
priceeach float,
orderlinenumber int,
sales float,
status string,
qtr_id int,
month_id int,
year_id int,
productline string,
msrp int,
productcode string,
phone string,
city string,
state string,
postalcode string,
country string,
territory string,
contactlastname string,
contactfirstname string,
dealsize string
stored as orc;

6. Load data from "sales_order_csv" into "sales_order_orc"

from sales_csv insert overwrite table sales_orc select *;

Perform below menioned queries on "sales_order_orc" table :

a. Calculate total sales per year

b. Find a product for which maximum orders were placed
input: Select productline, max(qtyordered) as Max_orders from sales_orc group by productline;
output : Classic Cars : 97

c. Calculate the total sales for each quarter
input: select qtr_id as QTR ,sum(sales) as total_sales from sales_orc group by qtr_id;
output: QTR total_sales
        1   2350817.726501465
        2   2048120.3029174805
        3   1758910.808959961
        4   3874780.010925293
        
d. In which quarter sales was minimum
input : select qtr_id as QTR ,min(sales) as Min_sales from sales_orc group by qtr_id;
output: QTR Min_sales
        1   683.8
        2   482.13
        3   577.6
        4   694.6
e. In which country sales was maximum and in which country sales was minimum
 input : select country, max(sales) from sales_orc group by country,
Australia       9774.03
Austria 9240.0
Belgium 6804.63
Canada  9064.89
Denmark 10468.9
Finland 10606.2
France  11739.7
Germany 8940.96
Ireland 8258.0
Italy   9160.36
Japan   10758.0
Norway  8844.12
Philippines     7483.98
Singapore       10993.5
Spain   12001.0
Sweden  7209.11
Switzerland     6761.6
UK      11886.6
USA     14082.8

Ans : Max Sales : USA     14082.8
Min Sales:Switzerland     6761.6
f. Calculate quartelry sales for each city
input: select qtr_id as QTR, sum(sales) as Total_sales ,city as City from sales_orc group by qtr_id, city;
1       56181.320068359375      Bergamo
1       31606.72021484375       Boras
1       31474.7802734375        Brickhaven
1       16118.479858398438      Brisbane
1       18800.089721679688      Bruxelles
1       37850.07958984375       Burbank
1       13529.570190429688      Burlingame
1       21782.699951171875      Cambridge
1       16628.16015625  Charleroi
1       26906.68017578125       Cowes
1       38784.470458984375      Dublin
1       51373.49072265625       Espoo
1       48698.82922363281       Frankfurt
1       50432.549560546875      Gensve
1       3987.199951171875       Glendale
1       8775.159912109375       Graz
1       26422.819458007812      Helsinki
1       58871.110107421875      Kobenhavn
1       20178.1298828125        Lille
1       8477.219970703125       London
1       23889.320068359375      Los Angeles
1       9748.999755859375       Lule
1       101339.13977050781      Lyon
1       357668.4899291992       Madrid
1       55245.02014160156       Makati City
1       51017.919860839844      Manchester
1       2317.43994140625        Marseille
1       49637.57067871094       Melbourne
1       38191.38977050781       Minato-ku
1       32647.809814453125      NYC
1       59617.39978027344       Nantes
1       12133.25        Nashua
1       48578.95935058594       New Bedford
1       8722.1201171875 Newark
1       65012.41955566406       North Sydney
1       50490.64013671875       Osaka
1       49055.40026855469       Oulu
1       71494.17944335938       Paris
1       44273.359436035156      Pasadena
1       27398.820434570312      Philadelphia
1       52029.07043457031       Reims
1       87489.23010253906       San Diego
1       72899.19995117188       San Francisco
1       267315.2586669922       San Rafael
1       28395.18994140625       Singapore
1       21730.029907226562      South Brisbane
1       54701.999755859375      Stavern
1       15139.1201171875        Toulouse
1       5759.419921875  Versailles
2       6166.7998046875 Allentown
2       4219.2001953125 Barcelona
2       74994.240234375 Boston
2       7277.35009765625        Brickhaven
2       75778.99060058594       Bridgewater
2       8411.949829101562       Bruxelles
2       14380.920043945312      Cambridge
2       1711.260009765625       Charleroi
2       43971.429931640625      Chatswood
2       31018.230102539062      Espoo
2       14378.089965820312      Glen Waverly
2       20350.949768066406      Glendale
2       62091.880615234375      Kobenhavn
2       33847.61975097656       Las Vegas
2       91211.0595703125        Liverpool
2       32376.29052734375       London
2       339588.0513305664       Madrid
2       52481.840087890625      Marseille
2       60135.84033203125       Melbourne
2       26482.700256347656      Minato-ku
2       58257.50012207031       Montreal
2       165100.33947753906      NYC
2       60344.990173339844      Nantes
2       36973.309814453125      New Haven
2       74506.06909179688       Newark
2       17114.43017578125       Osaka
2       17813.40008544922       Oulu
2       80215.4203491211        Paris
2       7287.240234375  Philadelphia
2       41509.94006347656       Reggio Emilia
2       18971.959716796875      Reims
2       98104.24005126953       Salzburg
2       160010.27026367188      San Jose
2       7261.75 San Rafael
2       92033.77014160156       Singapore
2       80438.47985839844       Strasbourg
2       31302.500244140625      Tsawassen
3       71930.61041259766       Allentown
3       16363.099975585938      Bergen
3       53941.68981933594       Boras
3       15344.640014648438      Boston
3       114974.53967285156      Brickhaven
3       34100.030029296875      Brisbane
3       47760.479736328125      Bruxelles
3       42031.83020019531       Burlingame
3       48828.71942138672       Cambridge
3       1637.199951171875       Charleroi
3       69694.40002441406       Chatswood
3       18971.959838867188      Dublin
3       31569.430053710938      Espoo
3       67281.00903320312       Gensve
3       12334.819580078125      Glen Waverly
3       7600.1201171875 Glendale
3       42744.0595703125        Helsinki
3       34453.84973144531       Las Vegas
3       69714.09008789062       Madrid
3       34993.92004394531       Munich
3       63027.92004394531       NYC
3       61310.880126953125      Nantes
3       45738.38952636719       New Bedford
3       47191.76013183594       North Sydney
3       34145.47021484375       Oslo
3       37501.580322265625      Oulu
3       27798.480102539062      Paris
3       55776.119873046875      Pasadena
3       56421.650390625 Reggio Emilia
3       15146.31982421875       Reims
3       6693.2802734375 Salzburg
3       216297.40063476562      San Rafael
3       90250.07995605469       Singapore
3       10640.290161132812      South Brisbane
3       94117.25988769531       Torino
3       17251.08056640625       Toulouse
3       43332.349609375 Tsawassen
4       100595.5498046875       Aaarhus
4       44040.729736328125      Allentown
4       74192.66003417969       Barcelona
4       81774.40008544922       Bergamo
4       95277.17993164062       Bergen
4       48710.92053222656       Boras
4       63730.7802734375        Boston
4       11528.52978515625       Brickhaven
4       26115.800537109375      Bridgewater
4       8234.559936523438       Burbank
4       65221.67004394531       Burlingame
4       54251.659912109375      Cambridge
4       13463.480224609375      Charleroi
4       37905.14990234375       Chatswood
4       51334.15966796875       Cowes
4       36472.76025390625       Frankfurt
4       37878.54992675781       Glen Waverly
4       34485.49987792969       Glendale
4       43488.740234375 Graz
4       42083.499755859375      Helsinki
4       24078.610107421875      Kobenhavn
4       100306.58020019531      Koln
4       14449.609741210938      Las Vegas
4       48874.28088378906       Lille
4       26797.210083007812      Liverpool
4       83970.029296875 London
4       24159.14013671875       Los Angeles
4       66005.8798828125        Lule
4       41535.11022949219       Lyon
4       315580.80963134766      Madrid
4       38770.71032714844       Makati City
4       106789.88977050781      Manchester
4       20136.859985351562      Marseille
4       91221.99914550781       Melbourne
4       55888.65026855469       Minato-ku
4       15947.290405273438      Montreal
4       300011.6999511719       NYC
4       23031.589599609375      Nantes
4       119552.04949951172      Nashua
4       113557.509765625        New Bedford
4       42498.760498046875      New Haven
4       41791.949462890625      North Sydney
4       45078.759765625 Oslo
4       89436.60034179688       Paris
4       4512.47998046875        Pasadena
4       116503.07043457031      Philadelphia
4       44669.740478515625      Reggio Emilia
4       48895.59014892578       Reims
4       45001.10986328125       Salzburg
4       151459.4805908203       San Francisco
4       163983.64880371094      San Rafael
4       54723.621154785156      Sevilla
4       77809.37023925781       Singapore
4       27098.800048828125      South Brisbane
4       61897.19006347656       Stavern
4       38098.240234375 Toulouse
4       75238.91955566406       Vancouver
4       59074.90026855469       Versailles
4       85555.98962402344       White Plains
h. Find a month for each year in which maximum number of quantities were sold
input:select month_id,year_id, max(qtyordered) from sales_orc group by month_id, year_id;
output:
1       2003    50
1       2004    50
1       2005    50
2       2003    50
2       2004    50
2       2005    50
3       2003    50
3       2004    50
3       2005    50
4       2003    50
4       2004    49
4       2005    97
5       2003    50
5       2004    50
5       2005    70
6       2003    50
6       2004    50
7       2003    49
7       2004    50
8       2003    49
8       2004    50
9       2003    50
9       2004    50
10      2003    50
10      2004    50
11      2003    50
11      2004    55
12      2003    49
12      2004    50
