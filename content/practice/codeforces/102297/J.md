---
title: "CF 102297J - Bạn Sẽ Vượt Qua"
description: "Chúng tôi có tới 50 học sinh và mỗi học sinh phải được phân vào lớp của Matt hoặc lớp của Sean. Học sinh (i) có xác suất cơ bản để đỗ lớp của Matt và một xác suất cơ bản khác để đỗ lớp của Sean."
date: "2026-08-13T08:38:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102297
codeforces_index: "J"
codeforces_contest_name: "UCF Locals 2015"
rating: 0
weight: 102297
solve_time_s: 117
verified: true
draft: false
---

[CF 102297J - Bạn sẽ vượt qua](https://codeforces.com/problemset/problem/102297/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có tới 50 học sinh và mỗi học sinh phải được phân vào lớp của Matt hoặc lớp của Sean. Học sinh (i) có xác suất cơ bản để đỗ lớp của Matt và một xác suất cơ bản khác để đỗ lớp của Sean. 

Nếu hai học sinh (i) và (j) được xếp vào cùng một lớp, nhóm học sẽ tăng xác suất của học sinh (i) lên (a_{ij}) và tăng xác suất của học sinh (j) lên (a_{ji}). Do đó, đối với một cặp được đặt cùng nhau, tổng đóng góp của cặp đó là (a_{ij}+a_{ji}). Nếu họ bị tách ra, không có sự đóng góp nào được nhận. 

Mục tiêu là tối đa hóa số lượng học sinh đỗ dự kiến. Vì kỳ vọng của một tổng là tổng của các kỳ vọng nên chúng ta chỉ cần tối đa hóa tổng của tất cả các xác suất đậu riêng lẻ. 

Đầu vào chứa một số học kỳ. Đối với mỗi học kỳ, (n) là số lượng sinh viên, theo sau là (n) xác suất Matt, (n) xác suất Sean và ma trận (n\times n) mô tả sự cải thiện của nhóm nghiên cứu. Kết quả đầu ra là số lượng học sinh đậu dự kiến ​​tối đa có thể, được in đến hai chữ số thập phân. Bản PDF chính thức của cuộc thi xác nhận thông tin đầu vào mẫu hoàn chỉnh, bao gồm cả trường hợp kiểm tra và số học sinh bị mất trong đoạn trích được cung cấp. 

Giới hạn (n\leq 50) là ràng buộc chính. Có thể có (2^n) bài tập trên lớp, tức là khoảng (1,13\times10^{15}) bài tập tại (n=50). Ngay cả việc đánh giá một bài tập trong (O(n^2)) cũng sẽ vô vọng. Mặt khác, một đồ thị chỉ có khoảng 50 đỉnh sinh viên là rất nhỏ, do đó, thuật toán cắt tối đa hoặc cắt tối thiểu theo thời gian đa thức là dễ dàng thực tế. 

Các giá trị thập phân có chính xác hai chữ số sau dấu thập phân. Chúng ta có thể nhân mọi giá trị với 100 và làm việc hoàn toàn với số nguyên. Điều này loại bỏ lỗi dấu phẩy động và làm cho câu trả lời cuối cùng là một số nguyên chính xác đến phần trăm. 

Một trường hợp tế nhị là ma trận nhóm nghiên cứu không đối xứng. Ví dụ: với hai học sinh và```
1
2
0.50 0.50
0.50 0.50
0.00 0.30
0.10 0.00
```đặt cả hai học sinh lại với nhau sẽ có thêm (0,30+0,10=0,40), vì vậy câu trả lời là`1.40`. Việc triển khai bất cẩn chỉ sử dụng (a_{ij}), thay vì cả hai hướng, sẽ thu được kết quả không chính xác`1.30`. 

Một trường hợp khác là một lớp có thể trống. Với```
1
2
1.00 0.00
0.00 1.00
0.00 0.00
0.00 0.00
```đặt học sinh 1 với Matt và học sinh 2 với Sean cho giá trị kỳ vọng là`2.00`. Bất kỳ phương pháp nào giả định cả hai lớp phải có một học sinh sẽ loại trừ các bài tập một bên hợp lệ trong các trường hợp khác. 

Vấn đề thứ ba là xử lý số thập phân chính xác. Vì mọi giá trị đầu vào đều là bội số của (0,01), nên giá trị tối ưu cũng là bội số của (0,01). Sử dụng dấu phẩy động nhị phân và sau đó định dạng kết quả có thể gây ra các lỗi có thể tránh được xung quanh ranh giới thập phân. Chia tỷ lệ số nguyên sẽ tránh được vấn đề đó hoàn toàn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi sự phân công có thể có của học sinh vào hai lớp. Biểu thị phép gán bằng vectơ nhị phân, trong đó số 0 nghĩa là Sean và một nghĩa là Matt. Đối với mỗi bài tập, chúng ta có thể tính xác suất cơ bản của mỗi học sinh và sau đó kiểm tra từng cặp học sinh để xác định xem sự đóng góp của nhóm học tập của họ có áp dụng hay không. Có (2^n) bài tập và việc đánh giá một bài tập mất (O(n^2)), cho (O(2^n n^2)) thời gian. Tại (n=50), con số này xấp xỉ (1,13\times10^{15}\cdot2500), vượt xa mọi giá trị thực tế. 

Lực lượng vũ phu hoạt động vì mọi nhiệm vụ đều có thể được đánh giá một cách độc lập và trực tiếp. Vấn đề là mục tiêu có cấu trúc hữu ích hơn nhiều so với sự phụ thuộc tùy tiện giữa các nhiệm vụ. 

Với mỗi cặp (i,j), nếu chúng cùng lớp thì chúng ta thu được 

[ 
w_{ij}=a_{ij}+a_{ji}. 
] 

Nếu chúng bị tách ra, chúng ta sẽ mất toàn bộ số tiền đó. Vì tất cả các giá trị của nhóm nghiên cứu đều không âm nên việc tách một cặp chỉ có thể loại bỏ phần thưởng. Đây chính xác là loại tương tác theo cặp có thể được biểu diễn bằng một cạnh cắt vô hướng. 

Sở thích cá nhân của sinh viên cũng có thể được biểu diễn dưới dạng một thuật ngữ đơn nhất. Xác định 

[ 
d_i = M_i-S_i, 
] 

trong đó (M_i) và (S_i) là hai xác suất cơ bản. Nếu sinh viên (i) được chuyển từ Sean sang Matt, mức đóng góp cơ bản sẽ thay đổi theo (d_i). 

Chọn Sean làm cấu hình tham chiếu, nghĩa là ban đầu mọi học sinh đều thuộc lớp của Sean. Trong cấu hình đó, giá trị là 

[ 
B=\sum_i S_i+\sum_{i<j}w_{ij}. 
] 

Nếu một học sinh chuyển đến Matt, chúng ta sẽ đạt được (d_i). Nếu hai học sinh ở hai phía khác nhau, chúng ta thua (w_{ij}). Do đó, đối với một tập hợp (X) học sinh được giao cho Matt, 

B+\sum_{i\in X}d_i 
-\sum_{\substack{i<j\i\in X,\ j\notin X}}w_{ij}. 
] 

