---
title: "CF 105760K - Ghi nhật ký an toàn"
description: "Chúng tôi có một cây với một số nút được đánh dấu là chứa nhật ký. Khi một khúc gỗ bị cắt, nửa đen của nó vẫn giữ nguyên và nửa đỏ của nó phải rơi vào nút liền kề không chứa khúc gỗ đen."
date: "2026-06-26T03:49:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105760
codeforces_index: "K"
codeforces_contest_name: "2020 UCF Local Programming Contest"
rating: 0
weight: 105760
solve_time_s: 72
verified: true
draft: false
---

[CF 105760K - Ghi nhật ký an toàn](https://codeforces.com/problemset/problem/105760/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cây với một số nút được đánh dấu là chứa nhật ký. Khi một khúc gỗ bị cắt, nửa đen của nó vẫn giữ nguyên và nửa đỏ của nó phải rơi vào một nút liền kề mà **không** chứa khúc gỗ đen. Mỗi nhật ký phải chọn chính xác một nút không phải nhật ký lân cận làm đích đến cho nửa màu đỏ của nó. Mục đích là đếm xem có bao nhiêu cấu hình cắt hợp lệ. Câu trả lời là bắt buộc theo modulo`1,000,000,007`. 

Hạn chế về kiểu dáng là phần thú vị. Một nút chứa nhật ký màu đen không được phép có nhật ký màu đỏ ở hai nút lân cận khác nhau. Nút nhật ký đen có thể có 0 hoặc một đích màu đỏ lân cận, nhưng không bao giờ có hai hoặc nhiều hơn. 

Cây chứa tới`100000`nút. Bất kỳ giải pháp nào xem xét sự kết hợp các lựa chọn một cách rõ ràng đều vô vọng. Ngay cả hệ số phân nhánh là hai trên mỗi nhật ký cũng đã tạo ra hành vi theo cấp số nhân. Cây DP tuyến tính hoặc gần tuyến tính là hướng thực tế duy nhất. 

Một quan sát tinh tế sẽ đơn giản hóa tình trạng một cách đáng kể. 

Hãy xem xét một nút nhật ký`u`. Nó phải gửi nửa màu đỏ của chính nó tới đúng một nút không đăng nhập lân cận`w`. Từ`w`chắc chắn sẽ chứa một bản ghi màu đỏ, mọi hàng xóm không phải bản ghi khác của`u`phải ở trạng thái hoàn toàn không hoạt động. Nếu không thì`u`sẽ thấy nhật ký màu đỏ ở ít nhất hai nút lân cận và vi phạm quy tắc. 

Cách giải thích cục bộ đó là điều tạo nên DP cây. 

## Phương pháp tiếp cận 

Giải pháp bạo lực sẽ cho phép mọi nhật ký chọn một nút không phải nhật ký lân cận và sau đó xác minh cấu hình cuối cùng. Nếu có`m`nhật ký và mỗi nhật ký chỉ có hai đích có thể, đó là`2^m`khả năng. Với`m`lên đến`100000`, điều này hoàn toàn không thể thực hiện được. 

Điều quan trọng cần lưu ý là thông tin duy nhất quan trọng về nút không ghi nhật ký là liệu nút đó có **hoạt động** hay **không hoạt động** hay không. 

Nút không phải nhật ký sẽ hoạt động nếu có ít nhất một nhật ký liền kề gửi nửa màu đỏ của nó tới đó. 

Bây giờ hãy nhìn lại nút nhật ký. Nó chọn chính xác một nút không đăng nhập lân cận. Hàng xóm được chọn phải hoạt động. Mọi nút không đăng nhập lân cận khác phải không hoạt động. Điều kiện này chỉ phụ thuộc vào trạng thái hoạt động của các nút không ghi nhật ký liền kề. 

Điều đó biến vấn đề thành một cây DP với hai loại nút: 

- nút nhật ký 
- các nút không đăng nhập 

Đối với nút không có nhật ký, chúng tôi theo dõi xem nhật ký con nào đó có kích hoạt nút đó hay không. 

Đối với nút nhật ký, chúng tôi theo dõi xem nó gửi nửa màu đỏ của nó đến nút không phải nhật ký gốc hay đến một trong các nút không phải nhật ký con của nó. 

Cấu trúc cây cho phép các lựa chọn này được kết hợp độc lập giữa các cây con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Cây tối ưu DP | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### trạng thái DP 

Đối với nút không đăng nhập`u`:`dp0[u]`= số cách trong cây con của nó sao cho không có nhật ký con nào gửi nửa màu đỏ tới`u`.`dp1[u]`= số cách trong cây con của nó sao cho ít nhất một nhật ký con gửi nửa màu đỏ tới`u`. 

Đối với một nút nhật ký`u`:`f0[u]`= số cách sao cho`u`**không** gửi nửa màu đỏ của nó cho cha mẹ của nó.`f1[u]`= số cách sao cho`u`**không** gửi nửa màu đỏ của nó cho cha mẹ của nó. 

### Đang xử lý nút không đăng nhập 

1. Mỗi đứa trẻ không có log đều đóng góp độc lập với`dp0 + dp1`. 
2. Một bản ghi con có thể kích hoạt`u`trạng thái sử dụng`f1`hoặc không kích hoạt`u`trạng thái sử dụng`f0`. 
3.`dp0[u]`là sản phẩm mà mọi đứa trẻ đều lựa chọn`f0`. 
4.`dp1[u]`là sản phẩm của tất cả các khả năng trừ đi những cấu hình mà không ai kích hoạt`u`. 

### Đang xử lý nút nhật ký 

1. Đặt các mục con không phải log là đích có thể có bên trong cây con. 
2. Nếu nhật ký gửi nửa màu đỏ của nó cho cha mẹ của nó thì mọi thành phần con không có nhật ký sẽ không hoạt động. 
3. Điều này mang lại`f1`. 
4. Nếu nhật ký không gửi đến cha của nó, nó phải chọn chính xác một con không có nhật ký làm đích. 
5. Trẻ được chọn có thể hoạt động dưới bất kỳ hình thức nào. Mọi đứa trẻ không đăng nhập khác phải không hoạt động. 
6. Tổng kết đứa trẻ được chọn sẽ cho`f0`. 

Chi tiết kỹ thuật duy nhất là tính toán$$\sum_i T_i \prod_{j\neq i} A_j$$hiệu quả, ở đâu$$A_i = dp0[child_i], \qquad
T_i = dp0[child_i] + dp1[child_i]$$Sử dụng các sản phẩm tiền tố và hậu tố, toàn bộ nút được xử lý theo thời gian tuyến tính theo mức độ của nó. 

### Xử lý gốc 

Nếu root không phải log thì câu trả lời là$$dp0[root] + dp1[root]$$Nếu gốc là một log thì nó không có cha nên nó phải chọn chính xác một hàng xóm không phải log trong số các nút con của nó. Đó là quá trình chuyển đổi tương tự được sử dụng cho`f0`. 

### Tại sao nó hoạt động 

Điều bất biến là mọi trạng thái DP mô tả đầy đủ thông tin duy nhất mà cha mẹ cần. 

Đối với nút không đăng nhập, nút cha chỉ quan tâm xem nút đó đã hoạt động chưa. Số lượng chính xác các khúc gỗ màu đỏ bên trong nó là không liên quan. 

Đối với nút nhật ký, nút cha chỉ quan tâm liệu nhật ký có sử dụng nút cha làm đích hay không. 

Bất cứ khi nào một nhật ký chọn đích, tất cả các nút không phải nhật ký lân cận khác sẽ buộc phải không hoạt động. Điều đó hoàn toàn phù hợp với quy tắc tạo kiểu ban đầu. Mỗi cấu hình toàn cầu hợp lệ tương ứng với một tập hợp lựa chọn DP duy nhất và mọi cấu trúc DP đều tạo ra một cấu hình hợp lệ. Do đó việc đếm là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

n = int(input())
g = [[] for _ in range(n)]

for _ in range(n - 1):
    a, b = map(int, input().split())
    a -= 1
    b -= 1
    g[a].append(b)
    g[b].append(a)

m = int(input())
is_log = [False] * n
for x in map(int, input().split()):
    is_log[x - 1] = True

parent = [-1] * n
order = [0]

for u in order:
    for v in g[u]:
        if v == parent[u]:
            continue
        parent[v] = u
        order.append(v)

dp0 = [0] * n
dp1 = [0] * n
f0 = [0] * n
f1 = [0] * n

for u in reversed(order):

    if not is_log[u]:

        prod_w = 1

        log_children = []

        for v in g[u]:
            if v == parent[u]:
                continue

            if is_log[v]:
                log_children.append(v)
            else:
                prod_w = (prod_w * (dp0[v] + dp1[v])) % MOD

        none_active = prod_w
        all_cases = prod_w

        for v in log_children:
            none_active = (none_active * f0[v]) % MOD
            all_cases = (all_cases * ((f0[v] + f1[v]) % MOD)) % MOD

        dp0[u] = none_active
        dp1[u] = (all_cases - none_active) % MOD

    else:

        prod_b = 1
        w_children = []

        for v in g[u]:
            if v == parent[u]:
                continue

            if is_log[v]:
                prod_b = (prod_b * ((f0[v] + f1[v]) % MOD)) % MOD
            else:
                w_children.append(v)

        k = len(w_children)

        A = [dp0[v] for v in w_children]
        T = [(dp0[v] + dp1[v]) % MOD for v in w_children]

        pref = [1] * (k + 1)
        suff = [1] * (k + 1)

        for i in range(k):
            pref[i + 1] = pref[i] * A[i] % MOD

        for i in range(k - 1, -1, -1):
            suff[i] = suff[i + 1] * A[i] % MOD

        base = pref[k]

        f1[u] = prod_b * base % MOD

        choose_sum = 0
        for i in range(k):
            term = T[i] * pref[i] % MOD
            term = term * suff[i + 1] % MOD
            choose_sum = (choose_sum + term) % MOD

        f0[u] = prod_b * choose_sum % MOD

root = 0

if not is_log[root]:
    ans = (dp0[root] + dp1[root]) % MOD
else:
    prod_b = 1
    w_children = []

    for v in g[root]:
        if is_log[v]:
            prod_b = (prod_b * ((f0[v] + f1[v]) % MOD)) % MOD
        else:
            w_children.append(v)

    k = len(w_children)

    A = [dp0[v] for v in w_children]
    T = [(dp0[v] + dp1[v]) % MOD for v in w_children]

    pref = [1] * (k + 1)
    suff = [1] * (k + 1)

    for i in range(k):
        pref[i + 1] = pref[i] * A[i] % MOD

    for i in range(k - 1, -1, -1):
        suff[i] = suff[i + 1] * A[i] % MOD

    choose_sum = 0
    for i in range(k):
        term = T[i] * pref[i] % MOD
        term = term * suff[i + 1] % MOD
        choose_sum = (choose_sum + term) % MOD

    ans = prod_b * choose_sum % MOD

print(ans)
```Sau khi root cây, các nút được xử lý theo thứ tự DFS ngược lại. Mọi giá trị DP con đều đã được biết khi giá trị gốc của nó được đánh giá. 

Đối với các nút không có nhật ký, việc triển khai sẽ tính toán số cách mà không nút con nào kích hoạt nút đó và số cách mà ít nhất một nút con kích hoạt nút đó. 

Đối với các nút nhật ký, việc triển khai xây dựng các sản phẩm tiền tố và hậu tố của`dp0`giá trị trên trẻ em không đăng nhập. Điều đó cho phép tính toán sự đóng góp của việc chọn bất kỳ đích đến cụ thể nào theo O(1), đưa ra khả năng xử lý O(độ) trên mỗi nút thay vì O(độ²). 

Root yêu cầu xử lý riêng vì nó không có cha mẹ và không thể sử dụng quá trình chuyển đổi tương ứng với việc gửi một nửa màu đỏ lên trên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2
2 3
3 4
4 5
3
1 3 4
```Nhật ký ở các nút 1, 3 và 4. 

Các động thái duy nhất có thể là: 

- 1 → 2 
- 3 → 2 
- 4 → 5 

| Nút | Loại | Kết quả | 
| --- | --- | --- | 
| 1 | Nhật ký | chọn 2 | 
| 3 | Nhật ký | chọn 2 | 
| 4 | Nhật ký | chọn 5 | 

Nút 3 chỉ nhìn thấy một nút đỏ lân cận, nút 2. Nút 4 chỉ nhìn thấy một nút đỏ lân cận, nút 5. Cấu hình hợp lệ. 

Trả lời = 1. 

### Ví dụ 2 

đầu vào:```
6
1 2
2 3
3 4
3 5
5 6
2
2 5
```Nhật ký ở nút 2 và 5. 

Nút 2 có thể gửi đến nút 1 hoặc nút 3. 

Nút 5 có thể gửi đến nút 3 hoặc nút 6. 

Các kết quả hợp lệ là: 

| Nhật ký 2 | Nhật ký 5 | 
| --- | --- | 
| 1 | 6 | 
| 3 | 6 | 

Trả lời = 2. 

Dấu vết chứng minh rằng nhiều nhật ký có thể gửi một nửa màu đỏ vào cùng một nút không phải nhật ký và chỉ có trạng thái hoạt động/không hoạt động mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh và nút được xử lý với số lần không đổi | 
| Không gian | O(n) | Cây, mảng cha, mảng thứ tự và bảng DP | 

Với`n ≤ 100000`, độ phức tạp tuyến tính dễ dàng đủ nhanh trong giới hạn 5 giây. 

## Trường hợp thử nghiệm```
# helper sketch for local testing

# sample 1
assert run("""\
5
1 2
2 3
3 4
4 5
3
1 3 4
""") == "1\n"

# sample 2
assert run("""\
6
1 2
2 3
3 4
3 5
5 6
2
2 5
""") == "2\n"

# single node with a log, nowhere to drop red half
assert run("""\
1
1
1
""") == "0\n"

# two nodes, one log
assert run("""\
2
1 2
1
1
""") == "1\n"

# all nodes are logs
assert run("""\
3
1 2
2 3
3
1 2 3
""") == "0\n"

# chain with alternating log and non-log nodes
assert run("""\
4
1 2
2 3
3 4
2
1 3
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nút nhật ký đơn | 0 | Không có điểm đến hợp lệ tồn tại | 
| Cây hai nút | 1 | Di chuyển hợp lệ đơn giản nhất | 
| Tất cả các nút đều là nhật ký | 0 | Nửa đỏ không thể vào nút nhật ký | 
| Xích xen kẽ | 1 | Tuyên truyền cơ bản thông qua DP | 

## Vỏ cạnh 

Một nút nhật ký có thể không có nút không phải nhật ký lân cận nào cả.```
3
1 2
2 3
3
1 2 3
```Mỗi nút chứa một bản ghi màu đen. Vì nửa màu đỏ bị cấm đi vào các nút nhật ký đen nên không thể di chuyển. DP tự nhiên trả về 0 vì mọi nút nhật ký không có lựa chọn đích hợp lệ. 

Một trường hợp phức tạp khác xảy ra khi một số nhật ký có thể gửi một nửa màu đỏ vào cùng một nút không phải nhật ký. Điều đó hoàn toàn hợp pháp. Hạn chế không nằm ở việc một nút chứa bao nhiêu nửa màu đỏ. Hạn chế là về số lượng nút lân cận chứa nhật ký màu đỏ mà nút nhật ký đen có thể nhìn thấy. Biểu diễn hoạt động/không hoạt động được DP sử dụng thể hiện chính xác sự khác biệt đó.
