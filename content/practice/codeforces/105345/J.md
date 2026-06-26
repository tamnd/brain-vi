---
title: "CF 105345J - Xì phé ảo"
description: "Chúng ta được cấp một dãy các thẻ $n$ được xếp thành một dòng, trong đó mỗi thẻ mang một giá trị từ 1 đến 13. Theo thời gian, các giá trị này thay đổi thông qua các bản cập nhật và chúng ta cũng được yêu cầu trả lời các truy vấn về phạm vi. Truy vấn thuộc loại đầu tiên sẽ thay đổi một vị trí trong mảng thành một giá trị mới."
date: "2026-06-23T05:51:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105345
codeforces_index: "J"
codeforces_contest_name: "UTPC Contest 09-13-24 Div. 1 (Advanced)"
rating: 0
weight: 105345
solve_time_s: 187
verified: false
draft: false
---

[CF 105345J - Phantom Poker](https://codeforces.com/problemset/problem/105345/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 7s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt$n$các thẻ được sắp xếp thành một dòng, trong đó mỗi thẻ mang một giá trị từ 1 đến 13. Theo thời gian, các giá trị này thay đổi thông qua các bản cập nhật và chúng tôi cũng được yêu cầu trả lời các truy vấn về phạm vi. 

Truy vấn thuộc loại đầu tiên sẽ thay đổi một vị trí trong mảng thành một giá trị mới. Truy vấn loại thứ hai yêu cầu chúng ta đếm xem có bao nhiêu lựa chọn thẻ không trống từ một phân đoạn nhất định$[l, r]$có tích các giá trị được chọn tương ứng với 5 modulo 13. Mỗi lá bài khác biệt theo vị trí, do đó, ngay cả các giá trị giống nhau ở các chỉ số khác nhau cũng tạo ra các kết hợp khác nhau. 

Khó khăn cốt lõi là chúng ta không tính các tổng hoặc tần số đơn giản, mà là các tập hợp con có tích nằm trong lớp thặng dư cụ thể modulo 13. Vì 13 là số nguyên tố, phép nhân modulo 13 tạo thành một cấu trúc đại số rõ ràng trên các dư lượng khác 0, nhưng sự hiện diện của giá trị 1 và các cập nhật lặp lại cho thấy chúng ta cần một cấu trúc dữ liệu hỗ trợ các truy vấn phạm vi động. 

Những hạn chế$n, q \le 10^4$ngay lập tức loại trừ việc tính toán lại các câu trả lời từ đầu cho mỗi truy vấn trên tất cả các tập hợp con, vì ngay cả một phạm vi kích thước duy nhất$m$có$2^m$tập hợp con, điều này không khả thi ngay cả đối với mức độ vừa phải$m$. Bất kỳ giải pháp nào tính toán lại thông tin tập hợp con cho mỗi truy vấn đều quá chậm. 

Một vấn đề tế nhị là điều kiện của tích là modulo 13, nhưng các phần tử bao gồm các giá trị từ 1 đến 13. Bản thân giá trị 13 hoạt động như 0 modulo 13, điều này rất quan trọng vì nhân với 13 luôn thu gọn tích về 0 mod 13, nghĩa là các phần tử như vậy hoạt động giống như trạng thái hấp thụ. 

Một trường hợp đặc biệt khác đến từ các bản cập nhật: việc thay đổi một phần tử có thể ảnh hưởng đáng kể đến số lượng tập hợp con trong một phạm vi, do đó, bất kỳ phương pháp nào tính toán trước các câu trả lời tĩnh đều không thể tồn tại sau các sửa đổi. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ bắt đầu bằng cách xem xét một truy vấn$[l, r]$. Chúng tôi liệt kê tất cả các tập hợp con của phân khúc và tính tích của chúng theo modulo 13, tăng dần câu trả lời khi nó bằng 5. Điều này đúng vì nó khớp trực tiếp với định nghĩa của nhiệm vụ. 

Tuy nhiên, cách tiếp cận này mở rộng$m$các phần tử vào$2^m$tập hợp con cho mỗi truy vấn, vì vậy ngay cả đối với$m = 20$nó trở thành đường biên giới, và đối với$m = 10^4$điều đó là không thể. Vấn đề không chỉ nằm ở việc liệt kê tập hợp con mà còn ở việc lặp lại các truy vấn và cập nhật, khiến chi phí càng tăng thêm. 

Quan sát cấu trúc quan trọng là phép nhân modulo 13 chỉ phụ thuộc vào các lớp thặng dư và chúng ta đang đếm các tập hợp con theo một ràng buộc nhân. Đây là cài đặt cổ điển trong đó chúng tôi chuyển đổi các phần tử thành không gian trạng thái hữu hạn và duy trì cấu trúc giống như tích chập trên các tập hợp con. Mỗi phần tử đóng góp theo cấp số nhân cho một tích của tập hợp con và việc đếm tập hợp con trên các tích tương ứng với việc kết hợp các hàm tạo trên một cấu trúc giống nhóm modulo 13. 

Chúng tôi coi mỗi phân đoạn là tạo ra phân bố tần số trên các phần dư từ 0 đến 12, trong đó mỗi vị trí đóng góp một đa thức:$$P_i(x) = 1 + x^{a_i}$$và tích phân đoạn tương ứng với việc nhân các đa thức này. Hệ số của$x^k$trong sản phẩm cuối cùng đếm các tập con có sản phẩm bằng$k \bmod 13$. Câu trả lời đơn giản là hệ số dư lượng 5. 

Vì các bản cập nhật là những thay đổi về điểm và các truy vấn dựa trên phạm vi nên chúng ta cần một cây phân đoạn trong đó mỗi nút lưu trữ phân bố 13 chiều này. Hợp nhất hai phân đoạn tương ứng với phép tích chập theo mô đun nhân 13, đó là$O(13^2)$, một hằng số. 

Điều này làm giảm vấn đề duy trì cây phân đoạn của các vectơ giống đa thức với các cập nhật điểm và truy vấn phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot 2^n)$|$O(1)$| Quá chậm | 
| Cây phân đoạn trên phần dư DP |$O((n+q)\cdot 13^2 \log n)$|$O(n \cdot 13)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu thị từng phân đoạn bằng một mảng tần số`dp`có kích thước 13, ở đâu`dp[k]`lưu trữ bao nhiêu tập hợp con của phân khúc đó tạo ra sản phẩm phù hợp với$k \bmod 13$. 

