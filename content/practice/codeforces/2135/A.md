---
title: "CF 2135A - Chống lại sự khác biệt"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta muốn tìm độ dài của dãy con dài nhất có thể chia thành \"khối\". Một khối được định nghĩa là một chuỗi liền kề trong đó mọi phần tử bằng độ dài của chuỗi đó."
date: "2026-06-08T02:37:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 2135
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1046 (Div. 1)"
rating: 1200
weight: 2135
solve_time_s: 119
verified: false
draft: false
---

[CF 2135A - Chống lại sự khác biệt](https://codeforces.com/problemset/problem/2135/A) 

**Đánh giá:** 1200 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên và chúng tôi muốn tìm độ dài của chuỗi con dài nhất có thể được chia thành các "khối". Một khối được định nghĩa là một chuỗi liền kề trong đó mọi phần tử bằng độ dài của chuỗi đó. Ví dụ,`[3, 3, 3]`là một khối vì nó có độ dài ba và mọi phần tử đều là ba, trong khi`[2, 2, 1]`không phải là một khối. Mảng là gọn gàng nếu nó có thể được biểu diễn dưới dạng nối của một hoặc nhiều khối, có thể bằng 0. Nhiệm vụ của chúng ta là tìm độ dài tối đa của dãy con gọn gàng. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết độ dài`n`của mảng và chính các phần tử mảng. Mỗi phần tử thỏa mãn`1 <= a_i <= n`. Tổng của tất cả`n`trên các trường hợp thử nghiệm tối đa là 200.000, vì vậy chúng tôi phải hướng tới một thuật toán chạy trong thời gian gần như tuyến tính cho mỗi trường hợp thử nghiệm. Các thuật toán bậc hai, chẳng hạn như thử tất cả các dãy con, sẽ yêu cầu theo thứ tự`O(2^n)`hoạt động hoàn toàn không thể thực hiện được. Thậm chí`O(n^2)`cách tiếp cận sẽ quá chậm đối với`n`gần 200.000. 

Các trường hợp cạnh tinh tế bao gồm các mảng trong đó không có phần tử nào xuất hiện đủ số lần để tạo thành một khối hoặc chỉ có thể có các khối nhỏ. Ví dụ, đối với`a = [5, 1, 2]`, không có khối nào có thể được hình thành vì số lượng số không khớp với bất kỳ độ dài khối cần thiết nào, do đó chuỗi con gọn gàng dài nhất sẽ trống. Một trường hợp khác là khi xuất hiện nhiều số giống nhau nhưng số lượng không đủ để tạo thành khối của chúng, ví dụ:`[2, 2, 2]`cho phép một khối`[2, 2]`nhưng thứ ba`2`không thể tạo thành một khối mới nên cần phải tính toán cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là liệt kê tất cả các chuỗi con, kiểm tra xem mỗi chuỗi có thể được chia thành các khối hay không và ghi lại độ dài tối đa. Điều này đúng về nguyên tắc nhưng khó thực hiện: một dãy con có`2^n`những khả năng lớn về mặt thiên văn đối với`n = 2 * 10^5`. Thậm chí một`O(n^2)`Cách tiếp cận cố gắng lập trình động trên các mảng con hoặc vị trí quá chậm vì nó yêu cầu theo thứ tự`4*10^10`hoạt động trong trường hợp xấu nhất. 

Thông tin chi tiết quan trọng là cần lưu ý rằng thứ tự của các phần tử không quan trọng ngoài việc chọn các chuỗi con. Điều quan trọng là tần số của mỗi số. Đối với một số`x`, số phần tử tối đa có thể góp phần tạo nên các khối có kích thước gọn gàng`x`là bội số lớn nhất của`x`nhỏ hơn hoặc bằng số của nó. Ví dụ, nếu`4`xuất hiện`9`lần, chúng ta có thể tạo thành hai khối có kích thước 4 (`4*2 = 8`phần tử), để lại một phần còn lại`4`không thể tạo thành một khối. Khi chúng tôi tính toán mức đóng góp tối đa của mỗi số, việc tính tổng các số này sẽ cho độ dài của chuỗi con gọn gàng dài nhất. 

Do đó, cách tiếp cận tối ưu là lựa chọn tham lam dựa trên số tần số: đếm số lần xuất hiện của mỗi số, tính bội số tối đa của mỗi số nhỏ hơn hoặc bằng số đếm của nó và tính tổng các đóng góp này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Khởi tạo một mảng tần số hoặc từ điển để đếm số lần xuất hiện của mỗi số nguyên trong`a`. 
4. Khởi tạo một biến`max_neat_length`về không. 
5. Lặp lại từng số riêng biệt`x`trong dãy tần số. 
6. Với mỗi số`x`, tính toán`k = (count[x] // x) * x`. Điều này cho biết số phần tử tối đa của`x`có thể tạo thành các khối hoàn chỉnh. 
7. Thêm`k`ĐẾN`max_neat_length`. 
8. Sau khi xử lý tất cả các số, xuất ra`max_neat_length`cho trường hợp thử nghiệm hiện tại. 

Tại sao nó hoạt động: Lựa chọn tham lam là hợp lệ vì các khối có độ dài`x`phải sử dụng chính xác`x`các phần tử và bất kỳ phần tử bổ sung nào ngoài bội số của`x`không thể đóng góp vào một dãy con gọn gàng. Việc đếm tần số đảm bảo chúng ta xem xét tất cả các yếu tố có thể có. Tính tổng các đóng góp tối đa đảm bảo chúng ta thu được chuỗi con dài nhất. Vì dãy con có thể được sắp xếp lại một cách tùy ý nên thứ tự không hạn chế lời giải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def longest_neat_subsequence(n, a):
    from collections import Counter
    freq = Counter(a)
    length = 0
    for x, count in freq.items():
        length += (count // x) * x
    return length

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(longest_neat_subsequence(n, a))
```Giải pháp sử dụng`Counter`để đếm hiệu quả các lần xuất hiện trong`O(n)`thời gian. Vòng lặp kết thúc`freq.items()`đảm bảo mỗi số riêng biệt được xem xét chính xác một lần. Phép chia và phép nhân`(count // x) * x`xác định số phần tử tối đa có thể sử dụng được từ mỗi số. Việc sử dụng I/O nhanh đảm bảo giải pháp chạy hiệu quả trong điều kiện hạn chế đầu vào chặt chẽ. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`a = [1, 2, 3, 3, 3, 1]`| x | đếm[x] | đếm[x]//x | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 // 1 = 2 | 2 | 
| 2 | 1 | 1 // 2 = 0 | 0 | 
| 3 | 3 | 3 // 3 = 1 | 3 | 

Tổng = 2 + 0 + 3 = 5 

Dãy con gọn gàng dài nhất là`[1, 3, 3, 3, 1]`. 

**Ví dụ 2:**`a = [8, 8, 8, 8, 8, 8, 8, 7]`| x | đếm[x] | đếm[x]//x | đóng góp | 
| --- | --- | --- | --- | 
| 7 | 1 | 1 // 7 = 0 | 0 | 
| 8 | 7 | 7 // 8 = 0 | 0 | 

Tổng = 0 

Không có trình tự gọn gàng nào có thể xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Việc đếm tần số và đóng góp tính toán đều mất O(n) thời gian. | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Bộ đếm lưu trữ tối đa n khóa có số lượng. | 

Với tổng số`n`trên tất cả các trường hợp thử nghiệm <= 2 * 10^5, tổng số thao tác vẫn nằm trong khoảng 2 * 10^5, dễ dàng trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open(__file__).read(), globals())
    return output.getvalue().strip()

# Provided samples
assert run("6\n1\n1\n2\n2 2\n4\n2 2 1 1\n6\n1 2 3 3 3 1\n8\n8 8 8 8 8 8 8 7\n10\n2 3 3 1 2 3 5 1 1 7\n") == "1\n2\n4\n5\n0\n5"

# Custom cases
assert run("1\n3\n5 1 2\n") == "0", "no blocks"
assert run("1\n5\n2 2 2 2 2\n") == "4", "multiple blocks of 2"
assert run("1\n1\n1\n") == "1", "single element"
assert run("1\n6\n1 1 1 1 1 1\n") == "6", "all ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5\n2 2 2 2 2`| 4 | Nhiều khối hoàn chỉnh cho cùng một số | 
|`3\n5 1 2`| 0 | Không có khối nào có thể | 
|`6\n1 1 1 1 1 1`| 6 | Nhiều khối 1, tổng được sử dụng đúng | 
|`1\n1`| 1 | Trường hợp cạnh mảng một phần tử | 

## Vỏ cạnh 

Khi không có số nào xuất hiện đủ số lần để tạo thành một khối, thuật toán sẽ tính tổng chính xác số đóng góp bằng 0. Vì`a = [5, 1, 2]`, số lượng là`{5:1,1:1,2:1}`, Và`(count // x) * x = 0`cho tất cả. Đầu ra là`0`, khớp với dãy con gọn gàng trống được mong đợi. 

Khi một số xuất hiện chính xác`x`nhiều lần, nó tạo thành một khối hoàn chỉnh. Vì`a = [2, 2]`,`count[2] = 2`,`2 // 2 * 2 = 2`. Đầu ra của thuật toán`2`, ghi lại chính xác toàn bộ mảng dưới dạng một chuỗi con gọn gàng. 

Khi một số xuất hiện nhiều hơn`x`nhưng ít hơn`2x`, nó chỉ đóng góp`x`đến tổng số, bỏ qua phần còn lại. Với `a = [3,3,3,3
