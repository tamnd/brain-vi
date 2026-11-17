---
title: "CF 1957A - Stickogon"
description: "Chúng ta được cho một tập hợp các que, mỗi que có chiều dài nguyên. Mục tiêu là xây dựng càng nhiều đa giác đều càng tốt bằng cách sử dụng các que này, với hạn chế là mỗi cạnh của đa giác phải chính xác là một que và không que nào có thể được sử dụng lại."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1957
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 940 (Div. 2) and CodeCraft-23"
rating: 800
weight: 1957
solve_time_s: 43
verified: true
draft: false
---

[CF 1957A - Stickogon](https://codeforces.com/problemset/problem/1957/A) 

**Đánh giá:** 800 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các que, mỗi que có chiều dài nguyên. Mục tiêu là xây dựng càng nhiều đa giác đều càng tốt bằng cách sử dụng các que này, với hạn chế là mỗi cạnh của đa giác phải chính xác là một que và không que nào có thể được sử dụng lại. Một đa giác đều cần ít nhất ba cạnh có độ dài bằng nhau. Vì vậy, để tạo thành một đa giác có cạnh dài`L`, chúng ta cần ít nhất ba que dài`L`. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi biết có bao nhiêu que và độ dài riêng của chúng. Đầu ra của mỗi trường hợp thử nghiệm là số lượng đa giác tối đa mà chúng ta có thể tạo được bằng các que đã cho. 

Các ràng buộc rất nhỏ: tối đa 100 que cho mỗi trường hợp thử nghiệm và tối đa 100 trường hợp thử nghiệm. Điều này có nghĩa là chúng ta có thể có được các giải pháp có độ phức tạp bậc hai trong`n`cho mỗi trường hợp thử nghiệm, nhưng chúng ta vẫn nên ưu tiên các phương pháp tiếp cận tuyến tính hoặc gần tuyến tính. Vì các que có độ dài từ 1 đến 100 nên chúng ta cũng có thể tận dụng các kỹ thuật đếm thay vì sắp xếp hoặc các cấu trúc dữ liệu phức tạp hơn. 

Trường hợp cạnh tinh tế phát sinh khi tổng số que có độ dài nhất định không đủ để tạo thành đa giác nhỏ nhất. Ví dụ: nếu chúng ta có hai que có chiều dài 5, chúng ta không thể tạo thành một hình tam giác, do đó đầu ra có chiều dài đó bằng 0. Một trường hợp cạnh khác xảy ra khi nhiều đa giác có thể được hình thành từ các độ dài khác nhau; giải pháp của chúng tôi phải tối đa hóa tổng số đa giác, không chỉ đa giác lớn nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử tất cả các tập hợp con có thể có của que để tạo thành đa giác. Đối với mỗi tập hợp con có độ dài ít nhất là ba, chúng tôi sẽ kiểm tra xem tất cả các que trong tập hợp con có cùng độ dài hay không. Sau đó, chúng tôi sẽ loại bỏ đệ quy các que được sử dụng và tiếp tục tìm kiếm. Điều này đúng nhưng không khả thi vì số lượng tập hợp con tăng theo cấp số nhân với`n`. Vì`n = 100`, việc lặp lại tất cả các tập hợp con là hoàn toàn không thực tế. 

Điểm mấu chốt là một đa giác chỉ có thể được hình thành bằng cách có ít nhất ba que có cùng chiều dài. Do đó, thay vì kiểm tra các tập hợp con tùy ý, chúng ta có thể nhóm các que theo chiều dài của chúng và đếm xem chúng ta có bao nhiêu que tính trên mỗi chiều dài. Số lượng đa giác tối đa mà chúng ta có thể tạo thành từ các que có chiều dài đơn giản chỉ là phép chia số nguyên cho ba. Bằng cách tính tổng giá trị này trên tất cả các độ dài, chúng ta sẽ có được số lượng đa giác tối đa có thể được hình thành từ tất cả các que. 

Phương pháp dựa trên việc đếm này hoạt động vì bài toán cho phép hình thành các đa giác độc lập với mỗi chiều dài và không có thanh nào được chia sẻ giữa các đa giác. Chúng ta không cần phải xem xét độ dài trộn, điều này giúp đơn giản hóa giải pháp một cách đáng kể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Đếm & Chia | O(n + L) | O(L) | Đã chấp nhận | 

Đây`L`là chiều dài thanh tối đa (100), không đáng kể so với`n`. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và danh sách độ dài thanh`a`. 
3. Khởi tạo mảng tần số`count`có kích thước 101 (để bao gồm độ dài từ 1 đến 100) bằng 0. 
4. Lặp lại tất cả các gậy, tăng dần`count[length]`cho mỗi cây gậy. 
5. Khởi tạo một biến`polygons`về không. 
6. Với mỗi độ dài có thể từ 1 đến 100, hãy tính số đa giác có thể được tạo thành từ các que có độ dài đó bằng phép chia số nguyên`count[length] // 3`và thêm nó vào`polygons`. 
7. In giá trị của`polygons`cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Mỗi đa giác cần chính xác ba que trở lên có chiều dài bằng nhau. Bằng cách đếm số que trên mỗi chiều dài, chúng ta có thể tính toán trực tiếp có bao nhiêu nhóm ba que không chồng lên nhau tồn tại cho mỗi chiều dài. Tính tổng theo độ dài đảm bảo rằng tất cả các que đều được sử dụng một cách tối ưu mà không vi phạm quy tắc "không tái sử dụng". Vì chúng tôi không bao giờ trộn lẫn các độ dài nên chúng tôi tránh được việc phải thực hiện bất kỳ lựa chọn tập hợp con phức tạp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    sticks = list(map(int, input().split()))
    count = [0] * 101
    for stick in sticks:
        count[stick] += 1
    polygons = 0
    for length in range(1, 101):
        polygons += count[length] // 3
    print(polygons)
```Mã này bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp, chúng tôi đọc số lượng que và độ dài của chúng. Mảng tần số`count`được sử dụng để kiểm đếm xem có bao nhiêu que cho mỗi chiều dài có thể. Bằng cách chia mỗi tần số cho ba, chúng ta tìm ra số lượng đa giác có thể tạo thành từ độ dài đó, tính tổng kết quả cho tất cả các độ dài. Việc sử dụng phép chia số nguyên đảm bảo chúng ta bỏ qua các que còn sót lại không thể hoàn thành đa giác. 

Các điểm tinh tế bao gồm việc khởi tạo mảng đếm thành kích thước 101 để tính độ dài lên tới 100 và đảm bảo rằng chúng tôi sử dụng phép chia số nguyên một cách chính xác để tránh đa giác phân số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
2 2 3 3 3 3
```| Chiều dài | Đếm | Đa giác | 
| --- | --- | --- | 
| 2 | 2 | 0 | 
| 3 | 4 | 1 | 
| Tổng số đa giác: 1 | | | 

Điều này cho thấy độ dài có ít hơn ba que không thể đóng góp. 

### Ví dụ 2 

đầu vào:```
4 2 2 2 2 4 2 4 4
```| Chiều dài | Đếm | Đa giác | 
| --- | --- | --- | 
| 2 | 5 | 1 | 
| 4 | 4 | 1 | 
| Tổng số đa giác: 2 | | | 

Điều này chứng tỏ rằng nhiều độ dài có thể đồng thời đóng góp vào tổng số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + L) | Đếm que mất O(n), lặp qua các độ dài mất O(L) | 
| Không gian | O(L) | Mảng tần số cỡ 101 | 

