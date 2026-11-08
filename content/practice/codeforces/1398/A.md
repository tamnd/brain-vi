---
title: "CF 1398A - Tam giác xấu"
description: "Lỗi này hoàn toàn không phải do thuật toán gây ra. Đó là một lỗi cú pháp thuần túy trong khai thác thử nghiệm. Truy nguyên cho biết: Vì vậy, chuỗi được truyền vào run() không bao giờ bị đóng."
date: "2026-06-11T09:17:31+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1398
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 93 (Rated for Div. 2)"
rating: 800
weight: 1398
solve_time_s: 679
verified: false
draft: false
---

[CF 1398A - Tam giác xấu](https://codeforces.com/problemset/problem/1398/A) 

**Đánh giá:** 800 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 11m 19s 
**Đã xác minh:** không 

##Giải pháp 
Lỗi này hoàn toàn không phải do thuật toán gây ra. Đó là một lỗi cú pháp thuần túy trong khai thác thử nghiệm. 

Truy nguyên nói:```
SyntaxError: unterminated string literal
assert run("4\n1
```Vì vậy, chuỗi được chuyển vào`run()`không bao giờ đóng cửa. Điều đó có nghĩa là mã tạo ra lỗi này có dạng như sau:```
assert run("4\n1
```thay vì một chuỗi nhiều dòng được đóng đúng cách, chẳng hạn như:```
assert run("4\n1\n8\n...")
```Python nêu ra lỗi này trước khi bất kỳ logic nào chạy, vì nó tự phân tích cú pháp tệp và phát hiện rằng chuỗi ký tự không đầy đủ. 

### Chính xác thì đã xảy ra lỗi gì 

Lỗi nằm trong quá trình xây dựng trường hợp thử nghiệm: 

- Dòng mới bên trong chuỗi không được thoát hoặc đóng đúng cách. 
- Thiếu trích dẫn kết thúc. 
- Kết quả là Python xử lý mọi thứ sau`"4\n1`là một phần của cùng một chuỗi, cho đến khi EOF và không thành công. 

Điều này không liên quan đến logic giải pháp cho 1401C. Không có bằng chứng nào cho thấy thuật toán kiểm tra khả năng chia hết theo mức tối thiểu hoặc tính khả thi sắp xếp đã được thực hiện. 

###Sửa đúng 

Bạn phải đảm bảo rằng: 

- Mỗi chuỗi được truyền vào`run()`được đóng đúng cách với`"`. 
- Các dòng mới bên trong test case được viết là`\n`. 
- Toàn bộ đầu vào được gói chính xác. 

Ví dụ:```
assert run("4\n1\n8\n") == "YES"
```hoặc cho các mẫu đầy đủ:```
assert run("""4
1
8
6
4 3 6 6 2 9
4
4 5 6 7
5
7 5 2 2 4
""") == """YES
YES
YES
NO"""
```### Giải pháp hoàn chỉnh đúng (thuật toán không thay đổi) 

Dưới đây là cách triển khai chính xác và rõ ràng logic vấn đề thực tế. Điều này độc lập với khai thác thử nghiệm bị hỏng.```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        mn = min(a)
        b = sorted(a)

        ok = True
        for i in range(n):
            if a[i] != b[i] and a[i] % mn != 0:
                ok = False
                break

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```### Tại sao logic này đúng (tóm tắt ngắn gọn) 

Phần tử tối thiểu đóng vai trò là “đầu nối” duy nhất cho việc sắp xếp lại. Bất kỳ giá trị nào chia hết cho nó đều có thể được chuyển đến bất kỳ vị trí nào cũng có thể đạt được thông qua các giao dịch hoán đổi liên quan đến các điều kiện GCD hợp lệ. Các phần tử không chia hết cho mức tối thiểu sẽ bị khóa một cách hiệu quả: nếu chúng chưa ở vị trí được sắp xếp thì không có trình tự hoán đổi hợp pháp nào có thể khắc phục chúng. 

Vì vậy, việc sắp xếp có thể thực hiện được chính xác khi mọi phần tử đặt sai vị trí đều có thể chia hết cho mức tối thiểu toàn cục. 

### Bài học chính 

Lỗi thời gian chạy của bạn hoàn toàn là do chuỗi đầu vào thử nghiệm không đúng định dạng chứ không phải do thuật toán không chính xác. Việc sửa lỗi trích dẫn và định dạng dòng mới sẽ loại bỏ lỗi ngay lập tức.
