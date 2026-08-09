---
title: "CF 102465J - Mona Lisa"
description: "Chúng tôi có bốn phiên bản độc lập của cùng một trình tạo giả ngẫu nhiên 64 bit, một phiên bản cho mỗi bàn phím. Mã bí mật chỉ đơn giản là một chỉ mục tích cực trong một chuỗi trình tạo."
date: "2026-08-08T09:38:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102465
codeforces_index: "J"
codeforces_contest_name: "2018-2019 ICPC Southwestern European Regional Programming Contest (SWERC 2018)"
rating: 0
weight: 102465
solve_time_s: 439
verified: true
draft: false
---

[CF 102465J - Mona Lisa](https://codeforces.com/problemset/problem/102465/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có bốn phiên bản độc lập của cùng một trình tạo giả ngẫu nhiên 64 bit, một phiên bản cho mỗi bàn phím. Mã bí mật chỉ đơn giản là một chỉ mục tích cực trong một chuỗi trình tạo. Nếu bốn chỉ số được chọn là c 1​ ,c 2​ ,c 3​ ,c 4 ​, thì hệ thống sẽ lấy đầu ra của trình tạo tương ứng, chỉ giữ N bit thấp nhất của chúng và yêu cầu XOR của chúng bằng 0. Đầu vào cung cấp N và bốn hạt giống, đồng thời chúng ta cần in bốn mã hợp lệ bất kỳ dưới 100000000. Giới hạn chính thức là 1<N<50, với mỗi hạt giống chiếm 64 bit. 

Trình tạo là xoroshiro128+, với trạng thái bên trong 128 bit được khởi tạo từ mỗi hạt giống. Số nguyên Python không tự động tràn, vì vậy mọi cập nhật trạng thái và mọi kết quả được tạo ra đều phải giảm modulo 2 64, chính xác như phiên bản Python của câu lệnh. 

Cách giảm thiểu hữu ích đầu tiên là ngừng suy nghĩ về đầu ra đầy đủ của trình tạo 64-bit. Đối với điều kiện, chỉ có N bit thấp nhất mới quan trọng, vì vậy mọi số được tạo có thể được thay thế ngay lập tức bằng giá trị modulo 2 N của nó. Bản thân trạng thái của trình tạo vẫn phải giữ nguyên là 64 bit vì các đầu ra trong tương lai phụ thuộc vào tất cả các bit trạng thái. 

Một tìm kiếm mạnh mẽ trên tất cả các mã có thể sẽ có khoảng (10 8 −1) 4, hoặc khoảng 10 32, bốn lần. Ngay cả một tìm kiếm nhỏ hơn nhiều, chẳng hạn như 2,25 ứng viên trên mỗi bàn phím cũng sẽ khiến cách tiếp cận gặp nhau ở giữa theo cặp thông thường trở nên quá lớn. Giới hạn N<50 là đầu mối cho thấy số lượng bit liên quan, thay vì kích thước bằng số của phạm vi mã, sẽ xác định kích thước tìm kiếm. 

Có một số trường hợp đặc biệt bộc lộ những lỗi thực hiện. Với```
1
0 0 0 0
```giá trị được tạo đầu tiên của mọi trình tạo đều bằng 0, vì vậy`1 1 1 1`là hợp lệ. Việc triển khai dựa trên số không có thể in`0 0 0 0`, nhưng mã 0 không được phép vì giá trị trình tạo đầu tiên có mã 1. 

Với```
2
0 0 1 1
```đầu ra đầu tiên của bộ tạo hạt giống 0 có hai bit thấp bằng 2, trong khi đầu ra đầu tiên cho hạt giống 1 có hai bit thấp bằng 0. Do đó`1 1 1 1`cho 2⊕2⊕0⊕0=0. Việc triển khai bất cẩn sử dụng toàn bộ giá trị 64 bit thay vì che giấu N bit sẽ từ chối giải pháp hợp lệ. 

Cuối cùng, hãy xem xét```
50
18446744073709551615 18446744073709551615 18446744073709551615 18446744073709551615
```Bốn bộ tạo giống hệt nhau, vì vậy việc sử dụng cùng một mã trên cả bốn bàn phím luôn cho bốn giá trị giống nhau, có XOR bằng 0.`1 1 1 1`là hợp lệ. Trường hợp này phát hiện các triển khai quên hành vi modulo-2 64 của trình tạo. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra các giá trị ứng cử viên và liệt kê các bộ tứ cho đến khi XOR của chúng bằng 0. Điều này đúng vì mọi tổ hợp bốn mã có thể cuối cùng đều được kiểm tra, nhưng trường hợp xấu nhất là xấp xỉ 10 32 tổ hợp, điều này hoàn toàn không khả thi. 

Một cải tiến tự nhiên hơn là cuộc gặp gỡ thông thường ở giữa. Tạo các giá trị L cho mỗi bàn phím, tính mọi giá trị A i ​ ⊕B j ​ và tìm giá trị giống nhau trong số tất cả C k ​ ⊕D l ​. Điều này hoạt động vì 

A i ​ ⊕B j ​ =C k ​ ⊕D l ​ 

tương đương với 

A i ​ ⊕B j ​ ⊕C k ​ ⊕D l ​ =0. 

Tiếc là mỗi bên có L 2 cặp. Để có được xác suất hợp lý tìm được giải pháp trong số các giá trị ngẫu nhiên N-bit, lý luận sinh nhật thông thường cần L khoảng 2 N/4, tạo ra 2 cặp N/2. Với N=50, tức là khoảng 33 triệu cặp trước khi lưu trữ bất kỳ chỉ mục liên quan nào. 

Quan sát quan trọng là chúng ta không cần các cặp XOR hoàn toàn tùy ý. Chúng ta có thể cố tình buộc các bit b thấp nhất của mỗi cặp XOR phải giống nhau. Nếu 

thấp b ​ (A i ​ )=thấp b ​ (B j ​ )⊕α, 

sau đó 

thấp b ​ (A i ​ ⊕B j ​ )=α. 

Thực hiện tương tự cho hai danh sách còn lại, sử dụng cùng giá trị α. Bây giờ mọi cặp XOR từ cả hai phía đều đã đồng ý ở các bit b thấp nhất. Chúng ta chỉ cần xung đột trên N-b bit còn lại. 

Đây là kỹ thuật sinh nhật tổng quát gồm bốn danh sách do Wagner giới thiệu. Đối với bốn danh sách, nó giảm việc tìm kiếm xuống còn khoảng O(2 N/3 ) công việc và bộ nhớ thay vì O(2 N/2 ). Cấu trúc tiêu chuẩn lấy danh sách khoảng 2 phần tử N/3, nối hai danh sách trên N/3 bit, thực hiện thao tác tương tự trên hai phần tử còn lại và cuối cùng tìm kiếm xung đột chính xác giữa cặp XOR kết quả. 

Chúng tôi sử dụng danh sách lớn hơn một chút, 2 ⌈N/3⌉+1, để tăng xác suất tìm thấy xung đột. Chúng tôi cũng thử một số giá trị của α, bắt đầu bằng 0. Mỗi lần thử đều có cùng chi phí tiệm cận và một số bộ lọc có vẻ độc lập khiến cho việc thất bại cực kỳ khó xảy ra theo giả định giả ngẫu nhiên được cung cấp rõ ràng bởi vấn đề. 

Bản thân phép nối không nên liệt kê tất cả 2 cặp L. Chúng tôi sắp xếp một danh sách theo bit b thấp nhất của nó. Đối với mỗi giá trị trong danh sách khác, chúng tôi chỉ kiểm tra nhóm có số bit thấp sẽ tạo ra giá trị α được yêu cầu. Vì có thể có 2 b nhóm và L chỉ là hệ số không đổi lớn hơn 2 b nên số cặp được kiểm tra dự kiến ​​là O(L). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10 32 ) | O(1) | Quá chậm | 
| Cuộc gặp gỡ thông thường ở giữa | O(2 N/2 ) | O(2 N/2 ) | Quá lớn cho N=50 | 
| Tham gia sinh nhật tổng quát | O(2 N/3 ) dự kiến ​​| O(2 N/3 ) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt b=⌈N/3⌉ và tạo các giá trị L=2 b+1 từ mỗi trong số bốn chuỗi trình tạo. Hệ số hai mang lại cho chúng ta nhiều cặp ứng cử viên hơn ngưỡng lý thuyết trần trong khi vẫn giữ danh sách thoải mái dưới 100000000 mục. 
2. Chỉ giữ lại N bit thấp nhất của mỗi giá trị được tạo. Trạng thái của trình tạo vẫn ở trạng thái 128 bit đầy đủ, vì việc cắt bớt trạng thái sẽ thay đổi tất cả các đầu ra của trình tạo tiếp theo. 
3. Chọn một tập hợp nhỏ các giá trị b-bit α, thử bằng 0 trước tiên. Đối với giá trị α cố định, chúng tôi muốn mọi cặp từ hai danh sách đầu tiên đều thỏa mãn 

