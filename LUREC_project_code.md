    library(tidyverse)

    ## Warning: package 'ggplot2' was built under R version 4.4.3

    ## Warning: package 'tibble' was built under R version 4.4.3

    ## Warning: package 'tidyr' was built under R version 4.4.3

    ## Warning: package 'readr' was built under R version 4.4.3

    ## Warning: package 'purrr' was built under R version 4.4.3

    ## Warning: package 'dplyr' was built under R version 4.4.3

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.0     ✔ readr     2.1.6
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.2     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.1     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

    library(plotly)

    ## Warning: package 'plotly' was built under R version 4.4.3

    ## 
    ## Attaching package: 'plotly'
    ## 
    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     last_plot
    ## 
    ## The following object is masked from 'package:stats':
    ## 
    ##     filter
    ## 
    ## The following object is masked from 'package:graphics':
    ## 
    ##     layout

    library(regclass)

    ## Loading required package: bestglm
    ## Loading required package: leaps
    ## Loading required package: VGAM

    ## Warning: package 'VGAM' was built under R version 4.4.3

    ## Loading required package: stats4
    ## Loading required package: splines
    ## Loading required package: rpart
    ## Loading required package: randomForest
    ## randomForest 4.7-1.2
    ## Type rfNews() to see new features/changes/bug fixes.
    ## 
    ## Attaching package: 'randomForest'
    ## 
    ## The following object is masked from 'package:dplyr':
    ## 
    ##     combine
    ## 
    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     margin
    ## 
    ## Important regclass change from 1.3:
    ## All functions that had a . in the name now have an _
    ## all.correlations -> all_correlations, cor.demo -> cor_demo, etc.

The very first action taken, before the project began, was downloading
nearby precipitation data from this site:
<https://www.ncei.noaa.gov/access/past-weather/Woodstock%2C%20Illinois.>

This was later visualized against the soil water content data from the
field site.

    ## Warning: One or more parsing issues, call `problems()` on your data frame for details,
    ## e.g.:
    ##   dat <- vroom(...)
    ##   problems(dat)

    ## Rows: 6525 Columns: 7
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Date
    ## dbl (3): PRCP (Inches), SNOW (Inches), SNWD (Inches)
    ## lgl (3): TAVG (Degrees Fahrenheit), TMAX (Degrees Fahrenheit), TMIN (Degrees...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## Warning: One or more parsing issues, call `problems()` on your data frame for details,
    ## e.g.:
    ##   dat <- vroom(...)
    ##   problems(dat)

    ## Rows: 6402 Columns: 7
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Date
    ## dbl (3): PRCP (Inches), SNOW (Inches), SNWD (Inches)
    ## lgl (3): TAVG (Degrees Fahrenheit), TMAX (Degrees Fahrenheit), TMIN (Degrees...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## Warning: One or more parsing issues, call `problems()` on your data frame for details,
    ## e.g.:
    ##   dat <- vroom(...)
    ##   problems(dat)

    ## Rows: 8208 Columns: 7
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Date
    ## dbl (3): PRCP (Inches), SNOW (Inches), SNWD (Inches)
    ## lgl (3): TAVG (Degrees Fahrenheit), TMAX (Degrees Fahrenheit), TMIN (Degrees...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

I then prepared the data for visualization and analysis by reformatting
each date field to date class, filtering rows for 2025, and combining
each station’s data into a single dataset.

    Woodstock_0.7_SW_data<-Woodstock_0.7_SW_data%>%
      mutate(Date=ymd(Date))%>%
      mutate(Year=year(Date), Month=month(Date), Day=day(Date))%>%
      filter(Year==2025)%>%
      select(c(1,5,8,9,10))

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `Date = ymd(Date)`.
    ## Caused by warning:
    ## !  2 failed to parse.

    Woodstock_3.8_SW_data<-Woodstock_3.8_SW_data%>%
      mutate(Date=ymd(Date))%>%
      mutate(Year=year(Date), Month=month(Date), Day=day(Date))%>%
      filter(Year==2025)%>%
      select(c(1,5,8,9,10))

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `Date = ymd(Date)`.
    ## Caused by warning:
    ## !  2 failed to parse.

    Woodstock_5_NW_data<-Woodstock_5_NW_data%>%
      mutate(Date=ymd(Date))%>%
      mutate(Year=year(Date), Month=month(Date), Day=day(Date))%>%
      filter(Year==2025)%>%
      select(c(1,5,8,9,10))

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `Date = ymd(Date)`.
    ## Caused by warning:
    ## !  2 failed to parse.

    Woodstock_combined_weather_data<-Woodstock_0.7_SW_data%>%
      left_join(Woodstock_3.8_SW_data, by=c("Date"="Date"))%>%
      left_join(Woodstock_5_NW_data, by=c("Date"="Date"))%>%
      select(1,2,6,10)

    names(Woodstock_combined_weather_data)<-c("Date","SW_0.7_Precip", "SW_3.8_Precip", "NW_5_Precip")

