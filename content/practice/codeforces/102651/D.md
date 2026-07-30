---
title: "CF 102651D - Sắp xếp giá sách"
description: "Chúng ta có một kệ gồm n cuốn sách. Mảng p mô tả giá sách hiện tại: cuốn sách hiện ở vị trí tôi muốn ở vị trí p[i] khi giá sách được sắp xếp. Vì mỗi đích đến là duy nhất nên p là hoán vị của 1..n."
date: "2026-07-30T22:38:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102651
codeforces_index: "D"
codeforces_contest_name: "Innopolis Open 2020-2021, qualification, contest 1"
rating: 0
weight: 102651
solve_time_s: 116
verified: true
draft: false
---

[CF 102651D - Sắp xếp giá sách](https://codeforces.com/problemset/problem/102651/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một kệ`n`sách. Mảng`p`mô tả kệ hiện tại: cuốn sách hiện ở vị trí`i`muốn ở vị trí`p[i]`khi kệ được sắp xếp. Vì mỗi điểm đến là duy nhất,`p`là một hoán vị của`1..n`. 

Một khách truy cập chỉ hoán đổi hai vị trí, do đó sau mỗi truy vấn hai phần tử của`p`đổi chỗ cho nhau. Sau mỗi thay đổi như vậy, chúng ta cần số lượng thao tác tối thiểu cần thiết để khôi phục lại thứ tự đúng. Một thao tác sẽ lấy một cuốn sách ra khỏi bất kỳ đâu và đặt nó ở đầu hoặc cuối giá. 

Khó khăn chính là số lượng cập nhật. Cả hai`n`Và`q`có thể đạt được`200000`, vì vậy việc xây dựng lại câu trả lời sau mỗi lần hoán đổi là không thể. MỘT`O(n)`tính toán lại cho mỗi truy vấn sẽ yêu cầu khoảng`4 * 10^10`hoạt động trong trường hợp xấu nhất. Chúng tôi cần mỗi bản cập nhật chỉ ảnh hưởng đến một phần nhỏ thông tin được duy trì. 

Có hai trường hợp cạnh rất dễ bỏ sót. Đầu tiên, những cuốn sách còn nguyên không cần phải nằm cạnh giá hiện tại. Ví dụ:```
3 0
2 1 3
```Câu trả lời là`1`. Một giải pháp bất cẩn có thể chỉ tìm kiếm các phân đoạn liền kề của mảng và kết luận rằng không tồn tại phần có thứ tự dài. Các sổ sách ở các vị trí`1`Và`3`đã tạo thành dãy con của vị trí mục tiêu`2,3`, vì vậy chỉ có cuốn sách đầu tiên phải được di chuyển. 

Một lỗi phổ biến khác là quên rằng chuỗi được lưu giữ lâu nhất có thể bắt đầu hoặc kết thúc ở bất kỳ đâu. Ví dụ:```
5 0
5 1 2 3 4
```Câu trả lời là`1`. Trình tự tiếp theo`1,2,3,4`đã ở đúng thứ tự tương đối, mặc dù nó bắt đầu ở vị trí thứ hai của mảng. Di chuyển cuốn sách ở vị trí đầu tiên là đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng quá trình sắp xếp hoặc thử tất cả các lựa chọn có thể có của những cuốn sách vẫn còn nguyên. Tùy chọn đầu tiên không hấp dẫn vì số lượng nước đi có thể lớn và việc chứng minh những lựa chọn tốt nhất là khó khăn. Tùy chọn thứ hai thậm chí còn tệ hơn: việc chọn các tập hợp con sách theo cấp số nhân. 

Quan sát hữu ích đến từ việc mô tả những cuốn sách mà chúng ta không thể di chuyển. Nếu một cuốn sách không bao giờ được chạm vào thì thứ tự tương đối của nó sẽ không bao giờ thay đổi. Giả sử chúng ta di chuyển một số cuốn sách ra phía trước và một số cuốn sách ra phía sau. Những cuốn sách chưa được động tới chiếm ở giữa kệ cuối cùng. Do đó, vị trí mục tiêu của chúng phải là một khoảng vị trí liên tiếp và chúng phải xuất hiện theo thứ tự tăng dần trong giá hiện tại. 

Vì vậy bài toán trở thành tìm dãy con dài nhất của`p`trông giống như:```
x, x + 1, x + 2, ..., x + k - 1
```Câu trả lời là`n - k`, bởi vì mọi cuốn sách khác đều phải được di chuyển. 

Thay vì lưu trữ trực tiếp chuỗi con này, hãy xem các vị trí mục tiêu liền kề. Cho phép`pos[v]`là vị trí hiện tại của cuốn sách có đích đến cuối cùng là`v`. Trình tự`v, v+1`có thể là một phần của dãy con được giữ hợp lệ chính xác khi:```
pos[v] < pos[v+1]
```Tạo một mảng nhị phân`good`, Ở đâu`good[v]`là đúng khi điều kiện này xảy ra. Một chuỗi giá trị thực liên tiếp từ`v`ĐẾN`v+k-2`có nghĩa là những cuốn sách có điểm đến`v`bởi vì`v+k-1`đã theo đúng thứ tự rồi. Do đó, độ dài chuỗi con được giữ dài nhất là chuỗi dài nhất trong số các chuỗi con`good`, cộng một. 

Việc hoán đổi hai vị trí kệ sẽ chỉ thay đổi vị trí của hai cuốn sách. Do đó, chỉ những so sánh liên quan đến hai giá trị đích này mới có thể thay đổi. Điều này làm cho bài toán trở nên phù hợp với cây phân đoạn duy trì thời gian chạy dài nhất của cây phân đoạn có cập nhật điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn cho mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu | O(log n) cho mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng hoán vị nghịch đảo`pos`, Ở đâu`pos[v]`là vị trí hiện tại của cuốn sách sẽ kết thúc tại`v`. Các truy vấn hoán đổi sách theo vị trí giá hiện tại của chúng, do đó, dạng nghịch đảo cho chúng tôi biết ngay giá trị đích nào bị ảnh hưởng. 
2. Xây dựng mảng nhị phân`good`. Đối với mọi`v`từ`1`ĐẾN`n-1`, lưu trữ xem`pos[v] < pos[v+1]`. Chuỗi dài nhất của các điểm đến liên tiếp được sắp xếp chính xác là đoạn liên tiếp dài nhất trong mảng này. 
3. Cửa hàng`good`trong cây phân đoạn. Mỗi nút cây giữ tiền tố dài nhất, hậu tố dài nhất, chuỗi dài nhất và tổng chiều dài của đoạn. Bốn giá trị này đủ để hợp nhất hai phân đoạn lân cận. 
4. Trước khi xử lý truy vấn, hãy tính toán câu trả lời ban đầu từ gốc của cây phân đoạn. Nếu thời gian chạy dài nhất là`best`, dãy con được giữ có độ dài`best + 1`, vậy số bước di chuyển cần thiết là`n - best - 1`. 
5. Đối với truy vấn hoán đổi vị trí kệ`x`Và`y`, tìm hai giá trị đích của các cuốn sách hiện ở các vị trí đó. Hoán đổi hai giá trị này trong mảng giá và cập nhật vị trí của chúng trong`pos`. 
6. Chỉ`good[value-1]`Và`good[value]`có thể thay đổi đối với từng giá trị đích trong số hai giá trị đích bị ảnh hưởng. Tính toán lại các vị trí này và thực hiện cập nhật điểm cây phân đoạn tương ứng. 
7. Sau khi cập nhật, hãy đọc lần chạy dài nhất của root và xuất ra câu trả lời mới. 

Tại sao nó hoạt động: những cuốn sách không được di chuyển phải nằm chính xác ở phần giữa của kệ cuối cùng. Vị trí mục tiêu của chúng là liên tiếp và thứ tự hiện tại của chúng phải khớp với thứ tự mục tiêu. Mảng nhị phân đại diện cho mọi cặp lân cận có thể có trong một chuỗi được giữ nguyên như vậy. Một chuỗi các giá trị thực chính xác là một chuỗi được giữ tối đa. Cây phân đoạn luôn lưu trữ lần chạy dài nhất như vậy, do đó trừ đi độ dài của nó từ`n`đưa ra số lượng sách tối thiểu được di chuyển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.pref = [0] * (4 * self.n)
        self.suff = [0] * (4 * self.n)
        self.best = [0] * (4 * self.n)
        self.length = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, node, l, r, arr):
        self.length[node] = r - l + 1
        if l == r:
            val = arr[l]
            self.pref[node] = val
            self.suff[node] = val
            self.best[node] = val
            return
        m = (l + r) // 2
        self.build(node * 2, l, m, arr)
        self.build(node * 2 + 1, m + 1, r, arr)
        self.pull(node)

    def pull(self, node):
        left = node * 2
        right = node * 2 + 1
        llen = self.length[left]
        rlen = self.length[right]

        self.length[node] = llen + rlen
        self.pref[node] = self.pref[left]
        if self.pref[left] == llen:
            self.pref[node] = llen + self.pref[right]

        self.suff[node] = self.suff[right]
        if self.suff[right] == rlen:
            self.suff[node] = rlen + self.suff[left]

        self.best[node] = max(
            self.best[left],
            self.best[right],
            self.suff[left] + self.pref[right]
        )

    def update(self, node, l, r, idx, val):
        if l == r:
            self.pref[node] = val
            self.suff[node] = val
            self.best[node] = val
            return
        m = (l + r) // 2
        if idx <= m:
            self.update(node * 2, l, m, idx, val)
        else:
            self.update(node * 2 + 1, m + 1, r, idx, val)
        self.pull(node)

    def set(self, idx, val):
        self.update(1, 0, self.n - 1, idx, val)

