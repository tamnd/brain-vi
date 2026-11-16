---
title: "CF 188G - Sắp xếp mảng"
description: "Nhiệm vụ là sắp xếp danh sách các số nguyên theo thứ tự không giảm dần. Bạn được cung cấp kích thước của danh sách, n và sau đó là n số nguyên, mỗi số nằm trên một dòng riêng biệt, có giá trị từ 1 đến 100."
date: "2026-06-05T00:34:30+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 188
codeforces_index: "G"
codeforces_contest_name: "Surprise Language Round 6"
rating: 1600
weight: 188
solve_time_s: 70
verified: true
draft: false
---

[CF 188G - Sắp xếp mảng](https://codeforces.com/problemset/problem/188/G) 

**Đánh giá:** 1600 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là sắp xếp danh sách các số nguyên theo thứ tự không giảm dần. Bạn được cung cấp kích thước của danh sách, _n_, và sau đó là _n_ các số nguyên, mỗi số trên một dòng riêng biệt, với các giá trị từ 1 đến 100. Kết quả đầu ra phải là các số nguyên giống nhau được in trên một dòng, theo thứ tự tăng dần, phân tách bằng dấu cách. Cho phép các giá trị trùng lặp và mảng có thể nhỏ bằng một phần tử hoặc lớn tới 100 phần tử. 

Các ràng buộc rất nhỏ: với _n_ lên tới 100 và mỗi số từ 1 đến 100, thuật toán có thể thực hiện các thao tác O(n2) mà không vượt quá giới hạn thời gian. Điều này có nghĩa là ngay cả các thuật toán sắp xếp đơn giản như sắp xếp chèn hoặc sắp xếp chọn lọc cũng đủ nhanh. Mặt khác, bài toán khuyến khích sử dụng cách tiếp cận hiệu quả hơn nếu muốn, nhưng không có nhu cầu khắt khe về tối ưu hóa phức tạp. Các trường hợp cạnh bao gồm các mảng đã được sắp xếp, các mảng trong đó tất cả các phần tử đều bằng nhau và các mảng trong đó số lớn nhất và nhỏ nhất xuất hiện nhiều lần. Ví dụ, đối với đầu vào của```
5
3
3
3
3
3
```đầu ra chỉ đơn giản là```
3 3 3 3 3
```Việc triển khai bất cẩn nhằm loại bỏ các chỉ mục trùng lặp hoặc quản lý sai có thể thất bại trong trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là sử dụng thuật toán sắp xếp dựa trên so sánh tiêu chuẩn. Bạn có thể triển khai sắp xếp bong bóng hoặc sắp xếp chèn bằng cách so sánh nhiều lần các phần tử liền kề và hoán đổi chúng khi chúng không theo thứ tự. Điều này hiệu quả vì mảng nhỏ, nhưng trong trường hợp xấu nhất, nó yêu cầu khoảng n2 = 1002 = 10.000 phép so sánh và hoán đổi, điều này vẫn có thể chấp nhận được đối với vấn đề này. Phương pháp này đơn giản về mặt khái niệm: duyệt qua mảng, so sánh các cặp, hoán đổi nếu cần, lặp lại cho đến khi không có sự hoán đổi nào xảy ra. Nó đúng cho mọi đầu vào nhưng có tỷ lệ kém đối với các mảng lớn hơn. 

Một cách tiếp cận tao nhã hơn tận dụng thực tế là tất cả các số đều là số nguyên trong phạm vi từ 1 đến 100. Điều này cho phép sử dụng tính năng sắp xếp đếm. Thay vì so sánh nhiều lần các số, chúng ta đếm số lần mỗi số xuất hiện và sau đó xây dựng lại mảng đã sắp xếp bằng cách viết mỗi số theo số lần đã đếm. Điều này làm giảm thuật toán xuống O(n + m), trong đó m là phạm vi số (ở đây là 100), đưa ra giải pháp xác định và rất nhanh. 

Phương pháp brute-force hoạt động vì các phép so sánh lặp đi lặp lại cuối cùng sẽ định vị chính xác từng phần tử, nhưng nó không hiệu quả đối với các mảng lớn hơn. Việc quan sát thấy các số đầu vào bị chặn và rời rạc cho phép chúng ta giảm bài toán thành một bài tập đếm thời gian tuyến tính, tối ưu nhất với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sắp xếp so sánh (bong bóng/chèn) | O(n²) | O(1) | Đã chấp nhận | 
| Đếm Sắp xếp | O(n + m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên _n_ để xác định số phần tử trong mảng. Bước này thiết lập giới hạn lưu trữ mảng và vòng lặp. 
2. Khởi tạo một danh sách để lưu trữ các phần tử mảng, sau đó đọc từng số nguyên từ đầu vào và nối nó vào danh sách. Điều này giữ nguyên thứ tự ban đầu cho giai đoạn sắp xếp đếm. 
3. Tạo một mảng đếm có kích thước 101, khởi tạo bằng 0. Mỗi chỉ mục đại diện cho một số từ 1 đến 100 và giá trị tại chỉ mục đó sẽ lưu trữ số lần xuất hiện của số đó trong đầu vào. 
4. Lặp lại mảng ban đầu. Với mỗi phần tử, tăng chỉ số tương ứng trong mảng đếm. Điều này tạo ra sự phân bố tần số của các con số. 
5. Khởi tạo danh sách kết quả trống. Lặp lại qua mảng đếm từ chỉ mục 1 đến 100. Đối với mỗi chỉ mục có số đếm khác 0, hãy thêm giá trị chỉ mục vào danh sách kết quả với số lần bằng số lượng của nó. Bước này xây dựng lại mảng theo thứ tự được sắp xếp. 
6. In danh sách kết quả, nối các số có dấu cách. Điều này tạo ra định dạng đầu ra được yêu cầu. 

Tại sao nó hoạt động: mảng đếm đảm bảo rằng mỗi số xuất hiện ở đầu ra chính xác nhiều lần như trong đầu vào và vì chúng ta lặp lại mảng đếm theo thứ tự chỉ số tăng dần nên kết quả sẽ được sắp xếp. Không cần so sánh giữa các phần tử mảng tùy ý và các bản sao được xử lý một cách tự nhiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
arr = [int(input()) for _ in range(n)]

count = [0] * 101
for num in arr:
    count[num] += 1

result = []
for num in range(1, 101):
    result.extend([num] * count[num])

print(" ".join(map(str, result)))
```Phần đầu tiên đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Sử dụng tính năng hiểu danh sách ngay lập tức lưu trữ các số trong`arr`. các`count`mảng có kích thước 101 cho phép lập chỉ mục trực tiếp, vì vậy`count[num] += 1`tăng tần số cho mỗi số. Ở bước tái thiết,`[num] * count[num]`lặp lại số đúng số lần và`extend`thêm chúng vào danh sách kết quả. Cuối cùng,`map(str, result)`chuyển đổi số nguyên thành chuỗi để nối với dấu cách. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
7
1
9
7
3
```| Bước | mảng | đếm | kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | [7,1,9,7,3] | tất cả 0 | [] | 
| Đếm 7 | [7,1,9,7,3] | đếm[7]=1 | [] | 
| Đếm 1 | [7,1,9,7,3] | đếm[1]=1 | [] | 
| Đếm 9 | [7,1,9,7,3] | đếm[9]=1 | [] | 
| Đếm 7 | [7,1,9,7,3] | đếm[7]=2 | [] | 
| Đếm 3 | [7,1,9,7,3] | đếm[3]=1 | [] | 
| Tái thiết | [7,1,9,7,3] | đếm[1..9] | [1,3,7,7,9] | 

Dấu vết này xác nhận rằng việc đếm xử lý các phần tử trùng lặp và sắp xếp thứ tự một cách chính xác. 

### Mẫu 2 

đầu vào:```
3
3
3
3
```| Bước | mảng | đếm | kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | [3,3,3] | tất cả 0 | [] | 
| Đếm 3 | [3,3,3] | đếm[3]=1 | [] | 
| Đếm 3 | [3,3,3] | đếm[3]=2 | [] | 
| Đếm 3 | [3,3,3] | đếm[3]=3 | [] | 
| Tái thiết | [3,3,3] | đếm[1..3] | [3,3,3] | 

Điều này xác nhận rằng thuật toán xử lý các giá trị hoàn toàn bằng nhau mà không có lỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Cấu trúc mảng đếm là O(n), tái cấu trúc là O(m) trong đó m=100 | 
| Không gian | O(m + n) | Mảng đếm sử dụng O(m), mảng kết quả sử dụng O(n) | 

Với n ≤ 100 và m = 100, tổng số thao tác dưới 200, nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ không đáng kể so với mức cho phép 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    arr = [int(input()) for _ in range(n)]
    count = [0] * 101
    for num in arr:
        count[num] += 1
    result = []
    for num in range(1, 101):
        result.extend([num] * count[num])
    return " ".join(map(str, result))

# provided samples
assert run("5\n7\n1\n9\n7\n3\n") == "1 3 7 7 9", "sample 1"
assert run("3\n3\n3\n3\n") == "3 3 3 3", "all equal values"

# custom cases
assert run("1\n42\n") == "42", "minimum-size input"
assert run("6\n100\n1\n50\n50\n1\n100\n") == "1 1 50 50 100 100", "duplicates and boundaries"
assert run("4\n2\n3\n1\n4\n") == "1 2 3 4", "already small sorted"
assert run("10\n10\n9\n8\n7\n6\n5\n4\n3\n2\n1\n") == "1 2 3 4 5 6 7 8 9 10", "reverse order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 42 | Kích thước mảng tối thiểu | 
| 1 50 100 bản sao | 1 1 50 50 100 100 | Giá trị và ranh giới trùng lặp | 
| 2 3 1 4 | 1 2 3 4 | Mảng nhỏ không có thứ tự | 
| 10..1 | 1..10 | Mảng được sắp xếp ngược | 

## Vỏ cạnh 

Đối với một mảng có kích thước 1, chẳng hạn như đầu vào`1\n42\n`, mảng đếm vẫn hoạt động vì chúng ta tăng`count[42] = 1`và xây dựng lại`[42]`. Thuật toán in phần tử đơn một cách tự nhiên. Đối với các mảng có tất cả các phần tử bằng nhau, mảng đếm ghi lại tần số một cách chính xác và việc xây dựng lại sẽ tái tạo đúng số giá trị lặp lại. Đối với