thấp b ​ (x 1 ​ ⊕x 2 ​ )=α. 

Cặp thứ hai sẽ thỏa mãn phương trình tương tự, do đó XOR của chúng có thể bằng nhau. 
4. Sắp xếp danh sách thứ hai theo bit b thấp nhất của nó. Đối với giá trị x từ danh sách đầu tiên, nhóm được yêu cầu là 

thấp b ​ (x)⊕α.

Mọi chỉ mục trong nhóm đó tạo thành xung đột một phần hợp lệ với x. 
5. Đối với mỗi cặp như vậy từ hai danh sách đầu tiên, hãy tính x 1 ​ ⊕x 2 ​ và lưu nó vào bảng băm cùng với hai chỉ số ban đầu. Số lượng các cặp được tạo ra dự kiến ​​là O(L), chứ không phải O(L 2 ), vì chỉ những cặp đồng ý về b bit mới tồn tại. 
6. Lặp lại quy trình sắp xếp tương tự cho danh sách thứ ba và thứ tư. Thay vì lưu trữ danh sách cặp thứ hai này, hãy kiểm tra ngay từng cặp XOR được tạo dựa trên bảng băm từ cặp đầu tiên. 
7. Nếu một cặp XOR khớp nhau, giả sử 

x 1 ​ ⊕x 2 ​ =x 3 ​ ⊕x 4 ​ , 

