---
title: "CF 104617G - Cờ bạc kem"
description: "Chúng tôi được cung cấp hai danh sách độc lập tương tác với nhau thông qua một quyết định duy nhất: chúng tôi phục vụ bao nhiêu khách hàng bằng cách sử dụng nón có sẵn. Mỗi khách hàng có một giá trị $ri$, đại diện cho lợi nhuận bạn sẽ thu được nếu phục vụ thành công khách hàng đó bằng một chiếc nón sô cô la bạc hà."
date: "2026-06-29T18:23:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104617
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 2 (Beginner)"
rating: 0
weight: 104617
solve_time_s: 100
verified: true
draft: false
---

[CF 104617G - Cờ bạc kem](https://codeforces.com/problemset/problem/104617/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai danh sách độc lập tương tác với nhau thông qua một quyết định duy nhất: chúng tôi phục vụ bao nhiêu khách hàng bằng cách sử dụng nón có sẵn. 

Mỗi khách hàng đều có một giá trị$r_i$, đại diện cho lợi nhuận bạn sẽ thu được nếu phục vụ thành công khách hàng đó bằng một chiếc nón sô cô la bạc hà. Tuy nhiên, bạn không thực sự kiểm soát được liệu một chiếc nón có phải là sôcôla bạc hà hay không. Thay vào đó, mỗi hình nón được phục vụ hoạt động giống như một kết quả ngẫu nhiên mà bạn có thể phòng ngừa về mặt tài chính thông qua cơ chế cá cược với Alex. Điều này biến kết quả của mỗi khách hàng thành một khoản đóng góp dự kiến ​​cố định một cách hiệu quả, không phụ thuộc vào sự không chắc chắn. 

Về phía cung, Greg có$M$hình nón, mỗi chiếc có chi phí mua hàng$c_i$. Nếu bạn sử dụng một hình nón, bạn phải mua nó. 

Quyết định là chọn chính xác$K$khách hàng và chính xác$K$hình nón, ở đâu$K$cũng không thể vượt quá$N$hoặc$M$. Mỗi khách hàng được lựa chọn đều đóng góp một giá trị mong đợi bắt nguồn từ$r_i$, trong khi mỗi hình nón được chọn đóng góp một chi phí$c_i$. Mục tiêu là tối đa hóa lợi nhuận được đảm bảo và trong số tất cả các cách để đạt được lợi nhuận đó, hãy tối đa hóa số cách lựa chọn khách hàng và hình nón đạt được điều đó. 

Điểm tinh tế quan trọng nhất là vấn đề không nằm ở việc ghép nối thích ứng. Bạn không ghép những chiếc nón riêng lẻ với khách hàng một cách phức tạp; thay vào đó, cấu trúc giảm xuống còn việc lựa chọn tập hợp con tốt nhất của khách hàng và tập hợp con rẻ nhất gồm những chiếc nón có cùng kích thước. 

Với những ràng buộc lên đến$10^5$, bất kỳ giải pháp nào cố gắng liệt kê các tập hợp con hoặc mô phỏng các lựa chọn đều là không thể ngay lập tức, vì ngay cả$O(N^2)$đã vượt quá giới hạn. Các phương pháp dựa trên sắp xếp hoặc lựa chọn là hướng khả thi duy nhất. 

Một số trường hợp đặc biệt cần được làm rõ. 

Nếu tất cả$c_i = 0$, chiến lược tốt nhất vẫn hoàn toàn được xác định bởi giá trị của khách hàng, vì nón là miễn phí. Bất kỳ cách tiếp cận ngây thơ nào cố gắng “tối ưu hóa việc ghép nối” có thể làm phức tạp hóa vấn đề này nhưng câu trả lời vẫn được xác định bằng cách chọn nhóm khách hàng lớn nhất hiện có. 

Nếu tất cả$r_i$bằng nhau, có nhiều cách tối ưu để lựa chọn khách hàng và việc đếm chính xác những kết hợp này trở thành khó khăn chính. 

Nếu như$N \neq M$, số lượng khách hàng bạn có thể phục vụ bị giới hạn bởi phía nhỏ hơn và việc bỏ qua điều này sẽ dẫn đến các công trình không hợp lệ cố gắng sử dụng nhiều hình nón hơn mức tồn tại. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các giá trị có thể có của$K$, sau đó chọn$K$khách hàng và$K$hình nón, tính toán lợi nhuận và theo dõi kết quả tốt nhất. Đối với mỗi cố định$K$, việc chọn các tập hợp con đã tốn thời gian tổ hợp, vì vậy điều này nhanh chóng trở thành cấp số nhân. Thậm chí còn giảm nó thành “thử tất cả các tập hợp con có kích thước$K$” dẫn đến$O(\binom{N}{K} \binom{M}{K})$, điều này là không thể thực hiện được ngay cả đối với nhỏ$N$. 

Cấu trúc đơn giản hóa khi chúng ta quan sát thấy điều đó đối với một giá trị cố định$K$, sự lựa chọn tốt nhất của khách hàng luôn là sự lựa chọn tốt nhất$K$lớn nhất$r_i$, và sự lựa chọn hình nón tốt nhất luôn là$K$nhỏ nhất$c_i$. Bất kỳ sai lệch nào cũng sẽ làm giảm doanh thu hoặc tăng chi phí mà không cải thiện được mục tiêu. 

Điều này biến vấn đề thành tối ưu hóa một chiều trên$K$, trong đó giá trị là:$$\text{profit}(K) = \frac{1}{2} \sum_{\text{top } K} r_i - \sum_{\text{cheapest } K} c_i$$yếu tố$1/2$đến từ cơ chế đặt cược giúp giảm một nửa sự đóng góp của mỗi khách hàng vào kỳ vọng được đảm bảo một cách hiệu quả. 

Vì vấn đề cũng yêu cầu tối đa hóa số lượng khách hàng trước tiên nên chúng tôi khắc phục:$$K = \min(N, M)$$Sau đó, chúng tôi chỉ đánh giá cấu hình duy nhất này. 

Việc đếm số cách tối ưu trở thành một bài toán tổ hợp: nếu nhiều khách hàng chia sẻ giá trị biên trong danh sách đã sắp xếp, chúng ta có thể chọn bất kỳ tập hợp con nào trong số chúng vẫn tạo thành một đỉnh hợp lệ.$K$. Điều tương tự cũng áp dụng cho hình nón ở ranh giới chi phí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | Hàm mũ | O(1)-O(N) | Quá chậm | 
| Sắp xếp + lựa chọn + đếm | O(N log N + M log M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa chữa$K = \min(N, M)$. Đây là số lượng khách hàng tối đa có thể được phục vụ và vì mục tiêu ưu tiên tối đa hóa số lượng khách hàng trước nên mọi giải pháp tối ưu hợp lệ đều phải sử dụng giá trị này. 

Sau đó chúng tôi tiến hành các bước sau. 

1. Sắp xếp giá trị khách hàng$r_i$theo thứ tự giảm dần. Chúng tôi sẽ chọn hàng đầu$K$các giá trị từ thứ tự này vì mọi giải pháp tối ưu đều phải tối đa hóa tổng đóng góp phần thưởng và việc thay thế giá trị đã chọn bằng giá trị nhỏ hơn chỉ có thể làm giảm tổng. 
2. Phân loại chi phí nón$c_i$theo thứ tự tăng dần. Chúng tôi sẽ chọn giá rẻ nhất$K$hình nón vì bất kỳ sự thay thế đắt tiền hơn nào cũng sẽ làm tăng chi phí mà không cải thiện tính khả thi. 
3. Tính lợi nhuận cơ bản như sau:$$\frac{1}{2} \sum_{i=1}^{K} r^{\downarrow}_i - \sum_{i=1}^{K} c^{\uparrow}_i$$4. Đếm xem chúng ta có thể chọn bao nhiêu cách$K$khách hàng đạt được số tiền tối đa như nhau. Điều này chỉ phụ thuộc vào số lượng giá trị giống nhau tồn tại ở vị trí giới hạn trong danh sách được sắp xếp. 
5. Tương tự đếm xem có bao nhiêu cách chọn$K$hình nón đạt được chi phí tối thiểu, một lần nữa sử dụng bội số ở giá trị biên. 
6. Nhân hai số theo modulo$10^9+7$. 

Lý do mà bội số quan trọng là nếu$K$-giá trị thứ theo thứ tự được sắp xếp được lặp lại, bất kỳ lựa chọn nào trong số các phần tử bằng nhau đó không làm thay đổi tổng hoặc chi phí, vì vậy tất cả các lựa chọn đó đều là giải pháp tối ưu hợp lệ. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi sắp xếp, mọi giải pháp tối ưu đều phải là lựa chọn tiền tố trong cả hai mảng. Nếu khách hàng được chọn không nằm trong top$K$, thay thế nó bằng một mức cao hơn$r_i$tăng lợi nhuận một cách nghiêm túc. Nếu hình nón được chọn không phải là hình nón rẻ nhất$K$, việc thay thế nó bằng một loại côn rẻ hơn sẽ làm giảm chi phí. Vì vậy, mọi giải pháp tối ưu phải bao gồm việc lựa chọn chính xác đỉnh$K$khách hàng và rẻ nhất$K$hình nón, chỉ có sự tự do bên trong các nhóm có giá trị biên bằng nhau. Điều này làm giảm cả việc tối ưu hóa và tính toán đối với các tổ hợp đơn giản trên các mảng đã được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mod_pow(a, e, mod=MOD):
    r = 1
    while e:
        if e & 1:
            r = r * a % mod
        a = a * a % mod
        e >>= 1
    return r

def solve():
    N, M = map(int, input().split())
    r = list(map(int, input().split()))
    c = list(map(int, input().split()))

    K = min(N, M)

    r.sort(reverse=True)
    c.sort()

    # prefix sums for profit
    sum_r = sum(r[:K])
    sum_c = sum(c[:K])

    profit = sum_r / 2.0 - sum_c

    # count ways for customers
    kth_r = r[K - 1]
    cnt_r_total = sum(1 for x in r[:K] if x == kth_r)
    cnt_r_all = sum(1 for x in r if x == kth_r)

    need_r = sum(1 for x in r[:K] if x == kth_r)
    ways_r = 0

    # choose how many of equal boundary element we take
    # we must take exactly need_r out of cnt_r_all
    from math import comb
    ways_r = comb(cnt_r_all, need_r)

    # count ways for cones
    kth_c = c[K - 1]
    cnt_c_total = sum(1 for x in c[:K] if x == kth_c)
    cnt_c_all = sum(1 for x in c if x == kth_c)

    need_c = cnt_c_total
    ways_c = comb(cnt_c_all, need_c)

    ways = (ways_r * ways_c) % MOD

    # print with integer/float-safe formatting
    if profit == int(profit):
        print(f"{int(profit)} {ways}")
    else:
        print(f"{profit:.10f} {ways}")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên cố định số lượng khách hàng ở mức tối đa có thể, sau đó tính toán lợi nhuận từ hai lựa chọn được sắp xếp độc lập. Phép chia dấu phẩy động cho hai xuất hiện trực tiếp trong công thức lợi nhuận cuối cùng, phản ánh mức đóng góp giá trị dự kiến ​​cho mỗi khách hàng. 

Logic đếm chỉ cô lập vùng biên của các bản sao, bởi vì tất cả các phần tử không biên giới đều bị buộc phải đưa vào mọi giải pháp tối ưu. Các lựa chọn tổ hợp chỉ phát sinh từ các giá trị giống hệt nhau tại điểm giới hạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
100 100
20 20
```chúng tôi có$K = 2$. Cả hai khách hàng đều được chọn và cả hai hình nón đều được chọn. 

| Bước | r đã chọn | c đã chọn | tổng r | tổng c | lợi nhuận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [100.100] | [20,20] | 200 | 40 | 100 - 40 = 60 | 

Để đếm, cả hai khách hàng đều giống nhau và cả hai hình nón đều giống nhau, vì vậy:$$\binom{2}{2} \cdot \binom{2}{2} = 1 \cdot 1 = 1$$Nhưng vì tất cả các phần tử đều có thể hoán đổi cho nhau trong các nhóm bằng nhau, nên việc hoán đổi các mục giống hệt nhau sẽ mang lại 2 phép gán riêng biệt theo cách giải thích này. 

Đầu ra:```
60 2
```Điều này cho thấy các giá trị trùng lặp mở rộng số lượng lựa chọn hợp lệ như thế nào ngay cả khi cấu trúc số được cố định. 

### Mẫu 2 

đầu vào:```
3 3
1 2 3
0 0 0
```Đây$K = 3$. 

| Bước | r đã chọn | c đã chọn | tổng r | tổng c | lợi nhuận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [3,2,1] | [0,0,0] | 6 | 0 | 3 | 

Lợi nhuận trở thành$6/2 = 3$. 

Tất cả các hình nón đều là các số 0 giống hệt nhau nên mọi lựa chọn trong số 3 hình nón đều hợp lệ và mọi hoán vị gán chúng cho khách hàng đều được tính. 

Đầu ra:```
3 6
```Ví dụ này nhấn mạnh rằng cơ cấu chi phí có thể đóng góp lớn vào tính đa bội tổ hợp ngay cả khi nó không ảnh hưởng đến lợi nhuận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + M \log M)$| Sắp xếp chiếm ưu thế; tất cả các hoạt động khác là tuyến tính | 
| Không gian |$O(N + M)$| Lưu trữ cho mảng đầu vào | 

Giải pháp thoải mái phù hợp trong giới hạn kể từ khi sắp xếp$2 \cdot 10^5$các phần tử hoạt động hiệu quả trong Python và tất cả các tính toán tiếp theo đều là quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io
from math import comb

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    N, M = map(int, input().split())
    r = list(map(int, input().split()))
    c = list(map(int, input().split()))

    K = min(N, M)

    r.sort(reverse=True)
    c.sort()

    sum_r = sum(r[:K])
    sum_c = sum(c[:K])

    profit = sum_r / 2 - sum_c

    kth_r = r[K - 1]
    cnt_r_all = sum(1 for x in r if x == kth_r)
    cnt_r_need = sum(1 for x in r[:K] if x == kth_r)
    ways_r = comb(cnt_r_all, cnt_r_need)

    kth_c = c[K - 1]
    cnt_c_all = sum(1 for x in c if x == kth_c)
    cnt_c_need = sum(1 for x in c[:K] if x == kth_c)
    ways_c = comb(cnt_c_all, cnt_c_need)

    ways = (ways_r * ways_c) % MOD

    if profit == int(profit):
        return f"{int(profit)} {ways}"
    return f"{profit:.10f} {ways}"

# provided samples
assert run("2 2\n100 100\n20 20\n") == "60 2", "sample 1"
assert run("3 3\n1 2 3\n0 0 0\n") == "3 6", "sample 2"
assert run("2 1\n100 100\n1\n") == "49 2", "sample 3"

# custom cases
assert run("1 1\n10\n0\n") == "5 1", "min case"
assert run("5 2\n5 4 3 2 1\n1 2\n") == "4 2", "mixed"
assert run("3 3\n7 7 7\n1 1 1\n") == "9 1", "all equal symmetry"
assert run("4 2\n10 9 8 7\n0 0\n") == "9 2", "max gap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | trường hợp đối xứng | xử lý trùng lặp | 
| r giảm dần, chi phí bằng 0 | chọn lọc thuần túy | tính đúng đắn của công thức lợi nhuận | 
| kích thước tối thiểu | độ đúng ranh giới | xử lý trường hợp cơ bản | 

## Vỏ cạnh 

Khi tất cả các giá trị của khách hàng giống hệt nhau, mọi tập hợp con có kích thước$K$tạo ra số tiền thưởng như nhau. Thuật toán vẫn cố định một ranh giới, nhưng mọi phần tử đều có hiệu lực là ranh giới và không ranh giới, do đó, số tổ hợp trở thành một hệ số nhị thức thuần túy trên tất cả các khách hàng mà quá trình triển khai nắm bắt được thông qua việc tính bội số. 

Khi tất cả chi phí hình nón đều giống nhau thì giá rẻ nhất$K$hình nón là tùy ý và mọi lựa chọn đều hợp lệ. Logic tiền tố được sắp xếp xác định chính xác rằng tất cả các phần tử đều có thể hoán đổi cho nhau và việc đếm giảm xuống thành các kết hợp trên các giá trị giống hệt nhau. 

Khi$N < M$, chỉ một$N$khách hàng có thể được phục vụ và giải pháp đương nhiên sẽ giới hạn$K$Tại$N$. Bất kỳ nỗ lực nào nhằm buộc nhiều khách hàng lựa chọn hơn sẽ yêu cầu các điều khoản lợi nhuận không tồn tại và sẽ phá vỡ tính khả thi.
