# Cyber-
#Total number of incidents per year 
SELECT Year , COUNT(*) as num FROM global_cybersecurity_threats_2015_2024
GROUP BY Year 
#Total loss per year and country 
SELECT SUM(`Financial Loss (in Million $)`) as total,Year , Country FROM global_cybersecurity_threats_2015_2024
GROUP BY Year , Country ;
#What is the avg impact by attack type 
WITH cte AS (
    SELECT 
        `Attack Type`,
        SUM(`Financial Loss (in Million $)`) AS total,
        COUNT(`Attack Type`) AS num
    FROM global_cybersecurity_threats_2015_2024
    GROUP BY `Attack Type`
)
SELECT 
    `Attack Type`,
    ROUND(total / num, 2) AS avg
FROM cte
GROUP BY `Attack Type`
#wich industries are the most frequently targeted 
SELECT `Target Industry` , COUNT(*) as num ,
dense_rank() over ( ORDER BY num DESC) as rank FROM global_cybersecurity_threats_2015_2024 
GROUP by `Target Industry`
ORDER BY rank
limit 3 
#Which sources  cause for the highest financial loss 
SELECT `Attack Source` ,round( SUM(`Financial Loss (in Million $)`),2) as total ,
dense_rank() over ( ORDER by total DESC ) as rank FROM global_cybersecurity_threats_2015_2024 
GROUP BY `Attack Source` 
ORDER BY rank 
LIMIT 3 
#Which countries have slowest or fastest resolution time 
SELECT 
    `Country`, 
    COUNT(*) AS num,
    AVG(`Incident Resolution Time (in Hours)`) AS avg_time,
    DENSE_RANK() OVER (ORDER BY AVG(`Incident Resolution Time (in Hours)`) ASC) AS rank
FROM global_cybersecurity_threats_2015_2024
GROUP BY `Country`
ORDER BY rank;
#Which countries have most exploited vunerabilities and their Average financial loss
SELECT `Security Vulnerability Type` , round(AVG(`Financial Loss (in Million $)`),2) as avg , dense_rank() over ( ORDER BY avg) as rank FROM global_cybersecurity_threats_2015_2024 
GROUP BY `Security Vulnerability Type` 
ORDER BY rank DESC
now connecting to power bi by obdc connector 
KPIs
Average financial loss in millions [AVERAGE(global_cybersecurity_threats_2015_2024_1[Financial Loss (in Million $)])]
No. of incidents: [COUNT(global_cybersecurity_threats_2015_2024_1[Incident Resolution Time (in Hours)])]
Total affected users : 
  [SUM(global_cybersecurity_threats_2015_2024_1[Number of Affected Users])]
Total Financial loss in millions:
  [SUM(global_cybersecurity_threats_2015_2024_1[Financial Loss (in Million $)]]
Key Visualizations used :
Tree map : Total affected users vs targeted industry 
Line chart : Total financial loss in millions vs year 
Stacked bar chart : Average financial loss in millions vs attack source 
Stacked bar chart : total attack type vs attack type 
Map : total loss in millions vs country 
Stack column chart : total loss in millions vs attack type 
Scattered chart : Total affected users vs security vulnerability
Pie chart : total affected users vs attack source 
Donut chart : Total affected users vs security vulnerability
Tree map : total financial loss in millions vs targeted industry 
Map : Total attack vs country 
KPIs 
Average total loss in millions $ = 50.49
No.of incidents = 3000
Total Affected users = 2bn
Total financial loss in millions $ = 151.48k 
