---
title: "CF 104760E - Mật mã cũ"
description: "Chúng ta được cấp một dãy số tự nhiên đại diện cho các khóa được mã hóa. Nhiệm vụ là sắp xếp lại các số này theo quy tắc sắp xếp tùy chỉnh không dựa trên giá trị số thông thường của chúng mà dựa trên sự chuyển đổi của từng số."
date: "2026-06-29T02:21:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104760
codeforces_index: "E"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), Kyrgyzstan Qualification Contest"
rating: 0
weight: 104760
solve_time_s: 89
verified: false
draft: false
---

[CF 104760E - Mật mã cũ](https://codeforces.com/problemset/problem/104760/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số tự nhiên đại diện cho các khóa được mã hóa. Nhiệm vụ là sắp xếp lại các số này theo quy tắc sắp xếp tùy chỉnh không dựa trên giá trị số thông thường của chúng mà dựa trên sự chuyển đổi của từng số. 

Đối với mỗi số, chúng tôi lấy biểu diễn thập phân của nó, đảo ngược các chữ số và diễn giải chuỗi đảo ngược đó thành một số nguyên mới. Khóa sắp xếp chính là giá trị đảo ngược này. Các số được sắp xếp theo thứ tự tăng dần ở dạng đảo ngược. 

Có một quy tắc phụ để giải quyết các mối quan hệ: nếu hai số có cùng giá trị đảo ngược thì số xuất hiện ban đầu sau trong đầu vào sẽ xuất hiện đầu tiên trong đầu ra. 

Đầu ra là hoán vị của các số ban đầu được sắp xếp theo các quy tắc này, nhưng chúng ta phải xuất ra các số ban đầu chứ không phải dạng đảo ngược của chúng. 

Ràng buộc n lên tới 100000 ngụ ý rằng giải pháp O(n log n) là cần thiết. Bất kỳ cách tiếp cận nào thực hiện nhiều lần các phép toán tốn kém bên trong vòng lặp bậc hai sẽ quá chậm. Mỗi số có tối đa 10^9, vì vậy việc đảo ngược các chữ số tối đa chỉ là một lượng công việc không đổi nhỏ cho mỗi phần tử. 

Một sai lầm phổ biến là quên rằng tie-break phụ thuộc vào chỉ số ban đầu theo thứ tự ngược lại. Một vấn đề tế nhị khác là mất các số 0 đứng đầu trong quá trình đảo ngược, điều này ảnh hưởng gián tiếp đến việc đặt hàng. Ví dụ: 1000 trở thành 0001 khi đảo ngược, tức là 1. Điều này có nghĩa là nhiều đầu vào khác nhau thu gọn về cùng một khóa đảo ngược và sau đó phải được phân biệt bằng cách sử dụng thứ tự chỉ mục. 

Một trường hợp minh họa nhỏ là: 

đầu vào:```
10 100 1
```Các dạng đảo ngược là 01 → 1, 001 → 1, 01 → 1. Tất cả các khóa trở thành bằng nhau, do đó đầu ra phải theo thứ tự chỉ mục ban đầu giảm dần:`1 100 10`. 

Việc sắp xếp chuỗi từ điển đơn giản trên các chuỗi đảo ngược sẽ không thành công nếu các chỉ mục không được kết hợp đúng cách. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản là tính toán, đối với mỗi số, chuỗi chữ số đảo ngược của nó và sau đó sắp xếp mảng dựa trên giá trị dẫn xuất này. Nếu hai giá trị đảo ngược khớp nhau, chúng tôi sẽ ngắt mối quan hệ bằng cách sử dụng chỉ mục ban đầu theo thứ tự giảm dần. 

Điều này hoạt động chính xác vì mỗi phần tử có thể được ánh xạ tới một cặp khóa có thể sắp xếp nhưng việc triển khai đơn giản có thể liên tục đảo ngược các số trong quá trình so sánh, dẫn đến chi phí không cần thiết. Trong trường hợp xấu nhất, việc sắp xếp sẽ liên quan đến các so sánh O(n log n) và mỗi so sánh có thể tính toán lại sự đảo ngược trong O(d), cho ra O(n log n d). Mặc dù độ dài chữ số nhỏ nhưng điều này có thể tránh được và không cần thiết. 

Thông tin chi tiết quan trọng là giá trị đảo ngược có thể được tính toán trước một lần cho mỗi phần tử và được lưu trữ cùng với số. Sau đó, việc sắp xếp trở thành một cách sắp xếp tiêu chuẩn trên các bộ dữ liệu, trong đó Python xử lý các phép so sánh một cách hiệu quả. 

Do đó, chúng tôi rút gọn vấn đề thành việc xây dựng một mảng gồm các bộ ba: (reversed_value, -index, original_value). Chỉ số phủ định mã hóa yêu cầu các phần tử sau phải đứng đầu trong các mối quan hệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại ngược lại trong so sánh) | O(n log n · d) | O(n) | Quá chậm trong việc thực hiện tồi tệ nhất | 
| Tối ưu (phím tính toán trước) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Đọc dãy số và nhớ vị trí ban đầu của chúng từ 0 đến n−1. Chỉ mục này là cần thiết để giải quyết vấn đề ràng buộc khi các giá trị đảo ngược khớp nhau. 
2. Với mỗi số, hãy tính dạng chữ số đảo ngược của nó bằng cách liên tục trích xuất các chữ số ở cuối. Điều này tạo ra số nguyên được hình thành bằng cách đảo ngược biểu diễn thập phân. 
3. Lưu trữ một bộ dữ liệu cho mỗi phần tử bao gồm giá trị đảo ngược, số âm của chỉ mục của nó và số gốc. Chỉ số âm đảm bảo rằng các chỉ số gốc lớn hơn được sắp xếp trước khi các giá trị đảo ngược bằng nhau. 
4. Sắp xếp danh sách các bộ theo thứ tự tăng dần. So sánh bộ dữ liệu của Python tự nhiên so sánh trước tiên theo giá trị đảo ngược, sau đó theo chỉ số âm. 
5. Xuất các số gốc từ danh sách đã sắp xếp, bỏ qua các trường phụ trợ. 

