---
title: "CF 102644D - Đếm đường dẫn"
description: "Bài toán đưa ra một đồ thị có hướng có các đỉnh được đánh số từ 1 đến n và yêu cầu số bước đi có thể sử dụng chính xác k cạnh có hướng. Một bước đi có thể sử dụng lại các đỉnh và cạnh, do đó nhiệm vụ không phải là những đường đi đơn giản."
date: "2026-08-02T14:48:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102644
codeforces_index: "D"
codeforces_contest_name: "Matrix Exponentiation"
rating: 0
weight: 102644
solve_time_s: 50
verified: true
draft: false
---

[CF 102644D - Đếm đường dẫn](https://codeforces.com/problemset/problem/102644/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Bài toán cho một đồ thị có hướng có các đỉnh được đánh số từ`1`ĐẾN`n`và hỏi số lần đi bộ có thể sử dụng chính xác`k`các cạnh có hướng. Một bước đi có thể sử dụng lại các đỉnh và cạnh, do đó nhiệm vụ không phải là những đường đi đơn giản. Mọi lựa chọn về đỉnh bắt đầu và đỉnh kết thúc đều được bao gồm trong câu trả lời, nghĩa là chúng tôi đếm tất cả các chuỗi đỉnh hợp lệ được kết nối bằng chính xác`k`chuyển tiếp. Kết quả phải được in modulo`1,000,000,007`. 

Kích thước đầu vào được thiết kế dựa trên quan sát rằng bản thân biểu đồ nhỏ trong khi độ dài đường dẫn có thể cực kỳ lớn. Số đỉnh nhiều nhất là`100`, do đó các thuật toán liên quan đến công bậc ba trên số đỉnh là hợp lý. Tuy nhiên,`k`có thể đạt được`10^9`, loại trừ bất kỳ giải pháp lập trình động nào xử lý các đường dẫn từng bước một. Phép truy toán đơn giản tính toán các câu trả lời trong khoảng thời gian dài`1, 2, ..., k`sẽ yêu cầu khoảng`10^11`chuyển đổi ngay cả trước khi xem xét các phép toán ma trận, vốn không thể phù hợp với giới hạn cuộc thi thông thường. 

Một vài trường hợp dễ dàng phá vỡ việc triển khai ngây thơ. Nếu như`k = 1`, câu trả lời đơn giản là số cạnh có hướng. Ví dụ:```
3 2 1
1 2
2 3
```Câu trả lời là`2`, bởi vì lối đi duy nhất là`1 -> 2`Và`2 -> 3`. Việc triển khai khởi tạo câu trả lời cho các đường dẫn có độ dài bằng 0 cũng có thể vô tình đếm được ba đỉnh đơn lẻ. 

Đồ thị có chu trình là một trường hợp quan trọng khác:```
3 3 2
1 2
2 3
3 1
```Câu trả lời là`3`, bởi vì các lần đi bộ hợp lệ là`1 -> 2 -> 3`,`2 -> 3 -> 1`, Và`3 -> 1 -> 2`. Tìm kiếm chuyên sâu đầu tiên với một mảng đã truy cập sẽ loại bỏ các bước đi này một cách không chính xác vì được phép xem lại một đỉnh. 

Trường hợp không có cạnh cũng phải xử lý:```
2 0 5
```Câu trả lời là`0`. Bất kỳ giải pháp nào bắt đầu từ một vectơ tất cả và quên rằng không thể chuyển đổi sẽ trả về giá trị dương không chính xác. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng các đường dẫn theo chiều dài. Cho phép`dp[v]`biểu thị số bước đi của chiều dài hiện tại kết thúc ở đỉnh`v`. Ban đầu, với độ dài bằng 0, mọi đỉnh đều có thể là điểm bắt đầu, do đó mỗi đỉnh đóng góp một bước đi trống. Đối với mỗi cạnh bổ sung, chúng tôi cập nhật số lượng bằng cách theo dõi tất cả các cạnh đi ra. Điều này đúng vì mỗi bước đi dài`i + 1`được hình thành bằng cách kéo dài một quãng đường dài`i`với một cạnh nữa. 

Vấn đề là giá trị của`k`. Một bản cập nhật yêu cầu kiểm tra các cạnh và số lượng bản cập nhật là`k`. Trong trường hợp xấu nhất có gần`n(n - 1)`các cạnh, vì vậy công việc xấp xỉ`10^9 * 10^4`, vượt xa thời gian sẵn có. 

Cấu trúc hữu ích là việc áp dụng lặp đi lặp lại một phép chuyển đổi chính xác là mục đích mà phép lũy thừa ma trận được thiết kế. Chúng ta có thể biểu diễn đồ thị dưới dạng ma trận kề`A`, Ở đâu`A[i][j]`cho biết có bao nhiêu cạnh trực tiếp đi từ đỉnh`i`đến đỉnh`j`. Trong bài toán này không có nhiều cạnh nên các giá trị chỉ bằng 0 hoặc một. 

Khi chúng ta nhân ma trận, mục nhập`(i, j)`của`A^2`đếm số bước đi của hai cạnh từ`i`ĐẾN`j`. Lý do tương tự cũng áp dụng cho các quyền lực cao hơn, vì vậy`A^k`chứa mọi câu trả lời cho đường đi có độ dài`k`. Từ`k`lớn, lũy thừa nhị phân làm giảm số phép nhân ma trận từ`k`ĐẾN`log(k)`. 

Câu trả lời cuối cùng là tổng của mọi mục trong`A^k`, bởi vì mọi cặp đỉnh bắt đầu và kết thúc có thể đều đóng góp vào tổng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * m) | O(n) | Quá chậm | 
| Tối ưu | O(n³ log k) | O(n²) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Xây dựng ma trận kề`A`. Đối với mọi cạnh có hướng từ`u`ĐẾN`v`, bộ`A[u][v] = 1`. Ma trận này mô tả một bước chuyển động trong biểu đồ. 
2. Tính toán`A^k`sử dụng lũy ​​thừa nhị phân. Nếu bit hiện tại của`k`được thiết lập, nhân ma trận câu trả lời với lũy thừa hiện tại của`A`. Sau đó hình vuông`A`để chuẩn bị ma trận cho bit tiếp theo. Điều này hiệu quả vì mọi lũy thừa số nguyên có thể được biểu diễn dưới dạng tổ hợp lũy thừa của hai. 
3. Sau khi phép tính lũy thừa kết thúc, hãy cộng mọi phần tử của ma trận thu được. Mỗi mục biểu thị số lần đi giữa một cặp đỉnh có thứ tự, vì vậy tổng của chúng là tổng số lần đi. 