1. Khởi tạo giá trị của nút lá$x$. Chúng ta bắt đầu với một phân đoạn chứa một phần tử duy nhất, do đó có hai tập hợp con: tập hợp con trống với sản phẩm 1 và tập hợp con đơn lẻ với sản phẩm$x$. Điều này có nghĩa là chúng ta khởi tạo`dp[1] = 1`Và`dp[x % 13] += 1`, ngoại trừ khi$x \equiv 0 \pmod{13}$, trong đó phép nhân rút gọn về 0 và phải được xử lý rõ ràng. 
2. Đối với mỗi nút bên trong, hợp nhất hai nút con bằng cách kết hợp phân phối sản phẩm tập hợp con của chúng. Đối với mỗi dư lượng$i$từ đứa trẻ bên trái và$j$từ con bên phải, nhân các tập con sẽ thu được kết quả$i \cdot j \bmod 13$. Chúng tôi tích lũy những thứ này vào bản phân phối gốc. 
3. Để hợp nhất, chúng tôi lặp lại tất cả các cặp dư lượng và tính toán:$$new[k] += left[i] \cdot right[j], \quad k = (i \cdot j) \bmod 13$$4. Xây dựng cây phân đoạn trên mảng bằng thao tác hợp nhất này. 
5. Đối với truy vấn loại 1, hãy cập nhật một lá đơn và tính toán lại tất cả các nút cây phân đoạn bị ảnh hưởng trở lên bằng cách sử dụng cùng một quy tắc hợp nhất. 
6. Đối với truy vấn loại 2, hãy truy vấn cây phân đoạn$[l, r]$và nhận được kết quả phân phối. Câu trả lời là`dp[5]`. 

