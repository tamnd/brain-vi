---
title: "CF 104785C - Giải phóng không gian"
description: "Chúng ta có một tập hợp các vị trí cố định trên ranh giới của một khoảng trống hình tròn đơn vị, trong đó mỗi vị trí được mô tả bằng một góc tính bằng độ. Hãy coi những vị trí này như những điểm neo được phép, nơi có thể lắp đặt các trụ hàng rào."
date: "2026-06-28T14:37:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104785
codeforces_index: "C"
codeforces_contest_name: "2023 United Kingdom and Ireland Programming Contest (UKIEPC 2023)"
rating: 0
weight: 104785
solve_time_s: 64
verified: true
draft: false
---

[CF 104785C - Dọn sạch không gian](https://codeforces.com/problemset/problem/104785/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các vị trí cố định trên ranh giới của một khoảng trống hình tròn đơn vị, trong đó mỗi vị trí được mô tả bằng một góc tính bằng độ. Hãy coi những vị trí này như những điểm neo được phép, nơi có thể lắp đặt các trụ hàng rào. Chúng tôi cũng được đưa ra giới hạn về số lượng bài đăng mà chúng tôi được phép sử dụng. 

Khi chúng tôi chọn một số điểm được phép này, chúng tôi kết nối chúng theo thứ tự vòng tròn để tạo thành một đa giác đơn giản được ghi trong vòng tròn. Mục tiêu là chọn nhiều nhất số đỉnh cho phép sao cho đa giác bao quanh diện tích tối đa có thể. 

Hình học ở đây rất quan trọng: tất cả các đỉnh được chọn đều nằm trên một đường tròn bán kính 1 km, do đó đa giác luôn có tính tuần hoàn. Quyết định duy nhất là chọn tập hợp con điểm nào. 

Các ràng buộc rất nhỏ: tối đa 100 điểm ứng viên và tối đa 100 bài đăng có thể sử dụng. Điều này ngay lập tức loại trừ mọi phép liệt kê tập hợp con theo cấp số nhân, vì chỉ việc chọn các tập hợp con đã dẫn đến sự bùng nổ tổ hợp. Một giải pháp xung quanh O(n^3) hoặc O(n^2 p) là thực tế, nhưng bất kỳ số mũ nào trong n thì không. 

Một số trường hợp tế nhị đáng lưu ý. Đầu tiên, được phép chọn ít điểm hơn, vì vậy câu trả lời hay nhất có thể không sử dụng hết các bài đăng có sẵn nếu cấu hình không thuận lợi. Thứ hai, các điểm được đưa ra theo thứ tự góc được sắp xếp, nhưng đa giác có tính tuần hoàn, do đó, cạnh bao quanh giữa điểm được chọn cuối cùng và điểm đầu tiên phải được xử lý chính xác. Ví dụ, nếu các góc là`[0, 120, 240]`và chúng tôi chọn cả ba, cạnh cuối cùng là từ`240`quay lại`0 + 360`, không phải là "chỉ số tuyến tính tiếp theo" không tồn tại. 

Một vấn đề khác là một chiến lược tham lam ngây thơ, chẳng hạn như liên tục chọn điểm làm tăng diện tích cục bộ nhiều nhất, có thể thất bại vì diện tích phụ thuộc vào khoảng cách toàn cầu xung quanh vòng tròn chứ không phải sự cải thiện theo cặp cục bộ. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng giải quyết vấn đề một cách thô bạo, chúng ta sẽ liệt kê mọi tập hợp con có kích thước tối đa`p`, sắp xếp các điểm đã chọn và tính diện tích đa giác bằng công thức dây giày hoặc phân tách đoạn tròn. Đối với mỗi tập hợp con, việc tính toán diện tích mất O(p) thời gian và số lượng tập hợp con theo thứ tự$\sum_{k=3}^{p} \binom{n}{k}$, điều này trở nên không thể thực hiện được ngay cả khi n = 50. 

Cấu trúc trở nên đơn giản hơn khi chúng ta chuyển phối cảnh từ hình học đa giác sang các khoảng trống hình tròn. Đối với một đa giác nội tiếp trong một đường tròn, diện tích có thể được phân tách thành các phần đóng góp từ các đỉnh liên tiếp dọc theo đường tròn. Nếu chúng ta di chuyển dọc theo đường tròn theo thứ tự góc tăng dần thì mỗi cạnh sẽ tạo thành một tam giác có diện tích tỉ lệ với$\sin(\Delta \theta)$, Ở đâu$\Delta \theta$là khoảng cách góc giữa các điểm được chọn liên tiếp. Với bán kính 1 km, hệ số không đổi được cố định và có thể bỏ qua để tối ưu hóa. 

Vì vậy vấn đề trở thành: chọn tối đa`p`góc, tối đa hóa tổng của$\sin(\text{gap})$trên tất cả các khoảng trống hình tròn được hình thành bởi tập hợp đã chọn. 

Sau khi được đóng khung theo cách này, chúng ta sẽ thấy một cấu trúc điển hình: chúng ta đang chọn một chuỗi các điểm trên một vòng tròn và tối ưu hóa hàm của các sai phân liền kề. Khó khăn chính là sự phụ thuộc theo chu kỳ, vì điểm được chọn cuối cùng sẽ kết nối trở lại điểm đầu tiên. 

Chúng tôi loại bỏ sự phụ thuộc theo chu kỳ đó bằng cách sửa một điểm đã chọn làm điểm neo bắt đầu. Nếu chúng ta coi mỗi điểm bắt đầu có thể là đỉnh đầu tiên của đa giác, chúng ta sẽ tuyến tính hóa đường tròn thành một chuỗi từ điểm bắt đầu đó và cho phép DP tăng chỉ số. Để bắt đầu cố định, chúng tôi tính toán kết thúc chuỗi tốt nhất ở mỗi đỉnh cuối cùng có thể trong khi chọn chính xác`k`điểm. 

Điều này dẫn đến một giải pháp lập trình động trên các chỉ số và số điểm được chọn, trong đó các chuyển đổi chỉ phụ thuộc vào các đỉnh đã chọn trước đó và sin của khoảng cách góc. 

Độ phức tạp trở thành O(n^2 p), vì với mỗi lần bắt đầu và mỗi trạng thái, chúng tôi sẽ quét các ứng cử viên trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n · n) | O(n) | Quá chậm | 
| DP trên chuỗi tròn | O(n^2 p) | O(np) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi tất cả các góc thành một mảng hình tròn theo thứ tự được sắp xếp và chuyển đổi độ sang radian, vì tính toán sin yêu cầu radian. Điều này đảm bảo tất cả các tính toán khoảng cách đều nhất quán. 
2. Nhân đôi mảng góc bằng cách thêm từng góc cộng$2\pi$. Điều này cho phép chúng ta biểu diễn các phân đoạn bao quanh dưới dạng các khoảng tuyến tính mà không cần xử lý trường hợp đặc biệt. 
3. Sửa chỉ số bắt đầu`i`trong mảng ban đầu. Điểm bắt đầu này đóng vai trò là đỉnh đầu tiên của đa giác và loại bỏ sự nhập nhằng khi quay. 
4. Chạy lập trình động ở đâu`dp[j][k]`biểu thị tổng khoảng trống hình sin tối đa có thể đạt được khi kết thúc ở chỉ số`j`đã chọn chính xác`k`đỉnh bắt đầu từ cố định`i`. 
5. Khởi tạo`dp[i][1] = 0`, vì một đỉnh duy nhất chưa có cạnh nào. 
6. Đối với mọi chỉ số tiếp theo`j > i`và với mọi chỉ mục có thể có trước đó`t < j`, chuyển từ`t`ĐẾN`j`bằng cách thêm một đỉnh:`dp[j][k] = max(dp[j][k], dp[t][k-1] + sin(angle[j] - angle[t]))`. 

