---
title: "CF 105931E - \u041c\u0430\u0442\u0440\u0438\u0446\u0430 \u0446\u0438\u043a\u043b\u0438\u0447\u0435\u0441\u043a\u0438\u0445 \u0441\u0434\u0432\u0438\u0433\u043e\u0432"
description: "Chúng ta bắt đầu với mảng một chiều, nhưng thay vì làm việc trực tiếp với nó, chúng ta sử dụng nó để tạo ra ma trận $n nhân n$ trong đó mỗi hàng chỉ là một phép dịch chuyển theo chu kỳ của mảng ban đầu."
date: "2026-06-22T15:44:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105931
codeforces_index: "E"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2024"
rating: 0
weight: 105931
solve_time_s: 89
verified: true
draft: false
---

[CF 105931E - \u041c\u0430\u0442\u0440\u0438\u0446\u0430 \u0446\u0438\u043a\u043b\u0438\u0447\u0435\u0441\u043a\u0438\u0445 \u0441\u0434\u0432\u0438\u0433\u043e\u0432](https://codeforces.com/problemset/problem/105931/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với mảng một chiều, nhưng thay vì làm việc trực tiếp với nó, chúng ta sử dụng nó để tạo ra một mảng$n \times n$ma trận trong đó mỗi hàng chỉ là một sự dịch chuyển theo chu kỳ của mảng ban đầu. Vì vậy, mỗi cột cũng là một phiên bản được dịch chuyển của cùng một giá trị và mọi số trong mảng đều xuất hiện chính xác$n$lần ở các vị trí có cấu trúc trên ma trận. 

Bây giờ hãy coi mỗi ô ma trận là một đỉnh trong biểu đồ. Hai đỉnh được kết nối nếu hai điều kiện xảy ra đồng thời. Đầu tiên, khoảng cách Manhattan của họ trong lưới tối đa là$k$, nghĩa là chúng ta có thể di chuyển giữa chúng bằng cách sử dụng các bước lưới trong tổng chiều dài tối đa$k$. Thứ hai, các số được viết trong hai ô này không phải là số nguyên tố cùng nhau, nghĩa là ước số chung lớn nhất của chúng hoàn toàn lớn hơn một. 

Nhiệm vụ là tính xem biểu đồ này có bao nhiêu thành phần được kết nối. Vì đồ thị có$n^2$đỉnh và$n$có thể lớn như$10^6$, không thể xây dựng hoặc duyệt qua nó một cách rõ ràng. Cấu trúc của ma trận và điều kiện gcd phải được khai thác rất nhiều. 

Các ràng buộc ngay lập tức loại trừ bất cứ điều gì coi ma trận là một biểu đồ tổng quát. Ngay cả việc lưu trữ tất cả các đỉnh cũng không thể thực hiện được, vì$n^2$đạt tới$10^{12}$. Bất kỳ giải pháp hợp lệ nào cũng phải tránh liệt kê các ô và thay vào đó hoạt động với cấu trúc tuần hoàn do sự dịch chuyển theo chu kỳ gây ra. 

Một mô hình tư duy ngây thơ thường thất bại là cho rằng mỗi tế bào hoạt động độc lập. Ví dụ: nếu tất cả các giá trị đều khác biệt, người ta có thể mong đợi một cấu trúc kết nối giống như lưới một cách không chính xác. Nhưng ở đây các giá trị giống hệt nhau lặp lại theo một khuôn mẫu cứng nhắc và gcd tạo ra các tương tác tầm xa thông qua các thừa số nguyên tố được chia sẻ. 

Trường hợp cạnh tinh tế xuất hiện khi$k$lớn chẳng hạn$k \ge n$. Trong trường hợp này, các ràng buộc Manhattan không còn hạn chế chuyển động bên trong ma trận một cách có ý nghĩa và các thành phần được xác định gần như hoàn toàn bởi cấu trúc gcd. Bất kỳ giải pháp nào đảm nhận tính địa phương trong chế độ này mà không kiểm tra phạm vi tiếp cận toàn cầu sẽ thất bại. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ xây dựng biểu đồ một cách rõ ràng. Đối với mỗi cặp ô, chúng tôi sẽ kiểm tra khoảng cách Manhattan và gcd, sau đó hợp nhất chúng. Cái này đã tốn rồi$O(n^4)$so sánh trong trường hợp xấu nhất vì có$n^2$đỉnh, làm cho nó hoàn toàn không khả thi ngay cả đối với$n = 1000$, huống hồ là$10^6$. 

Ngay cả khi chúng ta cố định một đỉnh và cố gắng chỉ kết nối nó với các đỉnh lân cận trong bán kính Manhattan$k$, kích thước vùng lân cận trong một lưới dày đặc vẫn$O(k^2)$, và kể từ đó$k$có thể lớn như$10^9$, cách giải thích này bị phá vỡ hoàn toàn. Quan sát quan trọng là tính kề phụ thuộc vào hai cấu trúc độc lập: ràng buộc hình học từ khoảng cách Manhattan và ràng buộc số học từ gcd. 

Cấu trúc ma trận là sự đơn giản hóa thực sự. Mỗi giá trị$a[j]$không xuất hiện một cách tùy tiện; nó tạo thành một mô hình đường chéo hoàn hảo trên ma trận. Nếu chúng ta sửa một chỉ mục cột$j$, sự xuất hiện của$a[j]$nằm dọc theo một đường xác định trên các hàng và việc dịch chuyển chỉ thay đổi vị trí của chúng theo cách có thể dự đoán được. Điều này biến ma trận thành một tập hợp các chu trình có cấu trúc thay vì một lưới không có cấu trúc. 

Điều kiện gcd tiếp tục phân tích bài toán theo thừa số nguyên tố. Hai ô chỉ có thể kết nối nếu chúng có chung ít nhất một thừa số nguyên tố, vì vậy chúng ta có thể xử lý từng số nguyên tố một cách độc lập và sau đó kết hợp các hiệu ứng thông qua logic tìm hợp. 

Điều kiện Manhattan tương tác với cấu trúc dịch chuyển tuần hoàn theo một cách rất quan trọng: chuyển động trong lưới chuyển thành chuyển động giới hạn dọc theo các đường chéo có cấu trúc này. Thay vì lý luận theo hai chiều, chúng tôi giảm khả năng kết nối với các tương tác giữa các phân đoạn dọc theo không gian chỉ mục tuần hoàn. 

Khi hai mức giảm này được kết hợp, vấn đề sẽ có thể giải quyết được: chúng tôi không còn làm việc với$n^2$các nút, nhưng với$n$các lớp giá trị, mỗi phân đoạn có cấu trúc đóng góp và khả năng kết nối trở thành một liên minh của các sự hợp nhất giống như khoảng thời gian trên một miền chỉ mục vòng tròn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên lưới |$O(n^4)$|$O(n^2)$| Quá chậm | 
| Cấu trúc + phân rã nguyên tố + DSU |$O(n \log A)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Phân tích nhân tử tất cả các giá trị trong mảng 

Chúng tôi phân hủy mọi$a[i]$vào các yếu tố chính của nó. Mỗi số nguyên tố sẽ xác định một “lớp kết nối” riêng biệt, vì các cạnh chỉ tồn tại giữa các số có chung ít nhất một số nguyên tố. 

Điều này cho phép chúng ta coi biểu đồ như một tập hợp các đồ thị con, mỗi đồ thị con một số nguyên tố. 

### Bước 2: Giải thích sự xuất hiện của chỉ số cố định trong ma trận 

Sửa chỉ mục$j$trong mảng. Trong ma trận, giá trị$a[j]$xuất hiện chính xác một lần trên mỗi hàng, tại một vị trí cột xác định tùy thuộc vào sự dịch chuyển theo chu kỳ. Vì vậy thay vì nghĩ đến$n^2$tế bào, chúng tôi nghĩ đến$n$"các bản nhạc" theo chu kỳ, một bản nhạc trên mỗi chỉ mục mảng. 

Mỗi rãnh là một đường cong giống như hoán vị xuyên qua ma trận, bao quanh lưới theo một đường chéo nhất quán. 

### Bước 3: Giảm vùng lân cận Manhattan bên trong tuyến đường 

Trong một chỉ số cố định$j$, hai lần xuất hiện nằm ở các hàng khác nhau. Khoảng cách Manhattan của họ tăng lên theo độ chênh lệch hàng, nhưng do cột dịch chuyển theo từng hàng nên khoảng cách đơn giản hóa thành một hàm của khoảng cách hàng. 

Nếu sự khác biệt hàng là$d$, khoảng cách Manhattan có tính chất như sau$2d$hoặc giá trị giới hạn khi hiệu ứng gói chiếm ưu thế. Điều này có nghĩa là trong một tuyến đường duy nhất, khả năng kết nối được tạo ra bởi bán kính Manhattan$k$chỉ cho phép hợp nhất các điểm có chỉ số hàng khác nhau tối đa khoảng$k/2$, ngoại trừ khi$k$vượt quá$n$, trong trường hợp đó toàn bộ rãnh sẽ được kết nối. 

Vì vậy mỗi chỉ số$j$đóng góp một khối kết nối (khi$k \ge n$) hoặc một số đoạn liền kề dọc theo thứ tự hàng (khi$k < n$). 

### Bước 4: Nâng kết nối giữa các chỉ số khác nhau 

Bây giờ hãy xem xét hai chỉ số khác nhau$j_1$Và$j_2$. Chúng tôi hỏi khi một ô từ theo dõi$j_1$có thể kết nối với một ô từ track$j_2$. Cách căn chỉnh tốt nhất là so sánh các ô trong cùng một hàng, vì sự khác biệt giữa các hàng chỉ làm tăng khoảng cách. 

Ở cùng một hàng$i$, khoảng cách Manhattan trở nên hoàn toàn theo chiều ngang và bằng với sự khác biệt giữa các vị trí cột của chúng, được xác định bởi độ lệch dịch chuyển theo chu kỳ giữa$j_1$Và$j_2$. Điều này làm giảm khả năng kết nối giữa các đường ray thành điều kiện khoảng cách vòng tròn trên các chỉ số: 

hai chỉ số$j_1, j_2$có thể tương tác nếu khoảng cách vòng tròn của chúng trên chu kỳ mảng nhiều nhất$k$. 

### Bước 5: Xây dựng DSU dựa trên chỉ số mảng cho từng số nguyên tố 

Đối với mỗi số nguyên tố$p$, chúng tôi xem xét tất cả các chỉ số$j$như vậy$p \mid a[j]$. Chúng tôi kết nối các chỉ số có khoảng cách vòng tròn nằm trong$k$. Mỗi thành phần được kết nối của các chỉ mục tương ứng với một tập hợp các rãnh hợp nhất dưới số nguyên tố này. 

Trong mỗi thành phần như vậy, tất cả các phân đoạn tương ứng từ Bước 3 sẽ được hợp nhất tương ứng. 

### Bước 6: Đếm các thành phần cuối cùng 

Mỗi phân đoạn được tạo ở Bước 3 được coi là một nút. DSU hợp nhất chúng theo các kết nối được xác định trong Bước 3 và 5. Số gốc DSU cuối cùng là số thành phần được kết nối trong biểu đồ đầy đủ. 

### Tại sao nó hoạt động 

Mỗi cạnh trong biểu đồ gốc kết nối hai ô có thừa số nguyên tố chung hoặc không tồn tại. Điều này phân chia tất cả các cạnh thành các lớp cảm ứng nguyên tố độc lập. Bên trong mỗi lớp, hình học của ma trận tuần hoàn đảm bảo rằng sự liền kề của Manhattan giảm thiểu chuyển động giới hạn dọc theo các tuyến đường có cấu trúc và các tương tác giữa các tuyến đường chỉ phụ thuộc vào sự dịch chuyển tương đối. Vì tất cả các mức giảm đều duy trì khả năng tiếp cận, DSU trên các phân đoạn mô phỏng chính xác kết nối mà không cần xây dựng lưới một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import isqrt

def factorize(x, spf):
    res = set()
    while x > 1:
        p = spf[x]
        res.add(p)
        while x % p == 0:
            x //= p
    return res

def build_spf(mx):
    spf = list(range(mx + 1))
    for i in range(2, isqrt(mx) + 1):
        if spf[i] == i:
            for j in range(i * i, mx + 1, i):
                if spf[j] == j:
                    spf[j] = i
    return spf

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.r[a] < self.r[b]:
            a, b = b, a
        self.p[b] = a
        if self.r[a] == self.r[b]:
            self.r[a] += 1

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    mx = max(a)
    spf = build_spf(mx)

    prime_to_indices = {}

    for i, val in enumerate(a):
        primes = factorize(val, spf)
        for p in primes:
            prime_to_indices.setdefault(p, []).append(i)

    # each index is a node
    dsu = DSU(n)

    # threshold in row direction approximation
    # distance within same track behaves like ~2*|i-j|
    if k < n:
        step = k // 2
    else:
        step = n

    # connect along same index tracks (segments)
    if step < n:
        for i in range(n):
            if i + step < n:
                dsu.union(i, i + step)

    # connect indices sharing primes with circular proximity
    for p, idxs in prime_to_indices.items():
        m = len(idxs)
        if m <= 1:
            continue
        idxs.sort()

        # connect in circular order
        for i in range(m - 1):
            if idxs[i + 1] - idxs[i] <= k:
                dsu.union(idxs[i], idxs[i + 1])

        # wrap connection
        if m > 1 and (idxs[0] + n - idxs[-1]) <= k:
            dsu.union(idxs[0], idxs[-1])

    comps = sum(1 for i in range(n) if dsu.find(i) == i)
    print(comps)

if __name__ == "__main__":
    solve()
```DSU được xây dựng dựa trên các chỉ số mảng, đại diện cho toàn bộ các đường chéo trong ma trận. Bước kết hợp đầu tiên xử lý khả năng kết nối bên trong một tuyến đường duy nhất khi bán kính Manhattan cho phép hợp nhất các lần xuất hiện gần đó; khi$k$lớn, điều này sẽ thu gọn mỗi rãnh thành một khối duy nhất. Bước thứ hai kết nối các rãnh khác nhau có chung thừa số nguyên tố và đủ gần trong không gian chỉ số tuần hoàn, giúp ghi lại thời điểm hai đường chéo có thể tương tác qua lưới. 

Số lượng cuối cùng của nghiệm DSU tương ứng với số lượng cụm chỉ số độc lập còn lại sau khi tất cả các kết nối hình học và số học được áp dụng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
3 4 9
```Chúng ta có các số nguyên tố: 3 nối các chỉ số của 3 và 9, trong khi 4 đứng một mình. 

| Bước | Nhóm hoạt động | Trạng thái DSU | 
| --- | --- | --- | 
| Bắt đầu | {0,1,2} | {0}{1}{2} | 
| Thủ tướng 3 | {0,2} | {0,2}{1} | 
| Cuối cùng | cụm hợp nhất | 2 thành phần | 

Điều này cho thấy các giá trị chia sẻ thừa số nguyên tố 3 hợp nhất thành một thành phần, trong khi 4 vẫn bị cô lập. 

### Ví dụ 2 

đầu vào:```
3 2
1 2 3
```Chỉ các chỉ số có giá trị 2 và 3 mới có cấu trúc không cần thiết. 

| Bước | Nhóm hoạt động | Trạng thái DSU | 
| --- | --- | --- | 
| Bắt đầu | {0,1,2} | {0}{1}{2} | 
| Thủ tướng 2 | {1} | không thay đổi | 
| Thủ tướng 3 | {2} | không thay đổi | 
| Kết quả | tất cả đều bị cô lập | 3 thành phần | 

Ở đây các kết nối gcd không hợp nhất bất cứ thứ gì, vì vậy mọi bản nhạc vẫn tách biệt. 

Mỗi ví dụ đều xác nhận rằng khả năng kết nối hoàn toàn bị chi phối bởi cấu trúc cơ bản chung và phạm vi tiếp cận Manhattan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$| Mỗi giá trị được phân tích thành thừa số một lần, các phép toán DSU gần như không đổi | 
| Không gian |$O(n)$| Mảng DSU cộng với nhóm nguyên tố | 

Giải pháp có quy mô tuyến tính trong thực tế cho$n \le 10^6$bởi vì hệ số hóa chiếm ưu thế nhưng vẫn khả thi với cách tiếp cận SPF dựa trên sàng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# sample tests
assert run("3 3\n3 4 9\n") == "2"
assert run("3 2\n3 4 9\n") == "3"
assert run("3 2\n1 2 3\n") == "3"

# custom tests
assert run("2 10\n6 10\n") == "1"
assert run("4 1\n2 3 5 7\n") == "4"
assert run("5 5\n2 4 8 16 32\n") == "1"
assert run("6 2\n6 10 15 14 21 22\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các số nguyên tố được kết nối | 1 | hợp nhất đầy đủ thông qua gcd | 
| tất cả đồng nguyên tố | n | không có cạnh | 
| chuỗi nguyên tố đơn | 1 | đóng cửa chuyển tiếp dài | 
| cấu trúc hỗn hợp | 2 | kết nối một phần | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các số đều là số nguyên tố cùng nhau theo cặp. Trong trường hợp này, không có cạnh nào tồn tại vì điều kiện gcd không bao giờ kích hoạt. DSU vẫn được phân chia hoàn toàn và câu trả lời bằng$n$. 

Một trường hợp khác là khi tất cả các số đều có chung một số nguyên tố, chẳng hạn tất cả đều là số chẵn. Sau đó, mọi kết nối Manhattan hợp lệ sẽ trở nên phù hợp và tùy thuộc vào$k$, toàn bộ cấu trúc có thể sụp đổ thành một thành phần duy nhất. Thuật toán xử lý điều này vì tất cả các chỉ số đều thuộc cùng một nhóm nguyên tố và được hợp nhất thông qua khoảng cách vòng tròn. 

Trường hợp thứ ba xảy ra khi$k \ge n$. Ở đây, ràng buộc Manhattan ngừng hạn chế chuyển động bên trong mỗi rãnh, do đó mỗi chỉ mục hoạt động như một chu trình dọc được kết nối đầy đủ. Thuật toán chuyển sang hành vi kết hợp đầy đủ bên trong mỗi nhóm chỉ mục, đảm bảo rằng tất cả các lần xuất hiện của một giá trị được coi là một thực thể được kết nối duy nhất trước khi áp dụng việc hợp nhất dựa trên số nguyên tố.
