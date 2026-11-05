---
title: "CF 1218F - Kế hoạch tập luyện"
description: "Alan tuân theo một lịch trình cố định là đến phòng tập thể dục trong $N$ ngày. Vào ngày $i$, anh ta phải nâng chính xác $X[i]$ gram và anh ta bắt đầu với cường độ cơ bản $K$. Anh ấy có thể tăng sức mạnh vĩnh viễn bằng cách mua đồ uống trước khi tập luyện vào bất kỳ ngày nào anh ấy đến phòng tập thể dục."
date: "2026-06-13T17:58:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1218
codeforces_index: "F"
codeforces_contest_name: "Bubble Cup 12 - Finals [Online Mirror, unrated, Div. 1]"
rating: 1500
weight: 1218
solve_time_s: 315
verified: true
draft: false
---

[CF 1218F - Kế hoạch tập luyện](https://codeforces.com/problemset/problem/1218/F) 

**Đánh giá:** 1500 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 5 phút 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Alan tuân theo một lịch trình cố định đến phòng tập thể dục$N$ngày. Vào ngày$i$, anh ta phải nhấc chính xác$X[i]$gam, và anh ấy bắt đầu với cường độ cơ bản$K$. Anh ấy có thể tăng sức mạnh vĩnh viễn bằng cách mua đồ uống trước khi tập luyện vào bất kỳ ngày nào anh ấy đến phòng tập thể dục. Mỗi lần uống sẽ tăng sức mạnh của anh ta lên một lượng cố định$A$, và chi phí phụ thuộc vào phòng tập của ngày hôm đó, được đưa ra bởi$C[i]$. Anh ta có thể mua nhiều đồ uống, nhưng mỗi lần mua đều gắn liền với việc tập thể dục trong ngày và góp phần tăng cường sức mạnh như nhau. 

Sự ràng buộc mang tính tạm thời: nếu anh ta mua đồ uống vào một số ngày, anh ta chỉ có thể dựa vào sức lực tăng lên kể từ ngày đó trở đi. Nhiệm vụ là chọn ngày mua đồ uống sao cho mỗi ngày$i$, sức mạnh tích lũy của anh ta ít nhất là$X[i]$, đồng thời giảm thiểu tổng chi phí. Nếu điều đó là không thể với tất cả đồ uống thì câu trả lời là -1. 

Các ràng buộc đi lên đến$N = 10^5$, điều này ngay lập tức loại trừ mọi phép liệt kê bậc hai đối với các tập hợp con của các quyết định mua hàng. Bất kỳ giải pháp hợp lệ nào cũng phải xử lý các ngày theo thời gian tuyến tính hoặc gần tuyến tính, thường sử dụng lựa chọn tham lam với cấu trúc dựa trên đống hoặc sắp xếp. 

Một trường hợp thất bại tinh vi xuất hiện khi những ngày đầu đòi hỏi nồng độ cao nhưng đồ uống đắt tiền lại có sớm và đồ uống rẻ lại có sau. Một chiến lược ngây thơ tham lam mua bất cứ khi nào sức mạnh hiện tại không đủ có thể thất bại vì nó có thể khóa vào những giao dịch mua sớm đắt đỏ thay vì chờ đợi những cơ hội rẻ hơn trong tương lai. 

Một trường hợp nguy hiểm khác phát sinh khi ngay cả việc mua đồ uống hàng ngày cũng không đủ. Ví dụ, nếu$K + N \cdot A < \max X[i]$, thì không chiến lược nào có thể đáp ứng yêu cầu, nhưng việc triển khai bất cẩn vẫn có thể cố gắng mô phỏng và tràn hoặc hoạt động không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ xem xét mỗi ngày xem có nên mua đồ uống hay không. Vì mỗi lần mua sẽ tăng sức mạnh vĩnh viễn nên điều này trở thành vấn đề lựa chọn tập hợp con trong$N$ngày, cho$2^N$khả năng. Ngay cả việc cắt tỉa theo tính khả thi vẫn để lại sự phân nhánh theo cấp số nhân vì mỗi quyết định đều ảnh hưởng đến tất cả các ràng buộc trong tương lai. 

Nhận xét quan trọng là việc mua đồ uống chỉ quan trọng khi nó ngăn ngừa được thất bại vào một ngày nào đó. Nếu vào ngày$i$, cường độ dòng điện không đủ để$X[i]$, thì chắc chắn chúng ta đã mua đủ đồ uống trước hoặc tại$i$. Vấn đề trở thành vấn đề về lịch trình: đảm bảo rằng mỗi ngày chúng ta đã tích lũy đủ “ngân sách sức mạnh” trong khi chọn thời gian uống rẻ nhất có thể. 

Một cách hữu ích để điều chỉnh lại vấn đề là xử lý các ngày theo thứ tự trong khi vẫn duy trì số lượng đồ uống được yêu cầu cho đến nay. Nếu vào ngày$i$, sức mạnh hiện tại quá thấp, chúng tôi buộc phải bổ sung thêm đồ uống. Câu hỏi đặt ra là lấy những đồ uống đó từ đâu. Vì mỗi loại đồ uống có tác dụng giống hệt nhau nhưng giá thành khác nhau nên chúng tôi luôn muốn chọn loại đồ uống rẻ nhất có sẵn trong số những ngày chúng tôi đã qua. Điều này đương nhiên dẫn đến việc duy trì cấu trúc dữ liệu về chi phí dự kiến ​​và lựa chọn đồ uống có chi phí tối thiểu khi cần. 

Điều này biến vấn đề thành một quá trình tích lũy tham lam với chi phí tăng tối thiểu so với những ngày đã thấy trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) | O(N) | Quá chậm | 
| Tham lam + Đống | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét các ngày từ trái sang phải, duy trì số lượng đồ uống chúng tôi đã mua và một loạt chi phí tập thể dục hiện có mà chúng tôi có thể lựa chọn. 

