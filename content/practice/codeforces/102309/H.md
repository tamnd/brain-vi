---
title: "CF 102309H - Gấu trúc Horton và Orz"
description: "Chúng ta có một đồ thị vô hướng có các đỉnh là Orz Pandas và các cạnh có thể là các liên kết dữ liệu. Mỗi cạnh có hai giá trị dương là a và b. Đối với tập hợp các cạnh S đã chọn, yêu cầu giao tiếp nói rằng các cạnh được chọn phải tạo thành một đồ thị con bao trùm được kết nối."
date: "2026-08-13T23:47:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102309
codeforces_index: "H"
codeforces_contest_name: "The 2019 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102309
solve_time_s: 127
verified: true
draft: false
---

[CF 102309H - Gấu trúc Horton và Orz](https://codeforces.com/problemset/problem/102309/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng có các đỉnh là Orz Pandas và các cạnh có thể là các liên kết dữ liệu. Mỗi cạnh có hai giá trị dương`a`Và`b`. Đối với một tập hợp các cạnh được chọn`S`, yêu cầu giao tiếp nói rằng các cạnh được chọn phải tạo thành một sơ đồ con bao trùm được kết nối. Điểm của một bộ như vậy là 

[ 
f(S)=\frac{\sum_{e\in S} b_e}{\sum_{e\in S} a_e}. 
] 

Nhiệm vụ là tối đa hóa tỷ lệ này. 

Mẫu số luôn dương vì mọi`a_e`là dương và một đồ thị liên thông với`n > 1`cần ít nhất một cạnh. Đồ thị đảm bảo chứa ít nhất một đồ thị con bao trùm liên thông nên bài toán tối ưu luôn có lời giải khả thi. 

Các ràng buộc đủ lớn nên việc liệt kê các tập con cạnh là hoàn toàn không thể. Với tối đa (10^5) cạnh, thậm chí (O(m^2)) đã có sẵn (10^{10}) phép toán, trong khi các thuật toán đồ thị mà chúng ta cần phải ở gần (O(m\log m)) cho mỗi bước tối ưu hóa. Số lượng đỉnh nhiều nhất là (10^4), do đó, cấu trúc tập hợp rời rạc là sự phù hợp tự nhiên để duy trì liên tục các thành phần được kết nối. Các giá trị của`a`Và`b`có thể đạt tới (10^7), do đó tổng có thể đạt tới khoảng (10^{12}). Số nguyên Python xử lý việc này một cách an toàn, trong khi dấu phẩy động chỉ cần thiết cho tham số tỷ lệ. 

Có một số trường hợp việc đơn giản hóa có vẻ hợp lý lại thất bại. 

Đầu tiên, câu trả lời không nhất thiết phải được biểu diễn bằng cây bao trùm. Coi như```
3 3
1 2 1 10
2 3 1 1
1 3 10 10
```Chọn các cạnh`1-2`Và`2-3`cho (11/2=5,5), trong khi chọn cả ba sẽ cho (21/12=1,75). Giải pháp tối ưu ở đây là một cây, nhưng ví dụ này cho thấy rằng chỉ chọn mọi cạnh có tỷ lệ riêng lớn là không đủ. Tính kết nối và tử số và mẫu số tích lũy phải được xem xét cùng nhau. 

Thứ hai, một cạnh được biến đổi có thể có giá trị âm và vẫn cần thiết. Ví dụ,```
3 2
1 2 1 10
2 3 100 1
```Giải pháp được kết nối duy nhất chứa cả hai cạnh, vì vậy câu trả lời là 

[ 
\frac{10+1}{1+100}=\frac{11}{101}. 
] 

Một phương pháp chỉ giữ các cạnh có trọng số biến đổi dương sẽ loại bỏ cạnh thứ hai và khiến đồ thị bị ngắt kết nối. 

Thứ ba, một số cạnh có thể có tỷ lệ chính xác như nhau. Vì```
3 3
1 2 7 7
2 3 7 7
1 3 14 14
```mọi lựa chọn khả thi đều có điểm (1). Thuật toán phải xử lý các trọng số được chuyển đổi bằng 0 mà không coi chúng là lý do để kết thúc sớm. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản về mặt khái niệm. Liệt kê mọi tập hợp con của`m`các liên kết có thể, kiểm tra xem các cạnh được chọn có kết nối tất cả`n`các đỉnh và với mỗi tập hợp con được kết nối, hãy tính tỷ lệ tổng của nó`b`giá trị bằng tổng của nó`a`giá trị. Có (2^m) tập hợp con và kiểm tra chi phí kết nối (O(n+m)) bằng DFS hoặc DSU. Do đó, số lượng hoạt động trong trường hợp xấu nhất là (O(2^m(n+m))), điều này đã vô vọng đối với thậm chí vài chục cạnh, chứ đừng nói đến (10^5). 

Một ý tưởng hấp dẫn hơn là tìm kiếm cây bao trùm có tỷ lệ tốt nhất. Điều đó vẫn chưa đủ, vì bài toán ban đầu cho phép có thêm các cạnh. Một cạnh phụ có thể cải thiện tỷ lệ nếu cạnh của nó`b/a`tỷ lệ hiện tại cao hơn tỷ lệ hiện tại, ngay cả khi nó tạo ra một chu kỳ. Việc tối ưu hóa được thực hiện trên các sơ đồ con được kết nối chứ không chỉ trên cây. 

Quan sát quan trọng là tạm thời thay thế mục tiêu tỷ lệ bằng mục tiêu tuyến tính. Giả sử chúng ta đoán rằng câu trả lời là một giá trị nào đó (\lambda). Đối với bộ cạnh được kết nối`S`, xác định 

[ 
W_\lambda(S)=\sum_{e\in S}(b_e-\lambda a_e). 
] 

Nếu mức tối ưu thực sự là (R), thì 

[ 
R=\max_S\frac{B(S)}{A(S)} 
] 

tương đương với việc nói 

[ 
\max_S \left(B(S)-R A(S)\right)=0. 
] 

Đối với một ứng cử viên (\lambda), một cạnh đã thay đổi trọng số 

[ 
w_e=b_e-\lambda a_e. 
] 

Bây giờ chúng ta cần tìm tổng trọng số biến đổi tối đa trong số tất cả các đồ thị con bao trùm được kết nối. 

Bài toán bổ trợ này có cấu trúc tham lam đơn giản. Nên đưa vào mọi cạnh có trọng số chuyển đổi dương vì việc thêm nó không thể ảnh hưởng đến kết nối và làm tăng nghiêm ngặt mục tiêu được chuyển đổi. Mọi cạnh không trọng lượng cũng có thể được đưa vào vì nó không tốn chi phí gì cho mục tiêu được chuyển đổi và có thể hỗ trợ kết nối. Sau khi tất cả các cạnh không âm đã được đưa vào, điểm cuối của chúng tạo thành một số thành phần được kết nối. Nhiệm vụ duy nhất còn lại là kết nối các thành phần đó. Vì mọi cạnh vẫn đang được xem xét đều có trọng số chuyển đổi âm nên chúng tôi muốn các cạnh kết nối các thành phần ít gây hại nhất. Đó chính xác là bài toán cây bao trùm cực đại trên các thành phần còn lại mà thuật toán Kruskal giải quyết bằng cách xử lý các trọng số được chuyển đổi từ lớn nhất đến nhỏ nhất. 

Điều này đưa ra lời tiên tri chính xác cho một (\lambda) cố định. 

Sau đó chúng tôi sử dụng phương pháp quy hoạch phân số của Dinkelbach. Bắt đầu với bất kỳ tập cạnh kết nối khả thi nào và đặt tỷ lệ của nó là (\lambda). Tìm mức tối đa hóa đồ thị con được kết nối (B-\lambda A). Nếu giá trị được chuyển đổi của nó bằng 0 thì (\lambda) là giá trị tối ưu. Nếu không, đồ thị con mới được chọn có tỷ lệ tốt hơn rất nhiều, vì vậy hãy thay thế (\lambda) bằng tỷ lệ của nó và lặp lại. 

Brute-force hoạt động vì nó đánh giá trực tiếp mọi đồ thị con khả thi, nhưng không thành công vì số lượng đồ thị con là theo cấp số nhân. Quan sát rằng một tỷ lệ có thể được chuyển đổi thành một biểu thức tuyến tính mang lại cho chúng ta một oracle đồ thị con được kết nối có trọng số tối đa và phương pháp của Dinkelbach biến các lệnh gọi lặp lại tới oracle đó thành mức tối ưu mong muốn. 

Sự so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^m(n+m))) | (O(n+m)) | Quá chậm | 
| Tìm kiếm nhị phân + oracle đồ thị con được kết nối được chuyển đổi | (O(Km\log m)) | (O(n+m)) | Đúng, nhưng có nhiều cuộc gọi oracle | 
| Dinkelbach + tiên tri đồ thị con được kết nối đã chuyển đổi | (O(Im\log m)) | (O(n+m)) | Đã chấp nhận | 

