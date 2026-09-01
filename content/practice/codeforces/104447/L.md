---
title: "CF 104447L - Người thầy tuyệt vời"
description: "Chúng tôi được cung cấp một chuỗi các trường hợp thử nghiệm độc lập. Mỗi bộ kiểm tra chứa một số nguyên duy nhất biểu thị điểm ban đầu của học sinh theo thang điểm từ 0 đến 10."
date: "2026-06-30T18:46:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104447
codeforces_index: "L"
codeforces_contest_name: "Al-Baath Collegiate Programming Contest 2023"
rating: 0
weight: 104447
solve_time_s: 36
verified: true
draft: false
---

[CF 104447L - Người thầy tuyệt vời](https://codeforces.com/problemset/problem/104447/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các trường hợp thử nghiệm độc lập. Mỗi bộ kiểm tra chứa một số nguyên duy nhất biểu thị điểm ban đầu của học sinh theo thang điểm từ 0 đến 10. Nhiệm vụ là chuyển đổi điểm này theo một bộ quy tắc đơn giản mô phỏng một giáo viên nâng cấp các bài nộp đạt hoặc khác 0 thành điểm hoàn hảo, nhưng từ chối làm như vậy đối với các bài nộp hoàn toàn trống. 

Đối với mỗi trường hợp kiểm thử, nếu điểm ban đầu lớn hơn 0 thì kết quả đầu ra sẽ là 10. Nếu điểm chính xác bằng 0 thì kết quả đầu ra vẫn là 0. 

Những hạn chế là cực kỳ nhỏ. Có tối đa 11 trường hợp thử nghiệm và mỗi điểm nằm trong khoảng từ 0 đến 10. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu phức tạp hoặc tối ưu hóa. Bất kỳ giải pháp nào xử lý từng trường hợp thử nghiệm trong thời gian không đổi là đủ. 

Trường hợp cạnh tinh tế duy nhất là ranh giới giữa giá trị 0 và dương. Một sai lầm ngây thơ sẽ là coi số 0 là hợp lệ để nâng cấp hoặc giả định không chính xác rằng tất cả các đầu vào sẽ ánh xạ tới 10 bất kể giá trị. Ví dụ, đầu vào`0`phải ở lại`0`, trong khi nhập`1`phải trở thành`10`. Một lỗi khác có thể xảy ra là quá phức tạp hóa quy tắc và vô tình đưa ra logic sai lệch, chẳng hạn như kiểm tra`n >= 0`, điều này cũng sẽ nâng cấp không chính xác số 0. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo của vấn đề có thể cố gắng mô phỏng rõ ràng logic phân loại với nhiều nhánh có điều kiện hoặc thậm chí lưu trữ một bảng ánh xạ cho tất cả các giá trị có thể có từ 0 đến 10. Điều này sẽ hoạt động chính xác vì miền đầu vào rất nhỏ và chúng ta có thể chỉ cần xác định trước kết quả đầu ra cho từng điểm có thể. 

Tuy nhiên, cách tiếp cận như vậy là không cần thiết. Cấu trúc của bài toán cho thấy rằng chỉ có một điều kiện quan trọng: giá trị có bằng 0 hay không. Cách tiếp cận bạo lực xử lý từng giá trị một cách độc lập, nhưng chúng ta có thể nén tất cả các trường hợp khác 0 thành một kết quả duy nhất vì chúng hoạt động giống hệt nhau. Quan sát này làm giảm vấn đề thành một kiểm tra có điều kiện duy nhất. 

Thông tin chi tiết quan trọng là toàn bộ quá trình chuyển đổi là phân loại nhị phân: 0 ánh xạ tới 0, mọi thứ khác ánh xạ tới 10. Khi điều này được nhận ra, giải pháp sẽ trở thành thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng ánh xạ vũ phu | O(1) mỗi trường hợp | O(1) | Đã chấp nhận | 
| Kiểm tra có điều kiện tối ưu | O(1) mỗi trường hợp | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Điều này xác định có bao nhiêu phép biến đổi độc lập phải được áp dụng. 
2. Với mỗi test, hãy đọc số nguyên. 
3. Kiểm tra xem điểm có bằng 0 hay không. Đây là điểm quyết định có ý nghĩa duy nhất vì tất cả các giá trị dương đều hoạt động giống hệt nhau. 
4. Nếu điểm bằng 0, xuất ra 0. Ngược lại, xuất ra số mười. 

Mỗi quyết định đều mang tính cục bộ đối với trường hợp thử nghiệm của nó, vì vậy không cần phải thực hiện trạng thái nào qua các lần lặp lại. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là phép biến đổi chỉ phụ thuộc vào thành viên trong một phân vùng hai phần tử của miền đầu vào: tập đơn chứa 0 và tập hợp tất cả các số nguyên dương trong phạm vi. Vì tất cả các đầu vào khác 0 đều được ánh xạ tới cùng một đầu ra nên việc phân biệt giữa chúng là không cần thiết. Do đó, thuật toán duy trì tính chính xác bằng cách áp dụng định nghĩa quy tắc chính xác mà không cần xấp xỉ hoặc tổng hợp vượt quá những gì chính quy tắc đó cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        if n > 0:
            print(10)
        else:
            print(0)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo thuật toán trực tiếp. Đầu vào được xử lý bằng cách sử dụng I/O nhanh vì đây là thông lệ tiêu chuẩn trong lập trình cạnh tranh, mặc dù các ràng buộc ở đây không yêu cầu điều đó. 

điều kiện`if n > 0`là cốt lõi của giải pháp. Nó cố tình tránh viết`if n != 0`hoặc`if n >= 0`bởi vì cái sau sẽ ánh xạ không chính xác từ 0 đến 10. Việc tách biệt hai kết quả đầu ra là rõ ràng và tránh mọi thủ thuật số học ẩn có thể gây ra lỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét đầu vào:```
3
0
5
1
```| Trường hợp thử nghiệm | n | Điều kiện (n > 0) | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 0 | sai | 0 | 
| 2 | 5 | đúng | 10 | 
| 3 | 1 | đúng | 10 | 

Trường hợp thử nghiệm đầu tiên xác nhận rằng số 0 được giữ nguyên. Các trường hợp còn lại cho thấy tất cả các giá trị dương đều thu gọn về cùng một đầu ra. 

### Ví dụ 2 

đầu vào:```
4
10
2
0
7
```| Trường hợp thử nghiệm | n | Điều kiện (n > 0) | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 10 | đúng | 10 | 
| 2 | 2 | đúng | 10 | 
| 3 | 0 | sai | 0 | 
| 4 | 7 | đúng | 10 | 

Dấu vết này chứng tỏ rằng giới hạn trên của phạm vi đầu vào không quan trọng. Ngay cả giá trị tối đa 10 cũng được xử lý giống hệt với bất kỳ giá trị dương nào khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một lần so sánh duy nhất và đầu ra theo thời gian không đổi | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ nào được sử dụng ngoài các biến đầu vào | 

Cho rằng t nhiều nhất là 11, nghiệm chạy trong thời gian không đổi trong thực tế và gần như nằm trong giới hạn. 

Việc sử dụng bộ nhớ là tối thiểu vì không cần mảng hoặc bộ tích lũy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n = int(input())
        if n > 0:
            print(10)
        else:
            print(0)

# provided samples
assert run("3\n0\n5\n1\n") == "0\n10\n10"
assert run("4\n10\n2\n0\n7\n") == "10\n10\n0\n10"

# custom cases
assert run("1\n0\n") == "0", "zero stays zero"
assert run("1\n1\n") == "10", "smallest positive becomes 10"
assert run("1\n10\n") == "10", "max value still becomes 10"
assert run("5\n0\n0\n0\n0\n0\n") == "0\n0\n0\n0\n0", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0 | 0 | ranh giới không | 
| đơn 1 | 10 | trường hợp tích cực tối thiểu | 
| đơn 10 | 10 | trường hợp giới hạn trên | 
| tất cả số không | tất cả 0 | ổn định cạnh lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là chính giá trị 0. Đối với đầu vào`0`, thuật toán đánh giá điều kiện`n > 0`là đầu ra sai và chính xác`0`. Một dấu vết đúng là: 

| Bước | n | Điều kiện (n > 0) | Đầu ra | 
| --- | --- | --- | --- | 
| đọc | 0 | - | - | 
| đánh giá | 0 | sai | 0 | 

Điều này xác nhận rằng số 0 không bao giờ được nâng cấp. 

Đối với một giá trị dương tối thiểu như`1`, điều kiện được đánh giá là đúng, tạo ra`10`và hành vi này nhất quán trên tất cả các đầu vào tích cực bất kể cường độ.
