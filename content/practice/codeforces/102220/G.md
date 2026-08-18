---
title: "CF 102220G - Máy quét radar"
description: "Chúng tôi có (n) máy quét hình chữ nhật được căn chỉnh theo trục. Máy quét (i) hiện bao phủ mọi ô vuông có tọa độ nằm bên trong [ [ai,ci]times[bi,di]."
date: "2026-08-17T22:42:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102220
codeforces_index: "G"
codeforces_contest_name: "The 13th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102220
solve_time_s: 108
verified: true
draft: false
---

[CF 102220G - Máy quét radar](https://codeforces.com/problemset/problem/102220/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (n) máy quét hình chữ nhật được căn chỉnh theo trục. Máy quét (i) hiện bao phủ mọi ô vuông có tọa độ nằm bên trong 

[ 
[a_i,c_i]\times[b_i,d_i]. 
] 

Một lần di chuyển sẽ dịch chuyển toàn bộ máy quét đúng một ô vuông theo chiều ngang hoặc chiều dọc, do đó, việc di chuyển máy quét theo ((\Delta x,\Delta y)) sẽ tốn (|\Delta x|+|\Delta y|). Chúng tôi muốn định vị lại các máy quét sao cho có ít nhất một ô vuông thuộc về mỗi máy quét, đồng thời giảm thiểu tổng số lần di chuyển. 

Cách đơn giản hóa hữu ích đầu tiên là quên đi các vị trí máy quét cuối cùng và thay vào đó hãy chọn hình vuông mà tất cả các máy quét sẽ bao phủ. Khi hình vuông đó được cố định, mọi máy quét có thể được tối ưu hóa một cách độc lập. Câu hỏi còn lại là làm thế nào để chọn được hình vuông chung tốt nhất. 

Có tối đa (10^5) máy quét trong một trường hợp thử nghiệm và tổng số máy quét trên tất cả các trường hợp thử nghiệm tối đa là (10^6). Tọa độ có thể đạt tới (10^9), vì vậy các thuật toán liệt kê các vị trí có thể xảy ra ngay lập tức là không thể. Với giới hạn thời gian là hai giây, ngay cả phương pháp (O(n^2)) cũng quá đắt ở mức (n=10^5) và phương pháp tùy thuộc vào phạm vi tọa độ sẽ còn tệ hơn. Về cơ bản, chúng tôi cần tổng công việc (O(n\log n)) hoặc tốt hơn. 

Có một số trường hợp khó xử lý. Một máy quét đơn lẻ đã có sẵn một hình vuông có mái che chung, vì vậy```
1
1
5 7 9 11
```phải sản xuất```
0
```Việc triển khai bất cẩn luôn hướng tới tọa độ toàn cầu đã chọn có thể tạo ra câu trả lời tích cực mặc dù không cần thiết phải di chuyển. 

Hai hình chữ nhật có thể đã chồng lên nhau ngay cả khi không có hình chữ nhật nào nằm trong hình chữ nhật kia. Ví dụ,```
1
2
1 1 5 5
3 3 7 7
```có câu trả lời`0`, vì hình vuông ((3,3)) được bao phủ bởi cả hai. Cách tiếp cận chỉ dựa trên tâm hình chữ nhật có thể bỏ lỡ điều này. 

Chạm vào một ranh giới cũng được tính vì các ô vuông được bao phủ là các ô vuông nguyên và bao gồm các điểm cuối. Vì```
1
2
1 1 1 1
2 1 2 1
```câu trả lời là`1`: di chuyển máy quét một ô vuông theo chiều ngang. Việc coi các khoảng là mở sẽ loại bỏ các vị trí biên một cách không chính xác. 

Một hình chữ nhật có thể rộng hơn nhiều so với những hình chữ nhật khác. Coi như```
1
2
1 1 10 10
5 5 5 5
```Câu trả lời là`0`, vì máy quét thứ hai nằm bên trong máy quét thứ nhất. Phương pháp thay thế mỗi hình chữ nhật bằng tâm của nó sẽ làm mất chính xác thông tin khiến câu trả lời là 0. 

Cuối cùng, tọa độ có thể lớn bằng (10^9). Vì```
1
2
1 1 1 1
1000000000 1000000000 1000000000 1000000000
```câu trả lời là```
1999999998
```bởi vì một máy quét phải di chuyển (999999999) ô vuông theo mỗi hướng. Số nguyên Python xử lý việc này một cách an toàn, nhưng việc triển khai có chiều rộng cố định cần loại số nguyên đủ lớn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể chọn một hình vuông ứng cử viên ((x,y)), tính toán xem mỗi máy quét phải di chuyển bao xa để bao phủ hình vuông đó và giữ tổng chi phí tối thiểu. Điều này đúng vì mọi cấu hình cuối cùng hợp lệ đều có một số hình vuông được bao phủ chung, do đó, việc xem xét mọi hình vuông chung có thể cũng sẽ xem xét một cấu hình tối ưu. 

Vấn đề là số lượng ứng viên. Vì tất cả các tọa độ hữu ích đều nằm bên trong hộp giới hạn của hình chữ nhật đầu vào, nên vẫn có thể có (10^9) tọa độ (x) và (10^9) tọa độ (y). Điều đó mang lại tối đa (10^{18}) ô vuông ứng cử viên. Việc đánh giá tất cả (n) máy quét cho mỗi ứng viên sẽ yêu cầu tính toán khoảng cách lên tới (10^{23}) khi (n=10^5). Về mặt khái niệm, vũ lực có vẻ đơn giản nhưng nó gần như không thể thực hiện được. 

Quan sát quan trọng là hai tọa độ hoàn toàn độc lập. Giả sử hình vuông chung có tọa độ (x) (x). Đối với máy quét (i), khoảng ngang của nó là ([a_i,c_i]). Nếu (x) đã thuộc khoảng này thì không cần di chuyển theo chiều ngang. Nếu (x<a_i), máy quét phải di chuyển sang trái hoặc sang phải sao cho ranh giới bên trái của nó chạm tới (x), tính giá trị (a_i-x). Tương tự, nếu (x>c_i), chi phí là (x-c_i). 

Do đó, chi phí theo chiều ngang cho máy quét (i) chính xác là khoảng cách từ (x) đến khoảng ([a_i,c_i]): 

[ 
\operatorname{dist}(x,[a_i,c_i]). 
] 

Do đó, tổng chi phí theo chiều ngang là 

[ 
F(x)=\sum_i \operatorname{dist}(x,[a_i,c_i]). 
] 

Tọa độ dọc đưa ra một bài toán giống hệt nhau với các khoảng ([b_i,d_i]). Chúng ta có thể giải hai bài toán một chiều một cách độc lập và cộng đáp số của chúng. 

Câu hỏi còn lại là làm thế nào để giảm thiểu tổng khoảng cách đến các khoảng. Danh tính hữu ích là 

(r-l)+2\operatorname{dist}(x,[l,r]). 
] 

