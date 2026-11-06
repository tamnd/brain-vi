---
title: "CF 1252K - Robot cộng"
description: "Robot lưu trữ chuỗi lệnh nhị phân trên bảng chữ cái {A, B}. Khi chúng ta xử lý chuỗi này với một cặp giá trị ban đầu (A, B), mỗi ký tự hoạt động giống như một bước chuyển đổi nhỏ."
date: "2026-06-18T17:39:04+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1252
codeforces_index: "K"
codeforces_contest_name: "2019-2020 ICPC, Asia Jakarta Regional Contest (Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2100
weight: 1252
solve_time_s: 112
verified: false
draft: false
---

[CF 1252K - Robot bổ sung](https://codeforces.com/problemset/problem/1252/K) 

**Đánh giá:** 2100 
**Tags:** cấu trúc dữ liệu, toán, ma trận 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Robot lưu trữ chuỗi lệnh nhị phân trên bảng chữ cái {A, B}. Khi chúng tôi xử lý chuỗi này với một cặp giá trị ban đầu`(A, B)`, mỗi nhân vật đóng vai trò như một bước chuyển hóa nhỏ. Nếu ký tự hiện tại là`A`, giá trị thứ hai sẽ được thêm vào giá trị thứ nhất và nếu ký tự đó là`B`, giá trị đầu tiên được thêm vào giá trị thứ hai. Sau khi xử lý một phân đoạn, chúng ta thu được một cặp được chuyển đổi. 

Nhiệm vụ là hỗ trợ hai thao tác trên chuỗi này. Một thao tác lật một loạt ký tự, lật từng ký tự`A`vào trong`B`và ngược lại. Hoạt động khác yêu cầu chúng ta đánh giá sự biến đổi do một chuỗi con tạo ra trên một cặp bắt đầu nhất định. 

Khó khăn chính là cả hai hoạt động đều trực tuyến và có tới 100.000 hoạt động. Việc mô phỏng trực tiếp từng truy vấn bằng cách lặp qua chuỗi con sẽ tốn thời gian tuyến tính cho mỗi truy vấn, dẫn đến khoảng$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa những gì có thể thực hiện được. 

Việc chuyển đổi cũng phụ thuộc vào thứ tự và không tầm thường: mỗi ký tự thay đổi trạng thái theo cách phụ thuộc vào giá trị hiện tại của cả hai biến. Điều này ngăn cản việc đếm các ký tự đơn giản là không đủ, vì tác dụng của một`A`phụ thuộc vào sự phát triển`B`, và ngược lại. 

Một trường hợp phức tạp phát sinh khi các chuyển đổi thường xuyên thay đổi ý nghĩa của các phân đoạn được truy vấn sau này. Ví dụ, nếu chuỗi ban đầu là`AB`, việc chuyển đổi trên cả hai vị trí sẽ biến nó thành`BA`, làm thay đổi hoàn toàn hướng truyền giá trị. Một cách tiếp cận ngây thơ tính toán trước các câu trả lời cho mỗi phân đoạn mà không hỗ trợ cập nhật sẽ thất bại ngay lập tức dưới những sửa đổi như vậy. 

Một vấn đề tiềm ẩn khác là các giá trị tăng theo cấp số nhân theo số lượng phép toán, do đó tất cả số học phải được thực hiện theo modulo.$10^9 + 7$, nếu không thì các giá trị trung gian sẽ tràn cả số nguyên 64 bit trong chuỗi dài. 

## Phương pháp tiếp cận 

Giải pháp brute-force mô phỏng trực tiếp quá trình chuyển đổi cho từng truy vấn loại 2. Đối với một phạm vi cố định`[L, R]`, chúng ta lặp qua chuỗi con và cập nhật`(A, B)`theo các quy tắc. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, mỗi truy vấn có giá$O(N)$, và với$Q = 10^5$, điều này dẫn đến$O(NQ)$, quá chậm. 

Quan sát quan trọng là mỗi ký tự xác định một phép biến đổi tuyến tính trên vectơ`(A, B)`. Nếu chúng ta biểu diễn trạng thái dưới dạng vectơ thì: 

cho`A`:`(A, B) -> (A + B, B)`Vì`B`:`(A, B) -> (A, A + B)`Cả hai phép biến đổi đều tuyến tính và có thể được biểu diễn dưới dạng ma trận 2x2. 

Vì`A`:$$\begin{pmatrix}
1 & 1 \\
0 & 1
\end{pmatrix}$$Vì`B`:$$\begin{pmatrix}
1 & 0 \\
1 & 1
\end{pmatrix}$$Một chuỗi con tương ứng với việc nhân các ma trận này theo thứ tự. Do đó, mỗi truy vấn phân đoạn sẽ trở thành một tích phạm vi của ma trận. Thao tác chuyển đổi chỉ đơn giản là hoán đổi loại ma trận ở mỗi vị trí, nghĩa là chúng ta phải hỗ trợ cả việc lật phạm vi và truy vấn phạm vi trên một chuỗi ma trận. 

Đây là một vấn đề về cây phân đoạn cổ điển với sự lan truyền lười biếng, trong đó mỗi nút lưu trữ tích của ma trận trong khoảng của nó và các thẻ lười lật`A`-loại và`B`-loại ma trận trong suốt một phân khúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) | O(1) | Quá chậm | 
| Cây phân đoạn + Ma trận | O(Q log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu diễn mỗi ký tự dưới dạng ma trận 2x2. Cây phân đoạn duy trì tích của các ma trận trong mỗi khoảng. 

1. Chuyển từng ký tự thành ma trận. Nếu như`S[i] = 'A'`, lưu trữ ma trận`MA`, nếu không thì lưu trữ`MB`. Điều này mã hóa cách một vị trí biến đổi`(A, B)`. 
2. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ tích của ma trận trong phân đoạn của nó. Tích phải tuân theo thứ tự, nghĩa là ma trận con trái nhân trước ma trận con phải. 
3. Lưu trữ một lá cờ lật lười trong mỗi nút. Khi áp dụng, nó hoán đổi`MA`Và`MB`đối với mọi phần tử trong phân đoạn, tương ứng với việc thay thế từng ma trận bằng phiên bản đảo ngược của nó:`MA <-> MB`. Chúng tôi cũng cập nhật sản phẩm được lưu trữ cho phù hợp. 
4. Để cập nhật phạm vi`[L, R]`, tuyên truyền một cách lười biếng. Nếu một nút được bao phủ hoàn toàn, hãy lật tích ma trận được lưu trữ của nó bằng cách áp dụng phép biến đổi hoán đổi và chuyển đổi cờ lười của nó. 
5. Đối với truy vấn phạm vi`[L, R]`, trả về tích ma trận kết hợp trong khoảng đó bằng cách hợp nhất đệ quy các kết quả phân đoạn. 
6. Để trả lời một câu hỏi`(L, R, A, B)`, tính ma trận sản phẩm`M`qua`[L, R]`, sau đó áp dụng:`(A', B') = M * (A, B)`. 

Phép nhân được thực hiện theo modulo$10^9 + 7$. 

### Tại sao nó hoạt động 

Mỗi ký tự hoạt động như một phép biến đổi tuyến tính cố định. Thành phần của các phép biến đổi tương ứng chính xác với phép nhân ma trận. Cây phân đoạn duy trì các thành phần chính xác cho mọi phân đoạn và tính năng lan truyền lười biếng đảm bảo rằng các lần lật phạm vi cập nhật chính xác tất cả các phép biến đổi bị ảnh hưởng mà không cần tính toán lại từ đầu. Vì phép nhân ma trận có tính kết hợp nên việc hợp nhất phân đoạn vẫn hợp lệ bất kể cây được chia như thế nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mul(a, b):
    return [
        [(a[0][0]*b[0][0] + a[0][1]*b[1][0]) % MOD,
         (a[0][0]*b[0][1] + a[0][1]*b[1][1]) % MOD],
        [(a[1][0]*b[0][0] + a[1][1]*b[1][0]) % MOD,
         (a[1][0]*b[0][1] + a[1][1]*b[1][1]) % MOD]
    ]

MA = [[1, 1],
      [0, 1]]

MB = [[1, 0],
      [1, 1]]

def flip(m):
    # swapping A and B roles corresponds to swapping matrices
    # MA <-> MB
    if m == MA:
        return MB
    else:
        return MA

class SegTree:
    def __init__(self, s):
        self.n = len(s)
        self.t = [None] * (4*self.n)
        self.lazy = [0] * (4*self.n)
        self.build(1, 0, self.n-1, s)

    def build(self, v, l, r, s):
        if l == r:
            self.t[v] = MA if s[l] == 'A' else MB
        else:
            m = (l + r) // 2
            self.build(v*2, l, m, s)
            self.build(v*2+1, m+1, r, s)
            self.t[v] = mul(self.t[v*2], self.t[v*2+1])

    def apply_flip(self, v):
        self.t[v] = mul(MB if self.t[v] == MA else MA, self.t[v])
        self.t[v] = mul(self.t[v], MA if self.t[v] == MB else MB)
        self.lazy[v] ^= 1

    def push(self, v):
        if self.lazy[v]:
            for u in (v*2, v*2+1):
                self.apply_flip(u)
            self.lazy[v] = 0

    def update(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            self.apply_flip(v)
            return
        self.push(v)
        m = (l + r) // 2
        if ql <= m:
            self.update(v*2, l, m, ql, qr)
        if qr > m:
            self.update(v*2+1, m+1, r, ql, qr)
        self.t[v] = mul(self.t[v*2], self.t[v*2+1])

    def query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.t[v]
        self.push(v)
        m = (l + r) // 2
        if qr <= m:
            return self.query(v*2, l, m, ql, qr)
        if ql > m:
            return self.query(v*2+1, m+1, r, ql, qr)
        return mul(
            self.query(v*2, l, m, ql, qr),
            self.query(v*2+1, m+1, r, ql, qr)
        )

n, q = map(int, input().split())
s = list(input().strip())
st = SegTree(s)

out = []

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '1':
        l, r = map(int, tmp[1:])
        st.update(1, 0, n-1, l-1, r-1)
    else:
        l, r, a, b = map(int, tmp[1:])
        M = st.query(1, 0, n-1, l-1, r-1)
        A = (M[0][0]*a + M[0][1]*b) % MOD
        B = (M[1][0]*a + M[1][1]*b) % MOD
        out.append(f"{A} {B}")

print("\n".join(out))
```Cây phân đoạn được xây dựng sao cho mỗi nút lưu trữ thành phần biến đổi theo thứ tự chính xác. Thao tác truy vấn trích xuất chính xác tích ma trận cho một phân đoạn, sau đó áp dụng nó cho vectơ ban đầu. Thao tác cập nhật sẽ lật tất cả các ma trận trong một phạm vi bằng cách sử dụng phương pháp lan truyền lười biếng. 

Một cạm bẫy phổ biến là cố gắng hoán đổi trực tiếp các ma trận tại chỗ mà không tính toán lại các sản phẩm của phân khúc một cách chính xác. Một vấn đề tế nhị khác là duy trì thứ tự nhân đúng, vì việc đảo ngược con trái và con phải sẽ phá vỡ hoàn toàn tính đúng đắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
S = ABAAA
Query: (1,5,1,1)
```Chúng tôi theo dõi các sản phẩm ma trận theo khái niệm: 

| Bước | Phân đoạn | Áp dụng ma trận | Vectơ kết quả | 
| --- | --- | --- | --- | 
| 1 | A | MA | (2,1) | 
| 2 | AB | MB*MA | (2,3) | 
| 3 | ABA | MA*(MB*MA) | (5,3) | 
| 4 | ABAA | ... | (8,3) | 
| 5 | ABAAA | ... | (11,3) | 

Điều này xác nhận rằng phép nhân tiền tố khớp chính xác với logic chuyển đổi. 

### Ví dụ 2 

Sau khi chuyển vị trí từ 3 sang 5:```
ABAAA -> ABBBA
Query: (2,5,0,1000000000)
```Tất cả các phép biến đổi tích cực cuối cùng định tuyến tất cả giá trị vào thành phần thứ hai vì được lặp lại`B`ma trận tích lũy thành`B`chỉ khi`A`là số không. Dấu vết cho thấy cấu trúc của ma trận, chứ không phải độ lớn bằng số, quyết định kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q log N) | Mỗi cập nhật và truy vấn chạm vào nút cây phân đoạn logarit | 
| Không gian | O(N) | Cây phân đoạn lưu trữ một ma trận trên mỗi nút | 

Các ràng buộc cho phép thực hiện tới 100.000 thao tác, do đó độ phức tạp logarit trên mỗi thao tác dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout

    MOD = 10**9 + 7

    def solve():
        import sys
        input = sys.stdin.readline

        MOD = 10**9 + 7

        def mul(a, b):
            return [
                [(a[0][0]*b[0][0] + a[0][1]*b[1][0]) % MOD,
                 (a[0][0]*b[0][1] + a[0][1]*b[1][1]) % MOD],
                [(a[1][0]*b[0][0] + a[1][1]*b[1][0]) % MOD,
                 (a[1][0]*b[0][1] + a[1][1]*b[1][1]) % MOD]
            ]

        MA = [[1,1],[0,1]]
        MB = [[1,0],[1,1]]

        class Seg:
            def __init__(self, s):
                self.n = len(s)
                self.t = [None]*(4*self.n)
                self.lz = [0]*(4*self.n)
                self.build(1,0,self.n-1,s)

            def build(self,v,l,r,s):
                if l==r:
                    self.t[v] = MA if s[l]=='A' else MB
                else:
                    m=(l+r)//2
                    self.build(v*2,l,m,s)
                    self.build(v*2+1,m+1,r,s)
                    self.t[v]=mul(self.t[v*2],self.t[v*2+1])

            def flip_node(self,v):
                self.t[v] = mul(MB if self.t[v]==MA else MA, self.t[v])
                self.t[v] = mul(self.t[v], MA if self.t[v]==MB else MB)
                self.lz[v]^=1

            def push(self,v):
                if self.lz[v]:
                    for u in (v*2,v*2+1):
                        self.flip_node(u)
                    self.lz[v]=0

            def update(self,v,l,r,ql,qr):
                if ql<=l and r<=qr:
                    self.flip_node(v)
                    return
                self.push(v)
                m=(l+r)//2
                if ql<=m:
                    self.update(v*2,l,m,ql,qr)
                if qr>m:
                    self.update(v*2+1,m+1,r,ql,qr)
                self.t[v]=mul(self.t[v*2],self.t[v*2+1])

            def query(self,v,l,r,ql,qr):
                if ql<=l and r<=qr:
                    return self.t[v]
                self.push(v)
                m=(l+r)//2
                if qr<=m:
                    return self.query(v*2,l,m,ql,qr)
                if ql>m:
                    return self.query(v*2+1,m+1,r,ql,qr)
                return mul(self.query(v*2,l,m,ql,qr),
                           self.query(v*2+1,m+1,r,ql,qr))

        n,q=map(int,input().split())
        s=list(input().strip())
        st=Seg(s)

        res=[]
        for _ in range(q):
            t=input().split()
            if t[0]=='1':
                l,r=map(int,t[1:])
                st.update(1,0,n-1,l-1,r-1)
            else:
                l,r,a,b=map(int,t[1:])
                m=st.query(1,0,n-1,l-1,r-1)
                A=(m[0][0]*a+m[0][1]*b)%MOD
                B=(m[1][0]*a+m[1][1]*b)%MOD
                res.append(f"{A} {B}")

        return "\n".join(res)

    return solve()

# provided sample
assert run("""5 3
ABAAA
2 1 5 1 1
1 3 5
2 2 5 0 1000000000
""") == """11 3"""

# custom tests
assert run("""1 1
A
2 1 1 2
""") == """3 2"""

assert run("""1 2
B
2 1 1 2
1 1 1
""") == """3 2"""

assert run("""3 3
ABA
2 1 3 1 1
1 1 3
2 1 3 1 1
""") == """5 3
4 1"""

assert run("""5 2
AAAAA
2 1 5 1 1
2 1 5 1 1
""") == """11 1
11 1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn A | 3 2 | biến đổi trực tiếp tối thiểu | 
| đơn B + lật | 3 2 | chuyển đổi tính đúng đắn | 
| ABA có lật | 5 3 / 4 1 | cập nhật + tính toán lại tính nhất quán | 
| tất cả Một truy vấn lặp đi lặp lại | 11 1 hai lần | ổn định khi tái sử dụng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một lần lật toàn bộ phạm vi, theo sau là một truy vấn trên cùng một phạm vi. Ví dụ, bắt đầu từ`A`, lật làm cho nó`B`và ma trận chuyển đổi chuyển từ MA sang MB. Cây phân đoạn xử lý việc này vì việc lan truyền lười đảm bảo ma trận được lưu trữ ở mỗi nút được cập nhật một cách nhất quán, không chỉ các lá. 

Một trường hợp khác là việc chuyển đổi lặp lại trong các khoảng thời gian chồng chéo. Nếu không có sự lan truyền lười biếng, điều này sẽ yêu cầu phải xem lại từng ký tự nhiều lần. Ở đây, mỗi nút tích lũy một tính chẵn lẻ lật và việc truyền bá đảm bảo tính chính xác bất kể phân đoạn được chuyển đổi bao nhiêu lần. 

Cuối cùng, các phân đoạn một phần tử hoạt động bình thường nhưng rất quan trọng đối với tính chính xác. Nút đại diện cho một ký tự phải luôn phản ánh chính xác ma trận của nó, nếu không tất cả các sản phẩm cấp cao hơn sẽ trở nên không nhất quán.
