# Shared Objects
Content template은 일부 반복되는 데이터 타입을 표현하기 위해 다음과 같은 객체(Shared Objects)를 공유하여 사용합니다.

| 객체 이름            | 객체 설명                                            |
|--------------------|---------------------------------------------------|
| [ActionObject](#ActionObject)             | 클라이언트가 수행할 수 있는 동작 정보를 가지는 객체   |
| [CurrencyObject](#CurrencyObject)         | 통화 단위와 금액 정보를 가지는 객체               |
| [DateObject](#DateObject)                 | 날짜 정보를 가지는 객체                         |
| [DateTimeObject](#DateTimeObject)         | 날짜와 시간 정보를 가지는 객체                    |
| [NumberObject](#NumberObject)             | 단위 구분자(천 단위)가 처리된 숫자 정보 또는 풍속과 같이 단위가 포함된 측정 수치 정보를 가지는 객체 |
| [PercentageObject](#PercentageObject)     | 백분율 정보를 가지는 객체                        |
| [PhoneNumberObject](#PhoneNumberObject)   | 전화 번호 정보를 가지는 객체                     |
| [StringObject](#StringObject)             | 텍스트 정보를 가지는 객체                        |
| [TemperatureCObject](#TemperatureCObject) | 온도 정보(섭씨)를 가지는 객체                    |
| [TemperatureFObject](#TemperatureFObject) | 온도 정보(화씨)를 가지는 객체                    |
| [URIObject](#URIObject)                   | URI 정보를 가지는 객체                         |


## ActionObject {#ActionObject}
클라이언트가 사용자에게 제공해야 할 동작 정보를 가지는 객체입니다.

### Object fields
| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"action"` 값으로 고정되어 있습니다.  |
| `value`         | string  | [Action URI scheme](/Develop/References/ContentTemplates/Common_Fields.md#ActionURIScheme) 형태의 값 |

### Object Example
{% raw %}

```json
{
  "type": "action",
  "value": "clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search?query=이태원맛집"
}
```

{% endraw %}

## CurrencyObject {#CurrencyObject}
통화와 금액 정보를 가지는 객체입니다.

### Object fields
| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"currency"` 값으로 고정되어 있습니다.  |
| `value`         | string  | 통화와 금액이 조합된 정보               |

### Object Example
{% raw %}

```json
{
  "type": "currency",
  "value": "KRW500000"
}
```

{% endraw %}

## DateObject {#DateObject}
날짜 정보를 가지는 객체입니다.

### Object fields
| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"date"` 값으로 고정되어 있습니다.  |
| `value`         | string  | 날짜 정보. Content template 타입에 따라 `"YYYY-MM-DD"` 또는 `"YYYYMMDD"` 포맷형태로 값이 표현됩니다.    |

### Object Example
{% raw %}

```json
// 예제 1
{
  "type": "date",
  "value": "2017-05-29"
}

// 예제 2
{
  "type": "date",
  "value": "2018-01-05"
}
```

{% endraw %}

## DateTimeObject {#DateTimeObject}
날짜와 시간 정보를 가지는 객체입니다.

### Object fields
| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"datetime"` 값으로 고정되어 있습니다.   |
| `value`         | string  | 날짜와 시간 정보. Content template 타입에 따라 `"YYYY-MM-DDThh:mm:ssZ"` 또는 `"YYYYMMDD hh:mm"` 포맷 형태로 값이 표현됩니다. |

### Object Example
{% raw %}

```json
// 예제 1: YYYY-MM-DDThh:mm:ssZ 포맷
{
  "type": "datetime",
  "value": "2017-07-26T18:00:00Z"
}

// 예제 2: YYYYMMDD hh:mm 포맷
{
  "type": "datetime",
  "value": "20170726 18:00"
}
```

{% endraw %}

## NumberObject {#NumberObject}
단위 구분자(천 단위)가 처리된 숫자 정보나 풍속과 같이 단위가 포함된 측정 수치 정보를 가지는 객체입니다.

### Object fields
| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------
| `type`          | string  | `"number"` 값으로 고정되어 있습니다.    |
| `value`         | string  | 단위 구분자(천 단위)가 처리된 숫자 정보나 단위가 포함된 측성 수치 정보 |

### Object Example
{% raw %}

```json
// 예제 1
{
  "type": "number",
  "value": "19,304,213"
}

// 예제 2
{
  "type": "number",
  "value": "2m/s"
}
```

{% endraw %}

## PercentageObject {#PercentageObject}
백분율 정보를 가지는 객체입니다.

### Object fields
| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"percentage"` 값으로 고정되어 있습니다. |
| `value`         | number 또는 string  | 백분율 정보. Content template 타입에 따라 실수 형태의 값 또는 퍼센트 기호가 함께 포함된 백분율 정보가 포함될 수 있습니다.  |

### Object Example
{% raw %}

```json
// 예제 1
{
  "type": "percentage",
  "value": 20.2341
}

// 예제 2
{
  "type": "percentage",
  "value": "20%"
}
```

{% endraw %}

## PhoneNumberObject {#PhoneNumberObject}
전화 번호 정보를 가지는 객체입니다.

### Object fields
| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"phoneNum"` 값으로 고정되어 있습니다. |
| `value`         | string  | 전화 번호 정보                    |

### Object Example
{% raw %}

```json
{
  "type": "phoneNum",
  "value": "031-784-1000"
}
```

{% endraw %}

## StringObject {#StringObject}
텍스트 정보를 가지는 객체입니다.

### Object fields
| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"string"` 값으로 고정되어 있습니다.  |
| `value`         | string  | 텍스트 정보                      |

### Object Example
{% raw %}

```json
// 예제 1
{
  "type": "string",
  "value": "토트넘 입단 손흥민 “EPL 항상 꿈꿔왔던 무대”"
}

// 예제 2
{
  "type": "string",
  "value": "검색결과"
}
```

{% endraw %}

## TemperatureCObject {#TemperatureCObject}
섭씨 단위의 온도 정보를 가지는 객체입니다.

### Object fields
| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"temperature-c"`이나 `"temperature"` 값이 입력됩니다. |
| `value`         | number 또는 string | 섭씨 단위의 온도 정보. 정수 또는 실수 값이 포함될 수 있습니다.     |

### Object Example
{% raw %}

```json
// 예제 1
{
  "type": "temperature-c",
  "value": 31
}

// 예제 2
{
  "type": "temperature",
  "value": "-4"
}
```

{% endraw %}

## TemperatureFObject {#TemperatureFObject}
화씨 단위의 온도 정보를 가지는 객체입니다.

### Object fields
| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"temperature-f"` 값으로 고정되어 있습니다.   |
| `value`         | number  | 화씨 단위의 온도 정보. 정수 또는 실수 값이 포함될 수 있습니다.   |

### Object Example
{% raw %}

```json
{
  "type": "temperature-f",
  "value": 75
}
```

{% endraw %}

## URIObject {#URIObject}
URI 정보를 가지는 객체입니다.

### Object fields
| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"url"` 값으로 고정되어 있습니다.   |
| `value`         | string  | URI 정보                        |

### Object Example
{% raw %}

```json
// 예제 1
{
  "type": "url",
  "value": "https://m.search.contentservice.example.com/search?where=m_image&mode=default&query=%EC%86%90%ED%9D%A5%EB%AF%BC%20%EC%9D%B4%EB%AF%B8%EC%A7%80#imgId=news4100000269062_1"
}

// 예제 2
{
  "type": "url",
  "value": "https://search.pstatic.example.net/common/?src=http%3A%2F%2Fimgnews.contentservice.example.com%2Fimage%2F410%2F2015%2F08%2F31%2F20150831_1441012614_99_20150831181804.jpg&type=b360"
}
```

{% endraw %}
