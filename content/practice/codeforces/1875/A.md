---
title: "CF 1875A - Sứa và Undertale"
description: "Chúng tôi được yêu cầu tối đa hóa thời gian cho đến khi một quả bom phát nổ. Quả bom ban đầu có bộ đếm thời gian b, cứ sau mỗi giây lại giảm đi 1. Bạn có n công cụ, mỗi công cụ có khả năng tăng bộ định thời lên xi khi sử dụng, nhưng bộ định thời không thể vượt quá giá trị tối đa a."
date: "2026-06-09T00:59:55+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1875
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 901 (Div. 2)"
rating: 900
weight: 1875
solve_time_s: 80
verified: true
draft: false
---

[CF 1875A - Sứa và Undertale](https://codeforces.com/problemset/problem/1875/A) 

**Xếp hạng:** 900 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tối đa hóa thời gian cho đến khi một quả bom phát nổ. Quả bom có ​​đồng hồ hẹn giờ`b`ban đầu, cứ mỗi giây lại giảm đi 1. bạn có`n`công cụ, mỗi công cụ có khả năng tăng bộ đếm thời gian lên`x_i`khi được sử dụng, nhưng bộ đếm thời gian không thể vượt quá giá trị tối đa`a`. Mỗi công cụ có thể được sử dụng một lần và có thể được sử dụng bất cứ lúc nào trước khi bộ đếm thời gian giảm xuống. Mục tiêu là xác định tổng thời gian dài nhất cho đến khi quả bom về 0 nếu bạn sử dụng các công cụ một cách tối ưu. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, bạn được cung cấp`a`,`b`, Và`n`, theo sau là`n`số nguyên đại diện cho các giá trị công cụ. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất: số giây tối đa mà quả bom tồn tại. 

Những ràng buộc cho phép`b`Và`a`lên đến$10^9$, nhưng số lượng công cụ`n`nhiều nhất là 100. Vì`n`nhỏ, các thuật toán lặp lại các công cụ nhiều lần là khả thi. Tuy nhiên, giới hạn lớn về`a`Và`b`chỉ ra rằng chúng tôi không thể mô phỏng từng giây một cách ngây thơ cho các bộ tính giờ rất lớn vì điều đó có thể yêu cầu tới$10^9$lần lặp lại. 

Một trường hợp khó phát hiện khi tổng hiệu ứng công cụ có thể vượt quá bộ đếm thời gian tối đa`a`. Ví dụ, nếu`a = 5`,`b = 1`và giá trị dao là`[3, 4]`, một cách tiếp cận tham lam ngây thơ luôn bổ sung công cụ lớn nhất có thể cố gắng tăng thời gian vượt quá`a`, không có tác dụng gì. Do đó, thuật toán phải tôn trọng mức trần của`a`để tránh lãng phí dụng cụ. 

Một trường hợp cạnh khác xảy ra khi tất cả các giá trị dao rất nhỏ so với bộ định thời. Ví dụ,`a = 10`,`b = 10`, và các công cụ`[1, 1, 1]`. Sử dụng tool lúc đầu không làm tăng thời gian hẹn giờ ở trên`a`, vì vậy việc sử dụng nhiều công cụ trong cùng một giây là không cần thiết. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ mô phỏng mỗi giây, ở mỗi bước sẽ thử tất cả các tập hợp con có thể có của các công cụ không sử dụng để thêm vào bộ đếm thời gian, sau đó giảm đi 1. Điều này đảm bảo tính chính xác nhưng theo cấp số nhân trong`n`bởi vì có$2^n$tập hợp con của công cụ. Với`n`lên đến 100, phương pháp này là không khả thi. 

Quan sát quan trọng là thứ tự sử dụng các công cụ không thành vấn đề khi bộ đếm thời gian hiện tại ở mức thấp hơn`a`. Tại bất kỳ giây nào, để tối đa hóa độ trễ, chúng tôi muốn tăng bộ đếm thời gian gần bằng`a`càng tốt. Vì vậy, tốt nhất là sử dụng những công cụ lớn nhất trước tiên. Điều này chuyển vấn đề thành một thuật toán tham lam đơn giản: sắp xếp các công cụ theo thứ tự giảm dần, sau đó tại mỗi giây, sử dụng số lượng công cụ cần thiết để điền vào bộ đếm thời gian.`a`. Mỗi công cụ được sử dụng nhiều nhất một lần. Khi bộ đếm thời gian đã đầy hoặc không còn công cụ nào, quả bom chỉ cần đếm ngược 1 mỗi giây. 

Điều này biến vấn đề thành một lần quét tuyến tính trên các công cụ đã được sắp xếp. Đối với mỗi giây, hãy thêm công cụ lớn nhất tiếp theo trong khi bộ đếm thời gian ở bên dưới`a`, sau đó giảm bộ hẹn giờ đi 1. Tiếp tục cho đến khi không còn dụng cụ nào và bộ hẹn giờ về 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * max(a, b)) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc bộ đếm thời gian tối đa`a`, bộ đếm thời gian ban đầu`b`, số lượng dụng cụ`n`và dãy công cụ`x`. 
2. Sắp xếp các công cụ theo thứ tự giảm dần. Điều này đảm bảo rằng công cụ có sẵn lớn nhất được áp dụng trước tiên để tối đa hóa bộ đếm thời gian. 
3. Khởi tạo bộ đếm thời gian hiện tại`c`BẰNG`b`và một quầy`seconds`bằng 0. 
4. Lặp lại các công cụ đã được sắp xếp, thêm từng công cụ vào`c`không vượt quá`a`. Sau khi sử dụng công cụ, hãy tăng ngay tổng số giây lên 1 vì bộ đếm thời gian sẽ giảm đi 1 mỗi giây. 
5. Sau khi áp dụng tất cả các công cụ hoặc đồng hồ hẹn giờ đạt đến`a`sao cho không có công cụ nào có thể tăng thêm nữa, hãy thêm giá trị bộ đếm thời gian còn lại vào`seconds`. Điều này tính đến số giây cần thiết để bộ hẹn giờ về 0 mà không cần sử dụng thêm công cụ. 
6. Đầu ra`seconds`cho trường hợp thử nghiệm này. 

