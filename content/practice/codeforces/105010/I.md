---
title: "CF 105010I - Hòa nhập và đa dạng"
description: "Mỗi ứng cử viên có thể được đại diện hoàn toàn bởi nhóm thiểu số $m$ mà họ thuộc về. Vì vậy, mỗi người nộp đơn tương ứng với một tập hợp con của một tập hợp có kích thước $m$, và nhóm chứa tất cả các tập hợp con có thể có $2^m$."
date: "2026-06-28T02:29:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105010
codeforces_index: "I"
codeforces_contest_name: "Winter Cup 6.0 Online Mirror Contest"
rating: 0
weight: 105010
solve_time_s: 61
verified: true
draft: false
---

[CF 105010I - Hòa nhập và Đa dạng](https://codeforces.com/problemset/problem/105010/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi ứng cử viên có thể được đại diện hoàn toàn bởi ai trong số$m$nhóm thiểu số mà họ thuộc về. Vì vậy, mỗi người nộp đơn tương ứng với một tập hợp con của một tập hợp kích thước$m$, và hồ bơi chứa tất cả$2^m$tập hợp con có thể. 

Quy tắc lựa chọn cấm chọn hai ứng cử viên trong đó một tập hợp con được chứa trong một tập hợp con khác. Nếu ứng viên$A$có tất cả các nhóm thiểu số$B$và có thể hơn thế nữa$A$được coi là đa dạng hơn và cả hai không thể được chọn đồng thời. Nếu hai ứng cử viên tương ứng với cùng một tập hợp con thì chỉ có một bản sao quan trọng, nhưng vì nhóm đã chứa mọi tập hợp con chính xác một lần nên các bản sao trùng lặp không ảnh hưởng đến cấu trúc. 

Vì vậy, vấn đề giảm xuống còn việc chọn càng nhiều tập hợp con càng tốt sao cho không có tập hợp con đã chọn nào được chứa trong một tập hợp con đã chọn khác. 

Đầu vào chỉ mang lại$m$, do đó nhiệm vụ hoàn toàn mang tính tổ hợp: trong số tất cả các tập hợp con của một$m$- tập hợp phần tử, chúng tôi muốn có gia đình lớn nhất không có quan hệ bao hàm. 

Ràng buộc$m \le 4000$loại trừ bất kỳ cách xây dựng hàm mũ nào trên các tập hợp con. Thậm chí lặp đi lặp lại tất cả$2^m$tập hợp con là không thể. Mọi giải pháp đều phải hoạt động trong thời gian đa thức trong$m$, lý tưởng là xung quanh$O(m^2)$hoặc tốt hơn. 

Một trường hợp thất bại phổ biến xuất hiện khi người ta thử lựa chọn tham lam chẳng hạn như lấy tất cả các tập hợp con có kích thước cố định mà không căn chỉnh hoặc trộn các kích thước một cách tùy ý. Ví dụ, với$m = 3$, chọn tập hợp con$\{1\}, \{1,2\}, \{2\}$thất bại vì$\{1\} \subset \{1,2\}$, vi phạm quy tắc mặc dù kích thước chỉ khác nhau một chút. 

## Phương pháp tiếp cận 

Cấu trúc ở đây là mạng Boolean của các tập con được sắp xếp bằng cách đưa vào. Hạn chế nói rằng chúng tôi không thể chọn hai yếu tố có thể so sánh được trong bộ này, vì vậy chúng tôi đang tìm kiếm phản chuỗi lớn nhất. 

Một nỗ lực mạnh mẽ sẽ liệt kê tất cả các tập hợp con và thử tất cả các lựa chọn có thể, kiểm tra điều kiện bao gồm cho mỗi cặp. Ngay cả khi chúng ta giới hạn ở các tập con, số lượng các họ ứng viên vẫn là$2^{2^m}$, điều này hoàn toàn không thể thực hiện được ngay cả đối với$m = 10$. Một lực lượng vũ phu ít cực đoan hơn một chút sẽ thử tất cả các tập hợp con và duy trì một cấu trúc tham lam, nhưng bất kỳ quy tắc tham lam nào chỉ dựa trên kích thước tập hợp con hoặc thứ tự từ điển đều thất bại vì việc bao gồm phụ thuộc vào cấu trúc chứ không phải thứ tự. 

Cái nhìn sâu sắc về cấu trúc quan trọng là việc bao gồm tập hợp con được điều chỉnh bởi các lớp lượng số. Các tập con có cùng kích thước không bao giờ có quan hệ bao hàm với nhau. Bất kỳ tập hợp con nào có kích thước$k$không thể chứa tập hợp con khác có kích thước$k$và nó không thể được chứa trong một tập hợp con khác có kích thước$k$Hoặc là, vì việc ngăn chặn làm tăng kích thước một cách nghiêm ngặt. Điều này có nghĩa là mỗi “cấp” lượng số cố định tạo thành một tập hợp độc lập hợp lệ. 

Điều này làm giảm vấn đề chọn một hoặc nhiều lớp đầy đủ của mạng Boolean. Chúng tôi muốn tối đa hóa số lượng tập hợp con mà chúng tôi có thể lấy trong khi vẫn ở trong một lớp duy nhất hoặc kết hợp các lớp mà không đưa ra khả năng so sánh. Tuy nhiên, bất kỳ sự kết hợp nào của hai lớp khác nhau đều tạo ra mối quan hệ ngăn chặn, bởi vì mọi tập hợp kích thước$k$được chứa trong nhiều bộ kích thước$k+1$. Vì vậy, chúng tôi buộc phải ở trong một cấp độ duy nhất. 

Bài toán trở thành việc chọn lớp hệ số nhị thức lớn nhất, được cực đại hóa ở mức trung bình theo định lý Sperner. Vì vậy câu trả lời là$\binom{m}{\lfloor m/2 \rfloor}$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | Hàm mũ | Hàm mũ | Quá chậm | 
| Tổ hợp lớp giữa |$O(m)$hoặc$O(m^2)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp được đưa ra trực tiếp từ việc tính toán một hệ số nhị thức duy nhất. 

1. Lưu ý rằng lựa chọn tối ưu phải đến từ các tập hợp con có kích thước bằng nhau, vì bất kỳ sự khác biệt về kích thước nào cũng dẫn đến sự ngăn chặn. Điều này làm giảm vấn đề chọn tất cả các tập hợp con có kích thước cố định$k$. 
2. Số tập con có kích thước$k$là$\binom{m}{k}$. Chúng tôi muốn tối đa hóa giá trị này trên tất cả$k$. Các hệ số nhị thức tạo thành một chuỗi đơn thức đối xứng. 
3. Cực đại xảy ra ở$k = \lfloor m/2 \rfloor$. Vậy câu trả lời là$\binom{m}{\lfloor m/2 \rfloor}$. 
4. Tính giai thừa lên đến$m$, sau đó sử dụng nghịch đảo mô đun để đánh giá hệ số nhị thức theo mô đun$998244353$. 
5. Trả về kết quả. 

Tại sao nó hoạt động dựa trên cấu trúc của tập hợp con poset. Bất kỳ lựa chọn hợp lệ nào đều là một antichain. Định lý Sperner phát biểu rằng antichain lớn nhất trong mạng Boolean chính xác là lớp giữa lớn nhất và không có cấu trúc nào khác có thể vượt quá nó bởi vì bất kỳ họ tập hợp nào cũng có thể được “dịch chuyển” về phía giữa mà không giảm kích thước trong khi vẫn bảo toàn đặc tính của antichain. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    m = int(input().strip())
    
    fact = [1] * (m + 1)
    for i in range(1, m + 1):
        fact[i] = fact[i - 1] * i % MOD
    
    inv_fact = [1] * (m + 1)
    inv_fact[m] = pow(fact[m], MOD - 2, MOD)
    for i in range(m, 0, -1):
        inv_fact[i - 1] = inv_fact[i] * i % MOD
    
    k = m // 2
    ans = fact[m] * inv_fact[k] % MOD * inv_fact[m - k] % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Mảng giai thừa mã hóa$n!$các giá trị modulo số nguyên tố. Mảng giai thừa nghịch đảo được xây dựng bằng định lý nhỏ Fermat, cho phép chia theo mô đun. 

Sự lựa chọn của$k = m // 2$phản ánh trực tiếp lớp giữa của mạng Boolean. Không cần cấu trúc tổ hợp nào khác. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$m = 2$Chúng tôi tính giai thừa lên tới 2. 

| Bước | Giá trị | 
| --- | --- | 
|$k$| 1 | 
|$\binom{2}{1}$| 2 | 

Vậy đáp án là 2, tương ứng với tập con$\{1\}$Và$\{2\}$. 

Điều này xác nhận rằng việc chọn lớp giữa sẽ tránh được bất kỳ xung đột bao gồm nào, vì không có lớp đơn nào chứa lớp kia. 

### Ví dụ 2:$m = 3$| Bước | Giá trị | 
| --- | --- | 
|$k$| 1 | 
|$\binom{3}{1}$| 3 | 

Họ tối ưu là tất cả các tập con đơn lẻ. Bất kỳ nỗ lực nào nhằm bao gồm một tập hợp con gồm 2 phần tử sẽ buộc phải loại trừ các phần tử đơn lẻ chứa trong nó, vì vậy lớp giữa là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m)$| tính toán giai thừa và nghịch đảo lên đến$m$| 
| Không gian |$O(m)$| lưu trữ cho mảng giai thừa | 