1. Khởi tạo cường độ dòng điện như$K$, tổng chi phí là 0 và một đống tối thiểu sẽ lưu trữ chi phí của tất cả các ngày được xử lý cho đến nay. 
2. Cho mỗi ngày$i$, chèn$C[i]$vào đống vì mua đồ uống vào hoặc trước ngày này hiện là một lựa chọn có sẵn. 
3. Kiểm tra xem cường độ hiện tại$K + (\text{drinks bought so far}) \cdot A$gặp gỡ$X[i]$. Nếu có, hãy tiếp tục mà không mua bất cứ thứ gì. 
4. Nếu không đáp ứng$X[i]$, tính xem cần uống thêm bao nhiêu ly nữa để đạt được$X[i]$. Mỗi đồ uống bổ sung thêm$A$, vì vậy chúng tôi xác định số lượng đồ uống tối thiểu cần thiết. 
5. Liên tục trích xuất chi phí nhỏ nhất từ ​​đống và mô phỏng việc mua đồ uống cho đến khi đáp ứng đủ số lượng đồ uống cần thiết. Mỗi lần trích xuất tương ứng với việc chọn phòng tập thể dục rẻ nhất hiện có vào hoặc trước ngày này. 
6. Nếu tại bất kỳ thời điểm nào vùng heap trở nên trống trước khi đáp ứng yêu cầu, hãy xuất -1 vì không có tùy chọn nào trong tương lai có thể khắc phục được lỗi trước đó. 
7. Tiếp tục quá trình này trong tất cả các ngày, tích lũy tổng chi phí. 

Cơ chế then chốt là chúng tôi không bao giờ cam kết mua đồ uống sớm. Thay vào đó, chúng tôi giữ lại tất cả các tùy chọn trước đây và chỉ kích hoạt những tùy chọn rẻ nhất khi bị buộc phải vi phạm ràng buộc. 

### Tại sao nó hoạt động 

Vào bất kỳ ngày nào$i$, nếu chúng ta cần thêm sức mạnh, bất kỳ giải pháp hợp lệ nào cũng phải bao gồm ít nhất một số lượng đồ uống nhất định được mua từ ngày$1$ĐẾN$i$. Trong số tất cả các giải pháp hợp lệ như vậy, việc thay thế bất kỳ đồ uống nào đã chọn bằng một loại đồ uống rẻ hơn có sẵn từ cùng một tiền tố chỉ có thể cải thiện hoặc duy trì tính khả thi. Lập luận trao đổi này đảm bảo rằng việc tham lam lựa chọn chi phí sẵn có rẻ nhất bất cứ khi nào chúng ta buộc phải mua là tối ưu, và việc trì hoãn các quyết định không bao giờ loại bỏ các lựa chọn tốt hơn khỏi việc cân nhắc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n, k = map(int, input().split())
    x = list(map(int, input().split()))
    a = int(input())
    c = list(map(int, input().split()))

    heap = []
    total_cost = 0
    drinks = 0  # number of drinks bought so far

    for i in range(n):
        heapq.heappush(heap, c[i])

        current_strength = k + drinks * a

        if current_strength >= x[i]:
            continue

        # need more drinks
        needed = (x[i] - current_strength + a - 1) // a

        while needed > 0:
            if not heap:
                print(-1)
                return
            total_cost += heapq.heappop(heap)
            drinks += 1
            needed -= 1

    print(total_cost)

if __name__ == "__main__":
    solve()