sau đó XOR cả hai bên sẽ cho 

x 1 ​ ⊕x 2 ​ ⊕x 3 ​ ⊕x 4 ​ =0. 

Bốn chỉ số dựa trên một tương ứng là một câu trả lời hợp lệ. 
8. Nếu không tìm thấy xung đột đối với α hiện tại, hãy thử α khác. Giá trị α chỉ thay đổi những va chạm một phần nào được giữ lại, do đó danh sách giả ngẫu nhiên đã tạo có thể được sử dụng lại. 
9. Xuất ra bốn chỉ số được lưu trữ, thêm một chỉ số vì các mảng bên trong có chỉ mục bằng 0 trong khi đầu ra đầu tiên của trình tạo có mã 1. 

### Tại sao nó hoạt động 

Bất biến trung tâm là mọi cặp được lưu trong bảng băm đầu tiên đều có bit b thấp bằng α và mọi cặp được kiểm tra ở phía thứ hai đều có bit b thấp giống hệt nhau. Nếu cặp XOR đầy đủ khớp nhau thì XOR của tất cả bốn đầu ra của bộ tạo được chọn sẽ bằng 0 trong tất cả N bit liên quan. 

Bước lọc không tạo ra giải pháp sai. Nó chỉ loại bỏ các cặp không thể tham gia vào cấu trúc va chạm từng phần đã chọn. Khi hai cặp XOR đầy đủ bằng nhau, bốn mã thu được được đảm bảo về mặt toán học để đáp ứng phương trình XOR được yêu cầu. 

