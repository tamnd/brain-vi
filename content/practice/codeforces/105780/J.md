---
title: "CF 105780J - Hoa đa chiều"
description: "Bông hoa mọc trên lưới số nguyên d chiều. Nó bắt đầu từ điểm gốc và mỗi giây nó di chuyển chính xác một đơn vị dọc theo một trục tọa độ. Hướng phải luôn cách xa điểm gốc dọc theo trục đó."
date: "2026-06-25T15:54:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105780
codeforces_index: "J"
codeforces_contest_name: "UTPC x WiCS Contest 3-12-25 (UT Internal)"
rating: 0
weight: 105780
solve_time_s: 37
verified: true
draft: false
---

[CF 105780J - Bông hoa đa chiều](https://codeforces.com/problemset/problem/105780/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hoa mọc trên một`d`lưới số nguyên chiều. Nó bắt đầu từ điểm gốc và mỗi giây nó di chuyển chính xác một đơn vị dọc theo một trục tọa độ. Hướng phải luôn cách xa điểm gốc dọc theo trục đó. Nhiệm vụ là đếm xem có bao nhiêu chuỗi quyết định tăng trưởng khác nhau có thể xảy ra sau một thời gian chính xác.`t`giây. Hai chuỗi sẽ khác nhau nếu tại bất kỳ giây nào chúng chọn một trục khác hoặc một hướng khác. 

Đầu vào cung cấp số thứ nguyên và số bước tăng trưởng. Đầu ra là số trình tự có thứ tự hợp lệ của các bước này, lấy modulo`998244353`. 

Các ràng buộc cho phép cả hai`d`Và`t`lớn như`200000`. Một giải pháp bậc hai sẽ yêu cầu khoảng`4 * 10^10`trong trường hợp xấu nhất vượt xa giới hạn. Giải pháp cuối cùng phải gần với tuyến tính, vì vậy chúng ta cần tránh theo dõi từng giây hoặc kích thước riêng lẻ bằng các vòng lặp lồng nhau. 

Phần khó khăn là hiểu điều gì xảy ra với một tọa độ duy nhất. Khi tọa độ được chọn lần đầu tiên, nó sẽ trở thành dương hoặc âm. Mọi sự tăng trưởng sau này trên tọa độ đó chỉ có một hướng khả thi vì việc di chuyển về 0 là bị cấm. Một tọa độ chưa bao giờ được sử dụng sẽ không góp phần vào sự lựa chọn hướng. 

Một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. 

Đối với một chiều và một giây:```
Input:
1 1

Output:
2
```Tọa độ duy nhất có thể tăng trưởng tích cực hoặc tiêu cực. Một giải pháp giả định rằng chỉ có một hướng trên mỗi trục sẽ trả về sai`1`. 

Đối với hai chiều và một giây:```
Input:
2 1

Output:
4
```Bông hoa có thể chọn kích thước và ký hiệu. Ở đây chỉ tính các vị trí cuối cùng sẽ cho cùng một câu trả lời, nhưng đối với các ví dụ lớn hơn, các thứ tự tăng trưởng khác nhau có thể đạt đến cùng một điểm cuối cùng. Vấn đề yêu cầu các lịch sử khác nhau chứ không phải tọa độ kết thúc khác nhau. 

Đối với ba chiều và ba giây:```
Input:
3 3

Output:
126
```Tọa độ được sử dụng hai lần chỉ có hai lịch sử hướng có thể chứ không phải bốn. Khi dấu hiệu đầu tiên được chọn, tất cả các bước di chuyển trong tương lai trên tọa độ đó sẽ bị buộc phải thực hiện. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng mô phỏng tất cả các lịch sử tăng trưởng có thể có. Tại mỗi giây ban đầu có`2d`các lựa chọn, và mặc dù số lượng các lựa chọn có sẵn giảm đi một chút khi tọa độ cố định, số lượng các lịch sử có thể có vẫn tăng theo cấp số nhân. Vì`t`giây này đại khái là theo thứ tự`(2d)^t`, làm cho nó không thể sử dụng được ngay cả đối với đầu vào rất nhỏ. 

Quan sát hữu ích đến từ việc tách biệt các lựa chọn được thực hiện trên mỗi tọa độ. Giả sử một tọa độ cụ thể được chọn chính xác`k`lần trong toàn bộ quá trình. Nếu như`k = 0`, có một hành vi có thể xảy ra: tọa độ không bao giờ được chạm vào. Nếu như`k > 0`, có chính xác hai hành vi có thể xảy ra: tất cả các bước đi đã chọn đều đi theo hướng tích cực hoặc tất cả các bước đi đã chọn đều đi theo hướng tiêu cực. 

Vấn đề còn lại là các bước được sắp xếp trên toàn cầu. Nếu phối hợp`i`được chọn`c_i`lần, vị trí của những bước đó trong số tất cả`t`khoảnh khắc có thể được chọn trong:$$\frac{t!}{c_1!c_2!\cdots c_d!}$$cách. 

Điều này dẫn đến một hàm tạo hàm mũ. Đối với một chiều, đóng góp là:$$1 + 2\frac{x}{1!} + 2\frac{x^2}{2!} + 2\frac{x^3}{3!}+\cdots$$đó là:$$1+2(e^x-1)=2e^x-1$$Vì`d`thứ nguyên thì hệ số chúng ta cần là hệ số của`x^t`TRONG:$$(2e^x-1)^d$$Định lý nhị thức biến giá trị này thành tổng có thể được tính trực tiếp:$$(2e^x-1)^d
=
\sum_{i=0}^{d}
\binom di 2^i(-1)^{d-i}e^{ix}$$Hệ số của`x^t`TRONG`e^{ix}`là`i^t/t!`. Công thức tính ban đầu nhân với`t!`, do đó giai thừa biến mất:$$\text{answer}
=
\sum_{i=0}^{d}
(-1)^{d-i}\binom di2^ii^t$$Bây giờ vấn đề được giảm xuống còn một vòng lặp trên các kích thước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2d)^t) | O(t) | Quá chậm | 
| Tối ưu | O(d) | O(d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến`d`. Công thức cần các hệ số nhị thức và việc tính toán từng hệ số một cách độc lập sẽ quá chậm. 
2. Lặp lại mọi giá trị có thể`i`từ`0`ĐẾN`d`. giá trị`i`biểu thị có bao nhiêu thứ nguyên được chọn để đóng góp số mũ khi mở rộng`(2e^x-1)^d`. 
3. Đối với mỗi`i`, tính:$$(-1)^{d-i}\binom di2^ii^t$$và thêm nó vào câu trả lời. Hệ số nhị thức chọn kích thước nào đóng góp`2e^x`, trong khi dấu và lũy thừa đến từ phần còn lại`-1`số hạng và hệ số mũ. 

1. Chuẩn hóa modulo kết quả cuối cùng`998244353`và in nó. 

Tại sao nó hoạt động: 

Hàm tạo mô hình hóa mọi chiều một cách độc lập. Hệ số của`x^t/t!`ghi lại cách phân phối`t`các khoảnh khắc được sắp xếp giữa các kích thước, trong khi hệ số của`2`đối với mỗi kích thước được sử dụng, hãy ghi lại hai dấu hiệu có thể có. Nâng hàm sinh một chiều lên`d`sức mạnh thứ kết hợp tất cả các chiều. Việc khai triển nhị thức không làm thay đổi đối tượng được đếm, nó chỉ đưa ra cách nhanh hơn để trích xuất hệ số cần thiết. Vì mọi lịch sử có thể đều tương ứng với chính xác một số hạng trong bản khai triển này và mỗi số hạng biểu thị các lịch sử hợp lệ nên công thức tính mỗi câu trả lời chính xác một lần. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

MOD = 998244353

def solve():
    d, t = map(int, input().split())

    fact = [1] * (d + 1)
    for i in range(1, d + 1):
        fact[i] = fact[i - 1] * i % MOD

    inv_fact = [1] * (d + 1)
    inv_fact[d] = pow(fact[d], MOD - 2, MOD)
    for i in range(d, 0, -1):
        inv_fact[i - 1] = inv_fact[i] * i % MOD

    ans = 0
    pow_two = 1

    for i in range(d + 1):
        comb = fact[d] * inv_fact[i] % MOD * inv_fact[d - i] % MOD
        term = comb * pow_two % MOD
        term = term * pow(i, t, MOD) % MOD

        if (d - i) & 1:
            ans -= term
        else:
            ans += term

        ans %= MOD
        pow_two = pow_two * 2 % MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Mảng giai thừa cho phép tính toán mọi hệ số nhị thức trong thời gian không đổi.`inv_fact`được xây dựng bằng định lý nhỏ Fermat vì môđun là số nguyên tố. 

Vòng lặp tuân theo công thức trực tiếp.`pow_two`cửa hàng`2^i`tăng dần, tránh tính lũy thừa lặp lại. giá trị`pow(i, t, MOD)`xử lý số mũ lớn một cách hiệu quả bằng cách sử dụng lũy ​​thừa nhị phân mô-đun. 

Vụ án`i = 0`được xử lý một cách tự nhiên. Vì những hạn chế có`t >= 1`,`0^t`là`0`, vì vậy thuật ngữ đó không đóng góp gì cả. Không cần phải có trường hợp đặc biệt. 

Tất cả các phép cộng đều được giảm modulo`MOD`sau mỗi lần lặp. Số nguyên Python không bị tràn, nhưng việc giữ giá trị ở mức giảm sẽ tránh sự tăng trưởng không cần thiết và giữ cho việc triển khai gần với công thức toán học. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
```Vòng lặp xem xét việc chọn không, một hoặc hai chiều khi khai triển. 

| tôi | C(d, i) | 2^i | nó ^ t | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 0 | 
| 1 | 2 | 2 | 1 | 4 | 
| 2 | 1 | 4 | 2 | 8 | 

Các dấu hiệu được áp dụng bằng cách sử dụng`(-1)^(d-i)`: 

| tôi | Đóng góp đã ký | 
| --- | --- | 
| 0 | 0 | 
| 1 | -4 | 
| 2 | 4 | 

Kết quả là`4`. 

Điều này xác nhận rằng một bước có thể chọn một trong hai trục và một trong hai hướng. 

### Mẫu 2 

đầu vào:```
3 3
```| tôi | C(3, i) | 2^i | tôi^3 | Đóng góp đã ký | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 0 | 
| 1 | 3 | 2 | 1 | 6 | 
| 2 | 3 | 4 | 8 | -96 | 
| 3 | 1 | 8 | 27 | 216 | 

Bổ sung các điều khoản:$$0+6-96+216=126$$Câu trả lời là`126`. 

Điều này chứng tỏ rằng công thức tính lịch sử, bao gồm các thứ tự chọn tọa độ khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(d) | Giải pháp tính giai thừa một lần và thực hiện một vòng lặp trên tất cả các chiều. | 
| Không gian | O(d) | Hai mảng có chiều dài`d + 1`lưu trữ các giá trị giai thừa và nghịch đảo. | 

Với`d <= 200000`, độ phức tạp tuyến tính dễ dàng phù hợp với các giới hạn. Các phép tính lũy thừa bên trong vòng lặp mất thời gian logarit trong`t`, cũng bị giới hạn bởi`200000`. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    d, t = map(int, input().split())

    fact = [1] * (d + 1)
    for i in range(1, d + 1):
        fact[i] = fact[i - 1] * i % MOD

    inv_fact = [1] * (d + 1)
    inv_fact[d] = pow(fact[d], MOD - 2, MOD)
    for i in range(d, 0, -1):
        inv_fact[i - 1] = inv_fact[i] * i % MOD

    ans = 0
    pow_two = 1

    for i in range(d + 1):
        comb = fact[d] * inv_fact[i] % MOD * inv_fact[d - i] % MOD
        term = comb * pow_two % MOD * pow(i, t, MOD) % MOD
        if (d - i) & 1:
            ans -= term
        else:
            ans += term
        ans %= MOD
        pow_two = pow_two * 2 % MOD

    return str(ans % MOD)

assert solve("2 1\n") == "4", "sample 1"
assert solve("3 3\n") == "126", "sample 2"

assert solve("1 1\n") == "2", "single dimension"
assert solve("1 5\n") == "2", "one axis always has two directions"
assert solve("2 2\n") == "12", "two dimensions, two steps"
assert solve("3 1\n") == "6", "one step in three dimensions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`2`| Kích thước và số bước nhỏ nhất. | 
|`1 5`|`2`| Chuyển động lặp đi lặp lại trên một tọa độ không có thêm lựa chọn nào sau dấu hiệu đầu tiên. | 
|`2 2`|`12`| Các thứ tự tọa độ khác nhau phải được tính riêng. | 
|`3 1`|`6`| Tất cả các bước đi đầu tiên có thể được tính. | 

## Vỏ cạnh 

Dành cho:```
1 1
```tọa độ duy nhất được chọn một lần. Hoa có thể di chuyển đến`1`hoặc`-1`, vậy câu trả lời là`2`. Trong công thức chỉ có`i = 1`thời hạn tồn tại, cho`2`. 

Vì:```
2 1
```có bốn bước di chuyển đầu tiên có thể xảy ra: tích cực hoặc tiêu cực dọc theo một trong hai chiều. Thuật toán tính toán:$$(-1)^2\binom20 0^1
+
(-1)^1\binom21 2(1^1)
+
(-1)^0\binom22 4(2^1)$$trở thành:$$-4+8=4$$Vì:```
3 3
```tọa độ được chọn ba lần không tạo ra tám khả năng. Nước đi đầu tiên chọn dấu của nó, và hai nước tiếp theo bị ép buộc. Hàm tạo sử dụng hệ số`2x^3/3!`, chỉ đóng góp chính xác hai lựa chọn dấu cho tọa độ đó. 

Đối với một đầu vào lớn như:```
12345 67890
```thuật toán không bao giờ xây dựng đường dẫn hoặc trạng thái. Nó chỉ thực hiện tuyến tính vượt qua các kích thước, đó là lý do tại sao nó vẫn khả thi mặc dù có rất nhiều lịch sử tăng trưởng có thể có.
