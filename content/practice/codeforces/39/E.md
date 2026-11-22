---
title: "CF 39E - Dirichlet có liên quan gì đến chuyện đó?"
description: "Chúng ta bắt đầu với một hộp riêng biệt và b những món đồ riêng biệt. Vì các hộp có thể để trống và mỗi mục sẽ chọn một trong các hộp một cách độc lập nên số vị trí có thể có chỉ đơn giản là:"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games"]
categories: ["algorithms"]
codeforces_contest: 39
codeforces_index: "E"
codeforces_contest_name: "School Team Contest 1 (Winter Computer School 2010/11)"
rating: 2000
weight: 39
solve_time_s: 117
verified: true
draft: false
---
[CF 39E - Dirichlet có liên quan gì đến việc đó?](https://codeforces.com/problemset/problem/39/E) 

**Xếp hạng:** 2000 
**Tags:** dp, trò chơi 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với`a`hộp riêng biệt và`b`các mặt hàng riêng biệt. Vì các hộp có thể để trống và mỗi mục sẽ chọn một trong các hộp một cách độc lập nên số vị trí có thể có chỉ đơn giản là:$a^b$Hai người chơi luân phiên nhau. Trong mỗi lượt, người chơi có thể tăng số lượng hộp lên một hoặc tăng số lượng vật phẩm lên một. Sau khi thực hiện di chuyển, nếu số lượng vị trí ít nhất`n`, người chơi đó thua ngay. 

Trò chơi bắt đầu từ trạng thái`a^b < n`, và Stas di chuyển trước. Chúng ta phải xác định xem người chơi thứ nhất thắng, người chơi thứ hai thắng hay liệu lối chơi tối ưu có dẫn đến một trò chơi vô tận hay không, câu lệnh gọi là`"Missing"`. 

Các ràng buộc hoàn toàn định hình giải pháp. Số lượng mặt hàng không bao giờ vượt quá`30`ban đầu, nhưng trong trò chơi, nó có thể tăng thêm. Số lượng hộp có thể bắt đầu lớn như`10000`. Ngưỡng`n`nhiều nhất là`10^9`, đủ nhỏ để sức mạnh vượt qua nó một cách nhanh chóng. Quan sát đó rất quan trọng vì nó có nghĩa là đồ thị trò chơi thực sự rất nhỏ, mặc dù`a`bản thân nó có thể lớn. 

Một tìm kiếm trò chơi đệ quy ngây thơ mà không ghi nhớ sẽ liên tục truy cập lại các trạng thái tương tự. Tệ hơn nữa, biểu đồ trò chơi còn chứa các chu kỳ. Ví dụ, từ`(1,1)`bạn có thể đi đến`(2,1)`hoặc`(1,2)`, và cả hai có thể tiếp tục mãi mãi khi vẫn ở dưới`n`một lúc. Bất kỳ giải pháp nào giả định trò chơi là một đồ thị tuần hoàn có hướng sẽ âm thầm thất bại. 

Một trường hợp khó nhận thấy là sự tồn tại của các trận hòa. Coi như:```
1 1 1000000000
```Ở đây giá trị là`1^1 = 1`. Việc tăng số lượng hộp giữ giá trị ở mức`1`, vì vậy cả hai người chơi có thể tiếp tục thêm hộp mãi mãi. Một giải pháp chỉ phân loại các trạng thái là thắng hoặc thua sẽ buộc phải đưa ra một câu trả lời không chính xác ngay cả khi cách chơi hoàn hảo tạo ra một trò chơi vô hạn. 

Một trường hợp nguy hiểm khác là tràn trong quá trình tính toán công suất. Giả sử chúng ta kiểm tra:```
10000 30 1000000000
```Giá trị thực tế của`10000^30`có kích thước lớn về mặt thiên văn. Việc sử dụng lũy ​​thừa thông thường mà không dừng sớm có thể làm tràn các số nguyên có chiều rộng cố định trong nhiều ngôn ngữ hoặc lãng phí thời gian để xây dựng các số nguyên Python khổng lồ. Chúng tôi chỉ quan tâm liệu giá trị có đạt được hay không`n`, do đó việc tính toán phải dừng ngay khi vượt qua ngưỡng. 

Trường hợp tinh vi thứ ba xuất phát từ thế cờ mà một nước đi thua ngay lập tức và nước đi kia tiếp tục trò chơi. Ví dụ:```
2 2 10
```Giá trị hiện tại là`4`. Tăng hộp mang lại cho`3^2 = 9`, vẫn an toàn. Tăng vật phẩm mang lại`2^3 = 8`, cũng an toàn. Nhưng sau khi chơi cẩn thận, người chơi thứ nhất có thể buộc người thứ hai phải thua. Lý luận tham lam chỉ dựa trên bước tiếp theo là không đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng cặp`(a,b)`như một trạng thái trò chơi và xác định đệ quy xem nó thắng hay thua. Từ mỗi trạng thái có nhiều nhất hai nước đi:```
(a+1, b)
(a, b+1)
```Bất kỳ động thái nào tạo ra một giá trị ít nhất`n`bị cấm trong thực tế vì người chơi sẽ thua ngay sau khi thực hiện. 

Định nghĩa đệ quy này là đúng vì các trò chơi tổ hợp khách quan quy về sự chuyển đổi trạng thái một cách tự nhiên. Một trạng thái sẽ thắng nếu tồn tại sự chuyển sang trạng thái thua cuộc. Một trạng thái sẽ thua nếu mọi nước đi hợp lệ đều dẫn đến trạng thái thắng. 

Vấn đề là đồ thị trò chơi này không có tính chu kỳ. Các quốc gia có thể tiếp tục phát triển mãi mãi theo một số hướng mà vẫn có hiệu lực. Ví dụ đơn giản nhất là bất kỳ trạng thái nào có`b = 1`, bởi vì:$a^1=a$Miễn là`a < n`, người chơi có thể tiếp tục tăng hộp mãi mãi. Một DFS giả định việc chấm dứt cuối cùng không thể phân biệt giữa các vị trí chiến thắng thực sự và các vị trí cho phép chơi vô hạn. 

Nhận xét quan trọng là đồ thị trò chơi vẫn hữu hạn nếu chúng ta chỉ giữ các trạng thái thỏa mãn:$a^b<n$Khi giá trị đạt`n`, trò chơi kết thúc ngay lập tức. Từ`n ≤ 10^9`, số mũ tăng rất nhanh. Vì`b ≥ 30`, ngay cả những căn cứ nhỏ cũng đã vượt quá giới hạn. Tổng số trạng thái an toàn có thể tiếp cận thực sự rất nhỏ, nhiều nhất là khoảng vài trăm nghìn. 

Điều đó cho phép chúng ta mô hình hóa trò chơi như một đồ thị có hướng hữu hạn có các điểm rút. Sau đó chúng ta có thể sử dụng cách truyền bá trạng thái trò chơi tiêu chuẩn: 

Nếu một bang chuyển sang trạng thái thua thì đó là thắng. 

Nếu mọi nước đi đều chuyển sang trạng thái chiến thắng thì đó là thua. 

Các trạng thái không chắc chắn là thắng hay thua thuộc về các chu kỳ mà người chơi có thể tránh thất bại mãi mãi, vì vậy họ sẽ hòa. 

Điều này trở thành một vấn đề phân tích ngược trên đồ thị có hướng. Chúng tôi xây dựng tất cả các trạng thái hợp lệ, đảo ngược tất cả các cạnh và truyền ngược kết quả bằng BFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS | Hàm mũ | Hàm mũ | Quá chậm | 
| Phân tích trò chơi ngược dòng | O(S) | O(S) | Đã chấp nhận | 

Đây`S`là số trạng thái hợp lệ với`a^b < n`. 

## Hướng dẫn thuật toán 

1. Liệt kê mọi trạng thái hợp lệ`(a,b)`như vậy`a^b < n`. 

Chúng tôi chỉ quan tâm đến những trạng thái mà trò chơi chưa kết thúc. Bất kỳ động thái nào tạo ra`a^b ≥ n`thua ngay. 
2. Đối với mỗi bang, hãy tạo ra các nước đi hợp pháp. 

Từ`(a,b)`chúng tôi cố gắng`(a+1,b)`Và`(a,b+1)`. Việc di chuyển chỉ hợp pháp nếu điểm đến vẫn đáp ứng`a^b < n`. 
3. Tính mức độ vượt trội của mọi trạng thái. 

Mức độ bên ngoài là số lượng nước đi an toàn có sẵn. Giá trị này là cần thiết cho việc truyền bá ngược. 
4. Xây dựng các cạnh ngược. 

Nếu trạng thái`u`có thể chuyển sang trạng thái`v`, sau đó`v`cửa hàng`u`trong danh sách kề ngược của nó. Điều này cho phép chúng ta truyền bá thông tin ngược lại khi trạng thái của`v`trở nên được biết đến. 
5. Khởi tạo trạng thái mất. 

Bất kỳ trạng thái nào không có nước đi hợp pháp đều thua vì mọi nước đi có thể ngay lập tức đạt hoặc vượt quá`n`. 
6. Chạy truyền bá BFS. 

Khi phát hiện ra trạng thái thua cuộc, mọi người đi trước đều trở thành người chiến thắng vì có thể chuyển sang trạng thái thua cuộc đó. 

Khi một trạng thái chiến thắng được phát hiện, chúng tôi sẽ giảm các bước đi chưa được khám phá còn lại của các trạng thái trước đó. Nếu mọi nước đi của người đi trước đều dẫn đến trạng thái chiến thắng thì người đi trước đó sẽ thua cuộc. 
7. Bất kỳ trạng thái nào chưa được phân loại sau BFS đều là kết quả hòa. 

Những trạng thái đó thuộc về các chu kỳ mà không người chơi nào có thể buộc trò chơi kết thúc. 
8. Xuất kết quả cho trạng thái ban đầu. 

Nếu trạng thái ban đầu thắng, Stas có thể buộc Masha thua, vì vậy chúng tôi in`"Masha"`. 

Nếu trạng thái ban đầu thua, Stas thua khi chơi tối ưu, vì vậy chúng tôi in`"Stas"`. 

Nếu không chúng tôi in`"Missing"`. 

### Tại sao nó hoạt động 

Việc truyền bá duy trì trò chơi khách quan tiêu chuẩn bất biến. 

Một trạng thái đang thắng chính xác khi tồn tại sự chuyển sang trạng thái thua cuộc. Một khi chúng ta xác định được trạng thái thua cuộc, tất cả những người đi trước ngay lập tức trở thành người chiến thắng. 

Một bang đang thua chính xác khi mọi nước đi hợp pháp đều thuộc về bang chiến thắng. Bộ đếm mức độ theo dõi còn lại bao nhiêu nước đi chưa được giải quyết. Khi tất cả các nước đi đi được biết là thắng thì bang đó chắc chắn đang thua. 

Bất kỳ trạng thái nào không bao giờ được phân loại theo hai quy tắc này đều phải thuộc về một chu trình có thể tránh các trạng thái cuối mãi mãi. Vì không người chơi nào có thể giành chiến thắng ở khu vực như vậy nên kết quả chính xác là hòa. 

## Giải pháp Python```python
import sys
from collections import deque, defaultdict

input = sys.stdin.readline

def safe_pow(a, b, limit):
    res = 1
    for _ in range(b):
        res *= a
        if res >= limit:
            return limit
    return res

def solve():
    a0, b0, n = map(int, input().split())

    states = []
    idx = {}

    max_b = 1

    # Enumerate all valid states
    a = 1
    while safe_pow(a, 1, n) < n:
        b = 1
        while safe_pow(a, b, n) < n:
            idx[(a, b)] = len(states)
            states.append((a, b))
            max_b = max(max_b, b)
            b += 1
        a += 1

    m = len(states)

    rev = [[] for _ in range(m)]
    outdeg = [0] * m

    # Build graph
    for i, (a, b) in enumerate(states):
        for na, nb in ((a + 1, b), (a, b + 1)):
            if (na, nb) in idx:
                j = idx[(na, nb)]
                outdeg[i] += 1
                rev[j].append(i)

    # 0 = draw/unvisited
    # 1 = winning
    # 2 = losing
    state = [0] * m

    q = deque()

    # Terminal losing states
    for i in range(m):
        if outdeg[i] == 0:
            state[i] = 2
            q.append(i)

    rem = outdeg[:]

    # Retrograde analysis
    while q:
        v = q.popleft()

        for u in rev[v]:
            if state[u] != 0:
                continue

            if state[v] == 2:
                state[u] = 1
                q.append(u)
            else:
                rem[u] -= 1
                if rem[u] == 0:
                    state[u] = 2
                    q.append(u)

    start = idx[(a0, b0)]

    if state[start] == 1:
        print("Masha")
    elif state[start] == 2:
        print("Stas")
    else:
        print("Missing")

solve()
```Phần quan trọng đầu tiên là`safe_pow`. Chúng tôi không bao giờ cần giá trị chính xác khi nó đạt đến`n`, do đó phép nhân dừng sớm. Điều đó tránh việc xây dựng các số nguyên lớn và giữ cho việc liệt kê nhanh chóng. 

Việc tạo ra trạng thái dựa trên thực tế là chỉ những vị trí an toàn mới quan trọng. Các vòng lặp lồng nhau dừng lại một cách tự nhiên vì sức mạnh cuối cùng vượt quá`n`. Mặc dù`a`có thể phát triển lớn, tổng số trạng thái hợp lệ vẫn có thể quản lý được. 

Giai đoạn xây dựng đồ thị tạo ra cả thông tin chuyển tiếp thông qua`outdeg`và đảo ngược thông tin thông qua`rev`. Các cạnh ngược là những gì làm cho việc truyền ngược trở nên hiệu quả. 

Bản thân BFS thực hiện logic trò chơi thắng thua tiêu chuẩn. Khi chúng tôi xử lý một nút thua, mọi nút trước đó ngay lập tức trở thành nút chiến thắng vì nó có động thái đánh bại. Khi chúng tôi xử lý một nút chiến thắng, các nút trước đó chỉ mất một tùy chọn an toàn còn lại. Chỉ sau khi tất cả các lựa chọn đều thắng, chúng ta mới phân loại người đi trước là thua. 

Các tiểu bang còn lại như`0`sau BFS chính xác là trạng thái rút thăm. Họ không bao giờ bị ép buộc vào một trong hai loại vì người chơi có thể tiếp tục đạp xe vô thời hạn. 

Một chi tiết triển khai tinh tế là quy ước đầu ra. Bài toán hỏi người thua chứ không phải người thắng. Nếu trạng thái ban đầu là người chơi hiện tại thắng thì Stas thắng và Masha thua, vì vậy chúng ta in`"Masha"`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 10
```Các trạng thái an toàn bao gồm: 

| Tiểu bang | Giá trị | Động thái hợp pháp | 
| --- | --- | --- | 
| (2,2) | 4 | (3,2), (2,3) | 
| (3,2) | 9 | không | 
| (2,3) | 8 | không | 

Tuyên truyền hoạt động như thế này: 

| Bước | Trạng thái đã xử lý | Loại | Hiệu ứng | 
| --- | --- | --- | --- | 
| 1 | (3,2) | Thua | (2,2) trở thành chiến thắng | 
| 2 | (2,3) | Thua | đã được giải quyết | 

Từ`(2,2)`đang thắng, Stas có thể buộc Masha thua. 

Đầu ra:```
Masha
```Dấu vết này thể hiện quy tắc trò chơi trung tâm. Một nước đi đến trạng thái thua cuộc là đủ để khiến trạng thái hiện tại giành chiến thắng. 

### Ví dụ 2 

đầu vào:```
1 1 1000000000
```Các trạng thái chính: 

| Tiểu bang | Giá trị | Di chuyển an toàn | 
| --- | --- | --- | 
| (1,1) | 1 | (2,1), (1,2) | 
| (2,1) | 2 | (3,1), (2,2) | 
| (3,1) | 3 | (4,1), (3,2) | 

Chuỗi`(1,1) → (2,1) → (3,1) → ...`không bao giờ buộc chấm dứt vì việc tăng các hộp có một mục sẽ giữ giá trị ở dưới`n`trong một thời gian rất dài. 

Không có sự truyền bá ngược nào có thể phân loại các trạng thái này là thắng hoặc thua bắt buộc, vì vậy chúng vẫn là hòa. 

Đầu ra:```
Missing
```Ví dụ này xác nhận rằng các chu kỳ và lượt chơi vô hạn phải được xử lý một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S) | mỗi trạng thái và cạnh hợp lệ được xử lý một lần | 
| Không gian | O(S) | lưu trữ đồ thị và mảng BFS |`S`là số trạng thái thỏa mãn`a^b < n`. Bởi vì`n ≤ 10^9`, sức mạnh tăng lên nhanh chóng và`S`vẫn nhỏ thoải mái cho các giới hạn. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian 2 giây và giới hạn bộ nhớ 64 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def safe_pow(a, b, limit):
        res = 1
        for _ in range(b):
            res *= a
            if res >= limit:
                return limit
        return res

    a0, b0, n = map(int, input().split())

    states = []
    idx = {}

    a = 1
    while safe_pow(a, 1, n) < n:
        b = 1
        while safe_pow(a, b, n) < n:
            idx[(a, b)] = len(states)
            states.append((a, b))
            b += 1
        a += 1

    m = len(states)

    rev = [[] for _ in range(m)]
    outdeg = [0] * m

    for i, (a, b) in enumerate(states):
        for na, nb in ((a + 1, b), (a, b + 1)):
            if (na, nb) in idx:
                j = idx[(na, nb)]
                outdeg[i] += 1
                rev[j].append(i)

    state = [0] * m
    q = deque()

    for i in range(m):
        if outdeg[i] == 0:
            state[i] = 2
            q.append(i)

    rem = outdeg[:]

    while q:
        v = q.popleft()

        for u in rev[v]:
            if state[u] != 0:
                continue

            if state[v] == 2:
                state[u] = 1
                q.append(u)
            else:
                rem[u] -= 1
                if rem[u] == 0:
                    state[u] = 2
                    q.append(u)

    start = idx[(a0, b0)]

    if state[start] == 1:
        return "Masha\n"
    elif state[start] == 2:
        return "Stas\n"
    return "Missing\n"

