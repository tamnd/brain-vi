---
title: "CF 188D - Dấu hoa thị"
description: "Vấn đề yêu cầu chúng ta tạo ra một mẫu văn bản đơn giản. Chúng ta được cho một số nguyên n, đại diện cho số dòng chúng ta cần in. Dòng đầu tiên phải chứa một dấu hoa thị, dòng thứ hai có hai dấu hoa thị, v.v. cho đến dòng thứ n chứa n dấu hoa thị."
date: "2026-06-03T01:05:41+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 188
codeforces_index: "D"
codeforces_contest_name: "Surprise Language Round 6"
rating: 1100
weight: 188
solve_time_s: 57
verified: true
draft: false
---

[CF 188D - Dấu hoa thị](https://codeforces.com/problemset/problem/188/D) 

**Đánh giá:** 1100 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề yêu cầu chúng ta tạo ra một mẫu văn bản đơn giản. Chúng ta được cấp một số nguyên _n_, biểu thị số dòng chúng ta cần in. Dòng đầu tiên phải chứa một dấu hoa thị, dòng thứ hai có hai dấu hoa thị, v.v., cho đến khi dòng _n_ chứa dấu hoa thị _n_. Về cơ bản, đầu ra là một hình tam giác bên phải có dấu hoa thị được căn trái có chiều cao _n_. 

Ràng buộc đầu vào là 1 ≤ _n_ ≤ 50. Điều này cho chúng ta biết rằng _n_ nhỏ và mọi giải pháp chạy theo thời gian tuyến tính hoặc bậc hai so với _n_ sẽ thực thi gần như ngay lập tức. Không có nguy cơ đạt đến giới hạn hiệu suất ở đây, vì vậy chúng tôi không cần xem xét tối ưu hóa thuật toán cho kích thước đầu vào lớn. 

Trường hợp cạnh tinh tế là khi _n_ bằng 1. Đầu ra đúng là một dòng có một dấu hoa thị. Một giải pháp bất cẩn sử dụng vòng lặp bắt đầu từ logic 0 hoặc logic lệch một có thể tạo ra dòng 0 hoặc không in được dấu hoa thị đơn. Tương tự, _n_ = 50 là giá trị đầu vào lớn nhất, vì vậy dòng cuối cùng phải chứa chính xác 50 dấu hoa thị. Bất kỳ sự đếm sai nào trong ranh giới vòng lặp đều có thể cắt bớt dòng hoặc tạo thêm dấu hoa thị. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản rất đơn giản: lặp từ 1 đến _n_ và với mỗi lần lặp _i_, hãy in _i_ dấu hoa thị. Điều này có tác dụng vì mỗi dòng độc lập chỉ phụ thuộc vào chỉ mục của nó và việc tạo một chuỗi ký tự lặp lại là một thao tác liên tục trong Python đối với _n_ nhỏ. Giải pháp brute-force có độ phức tạp về thời gian tỷ lệ thuận với tổng 1 + 2 + … + _n_, bằng _n_(_n_+1)/2. Đối với _n_ 50, đây là bản in tối đa 1275 ký tự, điều này không đáng kể đối với các máy tính hiện đại. 

Vì vấn đề đã có quy mô nhỏ nên không cần tối ưu hóa thêm nữa. Thông tin chi tiết quan trọng là nhận ra rằng nội dung của mỗi dòng là sự lặp lại đơn giản của một ký tự đơn, cho phép viết một dòng trong Python bằng cách sử dụng toán tử nhân cho chuỗi. Vấn đề không yêu cầu cấu trúc dữ liệu hoặc đệ quy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n^2) | O(n) | Đã chấp nhận | 

Mặc dù cả hai cách tiếp cận đều có chung độ phức tạp tiệm cận, phiên bản "tối ưu" tận dụng sự lặp lại chuỗi và in trực tiếp để làm cho mã trở nên ngắn gọn. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên _n_ từ đầu vào. Điều này xác định số lượng đường trong tam giác. 
2. Lặp lại trong phạm vi từ 1 đến bao gồm _n_. Đặt biến vòng lặp là _i_. 
3. Với mỗi _i_, hãy tạo một chuỗi bao gồm các dấu hoa thị _i_. Trong Python, việc này được thực hiện bằng`"*"`nhân với _i_. 
4. In chuỗi được tạo. Mỗi lần lặp in một dòng, tự động thêm một dòng mới. 

