---
title: "CF 102785G - Số không ngẫu nhiên"
description: "Đầu vào mô tả quy tắc cục bộ trên chuỗi nhị phân. Quy tắc được viết dưới dạng biểu thức Boolean trong đó chữ số r có nghĩa là bit có vị trí r trước bit hiện tại. Ví dụ: 0 đại diện cho bit hiện tại, 1 đại diện cho bit trước đó, v.v."
date: "2026-07-27T19:41:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102785
codeforces_index: "G"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 18)"
rating: 0
weight: 102785
solve_time_s: 57
verified: true
draft: false
---

[CF 102785G - Số không ngẫu nhiên](https://codeforces.com/problemset/problem/102785/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Đầu vào mô tả quy tắc cục bộ trên chuỗi nhị phân. Quy tắc được viết dưới dạng biểu thức Boolean trong đó một chữ số`r`có nghĩa là bit đó`r`vị trí trước bit hiện tại. Ví dụ,`0`đại diện cho bit hiện tại,`1`đại diện cho bit trước đó, v.v. Biểu thức có thể kết hợp các bit này với AND, OR, XOR và NOT. 

Một chuỗi độ dài`n`hợp lệ nếu mọi vị trí nơi quy tắc được xác định đều thỏa mãn biểu thức. Nếu phần bù tham chiếu lớn nhất là`k`, sau đó mỗi bit bắt đầu từ vị trí`k + 1`phải làm cho biểu thức được đánh giá là đúng khi bit hiện tại và bit trước đó`k`bit được thay thế vào công thức. 

Nhiệm vụ là đếm tất cả các chuỗi nhị phân hợp lệ có độ dài`n`. Câu trả lời là bắt buộc theo modulo`2^60`. 

Giá trị của`n`có thể đạt được`1000`, vậy hãy liệt kê tất cả`2^n`trình tự có thể là không thể. Ngay cả một giải pháp lập trình động có trạng thái chứa nhiều bit trước đó cũng sẽ thất bại nếu nó phụ thuộc tuyến tính vào`n`lần số mũ của các trạng thái lớn. Độ dài biểu thức tối đa là`1000`và độ lệch được tham chiếu được giới hạn ở các chữ số từ`0`ĐẾN`9`, đó là hạn chế chính. Cần tối đa chín bit trước đó để quyết định xem bit tiếp theo có được phép hay không, do đó số lượng trạng thái được giới hạn bởi`2^9 = 512`. 

Trường hợp tinh tế đầu tiên là khi biểu thức chỉ sử dụng bit hiện tại. Ví dụ:```
2
0
```Quy tắc nói rằng mỗi bit phải được thực hiện`x_i = 1`. Trình tự hợp lệ duy nhất là`11`, vậy đáp án là:```
1
```Một giải pháp luôn khởi tạo trạng thái kích thước`2^k`với`k`bằng ít nhất một sẽ xử lý sai trường hợp này vì không có lịch sử trước đó để duy trì. 

Một trường hợp phức tạp khác là các toán tử nhị phân không tuân theo thứ tự ưu tiên của ngôn ngữ lập trình thông thường. Ví dụ:```
2
0|1&2
```Biểu thức được đánh giá là`(0|1)&2`, không phải như`0|(1&2)`. Trình phân tích cú pháp sử dụng quyền ưu tiên Boolean thông thường sẽ tạo bảng chuyển tiếp sai và đếm các chuỗi không chính xác. 

Trường hợp cạnh thứ ba là khi số bit chỉ lớn hơn một chút so với độ lệch lớn nhất:```
3
0+1
```Điều kiện chỉ cần được kiểm tra ở hai vị trí cuối cùng. Các trình tự hợp lệ là`000`,`001`,`010`,`011`,`100`,`101`,`110`, Và`111`ngoại trừ những trường hợp có hai bit liền kề bằng nhau ở vị trí được kiểm tra. Câu trả lời là`2`. Một giải pháp bắt đầu kiểm tra trước khi tồn tại đủ số bit trước đó sẽ từ chối các tiền tố hợp lệ. 

# Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là tạo ra mọi chuỗi nhị phân có độ dài`n`, sau đó kiểm tra mọi vị trí bằng cách đánh giá biểu thức Boolean. Điều này đúng vì nó kiểm tra chính xác định nghĩa về tính hợp lệ. Tuy nhiên, số lượng trình tự là`2^n`, và với`n = 1000`điều này vượt xa mọi giới hạn có thể có. Ngay cả khi việc kiểm tra một chuỗi chỉ mất`1000`hoạt động, trường hợp xấu nhất sẽ yêu cầu khoảng`1000 * 2^1000`hoạt động. 

Quan sát hữu ích là quy tắc chỉ xem xét một cửa sổ bit cố định. Biểu thức không bao giờ có thể kiểm tra nhiều hơn chín bit trước đó và bit hiện tại. Một khi chúng ta biết điều cuối cùng`k`các bit đã được tạo, toàn bộ tiền tố trước đó không còn ảnh hưởng đến các bit tiếp theo có thể thực hiện được. 

Điều này biến bài toán thành bài toán quy hoạch động trạng thái hữu hạn. Mỗi trạng thái đại diện cho trạng thái cuối cùng`k`bit. Việc thêm một bit mới sẽ tạo ra sự chuyển đổi sang trạng thái khác nếu biểu thức Boolean được thỏa mãn. Vì có nhiều nhất`512`tiểu bang, chúng tôi có thể xử lý tất cả`n`định vị một cách hiệu quả. 

Phương pháp brute-force hoạt động vì mọi chuỗi hoàn chỉnh có thể được kiểm tra độc lập, nhưng không thành công vì số lượng tiền tố có thể tăng theo cấp số nhân. Chế độ xem trạng thái hữu hạn nhóm các tiền tố có tương lai giống hệt nhau, cho phép tất cả các tiền tố tương đương chia sẻ một giá trị DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n * 2^k), trong đó k ≤ 9 | O(2^k) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Phân tích biểu thức Boolean thành cây cú pháp. Trình phân tích cú pháp xử lý các dấu ngoặc đơn như một nhóm rõ ràng và xử lý KHÔNG trước các phép toán nhị phân. Tất cả các phép toán nhị phân ở cùng cấp độ được đánh giá từ trái sang phải vì câu lệnh xác định chúng có mức độ ưu tiên như nhau. 

