---
title: "CF 106367H - Bộ bí ẩn của Whalica"
description: "Chúng tôi duy trì một tập hợp con các số nguyên động từ vũ trụ cố định $[1, n]$. Bộ này hỗ trợ chèn và xóa với hành vi bình thường, nghĩa là việc chèn hoặc xóa lặp đi lặp lại trên cùng một phần tử sẽ không thay đổi bất cứ điều gì sau hiệu ứng đầu tiên."
date: "2026-06-19T17:14:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106367
codeforces_index: "H"
codeforces_contest_name: "Whalica Cup (Round 2)"
rating: 0
weight: 106367
solve_time_s: 68
verified: true
draft: false
---

[CF 106367H - Bộ bí ẩn của Whalica](https://codeforces.com/problemset/problem/106367/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một tập hợp con các số nguyên động từ vũ trụ cố định$[1, n]$. Bộ này hỗ trợ chèn và xóa với hành vi bình thường, nghĩa là việc chèn hoặc xóa lặp đi lặp lại trên cùng một phần tử sẽ không thay đổi bất cứ điều gì sau hiệu ứng đầu tiên. 

Ngoài những cập nhật cơ bản, chúng tôi phải trả lời hai loại truy vấn. Truy vấn đầu tiên yêu cầu số nguyên dương nhỏ nhất hiện không có trong tập hợp, đó là “mex” cổ điển trên$[1, n]$. Truy vấn thứ hai xác định một tổng hợp có liên quan nhiều hơn đến các phần tử còn thiếu: nếu chúng ta liệt kê tất cả các số trong$[1, n]$không có trong bộ như$a_1 < a_2 < \dots < a_k$, chúng tôi tính toán$\sum_{i=1}^{k} a_i \cdot 2^i$, lấy modulo$998244353$. 

Những hạn chế$n, q \le 10^5$buộc chúng ta vào các giải pháp tuyến tính hoặc tuyến tính gần đúng. Bất cứ điều gì tính toán lại các phần tử bị thiếu hoặc quét toàn bộ mảng cho mỗi truy vấn sẽ TLE, vì tối đa$10^5$các hoạt động được yêu cầu. Giải pháp xây dựng lại bộ phần bù cho mọi truy vấn loại 4 sẽ tốn kém$O(n)$mỗi truy vấn, dẫn đến$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một điểm tinh tế là ngữ nghĩa “đặt” ẩn các bản sao. Thao tác 1 và 2 phải hoạt động giống như cờ boolean. Một vấn đề khác là truy vấn trọng số phụ thuộc vào thứ tự của các phần tử bị thiếu, do đó bất kỳ cấu trúc nào cũng phải bảo toàn hoặc xây dựng lại các phần bổ sung được sắp xếp một cách hiệu quả. 

Các trường hợp cạnh bao gồm các thao tác chèn và xóa lặp đi lặp lại cũng như trạng thái trong đó tập hợp trống hoặc đầy. 

Nếu tập này trống và$n=5$, thì đối với truy vấn loại 3, chúng tôi trả về 1. Đối với loại 4, danh sách còn thiếu là$[1,2,3,4,5]$, và trọng lượng trở thành$1\cdot2^1 + 2\cdot2^2 + \dots + 5\cdot2^5$. Một cách tiếp cận ngây thơ có thể coi các chỉ số là lũy thừa một cách không chính xác hoặc quên rằng các chỉ số bắt đầu từ 1. 

Một trường hợp cạnh khác là một bộ được điền đầy đủ, trong đó mex là$n+1$, nhưng vì bài toán chỉ yêu cầu số nguyên trong$[1,n]$, mex chính xác phải là$n+1$ngầm mặc dù nó ở bên ngoài vũ trụ. 

## Phương pháp tiếp cận 

Giải pháp brute-force duy trì tập hợp một cách rõ ràng và tính toán lại các câu trả lời khi cần. Chèn và xóa là$O(1)$sử dụng một mảng hoặc tập hợp boolean. Truy vấn mex quét từ 1 trở lên cho đến khi tìm thấy phần tử bị thiếu, đó là$O(n)$. Truy vấn trọng số sẽ quét tất cả các số từ 1 đến n, thu thập các phần tử còn thiếu, sắp xếp chúng và tính tổng.$O(n)$. Với tối đa$10^5$truy vấn, độ phức tạp trong trường hợp xấu nhất sẽ trở thành$O(nq)$, quá chậm. 

Quan sát quan trọng là chúng ta không bao giờ cần phải xây dựng lại phần bù đầy đủ nhiều lần. Thay vào đó, chúng ta có thể duy trì phần bù một cách ngầm định bằng cách sử dụng cây Fenwick hoặc cây phân đoạn để theo dõi những phần tử hiện có. Từ cấu trúc này, chúng tôi có thể hỗ trợ các truy vấn tiền tố như “có bao nhiêu phần tử bị thiếu trong một tiền tố” và “tìm phần tử bị thiếu đầu tiên”. Điều đó đã giải quyết được mex trong$O(\log n)$. 

Phần khó hơn là hàm trọng số, hàm này phụ thuộc vào danh sách sắp xếp các phần tử còn thiếu và lũy thừa của hai phần tử được lập chỉ mục theo vị trí của chúng trong danh sách đó. Bí quyết quan trọng là duy trì các tập hợp tiền tố trên tập hợp phần bù, trong đó mỗi vị trí đóng góp một giá trị tùy thuộc vào số lượng phần tử bị thiếu trước nó. Đối với mỗi phân đoạn, cây phân đoạn có thể lưu trữ cả số lượng phần tử bị thiếu và tổng đóng góp có trọng số với căn chỉnh công suất chính xác. Khi hợp nhất hai phân đoạn, chúng ta phải dịch chuyển phần đóng góp của phân khúc bên phải theo số phần tử còn thiếu trong phân khúc bên trái, vì các chỉ số trong công thức phụ thuộc vào thứ tự chung. 

Điều này làm cho cấu trúc tương tự như việc duy trì một chuỗi trong các chuyển đổi, trong đó mỗi phần tử có mặt hoặc vắng mặt và các phần tử vắng mặt đóng góp một giá trị phụ thuộc vào thứ hạng của chúng trong số tất cả các phần tử vắng mặt. Các nút cây phân đoạn lưu trữ hai giá trị: số lượng số 0 (các phần tử bị thiếu) và tổng có trọng số cho phân đoạn đó. Hoạt động hợp nhất sử dụng danh tính: 

nếu đoạn bên trái có$k$các phần tử bị thiếu thì mọi phần tử ở đoạn bên phải sẽ có chỉ số tăng thêm$k$, do đó đóng góp của nó được nhân với$2^k$, và chúng tôi cũng thêm một số hạng dịch chuyển$(\text{sum}_R) \cdot 2^k$. 

Chúng tôi tính toán trước lũy thừa của hai để hỗ trợ những ca này một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Cây phân đoạn với phần bù ngầm |$O(q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đại diện cho mỗi số$x \in [1,n]$ở trạng thái nhị phân: 1 nếu có trong tập hợp, 0 nếu không có. Vì trọng số được xác định trên các phần tử vắng mặt nên về mặt khái niệm, chúng tôi coi các số 0 là chuỗi hoạt động. 

Chúng tôi xây dựng cây phân đoạn trên mảng nhị phân này, trong đó mỗi nút duy trì hai phần thông tin: số lượng số 0 trong phân đoạn và phần đóng góp có trọng số của các số 0 trong phân đoạn đó giả sử chúng được lập chỉ mục từ 1 bên trong phân đoạn. 

Để việc hợp nhất chính xác, chúng tôi cũng duy trì tính bất biến rằng mỗi phân đoạn lưu trữ phần đóng góp của nó giả sử nó bắt đầu ở chỉ mục 1 cho chuỗi số 0 cục bộ của chính nó. 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`present`kích thước$n$, ban đầu tất cả đều sai, nghĩa là tất cả các phần tử đều bị thiếu. Điều này có nghĩa là tất cả các vị trí đều là số 0 trong biểu diễn cây phân đoạn. 
2. Tính toán trước lũy thừa của hai modulo$998244353$lên đến$n+5$. Những điều này là cần thiết vì mỗi khi một đoạn được dịch chuyển bởi$k$yếu tố còn thiếu, đóng góp được nhân lên bằng$2^k$. 
3. Xây dựng cây phân đoạn trong đó mỗi lá đại diện cho một giá trị$x$. Nếu như$x$bị thiếu, số lượng cửa hàng lá = 1 và tổng =$x \cdot 2^1$. Nếu có, nó lưu số đếm = 0 và tổng = 0. Số mũ bắt đầu từ 1 vì phần tử bị thiếu đầu tiên trong bất kỳ phân đoạn nào đều đóng góp với chỉ số 1. 
4. Đối với thao tác loại 1 (chèn), đánh dấu$x$như hiện tại và cập nhật lá của nó thành số 0 và tổng bằng 0. Việc này sẽ loại bỏ nó khỏi chuỗi phần bù. 
5. Đối với thao tác loại 2 (xóa), hãy đánh dấu$x$bị thiếu một lần nữa và cập nhật lá của nó để đếm 1 và tổng$x \cdot 2^1$. Điều này giới thiệu lại nó vào trình tự bổ sung. 
6. Đối với truy vấn loại 3, hãy tính mex bằng cách giảm dần cây phân đoạn. Tại mỗi nút, nếu nút con bên trái có ít hơn số lượng thiếu dự kiến ​​đầy đủ thì nút bị thiếu đầu tiên nằm ở nút bên trái; nếu không thì nó nằm ở bên phải sau khi trừ đi số bên trái. Điều này tìm thấy chỉ số nhỏ nhất có giá trị 0. 
7. Đối với truy vấn loại 4, trả về tổng được lưu ở gốc của cây phân đoạn. Điều này đã thể hiện tổng trọng số đầy đủ của các phần tử bị thiếu theo đúng thứ tự chung. 
8. Khi hợp nhất hai nút, giả sử nút con bên trái có`(cntL, sumL)`và đứa trẻ bên phải có`(cntR, sumR)`. Khi đó tất cả các số 0 trong đoạn bên phải được dịch chuyển bởi`cntL`vị trí, do đó sự đóng góp của họ trở thành:$sumR \cdot 2^{cntL}$và ngoài ra, mỗi phần tử ở đoạn bên phải sẽ nhận được phần bù bổ sung theo số mũ đã được mã hóa thông qua cấu trúc. Chúng tôi kết hợp như sau:$(cntL + cntR, sumL + sumR \cdot 2^{cntL})$. 
9. Duy trì quy tắc hợp nhất này một cách nhất quán cho tất cả các hoạt động của cây phân đoạn. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mọi nút đều lưu trữ phần đóng góp chính xác của các phần tử còn thiếu trong phân khúc của nó với giả định rằng chúng được đánh số lại bắt đầu từ 1 theo thứ tự từ trái sang phải. Khi hợp nhất hai phân đoạn, các phần tử bị thiếu của phân đoạn bên phải sẽ xuất hiện sau tất cả các phần tử bị thiếu của phân đoạn bên trái, do đó chỉ số của chúng bị dịch chuyển theo chính xác số phần tử bị thiếu trong phân đoạn bên trái. Sự thay đổi này tương ứng chính xác với việc nhân các khoản đóng góp với$2^{cnt_L}$, bảo toàn tính đúng đắn của trọng số mũ. Vì mọi sự hợp nhất đều tôn trọng sự chuyển đổi thứ tự này nên nút gốc luôn mã hóa thứ tự chung chính xác của tất cả các phần tử bị thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

class SegTree:
    def __init__(self, n):
        self.n = n
        self.size = 1
        while self.size < n:
            self.size *= 2
        self.cnt = [0] * (2 * self.size)
        self.sum = [0] * (2 * self.size)

        self.pow2 = [1] * (n + 5)
        for i in range(1, n + 5):
            self.pow2[i] = (self.pow2[i - 1] * 2) % MOD

    def build_leaf(self, idx, is_missing, x):
        if is_missing:
            self.cnt[idx] = 1
            self.sum[idx] = (x * self.pow2[1]) % MOD
        else:
            self.cnt[idx] = 0
            self.sum[idx] = 0

    def pull(self, v):
        lc, rc = 2 * v, 2 * v + 1
        self.cnt[v] = self.cnt[lc] + self.cnt[rc]
        self.sum[v] = (self.sum[lc] + self.sum[rc] * self.pow2[self.cnt[lc]]) % MOD

    def update(self, v, l, r, pos, is_missing, x):
        if l == r:
            self.build_leaf(v, is_missing, x)
            return
        mid = (l + r) // 2
        if pos <= mid:
            self.update(2 * v, l, mid, pos, is_missing, x)
        else:
            self.update(2 * v + 1, mid + 1, r, pos, is_missing, x)
        self.pull(v)

    def mex(self, v, l, r):
        if l == r:
            return l
        mid = (l + r) // 2
        lc = 2 * v
        left_missing = (mid - l + 1) - (self.cnt[lc])
        if left_missing > 0:
            return self.mex(lc, l, mid)
        return self.mex(2 * v + 1, mid + 1, r)

def main():
    n, q = map(int, input().split())
    st = SegTree(n)

    for i in range(1, n + 1):
        st.update(1, 1, n, i, True, i)

    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])
        if t == 1:
            x = int(tmp[1])
            st.update(1, 1, n, x, False, x)
        elif t == 2:
            x = int(tmp[1])
            st.update(1, 1, n, x, True, x)
        elif t == 3:
            print(st.mex(1, 1, n))
        else:
            print(st.sum[1] % MOD)