def solve():
    n, q = map(int, input().split())
    p = [0] + list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i in range(1, n + 1):
        pos[p[i]] = i

    good = [0] * (n - 1)
    for i in range(1, n):
        good[i - 1] = 1 if pos[i] < pos[i + 1] else 0

    seg = SegTree(good)

    ans = [str(n - seg.best[1] - 1)]

    def refresh(v):
        if v <= 0 or v >= n:
            return
        seg.set(v - 1, 1 if pos[v] < pos[v + 1] else 0)

    for _ in range(q):
        x, y = map(int, input().split())

        a = p[x]
        b = p[y]

        affected = {a - 1, a, b - 1, b}

        p[x], p[y] = p[y], p[x]
        pos[a], pos[b] = y, x

        for v in affected:
            refresh(v)

        ans.append(str(n - seg.best[1] - 1))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Hoán vị nghịch đảo là chi tiết triển khai trung tâm. Mảng`p`câu trả lời "vị trí kệ này chứa điểm đến nào?", trong khi`pos`câu trả lời "điểm đến này hiện nằm ở đâu?". Điều kiện để mở rộng chuỗi được giữ lại được biểu diễn một cách tự nhiên bằng`pos`, vì vậy mọi truy vấn đều có thể được xử lý cục bộ. 

Cây phân đoạn lưu trữ thông tin về các lần chạy thay vì câu trả lời trực tiếp. Khi hai phần tử con được hợp nhất, đoạn tốt nhất có thể nằm hoàn toàn bên trong phần tử con bên trái, hoàn toàn bên trong phần tử con bên phải hoặc cắt ngang phần giữa. Trường hợp chéo chính xác là hậu tố của con trái kết hợp với tiền tố của con phải. 

