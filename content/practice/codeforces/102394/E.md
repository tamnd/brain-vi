---
title: "CF 102394E - Đổi Quà"
description: "Chúng tôi bắt đầu với một loạt các loại quà tặng. Vị trí (i) ban đầu chứa (gi). Chúng tôi có thể tùy ý trao đổi quà tặng giữa các thí sinh, vì vậy dãy cuối cùng có thể là bất kỳ hoán vị nào của nhiều loại quà tặng ban đầu."
date: "2026-08-10T19:06:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "E"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 96
verified: true
draft: false
---

[CF 102394E - Đổi quà](https://codeforces.com/problemset/problem/102394/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một loạt các loại quà tặng. Vị trí (i) ban đầu chứa (g_i). Chúng tôi có thể tùy ý trao đổi quà tặng giữa các thí sinh, vì vậy dãy cuối cùng có thể là bất kỳ hoán vị nào của nhiều loại quà tặng ban đầu. 

Một thí sinh rất vui khi giá trị được đặt ở vị trí của họ sau khi trao đổi khác với giá trị mà họ có ban đầu. Nhiệm vụ là tối đa hóa số lượng các vị trí như vậy. 

Khó khăn là mảng cuối cùng (g=s_n) không được đưa ra một cách rõ ràng. Một chuỗi có thể được viết trực tiếp với tối đa (10^6) giá trị hoặc được hình thành bằng cách nối hai chuỗi được xác định trước đó. Một chuỗi có thể được sử dụng lại nhiều lần nên độ dài thực tế của nó có thể đạt tới (10^{18}). Bản thân đầu vào nhỏ hơn nhiều so với trình tự mà nó mô tả. 

Đơn giản hóa đầu tiên là bỏ qua thứ tự của mảng. Giả sử loại quà tặng thường xuyên nhất xảy ra (M) lần và toàn bộ mảng có độ dài (L). Nếu (M\le L/2) chúng ta có thể sắp xếp quà sao cho mỗi vị trí nhận một loại khác nhau, mang lại cho (L) thí sinh vui vẻ. Nếu (M>L/2), có (M) vị trí ban đầu nắm giữ loại ưu thế đó, nhưng chỉ có (L-M) quà thuộc loại khác có sẵn để chuyển vào chúng. Ít nhất (M-(L-M)=2M-L) trong số các vị trí đó phải giữ cùng loại. Tất cả các vị trí khác có thể được thực hiện khác nhau. Như vậy câu trả lời là 

[ 
\min(L,2(L-M)). 
] 

Tương đương, 

[ 
\text{answer}= 
\bắt đầu{trường hợp} 
L,&2M\le L,\ 
2(L-M),&2M>L. 
\end{trường hợp} 
] 

Vì vậy, toàn bộ vấn đề đã được rút gọn thành việc tìm hai đại lượng cho (s_n): độ dài của nó (L) và tần số lớn nhất (M) của bất kỳ loại quà tặng nào. 

Các ràng buộc làm cho việc mở rộng (s_n) là không thể. Số lượng định nghĩa trình tự nhiều nhất là (10^6) và tổng số giá trị được liệt kê rõ ràng trong tất cả các trường hợp thử nghiệm cũng nhiều nhất là (10^6), nhưng một chuỗi được tạo có thể có độ dài (10^{18}). Ngay cả việc quét tuyến tính của mảng mở rộng cũng có thể yêu cầu (10^{18}) thao tác. Cách tiếp cận bậc hai bị loại trừ sớm hơn nhiều, vì ngay cả (10^6) đối tượng đầu vào cũng đã làm cho (10^{12}) hoạt động không thể thực hiện được. Thuật toán dự định phải hoạt động trên mô tả được nén và dành thời gian tỷ lệ thuận với kích thước đầu vào. 

Có một số trường hợp đặc biệt trong đó việc triển khai trực tiếp có thể âm thầm thất bại. 

Đối với một phần tử duy nhất, thí sinh không được nhận quà khác. Ví dụ,```
1
1
1 1 7
```có câu trả lời (0). Một công thức cho rằng luôn có đủ quà tặng khác để di chuyển sẽ cho rằng thí sinh có thể trở nên hạnh phúc một cách không chính xác. 

Tất cả những món quà giống hệt nhau là một thái cực khác. Vì```
1
1
1 4 5 5 5 5
```câu trả lời là (0). Mọi hoán vị đều giống hệt với mảng ban đầu. Giải pháp chỉ kiểm tra xem mảng có chứa nhiều hơn một giá trị riêng biệt hay không là không đủ, vì đại lượng quan trọng là tần số tối đa. 

Ranh giới nửa tần số chính xác cũng có vấn đề. Vì```
1
1
1 4 1 1 2 2
```mỗi giá trị xảy ra hai lần nên mọi vị trí đều có thể nhận giá trị còn lại và câu trả lời là (4). Sử dụng sự so sánh chặt chẽ với (L/2) sai hướng có thể khiến một số thí sinh không hài lòng. 

Cuối cùng, các tham chiếu trình tự lặp lại phải được tính bằng bội số của chúng. Coi như```
1
3
1 1 1
2 1 1
2 2 2
```Chuỗi cuối cùng có (4) bản sao của (1), mặc dù chỉ có một lần xuất hiện rõ ràng trong đầu vào. Câu trả lời của nó là (0). Việc triển khai chỉ xử lý mỗi định nghĩa trình tự một lần mà không ghi lại số lần nó được tham chiếu bởi trình tự cuối cùng sẽ mắc lỗi này. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đầu tiên hãy mở rộng mọi phép nối cho đến khi có sẵn mảng (g) hoàn chỉnh. Sau đó đếm số lần xuất hiện của từng loại quà tặng, tìm tần suất tối đa (M) và sử dụng công thức trên để tính số lượng thí sinh hạnh phúc tối đa. 

Điều này đúng vì việc trao đổi quà tặng không bao giờ thay đổi nhiều giá trị. Khi chúng ta biết tần số của mọi giá trị, cấu trúc ban đầu của mảng không còn quan trọng nữa. 

Vấn đề là kích thước của mảng mở rộng. Một chuỗi như```
1 1 7
2 1 1
2 2 2
2 3 3
...
```có thể tăng gấp đôi chiều dài ở mỗi bước. Chỉ sau 60 lần nối, độ dài có thể là (2^{60}), lớn hơn (10^{18}). Việc mở rộng một mảng như vậy sẽ yêu cầu khoảng (10^{18}) thao tác phần tử, mặc dù dữ liệu đầu vào chỉ chứa vài chục dòng. 

Quan sát quan trọng là phép nối chỉ thêm tần số. Nếu một chuỗi (s_i) được sử dụng một lần trong chuỗi cuối cùng thì mọi phần tử của (s_i) sẽ đóng góp một lần. Nếu nó được sử dụng ba lần thì mọi phần tử của (s_i) đều đóng góp ba lần. 

Điều này đưa ra cách giải thích hữu ích về các định nghĩa trình tự dưới dạng biểu đồ tuần hoàn có hướng. Dãy số loại 2 (s_i=s_x+s_y) có các cạnh tới (s_x) và (s_y). Bắt đầu từ (s_n), chúng ta có thể truyền ngược bội số. Gọi (w_i) là số lần toàn bộ chuỗi (s_i) xuất hiện trong (s_n) mở rộng. Ban đầu (w_n=1). Bất cứ khi nào (s_i=s_x+s_y), mỗi lần xuất hiện của (s_i) sẽ tạo ra một lần xuất hiện của cả (s_x) và (s_y), vì vậy 

[ 
w_x\mathrel{+}=w_i,\qquad 
w_y\mathrel{+}=w_i. 
] 

Bởi vì mọi chuỗi được tham chiếu đều có chỉ mục nhỏ hơn nên việc xử lý các chỉ số từ (n) xuống (1) là đủ. 

Khi chúng ta đạt đến một chuỗi được chỉ định trực tiếp chứa các giá trị (q_1,\ldots,q_k), mỗi giá trị rõ ràng đó sẽ xuất hiện (w_i) lần trong chuỗi cuối cùng. Chúng ta có thể thêm (w_i) vào tần số chung của từng giá trị. Tổng chiều dài có thể được tích lũy cùng lúc với (w_i\cdot k). 

Điều này biến nhiệm vụ bất khả thi là duyệt tối đa (10^{18}) phần tử thành việc duyệt chỉ các phần tử được cung cấp rõ ràng và định nghĩa trình tự. Đây là nguyên tắc đánh dấu ngược tương tự được sử dụng trong giải pháp tham chiếu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(L+n)), (L\le 10^{18}) | (O(L)) | Quá chậm | 
| Tối ưu | (O(n+\sum k)) dự kiến ​​| (O(n+\tổng k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các định nghĩa về trình tự. Đối với một chuỗi được chỉ định trực tiếp, hãy lưu trữ các giá trị của nó. Để ghép nối, hãy lưu trữ hai chỉ số trình tự được tham chiếu. Chúng tôi giữ lại các định nghĩa vì các chuỗi thực tế có thể lớn về mặt thiên văn. 
2. Tạo một mảng (w) bội số và đặt (w_n=1). Điều này có nghĩa là chuỗi cuối cùng hiện chứa một bản sao của (s_n). 
3. Chỉ số trình tự xử lý từ (n) đến (1). Nếu (w_i=0), chuỗi (s_i) không đóng góp vào câu trả lời cuối cùng nên có thể bỏ qua. 
4. Nếu (s_i) là một phép nối (s_x+s_y), hãy thêm (w_i) vào cả (w_x) và (w_y). Mỗi lần xuất hiện của (s_i) chứa chính xác một lần xuất hiện của mỗi chuỗi con. 
5. Nếu (s_i) là một chuỗi được liệt kê rõ ràng với các giá trị (k), hãy thêm (w_i) vào tần số của mọi giá trị được liệt kê. Đồng thời thêm (w_i k) vào tổng chiều dài. Vì chuỗi chỉ được xử lý khi bội số của nó đã được xác định nên mọi lần xuất hiện rõ ràng đều được tính chính xác bằng số lần nó xuất hiện trong (s_n). 
6. Theo dõi tần số toàn cầu lớn nhất (M) trong khi tích lũy các giá trị. Không cần phải xây dựng lại mảng cuối cùng. 
7. Sau khi tất cả các lá liên quan đã được xử lý, gọi (L) là tổng chiều dài. Nếu (2M\le L), trả lời (L). Nếu không thì trả lời (2(L-M)). Trường hợp đầu tiên có nghĩa là mỗi vị trí có thể nhận được một loại khác nhau. Trong trường hợp thứ hai, giá trị vượt trội có quá nhiều bản sao nên chính xác các vị trí (2M-L) buộc phải giữ lại. 

### Tại sao nó hoạt động 

Bất biến là ngay trước khi xử lý chuỗi (s_i), (w_i) bằng số lần (s_i) xảy ra trong chuỗi cuối cùng được mở rộng hoàn toàn (s_n). Ban đầu nó đúng vì (s_n) xảy ra một lần. Đối với một phép nối (s_i=s_x+s_y), mỗi lần xuất hiện của (s_i) sẽ đóng góp một lần xuất hiện của mỗi con, do đó, việc thêm (w_i) vào cả hai con sẽ bảo toàn tính bất biến. Tại một lá, nhân mọi giá trị được liệt kê rõ ràng với (w_i) do đó sẽ đóng góp chính xác số lần xuất hiện thực sự của nó trong chuỗi cuối cùng.

Sau khi tất cả các lá đã được xử lý, (L) và mọi tần suất quà tặng đều chính xác. Số lượng tối thiểu các vị trí không thay đổi là (0) khi tần số lớn nhất nhiều nhất là một nửa mảng, còn ngược lại là (2M-L), bởi vì chỉ tồn tại (L-M) các quà tặng không chiếm ưu thế để chiếm ưu thế (M). Do đó đáp án tính toán được là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        n = int(input())

        kind = bytearray(n + 1)
        left = [0] * (n + 1)
        right = [0] * (n + 1)
        leaves = [None] * (n + 1)

        for i in range(1, n + 1):
            parts = list(map(int, input().split()))

            if parts[0] == 1:
                kind[i] = 1
                k = parts[1]
                leaves[i] = parts[2:]
            else:
                left[i] = parts[1]
                right[i] = parts[2]

        # ways[i] = number of times sequence i occurs in s_n.
        ways = [0] * (n + 1)
        ways[n] = 1

        freq = {}
        total = 0
        maximum = 0

        for i in range(n, 0, -1):
            w = ways[i]
            if w == 0:
                continue

            if kind[i] == 1:
                values = leaves[i]
                total += w * len(values)

                for v in values:
                    nv = freq.get(v, 0) + w
                    freq[v] = nv
                    if nv > maximum:
                        maximum = nv

                # Release the large list as soon as it is no longer needed.
                leaves[i] = None
            else:
                x = left[i]
                y = right[i]
                ways[x] += w
                ways[y] += w

        if maximum * 2 <= total:
            answer = total
        else:
            answer = 2 * (total - maximum)

        out.append(str(answer))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`kind`,`left`, Và`right`mảng lưu trữ DAG đã nén.`leaves[i]`chỉ được điền cho các chuỗi được chỉ định trực tiếp, do đó không có chuỗi được tạo nào được hiện thực hóa. 

các`ways`array thực hiện việc truyền ngược lại từ chuỗi cuối cùng. Thứ tự từ (n) xuống (1) là hợp lệ vì mọi phép nối chỉ đề cập đến các chuỗi trước đó. Do đó, đứa trẻ sẽ nhận được tất cả những đóng góp của mình trước khi thuật toán đến được với đứa trẻ đó. 

Từ điển`freq`lưu trữ tần suất của từng loại quà tặng thực tế. Từ điển là cần thiết vì giá trị quà tặng có thể lớn tới (10^9) và không phù hợp để lập chỉ mục trực tiếp. Bản thân tần số có thể lớn bằng (10^{18}), vì vậy số nguyên Python rất hữu ích ở đây. Trong C++, những đại lượng này cần số nguyên 64 bit. Các ràng buộc chính thức rõ ràng cho phép độ dài chuỗi lên tới (10^{18}). 

biểu thức`maximum * 2 <= total`xử lý chính xác nửa ranh giới một cách chính xác. Khi tần số tối đa bằng chính xác một nửa tổng tần số, mọi vị trí vẫn có thể được thay đổi. 

Danh sách một lá được giải phóng sau khi xử lý nó. Điều này không thay đổi mức sử dụng bộ nhớ tiệm cận nhưng nó làm giảm áp lực bộ nhớ tối đa trong quá trình truyền tải ngược. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên bao gồm một chuỗi được đưa ra rõ ràng:```
[3, 3, 2, 1, 3]
```Truyền tải ngược lại chỉ có một nút. 

| Trình tự | Đa bội | Giá trị lá | Tổng chiều dài | Tần số tối đa | 
| --- | --- | --- | --- | --- | 
| (s_1) | 1 | 3, 3, 2, 1, 3 | 5 | 3 | 

Loại quà tặng (3) xảy ra ba lần. Vì (2M=6>5), một vị trí buộc phải giữ loại (3), trong khi bốn vị trí còn lại có thể nhận loại khác. Câu trả lời là 

[ 
2(5-3)=4. 
] 

Điều này thể hiện trường hợp tần số vượt trội. 

### Mẫu 2 

Các định nghĩa là```
s1 = [3, 3, 2]
s2 = [2, 2, 3, 3]
s3 = s1 + s2
```Chúng tôi bắt đầu với (w_3=1). 

| Trình tự xử lý | Đa bội | Hành động | Tổng chiều dài | Tần số tối đa | 
| --- | --- | --- | --- | --- | 
| (s_3) | 1 | Thêm 1 vào (w_1,w_2) | 0 | 0 | 
| (s_2) | 1 | Thêm một 2 và hai 3 | 4 | 2 | 
| (s_1) | 1 | Thêm hai số 3 và một số 2 | 7 | 4 | 

Tần số cuối cùng là (3\mapsto4) và (2\mapsto3). Do đó (L=7) và (M=4). Vì (2M=8>7), 

[ 
\text{answer}=2(7-4)=6. 
] 

Dấu vết chứng minh rằng bản thân phép nối không bao giờ cần phải được xây dựng. Chỉ có sự đa dạng của nó được truyền bá đến con cái của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+\sum k)) dự kiến ​​| Mỗi định nghĩa trình tự được xử lý một lần và mọi giá trị được liệt kê rõ ràng đều được tính một lần. Các hoạt động từ điển được mong đợi (O(1)). | 
| Không gian | (O(n+\tổng k)) | Biểu đồ trình tự nén, giá trị lá, bội số và từ điển tần số được lưu trữ. | 