Đây là một bài toán tối ưu hóa nhị phân với độ lợi đơn phân và hình phạt không âm khi tách hai đỉnh. Chúng ta có thể chuyển đổi nó thành mức cắt tối thiểu (s)-(t). 

Điều phức tạp nhỏ duy nhất là việc cắt giảm tối thiểu chỉ có thể thể hiện chi phí không âm. Thêm 

[ 
P=\sum_i\max(d_i,0) 
] 

đến công thức cắt. Đối với một học sinh có (d_i>0), biểu đồ cho biết (d_i) nếu học sinh đó đứng về phía Sean, vì làm như vậy sẽ tạo ra thiện cảm tích cực dành cho Matt. Đối với (d_i<0), biểu đồ trả tiền (-d_i) nếu học sinh chuyển sang Matt, vì điều đó từ bỏ ưu tiên dành cho Sean. 

Với mỗi cặp, hãy thêm một cạnh vô hướng của công suất (w_{ij}). Nếu cả hai học sinh ở cùng một phía thì cạnh đó không đóng góp gì vào đường cắt. Nếu chúng nằm ở phía đối diện thì có đúng một cung có hướng đi qua vết cắt và đóng góp (w_{ij}). 

Do đó, 

P-\sum_{i\in X}d_i 
+\sum_{\text{được phân tách }i,j}w_{ij}, 
] 

