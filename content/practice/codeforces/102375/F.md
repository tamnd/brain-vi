---
title: "CF 102375F - \u041f\u0440\u0430\u0432\u0438\u043b\u044c\u043d\u044b\u0439 \u043f\u043e\u0434\u043c\u043d\u043e\u0433\u043e\u0443\u0433\u043e\u043b\u044c\u043d\u0438\u043a"
description: "Chúng ta có một đa giác đều có (N) đỉnh và chúng ta muốn giữ càng ít đỉnh đó càng tốt trong khi biến các đỉnh được chọn thành các đỉnh của một đa giác đều khác. Giả sử chúng ta chọn (k) đỉnh."
date: "2026-08-15T07:04:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "F"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 128
verified: false
draft: false
---

[CF 102375F - \u041f\u0440\u0430\u0432\u0438\u043b\u044c\u043d\u044b\u0439 \u043f\u043e\u0434\u043c\u043d\u043e\u0433\u043e\u0443\u0433\u043e\u043b\u 044c\u043d\u0438\u043a](https://codeforces.com/problemset/problem/102375/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác đều có (N) đỉnh và chúng ta muốn giữ càng ít đỉnh đó càng tốt trong khi biến các đỉnh được chọn thành các đỉnh của một đa giác đều khác. 

Giả sử chúng ta chọn (k) đỉnh. Vì tất cả các đỉnh ban đầu đều nằm trên cùng một đường tròn và cách đều nhau một góc (2\pi/N), nên các đỉnh được chọn tạo thành một (k)-giác đều chính xác khi chúng ta có thể lấy mọi đỉnh ban đầu thứ (N/k). Điều đó có thể xảy ra chính xác khi (k) chia hết (N). 

Vì vậy, bài toán quy về việc tìm ước số nhỏ nhất của (N) ít nhất là (3). Nếu (N) là số nguyên tố thì ít nhất không có ước số thực sự (3), nên đáp án chính là (N). 

Giá trị của (N) có thể đạt tới (10^{12}). Một thuật toán quét tất cả các kích thước đa giác có thể lên tới (N) có thể thực hiện gần như (10^{12}) lần lặp, vượt xa những gì thực tế. Chúng ta cần khai thác cấu trúc ước số của (N), thay vì kiểm tra mọi số có thể. 

Có hai trường hợp nhỏ dễ lộ sai sót. Với (N=5), câu trả lời là (5), vì (5) là số nguyên tố và không có tam giác hoặc đa giác đều nhỏ hơn nào giữa các đỉnh của nó. Với (N=8), câu trả lời là (4), không phải (8), vì mỗi đỉnh thứ hai tạo thành một hình vuông. Giải pháp bất cẩn chỉ tìm ước số lẻ sẽ bỏ sót (4). Một ví dụ hữu ích khác là (N=10), trong đó câu trả lời là (5). Ước số (2) quá nhỏ để biểu thị một đa giác, vì vậy ước số hữu dụng đầu tiên là (5). 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi số có thể (k) của các đỉnh đã chọn, bắt đầu từ (3) và kiểm tra xem (k) có chia hết (N) hay không. Giá trị thành công đầu tiên chính xác là câu trả lời vì một (k)-giác đều có thể được hình thành chính xác cho các ước số (k) của (N). Trong trường hợp xấu nhất, khi (N) là số nguyên tố, chúng tôi kiểm tra mọi giá trị từ (3) đến (N), tức là kiểm tra tính chia hết của (N-2). Đối với (N=10^{12}-11), về cơ bản đó là một nghìn tỷ lần lặp, vì vậy phương pháp này không thể sử dụng được. 

Quan sát quan trọng là thuộc tính ước số tiêu chuẩn mà mọi số tổng hợp (N) đều có ước số không vượt quá (\sqrt N). Giả sử ước số hữu dụng nhỏ nhất (d\ge3) lớn hơn (\sqrt N). Khi đó ước số bù (N/d) của nó sẽ nhỏ hơn (\sqrt N). Vì (d) ít nhất là (3), nên ước số bù cũng là ước số của (N) và nếu nó ít nhất bằng (3) thì đó sẽ là một câu trả lời thậm chí còn nhỏ hơn có thể sử dụng được. Ước số bổ sung duy nhất có thể có bên dưới (3) là (1) hoặc (2) và những trường hợp đó có nghĩa là thừa số liên quan (d) là (N) hoặc (N/2). Trong cả hai trường hợp, việc kiểm tra tất cả các ước số cho đến (\sqrt N) vẫn cho chúng ta câu trả lời khi kết hợp với việc kiểm tra trực tiếp từng ước số ứng cử viên. 

Trên thực tế, chúng ta có thể kiểm tra mọi số nguyên (d) từ (3) đến (\lfloor\sqrt N\rfloor). Ước số đầu tiên gặp phải là câu trả lời. Nếu không tồn tại ước số như vậy thì (N) là số nguyên tố, ngoại trừ khả năng ước số nhỏ nhất là (2). Nhưng (2) bản thân nó không thể là câu trả lời, vì vậy chúng ta phải tiếp tục tìm kiếm một ước số có thể sử dụng được. Vòng lặp qua tất cả các số nguyên sẽ xử lý việc này một cách tự nhiên. Đối với trường hợp giống số nguyên tố chẵn chẳng hạn như (N=2p), vòng lặp cuối cùng sẽ đạt đến (p), với điều kiện là (p\le\sqrt N) và nếu (p>\sqrt N), thì (N) thực sự được xử lý bằng cách nhận ra rằng ước số thực sự duy nhất của nó là (2), vì vậy câu trả lời là (p=N/2). Do đó, cách triển khai rõ ràng nhất sẽ xử lý rõ ràng hệ số (2), sau đó tìm kiếm các ước số lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(\sqrt N)) | (O(1)) | Đã chấp nhận |

