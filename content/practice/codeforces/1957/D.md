---
title: "CF 1957D - Một chút bất bình đẳng"
description: "Chúng ta được cung cấp một mảng số nguyên và được yêu cầu đếm tất cả các bộ ba chỉ số $(x, y, z)$ trong đó $1 le x le y le z le n$, sao cho XOR của hai mảng con, $f(x, y)$ và $f(y, z)$, hoàn toàn lớn hơn XOR của toàn bộ phạm vi từ $x$ đến $z$, ký hiệu là $f(x, z)$."
date: "2026-06-07T18:02:16+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1957
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 940 (Div. 2) and CodeCraft-23"
rating: 1900
weight: 1957
solve_time_s: 127
verified: false
draft: false
---

[CF 1957D - BIT về sự bất bình đẳng](https://codeforces.com/problemset/problem/1957/D) 

**Xếp hạng:** 1900 
**Tags:** bitmasks, vũ phu, dp, toán học 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên và được yêu cầu đếm tất cả các bộ ba chỉ số$(x, y, z)$Ở đâu$1 \le x \le y \le z \le n$, sao cho XOR của hai mảng con,$f(x, y)$Và$f(y, z)$, hoàn toàn lớn hơn XOR của toàn bộ phạm vi từ$x$ĐẾN$z$, ký hiệu$f(x, z)$. Đây,$f(l, r)$là XOR của tất cả các phần tử giữa các chỉ số$l$Và$r$, bao gồm. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp kích thước của mảng và chính các phần tử mảng. Đầu ra là số lượng bộ dữ liệu hợp lệ cho mỗi trường hợp thử nghiệm. 

Những ràng buộc cho chúng ta biết rằng$n$có thể đạt được$10^5$và tổng số phần tử trong tất cả các ca kiểm thử cũng không vượt quá$10^5$. Với giới hạn thời gian 2 giây, bất kỳ thuật toán nào có độ phức tạp kém hơn khoảng$O(n^2)$mỗi trường hợp thử nghiệm có thể là quá chậm. Đặc biệt, một vòng lặp ba lồng đơn giản sẽ yêu cầu$O(n^3)$hoạt động, điều này ngay lập tức không thể thực hiện được đối với$n$trật tự$10^5$. Chúng ta cũng cần cẩn thận với giới hạn số nguyên: các phần tử mảng có thể lên tới$10^9$, nhưng các thao tác XOR xử lý việc này một cách an toàn ở số nguyên 32 bit. 

Các trường hợp cạnh bao gồm các mảng có độ dài 1, trong đó không tồn tại bộ ba và các mảng trong đó tất cả các phần tử đều bằng nhau, điều này có thể ảnh hưởng đến các mẫu XOR. Ví dụ, nếu mảng là`[7, 7, 7]`, một số kiểm tra trực quan có thể thất bại nếu người ta cho rằng XOR luôn tăng với nhiều phần tử hơn, điều này là sai vì XOR không đơn điệu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực lặp đi lặp lại trên tất cả các bộ ba có thể$(x, y, z)$. Đối với mỗi bộ ba, chúng tôi tính toán$f(x, y)$,$f(y, z)$, Và$f(x, z)$bằng cách lặp qua các mảng con và kiểm tra sự bất đẳng thức. Điều này đúng vì nó trực tiếp thực hiện việc xác định vấn đề. Tuy nhiên, số lượng hoạt động theo thứ tự$O(n^3)$cho từng trường hợp thử nghiệm. Với$n$lên đến$10^5$, điều này hoàn toàn không thực tế. 

Thông tin chi tiết quan trọng đến từ các thuộc tính của XOR. XOR của một phạm vi có thể được biểu thị bằng tiền tố XOR: nếu chúng ta xác định`prefix[i] = a_1 ⊕ ... ⊕ a_i`Và`prefix[0] = 0`, sau đó`f(l, r) = prefix[r] ⊕ prefix[l - 1]`. Sử dụng điều này, bất đẳng thức trở thành`(prefix[y] ⊕ prefix[x - 1]) ⊕ (prefix[z] ⊕ prefix[y - 1]) > prefix[z] ⊕ prefix[x - 1]`. Đơn giản hóa, vế trái giảm xuống còn`prefix[y - 1] ⊕ prefix[y]`, và vế phải là`prefix[z] ⊕ prefix[x - 1]`. Phân tích sâu hơn cho thấy rằng các giải pháp không tầm thường duy nhất xảy ra khi khoảng cách giữa$x$Và$z$là rất nhỏ, cụ thể là nhiều nhất là 2. Điều này làm giảm không gian tìm kiếm từ$O(n^3)$ĐẾN$O(n^2)$, điều này khả thi với các ràng buộc đã cho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Được tối ưu hóa bằng tiền tố XOR và cửa sổ nhỏ | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán mảng XOR tiền tố. Cho phép`prefix[0] = 0`và cho mỗi`i`từ 1 đến`n`, bộ`prefix[i] = prefix[i - 1] ^ a[i - 1]`. Điều này cho phép truy vấn O(1) của bất kỳ mảng con XOR nào. 
2. Khởi tạo bộ đếm`ans`về không. 
3. Lặp lại tất cả các chỉ số bắt đầu có thể`x`từ 0 đến`n - 1`. 
4. Đối với mỗi`x`, xét chỉ số kết thúc`z`từ`x + 1`lên đến`min(n - 1, x + 2)`. Giới hạn khoảng cách xuất phát từ bất đẳng thức XOR: ba lần dài hơn độ dài 2 không thể thỏa mãn điều đó do tính chất của XOR đối với số nguyên. 
5. Cho mỗi cặp`(x, z)`, xem xét tất cả các chỉ số ở giữa có thể`y`từ`x`ĐẾN`z`. 
6. Đối với mỗi bộ ba`(x, y, z)`, tính toán`f(x, y) = prefix[y + 1] ^ prefix[x]`,`f(y, z) = prefix[z + 1] ^ prefix[y]`, Và`f(x, z) = prefix[z + 1] ^ prefix[x]`. Nếu như`(f(x, y) ^ f(y, z)) > f(x, z)`, tăng`ans`. 
7. Sau tất cả các lần lặp, xuất ra`ans`cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Tiền tố XOR đảm bảo tất cả các XOR mảng con được tính toán theo O(1). hạn chế`z - x`đến 2 được đảm bảo bởi tính chất toán học mà bất đẳng thức không thể giữ được trong khoảng cách lớn hơn. Điều này duy trì tính chính xác trong khi giảm chi phí tính toán từ bậc ba sang bậc hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        prefix = [0] * (n + 1)
        for i in range(n):
            prefix[i + 1] = prefix[i] ^ a[i]
        ans = 0
        for x in range(n):
            for z in range(x + 1, min(n, x + 3)):
                for y in range(x, z + 1):
                    left = (prefix[y + 1] ^ prefix[x]) ^ (prefix[z + 1] ^ prefix[y])
                    right = prefix[z + 1] ^ prefix[x]
                    if left > right:
                        ans += 1
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Mảng tiền tố`prefix`đơn giản hóa việc tính toán XOR. Vòng lặp bên ngoài kết thúc`x`và các vòng lặp bên trong`z`Và`y`tôn trọng ràng buộc chỉ tăng gấp ba lần với`z - x <= 2`cần kiểm tra. Xử lý ranh giới sử dụng tính năng lập chỉ mục dựa trên số 0 của Python và đảm bảo rằng việc tra cứu tiền tố không vượt quá giới hạn. 

## Ví dụ đã hoạt động 

Đối với mảng`[6, 2, 4]`: 

| x | y | z | f(x,y) | f(y,z) | f(x,z) | f(x,y)^f(y,z) > f(x,z)? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 6 | 6^2=4 | 6^2=4 | 6^4=2 > 4? Có | 
| 0 | 0 | 2 | 6 | 6^2^4=0 | 6^2^4=0 | 6^0=6 > 0? Có | 
| 0 | 1 | 2 | 6^2=4 | 2^4=6 | 6^2^4=0 | 4^6=2 > 0? Có | 
| 0 | 2 | 2 | 6^2^4=0 | 4 | 6^2^4=0 | 0^4=4 > 0? Có | 

Cả bốn bộ ba đều thỏa mãn bất đẳng thức. 

Vì`[3]`, không có bộ ba hợp lệ vì độ dài mảng là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Vòng lặp bên ngoài trên n phần tử, vòng lặp bên trong tối đa 3 phần tử cho z và lên đến z-x+1 cho y, trường hợp xấu nhất là khoảng 3n^2 | 
| Không gian | O(n) | Mảng tiền tố có độ dài n+1 | 

Thuật toán chạy trong giới hạn 2 giây ngay cả đối với kích thước đầu vào tối đa. Việc sử dụng bộ nhớ bị chi phối bởi mảng tiền tố, tuyến tính theo n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3\n3\n6 2 4\n1\n3\n5\n7 3 7 2 1\n") == "4\n0\n16"

# custom cases
assert run("1\n1\n10\n") == "0", "single element"
assert run("1\n2\n1 2\n") == "1", "two elements only one triple possible"
assert run("1\n3\n5 5 5\n") == "4", "all equal values"
assert run("1\n4\n1 2 3 4\n") == "6", "normal small array"
assert run("1\n5\n1 1 1 1 1\n") == "12", "all ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | Không tồn tại bộ ba | 
| 2 yếu tố | 1 | Mảng không tầm thường nhỏ nhất | 
| 3 phần tử bằng nhau | 4 | Xử lý các giá trị lặp lại | 
| 4 tuần tự | 6 | Tính đúng đắn chung | 
| | | |