At the onset of my Environmental Research class in January, I obtained
the onsite soil moisture data from my professor, which includes moisture
data from November 10, 2024 (just after being installed) to October 6,
2025.

    six_LUREC_water_sensor_data<-read_csv("~/Desktop/Ghatak_research_project/6_LUREC_soil_water_sensors.csv")

    ## New names:
    ## Rows: 7918 Columns: 9
    ## ── Column specification
    ## ──────────────────────────────────────────────────────── Delimiter: "," chr
    ## (9): z6B03659, Port 1, Port 2, Port 3, Port 4, Port 5, Port 6, Port 7......
    ## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
    ## Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## • `Port 7` -> `Port 7...8`
    ## • `Port 7` -> `Port 7...9`

    six__more_LUREC_water_sensors<-read_csv("~/Desktop/Ghatak_research_project/6_more_LUREC_water_sensors.csv")

    ## New names:
    ## Rows: 7912 Columns: 9
    ## ── Column specification
    ## ──────────────────────────────────────────────────────── Delimiter: "," chr
    ## (9): z6B03782, Port 1, Port 2, Port 3, Port 4, Port 5, Port 6, Port 7......
    ## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
    ## Specify the column types or set `show_col_types = FALSE` to quiet this message.
    ## • `Port 7` -> `Port 7...8`
    ## • `Port 7` -> `Port 7...9`

Each dataset contains readings from a single data logger, which receives
data from six of the twelve sensors (2 datasets = 2 data loggers = 12
total sensors).

I then performed some basic data cleaning, including removal of column
headers that were formatted as observations and renaming columns to
prevent word splitting issues, and changing water content readings from
character to numeric class.

    six_LUREC_water_sensor_data_new<-six_LUREC_water_sensor_data%>%
    tail(7916)
    colnames(six_LUREC_water_sensor_data_new)<-c("Timestamp", "Pot_1_Water_Content_proportion", "Pot_2_Water_Content_proportion", "Pot_3_Water_Content_proportion", "Pot_4_Water_Content_proportion", "Pot_5_Water_Content_proportion", "Pot_6_Water_Content_proportion", "Battery_percent", "Battery_voltage_mV")

    six__more_LUREC_water_sensors_new<-six__more_LUREC_water_sensors%>%
    tail(7910)
    colnames(six__more_LUREC_water_sensors_new)<-c("Timestamp", "Pot_7_Water_Content_proportion", "Pot_8_Water_Content_proportion", "Pot_9_Water_Content_proportion", "Pot_10_Water_Content_proportion", "Pot_11_Water_Content_proportion", "Pot_12_Water_Content_proportion", "Battery_percent", "Battery_voltage_mV")

    rm(six_LUREC_water_sensor_data, six__more_LUREC_water_sensors)

    six_LUREC_water_sensor_data_new<-six_LUREC_water_sensor_data_new%>%
    mutate(Pot_1_Water_Content_proportion=as.numeric(Pot_1_Water_Content_proportion))%>%
    mutate(Pot_2_Water_Content_proportion=as.numeric(Pot_2_Water_Content_proportion))%>%
    mutate(Pot_3_Water_Content_proportion=as.numeric(Pot_3_Water_Content_proportion))%>%
    mutate(Pot_4_Water_Content_proportion=as.numeric(Pot_4_Water_Content_proportion))%>%
    mutate(Pot_5_Water_Content_proportion=as.numeric(Pot_5_Water_Content_proportion))%>%
    mutate(Pot_6_Water_Content_proportion=as.numeric(Pot_6_Water_Content_proportion))

    six_more_LUREC_water_sensors_new<-six__more_LUREC_water_sensors_new%>%
      mutate(Pot_7_Water_Content_proportion=as.numeric(Pot_7_Water_Content_proportion))%>%
      mutate(Pot_8_Water_Content_proportion=as.numeric(Pot_8_Water_Content_proportion))%>%
      mutate(Pot_9_Water_Content_proportion=as.numeric(Pot_9_Water_Content_proportion))%>%
      mutate(Pot_10_Water_Content_proportion=as.numeric(Pot_10_Water_Content_proportion))%>%
      mutate(Pot_11_Water_Content_proportion=as.numeric(Pot_11_Water_Content_proportion))%>%
      mutate(Pot_12_Water_Content_proportion=as.numeric(Pot_12_Water_Content_proportion))

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `Pot_7_Water_Content_proportion =
    ##   as.numeric(Pot_7_Water_Content_proportion)`.
    ## Caused by warning:
    ## ! NAs introduced by coercion

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `Pot_10_Water_Content_proportion =
    ##   as.numeric(Pot_10_Water_Content_proportion)`.
    ## Caused by warning:
    ## ! NAs introduced by coercion

    rm(six__more_LUREC_water_sensors_new)

    six_LUREC_water_sensor_data_new<-six_LUREC_water_sensor_data_new%>%
    tail(7915)
    Combined_LUREC_water_data<-six_LUREC_water_sensor_data_new%>%
    left_join(six_more_LUREC_water_sensors_new, by=c("Timestamp"="Timestamp"))

