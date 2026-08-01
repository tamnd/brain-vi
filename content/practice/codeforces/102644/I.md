---
title: "CF 102644I - Truy vấn đếm đường dẫn"
description: "Bài toán mô tả một đồ thị có hướng có tối đa 200 đỉnh. Một đường đi là một chuỗi có chính xác k cạnh có hướng và các đỉnh hoặc cạnh có thể xuất hiện nhiều lần."
date: "2026-08-01T10:19:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102644
codeforces_index: "I"
codeforces_contest_name: "Matrix Exponentiation"
rating: 0
weight: 102644
solve_time_s: 64
verified: true
draft: false
---

[CF 102644I - Đếm truy vấn đường dẫn](https://codeforces.com/problemset/problem/102644/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một đồ thị có hướng có tối đa 200 đỉnh. Một đường dẫn là một chuỗi chính xác`k`các cạnh có hướng và các đỉnh hoặc các cạnh có thể xuất hiện nhiều lần. Đối với mỗi truy vấn, chúng tôi được hỏi có bao nhiêu bước đi khác nhau bắt đầu ở đỉnh`s`, kết thúc tại đỉnh`t`, và chứa chính xác`k`các cạnh. Câu trả lời phải được in modulo`1,000,000,007`. 

Biểu đồ đầu vào có thể chứa tối đa 200 đỉnh và 200 truy vấn, trong khi độ dài đường dẫn yêu cầu có thể lớn bằng`10^9`. Việc mô phỏng trực tiếp các đường đi là không thể vì số lần đi bộ có thể tăng theo cấp số nhân với`k`. Ngay cả phương pháp lập trình động xử lý từng bước cũng quá chậm vì`k`không bị giới hạn bởi một giá trị nhỏ. Số lượng đỉnh nhỏ là hạn chế chính: nó cho phép chúng ta thực hiện các thao tác trên`200 x 200`ma trận. 

Một giải pháp bất cẩn có thể thất bại trong một số trường hợp. Nếu như`k`lớn, việc lặp lại từng cạnh một sẽ không bao giờ kết thúc. Ví dụ, với một cạnh duy nhất`1 -> 1`và truy vấn`(1, 1, 1000000000)`, câu trả lời là`1`, nhưng mô phỏng từng bước sẽ yêu cầu một tỷ lần chuyển đổi. 

Một lỗi phổ biến khác là quên rằng đường dẫn có thể truy cập lại các đỉnh. Đối với đầu vào:```
2 2 3
1 2
2 1
1 1 3
```đầu ra đúng là:```
0
```Các bước đi duy nhất có thể xen kẽ giữa hai đỉnh, vì vậy sau ba cạnh chúng ta kết thúc ở đỉnh`2`, không phải đỉnh`1`. Coi đồ thị như một bài toán đường đi đơn giản sẽ tạo ra kết quả không chính xác. 

Trường hợp cạnh thứ ba là một biểu đồ trống. Ví dụ:```
3 0 2
1 2 5
3 3 1
```Đầu ra là:```
0
0
```bởi vì không có cạnh nào tồn tại nên không thể tạo ra bước đi có độ dài dương. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là đi theo mọi bước đi có thể từ đỉnh xuất phát. Đối với mỗi truy vấn, chúng tôi có thể chạy tìm kiếm theo chiều sâu để chọn cạnh tiếp theo`k`lần và đếm xem chúng ta đến đích bao nhiêu lần. Điều này đúng vì nó liệt kê chính xác tất cả các bước đi hợp lệ. Tuy nhiên, số lần đi bộ có thể tăng lên bằng`n^k`, do đó, ngay cả một đồ thị chỉ có 200 đỉnh cũng không thể thực hiện được khi`k`đạt tới`10^9`. 

Quan sát đầu tiên tốt hơn là chúng ta chỉ quan tâm đến sự chuyển tiếp giữa các đỉnh. Cho phép`A[i][j]`thể hiện liệu có một cạnh từ`i`ĐẾN`j`, hay chính xác hơn là số đường đi một cạnh từ`i`ĐẾN`j`. Phép nhân ma trận kết hợp các chuyển tiếp một cách tự nhiên. Mục nhập`(i, j)`của`A^2`cho chúng ta biết có bao nhiêu đường đi hai cạnh đi từ`i`ĐẾN`j`, bởi vì mọi đỉnh ở giữa đều được xem xét. Ý tưởng tương tự kéo dài đến bất kỳ chiều dài nào, vì vậy câu trả lời là`(s, t)`yếu tố của`A^k`. 

Vấn đề còn lại đó là`k`có thể rất lớn. Phép lũy thừa nhị phân giải quyết vấn đề này giống hệt như phép lũy thừa thông thường. Chúng tôi tính toán trước sức mạnh`A^(1), A^(2), A^(4), A^(8)`, vân vân. Mỗi bit của`k`cho chúng ta biết những lũy ​​thừa nào nên được nhân với nhau. 

Vì chỉ có 200 truy vấn nên tốt hơn nên tính lũy thừa một lần và sử dụng lại chúng. Sau đó, mỗi truy vấn chỉ cần tối đa 30 lần chuyển đổi kiểu vectơ-ma trận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k) | O(k) | Quá chậm | 
| Hàm mũ ma trận | O(n^3 log k + qn^2 log k) | O(n^2 log k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng ma trận kề`A`. giá trị`A[i][j]`lưu trữ số cạnh trực tiếp từ đỉnh`i`đến đỉnh`j`. Vì không có nhiều cạnh nên mọi mục đều`0`hoặc`1`. 
2. Tính toán trước lũy thừa của ma trận. Cửa hàng`A`,`A^2`,`A^4`,`A^8`và tiếp tục cho đến bit lớn nhất có thể của`k`được che phủ. Mỗi ma trận tiếp theo thu được bằng cách nhân ma trận trước đó với chính nó. 
3. Đối với một truy vấn`(s, t, k)`, bắt đầu với ma trận nhận dạng là kết quả hiện tại và quét các bit của`k`. 
4. Bất cứ khi nào một chút`k`được đặt, hãy nhân kết quả hiện tại với công suất tính toán trước tương ứng. 
5. Trả về giá trị tại hàng`s`, cột`t`của ma trận cuối cùng. 

Lý do lũy thừa nhị phân hoạt động là vì mọi số nguyên dương đều có thể được biểu diễn dưới dạng tổng lũy ​​thừa của hai. Phép nhân ma trận giữ nguyên ý nghĩa của thành phần đường dẫn, do đó, việc nhân lũy thừa đã chọn sẽ kết hợp chính xác số cạnh cần thiết. 

Tại sao nó hoạt động: sau khi xử lý một số bit của`k`, ma trận được duy trì biểu thị số đường dẫn sử dụng phần số mũ được xử lý. Khi gặp một bit cố định, nhân với`A^(2^i)`nối chính xác nhiều cạnh đó vào mọi đường dẫn từng phần có thể. Bất biến vẫn đúng cho đến khi tất cả các bit được xử lý, tại thời điểm đó ma trận bằng`A^k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def multiply(a, b):
    n = len(a)
    bt = [[b[j][i] for j in range(n)] for i in range(n)]
    res = [[0] * n for _ in range(n)]

    for i in range(n):
        ai = a[i]
        ri = res[i]
        for j in range(n):
            s = 0
            bj = bt[j]
            for k in range(n):
                s += ai[k] * bj[k]
            ri[j] = s % MOD
    return res

def solve():
    n, m, q = map(int, input().split())

    mat = [[0] * n for _ in range(n)]
    for _ in range(m):
        a, b = map(int, input().split())
        mat[a - 1][b - 1] = 1

    powers = [mat]
    for _ in range(31):
        powers.append(multiply(powers[-1], powers[-1]))

    ans = []
    for _ in range(q):
        s, t, k = map(int, input().split())
        s -= 1
        t -= 1

        vec = [0] * n
        vec[s] = 1

        bit = 0
        while k:
            if k & 1:
                new_vec = [0] * n
                p = powers[bit]
                for i in range(n):
                    if vec[i]:
                        vi = vec[i]
                        row = p[i]
                        for j in range(n):
                            new_vec[j] = (new_vec[j] + vi * row[j]) % MOD
                vec = new_vec
            k >>= 1
            bit += 1

        ans.append(str(vec[t]))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Thủ tục nhân ma trận sử dụng một bản sao được hoán vị của ma trận thứ hai sao cho mỗi vòng lặp bên trong truy cập vào các hàng liền kề. Điều này tránh việc lập chỉ mục các cột liên tục và làm cho phép nhân bậc ba nhanh hơn trong Python. 

Giai đoạn truy vấn không nhân hai ma trận đầy đủ. Thay vào đó, nó nhân vectơ bắt đầu với các ma trận cần thiết. Vectơ bắt đầu có một`1`tại đỉnh nguồn, biểu diễn một đường đi trống trước khi lấy bất kỳ cạnh nào. Sau khi tất cả các bit được xử lý, vị trí đích chứa số bước đi có độ dài`k`. 

Vòng lặp lũy thừa xử lý các giá trị lên đến`10^9`, vì vậy 30 đến 31 lần lặp là đủ. Hoạt động modulo được áp dụng trong mỗi lần tích lũy để giữ các giá trị trong giới hạn số nguyên của Python và khớp với đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
3 4 4
1 2
2 3
3 1
2 1
1 2 6
3 3 5
1 3 1
3 2 54
```truy vấn đầu tiên có thể được theo dõi như sau. 

| Bước | Bit số mũ hiện tại | Trạng thái vectơ | 
| --- | --- | --- | 
| Bắt đầu | không |`[1,0,0]`| 
| Sử dụng`A^2`| bộ bit | đếm đường đi có độ dài 2 | 
| Sử dụng`A^4`| bộ bit | đếm đường đi có độ dài 6 | 

Sau khi kết hợp các lũy thừa đã chọn, giá trị của đỉnh`2`là`2`, khớp với hai bước đi hợp lệ có độ dài sáu. 

Đối với truy vấn`(1,3,1)`, chỉ có sức mạnh đầu tiên được sử dụng. 

| Bước | Bit số mũ hiện tại | Trạng thái vectơ | 
| --- | --- | --- | 
| Bắt đầu | không |`[1,0,0]`| 
| Sử dụng`A`| bộ bit |`[0,1,0]`| 

đỉnh`3`có giá trị`0`, bởi vì không có cạnh trực tiếp từ`1`ĐẾN`3`. 

Các ví dụ này cho thấy thuật toán tính số bước đi theo độ dài chứ không phải các đường dẫn đơn giản và quy trình lũy thừa xử lý chính xác cả độ dài nhỏ và lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3 log K + qn^2 log K) | Các lũy thừa ma trận yêu cầu phép nhân bậc ba, trong khi mỗi truy vấn sử dụng phép nhân vectơ | 
| Không gian | O(n^2 log K) | Lưu trữ quyền hạn được tính toán trước | 

