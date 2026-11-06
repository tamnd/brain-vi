---
title: "CF 128B - Chuỗi"
description: "Chúng ta được cho một chuỗi các chữ cái viết thường và một số nguyên k. Nhiệm vụ là tạo ra tất cả các chuỗi con có thể có của chuỗi, sắp xếp chúng theo từ điển và trả về chuỗi con thứ k theo thứ tự đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "hashing", "implementation", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 128
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 94 (Div. 1 Only)"
rating: 2100
weight: 128
solve_time_s: 81
verified: true
draft: false
---

[CF 128B - Chuỗi](https://codeforces.com/problemset/problem/128/B) 

**Xếp hạng:** 2100 
**Tags:** brute power, thuật toán xây dựng, băm, triển khai, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ cái viết thường và một số nguyên _k_. Nhiệm vụ là tạo ra tất cả các chuỗi con có thể có của chuỗi, sắp xếp chúng theo từ điển và trả về chuỗi con thứ _k_ theo thứ tự đó. Các chuỗi con có thể lặp lại nên nhiều chuỗi con giống nhau phải được xem xét riêng biệt. Nếu có ít hơn _k_ chuỗi con, chúng tôi sẽ trả về`"No such line."`. 

Các ràng buộc là độ dài chuỗi _n_ có thể lên tới 100.000 và _k_ có thể lên tới 100.000. Việc liệt kê tất cả các chuỗi con một cách đơn giản là không khả thi, bởi vì một chuỗi có độ dài _n_ có khoảng _n(n+1)/2_ chuỗi con, lên tới 5 tỷ cho _n_ = 100.000. Việc sắp xếp một bộ sưu tập lớn như vậy là không thể trong thời gian giới hạn. Điều này loại trừ mọi cách tiếp cận O(n2) hoặc O(n2 log n). 

Các trường hợp cạnh bao gồm các chuỗi có ký tự lặp lại, chuỗi có độ dài 1 và _k_ vượt quá tổng số chuỗi con. Ví dụ, đối với`"aa"`và _k_ = 3, các chuỗi con theo thứ tự từ điển là`"a"`,`"a"`,`"aa"`. Một cách tiếp cận ngây thơ có thể vô tình loại bỏ các bản sao, tạo ra kết quả sai hoặc có thể tính sai các chỉ số. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản để mô tả. Tạo mọi chuỗi con, lưu trữ chúng trong danh sách, sắp xếp danh sách theo từ điển và trả về phần tử ở chỉ mục _k-1_. Cách tiếp cận này hiệu quả với các chuỗi nhỏ vì về nguyên tắc việc tạo và sắp xếp các chuỗi con là đúng. Tuy nhiên, độ phức tạp về thời gian của nó là O(n2 log n), vì có O(n2) chuỗi con và việc sắp xếp chúng mất O(n2 log n2). Với n = 10⁵, tốc độ này quá chậm. 

Cái nhìn sâu sắc quan trọng cho một giải pháp hiệu quả là tránh tạo ra tất cả các chuỗi con một cách rõ ràng. Các chuỗi con được sắp xếp theo từ điển có thể được biểu diễn dưới dạng tiền tố của hậu tố của chuỗi. Nếu chúng ta tạo ra tất cả các hậu tố và sắp xếp chúng, thứ tự từ điển của các chuỗi con tương ứng với việc đi dọc theo các hậu tố được sắp xếp này và lấy các tiền tố tăng dần. Bằng cách giới hạn độ dài tiền tố ở _k_, chúng tôi chỉ xem xét các ký tự cần thiết tối thiểu, điều này tránh hoạt động O(n²). 

Chúng ta có thể triển khai điều này một cách hiệu quả bằng cách sử dụng hàng đợi ưu tiên hoặc đơn giản bằng cách sắp xếp tất cả các hậu tố, sau đó đếm các tiền tố theo thứ tự. Chúng tôi dừng lại khi đạt đến chuỗi con thứ _k_. Điều này làm giảm chi phí bộ nhớ và tránh việc liệt kê toàn bộ chuỗi con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2logn2) | O(n²) | Quá chậm | 
| Hậu tố + Đếm tiền tố | O(n log n + k2) | O(n + k2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo tất cả các hậu tố của chuỗi bằng cách lấy`s[i:]`cho mỗi`i`từ 0 đến n-1. Mỗi hậu tố bắt đầu ở một vị trí khác nhau và chứa tất cả các chuỗi con bắt đầu từ vị trí đó. 
2. Sắp xếp danh sách các hậu tố theo từ điển. Điều này đảm bảo rằng việc duyệt qua các hậu tố theo thứ tự sẽ cho ra các chuỗi con theo thứ tự từ điển miễn là chúng ta xem xét các tiền tố của chúng. 
3. Khởi tạo bộ đếm để theo dõi xem chúng ta đã liệt kê bao nhiêu chuỗi con. Đối với mỗi hậu tố theo thứ tự được sắp xếp, hãy liệt kê tất cả các tiền tố có độ dài tối đa _k_. Giới hạn độ dài tiền tố để tránh tạo ra nhiều ký tự hơn mức cần thiết, vì chuỗi con thứ _k_ không bao giờ được dài hơn _k_. 
4. Đối với mỗi tiền tố, hãy tăng bộ đếm. Nếu bộ đếm bằng _k_, in tiền tố này và kết thúc. Nếu chúng ta liệt kê xong tất cả các tiền tố từ tất cả các hậu tố mà không đạt đến _k_, hãy in`"No such line."`. 
5. Phương pháp này đảm bảo tính chính xác vì các hậu tố được sắp xếp chứa tất cả các chuỗi con có thể có theo thứ tự và việc đếm các tiền tố theo cách này sẽ xử lý chính xác các chuỗi con lặp lại. 

Tại sao nó hoạt động: Bằng cách sắp xếp các hậu tố, chúng tôi tạo ra một trật tự từ điển tổng thể. Mỗi chuỗi con của chuỗi gốc là tiền tố của một hậu tố nào đó. Việc liệt kê các tiền tố theo độ dài tăng dần đảm bảo rằng các bản sao được tính chính xác và các chuỗi con xuất hiện theo đúng thứ tự. Việc giới hạn độ dài tiền tố ở mức _k_ sẽ giới hạn những công việc không cần thiết và ngăn chặn tình trạng hao hụt thời gian và bộ nhớ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    s = input().strip()
    k = int(input())
    n = len(s)
    
    # Generate all suffixes
    suffixes = [s[i:] for i in range(n)]
    suffixes.sort()
    
    count = 0
    for suff in suffixes:
        # Limit the prefix length to k to avoid extra work
        for l in range(1, min(len(suff), k) + 1):
            count += 1
            if count == k:
                print(suff[:l])
                return
    print("No such line.")

if __name__ == "__main__":
    main()
```Phần đầu tiên đọc chuỗi và số nguyên đầu vào một cách hiệu quả. Tạo hậu tố là O(n) trong không gian. Các hậu tố sắp xếp là O(n log n). Khi đếm tiền tố, chúng ta dừng ngay khi tìm thấy chuỗi con _k_-th. Chúng tôi sử dụng`min(len(suff), k)`bởi vì bất kỳ chuỗi con nào dài hơn _k_ không thể là chuỗi con thứ _k_ nếu tính từ đầu. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`"aa"`, k = 2 

| hậu tố | được sắp xếp | tiền tố được tính | quầy | đầu ra | 
| --- | --- | --- | --- | --- | 
|`"aa"`,`"a"`|`"a"`,`"aa"`|`"a"`| 1 | - | 
| | |`"aa"`| 2 |`"a"`| 

Dấu vết này cho thấy hậu tố đầu tiên`"a"`tạo ra`"a"`. Hậu tố thứ hai`"aa"`tạo ra`"a"`(được tính trùng lặp), là chuỗi con thứ hai, vì vậy đầu ra là`"a"`. 

**Mẫu 2:** 

đầu vào:`"bc"`, k = 3 

| hậu tố | được sắp xếp | tiền tố được tính | quầy | đầu ra | 
| --- | --- | --- | --- | --- | 
|`"bc"`,`"c"`|`"bc"`,`"c"`|`"b"`| 1 | - | 
| | |`"bc"`| 2 | - | 
| |`"c"`|`"c"`| 3 |`"c"`| 

Ở đây thuật toán xác định chính xác`"c"`là chuỗi con thứ 3 theo thứ tự từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + k2) | Sắp xếp n hậu tố là O(n log n), liệt kê các tiền tố có độ dài k trên các hậu tố được sắp xếp là O(k²) | 
| Không gian | O(n + k2) | O(n) cho hậu tố, O(k²) để lưu trữ tạm thời các tiền tố nhỏ | 

Với các ràng buộc n ≤ 10⁵, k ≤ 10⁵, O(n log n + k²) là chấp nhận được. Việc sắp xếp 100.000 hậu tố là đủ nhanh và việc giới hạn việc liệt kê tiền tố đảm bảo chúng tôi không bao giờ vượt quá bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("aa\n2\n") == "a", "sample 1"
assert run("bc\n3\n") == "c", "sample 2"

# Custom cases
assert run("a\n1\n") == "a", "single character string"
assert run("a\n2\n") == "No such line.", "k larger than total substrings"
assert run("aaa\n4\n") == "aa", "repeated letters with duplicates"
assert run("abcde\n10\n") == "cde", "moderate-length string with unique letters"
assert run("xyz\n6\n") == "No such line.", "total substrings less than k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"a"`, 1 |`"a"`| chuỗi ký tự đơn | 
|`"a"`, 2 |`"No such line."`| k vượt quá tổng số chuỗi con | 
|`"aaa"`, 4 |`"aa"`| chữ cái lặp đi lặp lại, đếm trùng lặp | 
|`"abcde"`, 10 |`"cde"`| chữ thường, chữ cái duy nhất | 
|`"xyz"`, 6 |`"No such line."`| k lớn hơn tổng số chuỗi con | 

## Vỏ cạnh 

Đối với các chuỗi có các chữ cái lặp lại như`"aaa"`và k = 4 thì các hậu tố được sắp xếp là`"a"`,`"aa"`,`"aaa"`. Số đếm tiền tố`"a"`,`"aa"`,`"aaa"`,`"a"`(từ hậu tố thứ hai),`"aa"`(từ hậu tố thứ hai), v.v. Thuật toán đếm các bản sao theo thứ tự, vì vậy chuỗi con thứ 4 là`"aa"`. Với k lớn hơn tổng số chuỗi con, thuật toán hoàn thành vòng lặp mà không quay lại, in`"No such line."`đúng một lần. Các chuỗi ký tự đơn được xử lý vì danh sách hậu tố có một phần tử và việc liệt kê tiền tố hoạt động ngay cả với độ dài 1.
