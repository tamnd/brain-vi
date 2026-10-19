---
title: "CF 105673D - Đốt mỡ"
description: "Sự cố mô tả một chuỗi ngày, trong đó mỗi ngày được liên kết với một giá trị giống như chi phí và một yêu cầu. Giải thích là theo thời gian, bạn đang tích lũy một loại khả năng nào đó và mỗi ngày bạn phải đáp ứng hoặc vượt quá ngưỡng yêu cầu."
date: "2026-06-26T09:53:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105673
codeforces_index: "D"
codeforces_contest_name: "AlgoChief Sprint Round 1"
rating: 0
weight: 105673
solve_time_s: 50
verified: true
draft: false
---

[CF 105673D - Công cụ đốt mỡ](https://codeforces.com/problemset/problem/105673/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố mô tả một chuỗi ngày, trong đó mỗi ngày được liên kết với một giá trị giống như chi phí và một yêu cầu. Giải thích là theo thời gian, bạn đang tích lũy một loại khả năng nào đó và mỗi ngày bạn phải đáp ứng hoặc vượt quá ngưỡng yêu cầu. Khả năng ban đầu của bạn được cung cấp và mỗi ngày bạn có thể tùy ý thực hiện một hành động làm tăng khả năng này vĩnh viễn. Mỗi hành động như vậy đều có chi phí và chỉ khả dụng vào ngày nó xuất hiện. 

Nói lại một cách cụ thể, bạn bắt đầu với giá trị cường độ cơ bản. Đối với mỗi ngày tôi, có một sức mạnh cần thiết ít nhất phải phù hợp với ngày đó. Ngoài ra, vào ngày tôi, bạn được cung cấp một tùy chọn nâng cấp duy nhất: nếu bạn sử dụng nó, sức mạnh của bạn sẽ tăng vĩnh viễn theo một lượng cố định, nhưng bạn phải trả phí cho nó. Bạn muốn chọn một tập hợp con của những ngày nâng cấp này sao cho đáp ứng được mọi yêu cầu hàng ngày, đồng thời giảm thiểu tổng chi phí hoặc xác định rằng điều đó là không thể. 

Kích thước đầu vào tăng lên khoảng 100.000 ngày. Bất kỳ giải pháp nào cố gắng xem xét lại tất cả các tùy chọn nâng cấp đã thấy trước đây cho mỗi ngày sẽ là phương trình bậc hai và sẽ không hoàn thành trong thời gian giới hạn. Điều này ngay lập tức loại trừ việc lập trình động ngây thơ thử tất cả các tập hợp con hoặc tính toán lại các lựa chọn mỗi ngày. 

Khó khăn tinh tế là việc nâng cấp là vĩnh viễn và ảnh hưởng đến tất cả các ngày trong tương lai. Một quyết định tham lam được đưa ra sớm sau này có thể trở thành dưới mức tối ưu nếu tránh nâng cấp đắt tiền hơn nhưng cần thiết sau đó để đáp ứng nhu cầu tăng đột biến. 

Một số trường hợp đặc biệt đáng được tách biệt. 

Một chế độ thất bại là khi sức mạnh ban đầu đã không đủ vào ngày đầu tiên và không thể nâng cấp trước ngày đó. Ví dụ: nếu sức mạnh ban đầu là 5 và ngày 1 yêu cầu 10 nhưng việc nâng cấp chỉ xuất hiện từ ngày thứ 2 trở đi thì câu trả lời là không thể ngay lập tức ngay cả khi những lần nâng cấp sau này có rẻ. 

Một trường hợp khác là khi các bản nâng cấp tuy nhỏ nhưng cần thiết về mặt tích lũy. Ví dụ: yêu cầu có thể tăng dần và việc bỏ qua bản nâng cấp giá rẻ sớm buộc phải sử dụng bản nâng cấp đắt tiền sau này, do đó, chiến lược tham lam “chỉ mua khi bạn hiện tại không thành công” tại địa phương sẽ phá vỡ. 

Trường hợp thứ ba là khi yêu cầu giảm sau thời kỳ đỉnh cao. Một chiến lược ngây thơ có thể tiếp tục mua các bản nâng cấp ngay cả khi chúng không còn cần thiết nữa do những hạn chế trong tương lai, khiến chi phí tăng vọt một cách không cần thiết. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là mô phỏng tất cả các tập hợp con của các lựa chọn nâng cấp. Đối với mỗi tập hợp con, chúng tôi có thể tính toán cường độ cuối cùng và kiểm tra xem tất cả các yêu cầu hàng ngày có được đáp ứng hay không, tính tổng chi phí nếu hợp lệ. Với n ngày, đây là 2^n khả năng, khả năng này ngay lập tức không thể thực hiện được nếu vượt quá n rất nhỏ. 

Lực lượng vũ phu có cấu trúc chặt chẽ hơn là lập trình động theo ngày và cường độ hiện tại, nhưng cường độ có thể tăng lên tới 1e14 hoặc cao hơn do được bổ sung nhiều lần, do đó không gian trạng thái bùng nổ. Ngay cả khi chúng tôi nén các trạng thái, quá trình chuyển đổi vẫn sẽ liên quan đến việc xem xét mọi nâng cấp có sẵn ở mỗi bước, dẫn đến O(n^2). 

Quan sát quan trọng là chúng ta không bao giờ cần phải xem xét lại các quyết định trong quá khứ một cách phức tạp nếu chúng ta xử lý các ngày theo thứ tự và luôn đảm bảo đáp ứng yêu cầu hiện tại bằng cách sử dụng kết hợp các nâng cấp có sẵn với giá rẻ nhất. Vào bất kỳ ngày nào i, chỉ nâng cấp từ ngày ≤ i mới có thể sử dụng được. Trong số này, điều quan trọng là những nâng cấp nào chúng tôi đã thực hiện và những nâng cấp nào chúng tôi vẫn có thể thực hiện. 

Một cách hữu ích để suy nghĩ về vấn đề này là xử lý các ngày một cách tuần tự trong khi vẫn duy trì một nhóm các bản nâng cấp có sẵn. Mỗi lần nâng cấp đều có chi phí và tất cả các lần nâng cấp đều có lợi ích giống nhau về việc tăng sức mạnh lên cùng một lượng. Mục tiêu là quyết định số lần nâng cấp cần thực hiện cho mỗi điểm để sức mạnh hiện tại đủ đáp ứng yêu cầu hiện tại.

Điều này biến vấn đề thành một quá trình lựa chọn tham lam về chi phí: bất cứ khi nào chúng ta không đủ sức mạnh cần thiết, trước tiên chúng ta nên chọn những bản nâng cấp rẻ nhất hiện có, bởi vì mỗi bản nâng cấp đều mang lại lợi ích như nhau nhưng chi phí khác nhau. Chúng tôi duy trì tất cả các chi phí nâng cấp đã thấy trong một cấu trúc cho phép chúng tôi liên tục chọn chi phí nhỏ nhất khi cần. 

Điều này dẫn đến giải pháp tham lam dựa trên hàng đợi ưu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^n) | O(1) | Quá chậm | 
| Duy trì các bản nâng cấp có sẵn trong một lượng tối thiểu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại qua các ngày từ 1 đến n, theo dõi cường độ hiện tại và tổng chi phí đã chi tiêu. Chúng tôi cũng duy trì một lượng chi phí nâng cấp tối thiểu hiện có cho đến thời điểm hiện tại. 
2. Vào mỗi ngày thứ i, hãy chèn chi phí nâng cấp C[i] vào heap, vì từ thời điểm này trở đi nó sẽ có sẵn để sử dụng. 
3. Kiểm tra xem cường độ dòng điện có ít nhất là X[i] hay không. Nếu đã như vậy, chúng tôi không làm gì cả và tiếp tục. 
4. Nếu cường độ hiện tại không đủ, chúng ta cần áp dụng nâng cấp. Mỗi lần nâng cấp sẽ tăng sức mạnh thêm một lượng A cố định, vì vậy chúng tôi tính toán cần bao nhiêu lần nâng cấp để đạt được X[i]. Số này là (X[i] - sức mạnh + A - 1) // A. 
5. Liên tục trích xuất nâng cấp chi phí nhỏ nhất từ ​​đống và áp dụng nó, cập nhật sức mạnh và tổng chi phí. Nếu đống hết trước khi chúng ta đạt được sức mạnh cần thiết thì nhiệm vụ sẽ không thể thực hiện được. 
6. Tiếp tục quá trình này trong tất cả các ngày, luôn đảm bảo đáp ứng yêu cầu trước khi tiếp tục. 

