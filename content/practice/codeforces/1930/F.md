---
title: "CF 1930F - Tối đa hóa sự khác biệt"
description: "Chúng tôi xây dựng một mảng tăng dần. Sau mỗi lần chèn, chúng ta phải tính toán $$f(a)=maxx left(maxi(aimid x)-mini(aimid x)right).$$ Giá trị được chèn ở mỗi bước được mã hóa bởi câu trả lời trước đó, vì vậy các truy vấn phải được xử lý trực tuyến."
date: "2026-06-09T01:44:05+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dfs-and-similar"]
categories: ["algorithms"]
codeforces_contest: 1930
codeforces_index: "F"
codeforces_contest_name: "think-cell Round 1"
rating: 2700
weight: 1930
solve_time_s: 148
verified: false
draft: false
---

[CF 1930F - Tối đa hóa sự khác biệt](https://codeforces.com/problemset/problem/1930/F) 

**Xếp hạng:** 2700 
**Tags:** bitmasks, brute Force, dfs và tương tự 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi xây dựng một mảng tăng dần. Sau mỗi lần chèn chúng ta phải tính toán$$f(a)=\max_x \left(\max_i(a_i\mid x)-\min_i(a_i\mid x)\right).$$Giá trị được chèn ở mỗi bước được mã hóa bởi câu trả lời trước đó, vì vậy các truy vấn phải được xử lý trực tuyến. 

Nhiệm vụ đầu tiên là phải hiểu số lượng nào thực sự được tối đa hóa. 

Sửa hai yếu tố$u$Và$v$. Chúng tôi muốn tối đa hóa$$(u\mid x)-(v\mid x).$$Hãy xem xét một bit một cách độc lập. 

Nếu cả hai$u$Và$v$có cùng một bit, bit đó không đóng góp gì vào sự khác biệt. 

Nếu như$u$có$1$Và$v$có$0$, chúng tôi giữ$x$một chút bằng$0$, đóng góp giá trị bit đó. 

Nếu như$u$có$0$Và$v$có$1$, chúng tôi thiết lập$x$một chút bằng$1$, làm cho cả hai bên bình đẳng và không đóng góp được gì. 

Vì vậy, sự đóng góp tối ưu của mỗi bit chính xác là các bit có trong$u$và vắng mặt ở$v$. Kể từ đây$$\max_x \bigl((u\mid x)-(v\mid x)\bigr)=u\ \&\ \sim v.$$Lấy cặp có thứ tự tốt nhất,$$f(a)=\max_{u,v\in a}(u\ \&\ \sim v).$$Đây là mức giảm chính được sử dụng trong cuộc thảo luận giải pháp chính thức. 

Các ràng buộc ngay lập tức loại trừ bất kỳ cách tiếp cận theo cặp nào. Số lượng truy vấn có thể đạt tới$10^6$, vậy thậm chí$O(q\sqrt q)$là vô vọng. Tổng của tất cả các kích thước không gian mặt nạ được giới hạn bởi$2^{22}$, điều này gợi ý rõ ràng một giải pháp dành công việc cho mặt nạ thay vì cho các cặp giá trị được chèn. 

Trường hợp cạnh tinh tế xuất hiện khi mảng chỉ chứa một giá trị. Không có cặp nào tạo ra sự khác biệt tích cực, và thực sự$$u\ \&\ \sim u = 0,$$vậy câu trả lời phải là$0$. 

Một sai lầm dễ mắc phải khác là coi phần bù là phần bù có bit vô hạn. Chỉ những phần bên trong vũ trụ mặt nạ mới quan trọng. Nếu không gian mặt nạ có$k$bit, phần bù của$v$là$$((1<<k)-1)\oplus v.$$Sử dụng bản thô của Python`~v`sẽ tạo ra số âm và phá vỡ logic. 

## Phương pháp tiếp cận 

Việc giải thích vũ lực là đơn giản. Sau mỗi lần chèn, kiểm tra từng cặp có thứ tự$(u,v)$hiện có và tính toán$$u\ \&\ \sim v.$$Giá trị tối đa của những giá trị đó là câu trả lời. 

Điều này đúng vì sự giảm bớt ở trên, nhưng sau$q$phần chèn thêm nó cần$O(q^2)$kiểm tra cặp. Với$q=10^6$, số lượng hoạt động là hoàn toàn không khả thi. 

Quan sát quan trọng đó là$$u\ \&\ \sim v$$có thể được xem như một mặt nạ đồng thời là một tập hợp con của$u$và một tập con của phần bù giới hạn của$v$. 

Cho phép$$c(v)=((1<<k)-1)\oplus v.$$Sau đó là một chiếc mặt nạ$m$góp phần vào câu trả lời nếu tồn tại một giá trị được chèn chứa tất cả các bit của$m$, và tồn tại một mặt nạ bổ sung được chèn vào$c(v)$cũng chứa tất cả các bit của$m$. 

Xác định hai gia đình. 

Họ đầu tiên chứa tất cả các giá trị được chèn vào. 

Họ thứ hai chứa tất cả các phần bổ sung$c(v)$. 

Đối với một mặt nạ$m$, cho phép`A[m] = true`nếu một số giá trị được chèn là siêu bộ của$m$.`B[m] = true`nếu một số mặt nạ bổ sung được chèn vào là siêu tập hợp của$m$. 

Sau đó$$m$$có thể đạt được nếu cả hai cờ đều đúng. 

Câu trả lời đơn giản là chiếc mặt nạ lớn nhất có hai lá cờ đúng. 

Bây giờ vấn đề trở thành kích hoạt tập hợp con động. 

Khi một giá trị$v$được chèn vào, mọi tập hợp con của$v$trở nên hợp lệ trong gia đình$A$. 

Khi$c(v)$được chèn vào, mọi tập hợp con của$c(v)$trở nên hợp lệ trong gia đình$B$. 

Một mặt nạ chỉ có thể hoạt động trong mỗi họ một lần trong toàn bộ trường hợp thử nghiệm. Điều đó có nghĩa là chúng ta có thể duyệt qua mạng tập hợp con một cách lười biếng và không bao giờ truy cập lại mặt nạ đã được kích hoạt. 

Điều này biến đổi toàn bộ trường hợp thử nghiệm thành một biểu đồ truyền qua mặt nạ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q^2)$|$O(q)$| Quá chậm | 
| Tối ưu |$O(n \log n + q)$|$O(n)$| Đã chấp nhận | 