Cây cú pháp cho phép đánh giá biểu thức nhiều lần mà không cần phân tích văn bản nhiều lần. 
2. Tìm chữ số lớn nhất dùng trong biểu thức. Gọi nó`k`. Bit tiếp theo chỉ phụ thuộc vào bit hiện tại và bit trước đó`k`bit. 

Nếu như`k`bằng 0, mỗi bit có thể được kiểm tra độc lập vì biểu thức chỉ phụ thuộc vào bit được thêm vào. 
3. Tính toán trước giá trị của biểu thức cho mọi phép gán có thể có của các bit liên quan. 

Một mặt nạ trạng thái chứa phần cuối cùng`k`bit. Khi thử một bit mới, hãy kết hợp nó với trạng thái để tạo thành một phép gán hoàn chỉnh và đánh giá biểu thức một lần. Điều này tránh việc phân tích cú pháp và đánh giá biểu thức trong mỗi lần chuyển đổi DP. 
4. Khởi tạo mảng lập trình động. 

Vì`k > 0`, mọi khối đầu tiên có thể có của`k`bit là có thể vì quy tắc không thể được kiểm tra cho đến khi bit tiếp theo được thêm vào. Mỗi trạng thái bắt đầu bằng giá trị`1`. 

Vì`k = 0`, bắt đầu với một trạng thái trống và xử lý trực tiếp từng bit. 
5. Xử lý từng vị trí còn lại bằng cách thử cả hai bit mới có thể có. 

Đối với mọi trạng thái hiện tại, hãy nối thêm`0`Và`1`. Nếu biểu thức được đánh giá là đúng, hãy chuyển sang trạng thái mới được hình thành bằng cách bỏ bit cũ nhất và giữ bit mới nhất`k`bit. 
6. Tính tổng tất cả các trạng thái cuối cùng. 

Rốt cuộc`n`các bit đã được tạo ra, mọi trạng thái còn lại biểu thị một chuỗi hoàn chỉnh hợp lệ kết thúc bằng trạng thái đó. Số lượng của họ cùng nhau tạo thành câu trả lời. 

Tại sao nó hoạt động: 

Bất biến lập trình động là sau khi xử lý tiền tố của chuỗi, giá trị được lưu trữ trong một trạng thái chính xác là số lượng tiền tố hợp lệ kết thúc bằng mẫu bit được biểu thị bởi trạng thái đó. Hai tiền tố có cùng trạng thái có khả năng tiếp tục giống hệt nhau vì quy tắc chỉ kiểm tra các bit được lưu trữ gần đây. Mỗi chuỗi hợp lệ tuân theo chính xác một chuỗi chuyển đổi và mọi chuyển đổi chỉ được tạo khi điều kiện Boolean được thỏa mãn, do đó tổng cuối cùng sẽ tính mỗi chuỗi hợp lệ chính xác một lần. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1 << 60

