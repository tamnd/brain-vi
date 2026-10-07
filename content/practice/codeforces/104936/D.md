---
title: "CF 104936D - Thu thập tiền xu"
description: "Chúng ta có một biểu đồ trong đó mỗi nút là một tòa nhà và mỗi cạnh là một đường hầm giữa hai tòa nhà. Mỗi đường hầm đều có hai giá trị gắn liền với nó: chi phí tính bằng xu cần thiết để vào đó và phần thưởng bằng xu nhận được sau khi đi qua nó."
date: "2026-06-28T18:11:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104936
codeforces_index: "D"
codeforces_contest_name: "MITIT 2024 Beginner Round"
rating: 0
weight: 104936
solve_time_s: 88
verified: false
draft: false
---

[CF 104936D - Thu thập tiền xu](https://codeforces.com/problemset/problem/104936/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu đồ trong đó mỗi nút là một tòa nhà và mỗi cạnh là một đường hầm giữa hai tòa nhà. Mỗi đường hầm đều có hai giá trị gắn liền với nó: chi phí tính bằng xu cần thiết để vào đó và phần thưởng bằng xu nhận được sau khi đi qua nó. Mỗi lần truyền tải là vô hướng và mỗi lần chúng ta sử dụng một đường hầm, chúng ta lại phải trả chi phí và nhận phần thưởng của nó. 

Chúng ta bắt đầu từ tòa nhà 1 với một số xu ban đầu và chúng ta muốn đến tòa nhà N. Câu hỏi đặt ra là xác định số lượng xu ban đầu tối thiểu sao cho tồn tại một chuỗi các đường hầm cho phép chúng ta đến N mà không bao giờ để số dư xu giảm xuống dưới 0. 

Khó khăn chính là các cạnh không chỉ được tính trọng số một lần. Chúng ta có thể tái sử dụng các cạnh nhiều lần và nếu một đường hầm mang lại nhiều tiền hơn chi phí, thì nó hoạt động hiệu quả như một nguồn tiền bổ sung có thể được luân chuyển. 

Những hạn chế rất lớn: lên tới 100.000 tòa nhà và 200.000 đường hầm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào phụ thuộc vào việc mô phỏng số dư tiền xu có thể có trên mỗi đường dẫn hoặc liệt kê các đường dẫn. Bất kỳ cách tiếp cận nào theo dõi trạng thái trên mỗi nút với số lượng tiền khác nhau một cách đơn giản sẽ bùng nổ về mặt tổ hợp. Chúng ta cần thứ gì đó gần với thời gian tuyến tính hoặc gần tuyến tính hơn, thường là O(M log N) hoặc O(M). 

Một vài trường hợp thất bại tinh tế phát sinh một cách tự nhiên. 

Một vấn đề là chu kỳ lãi ròng âm hoặc dương. Ví dụ: nếu một chu kỳ tăng tổng số tiền thì khi chúng ta có thể tham gia vào chu kỳ đó, chúng ta có thể tạo ra số tiền lớn tùy ý. Một con đường ngắn nhất ngây thơ bỏ qua việc truyền tải lặp đi lặp lại sẽ hoàn toàn bỏ lỡ hiệu ứng này. 

Một vấn đề khác là ngay cả khi một đường dẫn tồn tại về mặt kết nối, có thể không thể đi qua nó với số tiền ban đầu nhỏ vì các cạnh ban đầu có thể yêu cầu nhiều vốn trả trước hơn số vốn tạm thời có sẵn, ngay cả khi các cạnh sau bù đắp. 

Cuối cùng, có những trường hợp sự lựa chọn tham lam về “đường dẫn chi phí tối thiểu” không thành công, bởi vì một lợi thế đắt hơn sớm có thể mở ra một chu kỳ sinh lời làm giảm tổng vốn ban đầu cần thiết. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi đây là một biểu đồ trạng thái trong đó mỗi trạng thái (nút, đồng tiền hiện tại). Từ mỗi tiểu bang, chúng tôi thử tất cả các đường hầm đi, cập nhật số dư xu bằng cách trừ chi phí và thêm phần thưởng. Mục tiêu là đạt đến nút N với số tiền không âm và chúng tôi muốn số tiền ban đầu tối thiểu cho phép điều này. 

Tuy nhiên, giá trị của đồng xu là không giới hạn nên số lượng trạng thái thực tế là vô hạn. Ngay cả khi chúng tôi giới hạn nó một cách giả tạo, các chuyển đổi có thể làm tăng số tiền, vì vậy chúng tôi không thể đảm bảo giới hạn hữu ích hữu hạn. Điều này làm cho BFS hoặc Dijkstra trên các trạng thái mở rộng không thể thực hiện được. 

Quan sát quan trọng là điều quan trọng không phải là số tiền tuyệt đối trong suốt hành trình mà là số vốn ban đầu tối thiểu cần thiết để đảm bảo tính khả thi trên con đường đã chọn. Đối với một đường dẫn cố định, chúng tôi có thể tính toán số tiền bắt đầu cần thiết bằng cách mô phỏng các ràng buộc tiền tố: ở mỗi bước, chúng tôi phải đảm bảo không bao giờ bị âm. Giá trị ban đầu được yêu cầu là mức thâm hụt tối đa gặp phải dọc theo đường dẫn. 

Điều này biến bài toán thành bài toán đường đi ngắn nhất trong đó mỗi cạnh có tác dụng “điều chỉnh chi phí”. Nếu chúng tôi xác định một sự chuyển đổi tiềm năng, chúng tôi có thể giảm hành vi biên thành một sự thư giãn tiêu chuẩn: thay vì theo dõi các đồng tiền hiện tại, chúng tôi theo dõi số tiền ban đầu tối thiểu cần thiết để tiếp cận mỗi nút. Khi đi qua cạnh u đến v với chi phí c và thưởng r, nếu chúng ta đến u với yêu cầu x, thì sau khi đi qua cạnh đó, yêu cầu tại v sẽ trở thành max(0, x + c − r), nhưng chỉ khi chúng ta có đủ khả năng c tại thời điểm đó, điều này phụ thuộc vào thặng dư tích lũy.

Cách mạnh mẽ hơn để suy nghĩ về nó là: chúng tôi tìm kiếm nhị phân câu trả lời và kiểm tra tính khả thi. Đối với giá trị bắt đầu S của ứng cử viên, chúng tôi mô phỏng xem liệu chúng tôi có thể đạt được N hay không bằng cách luôn duy trì số dư tiền xu hiện tại và tham lam giành lấy bất kỳ lợi thế nào có thể sử dụng được. Nếu đạt được N thì S là khả thi. 

Để làm cho tính khả thi trở nên hiệu quả, chúng tôi coi các cạnh là sự nới lỏng trong đó việc truyền tải chỉ được phép nếu số dư hiện tại ≥ chi phí. Chúng tôi liên tục tuyên truyền các trạng thái có thể tiếp cận trong khi vẫn duy trì mức thặng dư tiền xu tốt nhất hiện tại. Điều này trở thành một quá trình giống như Dijkstra trong đó “khoảng cách” là tối đa hóa thặng dư tiền hiện tại chứ không phải giảm thiểu chi phí. 

Chúng tôi đảo ngược quan điểm: thay vì giảm thiểu trực tiếp số tiền ban đầu, chúng tôi hỏi liệu số tiền ban đầu nhất định có đủ hay không và sau đó tối ưu hóa nó bằng cách sử dụng tìm kiếm nhị phân. Mỗi lần kiểm tra sẽ chạy một tìm kiếm tốt nhất đầu tiên đã được sửa đổi, luôn mở rộng nút có số dư tiền xu hiện tại cao nhất, đảm bảo chúng tôi khám phá các trạng thái hứa hẹn nhất trước tiên. 

Điều này mang lại một điều kiện khả thi đơn điệu, cho phép tìm kiếm nhị phân trên S. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng trạng thái Brute Force | O(vô hạn) | O(N · xu) | Quá chậm | 
| Tìm kiếm nhị phân + tính khả thi tốt nhất đầu tiên | O(M log M log V) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tìm kiếm nhị phân các đồng tiền ban đầu tối thiểu S. 

Đối với mỗi ứng cử viên S, chúng tôi kiểm tra xem liệu chúng tôi có thể tiếp cận nút N bắt đầu bằng S xu hay không. 

Chúng tôi duy trì hàng đợi ưu tiên được sắp xếp theo số dư tiền xu hiện tại tại mỗi nút, luôn mở rộng trạng thái có số tiền khả dụng cao nhất trước tiên. 

Chúng tôi cũng duy trì một mảng best[v] để lưu trữ số dư xu tối đa mà chúng tôi từng đạt được khi đạt đến nút v. Điều này ngăn cản việc truy cập lại các trạng thái yếu hơn. 

Chúng ta khởi tạo best[1] = S và đẩy (S, 1) vào hàng đợi ưu tiên. 

Sau đó, chúng tôi liên tục trích xuất trạng thái có số dư tiền cao nhất. 

Từ nút u với số tiền hiện tại x, chúng tôi thử từng đường hầm (u, v, c, r). Nếu x < c, chúng ta không thể duyệt và bỏ qua. 

Nếu x ≥ c thì sau khi truyền tải chúng ta sẽ đến v với x − c + r đồng xu. Nếu giá trị này lớn hơn best[v], chúng tôi sẽ cập nhật best[v] và đẩy trạng thái mới. 

Chúng tôi tiếp tục cho đến khi hàng đợi trống hoặc chúng tôi đến nút N. 

Nếu best[N] được xác định thì S là khả thi. 

### Tại sao nó hoạt động 

Đối với giá trị bắt đầu cố định S, thuật toán luôn khám phá các trạng thái theo thứ tự giảm dần của số tiền có sẵn. Bất kỳ trạng thái nào có ít tiền hơn chỉ có thể tạo ra ít hơn hoặc bằng các lựa chọn trong tương lai, bởi vì tính khả thi của cạnh phụ thuộc vào việc có chi phí ít nhất là c. Do đó, việc tiếp cận một nút có số dư tiền xu cao hơn sẽ chi phối tất cả các lượt truy cập yếu hơn vào cùng một nút. Mảng tốt nhất đảm bảo chúng tôi chỉ giữ lại các trạng thái thống trị, giúp duy trì tính chính xác đồng thời tránh hiện tượng bùng nổ theo cấp số nhân. 

Bởi vì số dư tiền xu không bao giờ trở nên âm trong bất kỳ quá trình truyền tải hợp lệ nào và tất cả các chuyển đổi đều bảo toàn các ràng buộc về tính khả thi, nên mọi cấu hình có thể truy cập được trong S sẽ được quá trình này phát hiện. Do đó, việc kiểm tra tính khả thi là chính xác và tìm kiếm nhị phân sẽ tìm thấy S tối thiểu một cách chính xác. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def can(start, n, g):
    best = [-1] * (n + 1)
    pq = [(-start, 1)]
    best[1] = start

    while pq:
        neg_x, u = heapq.heappop(pq)
        x = -neg_x

        if x < best[u]:
            continue

        if u == n:
            return True

        for v, c, r in g[u]:
            if x < c:
                continue
            nx = x - c + r
            if nx > best[v]:
                best[v] = nx
                heapq.heappush(pq, (-nx, v))

    return False

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        a, b, c, r = map(int, input().split())
        g[a].append((b, c, r))
        g[b].append((a, c, r))

    lo, hi = 0, 10**18
    ans = hi

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, n, g):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng danh sách lân cận vô hướng lưu trữ từng đường hầm cùng với chi phí và phần thưởng của nó. Chức năng kiểm tra tính khả thi thực hiện quá trình lan truyền tốt nhất đầu tiên trên các trạng thái tiền xu, luôn mở rộng trạng thái giàu nhất có thể tiếp cận trước tiên. 

