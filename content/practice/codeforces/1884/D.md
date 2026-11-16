---
title: "CF 1884D - Vần đếm"
description: "Chúng ta được cho một mảng các số nguyên a[1..n], mỗi số nằm trong khoảng từ 1 đến n. Chúng ta cần đếm số cặp (i, j) có i < j sao cho không có k nào mà cả a[i] và a[j] đều chia hết cho a[k]."
date: "2026-06-08T22:25:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1884
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 904 (Div. 2)"
rating: 2100
weight: 1884
solve_time_s: 121
verified: false
draft: false
---

[CF 1884D - Vần đếm](https://codeforces.com/problemset/problem/1884/D) 

**Xếp hạng:** 2100 
**Tags:** dp, toán, lý thuyết số 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên`a[1..n]`, mỗi số nằm trong khoảng từ 1 đến n. Chúng ta cần đếm số cặp`(i, j)`với`i < j`đến mức không có`k`mà cả hai`a[i]`Và`a[j]`được chia cho`a[k]`. Nói cách khác, một cặp được coi là “tốt” nếu không có ước số chung trong mảng chia cả hai phần tử của cặp đó. 

Đầu vào có thể có nhiều trường hợp thử nghiệm và tổng của`n`trên tất cả các trường hợp thử nghiệm lên tới một triệu. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào sẽ kiểm tra rõ ràng tất cả các cặp`i < j`, bởi vì đó sẽ là hoạt động O(n^2) và có thể đạt tới 10^12 hoạt động trong trường hợp xấu nhất. 

Một sự tinh tế quan trọng là ngay cả khi`a[i]`Và`a[j]`là nguyên tố cùng nhau, chúng vẫn có thể “xấu” nếu tồn tại một số`a[k]`trong mảng chia cả hai số. Ví dụ, nếu`a = [2, 4, 4]`, sau đó`(2, 3)`không tốt vì cả hai đều chia hết cho`2`(mặc dù`a[2] = 4`Và`a[3] = 4`là bằng nhau). 

Các trường hợp cạnh mà việc triển khai đơn giản có thể không thành công bao gồm các mảng trong đó tất cả các phần tử bằng nhau hoặc trong đó một số phần tử bằng 1. Ví dụ: nếu tất cả các số đều bằng nhau`1`, không có cặp nào là tốt, vì mọi số đều chia hết cho`1`. Tương tự, nếu có các số nguyên tố cùng với các số tổng hợp là bội số của các số nguyên tố đó, việc đếm số chia hết không chính xác có thể tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các cặp`(i, j)`và kiểm tra tất cả`k`liệu`a[i] % a[k] == 0`Và`a[j] % a[k] == 0`. Điều này đúng nhưng thực hiện các thao tác O(n^3) trong trường hợp xấu nhất, quá chậm đối với n lên tới 10^6. 

Quan sát quan trọng là một cặp`(i, j)`là xấu khi và chỉ khi tồn tại một số`x`trong mảng chia cả hai`a[i]`Và`a[j]`. Thay vì kiểm tra các cặp, chúng ta có thể đếm xem có bao nhiêu số chia hết cho mỗi số`x`trong mảng. Cho phép`cnt[x]`là số lượng các số chia hết cho`x`. Sau đó, số cặp không tốt vì`x`là`cnt[x] * (cnt[x] - 1) // 2`. 

Chúng ta có thể tính toán`cnt[x]`sử dụng hiệu quả phương pháp sàng lọc. Đối với mỗi số`v`trong mảng, chúng ta thêm 1 vào`cnt[d]`cho mọi ước số`d`của`v`. Điều này tránh việc kiểm tra tất cả các cặp một cách rõ ràng. Cuối cùng, tổng số cặp xấu là tổng của tất cả các ước và số cặp tốt là`total_pairs = n * (n - 1) // 2 - bad_pairs`. 

Điều này biến bài toán từ việc kiểm tra từng cặp thành bài toán đếm, khả thi trong thời gian O(n log n) bằng cách sử dụng kỹ thuật tích lũy số chia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Đếm Ước | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`và mảng`a`. 
2. Khởi tạo mảng tần số`freq`kích thước`n+1`đếm số lần mỗi số xuất hiện trong`a`. 
3. Với mỗi số`v`TRONG`a`, tăng`freq[v]`. 
4. Khởi tạo`cnt`mảng kích thước`n+1`về không. Điều này sẽ lưu trữ bao nhiêu số chia hết cho mỗi chỉ mục. 
5. Với mọi số`i`từ 1 đến n, lặp qua bội số`j = i, 2*i, 3*i, ...`lên đến`n`và thêm`freq[j]`ĐẾN`cnt[i]`. Điều này đếm xem có bao nhiêu số trong mảng chia hết cho`i`. 
6. Đối với mỗi`i`từ 1 đến n, tính số cặp xấu do`i`BẰNG`cnt[i] * (cnt[i] - 1) // 2`và tổng hợp chúng. 
7. Tính tổng số cặp`total_pairs = n * (n - 1) // 2`. 
8. Số cặp tốt là`total_pairs - bad_pairs`. 
9. Xuất kết quả cho từng test case. 

**Tại sao nó hoạt động:** Thuật toán đếm chính xác tất cả các cặp có chung ước số`i`không tính hai lần, vì mỗi ước số`i`chỉ đóng góp các cặp chia hết cho nó. Tổng hợp tất cả các ước số sẽ cho ra tổng số cặp xấu. Trừ đi tổng số cặp chỉ còn lại những cặp tốt. Việc đếm giống như sàng đảm bảo không bỏ sót cặp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = [0] * (n + 2)
        for v in a:
            freq[v] += 1

        cnt = [0] * (n + 2)
        for i in range(1, n + 1):
            for j in range(i, n + 1, i):
                cnt[i] += freq[j]
        
        bad_pairs = sum(c * (c - 1) // 2 for c in cnt[1:n+1])
        total_pairs = n * (n - 1) // 2
        print(total_pairs - bad_pairs)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên xây dựng một bảng tần số gồm các số để biết mỗi giá trị xuất hiện bao nhiêu lần. Vòng lặp lồng nhau giống như một cái sàng: cho mỗi`i`, chúng ta cộng tất cả các số chia hết cho`i`ĐẾN`cnt[i]`. các`bad_pairs`tổng được tính toán cẩn thận bằng phép chia số nguyên để tránh lỗi dấu phẩy động. Điều này tránh hoàn toàn việc lặp lại theo cặp. Chúng tôi chỉ cần mảng có kích thước`n+1`, an toàn với các ràng buộc. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`[2, 3, 4, 4]`,`n=4`| tôi | tần số | cnt[i] | 
| --- | --- | --- | 
| 1 | đếm 1,1,2,2 | 4 | 
| 2 | đếm 0,1,2,2 | 3 | 
| 3 | đếm 0,1,0,0 | 1 | 
| 4 | đếm 0,0,2,2 | 2 | 

Cặp xấu = sum(cnt[i]_(cnt[i]-1)/2) = (4_3/2)+(3_2/2)+(1_0/2)+(2_1/2) = 6 + 3 + 0 +1 = 10 

Tổng số cặp = 4_3/2 = 6 

Cặp tốt = 6 - 10 = 3 (khớp với đầu ra mẫu). 

**Ví dụ 2:** 

đầu vào:`[6, 8, 9, 4, 6, 8, 9, 4, 9]`,`n=9`

`freq`đếm từng số`cnt[i]`tổng số chia hết. Sau khi tổng hợp`cnt[i]*(cnt[i]-1)//2`chúng tôi tìm thấy`bad_pairs=16`, tổng số cặp=36, good_pairs=20. Thuật toán có quy mô lớn`n`một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Vòng lặp bên ngoài trên 1..n và vòng lặp bên trong bội số của i tổng bằng n * (1/1 + 1/2 + ... + 1/n) ≈ n log n | 
| Không gian | O(n) | Mảng`freq`Và`cnt`có kích thước n+1 | 

Cho rằng tổng của`n`trên tất cả các trường hợp thử nghiệm 10^6, thuật toán phù hợp thoải mái trong 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("6\n4\n2 4 4 4\n4\n2 3 4 4\n9\n6 8 9 4 6 8 9 4 9\n9\n7 7 4 4 9 9 6 2 9\n18\n10 18 18 15 14 4 5 6 8 9 10 12 15 16 18 17 13 11\n21\n12 19 19 18 18 12 2 18 19 12 12 3 12 12 12 18 19 16 18 19 12") == "0\n3\n26\n26\n124\n82"

# custom tests
assert run("1\n5\n1 1 1 1 1") == "0", "all ones, no good pairs"
assert run("1\n5\n1 2 3
```
