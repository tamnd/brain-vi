---
title: "CF 102282B - \u0415\u0449\u0451 \u043e\u0434\u043d\u0430"
description: "Chúng ta cần đếm các số nguyên dương từ 1 đến r nguyên tố cùng nhau với n. Hai số nguyên tố cùng nhau khi ước số chung lớn nhất của chúng chính xác là 1. Dữ liệu đầu vào chứa hai số nguyên n và r, mỗi số nhiều nhất là 10^9."
date: "2026-08-13T16:11:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102282
codeforces_index: "B"
codeforces_contest_name: "2011, \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u043a\u043e\u043d\u0442\u0435\u0441\u0442 \u0421\u0413\u0410\u0423 \u043d\u0430 \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b ACM ICPC"
rating: 0
weight: 102282
solve_time_s: 76
verified: true
draft: false
---

[CF 102282B - \u0415\u0449\u0451 \u043e\u0434\u043d\u0430](https://codeforces.com/problemset/problem/102282/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm các số nguyên dương từ`1`bởi vì`r`đó là nguyên tố cùng nhau với`n`. Hai số nguyên tố cùng nhau khi ước chung lớn nhất của chúng bằng chính xác`1`. 

Đầu vào chứa hai số nguyên,`n`Và`r`, mỗi cái nhiều nhất`10^9`. Giá trị của`r`đủ lớn để việc kiểm tra từng số riêng lẻ là không khả thi. Ngay cả khi một phép tính ước chung lớn nhất rất rẻ, việc lặp lại tất cả`10^9`ứng viên sẽ yêu cầu lên tới`10^9`tính toán gcd. Việc nhân tố hóa`n`hứa hẹn hơn nhiều bởi vì`n <= 10^9`, nên phép chia thử chỉ cần xét các ước số đến`sqrt(n) <= 31623`. Một khi các thừa số nguyên tố riêng biệt của`n`đã biết thì có thể có nhiều nhất là chín số đó, vì tích của mười số nguyên tố đầu tiên đã vượt quá`10^9`. 

Trường hợp cạnh chính là`n = 1`. Mọi số nguyên dương đều nguyên tố cùng nhau`1`, vì vậy đối với`1 5`câu trả lời là`5`. Một công thức giả định một cách mù quáng rằng`n`có ít nhất một thừa số nguyên tố có thể vô tình trả về 0 hoặc giữ nguyên câu trả lời ban đầu không chính xác. 

ranh giới`r`phải bao hàm. Vì`n = 2, r = 2`, các số đang xét là`1`Và`2`, và chỉ`1`là nguyên tố cùng nhau với`2`, vậy câu trả lời là`1`. Một vòng lặp`range(1, r)`sẽ vô tình loại trừ`r`và tình cờ đưa ra kết quả sai cho nhiều đầu vào. 

Các thừa số nguyên tố lặp đi lặp lại cũng phải được xử lý chính xác. Vì`n = 12, r = 12`, các thừa số nguyên tố liên quan chỉ là`2`Và`3`, không`2, 2, 3`. Câu trả lời là`4`, tương ứng với`1, 5, 7, 11`. Việc coi các yếu tố lặp lại như các lựa chọn loại trừ bao gồm riêng biệt sẽ tính cùng một điều kiện bị cấm nhiều lần. 

Trường hợp ranh giới cuối cùng là khi`n`bản thân nó lớn hơn nhiều so với`r`. Vì`n = 7, r = 3`, cả ba số`1, 2, 3`là nguyên tố cùng nhau với`7`, vậy câu trả lời là`3`. Không có lý do gì để hạn chế ứng viên theo quy mô của`n`; chỉ có ước chung của chúng là quan trọng. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là kiểm tra mọi số nguyên`x`từ`1`bởi vì`r`và kiểm tra xem`gcd(x, n) = 1`. Điều này đúng vì định nghĩa của tập hợp bắt buộc chính xác là điều kiện gcd đó. Tuy nhiên, với`r = 10^9`, thuật toán có thể thực hiện`10^9`tính toán gcd. Mặc dù bản thân thuật toán của Euclid rất nhanh nhưng một tỷ lần lặp lại vượt xa ngân sách thời gian dự định. 

Quan sát hữu ích là liệu`x`là nguyên tố cùng nhau với`n`chỉ phụ thuộc vào các thừa số nguyên tố riêng biệt của`n`. Giả sử các số nguyên tố đó là`p1, p2, ..., pk`. Một số nguyên không thể nguyên tố cùng nhau`n`chính xác khi nó chia hết cho ít nhất một trong các số nguyên tố này. 

Chúng ta có thể đếm các số nguyên chia hết cho ít nhất một số nguyên tố bị cấm bằng cách sử dụng loại trừ bao gồm. Với mọi tập con khác rỗng của các thừa số nguyên tố phân biệt, hãy`d`là tích của các số nguyên tố trong tập con đó. Các số chia hết cho mọi số nguyên tố trong tập hợp con chính xác là bội số của`d`, vậy có`r // d`trong số họ`1`bởi vì`r`. 

Đối với các tập hợp con chứa số nguyên tố lẻ, các bội số này sẽ được thêm vào. Đối với các tập hợp con chứa số nguyên tố chẵn, chúng sẽ bị trừ. Nếu như`bad`là số kết quả, câu trả lời mong muốn là`r - bad`. 

Nhiệm vụ duy nhất còn lại là tìm các thừa số nguyên tố riêng biệt của`n`. Phép chia thử nghiệm đến căn bậc hai là đủ cho`n <= 10^9`. Sau khi trích xuất mọi lần xuất hiện của một thừa số nguyên tố, chúng ta chỉ giữ lại số nguyên tố đó một lần, vì phép loại trừ bao hàm cần các điều kiện chia hết riêng biệt. 

Ngoài ra còn có cách giải thích tương đương hữu ích thông qua hàm tổng Euler. Nếu giới hạn trên chính xác là`n`, câu trả lời sẽ là`phi(n)`. Ở đây khoảng kết thúc tùy ý`r`, do đó công thức dạng đóng thông thường cho`phi(n)`là không đủ. Loại trừ bao gồm áp dụng trực tiếp cho một tùy ý`r`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(r log n)`|`O(1)`| Quá chậm | 
| Tối ưu |`O(sqrt(n) + 2^k)`|`O(k)`| Đã chấp nhận | 

Đây`k`là số các thừa số nguyên tố phân biệt của`n`. Từ`n <= 10^9`,`k <= 9`, do đó bảng liệt kê tập hợp con có nhiều nhất`512`trường hợp. 

## Hướng dẫn thuật toán 

1. Phân tích nhân tử`n`bằng cách thử các ước số từ`2`bởi vì`sqrt(n)`. Bất cứ khi nào một số chia`p`chia giá trị hiện tại của`n`, ghi`p`một lần và nhiều lần chia nó ra. Việc loại bỏ tất cả các bản sao là cần thiết vì chỉ có sự hiện diện của các bản sao chính mới đảm bảo tính đồng nguyên. 
2. Sau khi chia thử, nếu giá trị còn lại lớn hơn`1`, ghi lại nó dưới dạng một thừa số nguyên tố khác. Giá trị còn lại lớn hơn`1`không thể có thừa số chưa được xử lý nhỏ hơn nên bản thân nó phải là số nguyên tố. 
3. Liệt kê mọi tập con của các thừa số nguyên tố phân biệt. Đối với một tập hợp con, nhân các số nguyên tố đã chọn của nó để thu được`d`. Các số nguyên chia hết cho mọi số nguyên tố được chọn chính xác là bội số của`d`, cho`r // d`những số nguyên như vậy. 
4. Thêm`r // d`khi tập hợp con chứa số nguyên tố lẻ và trừ nó khi tập hợp con chứa số nguyên tố chẵn. Đây là hiệu chỉnh loại trừ bao gồm đối với sự chồng chéo giữa các điều kiện chia hết. 
5. Trừ số số nguyên không nguyên tố cùng nhau từ`r`. Số còn lại chính xác là số số nguyên trong`[1, r]`gcd của ai với`n`bằng`1`. 

### Tại sao nó hoạt động 

hãy để`P`là tập hợp các thừa số nguyên tố phân biệt của`n`. một số nguyên`x`không cùng nguyên tố với`n`chính xác khi có ít nhất một số nguyên tố trong`P`chia rẽ`x`. Do đó, các số nguyên không mong muốn tạo thành hợp của các tập hợp`A_p = {x : p divides x}`vì`p`TRONG`P`. 

Với mọi tập con của các số nguyên tố này có tích`d`, giao điểm của nó chứa chính xác bội số của`d`, và có`r // d`các số nguyên như vậy lên đến`r`. Loại trừ bao gồm đếm chính xác sự kết hợp bằng cách cộng và trừ xen kẽ theo kích thước tập hợp con. Do đó,`bad`chính xác là số số nguyên chia sẻ một ước số không tầm thường với`n`, Và`r - bad`đếm chính xác các số nguyên nguyên tố cùng nhau. 

Giai đoạn phân tích nhân tử cung cấp mọi số nguyên tố trong`P`chính xác một lần, do đó giai đoạn loại trừ bao gồm đại diện cho mọi ước số nguyên tố chung có thể có và không có điều kiện không liên quan. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, r = map(int, input().split())

    x = n
    primes = []

    p = 2
    while p * p <= x:
        if x % p == 0:
            primes.append(p)
            while x % p == 0:
                x //= p
        p += 1 if p == 2 else 2

    if x > 1:
        primes.append(x)

    k = len(primes)
    bad = 0

    for mask in range(1, 1 << k):
        product = 1
        bits = 0

        for i in range(k):
            if mask & (1 << i):
                product *= primes[i]
                bits += 1

        count = r // product

        if bits % 2 == 1:
            bad += count
        else:
            bad -= count

    print(r - bad)

if __name__ == "__main__":
    solve()
```Mã bản sao đầu tiên`n`vào trong`x`, vì phép chia thử liên tục làm giảm số được phân tích thành nhân tử. Danh sách`primes`chứa mỗi thừa số nguyên tố riêng biệt đúng một lần. 

Vòng lặp bắt đầu với`p = 2`, sau đó chỉ kiểm tra các giá trị lẻ sau`2`. Điều này tránh việc kiểm tra ngay cả các ước số không thể là số nguyên tố. điều kiện`p * p <= x`là an toàn vì nếu`x`có hệ số tổng hợp, một trong các thừa số của nó nhiều nhất là căn bậc hai của nó. Khi vòng lặp kết thúc, phần còn lại`x > 1`phải là số nguyên tố. 

Vòng lặp tập hợp con sử dụng bitmask. Chút`i`xác định liệu`primes[i]`thuộc về tập con hiện tại.`product`trở thành tích của các số nguyên tố đã chọn, trong khi`bits`ghi lại kích thước tập hợp con và xác định dấu bao gồm-loại trừ. 

Việc tính toán sử dụng phép chia số nguyên`r // product`, không`(r - 1) // product`, bởi vì`r`chính nó thuộc về khoảng đó. Số nguyên Python cũng loại bỏ mọi lo ngại về tràn, mặc dù tất cả các sản phẩm trung gian ở đây cũng đủ nhỏ đối với số nguyên 64 bit thông thường. 

Vì`n = 1`, danh sách thừa số vẫn trống. Vòng lặp tập hợp con không có lần lặp lại, vì vậy`bad = 0`và câu trả lời trở thành`r`, điều đó hoàn toàn chính xác. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`n = 2`có một thừa số nguyên tố riêng biệt,`2`. 

| Mặt nạ | Các số nguyên tố được chọn | Sản phẩm | Kích thước tập hợp con |`r // product`|`bad`| 
| --- | --- | --- | --- | --- | --- | 
|`1`|`{2}`|`2`|`1`|`1`|`1`| 

Số xấu duy nhất lên tới`2`là`2`chính nó. Trừ nó khỏi hai ứng cử viên sẽ cho`2 - 1 = 1`, vì vậy đầu ra là`1`. 

Đối với mẫu thứ hai,`n = 10`có thừa số nguyên tố phân biệt`2`Và`5`. 

| Mặt nạ | Các số nguyên tố được chọn | Sản phẩm | Kích thước tập hợp con |`r // product`|`bad`| 
| --- | --- | --- | --- | --- | --- | 
|`01`|`{2}`|`2`|`1`|`4`|`4`| 
|`10`|`{5}`|`5`|`1`|`1`|`5`| 
|`11`|`{2, 5}`|`10`|`2`|`0`|`5`| 

Giữa`1`bởi vì`9`, bốn số chia hết cho`2`, một có thể chia hết cho`5`và giao điểm của chúng không chứa số. Vậy năm số không nguyên tố cùng nhau`10`, rời đi`9 - 5 = 4`. Các số nguyên tố cùng nhau là`1, 3, 7, 9`, điều này cũng chứng tỏ tại sao số nguyên tố không liên quan đến thuộc tính bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(sqrt(n) + 2^k k)`| Phân chia thử nghiệm lấy`O(sqrt(n))`, và mỗi`2^k`tập hợp con kiểm tra nhiều nhất`k`số nguyên tố | 
| Không gian |`O(k)`| Chỉ các thừa số nguyên tố riêng biệt và số lượng biến không đổi được lưu trữ | 

Vì`n <= 10^9`,`sqrt(n) <= 31623`, do đó việc nhân tử hóa chỉ cần một số lượng nhỏ phép chia thử. Cũng,`k <= 9`, làm cho việc liệt kê tập hợp con trở nên nhỏ bé. Do đó, thuật toán tránh mọi sự phụ thuộc vào giá trị có kích thước hàng tỷ của`r`và thoải mái phù hợp với giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        n, r = map(int, sys.stdin.readline().split())

        x = n
        primes = []

        p = 2
        while p * p <= x:
            if x % p == 0:
                primes.append(p)
                while x % p == 0:
                    x //= p
            p += 1 if p == 2 else 2

        if x > 1:
            primes.append(x)

        k = len(primes)
        bad = 0

        for mask in range(1, 1 << k):
            product = 1
            bits = 0

            for i in range(k):
                if mask & (1 << i):
                    product *= primes[i]
                    bits += 1

            count = r // product

            if bits & 1:
                bad += count
            else:
                bad -= count

        print(r - bad)

        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert solve_data("2 2\n") == "1", "sample 1"
assert solve_data("10 9\n") == "4", "sample 2"

# Minimum-size input: the only positive integer is coprime with itself when it is 1.
assert solve_data("1 1\n") == "1", "minimum input"

# n = 1 means every number up to r is coprime with n.
assert solve_data("1 1000000000\n") == "1000000000", "n = 1"

# Equal values with repeated prime factors: gcd(1, 10)=1, gcd(3,10)=1,
# gcd(7,10)=1, gcd(9,10)=1.
assert solve_data("10 10\n") == "4", "equal values"

# Boundary where r itself is divisible by n's prime factor.
assert solve_data("2 2\n") == "1", "inclusive upper boundary"

# Large composite n with repeated factors: 1e9 = 2^9 * 5^9.
# Count = 1e9 * (1 - 1/2) * (1 - 1/5) = 400000000.
assert solve_data("1000000000 1000000000\n") == "400000000", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đầu vào tối thiểu và tập hợp thừa số nguyên tố trống | 
|`1 1000000000`|`1000000000`| Mỗi ứng cử viên đều đồng nguyên với`1`| 
|`10 10`|`4`| Bình đẳng`n`Và`r`, với nhiều thừa số nguyên tố riêng biệt | 
|`2 2`|`1`| Bao gồm ranh giới trên | 
|`1000000000 1000000000`|`400000000`| Giá trị kích thước tối đa và các thừa số nguyên tố lặp lại | 

## Vỏ cạnh 

cho`n = 1, r = 5`, việc phân tích nhân tử sẽ tạo ra một danh sách trống vì`1`không có thừa số nguyên tố. Không có tập hợp con bao gồm-loại trừ, vì vậy`bad`vẫn bằng không. Tính toán cuối cùng là`5 - 0 = 5`, đếm chính xác mọi số từ`1`ĐẾN`5`. 

Vì`n = 2, r = 2`, danh sách thừa số là`[2]`. Tập hợp con duy nhất đại diện cho các số chia hết cho`2`, Và`2 // 2 = 1`. Do đó, một ứng viên bị từ chối và câu trả lời là`2 - 1 = 1`. Điều này xác nhận rằng điểm cuối trên`r`được bao gồm. 

Vì`n = 12, r = 12`, nhân tử hóa tạo ra`[2, 3]`, không`[2, 2, 3]`. Số lượng loại trừ bao gồm`6`bội số của`2`,`4`bội số của`3`, và trừ`2`bội số của`6`. Kể từ đây`bad = 6 + 4 - 2 = 8`, cho`12 - 8 = 4`. Bốn giá trị hợp lệ là`1, 5, 7, 11`. 

Vì`n = 7, r = 3`, nhân tử hóa tạo ra`[7]`, Nhưng`3 // 7 = 0`. Không có số nào lên tới`3`chia hết cho`7`, Vì thế`bad = 0`và câu trả lời là`3`. Điều này cho thấy tại sao phương pháp này hoạt động ngay cả khi mọi ứng cử viên đều nhỏ hơn`n`. 

Đối với trường hợp kích thước tối đa`n = r = 10^9`, các yếu tố riêng biệt là`2`Và`5`. Số xấu là`500000000 + 200000000 - 100000000 = 600000000`, rời đi`400000000`những con số hợp lệ. Thuật toán đạt được kết quả này mà không cần lặp lại bất kỳ`10^9`số nguyên ứng viên.