I then investigated the distribution of each pot’s water content.

    Averaged_LUREC_water_data<-Combined_LUREC_water_data%>%
    mutate(Timestamp=mdy_hm(Timestamp))%>%
    mutate(Day=format(Timestamp, format= "%d"), Month=format(Timestamp, format="%m"), Year=format(Timestamp, format="%y"))%>%
    mutate(Date=str_c(Month, "/", Day, "/", Year))%>%
    mutate(Date=mdy(Date))%>%
    group_by(Date)%>%
    summarize(Pot_1_mean_water_content=mean(Pot_1_Water_Content_proportion), Pot_2_mean_water_content=mean(Pot_2_Water_Content_proportion), Pot_3_mean_water_content=mean(Pot_3_Water_Content_proportion), Pot_4_mean_water_content=mean(Pot_4_Water_Content_proportion), Pot_5_mean_water_content=mean(Pot_5_Water_Content_proportion), Pot_6_mean_water_content=mean(Pot_6_Water_Content_proportion), Pot_7_mean_water_content=mean(Pot_7_Water_Content_proportion), Pot_8_mean_water_content=mean(Pot_8_Water_Content_proportion), Pot_9_mean_water_content=mean(Pot_9_Water_Content_proportion), Pot_10_mean_water_content=mean(Pot_10_Water_Content_proportion), Pot_11_mean_water_content=mean(Pot_11_Water_Content_proportion), Pot_12_mean_water_content=mean(Pot_12_Water_Content_proportion))    

    Water_content_precipitation_combined<-Averaged_LUREC_water_data%>%
    left_join(Woodstock_combined_weather_data, by=c("Date"="Date"))%>%
    mutate(Avg_MWC=(Pot_1_mean_water_content+Pot_2_mean_water_content+Pot_3_mean_water_content+Pot_4_mean_water_content+Pot_5_mean_water_content+Pot_6_mean_water_content+Pot_7_mean_water_content+Pot_8_mean_water_content+Pot_9_mean_water_content+Pot_10_mean_water_content+Pot_11_mean_water_content+Pot_12_mean_water_content)/12)%>%
    select(-17)

    colnames(Water_content_precipitation_combined)=c("Date", "Pot_1_MWC", "Pot_2_MWC", "Pot_3_MWC", "Pot_4_MWC", "Pot_5_MWC", "Pot_6_MWC", "Pot_7_MWC", "Pot_8_MWC", "Pot_9_MWC", "Pot_10_MWC", "Pot_11_MWC", "Pot_12_MWC","PRCP_x", "PRCP_y", "PRCP_z")
    Water_content_precipitation_combined<-Water_content_precipitation_combined%>%
    mutate(Avg_precip=(PRCP_x+PRCP_y+PRCP_z)/3)

    Combined_LUREC_water_data<-six_LUREC_water_sensor_data_new%>%
    tail(7915)%>%
    left_join(six_more_LUREC_water_sensors_new, by=c("Timestamp"="Timestamp"))

    Combined_LUREC_water_data_pivoted<-Combined_LUREC_water_data%>%
    select(-Battery_percent.x, -Battery_voltage_mV.x, -Battery_percent.y, -Battery_voltage_mV.y)%>%
    pivot_longer(cols=-Timestamp, names_to="Pot", values_to="Water_content")

    Combined_LUREC_water_data_pivoted$Pot<-rep(1:12, 7915)
    Combined_LUREC_water_data_pivoted$Pot<-as.factor(Combined_LUREC_water_data_pivoted$Pot)

    Pot_water_content_boxplots<-plot_ly(data=Combined_LUREC_water_data_pivoted, color=~Pot, y=~Water_content, type="box")%>%
    layout(title="Comparative boxplots of Water Content for Each LUREC Pot", xaxis=list(title="Pot Number"), yaxis=list(title="Water content (m^3 water/m^3 soil"))
    Pot_water_content_boxplots

    ## Warning: Ignoring 2999 observations

    ## Warning in RColorBrewer::brewer.pal(max(N, 3L), "Set2"): n too large, allowed maximum for palette Set2 is 8
    ## Returning the palette you asked for with that many colors
    ## Warning in RColorBrewer::brewer.pal(max(N, 3L), "Set2"): n too large, allowed maximum for palette Set2 is 8
    ## Returning the palette you asked for with that many colors

