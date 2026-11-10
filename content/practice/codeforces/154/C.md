---
title: "CF 154C - Biên dạng kép"
description: "Chúng ta được cung cấp một đồ thị vô hướng thể hiện một mạng xã hội. Mỗi đỉnh là một hình cắt và mỗi cạnh là một tình bạn. Hai cấu hình riêng biệt i và j được coi là gấp đôi nếu mọi cấu hình khác nhìn chúng theo cách giống hệt nhau."
date: "2026-06-02T16:54:31+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "hashing", "sortings"]
categories: ["algorithms"]
codeforces_contest: 154
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 109 (Div. 1)"
rating: 2300
weight: 154
solve_time_s: 63
verified: true
draft: false
---

[CF 154C - Cấu hình kép](https://codeforces.com/problemset/problem/154/C) 

**Đánh giá:** 2300 
**Thẻ:** biểu đồ, băm, sắp xếp 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng thể hiện một mạng xã hội. Mỗi đỉnh là một hình cắt và mỗi cạnh là một tình bạn. 

Hai hồ sơ riêng biệt`i`Và`j`được coi là gấp đôi nếu mọi hồ sơ khác đều nhìn nhận chúng theo cách giống hệt nhau. Với mọi đỉnh`k ≠ i, j`, cả hai cạnh`(i, k)`Và`(j, k)`tồn tại hoặc không tồn tại. Tình trạng tình bạn giữa`i`Và`j`bản thân chúng không quan trọng đối với định nghĩa này. 

Trong thuật ngữ đồ thị, nếu chúng ta bỏ qua các đỉnh thì`i`Và`j`phải có các lân cận giống hệt nhau. Sự tinh tế duy nhất là sự hiện diện hay vắng mặt của cạnh`(i, j)`không nên ảnh hưởng đến việc so sánh. 

Chúng ta phải đếm có bao nhiêu cặp đỉnh không có thứ tự thỏa mãn tính chất này. 

Những hạn chế là vô cùng lớn. Cả hai`n`Và`m`có thể đạt được`10^6`. Một biểu đồ có một triệu đỉnh ngay lập tức loại trừ bất kỳ thuật toán nào so sánh các danh sách kề theo cặp. Thậm chí việc lưu trữ một`n × n`cấu trúc là không thể. Với giới hạn thời gian 3 giây, chúng ta cần thứ gì đó gần tuyến tính hoặc`O(m log n)`. 

Đồ thị thưa thớt vì`m ≤ 10^6`, trong khi một biểu đồ hoàn chỉnh trên`10^6`các đỉnh sẽ chứa khoảng`5·10^11`các cạnh. Điều này cho thấy rằng việc xử lý dựa trên danh sách kề là hướng đi đúng đắn. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Xét các đỉnh cô lập:```
4 0
```Mỗi đỉnh có cùng một lân cận, gọi là tập rỗng. Mỗi cặp là một đôi, vì vậy câu trả lời là:```
6
```Một giải pháp chỉ xử lý các đỉnh xuất hiện ở các cạnh có thể quên hoàn toàn các đỉnh bị cô lập. 

Một trường hợp tinh tế khác là khi hai đỉnh khác nhau chỉ vì chúng được kết nối với nhau.```
2 1
1 2
```Câu trả lời là:```
1
```Không có đỉnh thứ ba nên điều kiện là đúng. Nếu chúng ta so sánh danh sách kề thông thường, chúng ta sẽ nhận được`{2}`so với`{1}`, khác nhau. So sánh trực tiếp vùng lân cận sẽ từ chối cặp này một cách không chính xác. 

Một ví dụ lớn hơn một chút cho thấy hiện tượng tương tự:```
3 2
1 3
2 3
```Đỉnh`1`Và`2`là gấp đôi vì cả hai chỉ được kết nối với đỉnh`3`. 

Bây giờ thêm cạnh`(1,2)`:```
3 3
1 2
1 3
2 3
```Đỉnh`1`Và`2`vẫn là gấp đôi. Sự khác biệt duy nhất trong danh sách lân cận của họ là lẫn nhau. Bất kỳ cách tiếp cận nào so sánh các vùng lân cận đầy đủ mà không tính đến điều này sẽ thất bại. 

Khó khăn cốt lõi là xử lý cụm từ "tất cả các đỉnh khác" một cách hiệu quả. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là kiểm tra từng cặp đỉnh`(i,j)`. Đối với mỗi cặp, so sánh mối quan hệ của chúng với tất cả các đỉnh còn lại. Nếu mọi đỉnh thứ ba đều nhìn thấy`i`Và`j`giống hệt nhau, đếm cặp. 

Điều này đúng vì nó tuân theo định nghĩa theo nghĩa đen. Thật không may là có`O(n²)`cặp và mỗi so sánh có thể yêu cầu kiểm tra`O(n)`đỉnh. Trường hợp xấu nhất là`O(n³)`, điều này hoàn toàn không thể xảy ra đối với`n = 10^6`. 

Chúng ta cần phải định dạng lại điều kiện. 

Giả định`N(v)`biểu thị tập hợp các đỉnh lân cận`v`. 

Nếu đỉnh`u`Và`v`không liền kề nhau thì định nghĩa chỉ đơn giản nói:```
N(u) = N(v)
```vì không lân cận nào chứa đỉnh kia. 

Nếu đỉnh`u`Và`v`kề nhau thì mỗi lân cận chứa đỉnh còn lại. Sau khi loại bỏ nhau, các hàng xóm còn lại phải khớp:```
N(u) \ {v} = N(v) \ {u}
```Quan sát này biến vấn đề thành nhóm các đỉnh theo chữ ký lân cận. 

Thách thức là danh sách kề có thể chứa tới một triệu đỉnh, do đó việc so sánh trực tiếp các tập hợp là quá tốn kém. Thay vào đó, chúng tôi gán cho mỗi đỉnh một giá trị băm và biểu thị một vùng lân cận bằng tổng số giá trị băm của các thành viên của nó. 

Cho phép`rnd[x]`là số 64 bit ngẫu nhiên được gán cho đỉnh`x`. 

Đối với mọi đỉnh:```
H(v) = Σ rnd[x]   over all x in N(v)
```Với xác suất áp đảo, các tập lân cận bằng nhau tạo ra các tổng bằng nhau và các tập khác nhau tạo ra các tổng khác nhau. 

Bây giờ hãy xem xét hai trường hợp. 

Đối với các cặp đôi không liền kề, chúng ta cần các vùng lân cận bình thường bằng nhau. Chúng ta có thể đơn giản nhóm các đỉnh theo`H(v)`. 

Đối với các cặp đôi liền kề, chúng ta cần:```
N(u) \ {v} = N(v) \ {u}
```Tương đương:```
H(u) - rnd[v] = H(v) - rnd[u]
```Định nghĩa:```
G(u,v) = H(u) + rnd[u]
```Khi đó các đỉnh liền kề tạo thành một cặp hợp lệ chính xác khi:```
H(u) + rnd[u] = H(v) + rnd[v]
```Điều này dẫn đến một chiến lược rất hiệu quả. 

Đầu tiên đếm tất cả các cặp đỉnh giống nhau`H(v)`. Đây là những đôi không liền kề. 

Sau đó đếm tất cả các cặp đỉnh giống nhau`H(v)+rnd[v]`. Điều này bao gồm cả cặp liền kề và không liền kề, vì vậy chúng tôi không thể thêm tất cả chúng. Thay vào đó, chúng tôi chỉ kiểm tra các cạnh thực tế`(u,v)`và kiểm tra xem:```
H(u)+rnd[u] = H(v)+rnd[v]
```Mỗi cạnh như vậy đóng góp thêm một cặp đôi. 

Đồ thị chỉ có`m ≤ 10^6`các cạnh, vì vậy lần kiểm tra cuối cùng này rẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n + m) dự kiến ​​| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gán mỗi đỉnh một số nguyên 64 bit ngẫu nhiên`rnd[i]`. 
2. Tạo một mảng`H`được khởi tạo về 0. 
3. Cho mọi cạnh`(u,v)`, thêm vào`rnd[v]`ĐẾN`H[u]`và thêm`rnd[u]`ĐẾN`H[v]`. 

