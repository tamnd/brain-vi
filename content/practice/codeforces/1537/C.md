---
title: "CF 1537C - Vách đá đầy thử thách"
description: "Chúng ta được cung cấp nhiều tập hợp độ cao của các ngọn núi và phải sắp xếp chúng thành một chuỗi. Việc tối ưu hóa xảy ra trong hai lớp. Đầu tiên, trong số tất cả các hoán vị có thể có, chúng tôi muốn độ cao đầu tiên và cuối cùng càng gần nhau càng tốt."
date: "2026-06-10T15:08:05+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1537
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 726 (Div. 2)"
rating: 1200
weight: 1537
solve_time_s: 490
verified: false
draft: false
---

[CF 1537C - Vách đá đầy thử thách](https://codeforces.com/problemset/problem/1537/C) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 8 phút 10s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều tập hợp độ cao của các ngọn núi và phải sắp xếp chúng thành một chuỗi. 

Việc tối ưu hóa xảy ra trong hai lớp. Đầu tiên, trong số tất cả các hoán vị có thể có, chúng tôi muốn độ cao đầu tiên và cuối cùng càng gần nhau càng tốt. Nói cách khác, chúng ta giảm thiểu giá trị$|h_1 - h_n|$. 

Khi đã đạt được sự khác biệt về điểm cuối tối thiểu có thể có đó, chúng tôi chỉ xem xét những cách sắp xếp thỏa mãn nó. Trong số những cách sắp xếp đó, chúng ta muốn tối đa hóa số vị trí mà chuỗi không giảm, nghĩa là$h_i \le h_{i+1}$. 

Đầu ra là bất kỳ sự sắp xếp nào thỏa mãn cả hai mục tiêu. 

Các ràng buộc ngay lập tức gợi ý rằng chúng ta cần một cái gì đó gần với$O(n \log n)$mỗi trường hợp thử nghiệm. Tổng số độ cao trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, vì vậy việc sắp xếp là hoàn toàn có thể chấp nhận được. Bất cứ điều gì liên quan đến việc liệt kê các hoán vị đều không thể thực hiện được. Thậm chí$O(n^2)$sẽ quá chậm trong trường hợp xấu nhất bởi vì$n$có thể đạt được$2 \cdot 10^5$. 

Phần tinh tế là vấn đề không yêu cầu số lượng chuyển đổi không giảm lớn nhất trong số tất cả các hoán vị. Điều kiện điểm cuối đến trước và chi phối mọi thứ khác. 

Hãy xem xét đầu vào:```
4
1 2 3 10
```Một cách tiếp cận bất cẩn có thể dẫn đến:```
1 2 3 10
```Điều này có ba chuyển đổi không giảm, là mức tối đa, nhưng các điểm cuối khác nhau bởi$9$. Sự khác biệt điểm cuối tối thiểu có thể thực sự là$1$, đạt được bằng cách chọn cặp liền kề$1,2$hoặc$2,3$như các điểm cuối. Bất kỳ giải pháp nào bỏ qua mục tiêu đầu tiên đều sai. 

Một lỗi dễ xảy ra khác khi tồn tại các giá trị trùng lặp.```
4
2 2 5 8
```Sự khác biệt điểm cuối tối thiểu là$0$, đạt được bằng cách đặt hai bản sao của$2$ở cuối. Một giải pháp chỉ tìm kiếm các giá trị liền kề tăng dần có thể bỏ lỡ các giá trị bằng nhau thậm chí còn tốt hơn vì chúng tạo ra chênh lệch điểm cuối nhỏ nhất có thể. 

Kích thước hợp lệ nhỏ nhất cũng cần được chú ý.```
2
3 1
```Chỉ có một cặp điểm cuối vì mọi phần tử đều phải được sử dụng. Sau khi sắp xếp ta thu được:```
1 3
```Mọi công trình phức tạp hơn vẫn phải hoạt động chính xác khi$n=2$. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo mọi hoán vị, tính toán chênh lệch điểm cuối, chỉ giữ lại những giá trị có giá trị nhỏ nhất và trong số đó chọn một hoán vị có số lượng cặp liền kề không giảm lớn nhất. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa bài toán. Thật không may, nó yêu cầu kiểm tra$n!$hoán vị. Ngay cả đối với$n=10$, đó là rồi$3{,}628{,}800$khả năng. Vì$n=20$, con số trở nên lớn về mặt thiên văn. Với$n$lên tới$2 \cdot 10^5$, vũ lực là hoàn toàn không thể. 

Để tìm một cấu trúc hữu ích, hãy sắp xếp độ cao:$$a_0 \le a_1 \le \cdots \le a_{n-1}$$Sự khác biệt điểm cuối nhỏ nhất có thể xảy ra phải đến từ hai phần tử liền kề theo thứ tự được sắp xếp. Nếu chúng ta chọn bất kỳ hai giá trị nào không liền kề thì tồn tại ít nhất một giá trị giữa chúng, do đó hiệu của chúng không thể nhỏ hơn khoảng cách liền kề tối thiểu. 

Giả sử khoảng cách liền kề tối thiểu xảy ra giữa$a_i$Và$a_{i+1}$. 

Bây giờ hãy nghĩ đến việc tối đa hóa số lần chuyển đổi không giảm. Một trình tự được sắp xếp sẽ cho$n-1$những chuyển đổi như vậy, nhưng điểm cuối của nó là giá trị nhỏ nhất và lớn nhất, thường không giảm thiểu chênh lệch điểm cuối. 

Quan sát quan trọng là chúng ta có thể thực hiện$a_i$Và$a_{i+1}$trở thành điểm cuối trong khi vẫn giữ gần như toàn bộ chuỗi được sắp xếp. 

Đặt tất cả các phần tử lớn hơn$a_{i+1}$đầu tiên theo thứ tự tăng dần. Sau đó đặt tất cả các phần tử nhỏ hơn$a_i$theo thứ tự tăng dần. Vị trí cuối cùng$a_i$. 

Trình tự bắt đầu bằng$a_{i+1}$và kết thúc bằng$a_i$. 

Viết rõ ràng:$$a_{i+1}, a_{i+2}, \ldots, a_{n-1}, a_0, a_1, \ldots, a_{i-1}, a_i$$Bên trong mỗi khối được sắp xếp, mọi chuyển đổi đều không giảm. Sự giảm duy nhất xảy ra khi chúng ta chuyển từ phần tử lớn nhất sang phần tử nhỏ nhất giữa hai khối. 

Điều đó mang lại khó khăn tối đa có thể có trong số các thỏa thuận có điểm cuối đạt được sự khác biệt tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng độ cao. 
2. Tìm cặp liền kề có chênh lệch nhỏ nhất. Đặt chỉ số của nó là$i$, nghĩa là cặp đó là$(a_i, a_{i+1})$. 

Cặp này phải được sử dụng làm điểm cuối vì không có cặp nào khác có thể tạo ra chênh lệch điểm cuối nhỏ hơn. 
3. Bắt đầu câu trả lời với tất cả các yếu tố từ$a_{i+1}$ĐẾN$a_{n-1}$theo thứ tự. 

Các giá trị này vẫn được sắp xếp, vì vậy mọi chuyển đổi bên trong phân đoạn này đều góp phần gây ra khó khăn. 
4. Nối tất cả các phần tử từ$a_0$ĐẾN$a_{i-1}$theo thứ tự. 

Điều này tạo ra chính xác một giọt, từ giá trị lớn nhất đến giá trị nhỏ nhất. 
5. Nối thêm$a_i$ở cuối. 

Trình tự bây giờ bắt đầu tại$a_{i+1}$và kết thúc tại$a_i$, do đó chênh lệch điểm cuối bằng khoảng cách liền kề tối thiểu. 
6. Xuất ra chuỗi đã xây dựng. 

### Tại sao nó hoạt động 

Đặt khoảng cách liền kề tối thiểu trong mảng được sắp xếp nằm trong khoảng$a_i$Và$a_{i+1}$. 

Bất kỳ cặp giá trị nào được sử dụng làm phần tử đầu tiên và cuối cùng phải có sự khác biệt ít nhất là khoảng cách này. Đó là thuộc tính cơ bản của mảng được sắp xếp: sự khác biệt tối thiểu giữa hai phần tử bất kỳ xuất hiện giữa các phần tử liền kề. Vì công trình của chúng tôi sử dụng chính xác$a_i$Và$a_{i+1}$làm điểm cuối, nó đạt được sự khác biệt điểm cuối nhỏ nhất có thể. 

Trong số tất cả các sắp xếp với các điểm cuối này, chúng ta muốn có càng nhiều chuyển đổi không giảm càng tốt. Việc xây dựng tạo thành một lần duyệt theo chu kỳ của mảng đã được sắp xếp. Mọi cặp lân cận đều tuân theo thứ tự được sắp xếp ngoại trừ một bước nhảy vòng quanh từ phần tử tối đa trở lại phần tử tối thiểu. Vì phải tồn tại ít nhất một mức giảm khi các điểm cuối được cố định theo cách này, nên có chính xác một mức giảm là tối ưu. Do đó, số lượng chuyển tiếp không giảm được tối đa hóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        a.sort()

        idx = 0
        best = a[1] - a[0]

        for i in range(1, n - 1):
            diff = a[i + 1] - a[i]
            if diff < best:
                best = diff
                idx = i

        res = a[idx + 1:] + a[:idx] + [a[idx]]
        ans.append(" ".join(map(str, res)))

    sys.stdout.write("\n".join(ans))

solve()
```Bước đầu tiên sắp xếp độ cao. Sau khi sắp xếp, việc tìm chênh lệch điểm cuối tối thiểu sẽ tương đương với việc tìm khoảng cách liền kề tối thiểu. 

Biến`idx`lưu trữ điểm cuối bên trái của cặp khoảng cách tối thiểu đó. Nếu cặp tốt nhất là`(a[idx], a[idx + 1])`, việc xây dựng cần thiết bắt đầu từ`a[idx + 1]`. 

Biểu thức:```
a[idx + 1:] + a[:idx] + [a[idx]]
```thực hiện sự sắp xếp chính xác được mô tả trong bằng chứng. 

lát cắt`a[idx + 1:]`chứa tất cả các phần tử từ điểm cuối lớn hơn đến giá trị tối đa. 

lát cắt`a[:idx]`chứa tất cả các giá trị nhỏ hơn điểm cuối nhỏ hơn. 

Cuối cùng,`a[idx]`được đặt ở cuối sao cho hai điểm cuối trở thành cặp khoảng cách tối thiểu. 

Việc thực hiện cũng xử lý`n = 2`một cách chính xác. Trong trường hợp đó,`idx = 0`, do đó kết quả trở thành:```
[a[1]] + [] + [a[0]]
```đó chính xác là sự sắp xếp cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
4 2 1 2
```Mảng được sắp xếp:```
[1, 2, 2, 4]
```| Bước | Giá trị | 
| --- | --- | 
| Mảng được sắp xếp | [1, 2, 2, 4] | 
| Khoảng trống liền kề | 1, 0, 2 | 
| Chỉ số khoảng cách tối thiểu | 1 | 
| Cặp được chọn | (2, 2) | 
| a[idx+1:] | [2, 4] | 
| a[:idx] | [1] | 
| Yếu tố cuối cùng | [2] | 
| Kết quả | [2, 4, 1, 2] | 

Điểm cuối đều bằng 2, do đó chênh lệch điểm cuối là 0, đây là giá trị nhỏ nhất có thể. Bên trong chuỗi chỉ có một mức giảm, từ 4 xuống 1, giúp tối đa hóa độ khó. 

### Mẫu 2 

đầu vào:```
2
3 1
```Mảng được sắp xếp:```
[1, 3]
```| Bước | Giá trị | 
| --- | --- | 
| Mảng được sắp xếp | [1, 3] | 
| Khoảng trống liền kề | 2 | 
| Chỉ số khoảng cách tối thiểu | 0 | 
| Cặp được chọn | (1, 3) | 
| a[idx+1:] | [3] | 
| a[:idx] | [] | 
| Yếu tố cuối cùng | [1] | 
| Kết quả | [3, 1] | 

Sự khác biệt điểm cuối là 2, điều này là không thể tránh khỏi vì chỉ có hai phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian |$O(n)$| Mảng câu trả lời lưu trữ tất cả các độ cao một lần | 

Tổng số độ cao trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Sắp xếp nhiều giá trị yêu cầu đại khái$2 \cdot 10^5 \log_2(2 \cdot 10^5)$hoạt động, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    input_stream = io.StringIO(inp)

    def input():
        return input_stream.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        a.sort()

        idx = 0
        best = a[1] - a[0]

        for i in range(1, n - 1):
            diff = a[i + 1] - a[i]
            if diff < best:
                best = diff
                idx = i

        res = a[idx + 1:] + a[:idx] + [a[idx]]
        out.append(" ".join(map(str, res)))

    return "\n".join(out)

# provided sample
assert run(
"""2
4
4 2 1 2
2
3 1
"""
) == "2 4 1 2\n3 1"

# minimum size
assert run(
"""1
2
5 8
"""
) == "8 5"

# all equal values
assert run(
"""1
4
7 7 7 7
"""
) == "7 7 7 7"

# duplicate values creating gap 0
assert run(
"""1
5
1 3 3 10 20
"""
) == "3 10 20 1 3"

# off-by-one near array ends
assert run(
"""1
4
1 2 10 11
"""
) == "2 10 11 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 8 | 8 5 | Kích thước tối thiểu cho phép | 
| 7 7 7 7 | 7 7 7 7 | Tất cả các khoảng trống đều bằng 0 | 
| 1 3 3 10 20 | 3 10 20 1 3 | Giá trị trùng lặp và khoảng cách tối thiểu ở giữa | 
| 1 2 10 11 | 2 10 11 1 | Cắt đúng khi cặp đã chọn ở gần đầu | 

## Vỏ cạnh 

Hãy xem xét:```
4
2 2 5 8
```Sau khi sắp xếp ta được:```
[2, 2, 5, 8]
```Khoảng cách liền kề tối thiểu là 0 giữa hai bản sao của 2. Thuật toán chọn cặp đó, tạo ra:```
2 5 8 2
```Điểm cuối đều bằng 2 nên chênh lệch điểm cuối là 0. Không có sự sắp xếp nào có thể làm tốt hơn. 

Coi như:```
2
3 1
```Sau khi sắp xếp:```
[1, 3]
```Cặp khoảng cách tối thiểu là cặp duy nhất có sẵn. Việc xây dựng mang lại:```
3 1
```Mã không bao giờ truy cập các chỉ mục không hợp lệ vì vòng lặp tìm kiếm không thực thi khi$n=2$. 

Coi như:```
4
1 2 3 10
```Mảng được sắp xếp đã có:```
[1, 2, 3, 10]
```Khoảng cách liền kề tối thiểu là 1. Thuật toán chọn cặp$(1,2)$và xây dựng:```
2 3 10 1
```Các điểm cuối khác nhau 1, là tối ưu. Một sự sắp xếp ngây thơ`1 2 3 10`sẽ tạo ra sự khác biệt về điểm cuối 9 và không đạt được mục tiêu chính. 

Coi như:```
5
4 4 4 4 4
```Mọi khoảng cách liền kề đều bằng 0. Thuật toán chọn cặp đầu tiên như vậy và trả về cùng một chuỗi:```
4 4 4 4 4
```Mọi chuyển đổi đều không giảm và chênh lệch điểm cuối vẫn bằng 0. Cấu trúc xử lý một cách tự nhiên trường hợp tất cả các giá trị giống hệt nhau.