Đây`K`là số lần lặp tìm kiếm nhị phân và`I`là số lần lặp Dinkelbach. Đối với bài toán tổ hợp hữu hạn này, Dinkelbach đạt tỷ lệ tối ưu sau hữu hạn nhiều lần thay đổi lời giải đã chọn và trong thực tế số lần lặp là nhỏ. Cách tiếp cận cuộc thi được chấp nhận sử dụng thuộc tính này thay vì trả tiền cho hàng chục vòng tìm kiếm nhị phân có độ chính xác cố định. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cạnh và tính tỷ lệ của toàn bộ tập hợp cạnh, (\lambda=B_{\text{all}}/A_{\text{all}}). Vì bản thân đồ thị đầu vào được kết nối nên tập cạnh hoàn chỉnh là một giải pháp khả thi, do đó, điều này mang lại tỷ lệ bắt đầu hợp lệ. 
2. Đối với giá trị hiện tại của (\lambda), gán trọng số được chuyển đổi cho mỗi cạnh 

[ 
w_e=b_e-\lambda a_e. 
] 

Tối đa hóa tỷ lệ ban đầu ở tham số này tương đương với việc tối đa hóa tổng các trọng số được chuyển đổi này. 
3. Sắp xếp tất cả các cạnh bằng cách giảm trọng số chuyển đổi. Sử dụng cấu trúc DSU để duy trì các thành phần được kết nối được hình thành bởi các cạnh đã được chấp nhận. 
4. Bất cứ khi nào một cạnh có trọng số biến đổi không âm, hãy đưa nó vào đồ thị con đã chọn. Các điểm cuối của nó cũng được thống nhất trong DSU. Cạnh dương làm tăng mục tiêu được chuyển đổi, trong khi cạnh bằng 0 chỉ có thể hỗ trợ kết nối. 
5. Đối với cạnh được chuyển đổi âm, chỉ bao gồm cạnh đó nếu điểm cuối của nó hiện thuộc về các thành phần DSU khác nhau. Một cạnh như vậy là cần thiết để kết nối các thành phần đó và trật tự của Kruskal đảm bảo rằng trong số tất cả các cách kết nối các thành phần, các cạnh âm được chọn sẽ tối đa hóa tổng được chuyển đổi. 
6. Trong khi chọn các cạnh, tích lũy cả tử số và mẫu số ban đầu, cụ thể là`sum_b`Và`sum_a`. Mục tiêu chuyển đổi cũng bằng`sum_b - lambda * sum_a`. 
7. Sau khi tất cả các cạnh đã được xử lý, hãy tính 

