# BeerList
![Simulator Screen Recording - iPod touch (7th generation) - 2023-01-31 at 23 40 47](https://user-images.githubusercontent.com/42196410/215799013-0b1eba2a-0719-4d04-a0c0-a779239ff980.gif)



## ๐งฉ ๊ฐ์

- `URLSession`์ผ๋ก ๋ฐ์์จ data๋ฅผ `UITableView`๋ก ํ์
- ๋ฐ์์ฌ ๋ฐ์ดํฐ๋ฅผ `Prefetch`ํ์ฌ ์ต์ ํ
- `.insetGrouped` ์คํ์ผ์ `UITableView`๋ฅผ ํ์ 

## ๐ค ๋ฐฐ์ด ๋ด์ฉ

### โ๏ธ UITableViewDataSourcePrefetching

#### ์ ์:

์ ์ฌ์ ์ผ๋ก ์ค๋ ์คํ๋๋ ๋ฐ์ดํฐ ์์์ ์กฐ๊ธฐ์ ์์ํ  ์ ์๋๋ก ๋ฏธ๋ฆฌ ์ ๋ณด๋ฅผ ์ ๊ณตํ๋ ํ๋กํ ์ฝ

#### ์ฃผ์์ฌํญ:

`tableView(_:cellForRowAt:)` ๋ฉ์๋๊ฐ ์คํ๋๊ธฐ ์ ์ prefetch๊ฐ ๋น๋๊ธฐ์ ์ผ๋ก ์คํ๋๋ค.

๋ฐ๋ผ์, ํด๋น ํ๋กํ ์ฝ ๊ตฌํ์ ๋ฐ๋์ `tableView(_:cellForRowAt:)` ๋ฉ์๋๋ฅผ ๊ตฌํํ์ฌ์ผ ํ๋ค.

```swift
extension BeerListViewController: UITableViewDataSourcePrefetching {
    func tableView(_ tableView: UITableView, prefetchRowsAt indexPaths: [IndexPath]) {
        // ํ์ฌํ์ด์ง๊ฐ 2์ด์์ผ๋๋ถํฐ 
        guard currentPage != 1 else { return }
        // prefetch๋ฅผ ์คํ
        indexPaths.forEach {
            if ($0.row + 1)/25 + 1 == currentPage {
                self.fetchBeer(of: currentPage)
            }
        }
    }
}
```

### โ๏ธ ํด์ฌํ๊ทธ ๋ณํ ํจ์

์๋ฅผ๋ค์ด `first, second, third` ๋ฌธ์์ด์ `#first #second #third`๋ก ๋ณํ.

```swift
  let tags = taglineString?.components(separatedBy: ". ")
  let hashtags = tags?.map { "#" + $0.replacingOccurrences(of: " ", with: "")
      .replacingOccurrences(of: ".", with: "")
      .replacingOccurrences(of: ",", with: " #")
  }
  return hashtags?.joined(separator: " ") ?? ""
```
### โ๏ธ imageLiteral๋ก ์ด๋ฏธ์ง ๋ถ๋ฌ์ค๊ธฐ

`#imageLiteral(resource:)` ์ฝ๋๋ฅผ ์๋ ฅํ๋ฉด ์๋์ ๊ฐ์ด ์ด๋ฏธ์ง๋ฅผ ์ ํํ  ์ ์๋ ํ์์ด ๋ํ๋๋ค.

<img width="397" alt="Other" src="https://user-images.githubusercontent.com/42196410/215799652-6c8c8a4c-eedd-4ab5-9daa-fa5ede3ff5d1.png">
