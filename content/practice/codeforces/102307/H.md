---
title: "CF 102307H - Thử thách khó khăn nhất"
description: "Mỗi đội có ba dây có độ dài riêng. Ở mọi vị trí, đội có thể chọn nhân vật từ bất kỳ một trong ba thành viên của mình, độc lập với mọi vị trí khác. Do đó, một nhóm có các chuỗi (P,Q,R) có thể xây dựng tối đa (3^n) chuỗi khác nhau."
date: "2026-08-13T07:22:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "H"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 193
verified: true
draft: false
---

[CF 102307H - Thử thách khó khăn nhất](https://codeforces.com/problemset/problem/102307/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi đội có ba dây có độ dài riêng. Ở mọi vị trí, đội có thể chọn nhân vật từ bất kỳ một trong ba thành viên của mình, độc lập với mọi vị trí khác. Do đó, một nhóm có các chuỗi (P,Q,R) có thể xây dựng tối đa (3^n) chuỗi khác nhau. 

Điểm của một chuỗi được xây dựng là modulo băm đa thức cơ sở 127 của nó 

[ 
MOD=10^{15}+37. 
] 

Cách giải thích dự định của hàm băm là phép lặp tiêu chuẩn từ trái sang phải 

[ 
h_0=0,\qquad h_{i+1}=(127h_i+\operatorname{ord}(S_i))\bmod MOD. 
] 

Điều này tương đương với việc tính trọng số cho các ký tự theo lũy thừa giảm dần của 127. Trang bài toán chính thức đưa ra (A,B\le 28) và các ví dụ sử dụng chính xác cấu trúc này. 

Đối với Cú, chúng ta phải tìm điểm nhỏ nhất có thể có trong số tất cả các chuỗi có độ dài (A). Chúng tôi làm tương tự một cách độc lập cho Dê, sau đó so sánh hai điểm tối thiểu. Điểm nhỏ hơn sẽ thắng, còn điểm bằng nhau sẽ hòa. 

Giới hạn trên của 28 là toàn bộ độ khó. Việc liệt kê trực tiếp sẽ yêu cầu (3^{28}=22,876,792,454,961) chuỗi ứng cử viên cho một nhóm, vượt xa những gì có thể được xử lý trong 10 giây. Mặt khác, việc chia 28 vị trí thành hai nửa sẽ mang lại nhiều nhất (3^{14}=4,782,969) khả năng cho mỗi nửa. Một vài triệu tiểu bang tuy lớn nhưng có thể quản lý được, điều này gợi ý rõ ràng về một giải pháp đáp ứng ở giữa. 

Mô-đun cũng có vấn đề. Nếu không có phép toán modulo, việc chọn chuỗi nhỏ nhất về mặt từ điển hoặc số nhỏ nhất là đủ vì tất cả các giá trị ký tự đều dương và các vị trí trước đó có lũy thừa lớn hơn là 127. Sau khi lấy mô đun, giá trị đa thức lớn hơn có thể có phần dư nhỏ hơn nhiều. Bất kỳ cách tiếp cận nào nhằm giảm thiểu đa thức chưa sửa đổi đều có thể âm thầm chọn sai chuỗi. 

Trường hợp thứ hai là một đội có ba thành viên có tính cách giống nhau ở một vị trí nào đó. Khi đó chỉ có một lựa chọn riêng biệt ở vị trí đó, mặc dù việc triển khai ngây thơ có thể tính nó ba lần. Các bản sao không ảnh hưởng đến tính chính xác nhưng việc loại bỏ chúng có thể làm giảm đáng kể khối lượng công việc thực tế. 

Trường hợp độ dài một cũng dễ bị xử lý sai nếu số mũ trong câu lệnh được hiểu theo nghĩa đen. Đối với một ký tự, điểm số phải đơn giản là giá trị ASCII của nó. Ví dụ,```
1 1
E
L
I
X
Y
Z
```cho`Owls`, bởi vì Cú có thể có được`E`, có điểm là 69, trong khi Dê có điểm tối thiểu là 88. Phép lặp hàm băm từ trái sang phải làm cho trường hợp ranh giới này trở nên rõ ràng. 

Cuối cùng, việc bao quanh mô-đun có thể xảy ra đối với các chuỗi dài. Ví dụ: mẫu thứ hai có 28 ký tự ở phía Dê, do đó đa thức phát triển vượt xa (MOD) trước khi bị giảm. So sánh các giá trị đa thức thô sẽ không tương đương với việc so sánh điểm số của chúng. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa trực tiếp. Đối với mọi vị trí, hãy thử từng ký tự trong số ba ký tự có sẵn, xây dựng đệ quy mọi chuỗi có thể, tính toán hàm băm của nó và giữ điểm nhỏ nhất. Điều này đúng vì mỗi chuỗi được xây dựng hợp pháp đều xuất hiện chính xác một lần, ngoại trừ sự trùng lặp vô hại khi hai thành viên trong nhóm có cùng ký tự ở một vị trí. 

Vấn đề là số lượng lá. Với độ dài 28, có (3^{28}=22,876,792,454,961) chuỗi có thể có cho một đội. Ngay cả khi việc tính toán từng hàm băm được giảm xuống theo thời gian không đổi thì hàng chục nghìn tỷ ứng viên vẫn không thể kiểm tra được. Việc tính toán lại toàn bộ hàm băm ở mỗi lá sẽ khiến tình hình trở nên tồi tệ hơn. 

Quan sát quan trọng là một chuỗi có thể được chia thành hai phần độc lập. Giả sử mảnh bên trái có hàm băm (L), mảnh bên phải có hàm băm (R) và mảnh bên phải có chiều dài (k). Sự nối của chúng có hàm băm 

[ 
(L\cdot127^k+R)\bmod MOD. 
] 

Vì vậy, chúng ta có thể liệt kê từng nửa bên trái và mỗi nửa bên phải có thể có một cách riêng biệt. Với tối đa 14 vị trí ở mỗi hiệp, mỗi bên có nhiều nhất (3^{14}=4,782,969) khả năng. Đây là sự giảm thiểu gặp gỡ ở giữa. 

Có một quan sát hữu ích hơn vì giá trị cuối cùng được lấy theo modulo (MOD). Đối với hàm băm trái cố định, hãy xác định 

[ 
X=(L\cdot127^k)\bmod MOD. 
] 

Chúng ta cần giảm thiểu 

[ 
(X+R)\bmod MOD 
] 

trên tất cả các giá trị băm phải có thể có (R). 

Nếu (X+R<MOD), kết quả chỉ đơn giản là (X+R), do đó, trong số tất cả các ứng cử viên không gói, hàm băm bên phải nhỏ nhất là tốt nhất. Nếu (X+R\ge MOD), kết quả là (X+R-MOD), do đó, ứng cử viên gói tốt nhất là hàm băm bên phải nhỏ nhất thỏa mãn 

[ 
R\ge MOD-X. 
] 

Sau khi sắp xếp tất cả các giá trị băm phù hợp, ứng viên đó sẽ được tìm thấy bằng một lần tìm kiếm nhị phân. Vì vậy, chúng ta không cần phải lưu trữ hoặc sắp xếp nửa bên trái. 

Lực lượng vũ phu hoạt động vì mọi lựa chọn đều độc lập, nhưng thất bại vì nó khám phá tích Descartes của tất cả các lựa chọn vị trí. Quan sát thấy hàm băm của một phép nối tách thành hàm băm bên trái được biến đổi cộng với hàm băm bên phải cho phép chúng ta thay thế tích Descartes khổng lồ đó bằng hai tập hợp trạng thái gần đúng (3^{14}) và tra cứu logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(3^n n)) | (O(n)) | Quá chậm | 
| Gặp gỡ ở giữa | (O(3^{n/2}\log 3^{n/2})) | (O(3^{n/2})) | Đã chấp nhận | 

