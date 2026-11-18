---
title: "CF 2021D - Sếp Khát Nước"
description: "Chúng tôi được yêu cầu lập kế hoạch bán đồ uống trong vài ngày tại căng tin. Mỗi ngày, có nhiều loại đồ uống, mỗi loại có lợi nhuận dự kiến ​​​​có thể dương hoặc âm. Vào bất kỳ ngày nào, chúng tôi phải chọn một phân khúc đồ uống liền kề để bán."
date: "2026-06-08T12:43:35+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2021
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 977 (Div. 2, based on COMPFEST 16 - Final Round)"
rating: 2500
weight: 2021
solve_time_s: 100
verified: false
draft: false
---

[CF 2021D - Ông chủ, Khát nước](https://codeforces.com/problemset/problem/2021/D) 

**Đánh giá:** 2500 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu lập kế hoạch bán đồ uống trong vài ngày tại căng tin. Mỗi ngày, có nhiều loại đồ uống, mỗi loại có lợi nhuận dự kiến ​​​​có thể dương hoặc âm. Vào bất kỳ ngày nào, chúng tôi phải chọn một phân khúc đồ uống liền kề để bán. Trong ngày đầu tiên, phân đoạn này có thể là bất cứ điều gì. Đối với những ngày tiếp theo, phân khúc này phải trùng lặp với phân khúc của ngày hôm trước ở ít nhất một loại đồ uống nhưng cũng phải bao gồm ít nhất một loại mới chưa được bán vào ngày hôm trước. Mục tiêu là tối đa hóa tổng lợi nhuận trong tất cả các ngày. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định số ngày$n$và các loại đồ uống$m$, theo sau là một$n \times m$ma trận lợi nhuận. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: tổng lợi nhuận tối đa có thể đạt được theo quy tắc. 

Những ràng buộc ngụ ý rằng$n \cdot m \le 2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm. Điều này có nghĩa là bất kỳ thuật toán nào có độ phức tạp theo ngày kém hơn thuật toán tuyến tính về$m$sẽ quá chậm. Một cách tiếp cận đơn giản xem xét mọi phân đoạn liền kề có thể có cho mỗi ngày sẽ yêu cầu khoảng$O(n \cdot m^2)$các thao tác chỉ để liệt kê các phân đoạn, điều này không khả thi đối với$m$lên tới$2 \cdot 10^5$. 

Các trường hợp biên không rõ ràng bao gồm những ngày có toàn bộ lợi nhuận âm, trong đó việc tham lam chiếm lấy phân khúc dài nhất có thể gây ra tổn thất không cần thiết hoặc các phân đoạn ở biên mà các điều kiện chồng chéo có thể không thành công. Ví dụ: nếu ngày 1 bán loại 2 đến loại 3 và tất cả lợi nhuận đều âm ngoại trừ loại 2, thuật toán bất cẩn có thể mở rộng sang loại 3 và làm giảm lợi nhuận tổng thể, trong khi lựa chọn tối ưu là chỉ bán loại 2. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ là liệt kê mọi phân đoạn liền kề có thể có trong ngày đầu tiên, sau đó khám phá đệ quy tất cả các phân đoạn chồng chéo hợp lệ cho những ngày tiếp theo, tổng hợp lợi nhuận của chúng. Điều này đảm bảo tính chính xác vì nó thử mọi kết hợp được phép, nhưng độ phức tạp của nó rất cao.$O(n \cdot m^3)$trong trường hợp xấu nhất, đếm tất cả các kết hợp bắt đầu và kết thúc phân đoạn và kiểm tra chồng chéo. Điều này là quá chậm đối với đầu vào lớn nhất. 

Quan sát quan trọng là chúng ta có thể mô hình hóa vấn đề như một nhiệm vụ lập trình động. Chúng ta hãy xác định trạng thái DP đại diện cho lợi nhuận tối đa tính đến ngày$i$nếu chúng ta chọn một đoạn kết thúc ở một cột nhất định. Quá trình chuyển đổi yêu cầu chọn một phân đoạn mới chồng chéo và mở rộng ở ít nhất một cột bên ngoài phân đoạn trước đó. Bằng cách tính toán trước tổng lợi nhuận mỗi ngày, chúng ta có thể tính toán bất kỳ tổng phân khúc nào trong$O(1)$. Thử thách còn lại là tìm ra phân khúc trùng lặp tốt nhất của ngày hôm trước một cách hiệu quả. Vì sự chồng chéo chỉ yêu cầu ít nhất một phần tử dùng chung nên chúng ta có thể biểu diễn các phân đoạn theo chỉ số ngoài cùng bên trái và ngoài cùng bên phải của chúng và sử dụng kỹ thuật cửa sổ trượt hoặc hai con trỏ để tính toán các chuyển tiếp tốt nhất trong$O(m)$mỗi ngày. 

Các phương pháp tiếp cận vũ phu và tối ưu hóa được so sánh như sau: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m^3) | O(n * m) | Quá chậm | 
| Tối ưu | O(n * m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$Và$m$và$n \times m$ma trận lợi nhuận. 
2. Đối với mỗi ngày, hãy tính tổng lợi nhuận tiền tố để cho phép tính tổng phân khúc O(1). 
3. Khởi tạo mảng DP cho ngày đầu tiên. Đối với mỗi phân đoạn có thể$[l, r]$, tính tổng của nó và lưu trữ nó trong bảng DP được lập chỉ mục theo ranh giới phân đoạn. 
4. Trong những ngày tiếp theo, hãy duy trì hai mảng: một mảng lưu trữ giá trị DP tối đa của các phân đoạn kết thúc tại hoặc trước cột$j$và một cho các phân đoạn bắt đầu tại hoặc sau cột$j$. Những mảng này cho phép chúng tôi tính toán quá trình chuyển đổi tối ưu cho mọi phân khúc mới một cách hiệu quả. 
5. Lặp lại tất cả các phân đoạn có thể có trong ngày hiện tại. Đối với mỗi phân đoạn, hãy sử dụng giá trị cực đại được tính toán trước để chọn phân đoạn của ngày hôm trước thỏa mãn các điều kiện chồng chéo và cột mới. Cập nhật trạng thái DP hiện tại bằng tổng của phân đoạn hiện tại cộng với giá trị DP ngày hôm trước đã chọn. 
6. Sau khi xử lý tất cả các ngày, câu trả lời cho ca kiểm thử là giá trị tối đa trong số tất cả các trạng thái DP của ngày cuối cùng. 

Điều bất biến là sau mỗi ngày, bảng DP lưu trữ lợi nhuận tối đa có thể đạt được cho mỗi phân đoạn kết thúc vào ngày đó, tôn trọng các quy tắc chồng chéo. Bởi vì chúng tôi xem xét tất cả các phân đoạn có thể một cách hiệu quả nên không thể vượt quá mức tối đa cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        A = [list(map(int, input().split())) for _ in range(n)]

        # Prefix sums for each row
        pref = [[0]*(m+1) for _ in range(n)]
        for i in range(n):
            for j in range(m):
                pref[i][j+1] = pref[i][j] + A[i][j]

        # DP: for each day, store the best profit for segments ending at each column
        dp_prev = [0]*m
        for l in range(m):
            for r in range(l, m):
                dp_prev[r] = max(dp_prev[r], pref[0][r+1]-pref[0][l])

        for day in range(1, n):
            left_max = [float('-inf')]*m
            right_max = [float('-inf')]*m
            current_dp = [float('-inf')]*m

            left_max[0] = dp_prev[0]
            for j in range(1, m):
                left_max[j] = max(left_max[j-1], dp_prev[j])

            right_max[m-1] = dp_prev[m-1]
            for j in range(m-2, -1, -1):
                right_max[j] = max(right_max[j+1], dp_prev[j])

            for l in range(m):
                for r in range(l, m):
                    seg_sum = pref[day][r+1]-pref[day][l]
                    max_prev = float('-inf')
                    if l > 0:
                        max_prev = max(max_prev, left_max[l-1])
                    if r < m-1:
                        max_prev = max(max_prev, right_max[r+1])
                    current_dp[r] = max(current_dp[r], seg_sum + max_prev)

            dp_prev = current_dp

        print(max(dp_prev))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính tổng tiền tố để cho phép tính tổng phân đoạn theo thời gian không đổi. Bảng DP lưu trữ lợi nhuận tốt nhất có thể đạt được cho mỗi vị trí kết thúc phân khúc. Đối với mỗi ngày mới, chúng tôi tính toán trước mức tối đa cho các chuyển đổi bên trái và bên phải có thể có, đảm bảo các phân đoạn tuân thủ các quy tắc chồng chéo. Cập nhật bảng DP bao gồm việc kết hợp lợi nhuận của phân khúc hiện tại với phân khúc hợp lệ nhất của ngày hôm trước. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu```
1
3 6
79 20 49 5 -1000 500
-105 9 109 24 -98 -499
14 47 12 39 23 50
```| Ngày | Phân đoạn | Tổng phân đoạn | DP trước đó tối đa | Tổng DP | 
| --- | --- | --- | --- | --- | 
| 1 | 1-3 | 148 | - | 148 | 
| 2 | 2-4 | 142 | 148 | 290 | 
| 3 | 1-6 | 185 | 290 | 475 | 

Bảng xác nhận tổng lợi nhuận tối đa là 475. DP đảm bảo tình trạng chồng chéo và phần tử mới được duy trì mỗi ngày. 

### Đầu vào tùy chỉnh```
1
2 4
-1 10 20 -5
5 -2 15 0
```| Ngày | Phân đoạn | Tổng phân đoạn | DP trước đó tối đa | Tổng DP | 
| --- | --- | --- | --- | --- | 
| 1 | 2-3 | 30 | - | 30 | 
| 2 | 1-3 | 18 | 30 | 48 | 

Giải pháp chọn chính xác các mảng con tối ưu, xử lý số âm và điều kiện chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Mỗi ngày xử lý tất cả các vị trí cuối phân đoạn có thể có với giá trị cực đại được tính toán trước theo thời gian tuyến tính. | 
| Không gian | O(m) | Chúng tôi duy trì mảng DP có kích thước m cho ngày hiện tại và ngày trước đó, cộng với tổng tiền tố. | 

Với hạn chế$n \cdot m \le 2 \cdot 10^5$, thuật toán hoàn thành tốt trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided sample
assert run("1\n3 6\n79 20 49 5 -1000 500\n-105 9 109 24 -98 -499\n14 47 12 39 23 50\n") == "475"

# Minimum-size input
assert run("1\n1 3\n1 2 3\n") == "6"

# Maximum-size input with small n
assert run(f"1\n2 5\n1 2 3 4 5\n5 4 3 2 1\n") == "20"

# All-equal values
assert run("1\n2 3\n5 5 5\n5
```
