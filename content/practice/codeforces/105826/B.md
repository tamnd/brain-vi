---
title: "CF 105826B - \u0417\u0430\u043a\u0440\u0430\u0441\u043a\u0430 \u0442\u043e\u0447\u0435\u043a"
description: "Chúng ta có một đa giác đều có n đỉnh được đánh số. Bắt đầu từ đỉnh đầu tiên, chúng ta liên tục di chuyển một bước k cố định xung quanh đa giác và vẽ các đoạn giữa các đỉnh đã thăm. Mọi đỉnh mà quá trình này đạt tới sẽ được sơn."
date: "2026-06-25T14:57:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105826
codeforces_index: "B"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 2025"
rating: 0
weight: 105826
solve_time_s: 35
verified: true
draft: false
---

[CF 105826B - \u0417\u0430\u043a\u0440\u0430\u0441\u043a\u0430 \u0442\u043e\u0447\u0435\u043a](https://codeforces.com/problemset/problem/105826/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác đều với`n`các đỉnh được đánh số. Bắt đầu từ đỉnh đầu tiên, chúng ta liên tục di chuyển theo một bước cố định`k`xung quanh đa giác và vẽ các đoạn giữa các đỉnh đã thăm. Mọi đỉnh mà quá trình này đạt tới sẽ được sơn. Nhiệm vụ là chọn giá trị hợp lệ nhỏ nhất`k`sao cho số đỉnh được sơn càng nhỏ càng tốt. 

Đầu vào chỉ chứa số đỉnh. Đầu ra là kích thước bước tạo ra tập hợp các đỉnh được thăm nhỏ nhất có thể, với bước nhỏ nhất được chọn trong số tất cả các lựa chọn tối ưu. 

Hạn chế quan trọng đó là`n`có thể lớn như`10^10`, vì vậy các giải pháp lặp qua tất cả các bước có thể hoặc tất cả các đỉnh là không thể. Thậm chí một`O(n)`thuật toán có thể thực hiện tới mười tỷ thao tác, vượt xa giới hạn cuộc thi thông thường cho phép. Chúng ta cần sử dụng lý thuyết số và làm việc với các ước của`n`thay vì chính đa giác đó. 

Một sai lầm phổ biến là cho rằng một bước lớn luôn vẽ ra ít điểm hơn. Số đỉnh được sơn chỉ phụ thuộc vào cách bước đó tương tác với`n`, không phải trên kích thước số của`k`. 

Ví dụ, nếu`n = 9`, đang chọn`k = 8`có vẻ lớn, nhưng các đỉnh được thăm là:```
1, 9, 8, 7, 6, 5, 4, 3, 2
```Cả 9 đỉnh đều được sơn nên đáp án không phải là 8. Đáp án đúng là`3`, bởi vì nó truy cập:```
1, 4, 7
```và chỉ vẽ 3 đỉnh. 

Một trường hợp cạnh khác là số nguyên tố của các đỉnh. Vì`n = 7`, mọi bước khác 0 đều nguyên tố cùng nhau`7`, do đó mọi bước có thể đều đạt đến tất cả các đỉnh. Bước hợp lệ tối thiểu chỉ đơn giản là`1`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi cách có thể`k`từ`1`ĐẾN`n - 1`. Đối với mỗi bước, chúng ta có thể mô phỏng chuyển động xung quanh đa giác cho đến khi chúng ta quay lại đỉnh bắt đầu và đếm xem đã đạt được bao nhiêu đỉnh khác nhau. Điều này đúng vì nó diễn ra chính xác theo quy trình sơn. 

Vấn đề là một mô phỏng có thể mất tới`n`di chuyển. Đang thử tất cả`n - 1`các giá trị có thể mang lại`n^2`hoạt động. Với`n`đạt`10^10`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là các đỉnh được thăm tạo thành một chu trình. Sau đó`t`di chuyển, đỉnh hiện tại là:```
1 + t * k (mod n)
```Hai vị trí trong dãy này giống nhau khi hiệu của chúng là bội số của`n`. Số vị trí duy nhất trong chu kỳ này chính xác là:```
n / gcd(n, k)
```Vì vậy thay vì mô phỏng đa giác, chúng ta chỉ cần tối đa hóa`gcd(n, k)`. Số lượng sơn nhỏ nhất khi gcd này càng lớn càng tốt. 

Gcd lớn nhất có thể giữa`n`và tích cực thích hợp`k`là ước số thực sự lớn nhất của`n`. Nếu như`n`là hợp số và thừa số nguyên tố nhỏ nhất của nó là`p`, số chia này là`n / p`. Nhỏ nhất`k`với gcd đó cũng vậy`n / p`. 

Nếu như`n`là số nguyên tố nên không có ước số nào lớn hơn`1`, do đó mỗi bước sẽ vẽ tất cả các đỉnh. Bước nhỏ nhất có thể là`1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(√n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và xác định xem nó là số nguyên tố hay tìm thừa số nguyên tố nhỏ nhất của nó. 
2. Nếu`n`không có ước số nào nhỏ hơn chính nó thì là số nguyên tố. Mỗi bước có thể sẽ truy cập vào mọi đỉnh, vì vậy hãy quay lại`1`. 
3. Nếu không, hãy để`p`là thừa số nguyên tố nhỏ nhất của`n`. Trở lại`n / p`, bởi vì đây là ước số thực sự lớn nhất và cho gcd lớn nhất có thể với`n`. 

Tại sao nó hoạt động: chuyển động luôn tạo ra một chu kỳ có độ dài được điều khiển bởi`gcd(n, k)`. Gcd lớn hơn có nghĩa là có ít đỉnh hơn trong chu trình. gcd lớn nhất là ước số lớn nhất của`n`cái đó nhỏ hơn`n`, và bước nhỏ nhất tạo ra nó chính xác là số chia đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    if n % 2 == 0:
        print(n // 2)
        return

    d = 3
    while d * d <= n:
        if n % d == 0:
            print(n // d)
            return
        d += 2

    print(1)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xử lý các số chẵn vì`2`là thừa số nguyên tố nhỏ nhất có thể. Thậm chí`n`, bước tốt nhất là`n / 2`. 

Đối với số lẻ, chúng ta tìm ước số lẻ nhỏ nhất bắt đầu từ`3`. Chúng ta chỉ cần kiểm tra căn bậc hai của`n`, bởi vì nếu`n`có hệ số lớn hơn căn bậc hai của nó thì hệ số phù hợp sẽ nhỏ hơn. 

Ước số đầu tiên được tìm thấy là thừa số nguyên tố nhỏ nhất. Trở về`n / divisor`đưa ra ước số thích hợp lớn nhất, đây là bước bắt buộc. Nếu không tìm được số chia,`n`là số nguyên tố và câu trả lời là`1`. 

Không cần mảng hoặc tập hợp đã truy cập vì toàn bộ cấu trúc đa giác được biểu thị bằng mối quan hệ gcd. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
9
```Tìm kiếm số chia tìm thấy`3`là thừa số nguyên tố nhỏ nhất. 

| Bước | n | Số chia hiện tại | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 2 | Không chia được | - | 
| 2 | 9 | 3 | Đã tìm thấy số chia | 9/3 = 3 | 

Đầu ra:```
3
```bước`3`tạo ra chu kỳ`1, 4, 7`, vậy chỉ có ba đỉnh được sơn. 

### Mẫu 2 

đầu vào:```
7
```Không có ước số nào được tìm thấy cho đến`sqrt(7)`. 

| Bước | n | Số chia hiện tại | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 2, 3 | Không có số chia | 1 | 

Đầu ra:```
1
```Bởi vì`7`là số nguyên tố, mọi bước đều đạt đến cả bảy đỉnh và`1`là lựa chọn hợp lệ nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) | Chúng tôi chỉ kiểm tra các yếu tố có thể có đến căn bậc hai của`n`. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một vài biến số nguyên. | 

