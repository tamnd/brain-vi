---
problem: 924A
contest_id: 924
problem_index: A
name: "Mystical Mosaic"
contest_name: "VK Cup 2018 - Round 2"
rating: 1300
tags: ["greedy", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 120
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a326e8d-a098-83ec-99d9-7153824898fc
---

#CF 924A - Khảm huyền bí 

**Đánh giá:** 1300 
**Tags:** tham lam, thực hiện 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a326e8d-a098-83ec-99d9-7153824898fc 

--- 

##Giải pháp 

## Hiểu vấn đề 

Lưới bắt đầu hoàn toàn trống và chúng ta có mẫu cuối cùng gồm các ô đen trắng. Quá trình tạo các ô đen không phải là vẽ tùy ý mà là một thao tác có cấu trúc: mỗi thao tác chọn một số hàng và một số cột, sau đó vẽ mọi giao điểm giữa các hàng và cột đã chọn đó. Điều đó có nghĩa là một thao tác luôn tạo ra tích chéo hình chữ nhật đầy đủ giữa các hàng và cột đã chọn của nó. 

Có một hạn chế bổ sung làm cho cấu trúc trở nên cứng nhắc. Mỗi hàng và mỗi cột có thể tham gia tối đa một thao tác. Vì vậy, khi một hàng đã được sử dụng trong một thao tác, nó không bao giờ có thể đóng góp cho bất kỳ thao tác nào khác và điều tương tự cũng áp dụng độc lập cho các cột. Điều này biến việc xây dựng thành một vấn đề phân chia chứ không phải là một chuỗi các bước sơn độc lập. 

Nhiệm vụ là quyết định xem một lưới đen trắng nhất định có thể được tạo ra bởi một số hoạt động hợp lệ như vậy hay không. 

Các ràng buộc nhỏ, với cả hai chiều lên tới 50, điều này ngay lập tức gợi ý rằng các giải pháp xung quanh phương trình bậc hai hoặc tệ hơn một chút cho mỗi lần kiểm tra là đủ. Không cần phải có tổ hợp trên tất cả các tập hợp con hoặc quay lui các phép toán vì cấu trúc của các phép toán bị ràng buộc rất nhiều. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ cục bộ. Một trực giác ngây thơ có thể nói rằng mỗi ô đen có thể được xử lý độc lập hoặc chúng ta có thể nhóm các hàng hoặc cột một cách tham lam dựa trên vị trí các ô đen xuất hiện. Điều này không thành công khi biểu mẫu phụ thuộc xoay vòng qua các hàng và cột được chia sẻ. 

Ví dụ: hãy xem xét cấu hình trong đó hàng 1 chia sẻ các ô màu đen với cột 1 và cột 2, đồng thời hàng 2 cũng tương tác với các cột đó. Việc nhóm tham lam có thể hợp nhất một số trong số chúng không chính xác, tạo ra thêm các ô đen không tồn tại trong lưới mục tiêu. 

Một trường hợp lỗi khác xuất phát từ việc giả định rằng mỗi thành phần được kết nối màu đen trong lưới là đủ. Ở đây, khả năng kết nối là chưa đủ, bởi vì một hoạt động sẽ tạo ra một sản phẩm đầy đủ của Descartes. Nếu hai hàng và hai cột thuộc cùng một thao tác thì cả bốn giao điểm đều phải có màu đen. Nếu thậm chí một trong số đó có màu trắng, cấu trúc sẽ bị phá vỡ, ngay cả khi mọi thứ được kết nối thông qua các ô đen khác. 

Vì vậy, khó khăn thực sự không phải là khả năng tiếp cận mà là việc thực thi mọi hình chữ nhật ngụ ý đều được lấp đầy. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu sẽ cố gắng đoán trực tiếp các hoạt động. Mỗi thao tác được xác định bởi một tập hợp con các hàng và một tập hợp con các cột và các tập hợp con này phải tách rời nhau trong các thao tác. Việc thử tất cả các phân vùng như vậy vượt xa các giới hạn khả thi, vì ngay cả việc chỉ định các hàng thành các nhóm cũng theo cấp số nhân và các cột phải được phối hợp với chúng. 

Sự đơn giản hóa quan trọng đến từ việc điều chỉnh lại hiệu quả hoạt động. Thay vì suy nghĩ về mặt các phép toán, chúng ta quan sát thấy rằng mọi phép toán đều tạo ra một quy tắc: mỗi hàng trong tập hợp hàng của nó hoạt động giống hệt nhau đối với mọi cột trong tập hợp cột của nó. Đặc biệt, bất kỳ hàng và cột nào xuất hiện cùng nhau trong ít nhất một ô đen đều phải thuộc cùng một chỉ mục thao tác ẩn. 

Điều này gợi ý một ý tưởng thống nhất. Chúng tôi coi mỗi hàng và mỗi cột là các nút trong một cấu trúc duy nhất. Bất cứ khi nào một ô có màu đen, chúng ta buộc hàng và cột của nó thuộc cùng một nhóm. Điều đó có nghĩa là chúng ta có thể hợp nhất chúng bằng cấu trúc hợp nhất. Sau khi xử lý tất cả các ô màu đen, mọi thành phần được kết nối sẽ thể hiện một nhãn hoạt động tiềm năng.

Bây giờ chúng ta phải xác minh rằng cấu trúc này không vô tình tạo ra quá nhiều ô đen. Nếu hai nút nằm trong cùng một thành phần, chúng sẽ được coi là thuộc cùng một hoạt động, nghĩa là mọi cặp hàng-cột bên trong thành phần sẽ trở thành màu đen. Do đó, nếu tồn tại một ô màu trắng có hàng và cột nằm trong cùng một thành phần, thì việc xây dựng sẽ buộc ô đó có màu đen một cách không chính xác. Đó là cách duy nhất để vi phạm tính khả thi. 

Vì vậy, toàn bộ quá trình kiểm tra chỉ dừng lại ở việc xây dựng các thành phần được kết nối từ các ô màu đen và xác thực rằng không có ô màu trắng nào nằm bên trong một cặp thành phần duy nhất. 

Sự chuyển đổi này biến một quy trình tổ hợp phức tạp thành một quá trình kiểm tra tính nhất quán đối với cấu trúc tìm kiếm liên minh lưỡng đảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua hoạt động | Hàm mũ | Hàm mũ | Quá chậm | 
| Kiểm tra tính nhất quán của Union-Find | O(nm α(n+m)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình mỗi hàng và mỗi cột dưới dạng các nút riêng biệt trong cấu trúc tìm liên kết. Các hàng được lập chỉ mục từ 0 đến n−1 và các cột được lập chỉ mục từ n đến n+m−1 để chúng nằm trong cùng một DSU. 

1. Khởi tạo một DSU trên n + m nút, ban đầu coi mỗi hàng và cột là thành phần riêng của nó. Điều này thể hiện rằng chưa có ràng buộc nào được áp dụng. 
2. Quét từng ô trong lưới. Nếu một ô (i, j) có màu đen, hãy hợp nhất hàng i với cột j + n trong DSU. Điều này buộc bất kỳ hoạt động nào tạo ra ô đen này phải bao gồm cả hai điểm cuối, vì vậy chúng phải chia sẻ cùng một nhãn ẩn. 
3. Sau khi tất cả các kết hợp được xử lý, hãy quét lại lưới. Với mỗi ô trắng (i, j), kiểm tra xem hàng i và cột j + n có thuộc cùng một thành phần DSU hay không. Nếu đúng như vậy, không thể trả về ngay lập tức vì điều đó sẽ buộc ô trở thành màu đen trong bất kỳ cấu trúc hợp lệ nào. 
4. Nếu không có ô trắng nào vi phạm điều kiện này, có thể quay lại. 

Lý do điều này hoạt động là vì các ô màu đen xác định các ràng buộc đẳng thức giữa các hàng và cột, và công cụ tìm liên kết tính toán việc đóng bắc cầu của các đẳng thức đó. Bất kỳ sự phân công hoạt động nhất quán nào cũng phải tôn trọng những sự bình đẳng này. Khi các đẳng thức được cố định, mỗi thành phần sẽ hoạt động giống như một nhãn nguyên tử duy nhất. Yêu cầu duy nhất còn lại là chúng ta không bao giờ buộc một ô trắng phải ở bên trong một khối nguyên tử duy nhất, vì điều đó sẽ mâu thuẫn với lưới mục tiêu. 

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
        ra = self.find(a)
        rb = self.find(b)
        if ra == rb:
            return
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]

def solve():
    n, m = map(int, input().split())
    g = [input().strip() for _ in range(n)]

    dsu = DSU(n + m)

    def row(i):
        return i

    def col(j):
        return n + j

    for i in range(n):
        for j in range(m):
            if g[i][j] == '#':
                dsu.union(row(i), col(j))

    for i in range(n):
        for j in range(m):
            if g[i][j] == '.':
                if dsu.find(row(i)) == dsu.find(col(j)):
                    print("No")
                    return

    print("Yes")

if __name__ == "__main__":
    solve()
```DSU được sử dụng để thu gọn các nhận dạng hàng và cột do các ô đen tạo ra. Hàm ánh xạ cho các cột sẽ bù các chỉ số theo n để các hàng và cột nằm trong một cấu trúc mà không bị xung đột. Bước thứ hai rất cần thiết vì nó kiểm tra những mâu thuẫn hơn là xây dựng bất cứ điều gì một cách rõ ràng. 

Một sai lầm phổ biến là chỉ kết hợp các ô màu đen và ngay lập tức cho rằng nó có giá trị. Điều đó bỏ lỡ các trường hợp trong đó các công đoàn ngầm buộc thêm các ô đen không có trong lưới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
#.#
.#.
#.#
```Chúng tôi xử lý các tế bào đen trước tiên. 

| Bước | Hành động | Bang DSU (khái niệm) | 
| --- | --- | --- | 
| (0,1) | hợp r0 và c1 | {r0, c1} | 
| (1,0) | hợp r1 và c0 | {r1, c0} | 
| (1,2) | hợp r1 và c2 | {r1, c0, c2} | 
| (2,1) | hợp r2 và c1 | hợp nhất nhóm hàng với nhóm cột thông qua c1 | 

Sau khi kết hợp, tất cả các nút được kết nối thông qua các liên kết hàng-cột xen kẽ. Bây giờ mỗi cặp hàng-cột đều nằm trong một thành phần duy nhất. 

Bây giờ chúng tôi kiểm tra các tế bào màu trắng. Mỗi ô màu trắng nằm giữa các nút trong cùng một thành phần, vì vậy tất cả chúng sẽ bị buộc phải có màu đen, khớp với mẫu được quan sát, do đó cấu trúc sẽ nhất quán. 

Đầu ra là hợp lệ. 

### Ví dụ 2 

đầu vào:```
2 2
#.
.#
```Xử lý tế bào đen: 

| Bước | Hành động | Bang DSU | 
| --- | --- | --- | 
| (0,0) | hợp r0 và c0 | {r0, c0} | 
| (1,1) | đoàn r1 và c1 | {r1, c1} | 

Bây giờ hãy kiểm tra các ô màu trắng: 

Ô (0,1): r0 nằm trong {r0,c0}, c1 nằm trong {r1,c1}, các thành phần khác nhau, OK. 

Ô (1,0): r1 và c0 nằm trong các thành phần khác nhau, OK. 

Không có mâu thuẫn nào xuất hiện nên lưới là hợp lệ. 

Ví dụ này cho thấy các thành phần độc lập tương ứng với các hoạt động độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm α(n+m)) | Mỗi ô đen kích hoạt một liên kết, mỗi ô trắng kích hoạt find | 
| Không gian | O(n+m) | Mảng DSU cho hàng và cột | 

