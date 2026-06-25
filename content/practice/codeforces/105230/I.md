---
title: "CF 105230I - Pizza"
description: "Chúng tôi được cung cấp một bộ nguyên liệu và một số cặp nguyên liệu được biết là có mùi vị giống nhau. Mối quan hệ đó không chỉ theo cặp mà còn mở rộng một cách bắc cầu, vì vậy nếu thành phần A phù hợp với B và B phù hợp với C thì A, B và C đều thuộc cùng một nhóm hương vị."
date: "2026-06-24T16:01:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105230
codeforces_index: "I"
codeforces_contest_name: "2024-2025 ICPC Bolivia Pre-National Contest"
rating: 0
weight: 105230
solve_time_s: 77
verified: true
draft: false
---

[CF 105230I - Pizza](https://codeforces.com/problemset/problem/105230/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ nguyên liệu và một số cặp nguyên liệu được biết là có mùi vị giống nhau. Mối quan hệ đó không chỉ theo cặp mà còn mở rộng một cách bắc cầu, vì vậy nếu thành phần A phù hợp với B và B phù hợp với C thì A, B và C đều thuộc cùng một nhóm hương vị. 

Quá trình tạo ra một chiếc bánh pizza được mô tả là lấy thứ tự ngẫu nhiên của tất cả các thành phần và quét chúng từ trái sang phải. Khi chúng tôi nhìn thấy một thành phần, chúng tôi cố gắng thêm nó vào. Theo thông tin tương đương, nó chỉ được thêm vào nếu nó mang lại một hương vị chưa từng xuất hiện trước đây trong số các thành phần đã được chọn. Nếu không nó sẽ bị bỏ qua. 

Đầu ra cuối cùng không phải là về một lần thực hiện. Chúng tôi được hỏi có bao nhiêu bộ thành phần khác nhau có thể được tạo ra bằng quy trình này trên tất cả các hoán vị có thể có của các thành phần. Hai chiếc bánh pizza sẽ khác nhau nếu bộ nguyên liệu thực tế được chọn khác nhau, không chỉ về thứ tự. 

Ràng buộc n lên tới 100000 và m lên tới 100000 ngụ ý về cơ bản chúng ta cần xử lý tuyến tính hoặc gần tuyến tính. Bất kỳ cách tiếp cận nào cố gắng liệt kê các hoán vị hoặc mô phỏng thứ tự đều không thể thực hiện được vì n giai thừa quá lớn và thậm chí hành vi bậc hai trên các cạnh sẽ có nguy cơ hết thời gian chờ. 

Một khó khăn tinh tế là quan hệ tương đương có tính bắc cầu nhưng chỉ được cho dưới dạng các cạnh. Một cách tiếp cận ngây thơ chỉ xem xét các cặp trực tiếp sẽ phân loại sai các nhóm hương vị. 

Một điểm khó khăn khác là giải thích tính ngẫu nhiên. Vấn đề không phải là yêu cầu xác suất hoặc giá trị mong đợi đối với các hoán vị. Nó đang yêu cầu số lượng tập hợp kết quả riêng biệt có thể xuất hiện cho bất kỳ hoán vị nào. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là tạo ra mọi hoán vị của n thành phần, mô phỏng quá trình quét và lưu trữ tập hợp đã chọn kết quả. Bản thân mô phỏng là tuyến tính theo n, vì vậy đây sẽ là thời gian O(n · n!), điều này ngay lập tức không thể thực hiện được ngay cả đối với n rất nhỏ. Nút thắt không phải là sự mô phỏng mà là sự bùng nổ của các đơn đặt hàng có thể có. 

Nhận xét quan trọng là điều duy nhất quan trọng đối với đồ thị của các quan hệ cùng hương vị là các thành phần được kết nối của nó. Bên trong một thành phần được kết nối, khi bất kỳ thành phần nào từ thành phần đó được chọn, mọi thành phần khác trong thành phần đó sẽ được coi là có hương vị đã thấy và sẽ bị loại bỏ khi gặp phải sau này. 

Bây giờ hãy xem xét điều gì quyết định tập hợp cuối cùng cho một hoán vị cố định. Trong mỗi thành phần được kết nối, chính xác một thành phần sẽ là thành phần đầu tiên gặp trong hoán vị đó giữa thành phần đó. Thành phần đó được chọn và tất cả những thành phần khác trong cùng thành phần đều bị bỏ qua. Điều này có nghĩa là mọi hoán vị đều tạo ra một tập hợp chứa chính xác một đại diện từ mỗi thành phần được kết nối. 

Điểm quan trọng là các hoán vị khác nhau có thể chọn các đại diện khác nhau từ cùng một thành phần và những lựa chọn này độc lập giữa các thành phần. Trong một thành phần có kích thước s, bất kỳ nút s nào của nó có thể là nút đầu tiên xuất hiện trong hoán vị so với thành phần đó và do đó trở thành đại diện được chọn. Vì các thành phần không ảnh hưởng lẫn nhau nên tổng số bánh pizza có thể có là tích của các kích thước thành phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) | O(n · n!) | O(n) | Quá chậm | 
| Các thành phần được kết nối + đếm | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta hiểu các cặp đã cho là các cạnh trong đồ thị vô hướng. Hai thành phần được kết nối bằng một đường dẫn thuộc cùng một nhóm hương vị nên chúng ta cần tính toán các thành phần được kết nối. Điều này có thể được thực hiện bằng cách sử dụng cấu trúc tập hợp rời rạc hoặc DFS.