[ 
\lambda_{\text{new}}=\frac{\text{sum_b}}{\text{sum_a}}. 
] 

Nếu giá trị mới không thay đổi so với giá trị hiện tại thì giá trị tối ưu được chuyển đổi bằng 0 và tỷ lệ hiện tại là tối ưu. Ngược lại, đặt`lambda = lambda_new`và lặp lại. 
8. In tỷ số cuối cùng có đủ chữ số thập phân. Mười hai chữ số sau dấu thập phân là quá đủ cho lỗi tuyệt đối hoặc tương đối (10^{-9}) bắt buộc. 

Lý do bài toán con được chuyển đổi hoạt động được nắm bắt bởi tính bất biến của nó: sau khi xử lý bất kỳ tiền tố nào của các cạnh theo trọng số được chuyển đổi giảm dần, DSU biểu thị chính xác kết nối có thể thu được bằng cách sử dụng các cạnh đã được xem xét, trong khi tập hợp đã chọn có giá trị được chuyển đổi tối đa có thể tùy thuộc vào quá trình xử lý một phần đó. Tất cả các cạnh không âm là bắt buộc trong một bài toán tối ưu được chuyển đổi và các cạnh âm vẫn cần thiết tạo thành một khu rừng bao trùm tối đa giữa các thành phần kết quả. 

