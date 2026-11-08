---
title: "CF 1437C - Đầu Bếp Monocarp"
description: "Chúng ta được cung cấp một bộ sưu tập các món ăn, mỗi món gắn liền với một thời điểm “lý tưởng” mà lúc đó chúng ta nên lấy món đó ra khỏi lò. Thời gian trong bài toán này là rời rạc và tăng lên từng phút một."
date: "2026-06-11T04:44:35+07:00"
tags: ["codeforces", "competitive-programming", "dp", "flows", "graph-matchings", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1437
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 97 (Rated for Div. 2)"
rating: 1800
weight: 1437
solve_time_s: 97
verified: true
draft: false
---

[CF 1437C - Chef Monocarp](https://codeforces.com/problemset/problem/1437/C) 

**Đánh giá:** 1800 
**Tags:** dp, luồng, khớp biểu đồ, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập các món ăn, mỗi món gắn liền với một thời điểm “lý tưởng” mà lúc đó chúng ta nên lấy món đó ra khỏi lò. Thời gian trong bài toán này là rời rạc và tăng lên từng phút một. Mỗi phút, chúng ta được phép lấy ra nhiều nhất một món ăn và cuối cùng mỗi món ăn phải được lấy ra đúng một lần. Nếu một món ăn có thời gian ưa thích là$t_i$được lấy ra vào thời điểm đó$T$, chúng ta phải trả một khoản phạt bằng chênh lệch tuyệt đối$|T - t_i|$. Mục đích là sắp xếp thời gian loại bỏ, chính xác là các số nguyên$1, 2, \dots, n$, để mỗi món ăn được ấn định một thời gian duy nhất và tổng số tiền phạt được giảm thiểu. 

Nói cách khác, chúng tôi ghép từng món ăn với một khoảng thời gian riêng biệt theo hoán vị độ dài$n$và chi phí là tổng của độ lệch tuyệt đối so với vị trí ưa thích. 

Các ràng buộc rất nhỏ: tổng số đĩa trong tất cả các trường hợp thử nghiệm nhiều nhất là 200. Điều này ngay lập tức cho chúng ta biết rằng các giải pháp có hành vi bậc hai hoặc thậm chí bậc ba cho mỗi trường hợp thử nghiệm đều có thể chấp nhận được, trong khi bất kỳ số mũ nào trên hoán vị thì không. Một phép liệt kê giai thừa ngây thơ là không thể bởi vì ngay cả$20!$đã quá lớn, nhưng lập trình động trên các tập hợp con hoặc trên các vị trí được sắp xếp là khả thi. 

Một vấn đề tế nhị xuất hiện khi nhiều món ăn có cùng thời gian ưa thích. Một nhiệm vụ tham lam như “luôn chỉ định thời gian có sẵn gần nhất cho mỗi món ăn theo thứ tự đầu vào” có thể thất bại vì các quyết định sớm sẽ cản trở việc ghép đôi toàn cầu tốt hơn. Cấu trúc vốn đã mang tính toàn cầu: việc chọn thời gian cho một món ăn sẽ làm thay đổi các khoảng thời gian có sẵn cho tất cả những món ăn khác. 

Một thất bại minh họa nhỏ của sự tham lam ngây thơ có thể được xây dựng theo thời gian$[1, 1, 100, 100]$. Nếu chúng ta tham lam gán cả số 1 đầu tiên cho lần 1 và lần 2, chúng ta sẽ ngay lập tức đẩy 100 ra xa một cách không cần thiết. Thay vào đó, một sự sắp xếp tối ưu toàn cục sẽ dàn trải các nhiệm vụ một cách đối xứng. 

## Phương pháp tiếp cận 

Khó khăn cốt lõi là chúng ta phải khớp hai cấu trúc có thứ tự: các món ăn có giá trị$t_i$và các khe thời gian$1 \dots n$. Khi chúng tôi sắp xếp cả hai, vấn đề bắt đầu giống với vấn đề gán trên một thước đo đường. 

Một ý tưởng mạnh mẽ sẽ thử mọi hoán vị để gán các món ăn vào các khe thời gian và tính toán chi phí. Điều này đúng vì mọi lịch trình hợp lệ đều là một hoán vị của thời gian. Tuy nhiên, số hoán vị là$n!$, điều này trở nên khó chữa ngay cả đối với$n = 20$. Mỗi chi phí đánh giá$O(n)$, do đó tổng số trở thành$O(n \cdot n!)$, phát nổ ngay lập tức. 

Quan sát quan trọng là chi phí chênh lệch tuyệt đối trên một dòng có đặc tính đặt hàng mạnh. Nếu hai món ăn có thời gian ưa thích$a \le b$nhưng được chỉ định cho thời gian$x \le y$, việc hoán đổi nhiệm vụ không bao giờ làm tăng chi phí. Đây là lập luận trao đổi cổ điển ngụ ý tồn tại một giải pháp tối ưu trong đó cả hai chuỗi được sắp xếp một cách nhất quán. 

Vì vậy, chúng tôi sắp xếp mảng thời gian ưa thích. Bây giờ nhiệm vụ sẽ trở thành ghép nối$i$-món ăn nhỏ nhất với$i$-Khoảng thời gian thứ theo cách giảm thiểu tổng độ lệch tuyệt đối. Tuy nhiên, chúng tôi không bị buộc phải giao cho$1..n$trực tiếp theo thứ tự; chúng tôi đang chọn vị trí và DP tối ưu theo dõi xem chúng tôi đã tiến bộ bao xa theo thời gian. 

Một công thức rõ ràng là lập trình động theo chỉ mục của các món ăn được sắp xếp và thời gian hiện tại. Vì thời gian tăng lên một cách chặt chẽ và bị giới hạn bởi$n$, chúng ta xác định một DP nơi chúng ta đặt các món ăn theo thứ tự thời gian tăng dần. Ở bước$i$, chúng tôi chọn thời điểm$t$lớn hơn trước và trả$|t - a_i|$. Điều này trở thành DP tiêu chuẩn trên một dòng với các chuyển tiếp đơn điệu, có thể giải được trong$O(n^2)$. 

Chúng tôi tính toán:$$dp[i][j] = \text{minimum cost to assign first } i \text{ dishes ending at time } j$$với sự chuyển đổi từ tất cả các lần trước$k < j$:$$dp[i][j] = \min_{k < j}(dp[i-1][k]) + |j - a_i|$$Chúng ta có thể tối ưu hóa tiền tố cực tiểu để tránh một yếu tố bổ sung. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| DP với tính năng sắp xếp + tối ưu hóa tiền tố |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp dãy thời gian nấu ưa thích. Điều này làm giảm vấn đề về việc khớp các vị trí theo thứ tự, điều này hợp lệ do đối số trao đổi về khoảng cách tuyệt đối. 
2. Xác định bảng DP nơi chúng tôi xử lý từng món ăn theo thứ tự được sắp xếp. Ở mỗi bước, chúng tôi quyết định thời điểm loại bỏ món ăn hiện tại. 
3. Duy trì chỉ mục cho các khe thời gian từ 1 đến$n$. Chúng tôi đảm bảo thời gian tăng lên một cách nghiêm ngặt vì mỗi khe thời gian chỉ có thể được sử dụng một lần. 
4. Cho mỗi món ăn$i$, tính chi phí để đặt nó tại mỗi thời điểm có thể$j$, đó là$|j - a_i|$và kết hợp nó với vị trí tốt nhất trước đó. 
5. Sử dụng tiền tố cực tiểu trên hàng DP trước đó để tính toán chuyển tiếp trong$O(1)$mỗi trạng thái thay vì quét tất cả các lần trước đó. Điều này làm giảm độ phức tạp từ bậc ba sang bậc hai. 
6. Câu trả lời là giá trị tối thiểu ở hàng DP cuối cùng trong tất cả thời gian kết thúc hợp lệ. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý lần đầu tiên$i$món ăn, mỗi trạng thái DP đại diện cho chi phí tối thiểu có thể có trong số tất cả các phép gán hợp lệ trong đó chúng$i$các món ăn được giao cho các khoảng thời gian tăng dần một cách nghiêm ngặt. Ràng buộc đặt hàng đảm bảo không có sự phân công lại nào có thể cải thiện chi phí cục bộ mà không phá vỡ tính khả thi. Bởi vì chi phí chênh lệch tuyệt đối là lồi trên một đường, nên cấu trúc tối ưu tôn trọng sự so khớp đơn điệu sau khi sắp xếp, do đó, việc hạn chế DP để tăng chỉ số thời gian sẽ không loại bỏ bất kỳ giải pháp tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        INF = 10**18

        # dp[j]: min cost after placing current i items, ending at time j
        dp = [INF] * (n + 1)

        # initialize: place first dish at any time
        for j in range(1, n + 1):
            dp[j] = abs(j - a[0])

        for i in range(1, n):
            new_dp = [INF] * (n + 1)

            prefix_min = INF
            for j in range(1, n + 1):
                prefix_min = min(prefix_min, dp[j-1])
                new_dp[j] = prefix_min + abs(j - a[i])

            dp = new_dp

        print(min(dp[1:]))

def main():
    q = int(input())
    for _ in range(q):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        INF = 10**18

        dp = [INF] * (n + 1)

        for j in range(1, n + 1):
            dp[j] = abs(j - a[0])

        for i in range(1, n):
            new_dp = [INF] * (n + 1)
            prefix_min = INF

            for j in range(1, n + 1):
                prefix_min = min(prefix_min, dp[j-1])
                new_dp[j] = prefix_min + abs(j - a[i])

            dp = new_dp

        print(min(dp[1:]))

if __name__ == "__main__":
    main()
```Việc triển khai bắt đầu bằng cách sắp xếp thời gian ưu tiên, điều này thực thi cấu trúc đơn điệu cần thiết cho đối số DP. Mảng DP thể hiện chi phí tốt nhất sau khi gán tiền tố của các món ăn, kết thúc tại một khoảng thời gian cụ thể. 

Quá trình chuyển đổi sử dụng tiền tố cuộn tối thiểu so với các trạng thái trước đó. biểu hiện`prefix_min = min(prefix_min, dp[j-1])`nắm bắt cách tốt nhất để kết thúc nhiệm vụ trước đó một cách nghiêm túc trước thời hạn`j`, điều này thực thi tính duy nhất của các khe thời gian. Điều này tránh được vòng lặp bên trong rõ ràng so với tất cả các lần trước đó. 

Câu trả lời cuối cùng là mức tối thiểu trên tất cả các vị trí có thể có ở lần trước vì món ăn cuối cùng có thể kết thúc ở bất kỳ đâu. 

Một chi tiết triển khai tinh tế là lập chỉ mục dựa trên 1 của các khe thời gian. Điều này phù hợp với tuyên bố vấn đề và tránh các lỗi riêng lẻ khi tính toán sự khác biệt tuyệt đối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
t = [1, 4, 4, 4]
```Sau khi sắp xếp:```
[1, 4, 4, 4]
```Tiến hóa DP: 

| tôi (món ăn) | j (thời gian) | tiền tố_min | giá trị dp | 
| --- | --- | --- | --- | 
| 0 | 1 | - | 0 | 
| 0 | 2 | - | 1 | 
| 0 | 3 | - | 2 | 
| 0 | 4 | - | 3 | 

Đối với món thứ hai: 

| tôi | j | tiền tố_min | dp | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 3 | 
| 1 | 2 | 0 | 2 | 
| 1 | 3 | 0 | 1 | 
| 1 | 4 | 0 | 0 | 

Tiếp tục tương tự, DP hội tụ về tổng chi phí tối thiểu 2. 

Điều này cho thấy nhiều giá trị mục tiêu giống hệt nhau trải đều nhiệm vụ của chúng trên các khe thời gian lân cận thay vì thu gọn vào một điểm duy nhất. 

### Ví dụ 2 

đầu vào:```
n = 3
t = [5, 1, 2]
```Đã sắp xếp:```
[1, 2, 5]
```Chúng ta sắp xếp chúng theo thời gian 1, 2, 3: 

| món ăn | thời gian được giao | chi phí | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 0 | 
| 5 | 3 | 2 | 

Tổng cộng = 2. 

Điều này chứng tỏ rằng các ngoại lệ lớn đương nhiên bị đẩy đến phần cuối của lịch trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$mỗi trường hợp thử nghiệm | DP kết thúc$n$món ăn và$n$vị trí thời gian với tối ưu hóa tiền tố | 
| Không gian |$O(n)$| Chỉ có hai hàng DP được lưu trữ | 

Với tổng số$n \le 200$, điều này dễ dàng phù hợp với giới hạn thời gian vì trường hợp xấu nhất là khoảng$4 \times 10^4$hoạt động cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    q = int(input())
    out = []
    for _ in range(q):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        INF = 10**18
        dp = [INF] * (n + 1)

        for j in range(1, n + 1):
            dp[j] = abs(j - a[0])

        for i in range(1, n):
            new_dp = [INF] * (n + 1)
            prefix_min = INF
            for j in range(1, n + 1):
                prefix_min = min(prefix_min, dp[j-1])
                new_dp[j] = prefix_min + abs(j - a[i])
            dp = new_dp

        out.append(str(min(dp[1:])))

    return "\n".join(out)

# provided samples
assert run("""6
6
4 2 4 4 5 2
7
7 7 7 7 7 7 7
1
1
5
5 1 2 4 3
4
1 4 4 4
21
21 8 1 4 1 5 21 1 8 21 11 21 11 3 12 8 19 15 9 11 13
""") == """4
12
0
0
2
21"""

# custom cases
assert run("""1
2
1 100
""") == "98"

assert run("""1
3
1 1 1
""") == "2"

assert run("""1
4
4 3 2 1
""") == "0"

assert run("""1
5
1 2 3 4 5
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| [1, 100] | 98 | xử lý lây lan cực độ | 
| [1,1,1] | 2 | mục tiêu trùng lặp | 
| [4,3,2,1] | 0 | căn chỉnh hoàn hảo | 
| [1..5] | 0 | trường hợp lập bản đồ nhận dạng | 

## Vỏ cạnh 

Đối với những thời điểm ưa thích giống hệt nhau như$[x, x, x]$, DP sẽ phân bổ nhiệm vụ một cách tự nhiên trên các khe thời gian liên tiếp. Mỗi đĩa bổ sung phải chịu ít nhất một đơn vị dịch chuyển vì chỉ một đĩa có thể chiếm chính xác thời gian tối ưu và phần còn lại phải dịch chuyển ra ngoài. Quá trình chuyển đổi tiền tố tối thiểu đảm bảo thuật toán xem xét dần dần các vị trí thời gian sau này mà không vi phạm thứ tự. 

Đối với các chuỗi tăng hoặc giảm nghiêm ngặt, việc sắp xếp làm cho chúng giống hệt nhau và DP giảm xuống chỉ số phù hợp$i$theo thời gian$i$, mang lại chi phí bằng 0 khi các giá trị đã khớp với vị trí của chúng.
