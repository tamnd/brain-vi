---
title: "CF 104257G - Điểm trung bình Go Go"
description: "Chúng tôi được cung cấp một chuỗi các khóa học phải được thực hiện theo một thứ tự cố định. Mỗi khóa học có điểm ước tính và giá trị tín chỉ. Sinh viên chia các khóa học này thành chính xác $K$ các học kỳ liên tiếp và mỗi học kỳ phải có ít nhất một khóa học."
date: "2026-07-01T21:46:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104257
codeforces_index: "G"
codeforces_contest_name: "2021 NTUIM Programming Design And Optimization (PDAO 2021)"
rating: 0
weight: 104257
solve_time_s: 68
verified: true
draft: false
---

[CF 104257G - Điểm trung bình Go Go](https://codeforces.com/problemset/problem/104257/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các khóa học phải được thực hiện theo một thứ tự cố định. Mỗi khóa học có điểm ước tính và giá trị tín chỉ. Học sinh chia các khóa học này thành chính xác$K$học kỳ liên tiếp và mỗi học kỳ phải có ít nhất một môn học. 

Trong một học kỳ, “kết quả học tập” được tính theo ba lớp. Đầu tiên, chúng tôi lấy điểm trung bình theo tín chỉ của các khóa học thô trong học kỳ đó. Điều này tạo ra một số thực trong$[0,100]$. Giá trị đó được làm tròn đến số nguyên gần nhất và sau đó được chuyển đổi thành giá trị GPA bằng cách sử dụng bảng điểm-GPA cố định. Cuối cùng, kết quả tổng thể chỉ đơn giản là giá trị trung bình số học của$K$Điểm trung bình học kỳ, không tính trọng số theo số lượng khóa học hoặc tín chỉ. 

Vì vậy, nhiệm vụ không phải là tối đa hóa điểm thô mà là chọn vị trí để cắt chuỗi thành$K$các khối liền kề sao cho giá trị trung bình của GPA rời rạc thu được càng lớn càng tốt. 

Các ràng buộc rất nhỏ: nhiều nhất là 100 khóa học và nhiều nhất là 100 học kỳ. Điều này ngay lập tức loại trừ bất kỳ phép liệt kê hàm mũ nào trên các phân vùng. Ngay cả lập trình động bậc hai hoặc bậc ba cũng được chấp nhận vì$10^6$ĐẾN$10^7$hoạt động an toàn trong Python. 

Một trường hợp thất bại tinh vi xuất phát từ bước làm tròn. Bởi vì điểm trung bình của học kỳ phụ thuộc vào điểm trung bình được làm tròn, hai phân đoạn có điểm trung bình gần giống nhau có thể tạo ra điểm trung bình khác nhau sau khi làm tròn. 

Ví dụ: nếu một học kỳ có điểm trung bình có trọng số$89.4$, nó trở thành 89, nhưng$89.5$trở thành 90. Hai giá trị này ánh xạ tới các dải GPA khác nhau, do đó, một cách tiếp cận ngây thơ cố gắng “làm mịn” mức trung bình hoặc mở rộng các phân đoạn một cách tham lam có thể thất bại. 

Một trường hợp đặc biệt khác là các quyết định phân khúc được kết hợp trên toàn cầu. Việc lấy sớm một phân khúc kém hơn một chút có thể cho phép phân khúc sau vượt qua ranh giới GPA sau khi làm tròn, làm tăng tổng số. Những quyết định tham lam của địa phương là không đáng tin cậy. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cách để chia cắt$N$các khóa học vào$K$các đoạn liền kề không trống. Số phân vùng như vậy là$\binom{N-1}{K-1}$, vốn đã rất lớn ngay cả đối với$N=100$. Đối với mỗi phân vùng, việc tính toán tất cả điểm trung bình của học kỳ yêu cầu quét các phân đoạn và thực hiện tính trung bình có trọng số, mang lại thêm$O(N)$nhân tố. Điều này trở nên hoàn toàn không khả thi. 

Cấu trúc của bài toán là giá trị của một học kỳ chỉ phụ thuộc vào chính phân đoạn đó chứ không phụ thuộc vào cách chọn các học kỳ trước đó hay tương lai. Sau khi chúng tôi khắc phục rằng học kỳ cuối cùng là các khóa học$j+1$ĐẾN$i$, phần còn lại của vấn đề giảm xuống đầu tiên$j$các khóa học với$K-1$học kỳ. Đây là một phân vùng cổ điển DP trên tiền tố. 

Quan sát quan trọng là chúng ta có thể tính toán trước mức trung bình có trọng số của bất kỳ phân khúc nào trong thời gian không đổi bằng cách sử dụng tổng tiền tố của tổng tín chỉ và tổng điểm có trọng số. Điều này giúp có thể đánh giá từng phân khúc cuối cùng của ứng viên một cách hiệu quả. 

Trạng thái DP trở thành “tổng điểm trung bình tốt nhất có thể bằng cách sử dụng điểm đầu tiên”.$i$các khóa học chia thành$k$học kỳ”, và quá trình chuyển đổi hãy thử mọi vị trí có thể bị cắt trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force |$O(\binom{N}{K} \cdot N)$|$O(N)$| Quá chậm | 
| Khoảng thời gian DP |$O(N^2 K)$|$O(NK)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trước tổng tiền tố để bất kỳ phân đoạn nào$[l, r]$có thể tính toán trung bình có trọng số của nó một cách nhanh chóng. 

Chúng tôi cũng tính toán trước việc tra cứu từ điểm số nguyên$0$ĐẾN$100$thành các giá trị GPA bằng cách sử dụng bảng ánh xạ đã cho, do đó, khi làm tròn điểm trung bình của học kỳ, chúng ta có thể nhận được điểm trung bình của nó ngay lập tức. 

### Các bước 

1. Tính mảng tiền tố cho số tín chỉ và điểm có trọng số. 

Đối với mỗi$i$, lưu trữ tổng số tín dụng lên đến$i$và tổng cộng$a_i \cdot b_i$. Điều này cho phép bất kỳ tổng phân đoạn nào được tính toán trong thời gian không đổi. 
2. Xác định hàm tính điểm trung bình học kỳ cho một phân đoạn$[l, r]$. 

Chúng tôi tính toán trung bình có trọng số$x = \frac{\sum a_i b_i}{\sum b_i}$, làm tròn nó đến số nguyên gần nhất, sau đó ánh xạ nó tới GPA. Bước làm tròn là rất quan trọng vì nó thay đổi kết quả GPA rời rạc. 
3. Xây dựng bảng DP trong đó$dp[i][k]$là tổng điểm trung bình tối đa có thể đạt được khi sử dụng lần đầu tiên$i$các khóa học chia thành$k$học kỳ. 
4. Khởi tạo$dp[0][0] = 0$, nghĩa là không có khóa học và không có học kỳ nào mang lại điểm trung bình bằng 0. 
5. Đối với mỗi$i$từ$1$ĐẾN$N$, và mỗi$k$từ$1$ĐẾN$K$, thử tất cả các điểm cắt có thể có trước đó$j < i$. 

Học kỳ cuối cùng là$[j+1, i]$, vì vậy chúng tôi cập nhật:$$dp[i][k] = \max(dp[i][k], dp[j][k-1] + GPA(j+1, i))$$6. Câu trả lời là$dp[N][K] / K$, vì điểm trung bình cuối kỳ là điểm trung bình của các học kỳ. 

### Tại sao nó hoạt động 

Mỗi lịch trình hợp lệ tương ứng với một chuỗi các điểm cắt tăng dần duy nhất chia tiền tố thành$K$phân đoạn. DP liệt kê lần cắt cuối cùng của mỗi trạng thái, đảm bảo mọi phân vùng hợp lệ đều được xem xét chính xác một lần. Bởi vì mỗi trạng thái lưu trữ giá trị tốt nhất có thể đạt được cho tiền tố đó và số học kỳ, đồng thời quá trình chuyển đổi chỉ phụ thuộc vào trạng thái tiền tố độc lập cộng với một giá trị phân đoạn duy nhất nên cấu trúc con tối ưu được giữ nguyên. Việc làm tròn và ánh xạ GPA được chứa đầy đủ trong mỗi đánh giá phân đoạn, do đó không mất đi sự phụ thuộc giữa các phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# GPA mapping based on rounded score
def score_to_gpa(x):
    if 90 <= x <= 100: return 4.3
    if 85 <= x <= 89: return 4.0
    if 80 <= x <= 84: return 3.7
    if 77 <= x <= 79: return 3.3
    if 73 <= x <= 76: return 3.0
    if 70 <= x <= 72: return 2.7
    if 67 <= x <= 69: return 2.3
    if 63 <= x <= 66: return 2.0
    if 60 <= x <= 62: return 1.7
    return 0.0

N = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))
K = int(input())

