---
title: "CF 43C - Vé may mắn"
description: "Chúng tôi được tặng một bộ sưu tập các mảnh vé. Mỗi mảnh là một con số, đại diện cho mảnh vé ban đầu được chia cho ba. Bài toán yêu cầu chúng ta tái tạo lại số lượng vé may mắn tối đa có thể từ những mảnh này."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 43
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 42 (Div. 2)"
rating: 1300
weight: 43
solve_time_s: 72
verified: true
draft: false
---
[CF 43C - Vé may mắn](https://codeforces.com/problemset/problem/43/C) 

**Đánh giá:** 1300 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập các mảnh vé. Mỗi mảnh là một con số, đại diện cho mảnh vé ban đầu được chia cho ba. Bài toán yêu cầu chúng ta tái tạo lại số lượng vé may mắn tối đa có thể từ những mảnh này. Một tấm vé may mắn được định nghĩa là một số chia hết cho ba và mỗi tấm vé được làm lại phải có đúng hai mảnh được dán lại với nhau. Các mảnh có thể được dán theo bất kỳ thứ tự nào, vì vậy nếu chúng ta có các mảnh`x`Và`y`, chúng ta có thể hình thành`xy`hoặc`yx`. 

Đầu vào bao gồm số phần`n`và một danh sách các số nguyên đại diện cho mỗi phần. Đầu ra là số lượng vé may mắn tối đa mà chúng tôi có thể tạo ra bằng cách ghép các mảnh. Vì chúng ta đang xử lý tối đa 10.000 phần nên bất kỳ thuật toán nào thử tất cả các cặp có thể một cách rõ ràng sẽ liên quan đến tối đa$\binom{10^4}{2} \approx 5 \times 10^7$hoạt động quá chậm trong giới hạn thời gian 2 giây. Điều này chỉ ra rằng chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Một trường hợp phức tạp phát sinh khi chúng ta có các phần riêng lẻ chia hết cho ba nhưng không thể ghép với các phần khác để duy trì khả năng chia hết. Ví dụ, nếu tất cả các phần đều`3`, chúng ta có thể ghép chúng một cách tự do, nhưng nếu chúng ta có`3, 3, 1`, cái`1`không thể ghép nối để tạo thành một vé chia hết cho ba trừ khi khớp với phần còn lại bổ sung phù hợp. Một kịch bản phức tạp khác xảy ra khi các phần có phần dư 1 và 2 modulo 3: chúng bổ sung cho nhau, nhưng việc ghép đôi đơn giản có thể khiến phần còn lại không được sử dụng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ lặp lại trên tất cả các cặp phần, kiểm tra cả hai phần nối xem có chia hết cho ba không và đếm các vé hợp lệ. Điều này có tác dụng vì việc kiểm tra mức chia hết cho 3 rất đơn giản, nhưng đối với$n = 10^4$, chúng ta sẽ cần khoảng$n^2 = 10^8$kiểm tra trong trường hợp xấu nhất. Điều này là quá chậm do hạn chế về thời gian. 

Cái nhìn sâu sắc quan trọng là chúng tôi chỉ quan tâm đến khả năng chia hết cho ba. Khả năng chia hết cho ba được bảo toàn khi cộng các chữ số và ghép các số`x`Và`y`tương đương với việc đánh giá`(x * 10^len(y) + y) % 3`. Theo tính chất của số học mô đun,`10^k % 3 == 1`cho bất kỳ`k >= 1`. Điều này đơn giản hóa việc kiểm tra của chúng tôi:`(x * 10^len(y) + y) % 3 == (x + y) % 3`. Do đó, chúng ta có thể phân loại tất cả các phần theo phần dư của chúng theo modulo 3. Giả sử`count0`là số phần chia hết cho 3,`count1`là số dư 1, và`count2`là số dư 2. 

Với cách phân nhóm này, bất kỳ cặp nào`(0,0)`,`(1,2)`, hoặc`(0,0)`tạo thành một tấm vé may mắn. Số lượng vé may mắn tối đa sau đó có thể được tính bằng cách ghép nhiều vé`1`s với`2`s càng tốt và sử dụng các mảnh chia hết cho 3 còn lại theo cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách các phần và tính từng phần theo modulo 3. Bước này chuyển bài toán từ xử lý số lớn sang xử lý ba số đếm nhỏ: số dư 0, số dư 1 và số dư 2. 
2. Đếm số miếng ở mỗi loại còn lại:`count0`,`count1`,`count2`. 
3. Tính các cặp dư 1 và 2. Đây là`pairs12 = min(count1, count2)`bởi vì mỗi cặp yêu cầu một mảnh từ mỗi loại. 
4. Tính các cặp từ các phần còn lại 0:`pairs0 = count0 // 2`bởi vì hai phần còn lại-0 luôn có tổng bằng bội số của 3. 
5. Tổng hợp 2 loại cặp để có tổng số vé may mắn tối đa:`max_tickets = pairs12 + pairs0`. 

Tại sao nó hoạt động: Mỗi phần thuộc về chính xác một danh mục còn lại. Ghép 1 và 2 đảm bảo khả năng chia hết, ghép 0 với 0 đảm bảo khả năng chia hết và các phần còn sót lại không thể tạo thành bất kỳ vé chia hết cho ba mới nào mà không vi phạm ràng buộc hai phần. Điều này đảm bảo chúng tôi tối đa hóa số lượng vé mà không để lại bất kỳ cặp tiềm năng nào không được sử dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
pieces = list(map(int, input().split()))

count0 = count1 = count2 = 0

for num in pieces:
    r = num % 3
    if r == 0:
        count0 += 1
    elif r == 1:
        count1 += 1
    else:
        count2 += 1

pairs12 = min(count1, count2)
pairs0 = count0 // 2

print(pairs12 + pairs0)
```Mã đầu tiên phân loại các phần theo phần dư modulo 3 của chúng. Việc đếm là chuyện nhỏ nhưng không được trộn lẫn các loại. Việc ghép các số dư bổ sung rất đơn giản với`min`và phép chia số nguyên xử lý các cặp số dư-0. Lỗi sai lệch một sẽ xảy ra nếu chúng ta sử dụng phép chia nổi hoặc cố gắng ghép các phần còn lại 0 với các phần còn lại 1/2. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`3\n123 123 99\n`| mảnh | mod 3 | 
| --- | --- | 
| 123 | 0 | 
| 123 | 0 | 
| 99 | 0 |`count0 = 3`,`count1 = 0`,`count2 = 0`

`pairs0 = 3 // 2 = 1`

`pairs12 = min(0,0) = 0`

`max_tickets = 1 + 0 = 1`**Ví dụ 2:** 

đầu vào:`4\n1 2 1 2\n`| mảnh | mod 3 | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 1 | 1 | 
| 2 | 2 |`count0 = 0`,`count1 = 2`,`count2 = 2`

`pairs12 = min(2,2) = 2`

`pairs0 = 0`

`max_tickets = 2 + 0 = 2`Điều này xác nhận rằng thuật toán ghép cặp chính xác các phần dư bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính modulo và đếm | 
| Không gian | O(1) | Chỉ cần ba quầy, không cần mảng bổ sung | 

Điều này phù hợp thoải mái trong các ràng buộc:$n \le 10^4$và 2 giây cho phép thực hiện tối đa khoảng 10^8 thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    pieces = list(map(int, input().split()))
    count0 = count1 = count2 = 0
    for num in pieces:
        r = num % 3
        if r == 0:
            count0 += 1
        elif r == 1:
            count1 += 1
        else:
            count2 += 1
    return str(min(count1, count2) + count0 // 2)

# Provided sample
assert run("3\n123 123 99\n") == "1", "sample 1"

# Minimum input
assert run("1\n3\n") == "0", "single piece cannot form ticket"

# All equal remainder-0
assert run("4\n6 6 6 6\n") == "2", "two pairs of divisible by 3"

# All remainder 1 and 2
assert run("6\n1 2 1 2 1 2\n") == "3", "pair all 1s and 2s"

# Mixed with leftovers
assert run("5\n3 6 1 2 4\n") == "2", "one pair 0s, one pair 1+2"

# Maximum size, all divisible by 3
assert run("10000\n" + " ".join(["3"]*10000) + "\n") == "5000", "largest input all 0s"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n3`| 0 | Một mảnh không thể tạo thành một vé | 
|`4\n6 6 6 6`| 2 | Tất cả các cặp còn lại-0 | 
|`6\n1 2 1 2 1 2`| 3 | Ghép nối phần còn lại-1 và phần còn lại-2 | 
|`5\n3 6 1 2 4`| 2 | Các loại hỗn hợp còn sót lại | 
|`10000\n3 3 ...`| 5000 | Kiểm tra căng thẳng đầu vào lớn | 

## Vỏ cạnh 

Nếu chúng ta có một mảnh duy nhất, nó không thể tạo thành một vé: đầu vào`1\n3`kết quả đầu ra`0`. Thuật toán chính xác để nó không được ghép nối. 

Nếu chúng ta có số lẻ các phần còn lại-0, ví dụ:`3 6 9`,`count0 // 2`đảm bảo chỉ có một vé được hình thành và một vé vẫn chưa được sử dụng. 

Nếu phần dư-1 và phần dư-2 không cân bằng, ví dụ:`1 1 2`, `cặp12
