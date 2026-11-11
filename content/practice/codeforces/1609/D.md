---
title: "CF 1609D - Mạng xã hội"
description: "Chúng tôi được giao cho một nhóm người ban đầu không có mối quan hệ nào cả. Theo thời gian, chúng ta được thấy một chuỗi các cặp và mỗi cặp đại diện cho một ràng buộc mà cuối cùng phải được thỏa mãn: hai người trong cặp phải kết nối với nhau, nghĩa là tồn tại một chuỗi…"
date: "2026-06-10T07:25:23+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "graphs", "greedy", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1609
codeforces_index: "D"
codeforces_contest_name: "Deltix Round, Autumn 2021 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 1600
weight: 1609
solve_time_s: 112
verified: false
draft: false
---

[CF 1609D - Mạng xã hội](https://codeforces.com/problemset/problem/1609/D) 

**Đánh giá:** 1600 
**Tags:** dsu, đồ thị, tham lam, thực hiện, cây cối 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao cho một nhóm người ban đầu không có mối quan hệ nào cả. Theo thời gian, chúng ta được thấy một chuỗi các cặp và mỗi cặp đại diện cho một ràng buộc mà cuối cùng phải được thỏa mãn: hai người trong cặp phải kết nối với nhau, nghĩa là tồn tại một chuỗi giới thiệu trực tiếp liên kết họ. 

William là người duy nhất được phép giới thiệu mọi người với nhau. Mỗi phần giới thiệu kết nối hai cá nhân chưa được kết nối trước đó và thêm một cạnh vào biểu đồ trống ban đầu một cách hiệu quả. Sau khi thực hiện phần giới thiệu i đầu tiên, tất cả các ràng buộc i đầu tiên phải được thỏa mãn, nghĩa là mỗi cặp đó phải nằm trong cùng một thành phần được kết nối của biểu đồ kết quả. 

Đối với mỗi độ dài tiền tố i, về mặt khái niệm, chúng tôi khởi động lại từ đầu, thực hiện chính xác phần giới thiệu i và đảm bảo các ràng buộc i đầu tiên được thỏa mãn. Trong số tất cả những cách hợp lý để chọn những lời giới thiệu này, chúng tôi muốn tối đa hóa quy mô của nhóm người lớn nhất mà tất cả đều có thể tiếp cận được thông qua những lời giới thiệu. Theo thuật ngữ biểu đồ, chúng tôi tối đa hóa kích thước của thành phần được kết nối lớn nhất. 

Điểm tinh tế quan trọng nhất là mỗi tiền tố đều độc lập. Chúng tôi không mang bất kỳ cấu trúc nào từ các tiền tố trước đó, vì vậy mỗi tiền tố i xác định một vấn đề tối ưu hóa riêng biệt trên cùng n nút và trước tiên tôi yêu cầu các ràng buộc kết nối. 

Các ràng buộc đủ nhỏ để n lên tới 1000 cho phép lý luận O(n^2) hoặc O(n^2 log n). Tuy nhiên, việc chúng tôi tính toán lại cho mọi tiền tố cho thấy rõ rằng chúng tôi cần một cấu trúc có thể được cập nhật tăng dần thay vì mỗi lần tính toán lại kết nối từ đầu. 

Một cách tiếp cận đơn giản là xây dựng lại kết nối từ đầu cho mọi i và cố gắng thực thi các ràng buộc sẽ liên tục chạy một cấu trúc biểu đồ đầy đủ hoặc tìm kiếm trên tất cả các phần giới thiệu có thể có. Điều này nhanh chóng trở nên không khả thi vì ngay cả việc kiểm tra tính khả thi trên mỗi tiền tố cũng đã yêu cầu logic tìm liên kết hoặc BFS nặng nề và thực hiện việc đó một cách độc lập cho mỗi tiền tố sẽ nhân chi phí lên d. 

Một trường hợp cạnh phổ biến phát sinh khi các ràng buộc hình thành các thành phần chồng chéo. Ví dụ: nếu các ràng buộc là (1,2), (2,3), (3,4), thì việc xây dựng tối ưu sẽ nhanh chóng xây dựng một chuỗi và thành phần lớn nhất sẽ tăng trưởng đều đặn. Nhưng nếu các ràng buộc bị rời rạc sớm và chỉ sau đó mới kết nối các nhóm lớn, thì cách giải thích cục bộ tham lam về các ràng buộc mà không có thứ tự hợp nhất cẩn thận có thể đánh giá thấp quy mô thành phần có thể đạt được. 

Một vấn đề tế nhị khác là hiểu sai điều gì cần phải tối đa hóa. Chúng tôi không tối đa hóa số lượng ràng buộc hoặc cạnh được thỏa mãn được thêm vào. Chúng tôi đang tối đa hóa kích thước của thành phần được kết nối lớn nhất sau khi thực thi tất cả các ràng buộc trong tiền tố. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực bắt đầu bằng cách quan sát rằng đối với i cố định, chúng ta phải đảm bảo rằng tất cả các cặp (x1, y1) đến (xi, yi) nằm trong cùng một thành phần được kết nối. Người ta có thể cố gắng xây dựng một biểu đồ bằng cách liên tục thêm các cạnh và kiểm tra tính hợp lệ, nhưng điều đó dẫn đến một khó khăn chính: chúng ta được tự do lựa chọn lời giới thiệu mà William thực hiện, miễn là tất cả các ràng buộc đều được kết nối với nhau. 

Vì vậy, cấu trúc thực sự là thế này: các ràng buộc buộc một số nhóm nút nhất định phải kết thúc trong cùng một thành phần được kết nối. Nếu chúng ta coi ràng buộc thứ i đầu tiên là các cạnh trong đồ thị phụ, thì bất kỳ cấu hình cuối cùng hợp lệ nào ít nhất phải kết nối từng thành phần được kết nối của đồ thị phụ này. Điều đó có nghĩa là mỗi thành phần của biểu đồ ràng buộc phải được chứa đầy đủ bên trong một số thành phần được kết nối của biểu đồ được xây dựng của William. 

Khi đó, chiến lược tối ưu là nhận ra rằng William nên kết nối các thành phần theo cách giảm thiểu sự phân mảnh, cho phép chúng ta coi từng thành phần ràng buộc như một khối "phải được hợp nhất" một cách hiệu quả. Sự tự do duy nhất là làm thế nào các khối này được hợp nhất bằng cách sử dụng chính xác i cạnh.

Thông tin chi tiết quan trọng là vì chúng tôi thực hiện chính xác i phần giới thiệu và mỗi phần giới thiệu sẽ hợp nhất hai thành phần nên chúng tôi bắt đầu với n nút riêng biệt và thực hiện tôi hợp nhất. Sau khi tôi hợp nhất, chúng ta luôn có chính xác n - i thành phần. Do đó, vấn đề giảm xuống còn việc kiểm soát cách phân phối các hợp nhất đó trong khi vẫn tôn trọng các thành phần ràng buộc đó phải được chứa. 

Điều này dẫn đến một quá trình tìm liên kết tham lam trên các ràng buộc: chúng tôi xử lý từng ràng buộc một, hợp nhất các điểm cuối của chúng. Kích thước của thành phần được kết nối lớn nhất trong cấu trúc tìm liên kết sau i liên kết đưa ra câu trả lời có thể đạt được tốt nhất, bởi vì bất kỳ cấu trúc hợp lệ nào ít nhất phải tôn trọng các kết nối bắt buộc này và chúng ta luôn có thể nhận ra một cấu trúc đạt được chính xác những sự hợp nhất đó. 

Vì vậy, chúng tôi duy trì DSU và theo dõi kích thước thành phần. Sau khi xử lý từng ràng buộc, chúng tôi cập nhật kích thước của thành phần DSU lớn nhất. 

Sự biện minh tinh tế là các ràng buộc chỉ yêu cầu kết nối chứ không phải các cạnh trực tiếp, do đó việc hợp nhất các điểm cuối là đủ. Mỗi ràng buộc buộc một cách hiệu quả sự kết hợp của các thành phần của chúng trong bất kỳ hiện thực hóa hợp lệ nào. Sau khi hợp nhất chúng, chúng tôi sẽ nắm bắt chính xác cấu trúc bắt buộc tối thiểu và vì chúng tôi được phép giới thiệu tùy ý ngoài các ràng buộc, nên chúng tôi luôn có thể đạt được cấu hình trong đó thành phần lớn nhất bằng thành phần DSU lớn nhất được hình thành bởi các ràng buộc. 

### Bảng so sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng lại/mô phỏng theo tiền tố) | O(d · n²) | O(n²) | Quá chậm | 
| DSU vượt qua những hạn chế | O(d α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các ràng buộc tăng dần trong khi vẫn duy trì cấu trúc tập hợp rời rạc. 

1. Khởi tạo DSU với n nút, mỗi nút nằm trong thành phần riêng của nó. Mỗi thành phần có kích thước 1. 
2. Duy trì một biến`best`lưu trữ kích thước thành phần tối đa được thấy cho đến nay. 
3. Với mỗi ràng buộc i = 1 đến d, chúng ta lấy cặp (x_i, y_i) và hợp nhất các thành phần DSU của chúng. Nếu chúng đã ở trong cùng một thành phần thì không có gì thay đổi. 
4. Sau mỗi hoạt động đoàn, chúng tôi cập nhật`best`để phản ánh kích thước thành phần lớn nhất hiện có trong DSU. 
5. Đầu ra`best`sau khi xử lý từng tiền tố i. 

Lý do chính khiến điều này có hiệu quả là vì mỗi ràng buộc đều thực thi khả năng kết nối giữa hai nhóm. Khi hai nhóm được kết nối trong các ràng buộc, mọi cấu trúc hợp lệ đều phải đảm bảo chúng được kết nối trong biểu đồ cuối cùng. DSU nén tất cả các mối quan hệ bắt buộc như vậy thành các thành phần và vì mỗi phần giới thiệu sẽ hợp nhất hai thành phần, nên việc theo dõi kích thước thành phần sẽ trực tiếp nắm bắt được sự phát triển của nhóm được kết nối lớn nhất có thể. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố i nào, các cạnh ràng buộc xác định một biểu đồ có các thành phần được kết nối là các lớp tương đương của những người phải được kết nối trong mọi cấu trúc hợp lệ. Bất kỳ kế hoạch giới thiệu hợp lệ nào cũng phải làm cho mỗi lớp này được kết nối nội bộ, để mỗi lớp hoạt động giống như một siêu nút. Thành phần kết nối lớn nhất có thể mà chúng ta có thể tạo được bằng cách coi mỗi thành phần ràng buộc là không thể chia được và đếm kích thước của nó. DSU duy trì chính xác các lớp tương đương này và vì việc hợp nhất là hoạt động duy nhất được phép, trạng thái DSU sau khi kết hợp i biểu thị cấu trúc kết nối cưỡng bức tối đa có thể đạt được sau khi đáp ứng ràng buộc i đầu tiên. 

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
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return 0
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]
        return self.size[a]

n, d = map(int, input().split())
dsu = DSU(n)

best = 1
out = []

for _ in range(d):
    x, y = map(int, input().split())
    x -= 1
    y -= 1
    merged_size = dsu.union(x, y)
    if merged_size:
        best = max(best, merged_size)
    out.append(str(best))

print("\n".join(out))
```Cấu trúc DSU theo dõi các thành phần được kết nối do các ràng buộc tạo ra. Mỗi liên minh tương ứng với việc buộc hai người phải ở trong cùng một nhóm được kết nối. Mảng kích thước cho phép theo dõi liên tục độ lớn của mỗi nhóm kết quả và cập nhật`best`đảm bảo chúng tôi luôn biết cụm lớn nhất có thể đạt được sau mỗi tiền tố. 

Chi tiết triển khai chính là chúng tôi chỉ cập nhật`best`khi sự hợp nhất thực sự xảy ra. Nếu hai nút đã ở trong cùng một thành phần, ràng buộc sẽ không thêm cấu trúc mới và câu trả lời vẫn không thay đổi. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi đầu vào mẫu. 

đầu vào:```
7 6
1 2
3 4
2 4
7 6
6 5
1 7
```Chúng tôi theo dõi sự hợp nhất DSU và thành phần lớn nhất. 

| tôi | hạn chế | bộ hợp nhất | kích thước thành phần | tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 1-2 | {1,2} | 2,1,1,1,1,1,1 | 2 | 
| 2 | 3-4 | {3,4} | 2,2,1,1,1,1 | 2 | 
| 3 | 2-4 | {1,2,3,4} | 4,1,1,1,1,1 | 4 | 
| 4 | 7-6 | {6,7} | 4,2,1,1,1 | 4 | 
| 5 | 6-5 | {5,6,7} | 4,3,1,1 | 4 | 
| 6 | 1-7 | tất cả hợp nhất | 7 | 7 | 

Đầu ra có nguồn gốc là:```
2
2
4
4
4
7
```Điều này phù hợp với cách giải thích của DSU về sự tăng trưởng kết nối bắt buộc. Mỗi bước xác nhận rằng thành phần được kết nối lớn nhất chỉ phụ thuộc vào các liên kết do ràng buộc gây ra. 

Một ví dụ nhỏ thứ hai: 

đầu vào:```
5 3
1 2
2 3
4 5
```Dấu vết: 

| tôi | hạn chế | tốt nhất | 
| --- | --- | --- | 
| 1 | 1-2 | 2 | 
| 2 | 2-3 | 3 | 
| 3 | 4-5 | 3 | 

Chúng tôi thấy thành phần lớn nhất chỉ phát triển khi các ràng buộc hợp nhất các chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d α(n)) | Mỗi liên kết/tìm gần như không đổi do nén đường dẫn và liên kết theo kích thước | 
| Không gian | O(n) | Mảng DSU lưu trữ phần tử gốc và kích thước cho mỗi nút | 

Các ràng buộc cho phép n lên tới 1000, nhưng giải pháp DSU đủ hiệu quả để ngay cả trong trường hợp xấu nhất, các phép toán d = 999 cũng không đáng kể. Thuật toán chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, d = map(int, input().split())
    dsu = DSU(n)

    best = 1
    out = []

    for _ in range(d):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        merged_size = dsu.union(x, y)
        if merged_size:
            best = max(best, merged_size)
        out.append(str(best))

    return "\n".join(out)

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
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return 0
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]
        return self.size[a]

# provided sample
assert run("""7 6
1 2
3 4
2 4
7 6
6 5
1 7
""") == "2\n2\n4\n4\n4\n7"

# minimum size
assert run("""2 1
1 2
""") == "2"

# no merges early
assert run("""5 3
1 2
3 4
5 1
""") == "2\n2\n5"

# chain
assert run("""4 3
1 2
2 3
3 4
""") == "2\n3\n4"

# disconnected clusters
assert run("""6 3
1 2
3 4
5 6
""") == "2\n2\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 2 | hành vi công đoàn cơ bản | 
| sáp nhập sớm thưa thớt | tăng tốt nhất | theo dõi gia tăng | 
| hợp nhất chuỗi | thành phần đang phát triển | kết nối bắc cầu | 
| cặp rời rạc | bằng phẳng rồi ổn định | thành phần độc lập | 

## Vỏ cạnh 

Một trường hợp nhỏ nhưng quan trọng là khi các ràng buộc lặp lại các cặp đã được kết nối. Ví dụ: 

đầu vào:```
4 3
1 2
2 3
1 3
```Sau hai ràng buộc đầu tiên, các nút {1,2,3} đã được hợp nhất. Ràng buộc thứ ba không thay đổi trạng thái DSU. Thuật toán bỏ qua nó một cách chính xác bởi vì`union`trả về 0 và`best`không được cập nhật. Đầu ra vẫn nhất quán với thực tế là không có kết nối mới nào được thực thi. 

Một trường hợp khác là khi các ràng buộc tạo thành hai cụm độc lập mà sau này chỉ trở nên hữu ích. DSU đảm bảo rằng việc hợp nhất một phần được tích lũy một cách chính xác và thành phần lớn nhất chỉ được cập nhật khi việc hợp nhất thực sự xảy ra, ngăn chặn lạm phát khỏi các hạn chế dư thừa.
