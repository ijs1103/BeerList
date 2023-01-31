# BeerList
![Simulator Screen Recording - iPod touch (7th generation) - 2023-01-31 at 23 40 47](https://user-images.githubusercontent.com/42196410/215799013-0b1eba2a-0719-4d04-a0c0-a779239ff980.gif)



## 🧩 개요

- `URLSession`으로 받아온 data를 `UITableView`로 표시
- 받아올 데이터를 `Prefetch`하여 최적화
- `.insetGrouped` 스타일의 `UITableView`를 표시 

## 🤔 배운 내용

### ✔️ UITableViewDataSourcePrefetching

#### 정의:

잠재적으로 오래 실행되는 데이터 작업을 조기에 시작할 수 있도록 미리 정보를 제공하는 프로토콜

#### 주의사항:

`tableView(_:cellForRowAt:)` 메서드가 실행되기 전에 prefetch가 비동기적으로 실행된다.

따라서, 해당 프로토콜 구현시 반드시 `tableView(_:cellForRowAt:)` 메서드를 구현하여야 한다.

```swift
extension BeerListViewController: UITableViewDataSourcePrefetching {
    func tableView(_ tableView: UITableView, prefetchRowsAt indexPaths: [IndexPath]) {
        // 현재페이지가 2이상일때부터 
        guard currentPage != 1 else { return }
        // prefetch를 실행
        indexPaths.forEach {
            if ($0.row + 1)/25 + 1 == currentPage {
                self.fetchBeer(of: currentPage)
            }
        }
    }
}
```

### ✔️ 해쉬태그 변환 함수

예를들어 `first, second, third` 문자열을 `#first #second #third`로 변환.

```swift
  let tags = taglineString?.components(separatedBy: ". ")
  let hashtags = tags?.map { "#" + $0.replacingOccurrences(of: " ", with: "")
      .replacingOccurrences(of: ".", with: "")
      .replacingOccurrences(of: ",", with: " #")
  }
  return hashtags?.joined(separator: " ") ?? ""
```
### ✔️ imageLiteral로 이미지 불러오기

`#imageLiteral(resource:)` 코드를 입력하면 아래와 같이 이미지를 선택할 수 있는 팝업이 나타난다.

<img width="397" alt="Other" src="https://user-images.githubusercontent.com/42196410/215799652-6c8c8a4c-eedd-4ab5-9daa-fa5ede3ff5d1.png">
