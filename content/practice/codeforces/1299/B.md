---
title: "CF 1299B - Khí động học"
description: "Chúng ta có một đa giác lồi hoàn toàn $P$ trong mặt phẳng dưới dạng danh sách các đỉnh của nó theo thứ tự ngược chiều kim đồng hồ. Mỗi đỉnh có tọa độ nguyên và không có 3 đỉnh nào thẳng hàng."
date: "2026-06-11T18:23:56+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 1299
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 618 (Div. 1)"
rating: 1800
weight: 1299
solve_time_s: 118
verified: true
draft: false
---

[CF 1299B - Khí động lực học](https://codeforces.com/problemset/problem/1299/B) 

**Đánh giá:** 1800 
**Thẻ:** hình học 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đa giác lồi hoàn toàn$P$trong mặt phẳng dưới dạng danh sách các đỉnh của nó theo thứ tự ngược chiều kim đồng hồ. Mỗi đỉnh có tọa độ nguyên và không có 3 đỉnh nào thẳng hàng. Từ$P$, chúng tôi xác định một đa giác mới$T$là tập hợp tất cả các vectơ$\overrightarrow{AB}$Ở đâu$A$Và$B$là những điểm trong$P$. Về mặt hình học,$T$là đa giác được hình thành bởi tất cả các bản dịch của$P$chứa đựng nguồn gốc. Nhiệm vụ là xác định xem liệu$P$Và$T$giống nhau về mặt hình học - chúng có hình dạng giống nhau về tỷ lệ và góc xoay. 

Đầu vào mang lại$n$, số đỉnh, theo sau là$n$các cặp tọa độ. Đầu ra phải là "CÓ" nếu$P$Và$T$tương tự nhau và "KHÔNG" nếu không. 

Được cho$n$có thể đạt được$10^5$và tọa độ có thể lên tới$10^9$, thuật toán phải chạy trong khoảng$O(n)$hoặc$O(n \log n)$thời gian. Bất kỳ cách tiếp cận nào có vòng lặp lồng nhau trên tất cả các cặp điểm ($O(n^2)$) bị loại trừ ngay lập tức. 

Trường hợp cạnh tinh tế phát sinh với các đa giác rất nhỏ hoặc các hình dạng không đối xứng cao. Ví dụ: một tam giác có các đỉnh$(0,0),(2,0),(1,3)$tạo ra một$T$đó là một hình lục giác có tỉ số cạnh khác nhau. Một cách tiếp cận ngây thơ chỉ nhìn vào các cạnh hoặc các hộp giới hạn có thể khẳng định sự giống nhau một cách không chính xác. Việc kiểm tra đúng phải tính đến các vectơ tương đối giữa các điểm, không chỉ độ dài các cạnh. 

## Phương pháp tiếp cận 

Cách thức mạnh mẽ để xây dựng$T$là lặp lại tất cả các cặp đỉnh$(A,B)$TRONG$P$và lưu trữ vectơ$\overrightarrow{AB}$như một đỉnh của$T$. Sau đó chúng ta sẽ sắp xếp các vectơ kết quả ngược chiều kim đồng hồ và so sánh với$P$cho sự giống nhau. Đây là$O(n^2)$và ngay lập tức không thực tế đối với$n=10^5$, vì nó sẽ hoạt động xung quanh$10^{10}$hoạt động. 

Cái nhìn sâu sắc quan trọng là$T$thực sự là tổng Minkowski của$P$Và$-P$(đa giác thu được bằng cách phản ánh$P$qua gốc). Tổng Minkowski của các đa giác lồi bảo toàn các hướng cạnh và tạo ra một đa giác có số đỉnh gấp đôi. Đối với đa giác lồi hoàn toàn, một kết quả đã biết là$P$Và$T$chỉ có thể giống nhau nếu$P$là đối xứng ở tâm. Cụ thể hơn,$T$có các đỉnh tại trung điểm giữa các cạnh đối diện trong$P$, do đó điều kiện giảm xuống còn việc kiểm tra điều đó đối với mọi vectơ cạnh trong nửa đầu của$P$, tồn tại một vectơ cạnh song song và có độ dài bằng nhau ở nửa đối diện. 

Điều này có nghĩa là chúng ta có thể giải quyết vấn đề bằng cách so sánh chuỗi các vectơ cạnh trong$P$với chuỗi được hình thành bằng cách tính tổng các cạnh đối diện, tất cả đều có$O(n)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính các vectơ cạnh của$P$theo thứ tự. Đối với các đỉnh$v_0, v_1, ..., v_{n-1}$, vectơ cạnh$e_i = v_{i+1} - v_i$modulo$n$. Mỗi vectơ mã hóa cả hướng và chiều dài của cạnh. 
2. Kiểm tra xem$n$là chẵn. Nếu như$n$thật kỳ lạ,$P$không thể đối xứng tâm nên$P$Và$T$không thể giống được. Đầu ra "KHÔNG". 
3. Thậm chí$n$, chia các cạnh thành hai nửa. Ghép từng cạnh$e_i$với$e_{i + n/2}$. Tổng của mỗi cặp phải bằng 0 nếu$P$là đối xứng ở tâm. Điều này mã hóa trực tiếp điều kiện đối xứng tổng Minkowski. 
4. Lặp lại$i$từ 0 đến$n/2 - 1$. Nếu như$e_i + e_{i+n/2} \neq (0,0)$cho bất kỳ$i$, xuất ra "KHÔNG". Nếu không thì xuất ra "CÓ". 

**Tại sao nó hoạt động:** Đối với đa giác lồi,$T = P + (-P)$. Hai đa giác giống nhau nếu một đa giác là bản sao được thu nhỏ và xoay của đa giác kia. Đối với lồi$P$, điều này xảy ra khi và chỉ khi$P$đối xứng tâm, bởi vì$T$tăng gấp đôi hình dạng một cách hiệu quả bằng cách phản chiếu. Bằng cách kiểm tra các cặp vectơ cạnh như trên, chúng ta xác minh được tính đối xứng trung tâm mà không cần xây dựng một cách rõ ràng$T$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
vertices = [tuple(map(int, input().split())) for _ in range(n)]

if n % 2 == 1:
    print("NO")
    sys.exit(0)

edges = []
for i in range(n):
    x1, y1 = vertices[i]
    x2, y2 = vertices[(i+1)%n]
    edges.append((x2-x1, y2-y1))

half = n // 2
for i in range(half):
    ex, ey = edges[i]
    ex_op, ey_op = edges[i+half]
    if ex + ex_op != 0 or ey + ey_op != 0:
        print("NO")
        sys.exit(0)

print("YES")
```Giải pháp đọc các đỉnh đa giác và ngay lập tức loại bỏ các đa giác có kích thước lẻ. Các vectơ cạnh được tính toán theo thứ tự modulo$n$để xử lý sự bao quanh. Mỗi cạnh được ghép với cạnh đối diện của nó; không thực hiện bất kỳ kiểm tra cặp nào đều trả về "KHÔNG". "CÓ" cuối cùng chỉ xảy ra nếu tất cả các cặp thỏa mãn tính đối xứng trung tâm. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
4
1 0
4 1
3 4
0 3
```Tính toán các cạnh: 

| Cạnh | Vectơ | 
| --- | --- | 
| 0->1 | (3,1) | 
| 1->2 | (-1,3) | 
| 2->3 | (-3,-1) | 
| 3->0 | (0,-3) | 

Kiểm tra các cặp: (3,1)+(-3,-1)=(0,0), (-1,3)+(0,-3)=( -1+0,3-3)=(-1,0) → thất bại? Chờ kiểm tra cẩn thận. 

Tính toán cạnh phải được bọc cẩn thận: 

0->1: (4-1,1-0) = (3,1) 

1->2: (3-4,4-1) = (-1,3) 

2->3: (0-3,3-4) = (-3,-1) 

3->0: (1-0,0-3) = (1,-3) 

Bây giờ các cặp: 

(0,1) tổng: (3,1)+(-3,-1)=(0,0) 

(1,2) tổng: (-1,3)+(1,-3)=(0,0) 

Đầu ra: CÓ 

**Mẫu 2 (tam giác, n=3):**```
3
0 0
1 0
0 1
```n là số lẻ → đầu ra NO 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tính các vectơ cạnh, một lượt để kiểm tra các cặp đối xứng | 
| Không gian | O(n) | Lưu trữ các vectơ cạnh và đỉnh | 

Điều này phù hợp thoải mái trong vòng 1 giây cho$n=10^5$với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    captured = io.StringIO()
    sys.stdout = captured
    n = int(input())
    vertices = [tuple(map(int, input().split())) for _ in range(n)]

    if n % 2 == 1:
        print("NO")
        return captured.getvalue().strip()

    edges = []
    for i in range(n):
        x1, y1 = vertices[i]
        x2, y2 = vertices[(i+1)%n]
        edges.append((x2-x1, y2-y1))

    half = n // 2
    for i in range(half):
        ex, ey = edges[i]
        ex_op, ey_op = edges[i+half]
        if ex + ex_op != 0 or ey + ey_op != 0:
            print("NO")
            return captured.getvalue().strip()

    print("YES")
    return captured.getvalue().strip()

# provided samples
assert run("4\n1 0\n4 1\n3 4\n0 3\n") == "YES", "sample 1"
assert run("3\n0 0\n1 0\n0 1\n") == "NO", "sample 2"

# custom cases
assert run("4\n0 0\n2 0\n2 2\n0 2\n") == "YES", "square"
assert run("6\n0 0\n2 0\n3 1\n2 2\n0 2\n-1 1\n") == "YES", "hexagon symmetric"
assert run("4\n0 0\n1 0\n
```
