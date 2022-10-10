---
title: iOS, Swift Date 이해하기 2편
date: 2022-10-10 13:13:00 +0900
categories: [Swift, Date]
tags: [swift]     # TAG names should always be lowercase
author: JohnCoder
---
iOS, Swift Date 이해하기 1편
<>

지난 시간에는 Date 타입의 시간이 어디를 기준으로 하는지, 생성자는 어떻게 구성되어 있는지 알아보았다.
Date 타입은 시간으로 이루어져 있어 정수형에 그낭 갖다 대입해도 될거같고, 출력되는 포맷 자체가 
문자열 처럼도 보여서 아무렇게나 쓸 수 있을것 같아 보이는데, Date도 타입이기 때문에 다른 타입으로
변환과정을 거쳐야 잘 사용할 수 있다. 

Date 타입형을 구워 삶을 수 있는 자료형은

***1) DateFormatter***
***2) DateComponents***

두가지가 있다.

## <DateFormatter VS DateComponents>
### 1) DateFormatter

DateFormatter의 설명을 공식문서에서 확인하면 
***A formatter that converts between dates and their textual representations.***
***번역하면, 날짜와 텍스트 사이를 변환하는 포매터(형식지정자) 역할***

DateComponents의 설명은 

A date or time specified in terms of units
(such as year, month, day, hour, and minute) to be evaluated in a calendar
 system and time zone.

날짜/시간을 단위별(연, 월, 일, 시간, 분 등)로 지정된 날짜 또는 시간이라고 하는데, 사실 무슨말인지 번역을 
봐도 어렵다. 

이 부분은 아래 적용 코드에서 이해를 돕도록 하겠다.

```swift
import Foundation

let curruntTime: Date = Date() 
//GMT +0의 현재시간. 만약 지금이 한국 23시면, -9시. 즉 낮 2시가 뜨겠죠?
var dateConvert: DateFormatter = DateFormatter()
//DateFormatter 인스턴스 생성

print(TimeZone.abbreviationDictionary)
//TimeZone.abbreviationDictionary 메서드를 출력하면 각 타임존별 축악여를 전부 확인할 수 있다.
//한국은 "KST" 인 것 확인 가능!

dateConvert.timeZone = TimeZone(abbreviation: "KST")
//한국 시간 적용. abbrevitation에는 각 날짜 타임존의 축약어가 들어간다.
//위에서 설명함!

dateConvert.dateFormat = "yyyy-MM-dd HH:mm:ss"
//dateFormatter 타입의 프로퍼티인 dateFormat을 정해준다.
//dateFormate은 날짜를 어떤 방식으로 보여줄 것인지 문자열로 포맷을 정한다.
//포맷 방식은 아래 표로 정리해놓았다.

let dateString = dateConvert.string(from: curruntTime)
//dateConvert에 세팅한 포맷으로 from 인자로 들어간 날짜를 문자열로 반환하는 메서드이다.

print(curruntTime)
//2022-10-04 14:53:20 +0000 -> Date타입

print(dateConvert.string(from: curruntTime))
//2022-10-04 23:53:20 -> String타입!
```

물론, Date <-> DateFormatter 를 오갈 수 있도록 메서드도 있으니 공식문서 참고!

func date(from string: String) -> Date? : 포맷팅되어 표현된 날짜의 문자열을 Date로 반환! 
func string(from date: Date) -> String : 날짜를 포맷팅된 문자열로 반환!


 ++ 그리고, DateFormatter에 Locale(identifier: String) 이라는 메서드가 있는데, 여기 들어가는
 identifier 인자에 지역 이름 비슷한게 들어가게 되어있다. 여기에 한국 기준으로 “ko_KR” 이라는 축약어가
 identifier 인자 로 들어가는데, 저 인자를 넣으면 뭔가 시차가 적용될 것 같은 느낌이 팍팍 드는데, 
 이는 잘못되었다.
 (실제로 나도 그랬고 여러 커뮤니티에 Locale 지정했는데 왜 시차적용이 안되요? 하는 글들을 많이 보았다.)
 여기에 들어가는 지역별 identifier에 따라 .dateFormat으로 표시했을 때, 각 나라별 방식으로 날짜들이 
 표현되게 된다. 따라서 Locale은 시차와는 전~~~혀 관련이 없다!

 ex) 한국: 12월, 미국: Dec…
 (identifier 축약어는 구글에 Locale identifier list라고 검색하면 잘 정해놓은 글들이 많다.)

