---
title: "CF 150A - Giành chiến thắng hoặc đóng băng"
description: "Chúng ta bắt đầu với một số q được viết trên giấy. Trong mỗi lượt, người chơi phải thay thế số hiện tại bằng một trong các ước số không tầm thường của nó, nghĩa là một số chia hoàn toàn nằm giữa 1 và chính số đó. Nếu một người chơi không thể di chuyển, người chơi đó sẽ thắng ngay lập tức."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "games", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 150
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 107 (Div. 1)"
rating: 1400
weight: 150
solve_time_s: 122
verified: false
draft: false
---

[CF 150A - Thắng hoặc Đóng băng](https://codeforces.com/problemset/problem/150/A) 

**Đánh giá:** 1400 
**Tags:** trò chơi, toán học, lý thuyết số 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một con số`q`viết trên giấy. Trong mỗi lượt, người chơi phải thay thế số hiện tại bằng một trong các ước số không tầm thường của nó, nghĩa là một số chia hoàn toàn nằm giữa`1`và chính con số đó. 

Nếu một người chơi không thể di chuyển, người chơi đó sẽ thắng ngay lập tức. Điều này trái ngược với quy ước chơi trò chơi thông thường, trong đó người chơi không thể di chuyển sẽ thua cuộc. 

Nhiệm vụ là xác định xem người chơi thứ nhất hay thứ hai sẽ thắng khi cả hai đều chơi tối ưu. Nếu người chơi đầu tiên có chiến lược chiến thắng, chúng ta cũng cần in một nước đi đầu tiên hợp lệ. 

Kích thước đầu vào có số lượng nhỏ, chỉ có một số nguyên, nhưng bản thân giá trị có thể lớn bằng`10^13`. Điều đó loại trừ bất cứ điều gì cố gắng liệt kê tất cả các ước số một cách liên tục hoặc thực hiện việc khám phá trạng thái trò chơi đệ quy đắt tiền. Hệ số căn bậc hai lên tới gần đúng`3 * 10^6`hoàn toàn có thể chấp nhận được trong vòng 2 giây, vì`sqrt(10^13)`là về`3.16 * 10^6`. 

Phần khó khăn không phải là hiệu suất, mà là hiểu chính xác logic trò chơi vì điều kiện chiến thắng bất thường. 

Một sai lầm rất phổ biến là cho rằng “không đi được là thua”. Điều đó tạo ra câu trả lời hoàn toàn ngược lại trong nhiều trường hợp. 

Hãy xem xét: 

đầu vào:```
2
```số`2`không có ước số không tầm thường. Người chơi đầu tiên không thể di chuyển nên người chơi đầu tiên sẽ thắng ngay lập tức. Đầu ra đúng là:```
1
0
```Việc triển khai bất cẩn bằng cách sử dụng trực giác Sprague-Grundy tiêu chuẩn sẽ in sai trình phát`2`. 

Một trường hợp tinh tế khác là khi số đó là tích của đúng hai số nguyên tố. 

đầu vào:```
6
```Những động thái hợp pháp duy nhất là`2`Và`3`. Cả hai đều là số nguyên tố nên người chơi tiếp theo không được di chuyển và giành chiến thắng ngay lập tức. Điều đó có nghĩa là chuyển sang`2`hoặc`3`thua cho người chơi hiện tại. Mọi động thái có thể từ`6`là xấu, vì vậy`6`bản thân nó đang thua cuộc. 

Đầu ra đúng là:```
2
```Một giải pháp đệ quy ngây thơ mà không đảo ngược cẩn thận điều kiện cuối thường sẽ dẫn đến kết quả ngược lại. 

Ngoài ra còn có một trường hợp cạnh cấu trúc xung quanh các lũy thừa chính. 

đầu vào:```
8
```Những động thái hợp pháp là`2`Và`4`. 

Nếu chúng ta chuyển đến`2`, đối phương không thể di chuyển và thắng ngay nên nước đi đó coi như thua. 

Nếu chúng ta chuyển đến`4`, đối thủ phải di chuyển đến`2`, và khi đó chúng ta không thể di chuyển và giành chiến thắng. 

Vì thế`8`đang thắng và một nước đi đầu tiên hợp lệ là`4`. 

Trường hợp này cho thấy trò chơi không chỉ được xác định bởi số hiện tại có phải là hợp số hay không. Cấu trúc nhân tố hóa có vấn đề. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ coi mọi số có thể truy cập được là trạng thái trò chơi và xác định đệ quy xem số đó thắng hay thua. Đối với một số`x`, chúng ta sẽ liệt kê tất cả các ước số không tầm thường và phân tích chúng một cách đệ quy. 

Bản thân việc đệ quy có thể quản lý được vì mỗi lần di chuyển đều làm giảm số lượng một cách nghiêm ngặt, nhưng phần tốn kém là phép liệt kê số chia. Trong trường hợp xấu nhất chúng tôi sẽ quét tới`sqrt(x)`cho nhiều tiểu bang nhiều lần. Với`x`lớn như`10^13`, điều đó nhanh chóng trở nên lãng phí. 

Vấn đề sâu xa hơn là chúng ta thực sự không cần tìm kiếm toàn bộ trò chơi. Trò chơi có cấu trúc rất cứng nhắc vì mỗi nước đi đều thay thế số bằng một ước số thích hợp. 

Chúng ta hãy phân loại các trạng thái một cách cẩn thận. 

Một số nguyên tố không có động thái hợp pháp. Vì người chơi không thể di chuyển sẽ thắng nên mọi số nguyên tố đều là trạng thái chiến thắng. 

Bây giờ hãy nhìn vào một con số có chính xác một nước đi có thể xảy ra, chẳng hạn như`p^2`. 

Ví dụ:```
9 -> 3
```Nước đi đi đến điểm cao nhất, chiến thắng thuộc về người chơi tiếp theo. Vì thế`9`đang thua. 

Bây giờ hãy nhìn vào`p^3`. 

Ví dụ:```
27 -> 9` or `3
```Di chuyển trực tiếp đến`3`là xấu vì đối thủ ngay lập tức thắng. Nhưng chuyển sang`9`là tốt bởi vì`9`đang thua. Vì thế`27`đang chiến thắng. 

Hành vi xen kẽ này tiếp tục đối với các lũy thừa nguyên tố thuần túy. 

Đối với các số có ít nhất hai thừa số nguyên tố riêng biệt, tình huống trở nên đơn giản hơn nhiều. Chúng ta hầu như luôn có thể chuyển sang trạng thái thua cuộc ngay lập tức. 

Quan sát quan trọng là các trạng thái thua duy nhất là các số có tổng số thừa số nguyên tố chẵn được tính bằng bội số. 

Cho phép:```
n = p1 * p2 * ... * pk
```trong đó các số nguyên tố lặp lại được tính liên tục. Định nghĩa:```
Ω(n) = total count of prime factors with multiplicity
```Ví dụ:```
Ω(12) = Ω(2^2 * 3) = 3
Ω(18) = Ω(2 * 3^2) = 3
Ω(36) = Ω(2^2 * 3^2) = 4
```Mỗi nước đi đều loại bỏ ít nhất một thừa số nguyên tố, do đó mỗi nước đi đều giảm đi một cách nghiêm ngặt`Ω(n)`. 

Một số nguyên tố có`Ω = 1`và đang chiến thắng. 

Một số với`Ω = 2`đang thua vì mọi nước đi đều đạt đến số nguyên tố. 

Một số với`Ω = 3`đang chiến thắng vì chúng ta có thể chuyển đến một trạng thái có`Ω = 2`. 

Bằng quy nạp: 

- kỳ quặc`Ω(n)`có nghĩa là chiến thắng 
- thậm chí`Ω(n)`có nghĩa là thua 

Toàn bộ vấn đề quy về việc phân tích số đó và đếm các bội số nguyên tố. 

Nếu số chẵn thì người chơi`2`thắng. 

Nếu số đếm là số lẻ: 

- nếu không có động thái hợp pháp nào cả, hãy in`0`- mặt khác in bất kỳ ước số nào để lại số thừa số chẵn 

Cách dễ nhất là chia cho một thừa số nguyên tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lượng trạng thái có thể truy cập | O(độ sâu) | Quá chậm | 
| Tối ưu | O(sqrt(q)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích số`q`sử dụng phép chia thử lên đến`sqrt(q)`. 

Với mỗi ước số`d`, chia nhiều lần`q`qua`d`trong khi có thể và đếm xem có bao nhiêu thừa số nguyên tố đã bị loại bỏ. 
2. Nếu sau khi chia thử giá trị còn lại lớn hơn`1`, nó là thừa số nguyên tố lớn hơn`sqrt(original q)`, vì vậy hãy tăng số lượng hệ số lên một lần nữa. 
3. Hãy để`cnt`là tổng số các thừa số nguyên tố được tính theo bội số. 

Ví dụ:```
12 = 2 * 2 * 3 -> cnt = 3
18 = 2 * 3 * 3 -> cnt = 3
36 = 2 * 2 * 3 * 3 -> cnt = 4
```4. Nếu`cnt`chẵn, in:```
2
```Tình trạng hiện tại đang thua đối với người chơi đầu tiên. 
5. Nếu không thì người chơi đầu tiên sẽ thắng. 

In:```
1
```6. Tìm bất kỳ ước số không tầm thường nào để lại số chẵn các thừa số nguyên tố. 

Chia cho một thừa số nguyên tố luôn làm giảm số đếm đi đúng một, thay đổi tính chẵn lẻ từ số lẻ thành số chẵn. 
7. Nếu số ban đầu là số nguyên tố thì không có nước đi hợp pháp. In:```
0
```8. Nếu không thì in:```
q / p
```Ở đâu`p`là thừa số nguyên tố bất kỳ của`q`. 

### Tại sao nó hoạt động 

Bất biến là tính chẵn lẻ của tổng số thừa số nguyên tố. 

Mỗi bước đi đều thay thế`n`bằng một số chia thích hợp. Điều đó có nghĩa là ít nhất một thừa số nguyên tố biến mất, nên giá trị của`Ω(n)`giảm nghiêm ngặt. 

Một động thái làm thay đổi tính chẵn lẻ của`Ω(n)`bởi vì việc loại bỏ bất kỳ số dương nào của các thừa số nguyên tố sẽ làm lật tính chẵn lẻ khi và chỉ khi số đó là số lẻ. Vì chúng ta luôn có thể loại bỏ chính xác một thừa số nguyên tố bằng cách chia cho một ước số nguyên tố, nên mọi trạng thái lẻ đều chuyển sang trạng thái chẵn. 

Các số nguyên tố có`Ω = 1`và đang thắng vì người chơi hiện tại không thể di chuyển. 

Sử dụng cảm ứng trên`Ω(n)`: 

- tất cả số lẻ đều thắng 
- tất cả số chẵn đều thua 

Vì vậy tính chẵn lẻ hoàn toàn quyết định kết quả của trò chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    original = n

    cnt = 0
    first_prime = -1

    d = 2
    while d * d <= n:
        while n % d == 0:
            if first_prime == -1:
                first_prime = d
            cnt += 1
            n //= d

        if d == 2:
            d = 3
        else:
            d += 2

    if n > 1:
        if first_prime == -1:
            first_prime = n
        cnt += 1

    if cnt % 2 == 0:
        print(2)
    else:
        print(1)

        if cnt == 1:
            print(0)
        else:
            print(original // first_prime)

solve()
```Phần đầu tiên thực hiện phân tích thành thừa số nguyên tố bằng phép chia thử. Chúng tôi liên tục chia cho từng yếu tố thay vì loại bỏ nó một lần vì trò chơi phụ thuộc vào bội số. Ví dụ,`12 = 2^2 * 3`phải đóng góp`3`tổng các yếu tố, không`2`. 

Biến`first_prime`lưu trữ một ước số nguyên tố của số ban đầu. Sau đó, nếu vị trí thắng, việc chia cho số nguyên tố này sẽ làm giảm số hệ số đi đúng một và đảo ngược tính chẵn lẻ. 

Vòng lặp bỏ qua các số chẵn sau khi kiểm tra`2`, giúp cải thiện hiệu suất một chút. Từ`sqrt(10^13)`chỉ ở xung quanh`3 * 10^6`, việc triển khai này dễ dàng phù hợp với giới hạn thời gian. 

điều kiện`cnt == 1`xác định số nguyên tố. Một số nguyên tố không có ước số không tầm thường hợp pháp, vì vậy mặc dù người chơi đầu tiên thắng ngay lập tức, nhưng kết quả nước đi cần thiết là`0`. 

Một lỗi triển khai phổ biến là quên xử lý hệ số nguyên tố còn lại sau vòng lặp. Ví dụ, khi tính toán`10`, sau khi chia cho`2`chúng tôi còn lại với`5`. Từ`5 * 5 > 5`, vòng lặp dừng lại và chúng ta phải đếm số nguyên tố cuối cùng đó theo cách thủ công. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
```Thừa số nguyên tố:```
6 = 2 * 3
```| Bước | Hiện tại | Đã tìm thấy yếu tố | cnt | 
| --- | --- | --- | --- | 
| Bắt đầu | 6 | - | 0 | 
| Chia cho 2 | 3 | 2 | 1 | 
| Còn lại nguyên tố | 1 | 3 | 2 |`cnt = 2`, tức là chẵn. 

Đầu ra:```
2
```Điều này thể hiện cấu hình mất lõi. Mỗi bước di chuyển từ`6`đạt đến số nguyên tố và số nguyên tố là trạng thái chiến thắng. 

### Ví dụ 2 

đầu vào:```
8
```Thừa số nguyên tố:```
8 = 2 * 2 * 2
```| Bước | Hiện tại | Đã tìm thấy yếu tố | cnt | 
| --- | --- | --- | --- | 
| Bắt đầu | 8 | - | 0 | 
| Chia cho 2 | 4 | 2 | 1 | 
| Chia cho 2 | 2 | 2 | 2 | 
| Chia cho 2 | 1 | 2 | 3 |`cnt = 3`, thật kỳ lạ. 

Chúng tôi lưu trữ`first_prime = 2`. 

Nước đi thắng:```
8 / 2 = 4
```Đầu ra:```
1
4
```Ví dụ này cho thấy việc chuyển sang trạng thái đếm số chẵn đảm bảo chiến thắng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(q)) | Phép chia thử kiểm tra các ước số lên tới sqrt(q) | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Đầu vào tối đa là`10^13`, vậy căn bậc hai lớn nhất mà chúng tôi từng quét là gần như`3.16 * 10^6`. Điều đó đủ nhanh trong Python trong giới hạn 2 giây, đặc biệt với việc tối ưu hóa việc bỏ qua các số chẵn sau`2`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    original = n

    cnt = 0
    first_prime = -1

    d = 2
    while d * d <= n:
        while n % d == 0:
            if first_prime == -1:
                first_prime = d
            cnt += 1
            n //= d

        if d == 2:
            d = 3
        else:
            d += 2

    if n > 1:
        if first_prime == -1:
            first_prime = n
        cnt += 1

    out = []

    if cnt % 2 == 0:
        out.append("2")
    else:
        out.append("1")

        if cnt == 1:
            out.append("0")
        else:
            out.append(str(original // first_prime))

    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("6\n") == "2", "sample 1"

# prime number
assert run("2\n") == "1\n0", "prime case"

# odd multiplicity
assert run("8\n") == "1\n4", "prime power"

# even multiplicity
assert run("36\n") == "2", "even factor count"

# large prime
assert run("9999999967\n") == "1\n0", "large prime"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`1 0`| Số nguyên tố là thắng ngay lập tức | 
|`8`|`1 4`| Số yếu tố lẻ là thắng | 
|`36`|`2`| Số lượng yếu tố chẵn cũng bị mất | 
|`9999999967`|`1 0`| Trường hợp cạnh thừa số nguyên tố lớn | 

## Vỏ cạnh 

Số nguyên tố là trường hợp phản trực giác nhất vì người chơi không đi nước nào sẽ thắng. 

đầu vào:```
2
```Nhân tố hóa:```
2 = 2
```Vì thế:```
cnt = 1
```Tính chẵn lẻ có nghĩa là người chơi đầu tiên thắng. Vì không có ước số không tầm thường nên kết quả di chuyển đúng là:```
0
```Thuật toán xử lý việc này thông qua trường hợp đặc biệt`cnt == 1`. 

Bây giờ hãy xem xét một nửa nguyên tố. 

đầu vào:```
15
```Nhân tố hóa:```
15 = 3 * 5
```Vì thế:```
cnt = 2
```Thậm chí ngang bằng có nghĩa là thua. 

Các bước di chuyển có thể:```
15 -> 3
15 -> 5
```Cả hai mục tiêu đều là số nguyên tố và số nguyên tố là trạng thái chiến thắng vì người chơi tiếp theo không thể di chuyển. Thuật toán in chính xác:```
2
```Bây giờ hãy xem xét một lũy thừa nguyên tố cao hơn. 

đầu vào:```
32
```Nhân tố hóa:```
32 = 2^5
```Vì thế:```
cnt = 5
```Tính chẵn lẻ có nghĩa là chiến thắng. 

Thuật toán chọn:```
32 / 2 = 16
```Hiện nay:```
16 = 2^4
```có số chẵn chẵn và đang thua. 

Đầu ra:```
1
16
```Điều này xác nhận rằng việc loại bỏ chính xác một thừa số nguyên tố luôn lật ngược tính chẵn lẻ của trạng thái trò chơi một cách chính xác.
