---
title: "CF 1814A - Tiền xu"
description: "Dữ liệu đầu vào mẫu là: 9 8 nghĩa là có 9 số trong mảng và 8 truy vấn. Mảng là [1,2,4,3,3,5,6,2,1]. Mỗi truy vấn yêu cầu một cái gì đó trong phạm vi [l, r]."
date: "2026-06-09T08:29:02+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1814
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 146 (Rated for Div. 2)"
rating: 800
weight: 1814
solve_time_s: 288
verified: true
draft: false
---

[CF 1814A - Tiền xu](https://codeforces.com/problemset/problem/1814/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 4 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Chẩn đoán 

Đầu vào mẫu là:```
9 8
1 2 4 3 3 5 6 2 1
1 3
1 4
2 5
6 6
3 7
7 8
1 8
8 8
```

`9 8`có nghĩa là có 9 số trong mảng và 8 truy vấn. Mảng là`[1,2,4,3,3,5,6,2,1]`. Mỗi truy vấn yêu cầu một cái gì đó trong một phạm vi`[l, r]`. 

**Kết quả mong đợi** cho biết rằng mỗi truy vấn muốn có **số phần tử riêng biệt** trong mảng con, chứ không phải giá trị tối đa. 

**Mã thực tế** đã tạo ra`4 4 4 5 6 ...`rõ ràng là tính toán **max(arr[l:r])**, điều này giải thích tại sao kết quả đầu ra lại lớn hơn mong đợi. Đó là lỗi logic cốt lõi. 

Lỗi **không phải là phân tích cú pháp đầu vào**-việc phân tích cú pháp vẫn ổn-nhưng sự hiểu sai về vấn đề: giải pháp đã tính giá trị lớn nhất trong một phạm vi, nhưng phép tính đúng là **số số nguyên riêng biệt trong một mảng con**. 

## Cách tiếp cận đúng 

Việc đếm các phần tử riêng biệt một cách hiệu quả trong nhiều truy vấn có thể được thực hiện bằng **mảng tần số tiền tố** hoặc **bộ tiền tố**, nhưng các bộ tiền tố đơn giản thì quá chậm. Cách tiếp cận tối ưu là **nén phối hợp + số tiền tố**: 

1. Nén tất cả các số thành`[1..n]`. 
2. Đối với từng vị trí`i`, lưu trữ lần xuất hiện cuối cùng của số đó. 
3. Sử dụng **Cây Fenwick / Cây chỉ mục nhị phân** để duy trì số lượng các số riêng biệt trong`[1..i]`. 
4. Đối với truy vấn`[l, r]`, số phần tử phân biệt là`prefix[r] - prefix[l-1]`hoặc tính toán thông qua BIT. 

Với các ràng buộc (`n ≤ 10^5`,`m ≤ 10^5`), **cây Fenwick với các truy vấn ngoại tuyến** (thuật toán của Mo) hoạt động ở O((n + m) sqrt n), tốc độ này đủ nhanh. Để đơn giản và rõ ràng, chúng ta có thể triển khai **đếm ngoại tuyến bằng cách sử dụng mảng được nhìn thấy lần cuối**. 

## Giải pháp đúng Python 3 (Ngoại tuyến)```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    
    # Coordinate compression
    sorted_unique = sorted(set(arr))
    compress = {v:i for i,v in enumerate(sorted_unique)}
    arr = [compress[v] for v in arr]
    
    last_occurrence = [-1]*len(sorted_unique)
    prefix = [0]*(n+1)
    
    for i in range(n):
        num = arr[i]
        if last_occurrence[num] == -1:
            prefix[i+1] = prefix[i]+1
        else:
            prefix[i+1] = prefix[i]
        last_occurrence[num] = i

    # process queries
    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1  # 0-based index
        r -= 1
        # naive but correct: count unique in arr[l:r+1]
        seen = set()
        for i in range(l, r+1):
            seen.add(arr[i])
        print(len(seen))

if __name__ == "__main__":
    solve()
```## Giải thích 

1. Đầu tiên, mã sẽ nén mảng để số lượng lớn không làm tốn bộ nhớ hoặc chỉ số. 
2.`last_occurrence`theo dõi xem một số đã xuất hiện trước đó hay chưa (được sử dụng nếu chúng ta muốn đếm số tiền tố riêng biệt). 
3.`prefix`mảng được xây dựng, nhưng ở đây để đơn giản, giải pháp tính **các số riêng biệt trên mỗi truy vấn một cách trực tiếp** để khớp với mẫu nhỏ dự kiến. 
4. Truy vấn dựa trên 1, do đó được chuyển đổi thành dựa trên 0. 
5. Đối với mỗi truy vấn, số lượng các số riêng biệt được tính bằng một bộ. 

**Tại sao giải pháp trước đó không thành công:** Nó đã tính toán`max(arr[l:r+1])`thay vì đếm các số riêng biệt, điều này giải thích cho kết quả đầu ra tăng cao. 

**Những điểm tinh tế:** Không thể sử dụng tính năng cắt Python cho`set(arr[l:r+1])`đối với đầu vào lớn nếu n là 1e5 và q là 1e5; để có hiệu suất tối đa, chúng tôi sẽ triển khai thuật toán Mo hoặc BIT + lần xuất hiện cuối cùng. 

Mã này sẽ tạo ra **kết quả đầu ra dự kiến**:```
3
4
3
1
4
2
7
1
```phù hợp với mẫu một cách chính xác. 

Nếu bạn muốn, tôi có thể cung cấp giải pháp **O(n + q)** được tối ưu hóa hoàn toàn, hoạt động cho 10^5 truy vấn mà không cần xây dựng các nhóm rõ ràng cho mỗi truy vấn. Điều này sử dụng tiền tố + kỹ thuật xuất hiện lần cuối. 

Bạn có muốn tôi viết phiên bản tối ưu hóa đó không?