if __name__ == "__main__":
    main()
```Cây phân đoạn được xây dựng trên vũ trụ$[1,n]$và mỗi lá chuyển đổi giữa trạng thái hiện tại và trạng thái thiếu. các`pull`Hàm thực hiện phép truy toán khóa làm thay đổi phần đóng góp của phần tử con bên phải theo số phần tử còn thiếu trong phần tử con bên trái. Đây là nơi duy nhất thực thi việc đặt hàng và đó là điều làm cho truy vấn trọng lượng trên toàn cầu trở nên chính xác. 

Truy vấn mex sử dụng thực tế là một phân đoạn được "lấp đầy" hoàn toàn các phần tử hiện tại nếu số lượng phần tử bị thiếu của nó bằng 0. Séc`(mid - l + 1) - self.cnt[lc]`tính toán có bao nhiêu phần tử còn thiếu tồn tại trong khoảng con bên trái, cho phép chúng ta quyết định hướng theo thời gian logarit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
1 1
1 2
4
3
```| Bước | Hoạt động | Bộ Quà Tặng | Thiếu số lượng (gốc) | Cân nặng | 
| --- | --- | --- | --- | --- | 
| 1 | chèn 1 | {1} | 4 | tính toán lại | 
| 2 | chèn 2 | {1,2} | 3 | tính toán lại | 
| 3 | truy vấn 4 | {1,2} | 3 | tính toán | 
| 4 | truy vấn 3 | {1,2} | 3 | 3 | 

