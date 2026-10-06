---
title: "CF 104925H - Luồng chi phí tối thiểu\u00b2"
description: "Chúng ta được cung cấp một đồ thị có hướng với nút nguồn và nút đích được chỉ định. Thay vì chọn một tập hợp các đường dẫn hoặc luồng số nguyên rời rạc, chúng tôi gán một luồng có giá trị thực cho mọi cạnh, có thể âm, miễn là việc bảo toàn luồng giữ nguyên ở mọi đỉnh và luồng thực từ nguồn…"
date: "2026-06-28T07:54:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104925
codeforces_index: "H"
codeforces_contest_name: "Osijek Competitive Programming Camp, Fall 2023. Day 6: Estonian Contest (The 2nd Universal Cup. Stage 19: Estonia)"
rating: 0
weight: 104925
solve_time_s: 35
verified: true
draft: false
---

[CF 104925H - Luồng chi phí tối thiểu\u00b2](https://codeforces.com/problemset/problem/104925/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng với nút nguồn và nút đích được chỉ định. Thay vì chọn một tập hợp các đường đi hoặc các luồng số nguyên rời rạc, chúng ta gán một luồng có giá trị thực cho mọi cạnh, có thể âm, miễn là việc bảo toàn luồng giữ nguyên ở mọi đỉnh và luồng thực từ nguồn đến đích bằng một đơn vị. 

Mô hình chi phí là sự khởi đầu quan trọng từ dòng chi phí tối thiểu cổ điển. Mỗi cạnh đóng góp một chi phí bằng bình phương của luồng trên cạnh đó nhân với hệ số dương. Vì vậy nếu một cạnh mang dòng chảy$f_e$, nó góp phần$c_e f_e^2$đến tổng chi phí. Mục tiêu là định tuyến chính xác một đơn vị luồng từ nút 1 đến nút n trong khi giảm thiểu tổng chi phí cạnh bậc hai này. 

Biểu đồ có tối đa 100 đỉnh và 300 cạnh cho mỗi lần kiểm tra và nhiều trường hợp kiểm tra có kích thước kết hợp nhỏ. Ý nghĩa quan trọng là bất cứ thứ gì có số đỉnh đều có khả năng được chấp nhận, nhưng bất cứ thứ gì liên quan đến việc liệt kê tổ hợp các luồng hoặc đường đi đều không khả thi ngay lập tức. Sự hiện diện của luồng có giá trị thực và tín hiệu khách quan lồi chặt chẽ rằng giải pháp sẽ đến từ việc tối ưu hóa liên tục chứ không phải tổ hợp. 

Một khía cạnh tinh tế là các luồng được phép âm, có nghĩa là chúng ta không bị hạn chế đối với khái niệm định tuyến theo chu kỳ có hướng. Điều này biến bài toán thành một bài toán tối ưu bậc hai đối xứng trên một không gian con tuyến tính được xác định bằng bảo toàn dòng. 

Một sai lầm ngây thơ là nghĩ đến việc chia luồng đơn vị thành các đường dẫn và giảm thiểu tổng trên các đường dẫn. Điều đó không thành công vì việc phân tách tương tác phi tuyến với chi phí cạnh bình phương. 

Trong trường hợp lỗi cụ thể, hãy xem xét một biểu đồ có hai cạnh song song từ 1 đến 2, cả hai đều có chi phí 1. Gửi tất cả luồng qua một cạnh sẽ mang lại chi phí$1$. Việc chia đều sẽ tạo ra dòng chảy cho mỗi cạnh$1/2$, do đó chi phí trở thành$2 \cdot (1/4) = 1/2$, thực sự tốt hơn. Bất kỳ lý do dựa trên đường dẫn hoặc không thể phân chia nào cũng sẽ đề xuất sai chi phí 1 là tối ưu. 

Một cạm bẫy phổ biến khác là giả định tính tuyến tính của các luồng tối ưu. Nếu chi phí là tuyến tính thì giải pháp sẽ là con đường ngắn nhất. Ở đây, tính lồi đẩy dòng chảy trải rộng trên nhiều tuyến đường, làm thay đổi cơ bản cấu trúc. 

## Phương pháp tiếp cận 

Quan điểm brute-force là xử lý từng luồng cạnh$f_e$như một biến và giải một chương trình bậc hai bị ràng buộc với$m$biến và$n-1$ràng buộc tuyến tính. Viết nó trực tiếp cho một sự giảm thiểu bậc hai lồi:$$\min \sum c_e f_e^2 \quad \text{subject to } Af = b$$Ở đâu$A$mã hóa bảo toàn dòng chảy. 

Một giải pháp số trực tiếp sẽ cố gắng loại bỏ các ràng buộc hoặc áp dụng các bộ giải lồi tổng quát. Tuy nhiên, việc loại bỏ Gaussian tổng quát trên hệ thống KKT đầy đủ có kích thước xấp xỉ$m + n$, dẫn đến$O((n+m)^3)$thời gian cho mỗi lần kiểm tra. Với tổng số 400 đỉnh và cạnh trong các thử nghiệm, đây vẫn là giới hạn nhưng chỉ có thể chấp nhận được nếu được triển khai cẩn thận và khai thác cấu trúc tái sử dụng. 

Quan sát quan trọng là vật kính có đường chéo trong không gian cạnh, nghĩa là không có số hạng chéo giữa các cạnh. Điều này làm cho hệ thống KKT trở nên thưa thớt và có cấu trúc: nó tương ứng chính xác với hệ thống Laplacian có trọng số trên biểu đồ. Điều này biến bài toán thành giải một hệ tuyến tính duy nhất rút ra từ các định luật kiểu Kirchhoff, trong đó các trọng số của các cạnh hoạt động giống như các điện trở.$1/c_e$. 

Sau khi được diễn giải lại về mặt vật lý, mỗi cạnh hoạt động giống như một điện trở có độ dẫn tỷ lệ với$c_e$, và chúng ta bơm một đơn vị dòng điện từ nguồn tới bồn. Luồng tối ưu chính xác là dòng điện và năng lượng tối thiểu bằng điện trở hiệu dụng giữa các nút 1 và n, được biểu thị dưới dạng trọng số kép. 

Do đó, bài toán giảm xuống còn việc giải hệ tuyến tính Laplacian và rút ra hiệu điện thế gây ra bởi phép tiêm đơn vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lập trình bậc hai trực tiếp |$O((n+m)^3)$|$O(n^2)$| Quá chậm | 
| Giảm hệ thống Laplacian / tuyến tính |$O(n^3)$mỗi bài kiểm tra |$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại các biến dòng chảy bằng cách sử dụng điện thế nút. Mục tiêu bậc hai lồi ngụ ý rằng tính tối ưu thỏa mãn các điều kiện bậc nhất: mỗi luồng cạnh tỷ lệ thuận với hiệu điện thế giữa các điểm cuối của nó. 

### 1. Giới thiệu tiềm năng của nút 

Chỉ định một tiềm năng$x_u$tới mỗi đỉnh. Đối với một cạnh$u \to v$, sự tối ưu buộc dòng chảy phải thỏa mãn$$f_{uv} = \frac{x_u - x_v}{2c_{uv}}.$$Điều này xuất phát từ việc giảm thiểu$c_e f_e^2$liên quan đến$f_e$theo hệ số nhân Lagrange liên quan đến các hạn chế bảo tồn. 

yếu tố$2$là chuẩn hóa modulo không liên quan và sẽ được hấp thụ sau. 

### 2. Thay thế vào định luật bảo toàn 

Tại mọi đỉnh ngoại trừ đỉnh nguồn và đỉnh đích, sự bảo toàn dòng chảy trở thành:$$\sum_{(u,v)} \frac{x_u - x_v}{c_{uv}} = 0.$$Đây chính xác là một phương trình Laplacian có trọng số. 

### 3. Xây dựng hệ thống Laplacian 

Xây dựng ma trận$L$mỗi cạnh ở đâu$u-v$chúng tôi thêm trọng lượng$w = 1/c$. Sau đó:

-$L[u][u] += w$-$L[v][v] += w$-$L[u][v] -= w$-$L[v][u] -= w$Sau đó chúng tôi sửa:$$x_n = 0$$và thực thi việc chèn luồng đơn vị tại nút 1, nút này trở thành vectơ bên phải với$b_1 = 1$,$b_n = -1$. 

### 4. Giải hệ tuyến tính 

Giải quyết$L' x = b$sau khi loại bỏ một hàng và cột (bồn rửa được nối đất). Việc loại bỏ Gaussian trên số học mô-đun mang lại tiềm năng. 

### 5. Tính đáp số 

Khi đã biết thế năng thì năng lượng bằng:$$\sum c_e f_e^2 = \sum \frac{(x_u - x_v)^2}{4c_e}.$$Tính toán điều này trực tiếp trên tất cả các cạnh. 

### Tại sao nó hoạt động 

Mục tiêu là lồi hoàn toàn và các ràng buộc là tuyến tính nên điều kiện KKT vừa cần vừa đủ. Các điều kiện dừng chuyển đổi các hình phạt bậc hai thành các mối quan hệ tuyến tính giữa dòng chảy và tiềm năng nút. Điều này làm sụp đổ hệ thống thành cấu trúc Laplacian, đặc trưng duy nhất cho dòng chảy tối ưu. Do hệ thống Laplacian có một nghiệm duy nhất khi nút tham chiếu được cố định, nên điện thế được tính toán tương ứng chính xác với bộ giảm thiểu toàn cục và luồng kết quả đáp ứng mọi ràng buộc trong khi giảm thiểu năng lượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve_case(n, edges):
    # Build Laplacian
    # We solve Lx = b with x[n-1] = 0
    size = n - 1
    L = [[0] * size for _ in range(size)]
    b = [0] * size

    def add_edge(u, v, c):
        w = 1 * modinv(c) % MOD
        if u != n:
            L[u-1][u-1] = (L[u-1][u-1] + w) % MOD
        if v != n:
            L[v-1][v-1] = (L[v-1][v-1] + w) % MOD
        if u != n and v != n:
            L[u-1][v-1] = (L[u-1][v-1] - w) % MOD
            L[v-1][u-1] = (L[v-1][u-1] - w) % MOD

    for u, v, c in edges:
        add_edge(u, v, c)

    # inject 1 unit flow at source
    b[0] = 1

    # Gaussian elimination
    for i in range(size):
        pivot = i
        for j in range(i, size):
            if L[j][i]:
                pivot = j
                break
        L[i], L[pivot] = L[pivot], L[i]
        b[i], b[pivot] = b[pivot], b[i]

        inv = modinv(L[i][i])
        for j in range(i, size):
            L[i][j] = L[i][j] * inv % MOD
        b[i] = b[i] * inv % MOD

        for j in range(size):
            if j != i and L[j][i]:
                factor = L[j][i]
                for k in range(i, size):
                    L[j][k] = (L[j][k] - factor * L[i][k]) % MOD
                b[j] = (b[j] - factor * b[i]) %*
```
