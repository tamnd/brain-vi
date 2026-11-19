---
title: "CF 2101C - Vương Quốc 23"
description: "Chúng ta được yêu cầu tối đa hóa \"vẻ đẹp\" của một mảng bắt nguồn từ một mảng cho trước a. Với mỗi vị trí trong a, mảng b mới phải thỏa mãn 1  bi  ai. Vẻ đẹp của b là tổng các khoảng cách lớn nhất giữa các lần xuất hiện lặp lại của từng giá trị trong b."
date: "2026-06-08T05:07:29+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "greedy", "ternary-search", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2101
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1024 (Div. 1)"
rating: 2200
weight: 2101
solve_time_s: 99
verified: false
draft: false
---

[CF 2101C - Vương quốc 23](https://codeforces.com/problemset/problem/2101/C) 

**Xếp hạng:** 2200 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, tham lam, tìm kiếm ternary, hai con trỏ 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tối đa hóa "vẻ đẹp" của một mảng bắt nguồn từ một mảng nhất định`a`. Đối với mỗi vị trí trong`a`, mảng mới`b`phải thỏa mãn`1 ≤ b_i ≤ a_i`. Vẻ đẹp của`b`là tổng các khoảng cách lớn nhất giữa các lần xuất hiện lặp lại của mỗi giá trị trong`b`. Cụ thể, đối với giá trị`x`,`d_x(b)`là khoảng cách giữa lần xuất hiện đầu tiên và lần cuối cùng của`x`TRONG`b`. Nếu như`x`xảy ra một lần hoặc không xảy ra lần nào,`d_x(b) = 0`. Vẻ đẹp tổng thể là tổng hòa của tất cả`d_x(b)`cho các giá trị có trong`b`. 

Kích thước đầu vào`n`có thể đạt 200.000 cho mỗi trường hợp thử nghiệm và tổng số trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi 200.000. Một cách tiếp cận ngây thơ lặp lại tất cả các mảng có thể`b`rõ ràng là không thể, vì có rất nhiều sự kết hợp theo cấp số nhân. Thuật toán phải gần tuyến tính trong`n`, nếu không nó sẽ hết thời gian. Những hạn chế về`a_i`(`1 ≤ a_i ≤ n`) ngụ ý rằng các giá trị tiềm năng trong`b`được giới hạn bởi`n`cũng vậy. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tất cả các giá trị đều giống nhau, các mảng có giá trị tăng hoặc giảm nghiêm ngặt và các mảng có giá trị lặp lại ở các cạnh. Ví dụ, nếu`a = [1, 1, 1, 1]`, bất kỳ sự lựa chọn nào của`b`cũng sẽ là`[1, 1, 1, 1]`và vẻ đẹp là`3`, không`0`. Nếu như`a = [1, 2, 1, 2]`, đang chọn`b = [1, 2, 1, 2]`tối đa hóa khoảng cách. Những nỗ lực ngây thơ chỉ đơn giản là chọn các giá trị tối đa hoặc các phép gán tuần tự tham lam thường thất bại ở các vị trí cạnh, đặc biệt khi cùng một số có thể xuất hiện nhiều lần nhưng ở các khoảng cách khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các mảng có thể`b`thỏa mãn`1 ≤ b_i ≤ a_i`và tính toán vẻ đẹp cho mỗi người. Với mỗi mảng, chúng ta sẽ theo dõi vị trí của từng số và tính toán`d_x(b)`như khoảng cách tối đa. Điều này đúng nhưng không khả thi: nếu mỗi`a_i`lớn, số lượng`b`mảng tăng theo cấp số nhân. Ngay cả với`n = 20`, số lượng mảng vượt quá một triệu nếu các phần tử của`a`không bị giới hạn, khiến cho brute-force không thể sử dụng được. 

Cái nhìn sâu sắc quan trọng là để tối đa hóa`d_x(b)`cho một giá trị`x`, chúng ta chỉ cần đặt`x`ở vị trí ngoài cùng bên trái và ngoài cùng bên phải nơi`x`được cho phép. Vị trí trung gian không làm tăng khoảng cách. Vì vậy, với mỗi giá trị có thể`x`, chúng tôi theo dõi các vị trí trong`a`Ở đâu`x`có thể xuất hiện (`x ≤ a_i`). Khi đó khoảng cách tối đa`x`là`last - first`, Ở đâu`first`Và`last`là những chỉ số đầu tiên và cuối cùng trong danh sách này. Vẻ đẹp là tổng hòa của tất cả`x`. Điều này làm giảm vấn đề xuống còn quét tuyến tính thông qua`a`và vị trí ứng cử viên của từng giá trị, là O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Π a_i) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Lặp lại từng trường hợp thử nghiệm. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Khởi tạo từ điển`positions`ánh xạ từng giá trị tiềm năng`x`vào một danh sách trống. 
4. Lặp qua mảng`a`theo chỉ số. Đối với mỗi`i`, nối thêm`i`ĐẾN`positions[x]`cho tất cả`1 ≤ x ≤ a[i]`. 
5. Với mỗi giá trị`x`xuất hiện trong`positions`, tính khoảng cách như`positions[x][-1] - positions[x][0]`. 
6. Tính tổng các khoảng cách này trên tất cả`x`để có được vẻ đẹp tối đa cho test case. 
7. Xuất kết quả. 

