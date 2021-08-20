# Common Query Parameters

In this section we'll discuss common patterns and rules for certain parameters that are used on many different endpoints across the API.

## `from`, `to`

> The following query would select a days worth of data in five minute chunks:

```shell
curl
    -X GET "https://api.vivacitylabs.com/v3/example-endpoint?from=2021-07-01&to=from=2021-07-02"
```

`from` and `to` are the parameters that are used to define the time range of the query. Endpoints have different maximum query ranges which vary depending on the specified `time_bucket` duration. The specifics of these maximum time ranges can be found in the documentation section for each endpoint.

**Formats:**

| Name                            | Format                     |
| ------------------------------- | -------------------------- |
| ISO UTC Time Date               | `2021-07-01`               |
| ISO UTC Time Date (Hour)        | `2021-07-01T06Z`           |
| ISO UTC Time Date (Minute)      | `2021-07-01T06:10Z`        |
| ISO UTC Time Date (Second)      | `2021-07-01T06:10:00Z`     |
| ISO UTC Time Date (Millisecond) | `2021-07-01T06:10:00.000Z` |
| Unix Timestamp                  | `1625097600`               |
| Unix Nano Timestamp             | `1625097600000000000`      |

<aside class="notice">
All times are considered UTC
</aside>

## `time_bucket`

Many requests allow or require the aggregation of data into time buckets of varying durations which can be selected by specifying the `time_bucket` parameter in the format `xs`, `xm` or `xh` (`3600s`, `60m` and `1h` all equating to an hour). Only certain time buckets durations can be used for each endpoint, these can be found in the documentation section for each endpoint.

When using the `time_bucket` parameter your 'from' parameter must align exactly with the time bucket, for example:

|                      Allowed                       | `time_bucket` | `from`                     |
| :------------------------------------------------: | ------------: | :------------------------- |
|  <img src="slate/img/correct.png" class="emoji"/>  |          `5m` | `2021-07-01T00:25Z`        |
|  <img src="slate/img/correct.png" class="emoji"/>  |         `10m` | `2021-07-01T06:40Z`        |
|  <img src="slate/img/correct.png" class="emoji"/>  |         `30m` | `2021-07-01T10:00Z`        |
|  <img src="slate/img/correct.png" class="emoji"/>  |          `1h` | `2021-07-01T12:00:00Z`     |
|  <img src="slate/img/correct.png" class="emoji"/>  |         `24h` | `2021-07-01T00:00:00.000Z` |
| <img src="slate/img/incorrect.png" class="emoji"/> |          `5m` | `2021-07-01T00:21Z`        |
| <img src="slate/img/incorrect.png" class="emoji"/> |         `30m` | `2021-07-01T10:20Z`        |
| <img src="slate/img/incorrect.png" class="emoji"/> |         `24h` | `2021-07-01T10:00Z`        |

Your requested time range must also divisible by your requested `time_bucket`, for example:

|                      Allowed                       | `time_bucket` | `from`                 | `to`                   |
| :------------------------------------------------: | ------------: | :--------------------- | :--------------------- |
|  <img src="slate/img/correct.png" class="emoji"/>  |          `5m` | `2021-07-01T00:25Z`    | `2021-07-01T00:55Z`    |
|  <img src="slate/img/correct.png" class="emoji"/>  |         `10m` | `2021-07-01T06:00Z`    | `2021-07-01T06:40Z`    |
|  <img src="slate/img/correct.png" class="emoji"/>  |         `30m` | `2021-07-01T10:00Z`    | `2021-07-01T11:30Z`    |
|  <img src="slate/img/correct.png" class="emoji"/>  |          `1h` | `2021-07-01T12:00:00Z` | `2021-07-01T21:00:00Z` |
|  <img src="slate/img/correct.png" class="emoji"/>  |         `24h` | `2021-07-01T00:00:00Z` | `2021-07-06T00:00:00Z` |
| <img src="slate/img/incorrect.png" class="emoji"/> |          `5m` | `2021-07-01T00:20Z`    | `2021-07-01T00:37Z`    |
| <img src="slate/img/incorrect.png" class="emoji"/> |         `30m` | `2021-07-01T10:30Z`    | `2021-07-01T11:20Z`    |
| <img src="slate/img/incorrect.png" class="emoji"/> |         `24h` | `2021-07-01T00:00Z`    | `2021-07-04T12:00Z`    |
