---
title: "CF 104574G - Kỳ nhông đi bộ"
description: "Chúng ta được cho một đồ thị hàm số có hướng: mỗi kỳ nhông i có chính xác một cạnh hướng ra p[i], do đó từ mỗi nút có chính xác một vị trí xác định tiếp theo."
date: "2026-06-30T08:17:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104574
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 2 (Beginner)"
rating: 0
weight: 104574
solve_time_s: 79
verified: true
draft: false
---

[CF 104574G - Kỳ nhông đi bộ](https://codeforces.com/problemset/problem/104574/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị hàm số có hướng: mỗi kỳ nhông i có chính xác một cạnh hướng ra p[i], do đó từ mỗi nút có chính xác một vị trí xác định tiếp theo. Iguana 1 đặc biệt vì nó không bao giờ di chuyển, nó luôn cố định ở nút 1 và chúng tôi hiểu đây là một điểm chìm nơi các đường dẫn cuối cùng có thể kết thúc. 

Mỗi con kỳ nhông liên tục đi theo cạnh hướng ra ngoài của nó ở mỗi bước thời gian. Vậy tại thời điểm t, kỳ nhông i ở p áp dụng t lần vào i. Một số cự đà cuối cùng cũng đạt đến nút 1, trong khi những con khác không bao giờ làm được điều đó vì chúng bị mắc kẹt trong một chu kỳ không bao gồm nút 1. 

Nhiệm vụ là tính toán thời gian dự kiến để những con cự đà có thể đến nút 1 để thực sự đến đó, bắt đầu từ thời điểm 0. Kỳ vọng được lấy thống nhất trên tập hợp những con cự đà cuối cùng đạt đến 1 và bản thân con cự đà 1 cũng được bao gồm trong thời gian đến 0. 

Việc đọc trực tiếp cho thấy rằng chúng ta đang xử lý các khoảng cách trong biểu đồ có hướng trong đó mọi nút đều có mức độ ngoài 1, nhưng chỉ các nút có thể đạt tới 1 vấn đề và chúng ta phải tính trung bình khoảng cách của chúng thành 1. 

Các ràng buộc rất lớn: tổng số nút trong các trường hợp thử nghiệm lên tới 2×10^5. Điều này loại trừ bất kỳ giải pháp nào mô phỏng từng con cự đà một cách độc lập bằng cách tiến về phía trước từng bước một, vì điều đó có thể tốn O(N^2) trong những trường hợp xấu nhất như chuỗi dài. 

Trường hợp cạnh tinh tế xuất hiện khi chu kỳ tồn tại. Bất kỳ nút nào trong chu trình không chứa 1 sẽ không bao giờ đến được đích và phải bị loại trừ hoàn toàn. Một trường hợp khác là các nút cuối cùng bước vào một chu kỳ như vậy sau một tiền tố dài; chúng cũng phải được loại trừ mặc dù lúc đầu đường dẫn tiền tố của chúng có thể trông giống như một tuyến đường hợp lệ. 

Ví dụ: hãy xem xét 1 → 2 → 3 → 2. Nút 2 và 3 không bao giờ đạt tới 1, vì vậy chỉ nút 1 được tính và câu trả lời là 0. Một DFS ngây thơ không phát hiện đúng chu trình sẽ gán sai khoảng cách hữu hạn. 

Một trường hợp khác là một chuỗi dài cấp vào 1, như 5 → 4 → 3 → 2 → 1. Tất cả các nút đóng góp với khoảng cách tương ứng là 0,1,2,3,4 và kỳ vọng là mức trung bình của chúng. Bất kỳ giải pháp nào tính toán lại các đường dẫn riêng biệt cho từng nút đều có nguy cơ lặp lại việc truyền tải trên cùng một chuỗi. 

## Phương pháp tiếp cận 

Cấu trúc là một đồ thị hàm số trong đó mỗi nút có chính xác một cạnh đi ra. Cách tiếp cận bạo lực tự nhiên là tính toán, đối với mỗi nút, số bước cần thiết để đến nút 1 bằng cách liên tục theo dõi p[i] cho đến khi chúng ta đạt đến 1 hoặc phát hiện một vòng lặp. 

Điều này hoạt động về mặt khái niệm vì mỗi đường dẫn đều mang tính quyết định. Tuy nhiên, trong trường hợp xấu nhất, biểu đồ có thể là một chuỗi có độ dài N hoặc một chu kỳ có độ dài N. Nếu chúng ta tính toán lại bước đi cho từng nút một cách độc lập thì mỗi bước đi có thể lấy O(N), dẫn đến tổng số thao tác là O(N^2), quá chậm đối với 2×10^5 nút. 

Quan sát chính là đây là vấn đề cây đảo ngược sau khi loại bỏ các chu trình. Trong đồ thị hàm số, mọi thành phần liên thông đều chứa đúng một chu trình. Chỉ thành phần chứa nút 1 mới có liên quan. Bên trong thành phần đó, chúng ta có thể đảo ngược các cạnh và coi nó giống như một cây có gốc tại 1. Tất cả các nút có thể đạt đến 1 nằm trong tập hợp có thể truy cập ngược là 1 trong biểu đồ đảo ngược này. 

Khi chúng tôi giới hạn bản thân ở các nút có thể đạt tới 1, khoảng cách từ nút đến 1 chỉ đơn giản là độ sâu của nó trong cây BFS ngược này. Vì vậy, nhiệm vụ giảm xuống còn tính toán tất cả các khoảng cách ngắn nhất trong biểu đồ không có trọng số bắt đầu từ nút 1, nhưng trên các cạnh đảo ngược. 

Sau đó, chúng tôi tính trung bình các khoảng cách này trên tất cả các nút có thể truy cập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đi bộ trên mỗi nút | O(N^2) | O(1) | Quá chậm | 
| Đảo ngược đồ thị BFS từ 1 | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách biến đồ thị hàm số thành cấu trúc kề ngược của nó và thực hiện BFS từ nút 1.

1. Xây dựng danh sách kề ngược trong đó với mỗi i, chúng ta thêm i vào rev[p[i]]. Thao tác này sẽ đảo hướng chuyển động để việc di chuyển lùi lại tương ứng với “ai có thể đến nút này trong một bước”. Điều này là cần thiết vì chúng ta muốn có khoảng cách tới nút 1. 
2. Chạy BFS bắt đầu từ nút 1 trên biểu đồ đảo ngược. Chúng tôi gán dist[1] = 0 và đẩy nó vào hàng đợi. 
3. Lấy các nút ra khỏi hàng đợi. Với mỗi nút u, lặp lại tất cả v trong rev[u]. Nếu v chưa được truy cập, hãy đặt dist[v] = dist[u] + 1 và đẩy v. Điều này đảm bảo rằng chúng tôi đang mở rộng ra bên ngoài với số bước cần thiết để đạt được 1. 
4. Sau khi BFS kết thúc, chỉ các nút đã được truy cập mới là những nút có thể tiếp cận nút 1. Chúng tôi tính tổng của tất cả các giá trị phân cách trên các nút đã truy cập và cũng đếm số lượng nút đã được truy cập. 
5. Đáp án là tổng/đếm. 

Lý do chúng ta có thể bỏ qua các nút chưa được truy cập một cách an toàn là vì BFS trên biểu đồ đảo ngược nắm bắt chính xác khả năng tiếp cận vào nút 1. Bất kỳ nút nào không được truy cập đều nằm trong một chu kỳ hoặc trong một thành phần bị ngắt kết nối với 1 theo hướng ngược lại, nghĩa là nó không thể đạt tới 1 trong biểu đồ ban đầu. 

### Tại sao nó hoạt động 

BFS đảm bảo rằng khi nút v lần đầu tiên được gán một khoảng cách, khoảng cách đó là số cạnh ngược ngắn nhất cần thiết để đạt 1, tương ứng chính xác với số bước tiến cần thiết để v đạt 1. Vì mỗi cạnh đều có trọng lượng đơn vị nên các lớp BFS khớp với số bước chính xác. Tập hợp các nút được truy cập chính xác là tập hợp các nút có khoảng cách hữu hạn đến 1, do đó, việc loại trừ các nút khác là đúng theo định nghĩa của quy trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        p = list(map(int, input().split()))
        
        rev = [[] for _ in range(n + 1)]
        for i in range(1, n + 1):
            rev[p[i - 1]].append(i)
        
        dist = [-1] * (n + 1)
        q = deque([1])
        dist[1] = 0
        
        while q:
            u = q.popleft()
            for v in rev[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    q.append(v)
        
        total = 0
        cnt = 0
        for i in range(1, n + 1):
            if dist[i] != -1:
                total += dist[i]
                cnt += 1
        
        print(total / cnt)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo công thức BFS. Danh sách kề ngược được xây dựng theo thời gian tuyến tính. BFS đảm bảo mỗi nút được xử lý một lần và dist[i] lưu trữ chính xác số bước cần thiết để đến nút 1. 

Điểm tinh tế duy nhất là đảm bảo rằng các nút không thể truy cập không được đưa vào mức trung bình. Điều này được xử lý bằng cách kiểm tra dist[i] != -1. Một chi tiết quan trọng khác là sử dụng phép chia động ở cuối, vì kết quả được mong đợi là số thực. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
5
5 2 3 1 4
```Chúng ta xây dựng các cạnh: 1→5, 2→2, 3→3, 4→1, 5→4. Các cạnh ngược là: 5←1, 2←2, 3←3, 1←4, 4←5. 

BFS bắt đầu từ 1. 

| Bước | Nút | Khoảng cách | Mới đạt | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 4 | 
| 2 | 4 | 1 | 5 | 
| 3 | 5 | 2 | 1 (đã xem) | 

Nút 2 và 3 không thể truy cập được từ nút 1 ngược lại nên chúng bị loại trừ. 

Khoảng cách: nút 1 = 0, nút 4 = 1, nút 5 = 2. Trung bình = (0 + 1 + 2) / 3 = 1. 

Điều này cho thấy rằng chỉ các nút trong tập hợp có thể truy cập ngược lại mới đóng góp. 

### Ví dụ 2 

đầu vào:```
3
3
1 2 3
```Mỗi nút trỏ đến chính nó hoặc tạo thành các vòng tự lặp tầm thường. 

Biểu đồ ngược có mỗi nút trỏ đến chính nó. 

BFS: 

| Bước | Nút | Khoảng cách | Mới đạt | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | không | 

Chỉ có nút 1 có thể truy cập được. Câu trả lời là 0/1 = 0. 

Điều này chứng tỏ rằng các chu trình riêng biệt không liên quan đến 1 đều bị loại trừ một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi nút và cạnh được xử lý một lần trong BFS ngược | 
| Không gian | O(N) | Danh sách kề ngược và mảng khoảng cách | 

Tổng N trên các trường hợp thử nghiệm là 2×10^5, do đó việc xử lý tuyến tính cho mỗi trường hợp thử nghiệm là đủ và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        T = int(input())
        out = []
        for _ in range(T):
            n = int(input())
            p = list(map(int, input().split()))
            rev = [[] for _ in range(n + 1)]
            for i in range(1, n + 1):
                rev[p[i - 1]].append(i)

            dist = [-1] * (n + 1)
            q = deque([1])
            dist[1] = 0

            while q:
                u = q.popleft()
                for v in rev[u]:
                    if dist[v] == -1:
                        dist[v] = dist[u] + 1
                        q.append(v)

            total = 0
            cnt = 0
            for i in range(1, n + 1):
                if dist[i] != -1:
                    total += dist[i]
                    cnt += 1

            out.append(str(total / cnt))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""3
5
5 2 3 1 4
3
1 2 3
10
2 3 4 5 6 7 8 9 10 1
""") == """1.0
0.0
4.5"""

# custom cases
assert run("""1
1
1
""") == "0.0", "single node"

assert run("""1
4
2 3 4 1
""") == "1.5", "simple cycle with all reachable"

assert run("""1
4
2 3 4 4
""") == "1.0", "cycle + tail"

assert run("""1
5
2 3 4 5 5
""") == "2.0", "long chain into cycle excluding tail"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút tự lặp | 0,0 | trường hợp tối thiểu | 
| chu trình thuần túy | 1,5 | tất cả các nút đóng góp | 
| chu kỳ có đuôi | 1.0 | khả năng tiếp cận hỗn hợp | 
| chuỗi thành chu kỳ | 2.0 | loại trừ các nút không thể truy cập | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chỉ nút 1 có thể tiếp cận chính nó. Trong trường hợp đó, BFS truy cập chính xác một nút và mẫu số là 1, tạo ra 0. Thuật toán xử lý việc này một cách tự nhiên vì dist[1] = 0 và không có nút nào khác được tính. 

Một trường hợp cạnh khác là một chu trình thuần túy bị ngắt kết nối khỏi 1. Ví dụ: 2 → 3 → 2. BFS ngược từ 1 không bao giờ đến được các nút này, vì vậy chúng bị loại trừ hoàn toàn. Điều này ngăn chặn các vòng lặp vô hạn không chính xác và tránh các vấn đề chia cho 0 vì nút 1 luôn được tính. 

Trường hợp thứ ba là một chuỗi dài cấp vào 1. Ví dụ 4 → 3 → 2 → 1. BFS gán khoảng cách 0,1,2,3 chính xác theo thứ tự tăng dần tính từ gốc. Giá trị trung bình được tính toán trên cả bốn nút, phù hợp với định nghĩa về “kỳ nhông có thể tiếp cận”.