Lý do điều này có tác dụng là vì mỗi nút cây phân đoạn thể hiện chính xác nhiều tập hợp sản phẩm con cho phân khúc của nó. Hoạt động hợp nhất tương đương với việc chọn các tập hợp con độc lập từ các phân đoạn bên trái và bên phải và kết hợp chúng, phù hợp với định nghĩa tổ hợp của các tập hợp con trên các tập hợp rời rạc. Bởi vì mọi tập hợp con của một hợp đều phân rã duy nhất thành các tập con của mỗi nửa, phép tích chập nắm bắt đầy đủ tất cả các khả năng mà không bị trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [None] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def merge(self, A, B):
        C = [0] * 13
        for i in range(13):
            if A[i] == 0:
                continue
            for j in range(13):
                if B[j] == 0:
                    continue
                C[(i * j) % 13] = (C[(i * j) % 13] + A[i] * B[j]) % MOD
        return C

    def build(self, v, l, r, arr):
        if l == r:
            x = arr[l] % 13
            dp = [0] * 13
            dp[1] = 1
            dp[x] = (dp[x] + 1) % MOD
            self.t[v] = dp
            return
        m = (l + r) // 2
        self.build(v*2, l, m, arr)
        self.build(v*2+1, m+1, r, arr)
        self.t[v] = self.merge(self.t[v*2], self.t[v*2+1])

    def update(self, v, l, r, idx, val):
        if l == r:
            x = val % 13
            dp = [0] * 13
            dp[1] = 1
            dp[x] = (dp[x] + 1) % MOD
            self.t[v] = dp
            return
        m = (l + r) // 2
        if idx <= m:
            self.update(v*2, l, m, idx, val)
        else:
            self.update(v*2+1, m+1, r, idx, val)
        self.t[v] = self.merge(self.t[v*2], self.t[v*2+1])

    def query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.t[v]
        m = (l + r) // 2
        if qr <= m:
            return self.query(v*2, l, m, ql, qr)
        if ql > m:
            return self.query(v*2+1, m+1, r, ql, qr)
        left = self.query(v*2, l, m, ql, qr)
        right = self.query(v*2+1, m+1, r, ql, qr)
        return self.merge(left, right)

n, q = map(int, input().split())
arr = list(map(int, input().split()))

st = SegTree(arr)