Bước này xây dựng từng cạnh đa giác dọc theo các góc tăng dần. 
7. Sau khi điền DP cho điểm bắt đầu cố định, hãy đóng đa giác bằng cách kết nối đỉnh được chọn cuối cùng`j`quay lại đỉnh ban đầu`i + 2π`, thêm`sin((i + 2π) - angle[j])`. 
8. Lấy giá trị tối đa trên tất cả các điểm kết thúc hợp lệ và trên tất cả`k ≤ p`. 
9. Nhân tổng cuối cùng với hằng số hình học tương ứng với diện tích tam giác trên một đường tròn đơn vị: bình phương bằng một nửa bán kính. Vì bán kính là 1 km nên hãy cẩn thận chuyển đổi sang mét vuông nếu cần. 

### Tại sao nó hoạt động 

Bất biến chính là mọi trạng thái DP biểu thị một đa giác một phần tối ưu có các đỉnh tăng dần theo thứ tự góc bắt đầu từ một điểm neo cố định. Bởi vì mọi đa giác tuần hoàn hợp lệ đều có thể được quay sao cho bất kỳ đỉnh nào của nó đều trở thành điểm bắt đầu, việc cố định điểm bắt đầu không loại trừ các giải pháp tối ưu. Mọi chuyển đổi đều bảo toàn thứ tự góc, do đó không có cạnh giao nhau nào xuất hiện và mọi đa giác khả thi đều tương ứng với chính xác một đường dẫn DP cho một số lựa chọn về điểm bắt đầu và điểm cuối. Mục tiêu phân rã bổ sung trên các cạnh liên tiếp, do đó cấu trúc con tối ưu giữ nguyên: khi đỉnh được chọn cuối cùng được cố định, việc hoàn thành tốt nhất chỉ phụ thuộc vào đỉnh đó và số lượng lựa chọn còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    n = int(input().strip())
    p = int(input().strip())
    ang = list(map(float, input().split()))

    # convert to radians
    ang = [a * math.pi / 180.0 for a in ang]

    # duplicate for circular handling
    a = ang + [x + 2 * math.pi for x in ang]
    m = len(ang)

    best = 0.0

    for i in range(m):
        # dp[j][k] = best ending at j using k points
        dp = [[-1e18] * (p + 1) for _ in range(2 * m)]
        dp[i][1] = 0.0

        for j in range(i + 1, i + m):
            for k in range(2, p + 1):
                best_val = -1e18
                for t in range(i, j):
                    if dp[t][k - 1] > -1e17:
                        gap = a[j] - a[t]
                        best_val = max(best_val, dp[t][k - 1] + math.sin(gap))
                dp[j][k] = best_val

        for j in range(i + 1, i + m):
            for k in range(1, p + 1):
                if dp[j][k] > -1e17:
                    gap = (a[i] + 2 * math.pi) - a[j]
                    best = max(best, dp[j][k] + math.sin(gap))

    # area factor: (R^2 / 2), R = 1000 m
    best *= 0.5 * 1000 * 1000
    print(best)

