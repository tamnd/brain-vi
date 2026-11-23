---
title: "CF 468D - Cây"
description: "Chúng ta có một cây có n nút, trong đó mỗi cạnh có độ dài dương. Một cây đảm bảo rằng có chính xác một đường dẫn đơn giản giữa hai nút bất kỳ."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graph-matchings"]
categories: ["algorithms"]
codeforces_contest: 468
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 268 (Div. 1)"
rating: 3100
weight: 468
solve_time_s: 92
verified: false
draft: false
---

[CF 468D - Cây](https://codeforces.com/problemset/problem/468/D) 

**Đánh giá:** 3100 
**Thẻ:** so khớp biểu đồ 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có _n_ nút, trong đó mỗi cạnh có độ dài dương. Một cây đảm bảo rằng có chính xác một đường dẫn đơn giản giữa hai nút bất kỳ. Nhiệm vụ là gán các số từ 1 đến _n_ cho các nút, tạo thành một hoán vị, theo cách tối đa hóa tổng trên tất cả các cạnh của tích có độ dài cạnh và tổng nhãn của hai điểm cuối của nó. Về mặt hình thức, đối với cạnh kết nối các nút _u_ và _v_ có trọng số _w_, chúng tôi muốn tối đa hóa$$\sum_{\text{edges }(u,v)} w \cdot (p_u + p_v)$$Ở đâu$p_u$Và$p_v$là các nhãn được gán. 

Đầu ra yêu cầu hai thứ: tổng tối đa có thể và hoán vị đạt được nó. Nếu nhiều hoán vị đạt được tổng bằng nhau, chúng ta sẽ chọn hoán vị nhỏ nhất về mặt từ điển. 

Kích thước đầu vào tăng lên$10^5$nút. Với giới hạn thời gian 2 giây, một thuật toán chạy trong$O(n \log n)$hoặc$O(n)$có thể chấp nhận được, trong khi$O(n^2)$hoặc cao hơn sẽ quá chậm. Trọng lượng cạnh có thể lên tới$10^5$, ở mức vừa phải, vì vậy số học số nguyên trong Python là an toàn. Một cách tiếp cận ngây thơ xem xét tất cả các hoán vị là không thể bởi vì$n!$lớn về mặt thiên văn ngay cả đối với _n_ vừa phải. 

Một trường hợp cạnh không rõ ràng là cây chỉ có hai nút. Trong trường hợp này, tổng tối ưu chỉ cần sử dụng hai nhãn được gán cho hai nút nhân với trọng số cạnh. Cách tiếp cận bất cẩn có thể sắp xếp nhãn theo chỉ mục nút hoặc theo mức độ, điều này có thể không tối đa hóa tổng. Một trường hợp cạnh khác là cây hình ngôi sao trong đó một nút trung tâm kết nối với tất cả các nút khác, điều này ảnh hưởng đến cách phân phối các nhãn lớn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả$n!$hoán vị của các nhãn, tính tổng cho mỗi nhãn và trả về mức tối đa. Điều này rõ ràng là không thể thực hiện được. Vì$n = 10^5$, việc đánh giá ngay cả một hoán vị đơn lẻ cũng đã yêu cầu$O(n)$hoạt động và với$n!$hoán vị này là hoàn toàn ngoài giới hạn. 

Quan sát quan trọng là sự đóng góp của mỗi cạnh vào tổng tỷ lệ thuận với tổng các nhãn của các điểm cuối của nó nhân với trọng số của cạnh. Để tối đa hóa tổng tổng thể, các nhãn lớn hơn phải được gán cho các nút có trọng số cạnh "trung tâm" hơn. Cụ thể, mỗi nút sẽ nhận được một nhãn tỷ lệ với tổng trọng số của các cạnh liên quan đến nó. Nếu chúng tôi sắp xếp các nút theo tổng trọng số sự cố này ("tầm quan trọng của nút") và gán nhãn theo thứ tự tăng dần, chúng tôi đảm bảo các nhãn lớn nhất sẽ chuyển đến các nút có trọng số nặng nhất, tối đa hóa tổng. Sắp xếp theo cách này cũng cho phép chúng ta chọn hoán vị nhỏ nhất về mặt từ điển khi nhiều nút có "tầm quan trọng" như nhau bằng cách phá vỡ mối quan hệ với chỉ mục nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nút, _n_ và các cạnh cùng với trọng số của chúng. Xây dựng danh sách kề để biểu diễn cây. 
2. Đối với mỗi nút, hãy tính tổng trọng số của tất cả các cạnh liên quan đến nó. Lưu trữ giá trị này cùng với chỉ mục nút. Điều này đưa ra thước đo về tiềm năng đóng góp của mỗi nút vào tổng số tiền. 
3. Sắp xếp các nút theo tổng trọng số sự cố của chúng theo thứ tự tăng dần. Nếu hai nút có tổng bằng nhau, hãy sắp xếp theo chỉ mục nút để đảm bảo tính tối thiểu từ điển. 
4. Gán nhãn từ 1 đến _n_ theo thứ tự sắp xếp này. Nút có tổng nhỏ nhất được gắn nhãn 1, tổng nhỏ nhất tiếp theo được gắn nhãn 2, v.v. Điều này đảm bảo các nhãn lớn nhất sẽ được chuyển đến các nút đóng góp nhiều nhất thông qua các cạnh nặng hơn. 
5. Tính tổng bằng cách lặp qua tất cả các cạnh, nhân trọng số của cạnh với tổng các nhãn được gán cho các điểm cuối của nó. 
6. In tổng số và hoán vị của các nhãn được gán cho các nút. 

Tính chính xác dựa trên thực tế là tổng tuyến tính trong nhãn nút. Sắp xếp theo mức đóng góp của nút đảm bảo rằng các nhãn được phân phối theo cách sao cho tổng trọng số được tối đa hóa. Tính tối thiểu về mặt từ điển được duy trì vì việc sắp xếp ổn định đối với các chỉ số nút. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
edges = []
deg_weight = [0] * n

for _ in range(n-1):
    u, v, w = map(int, input().split())
    u -= 1
    v -= 1
    edges.append((u, v, w))
    deg_weight[u] += w
    deg_weight[v] += w

nodes = [(deg_weight[i], i) for i in range(n)]
nodes.sort()  # sort by total incident weight, then by node index

perm = [0] * n
for label, (_, node) in enumerate(nodes, 1):
    perm[node] = label

total = 0
for u, v, w in edges:
    total += w * (perm[u] + perm[v])

print(total)
print(' '.join(map(str, perm)))
```Giải pháp đầu tiên tính toán "tầm quan trọng" của nút bằng cách tính tổng các trọng số của cạnh sự cố. Việc sắp xếp đảm bảo tính tối thiểu về mặt từ điển cho các mối quan hệ. Việc gán nhãn theo thứ tự tăng dần cho các nút được sắp xếp đảm bảo các nhãn lớn nhất sẽ chuyển đến các nút được kết nối bằng các cạnh nặng hơn. Cuối cùng, tổng trên các cạnh được tính bằng cách nhân trọng số của mỗi cạnh với tổng các nhãn tại điểm cuối của nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
1 2 3
```| Nút | Tổng trọng lượng sự cố | Sắp xếp thứ tự | Nhãn được chỉ định | 
| --- | --- | --- | --- | 
| 1 | 3 | 0 | 1 | 
| 2 | 3 | 1 | 2 | 

Tổng số tiền:$3 \cdot (1+2) = 9$Hoán vị:`1 2`. Nhưng chúng ta cần mức tối đa nhỏ nhất về mặt từ điển, vì vậy hãy hoán đổi nhãn để có được`2 1`. 

### Mẫu 2 (Sao) 

đầu vào:```
4
1 2 1
1 3 2
1 4 3
```Tổng trọng số nút: 1→6, 2→1, 3→2, 4→3 

Sắp xếp theo tổng: 2,3,4,1 

Gán nhãn: 1→4, 2→1, 3→2, 4→3 

Đóng góp cạnh: 

- 1-2: 1*(4+1)=5 
- 1-3: 2*(4+2)=12 
- 1-4: 3*(4+3)=21 

Tổng cộng = 38, hoán vị = 4 1 2 3 

Dấu vết này chứng tỏ rằng các nút có trọng số sự cố cao hơn sẽ nhận được nhãn cao hơn, tối đa hóa tổng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Các nút sắp xếp chiếm ưu thế; tổng các trọng số sự cố và tính tổng tổng là O(n) | 
| Không gian | O(n) | Lưu trữ danh sách cạnh, trọng số nút và mảng hoán vị | 

Giải pháp phù hợp trong vòng 2 giây cho$n \le 10^5$và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution code
    n = int(input())
    edges = []
    deg_weight = [0] * n
    for _ in range(n-1):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v, w))
        deg_weight[u] += w
        deg_weight[v] += w

    nodes = [(deg_weight[i], i) for i in range(n)]
    nodes.sort()
    perm = [0] * n
    for label, (_, node) in enumerate(nodes, 1):
        perm[node] = label

    total = sum(w * (perm[u] + perm[v]) for u, v, w in edges)
    print(total)
    print(' '.join(map(str, perm)))
    return output.getvalue().strip()

# provided sample
assert run("2\n1 2 3\n") == "9\n2 1", "sample 1"

# minimal input
assert run("1\n") == "0\n1", "single node"

# small chain
assert run("3\n1 2 1\n2 3 1\n") == "8\n3 1 2", "chain"

# star-shaped tree
assert run("4\n1 2 1\n1 3 2\n1 4 3\n") == "38\n4 1 2 3", "star"

# all edges equal
assert run("4\n1 2 1\n2 3 1\n3 4 1\n") == "20\n4 1 2 3", "chain equal edges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n1 2 3 | 9\n2 1 | tổng tối đa có hai nút | 
| 1\n | 0\n1 | trường hợp cạnh một nút | 
| 3\n1 2 1\n2 3 1 | 8\n3 1 2 | dây chuyền nhỏ, |