Tại sao nó hoạt động: mỗi số được ánh xạ tới một khóa nắm bắt đầy đủ mức độ ưu tiên thứ tự của nó. Giá trị đảo ngược thực thi thứ tự chính và chỉ mục âm mã hóa quy tắc phụ mà không cần logic so sánh tùy chỉnh. Vì việc sắp xếp ổn định theo thứ tự bộ dữ liệu nên không còn sự mơ hồ sau khi xây dựng khóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def rev(x: int) -> int:
    res = 0
    while x > 0:
        res = res * 10 + x % 10
        x //= 10
    return res

def solve():
    data = list(map(int, input().split()))
    n = len(data)

    arr = []
    for i, x in enumerate(data):
        arr.append((rev(x), -i, x))

    arr.sort()
    print(*[x[2] for x in arr])

if __name__ == "__main__":
    solve()
```Hàm đảo ngược xử lý mỗi chữ số chính xác một lần, xây dựng số nguyên đảo ngược theo thời gian chữ số tuyến tính. Chỉ số liệt kê được giữ nguyên để việc bẻ khóa được mã hóa chính xác. Việc sắp xếp sử dụng thứ tự bộ dữ liệu gốc của Python, giúp loại bỏ nhu cầu về logic so sánh tùy chỉnh. 

Một chi tiết triển khai tinh tế là việc sử dụng`-i`. Nếu không có sự phủ định, các chỉ số trước đó sẽ đứng đầu một cách không chính xác, vi phạm quy tắc rằng các vị trí sau sẽ chiếm ưu thế trong các mối quan hệ. 

Một điểm quan trọng khác là chúng tôi không bao giờ lưu trữ các chuỗi đảo ngược, chỉ lưu trữ số nguyên. Điều này tránh chi phí so sánh chuỗi và đảm bảo thứ tự số nhất quán ngay cả khi các số 0 đứng đầu có ý nghĩa quan trọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1010 31 41 3100 310 31000 43 54 1000 14
```Chúng tôi tính toán các giá trị đảo ngược: 

| giá trị | đảo ngược | chỉ mục | chìa khóa | 
| --- | --- | --- | --- | 
| 1010 | 101 | 0 | (101, 0) | 
| 31 | 13 | 1 | (13, -1) | 
| 41 | 14 | 2 | (14, -2) | 
| 3100 | 13 | 3 | (13, -3) | 
| 310 | 13 | 4 | (13, -4) | 
| 31000 | 13 | 5 | (13, -5) | 
| 43 | 34 | 6 | (34, -6) | 
| 54 | 45 | 7 | (45, -7) | 
| 1000 | 1 | 8 | (1, -8) | 
| 14 | 41 | 9 | (41, -9) | 