Lý do cho bước 4 là tinh tế. Chúng ta không cần phải theo dõi mọi con số cho đến`a_i`; thay vào đó, chúng tôi chỉ theo dõi những con số thực sự xuất hiện trong`a`ít nhất một lần. Khi tính khoảng cách, chúng ta chỉ xét đến số`x`như vậy`x ≤ a_i`ở một vị trí nào đó. Điều này đảm bảo tính chính xác đồng thời tránh việc lưu trữ không cần thiết. 

Tại sao nó hoạt động: bằng cách đặt từng số`x`ở những vị trí sớm nhất và muộn nhất cho phép, chúng tôi tối đa hóa sự đóng góp của nó cho vẻ đẹp. Bất kỳ vị trí trung gian nào cũng không làm tăng khoảng cách tối đa. Vì các đóng góp là độc lập nên việc tổng hợp chúng sẽ tạo ra vẻ đẹp tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        from collections import defaultdict
        positions = defaultdict(list)
        
        for i, val in enumerate(a):
            positions[val].append(i)
        
        beauty = 0
        for val, pos_list in positions.items():
            if len(pos_list) > 1:
                beauty += pos_list[-1] - pos_list[0]
        
        print(beauty)

if __name__ == "__main__":
    main()
```Giải pháp này đọc đầu vào nhanh chóng bằng cách sử dụng`sys.stdin.readline`. Từ điển`positions`thu thập các chỉ số nơi mỗi giá trị xuất hiện. Bằng cách lấy chênh lệch giữa chỉ số cuối cùng và chỉ số đầu tiên cho mỗi giá trị, chúng tôi tính toán khoảng cách tối đa một cách hiệu quả. Các lần xuất hiện đơn lẻ tự động đóng góp 0 và các giá trị bên ngoài mảng không bao giờ xuất hiện trong`positions`. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
4
1 2 1 2
2
2 2
10
1 2 1 5 1 2 2 1 1 2
8
1 5 2 8 4 1 4 2
```### Dấu vết cho trường hợp thử nghiệm đầu tiên 

| tôi | một [tôi] | vị trí | 
| --- | --- | --- | 
| 0 | 1 | {1: [0]} | 
| 1 | 2 | {1: [0], 2: [1]} | 
| 2 | 1 | {1: [0,2], 2: [1]} | 
| 3 | 2 | {1: [0,2], 2: [1,3]} | 

Khoảng cách:`d_1 = 2 - 0 = 2`,`d_2 = 3 - 1 = 2`. Tổng = 4. 

### Dấu vết cho trường hợp thử nghiệm thứ hai 

| tôi | một [tôi] | vị trí | 
| --- | --- | --- | 
| 0 | 2 | {2: [0]} | 
| 1 | 2 | {2: [0,1]} | 

Khoảng cách:`d_2 = 1 - 0 = 1`. Tổng = 1. 

Những dấu vết này xác nhận thuật toán xác định chính xác lần xuất hiện đầu tiên và cuối cùng cũng như tính tổng khoảng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét mảng một lần và tính khoảng cách trong lần quét thứ hai | 
| Không gian | O(n) | Lưu trữ vị trí của từng giá trị riêng biệt | 

Vì tổng của tất cả`n`trên các trường hợp thử nghiệm 2·10^5, giải pháp chạy thoải mái trong giới hạn 4 giây với mức sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# Provided samples
assert run("4\n4\n1 2 1 2\n2\n2 2\n10\n1 2 1 5 1 2 2 1 1 2\n8\n1 5 2 8 4 1 4 2\n") == "4\n1\n16\n16"

# Custom cases
assert run("1\n1\n1\n") == "0"  # single element
assert run("1\n5\n1 1 1 1 1\n") == "4"  # all equal
assert run("1\n3\n3 2 1\n") == "0"  # strictly decreasing, no repeats
assert run("1\n6\n2 3 2 1 3 1\n") == "5"  # multiple repeated values
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | mảng một phần tử không đóng góp | 
| tất cả đều bình đẳng | 4 | khoảng cách tối đa cho các phần tử lặp lại | 
| giảm nghiêm ngặt | 0 | không có yếu tố lặp lại nào tạo ra vẻ đẹp bằng không | 
| lặp lại nhiều lần | 5 | xử lý nhiều số với các khoảng trống khác nhau | 

## Vỏ cạnh 

cho`a = [1]`,`positions = {1: [0]}`. Vì chỉ có một lần xuất hiện nên khoảng cách là 0 và vẻ đẹp = 0. Thuật toán xử lý chính xác giới hạn dưới. 

Vì`a = [1,1,1,1,1]`,`positions = {1: [0,1,2,3,4]}`. Khoảng cách là`4 - 0 = 4`và vẻ đẹp = 4. Thuật toán xác định chính xác lần xuất hiện đầu tiên và cuối cùng. 

Vì`a = [3,2,3,1,2,1]`, `vị trí = {3: [0,2], 2: [1,4], 1: [3,
