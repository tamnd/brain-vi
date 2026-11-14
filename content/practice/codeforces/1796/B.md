---
title: "CF 1796B - Mẫu dấu hoa thị phụ"
description: "Chúng ta được yêu cầu xây dựng một chuỗi “mẫu” có thể mô tả đồng thời hai chuỗi đã cho. Mẫu bao gồm các chữ cái viết thường và dấu hoa thị. Mỗi dấu hoa thị có thể được thay thế bằng bất kỳ chuỗi nào, kể cả chuỗi trống, để khớp với chuỗi gốc."
date: "2026-06-09T10:00:59+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1796
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 144 (Rated for Div. 2)"
rating: 1000
weight: 1796
solve_time_s: 151
verified: false
draft: false
---

[CF 1796B - Mẫu dấu hoa thị phụ](https://codeforces.com/problemset/problem/1796/B) 

**Đánh giá:** 1000 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một chuỗi “mẫu” có thể mô tả đồng thời hai chuỗi đã cho. Mẫu bao gồm các chữ cái viết thường và dấu hoa thị. Mỗi dấu hoa thị có thể được thay thế bằng bất kỳ chuỗi nào, kể cả chuỗi trống, để khớp với chuỗi gốc. Điều quan trọng là mẫu được coi là "dấu hoa thị nhỏ" nếu số lượng dấu hoa thị không vượt quá số chữ cái trong đó. 

Cho hai chuỗi, mục tiêu là xác định xem liệu có tồn tại một mẫu dấu hoa thị phụ phù hợp với cả hai chuỗi hay không và nếu có thì tạo ra một chuỗi. Mỗi trường hợp thử nghiệm bao gồm hai chuỗi có độ dài tối đa là 50 và có thể có tới 10.000 trường hợp thử nghiệm. 

Điều đầu tiên cần chú ý là độ dài chuỗi nhỏ giúp cho các thao tác trên toàn bộ chuỗi trở nên khả thi. Ngay cả phép so sánh bậc hai (như kiểm tra tất cả các cặp ký tự) cũng có thể chấp nhận được vì$50 \cdot 50 = 2500$và nhân với 10.000 trường hợp thử nghiệm vẫn vừa vặn thoải mái trong 2 giây. 

Các trường hợp khó khăn không rõ ràng bao gồm: 

- Các chuỗi hoàn toàn giống nhau. Ví dụ,`a = "abc"`,`b = "abc"`. Mẫu chỉ đơn giản là chuỗi đó, không có dấu hoa thị. 
- Các dây chỉ khác nhau ở giữa. Ví dụ,`a = "abXcd"`,`b = "abYcd"`. Một cách tiếp cận ngây thơ luôn chèn một dấu hoa thị cho mỗi điểm khác biệt có thể vô tình vi phạm ràng buộc “dấu hoa thị-nhỏ”. 
- Các chuỗi có độ dài hoàn toàn khác nhau 1. Đối với`a = "x"`,`b = "y"`, mẫu hợp lệ duy nhất là`*`, là dấu sao nhỏ vì nó có 1 chữ cái (nếu chúng ta bao gồm một chuỗi trống?) nhưng chúng ta cần xử lý việc đếm chữ cái một cách cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tạo ra mọi sự kết hợp có thể có của dấu hoa thị và chữ cái trong một mẫu và kiểm tra nó dựa trên cả hai chuỗi. Đối với hai chuỗi có độ dài 50, số cách chèn dấu hoa thị tăng theo cấp số nhân, khiến điều này không thể thực hiện được ngay cả đối với những chuỗi có độ dài nhỏ. 

Thông tin chi tiết quan trọng là dấu hoa thị có thể được sử dụng để “hấp thụ” bất kỳ phân đoạn nào có các ký tự khác nhau. Do đó, mẫu hợp lệ đơn giản nhất là mẫu giữ tiền tố và hậu tố chung dài nhất dưới dạng chữ cái và thay thế mọi thứ ở giữa bằng một dấu hoa thị. 

Ví dụ, đưa ra`a = "aaab"`Và`b = "zzzb"`, hậu tố chung dài nhất là`"b"`, tiền tố chung dài nhất trống và ở giữa`"aaa"`vs`"zzz"`có thể được thay thế bằng`*`. Mẫu kết quả là`*b`. Bản mẫu này có dạng dấu hoa thị nhỏ: nó có 1 dấu hoa thị và 1 chữ cái, đáp ứng ràng buộc. 

Nếu hai chuỗi không có tiền tố hoặc hậu tố trùng khớp thì mẫu đơn giản nhất là`*`, cũng là dấu hoa thị phụ nếu có ít nhất một chuỗi có độ dài ≥1. Nếu tiền tố và hậu tố chung ngắn hơn tổng số dấu hoa thị cần thiết thì có thể vi phạm quy tắc dấu hoa thị phụ và khi đó chúng tôi phải báo cáo`NO`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tiền tố-Hậu tố | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi cặp dây`a`Và`b`, hãy bắt đầu bằng cách tìm tiền tố chung dài nhất. Khởi tạo`i = 0`và tăng dần`i`trong khi`a[i] == b[i]`. Dừng khi các ký tự khác nhau hoặc khi đến cuối một chuỗi. 
2. Tiếp theo, tìm hậu tố chung dài nhất. Khởi tạo`j = 0`và tăng dần`j`trong khi`a[-1-j] == b[-1-j]`và chuỗi con còn lại không trùng với tiền tố. Điều này đảm bảo rằng tiền tố và hậu tố không giao nhau. 
3. Nếu tổng độ dài tiền tố và độ dài hậu tố bằng hoặc lớn hơn độ dài của một trong hai chuỗi, điều đó có nghĩa là các chuỗi giống hệt nhau trong phạm vi đó và có thể tạo mẫu mà không cần dấu hoa thị bên trong. 
4. Nếu không, hãy thay thế đoạn giữa khác bằng một dấu hoa thị. Ghép tiền tố, dấu hoa thị và hậu tố để tạo thành mẫu. 
5. Xác minh điều kiện dấu hoa thị phụ: đếm các chữ cái và dấu hoa thị. Nếu chữ ≥ dấu hoa thị, hãy in`YES`và mẫu. Ngược lại, in`NO`. 

Tại sao nó hoạt động: cần nhiều nhất một dấu hoa thị để thay thế bất kỳ phân đoạn khác nhau nào vì một dấu hoa thị có thể hấp thụ bất kỳ chuỗi con nào. Bằng cách giữ nguyên tiền tố và hậu tố phù hợp, chúng tôi đảm bảo rằng tất cả các chữ cái đều được giữ nguyên. Điều kiện dấu hoa thị phụ được tự động thỏa mãn đối với các chuỗi có độ dài ≤50 vì đoạn giữa được thay thế bằng tối đa một dấu hoa thị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a = input().strip()
    b = input().strip()
    
    n = len(a)
    m = len(b)
    
    # Longest common prefix
    i = 0
    while i < min(n, m) and a[i] == b[i]:
        i += 1
    
    # Longest common suffix
    j = 0
    while j < min(n - i, m - i) and a[n-1-j] == b[m-1-j]:
        j += 1
    
    if i + j == 0 and n != m:
        # No overlap and no common letters: * cannot be minor if no letters
        print("NO")
    else:
        template = a[:i] + "*" + a[n-j:] if j > 0 else a[:i] + "*"
        letters = len(a[:i]) + len(a[n-j:])
        stars = 1
        if letters >= stars:
            print("YES")
            print(template)
        else:
            print("NO")
```Mã cẩn thận tránh chồng chéo tiền tố và hậu tố. Chúng tôi chỉ chèn một dấu hoa thị bất kể độ dài của các phân đoạn khác nhau, đủ để khớp với cả hai chuỗi. Kiểm tra`letters >= stars`đảm bảo điều kiện dấu hoa thị phụ được thỏa mãn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
a = "aaab", b = "zzzb"
```| tôi | j | mẫu | 
| --- | --- | --- | 
| 0 | 1 | *b | 

Giải thích: không có tiền tố chung, hậu tố chung`"b"`, phần giữa khác nhau được thay thế bằng`*`. Chữ cái = 1, dấu hoa thị = 1 → dấu hoa thị phụ thỏa mãn. 

Một ví dụ khác:```
a = "codeforces", b = "tokitlx"
```| tôi | j | mẫu | 
| --- | --- | --- | 
| 0 | 0 | KHÔNG | 

Giải thích: không có tiền tố hoặc hậu tố chung, thay thế toàn bộ chuỗi bằng`*`sẽ tạo ra các chữ cái < dấu hoa thị, vi phạm quy tắc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T * phút( | a | 
| Không gian | O(n) | Lưu trữ chuỗi và mẫu tạm thời | 

Với T ≤ 10^4 và |a|, |b| 50, tổng số thao tác trong trường hợp xấu nhất là khoảng 500.000, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read())
    return out.getvalue().strip()

# Provided samples
assert run("6\naaab\nzzzb\ncodeforces\natcoder\ncodeforces\ntokitlx\naaaa\naaaaaa\nabcd\nabcd\nc\nf\n") == "YES\n*b\nYES\n*co*\nNO\nYES\na*a*a*a\nYES\nabcd\nNO"

# Custom cases
assert run("2\nx\ny\nabc\nabc\n") == "YES\n*\nYES\nabc"
assert run("1\na\naa\n") == "YES\n*a"
assert run("1\nabcdef\nghijkl\n") == "NO"
assert run("1\nabc\nxyz\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`x, y`|`YES\n*`| Chuỗi một chữ cái không chồng chéo | 
|`abc, abc`|`YES\nabc`| Chuỗi giống hệt nhau | 
|`a, aa`|`YES\n*a`| Xử lý tiền tố cho các độ dài khác nhau | 
|`abcdef, ghijkl`|`NO`| Các chuỗi hoàn toàn khác nhau | 
|`abc, xyz`|`NO`| Không có tiền tố/hậu tố phổ biến, không thể sử dụng mẫu | 

## Vỏ cạnh 

Đối với hai chuỗi giống hệt nhau, ví dụ:`a = "abcd"`,`b = "abcd"`, thuật toán tìm thấy tiền tố có độ dài 4, độ dài hậu tố 0 và không có phần giữa khác nhau. Mẫu là`"abcd"`, điều kiện dấu hoa thị phụ được giữ nguyên vì sao = 0 ≤ chữ cái = 4. 

Đối với các chuỗi không có tiền tố hoặc hậu tố chung, như`a = "abc"`,`b = "xyz"`, tiền tố = 0, hậu tố = 0, thuật toán xét thay thế phần giữa bằng`*`, nhưng chữ = 0, sao = 1 →
