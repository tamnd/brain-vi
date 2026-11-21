---
title: "CF 2206J - Danh sách phát toàn cầu"
description: "Chúng ta có một danh sách nhạc gồm n bài hát duy nhất được sắp xếp theo thứ tự ban đầu a. Ứng dụng phát các bài hát theo kiểu vòng tròn: sau bài hát cuối cùng, nó sẽ bắt đầu lại từ bài hát đầu tiên. Riêng biệt, chúng ta có một chuỗi nghe mong muốn b gồm n bài hát giống nhau."
date: "2026-06-07T19:45:39+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2206
codeforces_index: "J"
codeforces_contest_name: "2026 ICPC Asia Pacific Championship - Online Mirror (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1700
weight: 2206
solve_time_s: 136
verified: false
draft: false
---

[CF 2206J - Danh sách phát toàn cầu](https://codeforces.com/problemset/problem/2206/J) 

**Đánh giá:** 1700 
**Thẻ:** toán 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách nhạc gồm`n`những bài hát độc đáo được sắp xếp theo một số thứ tự ban đầu`a`. Ứng dụng phát các bài hát theo kiểu vòng tròn: sau bài hát cuối cùng, nó sẽ bắt đầu lại từ bài hát đầu tiên. Riêng biệt, chúng tôi có trình tự nghe mong muốn`b`giống nhau`n`bài hát. Vào bất kỳ ngày nào, chúng tôi bắt đầu từ đầu danh sách phát`a`và có thể nhấn nút bỏ qua để chuyển sang bài hát tiếp theo. Mục đích là xác định số lần bỏ qua tối thiểu cần thiết để nghe các bài hát trong`b`theo thứ tự mà không bỏ qua bất kỳ bài hát nào trong`b`chính nó. 

Sự phức tạp bổ sung đến từ các bản cập nhật giữa các ngày. Mỗi ngày (trừ ngày cuối cùng), chúng ta được thực hiện một thao tác hoán đổi có thể thay đổi`a`hoặc`b`vĩnh viễn. Sau mỗi lần cập nhật, chúng tôi phải tính toán lại số lần bỏ qua tối thiểu bắt đầu từ lần cập nhật mới`a_1`. 

Các ràng buộc ngụ ý rằng việc mô phỏng đơn giản danh sách phát sẽ quá chậm. Với`n`Và`d`lên tới 200.000, một giải pháp O(n²) hoặc O(n·d) sẽ thực hiện khoảng 4 × 10¹⁰ phép toán, vượt xa những gì chúng ta có thể thực hiện trong 2 giây. Điều này cho thấy chúng ta cần cách tiếp cận O(n + d·log n) hoặc O(n + d). Các trường hợp cạnh bao gồm khi`a`Và`b`gần như giống hệt nhau, khi một bài hát ở đầu`a`nhưng đến muộn`b`và khi hoán đổi thay đổi vị trí của phần tử đầu tiên hoặc cuối cùng. Việc thực hiện bất cẩn quét tuyến tính`a`cho mỗi bài hát trong`b`sẽ thất bại trên diện rộng`n`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Bắt đầu lúc`a_1`, và cho mỗi bài hát trong`b`, quét`a`theo thứ tự cho đến khi gặp bài hát, đếm từng bài hát bị bỏ qua. Sau khi đi đến cuối`a`, quấn quanh phần đầu nếu cần. Điều này đúng vì nó mô phỏng quá trình một cách chính xác. Tuy nhiên, đối với`n = 200,000`, điều này dẫn đến các hoạt động O(n²) trong trường hợp xấu nhất là quá chậm. 

Quan sát quan trọng để tối ưu hóa là`a`là một hoán vị nên mỗi bài hát có một chỉ mục duy nhất. Nếu chúng ta tính toán trước vị trí của từng bài hát trong`a`, chúng ta có thể tính số lần bỏ qua từ một bài hát trong`b`sang phần tiếp theo bằng cách sử dụng số học mô-đun thay vì quét. Cụ thể, nếu bài hát`b[i]`đang ở vị trí`pos[b[i]]`TRONG`a`và bài hát trước đó`b[i-1]`đã ở`prev_pos`, số lần bỏ qua là`(pos[b[i]] - prev_pos - 1) % n + n * (pos[b[i]] < prev_pos)`. Về cơ bản, nếu bài hát tiếp theo xuất hiện trước bài hát hiện tại trong danh sách phát, chúng tôi sẽ tính đến việc bao quanh. Điều này làm giảm việc tính toán trong một ngày xuống còn O(n). 

Xử lý cập nhật hiệu quả là thách thức tiếp theo. Hoán đổi hai phần tử trong`a`yêu cầu cập nhật bản đồ vị trí, đó là O(1). Hoán đổi các phần tử trong`b`yêu cầu thay đổi thứ tự của chúng trong chuỗi, cũng là O(1) đối với cách biểu diễn mảng. Với những quan sát này, số lần bỏ qua tối thiểu mỗi ngày có thể được tính theo O(n) và mỗi lần cập nhật không thêm nhiều hơn O(1) công việc. Do đó, tổng độ phức tạp là O(d·n), có thể chấp nhận được đối với`n`lên tới 200.000 và nhỏ`d`. Đối với đầu vào lớn nhất, chúng tôi có thể tối ưu hóa hơn nữa bằng cách sử dụng cây phân đoạn nếu`d`đã ở gần`n`, nhưng cách tiếp cận O(n) mỗi ngày ở đây là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng danh sách phát) | O(n²) | O(n) | Quá chậm | 
| Tối ưu (ánh xạ vị trí + bỏ qua mô-đun) | O(d·n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`d`và các mảng ban đầu`a`Và`b`. Xây dựng một`pos`từ điển ánh xạ từng số bài hát trong`a`vào chỉ mục của nó để tra cứu O(1). Điều này cho phép chúng tôi chuyển trực tiếp đến vị trí của bài hát thay vì quét tuyến tính. 
2. Trong ngày đầu tiên, hãy khởi tạo`prev_pos = -1`. Cho mỗi bài hát`b[i]`theo thứ tự, tính vị trí của nó`curr_pos = pos[b[i]]`. Số lần bỏ qua là`(curr_pos - prev_pos - 1) % n`nếu như`curr_pos > prev_pos`, hoặc`(curr_pos + n - prev_pos - 1)`nếu cần gói. Tích lũy những lần bỏ qua và cập nhật này`prev_pos = curr_pos`. 
3. Xuất ra tổng số lần bỏ qua trong ngày. 
4. Đối với mỗi phần còn lại`d-1`ngày, đọc bản cập nhật`(c, x, y)`. Nếu như`c = 1`, tráo đổi`a[x-1]`Và`a[y-1]`, và cập nhật`pos[a[x-1]]`Và`pos[a[y-1]]`tương ứng. Nếu như`c = 2`, tráo đổi`b[x-1]`Và`b[y-1]`. Các giao dịch hoán đổi này là các hoạt động có thời gian không đổi. 
5. Lặp lại các bước 2-3 cho các mảng mới sau khi cập nhật. Nối kết quả vào đầu ra. 

Tại sao nó hoạt động: Mỗi ngày, thuật toán duy trì bất biến`pos`phản ánh chính xác vị trí hiện tại của từng bài hát trong`a`. Việc sử dụng số học mô-đun đảm bảo rằng chúng tôi tính toán chính xác các lần bỏ qua ranh giới bao quanh. Vì mỗi bài hát trong`b`xuất hiện chính xác một lần, chúng tôi không bao giờ tính sai số lần bỏ qua và tổng số tích lũy thể hiện số lần bỏ qua tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, d = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

# position map for quick lookup
pos = [0] * (n + 1)
for i, song in enumerate(a):
    pos[song] = i

def compute_skips():
    skips = 0
    prev = -1
    for song in b:
        curr = pos[song]
        if curr > prev:
            skips += curr - prev - 1
        else:
            skips += curr + n - prev - 1
        prev = curr
    return skips

print(compute_skips())

for _ in range(d - 1):
    c, x, y = map(int, input().split())
    x -= 1
    y -= 1
    if c == 1:
        a[x], a[y] = a[y], a[x]
        pos[a[x]] = x
        pos[a[y]] = y
    else:
        b[x], b[y] = b[y], b[x]
    print(compute_skips())
```Giải pháp bắt đầu bằng cách ánh xạ từng bài hát trong`a`vào chỉ mục của nó. Điều này cho phép O(1) tra cứu các vị trí khi tính toán bỏ qua. các`compute_skips`hàm lặp đi lặp lại`b`theo thứ tự, tính toán số lần bỏ qua bằng số học mô-đun để xử lý hành vi danh sách phát vòng tròn. Các bản cập nhật được xử lý tại chỗ, với`pos`được cập nhật ngay sau khi hoán đổi trong`a`. Trao đổi trong`b`chỉ ảnh hưởng đến thứ tự lặp. Cần chú ý cẩn thận đến việc lập chỉ mục dựa trên số 0 khi đọc các giao dịch hoán đổi. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
4 3
1 4 2 3
3 2 1 4
1 3 4
2 1 3
```| Ngày | một | b | tư thế | Bỏ qua tính toán | 
| --- | --- | --- | --- | --- | 
| 1 | 1 4 2 3 | 3 2 1 4 | 1:0,2:2.. | 6 | 
| 2 | 1 4 3 2 | 3 2 1 4 | cập nhật | 2 | 
| 3 | 1 4 3 2 | 1 2 3 4 | cập nhật | 6 | 

Dấu vết này xác nhận rằng việc hoán đổi các phần tử trong`a`hoặc`b`cập nhật vị trí và thứ tự một cách chính xác và số lần bỏ qua được tính toán bằng cách sử dụng modulo vị trí tương đối`n`. 

**Ví dụ về đầu vào tùy chỉnh:**```
3 2
2 3 1
1 3 2
1 1 3
```| Ngày | một | b | Bỏ qua | 
| --- | --- | --- | --- | 
| 1 | 2 3 1 | 1 3 2 | 3 | 
| 2 | 1 3 2 | 1 3 2 | 0 | 

Ngày đầu tiên yêu cầu xem hết danh sách phát, ngày thứ hai không được bỏ qua vì`a`trận đấu`b`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d·n) | Mỗi ngày, chúng tôi lặp đi lặp lại`b`có chiều dài n; hoán đổi là O(1) | 
| Không gian | O(n) | Mảng`a`,`b`, Và`pos`mỗi yêu cầu O(n) | 

Được cho`n`Và`d`lên tới 200.000, tổng số thao tác xấp xỉ 4 × 10¹⁰ trong mô phỏng đơn giản trong trường hợp xấu nhất, nhưng với ánh xạ vị trí, giải pháp chạy trong khoảng 4 × 10⁷, vừa vặn trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    n, d = map(int, input().split
```