Việc triển khai bên dưới sử dụng một tối ưu hóa bổ sung nhỏ cho việc sử dụng bộ nhớ Python. Một nửa chiều dài 14 được chia thành hai phần có tối đa 7 ký tự khi tạo giá trị băm của nó. Khi đó, mỗi phần có tối đa (3^7=2187) giá trị và tích Descartes của chúng tạo ra các giá trị băm cần thiết (3^{14}) mà không giữ đồng thời danh sách kích thước trung gian (3^8,3^9,\ldots,3^{13}). 

## Hướng dẫn thuật toán 

1. Đối với đội hiện tại, hãy chia các vị trí chuỗi của nó thành tiền tố có độ dài (L=\lfloor n/2\rfloor) và hậu tố chứa các vị trí còn lại. Việc phân chia giữ cho cả hai phần có độ dài tối đa là 14, vì vậy không bên nào có nhiều hơn (3^{14}) chuỗi có thể. 
2. Tạo tất cả các giá trị băm có thể có của hậu tố. Đối với một phần chuỗi, hãy cập nhật hàm băm của nó bằng`hash = (hash * 127 + character) % MOD`. Các ký tự bằng nhau ở cùng một vị trí có thể được loại bỏ trùng lặp vì việc chọn bản sao đầu tiên hoặc bản sao thứ hai đều tạo ra cùng một chuỗi kết quả. 
3. Sắp xếp các giá trị băm hậu tố. Việc sắp xếp cho chúng ta khả năng tìm hàm băm hậu tố nhỏ nhất lớn hơn hoặc bằng bất kỳ ngưỡng bắt buộc nào bằng cách sử dụng`bisect_left`. 
4. Tạo giá trị băm của tiền tố theo cách tương tự. Chúng ta không cần lưu trữ tất cả các giá trị băm tiền tố kết hợp, bởi vì mỗi giá trị băm có thể được khớp ngay lập tức với mảng hậu tố đã được sắp xếp. 
5. Đối với hàm băm tiền tố (L_h), gọi (R) là độ dài hậu tố và tính toán 

