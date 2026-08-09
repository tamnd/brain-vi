---
title: "CF 102431L - Ma trận xoắn ốc"
description: "Chúng ta có một lưới các gian hàng hình chữ nhật (n lần m). Lee có thể chọn bất kỳ gian hàng nào làm điểm xuất phát và bất kỳ hướng nào trong bốn hướng ban đầu. Sau đó, mỗi lần di chuyển đều là đi thẳng hoặc rẽ phải một bước rồi đi tiếp."
date: "2026-08-08T23:53:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102431
codeforces_index: "L"
codeforces_contest_name: "2019 China Collegiate Programming Contest Final (CCPC-Final 2019)"
rating: 0
weight: 102431
solve_time_s: 138
verified: true
draft: false
---

[CF 102431L - Ma trận xoắn ốc](https://codeforces.com/problemset/problem/102431/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới các gian hàng hình chữ nhật (n \times m). Lee có thể chọn bất kỳ gian hàng nào làm điểm xuất phát và bất kỳ hướng nào trong bốn hướng ban đầu. Sau đó, mỗi lần di chuyển đều là đi thẳng hoặc rẽ phải một bước rồi đi tiếp. Không bao giờ được phép rẽ trái và chỉ được phép rẽ phải một chuỗi mỗi lần giữa các ô được truy cập liên tiếp. 

Nhiệm vụ là đếm các thứ tự riêng biệt trong đó tất cả các ô (nm) có thể được truy cập chính xác một lần. Hai bước đi được coi là khác nhau khi trình tự các ô được truy cập của chúng khác nhau. Câu trả lời là bắt buộc theo modulo (10^9+7). Vấn đề chính thức có (n,m\le100), lên tới 100 trường hợp thử nghiệm, với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

Các ràng buộc đủ nhỏ để một công thức tuyến tính hoặc thời gian không đổi có thể dễ dàng đủ nhanh, nhưng chúng loại trừ mọi thứ theo cấp số nhân về số lượng ô. Một lưới (100\times100) chứa 10.000 ô, do đó, thậm chí (O(nm)) cho mỗi trường hợp thử nghiệm chỉ có khoảng 10.000 thao tác. Ngược lại, việc liệt kê tất cả các bước đi có thể có sẽ có một cây tìm kiếm theo cấp số nhân có độ sâu là 10.000, điều này hoàn toàn không khả thi. 

Các trường hợp cạnh chính được làm từ hình chữ nhật rất mỏng. Đối với lưới (1\times1), có chính xác một thứ tự ghé thăm vì chỉ có một gian hàng, vì vậy câu trả lời là 1. Công thức dành cho hình chữ nhật thông thường sẽ cho kết quả 0 ở đây không chính xác. Ví dụ,```
1
1 1
```sản xuất```
Case #1: 1
```Đối với lưới (1\times2), có chính xác hai đơn hàng truy cập, một đơn hàng bắt đầu từ một trong hai ô. Hướng ban đầu không tạo thêm thứ tự khi ô đầu tiên được cố định, bởi vì bước di chuyển hữu ích duy nhất là về phía ô khác. Như vậy```
1
1 2
```sản xuất```
Case #1: 2
```Một sai lầm dễ mắc thứ hai là coi (1\times m) và (n\times1) như hình chữ nhật hai chiều thông thường. Đối với mọi lưới một chiều có ít nhất hai ô, câu trả lời là 2, không phải biểu thức được sử dụng cho cả hai chiều lớn hơn một. Ví dụ: (1\times100) chỉ có thứ tự truy cập từ trái sang phải và từ phải sang trái. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thực hiện tìm kiếm theo chiều sâu trên tất cả các bước đi có thể. Chúng tôi chọn mọi ô bắt đầu có thể và hướng ban đầu, đánh dấu ô là đã ghé thăm và liên tục thử đi thẳng hoặc rẽ phải trước khi di chuyển một ô. Bất cứ khi nào bước đi đến tất cả các ô (nm), chúng tôi sẽ đếm nó. 

Tìm kiếm này đúng vì mọi thứ tự truy cập hợp lệ đều tương ứng với một nhánh của cây tìm kiếm và mọi nhánh đều bị từ chối ngay khi nó rời khỏi lưới hoặc truy cập lại một ô. Tuy nhiên, việc tìm kiếm có thể có hai lựa chọn ở hầu hết mọi bước. Với các ô (N=nm), giới hạn trên thô là (O(nm\cdot 2^{N})) hoạt động sau khi tính toán các trạng thái bắt đầu có thể có (O(nm)). Đối với lưới (100\times100), điều này có nghĩa là giới hạn trên của thứ tự các hoạt động (10^4\cdot2^{10000}), không thể sử dụng được từ xa. 

Quan sát hữu ích là các bước đi hợp pháp trong một lưới hình chữ nhật bị hạn chế rất nhiều. Vì Lee không bao giờ có thể rẽ trái nên hướng đi chỉ có thể không thay đổi hoặc xoay theo chiều kim đồng hồ. Khi lối đi rẽ, hướng mới của nó sẽ được cố định và một lối rẽ khác chỉ có thể tiếp tục tiến trình theo chiều kim đồng hồ đó. Do đó, một cuộc đi bộ tự tránh thăm viếng từng ô không thể lang thang tùy tiện trong hình chữ nhật. Để bao phủ toàn bộ hình chữ nhật, nó phải vạch ra ranh giới của nó theo kiểu xoắn ốc. 

Chỉ có (2(n+m)-4) đơn đặt hàng truy cập như vậy khi cả hai chiều đều vượt quá một. Một cách để hiểu số lượng là kiểm tra xem cuộc đi bộ có thể bắt đầu từ đâu và ranh giới bên ngoài tác động như thế nào đến phần còn lại của con đường. Mỗi bước đi toàn lưới hợp lệ được xác định bằng cách chọn một trong các vị trí biên mà tại đó quá trình truyền tải theo hình xoắn ốc này bắt đầu, với bốn trường hợp góc được xác định một cách thích hợp. Số kết quả chính xác là 

[ 
2(n+m)-4. 
] 

Đặc tính này cũng là giải pháp nhỏ gọn tiêu chuẩn cho vấn đề này. 

Tìm kiếm brute-force hoạt động vì nó khám phá rõ ràng mọi đường dẫn hợp pháp, nhưng không thành công vì số lượng đường dẫn một phần tăng theo cấp số nhân. Việc quan sát cấu trúc thay thế toàn bộ việc tìm kiếm bằng phép tính thời gian không đổi chỉ dựa trên độ dài hai cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nm\cdot2^{nm})) | (O(nm)) | Quá chậm | 
| Tối ưu | (O(1)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và (m), số hàng và cột. Câu trả lời chỉ phụ thuộc vào kích thước chứ không phụ thuộc vào bất kỳ nội dung nào của ô, bởi vì mọi ô đều có cấu trúc giống hệt nhau. 
2. Nếu (n=m=1), trả về 1. Chỉ có một ô, do đó có thể có chính xác một thứ tự truy cập. 
3. Nếu chính xác một chiều là 1, trả về 2. Lưới một chiều không cần thiết chỉ có thể di chuyển từ điểm cuối này sang điểm cuối khác, tạo ra hai hướng. 
4. Nếu cả hai thứ nguyên đều lớn hơn 1, hãy tính 

[ 
2(n+m)-4. 
] 

Phép trừ 4 chiếm bốn góc mà nếu không sẽ được tính hai lần khi các điểm bắt đầu xoắn ốc có thể được liên kết với bốn cạnh. 
5. In kết quả bằng cách sử dụng`Case #x: y`định dạng. Câu trả lời lớn nhất chỉ là (396) cho (100\times100), do đó mô đun không bao giờ thực sự có liên quan theo các ràng buộc đã cho, mặc dù việc sử dụng số học số nguyên giúp việc triển khai tương thích với yêu cầu đầu ra đã nêu. 

**Tại sao nó hoạt động.** Mọi bước đi hợp pháp đều có hướng không bao giờ quay ngược chiều kim đồng hồ. Do đó, việc truyền toàn bộ hình chữ nhật không có khả năng tạo ra một đường đi Hamilton tùy ý. Các lượt của nó bị ép vào cấu trúc xoắn ốc theo chiều kim đồng hồ của hình chữ nhật. Đối với hình chữ nhật hai chiều, các đường xoắn ốc hoàn chỉnh có thể có tương ứng một-một với các lựa chọn ranh giới hợp lệ (2(n+m)-4). Các hình chữ nhật mỏng thu gọn cấu trúc này thành một đường thẳng, trong đó chỉ còn lại hai hướng di chuyển và trường hợp một ô chỉ có một thứ tự. Những trường hợp này cạn kiệt tất cả các giá trị có thể có của (n) và (m). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    out = []

    for case in range(1, t + 1):
        n, m = map(int, input().split())

        if n == 1 and m == 1:
            ans = 1
        elif n == 1 or m == 1:
            ans = 2
        else:
            ans = 2 * (n + m) - 4

        ans %= MOD
        out.append(f"Case #{case}: {ans}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào tuân theo cấu trúc trường hợp thử nghiệm trực tiếp. Đối với mỗi hình chữ nhật, điều kiện đầu tiên sẽ tách biệt trường hợp một ô duy nhất trước khi kiểm tra trường hợp một chiều. 

Biểu thức cho hình chữ nhật mỏng được tách biệt một cách có chủ ý khỏi công thức chung. Thay (n=1) vào (2(n+m)-4) sẽ cho (2m-2), điều này sai với mọi (m>2). Hình dạng thay đổi hoàn toàn khi không có hàng thứ hai. 

Đối với hình chữ nhật hai chiều thực sự, câu trả lời được tính trực tiếp là`2 * (n + m) - 4`. Không có mô phỏng và không có lưới để phân bổ. Phép toán modulo được đưa vào vì câu lệnh yêu cầu câu trả lời modulo (10^9+7), mặc dù bản thân công thức vẫn nằm dưới mô đun cho các giới hạn đã cho. 

Mã thu thập tất cả các dòng đầu ra và viết chúng một lần. Điều này đơn giản và tránh các lệnh gọi đầu ra lặp đi lặp lại trong tối đa 100 trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Câu lệnh cung cấp một mẫu, (2\times2). Vì không có mẫu thứ hai nên dấu vết thứ hai sử dụng (2\times3), thực hiện công thức hai chiều tổng quát. 

### Ví dụ 1: (2\times2) 

| Bước | (n) | (m) | Tình trạng | Trả lời | 
| --- | --- | --- | --- | --- | 
| Đọc kích thước | 2 | 2 | Cả hai chiều đều lớn hơn 1 | | 
| Áp dụng công thức | 2 | 2 | (2(n+m)-4) | (2(2+2)-4=4) | 
| Đầu ra | 2 | 2 | Kết quả cuối cùng | 4 | 

Bốn thứ tự truy cập tương ứng với bốn hướng có thể có của quá trình truyền tải (2\times2). Điều này phù hợp với mẫu được cung cấp, có đầu ra là`Case #1: 4`. 

### Ví dụ 2: (2\times3) 

| Bước | (n) | (m) | Tình trạng | Trả lời | 
| --- | --- | --- | --- | --- | 
| Đọc kích thước | 2 | 3 | Cả hai chiều đều lớn hơn 1 | | 
| Áp dụng công thức | 2 | 3 | (2(n+m)-4) | (2(2+3)-4=6) | 
| Đầu ra | 2 | 3 | Kết quả cuối cùng | 6 | 

Phần quan trọng của đường này là việc tăng hình chữ nhật từ (2\times2) lên (2\times3) sẽ thêm hai đường đi hoàn chỉnh có thể có. Công thức chỉ phụ thuộc vào kích thước chu vi chứ không phụ thuộc vào mô phỏng rõ ràng của sáu ô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T)) | Mỗi trường hợp thử nghiệm yêu cầu một số phép tính số học không đổi. | 
| Không gian | (O(T)) | Chuỗi đầu ra cho tất cả các trường hợp thử nghiệm được lưu trữ trước khi được viết. Bản thân thuật toán sử dụng thêm (O(1)) không gian cho mỗi trường hợp. | 

