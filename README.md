
# AtliQ Grands Business Intelligence Dashboard

## Problem Statement

AtliQ Grands, a leading hospitality chain in India, faces challenges in maintaining market share and revenue in the luxury/business hotels category due to competition and ineffective decision-making. To address this, the managing director aims to incorporate Business and Data Intelligence. However, lacking an in-house data analytics team, they seek insights from historical data through a third-party service provider.

This project is part of the Codebasics Resume Project Challenge . It involves the analysis of data from AtliQ Grands, a leading hospitality chain in India, to create a Business Intelligence Dashboard.

## Challenge Details

- **Challenge Link:** [CodeBasics Resume Project Challenge](https://codebasics.io/challenge/codebasics-resume-project-challenge)
- **Live Dashboard:** [AtliQ Hospitality Dashboard](https://www.novypro.com/project/atliq-hospitality-dashboard---resume-project-challenge-1)



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

# AtliQ Hospitality Data Description

## dim_date
- **date:** Represents dates in May, June, and July.
- **mmm yy:** Represents dates in the format of month name and year (mmm yy).
- **week no:** Unique week number for each date.
- **day_type:** Indicates whether the day is a Weekend or Weekday.

## dim_hotels
- **property_id:** Unique ID for each hotel.
- **property_name:** Name of each hotel.
- **category:** Indicates if the hotel belongs to the Luxury or Business class.
- **city:** Location of the hotel.

## dim_rooms
- **room_id:** Type of room (e.g., RT1, RT2, RT3, RT4) in a hotel.
- **room_class:** Class of the room (e.g., Standard, Elite, Premium, Presidential).

## fact_aggregated_bookings
- **property_id:** Unique ID for each hotel.
- **check_in_date:** Dates when customers checked in.
- **room_category:** Type of room (e.g., RT1, RT2, RT3, RT4) in a hotel.
- **successful_bookings:** Count of successful room bookings for a specific room type and date.
- **capacity:** Maximum count of rooms available for a particular room type and date.

## fact_bookings
- **booking_id:** Unique booking ID for each customer.
- **property_id:** Unique ID for each hotel.
- **booking_date:** Date when the customer made the booking.
- **check_in_date:** Date when the customer checked in.
- **check_out_date:** Date when the customer checked out.
- **no_guests:** Number of guests staying in a room.
- **room_category:** Type of room (e.g., RT1, RT2, RT3, RT4) in a hotel.
- **booking_platform:** Platform through which the booking was made.
- **ratings_given:** Ratings given by the customer for hotel services.
- **booking_status:** Indicates if the booking was Cancelled, Checked Out, or No Show.
- **revenue_generated:** Amount of money generated by the hotel from the booking.
- **revenue_realized:** Final amount received by the hotel, considering the booking status.

# Data Modelling
![image](https://github.com/devrajmuni143/AtliQ-Hospitality-Insights/assets/100869651/ab03ce71-5bcd-4ea3-b22f-49b6d5be9606)

# Dashboard
![Hospitality Insights Dashboard](https://github.com/devrajmuni143/AtliQ-Hospitality-Insights/assets/100869651/0f27d830-641b-4759-8f45-120768f3938b)

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
- Acquired knowledge of hospitality domain terms such as RevPar, ADR, RevPAR, DBRN, DSRN, and DURN.
