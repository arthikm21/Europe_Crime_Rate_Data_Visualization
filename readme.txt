1. Load the data(Excel File) to PowerBI and transform it as pet the requirement.
2. Removed Longitute Column from the OtherDate.xlsl file as it had null componenets which could hamper our analysis.
3. Applied Feature Engineering in Crime Date_Time column to product date and time in separate columns. Used Split Column feature present in HomeTab of PowerBI
4. Used Extract Function from AddColumn tab to extract AM/PM from the Time Column.
5. Created a Table to narrow down the years of reported crime to only last 3 years 2021,2020,2019. Then, extracted the Year and Month as needed. The code used is as follows: 
DateTable = VAR _MinDate = YEAR(MIN(Data[Crime Date])) 
VAR _MaxDate = YEAR(MAX(Data[Crime Date]))
return
ADDCOLUMNS(
    FILTER(
        CALENDARAUTO(),
        YEAR([Date])>= _MinDate &&
        YEAR([Date])<= _MaxDate),
        "Year",YEAR([Date]))
6. To Identify the Year of Year change in Total Crime rate, I used the following query to calculate the change in number of crimes and created a measure named Lebel in Calculations Table:
Lebel (Year) = 
 VAR _PrevYear =
    CALCULATE(
        [Total Crimes],
        SAMEPERIODLASTYEAR(DateTable[Date]))
        VAR _YoYChange =
          [Total Crimes] - _PrevYear
          return
          _YoYChange
7.Created Measures to create the data visualization dynamic, interactive and much easier to understand. For example, A measure to do Conditional Formating in Year Of Year Change in Crime rates as follows;
CF (Year) = 
 VAR _PrevYear =
    CALCULATE(
        [Total Crimes],
        SAMEPERIODLASTYEAR(DateTable[Date]))
        VAR _YoYChange =
        IF(_PrevYear<>BLANK(),
            [Total Crimes] - _PrevYear, "-")
         
        return
        SWITCH(
              TRUE(),
            _YoYChange= 0,"Gray",
            _YoYChange>=1,"Green",
            _YoYChange<1,"Red"
          )
8. 













