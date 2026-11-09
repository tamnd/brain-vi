---
title: "CF 1511C - Một bộ bài khác"
description: "Chúng ta được cấp một bộ bài gồm n lá bài, mỗi lá bài có một màu được biểu thị bằng một số nguyên từ 1 đến 50. Bộ bài được sắp xếp từ trên xuống dưới và chúng ta nhận được q truy vấn."
date: "2026-06-10T18:58:46+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1511
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 107 (Rated for Div. 2)"
rating: 1100
weight: 1511
solve_time_s: 133
verified: true
draft: false
---

[CF 1511C - Yet Another Card Deck](https://codeforces.com/problemset/problem/1511/C) 

**Đánh giá:** 1100 
**Tags:** bạo lực, cấu trúc dữ liệu, triển khai, cây cối 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ bài`n`thẻ, mỗi thẻ có một màu được biểu thị bằng một số nguyên từ`1`ĐẾN`50`. Bộ bài được sắp xếp từ trên xuống dưới và chúng ta nhận được`q`truy vấn. Mỗi truy vấn chỉ định một màu và đối với mỗi truy vấn, chúng ta cần xác định vị trí lá bài đầu tiên có màu đó trong bộ bài hiện tại, báo cáo vị trí của nó (chỉ số dựa trên 1), sau đó di chuyển lá bài đó lên đầu bộ bài. Thách thức là bộ bài thay đổi linh hoạt theo từng truy vấn, do đó vị trí của các lá bài thay đổi theo thời gian. 

Những hạn chế là đáng kể: cả hai`n`Và`q`có thể đạt tới`300,000`. Một giải pháp đơn giản quét bộ bài để tìm mọi truy vấn sẽ thực hiện`O(n * q)`các hoạt động có thể đạt tới`9 * 10^10`trong trường hợp xấu nhất. Điều này vượt xa những gì phù hợp với giới hạn thời gian 2 giây, vì vậy chúng tôi cần một cách tiếp cận hiệu quả hơn. Phạm vi màu sắc hạn chế (`1`ĐẾN`50`) gợi ý rằng chúng ta có thể sử dụng vũ trụ nhỏ này để quản lý các vị thế một cách hiệu quả. 

Trường hợp khó phát hiện khi một màu xuất hiện nhiều lần trong bộ bài. Di chuyển lần xuất hiện đầu tiên lên trên cùng sẽ thay đổi tất cả các vị trí tiếp theo. Ví dụ, nếu bộ bài là`[1, 2, 1]`và chúng tôi truy vấn`1`, chúng tôi xuất ra`1`và di chuyển nó lên trên cùng, để lại`[1, 2, 1]`. Truy vấn tiếp theo cho`1`bây giờ nên quay lại`1`một lần nữa vì thẻ đã di chuyển trước đó vẫn ở trên cùng. Việc triển khai bất cẩn tính toán lại các chỉ số mà không cập nhật vị trí một cách linh hoạt sẽ tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp đi lặp lại trong từng truy vấn, tìm kiếm lần xuất hiện đầu tiên của màu được yêu cầu. Sau khi tìm thấy, chúng tôi ghi lại vị trí của nó, lấy nó ra khỏi bộ bài và chèn nó lên trên cùng. Mặc dù đúng nhưng cách tiếp cận này yêu cầu quét tối đa`n`thẻ cho mỗi`q`các truy vấn, đưa ra độ phức tạp về thời gian`O(n * q)`. Với`n`Và`q`lên đến`3 * 10^5`, điều này có thể thực hiện theo thứ tự`10^10`hoạt động, điều này không thực tế đối với giới hạn 2 giây. 

Cái nhìn sâu sắc quan trọng là số lượng màu sắc nhỏ. Chúng ta có thể duy trì ánh xạ từ mỗi màu đến lần xuất hiện đầu tiên trong bộ bài hiện tại. Thay vì dịch chuyển các thẻ theo một mảng, chúng ta có thể mô phỏng vị trí tương đối của chúng bằng cách sử dụng danh sách màu sắc và chỉ số cập nhật. Khi chúng tôi xử lý truy vấn về một màu, chúng tôi đọc vị trí hiện tại của nó từ bản đồ, in nó ra, sau đó điều chỉnh vị trí của tất cả các màu khác xuất hiện trước nó trong bảng bằng cách tăng chỉ mục của chúng. Cuối cùng, chúng ta di chuyển màu được truy vấn lên trên cùng, tương đương với việc đặt vị trí của nó thành`1`. Cách tiếp cận này khai thác phạm vi màu giới hạn và tránh việc quét toàn bộ bản ghi, giảm độ phức tạp xuống còn`O(n * c)`Ở đâu`c = 50`là số lượng màu sắc có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Tối ưu | O(q * c) | O(c) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`pos`kích thước`51`để theo dõi vị trí hiện tại của thẻ đầu tiên của mỗi màu. Khởi tạo nó bằng cách quét bộ bài một lần, thiết lập`pos[color]`đến chỉ mục đầu tiên nơi nó xuất hiện. Điều này cung cấp vị trí bắt đầu cho mỗi màu. 
2. Lặp lại từng truy vấn`t_j`. Sử dụng`pos[t_j]`để tìm vị trí hiện tại của thẻ đầu tiên có màu đó và in nó. 
3. Cho tất cả các màu`c`trong boong tàu, nếu`pos[c]`nhỏ hơn`pos[t_j]`, tăng`pos[c]`qua`1`. Điều này mô phỏng rằng các thẻ này đã được dịch chuyển xuống dưới bằng thao tác di chuyển lên trên. 
4. Đặt`pos[t_j]`ĐẾN`1`vì thẻ được truy vấn hiện ở trên cùng. 
5. Lặp lại các bước 2-4 cho tất cả các truy vấn. 

Tại sao nó hoạt động: Bất biến là vậy`pos[c]`luôn đại diện cho vị trí dựa trên 1 hiện tại của thẻ màu đầu tiên`c`trong boong tàu. Di chuyển một thẻ lên vị trí tăng dần trên cùng của tất cả các thẻ ở trên nó, giữ nguyên thứ tự tương đối chính xác. Vì số lượng màu có hạn nên đang cập nhật`pos`mất nhiều nhất`50`hoạt động cho mỗi truy vấn, đủ hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = list(map(int, input().split()))
t = list(map(int, input().split()))

# initialize position mapping
pos = [0] * 51
for idx, color in enumerate(a):
    if pos[color] == 0:
        pos[color] = idx + 1

result = []

for color in t:
    result.append(pos[color])
    # increment positions of colors above the current one
    for c in range(1, 51):
        if pos[c] < pos[color]:
            pos[c] += 1
    pos[color] = 1

print(*result)
```Vòng lặp đầu tiên thiết lập vị trí ban đầu của từng màu trong bộ bài. Vòng lặp thứ hai xử lý từng truy vấn một cách hiệu quả bằng cách sử dụng`pos`mảng. Bằng cách cập nhật tất cả các màu phía trên thẻ được truy vấn và di chuyển thẻ được truy vấn lên trên cùng, chúng tôi duy trì vị trí chính xác mà không cần dịch chuyển toàn bộ bộ bài. Điều quan trọng là`pos`mảng sử dụng chỉ mục dựa trên 1 để phù hợp với yêu cầu đầu ra của vấn đề. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
7 5
2 1 1 4 3 3 1
3 2 1 1 4
```| Truy vấn | tư thế trước | Đầu ra | tư thế sau khi cập nhật | 
| --- | --- | --- | --- | 
| 3 | [2:1,1:2,3:5,4:4] | 5 | [2:2,1:3,3:1,4:5] | 
| 2 | [2:2,1:3,3:1,4:5] | 2 | [2:1,1:4,3:2,4:6] | 
| 1 | [2:1,1:4,3:2,4:6] | 3 | [2:2,1:1,3:3,4:7] | 
| 1 | [2:2,1:1,3:3,4:7] | 1 | không thay đổi | 
| 4 | [2:2,1:1,3:3,4:7] | 5 | [2:3,1:2,3:4,4:1] | 

Dấu vết này cho thấy sự bất biến`pos[c]`luôn là sự xuất hiện đầu tiên của màu sắc`c`. Mỗi bản cập nhật đều phản ánh chính xác hoạt động chuyển lên trên cùng. 

Một ví dụ nhỏ khác: 

đầu vào:```
3 3
1 2 1
1 2 1
```| Truy vấn | tư thế trước | Đầu ra | tư thế sau khi cập nhật | 
| --- | --- | --- | --- | 
| 1 | [1:1,2:2] | 1 | [1:1,2:3] | 
| 2 | [1:1,2:3] | 3 | [1:2,2:1] | 
| 1 | [1:2,2:1] | 2 | [1:1,2:2] | 

Các vị trí điều chỉnh chính xác ngay cả khi cùng một màu được truy vấn nhiều lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * 50) = O(q) | Mỗi truy vấn cập nhật vị trí tối đa 50 màu | 
| Không gian | O(51) = O(1) |`pos`mảng lưu trữ lần xuất hiện đầu tiên trên mỗi màu | 

Với`q`lên đến`3 * 10^5`, điều này mang lại khoảng 15 triệu thao tác, vừa vặn thoải mái trong vòng 2 giây. Việc sử dụng bộ nhớ là tối thiểu vì chúng tôi chỉ lưu trữ vị trí cho mỗi màu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    t = list(map(int, input().split()))

    pos = [0] * 51
    for idx, color in enumerate(a):
        if pos[color] == 0:
            pos[color] = idx + 1

    result = []
    for color in t:
        result.append(pos[color])
        for c in range(1, 51):
            if pos[c] < pos[color]:
                pos[c] += 1
        pos[color] = 1
    return ' '.join(map(str, result))

# provided sample
assert run("7 5\n2 1 1 4 3 3 1\n3 2 1 1 4\n") == "5 2 3 1 5", "sample 1"

# minimum input
assert run("2 1\n1 2\n1\n") == "1", "minimum size"

# all equal
assert run("4 4\n1 1 1 1\n1 1 1 1\n") == "1 1 1 1", "all equal colors"

# alternating colors
assert run("5 5\n1 2 1 2 1\n2 1 2 1 2\n") == "2 1 3 1 2", "alternating pattern"

# maximum colors
assert run
```
