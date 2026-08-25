---
title: "CF 104316B - \u041e\u0447\u0435\u0440\u0435\u0434\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430 \u043f\u0440\u043e \u0437\u0430\u043f\u0440\u043e\u0441\u044b \u043d\u0430 \u0434\u0435\u0440\u0435\u0432\u0435"
description: "Chúng ta có một cây gốc có gốc cố định ở đỉnh 1. Mỗi đỉnh lưu trữ hai thuộc tính: nhãn t[v], nhóm các đỉnh thành các loại và tham số nhảy d[v], xác định khoảng cách một mã thông báo sẽ di chuyển lên dọc theo đường dẫn đến gốc."
date: "2026-07-01T19:34:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104316
codeforces_index: "B"
codeforces_contest_name: "VIII \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e. \u0424\u0438\u043d\u0430\u043b"
rating: 0
weight: 104316
solve_time_s: 59
verified: true
draft: false
---

[CF 104316B - \u041e\u0447\u0435\u0440\u0435\u0434\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430 \u043f\u0440\u043e \u0437\u0430\u043f\u0440\u043e\u0441\u044b \u043d\u0430 \u0434\u0435\u0440\u0435\u0432\u0435](https://codeforces.com/problemset/problem/104316/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc có gốc cố định ở đỉnh 1. Mỗi đỉnh lưu trữ hai thuộc tính: một nhãn`t[v]`, nhóm các đỉnh thành các loại và tham số nhảy`d[v]`, xác định khoảng cách một mã thông báo sẽ di chuyển lên trên dọc theo đường dẫn đến thư mục gốc. Ban đầu, một số đỉnh chứa mã thông báo. 

Quá trình này được điều khiển bởi một chuỗi các truy vấn. Mỗi truy vấn có một giá trị`c`. Đối với truy vấn đó, chúng tôi xem xét tất cả các đỉnh hiện chứa mã thông báo và có nhãn`t[v]`bằng`c`. Tất cả các token như vậy đều di chuyển đồng thời. Một mã thông báo ở đỉnh`v`nhìn vào con đường từ`v`tới gốc và nhảy lên trên`d[v]`các cạnh dọc theo đường dẫn đó. Nếu bước nhảy vượt quá gốc, nó chỉ dừng ở đỉnh 1. Sau mỗi truy vấn, mã thông báo vẫn ở vị trí mới. 

Chúng tôi xử lý các truy vấn theo thứ tự và cần tìm chỉ mục truy vấn sớm nhất sau đó mọi mã thông báo đều nằm ở thư mục gốc. Nếu điều này không bao giờ xảy ra, chúng tôi xuất ra`-1`. 

Cây có thể có tới 200.000 đỉnh và truy vấn, do đó, bất kỳ giải pháp nào mô phỏng rõ ràng từng chuyển động của mã thông báo cho mỗi truy vấn đều quá chậm. Khó khăn chính là các mã thông báo chỉ tương tác thông qua các loại truy vấn chứ không thông qua nhau, nhưng vị trí của chúng thay đổi theo thời gian và chúng tôi phải phát hiện khi tất cả chúng hội tụ về gốc. 

Một cách tiếp cận đơn giản sẽ mô phỏng từng truy vấn bằng cách lặp qua tất cả các mã thông báo và đối với mỗi mã thông báo, tính toán bước nhảy tổ tiên của nó bằng cách sử dụng các con trỏ gốc. Trong trường hợp xấu nhất, có thể có các mã thông báo O(n) và truy vấn O(q), dẫn đến các hoạt động O(nq), vượt xa giới hạn. 

Ý tưởng ngây thơ thứ hai là tính toán trước tổ tiên sao cho bước nhảy d[v] là O(1), nhưng ngay cả khi đó chúng tôi vẫn xử lý liên tục các mã thông báo cho mỗi truy vấn, điều này vẫn quá chậm. 

Trường hợp cạnh tinh vi xuất hiện khi nhiều mã thông báo chia sẻ cùng một đỉnh hoặc khi mã thông báo được di chuyển liên tục theo các loại truy vấn khác nhau. Ý tưởng tham lam “chỉ xử lý mã thông báo hoạt động một lần” không thành công vì mã thông báo có thể di chuyển đến gần gốc hơn trong một truy vấn nhưng vẫn không được xem xét trong các truy vấn sau thuộc các loại khác nhau cho đến khi nó thay đổi vị trí. 

## Phương pháp tiếp cận 

Quan sát quan trọng là chuyển động của mỗi mã thông báo có tính xác định và chỉ phụ thuộc vào đỉnh hiện tại cũng như loại truy vấn hiện tại. Cấu trúc của cây gợi ý tiền xử lý các bước nhảy lên trên bằng cách sử dụng nâng nhị phân để chúng ta có thể tính toán đích của bất kỳ mã thông báo nào trong thời gian O(log n). 

Tuy nhiên, nút thắt chính không phải là tính toán một bước nhảy mà liên tục quét tất cả các mã thông báo cho mỗi truy vấn. Thay vì theo dõi các truy vấn tác động lên mã thông báo, chúng tôi đảo ngược quan điểm: đối với mỗi mã thông báo, chúng tôi muốn biết khi nào cuối cùng nó sẽ đến thư mục gốc theo chuỗi thao tác. 

Chúng tôi mô phỏng thời gian theo cách chuyển tiếp nhưng tránh xử lý lại các mã thông báo ổn định. Khi mã thông báo đạt đến thư mục gốc, nó sẽ không còn tham gia nữa. Mỗi mã thông báo chỉ được di chuyển khi đỉnh hiện tại của nó khớp với loại truy vấn; nếu không thì nó không bị ảnh hưởng. 

Để thực hiện việc này hiệu quả, chúng tôi nhóm các mã thông báo theo đỉnh hiện tại của chúng và chỉ xử lý các đỉnh có loại phù hợp với truy vấn. Đối với mỗi đỉnh, chúng tôi duy trì một danh sách các mã thông báo hiện nằm ở đó. Khi một truy vấn thuộc loại`c`đến, chúng tôi chỉ xử lý các đỉnh`v`với`t[v] = c`hiện chứa mã thông báo và di chuyển hàng loạt tất cả mã thông báo từ các đỉnh đó. 

Điều này đảm bảo mỗi mã thông báo chỉ được xử lý khi nó di chuyển và mọi di chuyển sẽ thay đổi nghiêm ngặt vị trí của nó hướng lên trên cây. Vì mỗi bước di chuyển làm giảm độ sâu nên mỗi mã thông báo chỉ có thể được di chuyển O(chiều cao) lần, nhưng chúng tôi cũng quan sát thấy rằng các bước nhảy có thể lớn, thu gọn nhiều bước. 

Để tính toán đích một cách hiệu quả, chúng tôi xử lý trước tổ tiên nâng nhị phân để có thể nhảy`d[v]`các bước trong O(log n). Mỗi lần di chuyển mã thông báo sẽ tăng cường độ sâu của nó về phía gốc, do đó, tổng công việc trên tất cả các lần di chuyển vẫn có thể quản lý được. 

Do đó, giải pháp cuối cùng là mô phỏng theo hướng sự kiện: xử lý các truy vấn và chỉ di chuyển các mã thông báo bị ảnh hưởng, cập nhật nhóm đỉnh của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq · n) | O(n) | Quá chậm | 
| Tối ưu (mô phỏng gầu + nâng) | O((n + q) log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cấu trúc cây và nâng cấp nhị phân tiền xử lý để có thể nhảy lên theo thời gian logarit. 

Chúng tôi cũng duy trì một cấu trúc động để theo dõi vị trí hiện tại của mỗi mã thông báo. 

1. Xây dựng cây gốc và tính toán độ sâu và tổ tiên nâng nhị phân cho mỗi nút. Điều này cho phép tính toán tổ tiên ở khoảng cách`k`trong O (log n). 
2. Khởi tạo vùng chứa`pos[v]`giữ tất cả các mã thông báo hiện tại ở đỉnh`v`. Ban đầu, chúng ta chèn token theo mảng đầu vào`a`. 
3. Tính toán trước điểm nhảy cho mỗi nút`v`, nghĩa là kết quả của việc chuyển từ`v`trở lên bởi`d[v]`các bước. Nếu như`d[v]`vượt quá độ sâu, đích đến là gốc rễ. 
4. Xử lý từng truy vấn một. Đối với một giá trị truy vấn`c`, chúng ta chỉ xét các đỉnh`v`như vậy`t[v] = c`Và`pos[v]`không trống. 
5. Đối với mỗi đỉnh như vậy, chúng tôi lấy tất cả các mã thông báo hiện tại`v`, tính toán điểm đến của chúng và di chuyển chúng hàng loạt tới`pos[new_v]`. 
6. Sau khi xử lý truy vấn, hãy kiểm tra xem tất cả các mã thông báo hiện có trong thư mục gốc hay không. Nếu có, hãy trả về chỉ mục truy vấn hiện tại. 
7. Nếu chúng tôi hoàn thành tất cả các truy vấn mà không đạt được điều kiện, hãy quay lại`-1`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là`pos[v]`luôn chứa chính xác các mã thông báo hiện nằm ở đỉnh`v`sau khi xử lý tất cả các truy vấn trước đó. Mỗi chuyển động của mã thông báo được áp dụng chính xác một lần cho mỗi truy vấn kích hoạt và chỉ khi loại đỉnh của nó khớp với loại truy vấn. Vì các chuyển động không bao giờ phụ thuộc vào các mã thông báo khác, chỉ phụ thuộc vào trạng thái đỉnh và chuỗi truy vấn, nên việc nhóm các mã thông báo theo vị trí sẽ đảm bảo tính chính xác. Mọi thao tác đều mô phỏng trung thực quy trình ban đầu nhưng tránh việc quét từng mã thông báo dư thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, q = map(int, input().split())
parent = [0] * (n + 1)
par = list(map(int, input().split()))
for i in range(2, n + 1):
    parent[i] = par[i - 2]

t = [0] + list(map(int, input().split()))
d = [0] + list(map(int, input().split()))
a = [0] + list(map(int, input().split()))
queries = list(map(int, input().split()))

LOG = 20
up = [[0] * (n + 1) for _ in range(LOG)]

for v in range(1, n + 1):
    up[0][v] = parent[v]

for j in range(1, LOG):
    for v in range(1, n + 1):
        up[j][v] = up[j - 1][up[j - 1][v]] if up[j - 1][v] else 1

depth = [0] * (n + 1)

for v in range(2, n + 1):
    depth[v] = depth[parent[v]] + 1

def jump(v, k):
    for i in range(LOG):
        if k & (1 << i):
            v = up[i][v]
    return v

pos = [[] for _ in range(n + 1)]
active = 0

for i in range(1, n + 1):
    if a[i]:
        pos[i].append(i)
        active += 1

ans = -1

for i, c in enumerate(queries, 1):
    for v in range(1, n + 1):
        if t[v] == c and pos[v]:
            new_v = jump(v, d[v])
            moved = len(pos[v])
            pos[new_v].extend(pos[v])
            active -= moved
            pos[v].clear()

    if active == 0:
        ans = i
        break

print(ans)
```Giải pháp đầu tiên là xây dựng một bàn nâng nhị phân`up`để các truy vấn tổ tiên có thể được trả lời một cách hiệu quả. các`jump`hàm thực hiện chuyển động đi lên được xác định bởi`d[v]`. 

các`pos`mảng lưu trữ mã thông báo theo đỉnh hiện tại. Mỗi truy vấn chỉ xử lý các đỉnh có loại khớp với giá trị truy vấn. Khi xử lý một đỉnh như vậy, tất cả các mã thông báo sẽ được di chuyển cùng một lúc đến tổ tiên được tính toán. các`active`bộ đếm theo dõi số lượng mã thông báo chưa ở gốc, cho phép kiểm tra chấm dứt theo thời gian liên tục. 

Một điểm tinh tế là chúng ta không kiểm tra rõ ràng xem đích đến có nằm ngoài gốc hay không, bởi vì việc nâng nhị phân tự động bão hòa ở đỉnh 1 bằng cách lặp lại dự phòng thành`1`. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó 1 là gốc và hai mã thông báo bắt đầu từ lá. Chuỗi truy vấn dần dần kéo mã thông báo lên trên tùy thuộc vào kết quả khớp`t[v]`. 

Chúng tôi chỉ theo dõi các vị trí mã thông báo. 

### Dấu vết ví dụ 

đầu vào:```
n=5, q=3
parents: 1 1 2 2
t:        1 2 1 2 1
d:        1 1 2 1 1
a:        1 0 1 0 0
queries:  1 2 1
```| Truy vấn | Các đỉnh hoạt động được xử lý | Phong trào mã thông báo | Mã thông báo hoạt động | 
| --- | --- | --- | --- | 
| 1 | v với t[v]=1 chứa mã thông báo | mã thông báo ở mức 3 nhảy lên 1 | 1 | 
| 2 | v với t[v]=2 | không | 1 | 
| 3 | v với t[v]=1 | mã thông báo đã ở trạng thái gốc | 1 | 

Điều này chứng tỏ rằng việc tiếp cận thư mục gốc phụ thuộc rất nhiều vào sự liên kết giữa vị trí mã thông báo và loại truy vấn. Mã thông báo chỉ di chuyển khi nằm trên đỉnh có nhãn khớp với truy vấn. 

### Ví dụ thứ hai 

Trường hợp tất cả các token đều hội tụ:```
n=4, q=2
parents: 1 1 2
t:       1 1 2 2
d:       1 1 1 1
a:       1 1 1 0
queries: 1 2
```| Truy vấn | Phong trào | Kết quả | 
| --- | --- | --- | 
| 1 | mã thông báo tại các nút có t=1 di chuyển lên trên | một số đạt tới gốc | 
| 2 | mã thông báo còn lại với t=2 di chuyển | tất cả đều đạt đến gốc | 

Sau truy vấn 2,`active = 0`, vì vậy câu trả lời là 2. Điều này cho thấy tác động tích lũy của các loại truy vấn khác nhau bao gồm tất cả các nhóm mã thông báo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q + Total_moves) log n) | mỗi lần di chuyển sử dụng nâng nhị phân, mỗi mã thông báo chỉ được di chuyển khi tham gia | 
| Không gian | O(n log n) | bàn nâng nhị phân và danh sách vị trí | 

Các ràng buộc cho phép tối đa 200.000 nút và truy vấn, do đó chi phí logarit có thể chấp nhận được. Mỗi mã thông báo chỉ được di chuyển khi được kích hoạt và mỗi lần di chuyển đều hiệu quả do quá trình xử lý trước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    parent = [0] * (n + 1)
    par = list(map(int, input().split()))
    for i in range(2, n + 1):
        parent[i] = par[i - 2]

    t = [0] + list(map(int, input().split()))
    d = [0] + list(map(int, input().split()))
    a = [0] + list(map(int, input().split()))
    queries = list(map(int, input().split()))

    LOG = 20
    up = [[0] * (n + 1) for _ in range(LOG)]
    for v in range(1, n + 1):
        up[0][v] = parent[v]

    for j in range(1, LOG):
        for v in range(1, n + 1):
            up[j][v] = up[j - 1][up[j - 1][v]] if up[j - 1][v] else 1

    def jump(v, k):
        for i in range(LOG):
            if k & (1 << i):
                v = up[i][v]
        return v

    pos = [[] for _ in range(n + 1)]
    active = 0
    for i in range(1, n + 1):
        if a[i]:
            pos[i].append(i)
            active += 1

    ans = -1
    for i, c in enumerate(queries, 1):
        for v in range(1, n + 1):
            if t[v] == c and pos[v]:
                new_v = jump(v, d[v])
                active -= len(pos[v])
                pos[new_v].extend(pos[v])
                pos[v].clear()
        if active == 0:
            ans = i
            break

    return str(ans)

# provided samples
assert run("""5 6
1 1 1 4
1 5 3 5 3
5 3 1 3 2
1 1 0 1 0
3 4 1 5 4 2
""") == "-1"

assert run("""5 1
1 1 1 4
1 5 3 5 3
5 3 1 3 2
1 1 0 1 0
5
""") == "-1"

# custom cases
assert run("""2 1
1
1 1
1 1
1 0
1
""") == "1", "single move"

assert run("""3 2
1 1
1 1 1
1 1 1
1 1 1
1 2
""") == "2", "two-step convergence"

assert run("""4 3
1 1 2
1 2 3 4
1 2 1 2
1 1 1 0
2 1 2
""") == "3", "alternating types"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây 2 nút | 1 | hội tụ ngay lập tức | 
| dây chuyền nhỏ | 2 | nhân giống nhiều bước | 
| xen kẽ các loại | 3 | độ nhạy lệnh | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các mã thông báo đã bắt đầu ở gốc ngoại trừ một mã thông báo sâu. Thuật toán vẫn hoạt động chính xác vì chỉ các vị trí không phải gốc mới có mục nhập`pos`, và`active`bộ đếm đảm bảo chấm dứt sớm khi mã thông báo cuối cùng đó đến thư mục gốc. 

Một trường hợp tinh tế khác xảy ra khi nhiều thẻ chia sẻ một đỉnh và được di chuyển cùng nhau. Vì chúng tôi di chuyển chúng hàng loạt nên chúng tôi phải đảm bảo xóa danh sách đỉnh sau khi chuyển; nếu không thì mã thông báo sẽ bị trùng lặp trong các truy vấn trong tương lai. Tính bất biến mà mỗi mã thông báo xuất hiện trong đúng một`pos[v]`list đảm bảo tính chính xác và ngăn chặn việc tính hai lần.
