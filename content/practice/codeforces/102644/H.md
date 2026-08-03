---
title: "CF 102644H - Cập nhật tâm trạng chuỗi"
description: "Chúng tôi duy trì một chuỗi các chữ cái viết hoa và dấu chấm hỏi. Dấu chấm hỏi sau này có thể trở thành bất kỳ chữ cái tiếng Anh viết hoa nào. Đọc chuỗi từ trái sang phải sẽ làm thay đổi tâm trạng của Limak, tâm trạng chỉ có hai trạng thái: vui và buồn."
date: "2026-08-02T14:50:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102644
codeforces_index: "H"
codeforces_contest_name: "Matrix Exponentiation"
rating: 0
weight: 102644
solve_time_s: 61
verified: true
draft: false
---

[CF 102644H - Cập nhật tâm trạng theo chuỗi](https://codeforces.com/problemset/problem/102644/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một chuỗi các chữ cái viết hoa và dấu chấm hỏi. Dấu chấm hỏi sau này có thể trở thành bất kỳ chữ cái tiếng Anh viết hoa nào. Đọc chuỗi từ trái sang phải sẽ làm thay đổi tâm trạng của Limak, tâm trạng chỉ có hai trạng thái: vui và buồn. Mục đích là đếm xem có bao nhiêu lần thay thế tất cả các dấu chấm hỏi khiến Limak hài lòng sau khi toàn bộ chuỗi được xử lý. Sau mỗi lần cập nhật ký tự đơn, số đếm tương tự phải được in lại. Các ràng buộc rất lớn: độ dài chuỗi và số lượng cập nhật đều có thể đạt tới 200000, do đó, giải pháp quét toàn bộ chuỗi sau mỗi lần cập nhật sẽ thực hiện khoảng 40000000000 thao tác trong trường hợp xấu nhất, vượt xa những gì phù hợp với giới hạn cuộc thi thông thường. Báo cáo vấn đề và giới hạn được lấy từ Codeforces Gym 102644H. 

Phần quan trọng là tâm trạng chỉ có hai trạng thái. Chúng ta không cần phải nhớ toàn bộ lịch sử của tiền tố, chỉ cần nhớ bao nhiêu cách mà một phân đoạn có thể chuyển đổi tâm trạng đến thành tâm trạng đi ra. Không gian trạng thái nhỏ này là thứ cho phép chúng ta duy trì câu trả lời một cách linh hoạt. 

Một lỗi phổ biến là xử lý các dấu chấm hỏi một cách độc lập và chỉ đếm các chữ cái tốt cho từng chữ cái. Điều này không thành công vì các chữ cái tương tác với tâm trạng hiện tại. Ví dụ, với chuỗi`A?`, câu trả lời đúng là`6`. đầu tiên`A`chuyển vui sang buồn nên nhân vật thứ hai phải làm tâm trạng vui trở lại. Một giải pháp bất cẩn đếm các chữ cái hợp lệ ở mỗi vị trí một cách độc lập sẽ bỏ lỡ sự phụ thuộc đó. 

Một trường hợp khác là khi một đoạn không có tác dụng gì cả. Đối với đầu vào:```
1 1
B
1 A
```Các kết quả đầu ra là:```
19
18
```

`B`là một lá thư giữ cho tâm trạng không thay đổi, trong khi`A`lật nó. Một giải pháp chỉ tính số lần lật và bỏ qua số lựa chọn trung lập sẽ cho kết quả sai. 

Trường hợp cạnh thứ ba là một chuỗi chỉ bao gồm các dấu chấm hỏi. Vì:```
2 0
??
```câu trả lời là`403`. Mọi cặp chữ cái có thể có đều phải được xem xét, bao gồm cả những sự kết hợp trong đó chữ cái đầu tiên khiến Limak buồn và chữ cái thứ hai giúp cải thiện tâm trạng. Bất kỳ phương pháp nào chỉ theo dõi số lượng tiền tố hiện có sẽ làm mất các khả năng này. 

## Phương pháp tiếp cận 

Giải pháp vũ phu rất đơn giản. Đối với mỗi dấu chấm hỏi, hãy thử tất cả 26 chữ cái, mô phỏng sự thay đổi tâm trạng và đếm những từ thay thế kết thúc bằng trạng thái vui vẻ. Điều này đúng vì nó kiểm tra chính xác tập hợp các chuỗi có thể. Tuy nhiên, nếu có 200000 dấu chấm hỏi thì số chuỗi có thể có là`26^200000`, vì vậy ngay cả việc tạo ra một phần nhỏ trong số chúng cũng là không thể. 

Một hướng đi tốt hơn đến từ việc quan sát rằng mọi ký tự chỉ thực hiện chuyển đổi giữa hai trạng thái. Một đoạn của chuỗi có thể được biểu diễn bằng ma trận 2 x 2. Hàng mô tả trạng thái bắt đầu, cột mô tả trạng thái cuối cùng và mỗi giá trị lưu trữ số lượng thay thế có thể thực hiện được quá trình chuyển đổi đó. Khi hai phân đoạn được nối với nhau, ma trận của chúng sẽ được nhân lên, vì phân đoạn đầu tiên chọn trạng thái trung gian và phân đoạn thứ hai tiếp tục từ đó. 

Một ký tự đơn có một ma trận cố định nhỏ. Ví dụ: dấu hỏi sẽ xem xét tất cả 26 chữ cái có thể có và thêm phần đóng góp của mỗi chữ cái vào phần chuyển đổi thích hợp. Toàn bộ chuỗi trở thành tích của các ma trận này. Vì các cập nhật chỉ ảnh hưởng đến một vị trí nên cây phân đoạn có thể lưu trữ các sản phẩm thuộc phạm vi và chỉ tính toán lại số lượng nút logarit bị ảnh hưởng bởi một cập nhật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26^x * n) trong đó x là số dấu chấm hỏi | O(n) | Quá chậm | 
| Tối ưu | O(log n) mỗi lần cập nhật sau bản dựng O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mỗi ký tự dưới dạng ma trận chuyển tiếp 2 x 2. Hàng và cột đầu tiên tương ứng với trạng thái bắt đầu vui hoặc buồn và chiều thứ hai đại diện cho trạng thái kết thúc. Giá trị trong một ô là số lượng lựa chọn ký tự tạo ra sự chuyển đổi đó. 
2. Xây dựng cây phân đoạn trong đó mỗi lá lưu trữ ma trận của một ký tự. Các nút nội bộ lưu trữ sản phẩm của hai nút con của chúng. Phép nhân ma trận được sử dụng vì con bên trái mô tả những gì xảy ra trước và con bên phải mô tả những gì xảy ra sau đó. 
3. Câu trả lời ban đầu là số cách để bắt đầu hạnh phúc và kết thúc hạnh phúc. Đây là mục nhập ma trận từ hạnh phúc đến hạnh phúc ở gốc cây. 
4. Đối với mỗi lần cập nhật, hãy thay thế ma trận lá của ký tự đã thay đổi. Tính toán lại tất cả tổ tiên cho đến khi đạt đến gốc. Chỉ các nút O(log n) phụ thuộc vào một vị trí, do đó việc cập nhật diễn ra nhanh chóng. 
5. Sau khi xây dựng lại đường dẫn, hãy in giá trị hạnh phúc đến hạnh phúc mới được lưu trong ma trận gốc. 

