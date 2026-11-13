---
title: "CF 1703A - CÓ hay CÓ?"
description: "Chúng tôi được yêu cầu kiểm tra xem chuỗi ba ký tự có đại diện cho từ \"YES\" hay không, bỏ qua chữ cái viết hoa. Mỗi trường hợp kiểm thử cung cấp một chuỗi duy nhất và chúng ta phải xuất ra một chuỗi \"CÓ\" hoặc \"KHÔNG\" thống nhất cho mỗi trường hợp."
date: "2026-06-09T21:35:19+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1703
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 806 (Div. 4)"
rating: 800
weight: 1703
solve_time_s: 110
verified: true
draft: false
---

[CF 1703A - CÓ hay CÓ?](https://codeforces.com/problemset/problem/1703/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thực hiện, dây 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu kiểm tra xem chuỗi ba ký tự có đại diện cho từ "CÓ" hay không, bỏ qua trường hợp chữ cái. Mỗi trường hợp kiểm thử cung cấp một chuỗi duy nhất và chúng ta phải xuất ra một chuỗi "CÓ" hoặc "KHÔNG" thống nhất cho mỗi trường hợp. Điều quan trọng là "yEs", "yes" hoặc "YES" đều là các kết quả khớp hợp lệ, vì vậy giải pháp phải phân biệt chữ hoa chữ thường. 

Đầu vào có thể bao gồm tối đa 1000 trường hợp thử nghiệm. Mỗi chuỗi có chính xác ba ký tự, có nghĩa là chúng ta không xử lý các đầu vào lớn. Bất kỳ thuật toán nào xử lý từng chuỗi trong thời gian không đổi sẽ chạy hiệu quả trong giới hạn thời gian 1 giây. Bộ nhớ không phải là vấn đề đáng lo ngại vì chúng ta chỉ cần lưu trữ chuỗi và một vài biến cho mỗi trường hợp kiểm thử. 

Trường hợp cạnh không rõ ràng sẽ phát sinh nếu ai đó so sánh chuỗi trực tiếp với "CÓ" mà không bình thường hóa trường hợp đó. Ví dụ: đầu vào "yEs" sẽ không đạt được phép kiểm tra đẳng thức cơ bản một cách không chính xác. Tương tự, nếu một chuỗi chứa các chữ cái không phải tiếng Anh hoặc nhiều hơn ba ký tự thì chuỗi đó cũng sẽ không thành công, nhưng những ký tự đó nằm ngoài các ràng buộc đã nêu. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là kiểm tra từng ký tự chuỗi riêng lẻ với ba chữ cái 'Y', 'E', 'S' ở cả dạng chữ hoa và chữ thường. Điều này sẽ liên quan đến sáu so sánh trên mỗi chuỗi. Mặc dù cách này có hiệu quả nhưng nó tẻ nhạt và dễ mắc lỗi vì bạn phải nhớ tất cả các kết hợp chữ hoa chữ thường. 

Một cách tiếp cận rõ ràng hơn sử dụng quan sát rằng Python cung cấp khả năng so sánh không phân biệt chữ hoa chữ thường thông qua`str.upper()`hoặc`str.lower()`. Bằng cách chuyển đổi chuỗi đầu vào thành chữ hoa và so sánh nó với "CÓ", chúng tôi xử lý tất cả các kết hợp chữ cái hợp lệ trong một thao tác. Điều này làm giảm vấn đề xuống còn một so sánh duy nhất cho mỗi trường hợp thử nghiệm, vừa dễ đọc vừa nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra từng trường hợp từng ký tự | O(1) mỗi chuỗi | O(1) | Được chấp nhận, nhưng dài dòng | 
| Chuyển sang chữ hoa và so sánh | O(1) mỗi chuỗi | O(1) | Được chấp nhận, sạch nhất | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Chúng tôi sẽ lặp lại chính xác`t`chuỗi, vì vậy chúng tôi biết trước giới hạn vòng lặp. 
2. Đối với mỗi chuỗi`s`, chuyển nó thành chữ hoa bằng cách sử dụng`s.upper()`. Điều này đảm bảo rằng tất cả các chữ cái viết thường được ánh xạ tới chữ hoa tương đương của chúng. 
3. So sánh chuỗi đã chuyển đổi thành "CÓ". Nếu chúng khớp nhau, xuất ra "CÓ"; nếu không thì xuất ra "NO". 
4. Lặp lại bước 2-3 cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Việc chuyển đổi chuỗi thành chữ hoa đảm bảo rằng tất cả các biến thể của cách viết hoa chữ cái đều ánh xạ sang một dạng chuẩn duy nhất. Việc so sánh dạng chuẩn này với "CÓ" đảm bảo phát hiện chính xác vì thao tác này giữ nguyên thứ tự tương đối của các ký tự. Bất kỳ chuỗi nào khác với "CÓ" về chữ cái hoặc thứ tự sẽ không so sánh được và tạo ra phản hồi "KHÔNG" chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    if s.upper() == "YES":
        print("YES")
    else:
        print("NO")
```Trước tiên, giải pháp sẽ đọc số lượng trường hợp kiểm thử bằng cách sử dụng I/O nhanh. các`.strip()`phương thức này sẽ loại bỏ mọi ký tự dòng mới ở cuối, điều này rất quan trọng vì`input()`bao gồm dòng mới.`s.upper()`chuyển đổi tất cả các chữ cái thành chữ hoa trong một lệnh gọi, đơn giản hóa việc so sánh. Câu lệnh if-else xuất ra chuỗi được yêu cầu dựa trên so sánh. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`YES`| Bước | s | s.upper() | So sánh | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | "CÓ" | "CÓ" | "CÓ" == "CÓ" | CÓ | 

Chuỗi này đã là chữ hoa nên nó khớp ngay lập tức. 

### Ví dụ 2:`yEs`| Bước | s | s.upper() | So sánh | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | "yEs" | "CÓ" | "CÓ" == "CÓ" | CÓ | 

Các chữ cái viết thường được chuyển đổi và dạng chuẩn khớp với "CÓ". 

### Ví dụ 3:`Noo`| Bước | s | s.upper() | So sánh | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | "Không" | "KHÔNG" | "KHÔNG" != "CÓ" | KHÔNG | 

Dạng chuẩn không khớp với "CÓ", vì vậy đầu ra là "KHÔNG". 

Những dấu vết này xác nhận rằng cả biến thể kiểu chữ và chuỗi không khớp đều được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu chuyển đổi và so sánh chuỗi đơn, cả O(1). Đối với t trường hợp thử nghiệm, tổng O(t). | 
| Không gian | O(1) | Chỉ một vài biến cho mỗi trường hợp thử nghiệm được lưu trữ; không có thang đo bộ nhớ bổ sung với t. | 

Với t 1000, giải pháp chạy trong thời gian không đáng kể và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        s = input().strip()
        if s.upper() == "YES":
            print("YES")
        else:
            print("NO")
    
    return output.getvalue().strip()

# Provided samples
assert run("10\nYES\nyES\nyes\nYes\nYeS\nNoo\norZ\nyEz\nYas\nXES\n") == "YES\nYES\nYES\nYES\nYES\nNO\nNO\nNO\nNO\nNO", "Sample 1"

# Custom cases
assert run("3\nyes\nYES\nYEs\n") == "YES\nYES\nYES", "All case variations"
assert run("2\nNOO\nyeS\n") == "NO\nYES", "Mixed match and mismatch"
assert run("1\nabc\n") == "NO", "Non-YES string"
assert run("1\nYES\n") == "YES", "Exact match"
assert run("1\nYeS\n") == "YES", "Mixed case match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "3\nyes\nCÓ\nCÓ\n" | "CÓ\nCÓ\nCÓ" | Tất cả các biến thể viết hoa đều khớp | 
| "2\nNOO\nyeS\n" | "KHÔNG\nCÓ" | Phân biệt chính xác các chuỗi khớp và không khớp | 
| "1\nabc\n" | "KHÔNG" | Các chuỗi không CÓ bị từ chối | 
| "1\nCÓ\n" | "CÓ" | Kết hợp chính xác | 
| "1\nCó\n" | "CÓ" | Phát hiện đúng trường hợp hỗn hợp | 

## Vỏ cạnh 

Thuật toán xử lý tất cả các trường hợp không rõ ràng vì việc chuyển đổi sang chữ hoa sẽ bình thường hóa kiểu chữ cái. Đối với "yE",`s.upper()`tạo ra "CÓ" và so sánh thành công. Đối với đầu vào không khớp như "Noo",`s.upper()`tạo ra "NOO", không so sánh được. Khoảng trắng ở cuối hoặc ở đầu được loại bỏ bằng`.strip()`, do đó không xảy ra phủ định sai từ các ký tự dòng mới. Tất cả các chuỗi ba ký tự đều được đánh giá chính xác trong thời gian không đổi.
