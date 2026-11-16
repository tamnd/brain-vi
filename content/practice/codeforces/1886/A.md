---
title: "CF 1886A - Tổng của Ba"
description: "Chúng ta được cho một số nguyên $n$ và phải chia nó thành ba số có tổng chính xác là $n$. Ba số phải đồng thời thỏa mãn ba điều kiện. Tất cả chúng đều phải dương, tất cả chúng đều phải khác biệt và không có số nào trong số chúng có thể chia hết cho $3$."
date: "2026-06-08T22:16:53+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1886
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 156 (Rated for Div. 2)"
rating: 800
weight: 1886
solve_time_s: 225
verified: false
draft: false
---

[CF 1886A - Tổng của ba](https://codeforces.com/problemset/problem/1886/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán xây dựng, toán học 
**Thời gian giải:** 3 phút 45 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên$n$và phải chia nó thành ba số có tổng bằng chính xác$n$. 

Ba số phải đồng thời thỏa mãn ba điều kiện. Tất cả chúng đều phải dương, chúng phải phân biệt và không có cái nào có thể chia hết cho$3$. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi chỉ cần tìm một cách xây dựng hợp lệ. Nếu không có cấu trúc như vậy tồn tại, chúng tôi in`NO`. 

Các ràng buộc là rất nhỏ từ góc độ thuật toán. Mặc dù$n$có thể lớn như$10^9$, chỉ có tối đa$10^4$trường hợp thử nghiệm. Bất kỳ giải pháp nào thực hiện một lượng công việc không đổi cho mỗi trường hợp thử nghiệm đều có hiệu quả ngay lập tức. Việc tìm kiếm mạnh mẽ trên các bộ ba có thể sẽ là không thể vì không gian tìm kiếm tăng gần bằng$O(n^2)$hoặc tệ hơn, điều này hoàn toàn không khả thi đối với các giá trị gần$10^9$. 

Phần khó khăn không phải là hiệu quả mà là việc xây dựng. 

Trường hợp một cạnh là khi$n$là rất nhỏ. Ví dụ:```
n = 4
```Bộ ba riêng biệt dương duy nhất có tổng bằng$4$sẽ cần sử dụng những con số như$(1,1,2)$hoặc hoán vị của nó, không khác biệt. Câu trả lời đúng là`NO`. 

Một trường hợp quan trọng khác là khi số thứ ba còn lại chia hết cho$3$. 

Giả sử chúng ta luôn chọn$1$Và$2$như hai số đầu tiên. Sau đó:```
n = 9
```sẽ sản xuất```
1 + 2 + 6 = 9
```Nhưng$6$chia hết cho$3$, vì vậy cách xây dựng này không hợp lệ. Một giải pháp chỉ sử dụng$(1,2)$sẽ thất bại trên tất cả các bội số của$3$. 

Trường hợp tinh tế cuối cùng là khi số thứ ba va chạm với một trong các số đã chọn. 

Ví dụ:```
n = 6
```sử dụng$(1,2)$cho```
z = 3
```cái đó có thể chia được cho$3$. 

sử dụng$(1,4)$cho```
z = 1
```đó không phải là khác biệt. 

Câu trả lời đúng là`NO`. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử tất cả ba lần$(x,y,z)$với$$x+y+z=n,$$kiểm tra xem tất cả các số có dương, khác biệt và không chia hết cho$3$, sau đó dừng lại khi tìm thấy bộ ba hợp lệ. 

Cách tiếp cận này đúng vì nó xem xét rõ ràng mọi khả năng. Vấn đề là kích thước của không gian tìm kiếm. Thậm chí sửa chữa$z=n-x-y$, có khoảng$O(n^2)$cặp ứng cử viên$(x,y)$. Vì$n=10^9$, điều này là hoàn toàn không thể. 

Quan sát quan trọng là chúng ta không cần phải tìm kiếm. Chúng tôi chỉ cần một công trình hợp lệ. 

Sự hạn chế liên quan đến việc chia hết cho$3$đề nghị làm việc với các số nhỏ có số dư theo modulo$3$được biết đến. 

Hãy xem xét hai trường hợp. 

Nếu như$n$không chia hết cho$3$, chọn$$x=1,\qquad y=2.$$Không có số nào chia hết cho$3$, và chúng khác biệt. Số thứ ba là$$z=n-3.$$Từ$n \not\equiv 0 \pmod 3$,$$z \equiv n \pmod 3,$$Vì thế$z$cũng không chia hết cho$3$. 

Nếu như$n$chia hết cho$3$, việc xây dựng trước đó thất bại vì$z=n-3$cũng sẽ chia hết cho$3$. Chúng ta cần một cặp khác. Chọn$$x=1,\qquad y=4.$$Sau đó$$z=n-5.$$Bởi vì$n \equiv 0 \pmod 3$,$$z \equiv -5 \equiv 1 \pmod 3,$$Vì thế$z$không chia hết cho$3$. 

Nhiệm vụ duy nhất còn lại là kiểm tra xem$z$vẫn dương và khác biệt với các số đã chọn. Điều này chỉ thất bại đối với các giá trị nhỏ như$n=6$Và$n=9$, không có câu trả lời hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$. 
2. Nếu$n \le 6$, in`NO`. 

Ba số nguyên dương phân biệt bất kỳ đều có tổng nhỏ nhất có thể$$1+2+3=6.$$Vì$n<6$một giải pháp là không thể, và đối với$n=6$bộ ba khác biệt duy nhất là$(1,2,3)$trong đó chứa bội số của$3$. 
3. Nếu$n \bmod 3 \ne 0$, chọn$$x=1,\quad y=2,\quad z=n-3.$$4. Kiểm tra xem$$z>2.$$Điều này đảm bảo tính tích cực và khác biệt. 
5. In`YES`và bộ ba. 
6. Nếu$n \bmod 3 = 0$, chọn$$x=1,\quad y=4,\quad z=n-5.$$7. Nếu$$z\le4,$$in`NO`. 

Bội số có vấn đề duy nhất của$3$lớn hơn$6$là$n=9$, mang lại$z=4$và vi phạm tính khác biệt. 
8. Nếu không thì in`YES`và bộ ba. 

### Tại sao nó hoạt động 

cho$n \not\equiv 0 \pmod 3$, việc xây dựng$(1,2,n-3)$sử dụng các số có modulo khác 0$3$. Từ$n-3 \equiv n \pmod 3$, số thứ ba cũng không chia hết cho$3$. 

Vì$n \equiv 0 \pmod 3$, việc xây dựng$(1,4,n-5)$hoạt động vì$n-5 \equiv 1 \pmod 3$. Một lần nữa không có số nào chia hết cho$3$. 

Lỗi duy nhất xảy ra khi số thứ ba trở thành không dương hoặc bằng một trong các giá trị đã chọn. Những trường hợp đó chính xác là những giá trị nhỏ đã bị từ chối. Mọi trường hợp hợp lệ lớn hơn đều nhận được một cấu trúc chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())

    if n <= 6:
        print("NO")
        continue

    if n % 3 != 0:
        print("YES")
        print(1, 2, n - 3)
    else:
        z = n - 5

        if z == 1 or z == 4:
            print("NO")
        else:
            print("YES")
            print(1, 4, z)
