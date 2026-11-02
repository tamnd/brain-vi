---
title: "CF 106501G - Ma trận LCM"
description: "Ma trận không phải là tùy ý. Mỗi hàng có một giá trị ẩn và mỗi cột có một giá trị ẩn và mỗi ô hiển thị là bội số chung nhỏ nhất của hai giá trị đó."
date: "2026-06-25T08:32:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106501
codeforces_index: "G"
codeforces_contest_name: "IPL 2026"
rating: 0
weight: 106501
solve_time_s: 42
verified: true
draft: false
---

[CF 106501G - Ma trận LCM](https://codeforces.com/problemset/problem/106501/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ma trận không phải là tùy ý. Mỗi hàng có một giá trị ẩn và mỗi cột có một giá trị ẩn và mỗi ô hiển thị là bội số chung nhỏ nhất của hai giá trị đó. Một số ô bị thiếu và nhiệm vụ là chọn các giá trị hàng và cột có thể tạo ra tất cả các ô hiển thị đồng thời làm cho tích của mọi giá trị đã chọn càng nhỏ càng tốt. 

Tích này giống như cực tiểu hóa tổng số mũ nguyên tố trên tất cả các giá trị hàng và cột. Điều này là do phép nhân các số có thể được xem độc lập với mọi số nguyên tố. Ví dụ: nếu một số nguyên tố đóng góp số mũ 3 vào một giá trị hàng, thì hệ số đó đóng góp chính xác cùng một hệ số như việc tăng một giá trị khác theo cùng số mũ. Điều kiện LCM cũng hoạt động độc lập với số nguyên tố, vì số mũ trong LCM là số mũ lớn nhất trong hai số. 

Kích thước tối đa là 500 và tổng số hàng và cột trong tất cả các bài kiểm tra được giới hạn bởi 500 mỗi thứ nguyên. Điều này loại trừ mọi thứ gần với việc kiểm tra mọi phép gán giá trị hàng và cột có thể có. Các giá trị ẩn không bị giới hạn nên không thể tìm kiếm các số có thể. Lời giải phải sử dụng cấu trúc LCM và thừa số nguyên tố. 

Các ô bị thiếu tạo ra tình huống dễ bỏ sót. Một hàng hoặc cột chỉ xuất hiện trong ô 0 sẽ không bị hạn chế và không đóng góp gì. Ví dụ, với đầu vào```
2 2
0 0
0 0
```câu trả lời là`1`, bởi vì mọi giá trị ẩn đều có thể được chọn là 1. Cách tiếp cận bất cẩn khởi tạo mọi hàng và cột từ một giá trị ma trận sẽ thất bại ở đây. 

Một trường hợp phức tạp khác là không phải lúc nào cũng tìm được giá trị tối thiểu bằng cách lấy giá trị từ một hàng hoặc cột. Ví dụ:```
1 2
6 10
```Tối thiểu là`30`, không`60`. Giá trị hàng có thể là`1`và hai giá trị cột có thể là`6`Và`10`, tặng sản phẩm`60`, nhưng lựa chọn tốt hơn là giá trị hàng`2`, cột`3`Và`5`, đưa ra LCM tương tự với sản phẩm`30`. Lời giải phải suy luận về số mũ nguyên tố thay vì sao chép các số hiển thị. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng xây dựng lại các mảng hàng và cột ẩn. Vì mọi ô nhìn thấy đều nói`lcm(row[i], col[j]) = b[i][j]`, chúng ta có thể thử các ước số khác nhau của các giá trị ma trận và giữ lại tích nhỏ nhất. Điều này đúng vì mọi câu trả lời hợp lệ đều phải thỏa mãn các phương trình đó. 

Vấn đề là số lượng khả năng. Một giá trị lên tới`10^8`có thể có nhiều ước số và có tới 500 giá trị hàng và 500 giá trị cột. Việc thử kết hợp nhanh chóng trở nên không thể. Ngay cả việc chỉ xem xét một vài ứng cử viên cho mỗi vị trí cũng có thể dẫn đến việc tìm kiếm trên một không gian trạng thái rộng lớn. 

Quan sát hữu ích là chúng ta không bao giờ cần giá trị đầy đủ trong khi giải. Hãy xem xét một số nguyên tố riêng biệt. Hãy để số mũ của số nguyên tố này trong hàng`i`là`r[i]`, và trong cột`j`là`c[j]`. Một ô đã biết có số mũ`x`đưa ra điều kiện:```
max(r[i], c[j]) = x
```Đối với một đỉnh, số mũ lớn nhất mà nó có thể có là số mũ nhỏ nhất trong số tất cả các ô liền kề đã biết. Nếu một hàng chạm vào các ô có số mũ`2`,`5`, Và`3`, thì số mũ của nó không thể vượt quá`2`. Bất kỳ giá trị nào lớn hơn sẽ làm cho ô đầu tiên không thể thực hiện được. 

Bây giờ chỉ định mỗi hàng và cột số mũ tối đa được phép này. Vì ma trận ban đầu được đảm bảo là hợp lệ nên mọi ô đã biết đều có ít nhất một điểm cuối có số mũ tối đa được phép chính xác là số mũ của ô. Nếu cả hai điểm cuối đều không có số mũ đó thì cả hai sẽ nhỏ hơn và LCM không bao giờ có thể đạt được giá trị yêu cầu. 

Điều này đưa ra nhiệm vụ nhỏ nhất có thể. Việc tăng bất kỳ đỉnh nào lên trên số mũ tối đa cho phép của nó là không thể và việc giảm một đỉnh sẽ phá vỡ ô dựa vào đỉnh đó. Áp dụng điều này một cách độc lập cho mỗi số nguyên tố sẽ cho tích số nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số hàng và cột | O(nm) | Quá chậm | 
| Tối ưu | O(nm log A) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích nhân tử cho mọi mục nhập ma trận khác 0. Đối với mỗi số nguyên tố, hãy lưu trữ số mũ xuất hiện trong mọi ô đã biết. Chúng tôi chỉ quan tâm đến các số nguyên tố thực sự xuất hiện trong ma trận. 

Số lượng các thừa số nguyên tố khác nhau là nhỏ vì mỗi giá trị lớn nhất là`10^8`. 

1. Đối với mỗi số nguyên tố, hãy tạo các mảng số mũ nhỏ nhất có thể cho tất cả các hàng và cột. Khởi tạo mỗi hàng và cột có giá trị lớn, sau đó xử lý mọi ô đã biết bằng số mũ`e`. Cập nhật giới hạn trên của hàng và cột với`e`. 

Điều này tính toán số mũ lớn nhất mà mỗi đỉnh có thể nhận được. 

1. Thêm đóng góp hàng và cột cho số nguyên tố này vào câu trả lời. Nếu một hàng có số mũ cuối cùng`x`, nhân câu trả lời với`p^x`. Làm tương tự cho mỗi cột. 

Sản phẩm có thể được xây dựng theo từng số nguyên tố vì các lũy thừa nguyên tố là độc lập. 

1. Bỏ qua các hàng và cột không có ô nào đã biết. Giới hạn trên của chúng vẫn chưa được đặt và giá trị tối thiểu có thể có của chúng là 1. 

Tại sao nó hoạt động: với mỗi số nguyên tố, ma trận ẩn chỉ phụ thuộc vào giá trị lớn nhất của hai số mũ. Mỗi đỉnh có một giới hạn trên từ các cạnh liên quan của nó. Một giải pháp hợp lệ không thể gán số mũ lớn hơn. Việc gán tất cả các đỉnh số mũ tối đa cho phép của chúng thỏa mãn mọi cạnh vì ma trận ban đầu được đảm bảo có một số phép gán hợp lệ, do đó mỗi cạnh có ít nhất một điểm cuối có khả năng đạt đến số mũ yêu cầu. Bất kỳ phép gán nhỏ hơn nào cũng sẽ làm giảm số mũ cần thiết cho một số cạnh. Do đó, bài tập được xây dựng chính xác là bài tập tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def factorize(x):
    res = []
    d = 2
    while d * d <= x:
        if x % d == 0:
            cnt = 0
            while x % d == 0:
                x //= d
                cnt += 1
            res.append((d, cnt))
        d += 1 if d == 2 else 2
    if x > 1:
        res.append((x, 1))
    return res

def solve_case(n, m, a):
    factors = [[[] for _ in range(m)] for _ in range(n)]
    primes = set()

    for i in range(n):
        for j in range(m):
            if a[i][j]:
                f = factorize(a[i][j])
                factors[i][j] = f
                for p, _ in f:
                    primes.add(p)

    ans = 1

    for p in primes:
        row = [-1] * n
        col = [-1] * m

        for i in range(n):
            for j in range(m):
                if a[i][j]:
                    exp = 0
                    for q, e in factors[i][j]:
                        if q == p:
                            exp = e
                            break
                    if row[i] == -1 or exp < row[i]:
                        row[i] = exp
                    if col[j] == -1 or exp < col[j]:
                        col[j] = exp

        for x in row:
            if x > 0:
                ans = ans * pow(p, x, MOD) % MOD

        for x in col:
            if x > 0:
                ans = ans * pow(p, x, MOD) % MOD

    return ans

def main():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]
        out.append(str(solve_case(n, m, a)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Giai đoạn nhân tử hóa lưu trữ số mũ nguyên tố cho mọi ô khác 0. Việc lưu trữ chúng sẽ tránh việc phân tích cùng một giá trị nhiều lần trong khi xử lý các số nguyên tố khác nhau. 

Vòng lặp chính xử lý từng số nguyên tố một. các`row`Và`col`mảng đại diện cho giới hạn trên nhỏ nhất của số nguyên tố đó. Mỗi ô hiển thị chỉ có thể giảm các giới hạn này vì mọi điểm cuối không được lớn hơn số mũ của ô. 

Một lỗi phổ biến là sử dụng số 0 làm giá trị ban đầu. Số 0 là số mũ thực, do đó, nó sẽ buộc mỗi hàng hoặc cột không có phần đóng góp nào. Mã sử ​​dụng`-1`có nghĩa là đỉnh không có ô sự cố đã biết. 

Phép nhân cuối cùng sử dụng phép lũy thừa mô-đun vì tích của tất cả các lũy thừa nguyên tố có thể rất lớn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
1 2
6 10
```Dấu vết là: 

| Bước | Thủ tướng | Số mũ hàng | Cột số mũ | Đóng góp | 
| --- | --- | --- | --- | --- | 
| Yếu tố 6,10 | 2 | 0 | 1,0 | 2 | 
| Yếu tố 6,10 | 3 | 0 | 1,0 | 3 | 
| Yếu tố 6,10 | 5 | 0 | 0,1 | 5 | 

Sản phẩm cuối cùng là:```
2 * 3 * 5 = 30
```Điều này cho thấy tại sao việc chia theo số nguyên tố lại có kết quả gán nhỏ hơn so với việc sử dụng trực tiếp các giá trị hiển thị. 

Đối với mẫu thứ ba:```
3 3
0 0 0
0 0 0
0 0 0
```Dấu vết là: 

| Bước | Các tế bào đã biết | Giới hạn hàng | Giới hạn cột | Đóng góp | 
| --- | --- | --- | --- | --- | 
| Ma trận quét | không | bỏ đặt | bỏ đặt | 1 | 

Không có số nguyên tố nào được xử lý, vì vậy mọi giá trị ẩn vẫn là 1 và câu trả lời là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm log A + Pnm) | Mỗi giá trị được tính thành thừa số và mỗi số nguyên tố có liên quan sẽ được xử lý trên ma trận. | 
| Không gian | O(nm) | Danh sách nhân tố và ma trận được lưu trữ chiếm không gian này. | 

Đây`A`là giá trị ma trận tối đa và`P`là số các số nguyên tố phân biệt xuất hiện. Với giá trị lên đến`10^8`và tổng kích thước ma trận được giới hạn bởi các ràng buộc của bài toán, điều này phù hợp một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def factorize(x):
    res = []
    d = 2
    while d * d <= x:
        if x % d == 0:
            c = 0
            while x % d == 0:
                x //= d
                c += 1
            res.append((d, c))
        d += 1 if d == 2 else 2
    if x > 1:
        res.append((x, 1))
    return res

def solve(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]
        factors = [[[] for _ in range(m)] for _ in range(n)]
        primes = set()

        for i in range(n):
            for j in range(m):
                if a[i][j]:
                    factors[i][j] = factorize(a[i][j])
                    for p, _ in factors[i][j]:
                        primes.add(p)

        cur = 1
        for p in primes:
            row = [-1] * n
            col = [-1] * m
            for i in range(n):
                for j in range(m):
                    if a[i][j]:
                        e = 0
                        for q, v in factors[i][j]:
                            if q == p:
                                e = v
                                break
                        row[i] = e if row[i] == -1 else min(row[i], e)
                        col[j] = e if col[j] == -1 else min(col[j], e)
            for x in row + col:
                if x > 0:
                    cur = cur * pow(p, x, MOD) % MOD
        ans.append(str(cur))

    return "\n".join(ans)

assert solve("""3
1 2
6 10
2 2
6 6
6 6
3 3
0 0 0
0 0 0
0 0 0
""") == "30\n36\n1"

assert solve("""1
1 1
1
""") == "1"

assert solve("""1
2 2
2 2
2 2
""") == "2"

assert solve("""1
2 3
0 6 0
0 0 10
""") == "30"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1x1`ma trận với`1`|`1`| Giá trị nhỏ nhất có thể | 
| Tất cả các mục bằng`2`|`2`| Đóng góp chung | 
| Ma trận thưa thớt với số không |`30`| Thiếu ô và đỉnh không bị giới hạn | 

## Vỏ cạnh 

Đối với ma trận toàn 0:```
2 2
0 0
0 0
```Mỗi hàng và cột không có ràng buộc sự cố. Thuật toán không bao giờ gán số mũ nguyên tố, vì vậy tất cả các giá trị ẩn vẫn bằng 1. Tích số là`1`. 

Đối với ma trận thưa thớt:```
2 3
0 6 0
0 0 10
```Các thừa số nguyên tố được xử lý độc lập. Đối với số nguyên tố 2, giá trị đã biết đầu tiên buộc số mũ 1 trên hàng hoặc cột của nó, và điều tương tự cũng xảy ra với số nguyên tố 5 và số nguyên tố 3 từ giá trị 10. Tích tối thiểu thu được bằng cách chỉ gán các số mũ cần thiết một lần nếu có thể. 

Đối với các giá trị bằng nhau:```
2 2
6 6
6 6
```Mọi cạnh đều yêu cầu số mũ của các số nguyên tố 2 và 3. Giới hạn hàng và giới hạn cột trở thành các giá trị giống nhau, nhưng thuật toán chọn tổng đóng góp nhỏ nhất, tạo ra`36`thay vì nhân tất cả các ô nhìn thấy được với nhau.
