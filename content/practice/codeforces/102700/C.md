---
title: "CF 102700C - Số lượng mật mã"
description: "Điều quan trọng cần lưu ý là hai chuỗi khóa khác nhau không nhất thiết phải biểu diễn hai mật mã Vigenère khác nhau. Nếu bản thân một khóa là sự lặp lại của một chuỗi ngắn hơn, thì việc mở rộng một trong hai khóa theo định kỳ sẽ tạo ra cùng một chuỗi dịch chuyển."
date: "2026-08-10T05:50:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "C"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 501
verified: true
draft: false
---

[CF 102700C - Số lượng mật mã](https://codeforces.com/problemset/problem/102700/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8m 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Điều quan trọng cần lưu ý là hai chuỗi khóa khác nhau không nhất thiết phải biểu diễn hai mật mã Vigenère khác nhau. Nếu bản thân một khóa là sự lặp lại của một chuỗi ngắn hơn, thì việc mở rộng một trong hai khóa theo định kỳ sẽ tạo ra cùng một chuỗi dịch chuyển. 

Ví dụ: trên bảng chữ cái nhị phân, các phím`0`,`00`,`000`, v.v. đều tạo ra chuỗi dịch chuyển vô hạn giống nhau. Tương tự như vậy,`01`Và`0101`tương đương vì cả hai đều tạo ra`010101...`. Kẻ tấn công chỉ cần thử một đại diện từ mỗi lớp tương đương như vậy. 

Mọi chuỗi không rỗng có thể được viết duy nhất dưới dạng lặp lại của chuỗi ngắn nhất. Chuỗi ngắn nhất đó được gọi là gốc nguyên thủy của nó. Một chuỗi là nguyên thủy khi bản thân nó không thể được viết dưới dạng lặp lại của một chuỗi ngắn hơn. Vì vậy, bài toán tương đương với việc đếm tất cả các chuỗi nguyên thủy có độ dài tối đa`k`. 

Đối với một bảng chữ cái có kích thước`a`, có`a^n`chuỗi có độ dài`n`. Cho phép`P(n)`là số chuỗi nguyên thủy có độ dài`n`. Mỗi chuỗi có độ dài`n`có một gốc nguyên thủy duy nhất có chiều dài chia`n`, vậy 

[ 
a^n = \sum_{d\mid n} P(d). 
] 

Đảo ngược Möbius mang lại 

[ 
P(n)=\sum_{d\mid n}\mu(d)a^{n/d}, 
] 

ở đâu`μ`là hàm Mobius. Công thức từ nguyên thủy tiêu chuẩn này được rút ra trực tiếp từ sự phân rã gốc nguyên thủy duy nhất. 

Câu trả lời bắt buộc là 

[ 
\sum_{n=1}^{k}P(n). 
] 

Kích thước bảng chữ cái nhiều nhất là`10^3`, trong khi độ dài khóa tối đa là`5 * 10^6`. Hạn chế quan trọng là sau này. Bất kỳ thuật toán nào liên quan đến tất cả các khóa có thể đều có tính hàm mũ trong`k`, và thậm chí là một`O(k log k)`việc liệt kê số chia là tốn kém không cần thiết. Sàng thời gian tuyến tính theo sau là quét tuyến tính là mục tiêu thích hợp. 

Có một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. Vì`a = 1`Và`k = 3`, khóa duy nhất có thể có ở mọi độ dài là một chuỗi các ký hiệu giống hệt nhau, vì vậy mọi khóa đều biểu thị cùng một mật mã và câu trả lời là`1`. Vì`a = 2`Và`k = 2`, bốn khóa có thể là`0`,`1`,`00`, Và`11`với độ dài lên đến hai, nhưng`00`có gốc nguyên thủy`0`Và`11`có gốc nguyên thủy`1`, vậy câu trả lời là`4`, không`4`cộng với một số lớp khóa lặp lại bổ sung. Vì`a = 2`Và`k = 3`, số nguyên thủy là`P(1)=2`,`P(2)=2`, Và`P(3)=6`, đưa ra câu trả lời`10`. Một giải pháp đơn giản là tính tổng`2^n`sẽ đếm sai các phím lặp đi lặp lại nhiều lần. 

Tuyên bố chính thức đưa ra các ràng buộc và hành vi mẫu tương tự, bao gồm cả thực tế là các khóa như`00`Và`11`không cần phải thử cả hai một cách riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ tạo ra mọi khóa có thể có ở mọi độ dài từ`1`bởi vì`k`. có 

[ 
a+a^2+\cdots+a^k 
] 

những phím như vậy. Đối với mỗi khóa, chúng ta có thể tìm khoảng thời gian ngắn nhất của nó và chỉ tính nó nếu nó là nguyên thủy. Điều này đúng vì chính xác một đại diện từ mỗi lớp tương đương mật mã là nguyên thủy. 

Vấn đề là số lượng ứng viên. Khi`a >= 2`, tổng số là`Θ(a^k)`, đã theo cấp số nhân. Ở các giá trị tối đa`a=1000`Và`k=5*10^6`, số chiều dài-`k`riêng ứng viên là`1000^{5,000,000}=10^{15,000,000}`. Nếu chúng tôi cũng kiểm tra rõ ràng lên đến`k`vị trí để kiểm tra tính tuần hoàn, công việc trở nên`Θ(k a^k)`. Bản thân việc liệt kê đã là không thể, vì vậy việc tối ưu hóa kiểm tra định kỳ không thể cứu được phương pháp này. 

Quan sát hữu ích là chúng ta không bao giờ cần xây dựng một khóa. Chúng ta chỉ cần số lượng chuỗi nguyên thủy của mỗi độ dài. Sự phân tách gốc nguyên thủy mang lại một nhận dạng ước số rõ ràng, 

[ 
a^n=\sum_{d\mid n}P(d). 
] 

Đảo ngược Möbius chuyển đổi điều này thành một công thức cho`P(n)`. Tính độc lập công thức đó cho mọi`n`vẫn sẽ yêu cầu liệt kê các ước số. Quan sát thứ hai loại bỏ chi phí đó. 

Bắt đầu với 

\sum_{n=1}^{k}\sum_{d\mid n}\mu(d)a^{n/d}. 
]

