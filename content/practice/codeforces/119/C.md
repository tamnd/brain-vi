---
title: "CF 119C - Cải cách giáo dục"
description: "Chúng tôi có tới 50 đối tượng. Mỗi chủ đề có ba thuộc tính. Khoảng $[ai, bi]$ mô tả số lượng bài tập về nhà mà môn học này có thể giao. Chúng ta có thể tự do lựa chọn bất kỳ giá trị nào trong khoảng đó. Giá trị $ci$ là độ phức tạp của chủ đề."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 119
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 90"
rating: 2000
weight: 119
solve_time_s: 179
verified: false
draft: false
---

[CF 119C - Cải cách giáo dục](https://codeforces.com/problemset/problem/119/C) 

**Xếp hạng:** 2000 
**Thẻ:** dp 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có tới 50 đối tượng. Mỗi chủ đề có ba thuộc tính. 

Khoảng thời gian$[a_i, b_i]$mô tả số lượng bài tập về nhà mà chủ đề này có thể giao. Chúng ta có thể tự do lựa chọn bất kỳ giá trị nào trong khoảng đó. 

giá trị$c_i$là độ phức tạp của chủ đề. Thời khóa biểu phải sử dụng các môn học theo thứ tự phức tạp tăng dần nên nếu chọn môn học$p_1, p_2, \dots, p_n$, sau đó:$$c_{p_1} < c_{p_2} < \dots < c_{p_n}$$Việc tính bài tập về nhà cũng phải tuân theo một quy tắc chuyển tiếp đặc biệt. Nếu ngày hiện tại có$x$bài tập thì ngày hôm sau phải có:$$x + k$$hoặc$$x \cdot k$$Trong số tất cả các lịch trình hợp lệ chính xác$n$môn học, chúng tôi muốn môn học có tổng số bài tập tối đa. 

Điều đầu tiên quan trọng trong các ràng buộc là kích thước của các khoảng. Bản thân các giá trị có thể lớn bằng$10^{16}$, vì vậy việc đếm bài tập về nhà một cách thô bạo là không thể. Nhưng độ rộng khoảng thỏa mãn:$$b_i - a_i \le 100$$Điều này thay đổi mọi thứ. Mặc dù giá trị rất lớn nhưng mỗi môn học chỉ cho phép tối đa 101 bài tập về nhà riêng biệt. Điều đó có nghĩa là không gian trạng thái thực sự rất nhỏ. 

Hạn chế quan trọng thứ hai là$m \le 50$. Một giải pháp có độ phức tạp xung quanh$50^3 \cdot 100^2$là hoàn toàn an toàn. 

Phần khó nhất của vấn đề là nhận ra rằng chúng ta không nên nghĩ về các số nguyên tùy ý. Chúng tôi chỉ quan tâm đến các giá trị thực sự xuất hiện bên trong khoảng chủ đề nào đó. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả định$k = 1$. Sau đó, hai chuyển đổi được phép trở thành:$$x_i = x_{i-1} + 1$$hoặc$$x_i = x_{i-1}$$Việc triển khai bất cẩn có thể cho rằng phép nhân luôn tăng giá trị và vô tình tạo ra các chu kỳ hoặc chuyển tiếp không hợp lệ. 

Ví dụ:```
2 2 1
5 5 1
5 5 2
```Câu trả lời đúng là:```
YES
1 5
2 5
```vì phép nhân với 1 giữ giá trị không đổi. 

Một trường hợp nguy hiểm khác là sự phức tạp trùng lặp. Các môn có độ phức tạp như nhau không thể cùng xuất hiện trong thời gian biểu. 

Ví dụ:```
2 3 2
1 10 5
1 10 5
2 20 6
```Chúng ta chỉ được chọn một trong hai môn đầu tiên. 

Giá trị lớn cũng tạo ra bẫy tràn trong các ngôn ngữ có số nguyên có chiều rộng cố định. 

Ví dụ:```
2 2 100
10000000000000000 10000000000000000 1
10000000000000100 10000000000000100 2
```Việc chuyển đổi là hợp lệ vì:$$10^{16} + 100 = 10000000000000100$$Sử dụng số nguyên 32 bit sẽ phá vỡ hoàn toàn trường hợp này. 

Một vấn đề tinh tế hơn xuất hiện khi tối đa hóa số tiền một cách tham lam. Việc chọn số lượng bài tập về nhà lớn nhất tại địa phương có thể cản trở quá trình chuyển đổi trong tương lai. 

Ví dụ:```
3 3 2
1 5 1
6 6 2
8 20 3
```Nếu chúng ta tham lam chọn 5 vào ngày đầu tiên thì giá trị tiếp theo phải là 7 hoặc 10, cả hai giá trị này đều không phù hợp với chủ đề 2. Trình tự hợp lệ tối ưu là:```
4 -> 6 -> 8
```với tổng số 18. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con của$n$môn học, mọi thứ tự hợp lệ theo độ phức tạp và mọi bài tập về nhà đều được tính trong mỗi khoảng thời gian. 

Mỗi môn học có tối đa 101 giá trị bài tập về nhà có thể có. Ngay cả khi chúng tôi sửa các chủ đề, điều đó đã mang lại khoảng:$$101^n$$các trình tự có thể. Với$n = 50$, cái này lớn về mặt thiên văn. 

Brute-force hoạt động về mặt khái niệm vì quy tắc chuyển đổi có tính chất cục bộ. Khi biết số bài tập về nhà của ngày hôm qua, chúng ta có thể xác định được khả năng của ngày hôm nay. Vấn đề đơn giản là hệ số phân nhánh quá lớn. 

Quan sát quan trọng là mọi giá trị bài tập về nhà có thể sử dụng được phải thuộc về một khoảng nào đó.$[a_i, b_i]$và mỗi khoảng chứa tối đa 101 số. Vì có tối đa 50 môn học nên tổng số giá trị bài tập về nhà riêng biệt trên toàn bộ dữ liệu đầu vào nhiều nhất là:$$50 \cdot 101 = 5050$$Nó đủ nhỏ để lập trình động. 

Chúng ta có thể nghĩ về mỗi trạng thái như sau:$$(dp[i][v])$$ý nghĩa: 

"Chúng ta kết thúc ở chủ đề$i$sử dụng số bài tập về nhà$v$." 

Từ một trạng thái, chúng ta có thể chuyển sang trạng thái khác nếu: 

1. Chủ đề tiếp theo có độ phức tạp lớn hơn. 
2. Số bài tập về nhà tiếp theo bằng hoặc$v + k$hoặc$v \cdot k$. 
3. Giá trị tiếp theo nằm trong khoảng chủ đề tiếp theo. 

Vì mục tiêu là tối đa hóa tổng số tiền nên DP sẽ lưu trữ tổng số tiền tốt nhất có thể đạt được. 

Vấn đề trở thành DP kiểu đường đi dài nhất trên biểu đồ chu kỳ có hướng. Thứ tự phức tạp đảm bảo tính chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O\left(\binom{m}{n} \cdot 101^n\right)$| Hàm mũ | Quá chậm | 
| DP tối ưu |$O(m^2 \cdot 101^2)$|$O(m \cdot 101)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các chủ đề và sắp xếp chúng theo mức độ phức tạp. 

Việc sắp xếp là cần thiết vì thời gian biểu hợp lệ phải sử dụng độ phức tạp tăng dần. Sau khi được sắp xếp, mọi chuyển đổi hợp lệ sẽ tiếp tục trong mảng. 
2. Đối với mỗi môn học, hãy liệt kê tất cả số bài tập về nhà có thể có trong khoảng thời gian đó. 

Vì mỗi chiều rộng khoảng tối đa là 100, nên điều này tạo ra tối đa 101 giá trị cho mỗi chủ đề. 
3. Xác định trạng thái DP. 

Cho phép:$$dp[i][t]$$thể hiện tổng số bài tập tối đa cho một chuỗi hợp lệ kết thúc ở chủ đề$i$sử dụng$t$-giá trị thứ từ khoảng của nó. 
4. Khởi tạo tất cả các chuỗi một chủ đề. 

Bất kỳ môn học nào cũng có thể bắt đầu thời gian biểu, vì vậy:$$dp[i][t] = value$$ban đầu. 
5. Hãy thử chuyển tiếp giữa từng cặp đối tượng. 

Đối với môn học$j \to i$, quá trình chuyển đổi chỉ được phép nếu:$$c_j < c_i$$Sau đó với mỗi giá trị bài tập về nhà$x$chủ đề$j$, tính toán hai giá trị tiếp theo duy nhất có thể có:$$x + k$$Và$$x \cdot k$$Nếu một trong hai giá trị thuộc về chủ đề$i$khoảng thời gian của nó, chúng tôi có thể cập nhật DP. 
6. Lưu trữ con trỏ gốc. 

Bất cứ khi nào quá trình chuyển đổi cải thiện giá trị DP, hãy ghi lại trạng thái trước đó. Điều này cho phép xây dựng lại thời gian biểu sau này. 
7. Theo dõi độ dài chuỗi riêng biệt. 

Chúng tôi cần chính xác$n$đối tượng, không chỉ đơn thuần là số tiền tối đa. Vì vậy, mỗi trạng thái DP cũng lưu trữ số lượng đối tượng đã được sử dụng. 
8. Trong số tất cả các trạng thái sử dụng chính xác$n$môn học, chọn môn có tổng số tiền tối đa. 
9. Xây dựng lại câu trả lời bằng cách sử dụng con trỏ gốc. 
10. Nếu không có trạng thái nào đạt đến độ dài$n$, in`"NO"`. 

### Tại sao nó hoạt động 

DP xử lý mọi trạng thái cuối cùng hợp lệ chính xác một lần. 

Thời gian biểu hợp lệ kết thúc ở chủ đề$i$và giá trị$v$phải đến từ chính xác một số bài tập về nhà trước đó:$$v - k$$hoặc một số giá trị thỏa mãn:$$v = previous \cdot k$$Các quy tắc chuyển tiếp có tính chất cục bộ, do đó việc mở rộng thời gian biểu từng phần tối ưu luôn duy trì tính tối ưu. Bởi vì độ phức tạp tăng lên nghiêm trọng nên các quá trình chuyển đổi tạo thành một DAG và không xuất hiện sự phụ thuộc theo chu kỳ. 

Bất biến DP là:$$dp[i][t]$$lưu trữ tổng số bài tập tối đa trong số tất cả các chuỗi hợp lệ kết thúc ở trạng thái chính xác đó. 

Mỗi thời gian biểu hợp pháp tương ứng với một đường dẫn trong biểu đồ DP và mỗi đường dẫn DP tương ứng với một thời gian biểu hợp pháp. Vì chúng tôi tối đa hóa trên tất cả các đường dẫn có độ dài$n$, thuật toán đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    subjects = []
    for idx in range(1, m + 1):
        a, b, c = map(int, input().split())
        vals = list(range(a, b + 1))
        subjects.append((c, idx, vals))

    subjects.sort()

    # dp[i][t] = best total sum
    # len_dp[i][t] = length of sequence
    dp = []
    len_dp = []
    parent = []

    for _, _, vals in subjects:
        sz = len(vals)
        dp.append(vals[:])
        len_dp.append([1] * sz)
        parent.append([None] * sz)

    for i in range(m):
        ci, _, vals_i = subjects[i]

        pos_i = {}
        for t, v in enumerate(vals_i):
            pos_i[v] = t

        for j in range(i):
            cj, _, vals_j = subjects[j]

            if cj >= ci:
                continue

            for pj, x in enumerate(vals_j):
                candidates = [x + k, x * k]

                for y in candidates:
                    if y not in pos_i:
                        continue

                    pi = pos_i[y]

                    new_len = len_dp[j][pj] + 1
                    new_sum = dp[j][pj] + y

                    if new_len > len_dp[i][pi]:
                        len_dp[i][pi] = new_len
                        dp[i][pi] = new_sum
                        parent[i][pi] = (j, pj)

                    elif new_len == len_dp[i][pi] and new_sum > dp[i][pi]:
                        dp[i][pi] = new_sum
                        parent[i][pi] = (j, pj)

    best = None
    best_sum = -1

    for i in range(m):
        vals = subjects[i][2]

        for t, v in enumerate(vals):
            if len_dp[i][t] == n and dp[i][t] > best_sum:
                best_sum = dp[i][t]
                best = (i, t)

    if best is None:
        print("NO")
        return

    ans = []

    cur = best
    while cur is not None:
        i, t = cur
        _, idx, vals = subjects[i]
        ans.append((idx, vals[t]))
        cur = parent[i][t]

    ans.reverse()

    print("YES")
    for x in ans:
        print(*x)

solve()
```Lựa chọn triển khai chính đầu tiên là lưu trữ các giá trị bài tập về nhà rõ ràng cho mọi môn học. Vì độ rộng khoảng không bao giờ vượt quá 100 nên điều này vẫn nhỏ gọn và tránh việc nén tọa độ phức tạp. 

DP giữ hai phần thông tin riêng biệt.`len_dp[i][t]`lưu trữ có bao nhiêu chủ đề được sử dụng theo trình tự tốt nhất kết thúc ở trạng thái đó.`dp[i][t]`lưu trữ tổng số tiền tương ứng. 

Thứ tự so sánh rất quan trọng. Chúng tôi luôn ưu tiên độ dài chuỗi lớn hơn trước tiên vì bài toán yêu cầu chính xác$n$môn học. Trong số các chuỗi có độ dài bằng nhau, chúng tôi tối đa hóa tổng số bài tập. 

Một chi tiết triển khai tinh tế là xử lý các kết quả chuyển tiếp trùng lặp. Khi$k = 1$, hai chuyển tiếp:```
x + k
x * k
```có thể trở thành các giá trị liên tiếp như`x + 1`Và`x`hoặc thậm chí trùng lặp giá trị trong các tình huống đặc biệt. Mã xử lý việc này một cách tự nhiên vì cả hai đều được kiểm tra độc lập. 

các`pos_i`từ điển đưa ra$O(1)$tra cứu xem giá trị đích có tồn tại trong khoảng tiếp theo hay không. Nếu không có nó, mọi chuyển đổi sẽ yêu cầu quét liên tục tới 101 giá trị. 

Việc xây dựng lại cha mẹ chỉ lưu trữ trạng thái DP trước đó. Vì các quá trình chuyển đổi tạo thành một DAG được sắp xếp theo độ phức tạp nên việc tái thiết rất đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5 2
1 10 1
1 10 2
1 10 3
1 20 4
1 100 5
```Một con đường tối ưu là:```
8 -> 10 -> 20 -> 40
```| Bước | Chủ đề | Bài tập về nhà | Chuyển tiếp | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 8 | bắt đầu | 8 | 
| 2 | 3 | 10 |$8 + 2$| 18 | 
| 3 | 4 | 20 |$10 \times 2$| 38 | 
| 4 | 5 | 40 |$20 \times 2$| 78 | 

Dấu vết cho thấy tại sao lập trình động là cần thiết. Một chuỗi có thể xen kẽ giữa các chuyển đổi cộng và chuyển đổi nhân, vì vậy các quyết định cục bộ tham lam là không đáng tin cậy. 

### Ví dụ 2 

đầu vào:```
3 3 2
1 5 1
6 6 2
8 20 3
```| Bước | Chủ đề | Bài tập về nhà | Chuyển tiếp | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | bắt đầu | 4 | 
| 2 | 2 | 6 |$4 + 2$| 10 | 
| 3 | 3 | 8 |$6 + 2$| 18 | 

Thay vào đó, nếu chúng ta bắt đầu từ 5 thì giá trị tiếp theo sẽ phải là 7 hoặc 10, cả hai giá trị này đều không phù hợp với chủ đề 2. Ví dụ này chứng minh rằng lựa chọn cục bộ lớn nhất có thể phá hủy tính khả thi trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m^2 \cdot 101)$| Mỗi cặp đối tượng đều được kiểm tra và mỗi đối tượng có tối đa 101 giá trị | 
| Không gian |$O(m \cdot 101)$| Bảng DP lưu trữ trạng thái cho mọi giá trị khoảng | 