Sắp xếp lại, 

\frac{|x-l|+|x-r|-(r-l)}{2}. 
] 

Sau khi tính tổng số này trên tất cả các khoảng, các số hạng liên quan đến (x) trở thành tổng khoảng cách tuyệt đối từ (x) đến tất cả (2n) điểm cuối. Tổng khoảng cách tuyệt đối được giảm thiểu ở mức trung bình. Vì vậy, chúng ta chỉ cần điểm trung bình của tất cả các điểm cuối bên trái và bên phải. 

Brute-force hoạt động vì nó tìm kiếm hình vuông chung một cách rõ ràng. Nó thất bại vì không gian tìm kiếm là rất lớn. Quan sát cho thấy chi phí di chuyển được phân tách bằng tọa độ biến bài toán thành hai bài toán khoảng một chiều và việc nhận dạng điểm cuối làm giảm từng bài toán đó thành việc tìm trung vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\cdot 10^{18})) | (O(1)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi máy quét, hãy thu thập các điểm cuối theo chiều ngang (a_i,c_i) của nó vào một mảng và các điểm cuối theo chiều dọc (b_i,d_i) của nó vào một mảng khác. Đồng thời tích lũy tổng chiều rộng ngang (\sum(c_i-a_i)) và chiều cao dọc (\sum(d_i-b_i)). Hai trục có thể được xử lý độc lập vì di chuyển theo chiều ngang không bao giờ thay đổi phạm vi bao phủ theo chiều dọc. 
2. Sắp xếp các điểm cuối theo chiều ngang (2n). Chọn phần tử tại chỉ mục (n-1), sử dụng chỉ mục dựa trên 0, làm tọa độ đích ngang (x). Đây là một trong hai điểm cuối ở giữa nên nó là trung vị hợp lệ của tất cả (2n) điểm cuối. 
3. Tính tổng chênh lệch tuyệt đối giữa (x) và mọi điểm cuối theo chiều ngang. Nếu điểm cuối được sắp xếp là (e_1,\ldots,e_{2n}), chi phí di chuyển theo chiều ngang là 

[ 
\frac{\sum_j |e_j-x|-\sum_i(c_i-a_i)}{2}. 
] 

Việc trừ tổng chiều rộng sẽ loại bỏ chuyển động vốn đã tự do vì tọa độ mục tiêu có thể nằm bên trong hình chữ nhật thay vì buộc cả hai điểm cuối về phía nó.

1. Lặp lại phép tính tương tự cho các điểm cuối dọc, sử dụng (y) bằng điểm cuối dọc ở giữa và trừ đi tổng chiều cao của hình chữ nhật. 
2. Cộng chi phí theo chiều ngang và chiều dọc. Đây là số lần di chuyển máy quét riêng lẻ tối thiểu vì mỗi đơn vị dịch chuyển ngang hoặc dọc tương ứng với chính xác một thao tác được phép. 

### Tại sao nó hoạt động 

Đối với bất kỳ hình vuông mục tiêu cố định ((x,y)), máy quét (i) cần chính xác 

[ 
\operatorname{dist}(x,[a_i,c_i]) 
+ 
\operatorname{dist}(y,[b_i,d_i]) 
] 

di chuyển. Việc tổng hợp các máy quét sẽ tách mục tiêu thành phần chỉ (x) và phần chỉ (y), do đó việc tối thiểu hóa hai tọa độ một cách độc lập là tối ưu. 

Trong một khoảng ([l,r]), 

\frac{|x-l|+|x-r|-(r-l)}{2}. 
] 