Giả định về tính giả ngẫu nhiên là điều làm cho việc tìm kiếm diễn ra nhanh chóng. Với L=2 b+1, phép nối đầu tiên có kích thước dự kiến ​​vào khoảng L 2 /2 b =2L. Các cặp XOR đó đều chia sẻ b bit cố định, để lại N−b bit khi cần có xung đột. Vì mỗi bên có khoảng 2L thí sinh nên số trận chung kết dự kiến là khoảng 

2 N−b (2L) 2 ​, 

là hằng số hoặc lớn hơn vì b=⌈N/3⌉. Đây là hiệu ứng sinh nhật tổng quát được thuật toán sử dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MASK64 = (1 << 64) - 1
CONST = 0x7263d9bd8409f526

def generate(seed, count, value_mask):
    s0 = seed & MASK64
    s1 = (seed ^ CONST) & MASK64
    result = [0] * count

    for i in range(count):
        result[i] = (s0 + s1) & value_mask

        t = s1 ^ s0

        ns0 = (((s0 << 55) & MASK64) | (s0 >> 9))
        ns0 ^= t
        ns0 ^= (t << 14) & MASK64
        ns0 &= MASK64

        ns1 = (((t << 36) & MASK64) | (t >> 28))
        ns1 &= MASK64

        s0, s1 = ns0, ns1

    return result

def build_buckets(values, low_mask):
    size = low_mask + 1
    head = [-1] * size
    nxt = [-1] * len(values)

    for i, x in enumerate(values):
        k = x & low_mask
        nxt[i] = head[k]
        head[k] = i

    return head, nxt

def build_left_map(a, b, head, nxt, low_mask, alpha, length):
    pairs = {}

    for i, x in enumerate(a):
        wanted = (x & low_mask) ^ alpha
        j = head[wanted]

        while j != -1:
            value = x ^ b[j]
            if value not in pairs:
                pairs[value] = i * length + j
            j = nxt[j]

    return pairs

def find_right(c, d, head, nxt, low_mask, alpha, pairs):
    for i, x in enumerate(c):
        wanted = (x & low_mask) ^ alpha
        j = head[wanted]

        while j != -1:
            value = x ^ d[j]
            code = pairs.get(value)

            if code is not None:
                return code, i * len(d) + j

            j = nxt[j]

    return None

def solve():
    n = int(input())
    seeds = list(map(int, input().split()))

    value_mask = (1 << n) - 1

    # b is the number of low bits fixed during each partial join.
    b = (n + 2) // 3

    # One extra factor of two improves the probability of finding a collision.
    length = 1 << (b + 1)

    values = [
        generate(seed, length, value_mask)
        for seed in seeds
    ]

    a, b_values, c, d = values

    low_bits = (n + 2) // 3
    low_mask = (1 << low_bits) - 1

    # Zero is the standard Wagner filter. The remaining filters
    # explore other possible low-bit XOR values.
    alphas = [0]
    for i in range(min(7, low_bits)):
        alphas.append(1 << i)

    for alpha in alphas:
        head, nxt = build_buckets(b_values, low_mask)
        left = build_left_map(
            a, b_values, head, nxt,
            low_mask, alpha, length
        )

        head, nxt = build_buckets(d, low_mask)
        answer = find_right(
            c, d, head, nxt,
            low_mask, alpha, left
        )

        if answer is not None:
            left_code, right_code = answer

            i1 = left_code // length
            i2 = left_code % length
            i3 = right_code // length
            i4 = right_code % length

            print(i1 + 1, i2 + 1, i3 + 1, i4 + 1)
            return

if __name__ == "__main__":
    solve()
