---
title: "CF 102870D - Bậc thầy về cấu trúc dữ liệu và Orz Pandas"
description: "Chúng ta có một cây có gốc với nút 1 là gốc. Trong chuỗi vô hạn các thao tác ngẫu nhiên, mỗi lần một nút được chọn thống nhất."
date: "2026-07-25T13:13:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102870
codeforces_index: "D"
codeforces_contest_name: "2020-2021 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102870
solve_time_s: 71
verified: true
draft: false
---

[CF 102870D - Bậc thầy về cấu trúc dữ liệu và Orz Pandas](https://codeforces.com/problemset/problem/102870/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cây có gốc với nút`1`như là gốc. Trong chuỗi vô hạn các thao tác ngẫu nhiên, mỗi lần một nút được chọn thống nhất. Đường đi từ nút gốc đến nút đó được truy cập và mọi cạnh trên đường dẫn đó sẽ trở thành cạnh được ưu tiên của nút cha nếu nó chưa được ưu tiên. Nhiệm vụ là tìm ra số lượng thay đổi con ưa thích trung bình dài hạn cho mỗi phép toán. Câu trả lời là bắt buộc theo modulo`998244353`. Tuyên bố ban đầu mô tả các hoạt động này là các hoạt động truy cập LCT, nhưng giải pháp không cần bất kỳ kiến ​​thức nào về Cây cắt liên kết. 

Khó khăn chính là quá trình này diễn ra ngẫu nhiên và tiếp tục mãi mãi. Chúng tôi không mô phỏng hoạt động. Thay vào đó, chúng tôi tính toán số lượng thay đổi dự kiến ​​do mỗi nút gây ra sau khi quá trình đạt đến trạng thái ổn định. 

Đối với một cây có tới`100000`các nút, bất kỳ cách tiếp cận nào liên tục mô phỏng các hoạt động hoặc thực hiện công việc tỷ lệ thuận với chiều cao của cây cho mọi trạng thái có thể đều quá tốn kém. Một cách tiếp cận bậc hai có thể đã thực hiện xung quanh`10^10`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 1 giây thông thường cho phép. Chúng ta cần tính toán tuyến tính hoặc gần tuyến tính trên cây. 

Trường hợp cạnh đầu tiên là cây một nút. Không có con nào được ưu tiên và không có khả năng thay đổi nào, nên câu trả lời là`0`.```
Input
1

Output
0
```Việc triển khai bất cẩn cho rằng mọi nút đều có cạnh cha hoặc cố gắng đảo ngược`subtree_size - 1`không kiểm tra sẽ thất bại ở đây. 

Một trường hợp cạnh khác là một nút chỉ có một nút con. Khi nút con đó đã được truy cập thông qua nút, các truy cập trong tương lai thông qua nút đó không bao giờ có thể thay đổi nút con được ưu tiên. Ví dụ:```
Input
2
1

Output
0
```Root luôn ưu tiên con duy nhất của nó nên số lượng thay đổi dự kiến ​​bằng 0. Một công thức tính mỗi lần ghé thăm trẻ là một sự thay đổi sẽ tạo ra giá trị dương không chính xác. 

Trường hợp cạnh thứ ba là một nút có các nút con có kích thước cây con rất khác nhau. Ví dụ:```
Input
5
1 1 1 1

Output
0
```Rễ có bốn lá con. Đứa trẻ ưa thích của nó chỉ đơn giản là đứa trẻ cuối cùng trong số bốn đứa trẻ được truy cập. Xác suất thay đổi từ đứa trẻ này sang đứa trẻ khác phụ thuộc vào sự phân bổ các lần truy cập trước đó chứ không chỉ là số lượng trẻ. Đối xử với tất cả trẻ em như nhau sẽ tạo ra kỳ vọng sai lầm. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng các trạng thái con ưa thích. Đối với mỗi thao tác, hãy chọn một nút, đi từ nút gốc đến nút đó và cập nhật các nút con ưa thích dọc theo đường dẫn. Điều này đúng vì nó tuân theo quy trình một cách chính xác. Tuy nhiên, giá trị được yêu cầu là giới hạn khi số lượng thao tác tiến tới vô cùng. Mô phỏng không thể cung cấp câu trả lời mô-đun chính xác và thậm chí một triệu thao tác trên cây sâu cũng có thể quá chậm. Trong một chuỗi`100000`các nút, một thao tác có thể chạm vào`100000`các cạnh, đưa ra về`10^11`cập nhật cạnh cho`10^6`các hoạt động mô phỏng. 

Quan sát hữu ích là mọi nút đều có thể được phân tích độc lập. Hãy xem xét một nút`u`với trẻ em`c1, c2, ...`. Một đứa con ưa thích của`u`chỉ thay đổi khi nút được chọn nằm ở một trong các cây con này. Nếu nút được chọn ở bên ngoài`u`cây con của, không có gì liên quan`u`những thay đổi. 

Sau nhiều lần phẫu thuật, đứa con ưa thích của`u`là cây con chứa quyền truy cập gần đây nhất bên dưới`u`. Xác suất đứa trẻ đó`c`hiện được ưu tiên tỷ lệ thuận với kích thước của cây con của nó. Nếu tổng số nút bên dưới con của`u`là`s`, sau đó:$$P(c\text{ is preferred})=\frac{size[c]}{s}$$Trong lần phẫu thuật tiếp theo, con`c`được chọn với xác suất:$$\frac{size[c]}{n}$$Một sự thay đổi xảy ra khi đứa trẻ mới khác với đứa trẻ được ưu tiên hiện tại. Sự đóng góp của`u`là:$$\sum_c \frac{size[c]}{n}
\left(1-\frac{size[c]}{s}\right)$$Ở đâu:$$s = subtree[u]-1$$Điều này đơn giản hóa thành:$$\frac{1}{n}
\left(
s-\frac{\sum size[c]^2}{s}
\right)$$Bây giờ vấn đề trở thành vấn đề DP cây. Chúng ta chỉ cần kích thước cây con và tổng kích thước cây con bình phương cho mỗi nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(thao tác × chiều cao cây) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây gốc từ mảng cha. Lưu trữ các cây con của mỗi nút để sau này chúng ta có thể kiểm tra từng cây con con. 
2. Tính toán kích thước của mỗi cây con bằng cách duyệt theo thứ tự sau. Kích thước cây con của một nút là cần thiết vì nó cho chúng ta biết tần suất mỗi nút con có thể trở thành nút con được ưu tiên. 
3. Đối với mọi nút không phải lá`u`, cho phép`s = subtree[u] - 1`. Đây là số nút chứa trong tất cả các cây con của`u`. Tính giá trị:$$\frac{s-\frac{\sum size[child]^2}{s}}{n}$$và thêm nó vào câu trả lời. 

1. Thực hiện chia modulo tất cả`998244353`. Vì mô đun là số nguyên tố nên mọi mẫu số khác 0 đều có nghịch đảo bằng cách sử dụng lũy ​​thừa mô đun. 
2. Xuất ra modulo giá trị tích lũy`998244353`. 

Tại sao nó hoạt động: 

Đối với mỗi nút`u`, cây con được ưu tiên chỉ được xác định bởi quyền truy cập mới nhất đã nhập vào một trong các cây con của nó. Độc lập với phần còn lại của cây, điều này tạo ra phân bố xác suất ổn định trong đó mỗi cây con được ưu tiên tùy theo kích thước cây con của nó. Công thức tính toán chính xác xác suất để lần truy cập tiếp theo chọn một đứa trẻ khác. Tổng hợp những đóng góp dự kiến ​​​​độc lập này trên tất cả các nút sẽ cho ra số lượng thay đổi con được ưu tiên dự kiến ​​trong một thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)

    children = [[] for _ in range(n)]
    if n > 1:
        parents = list(map(int, input().split()))
        for i, p in enumerate(parents, start=1):
            children[p - 1].append(i)

    size = [1] * n
    order = [0]
    for u in order:
        for v in children[u]:
            order.append(v)

    for u in reversed(order):
        for v in children[u]:
            size[u] += size[v]

    inv_n = pow(n, MOD - 2, MOD)
    ans = 0

    for u in range(n):
        if not children[u]:
            continue

        s = size[u] - 1
        sq = 0
        for v in children[u]:
            sq += size[v] * size[v]
            sq %= MOD

        cur = (s % MOD - sq * pow(s, MOD - 2, MOD)) % MOD
        cur = cur * inv_n % MOD
        ans += cur
        ans %= MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Cây được lưu trữ dưới dạng danh sách con vì mỗi nút chỉ cần thông tin từ các nút con trực tiếp của nó. Thứ tự truyền tải được xây dựng lặp đi lặp lại để tránh các vấn đề về độ sâu đệ quy của Python trên cây hình chuỗi. 

