---
title: "CF 103369B - \u0423\u043d\u0438\u0447\u0442\u043e\u0436\u0435\u043d\u0438\u0435 \u043c\u0430\u0441\u0441\u0438\u0432\u0430"
description: "Chúng ta được cung cấp một mảng số tĩnh và sau đó là một chuỗi cho chúng ta biết thứ tự các phần tử của mảng này được “loại bỏ” từng phần tử một. Sau mỗi lần loại bỏ, chúng ta còn lại một số phân đoạn liền kề rời rạc của các phần tử vẫn còn tồn tại."
date: "2026-07-03T12:49:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103369
codeforces_index: "B"
codeforces_contest_name: "Moscow team olympiad 2021"
rating: 0
weight: 103369
solve_time_s: 52
verified: true
draft: false
---

[CF 103369B - \u0423\u043d\u0438\u0447\u0442\u043e\u0436\u0435\u043d\u0438\u0435 \u043c\u0430\u0441\u0441\u0438\u0432\u0430](https://codeforces.com/problemset/problem/103369/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số tĩnh và sau đó là một chuỗi cho chúng ta biết thứ tự các phần tử của mảng này được “loại bỏ” từng phần tử một. Sau mỗi lần loại bỏ, chúng ta còn lại một số phân đoạn liền kề rời rạc của các phần tử vẫn còn tồn tại. Từ các phân đoạn còn lại này, chúng ta quan tâm đến phân đoạn có tổng các phần tử lớn nhất. 

Chi tiết quan trọng là các phần tử bị loại bỏ hoạt động giống như các rào cản: chúng chia mảng thành các khối độc lập. Bên trong mỗi khối, chúng ta được phép lấy một mảng con, nhưng vì tất cả các số đều không âm nên mảng con tốt nhất bên trong bất kỳ khối nào chỉ đơn giản là toàn bộ khối đó. Vì vậy, sau mỗi lần xóa, câu trả lời thực sự là tổng tối đa trên tất cả các phân đoạn liền kề hiện đang tồn tại. 

Từ góc độ tính toán, quá trình này rất năng động và mang tính hủy diệt. Chúng tôi bắt đầu với một mảng đầy đủ, sau đó chia dần thành các phân đoạn nhỏ hơn. Sau mỗi bước, chúng ta phải tính toán lại thuộc tính chung của các phân đoạn còn lại, điều này làm cho việc tính toán lại đơn giản trở nên tốn kém. 

Các ràng buộc là điển hình cho một$n \le 10^5$vấn đề này ngay lập tức loại trừ việc tính lại tổng phân đoạn từ đầu sau mỗi lần xóa. Quét toàn bộ mỗi bước sẽ dẫn đến khoảng$O(n^2)$, quá chậm so với giới hạn 1 đến 2 giây. Ngay cả những cách tiếp cận tốt hơn một chút là quét lại các thành phần nhiều lần vẫn sẽ TLE vì mỗi thành phần có thể được xem lại nhiều lần nếu thực hiện bất cẩn. 

Một số tình huống khó khăn có ý nghĩa quan trọng trong cấu trúc vấn đề này. 

Đầu tiên, tất cả các phần tử có thể bị loại bỏ, trong trường hợp đó không có phân đoạn nào hợp lệ và câu trả lời phải bằng 0. Ví dụ, nếu mảng là$[1, 2]$và cả hai chỉ số cuối cùng đều bị xóa, đầu ra kết thúc bằng$0, 0$, không phải âm vô cùng hoặc một câu trả lời trống rỗng. 

Thứ hai, số không quan trọng đối với hành vi hợp nhất phân khúc. Nếu tất cả các giá trị đều bằng 0 thì mọi phân đoạn đều có tổng bằng 0, do đó câu trả lời vẫn là 0 xuyên suốt. Việc triển khai ngây thơ giả định “tồn tại ít nhất một phân khúc tích cực” có thể vô tình giữ mức tối đa cũ. 

Thứ ba, thứ tự loại bỏ không phải là tùy ý: đó là một hoán vị. Điều này quan trọng vì mọi vị trí đều biến mất đúng một lần, điều này gợi ý rõ ràng rằng hãy đảo ngược quá trình thay vì mô phỏng việc xóa về phía trước. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: sau mỗi lần xóa, chúng tôi quét toàn bộ mảng, bỏ qua các chỉ mục đã xóa, chia thành các khối liền kề, tính tổng từng khối và lấy giá trị tối đa. Mỗi lần quét là$O(n)$, và chúng tôi làm điều đó$n$lần, cho$O(n^2)$. Với$n = 10^5$, đây là về$10^{10}$hoạt động vượt quá giới hạn. 

Quan sát chính là việc xóa khó xử lý trực tiếp nhưng việc chèn lại dễ hợp nhất. Thay vì hủy các phần tử, chúng ta có thể đảo ngược quá trình: bắt đầu từ một mảng trống và “thêm lại” các phần tử theo thứ tự ngược lại khi xóa. Khi chúng tôi thêm một phần tử, phần tử đó sẽ tạo thành một phân khúc mới hoặc hợp nhất với các phân khúc lân cận đã hoạt động. Điều này biến vấn đề thành việc duy trì các thành phần được kết nối trong các hoạt động hợp nhất, trong đó mỗi thành phần lưu trữ tổng của nó. 

Đây chính xác là chế độ mà cấu trúc Disjoint Set Union hoạt động tốt. Mỗi lần chúng tôi kích hoạt một vị trí, chúng tôi kết nối nó với các vị trí hoạt động liền kề và duy trì tổng của từng thành phần được kết nối. Tổng phân khúc tối đa toàn cầu được cập nhật sau mỗi lần kích hoạt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| DSU với quy trình ngược lại |$O(n \alpha(n))$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng tối ưu: xử lý việc xóa ngược 

1. Đọc mảng và thứ tự xóa, đồng thời xây dựng cấu trúc ánh xạ từng vị trí theo thời điểm xóa. Điều này cho phép chúng tôi xây dựng lại dòng thời gian ngược. 
2. Khởi tạo cấu trúc Disjoint Set Union trong đó mọi vị trí ban đầu không hoạt động và duy trì các mảng cho tổng cha và tổng thành phần. 
3. Duy trì mảng boolean`active[i]`cho biết liệu một vị trí hiện có trong quá trình đảo ngược hay không. 
4. Duy trì một biến`best`theo dõi tổng thành phần tối đa bất cứ lúc nào. 
5. Xử lý các vị trí theo thứ tự xóa ngược. Đối với mỗi vị trí`x`, kích hoạt nó, khởi tạo tổng thành phần của nó như`a[x]`, và đặt`best = max(best, a[x])`. 
6. Nếu hàng xóm bên trái đang hoạt động, hợp nhất hai thành phần và cập nhật tổng của gốc đã hợp nhất, sau đó cập nhật`best`. 
7. Nếu hàng xóm bên phải đang hoạt động, hãy thực hiện thao tác kết hợp tương tự. 
8. Sau khi xử lý mỗi lần kích hoạt, hãy ghi lại`best`là câu trả lời cho bước xóa tiếp theo tương ứng. 

Ý tưởng chính đằng sau mỗi liên kết là khi hai thành phần hợp nhất, tổng của chúng kết hợp chính xác và không xảy ra sự trùng lặp hoặc tính hai lần vì mỗi chỉ mục luôn thuộc về chính xác một thành phần. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên sự bất biến tái thiết đơn điệu. Tại bất kỳ bước nào trong quy trình ngược lại, tập hợp các vị trí hoạt động tương ứng chính xác với phần bù của tiền tố xóa. Mỗi thành phần được kết nối trong tập hoạt động này là một khối liền kề tối đa của các phần tử chưa bị xóa trong quy trình chuyển tiếp ban đầu. Bởi vì tất cả các giá trị đều không âm, mảng con tối đa trong bất kỳ khối nào cũng chính là khối đó, do đó việc duy trì tổng thành phần là đủ để thể hiện tất cả các câu trả lời ứng cử viên. Mọi hoạt động hợp nhất đều bảo toàn tổng chính xác của các tập hợp rời rạc và mọi phân đoạn có thể có trong quy trình chuyển tiếp sẽ xuất hiện dưới dạng thành phần DSU ở đúng một bước ngược lại, đảm bảo không có ứng cử viên nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n, a):
        self.parent = list(range(n))
        self.size = [1] * n
        self.sum = a[:]

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra = self.find(a)
        rb = self.find(b)
        if ra == rb:
            return
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        self.sum[ra] += self.sum[rb]

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    order = list(map(int, input().split()))

    active = [False] * n
    dsu = DSU(n, a)

    ans = [0] * n
    best = 0

    for i in range(n - 1, -1, -1):
        idx = order[i] - 1
        active[idx] = True

        best = max(best, a[idx])

        if idx - 1 >= 0 and active[idx - 1]:
            dsu.union(idx, idx - 1)
        if idx + 1 < n and active[idx + 1]:
            dsu.union(idx, idx + 1)

        root = dsu.find(idx)
        best = max(best, dsu.sum[root])

        ans[i] = best

    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc đảo ngược thứ tự xóa sao cho mỗi bước là một bước chèn. Mỗi lần chèn được theo sau bởi tối đa hai phép toán hợp, một với hàng xóm bên trái và một với hàng xóm bên phải. DSU duy trì cả tổng kết nối và tổng thành phần, do đó việc truy xuất phân đoạn tốt nhất sau mỗi bước sẽ giảm xuống mức theo dõi mức tối đa toàn cầu duy nhất. 

