---
title: "CF 106390C - Nhà Giường"
description: "Vấn đề là tối đa hóa lợi nhuận khi nướng bánh. Có sẵn một lượng bột cố định và nhiều loại nhân khác nhau. Một chiếc bánh bao có nhân nhất định tiêu tốn một ít bột và một lượng nhân đó, sau đó mang lại một lợi nhuận nhất định."
date: "2026-06-25T10:12:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106390
codeforces_index: "C"
codeforces_contest_name: "Purdue Spring 2026 In-House Contest #2"
rating: 0
weight: 106390
solve_time_s: 38
verified: true
draft: false
---

[CF 106390C - Nhà giường](https://codeforces.com/problemset/problem/106390/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề là tối đa hóa lợi nhuận khi nướng bánh. Có sẵn một lượng bột cố định và nhiều loại nhân khác nhau. Một chiếc bánh bao có nhân nhất định tiêu tốn một ít bột và một lượng nhân đó, sau đó mang lại một lợi nhuận nhất định. Một chiếc bánh mì đơn giản chỉ tiêu tốn bột và mang lại lợi nhuận riêng. Nhiệm vụ là quyết định làm bao nhiêu loại bánh có thể làm để tổng số tiền thu được càng lớn càng tốt. 

Dữ liệu đầu vào cung cấp tổng số bột, số loại nhân và các yêu cầu đối với bánh thường. Mỗi loại nhân mô tả số lượng nhân đó tồn tại, một chiếc bánh cần bao nhiêu nhân, chiếc bánh tiêu tốn bao nhiêu bột và kiếm được bao nhiêu tiền. Sản lượng là số tiền tối đa có thể kiếm được. 

Giới hạn nhỏ về số lượng loại nhân nhưng lại lớn về lượng bột. Vì số lượng kiểu nhồi nhiều nhất là 10 nên chúng ta có thể sử dụng giải pháp lập trình hàm mũ hoặc lập trình động cho các kiểu nhồi. Tuy nhiên, lượng bột có thể lên tới 1000, do đó, giải pháp thử mọi số lượng bánh có thể mà không có giới hạn trạng thái sẽ tăng quá nhiều. Chúng ta cần sử dụng số lượng nhỏ các danh mục nhồi để kiểm soát việc tìm kiếm và lượng bột làm kích thước DP. 

Cái bẫy chính là xử lý phần bột còn lại sau khi chọn bánh nhồi. Chiếc bánh mì trơn đóng vai trò như một chất độn cho bất kỳ loại bột nào chưa sử dụng, vì vậy câu trả lời không chỉ đơn giản là sự kết hợp tốt nhất giữa những chiếc bánh nhồi. Ví dụ:```
10 1 3 100
5 10 2 1
```Câu trả lời đúng là 300. Chúng ta không thể làm bánh nhân vì một người cần 10 đơn vị nhân trong khi chỉ có 5 đơn vị, vì vậy chúng ta nên làm ba chiếc bánh đơn giản. Một giải pháp chỉ xem xét các lựa chọn nhồi nhét và quên mất búi tóc đơn giản sẽ cho kết quả bằng 0. 

Một trường hợp phức tạp khác là khi loại nhân có lợi nhuận kém hơn trên mỗi đơn vị bột nhưng lại trở nên hữu ích vì nó tiêu thụ lượng nhân nhồi một cách hiệu quả. Ví dụ:```
10 2 5 1
100 1 5 10
2 10 1 100
```Câu trả lời đúng là 10. Chúng ta có thể làm được hai chiếc bánh loại thứ nhất. Một cách tiếp cận tham lam luôn chọn giá trị tốt nhất cho mỗi khối bột sẽ chọn sai loại thứ hai vì nó có vẻ có lợi hơn, nhưng giới hạn nhồi đã ngăn cản điều đó. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi số lượng có thể cho mọi loại nhồi. Đối với mỗi loại, chúng tôi quyết định số lượng bánh cần nướng, sau đó kiểm tra xem tổng số nhân và lượng bột sử dụng có hợp lệ hay không. Cách tiếp cận này đúng vì mọi sự kết hợp có thể đều được kiểm tra, do đó phải tìm ra sự kết hợp hợp lệ nhất. Vấn đề là mỗi loại nhồi có thể có tới 1000 số lượng có thể có và với tối đa 10 loại thì điều này trở thành quá nhiều sự kết hợp. Trường hợp xấu nhất đạt tới 1000^10 khả năng, điều này là không thể. 

Quan sát hữu ích là số lượng loại nhồi rất nhỏ. Thay vì theo dõi mọi cách phân phối bánh có thể, chúng tôi có thể xử lý từng loại nhân nhồi và chỉ lưu trữ lợi nhuận tốt nhất có thể đạt được cho mỗi lượng bột được sử dụng. Kích thước trạng thái bị giới hạn bởi số lượng bột, chỉ là 1000. 

Khi xử lý một loại nhân, chúng tôi thử tất cả số lượng bánh có thể được làm bằng loại nhân đó. Nếu chúng ta làm k loại bánh này, chúng ta sẽ biết chính xác họ tiêu thụ bao nhiêu nhân và bột và lãi thêm bao nhiêu. Đây là một quá trình chuyển đổi ba lô giới hạn. Sau khi tất cả các loại nhân được xử lý, phần bột còn lại có thể được chuyển thành bánh mì trơn, vì vậy chúng tôi chỉ cần cộng lợi nhuận bánh mì trơn tốt nhất có thể cho mọi trạng thái. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi sự kết hợp, nhưng không thành công vì số lượng kết hợp bùng nổ. DP hoạt động vì các lựa chọn nhồi khác nhau chỉ quan trọng thông qua số lượng bột đã chi và lợi nhuận kiếm được, đó chính xác là những gì nhà nước tích trữ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tích của các số có thể) | O(1) | Quá chậm | 
| Tối ưu | O(m * n * 100) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng lập trình động trong đó`dp[j]`đại diện cho lợi nhuận tối đa sau khi sử dụng chính xác`j`gam bột với các loại nhân đã được chế biến cho đến nay. Ban đầu, sử dụng bột không mang lại lợi nhuận bằng không. 
2. Đối với mỗi loại nhân, hãy tính số lượng bánh tối đa có thể làm được từ những loại nhân có sẵn. Chúng tôi không thể vượt quá con số này vì việc nhồi nhét là nguồn tài nguyên có hạn. 
3. Đối với mỗi lần sử dụng bột trước đó và mỗi lần đếm bánh có thể có của loại hiện tại, hãy cập nhật trạng thái mới. Làm`k`bánh thêm`k * d[i]`lợi nhuận và tiêu dùng`k * c[i]`bột. 
4. Sau khi tất cả các loại bánh nhồi đã được xử lý, hãy xem xét mọi lượng bột có thể đã được sử dụng. Phần bột còn lại chỉ có thể dùng làm bánh thường, nên thêm vào`(remaining dough / c0) * d0`đến trạng thái đó. 
5. Giá trị tối đa trong số tất cả các trạng thái cuối cùng là câu trả lời vì mọi cách sử dụng nhân hợp lệ đều được thể hiện và phần bột còn sót lại được lấp đầy bằng bánh mì trơn một cách tối ưu. 

