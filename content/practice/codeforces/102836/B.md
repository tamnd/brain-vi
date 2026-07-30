---
title: "CF 102836B - \u041f\u0435\u0440\u0435\u043b\u0438\u0432\u0430\u043d\u0438\u0435 \u0436\u0438\u0436\u0438"
description: "Chúng tôi có ba xe tăng với công suất cố định. Mỗi bể hiện chứa một lượng chất lỏng và chúng tôi muốn đạt đến tình huống cuối cùng trong đó ba lượng khớp với các giá trị được yêu cầu, nhưng các giá trị được yêu cầu có thể thuộc về các bể khác nhau vì thứ tự của bể không quan trọng."
date: "2026-07-26T14:55:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102836
codeforces_index: "B"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434, \u0421\u0435\u0437\u043e\u043d 2020-21, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102836
solve_time_s: 50
verified: true
draft: false
---

[CF 102836B - \u041f\u0435\u0440\u0435\u043b\u0438\u0432\u0430\u043d\u0438\u0435 \u0436\u0438\u0436\u0438](https://codeforces.com/problemset/problem/102836/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có ba xe tăng với công suất cố định. Mỗi bể hiện chứa một lượng chất lỏng và chúng tôi muốn đạt đến tình huống cuối cùng trong đó ba lượng khớp với các giá trị được yêu cầu, nhưng các giá trị được yêu cầu có thể thuộc về các bể khác nhau vì thứ tự của bể không quan trọng. 

Một nước đi bao gồm việc chọn bể nguồn và bể đích. Chất lỏng được đổ từ nguồn vào đích cho đến khi một trong hai sự kiện xảy ra: nguồn trở nên trống hoặc đích đến trở nên đầy hoàn toàn. Nhiệm vụ là tìm ra số lần đổ tối thiểu cần thiết hoặc báo cáo rằng không thể đạt được cấu hình mục tiêu. 

Dung lượng và số lượng có thể lớn như$10^6$, vì vậy việc mô phỏng mọi sự kết hợp số lượng có thể là không thể. Một tìm kiếm trực tiếp trên tất cả các bộ ba sẽ có tới$(n_1+1)(n_2+1)(n_3+1)$tiểu bang. Với công suất gần mức tối đa, con số này là khoảng$10^{18}$trạng thái vượt xa bất kỳ bộ nhớ hợp lý hoặc giới hạn thời gian nào. 

Hạn chế chính là chỉ có ba xe tăng. Mặc dù không gian trạng thái lý thuyết là rất lớn nhưng các trạng thái có thể tiếp cận được có cấu trúc nhỏ hơn nhiều. Sau bất kỳ lần đổ nào, ít nhất một trong hai thùng liên quan sẽ hết hoặc đầy. Điều này có nghĩa là sau lần di chuyển đầu tiên, mọi trạng thái có thể truy cập đều nằm trên một trong sáu bề mặt "viền" trong đó một tọa độ được cố định là 0 hoặc dung lượng của nó. Mỗi bề mặt này chỉ chứa một số lượng tuyến tính các trạng thái hữu ích vì tổng lượng chất lỏng không đổi. 

Việc thực hiện bất cẩn vẫn có thể thất bại trong một số trường hợp. Nếu trạng thái ban đầu đã là câu trả lời thì kết quả phải là 0 nước đi.```
Input:
5 7 9
1 3 4
4 1 3
```Đầu ra là:```
0
```BFS chỉ kiểm tra các trạng thái mới được tạo sau khi thực hiện di chuyển sẽ bỏ lỡ điều này. 

Một cái bẫy khác là thứ tự xe tăng mục tiêu không phù hợp. Ví dụ:```
Input:
10 5 3
7 1 2
1 3 6
```Tổng số tiền là 10, vậy nên bản thân mục tiêu là không thể vì tổng số tiền được yêu cầu là 10 nhưng một lượng xe tăng được yêu cầu lại vượt quá mọi kết quả phù hợp có thể xảy ra? Việc triển khai đúng phải so sánh với tất cả các hoán vị của giá trị đích thay vì chỉ kiểm tra`(b1, b2, b3)`theo thứ tự ban đầu. 

Trường hợp cạnh thứ ba là khi việc đổ không thay đổi gì vì đích đã đầy hoặc nguồn đã trống.```
Input:
5 5 5
5 0 0
5 0 0
```Câu trả lời là:```
0
```Việc tạo các chuyển đổi này không chính xác có thể thêm trạng thái giả hoặc lặp lại mãi mãi. 

## Phương pháp tiếp cận 

Nỗ lực đầu tiên tự nhiên là tìm kiếm đầu tiên trên diện rộng trên tất cả các chất làm đầy thùng chứa có thể có. Một trạng thái là một bộ ba`(x, y, z)`mô tả số lượng hiện tại trong mỗi bể. Từ một trạng thái, chúng tôi thử sáu hướng đổ có thể. BFS đúng vì mỗi lần chuyển đổi đều tốn chính xác một lần di chuyển, vì vậy, lần đầu tiên chúng tôi ghé thăm một trạng thái, chúng tôi đã tìm thấy chuỗi lượt đổ ngắn nhất để đạt đến trạng thái đó. 

Vấn đề là số lượng trạng thái. Với dung tích một triệu lít, việc lưu trữ mọi bộ ba có thể là không thể. Tìm kiếm brute-force coi nhiều trạng thái không thể hoặc không cần thiết đều quan trọng như nhau. 

Quan sát hữu ích đến từ cơ chế đổ. Giả sử chúng ta đổ từ bể A sang bể B. Hoạt động chỉ dừng khi A đạt 0 hoặc B đạt đến dung tích. Do đó, mỗi trạng thái sau khi di chuyển đều có ít nhất một xe tăng ở giá trị biên. Không gian tìm kiếm không phải là một hộp ba chiều đầy đủ. Nó là sự kết hợp của một số bề mặt hai chiều và vì tổng lượng chất lỏng không bao giờ thay đổi nên mỗi bề mặt chỉ chứa một số trạng thái tuyến tính. 

Cách tiếp cận bạo lực hoạt động vì nó mô hình hóa quy trình một cách chính xác, nhưng không thành công vì nó bỏ qua cấu trúc được tạo bởi quy tắc đổ. Bằng cách sử dụng BFS cùng với thuộc tính ranh giới, chúng tôi chỉ truy cập các trạng thái có thể truy cập và số lượng trạng thái được truy cập vẫn đủ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n1 · n2 · n3) | O(n1 · n2 · n3) | Quá chậm | 
| BFS tối ưu ở các trạng thái ranh giới có thể tiếp cận | O(n1 + n2 + n3) | O(n1 + n2 + n3) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc dung lượng, số tiền ban đầu và số tiền mục tiêu. Vì thứ tự mục tiêu không quan trọng nên hãy lưu trữ ngầm ba cách sắp xếp mục tiêu có thể có bằng cách kiểm tra các hoán vị sau đó. 
2. Trước khi bắt đầu BFS, hãy kiểm tra xem trạng thái ban đầu đã khớp với một trong các hoán vị đích hay chưa. Nếu đúng như vậy, câu trả lời là 0 vì không cần đổ. 
3. Bắt đầu BFS từ trạng thái ban đầu. Lưu trữ mỗi trạng thái được truy cập dưới dạng gấp ba số lượng. Hàng đợi luôn chứa các trạng thái theo thứ tự không giảm dần về số lần di chuyển cần thiết để đến được chúng. 
4. Đối với mọi trạng thái, hãy thử đổ từ mỗi bể vào hai bể còn lại. Để đổ từ bể`i`để xe tăng`j`, tính lượng được chuyển càng nhỏ hơn lượng chất lỏng có sẵn trong`i`và không gian trống trong`j`. 
5. Chèn mọi trạng thái kết quả mới vào hàng đợi BFS. Trạng thái kết quả đủ để mô tả tương lai vì quá trình này chỉ phụ thuộc vào số lượng hiện tại chứ không phụ thuộc vào lịch sử. 
6. Khi một trạng thái khớp với bất kỳ hoán vị nào của số lượng mục tiêu, hãy trả về khoảng cách BFS của nó. Nếu hàng đợi trống, không có chuỗi đổ nào có thể đến được mục tiêu, vì vậy hãy quay lại`-1`. 

