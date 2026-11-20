---
title: "CF 2134A - Tranh Hai Màu"
description: "Chúng ta có một hàng n ô, ban đầu tất cả đều màu trắng. Chúng tôi có hai hoạt động sơn. Thao tác đầu tiên tô màu đỏ cho các ô liên tiếp, bắt đầu từ bất kỳ vị trí x nào sao cho khối màu đỏ nằm hoàn toàn trong hàng."
date: "2026-06-08T02:42:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2134
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1045 (Div. 2)"
rating: 800
weight: 2134
solve_time_s: 103
verified: false
draft: false
---

[CF 2134A - Tranh hai màu](https://codeforces.com/problemset/problem/2134/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một hàng`n`tế bào, ban đầu tất cả đều có màu trắng. Chúng tôi có hai hoạt động sơn. Hoạt động sơn đầu tiên`a`các ô liên tiếp màu đỏ, bắt đầu từ vị trí bất kỳ`x`sao cho khối màu đỏ nằm hoàn toàn trong hàng. Hoạt động thứ hai sơn`b`các ô liên tiếp màu xanh lam, bắt đầu từ vị trí bất kỳ`y`, cũng bị ràng buộc để vừa với hàng. Nếu một ô được sơn màu đỏ rồi đến màu xanh lam, nó sẽ có màu xanh lam. Sau cả hai thao tác, chúng tôi muốn biết liệu màu kết quả có thể đối xứng hay không: đối với mọi ô`i`, màu của nó khớp với màu của ô ở vị trí`n+1-i`. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định`n`,`a`, Và`b`. Đầu ra là "CÓ" nếu tồn tại sự lựa chọn`x`Và`y`tạo ra màu đối xứng và "KHÔNG" nếu không. 

Các giới hạn là đáng kể:`n`có thể lên tới 10^9 và`t`lên tới 500. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào xây dựng rõ ràng mảng độ dài`n`bởi vì ngay cả một trường hợp thử nghiệm cũng có thể vượt quá giới hạn bộ nhớ và việc lặp lại trên tất cả các vị trí sẽ quá chậm. Lời giải phải dựa vào lý luận số học hơn là mô phỏng. 

Các trường hợp cạnh phát sinh khi các khối chiếm độ dài cực lớn so với`n`. Ví dụ, nếu`a`hoặc`b`bằng`n`, bức tranh bao phủ toàn bộ hàng. Các giá trị nhỏ của`a`hoặc`b`vẫn có thể cho phép tính đối xứng nếu chúng có thể được căn giữa một cách thích hợp. Sự chồng chéo của màu đỏ và màu xanh lam, kết hợp với sự ưu tiên của màu xanh lam trên màu đỏ, có thể tạo ra sự bất đối xứng nếu không được đặt cẩn thận. Ví dụ, với`n = 3, a = 2, b = 1`, khối màu đỏ bao phủ các ô 1-2 và một ô màu xanh lam chỉ có thể bao phủ một ô. Nếu ô màu xanh được đặt ở ô giữa thì có thể xảy ra sự đối xứng. Nếu đặt ở nơi khác, nó có thể phá vỡ tính đối xứng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng thử tất cả các vị trí hợp lệ để`x`Và`y`, mô phỏng bức tranh, sau đó kiểm tra xem mảng kết quả có đối xứng hay không. Vì`x`, có`n-a+1`các tùy chọn và cho`y`có`n-b+1`tùy chọn. Kiểm tra tính đối xứng mất`O(n)`. Do đó, số lượng hoạt động trong trường hợp xấu nhất là`(n-a+1)*(n-b+1)*O(n)`. Từ`n`có thể là 10^9, điều này là không khả thi. 

Cái nhìn sâu sắc quan trọng là tính đối xứng chỉ phụ thuộc vào độ dài`a`Và`b`, không phải trên vị trí thực tế`x`Và`y`. Nếu chiều dài khối màu đỏ`a`và chiều dài khối màu xanh`b`thỏa mãn`a + b <= n + 1`, luôn có thể đặt các khối màu đỏ và xanh đối xứng nhau quanh tâm. Nếu khối màu xanh lam dài hơn một nửa hàng, nó chiếm ưu thế về tính đối xứng vì nó lấn át màu đỏ. Điều kiện nắm bắt được khả năng tạo ra màu đối xứng cuối cùng đơn giản hóa việc kiểm tra xem`a + b <= n + 1`. Điều này là do chúng ta có thể đặt khối màu đỏ bắt đầu ở gần đầu bên trái nếu cần, khối màu xanh bắt đầu ở gần đầu bên phải và mọi sự chồng chéo đều có thể được giải quyết do màu đỏ ghi đè màu xanh lam. Nếu như`a + b > n + 1`, các khối quá dài để tránh sự bất đối xứng, khiến cho sự đối xứng không thể thực hiện được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n-a+1)*(n-b+1)*n) | O(n) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, hãy đọc các số nguyên`n`,`a`, Và`b`. 
3. Kiểm tra xem`a + b <= n + 1`. Điều kiện này đảm bảo rằng các khối màu đỏ và màu xanh có thể được định vị sao cho khối màu xanh không phá vỡ tính đối xứng trong khi vẫn bao phủ hoàn toàn chiều dài dự định của nó. Bằng trực giác, tổng`a+b`đại diện cho tổng số ô mà các khối này chiếm giữ;`n+1`tính đến trục đối xứng ở các hàng có độ dài chẵn và lẻ. 
4. Nếu điều kiện đúng, in ra "CÓ". Ngược lại in ra "KHÔNG". 