Sau bước này,`H[v]`là hàm băm của lân cận đỉnh`v`. 
4. Nhóm tất cả các đỉnh theo`H[v]`. 

Nếu một nhóm chứa`k`đỉnh, thêm:```
k(k-1)/2
```để trả lời. 

Đây là những cặp có các khu dân cư bình thường giống hệt nhau. 
5. Đối với mỗi đỉnh tính toán:```
T[v] = H[v] + rnd[v]
```6. Cho mọi cạnh`(u,v)`, kiểm tra xem:```
T[u] = T[v]
```Nếu đúng, hãy thêm một vào câu trả lời. 

Điều này tương ứng chính xác với:```
H(u)-rnd[v] = H(v)-rnd[u]
```có nghĩa là các vùng lân cận trở nên giống hệt nhau sau khi loại bỏ nhau. 
7. Xuất số đếm cuối cùng. 

### Tại sao nó hoạt động 

Đối với các đỉnh không liền kề, điều kiện kép tương đương với sự bằng nhau của toàn bộ các tập lân cận của chúng. Hàm băm`H(v)`được xây dựng từ tất cả các lân cận của`v`, do đó các vùng lân cận bằng nhau sẽ tạo ra các giá trị băm bằng nhau. Nhóm các giá trị băm bằng nhau sẽ tính chính xác các cặp đó. 

