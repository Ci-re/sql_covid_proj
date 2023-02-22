select * from coviddeaths;
select * from covidvaccines;


-- likelihood of dying of covid in Nigeria in percentage
select location, date, total_cases, total_deaths, 
	(total_deaths/total_cases) * 100 as death_percent
		from coviddeaths where
		location like '%Nigeria%' 
		order by death_percent desc;
		
		
-- likelihood of getting covid in Nigeria
select location, date, total_cases, population, 
	(total_cases/population) * 100 as cases_percent
		from coviddeaths where
		location like '%Nigeria%'
		order by cases_percent desc;
		
		
-- countries with the highest infection rate
select location, max(total_cases) as highest, population,
	max((total_cases/population) * 100) as highest_percent_affected
		from coviddeaths
	group by location, population
	order by highest_percent_affected desc;
	
	
-- showing countries with the highest death count per population
select continent, max(cast(total_deaths as int)) as total_death_count
	from coviddeaths 
	where continent is not null
-- 	where location like '%Nigeria'
	group by continent
	order by total_death_count desc;
	

-- Showing rolling count of 
select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
	sum(cast(vac.new_vaccinations as int)) 
	over (partition by dea.location order by dea.location, dea.date)
	as rolling_people_vaccinated
from coviddeaths dea
join covidvaccines vac
	on dea.location = vac.location 
	and dea.date = vac.date
where dea.continent is not null
order by 2,3;


-- Using CTE
with population_vs_vac (continent, location, date, population, new_vaccinations, rolling_people_vaccinated) 
	as (
		select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
		sum(cast(vac.new_vaccinations as int))
		over (partition by dea.location order by dea.location, dea.date)
		as rolling_people_vaccinated
	from coviddeaths dea
	join covidvaccines vac
		on dea.location = vac.location
		and dea.date = vac.date
	where dea.continent is not null
	)
select *, (rolling_people_vaccinated/population) * 100 as rolling_people_percent from population_vs_vac 
	where location like '%Nigeria'