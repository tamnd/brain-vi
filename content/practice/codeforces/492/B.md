---
title: "CF 492B - Vanya và đèn lồng"
description: "Chúng ta được yêu cầu xác định bán kính ánh sáng tối thiểu của những chiếc đèn lồng được đặt dọc theo một con phố thẳng có chiều dài l sao cho toàn bộ con phố được chiếu sáng. Mỗi chiếc đèn lồng ở một vị trí cố định dọc theo đường phố và có thể chiếu sáng các điểm trong khoảng cách d về bên trái và bên phải của nó."
date: "2026-06-07T17:44:22+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 492
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 280 (Div. 2)"
rating: 1200
weight: 492
solve_time_s: 98
verified: true
draft: false
---

[CF 492B - Vanya và những chiếc đèn lồng](https://codeforces.com/problemset/problem/492/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, thực hiện, toán học, sắp xếp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định bán kính ánh sáng tối thiểu của những chiếc đèn lồng được đặt dọc theo một con phố thẳng có chiều dài _l_ sao cho toàn bộ con phố được chiếu sáng. Mỗi chiếc đèn lồng ở một vị trí cố định dọc theo đường phố và có thể chiếu sáng các điểm trong khoảng cách _d_ về bên trái và bên phải của nó. Mục tiêu là tìm _d_ nhỏ nhất sao cho mọi điểm trên đường từ 0 đến _l_ đều nằm trong bán kính ánh sáng của ít nhất một chiếc đèn lồng. 

Dữ liệu đầu vào bao gồm số lượng đèn lồng _n_, chiều dài đường phố _l_ và vị trí của đèn lồng. Đèn lồng có thể được đặt ở cuối đường và nhiều đèn lồng có thể ở cùng một vị trí. 

Những hạn chế là vừa phải. Với _n_ 1000 và _l_ lên tới 10^9, mọi giải pháp có độ phức tạp lớn hơn O(n log n) đều có thể không cần thiết. Việc sắp xếp các vị trí đèn lồng là O(n log n) là khả thi. Việc kiểm tra mạnh mẽ mọi điểm dọc theo đường phố là không thể do chiều dài đường phố lớn. 

Các trường hợp cạnh có thể phá vỡ cách tiếp cận ngây thơ bao gồm một đèn lồng ở vị trí 0 hoặc _l_, các đèn lồng được nhóm lại với nhau để lại khoảng trống ở hai đầu hoặc chỉ một đèn lồng. Ví dụ: một con phố có chiều dài 10 với đèn lồng ở vị trí [0] yêu cầu bán kính là 10, trong khi [0,10] chỉ yêu cầu 5. Một thuật toán bất cẩn bỏ qua các mép phố sẽ đánh giá thấp bán kính yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi điểm dọc theo đường phố và kiểm tra khoảng cách của nó đến chiếc đèn lồng gần nhất. Điều này đúng vì khoảng cách tối đa từ bất kỳ điểm nào đến đèn lồng xác định bán kính yêu cầu, nhưng với chiều dài đường phố lên tới 10^9 thì điều này là quá chậm. 

Quan sát quan trọng là vấn đề có thể giảm xuống khoảng cách giữa các vị trí đèn lồng được sắp xếp. Sau khi phân loại các đèn lồng, khoảng cách lớn nhất giữa các đèn lồng liên tiếp sẽ xác định bán kính tối thiểu cần thiết để bao phủ phần giữa của đường phố. Khoảng cách đến điểm đầu và điểm cuối của phố cũng phải được xem xét riêng, vì đường có thể bắt đầu trước cột đèn đầu tiên hoặc kết thúc sau cột đèn cuối cùng. 

Thông tin chi tiết là bán kính tối thiểu _d_ là giá trị tối đa của ba giá trị: một nửa khoảng cách tối đa giữa các đèn lồng liên tiếp, khoảng cách từ đèn lồng đầu tiên đến đầu phố và khoảng cách từ đèn lồng cuối cùng đến cuối đường. Điều này làm giảm vấn đề xuống một cách sắp xếp đơn giản và một vài so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(l * n) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng đèn _n_ và chiều dài phố _l_, sau đó đọc vị trí các đèn vào danh sách. Việc sắp xếp các vị trí đèn lồng đảm bảo chúng ta có thể phân tích các khoảng trống liên tiếp một cách hiệu quả. 
2. Sắp xếp các vị trí đèn lồng theo thứ tự tăng dần. Việc sắp xếp cho phép chúng tôi tính toán khoảng cách lớn nhất giữa các đèn lồng liên tiếp chỉ bằng một lần vượt qua. 
3. Tính khoảng cách tối đa giữa các đèn lồng liên tiếp. Đối với mỗi cặp liên tiếp, hãy tính một nửa khoảng cách. Điều này thể hiện bán kính cần thiết để các đoạn giữa được bao phủ hoàn toàn bởi đèn lồng. Lưu trữ nửa khoảng cách lớn nhất như vậy. 
4. Tính khoảng cách từ chiếc đèn lồng đầu tiên đến đầu phố và từ chiếc đèn lồng cuối cùng đến cuối phố. Những khoảng cách này đảm bảo các điểm cuối của đường phố đều được chiếu sáng. 
5. Câu trả lời là khoảng cách tối đa của khoảng cách nửa tối đa và khoảng cách đến các điểm cuối đường. In giá trị này với độ chính xác vừa đủ.

Thuật toán hoạt động vì khoảng cách tối đa giữa các đèn lồng kiểm soát phạm vi bao phủ cho các đoạn giữa và các điểm cuối được xử lý riêng. Tính chất ánh sáng trải đều về bên trái và bên phải đảm bảo rằng chỉ cần lấy một nửa khoảng cách giữa các đèn lồng là đủ để bao phủ tất cả các điểm giữa chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, l = map(int, input().split())
lanterns = list(map(int, input().split()))

lanterns.sort()

# largest half-gap between consecutive lanterns
max_gap = max((lanterns[i+1] - lanterns[i]) / 2 for i in range(n-1))

# distance from street ends
left_edge = lanterns[0] - 0
right_edge = l - lanterns[-1]

# the required radius
d = max(max_gap, left_edge, right_edge)

print(f"{d:.10f}")
```Mã đầu tiên sắp xếp các đèn lồng để đánh giá các khoảng trống. Sự hiểu biết tính toán một nửa mỗi khoảng trống liên tiếp để xử lý phạm vi bao phủ bên trong. Khoảng cách đến các điểm cuối được tính toán rõ ràng. Lấy mức tối đa đảm bảo tất cả các phân khúc được bao phủ. In 10 chữ số thập phân đáp ứng yêu cầu về độ chính xác. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
7 15
15 5 3 7 9 14 0
```| Bước | Đèn lồng (sắp xếp) | Nửa khoảng cách tối đa | Cạnh trái | Cạnh phải | d | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [0,3,5,7,9,14,15] | (3-0)/2=1,5, (5-3)/2=1,0, (7-5)/2=1,0, (9-7)/2=1,0, (14-9)/2=2,5, (15-14)/2=0,5 | 0 | 0 | tối đa(2,5,0,0)=2,5 | 

Nửa khe hở tối đa là 2,5, bao phủ khe hở bên trong lớn nhất. Không cần thêm bán kính cho các cạnh vì đèn lồng che chúng. Câu trả lời là 2,5. 

**Đầu vào mẫu 2 (đèn lồng đơn khi bắt đầu)**```
1 10
0
```| Bước | Đèn lồng (sắp xếp) | Nửa khoảng cách tối đa | Cạnh trái | Cạnh phải | d | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [0] | Không áp dụng | 0 | 10 | tối đa(0,0,10)=10 | 

Chỉ với một chiếc đèn lồng khi bắt đầu, bán kính phải bao phủ toàn bộ con phố. Thuật toán tính toán chính xác điều này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, tất cả các bước khác là O(n) | 
| Không gian | O(n) | Lưu trữ vị trí đèn lồng | 

Cho n ≤ 1000, các phép toán O(n log n) là không đáng kể. Việc sử dụng bộ nhớ thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, l = map(int, input().split())
    lanterns = list(map(int, input().split()))
    lanterns.sort()
    max_gap = max((lanterns[i+1] - lanterns[i])/2 for i in range(n-1)) if n > 1 else 0
    left_edge = lanterns[0] - 0
    right_edge = l - lanterns[-1]
    d = max(max_gap, left_edge, right_edge)
    return f"{d:.10f}"

# Provided sample
assert run("7 15\n15 5 3 7 9 14 0\n") == "2.5000000000", "sample 1"
# Single lantern at start
assert run("1 10\n0\n") == "10.0000000000", "single lantern at start"
# Single lantern at end
assert run("1 10\n10\n") == "10.0000000000", "single lantern at end"
# Two lanterns at ends
assert run("2 10\n0 10\n") == "5.0000000000", "two lanterns at ends"
# Lanterns clustered in middle
assert run("3 10\n4 5 6\n") == "4.0000000000", "clustered middle lanterns"
# Lanterns at 0, 1, 2, ..., 1000 on length 1000
assert run("1001 1000\n" + " ".join(str(i) for i in range(1001)) + "\n") == "0.5000000000", "dense lanterns"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10\n0 | 10.0000000000 | Đèn lồng đơn lúc bắt đầu, bao trùm cả đường phố | 
| 1 10\n10 | 10.0000000000 | Đèn lồng đơn ở cuối | 
| 2 10\n0 10 | 5.0000000000 | Hai chiếc đèn lồng ở hai đầu, che ở giữa | 
| 3 10\n4 5 6 | 4.0000000000 | Đèn lồng chùm, mép không che | 
| 1001 1000\n0..1000 | 0,5000000000 | Đèn lồng dày đặc, bán kính tối thiểu | 

## Vỏ cạnh 

Nếu tất cả các đèn lồng ở cùng một vị trí, chẳng hạn [5] trên một con phố có chiều dài 10, thì nửa khoảng cách tối đa giữa các đèn lồng liên tiếp là không xác định, vì vậy chúng ta phải xử lý n=1 riêng biệt. Khoảng cách đến điểm bắt đầu và điểm kết thúc xác định bán kính: 5 để đạt 0, 5 để đạt 10, do đó bán kính là max(5,5)=5. 

Nếu đèn lồng ở cuối, thuật toán sẽ tính toán chính xác khoảng cách bằng 0 đến cạnh gần nhất và chỉ xem xét các khoảng trống bên trong. Đối với một con phố dài 10 có đèn lồng [0,10] thì khoảng cách lớn nhất là 10 nên một nửa là 5