![](LUREC_project_code_files/figure-markdown_strict/unnamed-chunk-6-1.png)

Creating this boxplot made it visually apparent that pots 5 and 8
contained negative values. Since soil moisture content is measured as a
proportion of cubic meters of water per cubic meter of soil, negative
moisture readings make no sense.

I then investigated when these negative readings occured.

    Pot_5_negative_values<-Combined_LUREC_water_data%>%
    select(Timestamp, Pot_5_Water_Content_proportion)%>%
    filter(Pot_5_Water_Content_proportion<0)

    Pot_8_negative_values<-Combined_LUREC_water_data%>%
    select(Timestamp, Pot_8_Water_Content_proportion)%>%
    filter(Pot_8_Water_Content_proportion<0)

The sensor in Pot 5 recorded negative values for water content at each
hour mark from 1/21/25 at 9:00 AM to 1/22/25 at 7:00 PM. The sensor in
Pot 8 recorded negative values for water content at each hour mark from
10:00 AM to 5:00 PM on 1/21/26. The negative values then discontinue and
begin again from 12:00 AM to 5:00 PM on 1/22/25.

Based on this information and the locations of large clusters of NA
values between the months of November and January for 2 of the pots, I
decided to remove all observations at or before 2/3/25, after averaging
the observations by day. This was the dataset used for all subsequent
analyses.

    Averaged_LUREC_water_data<-Combined_LUREC_water_data%>%
    mutate(Timestamp=mdy_hm(Timestamp))%>%
    mutate(Day=format(Timestamp, format= "%d"), Month=format(Timestamp, format="%m"), Year=format(Timestamp, format="%y"))%>%
    mutate(Date=str_c(Month, "/", Day, "/", Year))%>%
    mutate(Date=mdy(Date))%>%
    group_by(Date)%>%
    summarize(Pot_1_mean_water_content=mean(Pot_1_Water_Content_proportion), Pot_2_mean_water_content=mean(Pot_2_Water_Content_proportion), Pot_3_mean_water_content=mean(Pot_3_Water_Content_proportion), Pot_4_mean_water_content=mean(Pot_4_Water_Content_proportion), Pot_5_mean_water_content=mean(Pot_5_Water_Content_proportion), Pot_6_mean_water_content=mean(Pot_6_Water_Content_proportion), Pot_7_mean_water_content=mean(Pot_7_Water_Content_proportion), Pot_8_mean_water_content=mean(Pot_8_Water_Content_proportion), Pot_9_mean_water_content=mean(Pot_9_Water_Content_proportion), Pot_10_mean_water_content=mean(Pot_10_Water_Content_proportion), Pot_11_mean_water_content=mean(Pot_11_Water_Content_proportion), Pot_12_mean_water_content=mean(Pot_12_Water_Content_proportion)) 

    Water_content_precipitation_combined<-Averaged_LUREC_water_data%>%
    left_join(Woodstock_combined_weather_data, by=c("Date"="Date"))%>%
    mutate(Avg_MWC=(Pot_1_mean_water_content+Pot_2_mean_water_content+Pot_3_mean_water_content+Pot_4_mean_water_content+Pot_5_mean_water_content+Pot_6_mean_water_content+Pot_7_mean_water_content+Pot_8_mean_water_content+Pot_9_mean_water_content+Pot_10_mean_water_content+Pot_11_mean_water_content+Pot_12_mean_water_content)/12)

    Useful_LUREC_averaged_data<-Water_content_precipitation_combined%>%
    filter(Date > "2025-02-03")

