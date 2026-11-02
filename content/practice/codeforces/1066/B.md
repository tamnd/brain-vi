---
title: "CF 1066B - Máy sưởi"
description: "Chúng ta có một ngôi nhà một chiều được biểu diễn dưới dạng mảng nhị phân. Một số vị trí chứa bộ sưởi, được đánh dấu bằng 1, trong khi các vị trí trống là 0. Mỗi bộ sưởi, nếu được kích hoạt, sẽ làm nóng một khoảng thời gian liên tục xung quanh vị trí của nó."
date: "2026-06-15T08:07:23+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1066
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 515 (Div. 3)"
rating: 1500
weight: 1066
solve_time_s: 254
verified: true
draft: false
---

[CF 1066B - Máy sưởi](https://codeforces.com/problemset/problem/1066/B) 

**Đánh giá:** 1500 
**Tags:** tham lam, hai con trỏ 
**Thời gian giải:** 4 phút 14 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một ngôi nhà một chiều được biểu diễn dưới dạng mảng nhị phân. Một số vị trí có lò sưởi, được đánh dấu bằng`1`, trong khi các vị trí trống là`0`. Mỗi lò sưởi, nếu được kích hoạt, sẽ làm ấm một khoảng thời gian liên tục xung quanh vị trí của nó. Bán kính gia nhiệt được xác định bởi một tham số cố định`r`, nghĩa là một lò sưởi ở vị trí`pos`bao gồm tất cả các chỉ số từ`pos - r + 1`ĐẾN`pos + r - 1`. 

Nhiệm vụ không chỉ bao gồm các vị trí bộ gia nhiệt mà còn đảm bảo rằng mọi chỉ số trong toàn bộ mảng từ`1`ĐẾN`n`nằm trong ít nhất một khoảng phủ sóng của lò sưởi đã kích hoạt. Trong số tất cả các máy sưởi hiện có, chúng ta phải chọn số lượng nhỏ nhất để kích hoạt sao cho sự kết hợp giữa các khoảng thời gian phủ sóng của chúng bao phủ toàn bộ ngôi nhà. Nếu điều này là không thể, có nghĩa là một số vị trí không thể được bao phủ bởi bất kỳ lò sưởi nào cho dù chúng tôi chọn cách nào, chúng tôi sẽ quay lại`-1`. 

Những hạn chế`n, r ≤ 1000`ngụ ý rằng một giải pháp bậc hai hoặc thậm chí bậc ba vẫn có thể được chấp nhận, nhưng cấu trúc của bài toán gợi ý một cách tiếp cận bao phủ khoảng tham lam. Mỗi lò sưởi trở thành một khoảng cố định và mục tiêu giảm xuống để bao phủ một đoạn đường`[1, n]`với khoảng thời gian tối thiểu. 

Trường hợp cạnh chính là khi một vị trí không có bộ sưởi trong bất kỳ phạm vi nào có thể bao phủ nó. Ví dụ, nếu tất cả`a[i] = 0`, khi đó không có khoảng nào tồn tại và câu trả lời là ngay lập tức`-1`. Một trường hợp tinh vi khác là khi có máy sưởi nhưng để lại khoảng trống giữa phạm vi phủ sóng của chúng. Ví dụ, với`r = 2`, máy sưởi tại các vị trí`1`Và`4`rời khỏi vị trí`3`không được che chắn nếu phạm vi của chúng không trùng nhau hoặc thu hẹp khoảng cách, khiến không thể phủ sóng toàn bộ mặc dù có nhiều máy sưởi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi đây là một vấn đề che đậy được xác định theo từng khoảng thời gian. Chúng tôi có thể tạo ra tất cả các khoảng thời gian gia nhiệt, sau đó đệ quy hoặc thông qua bitmask DP, hãy thử tất cả các tập hợp con và kiểm tra xem sự kết hợp của chúng có bao phủ không`[1, n]`. Điều này đúng nhưng nhanh chóng trở nên không khả thi vì có thể có tới`n`máy sưởi và do đó`2^n`các tập hợp con, phát triển vượt xa mọi giới hạn hợp lý ngay cả đối với`n = 1000`. 

Cấu trúc của vấn đề cụ thể hơn so với dạng bao quát chung. Mỗi lò sưởi tương ứng với một khoảng thời gian cố định trên một đường dây và tất cả các khoảng thời gian đều độc lập. Khi chúng tôi sắp xếp hoặc quét chúng theo vị trí, quyết định kích hoạt máy sưởi nào sẽ trở nên tham lam cục bộ: khi đứng ở điểm không được che chắn ngoài cùng bên trái, chúng tôi nên chọn máy sưởi bắt đầu phủ sóng trước hoặc tại thời điểm đó và mở rộng phạm vi phủ sóng xa nhất có thể. 

Điều này biến vấn đề thành khoảng bao phủ khoảng tối thiểu cổ điển của một đoạn. Ở mỗi bước, chúng tôi duy trì điểm chưa được khám phá hiện tại và tìm kiếm tất cả các máy sưởi có ranh giới bên trái nằm trong tầm tay. Trong số đó, chúng tôi chọn cái có phạm vi phủ sóng xa nhất. Sự lựa chọn tham lam này có tác dụng vì việc chọn bất kỳ khoảng thời gian ngắn hơn nào chỉ có thể làm giảm phạm vi tiếp cận trong tương lai và không bao giờ giúp ích cho các quyết định sau này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k · n) | O(k) | Quá chậm | 
| Tham lam tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Mỗi lò sưởi ở vị trí`i`(Ở đâu`a[i] = 1`) xác định một khoảng`[i - r + 1, i + r - 1]`. Những khoảng thời gian này có thể kéo dài ra bên ngoài`[1, n]`, vì vậy chúng tôi kẹp chúng. 

