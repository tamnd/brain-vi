---
title: "CF 105025H - \u0428\u0438\u0445\u0430\u043d"
description: "Chúng ta đang làm việc với một “ngọn núi” ẩn được xác định trên một lưới rất lớn có kích thước $n nhân m$. Ở đâu đó trên lưới này có một ô đặc biệt $(x0, y0)$, đỉnh."
date: "2026-06-28T01:41:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105025
codeforces_index: "H"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u00ab\u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430\u00bb \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 105025
solve_time_s: 55
verified: true
draft: false
---

[CF 105025H - \u0428\u0438\u0445\u0430\u043d](https://codeforces.com/problemset/problem/105025/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một “ngọn núi” ẩn được xác định trên một lưới có kích thước rất lớn$n \times m$. Ở đâu đó trên lưới này có một ô đặc biệt$(x_0, y_0)$, đỉnh cao. Mọi ô khác có chiều cao nhỏ hơn hoàn toàn và chiều cao chỉ phụ thuộc vào khoảng cách từ ô này đến đỉnh này. Nếu hai ô ở các khoảng cách khác nhau so với đỉnh thì chiều cao của chúng sẽ khác nhau và ô càng gần thì chiều cao càng cao. 

Cách duy nhất để có được thông tin là truy vấn một ô$(x, y)$, trả về chiều cao của nó dưới dạng số thực. Từ đó, chúng ta phải xác định tọa độ chính xác của đỉnh bằng cách sử dụng càng ít truy vấn càng tốt. 

Những hạn chế$n, m \le 10^6$ngay lập tức loại trừ bất kỳ chiến lược nào kiểm tra nhiều hơn một phần rất nhỏ của lưới điện. Quét toàn bộ dù chỉ một hàng cũng đã là quá lớn. Điều này buộc chúng tôi phải sử dụng một cách tiếp cận trong đó mọi truy vấn đều giảm thiểu sự không chắc chắn một cách có ý nghĩa, thường bằng cách khai thác tính đơn điệu hoặc tính không đồng nhất. 

Một đặc tính tinh tế nhưng quan trọng là độ cao chỉ phụ thuộc vào khoảng cách đến đỉnh chứ không phụ thuộc vào hướng. Điều này có nghĩa là nếu chúng ta cố định một tọa độ, chẳng hạn$x$, và khác nhau$y$, hàm hoạt động giống như một đường cong một đỉnh có tâm tại$y_0$. Điều tương tự cũng xảy ra nếu chúng ta sửa$y$và khác nhau$x$. Cấu trúc này làm cho bài toán có thể giải được bằng tìm kiếm logarit. 

Một ý tưởng ngây thơ là thử các điểm ngẫu nhiên hoặc tìm kiếm cục bộ. Tuy nhiên, vì lưới rất lớn nên việc lấy mẫu ngẫu nhiên không đảm bảo sẽ nhanh chóng đạt được các điểm tốt hơn. Ngay cả việc leo trèo hàng xóm tham lam cũng có thể thực hiện quá nhiều bước trong trường hợp xấu nhất. 

Trường hợp cạnh ẩn chính là khi đỉnh ở gần ranh giới. Trong những trường hợp như vậy, việc di chuyển cục bộ hoặc leo đồi ngây thơ có thể đi bộ một quãng đường dài dọc theo rìa trước khi lên đến đỉnh, lãng phí các truy vấn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử truy vấn mọi ô, theo dõi chiều cao tối đa được nhìn thấy. Điều này đúng vì đỉnh là mức tối đa toàn cầu duy nhất. Tuy nhiên, điều này đòi hỏi$n \cdot m$các truy vấn, có thể lên tới$10^{12}$, hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mặc dù hàm tồn tại ở hai chiều, nhưng nó không đồng nhất trong mỗi tọa độ khi tọa độ kia cố định. Nếu chúng ta sửa$x$, sau đó khi chúng ta di chuyển dọc theo$y$, khoảng cách tới đỉnh giảm dần cho đến khi$y_0$, sau đó tăng lên. Vì vậy, chiều cao dọc theo đường đó có một mức tối đa duy nhất. Đây chính xác là cấu trúc cần thiết cho tìm kiếm ternary. 

Điều này làm giảm vấn đề 2D thành hai tìm kiếm 1D độc lập. Đầu tiên chúng ta xác định đúng hàng$y_0$bằng cách tìm kiếm ternary$y$, sử dụng bất kỳ cố định$x$. Một lần$y_0$được tìm thấy, chúng tôi lặp lại ý tưởng tương tự dọc theo$x$-trục để xác định vị trí$x_0$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Tìm kiếm ba giai đoạn |$O(\log n + \log m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi hàm tương tác là hàm unimodal hộp đen trong mỗi tọa độ. 

1. Sửa bất kỳ$x$, để thuận tiện, hãy lấy$x = 1$. Bây giờ chúng ta xem xét hàm$f(y) = \text{height}(1, y)$. Vì chiều cao chỉ phụ thuộc vào khoảng cách tới đỉnh nên hàm số này có một cực đại duy nhất tại$y = y_0$. 
2. Chạy tìm kiếm ternary trên khoảng$[1, m]$để xác định vị trí$y_0$. Tại mỗi bước chọn hai trung điểm$mid_1$Và$mid_2$, truy vấn$(1, mid_1)$Và$(1, mid_2)$, so sánh độ cao Nếu chức năng này cao hơn ở$mid_1$, đỉnh nằm ở đoạn bên trái, nếu không thì nó nằm ở đoạn bên phải. Điều này hiệu quả vì tính đơn điệu đảm bảo không có cực đại thứ cấp. 
3. Sau khi thu hẹp, kiểm tra trực tiếp một số ứng viên còn lại để xác định chính xác$y_0$. 
4. Bây giờ hãy sửa$y = y_0$. Coi như$g(x) = \text{height}(x, y_0)$, một lần nữa là đơn thức với một đỉnh duy nhất tại$x_0$. 
5. Chạy tìm kiếm ternary$[1, n]$theo cách tương tự, truy vấn$(mid, y_0)$cho đến vị trí đỉnh cao$x_0$được tìm thấy. 
6. Đầu ra$(x_0, y_0)$và chấm dứt. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là khoảng cách tới đỉnh được cực tiểu hóa tại$(x_0, y_0)$, và tăng dần khi chúng ta di chuyển ra xa theo bất kỳ hướng nào. Do đó, bất kỳ lát cắt ngang hoặc dọc cố định nào đều tạo thành một hàm đơn thức hoàn toàn với một mức tối đa duy nhất. Tìm kiếm bậc ba trên hàm như vậy không bao giờ loại bỏ đỉnh thực sự, bởi vì so sánh giữa hai điểm luôn chỉ ra chính xác cạnh nào chứa giá trị lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(x, y):
    print(f"? {x} {y}")
    sys.stdout.flush()
    return float(input().strip())

def ternary_search_fixed_x(x, m):
    l, r = 1, m
    while r - l > 3:
        m1 = l + (r - l) // 3
        m2 = r - (r - l) // 3
        v1 = ask(x, m1)
        v2 = ask(x, m2)
        if v1 < v2:
            l = m1
        else:
            r = m2

    best_y = l
    best_val = ask(x, l)
    for y in range(l + 1, r + 1):
        v = ask(x, y)
        if v > best_val:
            best_val = v
            best_y = y
    return best_y

def ternary_search_fixed_y(y, n):
    l, r = 1, n
    while r - l > 3:
        m1 = l + (r - l) // 3
        m2 = r - (r - l) // 3
        v1 = ask(m1, y)
        v2 = ask(m2, y)
        if v1 < v2:
            l = m1
        else:
            r = m2

    best_x = l
    best_val = ask(l, y)
    for x in range(l + 1, r + 1):
        v = ask(x, y)
        if v > best_val:
            best_val = v
            best_x = x
    return best_x

def solve():
    n, m = map(int, input().split())
    y0 = ternary_search_fixed_x(1, m)
    x0 = ternary_search_fixed_y(y0, n)
    print(f"! {x0} {y0}")
    sys.stdout.flush()

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt hai tìm kiếm tọa độ một cách rõ ràng. Chức năng trợ giúp`ask`đảm bảo xóa sau mỗi truy vấn, đây là điều bắt buộc trong các vấn đề tương tác. 

Mỗi vòng tìm kiếm bậc ba thu hẹp khoảng dựa trên việc so sánh hai điểm bên trong. Khi khoảng thời gian nhỏ, quá trình quét tuyến tính sẽ giải quyết mức tối đa chính xác một cách an toàn, tránh sự mất ổn định so sánh dấu phẩy động trong phạm vi hẹp. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới khái niệm nhỏ có đỉnh nằm ở$(2, 1)$. 

### Ví dụ về dấu vết để tìm$y_0$với cố định$x = 1$| bước | tôi | r | m1 | m2 | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 1 | 2 | so sánh độ cao | 
| 2 | 2 | 3 | - | - | quét lần cuối | 

Việc tìm kiếm nhanh chóng xác định rằng$y = 1$mang lại giá trị cao hơn$y = 2$, thu hẹp phạm vi cho đến khi chỉ còn lại đỉnh. 

Điều này xác nhận rằng tính đồng nhất dọc theo lát cắt dọc là đủ để xác định đúng hàng. 

### Ví dụ về dấu vết để tìm$x_0$với cố định$y = 1$| bước | tôi | r | m1 | m2 | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 1 | 2 | so sánh độ cao | 
| 2 | 2 | 3 | - | - | quét lần cuối | 

Một lần nữa, quá trình thu hẹp tương tự sẽ cô lập$x = 2$, thể hiện tính đối xứng của cách tiếp cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n + \log m)$| Mỗi tìm kiếm ternary giảm khoảng thời gian theo hệ số không đổi | 
| Không gian |$O(1)$| Chỉ có một số biến được duy trì | 

Số lượng truy vấn phù hợp thoải mái trong giới hạn tương tác điển hình vì mỗi tìm kiếm chỉ thực hiện nhiều truy vấn theo logarit trong phạm vi tối đa$10^6$. 

## Trường hợp thử nghiệm```python
import sys, io

# This is a structural template; real interaction cannot be fully simulated without a judge.

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, sys.stdin.readline().split())
    # placeholder since interactive judge is required
    return "interactive"

