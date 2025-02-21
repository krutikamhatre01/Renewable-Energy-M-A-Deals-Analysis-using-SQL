# Renewable-Energy-M-A-Deals-Analysis-using-SQL

Project Overview:
This project analyzes Mergers & Acquisitions (M&A) trends in the Renewable Energy sector using SQL. By examining transaction data, we gain insights into top acquiring companies, deal values, market trends, and sector-wise M&A distribution.

Database Schema
Tables Used:
1. RE_Companies: Stores company details such as name, country, sector, and revenue.
2. Deals: Stores M&A transactions with details like acquiring company, acquired company, deal value, sector, and deal date

SQL Functions and Formulas Used
1. Aggregate Functions:
2. Built_in Functions
3. Where and Having Clauses
4. Joins
5. Set Operators
---------------------------------------------------------------------------

SQL Queries used for analysis

select * from RE_Companies
select * from deals

DESC RE_Companies
DESC Deals

-- Count total records
select count(*) from RE_Companies
select count(*) from Deals

-- View Sample Data
select * from Deals  -- Rows set to 10

-- Find the Top 5 Acquiring Companies (Max number of deals)
select buyer, count(*) AS total_acquisitions
from deals
group by buyer
order by total_acquisitions DESC  -- Rows set to 10

-- Find the Total Deal Value Per Sector
select sector, sum(deal_value_usd) as Total_Deal_Value
from deals
group by sector
order by Total_Deal_Value DESC

-- Find Yearly M&A Trends (Deals Per Year)
select extract(year from deal_date) AS Year, count(*) as Total_Deals
from deals
group by extract(year from deal_date)
order by Year ASC

-- Find Yearly M&A Trends (Deals Per Year By Sector)
select sector, extract(year from deal_date) AS Year, count(*) as Total_Deals
from deals
group by extract(year from deal_date), sector
order by Year ASC

-- Find the Largest M&A Deals (Top 10 by Value)
select buyer, seller, sector, deal_date, max(deal_value_usd) as MAX_Deal_Value
from deals
group by deal_date, buyer, seller, sector
order by MAX_Deal_Value DESC


-- Find the Most Acquired Companies
select seller, count(*) AS Acquisition_Count
from deals
group by seller
order by Acquisition_Count DESC

-- Find Deals in the Last 5 Years
select deal_date, buyer, seller, sector, deal_value_usd
from deals
where deal_date between '01-01-2020' and '12-31-2024'
order by deal_date DESC

select buyer, seller, deal_value_usd, deal_date
from deals
where deal_date >= add_months(sysdate, -60) -- Last 5 years
order by deal_date DESC

-- Find All M&A Deals Above $2 Billion
select * 
from deals 
where deal_value_usd > 2000000000
order by deal_value_usd

-- Find M&A Deals for a Specific Sector (e.g., Solar)
select deal_date, buyer, seller, deal_value_usd
from deals
where sector = 'Solar'

-- Find Sectors Where the Total Deal Value Exceeds $5 Billion
select sector, sum(deal_value_usd)
from deals
group by sector
having sum(deal_value_usd) > 5000000000
order by sum(deal_value_usd) DESC

-- Find Companies That Have Made More Than 10 Acquisitions
select buyer, count(*) as TotalDeals
from deals
group by buyer
having count(*) > 10
order by TotalDeals DESC

-- Get Company Details Along with Acquisition Count
select R.company_name, R.country, count(D.id) As total_acq
from RE_Companies R
inner join deals D
on R.ID = D.ID
group by R.company_name, R.country
order by total_acq DESC

-- Find Acquisitions Where the Acquired and Acquiring Companies Are from the Same Country
select D.buyer, D.seller, R1.country, D.deal_value_usd
from Deals D
join RE_Companies R1
on D.buyer = R1.company_name
join RE_Companies R2
on D.seller = R2.company_name
where R1.country = R2.country
order by D.deal_value_usd

-- Find Companies That Have Been Acquired More Than 5 Times Along with Their Sector
select R.company_name, R.sector, count(D.id) as Times_acq
from RE_Companies R
join Deals D
on R.company_name = D.Seller
group by R.company_name, R.sector
having count(D.id) > 5
order by Times_acq

-- Extract the Year and Month from Deal Dates
select buyer, seller, deal_value_usd, 
to_char(deal_date, 'YYYY'), 
to_char(deal_date, 'Month')
from deals
order by deal_date

-- Format Deal Values in Millions with Currency Symbol
select buyer, seller, CONCAT(deal_value_usd, 'M USD') from Deals


-- Find Deals Where the Acquiring Company's Name Starts with 'E'
select id, buyer, seller, sector, deal_value_usd
from deals
where buyer like 'E%'

-- Find the Average Deal Value Per Sector
select sector, round(avg(deal_value_usd)) AS average_deal_value
from deals
group by sector
order by average_deal_value DESC

-- Find the Highest and Lowest Deal Values in Each Sector
select sector, 
max(deal_value_usd) AS Highest_deal_value, 
min(deal_value_usd) AS Lowest_deal_value
from deals
group by sector

-- Count the Number of Deals Each Year
select to_char(deal_date, 'YYYY'), count(*) AS Total_deals
from deals
group by to_char(deal_date, 'YYYY')

-- Find Companies That Are Both Acquirers and Acquired
select buyer as Company_name from deals 
intersect 
select seller from deals

-- Find Companies That Have Only Been Acquired (Never Acquired Another Company)
select seller as Company_name from deals 
minus
select buyer from deals

--  Find All Unique Companies That Have Been Involved in M&A Deals (Either as Acquirer or Acquired)
select Buyer as Company_name from deals 
union
select Seller as Company_na   me from deals 

Conclusion:
The SQL analysis provides valuable insights into the Renewable Energy M&A sector, identifying: 
The most active acquiring companies 
Key trends in deal values and sectors 
Yearly trends in M&A activity
Next steps: If required, these findings can be visualized using Power BI for better presentation and deeper insights.
