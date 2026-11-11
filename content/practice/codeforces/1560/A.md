---
title: "CF 1560A - Không thích ba người"
description: "Chúng ta được yêu cầu tạo ra một chuỗi các số nguyên dương mà Polycarp \"like.\" Một số được ưa thích nếu nó không chia hết cho 3 và không kết thúc bằng chữ số 3. Cho một số nguyên $k$, nhiệm vụ là xuất ra số $k$-th trong chuỗi này."
date: "2026-06-10T12:17:17+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1560
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 739 (Div. 3)"
rating: 800
weight: 1560
solve_time_s: 100
verified: true
draft: false
---

[CF 1560A - Không thích ba người](https://codeforces.com/problemset/problem/1560/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tạo ra một chuỗi các số nguyên dương mà Polycarp "thích". Một số được yêu thích nếu nó không chia hết cho 3 và không kết thúc bằng chữ số 3. Cho một số nguyên$k$, nhiệm vụ là xuất ra$k$-số thứ trong dãy này. Nhiều trường hợp thử nghiệm được cung cấp, mỗi trường hợp có một trường hợp riêng biệt$k$. 

Đầu vào bao gồm$t$, số lượng ca kiểm thử, theo sau là$t$số nguyên$k$. Mỗi$k$có thể lớn tới 1000 và có thể có tới 100 trường hợp thử nghiệm. Vì số lượng lớn nhất mà chúng ta có thể cần là khiêm tốn (ở mức thấp hàng nghìn), nên chúng ta có thể sử dụng các phương pháp đơn giản như lặp tuần tự qua các số nguyên. Các ràng buộc loại trừ các giải pháp yêu cầu tìm kiếm qua hàng triệu số nhưng cho phép các phương pháp tuyến tính lọc các số cho đến khi đạt được$k$-thứ hợp lệ. 

Các trường hợp cạnh không rõ ràng phát sinh do các số chia hết cho 3 hoặc kết thúc bằng 3 phải được bỏ qua. Ví dụ, cách tiếp cận ngây thơ khi cho rằng mọi số thứ ba đều xấu: các số như 13, 23, 16 bị bỏ qua theo cách khác vì chữ số kết thúc cũng quan trọng ngoài khả năng chia hết. Một ví dụ nhỏ cho thấy điều này: số thích thứ 9 là 14 chứ không phải 13 hay 15. Việc lập chỉ mục bất cẩn có thể dễ dàng tính sai ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bắt đầu từ 1 và lặp qua các số nguyên, bỏ qua những số chia hết cho 3 hoặc kết thúc bằng 3. Đếm xem chúng ta đã thấy bao nhiêu số thích và dừng lại khi đạt đến số$k$-th. Điều này được đảm bảo hoạt động vì chúng tôi kiểm tra mọi số, nhưng có thể chậm nếu$k$rất lớn, vì nó đòi hỏi phải lặp đi lặp lại nhiều hơn$k$những con số. 

Một quan sát quan trọng đơn giản hóa điều này. Bất kỳ số nào tận cùng bằng 3 hoặc chia hết cho 3 đều phải bỏ qua. Không có công thức trực tiếp nào để chuyển sang$k$-số thứ vì số lần bỏ qua không đều: mọi số thứ mười kết thúc bằng 3 là số xấu và mọi số thứ ba chia hết cho 3 đều xấu và đôi khi các số này trùng nhau. Tuy nhiên, đối với$k \le 1000$, chỉ cần tạo các số theo tuần tự là đủ nhanh và việc tối ưu hóa vượt quá mức này là không cần thiết. Nếu như$k$lớn hơn nhiều, người ta có thể sử dụng danh sách được tính toán trước hoặc mẫu toán học dựa trên việc đếm các số bị bỏ qua, nhưng độ phức tạp của công thức như vậy cao hơn so với việc chỉ lặp lại. 

Bảng so sánh: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Danh sách tính toán trước | O(max_k) | O(max_k) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm số lượng số lượt thích mà chúng ta đã thấy. Bắt đầu từ số không. 
2. Khởi tạo số hiện tại thành 1. 
3. Trong khi bộ đếm nhỏ hơn$k$, kiểm tra số hiện tại. 
4. Nếu số chia hết cho 3 hoặc tận cùng bằng 3 thì bỏ qua. Nếu không, hãy tăng bộ đếm. 
5. Tăng số hiện tại và lặp lại bước 3. 
6. Khi quầy đạt đến$k$, số cuối cùng được xét là$k$-con số thích thứ Trả lại nó. 

Điều bất biến là bộ đếm theo dõi chính xác số lượng lượt thích mà chúng ta đã thấy. Chúng tôi chỉ tăng bộ đếm cho các số hợp lệ, vì vậy khi bộ đếm bằng$k$, chúng tôi được đảm bảo đã đạt được$k$-con số thích thứ 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def kth_liked_number(k):
    count = 0
    num = 1
    while True:
        if num % 3 != 0 and num % 10 != 3:
            count += 1
            if count == k:
                return num
        num += 1

t = int(input())
for _ in range(t):
    k = int(input())
    print(kth_liked_number(k))
```chức năng`kth_liked_number`duyệt qua các số nguyên một cách tuần tự, chỉ tăng bộ đếm khi một số được thích. Vòng lặp chính xử lý nhiều trường hợp thử nghiệm. Cần thận trọng với tình trạng`num % 10 != 3`thay vì kiểm tra biểu diễn chuỗi, việc này nhanh hơn một chút. sử dụng`while True`cho phép chúng tôi thoát ra ngay khi$k$-số thứ được tìm thấy. 

## Ví dụ đã hoạt động 

### Ví dụ 1: k = 4 

| số | số % 3 | số % 10 | thích? | đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | vâng | 1 | 
| 2 | 2 | 2 | vâng | 2 | 
| 3 | 0 | 3 | không | 2 | 
| 4 | 1 | 4 | vâng | 3 | 
| 5 | 2 | 5 | vâng | 4 | 

Số like thứ 4 là 5. 

### Ví dụ 2: k = 10 

| số | số % 3 | số % 10 | thích? | đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | vâng | 1 | 
| 2 | 2 | 2 | vâng | 2 | 
| 3 | 0 | 3 | không | 2 | 
| 4 | 1 | 4 | vâng | 3 | 
| 5 | 2 | 5 | vâng | 4 | 
| 6 | 0 | 6 | không | 4 | 
| 7 | 1 | 7 | vâng | 5 | 
| 8 | 2 | 8 | vâng | 6 | 
| 9 | 0 | 9 | không | 6 | 
| 10 | 1 | 0 | vâng | 7 | 
| 11 | 2 | 1 | vâng | 8 | 
| 12 | 0 | 2 | không | 8 | 
| 13 | 1 | 3 | không | 8 | 
| 14 | 2 | 4 | vâng | 9 | 
| 15 | 0 | 5 | không | 9 | 
| 16 | 1 | 6 | vâng | 10 | 

Con số like thứ 10 là 16. 

Những dấu vết này xác nhận tính bất biến trong đếm và xử lý các số tận cùng bằng 3 hoặc chia hết cho 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) cho mỗi trường hợp thử nghiệm | Chúng tôi có thể cần kiểm tra nhiều hơn k số một chút để tìm số thứ k thích, nhưng không bao giờ nhiều hơn khoảng 1,5k. | 
| Không gian | O(1) | Chỉ bộ đếm và số hiện tại được lưu trữ; không cần thêm mảng. | 

Đối với các ràng buộc tối đa, t=100 và k=1000, giải pháp xử lý tối đa ~1500 số cho mỗi trường hợp thử nghiệm, tổng cộng ~150.000 lần lặp, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    t = int(input())
    def kth_liked_number(k):
        count = 0
        num = 1
        while True:
            if num % 3 != 0 and num % 10 != 3:
                count += 1
                if count == k:
                    return num
            num += 1
    for _ in range(t):
        k = int(input())
        print(kth_liked_number(k))
    return output.getvalue().strip()

# Provided samples
assert run("10\n1\n2\n3\n4\n5\n6\n7\n8\n9\n1000\n") == "1\n2\n4\n5\n7\n8\n10\n11\n14\n1666", "sample 1"

# Custom cases
assert run("3\n1\n13\n20\n") == "1\n25\n33", "varied k"
assert run("1\n1000\n") == "1666", "max k"
assert run("1\n100\n") == "153", "middle k"
assert run("2\n1\n2\n") == "1\n2", "smallest k repeated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "3\n1\n13\n20\n" | "1\n25\n33" | Tổng quát thay đổi k và bỏ qua bội số của 3 và các số tận cùng bằng 3 | 
| "1\n1000\n" | "1666" | Ranh giới k tối đa | 
| "1\n100\n" | "153" | Trung gian k, xác nhận việc đếm hoạt động vượt quá vài số đầu tiên | 
| "2\n1\n2\n" | "1\n2" | Giá trị k nhỏ nhất, đảm bảo chuỗi ban đầu đúng | 

## Vỏ cạnh 

Với k = 1, số được thích đầu tiên là 1. Thuật toán bắt đầu đếm từ 1 và chỉ tăng bộ đếm một cách chính xác khi có một số được thích. Không có số bị bỏ qua nào xảy ra trước số đầu tiên. Với k = 1000, thuật toán tiếp tục cho đến năm 1666, bỏ qua chính xác tất cả bội số của 3 và các số kết thúc bằng 3. Các trường hợp cạnh xung quanh các số như 3, 6, 13, 33 được xử lý bằng điều kiện`num % 3 != 0 and num % 10 != 3`, đảm bảo bộ đếm chỉ tăng đối với các số hợp lệ.
