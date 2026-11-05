---
title: "CF 1209D - Bò và Đồ ăn nhẹ"
description: "Chúng tôi được cung cấp một bộ sưu tập các loại đồ ăn nhẹ và một nhóm khách. Mỗi loại đồ ăn nhẹ xuất hiện đúng một lần, do đó có $n$ món riêng biệt được gắn nhãn từ $1$ đến $n$. Mỗi khách có hai loại đồ ăn nhẹ ưa thích."
date: "2026-06-15T18:05:58+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1209
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 584 - Dasha Code Championship - Elimination Round (rated, open for everyone, Div. 1 + Div. 2)"
rating: 1700
weight: 1209
solve_time_s: 169
verified: true
draft: false
---

[CF 1209D - Bò và đồ ăn nhẹ](https://codeforces.com/problemset/problem/1209/D) 

**Đánh giá:** 1700 
**Tags:** dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các loại đồ ăn nhẹ và một nhóm khách. Mỗi loại đồ ăn nhẹ xuất hiện đúng một lần, vì vậy có$n$các mục riêng biệt được dán nhãn$1$ĐẾN$n$. Mỗi khách có hai loại đồ ăn nhẹ ưa thích. Khách đến theo thứ tự đã chọn và khi khách đến, họ sẽ ăn hết đồ ăn nhẹ còn lại thuộc hai loại họ yêu thích. Nếu cả hai món ăn nhẹ ưa thích của họ đã được khách trước đó ăn trước đó, thì vị khách đó sẽ không vui. 

Nhiệm vụ là sắp xếp thứ tự ghé thăm của khách sao cho số lượng khách không hài lòng càng ít càng tốt. 

Một cách hữu ích để diễn giải lại quá trình này là coi mỗi loại đồ ăn nhẹ như một nút. Mỗi vị khách là một cạnh vô hướng kết nối hai loại đồ ăn nhẹ mà họ quan tâm. Khi khách đến, họ “xóa” cả hai điểm cuối, loại bỏ các nút đó khỏi hệ thống. Bất kỳ khách nào sau này chạm vào nút đã xóa vẫn có thể sử dụng điểm cuối khác nếu nó vẫn tồn tại, nhưng nếu cả hai điểm cuối đều đã bị xóa thì khách đó sẽ đóng góp vào câu trả lời. 

Kích thước đầu vào đạt$10^5$nút và$10^5$các cạnh. Điều này ngay lập tức loại trừ bất kỳ lý do dựa trên hoán vị hoặc giai thừa nào đối với việc đặt hàng của khách. Ngay cả các thuật toán đồ thị bậc hai trên tất cả các cặp cũng quá chậm. Chúng ta cần một cái gì đó về cơ bản là tuyến tính hoặc gần tuyến tính, thông thường$O(n + k)$hoặc$O(k \log n)$. 

Một trường hợp phức tạp xuất hiện khi nhiều khách dùng chung một loại đồ ăn nhẹ. Ví dụ: nếu hai khách đều thích$(1,2)$, đỉnh thứ hai luôn không hài lòng nếu đỉnh đầu tiên được xử lý trước nó, nhưng thứ tự vẫn có thể ảnh hưởng đến việc các đỉnh này biến mất sớm hay muộn so với các cạnh khác. Một trường hợp đặc biệt khác là khi biểu đồ đã là một cái cây hoặc một khu rừng, trong đó cấu trúc đơn giản hơn so với khi nó chứa các chu trình, nơi các quyết định tương tác với nhau. 

Khó khăn chính là việc loại bỏ một khách có thể làm mất đi tính hữu dụng trong tương lai của những khách khác, vì vậy việc đặt hàng địa phương tham lam rõ ràng là không an toàn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của khách, mô phỏng quá trình ăn uống và đếm những vị khách không hài lòng. Điều này đúng nhưng hoàn toàn không khả thi vì nó đòi hỏi$k!$mô phỏng và bản thân mỗi mô phỏng đều có chi phí$O(n + k)$, dẫn đến thời gian chạy theo cấp số nhân. 

Nhận xét quan trọng là khách chỉ quan trọng thông qua cấu trúc kết nối của biểu đồ được hình thành bởi các loại đồ ăn nhẹ. Khi một vị khách ăn cả hai điểm cuối, họ sẽ “loại bỏ” một cách hiệu quả một cạnh có thể chịu trách nhiệm giữ cho một số đỉnh tồn tại lâu hơn. Câu hỏi trọng tâm trở thành: khách nào có thể được xử lý mà không cần cách ly ngay cả hai điểm cuối của họ quá sớm? 

Chúng tôi lật quan điểm. Thay vì nghĩ đến việc đặt hàng cho khách, chúng tôi nghĩ đến thời điểm khách trở thành kết nối đầu tiên liên kết hai thành phần vẫn đang hoạt động. Nếu chúng tôi xử lý khách theo cách mà chúng tôi không bao giờ mất khả năng “trì hoãn việc phá hủy” một thành phần cho đến khi điều đó là không thể tránh khỏi, thì về cơ bản chúng tôi đang duy trì các thành phần được kết nối một cách linh hoạt. 

Đây chính xác là một quan điểm liên kết rời rạc (DSU) nếu chúng ta xử lý khách theo chiều ngược lại: bắt đầu với tất cả các loại đồ ăn nhẹ hiện có và dần dần thêm khách (các cạnh). Khi chúng ta thêm một cạnh, nếu nó kết nối hai thành phần đã được kết nối sẵn thì nó sẽ tạo thành một cạnh chu kỳ. Những cạnh đó tương ứng với những vị khách không cần thiết phải duy trì kết nối và có thể “hy sinh cuối cùng”. Số cạnh dư thừa như vậy chính xác là số lượng khách có thể không hài lòng theo thứ tự tối ưu. 

Tương tự, chúng ta có thể mô phỏng quá trình xử lý khách và đếm số lần chúng ta cố gắng kết nối hai đỉnh đã được kết nối trong DSU. Mỗi nỗ lực như vậy tương ứng với một cạnh hình thành chu kỳ và mỗi cạnh chu kỳ cho biết một vị khách có thể được đặt vào vị trí mà cả hai món ăn nhẹ đều không có sẵn khi họ đến. 

Do đó, vấn đề giảm xuống còn việc đếm xem có bao nhiêu cạnh dư thừa khi xây dựng một khu rừng bao trùm trên đồ thị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(k! \cdot (n+k))$|$O(n+k)$| Quá chậm | 
| Phát hiện chu trình DSU |$O((n+k)\alpha(n))$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng cấu trúc liên kết rời rạc để duy trì kết nối giữa các loại đồ ăn nhẹ khi xử lý khách. 

1. Khởi tạo DSU với$n$các nút, mỗi loại đồ ăn nhẹ bắt đầu từ thành phần riêng của nó. Điều này thể hiện rằng ban đầu không có mối quan hệ ăn vặt nào được hợp nhất. 
2. Lặp lại từng khách theo thứ tự bất kỳ. Đối với một khách kết nối$x_i$Và$y_i$, cố gắng hợp nhất các thành phần của chúng trong DSU. 
3. Nếu$x_i$Và$y_i$đã ở trong cùng một thành phần DSU, chúng tôi không thực hiện liên kết. Thay vào đó, chúng tôi tăng bộ đếm “khách dư thừa”. Điều này tương ứng với một cạnh đóng một chu trình trong biểu đồ thay vì mở rộng kết nối. 
4. Nếu chúng ở các thành phần khác nhau, chúng ta hợp nhất chúng lại, hợp nhất các thành phần. Điều này bảo tồn cấu trúc rừng. 
5. Sau khi xử lý hết khách, xuất ra số lượng khách dư đã đếm. Giá trị này là số lượng khách buồn tối thiểu. 

Lý do thứ tự này có tác dụng là vì DSU đang xây dựng một khu rừng bao trùm của biểu đồ một cách hiệu quả. Bất kỳ cạnh nào không tăng số lượng thành phần được kết nối đều không cần thiết để duy trì kết nối và tương ứng với một cạnh chu kỳ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, DSU thể hiện sự phân chia các loại đồ ăn nhẹ thành các thành phần được kết nối được hình thành bởi những khách đã chấp nhận trước đó. Khi một khách kết nối hai đỉnh đã có trong cùng một thành phần, khách đó không góp phần mở rộng khả năng tiếp cận; nó chỉ tạo ra một tuyến đường thay thế bên trong một cấu trúc đã được kết nối. Các cạnh như vậy chính xác là những cạnh có thể bị “trì hoãn” cho đến khi cả hai điểm cuối đều cạn kiệt một cách hiệu quả bởi các cạnh đã chọn trước đó. Do đó, mọi khía cạnh hình thành chu kỳ đều tương ứng với một vị khách có thể bị buộc vào một vị trí mà cả món ăn nhẹ ưa thích của họ đều đã hết, khiến họ buồn theo bất kỳ thứ tự tối ưu nào. Ngược lại, mọi cạnh không theo chu kỳ đều cần thiết để kết nối các thành phần và luôn có thể được lên lịch sớm hơn theo cách ngăn ngừa sự buồn phiền. Điều này chứng tỏ rằng việc đếm các lỗi DSU mang lại câu trả lời tối ưu. 

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
        ra = self.find(a)
        rb = self.find(b)
        if ra == rb:
            return False
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        return True

def solve():
    n, k = map(int, input().split())
    dsu = DSU(n)
    bad = 0

    for _ in range(k):
        x, y = map(int, input().split())
        if not dsu.union(x, y):
            bad += 1

    print(bad)

if __name__ == "__main__":
    solve()
```Cấu trúc DSU duy trì các đại diện cho từng loại đồ ăn nhẹ và hợp nhất chúng khi khách kết nối hai thành phần riêng biệt trước đó. Hàm hợp trả về Sai chính xác khi cạnh thừa, đó là khi chúng ta tăng câu trả lời. Việc nén đường dẫn đảm bảo thời gian gần như không đổi cho mỗi thao tác, giúp giải pháp luôn hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
1 2
4 3
1 4
3 4
```Chúng tôi theo dõi các thành phần DSU và các cạnh dư thừa. 

| Bước | Cạnh | Tìm(1) | Tìm(2) | Cùng một thành phần? | Hành động | Số xấu | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | (1,2) | 1 | 2 | Không | công đoàn | 0 | 
| 2 | (4,3) | 4 | 3 | Không | công đoàn | 0 | 
| 3 | (1,4) | 1 | 4 | Không | công đoàn | 0 | 
| 4 | (3,4) | 1 | 1 | Có | dư thừa | 1 | 

Cạnh cuối cùng đóng một chu kỳ, nghĩa là nó không mở rộng kết nối. Điều đó tương ứng chính xác với việc một vị khách trở nên buồn bã không thể tránh khỏi. 

### Ví dụ 2 

Hãy xem xét:```
4 3
1 2
2 3
3 4
```| Bước | Cạnh | Cùng một thành phần? | Hành động | Số xấu | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | Không | công đoàn | 0 | 
| 2 | (2,3) | Không | công đoàn | 0 | 
| 3 | (3,4) | Không | công đoàn | 0 | 

Không có chu kỳ nào tồn tại nên không có vị khách nào bị buộc phải buồn bã. Câu trả lời là không. 

Những dấu vết này cho thấy chỉ có các cạnh hình thành chu trình mới đóng góp vào câu trả lời cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+k)\alpha(n))$| Mỗi thao tác kết hợp/tìm gần như không đổi do nén đường dẫn và kết hợp theo kích thước | 
| Không gian |$O(n)$| Mảng DSU lưu trữ cấp độ gốc và kích thước cho từng loại đồ ăn nhẹ | 