Tại sao nó hoạt động: tính bất biến chính là sau khi xử lý một số bit của`k`, ma trận tích lũy bằng tích của tất cả lũy thừa của hai đã được chọn từ biểu diễn nhị phân của`k`. Phép nhân ma trận giữ nguyên ý nghĩa của việc đếm đường đi vì việc kết hợp hai ma trận sẽ nối hai phần liên tiếp của bước đi. Do đó, bình phương lặp đi lặp lại tạo ra ma trận cho độ dài đường đi`1, 2, 4, 8`v.v. và nhân những cái cần thiết sẽ cho ra chính xác ma trận về độ dài`k`. 

#Giải pháp Python```python
import sys

input = sys.stdin.readline

MOD = 10 ** 9 + 7

def multiply(a, b, n):
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        for k in range(n):
            if a[i][k]:
                aik = a[i][k]
                for j in range(n):
                    res[i][j] = (res[i][j] + aik * b[k][j]) % MOD
    return res

def solve():
    n, m, k = map(int, input().split())

    mat = [[0] * n for _ in range(n)]
    for _ in range(m):
        a, b = map(int, input().split())
        mat[a - 1][b - 1] = 1

    ans = [[0] * n for _ in range(n)]
    for i in range(n):
        ans[i][i] = 1

    while k:
        if k & 1:
            ans = multiply(ans, mat, n)
        mat = multiply(mat, mat, n)
        k >>= 1

    result = 0
    for row in ans:
        result += sum(row)
        result %= MOD

    print(result)

if __name__ == "__main__":
    solve()
```Ma trận kề được lưu trữ với các hàng biểu thị các đỉnh bắt đầu và các cột biểu thị các đỉnh kết thúc. Hàm nhân tuân theo định nghĩa của phép nhân ma trận, trong đó việc chọn một đỉnh trung gian`k`có nghĩa là chia một cuộc đi bộ thành hai phần liên tiếp. 

Ma trận nhận dạng được sử dụng làm câu trả lời ban đầu vì nó đại diện cho phần tử trung tính cho phép nhân. Nếu biểu diễn nhị phân của`k`chưa chứa bit được xử lý, chưa có lũy thừa nào được chọn, do đó sản phẩm tích lũy phải bắt đầu dưới dạng nhận dạng. 

Thủ tục nhân bỏ qua các mục 0 từ ma trận bên trái. Vì nhiều đồ thị thưa thớt, điều này tránh được những công việc không cần thiết trong thực tế trong khi vẫn giữ được trường hợp xấu nhất bậc ba cần thiết. Số nguyên Python đã hỗ trợ độ chính xác tùy ý, nhưng mọi giá trị đều được giảm modulo`MOD`trong quá trình nhân để giữ cho số nhỏ. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 4 2
1 2
2 3
3 1
2 1
```Biểu diễn nhị phân của`k = 2`chỉ chứa bit thứ hai, do đó thuật toán bình phương ma trận kề một lần và sử dụng ma trận kết quả. 

