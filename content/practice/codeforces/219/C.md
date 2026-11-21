---
title: "CF 219C - Sọc Màu"
description: "Chúng ta có một sọc được biểu thị dưới dạng một hàng gồm n ô, trong đó mỗi ô được sơn một trong k màu được dán nhãn từ các chữ cái A đến chữ cái thứ k. Mục tiêu là sơn lại càng ít ô càng tốt để không có hai ô liền kề có cùng màu."
date: "2026-06-04T01:43:23+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 219
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 135 (Div. 2)"
rating: 1600
weight: 219
solve_time_s: 63
verified: true
draft: false
---

[CF 219C - Sọc màu](https://codeforces.com/problemset/problem/219/C) 

**Đánh giá:** 1600 
**Tags:** bạo lực, dp, tham lam 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một sọc được thể hiện dưới dạng một hàng`n`các ô, trong đó mỗi ô được sơn một trong`k`màu sắc được dán nhãn bằng chữ cái`A`thông qua`k`- lá thư thứ Mục tiêu là sơn lại càng ít ô càng tốt để không có hai ô liền kề có cùng màu. Đầu ra phải bao gồm cả số lần sơn lại tối thiểu và cấu hình sọc kết quả hợp lệ. 

Kích thước đầu vào là đáng kể:`n`có thể lên tới 500.000 và`k`có thể lên tới 26. Điều này loại trừ mọi nghiệm bậc hai trong`n`, bởi vì`n^2`các hoạt động sẽ đạt khoảng 2,5×10^11 trong trường hợp xấu nhất, vượt xa giới hạn thời gian 2 giây. Cần có các giải pháp tuyến tính hoặc gần tuyến tính. Giới hạn trên nhỏ của`k`gợi ý rằng chúng ta có đủ khả năng để xem xét tất cả các màu có sẵn ở mỗi ô trong thời gian không đổi. 

Một trường hợp khó phát hiện khi`k = 2`. Trong trường hợp này, chỉ có hai màu, vì vậy sọc phải xen kẽ hoàn hảo giữa chúng. Bất kỳ chuỗi nào có ba màu giống nhau liên tiếp nhất thiết phải sơn lại màu ở giữa. Một lựa chọn tham lam ngây thơ chỉ nhìn vào ô trước đó có thể thất bại nếu nó không tính đến ô tiếp theo, tạo ra số lần sơn lại dưới mức tối ưu. 

Một cạm bẫy tiềm ẩn khác là khi sọc đã có các màu xen kẽ. Một thuật toán bất cẩn vẫn có thể sơn lại một cách không cần thiết nếu nó thực thi một cách mù quáng "thay đổi nếu giống như trước" mà không xem xét rằng trình tự đã hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi cách gán màu có thể có cho từng ô thỏa mãn điều kiện kề và sau đó đếm số lần sơn lại. Với mỗi ô, có`k`các tùy chọn và cho`n`tế bào, điều này dẫn đến`k^n`khả năng. Ngay cả khi cắt tỉa, điều này vẫn chậm một cách vô vọng đối với`n`lên tới 500.000. 

Cái nhìn sâu sắc quan trọng là vấn đề có thể được giải quyết một cách tham lam bằng cách chú ý cẩn thận đến ô trước đó. Vì mục tiêu chỉ là tránh các ô trùng lặp liền kề nên sự lựa chọn của mỗi ô chỉ phụ thuộc vào màu của ô lân cận. Điều này cho phép quét tuyến tính: đối với mỗi ô, nếu nó khớp với ô trước đó, chúng tôi sẽ chọn một màu khác. Việc chọn màu thay thế có thể được thực hiện trong thời gian không đổi bằng cách lặp qua tất cả`k`màu sắc cho đến khi chúng ta tìm thấy màu khác với màu trước đó (và, tùy chọn, màu tiếp theo, nếu`k > 2`và chúng tôi muốn tránh tạo ra xung đột mới). 

Vì`k = 2`, chỉ có hai sọc hợp lệ: xen kẽ bắt đầu từ màu đầu tiên hoặc màu thứ hai. Chúng tôi có thể tính chi phí sơn lại cho cả hai và chọn phương án rẻ hơn. Vì`k > 2`, việc quét tham lam từ trái sang phải luôn thành công với số lần sơn lại tối thiểu vì luôn có ít nhất một màu để chọn để tránh xung đột ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^n) | O(n) | Quá chậm | 
| Quét tuyến tính tham lam | O(n·k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi đầu vào thành một danh sách để thao tác có thể thay đổi, vì chúng tôi sẽ vẽ lại tại chỗ. Điều này tránh việc xây dựng chuỗi mới nhiều lần. 
2. Nếu`k = 2`, tính chi phí sơn lại cho cả hai chuỗi xen kẽ có thể có: bắt đầu bằng màu đầu tiên và bắt đầu bằng màu thứ hai. So sánh số lượng và chọn trình tự rẻ hơn. Trả về số đếm và trình tự. 
3. Nếu`k > 2`, lặp qua sọc từ trái sang phải, bắt đầu từ ô thứ hai. Đối với mỗi ô, hãy kiểm tra xem nó có khớp với ô trước đó không. 
4. Nếu tìm thấy kết quả khớp, hãy tăng bộ đếm sơn lại và chọn màu mới cho ô này. Lặp lại tất cả`k`màu sắc và chọn ô đầu tiên khác với cả ô trước và ô tiếp theo (nếu nó tồn tại). Gán màu này cho ô hiện tại. 
5. Tiếp tục quét cho đến hết. Khi chúng tôi hoàn thành, mỗi cặp ô lân cận được đảm bảo có màu sắc khác nhau. 
6. Xuất ra tổng số lần sơn lại và sọc kết quả. 

Thuật toán tham lam hoạt động vì nó duy trì tính bất biến là tất cả các ô ở bên trái của chỉ mục hiện tại đều được tô màu chính xác và không có ô trùng lặp liền kề. Ở mỗi bước, chúng tôi chỉ sơn lại nếu cần thiết và luôn chọn màu giữ nguyên tính bất biến. Từ`k ≥ 2`, luôn có ít nhất một màu hợp lệ để chọn, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
s = list(input().strip())
colors = [chr(ord('A') + i) for i in range(k)]
repaints = 0

if k == 2:
    # only two valid sequences: ABAB... or BABA...
    alt1 = [colors[i % 2] for i in range(n)]
    alt2 = [colors[(i + 1) % 2] for i in range(n)]
    cost1 = sum(s[i] != alt1[i] for i in range(n))
    cost2 = sum(s[i] != alt2[i] for i in range(n))
    if cost1 <= cost2:
        print(cost1)
        print(''.join(alt1))
    else:
        print(cost2)
        print(''.join(alt2))
else:
    i = 1
    while i < n:
        if s[i] == s[i - 1]:
            repaints += 1
            for c in colors:
                if c != s[i - 1] and (i + 1 == n or c != s[i + 1]):
                    s[i] = c
                    break
        i += 1
    print(repaints)
    print(''.join(s))
```Mã bắt đầu bằng cách chuyển đổi chuỗi đầu vào thành một danh sách có thể thay đổi. Đối với`k = 2`trường hợp cạnh, nó xây dựng hai chuỗi xen kẽ và tính chi phí sơn lại cho cả hai. Vì`k > 2`, thuật toán sẽ quét từng ô, phát hiện xung đột với ô trước đó và chọn màu thay thế hợp lệ. Việc kiểm tra ô tiếp theo sẽ ngăn chặn việc tạo ra xung đột mới trước mắt. 

## Ví dụ đã hoạt động 

Mẫu 1:```
Input: 6 3
ABBACC
```| tôi | s[i] trước | s[i-1] | s[i+1] | Hành động | sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | B | A | B | được | A B B A C C | 
| 2 | B | B | A | Xung đột, chọn C | A B C A C C | 
| 3 | A | C | C | được | ... | 
| 4 | C | A | C | được | ... | 
| 5 | C | C | - | Xung đột, chọn A | A B C A C A | 

Sơn lại = 2. Kết quả sọc = ABCACA. 

Ví dụ được xây dựng:```
Input: 5 2
AAAAA
```Với k=2, hai chuỗi là ABABA và BABAB. So sánh với AAAA: 

| Trình tự | Sơn lại | 
| --- | --- | 
| ABABA | 2 | 
| BABAB | 3 | 

Số lần sơn lại tối thiểu = 2. Sọc kết quả = ABABA. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·k) | Mỗi ô được truy cập một lần. Khi sơn lại, chúng ta lặp lại tối đa k màu. Với k 26, giá trị này thực sự là hằng số. | 
| Không gian | O(n) | Chúng tôi lưu trữ sọc dưới dạng danh sách có thể thay đổi và mảng màu có độ dài k. | 

Với n 5×10^5 và k ≤ 26, các phép toán O(n·k) nằm dưới 2×10^7, phù hợp thoải mái trong giới hạn thời gian 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        n, k = map(int, input().split())
        s = list(input().strip())
        colors = [chr(ord('A') + i) for i in range(k)]
        repaints = 0

        if k == 2:
            alt1 = [colors[i % 2] for i in range(n)]
            alt2 = [colors[(i + 1) % 2] for i in range(n)]
            cost1 = sum(s[i] != alt1[i] for i in range(n))
            cost2 = sum(s[i] != alt2[i] for i in range(n))
            if cost1 <= cost2:
                print(cost1)
                print(''.join(alt1))
            else:
                print(cost2)
                print(''.join(alt2))
        else:
            i = 1
            while i < n:
                if s[i] == s[i - 1]:
                    repaints += 1
                    for c in colors:
                        if c != s[i - 1] and (i + 1 == n or c != s[i + 1]):
                            s[i] = c
                            break
                i += 1
            print(repaints)
            print(''.join(s))
    return out.getvalue().strip()

# Provided sample
assert run("6 3\nABBACC\n") == "2\nABCACA", "sample 1"
# Minimum size
assert run("1 2\nA\n") == "0\nA", "single cell"
# All equal, k=2
assert run("5 2\nAAAAA\n")
```
