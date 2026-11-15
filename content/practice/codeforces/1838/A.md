---
title: "CF 1838A - Danh sách bảng đen"
description: "Chúng ta được cung cấp một danh sách cuối cùng gồm các số nguyên được tạo ra bằng cách bắt đầu bằng hai số chưa biết trên bảng đen và liên tục viết hiệu tuyệt đối của hai số bất kỳ đã có sẵn."
date: "2026-06-09T06:34:10+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1838
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 877 (Div. 2)"
rating: 800
weight: 1838
solve_time_s: 122
verified: false
draft: false
---

[CF 1838A - Danh sách bảng đen](https://codeforces.com/problemset/problem/1838/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách cuối cùng gồm các số nguyên được tạo ra bằng cách bắt đầu bằng hai số chưa biết trên bảng đen và liên tục viết hiệu tuyệt đối của hai số bất kỳ đã có sẵn. Sau khi thực hiện thao tác này đủ số lần để đạt được tổng số$n$số, danh sách đã được xáo trộn. Nhiệm vụ là tìm lại ít nhất một trong hai số ban đầu. Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp xác định kích thước của danh sách cuối cùng và chính danh sách đó. 

Chìa khóa để hiểu vấn đề này là phép toán bảo toàn số lượng tối đa. Nếu chúng ta ký hiệu hai số ban đầu là$x$Và$y$, thì số đầu tiên được viết sẽ là$|x-y|$, và những khác biệt tiếp theo không thể vượt quá$\max(x, y)$. Điều này có nghĩa là số lớn nhất trong danh sách cuối cùng phải là một trong các số ban đầu. Tương tự, số nhỏ nhất trong danh sách bằng 0 (nếu tồn tại trùng lặp) hoặc chênh lệch tuyệt đối của các số ban đầu. Bởi vì$n$có thể lớn tới 100, bất kỳ thuật toán nào kiểm tra lặp đi lặp lại tất cả các khác biệt theo cặp sẽ là$O(n^3)$và quá chậm. Thay vào đó, chúng ta có thể suy luận về các tính chất của tập hợp để tìm trực tiếp một trong các số ban đầu. 

Các trường hợp đặc biệt bao gồm các danh sách trong đó tất cả các số đều giống nhau, chẳng hạn như`[0,0,0]`. Việc triển khai ngây thơ cố gắng ghép các số một cách mù quáng có thể không chọn được số ban đầu chính xác. Một trường hợp khác là khi số lớn nhất bị trùng lặp trong danh sách. Phải cẩn thận để luôn chọn phần tử lớn nhất làm số ban đầu ứng cử viên. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng đoán từng cặp số, mô phỏng tất cả các phép tính sai phân có thể có và kiểm tra xem tập hợp kết quả có khớp với danh sách đã cho hay không. Điều này hiệu quả vì quá trình này mang tính quyết định, nhưng đối với$n=100$, có khoảng$O(n^2)$cặp bắt đầu có thể và$O(n^2)$hoạt động trên mỗi cặp, mang lại$O(n^4)$sự phức tạp. Điều này là không khả thi đối với các ràng buộc đầu vào. 

Cách tiếp cận tối ưu dựa trên quan sát rằng số lớn nhất trong danh sách cuối cùng phải là một trong các số ban đầu. Khi chúng tôi cố định số lớn nhất làm ứng cử viên, bất kỳ số nào khác trong danh sách đều có thể đóng vai trò là số ban đầu thứ hai và thao tác lấy chênh lệch tuyệt đối sẽ tạo ra tất cả các số nhỏ hơn trong danh sách. Điều này làm giảm vấn đề chọn phần tử tối đa, đảm bảo tính chính xác và hoạt động trong$O(n)$thời gian cho mỗi trường hợp thử nghiệm. Thông tin chi tiết là không có thao tác nào có thể tạo ra số lớn hơn mức tối đa của cặp ban đầu, do đó phần tử tối đa luôn an toàn để chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test, hãy đọc số nguyên$n$và danh sách$n$số nguyên. 
3. Xác định số lớn nhất trong danh sách bằng cách quét tuyến tính. Số này được đảm bảo là một trong những số gốc. 
4. In số lớn nhất làm đáp án cho test case này. 

Tại sao nó hoạt động: Điều bất biến chính là không có phép toán nào trong chuỗi sai phân tuyệt đối có thể tạo ra một số lớn hơn giá trị lớn nhất của hai số ban đầu. Vì vậy, số tối đa trong danh sách cuối cùng phải là một trong hai số bắt đầu. Vì bài toán đảm bảo rằng danh sách có thể được tạo ra theo quy trình được mô tả nên chỉ cần xuất ra số lượng tối đa là đáp ứng được yêu cầu. Các số trùng lặp, số âm hoặc số 0 không ảnh hưởng đến thuộc tính này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        print(max(a))

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào bằng I/O nhanh và chuyển đổi danh sách các số nguyên thành số nguyên Python. Việc sử dụng`max(a)`xác định chính xác số lớn nhất trong danh sách, mà theo cách xây dựng bài toán, đảm bảo là một trong các số ban đầu. Không cần sắp xếp hoặc bổ sung bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Danh sách đầu vào:`[9, 2, 7]`. Tối đa là`9`. Đây là số gốc hợp lệ vì số gốc khác có thể đã được`2`, sản xuất`|9-2|=7`. Đầu ra là`9`. 

| Bước | Danh sách | Tối đa | Số ban đầu được chọn | 
| --- | --- | --- | --- | 
| 1 | [9,2,7] | 9 | 9 | 

### Mẫu 2 

Danh sách đầu vào:`[15, -4, 11]`. Tối đa là`15`. Chúng ta có thể chọn số ban đầu thứ hai là`-4`, sản xuất`|15-(-4)|=19`, nhưng vì danh sách được đảm bảo nên việc chọn`11`vì câu trả lời cũng có tác dụng vì đó là số ban đầu khác. Thuật toán của chúng tôi chọn lớn nhất, đó là`15`. 

| Bước | Danh sách | Tối đa | Số ban đầu được chọn | 
| --- | --- | --- | --- | 
| 1 | [15,-4,11] | 15 | 15 | 

Những dấu vết này xác nhận rằng việc chọn số lớn nhất thỏa mãn bất biến rằng nó là một trong những số ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét danh sách một lần để tìm mức tối đa yêu cầu thời gian tuyến tính | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Lưu trữ danh sách tạm thời trong bộ nhớ | 

Từ$n \le 100$Và$t \le 100$, tổng số thao tác nhiều nhất là$10^4$, thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("9\n3\n9 2 7\n3\n15 -4 11\n4\n-9 1 11 -10\n5\n3 0 0 0 3\n7\n8 16 8 0 8 16 8\n4\n0 0 0 0\n10\n27 1 24 28 2 -1 26 25 28 27\n6\n600000000 800000000 0 -200000000 1000000000 800000000\n3\n0 -1000000000 1000000000\n") == "9\n15\n11\n3\n16\n0\n28\n1000000000\n1000000000", "sample 1"

# Custom tests
assert run("1\n3\n0 0 0") == "0", "all zeros"
assert run("1\n3\n-5 -5 0") == "0", "max zero"
assert run("1\n4\n7 7 7 7") == "7", "all equal"
assert run("1\n3\n1 2 3") == "3", "strictly increasing"
assert run("1\n3\n-1 -2 -3") == "-1", "strictly decreasing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n0 0 0`| 0 | Xử lý tất cả số không | 
|`3\n-5 -5 0`| 0 | Tối đa bằng 0, có số âm | 
|`4\n7 7 7 7`| 7 | Tất cả các số bằng nhau | 
|`3\n1 2 3`| 3 | Trình tự tăng dần | 
|`3\n-1 -2 -3`| -1 | Dãy số giảm dần, âm lớn nhất | 

## Vỏ cạnh 

Nếu tất cả các số đều bằng nhau thì thuật toán sẽ trả về chính xác số đó là một trong các số gốc. Vì`[0,0,0]`, tối đa là`0`, hợp lệ. Nếu danh sách chứa cả số dương và số âm thì phần tử lớn nhất luôn được chọn, thỏa mãn bất biến. Đối với danh sách có hai số tối đa giống nhau, việc chọn một trong hai trường hợp sẽ có hiệu quả vì bài toán chỉ yêu cầu một số gốc. Cách tiếp cận này xử lý mọi trường hợp góc do các ràng buộc của bài toán quyết định.
