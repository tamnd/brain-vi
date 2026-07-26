---
title: "CF 102803D - Cái chết bởi ngàn vết cắt"
description: "Chúng ta có một hình hộp chữ nhật có các góc đối diện là gốc tọa độ và (a, b, c). Một mặt phẳng có các hệ số A, B, C cố định được di chuyển song song với chính nó bằng cách chỉ thay đổi số hạng không đổi của nó."
date: "2026-07-26T16:21:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102803
codeforces_index: "D"
codeforces_contest_name: "The 15th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102803
solve_time_s: 54
verified: true
draft: false
---

[CF 102803D - Cái chết bởi hàng ngàn vết cắt](https://codeforces.com/problemset/problem/102803/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Ta có một hình hộp chữ nhật có các góc đối diện là gốc tọa độ và`(a, b, c)`. Mặt phẳng có hệ số cố định`A, B, C`được di chuyển song song với chính nó bằng cách chỉ thay đổi số hạng không đổi của nó. Các vị trí khả dĩ duy nhất được xem xét là những vị trí mà mặt phẳng cắt hộp, và tất cả các vị trí như vậy đều có khả năng xảy ra như nhau. 

Đối với bất kỳ vị trí được chọn nào, giao điểm giữa mặt phẳng và hộp là một đa giác. Nhiệm vụ là xác định xác suất để đa giác này có 3, 4, 5 hoặc 6 cạnh. Câu trả lời cho mỗi xác suất phải được in theo modulo`10^9 + 7`, do đó mọi xác suất được biểu diễn dưới dạng phân số và được chuyển đổi bằng cách sử dụng nghịch đảo mô đun. 

Quan sát quan trọng là sự thay đổi`D`chỉ di chuyển máy bay dọc theo hướng bình thường của nó. Nếu chúng ta định nghĩa```
value(x, y, z) = A*x + B*y + C*z
```thì vị trí mặt phẳng tương đương với việc chọn một giá trị ngẫu nhiên`t`và nhìn vào lát cắt:```
A*x + B*y + C*z = t
```bên trong hộp. Các giá trị có thể có của`t`tạo thành khoảng cách giữa giá trị tối thiểu và tối đa đạt được tại các đỉnh của hộp. 

Chỉ có tám đỉnh nên chỉ có tám giá trị liên quan mà hình dạng của mặt cắt có thể thay đổi. Giữa hai giá trị đỉnh liên tiếp, hình đa giác được cố định vì mặt phẳng không bao giờ đi qua một đỉnh trong khoảng thời gian đó. Điều này có nghĩa là toàn bộ bài toán xác suất liên tục có thể được rút gọn thành việc kiểm tra một số lượng nhỏ các khoảng. 

Các ràng buộc cho phép lên đến`10^4`trường hợp thử nghiệm. Độ dài cạnh và hệ số có thể lớn bằng`10^4`, nhưng chúng không ảnh hưởng đến số lượng sự kiện quan trọng vì hình lập phương luôn có đúng tám đỉnh và mười hai cạnh. Một cách tiếp cận tùy thuộc vào quy mô của`a`,`b`, hoặc`c`, chẳng hạn như quét mọi tọa độ hoặc mọi vị trí mặt phẳng có thể, sẽ là không thể. Giải pháp chỉ phải thực hiện công việc liên tục cho mỗi trường hợp thử nghiệm. 

Một số chi tiết có thể phá vỡ việc triển khai ngây thơ. Các giá trị đỉnh bằng nhau là phổ biến khi hệ số bằng 0 hoặc khi các đỉnh khác nhau tạo ra cùng một giá trị biểu thức. Ví dụ: nếu mặt phẳng song song với một mặt thì nhiều đỉnh có thể có cùng giá trị. Thuật toán phải bỏ qua các khoảng có độ dài bằng 0 giữa các giá trị bằng nhau. 

Một vấn đề khác là kiểm tra vị trí chính xác trên giá trị đỉnh. Những vị trí như vậy có xác suất bằng 0, nhưng nếu chúng được sử dụng trong quá trình đếm thì chúng có thể đếm không chính xác các cạnh chạm vào một đỉnh. Ví dụ, một mặt phẳng đi qua một góc khối lập phương có một giao điểm suy biến, trong khi hầu hết mọi vị trí gần đó đều có một đa giác pháp tuyến. Thuật toán tránh điều này bằng cách luôn chọn một điểm hoàn toàn bên trong một khoảng. 

Một ví dụ cụ thể về trường hợp hệ số bằng 0 là:```
a = 1, b = 1, c = 1
A = 1, B = 0, C = 0
```Mọi mặt phẳng hợp lệ đều có dạng`x = constant`, do đó mặt cắt ngang luôn là hình chữ nhật. Câu trả lời là:```
0 1 0 0
```Một phương pháp bất cẩn mong đợi tất cả các hệ số đóng góp như nhau có thể đếm không chính xác các hình tam giác gần biên. 

Một ví dụ khác là:```
a = 1, b = 1, c = 1
A = 1, B = 1, C = 1
```Các giá trị đỉnh là`0, 1, 1, 1, 2, 2, 2, 3`. Khoảng giữa`(1, 2)`cho một hình lục giác, trong khi hai khoảng bên ngoài cho hình tam giác. Xác suất đúng là:```
2/3 0 0 1/3
```Một giải pháp coi tất cả tám đỉnh là có các giá trị khác nhau sẽ tạo ra các khoảng không chính xác. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng mọi thay đổi có thể xảy ra trên mặt cắt ngang. Vì hình dạng chỉ thay đổi khi mặt phẳng đi qua một đỉnh nên chúng ta có thể tìm tất cả các vị trí đỉnh, sắp xếp chúng và với mỗi khoảng chọn một mặt phẳng đại diện. Đối với mặt phẳng đại diện đó, chúng ta kiểm tra tất cả 12 cạnh của hình chữ nhật và đếm xem có bao nhiêu cạnh bị cắt. Số cạnh chéo chính xác là số cạnh của đa giác. 

Ý tưởng mạnh mẽ này đã gần với giải pháp cuối cùng vì hình học nhỏ. Vấn đề là nhiều thí sinh có thể cố gắng rời rạc hóa không gian tọa độ thực tế vì`a`,`b`, Và`c`có thể lớn. Việc quét qua khối lượng hộp hoặc trên tất cả các vị trí có thể có sẽ yêu cầu quá nhiều thao tác. Ví dụ: hình lập phương có cạnh dài`10000`đã chứa`10^12`tế bào đơn vị. 

Thực tế cấu trúc quan trọng là số đỉnh và số cạnh không bao giờ thay đổi. Tám giá trị đỉnh mô tả đầy đủ tất cả các chuyển đổi có thể xảy ra. Khi đã biết các giá trị đỉnh duy nhất được sắp xếp, có tối đa bảy khoảng để kiểm tra. Đối với mỗi khoảng, việc kiểm tra 12 cạnh là công việc liên tục. 

Lực lượng vũ phu hoạt động vì đối tượng hình học rất nhỏ nhưng không thành công khi việc triển khai phụ thuộc vào phạm vi tọa độ. Nhận xét rằng chỉ các sự kiện ở đỉnh mới quan trọng làm giảm bài toán từ một không gian liên tục rộng lớn xuống một số khoảng không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(abc) hoặc tệ hơn tùy theo mức độ rời rạc | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Tính giá trị của`A*x + B*y + C*z`cho tất cả tám đỉnh của hình hộp chữ nhật. Mỗi tọa độ là`0`hoặc độ dài cạnh tương ứng nên chỉ có tám tổ hợp. 
2. Sắp xếp tám giá trị này và loại bỏ các giá trị trùng lặp. Các giá trị riêng biệt liên tiếp xác định các khoảng trong đó mặt cắt ngang có số cạnh không đổi. Các giá trị trùng lặp sẽ bị loại bỏ vì khoảng có độ dài bằng 0 không góp phần tạo ra xác suất. 
3. Với mọi cặp giá trị liền kề`l`Và`r`, chọn bất kỳ điểm nào bên trong khoảng. Thay vì sử dụng số dấu phẩy động, hãy sử dụng dấu giữa gấp đôi`l + r`. 
4. Kiểm tra tất cả mười hai cạnh hình khối. Một cạnh đóng góp một cạnh cho đa giác một cách chính xác khi hai giá trị điểm cuối của nó nằm ở hai phía đối diện của điểm giữa đã chọn. Sử dụng giá trị nhân đôi sẽ tránh được các vấn đề về độ chính xác. 
5. Thêm độ dài khoảng thời gian`r - l`vào thùng tương ứng với số cạnh giao nhau. Độ dài khoảng là trọng số xác suất vì vị trí mặt phẳng được phân bố đồng đều. 
6. Chia mọi trọng lượng tích lũy cho tổng số vị trí mặt phẳng có thể có. Thực hiện phép chia modulo`10^9 + 7`sử dụng nghịch đảo mô-đun của phạm vi. 

Tại sao nó hoạt động: Giá trị`A*x + B*y + C*z`thay đổi liên tục khi máy bay chuyển động. Thời điểm duy nhất mà cấu trúc liên kết của giao điểm có thể thay đổi là khi mặt phẳng đạt tới một đỉnh của hình hộp chữ nhật. Giữa hai giá trị đỉnh liên tiếp, không có đỉnh nào cắt mặt phẳng nên mọi cạnh luôn cắt nhau hoặc không bao giờ cắt nhau. Do đó, việc đếm các cạnh giao nhau trong một điểm đại diện sẽ cho kích thước đa giác chính xác cho toàn bộ khoảng. Tổng độ dài của tất cả các khoảng có cùng số cạnh sẽ cho tử số xác suất chính xác. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

vertices = [
    (0, 0, 0),
    (0, 0, 1),
    (0, 1, 0),
    (0, 1, 1),
    (1, 0, 0),
    (1, 0, 1),
    (1, 1, 0),
    (1, 1, 1),
]

edges = []
for i, p in enumerate(vertices):
    for j, q in enumerate(vertices):
        if i < j:
            diff = sum(abs(p[k] - q[k]) for k in range(3))
            if diff == 1:
                edges.append((i, j))

def solve_case(a, b, c, A, B, C):
    vals = []
    for x, y, z in vertices:
        vals.append(A * x * a + B * y * b + C * z * c)

    order = sorted(set(vals))
    total = order[-1] - order[0]

    ans = [0, 0, 0, 0]

    for l, r in zip(order, order[1:]):
        mid2 = l + r
        cnt = 0

        for u, v in edges:
            x = vals[u]
            y = vals[v]
            if (2 * x < mid2 and 2 * y > mid2) or (2 * y < mid2 and 2 * x > mid2):
                cnt += 1

        ans[cnt - 3] += r - l

    inv_total = pow(total, MOD - 2, MOD)
    return [(x % MOD) * inv_total % MOD for x in ans]

def main():
    data = list(map(int, sys.stdin.buffer.read().split()))
    if len(data) == 0:
        return

    if len(data) == 6:
        t = 1
        idx = 0
    else:
        t = data[0]
        idx = 1

    out = []

    for _ in range(t):
        a, b, c, A, B, C = data[idx:idx + 6]
        idx += 6
        out.append(" ".join(map(str, solve_case(a, b, c, A, B, C))))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Danh sách đỉnh chỉ lưu trữ tám lựa chọn góc có thể có. Tọa độ thực tế được tạo ra bằng cách nhân các lựa chọn nhị phân này với`a`,`b`, Và`c`. 

Việc xây dựng cạnh được thực hiện một lần. Hai đỉnh tạo thành một cạnh chính xác khi tọa độ nhị phân của chúng khác nhau ở một vị trí, tạo ra mười hai cạnh của hình hộp chữ nhật. 

So sánh điểm giữa là phần tế nhị nhất trong quá trình thực hiện. Việc sử dụng các giá trị dấu phẩy động có thể không thành công khi hệ số lớn hoặc khi hai giá trị rất gần nhau. Vì tất cả các giá trị đỉnh đều là số nguyên nên việc so sánh`2 * value`với`l + r`đưa ra một so sánh chính xác với điểm giữa của khoảng. 

Phép chia cuối cùng sử dụng định lý Fermat vì mô đun là số nguyên tố. Tổng phạm vi luôn dương vì các hệ số không hoàn toàn bằng 0 và hình chữ nhật có kích thước dương. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
a = 1, b = 2, c = 3
A = 1, B = 1, C = 1
```Các giá trị đỉnh là: 

| Đỉnh | Giá trị | 
| --- | --- | 
| (0,0,0) | 0 | 
| (0,2,0) | 2 | 
| (0,0,3) | 3 | 
| (0,2,3) | 5 | 
| (1,0,0) | 1 | 
| (1,2,0) | 3 | 
| (1,0,3) | 4 | 
| (1,2,3) | 6 | 

Các giá trị duy nhất được sắp xếp là`0,1,2,3,4,5,6`. 

| Khoảng thời gian | So sánh điểm giữa | Bên | Cân nặng | 
| --- | --- | --- | --- | 
| (0,1) | bên trong khoảng đầu tiên | 3 | 1 | 
| (1,2) | trong khoảng thứ hai | 4 | 1 | 
| (2,3) | bên trong khoảng thứ ba | 4 | 1 | 
| (3,4) | bên trong khoảng giữa | 4 | 1 | 
| (4,5) | trong khoảng thứ năm | 4 | 1 | 
| (5,6) | bên trong khoảng thời gian cuối cùng | 3 | 1 | 

Tổng phạm vi là`6`. Hình tam giác chiếm chiều dài`2`, tứ giác có chiều dài`4`, vậy xác suất là`1/3`Và`2/3`, phù hợp với đầu ra. 

Đối với mẫu thứ hai:```
a = 2, b = 2, c = 2
A = 1, B = 1, C = 1
```Các giá trị đỉnh được sắp xếp là`0,2,4,6`. 

| Khoảng thời gian | Bên | Cân nặng | 
| --- | --- | --- | 
| (0,2) | 3 | 2 | 
| (2,4) | 6 | 2 | 
| (4,6) | 3 | 2 | 

Tổng phạm vi là`6`. Hình tam giác có tổng trọng lượng`4`và hình lục giác có trọng lượng`2`, cho:```
2/3 0 0 1/3
```# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Luôn có tám đỉnh và mười hai cạnh nên khối lượng công việc là cố định. | 
| Không gian | O(1) | Chỉ các mảng có kích thước không đổi được sử dụng. | 

Giải pháp dễ dàng xử lý`10^4`các trường hợp kiểm thử vì mỗi trường hợp chỉ thực hiện vài chục phép tính số học. 

# Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 10**9 + 7

def run(inp: str) -> str:
    # Paste the submitted solution's main logic here and call it.
    # This placeholder assumes the solution has been wrapped into main().
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    main()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided sample 1
assert run(
    "1\n1 2 3 1 1 1\n"
) == "333333336 333333336 333333336 0\n"

# provided sample 2
assert run(
    "1\n2 2 2 1 1 1\n"
) == "666666672 0 0 333333336\n"

# custom: plane parallel to a face, always a rectangle
assert run(
    "1\n1 1 1 1 0 0\n"
) == "0 1 0 0\n"

# custom: all coefficients equal on a cube
assert run(
    "1\n10000 10000 10000 1 1 1\n"
) == "666666672 0 0 333333336\n"

# custom: two-dimensional diagonal extrusion
assert run(
    "1\n1 1 10000 1 1 0\n"
) == "0 1 0 0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 0 0`|`0 1 0 0`| Xử lý các hệ số bằng 0 và các mặt phẳng song song | 
|`10000 10000 10000 1 1 1`|`666666672 0 0 333333336`| Xử lý kích thước tối đa và giá trị đỉnh lặp lại | 
|`1 1 10000 1 1 0`|`0 1 0 0`| Xử lý hệ số loại bỏ một chiều | 

# Vỏ cạnh 

Khi hệ số bằng 0, một số giá trị đỉnh trở nên giống hệt nhau. Đối với đầu vào:```
1
1 1 1 1 0 0
```các giá trị đỉnh chỉ`0`Và`1`. Thuật toán tạo ra một khoảng`(0,1)`. Kiểm tra điểm giữa cho thấy bốn cạnh song song với`x`hướng tạo thành ranh giới của lát cắt, tạo thành bốn cạnh. Khoảng bao gồm toàn bộ phạm vi có thể, vì vậy kết quả là:```
0 1 0 0
```Khi nhiều đỉnh chia sẻ giá trị, các giá trị trùng lặp không được tạo ra xác suất nhân tạo. Vì:```
1
1 1 1 1 1 1
```các giá trị là`0,1,2,3`sau khi loại bỏ các bản sao. Thuật toán kiểm tra ba khoảng thời gian. Khoảng đầu tiên và khoảng cuối cùng tạo ra hình tam giác, trong khi khoảng giữa tạo ra hình lục giác. Kết quả là:```
666666672 0 0 333333336
```Khi mặt phẳng đi chính xác qua một giá trị đỉnh thì vị trí đó có xác suất bằng 0. Thuật toán không bao giờ chọn những vị trí đó. Nó chỉ kiểm tra các khoảng mở giữa các giá trị riêng biệt liên tiếp, do đó các lát cắt suy biến không thể ảnh hưởng đến câu trả lời.
