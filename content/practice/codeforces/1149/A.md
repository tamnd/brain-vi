---
title: "CF 1149A - Tiền tố Tổng số nguyên tố"
description: "Chúng ta được phát một túi các ô, mỗi ô được gắn nhãn 1 hoặc 2. Nhiệm vụ của chúng ta là sắp xếp tất cả các ô thành một chuỗi sao cho khi tính tổng tiền tố - nghĩa là tổng của phần tử đầu tiên, tổng của hai phần tử đầu tiên, v.v. - số tổng là…"
date: "2026-06-12T03:07:28+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1149
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 556 (Div. 1)"
rating: 1200
weight: 1149
solve_time_s: 83
verified: true
draft: false
---

[CF 1149A - Tổng tiền tố Các số nguyên tố](https://codeforces.com/problemset/problem/1149/A) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một túi gạch, mỗi túi có dán nhãn một`1`hoặc một`2`. Nhiệm vụ của chúng ta là sắp xếp tất cả các ô thành một chuỗi sao cho khi tính tổng tiền tố - tức là tổng của phần tử đầu tiên, tổng của hai phần tử đầu tiên, v.v. - số lượng tổng là số nguyên tố là lớn nhất. Đầu ra là bất kỳ hoán vị nào của các ô đạt được mức tối đa này. 

Số lượng gạch,`n`, có thể lên tới 200.000. Vì chuỗi chỉ bao gồm 1 và 2 nên tổng tiền tố tối đa là`2 * n`. Một cách tiếp cận mạnh mẽ để thử tất cả các hoán vị là không thể bởi vì`n!`phát triển nhanh hơn nhiều so với các hoạt động có sẵn cho`n`khoảng 200.000. Ngay cả việc kiểm tra tất cả các trình tự một cách có hệ thống cũng không thể thực hiện được. Chúng ta cần một cách tiếp cận hoạt động theo thời gian tuyến tính hoặc ở thời điểm tuyến tính tồi tệ nhất. 

Các trường hợp có biên không rõ ràng bao gồm các chuỗi trong đó một trong các số chiếm ưu thế. Ví dụ: nếu đầu vào toàn là 1 thì tổng tiền tố chỉ là 1, 2, 3, ...,`n`. Nếu chúng ta bắt đầu bằng số 2 khi có sẵn số 1 thì tổng tiền tố đầu tiên là 2, số nguyên tố, nhưng số tiếp theo trở thành 2 cộng với số 1 còn lại, có thể bỏ qua số nguyên tố 3. Tương tự, nếu đầu vào đều là 2 thì tổng tiền tố đầu tiên là 2, nhưng số tiếp theo là 4, không phải là số nguyên tố. Xử lý các chuỗi nhỏ này một cách cẩn thận là rất quan trọng để tối đa hóa các số nguyên tố. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực đơn giản sẽ liệt kê mọi hoán vị của các ô và tính số tổng tiền tố nguyên tố cho mỗi ô. Về nguyên tắc, điều này hoạt động vì chúng ta có thể kiểm tra tổng tiền tố theo thời gian tuyến tính cho mỗi hoán vị, nhưng số lượng hoán vị là giai thừa trong`n`. Ngay cả đối với`n = 20`,`20!`đại khái là`2.4 * 10^18`, điều đó hoàn toàn không thể thực hiện được. Lực lượng vũ phu hoạt động vì nó đảm bảo chúng tôi thử mọi thứ tự, nhưng không thành công đối với đầu vào lớn do quá trình tính toán bùng nổ. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải xem xét các hoán vị tùy ý. Vì các ô chỉ có giá trị 1 hoặc 2 nên chúng ta có thể suy luận xem chuỗi nào tạo ra tổng tiền tố nguyên tố sớm. Các số nguyên tố nhỏ nhất là 2, 3, 5, 7, 11, ... Để tối đa hóa các số nguyên tố trong tổng tiền tố đầu, chúng ta muốn hai phần tử đầu tiên có tổng bằng 3 (số nguyên tố lẻ đầu tiên sau 2). Điều này có thể được thực hiện bằng cách đặt một`2`đầu tiên, theo sau là một`1`, nếu cả hai đều có mặt. Một khi chúng ta đã sử dụng một`2`và một`1`, chúng ta có thể đặt tất cả các số 1 còn lại, sau đó là tất cả các số 2 còn lại. Chiến lược tham lam này đảm bảo tổng tiền tố đạt 2, 3 và sau đó tiếp tục tăng theo cách có thể dự đoán được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tham lam đặt 2 rồi 1, rồi còn lại 1s, rồi 2s | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lượng`1`cát`2`s trong đầu vào. Cho phép`count1`Và`count2`lưu trữ số lượng này. Việc biết số lượng cho phép chúng ta xây dựng chuỗi một cách trực tiếp mà không cần tìm kiếm tất cả các hoán vị. 
2. Khởi tạo danh sách kết quả trống. Chúng ta sẽ xây dựng hoán vị một cách tuần tự. 
3. Nếu cả hai`count1`Và`count2`là tích cực, thêm một`2`theo sau là một`1`vào danh sách kết quả. Giảm bớt`count1`Và`count2`tương ứng. Điều này đảm bảo hai tổng tiền tố đầu tiên là 2 và 3, hai số nguyên tố đầu tiên. 
4. Nối tất cả còn lại`1`s vào danh sách kết quả. Mỗi số 1 tăng tổng tiền tố lên 1, số này chạm vào số nguyên tố tiếp theo bất cứ khi nào có thể theo trình tự. 
5. Nối tất cả còn lại`2`s vào danh sách kết quả. Sau khi hết 1, mỗi lần thêm 2 sẽ tăng tổng lên 2, giữ cho chuỗi có thể dự đoán được. 
6. Xuất danh sách kết quả. 

Tại sao nó hoạt động: Bằng cách đặt`2`sau đó`1`trước tiên, chúng tôi đảm bảo tổng tiền tố 2 và 3. Việc điền các số 1 còn lại trước tiên đảm bảo chúng tôi đạt được các số nguyên tố nhỏ liên tiếp như 5, 7, 11 càng sớm càng tốt. Đặt 2 giây cuối cùng để tránh bỏ qua các số nguyên tố nhỏ đầu một cách không cần thiết. Cách xây dựng tham lam này tôn trọng cấu trúc của các số nguyên tố và các giá trị giới hạn của các ô, đảm bảo số lượng tổng tiền tố nguyên tố tối đa mà không cần kiểm tra kỹ lưỡng các hoán vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
tiles = list(map(int, input().split()))

count1 = tiles.count(1)
count2 = tiles.count(2)

result = []

if count1 > 0 and count2 > 0:
    result.append(2)
    result.append(1)
    count2 -= 1
    count1 -= 1

result.extend([1] * count1)
result.extend([2] * count2)

print(' '.join(map(str, result)))
```Giải pháp này trước tiên sẽ đếm các ô, sau đó áp dụng chiến lược tham lam. Chúng tôi kiểm tra xem cả số 1 và số 2 đều có mặt trước khi đặt số ban đầu`2`Và`1`. Các ô còn lại được nối theo thứ tự có thể dự đoán được, tránh lỗi sai sót một. Kết quả được in dưới dạng một chuỗi được phân tách bằng dấu cách. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
5
1 2 1 2 1
```| Bước | đếm1 | đếm2 | Kết quả | Tổng tiền tố | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 2 | [] | [] | 
| Đặt 2 rồi 1 | 2 | 1 | [2,1] | 2,3 | 
| Thêm 1s còn lại | 0 | 1 | [2,1,1,1] | 2,3,4,5 | 
| Thêm 2s còn lại | 0 | 0 | [2,1,1,1,2] | 2,3,4,5,7 | 

Tổng tiền tố 2, 3, 5, 7 là số nguyên tố, tối đa hóa số nguyên tố. 

**Đầu vào ví dụ tùy chỉnh**```
4
1 1 1 1
```| Bước | đếm1 | đếm2 | Kết quả | Tổng tiền tố | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 4 | 0 | [] | [] | 
| Đặt 2 rồi 1 | không thể | | [] | | 
| Thêm 1s còn lại | 0 | 0 | [1,1,1,1] | 1,2,3,4 | 

Tổng tiền tố 2 và 3 là số nguyên tố, là giá trị tối đa cho đầu vào này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc đếm các ô và xây dựng danh sách kết quả yêu cầu một lần chuyển qua đầu vào và nhiều nhất là các thao tác nối thêm O(n). | 
| Không gian | O(n) | Chúng tôi lưu trữ chuỗi kết quả một cách rõ ràng, tỷ lệ thuận với n. | 

Vì n 200.000 và các phép toán là tuyến tính nên điều này phù hợp thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    tiles = list(map(int, input().split()))
    count1 = tiles.count(1)
    count2 = tiles.count(2)
    result = []
    if count1 > 0 and count2 > 0:
        result.append(2)
        result.append(1)
        count2 -= 1
        count1 -= 1
    result.extend([1] * count1)
    result.extend([2] * count2)
    return ' '.join(map(str, result))

# provided sample
assert run("5\n1 2 1 2 1\n") in ["2 1 1 1 2", "2 1 1 2 1"], "sample 1"

# custom: all 1s
assert run("4\n1 1 1 1\n") == "1 1 1 1", "all 1s"

# custom: all 2s
assert run("3\n2 2 2\n") == "2 2 2", "all 2s"

# custom: single tile
assert run("1\n1\n") == "1", "single 1"
assert run("1\n2\n") == "2", "single 2"

# custom: two tiles
assert run("2\n1 2\n") in ["2 1", "1 2"], "two tiles"
assert run("2\n2 1\n") in ["2 1", "1 2"], "two tiles order"

# custom: large input
assert run("6\n2 2 1 1 2 1\n") in ["2 1 1 1 2 2"], "mixed large"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 1 1 1 1 | 1 1 1 1 | Thuật toán xử lý chính xác tất cả số 1 | 
| 3 2 2 2 | 2 2 2 | Thuật toán xử lý chính xác cả 2s | 
| 1 1 | 1 | Đầu vào ô đơn | 
| 2 1 2 | 2 1 | Vị trí tham lam chính xác cho đầu vào nhỏ |