Sau khi tính tổng tất cả các khoảng, phần duy nhất phụ thuộc vào (x) là tổng khoảng cách tuyệt đối từ (x) đến tất cả (2n) điểm cuối. Mỗi trung vị giảm thiểu một số tiền như vậy. Do đó, điểm cuối ở giữa được chọn sẽ đưa ra chi phí theo chiều ngang tối thiểu có thể và đối số tương tự sẽ đưa ra chi phí theo chiều dọc tối thiểu. Vì hai chi phí này độc lập nên tổng của chúng là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def axis_cost(endpoints, span_sum, n):
    endpoints.sort()
    median = endpoints[n - 1]

    distance_sum = sum(abs(x - median) for x in endpoints)
    return (distance_sum - span_sum) // 2

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        xs = []
        ys = []
        x_span = 0
        y_span = 0

        for _ in range(n):
            a, b, c, d = map(int, input().split())

            xs.append(a)
            xs.append(c)
            ys.append(b)
            ys.append(d)

            x_span += c - a
            y_span += d - b

        answer = axis_cost(xs, x_span, n)
        answer += axis_cost(ys, y_span, n)

        out.append(str(answer))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào lưu trữ chính xác hai điểm cuối cần thiết cho mỗi trục. Không cần phải giữ lại các hình chữ nhật ban đầu sau khi điểm cuối và chiều rộng của chúng đã được ghi lại.`axis_cost`đầu tiên sắp xếp các điểm cuối (2n) và chọn`endpoints[n - 1]`. Vì có giá trị (2n) nên cả hai vị trí ở giữa đều là trung vị hợp lệ. Việc chọn giá trị ở giữa thấp hơn sẽ tránh mọi nhu cầu về số học dấu phẩy động và đặc biệt thuận tiện với việc lập chỉ mục dựa trên số 0. 

biểu thức```
distance_sum - span_sum
```thực hiện danh tính từ bằng chứng. Đối với mỗi khoảng ([l,r]), hai đóng góp điểm cuối của nó là (|x-l|+|x-r|), trong khi chiều rộng (r-l) của nó bị trừ đi. Kết quả chính xác là gấp đôi khoảng cách cần thiết đến khoảng thời gian. Phép chia số nguyên cho hai là an toàn vì số lượng kết quả là số nguyên. 

Tọa độ đầu vào tối đa là (10^9), nhưng tổng số câu trả lời có thể vào khoảng (2\cdot10^{14}), vì vậy các số nguyên có độ chính xác tùy ý của Python rất thuận tiện. Không có nguy cơ tràn. 

