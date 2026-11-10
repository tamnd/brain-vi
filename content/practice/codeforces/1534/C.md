---
title: "CF 1534C - Câu đố của bé Aawn"
description: "Chúng ta có một lưới 2 x n trong đó mỗi hàng là một hoán vị của các số từ 1 đến n. Vì vậy, mỗi số xuất hiện chính xác một lần trong mỗi hàng và mỗi cột chứa hai giá trị riêng biệt. Hoạt động duy nhất được phép là hoán đổi hai giá trị bên trong bất kỳ cột nào."
date: "2026-06-10T15:58:34+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "dsu", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 1534
codeforces_index: "C"
codeforces_contest_name: "Codeforces LATOKEN Round 1 (Div. 1 + Div. 2)"
rating: 1300
weight: 1534
solve_time_s: 106
verified: true
draft: false
---

[CF 1534C - Câu đố của Little Alawn](https://codeforces.com/problemset/problem/1534/C) 

**Đánh giá:** 1300 
**Tags:** tổ hợp, dp, dsu, đồ thị, toán 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới 2 x n trong đó mỗi hàng là một hoán vị của các số từ 1 đến n. Vì vậy, mỗi số xuất hiện chính xác một lần trong mỗi hàng và mỗi cột chứa hai giá trị riêng biệt. 

Hoạt động duy nhất được phép là hoán đổi hai giá trị bên trong bất kỳ cột nào. Sau khi thực hiện bất kỳ số lần hoán đổi cột nào như vậy, mỗi cột vẫn chứa hai số giống nhau nhưng thứ tự dọc của chúng có thể lật độc lập. 

Chúng ta được yêu cầu đếm xem có thể thu được bao nhiêu lưới cuối cùng hợp lệ riêng biệt sao cho cả hai hàng vẫn giữ nguyên hoán vị từ 1 đến n sau tất cả các lần hoán đổi cột. Hai cấu hình được coi là khác nhau nếu có ít nhất một hàng khác nhau trong một chuỗi. 

Ràng buộc chính là sau khi hoán đổi các cột một cách độc lập, một hàng vẫn phải chứa tất cả các số đúng một lần. Điều này có nghĩa là chúng ta không thể lật các cột một cách tùy ý: một số lựa chọn lật sẽ tạo ra các bản sao trong một hàng và không hợp lệ. 

Kích thước đầu vào lớn, với n lên tới 4e5 trên tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào thử tất cả các kết hợp lật cột sẽ kiểm tra 2^n trạng thái, điều này là không thể. Ngay cả việc xây dựng đồ thị là bậc hai theo n cũng sẽ thất bại, vì vậy chúng tôi đang tìm kiếm thứ gì đó tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi các chu trình tương tác: 

Ví dụ: hãy xem xét các cột:```
1 2
2 1
```Ở đây cả hai cột đều đối xứng và việc lật một trong hai cột không làm thay đổi tính hợp lệ. Điều này dẫn đến nhiều cấu hình toàn cục hợp lệ và lý do độc lập ngây thơ trên mỗi cột sẽ bị tính quá mức trừ khi chúng tôi tính đến các ràng buộc về tính nhất quán. 

Một trường hợp cạnh khác là khi cấu trúc tạo thành một chuỗi xen kẽ duy nhất trên các cột, buộc chính xác hai trạng thái toàn cục thay vì nhiều trạng thái cục bộ.

 ## Phương pháp tiếp cận 

Mỗi cột chứa một cặp giá trị và hành động duy nhất là hoán đổi vị trí dọc của chúng. Điều này gợi ý rằng mỗi cột hoạt động giống như một lựa chọn nhị phân. Nếu chúng ta bỏ qua các ràng buộc, câu trả lời sẽ chỉ là 2^n. 

Quan điểm ngây thơ này đúng về mặt tạo cấu hình, nhưng nó bỏ qua ràng buộc toàn cục: sau khi lật, mỗi hàng vẫn phải là một hoán vị. Điều đó có nghĩa là mỗi số phải xuất hiện chính xác một lần trong mỗi hàng, do đó, một số không thể bị trùng lặp trong một hàng do hướng cột không nhất quán. 

Chúng ta có thể diễn giải lại vấn đề như sau. Mỗi số xuất hiện chính xác trong một cột ở hàng trên cùng và trong chính xác một cột ở hàng dưới cùng. Nếu chúng ta coi mỗi số là kết nối hai cột thì mỗi cột tạo ra một kết nối giữa hai vị trí trong biểu đồ: vị trí của các giá trị ở hàng 1 và hàng 2 xác định các cạnh. Mỗi cột là một cạnh giữa hai số và việc lật một cột sẽ hoán đổi điểm cuối thuộc về hàng nào. 

Điều này tạo thành một biểu đồ về các số trong đó mỗi cột là một cạnh vô hướng. Ràng buộc rằng các hàng vẫn giữ nguyên hoán vị có nghĩa là yêu cầu rằng ở hướng cuối cùng, mỗi nút có chính xác một cạnh đi vào hàng 1 và một cạnh vào hàng 2, điều này buộc phải có sự nhất quán về hướng dọc theo các thành phần được kết nối. 

Mỗi thành phần được kết nối hoạt động độc lập. Bên trong một thành phần, sau khi chúng tôi sửa hướng của một cột, phần còn lại sẽ bị ép buộc bằng cách truyền: việc lật một cột sẽ xác định cách giải quyết các ràng buộc liền kề để tránh trùng lặp. Đối với một thành phần dạng cây, điều này mang lại chính xác 2 hướng toàn cục hợp lệ. Đối với các thành phần có chu kỳ, tính nhất quán vẫn có thể cho phép chính xác là 2, nhưng quan sát quan trọng là mọi thành phần được kết nối đều đóng góp hệ số 2 cho câu trả lời. 

Do đó, nhiệm vụ giảm xuống còn việc xây dựng biểu đồ theo các giá trị từ 1 đến n, kết nối hai giá trị xuất hiện trong cùng một cột và đếm các thành phần được kết nối. Câu trả lời là 2^(số thành phần). 

Lực lượng vũ phu sẽ thử tất cả các mẫu lật 2^n và kiểm tra tính hợp lệ theo O(n) cho mỗi lần kiểm tra, đưa ra O(n·2^n). Cách tiếp cận tối ưu nén tất cả các ràng buộc vào cấu trúc DSU và giảm vấn đề đếm các thành phần trong O(n α(n)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^n) | O(n) | Quá chậm | 
| Tối ưu (thành phần DSU) | O(n α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Coi mỗi giá trị từ 1 đến n là một nút trong biểu đồ. Mỗi cột kết nối hai giá trị xuất hiện trong cột đó. Điều này xây dựng một đồ thị vô hướng trên n nút. 
2. Khởi tạo cấu trúc hợp tập hợp rời rạc trên tất cả các nút. Cấu trúc này sẽ theo dõi kết nối giữa các giá trị do cột tạo ra. 
3. Đối với mỗi cột, lấy hai giá trị x và y và hợp nhất chúng trong DSU. Điều này mã hóa rằng chúng phải thuộc cùng một cấu trúc được kết nối vì chúng xuất hiện cùng nhau và việc hoán đổi chỉ ảnh hưởng đến hướng chứ không ảnh hưởng đến việc ghép nối. 
4. Sau khi xử lý tất cả các cột, hãy tính số lượng thành phần được kết nối trong DSU. Mỗi nghiệm đại diện tương ứng với một cấu trúc ràng buộc độc lập. 
5. Gọi k là số thành phần được kết nối. Câu trả lời cuối cùng là 2^k modulo 1e9+7. 

Lý do lũy thừa xuất hiện là vì mỗi thành phần thừa nhận chính xác hai hướng toàn cầu nhất quán. Khi hướng của một thành phần được chọn, tất cả các cột bên trong nó sẽ bị ép buộc và không có xung đột nào phát sinh. 

### Tại sao nó hoạt động

Mỗi cột xác định một mối quan hệ cứng nhắc giữa hai giá trị. Hoán đổi không phá vỡ tính liền kề, nó chỉ thay đổi việc gán cho các hàng. Các mối quan hệ này lan truyền trên các thành phần được kết nối, nghĩa là tất cả các giá trị trong một thành phần đều bị ràng buộc lẫn nhau. Toàn bộ không gian cấu hình phân tích thành thừa số trên các thành phần và mỗi thành phần có chính xác hai cách nhất quán để chỉ định tư cách thành viên hàng, tương ứng với việc chọn hướng truyền ban đầu. Tính độc lập của các thành phần đảm bảo tính nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

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
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]

def mod_pow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

t = int(input())
for _ in range(t):
    n = int(input())
    top = list(map(int, input().split()))
    bottom = list(map(int, input().split()))

    dsu = DSU(n)

    for i in range(n):
        dsu.union(top[i], bottom[i])

    roots = set()
    for i in range(1, n + 1):
        roots.add(dsu.find(i))

    print(mod_pow(2, len(roots)))
```DSU được sử dụng để hợp nhất tất cả các giá trị xuất hiện trong cùng một cột, nắm bắt ràng buộc rằng chúng thuộc về cùng một cấu trúc được kết nối. Sau khi xử lý tất cả các cột, chúng tôi quét tất cả các nút để đếm xem còn lại bao nhiêu đại diện riêng biệt. 

Bước lũy thừa là cần thiết vì mỗi thành phần được kết nối đóng góp một lựa chọn nhị phân. Việc triển khai nguồn nhanh đảm bảo chúng tôi có thể xử lý tổng kích thước lên tới 4e5 một cách hiệu quả. 

Một cạm bẫy triển khai phổ biến là quên nén đường dẫn hoặc đếm các thành phần không chính xác mà không áp dụng tìm kiếm cho mọi nút, điều này sẽ đánh giá thấp số lượng gốc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
top    = [1, 4, 2, 3]
bottom = [3, 2, 1, 4]
```Chúng tôi xử lý công đoàn theo từng cột. 

| Bước | Liên minh | thành phần DSU | 
| --- | --- | --- | 
| 1 | (1,3) | {1,3}, {2}, {4} | 
| 2 | (4,2) | {1,3}, {2,4} | 
| 3 | (2,1) | {1,2,3,4} | 
| 4 | (3,4) | {1,2,3,4} | 

Số thành phần cuối cùng là 1, vì vậy câu trả lời là 2^1 = 2. 

Điều này cho thấy rằng mặc dù có nhiều cột nhưng tất cả các giá trị đều được kết nối vào một hệ thống ràng buộc, chỉ để lại hai hướng chung nhất quán. 

### Ví dụ 2 

đầu vào:```
n = 8
top    = [2,6,5,1,4,3,7,8]
bottom = [3,8,7,5,1,2,4,6]
```Chúng tôi lại hợp nhất từng cặp cột. 

Sau khi xử lý tất cả các phép hợp, chúng ta thu được 4 thành phần được kết nối: 

{1,3,5}, {2,6,8}, {4}, {7} 

Vậy k = 4, đáp án = 2^4 = 8. 

Ví dụ này cho thấy các cấu trúc rời rạc tiến triển độc lập và mỗi cấu trúc đóng góp một hệ số nhân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) | Mỗi cột thực hiện liên kết DSU và việc đếm thành phần là tuyến tính | 
| Không gian | O(n) | Mảng DSU và bộ gốc đã truy cập | 

