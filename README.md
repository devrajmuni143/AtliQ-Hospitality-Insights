
# AtliQ Grands Business Intelligence Dashboard

## Problem Statement

AtliQ Grands, a leading hospitality chain in India, faces challenges in maintaining market share and revenue in the luxury/business hotels category due to competition and ineffective decision-making. To address this, the managing director aims to incorporate Business and Data Intelligence. However, lacking an in-house data analytics team, they seek insights from historical data through a third-party service provider.

## Task List

As a data analyst, you've been tasked with:

1. Creating metrics according to the provided metric list.
2. Designing a dashboard based on stakeholder mock-ups.
3. Generating relevant insights from the data.

## Data Sources Provided

- dim_date.csv
- dim_hotels.csv
- dim_rooms.csv
- fact_aggregated_bookings.csv
- fact_bookings.csv

These datasets contain information on dates, hotels, rooms, bookings, and more, necessary for analysis and dashboard creation.

# Key Measures

1. **Revenue**
   - **Formula**: `SUM(fact_bookings[revenue_realized])`

2. **Total Bookings**
   - **Formula**: `COUNT(fact_bookings[booking_id])`

3. **Total Capacity**
   - **Formula**: `SUM(fact_aggregated_bookings[capacity])`

4. **Total Successful Bookings**
   - **Formula**: `SUM(fact_aggregated_bookings[successful_bookings])`

5. **Occupancy %**
   - **Formula**: `DIVIDE([Total Successful Bookings],[Total Capacity],0)`

6. **Average Rating**
   - **Formula**: `AVERAGE(fact_bookings[ratings_given])`

7. **No of days**
   - **Formula**: `DATEDIFF(MIN(dim_date[date]),MAX(dim_date[date]),DAY) +1`

8. **Total Cancelled Bookings**
   - **Formula**: `CALCULATE([Total Bookings],fact_bookings[booking_status]="Cancelled",fact_bookings[booking_status]=”No Show”)`

9. **Cancellation %**
   - **Formula**: `DIVIDE([Total Cancelled Bookings],[Total Bookings])`

10. **Total Checked Out**
    - **Formula**: `CALCULATE([Total Bookings],fact_bookings[booking_status]="Checked Out")`

11. **Total No Show Bookings**
    - **Formula**: `CALCULATE([Total Bookings],fact_bookings[booking_status]="No Show")`

12. **No Show Rate %**
    - **Formula**: `DIVIDE([Total No Show Bookings],[Total Bookings])`

13. **Booking % by Platform**
    - **Formula**: `DIVIDE([Total Bookings], CALCULATE([Total Bookings], ALL(fact_bookings[booking_platform])))*100`

14. **Booking % by Room Class**
    - **Formula**: `DIVIDE([Total Bookings], CALCULATE([Total Bookings], ALL(dim_rooms[room_class])))*100`

15. **ADR (Average Daily Rate)**
    - **Formula**: `DIVIDE( [Revenue], [Total Bookings],0)`

16. **Realisation %**
    - **Formula**: `1- ([Cancellation %]+[No Show Rate %])`

17. **RevPAR (Revenue per Available Room)**
    - **Formula**: `DIVIDE([Revenue],[Total Capacity])`

18. **DBRN (Daily Booking Rate per Night)**
    - **Formula**: `DIVIDE([Total Bookings], [No of days])`

19. **DSRN (Daily Sold Room per Night)**
    - **Formula**: `DIVIDE([Total Capacity], [No of days])`

20. **DURN (Daily Utilized Room per Night)**
    - **Formula**: `DIVIDE([Total Checked Out],[No of days])`

21. **Revenue WoW Change %**
    - **Formula**: 
      ```scss
      Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
      var revcw = CALCULATE([Revenue],dim_date[wn]= selv)
      var revpw =  CALCULATE([Revenue],FILTER(ALL(dim_date),dim_date[wn]= selv-1))
      return DIVIDE(revcw,revpw,0)-1
      ```

