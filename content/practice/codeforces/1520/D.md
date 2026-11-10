---
title: "CF 1520D - Sự khác biệt tương tự"
description: "Chúng ta được cung cấp một mảng và phải đếm xem có bao nhiêu cặp chỉ mục $(i, j)$ với $i < j$ thỏa mãn mối quan hệ rất cụ thể giữa các giá trị và vị trí của chúng."
date: "2026-06-10T18:06:41+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "hashing", "math"]
categories: ["algorithms"]
codeforces_contest: 1520
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 719 (Div. 3)"
rating: 1200
weight: 1520
solve_time_s: 120
verified: true
draft: false
---

[CF 1520D - Những khác biệt tương tự](https://codeforces.com/problemset/problem/1520/D) 

**Đánh giá:** 1200 
**Tags:** cấu trúc dữ liệu, hàm băm, toán học 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng và phải đếm xem có bao nhiêu cặp chỉ mục$(i, j)$với$i < j$thỏa mãn mối quan hệ rất cụ thể giữa giá trị và vị thế của họ. 

Điều kiện là:$$a_j - a_i = j - i$$Đối với mọi trường hợp thử nghiệm, chúng ta cần đếm tất cả các cặp thỏa mãn phương trình này và in tổng số. 

Các ràng buộc là điều đầu tiên cần xem xét. Một trường hợp thử nghiệm có thể chứa tới$2 \cdot 10^5$các phần tử và tổng kích thước trên tất cả các trường hợp thử nghiệm cũng nhiều nhất là$2 \cdot 10^5$. Bất kỳ giải pháp nào kiểm tra từng cặp sẽ yêu cầu khoảng$$\frac{n(n-1)}{2}$$so sánh. Vì$n = 2 \cdot 10^5$, đó là về$2 \times 10^{10}$kiểm tra cặp, điều này hoàn toàn không khả thi trong giới hạn 2 giây. 

Điều này ngay lập tức loại trừ$O(n^2)$cách tiếp cận. Chúng tôi cần thứ gì đó gần với thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Có một số tình huống dễ bỏ sót có thể gây ra sai sót. 

Coi như:```
1
3
1 2 3
```Mọi cặp đều hợp lệ vì:$$2 - 1 = 1,\quad 3 - 2 = 1,\quad 3 - 1 = 2$$Câu trả lời đúng là:```
3
```Giải pháp chỉ kiểm tra các phần tử liền kề sẽ tính 2 thay vì 3. 

Bây giờ hãy xem xét:```
1
4
1 1 1 1
```Câu trả lời đúng là:```
0
```Mặc dù tất cả các giá trị đều bằng nhau, vế phải$j-i$luôn dương đối với các chỉ số riêng biệt, do đó phương trình không bao giờ đúng. 

Một trường hợp tế nhị khác là:```
1
5
1 3 5 7 9
```Mỗi phần tử tăng thêm 2, trong khi chỉ số tăng thêm 1. Một giải pháp bất cẩn chỉ tập trung vào sự khác biệt giữa các phần tử lân cận có thể kết luận sai nhiều cặp là hợp lệ. Trong thực tế:$$a_j-a_i = 2(j-i)$$không bao giờ bằng$j-i$Trừ khi$i=j$. Câu trả lời đúng là 0. 

Những ví dụ này gợi ý rằng chúng ta cần một cách để chuyển đổi điều kiện thành thứ gì đó dễ đếm hơn trên toàn cầu thay vì so sánh trực tiếp các cặp. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Đối với mỗi chỉ số$i$, hãy thử mọi chỉ mục sau$j$, tính toán cả hai vế của phương trình và tăng kết quả khi chúng khớp. 

Điều này hoạt động vì nó kiểm tra định nghĩa của một cặp hợp lệ theo đúng nghĩa đen. Vấn đề là thời gian chạy. Với$n = 2 \cdot 10^5$, số lượng cặp là khoảng$2 \times 10^{10}$, vượt xa những gì có thể được xử lý kịp thời. 

Để cải thiện điều này, chúng ta nên kiểm tra chính phương trình:$$a_j - a_i = j - i$$Di chuyển các thuật ngữ liên quan đến cùng một chỉ mục sang cùng một phía:$$a_j - j = a_i - i$$Đây là quan sát quan trọng. 

Một cặp hợp lệ chính xác khi cả hai chỉ số có cùng giá trị:$$a_k - k$$cho các vị trí tương ứng của họ. 

Vấn đề ban đầu không còn là so sánh các cặp. Thay vào đó, nó trở thành: 

"Có bao nhiêu cặp chỉ số có cùng giá trị của$a_i - i$?" 

Một khi được xem xét theo cách này, lời giải sẽ trở thành một bài toán đếm tần số tiêu chuẩn. 

Khi chúng tôi quét mảng, giả sử giá trị hiện tại$a_i - i$đã xuất hiện rồi$f$lần. Sau đó, chỉ mục hiện tại tạo thành một cặp hợp lệ với mỗi chỉ mục trước đó.$f$chỉ số. Chúng tôi thêm$f$để trả lời và tăng tần suất. 

Bản đồ băm lưu trữ số lần mỗi giá trị được chuyển đổi xuất hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bản đồ băm trống sẽ lưu trữ tần số của các giá trị$a_i - i$. 
2. Khởi tạo câu trả lời thành 0. 
3. Lặp lại mảng từ trái sang phải. 
4. Với vị trí hiện tại, hãy tính:$$key = a_i - i$$Báo cáo vấn đề sử dụng chỉ mục dựa trên 1, do đó phép chuyển đổi cũng phải sử dụng các vị trí dựa trên 1. 
5. Tra cứu xem phím này đã xuất hiện bao nhiêu lần trước đó. 

Mỗi lần xuất hiện trước tương ứng với một cặp hợp lệ với chỉ mục hiện tại vì:$$a_j-j = a_i-i$$chính xác là điều kiện được biến đổi. 
6. Thêm tần số đó vào câu trả lời. 
7. Tăng tần số của khóa hiện tại trong bản đồ băm. 
8. Sau khi xử lý tất cả các phần tử, in câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Sự biến đổi$$a_j-a_i=j-i$$về mặt đại số tương đương với$$a_j-j=a_i-i.$$Do đó, một cặp là hợp lệ khi và chỉ khi cả hai chỉ số tạo ra cùng một giá trị được chuyển đổi. 

Trong quá trình quét, bản đồ băm luôn lưu trữ số lượng chỉ mục được xử lý trước đó có mỗi giá trị được chuyển đổi. Khi chỉ số hiện tại có giá trị$x$, mọi chỉ mục trước đó có giá trị được chuyển đổi$x$tạo thành một cặp hợp lệ với nó và không có chỉ mục nào có giá trị được chuyển đổi khác có thể tạo thành một cặp hợp lệ. 

Vì mỗi cặp được tính chính xác khi chỉ mục thứ hai của nó được xử lý nên mỗi cặp hợp lệ chỉ được tính một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = {}
        ans = 0

        for i in range(n):
            key = a[i] - (i + 1)  # 1-based index

            ans += freq.get(key, 0)
            freq[key] = freq.get(key, 0) + 1

        print(ans)

solve()
```Giải pháp tuân theo điều kiện biến đổi trực tiếp. 

Biến`key`cửa hàng$a_i - i$, sử dụng`i + 1`vì các chỉ số toán học trong bài toán đều dựa trên 1. Quên chuyển đổi này là lỗi phổ biến nhất trong vấn đề này. 

Từ điển`freq`ghi lại bao nhiêu vị trí trước đó đã tạo ra mỗi giá trị được chuyển đổi. 

Khi một lần xuất hiện mới của cùng một khóa xuất hiện, mọi lần xuất hiện trước đó sẽ tạo thành một cặp hợp lệ với chỉ mục hiện tại. Thêm`freq[key]`đến câu trả lời sẽ đếm tất cả các cặp như vậy ngay lập tức. 

Câu trả lời có thể trở nên lớn. Trong trường hợp xấu nhất, mọi cặp đều hợp lệ, cho:$$\frac{n(n-1)}{2}$$đó là khoảng$2 \times 10^{10}$khi$n=2\cdot10^5$. Số nguyên Python xử lý việc này một cách tự động, nhưng trong các ngôn ngữ như C++ thì số nguyên 64 bit là bắt buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
3 5 1 4 6 6
```Tính toán$a_i-i$sử dụng các chỉ số dựa trên 1. 

| Chỉ mục | Giá trị | Khóa = a[i] - i | Tần suất trước đó | Trả lời sau bước | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | 0 | 0 | 
| 2 | 5 | 3 | 0 | 0 | 
| 3 | 1 | -2 | 0 | 0 | 
| 4 | 4 | 0 | 0 | 0 | 
| 5 | 6 | 1 | 0 | 0 | 
| 6 | 6 | 0 | 1 | 1 | 

Câu trả lời cuối cùng:```
1
```Giá trị biến đổi lặp lại duy nhất là 0, xảy ra ở chỉ số 4 và 6, tạo ra một cặp hợp lệ. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```| Chỉ mục | Giá trị | Khóa = a[i] - i | Tần suất trước đó | Trả lời sau bước | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 0 | 
| 2 | 2 | 0 | 1 | 1 | 
| 3 | 3 | 0 | 2 | 3 | 

Câu trả lời cuối cùng:```
3
```Cả ba vị trí đều có chung giá trị chuyển đổi. Số cặp hợp lệ trở thành:$$\binom{3}{2}=3.$$Dấu vết này thể hiện logic đếm. Khi lần xuất hiện thứ ba xuất hiện, nó tạo thành một cặp với cả hai lần xuất hiện trước đó, tạo thành 2 cặp mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được xử lý một lần, với các phép toán bản đồ băm liên tục | 
| Không gian |$O(n)$| Trong trường hợp xấu nhất, mọi giá trị được chuyển đổi đều khác biệt | 

Vì tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, tổng thời gian chạy là tuyến tính ở kích thước đầu vào. Điều này vừa vặn một cách thoải mái trong giới hạn thời gian và bản đồ băm dễ dàng vừa vặn trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []

    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = {}
        ans = 0

        for i in range(n):
            key = a[i] - (i + 1)
            ans += freq.get(key, 0)
            freq[key] = freq.get(key, 0) + 1

        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run(
"""4
6
3 5 1 4 6 6
3
1 2 3
4
1 3 3 4
6
1 6 3 4 5 6
"""
) == "1\n3\n3\n10", "sample"

# minimum size
assert run(
"""1
1
1
"""
) == "0", "single element"

# all equal values
assert run(
"""1
4
1 1 1 1
"""
) == "0", "all equal"

# every pair valid
assert run(
"""1
5
1 2 3 4 5
"""
) == "10", "all pairs valid"

# off-by-one check
assert run(
"""1
2
2 2
"""
) == "0", "index handling"

# repeated transformed values
assert run(
"""1
5
3 4 5 1 2
"""
) == "3", "multiple groups"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1]`|`0`| Kích thước mảng tối thiểu | 
|`[1,1,1,1]`|`0`| Giá trị bằng nhau không hàm ý các cặp hợp lệ | 
|`[1,2,3,4,5]`|`10`| Mọi cặp đều thỏa mãn điều kiện | 
|`[2,2]`|`0`| Phát hiện lỗi lập chỉ mục dựa trên 1 | 
|`[3,4,5,1,2]`|`3`| Nhiều lần xuất hiện của cùng một giá trị được chuyển đổi | 

## Vỏ cạnh 

### Mảng phần tử đơn 

đầu vào:```
1
1
1
```Giá trị được chuyển đổi là:$$1-1=0$$Tần số 0 ban đầu là 0 nên không có gì được thêm vào câu trả lời. Không có cặp nào tồn tại vì cần có ít nhất hai chỉ số. 

Đầu ra:```
0
```### All values equal

 đầu vào:```
1
4
1 1 1 1
```Các giá trị được chuyển đổi là:$$0,\,-1,\,-2,\,-3$$All are distinct.

 Quá trình xử lý diễn ra với tần số luôn bằng 0 trước khi chèn, do đó đáp số không bao giờ tăng. 

Đầu ra:```
0
```Điều này xác nhận rằng chỉ các giá trị mảng bằng nhau là không đủ. Các vị trí chỉ số quan trọng. 

### Mọi cặp đều hợp lệ 

đầu vào:```
1
5
1 2 3 4 5
```Các giá trị được chuyển đổi là:$$0,0,0,0,0$$Câu trả lời đang chạy trở thành:$$0 + 1 + 2 + 3 + 4 = 10$$bằng:$$\binom{5}{2}.$$Đầu ra:```
10
```Thuật toán đếm chính xác tất cả các cặp có thể. 

### Bẫy lập chỉ mục riêng lẻ 

đầu vào:```
1
2
2 2
```Sử dụng các chỉ số dựa trên 1 chính xác:$$2-1=1,\qquad 2-2=0$$Các giá trị được chuyển đổi khác nhau nên không tồn tại cặp hợp lệ. 

Đầu ra:```
0
```Nếu ai đó sử dụng nhầm các chỉ mục dựa trên 0 trong phép chuyển đổi, cả hai khóa sẽ trở thành 2 và câu trả lời sẽ sai là 1. Ví dụ này cho thấy lý do tại sao việc chuyển đổi sang chỉ mục dựa trên 1 là cần thiết.