và do đó 

[ 
\đóng hộp{ 
\text{answer}=B+P-\text{cắt tối thiểu} 
}. 
] 

Nhận xét rằng phần thưởng theo cặp chính xác là một hình phạt cho việc tách hai học sinh ra khỏi nhau là nguyên nhân biến một bài toán phân chia hàm mũ rõ ràng thành một bài toán cắt tối thiểu tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n n^2)) | (O(n^2)) | Quá chậm | 
| Tối ưu | (O(n^4)) với Dinic chung | (O(n^2)) | Đã chấp nhận | 

Giới hạn (O(n^4)) là giới hạn tiêu chuẩn (O(V^2E)) cho Dinic trên biểu đồ này, với (V=O(n)) và (E=O(n^2)). Chỉ với 50 sinh viên, đồ thị thu được rất nhỏ. 

## Hướng dẫn thuật toán 

1. Phân tích mọi xác suất dưới dạng số nguyên phần trăm. Ví dụ,`0.75`trở thành`75`. Điều này cho phép toàn bộ quá trình tối ưu hóa chạy bằng cách sử dụng số học số nguyên chính xác. 
2. Tính giá trị cơ sở (B) tương ứng với việc xếp mọi học sinh vào lớp của Sean. Bắt đầu với tổng xác suất cơ sở của Sean. Sau đó, với mỗi cặp (i<j), hãy thêm (a_{ij}+a_{ji}), vì cả hai học sinh đều nhận được những cải thiện tương ứng trong nhóm học tập khi họ ở cùng nhau. 
3. Với mỗi học sinh, hãy tính (d_i=M_i-S_i). Đồng thời tích lũy (P=\sum_i\max(d_i,0)). Giá trị (P) là hằng số cần thiết để chuyển đổi phần thưởng đơn nhất dương thành chi phí cắt giảm không âm. 
4. Tạo một đỉnh nguồn biểu thị lớp của Matt và một đỉnh chìm biểu thị lớp của Sean. Một học sinh ở phía nguồn được hiểu là đang học trong lớp của Matt, trong khi một học sinh ở phía bồn rửa được hiểu là đang học trong lớp của Sean. 
5. Nếu (d_i>0), thêm một cạnh từ nguồn vào sinh viên (i) có dung lượng (d_i). Cắt bỏ điều này có nghĩa là giữ một học sinh thích Matt đứng về phía Sean, vì vậy việc cắt giảm sẽ trả chính xác cho sự ưu tiên đã mất. Nếu (d_i<0), thêm một cạnh từ sinh viên (i) vào bồn có dung lượng (-d_i). Cắt bỏ điều này có nghĩa là chuyển một học sinh thích Sean vào lớp của Matt, một lần nữa trả lại sự ưu tiên đã mất. 
6. Với mỗi cặp (i<j), hãy tính (w_{ij}=a_{ij}+a_{ji}). Thêm dung lượng (w_{ij}) theo cả hai hướng giữa hai đỉnh học sinh. Nếu cả hai học sinh đều nhận được cùng một nhãn lớp thì không có hướng nào vượt qua được đường cắt. Nếu nhãn của chúng khác nhau, một hướng sẽ giao nhau và đóng góp chính xác (w_{ij}). 
7. Chạy thuật toán luồng cực đại từ nguồn tới đích. Theo định lý lưu lượng tối đa/cắt tối thiểu, giá trị lưu lượng thu được bằng công suất cắt tối thiểu. Mức cắt giảm tối thiểu thể hiện sự mất mát ít nhất có thể sau khi tính đến sở thích của sinh viên và các nhóm học tập riêng biệt. 
8. Trả về (B+P-\text{flow}), chia cho 100. Vì mọi số lượng được lưu trữ ở phần trăm, phép chia này đưa ra câu trả lời chính xác là hai thập phân mà không cần bất kỳ phép tính làm tròn nào. 

### Tại sao nó hoạt động 

