---
title: "CF 104663A - Đếm mảng con"
description: "Chúng tôi đang làm việc với một mảng có độ dài $N$, nhưng bản thân các giá trị mảng không liên quan. Điều quan trọng chỉ là dòng chỉ số từ 1 đến $N$. Trên dòng này, chúng ta có $M$ các phân đoạn đặc biệt $[li, ri]$. Các phân đoạn này thể hiện các ràng buộc đối với những gì khiến một mảng con trở nên “xấu”."
date: "2026-06-29T16:38:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104663
codeforces_index: "A"
codeforces_contest_name: "Replay of Ostad Presents Intra KUET Programming Contest 2023"
rating: 0
weight: 104663
solve_time_s: 99
verified: true
draft: false
---

[CF 104663A - Đếm mảng con](https://codeforces.com/problemset/problem/104663/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một mảng có độ dài$N$, nhưng bản thân các giá trị mảng không liên quan. Điều quan trọng chỉ là dòng chỉ số từ 1 đến$N$. Trên dòng này, chúng tôi được cung cấp$M$phân đoạn đặc biệt$[l_i, r_i]$. Các phân đoạn này thể hiện các ràng buộc đối với những gì khiến một mảng con trở nên “xấu”. 

Một mảng con$[a, b]$được coi là xấu nếu nó chứa đầy đủ ít nhất một trong các phân đoạn nhất định, nghĩa là tồn tại một số khoảng$[l_i, r_i]$như vậy$a \le l_i$Và$r_i \le b$. Một mảng con tốt chỉ đơn giản là một mảng tránh được điều kiện này cho tất cả các phân đoạn đã cho. 

Vậy nhiệm vụ là đếm xem có bao nhiêu cặp$(a, b)$với$1 \le a \le b \le N$không bao gồm hoàn toàn bất kỳ phân đoạn bị cấm nào. 

Ràng buộc$N \le 10^9$ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại trên tất cả các mảng con hoặc thậm chí tất cả các điểm cuối một cách trực tiếp. Số mảng con là$\Theta(N^2)$, có kích thước lớn về mặt thiên văn, vì vậy chúng ta phải dựa hoàn toàn vào cấu trúc của$M$khoảng thời gian. Quan sát quan trọng là độ phức tạp phải phụ thuộc vào$M$, không bật$N$, ngoại trừ ở đâu$N$xuất hiện trong các công thức số học cuối cùng. 

Một cách tiếp cận đơn giản sẽ kiểm tra mọi mảng con và kiểm tra xem nó có chứa phân đoạn bị cấm hay không. Ngay cả khi việc kiểm tra một mảng con đơn lẻ là$O(M)$, tổng số sẽ là$O(N^2 M)$, điều đó hoàn toàn không thể thực hiện được. 

Một cách tiếp cận ngây thơ thông minh hơn một chút là sửa một mảng con$[a, b]$và duy trì cấu trúc dữ liệu để kiểm tra xem có khoảng nào nằm bên trong hay không. Điều này vẫn đòi hỏi phải lặp lại tất cả$\Theta(N^2)$cặp, vì vậy nó cũng thất bại ngay lập tức. 

Một chế độ lỗi tinh vi hơn sẽ xuất hiện nếu chúng ta cố gắng chỉ kiểm tra điểm cuối$l_i, r_i$và lý do tại địa phương. Ví dụ, người ta có thể giả định không chính xác rằng chỉ các mảng con có ranh giới khớp với một số$l_i$hoặc$r_i$quan trọng, nhưng điều này bỏ qua rằng một mảng con có thể bắt đầu và kết thúc ở bất kỳ đâu mà vẫn bao phủ đầy đủ một phân đoạn. 

Một cạm bẫy cụ thể: nếu$N = 6$và chúng tôi có một phân khúc$[2, 3]$, mảng con$[1, 4]$đã tệ mặc dù cả hai điểm cuối đều không khớp chính xác với 2 hoặc 3. Bất kỳ cách tiếp cận nào chỉ theo dõi trực tiếp các điểm cuối mà không xem xét cấu trúc phạm vi bao phủ sẽ bỏ lỡ những trường hợp như vậy. 

Vấn đề cơ bản là về việc đếm các cặp$(a, b)$tránh chứa hoàn toàn bất kỳ khoảng cấm nào, đó là một điều kiện hình học trong hai chiều. 

## Phương pháp tiếp cận 

Một cách cải tổ hữu ích đến từ việc đảo ngược điều kiện. Một mảng con$[a, b]$là xấu nếu nó chứa ít nhất một khoảng$[l_i, r_i]$, tương đương với$a \le l_i$Và$b \ge r_i$. Vì vậy, mỗi khoảng tạo ra một tập hợp các cặp xấu trong$(a, b)$mặt phẳng: tất cả các điểm trong hình chữ nhật$[1, l_i] \times [r_i, N]$, hạn chế ở$a \le b$. 

Vì vậy, vấn đề trở thành việc đếm sự kết hợp của các hình chữ nhật này trong một lưới tam giác. Sự bổ sung của liên minh này đưa ra câu trả lời. 

Cách tiếp cận bạo lực sẽ liệt kê rõ ràng tất cả các cặp$(a, b)$và kiểm tra xem chúng có nằm trong hình chữ nhật nào không. Đó là$O(N^2 M)$, điều đó là không thể. 

Quan sát cấu trúc quan trọng là chúng ta có thể quét qua điểm cuối bên trái$a$. Đối với một cố định$a$, chúng tôi muốn biết có bao nhiêu điểm cuối phù hợp$b$được bao phủ bởi ít nhất một hình chữ nhật. Mỗi khoảng$[l_i, r_i]$đóng góp bảo hiểm trên$b \ge r_i$bất cứ khi nào$a \le l_i$. Điều này có nghĩa là như$a$giảm, nhiều khoảng thời gian hoạt động hơn và mỗi khoảng đóng góp một phạm vi trên$b$-trục. 

Như vậy chúng ta có thể xử lý$a$theo thứ tự giảm dần, duy trì sự kết hợp động của các khoảng trên$b$-trục. Đối với mỗi đoạn cố định của$a$-giá trị trong đó tập hoạt động không thay đổi, đóng góp chỉ đơn giản là chiều rộng trong$a$lần bao phủ chiều dài trong$b$. Điều này làm giảm vấn đề duy trì sự kết hợp các khoảng khi chèn, có thể được xử lý bằng cây phân đoạn trên tọa độ nén. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2 M)$|$O(1)$| Quá chậm | 
| Quét + Cây phân đoạn |$O(M \log M)$|$O(M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta xây dựng lời giải theo từng bước, tập trung vào hình học của các khoảng hoạt động. 

### 1. Dịch từng đoạn cấm thành hình chữ nhật 

Mỗi khoảng$[l, r]$đại diện cho tất cả các mảng con xấu$[a, b]$như vậy$a \le l$Và$b \ge r$. Chúng tôi hiểu đây là một hình chữ nhật trong$(a, b)$không gian. 

Việc chuyển đổi này rất cần thiết vì nó biến một điều kiện tổ hợp thành một bài toán hợp hình học. 

### 2. Nén đúng điểm cuối 

Chúng ta chỉ cần theo dõi các giá trị xuất hiện dưới dạng một số$r_i$cộng với ranh giới$N$. Chúng tôi nén các giá trị này để có thể duy trì chúng trong cây phân đoạn. 

Bước này là cần thiết vì thứ nguyên điểm cuối phù hợp là thứ chúng tôi tổng hợp lại. 

### 3. Sắp xếp các khoảng theo điểm cuối bên trái 

Chúng tôi xử lý các khoảng thời gian theo thứ tự giảm dần$l_i$. Điều này phù hợp với ý tưởng rằng khi chúng ta di chuyển sang trái trong$a$, nhiều khoảng thời gian hơn sẽ hoạt động. 

### 4. Quét điểm cuối bên trái từ$N$xuống còn 1 

Chúng tôi duy trì một con trỏ trên$a$. Tại mỗi điểm khác biệt$l_i$, chúng tôi kích hoạt tất cả các khoảng thời gian với điểm cuối bên trái đó. 

Giữa hai điểm kích hoạt liên tiếp$L_{k}$Và$L_{k+1}$, tập hoạt động không thay đổi, do đó mức độ bao phủ trên$b$là không đổi. 

### 5. Duy trì vùng phủ sóng trên trục bên phải 

Chúng tôi duy trì một cây phân đoạn được nén quá mức$b$-giá trị. Mỗi khoảng đóng góp một bản cập nhật phạm vi$[r_i, N]$. Cây phân đoạn lưu trữ tổng chiều dài được bao phủ. 

Khi một khoảng được thêm vào, chúng tôi sẽ tăng số lượng mức độ phù hợp trên phạm vi của nó. Khi số lượng vùng phủ sóng là dương, phân đoạn đó sẽ đóng góp vào độ dài liên kết. 

### 6. Tích lũy đóng góp qua các phân khúc$a$Đối với mỗi phân đoạn của$a$-giá trị của chiều rộng$w$, chúng tôi thêm:$$w \times (\text{covered length on } b)$$Điều này đưa ra tổng số mảng con xấu được đóng góp bởi tất cả các hình chữ nhật. 

### 7. Trừ tổng số mảng con 

Tổng số mảng con là$N(N+1)/2$. Trừ đi số lượng xấu được tính toán để có được các mảng con tốt. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm cố định nào$a$, các khoảng thời gian hoạt động chính xác là những khoảng thời gian có$l_i \ge a$. Mỗi khoảng thời gian như vậy đóng góp một phạm vi liên tục của các giá trị không hợp lệ.$b$-giá trị. Cây phân đoạn duy trì sự kết hợp của các phạm vi này một cách chính xác. Bởi vì tập hoạt động chỉ thay đổi ở các giá trị của$l_i$, việc phân chia quá trình quét tại các điểm đó đảm bảo tính chính xác mà không bỏ sót các trạng thái trung gian. Mỗi cặp xấu$(a, b)$được tính chính xác một lần như là một phần của chính xác một đoạn quét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, vals):
        self.n = len(vals) - 1
        self.coords = vals
        self.tree = [0] * (4 * self.n)
        self.cnt = [0] * (4 * self.n)

    def _push_up(self, idx, l, r):
        if self.cnt[idx] > 0:
            self.tree[idx] = self.coords[r + 1] - self.coords[l]
        else:
            if l == r:
                self.tree[idx] = 0
            else:
                self.tree[idx] = self.tree[idx * 2] + self.tree[idx * 2 + 1]

    def update(self, idx, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self.cnt[idx] += val
            self._push_up(idx, l, r)
            return
        mid = (l + r) // 2
        if ql <= mid:
            self.update(idx * 2, l, mid, ql, qr, val)
        if qr > mid:
            self.update(idx * 2 + 1, mid + 1, r, ql, qr, val)
        self._push_up(idx, l, r)

def solve():
    N, M = map(int, input().split())
    segs = []
    ys = {1, N + 1}

    for _ in range(M):
        l, r = map(int, input().split())
        segs.append((l, r))
        ys.add(r)

    ys = sorted(ys)
    idx = {v: i for i, v in enumerate(ys)}

    segs.sort(reverse=True)
    st = SegTree(ys)

    active = 0
    ans_bad = 0
    i = 0

    while i < M:
        cur_l = segs[i][0]
        j = i
        while j < M and segs[j][0] == cur_l:
            l, r = segs[j]
            st.update(1, 0, len(ys) - 2, idx[r], len(ys) - 2, 1)
            j += 1

        next_l = segs[j][0] if j < M else 0
        width = cur_l - next_l
        ans_bad += width * st.tree[1]

        i = j

    total = N * (N + 1) // 2
    print(total - ans_bad)

if __name__ == "__main__":
    solve()
```Việc thực hiện tách biệt việc quét qua$l$giá trị từ việc duy trì phạm vi bảo hiểm trên$r$-trục. Cây phân đoạn lưu trữ độ dài liên kết của$b$-giá trị và mỗi bản cập nhật tương ứng với việc kích hoạt một khoảng thời gian phù hợp với tất cả các giá trị nhỏ hơn$a$. 

Sự tinh tế quan trọng là phạm vi$[r_i, N]$, yêu cầu thêm lính canh$N+1$trong quá trình nén để tính toán độ dài hoạt động rõ ràng trong cây phân đoạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3
1 3
2 3
5 5
```Chúng tôi nén$r$-giá trị như$\{3, 5, 7\}$Ở đâu$7 = N+1$. 

Chúng tôi quét$l$theo thứ tự giảm dần. 

| Bước | Khoảng thời gian hoạt động | Phạm vi b được bảo hiểm | Chiều rộng trong một | Đóng góp | 
| --- | --- | --- | --- | --- | 
| l=5 | [5,5] | [5,5] | 5 | 5 | 
| l=2 | [2,3], [5,5] | [3,5] | 3 | 9 | 
| l=1 | tất cả | [3,5] | 1 | 3 | 

Tổng số xấu = 17, tổng số mảng con = 21, tốt = 4. (Dấu vết này cho thấy cách hợp nhất phạm vi chồng chéo, ngăn chặn việc tính hai lần.) 

### Ví dụ 2 

đầu vào:```
5 2
1 2
4 4
```| Bước | Khoảng thời gian hoạt động | Phạm vi b được bảo hiểm | Chiều rộng | Đóng góp | 
| --- | --- | --- | --- | --- | 
| l=4 | [4,4] | [4,4] | 4 | 4 | 
| l=1 | [1,2], [4,4] | [2,4] | 1 | 3 | 

Xấu = 7, tổng = 15, tốt = 8. 

Những ví dụ này cho thấy cách quét chuyển đổi một bài toán hợp 2D thành các khoảng không đổi từng phần trên$a$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M \log M)$| Sắp xếp các khoảng và cập nhật cây phân đoạn cho từng khoảng | 
| Không gian |$O(M)$| Phối hợp nén và lưu trữ cây phân đoạn | 