Viết`n = d x`. Sau đó`d x <= k`, vậy 

[ 
\sum_{x=1}^{k}a^x 
\sum_{d=1}^{\lfloor k/x\rfloor}\mu(d). 
] 

Xác định tiền tố tổng Möbius 

[ 
M(t)=\sum_{d=1}^{t}\mu(d). 
] 

Toàn bộ câu trả lời trở thành 

[ 
\đóng hộp{ 
\sum_{x=1}^{k}a^x M\left(\left\lfloor\frac{k}{x}\right\rfloor\right) 
}. 
] 

Bây giờ mọi số hạng đều thu được trong thời gian không đổi sau khi biết tổng tiền tố Möbius. Chúng ta có thể tạo ra tất cả các giá trị Möbius bằng sàng Euler tuyến tính, xây dựng tổng tiền tố của chúng và sau đó đánh giá công thức trong một lần tuyến tính nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`Θ(k a^k)`nếu tính tuần hoàn được kiểm tra trực tiếp |`O(k)`mỗi ứng viên | Quá chậm | 
| Tối ưu |`O(k)`|`O(k)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước bảng chữ cái`a`và độ dài khóa tối đa`k`. Nếu như`a=1`, quay lại ngay`1`, bởi vì mọi khóa có thể đều bao gồm cùng một ký hiệu và có cùng một gốc nguyên thủy. 
2. Tính hàm Mobius`μ(1), μ(2), ..., μ(k)`bằng sàng Euler tuyến tính. Sàng giữ hệ số nguyên tố nhỏ nhất của mỗi số và cấu trúc`μ`cùng một lúc. Một số nguyên tố nhận được giá trị Möbius`-1`. Nếu một số nhận được thừa số nguyên tố đã có sẵn trong hệ số hóa của nó, giá trị Möbius của nó sẽ trở thành`0`. Nếu không thì biển hiệu sẽ bị đảo lộn. 
3. Chuyển đổi mảng Möbius thành tổng tiền tố của nó. Sau phép biến đổi này, giá trị được lưu tại vị trí`t`là 

[ 
M(t)=\mu(1)+\mu(2)+\cdots+\mu(t). 
] 

Các giá trị Möbius ban đầu không còn cần thiết nữa, do đó, bộ lưu trữ tương tự được sử dụng cho sàng có thể được sử dụng lại cho các tổng tiền tố này. 

1. Duy trì`power = a^x mod MOD`trong khi lặp lại`x`từ`1`ĐẾN`k`. Đối với hiện tại`x`, thêm 

[ 
a^x M\left(\left\lfloor\frac{k}{x}\right\rfloor\right) 
] 

để trả lời. Tổng tiền tố được lập chỉ mục bởi`k // x`bởi vì đó chính xác là phạm vi của các ước số có thể`d`sau khi thay thế`n = d x`. 