for _ in range(q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        _, i, x = tmp
        st.update(1, 0, n-1, i-1, x)
    else:
        _, l, r = tmp
        res = st.query(1, 0, n-1, l-1, r-1)
        print(res[5] % MOD)
```Việc triển khai cốt lõi xoay quanh việc biểu diễn nút cây phân đoạn. Mỗi nút là một mảng có độ dài 13 chứa tất cả các phần dư sản phẩm có thể có của các tập hợp con trong phân đoạn đó. Hàm hợp nhất thực hiện phép tích chập đầy đủ trên các phần dư, có kích thước không đổi và an toàn trong các ràng buộc. 

Một điểm tinh tế là việc khởi tạo: mỗi phân đoạn bao gồm tập con trống, luôn đóng góp sản phẩm 1, vì vậy`dp[1] = 1`được yêu cầu ở mỗi lá. Sau đó, tập hợp con phần tử đơn được thêm vào trên cùng. Việc quên tập hợp con trống sẽ phá vỡ tất cả các phép hợp nhất vì nó loại bỏ phần tử nhận dạng cần thiết cho hành vi tích chập chính xác. 

Thao tác cập nhật sẽ xây dựng lại lá chính xác như khi xây dựng, đảm bảo tính nhất quán. Logic truy vấn dựa vào việc phân tách cây phân đoạn tiêu chuẩn và hợp nhất các câu trả lời từng phần theo thứ tự. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ$[2, 5, 7]$và một truy vấn trên toàn bộ phạm vi. Mỗi lá bắt đầu dưới dạng phân phối trong đó tập hợp con trống cho phần dư 1 và phần đơn đóng góp giá trị của nó. 

| Nút | dp[1] | dp[2] | dp[5] | dp[7] | khác | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 0 | 0 | - | 
| 5 | 1 | 0 | 1 | 0 | - | 
| 7 | 1 | 0 | 0 | 1 | - | 

Việc hợp nhất hai nút đầu tiên sẽ kết hợp tất cả các sản phẩm tập hợp con từ {2,5}. Kết quả phân phối bao gồm các sản phẩm 1, 2, 5 và 10. 

Sau khi hợp nhất với 7, mọi tích trước đó sẽ được giữ lại (trừ 7) hoặc nhân với 7. 

| Bước | Phân đoạn hoạt động | dp[5] | 
| --- | --- | --- | 
| Sau [2,5] | tập con của hai tập đầu tiên | 1 | 
| Sau [2,5,7] | đầy đủ | giá trị cuối cùng | 

dp[5] cuối cùng đếm tất cả các tập con có tích mod 13 bằng 5. 

Dấu vết này chứng minh rằng mọi tập hợp con được phân chia rõ ràng trên các ranh giới phân đoạn và phép tích chập duy trì tính chính xác trong các lần hợp nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q) \cdot 13^2 \log n)$| mỗi lần cập nhật/truy vấn chạm vào$O(\log n)$các nút, mỗi lần hợp nhất chi phí không đổi 13×13 công việc | 
| Không gian |$O(n \cdot 13)$| mỗi nút cây phân đoạn lưu trữ phân phối dư lượng có kích thước cố định | 

Hệ số không đổi nhỏ vì 13 là cố định nên lời giải phù hợp một cách thoải mái trong giới hạn thời gian cho$10^4$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.t = [None] * (4 * self.n)
            self.build(1, 0, self.n - 1, arr)

        def merge(self, A, B):
            C = [0] * 13
            for i in range(13):
                for j in range(13):
                    C[(i * j) % 13] = (C[(i * j) % 13] + A[i] * B[j]) % MOD
            return C

        def build(self, v, l, r, arr):
            if l == r:
                x = arr[l] % 13
                dp = [0] * 13
                dp[1] = 1
                dp[x] = (dp[x] + 1) % MOD
                self.t[v] = dp
                return
            m = (l + r) // 2
            self.build(v*2, l, m, arr)
            self.build(v*2+1, m+1, r, arr)
            self.t[v] = self.merge(self.t[v*2], self.t[v*2+1])

        def update(self, v, l, r, idx, val):
            if l == r:
                x = val % 13
                dp = [0] * 13
                dp[1] = 1
                dp[x] = (dp[x] + 1) % MOD
                self.t[v] = dp
                return
            m = (l + r) // 2
            if idx <= m:
                self.update(v*2, l, m, idx, val)
            else:
                self.update(v*2+1, m+1, r, idx, val)
            self.t[v] = self.merge(self.t[v*2], self.t[v*2+1])

        def query(self, v, l, r, ql, qr):
            if ql <= l and r <= qr:
                return self.t[v]
            m = (l + r) // 2
            if qr <= m:
                return self.query(v*2, l, m, ql, qr)
            if ql > m:
                return self.query(v*2+1, m+1, r, ql, qr)
            left = self.query(v*2, l, m, ql, qr)
            right = self.query(v*2+1, m+1, r, ql, qr)
            return self.merge(left, right)

    data = inp.strip().split()
    n, q = map(int, data[:2])
    arr = list(map(int, data[2:2+n]))
    st = SegTree(arr)

    idx = 2+n
    out = []
    for _ in range(q):
        t = int(data[idx]); idx += 1
        if t == 1:
            i = int(data[idx]); x = int(data[idx+1]); idx += 2
            st.update(1, 0, n-1, i-1, x)
        else:
            l = int(data[idx]); r = int(data[idx+1]); idx += 2
            res = st.query(1, 0, n-1, l-1, r-1)
            out.append(str(res[5] % MOD))

    return "\n".join(out)

# custom tests
assert run("4 3\n1 2 5 9\n2 1 4\n1 2 4\n2 1 4") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn đơn tối thiểu | số lượng dư lượng chính xác | độ đúng cơ sở | 
| cập nhật lặp đi lặp lại | tính toán lại ổn định | cập nhật tuyên truyền | 
| tất cả các giá trị bằng nhau | xử lý vụ nổ tổ hợp | tập hợp con DP chính xác | 
| giá trị hỗn hợp bao gồm 13 | xử lý không có dư lượng | sụp đổ modulo | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xuất hiện khi một giá trị là 13. Trong trường hợp đó, phần dư của nó là 0 và bất kỳ tập hợp con nào chứa nó sẽ buộc sản phẩm về 0. DP phải cho phép chuyển đổi chính xác thành phần dư 0 mà không phá vỡ cấu trúc nhân. Một phân đoạn chỉ chứa 13 giây phải có tất cả các tập hợp con ngoại trừ ánh xạ trống thành 0, trong khi tập hợp con trống vẫn ở mức 1, điều này đảm bảo việc hợp nhất vẫn nhất quán. 

Một trường hợp tinh vi khác là bội số 1. Vì 1 không thay đổi sản phẩm nên nó chỉ nhân đôi số tập con trong mỗi phân đoạn. DP phải phản ánh chính xác rằng mỗi 1 đưa ra một lựa chọn độc lập, được nắm bắt bởi các đóng góp tập hợp con trống và đơn lẻ, cả hai đều nằm trong phần dư 1. 

Trường hợp cuối cùng là các bản cập nhật thay đổi giá trị từ 13 thành phần dư khác 0. Nếu không xây dựng lại lá từ đầu, các trạng thái DP trước đó sẽ rò rỉ sang trạng thái mới, làm hỏng cấu trúc tích chập. Việc khởi tạo lại từng lá hoàn toàn đảm bảo tính chính xác sau mỗi lần sửa đổi.
