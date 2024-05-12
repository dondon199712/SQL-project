<h1> World Popluation</h1>
<h1>Contents</h1>

<ul>
  <li><a href="#introduction">Introduction</a></li>
  <li><a href="#casestudyquestionsandsolutions">Case Study Questions & Solutions</a></li>


<h1><a name="introduction">Introduction</a></h1>
<p>You’ll work with a dataset of world population by country data from recent years. You’ll write queries to retrieve interesting data and answer a set of specific questions.


<h1><a name="casestudyquestionsandsolutions"></a>Case Study Questions & Solutions</h1>

<h4><a name="a.digitalanalysis"></a>A. Digital Analysis</h4>
<ol> 
  <li><h5>What is the largest population size for Gabon in this dataset? </h5></li>

  ```sql
SELECT MAX(population)
FROM population_years
WHERE country = 'Gabon';
```
  <h6>Answer:1.54526M</h6>

  
  <li><h5>

What were the 10 lowest population countries in 2005? </h5></li>

  ```sql
SELECT country
FROM population_years
WHERE year = 2005
ORDER BY population ASC
LIMIT 10;
```

   <h6>Answer:Niue
Falkland Islands (Islas Malvinas)
Montserrat
Saint Pierre and Miquelon
Saint Helena
Nauru
Cook Islands
Turks and Caicos Islands
Virgin Islands, British
Gibraltar</h6>


  <li><h5> What are all the distinct countries with a population of over 100 million in the year 2010?</h5></li>

  ```sql
SELECT DISTINCT country
FROM population_years
WHERE year = 2010 AND
population > 100;
```
   <h6>Answer:Mexico
United States
Brazil
Russia
Nigeria
Bangladesh
China
India
Indonesia
Japan
Pakistan</h6>



  <li><h5>How many countries in this dataset have the word “Islands” in their name</h5></li>

  ```sql
SELECT COUNT(DISTINCT (country))
FROM population_years
WHERE country like '%Islands%';;
```
   <h6>Answer:9</h6>



  <li><h5>What is the difference in population between 2000 and 2010 in Indonesia?</h5></li>

  ```sql
SELECT year, population 
FROM population_years
WHERE country = 'Indonesia'
AND year = 2000
OR country = 'Indonesia'
AND year = 2010;
```
   <h6>Answer:2000= 214.67661
2010= 242.96834
</h6>



