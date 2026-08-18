---
title: "CF 102263D - Gặp gỡ Bahosain"
description: "Chúng ta có mảng đầu tiên a có các phần tử được phép sửa đổi và mảng thứ hai b chứa các kích thước bước được phép. Trong một thao tác, chúng ta chọn một phần tử của a và cộng hoặc trừ bất kỳ giá trị nào khỏi b."
date: "2026-08-17T19:53:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102263
codeforces_index: "D"
codeforces_contest_name: "ArabellaCPC 2019"
rating: 0
weight: 102263
solve_time_s: 62
verified: true
draft: false
---

[CF 102263D - Gặp gỡ Bahosain](https://codeforces.com/problemset/problem/102263/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng đầu tiên`a`các phần tử mà chúng tôi được phép sửa đổi và mảng thứ hai`b`chứa kích thước bước cho phép. Trong một thao tác, chúng ta chọn một phần tử của`a`và cộng hoặc trừ bất kỳ giá trị nào từ`b`. Chúng ta có thể lặp lại thao tác này bất kỳ số lần nào và mục tiêu là làm cho mọi phần tử của`a`bình đẳng. 

Câu hỏi quan trọng không phải là chúng ta nên đạt tới giá trị chung nào. Câu hỏi thực sự là liệu các thao tác được phép có thể thay đổi sự khác biệt giữa hai phần tử của`a`bằng chính xác số lượng cần thiết để làm cho sự khác biệt đó bằng không. 

Giả sử ước chung lớn nhất của tất cả các giá trị trong`b`là`g`. Mỗi thao tác thay đổi một phần tử bằng bội số của`g`, bởi vì mỗi`b[j]`chia hết cho`g`. Ngược lại, đẳng thức Bézout cho chúng ta biết rằng mọi bội số nguyên của`g`có thể được xây dựng bằng cách cộng và trừ các phần tử của`b`. Vì vậy, từ bất kỳ giá trị bắt đầu nào`x`, tập hợp các giá trị có thể truy cập được từ nó chính xác là tập hợp các số nguyên đồng dạng với`x`modulo`g`. 

Theo đó, tất cả các yếu tố của`a`có thể bằng nhau khi chúng có cùng modulo dư`g`. 

Các ràng buộc cho phép cả hai độ dài mảng đạt tới`10^6`. Một thuật toán có độ phức tạp bậc hai sẽ yêu cầu khoảng`10^12`hoạt động trong trường hợp xấu nhất, vượt xa những gì giới hạn thời gian lập trình cạnh tranh có thể xử lý. Thậm chí một`O(nm)`cách tiếp cận này bị loại bỏ ngay lập tức. Chúng ta chỉ cần xử lý mỗi giá trị đầu vào một số lần không đổi, đưa ra một`O(n + m)`giải pháp. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai dựa trên trực giác không thành công. Nếu như`m = 1`, bước duy nhất được phép là một số giá trị`b`, vì vậy các giá trị như`1`Và`3`có thể cân bằng khi`b = 2`, vì cả hai đều có cùng modulo dư`2`. Một giải pháp bất cẩn có thể yêu cầu chính sự khác biệt của chúng xuất hiện trong`b`. 

Ví dụ:```
2 1
1 3
2
```Đầu ra đúng là`Yes`. Chúng ta có thể thay đổi`1`ĐẾN`3`bằng cách thêm`2`. 

Một trường hợp tinh tế khác là khi các giá trị trong`b`có ước chung lớn hơn 1 mặc dù không có ước số chung nào`b[j]`là sự khác biệt cần thiết.```
2 2
1 7
6 10
```Đầu ra đúng là`Yes`, bởi vì`gcd(6, 10) = 2`và cả hai giá trị mảng đầu tiên đều là số lẻ. Sự khác biệt`6`bản thân nó có sẵn nên người ta có thể trực tiếp biến đổi`1`vào trong`7`, nhưng ví dụ này cũng minh họa rằng cấu trúc liên quan được tạo ra bởi gcd chung chứ không phải bởi các giá trị bước riêng lẻ. 

Trường hợp ngược lại cũng quan trọng không kém:```
2 2
1 8
6 10
```Đây là đầu ra đúng`No`, bởi vì`1`trong khi đó thật kỳ lạ`8`là chẵn. Mọi thao tác đều thay đổi một số một lượng chẵn, do đó tính chẵn lẻ của nó không bao giờ thay đổi. 

Cuối cùng, khi mảng đầu tiên chỉ chứa một phần tử, bạn có thể làm cho tất cả các phần tử của nó bằng nhau vì không có gì phải thay đổi.```
1 3
100
6 10 15
```Đầu ra đúng là`Yes`. Việc triển khai luôn so sánh các cặp phần tử phải xử lý trường hợp này một cách rõ ràng hoặc cho phép tập so sánh trống một cách tự nhiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng mô phỏng các hoạt động được phép. Bắt đầu từ một giá trị, nó có thể cộng hoặc trừ nhiều lần các giá trị từ`b`và tìm kiếm giá trị có thể truy cập được từ mọi phần tử khác. Cách tiếp cận này đúng về mặt khái niệm vì mọi hoạt động hợp pháp đều được thể hiện trong tìm kiếm nhưng không gian trạng thái là không giới hạn. Chúng ta có thể tiếp tục áp dụng các thao tác mãi mãi và có thể đạt được các giá trị tương tự thông qua nhiều chuỗi khác nhau. 

Một cách tiếp cận bạo lực hạn chế hơn có thể kiểm tra mọi cặp phần tử trong`a`và cố gắng xác định xem sự khác biệt của chúng có thể được biểu diễn dưới dạng kết hợp các giá trị từ`b`. Nếu nó sử dụng mọi giá trị của`b`trong khi xử lý từng cặp, trường hợp xấu nhất là`O(n^2m)`, có thể đạt tới khoảng`10^18`kiểm tra cơ bản cho`n,m = 10^6`. Thậm chí giảm điều này thành một cặp`O(n^2)`kiểm tra vẫn cho về`10^12`hoạt động. 

Brute-force hoạt động vì các hoạt động tạo thành một hệ thống phụ gia, nhưng nó không thành công khi chúng ta cố gắng liệt kê hệ thống đó một cách rõ ràng. Quan sát mở ra vấn đề là hệ cộng tính được tạo ra bởi một số số nguyên được mô tả hoàn toàn bằng gcd của chúng. 

Cho phép```
g = gcd(b[0], b[1], ..., b[m-1]).
```Mỗi bước được phép đều chia hết cho`g`, vậy modulo còn lại`g`không bao giờ thay đổi. Điều này đưa ra ngay một điều kiện cần thiết: mọi`a[i]`phải có cùng số dư theo modulo`g`. 

Điều kiện tương tự cũng là đủ. Từ`g`là gcd của các phần tử của`b`, tồn tại số nguyên`c1, c2, ..., cm`như vậy```
c1*b[0] + c2*b[1] + ... + cm*b[m-1] = g.
```Mỗi hệ số có thể được hiểu là phép cộng hoặc phép trừ lặp đi lặp lại, vì vậy chúng ta có thể xây dựng`g`và do đó bất kỳ bội số nào của`g`. Nếu hai giá trị của`a`có cùng số dư theo modulo`g`, hiệu của chúng chia hết cho`g`, do đó sự khác biệt đó có thể được tạo ra bằng cách sử dụng các thao tác được phép. Chúng ta có thể chuyển đổi mọi phần tử thành cùng một đại diện của lớp dư lượng mục tiêu. 

Điều này làm giảm toàn bộ vấn đề xuống còn một phép tính gcd và một lần vượt qua mảng đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²m) trong tìm kiếm hoạt động trực tiếp | O(n) trở lên | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai mảng. Chúng ta cần các giá trị trong`b`để xác định lớp đồng đẳng nào được bảo toàn và các giá trị trong`a`để kiểm tra xem tất cả các phần tử có thuộc về một lớp như vậy hay không. 
2. Tính gcd của mọi giá trị trong`b`. Bắt đầu với`g = 0`, sau đó cập nhật liên tục`g = gcd(g, b[i])`. Bắt đầu từ con số 0 có hiệu quả vì`gcd(0, x) = x`, vì vậy sau khi xử lý toàn bộ mảng thứ hai,`g`chính xác là gcd của tất cả các kích thước bước được phép. 
3. Lưu trữ phần còn lại của phần tử đầu tiên của`a`modulo`g`. Phần dư này đại diện cho lớp đồng dư duy nhất mà tất cả các phần tử được phép chiếm giữ nếu tồn tại nghiệm. 
4. Quét mọi phần tử còn lại`a[i]`. Nếu như`a[i] % g`khác với phần còn lại đầu tiên, in`No`ngay lập tức. Dư lượng của nó không thể bị thay đổi bởi bất kỳ chuỗi hoạt động pháp lý nào, vì vậy nó không bao giờ có thể ngang bằng với các phần tử khác. 
5. Nếu mọi phần tử đều có cùng modulo dư`g`, in`Yes`. Bất kỳ sự khác biệt giữa hai phần tử đều chia hết cho`g`và mọi bội số của`g`có thể được tạo ra từ các giá trị trong`b`, vì vậy tất cả các phần tử có thể được chuyển đổi thành một giá trị chung. 