# prefix sums
pref_w = [0] * (N + 1)
pref_c = [0] * (N + 1)

for i in range(1, N + 1):
    pref_w[i] = pref_w[i - 1] + a[i - 1] * b[i - 1]
    pref_c[i] = pref_c[i - 1] + b[i - 1]

def get_gpa(l, r):
    total_w = pref_w[r] - pref_w[l - 1]
    total_c = pref_c[r] - pref_c[l - 1]
    avg = total_w / total_c
    x = int(avg + 0.5)
    return score_to_gpa(x)

dp = [[-1e9] * (K + 1) for _ in range(N + 1)]
dp[0][0] = 0.0

for i in range(1, N + 1):
    for k in range(1, min(K, i) + 1):
        best = -1e9
        for j in range(k - 1, i):
            if dp[j][k - 1] < -1e8:
                continue
            best = max(best, dp[j][k - 1] + get_gpa(j + 1, i))
        dp[i][k] = best

ans = dp[N][K] / K
print(f"{ans:.7f}")
```Mảng tiền tố làm cho mỗi lần đánh giá phân đoạn có thời gian không đổi. DP đảm bảo mỗi tiền tố và số lượng học kỳ được tính toán một lần và vòng lặp bên trong sẽ chọn lần cắt cuối cùng tốt nhất. Ràng buộc$j \ge k-1$đảm bảo có đủ các khóa học để cung cấp cho mỗi học kỳ trước ít nhất một khóa học. 

Một sai lầm phổ biến là quên rằng câu trả lời cuối cùng là điểm trung bình của các học kỳ chứ không phải tổng, vì vậy chúng ta chia cho$K$chỉ ở cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
70 80 75
3 1 4
2
```Chúng tôi theo dõi trạng thái DP cho tiền tố. 

