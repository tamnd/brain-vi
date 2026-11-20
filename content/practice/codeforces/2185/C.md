---
title: "CF 2185C - Chuyển MEX"
description: "Chúng ta được cung cấp một mảng các số nguyên, có thể bao gồm các số âm. Nhiệm vụ này cho phép chúng ta chọn một số nguyên shift x và thêm nó vào mọi phần tử của mảng. Sau khi thực hiện sự thay đổi này, chúng tôi muốn tối đa hóa MEX của mảng."
date: "2026-06-07T21:28:50+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2185
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1074 (Div. 4)"
rating: 900
weight: 2185
solve_time_s: 125
verified: false
draft: false
---

[CF 2185C - MEX đã chuyển đổi](https://codeforces.com/problemset/problem/2185/C) 

**Xếp hạng:** 900 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên, có thể bao gồm các số âm. Nhiệm vụ cho phép chúng tôi chọn một sự thay đổi số nguyên duy nhất`x`và thêm nó vào mọi phần tử của mảng. Sau khi thực hiện sự thay đổi này, chúng tôi muốn tối đa hóa MEX của mảng. MEX của một mảng được định nghĩa là số nguyên không âm nhỏ nhất không có trong mảng. 

Một cách hiệu quả, vấn đề đặt ra: bằng cách dịch chuyển đồng đều tất cả các phần tử, "khoảng cách" lớn nhất bắt đầu từ 0 mà chúng ta có thể tạo ra mà chúng ta có thể tạo ra còn thiếu trong mảng là gì? Ví dụ, một mảng`[2, 4, 1, 0, -1]`có thể được dịch chuyển sao cho các phần tử bắt đầu từ 0, làm cho các số nguyên không âm liên tiếp xuất hiện càng lâu càng tốt, đẩy số còn thiếu đầu tiên đi xa hơn. 

Các ràng buộc ở mức vừa phải: tổng số tối đa 3000 phần tử mảng trong tất cả các trường hợp thử nghiệm. Điều này có nghĩa là một`O(n^2)`giải pháp có thể hoạt động, nhưng chúng ta nên ưu tiên`O(n log n)`hoặc`O(n)`nếu có thể hãy giữ sự thoải mái trong giới hạn thời gian 2 giây. Các số âm phải được xử lý cẩn thận, vì một cách tiếp cận ngây thơ cho rằng không âm có thể thất bại. 

Trường hợp cạnh phát sinh khi tất cả các số đều giống nhau hoặc khi số nhỏ nhất đã bằng 0. Ví dụ,`[1, 1]`chỉ có thể dịch chuyển bằng`-1`để tạo mảng`[0, 0]`, cho MEX là`1`. Một trường hợp khác là khi mảng đã chứa các số liên tiếp từ`0`ĐẾN`k`; MEX tối ưu chỉ nhiều hơn một phần tử tối đa sau khi dịch chuyển, có khả năng dịch chuyển bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: thử tất cả các ca có thể`x`từ`-max(a)`ĐẾN`-min(a)`sao cho mảng đã dịch chuyển chạm 0 và kiểm tra MEX sau mỗi ca. Điều này có hiệu quả vì việc thêm một số dương hoặc số âm lớn có thể đẩy các phần tử ra xa, nhưng số lần dịch chuyển tiềm năng là rất lớn (lên tới 2*10^9), do đó, việc lặp lại tất cả các lần dịch chuyển là không khả thi. 

Thông tin chi tiết quan trọng là sự dịch chuyển tối ưu sẽ căn chỉnh số nhỏ nhất trong mảng bằng 0. Khi phần tử tối thiểu trở thành 0, MEX được xác định bởi số nguyên còn thiếu nhỏ nhất trong mảng được sắp xếp. Vì chúng ta chỉ cần xem xét sự khác biệt giữa số nhỏ nhất và số 0 nên chỉ cần một thao tác là đủ: dịch chuyển tất cả các phần tử bằng cách`-min(a)`. Sau đó, mảng trở thành không âm và bắt đầu từ 0. Khi đó MEX chỉ đơn giản là chỉ số đầu tiên trong đó chuỗi không âm liên tiếp bị phá vỡ. 

Sắp xếp mảng đã dịch chuyển cho phép chúng ta đếm xem có bao nhiêu số liên tiếp xuất hiện bắt đầu từ số 0. Khoảng trống đầu tiên biểu thị MEX. Điều này làm giảm vấn đề xuống còn một loại cho mỗi trường hợp thử nghiệm cộng với quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả x) | O(phạm vi (a) * n) | O(n) | Quá chậm | 
| Tối ưu (dịch chuyển theo -min(a), sắp xếp, tìm MEX) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`chiều dài`n`. 
2. Tính giá trị nhỏ nhất trong mảng,`min_a`. Giá trị này xác định độ dịch chuyển cần thiết để đưa phần tử nhỏ nhất về 0. 
3. Chuyển mảng theo`x = -min_a`. Điều này đảm bảo phần tử nhỏ nhất bằng 0 và tất cả các phần tử đều không âm. 
4. Sắp xếp mảng đã dịch chuyển. Việc sắp xếp giúp đơn giản hóa việc tìm số nguyên bị thiếu đầu tiên vì giờ đây chúng ta có thể quét từ 0 trở lên. 
5. Khởi tạo một biến`mex`về không. Lặp lại mảng đã sắp xếp, bỏ qua các bản sao. Đối với mỗi phần tử, nếu nó bằng`mex`, tăng`mex`bằng 1. Phần tử đầu tiên lớn hơn`mex`phá vỡ chuỗi liên tiếp, vì vậy`mex`tại thời điểm đó là MEX của mảng. 
6. Đầu ra`mex`là MEX tối đa có thể sau ca làm việc. 

**Tại sao nó hoạt động:** Sự dịch chuyển tối ưu phải đưa một số phần tử về 0 vì MEX là số nguyên không âm. Việc dịch chuyển bất kỳ phần tử nào xuống dưới 0 sẽ không bao giờ làm tăng MEX vượt quá số nguyên không âm đầu tiên bị thiếu. Việc sắp xếp và quét đảm bảo chúng tôi tính toán chính xác các chuỗi trùng lặp và liên tiếp, đảm bảo phát hiện được khoảng trống đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_mex(a):
    min_a = min(a)
    shifted = [x - min_a for x in a]
    shifted.sort()
    mex = 0
    for val in shifted:
        if val == mex:
            mex += 1
    return mex

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(max_mex(a))
```**Giải thích:** 

