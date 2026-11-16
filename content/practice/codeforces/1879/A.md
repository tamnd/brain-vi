---
title: "CF 1879A - Gian lận!"
description: "Chúng ta có một cuộc thi cử tạ với n vận động viên. Vận động viên tôi có sức mạnh si và sức bền ei. Trọng lượng tạ w được chọn. Bất kỳ vận động viên nào có sức mạnh nhỏ hơn w đều không thể nhấc được thanh tạ. Vận động viên nào có sức mạnh ít nhất là w sẽ nâng nó đúng ei lần."
date: "2026-06-08T22:48:36+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1879
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 155 (Rated for Div. 2)"
rating: 800
weight: 1879
solve_time_s: 330
verified: true
draft: false
---

[CF 1879A - Gian lận!](https://codeforces.com/problemset/problem/1879/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 5 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cuộc thi cử tạ với`n`vận động viên. Vận động viên`i`có sức mạnh`s_i`và sức chịu đựng`e_i`. 

Trọng lượng một thanh tạ`w`được chọn. Bất kỳ vận động viên nào có sức mạnh thấp hơn`w`không thể nhấc được thanh tạ chút nào. Bất kỳ vận động viên nào có sức mạnh ít nhất`w`nâng nó lên một cách chính xác`e_i`lần. Người chiến thắng là vận động viên duy nhất có số lần nâng cao nhất. Nếu nhiều vận động viên có số điểm cao nhất hòa thì không ai thắng. 

Polycarp là vận động viên`1`. Chúng ta phải xác định liệu có tồn tại một trọng lượng nào đó hay không`w`điều đó khiến Polycarp trở thành người chiến thắng duy nhất. Nếu trọng số như vậy tồn tại, chúng tôi có thể xuất ra bất kỳ giá trị hợp lệ nào của`w`. Nếu không chúng tôi xuất ra`-1`. 

Những hạn chế là rất nhỏ. Có nhiều nhất`100`vận động viên cho mỗi trường hợp thử nghiệm và nhiều nhất`100`trường hợp thử nghiệm. Thậm chí một`O(n²)`giải pháp là tầm thường ở đây. Các giá trị cường độ lớn không thành vấn đề vì chúng ta không bao giờ cần phải lặp qua tất cả các trọng số có thể có. Chúng ta chỉ cần lý giải xem vận động viên nào vẫn đủ điều kiện cho một lựa chọn nhất định`w`. 

Một sai lầm phổ biến là tập trung vào việc tìm kiếm mức cân chiến thắng rõ ràng ngay lập tức. Coi như:```
Polycarp: (7, 4)
Other:    (9, 3)
```Lựa chọn`w = 5`cho phép cả hai vận động viên thi đấu, nhưng Polycarp thắng vì`4 > 3`. 

Một sai lầm dễ mắc phải khác là quên cà vạt. Coi như:```
Polycarp: (1337, 3)
Other:    (1337, 3)
```Bất kỳ trọng lượng nào cho phép Polycarp cũng cho phép vận động viên khác. Số lần nâng của họ bằng nhau nên không bao giờ có người chiến thắng duy nhất. Câu trả lời là`-1`. 

Trường hợp tinh tế thứ ba xảy ra khi một vận động viên khác khỏe hơn và ít nhất có sức bền tương đương:```
Polycarp: (4, 6)
Other:    (100, 100)
```Bất kỳ trọng lượng nào cho phép Polycarp tham gia cũng cho phép vận động viên khác tham gia, vì vận động viên kia khỏe hơn. Vì vận động viên kia nâng nhiều lần hơn nên Polycarp không bao giờ có thể thắng. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là thử mọi trọng lượng tạ phù hợp và mô phỏng cuộc thi. Vì điểm mạnh có thể lớn bằng`10^9`, việc lặp lại tất cả các trọng số có thể là không thể. Chúng tôi có thể giảm tìm kiếm theo các giá trị sức mạnh riêng biệt xuất hiện trong đầu vào vì tập hợp các vận động viên đủ điều kiện chỉ thay đổi khi chúng tôi vượt qua ngưỡng sức mạnh. Điều đó đã đủ nhanh cho những hạn chế này. 

Có một quan sát thậm chí còn đơn giản hơn. 

Giả sử một vận động viên khác có cả sức mạnh ít nhất là sức bền của Polycarp và sức bền ít nhất là sức bền của Polycarp. Sau đó, bất cứ khi nào Polycarp nâng được thanh tạ thì vận động viên đó cũng có thể nâng được. Vì sức bền của vận động viên không hề nhỏ nên Polycarp không bao giờ có thể là người chiến thắng duy nhất. 

Ngược lại, nếu không có vận động viên nào thỏa mãn đồng thời cả hai điều kiện thì chọn```
w = s₁
```làm cho Polycarp đủ điều kiện. Bất kỳ vận động viên nào có sức mạnh dưới đây`s₁`được loại bỏ ngay lập tức. Bất kỳ vận động viên nào có sức mạnh ít nhất`s₁`phải có sức chịu đựng ít hơn`e₁`, nếu không thì họ sẽ thỏa mãn cả hai điều kiện. Do đó Polycarp có sức chịu đựng lớn hơn mọi đối thủ còn lại và trở thành người chiến thắng duy nhất. 

Điều này biến toàn bộ vấn đề thành việc kiểm tra xem có tồn tại một vận động viên không`i > 1`như vậy```
s_i ≥ s₁ and e_i ≥ e₁.
```Nếu một vận động viên như vậy tồn tại thì câu trả lời là`-1`. Nếu không thì`w = s₁`hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua trọng lượng phù hợp | O(n²) | O(1) | Đã chấp nhận | 
| Quan sát thống trị | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc sức mạnh của Polycarp`s₁`và sức chịu đựng`e₁`. 
2. Đối với mọi vận động viên khác, hãy kiểm tra xem cả hai điều kiện có thỏa mãn không:```
s_i ≥ s₁
e_i ≥ e₁
```3. Nếu có một vận động viên như vậy, hãy in`-1`. 

Vận động viên này ít nhất cũng khỏe và ít nhất là bền bỉ như Polycarp. Bất cứ khi nào Polycarp có thể tham gia, vận động viên này cũng có thể tham gia, ngăn cản chiến thắng duy nhất của Polycarp. 
4. Nếu không có vận động viên như vậy tồn tại, hãy in`s₁`. 

Lựa chọn`w = s₁`đảm bảo rằng Polycarp tham gia. Mọi vận động viên tham gia khác phải có sức chịu đựng hoàn toàn thấp hơn`e₁`, do đó Polycarp thắng duy nhất. 

### Tại sao nó hoạt động 

Một vận động viên chỉ có thể đe dọa Polycarp nếu họ vẫn đủ điều kiện bất cứ khi nào Polycarp đủ điều kiện. Điều đó đòi hỏi sức mạnh ít nhất`s₁`. 

Trong số những vận động viên đó, những người duy nhất có khả năng ngăn Polycarp trở thành người chiến thắng duy nhất là những người có sức bền ít nhất là`e₁`. 

Vì vậy sự tồn tại của một vận động viên với```
s_i ≥ s₁ and e_i ≥ e₁
```chính xác là điều kiện khiến chiến thắng không thể xảy ra. 

Nếu không có vận động viên như vậy tồn tại, việc chọn`w = s₁`loại bỏ tất cả các vận động viên yếu hơn và chỉ để lại những vận động viên có sức bền nhỏ hơn Polycarp. Vì vậy Polycarp trở thành người chiến thắng duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())

        s1, e1 = map(int, input().split())
        possible = True

        for _ in range(n - 1):
            s, e = map(int, input().split())

            if s >= s1 and e >= e1:
                possible = False

        print(s1 if possible else -1)

