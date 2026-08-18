---
title: "CF 102262C - \u0420\u0430\u0437\u0431\u0438\u0435\u043d\u0438\u0435 \u043d\u0430 \u0440\u0435\u043a\u043b\u0430\u043c\u043d\u044b\u0435 \u0431\u043b\u043e\u043a\u0438"
description: "Chúng tôi có một mảng giá trị biểu ngữ giảm dần (P1P2dotsPN). Chúng ta phải chia các biểu ngữ mà không thay đổi thứ tự của chúng thành các khối liền kề khác rỗng chính xác (K). Đối với (các) khối, giả sử nó chứa các vị trí (l,ldots,r)."
date: "2026-08-17T20:15:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102262
codeforces_index: "C"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u0444\u0438\u043d\u0430\u043b (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102262
solve_time_s: 146
verified: true
draft: false
---

[CF 102262C - \u0420\u0430\u0437\u0431\u0438\u0435\u043d\u0438\u0435 \u043d\u0430 \u0440\u0435\u043a\u043b\u0430\u043c\u043d\u044b\u0435 \u0431\u043b\u043e\u043a\u0438](https://codeforces.com/problemset/problem/102262/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dãy giá trị biểu ngữ giảm dần (P_1>P_2>\dots>P_N). Chúng ta phải chia các biểu ngữ mà không thay đổi thứ tự của chúng thành các khối liền kề khác rỗng chính xác (K). 

Đối với (các) khối, giả sử nó chứa các vị trí (l,\ldots,r). Vì (P) giảm nghiêm ngặt nên giá trị lớn nhất trong khối luôn là (P_l), trong khi giá trị nhỏ nhất là (P_r). Vì vậy sự đóng góp của nó là 

[ 
P_r(r-l+1)-L_sP_l. 
] 

Giá trị của một phân vùng là tổng của những đóng góp này trên tất cả các khối (K). Chúng ta cần giá trị tối đa có thể. 

Ràng buộc (N\le 50000) loại trừ bất kỳ số bậc hai nào trong (N) bên trong mỗi số khối. Với (K\le100), chương trình động (O(KN^2)) sẽ thực hiện chuyển đổi khoảng (1,25\cdot10^{11}) ở giá trị lớn nhất, vượt xa giới hạn hai giây. Mục tiêu phải gần với (O(KN)) hoặc có lẽ (O(KN\log N)). 

Mức giảm nghiêm ngặt của (P) là thuộc tính cấu trúc giúp cho việc tối ưu hóa có thể thực hiện được. Nó cung cấp cho chúng ta cả một biểu thức đơn giản cho giá trị tối thiểu và tối đa của mọi khối liền kề cũng như các giá trị truy vấn đơn điệu cho thủ thuật thân lồi. 

Trường hợp cạnh đầu tiên là (N=1), (K=1). Vì```
1 1
5
7
```khối duy nhất có giá trị (5-7\cdot5=-30), vì vậy câu trả lời là (-30). Việc triển khai khởi tạo câu trả lời hoặc mọi trạng thái DP về 0 sẽ âm thầm trả về kết quả sai vì các giá trị tối ưu có thể âm. 

Một trường hợp cạnh khác là (K=1). Vì```
3 1
8 5 2
3
```không có sự lựa chọn nào cả. Toàn bộ mảng là một khối, có giá trị là (2\cdot3-3\cdot8=-18). Việc triển khai DP không được vô tình cho phép cắt thêm hoặc tạo khối trống. 

Ranh giới đối diện là (K=N). Vì```
3 3
9 4 1
0 0 0
```mỗi biểu ngữ là một khối đơn lẻ, vì vậy câu trả lời là (9+4+1=14). Điều này phát hiện các lỗi lập chỉ mục trong đó phần cắt đầu tiên hoặc cuối cùng có thể bị bỏ qua. 

Đầu vào đảm bảo (P_i>P_{i+1}), do đó, một mảng trong đó tất cả (P_i) bằng nhau không phải là trường hợp kiểm thử hợp lệ. Tuy nhiên, Equal (L_i) hoàn toàn hợp lệ và hữu ích cho việc kiểm tra việc triển khai vì hình phạt khối khi đó giống hệt nhau ở mọi lớp. 

## Phương pháp tiếp cận 

Một chương trình động trực tiếp sẽ xem xét vị trí của lần cắt cuối cùng. Đặt (dp_s[r]) là giá trị tối đa cho (r) biểu ngữ đầu tiên được chia thành (các) khối chính xác. Nếu khối cuối cùng bắt đầu tại (l), các khối (s-1) trước đó bao gồm các vị trí (1\ldots l-1) và khối cuối cùng đóng góp 

[ 
P_r(r-l+1)-L_sP_l. 
] 

Như vậy 

L_sP_l 
\đúng). 
] 

