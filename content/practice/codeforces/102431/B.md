---
title: "CF 102431B - Đường dẫn vô tận"
description: "Mỗi cạnh có hướng mang một chữ số thập phân từ 0 đến 9. Một đường dẫn được hiểu là phân số thập phân từ trái sang phải, nhưng mỗi chữ số mới được chia cho một hệ số khác là 10. Ví dụ: một đường dẫn có trọng số cạnh 3, 1, 3 có giá trị [ frac{3+frac{1+frac{3}{10}}{10}}{10}=0."
date: "2026-08-08T23:47:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102431
codeforces_index: "B"
codeforces_contest_name: "2019 China Collegiate Programming Contest Final (CCPC-Final 2019)"
rating: 0
weight: 102431
solve_time_s: 552
verified: true
draft: false
---

[CF 102431B - Đường dẫn vô tận](https://codeforces.com/problemset/problem/102431/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 12 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi cạnh có hướng mang một chữ số thập phân từ 0 đến 9. Một đường dẫn được hiểu là phân số thập phân từ trái sang phải, nhưng mỗi chữ số mới được chia cho một hệ số khác là 10. Ví dụ: một đường dẫn có trọng số cạnh`3, 1, 3`có giá trị 

[ 
\frac{3+\frac{1+\frac{3}{10}}{10}}{10}=0,313. 
] 

Nhiệm vụ là tìm giới hạn dưới lớn nhất của các giá trị của tất cả các đường đi từ đỉnh`0`đến đỉnh`1`. Sự khác biệt giữa các vấn đề tối thiểu và vô cùng vì việc lặp đi lặp lại một chu trình có thể tạo ra các giá trị tiến đến một giới hạn mà không có con đường hữu hạn nào thực sự đạt tới. Câu trả lời được in ra modulo (10^9+7). Sự cố chính thức sử dụng giới hạn thời gian 8 giây và bộ nhớ 256 MB. 

Các giới hạn này đủ nhỏ để cho phép thực hiện phép tính bậc hai gần đúng về số đỉnh. Với (n\le 2000), phương pháp (O(n^2)) hoặc (O(nm)) là hợp lý, đặc biệt vì tổng số đỉnh trong tất cả các trường hợp thử nghiệm nhiều nhất là 20000 và tổng số cạnh nhiều nhất là 40000. Một cách tiếp cận liệt kê các đường dẫn là hoàn toàn khác: chu kỳ có nghĩa là có thể có vô số đường dẫn và thậm chí hạn chế sự chú ý đến các đường dẫn có độ dài cố định (L) có thể tạo ra nhiều bước đi theo cấp số nhân. 

Có một số bẫy khiến việc triển khai đường dẫn ngắn nhất đơn giản trở nên không chính xác. Đầu tiên, việc giảm thiểu trọng số cạnh số thông thường không phải là mục tiêu. Vì```
2 1
0 1 9
```đường dẫn duy nhất có giá trị (0,9), vì vậy câu trả lời là (9/10), tức là`300000003`modulo (10^9+7). Tổng trọng số cạnh theo kiểu Dijkstra sẽ báo cáo là 9, đây là một vấn đề khác. 

Thứ hai, không cần phải đạt được mức tối thiểu. Coi như```
3 3
0 2 1
2 2 1
2 1 9
```Các đường dẫn hữu hạn có các giá trị (0,19), (0,119), (0,1119), v.v. điều tối thiểu của họ là 

[ 
0,11111\ldots=\frac19, 
] 

có giá trị mô-đun là`111111112`. Một giải pháp chỉ tìm kiếm đường đi tối thiểu hữu hạn sẽ bỏ lỡ câu trả lời thực tế. 

Thứ ba, chu trình có trọng số bằng 0 không thể đạt tới đỉnh 1 sẽ không ảnh hưởng đến câu trả lời. Ví dụ,```
4 4
0 2 3
2 1 4
0 3 0
3 3 0
```có câu trả lời (0,34=17/50), tức là`380000003`modulo (10^9+7). Chu trình 0 ở đỉnh 3 có vẻ hấp dẫn nhưng nó không bao giờ có thể hoàn thành ở đỉnh 1, vì vậy nó không liên quan. 

Cuối cùng, một số cạnh có cùng chữ số nhỏ nhất có thể yêu cầu so sánh hậu tố của chúng. Vì```
4 4
0 2 1
0 3 1
2 1 9
3 1 2
```cả hai chữ số đầu tiên đều là 1, vì vậy quyết định phải được đưa ra bằng chữ số tiếp theo. Giá trị tối ưu là (0,12=3/25), cho`840000006`modulo (10^9+7). 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê các đường đi từ đỉnh 0, tính giá trị thập phân của chúng và giữ giá trị nhỏ nhất. Điều này không chỉ đơn thuần là không hiệu quả mà còn không có điểm dừng tự nhiên vì một chu trình hữu ích có thể đi qua nhiều lần tùy ý. Trong biểu đồ có hệ số phân nhánh (b), việc liệt kê tất cả các bước có chiều dài (L) đã mất công (\Theta(b^L)). Với tối đa 4000 cạnh, thậm chí một giới hạn như (L=2000) cũng đưa ra số lượng ứng cử viên vô cùng lớn. Việc hạn chế tìm kiếm trong các đường dẫn đơn giản cũng không giải quyết được vấn đề, bởi vì đường dẫn tối thiểu có thể được tiếp cận bằng các đường dẫn lặp lại một chu kỳ. 

Quan sát quan trọng là so sánh thập phân mang tính từ điển. Chữ số đầu tiên nơi hai đường dẫn khác nhau hoàn toàn xác định giá trị nào nhỏ hơn. Do đó, chúng ta có thể coi câu trả lời là một chuỗi vô hạn các chữ số. Khi một đường dẫn hữu hạn đạt đến đỉnh 1, chúng ta có thể tưởng tượng việc gắn thêm một vòng tự có trọng số bằng 0 mãi mãi ở đỉnh 1. Chuỗi chữ số vô hạn của nó biểu thị chính xác cùng một giá trị thập phân hữu hạn. Ngược lại, mọi đỉnh có thể đạt đến 1 đều có thể đóng vai trò là tiền tố của một đường dẫn hữu hạn hợp lệ nào đó. Do đó, infimum ban đầu tương đương với việc tìm chuỗi chữ số vô hạn nhỏ nhất về mặt từ điển có thể được tạo ra từ đỉnh 0 sau khi thêm một vòng tự 0 ở đỉnh 1. Đây là phép biến đổi trung tâm được sử dụng bởi giải pháp chính thức. 

Các đỉnh không thể tới đỉnh 1 có thể bị loại bỏ ngay lập tức. Trong số các đỉnh còn lại, chỉ có các cạnh đi ra có trọng số tối thiểu là quan trọng. Nếu một đỉnh có một cạnh đi ra có trọng số 2 và một cạnh khác có trọng số 5 thì không có chuỗi vô hạn tối ưu nào bắt đầu từ đó có thể sử dụng cạnh 5, vì chữ số đầu tiên đã lớn hơn. 

Vẫn có thể có một số cạnh có trọng lượng tối thiểu. Tại một số vị trí, chúng ta có thể có một số đỉnh có khả năng tạo ra cùng một chữ số nhỏ nhất, vì vậy chúng ta giữ tất cả chúng làm tập ứng cử viên. Ở vị trí tiếp theo, chúng ta lại chọn chữ số nhỏ nhất có sẵn từ bất kỳ ứng cử viên nào. Việc lặp lại quy trình đặt điểm này sẽ tạo ra chuỗi chữ số nhỏ nhất theo từ điển mà không liệt kê các đường dẫn. 

Trình tự cuối cùng trở thành định kỳ. Đồ thị hữu hạn liên quan chỉ có (n) đỉnh, do đó, bước đi vô hạn tối ưu về mặt khái niệm bao gồm một tiền tố hữu hạn theo sau là một chu trình. Hai cấu trúc tiền tố cộng chu trình có thể có độ dài tối đa (n) có thể được phân biệt trong tổng độ dài của chúng, điều này tạo ra giới hạn chữ số (3n) tiêu chuẩn được sử dụng bởi giải pháp chính thức. Chúng tôi tạo ra (3n) chữ số và tìm kiếm chuỗi con bắt đầu từ vị trí (n) trong khoảng thời gian nhỏ nhất của nó. Bài xã luận chính thức mô tả chính xác việc lặp lại tập hợp điểm này và giới hạn (3n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo chiều dài đường dẫn | Hàm mũ theo chiều dài đường dẫn | Quá chậm | 
| Lặp lại điểm đặt | (O(nm+n^2)) | (O(n+m)) | Đã chấp nhận | 

Thuật ngữ (n^2) xuất phát từ việc tìm khoảng thời gian bằng cách kiểm tra độ dài chu kỳ của ứng viên. Vì (m\le 4000) và (n\le 2000), các giới hạn phù hợp với các giới hạn đã cho. 

## Hướng dẫn thuật toán

1. Xây dựng biểu đồ ngược và chạy tìm kiếm biểu đồ từ đỉnh 1. Đánh dấu mọi đỉnh cuối cùng có thể đạt tới 1. Bất kỳ cạnh nào chạm vào đỉnh không được đánh dấu đều có thể bị bỏ qua, vì không có đường dẫn hợp lệ nào có thể sử dụng nó. 
2. Thêm một cạnh khái niệm (1\to1) có trọng số 0. Điều này chuyển đổi việc dừng ở đỉnh 1 thành tiếp tục mãi mãi với các chữ số bằng 0. Nó cho phép chúng ta suy luận hoàn toàn về các dãy vô hạn thay vì có các trường hợp riêng biệt cho các đường đi hữu hạn và các đường đi tiếp cận vô số thông qua một chu trình. 
3. Đối với mỗi đỉnh còn lại, hãy tìm trọng lượng cạnh đi ra nhỏ nhất của nó và giữ lại tất cả các điểm đến đạt được bởi các cạnh có trọng số chính xác đó. Trọng số gửi đi lớn hơn không bao giờ có thể là một phần của phần tiếp theo nhỏ nhất về mặt từ điển vì chúng mất ở chữ số đầu tiên nơi chúng được sử dụng. 
4. Bắt đầu với tập ứng cử viên (S={0}). Đối với chữ số tiếp theo, hãy kiểm tra mọi đỉnh trong (S) và tìm trọng số xuất ra nhỏ nhất trong số đó. Gọi chữ số này là (d). Nối (d) vào chuỗi câu trả lời. 
5. Thay thế (S) bằng tất cả đích đến của các cạnh có trọng số tối thiểu từ các đỉnh trong (S) cũ có trọng số tối thiểu chính xác là (d). Chúng tôi giữ mọi đích đến bị ràng buộc vì hai đường dẫn có thể chia sẻ tiền tố hiện tại trong khi có các hậu tố khác nhau trong tương lai. 
6. Lặp lại hai bước trước đó (3n) lần. Chuỗi kết quả chứa đủ thông tin để xác định đuôi tuần hoàn cuối cùng của chuỗi thập phân vô hạn tối ưu. Giải pháp chính thức sử dụng cấu trúc bước (3n) này và tìm kiếm phần từ vị trí (n) trở đi cho chu kỳ nhỏ nhất. 
7. Cho (p) là số nguyên dương nhỏ nhất nhiều nhất (n) sao cho chuỗi con từ vị trí (n) đến vị trí (3n-1) lặp lại mọi vị trí (p). Chúng tôi cố tình sử dụng vị trí (n) làm điểm bắt đầu của biểu diễn tuần hoàn ngay cả khi tiền tố thực trở thành tuần hoàn sớm hơn. Việc mở rộng tiền tố sang phần tuần hoàn không làm thay đổi số được biểu thị. 
8. Gọi (P) là số nguyên được biểu thị bằng (n) chữ số đầu tiên và gọi (C) là số nguyên được biểu thị bằng (p) chữ số tiếp theo. Số thập phân vô hạn là 

[ 
\frac{P}{10^n} 
+ 
\frac{C}{10^n(10^p-1)}. 
] 

Công thức này chỉ là chuỗi hình học để nối lại khối chữ số (p) (C). 

1. Tính biểu thức modulo (10^9+7). Vì môđun là số nguyên tố và tất cả các mẫu số cần thiết đều khác 0 đối với các độ dài liên quan, nên có thể thu được nghịch đảo môđun bằng định lý nhỏ Fermat. 

### Tại sao nó hoạt động 

Sau khi loại bỏ các đỉnh không thể đạt tới 1, mọi tiền tố ứng cử viên có thể được hoàn thành thành một đường dẫn hợp lệ đến 1. Việc thêm vòng lặp 0 ở 1 có nghĩa là các đường dẫn hữu hạn và các phiên bản đệm 0 vô hạn của chúng có cùng một giá trị số. So sánh các giá trị thập phân như vậy tương đương với việc so sánh các chuỗi chữ số của chúng về mặt từ điển. 

Tại mọi vị trí, thuật toán chọn chữ số nhỏ nhất mà bất kỳ tiền tố tối ưu hiện tại nào cũng có thể tạo ra. Bất kỳ chuỗi nào bắt đầu bằng chữ số lớn hơn sẽ ngay lập tức tệ hơn, trong khi tất cả các chuỗi bắt đầu bằng chữ số đã chọn vẫn được biểu thị trong tập ứng cử viên mới. Điều này mang lại sự bất biến rằng sau (i) lần lặp, chuỗi chữ số (i) được tạo ra là tiền tố nhỏ nhất có thể có về mặt từ điển trong số tất cả các phần tiếp theo hợp lệ. 

Bởi vì đồ thị là hữu hạn nên sự tiếp tục vô hạn tối ưu có thể được biểu diễn bằng một tiền tố hữu hạn theo sau là một chu trình. Đối số chính thức (3n) chữ số đảm bảo rằng phần tuần hoàn có thể được xác định từ chuỗi được tạo. Một khi đã biết chu kỳ của nó, công thức chuỗi hình học sẽ đưa ra chính xác giá trị nhỏ nhất, kể cả những trường hợp không có đường đi hữu hạn nào đạt được nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve_case(n, m, edges):
    # Reverse graph for finding vertices that can reach 1.
    rev = [[] for _ in range(n)]
    for u, v, w in edges:
        rev[v].append(u)

    good = [False] * n
    good[1] = True
    stack = [1]

    while stack:
        v = stack.pop()
        for u in rev[v]:
            if not good[u]:
                good[u] = True
                stack.append(u)

    # For every useful vertex, keep only minimum-weight outgoing edges
    # whose destinations are also useful.
    min_w = [10] * n
    nxt = [[] for _ in range(n)]

    for u, v, w in edges:
        if not good[u] or not good[v]:
            continue

        if w < min_w[u]:
            min_w[u] = w
            nxt[u] = [v]
        elif w == min_w[u]:
            nxt[u].append(v)

    # The added 1 -> 1 edge of weight zero.
    if good[1]:
        if min_w[1] > 0:
            min_w[1] = 0
            nxt[1] = [1]
        elif min_w[1] == 0:
            nxt[1].append(1)

    # Point-set iteration.
    # cur contains all vertices that can realize the currently
    # smallest prefix.
    cur = {0}
    digits = []

    for _ in range(3 * n):
        d = 10

        for u in cur:
            if min_w[u] < d:
                d = min_w[u]

        digits.append(d)

        new_cur = set()
        for u in cur:
            if min_w[u] == d:
                new_cur.update(nxt[u])

        cur = new_cur

    # Find the smallest period of digits[n:3*n].
    period = None
    for p in range(1, n + 1):
        ok = True
        for i in range(n, 3 * n - p):
            if digits[i] != digits[i + p]:
                ok = False
                break
        if ok:
            period = p
            break

    # The first n digits are the prefix.
    prefix = 0
    for i in range(n):
        prefix = (prefix * 10 + digits[i]) % MOD

    # The next 'period' digits form the repeating block.
    cycle = 0
    for i in range(n, n + period):
        cycle = (cycle * 10 + digits[i]) % MOD

    inv_10_n = pow(pow(10, n, MOD), MOD - 2, MOD)

    ten_p = pow(10, period, MOD)
    cycle_den = (ten_p - 1) % MOD
    inv_cycle_den = pow(cycle_den, MOD - 2, MOD)

    value = (prefix + cycle * inv_cycle_den) % MOD
    value = value * inv_10_n % MOD

    return value

def solve():
    t = int(input())
    out = []

    for case_id in range(1, t + 1):
        n, m = map(int, input().split())
        edges = [tuple(map(int, input().split())) for _ in range(m)]

        ans = solve_case(n, m, edges)
        out.append(f"Case #{case_id}: {ans}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Biểu đồ ngược chỉ được sử dụng cho khả năng tiếp cận. Bắt đầu từ đỉnh 1, mọi đỉnh được tìm thấy trong đồ thị ngược đều có đường dẫn có hướng nào đó đến 1 trong đồ thị gốc. Quá trình xử lý trước này cũng loại bỏ các chu kỳ trọng số 0 gây hiểu lầm không bao giờ có thể đạt được mục tiêu. 

các`min_w`Và`nxt`mảng thực hiện việc giảm thứ hai. Đối với mỗi đỉnh hữu ích,`min_w[u]`là chữ số tiếp theo nhỏ nhất có thể có của nó và`nxt[u]`chứa mọi đích đạt được chữ số đó. Vòng tự đặc biệt ở đỉnh 1 được chèn sau khi đọc các cạnh ban đầu để việc đạt tới 1 luôn cho phép tiếp tục các chữ số 0. 

Vòng lặp tập hợp điểm là trái tim của thuật toán.`cur`đại diện cho mọi đỉnh có thể xuất hiện ngay sau tiền tố tối ưu đã được chọn. Lần quét đầu tiên tìm thấy chữ số tiếp theo nhỏ nhất trong số các đỉnh đó. Lần quét thứ hai chỉ tiếp tục chuyển đổi tạo ra chữ số đó. một con trăn`set`loại bỏ các đích trùng lặp, điều này rất hữu ích khi đầu vào chứa các cạnh song song. 

Việc tìm kiếm dấu chấm có chủ ý bắt đầu ở chữ số`n`, thay vì cố gắng xác định chính xác thời điểm bắt đầu của chu kỳ. Nếu dãy thực trở thành tuần hoàn sớm hơn thì việc lấy một số chữ số tuần hoàn làm một phần của tiền tố là vô hại. Công thức vẫn chính xác vì cùng một đuôi tuần hoàn vô hạn theo sau tiền tố dài hơn đó. 

Tất cả số học liên quan đến số thập phân đều được thực hiện theo modulo (10^9+7). Các số nguyên Python không bị tràn, nhưng việc rút gọn theo mô-đun giữ cho các giá trị trung gian ở mức nhỏ và làm cho số học dự kiến ​​trở nên rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là```
0 -> 2 (3)
2 -> 3 (4)
2 -> 4 (1)
3 -> 1 (2)
4 -> 1 (3)
```Các đỉnh hữu ích đều là năm đỉnh. Chữ số đi ra tối thiểu của đỉnh 0 là 3, của đỉnh 2 là 1, của đỉnh 3 là 2, của đỉnh 4 là 3 và đỉnh 1 có thêm vòng tự 0. 

| Vị trí | Ứng viên đặt trước bước | Chữ số được chọn | Ứng viên đặt sau bước | 
| --- | --- | --- | --- | 
| 1 |`{0}`| 3 |`{2}`| 
| 2 |`{2}`| 1 |`{4}`| 
| 3 |`{4}`| 3 |`{1}`| 
| 4 |`{1}`| 0 |`{1}`| 
| 5 |`{1}`| 0 |`{1}`| 

Trình tự được tạo bắt đầu bằng`31300...`, và sau khi đạt đến đỉnh 1, nó mãi mãi bằng 0. Do đó, giá trị thực tế là (0,313) hoặc 

[ 
\frac{313}{1000}. 
] 

Modulo (10^9+7), điều này mang lại`241000002`, phù hợp với mẫu 

### Mẫu 2 

Các cạnh quan trọng là```
0 -> 1 (9)
0 -> 3 (3)
3 -> 0 (1)
```Các đỉnh khác có thể đạt tới 1 nhưng không cung cấp chữ số đi ra tốt hơn từ các trạng thái quan trọng. 

| Vị trí | Ứng viên đặt trước bước | Chữ số được chọn | Ứng viên đặt sau bước | 
| --- | --- | --- | --- | 
| 1 |`{0}`| 3 |`{3}`| 
| 2 |`{3}`| 1 |`{0}`| 
| 3 |`{0}`| 3 |`{3}`| 
| 4 |`{3}`| 1 |`{0}`| 
| 5 |`{0}`| 3 |`{3}`| 
| 6 |`{3}`| 1 |`{0}`| 

Trình tự tối ưu là 

[ 
0,31313131\ldots=\frac{31}{99}. 
] 

Không có đường dẫn hữu hạn nào có chính xác giá trị này. Thay vào đó, các đường đi qua chu trình (0\to3\to0) ngày càng tiếp cận nó từ phía trên. Giá trị mô-đun là`40404041`, đây là đầu ra mẫu thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm+n^2)) | Chi phí về khả năng tiếp cận (O(n+m)), việc lặp lại tập hợp điểm sẽ kiểm tra các cạnh được giữ lại cho (3n) vị trí và chi phí kiểm tra theo chu kỳ (O(n^2)). | 
| Không gian | (O(n+m)) | Biểu đồ, biểu đồ ngược, danh sách cạnh hữu ích, bộ ứng cử viên và chuỗi chữ số đều tuyến tính ở kích thước đầu vào. | 

Đối với trường hợp thử nghiệm riêng lẻ tối đa, (n\le2000) và (m\le4000), do đó việc xử lý đồ thị vẫn ở dạng đa thức thoải mái. Tổng của (n) và (m) trên tất cả các trường hợp thử nghiệm cũng bị giới hạn, điều này ngăn không cho đầu vào chứa nhiều trường hợp lớn đồng thời. Giải pháp chính thức đưa ra cách xây dựng tập hợp điểm tương tự và sử dụng các chữ số được tạo (3n). 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10**9 + 7

def solve_case(n, m, edges):
    rev = [[] for _ in range(n)]
    for u, v, w in edges:
        rev[v].append(u)

    good = [False] * n
    good[1] = True
    stack = [1]

    while stack:
        v = stack.pop()
        for u in rev[v]:
            if not good[u]:
                good[u] = True
                stack.append(u)

    min_w = [10] * n
    nxt = [[] for _ in range(n)]

    for u, v, w in edges:
        if not good[u] or not good[v]:
            continue

        if w < min_w[u]:
            min_w[u] = w
            nxt[u] = [v]
        elif w == min_w[u]:
            nxt[u].append(v)

    if min_w[1] > 0:
        min_w[1] = 0
        nxt[1] = [1]
    elif min_w[1] == 0:
        nxt[1].append(1)

    cur = {0}
    digits = []

    for _ in range(3 * n):
        d = min(min_w[u] for u in cur)
        digits.append(d)

        new_cur = set()
        for u in cur:
            if min_w[u] == d:
                new_cur.update(nxt[u])
        cur = new_cur

    period = None
    for p in range(1, n + 1):
        if all(
            digits[i] == digits[i + p]
            for i in range(n, 3 * n - p)
        ):
            period = p
            break

    assert period is not None

    prefix = 0
    for d in digits[:n]:
        prefix = (prefix * 10 + d) % MOD

    cycle = 0
    for d in digits[n:n + period]:
        cycle = (cycle * 10 + d) % MOD

    inv_10_n = pow(pow(10, n, MOD), MOD - 2, MOD)
    inv_cycle_den = pow(
        (pow(10, period, MOD) - 1) % MOD,
        MOD - 2,
        MOD
    )

    return (
        (prefix + cycle * inv_cycle_den) % MOD
    ) * inv_10_n % MOD

def run(inp: str) -> str:
    data = iter(map(int, inp.split()))
    t = next(data)
    out = []

    for case_id in range(1, t + 1):
        n = next(data)
        m = next(data)
        edges = [
            (next(data), next(data), next(data))
            for _ in range(m)
        ]
        ans = solve_case(n, m, edges)
        out.append(f"Case #{case_id}: {ans}")

    return "\n".join(out)

sample = """\
2
5 5
0 2 3
2 3 4
2 4 1
3 1 2
4 1 3
5 6
0 1 9
2 0 6
3 0 1
0 3 3
4 0 3
4 2 7
"""

assert run(sample) == (
    "Case #1: 241000002\n"
    "Case #2: 40404041"
), "provided samples"

assert run("""\
1
2 1
0 1 0
""") == "Case #1: 0", "minimum-size zero path"

assert run("""\
1
2 1
0 1 9
""") == "Case #1: 300000003", "single boundary digit"

assert run("""\
1
3 3
0 2 1
2 2 1
2 1 9
""") == "Case #1: 111111112", "non-attained periodic infimum"

assert run("""\
1
4 4
0 2 1
0 3 1
2 1 9
3 1 2
""") == "Case #1: 840000006", "equal first digits"

max_case = "1\n2000 1\n0 1 9\n"
assert run(max_case) == "Case #1: 300000003", "maximum n"

| Test input | Expected output | What it validates |
|---|---|---|
| `2 1`, edge `0 -> 1` with weight 0 | `Case #1: 0` | Minimum graph size and zero value |
| `2 1`, edge `0 -> 1` with weight 9 | `Case #1: 300000003` | Digit 9 and modular division by 10 |
| `3 3`, `0 -> 2 -> 2`, then `2 -> 1` | `Case #1: 111111112` | Infimum produced by an endlessly repeated cycle |
| `4 4`, two weight-1 choices from 0 | `Case #1: 840000006` | Comparing tied first digits by their suffixes |
| `n=2000`, one edge `0 -> 1` with weight 9 | `Case #1: 300000003` | Maximum vertex count and boundary handling |
```Trường hợp tùy chỉnh đầu tiên xác nhận rằng vòng lặp tự mục tiêu không vô tình đưa ra mức đóng góp khác 0. Số thứ hai xác nhận biểu diễn mô-đun của số thập phân một chữ số. Cách thứ ba mắc phải sai lầm cơ bản nhất trong bài toán này, coi câu trả lời là giá trị của một đường đi hữu hạn chứ không phải là một đường dẫn vô cùng nhỏ. Bước thứ tư kiểm tra xem logic tập ứng viên có xử lý một số đường dẫn có cùng chữ số hiện tại hay không. Trường hợp cuối cùng thực hiện số đỉnh được phép lớn nhất mà không đưa ra cấu trúc đồ thị không cần thiết. 

## Vỏ cạnh 

Đối với đường đi trực tiếp không trọng lượng,```
2 1
0 1 0
```tập ứng viên bắt đầu lúc`{0}`và chọn chữ số 0. Sau đó, nó đạt đến đỉnh 1, tại đó vòng lặp tự thêm vào chỉ tạo ra các chữ số 0. Trình tự được tạo ra là`000...`, vậy đáp án chính xác là 0. 

Đối với cạnh trực tiếp có trọng số 9,```
2 1
0 1 9
```chữ số đầu tiên buộc phải là 9, sau đó đỉnh 1 đóng góp số không. Trình tự là`9000...`, đại diện cho (9/10). Tính toán mô-đun sử dụng (10^{-1}), tạo ra`300000003`. 

Đối với trường hợp định kỳ,```
3 3
0 2 1
2 2 1
2 1 9
```chữ số đầu tiên là 1. Tại đỉnh 2, chữ số đi ra nhỏ nhất lại là 1, do đó ứng viên vẫn ở đỉnh 2. Điều này lặp lại vô thời hạn trong chuỗi được tạo. Các đường dẫn cuối cùng đi qua cách tiếp cận cạnh trọng số 9 (0,11111\ldots), cho kết quả (1/9). Việc tìm kiếm dấu chấm sẽ tìm thấy một chu trình một chữ số chứa`1`. 

Đối với chu kỳ 0 không thể truy cập được,```
4 4
0 2 3
2 1 4
0 3 0
3 3 0
```tìm kiếm ngược lại từ đỉnh 1 đánh dấu các đỉnh 1, 2 và 0, nhưng không phải đỉnh 3. Chu trình 0 tại đỉnh 3 bị loại bỏ trước khi quá trình chữ số bắt đầu. Do đó, đỉnh 0 chỉ có cạnh hữu ích có trọng số 3, theo sau là trọng số 4 từ đỉnh 2, do đó chuỗi bắt đầu`34`và sau đó là số không. Giá trị là (34/100=17/50), cho`380000003`. 

Đối với mẫu thứ hai, đồ thị tạo ra chu trình 

[ 
0\xrightarrow{3}3\xrightarrow{1}0. 
] 

Trình tự tập ứng cử viên xen kẽ giữa`{0}`Và`{3}`, sản xuất`313131...`. Các đường đi có thể rời khỏi chu trình này và đến đỉnh 1 sau bất kỳ số lần lặp lại nào, do đó các giá trị đường đi hữu hạn sẽ tiếp cận (31/99). Thuật toán không cần liệt kê rõ ràng bất kỳ đường dẫn nào trong số đó. Nó phát hiện khối hai chữ số lặp lại và đánh giá trực tiếp chuỗi hình học tương ứng.