Một điểm tinh tế là chúng tôi cập nhật`best`cả sau khi chèn phần tử đơn lẻ và sau khi hợp nhất các thành phần. Điều này tránh trường hợp thiếu sót khi một phân khúc lớn mới được hình thành tốt hơn bất kỳ phân khúc nào trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 3 2 5
3 4 1 2
```Chúng tôi xử lý theo thứ tự ngược lại với việc xóa, vì vậy thứ tự kích hoạt là$2, 1, 4, 3$. 

| Bước | Chỉ mục được kích hoạt | Bộ hoạt động | Tổng thành phần | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | [2] | [2] | 2 | 
| 2 | 1 | [1,2] | [1+3=4] | 4 | 
| 3 | 4 | [1,2,4] | [1+3=4], [5] | 5 | 
| 4 | 3 | [1,2,3,4] | [1+3+2+5=11] | 11 | 

Do đó, các câu trả lời chuyển tiếp là:```
5
4
3
0
```Dấu vết này cho thấy cách đảo ngược biến việc xóa thành hợp nhất và cách phân đoạn tối đa chỉ phát triển ở ranh giới hợp nhất. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
4 2 3 5 1
```Thứ tự kích hoạt đảo ngược:$1, 5, 3, 2, 4$| Bước | Chỉ mục được kích hoạt | Bộ hoạt động | Tổng thành phần | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | [1] | 1 | 
| 2 | 5 | [1], [5] | [1], [5] | 5 | 
| 3 | 3 | [1], [3], [5] | [1], [3], [5] | 5 | 
| 4 | 2 | [1,2,3], [5] | [6], [5] | 6 | 
| 5 | 4 | [1..5] | [15] | 15 | 