Về phần Dinkelbach, đặt (F(\lambda)) là giá trị lớn nhất của (B(S)-\lambda A(S)) trên các đồ thị con bao trùm được kết nối. Nếu (\lambda<R), một số giải pháp khả thi có tỷ lệ lớn hơn (\lambda), do đó (F(\lambda)>0). Nếu (\lambda>R), mọi giải pháp khả thi đều có tỷ lệ tối đa (\lambda), do đó (F(\lambda)<0). Ở mức tối ưu (R), (F(R)=0). Khi oracle trả về một giải pháp có giá trị biến đổi dương, tỷ lệ của nó lớn hơn rất nhiều so với hiện tại (\lambda), do đó, chuỗi sẽ di chuyển đơn điệu về phía mức tối ưu. Khi oracle trả về 0, giá trị hiện tại chính xác là một tỷ lệ tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    __slots__ = ("parent", "size")

    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        parent = self.parent
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(self, a, b):
        parent = self.parent
        size = self.size

        a = self.find(a)
        b = self.find(b)

        if a == b:
            return False

        if size[a] < size[b]:
            a, b = b, a

        parent[b] = a
        size[a] += size[b]
        return True

def solve_case(n, edges):
    total_a = 0
    total_b = 0

    for u, v, a, b in edges:
        total_a += a
        total_b += b

    # The complete graph is connected, so this is a feasible starting point.
    lam = total_b / total_a

    # Dinkelbach iterations.
    while True:
        # Reordering the edge list is intentional. Timsort can reuse existing
        # order between iterations in many instances.
        edges.sort(key=lambda e: e[3] - lam * e[2], reverse=True)

        dsu = DSU(n)

        sum_a = 0
        sum_b = 0

        for u, v, a, b in edges:
            w = b - lam * a

            if w >= 0.0:
                # Every nonnegative transformed edge belongs to an optimum.
                sum_a += a
                sum_b += b
                dsu.union(u, v)
            else:
                # Negative edges are used only when they are necessary for
                # connecting two currently different components.
                if dsu.union(u, v):
                    sum_a += a
                    sum_b += b

        new_lam = sum_b / sum_a

        # At the exact optimum, the maximizing transformed solution has
        # ratio equal to the current parameter.
        if new_lam == lam:
            return lam

        lam = new_lam

