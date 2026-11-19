---
title: "CF 2094C - Brr Brrr Patapim"
description: "Chúng ta có một lưới $n nhân n$ được tạo ra từ một hoán vị không xác định $p$ có độ dài $2n$. Quy tắc xây dựng rất đơn giản: giá trị trong ô $(i,j)$ là $p{i+j}$."
date: "2026-06-08T05:34:04+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2094
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1017 (Div. 4)"
rating: 900
weight: 2094
solve_time_s: 138
verified: false
draft: false
---

[CF 2094C - Brr Brrr Patapim](https://codeforces.com/problemset/problem/2094/C) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới được tạo từ một hoán vị không xác định$p$chiều dài$2n$. 

Quy tắc xây dựng rất đơn giản: giá trị trong ô$(i,j)$là$p_{i+j}$. Từ$i$Và$j$dựa trên 1, mọi ô trên cùng một đường chéo có cùng giá trị vì chúng có cùng tổng$i+j$. 

Nhiệm vụ của chúng ta là xây dựng lại toàn bộ hoán vị$p_1,p_2,\dots,p_{2n}$. 

Quan sát quan trọng là lưới chỉ chứa thông tin về các chỉ số$2$bởi vì$2n$. giá trị$p_1$không bao giờ xuất hiện vì tổng nhỏ nhất có thể là$1+1=2$. 

Những hạn chế là rất nhỏ. Tổng của tất cả$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$800$, vì vậy ngay cả việc đọc đầu vào cũng chiếm ưu thế trong công việc. Bất kỳ giải pháp nào quét lưới một lần đều đủ nhanh. Không cần cấu trúc dữ liệu phức tạp hoặc tối ưu hóa. 

Một trường hợp cạnh không rõ ràng là$n=1$. 

đầu vào:```
1
1
1
```Lưới chỉ chứa$p_2=1$. Vì hoán vị phải chứa các số$1$Và$2$, giá trị còn thiếu là$2$. Câu trả lời là:```
2 1
```Một giải pháp bất cẩn có thể cho rằng mọi vị trí hoán vị đều xuất hiện ở đâu đó trong lưới và quên xây dựng lại$p_1$. 

Một trường hợp tinh tế khác là khi giá trị còn thiếu không phải là số nhỏ nhất cũng không phải số lớn nhất. 

đầu vào:```
1
2
2 3
3 4
```Từ lưới điện chúng tôi phục hồi$p_2=2$,$p_3=3$,$p_4=4$. Con số duy nhất từ$\{1,2,3,4\}$điều đó không xuất hiện là$1$, Vì thế:```
1 2 3 4
```Bất kỳ cách tiếp cận nào có thể đoán được$p_1$từ cấu trúc lưới thay vì sử dụng thuộc tính hoán vị sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là xây dựng lại mọi vị trí$p_k$bằng cách kiểm tra tất cả các ô có chỉ số thỏa mãn$i+j=k$. Vì lưới được đảm bảo là hợp lệ nên mọi ô như vậy đều chứa cùng một giá trị. Sau khi thu thập$p_2$bởi vì$p_{2n}$, ta xác định số còn lại và đặt nó vào vị trí$1$. 

Điều này đã hoạt động trong giới hạn. Lưới chỉ chứa$n^2$tế bào và$n \le 800$. 

Cái nhìn sâu sắc thực sự là chúng ta không cần phải kiểm tra từng đường chéo một cách riêng biệt. 

Đối với bất kỳ số tiền nào$s$giữa$2$Và$2n$, luôn có ít nhất một ô hợp lệ với$i+j=s$. Vì tất cả các ô trên đường chéo đó đều chứa$p_s$, một ô đại diện duy nhất là đủ. 

Vì$s \le n+1$, chúng ta có thể sử dụng ô$(1,s-1)$. 

Vì$s > n+1$, chúng ta có thể sử dụng ô$(s-n,n)$. 

Điều này ngay lập tức đưa ra tất cả các giá trị$p_2,p_3,\dots,p_{2n}$. Từ$p$là một hoán vị của$1$bởi vì$2n$, chính xác một số bị thiếu trong các giá trị được khôi phục đó. Số còn thiếu đó phải là$p_1$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Đã chấp nhận | 
| Tối ưu |$O(n^2)$|$O(n)$| Đã chấp nhận | 

Độ phức tạp tiệm cận là như nhau vì việc đọc lưới đã tốn kém$O(n^2)$, nhưng cách tiếp cận tối ưu sẽ trích xuất trực tiếp từng giá trị hoán vị. 

## Hướng dẫn thuật toán 

1. Đọc lưới. 
2. Tạo một mảng`p`chiều dài`2n + 1`sử dụng chỉ mục dựa trên 1. 
3. Với mọi tổng`s`từ`2`ĐẾN`2n`, hồi phục`p[s]`. 

Nếu như`s <= n + 1`, sử dụng`grid[0][s - 2]`, tương ứng với ô`(1, s-1)`. 

Nếu không, hãy sử dụng`grid[s - n - 1][n - 1]`, tương ứng với ô`(s-n, n)`. 

Cả hai ô đều nằm trên đường chéo với tổng`s`, vậy chúng chứa chính xác`p[s]`. 
4. Đánh dấu tất cả các giá trị được khôi phục`p[2] ... p[2n]`. 
5. Giữa các con số`1 ... 2n`, hãy tìm cái chưa bao giờ được đánh dấu. 
6. Đặt số đó là`p[1]`. 
7. Đầu ra`p[1] ... p[2n]`. 

### Tại sao nó hoạt động 

Mỗi đường chéo tương ứng với chính xác một vị trí hoán vị. Đường chéo với tổng$s$chỉ chứa giá trị$p_s$, do đó, việc lấy bất kỳ ô nào từ đường chéo đó sẽ phục hồi phần tử hoán vị chính xác. 

Lưới bao gồm các khoản tiền$2$bởi vì$2n$, có nghĩa là nó tiết lộ mọi vị trí hoán vị ngoại trừ$p_1$. Vì dãy được đảm bảo là một hoán vị của$1$bởi vì$2n$, chính xác một số bị thiếu trong các giá trị được khôi phục. Số còn thiếu đó phải là$p_1$. Không có lựa chọn nào khác, đó là lý do tại sao việc tái thiết là duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    g = [list(map(int, input().split())) for _ in range(n)]

    p = [0] * (2 * n + 1)

    for s in range(2, 2 * n + 1):
        if s <= n + 1:
            p[s] = g[0][s - 2]
        else:
            p[s] = g[s - n - 1][n - 1]

    seen = [False] * (2 * n + 1)
    for i in range(2, 2 * n + 1):
        seen[p[i]] = True

    for x in range(1, 2 * n + 1):
        if not seen[x]:
            p[1] = x
            break

    print(*p[1:])
```Phần đầu tiên đọc lưới chính xác như đã cho. 

Vòng lặp kết thúc`s`xây dựng lại vị trí`2`bởi vì`2n`. Việc lập chỉ mục là phần duy nhất hơi phức tạp. Lưới đầu vào được lưu trữ với các chỉ số dựa trên 0, vì vậy ô`(1, s-1)`trở thành`g[0][s-2]`, trong khi ô`(s-n, n)`trở thành`g[s-n-1][n-1]`. 

Sau khi khôi phục tất cả các giá trị hoán vị hiển thị, mã sẽ đánh dấu chúng trong`seen`mảng. Vì hoán vị chứa mọi số từ`1`ĐẾN`2n`đúng một lần, việc quét tìm số chưa được đánh dấu sẽ ngay lập tức mang lại kết quả`p[1]`. 

Không tồn tại mối lo ngại về tràn số nguyên vì tất cả các giá trị tối đa là`1600`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3

1 6 2
6 2 4
2 4 3
```Các giá trị được phục hồi: 

| s | Ô đã chọn | Giá trị | p[s] | 
| --- | --- | --- | --- | 
| 2 | (1,1) | 1 | 1 | 
| 3 | (1,2) | 6 | 6 | 
| 4 | (1,3) | 2 | 2 | 
| 5 | (2,3) | 4 | 4 | 
| 6 | (3,3) | 3 | 3 | 

Sau khi xây dựng lại: 

| Vị trí | Giá trị | 
| --- | --- | 
| p2 | 1 | 
| p3 | 6 | 
| p4 | 2 | 
| p5 | 4 | 
| p6 | 3 | 

Những con số hiện có là`{1,2,3,4,6}`. Số còn thiếu từ`1..6`là`5`, Vì thế`p1 = 5`. 

Câu trả lời cuối cùng:```
5 1 6 2 4 3
```Ví dụ này cho thấy rằng mỗi đường chéo phản ánh trực tiếp một vị trí hoán vị. 

### Ví dụ 2 

đầu vào:```
n = 2

2 3
3 4
```Các giá trị được phục hồi: 

| s | Ô đã chọn | Giá trị | p[s] | 
| --- | --- | --- | --- | 
| 2 | (1,1) | 2 | 2 | 
| 3 | (1,2) | 3 | 3 | 
| 4 | (2,2) | 4 | 4 | 

Tập được phục hồi là`{2,3,4}`. 

| Số | Hiện tại? | 
| --- | --- | 
| 1 | Không | 
| 2 | Có | 
| 3 | Có | 
| 4 | Có | 

Giá trị còn thiếu là`1`, Vì thế:```
1 2 3 4
```Ví dụ này minh họa việc xây dựng lại vị trí đầu tiên bị ẩn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Đọc lưới chiếm ưu thế trong công việc | 
| Không gian |$O(n^2)$| Lưới được lưu trữ chứa$n^2$giá trị | 

Tổng số tiền của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$800$, vậy nhiều nhất$640{,}000$các ô lưới được xử lý. Điều này thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        g = [list(map(int, input().split())) for _ in range(n)]

        p = [0] * (2 * n + 1)

        for s in range(2, 2 * n + 1):
            if s <= n + 1:
                p[s] = g[0][s - 2]
            else:
                p[s] = g[s - n - 1][n - 1]

        seen = [False] * (2 * n + 1)
        for i in range(2, 2 * n + 1):
            seen[p[i]] = True

        for x in range(1, 2 * n + 1):
            if not seen[x]:
                p[1] = x
                break

        out.append(" ".join(map(str, p[1:])))

    return "\n".join(out)

# provided samples
assert run(
"""3
3
1 6 2
6 2 4
2 4 3
1
1
2
2 3
3 4
"""
) == (
"""5 1 6 2 4 3
2 1
1 2 3 4"""
)

# minimum size
assert run(
"""1
1
1
"""
) == "2 1"

# missing value is largest
assert run(
"""1
2
1 2
2 3
"""
) == "4 1 2 3"

# missing value in the middle
assert run(
"""1
2
1 4
4 2
"""
) == "3 1 4 2"

# larger case
assert run(
"""1
3
2 5 1
5 1 6
1 6 4
"""
) == "3 2 5 1 6 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`|`2 1`| Ví dụ nhỏ nhất có thể | 
|`[[1,2],[2,3]]`|`4 1 2 3`| Thiếu giá trị tối đa | 
|`[[1,4],[4,2]]`|`3 1 4 2`| Giá trị thiếu nằm ở giữa | 
| hộp đựng tùy chỉnh 3×3 |`3 2 5 1 6 4`| Trích xuất chính xác tất cả các đường chéo | 

## Vỏ cạnh 

Hãy xem xét đầu vào hợp lệ nhỏ nhất:```
1
1
1
```Thuật toán chỉ phục hồi`p2 = 1`. Hoán vị phải chứa các số`{1,2}`. Giá trị còn thiếu là`2`, vì vậy nó xuất ra:```
2 1
```Việc xây dựng lại vẫn hợp lệ ngay cả khi lưới bao gồm một ô duy nhất. 

Bây giờ hãy xem xét:```
1
2
2 3
3 4
```Phục hồi mang lại:```
p2 = 2
p3 = 3
p4 = 4
```Số còn thiếu từ`{1,2,3,4}`là`1`, sản xuất:```
1 2 3 4
```Điều này xác nhận rằng thuật toán không dựa vào bất kỳ vị trí đặc biệt nào cho giá trị còn thiếu. 

Cuối cùng, hãy xem xét:```
1
2
1 4
4 2
```Sản lượng thu hồi:```
p2 = 1
p3 = 4
p4 = 2
```Số duy nhất chưa được sử dụng là`3`, Vì thế:```
3 1 4 2
```Điều này xác minh rằng phần tử đầu tiên bị ẩn có thể là bất kỳ giá trị nào trong phạm vi hoán vị, không chỉ là điểm cuối.
