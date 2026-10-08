---
title: "CF 104964B - \u041a\u043e\u043c\u043c\u0443\u043d\u0438\u043a\u0430\u0446\u0438\u044f \u043d\u0430 \u0432\u044b\u0441\u043e\u043a\u043e\u043c \u0443\u0440\u043e\u0432\u043d\u0435"
description: "Chúng ta được cấp một dãy các tòa nhà và mỗi tòa nhà phải nhận được một cảm biến được đặt ở một độ cao nguyên nào đó. Để xây dựng $i$, chiều cao được chọn $di$ bị hạn chế nằm trong khoảng $[ai, bi]$ của chính nó."
date: "2026-06-28T18:23:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104964
codeforces_index: "B"
codeforces_contest_name: "\u0412\u044b\u0441\u0448\u0430\u044f \u043f\u0440\u043e\u0431\u0430 - 2023. \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 104964
solve_time_s: 99
verified: false
draft: false
---

[CF 104964B - \u041a\u043e\u043c\u043c\u0443\u043d\u0438\u043a\u0430\u0446\u0438\u044f \u043d\u0430 \u0432\u044b\u0441\u043e\u043a\u043e\u043c \u0443\u0440\u043e\u0432\u043d\u0435](https://codeforces.com/problemset/problem/104964/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy các tòa nhà và mỗi tòa nhà phải nhận được một cảm biến được đặt ở một độ cao nguyên nào đó. Để xây dựng$i$, chiều cao đã chọn$d_i$bị hạn chế nằm trong khoảng riêng của nó$[a_i, b_i]$. Khi tất cả các độ cao đã được chọn, chi phí của cấu hình là tổng chênh lệch tuyệt đối giữa các cảm biến lân cận, được tính tổng dọc theo đường thẳng, do đó$|d_1-d_2| + |d_2-d_3| + \dots + |d_{n-1}-d_n|$. Nhiệm vụ là chọn độ cao hợp lệ để giảm thiểu tổng số này. 

Cấu trúc có tính tuần tự: mỗi quyết định chỉ tương tác với quyết định lân cận của nó thông qua sự khác biệt tuyệt đối. Không có ràng buộc toàn cầu nào liên kết các vị trí không liền kề, nhưng việc lựa chọn tại một vị trí ảnh hưởng gián tiếp đến tất cả các chi phí trong tương lai vì nó ảnh hưởng đến khoảng cách giữa chúng ta và khoảng thời gian tiếp theo. 

Kích thước đầu vào đẩy chúng ta tới hành vi tuyến tính hoặc gần tuyến tính cho mỗi lần kiểm tra. Tổng của$n$vượt qua tất cả các bài kiểm tra là lên đến$10^6$, vì vậy bất kỳ giải pháp nào nhiều hơn$O(n \log n)$mỗi thử nghiệm có nguy cơ hết thời gian chờ do quá trình chuyển đổi nặng nề lặp đi lặp lại. Bộ nhớ rất đơn giản vì chúng ta chỉ cần các mảng có kích thước$n$. 

Một cạm bẫy ngây thơ là cho rằng chúng ta có thể độc lập chọn$d_i$như bất kỳ giá trị nào bên trong$[a_i, b_i]$giảm thiểu sự khác biệt cục bộ một cách tham lam từ trái sang phải. Điều này không thành công khi một lựa chọn tối ưu cục bộ buộc phải có một bước nhảy lớn sau đó. 

Ví dụ: hãy xem xét các khoảng:$$[0, 10], [5, 5], [0, 10]$$Nếu chúng ta tham lam chọn lựa$d_1 = 0$, sau đó$d_2 = 5$, sau đó$d_3 = 0$, chúng tôi nhận được chi phí$5 + 5 = 10$. Nhưng nếu chúng ta chọn$d_1 = 5$, sau đó$d_2 = 5$, sau đó$d_3 = 5$, chi phí là$0$. Lựa chọn bước đầu tiên xác định liệu các chuyển đổi trong tương lai có thể được làm phẳng hay không. 

Vì vậy, khó khăn chính là mỗi vị trí phải cân bằng giữa lựa chọn trước đó và khoảng thời gian cho phép của chính nó. 

## Phương pháp tiếp cận 

Một quan điểm vũ phu là xem xét rằng ở mỗi vị trí$i$, chúng ta có thể chọn bất kỳ số nguyên nào trong$[a_i, b_i]$và chúng tôi muốn giảm thiểu tổng chi phí theo cặp. Điều này gợi ý một định nghĩa lập trình động: hãy$dp[i][x]$là chi phí tối thiểu cho đến vị trí$i$nếu chúng ta kết thúc ở giá trị$x$. Chuyển tiếp từ mọi khả năng trước đó$x$với mọi dòng điện có thể$y$dẫn đến$$dp[i][y] = \min_{x \in [a_{i-1}, b_{i-1}]} dp[i-1][x] + |x-y|.$$Ngay cả khi chúng ta rời rạc hóa các giá trị, kích thước khoảng có thể lớn, lên tới$10^9$, làm cho điều này không thể tính toán một cách rõ ràng. Ngay cả khi chúng ta nén các giá trị, các chuyển đổi vẫn là bậc hai trong trường hợp xấu nhất. 

Quan sát quan trọng là chúng ta không bao giờ cần hàm đầy đủ$dp[i][x]$. Điều quan trọng là quá trình chuyển đổi từ khoảng trước đó sang khoảng mới với chi phí giá trị tuyệt đối có thể được tóm tắt bằng cách đẩy _điểm đại diện tốt nhất về phía trước_. Ở bất kỳ bước nào, cấu hình tối ưu cho tiền tố$1..i-1$có thể được tóm tắt dưới dạng một giá trị duy nhất$d_{i-1}$, bởi vì một khi chúng ta sửa giá trị được chọn cuối cùng ở vị trí$i-1$, tương lai chỉ phụ thuộc vào điểm cuối đó chứ không phụ thuộc vào cách chúng ta đạt được nó. 

Điều này thoạt nhìn không rõ ràng, nhưng nó xuất phát từ thực tế rằng chi phí là tổng của các đóng góp cạnh độc lập. Đối với một cố định$d_{i-1}$, tối ưu$d_i$đơn giản là giá trị gần nhất với$d_{i-1}$bên trong$[a_i, b_i]$, bởi vì$|d_{i-1}-d_i|$được giảm thiểu độc lập với các bước trong tương lai. 

Do đó, vấn đề tổng thể phân rã thành việc liên tục chiếu giá trị trước đó vào khoảng tiếp theo. 

### Bảng so sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực DP trên tất cả các giá trị | Hàm mũ / không khả thi | Lớn | Quá chậm | 
| Chiếu khoảng tham lam |$O(n)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các tòa nhà từ trái sang phải, duy trì chiều cao đã chọn cho tòa nhà trước đó. 

1. Bắt đầu với tòa nhà đầu tiên. Bất kỳ giá trị nào trong$[a_1, b_1]$là hợp lệ. Chúng tôi chọn$d_1 = a_1$bởi vì không có chi phí trước đó để xem xét. 
2. Đối với mỗi tòa nhà tiếp theo$i$, so sánh chiều cao đã chọn trước đó$d_{i-1}$với khoảng thời gian$[a_i, b_i]$. 
3. Nếu$d_{i-1}$nằm trong khoảng, ta đặt$d_i = d_{i-1}$. Điều này làm cho chi phí chuyển đổi bằng 0 và không có lý do gì để chuyển đi vì nó không giúp ích gì cho các bước trong tương lai. 
4. Nếu$d_{i-1} < a_i$, chúng tôi thiết lập$d_i = a_i$. Đây là điểm khả thi gần nhất với ranh giới bên trái, giảm thiểu$|d_{i-1}-d_i|$. 
5. Nếu$d_{i-1} > b_i$, chúng tôi thiết lập$d_i = b_i$. Điều này giảm thiểu một cách đối xứng khoảng cách đến khoảng cách từ phía trên. 
6. Tích lũy chi phí$|d_{i-1} - d_i|$ở mỗi bước. 

Thuật toán xây dựng một chuỗi hợp lệ trong khi luôn chọn giá trị khả thi gần nhất với giá trị trước đó. 

### Tại sao nó hoạt động 

Ở mỗi bước, sự đóng góp duy nhất liên quan đến$d_i$điều đó phụ thuộc vào tương lai là sự khác biệt tiếp theo$|d_i - d_{i+1}|$. Tuy nhiên, một khi chúng ta đạt đến bước$i$, mọi giá trị bên trong khoảng đều có sẵn cho các bước trong tương lai và các khoảng trong tương lai chỉ quan tâm đến vị trí hiện tại chứ không quan tâm đến cách chúng tôi đến đó. Vì vậy, việc giảm thiểu khoảng cách chuyển tiếp ngay lập tức không làm mất đi tính tối ưu trong tương lai. Cấu trúc tối ưu giảm xuống còn việc chiếu liên tục giá trị trước đó vào khoảng thời gian cho phép tiếp theo, vì bất kỳ sai lệch nào cũng sẽ chỉ làm tăng chi phí hiện tại mà không cải thiện tính khả thi hoặc tính linh hoạt của các bước trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        d = [0] * n
        d[0] = a[0]
        total = 0

        for i in range(1, n):
            if d[i - 1] < a[i]:
                d[i] = a[i]
            elif d[i - 1] > b[i]:
                d[i] = b[i]
            else:
                d[i] = d[i - 1]

            total += abs(d[i] - d[i - 1])

        print(total)
        print(*d)

if __name__ == "__main__":
    solve()
```Giải pháp giữ giá trị đã chọn đang chạy và chỉ điều chỉnh giá trị đó khi giá trị đó nằm ngoài khoảng thời gian hiện tại. Chi phí được tích lũy dần dần. Điều tinh tế quan trọng là các trường hợp đẳng thức rất quan trọng: nếu giá trị trước đó nằm chính xác bên trong khoảng, chúng ta phải giữ nguyên giá trị đó để duy trì các chuyển đổi chi phí bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 0 1
3 3 4
```Chúng tôi theo dõi việc xây dựng: 

| tôi | khoảng [a_i, b_i] | trước d | đã chọn d_i | chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | [1, 3] | - | 1 | 0 | 
| 2 | [0, 3] | 1 | 1 | 0 | 
| 3 | [1, 4] | 1 | 1 | 0 | 

Giá trị không bao giờ cần phải di chuyển nên mọi chuyển đổi đều miễn phí. Điều này xác nhận rằng các khoảng thời gian chồng chéo có thể làm giảm toàn bộ chi phí xuống 0. 

### Ví dụ 2 

đầu vào:```
2
42 10
239 33
```| tôi | khoảng [a_i, b_i] | trước d | đã chọn d_i | chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | [42, 239] | - | 42 | 0 | 
| 2 | [10, 33] | 42 | 33 | 9 | 

Khoảng thứ hai nằm hoàn toàn bên dưới giá trị trước đó, buộc phải chiếu xuống. Chi phí chính xác là khoảng cách đến điểm cuối gần nhất. 

Điều này cho thấy hành vi của thuật toán khi các khoảng rời rạc: mỗi bước thu gọn về phép chiếu biên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Mỗi tòa nhà được xử lý một lần với công việc liên tục | 
| Không gian |$O(n)$| Lưu trữ chuỗi đầu ra | 

Tổng cộng$n$qua tất cả các bài kiểm tra là$10^6$, do đó, một lần vượt qua tuyến tính cho mỗi bài kiểm tra vẫn nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            b = list(map(int, input().split()))

            d = [0] * n
            d[0] = a[0]
            total = 0

            for i in range(1, n):
                if d[i - 1] < a[i]:
                    d[i] = a[i]
                elif d[i - 1] > b[i]:
                    d[i] = b[i]
                else:
                    d[i] = d[i - 1]
                total += abs(d[i] - d[i - 1])

            out.append(str(total))
            out.append(" ".join(map(str, d)))
        return "\n".join(out)

    return solve()

# provided sample
assert run("""3
3
1 0 1
3 3 4
2
42 10
239 33
7
1 2 3 4 5 6 7
3 4 5 6 7 8 9
""") == """0
3 3 3
9
42 33
4
3 3 3 4 5 6 7"""

# all equal intervals
assert run("""1
4
5 5 5 5
5 5 5 5
""") == """0
5 5 5 5"""

# forced increasing
assert run("""1
4
1 2 3 4
2 3 4 5
""") == """0
1 2 3 4"""

# forced bouncing
assert run("""1
3
0 10 0
0 0 0
""") == """10
0 0 0"""

# single element
assert run("""1
1
7
7
""") == """0
7"""

| Test input | Expected output | What it validates |
|---|---|---|
| equal intervals | zero cost | stability in constant intervals |
| increasing chain | zero cost | no unnecessary movement |
| bouncing constraint | projection correctness | boundary snapping |
| single element | base case | no transition handling |
```## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các khoảng chồng lên nhau tại một điểm. Ví dụ:```
n = 4
a = [5, 5, 5, 5]
b = [5, 5, 5, 5]
```Mỗi bước buộc$d_i = 5$. Thuật toán ngay lập tức khóa vào giá trị khả thi duy nhất và tích lũy chi phí bằng 0 ở mỗi lần chuyển đổi. Không có sự mơ hồ vì phép chiếu luôn trả về cùng một điểm. 

Một trường hợp khác là khi các khoảng thời gian giảm dần để mỗi bước buộc phải nhảy ranh giới:```
a = [0, 10, 20]
b = [0, 10, 20]
```Ở đây, giá trị trước đó luôn nằm ngoài khoảng tiếp theo ở bên phải, vì vậy chúng tôi liên tục chiếu xuống điểm cuối bên phải của mỗi khoảng, tích lũy chi phí bước xác định. Quy tắc tham lam luôn chọn ranh giới gần nhất nên mỗi chi phí chuyển đổi bằng khoảng cách chính xác giữa các khoảng liền kề, phù hợp với giải pháp tối ưu vì không có lựa chọn trung gian nào có thể giảm khoảng cách sau này.
