CREATE TABLE Total_Index 
AS SELECT fertility_rate.Country, 
  CAST(Birth_rate AS DECIMAL(10,2)) AS BR, 
  house_income.Affordability AS Afford_Index,
  CAST(peace_index.GPI AS DECIMAL(10,2)) AS GPI,
  gini_index.F_Schooling AS F_schooling_rate,
  gini_index.F_labor AS F_labor_rate,
  hdi_index.hdi
FROM fertility_rate
LEFT JOIN house_income
  ON fertility_rate.Country = house_income.Country
LEFT JOIN peace_index
  ON fertility_rate.Country = peace_index.Country
LEFT JOIN gini_index
  ON fertility_rate.Country = gini_index.Country
LEFT JOIN hdi_index
  ON fertility_rate.Country = hdi_index.Country;
