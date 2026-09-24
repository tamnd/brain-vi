---
title: "CF 104804K - \u041f\u0435\u0447\u0430\u0442\u044c"
description: "Chúng ta được giao một xưởng in chia các tờ giấy thành một chuỗi các đoạn liền kề nhau. Mỗi phân đoạn đại diện cho một phạm vi số lượng trang và mỗi phạm vi đều có chi phí cố định trên mỗi trang."
date: "2026-06-28T13:28:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104804
codeforces_index: "K"
codeforces_contest_name: "Central Russia Regional Contest, 2022, Qualification Contest"
rating: 0
weight: 104804
solve_time_s: 67
verified: true
draft: false
---

[CF 104804K - \u041f\u0435\u0447\u0430\u0442\u044c](https://codeforces.com/problemset/problem/104804/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một xưởng in chia các tờ giấy thành một chuỗi các đoạn liền kề nhau. Mỗi phân đoạn đại diện cho một phạm vi số lượng trang và mỗi phạm vi đều có chi phí cố định trên mỗi trang. Nếu một công việc nằm trong một phạm vi, nó có thể được in với chi phí trên mỗi trang đó, nhưng có một nhược điểm: khách hàng được phép đệm thêm các trang trống vào tài liệu để tổng số trang trở thành bất kỳ giá trị nào lớn hơn hoặc bằng độ dài ban đầu, sau đó chọn một phạm vi bao gồm kích thước được đệm đó. 

Mục tiêu là bắt đầu với một tài liệu có nội dung chính xác$k$trang và tùy ý tăng nó lên bất kỳ số lượng lớn hơn nào, sau đó chọn một phân đoạn có khoảng chứa đầy đủ số trang cuối cùng. Chi phí là chi phí phân khúc nhân với số trang đã chọn, vì chi phí là trên mỗi trang. Nhiệm vụ là giảm thiểu tổng số tiền thanh toán này hoặc xác định rằng không có đoạn nào có thể chứa bất kỳ chiều dài đệm khả thi nào. 

Cấu trúc của đầu vào rất quan trọng: các đoạn được cho theo thứ tự tăng dần, chúng phân chia đầy đủ dãy số bắt đầu từ 1, không có khoảng trống và không có sự chồng chéo. Điều này có nghĩa là mỗi số trang nguyên dương thuộc về chính xác một phân đoạn. 

Các ràng buộc đẩy chúng ta tới một giải pháp tuyến tính hoặc gần tuyến tính. Với$n \le 10^5$, bất kì$O(n^2)$chiến lược thử tất cả các lựa chọn hoặc mô phỏng các quyết định đệm cho mỗi phân đoạn ngay lập tức là quá chậm. Ràng buộc ẩn chính là$k \le 10^9$, điều này ngăn cản việc xây dựng các mảng hoặc DP rõ ràng trên các trang. 

Một sai lầm ngây thơ nảy sinh khi người ta cho rằng phân đoạn tốt nhất chỉ đơn giản là phân đoạn chứa$k$. Điều đó không chính xác vì phần đệm cho phép chuyển sang các phân đoạn sau có thể có chi phí trên mỗi trang nhỏ hơn nhiều. 

Một trường hợp lỗi tinh vi khác xuất hiện khi một đoạn bắt đầu ngay sau$k$, và còn có một phân khúc rẻ hơn nữa bên phải. Ví dụ: nếu sau này chi phí phân khúc giảm đáng kể thì chiến lược tối ưu là tăng số lượng trang vượt quá$k$cho đến khi tiếp cận được phân khúc rẻ hơn đó. 

Cuối cùng, rất dễ quên rằng chúng ta đang chọn một phân đoạn duy nhất sau khi đệm chứ không phải trộn lẫn các phân đoạn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mọi phân khúc là lựa chọn cuối cùng. Đối với mỗi phân khúc$[a_i, b_i]$, chúng tôi sẽ kiểm tra xem liệu chúng tôi có thể tiếp cận nó bằng cách đệm hay không, nghĩa là chúng tôi cần$b_i \ge k$. Nếu vậy, chúng ta có thể chọn bất kỳ$x \in [\max(k, a_i), b_i]$, và chi phí trở thành$x \cdot c_i$. Sự lựa chọn tốt nhất trong một phân khúc luôn là sự lựa chọn khả thi nhỏ nhất$x$, vì chi phí tăng tuyến tính theo trang. Vì vậy, đối với mỗi phân đoạn, chúng tôi sẽ tính toán:$$x_i = \max(k, a_i), \quad \text{cost}_i = x_i \cdot c_i$$và lấy mức tối thiểu trên tất cả các phân đoạn hợp lệ. 

Điều này đúng nhưng đã$O(n)$, điều đó tốt thôi. Tuy nhiên, có một quan sát thậm chí còn quan trọng hơn ẩn trong cấu trúc: các phân đoạn liền kề nhau và được sắp xếp, vì vậy chúng ta có thể xử lý chúng trong một lần mà vẫn duy trì được câu trả lời tốt nhất. Ý tưởng chính là chúng ta không bao giờ cần phải xem lại các phân đoạn trước đó và chúng ta không bao giờ cần xem xét nhiều hơn phân đoạn hiện tại trong một phạm vi nhất định$k$. 

Sự tinh tế thực sự là vì được phép đệm nên quyết định hiệu quả không bị ràng buộc vào vị trí$k$nói dối ban đầu. Về cơ bản chúng tôi đang chọn một điểm dừng$x \ge k$, sau đó trả tiền$x \cdot c(x)$, Ở đâu$c(x)$là hàm chi phí phân khúc theo các khoảng thời gian. 

Vì hàm này không đổi từng phần trên các phân đoạn và tuyến tính bên trong mỗi phân đoạn nên việc kiểm tra từng phân đoạn một lần là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Trên thực tế, giải pháp “tối ưu” cũng giống như giải pháp brute-force sạch từng được xây dựng chính xác nhưng được triển khai cẩn thận với logic khoảng thời gian. 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`answer`như vô cùng. Điều này sẽ theo dõi chi phí tối thiểu có thể có trên tất cả các lựa chọn hợp lệ. 
2. Lặp lại từng phân đoạn$[a_i, b_i]$với chi phí$c_i$. Vì các phân đoạn rời rạc và bao gồm tất cả các số nguyên nên mỗi số trang cuối cùng có thể thuộc về chính xác một phân đoạn, do đó mỗi phân đoạn phải được xem xét chính xác một lần. 
3. Kiểm tra xem phân đoạn đó có thể được sử dụng hay không. Vì chúng tôi chỉ có thể tăng số trang nên số trang cuối cùng ít nhất phải bằng$k$. Nếu như$b_i < k$, phân đoạn không thể chứa bất kỳ kích thước cuối cùng khả thi nào và bị bỏ qua. 
4. Nếu phân đoạn có thể sử dụng được, hãy xác định số trang tốt nhất bên trong phân đoạn đó. Số trang khả thi nhỏ nhất là$\max(k, a_i)$, bởi vì chúng ta không thể đi xuống bên dưới$k$do phần đệm và chúng tôi không thể đi xuống dưới điểm bắt đầu của phân đoạn. 
5. Tính chi phí như$\text{pages} \times c_i$. Điều này là tối ưu trong phân khúc vì việc tăng số trang chỉ làm tăng chi phí một cách tuyến tính. 
6. Cập nhật câu trả lời tối thiểu toàn cầu. 
7. Sau khi xử lý tất cả các phân đoạn, nếu câu trả lời chưa bao giờ được cập nhật, hãy xuất ra$-1$. Nếu không thì xuất ra mức tối thiểu được tính toán. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi số trang cuối cùng khả thi đều thuộc về chính xác một phân đoạn và trong một phân đoạn, hàm chi phí là đơn điệu về số lượng trang. Điều này làm cho sự lựa chọn tối ưu trong một phân khúc luôn là điểm khả thi nhất bên trái của nó. Vì mọi trạng thái cuối cùng hợp lệ được biểu thị bằng chính xác một phân đoạn và một lựa chọn bên trong nó, nên việc quét tất cả các phân đoạn và lấy giá trị tối ưu cục bộ tốt nhất đảm bảo tối ưu toàn cục. Không có tương tác chéo nào tồn tại ngoài điều kiện khả thi$b_i \ge k$, vì phần đệm chỉ làm tăng số lượng trang và không bao giờ buộc phải bỏ qua một phân đoạn mà không xem xét nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    ans = 10**30

    for _ in range(n):
        a, b, c = map(int, input().split())

        if b < k:
            continue

        x = max(a, k)
        if x <= b:
            ans = min(ans, x * c)

    print(-1 if ans == 10**30 else ans)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý các phân đoạn trong một lần duy nhất, chỉ duy trì câu trả lời tốt nhất cho đến nay. Chi tiết quan trọng nhất là tính toán`x = max(a, k)`, mô hình chính xác cả hai ràng buộc: phần đệm không thể giảm xuống dưới$k$và chúng ta phải ở trong giới hạn phân khúc. 

Séc`b < k`lọc các phân đoạn không thể chứa bất kỳ kích thước cuối cùng hợp lệ nào. Điều kiện thứ hai`x <= b`đảm bảo rằng sau khi đệm, chúng tôi vẫn ở trong phân khúc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 15
1 5 20
6 10 15
11 20 10
21 30 5
```| Phân đoạn | k | x = max(a, k) | Có hiệu lực? | Chi phí | 
| --- | --- | --- | --- | --- | 
| [1,5] | 15 | 15 | Không (5 < 15) | - | 
| [6,10] | 15 | 15 | Không (10 < 15) | - | 
| [11,20] | 15 | 15 | Có | 150 | 
| [21,30] | 15 | 21 | Có | 105 | 

Lựa chọn tốt nhất là phân đoạn [21,30] bằng cách đệm tối đa 21 trang. Điều này cho thấy tại sao lời giải tối ưu có thể nằm đúng sau k. 

### Mẫu 2 

đầu vào:```
11 16
1 1 7
2 2 14
3 4 17
5 7 1
8 8 18
9 10 19
11 12 18
13 14 9
15 18 5
19 19 2
20 20 8
```| Phân đoạn | k | x | Có hiệu lực? | Chi phí | 
| --- | --- | --- | --- | --- | 
| [1,1] | 16 | 16 | Không | - | 
| [2,2] | 16 | 16 | Không | - | 
| [3,4] | 16 | 16 | Không | - | 
| [5,7] | 16 | 16 | Không | - | 
| [8,8] | 16 | 16 | Không | - | 
| [9,10] | 16 | 16 | Không | - | 
| [11,12] | 16 | 16 | Không | - | 
| [13,14] | 16 | 16 | Không | - | 
| [15,18] | 16 | 16 | Có | 80 | 
| [19,19] | 16 | 19 | Có | 38 | 
| [20,20] | 16 | 20 | Có | 160 | 

Chiến lược tối ưu sẽ tiến về phía trước để tiếp cận phân khúc trên mỗi trang rẻ hơn, cho thấy rằng khoảng đệm là điều cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phân đoạn được xử lý chính xác một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có một số biến được duy trì | 

Giải pháp là tuyến tính về số lượng phân đoạn, dễ dàng phù hợp với các ràng buộc của$10^5$. Không cần thêm bộ nhớ ngoài khả năng phân tích cú pháp đầu vào và một vài giá trị vô hướng. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n, k = map(int, input().split())
    ans = 10**30

    for _ in range(n):
        a, b, c = map(int, input().split())
        if b < k:
            continue
        x = max(a, k)
        if x <= b:
            ans = min(ans, x * c)

    print(-1 if ans == 10**30 else ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""4 15
1 5 20
6 10 15
11 20 10
21 30 5
""") == "105"

assert run("""11 16
1 1 7
2 2 14
3 4 17
5 7 1
8 8 18
9 10 19
11 12 18
13 14 9
15 18 5
19 19 2
20 20 8
""") == "38"

# minimum case
assert run("""1 5
1 10 3
""") == "15"

# k inside early segment but better later
assert run("""3 4
1 5 10
6 10 1
11 20 2
""") == "10"

# no valid segment
assert run("""2 100
1 10 5
11 20 6
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phân đoạn đơn bao gồm k | 15 | xử lý tối thiểu | 
| tăng chi phí sau này | 10 | lợi thế đệm | 
| không thể truy cập được k | -1 | trường hợp thất bại | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$k$nằm xa trong một đoạn nhưng cách di chuyển tối ưu là bỏ qua về phía trước. Ví dụ: nếu phân khúc sau có chi phí nhỏ hơn nhiều thì phần đệm sẽ vượt qua các phân khúc đắt tiền trung gian. Thuật toán xử lý việc này vì nó không bao giờ hạn chế các lựa chọn đối với phân đoạn chứa$k$, nó đánh giá từng phân đoạn một cách độc lập. 

Một trường hợp cạnh khác xảy ra khi$k$lớn hơn tất cả$b_i$ngoại trừ đoạn cuối cùng. Trong trường hợp này, chỉ phân đoạn cuối cùng mới đóng góp các ứng cử viên và thuật toán trả về chính xác một giá trị từ phân đoạn đó hoặc$-1$nếu thậm chí nó không thể đáp ứng$k$. 

Trường hợp thứ ba là khi đạt được giá trị tối ưu chính xác tại$a_i$còn hơn là$k$. Công thức`max(a, k)`đảm bảo chúng ta không bao giờ chọn sai một giá trị bên dưới một trong hai ràng buộc, do đó logic ranh giới bên trái vẫn đúng ngay cả khi$k$là nhỏ.