Tại sao nó hoạt động: bất biến là sau khi xử lý bất kỳ tiền tố nào của kiểu nhồi,`dp[j]`tích trữ lợi nhuận tốt nhất có thể trong số tất cả các cách chi tiêu chính xác`j`gram bột chỉ sử dụng những loại đã được chế biến. Mọi quá trình chuyển đổi đều xem xét việc thêm loại nhồi hiện tại với số lần có thể bất kỳ, do đó, nó bao gồm mọi kết hợp hợp lệ mới. Vì chiếc bánh mì đơn giản không có nguồn lực hạn chế nào ngoại trừ bột nhào nên việc xử lý nó cuối cùng sẽ hoàn thành mọi giải pháp khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, c0, d0 = map(int, input().split())
    items = []

    for _ in range(m):
        a, b, c, d = map(int, input().split())
        items.append((a // b, c, d))

    dp = [-1] * (n + 1)
    dp[0] = 0

    for limit, cost, value in items:
        ndp = [-1] * (n + 1)
        for used in range(n + 1):
            if dp[used] == -1:
                continue
            for cnt in range(limit + 1):
                new_used = used + cnt * cost
                if new_used > n:
                    break
                profit = dp[used] + cnt * value
                if profit > ndp[new_used]:
                    ndp[new_used] = profit
        dp = ndp

    ans = 0
    for used in range(n + 1):
        if dp[used] != -1:
            total = dp[used] + ((n - used) // c0) * d0
            if total > ans:
                ans = total

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi từng loại nhồi thành giới hạn có thể sử dụng.`a // b`là số lượng bánh bao tối đa của loại đó vì mỗi bánh tiêu thụ`b`đơn vị nhồi và chỉ`a`đơn vị tồn tại. 

Mảng DP được xây dựng lại cho mọi kiểu nhồi để quá trình chuyển đổi không vô tình sử dụng cùng một kiểu nhồi nhiều lần vượt quá giới hạn của nó. Đối với mỗi lượng bột có thể tiếp cận, vòng lặp bên trong sẽ thử tất cả số lượng có thể có của loại bánh hiện tại. 

Vòng cuối cùng là nơi xử lý những chiếc bánh đơn giản. Nhà nước đã có sẵn lợi nhuận từ bánh nhồi, và số bột còn lại sẽ được chuyển thành càng nhiều bánh thường càng tốt. Điều này tránh cần một chiều DP khác. 

Điều kiện biên trong quá trình chuyển đổi là`new_used > n`. Khi vượt quá giới hạn bột, số lượng lớn hơn là không thể, do đó vòng lặp có thể dừng lại. Số nguyên Python đủ lớn cho phạm vi lợi nhuận tối đa, do đó không cần xử lý tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
10 2 2 1
7 3 2 100
12 3 1 10
```DP phát triển như sau. 

| Loại đã xử lý | Bột đã qua sử dụng | Lợi nhuận tốt nhất | 
| --- | --- | --- | 
| Không có | 0 | 0 | 
| Loại 1 | 0 | 0 | 
| Loại 1 | 2 | 100 | 
| Loại 1 | 4 | 200 | 
| Loại 2 | 4 | 200 | 
| Loại 2 | 8 | 240 | 
| Cuối cùng với bánh trơn | 10 | 241 | 

Kết quả tốt nhất là sử dụng hai chiếc bánh bao thuộc loại nhân thứ nhất, bốn chiếc bánh loại thứ hai và một chiếc bánh bao trơn. Dấu vết cho thấy tại sao chỉ giữ lại lợi nhuận tốt nhất cho mỗi lượng bột là đủ. 

Đối với mẫu thứ hai:```
100 1 25 50
15 5 20 10
```| Loại đã xử lý | Bột đã qua sử dụng | Lợi nhuận tốt nhất | 
| --- | --- | --- | 
| Không có | 0 | 0 | 
| Loại nhồi | 0 | 0 | 
| Loại nhồi | 20 | 10 | 
| Cuối cùng với bánh trơn | 100 | 200 | 

Những chiếc bánh nhồi không đủ hữu ích để đánh bại những chiếc bánh thường. Bước chuyển đổi cuối cùng sẽ lấp đầy toàn bộ khối bột một cách chính xác bằng bốn chiếc bánh đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * n * 100) | Đối với mỗi loại nhân, mỗi lượng bột sẽ cố gắng đạt tới 100 số lượng bánh có thể vì số lượng nhân được giới hạn bởi 100 | 
| Không gian | O(n) | Chỉ lớp DP trước và lớp hiện tại được lưu trữ | 

Giải pháp phù hợp vì kích thước bột chỉ là 1000 và số loại nhân nhiều nhất là 10. Tổng số thao tác vẫn đủ nhỏ trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline

    n, m, c0, d0 = map(int, data().split())
    items = []
    for _ in range(m):
        a, b, c, d = map(int, data().split())
        items.append((a // b, c, d))

    dp = [-1] * (n + 1)
    dp[0] = 0

    for limit, cost, value in items:
        ndp = [-1] * (n + 1)
        for used in range(n + 1):
            if dp[used] == -1:
                continue
            for cnt in range(limit + 1):
                nxt = used + cnt * cost
                if nxt > n:
                    break
                ndp[nxt] = max(ndp[nxt], dp[used] + cnt * value)
        dp = ndp

    ans = 0
    for i in range(n + 1):
        if dp[i] != -1:
            ans = max(ans, dp[i] + ((n - i) // c0) * d0)

    sys.stdin = old
    return str(ans)

assert run("""10 2 2 1
7 3 2 100
12 3 1 10
""") == "241"

assert run("""100 1 25 50
15 5 20 10
""") == "200"

assert run("""1 1 1 5
1 1 1 100
""") == "100"

assert run("""10 1 3 100
5 10 2 1
""") == "300"

assert run("""20 2 10 1
100 2 5 10
4 10 10 100
""") == "40"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu đầu tiên | 241 | Bánh bao nhân thường và bánh thường | 
| Mẫu thứ hai | 200 | Bánh thường ngon hơn bánh nhồi | 
|`1 1 1 5 / 1 1 1 100`| 100 | Ranh giới bột tối thiểu | 
|`10 1 3 100 / 5 10 2 1`| 300 | Không thể sử dụng nội dung không có sẵn | 
|`20 2 10 1 / 100 2 5 10 / 4 10 10 100`| 40 | Xử lý giới hạn nhồi | 

## Vỏ cạnh 

Khi không có đủ nhân để tạo ra dù chỉ một chiếc bánh nhồi, thuật toán sẽ không thực hiện được những chuyển đổi đó. Đối với trường hợp:```
10 1 3 100
5 10 2 1
```giới hạn nhồi trở thành 0 vì`5 // 10 = 0`. DP chỉ giữ lại lựa chọn nhồi trống và bước cuối cùng tạo ra ba chiếc bánh đơn giản để kiếm được lợi nhuận là 300. 

Khi nội dung có vẻ có lợi nhất không phải là lựa chọn tổng thể tốt nhất, DP vẫn hoạt động vì nó so sánh các kết hợp hoàn chỉnh. TRONG:```
10 2 5 1
100 1 5 10
2 10 1 100
```loại thứ hai không thể sử dụng đủ vì khả năng nhồi của nó có hạn. Các chuyển đổi bao gồm các lựa chọn loại đầu tiên và câu trả lời cuối cùng là 10. 

Khi tất cả bột nên được sử dụng cho bánh mì thường, giai đoạn chuyển qua cuối cùng sẽ xử lý nó một cách tự nhiên. Trạng thái DP với số bột đã sử dụng bằng 0 vẫn hợp lệ và phép tính bột còn lại sẽ tạo ra số lượng bánh mì đơn giản tối đa. Điều này tránh cần một trường hợp đặc biệt.
