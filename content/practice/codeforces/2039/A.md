---
title: "CF 2039A - Shohag Loves Mod"
description: "Chúng ta cần xây dựng một chuỗi có độ dài tăng dần $n$, trong đó mọi giá trị nằm trong khoảng từ 1 đến 100. Chuỗi phải thỏa mãn một điều kiện đặc biệt liên quan đến số dư. Với mỗi vị trí $i$, xét giá trị $ai bmod i$."
date: "2026-06-08T09:55:28+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2039
codeforces_index: "A"
codeforces_contest_name: "CodeTON Round 9 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 800
weight: 2039
solve_time_s: 130
verified: false
draft: false
---

[CF 2039A - Shohag Loves Mod](https://codeforces.com/problemset/problem/2039/A) 

**Đánh giá:** 800 
**Tas:** giải thuật xây dựng, lý thuyết số 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một dãy có độ dài tăng dần$n$, trong đó mọi giá trị nằm trong khoảng từ 1 đến 100. Chuỗi phải đáp ứng một điều kiện đặc biệt liên quan đến số dư. 

Đối với mỗi vị trí$i$, xét giá trị$a_i \bmod i$. Yêu cầu là những phần còn lại này đều khác nhau. Nói cách khác, nếu chúng ta tính toán$$a_1 \bmod 1,\ a_2 \bmod 2,\ a_3 \bmod 3,\ \ldots,\ a_n \bmod n,$$thì không có hai cái nào có thể bằng nhau. 

Đầu vào chứa một số trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi chỉ được cung cấp$n$, và chúng ta phải xuất ra bất kỳ chuỗi tăng hợp lệ nào. 

Những hạn chế là cực kỳ nhỏ. Giá trị lớn nhất có thể là$n=50$và mọi phần tử phải ở mức tối đa là 100. Vì câu lệnh đảm bảo rằng giải pháp luôn tồn tại nên nhiệm vụ thực sự là khám phá một mẫu mang tính xây dựng đơn giản thay vì tìm kiếm một mẫu. 

Một sai lầm phổ biến là tập trung vào các giá trị thay vì phần còn lại. Ví dụ, dãy tăng dần```
1 2 3
```trông có vẻ tự nhiên nhưng$$1 \bmod 1 = 0,\quad 2 \bmod 2 = 0,\quad 3 \bmod 3 = 0$$vì vậy tất cả phần còn lại đều giống hệt nhau, vi phạm điều kiện. 

Một cái bẫy dễ mắc khác là việc xây dựng các số dư phân biệt nhưng quên rằng dãy số phải tăng một cách nghiêm ngặt. Ví dụ, đối với$n=4$,```
4 3 2 1
```tạo ra số dư khác nhau$$0,1,2,1$$điều kiện còn lại đã thất bại và nó cũng không tăng. 

Phần thú vị của vấn đề là tìm ra một mẫu tự động đảm bảo cả hai thuộc tính cùng một lúc. 

## Phương pháp tiếp cận 

Tư duy vũ phu sẽ là tạo ra các chuỗi tăng dần và kiểm tra xem tất cả các phần còn lại có khác biệt hay không. Việc kiểm tra chuỗi ứng cử viên thật dễ dàng: tính toán$a_i \bmod i$cho mọi chỉ mục và xác minh tính duy nhất. 

Khó khăn là không gian tìm kiếm. Ngay cả với các giá trị được giới hạn ở 100, số lượng chuỗi tăng dần có độ dài 50 là rất lớn. Tìm kiếm toàn diện là hoàn toàn không thực tế. 

Quan sát quan trọng xuất phát từ việc kiểm tra phạm vi số dư có thể có. 

Đối với chỉ mục$i$, phần còn lại$a_i \bmod i$phải thuộc về$$0,1,\ldots,i-1.$$Nếu chúng ta có thể sắp xếp việc đó$$a_i \bmod i = i-1,$$thì mỗi chỉ mục sẽ nhận được phần còn lại khác nhau:$$0,1,2,\ldots,n-1.$$Bây giờ nhiệm vụ trở nên đơn giản hơn nhiều. Chúng ta chỉ cần một dãy tăng dần có$i$-phần tử thứ để lại phần còn lại$i-1$khi chia cho$i$. 

Một sự lựa chọn rất thuận tiện là$$a_i = 2i-1.$$Hãy kiểm tra phần còn lại:$$(2i-1)\bmod i = i-1.$$Thực vậy,$$2i-1 = i + (i-1),$$vậy chia cho$i$lá còn lại$i-1$. 

Trình tự$$1,3,5,7,\ldots$$đang tăng lên một cách nghiêm ngặt, mọi số dư đều khác nhau, và đối với$n \le 50$giá trị lớn nhất là$$2\cdot 50 - 1 = 99,$$thỏa mãn giới hạn 100. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Xây dựng tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$. 
2. Xây dựng trình tự$$a_i = 2i-1$$cho mọi$i$từ 1 đến$n$. 
3. Xuất các giá trị được tạo ra. 

Tại sao sự lựa chọn này hoạt động:$$a_i = 2i-1 = i + (i-1),$$Vì thế$$a_i \bmod i = i-1.$$Phần còn lại trở thành$$0,1,2,\ldots,n-1,$$tất cả đều khác biệt. 

Dãy số cũng tăng dần vì mỗi số hạng lớn hơn số trước đúng 2 đơn vị. 

### Tại sao nó hoạt động 

Bất biến là phần dư ở vị trí$i$bằng chính xác$i-1$. 

Vì các chỉ số khác nhau có giá trị khác nhau của$i-1$, không có hai vị trí nào có thể có cùng số dư. Do đó, điều kiện phân biệt theo cặp yêu cầu được tự động thỏa mãn. 

Đồng thời,$$a_{i+1}-a_i=(2(i+1)-1)-(2i-1)=2>0,$$nên dãy số tăng nghiêm ngặt. Cuối cùng, phần tử lớn nhất là 99 khi$n=50$, còn lại trong phạm vi cho phép. Mọi yêu cầu của bài toán đều được thỏa mãn đồng thời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    ans = [str(2 * i - 1) for i in range(1, n + 1)]
    print(" ".join(ans))
```Việc thực hiện trực tiếp sau khi xây dựng. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi tạo ra các số lẻ$$1,3,5,\ldots,2n-1.$$Việc sử dụng tính năng hiểu danh sách giúp mã ngắn gọn và chạy theo thời gian tuyến tính. 

Một điểm tinh tế là xác minh giới hạn trên. Từ$n\le 50$, giá trị được tạo ra lớn nhất là$99$, vì vậy không cần kiểm tra thêm. 

Việc lập chỉ mục trong bằng chứng toán học bắt đầu từ 1 và mã phản ánh điều đó bằng cách lặp lại`i`từ 1 đến`n`. Điều này tránh được những lỗi mắc phải khi liên hệ công thức$a_i=2i-1$đến việc thực hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```Trình tự được tạo:$$1,\ 3,\ 5$$| tôi | a_i | a_i mod tôi | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 3 | 1 | 
| 3 | 5 | 2 | 

Phần còn lại là$0,1,2$, tất cả đều khác biệt. Điều này chứng tỏ tính bất biến cốt lõi$a_i \bmod i=i-1$. 

### Ví dụ 2 

đầu vào:```
6
```Trình tự được tạo:$$1,\ 3,\ 5,\ 7,\ 9,\ 11$$| tôi | a_i | a_i mod tôi | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 3 | 1 | 
| 3 | 5 | 2 | 
| 4 | 7 | 3 | 
| 5 | 9 | 4 | 
| 6 | 11 | 5 | 

Phần còn lại tạo thành dãy$0,1,2,3,4,5$. Mỗi phần còn lại là duy nhất và các giá trị đang tăng lên một cách nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tạo và in n số | 
| Không gian | O(n) | Lưu trữ chuỗi đầu ra trước khi in | 

Từ$n\le 50$, thời gian chạy thực tế là rất nhỏ. Ngay cả với 50 trường hợp thử nghiệm, chỉ có vài nghìn số được tạo và in. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        out.append(" ".join(str(2 * i - 1) for i in range(1, n + 1)))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# provided samples (valid output from this construction)
assert run("2\n3\n6\n") == "1 3 5\n1 3 5 7 9 11"

# minimum n
assert run("1\n2\n") == "1 3"

# small odd size
assert run("1\n5\n") == "1 3 5 7 9"

# maximum n
expected = " ".join(str(2 * i - 1) for i in range(1, 51))
assert run("1\n50\n") == expected

# multiple test cases
assert run("3\n2\n3\n4\n") == (
    "1 3\n"
    "1 3 5\n"
    "1 3 5 7"
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n2\n`|`1 3`| Kích thước tối thiểu cho phép | 
|`1\n5\n`|`1 3 5 7 9`| Xây dựng tổng hợp | 
|`1\n50\n`| Số lẻ lên tới 99 | Kích thước tối đa và giới hạn trên 100 | 
|`3\n2\n3\n4\n`| Ba chuỗi hợp lệ | Xử lý nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

### Kích thước tối thiểu 

đầu vào:```
1
2
```Các đầu ra thuật toán:```
1 3
```Phần còn lại là$$1\bmod1=0,\qquad 3\bmod2=1.$$Chúng khác biệt và trình tự ngày càng tăng. Điều này xác minh đầu vào pháp lý nhỏ nhất. 

### Kích thước tối đa 

đầu vào:```
1
50
```Trình tự trở thành$$1,3,5,\ldots,99.$$Giá trị lớn nhất chính xác là 99, nằm dưới giới hạn 100. Phần còn lại là$$0,1,2,\ldots,49.$$Tất cả đều khác biệt. 

### Tránh lặp lại số dư bằng 0 

Một ý tưởng ngây thơ là sử dụng```
1 2 3 4 5
```Đối với trình tự này,$$a_i \bmod i = 0$$cho mọi vị trí, ngay lập tức thất bại. 

Công trình của chúng tôi tạo ra```
1 3 5 7 9
```với số dư$$0,1,2,3,4,$$nên va chạm biến mất hoàn toàn. 

### Sai lầm từng cái một trong công thức 

Giả sử có người chọn nhầm$$a_i = 2i.$$Vì$n=3$, số dư trở thành$$2\bmod1=0,\quad 4\bmod2=0,\quad 6\bmod3=0.$$Tất cả phần còn lại va chạm. 

sử dụng$$a_i=2i-1$$dịch chuyển mọi phần còn lại đến chính xác$i-1$, tạo ra trình tự riêng biệt cần thiết.