Với$m \le 50$, tổng số trạng thái là rất nhỏ. Giải pháp dễ dàng nằm gọn trong giới hạn 1 giây và sử dụng rất ít bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    def solve():
        n, m, k = map(int, input().split())

        subjects = []
        for idx in range(1, m + 1):
            a, b, c = map(int, input().split())
            vals = list(range(a, b + 1))
            subjects.append((c, idx, vals))

        subjects.sort()

        dp = []
        len_dp = []
        parent = []

        for _, _, vals in subjects:
            sz = len(vals)
            dp.append(vals[:])
            len_dp.append([1] * sz)
            parent.append([None] * sz)

        for i in range(m):
            ci, _, vals_i = subjects[i]

            pos_i = {v: t for t, v in enumerate(vals_i)}

            for j in range(i):
                cj, _, vals_j = subjects[j]

                if cj >= ci:
                    continue

                for pj, x in enumerate(vals_j):
                    for y in (x + k, x * k):
                        if y not in pos_i:
                            continue

                        pi = pos_i[y]

                        new_len = len_dp[j][pj] + 1
                        new_sum = dp[j][pj] + y

                        if new_len > len_dp[i][pi]:
                            len_dp[i][pi] = new_len
                            dp[i][pi] = new_sum
                            parent[i][pi] = (j, pj)

                        elif new_len == len_dp[i][pi] and new_sum > dp[i][pi]:
                            dp[i][pi] = new_sum
                            parent[i][pi] = (j, pj)

        best = None
        best_sum = -1

        for i in range(m):
            vals = subjects[i][2]

            for t, v in enumerate(vals):
                if len_dp[i][t] == n and dp[i][t] > best_sum:
                    best_sum = dp[i][t]
                    best = (i, t)

        if best is None:
            print("NO")
            return

        ans = []

        cur = best
        while cur is not None:
            i, t = cur
            _, idx, vals = subjects[i]
            ans.append((idx, vals[t]))
            cur = parent[i][t]

        ans.reverse()

        print("YES")
        for x in ans:
            print(*x)

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run(
"""4 5 2
1 10 1
1 10 2
1 10 3
1 20 4
1 100 5
"""
).startswith("YES")

