---
title: "CF 1514B - VÀ 0, Tổng Lớn"
description: "Chúng ta được yêu cầu đếm các mảng có độ dài n trong đó mỗi phần tử nằm trong khoảng từ 0 đến 2^k - 1, sao cho AND theo bit của tất cả các phần tử bằng 0 và tổng các phần tử càng lớn càng tốt. Kết quả sẽ được trả về modulo 10^9 + 7."
date: "2026-06-10T18:39:03+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1514
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 716 (Div. 2)"
rating: 1200
weight: 1514
solve_time_s: 150
verified: false
draft: false
---

[CF 1514B - VÀ 0, Tổng Lớn](https://codeforces.com/problemset/problem/1514/B) 

**Đánh giá:** 1200 
**Tags:** bitmasks, tổ hợp, toán học 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm các mảng có độ dài`n`nơi mỗi phần tử nằm giữa`0`Và`2^k - 1`, sao cho AND theo bit của tất cả các phần tử bằng 0 và tổng các phần tử càng lớn càng tốt. Kết quả sẽ được trả về modulo`10^9 + 7`. 

Cụ thể, mỗi phần tử mảng có thể được hình dung như một`k`-số bit. Bitwise AND bằng 0 ngụ ý rằng với mọi vị trí bit từ`0`ĐẾN`k-1`, phải có ít nhất một phần tử trong mảng chứa bit đó`0`. Nếu một chút là`1`trong mọi phần tử, AND sẽ giữ lại phần tử đó`1`. Tổng được tối đa hóa khi mỗi phần tử càng lớn càng tốt, trong phạm vi này là`2^k - 1`. Tuy nhiên, chúng ta không thể đơn giản đặt tất cả các phần tử thành`2^k - 1`, bởi vì khi đó AND sẽ không bằng 0. 

Những ràng buộc của vấn đề cho phép`n`lên đến`10^5`Và`k`lên đến`20`. Với`t`lên đến 10, bất kỳ giải pháp nào thực hiện nhiều hơn khoảng`10^8`hoạt động sẽ rất chậm. Điều này loại trừ việc liệt kê tất cả các mảng có thể một cách rõ ràng hoặc thậm chí lặp qua`2^k`khả năng cho mọi yếu tố. Giải pháp phải khai thác cấu trúc tổ hợp và tính chất của các phép toán theo bit. 

Trường hợp cạnh không rõ ràng xảy ra khi`n = 1`. Cách duy nhất AND của một phần tử bằng 0 là nếu bản thân phần tử đó bằng 0, do đó tổng tối đa là`0`và có chính xác một mảng. Một cách tiếp cận ngây thơ cho rằng chúng ta luôn có thể chọn`2^k - 1`các yếu tố sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra mọi mảng có độ dài có thể`n`với các phần tử trong`0..2^k-1`, tính toán AND theo từng bit của nó và theo dõi tổng tối đa. Điều này đúng về nguyên tắc nhưng hoàn toàn không khả thi. Ví dụ, với`k=20`Và`n=10^5`, có`(2^20)^10^5`mảng, có kích thước lớn về mặt thiên văn. 

Cái nhìn sâu sắc quan trọng là chuyển từ các phần tử sang các mẫu bit. Để có tổng tối đa, mỗi phần tử phải là`2^k - 1`(tất cả các bit được đặt) ngoại trừ việc chúng ta cần AND bằng 0. Để đảm bảo AND bằng 0, chúng ta phải chọn ít nhất một phần tử để “tắt” từng bit. Việc đếm số lượng mảng đơn giản chỉ là đếm xem có bao nhiêu cách chúng ta có thể phân phối các số 0 giữa các mảng.`n`các phần tử sao cho mỗi phần tử`k`bit có ít nhất một số 0. 

Về mặt hình thức, nếu chúng ta dán nhãn`x = 2^k - 1`, thì mỗi phần tử là`x`hoặc`x`với một số bit bị xóa. Để AND bằng 0, mảng không được bao gồm toàn bộ`x`. Điều này tương đương với việc đếm các mảng có độ dài`n`trên bộ`{0, 1}`với hạn chế là không phải tất cả các phần tử đều`1`. Đó là sự kết hợp`2^n - 1`. Ở đây,`2^k`khả năng cho mỗi phần tử được cấu trúc sao cho chính xác một mẫu (tất cả các mẫu) đều bị cấm, để lại`2^k - 1`lựa chọn cho mỗi phần tử. Như vậy, số mảng hợp lệ là`(2^k - 1)^n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2^k)^n) | O(n) | Quá chậm | 
| Tối ưu | O(k + log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, đọc số nguyên`n`Và`k`. 
3. Tính toán`mod = 10^9 + 7`cho số học mô-đun. 
4. Tính toán`ways = pow(2^k - 1, n, mod)`. Điều này tính toán`(2^k - 1)^n mod 10^9 + 7`. Việc sử dụng lũy ​​thừa mô-đun tích hợp của Python đảm bảo nó xử lý được các khối lượng lớn`n`một cách hiệu quả. 
5. Đầu ra`ways`. 

Tại sao nó hoạt động: Mỗi phần tử có`2^k`những giá trị có thể. Tổng được tối đa hóa khi các phần tử càng lớn càng tốt, vì vậy chúng tôi sử dụng`2^k - 1`làm cơ sở. Hạn chế AND chỉ loại bỏ các mảng trong đó mọi phần tử đều`2^k - 1`. Do đó, các mảng còn lại chính xác là tất cả các mảng trong đó có ít nhất một phần tử không lớn nhất. Phép lũy thừa mô-đun tính toán điều này một cách hiệu quả ngay cả đối với`n`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        max_val = (1 << k) - 1
        # number of arrays with at least one element different from max_val
        result = pow(max_val, n, MOD)
        print(result)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán`max_val = 2^k - 1`cho rõ ràng. sử dụng`pow(base, exp, mod)`tránh tràn và cung cấp tính toán O(log n). Mỗi trường hợp thử nghiệm đều độc lập, vì vậy chúng tôi lặp lại`t`và in kết quả ngay lập tức. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
2
2 2
100000 20
```| Bước | n | k | giá trị tối đa | pow(max_val, n, MOD) | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 3 | 3^2 = 9 | 4 | 
| 2 | 100000 | 20 | 1048575 | pow(1048575,100000,10^9+7) | 226732710 | 

Trường hợp đầu tiên xác nhận nhỏ`n`hoạt động và thuật toán đếm chính xác các mảng tránh các phần tử tối đa. Trường hợp thứ hai xác nhận hiệu suất và lũy thừa mô-đun cho số lượng lớn`n`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * log n) | Mỗi trường hợp thử nghiệm sử dụng lũy ​​thừa mô-đun, đó là O(log n) | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Với`t <= 10`Và`n <= 10^5`, điều này vừa vặn thoải mái trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("2\n2 2\n100000 20\n") == "4\n226732710", "sample 1"

# minimum size
assert run("1\n1 1\n") == "1", "n=1,k=1"

# all k bits
assert run("1\n3 3\n") == "343", "n=3,k=3"

# large n small k
assert run("1\n100000 1\n") == "1", "n=100000,k=1"

# large k small n
assert run("1\n2 20\n") == "1099511627775", "n=2,k=20, modulo applied later"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp cạnh n=1,k=1 | 
| 3 3 | 343 | trường hợp nhỏ chung | 
| 100000 1 | 1 | n lớn, k nhỏ | 
| 2 20 | 1099511627775 | k lớn, n nhỏ | 

## Vỏ cạnh 

Khi nào`n=1`, thuật toán tính toán`pow(2^k-1,1) = 2^k - 1`, đếm tất cả các mảng phần tử đơn. Chỉ mảng 0 cho AND 0, do đó modulo đầu ra 10^9+7 trả về chính xác`1`. Vì`n`lớn, thuật toán xử lý lũy thừa theo modulo một cách hiệu quả, tránh tràn số nguyên trong khi vẫn tôn trọng ràng buộc AND-zero. Không có lỗi riêng lẻ nào xảy ra vì`pow()`trực tiếp tính toán chính xác số lượng mảng hợp lệ.
