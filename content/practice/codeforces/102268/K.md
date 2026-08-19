---
title: "CF 102268K - Kiến thức"
description: "Chúng ta có một chuỗi nhị phân trên a và b. Các thao tác được phép chèn hoặc xóa một trong ba khối đặc biệt: aa, bbb hoặc ababab. Vì mọi thao tác có thể được đảo ngược bằng cách thực hiện thao tác chèn hoặc xóa tương ứng nên các thao tác xác định các lớp tương đương của chuỗi."
date: "2026-08-19T04:45:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102268
codeforces_index: "K"
codeforces_contest_name: "300iq Contest 1"
rating: 0
weight: 102268
solve_time_s: 751
verified: false
draft: false
---

[CF 102268K - Kiến thức](https://codeforces.com/problemset/problem/102268/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi nhị phân trên`a`Và`b`. Các thao tác được phép chèn hoặc xóa một trong ba khối đặc biệt:`aa`,`bbb`, hoặc`ababab`. Vì mọi thao tác có thể được đảo ngược bằng cách thực hiện thao tác chèn hoặc xóa tương ứng nên các thao tác xác định các lớp tương đương của chuỗi. Nhiệm vụ là xác định có bao nhiêu chuỗi riêng biệt có độ dài chính xác`x`thuộc cùng lớp tương đương với chuỗi đã cho`s`. 

Các ràng buộc được phân chia có chủ ý giữa một chuỗi đầu vào lớn và độ dài mục tiêu khổng lồ. Chuỗi gốc có thể chứa 300.000 ký tự, do đó việc xử lý chuỗi này về cơ bản phải tuyến tính. Đồng thời,`x`có thể`10^9`, loại trừ mọi lập trình động được lập chỉ mục theo độ dài và mọi mô phỏng thực hiện một chuyển đổi cho mỗi ký tự của chuỗi đích. Giới hạn thời gian một giây làm cho công việc siêu tuyến tính vừa phải trên chuỗi ban đầu trở nên không mong muốn, vì vậy thách thức chính là nén quan hệ tương đương thành một số trạng thái không đổi. Tuyên bố chính thức đưa ra`n <= 300000`,`x <= 10^9`và giới hạn thời gian một giây. 

Có một số trường hợp đặc biệt bộc lộ những cách hiểu sai phổ biến. Nếu đầu vào là```
1
a
0
```câu trả lời là`0`. Mục tiêu là chuỗi trống, nhưng`a`không thể giảm được vì cũng không`aa`,`bbb`, cũng không`ababab`xảy ra. Việc triển khai bất cẩn chỉ nhìn vào chênh lệch độ dài có thể cho rằng không chính xác rằng việc loại bỏ hai ký tự luôn có thể thực hiện được. 

Vì```
2
aa
0
```câu trả lời là`1`, vì chuỗi đã cho có thể rút gọn trực tiếp thành chuỗi trống. Điều này cũng kiểm tra xem các hoạt động bằng 0 có được phép không và độ dài đó`0`là mục tiêu chính đáng. 

Vì```
3
bbb
2
```câu trả lời là`1`. Chuỗi nguồn biểu diễn cùng lớp tương đương với chuỗi trống vì`bbb`có thể bị xóa và chỉ trong số hai chuỗi có độ dài`aa`đại diện cho lớp đó. Một giải pháp chỉ theo dõi độ dài có thể sẽ bỏ sót sự thật rằng việc sắp xếp thực tế các chữ cái rất quan trọng. 

Mối quan hệ`ababab = empty`cũng có vấn đề độc lập với hai mối quan hệ ngắn hơn. Vì```
6
ababab
3
```nguồn tương đương với chuỗi trống và chuỗi có độ dài ba duy nhất trong lớp đó là`bbb`, vậy câu trả lời là`1`. Việc bỏ qua mối quan hệ sáu ký tự sẽ phân loại nguồn không chính xác thành không trống. Những ví dụ này minh họa tại sao vấn đề lại nằm ở các lớp từ tương đương chứ không chỉ đơn giản là độ dài có thể đạt được. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ liệt kê mọi chuỗi nhị phân có độ dài`x`, kiểm tra xem nó có thể được chuyển đổi từ`s`, và đếm những người thành công. Có chính xác`2^x`các chuỗi ứng cử viên, do đó ngay cả trước khi kiểm tra tính tương đương, trường hợp xấu nhất yêu cầu`2^x`ứng viên. Với`x = 10^9`, điều này hoàn toàn không thể thực hiện được. Một cách tiếp cận bạo lực tự nhiên khác là thực hiện tất cả các thao tác chèn và xóa có thể, nhưng điều đó thậm chí còn ít hữu ích hơn vì các thao tác chèn có thể làm cho các chuỗi trung gian dài tùy ý. 

Lực lượng vũ phu hoạt động vì mọi hoạt động đều bảo toàn chính xác mối quan hệ tương đương mà chúng ta quan tâm. Thất bại là bản thân các chuỗi là những đối tượng khổng lồ, trong khi các mối quan hệ có nhiều cấu trúc hơn so với biểu diễn thô của chúng. 

Quan sát quan trọng là ba mối quan hệ có thể được viết đại số như`a² = 1`,`b³ = 1`, Và`(ab)³ = 1`. 

Đây chính xác là các mối quan hệ xác định của nhóm đối xứng quay của một tứ diện, là nhóm xen kẽ`A4`. Nhóm đó chỉ có 12 yếu tố. Tương tự, tất cả các chuỗi được chia thành đúng 12 lớp tương đương. Tập đại diện chung cho các lớp này là chuỗi rỗng,`a`,`b`,`ab`,`ba`,`bb`,`aba`,`abb`,`bab`,`bba`,`babb`, Và`bbab`. Cấu trúc 12 trạng thái này cũng là cơ sở của lời giải chuẩn cho bài toán. 

Chúng ta có thể làm cho quan sát trở nên cụ thể hơn bằng cách biểu diễn hai chữ cái dưới dạng hoán vị của bốn đỉnh. Cho phép`a`là hoán vị`(0 1)(2 3)`và để`b`là`(1 2 3)`. Cả hai đều là hoán vị chẵn nên chúng nằm trong`A4`. Họ thỏa mãn`a² = 1`,`b³ = 1`, Và`(ab)³ = 1`và chúng tạo ra tất cả 12 phần tử của`A4`. 

Vì vậy, đánh giá một chuỗi có nghĩa là nhân các hoán vị tương ứng. Hai chuỗi tương đương chính xác khi chúng đánh giá cùng một phần tử nhóm. Đang thêm một trong hai`a`hoặc`b`sau đó trở thành sự chuyển tiếp giữa hai trong số 12 trạng thái này. 

Bắt đầu từ chuỗi trống, mọi chuỗi nhị phân có độ dài`x`tương ứng với một cuộc đi bộ chính xác`x`chuyển đổi trong máy tự động 12 trạng thái này. Chúng ta chỉ cần đếm xem có bao nhiêu bước đi như vậy kết thúc ở phần tử nhóm được đại diện bởi`s`. Từ`x`có thể`10^9`, lũy thừa ma trận cho biết số bước đi trong`O(12^3 log x)`thời gian. 

Chuỗi gốc được xử lý một lần để tìm phần tử nhóm của nó, tạo ra độ phức tạp hoàn toàn`O(n + 12^3 log x)`. Cách tiếp cận ma trận trạng thái không đổi tương tự được mô tả trong lời giải đã biết cho bài toán này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^x)`ứng viên, cộng với việc kiểm tra tính tương đương | Hàm mũ | Quá chậm | 
| Tối ưu |`O(n + 12^3 log x)`|`O(12^2)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mỗi chữ cái bằng hoán vị của bốn phần tử. Sử dụng`a = (0 1)(2 3)`Và`b = (1 2 3)`. Một hoán vị được lưu trữ dưới dạng một bộ gồm bốn hình ảnh của nó, do đó, thành phần hoán vị có thể được triển khai với bốn lần truy cập mảng. 

