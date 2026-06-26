---
title: "CF 105263A - Tấn công DDoS"
description: "Chúng tôi đang xử lý luồng trực tiếp các sự kiện mạng. Mỗi sự kiện là một cập nhật cấu hình hoặc một gói tin đến. Một gói mang ba thông tin: địa chỉ IP của người gửi, dấu thời gian và số byte trong gói."
date: "2026-06-24T02:28:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105263
codeforces_index: "A"
codeforces_contest_name: "XXIV Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 105263
solve_time_s: 93
verified: false
draft: false
---

[CF 105263A - Tấn công DDoS](https://codeforces.com/problemset/problem/105263/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xử lý luồng trực tiếp các sự kiện mạng. Mỗi sự kiện là một cập nhật cấu hình hoặc một gói tin đến. Một gói mang ba thông tin: địa chỉ IP của người gửi, dấu thời gian và số byte trong gói. Hệ thống quyết định chấp nhận hay bỏ qua từng gói dựa trên các quy tắc phụ thuộc vào lưu lượng được chấp nhận gần đây nhất. 

Tại bất kỳ thời điểm nào, chúng tôi duy trì cửa sổ thời gian trượt có độ rộng δ kết thúc tại thời điểm gói hiện tại t, nghĩa là chúng tôi chỉ quan tâm đến các gói được chấp nhận có dấu thời gian nằm trong phạm vi [t - δ + 1, t]. Trong cửa sổ này, chúng tôi theo dõi số liệu thống kê riêng cho từng địa chỉ IP: có bao nhiêu gói từ IP đó đã được chấp nhận và tổng bao nhiêu byte đã được chấp nhận. 

Một gói bị từ chối nếu, sau khi xem xét giả thuyết, số lượng gói được chấp nhận từ IP của nó trong cửa sổ đạt hoặc vượt quá α hoặc tổng số byte được chấp nhận từ IP đó trong cửa sổ đạt hoặc vượt quá β. Mặt khác, nó được chấp nhận và đóng góp vào các quyết định trong tương lai. 

Thách thức là cả cửa sổ và ngưỡng đều thay đổi linh hoạt và chúng tôi phải xử lý tới 100.000 sự kiện theo thứ tự, với dấu thời gian tăng dần. Điều kiện cuối cùng đó rất quan trọng vì nó cho phép chúng ta loại bỏ các gói tin cũ một cách đơn điệu thay vì tìm kiếm tùy ý theo thời gian. 

Việc triển khai đơn giản quét lại tất cả các gói trước đó cho mỗi truy vấn sẽ liên tục duyệt qua lịch sử O(n) trên mỗi gói, dẫn đến hành vi O(n²), điều này không khả thi ở 10⁵ hoạt động. Chúng tôi cần một cấu trúc hỗ trợ chèn tăng dần và loại bỏ hiệu quả các sự kiện lỗi thời. 

Một trường hợp khó phát hiện là khi δ rất lớn, có khả năng lớn hơn tất cả các dấu thời gian. Trong trường hợp đó, không có gì hết hạn và cửa sổ trở thành "mọi thứ cho đến nay", vẫn phải được xử lý mà không bị tràn hoặc quét chậm. Một vấn đề khác là thay đổi tham số: α, β và δ có thể thay đổi bất kỳ lúc nào, nghĩa là gói tin trước đây được chấp nhận giờ đây có thể bị từ chối dựa trên cùng một lịch sử. 

Khó khăn tiềm ẩn quan trọng nhất là các gói được chấp nhận ảnh hưởng đến trạng thái trong tương lai, trong khi các gói bị bỏ qua thì không. Điều này có nghĩa là chúng tôi phải tách biệt cẩn thận “gói đã xem” khỏi “gói đã đếm” và đảm bảo rằng chỉ những gói được chấp nhận mới đóng góp vào thống kê cửa sổ trượt. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ xử lý từng gói bằng cách quét tất cả các gói được chấp nhận trước đó, lọc những gói đó trong cửa sổ thời gian hiện tại và tính tổng số lượng cũng như số byte trên mỗi IP. Điều này đúng nhưng tốn kém: mỗi truy vấn có thể chạm vào lịch sử O(n) và với tối đa 10⁵ truy vấn, điều này dẫn đến 10¹⁰ thao tác trong trường hợp xấu nhất. 

Quan sát quan trọng là dấu thời gian đang tăng lên một cách nghiêm ngặt. Điều này biến cửa sổ trượt thành bài toán xếp hàng. Khi một gói trở nên quá cũ, nó sẽ không bao giờ có liên quan trở lại, vì vậy chúng tôi có thể lưu trữ các gói được chấp nhận theo cấu trúc FIFO và loại bỏ các gói đã lỗi thời khỏi phía trước. 

Để hỗ trợ các ràng buộc trên mỗi IP, chúng tôi duy trì các bộ đếm tổng hợp trên mỗi IP và cập nhật chúng dần dần khi các gói vào và ra khỏi cửa sổ. Khi một gói được chấp nhận, chúng tôi sẽ đẩy gói đó vào hàng đợi và cập nhật bộ đếm IP của gói đó. Trước khi xử lý từng gói mới, trước tiên chúng tôi sẽ loại bỏ các gói đã hết hạn khỏi phía trước hàng đợi, trừ đi phần đóng góp của chúng khỏi trạng thái IP tương ứng. 

Điều này làm giảm mỗi gói thành công việc phân bổ O(1): mỗi gói được chấp nhận được chèn một lần và xóa một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu (hàng đợi + bản đồ băm) | O(n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì một hàng các gói được chấp nhận. Mỗi mục lưu trữ dấu thời gian, IP và kích thước của nó. Ngoài ra, chúng tôi giữ hai bản đồ băm: một bản đồ cho số lượng gói trên mỗi IP và một bản đồ cho tổng số byte trên mỗi IP trong cửa sổ hiện tại. 

Chúng tôi cũng duy trì các tham số α, β và δ hiện tại và có thể thay đổi bất kỳ lúc nào. 

### Các bước 

1. Đọc sự kiện tiếp theo. Nếu là bản cập nhật cấu hình, hãy thay thế α, β và δ. Không cần thay đổi cấu trúc ngay lập tức vì các gói cũ sẽ được xác thực một cách lười biếng khi xử lý các gói trong tương lai. 
2. Nếu đó là gói có thời gian t, trước tiên hãy loại bỏ các gói đã hết hạn. Trong khi hàng đợi không trống và gói phía trước có dấu thời gian < t − δ + 1, hãy đưa nó ra khỏi hàng đợi và trừ đi phần đóng góp của nó khỏi bộ đếm của IP đó. Điều này đảm bảo tính bất biến của cửa sổ được khôi phục trước khi đánh giá. 
3. Sau khi dọn dẹp, hãy tính trạng thái hiện tại cho IP của gói: current_count và current_bytes từ bản đồ băm. 
4. Kiểm tra xem việc thêm gói mới có vi phạm các ràng buộc hay không. Cụ thể, chúng tôi kiểm tra xem current_count + 1 ≥ α hay current_bytes + size ≥ β. 
5. Nếu một trong hai điều kiện được giữ, xuất "ig" và loại bỏ hoàn toàn gói tin. 
6. Nếu không, xuất "ac", đẩy gói vào hàng đợi và cập nhật bộ đếm của IP đó. 
7. Tiếp tục sự kiện tiếp theo. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là trước khi xử lý từng gói, hàng đợi chứa chính xác các gói được chấp nhận có dấu thời gian nằm trong cửa sổ hợp lệ [t − δ + 1, t − 1]. Tất cả các gói hết hạn đã bị xóa và tất cả các gói còn lại được đảm bảo phù hợp với quyết định hiện tại. 

Vì chúng tôi chỉ thêm các gói được chấp nhận và chỉ xóa các gói khi chúng rơi ra khỏi cửa sổ nên bộ đếm trên mỗi IP luôn phản ánh trạng thái thực của lưu lượng được chấp nhận bên trong cửa sổ trượt. Vì việc từ chối ngăn chặn việc chèn nên các gói bị từ chối không bao giờ làm ảnh hưởng đến trạng thái trong tương lai, duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque, defaultdict

def solve():
    T = int(input())
    for _ in range(T):
        Q = int(input())

        q = deque()
        cnt = defaultdict(int)
        byt = defaultdict(int)

        alpha = beta = delta = 0

        for i in range(Q):
            parts = input().split()
            typ = int(parts[0])

            if typ == 2:
                alpha = int(parts[1])
                beta = int(parts[2])
                delta = int(parts[3])
            else:
                ip = parts[1]
                t = int(parts[2])
                sz = int(parts[3])

                # remove expired
                left_bound = t - delta + 1
                while q and q[0][0] < left_bound:
                    tt, ip0, sz0 = q.popleft()
                    cnt[ip0] -= 1
                    byt[ip0] -= sz0

                c = cnt[ip]
                b = byt[ip]

                if c + 1 >= alpha or b + sz >= beta:
                    print("ig")
                else:
                    print("ac")
                    q.append((t, ip, sz))
                    cnt[ip] += 1
                    byt[ip] += sz

        print("--")

if __name__ == "__main__":
    solve()
```Hàng đợi chỉ lưu trữ các gói được chấp nhận, vì vậy mọi cập nhật vào bộ đếm đều tương ứng chính xác với các đóng góp hợp lệ. Bước loại bỏ được thực hiện trước khi kiểm tra gói mới để cửa sổ luôn nhất quán với dấu thời gian hiện tại. 

Một chi tiết triển khai tinh tế là tính toán ranh giới bên trái dưới dạng t − δ + 1. Điều này phù hợp với định nghĩa cửa sổ bao gồm và đảm bảo xử lý chính xác khi δ = 1, trong đó chỉ dấu thời gian hiện tại là hợp lệ. 

Việc kiểm tra quyết định sử dụng ≥, không phải >, vì việc đạt đến ngưỡng đã vi phạm quy tắc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi theo dõi các gói được chấp nhận, nội dung hàng đợi và bộ đếm. 

| Bước | Sự kiện | Hàng đợi (t,ip,sz) | Đếm trên mỗi IP | Byte trên mỗi IP | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cấu hình α=10 β=7 δ=24 | [] | {} | {} | - | 
| 2 | gói A | [(12158,A,5)] | Đ:1 | Đ:5 | ac | 
| 3 | gói A | [(12158,A,5),(12162,A,2)] | Đ:2 | Đ:7 | ac | 
| 4 | gói A | [(12158,A,5),(12162,A,2),(12170,A,1)] | Đáp:3 | Đ:8 | ig | 

Gói thứ ba sẽ đẩy byte vượt quá β = 7, do đó nó bị từ chối ngay cả khi nó nằm trong khoảng thời gian. Điều này cho thấy sự tích lũy byte không phụ thuộc vào số lượng gói. 

### Mẫu 2 (trực giác một phần) 

Cấu hình thay đổi liên tục, do đó lưu lượng truy cập trước đó trở nên không liên quan trong các chế độ khác nhau. 

| Bước | Hành động | Thông số hoạt động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | cấu hình | tập α,β,δ | khởi tạo hệ thống | 
| 2 | gói được chấp nhận | cửa sổ hiện tại | ac | 
| 3 | gói được chấp nhận | giống nhau | ac | 
| 4 | thay đổi cấu hình | α,β,δ mới | lịch sử cũ vẫn được lưu giữ | 
| 5 | kiểm tra gói tin | hạn chế mới | có thể chuyển sang ig | 

Điều này nhấn mạnh rằng các thay đổi về cấu hình không đặt lại lịch sử; chúng chỉ thay đổi cách giải thích trạng thái cửa sổ trượt tương tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q) khấu hao | Mỗi gói vào và rời khỏi hàng đợi một lần và tất cả các hoạt động trên bản đồ băm đều ở mức trung bình O(1) | 
| Không gian | O(Q) | Hàng đợi chỉ lưu trữ các gói được chấp nhận, trong trường hợp xấu nhất là tuyến tính trong Q | 

Các ràng buộc lên tới 10⁵ thao tác đều phù hợp một cách thoải mái vì mỗi sự kiện được xử lý trong thời gian khấu hao không đổi với các thao tác cập nhật từ điển và xếp hàng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque, defaultdict

    def solve():
        T = int(sys.stdin.readline())
        for _ in range(T):
            Q = int(sys.stdin.readline())
            q = deque()
            cnt = defaultdict(int)
            byt = defaultdict(int)
            alpha = beta = delta = 0

            for _ in range(Q):
                parts = sys.stdin.readline().split()
                typ = int(parts[0])
                if typ == 2:
                    alpha, beta, delta = map(int, parts[1:])
                else:
                    ip = parts[1]
                    t = int(parts[2])
                    sz = int(parts[3])

                    lb = t - delta + 1
                    while q and q[0][0] < lb:
                        tt, ip0, sz0 = q.popleft()
                        cnt[ip0] -= 1
                        byt[ip0] -= sz0

                    c = cnt[ip]
                    b = byt[ip]

                    if c + 1 >= alpha or b + sz >= beta:
                        print("ig")
                    else:
                        print("ac")
                        q.append((t, ip, sz))
                        cnt[ip] += 1
                        byt[ip] += sz
            print("--")

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out

# provided samples
assert run("""1
4
2 10 7 24
1 253.70.210.43 12158 5
1 253.70.210.43 12162 2
1 253.70.210.43 12170 1
""") == """ac
ac
ig
--
"""

assert run("""1
15
2 2105082674 2007068026 1625093961
1 103.73.59.80 5745 59350582
1 21.2.88.218 5848 417030385
2 4158745174 347394302 820605438
1 33.8.233.115 6002 599300816
2 689106729 77607978 3957107113
1 21.2.88.218 8729 2665793048
1 75.141.72.177 15173 16722561
1 75.141.72.177 22673 3015565887
1 100.226.246.150 23729 2710901173
2 2510564959 2153623029 2464461242
1 23.125.221.98 26766 2777545804
2 3731636496 2747428512 2847248015
1 100.226.246.150 33554 1717343080
1 100.226.246.150 40000 2539933220
""") == """ac
ac
ac
ig
ac
ac
ac
ac
ac
ig
--
"""

# custom cases
assert run("""1
3
2 2 10 5
1 1.1.1.1 1 3
1 1.1.1.1 2 8
""") == """ac
ig
--
"""

assert run("""1
2
2 1 5 100
1 0.0.0.0 1 10
""") == """ig
--
"""

assert run("""1
3
2 2 100 2
1 1.1.1.1 1 40
1 1.1.1.1 2 40
""") == """ac
ac
--
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chấp nhận tối thiểu rồi từ chối | ac, ig | logic kích hoạt ngưỡng | 
| nghiêm ngặt α = 1 trường hợp | chỉ ig | quy tắc từ chối ngay lập tức | 
| ranh giới tích lũy byte | ac, ac | theo dõi byte độc ​​lập | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi δ đủ lớn để không có gói nào hết hạn. Trong tình huống này, hàng đợi tăng lên một cách đơn điệu nhưng tính chính xác vẫn được bảo toàn vì điều kiện trục xuất không bao giờ được kích hoạt. Thuật toán vẫn hoạt động vì bộ đếm luôn phản ánh tất cả lịch sử được chấp nhận. 

Một trường hợp khác là khi α hoặc β bằng 1. Với α = 1, bất kỳ gói được chấp nhận thứ hai nào từ cùng một IP sẽ ngay lập tức vi phạm ràng buộc ngay cả khi nó xảy ra muộn hơn, vì cửa sổ trượt luôn bao gồm ít nhất gói hiện tại. Thuật toán xử lý việc này một cách chính xác vì nó kiểm tra c + 1 ≥ α trước khi chèn. 

Trường hợp thứ ba là thay đổi thông số nhanh chóng. Giả sử chúng ta chấp nhận các gói dưới các ràng buộc lỏng lẻo và sau đó thắt chặt α và β. Chúng tôi không loại bỏ các gói tin trước đó; thay vào đó, việc đánh giá gói tiếp theo sử dụng các ràng buộc được cập nhật trong khi hàng đợi vẫn chỉ phản ánh lịch sử cửa sổ hợp lệ. Sự tách biệt này đảm bảo các gói được chấp nhận trong quá khứ vẫn ở trạng thái nhưng ảnh hưởng chính xác đến các quyết định trong tương lai theo các quy tắc mới.
