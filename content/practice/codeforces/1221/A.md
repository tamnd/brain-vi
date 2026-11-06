---
title: "Trò chơi CF 1221A - 2048"
description: "Chúng ta được cung cấp một số trạng thái trò chơi độc lập. Mỗi trạng thái bao gồm các ô có giá trị là lũy thừa của hai. Trong một lần di chuyển, chúng ta có thể chọn hai ô có cùng giá trị và hợp nhất chúng thành một ô duy nhất có giá trị bằng tổng của chúng."
date: "2026-06-11T22:36:51+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1221
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 73 (Rated for Div. 2)"
rating: 1000
weight: 1221
solve_time_s: 96
verified: true
draft: false
---

[Trò chơi CF 1221A - 2048](https://codeforces.com/problemset/problem/1221/A) 

**Đánh giá:** 1000 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số trạng thái trò chơi độc lập. Mỗi trạng thái bao gồm các ô có giá trị là lũy thừa của hai. Trong một lần di chuyển, chúng ta có thể chọn hai ô có cùng giá trị và hợp nhất chúng thành một ô duy nhất có giá trị bằng tổng của chúng. 

Câu hỏi đặt ra là liệu có thể lấy được một ô có giá trị`2048`sau khi thực hiện bất kỳ số lần hợp nhất nào. 

Hoạt động hoàn toàn giống như trong trò chơi 2048. Hai lũy thừa bằng nhau của hai kết hợp thành lũy thừa lớn hơn tiếp theo của hai. Vì mọi giá trị đều đã là lũy thừa của hai nên mọi sự hợp nhất cũng tạo ra lũy thừa của hai. 

Những hạn chế là rất nhỏ. Có tối đa 100 truy vấn và mỗi truy vấn chứa tối đa 100 số. Ngay cả một mô phỏng sử dụng tần số lũy thừa của hai cũng có thể dễ dàng đủ nhanh. Chúng tôi không cần bất kỳ cấu trúc dữ liệu phức tạp nào. 

Quan sát quan trọng xuất phát từ việc hiểu loại gạch nào có thể đóng góp cho tương lai`2048`. 

Bất kỳ ô nào lớn hơn`2048`là vô ích. Vì việc hợp nhất chỉ làm tăng giá trị nên một ô như`4096`không bao giờ có thể trở thành`2048`. Nó cũng không thể hợp nhất với các ô nhỏ hơn để tạo`2048`, vì việc hợp nhất chỉ được phép giữa các giá trị bằng nhau. 

Một trường hợp tinh vi khác là khi tổng số ô có thể sử dụng đạt tới`2048`, mặc dù không có ô riêng lẻ nào bắt đầu gần giá trị đó. 

Ví dụ:```
1
11
1 1 2 2 4 4 8 8 16 16 1024
```Tổng số tiền có thể sử dụng là:```
1+1+2+2+4+4+8+8+16+16+1024 = 1086
```điều đó là không đủ, vì vậy câu trả lời là`NO`. 

Một ví dụ khác:```
1
2
1024 1024
```Câu trả lời là`YES`bởi vì hai`1024`gạch hợp nhất trực tiếp vào`2048`. 

Một lỗi phổ biến là bao gồm các giá trị lớn hơn`2048`trong tổng số:```
1
2
4096 4
```Câu trả lời đúng là`NO`. 

Một giải pháp bất cẩn có thể tính ra tổng số tiền`4100 ≥ 2048`và trả lời sai`YES`, mặc dù`4096`gạch không bao giờ có thể giúp sản xuất`2048`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng trò chơi. Chúng ta có thể đếm được có bao nhiêu viên gạch có lũy thừa hai. Bất cứ khi nào có ít nhất hai bản sao của một giá trị nào đó`x`, chúng tôi hợp nhất các cặp của chúng thành giá trị`2x`. Việc lặp lại quá trình này cuối cùng sẽ tạo ra mọi ô có thể được tạo. 

Mô phỏng này là chính xác vì mọi trình tự hợp nhất hợp pháp đều tạo ra sự lan truyền mang cuối cùng giống hệt nhau. Vấn đề là có nhiều máy móc hơn mức chúng ta thực sự cần. 

Quan sát quan trọng là mọi ô có giá trị lớn nhất`2048`luôn có thể được hợp nhất nhiều như cần thiết. Quyền hạn của hai hành xử giống hệt như mang nhị phân. 

Giả sử chúng ta lấy tất cả các giá trị không vượt quá`2048`và cộng chúng lại với nhau. Nếu tổng số tiền của họ ít nhất là`2048`, thì chúng ta luôn có thể thực hiện việc hợp nhất cho đến khi`2048`gạch xuất hiện. Lý do là sự hợp nhất lặp đi lặp lại chỉ đơn giản là chuyển đổi tổng khối lượng này thành lũy thừa lớn hơn và lớn hơn của hai. 

Nếu tổng số tiền của tất cả các ô không vượt quá`2048`ít hơn`2048`, sau đó tạo một`2048`ô là không thể vì việc hợp nhất không bao giờ làm tăng tổng số tiền. 

Điều này làm giảm toàn bộ vấn đề thành một tổng đơn giản. 

Đối với mỗi truy vấn, bỏ qua mọi số lớn hơn`2048`, cộng các số còn lại và kiểm tra xem tổng có nhỏ nhất không`2048`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n + log V) | O(log V) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Đây`V`là lũy thừa lớn nhất của hai có thể xuất hiện trong quá trình mô phỏng. 