Sự ràng buộc$m \le 4000$làm cho điều này trở nên tầm thường trong giới hạn 1 giây, vì chỉ cần vài nghìn phép nhân mô-đun. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve():
    m = int(input().strip())
    
    fact = [1] * (m + 1)
    for i in range(1, m + 1):
        fact[i] = fact[i - 1] * i % MOD
    
    inv_fact = [1] * (m + 1)
    inv_fact[m] = pow(fact[m], MOD - 2, MOD)
    for i in range(m, 0, -1):
        inv_fact[i - 1] = inv_fact[i] * i % MOD
    
    k = m // 2
    print(fact[m] * inv_fact[k] % MOD * inv_fact[m - k] % MOD)

def run(inp: str) -> str:
    global input
    backup = sys.stdin
    sys.stdin = io.StringIO(inp)
    from io import StringIO
    out_backup = sys.stdout
    sys.stdout = io.StringIO()
    
    solve()
    
    res = sys.stdout.getvalue().strip()
    sys.stdin = backup
    sys.stdout = out_backup
    return res

# provided samples
assert run("1\n") == "1", "sample 1"
assert run("2\n") == "2", "sample 2"
assert run("3\n") == "3", "sample 3"

# custom cases
assert run("4\n") == "6", "C(4,2)=6"
assert run("5\n") == "10", "C(5,2)=10"
assert run("6\n") == "20", "C(6,3)=20"
assert run("10\n") == "252", "middle binomial"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 | 6 | lớp giữa trường hợp chẵn | 
| 5 | 10 | đối xứng trường hợp lẻ | 
| 10 | 252 | kiểm tra tính đúng đắn lớn hơn | 

## Vỏ cạnh 

Khi nào$m = 1$, các tập hợp con duy nhất là$\emptyset$Và$\{1\}$. Lớp giữa là$k = 0$, cho$\binom{1}{0} = 1$. Thuật toán tính toán chính xác điều này vì$m // 2 = 0$. 

Khi$m = 2$, cả hai tập hợp con đơn lẻ đều tạo thành phản chuỗi tối ưu. Việc tính toán chọn$k = 1$, mang lại$\binom{2}{1} = 2$, phù hợp với lựa chọn đúng. 

Khi$m$lớn, chẳng hạn như$m = 4000$, tính toán giai thừa vẫn tuyến tính trong$m$và không cần liệt kê tập hợp con. Thuật toán dựa hoàn toàn vào tổ hợp được tính toán trước thay vì liệt kê cấu trúc, vì vậy nó nằm trong giới hạn.
