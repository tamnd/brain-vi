---
title: "CF 431A - Vuông Màu Đen"
description: "Trong "Hình vuông màu đen", Ban giám khảo phải nhấn vào một trong bốn dải dọc bất cứ khi nào hình vuông màu đen xuất hiện. Mỗi dải có một mức tiêu hao năng lượng cố định, được đo bằng calo, để ép nó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 431
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 247 (Div. 2)"
rating: 800
weight: 431
solve_time_s: 70
verified: true
draft: false
---

[CF 431A - Hình vuông màu đen](https://codeforces.com/problemset/problem/431/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trong "Hình vuông đen", Ban giám khảo phải nhấn vào một trong bốn dải dọc bất cứ khi nào hình vuông màu đen xuất hiện. Mỗi dải có một mức tiêu hao năng lượng cố định, được đo bằng calo, để ép nó. Chúng ta được cung cấp bốn số nguyên biểu thị lượng calo tiêu thụ khi nhấn dải đầu tiên đến dải thứ tư. Tiến trình của trò chơi được mô tả bằng một chuỗi trong đó mỗi ký tự là một chữ số từ "1" đến "4" cho biết dải nào có hình vuông ở giây đó. Nhiệm vụ là tính toán tổng lượng calo Ban giám khảo tiêu tốn bằng cách làm theo trình tự này và nhấn các dải tương ứng. 

Dữ liệu đầu vào đảm bảo tối đa 105 giây trò chơi, vì vậy giải pháp của chúng tôi phải xử lý từng ký tự một cách hiệu quả, lý tưởng nhất là theo thời gian tuyến tính. Mỗi chi phí calo có thể bằng 0, nghĩa là nhấn một dải có thể không tiêu tốn năng lượng. Các trường hợp cạnh bao gồm chuỗi hoàn toàn là một loại dải, các dải xen kẽ hoặc có độ dài tối đa là 105. Một giải pháp đơn giản liên tục tìm kiếm hoặc thực hiện các thao tác phức tạp trên mỗi ký tự có thể vượt quá giới hạn thời gian. 

Một trường hợp phức tạp phát sinh khi tất cả chi phí calo bằng không. Ví dụ: nếu đầu vào là`0 0 0 0`Và`s = "12341"`, đầu ra đúng là`0`, nhưng một cách tiếp cận bất cẩn, chẳng hạn như cho rằng mỗi lần nhấn đóng góp ít nhất một calo sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lặp lại chuỗi và đối với mỗi ký tự, ánh xạ chuỗi đó tới chi phí calo tương ứng và thêm nó vào tổng số đang chạy. Điều này hiệu quả vì mỗi giây đều độc lập; chúng tôi chỉ cần biết chi phí cho dải cụ thể đó. Brute-force đã tối ưu về mặt kỹ thuật vì mọi ký tự đều phải được kiểm tra, đưa ra độ phức tạp trong trường hợp xấu nhất tỷ lệ thuận với độ dài chuỗi. 

Quan sát quan trọng xác nhận phương pháp này là đủ là không có sự tương tác giữa các dải; calo là chất phụ gia. Không cần tối ưu hóa hoặc cấu trúc dữ liệu phức tạp vì mỗi lần tra cứu là một hoạt động liên tục. Bằng cách ánh xạ trực tiếp ký tự "1"-"4" tới chỉ số tương ứng trong mảng calo, chúng ta có thể tích lũy tổng lượng calo một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên biểu thị lượng calo từ dải đầu tiên đến dải thứ tư. Lưu trữ chúng trong một danh sách`calories`để lập chỉ mục trực tiếp. 
2. Đọc chuỗi`s`đại diện cho chuỗi các ô vuông màu đen. 
3. Khởi tạo một biến`total`về 0 để tích lũy lượng calo. 
4. Lặp lại từng ký tự`c`trong chuỗi`s`. Chuyển thành`c`thành một số nguyên và trừ đi một để khớp với việc lập chỉ mục dựa trên số 0, sau đó truy cập`calories`tại chỉ mục đó và thêm giá trị vào`total`. 
5. Sau khi xử lý tất cả các ký tự, in giá trị của`total`. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`total`luôn bằng tổng lượng calo của tất cả các dải đã xử lý. Vì mỗi ký tự được xử lý chính xác một lần và được ánh xạ trực tiếp tới chi phí của ký tự đó nên không có lượng calo nào bị bỏ sót hoặc bị tính gấp đôi. trận chung kết`total`do đó là tổng của tất cả các giây, thỏa mãn yêu cầu bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# read calorie costs for each strip
calories = list(map(int, input().split()))
s = input().strip()

total = 0
for c in s:
    total += calories[int(c) - 1]

print(total)
```Dòng đầu tiên đọc bốn giá trị calo và lưu chúng vào danh sách. Trừ một từ`int(c)`đảm bảo rằng ánh xạ từ "1"-"4" tới các chỉ mục danh sách dựa trên số 0 là chính xác. Vòng lặp trực tiếp thêm giá trị calo thích hợp vào`total`không có bất kỳ kiểm tra có điều kiện hoặc phân nhánh nào. Điều này tránh được từng lỗi một. Chuỗi được loại bỏ khoảng trắng để đảm bảo chúng ta không vô tình xử lý ký tự dòng mới. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
1 2 3 4
123214
```| Bước | Nhân vật | Chỉ mục | Lượng calo bổ sung | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | '1' | 0 | 1 | 1 | 
| 2 | '2' | 1 | 2 | 3 | 
| 3 | '3' | 2 | 3 | 6 | 
| 4 | '2' | 1 | 2 | 8 | 
| 5 | '1' | 0 | 1 | 9 | 
| 6 | '4' | 3 | 4 | 13 | 

Dấu vết này xác nhận tổng lượng calo tích lũy chính xác cho mỗi lần nhấn. 

**Mẫu 2** 

đầu vào:```
5 5 5 5
4441
```| Bước | Nhân vật | Chỉ mục | Lượng calo bổ sung | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | '4' | 3 | 5 | 5 | 
| 2 | '4' | 3 | 5 | 10 | 
| 3 | '4' | 3 | 5 | 15 | 
| 4 | '1' | 0 | 5 | 20 | 

Điều này thể hiện việc xử lý các lần nhấn lặp đi lặp lại trên cùng một dải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự trong chuỗi được xử lý một lần; chuyển đổi số nguyên và tra cứu danh sách là O(1). | 
| Không gian | O(1) | Chỉ sử dụng một danh sách nhỏ có kích thước cố định về lượng calo và tổng lượng calo đang chạy. | 

Với n ≤ 105, phương pháp tuyến tính này thực thi thoải mái trong vòng 1 giây, ngay cả trên Python và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    calories = list(map(int, input().split()))
    s = input().strip()
    total = 0
    for c in s:
        total += calories[int(c) - 1]
    return str(total)

# provided sample
assert run("1 2 3 4\n123214\n") == "13", "sample 1"

# custom cases
assert run("0 0 0 0\n12341\n") == "0", "all zero calories"
assert run("1 1 1 1\n11111\n") == "5", "all same strip"
assert run("1 2 3 4\n4321\n") == "10", "descending order"
assert run("10 20 30 40\n444444\n") == "240", "repeated max strip"
assert run("5 5 5 5\n1\n") == "5", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 0 / 12341 | 0 | Xử lý dải không calo | 
| 1 1 1 1 / 11111 | 5 | Máy ép lặp đi lặp lại trên cùng một dải | 
| 1 2 3 4 / 4321 | 10 | Ánh xạ chỉ mục chính xác cho chuỗi giảm dần | 
| 10 20 30 40 / 444444 | 240 | Nhấn nhiều lần vào dải cuối cùng | 
| 5 5 5 5/1 | 5 | Kích thước đầu vào tối thiểu | 

## Vỏ cạnh 

Khi tất cả các giá trị calo bằng 0,`total`vẫn bằng 0 bất kể thứ tự. Ví dụ, đầu vào`0 0 0 0\n12341`sản xuất`0`. Thuật toán xử lý chính xác một chuỗi có độ dài bằng một, như trong`5 5 5 5\n1`, trong đó nó chỉ cộng thêm chi phí của dải đầu tiên một lần. Đối với các lần nhấn lặp lại trên một dải, chẳng hạn như`11111`với tất cả lượng calo 1, thuật toán sẽ tích lũy tổng một cách chính xác, xác nhận giá trị bất biến trên nhiều ký tự giống hệt nhau.
