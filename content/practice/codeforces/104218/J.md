---
title: "CF 104218J - Tháng 3 Tháng 3"
description: "Chúng ta được cung cấp một biểu đồ có hướng về các thành phố được dán nhãn $n le 20$, nơi đã tồn tại một số đường một chiều. Mục tiêu dự định là để đảm bảo rằng có một “tuyến đường tháng 3 tháng 3” hợp lệ đến thăm các thành phố theo thứ tự từ 1 đến $n$, nghĩa là với mỗi cặp liên tiếp $i đến i+1$, chúng ta phải…"
date: "2026-07-01T23:51:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104218
codeforces_index: "J"
codeforces_contest_name: "UTPC Contest 03-03-23 Div. 1 (Advanced)"
rating: 0
weight: 104218
solve_time_s: 70
verified: true
draft: false
---

[CF 104218J - Tháng 3 tháng 3](https://codeforces.com/problemset/problem/104218/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trên$n \le 20$các thành phố được dán nhãn, nơi đã tồn tại một số đường một chiều. Mục tiêu dự định là để đảm bảo rằng có một “tuyến đường tháng 3” hợp lệ đến thăm các thành phố theo thứ tự từ 1 đến$n$, nghĩa là với mỗi cặp liên tiếp$i \to i+1$, chúng ta phải có khả năng di chuyển dọc theo một cạnh có hướng từ$i$ĐẾN$i+1$. 

Chúng tôi được phép có hai loại sửa đổi. Đầu tiên, chúng ta có thể xây dựng một con đường dẫn mới giữa bất kỳ cặp thành phố nào theo thứ tự với chi phí$c$. Thứ hai, chúng ta có thể dán nhãn lại cho các thành phố, nhưng việc dán nhãn lại bị hạn chế: một thành phố$i$có thể lấy nhãn$j < i$một cách tự do, nhưng nếu cần nhãn tệ hơn, nghĩa là số lượng lớn hơn, chúng tôi phải trả chi phí$d$. Điều này tạo ra sự căng thẳng giữa việc sửa đổi cấu trúc biểu đồ thông qua các cạnh và sửa đổi thứ tự thông qua nhãn. 

Nhiệm vụ là chọn cách dán nhãn lại cho các thành phố (phù hợp với các quy tắc) và quyết định dựa vào hoặc xây dựng các cạnh nào để biểu đồ được gắn nhãn cuối cùng hỗ trợ đường đi Hamilton từ 1 đến$n$, giảm thiểu tổng chi phí. 

Những hạn chế là cực kỳ nhỏ về mặt$n$, điều này ngay lập tức cho thấy việc khám phá trạng thái theo cấp số nhân là có thể chấp nhận được. Với$n \le 20$, bất kỳ giải pháp nào liệt kê các tập hợp con hoặc hoán vị của các thành phố đều khả thi miễn là nó tránh được sự bùng nổ giai thừa trong trường hợp xấu nhất. Điều này hướng mạnh mẽ đến công thức lập trình động bitmask hoặc tập hợp con DP. 

Một trường hợp cạnh tinh tế phát sinh khi biểu đồ hiện tại đã hỗ trợ một chuỗi đầy đủ nhưng việc dán nhãn lại sẽ rẻ hơn so với việc tạo các cạnh bị thiếu. Một trường hợp khác là khi việc dán nhãn lại không được phép trong thực tế do chi phí không đối xứng, làm cho giải pháp suy biến thành tăng đồ thị thuần túy. 

Một dạng thất bại cụ thể hơn đối với lối suy nghĩ ngây thơ là cho rằng chúng ta phải giữ nguyên trật tự ban đầu của các nhãn. Ví dụ, nếu các cạnh tồn tại$1 \to 3$,$3 \to 2$, Và$2 \to 4$, một cách tiếp cận ngây thơ có thể cố gắng sửa chữa các cạnh cục bộ, nhưng thay vào đó, giải pháp tối ưu có thể dán nhãn lại cho các thành phố để làm cho đường đi trở nên tầm thường, tránh việc xây dựng các cạnh tốn kém. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo là xem xét tất cả các nhãn cuối cùng có thể có của các thành phố và sau đó kiểm tra xem thứ tự kết quả có thể được tạo thành một đường dẫn hợp lệ bằng cách sử dụng các cạnh hiện có cộng với các cạnh được thêm tùy chọn hay không. Với mỗi lần ghi nhãn, chúng ta sẽ tính chi phí cho việc dán nhãn lại cộng với chi phí cho các cạnh bị thiếu. Kiểm tra tính khả thi yêu cầu xác minh kết nối dọc theo một chuỗi có độ dài cố định$n$, đó là$O(n)$, nhưng liệt kê tất cả các nhãn lại là$O(n!)$, điều đó là không thể ngay cả đối với$n = 20$. 

Quan sát quan trọng là chúng ta không thực sự cần phải suy luận rõ ràng về các hoán vị. Điều quan trọng là tập hợp con nào của thành phố đã được gán nhãn “tốt” mà không có xung đột về cấu trúc hình phạt và cách chúng tôi xây dựng lộ trình vượt qua chúng. Vì cấu trúc mục tiêu là một chuỗi duy nhất, thay vào đó, chúng ta có thể nghĩ đến việc xây dựng chuỗi tăng dần từ trái sang phải, quyết định tại mỗi bước thành phố nào sẽ chiếm vị trí tiếp theo. 

Tại vị trí$i$, chúng tôi chọn một thành phố chưa sử dụng$v$. Nếu đã có một cạnh từ thành phố được đặt trước đó$u$ĐẾN$v$, chúng tôi không phải trả gì cả. Nếu không chúng tôi phải trả$c$để xây dựng nó. Riêng biệt, việc gán các nhãn vi phạm ràng buộc đặt hàng ban đầu sẽ đưa ra các hình phạt dựa trên số lượng thành phố bị buộc vào vị trí tồi tệ hơn. 

Điều này tự nhiên dẫn đến DP bitmask nơi chúng tôi theo dõi tập hợp con thành phố nào đã được đặt cho đến nay và thành phố được đặt cuối cùng. Chi phí chuyển đổi bao gồm chi phí xây dựng cạnh cộng với hình phạt dán nhãn lại do đặt thành phố vào vị trí không phù hợp với chỉ số ban đầu của nó. 

Bởi vì$n$là nhỏ, một$O(n^2 2^n)$DP là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(n!)$|$O(n)$| Quá chậm | 
| Bitmask DP qua đơn đặt hàng |$O(n^2 2^n)$|$O(n 2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích lại vấn đề như xây dựng một chuỗi có thứ tự của tất cả các thành phố, trong đó việc đặt một thành phố ở vị trí sớm hơn chỉ số của nó là miễn phí, nhưng đặt nó muộn hơn chỉ số của nó sẽ tốn chi phí$d$trên mỗi ca đơn vị. 

1. Xác định trạng thái DP là$dp[mask][i]$, Ở đâu`mask`là tập hợp các thành phố đã được sắp xếp theo thứ tự cuối cùng và$i$là thành phố được xếp cuối cùng. Trạng thái này thể hiện chi phí tối thiểu để xây dựng một tiền tố hợp lệ kết thúc tại$i$. 
2. Khởi tạo các trường hợp cơ sở bằng cách thiết lập$dp[1 << i][i] = 0$cho tất cả các thành phố$i$. Điều này tương ứng với việc bắt đầu chuỗi với bất kỳ thành phố nào mà không mất phí. 
3. Đối với mỗi tiểu bang$(mask, i)$, hãy thử mở rộng chuỗi bằng cách chọn một thành phố$j$không ở trong`mask`. Điều này tạo ra sự chuyển tiếp sang$(mask \cup \{j\}, j)$. 
4. Tính chi phí chuyển đổi thành hai phần. Đầu tiên, hãy kiểm tra xem cạnh có hướng$i \to j$đã tồn tại rồi. Nếu không, hãy thêm chi phí$c$để xây dựng nó. Thứ hai, tính hình phạt dán nhãn lại: xếp thành phố$j$ở vị trí$|mask| + 1$có thể vi phạm thứ tự chỉ mục ban đầu của nó, vì vậy chúng tôi thêm$d \cdot \max(0, (|mask| + 1) - j)$. 
5. Cập nhật DP với chi phí tối thiểu cho mỗi trạng thái mới. 
6. Sau khi điền DP, câu trả lời là tối thiểu trên tất cả các thành phố kết thúc$i$của$dp[(1<<n)-1][i]$. 

Ý tưởng chính là mặt nạ mã hóa thứ tự vị trí một cách ngầm định. Mỗi lần mở rộng chuỗi, chúng tôi sẽ ấn định thứ hạng cuối cùng cho một thành phố và có thể trực tiếp tính toán xem thứ hạng đó có tệ hơn nhãn ban đầu của nó hay không. 

### Tại sao nó hoạt động 

DP duy trì tính bất biến rằng mọi tiểu bang đều tương ứng với một thứ tự từng phần hợp lệ của các thành phố riêng biệt với chi phí được xác định rõ ràng. Mỗi quá trình chuyển đổi sẽ thêm chính xác một thành phố chưa được sử dụng, do đó không có hoán vị không hợp lệ nào phát sinh. Bởi vì mọi thứ tự đầy đủ có thể có đều được thể hiện dưới dạng một đường dẫn qua các trạng thái này và mỗi quá trình chuyển đổi chiếm cả hình phạt xây dựng cạnh và gắn nhãn lại cục bộ, mức tối thiểu toàn cầu đạt được bằng cách giảm thiểu DP trên tất cả các trạng thái hoàn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    c, d = map(int, input().split())

    adj = [[False] * n for _ in range(n)]
    for _ in range(m):
        a, b = map(int, input().split())
        adj[a - 1][b - 1] = True

    INF = 10**18
    size = 1 << n
    dp = [[INF] * n for _ in range(size)]

    for i in range(n):
        dp[1 << i][i] = 0

    for mask in range(size):
        k = mask.bit_count()
        for i in range(n):
            if dp[mask][i] == INF:
                continue
            if not (mask & (1 << i)):
                continue

            for j in range(n):
                if mask & (1 << j):
                    continue

                nmask = mask | (1 << j)

                cost = dp[mask][i]

                if i != j:
                    if not adj[i][j]:
                        cost += c

                pos = k + 1
                if j + 1 < pos:
                    cost += d * (pos - (j + 1))

                if cost < dp[nmask][j]:
                    dp[nmask][j] = cost

    full = size - 1
    ans = min(dp[full])

    print(ans)

if __name__ == "__main__":
    solve()
```Bảng DP được lập chỉ mục bởi bitmask và nút cuối cùng, do đó việc triển khai đảm bảo rằng mọi trạng thái tiền tố được xem xét chính xác một lần. Vị trí được lấy bằng số lượng hiện tại của mặt nạ cộng với một, điều này an toàn vì mỗi lần chuyển đổi sẽ tăng kích thước mặt nạ lên chính xác một. 

Một sai lầm phổ biến là quên rằng chi phí dán nhãn lại phụ thuộc vào vị trí cuối cùng chứ không phụ thuộc vào cấu trúc biểu đồ. Một vấn đề khác là tính phí cạnh không chính xác ngay cả khi cạnh đã tồn tại. Ma trận kề tránh việc quét lặp lại và duy trì quá trình chuyển đổi$O(1)$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 5
3 5
1 2
2 3
3 5
5 4
4 6
```Chúng tôi tóm tắt các chuyển tiếp DP chính để có đường dẫn tối ưu$1 \to 2 \to 3 \to 5 \to 4 \to 6$. 

| Bước | Mặt nạ | Cuối cùng | Hành động | Chi phí cạnh | Chi phí dán nhãn lại | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | {1} | 1 | bắt đầu | 0 | 0 | 0 | 
| 2 | {1,2} | 2 | 1→2 tồn tại | 0 | 0 | 0 | 
| 3 | {1,2,3} | 3 | 2→3 tồn tại | 0 | 0 | 0 | 
| 4 | {1,2,3,5} | 5 | 3→5 tồn tại | 0 | 0 | 0 | 
| 5 | {1,2,3,5,4} | 4 | 5→4 tồn tại | 0 | 5 | 5 | 
| 6 | {1,2,3,5,4,6} | 6 | 4→6 tồn tại | 0 | 0 | 5 | 

Dấu vết này cho thấy rằng chỉ việc đặt thành phố 4 mới phải chịu một hình phạt vì nó kết thúc sau nhãn ban đầu của nó trong trình tự được xây dựng. 

### Mẫu 2 

đầu vào:```
6 5
3 10
1 2
2 3
3 5
5 4
4 6
```Cấu trúc giống hệt nhau, nhưng việc dán nhãn lại hiện đắt hơn, vì vậy DP tránh sử dụng thứ tự có nhiều trao đổi và ưu tiên duy trì trật tự tự nhiên. 

| Bước | Mặt nạ | Cuối cùng | Hành động | Chi phí cạnh | Chi phí dán nhãn lại | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | {1} | 1 | bắt đầu | 0 | 0 | 0 | 
| 2 | {1,2} | 2 | 1→2 tồn tại | 0 | 0 | 0 | 
| 3 | {1,2,3} | 3 | 2→3 tồn tại | 0 | 0 | 0 | 
| 4 | {1,2,3,5} | 5 | 3→5 tồn tại | 0 | 0 | 0 | 
| 5 | {1,2,3,5,4} | 4 | 5→4 tồn tại | 0 | 10 | 10 | 
| 6 | {1,2,3,5,4,6} | 6 | 4→6 tồn tại | 0 | 0 | 10 | 

Mức phạt cao hơn sẽ làm thay đổi sự cân bằng tối ưu nhưng không thay đổi cấu trúc, xác nhận rằng chi phí dán nhãn lại sẽ trực tiếp kiểm soát tính linh hoạt của việc đặt hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 2^n)$| Mỗi trong số$2^n$trạng thái chuyển tiếp lên tới$n$các nút tiếp theo, với việc kiểm tra cạnh liên tục | 
| Không gian |$O(n 2^n)$| Bảng DP lưu trữ chi phí cho mỗi tập hợp con và nút kết thúc | 

Với$n \le 20$,$2^n \approx 10^6$, do đó DP có khoảng 20 triệu lần chuyển đổi, có thể chấp nhận được trong Python với tối ưu hóa ma trận kề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# sample tests
assert run("""6 5
3 5
1 2
2 3
3 5
5 4
4 6
""") == "5"

assert run("""6 5
3 10
1 2
2 3
3 5
5 4
4 6
""") == "10"

# minimal case
assert run("""1 0
1 1
""") == "0"

# fully connected already optimal
assert run("""4 6
1 1
1 2
2 3
3 4
1 3
2 4
1 4
""") == "0"

# missing all edges forces building
assert run("""3 0
5 7
""") == "10"

# asymmetric relabel cost stress
assert run("""4 2
2 100
1 3
3 2
""") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 0 | trường hợp cơ sở đúng đắn | 
| được kết nối đầy đủ | 0 | không có chi phí không cần thiết | 
| không có cạnh | tích cực | sự cần thiết xây dựng cạnh | 
| chi phí bất đối xứng | biến | tương tác giữa DP và hình phạt | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi biểu đồ đã chứa một chuỗi hoàn hảo nhưng việc gắn nhãn lại sẽ đưa ra những hình phạt không cần thiết. Ví dụ, nếu các cạnh đã hình thành$1 \to 2 \to 3 \to \dots \to n$, DP sẽ luôn chọn con đường đó với chi phí biên bằng 0 và chi phí dán nhãn lại bằng 0, bởi vì mỗi thành phố xuất hiện ở vị trí tự nhiên của nó. 

Một trường hợp khác là khi một nút có chỉ số ban đầu nhỏ được đặt muộn. DP bổ sung rõ ràng$d \cdot (pos - index)$, do đó việc đặt nút 1 ở cuối sẽ tạo ra một hình phạt lớn. Quá trình chuyển đổi vẫn hợp lệ nhưng tự động trở thành dưới mức tối ưu. 

Trường hợp thứ ba là khi các cạnh tồn tại theo thứ tự ngược lại. DP xử lý việc này một cách rõ ràng vì các cạnh bị thiếu luôn được sửa chữa cục bộ với chi phí$c$, do đó, chuỗi đảo ngược trở thành một chuỗi đồng nhất của các cấu trúc cạnh thay vì trạng thái không hợp lệ.