Đối với bất kỳ bài tập lớp nào (X), đường cơ sở (B) đã chứa giá trị để xếp mọi người vào lớp của Sean. Việc chuyển sinh viên (i) sang Matt sẽ thay đổi mức đóng góp cơ bản bằng (d_i), trong khi việc tách một cặp (i,j) sẽ loại bỏ (w_{ij}) khỏi phần thưởng cơ bản của nhóm nghiên cứu. 

Việc cắt giảm được xây dựng có chính xác chi phí bổ sung. (d_i) dương đóng góp (d_i) vào vết cắt chính xác khi (i) được đặt không chính xác ở phía Sean, trong khi (d_i) âm đóng góp (-d_i) chính xác khi (i) được chuyển sang phía Matt. Một cặp đóng góp (w_{ij}) chính xác khi hai học sinh của nó bị tách ra. Việc cộng (P) làm cho tất cả chi phí đơn phân không âm, do đó mọi phép gán đều cắt giảm chi phí 

[ 
P+\bigl(B-\văn bản{giá trị}(X)\bigr). 
] 

Vì vậy, việc giảm thiểu việc cắt giảm hoàn toàn tương đương với việc tối đa hóa số lượng học sinh đỗ dự kiến. Do đó, mức cắt giảm tối thiểu mang lại sự phân chia lớp tối ưu trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Dinic:
    class Edge:
        __slots__ = ("to", "rev", "cap")

        def __init__(self, to, rev, cap):
            self.to = to
            self.rev = rev
            self.cap = cap

    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]
        self.level = [-1] * n
        self.it = [0] * n

    def add_edge(self, u, v, cap):
        a = self.Edge(v, len(self.g[v]), cap)
        b = self.Edge(u, len(self.g[u]), 0)
        self.g[u].append(a)
        self.g[v].append(b)

    def bfs(self, s, t):
        self.level = [-1] * self.n
        q = [s]
        self.level[s] = 0

        head = 0
        while head < len(q):
            u = q[head]
            head += 1

            for e in self.g[u]:
                if e.cap > 0 and self.level[e.to] == -1:
                    self.level[e.to] = self.level[u] + 1
                    q.append(e.to)

        return self.level[t] != -1

    def dfs(self, u, t, pushed):
        if u == t:
            return pushed

        while self.it[u] < len(self.g[u]):
            e = self.g[u][self.it[u]]

            if e.cap > 0 and self.level[e.to] == self.level[u] + 1:
                flow = self.dfs(e.to, t, min(pushed, e.cap))

                if flow:
                    e.cap -= flow
                    self.g[e.to][e.rev].cap += flow
                    return flow

            self.it[u] += 1

        return 0

    def max_flow(self, s, t):
        flow = 0
        INF = 10**30

        while self.bfs(s, t):
            self.it = [0] * self.n

            while True:
                pushed = self.dfs(s, t, INF)
                if pushed == 0:
                    break
                flow += pushed

        return flow

def parse100(x):
    whole, frac = x.split(".")
    return int(whole) * 100 + int(frac)

def solve_case(n, matt, sean, a):
    source = n
    sink = n + 1

    dinic = Dinic(n + 2)

    baseline = sum(sean)

    d = [matt[i] - sean[i] for i in range(n)]
    positive = 0

    for i in range(n):
        if d[i] > 0:
            dinic.add_edge(source, i, d[i])
            positive += d[i]
        elif d[i] < 0:
            dinic.add_edge(i, sink, -d[i])

    for i in range(n):
        for j in range(i + 1, n):
            w = a[i][j] + a[j][i]

            baseline += w

            if w:
                dinic.add_edge(i, j, w)
                dinic.add_edge(j, i, w)

    cut = dinic.max_flow(source, sink)

    return baseline + positive - cut