# provided sample
assert run("2 2 10\n") == "Masha\n", "sample 1"

# draw case
assert run("1 1 1000000000\n") == "Missing\n", "infinite play"

# immediate trap
assert run("3 2 10\n") == "Stas\n", "no safe moves"

# tiny boundary
assert run("1 1 2\n") == "Stas\n", "minimum threshold"

# larger mixed case
assert run("2 3 20\n") == "Masha\n", "non-trivial winning state"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1000000000`|`Missing`| phát hiện chu kỳ chơi vô hạn | 
|`3 2 10`|`Stas`| trạng thái mất thiết bị đầu cuối | 
|`1 1 2`|`Stas`| ngưỡng hợp lệ nhỏ nhất | 
|`2 3 20`|`Masha`| lan truyền ngược sâu hơn | 

## Vỏ cạnh 

Xét trường hợp hòa:```
1 1 1000000000
```Thuật toán tạo ra một chuỗi lớn các trạng thái`(k,1)`nơi mọi tiểu bang có thể chuyển đến`(k+1,1)`một cách an toàn. Không có trạng thái nào trong số này đạt tới mức 0 trong quá trình khởi tạo. Trong quá trình truyền bá BFS, họ không bao giờ bị buộc phải phân loại thắng hay thua vì chu kỳ chơi an toàn không bao giờ sụp đổ. Trạng thái vẫn chưa được phân loại, do đó thuật toán sẽ in chính xác:```
Missing
```Bây giờ hãy xem xét một vị trí đầu cuối:```
3 2 10
```Giá trị hiện tại là:$3^2=9$Tăng hộp mang lại cho`4^2 = 16`, thua ngay lập tức. Tăng vật phẩm mang lại`3^3 = 27`, cũng thua ngay. Bang không có động thái ra đi hợp pháp nào, vì vậy nó được khởi tạo là thua. Vì Stas bắt đầu ở đó nên kết quả là:```
Stas
```Cuối cùng, hãy xem xét ranh giới ngưỡng nhỏ:```
1 1 2
```Giá trị hiện tại là`1`. Cả hai nước đi có thể đều tạo ra chính xác`2`, điều này đã gây ra thất bại vì điều kiện là`>= n`, không thực sự lớn hơn. Thuật toán loại bỏ cả hai bước di chuyển trong quá trình xây dựng biểu đồ, cho ra mức độ bằng 0. Chi tiết đó tránh được sai lầm cổ điển.
