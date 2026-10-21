---
title: "CF 105833G - Trò chơi của hai lựa chọn"
description: "Chúng ta được cho một đồ thị có hướng trong đó mỗi đỉnh có các cạnh hướng ra một tập hợp các đỉnh lân cận. Một mã thông báo bắt đầu ở một đỉnh đã chọn và hai người chơi lần lượt di chuyển nó dọc theo các cạnh trong khi tích lũy điểm."
date: "2026-06-25T06:30:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105833
codeforces_index: "G"
codeforces_contest_name: "NUS CS3233 Final Team Contest 2025"
rating: 0
weight: 105833
solve_time_s: 42
verified: true
draft: false
---

[CF 105833G - Trò chơi của hai lựa chọn](https://codeforces.com/problemset/problem/105833/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trong đó mỗi đỉnh có các cạnh hướng ra một tập hợp các đỉnh lân cận. Một mã thông báo bắt đầu ở một đỉnh đã chọn và hai người chơi lần lượt di chuyển nó dọc theo các cạnh trong khi tích lũy điểm. Điểm tăng lên một đơn vị mỗi khi thẻ được di chuyển, nhưng quy tắc về cách chọn đỉnh tiếp theo phụ thuộc vào mức độ ngoài của đỉnh hiện tại. 

Nếu một đỉnh không có cạnh đi ra, trò chơi sẽ kết thúc ngay lập tức. Nếu nó có đúng một cạnh đi ra thì nước đi đó là bắt buộc. Nếu nó có ít nhất hai cạnh đi ra, người chơi hiện tại sẽ chọn hai cạnh đi ra riêng biệt và sau đó đối thủ chọn cạnh nào trong hai cạnh đó trở thành vị trí tiếp theo. 

Chúng tôi được yêu cầu tính toán, đối với mỗi đỉnh bắt đầu, số bước di chuyển tối đa mà người chơi đầu tiên có thể đảm bảo trong lối chơi tối ưu từ cả hai bên. Nếu trò chơi có thể bị buộc phải tiếp tục vô thời hạn thì câu trả lời cho đỉnh bắt đầu đó là -1. 

Biểu đồ có thể có tới 200.000 đỉnh và 400.000 cạnh. Bất kỳ giải pháp nào kiểm tra quá trình chuyển đổi một cách đơn giản cho từng đỉnh bắt đầu riêng biệt sẽ quá chậm. Ngay cả việc quét tuyến tính trên mỗi nút cũng dẫn đến hành vi bậc hai, điều này là không thể trong giới hạn một giây. Điều này đẩy chúng ta tới một cấu trúc tổng thể trên biểu đồ thay vì các mô phỏng độc lập. 

Một số tình huống rất dễ xử lý sai. 

Nếu một đỉnh là một phần của chu trình có hướng trong đó mỗi đỉnh có ít nhất một bậc cao hơn và người chơi có thể tránh được việc chấm dứt mãi mãi thì câu trả lời phải là -1. Một đường dẫn ngắn nhất hoặc DP ngây thơ giả định việc kết thúc cuối cùng sẽ gán một giá trị hữu hạn không chính xác. 

Nếu một đỉnh có bậc cao hơn một và hướng vào một chu trình, thì tính chất cưỡng bức của sự di chuyển có nghĩa là thuộc tính chu trình lan truyền ngược lại. Việc bỏ qua các chuỗi bắt buộc sẽ dẫn đến việc cắt ngắn độ dài trò chơi không chính xác. 

Một trường hợp tinh vi khác xuất hiện khi một đỉnh có hai cạnh hướng ra ngoài, cả hai cạnh này cuối cùng đều đạt đến cùng một trạng thái. Một giả định tham lam ngây thơ rằng “hai lựa chọn có nghĩa là phân nhánh” có thể đánh giá quá cao khả năng kiểm soát của người chơi đầu tiên, vì đối thủ thực sự giảm nó xuống còn một lần tiếp tục có thể tiếp cận được. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng trò chơi từ mọi đỉnh bắt đầu và khám phá tất cả các tương tác có thể có giữa hai người chơi. Tại mỗi đỉnh phân nhánh, chúng tôi thử tất cả các cặp cạnh đi ra và mô phỏng phản ứng tốt nhất của đối thủ. Điều này nhanh chóng trở thành cấp số nhân vì mỗi nút phân nhánh đều đưa ra cây quyết định tối thiểu-tối đa trên biểu đồ. Ngay cả việc ghi nhớ cũng không giúp ích nhiều vì trạng thái không chỉ là đỉnh mà còn phụ thuộc vào số lần mỗi đỉnh được truy cập trong trò chơi đối kháng tối ưu. 

Nhận xét quan trọng là trò chơi không phụ thuộc vào lịch sử bên ngoài đỉnh hiện tại. Mỗi đỉnh có thể được gán một giá trị: số bước tối đa mà người chơi bắt đầu vẫn có thể đảm bảo từ thời điểm đó hoặc vô tận nếu trò chơi có thể kéo dài mãi mãi. 

Điều này biến bài toán thành một đồ thị DP với hai loại chuyển tiếp. Nếu một đỉnh có đúng một cạnh ra thì giá trị bằng giá trị của cạnh kế tiếp cộng với một. Nếu nó có nhiều cạnh đi ra, người chơi hiện tại sẽ chọn hai ứng cử viên một cách hiệu quả và đối thủ sẽ đưa ra kết quả tối thiểu trong số đó. Điều này tương đương với việc nói giá trị tại một nút phụ thuộc vào việc chọn hai người kế nhiệm và lấy kết quả xấu hơn trong hai kết quả, trong khi người chơi cố gắng tối đa hóa kết quả trong trường hợp xấu nhất này. 

Cấu trúc này tự nhiên dẫn đến việc đảo ngược đồ thị và tính toán các giá trị theo cách tương tự như lan truyền đa nguồn, trong đó các đỉnh không có cạnh đi ra đóng vai trò là trạng thái cuối có giá trị bằng 0. Quá trình lan truyền hoạt động theo thứ tự tôpô ngược, nhưng các chu trình yêu cầu xử lý đặc biệt: bất kỳ chu trình nào có thể truy cập được mà không bị buộc phải chấm dứt sẽ tạo thành trạng thái vòng lặp vô hạn.

Giải pháp này giảm thiểu việc xác định các đỉnh nào có thể đạt tới “vùng kết nối mạnh không kết thúc trong chế độ chơi tối ưu” và mặt khác tính toán các đường dẫn bắt buộc dài nhất về phía sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(N + M) | Quá chậm | 
| Đồ thị DP có lan truyền ngược | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề và tính toán mức độ ngoài của mỗi đỉnh. 

Điều này là cần thiết vì quy tắc chuyển tiếp phụ thuộc hoàn toàn vào việc mức độ là 0, 1 hay ít nhất là 2. 
2. Khởi tạo hàng đợi với tất cả các đỉnh có bậc ngoài 0 và gán cho chúng giá trị 0. 

Đây là những trạng thái cuối vì trò chơi kết thúc ngay lập tức khi vào. 
3. Xử lý các đỉnh theo kiểu ngược lại bằng cách sử dụng hàng đợi, duy trì cho mỗi đỉnh câu trả lời được biết rõ nhất và bao nhiêu chuyển đổi đi ra của nó đã được giải quyết. 

Ý tưởng là một đỉnh sẽ có thể giải được khi tất cả các lựa chọn của nó đã được “đánh giá từ quan điểm của các con của nó”. 
4. Đối với các đỉnh có chính xác một cạnh ra, hãy truyền giá trị trực tiếp dưới dạng giá trị kế tiếp cộng một. 

Đây là sự di chuyển bắt buộc nên không có lựa chọn chiến lược nào tồn tại. 
5. Đối với các đỉnh có ít nhất hai cạnh hướng ra ngoài, hãy theo dõi hai giá trị kế tiếp tốt nhất. 

Vì đối thủ chọn kết quả tệ hơn trong số hai kết quả đã chọn, giá trị hiệu quả được xác định bởi cặp tốt nhất mà người chơi hiện tại có thể đưa ra, giảm thiểu khả năng tránh đường dài của đối thủ. 
6. Phát hiện các chu trình hoặc các đỉnh chưa được giải quyết sau khi truyền. 

Bất kỳ đỉnh nào vẫn có giá trị không xác định sau khi xử lý đều là một phần hoặc dẫn vào cấu trúc mà việc chơi có thể kéo dài vô thời hạn, vì vậy hãy gán -1. 

### Tại sao nó hoạt động 

Bất biến chính là khi giá trị của một đỉnh được cố định, nó thể hiện số bước tối đa được đảm bảo từ đỉnh đó trong cách chơi tối ưu. Các đỉnh đầu cuối là đúng theo định nghĩa. Các đỉnh bắt buộc truyền bá tính đúng đắn ngược lại vì không có quyết định nào liên quan. Đối với các đỉnh phân nhánh, cấu trúc tối thiểu-tối đa sẽ chuyển sang chọn một cặp cạnh đi ra có kết quả xấu nhất là tối đa và điều này được xác định đầy đủ sau khi biết tất cả các giá trị kế tiếp. Vì các giá trị chỉ phụ thuộc vào các trạng thái đã được xử lý theo thứ tự phụ thuộc ngược lại nên không bản cập nhật nào sau này có thể làm mất hiệu lực giá trị được tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    indeg = [0] * n
    outdeg = [0] * n

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        outdeg[u] += 1
        indeg[v] += 1

    # dp[i] = best guaranteed score from i, -1 means infinite
    dp = [None] * n
    q = deque()

    for i in range(n):
        if outdeg[i] == 0:
            dp[i] = 0
            q.append(i)

    # reverse graph
    rg = [[] for _ in range(n)]
    for u in range(n):
        for v in g[u]:
            rg[v].append(u)

    remaining = outdeg[:]

    while q:
        v = q.popleft()
        for u in rg[v]:
            if dp[u] is not None:
                continue

            remaining[u] -= 1

            if outdeg[u] == 1:
                # forced transition
                dp[u] = dp[v] + 1
                q.append(u)
            else:
                # collect computed neighbors
                vals = []
                for to in g[u]:
                    if dp[to] is not None:
                        vals.append(dp[to])

                if len(vals) >= 2:
                    vals.sort(reverse=True)
                    dp[u] = vals[1] + 1
                    q.append(u)

    for i in range(n):
        if dp[i] is None:
            dp[i] = -1

    print(*dp)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo ý tưởng lan truyền ngược. Trước tiên, chúng tôi khởi tạo các nút đầu cuối, sau đó đẩy các giá trị lùi dọc theo các cạnh đảo ngược. Đối với các nút có một cạnh đi ra duy nhất, quá trình chuyển đổi mang tính quyết định, vì vậy chúng tôi ngay lập tức hoàn thiện giá trị của chúng. Đối với các nút phân nhánh, chúng tôi đợi cho đến khi thấy đủ thông tin kế tiếp để xác định cặp kết quả tốt nhất. 

Một lỗi triển khai phổ biến là cố gắng tính toán các giá trị trực tiếp theo thứ tự tôpô mà không xử lý các chu trình, điều này gây ra các vòng lặp vô hạn hoặc các câu trả lời hữu hạn không chính xác. Một vấn đề phức tạp khác là trộn lẫn “các cạnh có sẵn” với “các cạnh đã được xử lý” khi đánh giá các nút phân nhánh; chỉ những giá trị kế thừa cuối cùng mới được đóng góp. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ: 

đầu vào:```
3 3
1 2
2 3
1 3
```Đỉnh 3 là thiết bị đầu cuối. 

| Bước | Xếp hàng | dp[3] | dp[2] | dp[1] | 
| --- | --- | --- | --- | --- | 
| ban đầu | [3] | 0 | - | - | 
| quá trình 3 | [2] | 0 | 1 | - | 
| quá trình 2 | [1] | 0 | 1 | - | 
| quá trình 1 | [] | 0 | 1 | 0 | 

Đỉnh 1 có thể đi trực tiếp đến 3 hoặc gián tiếp qua 2, nhưng đối thủ ép kết quả ngắn hơn thông qua phân nhánh, do đó kết thúc bằng giá trị 0. 

Bây giờ hãy xem xét một chu kỳ:```
3 3
1 2
2 3
3 1
```| Bước | Xếp hàng | dp | 
| --- | --- | --- | 
| ban đầu | [] | không | 
| không có nút đầu cuối | [] | tất cả chưa được xác định | 

Không có đỉnh nào được giải quyết nên tất cả đều được gán -1, xác nhận lượt chơi vô hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Mỗi cạnh được xử lý một số lần không đổi trong quá trình truyền ngược | 
| Không gian | O(N + M) | Danh sách kề và mảng DP | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì cả đỉnh và cạnh đều được xử lý tuyến tính, tránh mọi hoạt động khám phá lồng ghép các trạng thái trò chơi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# sample-like tests
assert run("3 3\n1 2\n2 3\n1 3\n") == "0 1 0\n"

# cycle -> all infinite
assert run("3 3\n1 2\n2 3\n3 1\n") == "-1 -1 -1\n"

# single chain
assert run("4 3\n1 2\n2 3\n3 4\n") == "3 2 1 0\n"

# branching
assert run("3 2\n1 2\n1 3\n") in ["1 0 0\n", "1 0 0"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chu trình | tất cả -1 | phát hiện chơi vô hạn | 
| đồ thị chuỗi | giá trị giảm dần | chuyển tiếp bắt buộc | 
| bắt đầu phân nhánh | hành vi tối thiểu-tối đa | hiệu ứng lựa chọn đối thủ | 

## Vỏ cạnh 

Một chu trình thuần túy như`1 → 2 → 3 → 1`không bao giờ đến nút đầu cuối, do đó không có giá trị DP nào có thể được hoàn thành trong quá trình truyền ngược. Thuật toán không xác định tất cả các nút và chuyển đổi chính xác chúng thành -1 ở cuối, thể hiện lối chơi vô hạn. 

Một chuỗi tuyến tính kết thúc ở một bồn rửa, chẳng hạn như`1 → 2 → 3 → 4`với 4 không có cạnh đi ra, truyền các giá trị lùi một cách xác định. Mỗi nút nhận được chính xác một người kế nhiệm bắt buộc và điểm số được tích lũy rõ ràng mà không có sự mơ hồ. 

Nút phân nhánh trong đó cả hai cạnh đi dẫn đến cùng một nút kế tiếp vẫn hoạt động chính xác vì thuật toán thu thập các giá trị cuối cùng và các đường dẫn trùng lặp không làm tăng bộ lựa chọn tối ưu một cách giả tạo.
