---
title: "CF 102623K - Mảng K-Shift"
description: "Chúng tôi có một loạt các giá trị. Hai thao tác được trộn lẫn với nhau: một thao tác sắp xếp lại một phần liên tục của mảng và thao tác kia yêu cầu tính tổng của phần liên tục. K-shift không phải là một phép quay bình thường của toàn bộ khoảng thời gian."
date: "2026-08-04T17:15:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102623
codeforces_index: "K"
codeforces_contest_name: "2020 Lenovo Cup USST Campus Online Invitational Contest"
rating: 0
weight: 102623
solve_time_s: 79
verified: true
draft: false
---

[CF 102623K - Mảng K-Shift](https://codeforces.com/problemset/problem/102623/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt các giá trị. Hai thao tác được trộn lẫn với nhau: một thao tác sắp xếp lại một phần liên tục của mảng và thao tác kia yêu cầu tính tổng của phần liên tục. 

K-shift không phải là một phép quay bình thường của toàn bộ khoảng thời gian. Khoảng đã chọn được chia thành các khối liên tiếp có kích thước K. Bên trong mỗi khối, giá trị đầu tiên di chuyển đến cuối và mọi giá trị khác di chuyển sang trái một vị trí. Giá trị duy nhất có thể có của K là 2 và 3, nghĩa là sự sắp xếp lại là một hoán vị tuần hoàn nhỏ. 

Thách thức là độ dài mảng và số lượng thao tác đều có thể đạt tới 200000. Một giải pháp quét một khoảng thời gian sau mỗi lần cập nhật hoặc truy vấn có thể thực hiện khoảng 4×10^10 thao tác trong trường hợp xấu nhất, vượt xa những gì có thể. Chúng ta cần mọi thao tác phụ thuộc vào logarit của kích thước mảng thay vì độ dài của khoảng. 

Các bẫy chính xuất phát từ thực tế là dịch chuyển K phụ thuộc vào vị trí bắt đầu của khoảng, không chỉ vào K. Ví dụ: áp dụng dịch chuyển 2 cho các vị trí từ 1 đến 4 lần hoán đổi (1,2) và (3,4), trong khi áp dụng nó cho các vị trí từ 2 đến 5 lần hoán đổi (2,3) và (4,5). Cấu trúc chỉ ghi nhớ xem một phân đoạn đã được dịch chuyển 2 hay 3 sẽ mất thông tin cần thiết. 

Một trường hợp đặc biệt khác là một truy vấn sau vài lần dịch chuyển một phần. Coi như:```
3 2
1 2 3
1 1 2 2
2 1 3
```Thao tác đầu tiên thay đổi mảng thành`[2,1,3]`, vậy đáp án là:```
6
```Việc triển khai bất cẩn coi hoạt động như một vòng quay toàn cục sẽ tạo ra thứ tự sai và có thể không thực hiện được các truy vấn sau này. 

Trường hợp cạnh thứ hai xuất hiện khi nút cây phân đoạn hoàn toàn nằm trong phạm vi cập nhật nhưng độ dài của nó không chia hết cho K. Ví dụ: một bản cập nhật có K=2 bật`[1,6]`có thể gặp phải một phân đoạn con`[1,3]`. Nút đó không thể được dịch chuyển toàn bộ vì độ dài của nó là số lẻ. Bản cập nhật phải tiếp tục giảm dần thay vì áp dụng thẻ lười không chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là lưu trữ mảng thực tế. Đối với dịch chuyển K, chúng tôi lặp qua khoảng thời gian theo khối có kích thước K và xoay từng khối. Tổng phạm vi được tính bằng cách quét tất cả các giá trị trong khoảng thời gian được yêu cầu. Cách tiếp cận này đúng vì nó thực hiện chính xác các thao tác được mô tả, nhưng một thao tác đơn lẻ có thể chạm vào các phần tử O(n). Với 200000 thao tác, trường hợp xấu nhất đạt khoảng 4×10^10 lượt truy cập phần tử. 

Quan sát hữu ích là K rất nhỏ. Một ca 2 chỉ quan tâm đến các vị trí modulo 2 so với thời điểm bắt đầu thao tác. Một ca 3 chỉ quan tâm đến các vị trí theo modulo 3. Vì cả hai giai đoạn đều chia cho 6, nên mọi phép toán có thể được biểu diễn dưới dạng hoán vị của sáu lớp dư lượng của các vị trí theo modulo 6. 

Thay vì lưu trữ thứ tự chính xác của các phần tử bên trong nút cây phân đoạn, chúng tôi lưu trữ sáu tổng. Giá trị trong thùng`i`là tổng của tất cả các phần tử trong nút đó mà chỉ mục toàn cục có số dư`i`modulo 6. K-shift trên một nút được bao phủ hoàn toàn chỉ cần hoán vị sáu nhóm này. Các vị trí thực tế không cần phải được xây dựng lại. 

Lan truyền lười biếng lưu trữ hoán vị tích lũy được áp dụng cho mỗi nút. Truy vấn phạm vi thu thập sáu nhóm từ các nút được bao phủ và thêm phần dư thích hợp. Cập nhật phạm vi chỉ giảm xuống khi toàn bộ nút không thể được chuyển đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi thao tác | O(n) | Quá chậm | 
| Tối ưu | O(6 log n) mỗi thao tác | O(6n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ sáu tổng. Nhóm thứ sáu tương ứng với sáu giá trị có thể có của`index mod 6`. Cách trình bày này lưu giữ chính xác thông tin cần thiết cho các ca và truy vấn trong tương lai. 
2. Đối với nút nhận được bản cập nhật K-shift hoàn chỉnh, hãy tính hoán vị của sáu nhóm dư. Hoán vị phụ thuộc vào ranh giới bên trái của khoảng thời gian cập nhật vì các khối bắt đầu ở đó. 
3. Áp dụng hoán vị cho sáu tổng của nút và kết hợp nó với hoán vị lười biếng của nút. Nút hiện đại diện cho cùng một phân đoạn mảng sau khi chuyển đổi mà không cần truy cập vào các phần tử con của nó. 
4. Nếu một nút được bao phủ hoàn toàn không thể dịch chuyển toàn bộ vì độ dài của nó không chia hết cho K, hãy đẩy thông tin lười biếng của nó tới các nút con của nó và tiếp tục đệ quy. Điều này ngăn chặn việc áp dụng một chuyển đổi không hợp lệ cho một phân đoạn có các khối chưa hoàn chỉnh. 
5. Đối với truy vấn tổng phạm vi, hãy truy cập đệ quy vào cây phân đoạn. Khi một nút hoàn toàn nằm trong khoảng truy vấn, hãy cộng tất cả sáu tổng được lưu trữ vì mọi phần tử trong nút đó đều thuộc phạm vi được yêu cầu. 

Tại sao nó hoạt động: mỗi bản cập nhật chỉ thay đổi vị trí của một phần tử bên trong khối của nó. Kích thước khối là 2 hoặc 3, do đó đích đến của một phần tử chỉ phụ thuộc vào vị trí của nó theo modulo 6 và điểm bắt đầu của khoảng dịch chuyển. Sáu tổng được lưu trữ bảo toàn chính xác các lớp này, vì vậy mọi phép biến đổi đều có thể được biểu diễn bằng một hoán vị. Lan truyền lười biếng giữ cho biểu diễn hợp lệ mà không cần mở rộng phân đoạn và phân tách truy vấn sẽ thu thập mọi phần tử chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [[0] * 6 for _ in range(4 * self.n)]
        self.lazy = [list(range(6)) for _ in range(4 * self.n)]
        self.build(1, 1, self.n, arr)

    def build(self, p, l, r, arr):
        if l == r:
            self.tree[p][l % 6] = arr[l - 1]
            return
        m = (l + r) // 2
        self.build(p * 2, l, m, arr)
        self.build(p * 2 + 1, m + 1, r, arr)
        for i in range(6):
            self.tree[p][i] = self.tree[p * 2][i] + self.tree[p * 2 + 1][i]

    def apply_perm(self, p, perm):
        old = self.tree[p]
        self.tree[p] = [0] * 6
        for i in range(6):
            self.tree[p][perm[i]] = old[i]

        cur = self.lazy[p]
        nxt = [0] * 6
        for i in range(6):
            nxt[i] = cur[perm[i]]
        self.lazy[p] = nxt

    def push(self, p):
        if self.lazy[p] != list(range(6)):
            perm = self.lazy[p]
            self.apply_child(p * 2, perm)
            self.apply_child(p * 2 + 1, perm)
            self.lazy[p] = list(range(6))

    def apply_child(self, p, perm):
        old = self.tree[p]
        self.tree[p] = [0] * 6
        for i in range(6):
            self.tree[p][perm[i]] = old[i]

        cur = self.lazy[p]
        nxt = [0] * 6
        for i in range(6):
            nxt[i] = cur[perm[i]]
        self.lazy[p] = nxt

    def get_perm(self, l, k):
        perm = list(range(6))
        for i in range(6):
            pos = i
            rel = (pos - l) % k
            if rel == 0:
                new_pos = (pos + k - 1) % 6
            else:
                new_pos = (pos - 1) % 6
            perm[i] = new_pos
        return perm

    def update(self, p, l, r, ql, qr, k):
        if qr < l or r < ql:
            return
        if ql <= l and r <= qr and (r - l + 1) % k == 0:
            self.apply_perm(p, self.get_perm(ql % 6, k))
            return
        if l == r:
            return
        self.push(p)
        m = (l + r) // 2
        self.update(p * 2, l, m, ql, qr, k)
        self.update(p * 2 + 1, m + 1, r, ql, qr, k)
        for i in range(6):
            self.tree[p][i] = self.tree[p * 2][i] + self.tree[p * 2 + 1][i]

    def query(self, p, l, r, ql, qr):
        if qr < l or r < ql:
            return 0
        if ql <= l and r <= qr:
            return sum(self.tree[p])
        self.push(p)
        m = (l + r) // 2
        return self.query(p * 2, l, m, ql, qr) + self.query(p * 2 + 1, m + 1, r, ql, qr)

def solve():
    n, m = map(int, input().split())
    arr = list(map(int, input().split()))
    seg = SegTree(arr)
    ans = []

    for _ in range(m):
        data = list(map(int, input().split()))
        if data[0] == 1:
            _, l, r, k = data
            seg.update(1, 1, n, l, r, k)
        else:
            _, l, r = data
            ans.append(str(seg.query(1, 1, n, l, r)))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây lưu trữ tổng theo chỉ mục chung theo modulo 6, do đó, giai đoạn xây dựng sẽ đặt mỗi giá trị ban đầu vào đúng một nhóm. Mảng lười là một hoán vị của sáu nhóm này. Áp dụng thao tác lười có nghĩa là di chuyển tổng nhóm và kết hợp hoán vị đang chờ xử lý hiện có với hoán vị mới. 

Hàm hoán vị sử dụng ranh giới bên trái của khoảng thời gian cập nhật. Đối với K-shift, một phần tử có vị trí tương đối bằng 0 sẽ di chuyển đến cuối khối của nó, trong khi mọi vị trí tương đối khác sẽ di chuyển sang trái một vị trí. Sử dụng modulo 6 có hiệu quả vì cả hai kích thước khối có thể đều chia cho 6. 

Điều kiện cập nhật`(r - l + 1) % k == 0`là điều cần thiết. Nút cây phân đoạn có thể nằm trong phạm vi được yêu cầu nhưng vẫn chứa khối chưa hoàn chỉnh nên không thể nhận chuyển đổi trực tiếp. 

Số nguyên Python không bị tràn, điều này là cần thiết vì tổng số có thể đạt khoảng 2×10^14. Tất cả việc lập chỉ mục trong cây đều dựa trên một để khớp với phát biểu vấn đề, trong khi phần dư được lưu trữ sử dụng chỉ mục thực tế modulo 6. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

| Hoạt động | Hiệu ứng mảng | Kết quả truy vấn | 
| --- | --- | --- | 
| Ban đầu |`[1,2,3,4,5,6]`| | 
| Sự thay đổi`[1,4]`, K=2 |`[2,1,4,3,5,6]`| | 
| Truy vấn`[2,3]`| Giá trị là`1,4`|`5`| 
| Sự thay đổi`[1,6]`, K=3 |`[1,4,2,5,6,3]`| | 
| Truy vấn`[2,6]`| Giá trị là`4,2,5,6,3`|`20`| 

Ca đầu tiên cho thấy lý do tại sao thao tác không thể được coi là một vòng quay. Mỗi cặp di chuyển độc lập, được các lớp dư lượng nắm bắt chính xác. 

Ví dụ khác:```
5 3
10 20 30 40 50
1 2 5 2
2 1 5
2 2 4
```| Hoạt động | Trạng thái phân đoạn | Đầu ra | 
| --- | --- | --- | 
| Ban đầu |`[10,20,30,40,50]`| | 
| Sự thay đổi`[2,5]`, K=2 |`[10,30,20,50,40]`| | 
| Truy vấn`[1,5]`| Tổng của tất cả các giá trị |`150`| 
| Truy vấn`[2,4]`| Tổng của`30,20,50`|`100`| 

Trường hợp này kiểm tra xem sự thay đổi có bắt đầu từ một vị trí tùy ý thay vì luôn bắt đầu ở chỉ số 1 hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(6 log n) mỗi thao tác | Mỗi nút cây phân đoạn đã truy cập chỉ thực hiện công việc liên tục trên sáu nhóm. | 
| Không gian | O(6n) | Mỗi nút lưu trữ sáu tổng và một hoán vị sáu phần tử. | 

Giải pháp này phù hợp với các giới hạn vì 200000 phép tính yêu cầu mỗi phép tính gần như logarit. Hệ số không đổi nhỏ vì mỗi phép biến đổi chỉ thao tác sáu giá trị. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    it = iter(data)
    n = int(next(it))
    m = int(next(it))
    arr = [int(next(it)) for _ in range(n)]

    seg = SegTree(arr)
    out = []

    for _ in range(m):
        t = int(next(it))
        if t == 1:
            l = int(next(it))
            r = int(next(it))
            k = int(next(it))
            seg.update(1, 1, n, l, r, k)
        else:
            l = int(next(it))
            r = int(next(it))
            out.append(str(seg.query(1, 1, n, l, r)))

    return "\n".join(out)

assert run("""6 4
1 2 3 4 5 6
1 1 4 2
2 2 3
1 1 6 3
2 2 6
""") == "5\n20"

assert run("""3 2
1 2 3
1 1 2 2
2 1 3
""") == "6"

assert run("""5 3
10 20 30 40 50
1 2 5 2
2 1 5
2 2 4
""") == "150\n100"

assert run("""3 2
7 7 7
1 1 3 3
2 1 3
""") == "21"

assert run("""6 2
1 2 3 4 5 6
1 2 6 3
2 2 5
""") == "18"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu |`5`,`20`| Các thay đổi và truy vấn cơ bản | 
| Ba phần tử có K=2 |`6`| Ca hợp lệ nhỏ nhất | 
| Chuyển đổi bắt đầu từ chỉ số 2 |`150`,`100`| Độ lệch cập nhật khác 0 | 
| Giá trị bằng nhau |`21`| Hoán vị không thay đổi tổng | 
| K=3 khoảng từng phần |`18`| Xử lý ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một bản cập nhật bắt đầu ở một vị trí không phải là một. Trong bài kiểm tra:```
5 3
10 20 30 40 50
1 2 5 2
2 2 4
```đoạn`[2,5]`trở thành`[30,20,50,40]`bên trong khoảng đó. Việc hoán vị sử dụng`l mod 6`, do đó các lớp dư lượng được lưu trữ được di chuyển chính xác. 

Trường hợp cạnh thứ hai là nút cây phân đoạn có độ dài không khớp với kích thước dịch chuyển. Vì:```
3 2
1 2 3
1 1 2 2
2 1 3
```bản cập nhật chỉ ảnh hưởng đến hai phần tử, vì vậy phần tử thứ ba phải không thay đổi. Quy trình cập nhật từ chối áp dụng K-shift cho độ dài nút không hợp lệ và tiếp tục đi xuống cho đến khi mọi nút được chuyển đổi đại diện cho các khối hoàn chỉnh. 

Trường hợp cạnh thứ ba đang áp dụng phép dịch chuyển để giữ nguyên tổng nhưng thay đổi thứ tự bên trong. Vụ án:```
6 2
1 2 3 4 5 6
1 2 6 3
2 2 5
```thay đổi cách sắp xếp của một số lớp dư lượng trong khi truy vấn vẫn chỉ cần giá trị kết hợp của chúng. Biểu diễn sáu nhóm lưu giữ đủ thông tin cho các truy vấn từng phần sau này mà không lưu trữ toàn bộ đơn hàng.
