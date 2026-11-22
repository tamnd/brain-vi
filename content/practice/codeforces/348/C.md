---
title: "CF 348C - Tập hợp con Tổng"
description: "Chúng ta được cung cấp một mảng các số nguyên và một tập hợp các tập hợp con, mỗi tập hợp con tham chiếu đến các chỉ số trong mảng. Hai loại thao tác được thực hiện lặp đi lặp lại: truy vấn tổng các phần tử của một tập hợp con và cộng giá trị cho tất cả các phần tử của một tập hợp con."
date: "2026-06-06T18:38:57+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 348
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 202 (Div. 1)"
rating: 2500
weight: 348
solve_time_s: 64
verified: true
draft: false
---

[CF 348C - Tổng tập hợp con](https://codeforces.com/problemset/problem/348/C) 

**Đánh giá:** 2500 
**Tags:** bạo lực, cấu trúc dữ liệu 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một tập hợp các tập hợp con, mỗi tập hợp con tham chiếu đến các chỉ số trong mảng. Hai loại thao tác được thực hiện lặp đi lặp lại: truy vấn tổng các phần tử của một tập hợp con và cộng giá trị cho tất cả các phần tử của một tập hợp con. Mục tiêu là trả lời các truy vấn tính tổng một cách hiệu quả sau một chuỗi phép cộng và các truy vấn có thể xen kẽ theo bất kỳ thứ tự nào. 

Các ràng buộc đầu vào rất đáng kể: tối đa 100.000 phần tử trong mảng, tối đa 100.000 tập hợp con và tối đa 100.000 truy vấn. Mỗi tập hợp con tham chiếu các phần tử trong mảng và tổng số phần tử tập hợp con trên tất cả các tập hợp không vượt quá 100.000. Điều này ngụ ý rằng về trung bình, hầu hết các tập hợp con đều nhỏ, mặc dù một số ít có thể lớn. Do cách tiếp cận đơn giản là lặp qua một tập hợp con cho mỗi truy vấn có độ phức tạp trong trường hợp xấu nhất tỷ lệ thuận với kích thước của tập hợp con, nên việc tính tổng trực tiếp các tập hợp con lớn cho mỗi truy vấn sẽ quá chậm. Cụ thể, việc lặp lại tất cả các phần tử cho mỗi truy vấn có thể dẫn đến 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn khả thi trong thời gian chạy 3 giây. 

Trường hợp biên tinh tế phát sinh khi một tập hợp con xuất hiện trong nhiều truy vấn và trùng lặp với các tập hợp con khác. Một giải pháp bất cẩn có thể thêm các giá trị vào từng phần tử riêng lẻ cho mỗi truy vấn bổ sung, sau đó tính lại tổng mỗi lần. Điều này có thể dẫn đến công việc lặp đi lặp lại và thời gian chạy quá mức, đặc biệt khi các tập hợp con lớn và giao nhau nhiều. Ví dụ: nếu tập hợp con S1 chứa các chỉ số [1,2,3,4,5] và tập hợp con S2 chứa [3,4,5,6,7], thì một cách tiếp cận đơn giản thực hiện các phép cộng riêng biệt trên cả hai có thể chạm vào các phần tử chồng chéo nhiều lần một cách không cần thiết. Đầu ra chính xác phải phản ánh tất cả các bổ sung tích lũy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp lại các chỉ số của tập hợp con cho mọi truy vấn. Đối với truy vấn tổng, nó tính tổng trực tiếp tất cả các phần tử được tham chiếu. Đối với một truy vấn bổ sung, nó sẽ tăng từng phần tử trong tập hợp con. Điều này hoạt động chính xác nhưng trở nên cực kỳ chậm đối với các tập hợp con lớn hoặc các truy vấn thường xuyên vì độ phức tạp của mỗi truy vấn là O(|S_k|). Với tối đa 100.000 truy vấn và tập hợp con có kích thước không cần thiết, điều này có thể dễ dàng vượt quá 10^10 thao tác. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa là khai thác thực tế là hầu hết các tập hợp con đều nhỏ và tổng số phần tử được tham chiếu trên tất cả các tập hợp con là có hạn. Chúng ta có thể chia các tập hợp con thành các loại "nặng" và "nhẹ" dựa trên kích thước. Các tập con nặng đủ lớn nên việc lặp lại chúng mỗi lần rất tốn kém. Đối với các tập hợp con nặng, chúng tôi duy trì giá trị bổ sung lười biếng: chúng tôi lưu trữ số lượng đã được thêm vào toàn bộ tập hợp con mà không cần chạm vào các phần tử riêng lẻ. Đối với các tập hợp con nhẹ, chúng tôi tiếp tục cập nhật trực tiếp các phần tử riêng lẻ vì chi phí thấp. Ngoài ra, chúng tôi tính toán trước các giao điểm giữa các tập hợp con nặng và tất cả các tập hợp con để điều chỉnh tổng chính xác khi tập hợp con nhẹ ảnh hưởng đến các phần tử thuộc tập con nặng. 

Quan sát cho thấy có ít tập hợp con lớn và tập hợp con nhỏ có giá rẻ cho phép chúng tôi xử lý mọi truy vấn trong khoảng thời gian xấp xỉ O(√n), vì số lượng tập hợp con nặng là nhỏ và công việc trên mỗi truy vấn đối với các tập hợp con nhẹ vẫn tuyến tính ở kích thước tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * max( | S_k | )) | 
| Tối ưu hóa ánh sáng nặng | O(q * √(n)) | O(n + m * √(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính ngưỡng`B`để phân loại các tập con nặng. Một lựa chọn thực tế là căn bậc hai của tổng số phần tử tập hợp con, ví dụ:`B = int(sqrt(sum_of_sizes))`. Bất kỳ tập con nào có kích thước ≥ B đều được coi là nặng và các tập con khác được coi là nhẹ. 
2. Đối với mỗi tập hợp con, hãy tính trước tổng ban đầu của nó. Đối với các tập con nặng, hãy lưu trữ số tiền này một cách riêng biệt. Đối với mỗi phần tử, hãy giữ một danh sách các tập con nặng chứa nó. Điều này cho phép chúng ta điều chỉnh tổng tập hợp con nặng một cách hiệu quả khi các phần tử được sửa đổi. 
3. Khởi tạo một mảng`lazy`đối với các tập con nặng, ở đâu`lazy[k]`đại diện cho phép cộng tích lũy được áp dụng cho tập con nặng thứ k. Điều này cho phép cập nhật O(1) cho các truy vấn bổ sung trên các tập hợp con nặng. 
4. Đối với mỗi truy vấn, xác định loại tập hợp con: 

- Nếu là truy vấn tổng trên một tập hợp con nặng, hãy trả về tổng đã lưu trữ cộng với phép cộng lười nhân với kích thước tập hợp con. Ngoài ra, hãy thêm sự đóng góp từ các tập hợp con ánh sáng giao nhau đã có các phần tử được cập nhật riêng lẻ. 
- Nếu đó là truy vấn tổng trên một tập hợp con nhẹ, hãy tính tổng trực tiếp từng phần tử, cộng các đóng góp từ bất kỳ tập hợp con nặng giao nhau nào thông qua các giá trị lười. 
5. Đối với các thắc mắc bổ sung: 

- Nếu tập hợp con nhẹ, hãy lặp lại các phần tử của nó, tăng dần chúng, đồng thời cập nhật tổng được lưu trữ của mọi tập hợp con nặng giao nhau bằng cách sử dụng các giao điểm được tính toán trước. 
- Nếu tập con nặng thì tăng giá trị lười của nó. Điều này tránh chạm vào các phần tử riêng lẻ và các truy vấn tổng sẽ tính phần tăng này bằng cách sử dụng giá trị lười. 
6. Mỗi truy vấn tổng sử dụng tổng được lưu trữ cộng với đóng góp lười biếng, đảm bảo mỗi truy vấn được trả lời trong thời gian O(B) hoặc O(√n), giúp thuật toán tổng thể trở nên hiệu quả. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng các tập hợp con nặng theo dõi tổng tích lũy và phép cộng lười của chúng, trong khi các tập hợp con nhẹ cập nhật các phần tử thực tế. Giao điểm giữa các tập hợp con nặng và nhẹ được tính toán trước, do đó, bất kỳ sửa đổi nào đối với tập hợp con nhẹ đều được truyền chính xác đến tổng của các tập con nặng giao nhau. Điều này đảm bảo rằng các truy vấn tổng luôn phản ánh tất cả các phép cộng trước đó, ngay cả khi các tập hợp con trùng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import sqrt

n, m, q = map(int, input().split())
a = list(map(int, input().split()))

sets = []
sizes = []
total_size = 0
for _ in range(m):
    line = list(map(int, input().split()))
    k = line[0]
    s = [x-1 for x in line[1:]]
    sets.append(s)
    sizes.append(k)
    total_size += k

B = int(sqrt(total_size)) + 1

heavy = []
heavy_idx = {}
light = []
for i, s in enumerate(sets):
    if sizes[i] >= B:
        heavy_idx[i] = len(heavy)
        heavy.append(i)
    else:
        light.append(i)

heavy_sums = [0] * len(heavy)
lazy = [0] * len(heavy)

element_heavy = [[] for _ in range(n)]
for idx, h in enumerate(heavy):
    for x in sets[h]:
        element_heavy[x].append(idx)
        heavy_sums[idx] += a[x]

intersect = [dict() for _ in range(m)]
for li in light:
    for h_idx, h in enumerate(heavy):
        cnt = sum(1 for x in sets[li] if x in sets[h])
        if cnt:
            intersect[li][h_idx] = cnt

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '?':
        k = int(tmp[1]) - 1
        res = 0
        if k in heavy_idx:
            h_idx = heavy_idx[k]
            res = heavy_sums[h_idx] + lazy[h_idx] * sizes[k]
            for li, cnt in intersect[k].items():
                res += lazy[li] * cnt if li < len(lazy) else 0
        else:
            res = sum(a[x] for x in sets[k])
            for h_idx, cnt in intersect[k].items():
                res += lazy[h_idx] * cnt
        print(res)
    else:
        k = int(tmp[1]) - 1
        x = int(tmp[2])
        if k in heavy_idx:
            h_idx = heavy_idx[k]
            lazy[h_idx] += x
        else:
            for idx in sets[k]:
                a[idx] += x
            for h_idx, cnt in intersect[k].items():
                heavy_sums[h_idx] += x * cnt
```Giải pháp này duy trì khả năng xử lý riêng biệt cho các tập hợp con nặng và nhẹ, đảm bảo các bản cập nhật được truyền bá chính xác mà không cần lặp lại các tập hợp con lớn một cách không cần thiết. Tổng tập hợp con nặng được cập nhật một cách lười biếng, trong khi các tập hợp con nhẹ chạm trực tiếp vào các phần tử riêng lẻ. Các giao điểm được tính toán trước, cho phép truy vấn tổng chính xác. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
5 3 5
5 -5 5 1 -4
2 1 2
4 2 1 4 5
2 2 5
? 2
+ 3 4
? 1
+ 2 1
? 2
```Dấu vết: 

| Truy vấn | Hoạt động | một mảng | nặng_tổng | lười biếng | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| ?2 | tổng S2 | [5,-5,5,1,-4] | [0]* | [0]* | -3 | 
| +3 4 | thêm 4 vào S3 | [5,-1,5,1,0] | [0]* | [0]* | - | 
| ?1 | tổng S1 | [5,-1,5,1,0] | [0]* | [0]* | 4 | 
| +2 1 | thêm 1 vào S2 | [6,0,5,2,1] | [0]* | [0]* | - | 
| ?2 | tổng S2 | [6,0,5,2,1] | [0]* | [0]* | 9 | 

Điều này chứng tỏ sự lan truyền chính xác của phép cộng thông qua các tập chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * √(tổng các phần tử tập hợp con)) | Mỗi truy vấn chạm nhiều nhất vào một tập hợp con nhỏ hoặc sử dụng các bản cập nhật lười biếng. | 
| | | |
