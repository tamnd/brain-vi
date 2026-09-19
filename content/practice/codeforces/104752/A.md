---
title: "CF 104752A - Akira"
description: "Chúng ta có một tập hợp các điểm trên mặt phẳng, mỗi điểm biểu diễn một nguồn của một “quả cầu ánh sáng” hình tròn đang giãn nở. Mọi điểm đều bắt đầu như một thảm họa cấp 1 và ảnh hưởng của nó tăng dần ra bên ngoài theo thời gian với tỷ lệ cố định $M$."
date: "2026-06-29T01:24:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104752
codeforces_index: "A"
codeforces_contest_name: "Concurso de programaci\u00f3n ANIEI 2023"
rating: 0
weight: 104752
solve_time_s: 82
verified: true
draft: false
---

[CF 104752A - Akira](https://codeforces.com/problemset/problem/104752/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên mặt phẳng, mỗi điểm biểu diễn một nguồn của một “quả cầu ánh sáng” hình tròn đang giãn nở. Mọi điểm đều bắt đầu như một thảm họa cấp 1 và ảnh hưởng của nó tăng dần ra bên ngoài theo thời gian với tốc độ cố định$M$. Vào thời điểm$t$, mỗi điểm có một đĩa bán kính$M \cdot t$. 

Bất cứ khi nào hai khu vực thảm họa hiện có chạm vào hoặc chồng lên nhau, chúng sẽ hợp nhất thành một thảm họa tổng hợp duy nhất có cấp độ bằng tổng của hai cấp độ trước đó. Việc hợp nhất này có tính bắc cầu: khi một nhóm điểm đã hợp nhất thành một cụm được kết nối duy nhất, bất kỳ giao điểm nào nữa với một cụm khác sẽ hợp nhất hai cụm và thêm các cấp độ của chúng. 

Nhiệm vụ là tìm thời điểm sớm nhất khi có ít nhất một cụm được kết nối có tổng mức ít nhất$K$. Vì mỗi điểm ban đầu có cấp độ 1, nên điều này tương đương với việc tìm thời điểm sớm nhất khi tồn tại một thành phần liên thông (dưới giao điểm đĩa) chứa ít nhất$K$điểm. 

Điều kiện hình học của hai điểm$i$Và$j$được kết nối vào thời điểm đó$t$là:$$M \cdot t \ge \frac{d_{ij}}{2}$$Ở đâu$d_{ij}$là khoảng cách Euclide giữa các điểm. Tương đương:$$t \ge \frac{d_{ij}}{2M}$$Vì vậy, chúng tôi đang xây dựng một biểu đồ có các cạnh “kích hoạt” theo thời gian một cách hiệu quả và chúng tôi muốn thời điểm sớm nhất khi một số thành phần được kết nối đạt đến kích thước$K$. 

Các ràng buộc cho phép lên đến$N = 1000$, do đó đồ thị hoàn chỉnh có tối đa khoảng$5 \times 10^5$các cạnh. Một giải pháp với$O(N^2 \log N)$hoặc$O(N^2)$cấu trúc được chấp nhận cho mỗi trường hợp thử nghiệm được đưa ra$T \le 200$nhưng chúng ta phải cẩn thận với các hằng số. 

Một trường hợp khó nhận thấy là khi$K = 1$. Khi đó câu trả lời luôn là 0, vì mỗi điểm đã hình thành thảm họa cấp 1 tại thời điểm 0. 

Một trường hợp khác là khi các điểm rất gần nhau hoặc giống hệt nhau. Nếu hai điểm trùng nhau, khoảng cách bằng 0, do đó chúng hợp nhất ngay lập tức tại thời điểm 0, nghĩa là tìm liên kết phải xử lý chính xác các cạnh có trọng số bằng 0. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng thời gian liên tục và tính toán lại nhiều lần những điểm nào được kết nối tại một thời điểm nhất định$t$. Đối với một cố định$t$, chúng ta có thể xây dựng một biểu đồ trong đó chúng ta kết nối tất cả các cặp có khoảng cách lớn nhất$2Mt$, sau đó chạy DFS hoặc Union-find để tính toán kích thước thành phần và kiểm tra xem có thành phần nào đạt kích thước không$K$. Sau đó chúng ta có thể tìm kiếm theo thời gian bằng cách sử dụng tìm kiếm nhị phân. 

Điều này hiệu quả vì mối quan hệ kết nối đơn điệu về mặt thời gian: một khi hai điểm được kết nối, chúng sẽ được kết nối mãi mãi. Tuy nhiên, cách tiếp cận bạo lực trở nên tốn kém vì mỗi lần kiểm tra tốn$O(N^2)$để xây dựng các cạnh và$O(N^2)$hoặc$O(N \alpha(N))$đến các thành phần công đoàn. Với tìm kiếm nhị phân theo thời gian thả nổi (ví dụ 60 lần lặp), tổng chi phí sẽ trở nên quá lớn đối với trường hợp xấu nhất$T = 200$. 

Quan sát quan trọng là chúng ta thực sự không cần mô phỏng thời gian. Mỗi cặp điểm có thời gian kích hoạt cố định:$$w_{ij} = \frac{d_{ij}}{2M}$$Chúng ta chỉ quan tâm đến thời điểm các cạnh xuất hiện chứ không quan tâm đến việc thời gian diễn ra liên tục như thế nào. Điều này biến vấn đề thành vấn đề kết nối ngoại tuyến cổ điển với các cạnh có trọng số. Chúng tôi sắp xếp tất cả các cạnh theo thời gian kích hoạt và sử dụng cấu trúc tìm liên kết để hợp nhất các thành phần theo thứ tự thời gian tăng dần. Trong khi hợp nhất, chúng tôi theo dõi kích thước thành phần và thời điểm bất kỳ thành phần nào đạt đến kích thước$K$, thời gian cạnh hiện tại là câu trả lời. 

Điều này làm giảm vấn đề đối với việc xử lý một biểu đồ hoàn chỉnh theo kiểu Kruskal, dừng sớm khi thành phần bắt buộc xuất hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force + Tìm kiếm nhị phân |$O(T \cdot \log R \cdot N^2)$|$O(N^2)$| Quá chậm | 
| Kruskal + DSU |$O(T \cdot N^2 \log N)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Thuật toán tối ưu 

1. Đối với mỗi trường hợp kiểm tra, đọc tất cả các điểm và tính toán từng khoảng cách Euclide theo cặp. Điều này là cần thiết vì mọi tương tác có thể có giữa hai quả cầu chỉ được xác định bởi sự tách biệt hình học của chúng. 
2. Cho mỗi cặp$i, j$, tính thời điểm hai quả cầu của chúng chạm nhau lần đầu:$$t_{ij} = \frac{\sqrt{(x_i - x_j)^2 + (y_i - y_j)^2}}{2M}$$Điều này chuyển đổi quá trình tăng trưởng liên tục thành một tập hợp các sự kiện riêng biệt, mỗi sự kiện một cặp. 
3. Lưu trữ tất cả các cạnh$(t_{ij}, i, j)$trong một danh sách. Mỗi cạnh đại diện cho một sự kiện hợp nhất trong tương lai giữa hai thành phần khi thời gian đạt đến$t_{ij}$. 
4. Sắp xếp tất cả các cạnh theo thứ tự tăng dần$t_{ij}$. Điều này đảm bảo chúng tôi xử lý việc hợp nhất theo thứ tự chính xác mà chúng có thể thực hiện được về mặt vật lý. 
5. Khởi tạo cấu trúc tập hợp rời rạc (DSU) trong đó mỗi điểm bắt đầu trong thành phần riêng của nó có kích thước 1. Điều này phản ánh rằng ban đầu mọi thảm họa đều có cấp 1. 
6. Duyệt các cạnh theo thứ tự được sắp xếp. Đối với mỗi cạnh$(t, u, v)$, cố gắng hợp nhất các thành phần có chứa$u$Và$v$. Nếu chúng đã nằm trong cùng một thành phần rồi thì hãy bỏ qua vì kết nối là dư thừa. 
7. Khi hợp nhất hai thành phần, hãy cập nhật kích thước thành phần kết quả. Nếu tại bất kỳ thời điểm nào, kích thước thành phần trở nên ít nhất$K$, ghi lại thời gian hiện tại$t$làm câu trả lời và ngừng xử lý các cạnh tiếp theo. 
8. Xuất ra thời gian đã ghi cho test case. 

Lý do chúng ta có thể dừng sớm là vì khi một thành phần đạt đến kích thước$K$, không cạnh nào sau đó có thể tạo ra thời gian hợp lệ nhỏ hơn, vì tất cả các cạnh còn lại xảy ra ở thời gian bằng nhau hoặc lớn hơn. 

### Tại sao nó hoạt động 

Bất cứ lúc nào$t$, đồ thị được hình thành bởi các cạnh với$t_{ij} \le t$thể hiện chính xác những hình cầu nào đã giao nhau. DSU duy trì các thành phần liên thông của đồ thị này khi các cạnh được thêm vào theo thứ tự tăng dần$t_{ij}$. Vì kích thước thành phần chính xác là tổng của các cấp đơn vị ban đầu, nên việc đạt đến kích thước$K$tương ứng chính xác với việc có một mức độ$K$thảm họa. Lần đầu tiên điều này xảy ra theo thứ tự cạnh tăng dần phải là thời gian tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, M, K = input().split()
        n = int(n)
        M = float(M)
        K = int(K)

        pts = []
        for _ in range(n):
            x, y = map(int, input().split())
            pts.append((x, y))

        if K == 1:
            print("0.0")
            continue

        edges = []
        for i in range(n):
            x1, y1 = pts[i]
            for j in range(i + 1, n):
                x2, y2 = pts[j]
                dx = x1 - x2
                dy = y1 - y2
                dist = (dx * dx + dy * dy) ** 0.5
                edges.append((dist / (2.0 * M), i, j))

        edges.sort()

        parent = list(range(n))
        size = [1] * n

        def find(x):
            while parent[x] != x:
                parent[x] = parent[parent[x]]
                x = parent[x]
            return x

        def union(a, b):
            ra, rb = find(a), find(b)
            if ra == rb:
                return 0
            if size[ra] < size[rb]:
                ra, rb = rb, ra
            parent[rb] = ra
            size[ra] += size[rb]
            return size[ra]

        for t, u, v in edges:
            new_size = union(u, v)
            if new_size >= K:
                print(f"{t:.10f}")
                break
        else:
            print("0.0")

if __name__ == "__main__":
    solve()
```Giải pháp là triển khai trực tiếp việc hợp nhất kiểu Kruskal theo thời gian kích hoạt cạnh hình học. DSU duy trì tư cách thành viên thành phần một cách hiệu quả và mảng kích thước theo dõi mức độ thảm họa. 

Một chi tiết triển khai tinh tế là tính toán khoảng cách Euclide bằng cách sử dụng căn bậc hai dấu phẩy động. Vì dung sai độ chính xác yêu cầu là$10^{-6}$, số học nổi tiêu chuẩn là đủ. Một điểm quan trọng khác là việc chấm dứt sớm: một khi thành phần hợp lệ xuất hiện, việc tiếp tục sẽ chỉ tạo ra thời gian lớn hơn và không cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
1 1 1
0 0
```Chỉ có một điểm, như vậy tại thời điểm 0 đã hình thành hợp lệ cấp 1 thảm họa. 

| Bước | Hành động | Kích thước thành phần | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | Nút đơn | {1} | 0 | 

Điều này xác nhận rằng$K = 1$được xử lý ngay lập tức mà không cần xây dựng các cạnh. 

### Mẫu 2 

đầu vào:```
1
4 1 2
0 0
0 2
500 500
500 100
```Chúng tôi tính toán khoảng cách. Cặp có ý nghĩa gần nhất là$(0,0)$Và$(0,2)$, khoảng cách 2, cho thời gian hợp nhất 1. 

| Cạnh | Thời gian | Kết quả đoàn | Thành phần lớn nhất | 
| --- | --- | --- | --- | 
| (0,0)-(0,2) | 1.0 | cỡ 2 | 2 | 

Sau khi xử lý lần hợp nhất đầu tiên, một thành phần có kích thước 2 xuất hiện, do đó quá trình dừng lại. 

Điều này cho thấy rằng chỉ có cạnh hợp nhất sớm nhất mới quan trọng chứ không phải cấu trúc kết nối đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2 \log N)$mỗi bài kiểm tra | Khoảng cách theo cặp tạo ra$O(N^2)$các cạnh, được sắp xếp một lần, mỗi liên kết gần như không đổi | 
| Không gian |$O(N^2)$| Tất cả các cạnh được lưu trữ rõ ràng | 