Lý do việc tìm kiếm này vẫn hiệu quả là do tính bất biến được tạo bởi quy tắc chuyển tiếp. Mọi trạng thái ngoại trừ trạng thái ban đầu đều có ít nhất một bể trống hoặc đầy. BFS không bao giờ cần khám phá các trạng thái bên ngoài ranh giới có thể tiếp cận này vì không có động thái hợp lệ nào có thể tạo ra chúng. 

## Tại sao nó hoạt động 

Mỗi chuỗi đổ có thể tương ứng với một đường dẫn trong biểu đồ trạng thái. BFS khám phá biểu đồ này theo thứ tự độ dài đường dẫn, do đó, lần đầu tiên nó đạt đến trạng thái mục tiêu, số lần di chuyển là tối thiểu. 

Mối quan tâm duy nhất là liệu không gian trạng thái bị thu hẹp có làm mất đi các câu trả lời hợp lệ hay không. Không phải vậy, vì thuật toán không loại bỏ các chuyển đổi một cách giả tạo. Nó tạo ra mọi khoản đổ hợp pháp từ mọi trạng thái được phát hiện. Thuộc tính ranh giới chỉ giải thích tại sao số lượng trạng thái được phát hiện vẫn có thể quản lý được. Vì mọi đường dẫn có thể vẫn được biểu thị trong biểu đồ BFS nên câu trả lời vẫn đúng. 