## Hướng dẫn thuật toán 

1. Đọc số lượng truy vấn. 
2. Đối với mỗi truy vấn, hãy đọc tất cả các giá trị ô. 
3. Khởi tạo tổng hiện có bằng 0. 
4. Đối với mỗi ô, hãy kiểm tra xem giá trị của nó có lớn nhất không`2048`. 
5. Nếu giá trị lớn nhất`2048`, thêm nó vào tổng số hiện có. 

Giá trị lớn hơn`2048`không bao giờ có thể góp phần tạo ra một`2048`gạch, vì vậy chúng bị bỏ qua. 
6. Sau khi xử lý tất cả các ô, hãy so sánh tổng với`2048`. 
7. Nếu tổng ít nhất`2048`, in`"YES"`. 
8. Nếu không, hãy in`"NO"`. 

### Tại sao nó hoạt động 

Tổng số của tất cả các ô được bảo toàn bằng mọi thao tác hợp nhất. Hai viên gạch bằng nhau`x + x`trở thành một viên gạch duy nhất`2x`, do đó không có giá trị nào bị mất hoặc thu được. 

Mỗi viên gạch lớn hơn`2048`không liên quan vì việc hợp nhất không bao giờ làm giảm giá trị. Một viên gạch như vậy không bao giờ có thể trở thành`2048`, nó cũng không thể hợp nhất với các giá trị nhỏ hơn để tạo`2048`. 

Trong số các ô có giá trị không vượt quá`2048`, các phép hợp nhất lặp lại hoạt động giống hệt như thực hiện phép cộng nhị phân. Nếu tổng số tiền của họ ít nhất là`2048`, những thứ đó cuối cùng tạo ra một`2048`ngói. Nếu tổng số tiền của họ nhỏ hơn`2048`, không có chuỗi hợp nhất nào có thể tạo ra một ô có giá trị vượt quá tổng khối lượng có sẵn. 

Như vậy điều kiện```
sum(all values ≤ 2048) ≥ 2048
```là vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())

    for _ in range(q):
        n = int(input())
        arr = list(map(int, input().split()))

        total = 0
        for x in arr:
            if x <= 2048:
                total += x

        print("YES" if total >= 2048 else "NO")

solve()
```Việc thực hiện tuân theo bằng chứng trực tiếp. 

Biến`total`lưu trữ tổng của mỗi ô có thể đóng góp cho tương lai`2048`. Bất kỳ giá trị nào lớn hơn`2048`được bỏ qua ngay lập tức. 

Sau khi xử lý một truy vấn, một so sánh duy nhất sẽ xác định câu trả lời. 

Không có mối lo ngại về tràn trong Python. Ngay cả trong trường hợp xấu nhất, tổng vẫn thấp hơn nhiều so với giới hạn số nguyên của Python. 

Lỗi triển khai phổ biến nhất là quên bỏ qua các giá trị lớn hơn`2048`. Làm như vậy sẽ xử lý không đúng các ô như`4096`như những người đóng góp hữu ích. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1024 512 64 512
```| Ngói | Bao gồm? | Tổng Chạy | 
| --- | --- | --- | 
| 1024 | Có | 1024 | 
| 512 | Có | 1536 | 
| 64 | Có | 1600 | 
| 512 | Có | 2112 | 

