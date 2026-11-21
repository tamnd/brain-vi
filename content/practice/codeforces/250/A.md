---
title: "CF 250A - Công việc giấy tờ"
description: "Polycarpus cần sắp xếp các báo cáo lợi nhuận hàng ngày vào các thư mục, trong đó mỗi thư mục chứa các ngày liên tiếp. Hạn chế chính là một thư mục không được chứa ba ngày trở lên với lợi nhuận âm, bởi vì ông chủ không thể chịu đựng quá hai ngày thua lỗ cho mỗi thư mục."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 250
codeforces_index: "A"
codeforces_contest_name: "CROC-MBTU 2012, Final Round (Online version, Div. 2)"
rating: 1000
weight: 250
solve_time_s: 189
verified: false
draft: false
---

[CF 250A - Công việc giấy](https://codeforces.com/problemset/problem/250/A) 

**Đánh giá:** 1000 
**Thẻ:** tham lam 
**Thời gian giải:** 3 phút 9 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Polycarpus cần sắp xếp các báo cáo lợi nhuận hàng ngày vào các thư mục, trong đó mỗi thư mục chứa các ngày liên tiếp. Hạn chế chính là một thư mục không được chứa ba ngày trở lên với lợi nhuận âm, bởi vì ông chủ không thể chịu đựng quá hai ngày thua lỗ cho mỗi thư mục. Mục đích là giảm thiểu tổng số thư mục và xuất ra cả số lượng thư mục cũng như kích thước của từng thư mục theo trình tự. 

Đầu vào là một số nguyên`n`biểu thị số ngày, theo sau là dãy số nguyên biểu thị lợi nhuận mỗi ngày. Đầu ra là một số nguyên biểu thị số lượng thư mục tối thiểu và một chuỗi hiển thị số ngày trong mỗi thư mục. 

Với`n`lên tới 100, chúng tôi biết thuật toán có thể cung cấp các vòng lặp lồng nhau có kích thước vừa phải, vì O(n²) vẫn khả thi. Tuy nhiên, chúng ta nên hướng tới cách tiếp cận O(n) tuyến tính, bởi vì chúng ta chỉ cần quét mảng một cách tuần tự và duy trì số ngày âm đơn giản. 

Trường hợp phức tạp phát sinh khi các ngày âm xuất hiện liên tiếp. Ví dụ, trong trình tự`[1, -1, -2, -3, 4]`, chúng tôi không thể gộp cả ba ngày âm vào một thư mục. Một kẻ tham lam ngây thơ chỉ nhìn vào kích thước thư mục tích lũy mà không theo dõi số lượng âm có thể đặt nhầm cả ba thư mục lại với nhau, tạo ra một thư mục không hợp lệ. Một trường hợp đặc biệt khác là các chuỗi có ít hơn ba ngày âm; tất cả chúng đều có thể vừa với một thư mục bất kể vị trí của chúng. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ tạo ra tất cả các phân vùng có thể có của mảng thành các phân đoạn liên tiếp và kiểm tra tính hợp lệ của từng phân vùng. Mỗi phân đoạn hợp lệ nếu nó chứa tối đa hai ngày âm. Với`n`lên tới 100, số lượng phân vùng tăng theo cấp số nhân, khiến điều này không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải xem xét việc phân vùng phức tạp. Hạn chế duy nhất là một thư mục không thể có ba ngày âm. Chúng ta có thể quét mảng từ trái sang phải, theo dõi xem thư mục hiện tại có bao nhiêu ngày âm. Bất cứ khi nào việc thêm vào ngày hôm sau sẽ dẫn đến ba kết quả âm, chúng tôi sẽ bắt đầu một thư mục mới. Cách tiếp cận tham lam này có tác dụng vì ràng buộc chỉ phụ thuộc vào số ngày âm chứ không phụ thuộc vào vị trí hoặc tổng tuyệt đối. Bằng cách nhóm nhiều ngày liên tiếp nhất có thể trong khi vẫn giữ số âm dưới ba, chúng tôi đảm bảo số lượng thư mục tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Quét tham lam bằng số âm | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách trống`folders`để lưu trữ kích thước của mỗi thư mục. Bộ`current_folder_size`Và`neg_count`về không. 
2. Lặp lại lợi nhuận mỗi ngày`a[i]`từ đầu tiên đến cuối cùng. 
3. Tăng`current_folder_size`bằng 1 để tính ngày hiện tại. 
4. Nếu`a[i]`là âm, tăng`neg_count`. 
5. Sau khi thêm ngày, hãy kiểm tra xem`neg_count`đạt 3. Nếu có thì thư mục hiện tại không thể chứa ngày này. Chúng tôi hoàn thiện thư mục trước đó với kích thước`current_folder_size - 1`và bắt đầu một thư mục mới với ngày hiện tại. Cài lại`neg_count`thành 1 nếu ngày hiện tại âm, ngược lại là 0 và đặt`current_folder_size`đến 1. 
6. Sau vòng lặp, hãy thêm kích thước của thư mục cuối cùng vào`folders`. 
7. In số lượng thư mục và kích thước thư mục theo thứ tự. 

Thuật toán hoạt động vì chúng tôi duy trì một bất biến rằng không có thư mục nào chứa quá hai ngày âm. Bằng cách bắt đầu một thư mục mới chính xác khi số âm đạt tới ba, chúng tôi đảm bảo cả tính hợp lệ và mức tối thiểu: không thư mục nào có thể được mở rộng mà không vi phạm ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

folders = []
current_folder_size = 0
neg_count = 0

for x in a:
    if x < 0:
        neg_count += 1
    current_folder_size += 1
    if neg_count == 3:
        folders.append(current_folder_size - 1)
        current_folder_size = 1
        neg_count = 1 if x < 0 else 0

folders.append(current_folder_size)

print(len(folders))
print(' '.join(map(str, folders)))
```Chúng tôi lặp lại mỗi ngày, cập nhật kích thước thư mục và số lượng âm. Khi số âm chạm tới ba, chúng tôi sẽ đóng thư mục trước ngày hiện tại, bắt đầu một thư mục mới và đặt lại bộ đếm một cách thích hợp. Cuối cùng, chúng tôi nối thêm thư mục cuối cùng. Có thể tránh từng lỗi một bằng cách điều chỉnh kích thước thư mục trước khi đặt lại. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
11
1 2 3 -4 -5 -6 5 -5 -6 -7 6
```| Ngày | Lợi nhuận | phủ định | current_folder_size | Hành động | thư mục | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | tiếp tục | [] | 
| 2 | 2 | 0 | 2 | tiếp tục | [] | 
| 3 | 3 | 0 | 3 | tiếp tục | [] | 
| 4 | -4 | 1 | 4 | tiếp tục | [] | 
| 5 | -5 | 2 | 5 | tiếp tục | [] | 
| 6 | -6 | 3 | 6 | neg_count=3 → chia | [5] | 
| 6 | -6 | 1 | 1 | bắt đầu mới | [5] | 
| 7 | 5 | 1 | 2 | tiếp tục | [5] | 
| 8 | -5 | 2 | 3 | tiếp tục | [5] | 
| 9 | -6 | 3 | 4 | chia | [5,3] | 
| 9 | -6 | 1 | 1 | bắt đầu mới | [5,3] | 
| 10 | -7 | 2 | 2 | tiếp tục | [5,3] | 
| 11 | 6 | 2 | 3 | kết thúc | [5,3,3] | 

Bảng xác nhận rằng số âm không bao giờ vượt quá 2 trong bất kỳ thư mục nào và bao gồm tất cả các ngày. 

Đầu vào mẫu 2:```
5
1 2 3 4 5
```Tất cả lợi nhuận đều dương nên thư mục không bao giờ bị chia tách. Đầu ra là`[5]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ngày được xử lý đúng một lần, hoạt động liên tục mỗi ngày | 
| Không gian | O(n) | các`folders`liệt kê các cửa hàng tối đa n mục trong trường hợp xấu nhất | 

Với n 100, thuật toán này thực thi đơn giản trong vòng 2 giây và mức sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    folders = []
    current_folder_size = 0
    neg_count = 0
    for x in a:
        if x < 0:
            neg_count += 1
        current_folder_size += 1
        if neg_count == 3:
            folders.append(current_folder_size - 1)
            current_folder_size = 1
            neg_count = 1 if x < 0 else 0
    folders.append(current_folder_size)
    return f"{len(folders)}\n{' '.join(map(str, folders))}"

# Provided samples
assert run("11\n1 2 3 -4 -5 -6 5 -5 -6 -7 6\n") == "3\n5 3 3", "sample 1"
assert run("5\n1 2 3 4 5\n") == "1\n5", "sample 2"

# Custom cases
assert run("3\n-1 -2 -3\n") == "2\n2 1", "all negatives split"
assert run("1\n0\n") == "1\n1", "single day, zero"
assert run("6\n-1 2 -3 4 -5 6\n") == "3\n3 2 1", "interleaved negatives"
assert run("100\n" + " ".join(["1"]*100) + "\n") == "1\n100", "all positives max size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 tiêu cực | 2 thư mục | tách khi số âm vượt quá 2 | 
| 1 ngày không | 1 thư mục | đầu vào kích thước tối thiểu | 
| âm bản xen kẽ | 3 thư mục | phân chia chính xác với những khoảng trống | 
| tất cả các chiều dài tích cực 100 | 1 thư mục | kích thước tối đa, không bị chia tách | 

## Vỏ cạnh 

cho`[ -1, -2, -3 ]`, thuật toán theo dõi số âm. Sau ngày thứ hai,`neg_count = 2`, ngày hôm sau mang đến`neg_count = 3`, gây ra sự chia rẽ. Kích thước thư mục`[2,1]`thỏa mãn ràng buộc. 

Vì`[1, 2, 3, 4, 5]`, tất cả đều tích cực,`neg_count`không bao giờ đạt tới 3, vì vậy chỉ có một thư mục có kích thước 5 được tạo. 

Trong một ngày`[0]`,`current_folder_size = 1`,`neg_count = 0`và phần bổ sung cuối cùng mang lại`[1]`, xử lý chính xác đầu vào tối thiểu. 

Thuật toán quản lý một cách có hệ thống số âm và phân chia thư mục, bao gồm tất cả các trường hợp có thể xảy ra mà không cần mã trường hợp đặc biệt.