def solve():
    g = int(input())

    out = []

    for _ in range(g):
        n = int(input())

        matt = [parse100(x) for x in input().split()]
        sean = [parse100(x) for x in input().split()]

        a = []
        for _ in range(n):
            a.append([parse100(x) for x in input().split()])

        ans = solve_case(n, matt, sean, a)

        out.append(f"{ans // 100}.{ans % 100:02d}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`parse100`hàm cố tình hoạt động trên các chuỗi gốc thay vì chuyển đổi chúng thông qua`float`. Một giá trị như`0.20`trở thành chính xác 20, vì vậy tất cả dung lượng đồ thị đều là số nguyên. 

Đường cơ sở được khởi tạo với tất cả xác suất Sean và sau đó nhận được phần thưởng tổng hợp của mỗi cặp. Các phần tử chéo của ma trận bị bỏ qua vì phần đóng góp của nhóm nghiên cứu mô tả một cặp sinh viên khác nhau. Đối với mỗi cặp (i<j), cả hai hướng được kết hợp thành một phần thưởng (w_{ij}), do đó, đầu vào không đối xứng được xử lý chính xác. 

Các cạnh đơn nhất tuân theo dấu của (d_i). Sự khác biệt tích cực có nghĩa là Matt sẽ tốt hơn cho học sinh đó, do đó, lợi thế nguồn-học sinh sẽ tính sự khác biệt khi học sinh vẫn đứng về phía Sean. Một sự khác biệt tiêu cực có nghĩa là Sean tốt hơn, do đó, lợi thế của học sinh bị đánh chìm sẽ tính ra sự khác biệt tuyệt đối khi học sinh được xếp cùng với Matt. 

Cạnh cặp được thêm vào theo cả hai hướng. Đang gọi`add_edge(i, j, w)`riêng nó sẽ tạo ra một cạnh đảo ngược còn lại có công suất bằng 0, không đủ để mô hình hóa một hình phạt cắt vô hướng. Việc thêm cạnh đối diện một cách rõ ràng sẽ cho dung lượng đồ thị (w) theo cả hai hướng. Chính xác một trong hai cung đó cắt (s)-(t) khi học sinh được tách ra. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn mặc dù tất cả dung lượng đều được chia tỷ lệ theo 100. Tổng mục tiêu lớn nhất cũng rất nhỏ so với phạm vi số nguyên của Python. 

Cuối cùng,`ans`đã là con số chính xác của phần trăm. biểu hiện`ans // 100`cho phần nguyên và`ans % 100`cho hai chữ số thập phân. Không cần làm tròn dấu phẩy động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Học kỳ đầu tiên có hai sinh viên. Xác suất cơ bản của chúng là 

[ 
M=(0,75,0,25),\qquad S=(0,25,0,75). 
] 

Ma trận nhóm nghiên cứu cho ra (a_{12}=0,20) và (a_{21}=0,20), do đó, việc gộp cả hai học sinh lại với nhau sẽ có tổng phần thưởng cho cặp là (0,40). 

Các giá trị tỷ lệ số nguyên chính được hiển thị bên dưới. 

| Biến | Giá trị | 
| --- | --- | 
| Đường cơ sở Sean | 100 | 
| Phần thưởng cặp | 40 | 
| Tổng số đường cơ sở (B) | 140 | 
| (d_1) | 50 | 
| (d_2) | -50 | 
| Tổng dương (P) | 50 | 
| Cắt tối thiểu | 20 | 
| Giá trị cuối cùng | 170 | 

Mức cắt giảm tối thiểu giữ cho học sinh 1 đứng về phía Matt và học sinh 2 về phía Sean. Phần thưởng cặp 40 bị mất, nhưng học sinh 1 nhận được 50 khi chọn Matt thay vì Sean. So với đường cơ sở của toàn Sean, mức tăng ròng là 10, cho (150) phần trăm hoặc`1.50`. 

Việc tính toán đồ thị cho kết quả tương tự thông qua 

[ 
B+P-\văn bản{cut}=140+50-20=170. 
] 

Ở đây đường cơ sở phải được giải thích một cách cẩn thận. Nó chứa phần thưởng cặp cho mọi người ở cùng nhau, trong khi mức cắt giảm tối thiểu sẽ loại bỏ các phần thưởng và ưu đãi mà phần chia đã chọn không giữ lại. 

### Mẫu 2 

Học kỳ thứ hai có ba sinh viên. Xác suất Sean là (0,40,0,40,0,95) và các giá trị nhóm nghiên cứu khác 0 duy nhất là (a_{13}=0,55) và (a_{23}=0,35). 

Đưa mọi người vào lớp của Sean sẽ mang lại 

[ 
0,40+0,55+0,40+0,35+0,95=2,65. 
] 

Các biến đồ thị là: 

| Biến | Giá trị | 
| --- | --- | 
| Tổng cơ sở Sean | 175 | 
| Phần thưởng cặp (w_{13}) | 55 | 
| Phần thưởng cặp (w_{23}) | 35 | 
| Tổng số đường cơ sở (B) | 265 | 
| (d_1) | -20 | 
| (d_2) | 20 | 
| (d_3) | 0 | 
| Tổng dương (P) | 20 | 
| Cắt tối thiểu | 20 | 
| Giá trị cuối cùng | 265 | 

Học sinh 2 có ưu tiên 20 phần trăm dành cho Matt, vì vậy biểu đồ chứa cạnh nguồn-đến-sinh viên-2 có công suất 20. Việc chuyển học sinh 2 sang Matt cũng sẽ tách học sinh đó khỏi học sinh 3, làm mất phần thưởng nhóm nghiên cứu 35 phần trăm. Do đó, mức cắt giảm tối thiểu khiến mọi người đứng về phía Sean và trả mức lợi nhuận đơn nhất là 20 phần trăm. 

Kết quả cuối cùng là 

[ 
265+20-20=265, 
] 

đó là`2.65`. 

Dấu vết này chứng minh lý do tại sao biểu đồ phải xem xét đồng thời sở thích của học sinh và ghép đôi phần thưởng. Việc chọn một lớp chỉ dựa trên xác suất cơ bản tốt hơn của mỗi học sinh có thể là chưa tối ưu vì việc di chuyển một học sinh có thể làm mất đi một số phần thưởng của nhóm học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^4)) | Biểu đồ có (O(n)) đỉnh và (O(n^2)) cạnh và Dinic chung có giới hạn trường hợp xấu nhất (O(V^2E)). | 
| Không gian | (O(n^2)) | Đồ thị cặp dày đặc chứa (O(n^2)) cạnh dư. | 

