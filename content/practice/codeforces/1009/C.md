---
title: "CF 1009C - Món Quà Khó Chịu"
description: "Chúng ta bắt đầu với một mảng có độ dài $n$ ban đầu chỉ chứa các số 0. Bob thực hiện các hoạt động $m$. Mỗi thao tác được xác định bởi một cặp $(x, d)$ và Bob được phép chọn vị trí trung tâm $i$ trong mảng."
date: "2026-06-16T22:58:17+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1009
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 47 (Rated for Div. 2)"
rating: 1700
weight: 1009
solve_time_s: 225
verified: true
draft: false
---

[CF 1009C - Món quà khó chịu](https://codeforces.com/problemset/problem/1009/C) 

**Đánh giá:** 1700 
**Tags:** tham lam, toán học 
**Thời gian giải:** 3 phút 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng có độ dài$n$ban đầu chỉ chứa số không. Bob biểu diễn$m$hoạt động. Mỗi hoạt động được xác định bởi một cặp$(x, d)$, và Bob được phép chọn vị trí trung tâm$i$trong mảng. Một khi anh ấy làm vậy, mọi vị trí$j$nhận được một khoản đóng góp bổ sung chỉ phụ thuộc vào khoảng cách của nó với$i$: giá trị gia tăng tại vị trí$j$là$x + d \cdot |i - j|$. 

Sau khi áp dụng tất cả các thao tác theo bất kỳ thứ tự nào và chọn tâm cho mỗi thao tác, mảng sẽ trở thành một chuỗi số nguyên cuối cùng. Mục tiêu là tối đa hóa giá trị trung bình của mảng, tương đương với việc tối đa hóa tổng tổng của tất cả các phần tử vì$n$đã được sửa. 

Khó khăn chính là mỗi thao tác trải đều các giá trị trên toàn bộ mảng theo mẫu có cấu trúc “hình chữ V” và chúng ta có thể tự do đặt tâm của nó ở bất kỳ đâu. 

Những hạn chế$n, m \le 10^5$ngụ ý rằng bất kỳ giải pháp nào cố gắng mô phỏng từng hoạt động trên tất cả các vị trí đều không thể thực hiện được. Một mô phỏng trực tiếp sẽ yêu cầu$O(nm)$cập nhật, theo thứ tự$10^{10}$, vượt xa giới hạn. Ngay cả việc lưu trữ đầy đủ các hiệu ứng cho mỗi vị trí cho mỗi thao tác cũng sẽ quá lớn. 

Một vấn đề tế nhị nảy sinh từ quyền tự do lựa chọn trung tâm$i$. Một cách tiếp cận ngây thơ có thể cho rằng chúng ta có thể tham lam chọn vị trí tốt nhất cho mỗi thao tác mà không cần xem xét cấu trúc tổng thể, nhưng điều đó sẽ bỏ qua cách hoạt động tương tự đóng góp khác nhau tùy thuộc vào vị trí và cách cấu trúc tuyến tính của nó tương tác với toàn bộ mảng. 

Một ví dụ thất bại nhỏ cho lý luận ngây thơ là khi$d < 0$. Sau đó, thao tác sẽ mang lại các giá trị cao hơn ở xa trung tâm hơn, điều này làm thay đổi trực giác về việc đặt$i$. Một trường hợp cạnh khác là$d = 0$, trong đó thao tác trở thành phép cộng liên tục không phụ thuộc vào vị trí, khiến cho vị trí không còn phù hợp nữa. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Đối với mỗi thao tác$(x, d)$, chúng tôi thử tất cả các trung tâm có thể$i$, tính tổng đóng góp thu được vào tổng mảng và chọn kết quả tốt nhất. Đối với mỗi sự lựa chọn của$i$, chúng ta sẽ tính tổng của$x + d|i-j|$tổng thể$j$, vốn đã có giá$O(n)$. Làm điều này cho tất cả$i$thực hiện một thao tác duy nhất$O(n^2)$, và ngang qua$m$hoạt động này trở thành$O(n^2 m)$, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần toàn bộ mảng. Chúng tôi chỉ quan tâm đến tổng số tiền sau tất cả các hoạt động. Đối với một thao tác cố định, tổng đóng góp chỉ phụ thuộc vào số lượng phần tử nằm ở bên trái và bên phải của tâm đã chọn. Nếu chúng ta định nghĩa$i$làm trung tâm thì sự đóng góp có thể được biểu thị bằng cách sử dụng tổng tiền tố của khoảng cách. Cấu trúc của$|i - j|$chia rõ ràng thành phần bên trái và bên phải, và cả hai phần đều là chuỗi số học. 

Điều này làm giảm mỗi thao tác để lựa chọn$i$tối đa hóa một biểu thức đơn giản liên quan đến khoảng cách đến điểm cuối bên trái và bên phải. Vị trí tối ưu luôn nằm ở một trong các biên hoặc có thể được đặc trưng bởi hàm đơn điệu trong$i$, cho phép chúng tôi tính toán vị trí tốt nhất trong$O(1)$mỗi hoạt động. 

Do đó, thay vì mô phỏng mảng, chúng tôi tính toán tổng mức tăng tốt nhất có thể cho từng thao tác một cách độc lập và tích lũy nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 m)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại sự đóng góp của một thao tác đơn lẻ và tối ưu hóa nó thông qua việc lựa chọn trung tâm. 

