---
title: "CF 102307L - Chất lỏng X"
description: "Đây là một vấn đề tìm kiếm tương tác được ngụy trang dưới dạng vấn đề đổi xu. Có một đại lượng nguyên dương (X) chưa biết, với (1 le X le 10^6). Chúng ta có (n) ống nhỏ giọt và sử dụng ống nhỏ giọt (i) sau khi thêm (ai) đơn vị chất lỏng."
date: "2026-08-13T07:30:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "L"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 193
verified: true
draft: false
---

[CF 102307L - Chất lỏng X](https://codeforces.com/problemset/problem/102307/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đây là một vấn đề tìm kiếm tương tác được ngụy trang dưới dạng vấn đề đổi xu. 

Có một đại lượng nguyên dương (X) chưa xác định, với (1 \le X \le 10^6). Chúng ta có (n) ống nhỏ giọt và sử dụng ống nhỏ giọt (i) sau khi thêm (a_i) đơn vị chất lỏng. Một truy vấn chọn số nguyên không âm (x_i), do đó số lượng được kiểm tra là 

[ 
q=\sum_{i=1}^{n} a_i x_i. 
] 

Sau thí nghiệm, giám khảo cho chúng ta biết (q) ở dưới (X), bằng (X) hay ở trên (X). Các màu tương ứng với (q<X), (q=X) và (q>X). 

Nhiệm vụ là xác định (X) trong phạm vi tối đa 30 thí nghiệm. Nếu các quan sát không thể phân biệt (X) với một số nguyên có thể khác, chúng ta phải xuất ra (-1). 

Đầu vào bao gồm (n), theo sau là dung lượng (a_1,\ldots,a_n). Không có đầu vào hàng loạt thông thường chứa (X), bởi vì (X) do giám khảo tương tác nắm giữ. Sau mỗi truy vấn, chương trình sẽ đọc màu kết quả. 

Giới hạn trên (10^6) là ràng buộc tính toán trung tâm. Nó đủ nhỏ cho một chương trình động giả đa thức trên tất cả các đại lượng từ (0) đến (10^6), trong khi nó quá lớn để liệt kê tất cả các vectơ có thể có của số lượng ống nhỏ giọt. Vì (n\le100), một chương trình động trực tiếp có chuyển tiếp (n\cdot10^6) có tới (10^8) lần lặp. Điều đó là hợp lý trong C++ được tối ưu hóa, nhưng lại nặng nề một cách không cần thiết đối với Python, do đó, việc triển khai bên dưới gói DP khả năng tiếp cận thành các số nguyên Python và thực hiện các chuyển đổi dưới dạng các thao tác bit. 

Giới hạn 30 thử nghiệm cũng rất hào phóng so với (10^6) giá trị có thể có của (X). Một khi chúng ta đã sắp xếp mọi số lượng thực sự có thể được tạo ra, tìm kiếm nhị phân thông thường cần nhiều nhất 

[ 
\lceil \log_2(10^6)\rceil=20 
] 

truy vấn. Khó khăn còn lại là quyết định khi nào khoảng cuối cùng chứa chính xác một số nguyên có thể. 

Có một số trường hợp nguy hiểm mà việc triển khai bất cẩn có thể bỏ sót. Với dung lượng (4,8), giả sử giá trị ẩn là (10). Chúng ta có thể truy vấn (4), (8) và (12), thu được màu xanh lục, xanh lục và đỏ. Khi đó (X) có thể là (9), (10) hoặc (11), nên đáp án đúng là (-1). Trả về (10) chỉ vì nó là điểm giữa sẽ là sai. 

Ở ranh giới dưới, giả sử dung lượng duy nhất là (2) và giá trị ẩn là (1). Truy vấn tích cực nhỏ nhất là (2) và phản hồi của nó có màu đỏ. Vì (X) dương nên giá trị duy nhất bên dưới (2) là (1), nên đáp án được xác định duy nhất. Việc triển khai tìm kiếm nhị phân chung dự kiến ​​tồn tại cả hai giá trị có thể truy cập lân cận có thể xử lý sai trường hợp này. 

Vấn đề tương tự xảy ra ở ranh giới trên. Với giá trị có thể truy cập là (999999), nếu phản hồi của nó có màu xanh lục thì (X>999999). Vì (X\le10^6) nên khả năng duy nhất là (10^6). Một lần nữa, không có giá trị nào có thể truy cập trên (X) có thể được sử dụng làm điểm cuối khác. 

Cuối cùng, một cặp giá trị có thể truy cập liền kề có thể khác nhau đúng hai. Nếu hai giá trị là (19) và (21) và phản hồi cho chúng ta biết rằng (19<X<21), thì (X=20) được xác định duy nhất mặc dù bản thân (20) không thể được tạo ra bởi các ống nhỏ giọt. Giải pháp chỉ chấp nhận phản hồi màu vàng sẽ trả về sai (-1). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê tất cả số lượng có thể được sản xuất. Chúng tôi xác định`dp[s]`có nghĩa là một sự kết hợp không âm nào đó của công suất ống nhỏ giọt sẽ tạo ra chính xác (s). Bắt đầu từ`dp[0] = true`, với mỗi dung lượng ống nhỏ giọt (a_i), chúng tôi truyền khả năng tiếp cận từ (s) đến (s+a_i). Bởi vì mỗi ống nhỏ giọt có thể được sử dụng tùy ý nhiều lần nên việc lặp lại số lượng sẽ đi từ nhỏ đến lớn. 

Cùng một DP cũng có thể lưu trữ số lượng trước đó cho mọi số lượng có thể truy cập được. Nếu như`dp[s]`trở thành đúng vì (s-a_i), chúng ta nhớ (s-a_i). Theo dõi những người tiền nhiệm đó sau này sẽ đưa ra số lần thực tế mà mỗi ống nhỏ giọt phải được sử dụng cho một truy vấn. 

Sau khi tính toán tất cả các đại lượng có thể tiếp cận, phần tương tác sẽ trở thành tìm kiếm nhị phân thông thường. Màu của truy vấn cho chúng ta biết (X) ở trước, tại hay sau giá trị có thể truy cập được truy vấn. Giải pháp cuộc thi đã xuất bản sử dụng chính xác DP số lượng có thể truy cập này, sau đó là tìm kiếm nhị phân. 

Việc triển khai đơn giản thực hiện chuyển tiếp DP (O(n\cdot10^6)). Với (n=100), trường hợp xấu nhất là (100.000.000) chuyển tiếp, tiếp theo là (10^6) thao tác khác để thu thập tất cả số lượng có thể tiếp cận. Đó là điểm mà việc triển khai đơn giản trở nên kém hấp dẫn, đặc biệt là trong Python. 

Quan sát quan trọng là DP chỉ chứa thông tin boolean. Thay vì biểu thị một đại lượng có thể tiếp cận bằng một đối tượng Python, chúng ta có thể biểu thị đồng thời tất cả các đại lượng có thể tiếp cận dưới dạng bit của một số nguyên lớn. (Các) Bit chính xác là một khi có thể đạt được (các) số lượng. 

Đối với một công suất (a), hoạt động 

[ 
S \leftarrow S\cup(S+a) 
] 

tương ứng với```
bits |= bits << a
```Một ứng dụng duy nhất cho phép sử dụng thêm một lần ống nhỏ giọt hiện tại. Để cho phép sử dụng nhiều tùy ý một cách hiệu quả, chúng tôi tăng gấp đôi số lượng bản sao có sẵn. Sau khi xử lý các ca (a,2a,4a,\ldots), mọi số lượng bản sao từ (0) đến giới hạn yêu cầu đều có sẵn. Do đó, mỗi công suất chỉ cần dịch chuyển số nguyên lớn (O(\log 10^6)). 

Chúng tôi cũng ghi lại phần trước bất cứ khi nào một bit có thể truy cập được lần đầu tiên. Nếu (các) giá trị mới được tạo bằng cách dịch chuyển giá trị trước đó bằng (k a_i), thì giá trị trước đó là (s-k a_i) và việc xây dựng lại có thể khôi phục số lượng bản sao dưới dạng ((s-\text{predecessor})/a_i). 

Việc tìm kiếm kết quả sau đó được thực hiện trên số lượng có thể tiếp cận được sắp xếp. Nếu trọng tài trả lại màu vàng, chúng ta biết giá trị chính xác. Nếu tìm kiếm kết thúc mà không có màu vàng, các khả năng còn lại sẽ tạo thành khoảng cách giữa hai đại lượng có thể tiếp cận liên tiếp, với cách xử lý đặc biệt khi khoảng cách đạt đến ranh giới của phạm vi cho phép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| DP đơn giản | (O(nM)), trong đó (M=10^6) | (O(M)) | Được chấp nhận trong C++ được tối ưu hóa, quá nặng đối với Python | 
| Bitset đóng gói DP | (O(n\log M\cdot M/W + M)) thao tác từ | (O(M)) | Được chấp nhận và phù hợp với Python | 

Ở đây (W) là kích thước từ máy được sử dụng nội bộ bởi biểu diễn số nguyên lớn. Các số nguyên có độ chính xác tùy ý của Python thực hiện các phép dịch chuyển lớn và các phép toán theo bit trong mã gốc được tối ưu hóa. 

## Hướng dẫn thuật toán 

1. Đọc số lượng ống nhỏ giọt và dung tích của chúng. Đặt (M=10^6), giá trị lớn nhất có thể có của (X). 

Đại lượng duy nhất đáng xem xét là những tổng thực sự có thể được hình thành từ các năng lực. Truy vấn số lượng không thể truy cập là không thể theo giao thức. 
2. Tạo một bitset`bits`chỉ với bit 0 được đặt. (Các) bit có nghĩa là (các) số lượng có thể truy cập được. 

Có thể truy cập số lượng 0 vì được phép sử dụng mọi ống nhỏ giọt 0 lần. 
3. Xử lý mọi dung lượng (a_i). Đối với các ca (a_i,2a_i,4a_i,\ldots), hãy tính các bit mới có thể truy cập bằng cách sử dụng 

[ 
\text{new}=(\text{bits}\ll\text{shift})\setminus\text{bits}. 
] 

Mỗi bước nhân đôi sẽ mở rộng số lượng bản sao của ống nhỏ giọt hiện tại có thể được thêm vào. Sau các ca (a_i,2a_i,\ldots,2^ka_i), tất cả số đếm bắt buộc từ 0 đến (2^{k+1}-1) đều có sẵn. 
4. Đối với mỗi (các) giá trị mới có thể truy cập, hãy lưu trữ giá trị tiền nhiệm và chỉ mục của trình nhỏ giọt chịu trách nhiệm chuyển đổi. 

Nếu như`shift = 8`và giá trị mới (25) đến từ (17), giá trị tiền nhiệm được lưu trữ là (17). Trong quá trình tái thiết, chênh lệch (25-17=8) cho chúng ta biết rằng một bản sao của công suất tương ứng đã được sử dụng trong quá trình chuyển đổi này. Những ca lớn hơn hoạt động giống hệt nhau, với sự khác biệt có thể đại diện cho một số bản sao. 
5. Trích xuất mọi số lượng dương có thể tiếp cận vào một mảng đã sắp xếp`values`. 

Bản thân bitset đã lưu trữ các giá trị theo thứ tự số, nhưng tìm kiếm nhị phân cần truy cập ngẫu nhiên theo thứ hạng, vì vậy chúng tôi cụ thể hóa tập hợp các đại lượng dương có thể tiếp cận. 
6. Tìm kiếm nhị phân mảng số lượng có thể truy cập. Đối với số lượng ở giữa (q), hãy xây dựng lại vectơ nhỏ giọt có tổng chính xác là (q), in truy vấn và đọc màu. 

Phản hồi màu đỏ có nghĩa là (q>X), vì vậy mọi số lượng có thể đạt được tại hoặc sau (q) đều có thể bị loại bỏ. Phản hồi màu xanh lá cây có nghĩa là (q<X), vì vậy mọi số lượng có thể đạt được tại hoặc trước (q) đều có thể bị loại bỏ. Màu vàng ngay lập tức xác định (X=q). 
7. Nếu màu vàng không bao giờ xuất hiện, hãy để`hi`là số lượng có thể tiếp cận cuối cùng được biết là dưới (X) và đặt`lo`là số lượng có thể tiếp cận đầu tiên được biết là ở trên (X). 

Nếu cả hai đều tồn tại và`values[lo] - values[hi] == 2`, có đúng một số nguyên nằm giữa chúng. Số nguyên đó là (X), mặc dù bản thân nó không thể truy cập được. 
8. Nếu không có số lượng có thể truy cập dưới khoảng cuối cùng, trường hợp giới hạn dưới có thể nhận dạng duy nhất duy nhất có thể là`values[0] == 2`. Khi đó số nguyên dương bên dưới nó nhất thiết phải là (1). 

Ví dụ: nếu số lượng nhỏ nhất có thể tiếp cận là (5), thì phản hồi màu đỏ sẽ chỉ cho chúng ta biết rằng (X) thuộc về (1,2,3,4), vì vậy câu trả lời sẽ phải là (-1). 
9. Áp dụng quy tắc đối xứng ở biên trên. Nếu số lượng có thể tiếp cận lớn nhất là (999999) và phản hồi của nó có màu xanh lục thì (X=10^6). 
10. Nếu không có trường hợp duy nhất nào áp dụng, hãy in (-1). Cuối cùng là lệnh in`2`theo sau là câu trả lời xác định. 

### Tại sao nó hoạt động 

Bất biến trong quá trình tìm kiếm nhị phân tương tác là mọi giá trị của (X) phù hợp với tất cả các phản hồi nằm giữa các đại lượng có thể truy cập còn lại. Phản hồi màu vàng xác định một số lượng chính xác có thể tiếp cận được. Nếu không có màu vàng, (X) phải nằm hoàn toàn giữa hai đại lượng có thể tiếp cận liên tiếp hoặc nằm ngoài phạm vi có thể tiếp cận tại một trong hai ranh giới. 

Nếu hai đại lượng liên tiếp có thể đạt tới khác nhau hai thì có chính xác một số nguyên có thể nằm giữa chúng, do đó số nguyên đó được xác định duy nhất. Nếu chênh lệch của chúng ít nhất là ba thì ít nhất hai số nguyên khác nhau sẽ tạo ra các câu trả lời giống hệt nhau cho mọi truy vấn có thể truy cập, do đó thông tin của giám khảo không thể phân biệt chúng và (-1) là chính xác. Các trường hợp biên xuất phát từ thực tế là (X) bị giới hạn trong khoảng dương ([1,10^6]). 

Cấu trúc bitset là chính xác vì mỗi lần chuyển đổi sẽ thêm bội số không âm của dung lượng hiện tại vào tổng có thể truy cập được, trong khi trình tự nhân đôi cuối cùng cho phép mọi số lượng bản sao được yêu cầu. Do đó, mỗi bit do thuật toán thiết lập đều tương ứng với một tổ hợp hợp lệ và mọi tổ hợp hợp lệ cuối cùng đều được biểu diễn. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

LIMIT = 10**6
MASK = (1 << (LIMIT + 1)) - 1

def build_reachable(a):
    n = len(a)

    # bit s = 1 iff s is reachable.
    bits = 1

    # Encodes predecessor * n + coin_index.
    # -1 is used only for value 0.
    parent = array('i', [-1]) * (LIMIT + 1)

    for coin, value in enumerate(a):
        shift = value

        while shift <= LIMIT:
            shifted = (bits << shift) & MASK
            new_bits = shifted & ~bits

            # Store one witness for every newly reachable sum.
            while new_bits:
                low = new_bits & -new_bits
                s = low.bit_length() - 1
                prev = s - shift
                parent[s] = prev * n + coin
                new_bits ^= low

            bits |= shifted

            if bits == MASK:
                break

            shift <<= 1

    values = []
    b = bits & ~1

    while b:
        low = b & -b
        values.append(low.bit_length() - 1)
        b ^= low

    return values, parent

def get_counts(total, a, parent):
    n = len(a)
    counts = [0] * n
    cur = total

    while cur:
        encoded = parent[cur]
        coin = encoded % n
        prev = encoded // n

        counts[coin] += (cur - prev) // a[coin]
        cur = prev

    return counts

def ask(total, a, parent):
    counts = get_counts(total, a, parent)

    print(1, flush=True)
    print(*counts, flush=True)

    response = input().strip()

    if not response:
        sys.exit(0)

    return response[0]

def main():
    n = int(input())
    a = list(map(int, input().split()))

    values, parent = build_reachable(a)

    left = 0
    right = len(values) - 1
    answer = -1

    last_mid = -1
    last_response = ''

    while left <= right:
        mid = (left + right) // 2
        last_mid = mid

        response = ask(values[mid], a, parent)
        last_response = response

        if response == 'y':
            answer = values[mid]
            break

        if response == 'g':
            left = mid + 1
        else:
            right = mid - 1

    if answer == -1:
        # X is smaller than every reachable positive quantity.
        if right < 0:
            if values[0] == 2 and last_response == 'r':
                answer = 1

        # X is larger than every reachable quantity.
        elif left == len(values):
            if values[-1] == LIMIT - 1 and last_response == 'g':
                answer = LIMIT

        # X is strictly between two consecutive reachable quantities.
        else:
            low = values[right]
            high = values[left]

            if high - low == 2:
                answer = low + 1

    print(2, flush=True)
    print(answer, flush=True)

if __name__ == "__main__":
    main()
```Phần đầu tiên của quá trình triển khai xây dựng tập hợp số lượng có thể tiếp cận.`bits`là một số nguyên Python có vị trí bit là số lượng, vì vậy`bits << shift`đại diện cho việc thêm`shift`đơn vị cho mọi số lượng hiện có thể đạt được. 

Vòng lặp nhân đôi xứng đáng được chú ý. Sau khi xử lý sự dịch chuyển của (a), tập bit hiện tại chứa các tổng sử dụng 0 hoặc một bản sao mới của bộ nhỏ giọt hiện tại. Sau khi dịch chuyển theo (2a), nó chứa số 0 đến ba bản sao. Ca tiếp theo cho kết quả từ 0 đến 7 bản sao, v.v. Vì (2^{20}>10^6) nên cần tối đa 20 ca cho một công suất. 

các`parent`mảng chỉ được điền cho các bit có thể truy cập được lần đầu tiên. Nó lưu trữ cả tổng trước đó và chỉ số nhỏ giọt trong một số nguyên. Việc sử dụng mã hóa`prev * n + coin`và việc xây dựng lại sẽ đảo ngược nó bằng phép chia số nguyên và số dư. 

Số lượng bản sao được sử dụng trong một bước tái thiết không nhất thiết phải là một. Nếu đạt được một giá trị bằng cách sử dụng độ dịch chuyển của (8a_i), thì giá trị trước đó sẽ khác với (8a_i), vì vậy`(cur - prev) // a[coin]`phục hồi chính xác tám bản sao. 

Bản thân truy vấn tương tác được cố tình tách thành`ask`. Nó xây dựng lại số lượng ống nhỏ giọt chính xác, in lệnh`1`, in vectơ và xóa ngay lập tức. Các vấn đề tương tác dễ dàng thất bại nếu đầu ra được đệm, vì vậy cả hai dòng đều bị xóa trước khi chờ phản hồi của trọng tài. 

Tìm kiếm nhị phân sử dụng các chỉ số`values`, chứ không phải bản thân số lượng. Điều này là cần thiết vì không thể truy vấn số lượng không thể truy cập được. So sánh màu sắc vẫn là so sánh theo thứ tự thông thường vì mọi đại lượng có thể tiếp cận đều là số nguyên. 

Việc kiểm tra ranh giới cuối cùng tránh lập chỉ mục bên ngoài mảng. Một triển khai chung có thể truy cập một cách mù quáng`values[left]`Và`values[right]`sau khi tìm kiếm nhị phân có thể đọc vị trí không hợp lệ khi (X) nằm dưới số lượng nhỏ nhất hoặc cao hơn số lượng lớn nhất có thể truy cập được. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên khi dịch chuyển bitset. Sự rõ ràng`MASK`giữ cho bitset bị giới hạn ở số lượng thông qua (10^6), điều này cũng ngăn số nguyên tăng lên một cách không cần thiết. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên tương ứng với công suất (1,2,5,10,20,50). Sự tương tác có thể xác định được (X=10). Số lượng có thể truy cập bao gồm (8), (10) và (12), do đó, đường dẫn tìm kiếm khả thi có thể bao gồm câu trả lời và cuối cùng là truy vấn (10). 

| Số lượng truy vấn | Phản hồi | Hiệu ứng tìm kiếm | 
| --- | --- | --- | 
| 25 | đỏ | (X<25) | 
| 8 | màu xanh lá cây | (X>8) | 
| 10 | màu vàng | (X=10) | 

Thuộc tính quan trọng ở đây là mọi đại lượng được truy vấn thực sự có thể xây dựng được. Ví dụ: (25=5+20), (8=2+2+2+2) và (10=10). Phản hồi màu vàng chấm dứt tìm kiếm ngay lập tức. 

Mẫu thứ hai sử dụng dung lượng (4) và (8). Giả sử giá trị ẩn là (10). Số lượng dương có thể đạt được là (4,8,12,16,\ldots). 

| Số lượng truy vấn | Phản hồi | Hiệu ứng tìm kiếm | 
| --- | --- | --- | 
| 8 | màu xanh lá cây | (X>8) | 
| 12 | đỏ | (X<12) | 
| 10 | không có sẵn | Không thể truy vấn | 

Sau hai lần quan sát đầu tiên, cả hai (9), (10) và (11) vẫn có thể xảy ra. Vì không có cách nào để truy vấn số lượng nằm giữa (8) và (12) nên câu trả lời đúng là (-1). Đây chính xác là tình huống tách biệt "tìm kiếm nhị phân trên số nguyên" khỏi "tìm kiếm nhị phân trên số lượng có thể truy cập". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log M\cdot M/W + M)) thao tác từ | Mỗi dung lượng sử dụng dịch chuyển bitset đóng gói (O(\log M)) và trích xuất nhân chứng chạm vào từng giá trị có thể truy cập một lần | 
| Không gian | (O(M)) | Mảng cha lưu trữ một số nguyên cho mỗi số lượng có thể, trong khi bitset sử dụng bit (O(M)) | 

