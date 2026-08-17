---
title: "CF 102279E - Nâng tầm để thống trị"
description: "Chúng tôi có (N) bộ tấn công. Bộ (i) chứa các đòn tấn công (Ai), và sau khi hoàn thành một bộ, thời gian cần thiết cho mỗi đòn tấn công trong tương lai chỉ có thể giảm xuống, trở thành mức tối thiểu của thời gian hiện tại và (Bi). Hiệp 1 đã hoàn thành nên thời gian tấn công ban đầu là (B1)."
date: "2026-08-16T19:14:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102279
codeforces_index: "E"
codeforces_contest_name: "HCW 19 Team Round (ICPC format)"
rating: 0
weight: 102279
solve_time_s: 125
verified: true
draft: false
---

[CF 102279E - Nâng cao để thống trị](https://codeforces.com/problemset/problem/102279/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (N) bộ tấn công. Bộ (i) chứa (A_i) các đòn tấn công và sau khi hoàn thành một bộ, thời gian cần thiết cho mỗi đòn tấn công trong tương lai chỉ có thể giảm xuống, trở thành mức tối thiểu của thời gian hiện tại và (B_i). Hiệp 1 đã hoàn thành nên thời gian tấn công ban đầu là (B_1). Chúng ta có thể chọn thứ tự hoàn thành các bộ còn lại. 

Mục tiêu là giảm thiểu tổng thời gian hoàn thành các hiệp từ (2) đến (N). Vì (B_N=0), sau khi hoàn thành bộ (N), mọi đòn tấn công còn lại sẽ không mất thời gian. Do đó, vấn đề thực sự là quyết định xem tập hợp nào sẽ được hoàn thành trước tập hợp (N) và theo thứ tự nào để bản thân tập hợp (N) đó trở nên rẻ nhất có thể. 

Các mảng có cấu trúc đặc biệt hữu ích. Số lần tấn công tăng lên đáng kể, (A_1<A_2<\dots<A_N), trong khi số lần tấn công giảm đáng kể, (B_1>B_2>\dots>B_N). Với (N\le 10^5), một chương trình động (O(N^2)) sẽ yêu cầu 

[ 
\frac{N(N-1)}2 
] 

chuyển tiếp, tức là (4.999.950.000) chuyển tiếp ở kích thước đầu vào tối đa. Điều đó vượt xa những gì giới hạn một giây có thể chịu đựng được. Chúng ta cần một nghiệm (O(N)) hoặc (O(N\log N)). 

Có một số trường hợp ranh giới trong đó việc giải thích tập hợp đầu tiên đã hoàn thành và tập hợp chi phí bằng 0 cuối cùng có ý nghĩa quan trọng. Với (N=1),```
1
1
0
```câu trả lời là`0`, vì chẳng còn gì để hoàn thành. Giải pháp cộng thêm chi phí của bộ ban đầu như một phần của câu trả lời là mô hình hóa quy trình không chính xác. 

Vì```
2
1 2
5 0
```câu trả lời là`10`. Hiệp 1 đã kết thúc và hiệp 2 yêu cầu hai đòn tấn công với tốc độ hiện tại là 5. Bản thân nó (B_2=0) chỉ ảnh hưởng đến tốc độ sau khi hoàn thành hiệp 2 nên không thể giải phóng hai đòn tấn công đó. 

Trường hợp cạnh thứ ba là bộ hoàn thành (N) ngay lập tức không phải lúc nào cũng tối ưu. Đối với mẫu đầu tiên, việc hoàn thành bộ 5 có chi phí ngay lập tức (6\cdot5=30), nhưng hoàn thành bộ 4 với chi phí đầu tiên (4\cdot5=20), sau đó đặt 5 chi phí (6\cdot1=6). Tổng cộng là 26, vì vậy chiến lược tối ưu sẽ cố tình trả tiền cho hiệp trung gian để giảm tốc độ trước khi xử lý hiệp cuối cùng. 

Cuối cùng, câu trả lời có thể vượt quá phạm vi số nguyên 32 bit. Với (N=100000), (A_i=i) và (B_i=N-i), câu trả lời là (N(N-1)=9,999,900,000). Số nguyên Python có độ chính xác tùy ý, do đó, điều này không yêu cầu xử lý đặc biệt khi triển khai. 

## Phương pháp tiếp cận 

Chương trình động lực mạnh xuất phát trực tiếp từ việc xem xét tập cuối cùng được hoàn thành trước tập (i). Giả sử tập đó (j<i) là tập cuối cùng được hoàn thành trước (i). Đến thời điểm đó, thời gian tấn công đã giảm xuống (B_j), do đó, chi phí hoàn thành bộ (i) (A_iB_j). Nếu (dp[j]) là chi phí tối thiểu cần thiết để đạt được trạng thái đó thì chi phí ứng cử viên là 

[ 
dp[j]+B_jA_i. 
] 

Như vậy 

[ 
dp[i]=\min_{1\le j<i}\left(dp[j]+B_jA_i\right), 
] 

với (dp[1]=0). Câu trả lời là (dp[N]). Sự giảm thiểu này là dạng tiêu chuẩn được sử dụng cho chương trình động đơn điệu này. 

Sự lặp lại là đúng vì một lịch trình tối ưu có thể được biểu diễn bằng các chỉ số của các tập hợp thực sự làm giảm thời gian tấn công hiện tại. Những chỉ số đó đang ngày càng tăng lên. Nếu (j) là chỉ số trước đó và (i) là chỉ số tiếp theo, thì tất cả các tập bị bỏ qua giữa chúng có thể được hoàn thành sau, sau khi thời gian tấn công thậm chí còn nhỏ hơn. Đặc biệt, sau khi tập (N) hoàn thành, tốc độ bằng 0, do đó những tập bị bỏ qua đó không đóng góp gì. Do đó, toàn bộ chi phí để đạt được (i) sẽ được tính bởi một trạng thái trước đó (j) và quá trình chuyển đổi (B_jA_i). 

Tính toán phép truy hồi này trực tiếp kiểm tra mọi (j<i) với mọi (i). Số lượng đánh giá chuyển tiếp chính xác là (N(N-1)/2), tức là khoảng năm tỷ cho (N=10^5). Công thức Brute-Force rất hữu ích vì nó bộc lộ cấu trúc của giải pháp tối ưu nhưng không thể sử dụng trực tiếp. 

Quan sát quan trọng là mọi chuyển đổi ứng viên đều có dạng 

[ 
dp[j]+B_jA_i. 
] 

Đối với một (j cố định), hãy coi đây là một đường thẳng 

[ 
y=B_jx+dp[j]. 
] 

Khi tính toán (dp[i]), chúng tôi truy vấn các dòng này tại (x=A_i). Đầu vào đảm bảo rằng độ dốc (B_j) đang giảm dần, trong khi tọa độ truy vấn (A_i) đang tăng lên. Đây chính xác là bối cảnh mà Thủ thuật thân lồi đơn điệu chỉ có thể duy trì những đường có thể trở nên tối ưu. 

Bởi vì các truy vấn chỉ di chuyển sang bên phải, khi dòng thứ hai trong thân không tệ hơn dòng đầu tiên tại điểm truy vấn hiện tại, thì dòng đầu tiên không bao giờ có thể trở lại tối ưu nữa. Tương tự như vậy, khi một đường mới làm cho đường giữa trở nên dư thừa giữa hai đường lân cận, thì đường giữa đó có thể bị loại bỏ vĩnh viễn. 

Thuật toán kết quả sẽ chèn mỗi dòng một lần, xóa mỗi dòng nhiều nhất một lần và xử lý mọi truy vấn một lần. Tổng công việc là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(N)) | Quá chậm | 
| Thủ thuật thân lồi tối ưu | (O(N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định (dp[i]) là thời gian tối thiểu cần thiết để đạt đến trạng thái tại đó tập (i) vừa được hoàn thành và thời gian tấn công của nó là (B_i). Bộ 1 đã hoàn thành nên (dp[1]=0). Câu trả lời cuối cùng là (dp[N]), bởi vì việc hoàn thành bộ (N) khiến mỗi lần tấn công sau đó không mất thời gian. 
2. Viết lại quá trình chuyển đổi thành 

[ 
dp[i]=\min_{j<i}\left(B_jA_i+dp[j]\right). 
] 

Với mọi (j) cố định, đây là một đường thẳng có độ dốc (B_j) và điểm giao nhau (dp[j]). Tính toán (dp[i]) có nghĩa là tìm giá trị dòng tối thiểu tại (x=A_i).

1. Lưu trữ các dòng ứng cử viên trong một deque. Các hệ số góc có thứ tự giảm dần vì (B_1>B_2>\dots>B_N). Các vị trí truy vấn có thứ tự tăng dần vì (A_1<A_2<\dots<A_N). 
2. Trước khi truy vấn (A_i), hãy so sánh hai dòng đầu tiên trong deque. Nếu dòng thứ hai có giá trị không lớn hơn giá trị đầu tiên tại (A_i), hãy xóa dòng đầu tiên. Vì đường đầu tiên có độ dốc lớn hơn và tất cả các giá trị (A) trong tương lai thậm chí còn lớn hơn nên nó không bao giờ có thể trở nên tốt hơn nữa. 
3. Sau khi loại bỏ tất cả các dòng lỗi thời ở phía trước, hãy đánh giá dòng còn lại đầu tiên tại (A_i). Giá trị này là (dp[i]), vì deque chứa chính xác các dòng ứng cử viên vẫn có thể tối ưu. 
4. Xây dựng đường dây mới 

[ 
y=B_ix+dp[i]. 
] 

Trước khi nối nó, hãy kiểm tra hai dòng cuối cùng và dòng mới. Nếu đường giữa nằm phía trên đường bao dưới thì nó không bao giờ có thể là mức tối thiểu cho bất kỳ truy vấn nào trong tương lai, vì vậy hãy loại bỏ nó. Kiểm tra này được thực hiện bằng cách sử dụng phép nhân chéo, tránh tính toán giao điểm dấu phẩy động. 

1. Nối dòng mới và tiếp tục. Mỗi dòng được chèn một lần và chỉ có thể được xóa khỏi một trong hai đầu một lần, do đó tổng số thao tác deque là (O(N)). 

Tại sao nó hoạt động: bất biến là deque chứa đường bao dưới của tất cả các dòng được tạo cho đến nay, theo thứ tự chúng trở nên tối ưu khi (x) tăng lên. Quy tắc truy vấn phía trước chỉ xóa một dòng sau khi dòng sau ít nhất đã tốt ở mức hiện tại (x) và dòng sau có độ dốc nhỏ hơn, do đó ít nhất nó vẫn tốt cho mọi tương lai (x). Quy tắc loại bỏ ngược loại bỏ một đường có khoảng tối ưu đã biến mất giữa các đường lân cận. Do đó, đường phía trước luôn thể hiện sự chuyển tiếp tối ưu cho dòng điện (A_i), do đó giá trị được tính toán chính xác là giá trị tối ưu của lập trình động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    if n == 1:
        print(0)
        return

    # Each line is represented as (slope, intercept).
    # We need minimum values, slopes are strictly decreasing,
    # and query x-values are strictly increasing.
    hull = [(b[0], 0)]
    head = 0

    dp = [0] * n

    def value(line, x):
        m, c = line
        return m * x + c

    for i in range(1, n):
        x = a[i]

        # Remove lines that can never be optimal again.
        while head + 1 < len(hull) and value(hull[head], x) >= value(hull[head + 1], x):
            head += 1

        dp[i] = value(hull[head], x)

        new_line = (b[i], dp[i])

        # Remove redundant lines from the back.
        while len(hull) - head >= 2:
            l1 = hull[-2]
            l2 = hull[-1]
            l3 = new_line

            m1, c1 = l1
            m2, c2 = l2
            m3, c3 = l3

            # Intersection(l1, l2) >= Intersection(l2, l3)
            # means l2 can never be the minimum on a future query.
            if (c2 - c1) * (m2 - m3) >= (c3 - c2) * (m1 - m2):
                hull.pop()
            else:
                break

        hull.append(new_line)

    print(dp[-1])

if __name__ == "__main__":
    solve()
```Các mảng đầu vào được lưu trữ trực tiếp vì tất cả các chuyển đổi đều sử dụng các giá trị (A_i) và (B_i) ban đầu. Dòng đầu tiên được khởi tạo là`(B1, 0)`, tương ứng chính xác với (dp[1]=0). 

Vòng lặp chính bắt đầu từ chỉ số 1, đại diện cho bộ 2. Điều này là cần thiết vì bộ 1 đã được hoàn thành và không được tính phí lại. Tại lần lặp (i), tọa độ truy vấn hiện tại là`a[i]`, và mặt trước của thân tàu mang lại tập hợp tối ưu trước đó. 

Con trỏ phía trước`head`tránh việc xóa các dòng vật lý từ phía trước. Một khi một dòng trở nên lỗi thời, tiến lên`head`là đủ. Các dòng bị xóa ở phía sau sẽ được hiển thị về mặt vật lý vì chúng dư thừa bất kể các truy vấn trong tương lai. 

Điều kiện phía sau so sánh các giao điểm không chia: 

[ 
\frac{c_2-c_1}{m_1-m_2} 
\ge 
\frac{c_3-c_2}{m_2-m_3}. 
] 

Sự khác biệt về độ dốc là dương vì độ dốc đang giảm dần. Phép nhân chéo là chính xác và số nguyên của Python không thể tràn. Sử dụng tọa độ giao điểm dấu phẩy động ở đây sẽ gây ra những rủi ro về độ chính xác không cần thiết. 

Dòng cuối cùng của tập hợp (N) có độ dốc (B_N=0). Giá trị của nó chính xác là tổng chi phí tối thiểu để đạt được tập hợp (N). Sau khi hoàn thành bộ đó, mọi bộ còn lại đều miễn phí nên không cần thêm thời hạn. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
5
1 2 3 4 6
5 4 3 1 0
```các trạng thái lập trình động như sau. 

| Đặt (i) | (A_i) | (B_i) | Bộ trước hay nhất | (dp[i]) | Quyết định thân tàu | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | đã hoàn thành | 0 | chèn (5x) | 
| 2 | 2 | 4 | 1 | 10 | chèn (4x+10) | 
| 3 | 3 | 3 | 1 | 15 | dòng 2 trở nên dư thừa | 
| 4 | 4 | 1 | 1 | 20 | dòng 3 trở nên dư thừa | 
| 5 | 6 | 0 | 4 | 26 | chèn dòng cuối cùng | 

Đối với tập 5, việc chọn tập 4 làm tập thay đổi tốc độ trước đó sẽ mang lại 

[ 
dp[4]+B_4A_5=20+1\cdot6=26. 
] 

Lịch trình tương ứng là hoàn thành tập 4 trước, sau đó đến tập 5. Tập 2 và 3 có thể hoãn lại cho đến sau tập 5, khi chi phí của chúng bằng 0. 

Đối với mẫu 2,```
6
1 2 3 8 9 10
5 4 3 2 1 0
```các tiểu bang là: 

| Đặt (i) | (A_i) | (B_i) | Bộ trước hay nhất | (dp[i]) | Quyết định thân tàu | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | đã hoàn thành | 0 | chèn (5x) | 
| 2 | 2 | 4 | 1 | 10 | chèn (4x+10) | 
| 3 | 3 | 3 | 1 | 15 | bỏ dòng thừa | 
| 4 | 8 | 2 | 3 | 39 | loại bỏ tiền tuyến lỗi thời | 
| 5 | 9 | 1 | 3 | 42 | giữ phong bì liên quan | 
| 6 | 10 | 0 | 3 | 45 | câu trả lời cuối cùng | 

Quá trình chuyển đổi cuối cùng sử dụng bộ 3: 

[ 
dp[6]=dp[3]+B_3A_6 
=15+3\cdot10 
=45. 
] 

Điều này tương ứng với chiến lược của mẫu là hoàn thành bộ 3, sau đó là bộ 6. Sau khi hoàn thành bộ 6, mọi bộ còn lại có thể được hoàn thành với chi phí bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Mỗi dòng được chèn một lần và bị xóa nhiều nhất một lần, trong khi mỗi truy vấn tiến lên con trỏ phía trước một cách đơn điệu. | 
| Không gian | (O(N)) | Thân tàu và mảng đầu vào chứa các giá trị (O(N)). | 

Với (N\le10^5), giải pháp tuyến tính chỉ thực hiện một số lượng phép toán số học không đổi cho mỗi phần tử ngoài việc loại bỏ deque được khấu hao. Nó tránh được khoảng năm tỷ lần chuyển đổi của chương trình động bậc hai một cách thoải mái. Các số nguyên có độ chính xác tùy ý của Python cũng xử lý an toàn các sản phẩm và câu trả lời lớn hơn phạm vi 32 bit. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    if n == 1:
        print(0)
        return

    hull = [(b[0], 0)]
    head = 0
    dp = [0] * n

    def value(line, x):
        return line[0] * x + line[1]

    for i in range(1, n):
        x = a[i]

        while head + 1 < len(hull) and value(hull[head], x) >= value(hull[head + 1], x):
            head += 1

        dp[i] = value(hull[head], x)

        new_line = (b[i], dp[i])

        while len(hull) - head >= 2:
            m1, c1 = hull[-2]
            m2, c2 = hull[-1]
            m3, c3 = new_line

            if (c2 - c1) * (m2 - m3) >= (c3 - c2) * (m1 - m2):
                hull.pop()
            else:
                break

        hull.append(new_line)

    print(dp[-1])

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run(
    "5\n"
    "1 2 3 4 6\n"
    "5 4 3 1 0\n"
) == "26", "sample 1"

assert run(
    "6\n"
    "1 2 3 8 9 10\n"
    "5 4 3 2 1 0\n"
) == "45", "sample 2"

# Minimum-size input
assert run(
    "1\n"
    "1\n"
    "0\n"
) == "0", "only the already-completed set exists"

# Boundary case: the first set must not be charged again
assert run(
    "2\n"
    "1 2\n"
    "5 0\n"
) == "10", "set 1 is already completed"

# All A values equal, outside the official constraints.
# This checks that equal query coordinates do not break the hull logic.
assert run(
    "3\n"
    "1 1 1\n"
    "2 1 0\n"
) == "2", "equal query coordinates"

# Maximum-size case and large answer.
# A[i] = i, B[i] = n-i, so the answer is n*(n-1).
n = 100000
a = " ".join(map(str, range(1, n + 1)))
b = " ".join(map(str, range(n - 1, -1, -1)))
expected = str(n * (n - 1))

assert run(f"{n}\n{a}\n{b}\n") == expected, "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 0`|`0`| Đầu vào có kích thước tối thiểu và đã hoàn thành bộ đầu tiên | 
|`2 / 1 2 / 5 0`|`10`| Ranh giới giữa trạng thái ban đầu và quá trình chuyển đổi thực tế đầu tiên | 
|`3 / 1 1 1 / 2 1 0`|`2`| Tọa độ truy vấn bằng nhau, nằm ngoài các ràng buộc chính thức | 
| (N=100000,\ A_i=i,\ B_i=N-i) |`9999900000`| Kích thước tối đa và số học số nguyên lớn | 

Bài kiểm tra hoàn toàn bằng nhau được cố tình đánh dấu là nằm ngoài các đảm bảo đầu vào chính thức. Một trường hợp bài toán hợp lệ không thể có tất cả (A_i) bằng nhau vì các giá trị (A_i) phải tăng một cách nghiêm ngặt và nó không thể có tất cả (B_i) bằng nhau vì các giá trị (B_i) phải giảm một cách nghiêm ngặt và (B_N=0). Nó vẫn hữu ích khi kiểm tra độ mạnh của việc triển khai vì logic truy vấn vẫn hợp lệ đối với không giảm (A_i). 

## Vỏ cạnh 

Đối với trường hợp tối thiểu,```
1
1
0
```thuật toán đi vào`n == 1`chi nhánh và bản in`0`. Không có sự chuyển đổi sang tính toán vì tập hợp duy nhất đã được hoàn thành. Thân tàu không bao giờ cần thiết. 

Đối với ranh giới trạng thái ban đầu,```
2
1 2
5 0
```thân tàu bắt đầu bằng dòng (5x), biểu thị tập 1 với (dp[1]=0). Truy vấn cho tập 2 là (x=2), do đó giá trị là (5\cdot2=10). Thuật toán in`10`. Nó không bao giờ thêm (1\cdot5), vì tập 1 không phải là một phần của công việc còn lại. 

Đối với trường hợp đi thẳng đến tập cuối cùng là không tối ưu,```
5
1 2 3 4 6
5 4 3 1 0
```quá trình chuyển đổi có liên quan cho tập 5 là thông qua tập 4. Thuật toán tính toán (dp[4]=20), sau đó đánh giá dòng cho tập 4 tại (A_5=6): 

[ 
20+1\cdot6=26. 
] 

Việc chuyển đổi trực tiếp từ tập 1 sẽ tốn (0+5\cdot6=30), do đó thân tàu chọn đúng tập 4 và tạo ra`26`. 

Đối với trường hợp giá trị lớn với (N=100000), (A_i=i) và (B_i=N-i), dòng đầu tiên biểu thị (dp[1]=0). Với mọi (i), sử dụng tập 1 sẽ cho 

[ 
dp[1]+B_1A_i=(N-1)i. 
] 

Đối với cách xây dựng cụ thể này, mọi chuyển đổi sau này cũng không tốt hơn, vì vậy 

[ 
dp[N]=N(N-1)=9,999,900,000. 
] 

Kết quả lớn hơn (2^{31}-1), kết quả này nắm bắt được việc triển khai bằng cách sử dụng số học 32 bit. Biểu diễn số nguyên của Python xử lý trực tiếp giá trị. 

Đối với tọa độ truy vấn bằng nhau, chẳng hạn như```
3
1 1 1
2 1 0
```quá trình chuyển đổi đầu tiên cho (dp[2]=2). Ở truy vấn thứ ba, cả dòng (2x) và dòng (x+2) đều có giá trị là 2 tại (x=1). Điều kiện truy vấn phía trước cho phép loại bỏ dòng cũ hơn và câu trả lời vẫn giữ nguyên`2`. Thử nghiệm này nằm ngoài điều kiện tăng nghiêm ngặt chính thức nhưng nó xác nhận rằng việc triển khai không phụ thuộc vào so sánh giao điểm dấu phẩy động hoặc tọa độ truy vấn khác biệt nghiêm ngặt.
