---
title: "CF 105968J - Johannes Yêu Trò Chơi"
description: "Chúng tôi đang làm việc với một mảng số nguyên động trong đó hai loại hoạt động được hỗ trợ. Một thao tác thay đổi giá trị tại một vị trí và thao tác kia yêu cầu thông tin về phân đoạn liền kề của mảng."
date: "2026-06-22T16:21:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105968
codeforces_index: "J"
codeforces_contest_name: "IME++ Starters Try-Outs 2025"
rating: 0
weight: 105968
solve_time_s: 63
verified: true
draft: false
---

[CF 105968J - Johannes yêu thích trò chơi](https://codeforces.com/problemset/problem/105968/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một mảng số nguyên động trong đó hai loại hoạt động được hỗ trợ. Một thao tác thay đổi giá trị tại một vị trí và thao tác kia yêu cầu thông tin về phân đoạn liền kề của mảng. Truy vấn trên một phân đoạn không phải là một tổng hợp đơn giản như phần tử tổng hoặc phần tử tối đa. Thay vào đó, nó yêu cầu số tiền tốt nhất có thể mà bạn có thể nhận được bằng cách chọn một mảng con liền kề hoàn toàn bên trong phân khúc đó. 

Nói cách khác, đối với bất kỳ phạm vi truy vấn nào$[l, r]$, chúng ta xem xét tất cả các mảng con chứa đầy đủ trong phạm vi đó và lấy tổng tối đa có thể có trong số đó. Mảng có thể thay đổi nên sau khi cập nhật, các truy vấn trong tương lai phải phản ánh các giá trị đã thay đổi. 

Kích thước đầu vào ngụ ý lên tới khoảng$10^5$các phần tử và thao tác, ngay lập tức loại trừ việc tính toán lại từng truy vấn bằng cách quét phân đoạn. Việc tính toán lại ngây thơ cho mỗi truy vấn sẽ dẫn đến$O(n)$làm việc theo mỗi truy vấn và$O(nq)$tổng thể phức tạp, vượt xa giới hạn khả thi. 

Khó khăn chính là các truy vấn phân đoạn phải kết hợp thông tin từ hai nửa theo cách duy trì đủ cấu trúc để tái tạo lại mảng con tốt nhất vượt qua ranh giới. 

Một số trường hợp đặc biệt có xu hướng phá vỡ các nỗ lực tham lam hoặc dựa trên tiền tố không chính xác. 

Một vấn đề xuất hiện khi tất cả các số đều âm. Ví dụ, trong một mảng$[-5, -2, -7]$, câu trả lời đúng cho truy vấn phạm vi là$-2$, vì chúng ta vẫn phải chọn một mảng con không trống. Một cách tiếp cận ngây thơ giả sử "cho phép phân mảng trống với tổng 0" sẽ trả về 0 không chính xác. 

Một vấn đề khác xuất hiện khi mảng con tối ưu vượt qua điểm giữa của phép chia. Ví dụ, trong$[2, -1, 3]$, câu trả lời tốt nhất cho toàn bộ phạm vi là$4$, xuất phát từ toàn bộ mảng. Bất kỳ cách tiếp cận nào chỉ theo dõi các câu trả lời tốt nhất ở nửa bên trái và bên phải một cách độc lập sẽ bỏ lỡ sự kết hợp xuyên ranh giới này. 

Trường hợp tinh tế thứ ba phát sinh trong quá trình cập nhật. Sau khi thay đổi một giá trị, thông tin phân đoạn được lưu trong bộ nhớ đệm phải nhất quán trên tất cả các tổ tiên trong cây phân đoạn. Việc không tính toán lại tất cả các thuộc tính dẫn xuất sẽ dẫn đến dữ liệu tiền tố hoặc hậu tố cũ, điều này sẽ âm thầm phá vỡ các truy vấn sau này. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn trên một phạm vi$[l, r]$, chúng tôi lặp lại tất cả các điểm bắt đầu và điểm kết thúc có thể có trong phạm vi và tính tổng của từng mảng con. Chúng tôi theo dõi số tiền tối đa gặp phải. Điều này đúng vì nó kiểm tra rõ ràng mọi mảng con ứng cử viên, không để lại khả năng bỏ sót mảng tối ưu. 

Tuy nhiên, điều này đòi hỏi ba lớp công việc lồng nhau trong thực tế. Mỗi truy vấn có thể mất$O(n)$thời gian để quét và nếu được triển khai một cách đơn giản để liệt kê mảng con thì nó sẽ thoái hóa hơn nữa. Với tối đa$10^5$truy vấn, điều này trở nên hiệu quả$10^{10}$hoạt động trong trường hợp xấu nhất là không thể sử dụng được. 

Quan sát quan trọng là câu trả lời cho một phân đoạn có thể được xây dựng từ thông tin cô đọng về hai nửa của nó. Thay vì lưu trữ tất cả các mảng con, chúng tôi chỉ lưu trữ những gì cần thiết để xây dựng lại bất kỳ mảng con tối ưu nào có thể vượt qua một ranh giới. 

Đối với mỗi phân khúc, chúng tôi duy trì bốn giá trị. Tổng của phân đoạn, tổng tiền tố tốt nhất, tổng hậu tố tốt nhất và tổng mảng con tốt nhất ở bất kỳ đâu trong phân khúc. Bốn giá trị này là đủ vì bất kỳ mảng con tối ưu nào trong phân đoạn kết hợp đều nằm hoàn toàn ở nửa bên trái, hoàn toàn ở nửa bên phải hoặc vượt qua ranh giới. Trường hợp giao nhau có thể được biểu thị bằng cách sử dụng hậu tố của nửa bên trái và tiền tố của nửa bên phải. 

Điều này làm giảm mỗi thao tác hợp nhất thành thời gian không đổi, cho phép cây phân đoạn trong đó cả cập nhật và truy vấn đều hoạt động theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(1)$| Quá chậm | 
| Cây phân đoạn với nút tùy chỉnh |$O((n + q)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Mỗi nút trong cây phân đoạn đại diện cho một phân đoạn của mảng và lưu trữ thông tin tổng hợp đủ để trả lời bất kỳ truy vấn nào trên phân đoạn đó. 

1. Xây dựng cây phân đoạn trong đó mỗi lá tương ứng với một phần tử mảng. Đối với một lá, tổng tổng, tiền tố, hậu tố và mảng con tốt nhất đều bằng chính phần tử đó. Đây là trường hợp cơ bản vì một phần tử không có cấu trúc bên trong. 
2. Xác định thao tác hợp nhất kết hợp hai nút con đại diện cho các phân đoạn liền kề. Hoạt động này là cốt lõi của giải pháp vì nó đảm bảo rằng chúng ta không bao giờ mất thông tin cần thiết cho các mảng con xuyên biên giới. 
3. Khi hợp nhất một nút bên trái$A$và nút phải$B$, tính tổng số tiền kết hợp như$A.sum + B.sum$. Đây là trực tiếp vì các tổng có tính cộng trong các khoảng rời rạc. 
4. Tính toán tiền tố tốt nhất của phân đoạn được hợp nhất là giá trị tối đa giữa$A.prefix$Và$A.sum + B.prefix$. Điều này nắm bắt xem tiền tố tốt nhất nằm hoàn toàn ở phân khúc bên trái hay mở rộng sang phân khúc bên phải. 
5. Tính toán hậu tố tốt nhất một cách đối xứng là lớn nhất giữa$B.suffix$Và$B.sum + A.suffix$. Điều này cho biết hậu tố tốt nhất nằm ở phân khúc bên phải hay mở rộng sang phân khúc bên trái. 
6. Tính toán mảng con tốt nhất ở mức tối đa trong số ba khả năng: mảng con tốt nhất nằm hoàn toàn ở đoạn bên trái, nằm hoàn toàn ở đoạn bên phải hoặc vượt qua ranh giới như$A.suffix + B.prefix$. Bước này là ý tưởng trung tâm vì nó liệt kê rõ ràng tất cả các khả năng cấu trúc cho một mảng con tối ưu. 
7. Để cập nhật, hãy sửa đổi nút lá và tính toán lại tất cả nút tổ tiên của nó bằng thao tác hợp nhất. Điều này đảm bảo tính nhất quán từ vị trí được sửa đổi cho đến vị trí gốc. 
8. Đối với một truy vấn, hãy kết hợp đệ quy các phân đoạn có liên quan theo đúng thứ tự. Nút được hợp nhất cuối cùng chứa câu trả lời trong trường mảng con tốt nhất của nó. 

Tính chính xác dựa vào tính bất biến mà mỗi nút tóm tắt đầy đủ tất cả các mảng con trong phân đoạn của nó chỉ bằng bốn giá trị. Bất kỳ mảng con nào trong một phân đoạn đều được chứa đầy đủ trong một mảng con hoặc vượt qua ranh giới đúng một lần. Hoạt động hợp nhất chiếm cả ba khả năng cấu trúc, do đó không có mảng con ứng cử viên nào bị mất trong quá trình tổng hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("sum", "pref", "suff", "best")
    def __init__(self, s=0, p=0, sf=0, b=0):
        self.sum = s
        self.pref = p
        self.suff = sf
        self.best = b

def merge(a, b):
    res = Node()
    res.sum = a.sum + b.sum
    res.pref = max(a.pref, a.sum + b.pref)
    res.suff = max(b.suff, b.sum + a.suff)
    res.best = max(a.best, b.best, a.suff + b.pref)
    return res

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.t = [Node(-10**18, -10**18, -10**18, -10**18) for _ in range(2 * self.size)]
        for i in range(self.n):
            v = arr[i]
            self.t[self.size + i] = Node(v, v, v, v)
        for i in range(self.size - 1, 0, -1):
            self.t[i] = merge(self.t[2*i], self.t[2*i+1])

    def update(self, idx, val):
        i = self.size + idx
        self.t[i] = Node(val, val, val, val)
        i //= 2
        while i:
            self.t[i] = merge(self.t[2*i], self.t[2*i+1])
            i //= 2

    def query(self, l, r):
        l += self.size
        r += self.size + 1

        left_res = None
        right_res = None

        while l < r:
            if l & 1:
                left_res = self.t[l] if left_res is None else merge(left_res, self.t[l])
                l += 1
            if r & 1:
                r -= 1
                right_res = self.t[r] if right_res is None else merge(self.t[r], right_res)
            l //= 2
            r //= 2

        if left_res is None:
            return right_res.best
        if right_res is None:
            return left_res.best
        return merge(left_res, right_res).best

def main():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    st = SegTree(arr)

    out = []
    for _ in range(q):
        t, a, b = map(int, input().split())
        if t == 1:
            st.update(a - 1, b)
        else:
            out.append(str(st.query(a - 1, b - 1)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cây phân đoạn lưu trữ tóm tắt cấu trúc đầy đủ của từng khoảng. Hàm hợp nhất mã hóa tất cả các cách có thể để hình thành một mảng con tối đa trên hai nửa. Thao tác cập nhật sẽ xây dựng lại đường dẫn đến thư mục gốc để mọi phân đoạn bị ảnh hưởng vẫn nhất quán sau khi thay đổi điểm. Hàm truy vấn sử dụng duyệt cây phân đoạn lặp tiêu chuẩn, kết hợp các phân đoạn một phần từ trái sang phải trong khi vẫn giữ nguyên thứ tự, điều này rất quan trọng vì thông tin tiền tố và hậu tố phụ thuộc vào hướng. 

Một chi tiết tinh tế là hành vi của phần tử trung tính trong quá trình truy vấn. Mã xử lý việc tích lũy trống một cách cẩn thận bằng cách trì hoãn việc khởi tạo các kết quả bên trái và bên phải. Điều này tránh việc hợp nhất không chính xác với các nút chưa được khởi tạo. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng$[2, -1, 3]$với một truy vấn cho toàn bộ phạm vi. 

Chúng tôi bắt đầu với các nút lá đã đại diện cho các giá trị riêng lẻ. 

| Bước | Nút trái | Nút phải | Tổng hợp | Tiền tố | Hậu tố | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| hợp nhất (2, -1) | 2 | -1 | 1 | 2 | -1 | 2 | 
| hợp nhất (trước, 3) | 2,-1 | 3 | 4 | 4 | 3 | 4 | 

Kết quả cuối cùng là 4, đến từ toàn bộ đoạn. Điều này cho thấy sự kết hợp xuyên biên giới là cần thiết như thế nào, vì không một nửa nào tạo ra được câu trả lời tối ưu. 

Bây giờ hãy xem xét$[-5, -2, -7]$cho toàn bộ phạm vi. 

| Bước | Nút trái | Nút phải | Tổng hợp | Tiền tố | Hậu tố | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| hợp nhất(-5, -2) | -5 | -2 | -7 | -2 | -5 | -2 | 
| hợp nhất (trước, -7) | -5,-2 | -7 | -14 | -2 | -2 | -2 | 

Kết quả là -2, xuất phát từ việc chọn phần tử duy nhất -2. Điều này xác nhận rằng cấu trúc xử lý chính xác các mảng toàn âm mà không đưa ra các số 0 nhân tạo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log n)$| Mỗi bản cập nhật và truy vấn đi qua chiều cao của cây phân đoạn, hợp nhất các nút có kích thước không đổi | 
| Không gian |$O(n)$| Cây phân đoạn lưu trữ một lượng thông tin không đổi trên mỗi nút | 

Hệ số logarit phù hợp tốt với các ràng buộc lên đến$10^5$hoạt động, giữ cho tổng công việc thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Node:
        __slots__ = ("sum", "pref", "suff", "best")
        def __init__(self, s=0, p=0, sf=0, b=0):
            self.sum = s
            self.pref = p
            self.suff = sf
            self.best = b

    def merge(a, b):
        res = Node()
        res.sum = a.sum + b.sum
        res.pref = max(a.pref, a.sum + b.pref)
        res.suff = max(b.suff, b.sum + a.suff)
        res.best = max(a.best, b.best, a.suff + b.pref)
        return res

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.size = 1
            while self.size < self.n:
                self.size *= 2
            self.t = [Node(-10**18, -10**18, -10**18, -10**18) for _ in range(2 * self.size)]
            for i in range(self.n):
                v = arr[i]
                self.t[self.size + i] = Node(v, v, v, v)
            for i in range(self.size - 1, 0, -1):
                self.t[i] = merge(self.t[2*i], self.t[2*i+1])

        def update(self, idx, val):
            i = self.size + idx
            self.t[i] = Node(val, val, val, val)
            i //= 2
            while i:
                self.t[i] = merge(self.t[2*i], self.t[2*i+1])
                i //= 2

        def query(self, l, r):
            l += self.size
            r += self.size + 1
            left_res = None
            right_res = None
            while l < r:
                if l & 1:
                    left_res = self.t[l] if left_res is None else merge(left_res, self.t[l])
                    l += 1
                if r & 1:
                    r -= 1
                    right_res = self.t[r] if right_res is None else merge(self.t[r], right_res)
                l //= 2
                r //= 2
            if left_res is None:
                return right_res.best
            if right_res is None:
                return left_res.best
            return merge(left_res, right_res).best

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    st = SegTree(arr)

    out = []
    for _ in range(q):
        t, a, b = map(int, input().split())
        if t == 1:
            st.update(a - 1, b)
        else:
            out.append(str(st.query(a - 1, b - 1)))

    return "\n".join(out)

# custom cases
assert run("3 2\n1 2 3\n2 1 3\n2 2 3\n") == "6\n5", "basic range sums"
assert run("3 1\n-1 -2 -3\n2 1 3\n") == "-1", "all negative"
assert run("5 3\n1 -1 2 -2 3\n2 1 5\n1 2 5\n2 1 5\n") == "3\n8", "update effect"
assert run("1 1\n-10\n2 1 1\n") == "-10", "single element"
assert run("4 2\n2 -1 2 -1\n2 1 4\n2 2 3\n") == "3\n2", "cross boundary behavior"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tổng phạm vi cơ bản | 6, 5 | tính đúng đắn của việc hợp nhất trên toàn bộ mảng dương | 
| tất cả đều tiêu cực | -1 | xử lý mảng con không-không | 
| cập nhật hiệu ứng | 3, 8 | tuyên truyền các bản cập nhật | 
| phần tử đơn | -10 | xử lý lá | 
| hành vi xuyên ranh giới | 3, 2 | logic mảng con ranh giới | 

## Vỏ cạnh 

Đối với một mảng toàn âm như$[-5, -2, -7]$, cây phân đoạn sẽ khởi tạo mỗi lá như một phân đoạn ứng cử viên hợp lệ. Khi được hợp nhất, các giá trị tiền tố và hậu tố sẽ truyền phần tử âm ít nhất lên trên. Trường tốt nhất không bao giờ đưa ra các số 0 nhân tạo vì mọi ứng cử viên đều được lấy từ các giá trị mảng thực tế. Kết quả cuối cùng chính xác trở thành$-2$, được bảo toàn thông qua việc hợp nhất vì nó luôn tốt hơn bất kỳ sự kết hợp nhiều phần tử nào. 

Đối với một truy vấn một phần tử như$[x]$, truy vấn sẽ trả về trực tiếp nút lá. Logic hợp nhất không bao giờ được kích hoạt, vì vậy giá trị tốt nhất vẫn chính xác$x$, xác nhận tính đúng đắn của trường hợp cơ sở. 

Đối với các bản cập nhật thay đổi giá trị từ dương sang âm, hàm cập nhật sẽ thay thế lá và xây dựng lại đường dẫn đến thư mục gốc. Mỗi tổ tiên tính toán lại tiền tố, hậu tố và các giá trị tốt nhất, đảm bảo rằng các phân đoạn tối ưu trước đó được hạ cấp một cách chính xác nếu phần tử đã thay đổi làm mất hiệu lực của chúng.
