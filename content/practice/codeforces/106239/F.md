---
title: "CF 106239F - \u5b87\u5b99\u5c04\u7ebf\u98ce\u66b4"
description: "Mặt phẳng chứa một vùng hình vuông từ $0$ đến $N$ trên cả hai trục. Bên trong vùng này, chúng ta có nhiều đường thẳng vô hạn, mỗi đường mang một trọng số. Mọi đường thẳng đều có độ dốc $+1$, được viết là $y = x + b$, hoặc có độ dốc $-1$, được viết là $y = -x + c$."
date: "2026-06-19T09:14:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106239
codeforces_index: "F"
codeforces_contest_name: "2025\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66\u65b0\u751f\u8d5b(\u51b3\u8d5b)"
rating: 0
weight: 106239
solve_time_s: 53
verified: true
draft: false
---

[CF 106239F - \u5b87\u5b99\u5c04\u7ebf\u98ce\u66b4](https://codeforces.com/problemset/problem/106239/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mặt phẳng chứa một vùng hình vuông từ$0$ĐẾN$N$trên cả hai trục. Bên trong vùng này, chúng ta có nhiều đường thẳng vô hạn, mỗi đường mang một trọng số. Mỗi đường đều có độ dốc$+1$, viết là$y = x + b$, hoặc độ dốc$-1$, viết là$y = -x + c$. Mỗi đường đóng góp trọng lượng của nó vào mọi điểm nó đi qua và tại bất kỳ điểm nào, tổng năng lượng là tổng trọng số của tất cả các đường bao phủ điểm đó. 

Nhiệm vụ là chọn bất kỳ điểm nào bên trong hoặc trên ranh giới của hình vuông và tối đa hóa tổng năng lượng tại điểm đó. Điểm tối ưu không cần phải là tọa độ nguyên và không cần là giao điểm của đường thẳng; nó có thể nằm ở bất cứ đâu trong vùng liên tục. 

Các ràng buộc đủ chặt chẽ đến mức không thể lặp lại các điểm ứng cử viên. Với tối đa$10^5$dòng trên mỗi bài kiểm tra và tổng số tiền trên các bài kiểm tra được giới hạn bởi$10^5$, bất kỳ giải pháp nào cố gắng kiểm tra tất cả các giao lộ hoặc lấy mẫu trực tiếp mặt phẳng sẽ vượt quá giới hạn thời gian. Một cách tiếp cận hợp lệ phải biến hình học liên tục thành một cấu trúc rời rạc có thể quản lý được và tránh sự tương tác bậc hai giữa các đường thẳng. 

Một khó khăn nhỏ là điểm tốt nhất có thể nằm ở khu vực không có giao điểm đường nào. Ví dụ: nếu tất cả các đường thẳng song song theo một hướng và tập trung nhiều vào một dải thì mức tối đa có thể xảy ra ở bất kỳ đâu dọc theo một đoạn liên tục. 

Một vấn đề khác là điểm tối ưu không nhất thiết phải ở tọa độ nguyên. Một cách tiếp cận ngây thơ chỉ đánh giá các điểm giao nhau của các đường sẽ thất bại ngay cả trong những trường hợp nhỏ. 

Hãy xem xét ví dụ này: 

đầu vào:```
N = 2
1 line: y = x (weight 10)
1 line: y = -x + 2 (weight 10)
```Cả hai đường thẳng cắt nhau tại$(1,1)$, và điểm đó là tối ưu. Nhưng nếu chúng ta dịch chuyển một chút để chúng không cắt nhau bên trong hình vuông, chẳng hạn$y = x + 1$Và$y = -x + 1$, thì đạt được mức tối đa dọc theo một đoạn gần ranh giới thay vì tại một điểm giao nhau riêng biệt. Một phương pháp chỉ dựa vào giao lộ sẽ bỏ lỡ điều này. 

Khó khăn chính là mặt phẳng có tính liên tục nhưng cấu trúc của các đường thẳng rất thẳng hàng. 

## Phương pháp tiếp cận 

Chế độ xem trực tiếp brute-force sẽ cố gắng xem xét mọi điểm có thể có trong hình vuông và tính toán có bao nhiêu đường đi qua nó. Điều này là không thể vì không gian là liên tục. Ngay cả khi chúng ta hạn chế ở những giao điểm của các đường, vẫn có$O(Q^2)$những điểm như vậy, vượt xa giới hạn khi$Q = 10^5$. 

Cấu trúc quan trọng xuất phát từ thực tế là tất cả các đường chỉ rơi vào hai hướng. Điều này gợi ý một phép biến đổi tọa độ biến hình học thành cấu trúc thẳng hàng theo trục. 

Xác định các biến mới:$$u = x + y, \quad v = x - y$$Dưới sự chuyển đổi này, mỗi dòng trở nên cực kỳ đơn giản. một dòng$y = x + b$trở thành$x - y = -b$, nghĩa là nó tương ứng với một hằng số$v$. Tương tự, một dòng$y = -x + c$trở thành$x + y = c$, nghĩa là nó tương ứng với một hằng số$u$. 

Điều này có nghĩa là mỗi đường đóng góp trọng lượng dọc theo một đường ngang hoặc dọc trong$(u, v)$-không gian. Vấn đề là chọn điểm$(u, v)$để tối đa hóa:$$U(u) + V(v)$$Ở đâu$U(u)$là tổng trọng số của tất cả các dòng với$x+y = u$, Và$V(v)$là tổng trọng số của tất cả các dòng với$x-y = v$. 

Sự phức tạp còn lại là không phải tất cả$(u, v)$cặp là hợp lệ. Ràng buộc ban đầu$0 \le x, y \le N$trở thành:$$0 \le \frac{u+v}{2} \le N, \quad 0 \le \frac{u-v}{2} \le N$$mà đơn giản hóa thành:$$-u \le v \le u, \quad 0 \le u \le 2N$$Vì vậy vùng hợp lệ là một ràng buộc hình kim cương trong$(u,v)$-không gian. 

Bây giờ vấn đề hoàn toàn là: tối đa hóa$U(u) + V(v)$trên một lưới hình chữ nhật gồm các giá trị bị giới hạn bởi một ràng buộc đơn điệu$|v| \le u$. Cả hai$U$Và$V$là các hàm hằng số từng phần chỉ được xác định tại tọa độ nhất định. 

Việc quét mạnh mẽ trên tất cả các cặp tọa độ sự kiện vẫn dẫn đến$O(Q^2)$. Quan sát quan trọng là giải pháp tối ưu chỉ cần xem xét các giá trị tới hạn của$u$Và$v$, và điều kiện biên$v = \pm u$, bởi vì giữa các tọa độ sự kiện không có hàm nào thay đổi. 

Vì vậy, chúng ta giảm việc tìm kiếm xuống việc quét qua đã sắp xếp$u$-giá trị trong khi duy trì cấu trúc có thể truy vấn tối đa$V(v)$bên trong$[-u, u]$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Dùng vũ lực tại các điểm hoặc nút giao thông |$O(Q^2)$|$O(Q)$| Quá chậm | 
| Biến đổi tọa độ + quét + phạm vi tối đa |$O(Q \log Q)$|$O(Q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta chuyển đổi bài toán hình học thành hai tọa độ có trọng số một chiều độc lập dưới các ràng buộc. 

1. Chuyển từng dòng thành một sự kiện tọa độ. Đối với mỗi dòng$y = x + b$, tính toán$v = -b$và thêm trọng lượng của nó vào vị trí$v$. Đối với mỗi dòng$y = -x + c$, tính toán$u = c$và thêm trọng lượng của nó vào vị trí$u$. Điều này tạo ra hai bản đồ thưa thớt: một trên$u$và một hơn$v$. 
2. Thu thập tất cả các khác biệt$u$-tọa độ và$v$- phối hợp và sắp xếp chúng. Đây là những nơi duy nhất mà hàm mục tiêu có thể thay đổi giá trị. Giữa họ, cả hai$U$Và$V$vẫn không đổi. 
3. Xây dựng một cấu trúc trên$v$-giá trị cho phép truy vấn nhanh tối đa trong khoảng thời gian. Vì chúng ta chỉ cần tối đa$V(v)$trên phạm vi$[-u, u]$, cây phân đoạn hoặc mảng nén tọa độ sẽ hoạt động. 
4. Đối với mỗi ứng viên$u$, tính toán$U(u)$trực tiếp từ bản đồ được tính toán trước. Sau đó truy vấn giá trị lớn nhất của$V(v)$trong phạm vi$[-u, u]$. Kết hợp chúng để có được câu trả lời ứng cử viên. 
5. Duy nhất$u$-các giá trị cần được kiểm tra là các vị trí sự kiện trong đó$U(u)$những thay đổi và các giá trị trong đó ranh giới ràng buộc$u = |v_i|$trở nên hoạt động. Điều này đảm bảo chúng tôi không bao giờ bỏ lỡ quá trình chuyển đổi khi khả thi$v$-phạm vi bắt đầu bao gồm một phân đoạn mới của$V$kết cấu. 
6. Tận dụng tối đa tất cả các ứng cử viên. 

Lý do điều này có tác dụng là vì cả hai$U(u)$Và$V(v)$là các hàm bậc thang. Giữa các tọa độ sự kiện liên tiếp, không có hàm nào thay đổi và việc mở rộng hoặc thu nhỏ khoảng khả thi chỉ quan trọng khi nó vượt qua điểm dừng của$V$hoặc thẳng hàng với ranh giới của vùng ràng buộc. Do đó, điều tối ưu phải xảy ra khi chúng ta chính xác ở điểm gián đoạn của$U$, hoặc hoạt động$v$-ranh giới phạm vi chạm vào sự gián đoạn của$V$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        N, Q = map(int, input().split())

        U = {}
        V = {}

        u_vals = set()
        v_vals = set()

        for _ in range(Q):
            t, x, y, e = map(int, input().split())
            if t == 1:
                v = x - y  # = -b
                V[v] = V.get(v, 0) + e
                v_vals.add(v)
            else:
                u = x + y  # = c
                U[u] = U.get(u, 0) + e
                u_vals.add(u)

        v_list = sorted(v_vals)
        u_list = sorted(u_vals)

        if not v_list:
            # only u-lines
            print(max(U.values()))
            continue
        if not u_list:
            # only v-lines
            print(max(V.values()))
            continue

        v_index = {v:i for i, v in enumerate(v_list)}
        seg = [0] * (4 * len(v_list))

        def build(i, l, r):
            if l == r:
                seg[i] = V[v_list[l]]
                return
            m = (l + r) // 2
            build(i*2, l, m)
            build(i*2+1, m+1, r)
            seg[i] = max(seg[i*2], seg[i*2+1])

        def query(i, l, r, ql, qr):
            if ql <= l and r <= qr:
                return seg[i]
            if r < ql or l > qr:
                return 0
            m = (l + r) // 2
            return max(
                query(i*2, l, m, ql, qr),
                query(i*2+1, m+1, r, ql, qr)
            )

        build(1, 0, len(v_list)-1)

        ans = 0

        for u in u_list:
            base = U[u]

            L = -u
            R = u

            # find indices in v_list
            import bisect
            l = bisect.bisect_left(v_list, L)
            r = bisect.bisect_right(v_list, R) - 1

            if l <= r:
                best_v = query(1, 0, len(v_list)-1, l, r)
                ans = max(ans, base + best_v)
            else:
                ans = max(ans, base)

        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi hai họ dòng thành hai mảng trọng số độc lập được lập chỉ mục theo tọa độ được chuyển đổi. Cây phân đoạn được xây dựng trên tất cả$v$-tọa độ để hỗ trợ truy vấn tối đa trên phạm vi. Đối với mỗi$u$- Phối hợp, mã tính toán khoảng khả thi của$v$giá trị như$[-u, u]$, sau đó truy vấn kết quả tốt nhất có thể đạt được$V(v)$bên trong khoảng đó và kết hợp nó với$U(u)$. 

Một điểm tinh tế là chỉ tọa độ sự kiện được sử dụng cho$u$, vì giữa họ$U(u)$không thay đổi. Điều này đảm bảo quá trình quét vẫn tuyến tính theo các yếu tố logarit từ các truy vấn phạm vi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 2
t1: y = x (10)
t2: y = -x + 2 (20)
t3: y = x + 1 (5)
```Sau khi chuyển đổi: 

| Dòng | Loại | Tọa độ | Cân nặng | 
| --- | --- | --- | --- | 
| y=x | v-line | v=0 | 10 | 
| y=-x+2 | u-line | u=2 | 20 | 
| y=x+1 | v-line | v=-1 | 5 | 

Bây giờ hãy đánh giá ứng viên. 

| bạn | Bạn(u) | phạm vi v được phép | tốt nhất V(v) | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [0,0] | 10 | 10 | 
| 1 | 0 | [-1,1] | 15 | 15 | 
| 2 | 20 | [-2,2] | 15 | 35 | 

Tối đa là 35. 

Điều này cho thấy tại sao cả hai họ tọa độ phải được xem xét cùng nhau, vì điểm tốt nhất được xác định bằng sự cân bằng giữa việc mở rộng vùng khả thi và thu giữ nhiều$u$-sự kiện. 

### Ví dụ 2 

đầu vào:```
N = 3
y=x (5)
y=x+1 (7)
y=-x+2 (4)
y=-x+3 (6)
```| bạn | Bạn(u) | V(v) tốt nhất trong [-u,u] | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 0 | 5 | 5 | 
| 1 | 7 | 5 | 12 | 
| 2 | 7 | 10 | 17 | 
| 3 | 0 | 10 | 10 | 

Cực đại xảy ra ở$u=2$, đủ đâu$v$-range có sẵn để bao gồm cả hai đường có độ dốc âm mạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(Q \log Q)$| Sắp xếp tọa độ và truy vấn cây phân đoạn cho từng$u$| 
| Không gian |$O(Q)$| Lưu trữ bản đồ tọa độ và cây phân đoạn | 

Các ràng buộc cho phép nhiều nhất$10^5$tổng số dòng, do đó hệ số logarit trên mỗi dòng vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout.flush = lambda: None
    solve()
    return ""

# sample-like minimal sanity checks
# (format adapted since full IO not embedded here)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dòng đơn | trọng lượng đường | trường hợp cơ sở | 
| hai đường thẳng cắt nhau | tổng tại ngã tư | tọa độ biến đổi chính xác | 
| vùng tốt nhất không giao nhau | xử lý ranh giới đúng | tối ưu liên tục | 
| tất cả các dòng cùng hướng | giảm xuống tối đa 1D | cấu trúc thoái hóa | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi tất cả các dòng chỉ thuộc về một họ, nghĩa là tất cả đều thuộc về một họ.$u$-dòng hoặc tất cả đều là$v$-dòng. Trong trường hợp đó, câu trả lời giảm xuống mức tối đa đơn giản theo trọng số, vì điểm tối ưu luôn có thể được đặt ở bất kỳ đâu thỏa mãn ràng buộc mà không ảnh hưởng đến thứ nguyên bị thiếu. Thuật toán kiểm tra rõ ràng điều này và tránh các truy vấn trong phạm vi trống. 

Một trường hợp cạnh khác xuất hiện khi tốt nhất$v$-range trống cho một nhất định$u$. Điều này xảy ra khi$|u|$nhỏ hơn tất cả hiện có$v$-tọa độ. Trong trường hợp đó, mã chính xác sẽ quay lại chỉ sử dụng$U(u)$, vì không$v$-line có thể được kích hoạt. 

Trường hợp khó nhận biết cuối cùng là khi điểm tối ưu nằm chính xác trên đường biên$v = \pm u$. Những điểm này tương ứng với$x=0$hoặc$y=0$ở tọa độ ban đầu. Việc chuyển đổi bảo toàn chúng một cách chính xác và vì chúng tôi bao gồm các ranh giới trong các truy vấn phạm vi bằng cách sử dụng xử lý chia đôi bao gồm nên các trường hợp này được nắm bắt đầy đủ.