Một cách rõ ràng hơn một chút để xây dựng phép tìm kiếm tối ưu là tìm ước số nhỏ nhất của (N) ít nhất là (3). Trước tiên chúng ta có thể xử lý lũy thừa của (2) thông qua ứng viên (4), sau đó tìm ước số lẻ. Tuy nhiên, có một cách thực hiện thậm chí còn đơn giản hơn: kiểm tra (d=3,4,5,\ldots) lên tới (\sqrt N) và nếu không chia hết (N), hãy xác định trường hợp còn lại từ hệ số (2). Việc thực hiện dưới đây sử dụng công thức trực tiếp này. 

## Hướng dẫn thuật toán 

1. Đọc (N). Chúng ta đang tìm ước số nhỏ nhất của (N) ít nhất là (3). 
2. Nếu (N) chia hết cho (4) thì trả về ngay (4). Hình vuông là đa giác nhỏ nhất có thể có sau một hình tam giác và khả năng chia hết cho (4) có nghĩa là mọi đỉnh thứ (N/4) tạo thành một. 
3. Bắt đầu từ (d=3), kiểm tra mọi số nguyên (d) while (d^2\le N). Nếu (d\mid N), trả về (d). Tìm kiếm theo thứ tự tăng dần đảm bảo rằng ước số đầu tiên tìm được là câu trả lời nhỏ nhất có thể. 
4. Nếu không có ước từ (3) đến (\sqrt N), phân biệt các trường hợp còn lại. Nếu (N) là số chẵn thì ước số thực sự duy nhất của nó có thể nhỏ hơn (\sqrt N) là (2), do đó (N=2p) đối với một số nguyên tố lẻ (p) và ước số nhỏ nhất có thể sử dụng là (p=N/2). Nếu (N) là số lẻ thì việc không có ước số lên tới (\sqrt N) có nghĩa là (N) là số nguyên tố, vì vậy câu trả lời là (N). 

Lý do tìm kiếm chỉ cần đạt tới (\sqrt N) là vì các ước số có cặp (d) và (N/d). Nếu một số tổng hợp có ước số lớn hơn (\sqrt N), thì ước số ghép đôi của nó sẽ nhỏ hơn (\sqrt N). Do đó, một ước số bị thiếu bên dưới (\sqrt N) sẽ cho chúng ta biết chính xác hệ số còn lại có thể trông như thế nào. 

### Tại sao nó hoạt động 