++코드 설명 추가

### 1) DateComponents

DateComponents는 공식 문서에 의하면
*** A date or time specified in terms of units
    (such as year, month, day, hour, and minute)
    to be evaluated in a calendar system and time zone.

즉, 특정한 날짜/시간의 단위와 관련이 있어 보인다. DateFormatter와 다르게 DateComponents
는 생성자만 봐도 뭔가 복잡하게는 되어있는데, 그래도 인자 네임태그가 잘 표현되어 있다.

```swift
init(
    calendar: Calendar? = nil,
    timeZone: TimeZone? = nil,
    era: Int? = nil,
    year: Int? = nil,
    month: Int? = nil,
    day: Int? = nil,
    hour: Int? = nil,
    minute: Int? = nil,
    second: Int? = nil,
    nanosecond: Int? = nil,
    weekday: Int? = nil,
    weekdayOrdinal: Int? = nil,
    quarter: Int? = nil,
    weekOfMonth: Int? = nil,
    weekOfYear: Int? = nil,
    yearForWeekOfYear: Int? = nil
)
```
위에서 특정한 날짜/시간의 단위와 관련있다고 했는데, 위 코드를 보면 연, 월, 일, 시간, 분, 초 등
날짜와 관련된 요소들이 생성자의 인자로 들어가있는데 특이하게도 전부 옵셔널 변수로 들어가 있다.
이 말인 즉슨, 모든 인자들이 필수로 들어갈 필요가 없고 원하는 인자만 넣어서 활용할 수 있게
되어있는 것 같다. 아래 코드를 보면 이해가 갈 것 이다!

```swift
//오늘날짜로부터 35일 되는날을 구하는 코드
import Foundation

let currentDate = Date() //현재 날짜
var calendar = Calendar.current //dateComponents를 활용하기 위한 Calendar 변수
calendar.timeZone = TimeZone(abbreviation: "UTC")!

//Date를 문자열로 처리하기 위한 DateFormatter 변수 선언
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyyy-MM-dd HH:mm:ss"
dateFormatter.timeZone = TimeZone(abbreviation: "UTC")

let day = DateComponents(day: 35)
//day: 35일 이라는 요소를 갖도록 인스턴스화. day 변수에는 다른 요소들은 nil이고, 일만 가지고 있다.

if let d100 = calendar.date(byAdding: day, to: currentDate)
{
    print(currentDate) // 
    print(dateFormatter.string(from: d100))

}
//2022-10-10 12:07:35 +0000 -> 현재 시각
//2022-11-14 12:07:35 -> 35일 지난 날짜

//calendar.date(byAdding: DateComponents, to: Date) 함수를 사용해서
//byAdding에 day변수(35일)를, to: 에 현재 날짜를 넣으면
//현재 날짜에서 35일 지난 날짜를 d100 변수에 반환시켜 준다.(옵셔널이라 if let 처리!)

```

### 마치며
내가 느끼는 두 자료형의 용도는 

***DateFormatter : Date 를 문자열로 쓰기위한, 혹은 그 반대를 위한 기능을 담당하고,***
***DateComponents : 날짜 데이터 요소(년, 월, 일, 시간 등..)간 계산을 위한 성격이 강하다.***

DateComponents 자료형에는 별도로 Date나 String으로 반환하는 함수가 없으니
반드시 Date나 Calendar에 계산되어 쓴 다음에 DateFormatter로 활용하는 것이 좋다.