Với (n\leq50), đồ thị có tối đa 52 đỉnh và khoảng (O(2500)) cạnh cặp sinh viên trước khi tính các cạnh dư. Ngay cả giới hạn bảo thủ (O(n^4)) cũng nhỏ ở quy mô này và mức sử dụng bộ nhớ là bậc hai. 

Việc liệt kê theo cấp số nhân của các bài tập (2^{50}) là trở ngại thực sự. Việc thay thế nó bằng một phép tính nhỏ với mức cắt giảm tối thiểu là điều làm cho giải pháp trở nên thiết thực. 

## Trường hợp thử nghiệm```python
import sys
import io

class Dinic:
    class Edge:
        __slots__ = ("to", "rev", "cap")

        def __init__(self, to, rev, cap):
            self.to = to
            self.rev = rev
            self.cap = cap

    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]
        self.level = [-1] * n
        self.it = [0] * n

    def add_edge(self, u, v, cap):
        a = self.Edge(v, len(self.g[v]), cap)
        b = self.Edge(u, len(self.g[u]), 0)
        self.g[u].append(a)
        self.g[v].append(b)

    def bfs(self, s, t):
        self.level = [-1] * self.n
        self.level[s] = 0
        q = [s]
        head = 0

        while head < len(q):
            u = q[head]
            head += 1

            for e in self.g[u]:
                if e.cap > 0 and self.level[e.to] == -1:
                    self.level[e.to] = self.level[u] + 1
                    q.append(e.to)

        return self.level[t] != -1

    def dfs(self, u, t, pushed):
        if u == t:
            return pushed

        while self.it[u] < len(self.g[u]):
            e = self.g[u][self.it[u]]

            if e.cap > 0 and self.level[e.to] == self.level[u] + 1:
                got = self.dfs(e.to, t, min(pushed, e.cap))

                if got:
                    e.cap -= got
                    self.g[e.to][e.rev].cap += got
                    return got

            self.it[u] += 1

        return 0

    def max_flow(self, s, t):
        flow = 0
        INF = 10**30

        while self.bfs(s, t):
            self.it = [0] * self.n

            while True:
                pushed = self.dfs(s, t, INF)
                if pushed == 0:
                    break
                flow += pushed

        return flow

def parse100(x):
    whole, frac = x.split(".")
    return int(whole) * 100 + int(frac)

