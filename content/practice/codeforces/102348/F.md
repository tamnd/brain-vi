---
title: "CF 102348F - Số lượng sản phẩm"
description: "Chúng ta có một mảng gồm (n) số nguyên và mỗi mảng con liền kề đóng góp theo dấu của tích của nó. Đối với mỗi cặp điểm cuối (l le r), tích của (al,a{l+1},ldots,ar) là âm, 0 hoặc dương."
date: "2026-08-16T16:02:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "F"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 880
verified: false
draft: false
---

[CF 102348F - Số lượng sản phẩm](https://codeforces.com/problemset/problem/102348/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng gồm (n) số nguyên và mỗi mảng con liền kề đóng góp theo dấu của tích của nó. Đối với mỗi cặp điểm cuối (l \le r), tích của (a_l,a_{l+1},\ldots,a_r) là âm, 0 hoặc dương. Chúng ta cần đếm xem có bao nhiêu mảng con thuộc về mỗi danh mục và in ba số đếm theo thứ tự đó. 

Giá trị thực tế của một sản phẩm khác không là không liên quan. Chỉ có dấu hiệu của nó là quan trọng. Một tích số âm chính xác khi mảng con chứa số phần tử âm lẻ và nó dương khi nó chứa số phần tử âm chẵn. Bất kỳ mảng con nào chứa ít nhất một số 0 đều có tích bằng 0. 

Mảng có thể chứa tối đa (2\cdot10^5) phần tử. Thuật toán bậc hai sẽ kiểm tra khoảng (n(n+1)/2) mảng con, tức là khoảng (2\cdot10^{10}) khi (n=2\cdot10^5). Điều đó vượt xa những gì giới hạn 2 giây có thể xử lý. Chúng ta cần một giải pháp (O(n)) hoặc (O(n\log n)). Bản thân câu trả lời cũng có thể nằm trong khoảng (n(n+1)/2), do đó, số nguyên 64 bit là bắt buộc trong các ngôn ngữ có loại số nguyên có chiều rộng cố định. Số nguyên Python đã xử lý các giá trị này một cách an toàn. 

Số 0 cần được xử lý riêng vì số 0 sẽ phá hủy thông tin dấu hiệu. Ví dụ, với đầu vào```
1
0
```câu trả lời đúng là```
0 1 0
```Thuật toán chẵn lẻ dấu coi số 0 không phải là dương hay âm nhưng vẫn tiếp tục trạng thái tiền tố trước đó sẽ cho phép các mảng con vượt qua số 0 và phân loại chúng theo dấu cũ của chúng một cách không chính xác. Số 0 phải chia mảng thành các phân đoạn khác 0 độc lập. 

Một trường hợp dễ xử lý sai khác là một mảng chỉ chứa các giá trị dương. Vì```
3
1 1 1
```mỗi một trong (6) mảng con đều có tích dương, vì vậy câu trả lời là```
0 0 6
```Một phương pháp chỉ đếm các sản phẩm thay đổi dấu có thể quên đếm các tổ hợp phần tử đơn và độ dài chẵn của các phần tử dương. 

Các giá trị âm cũng yêu cầu đếm chính xác tiền tố trống. Vì```
3
-1 -1 -1
```câu trả lời đúng là```
4 0 2
```Các mảng con tích âm có độ dài lẻ, cho (3+1=4), trong khi hai mảng con có độ dài chẵn có tích dương. Quên tiền tố trước khi phần tử đầu tiên làm mất các mảng con bắt đầu từ chỉ mục (1). 

Cuối cùng, số không có thể xuất hiện ở ranh giới. Vì```
3
0 1 0
```câu trả lời đúng là```
0 5 1
```Có tổng cộng sáu mảng con và chỉ ([2,2]) có tích dương khác 0. Năm số còn lại chứa số 0. Bất kỳ giải pháp nào chỉ đếm các mảng con chứa số 0 khi nó gặp số 0 có thể bỏ lỡ các mảng con tiếp tục vượt quá số 0 đó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi cặp điểm cuối ((l,r)). Đối với điểm cuối bên trái cố định, chúng ta có thể mở rộng điểm cuối bên phải từng vị trí một và duy trì dấu hiệu sản phẩm hiện tại. Việc nhân các giá trị thực tế là không cần thiết, vì chỉ số phần tử âm là số lẻ hay số chẵn mới có thể được phát hiện ngay lập tức. 

Phương pháp brute-force này đúng vì mỗi mảng con liền kề được xem xét chính xác một lần. Vấn đề là số lượng mảng con. Có (n(n+1)/2) trong số chúng, đạt tới (20.000.100.000) cho (n=200.000). Ngay cả việc xử lý thời gian không đổi cho mỗi mảng con cũng là quá nhiều, do đó phương pháp bậc hai bị loại trừ. 

Quan sát quan trọng là dấu của một mảng con khác 0 chỉ phụ thuộc vào tính chẵn lẻ của số phần tử âm bên trong nó. Chúng ta có thể mã hóa tính chẵn lẻ này thành (0) cho số âm chẵn và (1) cho số lẻ. 

Hãy xem xét các chẵn lẻ tiền tố. Gọi (p_i) là số chẵn lẻ của số phần tử âm giữa đoạn khác 0 hiện tại cho đến vị trí (i). Đối với một mảng con từ (l) đến (r), số phần tử âm theo modulo (2) của nó là 

[ 
p_r \oplus p_{l-1}. 
] 

Do đó, nếu hai chẵn lẻ tiền tố bằng nhau thì mảng con có tích dương. Nếu chúng khác nhau thì mảng con có tích âm. 

Khi quét từ trái sang phải, chúng ta chỉ cần biết có bao nhiêu tiền tố trước đó có tính chẵn lẻ và chẵn lẻ. Nếu chẵn lẻ tiền tố hiện tại là chẵn thì mọi tiền tố chẵn trước đó tạo thành một mảng con tích dương kết thúc ở đây, trong khi mọi tiền tố lẻ trước đó tạo thành một mảng con tích âm. Tình hình sẽ đảo ngược khi số chẵn lẻ hiện tại là số lẻ. 

Zeros chỉ đơn giản là tách các phân đoạn độc lập. Khi số 0 xuất hiện, không có mảng con khác 0 hợp lệ nào có thể vượt qua nó, do đó số lượng chẵn lẻ tiền tố được đặt lại về trạng thái ban đầu. 

Khi tất cả các sản phẩm khác 0 đã được đếm, mọi mảng con còn lại phải có sản phẩm 0. Tổng cộng có (n(n+1)/2) mảng con, vì vậy 

[ 
\text{không} = 
\frac{n(n+1)}2-\text{âm bản}-\text{tích cực}. 
] 

Điều này tránh việc phải đếm trực tiếp các mảng con chứa 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(1)) | Quá chậm | 
| Tiền tố Dấu chẵn lẻ | (O(n)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với một tiền tố chẵn và không có tiền tố lẻ. Tiền tố ban đầu biểu thị chuỗi trống trước phần tử đầu tiên, có số giá trị âm bằng 0. 
2. Duy trì`even`Và`odd`, số lượng trạng thái tiền tố trước đó có tính chẵn lẻ số âm chẵn và lẻ. Cũng duy trì tính chẵn lẻ hiện tại, trong đó`0`có nghĩa là chẵn và`1`có nghĩa là kỳ quặc. 
3. Đối với mỗi phần tử mảng, trước tiên hãy kiểm tra xem nó có bằng 0 hay không. Nếu bằng 0, hãy đặt lại`even`đến (1),`odd`đến (0) và tính chẵn lẻ hiện tại thành chẵn. Số 0 phân tách các phần khác 0 của mảng, vì vậy các tiền tố trước nó không được ghép nối với các tiền tố sau nó. 
4. Nếu phần tử âm, lật tính chẵn lẻ hiện tại. Nếu phần tử là dương, hãy giữ nguyên tính chẵn lẻ. Chỉ có dấu quan trọng nên độ lớn của phần tử không bao giờ được đưa vào phép tính. 
5. Nếu số chẵn lẻ hiện tại là chẵn, hãy thêm`even`đến số dương và`odd`đến số âm. Mỗi tiền tố chẵn trước đó tạo ra số âm chẵn giữa hai tiền tố, trong khi mọi tiền tố lẻ trước đó tạo ra số lẻ. 
6. Nếu số chẵn lẻ hiện tại là số lẻ, hãy thêm`odd`đến số dương và`even`đến số âm. Hai trường hợp này bị đảo ngược vì các số chẵn lẻ tiền tố bằng nhau vẫn bị hủy đến mức chênh lệch chẵn. 
7. Thêm tiền tố hiện tại vào bộ đếm thích hợp. Điều này phải xảy ra sau khi đếm các mảng con kết thúc ở vị trí hiện tại, vì tiền tố hiện tại không thể được sử dụng làm ranh giới bên trái của mảng con kết thúc ở cùng một vị trí. 
8. Sau khi xử lý toàn bộ mảng, tính tổng số mảng con là (n(n+1)/2). Trừ số dương và số âm để thu được số có tích bằng 0. 

### Tại sao nó hoạt động 

Đối với mọi mảng con khác 0, dấu của nó được xác định bởi tính chẵn lẻ của số phần tử âm. Tính chẵn lẻ của số âm bên trong ([l,r]) là XOR của các số chẵn lẻ tiền tố ngay sau (r) và ngay trước (l). Do đó, các số chẵn lẻ tiền tố bằng nhau đại diện cho các sản phẩm dương, trong khi các số chẵn lẻ tiền tố khác nhau đại diện cho các sản phẩm âm. các quầy`even`Và`odd`chứa chính xác các tiền tố bắt buộc trước đó, vì vậy mọi mảng con khác 0 đều được tính một lần khi điểm cuối bên phải của nó được xử lý. Việc đặt lại sau số 0 sẽ ngăn không cho bất kỳ mảng con chứa số 0 nào nhập vào các số đếm này. Vì mỗi mảng con đều là dương, âm hoặc bằng 0 nên việc trừ hai số khác 0 khỏi tổng sẽ cho kết quả chính xác là số 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_products(a):
    n = len(a)

    # Number of previous prefix parities in the current nonzero segment.
    even = 1
    odd = 0

    parity = 0

    negative = 0
    positive = 0

    for x in a:
        if x == 0:
            # No nonzero subarray can cross a zero.
            even = 1
            odd = 0
            parity = 0
            continue

        if x < 0:
            parity ^= 1

        if parity == 0:
            positive += even
            negative += odd
            even += 1
        else:
            positive += odd
            negative += even
            odd += 1

    total = n * (n + 1) // 2
    zero = total - positive - negative

    return negative, zero, positive

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    negative, zero, positive = count_products(a)
    print(negative, zero, positive)

if __name__ == "__main__":
    solve()
```chức năng`count_products`chứa toàn bộ quá trình quét tuyến tính.`even = 1`đại diện cho tiền tố trước phần tử đầu tiên. Việc khởi tạo này cho phép đếm một mảng con bắt đầu ở vị trí mảng đầu tiên. 

Đối với giá trị âm,`parity ^= 1`lật ngược tính chẵn lẻ vì gặp phải thêm một phần tử âm. Các giá trị dương giữ nguyên tính chẵn lẻ. Độ lớn thực tế của một giá trị khác 0 không bao giờ được sử dụng. 

Những bổ sung cho`positive`Và`negative`xảy ra trước khi tăng`even`hoặc`odd`. Tại thời điểm đó, các bộ đếm đó chỉ mô tả các tiền tố ngay trước phần tử hiện tại, đây chính xác là những gì công thức điểm cuối yêu cầu. Việc cập nhật chúng trước tiên sẽ đưa ra một đóng góp có độ dài trống không hợp lệ. 

Khi`x == 0`, đoạn khác 0 hiện tại kết thúc. Đặt lại thành`even = 1`,`odd = 0`, Và`parity = 0`bắt đầu một chuỗi tiền tố mới ngay sau số 0. Các mảng con chứa số 0 được loại trừ một cách có chủ ý khỏi số đếm dương và âm. 

biểu thức`n * (n + 1) // 2`đếm mọi cặp điểm cuối có thể. Các số nguyên có độ chính xác tùy ý của Python làm cho câu trả lời có khả năng lớn trở nên an toàn mà không cần bất kỳ xử lý đặc biệt nào. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, mảng là```
5 -3 3 -1 0
```Bảng ghi lại trạng thái tiền tố sau mỗi phần tử được xử lý.`even`Và`odd`là số tiền tố sau khi vị trí hiện tại đã được kết hợp. 

| Vị trí | Giá trị | Chẵn lẻ | Đã thêm tích cực | Đã thêm Tiêu cực | Thậm chí | lẻ | Tích cực | Tiêu cực | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | | 0 | | | 1 | 0 | 0 | 0 | 
| 1 | 5 | 0 | 1 | 0 | 2 | 0 | 1 | 0 | 
| 2 | -3 | 1 | 0 | 2 | 2 | 1 | 1 | 2 | 
| 3 | 3 | 1 | 1 | 2 | 2 | 2 | 2 | 4 | 
| 4 | -1 | 0 | 2 | 2 | 3 | 2 | 4 | 6 | 
| 5 | 0 | đặt lại | 0 | 0 | 1 | 0 | 4 | 6 | 

Có tổng cộng (5\cdot6/2=15) mảng con. Quá trình quét tìm thấy (6) sản phẩm âm và (4) sản phẩm dương, vì vậy (15-6-4=5) sản phẩm bằng 0. Kết quả là```
6 5 4
```Số 0 ở vị trí (5) giải thích tại sao bộ đếm tiền tố phải được đặt lại. Bốn phần tử đầu tiên tạo thành một phân đoạn khác 0 độc lập và mọi mảng con đạt đến 0 thuộc về loại 0 thay vì được phân loại theo dấu chẵn lẻ của nó. 

Đối với Mẫu 2, mảng là```
4 0 -4 3 1 2 -4 3 0 3
```| Vị trí | Giá trị | Chẵn lẻ | Đã thêm tích cực | Đã thêm Tiêu cực | Thậm chí | lẻ | Tích cực | Tiêu cực | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | | 0 | | | 1 | 0 | 0 | 0 | 
| 1 | 4 | 0 | 1 | 0 | 2 | 0 | 1 | 0 | 
| 2 | 0 | đặt lại | 0 | 0 | 1 | 0 | 1 | 0 | 
| 3 | -4 | 1 | 0 | 1 | 1 | 1 | 1 | 1 | 
| 4 | 3 | 1 | 1 | 1 | 1 | 2 | 2 | 2 | 
| 5 | 1 | 1 | 2 | 1 | 1 | 3 | 4 | 3 | 
| 6 | 2 | 1 | 3 | 1 | 1 | 4 | 7 | 4 | 
| 7 | -4 | 0 | 1 | 4 | 2 | 4 | 8 | 8 | 
| 8 | 3 | 0 | 2 | 4 | 3 | 4 | 10 | 12 | 
| 9 | 0 | đặt lại | 0 | 0 | 1 | 0 | 10 | 12 | 
| 10 | 3 | 0 | 1 | 0 | 2 | 0 | 11 | 12 | 

Có tổng cộng (10\cdot11/2=55) mảng con. Quá trình quét cho ra (12) sản phẩm âm và (11) sản phẩm dương, để lại (55-12-11=32) không có sản phẩm nào. Kết quả là```
12 32 11
```Dấu vết cũng cho thấy tại sao hai vị trí số 0 lại là dấu phân cách hữu ích. Phần tử dương đầu tiên được tính độc lập, phân đoạn sáu phần tử giữa các số 0 được xử lý bằng cách sử dụng số chẵn lẻ tiền tố của chính nó và phần tử cuối cùng bắt đầu một phân đoạn độc lập khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi phần tử mảng được xử lý chính xác một lần với các phép toán có thời gian không đổi. | 
| Không gian | (O(1)) | Chỉ có một số bộ đếm cố định và tính chẵn lẻ hiện tại được duy trì. | 

Với (n\le2\cdot10^5), quét tuyến tính chỉ thực hiện một vài thao tác cho mỗi phần tử, dễ dàng nằm trong giới hạn 2 giây. Thuật toán sử dụng không gian phụ không đổi và biểu diễn số nguyên của Python xử lý một cách an toàn các câu trả lời lớn tới (20.000.100.000). 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    n = data[0]
    a = data[1:1 + n]

    even = 1
    odd = 0
    parity = 0

    negative = 0
    positive = 0

    for x in a:
        if x == 0:
            even = 1
            odd = 0
            parity = 0
            continue

        if x < 0:
            parity ^= 1

        if parity == 0:
            positive += even
            negative += odd
            even += 1
        else:
            positive += odd
            negative += even
            odd += 1

    total = n * (n + 1) // 2
    zero = total - positive - negative

    return f"{negative} {zero} {positive}"

def run(inp: str) -> str:
    return solve_data(inp)

# provided samples
assert run("5\n5 -3 3 -1 0\n") == "6 5 4", "sample 1"
assert run("10\n4 0 -4 3 1 2 -4 3 0 3\n") == "12 32 11", "sample 2"
assert run("5\n-1 -2 -3 -4 -5\n") == "9 0 6", "sample 3"

# minimum-size input
assert run("1\n0\n") == "0 1 0", "single zero"

# all equal positive values
assert run("3\n1 1 1\n") == "0 0 6", "all positive"

# all equal negative values
assert run("4\n-1 -1 -1 -1\n") == "6 0 4", "all negative"

# zeros at both boundaries
assert run("3\n0 1 0\n") == "0 5 1", "zeros at boundaries"

# zero splitting two nonzero segments
assert run("5\n1 -1 0 -1 1\n") == "2 11 2", "zero separator"

# maximum-size input
n = 200000
inp = f"{n}\n" + " ".join(["1"] * n) + "\n"
assert run(inp) == "0 0 20000100000", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`0 1 0`| Kích thước tối thiểu và một số 0 duy nhất | 
|`3 / 1 1 1`|`0 0 6`| Tất cả các giá trị dương bằng nhau | 
|`4 / -1 -1 -1 -1`|`6 0 4`| Tính chẵn lẻ và số chẵn âm | 
|`3 / 0 1 0`|`0 5 1`| Số không ở cả hai ranh giới | 
|`5 / 1 -1 0 -1 1`|`2 11 2`| Đặt lại trạng thái xung quanh số 0 bên trong | 
| (n=200000), tất cả`1`|`0 0 20000100000`| Kích thước đầu vào tối đa và câu trả lời lớn | 

## Vỏ cạnh 

Đối với một số 0, đầu vào là```
1
0
```Trạng thái ban đầu là`even = 1`,`odd = 0`. Số 0 kích hoạt thiết lập lại, để lại số đếm khác 0 ở mức 0. Có chính xác một mảng con, vì vậy`zero = 1 - 0 - 0 = 1`. Đầu ra là`0 1 0`. 

Với mọi giá trị dương,```
3
1 1 1
```sự ngang bằng không bao giờ thay đổi. Cặp phần tử đầu tiên với tiền tố chẵn ban đầu, cặp thứ hai có hai tiền tố chẵn và cặp thứ ba có ba tiền tố chẵn. Số dương sẽ trở thành (1+2+3=6), là mọi mảng con. Đầu ra là`0 0 6`. 

Đối với tất cả các giá trị âm,```
4
-1 -1 -1 -1
```tính chẵn lẻ xen kẽ giữa số lẻ và số chẵn. Số âm nhận được (1), (2), (1) và (2) đóng góp, tổng cộng là (6). Số dương nhận được (1), (0), (2) và (1) đóng góp, tổng cộng là (4). Không có số 0 nên kết quả là`6 0 4`. 

Đối với số không ở cả hai ranh giới,```
3
0 1 0
```số 0 đầu tiên đặt lại trạng thái trước khi bất kỳ mảng con khác 0 nào được tính. các`1`sau đó tạo chính xác một mảng con dương. Số 0 cuối cùng sẽ đặt lại trạng thái một lần nữa. Vì có tổng cộng sáu mảng con và chỉ có một mảng con khác 0 nên số 0 là (5). Đầu ra là`0 5 1`. 

Đối với số 0 bên trong,```
5
1 -1 0 -1 1
```đoạn đầu tiên`[1,-1]`đóng góp một mảng con dương và một mảng con âm. Số 0 ngăn bất kỳ vị trí giao nhau của mảng con (3) nào đi vào danh mục khác 0. Đoạn thứ hai`[-1,1]`đóng góp số lượng tương tự. Do đó có (2) mảng con dương và (2) mảng con âm. Với (15) mảng con tổng cộng, (11) mảng còn lại có tích bằng 0, cho`2 11 2`. 

Đối với mảng dương có kích thước tối đa, tất cả (200000) phần tử có thể`1`. Tính chẵn lẻ vẫn duy trì xuyên suốt và số đếm dương trở thành 

# \frac{200000\cdot200001}{2} 

20000100000. 
] 

Không có tích âm hoặc tích bằng 0 tồn tại nên kết quả đầu ra là`0 0 20000100000`. Điều này xác nhận cả thời gian chạy tuyến tính và nhu cầu về kiểu số nguyên có khả năng lưu trữ đầy đủ số lượng mảng con.
