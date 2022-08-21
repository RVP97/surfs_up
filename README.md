# Surfs Up

## Overview of the Analysis

The purpose of the analysis was to provide W. Avy and the board of directors with processed data about the temperatures in Oahu to support the decision to open a new surf shop. Specifically, there will be a direct comparison between the results of the months of December and June to better understand if the surf shop and ice cream business is sustainable all year.

## Results

- The weather data from June is not severely different than the one from December; although there are more weather observations in June.
- There were **1,700** observations in June and **1,517** in December, which might explain why the standard deviation is slightly higher in December,
- The average temperature per day is also quite similar with only a **3.9** degree difference.
- Even though the temperatures are similar, it is important to note that the range of these is higher in December. The lowest was **56** while the highest **83**. In contrast, June has a range of only **21**.
- The following image will be able to give a clear snapshot of the main differences between the summary statistics of June and December:

![CleanShot 2022-08-21 at 09 47 42](https://user-images.githubusercontent.com/85131345/185796823-8534d485-258f-4767-9b4e-2aaf5fe01c7e.png)

- There are more anomalies in December, especially for cold days than there are in June:
  ![CleanShot 2022-08-21 at 09 51 52](https://user-images.githubusercontent.com/85131345/185797010-b39df4d0-6126-45ee-ae39-dd368b164da8.png)
  ![CleanShot 2022-08-21 at 09 52 13](https://user-images.githubusercontent.com/85131345/185797030-aa4a9ee5-ca0b-4a9d-b871-0a0587cb1698.png)

## Summary

- The analysis provided a clear sense of the current situation regarding the weather in Oahu. The decision in opening a new shop lies in how much the temperature varies from season to season.
- The temperature in December is certainly more volatile, having a higher standard deviation and more anomalies.
- Half of the days in December have a temperature lower than **71** degrees, which might be considered not that desirable for ice cream.
- On the other hand, the average temperature is pretty in line with June and is still hot enough to have great surf days and good for the ice cream business.
- In general, the temperature behavior is quite similar and it would be wise to go ahead and open a new shop. However, a few more queries should be done to have other important data.
- It is important to query the precipitation data; the success of the shop will have a strong negative correlation with this. We can use the following code to do so:

```py
prec_dec = session.query(Measurement.prcp).filter(extract('month', Measurement.date)==12).all()
prec_dec_df = pd.DataFrame(prec_dec, columns=['December Precipitation'])
prec_june = session.query(Measurement.prcp).filter(extract('month', Measurement.date)==6).all()
prec_june_df = pd.DataFrame(prec_june, columns=['June Precipitation'])
summary_prec = prec_june_df.describe()
summary_prec['December Precipitation'] = prec_dec_df['December Precipitation'].describe()
```

The following is the resulting DataFrame:
![CleanShot 2022-08-21 at 10 08 25](https://user-images.githubusercontent.com/85131345/185797717-d8f29758-67ea-48eb-b092-551e3f349453.png)

- The other query I would perform is to get the temperature where there was no rain in the observation. For that I would use the following code:

```py
no_rain_june = pd.DataFrame(session.query(Measurement.tobs).filter(Measurement.prcp == 0).filter(extract('month', Measurement.date)==6).all(), columns=['No Rain June'])
no_rain_dec = pd.DataFrame(session.query(Measurement.tobs).filter(Measurement.prcp == 0).filter(extract('month', Measurement.date)==12).all(), columns=['No Rain December'])
no_rain_summary = no_rain_june.describe()
no_rain_summary['No Rain December'] = no_rain_dec['No Rain December'].describe()
```
