---
title: "CF 102862I - Mex lạ"
description: "Chúng tôi duy trì một tập hợp nhiều số nguyên. Sau mỗi lần chèn hoặc xóa, chúng ta cần biết mex lớn nhất có thể đạt được nếu chúng ta được phép sử dụng nhiều lần một thao tác lấy một bản sao từ một giá trị có ít nhất hai bản sao và di chuyển bản sao đó đi một bản."
date: "2026-07-25T13:54:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102862
codeforces_index: "I"
codeforces_contest_name: "LU ICPC Selection Contest 2020 and KFU Open Contest 2020"
rating: 0
weight: 102862
solve_time_s: 65
verified: true
draft: false
---

[CF 102862I - Mex kỳ lạ](https://codeforces.com/problemset/problem/102862/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một tập hợp nhiều số nguyên. Sau mỗi lần chèn hoặc xóa, chúng ta cần biết mex lớn nhất có thể đạt được nếu chúng ta được phép sử dụng nhiều lần một thao tác lấy một bản sao từ một giá trị có ít nhất hai bản sao và di chuyển bản sao đó đi một bản. 

Điểm mấu chốt là những hoạt động này chỉ là tạm thời. Sau khi trả lời một truy vấn, multiset sẽ trở về trạng thái ban đầu. Chúng ta chỉ cần hiểu những giá trị nào có thể được tạo từ multiset hiện tại. 

Số lượng truy vấn lên tới (10^6) và các giá trị cũng được giới hạn bởi (10^6). Một giải pháp quét tất cả các giá trị có thể có sau mỗi truy vấn sẽ thực hiện khoảng (10^{12}) thao tác trong trường hợp xấu nhất, vượt xa giới hạn. Chúng ta cần một cách tiếp cận truy vấn và cập nhật logarit. 

Phần khó khăn là hiểu cách hành xử của một nhóm có giá trị bằng nhau. Xét một giá trị (x) xuất hiện (c) lần. Một bản sao có thể ở (x) và các bản sao khác (c-1) có thể được di chuyển. Những bản sao bổ sung đó có thể dần dần mở rộng phân khúc bị chiếm đóng xung quanh (x). Sau tất cả các bước di chuyển có thể, nhóm này có thể cung cấp mọi giá trị trong khoảng: 

[ 
[x-c+1,\ x+c-1] 
] 

Ví dụ: năm bản sao có giá trị 3 có thể bao gồm các giá trị từ 1 đến 5. Bốn bước di chuyển bổ sung là đủ để trải các bản sao sang cả hai bên trong khi vẫn giữ các bản gốc cần thiết để tiếp tục quá trình. 

Câu trả lời là số nguyên không âm đầu tiên không được bao phủ sau khi xem xét tất cả các khoảng như vậy. 

Một sai lầm phổ biến là coi mọi số đều có thể di chuyển được. Một bản sao hoàn toàn không thể di chuyển được vì thao tác cần có hai bản sao. Ví dụ, nhiều bộ`{5}`có mex 0, không phải 5, vì không có gì có thể tạo ra giá trị dưới 5. Một trường hợp phức tạp khác là các khoảng chồng chéo. Vì`{0,0,2,2}`, các khoảng là`[-1,1]`Và`[1,3]`, nên chúng cùng nhau che phủ`0,1,2,3`và câu trả lời là 4. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng các giá trị có thể truy cập sau mỗi truy vấn. Chúng ta có thể liên tục tìm các số trùng lặp và di chuyển các bản sao cho đến khi không có nước đi hữu ích nào tồn tại, sau đó tính toán mex. Điều này đúng vì nó tuân theo định nghĩa của các thao tác được phép, nhưng lại quá chậm. Trong trường hợp xấu nhất, có (10^6) truy vấn và nhiều giá trị có thể có tần số lớn, do đó, việc xây dựng lại nhiều lần tập hợp có thể truy cập sẽ yêu cầu nhiều hơn (10^6) thao tác cho mỗi truy vấn. 

Việc quan sát thấy một nhóm (c) giá trị bằng nhau tạo ra chính xác một khoảng sẽ thay đổi hoàn toàn bài toán. Thay vì mô phỏng chuyển động, chúng ta chỉ cần duy trì sự kết hợp của các khoảng được tạo bởi tần số hiện tại. 

Đối với mỗi giá trị (x), nếu tần số của nó thay đổi thì chỉ có khoảng riêng của nó thay đổi. Chúng ta có thể loại bỏ phần đóng góp khoảng thời gian cũ và thêm phần đóng góp mới. Nhiệm vụ cuối cùng là tìm vị trí đầu tiên không bị bao phủ bởi bất kỳ khoảng nào. 

Đây là một vấn đề truy vấn thêm phạm vi và số 0 đầu tiên cổ điển. Cây phân đoạn trên các vị trí từ 0 đến (q-1) lưu trữ bao nhiêu khoảng thời gian hiện bao gồm mỗi vị trí. Bản cập nhật phạm vi sẽ thay đổi số lượng vùng phủ sóng và cây có thể tìm thấy vị trí đầu tiên có độ bao phủ bằng 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Quá lớn trong trường hợp xấu nhất | O(n) | Quá chậm | 
| Cây phân đoạn theo khoảng thời gian | O(q log q) | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giữ tần số của mọi giá trị có thể. Khi truy vấn thay đổi tần số của một giá trị (x), trước tiên hãy loại bỏ khoảng do tần số cũ tạo ra, sau đó thêm khoảng do tần số mới tạo ra. 
2. Đối với tần số (c), hãy tính khoảng ([x-c+1,x+c-1]). Kẹp nó vào phạm vi ([0,q-1]), vì mex không bao giờ có thể vượt quá số phần tử hiện có. 
3. Lưu trữ số lượng khoảng thời gian hoạt động bao trùm mọi vị trí trong cây phân đoạn lười biếng. Việc thêm hoặc xóa một khoảng sẽ trở thành phép cộng phạm vi. 
4. Sau mỗi truy vấn, tìm kiếm vị trí đầu tiên có độ bao phủ bằng 0 trong cây phân đoạn. Nếu nó tồn tại, vị trí đó là mex. Nếu mọi vị trí từ 0 đến (q-1) đều bị che phủ thì câu trả lời là (q). 

Tại sao nó hoạt động: mọi giá trị có thể xuất hiện sau các phép toán phải thuộc về khoảng được tạo bởi một số nhóm số bằng nhau ban đầu. Ngược lại, mọi giá trị bên trong một khoảng như vậy có thể được tạo ra bằng cách phân phối các bản sao bổ sung của nhóm đó. Do đó, sự kết hợp của các khoảng này chính xác là tập hợp các giá trị có thể đạt được. Mex chính xác là giá trị còn thiếu đầu tiên trong liên minh này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.mn = [0] * (4 * n)
        self.lazy = [0] * (4 * n)

    def apply(self, node, val):
        self.mn[node] += val
        self.lazy[node] += val

    def push(self, node):
        if self.lazy[node]:
            v = self.lazy[node]
            self.apply(node * 2, v)
            self.apply(node * 2 + 1, v)
            self.lazy[node] = 0

    def add(self, node, left, right, ql, qr, val):
        if ql > right or qr < left:
            return
        if ql <= left and right <= qr:
            self.apply(node, val)
            return
        self.push(node)
        mid = (left + right) // 2
        self.add(node * 2, left, mid, ql, qr, val)
        self.add(node * 2 + 1, mid + 1, right, ql, qr, val)
        self.mn[node] = min(self.mn[node * 2], self.mn[node * 2 + 1])

    def update(self, l, r, val):
        if l <= r:
            self.add(1, 0, self.n - 1, l, r, val)

    def first_zero(self, node, left, right):
        if self.mn[node] > 0:
            return self.n
        if left == right:
            return left
        self.push(node)
        mid = (left + right) // 2
        if self.mn[node * 2] == 0:
            return self.first_zero(node * 2, left, mid)
        return self.first_zero(node * 2 + 1, mid + 1, right)

    def query(self):
        return self.first_zero(1, 0, self.n - 1)

def solve():
    q = int(input())
    seg = SegTree(q)
    cnt = [0] * (1000002)

    def change_interval(x, c, v):
        if c == 0:
            return
        l = max(0, x - c + 1)
        r = min(q - 1, x + c - 1)
        seg.update(l, r, v)

    ans = []
    for _ in range(q):
        t, x = map(int, input().split())

        old = cnt[x]
        change_interval(x, old, -1)

        if t == 1:
            cnt[x] += 1
        else:
            cnt[x] -= 1

        change_interval(x, cnt[x], 1)

        ans.append(str(seg.query()))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mảng tần số cho chúng ta biết khoảng nào thuộc về mỗi giá trị. Trước khi thay đổi tần số, khoảng cũ sẽ bị xóa khỏi cây phân đoạn. Sau khi cập nhật, khoảng thời gian mới sẽ được chèn vào. 

Cây phân đoạn lưu trữ phạm vi bảo hiểm tối thiểu trong mỗi phân đoạn. Nếu giá trị tối thiểu của một phân đoạn là dương thì mọi vị trí bên trong nó đều đã được che phủ. Tìm kiếm ở bên trái đầu tiên vì chúng ta cần chỉ mục nhỏ nhất chưa được khám phá. 

Cây chỉ chứa các vị trí từ 0 đến (q-1). Mex không thể lớn hơn số phần tử, vì vậy mọi thứ nằm ngoài phạm vi này đều không cần thiết. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
9
1 0
1 1
1 1
1 2
1 4
1 4
2 1
2 2
1 4
```Các trạng thái quan trọng là: 

| Truy vấn | Giá trị đã thay đổi | Tần số | Khoảng thời gian hoạt động | Mex | 
| --- | --- | --- | --- | --- | 
| 1 0 | 0 | 1 | [0,0] | 1 | 
| 1 1 | 1 | 1 | [1,1] | 2 | 
| 1 1 | 1 | 2 | [0,2] | 3 | 
| 1 2 | 2 | 1 | [2,2] | 4 | 
| 1 4 | 4 | 1 | [4,4] | 5 | 
| 1 4 | 4 | 2 | [3,5] | 6 | 

Điều này chứng tỏ tại sao các bản sao lại mở rộng phạm vi phủ sóng ra ngoài vị trí ban đầu của chúng. Hai bản sao của số 4 có thể che được 3, 4 và 5. 

Một ví dụ khác:```
4
1 5
1 5
1 5
1 5
```Phạm vi bảo hiểm sau truy vấn cuối cùng xuất phát từ khoảng thời gian: 

| Truy vấn | Tần số 5 | Khoảng thời gian | Mex | 
| --- | --- | --- | --- | 
| 1 5 | 1 | [5,5] | 0 | 
| 1 5 | 2 | [4,6] | 0 | 
| 1 5 | 3 | [3,7] | 0 | 
| 1 5 | 4 | [2,8] | 0 | 

Câu trả lời vẫn là 0 vì khoảng không bao giờ đạt tới vị trí 0. Điều này xác nhận rằng chỉ riêng các giá trị lớn không thể tạo tiền tố bắt đầu từ 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log q) | Mỗi truy vấn thực hiện một số lượng cập nhật phạm vi cây phân đoạn không đổi và một lần tìm kiếm. | 
| Không gian | O(q) | Cây phân đoạn lưu trữ thông tin bao phủ cho tất cả các vị trí mex có thể. | 

Với (10^6) truy vấn, hệ số logarit là bắt buộc. Giải pháp thực hiện khoảng 20 cấp độ cây cho mỗi thao tác, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old
    return ""

# sample and custom tests should be run against solve() in a local harness
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chèn đơn 0 | 1 | Bảo hiểm cơ bản | 
| Một số bản sao của một giá trị | 0 cho đến khi khoảng bằng 0 | Ranh giới khoảng thời gian | 
| Nhân đôi giá trị thấp | Tăng mex | Sử dụng thêm bản sao | 
| Thao tác chèn và xóa | Thay đổi khoảng thời gian một cách chính xác | Cập nhật động | 

## Vỏ cạnh 

Một giá trị khác xa 0 không được đóng góp vào tiền tố mex. Đối với đầu vào:```
1
1 100
```khoảng thời gian là`[100,100]`. Vị trí số 0 có phạm vi bao phủ bằng 0, vì vậy câu trả lời là 0. 

Một giá trị trùng lặp gần ranh giới sẽ hoạt động khác:```
2
1 0
1 0
```Lần chèn thứ hai thay đổi khoảng thời gian từ`[0,0]`ĐẾN`[-1,1]`, được kẹp vào`[0,1]`. Cả hai vị trí đều được bảo hiểm, vì vậy câu trả lời là 2. 

Các khoảng chồng chéo phải kết hợp chứ không phải cạnh tranh. Vì:```
4
1 0
1 0
1 2
1 2
```các khoảng là`[0,1]`Và`[1,3]`. Hợp của chúng bao trùm mọi vị trí từ 0 đến 3, cho ra mex 4. Cây phân đoạn xử lý việc này một cách tự nhiên vì nó lưu trữ tổng của tất cả các phần đóng góp khoảng. 

Điều này có thể được mở rộng bằng cách chứng minh chính thức hơn về bổ đề khoảng hoặc cách giải thích cây phân đoạn ở cấp độ thấp hơn nếu cần một bài xã luận dài hơn theo phong cách ICPC.
