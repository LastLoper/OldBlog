---
layout: post
menubar: type_casting_menu
date: 2022-11-26 16:57:00 +0900
title: Swift의 is, as 타입캐스팅
subtitle: iOS앱에서 Swift is, as 타입캐스팅 이렇게 쓸 수 있다.
categories: property-observer
description: "클래스명, 변수명등을 손쉽게 바꾸는 단축키"
published: true
---

먼저 Is/As를 뜻하는 **타입 캐스팅** 용어의 뜻을 한번 풀어볼까요? 프로그래밍 용어는 뜻만 알아도 이놈이 어떤 일을 하는지 대충 알 수 있거든요. (변수명을 잘 지어야 하는 이유!)

<br/>

Type : 많이 익숙한 단어죠? **유형, 종류**의 뜻을 가지고 있습니다.

Casting : cast + ing의 형태로 **배역을 맡는 것** 정도로 생각하시면 좋겠네요.

<br/>

두 단어를 합쳐보면 아 대충 **어떤 유형(객체, 데이터형등)에 배역을 맡기는 것**이구나하고 짐작할 수 있죠. 이 역할을 하는 것이 바로 **As연산자**입니다.

그리고 Swift에는 배역을 맡기는 것뿐만 아니라 **배역을 확인하는 것**또한 가능해요. 이것이 **Is연산자**입니다.

대충 어떤 역할을 하는지 느낌이 오시나요?

<br/>

## 사용법

### is 연산자

Is연산자는 위에서 언급했듯이 '배역을 확인하는 역할'입니다. 여기서 말하는 배역이란 바로 타입입니다. String, Int, Instance등의 타입이죠.

Is연산자는 맞는지 틀리는지만을 Return합니다.

그래서 사실 타입 확인이 필요한 부분, iOS 코드 어디에서든 사용할 수 있습니다.

AnyObject에 대한 확인이 가능하고요.

```swift
let isNumber = 123
if isNumber is Int {
    print("숫자입니다.")
}

//숫자입니다.

let isString = "Loperz"
if isString is String {
    print("문자열입니다.")
}

//문자열입니다.
```

객체 확인으로도 사용할 수 있습니다.

```swift
class Instrument { }
class Weapon { }
class Guitar: Instrument { }
class Piano: Instrument { }

let guitar = Guitar()
let piano = Piano()

guitar is Instrument        //true
guitar is Weapon            //false
piano is Instrument         //true
```

### as 연산자

As연산자에 대해서는 이미 많이 써 보셨을 것 같아요, 테이블뷰 또는 컬렉션뷰에서 셀을 가져올 때 혹은 화면전환하기 위해 뷰컨트롤러를 찾을 때 말이지요. 그래도 다시한번 볼까요?

```swift
//CollectionView에서 Cell을 찾아 가져올 때
func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
    //사진을 보여줄 Cell
    guard let cell = collectionView.dequeueReusableCell(
            withReuseIdentifier: Keys.CellIds.albumListCell, 
            for: indexPath
        ) as? AlbumCell else { return UICollectionViewCell() }
        //cell을 AlbumCell로 쓸 수 있는지 guard let을 통해 확인합니다.
        
    return cell
}
```

위 코드처럼 컬렉션뷰등에서 셀을 가져올 때 많이 볼 수 있습니다. as가 쓰인 문장을 말로 풀어본다면, cell을 AlbumCell타입의 cell로 쓸 것인지 확인합니다. 이때는 as에 널 세이프 연산자인 ?(물음표)를 붙여 만약 아니라면 UICollectionViewCell()을 Retrun하고 종료되겠네요.

이번엔 Segue를 통한 화면전환시 as를 사용한 코드를 볼까요?

```swift
//화면전환 할때
if segue.identifier == "DetailVC" {
    let vc = segue.destination as! DetailVC
    //vc는 DetailVC타입의 segue의 목적지인 ViewController가 됩니다.
    vc.setImageToView(asset: imgAsset)
}
```

여기서 as에 !(느낌표)를 붙여 vc가 DetailVC타입의 ViewController임을 보장하는 코드입니다.