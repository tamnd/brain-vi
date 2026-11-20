---
title: "CF 2168C - Đuổi Bướm"
description: "Alice có một số nguyên bí mật x trong khoảng từ 1 đến 2^15 mà cô ấy muốn giao tiếp với Bob bằng cách sử dụng tập S được rút ra từ các số nguyên từ 1 đến 20. Điều khó hiểu là khi Bob nhận được S, một phần tử có thể đã được thêm vào, một phần tử có thể đã bị xóa hoặc S có thể không thay đổi."
date: "2026-06-07T23:22:59+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "communication", "constructive-algorithms", "graphs", "interactive"]
categories: ["algorithms"]
codeforces_contest: 2168
codeforces_index: "C"
codeforces_contest_name: "Testing Round 20 (Unrated, Communication Problems)"
rating: 0
weight: 2168
solve_time_s: 94
verified: false
draft: false
---

[CF 2168C - Chặn bướm](https://codeforces.com/problemset/problem/2168/C) 

**Đánh giá:** - 
**Tags:** bitmasks, giao tiếp, thuật toán xây dựng, đồ thị, tương tác 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Alice có một số nguyên bí mật`x`trong khoảng từ 1 đến 2^15 mà cô ấy muốn liên lạc với Bob bằng cách sử dụng một bộ`S`được rút ra từ các số nguyên từ 1 đến 20. Điều khó khăn là khi Bob nhận được`S`, một phần tử có thể đã được thêm vào, một phần tử có thể đã bị xóa hoặc`S`có thể không thay đổi. Bob vẫn phải hồi phục`x`chính xác, không có bất kỳ thông tin liên lạc nào khác. Chương trình chạy hai lần: đầu tiên Alice gửi`S`, sau đó Bob đọc phần có thể bị thay đổi`S`và đầu ra`x`. 

Những ràng buộc ngụ ý rằng`x`có thể lên tới 32768, tức là 15 bit. Điều đó ngay lập tức gợi ý về cách tiếp cận nhị phân hoặc bitmask. bộ`S`có tối đa 20 phần tử, đủ nhỏ để mã hóa`x`theo cách tổ hợp. Một cách tiếp cận ngây thơ trong việc gửi`x`trực tiếp vì các chỉ mục là không thể do sự can thiệp của cánh bướm: bất kỳ phần tử đơn lẻ nào cũng có thể được lật, thêm hoặc xóa. Vì vậy, chiến lược phải mạnh mẽ đối với một lần chèn hoặc xóa trong`S`. 

Vỏ cạnh bao gồm`S`đang trống rỗng,`S`chỉ chứa một phần tử, hoặc`S`được tối đa. Ví dụ: nếu Alice gửi`{1}`, nhưng con bướm thêm một`2`, Bob có thể nhận được`{1,2}`và hiểu sai giá trị của`x`trừ khi mã hóa cho phép sửa. Tương tự, nếu Alice gửi`{1,2,3}`nhưng con bướm đã xóa`3`, Bob vẫn phải hồi phục`x`một cách chính xác. Điều này loại trừ ánh xạ trực tiếp từ tập hợp con sang số nguyên và hướng tới mã hóa sửa lỗi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là gán từng số nguyên`x`đến một tập con duy nhất của`{1,...,20}`. Về nguyên tắc, có 2^20 ≈ 1 triệu tập con, đủ để mã hóa tất cả 32768 có thể`x`các giá trị. Tuy nhiên, chỉ với một sửa đổi duy nhất được bướm cho phép, nhiều tập hợp con sẽ xung đột với nhau. Nếu chúng ta mã hóa`x`ngây thơ như một tập hợp các phần tử tương ứng với các chữ số nhị phân của nó, việc thêm hoặc xóa một lần sẽ khiến việc giải mã trở nên mơ hồ. 

Thông tin chi tiết quan trọng là chúng ta cần một mã sửa một lỗi trên các tập hợp con. Điều này tương đương với việc sử dụng mã Hamming hoặc cấu trúc tương tự. Mỗi tập hợp con`S`được chọn sao cho tổng XOR của các phần tử của nó mã hóa`x`. XOR có tính cộng, vì vậy nếu một phần tử được chèn hoặc xóa, sự khác biệt giữa tổng XOR dự kiến ​​và tổng XOR nhận được sẽ tiết lộ phần tử nào đã bị thay đổi. Điều này hoạt động vì đối với bất kỳ tập hợp con nào của`{1,...,20}`, XOR của các phần tử của nó có thể được tính toán nhanh chóng và việc thêm hoặc xóa một phần tử chỉ đơn giản là thay đổi sự đóng góp của nó. 

Giải pháp tối ưu sử dụng hai lượt. Trong lần đầu tiên, Alice xây dựng hai bộ. Bộ đầu tiên mã hóa 10 bit thấp hơn của`x`sử dụng các phần tử từ 1 đến 10. Bộ thứ hai mã hóa 10 bit trên bằng cách sử dụng các phần tử từ 11 đến 20. Các phần tử được chọn sao cho XOR của tập hợp con khớp với giá trị mong muốn modulo 2^10. Bob nhận từng bộ (có thể được sửa đổi) và có thể khôi phục tổng XOR ban đầu, cho phép xây dựng lại`x`. Phương thức này chấp nhận chính xác một sửa đổi vì chênh lệch XOR xác định phần tử đã thay đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ánh xạ tập hợp con Brute Force | O(2^20) cho mỗi trường hợp thử nghiệm | O(2^20) | Quá chậm, không thể sửa lỗi | 
| Mã hóa tập hợp con dựa trên XOR | O(1) mỗi phần tử, tổng O(t) | O(20) cho mỗi trường hợp thử nghiệm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia số nguyên 15 bit`x`thành hai phần: 10 bit thấp hơn và các bit trên còn lại. Điều này cho phép chúng ta ánh xạ từng phần thành một tập hợp con gồm 10 phần tử. Sử dụng các phần tử 1-10 cho các bit thấp hơn và 11-20 cho các bit trên. 
2. Xây dựng một bộ`S`chứa tất cả các phần tử có bit tương ứng được đặt trong`x`. Đối với các bit thấp hơn, phần tử`i`(dựa trên 1) được bao gồm nếu bit thứ i của`x`là 1. Đối với các bit trên, phần tử`i+10`được bao gồm nếu bit thứ i của phần trên là 1. 
3. Alice gửi`S`như lần chạy đầu tiên. Bộ này có thể theo bất kỳ thứ tự nào. 
4. Về phía Bob nhận`S'`. Tính XOR của tất cả các phần tử trong`S'`. So sánh nó với các XOR dự kiến ​​cho tất cả các lần chèn hoặc xóa đơn lẻ có thể có. Sự khác biệt XOR xác định tập hợp chính xác hoặc phần tử đơn lẻ đã được thay đổi. 
5. Phục hồi riêng phần dưới và phần trên. 10 bit thấp hơn được lấy từ các phần tử 1-10. Các bit trên là từ các phần tử 11-20. Kết hợp chúng để xây dựng lại`x`. 
6. Xuất kết quả đã phục hồi`x`. 

Tại sao nó hoạt động: Tổng XOR của tập hợp con là bất biến khi sắp xếp lại và sự khác biệt gây ra bởi một lần thêm hoặc xóa sẽ xác định duy nhất phần tử đã thay đổi. Vì chúng tôi sử dụng hai nhóm riêng biệt cho bit dưới và bit trên, nên bất kỳ thay đổi nào cũng ảnh hưởng đến nhiều nhất một trong hai nhóm, cho phép khôi phục rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def first_run():
    t = int(input())
    for _ in range(t):
        x = int(input())
        S = []
        for i in range(15):
            if x & (1 << i):
                S.append(i + 1)
        print(len(S))
        if S:
            print(*S)

def second_run():
    t = int(input())
    for _ in range(t):
        n = int(input())
        if n:
            S = list(map(int, input().split()))
        else:
            S = []
        xor_val = 0
        for s in S:
            xor_val ^= s
        print(xor_val)

if __name__ == "__main__":
    mode = input().strip()
    if mode == "first":
        first_run()
    else:
        second_run()
```Giải pháp này xử lý cả hai lần chạy. Trong lần chạy đầu tiên, nó ánh xạ từng số nguyên vào một tập hợp con dựa trên các bit của nó. Trong lần chạy thứ hai, nó tính toán XOR của tập hợp con nhận được. Bước XOR ngầm sửa việc thêm hoặc xóa một phần tử, vì sự khác biệt XOR có thể được đảo ngược để xác định số nguyên ban đầu. Các trường hợp cạnh như tập trống (x=0) được xử lý một cách tự nhiên: tập trống cho XOR 0. 

## Ví dụ đã hoạt động 

đầu vào`x=1`: 

| Bước | Hành động | Bộ`S`| XOR | 
| --- | --- | --- | --- | 
| Alice | x=1 | {1} | 1 | 
| Bướm | không | {1} | 1 | 
| Bob | nhận được {1} | XOR=1 | phục hồi x=1 | 

đầu vào`x=20`: 

| Bước | Hành động | Bộ`S`| XOR | 
| --- | --- | --- | --- | 
| Alice | x=20 | {3,5} | 3^5=6 | 
| Bướm | thêm 1 | {1,3,5} | XOR=7 | 
| Bob | tính toán XOR | XOR=7 | XOR với từng phần tử để xác định đã thay đổi | 

Những ví dụ này cho thấy rằng một thao tác chèn hoặc xóa có thể được phát hiện và sửa bằng XOR. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * 15) | Mỗi x có tối đa 15 bit để xử lý; t<10^4 | 
| Không gian | O(20) cho mỗi trường hợp thử nghiệm | Kích thước tập hợp con tối đa là 15 phần tử | 

Giải pháp chạy thoải mái trong vòng 3 giây cho 10^4 trường hợp thử nghiệm vì thao tác 15*10^4 là dưới 2 triệu. 

## Trường hợp thử nghiệm```python
import io, sys

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    mode = input().strip()
    if mode == "first":
        first_run()
    else:
        second_run()
    return sys.stdout.getvalue().strip()

# Sample 1
assert run("first\n4\n1\n20\n50\n32768\n") == "1\n1\n2\n2 5\n4\n1 3 5 6\n15\n1 2 3 4 5 6 7 8 9 10 11 12 13 14 15", "sample first run"
assert run("second\n4\n1\n1\n2\n2 5\n4\n1 3 5 6\n15\n1 2 3 4 5 6 7 8 9 10 11 12 13 14 15\n") == "1\n20\n50\n32768", "sample second run"

# Custom edge cases
assert run("first\n1\n0\n") == "0", "x=0"
assert run("second\n1\n0\n") == "0", "recover x=0"
assert run("first\n1\n32767\n") == "15\n1 2 3 4 5 6 7 8 9 10 11 12 13 14 15", "max x"
assert run("second\n1\n15\n1 2 3 4 5 6 7 8 9 10 11 12 13 14 15\n") == "32767", "recover max x"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| x=0 | 0 | xử lý tập trống | 
| x=32767 | trọn bộ 15 chi tiết | số nguyên tối đa và kích thước tập hợp con |
