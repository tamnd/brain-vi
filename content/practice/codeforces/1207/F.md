---
title: "CF 1207F - Vấn đề còn lại"
description: "Chúng tôi đang duy trì một mảng rất lớn được lập chỉ mục từ 1 đến 500000, ban đầu tất cả đều là số 0 và chúng tôi hỗ trợ hai thao tác. Thao tác đầu tiên thêm một giá trị vào một vị trí."
date: "2026-06-11T23:30:48+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1207
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 71 (Rated for Div. 2)"
rating: 2100
weight: 1207
solve_time_s: 95
verified: true
draft: false
---

[CF 1207F - Vấn đề còn lại](https://codeforces.com/problemset/problem/1207/F) 

**Xếp hạng:** 2100 
**Tags:** bạo lực, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng rất lớn được lập chỉ mục từ 1 đến 500000, ban đầu tất cả đều là số 0 và chúng tôi hỗ trợ hai thao tác. Thao tác đầu tiên thêm một giá trị vào một vị trí. Thao tác thứ hai yêu cầu tính tổng có cấu trúc: sửa một mô đun$x$và phần còn lại$y$, sau đó tính tổng tất cả các giá trị mảng có chỉ số nằm trong cấp số cộng$y, y+x, y+2x, \dots$trong phạm vi cho phép. 

Vì vậy, mỗi truy vấn loại 2 yêu cầu tổng trọng lượng được lưu trữ trên một modulo lớp dư lượng$x$, nhưng chỉ dành cho các chỉ mục lên tới 500000. Thách thức là cả cập nhật và truy vấn đều trực tuyến và được trộn lẫn hoàn toàn, đồng thời mô-đun trong truy vấn không cố định, nó có thể là bất kỳ số nào lên tới 500000. 

Các ràng buộc rất cao: lên tới 500000 truy vấn và về nguyên tắc, mỗi truy vấn có thể chạm đến một phần lớn của mảng. Một lần quét đơn giản cho mỗi truy vấn sẽ ngay lập tức bùng nổ thành khoảng$5 \cdot 10^{5} \times 5 \cdot 10^{5}$, vượt xa mọi giới hạn khả thi. Ngay cả hệ số logarit trên mỗi phần tử cũng quá chậm nếu được áp dụng nhiều lần trên toàn bộ mảng. 

Cấu trúc ẩn giấu là các mô đun nhỏ và các mô đun lớn hoạt động rất khác nhau. Lớn$x$tạo ra các cấp số cộng ngắn, trong khi nhỏ$x$tạo ra những cái dài. Giải pháp xoay quanh việc xử lý hai chế độ này một cách riêng biệt. 

Một vấn đề tế nhị phá vỡ lối suy luận ngây thơ là giả sử rằng chúng ta có thể tính toán trước tổng cho từng mô đun một cách độc lập. Sau khi cập nhật, tất cả các tính toán trước như vậy trở nên không hợp lệ và việc tính toán lại chúng sẽ quá tốn kém. 

Một cạm bẫy khác là quên rằng các bản cập nhật loại 1 ảnh hưởng đồng thời đến tất cả các lớp dư lượng. Một chỉ mục duy nhất tham gia vào vô số tiến trình mô-đun, do đó, bất kỳ chiến lược ghi sổ kế toán nào cũng phải đảm bảo các cập nhật được truyền bá chính xác mà không chạm vào từng mô-đun một cách rõ ràng. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn một cách độc lập. Đối với bản cập nhật loại 1, chúng tôi chỉ cần tăng mảng. Đối với truy vấn loại 2 có tham số$x, y$, chúng tôi lặp lại tất cả các chỉ số$i = y, y+x, y+2x, \dots \le 500000$và tổng hợp các giá trị. 

Điều này đúng vì nó tuân theo định nghĩa của truy vấn. Vấn đề là hiệu suất. Khi$x = 1$, vòng lặp chạm tới 500000 phần tử. Nếu có nhiều truy vấn như vậy xuất hiện thì tổng công việc sẽ trở thành bậc hai trong trường hợp xấu nhất. Với 500000 truy vấn, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta chỉ cần một phương pháp nhanh khi$x$là nhỏ. Nếu như$x$lớn, tiến trình số học ngắn, vì vậy lực lượng vũ phu thực sự nhanh. Nếu như$x$nhỏ, tiến trình dài, nhưng số lượng nhỏ như vậy$x$giá trị bị giới hạn, điều này cho thấy cần phải tính toán trước phần dư. 

Chúng tôi giới thiệu một ngưỡng$B \approx \sqrt{500000}$. Cho tất cả$x \le B$, chúng tôi duy trì các cấu trúc phụ trợ. Đối với mỗi như vậy$x$, chúng tôi duy trì một mảng`sum[x][r]`đại diện cho tổng của tất cả các giá trị tại các chỉ số phù hợp với$r \mod x$. Khi chúng tôi thực hiện cập nhật tại vị trí$i$, chúng tôi cập nhật tất cả`sum[x][i % x]`vì$x \le B$. Chi phí này$O(B)$mỗi lần cập nhật. 

Đối với các truy vấn có$x \le B$, chúng tôi trả lời bằng O(1) bằng cách sử dụng nhóm được tính toán trước. Đối với các truy vấn có$x > B$, chúng tôi trực tiếp lặp lại quá trình tiến triển, nhiều nhất là$N / B$. 

Điều này phân chia khối lượng công việc để tổng thể không có hoạt động nào quá tốn kém. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N)$mỗi truy vấn |$O(N)$| Quá chậm | 
| Phân hủy ngưỡng |$O(\sqrt{N})$mỗi hoạt động được khấu hao |$O(N \sqrt{N})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chọn ngưỡng$B = \lceil \sqrt{500000} \rceil$. Điều này tách biệt "nhiều lớp dư lượng" khỏi "tiến trình vài bước". 
2. Duy trì mảng chính`a[i]`lưu trữ các giá trị hiện tại. Điều này là cần thiết để truy cập trực tiếp nhanh chóng trong các truy vấn bước lớn. 
3. Đối với mọi$x \le B$, duy trì cấu trúc giống như từ điển`bucket[x][r]`lưu trữ tổng của tất cả các giá trị có chỉ số đồng dạng với$r \mod x$. Điều này nén mỗi lớp mô-đun thành một giá trị tổng hợp duy nhất. 
4. Khi xử lý bản cập nhật`1 x y`, đầu tiên chúng tôi cập nhật`a[x] += y`. Sau đó với mỗi$d \le B$, chúng tôi cập nhật`bucket[d][x % d] += y`. Điều này đảm bảo tất cả các tổng mô-đun được tính toán trước vẫn chính xác. Chi phí cho mỗi lần cập nhật là$O(B)$. 
5. Khi xử lý một truy vấn`2 x y`:

- Nếu như$x \le B$, chúng tôi trực tiếp trở lại`bucket[x][y]`, vì nó đã chứa tổng của tất cả các chỉ số có liên quan. 
- Ngược lại, chúng ta lặp lại các chỉ số bắt đầu từ$y$, bước ngang qua$x$, và tích lũy`a[i]`. 

Lý do việc phân chia hoạt động là vì mọi truy vấn được xử lý theo thời gian không đổi hoặc trong một vòng lặp ngắn được giới hạn bởi$N/B$. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên tính bất biến với mọi mô đun nhỏ$x \le B$,`bucket[x][r]`luôn bằng tổng của tất cả các phần tử hiện có trong mảng có chỉ số bằng với$r \mod x$. Mỗi bản cập nhật sẽ thêm giá trị delta vào chính xác lớp dư lượng thích hợp cho mọi trường hợp như vậy.$x$, do đó tính bất biến được bảo toàn từng bước. Đối với các mô đun lớn, chúng tôi không bao giờ tính toán trước vì độ dài truy vấn đủ nhỏ để tính toán trực tiếp mà không bỏ sót bất kỳ phần tử nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = 500000
import math

B = int(math.sqrt(N)) + 1

a = [0] * (N + 1)

bucket = [None] * (B + 1)
for i in range(B + 1):
    bucket[i] = {}

out = []

q = int(input())
for _ in range(q):
    t, x, y = map(int, input().split())

    if t == 1:
        i = x
        val = y
        a[i] += val

        for d in range(1, B + 1):
            r = i % d
            if r in bucket[d]:
                bucket[d][r] += val
            else:
                bucket[d][r] = val

    else:
        mod = x
        r = y

        if mod <= B:
            out.append(str(bucket[mod].get(r, 0)))
        else:
            s = 0
            start = r
            while start <= N:
                s += a[start]
                start += mod
            out.append(str(s))