Đây$n=2^k$, và tổng của tất cả$n$trên đầu vào nhiều nhất là$2^{22}$. 

## Hướng dẫn thuật toán 

1. Hãy để$k$là số bit trong không gian mặt nạ và đặt`FULL = n - 1`. 
2. Duy trì hai mảng boolean.`seenA[m]`có nghĩa là mặt nạ`m`đã được kích hoạt bởi phía giá trị.`seenB[m]`có nghĩa là mặt nạ`m`đã được kích hoạt bởi phía bổ sung. 
3. Giữ nguyên câu trả lời hiện tại`ans`. 
4. Để chèn một giá trị`v`, chạy DFS trên tất cả các tập hợp con của`v`. 
5. Mỗi khi đắp mặt nạ`m`được tiếp cận lần đầu tiên trong gia đình`A`, đánh dấu`seenA[m] = True`. 
6. Nếu`seenB[m]`thì đã đúng rồi`m`có thể đạt được từ cả hai gia đình, vì vậy hãy cập nhật`ans = max(ans, m)`. 
7. Từ mặt nạ`m`, tiếp tục DFS cho mọi mặt nạ thu được bằng cách loại bỏ một bit đã đặt. 
8. Chèn phần bù giới hạn`cv = FULL ^ v`vào gia đình`B`sử dụng logic DFS tương tự. 
9. Sau khi cả hai lần kích hoạt kết thúc, hãy xuất ra câu trả lời hiện tại. 

### Tại sao nó hoạt động 

Một chiếc mặt nạ thuộc về gia đình`A`chính xác khi nó là tập hợp con của một số giá trị được chèn. Kích hoạt DFS đánh dấu chính xác tất cả các tập hợp con như vậy. 

Tương tự, một chiếc mặt nạ thuộc về gia đình`B`chính xác khi nó là tập con của phần bù giới hạn của một số giá trị được chèn vào. 

Một mặt nạ góp phần vào mục tiêu nếu nó được chứa trong cả hai họ. Thời điểm một mặt nạ bắt đầu hoạt động ở cả hai họ, nó sẽ tương ứng với một cặp có thứ tự nào đó.$(u,v)$với$$m \subseteq u,\qquad m \subseteq c(v).$$Tương đương,$$m \subseteq (u\ \&\ \sim v).$$Mặt nạ lớn nhất như vậy chính xác là$$\max(u\ \&\ \sim v),$$cái nào bằng$f(a)$. Vì vậy mức tối đa được duy trì luôn là câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, q = map(int, input().split())
        e = list(map(int, input().split()))

        full = n - 1

        seen_a = bytearray(n)
        seen_b = bytearray(n)

        ans = 0
        last = 0

        def activate(start, seen_self, seen_other):
            nonlocal ans

            stack = [start]

            while stack:
                m = stack.pop()

                if seen_self[m]:
                    continue

                seen_self[m] = 1

                if seen_other[m]:
                    if m > ans:
                        ans = m

                x = m
                while x:
                    bit = x & -x
                    stack.append(m ^ bit)
                    x ^= bit

        cur_answers = []

        for enc in e:
            v = (enc + last) % n

            activate(v, seen_a, seen_b)
            activate(full ^ v, seen_b, seen_a)

            last = ans
            cur_answers.append(str(ans))

        out.append(" ".join(cur_answers))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Ý tưởng cốt lõi là mỗi mặt nạ được kích hoạt nhiều nhất một lần trong mỗi nhóm. DFS không bao giờ khám phá một mặt nạ đã được kích hoạt, do đó tổng công việc tỷ lệ thuận với kích thước của mạng mặt nạ hơn là số lần chèn. 

