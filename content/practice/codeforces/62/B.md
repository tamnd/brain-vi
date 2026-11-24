---
title: "CF 62B - Tyndex.Brome"
description: "Nhiệm vụ là tính toán một loại \"khoảng cách\" giữa địa chỉ do người dùng nhập và danh sách các địa chỉ tiềm năng, theo một hàm lỗi cụ thể. Người dùng nhập một chuỗi s có độ dài k. Khi đó có n địa chỉ tiềm năng, mỗi địa chỉ là một chuỗi có độ dài tùy ý."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation"]
categories: ["algorithms"]
codeforces_contest: 62
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 58"
rating: 1800
weight: 62
solve_time_s: 78
verified: true
draft: false
---

[CF 62B - Tyndex.Brome](https://codeforces.com/problemset/problem/62/B) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, triển khai 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là tính toán một loại "khoảng cách" giữa địa chỉ do người dùng nhập và danh sách các địa chỉ tiềm năng, theo một hàm lỗi cụ thể. Người dùng nhập một chuỗi`s`chiều dài`k`. Sau đó có`n`địa chỉ tiềm năng, mỗi địa chỉ có độ dài tùy ý. Đối với mỗi ký tự trong một địa chỉ tiềm năng, chúng tôi cố gắng khớp nó với lần xuất hiện gần nhất của cùng một ký tự trong`s`. Nếu ký tự tồn tại trong`s`, chúng tôi thêm khoảng cách tối thiểu giữa các vị trí. Nếu nó không tồn tại, chúng tôi sẽ thêm độ dài của địa chỉ tiềm năng. Đầu ra cho mỗi địa chỉ tiềm năng là một số duy nhất biểu thị tổng lỗi này. 

Giới hạn đầu vào là đáng kể. Chúng tôi có thể có tới 100.000 địa chỉ tiềm năng, mỗi địa chỉ có độ dài khác nhau nhưng tổng của tất cả các độ dài được giới hạn ở mức 200.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận ngây thơ nào nhằm kiểm tra từng ký tự với mọi ký tự trong`s`theo cách đơn giản O(n * k * len(address)) , vì trong trường hợp xấu nhất có thể đạt tới 10^10 thao tác. Chúng ta cần một thuật toán gần tuyến tính hơn trong tổng của tất cả các độ dài địa chỉ. 

Các trường hợp cạnh tinh vi bao gồm các địa chỉ chứa các ký tự hoàn toàn không có trong`s`, địa chỉ giống hệt với`s`và địa chỉ rất ngắn so với`s`. Ví dụ, nếu`s = "abc"`và một địa chỉ tiềm năng là`"d"`, hàm`F`nên quay lại`1`bởi vì`d`không xuất hiện trong`s`. Thay vào đó, việc triển khai đơn giản không xử lý chính xác các ký tự bị thiếu có thể trả về số 0. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mỗi địa chỉ tiềm năng, chúng tôi lặp lại từng ký tự và quét`s`để tìm trận đấu gần nhất. Điều này đảm bảo tính chính xác vì chúng tôi đang tính toán khoảng cách tối thiểu cho mỗi ký tự theo đúng nghĩa đen. Tuy nhiên, nếu`s`có độ dài 10^5 và chúng tôi có 10^5 địa chỉ tiềm năng, mỗi địa chỉ có độ dài tối đa 2, phương pháp này có thể yêu cầu theo thứ tự 10^10 so sánh. Điều này vượt quá giới hạn thời gian. 

Quan sát quan trọng để tối ưu hóa là chúng ta không cần quét`s`cho mỗi nhân vật mọi lúc. Thay vào đó, chúng ta có thể tính toán trước vị trí của từng ký tự trong`s`. Khi chúng tôi đã sắp xếp danh sách các chỉ mục cho từng ký tự, chúng tôi có thể sử dụng tìm kiếm nhị phân để tìm vị trí gần nhất với bất kỳ chỉ mục đã cho nào trong thời gian O(log k). Tổng chi phí khi đó tỷ lệ thuận với tổng độ dài của tất cả các địa chỉ nhân với log k, điều này khả thi đối với các ràng buộc của bài toán. 

Một cải tiến tinh tế khác là xử lý các ký tự không có trong`s`. Nếu một nhân vật không có trong`s`, đóng góp lỗi tối thiểu là độ dài của địa chỉ. Điều này rất đơn giản để kiểm tra trong bước tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k * m) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân trên mỗi ký tự | O(sum(len(địa chỉ)) * log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào: số nguyên`n`Và`k`, chuỗi do người dùng nhập`s`và danh sách các địa chỉ tiềm năng. 
2. Tiền xử lý`s`bằng cách ghi lại, đối với mỗi ký tự 'a' đến 'z', một danh sách được sắp xếp các chỉ mục nơi nó xuất hiện trong`s`. Điều này cho phép chúng tôi nhanh chóng xác định vị trí xuất hiện gần nhất của bất kỳ ký tự nào. 
3. Với mỗi địa chỉ tiềm năng, hãy khởi tạo hàm lỗi`F`về không. Lặp lại từng ký tự của nó. 
4. Đối với ký tự hiện tại, kiểm tra xem nó có tồn tại trong`s`sử dụng các vị trí được tính toán trước. Nếu không, hãy thêm độ dài của địa chỉ tiềm năng vào`F`và tiếp tục đến ký tự tiếp theo. 
5. Nếu ký tự tồn tại trong`s`, hãy sử dụng tìm kiếm nhị phân để tìm chỉ mục gần nhất trong`s`đến vị trí hiện tại trong địa chỉ tiềm năng. Tính khoảng cách tuyệt đối và thêm nó vào`F`. 
6. Sau khi xử lý tất cả các ký tự trong địa chỉ tiềm năng, hãy in hoặc lưu trữ`F`. 
7. Lặp lại các bước 3-6 cho tất cả các địa chỉ tiềm năng. 

Điều bất biến là sau khi tiền xử lý, mỗi ký tự trong một địa chỉ tiềm năng sẽ đóng góp khoảng cách tối thiểu có thể tới`F`nếu nó tồn tại trong`s`hoặc đóng góp độ dài địa chỉ đầy đủ nếu không. Tìm kiếm nhị phân đảm bảo chúng ta tìm được chỉ mục gần nhất nên thuật toán tính toán chính xác hàm như đã xác định. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

n, k = map(int, input().split())
s = input().strip()

# Precompute positions of each character in s
pos = {chr(c): [] for c in range(ord('a'), ord('z')+1)}
for i, ch in enumerate(s):
    pos[ch].append(i)

addresses = [input().strip() for _ in range(n)]

for address in addresses:
    F = 0
    for i, ch in enumerate(address):
        if not pos[ch]:
            F += len(address)
            continue
        idx_list = pos[ch]
        insert_point = bisect.bisect_left(idx_list, i)
        min_dist = float('inf')
        if insert_point < len(idx_list):
            min_dist = abs(idx_list[insert_point] - i)
        if insert_point > 0:
            min_dist = min(min_dist, abs(idx_list[insert_point-1] - i))
        F += min_dist
    print(F)
```Trong mã,`pos`nắm giữ mọi vị trí của từng nhân vật trong`s`. Đối với mỗi ký tự trong một địa chỉ tiềm năng, chúng tôi tìm kết quả trùng khớp nhất bằng cách sử dụng`bisect_left`, xem xét cả chỉ mục được tìm thấy và chỉ mục trước đó, điều này đảm bảo chúng tôi luôn lấy khoảng cách tối thiểu. Nếu thiếu một ký tự, chúng tôi sẽ thêm ngay độ dài đầy đủ. Có thể tránh các lỗi riêng lẻ bằng cách lập chỉ mục cẩn thận và xem xét cả hai mặt của điểm chèn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 10
codeforces
codeforces
codehorses
```Xử lý địa chỉ đầu tiên`codeforces`: 

| Nhân vật | Chỉ mục trong địa chỉ | Chỉ số gần nhất trong s | Khoảng cách | 
| --- | --- | --- | --- | 
| c | 0 | 0 | 0 | 
| o | 1 | 1 | 0 | 
| d | 2 | 2 | 0 | 
| e | 3 | 3 | 0 | 
| f | 4 | 4 | 0 | 
| o | 5 | 5 | 0 | 
| r | 6 | 6 | 0 | 
| c | 7 | 7 | 0 | 
| e | 8 | 8 | 0 | 
| s | 9 | 9 | 0 |`F = 0`Xử lý địa chỉ thứ hai`codehorses`: 

| Nhân vật | Chỉ mục | Chỉ số gần nhất trong s | Khoảng cách | 
| --- | --- | --- | --- | 
| c | 0 | 0 | 0 | 
| o | 1 | 1 | 0 | 
| d | 2 | 2 | 0 | 
| e | 3 | 3 | 0 | 
| h | 4 | không | 11 | 
| o | 5 | 5 | 0 | 
| r | 6 | 6 | 0 | 
| s | 7 | 9 | 2 | 
| e | 8 | 8 | 0 | 
| s | 9 | 9 | 0 |`F = 12`Điều này xác nhận tính toán chính xác về khoảng cách và xử lý các ký tự bị thiếu. 

### Mẫu 2 (Vỏ cạnh) 

đầu vào:```
1 3
abc
d
```Đầu ra:```
1
```Ở đây, nhân vật`d`không tồn tại trong`s`, Vì thế`F = len("d") = 1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sum(len(địa chỉ)) * log k) | Tiền xử lý`s`là O(k). Mỗi ký tự của mỗi địa chỉ yêu cầu tìm kiếm nhị phân trong danh sách vị trí. Tổng tất cả độ dài địa chỉ 2·10^5, log k 17, chấp nhận được. | 
| Không gian | O(k + n) | Lưu trữ vị trí của từng ký tự trong`s`yêu cầu O(k). Địa chỉ yêu cầu O(n) cho đầu vào. | 

Thuật toán thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        n, k = map(int, input().split())
        s = input().strip()
        pos = {chr(c): [] for c in range(ord('a'), ord('z')+1)}
        for i, ch in enumerate(s):
            pos[ch].append(i)
        addresses = [input().strip() for _ in range(n)]
        for address in addresses:
            F = 0
            for i, ch in enumerate(address):
                if not pos[ch]:
                    F += len(address)
                    continue
                idx_list = pos[ch]
                insert_point = bisect.bisect_left(idx_list, i)
                min_dist = float('inf')
                if insert_point < len(idx_list):
                    min_dist = abs(idx_list[insert_point] - i)
                if insert_point > 0
```