Vì`n ≤ 10^10`, kiểm tra đến`100000`các ước số có thể dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve_case(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())

    if n % 2 == 0:
        return str(n // 2) + "\n"

    d = 3
    while d * d <= n:
        if n % d == 0:
            return str(n // d) + "\n"
        d += 2

    return "1\n"

# samples
assert solve_case("9\n") == "3\n", "sample 1"
assert solve_case("7\n") == "1\n", "sample 2"

# minimum size
assert solve_case("3\n") == "1\n", "prime triangle"

# composite with small factor
assert solve_case("12\n") == "6\n", "smallest factor is 2"

# prime near upper range
assert solve_case("9999999967\n") == "1\n", "large prime"

# odd composite
assert solve_case("25\n") == "5\n", "square number"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`|`1`| Trường hợp nguyên tố tối thiểu | 
|`12`|`6`| Số chẵn và ước số thực lớn nhất | 
|`9999999967`|`1`| Xử lý số nguyên tố lớn | 
|`25`|`5`| Tổng hợp số lẻ | 

## Vỏ cạnh 

cho`n = 9`, một giải pháp bất cẩn có thể chọn bước lớn nhất có thể và kết quả`8`. Thay vào đó, thuật toán sẽ tìm hệ số nhỏ nhất`3`và trả về`9 / 3 = 3`. gcd trở thành`3`, tạo ra chu kỳ ngắn nhất có thể. 

Vì`n = 7`, mọi bước khác 0 đều có gcd`1`với`7`. Số đỉnh được sơn luôn là`7`, do đó, việc giảm thiểu bước đó sẽ đưa ra câu trả lời`1`. Thuật toán đi đến cuối tìm kiếm số chia và xử lý chính xác trường hợp nguyên tố này. 

Đối với một số tổng hợp chẵn như`n = 12`, chuyển động nên đi qua càng ít đỉnh càng tốt. Thừa số nguyên tố nhỏ nhất là`2`, vậy gcd hữu ích lớn nhất là`6`. Lựa chọn`k = 6`thăm đỉnh`1`Và`7`chỉ, đó là mức tối thiểu.
