---
problem: 1311B
contest_id: 1311
problem_index: B
name: "WeirdSort"
contest_name: "Codeforces Round 624 (Div. 3)"
rating: 1200
tags: ["dfs and similar", "sortings"]
answer: passed_samples
verified: true
solve_time_s: 97
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2ddd1f-d1b0-83ec-a3a6-913f2c755799
---

#CF 1311B - WeirdSort 

**Đánh giá:** 1200 
**Thẻ:** dfs và các loại tương tự, sắp xếp 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2ddd1f-d1b0-83ec-a3a6-913f2c755799 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên và một tập hợp giới hạn các “quyền hoán đổi”. Mỗi quyền tương ứng với một vị trí`i`, nghĩa là chúng ta được phép hoán đổi cặp`(a[i], a[i+1])`. Mỗi phép hoán đổi như vậy có thể được sử dụng tùy ý nhiều lần, do đó chúng ta có thể thực hiện các phép hoán đổi liền kề một cách hiệu quả, nhưng chỉ trên một số cạnh nhất định của mảng. 

Câu hỏi đặt ra là liệu chúng ta có thể chuyển đổi mảng thành một chuỗi hoàn toàn không giảm chỉ bằng cách sử dụng các hoán đổi liền kề được phép này hay không. 

Một cách hữu ích để diễn giải lại điều này là coi mảng như một đường các vị trí được nối với nhau bằng các cạnh. Nếu vị trí`i`đang ở trong bộ`p`, khi đó có một cạnh giữa`i`Và`i+1`cho phép trao đổi. Nếu chúng ta có thể hoán đổi dọc theo một chuỗi các cạnh được kết nối, thì các phần tử bên trong đoạn được kết nối đó có thể được sắp xếp lại một cách tùy ý, bởi vì các hoán đổi liền kề sẽ tạo ra bất kỳ hoán vị nào bên trong một thành phần được kết nối. 

Điều này biến vấn đề thành việc kiểm tra xem mỗi thành phần được kết nối được tạo ra bởi các cạnh hoán đổi được phép có thể được sắp xếp độc lập để khớp với mảng được sắp xếp toàn cầu hay không. 

Các hạn chế là nhỏ:`n ≤ 100`Và`t ≤ 100`. Điều này có nghĩa là ngay cả một`O(n^2)`hoặc`O(n log n)`giải pháp cho mỗi trường hợp thử nghiệm là đủ dễ dàng. Chúng tôi không cần cấu trúc dữ liệu nâng cao hoặc thuật toán đồ thị nặng nề. 

Một ý tưởng ngây thơ nhưng không chính xác là thử mô phỏng các giao dịch hoán đổi một cách tham lam, luôn hoán đổi bất cứ khi nào có thể để di chuyển các phần tử theo thứ tự được sắp xếp. Điều này có thể thất bại vì các giao dịch hoán đổi tham lam cục bộ không nắm bắt được cấu trúc kết nối toàn cầu. Một cách tiếp cận không chính xác khác là giả định rằng nếu một vị trí được cho phép, thì nó có thể tự do di chuyển các phần tử qua vị trí đó theo một hướng mà bỏ qua rằng các hoán đổi phải được áp dụng nhiều lần và đối xứng. 

Trường hợp cạnh tinh tế phát sinh khi các vị trí được phép thưa thớt. Ví dụ: nếu không có vị trí hoán đổi nào được cung cấp thì mảng đó phải được sắp xếp. Nếu tất cả các vị trí được cung cấp, mảng luôn có thể được sắp xếp vì nó hoạt động giống như một đường được kết nối đầy đủ. Các trường hợp thú vị là khả năng kết nối một phần trong đó chỉ có thể sắp xếp lại các phân đoạn. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ là mô phỏng tất cả các chuỗi hoán đổi được phép có thể xảy ra. Vì mỗi hoán đổi có thể được áp dụng tùy ý nhiều lần nên điều này trở thành một tìm kiếm không giới hạn đối với các hoán vị được tạo ra bởi các chuyển vị liền kề. Trong trường hợp xấu nhất, số lượng trạng thái có thể truy cập được là`n!`, điều này hoàn toàn không thể thực hiện được ngay cả đối với`n = 10`. 