[ 
X=(L_h\cdot127^R)\bmod MOD. 
] 

Mỗi chuỗi hoàn chỉnh sử dụng tiền tố này đều có điểm 

[ 
(X+H_r)\bmod MOD 
] 

đối với một số hàm băm hậu tố (H_r). 
6. Xét hàm băm hậu tố nhỏ nhất. Nếu như`X + smallest_suffix < MOD`, nó mang lại kết quả không gói tốt nhất cho tiền tố này. Không có hậu tố lớn hơn nào có thể cải thiện kết quả không gói vì biểu thức đang tăng lên trong hàm băm hậu tố. 
7. Tìm hàm băm hậu tố đầu tiên thỏa mãn`suffix >= MOD - X`. Nếu giá trị đó tồn tại, nó sẽ cho kết quả gói tốt nhất. Một lần nữa, mỗi hàm băm hậu tố sau này tạo ra một kết quả ít nhất cũng lớn bằng vì sau khi gói biểu thức là`X + suffix - MOD`. 
8. Giữ ứng cử viên nhỏ nhất trên mỗi hàm băm tiền tố. Đây là số điểm tối thiểu có thể có của đội. 
9. Thực hiện quy trình tương tự cho đội kia và so sánh kết quả của hai đội. In`Owls`nếu điểm của Cú nhỏ hơn,`Goats`nếu điểm của Dê nhỏ hơn và`Tie`nếu không thì. 

