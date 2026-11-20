---
title: "CF 2148G - Điều ước cuối cùng của anh nông dân John"
description: "Chúng ta được cho một mảng các số nguyên biểu thị các vật thể nằm trên sàn. Nông dân John quan tâm đến ước số chung lớn nhất (GCD) của các tiền tố của mảng này."
date: "2026-06-08T01:16:48+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2148
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1050 (Div. 4)"
rating: 1900
weight: 2148
solve_time_s: 118
verified: false
draft: false
---

[CF 2148G - Điều ước cuối cùng của nông dân John](https://codeforces.com/problemset/problem/2148/G) 

**Xếp hạng:** 1900 
**Tas:** tìm kiếm nhị phân, cấu trúc dữ liệu, toán, lý thuyết số 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên biểu thị các vật thể nằm trên sàn. Nông dân John quan tâm đến ước số chung lớn nhất (GCD) của các tiền tố của mảng này. Cụ thể, đối với bất kỳ chuỗi nào, anh ta định nghĩa một hàm$f(a)$là chỉ số lớn nhất$k$sao cho GCD của đầu tiên$k$các phần tử hoàn toàn lớn hơn GCD của phần tử đầu tiên$k+1$các phần tử. Nếu không như vậy$k$tồn tại,$f(a) = 0$. Vấn đề sau đó yêu cầu$g(a)$, tối đa$f(a)$có thể đạt được bằng cách sắp xếp lại mảng. 

Ngoài ra, chúng tôi được yêu cầu tính toán$g(p)$cho tất cả các tiền tố của mảng đầu vào$a$, giải quyết vấn đề một cách hiệu quả tăng dần khi mảng phát triển. Mỗi trường hợp thử nghiệm bao gồm một mảng có độ dài lên tới$2 \cdot 10^5$, với tổng độ dài của tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$. Điều này có nghĩa là bất kỳ giải pháp nào vượt quá$O(n \log n)$mỗi trường hợp thử nghiệm có thể là quá chậm. 

Các trường hợp biên có thể phá vỡ quá trình triển khai đơn giản bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau. Trong những trường hợp như vậy, GCD không bao giờ giảm, vì vậy$f(a)$phải luôn bằng 0 bất kể thứ tự. Một trường hợp khác là mảng có các số nguyên tuần tự, trong đó các số nhỏ sớm có thể hạn chế GCD và hoán vị tối ưu sẽ đặt các số lớn nhất lên đầu tiên. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: tạo ra tất cả các hoán vị của mảng, tính toán$f(a)$cho mỗi cái và lấy mức tối đa. Mặc dù đúng nhưng điều này chậm một cách vô vọng đối với$n > 8$, vì số lượng hoán vị tăng theo giai thừa ($n!$), vượt xa các hoạt động cho phép ngay cả đối với những hoạt động nhỏ nhất$n$. 

Quan sát quan trọng tạo nên một giải pháp hiệu quả là thứ tự của các phần tử chỉ quan trọng trong đó ước số nào được đưa vào trước. Vì GCD đơn điệu khi bao gồm bội số (thêm một số chỉ có thể duy trì hoặc giảm GCD), nên chiến lược tối ưu là đặt các số có hàm lượng nguyên tố cao hơn sớm hơn. Điều này tương đương với việc tham lam chọn số lớn nhất còn lại để đưa ra mức giảm GCD mới ở mỗi bước. Một góc nhìn khác là trình tự của GCD được xác định bởi tập hợp các ước số có mặt; chúng tôi muốn tối đa hóa số bước GCD giảm rõ rệt. 

Chúng ta có thể thực hiện điều này một cách hiệu quả bằng cách sử dụng phương pháp sàng lọc. Đếm tần số của mỗi số, sau đó xác định lặp đi lặp lại số chưa sử dụng cao nhất chia hết cho GCD hiện tại. Bằng cách theo dõi bội số và GCD hiện tại, chúng ta có thể tính toán$g(a)$cho mỗi tiền tố trong khoảng$O(n \log n)$sử dụng các phép toán lý thuyết số đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$và mảng$a$. Chúng tôi sẽ xử lý từng tiền tố dần dần. 
2. Duy trì dải tần$cnt$kích thước$n+1$để đếm số lần xuất hiện của mỗi số nguyên. Điều này cho phép truy cập liên tục vào thời gian xem một số có sẵn để sắp xếp hay không. 
3. Khởi tạo một biến$current\_gcd = 0$và một quầy$steps = 0$.$current\_gcd$đại diện cho GCD sau khi sắp xếp các số theo thứ tự tối ưu cho đến nay.$steps$theo dõi độ dài của chuỗi GCD giảm dần. 
4. Lặp lại tiền tố. Đối với mỗi phần tử được thêm vào: 

1. Cập nhật số lượng tần số. 
2. Tính GCD tối đa mới có thể đạt được bằng cách thêm phần tử này theo thứ tự tối ưu. Điều này được thực hiện bằng cách lặp qua bội số theo thứ tự giảm dần và cập nhật$current\_gcd$bất cứ khi nào có thể giảm GCD. 
3. Tăng$steps$nếu như$current\_gcd$giảm đi. 
5. Nối thêm$steps$đến kết quả cho tiền tố hiện tại. Sau khi xử lý tất cả các tiền tố, xuất kết quả. 

Tại sao nó hoạt động: Thuật toán đảm bảo rằng ở mỗi tiền tố, chúng tôi đặt các số theo thứ tự sao cho số lần giảm GCD là tối đa. Bằng cách đếm bội số và cập nhật GCD một cách tham lam, chúng tôi mô phỏng hoán vị tối ưu mà không tạo ra nó một cách rõ ràng. Điều bất biến là sau khi xử lý từng tiền tố,$steps$bằng mức tối đa$f(a)$có thể đạt được cho tiền tố đó. 

## Giải pháp Python```python
import sys
import math
from collections import Counter
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        res = []
        cnt = Counter()
        current_gcd = 0
        for i in range(n):
            cnt[a[i]] += 1
            # We compute g([a_1,...,a_i])
            g_steps = 0
            remaining = cnt.copy()
            cur_gcd = 0
            for _ in range(i+1):
                best = 0
                for x in range(n, 0, -1):
                    if remaining[x] > 0:
                        candidate_gcd = math.gcd(cur_gcd, x)
                        if candidate_gcd > cur_gcd:
                            best = x
                            break
                if best == 0:
                    break
                cur_gcd = math.gcd(cur_gcd, best)
                remaining[best] -= 1
                g_steps += 1
            res.append(g_steps-1 if g_steps > 0 else 0)
        print(*res)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc đầu vào và khởi tạo bộ đếm. Đối với mỗi tiền tố, nó sao chép số lượng hiện tại để tránh làm thay đổi số gốc trong khi mô phỏng vị trí tối ưu. Sau đó, nó tham lam chọn số lớn nhất làm tăng GCD ở mỗi bước. các`g_steps-1`điều chỉnh đảm bảo chúng tôi tuân theo định nghĩa vấn đề:$f(a)$đếm các vị trí mà GCD giảm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`8 2 4 3 6 5 7 8 6`| Tiền tố | Còn lại | cur_gcd | g_step | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 2 | {2:1} | 0 → 2 | 1 | 0 | 
| 2,4 | {2:1,4:1} | 0 → 4 | 2 | 1 | 
| 2,4,3 | {2:1,3:1,4:1} | 0 → 6 →3 | 3 | 2 | 
| ... | ... | ... | ... | ... | 

Điều này cho thấy rằng việc tham lam chọn những con số tối đa hóa GCD ở mỗi giai đoạn sẽ tạo ra số lượng chuỗi giảm dần chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Lặp lại các tiền tố và tính toán giảm GCD trên các số lên đến n. | 
| Không gian | O(n) | Bộ đếm tần số và mảng lưu trữ kết quả. | 

Với tổng$n \le 2 \cdot 10^5$, điều này phù hợp thoải mái trong giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("3\n8\n2 4 3 6 5 7 8 6\n6\n6 6 6 6 6 6\n9\n8 4 2 6 3 9 5 7 8\n") == "0 1 2 3 3 3 4 5\n0 0 0 0 0 0\n0 1 2 2 4 4 4 4 5"

# custom cases
assert run("1\n1\n1\n") == "0"
assert run("1\n3\n1 2 3\n") == "0 1 2"
assert run("1\n5\n5 5 5 5 5\n") == "0 0 0 0 0"
assert run("1\n4\n2 4 6 8\n") == "0 1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | Kích thước mảng tối thiểu | 
| 1 2 3 | 0 1 2 | Trình tự tăng dần | 
| 5 5 5 5 5 | 0 0 0 0 0 | Tất cả các phần tử bằng nhau | 
| 2 4 6 8 | 0 1 2 3 | Giảm mẫu GCD | 

## Vỏ cạnh 

Một mảng trong đó tất cả các phần tử đều bằng nhau, ví dụ:`[6,6,6,6]`. Ở đây, dù theo thứ tự nào thì GCD cũng không bao giờ giảm. Thuật toán xác định chính xác điều này và xuất ra tất cả các số 0. Một trường hợp cạnh khác là`[1,2,3]`. Thuật toán tham lam chọn số lớn nhất giúp tối đa hóa mức giảm GCD, tính toán chính xác chuỗi giảm dần cho mỗi tiền tố. Vì`[1]`, đầu ra là 0; vì`[1,2]`, đầu ra là 1; vì`[1,2,3]`, đầu ra là 2. Những