Các giới hạn n, m 50 tạo thành nm 2500, do đó, ngay cả các thao tác DSU đơn giản cũng dễ dàng đủ nhanh trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# NOTE: placeholder; integrate solve() when running locally
```Vì chúng tôi không thể đưa toàn bộ trình chạy vào nội tuyến mà không xác định lại môi trường giải pháp, dưới đây là các xác nhận khái niệm giả định`solve()`được nối dây chính xác.```
# provided sample
# assert run("5 8\n.#.#..#.\n.....#..\n.#.#..#.\n#.#....#\n.....#..\n") == "Yes"

# minimal all white
# assert run("1 1\n.\n") == "Yes"

# single black
# assert run("1 1\n#\n") == "Yes"

# conflicting white inside forced component
# assert run("2 2\n##\n#.\n") == "No"

# checkerboard consistent
# assert run("2 2\n#.\n.#\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 '.' | Có | xây dựng trống tầm thường | 
| 1x1 '#' | Có | hoạt động đơn lẻ | 
| 2x2 toàn màu đen ngoại trừ một màu trắng | Không | phát hiện mâu thuẫn | 
| Bàn cờ 2x2 | Có | thành phần độc lập | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các ô màu đen buộc tất cả các hàng và cột thành một thành phần duy nhất, nhưng vẫn còn một ô màu trắng. Trong tình huống đó, DSU thu gọn mọi thứ và kiểm tra màu trắng ngay lập tức phát hiện rằng cả hai điểm cuối đều nằm trong cùng một thành phần, từ chối lưới một cách chính xác. 

Một trường hợp khác là khi không có ô đen nào cả. DSU vẫn hoàn toàn tách biệt và không có công đoàn nào diễn ra. Vì không có ô trắng nào có thể nằm trong một cặp thành phần giống nhau bắt buộc nên câu trả lời gần như hợp lệ, tương ứng với các phép toán bằng 0. 

Trường hợp thứ ba là khi các ô màu đen tạo thành nhiều vùng bị ngắt kết nối vô tình kết nối thông qua các mối quan hệ hàng-cột xen kẽ. DSU xử lý việc này một cách tự nhiên, bởi vì khả năng kết nối thông qua bất kỳ đường dẫn nào sẽ hợp nhất các thành phần, đảm bảo rằng các ràng buộc chuyển tiếp ẩn được tôn trọng ngay cả khi không rõ ràng trong lưới.