```Nhánh đầu tiên xử lý các số không chia hết cho ba. Cặp đôi$(1,2)$được cố định và giá trị còn lại tự động tránh chia hết cho ba. 

Nhánh thứ hai xử lý bội số của ba. Cặp đôi$(1,4)$tránh mô-đun$3$vấn đề phá vỡ công trình đầu tiên. 

Thất bại đặc biệt duy nhất là$n=9$, trong đó giá trị thứ ba trở thành$4$, làm cho các số không phân biệt được. điều kiện`z == 1 or z == 4`nắm bắt mọi trường hợp có vấn đề. Trong thực tế, sau lần trước`n <= 6`kiểm tra, chỉ`z == 4`có thể xảy ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 10
```| Bước | Giá trị | 
| --- | --- | 
| n | 10 | 
| n % 3 | 1 | 
| x | 1 | 
| y | 2 | 
| z | 7 | 

Đầu ra:```
YES
1 2 7
```Số thứ ba dương, khác với hai số đầu và không chia hết cho$3$. 

### Ví dụ 2 

đầu vào:```
n = 15
```| Bước | Giá trị | 
| --- | --- | 
| n | 15 | 
| n % 3 | 0 | 
| x | 1 | 
| y | 4 | 
| z | 10 | 

Đầu ra:```
YES
1 4 10
```Cả ba số đều khác nhau, dương và có số dư$1$,$1$, Và$1$modulo$3$. 