| tôi | k | đã chọn chia | GPA phân khúc | dp[i][k] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | 3.0 | 3.0 | 
| 2 | 1 | [1,2] | 3.0 | 3.0 | 
| 2 | 2 | [1] + [2] | 3.0 + 3.0 | 6.0 | 
| 3 | 2 | [1,2] + [3] | 3.0 + 3.0 | 6.0 | 

Câu trả lời cuối cùng là$6.0 / 2 = 3.0$. 

Điều này cho thấy rằng mặc dù tồn tại các phân đoạn khác nhau, cả hai phần phân chia tối ưu đều thu gọn về cùng một GPA do làm tròn. 

### Ví dụ 2 

đầu vào:```
6
30 95 65 75 55 80
1 1 1 1 1 1
1
```Đây$K=1$, vì vậy chúng tôi học mọi thứ trong một học kỳ. 

Bình quân gia quyền là:$$(30+95+65+75+55+80)/6 = 66.67 \rightarrow 67$$67 bản đồ đạt GPA 2.3. 

Điều này xác nhận rằng DP xử lý chính xác trường hợp suy biến trong đó không cho phép phân tách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2 K)$| mỗi bang thử tất cả các vị trí cắt trước đó | 
| Không gian |$O(NK)$| Bảng DP qua các tiền tố và học kỳ | 

