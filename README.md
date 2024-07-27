Market Analysis of Germany Energy Market with price prediction via regression 

.csv files are the main files. These files combined in a SQL Server db. this is the code:

SELECT 
	tm.*,
    YEAR(C.datetime) AS 'Year',
    MONTH(C.datetime) AS 'Month',
    DAY(C.datetime) AS 'Day',
    DATEPART(HOUR, C.datetime) AS 'Hour',
	c.datetime,
    C.consumption,
	pr.dam,pr.solar,pr.ror,pr.WindOffshore,pr.WindOnshore,pr.HardCoal,pr.Nuclear,pr.NaturalGas,
	C.consumption-pr.solar-pr.ror-pr.WindOffshore-pr.WindOnshore-pr.dam as 'Consumptionfromnonren',
	pr.solar+pr.ror+pr.WindOffshore+pr.WindOnshore+pr.dam as 'Renewable',
	co.Clean_Dark_Spread-co.Clean_Spark_Spread as difofspreads,
	CASE 
        WHEN co.Clean_Dark_Spread >= Clean_Spark_Spread THEN Clean_Dark_Spread
        ELSE Clean_Spark_Spread
    END AS max_spread,
CASE WHEN co.Clean_Dark_Spread <= Clean_Spark_Spread THEN Clean_Dark_Spread ELSE Clean_Spark_Spread END AS min_spread, Co.*, NI.*, P.*, W.*, pmı.PMI_Index

from [dbo].[hourly_data_with_bank_holidays] tm 
left join 
    [dbo].[consumption] AS C on  YEAR(C.datetime) = tm.[Year] AND MONTH(C.datetime) = tm.[Month] AND DAY(C.datetime) = tm.[Day] AND DATEPART(HOUR, C.datetime) =tm.[Hour]
LEFT JOIN 
    [dbo].[costs] AS Co ON tm.[Year] = YEAR(Co.date) AND tm.[month] = MONTH(Co.date) AND tm.[Day] = DAY(Co.date) 
LEFT JOIN 
    [dbo].[net_imports] AS NI ON tm.[Year] = YEAR(NI.date) AND  tm.[month] = MONTH(NI.date) AND tm.[Day] = DAY(NI.date) AND DATEPART(HOUR, C.datetime) = DATEPART(HOUR, NI.date)
LEFT JOIN 
    [dbo].[price] AS P ON YEAR(C.datetime) = YEAR(P.datetime) AND MONTH(C.datetime) = MONTH(P.datetime) AND DAY(C.datetime) = DAY(P.datetime) AND tm.Hour = DATEPART(HOUR, P.datetime)
LEFT JOIN 
    [dbo].[production] AS Pr ON tm.Year = YEAR(Pr.datetime) AND tm.Month = MONTH(Pr.datetime) AND tm.Day = DAY(Pr.datetime) AND tm.Hour = DATEPART(HOUR, Pr.datetime)
LEFT JOIN 
    [dbo].[weather] AS W ON tm.Year = YEAR(W.datetime) AND tm.Month = MONTH(W.datetime) AND tm.dAY = DAY(W.datetime) AND TM.Hour = DATEPART(HOUR, W.datetime)
Left join [dbo].[germany_pmi_index_2018_2023] pmı on  tm.Year=pmı.Year and tm.Month=pmı.Month 
	ORDER BY     tm.Year,tm.Month,tm.Day,tm.Hour

 When the data is manipulated, analysis file conducted. Then with a new data (prediction) prediction conducted. 
 