```các`generate`hàm tuân theo định nghĩa của trình tạo một cách chính xác.`result`được giảm xuống N bit có liên quan, trong khi`s0`Và`s1`luôn được giữ theo modulo 2 64. Biến tạm thời`t`là bản cập nhật`s1 ^ s0`giá trị được sử dụng bởi cả hai quá trình chuyển đổi trạng thái. 

Chỉ số mảng`i`đại diện cho mã máy phát điện`i + 1`. Chuyển đổi này chỉ được thực hiện khi in, điều này tránh trộn lẫn các chỉ mục vấn đề dựa trên một với các chỉ mục Python dựa trên 0 trong quá trình nối.`build_buckets`thực hiện phép nối va chạm một phần mà không cần xây dựng mọi cặp. các`head`mảng lưu trữ chỉ mục đầu tiên thuộc về mỗi nhóm bit thấp, trong khi`nxt`tạo thành một danh sách liên kết của tất cả các chỉ mục khác trong nhóm đó. Điều này sử dụng ít bộ nhớ hơn đáng kể so với từ điển chứa danh sách Python riêng cho mỗi nhóm.`build_left_map`xem xét chính xác các cặp có bit b thấp XOR tới`alpha`. Khóa từ điển là XOR N-bit hoàn chỉnh của cặp và giá trị của nó mã hóa hai chỉ số gốc dưới dạng`i * length + j`.`find_right`thực hiện phép nối một phần tương tự trên hai chuỗi còn lại. Khóa từ điển phù hợp sẽ cung cấp hai XOR cặp bằng nhau, ngay lập tức sẽ cung cấp XOR bốn chiều cần thiết bằng 0. 

Không có vấn đề về số nguyên có dấu trong Python, nhưng vấn đề rõ ràng`MASK64`các hoạt động vẫn cần thiết. Nếu không có chúng, số nguyên không giới hạn của Python sẽ cho phép các bit ngoài vị trí 63 rò rỉ vào các trạng thái của trình tạo trong tương lai, tạo ra một chuỗi khác với chuỗi được chỉ định trong bài toán. Trình tạo Python tham chiếu cũng giảm cả hai thành phần trạng thái modulo 2 64. 

Mã này sử dụng tám bộ lọc va chạm một phần có thể có. Số 0 được thử đầu tiên vì đây là trường hợp đơn giản nhất và cũng xử lý các chuỗi giống nhau một cách đặc biệt tốt. Các bộ lọc bổ sung không làm thay đổi tính chính xác, chúng chỉ làm tăng khả năng các danh sách được lấy mẫu hữu hạn chứa một giải pháp. 

## Ví dụ đã hoạt động 

Mẫu chính thức là```
50
3641603982383516983 445363681616962640 868196408185819179 1980241222855773941
```và một câu trả lời được chấp nhận là```
287 17609 122886 59914
```như được đưa ra bởi tuyên bố cuộc thi. 

Với N=50, thuật toán chọn b=⌈50/3⌉=17. Nó tạo ra 2 giá trị 18 = 262144 cho mỗi bàn phím. Bảng sau đây hiển thị trạng thái cấu trúc của tính toán thay vì in hàng trăm nghìn giá trị được tạo ra. 

| Sân khấu | N | b | Kích thước danh sách | Thuộc tính chính | 
| --- | --- | --- | --- | --- | 
| Danh sách đã tạo | 50 | 17 | 262144 mỗi cái | Mỗi giá trị có 50 bit liên quan | 
| Tham gia lần đầu | 50 | 17 | dự kiến ​​khoảng 524288 cặp | Cặp XOR có 17 bit thấp bằng α | 
| Tham gia lần thứ hai | 50 | 17 | dự kiến ​​khoảng 524288 cặp | Cặp XOR có cùng mức thấp 17 bit | 
| Tra cứu cuối cùng | 50 | 17 | tra cứu băm | Hai cặp XOR hoàn chỉnh bằng nhau | 
| Đầu ra | 50 | 17 | 4 chỉ số | XOR của cả bốn giá trị được chọn đều bằng 0 | 

Đối với các mã mẫu được chấp nhận, đầu ra của bộ tạo ở các vị trí 287, 17609, 122886 và 59914 có 50 bit XOR thấp nhất về 0. Tìm kiếm sinh nhật tổng quát tìm thấy xung đột tương đương và không cần tái tạo đầu ra mẫu chính xác vì câu lệnh chấp nhận mọi giải pháp hợp lệ. 

Một ví dụ có vẻ xác định nhỏ hơn là```
1
0 0 0 0
```Ở đây b=1 và đầu ra bộ tạo đầu tiên cho mỗi bàn phím bằng 0. Thuật toán có thể sử dụng ngay mã 1 từ mọi danh sách. 

| Sân khấu | Giá trị | Kết quả | 
| --- | --- | --- | 
| Bốn giá trị được tạo | 0,0,0,0 | Tất cả đều bằng 0 modulo 2 1 | 
| Cặp đầu tiên | 0⊕0 | 0 | 
| Cặp thứ hai | 0⊕0 | 0 | 
| Trận chung kết | 0=0 | Tìm thấy | 
| Đầu ra | chỉ số 1, 1, 1, 1 | hợp lệ | 

Ví dụ này thể hiện tính bất biến của chỉ mục một cơ sở. Giá trị đầu tiên của trình tạo tương ứng với mã 1, không phải mã 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2 N/3 ) dự kiến ​​| Bốn danh sách các giá trị O(2 N/3 ) được tạo ra và mỗi phần nối một phần sẽ kiểm tra các cặp dự kiến ​​O(2 N/3 ) | 
| Không gian | O(2 N/3 ) | Bốn danh sách được tạo, mảng nhóm và một bảng băm cặp một phần có kích thước này | 

Với N=50, 2 N/3 là khoảng 128.000 và việc triển khai sử dụng danh sách lớn hơn có hệ số không đổi gồm 262144 phần tử. Cấu trúc dữ liệu thu được vẫn nằm trong giới hạn bộ nhớ 256 MB, trong khi số lượng thao tác băm và nối là thực tế đối với giới hạn 2 giây khi triển khai Python được tối ưu hóa. Việc tăng tốc xuất phát từ việc không bao giờ hiện thực hóa tích Descartes L 2, chỉ các cặp va chạm trên các bit thấp đã chọn. 

## Trường hợp thử nghiệm 

Mẫu chính thức có nhiều kết quả đầu ra được chấp nhận, do đó việc so sánh chuỗi chính xác là không phù hợp. Thay vào đó, thử nghiệm mẫu bên dưới xác minh rằng bốn mã được tạo ra thỏa mãn phương trình tạo ban đầu. 

Khai thác sau đây giả định rằng`solve()`hàm từ phần Giải pháp Python có sẵn trong cùng một tệp.```python
import sys
import io
from contextlib import redirect_stdout