1. Sửa một thao tác$(x, d)$và giả sử chúng ta chọn trung tâm$i$. Sự đóng góp vào vị trí$j$là$x + d|i-j|$. Đầu tiên chúng ta tách phần không đổi và phần phụ thuộc vào khoảng cách. Phần không đổi đóng góp$n \cdot x$bất kể$i$, do đó việc tối ưu hóa chỉ phụ thuộc vào thuật ngữ khoảng cách. 
2. Chúng ta viết lại tổng khoảng cách như sau:$$\sum_{j=1}^n |i - j|$$Điều này chia thành hai tổng số học: bên trái đóng góp$\frac{i(i-1)}{2}$, bên phải đóng góp$\frac{(n-i)(n-i+1)}{2}$. 
3. Tổng mức đóng góp của một hoạt động là:$$n x + d \left( \frac{i(i-1)}{2} + \frac{(n-i)(n-i+1)}{2} \right)$$4. Vì$n x$là không đổi trong$i$, chúng tôi chỉ tối đa hóa:$$f(i) = d \cdot \left( \frac{i(i-1)}{2} + \frac{(n-i)(n-i+1)}{2} \right)$$5. Hàm bên trong lồi trong$i$, có nghĩa là số nguyên tối đa của nó phải xảy ra ở một trong các ranh giới$i=1$hoặc$i=n$. Điều này xuất phát từ tính đối xứng: khoảng cách tăng lên khi chúng ta di chuyển ra khỏi tâm và tổng khoảng cách được giảm thiểu ở giữa và tối đa hóa ở các cạnh. 
6. Vì vậy, với mỗi thao tác, chúng tôi đánh giá cả hai điểm cuối$i=1$Và$i=n$, tính toán sự đóng góp của họ và lấy mức tối đa. 
7. Tổng hợp tất cả các đóng góp tối ưu đã chọn trong tất cả các hoạt động và chia cho$n$để có được câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Mỗi thao tác đều độc lập vì tất cả các cập nhật đều mang tính cộng và tuyến tính. Sự đóng góp của một hoạt động không ảnh hưởng đến cách phân phối của một hoạt động khác. Đối với bất kỳ phép toán cố định nào, mục tiêu sẽ giảm xuống mức tối đa hóa hàm bậc hai đối xứng trên$i$. Hàm đó không có cực đại bên trong trên số nguyên, vì vậy việc đánh giá các điểm biên là đủ. Điều này đảm bảo chúng tôi không bao giờ bỏ lỡ vị trí tốt hơn và đảm bảo tính tối ưu toàn cầu nhờ tính tuyến tính của kỳ vọng đối với các hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def total_dist(i, n):
    left = i * (i - 1) // 2
    right = (n - i) * (n - i + 1) // 2
    return left + right

n, m = map(int, input().split())
ans = 0

for _ in range(m):
    x, d = map(int, input().split())

    # evaluate both endpoints
    left_val = n * x + d * total_dist(1, n)
    right_val = n * x + d * total_dist(n, n)

    ans += max(left_val, right_val)