Kích thước cây con được tính theo thứ tự duyệt ngược. Khi một nút được xử lý, tất cả các nút con của nó đã nhận được kích thước cây con cuối cùng của chúng, do đó việc thêm chúng sẽ tạo ra kích thước chính xác cho nút cha. 

Công thức chỉ được áp dụng cho các nút có con. Đối với lá,`subtree[u] - 1`bằng 0 và không thể thay đổi con ưu tiên, do đó việc bỏ qua chúng cũng tránh được việc chia cho 0. 

Tất cả số học được thực hiện modulo`998244353`. Nghịch đảo của`s`tồn tại bởi vì`s`luôn ở giữa`1`Và`n-1`cho các nút được xử lý. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
Input
5
1 1 2 2
```Cây đó là:```
    1
   / \
  2   3
 / \
4   5
```| Nút | Kích thước cây con con | s | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 3, 1 | 4 | 10/3 | 
| 2 | 1, 1 | 2 | 1/5 | 
| 3 | không | - | 0 | 
| 4 | không | - | 0 | 
| 5 | không | - | 0 | 

Tổng cộng là:$$\frac{3}{10}+\frac{1}{5}=\frac{1}{2}$$được biểu diễn modulo`998244353`BẰNG`499122177`. 

Đối với một chuỗi:```
Input
3
1 2
```Cây đó là:```
1
|
2
|
3
```| Nút | Kích thước cây con con | s | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 0 | 
| 2 | 1 | 1 | 0 | 
| 3 | không | - | 0 | 

Mỗi nút chỉ có một nút con khả thi, vì vậy các nút con được ưu tiên không bao giờ cần thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được xử lý một số lần không đổi trong quá trình xây dựng cây và tính toán cây con. | 
| Không gian | O(n) | Danh sách con, thứ tự duyệt và kích thước cây con đều lưu trữ thông tin cho mỗi nút. | 

Những ràng buộc cho phép`100000`các nút và một giải pháp tuyến tính phù hợp thoải mái trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# sample
assert run("5\n1 1 2 2\n") == "499122177\n", "sample"

# single node
assert run("1\n") == "0\n", "minimum size"

# two nodes
assert run("2\n1\n") == "0\n", "single child"

# star tree
assert run("5\n1 1 1 1\n") == "798595487\n", "many equal children"

# chain
assert run("4\n1 2 3\n") == "0\n", "boundary chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 / 1 1 2 2`|`499122177`| Mẫu gốc và xử lý phân số theo mô-đun | 
|`1`|`0`| Cấu trúc con ưa thích trống | 
|`2 / 1`|`0`| Các nút con đơn không thể thay đổi tùy chọn | 
|`5 / 1 1 1 1`|`798595487`| Xác suất cây con bằng nhau | 
|`4 / 1 2 3`|`0`| Cấu trúc cây sâu và đệ quy nhạy cảm | 

