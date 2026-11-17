---
title: "CF 1985E - Chiếc Hộp Bí Mật"
description: "Chúng ta được yêu cầu đặt một hộp nhỏ có độ dài các cạnh nguyên bên trong một hộp lớn hơn trong không gian 3D. Hộp nhỏ phải có thể tích cụ thể $k$ và phải thẳng hàng với các trục, với tất cả các góc ở tọa độ nguyên."
date: "2026-06-08T16:21:13+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1985
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 952 (Div. 4)"
rating: 1200
weight: 1985
solve_time_s: 151
verified: true
draft: false
---

[CF 1985E - Hộp bí mật](https://codeforces.com/problemset/problem/1985/E) 

**Đánh giá:** 1200 
**Tags:** lực lượng vũ phu, tổ hợp, toán học 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đặt một hộp nhỏ có độ dài các cạnh nguyên bên trong một hộp lớn hơn trong không gian 3D. Hộp nhỏ phải có thể tích cụ thể$k$và phải căn chỉnh với các trục, với tất cả các góc có tọa độ nguyên. Thử thách ở đây là chọn độ dài các cạnh của hộp nhỏ này sao cho số lượng vị trí hợp lệ bên trong hộp lớn hơn là lớn nhất. Vị trí hợp lệ xảy ra khi hộp nằm hoàn toàn bên trong hộp lớn hơn mà không vượt quá ranh giới của nó. 

Đầu vào bao gồm các kích thước$x, y, z$của hộp lớn và khối lượng mong muốn$k$của chiếc hộp bí mật. Đối với mỗi trường hợp thử nghiệm, chúng ta phải xuất ra số lượng vị trí số nguyên riêng biệt tối đa cho bất kỳ lựa chọn kích thước nguyên nào của hộp bí mật. Nếu không có tập kích thước nguyên nào tạo ra thể tích$k$có thể nhét vừa bên trong chiếc hộp lớn, câu trả lời là số không. 

Các ràng buộc cho phép mỗi cạnh của hộp lớn lên tới 2000 và$k$lên đến$x \cdot y \cdot z$. Có tới 2000 trường hợp thử nghiệm, nhưng tổng của tất cả$x$,$y$, Và$z$trong tất cả các thử nghiệm không vượt quá 2000. Điều này ngụ ý rằng, tổng cộng, các vòng lặp lồng nhau của chúng tôi theo độ dài cạnh hoặc các hệ số của$k$sẽ không vượt quá khoảng$2000^3$trên tất cả các trường hợp thử nghiệm.$k$có thể rất lớn, do đó phải cẩn thận để tránh tràn 32 bit. 

Một cách tiếp cận đơn giản có thể bỏ sót các trường hợp trong đó một hoặc nhiều chiều dài cạnh lớn hơn kích thước tương ứng của hộp, tạo ra không có vị trí nào. Ví dụ, một hộp lớn có kích thước$2 \times 2 \times 2$với$k = 7$không thể vừa với bất kỳ hộp có kích thước nguyên nào vì không có bộ ba số nguyên nào nhân thành 7 trong giới hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là liệt kê tất cả các bộ ba số nguyên có thể$(a, b, c)$như vậy$a \cdot b \cdot c = k$. Với mỗi bộ ba, hãy tính xem có bao nhiêu vị trí phù hợp bên trong hộp lớn bằng cách sử dụng$(x-a+1) \cdot (y-b+1) \cdot (z-c+1)$, chỉ tính nếu cả ba yếu tố đều dương. Điều này có tác dụng vì số lượng vị trí hợp lệ dọc theo mỗi trục chỉ đơn giản là số lượng vị trí mà hộp có thể bắt đầu mà không vượt quá ranh giới. Khó khăn của lực lượng vũ phu này là bao thanh toán$k$và tạo ra tất cả các bộ ba. Một vòng lặp hình khối ngây thơ kết thúc$a, b, c$lên đến$\sqrt[3]{k}$quá chậm khi$k$rất lớn, lên tới$2000^3$về mặt lý thuyết. 

Cái nhìn sâu sắc quan trọng là chúng ta chỉ cần xem xét các ước số của$k$. Đầu tiên, tạo ra tất cả các ước của$k$lên đến$\sqrt{k}$. Với mỗi số chia$a$, xét tất cả các ước số$b$của$k // a$và thiết lập$c = k // (a \cdot b)$. Điều này đảm bảo$a \cdot b \cdot c = k$. Sau đó kiểm tra xem$a \le x$,$b \le y$, Và$c \le z$. Nếu vậy, hãy tính số lượng vị trí$(x-a+1) \cdot (y-b+1) \cdot (z-c+1)$và theo dõi tối đa. Điều này làm giảm đáng kể số lần lặp vì số lượng ước số tăng chậm, gần như$O(k^{1/3})$để tạo ra bộ ba. 

Bài toán có tính đối xứng: thứ tự của$a, b, c$vấn đề liên quan đến$x, y, z$vì hộp không thể xoay được. Tất cả sáu hoán vị phải được xem xét cho mỗi bộ ba hợp lệ để có được sự phù hợp chính xác. Nếu không xem xét tất cả các hoán vị, một số cách sắp xếp tối ưu có thể bị bỏ qua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^(1/3) * k^(1/3)) | O(1) | Quá chậm đối với k lớn | 
| Tìm kiếm ba dựa trên số chia | O(d(k)^2) | O(d(k)) | Được chấp nhận, hiệu quả | 