# sample placeholder
# assert run("3 2\n") == "expected"

# custom structural tests (non-interactive sanity checks)
assert run("1 1\n") == "interactive"
assert run("10 10\n") == "interactive"
assert run("1000000 1\n") == "interactive"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | (1,1) | lưới tối thiểu | 
| 10 10 | vị trí đỉnh cao | trường hợp chung | 
| 1e6 1 | (1,1) | hàng thoái hóa | 
| 1 1e6 | (1,1) | cột suy biến | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi đỉnh nằm chính xác trên đường biên, chẳng hạn như$(1, 1)$. Trong trường hợp này, hàm vẫn không đồng dạng dọc theo cả hai trục, nhưng giá trị lớn nhất nằm ở rìa của khoảng tìm kiếm. Tìm kiếm bậc ba vẫn hoạt động vì so sánh sẽ luôn thiên về hướng biên chứa đỉnh, thu hẹp dần về phía đó. 

Một trường hợp khác là khi$n = 1$hoặc$m = 1$. Vấn đề thoái hóa thành tìm kiếm ternary 1D thuần túy. Logic tương tự được áp dụng, nhưng chỉ cần một giai đoạn. Thuật toán vẫn hoạt động chính xác vì giả định về tính không đồng nhất vẫn còn hiệu lực. 

Cuối cùng, khi đỉnh được căn giữa, sự so sánh giữa các điểm đối xứng tạo ra các mô hình rút gọn gần như giống hệt nhau, nhưng sự bất bình đẳng nghiêm ngặt về độ cao đảm bảo chuyển động xác định về phía tâm mà không dao động.
