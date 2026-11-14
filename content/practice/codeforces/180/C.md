---
title: "CF 180C - Thư"
description: "Chúng ta được cho một chuỗi bao gồm chữ hoa và chữ thường. Patrick muốn chuyển đổi nó thành một chuỗi \"fancy\", được định nghĩa là có tất cả các chữ cái viết hoa ở bên trái và tất cả các chữ cái viết thường ở bên phải. Chúng ta có thể thay đổi kiểu chữ của bất kỳ chữ cái nào bằng một hành động."
date: "2026-06-03T00:46:55+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 180
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 116 (Div. 2, ACM-ICPC Rules)"
rating: 1400
weight: 180
solve_time_s: 72
verified: true
draft: false
---

[CF 180C - Thư](https://codeforces.com/problemset/problem/180/C) 

**Đánh giá:** 1400 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi bao gồm chữ hoa và chữ thường. Patrick muốn biến nó thành một chuỗi "ưa thích", được định nghĩa là có tất cả các chữ cái viết hoa ở bên trái và tất cả các chữ cái viết thường ở bên phải. Chúng ta có thể thay đổi kiểu chữ của bất kỳ chữ cái nào bằng một hành động. Mục tiêu của chúng tôi là tính toán số lượng hành động tối thiểu cần thiết để đạt được định dạng ưa thích. 

Chuỗi đầu vào có thể dài tới 100.000 ký tự. Điều này ngay lập tức loại trừ các giải pháp kiểm tra mọi phân vùng có thể có và tính toán số lượng thay đổi từ đầu cho từng phân vùng theo thời gian tuyến tính, vì đó sẽ là O(n²) và quá chậm. Chúng ta cần một giải pháp quét chuỗi nhiều nhất với số lần không đổi, lý tưởng là O(n). Chuỗi được đảm bảo không trống, vì vậy chúng tôi không cần xử lý trường hợp đầu vào trống, nhưng chúng tôi cần xử lý các chuỗi đã có tất cả chữ hoa, chữ thường hoặc chữ cái xen kẽ. 

Trường hợp cạnh không rõ ràng phát sinh khi chuỗi đã được ưa thích. Ví dụ, đầu vào`ABCdef`yêu cầu 0 thay đổi và cách tiếp cận ngây thơ luôn đếm cả hai bên có thể tạo nhầm số dương. Một kịch bản phức tạp khác là`aAaA`, trong đó chữ hoa và chữ thường được xen kẽ hoàn toàn. Thuật toán phải tính toán chính xác điểm phân tách tối ưu, có thể nằm ở giữa chuỗi và thay đổi các chữ cái để khớp với điểm phân tách đó. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force lặp lại trên mọi phân chia có thể có của chuỗi, từ 0 đến n, coi bên trái là tất cả chữ hoa và bên phải là tất cả chữ thường. Với mỗi lần chia, chúng ta đếm số chữ cái viết thường ở đoạn bên trái và số chữ cái viết hoa ở đoạn bên phải. Tổng hợp những điều này sẽ cho số lượng hành động cần thiết cho sự phân chia đó. Sau đó chúng tôi lấy mức tối thiểu trên tất cả các phần tách. Mặc dù điều này đơn giản về mặt khái niệm, nhưng chi phí sẽ là O(n2) trong trường hợp xấu nhất vì đối với mỗi lần chia n, việc đếm các chữ cái ở cả hai bên đều yêu cầu O(n). Điều này là quá chậm đối với n lên tới 100.000. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa là vấn đề có thể được giảm xuống thành chiến lược đếm tiền tố/hậu tố. Chúng ta có thể tính toán trước cho mỗi chỉ mục số lượng chữ cái viết thường tích lũy cho đến vị trí đó và số lượng chữ cái viết hoa tích lũy từ vị trí đó đến cuối. Sau đó, đối với bất kỳ điểm phân chia nào, chúng ta có thể tính số lượng thay đổi cần thiết trong O(1) bằng cách sử dụng các số tích lũy này. Điều này làm giảm tổng độ phức tạp xuống còn O(n) thời gian và không gian O(n). Về mặt khái niệm, chúng tôi đang quét một lần từ trái sang phải để xây dựng số lượng tiền tố và một lần từ phải sang trái để đếm số lượng hậu tố, sau đó đánh giá tất cả các điểm phân tách trong một lần tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Số tiền tố/hậu tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mảng tiền tố`lower_prefix`Ở đâu`lower_prefix[i]`đếm số chữ cái viết thường trong`s[0..i-1]`. Điều này thể hiện số lượng thay đổi cần thiết để chuyển đoạn bên trái thành toàn chữ hoa nếu chúng ta chia tách sau`i-1`. 
2. Tính mảng hậu tố`upper_suffix`Ở đâu`upper_suffix[i]`đếm số chữ in hoa trong`s[i..n-1]`. Điều này thể hiện số lượng thay đổi cần thiết để chuyển đổi đoạn bên phải thành tất cả chữ thường nếu chúng ta chia trước`i`. 
3. Khởi tạo một biến`min_actions`với số lượng lớn. Lặp lại tất cả các điểm phân chia`i`từ 0 đến n. Với mỗi lần chia, số hành động cần thực hiện là`lower_prefix[i] + upper_suffix[i]`. Cập nhật`min_actions`đến mức tối thiểu trên tất cả các phần tách. 
4. Đầu ra`min_actions`. 

Tại sao nó hoạt động: Tại mỗi điểm phân chia, mảng tiền tố cho chúng ta biết chính xác có bao nhiêu chữ cái viết thường cần được chuyển đổi thành chữ hoa ở bên trái và mảng hậu tố cho chúng ta biết có bao nhiêu chữ cái viết hoa cần được chuyển đổi thành chữ thường ở bên phải. Việc đánh giá tất cả các phần tách đảm bảo chúng tôi tìm thấy điểm tối ưu toàn cục vì bất kỳ chuỗi ưa thích nào cũng có thể được biểu diễn dưới dạng một số phần tách giữa chữ hoa và chữ thường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

lower_prefix = [0] * (n + 1)
for i in range(n):
    lower_prefix[i + 1] = lower_prefix[i] + (1 if s[i].islower() else 0)

upper_suffix = [0] * (n + 1)
for i in range(n - 1, -1, -1):
    upper_suffix[i] = upper_suffix[i + 1] + (1 if s[i].isupper() else 0)

min_actions = n  # maximum possible actions is n
for i in range(n + 1):
    actions = lower_prefix[i] + upper_suffix[i]
    if actions < min_actions:
        min_actions = actions

print(min_actions)
```Mảng tiền tố và hậu tố lưu trữ số lượng tích lũy để tránh việc quét chuỗi lặp lại. Vòng lặp tính toán`min_actions`đánh giá tất cả các điểm phân tách có thể có, bao gồm cả trước ký tự đầu tiên và sau ký tự cuối cùng, xử lý chính xác các chuỗi vốn đã toàn là chữ hoa hoặc toàn chữ thường. 

## Ví dụ đã hoạt động 

Đối với đầu vào`PRuvetSTAaYA`: 

| tôi | s[i] | low_prefix[i+1] | Upper_suffix[i] | Hành động = low_prefix + Upper_suffix | 
| --- | --- | --- | --- | --- | 
| 0 | P | 0 | 7 | 0 + 7 = 7 | 
| 1 | R | 0 | 7 | 0 + 7 = 7 | 
| 2 | bạn | 1 | 7 | 1 + 7 = 8 | 
| 3 | v | 2 | 6 | 2 + 6 = 8 | 
| 4 | e | 3 | 6 | 3 + 6 = 9 | 
| 5 | t | 4 | 6 | 4 + 6 = 10 | 
| 6 | S | 4 | 5 | 4 + 5 = 9 | 
| 7 | T | 4 | 4 | 4 + 4 = 8 | 
| 8 | A | 4 | 3 | 4 + 3 = 7 | 
| 9 | một | 5 | 3 | 5 + 3 = 8 | 
| 10 | Y | 5 | 2 | 5 + 2 = 7 | 
| 11 | A | 5 | 1 | 5 + 1 = 6 | 
| 12 | | 5 | 0 | 5 + 0 = 5 | 

Các hành động tối thiểu là 5, phù hợp với kết quả mong đợi. 

Đối với một chuỗi đã ưa thích`ABCdef`: 

| tôi | low_prefix[i+1] | Upper_suffix[i] | Hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 3 | 3 | 
| 1 | 0 | 3 | 3 | 
| 2 | 0 | 3 | 3 | 
| 3 | 0 | 3 | 3 | 
| 4 | 0 | 2 | 2 | 
| 5 | 0 | 1 | 1 | 
| 6 | 0 | 0 | 0 | 

Tối thiểu là 0, xác định chính xác không cần thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Hai lượt để xây dựng mảng tiền tố và hậu tố, một lượt để tính toán các hành động tối thiểu | 
| Không gian | O(n) | Hai mảng có kích thước n+1 để lưu trữ số lượng tích lũy | 

Cho n ≤ 10^5, giải pháp O(n) dễ dàng thực thi trong vòng 1 giây. Mức sử dụng bộ nhớ thấp hơn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    n = len(s)

    lower_prefix = [0] * (n + 1)
    for i in range(n):
        lower_prefix[i + 1] = lower_prefix[i] + (1 if s[i].islower() else 0)

    upper_suffix = [0] * (n + 1)
    for i in range(
```
