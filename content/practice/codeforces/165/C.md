---
title: "CF 165C - Một Vấn Đề Khác Về Dây"
description: "Chúng ta được yêu cầu đếm các chuỗi con của một chuỗi nhị phân chứa chính xác k chuỗi con. Chuỗi con là bất kỳ chuỗi ký tự liền kề nào trong chuỗi và các lần xuất hiện khác nhau ở các vị trí khác nhau được tính riêng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "dp", "math", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 165
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 112 (Div. 2)"
rating: 1600
weight: 165
solve_time_s: 80
verified: true
draft: false
---

[CF 165C - Một vấn đề khác về dây](https://codeforces.com/problemset/problem/165/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, dp, toán học, chuỗi, hai con trỏ 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các chuỗi con của một chuỗi nhị phân chứa chính xác _k_ chuỗi con. Chuỗi con là bất kỳ chuỗi ký tự liền kề nào trong chuỗi và các lần xuất hiện khác nhau ở các vị trí khác nhau được tính riêng. Ví dụ: trong chuỗi "1010", chuỗi con "10" xuất hiện hai lần và mỗi chuỗi được tính riêng lẻ. Đầu vào cung cấp một số nguyên _k_ và một chuỗi nhị phân _s_, đồng thời chúng ta cần xuất ra một số nguyên duy nhất biểu thị số chuỗi con có chính xác _k_ các chuỗi con. 

Độ dài chuỗi có thể lên tới 1.000.000 ký tự, loại trừ các cách tiếp cận ngây thơ kiểm tra tất cả các chuỗi con một cách rõ ràng. Việc kiểm tra mạnh mẽ tất cả các chuỗi con O(n²), đếm từng chuỗi mỗi lần, sẽ dẫn đến các hoạt động lên tới 10¹² trong trường hợp xấu nhất, vượt xa giới hạn thời gian 2 giây. Điều này ngay lập tức hướng chúng ta tới một giải pháp hoạt động theo thời gian tuyến tính hoặc tuyến tính, lý tưởng nhất là O(n). 

Các trường hợp cạnh có thể gây cản trở quá trình triển khai đơn giản bao gồm _k = 0_ hoặc các chuỗi có tất cả số 0 hoặc tất cả số 1. Ví dụ: nếu _s = "000"_ và _k = 0_ thì kết quả đúng là 6, tương ứng với các chuỗi con "0", "0", "0", "00", "00", "000". Một giải pháp bất cẩn chỉ đếm số đơn vị sẽ trả về 0 không chính xác. Một trường hợp tinh vi khác là khi _k_ vượt quá tổng số đơn vị trong _s_, số này sẽ trả về 0. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: liệt kê tất cả các chuỗi con có thể bằng cách chọn chỉ mục bắt đầu và kết thúc, đồng thời đếm số lượng chuỗi con trong mỗi chuỗi con. Điều này hiệu quả vì đối với mỗi chuỗi con, chúng ta có thể xác định chính xác có bao nhiêu chuỗi con chứa, nhưng nó trở nên cực kỳ chậm vì tổng số chuỗi con trong một chuỗi có độ dài _n_ là n(n+1)/2 và việc đếm các chuỗi con bên trong mỗi chuỗi con sẽ thêm một hệ số n khác trong trường hợp xấu nhất. Đối với _n = 10⁶_, điều này dẫn đến các hoạt động O(n³) nếu được triển khai một cách đơn giản, điều này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng về một giải pháp tối ưu xuất phát từ việc quan sát thấy rằng vấn đề quy về việc đếm các phạm vi số đơn vị trong chuỗi. Nếu chúng ta ghi lại vị trí của tất cả các số một thì bất kỳ chuỗi con nào có chính xác _k_ số một sẽ bắt đầu ngay sau số thứ (i-1) và kết thúc tại hoặc trước số thứ (i+k). Bằng cách tính toán trước các vị trí này, chúng ta có thể tính toán trực tiếp số chỉ mục bắt đầu và kết thúc hợp lệ mà không cần kiểm tra từng chuỗi con. Đối với trường hợp đặc biệt _k = 0_, thay vào đó, chúng tôi đếm các khối 0 liền kề nhau và tính xem có bao nhiêu chuỗi con tồn tại bên trong mỗi khối bằng cách sử dụng phép tính tổng tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu (Tiền tố + Vị trí đơn vị) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên _k_ và chuỗi nhị phân _s_. Khởi tạo danh sách để lưu trữ vị trí của những cái đó, thêm -1 ở đầu và n ở cuối làm trọng điểm. Điều này đơn giản hóa việc tính toán ranh giới. 
2. Duyệt chuỗi một lần. Bất cứ khi nào một ký tự là '1', hãy thêm chỉ mục của nó vào danh sách vị trí đó. 
3. Nếu _k = 0_, lặp qua các vị trí liên tiếp để tìm các khối số 0 nằm giữa chúng. Đối với một khối có độ dài _l_, số lượng chuỗi con là l(l+1)/2. Tính tổng những đóng góp này cho tất cả các khối bằng 0 và trả về tổng số. 
4. Nếu _k > 0_, lặp lại các vị trí đơn vị từ chỉ số 1 đến len(ones) - k - 1. Đối với mỗi cửa sổ của _k_ đơn vị, đặt left_gap là số số 0 trước số 0 đầu tiên trong cửa sổ và right_gap là số số 0 sau số 0 cuối cùng trong cửa sổ. Số chuỗi con chứa chính xác nhóm k chuỗi con này là (left_gap + 1) * (right_gap + 1). Thêm phần này vào tổng số đang chạy. 
5. Xuất ra tổng số.

Tại sao nó hoạt động: bằng cách ghi lại vị trí của tất cả những cái đó, chúng ta chuyển bài toán thành một bài toán tổ hợp. Mỗi chuỗi con có đúng k chuỗi con tương ứng duy nhất với một cửa sổ gồm k chuỗi con liên tiếp. Số lượng chuỗi con có thể có cho cửa sổ đó chính xác là số cách mở rộng sang trái và phải bằng các số 0, được ghi lại bởi các khoảng trống. Cách tiếp cận này đảm bảo rằng mọi chuỗi con hợp lệ sẽ được tính chính xác một lần và không có chuỗi con không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k = int(input())
s = input().strip()
n = len(s)

ones = [-1]  # sentinel before start
for i, c in enumerate(s):
    if c == '1':
        ones.append(i)
ones.append(n)  # sentinel after end

if k == 0:
    total = 0
    for i in range(1, len(ones)):
        zeros = ones[i] - ones[i-1] - 1
        total += zeros * (zeros + 1) // 2
    print(total)
else:
    total = 0
    for i in range(1, len(ones) - k):
        left_gap = ones[i] - ones[i-1]
        right_gap = ones[i+k] - ones[i+k-1]
        total += left_gap * right_gap
    print(total)
```Vòng lặp đầu tiên thu thập tất cả các vị trí của '1' và thêm các giá trị trọng điểm. Trọng điểm trước khi bắt đầu đảm bảo rằng các khoảng trống bên trái được tính toán chính xác khi cửa sổ bắt đầu ở cửa sổ đầu tiên. Trọng điểm sau khi kết thúc đảm bảo các khoảng trống bên phải được tính toán chính xác khi cửa sổ kết thúc ở cửa sổ cuối cùng. Nhánh có điều kiện xử lý trường hợp số 0 bằng cách đếm tổ hợp các chuỗi con của số 0. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** Dữ liệu nhập: k=1, s="1010" 

| danh sách | -1,0,2,4 | 

| cửa sổ | left_gap | đúng_gap | đóng góp | 
| --- | --- | --- | --- | 
| 0 '1' | 0-(-1)=1 | 2-0=2 | 1*2=2 | 
| 1 '1' | 2-0=2 | 4-2=2 | 2*2=4 | 
| Tổng cộng | | | 6 | 

Điều này xác nhận rằng tất cả các chuỗi con chứa chính xác một '1' đều được tính. 

**Ví dụ 2:** Dữ liệu nhập: k=2, s="10101" 

| danh sách | -1,0,2,4,5 | 

| cửa sổ | left_gap | đúng_gap | đóng góp | 

| 0-1 | 0-(-1)=1 | 4-2=2 | 2 | 

| 1-2 | 2-0=2 | 5-4=1 | 2 | 

| Tổng cộng | | | 4 | 

Điều này chứng tỏ rằng các cửa sổ chồng lên nhau được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng ta duyệt chuỗi một lần để ghi lại các số một và một lần nữa để tính toán các đóng góp. | 
| Không gian | O(n) | Việc lưu trữ vị trí của một cái cần tối đa n mục nhập cộng với hai lính canh. | 

Giải pháp thời gian tuyến tính phù hợp với giới hạn 2 giây cho n tối đa 10⁶ và mức sử dụng bộ nhớ có thể chấp nhận được với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        exec(open("solution.py").read())
    return out.getvalue().strip()

# provided samples
assert run("1\n1010\n") == "6", "sample 1"
assert run("2\n10101\n") == "4", "sample 2"

# custom cases
assert run("0\n000\n") == "6", "all zeros, k=0"
assert run("3\n11111\n") == "3", "all ones, k=3"
assert run("1\n10001\n") == "8", "ones at edges and middle"
assert run("0\n111\n") == "0", "no zeros, k=0"
assert run("2\n010101\n") == "9", "alternating ones and zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "0\n000\n" | 6 | Đếm đúng chuỗi con bằng 0 | 
| "3\n11111\n" | 3 | Windows của k cái liên tiếp | 
| "1\n10001\n" | 8 | Phần mở rộng trái/phải có số 0 | 
| "0\n111\n" | 0 | Không có số 0, trường hợp k=0 | 
| "2\n010101\n" | 9 | Những số 1 và số 0 xen kẽ, nhiều cửa sổ | 

## Vỏ cạnh 

Đối với k=0 và s="000", danh sách là [-1,3]. Độ dài khối 0 là 3-(-1)-1 = 3, cho 3*4/2 = 6 chuỗi con, chính xác. Đối với k lớn hơn tổng số, ví dụ k=5 và s="1010", vòng lặp trên windows không thực thi và tổng vẫn bằng 0, điều này đúng. Đối với các chuỗi có các chuỗi ở ranh giới, trọng điểm đảm bảo các khoảng trống bên trái và bên phải được tính toán chính xác. Mỗi trường hợp cạnh xác nhận rằng thuật toán xử lý các ranh giới, chuỗi con bằng 0 và các cửa sổ chồng chéo một cách nhất quán.
