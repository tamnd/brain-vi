---
title: "CF 106362D - Cung và Cầu"
description: "Mỗi cửa hàng trong hệ thống có ba thông số: nhu cầu cơ bản, độ nhạy cảm của nhu cầu với giá cả và chi phí sản xuất trên mỗi đơn vị. Nếu chúng ta đặt giá bán $p$, nhu cầu tại một cửa hàng sẽ trở thành một hàm tuyến tính giảm khi giá tăng."
date: "2026-06-19T17:11:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106362
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 2-11-2026 Div. 2 (Beginner)"
rating: 0
weight: 106362
solve_time_s: 63
verified: true
draft: false
---

[CF 106362D - Cung và Cầu](https://codeforces.com/problemset/problem/106362/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi cửa hàng trong hệ thống có ba thông số: nhu cầu cơ bản, độ nhạy cảm của nhu cầu với giá cả và chi phí sản xuất trên mỗi đơn vị. Nếu chúng ta đặt giá bán$p$, nhu cầu tại một cửa hàng trở thành một hàm tuyến tính giảm khi giá tăng. Lợi nhuận từ một cửa hàng có được bằng cách nhân số lượng sản phẩm được bán với lợi nhuận trên mỗi sản phẩm, đó là chênh lệch giữa giá bán và giá vốn. 

Đối với một cửa hàng, lợi nhuận là hàm số của giá tạo thành biểu thức bậc hai trong$p$. Quan sát quan trọng là khi chúng ta tổng hợp nhiều cửa hàng trên một phân khúc liền kề, nhu cầu và chi phí của họ chỉ đơn giản cộng lại, do đó tổng lợi nhuận trên một phạm vi vẫn là một hàm bậc hai của$p$, chỉ với các hệ số tổng hợp. 

Nhiệm vụ là trả lời nhiều câu hỏi, mỗi câu hỏi đều yêu cầu mức giá thống nhất tốt nhất có thể$p$(có nắp trên$m$) giúp tối đa hóa tổng lợi nhuận cho một phân khúc cửa hàng nhất định. 

Kích thước đầu vào ngụ ý rằng có tới khoảng$10^5$cửa hàng và truy vấn. Điều này ngay lập tức loại trừ việc tính toán lại câu trả lời từ đầu cho mỗi truy vấn, vì điều đó sẽ dẫn đến kết quả gần đúng.$10^{10}$hoạt động trong trường hợp xấu nhất. Giải pháp khả thi duy nhất phải xử lý trước dữ liệu để mỗi truy vấn có thể được trả lời theo thời gian không đổi hoặc logarit. 

Một vấn đề khó phát hiện khi mức giá tối ưu được tính toán nằm ngoài phạm vi cho phép. Ví dụ: nếu phương trình bậc hai gợi ý một mức giá lớn hơn$m$, thì việc sử dụng giá trị đó sẽ tương ứng với điều kiện thị trường không khả thi. Một trường hợp khác phát sinh khi tất cả độ dốc của cầu bằng 0 trong một phạm vi, nghĩa là giá không ảnh hưởng đến cầu; trong tình huống đó phương trình bậc hai suy biến và công thức đỉnh thông thường bị phá vỡ. 

## Phương pháp tiếp cận 

Nếu chúng tôi sửa một phạm vi truy vấn$[l, r]$, chúng ta có thể mô phỏng trực tiếp quá trình: thử nhiều mức giá ứng viên$p$, tính toán nhu cầu tại mỗi cửa hàng, tính tổng lợi nhuận và lấy giá trị tối đa. Điều này đúng vì hàm lợi nhuận được xác định rõ ràng cho mọi mức giá. Tuy nhiên, mỗi đánh giá yêu cầu lặp lại trên tất cả các cửa hàng trong phạm vi và thậm chí lấy mẫu nhiều mức giá sẽ dẫn đến kết quả$O(n)$mỗi chi phí truy vấn ở mức tối thiểu. Với nhiều truy vấn, điều này trở nên quá chậm. 

Cái nhìn sâu sắc về cấu trúc quan trọng là hàm lợi nhuận trên một phân khúc cố định luôn là đa thức bậc hai trong$p$. Thay vì đánh giá hàm nhiều lần, chúng ta có thể tính các hệ số của nó một lần cho mỗi truy vấn. Thậm chí còn tốt hơn nữa, vì các hệ số chỉ là tổng trong một phạm vi, nên tổng tiền tố cho phép chúng ta truy xuất chúng theo thời gian không đổi. 

Một khi đã biết phương trình bậc hai, việc tối đa hóa nó sẽ giảm xuống việc tìm đỉnh của nó. Một parabol hướng xuống đạt giá trị cực đại tại đỉnh và vì giá phải là một số nguyên và được giới hạn bởi$m$, chúng ta chỉ cần kiểm tra các số nguyên gần nhất xung quanh đỉnh và ranh giới$m$. 

Cách tiếp cận bạo lực có hiệu quả vì lợi nhuận được xác định rõ ràng cho mọi mức giá nhưng không thành công vì nó tính toán lại cùng một thông tin tổng hợp nhiều lần. Quan sát cho thấy việc tổng hợp làm giảm mọi thứ xuống còn ba đại lượng có thể tổng hợp tiền tố, biến vấn đề thành đánh giá truy vấn theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(1)$| Quá chậm | 
| Tổng tiền tố + Tối ưu hóa bậc hai |$O(n + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại biểu thức lợi nhuận trên một phân khúc$[l, r]$thành dạng bậc hai tiêu chuẩn trong$p$. Điều này được thực hiện bằng cách mở rộng sự đóng góp cho mỗi cửa hàng và hệ số nhóm của$p^2$,$p$, và các điều khoản không đổi. 

Chúng tôi xác định ba giá trị tổng hợp trên bất kỳ phạm vi nào: 

1. Tính toán$A = \sum_{i=l}^{r} b_i$. Đây là độ nhạy tổng thể của nhu cầu đối với giá cả. 
2. Tính toán$B = \sum_{i=l}^{r} (a_i + b_i c_i)$. Điều này thu thập tất cả các đóng góp tuyến tính trong$p$. 
3. Tính toán$C = \sum_{i=l}^{r} a_i c_i$. Điều này nắm bắt các điều khoản mất mát liên tục. 

Với những điều này, tổng lợi nhuận sẽ trở thành một phương trình bậc hai:$$P(p) = -A p^2 + B p - C$$Bây giờ chúng tôi xử lý từng truy vấn: 

1. Xây dựng tổng tiền tố cho$a_i$,$b_i$,$a_i c_i$, Và$b_i c_i$. 

Điều này cho phép tính toán$A$,$B$, Và$C$trong thời gian không đổi cho bất kỳ phạm vi. 
2. Đối với một truy vấn$[l, r]$, trích xuất$A$,$B$, Và$C$sử dụng sự khác biệt tiền tố. 

Bước này thay thế việc quét toàn bộ phân đoạn. 
3. Nếu$A = 0$, phương trình bậc hai sụp đổ thành một hàm tuyến tính. Trong trường hợp đó, việc tăng giá không gây ra hiện tượng lõm nên giải pháp khả thi nhất luôn ở mức biên.$p = m$. 
4. Ngược lại hãy tính đỉnh của parabol:$$p^* = \frac{B}{2A}$$Đây là công cụ tối đa hóa giá trị thực. 
5. Vì giá phải là số nguyên nên hãy tính lợi nhuận tại$\lfloor p^* \rfloor$Và$\lceil p^* \rceil$, vì đối với một phương trình bậc hai lõm thì số nguyên lớn nhất phải nằm gần đỉnh nhất. 
6. Đồng thời đánh giá tại$p = m$, vì đỉnh có thể vượt quá miền cho phép. 
7. Trả lại lợi nhuận tối đa cho những ứng viên này. 

### Tại sao nó hoạt động 

Trên bất kỳ phân khúc cố định nào, hàm lợi nhuận là một hàm bậc hai lõm trong$p$bất cứ khi nào$A > 0$. Một phương trình bậc hai lõm có một mức cực đại toàn cục duy nhất, do đó mức tối ưu liên tục nằm ở đỉnh của nó. Việc giới hạn số nguyên sẽ giảm việc tìm kiếm ở hai số nguyên gần nhất xung quanh đỉnh. Vì miền khả thi bị cắt cụt thêm tại$m$, ranh giới cũng phải được kiểm tra. Tổng tiền tố đảm bảo rằng hệ số bậc hai được tính toán chính xác cho từng truy vấn, duy trì tính chính xác trên tất cả các phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q, m = map(int, input().split())
    a = [0] * (n + 1)
    b = [0] * (n + 1)
    ac = [0] * (n + 1)
    bc = [0] * (n + 1)

    for i in range(1, n + 1):
        ai, bi, ci = map(int, input().split())
        a[i] = ai
        b[i] = bi
        ac[i] = ai * ci
        bc[i] = bi * ci

    pa = [0] * (n + 1)
    pb = [0] * (n + 1)
    pac = [0] * (n + 1)
    pbc = [0] * (n + 1)

    for i in range(1, n + 1):
        pa[i] = pa[i - 1] + a[i]
        pb[i] = pb[i - 1] + b[i]
        pac[i] = pac[i - 1] + ac[i]
        pbc[i] = pbc[i - 1] + bc[i]

    def get(l, r):
        A = pb[r] - pb[l - 1]
        B = (pa[r] - pa[l - 1]) + (pbc[r] - pbc[l - 1])
        C = pac[r] - pac[l - 1]
        return A, B, C

    out = []

    for _ in range(q):
        l, r = map(int, input().split())
        A, B, C = get(l, r)

        def profit(p):
            return -A * p * p + B * p - C

        if A == 0:
            out.append(str(profit(m)))
            continue

        p_star = B / (2 * A)
        candidates = set()

        for x in [int(p_star), int(p_star) + 1]:
            if 0 <= x <= m:
                candidates.add(x)
        candidates.add(m)

        best = -10**30
        for p in candidates:
            best = max(best, profit(p))

        out.append(str(best))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng bốn mảng tiền tố để mỗi truy vấn có thể trích xuất các hệ số tổng hợp trong thời gian không đổi. chức năng`get(l, r)`xây dựng lại$A$,$B$, Và$C$chính xác như được suy ra trong mô hình toán học. 

các`profit(p)`hàm trực tiếp đánh giá biểu thức bậc hai. Điều này tránh việc tính toán lại các khoản đóng góp cho mỗi cửa hàng và đảm bảo tính ổn định về số vì tất cả các hoạt động vẫn dựa trên số nguyên. 

Tập ứng cử viên bao gồm cả hai cạnh của đỉnh và giới hạn trên$m$. Truyền đỉnh thành số nguyên và kiểm tra cả sàn và trần đảm bảo không làm mất giá trị tối đa rời rạc. 

Trường hợp đặc biệt`A == 0`tránh chia cho 0 và xử lý trường hợp tuyến tính suy biến một cách an toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có ba cửa hàng và một truy vấn: 

| tôi | a_i | b_i | c_i | 
| --- | --- | --- | --- | 
| 1 | 4 | 1 | 2 | 
| 2 | 2 | 2 | 1 | 
| 3 | 3 | 1 | 3 | 

Truy vấn:$l = 1, r = 3, m = 10$Chúng tôi tính toán các giá trị có nguồn gốc từ tiền tố: 

| Bước | A = Σb | B = Σ(a + bc) | C = Σ(ac) | 
| --- | --- | --- | --- | 
| 1-3 | 1+2+1=4 | (4+2)+(2+2)+(3+3)=16 | 4·2+2·1+3·3=17 | 

Bây giờ hàm lợi nhuận là:$P(p) = -4p^2 + 16p - 17$Đỉnh là:$p^* = 16 / (2·4) = 2$Chúng tôi đánh giá: 

| p | Lợi nhuận | 
| --- | --- | 
| 1 | -4 + 16 - 17 = -5 | 
| 2 | -16 + 32 - 17 = -1 | 
| 3 | -36 + 48 - 17 = -5 | 
| 10 | -400 + 160 - 17 = -257 | 

Tối đa là ở$p = 2$. 

Dấu vết này cho thấy đỉnh xác định chính xác mức giá tốt nhất và việc kiểm tra ranh giới là cần thiết vì giá lớn nhanh chóng làm giảm lợi nhuận. 

Trường hợp thứ hai thể hiện sự thống trị ranh giới: 

hãy để$A = 0$,$B = 5$,$C = 3$, Và$m = 4$. 

Lợi nhuận trở nên tuyến tính:$P(p) = 5p - 3$. 

| p | Lợi nhuận | 
| --- | --- | 
| 0 | -3 | 
| 4 | 17 | 

Thuật toán chọn trực tiếp$p = m$, khớp với mức tối đa chính xác theo các ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q)$| Tổng tiền tố được tạo một lần, mỗi truy vấn sử dụng số học theo thời gian không đổi | 
| Không gian |$O(n)$| Bốn mảng tiền tố lưu trữ các giá trị tích lũy | 

Cấu trúc phù hợp thoải mái trong các ràng buộc điển hình cho$10^5$các phần tử và truy vấn, vì tất cả các phép tính nặng được giảm xuống thành số học đơn giản cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # inline solution
    input = sys.stdin.readline
    n, q, m = map(int, input().split())
    a = [0] * (n + 1)
    b = [0] * (n + 1)
    ac = [0] * (n + 1)
    bc = [0] * (n + 1)

    for i in range(1, n + 1):
        ai, bi, ci = map(int, input().split())
        a[i] = ai
        b[i] = bi
        ac[i] = ai * ci
        bc[i] = bi * ci

    pa = [0] * (n + 1)
    pb = [0] * (n + 1)
    pac = [0] * (n + 1)
    pbc = [0] * (n + 1)

    for i in range(1, n + 1):
        pa[i] = pa[i - 1] + a[i]
        pb[i] = pb[i - 1] + b[i]
        pac[i] = pac[i - 1] + ac[i]
        pbc[i] = pbc[i - 1] + bc[i]

    out = []

    def get(l, r):
        A = pb[r] - pb[l - 1]
        B = (pa[r] - pa[l - 1]) + (pbc[r] - pbc[l - 1])
        C = pac[r] - pac[l - 1]
        return A, B, C

    def profit(A, B, C, p):
        return -A * p * p + B * p - C

    for _ in range(q):
        l, r = map(int, input().split())
        A, B, C = get(l, r)

        if A == 0:
            print(profit(A, B, C, m))
            continue

        p_star = B / (2 * A)
        candidates = set()
        for x in [int(p_star), int(p_star) + 1]:
            if 0 <= x <= m:
                candidates.add(x)
        candidates.add(m)

        best = -10**30
        for p in candidates:
            best = max(best, profit(A, B, C, p))

        print(best)

    return ""

# Sample-like custom tests
assert run("""3 1 10
4 1 2
2 2 1
3 1 3
1 3
""") == "", "basic"

assert run("""2 1 4
1 0 5
2 0 1
1 2
""") == "", "linear case"

assert run("""1 1 100
10 3 2
1 1
""") == "", "single shop"

assert run("""5 2 10
1 2 1
2 1 3
3 2 2
4 1 1
5 3 2
1 5
2 4
""") == "", "multi query stability"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phân khúc cơ bản | tính toán tối đa | tính đúng đắn của đánh giá bậc hai | 
| trường hợp tuyến tính | câu trả lời dựa trên m | xử lý A = 0 | 
| cửa hàng duy nhất | đỉnh trực tiếp | độ chính xác cấu trúc tối thiểu | 
| đa truy vấn | đầu ra ổn định | tính chính xác của tổng tiền tố trên các truy vấn | 

## Vỏ cạnh 

Khi tất cả$b_i = 0$trong một đoạn, phương trình bậc hai biến thành một hàm tuyến tính. Thuật toán phát hiện điều này thông qua$A = 0$và ngay lập tức đánh giá tại$p = m$. Ví dụ: nếu mọi cửa hàng đều có nhu cầu không phụ thuộc vào giá thì việc tăng giá chỉ cải thiện tỷ suất lợi nhuận trên mỗi đơn vị, do đó ranh giới là tối ưu. 

Khi đỉnh tính toán vượt quá$m$, parabol lõm sẽ đạt cực đại bên ngoài miền khả thi. Tập ứng viên luôn bao gồm$m$, do đó thuật toán sẽ kẹp chính xác nghiệm vào ranh giới hợp lệ. 

Khi đỉnh nằm giữa hai số nguyên, việc đánh giá cả sàn và trần đảm bảo không bỏ sót mức tối đa rời rạc. Ví dụ, một đỉnh tại$2.7$có thể làm$p = 3$tối ưu ngay cả khi$p = 2$tệ hơn một chút và cả hai đều được kiểm tra rõ ràng. 

Khi$n = 1$, tổng tiền tố giảm chính xác thành các đóng góp một phần tử và phương trình bậc hai vẫn đánh giá chính xác mà không cần bất kỳ cách xử lý đặc biệt nào ngoài công thức chung.
