# splunk
Splunk querys

## Basic Search Text
We can pass text of search

```splunk
index=kubernetes_azure namespace="api" sourcetype=ms-example-util "Example Text"
```

| Time                       | Event                                |
|----------------------------|--------------------------------------|
| 07/11/2024 12:38:28.417 PM | 2024-11-07 00:38:28.417 Example Text |

## Get Field And Values Of Log Using Rex
We can get fields and values using the rex function

```splunk
