---
title: "CF 1927C - Hãy chọn những điều khác biệt!"
description: "Chúng ta có hai mảng a và b và một số nguyên k. Chúng ta cần chọn chính xác k/2 phần tử từ mỗi mảng sao cho tập hợp các phần tử được chọn chứa tất cả các số nguyên từ 1 đến k."
date: "2026-06-08T18:53:24+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1927
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 923 (Div. 3)"
rating: 1000
weight: 1927
solve_time_s: 138
verified: false
draft: false
---

[CF 1927C - Chọn những cái khác nhau!](https://codeforces.com/problemset/problem/1927/C) 

**Đánh giá:** 1000 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`Và`b`và một số nguyên chẵn`k`. Chúng ta cần chọn chính xác`k/2`các phần tử từ mỗi mảng sao cho tập hợp các phần tử được chọn chứa **tất cả các số nguyên từ`1`ĐẾN`k`**. Thứ tự chúng ta chọn các phần tử không quan trọng; điều quan trọng là phạm vi phủ sóng đầy đủ`[1, k]`. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định độ dài của mảng`n`Và`m`, chính các mảng và giá trị của`k`. Đầu ra là "CÓ" hoặc "KHÔNG" đơn giản tùy thuộc vào việc lựa chọn có khả thi hay không. 

Những hạn chế là vừa phải nhưng quan trọng.`n`Và`m`có thể lên đến`2 * 10^5`riêng lẻ và tổng của tất cả các trường hợp thử nghiệm không vượt quá`4 * 10^5`. Điều này loại trừ mọi cách tiếp cận kiểm tra **tất cả các tập hợp con** có thể có về kích thước`k/2`trong một trong hai mảng, vì số lượng tập hợp con tăng lên theo kiểu tổ hợp. Thay vào đó, chúng ta cần một giải pháp có thể quét từng mảng một cách tuyến tính hoặc gần tuyến tính. Giá trị của`k`luôn chẵn và thỏa mãn`k <= 2 * min(n, m)`, điều này đảm bảo rằng việc chọn`k/2`các phần tử từ mỗi mảng luôn khả thi về mặt số lượng. 

Các trường hợp cạnh rất tinh tế. Một là khi một mảng chứa các phần tử lặp lại. Ví dụ, nếu`k=4`,`a=[1,1,1,1]`Và`b=[2,3,4]`, chúng ta chỉ có thể chọn`1`từ`a`(bao nhiêu tùy thích, nhưng các bản sao không bao gồm các số mới) và chúng tôi không thể đáp ứng yêu cầu về mức độ phù hợp, vì vậy câu trả lời là "KHÔNG". Một trường hợp khác là khi`k/2`bằng độ dài của một mảng, buộc chúng ta phải chọn tất cả các phần tử ở đó. Chúng ta vẫn phải kiểm tra xem mảng kia có thể bao gồm các số còn lại hay không. Bỏ qua những sắc thái này sẽ dẫn đến những sai sót tinh tế. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là vũ phu. Người ta có thể liệt kê tất cả các tập con có kích thước`k/2`từ`a`và tất cả các tập hợp con có kích thước`k/2`từ`b`, sau đó kiểm tra xem liệu hợp của chúng có bao gồm tất cả các số từ`1`ĐẾN`k`. Mặc dù điều này có hiệu quả về mặt lý thuyết nhưng độ phức tạp là`O(n choose k/2 * m choose k/2 * k)`, điều này là không thể thực hiện được đối với`n, m`lên đến`2*10^5`. 

Thông tin chi tiết quan trọng là chúng tôi không quan tâm đến các tập hợp con cụ thể mà chỉ quan tâm đến **những số nào hiện diện**. Mỗi mảng chỉ có thể đóng góp một số vào vùng phủ sóng một lần, ngay cả khi nó có nhiều bản sao. Vì vậy, chúng ta có thể coi mỗi mảng là một **tập hợp các số nhỏ hơn hoặc bằng`k`**. Vấn đề sau đó giảm xuống còn việc kiểm tra xem liệu có thể chọn nhiều nhất`k/2`các phần tử từ mỗi tập hợp trong khi bao trùm chung tất cả`1..k`. 

Trong thực tế, chúng ta chỉ cần tính có bao nhiêu số từ`1..k`có mặt trong mỗi mảng. Cho phép`count_in_a`là số các số trong`1..k`xuất hiện trong`a`, và tương tự`count_in_b`vì`b`. Nếu như`count_in_a > k/2`, chúng ta chỉ có thể chọn`k/2`số từ`a`, vì vậy số lượng các số được bao phủ từ`a`là`min(count_in_a, k/2)`. Tương tự cho`b`. Sau đó, vấn đề giảm xuống còn việc xác minh xem tổng của những đóng góp này có ít nhất là`k`. Nếu vậy, có một lựa chọn hợp lệ; nếu không thì không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n chọn k/2) * (m chọn k/2) * k) | O(k) | Quá chậm | 
| Tối ưu | O(n + m + k) cho mỗi trường hợp thử nghiệm | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`,`m`,`k`và các mảng`a`Và`b`. 
3. Khởi tạo hai mảng (hoặc bộ) boolean`present_a`Và`present_b`chiều dài`k+1`để theo dõi những con số từ`1..k`xuất hiện ở`a`Và`b`tương ứng. 
4. Với mỗi số trong`a`, nếu nó nằm giữa`1`Và`k`, đánh dấu nó là có mặt trong`present_a`. 
5. Lặp lại cho`b`, đánh dấu`present_b`. 
6. Đếm các số trong`1..k`xuất hiện trong mỗi mảng:`count_in_a`là tổng của`present_a[1..k]`, Và`count_in_b`là tổng của`present_b[1..k]`. 
7. Tính toán phần đóng góp hiệu quả của từng mảng:`contrib_a = min(count_in_a, k/2)`,`contrib_b = min(count_in_b, k/2)`. 
8. Nếu`contrib_a + contrib_b >= k`, xuất ra "CÓ". Nếu không thì xuất ra "NO". 

**Tại sao nó hoạt động:** Điều bất biến là mỗi mảng có thể đóng góp nhiều nhất`k/2`số để bảo hiểm. Bằng cách đếm số lượng thực tế hiện có và giới hạn chúng ở mức`k/2`, chúng tôi mô phỏng phạm vi bao phủ tối đa mà mỗi mảng có thể cung cấp. Nếu phạm vi bảo hiểm kết hợp của họ đạt đến`k`, thì một lựa chọn hợp lệ của`k/2`các phần tử từ mỗi mảng tồn tại; nếu không thì không thể được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    present_a = [0] * (k + 1)
    present_b = [0] * (k + 1)
    
    for x in a:
        if 1 <= x <= k:
            present_a[x] = 1
    for x in b:
        if 1 <= x <= k:
            present_b[x] = 1
    
    count_in_a = sum(present_a[1:])
    count_in_b = sum(present_b[1:])
    
    contrib_a = min(count_in_a, k // 2)
    contrib_b = min(count_in_b, k // 2)
    
    print("YES" if contrib_a + contrib_b >= k else "NO")
```Giải pháp đọc đầu vào một cách hiệu quả và sử dụng mảng boolean để theo dõi những số nào có sẵn. Việc sử dụng`min(count, k//2)`đảm bảo chúng tôi tôn trọng giới hạn số lượng phần tử chúng tôi có thể chọn từ mỗi mảng. Tính tổng mảng hiện diện là an toàn vì chúng ta bỏ qua các số ở bên ngoài`1..k`. Điều này tránh được từng lỗi một. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`a=[2,3,8,5,6,5]`,`b=[1,3,4,10,5]`,`k=6`. 

| Số | Trình bày trong | Có mặt trong b | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 2 | 1 | 0 | 
| 3 | 1 | 1 | 
| 4 | 0 | 1 | 
| 5 | 1 | 1 | 
| 6 | 1 | 0 |`count_in_a = 4`,`count_in_b = 4`.`k/2 = 3`.`contrib_a = 3`,`contrib_b = 3`. Tổng = 6 ≥ k → CÓ. 

**Ví dụ 2:**`a=[2,3,4,5,6,5]`,`b=[1,3,8,10,3]`,`k=6`. 

| Số | Trình bày trong | Có mặt trong b | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 2 | 1 | 0 | 
| 3 | 1 | 1 | 
| 4 | 1 | 0 | 
| 5 | 1 | 0 | 
| 6 | 1 | 0 |`count_in_a = 5`,`count_in_b = 2`.`contrib_a = 3`,`contrib_b = 2`. Tổng = 5 < 6 → KHÔNG. 

Những dấu vết này cho thấy rằng việc giới hạn các khoản đóng góp đảm bảo giải pháp tôn trọng`k/2`giới hạn lựa chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + k) cho mỗi trường hợp thử nghiệm | Quét từng mảng một lần, tính tổng các mảng có mặt | 
| Không gian | O(k) | Hai mảng có kích thước k+1 để theo dõi sự hiện diện | 

Trên tất cả các trường hợp thử nghiệm, tổng của`n + m`4 * 10^5, và`k ≤ 2 * min(n, m)`4 * 10^5. Tổng số thao tác vừa vặn thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution code
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        present_a =
```