Trên tất cả các trường hợp thử nghiệm, cả (n) và tổng số giá trị được liệt kê rõ ràng đều bị giới hạn bởi (10^6). Do đó, thuật toán chỉ thực hiện công việc tuyến tính ở kích thước đầu vào thực tế, ngay cả khi chuỗi được biểu diễn có độ dài gần bằng (10^{18}). Giới hạn bộ nhớ 512 MB cũng đủ cho cách trình bày này. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
import sys
import io

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """1
1
1 5 3 3 2 1 3
"""
) == "4", "sample 1"

# Provided sample 2
assert run(
    """1
3
1 3 3 3 2
1 4 2 2 3 3
2 1 2
"""
) == "6", "sample 2"

# Minimum-size case
assert run(
    """1
1
1 1 42
"""
) == "0", "single element"

# All values are equal
assert run(
    """1
1
1 5 7 7 7 7 7
"""
) == "0", "all equal"

# Exactly half of each value
assert run(
    """1
1
1 4 1 1 2 2
"""
) == "4", "exact half boundary"

# Dominant value
assert run(
    """1
1
1 4 1 1 1 2
"""
) == "2", "dominant frequency"

# Reused sequence
assert run(
    """1
3
1 1 1
2 1 1
2 2 2
"""
) == "0", "repeated references"

