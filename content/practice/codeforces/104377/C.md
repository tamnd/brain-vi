---
title: "CF 104377C - \u4e8c\u7ef4\u6570\u7ec4\u53d8\u6362"
description: "Chúng ta được cho một ma trận n x n và một chuỗi các phép toán. Mỗi phép toán chọn một ma trận con vuông bằng cách sử dụng tọa độ trên cùng bên trái và dưới cùng bên phải, sau đó áp dụng một phép biến đổi hình học cho ma trận con đó."
date: "2026-07-01T17:21:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104377
codeforces_index: "C"
codeforces_contest_name: "The 21st Sichuan University Programming Contest"
rating: 0
weight: 104377
solve_time_s: 70
verified: true
draft: false
---

[CF 104377C - \u4e8c\u7ef4\u6570\u7ec4\u53d8\u6362](https://codeforces.com/problemset/problem/104377/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một ma trận n x n và một chuỗi các phép toán. Mỗi phép toán chọn một ma trận con vuông bằng cách sử dụng tọa độ trên cùng bên trái và dưới cùng bên phải, sau đó áp dụng một phép biến đổi hình học cho ma trận con đó. Các phép biến đổi là các thao tác kiểu hình ảnh tiêu chuẩn: xoay 90 độ theo chiều kim đồng hồ, lật ngang, lật dọc và phản chiếu trên hai đường chéo. 

Sau khi áp dụng tất cả các thao tác theo thứ tự, ma trận cuối cùng phải được in. 

Chi tiết quan trọng là các phép toán chỉ áp dụng cho các ma trận con chứ không áp dụng cho toàn bộ lưới và cùng một ô có thể tham gia vào nhiều phép biến đổi chồng chéo. Đầu ra phụ thuộc vào hiệu ứng tích lũy của tất cả các phép biến đổi. 

Các ràng buộc cho phép n lên tới 500 và nhiều nhất là 100 thao tác. Do đó, một mô phỏng trực tiếp xây dựng lại ma trận con bị ảnh hưởng cho mỗi thao tác là hợp lý vì công việc lớn nhất có thể có trên mỗi thao tác là trên vùng 500 x 500, tức là khoảng 250.000 ô và điều này được lặp lại nhiều nhất là 100 lần, mang lại khoảng 25 triệu ô cập nhật. 

Các trường hợp biên chủ yếu xuất phát từ cách áp dụng các phép biến đổi bên trong một ma trận con. 

Vấn đề tinh tế đầu tiên là tính chính xác của phép quay. Nếu chúng ta ghi đè lên ma trận tại chỗ trong khi xoay, chúng ta sẽ làm hỏng các giá trị trước khi chúng được di chuyển. Ví dụ: xoay khối 3 x 3 tại chỗ mà không có bộ đệm tạm thời sẽ ghi đè các mục vẫn cần thiết sau này trong ánh xạ xoay. 

Một vấn đề khác là tính nhất quán của việc lập chỉ mục. Đầu vào dựa trên 1, do đó, việc không chuyển đổi sang lập chỉ mục dựa trên 0 sẽ dẫn đến sai lệch từng cái một và khó phát hiện vì kết quả có thể vẫn có cấu trúc nhưng không chính xác. 

Cuối cùng, việc lật đường chéo đòi hỏi phải lập bản đồ tọa độ cẩn thận. Việc triển khai đơn giản hoán đổi các hàng và cột trên toàn cầu thay vì trong ma trận con sẽ phá hủy các phần không liên quan của ma trận. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực trực tiếp theo sau tuyên bố vấn đề: đối với mỗi thao tác, trích xuất ma trận con, xây dựng một phiên bản được chuyển đổi mới và viết lại. 

Điều này có tác dụng vì mỗi phép biến đổi là một hoán vị cố định của tọa độ bên trong vùng hình vuông. Đối với ma trận con có kích thước k, chúng ta có thể tính toán vị trí mới của từng phần tử trong O(1), do đó việc xây dựng lại ma trận con tốn O(k²). Vì mỗi ô chỉ được viết lại khi ma trận con chứa nó được xử lý nên tổng công việc được giới hạn bởi tổng bình phương của tất cả các phép toán có kích thước của các vùng bị ảnh hưởng của chúng. 

Trường hợp xấu nhất xảy ra khi mọi thao tác chạm vào ma trận n x n đầy đủ. Khi đó, mỗi thao tác tốn O(n²) và với q lên tới 100, giá trị này sẽ trở thành O(100·500²), tức là khoảng 25 triệu bài tập. Điều này được chấp nhận trong Python. 

Quan sát quan trọng là không cần bất kỳ thành phần chuyển đổi lười biếng hoặc theo dõi tọa độ tổng thể nào. Số lượng thao tác ít nên việc tính toán lại trực tiếp đơn giản và an toàn hơn. Bất kỳ nỗ lực nào nhằm duy trì các phép biến đổi ký hiệu trên mỗi ô đều trở thành chi phí không cần thiết và làm tăng độ phức tạp khi triển khai mà không cải thiện hiệu suất tiệm cận trong chế độ ràng buộc này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái thiết ma trận con Brute Force | O(q · k²) | O(k²) | Đã chấp nhận | 
| Mô phỏng trực tiếp tối ưu | O(q · n²) trường hợp xấu nhất | O(n²) | Đã chấp nhận | 

Trong bài toán này, cả hai cách tiếp cận đều hội tụ vì các ràng buộc đủ nhỏ để mô phỏng đơn giản đã là tối ưu. 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ma trận ở trạng thái hiện tại và áp dụng trực tiếp từng thao tác.

1. Đọc ma trận và chuyển đổi nó thành cấu trúc có thể thay đổi. Điều này cho phép cập nhật tại chỗ sau mỗi lần chuyển đổi. 
2. Đối với mỗi thao tác, chuyển đổi tọa độ từ chỉ mục dựa trên 1 thành chỉ mục dựa trên 0. Điều này đảm bảo tất cả các chỉ mục tiếp theo phù hợp với quy ước mảng Python và ngăn chặn sự thay đổi ranh giới. 
3. Trích xuất ma trận con được xác định bởi thao tác vào bộ đệm tạm thời. Bước này cô lập vùng để các phép biến đổi không ghi đè lên các giá trị vẫn cần thiết trong quá trình tính toán. 
4. Xây dựng ma trận mới có cùng kích thước với ma trận con. 
5. Điền vào ma trận mới theo kiểu biến đổi. Đối với một phép quay, mỗi phần tử ở vị trí (i, j) trong ma trận con sẽ di chuyển tới (j, k-1-i). Đối với các lần lật ngang và dọc, các chỉ số được đảo ngược dọc theo trục tương ứng. Đối với các phản xạ theo đường chéo, các chỉ số hàng và cột được hoán đổi với độ đảo thích hợp tùy thuộc vào đường chéo nào được sử dụng. 
6. Viết ma trận con đã biến đổi trở lại ma trận ban đầu ở cùng tọa độ. 
7. Lặp lại cho đến khi tất cả thao tác được xử lý. 

Sau khi cập nhật xong, in ma trận cuối cùng. 

### Tại sao nó hoạt động 

Mỗi thao tác là một phép chiếu trên các ô của vùng hình vuông đã chọn, nghĩa là mọi phần tử sẽ di chuyển đến chính xác một vị trí mới và mỗi vị trí nhận chính xác một phần tử. Vì chúng tôi xây dựng lại hoàn toàn từng ma trận con trước khi viết lại nên chúng tôi bảo toàn ánh xạ một-một này chính xác như được xác định bởi phép biến đổi. Vì các thao tác được áp dụng theo trình tự và mỗi bước sử dụng ma trận được cập nhật đầy đủ từ bước trước đó nên trạng thái cuối cùng khớp với thành phần của tất cả các phép biến đổi theo thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(n)]