Tổng cuối cùng = 2112. 

Kể từ khi`2112 ≥ 2048`, câu trả lời là:```
YES
```Điều này chứng tỏ rằng chúng ta không cần mô phỏng rõ ràng việc hợp nhất. Khối lượng có sẵn đã vượt quá`2048`, vì vậy một`2048`gạch có thể được hình thành. 

### Ví dụ 2 

đầu vào:```
2
4096 4
```| Ngói | Bao gồm? | Tổng Chạy | 
| --- | --- | --- | 
| 4096 | Không | 0 | 
| 4 | Có | 4 | 

Tổng cuối cùng = 4. 

Kể từ khi`4 < 2048`, câu trả lời là:```
NO
```Ví dụ này cho thấy tại sao các giá trị lớn hơn`2048`phải được bỏ qua. các`4096`ô không thể giúp tạo ra một`2048`ngói. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ô được xử lý chính xác một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Đối với mỗi truy vấn, chúng tôi thực hiện quét tuyến tính duy nhất qua tối đa 100 số. Ngay cả trên tất cả các trường hợp thử nghiệm, khối lượng công việc rất nhỏ so với giới hạn, do đó giải pháp dễ dàng phù hợp với cả giới hạn về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    q = int(input())
    ans = []

    for _ in range(q):
        n = int(input())
        arr = list(map(int, input().split()))

        total = sum(x for x in arr if x <= 2048)
        ans.append("YES" if total >= 2048 else "NO")

    return "\n".join(ans)

# provided sample
assert run(
"""6
4
1024 512 64 512
1
2048
3
64 512 2
2
4096 4
7
2048 2 2048 2048 2048 2048 2048
2
2048 4096
"""
) == """YES
YES
NO
NO
YES
YES"""

# minimum size
assert run(
"""1
1
1
"""
) == "NO", "single small tile"

# exact boundary
assert run(
"""1
2
1024 1024
"""
) == "YES", "two tiles merge into 2048"

# large value should be ignored
assert run(
"""1
2
4096 1024
"""
) == "NO", "4096 cannot help create 2048"

# many small values
assert run(
"""1
11
1 1 2 2 4 4 8 8 16 16 1986
"""
) == "YES", "sum of usable tiles reaches 2048"

# all equal values
assert run(
"""1
100
32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32
"""
) == "YES", "100 * 32 = 3200"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`NO`| Truy vấn kích thước tối thiểu | 
|`1024 1024`|`YES`| Hợp nhất chính xác vào năm 2048 | 
|`4096 1024`|`NO`| Giá trị lớn hơn phải được bỏ qua | 
| Quyền hạn nhỏ tổng hợp đến 2048+ |`YES`| Hành vi mang nhị phân | 
| 100 bản 32 |`YES`| Đa bội lớn | 

## Vỏ cạnh 

Hãy xem xét:```
1
1
2048
```Thuật toán bổ sung`2048`vào tổng số tiền hiện có, thu được`2048`. Vì số tiền đó ít nhất đã là`2048`, nó xuất ra`YES`. Không cần phải hợp nhất vì ngăn xếp mục tiêu đã tồn tại. 

Bây giờ hãy xem xét:```
1
2
4096 4
```Thuật toán bỏ qua`4096`và chỉ giữ lại`4`. Tổng kết quả là`4`, vì vậy nó xuất ra`NO`. Điều này phù hợp với luật chơi vì giá trị không bao giờ giảm. 

Cuối cùng, hãy xem xét:```
1
4
512 512 512 512
```Tổng số tiền chạy trở thành`2048`. Đầu ra của thuật toán`YES`. 

Truy tìm sự hợp nhất thực tế:```
512 + 512 -> 1024
512 + 512 -> 1024
1024 + 1024 -> 2048
```Điều này xác nhận tính bất biến chính đằng sau lời giải: tất cả các viên gạch có thể sử dụng được đều đóng góp tổng khối lượng của chúng vào việc xây dựng một`2048`ngói, và chỉ có tổng số là quan trọng.