Các hoán vị được chọn thỏa mãn ba quan hệ được phép. Vì chúng tạo ra tất cả các hoán vị chẵn của bốn phần tử nên có thể truy cập chính xác 12 trạng thái nhóm. 
2. Tạo 12 phần tử nhóm bằng tìm kiếm theo chiều rộng bắt đầu từ hoán vị danh tính. Từ mọi trạng thái đã biết, nhân với`a`và bởi`b`. Lưu trữ mọi hoán vị mới được phát hiện và gán cho nó một chỉ mục trạng thái. 

Việc tạo các trạng thái thay vì mã hóa cứng tên của chúng làm cho việc triển khai trở nên độc lập với danh sách các chuỗi rút gọn cụ thể. Thực tế bắt buộc duy nhất là nhóm được tạo có 12 phần tử. 
3. Tính trạng thái tương ứng với chuỗi đầu vào`s`. Bắt đầu từ danh tính và nhân với hoán vị liên quan đến mỗi ký tự. 

Việc này cần`O(n)`time và giảm toàn bộ chuỗi gốc xuống một trong 12 trạng thái có thể. 
4. Xây dựng một`12 x 12`ma trận chuyển tiếp`M`. Bộ`M[i][j]`đến số chữ cái di chuyển trạng thái nhóm`i`sang trạng thái nhóm`j`. 

