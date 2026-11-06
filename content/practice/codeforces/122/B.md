---
title: "CF 122B - Chuỗi con may mắn"
description: "Bài toán yêu cầu chúng ta tìm chuỗi con \"luckiest\" của một chuỗi chữ số cho trước. May mắn nhất, chúng tôi muốn nói đến một chuỗi con chỉ bao gồm các chữ số 4 và 7, xuất hiện trong chuỗi nhiều lần nhất có thể và là chuỗi con nhỏ nhất về mặt từ điển nếu có các mối liên kết."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 122
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 91 (Div. 2 Only)"
rating: 1000
weight: 122
solve_time_s: 78
verified: true
draft: false
---

[CF 122B - Chuỗi con may mắn](https://codeforces.com/problemset/problem/122/B) 

**Đánh giá:** 1000 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta tìm chuỗi con "may mắn nhất" của chuỗi chữ số cho trước. May mắn nhất, chúng tôi muốn nói đến một chuỗi con chỉ bao gồm các chữ số 4 và 7, xuất hiện trong chuỗi nhiều lần nhất có thể và là chuỗi con nhỏ nhất về mặt từ điển nếu có các mối liên kết. Chuỗi đầu vào có thể ngắn bằng một ký tự hoặc tối đa 50 ký tự và có thể bao gồm bất kỳ chữ số nào, kể cả số 0. Đầu ra phải là một chuỗi con may mắn hoặc "-1" nếu không tồn tại. 

Với độ dài đầu vào tối đa nhỏ là 50 ký tự, mọi giải pháp chạy trong khoảng thời gian khoảng O(n²) hoặc O(n·m) sẽ hoạt động hiệu quả. Một trường hợp khó phát hiện khi chuỗi không chứa số 4 hoặc số 7 nào cả, trong trường hợp đó câu trả lời đúng là "-1". Một trường hợp đặc biệt khác là khi nhiều chuỗi con may mắn xuất hiện với số lần giống nhau, nhưng chỉ trả về giá trị nhỏ nhất về mặt từ điển. Ví dụ, trong chuỗi`"4747"`, chuỗi con may mắn`"4"`Và`"7"`mỗi cái xuất hiện hai lần, nhưng`"4"`nên được trả lại. 

Một cách tiếp cận đơn giản là thử mọi chuỗi con là khả thi do độ dài đầu vào ngắn, nhưng chúng ta có thể đơn giản hóa hơn nữa bằng cách nhận thấy rằng chỉ các chữ số đơn '4' và '7' mới có thể tối đa hóa sự xuất hiện dưới dạng chuỗi con vì mọi số may mắn có nhiều chữ số đều ít thường xuyên hơn các chữ số riêng lẻ của nó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các chuỗi con có thể có của chuỗi đầu vào, lọc những chuỗi may mắn, đếm số lần xuất hiện cho mỗi chuỗi và cuối cùng chọn chuỗi có tần suất tối đa. Việc tạo tất cả các chuỗi con của một chuỗi có độ dài n yêu cầu khoảng n·(n+1)/2 chuỗi con và việc đếm số lần xuất hiện cho mỗi chuỗi con có thể làm cho tổng số phép toán là O(n³) nếu được triển khai một cách đơn giản. Với n 50, điều này chỉ khả thi nhưng phức tạp không cần thiết. 

Quan sát quan trọng là bất kỳ chuỗi con may mắn nào có độ dài lớn hơn 1 đều không thể xuất hiện nhiều lần hơn số '4' hoặc '7' riêng lẻ. Điều này là do các chuỗi con dài hơn yêu cầu các chữ số liên tiếp phải khớp chính xác. Vì vậy, chúng ta chỉ cần đếm số lần '4' xảy ra và số lần '7' xảy ra. Kết quả là chữ số xuất hiện thường xuyên nhất, với các mối quan hệ bị phá vỡ bằng cách chọn '4' làm tùy chọn nhỏ hơn về mặt từ điển. Điều này làm giảm giải pháp từ O(n³) xuống O(n), đơn giản và mạnh mẽ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n²) | Khả thi với n<50 nhưng quá mức cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai bộ đếm, một cho '4' và một cho '7'. Chúng sẽ theo dõi số lần mỗi cái xuất hiện trong chuỗi. Điều này trực tiếp nhắm mục tiêu quan sát rằng chỉ các chữ số riêng lẻ mới có thể tối đa hóa tần số. 
2. Lặp lại từng ký tự trong chuỗi. Đối với mỗi ký tự, nếu là '4', hãy tăng bộ đếm '4'. Nếu là '7', hãy tăng bộ đếm '7'. Bỏ qua tất cả các chữ số khác vì chúng không thể tạo thành số may mắn. 
3. Sau khi đếm, kiểm tra xem chữ số nào xuất hiện thường xuyên hơn. Nếu '4' xuất hiện thường xuyên hơn '7', câu trả lời là`"4"`. Nếu '7' xuất hiện thường xuyên hơn, câu trả lời là`"7"`. Nếu cả hai xảy ra như nhau thì chọn`"4"`vì nó nhỏ hơn về mặt từ điển. 
4. Nếu cả '4' và '7' đều không xuất hiện trong chuỗi, hãy trả về "-1". 