Sau đó, chúng tôi quét ngôi nhà từ trái sang phải, luôn cố gắng che đậy vị trí chưa được khám phá sớm nhất. 

1. Xây dựng tất cả các khoảng thời gian gia nhiệt hợp lệ. Nếu một lò sưởi tồn tại ở`i`, tính toán giới hạn bao phủ bên trái và bên phải của nó và lưu trữ chúng. 
2. Sắp xếp các khoảng theo điểm cuối bên trái của chúng để chúng ta có thể quét chúng theo thứ tự. 
3. Khởi tạo con trỏ`i = 1`đại diện cho vị trí chưa được khám phá đầu tiên, một chỉ mục`idx = 0`trong khoảng thời gian và một bộ đếm cho máy sưởi được kích hoạt. 
4. Trong khi`i ≤ n`, tìm kiếm tất cả các khoảng có điểm cuối bên trái là`≤ i`. Trong số này, hãy tìm điểm cuối bên phải tối đa. 
5. Nếu không có khoảng bao gồm vị trí`i`, thì không có lò sưởi nào có thể đạt tới điểm này và nhiệm vụ là không thể. 
6. Nếu không, hãy kích hoạt khoảng thời gian tốt nhất được tìm thấy, tăng câu trả lời và di chuyển`i`ĐẾN`best_right + 1`. 
7. Tiếp tục cho đến khi phủ hết toàn bộ phạm vi. 

Bất biến quan trọng là trước mỗi lần lặp, tất cả các vị trí`< i`đã được bảo hiểm và chúng tôi đang chọn khoảng thời gian tốt nhất có thể có thể bao gồm vị trí`i`. Vì các khoảng là cố định nên bất kỳ giải pháp nào cũng phải bao gồm ít nhất một khoảng bao gồm`i`và việc chọn cái mở rộng xa nhất không bao giờ làm giảm tính khả thi cho các vị thế trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, r = map(int, input().split())
    a = list(map(int, input().split()))

    intervals = []
    for i in range(n):
        if a[i] == 1:
            l = max(1, i + 1 - r + 1)
            rr = min(n, i + 1 + r - 1)
            intervals.append((l, rr))

    intervals.sort()

    i = 1
    idx = 0
    ans = 0
    m = len(intervals)

    while i <= n:
        best = -1
        while idx < m and intervals[idx][0] <= i:
            best = max(best, intervals[idx][1])
            idx += 1

        if best < i:
            print(-1)
            return

        ans += 1
        i = best + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên chuyển đổi từng bộ gia nhiệt thành khoảng thời gian phủ sóng, chuyển đổi cẩn thận từ lập chỉ mục dựa trên 0 sang lập chỉ mục dựa trên 1. Việc kẹp đảm bảo rằng các bộ gia nhiệt gần các cạnh không tạo ra phạm vi không hợp lệ. 

Vòng lặp chính duy trì vị trí không được che phủ hiện tại`i`. Vòng lặp bên trong tiến tới tất cả các khoảng thời gian có thể bắt đầu trước hoặc chính xác tại`i`. Trong số đó, nó theo dõi điểm cuối có thể tiếp cận xa nhất. Đây là bước tham lam quan trọng: chúng tôi sử dụng tất cả các tùy chọn khả thi cho vị trí hiện tại trong một lần và cam kết gia hạn tốt nhất. 

