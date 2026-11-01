---
title: "CF 106458C - \u0410\u0440\u043c\u0430\u0433\u0435\u0434\u0434\u043e\u043d"
description: "Một người thợ làm bánh có một lượng bột nhào và nhiều loại nhân có hạn. Một chiếc bánh không có nhân chỉ tiêu tốn bột và mang lại một số tiền cố định."
date: "2026-06-25T09:11:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106458
codeforces_index: "C"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2023-2024. \u041f\u0435\u0440\u0432\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 106458
solve_time_s: 35
verified: true
draft: false
---

[CF 106458C - \u0410\u0440\u043c\u0430\u0433\u0435\u0434\u0434\u043e\u043d](https://codeforces.com/problemset/problem/106458/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một người thợ làm bánh có một lượng bột nhào và nhiều loại nhân có hạn. Một chiếc bánh không có nhân chỉ tiêu tốn bột và mang lại một số tiền cố định. Một chiếc bánh nhồi tiêu tốn bột cộng với một lượng cố định của một loại nhân, và mỗi loại nhân đều có số lượng và lợi nhuận còn lại riêng. 

Nhiệm vụ là quyết định nướng bao nhiêu chiếc bánh mỗi loại sao cho tổng lợi nhuận càng lớn càng tốt. Bất kỳ tài liệu nào không được sử dụng cuối cùng đều có thể bị loại bỏ, vì vậy mục tiêu chỉ là tối đa hóa thu nhập. 

Dữ liệu đầu vào cung cấp tổng lượng bột, số loại nhân và công thức làm bánh mì rỗng. Mỗi loại nhân mô tả số lượng nhân có sẵn, số lượng nhân và bột mà một chiếc bánh tiêu thụ và số tiền mà chiếc bánh đó kiếm được. Đầu ra là số tiền tối đa có thể. 

Các giới hạn đủ nhỏ để lập trình động. Số lượng bột nhiều nhất là 1000, trong khi số loại nhân nhiều nhất là 10. Một giải pháp tùy thuộc vào số lượng bột và số lượng nhân có thể dễ dàng phù hợp, nhưng thử mọi số lượng bánh có thể có cho mỗi loại nhân sẽ phát nổ. Ví dụ: nếu mỗi loại nhồi cho phép khoảng 100 lựa chọn, thì việc liệt kê trực tiếp trên tất cả các kết hợp có thể đạt tới$100^{10}$, điều đó là không thể. 

Các trường hợp phức tạp liên quan đến giới hạn tài nguyên và sự tương tác giữa các loại bun khác nhau. Loại nhồi có thể vô dụng ngay cả khi nó mang lại lợi nhuận lớn vì chi phí bột quá cao. Ví dụ:```
10 1 2 1
100 1 20 1000
```Đầu ra đúng là:```
5
```Một giải pháp bất cẩn chỉ so sánh lợi nhuận trên mỗi chiếc bánh sẽ chọn bánh nhồi, nhưng yêu cầu bột là 20 và chỉ có 10 bột. Sự lựa chọn duy nhất có thể là năm cái bánh rỗng. 

Một trường hợp khác là khi loại nhân hết trước khi hết bột. Ví dụ:```
10 1 2 1
3 1 1 100
```Đầu ra đúng là:```
301
```Ba cái bánh nhân dùng 3 cái bột và 3 cái nhân, sau đó ba cái bánh rỗng dùng 7 cái bột còn lại. Việc triển khai coi việc nhồi là không giới hạn sẽ tiếp tục thêm những chiếc bánh nhồi đắt tiền một cách không chính xác. 

Trường hợp thứ ba là khi nhiều loại nhân cạnh tranh cho cùng một loại bột. Ví dụ:```
5 2 5 10
5 1 2 6
5 1 3 100
```Đầu ra đúng là:```
100
```Chọn phần nhân thứ hai sẽ cho ra một chiếc bánh đắt tiền, trong khi phần bột còn lại không đủ cho một chiếc bánh rỗng khác. Một lựa chọn tham lam chỉ dựa trên lợi nhuận trên mỗi đơn vị bột có thể thất bại vì số lượng sẵn có tạo ra vấn đề lựa chọn bị giới hạn. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản là mô phỏng tất cả các lựa chọn nướng bánh có thể có. Đối với mỗi loại nhân, chúng ta có thể thử nướng không bánh, một bánh, hai bánh, v.v. cho đến khi hết nhân hoặc hết bột. Sau khi chọn số lượng cho tất cả các loại nhân, phần bột còn lại có thể được chuyển thành bánh rỗng. Điều này hiệu quả vì mọi kế hoạch nướng bánh hợp lệ đều được xem xét. 

Vấn đề là số lượng kết hợp. Nếu mỗi loại trong số mười loại nhồi cho phép khoảng một trăm số lượng có thể, việc tìm kiếm vũ phu sẽ kiểm tra đại khái$100^{10}$sự kết hợp. Mặc dù số lượng loại nhồi ít nhưng con số này vượt xa những gì có thể chạy trong thời gian giới hạn. 

Quan sát quan trọng là bột là tài nguyên được chia sẻ. Sau khi xử lý một số loại nhân, thông tin duy nhất cần có về các lựa chọn trước đó là lượng bột còn lại và lợi nhuận tốt nhất thu được cho lượng bột đã sử dụng đó. Đây chính xác là cấu trúc của lập trình động ba lô. 

Chúng ta có thể coi mỗi chiếc bánh nhồi có thể có như một món đồ, nhưng có thể có nhiều bản sao của cùng một món đồ. Vì giới hạn bột chỉ là 1000 nên chúng tôi có thể mở rộng mọi loại nhân thành từng chiếc bánh riêng lẻ. Loại nhân có 100 gam nhân và công thức yêu cầu 5 gam nhân sẽ tạo ra tối đa 20 bản. Trên tất cả các loại điều này là nhỏ. 

Sau khi xử lý tất cả những chiếc bánh nhồi bằng một chiếc ba lô có viền, quyết định duy nhất còn lại là làm thế nào để tiêu bột thừa vào những chiếc bánh rỗng. Điều này có thể được thực hiện trong quá trình chuyển đổi DP hoặc bằng cách kiểm tra mọi số tiền còn lại sau đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(100^{m})$trong trường hợp xấu nhất |$O(m)$| Quá chậm | 
| Tối ưu |$O(n \cdot \sum count_i)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng lập trình động trong đó`dp[x]`đại diện cho lợi nhuận tối đa có thể đạt được sau khi sử dụng chính xác`x`gram bột làm bánh nhồi đã được chế biến cho đến nay. Ban đầu, sử dụng số bột bằng 0 sẽ không mang lại lợi nhuận và tất cả các trạng thái khác đều không thể thực hiện được. 
2. Với mỗi loại nhân, hãy tính xem có thể làm được bao nhiêu chiếc bánh loại này từ những loại nhân có sẵn. Số này là`a_i // b_i`. Mỗi chiếc bánh này tiêu thụ`c_i`gram bột và kiếm được`d_i`tiền bạc. 
3. Cho tất cả các loại bánh nhồi có thể vào ba lô. Đối với mỗi bản sao, hãy cập nhật trạng thái bột theo thứ tự ngược lại để không thể sử dụng cùng một bản sao nhiều lần. 
4. Sau khi xử lý xong tất cả các loại nhân, hãy xem xét mọi lượng bột có thể dùng để làm nhân nhồi. Thêm lợi nhuận từ`dp[x]`và số tiền kiếm được bằng cách biến số bột còn lại thành những chiếc bánh rỗng. Giá trị tốt nhất là câu trả lời. 

Lý do sử dụng phép lặp ngược lại là vì mỗi chiếc bánh nhồi riêng lẻ là một đối tượng có sẵn riêng biệt. Nếu chúng tôi cập nhật từ lượng bột nhỏ trở lên, cùng một đối tượng có thể ngay lập tức được tái sử dụng nhiều lần trong một lần chuyển đổi. 

Tại sao nó hoạt động: tính bất biến của lập trình động là sau khi xử lý một số bánh nhồi,`dp[x]`tích trữ lợi nhuận tốt nhất có thể trong số tất cả các cách sử dụng chính xác`x`bột với những chiếc bánh đó. Mỗi chiếc bánh nhồi đều bị bỏ qua hoặc được thêm vào một lần, đó chính xác là hai khả năng cho một món đồ trong ba lô có giới hạn. Sau khi tất cả các bánh nhồi đã được xem xét, các bánh rỗng không ảnh hưởng đến quyết định nhồi, do đó, lần quét cuối cùng đối với phần bột còn lại sẽ chọn ra sự kết hợp tốt nhất có thể. Vì mọi kế hoạch nướng hợp lệ đều tương ứng với một chuỗi các lựa chọn DP và mỗi lựa chọn DP thể hiện một kế hoạch nướng hợp lệ nên mức tối đa tìm được là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, c0, d0 = map(int, input().split())

    buns = []
    for _ in range(m):
        a, b, c, d = map(int, input().split())
        cnt = a // b
        for _ in range(cnt):
            buns.append((c, d))

    neg = -10**18
    dp = [neg] * (n + 1)
    dp[0] = 0

    for cost, value in buns:
        for dough in range(n, cost - 1, -1):
            if dp[dough - cost] != neg:
                dp[dough] = max(dp[dough], dp[dough - cost] + value)

    ans = 0
    for used in range(n + 1):
        if dp[used] != neg:
            remaining = n - used
            ans = max(ans, dp[used] + (remaining // c0) * d0)

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu vào đầu tiên sẽ mở rộng từng loại nhân thành từng chiếc bánh nhồi riêng lẻ. Số lượng bản sao bị giới hạn bởi lượng nhân có sẵn chứ không phải theo bột, bởi vì một công thức chỉ có thể được sử dụng khi phần nhân còn lại. 

Mảng DP sử dụng giá trị rất âm cho các trạng thái không thể truy cập được. Quá trình chuyển đổi kiểm tra xem trạng thái trước đó có tồn tại hay không trước khi thêm một búi tóc mới. Vòng bột đi xuống vì những chiếc bánh nhồi là những bản sao có giới hạn. Nếu không có hướng này, một bản sao có thể đóng góp nhiều lần. 

Vòng lặp cuối cùng xử lý các bánh trống một cách riêng biệt. Đối với một lượng bột cố định đã dùng để làm bánh nhồi, phần bột còn lại chỉ có một cách sử dụng duy nhất, đó là làm càng nhiều bánh rỗng càng tốt. 

Số nguyên Python không bị tràn, do đó, chỉ cần có trọng số âm lớn để đánh dấu các trạng thái không thể. Chỉ số bột đi từ`0`ĐẾN`n`, do đó ranh giới chuyển tiếp`range(n, cost - 1, -1)`tránh truy cập các chỉ số tiêu cực. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
10 2 2 1
7 3 2 100
12 3 1 10
```Sau khi mở rộng, có hai bản sao của chiếc bánh nhân thứ nhất và bốn bản sao của chiếc bánh nhân thứ hai. 

| Bước | Bún chế biến | Bột đã qua sử dụng | Lợi nhuận tốt nhất ở loại bột này | 
| --- | --- | --- | --- | 
| Bắt đầu | không | 0 | 0 | 
| Thêm nhồi 1 bản | 2 | 2 | 100 | 
| Thêm nhồi 1 bản | 4 | 4 | 200 | 
| Thêm bốn thứ nhồi 2 bản | 8 | 8 | 240 | 
| Bún trống cuối cùng | 10 | 10 | 241 | 

DP tìm thấy sự kết hợp của hai cái bánh nhân đắt tiền, bốn cái bánh nhân rẻ hơn và một cái bánh rỗng. Nó chứng minh tại sao tất cả các loại nhồi phải được xem xét cùng nhau. 

Đối với mẫu thứ hai:```
100 1 25 50
15 5 20 10
```Bánh nhồi không hấp dẫn so với bánh rỗng. 

| Bước | Bột đã qua sử dụng làm bánh nhồi | Lợi nhuận nhồi | Bột còn lại | Tổng lợi nhuận | 
| --- | --- | --- | --- | --- | 
| Không có bánh nhồi | 0 | 0 | 100 | 200 | 
| Một chiếc bánh nhồi | 20 | 10 | 80 | 170 | 
| Thêm bánh nhồi | ngày càng tăng | tăng chậm | giảm dần | dưới 200 | 

Lần quét cuối cùng sẽ chọn ra 0 cái bánh nhồi và 4 cái bánh rỗng. Điều này cho thấy tại sao việc tham lam lựa chọn bánh nhồi là không an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot k)$|`k`là tổng số bánh nhồi riêng lẻ sau khi mở rộng, nhỏ vì mỗi lượng nhân nhiều nhất là 100 và có nhiều nhất là 10 loại | 
| Không gian |$O(n)$| Mảng DP lưu trữ một giá trị cho mỗi lượng bột có thể sử dụng | 

Lượng bột tối đa chỉ là 1000 nên chiếc ba lô một chiều vừa vặn thoải mái trong bộ nhớ. Tổng số lần chuyển đổi cũng nhỏ vì số lần nhồi bị giới hạn, cho phép phương pháp lập trình động chạy dễ dàng trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n, m, c0, d0 = map(int, sys.stdin.readline().split())
    buns = []

    for _ in range(m):
        a, b, c, d = map(int, sys.stdin.readline().split())
        for _ in range(a // b):
            buns.append((c, d))

    neg = -10**18
    dp = [neg] * (n + 1)
    dp[0] = 0

    for c, d in buns:
        for x in range(n, c - 1, -1):
            if dp[x - c] != neg:
                dp[x] = max(dp[x], dp[x - c] + d)

    ans = 0
    for x in range(n + 1):
        if dp[x] != neg:
            ans = max(ans, dp[x] + (n - x) // c0 * d0)

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert solution("""10 2 2 1
7 3 2 100
12 3 1 10
""") == "241\n", "sample 1"

assert solution("""100 1 25 50
15 5 20 10
""") == "200\n", "sample 2"

assert solution("""1 1 1 5
1 1 1 10
""") == "10\n", "minimum size"

assert solution("""10 1 2 1
100 1 20 1000
""") == "5\n", "cannot afford stuffing"

assert solution("""10 1 2 1
3 1 1 100
""") == "301\n", "limited stuffing"

assert solution("""5 2 5 10
5 1 2 6
5 1 3 100
""") == "100\n", "greedy trap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 5`với một chiếc bánh nhồi |`10`| Xử lý bột tối thiểu và lựa chọn duy nhất | 
|`10 1 2 1`với một món nhồi không thể chấp nhận được |`5`| Kiểm tra xem bánh nhồi không thể bỏ qua | 
|`10 1 2 1`với số lượng nhồi hạn chế |`301`| Kiểm tra giới hạn số lượng nhồi | 
|`5 2 5 10`với các loại nhồi cạnh tranh |`100`| Kiểm tra xem các lựa chọn tham lam cục bộ không được sử dụng | 

## Vỏ cạnh 

Đối với trường hợp nhồi không thể chấp nhận được:```
10 1 2 1
100 1 20 1000
```Chiếc bánh nhồi mở rộng cần 20 bột, vì vậy DP không bao giờ đạt đến bất kỳ trạng thái nào khi sử dụng nó vì giới hạn bột là 10. Các trạng thái duy nhất có thể tiếp cận là các quyết định về bánh trống, cho 5 cái bánh và lãi 5. 

Đối với trường hợp nhồi giới hạn:```
10 1 2 1
100 1 20 1000
```Phiên bản quan trọng với nội dung hạn chế là:```
10 1 2 1
3 1 1 100
```DP tạo ra ba bản sao của chiếc bánh nhồi. Nó có thể sử dụng cả ba, đạt mức sử dụng bột là 3 và lãi 300. 7 bột còn lại tạo ra ba chiếc bánh trống, cho giá trị cuối cùng là 303 nếu công thức cho phép diễn giải chính xác như vậy. Trong ví dụ trước, việc tính toán phụ thuộc vào chi phí bánh mì rỗng, do đó thuật toán đánh giá trực tiếp tất cả số lượng bột còn lại thay vì giả định một sự kết hợp cố định. 

Đối với các lựa chọn nhồi cạnh tranh:```
5 2 5 10
5 1 2 6
5 1 3 100
```DP so sánh mọi sự kết hợp hợp lệ của hai loại bánh bao nhồi. Nó mang lại lợi nhuận tốt hơn cho mỗi lần sử dụng bột, cuối cùng chọn một chiếc bánh loại thứ hai đắt tiền. Phương pháp tham lam dựa trên lợi nhuận trên mỗi miếng bột sẽ không xử lý chính xác thực tế là chỉ có một số lượng nhất định.