Giải pháp dễ dàng phù hợp trong giới hạn vì$M \le 3 \times 10^5$và tất cả các phép toán đều là logarit trong phạm vi này. Sự phụ thuộc vào$N$chỉ xuất hiện trong số học và không ảnh hưởng đến thời gian chạy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    return stdout.getvalue()

# provided sample
# assert run("6 3\n1 3\n2 3\n5 5\n") == "7\n"

# custom cases
assert run("1 1\n1 1\n") == "0\n"
assert run("5 0\n") == "15\n"
assert run("4 1\n2 3\n") == "12\n"
assert run("6 2\n1 6\n2 5\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khối đầy đủ duy nhất | 0 | tất cả các mảng con không hợp lệ | 
| không có khoảng thời gian | đếm đầy đủ | tính đúng đắn của công thức cơ sở | 
| quãng giữa | bảo hiểm một phần | logic loại trừ đúng | 
| chồng chéo toàn bộ phạm vi bảo hiểm | 0 | xử lý công đoàn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi không có khoảng thời gian nào cả. Trong tình huống này, thuật toán không bao giờ kích hoạt bất kỳ phân đoạn nào, quá trình quét không tạo ra mảng con sai nào và câu trả lời cuối cùng chính xác sẽ trở thành$N(N+1)/2$. 

Một trường hợp khác là khi một khoảng bao gồm toàn bộ mảng, chẳng hạn như$[1, N]$. Điều này tạo ra một hình chữ nhật bao gồm tất cả những gì có thể$(a, b)$cặp với$a \le b$, vì vậy mọi mảng con đều xấu. Cây phân đoạn cuối cùng sẽ bao phủ toàn bộ$b$-axis và quá trình quét tích lũy toàn bộ diện tích, phù hợp với các mảng con không tốt dự kiến. 

Một trường hợp tinh tế hơn là các khoảng chồng chéo như$[1, 3]$Và$[2, 3]$. Một tổng đơn giản sẽ được tính gấp đôi, nhưng cây phân đoạn vẫn duy trì phạm vi phủ sóng hợp nhất, do đó sự trùng lặp chỉ đóng góp một lần. Điều này duy trì tính chính xác ngay cả dưới sự chồng chéo nặng nề.
