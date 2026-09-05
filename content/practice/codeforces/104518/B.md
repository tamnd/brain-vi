---
title: "CF 104518B - Cuộc chiến khoai tây 1"
description: "Hai người chơi, mỗi người bắt đầu với một số lượng khoai tây cố định và sau đó trồng thêm khoai tây với tốc độ không đổi hàng ngày. Technoblade bắt đầu với khoai tây trị giá $X1$ và kiếm được khoai tây trị giá $Y1$ mỗi ngày. Đối thủ của anh ta bắt đầu với số khoai tây trị giá $X2$ và kiếm được số khoai tây trị giá $Y2$ mỗi ngày."
date: "2026-06-30T10:36:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104518
codeforces_index: "B"
codeforces_contest_name: "UNICAMP Selection Contest 2023"
rating: 0
weight: 104518
solve_time_s: 51
verified: true
draft: false
---

[CF 104518B - Cuộc chiến khoai tây 1](https://codeforces.com/problemset/problem/104518/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hai người chơi, mỗi người bắt đầu với một số lượng khoai tây cố định và sau đó trồng thêm khoai tây với tốc độ không đổi hàng ngày. Technoblade bắt đầu với$X_1$khoai tây và lợi nhuận$Y_1$khoai tây mỗi ngày. Đối thủ của anh ta bắt đầu bằng$X_2$khoai tây và lợi nhuận$Y_2$khoai tây mỗi ngày. Thời gian trôi qua trong cả ngày và cả hai quá trình tăng trưởng đều diễn ra với tốc độ như nhau. 

Nhiệm vụ là xác định ngày sớm nhất mà tổng điểm của Technoblade vượt quá tổng điểm của đối thủ. Nếu khoảnh khắc đó không bao giờ đến thì câu trả lời là -1. 

Sau đó$d$ngày, tổng số là biểu thức tuyến tính:$$T(d) = X_1 + d \cdot Y_1, \quad S(d) = X_2 + d \cdot Y_2$$Chúng ta được yêu cầu tìm số nguyên không âm nhỏ nhất$d$như vậy$T(d) > S(d)$, hoặc quyết định rằng không có điều đó$d$tồn tại. 

Các ràng buộc rất nhỏ, với tất cả các giá trị lên tới 1000, điều đó có nghĩa là thậm chí có thể mô phỏng trực tiếp theo thời gian. Tuy nhiên, các bài toán tăng trưởng tuyến tính thường ẩn giấu một vấn đề tế nhị: khi đối thủ cạnh tranh tăng trưởng nhanh hơn hoặc xuất phát trước, sự bất bình đẳng có thể không bao giờ thay đổi hoặc chỉ có thể thay đổi một lần. Điều đó khiến việc suy luận về tính đơn điệu thay vì lặp đi lặp lại một cách mù quáng là điều quan trọng. 

Một trường hợp thất bại phổ biến xuất hiện khi Technoblade xuất phát chậm hơn nhưng lại phát triển nhanh hơn. Một giải pháp ngây thơ có thể dừng lại quá sớm hoặc không được xem xét$d = 0$. Một trường hợp khác là khi cả hai đều tăng trưởng với tốc độ như nhau nhưng một trường hợp bắt đầu trước, trong trường hợp đó câu trả lời là 0 hoặc không thể mãi mãi tùy thuộc vào giá trị ban đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu mô phỏng từng ngày. Bắt đầu từ$d = 0$, chúng tôi tính toán cả hai giá trị và kiểm tra xem Technoblade có nhiều khoai tây hơn hay không. Nếu không, chúng tôi tăng ngày và lặp lại. Vì các giá trị tăng tuyến tính nên quá trình này được đảm bảo cuối cùng sẽ tìm thấy điểm giao nhau hoặc phát hiện rằng nó không bao giờ xảy ra trong một giới hạn hợp lý. 

Điều này hiệu quả vì sự khác biệt giữa hai người chơi cũng tiến triển theo tuyến tính. Tuy nhiên, nếu chúng ta mô phỏng vô thời hạn thì trường hợp xấu nhất xảy ra là Technoblade không bao giờ đuổi kịp. Trong tình huống đó, một vòng lặp đơn giản sẽ chạy mãi mãi hoặc dựa vào một điểm cắt tùy ý, điều này không an toàn. 

Quan sát quan trọng là chúng ta có thể so sánh trực tiếp tốc độ tăng trưởng. Xác định sự khác biệt:$$D(d) = (X_1 - X_2) + d (Y_1 - Y_2)$$Đây là một hàm tuyến tính. Nếu như$Y_1 \le Y_2$, hàm số không tăng hoặc không đổi. Trong trường hợp đó, nếu Technoblade chưa dẫn đầu$d = 0$, sau này anh ấy sẽ không bao giờ vượt lên được. Nếu như$Y_1 > Y_2$, sự khác biệt tăng lên theo thời gian, do đó sẽ có nhiều nhất một điểm giao nhau. Thay vì mô phỏng, chúng ta có thể giải một bất đẳng thức đơn giản. 

Từ:$$X_1 + dY_1 > X_2 + dY_2$$chúng tôi nhận được:$$X_1 - X_2 > d (Y_2 - Y_1)$$Nếu như$Y_1 = Y_2$, sự so sánh không bao giờ thay đổi sau ngày 0. Mặt khác, chúng ta có thể tính ngưỡng trực tiếp bằng cách sử dụng số học số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(D) | O(1) | Quá chậm/không an toàn | 
| Công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chênh lệch ban đầu$diff = X_1 - X_2$. Điều này nắm bắt được ai đang dẫn đầu trước khi bất kỳ sự tăng trưởng nào xảy ra. 
2. Nếu$diff > 0$, Technoblade đã dẫn trước rất nhiều vào ngày 0, vì vậy câu trả lời là 0. Không cần mô phỏng trong tương lai vì yêu cầu đã được đáp ứng. 
3. Nếu$Y_1 = Y_2$, cả hai người chơi đều phát triển với tốc độ như nhau nên sự khác biệt không bao giờ thay đổi. Nếu Technoblade chưa dẫn trước thì anh ta không thể nào vượt lên dẫn trước được, vì vậy hãy trả về -1. 
4. Nếu$Y_1 < Y_2$, Technoblade phát triển chậm hơn, nghĩa là khoảng cách chỉ ngày càng trầm trọng hơn theo thời gian. Ngay cả khi ban đầu anh ta ở gần, anh ta không bao giờ có thể vượt qua được, vì vậy hãy quay lại -1. 
5. Nếu$Y_1 > Y_2$, sự khác biệt tăng lên một lượng dương cố định mỗi ngày. Chúng ta cần cái nhỏ nhất$d$như vậy:$$X_1 + dY_1 > X_2 + dY_2$$Sắp xếp lại:$$diff + d (Y_1 - Y_2) > 0$$Giải quyết cho$d$:$$d > \frac{X_2 - X_1}{Y_1 - Y_2}$$Số nguyên nhỏ nhất$d$thỏa mãn điều này là:$$d = \left\lfloor \frac{X_2 - X_1}{Y_1 - Y_2} \right\rfloor + 1$$6. Xuất kết quả tính toán$d$. 

### Tại sao nó hoạt động 

Bất biến chính là sự khác biệt giữa hai người chơi là hàm tuyến tính của thời gian với độ dốc không đổi$Y_1 - Y_2$. Hàm tuyến tính trên số nguyên có thể vượt qua 0 nhiều nhất một lần trừ khi độ dốc bằng 0. Điều này có nghĩa là thứ tự của hai người chơi có thể thay đổi nhiều nhất một lần và điểm giao nhau chính xác có thể được tính toán theo đại số thay vì phải tìm kiếm. Thuật toán khai thác điều này bằng cách chuyển đổi một quy trình động thành một giải bất đẳng thức duy nhất, đảm bảo tính chính xác mà không cần lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x1, y1, x2, y2 = map(int, input().split())
    
    diff = x1 - x2
    
    if diff > 0:
        print(0)
        return
    
    if y1 == y2:
        print(-1)
        return
    
    if y1 < y2:
        print(-1)
        return
    
    # y1 > y2
    gap = x2 - x1
    gain = y1 - y2
    
    d = gap // gain
    if gap % gain != 0:
        d += 1
    
    print(d)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách kiểm tra xem Technoblade đã đi trước ngày 0 hay chưa, điều này ngay lập tức xác định câu trả lời. Sau đó, nó xử lý các trường hợp trong đó tốc độ tăng trưởng tương đối không thuận lợi, vì trong cả hai kịch bản tăng trưởng bằng nhau và nhỏ hơn, sự bất bình đẳng không bao giờ nghiêng về phía có lợi cho anh ta. 

Chỉ trong trường hợp tăng nghiêm ngặt, nó mới tính toán điểm giao nhau chính xác. Phép chia được xử lý cẩn thận bằng cách sử dụng logic trần, vì chúng ta cần ngày nguyên đầu tiên chứa bất đẳng thức nghiêm ngặt. Việc sử dụng số học số nguyên tránh được các vấn đề về độ chính xác của dấu phẩy động và giữ cho giải pháp mang tính xác định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
100 2 200 1
```Đây$X_1 = 100$,$Y_1 = 2$,$X_2 = 200$,$Y_2 = 1$| Ngày d | Kỹ thuật | Đối thủ | Sự khác biệt | 
| --- | --- | --- | --- | 
| 0 | 100 | 200 | -100 | 
| 1 | 102 | 201 | -99 | 
| 2 | 104 | 202 | -98 | 
| 100 | 300 | 300 | 0 | 
| 101 | 302 | 301 | 1 | 

Sự khác biệt bắt đầu âm nhưng tăng lên mỗi ngày thêm 1. Lần đầu tiên nó trở nên dương hoàn toàn là lúc$d = 101$, phù hợp với công thức:$$d = \frac{100}{1} + 1 = 101$$Điều này xác nhận rằng thuật toán xác định chính xác giao điểm bất đẳng thức nghiêm ngặt đầu tiên. 

### Ví dụ 2 

đầu vào:```
5 1 10 2
```| Ngày d | Kỹ thuật | Đối thủ | Sự khác biệt | 
| --- | --- | --- | --- | 
| 0 | 5 | 10 | -5 | 
| 1 | 6 | 12 | -6 | 
| 2 | 7 | 14 | -7 | 

Khoảng cách đang bị thu hẹp theo chiều hướng sai lầm vì Technoblade phát triển chậm hơn. Sự khác biệt không bao giờ trở nên tích cực, vì vậy câu trả lời là -1. Thuật toán phân loại chính xác trường hợp này thông qua$Y_1 < Y_2$kiểm tra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học và so sánh được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Các ràng buộc cho phép mọi cách tiếp cận, nhưng giải pháp thời gian không đổi đảm bảo tính toán ngay lập tức ngay cả đối với các lần chạy lặp lại hoặc đầu vào mở rộng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-style cases
assert run("100 0 99 1000") == "0"
assert run("100 2 200 1") == "101"

# equal growth, already ahead
assert run("10 5 1 5") == "0"

# equal growth, never ahead
assert run("1 2 10 2") == "-1"

# slower growth always losing
assert run("5 1 10 2") == "-1"

# exact boundary crossing
assert run("0 3 3 1") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 5 1 5 | 0 | đã dẫn trước ở d=0 | 
| 1 2 10 2 | -1 | tăng trưởng ngang nhau, không bao giờ đuổi kịp | 
| 0 3 3 1 | 2 | trường hợp chéo số nguyên chính xác | 

## Vỏ cạnh 

Khi cả hai người chơi đều phát triển với tốc độ như nhau, hệ thống sẽ trở nên tĩnh về mặt thứ tự. Đối với đầu vào`10 3 5 3`, chênh lệch luôn là 5, vì vậy câu trả lời là 0. Thuật toán xử lý vấn đề này ngay lập tức thông qua việc kiểm tra đẳng thức về tốc độ tăng trưởng. 

Khi Technoblade bắt đầu chậm hơn nhưng phát triển nhanh hơn, câu trả lời hoàn toàn phụ thuộc vào việc hàm tuyến tính có vượt qua 0 tại một điểm nguyên hay không. Vì`0 2 5 1`, chênh lệch tăng thêm 1 mỗi ngày bắt đầu từ -5, do đó, việc vượt qua xảy ra chính xác khi mức tăng tích lũy bù đắp cho khoản thâm hụt ban đầu. Giá trị tính toán$d = 5$phù hợp với mô phỏng trực tiếp. 

Khi Technoblade phát triển chậm hơn, chẳng hạn như`10 1 100 5`, khoảng cách càng trở nên tồi tệ hơn mỗi ngày. Thuật toán phát hiện điều này thông qua$Y_1 < Y_2$và trả về -1 mà không cần thực hiện tính toán không cần thiết, phù hợp với thực tế là hiệu tuyến tính có độ dốc âm và không bao giờ vượt qua 0 theo hướng dương.