### Tại sao nó hoạt động 

Bất biến là phần còn lại của mỗi phần tử mảng đầu tiên modulo`g`. Vì mọi phép toán được phép cộng hoặc trừ một số chia hết cho`g`, số dư này không bao giờ thay đổi. Nếu hai phần tử có số dư khác nhau thì chúng không bao giờ gặp nhau, điều này chứng tỏ điều kiện bác bỏ. 

Nếu tất cả các phần tử có cùng số dư thì hiệu giữa hai phần tử bất kỳ sẽ chia hết cho`g`. Gcd của các bước được phép tạo ra chính xác tất cả các bội số nguyên của`g`, do đó sự khác biệt đó có thể được loại bỏ bằng một chuỗi các phép cộng và phép trừ hợp pháp. Do đó tất cả các phần tử đều thuộc cùng một lớp tương đương có thể truy cập được và câu trả lời là`Yes`. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    g = 0
    for x in b:
        g = gcd(g, x)

    r = a[0] % g

    for x in a[1:]:
        if x % g != r:
            print("No")
            return

    print("Yes")

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tính toán gcd của tất cả các kích thước bước được phép. của Python`math.gcd`xử lý thuật toán Euclide một cách hiệu quả và bắt đầu từ số 0 tránh cần một trường hợp đặc biệt cho phần tử đầu tiên của`b`. 

