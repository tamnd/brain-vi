---
title: "CF 106259D - AND, OR và XOR"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta cần chọn một dãy con có độ dài ít nhất là hai. Đối với bất kỳ chuỗi con nào được chọn, chúng tôi tính toán hai tập hợp theo bit: AND theo bit của tất cả các số đã chọn và OR theo bit của tất cả các số đã chọn."
date: "2026-06-18T23:38:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106259
codeforces_index: "D"
codeforces_contest_name: "CUET Inter University Programming Contest 2025"
rating: 0
weight: 106259
solve_time_s: 46
verified: true
draft: false
---

[CF 106259D - AND, OR và XOR](https://codeforces.com/problemset/problem/106259/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và chúng ta cần chọn một dãy con có độ dài ít nhất là hai. Đối với bất kỳ chuỗi con nào được chọn, chúng tôi tính toán hai tập hợp theo bit: AND theo bit của tất cả các số đã chọn và OR theo bit của tất cả các số đã chọn. Điểm của chuỗi con là XOR giữa hai kết quả này. Nhiệm vụ là giảm thiểu điểm này trên tất cả các chuỗi con hợp lệ. 

Quan sát quan trọng là AND thu gọn các bit xuống trong khi OR mở rộng các bit lên trên. XOR giữa chúng đo lường sự không đồng nhất trong mẫu bit cuối cùng được tạo ra bởi hai lần nén cực độ này của cùng một tập hợp. Chúng tôi đang tìm kiếm trên tất cả các tập hợp con có kích thước ít nhất là hai, vì vậy không gian tìm kiếm là hàm mũ và việc liệt kê trực tiếp là không thể. 

Các ràng buộc rất lớn, với tổng số phần tử trong tất cả các trường hợp thử nghiệm lên tới ba trăm nghìn. Điều này ngay lập tức loại bỏ mọi phương pháp kiểm tra tất cả các tập hợp con hoặc thậm chí tất cả các cặp có xử lý bổ sung cho mỗi cặp. Một giải pháp chấp nhận được phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một cách tiếp cận ngây thơ có thể thử tất cả các cặp trước và cho rằng các chuỗi con lớn hơn không thể cải thiện câu trả lời. Điều này vốn đáng nghi ngờ vì việc thêm các phần tử sẽ thay đổi cả AND và OR theo những cách không hề tầm thường. Ví dụ: việc thêm số thứ ba có thể làm giảm đáng kể AND trong khi vẫn giữ OR không thay đổi, có khả năng làm giảm kết quả XOR. Điều này có nghĩa là chỉ lý luận theo cặp là không đủ nếu không có sự biện minh. 

Vấn đề tế nhị thứ hai là cho phép các chuỗi con chứ không phải mảng con. Điều này loại bỏ mọi hạn chế về địa phương và củng cố rằng chỉ có sự phân phối giá trị mới quan trọng chứ không phải thứ tự. 

## Phương pháp tiếp cận 

Phương pháp brute-force liệt kê tất cả các chuỗi con có kích thước ít nhất là hai, tính toán AND và OR của chúng và đánh giá XOR. Đối với mỗi tập hợp con, việc tính toán AND và OR mất thời gian tuyến tính theo kích thước tập hợp con, do đó độ phức tạp tổng thể theo thứ tự của tất cả các tập hợp con, vốn là hàm mũ theo n. Ngay cả việc giới hạn theo cặp cũng mang lại O(n²), điều này đã thất bại đối với n lên tới 3·10⁵. 

Thông tin chi tiết quan trọng đến từ việc kiểm tra cách hoạt động của AND và OR trên nhiều phần tử. Đối với vị trí bit cố định, AND chỉ là 1 nếu mọi phần tử được chọn có tập hợp bit đó, trong khi OR là 1 nếu ít nhất một phần tử có tập hợp bit đó. Đối với bất kỳ bit nào, cặp (bit AND, bit OR) chỉ có thể là (0,0), (0,1) hoặc (1,1). XOR chỉ đóng góp khi những thứ này khác nhau, điều này xảy ra chính xác khi OR được đặt bit nhưng AND thì không. Điều kiện đó có nghĩa là: trong số các phần tử được chọn, không phải tất cả các giá trị đều chia sẻ bit đó. 

Bây giờ hãy xem điều gì sẽ xảy ra nếu chúng ta nhìn vào các cặp. Với bất kỳ cặp (x, y) nào, AND là x & y và OR là x | y, do đó điểm trở thành (x & y) XOR (x | y). Một đẳng thức cổ điển cho thấy điều này bằng x XOR y. Điều này là do với mỗi bit, AND và OR khác nhau một cách chính xác khi các bit khác nhau giữa x và y, khớp với hành vi XOR. 

Điều này đơn giản hóa vấn đề một cách đáng kể. Đối với bất kỳ chuỗi con nào có kích thước ít nhất là hai, điểm của nó chỉ phụ thuộc vào việc liệu có tồn tại ít nhất một cặp bên trong nó có đạt được đóng góp nhỏ giống XOR hay không. Vì việc thêm nhiều phần tử hơn không thể làm giảm XOR theo cặp tối thiểu bên trong tập hợp đã chọn, nên một số cặp sẽ luôn đạt được chuỗi con tốt nhất. Bất kỳ bộ lớn hơn nào chỉ giới thiệu các cặp bổ sung và điểm số không thể giảm xuống dưới cặp tốt nhất. 

Do đó, bài toán rút gọn thành việc tìm giá trị nhỏ nhất của (a[i] & a[j]) XOR (a[i] | a[j]) trên tất cả các cặp, tương đương với việc tìm XOR nhỏ nhất trên tất cả các cặp của mảng. 

Điều này làm giảm nhiệm vụ thành bài toán “cặp XOR tối thiểu” cổ điển, có thể giải được bằng cách sử dụng phép thử bitwise trong thời gian tuyến tính cho mỗi lần chèn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(1) | Quá chậm | 
| Thử theo bit cho cặp tối thiểu XOR | O(n log A) | O(n log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Phương pháp tối ưu

1. Coi mỗi số là một số nguyên 31 bit và xử lý từng số một, chèn chúng vào một bộ ba nhị phân. Mỗi nút trie lưu trữ hai nút con tương ứng với bit 0 và bit 1. Cấu trúc này cho phép chúng ta tìm thấy số được chèn trước đó gần nhất một cách hiệu quả theo nghĩa XOR. 
2. Đối với mỗi số đến, trước khi chèn nó, hãy truy vấn thử để tìm số cực tiểu hóa XOR với nó. Điều này được thực hiện bằng cách cố gắng bám theo cùng một bit nếu có thể, vì các bit khớp sẽ tạo ra đóng góp 0 trong XOR, nếu không chúng ta buộc phải chọn nhánh đối diện. 
3. Trong khi truy vấn, hãy tích lũy giá trị XOR dọc theo đường dẫn. Mỗi lần chúng tôi buộc phải phân kỳ, chúng tôi sẽ thêm chi phí vào vị trí bit đó. Điều này xây dựng kết quả phù hợp nhất có thể giữa các số được chèn trước đó. 
4. Cập nhật câu trả lời chung với XOR tối thiểu tìm được cho mỗi phần tử. Điều này đảm bảo chúng tôi xem xét tất cả các cặp, vì mỗi phần tử được ghép với tất cả các phần tử trước đó đúng một lần. 
5. Chèn số hiện tại vào bản thử để nó có sẵn cho các truy vấn trong tương lai. 

### Tại sao nó hoạt động 

Phép thử đảm bảo rằng đối với mỗi số, chúng ta đang chọn một số khác một cách hiệu quả để giảm thiểu sự bất đồng về bit từ bit quan trọng nhất trở xuống. Cấu trúc tham lam này là tối ưu vì bất kỳ cải tiến nào ở bit cao hơn sẽ lấn át tất cả các bit thấp hơn. Vì mỗi cặp được xem xét một lần theo cách bất đối xứng này nên XOR tối thiểu trên tất cả các cặp được ghi lại chính xác một lần và không thể bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("child",)
    def __init__(self):
        self.child = [None, None]

def insert(root, x):
    node = root
    for b in range(30, -1, -1):
        bit = (x >> b) & 1
        if node.child[bit] is None:
            node.child[bit] = Node()
        node = node.child[bit]

def query(root, x):
    node = root
    res = 0
    for b in range(30, -1, -1):
        bit = (x >> b) & 1
        if node.child[bit] is not None:
            node = node.child[bit]
        else:
            node = node.child[bit ^ 1]
            res |= (1 << b)
    return res

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        root = Node()
        insert(root, arr[0])

        ans = float('inf')

        for i in range(1, n):
            x = arr[i]
            ans = min(ans, query(root, x))
            insert(root, x)

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng một trie nhị phân cho mỗi trường hợp thử nghiệm. Mỗi số được chèn từng bit từ bit quan trọng nhất trở xuống. Hàm truy vấn sẽ tìm kiếm các bit phù hợp, chỉ phân kỳ khi cần thiết và tích lũy chi phí XOR thu được. Phần tử đầu tiên được chèn trước khi xử lý để đảm bảo mọi phần tử tiếp theo đều có ít nhất một đối tác ứng cử viên. 

Việc lựa chọn 31 bit phù hợp với ràng buộc rằng các giá trị lên tới 10⁹. Sử dụng độ sâu cố định sẽ tránh được chi phí kiểm tra độ dài bit động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
1 2 3
```Chúng tôi chèn 1 đầu tiên. 

| Bước | Hiện tại x | Trận đấu hay nhất trong tri | Kết quả XOR | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | 3 | 
| 2 | 3 | 1 | 2 | 2 | 

Sau đó chèn 2 và 3 đảm bảo tất cả các cặp đều được xem xét. Tối thiểu là 1, đạt được bằng cách ghép 2 và 3. 

Dấu vết này cho thấy mọi phần tử đều được so sánh với các phần tử trước đó, đảm bảo bao phủ toàn bộ cặp mà không cần tính toán lại tất cả các cặp. 

### Ví dụ 2 

đầu vào:```
1
4
5 1 7 3
```| Bước | Hiện tại x | Trận đấu hay nhất trong tri | Kết quả XOR | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 4 | 4 | 
| 2 | 7 | 5 | 2 | 2 | 
| 3 | 3 | 1 | 2 | 2 | 

Câu trả lời cuối cùng là 2, đạt được bằng nhiều cặp. Điều này cho thấy nhiều ứng viên có thể tạo ra cùng một XOR tối ưu như thế nào và trie đảm bảo tất cả đều được kiểm tra ngầm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi lần chèn và truy vấn đi qua 31 bit | 
| Không gian | O(n log A) | Mỗi số đóng góp tới 31 nút trie | 

Tổng của n là 3·10⁵ và mỗi phép toán là hệ số không đổi 31, phù hợp thoải mái trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    class Node:
        __slots__ = ("child",)
        def __init__(self):
            self.child = [None, None]

    def insert(root, x):
        node = root
        for b in range(30, -1, -1):
            bit = (x >> b) & 1
            if node.child[bit] is None:
                node.child[bit] = Node()
            node = node.child[bit]

    def query(root, x):
        node = root
        res = 0
        for b in range(30, -1, -1):
            bit = (x >> b) & 1
            if node.child[bit] is not None:
                node = node.child[bit]
            else:
                node = node.child[bit ^ 1]
                res |= (1 << b)
        return res

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        root = Node()
        insert(root, arr[0])
        ans = inf
        for i in range(1, n):
            ans = min(ans, query(root, arr[i]))
            insert(root, arr[i])
        out.append(str(ans))
    return "\n".join(out)

# sample
assert run("1\n3\n1 2 3\n") == "1"

# minimum size
assert run("1\n2\n0 0\n") == "0"

# identical values
assert run("1\n4\n7 7 7 7\n") == "0"

# increasing pattern
assert run("1\n4\n1 2 4 8\n") == "3"

# mixed case
assert run("1\n5\n5 1 7 3 2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 0 | giá trị giống nhau | 
| 1 2 4 8 | 3 | mẫu bit có cấu trúc | 
| 5 1 7 3 2 | 1 | tương tác hỗn hợp | 

## Vỏ cạnh 

Đối với các mảng trong đó tất cả các giá trị giống hệt nhau, mỗi cặp đều mang lại XOR bằng 0 và bộ ba ngay lập tức tìm thấy các kết quả trùng khớp giống hệt nhau, tạo ra kết quả bằng 0. Thuật toán chèn giá trị đầu tiên và mọi truy vấn tiếp theo sẽ tìm thấy cùng một đường dẫn bit ở mọi cấp độ. 

Đối với lũy thừa tăng dần của hai, mỗi số khác nhau ở chính xác một vị trí bit và bộ ba buộc phân kỳ ở bit khác nhau cao nhất, tạo ra các giá trị XOR có thể dự đoán được. Truyền tải tham lam xác định chính xác các lân cận gần nhất trong không gian bit. 

Đối với các trường hợp có số 0, việc chèn số 0 trước tiên sẽ đảm bảo rằng tất cả các truy vấn trong tương lai có thể khớp ngay lập tức với các bit 0, thường tạo ra các giá trị XOR rất nhỏ. Bộ tri xử lý chính xác nhánh hoàn toàn bằng 0 mà không cần vỏ đặc biệt.
