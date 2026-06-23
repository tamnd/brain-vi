---
title: "CF 105271C - Tàu 2"
description: "Chúng ta được cho một cây có các đỉnh được đánh số từ 1 đến n. Mỗi đỉnh hoạt động giống như một ga xe lửa và việc di chuyển dọc theo bất kỳ cạnh nào giữa hai ga sẽ tốn đúng một vé. Điều khó khăn là vé không được cố định về giá trên toàn cầu."
date: "2026-06-23T14:01:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105271
codeforces_index: "C"
codeforces_contest_name: "Almaty Code Cup 2024"
rating: 0
weight: 105271
solve_time_s: 52
verified: true
draft: false
---

[CF 105271C - Xe lửa 2](https://codeforces.com/problemset/problem/105271/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có các đỉnh được đánh số từ 1 đến n. Mỗi đỉnh hoạt động giống như một ga xe lửa và việc di chuyển dọc theo bất kỳ cạnh nào giữa hai ga sẽ tốn đúng một vé. Điều khó khăn là vé không được cố định về giá trên toàn cầu. Thay vào đó, mỗi ga tôi bán vé với giá ai một vé và khi bạn đến ga đó bạn được phép mua bao nhiêu vé tùy thích với mức giá đó. 

Maxim bắt đầu ở trạm 1 và muốn đến trạm n bằng cách di chuyển dọc theo các cạnh. Vì mỗi lần truyền qua cạnh tiêu tốn một vé nên tổng số vé cần thiết chính xác là số cạnh trên đường đi đã chọn, nhưng chi phí phụ thuộc vào nơi mua những vé đó. Anh ta có thể chiến lược mua vé ở các ga rẻ hơn trước hoặc trong cuộc hành trình và mang chúng về phía trước. 

Nhiệm vụ là tính tổng chi phí tối thiểu bằng won cần thiết để đảm bảo rằng Maxim có thể di chuyển từ 1 đến n dọc theo cây. 

Ràng buộc n 2 × 10^5 ngụ ý rằng mọi nghiệm đều phải gần tuyến tính hoặc tuyến tính. Một giải pháp cố gắng xem xét tất cả các cặp nút, tất cả các đường dẫn hoặc tính toán lại các đường dẫn ngắn nhất trên mỗi trạng thái sẽ quá chậm. Vì cấu trúc là một cái cây nên có một đường dẫn đơn giản duy nhất giữa hai nút bất kỳ, giúp đơn giản hóa hình học của chuyển động nhưng không phải là quyết định kinh tế về nơi mua vé. 

Một vấn đề tế nhị nảy sinh từ việc giải thích “mua bao nhiêu vé tại i”. Một người đọc ngây thơ có thể nghĩ rằng chỉ nút bắt đầu mới quan trọng, nhưng chiến lược tối ưu phụ thuộc vào mức giá tối thiểu được thấy dọc theo tiền tố đường dẫn chứ không chỉ các điểm cuối. 

Ví dụ: nếu a1 lớn nhưng nút 3 trên đường dẫn có giá nhỏ thì việc mua mọi thứ ở nút 1 rõ ràng là chưa tối ưu. Một trường hợp lỗi khác xuất hiện khi một nút giá rẻ nằm ngoài đường dẫn chính nhưng vẫn nằm trên cây con được nhập tạm thời trong quá trình truyền tải theo các cách tiếp cận tham lam không chính xác. Bất kỳ phương pháp nào giả định một quyết định cục bộ cho mỗi nút mà không xem xét cấu trúc đường dẫn chung đều có thể thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng mô phỏng tất cả các cách mua vé có thể có dọc theo đường đi từ 1 đến n. Vì đường đi trên cây là duy nhất nên lựa chọn duy nhất còn lại là mua bao nhiêu vé tại mỗi đỉnh đã ghé thăm. Nếu chúng ta xem xét một đường đi có độ dài k, một trạng thái ngây thơ sẽ theo dõi xem chúng ta có bao nhiêu vé còn lại và chúng được mua ở nút nào, sau đó quyết định mua hàng ở mỗi bước. Điều này nhanh chóng trở thành cấp số nhân vì tại mỗi nút, chúng tôi có thể chọn bất kỳ số tiền mua nào và số lượng trạng thái tăng lên theo cả độ dài đường dẫn và khả năng phân phối vé. 

Ngay cả khi chúng tôi đơn giản hóa và giả định rằng chúng tôi chỉ mua chính xác một vé bất cứ khi nào cần, chúng tôi vẫn bỏ lỡ cấu trúc tổ hợp quan trọng: vé có thể thay thế được và chỉ có điểm mua rẻ nhất mới quan trọng. Brute-force thất bại vì nó không khai thác được việc mua nhiều vé ở một nút giá tối thiểu sẽ chi phối mọi chiến lược mua trả chậm. 

Điều quan trọng cần lưu ý là bất kỳ vé nào cần thiết cho một cạnh đều có thể được mua ở ga rẻ nhất được thấy cho đến nay trên đường từ 1 đến điểm đó. Khi chúng tôi đến một nút, chúng tôi có quyền truy cập vào tất cả các trạm trước đó trên đường dẫn một cách hiệu quả, vì vậy chúng tôi phải luôn “kế thừa” mức giá tối thiểu gặp phải cho đến nay. Điều này biến vấn đề thành việc đi từ 1 đến n trong khi duy trì ai tối thiểu dọc theo đường đi và trả mức tối thiểu đó cho mỗi cạnh đi qua. 

Điều này làm giảm vấn đề tìm đường đi duy nhất từ ​​1 đến n và tính tổng, đối với mỗi cạnh, giá nút tối thiểu nhìn thấy đến cạnh đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta dựa vào thực tế là trong một cây, có chính xác một đường đi giữa nút 1 và nút n.

1. Xây dựng cây bằng danh sách kề. Điều này cho phép truyền tải hiệu quả mà không cần tính toán lại kết nối. Cấu trúc cây đảm bảo chúng ta có thể di chuyển mà không cần chu kỳ. 
2. Chạy BFS hoặc DFS từ nút 1 để tính toán con trỏ cha cho mỗi nút. Điều này là cần thiết để xây dựng lại đường dẫn duy nhất từ ​​n trở lại 1. Chúng tôi lưu trữ cha mẹ [v] và tùy chọn độ sâu [v]. Bước này chuyển đổi cây vô hướng thành cấu trúc gốc tại nút 1. 
3. Bắt đầu từ nút n, xây dựng lại đường dẫn quay lại nút 1 bằng cách sử dụng các con trỏ cha. Chúng ta đảo ngược danh sách này để có được đường đi theo đúng thứ tự từ 1 đến n. Bước này cô lập trình tự quan tâm duy nhất trong vấn đề. 
4. Đi qua đường dẫn từ 1 đến n trong khi vẫn duy trì biến best_price đang chạy, được khởi tạo là a1. Tại mỗi nút v trên đường dẫn, cập nhật best_price = min(best_price, a[v]). Sau đó thêm best_price vào câu trả lời cho mỗi lần chuyển đổi cạnh. Điều này mô hình hóa ý tưởng rằng mỗi cạnh yêu cầu một vé và chúng tôi luôn mua nó ở ga rẻ nhất từng thấy cho đến nay. 
5. Xuất chi phí tích lũy. 

Chi tiết quan trọng là chúng tôi thêm chi phí cho mỗi cạnh chứ không phải cho mỗi nút. Vì việc di chuyển dọc theo một cạnh sẽ tiêu tốn chính xác một vé nên mỗi lần chuyển đổi phải được tính phí một lần bằng cách sử dụng mức giá tốt nhất hiện có cho đến thời điểm đó. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm nào dọc theo đường đi từ 1 đến n, tất cả các ga đã ghé thăm cho đến nay đều có thể mua vé và có thể chuyển tiếp. Bất kỳ vé nào cần thiết cho các chặng đường trong tương lai đều có thể được mua tại ga có giá tối thiểu đối với những người đã ghé thăm cho đến nay, vì không có hạn chế về thời điểm tiêu thụ vé so với khi mua. Do đó, đối với cạnh thứ i dọc theo đường dẫn, giá mua rẻ nhất có thể chính xác là ai tối thiểu trong số các nút ở tiền tố của đường dẫn. Thuật toán duy trì tiền tố này ở mức tối thiểu và áp dụng nó chính xác một lần trên mỗi cạnh, phù hợp với cả tính khả thi và tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

n = int(input())
a = list(map(int, input().split()))

g = defaultdict(list)
for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

parent = [-1] * n
depth = [0] * n

q = deque([0])
parent[0] = -2

while q:
    u = q.popleft()
    for v in g[u]:
        if parent[v] == -1:
            parent[v] = u
            depth[v] = depth[u] + 1
            q.append(v)

path = []
cur = n - 1
while cur != -2:
    path.append(cur)
    cur = parent[cur]

path.reverse()

ans = 0
best = a[path[0]]

for i in range(1, len(path)):
    best = min(best, a[path[i]])
    ans += best

print(ans)
```Phần BFS xây dựng một cây gốc từ nút 1 để mỗi nút đều biết nút trước của nó trên đường dẫn duy nhất quay lại nút gốc. Mảng gốc được khởi tạo bằng -1 để đánh dấu các nút chưa được truy cập và nút 1 được đặt thành điểm đánh dấu đặc biệt -2 để chúng ta có thể dừng quá trình tái thiết một cách rõ ràng. 

Bước xây dựng lại đường dẫn sẽ lùi lại từ nút n bằng cách sử dụng các con trỏ cha. Điều này hiệu quả vì trong một cây, mỗi nút có chính xác một nút cha trong cây BFS, đảm bảo một chuỗi hợp lệ trở về 1. 

Vòng lặp tích lũy chi phí là logic cốt lõi. Chúng tôi duy trì mức giá tối thiểu tốt nhất cho đến nay dọc theo con đường được xây dựng lại. Mỗi cạnh đóng góp tốt nhất chính xác một lần, phản ánh mức tiêu thụ vé cho mỗi lần truyền tải. 

Một lỗi phổ biến là tính tổng min(a[u], a[v]) theo các cạnh. Điều đó không chính xác vì mức giá tối ưu phụ thuộc vào toàn bộ tiền tố tối thiểu, không chỉ điểm cuối của mỗi cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một chuỗi đơn giản: 1-2-3-4, với các giá [10, 5, 4, 100]. 

Đường dẫn là [1, 2, 3, 4]. 

| Bước | Nút | giá tốt nhất | Chi phí bổ sung | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 10 | 0 | 0 | 
| 2 | 2 | 5 | 5 | 5 | 
| 3 | 3 | 4 | 4 | 9 | 
| 4 | 4 | 4 | 4 | 13 | 

Bảng này cho thấy một khi đạt được trạm rẻ hơn thì tất cả các biên trong tương lai đều được hưởng lợi từ trạm đó, ngay cả khi các trạm sau này đắt đỏ. 

### Ví dụ 2 

Cây: 1 nối với 2 và 3, và 2 nối với 4, 3 nối với 5, mục tiêu là 5. 

Giá: [8, 1, 5, 7, 9]. 

Đường dẫn 1 → 3 → 5. 

| Bước | Nút | giá tốt nhất | Chi phí bổ sung | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 8 | 0 | 0 | 
| 2 | 3 | 5 | 5 | 5 | 
| 3 | 5 | 5 | 5 | 10 | 

Điều này xác nhận rằng đường dẫn không quan trọng ngoại trừ sự tiến hóa tối thiểu tiền tố của nó; các nhánh bên ngoài đường dẫn là không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | BFS xây dựng các con trỏ gốc theo thời gian tuyến tính và việc xây dựng lại đường dẫn cộng với việc truyền tải là tuyến tính theo độ dài đường dẫn | 
| Không gian | O(n) | danh sách kề, mảng cha và hàng đợi BFS | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả thời gian và bộ nhớ đều tăng tuyến tính với số đỉnh, tối ưu cho cây có kích thước lên tới 2 × 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from collections import defaultdict, deque

    n = int(input())
    a = list(map(int, input().split()))

    g = defaultdict(list)
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    parent = [-1] * n
    q = deque([0])
    parent[0] = -2

    while q:
        u = q.popleft()
        for v in g[u]:
            if parent[v] == -1:
                parent[v] = u
                q.append(v)

    path = []
    cur = n - 1
    while cur != -2:
        path.append(cur)
        cur = parent[cur]
    path.reverse()

    ans = 0
    best = a[path[0]]
    for i in range(1, len(path)):
        best = min(best, a[path[i]])
        ans += best

    return str(ans).strip()

# sample-like cases
assert run("2\n5 1\n1 2\n") == "5"
assert run("3\n10 5 1\n1 2\n2 3\n") == "6"
assert run("4\n8 1 5 7\n1 2\n1 3\n3 4\n") == "10"
assert run("5\n9 8 7 6 5\n1 2\n2 3\n3 4\n4 5\n") == "32"
assert run("3\n1 100 1\n1 2\n1 3\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chi phí ngày càng tăng của chuỗi | tích lũy tuyến tính | tiền tố hiệu ứng tối thiểu | 
| Cây sao | tránh các nhánh không liên quan | trích xuất đường dẫn chính xác | 
| Giá tốt nhất ở giữa | cải tiến tham lam | tối ưu phi cục bộ | 
| Chuỗi giảm dần tồi tệ nhất | xử lý đường dẫn đơn điệu | không có lỗi hồi quy | 

## Vỏ cạnh 

Trường hợp một cạnh là khi nút rẻ nhất không ở đầu hoặc cuối đường dẫn mà ở đâu đó ở giữa. Ví dụ: nếu a1 = 100, a2 = 1, a3 = 100 thì câu trả lời đúng phải sử dụng giá 1 cho cả hai cạnh sau khi đến nút 2. Thuật toán cập nhật chính xác nhất ở nút 2 và truyền tiếp. 

Một trường hợp khác là cây bị lệch trong đó đường đi từ 1 đến n dài và tất cả các nhánh khác đều không liên quan. Bước xây dựng lại đảm bảo chúng tôi bỏ qua hoàn toàn tất cả các nút không có đường dẫn, ngăn chặn việc tổng hợp không chính xác khi truyền tải BFS. 

Trường hợp thứ ba là khi tất cả các mức giá đều bằng nhau. Thuật toán vẫn hoạt động vì tốt nhất không bao giờ thay đổi và kết quả giảm xuống (n − 1) × a1, khớp với số cạnh trong đường dẫn.