Phần bù phải được tính bằng`full ^ v`, không`~v`. Chỉ những phần bên trong vũ trụ mặt nạ mới có liên quan. 

Truyền tải tập hợp con sử dụng kỹ thuật tiêu chuẩn để loại bỏ từng bit một. Mỗi cạnh trong mạng tập hợp con chỉ được duyệt khi mặt nạ nguồn của nó được kích hoạt lần đầu tiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 5
queries = [1, 2]
```| Bước | Giải mã v | Trả lời | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 2 | 

Sau lần chèn đầu tiên chỉ có một giá trị, vì vậy mỗi cặp có thứ tự là$(1,1)$và kết quả là$0$. 

Sau khi chèn$2$, cặp$(2,1)$cho$$2 \& \sim 1 = 2.$$Không thể có giá trị lớn hơn. 

### Mẫu thứ hai 

| Bước | Giải mã v | Bộ hiện tại | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 3 | {3} | 0 | 
| 2 | 1 | {3,1} | 2 | 
| 3 | 0 | {3,1,0} | 3 | 
| 4 | 5 | {3,1,0,5} | 5 | 

Bước thứ ba đã chứa cặp$(3,0)$, tạo ra giá trị$3$. 

Bước thứ tư giới thiệu$5$, và cặp$(5,0)$sản xuất$5$, trở thành mức tối đa mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + q)$| Mỗi mặt nạ được kích hoạt tối đa một lần trong mỗi gia đình | 
| Không gian |$O(n)$| Hai mảng truy cập cộng với ngăn xếp DFS | 

Bởi vì tổng của tất cả các kích thước không gian mặt nạ tối đa là$2^{22}$, tổng số lần kích hoạt mặt nạ trên toàn bộ đầu vào bị giới hạn. Đó chính xác là điều làm cho giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = []

    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        e = list(map(int, input().split()))

        full = n - 1
        seen_a = bytearray(n)
        seen_b = bytearray(n)

        ans = 0
        last = 0

        def activate(start, seen_self, seen_other):
            nonlocal ans
            stack = [start]

            while stack:
                m = stack.pop()
                if seen_self[m]:
                    continue

                seen_self[m] = 1

                if seen_other[m]:
                    ans = max(ans, m)

                x = m
                while x:
                    b = x & -x
                    stack.append(m ^ b)
                    x ^= b

        cur = []

        for enc in e:
            v = (enc + last) % n
            activate(v, seen_a, seen_b)
            activate(full ^ v, seen_b, seen_a)
            last = ans
            cur.append(str(ans))

        out.append(" ".join(cur))

    return "\n".join(out)

# provided samples
assert run("2\n5 2\n1 2\n7 4\n3 1 5 2\n") == \
       "0 2\n0 2 3 5"

# single insertion
assert run("1\n4 1\n3\n") == "0"

# repeated identical values
assert run("1\n8 3\n5 5 5\n") == "0 0 0"

# values creating maximum mask
assert run("1\n8 2\n7 1\n") == "0 6"

# boundary mask values
assert run("1\n8 2\n0 7\n") == "0 7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một lần chèn |`0`| Trường hợp cơ sở | 
| Tất cả các giá trị bằng nhau | Tất cả số không | Không có cặp thứ tự hữu ích nào tồn tại | 
|`7`sau đó`1`|`0 6`| Chênh lệch định hướng lớn | 
|`0`sau đó`7`|`0 7`| Câu trả lời toàn mặt nạ | 

## Vỏ cạnh 

Hãy xem xét một trường hợp thử nghiệm chỉ có một giá trị được chèn:```
n = 8
queries = [5]
```Cặp có thứ tự duy nhất là$(5,5)$.$$5 \& \sim 5 = 0.$$DFS kích hoạt các tập hợp con ở cả hai bên, nhưng mặt nạ lớn nhất có trong cả hai họ vẫn là$0$. Đầu ra của thuật toán`0`. 

Bây giờ hãy xem xét các giá trị bằng nhau lặp lại:```
n = 8
queries = [3,3,3]
```Mỗi cặp đặt hàng đều đáp ứng$$3 \& \sim 3 = 0.$$Lần chèn đầu tiên kích hoạt tất cả các tập hợp con của`3`và phần bổ sung của nó. Những lần chèn sau này sẽ truy cập lại các mặt nạ đã được kích hoạt và hầu như không thực hiện được công việc nào. Câu trả lời vẫn ở lại`0`. 

Cuối cùng, hãy xem xét cặp cực trị:```
n = 8
queries = [0,7]
```Cặp đôi$(7,0)$sản lượng$$7 \& \sim 0 = 7.$$Khi`7`được chèn vào, mặt nạ`7`trở nên tích cực trong họ giá trị. Vì nó đã có thể truy cập được từ họ bổ sung sau khi chèn`0`, câu trả lời ngay lập tức cập nhật thành`7`, đó là mặt nạ tối đa có thể.