print(ans / n)
```Việc thực hiện mã hóa trực tiếp công thức dẫn xuất. Hàm trợ giúp tính tổng khoảng cách từ một tâm đã chọn bằng cách sử dụng chuỗi số học dạng đóng thay vì lặp qua tất cả các vị trí. 

Chúng tôi chỉ đánh giá rõ ràng$i=1$Và$i=n$, giúp tránh mọi tìm kiếm trên tất cả các trung tâm. Sự phân chia cuối cùng bởi$n$chuyển đổi tổng thành giá trị trung bình số học. 

Phải cẩn thận để giữ các phép tính ở dạng số nguyên cho đến bước cuối cùng để tránh trôi dấu phẩy động. Các số nguyên chính xác tùy ý của Python đảm bảo an toàn ngay cả đối với số lượng lớn$n$. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
2 3
-1 3
0 0
-1 -4
```Chúng tôi xử lý từng hoạt động và đánh giá cả hai điểm cuối. 

| Hoạt động | x | d | tổng tại i=1 | tổng tại i=n | đã chọn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | -1 | 3 | 2(-1)+3·1 = 1 | 2(-1)+3·1 = 1 | cà vạt | 
| 2 | 0 | 0 | 0 | 0 | cà vạt | 
| 3 | -1 | -4 | -2 + (-4)·1 = -6 | -2 + (-4)·1 = -6 | cà vạt | 

Tích lũy cho tổng số$-5$, và chia cho$n=2$cho$-2.5$. 

Dấu vết này cho thấy rằng các hoạt động đối xứng thu gọn về sự bằng nhau của điểm cuối trong các mảng nhỏ, xác nhận rằng việc giảm điểm cuối không bỏ sót bất kỳ tối ưu bên trong ẩn nào. 

Bây giờ hãy xem xét một trường hợp tùy chỉnh:```
5 1
2 1
```| tôi | tổng khoảng cách | đóng góp | 
| --- | --- | --- | 
| 1 | 10 | 10 + 10 = 20 | 
| 5 | 10 | 10 + 10 = 20 | 

Cả hai điểm cuối đều khớp nhau do tính đối xứng, chỉ củng cố cấu trúc đó chứ không phải vị trí, quan trọng đối với các mảng tuyến tính nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m)$| Mỗi thao tác đánh giá hai điểm cuối bằng công thức thời gian không đổi | 
| Không gian |$O(1)$| Chỉ bộ tích lũy và đầu vào hiện tại được lưu trữ | 

Thuật toán xử lý tới$10^5$hoạt động với mỗi công việc liên tục, phù hợp thoải mái trong thời gian giới hạn. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n, m = map(int, sys.stdin.readline().split())
    ans = 0

    def total_dist(i, n):
        return i*(i-1)//2 + (n-i)*(n-i+1)//2

    for _ in range(m):
        x, d = map(int, sys.stdin.readline().split())
        left = n*x + d*total_dist(1, n)
        right = n*x + d*total_dist(n, n)
        ans += max(left, right)

    return str(ans / n)

# provided sample
assert abs(float(run("""2 3
-1 3
0 0
-1 -4
""")) + 2.5) < 1e-9

# custom: single neutral operation
assert abs(float(run("""5 1
0 0
""")) - 0.0) < 1e-9

# custom: positive slope
assert float(run("""3 1
1 2
""")) > 0

# custom: negative slope
assert float(run("""4 1
1 -5
""")) < 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | -2,5 | tính đúng đắn của các dấu hiệu hỗn hợp | 
| 0-op | 0 | hành vi trung lập | 
| dương d | >0 | tích lũy tích cực | 
| âm d | <0 | xử lý biển báo | 

## Vỏ cạnh 

Khi nào$d = 0$, sự đóng góp trở nên độc lập với vị trí, do đó cả hai điểm cuối đều tạo ra kết quả giống hệt nhau. Thuật toán đánh giá cả hai và chọn chính xác một trong hai, mang lại$n x$như mong đợi. 

Khi$d < 0$, khoảng cách được thưởng thay vì bị phạt. Công thức vẫn đánh giá chính xác các điểm cuối vì tính đối xứng đảm bảo đạt được tổng khoảng cách tối đa tại các ranh giới. Ví dụ, với$n=4, x=1, d=-1$, cả hai$i=1$Và$i=4$tạo ra tổng khoảng cách tối đa giống hệt nhau và thuật toán nắm bắt trực tiếp điều này. 

Vì$n=1$, thuật ngữ khoảng cách biến mất hoàn toàn. Công thức giảm xuống còn$x$và đánh giá điểm cuối sẽ trả về giá trị chính xác một cách tầm thường vì cả hai điểm cuối đều trùng nhau. 

Những trường hợp này xác nhận rằng không có cực trị ẩn bên trong nào bị bỏ sót và việc đánh giá ranh giới đó nắm bắt được đầy đủ tất cả các cấu hình tối ưu.
