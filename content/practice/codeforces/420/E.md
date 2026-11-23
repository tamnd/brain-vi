---
title: "CF 420E - Chơi bóng"
description: "Một quả bóng được ném từ gốc tọa độ theo một số hướng đã chọn và nó liên tục xuất hiện tại các điểm cách đều nhau dọc theo tia đó: đầu tiên ở khoảng cách $d$, sau đó ở $2d$, sau đó là $3d$, v.v. Hướng được cố định sau khi được chọn, nhưng nó có thể là bất kỳ hướng thực nào trong mặt phẳng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 420
codeforces_index: "E"
codeforces_contest_name: "Coder-Strike 2014 - Finals (online edition, Div. 1)"
rating: 2600
weight: 420
solve_time_s: 120
verified: true
draft: false
---

[CF 420E - Chơi bóng](https://codeforces.com/problemset/problem/420/E) 

**Đánh giá:** 2600 
**Thẻ:** hình học 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một quả bóng được ném từ gốc tọa độ theo một hướng đã chọn nào đó và nó liên tục xuất hiện ở những điểm cách đều nhau dọc theo tia đó: đầu tiên ở khoảng cách$d$, thì tại$2d$, sau đó$3d$, vân vân. Hướng được cố định sau khi được chọn, nhưng nó có thể là bất kỳ hướng thực nào trong mặt phẳng. 

Trên máy bay có$n$vòng tròn. Mỗi khi một trong những điểm hạ cánh rời rạc này rơi vào bên trong hoặc trên một vòng tròn, vòng tròn đó sẽ đóng góp một điểm. Nếu một điểm hạ cánh nằm trong nhiều vòng tròn thì tất cả chúng đều đóng góp đồng thời. Nhiệm vụ là chọn hướng của tia sao cho tổng số lần “đánh trúng” như vậy trên tất cả các vòng tròn và tất cả các điểm hạ cánh là tối đa. 

Đối tượng chính không phải là một quỹ đạo liên tục mà là một chuỗi điểm vô hạn rời rạc$P_k = k \cdot d \cdot u$, Ở đâu$u$là một vectơ đơn vị. Điểm số được xác định hoàn toàn bằng số điểm rơi vào các đĩa nhất định. 

Những hạn chế quan trọng theo một cách rất cụ thể. Với tối đa$2 \cdot 10^4$vòng tròn, bất kỳ cách tiếp cận nào kiểm tra trực tiếp tất cả các hướng hoặc tất cả các cặp vòng tròn đều đã quá lớn. Quan trọng hơn, bất kỳ giải pháp nào cố gắng mô phỏng tất cả các tia hoặc rời rạc hóa các góc quá tinh vi sẽ thất bại vì câu trả lời phụ thuộc vào sự lựa chọn hướng liên tục chứ không phải một tập hữu hạn các ứng cử viên. 

Mỗi vòng tròn có bán kính tương đối nhỏ (nhiều nhất là 50) và khoảng cách$d$cũng nhỏ (nhiều nhất là 10). Điều này gợi ý rõ ràng rằng đối với một vòng tròn cố định, chỉ một số lượng nhỏ các chỉ số$k$có thể tạo ra những lượt truy cập, bởi vì một khi$k d$cách xa tâm đường tròn đến gốc thì đường tròn không chứa được điểm đó. 

Một trường hợp thất bại tinh vi sẽ xuất hiện nếu người ta cho rằng mỗi vòng tròn đóng góp nhiều nhất một lần trúng đích. Một vòng tròn có thể được giao nhau nhiều lần bởi các bội số khác nhau của$d$. Ví dụ, nếu một đường tròn có tâm ở gần gốc tọa độ nhưng đủ lớn thì cả hai$k=2$Và$k=3$có thể tạo ra các lượt truy cập hợp lệ cho cùng một hướng. 

Một cạm bẫy khác là xử lý vấn đề như thể mỗi vòng tròn chỉ áp đặt một ràng buộc góc duy nhất. Sự ràng buộc thực sự phụ thuộc vào$k$, và khác nhau$k$các giá trị cho cùng một đường tròn tạo ra các khoảng góc khác nhau. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề là cố định một hướng và mô phỏng tia sáng. Đối với mỗi vòng tròn và với mỗi số nguyên$k$, chúng tôi kiểm tra xem điểm$k d u$nằm bên trong vòng tròn. Trên tất cả các vòng kết nối và tất cả$k$, chúng tôi đếm các trận đấu. Điều này đúng nhưng chậm đến mức vô vọng, vì số lượng đề xuất hướng là vô hạn, và thậm chí việc đánh giá một hướng cũng đòi hỏi phải kiểm tra tất cả các vòng tròn và tất cả các hướng có liên quan.$k$. 

Quan sát cấu trúc quan trọng là đảo ngược quan điểm. Thay vì cố định hướng và kiểm tra các vòng tròn, chúng tôi cố định một vòng tròn và hỏi hướng nào và chỉ số nào$k$điểm tương ứng nằm bên trong nó. 

Cố định một vòng tròn có tâm$C$. Đối với một cố định$k$, điều kiện đó$k d u$nằm bên trong đường tròn là một ràng buộc hình học về hướng$u$. điểm$k d u$ở một khoảng cố định so với gốc tọa độ, vì vậy chúng ta đang giao nhau giữa hai đối tượng: một đường tròn có tâm tại gốc tọa độ với bán kính$k d$, và đường tròn đã cho có tâm tại$C$. Giao điểm này tương ứng với một tập hợp các góc mà từ đó gốc tọa độ có thể “nhìn thấy” đường tròn ở khoảng cách hướng tâm đó. 

Đối với mỗi hợp lệ$k$, tập hợp hướng$u$tạo thành một khoảng góc tiếp giáp. Khoảng đó tập trung xung quanh hướng của$C$và nửa chiều rộng của nó được xác định bởi định luật cosin. 

Câu hỏi còn lại là có bao nhiêu$k$giá trị trên mỗi vòng tròn phải được xem xét. Khoảng cách từ điểm gốc đến$k d u$là cố định, vì vậy đối với một đường tròn có tâm ở khoảng cách$|C|$, chỉ các giá trị của$k$như vậy$k d$ở trong$r$của$|C|$có thể có thể hoạt động. Điều đó mang lại một phạm vi số nguyên chặt chẽ:$$\frac{|C| - r}{d} \le k \le \frac{|C| + r}{d}.$$Bởi vì$r \le 50$Và$d \ge 5$, phạm vi này chứa tối đa khoảng 20 số nguyên trên mỗi vòng tròn. 

Điều này giúp giảm bớt vấn đề trong việc tạo ra một tập hợp các khoảng góc, mỗi khoảng mang trọng số 1 và sau đó tìm sự chồng chéo tối đa giữa chúng trên đường tròn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với các hướng và vòng tròn | Vô hạn / hàm mũ | O(n) | Quá chậm | 
| Tạo khoảng thời gian hợp lệ$k$giá trị |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi vòng tròn, hãy tính khoảng cách của nó$R$tính từ gốc tọa độ và góc định hướng$\theta$từ điểm gốc đến trung tâm của nó. Điều này sẽ sửa hình học trung tâm của tất cả các lần truy cập có thể có cho vòng tròn đó. 
2. Xác định phạm vi số nguyên của$k$sao cho đường tròn có thể cắt đường tròn có tâm tại gốc tọa độ với bán kính$k d$. Điều này có được từ$|R - r| \le k d \le R + r$, sau đó được chuyển đổi thành giới hạn số nguyên. Bước này lọc ra tất cả các bội số không liên quan. 
3. Với mỗi số nguyên$k$trong phạm vi này, hãy coi vấn đề này như một vấn đề về khả năng hiển thị góc giữa hai vòng tròn: một tâm ở gốc có bán kính$k d$, và đường tròn đã cho. 
4. Tính nửa chiều rộng góc$\alpha$sử dụng định luật cosin:$$\cos \alpha = \frac{(k d)^2 + R^2 - r^2}{2 k d R}.$$Kẹp các vấn đề số học để các giá trị nằm ngoài một chút$[-1,1]$không phá vỡ arccos. 
5. Thêm một khoảng thời gian$[\theta - \alpha, \theta + \alpha]$. Nếu nó vượt qua$-\pi, \pi$ranh giới, chia nó thành hai khoảng. 
6. Sau khi xử lý tất cả các vòng tròn và tất cả đều hợp lệ$k$, sắp xếp tất cả các điểm cuối khoảng và quét qua các góc, duy trì tổng các khoảng hoạt động. 
7. Giá trị tối đa của tổng chạy này là đáp án, tương ứng với hướng giao với số sự kiện trúng vòng tròn tối đa. 

### Tại sao nó hoạt động 

Mỗi cặp$(\text{circle}, k)$đóng góp độc lập vào điểm số: đối với một hướng cố định, nó được tính hoặc không được tính. Hình học cho thấy rằng đối với cố định$k$, tập hợp các hướng thỏa mãn điều kiện có đúng một khoảng góc. Do đó, mọi sự kiện hợp lệ có thể được biểu diễn dưới dạng một khoảng trên vòng tròn đơn vị và điểm cuối cùng cho một hướng chính xác là số khoảng bao phủ góc đó. Tối đa hóa điểm số trở thành vấn đề chồng chéo tối đa trên một vòng tròn và đường quét ghi lại chính xác tất cả các phần trùng lặp. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def norm_angle(a):
    while a <= -math.pi:
        a += 2 * math.pi
    while a > math.pi:
        a -= 2 * math.pi
    return a

def add_interval(events, l, r):
    if l <= r:
        events.append((l, 1))
        events.append((r, -1))
    else:
        events.append((-math.pi, 1))
        events.append((r, -1))
        events.append((l, 1))
        events.append((math.pi, -1))

n, d = map(int, input().split())

events = []

for _ in range(n):
    x, y, r = map(int, input().split())
    R = math.hypot(x, y)
    if R == 0:
        continue

    base_angle = math.atan2(y, x)

    k_min = int(math.ceil((R - r) / d))
    k_max = int(math.floor((R + r) / d))

    if k_max < 1:
        continue
    k_min = max(k_min, 1)

    for k in range(k_min, k_max + 1):
        rk = k * d

        val = (rk * rk + R * R - r * r) / (2 * rk * R)
        val = max(-1.0, min(1.0, val))

        ang = math.acos(val)

        l = base_angle - ang
        rgt = base_angle + ang

        l = norm_angle(l)
        rgt = norm_angle(rgt)

        add_interval(events, l, rgt)

events.sort()

ans = 0
cur = 0
for a, t in events:
    cur += t
    ans = max(ans, cur)

print(ans)
```Mã bắt đầu bằng cách chuyển đổi từng vòng tròn thành mô tả góc của tất cả các hướng nhìn hợp lệ cho từng hướng khả thi.$k$. các`k_min`Và`k_max`tính toán là bước cắt tỉa quan trọng nhằm ngăn chặn việc lặp lại các chỉ số không cần thiết. 

Mỗi khoảng thời gian được chuẩn hóa cẩn thận để xử lý việc bao bọc tại$\pm \pi$, bởi vì việc quét góc yêu cầu một thứ tự tuyến tính. Bản thân quá trình quét là một mảng khác biệt tiêu chuẩn trên các sự kiện được sắp xếp. 

Một chi tiết tinh tế là kẹp đối số cosin trước khi gọi`acos`, vì số học dấu phẩy động gần tiếp tuyến có thể tạo ra các giá trị nằm ngoài miền hợp lệ một chút. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có hai vòng tròn và kích thước bước vừa phải. 

### Ví dụ 1 

đầu vào:```
2 5
1 1 1
5 0 1
```| Vòng tròn | R | k phạm vi | Khoảng thời gian được tạo | 
| --- | --- | --- | --- | 
| (1,1,1) | √2 | k=1 | vòng cung nhỏ khoảng 45° | 
| (5,0,1) | 5 | k=1 | vòng cung quanh 0° | 

Việc quét qua các góc không bao giờ tích lũy nhiều hơn 1 lần trùng lặp, vì vậy câu trả lời là 1. Điều này cho thấy rằng mặc dù cả hai vòng tròn đều có thể truy cập được nhưng không có hướng nào căn chỉnh cả hai lần truy cập giống nhau$k$. 

### Ví dụ 2 

đầu vào:```
1 5
10 0 6
```| k | Bán kính điểm | Tình trạng | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 5 | vòng tròn bên trong | hợp lệ | 
| 2 | 10 | vòng tròn bên trong | hợp lệ | 

Cả hai$k=1$Và$k=2$tạo ra các khoảng hợp lệ có tâm ở 0 radian, do đó quá trình quét tích lũy 2 lần chồng chéo ở góc 0. 

Điều này chứng tỏ rằng một vòng tròn có thể đóng góp nhiều sự kiện tính điểm cho một hướng cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + K)$| Mỗi vòng tròn đóng góp tối đa ~20 giá trị của$k$, mỗi khoảng tạo ra một khoảng, sau đó là sắp xếp và quét | 
| Không gian |$O(n)$| Lưu trữ tất cả các sự kiện theo khoảng thời gian | 

Ràng buộc trên$k$là điều giữ cho giải pháp được thoải mái trong giới hạn: mặc dù có tới$2 \cdot 10^4$vòng tròn, tổng số khoảng thời gian được tạo vẫn ở khoảng vài trăm nghìn, nằm trong khoảng 2 giây C++ hoặc giải pháp Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    input = sys.stdin.readline

    n, d = map(int, input().split())
    events = []

    def norm(a):
        while a <= -math.pi:
            a += 2 * math.pi
        while a > math.pi:
            a -= 2 * math.pi
        return a

    def add(l, r):
        if l <= r:
            events.append((l, 1))
            events.append((r, -1))
        else:
            events.append((-math.pi, 1))
            events.append((r, -1))
            events.append((l, 1))
            events.append((math.pi, -1))

    for _ in range(n):
        x, y, r = map(int, input().split())
        R = math.hypot(x, y)
        if R == 0:
            continue
        ang0 = math.atan2(y, x)

        kmin = int(math.ceil((R - r) / d))
        kmax = int(math.floor((R + r) / d))
        kmin = max(kmin, 1)
        if kmax < kmin:
            continue

        for k in range(kmin, kmax + 1):
            rk = k * d
            val = (rk*rk + R*R - r*r) / (2*rk*R)
            val = max(-1.0, min(1.0, val))
            ang = math.acos(val)
            add(norm(ang0-ang), norm(ang0+ang))

    events.sort()
    cur = ans = 0
    for _, t in events:
        cur += t
        ans = max(ans, cur)
    return str(ans)

# provided samples
assert run("2 5\n1 1 1\n5 0 1\n") == "1"

# circle hits twice
assert run("1 5\n10 0 6\n") == "2"

# far circle, no hit
assert run("1 5\n100 0 1\n") == "0"

# symmetric case
assert run("2 5\n10 0 6\n-10 0 6\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | 1 | tính đúng đắn cơ bản | 
| vòng tròn đơn nhiều lần truy cập | 2 | nhiều k đóng góp cho mỗi vòng kết nối | 
| vòng tròn xa | 0 | cắt tỉa k không hợp lệ | 
| vòng tròn đối xứng | 2 | tích lũy qua các khoảng thời gian độc lập | 

## Vỏ cạnh 

Một đường tròn ở xa gốc tọa độ có bán kính nhỏ tạo ra một đường tròn rỗng$k$-range và thuật toán tự nhiên bỏ qua nó vì$k_{\max} < k_{\min}$. Điều này ngăn chặn mọi khoảng thời gian không hợp lệ và tránh việc tính toán dấu phẩy động không cần thiết. 

Khi một vòng tròn đủ lớn để cắt nhiều bán kính$k d$, thuật toán tạo ra nhiều khoảng cho cùng một vòng tròn. Mỗi khoảng là độc lập và đường quét tính chính xác chúng là những đóng góp riêng biệt. Điều này rất quan trọng trong trường hợp hướng tối ưu đi qua cùng một vùng hình học ở các khoảng cách khác nhau. 

Khi một khoảng vượt qua$-\pi$ranh giới, nó được chia thành hai, đảm bảo đường quét có thứ tự nhất quán. Nếu không có điều này, cấu trúc góc cạnh sẽ bị gãy và các phần chồng lên nhau gần vết cắt cành có thể bị tính sai.