# Large represented length, but tiny input.
# s_61 represents 2^60 copies of 1.
huge_lines = ["1", "61", "1 1 1"]
for i in range(2, 62):
    huge_lines.append(f"2 {i - 1} {i - 1}")

huge_input = "\n".join(huge_lines) + "\n"

assert run(huge_input) == "0", "huge implicit sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1 1 42`| 0 | Kích thước chuỗi tối thiểu | 
|`1 / 1 / 1 5 7 7 7 7 7`| 0 | Tất cả các giá trị giống nhau | 
|`1 / 1 / 1 4 1 1 2 2`| 4 | Ranh giới chính xác (M=L/2) | 
|`1 / 1 / 1 4 1 1 1 2`| 2 | Công thức tần số chiếm ưu thế | 
|`3 / 1 1 1 / 2 1 1 / 2 2 2`| 0 | Tái sử dụng một chuỗi nhiều lần | 
| Chuỗi nhân đôi 61 nút | 0 | Độ dài vượt xa kích thước đầu vào rõ ràng và số lượng quy mô 64 bit | 

## Vỏ cạnh 

Trường hợp một phần tử có (L=1) và (M=1). Thuật toán xử lý lá đơn có bội số (1), thu được`total = 1`Và`maximum = 1`và đi vào nhánh trội vì (2M>L). Câu trả lời là (2(1-1)=0), phù hợp với thực tế là không có món quà nào khác. 

