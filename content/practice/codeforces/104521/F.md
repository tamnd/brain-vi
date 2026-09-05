---
title: "CF 104521F - Cây ngũ cốc III"
description: "Thất bại này không phải là thuật toán. Nó hoàn toàn là một lỗi phân tích cú pháp đầu vào. Chương trình giả định mọi đầu vào kiểm tra đều bắt đầu bằng: Nhưng đầu vào không thành công là: Vì vậy, mã thông báo đầu tiên đã là một phần của trường hợp kiểm thử chứ không phải số lượng trường hợp kiểm thử."
date: "2026-06-30T10:23:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104521
codeforces_index: "F"
codeforces_contest_name: "CerealCodes II Novice"
rating: 0
weight: 104521
solve_time_s: 196
verified: false
draft: false
---

[CF 104521F - Cây ngũ cốc III](https://codeforces.com/problemset/problem/104521/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** không 

##Giải pháp 
###Chẩn đoán 

Thất bại này không phải là thuật toán. Đây hoàn toàn là **lỗi phân tích cú pháp đầu vào**. 

Chương trình giả định mọi đầu vào kiểm tra đều bắt đầu bằng:```
t
```Nhưng đầu vào không thành công là:```
34 27 38 6
```Vì vậy, mã thông báo đầu tiên đã là một phần của trường hợp thử nghiệm chứ không phải số lượng trường hợp thử nghiệm. 

Điều đó có nghĩa là định dạng thực sự trong thẩm phán này là: 

> nhiều trường hợp thử nghiệm được đưa ra cùng dòng mà không có phần đầu`t`Mỗi test case chỉ là một cặp`(n, k)`. 

Vì vậy, quy tắc phân tích cú pháp chính xác là: 

- Đọc tất cả các số nguyên từ đầu vào 
- Giải thích chúng thành cặp liên tiếp 

Giải pháp trước đó đã làm sai:```python
t = int(input())
```mà phá vỡ ngay lập tức. 

### Nguyên nhân cốt lõi 

Có hai định dạng không tương thích: 

| Định dạng giả định | Định dạng thực tế | 
| --- | --- | 
| số đầu tiên là t | đầu vào là một chuỗi các cặp phẳng | 
| t dòng theo dõi | không rõ ràng t | 

Vì vậy, sự cố xảy ra trước khi bất kỳ logic nào được thực thi. 

### Giải thích đúng 

Chúng ta nên phân tích như thế này:```
(n1, k1), (n2, k2), (n3, k3) ...
```cho đến khi đầu vào kết thúc. 

### Thuật toán đúng (logic không thay đổi) 

Chúng tôi vẫn sử dụng logic xây dựng tương tự: 

- sự không phù hợp đóng góp 2 
- chúng tôi tham lam chỉ định các cặp một cách an toàn 
- kiểm tra tính khả thi vẫn giữ nguyên ý tưởng 

Chỉ thay đổi xử lý đầu vào. 

## Giải pháp Python đúng```python
import sys

def solve_case(n, k):
    if k % 2 == 1 or k > n:
        return None

    need = k // 2
    s = ['0'] * n

    l, r = 0, n - 1

    while l < r:
        remaining_pairs = (r - l + 1) // 2

        if need > 0 and remaining_pairs >= need:
            s[l] = '0'
            s[r] = '1'
            need -= 1
        else:
            s[l] = s[r] = '0'

        l += 1
        r -= 1

    if need != 0:
        return None

    return "".join(s)

def main():
    data = list(map(int, sys.stdin.buffer.read().split()))
    out = []
    i = 0

    while i < len(data):
        n = data[i]
        k = data[i + 1]
        i += 2

        res = solve_case(n, k)
        if res is None:
            out.append("NO")
        else:
            out.append("YES")
            out.append(res)

    print("\n".join(out))

if __name__ == "__main__":
    main()
```### Tại sao cách này khắc phục được sự cố 

Thất bại cốt lõi là giả định số lượng bài kiểm tra hàng đầu. Việc loại bỏ giả định đó làm cho trình phân tích cú pháp phù hợp với định dạng đánh giá thực tế. 

Sau khi phân tích cú pháp được sửa, logic xây dựng sẽ hoạt động theo đúng mục đích`(n, k)`cặp và không còn đọc sai:```
34 27 38 6
```như một hình dạng không đúng`t`. 

### Bài học chính 

Khi một giải pháp gặp sự cố tại`int(input())`trên đầu vào nhiều số, vấn đề hầu như luôn luôn là: 

> giả định sai về cấu trúc đầu vào, không đúng về mặt thuật toán 

Ở đây, bản sửa lỗi đã chuyển sang trình phân tích cú pháp luồng số nguyên thô.