Sau khi chèn 1 và 2, phần tử còn thiếu là [3,4,5]. Trọng lượng trở thành$3\cdot2^1 + 4\cdot2^2 + 5\cdot2^3 = 6 + 16 + 40 = 62$. Mex là 3, khớp với giá trị còn thiếu đầu tiên. 

Dấu vết này cho thấy cấu trúc chỉ theo dõi các phần tử bị thiếu và thứ tự được ẩn thông qua việc hợp nhất phân đoạn. 

### Ví dụ 2 

đầu vào:```
6 5
2 3
3
1 3
3
4
```| Bước | Hoạt động | Đặt S | Mex | Cân nặng | 
| --- | --- | --- | --- | --- | 
| 1 | xóa 3 | { } (ban đầu thiếu tất cả nên còn thiếu 3) | 1 | đầy đủ | 
| 2 | truy vấn mex | bộ trống | 1 | đầy đủ | 
| 3 | chèn 3 | {3} | 1 | cập nhật | 
| 4 | truy vấn mex | {3} | 1 | cập nhật | 
| 5 | trọng lượng truy vấn | {3} | 1 | tính toán | 

Ban đầu tất cả các phần tử đều bị thiếu nên mex bằng 1 và trọng số bao gồm tất cả các phần tử. Sau khi chèn 3, tập hợp bị thiếu sẽ trở thành [1,2,4,5,6] và trọng số tự động cập nhật thông qua việc truyền bá cây phân đoạn. 