solve()
```Việc thực hiện tuân theo bằng chứng trực tiếp. 

Chúng tôi lưu trữ các thông số của Polycarp và quét qua các vận động viên còn lại một lần. Nếu chúng tôi tìm thấy một vận động viên có sức mạnh và sức bền ít nhất đều bằng giá trị của Polycarp, chúng tôi đánh dấu câu trả lời là không thể. 

Không cần phải lưu trữ tất cả các vận động viên vì mỗi vận động viên được kiểm tra độc lập. Giải pháp sử dụng bộ nhớ bổ sung liên tục. 

Việc so sánh phải sử dụng`>=`còn hơn là`>`. Một vận động viên có cùng sức bền vẫn gây ra tỷ số hòa và ngăn cản người chiến thắng duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
7 4
9 3
4 6
2 2
```| Vận động viên | Sức mạnh ≥ 7? | Sức chịu đựng ≥ 4? | Khối Polycarp? | 
| --- | --- | --- | --- | 
| (9, 3) | Có | Không | Không | 
| (4, 6) | Không | Có | Không | 
| (2, 2) | Không | Không | Không | 

Không có vận động viên nào thỏa mãn cả hai điều kiện. 

Đầu ra:```
7
```Một câu trả lời mẫu hợp lệ là`5`, nhưng mọi trọng lượng hợp lệ đều được chấp nhận. Lựa chọn`w = 7`cũng khiến Polycarp trở thành người chiến thắng duy nhất. 

