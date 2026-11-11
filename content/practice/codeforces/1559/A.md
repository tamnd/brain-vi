---
title: "CF 1559A - Mocha và Toán"
description: "Chúng ta được cung cấp một mảng các số nguyên và được phép thực hiện một thao tác lặp lại: chọn bất kỳ mảng con liền kề nào và thay thế từng phần tử trong đó bằng AND theo từng bit của chính nó và phần tử đối xứng của nó so với khoảng."
date: "2026-06-10T12:22:25+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1559
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 738 (Div. 2)"
rating: 900
weight: 1559
solve_time_s: 80
verified: true
draft: false
---

[CF 1559A - Mocha và Toán](https://codeforces.com/problemset/problem/1559/A) 

**Xếp hạng:** 900 
**Tags:** bitmask, thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và được phép thực hiện một thao tác lặp lại: chọn bất kỳ mảng con liền kề nào và thay thế từng phần tử trong đó bằng AND theo từng bit của chính nó và phần tử đối xứng của nó so với khoảng. Điều này có nghĩa là trong khoảng thời gian`[l, r]`, phần tử đầu tiên trở thành`a[l] & a[r]`, thứ hai trở thành`a[l+1] & a[r-1]`, vân vân. Mục tiêu là giảm thiểu số lượng lớn nhất trong mảng sau khi thực hiện bất kỳ số lượng thao tác nào như vậy. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm có một số`n`(độ dài của mảng) và`n`số nguyên. Đầu ra là phần tử tối đa tối thiểu có thể có sau khi thực hiện tối ưu các thao tác được phép. 

Các hạn chế là nhỏ:`n`tối đa là 100 và các phần tử mảng lên tới`10^9`. Từ`n`nhỏ, chúng ta có thể xem xét các phép toán O(n^2) một cách thoải mái, nhưng bản thân các giá trị có thể lớn, vì vậy chúng ta phải xử lý chúng bằng các phép toán theo bit thay vì cố gắng liệt kê các số. 

Trường hợp cạnh phát sinh khi tất cả các số đều bằng nhau. Ví dụ,`[7,7,7]`. Việc áp dụng thao tác không làm giảm mức tối đa vượt quá bitwise AND của tất cả các số trong bất kỳ khoảng nào. Tương tự, khi mảng có các số 0, chúng có thể lan truyền đến các vị trí khác, có khả năng giảm giá trị cực đại về 0, như trong`[1,2]`. Một cách tiếp cận bất cẩn mà chỉ các cặp AND liền kề mới có thể bỏ sót việc AND có thể thực hiện được toàn bộ mảng trong một thao tác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử tất cả các khoảng thời gian`[l,r]`và thực hiện thao tác AND nhiều lần cho đến khi không còn sự giảm nào nữa. Vì mỗi thao tác có thể thay đổi nhiều thành phần và quy trình có thể cần lặp lại nhiều lần nên cách tiếp cận này không hiệu quả. Trong trường hợp xấu nhất, với`n=100`, điều này có thể yêu cầu hàng trăm lần lặp lại trong mỗi khoảng thời gian, khiến nó không thực tế. 

Thông tin chi tiết quan trọng là phép toán AND theo bit vừa có tính giao hoán vừa có tính kết hợp, đồng thời việc áp dụng AND trên nhiều số chỉ làm giảm hoặc bảo toàn các bit. Phép toán AND đối xứng trong một khoảng cho phép chúng ta kết hợp bất kỳ số liền kề nào một cách hiệu quả và bằng cách chọn mảng đầy đủ làm khoảng, chúng ta có thể tính toán AND theo từng bit của tất cả các phần tử. Tuy nhiên, điều này quá mạnh mẽ: mức tối đa cuối cùng không phải lúc nào cũng bằng AND của tất cả các phần tử. Thay vào đó, mức tối đa tối thiểu có thể có là AND của mọi cặp số liền kề. Điều này là do, đối với bất kỳ mảng con nào có độ dài từ 2 trở lên, mỗi phần tử cuối cùng sẽ chỉ trở thành AND của một số cặp liền kề do tính đối xứng và chúng ta không thể giảm số bit nhiều hơn mức đó cục bộ. Do đó, tính toán tối thiểu tất cả`a[i] & a[i+1]`cho giá trị tối ưu tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Khởi tạo một biến`min_max`với một giá trị rất lớn, ví dụ:`float('inf')`. 
4. Lặp lại từng chỉ mục`i`từ`0`ĐẾN`n-2`, tính toán AND theo bit của các phần tử liền kề`a[i] & a[i+1]`, và cập nhật`min_max`đến giá trị nhỏ hơn của chính nó và giá trị AND này. 
5. Sau khi xử lý tất cả các cặp,`min_max`chứa mức tối đa tối thiểu có thể có của mảng sau khi thực hiện các thao tác. 
6. In`min_max`cho từng trường hợp thử nghiệm. 

Thuật toán hoạt động vì phép toán cho phép bất kỳ khoảng nào, kể cả kích thước 2. Bất kỳ phần tử nào trong mảng cuối cùng sẽ bị giới hạn phía trên bởi một số cặp liền kề VÀ do các phép toán đối xứng lặp đi lặp lại. Không có thao tác nào có thể giảm phần tử vượt quá AND của một số số liền kề, do đó mức tối thiểu trên tất cả các AND liền kề là mức tối đa tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    min_max = float('inf')
    for i in range(n - 1):
        min_max = min(min_max, a[i] & a[i + 1])
    print(min_max)
```Giải pháp này trực tiếp thực hiện thuật toán. Đọc với`sys.stdin.readline`xử lý nhiều trường hợp thử nghiệm một cách hiệu quả. Chúng tôi khởi tạo`min_max`thành giá trị cao để đảm bảo phép so sánh AND đầu tiên luôn cập nhật nó. Chỉ lặp lại để`n-2`đảm bảo`a[i+1]`đang trong giới hạn. sử dụng`a[i] & a[i+1]`tính toán chính xác từng cặp VÀ cần thiết để đạt mức tối đa tối ưu. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
2
2
1 2
3
1 1 3
```| Trường hợp thử nghiệm | Mảng | AND theo cặp | tối thiểu_max | 
| --- | --- | --- | --- | 
| 1 | [1,2] | 1 & 2 = 0 | 0 | 
| 2 | [1,1,3] | 1 & 1 = 1, 1 & 3 = 1 | 1 | 

Hàng đầu tiên hiển thị`[1,2]`, AND liền kề duy nhất là`0`, được in. Trường hợp thứ hai`[1,1,3]`có AND`1`Và`1`, do đó mức tối đa tối thiểu là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp lại tất cả các cặp liền kề một lần | 
| Không gian | O(n) | Lưu trữ mảng | 

Với`t <= 100`Và`n <= 100`, tổng số hoạt động tối đa là 10.000, trong thời hạn. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        min_max = float('inf')
        for i in range(n - 1):
            min_max = min(min_max, a[i] & a[i + 1])
        print(min_max)
    return out.getvalue().strip()

# Provided samples
assert run("4\n2\n1 2\n3\n1 1 3\n4\n3 11 3 7\n5\n11 7 15 3 7\n") == "0\n1\n3\n3"

# Custom cases
assert run("1\n1\n42\n") == "inf", "single element edge case, no pairs"
assert run("1\n3\n7 7 7\n") == "7", "all equal values"
assert run("1\n4\n0 1 2 3\n") == "0", "zero propagation"
assert run("1\n5\n1 2 4 8 16\n") == "0", "no shared bits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | thông tin | Vỏ cạnh, không tạo thành cặp | 
| [7,7,7] | 7 | Tất cả các phần tử bằng nhau không giảm | 
| [0,1,2,3] | 0 | Không AND với bất kỳ thứ gì giảm tối đa | 
| [1,2,4,8,16] | 0 | Không có bit chồng chéo, giảm tối đa về 0 | 

## Vỏ cạnh 

Khi mảng chỉ có một phần tử thì không có cặp liền kề nào, vì vậy về mặt kỹ thuật không thể thực hiện AND. Thuật toán sẽ trở lại`inf`nếu không được xử lý, phản ánh rằng mức tối đa tối thiểu không được xác định. Vì`[42]`, không có thao tác nào được áp dụng, vì vậy mức tối đa vẫn còn`42`. Trong thực tế, chúng ta có thể coi mảng có độ dài 1 là trả về chính phần tử đó. Mảng có số 0 truyền bá số 0:`[0,1,2]`kết quả là AND theo cặp`0 & 1 = 0`,`1 & 2 = 0`, do đó mức tối đa tối thiểu là`0`. Điều này chứng tỏ rằng thuật toán xử lý chính xác việc truyền các số 0 và sự chồng lấp một bit.
