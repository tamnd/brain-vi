---
title: "CF 102388F - Mua sắm"
description: "Chúng ta có n mệnh giá tiền xu và mỗi mệnh giá có thể được sử dụng bao nhiêu lần cũng được. Đối với số lượng mục tiêu m, chúng ta cần đếm có bao nhiêu tập hợp đồng xu khác nhau có tổng chính xác bằng m. Thứ tự của các đồng tiền không quan trọng, vì vậy 2 + 3 và 3 + 2 thể hiện cùng một cách thực hiện thay đổi."
date: "2026-08-15T08:29:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102388
codeforces_index: "F"
codeforces_contest_name: "SUFE ICPC Team Formation Test"
rating: 0
weight: 102388
solve_time_s: 512
verified: true
draft: false
---

[CF 102388F - Mua sắm](https://codeforces.com/problemset/problem/102388/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`mệnh giá tiền xu và mỗi mệnh giá có thể được sử dụng bất kỳ số lần nào. Đối với số tiền mục tiêu`m`, chúng ta cần đếm chính xác có bao nhiêu tập hợp đồng xu khác nhau`m`. Thứ tự của các đồng tiền không quan trọng, vì vậy`2 + 3`Và`3 + 2`đại diện cho cùng một cách thực hiện thay đổi. Câu trả lời được báo cáo modulo`1,000,000,007`. 

Ví dụ, với các mệnh đề`2, 3, 5, 7`và mục tiêu`10`, các kết hợp hợp lệ là`3 + 7`,`5 + 5`,`2 + 3 + 5`,`2 + 2 + 3 + 3`, và năm bản sao của`2`, đưa ra câu trả lời của`5`. 

Mục tiêu nhiều nhất là`10,000`, trong khi có thể có nhiều nhất`100`giáo phái. Điều đó gợi ý mạnh mẽ về một giải pháp lập trình động xung quanh`n * m`, nhiều nhất là khoảng một triệu cập nhật trạng thái cho mỗi trường hợp thử nghiệm. Với tối đa mười trường hợp thử nghiệm, điều này vẫn thực tế trong Python với DP một chiều nhỏ gọn. Một giải pháp liệt kê tất cả các kết hợp có thể là không khả thi từ xa, bởi vì ngay cả một mệnh giá duy nhất cũng có thể được sử dụng tới`m`lần và số lượng kết hợp tăng lên nhanh chóng khi có nhiều mệnh giá được thêm vào. 

Các giá trị mệnh giá cũng có thể lớn bằng`10,000`. Bất kỳ mệnh giá nào lớn hơn`m`không bao giờ có thể tham gia vào một sự kết hợp hợp lệ, do đó việc xử lý nó là không cần thiết. Câu lệnh mô tả các mệnh giá là khác nhau, nhưng việc loại bỏ chúng trong quá trình triển khai sẽ làm cho thuật toán trở nên mạnh mẽ hơn nếu các giá trị lặp lại được cung cấp. Các mệnh giá giống hệt nhau lặp đi lặp lại không được tạo ra các cách khác nhau, bởi vì một cách được xác định bởi giá trị và bội số của các đồng tiền, chứ không phải bởi vị trí đầu vào cung cấp một đồng xu. 

Có một số trường hợp nghiêm trọng trong đó việc triển khai bất cẩn có thể âm thầm đếm sai. Với```
1
1 3
5
```câu trả lời là`0`, bởi vì đồng xu duy nhất có sẵn đã lớn hơn mục tiêu. Một sự tái diễn lập chỉ mục một cách mù quáng`dp[amount - coin]`không kiểm tra ranh giới có thể truy cập trạng thái không hợp lệ. 

Với```
1
2 5
2 3
```câu trả lời là`1`, từ`2 + 3`. Một lỗi phổ biến là cập nhật DP không đúng thứ tự và vô tình đếm các hoán vị khác nhau của cùng một đồng tiền. Hai trình tự`2 + 3`Và`3 + 2`không được trở thành những câu trả lời riêng biệt. 

Với```
1
2 4
2 2
```câu trả lời là`1`nếu các mệnh giá trùng lặp được hiểu là cùng một giá trị đồng tiền. Việc coi hai mục đầu vào là các loại tiền độc lập sẽ tính cùng một kết hợp tiền tệ theo nhiều cách. Vấn đề ban đầu hứa hẹn các mệnh giá khác nhau, nhưng việc loại bỏ trùng lặp sẽ tránh được sự mơ hồ đó. 

Cuối cùng, với```
1
1 1
1
```câu trả lời là`1`. Tổng trống được biểu thị bằng`dp[0] = 1`, và thêm mệnh giá`1`phải biến điều đó thành đúng một cách để tạo thành số tiền`1`. Việc khởi tạo mảng DP bằng 0 ở mọi nơi sẽ làm mất trường hợp cơ bản cơ bản này. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là chọn số lượng bản sao của mỗi mệnh giá được sử dụng và kiểm tra xem tổng kết quả có đúng không.`m`. Đối với mệnh giá`c_i`, số đếm của nó có thể dao động từ 0 đến`floor(m / c_i)`, do đó số lượng kết hợp được kiểm tra là`product(floor(m / c_i) + 1)`. 

Điều này đúng vì mọi tập hợp xu có thể có đều tương ứng với chính xác một lựa chọn trong số đó. Vấn đề là kích thước của không gian tìm kiếm đó. Trong trường hợp xấu nhất, nếu tất cả các mệnh giá đều`1`, về mặt lý thuyết có`10001^100`lựa chọn số đếm, mặc dù tuyên bố ban đầu không cho phép các mệnh giá trùng lặp. Ngay cả với những mệnh giá nhỏ khác nhau, không gian tìm kiếm vẫn trở nên khổng lồ. Do đó, phép liệt kê đệ quy sẽ thất bại rất lâu trước khi đạt được`n = 100`Và`m = 10000`. 

Một chiến lược bạo lực tinh vi hơn tạo ra mọi chuỗi tiền xu có thứ tự mà tổng số tiền không vượt quá`m`, sau đó chỉ giữ các chuỗi đạt chính xác`m`. Điều này thậm chí còn tệ hơn vì sự kết hợp giống nhau xuất hiện trong nhiều hoán vị. Ví dụ,`2 + 3 + 5`có thể được tạo ra theo sáu thứ tự khác nhau. Bài toán thực tế yêu cầu một câu trả lời cho sự kết hợp đó, vì vậy việc khám phá trật tự sẽ tạo ra công việc mà bài toán không cần. 

Quan sát quan trọng là số tiền mục tiêu được giới hạn bởi`10,000`, vì vậy chúng ta không cần phải nhớ toàn bộ bộ sưu tập tiền xu đã chọn cho đến nay. Chúng ta chỉ cần biết mỗi lượng nhỏ hơn có thể được hình thành theo bao nhiêu cách. Khi các mệnh giá đã được xử lý theo một thứ tự cố định, chúng tôi có thể quyết định có nên thêm mệnh giá hiện tại vào một tổ hợp đã được tính hay không. 

Cho phép`dp[x]`là số cách để tạo thành số tiền`x`sử dụng các mệnh giá được xử lý cho đến nay. Khi xử lý một đồng tiền có giá trị`c`, mọi cách để hình thành`x - c`có thể được mở rộng thêm một`c`đồng xu để hình thành`x`. Vì vậy chúng tôi thêm`dp[x - c]`ĐẾN`dp[x]`. 

Thứ tự của các vòng lặp là yếu tố tạo nên sự kết hợp đếm này chứ không phải là hoán vị. Chúng tôi xử lý các mệnh giá ở vòng lặp bên ngoài và số lượng theo thứ tự tăng dần ở vòng lặp bên trong. Một khi giáo phái`c`đang được xử lý,`dp[x - c]`đã có những cách có thể sử dụng`c`, cho phép sao chép không giới hạn. Đồng thời, mọi mệnh giá được giới thiệu trong một giai đoạn cố định, do đó, sự kết hợp tương tự không thể được tạo lại theo một thứ tự khác. 

Lực lượng vũ phu hoạt động vì nó xem xét rõ ràng mọi bội số có thể có của mỗi đồng tiền, nhưng không thành công khi có quá nhiều lựa chọn như vậy. Quan sát thấy hai giải pháp từng phần có cùng lượng hiện tại có khả năng giống nhau trong tương lai cho phép chúng ta hợp nhất chúng thành một trạng thái DP. Điều đó làm giảm vấn đề xuống`O(nm)`chuyển tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(product(m / c_i + 1))`trong trường hợp xấu nhất |`O(n)`độ sâu đệ quy | Quá chậm | 
| Tối ưu |`O(nm)`|`O(m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các mệnh giá và loại bỏ các giá trị trùng lặp nếu có. Một mệnh giá đại diện cho một giá trị đồng xu, vì vậy các bản sao trùng lặp của cùng một giá trị sẽ không tạo ra những cách khác biệt để thực hiện thay đổi. 
2. Bỏ qua mọi mệnh giá lớn hơn`m`. Một đồng xu như vậy không bao giờ có thể xuất hiện với số tiền hợp lệ bằng`m`, vì vậy việc xử lý nó không thể thay đổi câu trả lời. 
3. Tạo mảng một chiều`dp`chiều dài`m + 1`và khởi tạo mọi mục nhập về 0. Bộ`dp[0] = 1`. Có chính xác một cách để kiếm được số tiền bằng 0, đó là không chọn xu. Trường hợp cơ bản này là điều cho phép đồng tiền thực tế đầu tiên bắt đầu kết hợp. 
4. Xử lý từng mệnh giá`c`từng cái một. Đối với mệnh giá hiện tại, lặp lại`x`từ`c`bởi vì`m`theo thứ tự tăng dần và thực hiện`dp[x] += dp[x - c]`modulo`1,000,000,007`. 
5. Thứ tự tăng dần của`x`cố tình cho phép`dp[x - c]`để bao gồm mệnh giá hiện tại. Ví dụ: trong khi xử lý tiền xu`2`, bản cập nhật cho`dp[4]`có thể sử dụng bản cập nhật mới`dp[2]`, đại diện cho hai bản sao của`2`. Nếu số tiền được xử lý theo thứ tự giảm dần thì mỗi mệnh giá có thể được sử dụng nhiều nhất một lần. 
6. Sau khi tất cả các mệnh giá đã được xử lý,`dp[m]`chứa số lượng các kết hợp khác nhau có tổng giá trị chính xác`m`. In giá trị đó theo modulo`1,000,000,007`. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý lần đầu tiên`k`giáo phái,`dp[x]`bằng số lượng kết hợp hình thành chính xác`x`chỉ sử dụng những`k`giáo phái. Khi xử lý một mệnh giá mới`c`, mọi sự kết hợp cũ hình thành`x`vẫn có sẵn và mọi kết hợp sử dụng ít nhất một`c`có thể thu được duy nhất bằng cách loại bỏ một`c`, để lại một sự kết hợp được tính bởi`dp[x - c]`. Do đó, bản cập nhật bổ sung chính xác các kết hợp mới được giới thiệu bởi`c`. 

Bởi vì các mệnh giá được xử lý theo một thứ tự cố định nên một sự kết hợp có một điểm duy nhất mà tại đó mệnh giá được lập chỉ mục lớn nhất của nó được đưa ra. Nó không thể được tính lại thông qua hoán vị khác của cùng một đồng tiền. Bởi vì số lượng được xử lý từ nhỏ đến lớn, mệnh giá hiện tại có thể được sử dụng nhiều lần, do đó tất cả các bội số hợp lệ đều được đưa vào. Do đó, tính bất biến có giá trị sau mỗi mệnh giá, và`dp[m]`là câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        coins = list(map(int, input().split()))

        # The original statement has distinct denominations.
        # Deduplication also makes the implementation robust to repeated values.
        coins = sorted(set(c for c in coins if c <= m))

        dp = [0] * (m + 1)
        dp[0] = 1

        for coin in coins:
            for amount in range(coin, m + 1):
                dp[amount] += dp[amount - coin]
                if dp[amount] >= MOD:
                    dp[amount] -= MOD

        out.append(str(dp[m]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc một lần cho mỗi trường hợp thử nghiệm và các mệnh giá được lọc và loại bỏ trùng lặp trước khi DP bắt đầu. Việc sắp xếp không cần thiết để đảm bảo tính chính xác nhưng giúp bộ đồng xu có một thứ tự xác định và giúp dễ dàng xử lý các mệnh giá một cách nhất quán. 

Mảng DP có các chỉ số`0`bởi vì`m`, vậy kích thước của nó chính xác là`m + 1`.`dp[0] = 1`đại diện cho sự kết hợp trống duy nhất. Không có trạng thái ban đầu nào khác được đặt thành giá trị khác 0. 

Đối với mỗi đồng xu, vòng lặp bên trong bắt đầu tại`coin`, bởi vì số tiền dưới giá trị đồng xu không thể sử dụng mệnh giá đó. Nó kết thúc lúc`m`, bởi vì số tiền lớn hơn mục tiêu không thể đóng góp vào`dp[m]`. 

Vòng bên trong di chuyển lên trên. Đây là chi tiết triển khai quan trọng nhất. Giả sử đồng tiền hiện tại là`2`. Sau khi tính toán`dp[2]`, bản cập nhật sau cho`dp[4]`đọc giá trị cập nhật đó, vì vậy hai bản sao của`2`được phép. Nếu vòng lặp di chuyển xuống dưới, đồng xu hiện tại sẽ không hiển thị trong cùng một lần lặp, biến quá trình lặp lại thành chuyển đổi sử dụng 0 hoặc 1. 

Số nguyên Python không tràn, nhưng lấy mọi bản cập nhật theo modulo`MOD`giữ các giá trị được lưu trữ ở mức nhỏ và trực tiếp thực hiện phép tính số học mô-đun cần thiết. Vì cả hai toán hạng đều ở bên dưới`MOD`, một phép trừ là đủ sau khi cộng để giữ kết quả trong phạm vi`[0, MOD)`. 

## Ví dụ đã hoạt động 

### Trường hợp mẫu: mệnh giá 5, 7, 2, 3, target 10 

Các mệnh giá có thể được xử lý theo thứ tự đầu vào. Bảng sau đây hiển thị trạng thái DP có liên quan sau khi mỗi mệnh giá được xử lý đầy đủ. 

| Tiền đã qua xử lý |`dp[0]`|`dp[2]`|`dp[3]`|`dp[4]`|`dp[5]`|`dp[6]`|`dp[7]`|`dp[8]`|`dp[9]`|`dp[10]`| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| không | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 
| 5 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 1 | 
| 5, 7 | 1 | 0 | 0 | 0 | 1 | 0 | 1 | 0 | 0 | 1 | 
| 5, 7, 2 | 1 | 1 | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 2 | 
| 5, 7, 2, 3 | 1 | 1 | 1 | 1 | 2 | 2 | 2 | 2 | 3 | 5 | 

Sau khi xử lý xu`2`,`dp[10] = 2`, đại diện`5 + 5`Và`2 + 2 + 2 + 2 + 2`. Khi đồng xu`3`được thêm vào, ba kết hợp khác sẽ xuất hiện:`3 + 7`,`2 + 3 + 5`, Và`2 + 2 + 3 + 3`. Giá trị cuối cùng là`5`, phù hợp với mẫu 

Dấu vết thể hiện cả hai phần của bất biến. Mỗi trạng thái thể hiện sự kết hợp chứ không phải là các chuỗi có thứ tự, trong khi vòng lặp số lượng tăng lên cho phép sao chép không giới hạn mỗi mệnh giá. 

###Trường hợp mẫu: mệnh giá 101, 102, 103, 104, target 100 

Mọi mệnh giá đều lớn hơn mục tiêu nên việc lọc không để lại đồng xu nào có thể sử dụng được. 

| Đồng xu đã được xử lý | Khởi tạo DP | Cuối cùng`dp[100]`| 
| --- | --- | --- | 
| không |`dp[0] = 1`, tất cả các tiểu bang khác`0`| 0 | 
| 101 | bỏ qua | 0 | 
| 102 | bỏ qua | 0 | 
| 103 | bỏ qua | 0 | 
| 104 | bỏ qua | 0 | 

Câu trả lời là`0`, bởi vì không có đồng xu sẵn có nào có thể được đặt vào một tổng`100`. Điều này thực hiện ranh giới trong đó mọi mệnh giá đều vượt quá mục tiêu và xác nhận rằng DP không cần bất kỳ trường hợp đặc biệt nào ngoài việc lọc những đồng tiền đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nm)`| Mỗi mệnh giá có thể sử dụng được cập nhật tối đa`m`trạng thái DP. | 
| Không gian |`O(m)`| Chỉ mảng DP một chiều hiện tại được lưu trữ. | 

Với`n <= 100`Và`m <= 10000`, có nhiều nhất khoảng một triệu chuyển đổi DP cơ bản cho mỗi trường hợp thử nghiệm. Ngay cả với mười trường hợp thử nghiệm, tổng số vẫn vào khoảng mười triệu lần chuyển đổi, phù hợp với các ràng buộc đã định. Mức tiêu thụ bộ nhớ là khoảng`10,001`Số nguyên Python cho mục tiêu lớn nhất, nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1_000_000_007

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        coins = list(map(int, input().split()))

        coins = sorted(set(c for c in coins if c <= m))

        dp = [0] * (m + 1)
        dp[0] = 1

        for coin in coins:
            for amount in range(coin, m + 1):
                dp[amount] += dp[amount - coin]
                if dp[amount] >= MOD:
                    dp[amount] -= MOD

        out.append(str(dp[m]))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample = """\
5
1 100
1
4 10
5 7 2 3
3 100
1 2 3
4 100
101 102 103 104
5 10000
5 4 3 2 1
"""

assert run(sample) == """\
1
5
884
0
649632988
""", "provided samples"

assert run("""\
1
1 1
1
""") == "1\n", "minimum target with the only usable coin"

assert run("""\
1
1 10000
9999
""") == "0\n", "coin is smaller than target but cannot divide it"

assert run("""\
1
2 5
2 3
""") == "1\n", "boundary combination 2 + 3"

assert run("""\
1
2 4
2 2
""") == "1\n", "duplicate denominations must not multiply the answer"

assert run("""\
1
100 10000
""" + " ".join(["1"] * 100) + "\n") == "1\n", \
    "maximum n and m with repeated denomination values"

assert run("""\
1
3 10
11 12 13
""") == "0\n", "every coin exceeds the target"

assert run("""\
1
3 6
2 3 6
""") == "3\n", "exact target and multiple combination sizes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 1`|`1`| Kích thước đầu vào tối thiểu và`dp[0]`trường hợp cơ sở | 
|`1 / 1 10000 / 9999`|`0`| Một mệnh giá trông có vẻ hữu dụng nhưng thực tế lại không thể đạt được mục tiêu | 
|`1 / 2 5 / 2 3`|`1`| Kết hợp ranh giới chính xác và điểm cuối DP bao gồm chính xác | 
|`1 / 2 4 / 2 2`|`1`| Xử lý mệnh giá trùng lặp | 
|`1 / 100 10000 / 100 copies of 1`|`1`| Tối đa`n`và tối đa`m`| 
|`1 / 3 10 / 11 12 13`|`0`| Tất cả các mệnh giá lớn hơn mục tiêu | 
|`1 / 3 6 / 2 3 6`|`3`| Một số bội số khác nhau đạt được mục tiêu chính xác | 

## Vỏ cạnh 

### Một mệnh giá lớn hơn mục tiêu 

Hãy xem xét```
1
1 3
5
```Đồng xu`5`bị loại bỏ vì`5 > 3`. DP vẫn còn`[1, 0, 0, 0]`, Vì thế`dp[3] = 0`. Thuật toán không cố gắng truy cập`dp[3 - 5]`, tránh lỗi lập chỉ mục tiêu cực trong Python. 

### Thứ tự không được tạo ra các kết hợp khác nhau 

Hãy xem xét```
1
2 5
2 3
```Ban đầu`dp[0] = 1`. Xử lý tiền xu`2`tạo ra một cách cho số tiền`2`Và`4`, tương ứng với`2`Và`2 + 2`. Xử lý tiền xu`3`sau đó sử dụng các trạng thái đó, vì vậy`dp[5]`nhận được một đóng góp từ`dp[2]`, đại diện`2 + 3`. Không có đóng góp thứ hai tương ứng với`3 + 2`, bởi vì mệnh giá`2`đã được xử lý trước đó`3`. Đầu ra chính xác là`1`. 

### Giá trị mệnh giá lặp lại 

Hãy xem xét```
1
2 4
2 2
```Sau khi loại bỏ trùng lặp, danh sách tiền xu chỉ là`[2]`. DP sản xuất`dp[4] = 1`, tương ứng với`2 + 2`. Nếu không loại bỏ trùng lặp, việc coi hai giá trị bằng nhau là các loại tiền riêng biệt sẽ làm cho cùng một tổ hợp tiền tệ xuất hiện thông qua các vị trí đầu vào khác nhau. Tuyên bố đảm bảo các mệnh giá riêng biệt, vì vậy trường hợp này nằm ngoài mô hình đầu vào chính thức, nhưng quá trình triển khai xử lý nó một cách an toàn. 

### Tổ hợp trống làm trường hợp cơ sở DP 

cho```
1
1 1
1
```trạng thái ban đầu là`dp[0] = 1`. Xử lý tiền xu`1`cập nhật`dp[1]`từ`dp[0]`, cho`dp[1] = 1`. Một cách là một cách duy nhất`1`đồng xu. Nếu như`dp[0]`được khởi tạo về 0, sẽ không có số lượng nào có thể truy cập được vì mọi chuyển đổi sẽ phụ thuộc vào trạng thái không thể truy cập được. 

### Ranh giới chính xác tại mục tiêu 

Hãy xem xét```
1
3 6
2 3 6
```Xử lý`2`đưa ra một cách để thực hiện`2`,`4`, Và`6`. Xử lý`3`thêm vào`3`Và`6`, trong khi số tiền`6`cũng nhận được sự kết hợp`3 + 3`. Cuối cùng là đồng tiền`6`thêm sự kết hợp trực tiếp`[6]`. Ba sự kết hợp riêng biệt là`2 + 2 + 2`,`3 + 3`, Và`6`, vậy câu trả lời là`3`. 

Vòng lặp`range(coin, m + 1)`bao gồm`m`chính nó. Dừng lại ở`m - 1`sẽ bỏ lỡ mọi sự kết hợp mà bản cập nhật cuối cùng của nó tạo ra mục tiêu trực tiếp, bao gồm cả một đồng xu`6`trong ví dụ này.