Phần thứ hai sử dụng`a[0]`như dư lượng tham chiếu. Mọi giá trị khác phải có modulo dư chính xác như nhau`g`. Việc kiểm tra có thể chấm dứt ngay khi xuất hiện một phần tử không khớp vì một yếu tố không khớp cũng đủ khiến câu trả lời là không thể. 

Không có vấn đề tràn số nguyên trong Python. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, bản thân các giá trị đầu vào vừa khít với số nguyên có dấu 32 bit và phép tính gcd không bao giờ cần giá trị lớn hơn toán hạng ban đầu. 

Vụ án`n = 1`được xử lý một cách tự nhiên. Vòng lặp kết thúc`a[1:]`trống, do đó thuật toán in`Yes`. 

Vụ án`m = 1`cũng được xử lý một cách tự nhiên. gcd trở thành giá trị duy nhất trong`b`, đưa ra chính xác điều kiện mong đợi là tất cả các giá trị của mảng đầu tiên phải đồng dư theo modulo với kích thước bước đó. 

## Ví dụ đã hoạt động 

Tuyên bố cung cấp một mẫu và ví dụ thứ hai có thể được xây dựng để chứng minh trường hợp được chấp nhận. 

### Mẫu 1```
5 2
3 6 7 2 5
2 4
```Gcd của các bước được phép là`gcd(2, 4) = 2`. Chúng tôi so sánh mọi giá trị của mảng đầu tiên với phần dư của`3`. 

| Yếu tố | Phần tử modulo g | Dư lượng tham khảo | Kết quả | 
| --- | --- | --- | --- | 
| 3 | 1 | 1 | Tiếp tục | 
| 6 | 0 | 1 | Không khớp | 
| 7 | 1 | 1 | Chưa đạt | 
| 2 | 0 | 1 | Chưa đạt | 
| 5 | 1 | 1 | Chưa đạt | 

Phần tử thứ hai đã có tính chẵn lẻ khác với phần tử thứ nhất. Mọi thao tác được phép đều thay đổi một giá trị bằng một số chẵn, vì vậy`3`không bao giờ có thể được chuyển đổi thành một giá trị có tính chẵn lẻ. Đầu ra là`No`. 

### Mẫu 2 

Hãy xem xét:```
4 3
3 9 15 21
6 10 14
```Các giá trị được phép có gcd`2`. Mọi giá trị của mảng đầu tiên đều là số lẻ. 

| Yếu tố | Phần tử modulo g | Dư lượng tham khảo | Kết quả | 
| --- | --- | --- | --- | 
| 3 | 1 | 1 | Tiếp tục | 
| 9 | 1 | 1 | Tiếp tục | 
| 15 | 1 | 1 | Tiếp tục | 
| 21 | 1 | 1 | Tiếp tục | 

Tất cả các phần tử thuộc về cùng một lớp modulo có thể truy cập`2`, vì vậy đầu ra là`Yes`. Thuật toán không cần xây dựng chuỗi hoạt động thực tế. Đặc tính gcd chứng minh rằng các chuỗi như vậy tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi giá trị trong`b`tham gia vào một phép tính gcd và mọi giá trị trong`a`được kiểm tra một lần. | 
| Không gian | O(n + m) | Việc thực hiện lưu trữ cả hai mảng đầu vào. | 

Với độ dài mảng lên đến`10^6`, xử lý tuyến tính là thang đo thích hợp. Thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi giá trị đầu vào và tránh so sánh theo cặp hoặc khám phá không gian trạng thái. Việc sử dụng bộ nhớ cũng tuyến tính, chủ yếu là lưu trữ hai mảng đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    g = 0
    for x in b:
        g = gcd(g, x)

    r = a[0] % g

    for x in a[1:]:
        if x % g != r:
            print("No")
            return

    print("Yes")

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

# Provided sample
assert run("""5 2
3 6 7 2 5
2 4
""") == "No\n", "sample 1"

# Minimum-size input
assert run("""1 1
100
7
""") == "Yes\n", "single element is already equal to itself"

# All values are already equal
assert run("""5 3
42 42 42 42 42
6 10 14
""") == "Yes\n", "all first-array values are equal"

# Same gcd residue, but no single b value equals the required differences
assert run("""4 3
1 7 13 19
6 10 14
""") == "Yes\n", "all values are congruent modulo gcd 2"

# Different residues modulo gcd
assert run("""3 3
1 7 8
6 10 14
""") == "No\n", "1 and 8 have different parity"

# Maximum-size-shaped test, generated rather than written out literally
n = 100000
a = [1000000000] * n
b = [999999937, 1000000000]

max_input = (
    f"{n} 2\n"
    + " ".join(map(str, a))
    + "\n"
    + " ".join(map(str, b))
    + "\n"
)

assert run(max_input) == "Yes\n", "large linear input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 100 / 7`|`Yes`| tối thiểu`n`, không có cặp nào để so sánh | 
|`5 3 / 42 42 42 42 42 / 6 10 14`|`Yes`| Các giá trị đã bằng nhau và gcd lớn hơn một | 
|`4 3 / 1 7 13 19 / 6 10 14`|`Yes`| Các giá trị không cần khác nhau bởi một trong những giá trị ban đầu`b`giá trị | 
|`3 3 / 1 7 8 / 6 10 14`|`No`| Phát hiện các dư lượng khác nhau theo modulo gcd | 
| Đã tạo`n = 100000`trường hợp |`Yes`| Hành vi thời gian tuyến tính gần giới hạn kích thước mảng | 