### Ví dụ 3 

đầu vào:```
n = 9
```| Bước | Giá trị | 
| --- | --- | 
| n | 9 | 
| n % 3 | 0 | 
| x | 1 | 
| y | 4 | 
| z | 4 | 

Từ$z=4$, các số sẽ không phân biệt được. 

Đầu ra:```
NO
```Điều này chứng tỏ trường hợp bất khả thi không tầm thường duy nhất lớn hơn$6$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Chỉ một vài phép tính số học và so sánh | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu phụ trợ | 

Ngay cả với$10^4$trường hợp thử nghiệm, chương trình chỉ thực hiện công việc liên tục cho từng trường hợp. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        if n <= 6:
            out.append("NO")
            continue

        if n % 3 != 0:
            out.append("YES")
            out.append(f"1 2 {n - 3}")
        else:
            z = n - 5
            if z == 1 or z == 4:
                out.append("NO")
            else:
                out.append("YES")
                out.append(f"1 4 {z}")

    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("4\n10\n4\n15\n9\n") == (
    "YES\n1 2 7\n"
    "NO\n"
    "YES\n1 4 10\n"
    "NO"
)

# minimum value
assert run("1\n1\n") == "NO"

# smallest valid non-multiple of 3
assert run("1\n7\n") == "YES\n1 2 4"

# smallest valid multiple of 3 after failure at 9
assert run("1\n12\n") == "YES\n1 4 7"

# maximum input
assert run("1\n1000000000\n") == "YES\n1 2 999999997"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`NO`| Đầu vào tối thiểu có thể | 
|`7`|`YES 1 2 4`| Công trình hợp lệ nhỏ nhất | 
|`12`|`YES 1 4 7`| Bội số 3 sử dụng mẫu thứ hai | 
|`1000000000`| Hợp lệ gấp ba | Giá trị được phép lớn nhất | 

## Vỏ cạnh 

Hãy xem xét:```
n = 6
```Thuật toán ngay lập tức từ chối nó vì`n <= 6`. Tổng ba lần dương khác biệt duy nhất để$6$là$(1,2,3)$, Và$3$chia hết cho$3$. Câu trả lời đúng là`NO`. 

Coi như:```
n = 9
```Thuật toán đi vào nhánh bội số của ba.$$z = 9 - 5 = 4.$$Bộ ba sẽ là$(1,4,4)$, không khác biệt. Thuật toán in`NO`, điều đó đúng. 

Coi như:```
n = 8
```Thuật toán sử dụng cách xây dựng đầu tiên.$$(1,2,5).$$Tất cả các số đều dương, khác biệt và không chia hết cho$3$. Thuật toán in ra một câu trả lời hợp lệ. 

Coi như:```
n = 999999999.
```Từ$n$chia hết cho$3$, thuật toán xây dựng$$(1,4,999999994).$$Giá trị thứ ba là dương, khác biệt và phù hợp với$1 \pmod 3$. Tất cả các yêu cầu đều được đáp ứng, chứng tỏ rằng công trình xây dựng hoạt động ngay cả ở quy mô lớn nhất.
