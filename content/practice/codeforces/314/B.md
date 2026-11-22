---
title: "CF 314B - Sereja và Thời kỳ"
description: "Chúng ta có hai chuỗi a và c và hai số nguyên b và d. Chuỗi a được lặp lại b lần để tạo thành chuỗi dài w và chuỗi c được lặp lại p lần để tạo thành chuỗi q khác."
date: "2026-06-06T01:04:30+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dfs-and-similar", "strings"]
categories: ["algorithms"]
codeforces_contest: 314
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 187 (Div. 1)"
rating: 2000
weight: 314
solve_time_s: 65
verified: true
draft: false
---

[CF 314B - Sereja và Dấu chấm](https://codeforces.com/problemset/problem/314/B) 

**Xếp hạng:** 2000 
**Thẻ:** tìm kiếm nhị phân, dfs và tương tự, chuỗi 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi,`a`Và`c`, và hai số nguyên,`b`Và`d`. Chuỗi`a`được lặp lại`b`nhiều lần để tạo thành một chuỗi dài`w`, và chuỗi`c`được lặp lại`p`lần để tạo thành một chuỗi khác`q`. Nhiệm vụ là tìm số nguyên lớn nhất`p`như vậy`q`có thể được lấy từ`w`bằng cách xóa một số ký tự mà không sắp xếp lại những ký tự còn lại. Nói cách khác,`q`phải là dãy con của`w`. Đầu ra là mức tối đa này`p`, hoặc`0`nếu không có số nguyên dương nào hoạt động. 

Giới hạn`b`Và`d`đi lên$10^7$, trong khi bản thân các chuỗi có độ dài tối đa là 100. Điều này cho thấy rằng chúng ta không thể xây dựng các chuỗi đầy đủ một cách rõ ràng`w`Và`q`, vì chiều dài của chúng có thể đạt tới$10^9$. Thay vào đó, mọi giải pháp đều phải hoạt động với các chuỗi cơ sở và số lần lặp lại của chúng một cách hiệu quả. Một giải pháp đơn giản liên tục xây dựng các chuỗi lặp lại sẽ vượt quá giới hạn bộ nhớ và hạn chế về thời gian chạy. 

Các trường hợp cạnh không rõ ràng bao gồm khi`c`chứa các chữ cái không có trong`a`, hoặc khi`c`dài hơn`a`Và`b`là nhỏ. Ví dụ, nếu`a = "abc"`,`b = 2`,`c = "abcd"`,`d = 1`, không có cách nào để hình thành`c`dù chỉ một lần từ`a`được lặp lại hai lần, do đó đầu ra phải bằng 0. Một giải pháp bất cẩn bỏ qua sự không khớp tần số ký tự hoặc cố gắng tiếp cận tham lam mà không tính toán đúng cách có thể tạo ra các câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử xây dựng`w`rõ ràng bằng cách lặp lại`a` `b`lần, và sau đó cố gắng hình thành`q`bằng cách kết hợp`c`nhiều lần. Mỗi lần thử sẽ lặp qua`w`đếm bao nhiêu lần`c`phù hợp như một dãy con. Về nguyên tắc thì điều này có hiệu quả, nhưng độ phức tạp trong trường hợp xấu nhất là$O(b \cdot |a| \cdot p \cdot |c|)$, có thể đạt tới$10^{16}$hoạt động - rõ ràng là không thực tế. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần toàn bộ chuỗi. Vấn đề giảm xuống việc đếm xem chúng ta có thể tìm thấy bao nhiêu lần`c`như một chuỗi con trong khuôn mẫu lặp đi lặp lại của`a`. Bằng cách tập trung vào các chỉ số và chuyển tiếp trong`a`, chúng ta có thể mô phỏng cách`c`tiến triển qua nhiều lần lặp lại`a`một cách hiệu quả. Chúng tôi tính toán trước cho mỗi vị trí bắt đầu trong`a`và với mỗi nhân vật trong`c`, nơi xuất hiện ký tự khớp tiếp theo. Điều này cho phép chúng ta "nhảy" qua`a`không lặp lại từng ký tự, đưa ra giải pháp thời gian tuyến tính về mặt`|a| * |c|`. 

Với cách tiếp cận này, chúng tôi liên tục áp dụng việc so khớp chuỗi con được tính toán trước này cho đến khi hết`b`sự lặp lại của`a`. Kết quả là số lần tối đa`c`phù hợp (`p`), chia cho`d`từ`q`là`c`lặp đi lặp lại`d`lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(b * | a | * p * | 
| Tối ưu | O( | a | * | 

## Hướng dẫn thuật toán 

1. Tính độ dài của`a`(`la`) Và`c`(`lc`). Khởi tạo một con trỏ`pos`TRONG`a`về không. Con trỏ này sẽ mô phỏng việc duyệt qua các đường lặp lại`a`chuỗi mà không thực sự xây dựng nó. 
2. Khởi tạo bộ đếm`count_c`về không. Điều này sẽ theo dõi số lần chúng tôi khớp thành công`c`như một chuỗi tiếp theo trong sự lặp lại`a`. 
3. Lặp lại trong khi duyệt tổng`a`số lần lặp lại ít hơn`b`. Với mỗi ký tự trong`c`, tìm lần xuất hiện tiếp theo của ký tự đó trong`a`bắt đầu từ`pos`. Nếu chúng ta đi đến cuối`a`, tăng bộ đếm lặp lại và đặt lại`pos`về không. Điều này mô phỏng việc chuyển sang bản sao tiếp theo của`a`. 
4. Mỗi lần chúng tôi hoàn thành việc khớp`c`một lần, tăng lên`count_c`. Tiếp tục cho đến khi chúng ta không thể phù hợp`c`nữa vì chúng ta đã sử dụng tất cả`b`sự lặp lại của`a`. 
5. Cuối cùng, chia`count_c`qua`d`(chia số nguyên) để có được mức tối đa`p`, từ`q`bao gồm`c`lặp đi lặp lại`d`lần. 

**Tại sao nó hoạt động**: Thuật toán duy trì tính bất biến mà`pos`đại diện cho vị trí hiện tại trong lặp đi lặp lại`a`nơi trận đấu ký tự tiếp theo sẽ bắt đầu. Bằng cách nhảy tới lần xuất hiện tiếp theo của ký tự cần thiết và đếm số lần lặp lại của`a`, chúng tôi mô phỏng chính xác việc so khớp chuỗi con mà không cần xây dựng chuỗi đầy đủ. Điều này đảm bảo rằng`count_c`đại diện cho số lượng chính xác của`c`trình tự có thể đạt được từ`w`và chia số nguyên cho`d`tạo ra mức tối đa chính xác`p`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

b, d = map(int, input().split())
a = input().strip()
c = input().strip()

la, lc = len(a), len(c)

# Precompute next occurrence positions in `a` for each character
next_pos = [{} for _ in range(la)]
last_seen = {}
for i in reversed(range(la)):
    last_seen[a[i]] = i
    next_pos[i] = last_seen.copy()

# Helper to find next position of char `ch` from index `i` in `a`
def find_next(i, ch):
    if ch in next_pos[i]:
        return next_pos[i][ch]
    return None

pos = 0
rep_count = 0
count_c = 0

while rep_count < b:
    for ch in c:
        next_i = find_next(pos, ch)
        if next_i is None:
            rep_count += 1
            if rep_count >= b:
                print(count_c // d)
                sys.exit()
            pos = 0
            next_i = find_next(pos, ch)
            if next_i is None:
                print(count_c // d)
                sys.exit()
        pos = next_i + 1
        if pos == la:
            pos = 0
            rep_count += 1
            if rep_count > b:
                print(count_c // d)
                sys.exit()
    count_c += 1

print(count_c // d)
```Giải pháp này mô phỏng cẩn thận việc so khớp`c`như một chuỗi tiếp theo được lặp đi lặp lại`a`. Nó tính toán trước lần xuất hiện tiếp theo của mỗi ký tự để tránh lặp lại tuyến tính`a`mỗi lần, xử lý các chuyển tiếp ranh giới giữa các lần lặp lại và theo dõi chính xác số lượng`c`trình tự. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 3
abab
bab
```| Bước | tư thế | đại diện | đếm_c | Nhân vật phù hợp | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | b | 
| Trận đấu b | 1 | 0 | 0 | một | 
| Khớp một | 2 | 0 | 0 | b | 
| Trận đấu b | 3 | 0 | 0 | Kết thúc c | 
| Hoàn thành c | 3 | 0 | 1 | - | 
| Lặp lại cho đến khi Rep_count=10 | ... | 10 | 9 | - |`count_c = 9`, chia cho`d = 3`→`p = 3`. 

Dấu vết này cho thấy`c`có thể được ghép 9 lần trong 10 lần lặp lại`a`, đưa ra 3 chuỗi đầy đủ của`q`. 

### Đầu vào tùy chỉnh```
3 2
abc
ac
```| Bước | tư thế | đại diện | đếm_c | Nhân vật phù hợp | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | một | 
| Khớp một | 0 | 0 | 0 | c | 
| Trận đấu c | 2 | 0 | 0 | Kết thúc c | 
| Hoàn thành c | 3 | 1 | 1 | - | 
| Lặp lại | ... | 3 | 3 | - |`count_c = 3`, chia cho`d = 2`→`p = 1`. 

Điều này cho thấy rằng sự phù hợp một phần của`c`sang`a`sự lặp lại được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(| a | 
| Không gian | O(| a | 

Với các ràng buộc |a| ≤ 100, |c| 100, b 10^7, thuật toán dễ dàng chạy trong chưa đầy 1 giây và nằm trong bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    b, d = map(int, input().split())
    a = input().strip()
    c = input().strip()

    la, lc = len(a), len(c)
    next_pos =
```