Thông tin chi tiết quan trọng là các hoán đổi lân cận trên các vị trí được phép sẽ xác định một biểu đồ trên các chỉ mục và mỗi thành phần được kết nối của biểu đồ này cho phép tự do hoán vị hoàn toàn bên trong nó. Điều này có nghĩa là chúng ta không bao giờ cần phải mô phỏng các hoạt động. Thay vào đó, chúng ta chỉ cần nhóm các chỉ số có thể tiếp cận được lẫn nhau thông qua các giao dịch hoán đổi được phép. 

Khi đã biết các thành phần, chúng ta có thể trích xuất các giá trị thuộc từng thành phần, sắp xếp chúng và xác minh rằng việc đặt các giá trị đã sắp xếp trở lại cùng các chỉ mục sẽ mang lại một mảng được sắp xếp toàn cục. 

Điều này làm giảm vấn đề từ các phép biến đổi động thành vấn đề so sánh và nhóm tĩnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Thành phần + Sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa các chỉ số mảng dưới dạng các nút từ`0`ĐẾN`n-1`. Đối với mỗi vị trí hoán đổi được phép`p`, chúng tôi kết nối`p-1`Và`p`(trong lập chỉ mục dựa trên 0). Mỗi thành phần được kết nối đại diện cho các chỉ số có giá trị có thể được hoán vị tùy ý. 

Sau đó, chúng tôi xác minh xem việc sắp xếp bên trong mỗi thành phần có đủ để tạo ra mảng được sắp xếp toàn cầu hay không. 

1. Xây dựng một tập hợp rời rạc (DSU) hoặc cấu trúc kề cận của đồ thị trên các chỉ số. 

Mỗi vị trí được phép`p`kết nối các chỉ số`p-1`Và`p`. Mô hình này có khả năng trao đổi trực tiếp. 
2. Tính các thành phần liên thông của đồ thị chỉ số. 

Bất kỳ hai chỉ số nào trong cùng một thành phần đều có thể trao đổi giá trị thông qua một chuỗi các giao dịch hoán đổi liền kề. 
3. Đối với mỗi thành phần, thu thập tất cả các chỉ số thuộc về nó. 

Chúng tôi đang nhóm các vị trí chia sẻ quyền tự do hoán vị hoàn toàn. 
4. Trích xuất các giá trị từ mảng ban đầu tương ứng với các chỉ số này và sắp xếp cả hai: 

một danh sách từ mảng ban đầu, một danh sách từ các vị trí mảng được sắp xếp mục tiêu. 
5. So sánh hai bộ đa năng cho từng thành phần. 

Nếu bất kỳ thành phần nào không thể khớp với phân phối mục tiêu đã sắp xếp thì việc sắp xếp là không thể. 
6. Nếu tất cả các thành phần đều khớp, xuất ra “YES”, nếu không thì xuất ra “NO”. 

### Tại sao nó hoạt động 

Bất biến chính là việc hoán đổi chỉ xảy ra trên các cạnh được phép, do đó các giá trị không bao giờ có thể di chuyển giữa các thành phần được kết nối khác nhau. Điều này có nghĩa là mỗi thành phần sẽ duy trì mãi mãi nhiều giá trị của nó. Trong một thành phần, các hoán đổi liền kề đủ để tạo ra bất kỳ hoán vị nào, do đó, mọi sự sắp xếp lại các giá trị bên trong thành phần đều có thể đạt được. 