Với tối đa 100 trường hợp thử nghiệm và kích thước không lớn hơn 100, con số này thấp hơn nhiều so với giới hạn 1 giây và 256 MB có sẵn. Việc triển khai không phân bổ lưới (100\times100), thực hiện tìm kiếm hoặc duy trì bất kỳ trạng thái trên mỗi ô nào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for case in range(1, t + 1):
        n, m = map(int, input().split())

        if n == 1 and m == 1:
            ans = 1
        elif n == 1 or m == 1:
            ans = 2
        else:
            ans = 2 * (n + m) - 4

        ans %= 10**9 + 7
        out.append(f"Case #{case}: {ans}")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    "1\n2 2\n"
) == "Case #1: 4", "provided sample"

# Minimum-size input
assert run(
    "1\n1 1\n"
) == "Case #1: 1", "single cell"

# One-dimensional boundary case
assert run(
    "1\n1 2\n"
) == "Case #1: 2", "one row"

# Transposed one-dimensional case
assert run(
    "1\n5 1\n"
) == "Case #1: 2", "one column"

# Small two-dimensional rectangle
assert run(
    "1\n2 3\n"
) == "Case #1: 6", "2 by 3 rectangle"

# Equal maximum dimensions
assert run(
    "1\n100 100\n"
) == "Case #1: 396", "maximum equal dimensions"

