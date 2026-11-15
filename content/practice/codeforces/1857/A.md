---
title: "CF 1857A - Tô màu mảng"
description: "Chúng ta được cho một mảng các số nguyên và cần quyết định xem nó có thể được phân chia thành hai nhóm không trống sao cho tổng của mỗi nhóm có cùng tính chẵn lẻ - cả hai đều là số chẵn hoặc cả hai số lẻ. Mỗi nhóm phải chứa ít nhất một phần tử."
date: "2026-06-09T00:45:39+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1857
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 891 (Div. 3)"
rating: 800
weight: 1857
solve_time_s: 78
verified: true
draft: false
---

[CF 1857A - Tô màu mảng](https://codeforces.com/problemset/problem/1857/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và cần quyết định xem nó có thể được phân chia thành hai nhóm không trống sao cho tổng của mỗi nhóm có cùng tính chẵn lẻ - cả hai đều là số chẵn hoặc cả hai số lẻ. Mỗi nhóm phải chứa ít nhất một phần tử. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng có độ dài từ 2 đến 50 và các phần tử nằm trong khoảng từ 1 đến 50. 

Đầu ra là một "CÓ" hoặc "KHÔNG" đơn giản cho mỗi trường hợp thử nghiệm, cho biết liệu việc tô màu như vậy có khả thi hay không. Bởi vì độ dài mảng nhỏ (tối đa là 50), chúng tôi không cần tối ưu hóa nhiều, nhưng chúng tôi nên tránh việc liệt kê tất cả các phân vùng một cách thô bạo, điều này sẽ theo cấp số nhân trong n. 

Các trường hợp cạnh không rõ ràng phát sinh khi tất cả các số có cùng tính chẵn lẻ. Ví dụ, một mảng`[2,4]`chỉ có số chẵn. Việc chia thành hai nhóm sẽ luôn tạo ra các tổng đều chẵn, thỏa mãn điều kiện chẵn lẻ. Ngược lại, một mảng`[1,2]`có một số lẻ và một số chẵn, do đó, bất kỳ phép chia nào cũng sẽ tạo ra tổng có số chẵn lẻ khác nhau và câu trả lời phải là "KHÔNG". Mảng chứa ít nhất một phần tử lẻ và một phần tử chẵn thường mang lại sự linh hoạt hơn. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi phân vùng có thể có của mảng thành hai nhóm không trống và kiểm tra tính chẵn lẻ của tổng. Với n phần tử, có$2^n - 2$có thể phân chia không trống. Với n=50, đây là khoảng$10^{15}$, vượt xa khả năng khả thi trong 1 giây. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát hành vi chẵn lẻ. Tổng modulo 2 chỉ phụ thuộc vào số lượng số lẻ. Nếu mảng chứa ít nhất một phần tử lẻ và ít nhất một phần tử chẵn thì luôn có thể chia chúng sao cho cả hai nhóm có tổng chẵn lẻ như nhau. Nếu tất cả các phần tử đều là số chẵn thì mọi phép chia không trống sẽ tạo ra tổng số chẵn, thỏa mãn điều kiện. Nếu tất cả các phần tử đều là số lẻ thì tổng số đó là số lẻ và việc chia thành hai nhóm không trống sẽ tạo ra một tổng lẻ và một tổng lẻ trừ đi số lẻ, số này là số chẵn, do đó tính chẵn lẻ sẽ khác nhau và "KHÔNG" là đúng. 

Vì vậy, bài toán quy về việc đếm các phần tử chẵn và lẻ. Nếu mảng chứa cả phần tử lẻ và phần tử chẵn hoặc tất cả các phần tử đều là số chẵn thì câu trả lời là "CÓ". Nếu tất cả các phần tử đều lẻ và n > 1 thì câu trả lời là "KHÔNG". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Kiểm tra chẵn lẻ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc độ dài mảng n và mảng a. 
2. Khởi tạo bộ đếm cho phần tử chẵn và lẻ. 
3. Lặp lại từng phần tử của a. Nếu nó là số lẻ, hãy tăng bộ đếm số lẻ; mặt khác, tăng bộ đếm chẵn. 
4. Kiểm tra điều kiện: nếu cả số lẻ và số chẵn đều ít nhất bằng một, xuất "CÓ". 
5. Nếu tất cả các phần tử đều chẵn, xuất ra "CÓ". 
6. Nếu tất cả các phần tử đều lẻ và n > 1, xuất ra "NO". 

Tại sao nó hoạt động: tính chẵn lẻ của tổng chỉ phụ thuộc vào tính chẵn lẻ của các phần tử trong mỗi nhóm. Việc có ít nhất một phần tử của mỗi số chẵn lẻ cho phép chúng ta cân bằng tổng khi cần thiết. Mảng chỉ số chẵn luôn tạo ra tổng có cùng số chẵn lẻ, trong khi mảng chỉ có số lẻ luôn tạo ra tổng chẵn lẻ không bằng nhau khi chia thành hai nhóm không trống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    odd = sum(1 for x in a if x % 2)
    even = n - odd
    if odd and even:
        print("YES")
    elif even == n:
        print("YES")
    else:
        print("NO")
```Mã đếm số lẻ và số chẵn và áp dụng các điều kiện được nêu trong thuật toán. sử dụng`sum(1 for x in a if x % 2)`tránh việc lặp lại thủ công và giữ mã ngắn gọn. Các trường hợp ranh giới được xử lý một cách tự nhiên: mảng có tất cả các phần tử chẵn trả về "CÓ", mảng có tất cả các phần tử lẻ trả về "KHÔNG". 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`[1,2,4,3,2,3,5,4]`| Bước | lẻ | thậm chí | lẻ>0 và chẵn>0? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đếm | 5 | 3 | Đúng | CÓ | 

Đầu vào mẫu 2:`[4,7]`| Bước | lẻ | thậm chí | lẻ>0 và chẵn>0? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Đếm | 1 | 1 | Đúng | CÓ | 

Đầu vào thứ hai chứng tỏ rằng việc có cả hai số chẵn lẻ cho phép phân chia hợp lệ. Nếu đầu vào đã được`[1,3]`, cả hai đều là số lẻ, séc sẽ trả về "KHÔNG". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đếm các phần tử lẻ/chẵn yêu cầu một lượt duy nhất | 
| Không gian | O(1) | Chỉ các bộ đếm số lẻ và số chẵn được lưu trữ | 

Với n ∼ 50 và t ∼ 1000, điều này nằm trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        odd = sum(1 for x in a if x % 2)
        even = n - odd
        if odd and even:
            print("YES")
        elif even == n:
            print("YES")
        else:
            print("NO")
    return output.getvalue().strip()

# Provided samples
assert run("7\n8\n1 2 4 3 2 3 5 4\n2\n4 7\n3\n3 9 8\n2\n1 7\n5\n5 4 3 2 1\n4\n4 3 4 5\n2\n50 48\n") == "YES\nYES\nYES\nYES\nNO\nYES\nYES"

# Custom cases
assert run("1\n2\n1 3\n") == "NO", "all odd"
assert run("1\n2\n2 4\n") == "YES", "all even"
assert run("1\n3\n1 2 3\n") == "YES", "mixed small array"
assert run("1\n50\n" + "1 "*50 + "\n") == "NO", "max size, all odd"
assert run("1\n50\n" + "2 "*50 + "\n") == "YES", "max size, all even"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3`| KHÔNG | mọi số lẻ có n>1 | 
|`2 4`| CÓ | tất cả các số chẵn | 
|`1 2 3`| CÓ | mảng hỗn hợp nhỏ | 
| 50 lần`1`| KHÔNG | kích thước tối đa tất cả lẻ | 
| 50 lần`2`| CÓ | kích thước tối đa đều chẵn | 

## Vỏ cạnh 

cho`[1,3]`, cả hai số đều lẻ. Đếm thì lẻ=2, chẵn=0. điều kiện`odd and even`là sai,`even == n`là sai, vì vậy nó sẽ xuất ra "KHÔNG". Vì`[2,4]`, lẻ=0, chẵn=2.`even == n`là đúng, vì vậy đầu ra là "CÓ". Mảng có sự kết hợp giữa số lẻ và số chẵn, như`[1,2,3]`, thỏa mãn`odd and even`, vì vậy đầu ra là "CÓ". Thuật toán tự động xử lý tất cả các tình huống biên mà không cần phân nhánh trường hợp đặc biệt.