### Ví dụ 2 

đầu vào:```
2
4 6
100 100
```| Vận động viên | Sức mạnh ≥ 4? | Sức chịu đựng ≥ 6? | Khối Polycarp? | 
| --- | --- | --- | --- | 
| (100, 100) | Có | Có | Có | 

Một vận động viên chặn tồn tại. 

Đầu ra:```
-1
```Ví dụ này thể hiện điều kiện cốt lõi không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua các vận động viên | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Từ`n ≤ 100`, giải pháp nằm dưới giới hạn thời gian rất nhiều. Ngay cả với số lượng trường hợp thử nghiệm tối đa, tổng khối lượng công việc vẫn rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = []
    t = int(input())

    for _ in range(t):
        n = int(input())

        s1, e1 = map(int, input().split())
        ok = True

        for _ in range(n - 1):
            s, e = map(int, input().split())
            if s >= s1 and e >= e1:
                ok = False

        out.append(str(s1 if ok else -1))

    return "\n".join(out)

# provided sample
assert run(
"""3
4
7 4
9 3
4 6
2 2
2
4 6
100 100
2
1337 3
1337 3
"""
) == "7\n-1\n-1"

# equal athlete causes tie
assert run(
"""1
2
10 5
10 5
"""
) == "-1"

# stronger but lower endurance does not block
assert run(
"""1
2
10 5
100 4
"""
) == "10"

# weaker but higher endurance does not block
assert run(
"""1
2
10 5
9 100
"""
) == "10"

# minimum n
assert run(
"""1
2
1 1
2 2
"""
) == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vận động viên bình đẳng | -1 | Hòa ngăn cản người chiến thắng duy nhất | 
| Mạnh hơn, sức chịu đựng thấp hơn | Sức mạnh của Polycarp | Cả hai điều kiện đều được yêu cầu | 
| Yếu hơn, sức chịu đựng cao hơn | Sức mạnh của Polycarp | Vấn đề về tình trạng sức mạnh | 
| Trường hợp kích thước tối thiểu | -1 | Đầu vào hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
10 5
10 5
```Vận động viên thứ hai có thông số giống hệt nhau. Thuật toán phát hiện```
10 ≥ 10
5 ≥ 5
```và đầu ra`-1`. Bất kỳ trọng lượng nào cho phép Polycarp cũng cho phép vận động viên khác, tạo ra một trận hòa. 

Coi như:```
1
2
10 5
100 4
```Vận động viên thứ hai khỏe hơn nhưng sức bền kém hơn. Thuật toán thấy rằng điều kiện độ bền không thành công và xuất ra`10`. Lựa chọn`w = 10`cho phép cả hai vận động viên thi đấu, nhưng Polycarp nâng thanh tạ năm lần trong khi vận động viên kia nâng nó bốn lần. 

Coi như:```
1
2
10 5
9 100
```Vận động viên thứ hai có sức bền rất lớn nhưng lại không đủ sức. Lựa chọn`w = 10`loại bỏ hoàn toàn vận động viên đó. Thuật toán xuất ra chính xác`10`. 

Coi như:```
1
2
4 6
100 100
```Vận động viên thứ hai lấn át Polycarp cả về sức mạnh lẫn sức bền. Bất kể trọng lượng hợp lệ nào được chọn, nếu Polycarp có thể nâng được thanh tạ thì vận động viên thứ hai cũng có thể nâng được và vận động viên thứ hai sẽ nâng được nhiều lần hơn. Thuật toán phát hiện sự thống trị này và đưa ra`-1`.