Với$N \le 100$, trường hợp xấu nhất xung quanh$10^6$quá trình chuyển đổi dễ dàng nhanh chóng trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    import sys
    backup = sys.stdin
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # re-run solution
    input = sys.stdin.readline

    def score_to_gpa(x):
        if 90 <= x <= 100: return 4.3
        if 85 <= x <= 89: return 4.0
        if 80 <= x <= 84: return 3.7
        if 77 <= x <= 79: return 3.3
        if 73 <= x <= 76: return 3.0
        if 70 <= x <= 72: return 2.7
        if 67 <= x <= 69: return 2.3
        if 63 <= x <= 66: return 2.0
        if 60 <= x <= 62: return 1.7
        return 0.0

    N = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    K = int(input())

    pref_w = [0] * (N + 1)
    pref_c = [0] * (N + 1)

    for i in range(1, N + 1):
        pref_w[i] = pref_w[i - 1] + a[i - 1] * b[i - 1]
        pref_c[i] = pref_c[i - 1] + b[i - 1]

    def get_gpa(l, r):
        total_w = pref_w[r] - pref_w[l - 1]
        total_c = pref_c[r] - pref_c[l - 1]
        avg = total_w / total_c
        x = int(avg + 0.5)
        return score_to_gpa(x)

    dp = [[-1e9] * (K + 1) for _ in range(N + 1)]
    dp[0][0] = 0.0

    for i in range(1, N + 1):
        for k in range(1, min(K, i) + 1):
            best = -1e9
            for j in range(k - 1, i):
                if dp[j][k - 1] < -1e8:
                    continue
                best = max(best, dp[j][k - 1] + get_gpa(j + 1, i))
            dp[i][k] = best

    ans = dp[N][K] / K
    sys.stdin = backup
    return f"{ans:.7f}"

# provided sample-like checks
assert run("""3
70 80 75
3 1 4
2
""") == "3.0000000"

assert run("""6
30 95 65 75 55 80
1 1 1 1 1 1
1
""") == "2.3000000"

# custom cases
assert run("""1
100
5
1
""") == "4.3000000", "single course"

assert run("""2
50 100
1 1
2
""") == "2.1500000", "split into extremes"

assert run("""4
90 90 90 90
2 2 2 2
2
""") == "4.3000000", "uniform scores"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 khóa học | 4.3 | ranh giới DP tối thiểu | 
| thái cực hỗn hợp | 2,15 | hiệu ứng làm tròn + chia tách | 
| điểm cao thống nhất | 4.3 | ổn định trên các phân vùng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các khóa học bị buộc phải thực hiện trong một học kỳ, tức là.$K = 1$. Thuật toán chỉ đánh giá chính xác phân đoạn$[1, N]$, tính trung bình có trọng số của nó, làm tròn nó và ánh xạ nó tới GPA mà không thử phân chia không hợp lệ. 

Một trường hợp khác là mỗi học kỳ phải có đúng một môn học, tức là$K = N$. DP hạn chế chuyển đổi để mỗi phân đoạn là một phần tử duy nhất, nghĩa là mỗi khóa học đóng góp độc lập sau khi làm tròn. Điều này mô hình chính xác ràng buộc rằng không có học kỳ nào có thể kết hợp các khóa học. 

Một trường hợp tinh tế hơn phát sinh khi các mức trung bình có trọng số nằm chính xác trên ranh giới 0,5. Ví dụ: điểm trung bình 89,5 trở thành 90 và nhảy lên cấp GPA cao hơn. Vì thuật toán thực hiện làm tròn trước khi ánh xạ nên mỗi phân đoạn được đánh giá nhất quán với định nghĩa vấn đề, đảm bảo các trường hợp biên như vậy được xử lý chính xác ngay cả khi chúng có thể lật cấu trúc phân vùng tối ưu.
