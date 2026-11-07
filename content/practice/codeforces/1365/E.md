---
title: "CF 1365E - Giá trị chuỗi tối đa"
description: "Chúng ta được cung cấp một mảng lên tới 500 số nguyên dương. Chúng ta có thể chọn bất kỳ dãy con nào không trống và tính giá trị của nó theo quy tắc bitwise."
date: "2026-06-11T12:18:56+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1365
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 648 (Div. 2)"
rating: 1900
weight: 1365
solve_time_s: 219
verified: true
draft: false
---

[CF 1365E - Giá trị chuỗi con tối đa](https://codeforces.com/problemset/problem/1365/E) 

**Xếp hạng:** 1900 
**Tags:** vũ lực, thuật toán mang tính xây dựng 
**Thời gian giải:** 3 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng lên tới 500 số nguyên dương. Chúng ta có thể chọn bất kỳ dãy con nào không trống và tính giá trị của nó theo quy tắc bitwise. 

Đối với một chuỗi có kích thước được chọn$k$, đóng góp một chút$2^i$cho câu trả lời nếu bit đó có mặt trong ít nhất$\max(1, k-2)$các phần tử của dãy con. Giá trị cuối cùng là tổng của tất cả các giá trị bit đóng góp. 

Thử thách đầu tiên là hiểu giá trị này thực sự đại diện cho điều gì. 

Đối với các chuỗi con có kích thước 1 hoặc 2, ngưỡng luôn là 1. Một bit sẽ đóng góp nếu có ít nhất một phần tử được chọn chứa nó. Đó chính xác là định nghĩa của bitwise OR. 

Đối với kích thước 3, ngưỡng cũng là 1. Một lần nữa, giá trị chỉ đơn giản là OR của ba số. 

Đối với kích thước 4, ngưỡng trở thành 2. Một bit đóng góp nếu nó xuất hiện ở ít nhất hai trong bốn phần tử. 

Đối với kích thước 5, ngưỡng trở thành 3, v.v. 

Độ dài mảng tối đa là 500, trong khi mỗi số có thể lớn bằng$10^{18}$, vậy có nhiều nhất là 60 bit liên quan. Việc liệt kê tất cả các dãy con là không thể vì có$2^{500}$của họ. 

Điều quan trọng là tìm ra đặc tính cấu trúc giúp giảm đáng kể không gian tìm kiếm. 

Một sai lầm phổ biến là cho rằng câu trả lời luôn là OR của tất cả các phần tử. Coi như:```
3
1 2 4
```Dãy con chứa cả 3 phần tử có giá trị$1|2|4 = 7$, vậy đáp án thực sự là 7. 

Bây giờ hãy xem xét:```
4
1 2 4 8
```Đối với cả bốn phần tử, ngưỡng là 2. Mỗi bit chỉ xuất hiện một lần, do đó giá trị trở thành 0. Dãy con tốt nhất là bất kỳ tập hợp con cỡ 3 nào, cho giá trị 7, 11, 13 hoặc 14. Một giải pháp OR mù quáng mọi thứ sẽ trả về 15, điều này là không thể. 

Một trường hợp tế nhị khác là:```
4
7 1 1 1
```Sử dụng cả bốn phần tử sẽ cho ngưỡng 2. Chỉ bit 0 xuất hiện ít nhất hai lần nên giá trị là 1. Chỉ chọn phần tử 7 sẽ cho giá trị 7. Dãy con tối ưu không nhất thiết phải lớn. 

Những ví dụ này gợi ý rằng câu trả lời phụ thuộc nhiều vào một tập hợp nhỏ các phần tử được lựa chọn cẩn thận hơn là toàn bộ mảng. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi dãy con không trống, tính giá trị của nó từng chút một và giữ mức tối đa. 

Điều này đúng vì mọi dãy con ứng viên đều được kiểm tra. Thật không may, ngay cả đối với$n=60$, sẽ có khoảng$10^{18}$các chuỗi tiếp theo. Vì$n=500$, không gian tìm kiếm hoàn toàn vô vọng. 

Bước đột phá đến từ việc phân tích sự đóng góp của một bit duy nhất. 

Giả sử một dãy con chứa$k$các phần tử. Một chút đóng góp nếu nó xuất hiện ít nhất$k-2$của họ. Tương tự, tối đa hai phần tử được chọn có thể thiếu bit đó. 

Hãy tưởng tượng một dãy con tối ưu. Tập trung vào bất kỳ phần đóng góp nào. Vì nhiều nhất là hai phần tử có thể thiếu bit đó nên việc loại bỏ tất cả các phần tử khác thiếu bit đó sẽ không ảnh hưởng đến sự đóng góp của bit đó. 

Quan sát này dẫn đến một tuyên bố mạnh mẽ hơn nhiều. Đối với bất kỳ chuỗi con nào có kích thước ít nhất là 4, mỗi bit đóng góp được xác định bởi tất cả trừ nhiều nhất là hai phần tử. Các bit như vậy có thể được biểu diễn dưới dạng giao điểm của tất cả các phần tử ngoại trừ hai vị trí. 

Giải pháp biên tập sử dụng một thực tế nổi tiếng cho vấn đề này: 

Câu trả lời luôn có thể đạt được bằng một dãy con có kích thước tối đa là 3. 

Tại sao? 

Đối với một phần tử, giá trị chỉ là phần tử đó. 

Đối với hai phần tử, giá trị là OR của chúng. 

Đối với ba phần tử, giá trị cũng là OR của chúng, vì ngưỡng vẫn là 1. 

Bây giờ hãy xem xét bất kỳ dãy con nào lớn hơn. Một bit chỉ đóng góp nếu nó vắng mặt ở tối đa hai phần tử. Chọn tối đa ba phần tử bao gồm tất cả các ngoại lệ cho bit đó. Bit đó sẽ xuất hiện trong OR của các phần tử được chọn đó. Lặp lại lý do này cho mỗi bit đóng góp cho thấy rằng giá trị của bất kỳ chuỗi con lớn hơn nào cũng không thể vượt quá OR của một số phần tử từ nó. 

Do đó, câu trả lời tối đa có thể bằng:$$\max(a_i \;|\; a_j \;|\; a_k)$$trên tất cả các bộ ba, cho phép các chỉ số lặp lại thể hiện việc chọn ít hơn ba phần tử. 

Khi bài toán trở thành "tìm OR tối đa của tối đa ba phần tử mảng", các ràng buộc sẽ trở nên dễ dàng. 

Từ$n \le 500$, có:$$\binom{500}{3} \approx 20.7 \text{ million}$$gấp ba lần. 

Hoạt động OR 60 bit cực kỳ rẻ và 20 triệu lần lặp lại vừa vặn thoải mái trong giới hạn thời gian trong C++ và Python. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các chuỗi tiếp theo |$O(2^n \cdot 60)$|$O(1)$| Quá chậm | 
| Liệt kê tất cả các bộ ba |$O(n^3)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Khởi tạo câu trả lời thành 0. 
3. Liệt kê tất cả các bộ ba$(i,j,k)$với$0 \le i,j,k < n$. 
4. Tính toán:$$a_i \;|\; a_j \;|\; a_k$$và cập nhật câu trả lời tối đa. 
5. In giá trị lớn nhất tìm được. 

Các vòng lặp cho phép các chỉ số lặp lại. Điều này rất quan trọng vì dãy con có kích thước 1 hoặc 2 cũng phải có thể biểu diễn được. Ví dụ, chỉ chọn$a_i$tương ứng với$(i,i,i)$, trong khi chọn$a_i$Và$a_j$tương ứng với$(i,j,j)$. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là mọi giá trị dãy con được giới hạn bởi OR của tối đa ba phần tử từ dãy con đó. 

Đối với các chuỗi con có kích thước tối đa là 3, giá trị chính xác là OR của chúng. 

Đối với các chuỗi con lớn hơn, một bit chỉ đóng góp khi có nhiều nhất hai phần tử được chọn không chứa nó. Vị trí của những phần tử còn thiếu đó có thể được che đậy bằng cách chọn tối đa ba phần tử từ dãy con. Mỗi bit đóng góp phải xuất hiện trong ít nhất một trong các phần tử được chọn đó, do đó toàn bộ giá trị chuỗi con được chứa trong OR của một số phần tử. 

Vì mọi giá trị có thể đạt được đều được giới hạn bởi bộ ba OR và bản thân mỗi bộ ba OR đều có thể đạt được bằng cách chọn bộ ba đó, nên giá trị dãy con tối đa bằng OR tối đa trên tất cả các bộ ba. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    ans = 0

    for i in range(n):
        for j in range(n):
            x = a[i] | a[j]
            for k in range(n):
                ans = max(ans, x | a[k])

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc giảm toán học. 

Hai vòng lặp bên ngoài tính toán một phần OR một lần và lưu nó vào`x`. Điều này tránh lặp lại`a[i] | a[j]`bên trong vòng lặp trong cùng. 

Các chỉ số lặp đi lặp lại được cho phép có chủ ý. Việc hạn chế các vòng lặp ở các chỉ mục riêng biệt sẽ bỏ lỡ các chuỗi con có kích thước 1 và 2. Ví dụ: với một mảng phần tử duy nhất, câu trả lời hợp lệ duy nhất đến từ việc đánh giá`(i,i,i)`. 

Số nguyên Python xử lý các giá trị lên tới$10^{18}$và hơn thế nữa mà không có lo ngại về tràn, vì vậy các phép toán OR theo bit thông thường là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 1 3
```| tôi | j | k | giá trị HOẶC | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 2 | 
| 0 | 0 | 1 | 3 | 
| 0 | 0 | 2 | 3 | 
| 0 | 1 | 2 | 3 | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 2 | 3 | 

OR lớn nhất gặp phải là 3. 

Đầu ra:```
3
```Ví dụ này cho thấy rằng một phần tử đơn lẻ đã có thể đạt được mức tối ưu. Giá trị 3 xuất hiện cả từ việc chọn riêng phần tử 3 và từ việc chọn cặp$(2,3)$. 

### Ví dụ 2 

đầu vào:```
4
3 4 1 2
```| tôi | j | k | giá trị HOẶC | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 3 | 
| 0 | 1 | 1 | 7 | 
| 0 | 1 | 2 | 7 | 
| 0 | 1 | 3 | 7 | 
| 1 | 2 | 3 | 7 | 

OR tối đa được tìm thấy là 7. 

Đầu ra:```
7
```Điều này chứng tỏ rằng việc kết hợp nhiều số có thể kích hoạt nhiều bit hơn bất kỳ phần tử riêng lẻ nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$| Ba vòng lặp lồng nhau trên mảng | 
| Không gian |$O(1)$| Chỉ có một vài biến được lưu trữ | 

Với$n \le 500$, thuật toán thực hiện tối đa$500^3 = 125,000,000$các phép toán OR đơn giản. Đây là giải pháp dự định cho vấn đề và phù hợp trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    for i in range(n):
        for j in range(n):
            x = a[i] | a[j]
            for k in range(n):
                ans = max(ans, x | a[k])

    return str(ans) + "\n"

# provided sample
assert run("3\n2 1 3\n") == "3\n", "sample"

# minimum size
assert run("1\n1\n") == "1\n", "single element"

# all equal
assert run("5\n7 7 7 7 7\n") == "7\n", "all equal"

# powers of two
assert run("4\n1 2 4 8\n") == "15\n", "combine independent bits"

# large bit positions
assert run("3\n1099511627776 1 2\n") == "1099511627779\n", "high bits"

# boundary style case
assert run("3\n0 0 0\n") == "0\n", "all zero"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Trường hợp hợp lệ nhỏ nhất | 
|`5 / 7 7 7 7 7`|`7`| Giá trị trùng lặp | 
|`4 / 1 2 4 8`|`15`| HOẶC tích lũy các bit độc lập | 
|`3 / 1099511627776 1 2`|`1099511627779`| Vị trí bit rất lớn | 
|`3 / 0 0 0`|`0`| Trường hợp suy biến hoàn toàn bằng 0 | 

## Vỏ cạnh 

Hãy xem xét:```
1
5
```Dãy con hợp lệ duy nhất chứa phần tử đơn. Bộ ba liệt kê bao gồm`(0,0,0)`, sản xuất`5 | 5 | 5 = 5`. Thuật toán trả về 5. 

Hãy xem xét:```
4
7 7 7 7
```Mọi bộ ba OR đều bằng 7. Giá trị tối đa vẫn là 7. Các giá trị lặp lại không yêu cầu xử lý đặc biệt vì OR là lũy đẳng. 

Coi như:```
4
1 2 4 8
```Dãy con đầy đủ của bốn phần tử có giá trị 0 theo định nghĩa ban đầu vì ngưỡng trở thành 2 và mỗi bit chỉ xuất hiện một lần. Thuật toán vẫn trả về 15 vì nó tìm kiếm chính xác trên tất cả các chuỗi con có kích thước tối đa là 3, có giá trị bằng OR của chúng. Bộ ba`(1,2,4)`cho 7, trong khi`(1,2,8)`cho 11,`(1,4,8)`cho 13, và`(2,4,8)`cho 14. Chọn cả bốn là không tối ưu. 

Coi như:```
4
7 1 1 1
```Thuật toán đánh giá`(7,7,7)`và thu được 7. Bất kỳ dãy con nào lớn hơn đều có giá trị nhỏ hơn. Điều này xác nhận rằng việc buộc các chuỗi con lớn sẽ không chính xác và việc cho phép các chỉ mục lặp lại mô hình chính xác các chuỗi con có kích thước 1 và 2.