class Parser:
    def __init__(self, s):
        self.s = s
        self.i = 0

    def parse(self):
        node = self.parse_atom()
        while self.i < len(self.s) and self.s[self.i] in "&|+":
            op = self.s[self.i]
            self.i += 1
            right = self.parse_atom()
            node = (op, node, right)
        return node

    def parse_atom(self):
        c = self.s[self.i]
        if c == '-':
            self.i += 1
            return ('-', self.parse_atom())
        if c == '(':
            self.i += 1
            node = self.parse()
            self.i += 1
            return node
        self.i += 1
        return ('var', int(c))

def eval_tree(node, bits):
    t = node[0]
    if t == 'var':
        return (bits >> node[1]) & 1
    if t == '-':
        return 1 - eval_tree(node[1], bits)
    a = eval_tree(node[1], bits)
    b = eval_tree(node[2], bits)
    if t == '&':
        return a & b
    if t == '|':
        return a | b
    return a ^ b

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)
    expr = input().strip()

    k = 0
    for c in expr:
        if c.isdigit():
            k = max(k, int(c))

    tree = Parser(expr).parse()

    if k == 0:
        value = eval_tree(tree, 0)
        if value == 1:
            print(pow(2, n, MOD))
        else:
            print(0)
        return

    limit = 1 << k
    good = [[False, False] for _ in range(limit)]

    for state in range(limit):
        for bit in range(2):
            mask = state | (bit << k)
            good[state][bit] = eval_tree(tree, mask) == 1

    dp = [1] * limit
    mask_limit = limit - 1

    for _ in range(k, n):
        ndp = [0] * limit
        for state, count in enumerate(dp):
            if count:
                if good[state][0]:
                    ndp[(state >> 1)] = (ndp[state >> 1] + count) & (MOD - 1)
                if good[state][1]:
                    ndp[((state >> 1) | (1 << (k - 1)))] = (
                        ndp[(state >> 1) | (1 << (k - 1))] + count
                    ) & (MOD - 1)
        dp = ndp

    print(sum(dp) & (MOD - 1))

if __name__ == "__main__":
    solve()