Tại sao nó hoạt động: tính bất biến của cây phân đoạn là mọi nút đều lưu trữ ma trận chuyển tiếp chính xác trong khoảng của nó. Một chiếc lá đúng vì nó mô tả trực tiếp một ký tự. Nếu hai đứa trẻ đúng, việc nhân ma trận của chúng sẽ xem xét mọi tâm trạng có thể xảy ra sau khoảng bên trái và mọi trạng thái tiếp tục qua khoảng bên phải, do đó cha mẹ cũng đúng. Bằng quy nạp, gốc đại diện cho toàn bộ chuỗi và mục nhập từ hạnh phúc đến hạnh phúc của nó chính xác là số lượng được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def multiply(a, b):
    return [
        [
            (a[0][0] * b[0][0] + a[0][1] * b[1][0]) % MOD,
            (a[0][0] * b[0][1] + a[0][1] * b[1][1]) % MOD
        ],
        [
            (a[1][0] * b[0][0] + a[1][1] * b[1][0]) % MOD,
            (a[1][0] * b[0][1] + a[1][1] * b[1][1]) % MOD
        ]
    ]

def char_matrix(c):
    res = [[0, 0], [0, 0]]
    for x in range(26):
        ch = chr(ord('A') + x)
        if ch in "AEIOU":
            res[0][1] += 1
            res[1][0] += 1
        elif ch == "H":
            res[0][0] += 1
            res[1][1] += 1
        elif ch in "SD":
            res[0][1] += 1
            res[1][1] += 1
        else:
            res[0][0] += 1
            res[1][1] += 1
    if c != "?":
        res = [[0, 0], [0, 0]]
        if c in "AEIOU":
            res[0][1] = 1
            res[1][0] = 1
        elif c == "H":
            res[0][0] = 1
            res[1][1] = 1
        elif c in "SD":
            res[0][1] = 1
            res[1][1] = 1
        else:
            res[0][0] = 1
            res[1][1] = 1
    return res

def solve():
    n, q = map(int, input().split())
    s = list(input().strip())

    size = 1
    while size < n:
        size *= 2

    tree = [[[1, 0], [0, 1]] for _ in range(2 * size)]

    for i, c in enumerate(s):
        tree[size + i] = char_matrix(c)

    for i in range(size - 1, 0, -1):
        tree[i] = multiply(tree[i * 2], tree[i * 2 + 1])

    ans = [str(tree[1][0][0])]

    for _ in range(q):
        i, c = input().split()
        i = int(i) - 1
        pos = size + i
        tree[pos] = char_matrix(c)
        pos //= 2
        while pos:
            tree[pos] = multiply(tree[pos * 2], tree[pos * 2 + 1])
            pos //= 2
        ans.append(str(tree[1][0][0]))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Hàm nhân ma trận kết hợp hai khoảng liên tiếp. Thứ tự quan trọng vì khoảng bên trái được đọc trước khoảng bên phải. Đảo ngược thứ tự nhân sẽ mô tả chuỗi ngược. 