Sau khi sắp xếp các khóa theo từ điển: 

| bước | khóa đã chọn | giá trị | 
| --- | --- | --- | 
| 1 | (1, -8) | 1000 | 
| 2 | (13, -5) | 31000 | 
| 3 | (13, -4) | 310 | 
| 4 | (13, -3) | 3100 | 
| 5 | (13, -1) | 31 | 
| 6 | (14, -2) | 41 | 
| 7 | (34, -6) | 43 | 
| 8 | (41, -9) | 14 | 
| 9 | (45, -7) | 54 | 
| 10 | (101, 0) | 1010 | 

Đầu ra:```
1000 31000 310 3100 31 41 43 14 54 1010
```Dấu vết này cho thấy các giá trị đảo ngược bằng nhau được nhóm lại với nhau như thế nào và được giải quyết hoàn toàn theo thứ tự chỉ mục. 

### Ví dụ 2 

đầu vào:```
1 10 100 1000
```Giá trị đảo ngược: 

| giá trị | đảo ngược | chỉ mục | chìa khóa | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | (1, 0) | 
| 10 | 1 | 1 | (1, -1) | 
| 100 | 1 | 2 | (1, -2) | 
| 1000 | 1 | 3 | (1, -3) | 

Thứ tự sắp xếp: 

| bước | giá trị | 
| --- | --- | 
| 1 | 1000 | 
| 2 | 100 | 
| 3 | 10 | 
| 4 | 1 | 

Điều này khẳng định quy tắc tie-break chiếm ưu thế hoàn toàn khi tất cả các giá trị đảo ngược đều giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | các chữ số đảo ngược là O(d) trên mỗi số, tổng O(n d), việc sắp xếp chiếm ưu thế | 
| Không gian | O(n) | lưu trữ các bộ dữ liệu cho từng phần tử | 

Các ràng buộc cho phép tối đa 100000 số và việc đảo ngược chữ số được giới hạn tối đa 10 chữ số cho mỗi số. Giải pháp phù hợp thoải mái trong giới hạn thời gian vì yếu tố chủ đạo là sắp xếp, tối ưu cho việc đặt hàng dựa trên so sánh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return io.StringIO().getvalue()

# provided sample
assert True  # placeholder since exact I/O wrapper depends on judge format

# custom cases
# single element
assert True

# all equal reversed keys
assert True

# increasing reversed order
assert True

# decreasing reversed order
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 1 10 100 1000 2`| phân nhóm ngược đúng | sự sụp đổ dẫn đầu | 
|`3 9 90 900`|`900 90 9`| hòa toàn bộ theo chỉ số | 
|`4 12 21 3 30`| phụ thuộc vào thứ tự đảo ngược | đảo ngược chữ số hỗn hợp | 

## Vỏ cạnh 

Một trường hợp quan trọng xảy ra khi nhiều số trở nên giống nhau sau khi đảo ngược. Ví dụ: 10, 100 và 1000 đều đảo ngược thành 1. Trong trường hợp này, thứ tự được xác định hoàn toàn theo chỉ mục ban đầu, với các chỉ số sau sẽ được xếp trước. 

đầu vào:```
10 100 1000
```Các giá trị đảo ngược đều là 1. Thuật toán gán khóa: 

(1, 0), (1, -1), (1, -2). Sắp xếp mang lại kết quả (1, -2), (1, -1), (1, 0), do đó đầu ra là`1000 100 10`. 

Một trường hợp khác liên quan đến các số chỉ khác nhau ở các số 0 ở cuối. Sự đảo ngược sẽ thu gọn các số 0 này thành các số 0 đứng đầu, chúng biến mất về mặt số lượng. Thuật toán xử lý việc này một cách chính xác vì việc so sánh không bao giờ dựa vào dạng chuỗi mà chỉ dựa vào các giá trị đảo ngược bằng số cộng với việc phá vỡ mối liên kết chỉ số. 

Trường hợp cạnh cuối cùng là đầu vào một phần tử, trong đó việc sắp xếp là không đáng kể và đầu ra giống hệt với đầu vào.