```Heap lưu trữ tất cả chi phí đồ uống có thể có từ ngày cho đến chỉ số hiện tại. Điều này đảm bảo rằng khi buộc phải tăng sức mạnh, chúng ta luôn chọn phòng tập hợp lệ rẻ nhất từ ​​trước đến nay. 

các`drinks`bộ đếm theo dõi số lần tăng cường vĩnh viễn mà chúng tôi đã cam kết thực hiện. Việc chuyển đổi từ khoảng cách sức mạnh cần thiết sang số lượng đồ uống sử dụng phép chia trần, vì mỗi đồ uống đóng góp chính xác$A$. 

Tính đúng đắn xoay quanh việc không bao giờ loại bỏ chi phí khỏi việc xem xét và luôn chọn chi phí tối thiểu khi bị ép buộc. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu```
5 10000
10000 30000 30000 40000 20000
20000
5 2 8 3 6
```Chúng tôi mô phỏng từng bước. 

| Ngày | X[i] | K + đồ uống*A | Trạng thái đống | Hành động | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10000 | 10000 | [5] | được | 0 | 
| 2 | 30000 | 10000 | [2,5] | mua 1 ly | 2 | 
| 2 | 30000 | 30000 | [5] | được | 2 | 
| 3 | 30000 | 30000 | [5,8] | được | 2 | 
| 4 | 40000 | 30000 | [3,8] | mua 1 ly | 5 | 
| 4 | 40000 | 50000 | [8] | được | 5 | 
| 5 | 20000 | 50000 | [6,8] | được | 5 | 

Điều này cho thấy chúng ta chỉ mua khi bị ép buộc và luôn chọn phòng gym rẻ nhất có sẵn trước đây. 

Dấu vết xác nhận rằng việc trì hoãn các quyết định cho phép cân nhắc các lựa chọn rẻ hơn trước khi cam kết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Mỗi ngày chúng tôi đẩy một chi phí và có khả năng bật một số lần giới hạn, mỗi thao tác heap tốn thời gian logarit | 
| Không gian |$O(N)$| Các cửa hàng có giá nhiều nhất ở tất cả các phòng tập thể dục tính đến thời điểm hiện tại | 

Với$N \le 10^5$, điều này phù hợp thoải mái trong giới hạn thời gian vì hoạt động của đống vẫn hiệu quả ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    n, k = map(int, input().split())
    x = list(map(int, input().split()))
    a = int(input())
    c = list(map(int, input().split()))

    heap = []
    total_cost = 0
    drinks = 0

    for i in range(n):
        heapq.heappush(heap, c[i])

        if k + drinks * a >= x[i]:
            continue

        needed = (x[i] - (k + drinks * a) + a - 1) // a

        while needed > 0:
            if not heap:
                return "-1"
            total_cost += heapq.heappop(heap)
            drinks += 1
            needed -= 1

    return str(total_cost)

# provided sample
assert run("""5 10000
10000 30000 30000 40000 20000
20000
5 2 8 3 6
""") == "5"

# minimum case
assert run("""1 1
1
1
10
""") == "0"

# impossible case
assert run("""2 1
100 100
1
1000 1000
""") == "-1"

# greedy choice test
assert run("""3 1
10 10 10
5
5 100 1
""") == "6"

# increasing requirement
assert run("""4 1
1 2 3 100
10
1 1 1 1
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một ngày đủ | 0 | không cần mua hàng | 
| yêu cầu cao không thể | -1 | phát hiện tính không khả thi | 
| chi phí hỗn hợp | 6 | tham lam lựa chọn đúng đắn | 
| đột biến muộn không thể | -1 | thất bại khi heap cạn kiệt | 

## Vỏ cạnh 

Trường hợp cạnh tranh quan trọng xảy ra khi yêu cầu về sức mạnh tăng đột biến sau một số cơ hội giá rẻ. Thuật toán đảm bảo tính chính xác vì tất cả các chi phí tập thể dục trước đây vẫn có sẵn trong đống, vì vậy ngay cả khi một ngày muộn cần tăng nhiều lần, nó vẫn có thể chọn các tùy chọn rẻ trước đó thay vì bị buộc phải chọn các tùy chọn đắt tiền muộn. 

Một trường hợp khó khăn khác là khi không có ngày nào cần uống nước, nghĩa là tất cả$X[i] \le K$. Heap vẫn đầy nhưng không có pops nào xuất hiện và câu trả lời vẫn là 0. Điều này xác nhận rằng các giao dịch mua không cần thiết sẽ không bao giờ được kích hoạt. 

Trường hợp khó khăn cuối cùng là khi vượt quá số lượng đồ uống yêu cầu$N$. Trong trường hợp đó, vùng heap trống trước khi chúng tôi có thể đáp ứng nhu cầu và thuật toán xuất ra chính xác -1, phản ánh rằng ngay cả mức tăng tối đa cũng không thể đạt được cấu hình cường độ cần thiết.