1. Cập nhật`power`bằng cách nhân nó với`a`modulo`10^9+7`. Sau lần lặp cuối cùng, in câu trả lời tích lũy theo modulo`10^9+7`. 

### Tại sao nó hoạt động 

Mỗi khóa không trống có một gốc nguyên thủy duy nhất. Hai khóa tạo ra cùng một chuỗi dịch chuyển định kỳ chính xác khi chúng có cùng một gốc nguyên thủy, do đó số lượng khóa mà kẻ tấn công phải thử chính xác là số chuỗi nguyên thủy có độ dài tối đa`k`. 

Về chiều dài`n`, mỗi chuỗi là sự lặp lại của một chuỗi nguyên thủy duy nhất có độ dài chia cho`n`. Như vậy`a^n = Σ_{d|n} P(d)`và nghịch đảo Möbius cho`P(n) = Σ_{d|n} μ(d)a^{n/d}`. 

Tổng hợp tất cả`n <= k`và trao đổi thứ tự tổng hợp cho 

\sum_{x=1}^{k}a^xM(\lfloor k/x\rfloor). 
] 

Thuật toán đánh giá chính xác biểu thức này, vì vậy mỗi lớp gốc nguyên thủy chỉ đóng góp một lần và chỉ một lần. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    a, k = map(int, input().split())

    # With a one-letter alphabet, every key is a repetition
    # of the same one-character primitive root.
    if a == 1:
        print(1)
        return

    # lp[x] = smallest prime factor of x.
    # Using a compact integer array keeps memory usage small.
    lp = array('I', [0]) * (k + 1)

    # mu[x] is stored as -1, 0, or 1.
    mu = array('b', [0]) * (k + 1)
    mu[1] = 1

    primes = array('I')

    # Linear Euler sieve for the Möbius function.
    for i in range(2, k + 1):
        if lp[i] == 0:
            lp[i] = i
            primes.append(i)
            mu[i] = -1

        li = lp[i]

        for p in primes:
            x = i * p
            if x > k:
                break

            lp[x] = p

            if p == li:
                mu[x] = 0
                break
            else:
                mu[x] = -mu[i]

    # lp is no longer needed as a smallest-prime-factor array.
    # Reuse it to store Mertens prefix sums:
    # lp[i] = mu(1) + ... + mu(i).
    prefix = 0
    for i in range(1, k + 1):
        prefix += mu[i]
        lp[i] = prefix

    del mu
    del primes

    ans = 0
    power = a % MOD

    # ans = sum_{x=1}^k a^x * M(floor(k/x)).
    for x in range(1, k + 1):
        ans = (ans + power * lp[k // x]) % MOD
        power = (power * a) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Sàng Euler là việc thực hiện Bước 2.`lp`ghi lại thừa số nguyên tố nhỏ nhất, trong khi`mu`chỉ lưu trữ ba giá trị có thể, do đó, một mảng byte có dấu là đủ cho hàm Möbius. 

điều kiện`p == li`là phần quan trọng của sàng. Nếu như`p`đã là thừa số nguyên tố nhỏ nhất của`i`, sau đó`i*p`chứa một thừa số nguyên tố bình phương, vì vậy giá trị Möbius của nó bằng 0. Ngược lại, nhân với một số nguyên tố mới sẽ thay đổi dấu của giá trị Möbius. 

Sau sàng,`lp`không còn cần thiết cho việc phân tích nhân tử nữa. Việc sử dụng lại nó cho tổng tiền tố Mertens sẽ tránh được việc phân bổ một mảng lớn khác. Điều này quan trọng bởi vì`k`có thể đạt tới năm triệu. 

Vòng lặp cuối cùng không tính toán`P(x)`riêng lẻ. Thay vào đó, nó trực tiếp đánh giá tổng được chuyển đổi 

[ 
a^xM(\lfloor k/x\rfloor). 
]`power`luôn chứa`a^x mod MOD`ở đầu vòng lặp cho`x`. Cập nhật nó sau khi thêm thuật ngữ hiện tại sẽ tránh được lỗi từng cái một. Số nguyên Python không bị tràn, nhưng việc giảm sau mỗi phép nhân sẽ giữ cho các giá trị trung gian nhỏ và việc triển khai hiệu quả. 

Trường hợp đặc biệt`a=1`cũng không chỉ là tối ưu hóa vi mô. Nó thực hiện việc kiểm tra độ dài tối đa ngay lập tức, bởi vì chỉ có một khóa có thể có ở mỗi độ dài và tất cả chúng đều có cùng một gốc nguyên thủy. 

## Ví dụ đã hoạt động 

### Mẫu 1:`a = 26, k = 1`Chỉ cho phép các khóa có độ dài một. Mỗi khóa một ký tự đều là khóa nguyên thủy, do đó câu trả lời phải là`26`. 

Vì`k=1`, các giá trị Möbius và tổng tiền tố là: 

|`x`|`μ(x)`|`M(x)`|`a^x`|`k // x`| Đã thêm thuật ngữ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 26 | 1 | 26 | 

Câu trả lời cuối cùng là`26`. 

Điều này xác nhận rằng phép biến đổi không làm mất trường hợp đơn giản nhất. Công thức giảm xuống còn`a^1 * M(1) = a`. 

### Mẫu 2:`a = 2, k = 2`Có bốn khóa thô:`0`,`1`,`00`, Và`11`. Hai cái đầu tiên là nguyên thủy. Hai cái cuối cùng là sự lặp lại của hai cái đầu tiên, vì vậy vẫn còn bốn chìa khóa để thử theo cách giải thích khóa đại diện của bài toán, bởi vì các chuỗi nguyên thủy có độ dài nhiều nhất là hai cái.`0`,`1`,`01`, Và`10`. 

Các giá trị Möbius liên quan là 

[ 
\mu(1)=1,\qquad \mu(2)=-1, 
] 

vậy tổng tiền tố là 

[ 
M(1)=1,\qquad M(2)=0. 
] 

Tổng được chuyển đổi là: 

|`x`|`μ(x)`|`M(x)`|`a^x`|`k // x`|`M(k // x)`| Đã thêm thuật ngữ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 2 | 0 | 0 | 
| 2 | -1 | 0 | 4 | 1 | 1 | 4 | 

Câu trả lời là`4`. 

Đóng góp bằng không cho`x=1`phản ánh sự hủy bỏ gây ra bởi sự lặp lại có độ dài hai. Bốn đại diện còn lại chính xác là bốn chuỗi nguyên thủy có độ dài một và hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(k)`| Sàng Euler thực hiện công tuyến tính, sau đó là hai lần quét tuyến tính. | 
| Không gian |`O(k)`| Mảng hệ số nguyên tố nhỏ nhất, giá trị Möbius và danh sách nguyên tố đều có kích thước tuyến tính. | 

Với`k <= 5 * 10^6`, độ phức tạp tuyến tính là phù hợp. Việc thực hiện sử dụng nhỏ gọn`array`lưu trữ thay vì danh sách số nguyên có mục đích chung lớn hơn nhiều của Python dành cho dữ liệu sàng, giữ mức sử dụng bộ nhớ thoải mái dưới mức`512 MB`giới hạn. 

## Trường hợp thử nghiệm```python
import io
import sys
from array import array

MOD = 1_000_000_007

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    a, k = map(int, sys.stdin.readline().split())

    if a == 1:
        print(1)
    else:
        lp = array('I', [0]) * (k + 1)
        mu = array('b', [0]) * (k + 1)
        mu[1] = 1
        primes = array('I')

        for i in range(2, k + 1):
            if lp[i] == 0:
                lp[i] = i
                primes.append(i)
                mu[i] = -1

            li = lp[i]

            for p in primes:
                x = i * p
                if x > k:
                    break

                lp[x] = p

                if p == li:
                    mu[x] = 0
                    break
                mu[x] = -mu[i]

        s = 0
        for i in range(1, k + 1):
            s += mu[i]
            lp[i] = s

        ans = 0
        power = a % MOD

        for x in range(1, k + 1):
            ans = (ans + power * lp[k // x]) % MOD
            power = power * a % MOD

        print(ans)

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("26 1\n") == "26\n", "sample 1"
assert run("2 2\n") == "4\n", "sample 2"
assert run("1 3\n") == "1\n", "sample 3"

assert run("1 1\n") == "1\n", "minimum alphabet and key length"
assert run("2 3\n") == "10\n", "primitive lengths 1, 2, and 3"
assert run("1000 1\n") == "1000\n", "maximum alphabet at key length 1"
assert run("1 5000000\n") == "1\n", "maximum key length with one-letter alphabet"
assert run("2 4\n") == "22\n", "length-four divisor boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Giá trị tối thiểu và kiểu chữ cái một ký hiệu | 
|`2 3`|`10`| Một số độ dài nguyên thủy và độ dài khóa nguyên tố | 
|`1000 1`|`1000`| Kích thước bảng chữ cái tối đa và`k=1`ranh giới | 
|`1 5000000`|`1`| Tối đa`k`và điều đặc biệt`a=1`trường hợp | 
|`2 4`|`22`| Chiều dài tổng hợp với các ước số không cần thiết | 

## Vỏ cạnh 

cho`a=1`Và`k=3`, các khóa duy nhất có thể có là chuỗi một ký hiệu, sự lặp lại hai ký hiệu và sự lặp lại ba ký hiệu của nó. Mọi người đều có cùng một gốc nguyên thủy. Việc thực hiện trở lại`1`ngay, tránh công việc sàng không cần thiết. 

Vì`a=2`Và`k=2`, các chuỗi nguyên thủy là`0`,`1`,`01`, Và`10`. Dây đàn`00`Và`11`không phải là những gốc nguyên thủy mới vì chúng là sự lặp lại của`0`Và`1`. Tính toán Mobius cho`P(1)=2`Và`P(2)=4-2=2`, vậy tổng số là`4`. 

Vì`a=2`Và`k=3`, đóng góp theo chiều dài là`2`. Với độ dài hai, hai chuỗi không nguyên thủy là`00`Và`11`, rời đi`2`các chuỗi nguyên thủy. Ở độ dài ba, ước số thực sự duy nhất là một, vì vậy`P(3)=2^3-2=6`. Câu trả lời là`2+2+6=10`. Trường hợp này phát hiện các triển khai vô tình loại trừ các chuỗi nguyên thủy ở độ dài nguyên tố. 

Vì`a=2`Và`k=4`, các chuỗi có độ dài bốn bao gồm sự lặp lại của các gốc có độ dài một và độ dài hai. Công thức cho 

[ 
P(4)=2^4-2^2=12, 
] 

bởi vì`μ(4)=0`Và`μ(2)=-1`. Tổng cộng là 

# 2+2+6+12 

1. 

] 

Điều này phát hiện các sai lầm về ranh giới số chia liên quan đến hệ số bình phương, trong đó giá trị Möbius phải bằng 0. 

Cuối cùng, đối với`a=1`Và`k=5,000,000`, câu trả lời vẫn còn`1`. Đây vừa là trường hợp cạnh toán học vừa là trường hợp ứng suất thực tế. Một giải pháp xây dựng hoặc sàng lọc mọi chiều dài một cách không cần thiết có thể lãng phí thời gian đáng kể, trong khi trường hợp đặc biệt sẽ kết thúc ngay lập tức.