## Vỏ cạnh 

Khi chỉ có một phần tử trong mảng đầu tiên, không có ràng buộc bình đẳng giữa các phần tử khác nhau. Vì```
1 1
100
7
```gcd là`7`, dư lượng tham chiếu là`100 % 7 = 2`, và không còn phần tử nào mâu thuẫn với nó. Thuật toán in`Yes`. 

Khi tất cả các giá trị đã bằng nhau thì không cần thực hiện thao tác nào. Vì```
5 3
42 42 42 42 42
6 10 14
```gcd là`2`và mọi giá trị đều có số dư`0`. Quá trình quét không tìm thấy sự không khớp và in`Yes`. Thuật toán không cần thiết phải có một thao tác có sẵn để thực hiện. 

Khi một số kích thước bước được phép chia sẻ một gcd không tầm thường thì các giá trị bước riêng lẻ không phải là bất biến thực sự. Vì```
4 3
1 7 13 19
6 10 14
```gcd là`2`và tất cả bốn giá trị mảng đầu tiên đều có phần dư`1`. Mặc dù sự khác biệt giữa các giá trị tùy ý như`1`Và`13`không cần phải là một trong những mục trong`b`, nó chia hết cho`2`, vì vậy nó có thể được tạo ra từ sự kết hợp của`6`,`10`, Và`14`. Thuật toán in chính xác`Yes`. 

Khi hai giá trị có thặng dư khác nhau theo modulo gcd, không có chuỗi phép toán nào có thể làm cho chúng bằng nhau. Vì```
3 3
1 7 8
6 10 14
```gcd là`2`. Hai giá trị đầu tiên là số lẻ, trong khi`8`là chẵn. Quá trình quét đạt tới`8`, tính toán`8 % 2 = 0`, so sánh nó với dư lượng tham chiếu`1`, và in ngay`No`. 

Kích thước đầu vào tối đa không đưa ra trường hợp ranh giới toán học nhưng nó cho thấy việc triển khai không hiệu quả. Với`n = 100000`hoặc lớn hơn, việc so sánh từng cặp sẽ cần hàng tỷ tờ séc. Thay vào đó, thuật toán được trình bày thực hiện một lượt gcd và một lượt dư, do đó tăng tỷ lệ kích thước đầu vào một cách tuyến tính.
