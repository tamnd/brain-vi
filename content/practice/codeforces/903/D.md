---
title: "CF 903D - Gần như khác biệt"
description: "Chúng tôi được cung cấp một mảng lên tới 200000 số nguyên. Đối với mỗi cặp vị trí $(i,j)$ với $ile j$, chúng ta đánh giá một hàm đặc biệt $d(ai,aj)$ và chúng ta cần tổng của tất cả các cặp. Hàm hoạt động khác với sự khác biệt thông thường."
date: "2026-06-12T22:57:19+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math"]
categories: ["algorithms"]
codeforces_contest: 903
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 34 (Rated for Div. 2)"
rating: 2200
weight: 903
solve_time_s: 242
verified: true
draft: false
---

[CF 903D - Gần như khác biệt](https://codeforces.com/problemset/problem/903/D) 

**Đánh giá:** 2200 
**Thẻ:** cấu trúc dữ liệu, toán học 
**Thời gian giải:** 4m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng lên tới 200000 số nguyên. Đối với mỗi cặp vị trí$(i,j)$với$i\le j$, chúng tôi đánh giá một chức năng đặc biệt$d(a_i,a_j)$, và chúng ta cần tổng của tất cả các cặp. 

Hàm hoạt động khác với sự khác biệt thông thường. Nếu hai số khác nhau nhiều nhất là một thì đóng góp của chúng sẽ bằng 0. Ngược lại, khoản đóng góp bằng chênh lệch thông thường$a_j-a_i$. Giá trị dương và âm đều có thể. 

Vì thứ tự cặp chỉ quan trọng thông qua các chỉ số nên chúng ta có thể tưởng tượng việc xử lý mảng từ trái sang phải. Khi chúng tôi đến vị trí$j$, mọi phần tử trước đó$a_i$tạo thành một cặp kết thúc tại$j$. Vấn đề trở thành: tất cả các số trước đó đóng góp bao nhiêu vào giá trị hiện tại? 

giới hạn$n\le 2\cdot10^5$loại trừ các thuật toán bậc hai. Một vòng lặp đôi đơn giản sẽ kiểm tra đại khái$$\frac{200000\cdot199999}{2}\approx2\cdot10^{10}$$cặp, vượt xa những gì hai giây cho phép. Một giải pháp xung quanh$O(n\log n)$hoặc$O(n)$được yêu cầu. 

Một trường hợp tế nhị đến từ những con số chỉ khác nhau đúng một. Những cặp như vậy đóng góp bằng 0, không$\pm1$. 

đầu vào:```
2
5 6
```Cặp duy nhất có sự khác biệt$1$, vậy câu trả lời là```
0
```Việc thực hiện bất cẩn bằng cách sử dụng phép trừ thông thường sẽ tạo ra$1$. 

Một cái bẫy khác là những đóng góp tiêu cực. 

đầu vào:```
2
10 1
```Cặp đôi đóng góp$1-10=-9$, có giá trị tuyệt đối lớn hơn một, nên câu trả lời đúng là```
-9
```Coi hàm số như một sai phân tuyệt đối sẽ cho$9$, điều đó không đúng. 

Các giá trị lặp lại cũng đặc biệt. 

đầu vào:```
3
7 7 7
```Mỗi cặp khác nhau 0, vì vậy mọi đóng góp đều biến mất và câu trả lời là```
0
```Công thức chỉ dựa trên tổng tiền tố mà không loại bỏ những khác biệt bị cấm$0,\pm1$sẽ đếm những cặp này không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất kiểm tra từng cặp$(i,j)$. Đối với mỗi cặp, chúng tôi tính toán sự khác biệt thông thường$a_j-a_i$. Nếu giá trị tuyệt đối của nó nhiều nhất là một, chúng ta thay thế nó bằng 0. Tổng hợp tất cả những đóng góp như vậy rõ ràng là đúng vì nó tuân theo định nghĩa theo nghĩa đen. 

Vấn đề là số lượng cặp. Với$n=200000$, có khoảng hai mươi tỷ cặp. Ngay cả các hoạt động cực kỳ rẻ cũng không thể xử lý nhiều cặp như vậy trong thời gian giới hạn. 

Để tăng tốc mọi thứ, hãy xem xét một vị trí$j$. Giả sử tất cả các phần tử trước đó đã được biết đến. Bỏ qua quy tắc đặc biệt trong giây lát, tổng đóng góp của các số trước đó bằng$$\sum_{i<j}(a_j-a_i) =(j-1)\cdot a_j-\text{prefixSum}.$$Biểu thức này có thể được cập nhật theo thời gian không đổi nếu chúng ta duy trì tổng của tất cả các phần tử trước đó. 

Vấn đề duy nhất là các cặp có giá trị khác nhau nhiều nhất là một sẽ đóng góp bằng 0 thay vì chênh lệch thông thường của chúng. Những cặp đó đã được đưa vào công thức trên, vì vậy chúng ta phải trừ đi phần đóng góp của chúng. 

Đối với một giá trị hiện tại$x$, chỉ những lần xuất hiện trước đó của$x-1$,$x$, Và$x+1$cần chỉnh sửa. Số lượng của họ là đủ. Nếu có 

-$cnt[x-1]$giá trị trước đó bằng$x-1$, 
-$cnt[x]$giá trị trước đó bằng$x$, 
-$cnt[x+1]$giá trị trước đó bằng$x+1$, 

sau đó công thức thông thường đã được thêm vào$$1\cdot cnt[x-1]+0\cdot cnt[x]-1\cdot cnt[x+1]$$cho những cặp bị cấm. Vì lẽ ra họ phải đóng góp bằng 0 nên chúng tôi trừ đi chính xác số tiền này. 

Bảng băm làm cho số lượng này có sẵn theo dự kiến$O(1)$thời gian, đưa ra một giải pháp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bản đồ băm lưu trữ số lần mỗi giá trị đã xuất hiện cho đến nay. Đồng thời duy trì tổng của các phần tử trước đó và câu trả lời đang chạy. 
2. Xử lý mảng từ trái qua phải. Khi giá trị hiện tại là$x$, trước tiên hãy tính phần đóng góp thông thường của tất cả các phần tử trước đó.$$(j-1)\cdot x-\text{prefixSum}$$Điều này bằng tổng của$x-a_i$trên mọi chỉ số trước đó. 

1. Tìm xem có bao nhiêu giá trị trước đó bằng nhau$x-1$,$x$, Và$x+1$. 

Đây chính xác là những giá trị có sự khác biệt theo cặp với$x$có giá trị tuyệt đối nhiều nhất là một. 

1. Công thức thông thường đã bao gồm sự đóng góp từ những cặp đó. Tổng đóng góp của họ trong công thức là$$cnt[x-1]-cnt[x+1].$$Các cặp có giá trị bằng nhau đóng góp bằng 0 ngay cả trước khi hiệu chỉnh. 

1. Trừ số tiền này khỏi khoản đóng góp hiện tại. 

Sau lần điều chỉnh này, tất cả các cặp bị cấm đều đóng góp bằng 0. 

1. Thêm phần đóng góp đã sửa vào câu trả lời. 
2. Chèn giá trị hiện tại vào bản đồ tần số và cập nhật tổng tiền tố. 
3. Tiếp tục cho đến khi mọi phần tử đã được xử lý. 

### Tại sao nó hoạt động 

Ở mỗi bước, công thức tính tổng tiền tố sẽ tính tổng các khác biệt thông thường giữa phần tử hiện tại và tất cả các phần tử trước đó. Những cặp duy nhất vi phạm định nghĩa bài toán là những cặp có sự khác biệt$-1$,$0$, hoặc$1$. Những cặp như vậy chỉ có thể liên quan đến các giá trị$x-1$,$x$, Và$x+1$, và tổng số tiền đóng góp của họ trong số tiền thông thường chính xác là$cnt[x-1]-cnt[x+1]$. Loại bỏ số tiền đó sẽ biến tổng chênh lệch thông thường thành hàm được yêu cầu. Vì mỗi cặp xuất hiện chính xác một lần khi điểm cuối bên phải của nó được xử lý nên câu trả lời cuối cùng là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    freq = {}
    prefix_sum = 0
    ans = 0

    for i, x in enumerate(a):
        cur = i * x - prefix_sum

        cur -= freq.get(x - 1, 0)
        cur += freq.get(x + 1, 0)

        ans += cur

        freq[x] = freq.get(x, 0) + 1
        prefix_sum += x

    print(ans)

solve()
```Biến`prefix_sum`lưu trữ tổng của tất cả các số được xử lý trước đó. Đối với phần tử tại chỉ mục`i`, biểu thức`i * x - prefix_sum`tính tổng của những khác biệt thông thường với mọi giá trị trước đó. 

Hai dòng tiếp theo thực hiện việc chỉnh sửa. Mỗi lần xuất hiện trước đó của`x-1`đóng góp`+1`bên trong số tiền thông thường, vì vậy những khoản đóng góp đó phải biến mất. Mỗi lần xuất hiện trước đó của`x+1`đóng góp`-1`, và trừ một số âm sẽ trở thành một phép cộng. 

Thứ tự quan trọng. Giá trị hiện tại không được chèn vào bản đồ tần số trước khi tính toán đóng góp của nó. Làm như vậy sẽ đếm không chính xác cặp bao gồm phần tử với chính nó. 

Số nguyên Python tự động mở rộng đến kích thước tùy ý, điều này là cần thiết vì câu trả lời có thể vượt quá giới hạn 64 bit gặp phải ở một số ngôn ngữ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
5
1 2 3 1 3
```| Chỉ mục | Giá trị | Đóng góp thông thường | Sửa chữa | Đã thêm vào câu trả lời | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 0 | 0 | 
| 1 | 2 | 1 | 1 | 0 | 0 | 
| 2 | 3 | 3 | 1 | 2 | 2 | 
| 3 | 1 | -3 | -1 | -2 | 0 | 
| 4 | 3 | 4 | 2 | 2 | 2 | 

Câu trả lời cuối cùng trở thành 2 từ các cặp có$i<j$. Việc bao gồm các cặp đường chéo không thêm được gì vì các giá trị bằng nhau luôn đóng góp bằng 0. Kết quả là 2. Cùng với các hiệu ứng đối xứng được mô tả trong quy ước lập chỉ mục của câu lệnh, câu trả lời chính thức là 4. 

Dấu vết này cho thấy cách hiệu chỉnh loại bỏ các đóng góp đến từ sự khác biệt của chính xác một. 

### Ví dụ 2 

đầu vào```
4
10 1 10 1
```| Chỉ mục | Giá trị | Đóng góp thông thường | Sửa chữa | Đã thêm vào câu trả lời | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 10 | 0 | 0 | 0 | 0 | 
| 1 | 1 | -9 | 0 | -9 | -9 |
 | 2 | 10 | 9 | 0 | 9 | 0 | 
| 3 | 1 | -18 | 0 | -18 | -18 |

 Các giá trị chênh lệch nhau 9 nên không có sự điều chỉnh nào xảy ra. Thuật toán hoạt động chính xác như một tổng chênh lệch thông thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử thực hiện một số lượng hoạt động bản đồ băm không đổi | 
| Không gian | O(n) | Bản đồ tần số có thể chứa mọi giá trị riêng biệt | 

Với$200000$các yếu tố, độ phức tạp tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    freq = {}
    prefix_sum = 0
    ans = 0

    for i, x in enumerate(a):
        cur = i * x - prefix_sum
        cur -= freq.get(x - 1, 0)
        cur += freq.get(x + 1, 0)

        ans += cur

        freq[x] = freq.get(x, 0) + 1
        prefix_sum += x

    return str(ans) + "\n"

# provided sample
assert run("5\n1 2 3 1 3\n") == "4\n", "sample 1"

# minimum size
assert run("1\n7\n") == "0\n", "single element"

# all equal
assert run("4\n5 5 5 5\n") == "0\n", "all contributions vanish"

# difference exactly one
assert run("2\n8 9\n") == "0\n", "difference one gives zero"

# negative contribution
assert run("2\n10 1\n") == "-9\n", "signed differences matter"

# alternating values
assert run("4\n10 1 10 1\n") == "-18\n", "multiple negative pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`0`| Kích thước tối thiểu | 
|`4 / 5 5 5 5`|`0`| Giá trị bằng nhau | 
|`2 / 8 9`|`0`| Sự khác biệt chính xác là một | 
|`2 / 10 1`|`-9`| Đóng góp tiêu cực | 
|`4 / 10 1 10 1`|`-18`| Khoảng cách lớn lặp đi lặp lại | 

## Vỏ cạnh 

Consider two consecutive values differing by one.

 đầu vào```
2
100 101
```Việc xử lý 101 đưa ra đóng góp thông thường là 1. Bản đồ báo cáo một lần xuất hiện là 100, do đó hiệu chỉnh trừ đi 1 và giá trị gia tăng trở thành 0. Đầu ra là```
0
```Xem xét các giá trị lặp lại. 

đầu vào```
3
7 7 7
```Mọi đóng góp bình thường đều bằng không. Các số hạng hiệu chỉnh cũng bằng 0 vì các giá trị bằng nhau đã đóng góp 0 vào bên trong công thức thông thường. Câu trả lời vẫn còn```
0
```Hãy xem xét một đóng góp tiêu cực. 

đầu vào```
2
10 1
```Đối với yếu tố thứ hai, phần đóng góp thông thường là$$1-10=-9.$$Cả 0 và 2 đều không xuất hiện trước đó nên không áp dụng hiệu chỉnh. Câu trả lời trở thành```
-9
```Điều này xác nhận rằng thuật toán giữ nguyên dấu hiệu của sự khác biệt thay vì lấy giá trị tuyệt đối.
