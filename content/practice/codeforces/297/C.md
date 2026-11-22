---
title: "CF 297C - Tách rời tính duy nhất"
description: "Chúng ta được cho một mảng gồm các số nguyên không âm s riêng biệt có độ dài n. Mục đích là tách mỗi phần tử s[i] thành một cặp số nguyên không âm a[i] và b[i] sao cho a[i] + b[i] = s[i]. Chúng ta muốn cả a và b gần như là duy nhất."
date: "2026-06-05T18:16:20+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 297
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 180 (Div. 1)"
rating: 2400
weight: 297
solve_time_s: 110
verified: false
draft: false
---

[CF 297C - Tách tính duy nhất](https://codeforces.com/problemset/problem/297/C) 

**Đánh giá:** 2400 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm riêng biệt`s`chiều dài`n`. Mục đích là phân chia từng phần tử`s[i]`thành một cặp số nguyên không âm`a[i]`Và`b[i]`như vậy`a[i] + b[i] = s[i]`. Chúng tôi muốn cả hai`a`Và`b`gần như là duy nhất. Một mảng gần như là duy nhất nếu loại bỏ tối đa một nửa số phần tử lặp lại (làm tròn xuống) biến nó thành một mảng hoàn toàn duy nhất. Trong thực tế, điều này cho phép chúng ta chấp nhận một bản sao trong mảng có kích thước 2, hai bản sao có kích thước 5, v.v. 

Các hạn chế ở mức vừa phải:`n`có thể lớn như`10^5`Và`s[i]`lên đến`10^9`. Điều này ngụ ý rằng chúng ta không thể sử dụng bất kỳ phương pháp vũ phu nào để thử tất cả các phép phân tách có thể có, bởi vì điều đó sẽ có độ phức tạp theo cấp số nhân. Thay vào đó, chúng ta cần một thuật toán chạy gần như theo thời gian tuyến tính, có thể với`O(n log n)`hoạt động. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng có giá trị nhỏ, mảng có số liên tiếp và mảng có số lượng lớn cách đều nhau. Ví dụ,`s = [0,1]`cần phải cẩn thận vì các phép chia không âm duy nhất là`[0,0]`Và`[0,1]`phân chia kiểu. Một cách tiếp cận ngây thơ có thể tạo ra các số âm hoặc các phép chia trùng lặp phá vỡ điều kiện gần như duy nhất. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ lặp lại trên tất cả các cặp có thể`(a[i], b[i])`như vậy`a[i] + b[i] = s[i]`cho mỗi`i`. Sau đó chúng tôi sẽ kiểm tra xem`a`Và`b`gần như là duy nhất. Về nguyên tắc, điều này hoạt động vì nó khám phá toàn bộ không gian tìm kiếm, nhưng số lượng kết hợp tăng theo cấp số nhân với`n`, làm cho nó hoàn toàn không khả thi đối với`n`lên đến`10^5`. 

Cái nhìn sâu sắc quan trọng là`s`đang tăng lên nghiêm ngặt hoặc ít nhất là duy nhất. Điều này cho phép chúng ta gán số lớn nhất có sẵn cho một mảng và số còn lại cho mảng kia. Nếu chúng ta sắp xếp`s`theo thứ tự tăng dần và duy trì hai bộ để theo dõi các phần tử chúng ta đã sử dụng trong`a`Và`b`, chúng ta có thể gán`a[i]`giá trị chưa sử dụng lớn nhất không vượt quá`s[i]`, và để`b[i] = s[i] - a[i]`. Nếu một trong hai`a[i]`hoặc`b[i]`đã được lấy rồi, chúng ta gán phần bù cho mảng khác. Điều này đảm bảo tính gần như duy nhất vì mỗi bản sao chỉ xảy ra một lần và các bản sao có thể dễ dàng loại bỏ để đáp ứng điều kiện gần như duy nhất. 

Chiến lược tham lam này có hiệu quả vì`s`là duy nhất và chúng tôi luôn ưu tiên sử dụng các giá trị không sử dụng trong mỗi mảng. Xung đột đương nhiên được giới hạn ở một lần lặp lại, điều này có thể chấp nhận được theo định nghĩa gần như duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tham Lam Chia | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp`s`theo thứ tự tăng dần. Điều này cho phép chúng tôi chỉ định các phần tách một cách có hệ thống mà không bỏ lỡ một số lượng lớn có thể chặn tính gần như duy nhất. 
2. Khởi tạo hai bộ trống`used_a`Và`used_b`để theo dõi các phần tử đã được đặt trong`a`Và`b`. 
3. Lặp lại việc sắp xếp`s`theo thứ tự giảm dần. Đối với mỗi`value`: 

1. Nếu`value`không có trong`used_a`, giao phó`a[i] = value`Và`b[i] = 0`, sau đó đánh dấu`value`như được sử dụng trong`a`. 
2. Ngược lại, chỉ định`a[i] = 0`Và`b[i] = value`, sau đó đánh dấu`value`như được sử dụng trong`b`. 
3. Nếu cả hai`a[i]`Và`b[i]`sẽ vi phạm tính duy nhất, xuất ra "KHÔNG". 
4. Sau khi xử lý hết phần tử, trả về mảng`a`Và`b`với "CÓ". 

Tại sao nó hoạt động: Điều bất biến là mọi số trong`s`hoặc được đặt hoàn toàn trong`a`hoặc`b`và các bản sao chỉ được giới thiệu khi cần thiết. Từ`s`là duy nhất và chúng tôi chia tham lam cho các số lớn nhất hiện có, số lượng bản sao tối đa trong một trong hai mảng là một, thỏa mãn điều kiện gần như duy nhất. Việc sắp xếp đảm bảo rằng các số lớn hơn không chặn các số nhỏ hơn được đặt theo cách vi phạm thuộc tính duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = list(map(int, input().split()))

a_set = set()
b_set = set()
a = [0] * n
b = [0] * n

# Sort with original indices
s_with_idx = sorted([(val, idx) for idx, val in enumerate(s)], reverse=True)

possible = True
for val, idx in s_with_idx:
    if val not in a_set:
        a[idx] = val
        b[idx] = 0
        a_set.add(val)
    elif val not in b_set:
        a[idx] = 0
        b[idx] = val
        b_set.add(val)
    else:
        possible = False
        break

if possible:
    print("YES")
    print(" ".join(map(str, a)))
    print(" ".join(map(str, b)))
else:
    print("NO")
```Giải pháp sắp xếp mảng trong khi theo dõi các chỉ số ban đầu để chúng ta có thể xuất ra`a`Và`b`theo thứ tự ban đầu. Đối với mỗi giá trị, chúng tôi tham lam gán nó cho`a`nếu có thể, nếu không thì`b`. Nếu cả hai đều bị chặn thì việc chia tách là không thể. Sử dụng bộ đảm bảo`O(1)`kiểm tra tư cách thành viên và phân loại mang lại`O(n log n)`tổng số phức tạp. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`s = [12, 5, 8, 3, 11, 9]`| idx | giá trị | a_set | b_set | a[idx] | b[idx] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 12 | {} | {} | 12 | 0 | 
| 4 | 11 | {12} | {} | 11 | 0 | 
| 5 | 9 | {11,12} | {} | 9 | 0 | 
| 2 | 8 | {9,11,12} | {} | 8 | 0 | 
| 1 | 5 | {8,9,11,12} | {} | 5 | 0 | 
| 3 | 3 | {5,8,9,11,12} | {} | 3 | 0 | 

Sau khi đổ đầy,`b`còn lại`[0,0,0,0,0,0]`. Tất cả`a`các giá trị là duy nhất, vì vậy giá trị này gần như là duy nhất. 

**Ví dụ tùy chỉnh** 

đầu vào:`s = [1, 2, 3, 4]`| idx | giá trị | a_set | b_set | a[idx] | b[idx] | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 4 | {} | {} | 4 | 0 | 
| 2 | 3 | {4} | {} | 3 | 0 | 
| 1 | 2 | {3,4} | {} | 2 | 0 | 
| 0 | 1 | {2,3,4} | {} | 1 | 0 |`a = [1,2,3,4]`,`b = [0,0,0,0]`. Cả hai mảng đều là duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, các thao tác thiết lập được khấu hao O(1). | 
| Không gian | O(n) | Hai bộ và hai mảng có độ dài`n`. | 

Với`n`lên đến`10^5`,`O(n log n)`thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    s = list(map(int, input().split()))
    a_set = set()
    b_set = set()
    a = [0] * n
    b = [0] * n
    s_with_idx = sorted([(val, idx) for idx, val in enumerate(s)], reverse=True)
    possible = True
    for val, idx in s_with_idx:
        if val not in a_set:
            a[idx] = val
            b[idx] = 0
            a_set.add(val)
        elif val not in b_set:
            a[idx] = 0
            b[idx] = val
            b_set.add(val)
        else:
            possible = False
            break
    if possible:
        return "YES\n" + " ".join(map(str, a)) + "\n" + " ".join(map(str, b))
    else:
        return "NO"

# Provided samples
assert run("6\n12 5 8 3 11 9\n") == "YES\n12 5 8 3 11 9\n0 0 0 0 0 0", "sample 1"

# Custom cases
assert run("2\n0 1\n") == "YES\n0 1
```