def solve_case(n, matt, sean, a):
    source = n
    sink = n + 1
    dinic = Dinic(n + 2)

    baseline = sum(sean)
    positive = 0

    for i in range(n):
        d = matt[i] - sean[i]

        if d > 0:
            dinic.add_edge(source, i, d)
            positive += d
        elif d < 0:
            dinic.add_edge(i, sink, -d)

    for i in range(n):
        for j in range(i + 1, n):
            w = a[i][j] + a[j][i]
            baseline += w

            if w:
                dinic.add_edge(i, j, w)
                dinic.add_edge(j, i, w)

    return baseline + positive - dinic.max_flow(source, sink)

def solve():
    input = sys.stdin.readline
    g = int(input())
    out = []

    for _ in range(g):
        n = int(input())
        matt = [parse100(x) for x in input().split()]
        sean = [parse100(x) for x in input().split()]
        a = [[parse100(x) for x in input().split()] for _ in range(n)]

        ans = solve_case(n, matt, sean, a)
        out.append(f"{ans // 100}.{ans % 100:02d}")

    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

sample = """\
2
2
0.75 0.25
0.25 0.75
0.00 0.20
0.20 0.00
3
0.20 0.60 0.95
0.40 0.40 0.95
0.00 0.00 0.55
0.00 0.00 0.35
0.00 0.00 0.00
"""

assert run(sample) == "1.50\n2.65", "official sample"

minimum = """\
1
2
1.00 0.00
0.00 1.00
0.00 0.00
0.00 0.00
"""

assert run(minimum) == "2.00", "minimum n and opposite class preferences"

asymmetric = """\
1
2
0.50 0.50
0.50 0.50
0.00 0.30
0.10 0.00
"""

assert run(asymmetric) == "1.40", "both directions of a study group must be counted"

zero_interactions = """\
1
2
0.25 0.80
0.75 0.20
0.00 0.00
0.00 0.00
"""

assert run(zero_interactions) == "1.55", "each student independently chooses the better class"

n = 50
matt = " ".join(["0.50"] * n)
sean = " ".join(["0.50"] * n)
zero_row = " ".join(["0.00"] * n)

maximum_size = "1\n" + str(n) + "\n"
maximum_size += matt + "\n"
maximum_size += sean + "\n"
maximum_size += "\n".join([zero_row] * n) + "\n"