I then created a three-axis time series graph displaying daily averaged
water content against the daily precipitation and maximum temperature at
the Crystal Lake 4 NW weather station (the only weather station near the
field site with both temperature and precipitation data for 2025). Data
for this weather station was downloaded from the same website as the
three Woodstock stations.

    library(tidyverse)
    Crystal_Lake_temperature_data<-read_csv("~/Desktop/Ghatak_research_project/Crystal_Lake_Temperature_data.csv")

    ## Warning: One or more parsing issues, call `problems()` on your data frame for details,
    ## e.g.:
    ##   dat <- vroom(...)
    ##   problems(dat)

    ## Rows: 12584 Columns: 7
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Column1
    ## dbl (5): Column3, Column4, Column5, Column6, Column7
    ## lgl (1): Column2
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    colnames(Crystal_Lake_temperature_data)<-c("Date", "Average_Temp_F", "Max_Temp_F", "Min_Temp_F", "PRCP_inches", "Snow_inches", "SNWD_inches")
    Crystal_Lake_temperature_data<-Crystal_Lake_temperature_data%>%
    tail(12582)%>%
    mutate(Date=ymd(Date))%>%
    mutate(Year=year(Date), Month=month(Date), Day=day(Date))%>%
    filter(Year==2025)%>%
    tail(417)%>%
    left_join(Averaged_LUREC_water_data, by=c("Date"="Date"))%>%
    filter(Date<"2025-10-07" & Date>"2025-02-03")%>%
    select(-2)%>%
    mutate(Avg_Temp_F=(Max_Temp_F+Min_Temp_F)/2)


    Combined_LUREC_water_data_new<-Combined_LUREC_water_data_pivoted%>%
    mutate(Timestamp=mdy_hm(Timestamp))%>%
    mutate(Day=format(Timestamp, format= "%d"), Month=format(Timestamp, format="%m"), Year=format(Timestamp, format="%y"))%>%
    mutate(Date=str_c(Month, "/", Day, "/", Year))%>%
    mutate(Date=mdy(Date))%>%
    group_by(Date)%>%
    summarize(Average_water_content=mean(Water_content))

    Crystal_Lake_temperature_data<-Crystal_Lake_temperature_data%>%
    left_join(Water_content_precipitation_combined, by=c("Date"="Date"))%>%
    mutate(Avg_precip=(SW_0.7_Precip+SW_3.8_Precip+NW_5_Precip)/3)%>%
    select(c(1,2,3,38,39))

    colnames(Crystal_Lake_temperature_data)<-c("Date", "Max_Temp_F", "Min_Temp_F", "MWC", "Avg_precip")

    Combined_Precip_water_content_max_temp_plot<-plot_ly(data=Crystal_Lake_temperature_data, x=~Date, y=~MWC, name="Mean Water Content(Left)", type="scatter", mode="lines+markers")%>%
      add_trace(x=~Date, y=~Avg_precip, yaxis="y2", name="Average Precip in Inches(Right)", type="bar")%>%  
      add_trace(x=~Date, y=~Max_Temp_F, yaxis="y3",  name="Crystal Lake 4 NW Max temp (Middle)", type="scatter", mode="lines+markers")%>%
      layout(title="Time Series of Mean Water Content, Maximum temperature and precipitation, averaged by day", xaxis=list(title="Date"), yaxis=list(title="Mean Water Content", side="left"), yaxis2=list(title="Crystal Lake precipitation", overlaying="y", side="right"), yaxis3=list(title="Maximum temperature", overlaying="y", position=0.5))
    Combined_Precip_water_content_max_temp_plot

    ## Warning: Ignoring 41 observations

    ## Warning: 'bar' objects don't have these attributes: 'mode'
    ## Valid attributes include:
    ## '_deprecated', 'alignmentgroup', 'base', 'basesrc', 'cliponaxis', 'constraintext', 'customdata', 'customdatasrc', 'dx', 'dy', 'error_x', 'error_y', 'hoverinfo', 'hoverinfosrc', 'hoverlabel', 'hovertemplate', 'hovertemplatesrc', 'hovertext', 'hovertextsrc', 'ids', 'idssrc', 'insidetextanchor', 'insidetextfont', 'legend', 'legendgroup', 'legendgrouptitle', 'legendrank', 'legendwidth', 'marker', 'meta', 'metasrc', 'name', 'offset', 'offsetgroup', 'offsetsrc', 'opacity', 'orientation', 'outsidetextfont', 'selected', 'selectedpoints', 'showlegend', 'stream', 'text', 'textangle', 'textfont', 'textposition', 'textpositionsrc', 'textsrc', 'texttemplate', 'texttemplatesrc', 'transforms', 'type', 'uid', 'uirevision', 'unselected', 'visible', 'width', 'widthsrc', 'x', 'x0', 'xaxis', 'xcalendar', 'xhoverformat', 'xperiod', 'xperiod0', 'xperiodalignment', 'xsrc', 'y', 'y0', 'yaxis', 'ycalendar', 'yhoverformat', 'yperiod', 'yperiod0', 'yperiodalignment', 'ysrc', 'key', 'set', 'frame', 'transforms', '_isNestedKey', '_isSimpleKey', '_isGraticule', '_bbox'

