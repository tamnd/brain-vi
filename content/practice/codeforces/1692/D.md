---
title: "CF 1692D - Chiếc Đồng Hồ"
description: "Chúng ta được cung cấp thời gian bắt đầu trên đồng hồ 24 giờ và số phút cố định giữa các lần quan sát. Bắt đầu từ thời điểm nhất định, Victor liên tục nhìn đồng hồ sau mỗi $x$ phút."
date: "2026-06-09T23:03:11+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1692
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 799 (Div. 4)"
rating: 1100
weight: 1692
solve_time_s: 155
verified: true
draft: false
---

[CF 1692D - Chiếc đồng hồ](https://codeforces.com/problemset/problem/1692/D) 

**Đánh giá:** 1100 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp thời gian bắt đầu trên đồng hồ 24 giờ và số phút cố định giữa các lần quan sát. Bắt đầu từ thời điểm đã định, Victor liên tục nhìn đồng hồ sau mỗi lần tập.$x$phút. 

Bởi vì đồng hồ 24 giờ sẽ kết thúc sau một ngày nên chuỗi thời gian được hiển thị cuối cùng sẽ trở thành tuần hoàn. Chúng ta phải đếm xem có bao nhiêu **thời gian đối xứng khác nhau** xuất hiện trong chuỗi lặp lại này. 

Một thời gian được viết là`"HH:MM"`. Để kiểm tra xem nó có phải là một bảng màu hay không, chúng ta xử lý toàn bộ chuỗi bao gồm cả dấu hai chấm. Ví dụ,`"12:21"`Và`"05:50"`là các bảng màu vì đọc ngược chuỗi năm ký tự sẽ cho ra cùng một chuỗi. 

Những hạn chế là rất nhỏ. Một chiếc đồng hồ chỉ chứa$24 \times 60 = 1440$những khoảnh khắc riêng biệt. Bất kể giá trị của$x$, sau tối đa 1440 lần chuyển đổi, chúng ta phải quay lại thời điểm đã thấy trước đó. Với tối đa 100 trường hợp thử nghiệm, thậm chí việc xử lý tất cả 1440 lần cho mỗi trường hợp thử nghiệm chỉ cần khoảng 144.000 lần lặp, điều này là không đáng kể. 

Thách thức chính không phải là hiệu suất mà là xử lý chính xác cấu trúc chu trình. 

Một sai lầm dễ mắc phải là tính số lần xuất hiện đối xứng thay vì thời gian đối xứng rõ rệt. Coi như:```
11:11 1440
```Mọi quan sát đều cho thấy`11:11`, nhưng câu trả lời là`1`, không phải là số lượng vô hạn và không phải số lượng quan sát. 

Một lỗi phổ biến khác là mô phỏng chính xác trong 1440 phút thay vì cho đến khi trình tự lặp lại. Ví dụ:```
00:00 720
```Trình tự là:```
00:00 -> 12:00 -> 00:00
```Chỉ có hai thời điểm riêng biệt từng đạt được. Một lời giải đúng sẽ dừng lại khi trạng thái ban đầu xuất hiện trở lại. 

Vấn đề tế nhị thứ ba là xử lý sự cố vào lúc nửa đêm. Ví dụ:```
23:59 1
```Thêm một phút sẽ tạo ra`00:00`, không`24:00`. Làm việc trong tổng số phút modulo 1440 sẽ tránh được hoàn toàn những lỗi như vậy. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là liên tục tăng tốc đồng hồ lên$x$phút và ghi lại mỗi lần gặp phải. Bất cứ khi nào thời gian được hiển thị là một bảng màu, chúng tôi sẽ thêm nó vào một bộ. Quá trình dừng lại khi chúng ta quay lại thời điểm đã truy cập trước đó. 

Mô phỏng lực lượng vũ phu này đã đủ nhanh. Không gian trạng thái chỉ chứa 1440 giá trị đồng hồ có thể có, do đó mô phỏng thực hiện tối đa 1440 lần lặp cho mỗi trường hợp thử nghiệm. 

Một lực lượng vũ phu ít cẩn thận hơn có thể tiếp tục vô tận vì trình tự có tính tuần hoàn. Quan sát quan trọng là trạng thái đồng hồ hoàn toàn được xác định theo phút hiện tại trong ngày. Vì chỉ tồn tại 1440 trạng thái nên việc xem lại một trạng thái có nghĩa là toàn bộ chuỗi tương lai sẽ lặp lại chính xác. 

Khi chúng tôi nhận thấy quy trình diễn ra theo chu kỳ tối đa 1440 trạng thái, giải pháp sẽ trở nên đơn giản. Chúng tôi mô phỏng trình tự, lưu trữ số lần truy cập và đếm các chuỗi đối xứng riêng biệt gặp phải trong một chu kỳ đầy đủ. 

Thuật toán kết quả vừa đơn giản vừa tối ưu cho các ràng buộc đã cho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu theo chu kỳ | O(1440) | O(1440) | Đã chấp nhận | 
| Mô phỏng chu trình tối ưu | O(1440) | O(1440) | Đã chấp nhận | 

Trong bài toán này, các phương pháp tiếp cận tối ưu và bạo lực thực sự giống nhau vì toàn bộ không gian trạng thái chỉ chứa 1440 lần có thể. 

## Hướng dẫn thuật toán 

1. Chuyển đổi thời gian bắt đầu`"HH:MM"`vào số phút kể từ nửa đêm. 
2. Tạo một tập hợp các trạng thái đồng hồ đã truy cập. Điều này sẽ cho phép chúng tôi phát hiện khi nào trình tự bắt đầu lặp lại. 
3. Tạo một bộ khác cho thời gian palindromic. Việc sử dụng một bộ đảm bảo rằng mỗi palindrome chỉ được tính một lần ngay cả khi nó xuất hiện nhiều lần trong chu kỳ. 
4. Trong khi trạng thái đồng hồ hiện tại chưa được truy cập: 

Thêm trạng thái vào tập đã truy cập. 
5. Chuyển đổi số phút hiện tại thành`"HH:MM"`định dạng. 
6. Kiểm tra xem chuỗi kết quả có phải là chuỗi palindrome hay không. 

Nếu có, hãy chèn nó vào bộ palindrome. 
7. Tăng đồng hồ lên$x$phút modulo 1440. 

Hoạt động modulo tự động xử lý xung quanh suốt nửa đêm. 
8. Khi đạt đến trạng thái đã truy cập trước đó, hãy dừng mô phỏng. 
9. Xuất ra kích thước của bộ palindrome. 

### Tại sao nó hoạt động 

Mỗi trạng thái đồng hồ tương ứng với một trong 1440 phút có thể có trong một ngày. Bắt đầu từ trạng thái ban đầu và liên tục thêm$x$phút tạo ra một chuỗi xác định. Khi một trạng thái lặp lại, trình tự sẽ quay lại cùng một chu kỳ và không có thời gian mới nào có thể xuất hiện. 

Thuật toán truy cập mọi trạng thái riêng biệt trong chu trình này đúng một lần. Mỗi thời gian palindrome xuất hiện trong chu trình sẽ được chèn vào tập hợp palindrome và tập hợp này sẽ tự động loại bỏ các bản sao. Vì chu trình chứa tất cả các lần Victor có thể nhìn thấy nên kích thước đặt cuối cùng chính xác là số lượng các palindrome khác nhau mà anh ấy có thể nhìn thấy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_palindrome_time(minutes):
    h = minutes // 60
    m = minutes % 60
    s = f"{h:02d}:{m:02d}"
    return s == s[::-1], s

t = int(input())

for _ in range(t):
    time_str, x = input().split()
    x = int(x)

    h = int(time_str[:2])
    m = int(time_str[3:])
    cur = h * 60 + m

    visited = set()
    palindromes = set()

    while cur not in visited:
        visited.add(cur)

        ok, s = is_palindrome_time(cur)
        if ok:
            palindromes.add(s)

        cur = (cur + x) % 1440

    print(len(palindromes))
```Bước đầu tiên chuyển đổi thời gian bắt đầu thành một số nguyên duy nhất biểu thị số phút kể từ nửa đêm. Số học trở nên dễ dàng hơn nhiều trong cách biểu diễn này vì việc tăng tốc đồng hồ chỉ là một phép cộng theo sau là modulo 1440. 

các`visited`set lưu trữ các giá trị phút thay vì các chuỗi được định dạng. Điều này xác định duy nhất từng trạng thái đồng hồ và cho phép phát hiện chu kỳ trong thời gian không đổi. 

Kiểm tra palindrome được thực hiện trên định dạng`"HH:MM"`sợi dây. Vì định dạng được yêu cầu luôn chứa các số 0 đứng đầu nên định dạng bằng`:02d`là điều cần thiết. Không có số 0 đứng đầu, những thời điểm như`"05:50"`sẽ được trình bày không chính xác và việc phát hiện palindrome sẽ thất bại. 

Vòng lặp dừng ngay khi một trạng thái được lặp lại. Tại thời điểm đó, toàn bộ chuỗi tương lai được biết là sẽ lặp lại, vì vậy việc tiếp tục sẽ chỉ xem lại thời gian đã xử lý trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
03:12 360
```Số phút bắt đầu:```
3 * 60 + 12 = 192
```| Bước | Thời gian hiện tại | Xuôi ngược đều giống nhau? | Palindromes riêng biệt | 
| --- | --- | --- | --- | 
| 1 | 03:12 | Không | 0 | 
| 2 | 09:12 | Không | 0 | 
| 3 | 15:12 | Không | 0 | 
| 4 | 21:12 | Có | 1 | 
| 5 | 03:12 | Lặp lại trạng thái | Dừng lại | 

Thời gian palindromic duy nhất đạt được là`21:12`, vậy câu trả lời là`1`. 

Dấu vết này cho thấy tại sao chúng ta phải dừng việc phát hiện chu kỳ thay vì sau một số lần quan sát tùy ý. 

### Ví dụ 2 

đầu vào:```
11:11 1440
```| Bước | Thời gian hiện tại | Xuôi ngược đều giống nhau? | Palindromes riêng biệt | 
| --- | --- | --- | --- | 
| 1 | 11:11 | Có | 1 | 
| 2 | 11:11 | Lặp lại trạng thái | Dừng lại | 

Đồng hồ ngay lập tức trở về trạng thái cũ vì cộng thêm 1440 phút bằng một ngày trọn vẹn. 

Palindrome xuất hiện vô số lần trong chuỗi quan sát, nhưng chỉ tồn tại một thời gian palindrome riêng biệt, vì vậy câu trả lời là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1440) | Tối đa 1440 trạng thái đồng hồ riêng biệt được truy cập | 
| Không gian | O(1440) | Tập đã truy cập có thể chứa mọi phút trong ngày | 

Vì 1440 là hằng số cố định nên thời gian chạy thực tế là cực kỳ nhỏ. Ngay cả với 100 trường hợp thử nghiệm, tổng khối lượng công việc vẫn thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    input_stream = io.StringIO(inp)

    def input():
        return input_stream.readline()

    t = int(input())
    out = []

    for _ in range(t):
        time_str, x = input().split()
        x = int(x)

        h = int(time_str[:2])
        m = int(time_str[3:])
        cur = h * 60 + m

        visited = set()
        pals = set()

        while cur not in visited:
            visited.add(cur)

            hh = cur // 60
            mm = cur % 60
            s = f"{hh:02d}:{mm:02d}"

            if s == s[::-1]:
                pals.add(s)

            cur = (cur + x) % 1440

        out.append(str(len(pals)))

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""6
03:12 360
00:00 1
13:22 2
15:15 10
11:11 1440
22:30 27
"""
) == (
"""1
16
10
0
1
1
"""
)

# minimum case
assert run(
"""1
00:00 1440
"""
) == (
"""1
"
```
