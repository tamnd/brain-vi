---
title: "CF 102939D - Robot tung"
description: "Hai robot đứng ở hai điểm lưới cố định trên một lưới. Họ ném qua lại một quả bóng và quả bóng di chuyển dọc theo đoạn thẳng nối các vị trí của họ. Điểm thứ ba, Eve, đang cố gắng chặn bóng."
date: "2026-07-04T07:46:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102939
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 01-22-21 Div. 2 (Beginner)"
rating: 0
weight: 102939
solve_time_s: 43
verified: true
draft: false
---

[CF 102939D - Ném Robot](https://codeforces.com/problemset/problem/102939/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hai robot đứng ở hai điểm lưới cố định trên một lưới. Họ ném qua lại một quả bóng và quả bóng di chuyển dọc theo đoạn thẳng nối các vị trí của họ. Điểm thứ ba, Eve, đang cố gắng chặn bóng. Eve thành công chính xác khi đoạn thẳng từ Alice đến Bob đi qua vị trí của cô ấy. 

Đối với mỗi vị trí ứng cử viên của Eve, chúng ta phải quyết định xem điểm đó có nằm trên đoạn thẳng giữa hai robot hay không. Đây là một bài kiểm tra thành viên hình học thuần túy: một điểm có nằm chính xác trên một đoạn nhất định trong lưới số nguyên hay không. 

Đầu vào bao gồm hai điểm cố định và sau đó lên tới 100 điểm truy vấn. Mỗi truy vấn độc lập và hỏi xem điểm đó có nằm trên đoạn thẳng hay không. Tọa độ là các số nguyên nằm trong khoảng từ âm mười nghìn đến mười nghìn, đủ nhỏ để bất kỳ số học thời gian không đổi nào cho mỗi truy vấn đều đủ. 

Hàm ý ràng buộc chính là chúng ta có thể đủ khả năng kiểm tra hình học trực tiếp cho mỗi truy vấn mà không cần xử lý trước. Ngay cả cách tiếp cận O(E) cho mỗi truy vấn cũng sẽ quá chậm, nhưng ở đây E tối đa là 100, do đó, kiểm tra O(1) cho mỗi truy vấn là đủ. 

Một sai lầm phổ biến là chỉ kiểm tra tính cộng tuyến. Điều đó là không đủ. Ví dụ: nếu Alice ở (0, 0) và Bob ở (2, 2), thì điểm (3, 3) thẳng hàng với họ nhưng rõ ràng nằm ngoài Bob và không được tính là chặn đoạn thẳng. Một trường hợp khó phát hiện khác là khi Eve trùng với Alice hoặc Bob. Trong trường hợp đó, câu trả lời vẫn phải là tích cực vì đoạn đó đi qua điểm cuối đó. 

## Phương pháp tiếp cận 

Ý tưởng Brute Force là mô hình hóa đoạn đường một cách rõ ràng và kiểm tra xem một điểm có nằm trên đó hay không. Một cách đơn giản là tham số hóa phân đoạn và kiểm tra xem có tồn tại tham số t trong [0, 1] sao cho điểm bằng Alice cộng với t nhân vectơ từ Alice đến Bob hay không. Điều này làm giảm việc giải hai phương trình và kiểm tra tính nhất quán. Mặc dù đúng nhưng nó gây ra các vấn đề về độ chính xác của dấu phẩy động nếu được triển khai trực tiếp và không cần thiết do tính chất nguyên của tọa độ. 

Một quan sát chắc chắn hơn là một điểm nằm trên đoạn thẳng khi và chỉ khi có hai điều kiện thỏa mãn. Đầu tiên, ba điểm phải thẳng hàng, có thể kiểm tra bằng tích chéo của vectơ AB và AE. Thứ hai, điểm phải nằm trong hộp giới hạn do Alice và Bob tạo, nghĩa là tọa độ x của nó nằm giữa hai tọa độ x và tọa độ y của nó nằm giữa hai tọa độ y. 

Sự đơn giản hóa chính là tính cộng tuyến đảm bảo điểm nằm trên đường vô hạn, trong khi ràng buộc hộp giới hạn giới hạn nó trong đoạn hữu hạn. Các điều kiện này cùng nhau mô tả đầy đủ tư cách thành viên trên một phân đoạn trong hình học số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Giải đường tham số bằng float | O(E) | O(1) | Rủi ro do độ chính xác | 
| Sản phẩm chéo + kiểm tra hộp giới hạn | O(E) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi Alice là điểm A, Bob là điểm B và mỗi truy vấn là điểm C. 

1. Tính vectơ từ A đến B bằng cách trừ tọa độ, tạo thành (dx, dy). Điều này xác định hướng của phân khúc. 
2. Với mỗi điểm truy vấn C, tính vectơ từ A đến C, tạo thành (cx, cy). 
3. Kiểm tra tính cộng tuyến bằng cách đánh giá tích chéo dx * cy − dy * cx. Nếu giá trị này không bằng 0 thì C không nằm trên đường thẳng qua A và B. 
4. Nếu thẳng hàng, kiểm tra xem C có nằm trong hộp giới hạn thẳng hàng theo trục của A và B hay không bằng cách kiểm tra min(xa, xb) ≤ xc ≤ max(xa, xb) và tương tự với y. 
5. Nếu cả hai điều kiện đều giữ nguyên thì xuất Yes, nếu không thì xuất No. 

Thử nghiệm sản phẩm chéo thay thế so sánh độ dốc và tránh hoàn toàn sự phân chia, giúp loại bỏ các vấn đề về độ chính xác và xử lý các đường thẳng đứng một cách tự nhiên. 

### Tại sao nó hoạt động

Điều kiện tích chéo đảm bảo rằng các vectơ AB và AC phụ thuộc tuyến tính, nghĩa là C nằm trên đường thẳng vô hạn đi qua A và B. Giới hạn hộp giới hạn sau đó sẽ loại bỏ tất cả các điểm nằm trên cùng một đường thẳng nhưng nằm ngoài điểm cuối của đoạn thẳng. Cùng với nhau, hai điều kiện này đều cần và đủ để một điểm nằm trên một đoạn kín trong mặt phẳng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

xa, ya = map(int, input().split())
xb, yb = map(int, input().split())
e = int(input())

dx = xb - xa
dy = yb - ya

def on_segment(xc, yc):
    cx = xc - xa
    cy = yc - ya

    if dx * cy != dy * cx:
        return False

    if min(xa, xb) <= xc <= max(xa, xb) and min(ya, yb) <= yc <= max(ya, yb):
        return True

    return False

out = []
for _ in range(e):
    x, y = map(int, input().split())
    out.append("Yes" if on_segment(x, y) else "No")

print("\n".join(out))
```Mã bắt đầu bằng cách cố định vectơ chỉ hướng giữa hai robot. Mỗi truy vấn được xử lý độc lập bằng cách tính toán vectơ tương đối của nó từ Alice. Kiểm tra sản phẩm chéo là điểm quyết định trọng tâm vì nó mã hóa tính cộng tuyến mà không phân chia. 

Việc kiểm tra hộp giới hạn là cần thiết vì chỉ riêng sự cộng tuyến sẽ chấp nhận các điểm kéo dài vô tận theo cả hai hướng. So sánh tối thiểu và tối đa đảm bảo chúng tôi chỉ chấp nhận các điểm nằm giữa các điểm cuối. 

Một chi tiết triển khai tinh tế là tất cả số học đều ở dạng số nguyên, do đó không có nguy cơ xảy ra lỗi dấu phẩy động. Một chi tiết quan trọng khác là sự bình đẳng mang tính bao trùm nên các điểm cuối được chấp nhận tự động. 

## Ví dụ đã hoạt động 

Xét Alice tại (0, 0), Bob tại (2, 2) và hai truy vấn (1, 1) và (-1, -1). 

Đối với truy vấn đầu tiên: 

| Bước | cx, cy | Tích chéo dx_cy − dy_cx | Cộng tuyến | Trong hộp giới hạn | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| (1,1) | (1,1) | 2_1 − 2_1 = 0 | Có | Có | Có | 

Điều này xác nhận rằng điểm nằm chính xác ở nửa dọc theo đoạn thẳng. 

Đối với truy vấn thứ hai: 

| Bước | cx, cy | Tích chéo dx_cy − dy_cx | Cộng tuyến | Trong hộp giới hạn | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| (-1,-1) | (-1,-1) | 2*(-1) − 2*(-1) = 0 | Có | Không | Không | 

Điểm này nằm trên cùng một đường vô hạn nhưng nằm ngoài điểm cuối của đoạn, do đó nó bị từ chối bởi điều kiện hộp giới hạn. 

Hai trường hợp này cho thấy sự cần thiết của cả hai kiểm tra: chỉ cộng tuyến thôi là không đủ, và chỉ riêng hộp giới hạn là vô nghĩa nếu không đảm bảo điểm nằm trên đường thẳng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(E) | Mỗi truy vấn được xử lý với số lượng phép tính số học không đổi | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Các ràng buộc cho phép tối đa 100 truy vấn, do đó, việc kiểm tra hình học theo thời gian không đổi cho mỗi truy vấn là đủ nhanh. Tất cả các thao tác đều là phép nhân và so sánh số nguyên đơn giản, hoạt động thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    data = sys.stdin.read().strip().split()
    it = iter(data)

    xa, ya = int(next(it)), int(next(it))
    xb, yb = int(next(it)), int(next(it))
    e = int(next(it))

    dx = xb - xa
    dy = yb - ya

    def ok(xc, yc):
        cx = xc - xa
        cy = yc - ya
        if dx * cy != dy * cx:
            return False
        return min(xa, xb) <= xc <= max(xa, xb) and min(ya, yb) <= yc <= max(ya, yb)

    res = []
    for _ in range(e):
        x, y = int(next(it)), int(next(it))
        res.append("Yes" if ok(x, y) else "No")

    return "\n".join(res) + ("\n" if res else "")

# sample case
assert run("""0 0
2 2
2
1 1
-1 -1
""") == "Yes\nNo\n"

# collinear but outside segment
assert run("""0 0
2 2
1
3 3
""") == "No\n"

# endpoint case
assert run("""0 0
5 0
1
0 0
""") == "Yes\n"

# vertical line
assert run("""2 0
2 5
2
2 3
3 3
""") == "Yes\nNo\n"

# horizontal line
assert run("""0 7
4 7
2
2 7
5 7
""") == "Yes\nNo\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | Có / Không | tính đúng đắn cơ bản | 
| (3,3) đoạn ngoài | Không | cộng tác và thành viên phân khúc | 
| điểm cuối | Có | bao gồm ranh giới | 
| đường thẳng đứng | hỗn hợp | xử lý không phân chia dx=0 | 
| đường ngang | hỗn hợp | độ chính xác căn chỉnh trục chung | 

## Vỏ cạnh 

Khi Eve trùng với Alice hoặc Bob, tích chéo tự động trở thành 0 vì các vectơ bằng nhau hoặc bằng 0. Điều kiện hộp giới hạn cũng trôi qua vì tọa độ bằng một điểm cuối. Điều này đảm bảo điểm cuối được chấp nhận mà không cần xử lý đặc biệt. 

Đối với các đoạn thẳng đứng hoặc nằm ngang, một trong dx hoặc dy bằng 0. Công thức tính tích chéo vẫn hoạt động vì nó rút gọn chính xác mà không cần chia. Ví dụ: nếu dx bằng 0, điều kiện trở thành dy * cx = 0, buộc cx bằng 0, nghĩa là x phải khớp với tọa độ x của Alice. 

Đối với các điểm thẳng hàng nhưng nằm ngoài đoạn thẳng, chẳng hạn như mở rộng ra ngoài Bob theo cùng một hướng, tích chéo sẽ vượt qua nhưng hộp giới hạn sẽ loại bỏ chúng. Sự tách biệt này giúp ngăn chặn các kết quả dương tính giả và đảm bảo phân đoạn được coi là một đối tượng hữu hạn chứ không phải là một đường vô hạn.