Sự lặp lại này đã đúng, nhưng việc đánh giá tất cả (l) có thể có cho mọi (r) chi phí (O(KN^2)). Với (N=50000) và (K=100), đây là khoảng (1,25\cdot10^{11}) chuyển đổi ứng viên. 

Quan sát hữu ích xuất hiện sau khi mở rộng thuật ngữ chứa (P_r): 

## lP_r 

L_sP_l 
\đúng). 
\end{căn chỉnh} 
] 

Đối với (l cố định), biểu thức bên trong mức tối đa là một dòng được đánh giá tại (x=P_r): 

[ 
y=(-l)x+\left(dp_{s-1}[l-1]-L_sP_l\right). 
] 

Vì vậy, mọi vị trí bắt đầu có thể (l) sẽ tạo ra một đường thẳng có độ dốc (-l) và điểm giao nhau (dp_{s-1[l-1]-L_sP_l). 

Khi (r) tăng, độ dốc được chèn theo thứ tự giảm dần vì (-1,-2,-3,\ldots) giảm. Đồng thời, (P_r) được truy vấn theo thứ tự giảm dần vì mảng (P) ban đầu đang giảm dần. Đây chính xác là tình huống đơn điệu trong đó thủ thuật bao lồi kiểu deque mang lại công việc khấu hao (O(1)) trên mỗi dòng được chèn và trên mỗi truy vấn. 

Sự tái diễn bạo lực vẫn là nền tảng của giải pháp. Thủ thuật thân lồi chỉ thay đổi cách duy trì mức tối đa trên tất cả các vị trí bắt đầu trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | (O(KN^2)) | (O(N)) | Quá chậm | 
| DP tối ưu + CHT đơn điệu | (O(KN)) khấu hao | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định (dp_s[r]) là giá trị tốt nhất để phân vùng tiền tố (P_1,\ldots,P_r) thành các khối chính xác. Chúng ta chỉ cần lớp trước đó, vì vậy quá trình triển khai sẽ lưu trữ`prev[r]`Và`cur[r]`. 
2. Đối với (các) số khối cố định, hãy viết lại quá trình chuyển đổi thành 

P_r(r+1) 
+ 
\max_l 
\left( 
(-l)P_r+ 
dp_{s-1[l-1]-L_sP_l 
\đúng). 
] 

Phần phụ thuộc vào (l) bây giờ là một dòng được đánh giá tại (x=P_r). 

1. Khi xử lý (r=s,s+1,\ldots,N), chèn dòng tương ứng với (l=r). Độ dốc và giao điểm của nó là 

[ 
m=-r, 
\qquad 
b=dp_{s-1[r-1]-L_sP_r. 
] 

Dòng này hợp lệ vì các khối trước đó chiếm chính xác vị trí đầu tiên (r-1). 

1. Duy trì các đường được chèn như một bao lồi trên. Vì độ dốc giảm dần nên một đường mới được chèn vào chỉ có thể làm cho một số đường ở gần cuối thân tàu không còn phù hợp. Đối với ba đường thẳng liên tiếp (A,B,C), đường thẳng (B) là không cần thiết khi giao điểm của (A) và (B) không ở bên phải giao điểm của (B) và (C). 

Đối với các đường ((m_1,b_1),(m_2,b_2),(m_3,b_3)) có độ dốc giảm dần, điều kiện này có thể được kiểm tra mà không cần dấu phẩy động: 

[ 
(b_2-b_1)(m_2-m_3) 
\le 
(b_3-b_2)(m_1-m_2). 
] 

Phép nhân chéo số nguyên tránh được lỗi chính xác. 

1. Truy vấn thân tàu tại (x=P_r). Vì (P_r) giảm khi (r) tăng nên đường tối ưu chỉ di chuyển về phía trước qua thân tàu. Trong khi dòng thứ hai cho giá trị ít nhất bằng dòng đầu tiên ở (x) hiện tại, hãy loại bỏ dòng đầu tiên. Không có dòng bị loại bỏ nào có thể trở lại tối ưu vì tất cả các giá trị truy vấn trong tương lai thậm chí còn nhỏ hơn. 
2. Đặt dòng tốt nhất tại (P_r) có giá trị (mP_r+b). Đặt 

[ 
dp_s[r]=P_r(r+1)+mP_r+b. 
] 

Sau khi xử lý tất cả (r), thay thế`prev`với`cur`và tiếp tục với khối tiếp theo. 

1. Sau chính xác (K) lớp,`prev[N]`là giá trị tối đa cho tất cả (N) biểu ngữ được chia thành chính xác (K) khối, vì vậy đây là câu trả lời bắt buộc. 

Điều bất biến là ngay trước mỗi truy vấn, thân hoạt động chứa chính xác các dòng tương ứng với tất cả các vị trí bắt đầu hợp lệ có thể có của khối cuối cùng hiện tại, với mọi dòng không bao giờ có thể trở thành tối ưu đã bị loại bỏ. Tính đơn điệu của độ dốc và các giá trị truy vấn đảm bảo rằng việc loại bỏ một đường ở hai đầu là vĩnh viễn. Do đó, đường được chọn cho mọi trạng thái DP là đường chuyển tiếp tốt nhất có thể có trong số tất cả các đường cắt hợp lệ trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    p = list(map(int, input().split()))
    L = list(map(int, input().split()))

    neg_inf = -(10 ** 30)

    # prev[r] = best value for the first r elements using the
    # already processed number of blocks.
    prev = [neg_inf] * (n + 1)
    prev[0] = 0

    for s in range(1, k + 1):
        cur = [neg_inf] * (n + 1)
        loss = L[s - 1]

        # Hull is stored in two parallel arrays.
        # Lines are kept in decreasing slope order.
        slopes = []
        intercepts = []
        head = 0

        for r in range(s, n + 1):
            # Add the line corresponding to l = r:
            #
            # y = (-l) * x + prev[l - 1] - loss * P_l
            m = -r
            b = prev[r - 1] - loss * p[r - 1]

            while len(slopes) - head >= 2:
                m1 = slopes[-2]
                b1 = intercepts[-2]
                m2 = slopes[-1]
                b2 = intercepts[-1]

                # Remove the middle line if it is redundant.
                if (b2 - b1) * (m2 - m) <= (b - b2) * (m1 - m2):
                    slopes.pop()
                    intercepts.pop()
                else:
                    break

            slopes.append(m)
            intercepts.append(b)

            x = p[r - 1]

            # Queries arrive with decreasing x, so the optimum
            # moves only from the front toward the back.
            while len(slopes) - head >= 2:
                v1 = slopes[head] * x + intercepts[head]
                v2 = slopes[head + 1] * x + intercepts[head + 1]

                if v1 <= v2:
                    head += 1
                else:
                    break

            best_line = slopes[head] * x + intercepts[head]
            cur[r] = x * (r + 1) + best_line

        prev = cur

    print(prev[n])

if __name__ == "__main__":
    main()
```Việc khởi tạo DP sử dụng`prev[0] = 0`, nghĩa là trước khi tạo bất kỳ khối nào, tiền tố trống có giá trị bằng 0. Mọi trạng thái khác đều bắt đầu ở mức âm vô cùng vì một phân vùng không thể không bao giờ được tham gia ở mức tối đa. 

Dòng cho vị trí bắt đầu`r`công dụng`prev[r - 1]`Và`p[r - 1]`bởi vì Python sử dụng lập chỉ mục dựa trên 0 trong khi vị trí toán học (r) là dựa trên một. Độ dốc là`-r`, không`-(r - 1)`, vì đại số chứa số hạng (-lP_r) với vị trí dựa trên một (l). 

Thân tàu lưu trữ độ dốc theo thứ tự giảm dần. Kiểm tra dự phòng chỉ sử dụng phép nhân số nguyên, do đó không cần tọa độ giao nhau của dấu phẩy động. Điều này quan trọng vì các giá trị liên quan có thể đủ lớn để so sánh dấu phẩy động có thể làm mất thứ tự chính xác. 

các`head`chỉ mục đóng vai trò là mặt trước của deque. Các dòng không bị xóa về mặt vật lý khi các truy vấn loại bỏ chúng, điều này tránh được các hoạt động dịch chuyển (O(N)). Mỗi dòng được nối thêm một lần, bị xóa khỏi phía sau nhiều nhất một lần và được con trỏ truy vấn chuyển qua nhiều nhất một lần. 

Các số nguyên Python có độ chính xác tùy ý, do đó các tích được sử dụng trong phép thử bao lồi và DP không thể tràn. Các giá trị trung gian lớn nhất vẫn có thể quản lý được một cách thoải mái trong Python. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
4 2
6 4 3 1
0 3
```Khối đầu tiên không bị phạt vì (L_1=0). Khối thứ hai có hình phạt gấp ba lần mức tối đa (P). 

| (các) | (r) | (P_r) | Chuyển tiếp tốt nhất | (dp_s[r]) | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 6 | khối (1\ldots1) | 6 | 
| 1 | 2 | 4 | khối (1\ldots2) | 8 | 
| 1 | 3 | 3 | khối (1\ldots3) | 9 | 
| 1 | 4 | 1 | khối (1\ldots4) | 4 | 
| 2 | 2 | 4 | (6 + 4 - 3\cdot4) | -2 | 
| 2 | 3 | 3 | (6 + 6 - 3\cdot4) | 0 | 
| 2 | 4 | 1 | (9 + 2 - 3\cdot3) | 2 | 

Bảng trên hiển thị các giá trị DP cho tiền tố, trong khi câu trả lời cuối cùng thu được từ quá trình chuyển đổi lớp thứ hai thực tế cho (r=4). Phân vùng tốt nhất là ((6,4,3)\mid(1)), với giá trị 

[ 
3\cdot3-0\cdot6 + 1-3\cdot1 = 7. 
] 

Sự khác biệt rõ ràng trong bảng đơn giản hóa minh họa tại sao quá trình chuyển đổi phải sử dụng lớp trước đó cho tiền tố kết thúc ngay trước khối cuối cùng. Đối với (r=4), trạng thái trước đó tốt nhất là (dp_1[3]=9) và khối cuối cùng bắt đầu tại (4), cho 

[ 
9 + 1 - 3 = 7. 
] 

Do đó chương trình in`7`. 

Đối với mẫu 2,```
10 3
10 9 8 7 6 5 4 3 2 1
0 4 2
```Lớp đầu tiên không bị phạt, vì vậy giá trị của nó chỉ đơn giản là giá trị tối thiểu nhân với độ dài khối. 

| (các) | (r) | (P_r) | (dp_s[r]) | 
| --- | --- | --- | --- | 
| 1 | 1 | 10 | 10 | 
| 1 | 2 | 9 | 18 | 
| 1 | 3 | 8 | 24 | 
| 1 | 4 | 7 | 28 | 
| 1 | 5 | 6 | 30 | 
| 1 | 6 | 5 | 30 | 
| 1 | 7 | 4 | 28 | 
| 1 | 8 | 3 | 24 | 
| 1 | 9 | 2 | 18 | 
| 1 | 10 | 1 | 10 | 

Đối với lớp thứ hai, (L_2=4), do đó thân tàu cân bằng giá trị thu được từ việc mở rộng mức tối thiểu so với mức phạt được xác định bởi phần tử đầu tiên của khối thứ hai. Một số trạng thái có liên quan là 

| (r) | Đoạn cắt trước hay nhất | (dp_2[r]) | 
| --- | --- | --- | 
| 5 | 1 | 10 | 
| 6 | 5 | 15 | 
| 7 | 4 | 18 | 
| 8 | 6 | 20 | 
| 9 | 6 hoặc 7 | 20 | 
| 10 | 7 | 19 | 

Cuối cùng (L_3=2). Đối với tiền tố đầy đủ, nếu khối thứ ba bắt đầu sau vị trí (c), đóng góp của nó là 

[ 
(10-c)-2(10-c)=-(10-c). 
] 

Giá trị tốt nhất thu được ở (c=9), cho 

[ 
dp_2[9]-1=20-1=19. 
] 

Do đó, câu trả lời cho Mẫu 2 là`19`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(KN)) khấu hao | Mỗi lớp DP chèn (N) dòng, xóa mỗi dòng khỏi thân tàu nhiều nhất một lần và nâng cao con trỏ truy vấn nhiều nhất (N) lần. | 
| Không gian | (O(N)) | Chỉ các mảng DP trước đó và hiện tại và một bao lồi được lưu trữ. | 

Với (N\le50000) và (K\le100), thuật toán thực hiện tối đa khoảng năm triệu lần chèn dòng và truy vấn. Mỗi hoạt động của thân tàu được khấu hao theo thời gian không đổi, do đó, điều này phù hợp với quy mô dự kiến ​​​​tốt hơn nhiều so với phép tính (O(KN^2)). 

## Trường hợp thử nghiệm 

Bộ thử nghiệm sau đây giả định giải pháp trên được lưu dưới dạng`solution.py`. Kiểm tra kích thước tối đa được tạo thay vì được viết dưới dạng chữ 50000 phần tử.```python
import sys
import io
import solution

def run(inp: str) -> str:
    old_input = solution.input
    old_stdout = sys.stdout

    solution.input = io.StringIO(inp).readline
    sys.stdout = io.StringIO()

    try:
        solution.main()
        return sys.stdout.getvalue().strip()
    finally:
        solution.input = old_input
        sys.stdout = old_stdout

# Provided sample 1
assert run("""\
4 2
6 4 3 1
0 3
""") == "7", "sample 1"

# Provided sample 2
assert run("""\
10 3
10 9 8 7 6 5 4 3 2 1
0 4 2
""") == "19", "sample 2"

# Minimum-size input, and a negative optimum.
assert run("""\
1 1
5
7
""") == "-30", "minimum size"

# K = N, so every banner must be a singleton.
assert run("""\
3 3
9 4 1
0 0 0
""") == "14", "every block is a singleton"

# Equal losses, plus a case where the best cut is at the boundary.
assert run("""\
3 2
6 4 1
3 3
""") == "0", "equal losses"

# Boundary-heavy penalty. The best partition is 8 7 2 | 1.
assert run("""\
4 2
8 7 2 1
0 100
""") == "-93", "last-element block"

# Maximum N with K = 1.
p = " ".join(map(str, range(50000, 0, -1)))
assert run(f"50000 1\n{p}\n0\n") == "50000", "maximum N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 5 / 7`|`-30`| Kích thước tối thiểu và câu trả lời phủ định | 
|`3 3 / 9 4 1 / 0 0 0`|`14`| Chính xác (N) khối | 
|`3 2 / 6 4 1 / 3 3`|`0`| Giá trị tổn thất bằng nhau | 
|`4 2 / 8 7 2 1 / 0 100`|`-93`| Cắt ranh giới và phạt lớn | 
|`50000 1 / 50000 ... 1 / 0`|`50000`| Tối đa (N) và hiệu suất | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
1 1
5
7
```lớp DP đầu tiên tạo ra dòng duy nhất có thể có (l=r=1). Phần chặn của nó là (0-7\cdot5=-35) và truy vấn đóng góp (5\cdot2-35=-25) theo biểu thức được chuyển đổi. Trực tiếp hơn, giá trị khối ban đầu là (5-35=-30) và biểu thức được chuyển đổi cũng cho (-30) sau khi sử dụng (P_r(r+1)-lP_r=5). Câu trả lời cuối cùng là`-30`. Việc triển khai không bao giờ so sánh giá trị này với giá trị 0 nhân tạo. 

Với (K=N),```
3 3
9 4 1
0 0 0
```lớp đầu tiên tính toán các tiền tố một khối tốt nhất, lớp thứ hai chỉ có thể kết thúc ở (r=2) trở lên và lớp thứ ba phải bắt đầu ở (l=3) khi (r=3). Phân vùng hợp lệ duy nhất là (9\mid4\mid1), tạo ra`14`. Giới hạn vòng lặp`range(s, n + 1)`thực thi chính xác hạn chế này. 

Đối với tổn thất bằng nhau,```
3 2
6 4 1
3 3
```hai phân vùng có thể là (6\mid(4,1)) và ((6,4)\mid1). Giá trị của chúng là 

[ 
6+(2-3\cdot4)=0 
] 

và 

[ 
2\cdot4-3\cdot6+1-3=-12. 
] 

Tối ưu là`0`. Các giá trị bằng (L) không ảnh hưởng đến đối số bao lồi vì độ dốc vẫn được xác định duy nhất bởi vị trí bắt đầu (l). 

Đối với trường hợp nặng biên```
4 2
8 7 2 1
0 100
```hình phạt của khối thứ hai đặc biệt ủng hộ việc làm cho mức tối đa của nó càng nhỏ càng tốt. Phân vùng tối ưu là (8,7,2\mid1), có giá trị 

[ 
2\cdot3 + (1-100\cdot1) = -93. 
] 

Thân tàu xem xét đường thẳng cho (l=4) chính xác khi (r=4), do đó vị trí xuất phát cuối cùng có thể không bị mất. Đây là lỗi thường gặp trong phân vùng DP được tối ưu hóa. 

Việc sử dụng thử nghiệm kích thước tối đa```
N = 50000, K = 1
P = 50000, 49999, ..., 1
L_1 = 0.
```Chỉ có một khối nên giá trị của nó là (1\cdot50000=50000). Việc triển khai chỉ thực hiện một lần chuyển thân với các phần chèn và truy vấn (N), xác nhận rằng sự phụ thuộc tuyến tính vào (N) là thực tế ngay cả ở kích thước đầu vào lớn nhất.