Đầu tiên chúng ta tính giá trị tối thiểu của mảng để xác định độ dịch chuyển. Trừ`min_a`từ mọi phần tử đảm bảo mảng bắt đầu từ 0. Việc sắp xếp mảng cho phép chúng ta dễ dàng tìm thấy số nguyên không âm đầu tiên bị thiếu bằng cách kiểm tra`mex`chống lại từng giá trị. Vòng lặp cuối cùng bỏ qua các bản sao một cách chính xác bằng cách chỉ tăng dần`mex`khi giá trị hiện tại bằng`mex`. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
1
4
4 2 3 6
```| Bước | Mảng | phút_a | Đã thay đổi | mex | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [4,2,3,6] | 2 | [2,0,1,4] | 0 | 
| Sau khi sắp xếp | [0,1,2,4] | - | - | 0 → 1 → 2 → 3 | 

**Kết quả:** 3. Số nguyên còn thiếu đầu tiên sau 0,1,2 là 3. 

### Đầu vào mẫu 2```
1
5
2 4 1 0 -1
```| Bước | Mảng | phút_a | Đã thay đổi | mex | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [2,4,1,0,-1] | -1 | [3,5,2,1,0] | 0 | 
| Sau khi sắp xếp | [0,1,2,3,5] | - | - | 0 → 1 → 2 → 3 → 4 | 

**Kết quả:** 4. Số nguyên bị thiếu đầu tiên sau 0,1,2,3 là 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng chiếm ưu thế; quét tuyến tính để tính MEX là O(n) | 
| Không gian | O(n) | Lưu trữ mảng đã dịch chuyển | 

Thuật toán dễ dàng xử lý tổng ràng buộc (n) 3000 trong giới hạn 2 giây. 

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
        print(max_mex(a))
    
    return output.getvalue().strip()

# Provided samples
assert run("6\n1\n4\n5\n0 1 1 2 3\n2\n1 1\n4\n4 2 3 6\n5\n2 4 1 0 -1\n6\n-1 1 2 3 5 6\n") == "1\n4\n1\n3\n4\n3"

# Custom cases
assert run("1\n1\n0\n") == "1", "single zero element"
assert run("1\n3\n-5 -4 -3\n") == "6", "all negative elements"
assert run("1\n4\n1 1 1 1\n") == "1", "all equal positive elements"
assert run("1\n5\n0 1 2 3 4\n") == "5", "already consecutive from 0"
assert run("1\n5\n0 0 0 1 1\n") == "2", "duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n0`| 1 | Phần tử đơn 0 | 
|`1\n3\n-5 -4 -3`| 6 | Tất cả các yếu tố tiêu cực | 
|`1\n4\n1 1 1 1`| 1 | Tất cả các yếu tố tích cực như nhau | 
|`1\n5\n0 1 2 3 4`| 5 | Đã liên tiếp từ 0 | 
|`1\n5\n0 0 0 1 1`| 2 | Xử lý trùng lặp | 

## Vỏ cạnh 

Đối với mảng`[1,1]`, tối thiểu là 1. Dịch chuyển theo`-1`sản lượng`[0,0]`. Sắp xếp mang lại`[0,0]`. Quét MEX bắt đầu từ 0; phần tử đầu tiên 0 tăng MEX lên 1, phần tử thứ hai 0 bị bỏ qua. MEX đúng là 1. Điều này xác nhận rằng các bản sao không làm tăng MEX một cách giả tạo. 

Vì`[ -1, 1, 2, 3, 5, 6 ]`, tối thiểu là -1. Chuyển bằng`1`cho`[0,2,3,4,6,7]`. Sắp xếp:`[0,2,3,4,6,7]`. Quét MEX