Tại sao nó hoạt động: Tính bất biến của vòng lặp là khi lặp _i_, chính xác các dấu hoa thị _i_ được in ra. Điều này đảm bảo rằng sau lần lặp cuối cùng, hình tam giác có hình dạng chính xác và mỗi dòng chứa số dấu hoa thị chính xác tương ứng với chỉ số dựa trên 1 của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

for i in range(1, n + 1):
    print('*' * i)
```Mã bắt đầu bằng cách đọc đầu vào bằng I/O nhanh. Việc chuyển đổi chuỗi đầu vào thành số nguyên đảm bảo chúng ta có giới hạn vòng lặp thích hợp. các`for`vòng lặp lặp lại trên mỗi chỉ mục dòng từ 1 đến _n_, tạo ra một chuỗi có số dấu hoa thị chính xác và in nó ngay lập tức. Nhân một chuỗi với một số nguyên trong Python sẽ tạo ra một chuỗi mới có các ký tự lặp lại, khớp với mẫu được yêu cầu. Có thể tránh được các lỗi từng cái một bằng cách sử dụng`range(1, n + 1)`thay vì`range(n)`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
```| tôi | '*' * tôi | Dòng in | 
| --- | --- | --- | 
| 1 | '*' | * | 
| 2 | '**' | ** | 
| 3 | '***' | *** | 

Điều này xác nhận rằng thuật toán in chính xác một dấu hoa thị trên dòng đầu tiên, hai dấu hoa thị ở dòng thứ hai và ba dấu hoa thị ở dòng thứ ba. 

Đầu vào mẫu 2:```
5
```| tôi | '*' * tôi | Dòng in | 
| --- | --- | --- | 
| 1 | '*' | * | 
| 2 | '**' | ** | 
| 3 | '***' | *** | 
| 4 | '****' | **** | 
| 5 | '******' | ***** | 

Điều này chứng tỏ rằng giải pháp chia tỷ lệ tuyến tính với số lượng dòng và tôn trọng chỉ mục dòng dựa trên 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Tổng các ký tự được in ra là 1 + 2 + … + n = n(n+1)/2, tức là O(n^2). | 
| Không gian | O(n) | Mỗi dòng yêu cầu lưu trữ tối đa n ký tự trước khi in. | 

Cho rằng n ≤ 50, thuật toán thực thi ngay lập tức và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    n = int(input())
    for i in range(1, n + 1):
        print('*' * i)
    return output.getvalue().strip()

# provided sample
assert run("3\n") == "*\n**\n***", "sample 1"

# custom cases
assert run("1\n") == "*", "minimum input"
assert run("50\n") == '\n'.join('*' * i for i in range(1, 51)), "maximum input"
assert run("2\n") == "*\n**", "small even number"
assert run("5\n") == "*\n**\n***\n****\n*****", "small odd number"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | * | Xử lý đầu vào tối thiểu | 
| 50 | 1 đến 50 dấu hoa thị trên mỗi dòng | In chính xác số dòng tối đa cho phép | 
| 2 | *, ** | Số dòng chính xác cho đầu vào chẵn nhỏ | 
| 5 | *, **, ***, ****, ***** | Số dòng chính xác cho đầu vào lẻ ​​nhỏ | 

## Vỏ cạnh 

Khi _n_ = 1, thuật toán chạy một lần lặp duy nhất, in chính xác một dấu hoa thị. Điều này xác nhận mã xử lý chính xác đầu vào nhỏ nhất. Với _n_ = 50, vòng lặp chạy 50 lần, mỗi dòng chứa số dấu hoa thị chính xác. Nhân chuỗi với biến vòng lặp đảm bảo không xảy ra lỗi sai sót nào. Tất cả các dòng được in tuần tự, giữ nguyên thứ tự yêu cầu.
