---
title: "CF 106170J - Cặp tốt trong đồ thị và cây"
description: "Chúng ta có hai cấu trúc khác nhau trên cùng một tập đỉnh được đánh nhãn từ 1 đến N. Một cấu trúc là một cây, do đó giữa hai đỉnh bất kỳ có đúng một đường đi đơn. Cấu trúc còn lại là một đồ thị đơn giản vô hướng tùy ý."
date: "2026-06-21T16:10:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106170
codeforces_index: "J"
codeforces_contest_name: "Swiss Subregional 2025-2026"
rating: 0
weight: 106170
solve_time_s: 65
verified: true
draft: false
---

[CF 106170J - Cặp tốt trong biểu đồ và cây](https://codeforces.com/problemset/problem/106170/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cấu trúc khác nhau trên cùng một tập đỉnh được đánh nhãn từ 1 đến N. Một cấu trúc là một cây, do đó giữa hai đỉnh bất kỳ có đúng một đường đi đơn. Cấu trúc còn lại là một đồ thị đơn giản vô hướng tùy ý. 

Đối với bất kỳ cặp đỉnh có thứ tự nào (u, v), chúng ta xét đường đi duy nhất nối u với v trên cây. Sau đó, chúng tôi hỏi liệu chuỗi đỉnh chính xác này cũng có thể được duyệt như một đường dẫn hợp lệ trong biểu đồ hay không. Nếu có thì cặp đó được gọi là tốt. Mọi đỉnh đều tốt với chính nó vì đường đi từ u đến u không có cạnh. 

Nhiệm vụ là đếm xem có bao nhiêu cặp đặt hàng tốt. 

Các ràng buộc đặt N và M lên tới 200.000. Điều này ngay lập tức loại trừ mọi giải pháp kiểm tra từng cặp riêng lẻ hoặc mô phỏng đường dẫn cho mỗi truy vấn. Bất kỳ cách tiếp cận nào gần hơn với O(N^2) hoặc thậm chí O(NM) sẽ quá chậm. Chúng ta nên mong đợi một cái gì đó gần với thời gian tuyến tính hoặc tuyến tính, có thể là O(N + M) hoặc O((N + M) log N). 

Một điểm tinh tế là “đường dẫn trong G” nghĩa là gì. Nó không đòi hỏi sự duy nhất, chỉ cần sự tồn tại. Vì vậy, nếu đường đi của cây là u → a → b → v thì chúng ta chỉ cần các cạnh (u, a), (a, b), (b, v) tồn tại trong G; Các cạnh bổ sung trong G không quan trọng miễn là chúng không làm mất hiệu lực sự tồn tại của dãy này. 

Một sai lầm phổ biến là nghĩ rằng chúng ta phải khớp các đường đi ngắn nhất trong G hoặc đảm bảo không có đường đi thay thế nào tồn tại. Điều đó không bắt buộc. 

Trường hợp một cạnh đáng được làm rõ là khi G giống hệt cây. Vậy cặp nào cũng tốt nên đáp án là N2. Một thái cực khác là khi G không có sự chồng lấn với các cạnh của cây; thì chỉ có cặp (u, u) là hợp lệ, cho ra N. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu bắt đầu từ định nghĩa. Đối với mỗi cặp có thứ tự (u, v), chúng ta tính toán đường dẫn duy nhất trong cây, sau đó kiểm tra xem mỗi cạnh trên đường dẫn đó có tồn tại trong G hay không. Nếu độ dài đường dẫn là k thì điều này tốn O(k) trên mỗi cặp. Tổng tất cả các cặp, kết quả này trở thành O(N^2) trong trường hợp xấu nhất vì đường đi của cây có thể dài đối với hầu hết các cặp trong cây hình chuỗi. Ngay cả khi chúng tôi tăng tốc kiểm tra sự tồn tại của cạnh bằng cách sử dụng hàm băm, bản thân số lượng cặp vẫn là bậc hai, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần suy luận về các cặp tùy ý một cách độc lập. Một cặp (u, v) chính xác là tốt khi mọi cạnh trên đường đi giữa chúng tồn tại trong G. Điều này biến bài toán từ bài toán kiểm tra đường đi trên các cặp thành bài toán lọc cạnh trên cây. 

Chúng ta có thể phân loại từng cạnh của cây là “có thể sử dụng được” hoặc “bị hỏng” tùy thuộc vào việc nó có xuất hiện trong G hay không. Nếu chúng ta chỉ giữ lại các cạnh có thể sử dụng được trong cây, chúng ta sẽ có được một khu rừng. Bên trong khu rừng này, hai đỉnh bất kỳ được kết nối khi và chỉ nếu đường cây ban đầu của chúng chỉ sử dụng các cạnh có thể sử dụng được. Điều kiện đó hoàn toàn tương đương với việc cặp tiền này tốt. 

Vì vậy, vấn đề giảm xuống còn việc tìm các thành phần được kết nối trong cây được lọc này và tính tổng số cặp có thứ tự bên trong mỗi thành phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(N) | Quá chậm | 
| Lọc cạnh cây + thành phần | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng cấu trúc thành viên nhanh chóng cho các cạnh của G, sau đó lọc cây tương ứng.

1. Lưu trữ tất cả các cạnh của G trong một tập băm hoặc từ điển được khóa theo cặp có thứ tự (min(x, y), max(x, y)). Điều này cho phép kiểm tra liên tục xem cạnh cây có tồn tại trong G hay không. 
2. Khởi tạo cấu trúc Disjoint Set Union (DSU) cho tất cả N đỉnh, trong đó ban đầu mỗi đỉnh là thành phần riêng của nó. DSU này sẽ thể hiện kết nối chỉ sử dụng các cạnh cây “tốt”. 
3. Với mỗi cạnh (a, b) trong cây, kiểm tra xem nó có tồn tại trong G hay không bằng cách sử dụng tập băm. Nếu có, hãy hợp nhất a và b trong DSU. Nếu không, chúng tôi hoàn toàn bỏ qua nó, coi nó như bị hỏng. 
4. Sau khi xử lý tất cả các cạnh của cây, mỗi thành phần DSU tương ứng chính xác với một thành phần được kết nối được hình thành bởi các cạnh có thể sử dụng được. 
5. Đối với mỗi thành phần DSU, hãy tính kích thước s của nó. Mọi cặp có thứ tự (u, v) bên trong thành phần đều tốt, kể cả (u, u), do đó nó đóng góp s × s. 
6. Tính tổng s × s trên tất cả các thành phần và đưa ra kết quả. 

Điều tinh tế duy nhất là chúng tôi không xây dựng biểu đồ mới một cách rõ ràng. DSU mô phỏng trực tiếp kết nối trong cây được lọc mà không cần danh sách kề. 

### Tại sao nó hoạt động 

Cây đảm bảo một đường đi đơn giản duy nhất giữa hai đỉnh bất kỳ. Đường đi đó hoàn toàn được xác định bởi chuỗi các cạnh của cây. Một cặp (u, v) chính xác là tốt khi mọi cạnh trên đường dẫn duy nhất đó tồn tại trong G. Điều này tương đương với việc nói rằng mọi cạnh trên đường dẫn đều được đánh dấu là có thể sử dụng được. 

Nếu tất cả các cạnh trên đường dẫn đều có thể sử dụng được thì kết nối DSU đảm bảo u và v kết thúc trong cùng một thành phần vì chúng tôi kết hợp chính xác các cạnh đó. Ngược lại, nếu chúng nằm trong cùng một thành phần DSU, sẽ tồn tại một đường đi được tạo hoàn toàn bằng các cạnh có thể sử dụng được, đường đi này phải trùng với đường đi duy nhất của cây vì cây không có đường đi thay thế. Sự tương ứng một-một này đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]