Việc phân chia phải xảy ra sau khi toàn bộ số tiền được hình thành. Việc chia các đóng góp điểm cuối riêng lẻ trước tiên sẽ không chính xác vì hai số hạng điểm cuối và độ rộng khoảng phải được kết hợp trước khi chia cho hai. 

## Ví dụ đã hoạt động 

Đối với mẫu chính thức,```
1
2
2 2 3 3
4 4 5 5
```các tính toán ngang và dọc là giống hệt nhau. 

| Trục | Điểm cuối | Trung bình | Tổng khoảng cách điểm cuối | Tổng chiều rộng | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| X | 2, 3, 4, 5 | 3 | 4 | 2 | 1 | 
| Y | 2, 3, 4, 5 | 3 | 4 | 2 | 1 | 

Hình chữ nhật đầu tiên đã bao phủ ((3,3)). Hình chữ nhật thứ hai phải di chuyển một hình vuông sang trái và một hình vuông xuống dưới, nên tổng số là (1+1=2). Tính toán trung bình tạo ra kết quả chính xác như nhau. 

Đối với trường hợp một hình chữ nhật chứa một hình chữ nhật khác,```
1
2
1 1 10 10
5 5 5 5
```các điểm cuối và tính toán là: 

| Trục | Điểm cuối | Trung bình | Tổng khoảng cách điểm cuối | Tổng chiều rộng | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| X | 1, 5, 5, 10 | 5 | 9 | 9 | 0 | 
| Y | 1, 5, 5, 10 | 5 | 9 | 9 | 0 | 

Hình vuông mục tiêu có thể là ((5,5)), đã được bao phủ bởi cả hai máy quét. Phép trừ chiều rộng là yếu tố làm cho chi phí bằng 0 đối với hình chữ nhật lớn thay vì tính phí không chính xác cho khoảng cách đến các điểm cuối của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Mỗi trục sắp xếp (2n) điểm cuối, sau đó quét tuyến tính | 
| Không gian | (O(n)) | Hai mảng chứa (2n) điểm cuối mỗi mảng | 

Tổng số máy quét trong tất cả các trường hợp thử nghiệm tối đa là (10^6), do đó tổng công việc sắp xếp bị giới hạn bởi tổng số hạng tương ứng của (O(n\log n)). Việc triển khai không bao giờ liệt kê tọa độ, ô vuông mục tiêu hoặc chuyển động của máy quét, do đó giới hạn tọa độ của (10^9) không ảnh hưởng đến thời gian chạy. 

Việc sử dụng bộ nhớ là tuyến tính theo số lượng máy quét. Với giới hạn bộ nhớ 512 MB nhất định, việc lưu trữ các mảng điểm cuối là khả thi. 

## Trường hợp thử nghiệm```python
import sys
import io

input = sys.stdin.readline

def axis_cost(endpoints, span_sum, n):
    endpoints.sort()
    median = endpoints[n - 1]
    distance_sum = sum(abs(x - median) for x in endpoints)
    return (distance_sum - span_sum) // 2

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        xs = []
        ys = []
        x_span = 0
        y_span = 0

        for _ in range(n):
            a, b, c, d = map(int, input().split())
            xs.append(a)
            xs.append(c)
            ys.append(b)
            ys.append(d)
            x_span += c - a
            y_span += d - b

        ans = axis_cost(xs, x_span, n)
        ans += axis_cost(ys, y_span, n)
        out.append(str(ans))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = sys.stdin.readline

assert run(
    "1\n"
    "2\n"
    "2 2 3 3\n"
    "4 4 5 5\n"
) == "2\n", "provided sample"

assert run(
    "1\n"
    "1\n"
    "5 7 9 11\n"
) == "0\n", "single scanner needs no movement"

assert run(
    "1\n"
    "2\n"
    "1 1 5 5\n"
    "3 3 7 7\n"
) == "0\n", "already overlapping rectangles"

assert run(
    "1\n"
    "2\n"
    "1 1 1 1\n"
    "2 1 2 1\n"
) == "1\n", "touching boundary and off-by-one case"

assert run(
    "1\n"
    "2\n"
    "1 1 1 1\n"
    "1000000000 1000000000 1000000000 1000000000\n"
) == "1999999998\n", "maximum coordinate distance"