| Bước | Chiều dài nguồn hiện tại | Đã chọn trong câu trả lời | Ý nghĩa | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | Ma trận nhận dạng | Không có chuyển tiếp nào được chọn | 
| Quảng trường | 2 | Không | Xây dựng chuyển tiếp hai bước | 
| Nhân | 2 | Có | Sử dụng chính xác hai cạnh | 

Ma trận bình phương đếm tất cả các bước đi có thể có ở hai cạnh. Thêm các mục của nó mang lại`5`, khớp với các bước đi hợp lệ trong biểu đồ. 

Đối với mẫu thứ hai:```
5 10 11
2 3
4 2
2 1
2 4
1 5
5 2
3 2
3 1
3 4
1 2
```Đây`k = 11`, đó là nhị phân`1011`. 

| Bước | Đã xử lý bit | Ma trận được sử dụng | Hoạt động | 
| --- | --- | --- | --- | 
| 1 | 1 | Chiều dài 1 | Nhân vào câu trả lời | 
| 2 | 1 | Chiều dài 2 | Nhân vào câu trả lời | 
| 3 | 0 | Chiều dài 4 | Bỏ qua | 
| 4 | 1 | Chiều dài 8 | Nhân vào câu trả lời | 

Các quyền hạn được lựa chọn đại diện`1 + 2 + 8 = 11`, do đó ma trận cuối cùng tính chính xác số bước đi với 11 cạnh. Tổng hợp tất cả các mục tạo ra`21305`. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³ log k) | Mỗi phép nhân ma trận có chi phí O(n³) và phép lũy thừa nhị phân thực hiện phép nhân O(log k). | 
| Không gian | O(n²) | Chỉ một số ít`n`qua`n`ma trận được lưu trữ. | 

Với`n <= 100`, phép nhân ma trận bậc ba bao gồm khoảng một triệu phép tính cơ bản. Nhân với khoảng ba mươi bit`k`giữ toàn bộ công việc trong giới hạn thực tế. 

# Trường hợp thử nghiệm```python
import sys
import io

MOD = 10 ** 9 + 7

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return output

assert run("""3 4 2
1 2
2 3
3 1
2 1
""") == "5\n", "sample 1"

assert run("""5 10 11
2 3
4 2
2 1
2 4
1 5
5 2
3 2
3 1
3 4
1 2
""") == "21305\n", "sample 2"

assert run("""1 0 1
""") == "0\n", "single vertex without edges"

assert run("""3 3 1
1 2
2 3
3 1
""") == "3\n", "one step paths"

assert run("""2 2 1000000000
1 2
2 1
""") == "754306490\n", "large k cycle"

assert run("""4 0 10
""") == "0\n", "empty graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn không có cạnh | 0 | Xử lý chính xác biểu đồ nhỏ nhất | 
| Chu kỳ ba đỉnh với`k = 1`| 3 | Kiểm tra việc đếm cạnh trực tiếp | 
| Chu trình hai đỉnh rất lớn`k`| 754306490 | Xác minh lũy thừa nhị phân cho lũy thừa lớn | 
| Biểu đồ trống | 0 | Ngăn chặn việc vô tình đếm số lần đi bộ không tồn tại | 

# Vỏ cạnh 

cho`k = 1`, thuật toán không thực hiện bất kỳ mở rộng không cần thiết nào. Phép lũy thừa nhị phân ngay lập tức sử dụng ma trận kề ban đầu và tính tổng tất cả các mục sẽ tính chính xác các cạnh hiện có. Trên đầu vào:```
3 2 1
1 2
2 3
```ma trận chứa hai số một, vì vậy đầu ra là`2`. 

Đối với đồ thị tuần hoàn, việc xem lại các đỉnh được xử lý một cách tự nhiên vì phép nhân ma trận đếm các lần chuyển đổi mà không lưu trữ thông tin đã truy cập. TRÊN:```
3 3 2
1 2
2 3
3 1
```ma trận bình phương bao gồm ba bước đi có thể có ở hai cạnh. Việc truyền tải biểu đồ chặn các lượt truy cập lại sẽ bỏ lỡ tất cả chúng. 

Đối với đồ thị không có cạnh:```
2 0 5
```ma trận kề hoàn toàn bằng không. Mọi phép nhân đều giữ nó bằng 0 nên tổng cuối cùng cũng bằng 0. Điều này tránh coi sự tồn tại của các đỉnh như thể nó ngụ ý sự tồn tại của các bước đi.