Sau khi nhóm các nút thành các thành phần, chúng tôi tính toán kích thước của từng thành phần. Mỗi thành phần đóng góp độc lập vào số lượng cuối cùng vì quá trình đặt hàng không bao giờ kết hợp cách một thành phần giải quyết đại diện đã chọn của nó. 

Cuối cùng, với mỗi thành phần có kích thước s, chúng ta nhân câu trả lời với s, vì có chính xác s lựa chọn nút nào sẽ trở thành nút gặp đầu tiên của thành phần đó trong phép hoán vị ngẫu nhiên. 

Chúng ta lấy tích modulo 10^9 + 7. 

Lý do nó hoạt động dựa trên tính bất biến là trong bất kỳ hoán vị nào, chính xác một phần tử cho mỗi thành phần được kết nối sẽ được chọn, cụ thể là phần tử sớm nhất của thành phần đó theo thứ tự. Việc ánh xạ từ hoán vị đến kết quả là pizza được xác định hoàn toàn bằng việc lựa chọn phần tử sớm nhất này trong mỗi thành phần. Mọi lựa chọn như vậy đều có thể đạt được bằng cách xây dựng một hoán vị trong đó phần tử được chọn đó xuất hiện trước tất cả các phần tử khác trong thành phần của nó và các thành phần không ràng buộc lẫn nhau, do đó các lựa chọn này nhân lên một cách độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]

def solve():
    n, m = map(int, input().split())
    dsu = DSU(n)

    for _ in range(m):
        a, b = map(int, input().split())
        dsu.union(a - 1, b - 1)

    comp_size = {}
    for i in range(n):
        r = dsu.find(i)
        comp_size[r] = comp_size.get(r, 0) + 1

    ans = 1
    for sz in comp_size.values():
        ans = (ans * sz) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng các thành phần được kết nối bằng cách sử dụng cấu trúc tập hợp rời rạc. Mỗi liên minh hợp nhất các thành phần tương đương với hương vị thành một bộ đại diện duy nhất. 

Sau khi xử lý tất cả các cạnh, chúng tôi lặp qua tất cả các nút một lần để đếm kích thước của từng thành phần gốc. Bước này là cần thiết vì DSU chỉ duy trì cấu trúc chứ không duy trì danh sách thành phần rõ ràng. 