Gọi (k) là số đỉnh được chọn. Các đỉnh được chọn tạo thành một (k)-giác đều chính xác khi vị trí của chúng xung quanh đa giác ban đầu cách đều nhau. Khoảng cách phải là (N/k) các cạnh gốc, vì vậy (N/k) phải là số nguyên. Do đó kích thước đa giác hợp lệ chính xác là các ước số của (N) ít nhất là (3). 

Thuật toán kiểm tra các ước số có thể có này theo thứ tự tăng dần lên tới (\sqrt N). Nếu nó tìm thấy một thì không thể bỏ qua ước số hợp lệ nhỏ hơn. Nếu không tìm thấy thì mọi ước số thích hợp nhỏ hơn (\sqrt N) là (1) hoặc (2). Đối với (N) lẻ thì (N) là số nguyên tố. Đối với (N) chẵn, điều đó có nghĩa là ước số thực sự duy nhất là (2), vì vậy (N=2p) và (p=N/2) là ước số nhỏ nhất có thể biểu thị một đa giác. Do đó, giá trị trả về luôn là số đỉnh hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    if n % 4 == 0:
        print(4)
        return

    d = 3
    while d * d <= n:
        if n % d == 0:
            print(d)
            return
        d += 2

    if n % 2 == 0:
        print(n // 2)
    else:
        print(n)

solve()
```Kiểm tra đầu tiên về khả năng chia hết cho (4) xử lý kích thước đa giác chẵn nhỏ nhất có thể. Nếu (4\mid N), không có câu trả lời nào nhỏ hơn (4) có thể tồn tại vì (3\nmid N), vì vậy (4) là tối ưu ngay lập tức. 

Sau đó, vòng lặp chỉ kiểm tra các ứng viên lẻ bắt đầu từ (3). Ngay cả những ứng cử viên khác ngoài (4) cũng không thể là câu trả lời nhỏ nhất. Ví dụ: nếu (8\mid N), (4) đã được trả về. Nếu (N) chia hết cho một số chẵn lớn hơn (4) thì nó cũng chia hết cho (4), vậy trường hợp đó đã được xử lý. 

điều kiện`d * d <= n`là dạng số nguyên của (d\le\sqrt N). Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn ngay cả khi (N=10^{12}). 

Nếu vòng lặp kết thúc, không có ước số lẻ của (N) giữa (3) và (\sqrt N). Đối với số lẻ (N), điều này có nghĩa là (N) là số nguyên tố, cho kết quả (N). Đối với (N), thừa số (2) là thừa số nhỏ duy nhất, do đó (N=2p) và ước số hợp lệ nhỏ nhất là (p=N/2). 

Vòng lặp tăng dần theo (2) vì mọi ứng cử viên chẵn đều đã được bao phủ bởi trường hợp đặc biệt (4). Điều này gần như giảm một nửa số lần lặp mà không thay đổi logic. 

## Ví dụ đã hoạt động 

Với (N=5), thời gian thực thi ngắn. 

| (N) | (d) | (d^2\le N) | (N\bmod d) | Hành động | 
| --- | --- | --- | --- | --- | 
| 5 | 3 | Không | | Dừng vòng lặp | 
| 5 | | | | (N) là số lẻ, đầu ra 5 | 

Việc tìm kiếm không bao giờ đạt đến ước số vì (5) là số nguyên tố. Do đó, câu trả lời là toàn bộ hình ngũ giác, với tất cả (5) đỉnh được chọn. 

Với (N=21), chúng ta có được dấu vết sau. 

| (N) | (d) | (d^2\le N) | (21\bmod d) | Hành động | 
| --- | --- | --- | --- | --- | 
| 21 | 3 | Có | 0 | Đầu ra 3 | 

Ứng viên đầu tiên (3) đã chia (21). Chọn mỗi đỉnh thứ bảy sẽ có ba đỉnh cách đều nhau, do đó câu trả lời là (3). 

Dấu vết bổ sung hữu ích là (N=10). 

| (N) | (d) | (d^2\le N) | (10\bmod d) | Hành động | 
| --- | --- | --- | --- | --- | 
| 10 | 3 | Có | 1 | Tiếp tục | 
| 10 | 5 | Không có điều kiện sau vòng lặp | | Điểm dừng vòng lặp | 
| 10 | | | | (N) chẵn, đầu ra (10/2=5) | 

Ở đây (2) chia (10), nhưng hai đỉnh không tạo thành đa giác. Ước số hợp lệ tiếp theo là (5) và nhánh số chẵn cuối cùng sẽ lấy lại chính xác giá trị đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\sqrt N)) | Tối đa khoảng (\sqrt N/2) số lẻ thí sinh được thi | 
| Không gian | (O(1)) | Chỉ (N) và ước số hiện tại được lưu trữ | 

Đối với (N\le10^{12}), (\sqrt N\le10^6). Do đó, thuật toán thực hiện tối đa khoảng nửa triệu phép kiểm tra tính chia hết, điều này rất dễ thực hiện. Việc sử dụng bộ nhớ không đổi bất kể kích thước của (N). 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_value(n: int) -> int:
    if n % 4 == 0:
        return 4

    d = 3
    while d * d <= n:
        if n % d == 0:
            return d
        d += 2

    if n % 2 == 0:
        return n // 2

    return n

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        n = int(sys.stdin.readline())
        return str(solve_value(n))
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("5\n") == "5", "sample 1"
assert run("21\n") == "3", "sample 2"

# Minimum-size input
assert run("3\n") == "3", "minimum N"

# Smallest composite case
assert run("4\n") == "4", "square"

# Even number with no divisor 3 or 4
assert run("10\n") == "5", "2 * prime"

# Maximum-size boundary
assert run("1000000000000\n") == "4", "maximum N"

# Large odd prime
assert run("999999999989\n") == "999999999989", "large prime"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`|`3`| Kích thước đa giác tối thiểu được phép | 
|`4`|`4`| Giá trị tổng hợp nhỏ nhất và trường hợp hình vuông đặc biệt | 
|`10`|`5`| Giá trị chẵn trong đó (2) không sử dụng được và (N/2) là câu trả lời | 
|`1000000000000`|`4`| Ranh giới đầu vào tối đa và lợi nhuận sớm | 
|`999999999989`|`999999999989`| Số nguyên tố lớn yêu cầu tìm kiếm đầy đủ (\sqrt N) | 

## Vỏ cạnh 

Với (N=3), vòng lặp bắt đầu tại (d=3), nhưng điều kiện (3^2\le3) đã sai. Vì (N) là số lẻ nên thuật toán trả về (N=3). Điều này đúng vì hình tam giác đã là đa giác đều nhỏ nhất có thể. 

Với (N=5), lý luận tương tự trả về (5). Việc triển khai bất cẩn giả sử mọi đa giác đều chứa một hình tam giác trong số các đỉnh của nó sẽ trả về không chính xác (3), nhưng ba đỉnh của một hình ngũ giác đều không cách đều nhau. 

Đối với (N=8), kiểm tra ban đầu (N\bmod4=0) trả về (4). Bốn đỉnh thu được bằng cách lấy mỗi đỉnh thứ hai tạo thành một hình vuông. Một giải pháp chỉ kiểm tra các ước số lẻ có thể kết luận sai rằng (8) là câu trả lời. 

Với (N=10), (4) không chia (N) và vòng lặp kiểm tra (3), đây không phải là ước số. Vòng lặp dừng vì (5^2>10). Vì (N) là số chẵn nên thừa số còn lại là (10/2=5), đây là câu trả lời đúng. Điều này mắc phải sai lầm phổ biến khi coi (2) là một câu trả lời có thể xảy ra. 

Với (N=21), ứng viên đầu tiên (d=3) chia (21), do đó thuật toán ngay lập tức trả về (3). Ba đỉnh được chọn cách nhau bởi (21/3=7) cạnh ban đầu, tạo ra các khoảng cách góc bằng nhau. 

Đối với đầu vào tối đa (N=10^{12}), số này chia hết cho (4), do đó thuật toán trả về (4) ngay lập tức mà không cần nhập vòng chia số. Tổng quát hơn, ngay cả khi giá trị có kích thước tối đa được chọn là số nguyên tố, vòng lặp chỉ phải kiểm tra các số lẻ tối đa (10^6), vẫn nằm trong độ phức tạp dự định.