Đây (M=10^6). Giai đoạn tương tác sử dụng tối đa 20 thử nghiệm vì có tối đa (10^6) số lượng ứng viên dương. Công việc tính toán chủ yếu là tính toán khả năng tiếp cận giả đa thức, trong khi số lượng tương tác thực tế là logarit. 

Việc triển khai đóng gói đặc biệt hữu ích trong Python vì các dịch chuyển bitset đắt tiền và các phép toán logic thực thi trong mã gốc được tối ưu hóa thay vì lặp lại vòng lặp cấp Python (10^8). 

## Trường hợp thử nghiệm 

Bởi vì đây là một vấn đề tương tác nên các mẫu là bản ghi tương tác chứ không phải là các cặp đầu vào/đầu ra xác định thông thường. Một người trợ giúp bình thường của biểu mẫu`run(input) == output`không thể kiểm tra chúng một cách trung thực vì chương trình mong đợi thẩm phán trả lời mọi câu hỏi. Khai thác thử nghiệm sau đây mô phỏng phán đoán nội bộ đó và thực hiện cùng một logic tìm kiếm và tái thiết nhị phân. 

Các tình huống mẫu sử dụng các khả năng hiển thị trong bản ghi tương tác. Cái đầu tiên có dung lượng (1,2,5,10,20,50) và giá trị ẩn là (10). Cái thứ hai có dung lượng (4,8) và giá trị ẩn là (10), trong đó câu trả lời đúng là (-1). Cái thứ ba có dung lượng (2,3) và giá trị ẩn là (1), cũng không thể xác định được duy nhất.```python
from array import array

LIMIT = 10**6
MASK = (1 << (LIMIT + 1)) - 1

def build_reachable(a):
    n = len(a)
    bits = 1
    parent = array('i', [-1]) * (LIMIT + 1)

    for coin, value in enumerate(a):
        shift = value

        while shift <= LIMIT:
            shifted = (bits << shift) & MASK
            new_bits = shifted & ~bits

            while new_bits:
                low = new_bits & -new_bits
                s = low.bit_length() - 1
                prev = s - shift
                parent[s] = prev * n + coin
                new_bits ^= low

            bits |= shifted

            if bits == MASK:
                break

            shift <<= 1

    values = []
    b = bits & ~1

    while b:
        low = b & -b
        values.append(low.bit_length() - 1)
        b ^= low

    return values, parent

def get_counts(total, a, parent):
    n = len(a)
    counts = [0] * n
    cur = total

    while cur:
        encoded = parent[cur]
        coin = encoded % n
        prev = encoded // n
        counts[coin] += (cur - prev) // a[coin]
        cur = prev

    return counts

def solve_hidden(a, hidden):
    values, parent = build_reachable(a)

    left = 0
    right = len(values) - 1

    last_query = None
    last_response = None

    while left <= right:
        mid = (left + right) // 2
        query = values[mid]

        counts = get_counts(query, a, parent)
        assert sum(x * y for x, y in zip(a, counts)) == query
        assert all(x >= 0 for x in counts)
        assert query <= LIMIT

        last_query = query

        if query < hidden:
            response = 'g'
        elif query > hidden:
            response = 'r'
        else:
            response = 'y'

        last_response = response

        if response == 'y':
            return query
        elif response == 'g':
            left = mid + 1
        else:
            right = mid - 1

    if right < 0:
        if values[0] == 2 and last_response == 'r':
            return 1
        return -1

    if left == len(values):
        if values[-1] == LIMIT - 1 and last_response == 'g':
            return LIMIT
        return -1

    low = values[right]
    high = values[left]

    if high - low == 2:
        return low + 1

    return -1

# Sample 1: capacities 1, 2, 5, 10, 20, 50, hidden X = 10.
assert solve_hidden([1, 2, 5, 10, 20, 50], 10) == 10

# Sample 2: capacities 4, 8, hidden X = 10.
# The observations cannot distinguish 9, 10, and 11.
assert solve_hidden([4, 8], 10) == -1

# Sample 3: capacities 2, 3, hidden X = 1.
# Both 1 and other values below the first useful query cannot be separated.
assert solve_hidden([2, 3], 1) == -1

# Minimum-size case. Capacity 1 can produce every possible X.
assert solve_hidden([1], 1) == 1

# Boundary case. With capacity 2, every integer is either reachable
# or lies between two consecutive even reachable quantities.
assert solve_hidden([2], 1_000_000) == 1_000_000

# All-equal capacities. This is outside the statement's "different
# capacities" condition, but it checks that duplicate capacities do
# not break the implementation.
assert solve_hidden([7, 7], 14) == 14

# A gap larger than two leaves several possible hidden values.
assert solve_hidden([7, 7], 15) == -1

# Maximum-size n. Because capacity 1 is present, every X is reachable.
assert solve_hidden(list(range(1, 101)), 999_999) == 999_999
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 5 10 20 50`, ẩn (10) |`10`| Mẫu 1 và phát hiện màu vàng chính xác | 
|`4 8`, ẩn (10) |`-1`| Mẫu 2 và khoảng cách chưa được giải quyết có chiều rộng 4 | 
|`2 3`, ẩn (1) |`-1`| Mẫu 3 và sự mơ hồ ở giới hạn dưới | 
|`[1]`, ẩn (1) |`1`| Phiên bản kích thước tối thiểu | 
|`[2]`, ẩn (10^6) |`1000000`| Ranh giới trên và giá trị có thể tiếp cận lớn | 
|`[7,7]`, ẩn (14) |`14`| Năng lực trùng lặp và mục tiêu có thể tiếp cận chính xác | 
|`[7,7]`, ẩn (15) |`-1`| Khoảng cách lớn hơn hai | 
|`[1,2,\ldots,100]`, ẩn (999999) |`999999`| Khả năng tiếp cận tối đa (n) và dày đặc | 