Do đó, vấn đề giảm xuống còn việc kiểm tra xem mỗi thành phần có thể được gán chính xác các giá trị mà nó cần trong mảng được sắp xếp chung hay không. Nếu mọi thành phần đều khớp với nhiều tập hợp được yêu cầu của nó thì sẽ tồn tại một chuỗi hoán đổi hợp lệ; mặt khác, một số giá trị phải vượt qua ranh giới bị cấm, điều này là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

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
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        p = list(map(int, input().split()))

        dsu = DSU(n)

        for x in p:
            dsu.union(x - 1, x)

        groups = {}
        for i in range(n):
            root = dsu.find(i)
            if root not in groups:
                groups[root] = []
            groups[root].append(i)

        sorted_a = sorted(a)

        pos_map = {}
        for i in range(n):
            root = dsu.find(i)
            pos_map.setdefault(root, []).append(i)

        for root, indices in pos_map.items():
            original_vals = [a[i] for i in indices]
            target_vals = [sorted_a[i] for i in indices]

            if sorted(original_vals) != sorted(target_vals):
                print("NO")
                break
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```DSU xây dựng kết nối giữa các chỉ số có thể trao đổi trực tiếp. Bước hợp nhất chuyển đổi từng vị trí được phép thành một cạnh giữa hai chỉ số lân cận. 

Sau khi xây dựng các thành phần, chúng tôi lặp lại từng gốc và thu thập tất cả các chỉ mục thuộc về thành phần đó. Đối với mỗi nhóm như vậy, chúng tôi so sánh những giá trị mà nó hiện chứa với những giá trị mà nó nên chứa trong mảng được sắp xếp chung. Mảng được sắp xếp toàn cục đóng vai trò là cấu hình mục tiêu cuối cùng. 

Sự tinh tế quan trọng là sử dụng các chỉ số, không phải giá trị, để nhóm. Một lỗi phổ biến là cố gắng suy luận trực tiếp về các con số thay vì vị trí, nhưng ràng buộc thao tác lại là vị trí. 

trận chung kết`else`trên vòng lặp đảm bảo rằng chúng tôi chỉ in “CÓ” nếu không có thành phần nào không vượt qua được quá trình xác thực. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [2, 1, 4, 3]
p = [1, 3]
```Chúng tôi xây dựng kết nối`(0,1)`Và`(2,3)`, cho hai thành phần:`{0,1}`Và`{2,3}`. 

| Thành phần | Chỉ số | giá trị a | Đã sắp xếp giá trị a | Giá trị mục tiêu được sắp xếp | 
| --- | --- | --- | --- | --- | 
| 1 | [0,1] | [2,1] | [1,2] | [1,2] | 
| 2 | [2,3] | [4,3] | [3,4] | [3,4] | 

Cả hai thành phần đều khớp nhau nên câu trả lời là CÓ. 