def main():
    out = []

    while True:
        line = input()
        if not line:
            break

        n, m = map(int, line.split())
        edges = []

        for _ in range(m):
            x, y, a, b = map(int, input().split())
            edges.append((x - 1, y - 1, a, b))

        ans = solve_case(n, edges)
        out.append(f"{ans:.12f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```DSU sử dụng tính năng nén đường dẫn cùng với kết hợp theo kích thước, do đó, mỗi thao tác kết hợp hoặc tìm kiếm đều có thời gian không đổi một cách hiệu quả đối với các kích thước vấn đề liên quan. Các đỉnh của biểu đồ được chuyển đổi ngay lập tức từ các chỉ mục đầu vào dựa trên một thành các chỉ mục dựa trên 0, giúp giữ cho tất cả các truy cập mảng bên trong nhất quán. 

Lần đầu tiên tính tỷ lệ của tất cả các cạnh. Tập cạnh đầy đủ được kết nối bằng đảm bảo đầu vào, vì vậy đây là tỷ lệ khả thi hợp lệ. Bắt đầu từ một tỷ lệ khả thi là rất hữu ích vì mỗi lần cập nhật Dinkelbach chỉ có thể cải thiện nó theo hướng tối ưu. 

Trong mỗi lần lặp, trọng số chuyển đổi được đánh giá là`b - lam * a`. Thứ tự sắp xếp giảm dần vì chúng ta đang giải bài toán có trọng số cực đại. Mã xử lý`w >= 0`như một sự bao gồm vô điều kiện. Điều này bao gồm các cạnh không trọng lượng, có thể kết nối các thành phần mà không làm thay đổi vật kính được chuyển đổi. 

Đối với các cạnh âm, kiểm tra DSU là điều kiện Kruskal. Nếu các điểm cuối đã được kết nối, việc thêm cạnh sẽ chỉ làm giảm mục tiêu chuyển đổi. Nếu chúng nằm trong các thành phần khác nhau thì cạnh đó là cần thiết để tiến tới một biểu đồ liên thông nên nó được chấp nhận. 

Bản gốc`a`Và`b`tổng được tích lũy chứ không phải là tổng được chuyển đổi theo dấu phẩy động. Điều này tránh được lỗi số không cần thiết khi tính tỷ lệ tiếp theo. Tất cả các tổng đầu vào đều vừa khít với số nguyên Python và các phép toán dấu phẩy động duy nhất là các phép so sánh được chuyển đổi và cập nhật tỷ lệ. 

Thử nghiệm chấm dứt sử dụng sự bằng nhau của tỷ lệ dấu phẩy động thay vì epsilon tùy ý. Bản thân mỗi bản cập nhật đều là thương của hai tổng số nguyên và khi cùng một giải pháp tổ hợp tối ưu tương tự được chọn lại, thương số được tính toán sẽ được biểu thị bằng cùng một giá trị dấu phẩy động Python. Điều này tránh việc dừng quá sớm chỉ vì epsilon được chọn lớn hơn khoảng trống còn lại. 

## Ví dụ đã hoạt động 

Chỉ có một mẫu chính thức được cung cấp, vì vậy dấu vết thứ hai bên dưới sử dụng một hộp đựng được thiết kế nhỏ nhằm đáp ứng nhu cầu về cạnh kết nối âm. 

### Mẫu 1 

Đồ thị là```
4 4
1 2 20 10
2 3 30 10
3 4 40 10
4 1 50 10
```Tỷ lệ ban đầu của cả bốn cạnh là (40/140=2/7). 

| Lặp lại | Hiện tại (\lambda) | Các cạnh được chọn | Tổng A | Tổng B | Tỷ lệ mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0,285714285714 | 1-2, 2-3, 3-4 | 90 | 30 | 0.333333333333 | 
| 2 | 0.333333333333 | 1-2, 2-3, 3-4 | 90 | 30 | 0.333333333333 | 

Ở lần lặp đầu tiên, các trọng số được chuyển đổi là xấp xỉ (4,286), (1,429), (-1,429) và (-4,286). Hai cạnh đầu tiên là dương và nối các đỉnh 1, 2 và 3. Trong số các cạnh âm,`3-4`ít có hại hơn`4-1`, do đó nó được chọn để nối đỉnh 4. 

Tỷ lệ kết quả là (30/90=1/3). Tại (\lambda=1/3), cạnh đầu tiên là dương, cạnh thứ hai chính xác bằng 0, cạnh thứ ba là âm nhưng cần để nối đỉnh 4 và cạnh thứ tư thậm chí còn âm hơn. Bộ giống nhau được chọn nên tỷ lệ đã đạt đến điểm cố định. Đầu ra là`0.333333333333`. 

### Xây dựng ví dụ 2 

Hãy xem xét```
3 2
1 2 1 10
2 3 100 1
```Chỉ có một tập cạnh bao trùm được kết nối, vì vậy cả hai cạnh phải được chọn. 

| Lặp lại | Hiện tại (\lambda) | Cạnh 1-2 | Cạnh 2-3 | Đã chọn A | Đã chọn B | Tỷ lệ mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 101/11 | tích cực | tiêu cực | 101 | 11 | 101/11 | 
| 2 | 101/11 | tích cực | không | 101 | 11 | 101/11 | 

Cạnh thứ hai là âm trong quá trình tối ưu hóa được chuyển đổi lần đầu tiên, nhưng nó nối đỉnh 3 với thành phần hiện có nên DSU chấp nhận nó. Điều này chứng tỏ tại sao giải pháp chuyển đổi không thể đơn giản loại bỏ mọi cạnh âm. 

Ở lần lặp thứ hai, trọng số biến đổi của cạnh thứ hai trở thành chính xác bằng 0 vì tỷ lệ của nó là (1/100), trong khi tỷ lệ chung hiện tại là (11/101). Cạnh vẫn được bao gồm vì kết nối yêu cầu nó. Đáp án cuối cùng là (101/11). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(Im\log m)) | Mỗi lần lặp Dinkelbach sắp xếp`m`trọng số cạnh được chuyển đổi và thực hiện công việc (O(m\alpha(n))) DSU | 
| Không gian | (O(n+m)) | Danh sách cạnh, mảng DSU và siêu dữ liệu sắp xếp yêu cầu bộ nhớ tuyến tính | 

