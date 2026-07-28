---
title: "CF 102787Z - Lừa hoặc Trấn"
description: "Vấn đề duy trì một tập hợp các nhóm nút treap được sắp xếp theo thứ tự. Một nút được tạo với một giá trị và mã định danh của nó là số truy vấn đã tạo ra nó."
date: "2026-07-27T19:19:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102787
codeforces_index: "Z"
codeforces_contest_name: "Algorithms Thread Treaps Contest"
rating: 0
weight: 102787
solve_time_s: 63
verified: true
draft: false
---

[CF 102787Z - Lừa hoặc bẫy](https://codeforces.com/problemset/problem/102787/Z) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Vấn đề duy trì một tập hợp các nhóm nút treap được sắp xếp theo thứ tự. Một nút được tạo với một giá trị và mã định danh của nó là số truy vấn đã tạo ra nó. Các nhóm hoạt động giống như các chuỗi: khi hai nhóm được nối với nhau, toàn bộ nhóm chứa một nút được đặt trước toàn bộ nhóm chứa nút kia. Một nhóm cũng có thể được cắt sau một số nút nhất định, tách tiền tố khỏi hậu tố của nó. Các truy vấn yêu cầu tổng của tất cả các giá trị trong nhóm chứa một nút cụ thể. 

Đầu vào là một luồng lên tới$5 \cdot 10^5$hoạt động. Vì mọi thao tác phải được xử lý trực tuyến nên việc xây dựng lại nhóm hoặc quét nội dung của chúng là không thể. Một giải pháp chạm tới mọi nút trong mỗi lần hợp nhất, phân tách hoặc truy vấn có thể tiếp cận$O(q^2)$, vượt xa những gì giới hạn 8 giây có thể xử lý. Chúng ta cần mỗi thao tác thực hiện theo thời gian logarit. 

Các trường hợp phức tạp xuất phát từ thực tế là các mã định danh nút không mô tả vị trí. Một nút có thể di chuyển tới bất cứ đâu trong nhóm của nó sau khi hợp nhất và phân chia. 

Ví dụ: sau:```
5
1 10
1 20
2 1 2
4 1
4 2
```đầu ra là:```
30
30
```Giải pháp bất cẩn chỉ nhớ nhóm ban đầu của mỗi nút sẽ thất bại vì nút 1 và 2 trở thành một phần của chuỗi có thứ tự giống nhau. 

Một trường hợp cạnh khác là tách một nhóm chứa nút được truy vấn nhưng để nút đó ở hai bên của phần cắt. 

Ví dụ:```
4
1 5
1 7
2 1 2
3 2 1
4 2
```Đầu ra là:```
7
```Trước khi phân chia, trình tự là`[1, 2]`. Tách sau khi nút đầu tiên được tạo`[1]`Và`[2]`. Nút 2 không còn nằm trong nhóm đầu tiên, do đó chỉ lưu trữ gốc cũ sẽ cho câu trả lời sai. 

Trường hợp ranh giới cuối cùng được phân chia ở gần cuối nhóm:```
3
1 8
3 1 1
4 1
```Đầu ra là:```
8
```Việc phân chia không có tác dụng gì vì nhóm chỉ có một nút. Việc triển khai giả định cả hai phần kết quả đều tồn tại có thể làm hỏng con trỏ gốc. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lưu trữ mọi nhóm dưới dạng danh sách. Việc tạo một nút rất dễ dàng và việc truy vấn một nhóm có nghĩa là tổng hợp danh sách của nó. Sự cố xuất hiện khi hoạt động di chuyển toàn bộ nhóm hoặc chia tách chúng. Việc hợp nhất hai nhóm lớn yêu cầu sao chép hoặc liên kết nhiều phần tử và việc tách yêu cầu phải đi đến vị trí cắt. Trong trường hợp xấu nhất,$5 \cdot 10^5$mỗi thao tác có thể chạm vào$5 \cdot 10^5$các nút, đưa ra về$2.5 \cdot 10^{11}$hoạt động. 

Cấu trúc của các hoạt động mang lại quan sát chính. Các nhóm không phải là các tập hợp tùy ý, chúng là các chuỗi có thứ tự. Các thao tác được yêu cầu chính xác là các thao tác được hỗ trợ bởi một treap ngầm: nối hai chuỗi, tách chuỗi theo vị trí và duy trì thông tin tổng hợp, chẳng hạn như tổng các giá trị. 

Thử thách còn lại là trả lời chuỗi nào chứa một nút nhất định. Mỗi nút treap lưu trữ một con trỏ cha. Con trỏ cha đi theo sau sẽ đến gốc hiện tại của nhóm của nó. Bởi vì chiều cao treap trung bình là logarit nên việc tìm nhóm cũng là logarit. 

Lực lượng vũ phu hoạt động vì nó mô hình hóa các nhóm một cách trực tiếp, nhưng không thành công vì nó bỏ qua rằng các nhóm là các chuỗi động. Việc quan sát rằng mọi thao tác là một thao tác theo trình tự cho phép chúng tôi biểu diễn mọi nhóm bằng một thuật toán ẩn và giữ cho tất cả các thao tác được nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q^2)$|$O(q)$| Quá chậm | 
| Tối ưu |$O(q \log q)$dự kiến ​​|$O(q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khi một truy vấn tạo xuất hiện, hãy tạo một nút treap ẩn. Giá trị của nó là giá trị đã cho, kích thước của nó là một và tổng của nó là giá trị đó. Lưu trữ chỉ mục của nó để các truy vấn trong tương lai có thể truy cập trực tiếp vào nút này. 
2. Đối với truy vấn hợp nhất, hãy tìm gốc của hai nút được tham chiếu. Nếu họ đã ở trong cùng một khu vực thì không có gì thay đổi. Nếu không, hãy hợp nhất hai trep, đặt gốc thứ nhất trước gốc thứ hai. Hoạt động hợp nhất treap ngầm giữ nguyên thứ tự trình tự trong khi vẫn giữ cho cây cân bằng. 
3. Đối với truy vấn phân tách, hãy tìm nút gốc của nút được tham chiếu. Chia phần thưởng đó sau lần đầu tiên$z$nút. Hai treap kết quả đại diện cho các nhóm tiền tố và hậu tố. Bản thân nút có thể nằm ở một trong hai phần, đó là lý do tại sao chúng ta phải dựa vào các con trỏ cha thay vì các mã định danh nhóm được lưu trữ. 
4. Đối với truy vấn tổng, hãy leo từ nút được tham chiếu qua các con trỏ cha cho đến khi đến gốc nhóm. Gốc lưu trữ tổng của toàn bộ kho dữ liệu của nó, vì vậy giá trị đó chính là câu trả lời. 

Tại sao nó hoạt động: 

Điều bất biến là mỗi treap đại diện chính xác cho một nhóm hiện tại và việc duyệt treap theo thứ tự là thứ tự của các nút bên trong nhóm đó. Hợp nhất giữ cho trình tự theo thứ tự bằng với sự kết hợp của hai nhóm. Split chia chuỗi theo thứ tự tại vị trí được yêu cầu, do đó cả hai nhóm kết quả đều chứa chính xác các nút chính xác. Tổng của cây con được lưu trữ được cập nhật sau mỗi lần thay đổi cấu trúc, làm cho tổng gốc bằng tổng của cả nhóm. Con trỏ gốc luôn kết nối mọi nút với gốc hiện tại của treap, do đó, truy vấn nút luôn đến đúng nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import random
sys.setrecursionlimit(1 << 25)

left = [0]
right = [0]
parent = [0]
size = [0]
total = [0]
value = [0]
priority = [0]

def pull(x):
    if x:
        size[x] = size[left[x]] + size[right[x]] + 1
        total[x] = total[left[x]] + total[right[x]] + value[x]

def merge(a, b):
    if not a:
        if b:
            parent[b] = 0
        return b
    if not b:
        parent[a] = 0
        return a
    if priority[a] > priority[b]:
        right[a] = merge(right[a], b)
        if right[a]:
            parent[right[a]] = a
        pull(a)
        parent[a] = 0
        return a
    else:
        left[b] = merge(a, left[b])
        if left[b]:
            parent[left[b]] = b
        pull(b)
        parent[b] = 0
        return b

def split(t, k):
    if not t:
        return 0, 0
    if size[left[t]] >= k:
        a, b = split(left[t], k)
        left[t] = b
        if b:
            parent[b] = t
        pull(t)
        parent[t] = 0
        if a:
            parent[a] = 0
        return a, t
    else:
        a, b = split(right[t], k - size[left[t]] - 1)
        right[t] = a
        if a:
            parent[a] = t
        pull(t)
        parent[t] = 0
        if b:
            parent[b] = 0
        return t, b

def root_of(x):
    while parent[x]:
        x = parent[x]
    return x

def new_node(v):
    idx = len(value)
    left.append(0)
    right.append(0)
    parent.append(0)
    size.append(1)
    total.append(v)
    value.append(v)
    priority.append(random.randrange(1 << 60))
    return idx

def solve():
    q = int(input())
    ans = []
    nodes = [0] * (q + 1)

    for i in range(1, q + 1):
        query = list(map(int, input().split()))
        t = query[0]

        if t == 1:
            nodes[i] = new_node(query[1])

        elif t == 2:
            y, z = query[1], query[2]
            a = root_of(nodes[y])
            b = root_of(nodes[z])
            if a != b:
                merge(a, b)

        elif t == 3:
            y, z = query[1], query[2]
            r = root_of(nodes[y])
            if size[r] > z:
                split(r, z)

        else:
            y = query[1]
            ans.append(str(total[root_of(nodes[y])]))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Các mảng đại diện cho các trường của nút tre. Sử dụng mảng thay vì đối tượng Python giúp giảm chi phí bộ nhớ vì số lượng nút tối đa là$5 \cdot 10^5$.`merge`Và`split`là các hoạt động xử lý ngầm tiêu chuẩn. Sau mỗi lần thay đổi con trỏ,`pull`tính toán lại kích thước và tổng của cây con. Con trỏ gốc được cập nhật trong các thao tác này vì chúng được yêu cầu sau này khi tìm nhóm hiện tại của nút. 

các`root_of`chức năng không cần nén đường dẫn. Chiều cao của treap dự kiến ​​sẽ giữ ở mức logarit vì mức độ ưu tiên là ngẫu nhiên, do đó việc đi lên là đủ nhanh. 

Điều kiện phân chia sử dụng`size[left[t]]`bởi vì các vùng ngầm định quyết định vị trí dựa trên kích thước của cây con bên trái. Điều này tránh được bất kỳ sai sót nào khi tách phần đầu tiên$z$nút. 

## Ví dụ đã hoạt động 

Sử dụng đầu vào mẫu:```
10
1 38788
3 1 1
3 1 2
1 56200
3 1 2
3 1 2
4 4
3 4 4
4 1
3 4 6
```| Bước | Hoạt động | Nhóm chứa nút 4 | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | Tạo nút 1 với 38788 | [1] | 38788 | 
| 2 | Chia sau 1 | [1] | 38788 | 
| 3 | Chia sau 2 | [1] | 38788 | 
| 4 | Tạo nút 4 với 56200 | [4] | 56200 | 
| 5 | Chia sau 2 | [4] | 56200 | 
| 6 | Chia sau 2 | [4] | 56200 | 
| 7 | Nút truy vấn 4 | [4] | 56200 | 

Dấu vết đầu tiên cho thấy việc tách một nhóm nút đơn sẽ khiến nhóm không thay đổi. 

Một ví dụ thứ hai:```
5
1 10
1 20
2 1 2
4 1
4 2
```| Bước | Hoạt động | Trình tự | Tổng hợp | 
| --- | --- | --- | --- | 
| 1 | Tạo nút 1 | [10] | 10 | 
| 2 | Tạo nút 2 | [20] | 20 | 
| 3 | Hợp nhất nhóm nút 1 trước nhóm nút 2 | [10,20] | 30 | 
| 4 | Nút truy vấn 1 | [10,20] | 30 | 
| 5 | Nút truy vấn 2 | [10,20] | 30 | 

Điều này chứng tỏ rằng các nút giữ được danh tính của chúng trong khi trình tự xung quanh chúng thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log q)$dự kiến ​​| Mỗi thao tác thực hiện một số lần hợp nhất, tách hoặc tìm kiếm gốc không đổi. | 
| Không gian |$O(q)$| Một nút treap được tạo cho mỗi truy vấn tạo. | 

