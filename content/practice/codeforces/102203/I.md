---
title: "CF 102203I - \u0412\u043e\u0441\u043f\u043e\u043c\u0438\u043d\u0430\u043d\u0438\u0435"
description: "Chúng ta bắt đầu với biểu thức n × n. Trong một lần lặp, mỗi lần xuất hiện của mẫu chính xác này được thay thế bằng n bản sao của số n. Các bản sao này được ghép từ trái sang phải, có dấu × được chèn vào bên trong mỗi cặp."
date: "2026-08-18T00:49:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "I"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 99
verified: true
draft: false
---

[CF 102203I - \u0412\u043e\u0441\u043f\u043e\u043c\u0438\u043d\u0430\u043d\u0438\u0435](https://codeforces.com/problemset/problem/102203/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với biểu thức`n × n`. Trong một lần lặp, mọi lần xuất hiện của mẫu chính xác này đều được thay thế bằng`n`bản sao của số`n`. Các bản sao này được ghép nối từ trái sang phải, với`×`chèn vào bên trong mỗi cặp. Giữa các cặp liên tiếp và giữa cặp cuối cùng và trận chung kết không có cặp`n`khi`n`là số lẻ, ký hiệu nhân thông thường`·`được chèn vào. 

Hoạt động được lặp lại`k`lần. Cuối cùng, mọi thứ còn lại`×`cũng được đổi thành`·`, do đó sự khác biệt giữa hai ký hiệu nhân biến mất. Nhiệm vụ là đếm tất cả các ký hiệu nhân trong biểu thức cuối cùng, modulo`998244353`. 

Các giá trị là`n <= 10^9`Và`k <= 10^6`. Ngay cả một thuật toán tuyến tính trong`k`chỉ thực hiện khoảng một triệu lần lặp, điều này hoàn toàn hợp lý. Điều không thể là tự xây dựng biểu thức, vì kích thước của nó tăng theo cấp số nhân với số lần lặp. Từ`n`có thể là một tỷ, thậm chí một lần mở rộng duy nhất cũng có thể tạo ra một biểu thức to lớn và đối với số lượng lớn`k`kích thước của nó vượt xa mọi bộ nhớ thực tế hoặc giới hạn thời gian. 

Có một số trường hợp nhỏ mà việc tái phát trực tiếp có thể dễ dàng dẫn đến sai sót. Vì`n = 1, k = 1`, ban đầu`1 × 1`trở nên công bằng`1`, vậy câu trả lời là`0`. Một phép lặp giả định một cách mù quáng mỗi lần lặp tạo ra các ký hiệu nhân mới sẽ giữ sai ký hiệu ban đầu. Vì`n = 2`, biểu thức`2 × 2`biến thành`(2 × 2)`, do đó số ký hiệu nhân vẫn bằng`1`mãi mãi. Đây là trường hợp đặc biệt khi tỷ lệ hình học trở thành`1`. Cuối cùng, khi`k = 0`, không có sự biến đổi nào xảy ra cả, vì vậy với mọi`n`câu trả lời là chính xác`1`. Ví dụ,`1 0`có câu trả lời`1`, mặc dù phép biến đổi đầu tiên sẽ loại bỏ hoàn toàn phép nhân. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là xây dựng biểu thức sau mỗi lần lặp. Nó đúng vì nó tuân theo định nghĩa theo nghĩa đen, thay thế từng dòng điện`n × n`và chèn các dấu ngoặc đơn và ký hiệu nhân theo yêu cầu. Vấn đề là lượng dữ liệu liên quan. Cho phép`q = floor(n / 2)`. Mỗi lần xuất hiện của`×`sản xuất chính xác`q`sự xuất hiện mới của`×`ở lần lặp tiếp theo, bởi vì`n`dạng số mới`q`cặp hoàn chỉnh. Bắt đầu từ một`×`, sau đó`t`lặp đi lặp lại có`q^t`những biểu tượng như vậy. Vì vậy, một giải pháp dựa trên xây dựng cần ít nhất`Theta(q^k)`chỉ hoạt động để thể hiện các ký hiệu liên quan. Đối với các giá trị như`n = 10^9`Và`k = 10^6`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không bao giờ cần biểu thức thực tế. Chúng ta chỉ cần biết nó chứa bao nhiêu ký hiệu nhân. Giả sử có`x_t`sự xuất hiện của`×`sau đó`t`lần lặp lại. Mỗi một trong số chúng độc lập trở thành một cấu trúc chứa`floor(n / 2)`mới`×`biểu tượng. Kể từ đây`x_t = floor(n / 2)^t`. 

Bây giờ hãy xem xét tổng số`a_t`của các ký hiệu nhân, đếm cả hai`×`Và`·`, sau đó`t`lần lặp lại. Một cái cũ`×`được thay thế bằng một biểu thức có chứa`n - 1`các ký hiệu nhân. Trước khi thay thế vị trí đó chứa một ký hiệu, do đó tổng số tăng chính xác`n - 2`. 

Tại lần lặp`t`, có`x_t`sự cố có sẵn để thay thế. Do đó,`a_{t+1} = a_t + (n - 2) x_t`với`a_0 = 1`. 

Thay thế`x_t = q^t`cho`a_k = 1 + (n - 2)(1 + q + q^2 + ... + q^(k-1))`. 

Sự thay thế cuối cùng của`×`qua`·`không thay đổi số lượng ký hiệu, vì vậy`a_k`chính xác là câu trả lời được yêu cầu. 

Do đó chúng ta chỉ cần một tiến trình hình học. Từ`q <= 5 * 10^8`, nó nhỏ hơn mô đun`998244353`. Nếu như`q != 1`, chúng ta có thể sử dụng`1 + q + ... + q^(k-1) = (q^k - 1) / (q - 1)`, 

sử dụng nghịch đảo mô-đun. Trường hợp duy nhất mà mẫu số bằng 0 modulo mô đun là`q = 1`, có nghĩa là`n = 2`. Trong trường hợp đó tổng chỉ đơn giản là`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`Theta(floor(n/2)^k)`|`Theta(floor(n/2)^k)`| Quá chậm | 
| Tối ưu |`O(log k)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`q = n // 2`. Đây là số lượng mới`×`biểu tượng được tạo ra bởi một người cũ`×`trong lần lặp tiếp theo, bởi vì`n`số chứa chính xác`floor(n / 2)`cặp hoàn chỉnh. 
2. Quan sát rằng sau khi lặp lại`t`, số lượng`×`biểu tượng là`q^t`. Biểu thức ban đầu chứa một ký hiệu như vậy, cho`x_0 = 1`và mỗi lần lặp lại nhân số này với`q`. 
3. Theo dõi tổng số ký hiệu nhân thay vì biểu thức. Thay thế một`×`với`n`những con số tạo ra`n - 1`ký hiệu nhân bên trong biểu thức kết quả. Vì lần xuất hiện cũ đóng góp một biểu tượng nên mức tăng ròng là`n - 2`. 
4. Tính tổng mức tăng này trên tất cả`k`lần lặp lại. Câu trả lời là`1 + (n - 2) * S`,

Ở đâu`S = 1 + q + q^2 + ... + q^(k-1)`. 
5. Nếu`k = 0`, sau đó`S = 0`, vậy câu trả lời là`1`. Công thức hình học cũng xử lý việc này một cách tự nhiên. 
6. Nếu`q = 1`, sử dụng`S = k`, bởi vì mọi số hạng của tiến trình đều là`1`. Điều này xảy ra chính xác khi`n = 2`. 
7. Ngược lại thì tính`S = (q^k - 1) * inverse(q - 1) mod MOD`. của Python`pow(q, k, MOD)`tính toán công suất mô đun một cách hiệu quả và định lý Fermat cho kết quả nghịch đảo là`pow(q - 1, MOD - 2, MOD)`. 

### Tại sao nó hoạt động 

Bất biến trung tâm là sau`t`lặp đi lặp lại, chính xác`q^t`sự xuất hiện của`×`hiện hữu. Mỗi lần xuất hiện như vậy được biến đổi độc lập và mỗi lần xuất hiện đều tạo ra chính xác`q`mới`×`biểu tượng. Đối với tổng số ký hiệu, mỗi lần xuất hiện được chuyển đổi sẽ thay đổi từ một ký hiệu nhân sang`n - 1`các ký hiệu nhân, do đó nó góp phần làm tăng ròng`n - 2`. Tổng hợp sự đóng góp này`1, q, ..., q^(k-1)`số lần xuất hiện cho chính xác số ký hiệu nhân cuối cùng. Chuyển đổi cuối cùng của`×`ĐẾN`·`chỉ thay đổi ký tự chứ không thay đổi số lượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, k = map(int, input().split())

    q = n // 2

    if q == 1:
        geometric_sum = k % MOD
    else:
        geometric_sum = (pow(q, k, MOD) - 1) % MOD
        geometric_sum = geometric_sum * pow(q - 1, MOD - 2, MOD) % MOD

    answer = (1 + (n - 2) * geometric_sum) % MOD
    print(answer)

if __name__ == "__main__":
    solve()
```Chương trình đầu tiên tính toán`q`, số cặp hoàn chỉnh được hình thành từ`n`bản sao được tạo ra bởi một sự thay thế. Biến`geometric_sum`đại diện cho số lượng chuyển đổi`×`số lần xuất hiện được tích lũy qua tất cả các lần lặp. 

các`q == 1`chi nhánh là cần thiết bởi vì`q - 1`sẽ bằng 0, do đó nghịch đảo môđun không tồn tại. Vì`n = 2`, sự tiến triển chỉ đơn giản là`1 + 1 + ... + 1`. 

Dành cho nhau`n`, công thức cấp số nhân là hợp lệ. Ba đối số của Python`pow`thực hiện phép lũy thừa mô-đun mà không cần xây dựng số nguyên khổng lồ`q^k`. 

biểu hiện`(n - 2) * geometric_sum`có thể rất lớn như một số nguyên thông thường, nhưng Python xử lý các số nguyên có kích thước tùy ý và phép toán modulo cuối cùng sẽ cho kết quả cần thiết. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, phép nhân phải được thực hiện với kiểu đủ rộng hoặc mô đun giảm`MOD`Đầu tiên. 

Công thức cũng xử lý`k = 0`. Trong trường hợp đó`pow(q, 0, MOD)`là`1`, làm cho tổng hình học bằng 0, do đó câu trả lời trở thành`1`, khớp chính xác với biểu thức ban đầu không thay đổi. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`n = 5`Và`k = 0`. 

| Bước |`n`|`q`|`k`| Tổng hình học | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 5 | 2 | 0 | 0 | 1 | 

Không có sự lặp lại nào được thực hiện. Biểu thức ban đầu là`5 × 5`, chứa đúng một ký hiệu nhân. 

Đối với mẫu thứ hai,`n = 5`Và`k = 1`. 

| Bước |`q`|`x_t`| Đã thêm biểu tượng | Tổng cộng | 
| --- | --- | --- | --- | --- | 
|`t = 0`| 2 | 1 |`1 * (5 - 2) = 3`| 4 | 

Sự biến đổi đầu tiên quay`5 × 5`vào trong`(5 × 5) · (5 × 5) · 5`. Có bốn ký hiệu nhân, hai`×`biểu tượng và hai`·`biểu tượng. Sự tái diễn coi đây là sự gia tăng của`3`, từ một ký hiệu đến bốn. 

Đối với mẫu thứ ba,`n = 5`Và`k = 2`. 

| Bước |`q`|`x_t`| Đã thêm biểu tượng | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 1 | 0 | 1 | 
| Lặp lại 1 | 2 | 1 | 3 | 4 | 
| Lặp lại 2 | 2 | 2 | 6 | 10 | 

hai`×`các ký hiệu được tạo ra bởi lần lặp đầu tiên đều được mở rộng trong lần lặp thứ hai. Mỗi thứ đều đóng góp vào sự gia tăng ròng của`3`, do đó tổng trở thành`10`. 

Tổng hình học là`1 + 2 = 3`, cho`1 + (5 - 2) * 3 = 10`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(log k)`| lũy thừa mô đun và lũy thừa logarit nghịch đảo mô đun | 
| Không gian |`O(1)`| Chỉ một số nguyên không đổi được lưu trữ | 

Lớn nhất có thể`k`là`10^6`, nhưng thuật toán không phụ thuộc tuyến tính vào kích thước biểu thức và không xây dựng bất kỳ phần nào của biểu thức. Số logarit của phép nhân mô-đun dễ dàng nằm trong giới hạn thời gian, trong khi mức sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solve():
    n, k = map(int, input().split())

    q = n // 2

    if q == 1:
        geometric_sum = k % MOD
    else:
        geometric_sum = (pow(q, k, MOD) - 1) % MOD
        geometric_sum = geometric_sum * pow(q - 1, MOD - 2, MOD) % MOD

    answer = (1 + (n - 2) * geometric_sum) % MOD
    print(answer)

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        from contextlib import redirect_stdout
        output = io.StringIO()
        with redirect_stdout(output):
            solve()
        return output.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run("5 0\n") == "1", "sample 1"
assert run("5 1\n") == "4", "sample 2"
assert run("5 2\n") == "10", "sample 3"

# Minimum n, no iterations
assert run("1 0\n") == "1", "initial 1 × 1"

# Minimum n, one iteration removes the only multiplication
assert run("1 1\n") == "0", "n = 1"

# n = 2 is the geometric-ratio boundary case
assert run("2 1000000\n") == "1", "q = 1"

# Even n, checking the pairing behavior
assert run("4 1\n") == "3", "n = 4, k = 1"

# Maximum-size input, verifies that no expression is constructed
def reference(n, k):
    q = n // 2
    if q == 1:
        s = k % MOD
    else:
        s = (pow(q, k, MOD) - 1) % MOD
        s = s * pow(q - 1, MOD - 2, MOD) % MOD
    return (1 + (n - 2) * s) % MOD

assert run("1000000000 1000000\n") == str(
    reference(1000000000, 1000000)
), "maximum constraints"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`1`| tối thiểu`n`, không lặp lại | 
|`1 1`|`0`| Hành vi đặc biệt khi`n = 1`| 
|`2 1000000`|`1`| các`q = 1`trường hợp ranh giới | 
|`4 1`|`3`| Thậm chí`n`và ghép nối hoàn chỉnh | 
|`1000000000 1000000`| Kết quả modulo dựa trên công thức | Các ràng buộc tối đa và tránh biểu thức hàm mũ | 

## Vỏ cạnh 

cho`n = 1, k = 1`, chúng tôi có`q = 0`. Tổng hình học là`1`, bởi vì chỉ có lần lặp đầu tiên hiện diện. Câu trả lời là`1 + (1 - 2) * 1 = 0`. Điều này phù hợp với thực tế rằng`1 × 1`trở thành đơn`1`, không để lại dấu nhân. 

Vì`n = 2`, chúng tôi có`q = 1`. Mỗi lần xuất hiện của`2 × 2`được thay thế bằng dấu ngoặc đơn khác`2 × 2`, nên tổng số ký hiệu nhân không bao giờ thay đổi. Công thức sử dụng`S = k`, nhưng hệ số`n - 2`bằng không, cho`1`cho mọi`k`, bao gồm`k = 10^6`. 

Vì`k = 0`, tổng hình học không chứa số hạng và bằng không. Câu trả lời là do đó`1`cho mọi thứ có thể`n`. Đây là kết quả đúng vì ban đầu`n × n`có chính xác một ký hiệu nhân và không có lần lặp nào sửa đổi nó. 

Đối với số lẻ`n`, bản sao cuối cùng của`n`được để bên ngoài các cặp. Điều này không thay đổi số lượng mới`×`những biểu tượng còn sót lại`floor(n / 2)`. Ví dụ, với`n = 5`, một lần xuất hiện trở thành hai`×`biểu tượng và một biểu tượng không ghép đôi`5`. Bình thường`·`các ký hiệu giữa các nhóm kết quả đã được tính đến bởi thực tế là việc thay thế một ký hiệu nhân bằng một biểu thức gồm năm toán hạng sẽ tạo ra tổng cộng bốn ký hiệu nhân. 

Đối với rất lớn`n`Và`k`, biểu thức thực tế rất lớn về mặt thiên văn, nhưng độ truy hồi chỉ phụ thuộc vào`q`, cấp số nhân và mức tăng ròng`n - 2`. Việc triển khai không bao giờ lưu trữ biểu thức hoặc bất kỳ số lượng nào tỷ lệ thuận với kích thước của nó, do đó, cùng một lượng bộ nhớ không đổi xử lý cả đầu vào nhỏ nhất và lớn nhất.