## Giải pháp Python```python
import sys
from collections import deque
from itertools import permutations

input = sys.stdin.readline

def solve():
    n = list(map(int, input().split()))
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    targets = set(permutations(b))

    start = tuple(a)
    if start in targets:
        print(0)
        return

    q = deque([start])
    dist = {start: 0}

    while q:
        state = q.popleft()
        d = dist[state]

        for i in range(3):
            if state[i] == 0:
                continue
            for j in range(3):
                if i == j or state[j] == n[j]:
                    continue

                nxt = list(state)
                amount = min(nxt[i], n[j] - nxt[j])
                nxt[i] -= amount
                nxt[j] += amount
                nxt = tuple(nxt)

                if nxt in dist:
                    continue

                if nxt in targets:
                    print(d + 1)
                    return

                dist[nxt] = d + 1
                q.append(nxt)

    print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng tập hợp tất cả các trạng thái mục tiêu hợp lệ. Điều này xử lý thực tế là thứ tự bể vật lý không quan trọng. Số hoán vị có thể có chỉ là sáu, nên việc kiểm tra này là thời gian không đổi. 

BFS sử dụng một từ điển thay vì một mảng khoảng cách riêng biệt vì các trạng thái thưa thớt. Từ điển lưu trữ cả trạng thái đã được truy cập hay chưa và cần phải di chuyển bao nhiêu lần để đến được trạng thái đó. 

Tính toán chuyển tiếp là chi tiết triển khai quan trọng nhất. Lượng di chuyển bị giới hạn bởi hai giá trị: chất lỏng có sẵn trong nguồn và dung lượng còn lại của điểm đến. Việc sử dụng giá trị tối thiểu của hai giá trị này sẽ mô hình chính xác quy tắc dừng của bài toán. 

