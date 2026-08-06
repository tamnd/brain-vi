---
title: "CF 102535Q - Cấp độ duy nhất QUÁ"
description: "Đầu vào chứa hai tập hợp số nguyên dương. Bộ sưu tập đầu tiên chứa các giá trị có thể có của k và bộ sưu tập thứ hai chứa các cơ sở có thể có b. Đối với mỗi cặp có thể được hình thành bằng cách chọn một giá trị từ mỗi bộ sưu tập, chúng ta cần quyết định xem cặp đó có tuyệt vời hay không."
date: "2026-08-05T15:46:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "Q"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 136
verified: true
draft: false
---

[CF 102535Q - Cấp độ duy nhất QUÁ](https://codeforces.com/problemset/problem/102535/Q) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào chứa hai tập hợp số nguyên dương. Bộ sưu tập đầu tiên chứa các giá trị có thể có của`k`và cái thứ hai chứa các căn cứ có thể`b`. Đối với mỗi cặp có thể được hình thành bằng cách chọn một giá trị từ mỗi bộ sưu tập, chúng ta cần quyết định xem cặp đó có tuyệt vời hay không. Câu trả lời là số cặp thỏa mãn điều kiện. 

Điều kiện này có vẻ như liên quan đến việc tính toán lặp đi lặp lại các tổng chữ số, nhưng điều quan trọng là gốc số trong cơ số`b`chỉ được xác định bởi modulo dư`b - 1`. Đối với số nguyên dương`x`, cơ sở của nó`b`gốc kỹ thuật số là giá trị duy nhất từ ​​​​`1`ĐẾN`b - 1`có cùng số dư với`x`modulo`b - 1`, có số dư`0`đại diện bởi`b - 1`. 

Các giới hạn đủ lớn nên việc kiểm tra từng cặp là không thể. Có thể có tới`300000`giá trị trong mỗi bộ, từ bỏ`9 * 10^10`các cặp có thể. Cách tiếp cận bậc hai đối với kích thước đầu vào không thể phù hợp trong hai giây. Vì mọi giá trị lớn nhất là`10^6`, giải pháp dự định phải khai thác cấu trúc số của các giá trị thay vì lặp qua tất cả các cặp. 

Một lỗi phổ biến là mô phỏng các tổng số cho mỗi phép nhân. Điều này vừa không cần thiết vừa nguy hiểm. Ví dụ, với`k = 6`Và`b = 8`, trình tự được dựa trên các giá trị`0, 6, 12, 18, ..., 42`, nhưng thông tin liên quan duy nhất là cách các giá trị này hoạt động theo modulo`7`. 

Một trường hợp cạnh khác là khi`b = 2`. Đây`b - 1 = 1`và mọi số nguyên đều nguyên tố cùng nhau với`1`. Trình tự chỉ chứa`f_2(0)`Và`f_2(k)`, vì vậy mỗi`k`tạo thành một cặp hợp lệ. Một triển khai cố gắng chia cho`b - 1`mà không xem xét trường hợp này có thể thất bại. 

Trường hợp cạnh cuối cùng là khi`k`Và`b - 1`chia sẻ một yếu tố. Ví dụ:```
Input:
1 1
6
4
```Đây`b - 1 = 3`. Các giá trị được tính bằng cách nhân với`6`modulo`3`, do đó mọi vị trí khác 0 đều thu gọn về cùng một nghiệm số. Đầu ra đúng là`0`. Một giải pháp chỉ kiểm tra một vài giá trị được tạo ra có thể cho rằng mẫu đó là một hoán vị không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ thử từng cặp`(k, b)`. Với mỗi cặp, nó sẽ tính các giá trị`f_b(0), f_b(k), ..., f_b((b - 1)k)`và kiểm tra xem tất cả các chữ số từ`0`ĐẾN`b - 1`xuất hiện đúng một lần. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, đầu vào lớn nhất có thể chứa`300000`giá trị ở cả hai bên, tạo ra tối đa`90000000000`cặp. Ngay cả khi bỏ qua chi phí kiểm tra từng chuỗi, số lượng cặp đã vượt xa mức có thể. 

Quan sát hữu ích đến từ việc thay thế các nghiệm số bằng số học mô-đun. Cho phép`m = b - 1`. Trình tự chứa`f_b(ik)`vì`0 <= i <= m`. Vì`1 <= i <= m - 1`, các giá trị chính xác là các phần dư khác 0 của`ik mod m`, và giá trị cuối cùng`f_b(mk)`là`m`vì số dư bằng 0. 

Nhân với`k`modulo`m`tạo ra mọi dư lượng đúng một lần khi và chỉ khi`k`có modulo nghịch đảo nhân`m`. Điều đó xảy ra chính xác khi`gcd(k, m) = 1`. Vấn đề ban đầu được giảm xuống thành các cặp đếm trong đó:`gcd(k, b - 1) = 1`. 

Nhiệm vụ còn lại là trả lời nhiều truy vấn về tính đồng nguyên. Đối với một giá trị cố định`x = b - 1`, gọi các ước nguyên tố phân biệt của nó là`p1, p2, ...`. Một giá trị`k`không hợp lệ nếu nó chia hết cho ít nhất một trong các số nguyên tố này. Loại trừ bao gồm cho số lượng hợp lệ`k`giá trị:`|K| - count(multiples of p1) - count(multiples of p2) + count(multiples of p1*p2) ...`Số lượng các thừa số nguyên tố riêng biệt của bất kỳ số nào dưới một triệu là nhỏ, do đó việc liệt kê tập hợp con này diễn ra nhanh chóng. Chúng tôi tính toán trước có bao nhiêu giá trị trong`K`được chia hết cho mọi ước số có thể bằng cách sử dụng mảng tần số của`K`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | K | * | 
| Tối ưu | O(MAX + | B | * 2^ω) | 

đây`ω`là số các thừa số nguyên tố phân biệt của`b - 1`. Đối với những con số dưới một triệu,`ω`nhiều nhất là bảy. 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tần số cho tất cả các giá trị trong`K`. Sau đó tính toán với mọi số nguyên`d`, có bao nhiêu giá trị trong`K`được chia cho`d`. Điều này cho phép các truy vấn loại trừ bao gồm yêu cầu số lượng bội số ngay lập tức thay vì quét tất cả`K`. 
2. Tính trước hệ số nguyên tố nhỏ nhất cho mọi số lên đến`10^6`. Điều này cho phép mọi giá trị`b - 1`được nhân tố hóa một cách nhanh chóng. 
3. Đối với mỗi căn cứ`b`TRONG`B`, bộ`x = b - 1`và nhân tử hóa`x`thành các ước nguyên tố riêng biệt của nó. Chỉ các số nguyên tố khác nhau mới quan trọng vì chúng ta chỉ quan tâm liệu một số có chia sẻ ít nhất một thừa số nguyên tố với`x`. 
4. Sử dụng phép loại trừ bao hàm đối với các ước nguyên tố này. Với mỗi tập con số nguyên tố, hãy tính tích của nó. Nếu kích thước tập hợp con là số lẻ, hãy trừ số giá trị trong`K`chia hết cho sản phẩm đó. Nếu kích thước tập hợp con là chẵn, hãy thêm nó trở lại. 
5. Cộng số kết quả vào câu trả lời. Mỗi giá trị được đếm tương ứng với chính xác một cặp mát mẻ với cơ sở hiện tại. 

Tại sao nó hoạt động: 

Đối với một cơ sở`b`, dãy căn số là một hoán vị chính xác khi nhân với`k`hoán vị mọi dư lượng modulo`b - 1`. Phép nhân là một hoán vị của phần dư mô đun chính xác khi số nhân của nó nguyên tố cùng nhau với mô đun. Do đó cặp này nguội chính xác khi`gcd(k, b - 1) = 1`. 

Bước loại trừ bao gồm đếm chính xác các giá trị của`k`không chứa bất kỳ thừa số nguyên tố nào của`b - 1`. Đó chính xác là những giá trị có gcd bằng một, vì vậy mọi cơ sở đều đóng góp số lượng cặp mát chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    K = list(map(int, input().split()))
    B = list(map(int, input().split()))

    MAXV = 10**6

    freq = [0] * (MAXV + 1)
    for x in K:
        freq[x] += 1

    divisible = [0] * (MAXV + 1)
    for d in range(1, MAXV + 1):
        s = 0
        for j in range(d, MAXV + 1, d):
            s += freq[j]
        divisible[d] = s

    spf = list(range(MAXV + 1))
    for i in range(2, int(MAXV ** 0.5) + 1):
        if spf[i] == i:
            for j in range(i * i, MAXV + 1, i):
                if spf[j] == j:
                    spf[j] = i

    def factorize(x):
        res = []
        while x > 1:
            p = spf[x]
            res.append(p)
            while x % p == 0:
                x //= p
        return res

    ans = 0

    for b in B:
        x = b - 1
        primes = factorize(x)

        bad = 0
        cnt = len(primes)

        def dfs(pos, prod, bits):
            nonlocal bad
            if pos == cnt:
                if bits:
                    if bits & 1:
                        bad += divisible[prod]
                    else:
                        bad -= divisible[prod]
                return
            dfs(pos + 1, prod, bits)
            dfs(pos + 1, prod * primes[pos], bits + 1)

        dfs(0, 1, 0)
        ans += len(K) - bad

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng tần số lưu trữ tập gốc`K`ở dạng hỗ trợ tính số chia. Vòng lặp lồng nhau tính toán`divisible[d]`, đó là số phần tử của`K`chia hết cho`d`. 

Mảng thừa số nguyên tố nhỏ nhất là tối ưu hóa sàng tiêu chuẩn. Thay vì thử mọi ước số có thể trong khi phân tích nhân tử`b - 1`, nó liên tục loại bỏ thừa số nguyên tố nhỏ nhất. Chỉ các yếu tố riêng biệt được lưu trữ vì lũy thừa lặp lại không ảnh hưởng đến việc hai số có phải là số nguyên tố cùng nhau hay không. 

Hàm đệ quy liệt kê các tập hợp con của các thừa số nguyên tố riêng biệt. các`bits`giá trị ghi lại có bao nhiêu yếu tố đã được chọn. Các tập hợp con có kích thước lẻ đại diện cho các số cần được loại bỏ, trong khi các tập hợp con có kích thước chẵn sẽ được thêm lại. Tập hợp con trống sẽ bị bỏ qua. 

Số nguyên Python không tràn và độ sâu đệ quy tối đa là bảy vì một số dưới một triệu không thể có nhiều ước nguyên tố khác nhau. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
4 3
6 9 11 24
8 16 10
```Các cơ sở được xử lý như sau. 

| Cơ sở b | b - 1 | Các yếu tố chính | Giá trị k hợp lệ | Đếm thêm | 
| --- | --- | --- | --- | --- | 
| 8 | 7 | 7 | 6, 9, 11, 24 | 4 | 
| 16 | 15 | 3, 5 | 11 | 1 | 
| 10 | 9 | 3 | 11 | 1 | 

Cơ sở thứ nhất có mô đun`7`. Không có giá trị nào trong`K`được chia cho`7`, vì vậy mọi giá trị đều hoạt động. Hai cơ sở còn lại loại bỏ các yếu tố chia sẻ giá trị bằng`15`Và`9`, chỉ để lại`11`. Câu trả lời cuối cùng là`4 + 1 + 1 = 6`. 

Một ví dụ bổ sung nhỏ:```
2 2
2 3
4 5
```| Cơ sở b | b - 1 | Các yếu tố chính | Giá trị k hợp lệ | Đếm thêm | 
| --- | --- | --- | --- | --- | 
| 4 | 3 | 3 | 2 | 1 | 
| 5 | 4 | 2 | 3 | 1 | 

Cơ sở đầu tiên từ chối`3`bởi vì nó chia sẻ yếu tố`3`với`b - 1`. Cơ sở thứ hai từ chối`2`bởi vì nó chia sẻ yếu tố`2`với`b - 1`. Câu trả lời là`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAX log MAX + | B | 
| Không gian | O(TỐI ĐA) | Mảng tần số, số chia và thừa số nguyên tố nhỏ nhất sử dụng bộ nhớ tuyến tính. | 

Giá trị tối đa là một triệu, do đó quá trình tiền xử lý có thể quản lý được. Mỗi cơ sở yêu cầu tối đa vài trăm phép toán bao gồm-loại trừ vì`b - 1`có rất ít thừa số nguyên tố khác nhau. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    return result

assert run("""4 3
6 9 11 24
8 16 10
""") == "6\n"

assert run("""2 2
2 3
4 5
""") == "2\n"

assert run("""1 1
1
2
""") == "1\n"

assert run("""3 1
3 6 9
4
""") == "0\n"

assert run("""3 3
2 3 5
2 3 4
""") == "7\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu | 6 | Hành vi bao gồm-loại trừ chung | 
|`2 2 / 2 3 / 4 5`| 2 | Các mẫu thừa số nguyên tố khác nhau | 
| Phần tử đơn có cơ số 2 | 1 | các`b - 1 = 1`trường hợp | 
| Giá trị chia sẻ một yếu tố với`b - 1`| 0 | Từ chối thông qua điều kiện gcd | 
| Một số căn cứ nhỏ | 7 | Nhiều truy vấn có thừa số nguyên tố lặp lại | 

## Vỏ cạnh 

cho`b = 2`, mô đun là`1`. Việc phân tích nhân tử không trả về số nguyên tố, do đó, việc loại trừ bao gồm đánh dấu không có giá trị nào là không hợp lệ. Thuật toán cộng mọi giá trị trong`K`, phù hợp với thực tế là mọi số nguyên dương đều nguyên tố cùng nhau với`1`. 

Đối với các giá trị ở đó`k`chia sẻ một yếu tố với`b - 1`, tập con nguyên tố chứa thừa số đó sẽ loại bỏ chúng. Ví dụ, với`k = 6`Và`b = 4`, mô đun là`3`. Danh sách thừa số nguyên tố là`[3]`, và số bội số của`3`bao gồm`6`, nên nó bị loại trừ. 

Đối với đầu vào lớn, thuật toán không bao giờ so sánh mọi`k`với mọi`b`. Nó chỉ xử lý các căn cứ trong`B`, và mỗi cái sử dụng tập hợp nhỏ các thừa số nguyên tố của riêng mình. Điều này tránh được điều không thể`|K| * |B|`liệt kê cặp.