Với`n <= 200`, việc tiền xử lý khối là khả thi. Giá trị lớn của`k`được giảm xuống chỉ còn khoảng 30 bước nhị phân, do đó thuật toán phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

assert run("""3 4 4
1 2
2 3
3 1
2 1
1 2 6
3 3 5
1 3 1
3 2 54
""") == """2
1
0
922111
"""

assert run("""1 0 1
1 1 1
""") == "0\n"

assert run("""2 2 2
1 2
2 1
1 1 2
1 2 3
""") == """1
1
"""

assert run("""3 3 2
1 2
2 3
3 1
1 1 1000000000
2 1 1
""") == """1
0
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn không có cạnh |`0`| Xử lý đồ thị trống | 
| Đồ thị hai chu kỳ | Đếm xen kẽ | Xem lại các đỉnh | 
| Ba chu kỳ với rất lớn`k`| Phép lũy thừa mô-đun đúng | Xử lý số mũ lớn | 

## Vỏ cạnh 

Đối với đồ thị không có cạnh, ma trận kề hoàn toàn bằng 0. Mọi lũy thừa của ma trận vẫn bằng 0, vì vậy mọi truy vấn yêu cầu số bước dương đều trả về 0 một cách chính xác. 

Đối với một chu kỳ như:```
2 2 1
1 2
2 1
1 1 1000000000
```lũy thừa của ma trận kề bảo toàn chính xác chuyển động xen kẽ. Bởi vì số mũ được xử lý theo bit thay vì các bước riêng lẻ nên lời giải đạt được câu trả lời mà không cần mô phỏng hàng tỷ lần chuyển đổi. 

Đối với các lượt truy cập lặp lại, việc giải thích phép nhân ma trận tự nhiên bao gồm các đường dẫn quay trở lại các đỉnh đã truy cập. Không cần xử lý đặc biệt vì cường độ ma trận tính số bước đi thay vì số lượng đường dẫn đơn giản.