![](LUREC_project_code_files/figure-markdown_strict/unnamed-chunk-9-1.png)

This graph suggested very rapid water infiltration, owing to the very
small time delays ( usually &lt;= 1 day) between precipitation events
and increases in mean water content. It appears that, at certain points
in the months of February and March, mean water content increases
without precipitation, but this is partially due to missingness in the
precipitation data during those months.

At this point, the relationship between maximum temperature and mean
water content was uncertain and was investigated as part of the multiple
linear regression model subsequently generated. The model regressed
daily change in mean water content against daily maximum temperature
from the Crystal Lake 4 NW weather station and the average of daily
precipitation from Crystal Lake 4 NW, Woodstock 5 NW, and Woodstock 3.8
SW.

First, the linearity assumption was assessed using a 3D scatterplot
since two explanatory variables were involved.

    Crystal_Lake_temperature_data<-Crystal_Lake_temperature_data%>%
    mutate(Avg_temp_F=(Min_Temp_F+Max_Temp_F)/2)%>%
    mutate(MWC_daily_change=MWC-lag(MWC))

    Precip_plot<-plot_ly(Crystal_Lake_temperature_data, x=~Max_Temp_F, y=~Avg_precip, z=~MWC_daily_change)%>%
      add_markers()
    Precip_plot

    ## Warning: Ignoring 108 observations

![](LUREC_project_code_files/figure-markdown_strict/unnamed-chunk-10-1.png)

With the linearity assumption apparently satisfied, I generated the
regression plane, checked the homoscedastictiy and residual normality
assumptions, and then added the regression plane to the scatterplot.

    MLR_model<-lm(MWC_daily_change~Max_Temp_F+Avg_precip, data=Crystal_Lake_temperature_data)
    summary(MLR_model)

    ## 
    ## Call:
    ## lm(formula = MWC_daily_change ~ Max_Temp_F + Avg_precip, data = Crystal_Lake_temperature_data)
    ## 
    ## Residuals:
    ##        Min         1Q     Median         3Q        Max 
    ## -0.0212069 -0.0017108 -0.0000999  0.0013923  0.0215998 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -3.309e-03  1.859e-03  -1.780   0.0773 .  
    ## Max_Temp_F   5.422e-06  2.633e-05   0.206   0.8372    
    ## Avg_precip   1.945e-02  1.582e-03  12.301   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.005763 on 134 degrees of freedom
    ##   (108 observations deleted due to missingness)
    ## Multiple R-squared:  0.5356, Adjusted R-squared:  0.5287 
    ## F-statistic: 77.28 on 2 and 134 DF,  p-value: < 2.2e-16

    plot(MLR_model, 1)

![](LUREC_project_code_files/figure-markdown_strict/unnamed-chunk-11-1.png)

    plot(MLR_model, 2)