def main():
    n, m = map(int, input().split())

    tree_edges = []
    for _ in range(n - 1):
        a, b = map(int, input().split())
        tree_edges.append((a, b))

    g_edges = set()
    for _ in range(m):
        x, y = map(int, input().split())
        if x > y:
            x, y = y, x
        g_edges.add((x, y))

    dsu = DSU(n)

    for a, b in tree_edges:
        x, y = (a, b) if a < b else (b, a)
        if (x, y) in g_edges:
            dsu.union(a, b)

    comp_size = {}
    for i in range(1, n + 1):
        root = dsu.find(i)
        comp_size[root] = comp_size.get(root, 0) + 1

    ans = 0
    for s in comp_size.values():
        ans += s * s

    print(ans)

if __name__ == "__main__":
    main()
```Đầu tiên, mã lưu trữ tất cả các cạnh của G trong một tập hợp với thứ tự chuẩn hóa sao cho việc tra cứu cạnh có thời gian không đổi. Sau đó, nó lặp qua các cạnh của cây và chỉ kết hợp các điểm cuối khi cạnh đó tồn tại trong G. Sau khi xây dựng DSU, nó tính toán kích thước thành phần bằng cách nén tất cả các đỉnh vào gốc của chúng và tích lũy số lượng. 

Một cạm bẫy triển khai phổ biến là quên chuẩn hóa các cạnh một cách nhất quán giữa cây và biểu đồ. Một cách khác là lặp lại DSU cha mà không nén đường dẫn cuối cùng, điều này có thể tính sai kích thước thành phần. Vòng lặp cuối cùng gọi find trên mọi nút một cách rõ ràng để đảm bảo tính chính xác. 

## Ví dụ đã hoạt động 

Xét cấu trúc mẫu có bốn nút trong một cây dòng và ba cạnh trong G. Các cạnh của cây là (1,2), (2,3), (3,4). Các cạnh của đồ thị cho phép tất cả ngoại trừ (1,2). 

Sau khi lọc, chỉ các cạnh (2,3) và (3,4) còn sử dụng được. Các thành phần DSU trở thành {1}, {2,3,4}. Số tiền đóng góp là 12 + 32 = 1 + 9 = 10. 

Bây giờ hãy xem xét ví dụ thứ hai trong đó cây là một ngôi sao có tâm tại 1 với các cạnh (1,2), (1,3), (1,4) và G chỉ chứa (1,2) và (1,3). 

Sau khi lọc, 1,2,3 tạo thành một thành phần, trong khi 4 bị cô lập. 

Bảng dưới đây theo dõi các công đoàn DSU. 

| Đã xử lý cạnh | Trong G | Trạng thái DSU | 
| --- | --- | --- | 
| (1,2) | vâng | {1,2}, {3}, {4} | 
| (1,3) | vâng | {1,2,3}, {4} | 
| (1,4) | không | không thay đổi | 

Kích thước cuối cùng là 3 và 1, cho 3² + 1² = 10. 

Những ví dụ này cho thấy rằng chỉ có các cạnh của cây được bảo tồn mới quan trọng và mọi lý do đều giảm xuống khả năng kết nối trong cấu trúc được lọc đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M α(N)) | Mỗi lần tra cứu cạnh là O(1), các hoạt động DSU gần như được khấu hao không đổi | 
| Không gian | O(N + M) | Lưu trữ mảng DSU và tập băm của các cạnh G | 

Các ràng buộc cho phép lên tới 200.000 đỉnh và cạnh, do đó, giải pháp dựa trên DSU gần tuyến tính phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return main()  # if integrated; adjust as needed

# Sample case
assert run("""4 3
1 2
2 3
3 4
1 3
3 2
2 4
""") == "6"

# Minimum case
assert run("""3 0
1 2
2 3
""") == "3"

# Fully matching graph and tree
assert run("""4 3
1 2
2 3
3 4
1 2
2 3
3 4
""") == "16"

# Star with partial edges
assert run("""4 2
1 2
1 3
1 4
1 2
1 3
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 nút, không có cạnh G | 3 | xử lý các đỉnh bị cô lập | 
| cây và đồ thị giống hệt nhau | N2 | trường hợp kết nối đầy đủ | 
| sao một phần | 10 | tính chính xác của việc hợp nhất thành phần | 

## Vỏ cạnh 

Trường hợp một cạnh là khi không có cạnh cây nào tồn tại trong G. Trong trường hợp đó, mọi đỉnh vẫn bị cô lập trong DSU. Thuật toán tạo ra N thành phần có kích thước 1, mỗi thành phần đóng góp 1, vì vậy câu trả lời là N. Điều này phù hợp với thực tế là chỉ có cặp (u, u) là hợp lệ. 

Một trường hợp khác là khi G chứa tất cả các cạnh của cây cộng với các cạnh bổ sung. Các cạnh bổ sung không ảnh hưởng đến sự kết hợp DSU vì chỉ các cạnh của cây được xử lý. Toàn bộ cây trở thành một thành phần, tạo ra các cặp có thứ tự N2, điều này đúng vì mọi đường dẫn của cây đều có trong G. 

Trường hợp khó phát hiện cuối cùng là khi G bị ngắt kết nối theo cách không thẳng hàng với cây. Vì DSU bỏ qua hoàn toàn các cạnh không phải là cây nên các cạnh như vậy không thể hợp nhất các thành phần một cách không chính xác. Chỉ có kết nối cây bị hạn chế bởi G mới quan trọng, đó chính xác là điều kiện xác định các cặp tốt.
