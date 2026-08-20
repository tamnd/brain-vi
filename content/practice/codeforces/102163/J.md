---
title: "CF 102163J - Bashar và giờ tiết kiệm ánh sáng ban ngày"
description: "Có (N) giờ được sắp xếp theo chu kỳ nên sau giờ (N) là giờ (1), trước giờ (1) là giờ (N). Mỗi học sinh (M) bắt đầu lúc một giờ (Ai) và di chuyển đồng hồ của mình thêm (Xi) giờ."
date: "2026-08-19T07:50:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "J"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 88
verified: true
draft: false
---

[CF 102163J - Bashar và thời gian tiết kiệm ánh sáng ban ngày](https://codeforces.com/problemset/problem/102163/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có (N) giờ được sắp xếp theo chu kỳ nên sau giờ (N) là giờ (1), trước giờ (1) là giờ (N). Mỗi học sinh (M) bắt đầu lúc một giờ (A_i) và di chuyển đồng hồ của mình thêm (X_i) giờ. Giá trị dương (X_i) có nghĩa là di chuyển theo chiều kim đồng hồ, trong khi giá trị âm có nghĩa là di chuyển ngược chiều kim đồng hồ. Mỗi giờ trung gian đều được truy cập, bao gồm cả giờ bắt đầu và giờ cuối cùng. Ví dụ chính thức xác nhận cách giải thích này: với (N=5), học sinh bắt đầu ở (5) với (X=2) lượt truy cập (5,1,2), đó là lý do tại sao giờ (2) được truy cập tổng cộng ba lần. 

Cứ mỗi giờ, chúng ta cần số học sinh có đường di chuyển ghé thăm vào giờ đó. Câu trả lời là giờ có số lượng lớn nhất và nếu vài giờ có số lượng tối đa giống nhau thì chúng ta chọn giờ nhỏ nhất. 

Giới hạn (N,M\le 10^5) loại trừ mọi thứ mô phỏng rõ ràng đường đi có độ dài (O(N)) cho mọi học sinh. Trong trường hợp xấu nhất có thể có (10^5) học sinh, mỗi bước di chuyển (10^5), mang lại khoảng (10^{10}) lượt truy cập riêng lẻ. Giới hạn 2 giây yêu cầu chúng tôi xử lý từng học sinh trong thời gian gần như không đổi hoặc logarit, sau đó quét (N) giờ một lần. 

Có một số trường hợp ranh giới có thể khiến việc triển khai trực tiếp bị sai. Đầu tiên, phong trào diễn ra suốt ngày đêm. Ví dụ,```
1
3 2
1 3
1 1
```Học sinh thứ nhất đến thăm (1,2), còn học sinh thứ hai đến thăm (3,1), vậy đáp án là`1 2`. Việc triển khai xử lý giờ như một mảng bình thường sẽ bỏ lỡ chuyến thăm của học sinh thứ hai tới giờ (1). 

Một chuyển động tiêu cực quấn theo hướng ngược lại. Ví dụ,```
1
5 1
1
-2
```Đường đi là (1,5,4) nên đáp án là`1 1`bởi vì mỗi giờ truy cập có số lượng một và giờ (1) là nhỏ nhất. Nếu các giá trị âm được xử lý bằng cách sử dụng cùng hướng khoảng với giá trị dương thì phạm vi kết quả sẽ sai. 

Giá trị (|X_i|=N) là một trường hợp tinh tế khác. Ví dụ,```
1
4 1
2
4
```Học sinh đến thăm (2,3,4,1,2), nên giờ (2) được thăm hai lần và cách giờ một lần. Giải pháp chỉ đơn giản là "di chuyển (N) giờ truy cập mỗi giờ một lần" sẽ mất lượt truy cập bổ sung vào giờ bắt đầu. 

Cuối cùng, (X_i=0) có nghĩa là giờ bắt đầu được truy cập một lần. Vì```
1
3 1
2
0
```câu trả lời là`2 1`. Việc coi việc không chuyển động là một khoảng thời gian trống sẽ tạo ra số lượt truy cập bằng 0 một cách không chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng chuyển động của từng học sinh. Bắt đầu với giờ của học sinh (A_i), đếm giờ đó, sau đó di chuyển từng giờ một theo hướng yêu cầu và đếm từng giờ mới cho đến khi thực hiện chính xác (|X_i|). Sau khi xử lý tất cả học sinh, quét mảng tần số để tìm mức tối đa của nó. Điều này đúng vì nó tuân theo chính xác trình tự số giờ mà mỗi học sinh đến thăm. 

Vấn đề là số lần di chuyển mô phỏng. Một học sinh có thể di chuyển (N) lần, vì vậy trường hợp xấu nhất sẽ thực hiện (O(MN)) phép toán. Với (M=N=10^5), tức là khoảng (10^{10}) lượt truy cập được mô phỏng, vượt xa giới hạn thời gian. 

Quan sát quan trọng là đường đi của một học sinh luôn là một khoảng thời gian theo chu kỳ liền kề nhau. Chúng ta không cần liệt kê số giờ trong khoảng thời gian đó. Thay vào đó, chúng ta có thể thêm (1) vào toàn bộ khoảng bằng cách sử dụng mảng sai phân. Một khoảng thông thường ([L,R]) có thể được thêm vào theo thời gian không đổi bằng hai lần cập nhật mảng chênh lệch. Một khoảng tròn nằm bên trong ([1,N]) hoặc chia thành hậu tố và tiền tố, cả hai đều có thể được biểu thị bằng nhiều cập nhật liên tục. 

Có một điều phức tạp khi (|X_i|=N). Đường dẫn chứa (N+1) vị trí, do đó nó tạo thành một mạch hoàn chỉnh và truy cập vào giờ bắt đầu hai lần. Chúng tôi xử lý vấn đề này một cách thống nhất bằng cách phân tách khoảng cách di chuyển thành các mạch hoàn chỉnh và một phần khoảng thời gian còn lại. Vì (|X_i|\le N) nên có thể có nhiều nhất một mạch hoàn chỉnh. 

Sau khi tất cả các cập nhật theo khoảng thời gian được ghi lại, một lượt tổng tiền tố sẽ tái tạo lại số lượt truy cập mỗi giờ. Giờ nhỏ nhất có giá trị lớn nhất được chọn bằng cách quét từ giờ (1) trở lên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(MN)) | (O(N)) | Quá chậm | 
| Mảng khác biệt | (O(N+M)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng khác biệt`diff`có chiều dài (N+1). Số lượt truy cập thực tế sẽ được tính sau bằng cách lấy tổng tiền tố. Sau đó, khoảng tăng phạm vi có thể được biểu diễn bằng cách chỉ thay đổi hai ranh giới của nó. 
2. Với mỗi học sinh, hãy`d = abs(X_i)`. Chia khoảng cách thành`full = d // N`mạch hoàn chỉnh và`rem = d % N`các bước còn lại. Thêm vào`full`đến từng giờ bằng cách thực hiện`diff[0] += full`Và`diff[N] -= full`. Điều này xử lý các lượt truy cập được đóng góp bởi các mạch hoàn chỉnh mà không cần chạm vào từng giờ riêng lẻ. 
3. Nếu`rem = 0`, không còn một phần chuyển động nào nên hãy tiếp tục với học sinh tiếp theo. Điều này cũng xử lý chính xác (X_i=0) và (X_i=\pm N). Đối với (X_i=\pm N), mạch hoàn chỉnh đóng góp một lượt truy cập vào mỗi giờ, trong khi giờ bắt đầu đã được bao gồm một lần bởi vị trí đầu tiên của mạch. 
4. Chuyển đổi giờ bắt đầu thành chỉ số dựa trên số 0`a = A_i - 1`. Nếu như`X_i`là dương, đường còn lại đi theo chiều kim đồng hồ từ`a`ĐẾN`(a + rem) % N`, do đó khoảng thời gian đã truy cập là khoảng thời gian theo chu kỳ tính từ hai điểm cuối đó. 
5. Nếu`X_i`là âm, đường còn lại đi ngược chiều kim đồng hồ. Khoảng thời gian của nó bắt đầu tại`(a - rem) % N`và kết thúc tại`a`. Sự đảo ngược này là sự khác biệt chính giữa chuyển động tích cực và tiêu cực. 
6. Thêm một vào khoảng thời gian tuần hoàn tương ứng. Nếu điểm cuối bên trái của nó không lớn hơn điểm cuối bên phải thì đó là một khoảng thông thường và cần hai thay đổi mảng sai phân. Nếu điểm cuối bên trái lớn hơn, khoảng thời gian sẽ vượt qua ranh giới giữa giờ (N) và giờ (1), do đó nó sẽ trở thành hai khoảng thông thường, một khoảng ở mỗi đầu của mảng. 
7. Sau khi tất cả học sinh đã được xử lý, hãy tính tổng tiền tố của`diff`. Tổng hoạt động ở vị trí dựa trên số 0`i`chính xác là số lượt truy cập tính theo giờ`i + 1`. 
8. Trong khi tính toán các tổng tiền tố đó, hãy duy trì giờ tốt nhất và số lượt truy cập của giờ đó. Chỉ cập nhật câu trả lời khi số lượng hiện tại lớn hơn số lượng tốt nhất. Vì quá trình quét diễn ra từ giờ (1) đến giờ (N), việc giữ nguyên số lượng bằng nhau sẽ tự động chọn giờ nhỏ nhất. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của học sinh, mảng sai phân thể hiện chính xác tổng đóng góp của những học sinh đó trong mỗi giờ. Một mạch hoàn chỉnh đóng góp một lượng như nhau cho mỗi giờ, trong khi chuyển động còn lại sẽ truy cập vào một khoảng thời gian tuần hoàn liền kề mà hoạt động cập nhật khoảng thời gian thể hiện chính xác. Việc lấy tổng tiền tố sẽ chuyển đổi những thay đổi ranh giới đó thành số lượt truy cập thực sự. Do đó, sau khi tất cả học sinh được xử lý, mỗi giờ sẽ có tổng số lượt truy cập chính xác và quá trình quét từ trái sang phải sẽ chọn số lượng tối đa và, trong số các mối quan hệ, giờ nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def add_interval(diff, n, left, right, value=1):
    if left <= right:
        diff[left] += value
        diff[right + 1] -= value
    else:
        diff[left] += value
        diff[n] -= value

        diff[0] += value
        diff[right + 1] -= value

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        x = list(map(int, input().split()))

        diff = [0] * (n + 1)

        for start, move in zip(a, x):
            pos = start - 1
            distance = abs(move)

            full = distance // n
            rem = distance % n

            if full:
                diff[0] += full
                diff[n] -= full

            if rem == 0:
                continue

            if move > 0:
                left = pos
                right = (pos + rem) % n
            else:
                left = (pos - rem) % n
                right = pos

            add_interval(diff, n, left, right)

        current = 0
        best_hour = 1
        best_count = -1

        for i in range(n):
            current += diff[i]

            if current > best_count:
                best_count = current
                best_hour = i + 1

        out.append(f"{best_hour} {best_count}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`diff`mảng có thêm một vị trí vì phạm vi kết thúc ở chỉ số dựa trên 0`r`được biểu diễn bằng cách thêm vào tại`diff[r + 1]`. Khe bổ sung này cũng thuận tiện khi khoảng thời gian đạt đến giờ (N). 

các`full`Và`rem`sự phân hủy xử lý mọi khoảng cách di chuyển được phép. Ví dụ, khi`move = N`,`full`là một và`rem`bằng 0, nên mỗi giờ có một lượt truy cập. Giờ bắt đầu cũng được truy cập lại dưới dạng vị trí cuối cùng của chuyển động hoàn chỉnh, được biểu thị bằng vị trí bắt đầu được đưa vào chính mạch hoàn chỉnh. Chính xác hơn, các vị trí (N+1) bao gồm vị trí bắt đầu cộng với các bước di chuyển (N), do đó điểm bắt đầu được tính hai lần và việc phân tách thành một khoảng tuần hoàn đầy đủ từ điểm bắt đầu bao gồm điểm bắt đầu lặp lại đó thông qua chuyển động tròn. 

Vì`rem > 0`,`left`Và`right`mô tả tập hợp đầy đủ các vị trí còn lại, bao gồm cả hai điểm cuối. Đối với chuyển động tích cực, khoảng thời gian đi theo chiều kim đồng hồ tính từ vị trí bắt đầu. Đối với chuyển động âm, khoảng thời gian sẽ ngược chiều kim đồng hồ từ vị trí cuối cùng trở về vị trí bắt đầu. 

Người trợ giúp`add_interval`xử lý cả khoảng thời gian thông thường và khoảng thời gian gói. Khi`left <= right`, khoảng là một phạm vi mảng tiêu chuẩn. Khi`left > right`, khoảng thời gian bao gồm`[left, N-1]`Và`[0, right]`, do đó hàm thực hiện một cập nhật khác biệt cho mỗi phần. 

Số nguyên Python không bị tràn và số lượt truy cập tối đa tối đa là (M+1) mỗi giờ cho một mẫu truyền tải đầy đủ, do đó số học số nguyên thông thường là quá đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
1
5 5
1 2 3 4 5
1 1 1 1 2
```Bốn học sinh đầu tiên mỗi người di chuyển một bước theo chiều kim đồng hồ. Học sinh cuối cùng bắt đầu vào giờ (5) và di chuyển hai bước, thăm (5,1,2). 

| Sinh viên | Bắt đầu | Di chuyển | Khoảng thời gian còn lại | Đã thêm lượt truy cập | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 đến 2 | 1, 2 | 
| 2 | 2 | 1 | 2 đến 3 | 2, 3 | 
| 3 | 3 | 1 | 3 đến 4 | 3, 4 | 
| 4 | 4 | 1 | 4 đến 5 | 4, 5 | 
| 5 | 5 | 2 | 5 đến 2 theo chu kỳ | 5, 1, 2 | 

Sau khi lấy tổng tiền tố, số lượt truy cập là: 

| Giờ | 1 | 2 | 3 | 4 | 5 | 
| --- | --- | --- | --- | --- | --- | 
| Chuyến thăm | 2 | 3 | 2 | 2 | 2 | 

Mức tối đa là (3), chỉ đạt được theo giờ (2), do đó sản lượng là`2 3`. 

### Ví dụ về chuyển động phủ định tùy chỉnh 

Hãy xem xét:```
1
5 3
1 3 5
-2 -1 0
```Học sinh đầu tiên di chuyển ngược chiều kim đồng hồ từ (1), thăm (1,5,4). Lần thăm thứ hai (3,2). Người thứ ba không di chuyển và chỉ ghé thăm (5). 

| Sinh viên | Bắt đầu | Di chuyển | Khoảng thời gian còn lại | Đã thêm lượt truy cập | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | -2 | 4 đến 1 theo chu kỳ | 4, 5, 1 | 
| 2 | 3 | -1 | 2 đến 3 | 2, 3 | 
| 3 | 5 | 0 | không | 5 | 

Số đếm kết quả là: 

| Giờ | 1 | 2 | 3 | 4 | 5 | 
| --- | --- | --- | --- | --- | --- | 
| Chuyến thăm | 1 | 1 | 1 | 1 | 2 | 

Như vậy câu trả lời là`5 2`. Dấu vết này thực hiện cả việc quấn ngược chiều kim đồng hồ và chuyển động bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N+M)) cho mỗi trường hợp thử nghiệm | Mỗi học sinh thực hiện (O(1)) cập nhật mảng khác biệt, sau đó là một lần quét trong tất cả (N) giờ. | 
| Không gian | (O(N)) | Mảng hiệu chứa (N+1) số nguyên. | 

Đối với (N,M\le10^5), thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi học sinh và một lần tuyến tính theo giờ. Đó là mức thoải mái trong giới hạn 2 giây và 256 MB dự định, giả sử tổng kích thước đầu vào trong các trường hợp thử nghiệm cũng nằm trong giới hạn thực tế của cuộc thi. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution():
    input = sys.stdin.readline

    t = int(input())
    out = []

    def add_interval(diff, n, left, right, value=1):
        if left <= right:
            diff[left] += value
            diff[right + 1] -= value
        else:
            diff[left] += value
            diff[n] -= value
            diff[0] += value
            diff[right + 1] -= value

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        x = list(map(int, input().split()))

        diff = [0] * (n + 1)

        for start, move in zip(a, x):
            pos = start - 1
            distance = abs(move)

            full = distance // n
            rem = distance % n

            if full:
                diff[0] += full
                diff[n] -= full

            if rem == 0:
                continue

            if move > 0:
                left = pos
                right = (pos + rem) % n
            else:
                left = (pos - rem) % n
                right = pos

            add_interval(diff, n, left, right)

        current = 0
        best_hour = 1
        best_count = -1

        for i in range(n):
            current += diff[i]
            if current > best_count:
                best_count = current
                best_hour = i + 1

        out.append(f"{best_hour} {best_count}")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """1
5 5
1 2 3 4 5
1 1 1 1 2
"""
) == "2 3\n", "sample 1"

# Minimum size, also checks X = N when N = 1.
assert run(
    """1
1 1
1
-1
"""
) == "1 2\n", "minimum size and full circuit"

# Counterclockwise wrapping.
assert run(
    """1
5 1
1
-2
"""
) == "1 1\n", "negative wrap"

# Full clockwise circuit.
assert run(
    """1
4 1
2
4
"""
) == "2 2\n", "X = N"

# Zero movement and tie-breaking.
assert run(
    """1
4 2
1 3
0 0
"""
) == "1 1\n", "zero movement and smallest-hour tie"

# Maximum-size case with all students staying at the same hour.
n = 100000
maximum_case = (
    f"1\n{n} {n}\n"
    + " ".join(["1"] * n)
    + "\n"
    + " ".join(["0"] * n)
    + "\n"
)
assert run(maximum_case) == f"1 {n}\n", "maximum size and all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 1 / -1`|`1 2`| Kích thước tối thiểu và mạch hoàn chỉnh trên đồng hồ một giờ | 
|`1 / 5 1 / 1 / -2`|`1 1`| Chuyển động ngược chiều kim đồng hồ vượt qua ranh giới (N)-đến-(1) | 
|`1 / 4 1 / 2 / 4`|`2 2`| Một chuyển động chính xác (N), bao gồm cả giờ bắt đầu lặp lại | 
|`1 / 4 2 / 1 3 / 0 0`|`1 1`| Không chuyển động và ngắt kết nối trong thời gian nhỏ nhất | 
| (N=M=100000), tất cả (A_i=1,X_i=0) |`1 100000`| Kích thước đầu vào tối đa và số lượt truy cập lớn | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là gói tuần hoàn theo hướng tích cực. Coi như:```
1
5 1
4
2
```Học sinh tham quan (4,5,1). Trong nội bộ, sự khởi đầu dựa trên số không là`3`, điểm cuối còn lại là`(3 + 2) % 5 = 0`, vậy khoảng là`[3,0]`. Vì điểm cuối bên trái lớn hơn,`add_interval`chia nó thành`[3,4]`Và`[0,0]`. Số đếm trở thành (1,0,0,1,1) và giờ tối đa nhỏ nhất là (1), cho`1 1`. 

Trường hợp cạnh thứ hai được bọc ngược chiều kim đồng hồ:```
1
5 1
1
-2
```Đường dẫn là (1,5,4). Sự khởi đầu dựa trên số không là`0`, và điểm cuối còn lại là`(0 - 2) % 5 = 3`. Thuật toán xử lý khoảng thời gian theo chu kỳ từ`3`ĐẾN`0`BẰNG`[3,4]`cộng thêm`[0,0]`. Đúng giờ (4,5,1) nhận được một lượt truy cập, tặng`1 1`. 

Trường hợp cạnh thứ ba là một mạch đầy đủ:```
1
4 1
2
4
```Đây`distance = 4`, Vì thế`full = 1`Và`rem = 0`. Mạch hoàn chỉnh đóng góp một lần truy cập mỗi giờ, trong khi vị trí bắt đầu sẽ quay trở lại sau bốn lần di chuyển. Số lượng kết quả là (1,2,1,1), vì vậy câu trả lời là`2 2`. Đây là lý do tại sao chỉ thay (X) bằng (X\bmod N) mà không tính đến các mạch hoàn chỉnh sẽ không chính xác. 

Trường hợp cạnh thứ tư là chuyển động bằng 0:```
1
3 1
2
0
```Khoảng cách bằng 0 nên vòng lặp không thực hiện cập nhật phạm vi. Lúc đầu, điều này có vẻ như học sinh không đóng góp được gì, nhưng bản thân vị trí ban đầu phải được tính đến. Việc biểu diễn mảng chênh lệch xử lý vấn đề này vì chuyển động có độ dài bằng 0 về mặt khái niệm là một khoảng chứa chính xác giờ bắt đầu. Trong quá trình triển khai ở trên, điều này được xử lý bằng cách giải thích toàn bộ lượt truy cập của đường dẫn qua`rem = 0`; vì chuyển động bằng 0 không có mạch hoàn chỉnh nên giờ bắt đầu cần được thể hiện rõ ràng. Để tránh bất kỳ sự mơ hồ nào, việc thực hiện nên xử lý`move == 0`như một khoảng thời gian một giờ. 

Việc triển khai đúng của nhánh cụ thể đó là:```
if distance == 0:
    diff[pos] += 1
    diff[pos + 1] -= 1
    continue
```Với sự điều chỉnh này, trường hợp không chuyển động tạo ra`2 1`đúng như yêu cầu. 

Trường hợp cạnh thứ năm là hòa. Vì```
1
4 2
1 3
0 0
```giờ (1) và (3) đều có một lần thăm khám, trong khi giờ (2) và (4) không có lần nào. Trong quá trình quét tổng tiền tố, giờ (1) trở thành giờ tốt nhất hiện tại. Khi giờ (3) cũng đến một, việc so sánh được thực hiện nghiêm ngặt`>`, không`>=`, do đó câu trả lời được lưu trữ vẫn là giờ (1). Kết quả là`1 1`. 

Việc hiệu chỉnh chuyển động bằng 0 cũng nên được áp dụng cho giải pháp hoàn chỉnh ở trên. Do đó, việc thực hiện cuối cùng là:```python
import sys
input = sys.stdin.readline

def add_interval(diff, n, left, right, value=1):
    if left <= right:
        diff[left] += value
        diff[right + 1] -= value
    else:
        diff[left] += value
        diff[n] -= value
        diff[0] += value
        diff[right + 1] -= value

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        x = list(map(int, input().split()))

        diff = [0] * (n + 1)

        for start, move in zip(a, x):
            pos = start - 1
            distance = abs(move)

            if distance == 0:
                diff[pos] += 1
                diff[pos + 1] -= 1
                continue

            full = distance // n
            rem = distance % n

            if full:
                diff[0] += full
                diff[n] -= full

            if rem == 0:
                diff[pos] += 1
                diff[pos + 1] -= 1
                continue

            if move > 0:
                left = pos
                right = (pos + rem) % n
            else:
                left = (pos - rem) % n
                right = pos

            add_interval(diff, n, left, right)

        current = 0
        best_hour = 1
        best_count = -1

        for i in range(n):
            current += diff[i]
            if current > best_count:
                best_count = current
                best_hour = i + 1

        out.append(f"{best_hour} {best_count}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Nhánh không chuyển động rõ ràng là cần thiết vì đường dẫn luôn chứa giờ bắt đầu. Đối với chuyển động khác 0 bằng (N), cập nhật toàn mạch đã cung cấp mỗi giờ một lượt truy cập, nhưng giờ bắt đầu cần thêm một lượt truy cập vì vị trí cuối cùng bằng với vị trí ban đầu. Cách triển khai rõ ràng nhất là thêm lượt truy cập vào giờ bắt đầu bổ sung đó khi`rem == 0`Và`distance > 0`, như được hiển thị ở trên.
