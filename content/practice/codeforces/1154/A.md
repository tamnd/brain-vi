---
title: "CF 1154A - Khôi phục ba số"
description: "Chúng ta có bốn số đại diện cho ba tổng theo cặp của các số nguyên dương chưa biết $a$, $b$, $c$ và tổng của cả ba số. Bốn số này không theo thứ tự cụ thể. Nhiệm vụ của chúng ta là xây dựng lại ba số nguyên ban đầu $a$, $b$, $c$ từ các tổng này."
date: "2026-06-12T02:47:34+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1154
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 552 (Div. 3)"
rating: 800
weight: 1154
solve_time_s: 91
verified: false
draft: false
---

[CF 1154A - Khôi phục ba số](https://codeforces.com/problemset/problem/1154/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn số đại diện cho ba tổng từng cặp số nguyên dương chưa biết$a$,$b$,$c$và tổng của ba số đó. Bốn số này không theo thứ tự cụ thể. Nhiệm vụ của chúng ta là xây dựng lại ba số nguyên ban đầu$a$,$b$,$c$từ những khoản tiền này. 

Đầu vào đảm bảo rằng giải pháp tồn tại, giúp đơn giản hóa nhiệm vụ của chúng tôi vì chúng tôi không cần xác thực tính khả thi. Mỗi số trên bảng ít nhất là 2 và nhiều nhất$10^9$, nghĩa là chúng ta đang xử lý kích thước đầu vào nhỏ (chỉ bốn số) nhưng có thể là số nguyên rất lớn. Vì chỉ có bốn số nên mọi thuật toán có độ phức tạp thời gian không đổi hoặc tuyến tính đều được chấp nhận. 

Các trường hợp đặc biệt cần xem xét bao gồm các tình huống trong đó một số hoặc tất cả$a$,$b$, Và$c$đều bình đẳng. Ví dụ, nếu$a = b = c = 1$, bốn số trên bảng sẽ là$2, 2, 2, 3$. Một cách tiếp cận ngây thơ giả định tất cả các khoản tiền đều khác nhau có thể thất bại ở đây. Một trường hợp khác là khi hai số bằng nhau, chẳng hạn như$a = b = 2, c = 3$, tạo ra số tiền$4, 5, 5, 7$. Nhận biết tổng của cả ba số và tách nó ra khỏi tổng theo cặp là rất quan trọng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng kiểm tra tất cả các phép gán có thể có của bốn số đối với vai trò của$a+b$,$a+c$,$b+c$, Và$a+b+c$. chỉ có$4! = 24$hoán vị nhỏ, do đó, một lực lượng vũ phu thuần túy sẽ hoạt động. Đối với mỗi hoán vị, chúng ta có thể giải một hệ phương trình tuyến tính nhỏ và kiểm tra xem tất cả các kết quả có phải là số nguyên dương hay không. Cách tiếp cận này hiệu quả vì chỉ có bốn con số, nhưng nó phức tạp một cách không cần thiết và không trực quan. 

Quan sát quan trọng là tổng của cả ba số$a+b+c$luôn là số lớn nhất trong bốn số vì nó gồm cả ba số nguyên dương. Khi chúng tôi xác định số lớn nhất là$a+b+c$, ba số còn lại phải là tổng theo cặp. Ký hiệu số lớn nhất là$S = a+b+c$và các số khác như$x, y, z$. Khi đó chúng ta có thể giải trực tiếp để tìm số ban đầu: 

-$a = S - (b+c) = S - x$-$b = S - (a+c) = S - y$-$c = S - (a+b) = S - z$Điều này hoạt động bất kể thứ tự vì$x, y, z$đại diện cho tất cả các khoản tiền theo cặp. Thuật toán giảm xuống việc sắp xếp, xác định số lớn nhất và phép trừ đơn giản. Nó đơn giản, trực tiếp và tránh liệt kê không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(24) = O(1) | O(1) | Hoạt động nhưng không cần thiết | 
| Tối ưu | O(4 log 4) = O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên vào một danh sách. Điều này nắm bắt các khoản tiền như được cung cấp theo thứ tự tùy ý. 
2. Sắp xếp danh sách theo thứ tự tăng dần. Việc sắp xếp giúp dễ dàng xác định số lớn nhất, đảm bảo$a+b+c$vì cả ba số đều dương. 
3. Gọi số cuối cùng trong danh sách đã sắp xếp là$S = a+b+c$. Gọi ba số đầu tiên trong danh sách đã sắp xếp là$x, y, z$. Những giá trị này tương ứng với các tổng theo cặp nhưng chúng ta không cần biết cái nào là cái nào. 
4. Tính các số ban đầu bằng cách trừ đi tổng từng cặp từ$S$: 

-$a = S - x$-$b = S - y$-$c = S - z$Những tính toán này hoạt động vì$S - x$loại bỏ tổng bao gồm tất cả các số nguyên ngoại trừ một số nguyên, để lại số nguyên còn lại. 
5. In$a$,$b$, Và$c$theo bất kỳ thứ tự nào. 

Tại sao nó hoạt động: Sắp xếp đảm bảo chúng tôi xác định được$a+b+c$như số lớn nhất. Trừ từng số còn lại từ tổng của cả ba sẽ tách biệt các số nguyên riêng lẻ. Bởi vì chúng tôi được đảm bảo một giải pháp với các số nguyên dương, không có sự mơ hồ và các phép toán sẽ tái tạo lại các số ban đầu một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

nums = list(map(int, input().split()))
nums.sort()
S = nums[3]
a = S - nums[0]
b = S - nums[1]
c = S - nums[2]
print(a, b, c)
```Giải pháp đọc dữ liệu đầu vào một cách hiệu quả, sắp xếp bốn số, xác định tổng lớn nhất và tái tạo lại từng số riêng lẻ bằng phép trừ. Việc sắp xếp đảm bảo chúng tôi xác định được một cách nhất quán$a+b+c$mà không cần kiểm tra thủ công tổng nào chứa cả ba số. Thứ tự trừ rất linh hoạt vì bài toán cho phép bất kỳ thứ tự nào ở đầu ra. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`3 6 5 4`Danh sách sắp xếp:`3 4 5 6`Tổng lớn nhất$S = 6$Số tiền còn lại:`3, 4, 5`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| 1 | số | [3,4,5,6] | 
| 2 | S | 6 | 
| 3 | a = S - nums[0] | 6 - 3 = 3 | 
| 4 | b = S - nums[1] | 6 - 4 = 2 | 
| 5 | c = S - nums[2] | 6 - 5 = 1 | 

Đầu ra:`3 2 1`(bất kỳ đơn hàng nào hợp lệ) 

Điều này xác nhận rằng việc trừ từng tổng theo cặp từ tổng sẽ tách ra các số nguyên chính xác. 

**Mẫu 2:** 

đầu vào:`2 2 2 3`Danh sách sắp xếp:`2 2 2 3`Tổng lớn nhất$S = 3$Số tiền còn lại:`2, 2, 2`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| 1 | số | [2,2,2,3] | 
| 2 | S | 3 | 
| 3 | a = 3 - 2 | 1 | 
| 4 | b = 3 - 2 | 1 | 
| 5 | c = 3 - 2 | 1 | 

Đầu ra:`1 1 1`Thuật toán xử lý chính xác trường hợp số hoàn toàn bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(4 log 4) = O(1) | Sắp xếp bốn số mất thời gian không đổi. | 
| Không gian | O(1) | Chỉ có bốn số được lưu trữ cộng với ba biến cho đầu ra. | 

Với kích thước đầu vào nhỏ, thuật toán này cực kỳ hiệu quả và phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    nums = list(map(int, input().split()))
    nums.sort()
    S = nums[3]
    a = S - nums[0]
    b = S - nums[1]
    c = S - nums[2]
    return f"{a} {b} {c}"

# Provided sample
assert run("3 6 5 4\n") in ["3 2 1", "2 3 1", "1 2 3"], "sample 1"

# Custom cases
assert run("2 2 2 3\n") == "1 1 1", "all equal case"
assert run("4 5 5 7\n") in ["3 2 2", "2 3 2", "2 2 3"], "two equal numbers"
assert run("10 11 15 21\n") in ["11 6 10", "10 11 6", "6 10 11"], "general case"
assert run("1000000000 1000000000 1000000000 3000000000\n") == "1000000000 1000000000 1000000000", "max values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 6 5 4 | 3 2 1 | trường hợp chung | 
| 2 2 2 3 | 1 1 1 | tất cả các số bằng nhau | 
| 4 5 5 7 | 3 2 2 | hai số bằng nhau | 
| 10 11 15 21 | 6 10 11 | thứ tự tổng tùy ý | 
| 10^9 10^9 10^9 3*10^9 | 10^9 10^9 10^9 | kích thước đầu vào tối đa | 

## Vỏ cạnh 

Ví dụ: nếu tất cả các số đều bằng nhau`2 2 2 3`, sắp xếp xác định`3`bằng tổng của ba số đó. Trừ`2`từ`3`sản xuất`1`cho từng số riêng lẻ, xây dựng lại chính xác`a = b = c = 1`. 

Ví dụ: nếu hai số bằng nhau`4 5 5 7`, sắp xếp cho`4,5,5,7`. lớn nhất`7`là$a+b+c$. Trừ`4,5,5`sản xuất`3,2,2`, xác định chính xác các số nguyên ban đầu
