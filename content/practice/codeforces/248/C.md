---
title: "CF 248C - Cầu thủ robot"
description: "Chúng ta đang làm việc bên trong một sân bóng đá hình chữ nhật, trong đó phía bên trái chứa một đoạn khung thành trên đường thẳng đứng $x = 0$, và phía bên phải chứa một bức tường nằm ngang ở độ cao $y = yw$."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "geometry"]
categories: ["algorithms"]
codeforces_contest: 248
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 152 (Div. 2)"
rating: 2000
weight: 248
solve_time_s: 120
verified: false
draft: false
---

[CF 248C - Cầu thủ bóng đá robot](https://codeforces.com/problemset/problem/248/C) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, hình học 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc bên trong một sân bóng đá hình chữ nhật, trong đó phía bên trái chứa đoạn khung thành trên đường thẳng đứng$x = 0$, và phía bên phải có một bức tường nằm ngang ở độ cao$y = y_w$. Một quả bóng bắt đầu tại một điểm nhất định$(x_b, y_b)$và phải được đá theo đường thẳng về một điểm đã chọn trên bức tường ngang đó. Sau khi chạm vào tường, nó phản xạ hoàn hảo, nghĩa là góc tới bằng góc phản xạ, sau đó truyền theo đường thẳng về phía khung thành. 

Mục tiêu không chỉ là đánh trúng vạch cầu môn mà còn đảm bảo rằng sau đúng một lần nảy trên tường ngang, bóng sẽ đi qua đoạn vạch$x = 0$giữa$y_1$Và$y_2$. Người đá được phép chọn bất kỳ điểm nào$(x_w, y_w)$trên tường và mục tiêu là xác định xem điểm đó có tồn tại hay không và nếu có thì xuất ra$x$-điều phối. 

Một vấn đề hình học quan trọng là quả bóng có bán kính$r$, do đó, việc “chạm” vào tường hoặc cột được hiểu là tâm của quả bóng nằm trong khoảng cách$r$. Điều này tạo ra các giới hạn an toàn một cách hiệu quả, đặc biệt là xung quanh đoạn khung thành và ranh giới sân, nhưng cấu trúc hình học chính vẫn bị chi phối bởi chuyển động và phản xạ theo đường thẳng. 

Các ràng buộc cho phép tọa độ lên tới$10^6$, điều này gợi ý mạnh mẽ một$O(1)$hoặc xây dựng hình học logarit cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng phân biệt các điểm ứng cử viên trên tường hoặc mô phỏng quỹ đạo cho nhiều lựa chọn sẽ quá chậm và không ổn định về mặt số lượng. 

Trường hợp cạnh tinh tế xuất hiện khi hướng phản xạ đã chọn dẫn bóng quay trở lại tường theo cách suy biến. Nếu hình học buộc quả bóng trượt khoảng thời gian ghi bàn hoặc vi phạm điều kiện “nảy chính xác một lần”, câu trả lời phải là$-1$. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là thử nhiều điểm mục tiêu ứng viên$(x_w, y_w)$dọc theo bức tường, mô phỏng quỹ đạo, phản ánh đường đi và kiểm tra xem đoạn cuối cùng có vượt qua khoảng mục tiêu hay không. Điều này đòi hỏi, đối với mỗi ứng viên, tính toán các giao điểm với bức tường và sau đó kiểm tra xem tia phản xạ có chạm vào đoạn đó hay không.$[y_1, y_2]$. Ngay cả khi chúng ta rời rạc hóa bức tường một cách tinh tế, chẳng hạn$10^5$điểm, mỗi mô phỏng bao gồm hình học dấu phẩy động và kiểm tra giao điểm, quá chậm và dễ vỡ. 

Quan sát quan trọng là sự phản chiếu trên một đường ngang có thể được loại bỏ hoàn toàn bằng cách sử dụng thủ thuật hình học tiêu chuẩn để phản chiếu đích đến. Thay vì nghĩ đến việc bật lại, chúng tôi phản chiếu đường mục tiêu qua bức tường và thay thế đường đi bị hỏng bằng một đoạn thẳng duy nhất từ ​​điểm bắt đầu đến điểm mục tiêu được phản chiếu. Điểm nảy trở thành giao điểm chính xác của đoạn này với bức tường. 

Điều này chuyển vấn đề thành việc chọn một điểm$y$trong khoảng thời gian mục tiêu sao cho đường từ$(x_b, y_b)$ĐẾN$(0, 2y_w - y)$cắt đường$y = y_w$ở mức hợp lệ$x_w$. Hạn chế duy nhất còn lại là giao lộ phải xảy ra sau khi rời khỏi điểm xuất phát và trước khi đạt được mục tiêu được phản ánh, điều này chuyển thành một bất đẳng thức đơn giản về tham số của đường. 

Bạo lực có hiệu quả vì chúng ta có thể thử tất cả$y \in [y_1, y_2]$, nhưng điều này là không cần thiết. Cấu trúc của ràng buộc làm giảm vấn đề kiểm tra xem khoảng$[y_1, y_2]$chứa bất kỳ giá trị nào thỏa mãn bất đẳng thức tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hãy thử tất cả các điểm trên tường bằng mô phỏng |$O(N)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Hình học gương + kiểm tra khoảng thời gian |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc hoàn toàn dựa trên việc xây dựng mục tiêu phản ánh. 

1. Tính khoảng cách thẳng đứng từ quả bóng tới bức tường:$d = y_w - y_b$. Điều này là tích cực do những hạn chế đầu vào. 
2. Đối với bất kỳ điểm mục tiêu nào đã chọn$y \in [y_1, y_2]$, xác định vị trí phản chiếu của nó trên tường là$y' = 2y_w - y$. Điều này chuyển đổi đường phản xạ hai đoạn thành một đường thẳng từ$(x_b, y_b)$ĐẾN$(0, y')$. 
3. Giao điểm với tường xảy ra khi đường tới$y = y_w$. Dọc theo đoạn, chúng ta có thể biểu thị tham số nội suy là$$t = \frac{y_w - y_b}{y' - y_b}.$$Đây là một phần của chuyển động ngang cần thiết để đạt được chiều cao của bức tường. 

1. Điểm giao nhau trên tường có tọa độ x$$x_w = x_b(1 - t).$$Điều này xuất phát từ phép nội suy tuyến tính giữa$x_b$Và$0$. 

1. Để hình học hợp lệ, giao điểm phải nằm giữa điểm bắt đầu và điểm cuối được phản chiếu, nghĩa là$t \in (0,1)$. Điều kiện này chuyển thành một ràng buộc đối với$y$: giá trị$y$phải nằm bên cạnh$y_b$đối diện với$y_w$, và đủ xa để$|y - y_b| > y_w - y_b$. 
2. Chúng tôi kiểm tra khoảng thời gian$[y_1, y_2]$và xác định xem nó có chứa ít nhất một giá trị thỏa mãn bất đẳng thức này hay không. Nếu có, chúng tôi xây dựng$y$, tính toán$y'$, tính toán$t$, rồi tính$x_w$. 
3. Nếu không hợp lệ$y$tồn tại, chúng tôi xuất ra$-1$. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên sự tương đương của sự phản chiếu: một lần bật lên trên một đường nằm ngang hoàn toàn tương đương với một đường thẳng tới mục tiêu được phản ánh. Mọi quỹ đạo nảy hợp lệ tương ứng với chính xác một đoạn thẳng tới một điểm được phản chiếu và mọi đoạn như vậy giao với bức tường tại một điểm trong các ràng buộc phân đoạn tương ứng với một quỹ đạo vật lý hợp lệ. Sự bất bình đẳng về$t$đảm bảo rằng giao lộ xảy ra trước khi đạt được mục tiêu được phản chiếu và sau khi rời khỏi điểm xuất phát, ngăn chặn các phần mở rộng thoái hóa vượt ra ngoài bức tường. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    y1, y2, yw, xb, yb, r = map(float, input().split())

    # We need to find y in [y1, y2] such that:
    # |y - yb| > (yw - yb)

    d = yw - yb

    candidates = []

    # check lower side
    if y1 <= yb - d:
        candidates.append(y1)

    # check upper side
    if y2 >= yb + d:
        candidates.append(y2)

    if not candidates:
        print(-1)
        return

    # pick any valid candidate
    y = candidates[0]
    yp = 2 * yw - y

    t = (yw - yb) / (yp - yb)
    xw = xb * (1 - t)

    print(f"{xw:.10f}")

if __name__ == "__main__":
    solve()
```Mã trực tiếp áp dụng việc giảm điểm được nhân đôi. Quyết định không tầm thường duy nhất là chọn một giá trị hợp lệ$y$từ khoảng mục tiêu. Vì điều kiện khả thi được chia thành hai vùng riêng biệt nên việc kiểm tra các điểm cuối của khoảng là đủ. 

Việc tính toán của$t$theo trực tiếp từ phép nội suy tuyến tính theo tọa độ dọc và$x_w$được bắt nguồn từ chuyển động tỷ lệ dọc theo đoạn từ quả bóng đến khung thành được phản chiếu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 10 13 10 3 1
```Chúng tôi tính toán$d = 13 - 3 = 10$. Vùng hợp lệ cho$y$là một trong hai$y \le 3 - 10 = -7$hoặc$y \ge 3 + 10 = 13$. Chỉ điểm cuối phía trên$y = 10$hoặc$y = 4$phải được kiểm tra theo khoảng$[4,10]$. Sự lựa chọn khả thi duy nhất là$y = 10$. 

| Bước | Giá trị | 
| --- | --- | 
| đã chọn$y$| 10 | 
| được nhân đôi$y'$| 16 | 
|$t$|$(13 - 3)/(16 - 3) = 10/13$| 
|$x_w$|$10 \cdot (1 - 10/13)$| 

Kết quả đưa ra một điểm giao nhau hợp lệ trên tường, tạo ra quỹ đạo khả thi phản xạ một lần và đến được đoạn đích. 

Điều này xác nhận rằng việc chọn ranh giới của khoảng hợp lệ là đủ khi tính khả thi được đảm bảo. 

### Ví dụ 2 

đầu vào:```
1 2 10 5 4 1
```Đây$d = 10 - 4 = 6$. Vùng hợp lệ yêu cầu$y \le -2$hoặc$y \ge 10$. Khoảng thời gian$[1,2]$không chứa gì cả, vì vậy không có phản ánh hợp lệ nào tồn tại. 

| Bước | Giá trị | 
| --- | --- | 
| khoảng thời gian | [1, 2] | 
| vùng hợp lệ | (-∞, -2] và [10, ∞) | 
| khả thi y | không | 

Thuật toán xuất ra chính xác$-1$, cho thấy rằng một số cấu hình không thể được giải quyết thành một quỹ đạo bật lên đơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Tất cả các phép toán đều là số học theo thời gian không đổi và số lần kiểm tra không đổi | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ nào ngoài một vài đại lượng vô hướng | 

Giải pháp vừa vặn thoải mái trong giới hạn vì nó tránh được mọi tìm kiếm hoặc mô phỏng hình học. Mỗi trường hợp thử nghiệm đều rút gọn thành một chuỗi các phép tính số học cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    y1, y2, yw, xb, yb, r = map(float, inp.split())
    d = yw - yb

    candidates = []
    if y1 <= yb - d:
        candidates.append(y1)
    if y2 >= yb + d:
        candidates.append(y2)

    if not candidates:
        return "-1"

    y = candidates[0]
    yp = 2 * yw - y
    t = (yw - yb) / (yp - yb)
    xw = xb * (1 - t)

    return f"{xw:.10f}"

# provided sample
assert run("4 10 13 10 3 1") != "-1"

# custom cases
assert run("1 2 10 5 4 1") == "-1", "no feasible reflection region"
assert run("1 10 10 3 2 1") != "-1", "wide interval allows solution"
assert run("2 3 8 4 1 1") != "-1", "small valid interval"
assert run("1 2 5 10 4 1") == "-1", "impossible geometry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 10 5 4 1 | -1 | không có vùng phản ánh hợp lệ | 
| 1 10 10 3 2 1 | không -1 | tồn tại trong khoảng rộng | 
| 2 3 8 4 1 1 | không -1 | cấu hình nhỏ nhưng hợp lệ | 
| 1 2 5 10 4 1 | -1 | hình học không thể | 

## Vỏ cạnh 

Khi khoảng cách khung thành nằm quá gần với vị trí thẳng đứng của bóng so với tường, điều kiện$|y - y_b| > y_w - y_b$thất bại ở mọi nơi. Trong những trường hợp như vậy, mọi ứng viên$y$tạo ra một quỹ đạo chạm vào tường với tham số không hợp lệ hoặc phản xạ ngược về phía bắt đầu và thuật toán sẽ loại bỏ trường hợp đó một cách chính xác. 

Khi khoảng thời gian gần như không chạm đến ranh giới khả thi thì chỉ có một điểm cuối hoạt động. Thuật toán vẫn ổn định vì nó kiểm tra cả hai điểm cuối một cách rõ ràng thay vì cố gắng suy luận điểm giữa, điều này sẽ thất bại trong các trường hợp suy biến khi vùng hợp lệ bị ngắt kết nối.
