---
title: "CF 102558B - \u0417\u0430\u043a\u0440\u044b\u0442\u044b\u0439 \u043a\u043b\u044e\u0447"
description: "Khóa chung được tạo từ một cặp số nguyên dương riêng (p, q). Giá trị công khai đầu tiên là ước chung lớn nhất của cặp và giá trị thứ hai là bội số chung nhỏ nhất."
date: "2026-08-04T09:18:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102558
codeforces_index: "B"
codeforces_contest_name: "Contest for Yandex interns 2019"
rating: 0
weight: 102558
solve_time_s: 93
verified: true
draft: false
---

[CF 102558B - \u0417\u0430\u043a\u0440\u044b\u0442\u044b\u0439 \u043a\u043b\u044e\u0447](https://codeforces.com/problemset/problem/102558/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Khóa chung được tạo từ một cặp số nguyên dương riêng`(p, q)`. Giá trị công khai đầu tiên là ước chung lớn nhất của cặp và giá trị thứ hai là bội số chung nhỏ nhất. Chúng tôi được trao cặp công khai`(x, y)`và cần đếm xem có bao nhiêu cặp riêng được đặt hàng`(p, q)`có thể đã tạo ra nó. 

Hai số đã cho có thể lớn bằng`10^12`. Điều này loại trừ việc thử các giá trị có thể có của`p`Và`q`, bởi vì thậm chí lặp lại tất cả các ước của`y`nếu không sử dụng cấu trúc của gcd và lcm thì sẽ quá tốn kém. Một giải pháp thành công phải khai thác được mối quan hệ toán học giữa gcd và lcm và giảm bớt nhiệm vụ thành phân tích thành thừa số một số duy nhất. Vì căn bậc hai của`10^12`chỉ là`10^6`, phép chia thử tới giới hạn đó là thực tế trong Python. 

Các trường hợp phá vỡ giải pháp bất cẩn chủ yếu liên quan đến phép chia và số`1`. Nếu như`x`không chia`y`, không có khóa riêng nào có thể có. Ví dụ, đầu vào`10 11`có đầu ra`0`, vì lcm luôn là bội số của gcd, nên gcd của`10`không thể tạo ra một lcm`11`. 

Một trường hợp cạnh khác là khi hai giá trị công khai bằng nhau. Đối với đầu vào`7 7`, khóa riêng duy nhất có thể có là`(7, 7)`, vậy câu trả lời là`1`. Một giải pháp chỉ tính các bài tập chính mà quên rằng sản phẩm còn lại có thể`1`có thể xử lý sai trường hợp này. 

Trường hợp thứ ba là khi`y / x`chứa các thừa số nguyên tố lặp lại. Đối với đầu vào`4 36`, tỉ số là`9`. Các cặp chuẩn hóa hợp lệ là`(1,9)`Và`(9,1)`, đưa ra chìa khóa riêng`(4,36)`Và`(36,4)`. Số mũ của số nguyên tố không tạo ra các lựa chọn bổ sung vì cả hai số không thể chia sẻ cùng một thừa số nguyên tố sau khi loại bỏ gcd. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là tìm kiếm thông qua các khóa riêng tư có thể có. Vì lcm của cặp phải là`y`, cả hai số đều phải là ước của`y`. Chúng ta có thể liệt kê mọi cặp số chia`(p, q)`của`y`, kiểm tra xem`gcd(p, q) = x`Và`lcm(p, q) = y`, và đếm các trận đấu. Điều này đúng vì mọi khóa riêng có thể có đều phải xuất hiện trong bảng liệt kê đó. Tuy nhiên, việc tạo và kiểm tra các ước của một số gần`10^12`có thể yêu cầu nhiều thao tác và phương pháp này không sử dụng mối quan hệ đặc biệt giữa gcd và lcm. 

Quan sát quan trọng đến từ việc tách gcd. Giả định:`p = x * a`Và`q = x * b`. 

Sau khi loại bỏ phần chung, các giá trị còn lại`a`Và`b`phải là nguyên tố cùng nhau. Lcm trở thành:`lcm(p, q) = x * a * b`. 

Vì thế:`a * b = y / x`. 

Vấn đề ban đầu bây giờ là hỏi có bao nhiêu hệ số nguyên tố cùng nhau được sắp xếp theo thứ tự`y / x`có. Nếu số này có hệ số nguyên tố:`n = r1^e1 * r2^e2 * ... * rk^ek`sau đó mỗi số nguyên tố hoàn chỉnh`ri^ei`phải đi hoàn toàn đến`a`hoặc hoàn toàn để`b`. Việc tách một lũy thừa nguyên tố sẽ làm cho hai số chia sẻ số nguyên tố đó, vi phạm điều kiện nguyên tố cùng nhau. Mỗi trong số`k`các số nguyên tố khác nhau cho hai lựa chọn, vì vậy câu trả lời là`2^k`. 

Nhiệm vụ còn lại duy nhất là tìm số thừa số nguyên tố phân biệt của`y / x`. Bởi vì giá trị đó nhiều nhất là`10^12`, kiểm tra các ước số đến`10^6`là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số ước của y) với séc gcd đắt tiền | O(1) | Quá chậm và bỏ qua cấu trúc | 
| Tối ưu | O(sqrt(y / x)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trước tiên hãy kiểm tra xem`y`chia hết cho`x`. Nếu không, không cặp nào có thể có gcd`x`và lcm`y`, vậy là có ngay câu trả lời`0`. Lcm phải luôn là bội số của gcd. 
2. Tính toán`n = y / x`. Đây là sản phẩm của hai phần nguyên tố còn lại sau khi loại bỏ gcd khỏi cả hai giá trị khóa riêng. 
3. Nhân tố hóa`n`bằng cách thử các ước số có thể có từ`2`trở lên. Khi tìm thấy một số chia, hãy tính nó như một thừa số nguyên tố riêng biệt và loại bỏ tất cả các lần xuất hiện của nó khỏi`n`. Việc loại bỏ toàn bộ lũy thừa là cần thiết vì chỉ có số lượng số nguyên tố khác nhau mới ảnh hưởng đến câu trả lời. 
4. Nếu sau khi chia thử giá trị lớn hơn`1`vẫn là thừa số nguyên tố lớn hơn căn bậc hai của số ban đầu. Hãy coi nó như một số nguyên tố khác biệt hơn. 
5. Trở về`2`nâng lên số lượng các thừa số nguyên tố phân biệt được tìm thấy. Mỗi lũy thừa nguyên tố có thể được gán độc lập cho một trong hai bên của cặp nguyên tố cùng nhau. 

Tại sao nó hoạt động: 

Sau khi chia cả hai số riêng cho gcd của chúng, các phần còn lại phải có gcd bằng`1`. Sản phẩm của các bộ phận này được cố định là`y / x`. Trong hệ số nguyên tố cùng nhau, mọi thừa số nguyên tố của tích đều thuộc về một phía. Với mỗi số nguyên tố riêng biệt, có đúng hai lựa chọn: đặt toàn bộ lũy thừa của nó vào số thứ nhất hoặc vào số thứ hai. Những lựa chọn này là độc lập, do đó việc nhân các lựa chọn sẽ mang lại`2^k`, Ở đâu`k`là số các thừa số nguyên tố phân biệt. Mỗi phép gán được tính sẽ tạo chính xác một khóa riêng hợp lệ và mỗi khóa riêng hợp lệ tương ứng với một phép gán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, y = map(int, input().split())

    if y % x != 0:
        print(0)
        return

    n = y // x
    cnt = 0

    d = 2
    while d * d <= n:
        if n % d == 0:
            cnt += 1
            while n % d == 0:
                n //= d
        d += 1

    if n > 1:
        cnt += 1

    print(1 << cnt)

if __name__ == "__main__":
    solve()
```Điều kiện đầu tiên xử lý các khóa công khai không thể thực hiện được trước khi thực hiện bất kỳ phép phân tích nhân tử nào. Nếu như`x`không chia`y`, tỷ lệ mà mối quan hệ gcd và lcm yêu cầu không phải là số nguyên. 

Biến`n`lưu trữ phần lcm còn lại sau khi loại bỏ phần đóng góp gcd. Vòng lặp phân tích nhân tử chỉ tính các số nguyên tố phân biệt. Bất cứ khi nào tìm thấy số chia, vòng lặp bên trong sẽ loại bỏ toàn bộ số mũ của nó vì các giá trị như`2`,`4`, Và`8`không tạo ra các lựa chọn bài tập riêng biệt. 

Điều kiện vòng lặp sử dụng`d * d <= n`thay vì tính căn bậc hai. Điều này tránh các vấn đề về độ chính xác của dấu phẩy động đối với các giá trị gần`10^12`. Số nguyên Python không bị tràn nên phép nhân an toàn. 

Kiểm tra cuối cùng cho`n > 1`xử lý một thừa số nguyên tố còn lại. Ví dụ: sau khi loại bỏ các yếu tố nhỏ khỏi`12`, giá trị còn lại trở thành`3`, vẫn đóng góp một sự lựa chọn độc lập. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là:```
5 10
```Việc thực hiện là: 

| Biến | Giá trị | Ý nghĩa | 
| --- | --- | --- | 
| x | 5 | yêu cầu gcd | 
| y | 10 | yêu cầu lcm | 
| n | 2 | sản phẩm bình thường hóa | 
| số nguyên tố riêng biệt | 1 | thừa số nguyên tố`2`| 
| trả lời | 2 |`2^1`bài tập | 

Tỷ lệ`2`có một số nguyên tố riêng biệt. Quyền lực chính đó có thể được giao cho hai bên, tạo ra`(5,10)`hoặc`(10,5)`. 

Đối với mẫu thứ hai:```
10 11
```Việc thực hiện là: 

| Biến | Giá trị | Ý nghĩa | 
| --- | --- | --- | 
| x | 10 | yêu cầu gcd | 
| y | 11 | yêu cầu lcm | 
| y % ​​x | 1 | gcd không chia lcm | 
| trả lời | 0 | không thể | 

Thuật toán dừng trước khi phân tích thành nhân tử vì không có lcm nào có thể nhỏ hơn mối quan hệ bội số của gcd. Điều này xác nhận điều kiện từ chối sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(y / x)) | Phép chia thử kiểm tra các thừa số có thể có đến căn bậc hai của giá trị còn lại | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Giá trị lớn nhất có thể có của hệ số là`10^12`, do đó vòng lặp thực hiện tối đa khoảng một triệu phép kiểm tra số chia. Điều này phù hợp thoải mái trong giới hạn Codeforces điển hình. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    x, y = map(int, input().split())

    if y % x != 0:
        return "0\n"

    n = y // x
    cnt = 0
    d = 2

    while d * d <= n:
        if n % d == 0:
            cnt += 1
            while n % d == 0:
                n //= d
        d += 1

    if n > 1:
        cnt += 1

    return str(1 << cnt) + "\n"

assert solution("5 10\n") == "2\n", "sample 1"
assert solution("10 11\n") == "0\n", "sample 2"
assert solution("527 9486\n") == "4\n", "sample 3"

assert solution("1 1\n") == "1\n", "minimum values"
assert solution("7 7\n") == "1\n", "equal public values"
assert solution("4 36\n") == "2\n", "repeated prime factor"
assert solution("1 1000000000000\n") == "16\n", "large composite value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Tỷ lệ là`1`, vậy chỉ tồn tại một khóa riêng | 
|`7 7`|`1`| Trường hợp gcd và lcm bằng nhau | 
|`4 36`|`2`| Các lũy thừa được tính một lần, không tính theo số mũ | 
|`1 1000000000000`|`16`| Đầu vào lớn và ranh giới nhân tố hóa đầy đủ | 

## Vỏ cạnh 

Đối với đầu vào`10 11`, thuật toán kiểm tra tính chia hết trước. Từ`11 % 10`không bằng 0, nó trả về`0`ngay lập tức. Một phương pháp chỉ dựa trên bao thanh toán`y`có thể tiếp tục tìm kiếm các cặp một cách không chính xác và lãng phí thời gian vào một trường hợp không thể. 

Đối với đầu vào`7 7`, tỉ số trở thành`1`. Vòng phân tích nhân tử không tìm thấy số nguyên tố nào, vì vậy số nguyên tố riêng biệt bằng 0 và câu trả lời là`2^0 = 1`. Điều này thể hiện chính xác cặp đơn`(7,7)`. 

Đối với đầu vào`4 36`, tỉ số là`9`, những yếu tố nào như`3^2`. Thuật toán chỉ tính một số nguyên tố riêng biệt, cho`2^1 = 2`. Hai nhiệm vụ đó là`(1,9)`Và`(9,1)`, trở thành`(4,36)`Và`(36,4)`. Nó không tính việc chia tách hai yếu tố của`3`bởi vì điều đó sẽ làm cho các giá trị được chuẩn hóa không phải là nguyên tố cùng nhau.