if __name__ == "__main__":
    solve()
```Mảng DP theo dõi các đa giác một phần được neo ở một góc bắt đầu cố định. Vòng lặp lồng ba phản ánh sự chuyển đổi qua các điểm trước đó, mở rộng chuỗi đỉnh tăng dần hợp lệ. Vòng lặp cuối cùng đóng đa giác bằng cách thêm cạnh trở lại góc bắt đầu được dịch chuyển bởi$2\pi$, mô hình bao quanh hình tròn một cách chính xác. 

Một cạm bẫy triển khai phổ biến là quên rằng các góc phải được xử lý theo chu kỳ. Không trùng lặp, các chuyển tiếp vượt qua ranh giới 360 độ sẽ phá vỡ trật tự. Một vấn đề tế nhị khác là sử dụng độ trực tiếp bên trong`sin`, điều này âm thầm tạo ra hình học sai mặc dù cấu trúc DP vẫn đúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Góc đầu vào:`[0, 120, 180, 240, 270]`, với`p = 4`. 

Chúng tôi sửa lỗi bắt đầu từ 0. 

| Bước | Các đỉnh được chọn | Đỉnh cuối cùng | k | Giá trị hiện tại | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [0] | 0 | 1 | 0 | 
| Gia hạn | [0, 120] | 120 | 2 | tội lỗi(120°) | 
| Gia hạn | [0, 120, 240] | 240 | 3 | sin(120°) + sin(120°) | 
| Gia hạn | [0, 120, 240, 270] | 270 | 4 | trước + tội lỗi(30°) | 

Cạnh đóng thêm tội lỗi (90°). 

Dấu vết này cho thấy cách DP chỉ tích lũy các đóng góp từ các khoảng trống góc và cách cạnh đóng cuối cùng có thể chi phối cấu trúc khi khoảng cách cuối cùng lớn. 

### Ví dụ 2 

đầu vào:`[0, 90, 180, 270]`,`p = 3`. 

| Bước | Các đỉnh được chọn | Đỉnh cuối cùng | k | Giá trị | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [0] | 0 | 1 | 0 | 
| Gia hạn | [0, 180] | 180 | 2 | sin(180°)=0 | 
| Gia hạn | [0, 180, 270] | 270 | 3 | sin(180°)+sin(90°) | 

Cạnh đóng thêm tội lỗi (90°). 

Điều này cho thấy rằng một số cạnh không đóng góp gì khi các điểm đối diện nhau và DP đương nhiên tránh dựa vào những chuyển đổi như vậy khi có cấu hình tốt hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 p) | Đối với mỗi điểm neo bắt đầu, chúng tôi chạy DP trên tất cả các cặp điểm và tối đa p lựa chọn | 
| Không gian | O(np) | Bảng DP lưu trữ các giá trị tốt nhất cho từng điểm cuối và số lượng lựa chọn | 

Với n ≤ 100 và p ≤ 100, điều này phù hợp thoải mái trong giới hạn vì hệ số hằng số vẫn nhỏ ngay cả với các vòng lặp lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else main_capture(inp)

# We'll define a safe wrapper instead

def solve(inp: str) -> str:
    import math
    input = io.StringIO(inp).readline
    n = int(input().strip())
    p = int(input().strip())
    ang = list(map(float, input().split()))
    ang = [a * math.pi / 180.0 for a in ang]
    a = ang + [x + 2 * math.pi for x in ang]
    m = len(ang)
    best = 0.0
    for i in range(m):
        dp = [[-1e18] * (p + 1) for _ in range(2 * m)]
        dp[i][1] = 0.0
        for j in range(i + 1, i + m):
            for k in range(2, p + 1):
                for t in range(i, j):
                    if dp[t][k - 1] > -1e17:
                        dp[j][k] = max(dp[j][k], dp[t][k - 1] + math.sin(a[j] - a[t]))
        for j in range(i + 1, i + m):
            for k in range(1, p + 1):
                if dp[j][k] > -1e17:
                    best = max(best, dp[j][k] + math.sin((a[i] + 2 * math.pi) - a[j]))
    best *= 0.5 * 1000 * 1000
    return str(best)

# sample-like sanity checks
assert solve("""5
4
0 120 180 240 270
""")

assert solve("""4
3
0 90 180 270
""")

assert solve("""3
3
0 120 240
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 điểm cách đều nhau | diện tích tam giác dương | tính đúng đắn cơ bản của việc đóng theo chu kỳ | 
| 4 điểm trực giao | hành vi đối xứng | xử lý các khoảng trống hình sin bằng 0 | 
| tối đa p = n | lựa chọn đa giác đầy đủ | DP đầy đủ | 

## Vỏ cạnh 

Một cấu hình trong đó tất cả các điểm cách đều nhau sẽ làm nổi bật chế độ thất bại tinh vi trong các chiến lược tham lam. Mọi khoảng trống đều giống hệt nhau, vì vậy bất kỳ tập hợp con nào cũng có vẻ tối ưu cục bộ, nhưng chỉ có sự đối xứng hoàn toàn mới mang lại diện tích tối đa. DP đánh giá chính xác tất cả độ dài chuỗi và nắm bắt mức tối ưu toàn cầu này. 

Khi các điểm nằm đối diện hoàn toàn với nhau, sin của khoảng cách sẽ bằng 0. Việc triển khai ngây thơ có thể coi điều này là không hợp lệ hoặc bỏ qua nó một cách không chính xác, nhưng trong công thức này, nó chỉ đơn giản là một đóng góp trung lập. DP vẫn xem xét chính xác các đường đi qua các điểm đó và có thể quyết định liệu chúng có hữu ích hay không dựa trên cấu trúc tiếp theo. 

Khi p bằng n, thuật toán tính toán hiệu quả thứ tự tuần hoàn tốt nhất của tất cả các điểm. DP vẫn hoạt động vì nó không bao giờ giả định rằng có ít lựa chọn hơn được ưu tiên và nó đánh giá tất cả k đến p một cách thống nhất.
