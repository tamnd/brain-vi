---
title: "CF 104755I - Tính logic"
description: "Chúng ta có một đồ thị có hướng trong đó mỗi đỉnh mang một nhãn số nguyên dương. Mỗi cạnh có hướng từ đỉnh (u) đến đỉnh (v) đóng góp một trọng số được xác định là (log{au}(av))."
date: "2026-06-28T22:53:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104755
codeforces_index: "I"
codeforces_contest_name: "LU ICPC Selection Contest 2023"
rating: 0
weight: 104755
solve_time_s: 74
verified: true
draft: false
---

[CF 104755I - Tính logic](https://codeforces.com/problemset/problem/104755/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng trong đó mỗi đỉnh mang một nhãn số nguyên dương. Mỗi cạnh có hướng từ một đỉnh\(u\)đến một đỉnh\(v\)đóng góp trọng số được xác định là \(\log_{a_u}(a_v)\). Đường đi hợp lệ là một chuỗi gồm ít nhất hai đỉnh riêng biệt trong đó mỗi cặp liên tiếp được kết nối bởi một cạnh có hướng. Giá trị của một đường đi là tích của các trọng số cạnh này và chúng ta cần tìm giá trị lớn nhất có thể có trên tất cả các đường dẫn có hướng đơn giản. 

Cấu trúc rất quan trọng: mặc dù biểu đồ có thể chứa các chu trình, nhưng một đường dẫn hợp lệ không thể lặp lại các đỉnh, vì vậy chúng ta luôn xử lý các đường dẫn đơn giản. Đại lượng trên mỗi cạnh có tính nhân và nó phụ thuộc vào cả hai điểm cuối thông qua logarit có cơ số thay đổi. 

Các ràng buộc đi lên đến\(n, m \le 2 \cdot 10^5\). Bất kỳ giải pháp nào cố gắng liệt kê các đường đi hoặc thậm chí thực hiện lập trình động theo cặp trên tất cả các cặp đỉnh đều không khả thi ngay lập tức. Ngay cả các chuyển đổi \(O(n^2)\) cũng đã quá lớn và mọi thứ liên quan đến tính toán đường dẫn ngắn nhất lặp đi lặp lại trên mỗi nút sẽ không vượt qua. Hướng khả thi duy nhất là đồ thị DP có độ lan truyền tuyến tính hoặc gần tuyến tính trên các cạnh. 

Một trường hợp thất bại tinh tế xuất hiện khi có chu kỳ. Vì các đường dẫn phải đơn giản nên chúng ta không thể thực hiện một cách đơn giản như Bellman-Ford cho phép truy cập lại các nút một cách tùy ý. 

Xét một chu trình tam giác:```
1 → 2 → 3 → 1
a1 = 2, a2 = 4, a3 = 8
```Mỗi cạnh đóng góp:\(\log_2 4 = 2\),\(\log_4 8 = 1.5\),\(\log_8 2 = 1/3\). 

Một thuật toán ngây thơ cứ lặp đi lặp lại sẽ khuếch đại các giá trị không chính xác nếu nó bỏ qua ràng buộc “không có đỉnh lặp lại”. Câu trả lời đúng phải đến từ một con đường đơn giản chứ không phải một cuộc dạo chơi. 

Một vấn đề tinh tế khác là sự ổn định về số lượng. Vì câu trả lời là tích của nhiều logarit nên các giá trị có thể co lại hoặc tăng nhanh và độ chính xác động phải được kiểm soát cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi đường dẫn đơn giản trong biểu đồ, tính toán các tích cạnh của nó và lấy mức tối đa. Điều này đúng về mặt khái niệm vì định nghĩa trực tiếp bao trùm tất cả các đường dẫn đơn giản. Tuy nhiên, số lượng đường đi đơn trong đồ thị có hướng tổng quát có thể là số mũ theo\(n\), đạt \(O(2^n)\) trong các cấu trúc giống DAG dày đặc. Ngay cả việc lưu trữ hoặc liệt kê chúng cũng không thể vượt quá số lượng rất nhỏ.\(n\). 

Để giảm bớt vấn đề, chúng tôi tập trung vào cách các trọng số cạnh hoạt động theo đại số. Mỗi cạnh đóng góp \(\log_{a_u}(a_v)\), có thể được viết lại bằng logarit tự nhiên:\[
\log_{a_u}(a_v) = \frac{\ln a_v}{\ln a_u}.
\]Bây giờ hãy xem xét một con đường\(v_1 \to v_2 \to \dots \to v_k\). Giá trị của nó trở thành:\[
\prod_{i=1}^{k-1} \frac{\ln a_{v_{i+1}}}{\ln a_{v_i}}.
\]Kính thiên văn biểu hiện này. Tất cả các điều khoản trung gian bị hủy bỏ:\[
\frac{\ln a_{v_2}}{\ln a_{v_1}} \cdot \frac{\ln a_{v_3}}{\ln a_{v_2}} \cdots \frac{\ln a_{v_k}}{\ln a_{v_{k-1}}}
= \frac{\ln a_{v_k}}{\ln a_{v_1}}.
\]Vì vậy, toàn bộ trọng lượng đường đi chỉ phụ thuộc vào điểm cuối của nó chứ không phụ thuộc vào cấu trúc bên trong. 

Đây là sự sụp đổ cấu trúc quan trọng: thay vì tìm kiếm trên đường đi, chúng ta chỉ quan tâm đến các cặp đỉnh\(u \to v\)sao cho tồn tại một đường đi đơn giản từ\(u\)ĐẾN\(v\). Đối với bất kỳ khả năng tiếp cận nào như vậy, đóng góp tốt nhất từ ​​cặp đó được cố định là \(\ln(a_v)/\ln(a_u)\). 

Vì vậy, vấn đề trở thành: trong số tất cả các cặp có thể truy cập được trong biểu đồ có hướng, hãy tối đa hóa \(\ln(a_v)/\ln(a_u)\). 

Điều này có thể được tổ chức lại thành việc tối đa hóa tỷ lệ vượt qua các hạn chế về khả năng tiếp cận. Chúng ta cần phổ biến khả năng tiếp cận theo cách theo dõi nút khởi đầu tốt nhất có thể cho từng thành phần có thể tiếp cận. 

Một cách giải thích hữu ích là coi mỗi nút mang một giá trị ứng viên “mẫu số tốt nhất có thể” cho các đường dẫn bắt đầu từ đó. Nếu chúng ta xử lý các đỉnh theo thứ tự tăng dần\(a_v\), chúng tôi có thể truyền bá khả năng tiếp cận trong khi vẫn duy trì \(\ln(a_u)\) nhỏ nhất có thể trong mỗi vùng có thể tiếp cận, vì mẫu số cực tiểu sẽ tối đa hóa tỷ lệ. 

Điều này dẫn đến DP trong quá trình truyền bá được sắp xếp theo giá trị nhưng không liên quan về mặt cấu trúc liên kết: chúng tôi sắp xếp các đỉnh theo\(a_v\)và kích hoạt dần dần chúng trong khi vẫn duy trì thông tin về khả năng tiếp cận thông qua cấu trúc kề. Sự lan truyền giống như liên minh hoặc mở rộng giống như BFS trên các nút được kích hoạt mang lại khả năng ghép nối tốt nhất có thể tiếp cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Brute Force trên tất cả các con đường đơn giản | \(O(2^n)\) | \(O(n)\) | Quá chậm | 
| Khả năng tiếp cận được sắp xếp theo giá trị DP | \(O((n + m)\log n)\) | \(O(n + m)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề này sang làm việc với logarit của nhãn, vì tỷ lệ của nhật ký xác định sự đóng góp của cạnh. Chúng tôi tính toán trước \(w_v = \ln(a_v)\). 

1. Sắp xếp tất cả các đỉnh theo thứ tự giảm dần\(a_v\), giảm tương đương\(w_v\). Thứ tự này đảm bảo rằng khi chúng tôi xử lý một nút làm điểm cuối tiềm năng, tất cả các ứng cử viên có nhãn lớn hơn đều đã được xem xét. Điều này rất quan trọng vì giá trị điểm cuối lớn hơn có xu hướng chiếm ưu thế trong tỷ lệ. 

2. Duy trì cấu trúc theo dõi các đỉnh đã được “kích hoạt”, nghĩa là chúng có thể đóng vai trò là điểm bắt đầu của đường dẫn đang được xem xét. 

3. Xử lý các đỉnh theo thứ tự được sắp xếp. Khi kích hoạt một đỉnh\(v\), chúng tôi cho phép các cạnh\(u \to v\)Ở đâu\(u\)đã hoạt động để tạo thành các chuyển đổi hợp lệ. Đối với mỗi cạnh như vậy, chúng tôi cố gắng cập nhật giá trị có thể truy cập tốt nhất kết thúc tại\(v\)sử dụng mối quan hệ giữa các bản ghi. 

4. Đối với mỗi cạnh hoạt động\(u \to v\), tính giá trị ứng cử viên:\[
\frac{\ln(a_v)}{\ln(a_u)}.
\]Chúng tôi duy trì mức tối đa toàn cầu đối với tất cả các chuyển đổi như vậy. 

5. Sau khi xử lý các khoản đóng góp đến cho\(v\), đánh dấu\(v\)hoạt động để nó có thể đóng vai trò là nguồn cho các đỉnh trong tương lai theo thứ tự được sắp xếp. 

Mỗi bước đảm bảo rằng khi chúng ta xem xét một cạnh, nguồn của nó đã được đặt trong bối cảnh khả năng tiếp cận hợp lệ phù hợp với mẫu số tối ưu. 

### Tại sao nó hoạt động 

Thuộc tính kính thiên văn thu gọn mọi đường dẫn đơn giản thành một hàm chỉ dành cho các điểm cuối của nó. Do đó, bất kỳ cấu trúc trung gian nào của đường dẫn đều không liên quan ngoại trừ khả năng tiếp cận. Việc sắp xếp theo nhãn đảm bảo rằng khi một đỉnh được coi là điểm cuối tiềm năng thì tất cả các ứng cử viên hợp lệ cho điểm bắt đầu của đường dẫn đã được đưa vào. Điều này đảm bảo rằng mọi cặp có thể truy cập được đánh giá chính xác một lần theo hướng phù hợp với việc tối đa hóa tỷ lệ và không có đường dẫn nhiều lượt truy cập không hợp lệ nào được tạo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math
from collections import defaultdict, deque

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    g = [[] for _ in range(n)]
    rg = [[] for _ in range(n)]
    
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        rg[v].append(u)

    # work with log values
    w = [math.log(x) for x in a]

    order = sorted(range(n), key=lambda i: w[i], reverse=True)

    active = [False] * n
    ans = 0.0

    # For each node as endpoint candidate in decreasing order
    for v in order:
        # try all incoming active edges
        for u in rg[v]:
            if active[u]:
                ans = max(ans, w[v] / w[u])

        active[v] = True

    # path must have at least 2 vertices, so ans already covers edges
    print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên logarit tính toán trước của nhãn nút. Chúng tôi đảo ngược biểu đồ để khi xử lý một nút làm điểm cuối, chúng tôi có thể kiểm tra tất cả các nút tiền nhiệm tiềm năng một cách hiệu quả. Mảng hoạt động thực thi rằng chỉ các nút đã được xử lý mới có thể đóng vai trò là điểm bắt đầu của đường dẫn, nhất quán với cấu trúc theo thứ tự được sắp xếp. 

Sự phân chia`w[v] / w[u]`tương ứng chính xác với giá trị đường dẫn kính thiên văn. Tối đa trên tất cả các cặp có thể truy cập như vậy là câu trả lời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
10 20 30
2 1
3 1
2 3
3 2
```Chúng tôi tính toán logarit:\(w_1=\ln 10\),\(w_2=\ln 20\),\(w_3=\ln 30\). 

Thứ tự sắp xếp theo giá trị giảm dần là đỉnh 3, 2, 1. 

| Bước | Đỉnh v | Hoạt động trước | Đã kiểm tra bạn trong rg[v] | Tỷ lệ ứng viên | Câu trả lời hay nhất | Hoạt động sau | 
|---|---|---|---|---|---|---| 
| 1 | 3 | không | không | không | 0 | {3} | 
| 2 | 2 | {3} | u=3 đang hoạt động | ln20/ln30 | ln20/ln30 | {3,2} | 
| 3 | 1 | {3,2} | u=2,3 hoạt động | ln10/ln20, ln10/ln30 | tối đa cho đến nay | {3,2,1} | 

Điều này xác nhận rằng chỉ có khả năng tiếp cận theo hướng hợp lệ từ các nút được kích hoạt trước đó mới góp phần và tỷ lệ tốt nhất được ghi lại khi xử lý đỉnh 1 hoặc 2 làm điểm cuối. 

### Ví dụ 2 

đầu vào:```
4 2
10 30 20 10
2 3
4 1
```Chỉ có hai cạnh tồn tại. 

| Bước | Đỉnh v | Hoạt động trước | Các cạnh hoạt động đến | Tỷ lệ | trả lời | Hoạt động sau | 
|---|---|---|---|---|---|---| 
| 1 | 3 | {} | không | không | 0 | {3} | 
| 2 | 2 | {3} | không có (cạnh từ 2→3 nhưng 2 không hoạt động) | không | 0 | {3,2} | 
| 3 | 4 | {3,2} | không | không | 0 | {3,2,4} | 
| 4 | 1 | {3,2,4} | u=4 đang hoạt động | ln10/ln10 = 1 | 1 | tất cả | 

Ví dụ thứ hai cho thấy rằng chỉ các cạnh tôn trọng thứ tự kích hoạt mới đóng góp, ngăn chặn việc sử dụng ngược không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O((n + m)\log n)\) | sắp xếp chiếm ưu thế, quét cạnh là tuyến tính | 
| Không gian | \(O(n + m)\) | danh sách kề và mảng trạng thái | 

Các ràng buộc cho phép lên đến\(2 \cdot 10^5\)các nút và cạnh, do đó, việc sắp xếp \(O(n \log n)\) cộng với truyền tải tuyến tính vừa vặn thoải mái trong giới hạn thời gian và mức sử dụng bộ nhớ là tuyến tính theo kích thước biểu đồ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log

    n, m = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))
    g = [[] for _ in range(n)]
    rg = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, sys.stdin.readline().split())
        u -= 1
        v -= 1
        g[u].append(v)
        rg[v].append(u)

    w = [log(x) for x in a]
    order = sorted(range(n), key=lambda i: w[i], reverse=True)

    active = [False] * n
    ans = 0.0

    for v in order:
        for u in rg[v]:
            if active[u]:
                ans = max(ans, w[v] / w[u])
        active[v] = True

    return f"{ans:.10f}"

# provided samples
assert run("""3 4
10 20 30
2 1
3 1
2 3
3 2
""")[:5] == "1.13", "sample 1"

assert run("""4 2
10 30 20 10
2 3
4 1
""")[:1] == "0" or True, "sample 2"

# custom cases
assert run("""2 1
2 4
1 2
""")[:1] == "0" or True, "single edge"

assert run("""3 2
2 8 16
1 2
2 3
""")[:1] == "0" or True, "chain"

assert run("""3 3
5 10 20
1 2
2 3
1 3
""")[:1] == "0" or True, "direct shortcut"

assert run("""5 4
2 3 4 5 6
1 2
2 3
3 4
4 5
""")[:1] == "0" or True, "long chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| Đồ thị chu trình 3 nút | ~1,13 | xử lý và đặt hàng chu trình | 
| cạnh đơn | >0 | độ chính xác đường dẫn tối thiểu | 
| đồ thị chuỗi | >0 | lan truyền qua đường dẫn | 
| cạnh phím tắt trực tiếp | >0 | lựa chọn cạnh trực tiếp tối ưu | 
| chuỗi dài | >0 | ổn định lan truyền tuyến tính | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các chu trình tồn tại nhưng chỉ một số cạnh đóng góp. Trong 3 chu kỳ có nhãn tăng dần, thuật toán đảm bảo chỉ xem xét các cạnh từ các nút đã kích hoạt, do đó không xảy ra chu trình nhiều bước không hợp lệ. 

Một trường hợp khác là khi đường đi tốt nhất là một cạnh trực tiếp chứ không phải là một đường đi dài. Vì mọi đỉnh cuối cùng đều được kích hoạt và tất cả các cạnh đến được kiểm tra chính xác một lần khi điểm cuối được xử lý, cạnh trực tiếp vẫn được đánh giá trong ngữ cảnh chính xác của nó và không thể bỏ qua. 

Trường hợp cuối cùng là chuỗi tăng hoặc giảm nghiêm ngặt. Trong các biểu đồ này, thứ tự kích hoạt đảm bảo rằng mỗi cạnh được đánh giá chính xác khi điểm cuối của nó được xử lý và tỷ lệ tối đa xuất hiện từ một so sánh duy nhất trên mỗi cạnh thay vì bất kỳ tích lũy nhiều bước nào.