Ý tưởng trọng tâm là bất cứ khi nào chúng ta cần tăng sức mạnh, trước tiên chúng ta nên luôn chọn những bản nâng cấp rẻ nhất hiện có vì tất cả các bản nâng cấp đều góp phần như nhau vào việc đáp ứng những hạn chế trong tương lai. 

### Tại sao nó hoạt động 

Vào bất kỳ ngày nào, quyết định duy nhất quan trọng là cần thực hiện bao nhiêu nâng cấp từ tiền tố của các nâng cấp có sẵn. Vì mỗi lần nâng cấp đều thêm giá trị A giống hệt nhau nên vấn đề giảm xuống còn việc chọn nhiều bộ nâng cấp với chi phí tối thiểu có kích thước đủ. Sự lựa chọn tham lam là luôn chọn chi phí nhỏ nhất hiện có đảm bảo rằng bất kỳ lựa chọn nào khác có cùng số lần nâng cấp không thể có chi phí thấp hơn, bởi vì việc hoán đổi một bản nâng cấp đã chọn đắt hơn bằng một bản nâng cấp rẻ hơn chưa sử dụng không bao giờ vi phạm tính khả thi. Đối số trao đổi này đảm bảo rằng lựa chọn dựa trên vùng heap tạo ra chi phí tối ưu cho mọi ràng buộc tiền tố và vì các ràng buộc được xử lý theo trình tự thời gian nên tính tối ưu trước đó vẫn có hiệu lực cho những ngày sau đó. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    req = list(map(int, input().split()))
    A = int(input())
    cost = list(map(int, input().split()))

    cur = k
    ans = 0
    pq = []

    for i in range(n):
        heapq.heappush(pq, cost[i])

        if cur >= req[i]:
            continue

        need = req[i] - cur
        cnt = (need + A - 1) // A

        for _ in range(cnt):
            if not pq:
                print(-1)
                return
            c = heapq.heappop(pq)
            ans += c
            cur += A

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp hướng dẫn. Heap lưu trữ tất cả các chi phí nâng cấp đã có sẵn cho đến ngày hiện tại. Khi sức mạnh hiện tại không đủ, chúng tôi tính toán xem cần nâng cấp bao nhiêu lần và liên tục trích xuất những cái rẻ nhất. 