Mã sẽ kiểm tra mục tiêu ngay sau khi tạo trạng thái mới. Trạng thái ban đầu được kiểm tra riêng vì nó không bao giờ đi vào vòng chuyển tiếp. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
10 5 3
7 1 2
3 3 4
```Tổng số tiền là 10 và mục tiêu có thể được sắp xếp thành`(3, 3, 4)`. 

| Bước | Hiện trạng | Di chuyển | Tiểu bang mới | Khoảng cách | 
| --- | --- | --- | --- | --- | 
| 0 | (7, 1, 2) | Đổ bể 1 sang bể 2 | (3, 5, 2) | 1 | 
| 1 | (3, 5, 2) | Đổ bể 2 sang bể 3 | (3, 4, 3) | 2 | 

Nhà nước`(3, 4, 3)`là một hoán vị của mục tiêu, vì vậy câu trả lời là 2. Ví dụ này chứng tỏ rằng thuật toán không yêu cầu các giá trị mục tiêu xuất hiện theo thứ tự xe tăng ban đầu. 

Một ví dụ khác:```
8 5 4
8 0 0
4 4 0
```| Bước | Hiện trạng | Di chuyển | Tiểu bang mới | Khoảng cách | 
| --- | --- | --- | --- | --- | 
| 0 | (8, 0, 0) | Đổ bể 1 sang bể 2 | (3, 5, 0) | 1 | 
| 1 | (3, 5, 0) | Đổ bể 2 sang bể 3 | (3, 1, 4) | 2 | 

Trạng thái thứ hai đã chứa số lượng mục tiêu dưới dạng hoán vị, vì vậy số lượng thao tác tối thiểu là 2. 

Những dấu vết này cho thấy sự bất biến chính: sau mỗi lần đổ, ít nhất một thùng rỗng hoặc đầy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n1 + n2 + n3) | Chỉ các trạng thái ranh giới có thể tiếp cận được với tổng lượng chất lỏng cố định mới được khám phá và mỗi trạng thái có sáu lần đổ có thể. | 
| Không gian | O(n1 + n2 + n3) | Hàng đợi BFS và từ điển đã truy cập chỉ chứa các trạng thái ranh giới có thể truy cập được. | 

Dung lượng lớn nhưng thuật toán không bao giờ phân bổ mảng ba chiều. Việc giảm trạng thái có thể tiếp cận là điều làm cho giải pháp phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque
from itertools import permutations

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline

    read = input()
    n = list(map(int, read().split()))
    a = list(map(int, input()().split()))
    b = list(map(int, input()().split()))

    targets = set(permutations(b))

    start = tuple(a)
    if start in targets:
        ans = "0"
    else:
        q = deque([start])
        dist = {start: 0}
        ans = "-1"

        while q:
            state = q.popleft()
            d = dist[state]

            for i in range(3):
                for j in range(3):
                    if i == j:
                        continue
                    if state[i] == 0 or state[j] == n[j]:
                        continue

                    nxt = list(state)
                    move = min(nxt[i], n[j] - nxt[j])
                    nxt[i] -= move
                    nxt[j] += move
                    nxt = tuple(nxt)

                    if nxt in dist:
                        continue
                    if nxt in targets:
                        ans = str(d + 1)
                        q.clear()
                        break

                    dist[nxt] = d + 1
                    q.append(nxt)

    sys.stdin = old_stdin
    return ans

assert solve("""10 5 3
7 1 2
3 3 4
""") == "2"

assert solve("""10 5 3
0 5 3
0 5 3
""") == "0"

assert solve("""5 5 5
5 0 0
5 0 0
""") == "0"

assert solve("""1 1 1
1 0 0
0 1 0
""") == "1"

assert solve("""2 4 6
2 0 0
1 1 4
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 5 3 kèm cấu hình mẫu | 2 | BFS cơ bản và xử lý hoán vị đích | 
| Cấu hình đã được giải quyết | 0 | Phát hiện trạng thái ban đầu | 
| Bể đầy và bể rỗng | 0 | Xử lý trạng thái biên | 
| Công suất nhỏ | 1 | Độ chính xác chuyển tiếp tối thiểu | 
| Tổng phân phối không thể | -1 | Phát hiện mục tiêu không thể tiếp cận | 

## Vỏ cạnh 

Khi trạng thái ban đầu đã phù hợp với mục tiêu, BFS phải dừng trước khi thực hiện bất kỳ động thái nào. Ví dụ:```
5 5 5
5 0 0
5 0 0
```Bộ dữ liệu bắt đầu đã là một trong những hoán vị đích, do đó thuật toán trả về 0 ngay lập tức. 

Khi các giá trị mục tiêu có thể được đặt vào các bể khác nhau, chỉ kiểm tra thứ tự ban đầu sẽ đưa ra câu trả lời sai. Ví dụ:```
10 5 3
7 1 2
4 3 3
```Mục tiêu có thể được thể hiện bằng`(3, 3, 4)`, do đó thuật toán chấp nhận trạng thái có ba giá trị này ở bất kỳ vị trí nào. 

Khi thùng đã trống hoặc đầy, một số lần đổ sẽ không hợp lệ. Ví dụ:```
5 5 5
5 0 0
0 5 0
```Cố gắng đổ từ thùng thứ hai trống rỗng hoặc vào thùng thứ nhất đã đầy sẽ tạo ra những động tác giả. Mã chuyển tiếp từ chối những trường hợp đó trước khi tính toán trạng thái tiếp theo. 

BFS xử lý tất cả các trường hợp này vì nó coi trạng thái là mô tả đầy đủ của hệ thống, chỉ tạo ra các lần rót hợp lệ về mặt vật lý và kiểm tra tất cả các sắp xếp cuối cùng có thể có.
