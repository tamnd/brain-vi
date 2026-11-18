---
title: "CF 2050A - Ngắt dòng"
description: "Chúng ta được cung cấp một danh sách các từ và một dải có độ dài cố định có thể chứa một số ký tự nhất định. Nhiệm vụ là quyết định xem chúng ta có thể đặt liên tiếp bao nhiêu từ trên dải đầu tiên này mà không vượt quá độ dài của nó, trong khi những từ còn lại nằm trên dải thứ hai có…"
date: "2026-06-08T08:46:56+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2050
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 991 (Div. 3)"
rating: 800
weight: 2050
solve_time_s: 95
verified: true
draft: false
---

[CF 2050A - Ngắt dòng](https://codeforces.com/problemset/problem/2050/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các từ và một dải có độ dài cố định có thể chứa một số ký tự nhất định. Nhiệm vụ là quyết định xem chúng ta có thể đặt bao nhiêu từ liên tiếp trên dải đầu tiên này mà không vượt quá độ dài của nó, trong khi những từ còn lại nằm trên dải thứ hai có không gian vô hạn. Các từ phải viết liền kề nhau, không có dấu cách, không được tách chữ giữa các dải. Đầu ra là số từ tối đa phù hợp với dải đầu tiên. 

Nhìn vào các ràng buộc, chúng ta thấy rằng mỗi trường hợp thử nghiệm có tối đa 50 từ, mỗi từ dài tối đa 10 ký tự và dải có thể chứa tối đa 500 ký tự. Giá trị này đủ nhỏ để xem xét một giải pháp lặp đơn giản trong đó chúng tôi tính tổng từng độ dài từ, vì ngay cả trong trường hợp xấu nhất, chúng tôi sẽ thực hiện tối đa 50 phép cộng cho mỗi trường hợp thử nghiệm và có tối đa 1000 trường hợp thử nghiệm. Vì vậy, một giải pháp đơn giản sẽ đủ nhanh. 

Các trường hợp cần xem xét bao gồm các tình huống trong đó từ đầu tiên dài hơn dải, nghĩa là không có từ nào có thể vừa. Một trường hợp tinh tế khác là khi tổng độ dài của tất cả các từ bằng chính xác độ dài dải. Việc triển khai bất cẩn có thể dừng lại trước khi thêm từ cuối cùng, tạo ra từng lỗi một. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại các từ, giữ tổng độ dài của chúng. Đối với mỗi từ, chúng tôi kiểm tra xem việc thêm nó có vượt quá độ dài dải hay không. Nếu đúng như vậy, chúng tôi dừng lại và trả về số từ đã thêm cho đến nay. Cách tiếp cận này đúng vì nó tôn trọng thứ tự của các từ và dừng chính xác khi chúng ta không thể khớp từ tiếp theo. Độ phức tạp của nó là O(n) cho mỗi trường hợp thử nghiệm hoặc nhiều nhất là 50 thao tác, có thể chấp nhận được đối với các ràng buộc của chúng tôi. 

Cách tiếp cận tối ưu thực sự giống nhau trong trường hợp này vì vấn đề nhỏ và không cần phương pháp nào nhanh hơn. Thông tin chi tiết quan trọng là nhận ra rằng chúng ta chỉ cần chuyển qua một lần duy nhất các từ có tổng số đang chạy. Không cần lập trình động hoặc tìm kiếm nhị phân vì kích thước bài toán nhỏ và việc tính tổng rất đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`Và`m`, đại diện cho số lượng từ và độ dài dải. 
3. Khởi tạo`current_length`đến 0 và`count`đến 0. 
4. Lặp lại từng từ trong danh sách: 

1. Đo độ dài của từ. 
2. Kiểm tra xem việc thêm độ dài này vào`current_length`vượt quá`m`. 
3. Nếu không, hãy thêm độ dài vào`current_length`và tăng dần`count`. 
4. Nếu đúng như vậy, hãy ngắt vòng lặp vì không thể thêm từ nào nữa. 
5. Sau khi lặp lại các từ,`count`chứa số lượng từ tối đa phù hợp. 
6. In kết quả của test case này. 

Tại sao nó hoạt động: Tổng số hoạt động đảm bảo chúng tôi không bao giờ vượt quá chiều dài dải. Vì chúng tôi kiểm tra từng từ theo thứ tự nên chúng tôi đảm bảo rằng tất cả các từ được chọn đều liên tiếp ngay từ đầu. Tính bất biến đó là`current_length`luôn bằng tổng độ dài của các từ hiện được đếm và không bao giờ vượt quá`m`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    words = [input().strip() for _ in range(n)]
    current_length = 0
    count = 0
    for word in words:
        length = len(word)
        if current_length + length <= m:
            current_length += length
            count += 1
        else:
            break
    print(count)
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó sẽ đọc số lượng từ và độ dài dải, sau đó đọc chính các từ đó. Chúng tôi duy trì tổng độ dài từ và bộ đếm. điều kiện`current_length + length <= m`đảm bảo rằng chúng tôi không vượt quá chiều dài dải. Vòng lặp sẽ ngắt ngay khi một từ tràn ra khỏi dải, đảm bảo tính chính xác và tránh các lỗi sai sót một. sử dụng`input().strip()`loại bỏ các ký tự dòng mới khỏi các từ. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
3 1
a
b
c
```| Lời | Độ dài hiện tại | Đếm | Hành động | 
| --- | --- | --- | --- | 
| một | 0 + 1 = 1 | 1 | phù hợp, tiếp tục | 
| b | 1 + 1 = 2 | - | vượt quá 1, dừng lại | 

Đầu ra:`1`Từ đầu tiên vừa vặn, nhưng việc thêm từ thứ hai vượt quá độ dài dải nên chỉ tính một từ. 

**Đầu vào mẫu 2:**```
2 9
alpha
beta
```| Lời | Độ dài hiện tại | Đếm | Hành động | 
| --- | --- | --- | --- | 
| alpha | 0 + 5 = 5 | 1 | phù hợp, tiếp tục | 
| phiên bản beta | 5 + 4 = 9 | 2 | phù hợp chính xác, tiếp tục | 

Đầu ra:`2`Tất cả các từ đều khớp chính xác với dải, thể hiện trường hợp cạnh có tổng chiều dài bằng`m`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * t) | Mỗi trường hợp thử nghiệm lặp lại trên tất cả`n`tổng số từ, t trường hợp kiểm thử | 
| Không gian | O(n) | Lưu trữ từ cho từng trường hợp thử nghiệm | 