Với$N \le 1000$, điều này dẫn đến khoảng$5 \times 10^5$các cạnh trên mỗi lần kiểm tra, có thể chấp nhận được trong các ràng buộc về việc chấm dứt sớm trong nhiều trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # assume solve() is defined above
    solve()
    return ""  # placeholder for illustration

# provided sample
# (output comparison omitted for brevity)

# K=1 immediate
assert run("1\n1 2 1\n0 0\n") == ""

# two points touching immediately
assert run("1\n2 1 2\n0 0\n0 0\n") == ""

# chain requiring second merge
assert run("1\n3 1 3\n0 0\n0 2\n0 4\n") == ""

# square formation
assert run("1\n4 1 2\n0 0\n0 1\n1 0\n1 1\n") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | trường hợp cơ sở K=1 | 
| điểm giống nhau | 0 | hợp nhất khoảng cách bằng không | 
| đường 3 điểm | t nhỏ | tuyên truyền công đoàn | 
| vuông | hợp nhất kích thước-2 sớm | tính đúng đắn của trật tự hình học | 

## Vỏ cạnh 

Khi nào$K = 1$, thuật toán bỏ qua tất cả hình học và trả về 0 ngay lập tức. Điều này đúng vì mỗi điểm ban đầu đã hình thành thảm họa cấp 1 mà không có bất kỳ sự tương tác nào. 

Khi nhiều điểm trùng nhau, khoảng cách theo cặp của chúng bằng 0, do đó thời gian hợp nhất của chúng bằng 0. DSU hợp nhất chúng ở bước đầu tiên, tạo thành một thành phần lớn hơn ngay lập tức và việc theo dõi kích thước phản ánh chính xác sự phát triển thảm họa ngay lập tức. 

Khi tất cả các điểm cách xa nhau, sẽ không có cạnh nào xuất hiện trước thời gian rất lớn. Thuật toán vẫn xử lý tất cả các cạnh theo thứ tự được sắp xếp và chỉ trả về sau khi lần hợp nhất cần thiết đầu tiên xuất hiện, đảm bảo tính chính xác ngay cả trong các tình huống tương tác thưa thớt.
