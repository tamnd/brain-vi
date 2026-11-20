---
title: "CF 2180A - Bánh Xe Lễ Hội"
description: "Chúng ta được trao một bánh xe giải thưởng hình tròn có l phần được đánh số từ 0 đến l-1. Con trỏ bánh xe bắt đầu tại phần a. Mỗi vòng quay di chuyển con trỏ về phía trước chính xác b vị trí modulo l."
date: "2026-06-07T22:06:16+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2180
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 31 (Div. 1 + Div. 2)"
rating: 800
weight: 2180
solve_time_s: 94
verified: true
draft: false
---

[CF 2180A - Bánh xe lễ hội](https://codeforces.com/problemset/problem/2180/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, lý thuyết số 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao một bánh xe giải thưởng hình tròn với`l`phần được đánh số từ`0`ĐẾN`l-1`. Con trỏ bánh xe bắt đầu tại một phần`a`. Mỗi lần quay sẽ di chuyển con trỏ về phía trước một cách chính xác`b`vị trí modulo`l`. Mục tiêu là xác định số phần tối đa mà chúng ta có thể đạt được bằng cách quay bánh xe với số lần bất kỳ, kể cả số lần quay bằng 0. 

Các hạn chế ở mức vừa phải:`l`Và`b`mỗi trường hợp có thể lên tới 5000 và có thể có tới 500 trường hợp thử nghiệm. Điều này có nghĩa là bất kỳ cách tiếp cận nào mô phỏng mọi vòng quay lên tới`l`các bước cho mỗi trường hợp thử nghiệm là khả thi vì 5000 bước nhân với 500 trường hợp thử nghiệm là khoảng 2,5 triệu thao tác, có thể chấp nhận được trong thời gian 1 giây. Phương pháp brute-force lặp lại các vòng quay cho đến khi các vị trí lặp lại sẽ hoạt động, nhưng đáng để nghiên cứu xem liệu có tồn tại một công thức đơn giản hơn để tránh việc lặp lại không cần thiết hay không. 

Các trường hợp cạnh không rõ ràng phát sinh khi`b`lớn hơn`l`, khi`b`Và`l`không phải là nguyên tố cùng nhau, hoặc khi`b`chia rẽ`l`chính xác. Ví dụ, với`l=2`,`a=0`, Và`b=6`, con trỏ không bao giờ rời khỏi phần`0`bởi vì`(0 + k*6) mod 2`luôn luôn là`0`. Một cách tiếp cận bất cẩn cho rằng`b`luôn tạo ra các vị trí mới có thể đề xuất không chính xác mức tối đa cao hơn. Một trường hợp cạnh khác là`b=1`, nơi mà tất cả các vị trí cuối cùng sẽ được truy cập, do đó mức tối đa luôn là`l-1`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bắt đầu từ`a`, chúng tôi tiếp tục thêm`b`modulo`l`và theo dõi giá trị tối đa đạt được. Điều này đúng vì phép cộng lặp lại modulo`l`duyệt qua một tập hợp con các phần được xác định bởi ước chung lớn nhất của`b`Và`l`. Trong trường hợp xấu nhất, con trỏ có thể chạm vào tất cả`l`vị trí, vì vậy số lần lặp tối đa là`l`. Trên 500 trường hợp thử nghiệm, điều này dẫn đến`500 * 5000 = 2.5 * 10^6`hoạt động nằm trong giới hạn có thể chấp nhận được. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là nhận ra rằng các vị trí mà con trỏ có thể đạt tới tạo thành một modulo cấp số cộng`l`. Cụ thể, trình tự các phần có thể truy cập là`a, (a+b) mod l, (a+2b) mod l, ...`. Chu kỳ sẽ kết thúc sau`l / gcd(b, l)`quay vì sau nhiều lần quay,`(a + k*b) mod l`trở về`a`. Phần có thể truy cập tối đa có thể được tính trực tiếp mà không cần lặp lại bằng công thức`l - gcd(l, b) + ((a - 1) % gcd(l, b))`hoặc logic đơn giản hơn: phần cuối cùng trong tiến trình số học trước khi chu trình lặp lại. 

Quan sát này làm giảm vấn đề trong việc tính ước số chung lớn nhất (GCD) của`l`Và`b`và sau đó xác định mức tối đa trong tập hợp con có thể truy cập. Điều này tránh việc mô phỏng mọi vòng quay trong khi vẫn đảm bảo tính chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(l) cho mỗi trường hợp thử nghiệm | O(1) | Có thể chấp nhận được với giới hạn nhất định | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận hoàn toàn | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`l`,`a`, Và`b`. 
2. Tính toán`g = gcd(l, b)`. Điều này cho chúng ta biết kích thước của bước giữa các vị trí có thể tiếp cận liên tiếp theo modulo`l`. 
3. Xác định vị trí có thể tiếp cận cuối cùng trong chu trình. Vị trí tối đa là`l - g + a % g`. Điều này hoạt động vì các vị trí cách nhau`g`ngoài, bắt đầu từ`a % g`, và giá trị lớn nhất dưới đây`l`trong cấp số cộng này là`l - g + (a % g)`. 
4. In giá trị tối đa này cho từng trường hợp thử nghiệm. 

Lý do điều này có tác dụng là vì con trỏ chỉ có thể tiếp cận các phần phù hợp với`a modulo g`. Phần lớn nhất trong lớp đồng dư dưới đây`l`là`l - g + (a % g)`. Mọi vị trí có thể tiếp cận đều được đưa vào cấp số cộng, vì vậy chúng tôi không bỏ lỡ bất kỳ ứng cử viên nào ở mức tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd

t = int(input())
for _ in range(t):
    l, a, b = map(int, input().split())
    g = gcd(l, b)
    max_prize = l - g + a % g
    print(max_prize)
```Giải pháp bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó tính toán GCD của`l`Và`b`, xác định kích thước bước của các vị trí modulo`l`. Sau đó, nó tính toán phần có thể tiếp cận tối đa trong chu trình bằng công thức`l - g + (a % g)`. Công thức này xử lý phần còn lại một cách cẩn thận để tính đến vị trí bắt đầu`a`và đảm bảo rằng giá trị đó hoàn toàn nhỏ hơn`l`. Kết quả được in ngay lập tức. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`l=5, a=3, b=2`| k | Vị trí (a + k*b) % l | Ghi chú | 
| --- | --- | --- | 
| 0 | 3 | bắt đầu | 
| 1 | 0 | 3+2=5 % 5=0 | 
| 2 | 2 | 0+2=2 %5=2 | 
| 3 | 4 | 2+2=4 %5=4 | 
| 4 | 1 | 4+2=6 %5=1 | 
| 5 | 3 | chu kỳ lặp lại | 

Các vị trí có thể tiếp cận là 0,1,2,3,4. Tối đa là 4. GCD là 5 và 2 là 1, vì vậy`max_prize = 5-1 + 3 %1 = 4`. 

Đầu vào mẫu:`l=2, a=0, b=6`| k | Vị trí (a + k*b) % l | 
| --- | --- | 
| 0 | 0 | 
| 1 | 0 | 
| 2 | 0 | 

GCD(2,6)=2. Có thể tiếp cận tối đa:`2 - 2 + 0%2 = 0`. 

Những dấu vết này xác nhận rằng công thức xử lý các chu trình và điểm cố định một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm lấy O(1) do tính toán và số học GCD. | 
| Không gian | O(1) | Chỉ cần một vài biến số nguyên cho mỗi trường hợp thử nghiệm. | 

Với`t=500`Và`l,b<=5000`, giải pháp này thực thi tốt dưới 1 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    t = int(input())
    for _ in range(t):
        l, a, b = map(int, input().split())
        g = gcd(l, b)
        output.write(str(l - g + a % g) + "\n")
    return output.getvalue().strip()

# Provided samples
assert run("4\n5 3 2\n2 0 6\n8 2 4\n100 0 1\n") == "4\n0\n6\n99", "samples"

# Custom cases
assert run("1\n1 0 1\n") == "0", "minimum size input"
assert run("1\n5000 4999 5000\n") == "4999", "l and b max, starting at last section"
assert run("1\n7 0 7\n") == "0", "b divisible by l"
assert run("1\n10 3 4\n") == "9", "small l, b and a in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 1 | 0 | đầu vào kích thước tối thiểu | 
| 5000 4999 5000 | 4999 | đầu vào kích thước tối đa | 
| 7 0 7 | 0 | b chia hết cho l, con trỏ không di chuyển được | 
| 10 3 4 | 9 | trường hợp nhỏ tổng quát, tính max đúng | 

## Vỏ cạnh 

cho`l=2, a=0, b=6`, con trỏ vẫn ở mức 0. Công thức tính`gcd(2,6)=2`, Và`max_prize = 2 - 2 + 0%2 = 0`, phù hợp với hành vi được quan sát. Vì`l=5, a=3, b=2`, con trỏ sẽ di chuyển qua tất cả các vị trí. GCD là 1, vì vậy`max_prize = 5-1 + 3%1 = 4`, chụp chính xác phần cao nhất. Công thức xử lý chính xác cả vị trí đứng yên và chu kỳ đầy đủ.
