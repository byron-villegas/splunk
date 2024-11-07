# splunk
Splunk querys

## Basic Search Text
We can pass text of search

### Query

```splunk
index=kubernetes_azure namespace="api" sourcetype=ms-example-util "Example Text"
```

### Result

| Time                       | Event                                |
|----------------------------|--------------------------------------|
| 07/11/2024 12:38:28.417 PM | 2024-11-07 00:38:28.417 Example Text |

## Get Field And Values Of Log Using Rex
We can get fields and values using the rex function

### Query

```splunk
index=kubernetes_azure namespace="api" sourcetype=ms-example-util "ExampleDto" | rex "ExampleDto\(id=(?<id>[0-9]+), name=(?<name>[aA-zZ ]+))"
```

### Result

| Time                       | Event                                              |
|----------------------------|----------------------------------------------------|
| 07/11/2024 12:38:28.417 PM | 2024-11-07 00:38:28.417 ExampleDto(id=1, name=Cat) |

The function rex create two variables id and name, in this example the value for id are 1 and the value for name Cat


## Get Field And Values Of Log Using Rex And Make Table
We can get fields and values using the rex function and generate table by these fields and values

### Query

```splunk
index=kubernetes_azure namespace="api" sourcetype=ms-example-util "ExampleDto" | rex "ExampleDto\(id=(?<id>[0-9]+), name=(?<name>[aA-zZ ]+)) | table id, name"
```

### Result

| id | name |
|----|------|
| 1  | Cat  |