Với các ràng buộc (n ≤ 50, t ≤ 1000), giải pháp thực hiện tối đa 50.000 thao tác, trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ ở mức tối thiểu, thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        words = [input().strip() for _ in range(n)]
        current_length = 0
        count = 0
        for word in words:
            length = len(word)
            if current_length + length <= m:
                current_length += length
                count += 1
            else:
                break
        print(count)
    return output.getvalue().strip()

# Provided samples
assert run("5\n3 1\na\nb\nc\n2 9\nalpha\nbeta\n4 12\nhello\nworld\nand\ncodeforces\n3 2\nab\nc\nd\n3 2\nabc\nab\na\n") == "1\n2\n2\n1\n0", "sample 1"

# Custom cases
assert run("1\n1 1\na\n") == "1", "single word fits"
assert run("1\n1 1\nab\n") == "0", "single word too long"
assert run("1\n3 5\na\nb\nc\n") == "3", "all words fit"
assert run("1\n3 2\na\nb\nc\n") == "2", "exact fit before last word"
assert run("1\n2 5\nabc\nde\n") == "2", "sum equals m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 từ phù hợp | 1 | Đầu vào tối thiểu nơi từ phù hợp | 
| 1 từ dài quá | 0 | Đầu vào tối thiểu nơi từ không thể vừa | 
| Nhiều từ nhỏ | 3 | Tất cả các từ phù hợp một cách dễ dàng | 
| Nhiều từ, cuối cùng quá lớn | 2 | Kiểm tra dừng ở từ cuối cùng | 
| Tổng bằng dải | 2 | Vỏ cạnh vừa vặn chính xác | 

## Vỏ cạnh 

Nếu từ đầu tiên vượt quá`m`, thuật toán trả về`0`. Ví dụ, đầu vào`1 2\nabc`sản xuất`0`bởi vì`len("abc") = 3 > 2`. Nếu nhiều từ tính tổng chính xác bằng`m`, tất cả đều được tính chính xác. Tổng số tiền đảm bảo chúng tôi không bao giờ vượt quá`m`và câu lệnh break sẽ ngăn cản việc đếm quá mức. Thuật toán xử lý các từ đơn và các dải trống một cách duyên dáng.