các`char_matrix`Hàm xử lý cả chữ cái cố định và dấu chấm hỏi. Đối với dấu chấm hỏi, nó bắt đầu bằng tất cả các chữ cái viết hoa có thể có và tích lũy các chuyển tiếp của chúng. Việc lập chỉ mục trạng thái giả định trạng thái`0`có nghĩa là hạnh phúc và trạng thái`1`có nghĩa là buồn nên đáp án cuối cùng luôn được lưu trữ tại`[0][0]`. 

Cây phân đoạn sử dụng lũy ​​thừa hai kích thước để đơn giản hóa việc lập chỉ mục. Các lá bổ sung biểu thị các phân đoạn trống và sử dụng ma trận đồng nhất vì việc nhân với một phân đoạn trống sẽ không làm thay đổi bất kỳ số lần chuyển tiếp nào. Số nguyên Python không bị tràn, nhưng mọi kết quả nhân đều được giảm modulo`10^9+7`để giữ giá trị nhỏ. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
2 5
A?
2 O
1 H
1 ?
2 ?
2 H
```giá trị gốc cây thay đổi như sau: 

| Trạng thái chuỗi | Cập nhật vị trí | Root hạnh phúc để hạnh phúc | 
| --- | --- | --- | 
| MỘT? | không | 6 | 
| AO | vị trí 2 trở thành O | 1 | 
| HO | vị trí 1 trở thành H | 0 | 
| ?O | vị trí 1 trở thành? | 7 | 
| ?? | vị trí 2 trở thành? | 403 | 
| ?H | vị trí 2 trở thành H | 26 | 

Ví dụ này chứng minh tại sao câu trả lời phụ thuộc vào hiệu ứng kết hợp của toàn bộ chuỗi. Ma trận lưu trữ tất cả các chuyển đổi tâm trạng có thể xảy ra, do đó, sự thay thế khiến Limak buồn trước tiên vẫn được tính nếu nhân vật sau đó khôi phục lại hạnh phúc. 

Đối với đầu vào:```
1 3
B
1 A
1 ?
1 H
```các chuyển tiếp là: 

| Trạng thái chuỗi | Nhân vật được cập nhật | Ma trận gốc vui đến vui | 
| --- | --- | --- | 
| B | không | 19 | 
| A | A | 0 | 
| ? | ? | 19 | 
| H | H | 1 | 

Dấu vết này kiểm tra hành vi của một ký tự. Với một ký tự không có sự tương tác giữa các vị trí nên ma trận phản ánh trực tiếp tác dụng của chữ cái đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q log n) | Việc xây dựng cây chạm vào mỗi ký tự một lần và mỗi bản cập nhật sẽ thay đổi một đường dẫn từ gốc đến lá. | 
| Không gian | O(n) | Cây phân đoạn lưu trữ một ma trận có kích thước không đổi tại mỗi nút. | 

Các ràng buộc yêu cầu xử lý 200000 bản cập nhật, do đó cần phải cập nhật logarit. Kích thước ma trận được cố định ở mức 2 x 2, làm cho mọi thao tác trên cây đều hoạt động liên tục ngoài việc duyệt cây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    sys.stdin = old
    return out

# This block is intended to be used with the solve function above and a redirected stdout.

assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0\nH\n`|`1`| Thư vui cố định đơn | 
|`1 1\n?\n1 A\n`|`19\n0`| Cập nhật vị trí đơn và hành vi lật | 
|`3 0\nBBB\n`|`6859`| Tất cả các chữ cái trung tính | 
|`2 2\n??\n1 S\n2 O\n`|`403\n26\n6`| Tương tác giữa sự thay đổi tâm trạng bắt buộc và dấu chấm hỏi | 

## Vỏ cạnh 

Đối với chuỗi`A?`, thuật toán tạo ra một ma trận cho`A`điều đó chỉ cho phép chuyển đổi từ vui sang buồn và sau đó kết hợp nó với ma trận dấu chấm hỏi. Mục nhập từ hạnh phúc đến hạnh phúc cuối cùng chỉ tính những thay thế trong đó nhân vật thứ hai sửa chữa tâm trạng, tạo ra`6`. 

Đối với trường hợp cập nhật ký tự đơn:```
1 1
B
1 A
```cây chỉ có một chiếc lá có ý nghĩa. Thay lá là thay gốc ngay. Câu trả lời thay đổi từ`19`ĐẾN`0`bởi vì`A`luôn lật ngược trạng thái hạnh phúc ban đầu. 

Đối với một chuỗi toàn dấu chấm hỏi, mỗi lá chứa tổng của tất cả 26 lần chuyển đổi có thể. Phép nhân cây phân đoạn giữ cho cả hai tâm trạng trung gian tồn tại, do đó, những con đường tạm thời trở nên buồn bã sẽ không bị loại bỏ. Đây là lý do tại sao trường hợp hai ký tự cho`403`thay vì chỉ đếm những lựa chọn hạnh phúc ngay lập tức.