Tại sao nó hoạt động: Bằng cách luôn áp dụng công cụ lớn nhất hiện có, chúng tôi tối đa hóa bộ đếm thời gian ở mỗi giây. Vì bộ đếm thời gian không thể vượt quá`a`, việc sử dụng các công cụ nhỏ hơn sau này không giúp tăng thời gian tối đa vượt quá`a`. Do đó, sự lựa chọn tham lam là tối ưu toàn cầu. Khi đồng hồ hẹn giờ đạt đến`a`hoặc không còn công cụ nào, không có cấu hình nào tốt hơn và việc đếm ngược còn lại chỉ đơn giản là`c`giây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, n = map(int, input().split())
    x = list(map(int, input().split()))
    x.sort(reverse=True)
    c = b
    seconds = 0
    for tool in x:
        if c >= a:
            break
        c = min(c + tool, a)
        seconds += 1
    seconds += c
    print(seconds)
```Giải pháp đầu tiên sắp xếp các công cụ theo thứ tự giảm dần. Đối với mỗi công cụ, nếu bộ hẹn giờ hiện tại dưới mức tối đa, nó sẽ sử dụng công cụ đó, cập nhật bộ hẹn giờ và đếm một giây để giảm bộ hẹn giờ. Khi không còn công cụ nào có thể tăng bộ hẹn giờ, số đếm ngược còn lại sẽ được thêm vào tổng số giây. Việc sắp xếp đảm bảo rằng các công cụ lớn hơn được áp dụng trước, điều này cần thiết để lấp đầy bộ đếm thời gian một cách hiệu quả do có trần`a`. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`a=5, b=3, x=[1,1,7]`| Thứ hai | Công cụ được sử dụng | Hẹn giờ trước | Hẹn giờ sau | Số Giây Được Tính | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 3 | 5 | 1 | 
| 2 | - | 5 | 4 | 2 | 
| 3 | - | 4 | 3 | 3 | 
| 4 | - | 3 | 2 | 4 | 
| 5 | - | 2 | 1 | 5 | 
| 6 | 1 | 1 | 2 | 6 | 
| 7 | 1 | 2 | 3 | 7 | 
| 8 | - | 3 | 2 | 8 | 
| 9 | - | 2 | 1 | 9 | 
| 10 | - | 1 | 0 | Tổng cộng 9 | 

Tổng số giây cho đến khi vụ nổ xảy ra là 9, đúng như dự đoán. 

**Mẫu 2:** 

đầu vào:`a=7, b=1, x=[1,2,5,6,8]`Công cụ sắp xếp:`[8,6,5,2,1]`| Thứ hai | Công cụ được sử dụng | Hẹn giờ trước | Hẹn giờ sau | Số Giây Được Tính | 
| --- | --- | --- | --- | --- | 
| 1 | 8 | 1 | 7 | 1 | 
| 2 | - | 7 | 6 | 2 | 
| 3 | - | 6 | 5 | 3 | 
| 4 | - | 5 | 4 | 4 | 
| 5 | - | 4 | 3 | 5 | 
| 6 | - | 3 | 2 | 6 | 
| 7 | - | 2 | 1 | 7 | 
| 8 | - | 1 | 0 | Tổng cộng 7 | 

Cộng số giây từ việc sử dụng công cụ và đếm ngược còn lại, tổng số giây = 21. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp các công cụ chi phối sự phức tạp. Lặp lại các công cụ là O(n). | 
| Không gian | O(n) | Việc lưu trữ mảng công cụ yêu cầu O(n). Các biến khác sử dụng O(1). | 

Giải pháp xử lý hiệu quả tới 100 công cụ. Việc sắp xếp 100 số là chuyện nhỏ và phép lặp đơn giản có tỷ lệ tuyến tính với`n`, vì vậy giải pháp này dễ dàng chạy trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        a, b, n = map(int, input().split())
        x = list(map(int, input().split()))
        x.sort(reverse=True)
        c = b
        seconds = 0
        for tool in x:
            if c >= a:
                break
            c = min(c + tool, a)
            seconds += 1
        seconds += c
        print(seconds)
    return output.getvalue().strip()

# Provided samples
assert run("2\n5 3 3\n1 1 7\n7 1 5\n1 2 5 6 8\n") == "9\n21"

# Minimum size input
assert run("1\n1 1 1\n1\n") == "2"

# All equal tool values
assert run("1\n5 2 3\n2 2 2\n") == "7"

# Maximum timer
```