Tại sao nó hoạt động theo bất biến tiền tố cố định. Khi hàm băm tiền tố được cố định, mọi điểm hoàn chỉnh có thể có đều có dạng`(X + suffix_hash) mod MOD`. Trong số các băm hậu tố bên dưới`MOD-X`, biểu thức tăng dần, do đó hàm băm hậu tố nhỏ nhất là tối ưu. Trong số các băm hậu tố ít nhất`MOD-X`, biểu thức được bao bọc cũng tăng lên, do đó hàm băm hậu tố đầu tiên ở ngưỡng đó là tối ưu. Đó là hai dạng duy nhất có thể có của kết quả modulo, vì vậy việc kiểm tra cả hai sẽ tìm ra cách hoàn thành tốt nhất cho mọi tiền tố. Vì mọi tiền tố có thể đều được kiểm tra nên mức tối thiểu toàn cục được tìm thấy. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

MOD = 1000000000000037
BASE = 127

def distinct_choices(strings, pos):
    a = ord(strings[0][pos])
    b = ord(strings[1][pos])
    c = ord(strings[2][pos])

    if a == b == c:
        return (a,)
    if a == b:
        return (a, c)
    if a == c:
        return (a, b)
    if b == c:
        return (a, b)

    return (a, b, c)

def small_hashes(strings, left, right):
    """All hashes for a segment of length at most 7."""
    values = [0]

    for pos in range(left, right):
        choices = distinct_choices(strings, pos)
        values = [
            (value * BASE + ch) % MOD
            for value in values
            for ch in choices
        ]

    return values

def segment_hashes(strings, left, right):
    """
    Generate all hashes for a segment of length at most 14.

    Split it into two pieces of at most 7 characters so that
    intermediate Python lists stay small.
    """
    length = right - left

    if length <= 7:
        return small_hashes(strings, left, right)

    middle = left + length // 2

    first = small_hashes(strings, left, middle)
    second = small_hashes(strings, middle, right)

    power = pow(BASE, right - middle, MOD)

    return [
        (x * power + y) % MOD
        for x in first
        for y in second
    ]

def best_score(strings):
    n = len(strings[0])

    left_len = n // 2
    right_start = left_len

    # Generate and sort every possible suffix hash.
    suffix_hashes = segment_hashes(strings, right_start, n)
    suffix_hashes.sort()

    min_suffix = suffix_hashes[0]
    right_len = n - right_start
    right_power = pow(BASE, right_len, MOD)

    # Generate prefix hashes in two small pieces.
    if left_len <= 7:
        prefix_hashes = small_hashes(strings, 0, left_len)
        prefix_parts = (prefix_hashes, None, 1)
    else:
        middle = left_len // 2
        first = small_hashes(strings, 0, middle)
        second = small_hashes(strings, middle, left_len)
        power_between = pow(BASE, left_len - middle, MOD)
        prefix_parts = (first, second, power_between)

    best = MOD

    first, second, power_between = prefix_parts

    if second is None:
        for prefix_hash in first:
            x = (prefix_hash * right_power) % MOD

            # Best non-wrapping candidate.
            candidate = x + min_suffix
            if candidate < MOD and candidate < best:
                best = candidate

            # Best wrapping candidate.
            threshold = MOD - x
            idx = bisect_left(suffix_hashes, threshold)

            if idx < len(suffix_hashes):
                candidate = x + suffix_hashes[idx] - MOD
                if candidate < best:
                    best = candidate
    else:
        for first_hash in first:
            base = (first_hash * power_between) % MOD

            for second_hash in second:
                prefix_hash = (base + second_hash) % MOD
                x = (prefix_hash * right_power) % MOD

                # Best non-wrapping candidate.
                candidate = x + min_suffix
                if candidate < MOD and candidate < best:
                    best = candidate

                # Best wrapping candidate.
                threshold = MOD - x
                idx = bisect_left(suffix_hashes, threshold)

                if idx < len(suffix_hashes):
                    candidate = x + suffix_hashes[idx] - MOD
                    if candidate < best:
                        best = candidate

    return best

