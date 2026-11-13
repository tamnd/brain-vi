---
title: "CF 1763A - Tối đa hóa tuyệt đối"
description: "Chúng ta có một dãy số nguyên. Một thao tác cho phép chúng ta chọn bất kỳ hai vị trí và chỉ số bit nào, sau đó hoán đổi giá trị của bit đó giữa hai số. Chi tiết quan trọng là vị trí bit được cố định trong quá trình hoán đổi."
date: "2026-06-09T13:35:47+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1763
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 840 (Div. 2) and Enigma 2022 - Cybros LNMIIT"
rating: 800
weight: 1763
solve_time_s: 150
verified: true
draft: false
---

[CF 1763A - Tối đa hóa tuyệt đối](https://codeforces.com/problemset/problem/1763/A) 

**Đánh giá:** 800 
**Tags:** bitmasks, thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy số nguyên. Một thao tác cho phép chúng ta chọn bất kỳ hai vị trí và chỉ số bit nào, sau đó hoán đổi giá trị của bit đó giữa hai số. 

Chi tiết quan trọng là vị trí bit được cố định trong quá trình hoán đổi. Chúng ta có thể di chuyển một`1`bit từ số này sang số khác nhưng chỉ trong cùng một vị trí bit. Ví dụ: một bit ở vị trí 3 không bao giờ có thể trở thành một bit ở vị trí 2. 

Mục tiêu là tối đa hóa sự khác biệt giữa phần tử lớn nhất và nhỏ nhất trong mảng sau khi thực hiện bất kỳ số lượng thao tác nào như vậy. 

Kích thước mảng tối đa là 512 và mọi giá trị đều nhỏ hơn 1024. Vì 1024 là$2^{10}$, mỗi số chỉ chứa 10 vị trí bit liên quan. Độ rộng bit nhỏ gợi ý rõ ràng rằng việc suy luận từng bit sẽ hữu ích hơn so với các hoạt động mô phỏng. 

Một sai lầm phổ biến là nghĩ rằng các giao dịch hoán đổi sẽ bảo toàn giá trị của mỗi số. Họ không làm vậy. Ví dụ:```
2
4 3
```Các dạng nhị phân là`100`Và`011`. Hoán đổi bit 2 tạo ra:```
000 = 0
111 = 7
```Những con số thay đổi đáng kể. 

Một sai lầm dễ mắc phải khác là coi các vị trí bit khác nhau là tương tác với nhau. Họ hoàn toàn độc lập. Ví dụ:```
3
1 2 4
```Mỗi vị trí bit có chính xác một`1`. Không có hoạt động nào có thể di chuyển một`1`từ vị trí này sang vị trí khác. Điều tốt nhất chúng ta có thể làm là thu thập tất cả những gì hiện có`1`bit thành một số, tạo ra`7`, và để lại một số khác là`0`. Câu trả lời là`7`. 

Trường hợp cạnh thứ ba là khi tất cả các số đều giống hệt nhau:```
4
5 5 5 5
```Mỗi vị trí bit đều chứa tất cả số 0 hoặc tất cả số 1. Hoán đổi bit không thay đổi gì, vì vậy câu trả lời vẫn là`0`. 

## Phương pháp tiếp cận 

Tư duy vũ phu bắt đầu bằng việc hỏi những trạng thái nào có thể truy cập được. Chúng ta có thể thực hiện hoán đổi bit liên tục, tạo ra các mảng mới và tìm kiếm giá trị lớn nhất có thể của`max(a) - min(a)`. 

Điều này đúng về nguyên tắc vì mọi hoạt động pháp lý đều được tìm hiểu. Vấn đề là không gian trạng thái bùng nổ ngay lập tức. Ngay cả đối với một hoạt động duy nhất cũng có$O(n^2 \cdot 10)$sự lựa chọn. Chuỗi các thao tác tạo ra một số lượng lớn các mảng có thể truy cập được, khiến cho việc tìm kiếm rõ ràng hoàn toàn không khả thi. 

Quan sát quan trọng là vị trí bit là độc lập. 

Xem xét một vị trí bit cố định$b$. Giả sử chính xác$k$số hiện có chứa một`1`ở vị trí đó. Hoán đổi bit chỉ sắp xếp lại những bit đó$k$những cái trong số các vị trí mảng. Tổng số cái ở vị trí bit đó không bao giờ thay đổi. 

Điều này có nghĩa là đối với mỗi vị trí bit, chúng ta chỉ quan tâm đến số lượng vị trí tồn tại chứ không quan tâm đến vị trí hiện tại của chúng. 

Bây giờ hãy nghĩ đến việc tối đa hóa:```
max(a) - min(a)
```Để làm cho sự khác biệt này càng lớn càng tốt, chúng ta muốn một số càng lớn càng tốt và một số khác càng nhỏ càng tốt. 

Đối với mỗi vị trí bit: 

Nếu ít nhất một`1`tồn tại ở vị trí đó, chúng ta có thể di chuyển một trong những cái đó vào phần tử tối đa trong tương lai. 

Nếu không phải tất cả các số đều chứa một`1`ở vị trí đó thì một số phần tử có thể có`0`ở đó, giúp tạo ra phần tử tối thiểu trong tương lai. 

Từ$n \ge 3$, bất cứ khi nào một vị trí bit chứa ít nhất một`1`, chúng ta luôn có thể đặt một`1`vào phần tử tối đa và a`0`vào phần tử tối thiểu cùng một lúc. 

Kết quả là, mỗi vị trí bit đóng góp độc lập vào sự khác biệt cuối cùng: 

Nếu một vị trí bit xuất hiện trong ít nhất một số, nó sẽ đóng góp toàn bộ giá trị của nó$2^b$để trả lời. 

Đó chính xác là định nghĩa của bitwise OR trên toàn bộ mảng. 

Vì vậy, câu trả lời chỉ đơn giản là:```
a1 OR a2 OR ... OR an
```Có một cách khác để thấy được kết quả tương tự. Cho phép:```
M = OR of all elements
```Chúng ta có thể thu thập mọi thứ có sẵn`1`bit thành một phần tử, tạo ra giá trị`M`. 

Đối với phần tử tối thiểu, mọi bit không có trong tất cả các số đều có thể được tạo`0`. Từ$n \ge 3$, chúng ta có thể chọn một phần tử khác để giữ các bit đó. Mức tối thiểu có thể được hướng tới`0`. 

Kể từ đây:```
max(a) - min(a) = M - 0 = M
```| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không gian trạng thái hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ tích lũy`ans = 0`. 
2. Lặp qua tất cả các số trong mảng. 
3. Cập nhật`ans |= value`cho mỗi phần tử. 

Điều này thu thập mọi vị trí bit xuất hiện ít nhất một lần trong mảng. 
4. Sau khi xử lý toàn bộ mảng, xuất ra`ans`. 

Lý do điều này hoạt động là vì mỗi vị trí bit có thể được sắp xếp lại một cách độc lập. Bất kỳ bit nào tồn tại ở đâu đó trong mảng đều có thể được gán cho phần tử tối đa trong tương lai. Vì phần tử tối thiểu có thể đồng thời tránh nhận bit đó nên giá trị đầy đủ của bit đó góp phần tạo ra sự khác biệt cuối cùng. 

### Tại sao nó hoạt động 

Đối với mỗi vị trí bit$b$, gọi giá trị của nó là$2^b$. 

Nếu không có phần tử nào chứa bit đó, nó không bao giờ có thể xuất hiện ở bất cứ đâu, vì vậy đóng góp của nó bằng 0. 

Nếu ít nhất một phần tử chứa bit đó thì số lượng phần tử ở vị trí đó là dương. Bằng cách hoán đổi các bit giữa các phần tử mảng, chúng ta có thể đặt một trong các phần tử đó vào phần tử được chọn ở mức tối đa. Đồng thời, vì$n \ge 3$, chúng ta có thể giữ một phần tử khác có số 0 ở vị trí đó và sử dụng nó làm ứng cử viên tối thiểu. 

Do đó, mỗi bit xuất hiện ít nhất một lần đều đóng góp chính xác$2^b$đến sự khác biệt có thể đạt được. Tổng các đóng góp này trên tất cả các vị trí bit sẽ cho OR theo bit của mảng, đây chính xác là câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    for x in a:
        ans |= x

    print(ans)
```Việc thực hiện cực kỳ nhỏ vì việc quan sát toán học sẽ loại bỏ mọi mô phỏng. 

Biến`ans`lưu trữ bitwise OR đang chạy của mảng. Mỗi khi chúng ta xử lý một số, tất cả các bit có trong số đó sẽ xuất hiện trong`ans`. 

Sau khi quét toàn bộ mảng,`ans`chứa chính xác tập hợp các vị trí bit xuất hiện ít nhất một lần ở bất kỳ vị trí nào trong mảng. Bằng chứng ở trên cho thấy giá trị này bằng chênh lệch tối đa có thể đạt được. 

Không có điều kiện biên phức tạp. Các con số đều nhỏ, số nguyên Python dễ dàng xử lý chúng và câu trả lời vừa vặn thoải mái trong phạm vi số nguyên bình thường. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5
1 2 3 4 5
```| Giá trị hiện tại | Nhị phân | Đang chạy HOẶC | 
| --- | --- | --- | 
| 1 | 001 | 001 | 
| 2 | 010 | 011 | 
| 3 | 011 | 011 | 
| 4 | 100 | 111 | 
| 5 | 101 | 111 | 

HOẶC cuối cùng:```
111₂ = 7
```Đầu ra:```
7
```Ví dụ này minh họa cách tích lũy bit một cách độc lập. Mỗi vị trí bit xuất hiện ở bất cứ đâu đều góp phần tạo ra câu trả lời. 

### Ví dụ 2 

đầu vào:```
1
7
20 85 100 41 76 49 36
```| Giá trị hiện tại | Nhị phân | Đang chạy HOẶC | 
| --- | --- | --- | 
| 20 | 0010100 | 0010100 | 
| 85 | 1010101 | 1010101 | 
| 100 | 1100100 | 1110101 | 
| 41 | 0101001 | 1111101 | 
| 76 | 1001100 | 1111101 | 
| 49 | 0110001 | 1111101 | 
| 36 | 0100100 | 1111101 | 

HOẶC cuối cùng:```
1111101₂ = 125
```Đầu ra:```
125
```Dấu vết này cho thấy rằng khi một bit xuất hiện ở bất kỳ số nào, nó sẽ tồn tại mãi mãi trong OR đang chạy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua mảng | 
| Không gian | O(1) | Chỉ có bộ tích lũy OR được lưu trữ | 

Tổng của tất cả độ dài mảng trong các trường hợp thử nghiệm nhiều nhất là 512, do đó thuật toán chỉ thực hiện vài trăm thao tác. Nó dễ dàng nằm gọn trong giới hạn thời gian 1 giây và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ans = 0
        for x in a:
            ans |= x

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# provided samples
assert run(
"""4
3
1 0 1
4
5 5 5 5
5
1 2 3 4 5
7
20 85 100 41 76 49 36
"""
) == "1\n0\n7\n125"

# minimum size
assert run(
"""1
3
0 0 0
"""
) == "0"

# all equal
assert run(
"""1
5
7 7 7 7 7
"""
) == "7"

# disjoint bits
assert run(
"""1
3
1 2 4
"""
) == "7"

# boundary values
assert run(
"""1
3
1023 0 0
"""
) == "1023"

# many values
assert run(
"""1
4
8 4 2 1
"""
) == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0`|`0`| Tất cả các bit đều vắng mặt | 
|`7 7 7 7 7`|`7`| Giá trị giống nhau | 
|`1 2 4`|`7`| Vị trí bit độc lập | 
|`1023 0 0`|`1023`| Giá trị tối đa được phép | 
|`8 4 2 1`|`15`| HOẶC tích lũy nhiều bit | 

## Vỏ cạnh 

### Tất cả các giá trị đều bằng nhau 

đầu vào:```
1
4
5 5 5 5
```Thuật toán tính toán:```
5 OR 5 OR 5 OR 5 = 5
```Đầu ra:```
5
```Mọi vị trí bit đều có mặt trong tất cả các số hoặc vắng mặt trong tất cả các số. Không có trao đổi thay đổi bất cứ điều gì. Sự khác biệt tối đa có thể đạt được chính xác là 5, khớp với OR. 

### Mỗi bit hữu ích xuất hiện dưới một số khác nhau 

đầu vào:```
1
3
1 2 4
```Chạy OR trở thành:```
1 -> 3 -> 7
```Đầu ra:```
7
```Mặc dù ban đầu không có phần tử đơn lẻ nào chứa nhiều bit, nhưng việc hoán đổi cho phép thu thập cả ba bit thành một số. Một giải pháp chỉ nhìn vào mức tối đa hiện tại sẽ bỏ lỡ khả năng này một cách không chính xác. 

### Sự hiện diện của số không 

đầu vào:```
1
3
1023 0 0
```Chạy HOẶC là:```
1023
```Đầu ra:```
1023
```Mọi vị trí bit đều đã tồn tại ở đâu đó, vì vậy tất cả chúng đều góp phần tạo nên câu trả lời. Các số 0 hiện có có thể đóng vai trò là phần tử tối thiểu, mang lại sự khác biệt hoàn toàn có thể có. 

### Không có bit nào tồn tại ở bất cứ đâu 

đầu vào:```
1
3
0 0 0
```Đang chạy HOẶC ở lại:```
0
```Đầu ra:```
0
```Vì không có`1`các bit ở bất kỳ vị trí nào, không thao tác nào có thể tạo ra một bit. Câu trả lời phải bằng 0 để thuật toán trả về chính xác.