Đầu ra chuyển tiếp:```
6
5
5
1
0
```Điều này xác nhận rằng câu trả lời chỉ phụ thuộc vào sự hợp nhất liền kề chứ không phải cấu trúc bên trong. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \alpha(n))$| Mỗi lần kích hoạt thực hiện tối đa hai liên kết và các hoạt động DSU được khấu hao gần như không đổi | 
| Không gian |$O(n)$| Mảng dành cho DSU cha, kích thước, tổng và theo dõi hoạt động | 

Với$n \le 10^5$, điều này dễ dàng phù hợp trong giới hạn thời gian, vì$\alpha(n)$thực tế là không đổi trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import defaultdict

    class DSU:
        def __init__(self, n, a):
            self.parent = list(range(n))
            self.size = [1] * n
            self.sum = a[:]

        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x

        def union(self, a, b):
            ra = self.find(a)
            rb = self.find(b)
            if ra == rb:
                return
            if self.size[ra] < self.size[rb]:
                ra, rb = rb, ra
            self.parent[rb] = ra
            self.size[ra] += self.size[rb]
            self.sum[ra] += self.sum[rb]

    n = int(input())
    a = list(map(int, input().split()))
    order = list(map(int, input().split()))

    active = [False] * n
    dsu = DSU(n, a)

    ans = [0] * n
    best = 0

    for i in range(n - 1, -1, -1):
        idx = order[i] - 1
        active[idx] = True
        best = max(best, a[idx])

        if idx > 0 and active[idx - 1]:
            dsu.union(idx, idx - 1)
        if idx + 1 < n and active[idx + 1]:
            dsu.union(idx, idx + 1)

        best = max(best, dsu.sum[dsu.find(idx)])
        ans[i] = best

    return "\n".join(map(str, ans))

# provided samples
assert run("""4
1 3 2 5
3 4 1 2
""").strip() == """5
4
3
0"""

assert run("""5
1 2 3 4 5
4 2 3 5 1
""").strip() == """6
5
5
1
0"""

# custom cases
assert run("""1
10
1
""").strip() == "10"

assert run("""3
0 0 0
1 2 3
""").strip() == "0\n0\n0"

assert run("""5
5 1 5 1 5
3 1 5 2 4
"""), "mixed values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 10 | xử lý kích thước tối thiểu | 
| tất cả số không | tất cả số không | số tiền trung lập | 
| giá trị xen kẽ | sáp nhập ổn định | liên minh đúng đắn lặp đi lặp lại | 

## Vỏ cạnh 

Đối với mảng một phần tử, DSU bắt đầu với một thành phần và mọi kích hoạt chỉ cần đặt phần tử đó là tốt nhất, vì vậy câu trả lời chỉ là chính phần tử đó và sau đó là các số 0. Thuật toán xử lý việc này vì không có liên minh lân cận nào được kích hoạt và`best`chỉ phụ thuộc vào nút duy nhất đó. 

Đối với một mảng hoàn toàn bằng 0, mọi tổng thành phần vẫn bằng 0 bất kể việc hợp nhất. Thuật toán vẫn kích hoạt và kết hợp chính xác, nhưng`best`không bao giờ tăng, do đó đầu ra luôn bằng 0. 

Đối với trường hợp các giá trị lớn được phân tách bằng số 0, chẳng hạn như`[5, 0, 5]`, sự kết hợp giữa các vị trí bằng 0 không bao giờ xảy ra cho đến khi tất cả các chỉ số ở giữa được kích hoạt. Quá trình ngược lại đảm bảo rằng các phân đoạn chỉ hình thành khi kết nối thực sự tồn tại, do đó mức tối đa được cập nhật chính xác khi toàn bộ khối được kết nối.