Chỉ có hai chuyển đổi đi từ mỗi trạng thái, một chuyển đổi để nối thêm`a`và một để nối thêm`b`. Nếu cả hai chữ cái đều dẫn đến cùng một trạng thái, mục nhập ma trận tương ứng sẽ trở thành`2`. 
5. Tính toán`M^x`sử dụng lũy ​​thừa nhị phân. Đối với bất kỳ tiểu bang nào`u`Và`v`, mục nhập`(M^x)[u][v]`là số chiều dài-`x`chuỗi bắt đầu tại`u`và kết thúc tại`v`. 

Chúng tôi bắt đầu từ trạng thái nhận dạng vì mọi chuỗi nhị phân được xây dựng bằng cách nối thêm các ký tự của nó vào chuỗi trống. 
6. Hãy để`target`là trạng thái của`s`và để`identity`là trạng thái chuỗi rỗng. Câu trả lời bắt buộc là`(M^x)[identity][target]`, lấy modulo`998244353`. 

Điều này đếm các chuỗi theo trình tự chữ cái thực tế của chúng chứ không chỉ theo điểm cuối của chúng. Phép nhân ma trận đếm từng chuỗi chuyển đổi riêng biệt. 

Tại sao nó hoạt động: mọi thao tác chèn hoặc xóa được phép sẽ giữ nguyên thành phần nhóm vì mỗi khối được chèn hoặc xóa sẽ đánh giá danh tính. Ngược lại, các mối quan hệ`a² = b³ = (ab)³ = 1`cho chính xác nhóm tứ diện 12 phần tử, vậy hai chuỗi nhị phân được nối với nhau bằng các phép toán cho phép một cách chính xác khi chúng biểu diễn cùng một phần tử nhóm. Ma trận chuyển tiếp ghi lại chính xác việc thêm một ký tự sẽ thay đổi phần tử đó như thế nào. Do đó, mỗi chiều dài-`x`chuỗi trong lớp tương đương của`s`tương ứng với một chiều dài-`x`đi bộ từ trạng thái nhận dạng đến`target`và mỗi bước đi như vậy tương ứng với một chuỗi nhị phân trong lớp đó. Do đó, mục nhập ma trận được tính toán bằng thuật toán chính xác là số lượng được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
K = 12

def compose(p, q):
    # p after q: (p o q)(i) = p(q(i))
    return (
        p[q[0]],
        p[q[1]],
        p[q[2]],
        p[q[3]],
    )

def build_group():
    identity = (0, 1, 2, 3)

    # a = (0 1)(2 3)
    a = (1, 0, 3, 2)

    # b = (1 2 3)
    b = (0, 2, 3, 1)

    generators = (a, b)

    states = [identity]
    index = {identity: 0}

    head = 0
    while head < len(states):
        cur = states[head]
        head += 1

        for g in generators:
            nxt = compose(cur, g)
            if nxt not in index:
                index[nxt] = len(states)
                states.append(nxt)

    return states, index, generators

def mat_mul(a, b):
    c = [[0] * K for _ in range(K)]

    for i in range(K):
        ci = c[i]
        ai = a[i]

        for k in range(K):
            x = ai[k]
            if x == 0:
                continue

            bk = b[k]
            for j in range(K):
                ci[j] = (ci[j] + x * bk[j]) % MOD

    return c

def mat_pow(a, e):
    result = [[0] * K for _ in range(K)]
    for i in range(K):
        result[i][i] = 1

    while e:
        if e & 1:
            result = mat_mul(result, a)
        a = mat_mul(a, a)
        e >>= 1

    return result

def solve():
    n = int(input())
    s = input().strip()
    x = int(input())

    states, index, generators = build_group()
    identity = states[0]

    a, b = generators

    transition = [[0] * K for _ in range(K)]

    for i, state in enumerate(states):
        to_a = index[compose(state, a)]
        to_b = index[compose(state, b)]

        transition[i][to_a] += 1
        transition[i][to_b] += 1

    target = identity
    for ch in s:
        if ch == 'a':
            target = compose(target, a)
        else:
            target = compose(target, b)

    target_index = index[target]
    powered = mat_pow(transition, x)

    print(powered[0][target_index] % MOD)

