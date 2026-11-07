---
title: "CF 134C - Hoán đổi"
description: "Mỗi người chơi ban đầu sở hữu những lá bài có đúng một màu, màu riêng của họ. Người chơi i bắt đầu bằng thẻ a[i], tất cả đều có màu i. Trong quá trình đổi bài, hai người chơi mỗi người đổi một lá bài. Người chơi chỉ có thể tặng các lá bài có màu riêng của họ và không bao giờ có thể nhận được màu mà họ đã sở hữu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 134
codeforces_index: "C"
codeforces_contest_name: "Codeforces Testing Round 3"
rating: 2200
weight: 134
solve_time_s: 135
verified: false
draft: false
---

[CF 134C - Hoán đổi](https://codeforces.com/problemset/problem/134/C) 

**Xếp hạng:** 2200 
**Tags:** thuật toán xây dựng, đồ thị, tham lam 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người chơi ban đầu sở hữu những lá bài có đúng một màu, màu riêng của họ. Người chơi`i`bắt đầu bằng`a[i]`thẻ, tất cả các màu sắc`i`. Trong quá trình đổi bài, hai người chơi mỗi người đổi một lá bài. Người chơi chỉ có thể tặng các lá bài có màu riêng của họ và không bao giờ có thể nhận được màu mà họ đã sở hữu. 

Mục tiêu mạnh mẽ hơn việc chỉ phân phối lại thẻ. Cuối cùng, mỗi người chơi phải loại bỏ tất cả các lá bài có màu riêng của mình. Vì người chơi chỉ có thể đưa ra màu sắc của riêng mình nên mỗi lá bài màu`i`phải được chuyển đi thông qua các giao dịch hoán đổi cho đến khi không còn gì với người chơi`i`. 

Hạn chế về màu sắc trùng lặp là khó khăn chính. Khi người chơi đã nhận được màu`x`, họ không bao giờ có thể nhận được màu khác`x`lại. Một chuỗi hoán đổi bất cẩn có thể dễ dàng làm bế tắc quá trình ngay cả khi đã có giải pháp. 

Những hạn chế là lớn. Cả hai`n`Và`s`đang lên đến`2 * 10^5`và tổng số lần hoán đổi cũng có thể trở nên lớn. Bất kỳ thuật toán nào liên tục quét tất cả người chơi hoặc mô phỏng quyền sở hữu thẻ một cách rõ ràng sẽ quá chậm. Hành vi bậc hai ngay lập tức bị loại trừ. Giải pháp phải hoạt động gần với thời gian tuyến tính về số lượng giao dịch hoán đổi được tạo ra. 

Ngoài ra còn có một điều kiện đếm ẩn. Mỗi lần hoán đổi sẽ loại bỏ chính xác một lá bài của mỗi người trong số hai người chơi. Nếu người chơi`i`bắt đầu bằng`a[i]`thẻ, sau đó chơi`i`phải tham gia chính xác`a[i]`hoán đổi, bởi vì mỗi lần hoán đổi cho phép họ cho đi một trong các thẻ của mình. Tổng hợp tất cả người chơi:$$\sum a[i]$$phải chẵn, bởi vì mọi hoán đổi đều đóng góp`2`vào tổng số người tham gia. 

Chỉ điều kiện đó thôi là chưa đủ. Một người chơi cũng không thể tham gia nhiều lần hơn tất cả những người khác cộng lại. Nếu một số người chơi có nhiều thẻ hơn những người còn lại, cuối cùng những người khác sẽ hết lượt đổi bài trong khi người chơi này vẫn còn thẻ. 

Ví dụ:```
3 5
5 0 0
```Người chơi 1 cần năm lần hoán đổi, nhưng người chơi 2 và 3 cùng nhau chỉ có thể tham gia vào lần hoán đổi bằng 0. Câu trả lời đúng là`No`. 

Một trường hợp tế nhị khác là:```
3 3
1 1 1
```Tổng số tiền là số lẻ. Vì mỗi lần hoán đổi sẽ tiêu tốn hai lá bài nên không có chuỗi nào có thể hoàn thành cả ba lá bài. Câu trả lời đúng là`No`. 

Một trường hợp hợp lệ thú vị hơn là:```
4 4
2 1 1 0
```Một chiến lược tham lam ngây thơ có thể liên tục ghép các số lượng lớn nhất mà không theo dõi cẩn thận các màu nhận được. Điều đó có thể tạo ra xung đột màu trùng lặp sau này. Việc xây dựng đúng phải đảm bảo rằng không có cặp người chơi nào hoán đổi nhiều hơn một lần, vì việc lặp lại một cặp sẽ buộc mỗi người chơi phải nhận cùng một màu hai lần. 

Quan sát này trở thành cái nhìn sâu sắc về cấu trúc quan trọng. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là mô phỏng trực tiếp quyền sở hữu thẻ. Ở mỗi bước, chúng tôi tìm kiếm hai người chơi có thể trao đổi hợp pháp, thực hiện trao đổi, cập nhật bộ quyền sở hữu và tiếp tục cho đến khi tất cả các thẻ biến mất hoặc không còn động thái hợp pháp nào. 

Cách tiếp cận này là đúng nếu cuối cùng nó thành công, bởi vì nó tuân theo các quy tắc trò chơi theo đúng nghĩa đen. Vấn đề là hiệu quả và độ phức tạp của trạng thái. Mỗi người chơi có thể tích lũy nhiều màu sắc, vì vậy việc kiểm tra tính hợp pháp đòi hỏi phải duy trì các bộ lớn. Tệ hơn nữa, việc quyết định trao đổi cặp nào là điều không hề dễ dàng. Một lựa chọn cục bộ tồi có thể bế tắc sau này ngay cả khi tồn tại một giải pháp toàn cầu hợp lệ. 

Trong trường hợp xấu nhất có thể lên tới$$\frac{\sum a[i]}{2}$$hoán đổi và việc tìm kiếm đơn giản một cặp hợp lệ ở mỗi bước sẽ trở thành bậc hai hoặc tệ hơn. 

Quan sát quan trọng là người chơi không bao giờ có thể nhận được cùng một màu hai lần. Kể từ khi người chơi`i`luôn mang lại màu sắc`i`, người chơi khác có thể trao đổi với người chơi`i`nhiều nhất một lần. 

Điều đó biến đổi hoàn toàn vấn đề. 

Thay vì nghĩ về những tấm thẻ, hãy nghĩ về một biểu đồ. Mỗi lần trao đổi giữa người chơi`u`Và`v`tạo ra một cạnh. Vì hai người chơi không thể đổi chỗ hai lần nên đồ thị phải đơn giản. Trình độ của người chơi`i`bằng chính xác`a[i]`, vì họ phải tham gia nhiều lần hoán đổi để tặng hết thẻ của mình. 

Bây giờ nhiệm vụ trở thành: 

Xây dựng đồ thị vô hướng đơn giản trên`n`đỉnh có trình tự bậc là`a`. 

Đây là một vấn đề trình tự đồ họa cổ điển. 

Cấu trúc tham lam tiêu chuẩn cho các chuỗi đồ họa là quy trình Havel-Hakimi. Liên tục lấy đỉnh có bậc lớn nhất còn lại và nối nó với một số đỉnh có bậc cao nhất khác. Mọi kết nối đều giảm cả hai độ xuống một. 

Điều này phù hợp hoàn hảo với cách giải thích trao đổi. Đang kết nối`u`Và`v`có nghĩa là chúng tôi lên lịch chính xác một lần trao đổi giữa chúng. 

Mô phỏng lực lượng vũ phu không thành công vì nó gây ra trạng thái sở hữu thẻ tạm thời. Việc giải thích biểu đồ loại bỏ hoàn toàn sự phức tạp đó. Khi chúng tôi đảm bảo không có cạnh nào lặp lại, các ràng buộc về màu sắc sẽ tự động được đáp ứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(s²) hoặc tệ hơn | O (các) | Quá chậm | 
| Tối ưu | O(s log n) | O (các) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc trình tự độ`a[i]`. 
2. Kiểm tra tổng số độ có phải là số chẵn hay không. 

Mỗi lần hoán đổi đều đóng góp mức độ`2`, một cho mỗi điểm cuối. Một số tiền lẻ khiến cho việc giải quyết không thể thực hiện được ngay lập tức. 
3. Chèn tất cả người chơi có cấp độ dương vào một đống tối đa được sắp xếp theo cấp độ còn lại. 
4. Liên tục giải nén trình phát`u`có bậc lớn nhất còn lại`d`. 

Người chơi`u`vẫn cần`d`đối tác trao đổi riêng biệt. 
5. Nếu có ít hơn`d`những người chơi khác còn lại trong đống thì việc xây dựng là không thể. 

Một đồ thị đơn giản không thể kết nối một đỉnh với nhiều đỉnh khác biệt hơn hiện tại. 
6. Tạm thời gỡ bỏ phần tiếp theo`d`người chơi có trình độ cao nhất từ ​​đống. 
7. Đối với mỗi người chơi được trích xuất`v`: 

1. Thêm trao đổi`(u, v)`để trả lời. 
2. Giảm`v`độ còn lại của một. 
3. Cửa hàng`v`để lắp lại nếu mức độ của nó vẫn dương. 
8. Sau khi xử lý xong tất cả`d`hàng xóm, hãy lắp lại mọi trình phát đã cập nhật vào vùng lưu trữ. 
9. Tiếp tục cho đến khi heap trống. 
10. Xuất tất cả các cạnh được xây dựng dưới dạng hoán đổi. 

### Tại sao nó hoạt động 

Điều bất biến là các bậc còn lại luôn mô tả các yêu cầu hoán đổi chưa hoàn thành của một đồ thị đơn giản được xây dựng một phần. 

Khi chúng ta loại bỏ đỉnh có bậc lớn nhất`u`và kết nối nó với`d`đỉnh riêng biệt, chúng tôi đáp ứng tất cả các giao dịch hoán đổi còn lại cần thiết cho`u`trong một bước. Bởi vì mỗi lân cận được chọn là khác biệt nên không có cạnh lặp lại nào xuất hiện. Vì mỗi cạnh tương ứng với một lần hoán đổi nên không người chơi nào nhận được cùng một màu hai lần. 

Định lý Havel-Hakimi đảm bảo rằng nếu tồn tại một cách thể hiện bằng đồ họa thì sự quy giản tham lam này sẽ duy trì tính khả thi. Nếu tại một bước nào đó không có đủ các đỉnh phân biệt để kết nối thì không thể tồn tại đồ thị đơn giản với mức độ yêu cầu. 

Do đó, thuật toán xây dựng một lịch trình hoán đổi hợp lệ chính xác khi có lịch trình đó. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, s = map(int, input().split())
    a = list(map(int, input().split()))

    if sum(a) % 2:
        print("No")
        return

    pq = []

    for i, deg in enumerate(a, 1):
        if deg > 0:
            heapq.heappush(pq, (-deg, i))

    ans = []

    while pq:
        deg_u, u = heapq.heappop(pq)
        deg_u = -deg_u

        if deg_u > len(pq):
            print("No")
            return

        taken = []

        for _ in range(deg_u):
            deg_v, v = heapq.heappop(pq)
            deg_v = -deg_v

            ans.append((u, v))

            deg_v -= 1

            if deg_v < 0:
                print("No")
                return

            if deg_v > 0:
                taken.append((-deg_v, v))

        for item in taken:
            heapq.heappush(pq, item)

    print("Yes")
    print(len(ans))

    for u, v in ans:
        print(u, v)

solve()
```Heap lưu trữ các cặp`(-degree, vertex)`để vùng heap tối thiểu của Python hoạt động giống như vùng heap tối đa. 

Phần tế nhị nhất là kiểm tra tính khả thi:```
if deg_u > len(pq):
```Người chơi`u`nhu cầu`deg_u`hàng xóm riêng biệt. Nếu số đỉnh còn lại ít hơn số đó thì việc xây dựng đồ thị đơn giản là không thể. 

Một chi tiết tinh tế khác là chúng tôi không ngay lập tức lắp lại các hàng xóm sau khi giảm chúng. Đầu tiên chúng tôi loại bỏ tất cả`deg_u`hàng xóm, xử lý chúng một lần, sau đó đẩy các giá trị đã cập nhật trở lại. Điều này đảm bảo tất cả các hàng xóm đều khác biệt, ngăn chặn các cạnh trùng lặp. 

Thuật toán không bao giờ theo dõi rõ ràng màu sắc của thẻ. Cấu trúc biểu đồ đã thực thi các quy tắc màu sắc. Vì mỗi cặp người chơi đổi chỗ nhiều nhất một lần nên không người chơi nào có thể nhận được cùng một màu hai lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 8
2 2 2 2
```Đống ban đầu: 

| Bước | Đã trích xuất | Độ Heap còn lại | Những góc cạnh mới | 
| --- | --- | --- | --- | 
| 1 | 1(2) | 2,2,2 | (1,2), (1,3) | 
| 2 | 4(2) | 1,1 | (4,2), (4,3) | 
| 3 | không | trống | xong | 

Hoán đổi được thực hiện:```
1 2
1 3
4 2
4 3
```Mỗi người chơi xuất hiện chính xác hai lần, khớp với số quân bài ban đầu của họ. 

Dấu vết này thể hiện sự giải thích mức độ một cách trực tiếp. Khi cấp độ của người chơi trở thành 0, họ sẽ không bao giờ xuất hiện nữa vì tất cả các thẻ của họ đã được cho đi. 

### Ví dụ 2 

đầu vào:```
3 3
1 1 1
```Tổng số bậc là:$$1 + 1 + 1 = 3$$điều đó thật kỳ quặc. 

| Bước | Kiểm tra | Kết quả | 
| --- | --- | --- | 
| 1 | tổng % 2 == 1 | không thể | 

Đầu ra:```
No
```Ví dụ này cho thấy điều kiện chẵn lẻ. Mỗi lần hoán đổi sẽ loại bỏ chính xác hai quân bài còn lại, do đó tổng số lẻ không bao giờ có thể bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(s log n) | Mọi cạnh đều gây ra hoạt động heap | 
| Không gian | O (các) | Lưu trữ đống và cạnh trả lời | 

Số lần hoán đổi bằng$$\frac{\sum a[i]}{2}$$nhiều nhất là`2 * 10^5`. Mỗi chi phí vận hành heap`O(log n)`, do đó tổng thời gian chạy dễ dàng nằm trong giới hạn. 

Việc sử dụng bộ nhớ cũng an toàn vì bản thân câu trả lời có thể chứa tối đa`2 * 10^5`trao đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    input = sys.stdin.readline

    def solve():
        n, s = map(int, input().split())
        a = list(map(int, input().split()))

        if sum(a) % 2:
            print("No")
            return

        pq = []

        for i, deg in enumerate(a, 1):
            if deg > 0:
                heapq.heappush(pq, (-deg, i))

        ans = []

        while pq:
            deg_u, u = heapq.heappop(pq)
            deg_u = -deg_u

            if deg_u > len(pq):
                print("No")
                return

            taken = []

            for _ in range(deg_u):
                deg_v, v = heapq.heappop(pq)
                deg_v = -deg_v

                ans.append((u, v))

                deg_v -= 1

                if deg_v > 0:
                    taken.append((-deg_v, v))

            for item in taken:
                heapq.heappush(pq, item)

        print("Yes")
        print(len(ans))

        for u, v in ans:
            print(u, v)

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("4 8\n2 2 2 2\n").startswith("Yes")

# minimum case
assert run("1 0\n0\n") == "Yes\n0\n"

# odd total degree
assert run("3 3\n1 1 1\n") == "No\n"

# impossible because one degree too large
assert run("3 4\n4 0 0\n") == "No\n"

# complete graph on 4 vertices
res = run("4 6\n3 3 3 3\n")
assert res.startswith("Yes")

# sparse valid graph
res = run("5 4\n1 1 1 1 0\n")
assert res.startswith("Yes")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 / 0`|`Yes`không có giao dịch hoán đổi | Ranh giới tối thiểu | 
|`3 3 / 1 1 1`|`No`| Tổng số lẻ | 
|`3 4 / 4 0 0`|`No`| Bằng cấp lớn hơn hàng xóm có sẵn | 
|`4 6 / 3 3 3 3`|`Yes`| Xây dựng hợp lệ dày đặc | 
|`5 4 / 1 1 1 1 0`|`Yes`| Xử lý các đỉnh bị cô lập | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
3 3
1 1 1
```Thuật toán ngay lập tức kiểm tra tính chẵn lẻ của tổng bậc. Vì tổng là số lẻ nên nó in ra`No`trước khi bất kỳ quá trình xử lý heap nào bắt đầu. Điều này tránh việc xây dựng các chuỗi hoán đổi một phần không bao giờ có thể kết thúc. 

Bây giờ hãy xem xét:```
3 4
4 0 0
```Heap ban đầu chỉ chứa một đỉnh có bậc`4`. Khi chúng ta bật nó lên, heap không còn đỉnh nào nữa. điều kiện```
deg_u > len(pq)
```trở thành sự thật bởi vì`4 > 0`. Thuật toán kết luận chính xác rằng người chơi 1 không thể trao đổi với bốn đối tác riêng biệt. 

Một trường hợp hợp lệ tinh tế hơn là:```
4 4
2 1 1 0
```Đống ban đầu:```
(2,1), (1,2), (1,3)
```Người chơi 1 kết nối với người chơi 2 và 3. Độ của họ trở thành 0 và đống trống hoàn toàn. 

Hoán đổi được xây dựng:```
1 2
1 3
```Người chơi 1 tặng cả hai lá bài của họ, trong khi người chơi 2 và 3 mỗi người tặng một lá bài. Không có cặp nào lặp lại nên không nhận được màu trùng lặp. 

Cuối cùng, hãy xem xét:```
5 8
2 2 2 2 0
```Trình phát bị cô lập không gây ra vấn đề gì. Heap đơn giản bỏ qua các đỉnh có độ bằng 0. Thuật toán xây dựng một chu trình giữa bốn người chơi đang hoạt động, đáp ứng mọi yêu cầu mà không cần chạm vào người chơi 5.