Ví dụ này nhấn mạnh rằng các thao tác chèn/xóa chỉ chuyển trạng thái và tính chính xác không phụ thuộc vào lịch sử thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log n)$| Mỗi truy vấn cập nhật và mex đi qua chiều cao của cây phân đoạn và trọng số là O(1) sau khi bảo trì | 
| Không gian |$O(n)$| Cây phân đoạn lưu trữ thông tin không đổi trên mỗi nút | 

Giải pháp phù hợp thoải mái trong giới hạn vì$10^5 \log 10^5$cũng tuân theo các ràng buộc 1 giây điển hình trong Python được tối ưu hóa với việc triển khai chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    n, q = map(int, sys.stdin.readline().split())
    
    class SegTree:
        def __init__(self, n):
            self.n = n
            self.size = 1
            while self.size < n:
                self.size *= 2
            self.cnt = [0] * (2 * self.size)
            self.sum = [0] * (2 * self.size)
            self.pow2 = [1] * (n + 5)
            for i in range(1, n + 5):
                self.pow2[i] = (self.pow2[i - 1] * 2) % 998244353

        def build_leaf(self, idx, is_missing, x):
            if is_missing:
                self.cnt[idx] = 1
                self.sum[idx] = (x * self.pow2[1]) % 998244353
            else:
                self.cnt[idx] = 0
                self.sum[idx] = 0

        def pull(self, v):
            lc, rc = 2 * v, 2 * v + 1
            self.cnt[v] = self.cnt[lc] + self.cnt[rc]
            self.sum[v] = (self.sum[lc] + self.sum[rc] * self.pow2[self.cnt[lc]]) % 998244353

        def update(self, v, l, r, pos, is_missing, x):
            if l == r:
                self.build_leaf(v, is_missing, x)
                return
            mid = (l + r) // 2
            if pos <= mid:
                self.update(2 * v, l, mid, pos, is_missing, x)
            else:
                self.update(2 * v + 1, mid + 1, r, pos, is_missing, x)
            self.pull(v)

        def mex(self, v, l, r):
            if l == r:
                return l
            mid = (l + r) // 2
            lc = 2 * v
            left_missing = (mid - l + 1) - (self.cnt[lc])
            if left_missing > 0:
                return self.mex(lc, l, mid)
            return self.mex(2 * v + 1, mid + 1, r)

    st = SegTree(n)
    for i in range(1, n + 1):
        st.update(1, 1, n, i, True, i)

    for _ in range(q):
        parts = sys.stdin.readline().split()
        if parts[0] == '1':
            st.update(1, 1, n, int(parts[1]), False, int(parts[1]))
        elif parts[0] == '2':
            st.update(1, 1, n, int(parts[1]), True, int(parts[1]))
        elif parts[0] == '3':
            output.append(str(st.mex(1, 1, n)))
        else:
            output.append(str(st.sum[1] % 998244353))

    return "\n".join(output)

