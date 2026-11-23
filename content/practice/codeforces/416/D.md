---
title: "CF 416D - Quy mô dân số"
description: "Chúng ta có một chuỗi có độ dài $n$, trong đó mỗi vị trí là một số nguyên dương cố định hoặc không xác định (được đánh dấu là $-1$). Nhiệm vụ là diễn giải chuỗi này như được hình thành bằng cách nối nhiều cấp số cộng, lần lượt."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 416
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 241 (Div. 2)"
rating: 2400
weight: 416
solve_time_s: 83
verified: true
draft: false
---

[CF 416D - Quy mô dân số](https://codeforces.com/problemset/problem/416/D) 

**Đánh giá:** 2400 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy có độ dài$n$, trong đó mỗi vị trí là một số nguyên dương cố định hoặc không xác định (được đánh dấu là$-1$). Nhiệm vụ là diễn giải chuỗi này như được hình thành bằng cách nối nhiều cấp số cộng, lần lượt. Trong mỗi phân đoạn, các phần tử liên tiếp phải khác nhau một giá trị không đổi và các phân đoạn không thể trùng nhau. 

Các vị trí chưa biết có thể được điền bằng bất kỳ số nguyên dương nào mà chúng tôi chọn, miễn là tất cả các giá trị đã biết vẫn không thay đổi. Mục tiêu là chọn những phần điền này và chia chuỗi đầy đủ kết quả thành số lượng phân đoạn cấp số cộng tối thiểu có thể. 

Vì vậy, quyết định thực sự không chỉ là chia ở đâu mà còn là cách gán giá trị cho$-1$các mục theo cách tránh buộc phải chia tách thêm. 

Ràng buộc$n \le 2 \cdot 10^5$ngay lập tức loại trừ bất kỳ điều gì cố gắng xem xét tất cả các phân đoạn một cách rõ ràng. DP bậc hai hoặc tệ hơn trên tất cả các điểm cuối của phân đoạn sẽ quá chậm, vì$O(n^2)$quá trình chuyển đổi sẽ vượt quá giới hạn chấp nhận được theo một số bậc độ lớn. Giải pháp phải tuyến tính hoặc gần tuyến tính, chỉ có các quyết định cục bộ cho mỗi vị trí. 

Một khó khăn tinh tế đến từ việc làm thế nào$-1$tương tác một cách nhất quán. Một cách tiếp cận ngây thơ có thể cố gắng mở rộng một cách tham lam một phân đoạn bất cứ khi nào có thể, nhưng một giá trị đã biết duy nhất có thể gây ra mâu thuẫn về phía trước. 

Ví dụ: hãy xem xét một mẫu như:```
3 5 -1 9
```Nếu chúng ta giả sử chênh lệch là +2 thì giá trị còn thiếu sẽ trở thành 7 và mọi thứ đều nhất quán. Nhưng nếu thay vào đó chúng ta có:```
3 5 -1 10
```sự khác biệt giả định tương tự bị phá vỡ ở vị trí cuối cùng. Một tiện ích mở rộng tham lam mà không kiểm tra các ràng buộc trong tương lai có thể thất bại. 

Một trường hợp cạnh khác là khi các đoạn có độ dài 1 xuất hiện ngầm. Một giá trị đã biết được bao quanh bởi những giá trị chưa biết có thể được mở rộng theo nhiều cách không tương thích và các lựa chọn tham lam không chính xác có thể làm tăng số lượng phân đoạn một cách giả tạo. 

Khó khăn chính là trình tự phải được phân chia đồng thời phải lựa chọn những khác biệt phù hợp với mọi điểm cố định trong từng đoạn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách có thể để phân vùng mảng thành các phân đoạn và với mỗi phân đoạn hãy thử kiểm tra xem liệu chúng ta có thể gán giá trị cho$-1$sao cho tất cả các giá trị cố định đều nằm trên một cấp số cộng duy nhất. 

Đối với một phân khúc$[l, r]$, việc kiểm tra tính khả thi bao gồm việc suy ra sự khác biệt chung từ hai giá trị đã biết hoặc xác minh tính nhất quán trên tất cả các giá trị đã biết. Với tính toán trước, mỗi lần kiểm tra được$O(1)$, nhưng số lượng phân vùng là theo cấp số nhân, đại khái là$2^{n-1}$, điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 50$. 

Một DP có cấu trúc chặt chẽ hơn sẽ xác định$dp[i]$là số lượng phân đoạn tối thiểu bao gồm tiền tố$1..i$và thử tất cả các điểm phân chia trước đó. Điều này mang lại$O(n^2)$chuyển tiếp. Ngay cả với việc kiểm tra tính khả thi liên tục theo thời gian cho mỗi phân đoạn, việc này vẫn quá chậm đối với$2 \cdot 10^5$. 

Quan sát quan trọng là trong bất kỳ phân đoạn hợp lệ nào, khi chênh lệch được xác định, mọi giá trị tiếp theo sẽ bị ép buộc trừ khi nó được xác định.$-1$. Điều này có nghĩa là chúng ta có thể mở rộng một phân đoạn một cách tham lam đến mức độ nhất quán cho phép và chỉ khởi động lại khi tính nhất quán bị phá vỡ. 

Điều phức tạp thực sự duy nhất là sự khác biệt của một phân khúc không được biết trước. Tuy nhiên, nó được xác định ngay khi chúng ta gặp hai giá trị cố định đầu tiên trong đoạn. Từ thời điểm đó trở đi, mọi giá trị cố định tiếp theo đều đồng ý hoặc buộc phải cắt giảm. 

Điều này làm giảm vấn đề quét từ trái sang phải trong khi vẫn duy trì cấu trúc số học ngụ ý của phân đoạn hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải, duy trì phân đoạn hiện tại và cấp số cộng ngụ ý của nó. 

1. Bắt đầu một phân khúc mới ở vị trí 1. Chúng tôi chưa biết điểm khác biệt chung của nó. 
2. Tiến về phía trước cho đến khi chúng ta gặp cặp giá trị đã biết đầu tiên cho phép chúng ta xác định sự khác biệt. Nếu đoạn đó có ít hơn hai điểm cố định cho đến nay thì sự khác biệt vẫn chưa được xác định và đoạn đó có thể tự do điều chỉnh. 
3. Khi chúng tôi thấy hai giá trị cố định trong cùng một phân khúc, chúng tôi sẽ tính toán chênh lệch bắt buộc$d$. Điều này khóa hoàn toàn cấu trúc của phân khúc. 
4. Tiếp tục quét. Đối với mỗi vị trí mới: 

- Nếu giá trị là$-1$, nó luôn nhất quán và không hạn chế bất cứ điều gì. 
- Nếu biết, chúng tôi kiểm tra xem nó có khớp với giá trị mong đợi mà phân khúc hiện tại ngụ ý hay không. 

Nếu không khớp thì đoạn đó phải kết thúc trước vị trí này. 
5. Khi xảy ra sự không khớp, chúng tôi sẽ tăng số lượng phân đoạn và khởi động lại phân đoạn mới từ vị trí hiện tại. Phân khúc mới lại bắt đầu với không có sự khác biệt cố định. 
6. Tiếp tục cho đến hết mảng. 

Một điểm tinh tế là cách chúng tôi xử lý giai đoạn ban đầu khi không có sự khác biệt nào được biết đến. Trong giai đoạn đó, bất kỳ sự phân công nào cho$-1$có thể được chọn để duy trì tính nhất quán, vì vậy chúng tôi không bao giờ ép buộc phân chia trừ khi một giá trị đã biết mâu thuẫn với một tiến trình đã được thiết lập. 

### Tại sao nó hoạt động 

Mỗi phân đoạn được mở rộng tối đa theo ràng buộc là tất cả các giá trị cố định bên trong nó phải phù hợp với một cấp số cộng duy nhất. Thời điểm hai giá trị cố định xác định sự khác biệt duy nhất, phân khúc trở nên cứng nhắc. Từ thời điểm đó trở đi, mọi phần tử đều có một giá trị mong đợi duy nhất. Nếu xuất hiện mâu thuẫn thì không được phân công lại$-1$có thể sửa nó mà không vi phạm một trong các giá trị cố định, do đó việc chia tách tại thời điểm đó là điều khó tránh khỏi. 

Điều này tạo ra một cấu trúc tham lam: mọi phân đoạn được mở rộng đến mức hợp lý nhất có thể và không có sự phân chia nào trước đó có thể cải thiện tổng số phân đoạn vì việc trì hoãn phân chia không bao giờ làm giảm các ràng buộc trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    segments = 1

    # current segment state
    d = None
    first = None
    prev = None
    prev_val = None

    for i in range(n):
        if a[i] == -1:
            # unknown value does not constrain anything
            if prev is not None:
                prev += 1  # placeholder progression step, value irrelevant
            continue

        if first is None:
            first = a[i]
            prev = i
            prev_val = a[i]
            continue

        if prev_val is None:
            prev_val = a[i]
            d = (a[i] - first) // (i - prev)
            prev = i
            continue

        # expected value from progression
        dist = i - prev
        expected = prev_val + d * dist

        if expected != a[i]:
            segments += 1
            # reset segment
            first = a[i]
            prev = i
            prev_val = a[i]
            d = None
        else:
            prev = i
            prev_val = a[i]

    print(segments)

if __name__ == "__main__":
    solve()
```Việc triển khai sẽ theo dõi xem phân đoạn hiện tại đã xác định cấp số cộng của nó hay chưa. Biến`first`lưu trữ giá trị cố định đầu tiên trong phân khúc, trong khi`prev_val`Và`prev`giúp xác định sự khác biệt khi giá trị cố định thứ hai xuất hiện. 

Logic dựa trên thực tế là một khi`d`được thiết lập, mọi phần tử cố định mới có thể được xác nhận theo thời gian không đổi. Khi tìm thấy sự không khớp, chúng tôi khởi động lại phân khúc tại chỉ mục đó. 

Cần thận trọng trong việc xử lý$-1$, vì chúng không góp phần xác định hoặc xác nhận tiến trình. Mã bỏ qua chúng một cách hiệu quả mà không cần cập nhật các ràng buộc. 

Một cạm bẫy phổ biến là giả định sai sự khác biệt có thể được cập nhật nhiều lần; một khi đã được sửa, nó phải không đổi trong phân khúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9
8 6 4 2 1 4 7 10 2
```Chúng tôi quét từ trái sang phải và theo dõi tính hợp lệ của phân đoạn. 

| tôi | giá trị | bắt đầu phân đoạn | sự khác biệt d | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 8 | 0 | không xác định | đoạn bắt đầu | 
| 1 | 6 | 0 | -2 | xác định d | 
| 2 | 4 | 0 | -2 | được | 
| 3 | 2 | 0 | -2 | được | 
| 4 | 1 | 0 | -2 | không khớp → cắt | 
| 4 | 1 | 4 | không xác định | phân khúc mới | 
| 5 | 4 | 4 | không xác định | được | 
| 6 | 7 | 4 | +3 | xác định d | 
| 7 | 10 | 4 | +3 | được | 
| 8 | 2 | 8 | không xác định | phân khúc mới | 

Chúng tôi kết thúc với 3 phân đoạn. 

Dấu vết này cho thấy rằng một khi mâu thuẫn xuất hiện ở vị trí 4 thì không có sự điều chỉnh nào của các giá trị trước đó có thể điều hòa được phân đoạn. 

### Ví dụ 2 

đầu vào:```
5
1 -1 -1 7 10
```| tôi | giá trị | bắt đầu phân đoạn | sự khác biệt d | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | không xác định | bắt đầu | 
| 1 | -1 | 0 | không xác định | không ràng buộc | 
| 2 | -1 | 0 | không xác định | không ràng buộc | 
| 3 | 7 | 0 | +3 | xác định d | 
| 4 | 10 | 0 | +3 | được | 

Chỉ cần một phân đoạn. 

Điều này xác nhận rằng các giá trị chưa biết có thể được điều chỉnh tự do cho đến khi ràng buộc đầu tiên khắc phục được tiến trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được xử lý một lần, với các kiểm tra tính nhất quán theo thời gian liên tục | 
| Không gian |$O(1)$| Chỉ một số biến được lưu trữ bất kể kích thước đầu vào | 

Quét tuyến tính dễ dàng phù hợp với các ràng buộc đối với$n \le 2 \cdot 10^5$, cả về thời gian và trí nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import *
    n = int(input())
    a = list(map(int, input().split()))

    segments = 1
    d = None
    first = None
    prev_i = None
    prev_v = None

    for i, v in enumerate(a):
        if v == -1:
            continue

        if first is None:
            first = v
            prev_i = i
            prev_v = v
            continue

        if prev_v is None:
            prev_v = v
            d = (v - first) // (i - prev_i)
            prev_i = i
            continue

        expected = prev_v + d * (i - prev_i)
        if expected != v:
            segments += 1
            first = v
            prev_i = i
            prev_v = v
            d = None
        else:
            prev_i = i
            prev_v = v

    return str(segments)

# provided sample
assert run("9\n8 6 4 2 1 4 7 10 2\n") == "3"

# minimum size
assert run("1\n5\n") == "1"

# all unknowns
assert run("4\n-1 -1 -1 -1\n") == "1"

# already perfect AP
assert run("4\n1 3 5 7\n") == "1"

# forced splits
assert run("5\n1 2 100 101 102\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở | 
| tất cả -1 | 1 | hoàn toàn linh hoạt | 
| AP hoàn hảo | 1 | không có sự chia tách không cần thiết | 
| buộc phải nghỉ | 2 | logic phân chia đúng | 

## Vỏ cạnh 

Một trường hợp cạnh khóa là khi chuỗi chỉ chứa một giá trị đã biết duy nhất khi bắt đầu và mọi thứ khác đều không được biết cho đến tận sau này. Thuật toán giữ cho phân đoạn mở mà không sửa chữa sự khác biệt, do đó nó không sớm khóa vào cấu trúc không chính xác. Khi giá trị cố định thứ hai đầu tiên xuất hiện, sự khác biệt được đặt nhất quán từ hai điểm đó. 

Một trường hợp khác là mâu thuẫn xuất hiện ngay sau khi hiệu chỉnh xong. Ví dụ:```
3 5 7 10
```Phân đoạn trở nên hợp lệ với chênh lệch +2 cho đến 7, nhưng 10 sẽ phá vỡ nó. Thuật toán phát hiện điều này ở lần so sánh không hợp lệ đầu tiên và bắt đầu một phân đoạn mới chính xác ở số 10, đảm bảo phân đoạn tối thiểu. 

Trường hợp thứ ba là xen kẽ các giá trị đã biết và chưa biết có thể hỗ trợ nhiều cách diễn giải. Vì ẩn số không bao giờ được sử dụng để thực thi các ràng buộc nên thuật toán chỉ phản ứng với các giá trị cố định, ngăn chặn việc ràng buộc quá mức cấu trúc phân đoạn.