22. **Occupancy WoW Change %**
    - **Formula**: 
      ```scss
      Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
      var revcw = CALCULATE([Occupancy %],dim_date[wn]= selv)
      var revpw =  CALCULATE([Occupancy %],FILTER(ALL(dim_date),dim_date[wn]= selv-1))
      return DIVIDE(revcw,revpw,0)-1
      ```

23. **ADR WoW Change %**
    - **Formula**: 
      ```scss
      Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
      var revcw = CALCULATE([ADR],dim_date[wn]= selv)
      var revpw =  CALCULATE([ADR],FILTER(ALL(dim_date),dim_date[wn]= selv-1))
      return DIVIDE(revcw,revpw,0)-1
      ```

24. **RevPAR WoW Change %**
    - **Formula**: 
      ```scss
      Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
      var revcw = CALCULATE([RevPAR],dim_date[wn]= selv)
      var revpw =  CALCULATE([RevPAR],FILTER(ALL(dim_date),dim_date[wn]= selv-1))
      return DIVIDE(revcw,revpw,0)-1
      ```

25. **Realisation WoW Change %**
    - **Formula**: 
      ```scss
      Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
      var revcw = CALCULATE([Realisation %],dim_date[wn]= selv)
      var revpw =  CALCULATE([Realisation %],FILTER(ALL(dim_date),dim_date[wn]= selv-1))
      return DIVIDE(revcw,revpw,0)-1
      ```

26. **DSRN WoW Change %**
    - **Formula**: 
      ```scss
      Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
      var revcw = CALCULATE([DSRN],dim_date[wn]= selv)
      var revpw =  CALCULATE([DSRN],FILTER(ALL(dim_date),dim_date[wn]= selv-1))
      return DIVIDE(revcw,revpw,0)-1
  27. **Selected Booking Platform (BP)**
    - **Formula**: `SELECTEDVALUE(fact_bookings[booking_platform])`

28. **Selected Room Class (RC)**
    - **Formula**: `SELECTEDVALUE(dim_rooms[room_class])`

29. **Loss Due to Cancellation (LDC)**
    - **Formula**: `SUM(fact_bookings[revenue_generated])-[Revenue]`

30. **LDC Title**
    - **Formula**: `"Loss Due To Cancellation For " & [Selected BP] & FORMAT([LDC]/ 1000000, " $ #,##0.0") & "M" & " Of " & FORMAT(CALCULATE([LDC],ALL(fact_bookings))/1000000," $ #,##0.0")& "M"`

31. **LDC Title By RC (Room Category)**
    - **Formula**: `"Loss Due To Cancellation For " & [Selected RC] & FORMAT([LDC]/ 1000000, " $ #,##0.0") & "M" & " Of " & FORMAT(CALCULATE([LDC],ALL(fact_bookings))/1000000," $ #,##0.0")& "M"`
  
  ## Important Insights from the Dashboard

- Mumbai leads in revenue generation, with 669 million, followed by Bangalore, Hyderabad, and Delhi.
- AtliQ Exotica outperforms all 7 types of properties, with revenue of 320 million, a rating of 3.62, 57% occupancy, and a 24.4% cancellation rate.
- AtliQ Blu boasts the highest occupancy rate of 62%.
- Week 29 recorded the highest revenue of 139.7 million.
- Delhi ranks highest in both occupancy and rating, followed by Hyderabad, Mumbai, and Bangalore.
- AtliQ suffered losses of around 299 million due to cancellations across different room categories.
- Elite-type rooms have the highest booking numbers but also a higher cancellation rate.
      ```
## Lessons Learned from this Project

- By examining various cancellation policies implemented by different hotels, I gained an understanding that most hotels impose no fees for cancellations made before three months from the booking date. However, if cancellations occur after that period, charges typically range from 60 to 90% of the booking cost.
- I acquired knowledge of hospitality domain terms such as RevPar, ADR, RevPAR, DBRN, DSRN, and DURN.