def main():
    A, B = map(int, input().split())

    owls = [input().strip() for _ in range(3)]
    goats = [input().strip() for _ in range(3)]

    owls_score = best_score(owls)
    goats_score = best_score(goats)

    if owls_score < goats_score:
        print("Owls")
    elif goats_score < owls_score:
        print("Goats")
    else:
        print("Tie")

if __name__ == "__main__":
    main()
```các`distinct_choices`hàm loại bỏ các ký tự lặp lại tại một vị trí. Đây chỉ là một sự tối ưu hóa, bởi vì các lựa chọn lặp lại biểu thị cùng một ký tự và do đó có cùng một chuỗi được xây dựng.`small_hashes`liệt kê tất cả các chuỗi của một phân đoạn bằng cách liên tục mở rộng các giá trị băm hiện có bằng một ký tự. Phân khúc được cố tình giới hạn ở bảy vị trí. Tại bảy vị trí có nhiều nhất là 2187 trạng thái, một con số rất nhỏ so với tập hợp trạng thái cuối cùng (3^{14}).`segment_hashes`kết hợp hai phân đoạn nhỏ như vậy. Nếu phần đầu tiên có hàm băm`x`và phần thứ hai có hàm băm`y`, hàm băm nối là 

[ 
(x\cdot127^{|giây|}+y)\bmod MOD. 
] 

Đây chính xác là đại số cần thiết cho phép chia gặp nhau ở giữa.`best_score`sắp xếp các giá trị băm hậu tố một lần. Đối với mỗi tiền tố,`right_power`thay đổi hàm băm của nó theo số lượng ký tự hậu tố. các`min_suffix`ứng viên xử lý trường hợp không gói, trong khi`bisect_left`tìm thấy hậu tố đầu tiên gây ra sự bao bọc và do đó là ứng cử viên bao bọc tốt nhất. 

Không có vấn đề tràn số nguyên trong Python vì số nguyên có độ chính xác tùy ý. Các phép toán modulo rõ ràng vẫn cần thiết vì bài toán xác định modulo điểm (MOD) và việc giữ các giá trị ở mức giảm cũng giúp phép tính hiệu quả. 

Các ranh giới phân chia sử dụng các khoảng thời gian nửa mở. Tiền tố là`[0, left_len)`, trong khi hậu tố là`[left_len, n)`. Điều này tránh việc vô tình bỏ sót hoặc trùng lặp ký tự ở vị trí tách. 

Việc xử lý đặc biệt ranh giới bảy ký tự cũng có chủ ý. Khi một đoạn có độ dài tối đa là bảy, nó sẽ được tạo trực tiếp. Khi dài hơn, nó được chia thành hai đoạn nhỏ hơn. Điều này giữ cho danh sách tạm thời lớn nhất ở mức nhỏ trong khi vẫn giữ nguyên bộ giá trị băm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
6 6
ANDRES
FELIPE
MANUEL
VICTOR
IVANSS
DIEGOS
```Đối với mỗi đội, độ dài là sáu, vì vậy phần chia ở giữa có ba nhân vật ở mỗi bên. Mỗi bên có tối đa (3^3=27) giá trị băm có thể có. 

Đối với Cú, ba vị trí đầu tiên có thể tạo ra 27 giá trị băm tiền tố có thể có và ba vị trí cuối cùng có thể tạo ra 27 giá trị băm hậu tố. Băm hậu tố được sắp xếp cho phép thuật toán tìm hậu tố tốt nhất cho mọi tiền tố. 

Dấu vết tương ứng là: 

| Biến | Cú | Dê | 
| --- | --- | --- | 
| Chiều dài | 6 | 6 | 
| Độ dài tiền tố | 3 | 3 | 
| Độ dài hậu tố | 3 | 3 | 
| Số băm tối đa mỗi nửa | 27 | 27 | 
| Kết quả cuối cùng | Nhỏ hơn | Lớn hơn | 
| Người chiến thắng | Cú | | 