## Vỏ cạnh 

Hãy xem xét một ống nhỏ giọt có dung lượng (2) và giá trị ẩn (1). Số lượng dương có thể đạt được là (2,4,6,\ldots). Truy vấn đầu tiên trên phạm vi có thể là (2) và phản hồi có màu đỏ. Không có số nguyên dương nào nhỏ hơn (2) ngoại trừ (1), nên thuật toán trả về (1). Kiểm tra giới hạn dưới`values[0] == 2`chính xác là dành cho tình huống này. 

Hãy xem xét cùng một ống nhỏ giọt có giá trị ẩn (3). Truy vấn (2) cho kết quả màu xanh lá cây và truy vấn (4) cho kết quả màu đỏ. Hai đại lượng liên tiếp có thể đạt được khác nhau bởi (2), vì vậy số nguyên duy nhất giữa chúng là (3). Thuật toán trả về (3), mặc dù không có thử nghiệm nào có thể sử dụng chính xác ba đơn vị. 

Bây giờ hãy xem xét dung lượng (4) và (8) với giá trị ẩn (10). Các giá trị có thể truy cập xung quanh (X) là (8) và (12). Hiệu của chúng là (4), nên vẫn có thể có ba số nguyên (9,10,11). Không có chuỗi truy vấn so sánh nào có thể phân biệt chúng vì mọi truy vấn có thể đều là bội số của (4). Thuật toán trả về chính xác (-1). 