Đây`I`biểu thị số lần lặp Dinkelbach. Mỗi lần lặp lại bị chi phối bởi việc sắp xếp các cạnh, trong khi các hoạt động DSU có hiệu quả tuyến tính. Với (m\le 10^5), bản thân biểu đồ có thể dễ dàng biểu diễn trong giới hạn bộ nhớ. Số lần lặp Dinkelbach thực tế là nhỏ vì mỗi lần lặp không kết thúc sẽ thay thế tỷ số hiện tại bằng tỷ số của một giải pháp biến đổi tốt hơn. 

Việc sử dụng Dinkelbach thay vì tìm kiếm nhị phân 60 hoặc 100 bước cố định đặc biệt hữu ích ở đây vì mọi lời gọi oracle đều yêu cầu sắp xếp tối đa (10^5) cạnh. Việc giảm số lượng các lệnh gọi như vậy là vấn đề cần xem xét hiệu suất chính. 

## Trường hợp thử nghiệm```python
# The test harness below mirrors the submitted algorithm.
import sys
import io

class DSU:
    __slots__ = ("parent", "size")

    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        parent = self.parent
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(self, a, b):
        parent = self.parent
        size = self.size

        a = self.find(a)
        b = self.find(b)

        if a == b:
            return False

        if size[a] < size[b]:
            a, b = b, a

        parent[b] = a
        size[a] += size[b]
        return True

def solve_case(n, edges):
    total_a = sum(e[2] for e in edges)
    total_b = sum(e[3] for e in edges)

    lam = total_b / total_a

    while True:
        edges.sort(key=lambda e: e[3] - lam * e[2], reverse=True)

        dsu = DSU(n)
        sa = 0
        sb = 0

        for u, v, a, b in edges:
            w = b - lam * a

            if w >= 0.0:
                sa += a
                sb += b
                dsu.union(u, v)
            elif dsu.union(u, v):
                sa += a
                sb += b

        new_lam = sb / sa

        if new_lam == lam:
            return lam

        lam = new_lam

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    out = []

    while True:
        line = sys.stdin.readline()
        if not line:
            break

        n, m = map(int, line.split())
        edges = []

        for _ in range(m):
            x, y, a, b = map(int, sys.stdin.readline().split())
            edges.append((x - 1, y - 1, a, b))

        out.append(f"{solve_case(n, edges):.12f}")

    sys.stdin = old_stdin
    return "\n".join(out)

# Provided sample
assert run(
    """\
4 4
1 2 20 10
2 3 30 10
3 4 40 10
4 1 50 10
"""
) == "0.333333333333", "sample 1"