Vòng lặp cuối cùng nhân kích thước của tất cả các thành phần theo số học modulo. Kết quả trực tiếp tương ứng với số cách mà mỗi thành phần có thể đóng góp thành phần đại diện của nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
1 3
2 4
3 5
```Chúng tôi hình thành các thành phần bằng cách hợp nhất các thành phần được kết nối. 

| Bước | Cạnh | Thành phần DSU (khái niệm) | 
| --- | --- | --- | 
| 1 | 1-3 | {1,3}, {2}, {4}, {5} | 
| 2 | 2-4 | {1,3}, {2,4}, {5} | 
| 3 | 3-5 | {1,3,5}, {2,4} | 

Bây giờ chúng ta tính kích thước 3 và 2. Câu trả lời là 3 × 2 = 6. 

Điều này cho thấy mỗi thành phần đóng góp độc lập và các hoán vị chỉ quyết định phần tử nào sẽ được nhìn thấy đầu tiên trong nhóm của nó. 

### Mẫu 2 

đầu vào:```
20 9
1 9
2 6
2 16
6 20
7 13
8 15
10 20
16 18
17 18
```Chúng tôi theo dõi các sự hợp nhất chính: 

| Thành phần | Thành viên | Kích thước | 
| --- | --- | --- | 
| A | {1,9} | 2 | 
| B | {2,6,16,20,10,18,17} | 7 | 
| C | {7,13} | 2 | 
| D | {8,15} | 2 | 

Tích là 2 × 7 × 2 × 2 = 56. 

Điều này xác nhận rằng ngay cả các thành phần được kết nối lớn cũng hoạt động theo cách tương tự: chúng chỉ đóng góp theo cấp số nhân thông qua kích thước của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m α(n)) | Hoạt động DSU với tính năng tìm/kết hợp khấu hao gần như không đổi cộng với một lần chuyển qua các nút | 
| Không gian | O(n) | Mảng gốc và mảng kích thước cộng với bản đồ đếm thành phần | 

Các ràng buộc cho phép lên tới 100000 nút và cạnh, do đó, giải pháp DSU gần tuyến tính phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else __import__("builtins").open  # placeholder
```Vì chúng ta không thể thu thập trực tiếp thiết bị xuất chuẩn ở định dạng này một cách rõ ràng nếu không tái cấu trúc bộ giải, dưới đây là các kiểm tra logic kiểu khẳng định giả định rằng giải() được điều chỉnh để trả về một chuỗi.```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""5 3
1 3
2 4
3 5
""") == "6"

assert run("""20 9
1 9
2 6
2 16
6 20
7 13
8 15
10 20
16 18
17 18
""") == "56"

# single node
assert run("""1 0
""") == "1"

# all isolated
assert run("""4 0
""") == "1"

# chain
assert run("""4 3
1 2
2 3
3 4
""") == "4"

# fully connected
assert run("""4 6
1 2
2 3
3 4
1 3
1 4
2 4
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ sở | 
| không có cạnh | 1 | tất cả các thành phần đơn lẻ | 
| đồ thị chuỗi | 4 | một thành phần có kích thước n | 
| đồ thị hoàn chỉnh | 4 | thành phần lớn duy nhất | 

## Vỏ cạnh 

Khi không có cạnh, mỗi thành phần sẽ tạo thành thành phần riêng của nó. Thuật toán coi mỗi nút là một gốc riêng biệt và tích số liên tục trở thành 1, dẫn đến 1, khớp với thực tế là mọi hoán vị đều mang lại cùng một mẫu lựa chọn xác định trên các thành phần đơn lẻ. 

Khi tất cả các thành phần được kết nối thông qua một chuỗi, DSU sẽ nén mọi thứ thành một thành phần. Câu trả lời cuối cùng là n, vì bất kỳ nút nào trong n nút đều có thể là nút đầu tiên gặp trong thành phần đó và điều đó xác định đầy đủ bộ bánh pizza kết quả. 

Khi các thành phần có mức độ mất cân bằng cao, chẳng hạn như một thành phần lớn và nhiều nút biệt lập, phép nhân vẫn hoạt động chính xác vì các nút biệt lập đóng góp hệ số 1, chỉ để lại kích thước thành phần lớn để xác định tính biến thiên.