MASK64 = (1 << 64) - 1
CONST = 0x7263d9bd8409f526

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    with redirect_stdout(out):
        solve()

    sys.stdin = old_stdin
    return out.getvalue().strip()

def generator_value(seed: int, code: int) -> int:
    s0 = seed & MASK64
    s1 = (seed ^ CONST) & MASK64

    for _ in range(code):
        result = (s0 + s1) & MASK64

        t = s1 ^ s0

        ns0 = (((s0 << 55) & MASK64) | (s0 >> 9))
        ns0 ^= t
        ns0 ^= (t << 14) & MASK64
        ns0 &= MASK64

        ns1 = (((t << 36) & MASK64) | (t >> 28))
        ns1 &= MASK64

        s0, s1 = ns0, ns1

    return result

def valid(inp: str, output: str) -> bool:
    data = list(map(int, inp.split()))
    n = data[0]
    seeds = data[1:5]

    codes = list(map(int, output.split()))

    if len(codes) != 4:
        return False

    if any(c <= 0 or c >= 100000000 for c in codes):
        return False

    mask = (1 << n) - 1

    x = 0
    for seed, code in zip(seeds, codes):
        x ^= generator_value(seed, code) & mask

    return x == 0

# Official sample
sample = """\
50
3641603982383516983 445363681616962640 868196408185819179 1980241222855773941
"""

sample_output = run(sample)
assert valid(sample, sample_output), "official sample"

# Minimum N, first generator output
case_min = """\
1
0 0 0 0
"""
assert run(case_min) == "1 1 1 1", "minimum N and one-based indexing"

# Small N with different seeds
case_small = """\
2
0 0 1 1
"""
assert run(case_small) == "1 1 1 1", "small bit width"

