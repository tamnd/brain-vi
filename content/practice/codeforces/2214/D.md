---
title: "CF 2214D - Mối thù thần kinh"
description: "Bài toán đưa ra một tương tác rất đơn giản: có tám câu hỏi được xác định trước, mỗi câu hỏi được liên kết với một câu trả lời bằng một từ. Bạn nhận được một số nguyên n từ 1 đến 8, cho biết câu hỏi nào đang được hỏi và bạn phải đưa ra câu trả lời tương ứng."
date: "2026-06-07T19:02:02+07:00"
tags: ["codeforces", "competitive-programming", "*special", "strings"]
categories: ["algorithms"]
codeforces_contest: 2214
codeforces_index: "D"
codeforces_contest_name: "April Fools Day Contest 2026"
rating: 0
weight: 2214
solve_time_s: 89
verified: false
draft: false
---

[CF 2214D - Mối thù thần kinh](https://codeforces.com/problemset/problem/2214/D) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, dây 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một tương tác rất đơn giản: có tám câu hỏi được xác định trước, mỗi câu hỏi được liên kết với một câu trả lời bằng một từ. Bạn nhận được một số nguyên`n`từ 1 đến 8, cho biết câu hỏi nào đang được hỏi và bạn phải đưa ra câu trả lời tương ứng. Đầu vào được đảm bảo là số nguyên hợp lệ trong phạm vi này. Thách thức không phải về bản chất tính toán mà là về việc ánh xạ đầu vào thành đầu ra một cách chính xác. 

Với các ràng buộc (`1 ≤ n ≤ 8`), hiệu suất là tầm thường. Bất kỳ giải pháp nào ánh xạ trực tiếp số câu hỏi tới câu trả lời trong thời gian không đổi là đủ. Các trường hợp đặc biệt duy nhất phát sinh từ việc lập chỉ mục sai các câu trả lời hoặc xử lý các số nằm ngoài phạm vi 1-8, nhưng theo báo cáo vấn đề, đầu vào luôn nằm trong phạm vi hợp lệ này. Một trường hợp khó phát hiện khác là khả năng không khớp giữa số đầu vào và mảng câu trả lời được lưu trữ nếu mảng có chỉ mục bằng 0; phải cẩn thận để truy cập vào phần tử chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là sử dụng một loạt câu lệnh có điều kiện. Ví dụ, kiểm tra`if n == 1`, in câu trả lời1;`elif n == 2`, in câu trả lời2, v.v. Điều này đúng nhưng dài dòng và dễ mắc lỗi. Số lượng hoạt động là không đáng kể vì`n`chỉ có thể từ 1 đến 8, nhưng việc duy trì nhiều điều kiện sẽ làm tăng khả năng viết sai câu trả lời hoặc sắp xếp sai số câu hỏi. 

Cách tiếp cận tối ưu tận dụng kích thước nhỏ, cố định của không gian đầu vào. Bằng cách lưu trữ tất cả các câu trả lời trong một danh sách, với chỉ mục tương ứng với`n-1`, câu trả lời đúng có thể được tìm ra trong thời gian không đổi. Điều này làm giảm độ phức tạp của mã, tránh phân nhánh nhiều lần và đảm bảo tính chính xác miễn là mảng ánh xạ được xây dựng chính xác. Thông tin chi tiết là không gian đầu vào rất nhỏ và tĩnh, do đó, việc tra cứu trực tiếp vừa nhanh hơn vừa ít xảy ra lỗi hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Câu lệnh có điều kiện | O(1) | O(1) | Đã chấp nhận | 
| Tra cứu mảng | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định danh sách các câu trả lời tương ứng với các câu hỏi từ 1 đến 8. Phần tử đầu tiên của danh sách thể hiện câu trả lời cho câu hỏi 1, phần tử thứ hai tương ứng với câu hỏi 2, v.v. 
2. Đọc số nguyên đầu vào`n`và chuyển đổi nó thành chỉ mục dựa trên 0 bằng cách trừ đi một, vì danh sách Python được lập chỉ mục bằng 0. 
3. Truy xuất câu trả lời từ danh sách tại chỉ mục được tính toán. 
4. Xuất câu trả lời được truy xuất. 

Tại sao nó hoạt động: Điều bất biến là danh sách được sắp xếp chính xác để khớp với số câu hỏi. Bằng cách chuyển đổi đầu vào dựa trên 1 thành chỉ mục dựa trên 0, mọi giá trị hợp lệ`n`ánh xạ trực tiếp đến câu trả lời tương ứng của nó. Không có phép lặp hoặc logic điều kiện nào có thể gây ra lỗi, do đó tính chính xác được đảm bảo miễn là danh sách ánh xạ là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

answers = [
    "walk",
    "no",
    "unrated",
    "no",
    "yes",
    "yes",
    "backwards",
    "7"
]

n = int(input())
print(answers[n - 1])
```Giải pháp bắt đầu bằng cách xác định danh sách`answers`, sắp xếp theo số câu hỏi. đầu vào`n`được đọc và chuyển đổi thành chỉ mục dựa trên 0 để truy cập chính xác vào danh sách. các`print`câu lệnh đưa ra câu trả lời trực tiếp. Sử dụng một danh sách sẽ tránh được nhiều`if`các câu lệnh và loại bỏ các lỗi lập chỉ mục. Phép trừ`n - 1`là rất quan trọng để căn chỉnh đầu vào dựa trên 1 thành lập chỉ mục Python dựa trên 0. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
1
```| Bước | n | n-1 | câu trả lời[n-1] | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 1 | 0 | "đi bộ" | đi bộ | 

Điều này thể hiện sự chuyển đổi từ lập chỉ mục dựa trên 1 sang lập chỉ mục dựa trên 0 và truy xuất trực tiếp từ danh sách. 

**Đầu vào mẫu 2**```
5
```| Bước | n | n-1 | câu trả lời[n-1] | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 5 | 4 | "có" | vâng | 

Điều này xác nhận thuật toán truy cập chính xác vào giữa danh sách mà không gặp lỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Truy cập danh sách theo chỉ mục là thời gian không đổi, độc lập với`n`. | 
| Không gian | O(1) | Chỉ có một danh sách có kích thước cố định gồm 8 phần tử được lưu trữ. | 

Với những hạn chế, giải pháp này là tối ưu. Việc sử dụng bộ nhớ là không đáng kể và quá trình thực thi hoàn tất sau một phần triệu giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    answers = [
        "walk",
        "no",
        "unrated",
        "no",
        "yes",
        "yes",
        "backwards",
        "7"
    ]
    n = int(input())
    return answers[n - 1]

# Provided samples
assert run("1\n") == "walk", "sample 1"
assert run("5\n") == "yes", "sample 2"

# Custom cases
assert run("2\n") == "no", "question 2"
assert run("3\n") == "unrated", "question 3"
assert run("8\n") == "7", "question 8 maximum index"
assert run("7\n") == "backwards", "question 7 string edge"
assert run("4\n") == "no", "question 4 repeated 'no'"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | không | Tra cứu phần tử ở giữa | 
| 3 | chưa được xếp hạng | Đầu ra chuỗi đặc biệt | 
| 8 | 7 | Ranh giới đầu vào tối đa | 
| 7 | ngược | Chuỗi không tầm thường | 
| 4 | không | Xử lý câu trả lời trùng lặp | 

## Vỏ cạnh 

Nếu`n`là 1, thuật toán sẽ chuyển nó thành chỉ số 0 dựa trên 0 và trả về chính xác`"walk"`. Nếu như`n`là 8, chuyển đổi sang chỉ số 7 trả về`"7"`. Không có lỗi ngoài phạm vi nào có thể xảy ra vì sự cố đảm bảo`1 ≤ n ≤ 8`. Câu trả lời trùng lặp như`"no"`xuất hiện nhiều lần, nhưng việc lập chỉ mục đảm bảo chọn đúng lần xuất hiện. Tất cả các kịch bản biên đều được xử lý bởi danh sách ánh xạ trực tiếp.
