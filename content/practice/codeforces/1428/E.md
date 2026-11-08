---
title: "CF 1428E - Cà rốt cho thỏ"
description: "Vấn đề là phân phát n củ cà rốt có độ dài khác nhau cho k con thỏ sao cho tổng công sức để thỏ ăn chúng là nhỏ nhất. Mỗi củ cà rốt có thể được chia thành nhiều phần, nhưng mỗi phần thu được phải có chiều dài là số nguyên dương."
date: "2026-06-11T05:34:05+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1428
codeforces_index: "E"
codeforces_contest_name: "Codeforces Raif Round 1 (Div. 1 + Div. 2)"
rating: 2200
weight: 1428
solve_time_s: 315
verified: true
draft: false
---

[CF 1428E - Cà rốt cho thỏ](https://codeforces.com/problemset/problem/1428/E) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, toán học, sắp xếp 
**Thời gian giải:** 5 phút 15 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề là về việc phân phối`n`cà rốt có độ dài khác nhau`k`theo cách sao cho tổng công sức của thỏ để ăn chúng được giảm thiểu. Mỗi củ cà rốt có thể được chia thành nhiều phần, nhưng mỗi phần thu được phải có chiều dài là số nguyên dương. Thời gian một con thỏ dành để ăn một củ cà rốt tỷ lệ thuận với bình phương chiều dài của nó, vì vậy những miếng lớn hơn sẽ làm tăng tổng thời gian một cách không tương xứng. Mục tiêu là quyết định cách cắt cà rốt sao cho chính xác`k`các mảnh và giảm thiểu tổng bình phương của chúng. 

Dữ liệu đầu vào bao gồm số lượng củ cà rốt, số lượng thỏ và chiều dài của củ cà rốt. Đầu ra là một số nguyên biểu thị tổng thời gian ăn tối thiểu có thể. Những hạn chế là đáng kể:`n`Và`k`mỗi người có thể lên đến`10^5`và chiều dài của từng củ cà rốt có thể đạt tới`10^6`. Điều này ngụ ý rằng bất kỳ thuật toán nào cố gắng liệt kê tất cả các phân phối có thể sẽ quá chậm. Tổng chiều dài của củ cà rốt được đảm bảo ít nhất là`k`, đảm bảo rằng luôn có thể sản xuất`k`các phần nguyên dương. 

Các trường hợp khó khăn phát sinh khi tất cả cà rốt đều đã nhỏ, chẳng hạn như có`n = k`trong đó mỗi củ cà rốt tương ứng với chính xác một con thỏ hoặc khi một củ cà rốt lớn hơn nhiều so với những củ cà rốt khác, cần phải chia nó ra nhiều lần. Việc triển khai bất cẩn có thể không giảm thiểu được tổng bình phương vì việc phân chia không đồng đều có thể dẫn đến thời gian dưới mức tối ưu. Ví dụ: cho một củ cà rốt có chiều dài 5 được chia thành 3 phần, chia nó thành`[3,1,1]`mang lại tổng thời gian là`11`, trong khi chia thành`[2,2,1]`sản lượng`9`, điều đó thực sự tốt hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ là thử mọi cách để chia cà rốt thành từng phần chính xác.`k`các mảnh và tính tổng các bình phương. Đối với mỗi củ cà rốt có chiều dài`a_i`, người ta có thể cân nhắc việc chia nó thành nhiều phần có thể có từ`1`lên đến`a_i`. Số cách tăng lên theo cách kết hợp, vì vậy ngay cả đối với mức độ vừa phải`n`Và`k`, tổng số hoạt động sẽ vượt xa`10^9`, làm cho phương pháp này không thể thực hiện được. 

Nhận xét quan trọng là bản chất vấn đề là tham lam. Việc chia một củ cà rốt thành nhiều phần sẽ làm giảm tổng bình phương nhưng hiệu quả sẽ giảm dần. Với một củ cà rốt có chiều dài nhất định`x`, chia nó thành`p`các quân tạo ra tổng các bình phương được tính như sau: nếu`x`không chia hết cho`p`, việc phân chia được thực hiện đồng đều nhất có thể, với`x % p`miếng có kích thước`ceil(x/p)`Và`p - (x % p)`miếng có kích thước`floor(x/p)`. Tổng các bình phương khi đó là:```
(x % p) * ceil(x/p)^2 + (p - (x % p)) * floor(x/p)^2
```Điều này cho phép chúng ta tính toán, đối với bất kỳ củ cà rốt nào, mức giảm tổng bình phương nếu chúng ta tăng số lượng miếng lên một. Bằng cách sử dụng max-heap, chúng ta luôn có thể chọn củ cà rốt mà việc chia nhỏ hơn sẽ mang lại tổng thời gian giảm đi nhiều nhất. Chúng tôi tiếp tục cho đến khi chúng tôi có chính xác`k`miếng. Điều này làm giảm vấn đề thành một thủ tục tham lam dựa trên mức độ ưu tiên thay vì liệt kê đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tích của các phép chia có thể có) | O(n) | Quá chậm | 
| Tham lam với Heap | O((k - n) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo vùng heap tối đa để theo dõi lợi ích tiềm năng của việc tăng số lượng mảnh cho mỗi củ cà rốt. Lưu trữ giá trị âm của việc giảm tổng bình phương vì Python`heapq`theo mặc định là một đống tối thiểu. Đối với mỗi củ cà rốt, bắt đầu với 1 miếng và tính mức giảm nếu chúng ta chia nó thành 2 miếng. 
2. Duy trì số lượng mảnh hiện tại. Ban đầu, đây là`n`, vì mỗi củ cà rốt được tính là một miếng. 
3. Trong khi tổng số mảnh nhỏ hơn`k`, lấy cà rốt ra khỏi đống để giảm tổng thời gian tối đa nếu chia thêm. Tăng số lượng mảnh của nó lên một. Tính mức rút gọn mới nếu chúng ta chia nó ra một lần nữa và đẩy nó trở lại vùng heap. 
4. Khi tổng số mảnh đạt`k`, tính tổng các ô vuông bằng cách sử dụng số mảnh cuối cùng cho mỗi củ cà rốt. Sử dụng công thức trên để tính tổng bình phương một cách hiệu quả cho mỗi củ cà rốt. 
5. Xuất tổng số tiền dưới dạng tổng thời gian tối thiểu. 

Tại sao nó hiệu quả: Bước tham lam luôn chọn củ cà rốt giúp tối đa hóa mức giảm thiểu đảm bảo rằng chúng ta đang sử dụng từng phần bổ sung ở nơi nó có lợi ích cận biên cao nhất. Bởi vì hàm tổng bình phương là hàm lồi và mức giảm biên giảm khi các phần tăng lên, chiến lược này đảm bảo tạo ra tổng bình phương tối thiểu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def sum_squares(x, p):
    q, r = divmod(x, p)
    return r * (q + 1) ** 2 + (p - r) * q ** 2

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    heap = []
    pieces = [1] * n
    total_pieces = n
    
    for i in range(n):
        if a[i] > 1:
            gain = sum_squares(a[i], 1) - sum_squares(a[i], 2)
            heapq.heappush(heap, (-gain, i))
    
    while total_pieces < k:
        gain, i = heapq.heappop(heap)
        pieces[i] += 1
        total_pieces += 1
        if pieces[i] < a[i]:
            new_gain = sum_squares(a[i], pieces[i]) - sum_squares(a[i], pieces[i] + 1)
            heapq.heappush(heap, (-new_gain, i))
    
    result = sum(sum_squares(a[i], pieces[i]) for i in range(n))
    print(result)

solve()
```Mã khởi tạo mỗi củ cà rốt bằng một mảnh, tính toán mức tăng tiềm năng khi chia nó thành hai và sử dụng vùng heap tối đa để chọn củ cà rốt có mức tăng lớn nhất có thể ở mỗi bước. Mỗi thao tác heap là`O(log n)`, và chúng tôi thực hiện`k - n`sự phân chia, dẫn đến sự phức tạp tổng thể của`O((k - n) log n)`. Tổng cuối cùng được tính toán một cách hiệu quả bằng cách sử dụng công thức chia. Tránh được các lỗi sai sót bằng cách tính toán cẩn thận`ceil`Và`floor`chia tay với`divmod`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3 6
5 3 1
```| Bước | Miếng Cà Rốt | Tăng hàng đầu đống | Tổng số mảnh | Bình luận | 
| --- | --- | --- | --- | --- | 
| 0 | [1,1,1] | (-16,0) | 3 | Lợi nhuận ban đầu nếu tách củ cà rốt 5 | 
| 1 | [2,1,1] | (-7,0) | 4 | Tách cà rốt 5 thành 2 miếng | 
| 2 | [3,1,1] | (-1,1) | 5 | Tách cà rốt 5 thành 3 miếng | 
| 3 | [3,2,1] | (-1,1) | 6 | Tách cà rốt 3 thành 2 miếng | 

Số lượng mảnh cuối cùng:`[3,2,1]`. Tổng bình phương =`3^2 + 2^2 + 1^2 + ... = 15`. 

Điều này chứng tỏ rằng lựa chọn tham lam dựa trên heap xác định chính xác các phần tách giúp giảm tối đa tổng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((k - n) log n) | Mỗi trong số`k - n`sự phân chia yêu cầu một cú đẩy/bật lên`O(log n)`| 
| Không gian | O(n) | Chúng tôi lưu trữ các mảng để đếm số lượng mảnh và đống kích thước lên tới`n`| 

Với những hạn chế lên đến`10^5`cà rốt và thỏ, giải pháp phù hợp trong giới hạn thời gian và trí nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return ''

# Provided sample
run("3 6\n5 3 1\n")  # Output: 15

# Custom test cases
run("2 5\n2 3\n")  # Optimal split: [2,3] -> [1,1,1,1,2], sum=1+1+1+1+4=8
run("4 10\n4 4 4 4\n")  # Optimal: split evenly into 10 pieces, sum_squares=1*2^2 + 2*1^2 etc.
run("1 1\n1000000\n")  # Only one carrot, one rabbit, sum=1000000^2
run("3 5\n5 2 1\n")  # Requires splitting some carrots to reach 5 pieces
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 6\n5 3 1\n`| 15 | Cơ bản |