n = 100000
maximum_size = (
    "1\n"
    f"{n}\n"
    + ("1000000000 1000000000 1000000000 1000000000\n" * n)
)
assert run(maximum_size) == "0\n", "maximum n with identical rectangles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 5 7 9 11`|`0`| Đầu vào có kích thước tối thiểu và không chuyển động | 
|`1 / 2 / 1 1 5 5 / 3 3 7 7`|`0`| Nút giao thông hiện tại | 
|`1 / 2 / 1 1 1 1 / 2 1 2 1`|`1`| Ranh giới bao gồm và lập chỉ mục trung bình | 
|`1 / 2 / 1 1 1 1 / 1000000000 1000000000 1000000000 1000000000`|`1999999998`| Giá trị tọa độ tối đa và câu trả lời lớn | 
|`1 / 100000 / identical maximum-coordinate rectangles`|`0`| Tối đa (n), giá trị lặp lại và hiệu suất | 

## Vỏ cạnh 

Đối với một máy quét duy nhất,```
1
1
5 7 9 11
```điểm cuối theo chiều ngang là (5,9), do đó trung vị được chọn là (5). Tổng khoảng cách tuyệt đối của chúng là (4), chính xác bằng chiều rộng hình chữ nhật (9-5=4), cho chi phí ngang bằng 0. Phép tính theo chiều dọc hoạt động giống hệt nhau, vì vậy câu trả lời cuối cùng là`0`. Thuật toán không bao giờ giả định rằng có ít nhất hai máy quét. 

Đối với các hình chữ nhật đã chồng chéo,```
1
2
1 1 5 5
3 3 7 7
```các điểm cuối theo chiều ngang là (1,3,5,7) và việc chọn (x=3) sẽ cho tổng khoảng cách điểm cuối là (8), trong khi tổng chiều rộng là (4+4=8). Chi phí theo chiều ngang bằng không. Trục tung giống hệt nhau nên đáp án cuối cùng là 0. Nội thất hình chữ nhật không cần phải có ranh giới giống nhau, chỉ cần có một hình vuông có mái che chung. 

Đối với liên hệ ranh giới,```
1
2
1 1 1 1
2 1 2 1
```các điểm cuối theo chiều ngang là (1,1,2,2). Trung vị được chọn là (1), cho tổng khoảng cách điểm cuối là (2). Cả hai hình chữ nhật đều có chiều rộng bằng 0, do đó chi phí theo chiều ngang là (2/2=1). Theo chiều dọc, mọi điểm cuối đều là (1), cho chi phí bằng 0. Như vậy câu trả lời là`1`. Việc sử dụng các khoảng đóng ([a_i,c_i]) và ([b_i,d_i]) được phản ánh trực tiếp trong công thức khoảng cách đến khoảng. 

Đối với một hình chữ nhật chứa một hình chữ nhật khác,```
1
2
1 1 10 10
5 5 5 5
```các điểm cuối theo chiều ngang là (1,5,5,10). Chọn trung vị dưới (5) sẽ cho khoảng cách (4,0,0,5), có tổng là (9). Hình chữ nhật lớn có chiều rộng (9), do đó chuyển động theo chiều ngang là ((9-9)/2=0). Tính toán tương tự áp dụng theo chiều dọc. Điều này chứng tỏ tại sao phải biểu diễn toàn bộ khoảng chứ không phải chỉ tâm của nó hoặc một điểm cuối. 

Để phân tách tọa độ lớn nhất,```
1
2
1 1 1 1
1000000000 1000000000 1000000000 1000000000
```điểm cuối theo chiều ngang là (1,1,10^9,10^9). Trung vị có thể là (1) hoặc (10^9) và chi phí theo chiều ngang là (999999999). Chi phí dọc là như nhau, cho (1999999998). Số học số nguyên của Python xử lý kết quả trực tiếp và công thức tránh mọi phép lặp tọa độ. 

Trường hợp khó triển khai nhất là số lượng điểm cuối chẵn. Luôn có chính xác (2n) điểm cuối nên có hai giá trị ở giữa. Bất kỳ điểm nào giữa chúng đều giảm thiểu tổng độ lệch tuyệt đối. Lựa chọn`endpoints[n - 1]`là đủ và vì mỗi điểm cuối là một số nguyên nên tọa độ đích được chọn cũng là tọa độ lưới hợp lệ. Không cần phải tính trung bình hai giá trị ở giữa, điều này có thể tạo ra tọa độ phân số không cần thiết.