![](LUREC_project_code_files/figure-markdown_strict/unnamed-chunk-11-2.png)
Residuals generally appear scattered around zero. It is difficult to
gauge if a fan shape is present because the precipitation data is very
positively skewed. As I discuss later, this is one of the model’s
primary limitations.

    x_seq<-seq(min(Crystal_Lake_temperature_data$Max_Temp_F, na.rm=TRUE), max(Crystal_Lake_temperature_data$Max_Temp_F, na.rm=TRUE), length.out=30)
    y_seq<-seq(min(Crystal_Lake_temperature_data$Avg_precip, na.rm=TRUE), max(Crystal_Lake_temperature_data$Avg_precip, na.rm=TRUE), length.out=30)
    grid<-expand.grid(Avg_precip=y_seq, Max_Temp_F=x_seq)
    grid$MWC_daily_change<-predict(MLR_model, newdata=grid)
    z_matrix<-matrix(grid$MWC_daily_change, nrow=length(y_seq), ncol=length(x_seq), byrow=FALSE)

    Precip_plot <- plot_ly() %>%
      add_markers(data = Crystal_Lake_temperature_data,
                  x = ~Max_Temp_F,
                  y = ~Avg_precip,
                  z = ~MWC_daily_change,
                  marker = list(size = 3)) %>%
      add_surface(x = x_seq,
                  y = y_seq,
                  z = z_matrix,
                  opacity = 0.6)

    Precip_plot

    ## Warning: Ignoring 108 observations

![](LUREC_project_code_files/figure-markdown_strict/unnamed-chunk-12-1.png)

There is a moderate violation of residual normality, with the normal
qqplot appearing linear between -1 and 1 standard deviations of the mean
of MWC\_daily\_change but exhibiting heavy tails on both ends. This
might be alleviated by investigating the three potentially influential
observations (observations 11, 77, and 121) using Jackknife residuals,
testing if each observation is actually influential at alpha=0.01.

    MWC_model_Jackknife_residuals<-rstudent(MLR_model)
    Crystal_Lake_temperature_data$jackknife_resid <- NA
    used_rows <- as.numeric(rownames(model.frame(MLR_model)))
    Crystal_Lake_temperature_data$jackknife_resid[used_rows] <- MWC_model_Jackknife_residuals

    n=nrow(Crystal_Lake_temperature_data)
    k=3
    p_value_11<-2*pt(abs(Crystal_Lake_temperature_data$jackknife_resid[11]), df=n-k-1, lower.tail=FALSE)

    p_value_77<-2*pt(abs(Crystal_Lake_temperature_data$jackknife_resid[77]), df=n-k-1, lower.tail=FALSE)

    p_value_121<-2*pt(abs(Crystal_Lake_temperature_data$jackknife_resid[121]), df=n-k-1, lower.tail=FALSE)

    p_value_11

    ## [1] 8.523704e-05

    p_value_77

    ## [1] 0.0001690765

    p_value_121

    ## [1] 3.607888e-05

All three observations are statistically influential at alpha=0.01, so I
will remove the observation with the highest absolute Jackknife residual
value and regenerate the model without that observation.

    Crystal_Lake_temperature_data$jackknife_resid[11]

    ## [1] -3.997206

    Crystal_Lake_temperature_data$jackknife_resid[77]

    ## [1] 3.821172

    Crystal_Lake_temperature_data$jackknife_resid[121]

    ## [1] 4.209982

    Crystal_Lake_temperature_data_2<-Crystal_Lake_temperature_data[-121, ]
    new_MLR_model<-lm(MWC_daily_change~Max_Temp_F+Avg_precip, data=Crystal_Lake_temperature_data_2)
    summary(new_MLR_model)

    ## 
    ## Call:
    ## lm(formula = MWC_daily_change ~ Max_Temp_F + Avg_precip, data = Crystal_Lake_temperature_data_2)
    ## 
    ## Residuals:
    ##        Min         1Q     Median         3Q        Max 
    ## -0.0214709 -0.0016659 -0.0000234  0.0013919  0.0217460 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -2.965e-03  1.755e-03  -1.690   0.0934 .  
    ## Max_Temp_F   1.181e-06  2.484e-05   0.048   0.9622    
    ## Avg_precip   1.740e-02  1.569e-03  11.094   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.005434 on 133 degrees of freedom
    ##   (108 observations deleted due to missingness)
    ## Multiple R-squared:  0.4839, Adjusted R-squared:  0.4761 
    ## F-statistic: 62.35 on 2 and 133 DF,  p-value: < 2.2e-16

    plot(new_MLR_model, 1)

![](LUREC_project_code_files/figure-markdown_strict/unnamed-chunk-14-1.png)

    plot(new_MLR_model, 2)