# Minimum-size graph. The only edge has ratio 8/2 = 4.
assert run(
    """\
2 1
1 2 2 8
"""
) == "4.000000000000", "minimum-size case"

# A negative transformed edge is unavoidable because it is the only bridge.
assert run(
    """\
3 2
1 2 1 10
2 3 100 1
"""
) == "0.108910891089", "required negative edge"

# All edges have exactly the same ratio.
assert run(
    """\
3 3
1 2 7 7
2 3 7 7
1 3 14 14
"""
) == "1.000000000000", "equal ratios"

# Maximum n and m. All edges have ratio 1, so every connected solution has
# the same ratio. The graph contains a chain plus many parallel edges.
n = 10000
m = 100000
lines = [f"{n} {m}"]

for i in range(1, n):
    lines.append(f"{i} {i + 1} 10000000 10000000")

for _ in range(m - (n - 1)):
    lines.append("1 2 10000000 10000000")

assert run("\n".join(lines) + "\n") == "1.000000000000", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2 2 8`|`4.000000000000`| Số đỉnh tối thiểu và đồ thị liên thông đơn giản nhất có thể | 
|`3 2 / 1 2 1 10 / 2 3 100 1`|`0.108910891089`| Một cạnh biến đổi tiêu cực bắt buộc phải có để kết nối | 
| Ba cạnh với`b/a = 1`|`1.000000000000`| Tỷ lệ bằng nhau và trọng lượng chuyển đổi bằng 0 | 
|`n=10000, m=100000`, tất cả`a=b=10000000`|`1.000000000000`| Kích thước tối đa đã nêu, tổng số nguyên lớn và khả năng mở rộng DSU | 

## Vỏ cạnh 

Một đồ thị chỉ có hai đỉnh có chính xác một kết nối cần thiết trong trường hợp hợp lệ nhỏ nhất. Vì```
2 1
1 2 2 8
```tập khả thi duy nhất chứa cạnh đơn, vì vậy câu trả lời là (8/2=4). Tỷ lệ ban đầu đã là 4 và trọng số chuyển đổi bằng 0, do đó thuật toán ngay lập tức đạt đến điểm cố định. 

Một cây cầu có tỷ lệ thấp bắt buộc được xử lý bởi phần Kruskal của nhà tiên tri đã biến đổi. TRONG```
3 2
1 2 1 10
2 3 100 1
```cạnh đầu tiên có giá trị biến đổi cao, trong khi cạnh thứ hai có giá trị âm. Sau khi chấp nhận cạnh đầu tiên, đỉnh 1 và 2 tạo thành một thành phần và đỉnh 3 bị cô lập. Cạnh tiêu cực tham gia vào các thành phần đó, vì vậy DSU chấp nhận nó bất chấp đóng góp tiêu cực của nó. Tỷ lệ cuối cùng là (11/101), xấp xỉ`0.108910891089`. 

Tỷ lệ bằng nhau là một trường hợp ranh giới hữu ích khác. TRONG```
3 3
1 2 7 7
2 3 7 7
1 3 14 14
```mọi cạnh đều có tỷ lệ 1. Tại (\lambda=1), mọi trọng số được chuyển đổi đều chính xác bằng 0. Oracle có thể bao gồm các cạnh và tỷ lệ kết quả vẫn là 1. Việc xử lý đẳng thức trong`w >= 0.0`cho phép các cạnh không trọng lượng cung cấp kết nối. 

Kiểm tra kích thước tối đa chứa (10^4) đỉnh và (10^5) cạnh, với mọi cạnh đều thỏa mãn`a=b=10000000`. Tổng số tiền lớn, nhưng số nguyên Python thể hiện chúng một cách chính xác. Vì mọi cạnh đều có tỷ lệ 1 nên các trọng số được chuyển đổi biến mất tại (\lambda=1) và câu trả lời vẫn chính xác là 1. Trường hợp này cũng thực hiện DSU và mã sắp xếp ở các kích thước đầu vào lớn nhất đã nêu.
