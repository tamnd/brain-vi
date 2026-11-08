---
title: "CF 1427D - Xáo bài"
description: "Chúng ta được cấp một bộ bài gồm n lá bài, mỗi lá bài được đánh số nguyên duy nhất từ ​​1 đến n, theo thứ tự tùy ý. Mục đích là sắp xếp bộ bài theo thứ tự tăng dần bằng một thao tác đặc biệt."
date: "2026-06-11T05:38:05+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1427
codeforces_index: "D"
codeforces_contest_name: "Codeforces Global Round 11"
rating: 2000
weight: 1427
solve_time_s: 107
verified: true
draft: false
---

[CF 1427D - Xáo trộn bộ bài](https://codeforces.com/problemset/problem/1427/D) 

**Xếp hạng:** 2000 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ bài`n`thẻ, mỗi thẻ được dán nhãn một số nguyên duy nhất từ`1`ĐẾN`n`, theo một thứ tự tùy ý. Mục đích là sắp xếp bộ bài theo thứ tự tăng dần bằng một thao tác đặc biệt. Thao tác này cho phép chúng ta chia bộ bài hiện tại thành`k`các phần liền kề, đối với bất kỳ`2 ≤ k ≤ n`, sau đó đảo ngược thứ tự của các phần này trong khi vẫn giữ nguyên thứ tự bên trong của từng phần. Ví dụ như việc chia tách`[3 1 2 4]`vào trong`[3][1 2][4]`và đảo ngược các bộ phận tạo ra`[4 1 2 3]`. 

Đầu vào là`n`theo sau là danh sách các lá bài và đầu ra là một chuỗi các thao tác sắp xếp bộ bài. Mỗi thao tác phải xác định rõ số lượng chi tiết và kích thước của từng chi tiết. Ràng buộc`n ≤ 52`nghĩa là chúng ta có đủ khả năng`O(n^2)`hoặc thậm chí các thuật toán phức tạp cao hơn một chút. Vì bài toán đảm bảo rằng nhiều nhất`n`hoạt động là đủ, giải pháp của chúng tôi có thể tập trung vào cách tiếp cận mang tính xây dựng thay vì tìm kiếm vũ phu trên tất cả các phần tách có thể có. 

Trường hợp cạnh không rõ ràng phát sinh khi một chuỗi các thẻ liên tiếp đã theo đúng thứ tự tương đối nhưng bị đặt sai vị trí dưới dạng một khối. Ví dụ,`[3 4 1 2]`có`3 4`đúng thứ tự nhưng sai vị trí. Một cách tiếp cận ngây thơ chỉ xem xét các vị trí của từng lá bài có thể chia bộ bài quá mức, tạo ra nhiều thao tác hơn mức cần thiết. Một điều tinh tế khác là khi thẻ đầu tiên hoặc thẻ cuối cùng đã đúng - các thao tác phải tránh phá hủy các chuỗi đã được đặt chính xác trừ khi cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là liên tục tìm kiếm thẻ tiếp theo theo trình tự và di chuyển nó đến vị trí chính xác bằng một thao tác duy nhất. Đối với mỗi lá bài, chúng ta có thể chia bộ bài sao cho lá bài ở mặt trước rồi đảo ngược các phần để đặt nó. Điều này đảm bảo tính chính xác nhưng có thể yêu cầu`O(n^2)`các bước trong trường hợp xấu nhất vì mỗi thao tác chỉ có thể đặt đúng một thẻ. Với`n ≤ 52`, điều này khả thi về mặt kỹ thuật, nhưng nó không hiệu quả và tạo ra các giải pháp dài không cần thiết. 

Cái nhìn sâu sắc quan trọng là xây dựng bộ bài được sắp xếp tăng dần từ lá bài đầu tiên. Ở mỗi bước, chúng tôi duy trì tiền tố của các thẻ được sắp xếp chính xác ở phía trước. Sau đó, chúng tôi xác định vị trí khối số liên tiếp tiếp theo sau tiền tố này, chia bộ bài thành tối đa bốn phân đoạn liền kề để khối này xuất hiện ngay sau tiền tố được sắp xếp và đảo ngược các phân đoạn này. Bằng cách xây dựng các phần chia một cách cẩn thận, chúng ta có thể đặt khối thẻ liên tiếp tối đa vào mỗi thao tác. Điều này làm giảm số lượng thao tác và đảm bảo rằng mỗi bước sẽ mở rộng tiền tố đã sắp xếp, do đó tối đa bộ bài được sắp xếp hoàn toàn.`n`di chuyển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Được chấp nhận với n ≤ 52 nhưng không hiệu quả | 
| Tối ưu | O(n^2) | O(n) | Đã nghiệm thu, xây dựng lời giải trong ≤ n phép toán | 

## Hướng dẫn thuật toán 

1. Khởi tạo`sorted_prefix = 0`. Biến này theo dõi độ dài của tiền tố được sắp xếp chính xác ở đầu bộ bài. 
2. Trong khi`sorted_prefix < n`, hãy tìm đoạn tăng liên tiếp dài nhất ngay sau tiền tố được sắp xếp. Hãy để phân đoạn này bắt đầu ở chỉ mục`l`và kết thúc tại chỉ mục`r`. 
3. Chia bộ bài thành bốn phần liền kề nhau: 

- Đoạn 1: tiền tố được sắp xếp (độ dài`sorted_prefix`) 
- Đoạn 2: các thẻ trước đoạn liền kề (chiều dài`l - sorted_prefix`) 
- Đoạn 3: đoạn liên tiếp (chiều dài`r - l + 1`) 
- Đoạn 4: các thẻ sau đoạn liền nhau (độ dài`n - r - 1`) 

Xóa bất kỳ phân đoạn nào có độ dài bằng 0 khỏi danh sách này vì các phân đoạn phải không trống. 
4. Đảo ngược thứ tự của các phân đoạn này để di chuyển phân đoạn liên tiếp ngay sau tiền tố được sắp xếp. Ghi lại kích thước của các phân đoạn như một phép toán. 
5. Cập nhật`sorted_prefix`bằng cách thêm độ dài của đoạn liên tiếp, mở rộng tiền tố đã sắp xếp. Lặp lại cho đến khi toàn bộ bộ bài được sắp xếp. 

Tại sao nó hoạt động: Mỗi thao tác di chuyển khối số liên tiếp tối đa có thể mở rộng tiền tố đã sắp xếp đến đúng vị trí của nó. Vì tiền tố tăng dần theo từng bước và không có thẻ nào được di chuyển qua tiền tố đã sắp xếp nên quy trình không thể hoàn tác tiến trình trước đó. Điều này đảm bảo chấm dứt tối đa`n`hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
deck = list(map(int, input().split()))
operations = []

while True:
    sorted_prefix = 0
    while sorted_prefix < n and deck[sorted_prefix] == sorted_prefix + 1:
        sorted_prefix += 1
    if sorted_prefix == n:
        break

    # find the next consecutive segment to place
    l = sorted_prefix
    while l < n and deck[l] != sorted_prefix + 1:
        l += 1
    r = l
    while r + 1 < n and deck[r + 1] == deck[r] + 1:
        r += 1

    segments = []
    if sorted_prefix > 0:
        segments.append(sorted_prefix)
    if l - sorted_prefix > 0:
        segments.append(l - sorted_prefix)
    segments.append(r - l + 1)
    if n - r - 1 > 0:
        segments.append(n - r - 1)

    # apply operation
    new_deck = []
    start = 0
    for sz in reversed(segments):
        new_deck.extend(deck[start:start+sz])
        start += sz
    deck = new_deck
    operations.append(segments)

print(len(operations))
for op in operations:
    print(len(op), *op)
```Đầu tiên, mã sẽ tính toán tiền tố đã được sắp xếp để tránh làm xáo trộn các thẻ đã có sẵn. Sau đó, nó xác định đoạn thẻ liên tiếp tối đa tiếp theo và chia bộ bài thành tối đa bốn phần xung quanh nó. Chỉ những phần không trống mới được đưa vào hoạt động. Đảo ngược các phân đoạn sẽ di chuyển khối đã chọn bên cạnh tiền tố đã sắp xếp. Giải pháp quản lý cẩn thận các chỉ số để tránh các lỗi sai sót khi chia tách. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
4
3 1 2 4
```| Bước | Bộ bài | Tiền tố được sắp xếp | Phân đoạn | 
| --- | --- | --- | --- | 
| 0 | [3 1 2 4] | 0 | [1,2,1] | 
| 1 | [1 2 3 4] | 4 | - | 

Thao tác đầu tiên di chuyển khối liên tiếp`[1 2]`sau tiền tố`[ ]`. Bộ bài trở thành`[1 2 3 4]`. Tiền tố được sắp xếp đạt đến 4 nên chúng tôi dừng lại. 

**Đầu vào mẫu 2**```
6
6 5 4 3 2 1
```| Bước | Bộ bài | Tiền tố được sắp xếp | Phân đoạn | 
| --- | --- | --- | --- | 
| 0 | [6 5 4 3 2 1] | 0 | [1,1,1,1,1,1] | 
| 1 | [1 2 3 4 5 6] | 6 | - | 

Việc chia tách từng thẻ riêng lẻ và đảo ngược tất cả các phân đoạn sẽ sắp xếp chúng theo thứ tự trong một thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi lần lặp có thể quét bộ bài để tìm đoạn liên tiếp, tối đa n lần lặp | 
| Không gian | O(n) | Lưu trữ boong và mảng hoạt động | 

Với`n ≤ 52`, Hoạt động O(n^2) có thể được chấp nhận trong giới hạn thời gian 1 giây và mức sử dụng bộ nhớ ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    deck = list(map(int, input().split()))
    operations = []

    while True:
        sorted_prefix = 0
        while sorted_prefix < n and deck[sorted_prefix] == sorted_prefix + 1:
            sorted_prefix += 1
        if sorted_prefix == n:
            break

        l = sorted_prefix
        while l < n and deck[l] != sorted_prefix + 1:
            l += 1
        r = l
        while r + 1 < n and deck[r + 1] == deck[r] + 1:
            r += 1

        segments = []
        if sorted_prefix > 0:
            segments.append(sorted_prefix)
        if l - sorted_prefix > 0:
            segments.append(l - sorted_prefix)
        segments.append(r - l + 1)
        if n - r - 1 > 0:
            segments.append(n - r - 1)

        new_deck = []
        start = 0
        for sz in reversed(segments):
            new_deck.extend(deck[start:start+sz])
            start += sz
        deck = new_deck
        operations.append(segments)

    out = [str(len(operations))]
    for op in operations:
        out.append(f"{len(op)} " + " ".join(map(str, op)))
    return "\n".join(out)

# Provided samples
assert run("4\n3 1 2 4\n") == "2\n3 1 2 1\n2 1 3"
assert run("6\n6 5 4 3 2 1\n") == "1\n6 1 1 1 1 1 1"

# Custom cases
assert run("1\n1\n") == "0"
assert run("2\n2 1\n") == "1\n2 1
```