Tổng n trên các trường hợp thử nghiệm được giới hạn bởi 4e5, do đó quá trình xử lý DSU tuyến tính dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ vẫn tuyến tính và ổn định qua các thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

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
            ra, rb = self.find(a), self.find(b)
            if ra == rb:
                return
            if self.size[ra] < self.size[rb]:
                ra, rb = rb, ra
            self.parent[rb] = ra
            self.size[ra] += self.size[rb]

    def mod_pow(a, e):
        res = 1
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        top = list(map(int, input().split()))
        bottom = list(map(int, input().split()))

        dsu = DSU(n)
        for i in range(n):
            dsu.union(top[i], bottom[i])

        roots = set(dsu.find(i) for i in range(1, n + 1))
        out.append(str(mod_pow(2, len(roots))))

    return "\n".join(out)

# provided samples
assert run("""2
4
1 4 2 3
3 2 1 4
8
2 6 5 1 4 3 7 8
3 8 7 5 1 2 4 6
""") == """2
8"""

# minimum case
assert run("""1
2
1 2
2 1
""") == "2"

# already split components
assert run("""1
3
1 2 3
1 2 3
""") == "8"

# fully connected chain
assert run("""1
4
1 2 3 4
2 3 4 1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trao đổi 2x2 | 2 | hành vi chu kỳ tối thiểu | 
| cột nhận dạng | 8 | nhiều thành phần bị cô lập | 
| sự dịch chuyển theo chu kỳ | 2 | đồ thị được kết nối đầy đủ | 

## Vỏ cạnh 

Một cấu hình tối thiểu như`n = 2`trong đó cả hai cột là các cặp giống hệt nhau cho thấy ngay cả khi mỗi cột trông có vẻ độc lập thì DSU vẫn thu gọn mọi thứ thành một thành phần. Chạy các liên kết thuật toán (1,2) hai lần, để lại một gốc duy nhất và tạo ra 2 trạng thái, khớp với hai lần hoán đổi hàng toàn cầu có thể có. 

Một cấu hình hoàn toàn bị ngắt kết nối trong đó mỗi cột kết nối các cặp đã giống hệt nhau thể hiện thái cực ngược lại. Mỗi nút vẫn bị cô lập nên mỗi nút đóng góp thành phần riêng, dẫn đến cấu hình 2^n. DSU đương nhiên duy trì điều này vì không có công đoàn nào hợp nhất các thành phần. 

Một chu kỳ lớn duy nhất cho thấy hiệu ứng lan truyền hạn chế rõ ràng nhất. Mặc dù mọi giá trị đều được kết nối, nhưng khi DSU hợp nhất mọi thứ thành một thành phần, thuật toán sẽ giảm chính xác toàn bộ không gian cấu hình xuống đúng hai hướng chung hợp lệ.