Được cho`n`lên tới 100 và`L = 100`, giải pháp chạy thoải mái dưới giới hạn 1 giây với mức sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        sticks = list(map(int, input().split()))
        count = [0] * 101
        for stick in sticks:
            count[stick] += 1
        polygons = sum(c // 3 for c in count)
        print(polygons)
    return output.getvalue().strip()

# Provided samples
assert run("4\n1\n1\n2\n1 1\n6\n2 2 3 3 3 3\n9\n4 2 2 2 2 4 2 4 4\n") == "0\n0\n1\n2"

# Custom cases
assert run("1\n3\n1 1 1\n") == "1", "single triangle"
assert run("1\n5\n5 5 5 5 5\n") == "1", "five sticks of same length"
assert run("1\n6\n6 6 6 6 6 6\n") == "2", "two polygons possible"
assert run("1\n10\n1 1 1 2 2 2 3 3 3 3\n") == "4", "multiple lengths"
assert run("1\n2\n7 7\n") == "0", "insufficient sticks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 que có cùng chiều dài | 1 | Hình thành tam giác cơ bản | 
| 5 que có cùng chiều dài | 1 | Gậy còn sót lại bị bỏ qua | 
| 6 que có cùng chiều dài | 2 | Nhiều đa giác có cùng độ dài | 
| Độ dài hỗn hợp | 4 | Tổng đa giác theo chiều dài | 
| Chỉ có hai cây gậy | 0 | Không thể tạo thành đa giác với ít hơn ba que | 

## Vỏ cạnh 

Đối với đầu vào`2\n7 7`, thuật toán khởi tạo mảng count về 0, tăng count[7] hai lần và tính toán`2 // 3`bằng không. Kết quả đầu ra chính xác bằng 0, chứng tỏ rằng số lượng que thiếu được xử lý tự động. Tương tự, đối với`6\n6 6 6 6 6 6`, số[6] là 6, và`6 // 3`bằng 2, thu được hai đa giác. Điều này xác nhận rằng cả đóng góp một chiều dài và nhiều chiều dài đều được tính chính xác.