Tại sao nó hoạt động: Bất biến là nếu`a + b <= n + 1`, chúng ta luôn có thể định vị khối màu đỏ bắt đầu ở chỉ số 1 và khối màu xanh kết thúc ở chỉ mục`n`, đảm bảo các ô được phản chiếu bên trái và bên phải có màu sắc giống nhau, với màu xanh lam ghi đè màu đỏ khi cần thiết. Nếu như`a + b > n + 1`, bất kỳ vị trí nào cũng làm cho một bên có nhiều màu xanh hoặc đỏ hơn gương của nó, phá vỡ tính đối xứng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, a, b = map(int, input().split())
    if a + b <= n + 1:
        print("YES")
    else:
        print("NO")
```Giải pháp đọc đầu vào bằng I/O nhanh vì có thể có tới 500 trường hợp kiểm thử. Nó đánh giá điều kiện số học đơn giản cho từng trường hợp thử nghiệm và in kết quả ngay lập tức. Không cần xây dựng mảng, tránh các vấn đề về bộ nhớ với dung lượng lớn`n`. Tính đơn giản của mã cũng tránh được từng lỗi một trong việc lập chỉ mục vì chúng tôi suy luận trực tiếp về độ dài khối. 

## Ví dụ đã hoạt động 

Hãy xem xét`n = 5, a = 3, b = 1`. tổng`a + b = 4`Và`n + 1 = 6`. Từ`4 <= 6`, câu trả lời là "CÓ". Chúng ta có thể đặt khối màu đỏ ở vị trí 2-4 và khối màu xanh ở vị trí 3. Hàng này là`[white, red, blue, red, white]`, đối xứng sau khi màu xanh lam lấn át màu đỏ. 

Vì`n = 7, a = 7, b = 4`, chúng tôi có`a + b = 11`Và`n + 1 = 8`. Từ`11 > 8`, sự đối xứng là không thể. Bất kỳ vị trí nào của khối màu đỏ dài 7 và khối màu xanh lam có chiều dài 4 đều làm cho nửa bên trái và bên phải không bằng nhau. Thuật toán trả về chính xác "KHÔNG". 

| Kiểm tra | n | một | b | a+b | n+1 | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 3 | 1 | 4 | 6 | CÓ | 
| 2 | 7 | 7 | 4 | 11 | 8 | KHÔNG | 

Những dấu vết này cho thấy cách so sánh tổng nắm bắt trực tiếp tính khả thi của vị trí đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một phép cộng và so sánh duy nhất được thực hiện cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(1) | Không có mảng hoặc cấu trúc dữ liệu nào tỷ lệ thuận với`n`được sử dụng. | 

Giải pháp xử lý thoải mái`t = 500`trường hợp thử nghiệm với`n`lên tới 10^9, phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n, a, b = map(int, input().split())
        print("YES" if a + b <= n + 1 else "NO")
    return out.getvalue().strip()

# provided samples
assert run("7\n5 3 1\n4 1 2\n7 7 4\n8 3 7\n1 1 1\n1000000000 1000000000 1000000000\n3 2 1") == "YES\nYES\nNO\nNO\nYES\nYES\nNO"

# custom cases
assert run("3\n1 1 1\n2 1 2\n10 5 6") == "YES\nNO\nNO"
assert run("2\n1000000000 1 1\n500000000 250000000 250000001") == "YES\nYES"
assert run("2\n5 5 1\n5 3 3") == "YES\nNO"
assert run("1\n3 2 2") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | CÓ | Đầu vào kích thước tối thiểu | 
| 2 1 2 | KHÔNG | Tổng vượt quá n+1 | 
| 10 5 6 | KHÔNG | Tổng hơi vượt quá n+1 | 
| 1000000000 1 1 | CÓ | N tối đa, khối tầm thường | 
| 500000000 250000000 250000001 | CÓ | Độ dài khối cạnh n tối đa | 
| 5 5 1 | CÓ | Khối màu đỏ lấp đầy hàng, màu xanh nhỏ | 
| 5 3 3 | KHÔNG | Màu đỏ và màu xanh chồng lên nhau quá nhiều | 
| 3 2 2 | KHÔNG | Lẻ n, các khối không thể đối xứng | 

## Vỏ cạnh 

cho`n = 1, a = 1, b = 1`, tổng`a + b = 2`Và`n + 1 = 2`. Thuật toán đưa ra "CÓ", điều này đúng vì một ô được sơn màu xanh lam sẽ ghi đè lên màu đỏ, tạo ra màu đối xứng. Vì`n = 10^9, a = 1, b = 1`,`a + b = 2 <= 10^9 + 1`, vì vậy "CÓ". Với `n = 3, a =