Phần quan trọng của ví dụ này là thuật toán không bao giờ xây dựng tất cả (3^6=729) chuỗi hoàn chỉnh. Nó chỉ xây dựng hai bộ 27 nửa băm và kết hợp chúng thông qua bất đẳng thức mô-đun. 

### Mẫu 2 

Đầu vào là```
1 28
E
L
I
AAAAAAAAAAAAAAAAAAAAAAAAAAAA
BBBBBBBBBBBBBBBBBBBBBBBBBBBB
CCCCCCCCCCCCCCCCCCCCCCCCCCCC
```Cú chỉ có một vị trí. Điểm có thể có của họ là 69, 76 và 73, vì vậy điểm tối thiểu của họ là 69. 

Dê có 28 vị trí, nhưng mỗi vị trí đều có ba lựa chọn giống nhau,`A`,`B`, Và`C`. Thuật toán chia 28 vị trí này thành hai nhóm 14. Mỗi nửa chứa tối đa (3^{14}=4.782.969) khả năng, mặc dù cấu trúc lặp lại tạo ra ít giá trị băm khác biệt hơn nhiều trong thực tế. 

Dấu vết ở mức cao là: 

| Biến | Cú | Dê | 
| --- | --- | --- | 
| Chiều dài | 1 | 28 | 
| Độ dài tiền tố | 0 | 14 | 
| Độ dài hậu tố | 1 | 14 | 
| Nửa trạng thái tối đa | 3 | 4.782.969 | 
| Điểm cuối cùng tối thiểu | 69 | Lớn hơn 69 | 
| Người chiến thắng | Cú | | 

Đa thức của Dê lớn hơn nhiều so với mô đun trước khi rút gọn, vì vậy ví dụ này cũng thực hiện băm mô đun thay vì so sánh số nguyên thông thường. Đầu ra mẫu chính thức là`Goats`bởi vì điểm tối thiểu của Dê thực sự nhỏ hơn sau phép tính modulo. 

Mẫu này đặc biệt hữu ích vì nó chứng minh tại sao cường độ đa thức thô không thể được sử dụng làm đại diện cho điểm số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(3^{n/2}\log 3^{n/2})) | Tạo tối đa (3^{n/2}) băm hậu tố, sắp xếp chúng, sau đó thực hiện một tìm kiếm nhị phân cho mỗi kết hợp tiền tố | 
| Không gian | (O(3^{n/2})) | Các băm hậu tố được sắp xếp chiếm ưu thế trong việc sử dụng bộ nhớ | 