assert run(maximum_size) == "25.00", "maximum n with all equal values"
```Khẳng định đầu tiên sử dụng mẫu hai học kỳ chính thức. Trường hợp kích thước tối thiểu kiểm tra xem mức tối ưu hợp lệ có thể đưa học sinh vào các lớp khác nhau hay không và lớp trống cũng được cho phép. 

Trường hợp bất đối xứng được thiết kế đặc biệt để phát hiện lỗi phổ biến khi coi (a_{ij}) và (a_{ji}) là một giá trị. Cả hai khoản đóng góp đều được áp dụng khi học sinh học chung một lớp, vì vậy phần thưởng cho cặp đôi là 40 phần trăm. 

Trường hợp không tương tác làm giảm vấn đề thành các quyết định độc lập cho mỗi học sinh. Học sinh 1 chọn Sean với (0,75), trong khi học sinh 2 chọn Matt với (0,80), cho`1.55`. 

Thử nghiệm cuối cùng sử dụng mức tối đa cho phép (n=50), với mọi xác suất bằng`0.50`và mọi giá trị của nhóm nghiên cứu đều bằng 0. Mọi phép gán đều có cùng một giá trị, cụ thể là (50\times0,50=25,00). Nó cũng kiểm tra xem ma trận đầu vào và cấu trúc đồ thị dày đặc có hoạt động ở kích thước lớn nhất cho phép hay không. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu hai học kỳ chính thức |`1.50`Và`2.65`| Cấu trúc chính và hành vi mẫu | 
| (n=2), sở thích lớp đối diện |`2.00`| Kích thước tối thiểu và khả năng lớp trống | 
| (a_{12}=0,30,\ a_{21}=0,10) |`1.40`| Đóng góp cặp bất đối xứng | 
| Ma trận tương tác bằng không |`1.55`| Lựa chọn đơn nhất độc lập | 
| (n=50), tất cả các giá trị đều bằng nhau |`25.00`| Kích thước tối đa và xử lý ma trận dày đặc | 

## Vỏ cạnh 

### Lớp trống 

Hãy xem xét```
1
2
1.00 0.00
0.00 1.00
0.00 0.00
0.00 0.00
```Không có phần thưởng cặp. Sự khác biệt là (d_1=1,00) và (d_2=-1,00). Biểu đồ có cạnh nguồn-tới-student-1 có dung lượng 100 và cạnh-student-2-sink có dung lượng 100. Điểm cắt tối thiểu đặt sinh viên 1 với Matt và sinh viên 2 với Sean, cho giá trị 200 phần trăm, hoặc`2.00`. 

Thay vào đó, nếu mọi học sinh đều thích cùng một lớp, thì mức cắt giảm tối thiểu sẽ xếp mọi người vào bên đó, để trống lớp kia. Đồ thị không áp đặt bất kỳ yêu cầu nào rằng cả hai bên đều chứa các đỉnh. 

### Nhóm nghiên cứu bất đối xứng 

Hãy xem xét```
1
2
0.50 0.50
0.50 0.50
0.00 0.30
0.10 0.00
```Giá trị cơ bản là 100 phần trăm. Nếu học sinh ở cùng nhau, phần thưởng của cặp là (30+10=40), tạo ra 140 phần trăm. Nếu chúng bị tách ra, phần thưởng cặp sẽ biến mất và giá trị chỉ còn 100. 

Đồ thị chứa hai cung đối diện có dung lượng 40 giữa các đỉnh của học sinh. Một vết cắt ngăn cách chúng đi qua chính xác một trong những cung đó, trả 40. Một vết cắt giữ chúng lại với nhau cũng không cắt nhau. Do đó, mức cắt tối thiểu sẽ chọn cùng một loại và đầu ra là`1.40`. 

### Không tương tác 

cho```
1
2
0.25 0.80
0.75 0.20
0.00 0.00
0.00 0.00
```học sinh đầu tiên đạt được 50 phần trăm khi chọn Sean, trong khi học sinh thứ hai đạt được 60 phần trăm khi chọn Matt. Không có cặp cạnh nào cả. Mức cắt giảm tối thiểu khiến hai quyết định đó trở nên độc lập, đưa ra (75+80=155) phần trăm hoặc`1.55`. 

Đây là một phép kiểm tra độ chính xác hữu ích vì biểu đồ sẽ giảm xuống còn hai lựa chọn đơn nhất độc lập khi mọi giá trị của nhóm nghiên cứu đều bằng 0. 

### Số học thập phân chính xác 

Mỗi giá trị đầu vào được biểu diễn dưới dạng số nguyên phần trăm. Ví dụ,`0.75`trở thành 75 và`0.20`trở thành 20. Do đó, mỗi dung lượng đồ thị là một số nguyên và mức tối ưu cuối cùng cũng là một số nguyên phần trăm. 

Đối với mẫu đầu tiên chính thức, kết quả là 150, do đó chương trình sẽ in`150 // 100 = 1`theo sau là`50`là phần phân số, tạo ra`1.50`. Không có phép tính dấu phẩy động ở bất kỳ đâu trong quá trình tối ưu hóa, vì vậy các giá trị như`0.10 + 0.20`không thể tích lũy các lỗi biểu diễn nhị phân. 

### Số lượng học sinh tối đa 

Tại (n=50), có 50 đỉnh sinh viên cộng với nguồn và đích. Phần cặp sinh viên chứa tối đa (50\cdot49/2=1225) cặp riêng biệt, với hai dung lượng định hướng được sử dụng cho mỗi cặp. Biểu đồ vẫn rất nhỏ và Dinic xử lý nó một cách thoải mái. 

Trường hợp có kích thước tối đa hoàn toàn bằng nhau có xác suất mọi học sinh`0.50`trong cả hai lớp và không có phần thưởng cho nhóm học tập. Mỗi phép gán đều có giá trị mong đợi chính xác là 25, do đó thuật toán có thể trả về bất kỳ mức cắt tối thiểu nào, nhưng mục tiêu được tính toán luôn là`25.00`. Điều này xác nhận rằng dây buộc không yêu cầu bất kỳ xử lý đặc biệt nào.
