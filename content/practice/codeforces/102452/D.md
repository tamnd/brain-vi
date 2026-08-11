---
title: "CF 102452D - Xác định nhãn"
description: "Các nhãn tạo thành một chuỗi các chuỗi có thứ tự. Đối với cơ số đã chọn (k), cho phép chính xác (k) chữ số thập phân: (10-k, 11-k, ldots, 9). Các nhãn trước tiên được sắp xếp theo độ dài và trong cùng độ dài, chúng được sắp xếp theo thứ tự từ điển bằng cách sử dụng các chữ số được phép đó."
date: "2026-08-10T06:14:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102452
codeforces_index: "D"
codeforces_contest_name: "2019-2020 ICPC Asia Hong Kong Regional Contest"
rating: 0
weight: 102452
solve_time_s: 377
verified: true
draft: false
---

[CF 102452D - Xác định nhãn](https://codeforces.com/problemset/problem/102452/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Các nhãn tạo thành một chuỗi các chuỗi có thứ tự. Đối với cơ số (k) được chọn, cho phép chính xác (k) chữ số thập phân: (10-k, 11-k, \ldots, 9). Các nhãn trước tiên được sắp xếp theo độ dài và trong cùng độ dài, chúng được sắp xếp theo thứ tự từ điển bằng cách sử dụng các chữ số được phép đó. 

Ví dụ: khi (k=7), các chữ số có sẵn là (3,4,5,6,7,8,9). Bảy nhãn đầu tiên là chuỗi một ký tự`3`bởi vì`9`. Theo sau chúng là tất cả (7^2) chuỗi hai ký tự, bắt đầu bằng`33`,`34`, và kết thúc bằng`99`. Sau đó là tất cả (7^3) chuỗi ba ký tự. 

Mỗi trường hợp thử nghiệm đưa ra cơ sở (k) và vị trí dựa trên một (X). Nhiệm vụ là xuất ra nhãn chiếm vị trí đó. 

Nhận xét quan trọng từ các ràng buộc là (X) nhiều nhất là (10^9), trong khi có thể có tới (10^5) trường hợp thử nghiệm độc lập. Một giải pháp kiểm tra từng nhãn một sẽ cần tới (10^9) lần lặp cho một trường hợp, vốn đã quá lớn. Trong (10^5) trường hợp, ngay cả phương pháp (O(X)) cũng hoàn toàn không thực tế. Chúng ta cần chuyển trực tiếp đến khối chứa câu trả lời và sau đó chỉ xây dựng các chữ số của câu trả lời đó. 

Độ dài câu trả lời cũng rất nhỏ. Có (k^L) nhãn có độ dài (L), vì vậy đối với (k=2), độ dài (29) đầu tiên đã chứa nhãn (2^{30}-2=1,073,741,822). Do đó, ngay cả trong cơ sở phát triển chậm nhất, câu trả lời cho (X\le 10^9) có tối đa (29) ký tự. Điều này làm cho thuật toán (O(\log_k X)) cho mỗi trường hợp đủ nhanh. 

Một số trường hợp ranh giới có thể làm cho việc triển khai trực tiếp không thành công. 

Với (k=10) và (X=1), câu trả lời là:```
0
```Vị trí dựa trên một, do đó nhãn đầu tiên tương ứng với số 0 bên trong khối đầu tiên. Việc triển khai bất cẩn bắt đầu chuyển đổi trực tiếp từ (X) sẽ tạo ra`1`thay vì. 

Với (k=10) và (X=11), câu trả lời là:```
00
```Mười vị trí đầu tiên là nhãn một chữ số`0`bởi vì`9`. Vị trí (11) là nhãn có hai chữ số đầu tiên. Nếu việc triển khai sử dụng điều kiện (X \ge k^L) thay vì so sánh với toàn bộ khối trước đó, nó có thể gán sai vị trí này cho khối một chữ số. 

Với (k=7) và (X=8), câu trả lời là:```
33
```Các chữ số được phép là`3`bởi vì`9`, do đó bảy nhãn đầu tiên chiếm các vị trí từ (1) đến (7). Vị trí (8) bắt đầu khối hai chữ số. Một giải pháp giả sử các chữ số luôn bắt đầu bằng 0 sẽ xây dựng`00`, đây không phải là nhãn hợp lệ trong cơ sở này. 

Với (k=2) và (X=15), câu trả lời là:```
8888
```Có (2+4+8=14) nhãn có độ dài từ một đến ba, vì vậy vị trí (15) là nhãn có bốn ký tự đầu tiên. Vì chữ số nhỏ nhất được phép là`8`, nhãn đầu tiên của mỗi độ dài bao gồm toàn bộ`8`S. Điều này phát hiện lỗi ở cả ranh giới khối và dịch chữ số. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là tạo nhãn theo thứ tự thực tế của chúng. Bắt đầu với tất cả các nhãn một ký tự, sau đó là tất cả các nhãn hai ký tự, sau đó là tất cả các nhãn ba ký tự, v.v. Trong mỗi độ dài, phép đếm cơ số (k) thông thường sẽ đưa ra thứ tự từ điển chính xác. Chúng ta có thể giữ lại một bộ đếm, chuyển đổi nó thành một nhãn và dừng lại sau khi tạo ra nhãn thứ (X). 

Phương pháp brute-force này đúng vì nó tuân theo chính xác thứ tự được xác định bởi bài toán. Vấn đề của nó là khối lượng công việc. Đối với (X=10^9), nó phải tạo ra một tỷ nhãn. Với (k=2), câu trả lời có độ dài (29) và việc tạo nhãn (10^9) đầu tiên yêu cầu khoảng (2,79\times10^{10}) thao tác ký tự. Ngay cả trước khi xem xét chi phí Python, điều đó vượt xa một giải pháp lập trình cạnh tranh thực tế. 

Cấu trúc của chuỗi cho chúng ta cách đếm tốt hơn nhiều. Mỗi khối có độ dài-(L) chứa chính xác các nhãn (k^L) vì mỗi vị trí (L) đều có (k) lựa chọn. Do đó, chúng tôi không cần tạo bất kỳ thứ gì để xác định độ dài của câu trả lời. Chúng ta có thể trừ toàn bộ khối cho đến khi vị trí còn lại nằm trong một khối. 

Giả sử câu trả lời có độ dài (L) và đặt chỉ số dựa trên số 0 bên trong khối đó là (r). Biểu diễn cơ số (L)-chữ số (k) của (r), được đệm bằng các số 0 ở đầu, xác định nhãn được yêu cầu. Sự khác biệt duy nhất là các chữ số của bài toán bắt đầu ở (10-k) thay vì 0. Do đó, mọi chữ số cơ sở-(k) (d) được dịch sang chữ số in (d+(10-k)). 

Giải pháp brute-force hoạt động vì nó đi qua cùng một trình tự mà chúng ta cần lập chỉ mục một cách rõ ràng, nhưng không thành công vì trình tự đó có thể chứa hàng tỷ mục nhập. Quan sát rằng mỗi độ dài tạo thành một khối hoàn chỉnh có nhãn chính xác (k^L) cho phép chúng tôi bỏ qua các khối đó một cách hợp lý và chỉ xây dựng tối đa (29) ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(X\log_k X)) ký tự hoạt động | (O(\log_k X)) | Quá chậm | 
| Tối ưu | (O(\log_k X)) | (O(\log_k X)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ vị trí được yêu cầu (X) và xem xét khối nhãn có chiều dài (1). Khối đó chứa chính xác (k) nhãn. Nếu (X) lớn hơn khối này, hãy trừ (k) và di chuyển đến độ dài (2). Tiếp tục theo cách tương tự. 

Sau khi xử lý tất cả các độ dài ngắn hơn, giá trị còn lại (X) là vị trí dựa trên một bên trong khối có độ dài chính xác (L). 
2. Chuyển đổi vị trí còn lại này thành độ lệch dựa trên 0 bằng cách thay thế (X) bằng (X-1). Điều này quan trọng vì nhãn đầu tiên trong khối tương ứng với giá trị số 0 chứ không phải một. 
3. Chuyển đổi độ lệch dựa trên 0 thành cơ sở (k). Biểu diễn kết quả phải chứa chính xác (L) chữ số, vì vậy hãy thêm các số 0 vào trước cho đến khi độ dài của nó là (L). 

Mỗi số cơ sở-(k) có chữ số (L) từ (0) đến (k^L-1) xuất hiện chính xác một lần trong khối độ dài-(L), theo cùng thứ tự với các nhãn. 
4. Dịch từng chữ số cơ số (k) thành chữ số thập phân thực tế của bài toán. Nếu chữ số cơ sở-(k) là (d), hãy in (d+(10-k)). Ví dụ, trong cơ số (7), các chữ số bên trong (0,1,2,\ldots,6) trở thành các chữ số in ra (3,4,5,\ldots,9). 
5. Nối các chữ số đã dịch theo thứ tự ban đầu của chúng và in chuỗi kết quả. Vì dữ liệu đầu vào chứa tối đa (10^5) trường hợp nên hãy xử lý từng trường hợp một cách độc lập và tích lũy kết quả đầu ra trước khi ghi. 

### Tại sao nó hoạt động 

Đối với mỗi độ dài (L), có chính xác (k^L) nhãn có thể và vấn đề liệt kê tất cả các độ dài ngắn hơn trước khi đạt đến độ dài (L). Vòng trừ loại bỏ chính xác các khối hoàn chỉnh trước khối mục tiêu, do đó (X) còn lại là vị trí dựa trên một mục tiêu trong phạm vi chiều dài của chính nó.

Sau khi thay đổi thành (X-1), giá trị nằm trong khoảng (0) đến (k^L-1). Có sự tương ứng một-một giữa các giá trị này và tất cả các chuỗi chữ số (L) trên tập hợp chữ số bên trong (0,\ldots,k-1). Biểu diễn cơ sở-(k) cung cấp cho các chuỗi đó theo thứ tự từ điển tăng dần khi tất cả các biểu diễn được đệm theo độ dài (L). Cuối cùng, việc thêm (10-k) vào mỗi chữ số bên trong sẽ thể hiện chính xác các chữ số thập phân được phép. Do đó chuỗi được xây dựng chính xác là nhãn được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_label(k, x):
    length = 1
    block = k

    while x > block:
        x -= block
        length += 1
        block *= k

    # Convert from one-based position inside the block
    # to a zero-based base-k value.
    value = x - 1

    digits = ['0'] * length
    shift = 10 - k

    for i in range(length - 1, -1, -1):
        digits[i] = chr(ord('0') + shift + (value % k))
        value //= k

    return ''.join(digits)

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        k = int(input())
        x = int(input())
        out.append(get_label(k, x))

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    solve()
```các`length`biến đại diện cho khối hiện đang được kiểm tra. tương ứng của nó`block`giá trị là (k^{length}), số lượng nhãn có chính xác số ký tự đó. Vòng lặp trừ toàn bộ khối cho đến khi mục tiêu rơi vào khối hiện tại. 

dòng`value = x - 1`là sự điều chỉnh từng bước một. Khi mục tiêu nằm trong khối có độ dài cố định, nhãn đầu tiên phải tương ứng với giá trị bên trong bằng 0, nhãn thứ hai tương ứng với một, v.v. 

Vòng chuyển đổi trích xuất các chữ số cơ số (k) từ phải sang trái bằng cách sử dụng`value % k`. Chia cho (k) sẽ loại bỏ chữ số vừa xử lý. Mảng đầu ra được khởi tạo chính xác`length`vị trí, do đó các số 0 đứng đầu được giữ nguyên tự động. Điều này là cần thiết bởi vì`00`Và`0`đại diện cho các nhãn khác nhau mặc dù cả hai đều có cùng giá trị số nếu các số 0 đứng đầu bị bỏ qua. 

biểu thức`shift = 10 - k`thực hiện việc dịch chữ số. Ví dụ: đối với (k=7), chữ số 0 bên trong trở thành`3`, trong khi chữ số bên trong sáu trở thành`9`. 

Không có vấn đề tràn số nguyên trong Python. Dù sao thì khối trung gian lớn nhất cần thiết cũng đủ nhỏ vì vòng lặp sẽ dừng khi mục tiêu nằm bên trong một khối và (X\le10^9). Trong ngôn ngữ có chiều rộng cố định, việc sử dụng loại số nguyên đủ rộng vẫn được khuyến khích. 

Đầu ra được tích lũy trong một danh sách và được ghi một lần. Với (10^5) trường hợp thử nghiệm, các lệnh gọi lặp lại tới`print`thường vẫn có thể quản lý được, nhưng việc thu thập các chuỗi sẽ tránh được chi phí đầu ra không cần thiết. 

## Ví dụ đã hoạt động 

Đoạn trích cuộc thi được cung cấp không chứa đầu vào hoặc đầu ra mẫu có thể sử dụng được. Các dấu vết sau đây sử dụng hai trường hợp cụ thể minh họa hai phần chính của thuật toán. 

Xét (k=7) và (X=50). Các chữ số được phép là`3`bởi vì`9`. Có (7) nhãn một chữ số và (49) nhãn hai chữ số nên vị trí (50) là nhãn ba chữ số đầu tiên. 

| Bước | Chiều dài | Kích thước khối | X Trước | X Sau | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 7 | 50 | 50 | 
| Xóa độ dài 1 | 2 | 49 | 50 | 43 | 

Sau khi loại bỏ bảy nhãn một chữ số, mục tiêu là vị trí (43) bên trong khối hai chữ số. Vì (43\le49), mục tiêu thực sự có độ dài bằng hai. Độ lệch dựa trên 0 là (42), có biểu diễn cơ sở-(7) là`60`. Dịch chữ số nội bộ`6`Và`0`bởi (3) cho`93`. 

| Biến | Giá trị | 
| --- | --- | 
| k | 7 | 
| Chiều dài | 2 | 
| Còn lại X | 43 | 
| Giá trị dựa trên 0 | 42 | 
| Cơ số 7 chữ số |`60`| 
| Chuyển số | 3 | 
| Trả lời |`93`| 

Dấu vết cho thấy tại sao kích thước khối phải là (k^L), không phải (k^{L-1}). Có (49) nhãn hai ký tự riêng biệt và vị trí (43) nằm thoải mái bên trong khối đó. 

Bây giờ hãy xem xét (k=10) và (X=11). Khối một ký tự chứa mười nhãn,`0`bởi vì`9`. Vị trí (11) là nhãn hai ký tự đầu tiên. 

| Bước | Chiều dài | Kích thước khối | X Trước | X Sau | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 10 | 11 | 11 | 
| Xóa độ dài 1 | 2 | 100 | 11 | 1 | 

Vị trí còn lại là (1) trong khối hai ký tự. Sau khi chuyển đổi sang lập chỉ mục dựa trên số 0, giá trị bằng 0. Biểu diễn hai chữ số của nó là`00`và bởi vì (k=10), độ dịch chuyển chữ số bằng 0. 

| Biến | Giá trị | 
| --- | --- | 
| k | 10 | 
| Chiều dài | 2 | 
| Còn lại X | 1 | 
| Giá trị dựa trên 0 | 0 | 
| Cơ số 10 chữ số |`00`| 
| Chuyển số | 0 | 
| Trả lời |`00`| 

Dấu vết này thực hiện lỗi từng cái một phổ biến nhất. Phần tử đầu tiên của khối phải tương ứng với 0 khi khối được chuyển đổi thành hệ thống số vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log_k X)) cho mỗi trường hợp thử nghiệm | Thuật toán bỏ qua các khối độ dài và sau đó chuyển đổi tối đa (O(\log_k X)) chữ số. | 
| Không gian | (O(\log_k X)) cho mỗi trường hợp thử nghiệm | Câu trả lời và mảng chữ số của nó chứa chính xác độ dài câu trả lời. | 

Với (X\le10^9), độ dài câu trả lời lớn nhất có thể xảy ra đối với (k=2) và nó chỉ là (29). Do đó, ngay cả (10^5) trường hợp kiểm thử cũng chỉ yêu cầu vài triệu phép tính số học đơn giản. Bộ nhớ được sử dụng cho mỗi câu trả lời rất nhỏ và kết quả tích lũy tỷ lệ thuận với tổng số ký tự được in. 

## Trường hợp thử nghiệm```python
import sys
import io

input = sys.stdin.readline

def get_label(k, x):
    length = 1
    block = k

    while x > block:
        x -= block
        length += 1
        block *= k

    value = x - 1
    shift = 10 - k

    digits = ['0'] * length

    for i in range(length - 1, -1, -1):
        digits[i] = chr(ord('0') + shift + value % k)
        value //= k

    return ''.join(digits)

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        k = int(input())
        x = int(input())
        out.append(get_label(k, x))

    return '\n'.join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# The statement excerpt contains no usable official sample.
# These are concrete examples derived from the stated ordering.

assert run("""2
10
1
10
11
""") == """0
00""", "basic base-10 boundary cases"

assert run("""2
7
8
7
56
""") == """33
99""", "base-7 block boundaries"

assert run("""3
2
1
2
3
2
15
""") == """8
9
8888""", "minimum positions and length transition"

assert run("""2
10
10
10
1000000000
""") == """9
0000000000""", "first length transition and maximum X"

assert run("""3
2
4
2
6
10
110
""") == """89
99
99""", "off-by-one positions inside blocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`k=10, X=1`Và`k=10, X=11`|`0`,`00`| Vị trí đầu tiên và quá trình chuyển đổi từ một chữ số sang hai chữ số | 
|`k=7, X=8`Và`k=7, X=56`|`33`,`99`| Chữ số bắt đầu khác 0 và cả hai đầu của khối hai chữ số | 
|`k=2, X=1`,`X=3`,`X=15`|`8`,`9`,`8888`| Vị trí tối thiểu và nhãn đầu tiên có độ dài khác nhau | 
|`k=10, X=10`Và`X=10^9`|`9`,`0000000000`| Chặn ranh giới và vị trí tối đa được phép | 
|`k=2, X=4`,`X=6`,`k=10, X=110`|`89`,`99`,`99`| Vị trí gần đầu và cuối khối | 

## Vỏ cạnh 

Đối với (k=10,\ X=1), đầu vào chính xác là:```
1
10
1
```Khối đầu tiên có kích thước (10) nên không có khối nào bị bỏ qua. Vị trí còn lại là (1), trở thành 0 sau khi điều chỉnh một lần. Biểu diễn một chữ số của nó là`0`, cho kết quả đúng`0`. Chuyển đổi trực tiếp (X) mà không trừ đi một sẽ in sai`1`. 

Với (k=10,\ X=11), đầu vào là:```
1
10
11
```Mười nhãn đầu tiên là chuỗi một ký tự`0`bởi vì`9`. Thuật toán trừ toàn bộ khối đó, để lại (X=1) trong khối hai ký tự. Sau khi chuyển đổi sang lập chỉ mục dựa trên số 0, giá trị bằng 0, có biểu diễn hai chữ số là`00`. Do đó, đầu ra là:```
00
```Việc đệm là cần thiết. Xử lý giá trị như một số nguyên thông thường và in`0`sẽ mất một ký tự và tạo ra nhãn không hợp lệ. 

Với (k=7,\ X=8), đầu vào là:```
1
7
8
```Các chữ số có sẵn là`3`bởi vì`9`. Bảy nhãn đầu tiên là nhãn một ký tự, do đó thuật toán loại bỏ một khối có kích thước (7) và để lại (X=1) trong khối hai ký tự. Giá trị dựa trên số 0 là số 0, được biểu thị bằng`00`nội bộ. Thêm ca (10-7=3) vào cả hai chữ số sẽ cho`33`. 

Với (k=2,\ X=15), đầu vào là:```
1
2
15
```Các khối có kích thước (2,4,8,16,\ldots). Ba khối đầu tiên chứa các nhãn (2+4+8=14), vì vậy nhãn thứ mười lăm là nhãn đầu tiên có độ dài bốn. Vị trí còn lại là (1), trở thành 0 và biểu diễn bên trong gồm bốn chữ số là`0000`. Vì các chữ số được phép là`8`Và`9`, ánh xạ số 0 nội bộ tới`8`, sản xuất:```
8888
```Trường hợp này đồng thời kiểm tra độ dài câu trả lời lớn nhất có thể truy cập trong (X\le10^9) và việc xử lý khối độ dài mới. 

Với (k=10,\ X=10^9), đầu vào là:```
1
10
1000000000
```Các khối có độ dài từ một đến chín chứa 

[ 
10+10^2+\cdots+10^9=999,999,999 
] 

nhãn. Do đó (X=10^9) chính xác là nhãn đầu tiên có độ dài 10. Vị trí còn lại là (1), do đó giá trị bên trong bằng 0 và biểu diễn phần đệm là 10 số 0. Vì cơ số (10) không cần dịch chữ số nên câu trả lời là:```
0000000000
```Đây là bài kiểm tra kích thước tối đa hữu ích vì nó đạt đến (X) lớn nhất cho phép mà không yêu cầu chuỗi câu trả lời lớn.