# Multiple test cases together
assert run(
    "4\n1 1\n1 100\n100 1\n3 4\n"
) == (
    "Case #1: 1\n"
    "Case #2: 2\n"
    "Case #3: 2\n"
    "Case #4: 10"
), "mixed boundary cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Truyền tải đơn bào độc đáo | 
|`1 2`|`2`| Lưới một hàng không cần thiết nhỏ nhất | 
|`5 1`|`2`| Trường hợp một cột và tính đối xứng | 
|`2 3`|`6`| Công thức hai chiều tổng quát | 
|`100 100`|`396`| Kích thước tối đa và các cạnh bằng nhau | 
|`1 1`,`1 100`,`100 1`,`3 4`|`1, 2, 2, 10`| Nhiều trường hợp và xử lý ranh giới | 

## Vỏ cạnh 

Đối với lưới (1\times1), thuật toán sẽ ngay lập tức lấy nhánh đầu tiên. Không có chuyển động nào cả, vì vậy trình tự truy cập duy nhất bao gồm một ô duy nhất. Đầu vào chính xác```
1
1 1
```sản xuất`Case #1: 1`. Điều này nắm bắt các triển khai áp dụng công thức hai chiều một cách mù quáng và thu được số 0. 

Đối với lưới (1\times100), nhánh thứ hai được áp dụng vì một chiều là một. Lee không thể tạo ra hình xoắn ốc hai chiều vì không có hàng thứ hai. Anh ta chỉ có thể thăm các ô từ trái sang phải hoặc từ phải sang trái nên đáp án đúng là 2:```
1
1 100
```sản xuất```
Case #1: 2
```Lý do tương tự được áp dụng sau khi chuyển đổi lưới. Đối với (100\times1), câu trả lời cũng là 2. Tính đối xứng này rất hữu ích để bắt mã xử lý các hàng và cột khác nhau ngay cả khi hình dạng không thay đổi khi xoay. 

Đối với hình chữ nhật chính hãng nhỏ nhất, (2\times2), nhánh tổng quát cho 

[ 
2(2+2)-4=4. 
] 

Như vậy```
1
2 2
```sản xuất```
Case #1: 4
```đó là mẫu chính thức. 

Cuối cùng, đối với lưới tối đa (100\times100), công thức thời gian không đổi tương tự cho 

[ 
2(100+100)-4=396. 
] 

Thuật toán không trở nên chậm hơn khi lưới phát triển vì nó không bao giờ xây dựng hoặc đi qua lưới. Đây chính xác là lợi thế về mặt cấu trúc so với tìm kiếm vũ phu.