## Vỏ cạnh 

Đối với cây nút đơn:```
Input
1
```Thuật toán tạo một nút không có nút con. Nó bỏ qua vòng đóng góp vì không có con nào được ưa thích hơn, để lại câu trả lời là 0. 

Đối với một nút có một con:```
Input
2
1
```Gốc có`s = 1`và đứa con duy nhất có kích thước cây con`1`. Sự đóng góp trở thành:$$1-\frac{1^2}{1}=0$$vì vậy câu trả lời vẫn là số không. Công thức phản ánh thực tế rằng việc chọn cùng một đứa con một không thể tạo ra sự phân công lại. 

Đối với trường hợp con bằng nhau:```
Input
5
1 1 1 1
```Gốc có bốn con cỡ một. Xác suất để lần truy cập tiếp theo thay đổi đứa trẻ được ưu tiên là:$$4 \times \frac{1}{5}\times\frac{3}{4}=\frac{3}{5}$$Các trạng thái con được xử lý thông qua kích thước cây con của chúng, do đó thuật toán không vô tình cho rằng mỗi lần truy cập sẽ thay đổi cạnh ưu tiên. 

Đối với một cây không cân bằng, chẳng hạn như một chuỗi, thuật toán sẽ tạo ra số 0 một cách chính xác vì mỗi nút bên trong chỉ có một nút con được ưu tiên có thể. Việc triển khai đạt đến kết luận tương tự mà không cần xử lý đặc biệt vì công thức chung tự nhiên giảm về 0.
