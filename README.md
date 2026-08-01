# NETFLIX TV SHOWS AND MOVIES DATA ANALYSIS USING SQL(postgreSQL) 
![NETFLIX LOGO](https://github.com/MaheshRajpanke/SQL_NETFLIX-_PROJECT/blob/main/NETFLIX%20LOGO.png)

## Overview
This project analyzes the Netflix Movies & TV Shows dataset from Kaggle using **PostgreSQL**. The goal is to solve real-world business problems through SQL by exploring content distribution, ratings, genres, countries, directors, and other key insights.

---

## Objectives
- Analyze Netflix content using SQL.
- Perform data exploration and transformation.
- Solve business-oriented analytical questions.
- Demonstrate PostgreSQL skills on a real-world dataset.

---
## Dataset

- Link:[Kaggle Netflix Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows)
---

## SQL Concepts Used
- SELECT, WHERE, GROUP BY, ORDER BY, LIMIT
- Aggregate Functions (`COUNT()`, `MAX()`)
- String Functions (`STRING_TO_ARRAY()`, `UNNEST()`, `SPLIT_PART()`, `TRIM()`, `ILIKE()`)
- Date Functions (`TO_DATE()`, `CURRENT_DATE`, `INTERVAL`)
- Common Table Expressions (CTEs)
- Window Functions (`RANK()`)
- CASE Statements
- Type Casting (`::INT`, `::NUMERIC`)

---
## Schema
```sql
CREATE TABLE IF NOT EXISTS netflix (
    show_id VARCHAR(6),
    show_type VARCHAR(10),
    title VARCHAR(150),
    director VARCHAR(210),
    casts VARCHAR(1000),
    country VARCHAR(150),
    date_added VARCHAR(50),
    release_year INT,
    rating VARCHAR(10),
    duration VARCHAR(15),
    listed_in VARCHAR(100),
    description VARCHAR(300)
);
```
## Bussiness Problems
```sql


--1) Count the no.of Movies vs TV Shows

   Select 
         show_type,
          count(*) as total
   from netflix
   group by Show_type;
	

--2)Find the Most Common rating for movie and tv show

  with cte as (   
        select 
          show_type,
		  rating,
		 count(*) as total 
		from netflix
	    group by show_type,rating
	  ),
  ranked as(
	  select
	   show_type,
	   rating,
	   total,
	  rank() over 
	  ( partition by show_type
	    order by total desc) as rnk 
	  from cte)

    select * from ranked
	where rnk=1;
	

--3)List all the movies released in the specific year (eg:2004)

   Select title,
          release_year
      from netflix
	  where release_year = 2004;

--4)Find the top 5 countries with most content on newtflix

   select
      unnest(string_to_array(country,',')) as new_country,
	  count(show_id) as total_content
   From netflix
   group by 1 
   order by 2 desc limit 5;

--5)Identify the longest  movie 

    Select * from netflix
	where 
	  show_type= 'Movie'
	  and 
	  duration = ( select max(duration) from netflix)

    
--6)Find the Content added in the last 10 years

     select * 
	 from netflix 
	 where 
	 TO_DATE(date_added,'Month DD,YYYY') >= current_date - Interval '10 years'
	 
--7)Find the movies/TV shows by director " Nikhil Pherwani"

       select show_type,
	           title,
			   director from netflix
			   where DIRECTOR ILIKE '%Nikhil Pherwani%'
			  
--8) List all TV shows with more than 5 seasons
 
         select
		     *
	        from netflix
			where 
			show_type= 'TV Show' 
			and
			split_part(duration, ' ', 1)::numeric > 5;
			
--9)Count the no.of content items in each genere 

       select 
	          unnest(string_to_array(listed_in, ',')) as genre,
			  Count(show_id) as total_content
			  from netflix
	   group  by 1;
	   

--10) List all the movie that are documentries

    select *
	  from netflix
	   where 
	      listed_in ILIKE '%documentaries%'

--11)Find all content without a director

	   select *
	     from netflix
	     where 
	     director is null;

--12)Categorize the content based on the presence of the key word 'KILL' and  'VIOLENCE'
      in the description field. Label content containing these keywords as 'BAD' and all 
	  other as 'GOOD'.Count how ,many items fall into each category.

  select *,
	  case 
	  when 
	  description ILIKE '%kill%' or 
	  description ILIKE '%violence%' then 'Bad_film'
		 else 'GOOD'
		 end category
	  from netflix
```
## Business Insights

- Analyzed the distribution of Movies and TV Shows to understand Netflix's overall content strategy.
- Identified the most common maturity ratings to uncover audience preferences and content classification patterns.
- Explored year-wise content releases to identify production and release trends.
- Discovered the top content-producing countries, highlighting Netflix's strongest regional markets.
- Examined movie durations and long-running TV series to identify high-engagement content.
- Assessed catalog freshness by analyzing content added over the last 10 years.
- Performed genre-wise analysis to identify the most popular content categories and support content planning.
- Evaluated data quality by detecting missing metadata, such as director information.
- Classified content into family-friendly and mature categories using keyword-based analysis of descriptions.
- Generated actionable insights to support content acquisition, audience targeting, catalog optimization, and data-driven business decision-making.
---

## 🚀 Key Takeaways
- Analyzed **8,800+ Netflix records**
- Solved **12 real-world business problems**
- Applied PostgreSQL functions for data cleaning, transformation, and analysis
- Strengthened SQL skills using real-world analytical scenarios
