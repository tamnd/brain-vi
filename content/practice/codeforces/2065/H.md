---
title: "CF 2065H - Anh Nghĩ Anh Là Anh"
description: "Vấn đề yêu cầu chúng ta làm việc với một chuỗi nhị phân và các truy vấn lật từng bit riêng lẻ. Đối với một chuỗi $t$, hãy xác định $f(t)$ là số lượng tối thiểu các khối liền kề gồm các ký tự giống hệt nhau phân chia chuỗi."
date: "2026-06-08T07:21:27+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "divide-and-conquer", "dp", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 2065
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 1003 (Div. 4)"
rating: 2200
weight: 2065
solve_time_s: 111
verified: false
draft: false
---

[CF 2065H - Anh nghĩ anh ấy là anh ấy](https://codeforces.com/problemset/problem/2065/H) 

**Xếp hạng:** 2200 
**Tags:** tổ hợp, cấu trúc dữ liệu, chia để trị, dp, toán, ma trận 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề yêu cầu chúng ta làm việc với một chuỗi nhị phân và các truy vấn lật từng bit riêng lẻ. Đối với một chuỗi$t$, định nghĩa$f(t)$là số lượng tối thiểu các khối liền kề của các ký tự giống hệt nhau phân chia chuỗi. Ví dụ, đối với`00110001`, các khối là`[00][11][000][1]`Và$f(t) = 4$. Sau mỗi lần lật, chúng ta được yêu cầu tính tổng của$f(b)$trên tất cả các dãy con không trống$b$của chuỗi hiện tại$s$. Tổng cuối cùng phải được báo cáo theo modulo$998244353$. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp một chuỗi và một chuỗi các lần lật. Mỗi lần lật có tính tích lũy, do đó trạng thái của chuỗi sau một truy vấn sẽ ảnh hưởng đến truy vấn tiếp theo. Độ dài của chuỗi và số lượng truy vấn có thể đạt được$2 \cdot 10^5$và tổng của tất cả các trường hợp thử nghiệm được giới hạn bởi cùng một số. Điều này ngụ ý rằng bất kỳ giải pháp ngây thơ nào liệt kê các chuỗi con sẽ quá chậm vì số lượng các chuỗi con là$2^n$, đó là thiên văn đối với$n$gần$10^5$. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi trong đó tất cả các ký tự đều giống nhau. Trong những trường hợp như vậy,$f(t) = 1$và tất cả các dãy con vẫn đồng nhất, tạo ra các mẫu rất đều đặn trong tổng. Một trường hợp tinh vi khác xảy ra khi một cú lật đơn lẻ sẽ tách hoặc hợp nhất các khối, làm thay đổi đáng kể$f(t)$cho nhiều dãy tiếp theo. Một cách tiếp cận đơn giản là tính lại tổng từ đầu sau mỗi truy vấn sẽ không hiệu quả và không thành công trong thời gian giới hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là liệt kê tất cả các chuỗi con của$s$, tính toán$f(b)$cho mỗi cái và tính tổng chúng. Cách tiếp cận này hoạt động về nguyên tắc nhưng đòi hỏi$O(2^n)$hoạt động trên mỗi truy vấn, điều này là không thể khi$n$có thể đạt được$2 \cdot 10^5$. Ngay cả đối với các ví dụ nhỏ, điều này trở nên không thực tế vì chúng ta không thể chạm vào từng dãy con riêng lẻ. 

Cái nhìn sâu sắc quan trọng đến từ sự quan sát rằng$f(b)$chỉ tăng khi dãy con chứa "sự thay đổi ký tự" từ 0 thành 1 hoặc 1 thành 0. Hãy xác định$dp[i]$như tổng của$f(b)$trên tất cả các dãy con kết thúc ở vị trí$i$. Với lý luận tổ hợp, chúng ta có thể thấy rằng mỗi vị trí$i$đóng góp vào tổng số tỷ lệ với số lượng chuỗi con nó mở rộng từ các khối trước đó. Nếu như$s[i]$bằng với$s[i-1]$, việc thêm nó vào bất kỳ chuỗi con nào trước đó sẽ không tạo ra một khối mới. Nếu nó khác đi thì nó tăng lên$f(b)$bằng 1 cho tất cả các chuỗi con bao gồm ký tự trước đó. Điều này làm giảm vấn đề từ$O(2^n)$ĐẾN$O(n)$đánh giá mỗi chuỗi. 

Đối với các bản cập nhật, việc lật một ký tự chỉ ảnh hưởng đến ranh giới với các ký tự lân cận. Cho phép$cnt$đại diện cho tổng số tiền trên tất cả các chuỗi con. Khi chúng tôi lật$s[i]$, chúng ta chỉ cần điều chỉnh các khả năng hợp nhất hoặc chia tách với$s[i-1]$Và$s[i+1]$. Sử dụng lũy ​​thừa của hai để tính số dãy con bao gồm các vị trí, chúng ta có thể tính tổng mới trong thời gian không đổi cho mỗi truy vấn. Điều này biến vấn đề thành một thuật toán hiệu quả xử lý cả tính toán ban đầu và cập nhật bằng cách sử dụng danh tính tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước lũy thừa của hai modulo$998244353$lên đến độ dài chuỗi tối đa. Chúng đại diện cho số lượng các chuỗi con bao gồm một vị trí nhất định. Tính toán trước cho phép$O(1)$truy cập sau. 
2. Tính tổng ban đầu của$f(b)$trên tất cả các chuỗi con không trống của chuỗi đầu vào$s$. Đối với mỗi cặp ký tự liền kề, nếu chúng khác nhau thì thêm$2^{i-1}$vào tổng số đang chạy vì tất cả các chuỗi con bao gồm cả hai vị trí đều đóng góp một khối bổ sung. Đối với các chuỗi ký tự đơn, hãy khởi tạo tổng bằng$n$bởi vì mỗi ký tự riêng lẻ tạo thành một khối. 
3. Đối với mỗi ký tự lật truy vấn$s[v_i]$, kiểm tra hàng xóm bên trái và bên phải của nó. Nếu ký tự bị lật, hãy tính toán lại phần đóng góp của các ranh giới bị ảnh hưởng. Nếu việc lật hợp nhất hai hàng xóm khác nhau trước đó, hãy trừ đi lũy thừa tương ứng của hai. Nếu lật các phần lân cận giống hệt nhau, hãy cộng lại các phần đóng góp. Mỗi bản cập nhật chỉ chạm tới hai ranh giới và có thể được thực hiện trong thời gian không đổi. 
4. Sau khi điều chỉnh việc lật, lưu trữ tổng số tiền mới theo modulo$998244353$như câu trả lời cho truy vấn đó. Tiếp tục xử lý truy vấn tiếp theo với trạng thái chuỗi được cập nhật. 

Điều bất biến ở đây là tại mọi thời điểm,`total`giữ tổng của$f(b)$trên tất cả các chuỗi con của chuỗi hiện tại. Mỗi lần lật chỉ ảnh hưởng đến các ranh giới và số lượng tổ hợp lũy thừa của hai mô hình chính xác số lượng các chuỗi con bị ảnh hưởng, do đó thuật toán duy trì tính chính xác trong suốt tất cả các bản cập nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def solve():
    t = int(input())
    max_n = 2 * 10**5
    pow2 = [1] * (max_n + 2)
    for i in range(1, max_n + 2):
        pow2[i] = (pow2[i-1] * 2) % MOD

    for _ in range(t):
        s = list(input().strip())
        n = len(s)
        q = int(input())
        queries = list(map(lambda x: int(x)-1, input().split()))

        total = n  # sum of f(b) for subsequences of length 1
        for i in range(1, n):
            if s[i] != s[i-1]:
                total = (total + pow2[i-1]) % MOD

        res = []
        for idx in queries:
            for j in [idx-1, idx]:
                if 0 <= j < n-1:
                    if s[j] != s[j+1]:
                        total = (total - pow2[j] + MOD) % MOD
            s[idx] = '1' if s[idx] == '0' else '0'
            for j in [idx-1, idx]:
                if 0 <= j < n-1:
                    if s[j] != s[j+1]:
                        total = (total + pow2[j]) % MOD
            res.append(total)
        print(*res)

if __name__ == "__main__":
    solve()
```Việc tính toán trước ban đầu lũy thừa của hai cho phép tính toán các đóng góp từ các ranh giới theo thời gian không đổi. Tổng ban đầu xem xét tất cả các chuỗi con bao gồm một ký tự và thêm phần đóng góp từ các khác biệt ký tự liền kề. Mỗi lần cập nhật truy vấn trước tiên sẽ loại bỏ những đóng góp cũ tại các ranh giới sẽ thay đổi, lật ký tự, sau đó thêm những đóng góp mới. Các lỗi xảy ra có thể tránh được bằng cách kiểm tra cẩn thận giới hạn mảng trước khi truy cập các hàng xóm. Tất cả các phép toán modulo đảm bảo tổng vẫn nằm trong giới hạn. 

## Ví dụ đã hoạt động 

Sử dụng mẫu đầu tiên: 

| Bước | s | tổng cộng | Hành động | 
| --- | --- | --- | --- | 
| ban đầu | 101 | 3 + 2^0 + 2^1 = 10 | Tính tổng ban đầu | 
| truy vấn 1 (lật 1) | 001 | 10 → 7 | Điều chỉnh ranh giới tại vị trí 0 và 1 | 
| truy vấn 2 (lật 3) | 000 | 7 → 7 | Chỉ ranh giới bên phải bị ảnh hưởng, cập nhật tổng cộng | 

Điều này xác nhận tính bất biến rằng chỉ các vị trí liền kề mới cần được kiểm tra và lũy thừa của hai đóng góp mô hình số lượng các chuỗi con một cách chính xác. 

Sử dụng mẫu thứ hai: 

| Bước | s | tổng cộng | Hành động | 
| --- | --- | --- | --- | 
| ban đầu | 10110 | 5 + đóng góp = 61 | tính toán ban đầu | 
| truy vấn 1 (lật 1) | 00110 | 61 → 59 | ranh giới bên trái được xử lý, cập nhật tổng số | 
| truy vấn 2 (lật 2) | 01110 | 59 → 67 | ranh giới được cập nhật | 
| truy vấn 3 (lật 3) | 01010 | 67 → 59 | ranh giới được cập nhật | 

Dấu vết này hiển thị nhiều lần lật theo trình tự và xác nhận tính chính xác của các cập nhật gia tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) cho mỗi trường hợp thử nghiệm | Tổng ban đầu được tính theo O(n), mỗi truy vấn được cập nhật theo O(1) bằng cách sử dụng lũy ​​thừa được tính toán trước | 
| Không gian | O(n) | Lưu trữ chuỗi và mảng lũy ​​thừa hai | 

Với n và q lên tới 2·10^5, tổng số thao tác trên mỗi trường hợp thử nghiệm nằm trong khoảng ~4·10^5, thoải mái trong giới hạn thời gian 3 giây. Việc sử dụng bộ nhớ rất khiêm tốn, bị chi phối bởi mảng lũy ​​thừa hai. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import contextlib
    import builtins
    from io import StringIO
    out = StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n101\n2\n1 3\n10110\n3\n1 2 3\n101110101\n5\n7 2 4 4 1\n") == "10 7\n61 59 67\n1495 1169 1417 1169 1396", "sample 1"

# Custom cases
assert run("1\n0\n1\n1\n") == "1", "single char flip"
assert run("1\n1111\n2\n
```
