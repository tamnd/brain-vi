---
title: "CF 144C - Tìm kiếm đảo chữ"
description: "Chúng ta được yêu cầu đếm số chuỗi con của chuỗi s có thể chuyển thành đảo chữ của chuỗi p cho trước. Chuỗi s có thể chứa dấu chấm hỏi ?, có thể được thay thế bằng bất kỳ chữ cái viết thường nào."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 144
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 103 (Div. 2)"
rating: 1500
weight: 144
solve_time_s: 83
verified: true
draft: false
---

[CF 144C - Tìm kiếm đảo chữ](https://codeforces.com/problemset/problem/144/C) 

**Đánh giá:** 1500 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số chuỗi con của một chuỗi`s`có thể được chuyển đổi thành đảo chữ của một chuỗi nhất định`p`. Chuỗi`s`có thể chứa dấu hỏi`?`, có thể được thay thế bằng bất kỳ chữ cái viết thường nào. Một chuỗi con được định nghĩa là một đoạn liền kề của`s`và hai chuỗi con bắt đầu ở các vị trí khác nhau được coi là khác biệt ngay cả khi chúng giống hệt nhau. 

Kích thước đầu vào cho phép`s`Và`p`mỗi cái có độ dài tối đa 100.000 ký tự. Một cách tiếp cận đơn giản tạo ra tất cả các chuỗi con và kiểm tra chúng riêng lẻ sẽ yêu cầu khoảng`O(n^2 * m)`hoạt động ở đâu`n`là chiều dài của`s`Và`m`là chiều dài của`p`. Với`n`Và`m`lên tới 10^5, điều này dẫn đến khoảng 10^15 thao tác, quá chậm so với giới hạn thời gian 2 giây. Chúng tôi cần một`O(n)`hoặc`O(n * 26)`cách tiếp cận để duy trì hiệu quả. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó độ dài chuỗi con của`s`ít hơn`p`, sẽ ngay lập tức tạo ra chuỗi con hợp lệ bằng 0. Một trường hợp tinh tế khác phát sinh khi một chuỗi con chỉ chứa các dấu chấm hỏi - nó có thể khớp với bất kỳ đảo chữ nào của`p`miễn là độ dài phù hợp. Ví dụ, nếu`s = "???"`Và`p = "abc"`, chuỗi con hợp lệ vì ba dấu chấm hỏi có thể được thay thế bằng 'a', 'b', 'c' theo thứ tự nào đó. Một giải pháp bất cẩn có thể tính sai hoặc bỏ sót những trường hợp như vậy. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực lặp đi lặp lại trên tất cả các chuỗi con của`s`có độ dài bằng`p`và kiểm tra xem chúng có thể tạo thành một phép đảo chữ bằng cách đếm các chữ cái và dấu chấm hỏi hay không. Đối với mỗi chuỗi con, người ta sẽ tính toán một dãy tần số gồm các chữ cái và xem liệu sự khác biệt giữa`p`Số lượng chữ cái của và số lượng chuỗi con có thể được bao phủ chính xác bằng dấu chấm hỏi. Điều này hoạt động hợp lý nhưng chạy trong`O(n*m)`thời gian. Đối với đầu vào lớn nhất, đó là khoảng 10^10 thao tác - quá chậm. 

Quan sát quan trọng là việc kiểm tra các chuỗi con có độ dài cố định sẽ tự đưa đến một cửa sổ trượt. Bằng cách duy trì số lượng tần số cho một cửa sổ có độ dài`m`(chiều dài của`p`), chúng ta có thể cập nhật số đếm một cách hiệu quả khi cửa sổ di chuyển sang phải một vị trí. Chúng tôi cũng duy trì số lượng dấu hỏi trong cửa sổ. Đối với mỗi ca, chúng ta chỉ cần kiểm tra xem có thiếu các chữ cái trong cửa sổ so với`p`chính xác bằng số dấu chấm hỏi. Điều này làm giảm việc kiểm tra trên mỗi cửa sổ xuống`O(26)`các thao tác, tạo nên thuật toán đầy đủ`O(n*26)`, đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(26) | Quá chậm | 
| Cửa Sổ Trượt | O(n*26) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mảng tần số`freq_p`cho chuỗi`p`, Ở đâu`freq_p[i]`đếm số lần xuất hiện của chữ cái`'a'+i`. 
2. Khởi tạo cửa sổ trượt có độ dài`m`qua`s`. Tính toán`freq_window`như chữ cái đếm của cửa sổ đầu tiên và đếm số dấu chấm hỏi`q_count`. 
3. Với mỗi cửa sổ, tính tổng số chữ cái còn thiếu so với`p`bằng cách tính tổng`max(0, freq_p[i] - freq_window[i])`trên tất cả các chữ cái. Nếu tổng này bằng`q_count`, cửa sổ tốt. 
4. Trượt cửa sổ sang bên phải một vị trí. Cập nhật`freq_window`Và`q_count`bằng cách giảm số lượng ký tự rời khỏi cửa sổ và tăng số lượng ký tự đi vào cửa sổ. 
5. Lặp lại bước 3 và 4 cho đến khi hết chuỗi. 

Lý do điều này có tác dụng là vì mỗi chữ cái thiếu hụt phải được bao phủ chính xác bằng các dấu hỏi để chuỗi con có thể chuyển đổi thành đảo chữ của`p`. Việc trượt cửa sổ đảm bảo chúng tôi kiểm tra mọi chuỗi con có thể một cách hiệu quả mà không cần tính toán lại số đếm từ đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
p = input().strip()

len_s = len(s)
len_p = len(p)

if len_p > len_s:
    print(0)
    sys.exit()

# frequency of letters in p
freq_p = [0] * 26
for ch in p:
    freq_p[ord(ch) - ord('a')] += 1

# frequency of letters in the current window
freq_window = [0] * 26
q_count = 0
for i in range(len_p):
    if s[i] == '?':
        q_count += 1
    else:
        freq_window[ord(s[i]) - ord('a')] += 1

result = 0

def is_good():
    missing = 0
    for i in range(26):
        if freq_p[i] > freq_window[i]:
            missing += freq_p[i] - freq_window[i]
    return missing == q_count

if is_good():
    result += 1

for i in range(len_p, len_s):
    # remove leftmost character
    left = s[i - len_p]
    if left == '?':
        q_count -= 1
    else:
        freq_window[ord(left) - ord('a')] -= 1
    # add new character
    right = s[i]
    if right == '?':
        q_count += 1
    else:
        freq_window[ord(right) - ord('a')] += 1
    if is_good():
        result += 1

print(result)
```Giải pháp khởi tạo số lượng chữ cái cho cả hai`p`và cửa sổ đầu tiên của`s`. Mỗi lần chuyển cửa sổ chỉ cập nhật các ký tự vào và ra, giữ cho việc kiểm tra trên mỗi cửa sổ theo thời gian không đổi (26 chữ cái). sử dụng`max(0, freq_p[i]-freq_window[i])`tránh những thiếu sót tiêu cực, xử lý chính xác các chữ cái xuất hiện nhiều trong cửa sổ hơn mức cần thiết. các`?`số lượng được điều chỉnh tăng dần, ngăn chặn việc tính toán lại. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
s = "bb??x???"
p = "aab"
```| Cửa sổ | tần số_window | q_count | chữ cái còn thiếu | Tốt? | 
| --- | --- | --- | --- | --- | 
| "bb?" | b=2 | 1 | thiếu a=2 → thiếu=2 | 2 == 1? Không | 
| "b??" | b=1 | 2 | thiếu a=2 → thiếu=2 | 2 == 2? Có | 
| "???" | 0 | 3 | thiếu a=2, b=1 → thiếu=3 | 3 == 3? Có | 
| "?x?" | x=1 | 2 | thiếu a=2, b=1 → thiếu=3 | 3 != 2? Không | 
| "x??" | x=1 | 2 | thiếu a=2, b=1 → thiếu=3 | 3 != 2? Không | 
| "???" | 0 | 3 | thiếu a=2, b=1 → thiếu=3 | 3 == 3? Có | 

Hai chuỗi con được tính. 

Đầu vào mẫu 2:```
s = "ab?c"
p = "abc"
```| Cửa sổ | tần số_window | q_count | mất tích | Tốt? | 
| --- | --- | --- | --- | --- | 
| "ab?" | a=1, b=1 | 1 | c=1 → thiếu=1 | 1==1 Có | 
| "b?c" | b=1, c=1 | 1 | a=1 → thiếu=1 | 1==1 Có | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*26) | Mỗi cửa sổ trong số n cửa sổ thực hiện kiểm tra thiếu 26 chữ cái. Cập nhật cửa sổ trượt là O(1). | 
| Không gian | O(26*2) = O(1) | Hai dãy tần số có kích thước 26 và một vài bộ đếm. | 

Giải pháp này rất phù hợp trong giới hạn 2 giây cho n lên tới 100.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    p = input().strip()
    len_s = len(s)
    len_p = len(p)
    if len_p > len_s:
        return "0"
    freq_p = [0]*26
    for ch in p:
        freq_p[ord(ch)-ord('a')] += 1
    freq_window = [0]*26
    q_count = 0
    for i in range(len_p):
        if s[i]=='?':
            q_count += 1
        else:
            freq_window[ord(s[i])-ord('a')] +=1
    result=0
    def is_good():
        missing=0
        for i in range(26):
            if freq_p[i]>freq_window[i]:
                missing += freq_p[i]-freq_window[i]
        return missing==q_count
    if is_good():
        result +=1
    for i in range(len_p,len_s):
        left = s[i-len_p]
        if left=='?':
            q_count -=1
        else:
            freq_window[ord(left)-ord('a')]-=1
        right = s[i]
        if
```