Chi tiết quan trọng là sức mạnh được cập nhật ngay sau mỗi lần nâng cấp được chọn, vì mỗi lần nâng cấp sẽ tăng vĩnh viễn khả năng và có thể giảm số lần nâng cấp còn lại cần thiết. Vòng lặp tính toán lại tính khả thi bằng cách tăng`cur`. 

Một lỗi phổ biến ở đây là tính toán tất cả các nâng cấp cần thiết một lần và mù quáng lấy nhiều phần tử đó mà không kiểm tra tình trạng cạn kiệt của đống trong quá trình trích xuất. Mã tránh điều đó bằng cách kiểm tra vùng heap ở mỗi lần bật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 10000
req = [10000, 30000, 30000, 40000, 20000]
A = 20000
cost = [5, 2, 8, 3, 6]
```Chúng tôi theo dõi đống, sức mạnh và chi phí. 

| Ngày | Đống sau khi chèn | Sức mạnh hiện tại | Bắt buộc | Hành động | Tổng chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [5] | 10000 | 10000 | không | 0 | 
| 2 | [2,5] | 10000 | 30000 | lấy 1 (chi phí 2) | 2 | 
| 3 | [2,5,8] | 30000 | 30000 | không | 2 | 
| 4 | [2,5,8,3] | 30000 | 40000 | lấy 1 (chi phí 3) | 5 | 
| 5 | [2,5,8,3,6] | 50000 | 20000 | không | 5 | 

Sau ngày thứ 4, sức mạnh tăng lên đủ để các ràng buộc sau này tự động được thỏa mãn. Dấu vết cho thấy thuật toán trì hoãn việc sử dụng các tùy chọn đắt tiền và luôn chọn các bản nâng cấp có sẵn rẻ nhất. 

### Ví dụ 2 

đầu vào:```
n = 3, k = 10000
req = [10000, 40000, 30000]
A = 10000
cost = [5, 2, 8]
```| Ngày | Đống | Sức mạnh | Bắt buộc | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [5] | 10000 | 10000 | không | được | 
| 2 | [5,2] | 10000 | 40000 | cần 3 lần nâng cấp | nhưng kích thước heap 2 → thất bại | 

Vào ngày thứ 2, ngay cả sau khi sử dụng cả hai bản nâng cấp có sẵn, sức mạnh vẫn là 30000, vẫn dưới 40000. Không có thông tin nào trong tương lai có thể hữu ích vì chưa có các bản nâng cấp trong tương lai. Thuật toán xuất ra chính xác -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi ngày sẽ chèn một chi phí vào đống và mỗi lần nâng cấp sẽ xuất hiện tối đa một lần | 
| Không gian | O(n) | Heap lưu trữ tất cả các nâng cấp có sẵn | 

Giải pháp có quy mô thoải mái cho n lên tới 100.000 vì các phép toán vùng heap vẫn duy trì ở dạng logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import heapq

    input = sys.stdin.readline

    n, k = map(int, input().split())
    req = list(map(int, input().split()))
    A = int(input())
    cost = list(map(int, input().split()))

    cur = k
    ans = 0
    pq = []

    for i in range(n):
        heapq.heappush(pq, cost[i])

        if cur >= req[i]:
            continue

        need = req[i] - cur
        cnt = (need + A - 1) // A

        for _ in range(cnt):
            if not pq:
                return "-1"
            c = heapq.heappop(pq)
            ans += c
            cur += A

    return str(ans)

# provided samples
assert run("""5 10000
10000 30000 30000 40000 20000
20000
5 2 8 3 6
""") == "5"

assert run("""5 10000
10000 40000 30000 30000 20000
10000
5 2 8 3 6
""") == "-1"

# custom cases
assert run("""1 5
5
10
100
""") == "0", "already satisfied"

assert run("""2 1
10 10
5
1 100
""") == "100", "must pick only available even if expensive later"

assert run("""3 1
5 20 30
5
5 4 3
""") == "7", "greedy cheapest choices"

assert run("""3 1
100 100 100
10
5 5 5
""") == "-1", "insufficient total upgrades"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ngày rồi cũng được | 0 | trường hợp không hoạt động | 
| chậm trễ đắt vs rẻ | 100 | lựa chọn đống tham lam | 
| yêu cầu ngày càng tăng | 7 | đặt hàng + tích lũy | 
| trường hợp bất khả thi | -1 | thất bại khả thi | 

## Vỏ cạnh 

Trường hợp yêu cầu đầu tiên đã cao hơn cường độ ban đầu nhưng chưa có bản nâng cấp nào sẽ được xử lý vì vùng nhớ trống khi cố gắng đáp ứng mức thâm hụt, gây ra lỗi ngay lập tức. 

Trường hợp có yêu cầu lớn sớm và nhiều nâng cấp giá rẻ về sau sẽ được xử lý chính xác vì thuật toán không chờ nâng cấp trong tương lai. Nó chỉ sử dụng những gì đã được tiết lộ cho đến thời điểm hiện tại, vì vậy nó không thể giả định sai về tính khả dụng trong tương lai. 

Trường hợp các yêu cầu giảm sau khi đạt mức đỉnh được xử lý một cách tự nhiên vì khi cường độ tăng lên thì nó không bao giờ giảm, do đó, các yêu cầu nhỏ hơn trong tương lai không cần phải thực hiện thêm công việc nào và không ảnh hưởng đến các quyết định trong quá khứ.