Đầu vào lớn nhất chứa nửa triệu thao tác. Chi phí dự kiến ​​theo logarit của mỗi thao tác xử lý giữ cho tổng công việc ở khoảng vài triệu lệnh gọi đệ quy, vừa vặn trong bộ nhớ và giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_out
    sys.stdin = old
    return out.getvalue()

assert run("""10
1 38788
3 1 1
3 1 2
1 56200
3 1 2
3 1 2
4 4
3 4 4
4 1
3 4 6
""") == "56200\n38788\n"

assert run("""5
1 10
1 20
2 1 2
4 1
4 2
""") == "30\n30\n"

assert run("""1
1 5
""") == ""

assert run("""6
1 1
1 1
1 1
2 1 2
2 2 3
4 1
""") == "3\n"

assert run("""5
1 8
1 9
2 1 2
3 1 1
4 1
""") == "8\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Sáng tạo đơn lẻ | Không có đầu ra | Đầu vào kích thước tối thiểu | 
| Hai nút hợp nhất | 30 | Hợp nhất và truy vấn cơ bản | 
| Ba giá trị bằng nhau | 3 | Giá trị bằng nhau lặp lại | 
| Tách sau khi hợp nhất | 8 | Hành vi phân chia ranh giới | 

## Vỏ cạnh 

Đối với vấn đề nhận dạng hợp nhất:```
5
1 10
1 20
2 1 2
4 1
4 2
```Thuật toán lưu trữ các nút riêng biệt tại thời điểm tạo, sau đó thao tác hợp nhất sẽ tạo ra một vùng chứa cả hai nút. Cả hai chuỗi gốc hiện đều có cùng một gốc nên cả hai truy vấn đều trả về tổng kết hợp. 

Để phân chia xung quanh nút được truy vấn:```
4
1 5
1 7
2 1 2
3 2 1
4 2
```Trình tự xử lý`[5,7]`được chia thành`[5]`Và`[7]`. Nút 2 trở thành gốc của treap thứ hai, vì vậy tổng gốc của nó là 7. Quá trình truyền tải gốc sẽ tìm thấy gốc mới đó thay vì sử dụng thông tin cũ. 

Đối với một sự phân chia không thể tách rời bất cứ điều gì:```
3
1 8
3 1 1
4 1
```Thuật toán kiểm tra kích thước nhóm trước khi tách. Vì kích thước không lớn hơn độ dài tiền tố được yêu cầu nên thao tác không chạm vào treap và truy vấn vẫn thấy tổng bằng 8.