Thuật toán này hoạt động vì chúng tôi được đảm bảo rằng không có chuỗi con may mắn gồm nhiều chữ số nào có thể có tần số cao hơn các chữ số cấu thành của nó. Điều bất biến là sau khi đếm, một trong hai số may mắn có một chữ số này sẽ luôn biểu thị tần số tối đa có thể có của bất kỳ chuỗi con may mắn nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

count4 = 0
count7 = 0

for ch in s:
    if ch == '4':
        count4 += 1
    elif ch == '7':
        count7 += 1

if count4 == 0 and count7 == 0:
    print("-1")
elif count4 >= count7:
    print("4")
else:
    print("7")
```Mã khởi tạo bộ đếm và quét chuỗi một lần. Chỉ '4' và '7' ảnh hưởng đến bộ đếm. Quyết định cuối cùng sử dụng những so sánh đơn giản. Trước tiên, chúng tôi kiểm tra trường hợp đặc biệt không có chữ số may mắn để trả về "-1". Quy tắc ràng buộc được xử lý một cách tự nhiên bằng cách so sánh`count4 >= count7`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`"047"`| Bước | Nhân vật | đếm4 | đếm7 | 
| --- | --- | --- | --- | 
| 1 | '0' | 0 | 0 | 
| 2 | '4' | 1 | 0 | 
| 3 | '7' | 1 | 1 |`count4 == count7`, vậy câu trả lời là`"4"`. 

Điều này xác nhận rằng việc bẻ dây được xử lý chính xác. 

### Mẫu 2 

đầu vào:`"123"`| Bước | Nhân vật | đếm4 | đếm7 | 
| --- | --- | --- | --- | 
| 1 | '1' | 0 | 0 | 
| 2 | '2' | 0 | 0 | 
| 3 | '3' | 0 | 0 | 

Cả hai số đều bằng 0, vì vậy câu trả lời là`"-1"`. 

Điều này xác nhận rằng các chuỗi không có chữ số may mắn sẽ bị từ chối một cách chính xác. 

### Mẫu 3 

đầu vào:`"777447"`| Bước | Nhân vật | đếm4 | đếm7 | 
| --- | --- | --- | --- | 
| 1 | '7' | 0 | 1 | 
| 2 | '7' | 0 | 2 | 
| 3 | '7' | 0 | 3 | 
| 4 | '4' | 1 | 3 | 
| 5 | '4' | 2 | 3 | 
| 6 | '7' | 2 | 4 |`count7 > count4`, vậy câu trả lời là`"7"`. 

Điều này thể hiện sự so sánh tần số và lựa chọn chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua chuỗi để đếm chữ số | 
| Không gian | O(1) | Chỉ có hai bộ đếm số nguyên được sử dụng | 

Với n 50, thao tác này chạy ngay lập tức và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = sys.stdin.readline().strip()
    count4 = 0
    count7 = 0
    for ch in s:
        if ch == '4':
            count4 += 1
        elif ch == '7':
            count7 += 1
    if count4 == 0 and count7 == 0:
        return "-1"
    elif count4 >= count7:
        return "4"
    else:
        return "7"

# Provided samples
assert run("047\n") == "4", "sample 1"
assert run("123\n") == "-1", "sample 2"
assert run("7\n") == "7", "sample 3"

# Custom cases
assert run("4747\n") == "4", "equal counts, tie-break"
assert run("4447777\n") == "7", "7 occurs more"
assert run("4444444\n") == "4", "only 4s"
assert run("0\n") == "-1", "no lucky digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"4747"`|`"4"`| Hòa để có số lượng bằng nhau | 
|`"4447777"`|`"7"`| Lựa chọn đúng khi xuất hiện nhiều hơn một chữ số | 
|`"4444444"`|`"4"`| Chỉ có một chữ số may mắn | 
|`"0"`|`"-1"`| Không có chữ số may mắn | 
|`"7"`|`"7"`| Đầu vào một chữ số | 

## Vỏ cạnh 

Khi chuỗi không chứa chữ số may mắn, chẳng hạn như`"1230"`, thuật toán đếm chính xác số 0 cho cả chữ số và kết quả đầu ra "-1". Đối với các chuỗi có số '4' và '7' bằng nhau, chẳng hạn như`"4747"`, thuật toán sẽ ưu tiên chính xác '4'. Ví dụ, đối với các chuỗi chỉ có một loại chữ số may mắn được lặp lại nhiều lần, thuật toán sẽ trả về chính xác chữ số đó`"7777"`trả lại`"7"`. Mỗi kịch bản này được xử lý rõ ràng bằng logic so sánh cuối cùng.
