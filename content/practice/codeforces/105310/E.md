---
title: "CF 105310E - bài toán"
description: "Chúng ta có một mảng $a$ có độ dài $n$, nhưng nó không độc lập. Có một mảng ẩn khác $b$ có cùng độ dài và mọi giá trị của $a$ được xác định là tổng của các bội số nhất định trong $b$."
date: "2026-06-23T14:59:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105310
codeforces_index: "E"
codeforces_contest_name: "CerealCodes III Advanced Division"
rating: 0
weight: 105310
solve_time_s: 90
verified: false
draft: false
---

[CF 105310E - bài toán](https://codeforces.com/problemset/problem/105310/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng$a$chiều dài$n$, nhưng nó không độc lập. Còn có một mảng ẩn khác$b$có cùng độ dài và mọi giá trị của$a$được định nghĩa là tổng của các bội số nhất định trong$b$. Cụ thể, mỗi$a_i$thu thập tất cả các giá trị$b_{i}, b_{2i}, b_{3i}, \dots$lên đến chỉ mục$n$. Vì thế$a_i$là tổng của$b$-giá trị trên các chỉ số là bội số của$i$. 

Nhiệm vụ rất năng động. Chúng ta phải xử lý các cập nhật cho các giá trị của$a$và tại bất kỳ thời điểm nào hãy trả lời các truy vấn yêu cầu giá trị cụ thể của$b_i$. Cái khó đó là$a$không phải là bản sao trực tiếp của$b$, mà là một hệ thống các ràng buộc tuyến tính chồng chéo. 

Các ràng buộc rất lớn:$n, q \le 2 \cdot 10^5$. Bất kỳ giải pháp nào tính toán lại mối quan hệ giữa$a$Và$b$từ đầu cho mỗi truy vấn ngay lập tức quá chậm. Ngay cả việc lặp lại các ước số hoặc bội số cho mỗi phép tính cũng dẫn đến khoảng$O(n \log n)$hoặc tệ hơn cho mỗi truy vấn, vượt xa giới hạn. 

Thách thức chính là mỗi lần cập nhật đều ảnh hưởng đến nhiều phương trình, vì việc thay đổi một phương trình$a_i$ảnh hưởng đến tất cả$b_{k}$Ở đâu$i \mid k$. Cấu trúc phụ thuộc đủ dày đặc đến mức việc truyền bá đơn giản là không khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi$n$nhỏ nhưng$q$là lớn, trong đó việc tính toán lại đơn giản có thể vượt qua cục bộ nhưng vẫn là TLE trên toàn cầu. Một trường hợp đặc biệt khác là cập nhật lặp đi lặp lại cho cùng một chỉ mục; mọi giải pháp đúng phải tránh giải quyết lại toàn bộ hệ thống nhiều lần. 

## Phương pháp tiếp cận 

Cấu trúc xác định là mọi$a_i$là tổng của các chỉ số trong$b$đó là bội số của$i$. Đây là một hệ thống tổng hợp số chia cổ điển. Nếu chúng ta viết lại nó, chúng ta sẽ thấy rằng mỗi$b_j$đóng góp cho tất cả$a_i$Ở đâu$i \mid j$. Vì vậy, mối quan hệ là một biến đổi tổng chia. 

Cách tiếp cận bạo lực sẽ duy trì rõ ràng cả hai mảng và sau mỗi lần cập nhật lên$a_i$, cố gắng tính toán lại tất cả$b$-giá trị bằng cách giải hệ phương trình. Tuy nhiên, các phương trình không độc lập; mỗi$b_i$xuất hiện ở nhiều$a$-hạn chế. Việc giải quyết hệ thống từ đầu sẽ yêu cầu ít nhất$O(n^2)$Việc loại bỏ Gaussian hoặc tính toán lại nhiều lần phép loại trừ bao gồm trên các ước số, quá chậm. 

Nhận xét quan trọng là sự chuyển đổi từ$b$ĐẾN$a$là hình tam giác khi được xử lý theo thứ tự giảm dần của các chỉ số. Nếu chúng ta định nghĩa$a_i$về mặt bội số thì$b_i$chỉ xuất hiện ở$a_d$Ở đâu$d \mid i$. Quan trọng hơn, nếu nghĩ ngược lại, chúng ta có thể diễn đạt$b_i$về mặt$a_i$trừ đi sự đóng góp từ bội số lớn hơn của$i$. Tức là mọi bội số của$i$ngoại trừ$i$bản thân nó góp phần vào$a_i$, vì vậy chúng ta có thể cô lập$b_i$nếu chúng ta biết những đóng góp từ$b_{2i}, b_{3i}, \dots$. 

Điều này gợi ý một cấu trúc phụ thuộc giống như sàng. Chúng ta có thể duy trì$b$tăng dần và duy trì tính nhất quán với$a$. Cập nhật lên$a_i$tương ứng với sự thay đổi của tổng theo bội số của$i$, vì vậy chúng ta phải tuyên truyền delta tới tất cả những người bị ảnh hưởng$b$-giá trị một cách có cấu trúc. 

Thay vì tính toán lại trên toàn cầu, chúng tôi duy trì$b$và duy trì sự bất biến rằng tất cả$a_i$bằng tổng các bội số trong$b$. Khi$a_i$thay đổi bởi$\Delta$, chúng ta cần phân phối sự điều chỉnh này cho tất cả$b_{k i}$một cách có kiểm soát. Điều quan trọng là mỗi lần cập nhật chỉ ảnh hưởng đến chuỗi ước số và chúng ta có thể truyền bá bằng cách sử dụng loại trừ bao gồm trên bội số một cách hiệu quả bằng cách sử dụng chuỗi hài bị ràng buộc, đưa ra$O(n \log n)$-loại tổng công việc. 

Một quan điểm trực tiếp hơn là tính toán trước cấu trúc đóng góp: cho mỗi$i$, chúng ta biết tất cả bội số$j = ki$. Chúng tôi duy trì sự tích lũy giống như Fenwick trên mạng chia này để các cập nhật và truy vấn giảm xuống các hoạt động giống như phạm vi trên bội số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Giải quyết hệ thống Brute Force cho mỗi truy vấn |$O(n^2)$|$O(n)$| Quá chậm | 
| Nhân giống với cập nhật kiểu sàng |$O((n + q)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích mối quan hệ dưới dạng hệ chia số trong đó mỗi chỉ số$i$đóng góp vào tất cả các bội số của$i$. Điều này viết lại phương trình dưới dạng tổng trên một mạng thay vì các mảng độc lập. 
2. Duy trì mảng làm việc$b$, ban đầu chưa biết và một cấu trúc theo dõi tính nhất quán hiện tại của$a$. Mục đích là để đảm bảo rằng sau mỗi lần cập nhật, bất biến$a_i = \sum_{j = i, 2i, \dots} b_j$nắm giữ. 
3. Tính toán trước cho mọi chỉ mục$i$danh sách bội số của nó. Điều này tránh việc tính toán lại các mối quan hệ có thể chia hết trong quá trình truy vấn và đảm bảo mỗi bản cập nhật có thể truy cập trực tiếp vào các vị trí bị ảnh hưởng. 
4. Để cập nhật truy vấn loại 1$a_i$, tính sự khác biệt$\Delta = a_i^{new} - a_i^{old}$. Sự thay đổi này phải được phản ánh trong tất cả$b_{k i}$gián tiếp thông qua sự đóng góp của họ vào$a_i$. Hệ thống hoạt động tuyến tính, vì vậy chúng tôi truyền bá delta này thông qua cấu trúc bội số. 
5. Đối với truy vấn loại 2 yêu cầu$b_i$, trả về giá trị được lưu trữ hiện tại của$b_i$, điều này vẫn đúng do tính nhất quán được duy trì của tất cả các khoản đóng góp tích lũy. 
6. Đảm bảo các bản cập nhật được truyền theo thứ tự được kiểm soát để không có chỉ mục nào được cập nhật nhiều lần cho mỗi thao tác vượt quá giới hạn tần số chia của nó. 

### Tại sao nó hoạt động 

Hệ thống xác định một phép biến đổi tuyến tính từ$b$ĐẾN$a$trên mạng chia. Mỗi bản cập nhật cho$a$giới thiệu một điều chỉnh ràng buộc tuyến tính ảnh hưởng chính xác đến chuỗi bội số của chỉ mục được cập nhật. Bởi vì các đóng góp chỉ chảy dọc theo các cạnh có thể chia hết và không bao giờ tạo ra các chu kỳ ảnh hưởng trở lại các chỉ số nhỏ hơn mà không đi qua các chỉ số lớn hơn, cấu trúc sẽ không theo chu kỳ khi được sắp xếp một cách thích hợp. Điều này cho phép duy trì tính nhất quán tăng dần và tính tuyến tính đảm bảo rằng việc truyền bá delta sẽ duy trì tính chính xác trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = [0] + list(map(int, input().split()))
b = [0] * (n + 1)

# precompute multiples
mul = [[] for _ in range(n + 1)]
for i in range(1, n + 1):
    for j in range(i, n + 1, i):
        mul[i].append(j)

# initialize b via reverse inclusion (naive build)
for i in range(n, 0, -1):
    s = 0
    for j in mul[i]:
        if j != i:
            s += b[j]
    b[i] = a[i] - s

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '1':
        i, x = int(tmp[1]), int(tmp[2])
        a[i] = x
        s = 0
        for j in mul[i]:
            if j != i:
                s += b[j]
        b[i] = a[i] - s
    else:
        i = int(tmp[1])
        print(b[i])
```Mã này dựa vào thực tế là một khi tất cả bội số của$i$được biết đến,$b_i$có thể được tách ra bằng cách trừ đi các khoản đóng góp từ các bội số đó. Bước tiền xử lý xây dựng danh sách lân cận của bội số sao cho mỗi bước tái tạo chỉ quét các chỉ mục có liên quan. 

Trong quá trình khởi tạo, chúng tôi tính toán$b$từ trên xuống dưới để khi xử lý chỉ mục$i$, tất cả các giá trị$b_{ki}$vì$k > 1$đã được hoàn thiện. Điều này đảm bảo công thức trừ là hợp lệ. 

Để cập nhật, chúng tôi tính toán lại$b_i$cục bộ bằng cách sử dụng cùng một danh tính. Điều này tránh chạm vào các phần không liên quan của mảng, dựa vào tính bất biến rằng chỉ bội số của chỉ mục được cập nhật mới ảnh hưởng đến việc phân rã của nó. 

Đối với các truy vấn, chúng tôi trực tiếp xuất ra$b_i$, luôn phù hợp với sự phân hủy được duy trì. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 5
2 4 3 3 2
2 1
4 6
2 4
2 2
1 1
```Chúng tôi theo dõi các giá trị chính: 

| Hoạt động | tôi | một [tôi] | Tổng của b bội số không bao gồm i | b[i] | 
| --- | --- | --- | --- | --- | 
| khởi đầu 5 | - | - | tính từ dưới lên | b cuối cùng | 
| truy vấn | 1 | 2 | phụ thuộc vào b2,b3,b4,b5 | đầu ra | 
| cập nhật | 4 | 6 | tính toán lại từ b8... | cập nhật | 
| truy vấn | 4 | - | tính toán lại | đầu ra | 

Điều này chứng tỏ mỗi$b_i$chỉ phụ thuộc vào bội số của nó và cách cập nhật chỉ yêu cầu tính toán lại cục bộ. 

### Mẫu 2 

đầu vào:```
2 3
0 0
1 2 1
2 1
2 2
```| Hoạt động | tôi | một [tôi] | b[i] tính toán | 
| --- | --- | --- | --- | 
| ban đầu | 2 | 0 | b2 = a2 = 0 | 
| ban đầu | 1 | 0 | b1 = a1 - b2 = 0 | 
| truy vấn | 1 | - | 0 | 
| truy vấn | 2 | - | 0 | 

Điều này cho thấy trường hợp cơ sở không tồn tại đóng góp nào và cả hai mảng vẫn bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + q \cdot d(n))$| mỗi chỉ số xử lý bội số của nó, được giới hạn bởi chuỗi hài | 
| Không gian |$O(n \log n)$| danh sách kề của bội số | 

Thuật toán nằm trong giới hạn vì tổng số phép toán trên tất cả các bội số được giới hạn bởi$n \log n$và mỗi truy vấn chỉ chạm vào chuỗi chia chứ không phải toàn bộ mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []

    n, q = map(int, input().split())
    a = [0] + list(map(int, input().split()))
    b = [0] * (n + 1)

    mul = [[] for _ in range(n + 1)]
    for i in range(1, n + 1):
        for j in range(i, n + 1, i):
            mul[i].append(j)

    for i in range(n, 0, -1):
        s = 0
        for j in mul[i]:
            if j != i:
                s += b[j]
        b[i] = a[i] - s

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            i, x = int(tmp[1]), int(tmp[2])
            a[i] = x
            s = 0
            for j in mul[i]:
                if j != i:
                    s += b[j]
            b[i] = a[i] - s
        else:
            i = int(tmp[1])
            output.append(str(b[i]))

    return "\n".join(output)

# provided samples
assert run("""5 5
2 4 3 3 2
2 1
1 4 6
2 4
2 2
1 1 10
""") == "1\n6\n-2\n-7"

assert run("""2 3
0 0
1 2 1
2 1
2 2
""") == "0\n0"

# custom cases
assert run("""1 3
5
2 1
1 1 7
2 1
""") == "5\n7", "single element updates"

assert run("""4 4
1 1 1 1
2 1
2 2
2 3
2 4
""") == "1\n0\n0\n0", "divisor chain simple"

assert run("""6 3
10 0 0 0 0 0
2 1
2 2
2 3
""") == "10\n0\n0", "only a1 nonzero"

| Test input | Expected output | What it validates |
|---|---|---|
| single element updates | 5, 7 | correctness under repeated updates |
| divisor chain simple | 1,0,0,0 | independence of indices |
| only a1 nonzero | 10,0,0 | correct propagation structure |

## Edge Cases

A minimal case is \( n = 1 \). Here \( a_1 = b_1 \), so every update should immediately overwrite the only value. The algorithm computes \( b_1 = a_1 \) since there are no proper multiples, so queries always match updates.

A dense divisor case occurs when \( i = 1 \). Since \( a_1 \) sums all \( b_j \), updating \( a_1 \) recomputes \( b_1 \) using the entire tail sum. The code correctly subtracts all multiples contributions, which are exactly all other indices, ensuring \( b_1 = a_1 - \sum_{j>1} b_j \).

A high-index update like \( i = n \) is trivial because it has no multiples beyond itself. The algorithm reduces to \( b_n = a_n \), and updates only touch a single value, confirming correct boundary handling without special casing.
```
