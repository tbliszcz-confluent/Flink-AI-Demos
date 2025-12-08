This example shows you how you can use the ML_FORECAST() function within Flink ML tools to use an ARIMA model to forecast and predict the future. ARIMA is a common statistic model used to forecast time series. For more information around the actual model please see: 

In the demo we will have time series of stock prices, and have the ARIMA model attempt to predict the next few values. The prices are more predictable than stock prices are, but this is intentional to show how ARIMA model prediction works. 

All of these commands are made to be ran inside a Flink SQL compute pool connected to a Kafka Cluster in Confluent Cloud 

1. Create the table we will use to insert the time series data. We are using a TIMESTAMP(3) value for the times and a DECIMAL value type for the values (val): 

```
CREATE TABLE stock_prices (
  times TIMESTAMP(3),
  val DECIMAL,
  WATERMARK FOR times AS times
);
```
2. Insert the stock price data into the table:

```
INSERT INTO stock_prices (times, val) VALUES
(CAST('2026-01-01 12:00:00.000' AS TIMESTAMP(3)), 182.00),
(CAST('2026-01-02 12:00:00.000' AS TIMESTAMP(3)), 183.21),
(CAST('2026-01-03 12:00:00.000' AS TIMESTAMP(3)), 181.89),
(CAST('2026-01-04 12:00:00.000' AS TIMESTAMP(3)), 184.67),
(CAST('2026-01-05 12:00:00.000' AS TIMESTAMP(3)), 182.33),
(CAST('2026-01-06 12:00:00.000' AS TIMESTAMP(3)), 185.78),
(CAST('2026-01-07 12:00:00.000' AS TIMESTAMP(3)), 183.12),
(CAST('2026-01-08 12:00:00.000' AS TIMESTAMP(3)), 186.45),
(CAST('2026-01-09 12:00:00.000' AS TIMESTAMP(3)), 184.01),
(CAST('2026-01-10 12:00:00.000' AS TIMESTAMP(3)), 187.33),
(CAST('2026-01-11 12:00:00.000' AS TIMESTAMP(3)), 185.67),
(CAST('2026-01-12 12:00:00.000' AS TIMESTAMP(3)), 188.12),
(CAST('2026-01-13 12:00:00.000' AS TIMESTAMP(3)), 186.78),
(CAST('2026-01-14 12:00:00.000' AS TIMESTAMP(3)), 189.44),
(CAST('2026-01-15 12:00:00.000' AS TIMESTAMP(3)), 187.01),
(CAST('2026-01-16 12:00:00.000' AS TIMESTAMP(3)), 190.56),
(CAST('2026-01-17 12:00:00.000' AS TIMESTAMP(3)), 188.23),
(CAST('2026-01-18 12:00:00.000' AS TIMESTAMP(3)), 191.78),
(CAST('2026-01-19 12:00:00.000' AS TIMESTAMP(3)), 189.34),
(CAST('2026-01-20 12:00:00.000' AS TIMESTAMP(3)), 192.89),
(CAST('2026-01-21 12:00:00.000' AS TIMESTAMP(3)), 190.45),
(CAST('2026-01-22 12:00:00.000' AS TIMESTAMP(3)), 194.01),
(CAST('2026-01-23 12:00:00.000' AS TIMESTAMP(3)), 191.56),
(CAST('2026-01-24 12:00:00.000' AS TIMESTAMP(3)), 195.12),
(CAST('2026-01-25 12:00:00.000' AS TIMESTAMP(3)), 192.67),
(CAST('2026-01-26 12:00:00.000' AS TIMESTAMP(3)), 196.23),
(CAST('2026-01-27 12:00:00.000' AS TIMESTAMP(3)), 193.78),
(CAST('2026-01-28 12:00:00.000' AS TIMESTAMP(3)), 197.34),
(CAST('2026-01-29 12:00:00.000' AS TIMESTAMP(3)), 194.89),
(CAST('2026-01-30 12:00:00.000' AS TIMESTAMP(3)), 198.45),
(CAST('2026-01-31 12:00:00.000' AS TIMESTAMP(3)), 196.00),
(CAST('2026-02-01 12:00:00.000' AS TIMESTAMP(3)), 199.56),
(CAST('2026-02-02 12:00:00.000' AS TIMESTAMP(3)), 197.11),
(CAST('2026-02-03 12:00:00.000' AS TIMESTAMP(3)), 200.67),
(CAST('2026-02-04 12:00:00.000' AS TIMESTAMP(3)), 198.22),
(CAST('2026-02-05 12:00:00.000' AS TIMESTAMP(3)), 201.78),
(CAST('2026-02-06 12:00:00.000' AS TIMESTAMP(3)), 199.33),
(CAST('2026-02-07 12:00:00.000' AS TIMESTAMP(3)), 202.89),
(CAST('2026-02-08 12:00:00.000' AS TIMESTAMP(3)), 200.44),
(CAST('2026-02-09 12:00:00.000' AS TIMESTAMP(3)), 204.00),
(CAST('2026-02-10 12:00:00.000' AS TIMESTAMP(3)), 201.55),
(CAST('2026-02-11 12:00:00.000' AS TIMESTAMP(3)), 205.11),
(CAST('2026-02-12 12:00:00.000' AS TIMESTAMP(3)), 202.66),
(CAST('2026-02-13 12:00:00.000' AS TIMESTAMP(3)), 206.22),
(CAST('2026-02-14 12:00:00.000' AS TIMESTAMP(3)), 203.77),
(CAST('2026-02-15 12:00:00.000' AS TIMESTAMP(3)), 207.33),
(CAST('2026-02-16 12:00:00.000' AS TIMESTAMP(3)), 204.88),
(CAST('2026-02-17 12:00:00.000' AS TIMESTAMP(3)), 208.44),
(CAST('2026-02-18 12:00:00.000' AS TIMESTAMP(3)), 205.99);
```

Run the ARIMA forecast model. 

```
SELECT
    ML_FORECAST(val, times, JSON_OBJECT('minTrainingSize' VALUE 10, 'enableStl' VALUE false))
    OVER (
        ORDER BY times
        RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS forecast
FROM stock_prices;

```

Resources: 
https://docs.confluent.io/cloud/current/ai/builtin-functions/overview.html