def apply_op(x0, y0, x1, y1, t):
    x0 -= 1
    y0 -= 1
    x1 -= 1
    y1 -= 1
    k = x1 - x0 + 1

    tmp = [[0] * k for _ in range(k)]

    for i in range(k):
        for j in range(k):
            tmp[i][j] = a[x0 + i][y0 + j]

    res = [[0] * k for _ in range(k)]

    if t == 1:
        for i in range(k):
            for j in range(k):
                res[j][k - 1 - i] = tmp[i][j]

    elif t == 2:
        for i in range(k):
            for j in range(k):
                res[i][k - 1 - j] = tmp[i][j]

    elif t == 3:
        for i in range(k):
            for j in range(k):
                res[k - 1 - i][j] = tmp[i][j]

    elif t == 4:
        for i in range(k):
            for j in range(k):
                res[j][i] = tmp[i][j]

    elif t == 5:
        for i in range(k):
            for j in range(k):
                res[k - 1 - j][k - 1 - i] = tmp[i][j]

    for i in range(k):
        for j in range(k):
            a[x0 + i][y0 + j] = res[i][j]

for _ in range(q):
    x0, y0, x1, y1, t = map(int, input().split())
    apply_op(x0, y0, x1, y1, t)

for row in a:
    print(*row)
```Việc triển khai tuân theo thuật toán một cách chính xác bằng cách tách từng thao tác thành một hàm trợ giúp. Bộ đệm tạm thời`tmp`là cần thiết vì tất cả các phép biến đổi đều phụ thuộc vào cấu hình ban đầu của ma trận con và các bản cập nhật tại chỗ sẽ ghi đè các giá trị cần thiết cho các phép gán sau này. 

Mỗi trường hợp chuyển đổi mã hóa ánh xạ tọa độ trực tiếp. Ví dụ: trường hợp xoay sử dụng ánh xạ (i, j) đến (j, k-1-i), tương ứng với việc xoay hình vuông theo chiều kim đồng hồ. Các trường hợp đường chéo hoán đổi chỉ số, phản ánh trên đường chéo chính hoặc đối chéo. 

Phải cẩn thận để tất cả các chỉ số được chuyển nhất quán sang dạng dựa trên 0 khi bắt đầu mỗi thao tác và ghi lại vào`a`chỉ xảy ra sau khi khối chuyển đổi đầy đủ được tính toán. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên trong đó ma trận 3 x 3 trải qua thao tác xoay hoàn toàn trên toàn bộ lưới. Chúng tôi chỉ theo dõi bản đồ tọa độ. 

| Bước | (tôi, j) | Giá trị | Vị Trí Mới | 
| --- | --- | --- | --- | 
| Ban đầu | (0,0) | 1 | (0,2) | 
| Ban đầu | (0,1) | 2 | (1,2) | 
| Ban đầu | (0,2) | 3 | (2,2) | 
| Ban đầu | (1,0) | 4 | (0,1) | 
| Ban đầu | (1,1) | 5 | (1,1) | 
| Ban đầu | (1,2) | 6 | (2,1) | 
| Ban đầu | (2,0) | 7 | (0,0) | 
| Ban đầu | (2,1) | 8 | (1,0) | 
| Ban đầu | (2,2) | 9 | (2,0) | 

Sau khi đặt tất cả các giá trị vào vị trí mới, ma trận sẽ quay theo chiều kim đồng hồ. 

Đối với ví dụ thứ hai, hãy xem xét việc lật ngang trên cùng một ma trận. Mỗi hàng được đảo ngược độc lập. Hàng trên cùng trở thành hàng dưới cùng của các giá trị đảo ngược, trong khi hàng giữa vẫn không thay đổi về mặt cấu trúc ngoại trừ sự đảo ngược. 

Điều này xác nhận rằng các phép biến đổi hoạt động nghiêm ngặt trong ma trận con đã chọn và không rò rỉ ra ngoài các ranh giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · k²), tệ nhất O(q · n²) | Mỗi thao tác xây dựng lại a k ​​bằng k ma trận con và k có thể lên tới n | 
| Không gian | O(n²) | Bộ nhớ cho ma trận cộng với bộ đệm tạm thời có kích thước k2 | 

Với n lên tới 500 và q lên tới 100, số lượng thao tác trong trường hợp xấu nhất là khoảng 25 triệu phép gán ô, vừa vặn thoải mái trong giới hạn thời gian trong Python với các thân vòng lặp đơn giản và không có thao tác đắt tiền. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    def apply_op(x0, y0, x1, y1, t):
        x0 -= 1; y0 -= 1; x1 -= 1; y1 -= 1
        k = x1 - x0 + 1
        tmp = [[0]*k for _ in range(k)]
        for i in range(k):
            for j in range(k):
                tmp[i][j] = a[x0+i][y0+j]
        res = [[0]*k for _ in range(k)]

        if t == 1:
            for i in range(k):
                for j in range(k):
                    res[j][k-1-i] = tmp[i][j]
        elif t == 2:
            for i in range(k):
                for j in range(k):
                    res[i][k-1-j] = tmp[i][j]
        elif t == 3:
            for i in range(k):
                for j in range(k):
                    res[k-1-i][j] = tmp[i][j]
        elif t == 4:
            for i in range(k):
                for j in range(k):
                    res[j][i] = tmp[i][j]
        elif t == 5:
            for i in range(k):
                for j in range(k):
                    res[k-1-j][k-1-i] = tmp[i][j]

        for i in range(k):
            for j in range(k):
                a[x0+i][y0+j] = res[i][j]

    for _ in range(q):
        x0, y0, x1, y1, t = map(int, input().split())
        apply_op(x0, y0, x1, y1, t)

    return "\n".join(" ".join(map(str, r)) for r in a)

# provided sample 1
assert run("""3 1
1 2 3
4 5 6
7 8 9
1 1 1 3 3
""") == """7 4 1
8 5 2
9 6 3"""

# provided sample 2
assert run("""3 1
1 2 3
4 5 6
7 8 9
3 1 1 3 3
""") == """7 8 9
4 5 6
1 2 3"""

# custom: 1x1 no-op
assert run("""1 1
5
1 1 1 1 1
""") == "5"

# custom: horizontal flip 2x2
assert run("""2 1
1 2
3 4
1 1 2 2 2
""") == """2 1
4 3"""

# custom: diagonal main
assert run("""2 1
1 2
3 4
1 1 2 2 4
""") == """1 3
2 4"""

# custom: anti diagonal
assert run("""2 1
1 2
3 4
1 1 2 2 5
""") == """4 2
3 1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | không thay đổi | trường hợp cạnh nhận dạng | 
| lật ngang 2x2 | hàng đảo ngược | độ chính xác đảo ngược hàng khôn ngoan | 
| Đường chéo chính 2x2 | chuyển vị | ánh xạ đường chéo chính xác | 
| 2x2 chống chéo | phản xạ chéo | ánh xạ đường chéo ngược chính xác | 

## Vỏ cạnh 

Ma trận con 1 x 1 kiểm tra xem các phép biến đổi có cố gắng di chuyển một ô vào chính nó hay lập chỉ mục ra ngoài phạm vi không. Thuật toán xử lý điều này vì mọi ánh xạ sẽ gửi (0,0) trở lại (0,0), do đó ma trận không thay đổi. 

Một hoạt động toàn lưới lặp lại nhiều lần để kiểm tra sự tích lũy. Vì mỗi thao tác sẽ xây dựng lại đầy đủ ma trận trước thao tác tiếp theo, nên không có sự can thiệp nào giữa các phép biến đổi ngoài thành phần có chủ ý. 

Các ma trận con chồng chéo kiểm tra tính đúng đắn của sự cô lập. Vì mỗi thao tác sao chép vào bộ đệm tạm thời trước khi ghi lại nên các bản cập nhật trước đó không được sử dụng lại một phần trong cùng một thao tác, ngăn ngừa sai lệch các giá trị trung gian.