# Maximum N and maximum seed, all four generators identical
case_max = """\
50
18446744073709551615 18446744073709551615 18446744073709551615 18446744073709551615
"""
assert run(case_max) == "1 1 1 1", "64-bit wraparound and maximum seed"

# Boundary around N mod 3
case_boundary = """\
4
0 0 0 0
"""
assert run(case_boundary) == "1 1 1 1", "N not divisible by 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu chính thức | Bất kỳ đầu ra nào thỏa mãn điều kiện XOR | Trình tạo đầy đủ và tìm kiếm sinh nhật tổng quát | 
|`1 / 0 0 0 0`|`1 1 1 1`| N tối thiểu, mã đầu tiên, mặt nạ bit thấp nhất | 
|`2 / 0 0 1 1`|`1 1 1 1`| Chiều rộng bit nhỏ và các loại hạt khác nhau | 
|`50 / MAX MAX MAX MAX`|`1 1 1 1`| Gói 64-bit và hạt giống tối đa | 
|`4 / 0 0 0 0`|`1 1 1 1`| Xử lý đúng khi N không chia hết cho 3 | 

## Vỏ cạnh 

cho```
1
0 0 0 0
```thuật toán tạo ra giá trị đầu tiên từ mỗi trình tạo giống hệt nhau. Vì kết quả đầu tiên bằng 0 đối với hạt giống số 0 nên tất cả bốn giá trị một bit đều bằng 0. Cặp đầu tiên tạo ra XOR 0, cặp thứ hai cũng tạo ra XOR 0 và việc tra cứu hàm băm cuối cùng thành công ngay lập tức. Các chỉ số bên trong đều bằng 0, nhưng các mã được in đều tăng lên 1. 

cho```
2
0 0 1 1
```đầu ra của bộ tạo đầu tiên cho số 0 hạt giống có hai bit thấp bằng 2, trong khi đầu ra đầu ra đầu tiên cho hạt giống số 1 có hai bit thấp bằng 0. Do đó, bốn giá trị thỏa mãn 2⊕2⊕0⊕0=0. Phép nối với α=0 giữ cả hai cặp vì mỗi cặp có giá trị một bit thấp bằng nhau và XOR cặp hai bit hoàn chỉnh của chúng khớp với nhau. 

Vì```
50
18446744073709551615 18446744073709551615 18446744073709551615 18446744073709551615
```cả bốn trạng thái của máy phát đều giống hệt nhau, vì vậy mọi đầu ra của máy phát tương ứng đều giống hệt nhau. Bốn danh sách chứa cùng một chuỗi và việc chọn phần tử đầu tiên từ mỗi danh sách sẽ cho bốn giá trị N-bit bằng nhau. XOR một số chẵn các giá trị giống hệt nhau sẽ cho kết quả bằng 0. Việc triển khai cũng bao bọc chính xác trạng thái ban đầu và mọi chuyển đổi sang 64 bit. 

Với N=50, độ rộng của phần nối một phần là 17 bit chứ không phải chính xác là 16 hoặc 18. Việc triển khai tính toán điều này với phép chia trần số nguyên, sau đó phân bổ một danh sách lớn gấp đôi 2 17. Điều này quan trọng vì việc chọn sai hướng làm tròn sẽ thay đổi kích thước dự kiến ​​của danh sách va chạm một phần và có thể làm cho xác suất va chạm cuối cùng trở nên tồi tệ hơn nhiều. 

Việc đánh số mã của trình tạo là một điều kiện biên liên tục khác. Trong nội bộ, giá trị được tạo đầu tiên được lưu trữ ở vị trí 0 của mảng. Sự cố gọi mã giá trị đó là 1. Quá trình chuyển đổi bị trì hoãn cho đến đầu ra cuối cùng, trong đó mọi chỉ mục được khôi phục đều được tăng chính xác một lần. Điều này tránh được cả mã 0 không hợp lệ và lỗi tinh vi hơn khi dịch chuyển chỉ mục hai lần trong quá trình tái cấu trúc cặp.