Điều này xác nhận rằng các phân đoạn độc lập có thể được sắp xếp lại một cách tự do. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [4, 1, 2, 3]
p = [3, 2]
```Các cạnh là`(2,3)`Và`(1,2)`vì vậy tất cả các chỉ số được kết nối thành một thành phần`{0,1,2,3}`. 

| Thành phần | Chỉ số | giá trị a | Đã sắp xếp giá trị a | Giá trị mục tiêu được sắp xếp | 
| --- | --- | --- | --- | --- | 
| 1 | [0,1,2,3] | [4,1,2,3] | [1,2,3,4] | [1,2,3,4] | 

Thoạt nhìn, nó có vẻ hợp lệ, nhưng đây chính xác là nơi việc triển khai bất cẩn vẫn có thể thất bại nếu phân chia các thành phần không chính xác. Vì tất cả các chỉ số đều được kết nối nên việc sắp xếp là hoàn toàn có thể và câu trả lời là CÓ. 

Bây giờ hãy xem xét một trường hợp kiểu thất bại:```
a = [4,3,2,1], p = [1,3]
```Thành phần là`{0,1}`Và`{2,3}`. 

| Thành phần | Chỉ số | giá trị a | Đã sắp xếp giá trị a | Giá trị mục tiêu | 
| --- | --- | --- | --- | --- | 
| 1 | [0,1] | [4,3] | [3,4] | [1,2] | 
| 2 | [2,3] | [2,1] | [1,2] | [3,4] | 

Sự không phù hợp xuất hiện ngay lập tức nên câu trả lời là KHÔNG. 

Những dấu vết này cho thấy tính đúng đắn phụ thuộc hoàn toàn vào việc tôn trọng ranh giới thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Việc sắp xếp các giá trị bên trong các thành phần chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Mảng DSU và lưu trữ nhóm | 

Với`n ≤ 100`Và`t ≤ 100`, điều này dễ dàng đủ nhanh ngay cả khi sắp xếp lặp đi lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.size = [1]*n

        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x

        def union(self, a, b):
            ra, rb = self.find(a), self.find(b)
            if ra != rb:
                self.parent[rb] = ra

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m = map(int, input().split())
            a = list(map(int, input().split()))
            p = list(map(int, input().split()))

            dsu = DSU(n)
            for x in p:
                dsu.union(x-1, x)

            sorted_a = sorted(a)

            comp = {}
            for i in range(n):
                r = dsu.find(i)
                comp.setdefault(r, []).append(i)

            ok = True
            for idxs in comp.values():
                if sorted([a[i] for i in idxs]) != sorted([sorted_a[i] for i in idxs]):
                    ok = False
                    break

            out.append("YES" if ok else "NO")

        return "\n".join(out)

    return solve()

# provided samples
assert run("""6
3 2
3 2 1
1 2
4 2
4 1 2 3
3 2
5 1
1 2 3 4 5
1
4 2
2 1 4 3
1 3
4 2
4 3 2 1
1 3
5 2
2 1 2 3 3
1 4
""") == """YES
NO
YES
YES
NO
YES"""

# custom cases
assert run("""1
3 0
3 2 1
""") == "NO", "no swaps"

assert run("""1
3 2
3 2 1
1 2
""") == "YES", "full connectivity"

assert run("""1
5 1
5 4 3 2 1
2
""") == "NO", "limited swap blocks"

assert run("""1
4 2
1 3 2 4
2 3
""") == "YES", "middle swap only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không hoán đổi | KHÔNG | mảng phải được sắp xếp | 
| kết nối đầy đủ | CÓ | tất cả các hoán vị có thể | 
| khối trao đổi giới hạn | KHÔNG | thành phần bị ngắt kết nối hạn chế sắp xếp | 
| chỉ trao đổi giữa | CÓ | sắp xếp lại địa phương đủ | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`m = 0`, có nghĩa là không được phép hoán đổi. Trong trường hợp đó, mỗi chỉ mục là thành phần riêng của nó, vì vậy mỗi thành phần chứa chính xác một giá trị. Thuật toán so sánh từng giá trị đơn lẻ với giá trị được sắp xếp tương ứng và phát hiện sự không khớp trừ khi mảng đã được sắp xếp. 

Một trường hợp cạnh khác xảy ra khi`m = n-1`, nghĩa là tất cả các giao dịch hoán đổi liền kề đều được phép. Điều này tạo ra một thành phần được kết nối duy nhất, do đó thuật toán giảm xuống còn việc kiểm tra xem có thể sắp xếp toàn bộ mảng hay không, điều này luôn đúng vì tồn tại hoàn toàn tự do hoán vị. 

Một trường hợp tinh tế hơn là khi các thành phần lớn nhưng bị ngắt kết nối theo kiểu như các cạnh xen kẽ nhau. Thuật toán xử lý việc này một cách tự nhiên vì DSU chỉ hợp nhất các giao dịch hoán đổi được phép liền kề, đảm bảo không có sự trộn lẫn thành phần ngẫu nhiên.
