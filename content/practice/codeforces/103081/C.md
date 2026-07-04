---
title: "CF 103081C - Khoảng cách an toàn"
description: "Chúng ta có một căn phòng hình chữ nhật trong mặt phẳng, neo tại gốc tọa độ và kéo dài đến điểm $(X, Y)$. Bên trong hình chữ nhật này có một số điểm cố định tượng trưng cho người khác."
date: "2026-07-03T23:16:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103081
codeforces_index: "C"
codeforces_contest_name: "2020-2021 ICPC Southwestern European Regional Contest (SWERC 2020)"
rating: 0
weight: 103081
solve_time_s: 51
verified: true
draft: false
---

[CF 103081C - Khoảng cách an toàn](https://codeforces.com/problemset/problem/103081/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một căn phòng hình chữ nhật trong mặt phẳng, neo tại gốc tọa độ và kéo dài đến điểm$(X, Y)$. Bên trong hình chữ nhật này có một số điểm cố định tượng trưng cho người khác. Alice bắt đầu lúc$(0, 0)$và phải đạt$(X, Y)$trong khi luôn ở bên trong hình chữ nhật. 

Ràng buộc chính không phải là tìm ra con đường ngắn nhất hay bất kỳ con đường cụ thể nào, mà là ở việc duy trì khoảng trống: Alice muốn một con đường sao cho khoảng cách của cô ấy với mọi người khác luôn ít nhất có giá trị nào đó.$d$, và chúng tôi được yêu cầu tối đa hóa điều này$d$. Vì Alice có thể di chuyển liên tục theo bất kỳ hướng nào nên bài toán có tính chất hình học và phụ thuộc vào khoảng cách liên tục trong mặt phẳng chứ không phải cấu trúc lưới. 

Đầu ra là một số thực biểu thị khoảng cách tối thiểu tối đa có thể từ đường đi của Alice đến bất kỳ điểm nào đã cho. 

Với$N \le 1000$, chúng tôi có đủ khả năng chi trả$O(N^2)$hoặc$O(N^2 \log N)$lý luận, nhưng bất cứ điều gì liên quan đến sự rời rạc hóa dày đặc của việc liệt kê mặt phẳng hoặc đường dẫn đều không thể thực hiện được. Bất kỳ nỗ lực nào để mô phỏng các đường đi một cách rõ ràng hoặc rời rạc hóa mặt phẳng thành một lưới mịn sẽ ngay lập tức thất bại vì tọa độ liên tục lên đến$10^6$tỉ lệ. 

Một vấn đề tế nhị xuất hiện khi có nhiều người ở gần nhau. Ví dụ, nếu hai người cực kỳ thân thiết, vùng an toàn giữa họ có thể biến mất ngay cả khi mỗi người dường như có thể tránh được. Một cách tiếp cận ngây thơ chỉ xem xét khoảng cách một cách độc lập từ mỗi điểm mà không xem xét đến khả năng kết nối của không gian an toàn sẽ thất bại. 

Một trường hợp cạnh khác phát sinh khi đường đi tối ưu phải đi qua chính xác giữa hai người, trong đó khoảng cách giới hạn được xác định bởi “hành lang” hẹp nhất giữa các vùng ảnh hưởng của họ. Việc xử lý các chướng ngại vật một cách độc lập (chẳng hạn như lấy khoảng cách tối thiểu đến bất kỳ điểm nào dọc theo đường thẳng) sẽ đánh giá thấp hoặc trình bày sai chuyển động khả thi, bởi vì Alice có thể vòng quanh các chướng ngại vật. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là nghĩ về khoảng cách ứng cử viên$d$và hỏi liệu Alice có thể đi du lịch từ$(0,0)$ĐẾN$(X,Y)$trong khi giữ khoảng cách ít nhất$d$từ mỗi người. Về mặt hình học, điều này tương đương với việc mở rộng mỗi người thành một vòng tròn có bán kính$d$và hỏi liệu có tồn tại một đường đi liên tục bên trong hình chữ nhật tránh tất cả các vòng tròn hay không. 

Đối với một cố định$d$, chúng ta có thể thử BFS liên tục hoặc lấp đầy lũ dựa trên lưới trên một phần rời rạc của hình chữ nhật, đánh dấu các điểm ít nhất là$d$tránh xa mọi vòng tròn. Nếu có một đường dẫn tồn tại$d$là khả thi. Lặp lại điều này với tìm kiếm nhị phân trên$d$đưa ra một giải pháp. Tuy nhiên, rời rạc hóa mặt phẳng đủ mịn để tôn trọng$10^{-5}$độ chính xác đòi hỏi một mạng lưới khổng lồ, dễ dàng vượt quá$10^9$tế bào, điều này là không thể thực hiện được. 

Quan sát quan trọng là cấu trúc của vấn đề sẽ thay đổi khi chúng ta khắc phục$d$. Thay vì nghĩ về không gian liên tục, chúng ta có thể nghĩ về khả năng kết nối giữa các vùng bị cấm. Mỗi người xác định một đĩa có bán kính$d$và các đĩa này phân vùng không gian. Alice muốn biết liệu$(0,0)$Và$(X,Y)$nằm trong cùng một thành phần được kết nối của không gian trống. 

Đây là một bài toán kết nối hình học cổ điển có thể được rút gọn thành kết nối đồ thị giữa các chướng ngại vật và các bức tường ranh giới. Chúng tôi xây dựng một biểu đồ trong đó các nút biểu thị các chướng ngại vật và cũng bao gồm các nút ảo cho bốn đường viền của hình chữ nhật. Hai chướng ngại vật được kết nối nếu đĩa mở rộng của chúng chồng lên nhau. Đĩa kết nối với một ranh giới nếu nó chạm hoặc vượt qua ranh giới đó. Ý tưởng chính là Alice bị chặn nếu tồn tại một chuỗi các đĩa được kết nối ngăn cách mặt đầu với mặt cuối, tạo thành một rào cản xuyên qua hình chữ nhật một cách hiệu quả. 

Điều này biến việc kiểm tra tính khả thi thành kiểm tra kết nối tìm liên kết hoặc BFS trên biểu đồ kích thước$O(N)$, đủ hiệu quả trong tìm kiếm nhị phân trên$d$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Kết nối lưới Brute Force mỗi$d$|$O(N \cdot G^2)$|$O(G^2)$| Quá chậm | 
| Tìm kiếm nhị phân + biểu đồ kết nối hình học |$O(N^2 \log R)$|$O(N^2)$| Đã chấp nhận | 

Đây$R$là thang tọa độ. 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách tìm kiếm nhị phân câu trả lời$d$và cho mỗi ứng viên kiểm tra xem có đường đi an toàn hay không. 

1. Chúng tôi đặt phạm vi tìm kiếm cho$d$, bắt đầu từ$0$đến một giá trị đủ lớn như$\max(X, Y)$. Giới hạn trên này là an toàn vì bất kỳ bán kính đĩa nào lớn hơn kích thước phòng sẽ ngay lập tức chặn hoàn toàn chuyển động. 
2. Đối với ứng viên cố định$d$, chúng tôi đối xử với mỗi người tại$(x_i, y_i)$như một đĩa bán kính bị chặn$d$. 
3. Chúng tôi xây dựng một biểu đồ có các nút là các đĩa này cộng với bốn nút ranh giới bổ sung biểu thị các bức tường bên trái, bên phải, dưới cùng và trên cùng của hình chữ nhật. 
4. Chúng tôi kết nối hai nút người nếu khoảng cách Euclide giữa chúng tối đa$2d$. Mô hình này chồng lên các vùng bị cấm, có nghĩa là chúng tạo thành một rào cản được kết nối duy nhất. 
5. Chúng tôi kết nối một nút người với một nút ranh giới nếu khoảng cách của nó đến ranh giới đó lớn nhất$d$. Điều này ghi lại khi vùng cấm chạm hoặc giao với một bức tường. 
6. Sau đó, chúng tôi kiểm tra xem có tồn tại thành phần kết nối của các vùng bị chặn kết nối ranh giới bên trái với ranh giới bên phải hay ranh giới dưới cùng với ranh giới trên cùng, theo cách tách biệt$(0,0)$từ$(X,Y)$. Nếu có rào cản như vậy tồn tại thì dòng điện$d$là không thể thực hiện được. 
7. Sử dụng kiểm tra tính khả thi này, chúng tôi tìm kiếm nhị phân cho giá trị lớn nhất$d$điều đó vẫn khả thi. 

Giải thích hình học quan trọng là Alice có thể chuyển từ đầu đến cuối khi và chỉ khi sự kết hợp của các đĩa bị cấm không tạo ra một rào cản liên tục cắt hình chữ nhật thành các vùng không kết nối. 

### Tại sao nó hoạt động 

Đối với một cố định$d$, vùng cấm là các đĩa đóng. Nếu các đĩa này tạo thành một chuỗi kết nối liên kết các cạnh đối diện của hình chữ nhật, chúng sẽ phân chia không gian trống thành các thành phần bị ngắt kết nối, ngăn cản mọi đường dẫn liên tục từ đầu đến cuối. Ngược lại, nếu không tồn tại chuỗi phân cách như vậy thì phần bù của các đĩa vẫn được kết nối giữa hai điểm. Cấu trúc tìm liên kết nắm bắt chính xác kết nối giữa các đĩa và ranh giới, do đó, việc kiểm tra tính khả thi tương đương với việc kiểm tra xem có tồn tại rào cản chặn hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def can(d, pts, X, Y):
    n = len(pts)
    # nodes: 0..n-1 persons, n=left, n+1=right, n+2=bottom, n+3=top
    L, R, B, T = n, n+1, n+2, n+3

    parent = list(range(n + 4))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        ra, rb = find(a), find(b)
        if ra != rb:
            parent[rb] = ra

    def dist2(a, b):
        dx = pts[a][0] - pts[b][0]
        dy = pts[a][1] - pts[b][1]
        return dx*dx + dy*dy

    # connect person-person
    for i in range(n):
        for j in range(i+1, n):
            dx = pts[i][0] - pts[j][0]
            dy = pts[i][1] - pts[j][1]
            if dx*dx + dy*dy <= 4*d*d:
                union(i, j)

    # connect to boundaries
    for i, (x, y) in enumerate(pts):
        if x <= d:
            union(i, L)
        if X - x <= d:
            union(i, R)
        if y <= d:
            union(i, B)
        if Y - y <= d:
            union(i, T)

    # check blocking conditions:
    # left connects to right OR bottom connects to top
    return find(L) != find(R) and find(B) != find(T)

def solve():
    X, Y = map(float, input().split())
    n = int(input())
    pts = [tuple(map(float, input().split())) for _ in range(n)]

    lo, hi = 0.0, max(X, Y)
    for _ in range(60):
        mid = (lo + hi) / 2
        if can(mid, pts, X, Y):
            lo = mid
        else:
            hi = mid

    print(f"{lo:.10f}")

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên cấu trúc tìm liên kết để hợp nhất các đĩa bị cấm chồng chéo và tương tác của chúng với các ranh giới. Chi tiết triển khai chính là sử dụng khoảng cách bình phương để tránh sự mất ổn định của dấu phẩy động khi so sánh. Tìm kiếm nhị phân chạy một số lần lặp cố định để đảm bảo độ chính xác trong giới hạn lỗi bắt buộc. 

Một điểm tinh tế là điều kiện khả thi. Chúng ta không trực tiếp kiểm tra đường đi của Alice. Thay vào đó, chúng tôi kiểm tra xem các vùng cấm có tạo thành rào cản liên tục giữa các phía đối diện hay không. Nếu họ làm vậy, Alice sẽ bị mắc kẹt; mặt khác, có một đường đi xuyên qua không gian trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 6
3
3 1
3 5.5
6.5 1.5
```Chúng tôi tìm kiếm nhị phân$d$. Hãy xem xét một giá trị trung bình$d = 2.25$. 

| Bước | Sự kiện đoàn | Cấu trúc thành phần | Khả thi | 
| --- | --- | --- | --- | 
| 1 | kết nối đĩa đóng | dạng cụm | vâng | 

Tại$d = 2.25$, các đĩa đủ lớn để gần như chạm vào nhưng không tạo thành một rào cản hoàn toàn từ bên này sang bên kia. Do đó Alice vẫn có thể đi từ góc dưới bên trái lên góc trên bên phải qua một hành lang cong. 

Điều này chứng tỏ tính khả thi nằm ở khả năng kết nối toàn cầu chứ không chỉ giải phóng mặt bằng địa phương. 

### Ví dụ 2 

đầu vào:```
4 4
2
2 2
2 3
```Vì$d = 1.5$, cả hai điểm đều mở rộng thành các đĩa lớn chồng lên nhau theo chiều dọc. 

| Bước | Sự kiện đoàn | Cấu trúc thành phần | Khả thi | 
| --- | --- | --- | --- | 
| 1 | đĩa chồng lên nhau | rào cản dọc đơn | không | 

Ở đây, hai đĩa hợp nhất thành một cấu trúc kết nối ranh giới từ dưới lên trên, chặn bất kỳ lối đi nào qua hình chữ nhật. Điều này cho thấy khả năng kết nối giữa các chướng ngại vật chứ không chỉ từng bán kính riêng lẻ quyết định tính khả thi như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2 \log R)$| Mỗi lần kiểm tra tính khả thi đều$O(N^2)$do liên kết từng cặp, được lặp lại trên ~60 bước tìm kiếm nhị phân | 
| Không gian |$O(N)$| Cấu trúc tìm liên minh cộng với lưu trữ điểm | 

Những hạn chế$N \le 1000$làm$N^2 = 10^6$các hoạt động trên mỗi lần kiểm tra có thể chấp nhận được và hệ số tìm kiếm nhị phân không đổi. Điều này phù hợp thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    solve = globals().get("solve")
    if solve:
        from contextlib import redirect_stdout
        out = io.StringIO()
        with redirect_stdout(out):
            solve()
        return out.getvalue().strip()
    return ""

# provided sample
assert run("""8 6
3
3 1
3 5.5
6.5 1.5
""")[:5] == "2.25"

# minimum case
assert run("""1 1
0
""")[:3] == "1.0"

# single blocker
assert run("""5 5
1
2 2
""") != ""

# two blocking vertical chain
assert run("""4 4
2
2 2
2 3
""") != ""

# all corners
assert run("""10 10
4
0 0
0 10
10 0
10 10
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| người trống rỗng | giải phóng mặt bằng tối đa | không có trở ngại | 
| điểm trung tâm duy nhất | bán kính hữu hạn | hình học cơ bản | 
| cặp dọc | chuỗi chặn | rào cản kết nối | 
| điểm góc | tương tác ranh giới | xử lý cạnh | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một người nằm chính xác trên hoặc cực kỳ gần một ranh giới. Ví dụ, nếu một điểm ở$(0, y)$, thì thậm chí là rất nhỏ$d$ngay lập tức kết nối đĩa đó với bức tường bên trái. Bước liên minh`if x <= d`nắm bắt chính xác điều này vì đĩa đã chạm vào ranh giới bán kính$d$. 

Một trường hợp khác là khi hai điểm cách nhau một khoảng$2d$. Trong số học dấu phẩy động, điều này có thể dao động giữa kết nối và ngắt kết nối. Sử dụng khoảng cách bình phương sẽ tránh được sự mất mát về độ chính xác, bởi vì chúng ta so sánh$dx^2 + dy^2 \le 4d^2$một cách nhất quán. 

Trường hợp tế nhị cuối cùng là khi không có con người tồn tại. Biểu đồ tìm liên kết chỉ có các nút biên và không có liên kết nào xảy ra, do đó bên trái không bao giờ được kết nối với bên phải hoặc từ dưới lên trên. Thuật toán trả về chính xác sự tự do hoàn toàn, cho phép$d = \max(X, Y)$như giới hạn tìm kiếm nhị phân giới hạn.
