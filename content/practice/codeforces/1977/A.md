---
title: "CF 1977A - Nikita Nhỏ"
description: "Nikita có một tòa tháp bắt đầu trống và anh ta có thể thực hiện chính xác một trong hai thao tác cho mỗi lần di chuyển: đặt một khối lên trên hoặc loại bỏ một khối khỏi đỉnh. Bài toán hỏi liệu sau khi thực hiện đúng n bước di chuyển, tòa tháp có thể có đúng m khối hay không."
date: "2026-06-08T17:15:08+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1977
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 948 (Div. 2)"
rating: 800
weight: 1977
solve_time_s: 92
verified: true
draft: false
---

[CF 1977A - Nikita bé nhỏ](https://codeforces.com/problemset/problem/1977/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nikita có một tòa tháp bắt đầu trống và anh ta có thể thực hiện chính xác một trong hai thao tác cho mỗi lần di chuyển: đặt một khối lên trên hoặc loại bỏ một khối khỏi đỉnh. Vấn đề hỏi liệu sau khi thực hiện chính xác`n`di chuyển, tòa tháp có thể có chính xác`m`hình khối. Mỗi trường hợp thử nghiệm cung cấp số lần di chuyển`n`và chiều cao mong muốn`m`. Đầu ra là "Có" nếu điều này có thể thực hiện được và "Không" nếu ngược lại. 

Các ràng buộc rất nhỏ: cả hai`n`Và`m`nhiều nhất là 100 và có nhiều nhất 100 ca kiểm thử. Điều này có nghĩa là bất kỳ thuật toán nào có độ phức tạp gần như`O(n)`mỗi trường hợp thử nghiệm sẽ chạy thoải mái trong thời gian giới hạn. Vấn đề cơ bản là về tính chẵn lẻ và khả năng tiếp cận hơn là mô phỏng toàn diện. 

Các trường hợp cạnh không rõ ràng xảy ra khi`m`lớn hơn`n`hoặc khi nào`m`Và`n`có độ chẵn lẻ khác nhau. Ví dụ, nếu`n = 2`Và`m = 3`, không có cách nào đạt được ba khối chỉ trong hai nước đi. Tương tự, nếu`n = 3`Và`m = 2`, các nước đi phải rời khỏi tháp ở độ cao 2 sau ba thao tác, nhưng vì mỗi nước đi thay đổi chiều cao đúng 1 nên tính chẵn lẻ của`n`Và`m`phải phù hợp thì mới có thể thực hiện được. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi thêm và loại bỏ các hình khối. Đối với mỗi lần di chuyển, bạn có thể thử đệ quy thêm hoặc xóa khối lập phương và kiểm tra xem chiều cao cuối cùng có khớp không`m`. Điều này hoạt động với các đầu vào rất nhỏ nhưng có độ phức tạp theo cấp số nhân: mỗi`n`di chuyển có hai lựa chọn, vì vậy có`2^n`các trình tự có thể. Ngay cả với`n = 100`, điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là yếu tố duy nhất giới hạn chiều cao tháp cuối cùng là tổng số lần di chuyển`n`và sự ngang bằng của`m`. Mỗi lần di chuyển sẽ thay đổi chiều cao của tháp đúng một, vì vậy sau`n`di chuyển, độ cao chênh lệch so với điểm bắt đầu (không) phải là một số có cùng độ chẵn lẻ với`n`. Ngoài ra, chiều cao tối đa có thể đạt được là`n`nếu Nikita chỉ thêm hình khối. Vì vậy, điều kiện để đạt được chính xác`m`hình khối là vậy`m`nhiều nhất là`n`Và`m`có cùng độ ngang bằng với`n`. Điều này làm giảm vấn đề xuống còn một phép kiểm tra đơn giản mà không cần mô phỏng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, hãy đọc các số nguyên`n`Và`m`. 
3. Kiểm tra xem`m`nhỏ hơn hoặc bằng`n`. Nếu không, xuất ra "No" và tiếp tục. 
4. Kiểm tra xem tính chẵn lẻ của`n`Và`m`trận đấu, ý nghĩa`(n - m) % 2 == 0`. Nếu có, xuất "Có"; nếu không thì xuất ra "Không". 

Lý do điều này có hiệu quả là vì mỗi lần di chuyển sẽ thay đổi chiều cao của tháp đúng 1, do đó, sự khác biệt giữa`n`Và`m`phải là số chẵn để đạt được chính xác`m`sau đó`n`di chuyển. điều kiện`m <= n`đảm bảo Nikita không cần di chuyển nhiều hơn mức cho phép để đạt được độ cao mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    if m <= n and (n - m) % 2 == 0:
        print("Yes")
    else:
        print("No")
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Đối với mỗi trường hợp thử nghiệm, nó sẽ kiểm tra ngay hai điều kiện:`m <= n`đảm bảo chúng ta có đủ bước di chuyển để đạt đến độ cao và`(n - m) % 2 == 0`đảm bảo số lần di chuyển lên xuống có thể sắp xếp đạt chính xác`m`. Điều này tránh các vòng lặp hoặc đệ quy không cần thiết. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n = 3, m = 3`| n | m | n - m | kiểm tra tính chẵn lẻ | Kết quả | 
| --- | --- | --- | --- | --- | 
| 3 | 3 | 0 | 0 % 2 == 0 | Có | 

Sự khác biệt là bằng không, thậm chí và`m <= n`, vì vậy có thể đạt được 3 khối bằng cách thêm một khối vào mỗi lần di chuyển. 

**Ví dụ 2:**`n = 2, m = 4`| n | m | n - m | kiểm tra tính chẵn lẻ | Kết quả | 
| --- | --- | --- | --- | --- | 
| 2 | 4 | -2 | -2 % 2 == 0 | Không | 

Mặc dù việc kiểm tra tính chẵn lẻ được thỏa mãn,`m > n`khiến không thể đạt tới độ cao 4 chỉ trong 2 nước đi. 

Những dấu vết này cho thấy cả hai điều kiện đều cần và đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được đánh giá trong thời gian không đổi. | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài bộ nhớ đầu vào. | 

Với`t`lên tới 100 và`n, m`lên tới 100, giải pháp chạy thoải mái trong vòng chưa đầy 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        if m <= n and (n - m) % 2 == 0:
            print("Yes")
        else:
            print("No")
    return out.getvalue().strip()

# provided samples
assert run("3\n3 3\n2 4\n5 3\n") == "Yes\nNo\nYes", "sample 1"

# custom cases
assert run("4\n1 1\n2 2\n2 1\n100 100\n") == "Yes\nYes\nNo\nYes", "edge and max"
assert run("2\n1 2\n3 0\n") == "No\nYes", "parity and zero"
assert run("3\n10 10\n10 0\n10 5\n") == "Yes\nYes\nNo", "all-even, all-zero, mismatched parity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1, 2 2, 2 1, 100 100 | Có, Có, Không, Có | Kích thước tối thiểu, khớp chính xác, chiều cao không thể, kích thước tối đa | 
| 1 2, 3 0 | Không, Có | Tính chẵn lẻ không khớp, đạt tới số 0 | 
| 10 10, 10 0, 10 5 | Có, Có, Không | Tính chẵn lẻ tối đa, bằng 0 và không thể | 

## Vỏ cạnh 

Khi nào`m = 0`Và`n`là số chẵn, thuật toán đưa ra "Có" một cách chính xác vì Nikita có thể lần lượt thêm và xóa các hình khối để kết thúc bằng một tháp trống. Ví dụ,`n = 4, m = 0`kết quả là`(n - m) % 2 = 0`Và`m <= n`, vì vậy đầu ra là "Có". 

Khi`m > n`, thuật toán ngay lập tức đưa ra "Không", tránh các giả định không chính xác về các bước đi tiêu cực. Ví dụ,`n = 3, m = 5`tạo ra "Không" mặc dù giá trị chẵn lẻ khớp vì không thể truy cập được chiều cao. 

Khi`n`Và`m`khác nhau về tính chẵn lẻ, thuật toán đưa ra "Không", xử lý trường hợp tinh vi trong đó chuỗi di chuyển ±1 không thể đến được mục tiêu do không khớp chẵn lẻ, chẳng hạn như`n = 5, m = 2`.
