---
title: "CF 127B - Khung Tranh Canvas"
description: "Chúng ta được cho một tập hợp các que, trong đó mỗi que có chiều dài nguyên. Một khung hình chữ nhật cần bốn que xếp thành hai cặp bằng nhau. Nếu khung là hình vuông thì cả bốn thanh phải có cùng chiều dài."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 127
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 93 (Div. 2 Only)"
rating: 1000
weight: 127
solve_time_s: 106
verified: true
draft: false
---

[CF 127B - Khung tranh canvas](https://codeforces.com/problemset/problem/127/B) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các que, trong đó mỗi que có chiều dài nguyên. Một khung hình chữ nhật cần bốn que xếp thành hai cặp bằng nhau. Nếu khung là hình vuông thì cả bốn thanh phải có cùng chiều dài. 

Nhiệm vụ là xây dựng càng nhiều khung hình càng tốt mà không cần cắt hay kết hợp các thanh. Chúng tôi có thể để lại một số que không được sử dụng nếu chúng không thể đóng góp vào khung khác. 

Quan sát quan trọng là mỗi khung hình tiêu thụ chính xác hai cặp có độ dài bằng nhau. Độ dài xuất hiện bốn lần có thể tự tạo một khung hình vuông hoặc đóng góp hai cặp riêng biệt vào các hình chữ nhật khác nhau. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 100 que và độ dài của que cũng nhiều nhất là 100. Ngay cả những giải pháp kém hiệu quả cũng có thể vượt qua một cách thoải mái ở đây, nhưng vấn đề chủ yếu là ở việc nhận ra logic đếm chính xác. 

Một sai lầm phổ biến là nghĩ rằng bốn cây gậy bằng nhau luôn tạo thành chính xác một khung và không thể hỗ trợ ở nơi khác. Hãy xem xét đầu vào này:```
6
5 5 5 5 2 2
```Câu trả lời đúng là:```
1
```Chúng tôi có tổng cộng ba cặp: hai cặp có chiều dài 5 và một cặp có chiều dài 2. Vì mỗi khung cần hai cặp nên chúng tôi chỉ có thể tạo một khung. 

Một trường hợp tinh tế khác xuất hiện khi nhiều độ dài khác nhau, mỗi độ dài chỉ đóng góp một cặp:```
8
1 1 2 2 3 3 4 4
```Câu trả lời đúng là:```
2
```Tổng cộng chúng tôi có bốn cặp, vì vậy chúng tôi có thể tạo hai khung. Việc triển khai bất cẩn cố gắng xây dựng các hình vuông một cách tham lam trước tiên sẽ vẫn hoạt động ở đây, nhưng các công trình xây dựng tham lam phức tạp hơn là không cần thiết vì chỉ có tổng số cặp là quan trọng. 

Trường hợp cạnh tối thiểu là:```
1
7
```Câu trả lời đúng là:```
0
```Một cây gậy không thể tạo thành dù chỉ một cặp. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là liên tục tìm kiếm hai cặp trong số các que còn lại, loại bỏ chúng và đếm một khung hình. Vì kích thước đầu vào rất nhỏ nên chúng tôi thậm chí có thể sắp xếp các que và mô phỏng trích xuất cặp trực tiếp. 

Ví dụ: sau khi sắp xếp, chúng ta có thể quét các giá trị bằng nhau liền kề, thu thập các cặp, sau đó cứ hai cặp được thu thập sẽ tạo thành một khung. Điều này đã gợi ý về cấu trúc thực sự của vấn đề. 

Phối cảnh vũ phu hoạt động vì khung chỉ phụ thuộc vào các cặp có độ dài bằng nhau. Kích thước thực tế không quan trọng ngoài điều đó. Một khi chúng ta nhận ra điều này thì vấn đề trở nên đơn giản hơn nhiều. 

Giả sử một thanh dài xuất hiện`cnt`lần. Sau đó nó góp phần`cnt // 2`cặp có thể sử dụng Mỗi khung hình tiêu thụ chính xác hai cặp. Vì vậy, nếu tổng số cặp là`P`, câu trả lời đơn giản là:```
P // 2
```Điều này tránh mọi việc xây dựng khung rõ ràng. 

Quan sát mở ra giải pháp là danh tính cặp không quan trọng. Một cặp chiều dài 2 và một cặp chiều dài 5 đã tạo thành một hình chữ nhật hợp lệ. Bốn que có cùng chiều dài chỉ cần đóng góp hai cặp. Toàn bộ vấn đề giảm xuống việc đếm tổng thể có bao nhiêu cặp tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số que và chiều dài que. 
2. Đếm xem mỗi chiều dài que xuất hiện bao nhiêu lần. 

Vì chiều dài thanh nhiều nhất là 100 nên chúng ta có thể sử dụng một dãy tần số nhỏ hoặc một từ điển. 
3. Với mỗi độ dài khác nhau, hãy tính xem nó đóng góp bao nhiêu cặp. 

Nếu chiều dài xuất hiện`cnt`nhiều khi nó góp phần`cnt // 2`cặp vì mỗi cặp cần hai que bằng nhau. 
4. Tổng hợp tất cả các cặp đóng góp thành một biến gọi là`pairs`. 
5. Tính toán`pairs // 2`. 

Mỗi khung hình tiêu thụ chính xác hai cặp, do đó chia tổng số cặp cho hai sẽ có số lượng khung hình tối đa. 
6. In kết quả. 

### Tại sao nó hoạt động 

Mỗi khung hợp lệ yêu cầu chính xác hai cặp thanh bằng nhau. Không thể xây dựng khung nào nếu không có hai cặp và hai cặp bất kỳ luôn có thể tạo thành hình chữ nhật. 

Xuất hiện một thời gian dài`cnt`lần, sử dụng nhiều hơn`cnt // 2`cặp là không thể vì mỗi cặp tiêu thụ hai que. Tính tổng theo tất cả các độ dài sẽ cho số lượng cặp tối đa có thể đạt được từ đầu vào. 

Vì mỗi khung tiêu thụ hai cặp độc lập với độ dài thanh, nên số lượng khung hình tối đa chính xác bằng tổng số cặp chia cho hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

freq = [0] * 101

for x in a:
    freq[x] += 1

pairs = 0

for cnt in freq:
    pairs += cnt // 2

print(pairs // 2)
```Giải pháp bắt đầu bằng cách đếm số lần xuất hiện của mỗi chiều dài thanh. Vì chiều dài thanh tối đa chỉ là 100 nên mảng có kích thước cố định sẽ đơn giản và nhanh hơn từ điển. 

Biến`pairs`lưu trữ tổng số cặp thanh bằng nhau có thể sử dụng được. Với mỗi tần số`cnt`, số nguyên chia cho 2 sẽ cho ra số cặp rời nhau mà chúng ta có thể trích xuất được. 

Câu trả lời cuối cùng là`pairs // 2`bởi vì mỗi khung hình tiêu thụ hai cặp. 

Một điểm tinh tế là chúng ta không bao giờ xây dựng hình chữ nhật hoặc hình vuông một cách rõ ràng. Đó là cố ý. Bằng chứng cho thấy chỉ riêng số cặp đã xác định hoàn toàn câu trả lời. 

Một chi tiết khác là phép chia số nguyên xử lý các que còn sót lại một cách tự nhiên. Ví dụ: nếu một độ dài xuất hiện 5 lần thì`5 // 2 = 2`, meaning one stick remains unused.

 ## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 4 3 2 3
```| Chiều dài | Tần số | Các cặp đã thêm | Tổng số cặp | 
| --- | --- | --- | --- | 
| 2 | 2 | 1 | 1 | 
| 3 | 2 | 1 | 2 | 
| 4 | 1 | 0 | 2 | 

Tính toán cuối cùng:```
frames = 2 // 2 = 1
```Đầu ra:```
1
```Ví dụ này cho thấy ý tưởng cốt lõi một cách rõ ràng. Chúng ta không cần bốn cây gậy bằng nhau. Hai cặp độc lập đã tạo thành một hình chữ nhật. 

### Ví dụ 2 

đầu vào:```
10
1 1 1 1 2 2 3 3 3 3
```| Chiều dài | Tần số | Các cặp đã thêm | Tổng số cặp | 
| --- | --- | --- | --- | 
| 1 | 4 | 2 | 2 | 
| 2 | 2 | 1 | 3 | 
| 3 | 4 | 2 | 5 | 

Tính toán cuối cùng:```
frames = 5 // 2 = 2
```Đầu ra:```
2
```Dấu vết này chứng minh rằng độ dài có khả năng bình phương chỉ đơn giản là đóng góp nhiều cặp. Chúng tôi không bao giờ cần xử lý đặc biệt cho hình vuông. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng ta quét các que một lần rồi quét mảng tần số nhỏ | 
| Không gian | O(1) | Mảng tần số có kích thước cố định 101 | 

Giải pháp dễ dàng phù hợp trong giới hạn. Thậm chí những cách tiếp cận chậm hơn nhiều cũng sẽ vượt qua`n = 100`, nhưng cách tính vừa đơn giản vừa tối ưu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    freq = [0] * 101

    for x in a:
        freq[x] += 1

    pairs = 0

    for cnt in freq:
        pairs += cnt // 2

    print(pairs // 2)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("5\n2 4 3 2 3\n") == "1", "sample 1"

# minimum input
assert run("1\n7\n") == "0", "single stick"

# all equal
assert run("8\n5 5 5 5 5 5 5 5\n") == "2", "two square frames"

# many distinct pairs
assert run("8\n1 1 2 2 3 3 4 4\n") == "2", "four pairs total"

# leftover sticks
assert run("7\n1 1 1 2 2 2 3\n") == "1", "unused leftovers"

# maximum useful grouping
assert run("12\n1 1 1 1 2 2 2 2 3 3 3 3\n") == "3", "multiple squares"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`0`| Đầu vào kích thước tối thiểu | 
|`8 / 5 5 5 5 5 5 5 5`|`2`| Nhiều khung hình vuông | 
|`8 / 1 1 2 2 3 3 4 4`|`2`| Độ dài khác nhau kết hợp thành hình chữ nhật | 
|`7 / 1 1 1 2 2 2 3`|`1`| Những que chưa ghép còn sót lại sẽ bị bỏ qua | 
|`12 / 1 1 1 1 2 2 2 2 3 3 3 3`|`3`| Một số nhóm cặp độc lập | 

## Vỏ cạnh 

Xét trường hợp một thanh dài xuất hiện nhiều lần:```
8
5 5 5 5 5 5 5 5
```Tần số của độ dài 5 là 8, do đó nó góp phần`8 // 2 = 4`cặp. Vì mỗi khung hình cần hai cặp nên câu trả lời là`4 // 2 = 2`. 

Thuật toán xử lý chính xác những khung này dưới dạng hai khung hình vuông hoặc bất kỳ sự sắp xếp tương đương nào. Không cần xử lý hình vuông đặc biệt. 

Bây giờ hãy xem xét các que còn sót lại:```
7
1 1 1 2 2 2 3
```Các tần số là:```
1 -> 3
2 -> 3
3 -> 1
```Số lượng cặp trở thành:```
1 -> 1 pair
2 -> 1 pair
3 -> 0 pairs
```Tổng số cặp bằng 2 nên đáp án là 1 khung. 

Các que bổ sung có độ dài 1 và 2 sẽ tự động bị bỏ qua vì phép chia số nguyên sẽ loại bỏ các cặp không đầy đủ. 

Cuối cùng, hãy xem xét nhiều độ dài khác nhau:```
8
1 1 2 2 3 3 4 4
```Mỗi chiều dài đóng góp chính xác một cặp, tạo thành tổng cộng bốn cặp. Đầu ra của thuật toán`4 // 2 = 2`. 

Điều này khẳng định tính bất biến trung tâm của nghiệm: chỉ có tổng số cặp là quan trọng chứ không phải độ dài của các cặp đó là bao nhiêu.