Đối với các đỉnh liền kề, mỗi vùng lân cận khác nhau bởi sự hiện diện của điểm cuối khác. Việc xóa điểm cuối đó khỏi cả hai phía sẽ đưa ra điều kiện:```
N(u)\{v} = N(v)\{u}
```Biểu diễn hàm băm biến đổi điều này thành:```
H(u)-rnd[v] = H(v)-rnd[u]
```về mặt đại số tương đương với:```
H(u)+rnd[u] = H(v)+rnd[v]
```Chỉ kiểm tra sự bằng nhau này trên các cạnh hiện tại sẽ tính chính xác các cặp đôi liền kề. 

Mỗi cặp đôi đều thuộc một trong hai loại này và không có cặp nào được tính hai lần. Do đó thuật toán là đúng. 

## Giải pháp Python```python
import sys
import random
from collections import defaultdict

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    rnd = [0] * (n + 1)
    rng = random.Random(154154)

    for i in range(1, n + 1):
        rnd[i] = rng.getrandbits(64)

    h = [0] * (n + 1)
    edges = []

    for _ in range(m):
        u, v = map(int, input().split())
        edges.append((u, v))

        h[u] += rnd[v]
        h[v] += rnd[u]

    groups = defaultdict(int)

    for i in range(1, n + 1):
        groups[h[i]] += 1

    ans = 0

    for cnt in groups.values():
        ans += cnt * (cnt - 1) // 2

    t = [0] * (n + 1)
    for i in range(1, n + 1):
        t[i] = h[i] + rnd[i]

    for u, v in edges:
        if t[u] == t[v]:
            ans += 1

    print(ans)

solve()
```Giai đoạn đầu tiên gán một giá trị 64-bit ngẫu nhiên cho mỗi đỉnh. Những giá trị này đóng vai trò là dấu vân tay cho việc xây dựng khu phố. 

Trong khi đọc các cạnh, mã sẽ cập nhật hàm băm lân cận của cả hai điểm cuối. Sau khi xử lý tất cả các cạnh,`h[v]`chứa tổng dấu vân tay của tất cả các hàng xóm của`v`. 

Từ điển`groups`đếm có bao nhiêu đỉnh có chung hàm băm lân cận. Nếu một hàm băm xuất hiện `
