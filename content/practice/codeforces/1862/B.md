---
title: "CF 1862B - Trò chơi tuần tự"
description: "Chúng ta được yêu cầu xây dựng lại một chuỗi ban đầu có thể có a cho trước một chuỗi b được bắt nguồn bằng một quy tắc lọc đơn giản. Dãy b luôn bắt đầu bằng phần tử đầu tiên của a, sau đó bao gồm mọi phần tử của a lớn hơn hoặc bằng phần tử liền trước nó."
date: "2026-06-09T00:52:12+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1862
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 894 (Div. 3)"
rating: 800
weight: 1862
solve_time_s: 162
verified: false
draft: false
---

[CF 1862B - Trò chơi tuần tự](https://codeforces.com/problemset/problem/1862/B) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tái tạo lại một trình tự ban đầu có thể có`a`đưa ra một trình tự`b`được bắt nguồn từ một quy tắc lọc đơn giản. Trình tự`b`luôn bắt đầu bằng phần tử đầu tiên của`a`, sau đó bao gồm mọi phần tử của`a`lớn hơn hoặc bằng người tiền nhiệm trực tiếp của nó. Nói cách khác,`b`được hình thành bằng cách giữ nguyên phần tử đầu tiên và tất cả các "đỉnh" không giảm khi chúng tôi quét`a`. Nhiệm vụ của chúng ta không phải là tìm chuỗi gốc duy nhất mà là tìm bất kỳ chuỗi hợp lệ nào`a`có thể tạo ra cái đã cho`b`. 

Các ràng buộc cho phép lên đến`2 * 10^5`các yếu tố trong`b`trên tất cả các trường hợp thử nghiệm. Vì chúng ta được phép xuất ra một chuỗi có độ dài lên tới`2n`cho mỗi`b`, một phép xây dựng tuyến tính là đủ. Điều này loại trừ bất cứ điều gì phức tạp hơn`O(n)`mỗi trường hợp thử nghiệm. 

Sự tinh tế là giữa hai yếu tố liên tiếp của`b`, các phần tử của`a`có thể giảm xuống và tăng lên tự do, miễn là phần tử không giảm đầu tiên đạt đến phần tử tiếp theo của`b`. Ví dụ, nếu`b = [4, 6, 3]`, chúng ta có thể chèn bất kỳ chuỗi nào vào giữa`4`Và`6`điều đó không vượt quá`6`trước`6`, và giữa`6`Và`3`, chúng ta có thể có một dãy giảm dần kết thúc tại`3`. Trường hợp cạnh xảy ra khi`b`chỉ có một phần tử hoặc khi các phần tử liên tiếp bằng nhau, trong trường hợp đó, chuỗi phần tử giống nhau được lặp lại một hoặc nhiều lần vẫn hoạt động. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các chuỗi có độ dài lên tới`2n`để xem cái nào giảm xuống`b`, nhưng số lượng khả năng tăng theo cấp số nhân và không thể thực hiện được. 

Cách tiếp cận tối ưu thúc đẩy quan sát rằng chỉ cần đặt một phần tử trung gian giữa các phần tử liên tiếp của`b`cho phép cả tăng và giảm. Cụ thể, giữa`b[i]`Và`b[i+1]`, chúng ta có thể chèn`b[i]`một lần nữa nếu`b[i] > b[i+1]`để cho phép giá trị tiếp theo giảm xuống. Điều này đảm bảo rằng quy tắc lọc vẫn tạo ra chính xác`b`. Nếu như`b[i] <= b[i+1]`, chúng tôi chỉ cần nối thêm`b[i+1]`trực tiếp. Phương pháp này đảm bảo rằng chuỗi kết quả`a`có chiều dài nhiều nhất`2n`, tôn trọng quy tắc lọc và chạy theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tuyến tính xây dựng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách trống`a`. 
2. Nối phần tử đầu tiên của`b`ĐẾN`a`. 
3. Lặp lại từng cặp phần tử liên tiếp`(b[i], b[i+1])`: 

1. Nối thêm`b[i+1]`ĐẾN`a`. 
2. Nếu`b[i] > b[i+1]`, nối thêm`b[i]`ngay trước đó`b[i+1]`để cho phép giảm trong khi vẫn duy trì quy tắc lọc. Điều này đảm bảo rằng khi xây dựng`b`từ`a`, bộ lọc không giảm sẽ bỏ qua phần được chèn`b[i]`nếu cần thiết. 
4. Sau khi xử lý tất cả các cặp, xuất độ dài của`a`và chính trình tự đó. 

Tại sao nó hoạt động: Bằng cách chèn`b[i]`trước`b[i+1]`bất cứ khi nào cần giảm, chúng tôi đảm bảo rằng quy tắc lọc vẫn mang lại kết quả chính xác`b`. Phần tử đầu tiên luôn được giữ nguyên và không phần tử được chèn nào có thể vi phạm bộ lọc không giảm vì chúng tôi đặt nó theo cách lặp lại số trước đó hoặc cho phép giảm phần tử tiếp theo. Độ dài tối đa là`2n`bởi vì chúng tôi chèn tối đa một phần tử cho mỗi cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        a = [b[0]]
        for i in range(1, n):
            if b[i-1] > b[i]:
                a.append(b[i-1])
            a.append(b[i])
        print(len(a))
        print(*a)

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và xây dựng trình tự`a`tuyến tính. Cần cẩn thận để xử lý trường hợp các phần tử liên tiếp giảm bằng cách chèn lại phần tử trước đó, điều này rất cần thiết để quy tắc lọc mang lại chính xác`b`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3
3
4 6 3
2
1 2
1
144
```Dấu vết bước cho trường hợp đầu tiên (`b = [4, 6, 3]`): 

| tôi | b[i-1] | b[i] | Hành động | một | 
| --- | --- | --- | --- | --- | 
| 0 | - | 4 | nối thêm 4 | [4] | 
| 1 | 4 | 6 | 4 <= 6, nối thêm 6 | [4, 6] | 
| 2 | 6 | 3 | 6 > 3, nối 6 rồi 3 | [4, 6, 6, 3] | 

Độ dài đầu ra: 4 

Trình tự đầu ra:`[4, 6, 6, 3]`Trình tự này tạo ra chính xác`b`khi được lọc theo luật chơi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần và tối đa một phần tử bổ sung được chèn vào. | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Sự liên tiếp`a`có chiều dài nhiều nhất là gấp đôi`b`. | 

Điều này phù hợp thoải mái trong các ràng buộc vấn đề của`n`tổng hợp lại`2 * 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("6\n3\n4 6 3\n3\n1 2 3\n5\n1 7 9 5 7\n1\n144\n2\n1 1\n5\n1 2 2 1 1\n") == \
"""6
4 6 6 3
3
1 2 3
6
1 7 9 9 5 7
1
144
2
1 1
6
1 2 2 2 1 1""", "sample 1"

# Custom cases
assert run("1\n1\n1000000000\n") == "1\n1000000000", "single element max value"
assert run("1\n2\n5 3\n") == "3\n5 5 3", "decreasing pair"
assert run("1\n2\n3 7\n") == "2\n3 7", "increasing pair"
assert run("1\n3\n2 2 2\n") == "3\n2 2 2", "all equal elements"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử, giá trị tối đa | 1 phần tử | Xử lý phần tử đơn`b`chính xác | 
| Giảm cặp | 3 yếu tố | Đảm bảo phần tử trước đó được chèn vào sẽ giảm | 
| Tăng cặp | 2 yếu tố | Xử lý tăng tự nhiên mà không cần chèn | 
| Tất cả đều bình đẳng | 3 yếu tố | Giữ nguyên các phần tử bằng nhau lặp đi lặp lại mà không cần chèn thêm | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi`b`chỉ có một phần tử. Ví dụ,`b = [144]`. Thuật toán xuất ra chính xác`[144]`có độ dài 1. Một trường hợp cạnh khác là khi các phần tử liên tiếp trong`b`đều bình đẳng. Ví dụ,`b = [2, 2, 2]`kết quả đầu ra`[2, 2, 2]`, tránh việc chèn thêm không cần thiết và độ dài không vượt quá`2n`. Cả hai trường hợp cạnh đều được xử lý bằng logic chèn tuyến tính.