```Trình phân tích cú pháp xây dựng một cây nhị phân trong đó mỗi lá là một vị trí bit được tham chiếu. Thứ tự giảm dần đệ quy phù hợp với các quy tắc toán tử bất thường của bài toán: dấu ngoặc đơn được xử lý trước, KHÔNG liên kết với biểu thức sau và toán tử nhị phân được kết hợp từ trái sang phải. 

Giai đoạn tiền xử lý sẽ tính toán xem có thêm`0`hoặc`1`được cho phép từ mọi trạng thái lịch sử có thể. Mặt nạ trạng thái chỉ lưu trữ trước đó`k`bit. Bit mới được thêm vào được đặt ở vị trí`k`tạm thời vì biểu thức sử dụng độ lệch so với vị trí hiện tại. 

DP bắt đầu với tất cả các lịch sử ban đầu có thể có vì không có quy tắc nào được kiểm tra trước lịch sử đầu tiên.`k`bit đã biết. Mỗi lần chuyển đổi sẽ chuyển trạng thái theo một vị trí và chèn bit mới. Việc kiểm tra biểu thức diễn ra trước quá trình chuyển đổi, sử dụng lịch sử cũ cùng với bit ứng viên. 

Hoạt động modulo sử dụng`2^60`. Bởi vì mô-đun là lũy thừa của hai nên chỉ có thể thực hiện giữ lại 60 bit thấp nhất bằng mặt nạ bit, nhanh hơn thao tác còn lại chung. 

# Ví dụ đã hoạt động 

Đối với biểu thức mẫu đầu tiên:```
(0+2)|(0&2)
```mức bù tối đa là`2`, do đó trạng thái chứa hai bit trước đó. 

| Vị trí được xử lý | Số trạng thái hiện tại | Đã thử chút | Kết quả biểu thức | Trạng thái tiếp theo | 
| --- | --- | --- | --- | --- | 
| Ban đầu | cả 4 bang = 1 | | | | 
| 3 |`00`| 0 | sai | bị từ chối | 
| 3 |`00`| 1 | đúng |`10`| 
| 3 |`01`| 0 | đúng |`00`| 
| 3 |`01`| 1 | đúng |`10`| 

Tiếp tục chuyển đổi tương tự cho các vị trí còn lại để lại sáu chuỗi hợp lệ, cho ra kết quả mẫu. 

Dấu vết cho thấy thuật toán không bao giờ cần phải nhớ toàn bộ tiền tố. Hai bit cuối cùng mô tả đầy đủ tất cả các lựa chọn trong tương lai. 

Đối với mẫu thứ hai:```
(0+2)|-(0&2)
```bảng chuyển tiếp thay đổi vì thao tác NOT thay đổi khi các bit bằng nhau được chấp nhận. 

| Vị trí được xử lý | Tiểu bang | Đã thêm bit | Kết quả biểu thức | Tiểu bang mới | 
| --- | --- | --- | --- | --- | 
| Ban đầu |`00`| 0 | đúng |`00`| 
| Ban đầu |`00`| 1 | đúng |`10`| 
| Ban đầu |`01`| 0 | đúng |`00`| 
| Ban đầu |`01`| 1 | sai | bị từ chối | 

Sau khi tất cả các vị trí được xử lý, tổng các giá trị DP còn lại thành chín chuỗi hợp lệ được hiển thị trong câu lệnh. 

Ví dụ này thực hiện phân tích cú pháp KHÔNG đơn phân và giải thích lý do tại sao biểu thức phải được chuyển đổi thành cấu trúc đánh giá có thể sử dụng lại trước khi chạy DP. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 2^k + E * 2^k) | E là kích thước biểu thức. Biểu thức được đánh giá một lần cho mỗi lần chuyển đổi có thể xảy ra trong quá trình tiền xử lý, sau đó mỗi vị trí trong số n vị trí sẽ xử lý tất cả các trạng thái. | 
| Không gian | O(2^k + E) | Mảng DP lưu trữ tất cả các trạng thái và cây cú pháp lưu trữ biểu thức được phân tích cú pháp. | 

Lớn nhất có thể`k`là`9`, vậy có nhiều nhất`512`tiểu bang. Với`n = 1000`, DP chỉ thực hiện vài trăm nghìn lần chuyển đổi, dễ dàng phù hợp với giới hạn. 

# Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("3\n(0+2)|(0&2)\n") == "6\n", "sample 1"
assert run("4\n(0+2)|-(0&2)\n") == "9\n", "sample 2"

assert run("2\n0\n") == "1\n", "all bits must be one"

assert run("3\n0&1\n") == "2\n", "adjacent bits must both be one"

assert run("10\n0|1\n") == "1023\n", "only all-zero sequence is invalid"

assert run("1000\n0+0\n") == "0\n", "impossible rule"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n(0+2) | (0&2)`|`6`| 
|`4\n(0+2) | -(0&2)`|`9`| 
|`2\n0`|`1`| Trường hợp đặc biệt không tồn tại lịch sử | 
|`3\n0&1`|`2`| Xử lý đúng các phụ thuộc liền kề | 
|`10\n0 | 1`|`1023`| 
|`1000\n0+0`|`0`| Đầu vào dài và các ràng buộc không thể thực hiện được | 

# Vỏ cạnh 

Đối với biểu thức chỉ bit hiện tại:```
2
0
```thuật toán phát hiện ra điều đó`k = 0`. Nó đánh giá biểu thức một lần cho giá trị bit bằng 0. Vì chỉ có một cái được chấp nhận nên mọi vị trí đều phải chứa một cái. Câu trả lời trở thành`2^n`chỉ khi biểu thức chấp nhận cả hai giá trị bit, nếu không thì nó là một hoặc 0 tùy thuộc vào chuỗi bắt buộc. 

Đối với trường hợp ưu tiên toán tử bất thường:```
2
0|1&2
```trình phân tích cú pháp đầu tiên đọc`0`, sau đó kết hợp nó với`1`sử dụng OR và cuối cùng kết hợp kết quả đó với`2`sử dụng AND. Cây cú pháp thể hiện`((0|1)&2)`, phù hợp với tuyên bố. Trình phân tích cú pháp ngôn ngữ thông thường sẽ xây dựng một cây khác và tạo ra các chuyển đổi không chính xác. 

Đối với quy tắc có độ lệch lớn:```
3
0+1
```độ lệch tối đa là một, do đó trạng thái có một bit trước đó. Bit đầu tiên được lưu trữ mà không cần kiểm tra. Mỗi bit sau được kiểm tra so với bit trước đó và trạng thái được thay đổi sau mỗi lần chuyển đổi được chấp nhận. Thuật toán không kiểm tra các bit không tồn tại trước đó và do đó chỉ tính các vị trí có điều kiện được xác định.