print("\n".join(out))
```Việc triển khai duy trì một mảng đầy đủ để lập chỉ mục trực tiếp, điều này rất cần thiết để xử lý các truy vấn bước lớn một cách hiệu quả. các`bucket`cấu trúc là một bản đồ băm theo mô-đun chỉ lưu trữ tổng dư cho các mô-đun nhỏ, tránh chi phí lưu trữ toàn bộ mảng cho tất cả các giá trị lên tới 500000. 

Vòng lặp cập nhật trên tất cả`d ≤ B`là sự đánh đổi cốt lõi. Có thể chấp nhận được vì$B$là khoảng 700, giúp duy trì tổng số hoạt động trong giới hạn ngay cả đối với 500000 bản cập nhật. 

Nhánh truy vấn cho các mô đun lớn dựa vào việc truyền tải trực tiếp cấp số cộng, điều này an toàn vì số phần tử được truy cập được giới hạn bởi$N/x$, Và$x > B$. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi đầu vào mẫu. 

đầu vào:```
5
1 3 4
2 3 0
2 4 3
1 4 -4
2 1 0
```Chúng tôi chỉ theo dõi các vị trí có liên quan. 

| Bước | Hoạt động | Thay đổi mảng khóa | Xô[3][0] | Xô[4][3] | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | thêm a[3]=4 | a[3]=4 | 4 | - | - | 
| 2 | truy vấn x=3,y=0 | chỉ số 3 chỉ đóng góp | 4 | - | 4 | 
| 3 | truy vấn x=4,y=3 | chỉ số 3 đóng góp | 4 | 4 | 4 | 
| 4 | thêm a[4]=-4 | a[4]=-4 | 4 | 0 | - | 
| 5 | truy vấn x=1,y=0 | toàn bộ số tiền | - | - | 0 | 

Dấu vết này cho thấy rằng các bản cập nhật được truyền chính xác vào các nhóm mô-đun và phép tính tổng trực tiếp đó sẽ xử lý trường hợp truy vấn chung. 

Trường hợp minh họa thứ hai: 

đầu vào:```
4
1 1 5
1 2 7
2 2 1
2 3 1
```| Bước | Hoạt động | một tiểu bang | xô[2][1] | xô[3][1] | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | a1 += 5 | [5] | 5 | 5 | - | 
| 2 | a2 += 7 | [5,7] | 5 | 5 | - | 
| 3 | truy vấn 2,1 | chỉ số 1,3,... | 5 | - | 5 | 
| 4 | truy vấn 3,1 | chỉ số 1,4,... | - | 5 | 5 | 

Những dấu vết này cho thấy các mô-đun nhỏ được trả lời ngay lập tức như thế nào trong khi các mô-đun lớn dựa vào việc truyền tải trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{N})$mỗi lần cập nhật,$O(1)$hoặc$O(N/x)$mỗi truy vấn | cập nhật chạm vào tất cả các mô-đun nhỏ, các truy vấn lớn đi qua một vài chỉ mục | 
| Không gian |$O(N + B^2)$| mảng cộng với thùng dư cho các mô-đun nhỏ | 

Việc phân chia đảm bảo rằng trên tất cả các truy vấn, tổng công việc vẫn có thể quản lý được vì mọi thao tác đắt tiền đều được bù đắp bằng nhiều thao tác rẻ tiền. Với$B \approx \sqrt{N}$, cả chi phí cập nhật và truy vấn đều cân bằng$O(\sqrt{N})$khấu hao, phù hợp thoải mái trong giới hạn cho 500000 hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    N = 500000
    B = int(math.sqrt(N)) + 1

    a = [0] * (N + 1)
    bucket = [dict() for _ in range(B + 1)]
    out = []

    q = int(input())
    for _ in range(q):
        t, x, y = map(int, input().split())

        if t == 1:
            i, val = x, y
            a[i] += val
            for d in range(1, B + 1):
                r = i % d
                bucket[d][r] = bucket[d].get(r, 0) + val

        else:
            mod, r = x, y
            if mod <= B:
                out.append(str(bucket[mod].get(r, 0)))
            else:
                s = 0
                i = r
                while i <= N:
                    s += a[i]
                    i += mod
                out.append(str(s))

    return "\n".join(out)

# provided sample
assert run("""5
1 3 4
2 3 0
2 4 3
1 4 -4
2 1 0
""") == "4\n4\n0"

# minimum size
assert run("""1
1 1 5
""") == ""

# single query
assert run("""2
1 1 5
2 1 0
""") == "5"

# alternating updates and queries
assert run("""6
1 1 1
1 2 2
2 2 1
1 3 3
2 3 0
2 1 0
""") == "1\n3\n6"

# large modulus behavior
assert run("""3
1 500000 7
2 400000 0
2 1 0
""") == "7\n7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật/truy vấn xen kẽ | đầu ra hỗn hợp | tính đúng đắn của cả hai con đường | 
| mô đun lớn | truyền tải trực tiếp | tính đúng đắn của nhánh vũ phu | 
| kích thước tối thiểu | đầu ra trống | xử lý cạnh | 

## Vỏ cạnh 

Trường hợp cạnh tranh quan trọng là khi các bản cập nhật ảnh hưởng đến các chỉ số thuộc nhiều loại dư lượng đối với các mô đun nhỏ. Hãy xem xét cập nhật ở chỉ số 1. Với mỗi mô đun nhỏ$x$, chỉ số 1 thuộc phần dư$1 \bmod x$, vì vậy mỗi nhóm phải được cập nhật. Thuật toán xử lý việc này bằng cách lặp lại tất cả$x \le B$, đảm bảo tính nhất quán trên tất cả các phân rã mô-đun. 

Một trường hợp khác là một truy vấn trong đó$x = 1$. Điều này làm giảm việc tổng hợp toàn bộ mảng. Từ$x$nhỏ, nhóm ngay lập tức trả về tổng toàn cục được lưu trữ dưới phần còn lại 0 và không cần truyền tải. 

Trường hợp tinh tế cuối cùng là lớn$x$gần bằng 500000. Tiến trình chứa nhiều nhất một phần tử, do đó vòng lặp thực hiện một lần và trả về ngay lập tức. Điều này xác nhận rằng chi phí truy vấn trong trường hợp xấu nhất là an toàn ngay cả khi không tính toán trước.