Đối với (n=28), nửa lớn nhất chứa (3^{14}=4,782,969) kết hợp. Thuật toán xử lý hai nhóm một cách độc lập nên mảng hậu tố lớn được giải phóng trước khi nhóm kia xử lý. Việc chia nhỏ bảy ký tự cũng tránh được việc giữ lại một số danh sách Python trung gian lớn cùng một lúc. Điều này giúp việc triển khai trong giới hạn bộ nhớ 256 MB thoải mái hơn nhiều so với việc xây dựng danh sách băm 14 ký tự thông qua việc mở rộng kích thước đầy đủ lặp đi lặp lại. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`main`.```python
# helper: run solution on input string, return output string
import sys
import io

from solution import main

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        main()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """6 6
ANDRES
FELIPE
MANUEL
VICTOR
IVANSS
DIEGOS
"""
) == "Owls", "sample 1"

# Provided sample 2
assert run(
    """1 28
E
L
I
AAAAAAAAAAAAAAAAAAAAAAAAAAAA
BBBBBBBBBBBBBBBBBBBBBBBBBBBB
CCCCCCCCCCCCCCCCCCCCCCCCCCCC
"""
) == "Goats", "sample 2"

# Minimum size, and all choices identical on both sides.
assert run(
    """1 1
A
A
A
A
A
A
"""
) == "Tie", "minimum size and identical choices"

# Small boundary case with different lengths.
# Owls can only make "AA", score 65*127+65 = 8320.
# Goats can only make "Z", score 90.
assert run(
    """2 1
AA
AA
AA
Z
Z
Z
"""
) == "Goats", "different lengths and two-character hash"

# Maximum size with identical values.
# Both teams can produce exactly the same 28-character string.
assert run(
    """28 28
AAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAA
"""
) == "Tie", "maximum length and all equal values"

# Duplicate choices at every position.
# The three members on each side are identical, so there is only one
# distinct constructed string per team.
assert run(
    """3 3
ABC
ABC
ABC
ABD
ABD
ABD
"""
) == "Owls", "duplicate choices and exact boundary split"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`Owls`| Ví dụ chính thức và hành vi gặp gỡ thông thường | 
| Mẫu 2 |`Goats`| Xử lý độ dài một và bao bọc mô-đun trên chuỗi 28 ký tự | 
|`1 1`với sáu`A`dây |`Tie`| Kích thước tối thiểu và điểm số giống hệt nhau | 
|`2 1`với`AA`so với`Z`|`Goats`| Độ dài không bằng nhau và số mũ băm không cần thiết đầu tiên | 
|`28 28`với tất cả`A`|`Tie`| Độ dài đầu vào tối đa và giá trị lặp lại | 
|`3 3`với các chuỗi thành viên lặp đi lặp lại |`Owls`| Loại bỏ các lựa chọn giống nhau cho mỗi vị trí | 

## Vỏ cạnh 

### Chiều dài Một 

Hãy xem xét```
1 1
E
L
I
X
Y
Z
```Cú có thể chọn`E`,`L`, hoặc`I`, vậy điểm tối thiểu của họ là`69`. Dê có thể chọn`X`,`Y`, hoặc`Z`, vậy điểm tối thiểu của họ là`88`. 

Tiền tố của Owls có độ dài bằng 0 và hậu tố của chúng có độ dài bằng 1. Mảng hậu tố chứa ba giá trị ASCII 69, 73 và 76. Giá trị băm tiền tố đơn bằng 0, do đó thuật toán đánh giá trực tiếp hậu tố nhỏ nhất và thu được 69. Tương tự, Dê thu được 88, cho kết quả`Owls`. 

Điều này phát hiện các triển khai vô tình sử dụng lũy ​​thừa 127 không chính xác cho ký tự cuối cùng. 

### Lựa chọn lặp lại 

Hãy xem xét```
3 3
ABC
ABC
ABC
ABD
ABD
ABD
```Ở mọi vị trí Cú, cả ba thành viên đều có cùng một ký tự, do đó có chính xác một chuỗi có thể,`ABC`. Tương tự, Dê có chính xác một chuỗi có thể,`ABD`.`distinct_choices`biến ba lựa chọn ký tự bằng nhau ở mỗi vị trí thành một lựa chọn. Do đó, các bộ băm được tạo chứa một giá trị mỗi bên thay vì (3^3) cấu trúc trùng lặp. Việc so sánh sau đó chỉ đơn giản là so sánh hàm băm của`ABC`với hàm băm của`ABD`, cho`Owls`. 

Tính chính xác không phụ thuộc vào sự trùng lặp. Nó chỉ loại bỏ các nhánh tương đương. 

### Độ dài một nửa không bằng nhau 

Hãy xem xét```
2 1
AA
AA
AA
Z
Z
Z
```Đối với Cú, sự phân chia là một ký tự cộng với một ký tự. Chuỗi duy nhất có thể là`AA`, số điểm của ai là 

[ 
65\cdot127+65=8320. 
] 

Đối với Dê, chuỗi duy nhất có thể là`Z`, với điểm 90. Thuật toán xử lý các độ dài khác nhau một cách độc lập, do đó hàm băm hai ký tự của Cú và hàm băm một ký tự của Dê không bao giờ bị trộn lẫn. Kết quả là`Goats`. 

Điều này phát hiện ra lỗi từng cái một trong đó độ dài hậu tố được tính từ độ dài nhóm ban đầu thay vì phần tách hiện tại. 

### Bao quanh mô-đun 

Hãy xem xét lại mẫu thứ hai:```
1 28
E
L
I
AAAAAAAAAAAAAAAAAAAAAAAAAAAA
BBBBBBBBBBBBBBBBBBBBBBBBBBBB
CCCCCCCCCCCCCCCCCCCCCCCCCCCC
```Cú có điểm tối thiểu là 69. Dê có giá trị đa thức tăng theo cấp số nhân vì chuỗi của chúng chứa 28 vị trí. Giá trị được giảm đi nhiều lần theo modulo (10^{15}+37), do đó, điểm cuối cùng không liên quan đơn điệu đến đa thức không được sửa đổi. 

Đối với hàm băm tiền tố Goats (L) cố định, thuật toán tính toán 

[ 
X=(L\cdot127^{14})\bmod MOD. 
] 

Sau đó, nó tìm kiếm các băm hậu tố được sắp xếp cho giá trị đầu tiên ít nhất`MOD - X`. Hậu tố đó chính xác là hậu tố đầu tiên có phép cộng bao quanh mô đun. Giá trị được bao bọc kết quả được so sánh với giá trị được bao bọc tốt nhất từ ​​hàm băm hậu tố nhỏ nhất. 

Thuật toán không bao giờ giả định rằng đa thức lớn hơn phải có điểm lớn hơn. Nó so sánh dư lượng thực tế, đó là những gì vấn đề yêu cầu. 

### Ngưỡng chính xác trong quá trình tìm kiếm nhị phân 

Giả sử đối với một số tiền tố, giá trị được tính toán là (X) và tập hậu tố chứa chính xác 

[ 
MOD-X. 
] 

Sau đó 

[ 
(X+(MOD-X))\bmod MOD=0. 
]`bisect_left`cố tình tìm kiếm hậu tố đầu tiên lớn hơn hoặc bằng ngưỡng, thay vì lớn hơn một cách nghiêm ngặt. Trường hợp đẳng thức này phải được chấp nhận vì nó tạo ra điểm tối ưu có thể có, bằng 0. 

Việc thực hiện bất cẩn bằng cách sử dụng`bisect_right`sẽ bỏ qua một ứng cử viên có kết quả bằng 0 và có thể trả về số điểm lớn hơn. 

### Độ dài tối đa 

Đối với một nhóm có độ dài 28, mỗi nửa chứa tối đa (3^{14}=4,782,969) chuỗi có thể. Quá trình triển khai xử lý mảng hậu tố của một nhóm tại một thời điểm và xây dựng bộ 14 ký tự bằng cách kết hợp hai bộ bảy ký tự. Phần sau chứa tối đa 2187 phần tử, vì vậy phân bổ lớn duy nhất là mảng hậu tố cuối cùng. 

Các kết hợp tiền tố được xử lý trực tiếp theo mảng hậu tố được sắp xếp thay vì lưu trữ một mảng khác gồm gần năm triệu số nguyên Python. Cách xử lý bất đối xứng đó đặc biệt hữu ích trong Python vì danh sách thông thường gồm hàng triệu số nguyên Python tiêu tốn nhiều bộ nhớ hơn đáng kể so với cùng số lượng số nguyên nhỏ gọn trong vectơ C++. 

Thuật toán kết quả vẫn kiểm tra không gian trạng thái gặp nhau ở giữa đầy đủ (3^{14}) khi đầu vào đa dạng tối đa nhưng không bao giờ tiếp cận không gian tìm kiếm cưỡng bức (3^{28}).
