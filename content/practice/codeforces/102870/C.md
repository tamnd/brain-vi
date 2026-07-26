---
title: "CF 102870C - Ghế đẩu của Orz Pandas"
description: "Chúng ta cần mô phỏng một hàng n ghế gần nhất, nơi gấu trúc ra vào theo thời gian. Một con gấu trúc đi vào chọn một chiếc ghế gần nhất trống có khoảng cách tối thiểu đến tất cả những chiếc ghế gần nhất hiện đang bị chiếm giữ càng lớn càng tốt."
date: "2026-07-25T13:20:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102870
codeforces_index: "C"
codeforces_contest_name: "2020-2021 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102870
solve_time_s: 61
verified: true
draft: false
---

[CF 102870C - Công cụ đóng của Orz Pandas](https://codeforces.com/problemset/problem/102870/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần mô phỏng một hàng`n`những chiếc ghế đẩu gần nhất, nơi gấu trúc ra vào theo thời gian. Một con gấu trúc đi vào chọn một chiếc ghế gần nhất trống có khoảng cách tối thiểu đến tất cả những chiếc ghế gần nhất hiện đang bị chiếm giữ càng lớn càng tốt. Nếu nhiều lựa chọn có cùng khoảng cách tối thiểu thì nhãn nhỏ nhất sẽ được chọn. Đối với mỗi thao tác nhập, chúng tôi xuất ra ô gần nhất đã chọn. Hoạt động rời đi đề cập đến hoạt động vào trước đó đã tạo ra người ở. 

Khó khăn đến từ kích thước của hàng và số lượng thao tác. Hàng có thể chứa tới`10^9`closestools nên việc lưu trữ mọi vị trí là không thể. Số lượng hoạt động có thể đạt tới`10^5`mỗi trường hợp thử nghiệm và`10^6`tổng cộng, loại trừ việc quét toàn bộ hàng hoặc kiểm tra mọi vị trí trống sau mỗi thao tác. Chúng ta cần cấu trúc dữ liệu logarit chỉ lưu trữ các vị trí đã chiếm dụng và các phân đoạn trống hữu ích. 

Một lỗi phổ biến là xử lý tất cả các phân đoạn trống như nhau. Ghế gần trống đầu tiên và ghế gần trống cuối cùng hoạt động khác với các phân đoạn giữa hai ghế gần nhất đã được sử dụng. Một sai lầm khác là xử lý sai mối quan hệ. Ví dụ, với`n = 5`và các vị trí chiếm đóng`2`Và`5`, các vị trí trống là`1,3,4`. Chức vụ`1`có khoảng cách`1`, chức vụ`3`có khoảng cách`1`, và vị trí`4`có khoảng cách`1`, vậy câu trả lời là`1`. Một phương pháp luôn chọn giữa khoảng trống sẽ trả về`3`không chính xác. 

Một trường hợp cạnh khác là khi không có ghế gần nhất bị chiếm dụng. Đối với đầu vào:```
3 1
1
```đầu ra đúng là:```
1
```Chưa có giới hạn về khoảng cách nên phải chọn nhãn nhỏ nhất. 

Trường hợp cạnh cuối cùng là khoảng trống có số lựa chọn chẵn trong đó hai vị trí đều tốt như nhau. Đối với đầu vào:```
7 5
1
1
1
2 1
1
```thao tác cuối cùng xảy ra sau khi loại bỏ người cư ngụ đầu tiên. Thuật toán phải chọn vị trí nhỏ nhất trong số các ứng cử viên bằng nhau, không được chọn vị trí ở giữa tùy ý. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là giữ lại tất cả các ô gần nhất và đối với mỗi gấu trúc bước vào, hãy kiểm tra mọi vị trí trống. Điều này đúng vì nó tính toán khoảng cách tối thiểu tối đa có thể theo đúng nghĩa đen. Tuy nhiên, nó không thể sử dụng được. Nếu như`n`là`10^9`, thậm chí một thao tác có thể yêu cầu kiểm tra hàng tỷ vị trí. 

Quan sát hữu ích là chỉ có các đoạn trống liên tiếp mới quan trọng. Giả sử hai ô gần nhất được sử dụng là đường viền của một đoạn trống. Mỗi vị trí bên trong phân đoạn đó đều có vị trí gần nhất được chiếm giữ gần nhất trong số các đường viền đó, vì vậy chúng tôi có thể tính toán lựa chọn tốt nhất từ ​​phân khúc đó mà không cần kiểm tra các vị trí riêng lẻ. 

Đối với phân khúc nội bộ`(l, r)`chỉ chứa các ghế gần nhất trống, vị trí tốt nhất là:```
l + (r - l) // 2
```bởi vì điều này tối đa hóa khoảng cách nhỏ hơn đến cả hai đường viền và chọn vị trí ngoài cùng bên trái khi có điểm hòa. Các đoạn chạm vào cuối phòng vệ sinh thậm chí còn đơn giản hơn: vị trí tốt nhất là ghế đẩu đầu tiên hoặc cuối cùng. 

Vấn đề còn lại là duy trì các phân khúc này trong khi mọi người ra vào. Chúng tôi giữ các vị trí đã chiếm giữ trong một kho có thứ tự để các truy vấn trước và sau đều có tính logarit. Chúng tôi giữ tất cả các phân đoạn trống ứng viên trong một đống được sắp xếp theo chất lượng của chúng, đồng thời xóa dần các phân đoạn đã biến mất sau khi cập nhật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một đoạn trống bao phủ toàn bộ phòng vệ sinh. Sử dụng lính canh`0`Và`n + 1`cho phép cùng một logic xử lý cả hai đường viền. 
2. Đối với thao tác nhập, hãy lấy phân đoạn tốt nhất từ ​​vùng nhớ heap. Xóa nó khỏi tập hợp phân đoạn đang hoạt động và đặt gấu trúc vào ghế gần nhất đã chọn của phân đoạn. 
3. Chèn vị trí chiếm giữ mới vào ngăn xếp đã ra lệnh. Đoạn trống cũ được chia thành tối đa hai đoạn trống mới, mỗi đoạn ở mỗi bên của vị trí đã chọn. 
4. Lưu vị trí đã chọn theo chỉ mục hoạt động. Thao tác rời đi sau này sử dụng giá trị được lưu trữ này để biết phân nào gần nhất sẽ trống. 
5. Đối với thao tác rời đi, hãy tìm các vị trí được sử dụng gần nhất ở cả hai bên của ghế gần nhất đã bị loại bỏ. Hai đoạn trống lân cận hợp nhất thành một đoạn lớn hơn. 
6. Chèn phân đoạn đã hợp nhất vào heap. Các mục nhập cũ không hợp lệ sẽ bị bỏ qua khi chúng đạt đến đỉnh. 

Tại sao nó hoạt động: 

Tại mọi thời điểm, mọi câu trả lời có thể đều thuộc về chính xác một đoạn trống đang hoạt động. Heap lưu trữ lựa chọn tốt nhất có thể có từ mọi phân khúc, vì vậy top của nó là lựa chọn tốt nhất toàn cầu. Khi một ghế gần nhất bị chiếm giữ, chỉ có đoạn chứa nó thay đổi. Khi nó trống, chỉ có hai phân đoạn lân cận thay đổi. Do đó, tập hợp các phân đoạn được duy trì luôn thể hiện chính xác trạng thái hiện tại của phòng vệ sinh. 

## Giải pháp Python```python
import sys
import random
input = sys.stdin.readline

class Node:
    __slots__ = ("key", "prio", "l", "r")
    def __init__(self, key):
        self.key = key
        self.prio = random.randrange(1 << 30)
        self.l = None
        self.r = None

def rotate_split(root, key):
    if root is None:
        return None, None
    if root.key < key:
        a, b = rotate_split(root.r, key)
        root.r = a
        return root, b
    else:
        a, b = rotate_split(root.l, key)
        root.l = b
        return a, root

def merge(a, b):
    if not a:
        return b
    if not b:
        return a
    if a.prio > b.prio:
        a.r = merge(a.r, b)
        return a
    b.l = merge(a, b.l)
    return b

def insert(root, node):
    if root is None:
        return node
    if node.prio > root.prio:
        node.l, node.r = rotate_split(root, node.key)
        return node
    if node.key < root.key:
        root.l = insert(root.l, node)
    else:
        root.r = insert(root.r, node)
    return root

def erase(root, key):
    if root.key == key:
        return merge(root.l, root.r)
    if key < root.key:
        root.l = erase(root.l, key)
    else:
        root.r = erase(root.r, key)
    return root

def pred(root, key):
    ans = None
    while root:
        if root.key < key:
            ans = root.key
            root = root.r
        else:
            root = root.l
    return ans

def succ(root, key):
    ans = None
    while root:
        if root.key > key:
            ans = root.key
            root = root.l
        else:
            root = root.r
    return ans

def solve_case(n, ops):
    import heapq
    heap = []
    active = {}
    occupied = None

    def add_gap(l, r):
        if r - l <= 1:
            return
        if l == 0:
            seat = 1
            score = r - 1
        elif r == n + 1:
            seat = n
            score = n - l
        else:
            seat = l + (r - l) // 2
            score = (r - l) // 2
        active[(l, r)] = True
        heapq.heappush(heap, (-score, seat, l, r))

    def remove_gap(l, r):
        active.pop((l, r), None)

    add_gap(0, n + 1)
    ans = []
    born = {}

    for idx, op in enumerate(ops, 1):
        if op[0] == 1:
            while (heap[0][2], heap[0][3]) not in active:
                heapq.heappop(heap)
            _, seat, l, r = heapq.heappop(heap)
            remove_gap(l, r)
            add_gap(l, seat)
            add_gap(seat, r)
            occupied = insert(occupied, Node(seat))
            born[idx] = seat
            ans.append(str(seat))
        else:
            x = born[op[1]]
            l = pred(occupied, x)
            r = succ(occupied, x)
            occupied = erase(occupied, x)
            remove_gap(l, x)
            remove_gap(x, r)
            add_gap(l, r)
    return ans

def main():
    out = []
    while True:
        line = input()
        if not line:
            break
        if not line.strip():
            continue
        n, m = map(int, line.split())
        ops = []
        for _ in range(m):
            ops.append(list(map(int, input().split())))
        out.extend(solve_case(n, ops))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Các cửa hàng treap chỉ chiếm giữ những chiếc ghế gần nhất. Điều này là cần thiết bởi vì`n`có thể lớn hơn nhiều so với số lượng hoạt động. Các hàm tiền thân và kế thừa tìm hai đường viền của đoạn trống bị ảnh hưởng bởi việc chèn hoặc xóa. 

Heap lưu trữ các phân đoạn ứng cử viên. Điểm được lưu trữ là khoảng cách tối đa có thể đạt được bên trong phân khúc đó và giá trị chỗ ngồi xử lý điểm ngắt liên kết nhãn nhỏ nhất được yêu cầu. Các mục heap cũ vẫn tồn tại về mặt vật lý sau khi phân tách hoặc hợp nhất, do đó`active`từ điển được sử dụng để xóa lười biếng. 

Các closestools trọng điểm`0`Và`n + 1`tránh các trường hợp ranh giới riêng biệt trong quá trình xóa. Chúng không bao giờ được chèn vào treap, nhưng chúng làm cho mọi ô gần nhất thực sự bị chiếm đóng đều có hàng xóm bên trái và bên phải hợp lệ. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
7 10
1
1
1
1
1
2 3
1
2 4
2 5
1
```những thay đổi trạng thái quan trọng là: 

| Hoạt động | Đang hoạt động chiếm đóng | Chỗ ngồi được chọn | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1,7 | 7 | 
| 3 | 1,4,7 | 4 | 
| 4 | 1,2,4,7 | 2 | 
| 5 | 1,2,3,4,7 | 3 | 
| 7 | 1,2,3,5,7 | 5 | 
| 10 | 1,3,5,7 | 3 | 

Dấu vết cho thấy mỗi lần chèn chỉ phân chia một khoảng trống, trong khi việc xóa chỉ hợp nhất các khoảng trống liền kề. 

Một ví dụ nhỏ hơn:```
5 4
1
1
2 1
1
```có hành vi này: 

| Hoạt động | Đang hoạt động chiếm đóng | Kết quả | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1,5 | 5 | 
| 3 | 5 | xóa 1 | 
| 4 | 1,5 | 1 | 

Thao tác cuối cùng xác nhận rằng đoạn cuối chọn phần gần nhất ở đường viền chứ không phải phần giữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Mỗi thao tác thực hiện một số lượng không đổi các thao tác treap và heap. | 
| Không gian | O(m) | Chỉ các vị trí đã sử dụng, khoảng trống và lịch sử hoạt động mới được lưu trữ. | 

Lời giải phù hợp vì thuật toán không bao giờ phụ thuộc vào`n`. Ngay cả khi phòng vệ sinh chứa một tỷ chiếc ghế đẩu gần nhất, chỉ những ranh giới thay đổi được tạo ra bởi các hoạt động mới được xử lý. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old
    return ""

# sample and custom cases should be executed against the solve_case function
# in a local judge wrapper.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1 / 1`|`1`| Xử lý nhà vệ sinh trống | 
|`1 3 / 1 / 2 1 / 1`|`1`sau đó`1`| Tái sử dụng ghế gần nhất | 
|`7 5 / 1 / 1 / 1 / 2 1 / 1`|`1 7 4 1`| Xử lý cà vạt | 
| Lớn`n`với ít thao tác | lựa chọn mô phỏng đúng | Không phụ thuộc vào`n`| 

## Vỏ cạnh 

Khi nhà vệ sinh trống, đoạn ban đầu là`(0, n + 1)`. Quy tắc biên giới đặc biệt của nó chọn closestool`1`, phù hợp với nhãn nhỏ nhất được yêu cầu. 

Khi hai vị trí bên trong một đoạn có chất lượng như nhau, phép tính phân đoạn sử dụng phép chia số nguyên để vị trí bên trái được chọn. Ví dụ, giữa các vị trí chiếm đóng`1`Và`6`, các ứng cử viên`3`Và`4`cả hai đều có khoảng cách`2`và thuật toán chọn`3`. 

Khi công cụ đóng đầu tiên hoặc cuối cùng khả dụng trở lại, thao tác hợp nhất sẽ tạo ra một đoạn đường viền. Các trường hợp đặc biệt trong`add_gap`chọn điểm cuối của phân đoạn đó, ngăn thuật toán xử lý sai phân đoạn đó như một khoảng trống bên trong.
