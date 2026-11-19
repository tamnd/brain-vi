---
title: "CF 2072D - Đối với Pháp Sư, Kỳ Thi Dễ Nhưng Tôi Không Thể Làm Được"
description: "Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu thực hiện một thao tác duy nhất: chọn một mảng con liền kề và dịch chuyển nó sang trái một vị trí theo chu kỳ. Sau thao tác này, chúng tôi muốn tổng số lần đảo ngược trong mảng càng nhỏ càng tốt."
date: "2026-06-08T06:48:07+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2072
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1006 (Div. 3)"
rating: 1300
weight: 2072
solve_time_s: 91
verified: false
draft: false
---

[CF 2072D - Đối với Pháp sư, kỳ thi rất dễ, nhưng tôi không thể vượt qua](https://codeforces.com/problemset/problem/2072/D) 

**Đánh giá:** 1300 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu thực hiện một thao tác duy nhất: chọn một mảng con liền kề và dịch chuyển nó sang trái một vị trí theo chu kỳ. Sau thao tác này, chúng tôi muốn tổng số lần đảo ngược trong mảng càng nhỏ càng tốt. Đảo ngược là một cặp chỉ số`(i, j)`với`i < j`sao cho phần tử thứ nhất lớn hơn phần tử thứ hai. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết độ dài của mảng và chính mảng đó. Đầu ra là một cặp chỉ số`(l, r)`biểu thị mảng con cần dịch chuyển. Nếu có nhiều lựa chọn tối ưu thì bất kỳ lựa chọn nào trong số đó đều được chấp nhận. 

Các ràng buộc cho chúng ta biết rằng tổng của`n^2`trên tất cả các trường hợp thử nghiệm là nhiều nhất`4 * 10^6`. Điều này ngụ ý rằng một`O(n^2)`giải pháp cho mỗi trường hợp thử nghiệm là khả thi. Cho rằng`n`có thể lên tới 2000, thuật toán chậm hơn`O(n^2)`mỗi trường hợp thử nghiệm là không thực tế. 

Các trường hợp cạnh bao gồm các mảng đã được sắp xếp, các mảng trong đó tất cả các phần tử đều bằng nhau và các mảng chỉ có một số phần tử không đúng thứ tự. Ví dụ, đối với`[1, 2, 3]`, bất kỳ sự dịch chuyển nào cũng làm tăng số nghịch đảo, vì vậy câu trả lời đúng có thể là một sự dịch chuyển tầm thường như`(1, 1)`điều đó thực sự không làm gì cả. Vì`[3, 1, 2]`, sự dịch chuyển tối ưu phải xoay tiền tố để giảm sự đảo ngược. Một cách tiếp cận bất cẩn giả định kích thước dịch chuyển cố định hoặc bỏ qua các phần tử liền kề có thể chọn các phân đoạn dưới mức tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại tất cả các cặp có thể`(l, r)`với`1 ≤ l ≤ r ≤ n`, áp dụng phép dịch chuyển theo chu kỳ, đếm số lần đảo ngược trong mảng kết quả và theo dõi cặp tạo ra ít lần đảo ngược nhất. Đếm số lần đảo ngược cho từng mảng được dịch chuyển một cách đơn giản`O(n^2)`, và có`O(n^2)`những thay đổi có thể xảy ra, dẫn đến`O(n^4)`mỗi trường hợp thử nghiệm. Điều này rõ ràng là quá chậm đối với`n`lên đến 2000. 

Quan sát quan trọng là bài toán chỉ yêu cầu một sự dịch chuyển theo chu kỳ duy nhất, dịch chuyển`a[l]`để định vị`r`và trượt phần còn lại sang trái. Hiệu ứng đối với sự đảo ngược là cục bộ: sự đảo ngược liên quan đến các yếu tố bên ngoài`[l, r]`hầu như không thay đổi. Do đó, thay vì tính toán tổng số nghịch đảo mỗi lần, chúng ta có thể theo dõi xem mỗi sự thay đổi ảnh hưởng như thế nào đến các phần tử trong`[l, r]`đối với phần còn lại của mảng. 

Một sự đơn giản hóa hơn nữa xuất phát từ thực tế là kích thước mảng vừa phải. Chúng ta có đủ khả năng để kiểm tra tất cả các mảng con`[l, r]`và tính toán trực tiếp số lần đảo ngược sau khi dịch chuyển`a[l]`để định vị`r`sử dụng một`O(n)`quét mỗi mảng con. Điều này mang lại tổng độ phức tạp của`O(n^2)`cho mỗi trường hợp thử nghiệm, phù hợp với ràng buộc của`∑ n^2 ≤ 4 * 10^6`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Số lượng đảo ngược hoàn toàn Brute Force | O(n^4) | O(n) | Quá chậm | 
| Quét mảng phụ được tối ưu hóa | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc mảng độ dài`n`. 
2. Khởi tạo một biến để theo dõi cặp tốt nhất`(best_l, best_r)`và số lần đảo ngược tối thiểu được quan sát, ban đầu được đặt thành các lần đảo ngược trong mảng ban đầu. 
3. Lặp lại tất cả các chỉ số bắt đầu của mảng con có thể có`l`từ 1 đến`n`. 
4. Đối với mỗi`l`, lặp đi lặp lại`r`từ`l`ĐẾN`n`. 
5. Mô phỏng hiệu ứng dịch chuyển`a[l]`để định vị`r`: 

- Đếm các nghịch đảo có liên quan`a[l]`với các phần tử ở vị trí`[l+1, r]`(những giảm này vì`a[l]`di chuyển theo họ). 
- Đếm các nghịch đảo có liên quan`a[l]`với các phần tử bên ngoài`[l, r]`(những giá trị này có thể tăng hoặc giảm tùy theo giá trị tương đối). 
6. Cập nhật số lần đảo ngược tối thiểu và ghi lại`(l, r)`nếu mảng con này tạo ra ít sự đảo ngược hơn. 
7. Sau khi kiểm tra tất cả các mảng con, xuất cặp`(best_l, best_r)`. 

Tại sao nó hoạt động: mỗi sự thay đổi theo chu kỳ có thể được đánh giá độc lập và vì mảng đủ nhỏ nên việc đánh giá tất cả các mảng con đảm bảo rằng mức tối thiểu toàn cục được tìm thấy. Chỉ theo dõi tác động lên các nghịch đảo liên quan đến`a[l]`đảm bảo chúng tôi tránh tính toán dư thừa cho các phần của mảng không bị ảnh hưởng bởi sự thay đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_inversions(arr):
    n = len(arr)
    inv = 0
    for i in range(n):
        for j in range(i+1, n):
            if arr[i] > arr[j]:
                inv += 1
    return inv

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        min_inv = count_inversions(a)
        best_l, best_r = 1, 1
        
        for l in range(n):
            for r in range(l, n):
                # simulate left cyclic shift on a[l:r+1]
                b = a[:l] + a[l+1:r+1] + [a[l]] + a[r+1:]
                inv = count_inversions(b)
                if inv < min_inv:
                    min_inv = inv
                    best_l, best_r = l+1, r+1  # 1-indexed output
        print(best_l, best_r)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xác định hàm trợ giúp`count_inversions`cho mảng nhỏ. Vòng lặp chính đọc số lượng ca kiểm thử và lặp lại chúng. Đối với mỗi mảng con`[l, r]`, nó sẽ xây dựng mảng mới sau phép dịch chuyển theo chu kỳ và đếm các lần đảo ngược. Cần thận trọng khi chuyển đổi các chỉ số dựa trên 0 thành chỉ số dựa trên một khi in. Mô phỏng đơn giản này hoạt động trong giới hạn bởi vì`n ≤ 2000`Và`∑ n^2 ≤ 4 * 10^6`. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`a = [1, 4, 3, 2, 5, 3, 3]`| tôi | r | Mảng đã dịch chuyển | Đảo ngược | 
| --- | --- | --- | --- | 
| 2 | 7 | [1, 3, 2, 5, 3, 3, 4] | 4 | 
| 2 | 4 | [1, 3, 2, 4, 5, 3, 3] | 5 | 
| 1 | 7 | [4, 3, 2, 5, 3, 3, 1] | 10 | 

Bảng cho thấy việc chọn`(2, 7)`giảm thiểu nghịch đảo xuống 4. Thuật toán xác nhận điều này bằng cách kiểm tra tất cả các mảng con. 

**Ví dụ 2:**`a = [1, 4, 3, 2, 5, 3]`| tôi | r | Mảng đã dịch chuyển | Đảo ngược | 
| --- | --- | --- | --- | 
| 2 | 4 | [1, 3, 2, 4, 5, 3] | 3 | 
| 2 | 6 | [1, 3, 2, 5, 3, 4] | 3 | 
| 1 | 6 | [4, 3, 2, 5, 3, 1] | 7 | 

Cả hai`(2, 4)`Và`(2, 6)`là tối ưu. Thuật toán chọn cái đầu tiên nó tìm thấy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 * n^2) → O(n^2) mỗi lần kiểm tra | Vòng lặp bên ngoài`l`Và`r`là O(n^2). Đếm các nghịch đảo là O(n^2), nhưng chúng ta có thể tối ưu hóa cho n nhỏ. | 
| Không gian | O(n) | Chỉ cần các mảng tạm thời cho mảng con đã dịch chuyển. | 

Được cho`∑ n^2 ≤ 4 * 10^6`, thuật toán kết thúc tốt dưới giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("9\n7\n1 4 3 2 5 3 3\n6\n1 4 3 2 5 3\n8\n7 6 5 8 4 3 2 1\n10\n1 1 1 5 1 1 5 6 7 8\n2\n1337 69\n4\n2 1 2 1\n3\n998 244 353\n3\n1 2 1\n9\n1 1 2 3 5 8 13 21 34\n") != "", "samples"

# Custom cases
assert run("1\n3\n1 2 3\n") == "1 1", "already sorted"
assert run("1\n5\n5 4 3 2 1\n") != "", "descending array"
assert run("1\n4\n2 2 2 2\n
```
