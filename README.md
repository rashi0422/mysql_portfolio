# mysql_portfolio

SELECT * FROM project.population;
SELECT * FROM project.literacy;

-- Number of rows in dataset --
select count(*) from population;
select count(*) from literacy;

-- avg growth --
select avg(Growth) from literacy ;
select avg(Growth) from literacy group by State;

-- avg sex_ratio --
select avg(Sex_Ratio) from literacy;

-- Dataset from jharkhand and bihar --
select * from population where State = 'Bihar' or 'Jharkhand' ;

-- Population of india --
select sum(Population) from population;

-- avg sex_ratio according to state --
select avg(Sex_Ratio) from literacy group by State ;

-- avg in order --
select state, avg(Sex_Ratio) as 'avg_ratio' from literacy group by State order by avg_ratio desc ;

-- avg round off --
select state, round(avg(Sex_Ratio),0) as 'avg_ratio' from literacy group by State order by avg_ratio desc ;

-- avg literacy rate greater than >90--
select State, round(avg(literacy),0) as 'avg_rate' from literacy group by State having round(avg(literacy),0) >90;

-- Top 3 State showing highest growth --
select State, round(avg(growth),0) as 'top_3_state' from literacy group by State order by top_3_state desc limit 3;

-- top & bottom 3 state lowest population--
select State, sum(Population) as 'bottom_3_state' from population group by State order by bottom_3_state asc limit 3;

-- States starting with letter a --
select * from project.population where State like 'a%';
select distinct state from project.population where lower(State) like 'a%';
select distinct state from project.population where lower(State) like 'a%' or lower(State) like 'b%';

-- Joining both table --
select project.population.State,project.population.Population , project.population.District, project.literacy.Sex_Ratio from project.population inner join project.literacy on project.population.District = project.population.District;

-- Calulate males and females --
select population.District ,population.State , population.Population/(literacy.Sex_Ratio +1 ) males, (population.Population*literacy.Sex_Ratio)/(literacy.Sex_Ratio+1) females from 
(select population.District, population.State, literacy.Sex_Ratio/1000 sex_ratio, population.population from population inner join literacy on population.District = population.District) as sub ;

-- total literacy rate --
select population.State, literacy.District, literacy.Literacy_ratio*population.population literate_people , (1-literacy.literacy_ratio)* population.population illiterate_people from 
(select population.District , population.State , literacy.Literacy/100 literacy_ratio, population.Population from project.population inner join project.literacy on population.District = literacy.District) as sub;

-- Rank --
select population.* from (select literacy.District , literacy.State, literacy.literacy , rank() over (partition by state order by literacy desc) rnk from project.population) population 
where Population.rnk in(1,2,3) order by State ;

 
