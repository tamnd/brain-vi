---
title: "CF 1100F - Ivan và Burgers"
description: "Chúng ta được cung cấp một mảng các giá trị trên một dòng, trong đó mỗi vị trí đại diện cho một cửa hàng bánh mì kẹp thịt và giá trị tại vị trí đó là giá của chiếc bánh mì kẹp thịt đắt nhất hiện có ở đó."
date: "2026-06-13T07:10:55+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1100
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 532 (Div. 2)"
rating: 2500
weight: 1100
solve_time_s: 240
verified: true
draft: false
---

[CF 1100F - Ivan và Burgers](https://codeforces.com/problemset/problem/1100/F) 

**Đánh giá:** 2500 
**Tags:** cấu trúc dữ liệu, chia để trị, tham lam, toán học 
**Thời gian giải:** 4 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các giá trị trên một dòng, trong đó mỗi vị trí đại diện cho một cửa hàng bánh mì kẹp thịt và giá trị tại vị trí đó là giá của chiếc bánh mì kẹp thịt đắt nhất hiện có ở đó. Đối với mỗi truy vấn, chúng tôi được cung cấp một phân đoạn của mảng này và chúng tôi được phép chọn bất kỳ tập hợp con chỉ mục nào bên trong phân đoạn đó. Tác dụng của việc chọn một tập hợp con là chúng ta bắt đầu với một số ban đầu rất lớn và liên tục áp dụng XOR với các giá trị đã chọn. Số lượng chúng ta quan tâm là giá trị giảm bao nhiêu so với trạng thái ban đầu, tương đương với việc tối đa hóa kết quả XOR của tập hợp con đã chọn. 

Việc đơn giản hóa khóa xuất phát từ giá trị ban đầu là tất cả các bit được đặt. Nếu chúng ta bắt đầu với một số dạng trong đó mỗi bit liên quan là 1, thì việc XOR với một giá trị sẽ lật các bit một cách hiệu quả. “Số tiền chi tiêu” chỉ phụ thuộc vào giá trị kết quả sau tất cả các hoạt động XOR, do đó nhiệm vụ giảm xuống mức tối đa hóa XOR của một số tập hợp con của phân khúc nhất định. 

Do đó, mỗi truy vấn sẽ hỏi: cho một mảng con, giá trị XOR tối đa mà chúng ta có thể nhận được bằng cách chọn bất kỳ tập hợp con nào của các phần tử bên trong nó là bao nhiêu. 

Các ràng buộc rất lớn, lên tới 500.000 phần tử và 500.000 truy vấn. Điều này ngay lập tức loại trừ việc tính toán lại mọi thứ cho mỗi truy vấn theo thời gian tuyến tính trên phân đoạn, vì điều đó sẽ dẫn đến khoảng$2.5 \times 10^{11}$hoạt động trong trường hợp xấu nhất. Thậm chí$O(n \log n)$mỗi truy vấn quá chậm. Chúng tôi cần một cấu trúc hỗ trợ các truy vấn phạm vi nhanh trên các tập hợp con XOR. 

Một cách tiếp cận đơn giản sẽ xây dựng tất cả các tập hợp con cho một phạm vi hoặc tính toán lại cơ sở XOR tham lam cho mỗi truy vấn. Điều này không thành công vì việc liệt kê tập hợp con có tính lũy thừa và thậm chí việc duy trì cơ sở chạy cho mỗi truy vấn từ đầu cũng quá chậm khi lặp lại nhiều lần. 

Trường hợp cạnh tinh vi phát sinh khi tất cả các giá trị trong một phân đoạn bằng 0. Câu trả lời đúng là bằng 0 và bất kỳ cách tiếp cận nào giả định ít nhất một vectơ cơ sở khác 0 mà không được chuẩn hóa thích hợp đều có thể trả về giá trị khác 0 không chính xác hoặc bị lỗi khi truy vấn các cơ sở trống. Một trường hợp đặc biệt khác là khi các giá trị được lặp lại; trùng lặp không được mở rộng cơ sở không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi thu thập tất cả các giá trị trong phân đoạn và thử từng tập hợp con, tính toán XOR và theo dõi mức tối đa. Điều này đúng vì nó trực tiếp đánh giá định nghĩa của vấn đề. Tuy nhiên, mỗi đoạn có chiều dài$k$có$2^k$tập hợp con, điều này trở nên không thể ngay cả đối với$k = 30$, huống hồ là$k = 500000$. 

Chúng ta cần nhận ra rằng tối ưu hóa tập hợp con XOR không phải là trực tiếp về các tập hợp con mà là về cấu trúc tuyến tính mà chúng hình thành trên GF(2). Bất kỳ tập hợp số nào cũng tạo ra một không gian vectơ trong XOR và bất kỳ tập hợp con XOR nào đều tương ứng với tổ hợp tuyến tính của các vectơ này. XOR tối đa có thể đạt được là giá trị tối đa có thể đạt được từ cơ sở tuyến tính của tập hợp. 

Điều này biến vấn đề thành việc duy trì cơ sở tuyến tính trên các phân đoạn của mảng. Quan sát quan trọng là hành vi đầy đủ của phân khúc được mô tả đầy đủ bằng cơ sở của nó, có kích thước tối đa là 20 vì các giá trị lên tới$10^6$, do đó có nhiều nhất là 20 bit liên quan. 

Sau đó, chúng tôi cần một cấu trúc dữ liệu có thể trả về cơ sở kết hợp của bất kỳ phân đoạn nào một cách hiệu quả. Cây phân đoạn hoạt động một cách tự nhiên: mỗi nút lưu trữ một cơ sở cho khoảng của nó và việc hợp nhất hai nút có nghĩa là hợp nhất hai cơ sở thành một cơ sở rút gọn. 

Khi chúng tôi có được cơ sở cho một phân đoạn truy vấn, chúng tôi tính toán XOR tối đa bằng cách cố gắng cải thiện kết quả từ bit cao nhất trở xuống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force |$O(2^n)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Cây phân đoạn với cơ sở XOR |$O((n + q)\log n \cdot B^2)$|$O(n \log n \cdot B)$| Đã chấp nhận | 

Đây$B$là số bit, khoảng 20. 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một cây phân đoạn trong đó mỗi nút lưu trữ cơ sở tuyến tính của tất cả các giá trị trong phân đoạn của nó. 

1. Khởi tạo mỗi nút lá với cơ sở chỉ chứa một giá trị duy nhất tại vị trí đó. Cơ sở này đại diện cho tất cả các XOR có thể đạt được từ phần tử đơn lẻ đó, vốn chỉ bằng 0 và chính phần tử đó. 
2. Xác định thao tác hợp nhất giữa hai cơ sở. Chúng tôi lấy tất cả các vectơ từ cơ sở thứ hai và chèn chúng vào cơ sở thứ nhất bằng cách sử dụng phương pháp chèn cơ sở tuyến tính tiêu chuẩn. Mỗi lần chèn cố gắng loại bỏ bit được đặt cao nhất bằng cách sử dụng các vectơ cơ sở hiện có, đảm bảo cấu trúc vẫn độc lập. 
3. Xây dựng cây phân đoạn từ dưới lên bằng cách hợp nhất các nút con. Mỗi nút bên trong lưu trữ cơ sở đã hợp nhất của các phân đoạn con trái và phải của nó. Điều này đảm bảo rằng mọi phân đoạn đều được thể hiện đầy đủ bằng cơ sở của nó. 
4. Đối với mỗi truy vấn$[l, r]$, chúng ta duyệt cây phân đoạn và thu thập cơ sở của tất cả các phân đoạn bao phủ đầy đủ phạm vi truy vấn. Chúng được hợp nhất thành một cơ sở tạm thời duy nhất. 
5. Sau khi có cơ sở cuối cùng cho một truy vấn, chúng tôi sẽ tính giá trị XOR tối đa. Chúng tôi bắt đầu từ 0 và lặp lại các bit từ cao nhất đến thấp nhất, cố gắng cải thiện giá trị bằng cách XOR với các vectơ cơ sở làm tăng giá trị đó. 
6. Xuất giá trị tối đa này cho mỗi truy vấn. 

Lý do việc hợp nhất hoạt động là do sự kết hợp XOR trên các phân đoạn rời rạc là độc lập. Bất kỳ tập hợp con nào của phạm vi đầy đủ đều có thể được chia thành các tập hợp con của phần bên trái và bên phải và XOR của chúng kết hợp tuyến tính. Do đó, việc hợp nhất các cơ sở sẽ bảo toàn tất cả các XOR có thể đạt được. 

### Tại sao nó hoạt động 

Tại mỗi nút cây phân đoạn, chúng tôi duy trì một cơ sở bao trùm chính xác tập hợp các giá trị có thể biểu thị XOR cho khoảng đó. Hoạt động hợp nhất duy trì khoảng vì sự kết hợp XOR của hai bộ tương ứng với sự kết hợp của các không gian vectơ của chúng. Vì mọi truy vấn phân tách thành các phân đoạn rời rạc mà sự kết hợp của chúng tạo thành phạm vi đầy đủ, nên việc hợp nhất các cơ sở của chúng sẽ tạo ra một cơ sở bao trùm tất cả các tập hợp con XOR có thể có trong khoảng truy vấn. Sau đó, bước trích xuất tham lam sẽ tìm thấy giá trị biểu thị tối đa trong không gian đó, giá trị này là tối ưu vì các cơ sở tuyến tính đảm bảo rằng mọi XOR có thể truy cập đều có thể được xây dựng thông qua cấu trúc bitwise xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

B = 20

class Basis:
    __slots__ = ("b",)
    def __init__(self):
        self.b = [0] * B

    def insert(self, x):
        for i in reversed(range(B)):
            if (x >> i) & 1:
                if self.b[i]:
                    x ^= self.b[i]
                else:
                    self.b[i] = x
                    return

    def merge(self, other):
        for x in other.b:
            if x:
                self.insert(x)

    def max_xor(self):
        res = 0
        for i in reversed(range(B)):
            res = max(res, res ^ self.b[i])
        return res

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [Basis() for _ in range(4 * self.n)]
        self.arr = arr
        self.build(1, 0, self.n - 1)

    def build(self, v, l, r):
        if l == r:
            self.t[v].insert(self.arr[l])
        else:
            m = (l + r) // 2
            self.build(v * 2, l, m)
            self.build(v * 2 + 1, m + 1, r)
            self.t[v].merge(self.t[v * 2])
            self.t[v].merge(self.t[v * 2 + 1])

    def query(self, v, l, r, ql, qr, res):
        if ql <= l and r <= qr:
            res.merge(self.t[v])
            return
        m = (l + r) // 2
        if ql <= m:
            self.query(v * 2, l, m, ql, qr, res)
        if qr > m:
            self.query(v * 2 + 1, m + 1, r, ql, qr, res)

def main():
    n = int(input())
    arr = list(map(int, input().split()))
    st = SegTree(arr)

    q = int(input())
    out = []

    for _ in range(q):
        l, r = map(int, input().split())
        res = Basis()
        st.query(1, 0, n - 1, l - 1, r - 1, res)
        out.append(str(res.max_xor()))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cây phân đoạn được xây dựng một lần trên mảng và mỗi nút lưu trữ một biểu diễn nén của tất cả các đóng góp XOR trong khoảng của nó. Hàm truy vấn chỉ thu thập các nút có liên quan và hợp nhất các cơ sở của chúng thành một cấu trúc tạm thời. Bước cuối cùng tính toán XOR tốt nhất có thể đạt được. 

Điểm tinh tế trong thực hiện là các căn cứ sáp nhập phải bảo toàn tính độc lập; việc chèn từng phần tử của cơ sở này vào cơ sở khác là cần thiết hơn là việc ghép nối đơn giản. Một chi tiết khác là đảm bảo cơ sở sử dụng giới hạn bit cố định, vì các giá trị được giới hạn bởi$10^6$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
arr = [7, 2, 3, 4]
query = [1, 4]
```| Bước | Cơ sở hiện tại | Hành động | 
| --- | --- | --- | 
| 1 | {} | chèn 7 | 
| 2 | {7} | chèn 2 | 
| 3 | {7, 2} | chèn 3 | 
| 4 | {7, 2, 3} | chèn 4 | 

Sau khi xây dựng cơ sở, chúng tôi tính toán XOR tối đa bằng phương pháp lựa chọn tham lam. XOR tốt nhất có thể đạt được là 7. 

Điều này cho thấy rằng mặc dù tất cả các phần tử đều có sẵn nhưng tập hợp con tối ưu chỉ là chọn một phần tử đóng góp mạnh duy nhất. 

### Ví dụ 2 

đầu vào:```
n = 3
arr = [12, 14, 23]
query = [1, 3]
```| Bước | Cơ sở | Giải thích | 
| --- | --- | --- | 
| Bắt đầu | {} | trống | 
| Chèn 12 | {12} | thêm vector đầu tiên | 
| Chèn 14 | {12, 14} | độc lập | 
| Chèn 23 | {12, 14, 23} | cơ sở đầy đủ | 

Bây giờ hãy tính XOR tối đa. Việc xây dựng tham lam mang lại 27 từ 12 XOR 23. 

Điều này xác nhận rằng các tập hợp con tối ưu có thể bao gồm nhiều phần tử ngay cả khi các giá trị riêng lẻ không tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log n \cdot B^2)$| mỗi lần hợp nhất chèn tối đa B vectơ, mỗi lần chèn có giá B | 
| Không gian |$O(n \log n \cdot B)$| mỗi nút cây phân đoạn lưu trữ một cơ sở có kích thước B | 

Độ phức tạp là đủ vì$B \approx 20$, làm cho tất cả các phép toán trở thành hằng số nhỏ một cách hiệu quả. Ngay cả với 500.000 truy vấn, các thao tác trên cây phân đoạn vẫn đủ nhanh trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    # assume solution is defined above in same file
    main()

    return output.getvalue().strip()

# provided sample
assert run("""4
7 2 3 4
3
1 4
2 3
1 3
""") == """7
3
7"""

# single element
assert run("""1
5
1
1 1
""") == "5"

# all zeros
assert run("""3
0 0 0
2
1 3
2 2
""") == "0\n0"

# all equal
assert run("""4
8 8 8 8
1
1 4
""") == "8"

# increasing
assert run("""5
1 2 4 8 16
1
1 5
""") == "31"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 | độ đúng của lá | 
| tất cả số không | 0, 0 | xử lý cơ sở trống | 
| tất cả đều bình đẳng | 8 | xử lý trùng lặp | 
| sức mạnh của hai | 31 | xây dựng cơ sở đầy đủ | 

## Vỏ cạnh 

Đối với một phân đoạn chỉ chứa các số 0, mỗi lần chèn vào cơ sở đều để trống, do đó XOR tham lam cuối cùng vẫn bằng 0. Cây phân đoạn lưu trữ chính xác các cơ sở trống cho các nút đó và việc hợp nhất các cơ sở trống không có tác dụng, duy trì tính chính xác. 

Đối với các giá trị lặp lại, việc chèn cùng một số hai lần sẽ không làm thay đổi cơ sở vì nó sẽ bị vectơ hiện có loại bỏ. Điều này đảm bảo các bản sao không làm tăng không gian có thể biểu diễn một cách giả tạo. 

Đối với các truy vấn một phần tử, nút lá đã chứa cơ sở chính xác nên không cần phải hợp nhất. Truy vấn chỉ trả về cơ sở của nút đó và bước tham lam sẽ trả về chính phần tử đó, phù hợp với câu trả lời đúng.
