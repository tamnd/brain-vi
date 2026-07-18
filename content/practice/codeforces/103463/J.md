---
title: "CF 103463J - Hsueh- sở hữu số lượng lớn táo"
description: "Chúng ta đang lập mô hình một quá trình chuyển giao xác định bắt đầu bằng một đống táo và di chuyển qua m trẻ em. Đứa trẻ đầu tiên nhận được tất cả n quả táo."
date: "2026-07-03T06:58:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103463
codeforces_index: "J"
codeforces_contest_name: "The Hangzhou Normal U Qualification Trials for ZJPSC 2020"
rating: 0
weight: 103463
solve_time_s: 43
verified: true
draft: false
---

[CF 103463J - Hsueh- sở hữu số lượng lớn táo](https://codeforces.com/problemset/problem/103463/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang lập mô hình một quá trình chuyển giao xác định bắt đầu bằng một đống táo và di chuyển qua m trẻ em. Đứa trẻ đầu tiên nhận được tất cả n quả táo. Mỗi đứa trẻ thực hiện cùng một thao tác: chúng ăn chính xác một quả táo và sau đó chúng cố gắng chia những quả táo còn lại thành x đống bằng nhau. Một trong những đống này được giữ lại, trong khi những đống x − 1 còn lại được chuyển tiếp làm đầu vào của đứa trẻ tiếp theo. 

Một hạn chế chính là ở mỗi bước, số táo còn lại sau khi ăn một quả phải chia hết cho x và đống táo còn lại của đứa trẻ cuối cùng phải là số nguyên dương. Nhiệm vụ là xác định số lượng táo n ban đầu nhỏ nhất có thể để làm cho toàn bộ quá trình này hợp lệ thông qua tất cả m con và xuất ra n modulo tối thiểu đó là 998244353. 

Cấu trúc vốn có tính tuần tự: mỗi phần tử con biến đổi giá trị đầu vào thành giá trị đầu ra nhỏ hơn thông qua điều kiện số học tuyến tính bao gồm phép trừ một và chia cho x. Đầu ra của một bước sẽ trở thành đầu vào của bước tiếp theo, do đó toàn bộ hệ thống là một phép lặp phải duy trì giá trị đối với m lần chuyển đổi. 

Các ràng buộc cho phép m lên tới 10^9 và x lên tới 10^9, trong khi số lượng trường hợp thử nghiệm có thể đạt tới 10^3. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào trên tất cả trẻ em, vì ngay cả O(m) trên mỗi trường hợp thử nghiệm cũng sẽ quá lớn. Giải pháp phải nén hiệu ứng lặp lại cùng một phép biến đổi m lần thành biểu thức dạng đóng. 

Một trường hợp cạnh tinh tế phát sinh khi x = 2 hoặc m lớn: phép truy hồi tăng rất nhanh và các giá trị trung gian vượt quá giới hạn tiêu chuẩn, do đó, bất kỳ cách tiếp cận nào dựa vào phép nhân lặp mà không có số học mô-đun hoặc không có dạng đóng sẽ thất bại. Một điều tinh tế khác là tính chia hết phải được duy trì ở mọi bước, vì vậy không phải mọi dãy số học đều hợp lệ ngay cả khi biểu thức cuối cùng có vẻ đúng. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng mô phỏng trực tiếp, chúng ta bắt đầu từ một số n chưa biết và liên tục áp dụng phép biến đổi: trừ một, chia cho x và truyền tiếp. Việc chạy tiếp này là không thể vì chúng ta không biết các giá trị trung gian trừ khi chúng ta đã biết n. 

Thay vào đó, chúng tôi đảo ngược quá trình. Giả sử đứa trẻ thứ i nhận được một giá trị a nào đó. Sau khi ăn một quả táo, a − 1 còn lại phải bằng x lần giá trị được truyền cho đứa trẻ tiếp theo. Điều đó có nghĩa là a − 1 luôn chia hết cho x và trạng thái tiếp theo là (a − 1) / x. Điều này xác định sự lặp lại ngược: nếu chúng ta biết giá trị hợp lệ cuối cùng, chúng ta có thể xây dựng lại các giá trị trước đó. 

Đặt cọc còn lại của con cuối cùng là 1, vì nó phải là số nguyên dương và chúng ta muốn giá trị bắt đầu tối thiểu. Làm ngược lại m lần sẽ khai triển số theo cấu trúc hình học: mỗi bước ngược lại nhân với x và cộng 1. Lặp lại m lần này sẽ thu được một chuỗi hình học dạng đóng. 

Điều này dẫn đến biểu thức n = 1 + x + x^2 + ... + x^{m}. Đây là tổng hình học tiêu chuẩn và có thể được viết lại thành (x^{m+1} − 1) / (x − 1) cho x ≠ 1. Vì x ≥ 2 nên phép chia luôn hợp lệ trong số học mô đun vì x − 1 là modulo khả nghịch 998244353. 

Do đó, vấn đề được rút gọn thành lũy thừa nhanh và tính toán nghịch đảo mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Dòng hình học + Công suất nhanh | O(log m) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Viết lại quy trình ngược lại 

Chúng tôi xác định trạng thái sau mỗi đứa trẻ. Nếu một phần tử con có giá trị a thì trạng thái trước đó phải thỏa mãn a = x * b + 1 đối với một số nguyên b. Việc đảo ngược này là bắt buộc bởi yêu cầu sau khi trừ đi một thì số dư phải chia đều thành x cọc. 

### 2. Xác định trạng thái cuối cùng hợp lệ tối thiểu

Con cuối cùng phải kết thúc bằng một đống số nguyên dương. Để giảm thiểu giá trị ban đầu, chúng tôi đặt giá trị cuối cùng này thành 1. Bất kỳ lựa chọn nào lớn hơn sẽ làm tăng nghiêm ngặt tất cả các giá trị được xây dựng lại trước đó. 

### 3. Mở rộng về phía sau nhiều lần 

Chúng tôi áp dụng phép biến đổi nghịch đảo nhiều lần: 

a_{k-1} = x * a_k + 1 

Bắt đầu từ a_m = 1, chúng ta tạo ra: 

a_{m-1} = x + 1 

a_{m-2} = x^2 + x + 1 

và vân vân. 

Mẫu này cho thấy rằng sau k bước ngược lại, giá trị sẽ trở thành tổng lũy ​​thừa của x. 

### 4. Nhận biết cấu trúc hình học 

Sau m bước, giá trị ban đầu là: 

n = sum_{i=0 đến m} x^i 

Điều này chuyển đổi sự phụ thuộc tuần tự thành một biểu thức dạng đóng. 

### 5. Tính toán hiệu quả theo mô đun 

Chúng ta tính x^{m+1} bằng cách sử dụng modulo lũy thừa nhanh 998244353. Sau đó tính nghịch đảo module của x − 1 bằng cách sử dụng định lý Fermat vì mô đun là số nguyên tố. Cuối cùng kết hợp chúng thành: 

n = (x^{m+1} − 1) * inv(x − 1) 

### Tại sao nó hoạt động 

Phép truy toán ngược xác định duy nhất tất cả các trạng thái trước đó sau khi trạng thái cuối cùng được cố định. Bởi vì mỗi bước là affine (hàm tuyến tính cộng với hằng số), phép tổ hợp lặp lại sẽ mang lại một cấp số nhân. Mỗi cấu hình chuyển tiếp hợp lệ tương ứng với chính xác một cấu trúc lại ngược, do đó việc chọn giá trị đầu cuối tối thiểu sẽ thực thi giá trị ban đầu tối thiểu. Số học mô đun bảo toàn đẳng thức vì tất cả các phép biến đổi đều là tuyến tính và mô đun là số nguyên tố, đảm bảo tồn tại nghịch đảo cho x − 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve():
    T = int(input())
    for _ in range(T):
        m, x = map(int, input().split())

        if x == 1:
            # degenerate case: sum of 1 repeated (m+1) times
            print((m + 1) % MOD)
            continue

        # geometric sum: (x^(m+1) - 1) / (x - 1)
        num = modpow(x, m + 1) - 1
        num %= MOD

        den_inv = modpow(x - 1, MOD - 2)
        ans = num * den_inv % MOD
        print(ans)

if __name__ == "__main__":
    solve()
```Mã này trực tiếp thực hiện việc dẫn xuất dạng đóng. Hàm lũy thừa nhanh tính lũy thừa theo thời gian logarit, điều này cần thiết vì m có thể lên tới 10^9. Nghịch đảo môđun sử dụng định lý Fermat vì 998244353 là số nguyên tố. 

Trường hợp đặc biệt x = 1 được xử lý riêng vì công thức hình học suy biến; tổng trở thành m + 1 vì mọi số hạng đều bằng 1. 

## Ví dụ đã hoạt động 

Xét trường hợp nhỏ m = 3, x = 2. Dãy số là: 

n = 1 + 2 + 4 + 8 = 15 

Chúng tôi tính toán nó thông qua công thức. 

| Bước | Giá trị | 
| --- | --- | 
| số mũ m+1 | 4 | 
| 2^4 | 16 | 
| tử số | 15 | 
| nghịch đảo của 1 | 1 | 
| kết quả | 15 | 

Điều này xác nhận dạng đóng phù hợp với việc mở rộng trực tiếp. 

Bây giờ xét m = 2, x = 3: 

n = 1 + 3 + 9 = 13 

| Bước | Giá trị | 
| --- | --- | 
| số mũ m+1 | 3 | 
| 3^3 | 27 | 
| tử số | 26 | 
| nghịch đảo của 2 | 499122177 | 
| kết quả | 13 | 

Dấu vết này cho thấy việc đảo ngược mô-đun sẽ tái tạo lại tổng số học một cách chính xác ngay cả trong mô-đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log m) mỗi lần kiểm tra | lũy thừa nhanh cho x^(m+1) và nghịch đảo mô-đun | 
| Không gian | O(1) | chỉ một số số nguyên được lưu trữ | 

Giải pháp có thể xử lý tối đa 10^3 trường hợp thử nghiệm một cách thoải mái vì mỗi trường hợp chạy theo thời gian logarit đối với m, tránh mọi sự phụ thuộc vào tham số tuyến tính lớn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve():
    input = sys.stdin.readline
    T = int(input())
    for _ in range(T):
        m, x = map(int, input().split())
        if x == 1:
            print((m + 1) % MOD)
            continue
        num = (modpow(x, m + 1) - 1) % MOD
        den = modpow(x - 1, MOD - 2)
        print(num * den % MOD)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old_stdin
    return out.getvalue().strip()

# sample-like checks
assert run("1\n3 2\n") == "15"
assert run("1\n2 3\n") == "13"

# edge cases
assert run("1\n0 5\n") == "1"
assert run("1\n5 1\n") == "6"
assert run("1\n1 2\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| m=3,x=2 | 15 | tính đúng đắn của khai triển hình học | 
| m=2,x=3 | 13 | độ đúng nghịch đảo mô-đun | 
| m=0,x=5 | 1 | hành vi chuỗi rỗng | 
| m=5,x=1 | 6 | xử lý suy biến x=1 | 
| m=1,x=2 | 3 | độ chính xác chuyển tiếp đơn | 

## Vỏ cạnh 

Khi x = 1, mỗi bước sẽ trở thành a − 1 = 1 * next_value, do đó mỗi bước chỉ trừ đi một bước. Bắt đầu từ giá trị cuối cùng 1, đảo ngược m lần sẽ cho n = m + 1. Việc triển khai xử lý rõ ràng trường hợp này vì công thức hình học sẽ cố gắng chia cho 0 modulo MOD. 

Khi m = 0, thực tế không có sự biến đổi nào. Giá trị hợp lệ duy nhất là n = 1, vì con cuối cùng cũng là con đầu tiên. 

Với x = 2 với m lớn, các giá trị tăng theo cấp số nhân, nhưng phép lũy thừa mô đun ngăn ngừa tràn. Phép truy toán ổn định theo modulo vì tất cả các phép toán đều tuyến tính và được xử lý thông qua lũy thừa thay vì lặp.