![](LUREC_project_code_files/figure-markdown_strict/unnamed-chunk-14-2.png)
Removing the single influential observation did not singificantly
improve the residual normality. Nevertheless, I will continue testing
and removing influential observations one at a time until none remain.

    Jackknife_residuals_2<-rstudent(new_MLR_model)

    Crystal_Lake_temperature_data_2$jackknife_resid <- NA
    used_rows <- as.numeric(rownames(model.frame(new_MLR_model)))
    Crystal_Lake_temperature_data_2$jackknife_resid[used_rows] <- Jackknife_residuals_2

    n2=nrow(Crystal_Lake_temperature_data_2)
    k2=3
    p_value_11_new<-2*pt(abs(Crystal_Lake_temperature_data_2$jackknife_resid[11]), df=n2-k2-1, lower.tail=FALSE)
    p_value_25_new<-2*pt(abs(Crystal_Lake_temperature_data_2$jackknife_resid[25]), df=n2-k2-1, lower.tail=FALSE)
    p_value_77_new<-2*pt(abs(Crystal_Lake_temperature_data_2$jackknife_resid[77]), df=n2-k2-1, lower.tail=FALSE)

    p_value_11_new

    ## [1] 2.149168e-05

    p_value_25_new

    ## [1] 0.0005842025

    p_value_77_new

    ## [1] 2.226054e-05

    #All 3 potnetially influential observations are statistically influential

    Crystal_Lake_temperature_data_2$jackknife_resid[11]

    ## [1] -4.33462

    Crystal_Lake_temperature_data_2$jackknife_resid[25]

    ## [1] 3.485347

    Crystal_Lake_temperature_data_2$jackknife_resid[77]

    ## [1] 4.326273

Jackknife residual 11 from the dataset with one observation removed has
the highest absolute residual value, so I will remove it and regenerate
the model again.

    Crystal_Lake_temperature_data_3<-Crystal_Lake_temperature_data_2[-11, ]
    MLR_model_v3<-lm(MWC_daily_change~Max_Temp_F+Avg_precip, data=Crystal_Lake_temperature_data_3)
    summary(MLR_model_v3)

    ## 
    ## Call:
    ## lm(formula = MWC_daily_change ~ Max_Temp_F + Avg_precip, data = Crystal_Lake_temperature_data_3)
    ## 
    ## Residuals:
    ##        Min         1Q     Median         3Q        Max 
    ## -0.0184312 -0.0016873 -0.0001627  0.0013221  0.0213037 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -1.202e-03  1.697e-03  -0.708    0.480    
    ## Max_Temp_F  -2.206e-05  2.394e-05  -0.921    0.359    
    ## Avg_precip   1.737e-02  1.473e-03  11.790   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.005104 on 132 degrees of freedom
    ##   (108 observations deleted due to missingness)
    ## Multiple R-squared:  0.5131, Adjusted R-squared:  0.5057 
    ## F-statistic: 69.55 on 2 and 132 DF,  p-value: < 2.2e-16

    plot(MLR_model_v3, 1)

![](LUREC_project_code_files/figure-markdown_strict/unnamed-chunk-16-1.png)

    plot(MLR_model_v3, 2)

![](LUREC_project_code_files/figure-markdown_strict/unnamed-chunk-16-2.png)

    Jackknife_residuals_3<-rstudent(MLR_model_v3)

    Crystal_Lake_temperature_data_3$jackknife_resid <- NA
    used_rows <- as.numeric(rownames(model.frame(MLR_model_v3)))
    Crystal_Lake_temperature_data_3$jackknife_resid[used_rows] <- Jackknife_residuals_3

    n3=nrow(Crystal_Lake_temperature_data_3)
    k3=3
    p_value_10_new<-2*pt(abs(Crystal_Lake_temperature_data_3$jackknife_resid[10]), df=n3-k3-1, lower.tail=FALSE)
    p_value_24_new<-2*pt(abs(Crystal_Lake_temperature_data_3$jackknife_resid[24]), df=n3-k3-1, lower.tail=FALSE)
    p_value_76_new<-2*pt(abs(Crystal_Lake_temperature_data_3$jackknife_resid[76]), df=n3-k3-1, lower.tail=FALSE)

    p_value_10_new

    ## [1] 0.000129284

    p_value_24_new

    ## [1] 0.0004091711

    p_value_76_new

    ## [1] 8.730041e-06

    Crystal_Lake_temperature_data_3$jackknife_resid[10]

    ## [1] -3.891415

    Crystal_Lake_temperature_data_3$jackknife_resid[24]

    ## [1] 3.584605

    Crystal_Lake_temperature_data_3$jackknife_resid[76]

    ## [1] 4.544929

Observation 76 from the dataset with two influential observations
removed has the highest abolsute residual value, so I will remove it and
regenerate the model for the third time.