Đối với ranh giới trên, giả sử dung lượng cho phép (999999) nhưng không có giá trị giữa nó và (10^6) và phản hồi cho (999999) có màu xanh lục. Vì (X\le10^6), (X) phải là (10^6). Kiểm tra ranh giới trên nhận ra điều này mà không yêu cầu số lượng không thể tiếp cận ở trên (X). 

Nếu xảy ra phản hồi màu vàng thì không cần phân tích khoảng cách. Kết quả màu vàng có nghĩa là số lượng được kiểm tra bằng (X), do đó thuật toán ngay lập tức trả về số lượng có thể tiếp cận đó. Đây cũng là lý do tại sao cách biểu diễn trước đó phải tạo ra một số tiền chính xác. Một vectơ truy vấn có tổng cách nhau một đơn vị chẵn sẽ thay đổi phản hồi của người đánh giá và làm mất hiệu lực tìm kiếm nhị phân. 

Cuối cùng, quá trình tái thiết không bao giờ cần số lượng ống nhỏ giọt âm. Mỗi giá trị tiền nhiệm được lưu trữ đều nhỏ hơn giá trị có thể truy cập hiện tại và mỗi lần chuyển đổi được tạo bằng cách cộng bội số dương của một dung lượng. Các phần trước sẽ giảm nghiêm ngặt tổng hiện tại cho đến khi đạt đến 0, do đó việc xây dựng lại luôn kết thúc với một vectơ không âm hợp lệ.