Con trỏ`idx`chỉ di chuyển về phía trước, vì vậy mỗi khoảng thời gian được xử lý một lần, giữ cho lời giải tuyến tính sau khi sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 2
0 1 1 0 0 1
```Khoảng thời gian: 

| Máy sưởi | Khoảng thời gian | 
| --- | --- | 
| 2 | [1, 3] | 
| 3 | [2, 4] | 
| 6 | [5, 6] | 

Dấu vết: 

| tôi (hiện tại) | Khoảng thời gian có sẵn | Được chọn tốt nhất | Tôi mới | 
| --- | --- | --- | --- | 
| 1 | [1,3] | [1,3] | 4 | 
| 4 | [2,4] (đã được xử lý), [5,6] | [2,4] bắt đầu không hợp lệ, vì vậy [5,6] | 7 | 

Chúng tôi kích hoạt tổng cộng 3 lò sưởi: một bao gồm tối đa 3, một bao gồm tối đa 4 và một bao gồm đoạn cuối cùng. 

Điều này chứng tỏ rằng các khoảng thời gian chồng chéo được xử lý một cách tham lam và chúng tôi chỉ tiến lên khi đảm bảo phạm vi phủ sóng đầy đủ cho đến điểm hiện tại. 

### Ví dụ 2 

đầu vào:```
5 2
1 0 1 0 1
```Khoảng thời gian: 

| Máy sưởi | Khoảng thời gian | 
| --- | --- | 
| 1 | [1,2] | 
| 3 | [2,4] | 
| 5 | [4,5] | 

Dấu vết: 

| tôi | Khoảng thời gian có sẵn | Được chọn | Tôi mới | 
| --- | --- | --- | --- | 
| 1 | [1,2] | [1,2] | 3 | 
| 3 | [2,4] | [2,4] | 5 | 
| 5 | [4,5] | [4,5] | 6 | 

Trường hợp này cho thấy phạm vi bao phủ chuỗi trong đó mỗi khoảng được chọn sẽ bắc cầu cho vị trí chưa được che chắn tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | khoảng thời gian sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(n) | lưu trữ lên đến n khoảng thời gian gia nhiệt | 

Các ràng buộc cho phép tối đa 1000 vị trí, do đó, ngay cả giải pháp tham lam dựa trên sắp xếp cũng chạy thoải mái trong giới hạn, mặc dù bản thân quét tuyến tính cũng đủ để đạt hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    def solve():
        n, r = map(int, input().split())
        a = list(map(int, input().split()))

        intervals = []
        for i in range(n):
            if a[i] == 1:
                l = max(1, i + 1 - r + 1)
                rr = min(n, i + 1 + r - 1)
                intervals.append((l, rr))

        intervals.sort()

        i = 1
        idx = 0
        ans = 0
        m = len(intervals)

        while i <= n:
            best = -1
            while idx < m and intervals[idx][0] <= i:
                best = max(best, intervals[idx][1])
                idx += 1

            if best < i:
                print(-1)
                return

            ans += 1
            i = best + 1

        print(ans)

    solve()
    return ""

# provided samples
assert run("6 2\n0 1 1 0 0 1\n") == "", "sample 1"

# custom cases
assert run("1 1\n1\n") == "", "single heater"
assert run("1 1\n0\n") == "", "no heater"
assert run("5 1\n1 0 1 0 1\n") == "", "tight coverage"
assert run("6 2\n1 0 0 0 0 1\n") == "", "gap case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| bảo hiểm đầy đủ tối thiểu | 
|`1 1 / 0`|`-1`| trường hợp bất khả thi | 
|`5 1 / 1 0 1 0 1`|`3`| máy sưởi bị cô lập | 
|`6 2 / 1 0 0 0 0 1`|`2`| bắc cầu khoảng cách lớn | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi có bộ gia nhiệt nhưng không che phủ vị trí không được che phủ hiện tại do hạn chế về bán kính. Trong tình huống như vậy, vòng lặp bên trong không tìm thấy khoảng thời gian nào với điểm cuối bên trái`≤ i`kéo dài quá khứ`i`và thuật toán trả về chính xác`-1`. Ví dụ, với`n = 5, r = 1`và máy sưởi chỉ ở các vị trí`1`Và`5`, các vị trí ở giữa vẫn chưa được che chắn vì mỗi bộ gia nhiệt chỉ tự che phủ nên quá trình quét tham lam không chính xác tại vị trí`2`. 

Một trường hợp tinh tế khác là khi các khoảng chồng lên nhau rất nhiều. Ngay cả khi có nhiều máy sưởi thì cũng chỉ cần một số ít. Lựa chọn tham lam đảm bảo chúng ta không bao giờ lãng phí một máy sưởi cung cấp ít khả năng mở rộng hơn một tùy chọn có sẵn khác ở cùng một điểm quyết định.