if __name__ == "__main__":
    solve()
```các`compose`hàm sử dụng quy ước`p`sau đó`q`, Vì thế`compose(cur, generator)`đại diện cho việc thêm một chữ cái mới vào bên phải của từ hiện tại. Quy ước chính xác không quan trọng miễn là nó được sử dụng nhất quán trong việc tạo trạng thái, xây dựng chuyển đổi và đánh giá`s`.`build_group`bắt đầu từ danh tính và áp dụng liên tục hai trình tạo. Ba mối quan hệ xác định đảm bảo rằng chỉ có 12 trạng thái xuất hiện. BFS chấm dứt sau khi phát hiện ra 12 hoán vị đó. 

Ma trận chuyển tiếp sử dụng các hàng cho trạng thái hiện tại và các cột cho trạng thái tiếp theo. Vì mỗi trạng thái có thể có hai ký tự tiếp theo nên mỗi hàng có tổng trọng số là hai. Quyền hạn ma trận giữ nguyên cách giải thích tương tự:`M^k[i][j]`đếm số lượng chiều dài-`k`chuỗi lấy trạng thái`i`để nêu`j`. 

Chuỗi đầu vào được đánh giá riêng thay vì được chèn vào quy trình ma trận. Điều này là cần thiết bởi vì`n`chỉ có 300.000, trong khi`x`có thể là một tỷ. Chúng tôi dành thời gian tuyến tính trên`s`, thì thời gian logarit trong`x`. 

Vòng lũy ​​thừa phải bắt đầu bằng ma trận nhận dạng. Tay cầm này`x = 0`tự động vì`M^0`là ma trận nhận dạng, vì vậy câu trả lời là`1`chính xác khi nào`s`thuộc về lớp nhận dạng. 

Số nguyên Python không bị tràn nhưng giảm từng modulo tích lũy ma trận`998244353`giữ các giá trị trung gian ở mức nhỏ và tránh sự tăng trưởng không cần thiết. Ma trận chỉ có 12 hàng và cột nên hằng số rất nhỏ. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`s = ababab`Và`x = 3`. Mối quan hệ sáu ký tự nói rằng`ababab`đại diện cho yếu tố nhận dạng. Đại số vô hướng liên quan từ phép tính ma trận là số chiều dài-`k`đi từ danh tính trở lại danh tính. 

| Chiều dài`k`| Bang đang được tính |`dp[identity]`| 
| --- | --- | --- | 
| 0 | danh tính | 1 | 
| 1 | danh tính | 0 | 
| 2 | danh tính | 1 | 
| 3 | danh tính | 1 | 

Ở độ dài hai,`aa`trở lại danh tính vì`a² = 1`. Ở độ dài ba,`bbb`trở lại danh tính vì`b³ = 1`. Số đếm kết quả là`1`, phù hợp với mẫu 

Đối với mẫu 2,`s = bbb`, do đó nguồn lại đại diện cho danh tính. Chúng tôi chỉ cần mục nhập nhận dạng sau hai lần chuyển đổi. 

| Chiều dài`k`| Bang đang được tính |`dp[identity]`| 
| --- | --- | --- | 
| 0 | danh tính | 1 | 
| 1 | danh tính | 0 | 
| 2 | danh tính | 1 | 

Từ dài hai duy nhất đại diện cho danh tính là`aa`. từ`bb`đại diện cho`b²`, đó là một ba chu kỳ không đồng nhất, trong khi`ab`Và`ba`cũng đại diện cho các yếu tố không đồng nhất. Do đó câu trả lời là`1`. 

Những dấu vết này cũng xác minh quy ước về độ dài bằng không. Ở độ dài bằng 0, chỉ tồn tại từ trống, do đó lớp nhận dạng chứa chính xác một từ có độ dài đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + 12^3 log x)`| Đánh giá`s`là tuyến tính và sử dụng lũy ​​thừa ma trận`O(log x)`phép nhân của`12 x 12`ma trận | 
| Không gian |`O(12^2)`| Chỉ có một số lượng không đổi`12 x 12`ma trận và 12 trạng thái nhóm được lưu trữ | 