Các ràng buộc cho phép lên đến$10^5$các nút và cạnh, do đó giải pháp DSU gần tuyến tính phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
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
            ra = self.find(a)
            rb = self.find(b)
            if ra == rb:
                return False
            if self.size[ra] < self.size[rb]:
                ra, rb = rb, ra
            self.parent[rb] = ra
            self.size[ra] += self.size[rb]
            return True

    n, k = map(int, input().split())
    dsu = DSU(n)
    bad = 0
    for _ in range(k):
        x, y = map(int, input().split())
        if not dsu.union(x, y):
            bad += 1
    return str(bad)

# provided sample
assert run("5 4\n1 2\n4 3\n1 4\n3 4\n") == "1"

# single edge
assert run("2 1\n1 2\n") == "0"

# all redundant edges (triangle)
assert run("3 3\n1 2\n2 3\n1 3\n") == "1"

# chain (no cycles)
assert run("5 4\n1 2\n2 3\n3 4\n4 5\n") == "0"

# multiple components with one cycle
assert run("6 5\n1 2\n2 3\n3 1\n4 5\n5 6\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 0 | kết nối tầm thường | 
| tam giác | 1 | phát hiện chu kỳ | 
| chuỗi | 0 | không có cạnh dư thừa | 
| đồ thị hỗn hợp | 1 | độc lập thành phần | 

## Vỏ cạnh 

Một đồ thị hoàn toàn không tuần hoàn, chẳng hạn như một chuỗi đơn giản$1-2-3-4$, không tạo ra các liên kết DSU dư thừa, vì vậy câu trả lời là 0. Thuật toán xử lý từng cạnh như một sự kết hợp thành công và không có sự gia tăng của bộ đếm. 

Một cấu trúc hoàn toàn tuần hoàn giống như một hình tam giác$1-2, 2-3, 1-3$kích hoạt chính xác một phát hiện dư thừa. Hai cạnh đầu tiên hợp nhất các thành phần, trong khi cạnh cuối cùng tìm thấy cả hai điểm cuối đã được kết nối, tăng câu trả lời lên một. 

Các thành phần bị ngắt kết nối hoạt động độc lập. Ví dụ: hai chuỗi riêng biệt không đóng góp gì cho câu trả lời vì không có cạnh nào đóng một chu trình trong mỗi thành phần.