# minimum case
assert run(
"""1 1 5
10 10 1
"""
) == "YES\n1 10\n"

# impossible case
assert run(
"""2 2 10
1 1 1
2 2 2
"""
) == "NO\n"

# k = 1
assert run(
"""2 2 1
5 5 1
5 5 2
"""
) == "YES\n1 5\n2 5\n"

# equal complexities
assert run(
"""2 3 2
1 1 5
3 3 5
5 5 6
"""
) == "NO\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chủ đề đơn | CÓ với chủ đề đó | Xử lý kích thước tối thiểu | 
| Chuyển tiếp bất khả thi | KHÔNG | Phát hiện tính khả thi thích hợp | 
|$k = 1$| Giá trị bài tập về nhà được phép bằng nhau | Xử lý phép nhân đúng | 
| Độ phức tạp bằng nhau | KHÔNG | Thứ tự phức tạp nghiêm ngặt | 

## Vỏ cạnh 

Hãy xem xét trường hợp:```
2 2 1
5 5 1
5 5 2
```Thuật toán khởi tạo cả hai trạng thái với độ dài 1. Từ giá trị 5, các chuyển đổi là:$$5 + 1 = 6$$Và$$5 \times 1 = 5$$Quá trình chuyển đổi thứ hai tồn tại trong khoảng tiếp theo, do đó DP mở rộng chuỗi đến độ dài 2. Đầu ra trở thành:```
YES
1 5
2 5
```Điều này xác nhận việc triển khai xử lý chính xác phép nhân không tăng khi$k = 1$. 

Bây giờ hãy xem xét sự phức tạp trùng lặp:```
2 3 2
1 10 5
1 10 5
2 20 6
```Trong quá trình chuyển đổi, thuật toán sẽ kiểm tra rõ ràng:```
if cj >= ci:
    continue
```Vì vậy, việc chuyển đổi giữa hai chủ đề đầu tiên bị cấm vì cả hai đều có độ phức tạp là 5. Vì không tồn tại chuỗi có độ dài 2 hợp lệ nên thuật toán sẽ xuất ra:```
NO
```Cuối cùng, hãy xem xét một cái bẫy tham lam:```
3 3 2
1 5 1
6 6 2
8 20 3
```DP khám phá tất cả các giá trị bài tập về nhà bắt đầu từ 1 đến 5. 

Bắt đầu từ số 5 sẽ thất bại ngay lập tức vì cả số 7 và số 10 đều không thuộc khoảng thứ hai. 

Bắt đầu từ 4 thành công:$$4 \to 6 \to 8$$DP giữ chuỗi hợp lệ này và trả về tổng số tối ưu. Điều này chứng tỏ tại sao việc khám phá toàn bộ không gian trạng thái compact là cần thiết.