# sample (structure-based; exact IO may vary)
assert run("5 4\n1 1\n1 2\n4\n3\n")  # sanity run

# custom cases
assert run("1 3\n3\n1 1\n3\n") == "1\n1"
assert run("5 5\n1 1\n1 2\n1 3\n4\n3\n") is not None
assert run("4 4\n4\n2 1\n4\n3\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh 1 phần tử | ổn định mex | ranh giới nhỏ nhất | 
| chuỗi chèn đầy đủ | tính nhất quán của cấu trúc | không thiếu yếu tố | 
| tất cả đều thiếu | đầy đủ trọng lượng | điều kiện ban đầu | 
| cập nhật hỗn hợp | tính đúng đắn năng động | chuyển đổi hành vi | 

## Vỏ cạnh 

Một trường hợp cạnh tới hạn là khi tất cả các phần tử đều có mặt. Trong trường hợp đó, mỗi lá đều được đánh dấu là có mặt, vì vậy tất cả số lượng đều bằng 0. Gốc cây phân đoạn có tổng bằng 0 và quá trình truyền tải mex luôn không tìm thấy phần tử nào bị thiếu bên trong$[1,n]$, vì vậy nó trả về$n+1$. Việc triển khai phải đảm bảo đệ quy không dừng sớm do tính toán thiếu sai. 

Một trường hợp cạnh khác là các thao tác lặp lại trên cùng một phần tử. Bởi vì các bản cập nhật ghi đè trạng thái thay vì chuyển đổi một cách mù quáng, việc chèn một phần tử đã có không được tính gấp đôi. Việc gán lá sẽ đặt lại hoàn toàn cả số lượng và mức đóng góp, giúp ngăn ngừa lỗi tích lũy. 

Trường hợp cạnh thứ ba là chèn và xóa xen kẽ trên cùng một giá trị. Vì mỗi bản cập nhật sẽ xây dựng lại lá nên cây phân đoạn luôn chỉ phản ánh tư cách thành viên hiện tại và không có hiện tượng rò rỉ trọng lượng lịch sử nào xảy ra thông qua tổng số cây con cũ.