Đây$d(k)$là số ước của$k$, có thể quản lý được đối với$k \le 2000^3$vì chúng ta chỉ cần phân tích số nguyên của kích thước vừa phải$k$. 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử và lặp lại từng trường hợp. Trích xuất$x, y, z, k$. 
2. Khởi tạo một biến để theo dõi số lượng vị trí tối đa cho trường hợp thử nghiệm này. 
3. Liệt kê tất cả các ước số$a$của$k$. Với mỗi số chia$a$,$a$đại diện cho một chiều dài có thể dọc theo trục x. 
4. Đối với mỗi$a$, liệt kê tất cả các ước số$b$của$k // a$. Tính toán$c = k // (a \cdot b)$để đảm bảo$a \cdot b \cdot c = k$. 
5. Tạo tất cả các hoán vị của bộ ba$(a, b, c)$. Với mỗi hoán vị, hãy kiểm tra xem$a \le x$,$b \le y$, Và$c \le z$. Nếu vậy, hãy tính số lượng vị trí dọc theo mỗi trục như$(x-a+1) \cdot (y-b+1) \cdot (z-c+1)$. 
6. Theo dõi số lượng vị trí tối đa trong số tất cả các hoán vị. 
7. Sau khi kiểm tra tất cả các ước số và hoán vị, hãy xuất ra số đếm tối đa hoặc bằng 0 nếu không thể sắp xếp được. 

Tại sao nó hoạt động: Bằng cách tính toán$k$và xem xét tất cả các hoán vị, chúng tôi đảm bảo kiểm tra tất cả các kết hợp độ dài cạnh nguyên tạo ra khối lượng$k$. Kiểm tra sự phù hợp với$x, y, z$đảm bảo tính hợp lệ. Việc xem xét các hoán vị sẽ nắm bắt tất cả các cách mà hộp có thể căn chỉnh với các trục. Số lượng vị trí được tính toán tối đa là chính xác vì chúng tôi đánh giá mọi ứng viên khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from itertools import permutations
from math import isqrt

def divisors(n):
    small, large = [], []
    for i in range(1, isqrt(n) + 1):
        if n % i == 0:
            small.append(i)
            if i != n // i:
                large.append(n // i)
    return small + large[::-1]

t = int(input())
for _ in range(t):
    x, y, z, k = map(int, input().split())
    max_places = 0
    divs_a = divisors(k)
    for a in divs_a:
        if a > x:
            continue
        remaining = k // a
        divs_b = divisors(remaining)
        for b in divs_b:
            c = remaining // b
            for (aa, bb, cc) in permutations([a, b, c]):
                if aa <= x and bb <= y and cc <= z:
                    places = (x - aa + 1) * (y - bb + 1) * (z - cc + 1)
                    if places > max_places:
                        max_places = places
    print(max_places)
```Giải pháp đầu tiên tạo ra các ước số một cách hiệu quả, sau đó xây dựng các bộ ba hợp lệ. Hoán vị đảm bảo chúng tôi kiểm tra tất cả các sắp xếp trục. Chúng tôi tránh tràn vì Python xử lý các số nguyên lớn và chúng tôi sớm bỏ qua các ước số vượt quá kích thước hộp. Nhân vị trí sử dụng công thức$(dimension - side + 1)$dọc theo mỗi trục. 

## Ví dụ đã hoạt động 

Mẫu 1:$x = 3, y = 3, z = 3, k = 8$| một | b | c | hoán vị | phù hợp? | vị trí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 8 | (1,1,8) | không | - | 
| 1 | 2 | 4 | (2,1,4) | không | - | 
| 2 | 2 | 2 | (2,2,2) | vâng | (3-2+1)_(3-2+1)_(3-2+1)=2_2_2=8 | 

Vị trí tối đa là 8, xác nhận đầu ra mẫu đầu tiên. 

Mẫu 2:$x = 3, y = 3, z = 3, k = 18$| một | b | c | hoán vị | phù hợp? | vị trí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 9 | (2,1,9) | không | - | 
| 2 | 3 | 3 | (2,3,3) | vâng | (3-2+1)_(3-3+1)_(3-3+1)=2_1_1=2 | 

Vị trí tối đa là 2, phù hợp với mẫu. 

Các bảng này minh họa việc tạo tất cả các bộ ba thông qua ước số và hoán vị cũng như tính toán chính xác số lượng vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d(k)^2 * 6) | d(k) là số ước của k; tạo hoán vị nhân với 6 | 
| Không gian | O(d(k)) | lưu trữ các ước của k và danh sách trung gian | 

Cách tiếp cận dựa trên số chia là hiệu quả cho tất cả$k \le 2000^3$bởi vì tổng của tất cả các kích thước hộp bị giới hạn. Số lượng ước số tăng chậm, do đó các vòng lặp lồng nhau trên các ước số vẫn nằm trong phạm vi hoạt động khả thi cho 2000 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin
```
