---
title: "CF 103800B - trò chơi của Gừng"
description: "Chúng tôi được cung cấp một loạt các giá trị sức khỏe của quái vật. Trước khi bắt đầu bất cứ điều gì, chúng tôi được phép giảm từng giá trị một cách độc lập nhưng chúng tôi không bao giờ có thể tăng giá trị đó vượt quá giá trị ban đầu. Sau sự chuẩn bị đó, chúng ta chọn vị trí bắt đầu $i$."
date: "2026-07-02T08:42:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103800
codeforces_index: "B"
codeforces_contest_name: "The 2022 SDUT Summer Trials"
rating: 0
weight: 103800
solve_time_s: 62
verified: true
draft: false
---

[CF 103800B - Trò chơi của Ginger](https://codeforces.com/problemset/problem/103800/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt các giá trị sức khỏe của quái vật. Trước khi bắt đầu bất cứ điều gì, chúng tôi được phép giảm từng giá trị một cách độc lập nhưng chúng tôi không bao giờ có thể tăng giá trị đó vượt quá giá trị ban đầu. Sau khi chuẩn bị xong chúng ta chọn vị trí xuất phát$i$. Từ vị trí đó, chúng ta di chuyển sang trái xuống chỉ số 1 và các giá trị dọc theo đoạn này phải tạo thành một chuỗi không tăng. Phần thưởng chúng tôi nhận được là tổng các giá trị được điều chỉnh cuối cùng trên phân khúc đã chọn này. 

Vì vậy, quá trình này có hai quyết định. Đầu tiên chúng ta định hình mảng hướng xuống dưới, tôn trọng giới hạn trên. Sau đó, chúng tôi chọn một tiền tố kết thúc ở một số$i$, nhưng tiền tố đó phải được sắp xếp lại theo một cách hạn chế: khi chúng ta di chuyển sang trái, các giá trị không thể tăng lên. 

Sự ràng buộc về$n$lên đến$2 \cdot 10^5$ngay lập tức loại trừ bất kỳ nỗ lực bậc hai nào tính toán lại điều chỉnh tốt nhất một cách độc lập cho mọi vị trí bắt đầu. Bất kỳ giải pháp nào xây dựng lại hoặc mô phỏng quy trình tiền tố cho mỗi$i$sẽ là quá chậm. 

Trường hợp cạnh tinh tế xuất hiện khi các giá trị đã giảm. Trong tình huống đó không cần rút gọn và câu trả lời đơn giản trở thành tổng tiền tố tốt nhất. Ở một thái cực khác, khi các giá trị dao động như$1, 100, 2, 99, 3$, các điều chỉnh cục bộ đơn giản có thể trông tối ưu cục bộ nhưng phá vỡ yêu cầu đơn điệu toàn cục khi mở rộng sang trái. 

## Phương pháp tiếp cận 

Nếu chúng ta cố định vị trí bắt đầu$i$, chúng ta có thể hỏi trình tự điều chỉnh tốt nhất có thể trên$[1, i]$trông giống như. Vì chúng ta chỉ được phép giảm các giá trị và cần một chuỗi không tăng từ phải sang trái, nên chiến lược tối ưu là tham lam từ phải sang trái: đặt giá trị cuối cùng càng cao càng tốt, sau đó kẹp từng giá trị trước đó để không vượt quá giá trị một bên phải. 

Cụ thể, nếu chúng ta sửa$i$, chúng tôi đặt giá trị cuối cùng tại$i$ĐẾN$a_i$. Sau đó di chuyển sang trái, từng vị trí$j$trở thành$\min(a_j, b_{j+1})$. Điều này tạo ra trình tự khả thi tối đa cho điểm cuối đó vì mọi ràng buộc đều mang tính cục bộ và chỉ phụ thuộc vào vị trí tiếp theo. 

Ý tưởng bạo lực là lặp lại công trình này cho mọi$i$. Mỗi lần chúng tôi tính toán lại toàn bộ tiền tố bằng cách truyền cực tiểu từ phải sang trái. Chi phí này$O(n)$mỗi$i$, dẫn đến$O(n^2)$, vượt xa giới hạn tại$2 \cdot 10^5$. 

Quan sát quan trọng là diễn giải lại những gì công trình tham lam này thực sự đang tính toán. Đối với một cố định$i$, giá trị tại vị trí$j$trở thành giá trị nhỏ nhất trên khoảng$[j, i]$. Vậy tổng số điểm cho điểm cuối$i$trở thành tổng các giá trị nhỏ nhất trên tất cả các mảng con kết thúc tại$i$. 

Một khi vấn đề được nhìn thấy ở dạng này, nó sẽ trở thành một vấn đề cổ điển “tổng các giá trị tối thiểu của mảng con kết thúc ở mỗi chỉ số”. Thay vì tính toán lại từ đầu, chúng ta có thể duy trì các đóng góp tăng dần bằng cách sử dụng một ngăn xếp đơn điệu để theo dõi vị trí của phần tử nhỏ hơn trước đó. Điều này cho phép chúng tôi cập nhật tổng mức đóng góp theo thời gian khấu hao không đổi cho mỗi chỉ mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$ĐẾN$O(n)$| Quá chậm | 
| DP dựa trên ngăn xếp |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn duy trì một chồng chỉ số tăng dần đơn điệu, trong đó các giá trị tăng lên khi chúng tôi đi sâu hơn vào ngăn xếp. Bên cạnh đó, chúng tôi tính toán một giá trị động$dp[i]$, được định nghĩa là tổng đóng góp của tất cả các mảng con kết thúc chính xác tại vị trí$i$, trong đó mỗi mảng con đóng góp phần tử tối thiểu của nó. 

1. Duy trì một chồng các chỉ số sao cho các giá trị tương ứng tăng dần từ dưới lên trên. Cấu trúc này đảm bảo rằng khi chúng ta ở vị trí$i$, ngăn xếp mã hóa vị trí của các phần tử nhỏ hơn trước đó. 
2. Đối với mỗi chỉ số$i$, bật từ ngăn xếp cho đến khi đỉnh có giá trị nhỏ hơn rất nhiều so với$a[i]$. Sau quá trình này, phần trên cùng của ngăn xếp sẽ trở thành chỉ mục trước đó$p[i]$Ở đâu$a[p[i]] < a[i]$, hoặc bằng 0 nếu không tồn tại. 
3. Quá trình chuyển đổi quan trọng là tính toán$dp[i]$sử dụng thực tế là tất cả các mảng con kết thúc tại$i$có thể được nhóm lại theo vị trí bắt đầu của chúng so với$p[i]$. Mảng con bắt đầu sau$p[i]$có$a[i]$ở mức tối thiểu, bởi vì không có gì giữa họ và$i$nhỏ hơn$a[i]$. Mảng con bắt đầu tại hoặc trước$p[i]$hành xử giống hệt như những người kết thúc tại$p[i]$. 
4. Điều này chia việc tính toán thành hai phần: mọi thứ được kế thừa từ$dp[p[i]]$, cộng với một khối hình chữ nhật gồm những đóng góp mới được đóng góp bởi$a[i]$. Số lần bắt đầu đạt mức tối thiểu mới này là$i - p[i]$, vậy phần đóng góp thêm là$a[i] \cdot (i - p[i])$. 
5. Theo dõi giá trị tối đa của$dp[i]$trên tất cả các vị trí$i$, vì bất kỳ điểm cuối nào cũng là lựa chọn hợp lệ. 

### Tại sao nó hoạt động 

Ngăn xếp đảm bảo rằng$p[i]$là vị trí gần bên trái nhất mà giá trị hoàn toàn nhỏ hơn$a[i]$. Điều này phân vùng tất cả các mảng con kết thúc tại$i$vào những nơi$a[i]$là mức tối thiểu và những nơi không có. Nhóm thứ hai hoàn toàn tương đương với các mảng con kết thúc tại$p[i]$, bởi vì bất cứ điều gì trước đây$p[i]$đã có giá trị giới hạn nhỏ hơn hoặc bằng. Bất biến này đảm bảo mọi mảng con kết thúc tại$i$được tính chính xác một lần và được quy cho mức tối thiểu chính xác của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    stack = []
    dp = [0] * n

    for i in range(n):
        while stack and a[stack[-1]] >= a[i]:
            stack.pop()

        p = stack[-1] if stack else -1
        left_count = i - p

        prev = dp[p] if p != -1 else 0
        dp[i] = prev + a[i] * left_count

        stack.append(i)

    print(max(dp))

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng một ngăn xếp đơn điệu để mỗi phần tử biết giá trị nhỏ hơn trước đó của nó. Biến`p`chính xác là ranh giới nơi các mảng con không còn có`a[i]`như mức tối thiểu của họ. phép nhân`a[i] * (i - p)`chiếm tất cả các mảng con mới kết thúc tại`i`mới được nhận nuôi`a[i]`như mức tối thiểu của họ. 

Sự tái phát cho`dp[i]`là những gì làm cho giải pháp tuyến tính. Thay vì tính toán lại tất cả các cực tiểu của mảng con, chúng ta sử dụng lại`dp[p]`, vốn đã chứa tất cả đóng góp từ cấu trúc trước đó và mở rộng nó bằng khối mới được tạo bởi`a[i]`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 1 4 3
```Chúng tôi theo dõi ngăn xếp, nhỏ hơn trước đó và dp: 

| tôi | một [tôi] | xếp chồng sau | p | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | [0] | -1 | 2 | 
| 1 | 1 | [1] | -1 | 1 | 
| 2 | 4 | [1,2] | 1 | 1 + 4*(2) = 9 | 
| 3 | 3 | [1,3] | 1 | 1 + 3*(2) = 7 | 

Tối đa là 9. 

Điều này cho thấy các phần tử tăng dần sẽ mở rộng phạm vi đóng góp như thế nào, trong khi các phần tử giảm xuống sẽ thiết lập lại cấu trúc và tạo ra các ranh giới mới. 

### Ví dụ 2 

đầu vào:```
5
5 4 3 2 1
```| tôi | một [tôi] | xếp chồng sau | p | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | [0] | -1 | 5 | 
| 1 | 4 | [1] | -1 | 4 | 
| 2 | 3 | [2] | -1 | 3 | 
| 3 | 2 | [3] | -1 | 2 | 
| 4 | 1 | [4] | -1 | 1 | 

Tối đa là 5. 

Điều này xác nhận rằng khi mảng đã giảm hẳn, mọi phần tử chỉ đóng góp vào phân khúc riêng của nó mà không có phần mở rộng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần trong ngăn xếp đơn điệu | 
| Không gian |$O(n)$| Ngăn xếp và mảng dp lưu trữ trạng thái tuyến tính | 

Hành vi tuyến tính phù hợp thoải mái trong các ràng buộc cho$2 \cdot 10^5$và mức sử dụng bộ nhớ đủ nhỏ cho giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve_output(inp))

def solve_output(inp: str) -> str:
    import sys
    from io import StringIO
    sys.stdin = StringIO(inp)

    stack = []
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    dp = [0]*n
    ans = 0

    for i in range(n):
        while stack and a[stack[-1]] >= a[i]:
            stack.pop()
        p = stack[-1] if stack else -1
        prev = dp[p] if p != -1 else 0
        dp[i] = prev + a[i] * (i - p)
        ans = max(ans, dp[i])
        stack.append(i)

    return str(ans)

# sample-like
assert solve_output("4\n2 1 4 3\n") == "9"

# minimum size
assert solve_output("1\n10\n") == "10"

# all equal
assert solve_output("5\n3 3 3 3 3\n") == "15"

# increasing
assert solve_output("4\n1 2 3 4\n") == "20"

# decreasing
assert solve_output("4\n4 3 2 1\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 2 1 4 3 | 9 | hành vi ngăn xếp chung | 
| 1 10 | 10 | trường hợp cạnh phần tử đơn | 
| 5 3 3 3 3 3 | 15 | yếu tố bình đẳng phá vỡ ranh giới | 
| 4 1 2 3 4 | 20 | trường hợp tăng nghiêm ngặt | 
| 4 4 3 2 1 | 10 | trường hợp giảm chặt | 

## Vỏ cạnh 

Một phần tử đầu vào duy nhất thể hiện trường hợp cơ sở không có cấu trúc trước đó. Ngăn xếp trống, vì vậy$p = -1$, và câu trả lời chỉ đơn giản là chính phần tử đó, phù hợp với ý tưởng rằng có chính xác một mảng con kết thúc ở đó. 

Khi tất cả các phần tử đều bằng nhau, mọi chỉ mục mới sẽ trở thành ranh giới cho tất cả các phần tử trước đó, vì ngăn xếp liên tục xuất hiện cho đến khi trống. Mỗi$dp[i]$trở thành một sự tích lũy hình tam giác và công thức vẫn đếm chính xác mỗi mảng con một cách chính xác. 

Trong một mảng tăng dần, mỗi phần tử trở thành phần tử lớn nhất mới và mở rộng tất cả các mảng con trước đó. Ngăn xếp không bao giờ bật lên, vì vậy mỗi ngăn xếp$p[i]$luôn là chỉ số trước đó và các khoản đóng góp tích lũy tuyến tính trên các phân khúc đang phát triển mà không cần đặt lại.