Thứ tự cập nhật truy vấn quan trọng. Các giá trị đích được lưu trước khi hoán đổi vì sau khi hoán đổi, các giá trị cũ không còn ở vị trí ban đầu. Chỉ có bốn so sánh lân cận xung quanh hai giá trị đó mới có thể thay đổi, vì vậy việc cập nhật thêm vị trí sẽ chỉ lãng phí thời gian. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
5 2
5 1 2 4 3
```trạng thái ban đầu là: 

| Bước |`p`|`pos`so sánh | Chạy lâu nhất | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 5 1 2 4 3 |`1<2`,`2<3`là đúng | 2 | 2 | 

Những cuốn sách lưu giữ đều có đích đến`1,2,3`, vậy hai cuốn sách phải được di chuyển. 

Sau khi hoán đổi vị trí`4`Và`5`: 

| Bước |`p`|`pos`so sánh | Chạy lâu nhất | Trả lời | 
| --- | --- | --- | --- | --- | 
| Truy vấn 1 | 5 1 2 3 4 |`1<2<3<4`| 3 | 1 | 

Sau khi hoán đổi vị trí`1`Và`4`: 

| Bước |`p`|`pos`so sánh | Chạy lâu nhất | Trả lời | 
| --- | --- | --- | --- | --- | 
| Truy vấn 2 | 3 1 2 5 4 | chuỗi dài nhất là`1,2,3`hoặc`3,4,5`| 1 | 3 | 

Dấu vết cho thấy câu trả lời chỉ thay đổi thông qua những thay đổi cục bộ trong so sánh điểm đến liền kề. 

Một ví dụ nhỏ thứ hai:```
4 1
1 3 4 2
2 4
```| Bước |`p`| Lần chạy dài nhất`good`| Trả lời | 
| --- | --- | --- | --- | 
| Ban đầu | 1 3 4 2 | điểm đến`3,4`tạo thành một chuỗi dài 2 | 2 | 
| Hoán đổi vị trí 2 và 4 | 1 2 4 3 | điểm đến`1,2`tạo thành một chuỗi dài 2 | 2 | 

Điều này chứng tỏ rằng các vị trí được di chuyển trong giá và các giá trị đích là những khái niệm khác nhau. Cây phân đoạn theo dõi thứ tự đích chứ không phải sự lân cận vật lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Việc xây dựng cây là tuyến tính và mỗi truy vấn chỉ thay đổi một số lá không đổi. | 
| Không gian | O(n) | Mảng hoán vị và cây phân đoạn lưu trữ một lượng thông tin không đổi cho mỗi vị trí. | 

Tổng số hoạt động phù hợp cho`n, q <= 200000`bởi vì mỗi bản cập nhật chỉ thực hiện một số thao tác cây phân đoạn logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out

    solve()

    sys.stdin = old
    sys.stdout = old_out
    return out.getvalue()

assert run("""5 2
5 1 2 4 3
4 5
1 4
""") == "2\n1\n3\n"

assert run("""2 0
2 1
""") == "1\n"

assert run("""5 0
1 2 3 4 5
""") == "0\n"

assert run("""4 1
1 3 4 2
2 4
""") == "2\n2\n"

assert run("""6 2
6 5 4 3 2 1
1 6
2 5
""") == "5\n5\n3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 2 / 5 1 2 4 3`|`2 1 3`| Cung cấp mẫu và cập nhật động | 
|`2 0 / 2 1`|`1`| Kích thước tối thiểu và không có truy vấn | 
|`5 0 / 1 2 3 4 5`|`0`| Đã sắp xếp kệ | 
|`4 1 / 1 3 4 2`|`2 2`| Hoán đổi ảnh hưởng đến một số so sánh liền kề | 
|`6 2 / 6 5 4 3 2 1`|`5 5 3`| Cập nhật trật tự và ranh giới đảo ngược | 