Đối với đầu vào hoàn toàn bằng nhau```
1
1
1 4 5 5 5 5
```tần số duy nhất là (M=4), trong khi (L=4). Thuật toán tính toán (2M=8>4), vì vậy câu trả lời là (2(4-4)=0). Không trao đổi nào có thể thay đổi loại quà tặng của bất kỳ thí sinh nào. 

Đối với đầu vào chính xác một nửa```
1
1
1 4 1 1 2 2
```tần số là (2) và (2), do đó (L=4) và (M=2). Điều kiện (2M\le L) đúng, cho kết quả (4). Một sự sắp xếp hợp lệ là`[2, 2, 1, 1]`, khác với`[1, 1, 2, 2]`ở mọi vị trí. 

Đối với trường hợp chiếm ưu thế```
1
1
1 4 1 1 1 2
```loại (1) xảy ra ba lần và loại (2) xảy ra một lần. Chỉ tồn tại một món quà không phải (1), do đó, nhiều nhất một trong ba vị trí (1) ban đầu có thể thay đổi. Hai người còn lại phải giữ nguyên (1), đưa ra đúng hai thí sinh hạnh phúc. Thuật toán thu được (L=4), (M=3) và trả về (2(4-3)=2). 

Để tham khảo nhiều lần,```
1
3
1 1 1
2 1 1
2 2 2
```quá trình truyền tải ngược lại bắt đầu bằng (w_3=1). Đang xử lý (s_3) cho ra (w_2=2). Đang xử lý (s_2) thì cho ra (w_1=4). Chiếc lá rõ ràng`[1]`do đó được tính bốn lần, vì vậy (L=M=4) và câu trả lời là (0). Đây chính xác là lý do tại sao việc truyền bá các bội số thay vì chỉ truy cập từng chuỗi một lần là cần thiết. 

Trường hợp cạnh cuối cùng là một chuỗi ẩn rất lớn. Chuỗi nhân đôi có thể biểu thị (2^{60}) bản sao của một giá trị chỉ bằng 61 định nghĩa. Thuật toán không bao giờ cố gắng tạo ra những bản sao đó. Nó truyền ngược bội số, cuối cùng đạt tới lá đơn phần tử có trọng số là (2^{60}). Các số nguyên có độ chính xác tùy ý của Python biểu thị chính xác số đếm đó và vì mọi phần tử đều có cùng giá trị nên câu trả lời cuối cùng vẫn là (0).