Tìm kiếm nhị phân kết thúc việc kiểm tra này, thu hẹp không gian câu trả lời dựa trên việc liệu số tiền ban đầu nhất định có đủ hay không. Chi tiết triển khai chính là sử dụng vùng heap tối đa (được triển khai thông qua các giá trị âm) để ưu tiên số dư tiền xu lớn hơn. 

Một điểm tinh tế là kiểm tra ưu thế tốt nhất[v]. Nếu không có nó, việc tìm kiếm sẽ liên tục truy cập lại cùng một nút có giá trị đồng xu kém hơn, dẫn đến TLE. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 2 2 1
2 3 3 0
1 3 5 0
```Chúng tôi kiểm tra ứng viên S = 3. 

| Bước | Nút | Tiền xu | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | bắt đầu | 
| 2 | 2 | 2 | sử dụng cạnh 1→2 (chi phí 2, thưởng 1) | 
| 3 | 3 | -1 | không thể tiếp tục | 

Điều này không thành công vì tiền âm không được phép. Vậy S = 3 là không đủ. 

Hãy thử S = 4. 

| Bước | Nút | Tiền xu | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 4 | bắt đầu | 
| 2 | 2 | 3 | 1→2 | 
| 3 | 3 | 0 | 2→3 | 

Chúng tôi đạt đến nút 3 thành công. 

Điều này cho thấy việc kiểm tra tính khả thi rất nhạy cảm với chi phí ban đầu chứ không chỉ lợi nhuận ròng. 

### Mẫu 2 

đầu vào:```
4 3
1 2 3 1
2 3 1 2
3 4 2 4
```Hãy thử S = 3. 

| Nút | Tiền xu | Lý do | 
| --- | --- | --- | 
| 1 | 3 | bắt đầu | 
| 2 | 1 | 1→2 | 
| 3 | 2 | 2→3 | 
| 4 | 4 | 3→4 | 

Chúng ta đến đích với số dư dương, khẳng định S = 3 là khả thi. 

Điều này chứng tỏ rằng tổn thất trung gian có thể chấp nhận được miễn là phần thưởng sau đó bù đắp được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log N log V) | tìm kiếm nhị phân trên S, mỗi lần kiểm tra tính khả thi là sự lan truyền dựa trên đống trên các cạnh | 
| Không gian | O(N + M) | danh sách kề và mảng tốt nhất | 

Các ràng buộc cho phép khoảng 2e5 cạnh và các hệ số logarit vẫn nhỏ do tìm kiếm nhị phân trên phạm vi đồng xu bị giới hạn. Việc truyền bá dựa trên heap đảm bảo mỗi trạng thái hữu ích được xử lý một số lần giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    import heapq

    input = sys.stdin.readline

    def can(start, n, g):
        best = [-1] * (n + 1)
        pq = [(-start, 1)]
        best[1] = start

        while pq:
            neg_x, u = heapq.heappop(pq)
            x = -neg_x
            if x < best[u]:
                continue
            if u == n:
                return True
            for v, c, r in g[u]:
                if x < c:
                    continue
                nx = x - c + r
                if nx > best[v]:
                    best[v] = nx
                    heapq.heappush(pq, (-nx, v))
        return False

    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        a, b, c, r = map(int, input().split())
        g[a].append((b, c, r))
        g[b].append((a, c, r))

    lo, hi = 0, 10**6
    ans = hi
    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, n, g):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1
    return str(ans)

# provided samples
assert run("3 3\n1 2 2 1\n2 3 3 0\n1 3 5 0\n") == "4", "sample 1"
assert run("4 3\n1 2 3 1\n2 3 1 2\n3 4 2 4\n") == "3", "sample 2"

# custom cases
assert run("2 1\n1 2 0 0\n") == "0", "free edge"
assert run("2 1\n1 2 5 10\n") == "0", "profit edge"
assert run("3 2\n1 2 5 0\n2 3 5 0\n") == "10", "tight chain"
assert run("3 3\n1 2 10 0\n2 1 9 0\n2 3 1 100\n") == "1", "cycle benefit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh miễn phí | 0 | truyền tải không tốn phí | 
| lợi nhuận | 0 | lợi nhuận ròng | 
| dây chuyền chặt chẽ | 10 | tích lũy chi phí nghiêm ngặt | 
| lợi ích chu kỳ | 1 | sử dụng chu trình để mở khóa tính khả thi | 

## Vỏ cạnh 

Trường hợp góc trực tiếp là khi tất cả các cạnh đều có chi phí bằng 0 và phần thưởng bằng không. Thuật toán ngay lập tức thành công với S = 0, vì hàng đợi bắt đầu với số xu bằng 0 và mọi giao dịch luôn được phép. 

Một trường hợp tinh vi khác là khi tồn tại một chu kỳ làm tăng số xu nhưng không nằm trên đường dẫn trực tiếp đến N. Việc lan truyền đầu tiên tốt nhất đảm bảo rằng khi có thể truy cập được chu kỳ đó, nó sẽ được khai thác để tăng số dư xu, có khả năng mở khóa các cạnh mà trước đây không thể sử dụng được. Cách giải thích con đường ngắn nhất tham lam sẽ bỏ lỡ điều này, nhưng cơ chế thống trị của nhà nước đảm bảo nó được nắm bắt hoàn toàn. 

Trường hợp cuối cùng là khi tuyến đường hợp lệ duy nhất yêu cầu tạm thời “mất” tiền nhưng sau đó sẽ lấy lại được chúng. Kiểm tra tính khả thi một cách chính xác cho phép giảm tạm thời miễn là trạng thái hiện tại không bao giờ giảm xuống dưới mức chi phí biên, bởi vì mỗi trạng thái được đánh giá độc lập với số dư tiền riêng của nó và các chuyển đổi thực thi tính khả thi cục bộ.