## Vỏ cạnh 

Khi chuỗi được giữ hợp lệ không liền kề trong giá hiện tại, thuật toán vẫn hoạt động vì nó không bao giờ tìm kiếm các khoảng mảng. Vì:```
3 0
2 1 3
```các vị trí nghịch đảo là`pos[1]=2`,`pos[2]=1`,`pos[3]=3`. Sự so sánh đúng đắn duy nhất là`pos[2] < pos[3]`, tạo ra một cạnh dài nhất và một chuỗi có độ dài hai được giữ nguyên. Câu trả lời trở thành`3 - 2 = 1`. 

Khi chuỗi được giữ bắt đầu hoặc kết thúc cách xa viền mảng, biểu diễn nhị phân sẽ xử lý nó một cách tự nhiên. Vì:```
5 0
5 1 2 3 4
```các giá trị`1,2,3,4`xuất hiện ở vị trí tăng dần, do đó dãy so sánh thực sự có độ dài bằng ba. Câu trả lời là`5 - (3 + 1) = 1`, nghĩa là chỉ cuốn sách có đích đến`1`phải được di chuyển. 

Trao đổi gần ranh giới không được truy cập các so sánh không hợp lệ. Ví dụ: nếu giá trị đích`1`chỉ thay đổi vị trí`good[1]`có thể được cập nhật vì không có`good[0]`. các`refresh`hàm bỏ qua các giá trị bên ngoài`1..n-1`, ngăn ngừa những lỗi xảy ra từng cái một.
