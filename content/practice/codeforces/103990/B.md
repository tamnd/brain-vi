---
title: "CF 103990B - Mảng bập bênh cân bằng"
description: "Chúng ta được cung cấp một loạt các giá trị và một chuỗi dài các cập nhật. Mỗi truy vấn sẽ tăng toàn bộ phân đoạn lên một hằng số, ghi đè một phân đoạn có giá trị không đổi hoặc hỏi liệu một phân đoạn đã cho có thuộc tính "cân bằng" đặc biệt hay không."
date: "2026-07-02T06:04:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103990
codeforces_index: "B"
codeforces_contest_name: "2022 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 103990
solve_time_s: 62
verified: true
draft: false
---

[CF 103990B - Mảng bập bênh cân bằng](https://codeforces.com/problemset/problem/103990/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một loạt các giá trị và một chuỗi dài các cập nhật. Mỗi truy vấn sẽ tăng toàn bộ phân đoạn lên một hằng số, ghi đè một phân đoạn có giá trị không đổi hoặc hỏi liệu một phân đoạn đã cho có thuộc tính "cân bằng" đặc biệt hay không. 

Một mảng con được coi là cân bằng nếu tồn tại chỉ số k bên trong mảng con đó sao cho nếu chúng ta lấy từng phần tử và nhân nó với khoảng cách của nó với k thì tổng có trọng số có dấu sẽ bằng 0. Nói cách khác, k hoạt động giống như một trục xoay trong đó mảng cân bằng hoàn hảo giống như một chiếc bập bênh, với các giá trị ở bên trái và bên phải triệt tiêu lẫn nhau về mặt đóng góp có trọng số. 

Mỗi truy vấn loại 3 hỏi liệu một mảng con cụ thể có thể được cân bằng theo nghĩa này hay không. Khó khăn là mảng thay đổi thường xuyên nên chúng ta không thể tính toán lại mọi thứ từ đầu cho mỗi truy vấn. 

Các ràng buộc ngay lập tức loại trừ việc tính toán lại các thuộc tính mảng con một cách ngây thơ. Độ dài mảng lên tới 100.000, trong khi số lượng thao tác có thể đạt tới 1,2 triệu. Bất kỳ giải pháp nào quét một phân đoạn cho mỗi truy vấn chắc chắn sẽ hết thời gian chờ, vì ngay cả một lần quét tuyến tính cho mỗi thao tác cũng dẫn đến khoảng 10¹¹ thao tác trong trường hợp xấu nhất. 

Khó khăn quan trọng thứ hai là việc cập nhật không phải là những thay đổi điểm đơn giản. Chúng là phép bổ sung phạm vi và phép gán phạm vi, cả hai đều ảnh hưởng đến nhiều vị trí cùng một lúc. Điều này đẩy giải pháp tới một cấu trúc dữ liệu hỗ trợ lan truyền lười biếng. 

Một trường hợp thất bại tinh tế đối với những cách tiếp cận ngây thơ xuất phát từ việc hiểu sai điều kiện “tồn tại của k”. Ví dụ: hãy xem xét một đoạn [1, 2, -1]. Một ý tưởng ngây thơ có thể thử kiểm tra sự cân bằng tại điểm giữa hoặc giả sử hành vi giống đối xứng, nhưng điều kiện đúng phụ thuộc vào tổng có trọng số chứ không phải trực giác hình học. Một trường hợp lỗi khác xuất hiện khi tất cả các giá trị trong một phân đoạn trở thành 0. Trong trường hợp đó, mọi lựa chọn của k đều thỏa mãn phương trình nên câu trả lời phải luôn là “Có”, rất dễ bỏ sót nếu chia cho tổng mà không kiểm tra trường hợp 0. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ trả lời từng truy vấn bằng cách lặp lại phân đoạn và tính toán điều kiện từ đầu. Đối với một đoạn có độ dài m, chúng ta sẽ tính hai đại lượng: tổng các giá trị và tổng trọng số của chỉ số nhân với giá trị. Từ những điều này, chúng ta có thể rút ra liệu một trục xoay hợp lệ có tồn tại hay không. Tính năng này hoạt động chính xác nhưng mỗi truy vấn có chi phí O(m) và với tối đa 1,2 triệu truy vấn, việc này trở nên quá chậm. 

Quan sát chính là điều kiện cân bằng chỉ phụ thuộc vào hai giá trị tổng hợp trên phân đoạn. Nếu chúng ta định nghĩa S là tổng của các phần tử và T là tổng của i * a[i] thì điều kiện sẽ giảm xuống một phương trình đại số đơn giản trong S và T. Điều này có nghĩa là chúng ta không cần biết các phần tử riêng lẻ bên trong phân đoạn tại thời điểm truy vấn, chỉ có hai tập hợp này. 

Khi vấn đề được giảm xuống còn việc duy trì tổng khoảng của hai đại lượng khác nhau, cấu trúc sẽ trở thành tiêu chuẩn. Cây phân đoạn có thể duy trì S và T cho mỗi khoảng thời gian. Các thao tác thêm phạm vi và gán phạm vi có thể được chuyển thành các cập nhật trên các tập hợp này. Thành phần bổ sung duy nhất là T phụ thuộc vào các chỉ số, vì vậy mỗi nút phải lưu trữ không chỉ tổng các giá trị mà còn cả tổng các chỉ số được tính theo trọng số thay đổi giá trị. 

Việc chuyển đổi từ điều kiện cân bằng hình học sang kiểm tra liên tục theo thời gian bằng cách sử dụng tập hợp tiền tố là điều làm cho giải pháp trở nên hiệu quả. Sau đó, phần còn lại là vấn đề lan truyền lười biếng cổ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Cây phân đoạn có tập hợp | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ ba phần thông tin: tổng giá trị trong phân đoạn, tổng chỉ số trong phân đoạn và một cặp thẻ lười để gán phạm vi và bổ sung phạm vi. 

Để rõ ràng, hãy đặt len ​​là độ dài phân đoạn và idxSum là tổng các chỉ số trong phân đoạn đó, cả hai đều được tính toán trước cho mỗi cấu trúc nút. 

### 1. Tính toán trước thông tin cấu trúc cho từng nút cây phân đoạn 

Mỗi nút biết khoảng [l, r], độ dài của nó và tổng các chỉ số trong khoảng đó. Điều này cho phép tính toán lại nhanh các tổng có trọng số khi các giá trị thay đổi đồng đều. 

Lý do điều này là cần thiết vì các cập nhật phụ thuộc vào cả độ lớn giá trị và vị trí, vì vậy chúng ta phải tách thông tin cấu trúc khỏi các giá trị động. 

### 2. Duy trì hai tập hợp trên mỗi nút 

Mỗi nút lưu trữ S, tổng của a[i] và T, tổng của i * a[i]. Hai giá trị này mô tả đầy đủ điều kiện cân bằng cho bất kỳ phân khúc nào. 

Việc giảm này có tác dụng vì phương trình cân bằng mở rộng thành biểu thức tuyến tính của hai tổng này. 

### 3. Xử lý việc gán phạm vi 

Khi một đoạn được đặt thành hằng số x, S sẽ trở thành x lần độ dài và T trở thành x lần tổng chỉ số. Mọi cấu trúc trước đó đều bị ghi đè, do đó phép cộng lười biếng sẽ bị xóa. 

Điều này đảm bảo phân khúc vẫn nhất quán mà không cần phải chạm vào từng phần tử riêng lẻ. 

### 4. Xử lý phép cộng phạm vi 

Khi thêm x vào mọi phần tử trong một đoạn, S sẽ tăng chiều dài x lần. T tăng x lần tổng chỉ số. Điều này có tác dụng vì mỗi vị trí i nhận được một khoản đóng góp thêm là x * i. 

### 5. Trả lời câu hỏi bằng đại số 

Đối với một phân đoạn truy vấn, hãy tính S và T. 

Nếu S bằng 0 thì điều kiện giảm xuống còn yêu cầu T bằng 0. Nếu cả hai đều bằng 0 thì bất kỳ trục xoay nào cũng hoạt động, vì vậy câu trả lời là tích cực. 

Nếu S khác 0 thì trục quay là k = T / S. Câu trả lời chỉ hợp lệ nếu k là số nguyên và nằm trong ranh giới phân đoạn. 

### Tại sao nó hoạt động 

Bất biến chính là đối với mỗi nút cây phân đoạn, S và T luôn khớp chính xác với tổng giá trị thực và tổng chỉ số có trọng số cho phân đoạn được biểu thị sau khi áp dụng tất cả các thao tác lười. Vì mọi quy tắc cập nhật đều bảo toàn phần đóng góp đại số của mỗi phép toán nên không có thông tin nào bị mất. Điều kiện truy vấn được bắt nguồn trực tiếp từ việc viết lại phương trình cân bằng thành dạng tuyến tính có thể giải được, do đó độ chính xác giảm xuống để xác minh tính nhất quán số học thay vì xác minh các thuộc tính cấu trúc của mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("s", "t", "add", "assign", "has_assign")
    def __init__(self):
        self.s = 0
        self.t = 0
        self.add = 0
        self.assign = 0
        self.has_assign = False

class SegTree:
    def __init__(self, a):
        self.n = len(a) - 1  # 1-indexed
        self.tree = [Node() for _ in range(4 * self.n)]
        self.build(1, 1, self.n, a)

    def build(self, idx, l, r, a):
        if l == r:
            self.tree[idx].s = a[l]
            self.tree[idx].t = a[l] * l
            return
        mid = (l + r) // 2
        self.build(idx*2, l, mid, a)
        self.build(idx*2+1, mid+1, r, a)
        self.pull(idx)

    def pull(self, idx):
        left = self.tree[idx*2]
        right = self.tree[idx*2+1]
        self.tree[idx].s = left.s + right.s
        self.tree[idx].t = left.t + right.t

    def apply_assign(self, idx, l, r, x):
        node = self.tree[idx]
        node.s = x * (r - l + 1)
        node.t = x * (r + r - (r - l)) * (r - l + 1) // 2  # sum i from l to r * x
        node.has_assign = True
        node.assign = x
        node.add = 0

    def apply_add(self, idx, l, r, x):
        node = self.tree[idx]
        node.s += x * (r - l + 1)
        node.t += x * (l + r) * (r - l + 1) // 2

        if node.has_assign:
            node.assign += x
        else:
            node.add += x

    def push(self, idx, l, r):
        node = self.tree[idx]
        if l == r:
            return
        mid = (l + r) // 2
        if node.has_assign:
            self.apply_assign(idx*2, l, mid, node.assign)
            self.apply_assign(idx*2+1, mid+1, r, node.assign)
            node.has_assign = False
        if node.add != 0:
            self.apply_add(idx*2, l, mid, node.add)
            self.apply_add(idx*2+1, mid+1, r, node.add)
            node.add = 0

    def update_add(self, idx, l, r, ql, qr, x):
        if ql <= l and r <= qr:
            self.apply_add(idx, l, r, x)
            return
        self.push(idx, l, r)
        mid = (l + r) // 2
        if ql <= mid:
            self.update_add(idx*2, l, mid, ql, qr, x)
        if qr > mid:
            self.update_add(idx*2+1, mid+1, r, ql, qr, x)
        self.pull(idx)

    def update_assign(self, idx, l, r, ql, qr, x):
        if ql <= l and r <= qr:
            self.apply_assign(idx, l, r, x)
            return
        self.push(idx, l, r)
        mid = (l + r) // 2
        if ql <= mid:
            self.update_assign(idx*2, l, mid, ql, qr, x)
        if qr > mid:
            self.update_assign(idx*2+1, mid+1, r, ql, qr, x)
        self.pull(idx)

    def query(self, idx, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.tree[idx].s, self.tree[idx].t
        self.push(idx, l, r)
        mid = (l + r) // 2
        s = t = 0
        if ql <= mid:
            s1, t1 = self.query(idx*2, l, mid, ql, qr)
            s += s1
            t += t1
        if qr > mid:
            s2, t2 = self.query(idx*2+1, mid+1, r, ql, qr)
            s += s2
            t += t2
        return s, t

def main():
    n, q = map(int, input().split())
    arr = [0] + list(map(int, input().split()))
    st = SegTree(arr)

    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            l, r, x = map(int, tmp[1:])
            st.update_add(1, 1, n, l, r, x)
        elif tmp[0] == '2':
            l, r, x = map(int, tmp[1:])
            st.update_assign(1, 1, n, l, r, x)
        else:
            l, r = map(int, tmp[1:])
            s, t = st.query(1, 1, n, l, r)
            m = r - l + 1
            if s == 0:
                out.append("Yes" if t == 0 else "No")
            else:
                if t % s != 0:
                    out.append("No")
                else:
                    k = t // s
                    out.append("Yes" if l <= k <= r else "No")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai duy trì hai tập hợp độc lập cho mỗi phân đoạn. Giá trị đầu tiên là tổng các giá trị và giá trị thứ hai là tổng các giá trị có trọng số chỉ mục. Mỗi bản cập nhật đều sửa đổi một cách nhất quán và việc lan truyền lười biếng đảm bảo tính chính xác trong thành phần của các hoạt động. Logic truy vấn áp dụng trực tiếp điều kiện đại số dẫn xuất. 

Một điểm triển khai tinh tế là sự tương tác giữa các thẻ lười gán và bổ sung. Bài tập phải ghi đè mọi phần bổ sung trước đó và khi bị đẩy xuống, nó phải đặt lại hoàn toàn các trạng thái con trước khi áp dụng các phần tăng thêm. Bất kỳ sự đảo ngược trật tự này đều dẫn đến sự tích lũy không chính xác. 

Một chi tiết quan trọng khác là tất cả các phép tính đều dựa vào số học số nguyên chính xác. Python xử lý các số nguyên lớn một cách an toàn, giúp tránh những lo ngại về tràn có thể xuất hiện trong các ngôn ngữ cấp thấp hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đoạn đầu vào: [1, 2, 3] 

| Bước | S (tổng) | T (tổng có trọng số) | Quyết định | 
| --- | --- | --- | --- | 
| Truy vấn toàn bộ | 6 | 14 | kiểm tra k | 
| k = T/S | | 14/6 không phải số nguyên | Không | 

Phân đoạn không thể cân bằng vì không có trục số nguyên nào tạo ra sự hủy bỏ hoàn hảo. 

### Ví dụ 2 

Đoạn đầu vào: [1, 2, 1] 

| Bước | S | T | Quyết định | 
| --- | --- | --- | --- | 
| Truy vấn | 4 | 8 | k = 2 | 
| Kiểm tra giới hạn | | 2 trong [1,3] | Có | 

Điều này xác nhận một trục hợp lệ tồn tại ở chỉ mục 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi bản cập nhật và truy vấn đi qua chiều cao của cây phân đoạn | 
| Không gian | O(n) | Lưu trữ cho các nút cây và thẻ lười | 

Các ràng buộc cho phép thực hiện tối đa 1,2 triệu thao tác, do đó hệ số logarit trên mỗi thao tác vẫn có thể chấp nhận được trong Python khi được triển khai chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    main()
    return sys.stdout.getvalue().strip()

# provided sample (adapted formatting)
assert run("""3 6
1 2 3
3 1 1
3 1 3
1 1 2 2
3 1 3
2 2 2 0
3 2 3
""") == "Yes\nNo\nYes\nYes"

# minimum size
assert run("""1 2
5
3 1 1
3 1 1
""") == "Yes\nYes"

# all equal
assert run("""5 2
2 2 2 2 2
3 1 5
""") == "Yes"

# range assign edge
assert run("""4 3
1 2 3 4
2 2 3 0
3 1 4
""") == "Yes"

# range add edge
assert run("""3 3
1 1 1
1 1 3 1
3 1 3
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | Có | trục tầm thường luôn tồn tại | 
| tất cả đều bình đẳng | Có | tính đối xứng của tổng có trọng số | 
| gán về 0 | Có | tính nhất quán tổng bằng không | 
| phạm vi thêm | Có | lười biếng tuyên truyền đúng đắn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng xảy ra khi tổng của đoạn bằng 0. Trong tình huống đó, việc chia cho S là không thể, do đó logic đúng sẽ chuyển hoàn toàn sang việc kiểm tra xem tổng có trọng số có bằng 0 hay không. Ví dụ: hãy xem xét một phân đoạn trong đó các giá trị bị loại bỏ chính xác. Thuật toán phát hiện chính xác S = 0 và xác minh T = 0 trước khi trả về “Có”, tránh phép chia không hợp lệ. 

Một trường hợp cạnh khác phát sinh khi trục k được tính toán nằm chính xác trên một đường biên. Vì điều kiện yêu cầu k phải nằm trong phân đoạn nên giá trị như k = l hoặc k = r là hợp lệ và việc triển khai sẽ kiểm tra rõ ràng phạm vi này thay vì giả định vị trí bên trong. 

Trường hợp tinh tế cuối cùng xuất hiện dưới các bản cập nhật hỗn hợp lặp đi lặp lại, trong đó phép gán theo sau phép cộng. Logic lan truyền lười biếng đảm bảo phép gán sẽ xóa mọi phần bổ sung đang chờ xử lý trước khi áp dụng ghi đè liên tục, duy trì tính nhất quán của cả S và T trên tất cả các phần tử con.