Chuỗi đầu vào lớn nhất chỉ yêu cầu một lần truyền, do đó`n = 300000`ràng buộc được xử lý dễ dàng. Độ dài mục tiêu không xuất hiện dưới dạng giới hạn vòng lặp vì phép lũy thừa xử lý biểu diễn nhị phân của nó, chỉ yêu cầu khoảng 30 ma trận bình phương cho`x <= 10^9`. Kích thước ma trận không đổi làm cho phương thức đủ nhỏ một cách thoải mái cho giới hạn một giây và giới hạn bộ nhớ 256 MiB được chỉ định bởi vấn đề chính thức. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp trên được lưu dưới dạng`solution.py`. Nó hoán đổi`stdin`, gọi thực tế`solve()`chức năng, chụp`stdout`, sau đó khôi phục cả hai luồng.```python
# Save the editorial solution as solution.py before running this file.

import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution.solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("6\nababab\n3\n") == "1", "sample 1"
assert run("3\nbbb\n2\n") == "1", "sample 2"
assert run("5\nbabab\n35\n") == "866826000", "sample 3"

# Minimum-size input and x = 0.
assert run("1\na\n0\n") == "0", "single a cannot reduce to empty"

# Empty string target from aa.
assert run("2\naa\n0\n") == "1", "aa reduces to empty"

# Boundary x = 1.
assert run("1\na\n1\n") == "1", "the original string itself is reachable"

# Small transition test: the only length-2 word equivalent to ab is ab.
assert run("2\nab\n2\n") == "1", "exact group-state matching"

# Maximum n and all-equal characters.
# 300000 is divisible by 2, so a^300000 is the identity.
max_input = "300000\n" + "a" * 300000 + "\n0\n"
assert run(max_input) == "1", "maximum n and all a characters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a / 0`|`0`| tối thiểu`n`, độ dài mục tiêu bằng 0, nguồn không xác định | 
|`2 / aa / 0`|`1`| Trực tiếp`aa`hủy bỏ và xử lý danh tính | 
|`1 / a / 1`|`1`| Độ dài mục tiêu ranh giới và khả năng tiếp cận không hoạt động | 
|`2 / ab / 2`|`1`| Phân biệt các phần tử nhóm có cùng độ dài | 
|`300000 / a...a / 0`|`1`| Tối đa`n`, đầu vào hoàn toàn bằng nhau, quét tuyến tính | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là`1 / a / 0`. Thuật toán đánh giá`a`như hoán vị`(0 1)(2 3)`, vì vậy trạng thái mục tiêu của nó khác với danh tính. Từ`x = 0`, công suất ma trận là`M^0 = I`và danh tính-to-`a`mục nhập là số không. Do đó, đầu ra là`0`. 

Vì`2 / aa / 0`, đánh giá hai chữ cái cho`a² = 1`, do đó nguồn đạt đến trạng thái nhận dạng. Lại`M^0`là ma trận nhận dạng, nhưng bây giờ trạng thái được yêu cầu chính xác là trạng thái bắt đầu. Câu trả lời là`1`, đại diện cho chuỗi trống. 

Vì`3 / bbb / 2`, nguồn đánh giá là`b³ = 1`. Thuật toán yêu cầu mục nhập nhận dạng của`M²`. Có một con đường như vậy, tương ứng với`aa`, vậy câu trả lời là`1`. Điều này phát hiện các triển khai gây nhầm lẫn giữa lớp nhận dạng với các chuỗi có cùng độ dài. 

Vì`2 / ab / 2`, trạng thái nguồn là sản phẩm`ab`. Trong số bốn dây dài hai, chỉ`ab`đạt đến trạng thái cụ thể đó. Do đó, ma trận trả về`1`. Điều này chứng tỏ rằng thuật toán đếm các lớp tương đương thay vì chỉ đếm tất cả các chuỗi có độ dài tương thích. 

Đối với trường hợp kích thước tối đa với`n = 300000`Và`s = a`lặp đi lặp lại 300.000 lần, mối quan hệ lặp đi lặp lại`aa = 1`giảm mỗi cặp`a`nhân vật, để lại danh tính. Với`x = 0`, câu trả lời là`1`. Thuật toán xử lý tất cả 300.000 ký tự trong một lần truyền và sau đó không thực hiện phép nhân ma trận vì số mũ bằng 0, do đó trường hợp này trực tiếp thực hiện cả ranh giới đầu vào lớn và`x = 0`ranh giới.
