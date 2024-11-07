# splunk
Splunk querys

## Basic Search Text
We can pass text of search

### Example Log

```bash
2024-11-07 00:38:28.417 Example Text
```

### Query

```splunk
index=kubernetes_azure namespace="api" sourcetype=ms-example-util "Example Text"
```

### Result

| Time                       | Event                                |
|----------------------------|--------------------------------------|
| 07/11/2024 12:38:28.417 PM | 2024-11-07 00:38:28.417 Example Text |

## Search And Exclude Logs
We can exclude logs using NOT

### Example Log

```bash
2024-11-07 00:38:28.417 Example Text
2024-11-07 00:38:28.417 Example Text2
```

### Query

```splunk
index=kubernetes_azure namespace="api" sourcetype=ms-example-util "Example Text" AND NOT "Example Text2"
```

### Result

| Time                       | Event                                |
|----------------------------|--------------------------------------|
| 07/11/2024 12:38:28.417 PM | 2024-11-07 00:38:28.417 Example Text |


## Get Field And Values Of Log Using Rex
We can get fields and values using the rex function

### Example Log

```bash
2024-11-07 00:38:28.417 ExampleDto(id=1, name=Cat)
```

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

### Example Log

```bash
2024-11-07 00:38:28.417 ExampleDto(id=1, name=Cat)
```

### Query

```splunk
index=kubernetes_azure namespace="api" sourcetype=ms-example-util "ExampleDto" | rex "ExampleDto\(id=(?<id>[0-9]+), name=(?<name>[aA-zZ ]+)) | table id, name"
```

### Result

| id | name |
|----|------|
| 1  | Cat  |

## Remove Duplicate Values
We can get fields and values using the rex function and exclude repeat values and generate table by these fields and values

### Example Log

```bash
2024-11-07 00:38:28.417 BEGIN ExampleDto(id=1, name=Cat)
2024-11-07 00:38:29.417 END ExampleDto(id=1, name=Cat)
2024-11-07 00:39:28.417 BEGIN ExampleDto(id=2, name=CatDog)
2024-11-07 00:39:29.417 END ExampleDto(id=2, name=CatDog)
```

### Query

```splunk
index=kubernetes_azure namespace="api" sourcetype=ms-example-util "ExampleDto" | rex "ExampleDto\(id=(?<id>[0-9]+), name=(?<name>[aA-zZ ]+))" | dedup id | table id, name
```

### Result

| id | name   |
|----|--------|
| 1  | Cat    |
| 2  | CatDog |

## Generate Stats By Field
We can get fields and values using the rex function and generate stats by field

### Example Log

```bash
2024-11-07 00:38:28.417 BEGIN ExampleDto(id=1, name=Cat)
2024-11-07 00:38:29.417 END ExampleDto(id=1, name=Cat)
```

### Query

```splunk
index=kubernetes_azure namespace="api" sourcetype=ms-example-util "ExampleDto" | rex "ExampleDto\(id=(?<id>[0-9]+), name=(?<name>[aA-zZ ]+))" | stats by id
```

### Result

| field | count |
|-------|-------|
| id    | 2     |

## Generate Stats Count By Field
We can get fields and values using the rex function and generate stats count by field

### Example Log

```bash
2024-11-07 00:38:28.417 BEGIN ExampleDto(id=1, name=Cat)
2024-11-07 00:38:29.417 END ExampleDto(id=1, name=Cat)
```

### Query

```splunk
index=kubernetes_azure namespace="api" sourcetype=ms-example-util "ExampleDto" | rex "ExampleDto\(id=(?<id>[0-9]+), name=(?<name>[aA-zZ ]+))" | stats count by name
```

### Result

| name | count |
|------|-------|
| Cat  | 2     |

## Generate Stats By Error Code And Message
We can get fields and values using the rex function and use eval function to concat error code and message and generate stats count by error

### Example Log

```bash
2024-11-07 00:38:28.417 ErrorDto(code=EXCP001, message=Error to create pet)
2024-11-07 00:38:29.417 ErrorDto(code=EXCP001, message=Error to create pet)
2024-11-07 00:38:30.417 ErrorDto(code=EXCP001, message=Error to create pet)
2024-11-07 00:38:28.417 ErrorDto(code=EXUP001, message=Error to update pet)
2024-11-07 00:38:29.417 ErrorDto(code=EXUP001, message=Error to update pet)
```

### Query

```splunk
index=kubernetes_azure namespace="api" sourcetype=ms-example-util "ErrorDto" | rex "ErrorDto\(code=(?<code>aA-zZ0-9]+), message=(?<message>[aA-zZ0-9 ]+))" | eval error=code+" : "+message | stats count by error
```

### Result

| error                           | count |
|---------------------------------|-------|
| EXCP001 : Error to create pet   | 3     |
| EXUP001 : Error to update pet   | 2     |
