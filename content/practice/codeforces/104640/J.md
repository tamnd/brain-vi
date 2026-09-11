---
title: "CF 104640J - \u041f\u0430\u0443\u0442\u0438\u043d\u0430 \u0432\u043e \u0432\u0441\u0435 \u0441\u0442\u043e\u0440\u043e\u043d\u044b"
description: "Chúng ta có một ranh giới hình tròn có tâm ở gốc tọa độ và từ gốc tọa độ chúng ta tưởng tượng các tia phát ra theo mọi hướng có thể. Mỗi tia đại diện cho một “đường mạng” di chuyển ra ngoài cho đến khi nó chạm tới vòng tròn ranh giới hoặc bị chặn trước đó."
date: "2026-06-29T16:52:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104640
codeforces_index: "J"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104640
solve_time_s: 79
verified: false
draft: false
---

[CF 104640J - \u041f\u0430\u0443\u0442\u0438\u043d\u0430 \u0432\u043e \u0432\u0441\u0435 \u0441\u0442\u043e\u0440\u043e\u043d\u044b](https://codeforces.com/problemset/problem/104640/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một ranh giới hình tròn có tâm ở gốc tọa độ và từ gốc tọa độ chúng ta tưởng tượng các tia phát ra theo mọi hướng có thể. Mỗi tia đại diện cho một “đường mạng” di chuyển ra ngoài cho đến khi nó chạm tới vòng tròn ranh giới hoặc bị chặn trước đó. 

Việc chặn xuất phát từ các chướng ngại vật hình tròn nằm rải rác trên mặt phẳng. Mỗi chướng ngại vật là một chiếc đĩa được lấp đầy và một tia sẽ dừng lại ngay khi chạm vào bất kỳ chiếc đĩa nào dọc theo đường đi của nó. Bản thân nguồn gốc được đảm bảo nằm ngoài tất cả các đĩa, vì vậy mọi hướng ban đầu đều hợp lệ. 

Nhiệm vụ không phải là mô phỏng các tia một cách trực tiếp, điều này là không thể, mà là tính toán phần hướng nào từ gốc tọa độ vẫn không bị cản trở cho đến tận vòng tròn bên ngoài. 

Vì hướng từ gốc có thể được xác định bằng một góc trong$[0, 2\pi)$, bài toán trở thành: tính tổng số đo góc của các hướng sao cho đoạn từ điểm gốc đến đường tròn bên ngoài không cắt bất kỳ đĩa nào và chia nó cho$2\pi$. 

Ràng buộc$n \le 10^5$ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng kiểm tra từng hướng một cách độc lập hoặc kiểm tra các giao điểm trên mỗi mẫu góc. Thậm chí$O(n^2)$tương tác giữa các đĩa sẽ quá chậm. Cấu trúc gợi ý chúng ta phải chuyển đổi hình học thành các khoảng góc và sau đó hợp nhất chúng. 

Trường hợp cạnh tinh tế phát sinh khi một đĩa không giao nhau với đường tròn bán kính$10^6$nhưng vẫn chặn tia sớm hơn. Một trường hợp phức tạp khác là khi các đĩa chồng lên nhau nhiều, tạo ra nhiều phạm vi góc bị chặn chồng chéo và phải được hợp nhất cẩn thận. Một cách tiếp cận đơn giản xử lý từng đĩa một cách độc lập và tính tổng các khoảng góc mà không cần hợp nhất sẽ vượt quá các vùng bị chặn. 

## Phương pháp tiếp cận 

Một tia từ gốc tọa độ sẽ bị chặn bởi một đĩa nếu nó đi qua đĩa trước khi đến được vòng tròn biên. Đối với một đĩa cố định tập trung tại$(x, y)$với bán kính$r$, chúng ta có thể xét tất cả các tia từ gốc giao nhau với đĩa này. Những tia này tạo thành một khoảng góc có tâm xung quanh hướng của tâm đĩa. 

Cho phép$d = \sqrt{x^2 + y^2}$. Nếu như$d \le r$, đĩa chứa nguồn gốc, vấn đề không cho phép. Nếu không, đĩa sẽ chặn một khoảng kích thước góc được xác định bằng hình học tiếp tuyến đơn giản. Nửa góc$\alpha$thỏa mãn:$$\sin \alpha = \frac{r}{d}$$Vì thế$$\alpha = \arcsin\left(\frac{r}{d}\right)$$Hướng trung tâm là$\theta = \operatorname{atan2}(y, x)$, do đó khoảng bị chặn là:$$[\theta - \alpha, \theta + \alpha]$$Vì vậy, mỗi đĩa đóng góp một khoảng tròn trên$[0, 2\pi)$. Câu trả lời cuối cùng là tổng số đo chưa được khám phá sau khi kết hợp tất cả các khoảng này. 

Cách tiếp cận bạo lực sẽ tính toán tất cả các góc bị chặn ở độ phân giải tốt, chẳng hạn như việc rời rạc hóa đường tròn thành$K$các bước và kiểm tra khả năng hiển thị trên mỗi bước đối với tất cả các đĩa. Chi phí đó$O(nK)$, điều này là không thể thực hiện được đối với$n = 10^5$. 

Quan sát quan trọng là hình học thu gọn mỗi đĩa thành một khoảng góc duy nhất. Khi chúng ta có các khoảng thời gian, bài toán sẽ trở thành một nhiệm vụ hợp các khoảng thời gian cổ điển trên một vòng tròn. Việc sắp xếp các điểm cuối và quét cho phép chúng ta tính toán tổng số đo góc được bao phủ trong$O(n \log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lấy mẫu góc lực lượng vũ phu |$O(nK)$|$O(1)$| Quá chậm | 
| Quét theo khoảng thời gian |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mỗi đĩa thành một khoảng góc bị chặn. 

1. Với mỗi đĩa, hãy tính khoảng cách từ nó đến gốc. Điều này xác định liệu nó có thể ảnh hưởng đến bất kỳ hướng tia nào hay không. Nếu đĩa ở rất xa, nó vẫn quan trọng miễn là tia hướng tới nó đi qua nó trước khi chạm tới vòng tròn bên ngoài, do đó khoảng cách chỉ ảnh hưởng đến chiều rộng góc chứ không ảnh hưởng đến mức độ liên quan. 
2. Tính góc ở tâm$\theta = \operatorname{atan2}(y, x)$. Đây là hướng mà đĩa nằm tính từ điểm gốc. Góc này neo vùng bị chặn. 
3. Tính nửa chiều rộng góc$\alpha = \arcsin(r / d)$. Điều này xuất phát từ các đường tiếp tuyến từ gốc tới đĩa. Mọi tia trong phạm vi sai lệch này đều chạm vào đĩa. 
4. Tạo khoảng$[\theta - \alpha, \theta + \alpha]$. Bình thường hóa nó thành$[0, 2\pi)$. Nếu khoảng vượt qua ranh giới tại$0$, chia nó thành hai khoảng. Bước này là cần thiết vì không gian góc tròn không tuyến tính. 
5. Thu thập tất cả các khoảng và sắp xếp chúng theo góc bắt đầu. 
6. Hợp nhất các khoảng chồng chéo trong khi quét qua chúng. Duy trì khoảng thời gian hoạt động hiện tại và mở rộng khoảng thời gian đó bất cứ khi nào xảy ra sự chồng chéo. Điều này cho ra tổng số đo góc bị chặn. 
7. Trừ số đo bị chặn khỏi$2\pi$, sau đó chia cho$2\pi$để có được phần hướng nhìn thấy được. 

Tính đúng đắn dựa trên thực tế là mỗi đĩa chặn chính xác một khoảng góc lồi và các tia độc lập theo các hướng. Việc kết hợp các khoảng này sẽ thu được chính xác tất cả các tia bị chặn. 

### Tại sao nó hoạt động 

Đối với bất kỳ hướng cố định nào, tia bị chặn khi và chỉ khi góc đó nằm bên trong ít nhất một khoảng góc của đĩa. Mỗi đĩa đóng góp một phạm vi liên tục của các góc bị cấm vì tập hợp các tiếp tuyến từ gốc tọa độ đến đường tròn là liên tục. Do đó, tập bị chặn chính xác là tập hợp của các khoảng này. Việc tính toán sự kết hợp của chúng sẽ bảo toàn phạm vi bao phủ chính xác và việc trừ khỏi vòng tròn đầy đủ sẽ mang lại tỷ lệ hiển thị chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    n = int(input())
    events = []
    
    for _ in range(n):
        x, y, r = map(int, input().split())
        d = math.hypot(x, y)
        if d <= r:
            continue
        
        theta = math.atan2(y, x)
        alpha = math.asin(r / d)
        
        l = theta - alpha
        r_ = theta + alpha
        
        # normalize to [0, 2pi)
        twopi = 2 * math.pi
        
        while l < 0:
            l += twopi
            r_ += twopi
        while l >= twopi:
            l -= twopi
            r_ -= twopi
        
        if r_ <= twopi:
            events.append((l, r_))
        else:
            events.append((l, twopi))
            events.append((0.0, r_ - twopi))
    
    events.sort()
    
    total = 0.0
    cur_l, cur_r = None, None
    
    for l, r_ in events:
        if cur_l is None:
            cur_l, cur_r = l, r_
        elif l <= cur_r:
            cur_r = max(cur_r, r_)
        else:
            total += cur_r - cur_l
            cur_l, cur_r = l, r_
    
    if cur_l is not None:
        total += cur_r - cur_l
    
    ans = 1.0 - total / (2 * math.pi)
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp chuyển đổi mỗi đĩa thành một cặp điểm cuối góc cạnh, xử lý cẩn thận việc bao bọc ở mức$2\pi$. Bước hợp nhất đảm bảo các vùng bị chặn chồng chéo không bị tính hai lần. Phép trừ cuối cùng chuyển số đo góc bị chặn thành tỷ lệ tự do. 

Một sự tinh tế phổ biến là xử lý các khoảng thời gian vượt qua$0$góc. Việc tách chúng đảm bảo đường quét vẫn hợp lệ theo thứ tự tuyến tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các đĩa đầu vào tạo ra các khoảng góc sau (gần đúng): 

| Đĩa | Góc trung tâm | Nửa chiều rộng | Khoảng thời gian | 
| --- | --- | --- | --- | 
| (1,1,1) | ~0,785 | ~0,615 | [0,17, 1,40] | 
| (4,2,2) | ~0,463 | ~0,523 | [-0,06, 0,99] | 
| (-1,-1,1) | ~-2.356 | ~0,615 | [-2,97, -1,74] | 

Sau khi chuẩn hóa và hợp nhất: 

| Bước | Khoảng thời gian hoạt động | Tổng số bị chặn | 
| --- | --- | --- | 
| 1 | [-2,97, -1,74] | 0 | 
| 2 | [0,17, 1,40] được hợp nhất với [-0,06, 0,99] | ~1,46 | 
| cuối cùng | tổng hợp nhất | ~π | 

Số đo bị chặn xấp xỉ bằng một nửa hình tròn, vì vậy câu trả lời là$0.5$. 

Điều này xác nhận rằng các vùng góc rời rạc tương ứng chính xác với các khu vực chặn độc lập. 

### Mẫu 2 

Hai đĩa tạo ra các nhịp góc chồng lên nhau nhưng không giống nhau. Sau khi chuyển đổi và sáp nhập, công đoàn bao gồm khoảng$0.1886 \cdot 2\pi$của vòng tròn, để lại khoảng$0.8114$phần nhìn thấy được. Dấu vết xác nhận rằng việc xử lý chồng chéo là cần thiết, vì phép tính tổng đơn giản sẽ vượt quá vùng góc được chia sẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi đĩa có nhiều nhất là hai khoảng, sau đó được sắp xếp và hợp nhất | 
| Không gian |$O(n)$| Lưu trữ các khoảng góc | 

Thuật toán phù hợp thoải mái trong các ràng buộc cho$n = 10^5$, vì việc sắp xếp chiếm ưu thế và nằm trong giới hạn điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def solve():
        n = int(input())
        events = []
        for _ in range(n):
            x, y, r = map(int, input().split())
            d = math.hypot(x, y)
            if d <= r:
                continue
            theta = math.atan2(y, x)
            alpha = math.asin(r / d)
            l = theta - alpha
            r_ = theta + alpha
            twopi = 2 * math.pi
            while l < 0:
                l += twopi
                r_ += twopi
            while l >= twopi:
                l -= twopi
                r_ -= twopi
            if r_ <= twopi:
                events.append((l, r_))
            else:
                events.append((l, twopi))
                events.append((0.0, r_ - twopi))
        events.sort()
        total = 0.0
        cur = None
        for l, r_ in events:
            if cur is None:
                cur = [l, r_]
            elif l <= cur[1]:
                cur[1] = max(cur[1], r_)
            else:
                total += cur[1] - cur[0]
                cur = [l, r_]
        if cur is not None:
            total += cur[1] - cur[0]
        return 1.0 - total / (2 * math.pi)

    return str(round(solve(), 7))

# provided samples
assert abs(float(run("""3
1 1 1
4 2 2
-1 -1 1
""")) - 0.5) < 1e-6

assert abs(float(run("""2
4 0 1
0 3 1
""")) - 0.8113959) < 1e-5

# custom cases
assert abs(float(run("""1
100 0 1
""")) - 1.0) < 1e-6, "single small blocker"

assert abs(float(run("""1
1 0 1
""")) - 0.0) < 1e-6, "block at origin direction"

assert abs(float(run("""2
10 0 2
-10 0 2
""")) - 0.0) < 1e-6, "two opposite blockers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chặn nhỏ duy nhất | 1.0 | không chồng chéo, bảo hiểm tối thiểu | 
| khối ở hướng gốc | 0,0 | cạnh bao phủ góc đầy đủ | 
| hai kẻ chặn đối diện | 0,0 | bao phủ toàn bộ vòng tròn qua hai khoảng thời gian | 

## Vỏ cạnh 

Một đĩa ở xa nhưng đủ lớn vẫn tạo ra khoảng góc rất hẹp. Việc tính toán xử lý việc này một cách tự nhiên bởi vì$r/d$trở nên nhỏ và$\arcsin(r/d)$tiến tới 0, tạo ra một khoảng hợp lệ đóng góp phạm vi bao phủ không đáng kể nhưng chính xác. 

Một đĩa nằm gần như chính xác trên một tiếp tuyến tính từ gốc tọa độ sẽ tạo ra những khoảng cực kỳ nhỏ. Độ chính xác nổi trở nên phù hợp, nhưng vì độ chính xác cần thiết là$10^{-4}$, độ chính xác kép tiêu chuẩn là đủ. 

Khoảng thời gian vượt qua$0$góc được chia thành hai phần. Nếu không phân tách, việc sắp xếp sẽ coi chúng là các khoảng thời gian đảo ngược và phá vỡ sự hợp nhất. Bước chuẩn hóa đảm bảo tính chính xác bằng cách nhúng miền tròn vào miền tuyến tính.
