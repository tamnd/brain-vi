---
title: "CF 102318C - Hát trong mưa"
description: "Đĩa CD chứa t bài hát được đánh số từ 1 đến t, sắp xếp theo chu kỳ. Anya chỉ định một chuỗi các bản nhạc phải được phát theo đúng thứ tự đó. Bản nhạc được yêu cầu đầu tiên đã được sắp xếp sẵn nên không cần nhấn nút cho bản nhạc đó."
date: "2026-08-14T04:41:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102318
codeforces_index: "C"
codeforces_contest_name: "UCF Locals 2017"
rating: 0
weight: 102318
solve_time_s: 81
verified: true
draft: false
---

[CF 102318C - Hát trong mưa](https://codeforces.com/problemset/problem/102318/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đĩa CD chứa`t`bài hát được đánh số từ`1`bởi vì`t`, sắp xếp theo chu kỳ. Anya chỉ định một chuỗi các bản nhạc phải được phát theo đúng thứ tự đó. Bản nhạc được yêu cầu đầu tiên đã được sắp xếp sẵn nên không cần nhấn nút cho bản nhạc đó. Sau một bài hát`k`kết thúc, người chơi tự nhiên xếp hàng`k + 1`, gói từ`t`quay lại`1`. 

Arup có thể thay đổi bản nhạc được xếp hàng đó bằng hai nút. Một lần nhấn tiến sẽ tiến lên bản nhạc đang xếp hàng đợi một, trong khi một lần nhấn lùi sẽ di chuyển nó lùi lại một bản nhạc. Vì các tuyến đường tạo thành một vòng tròn nên mục tiêu giữa mỗi cặp tuyến đường được yêu cầu liên tiếp chỉ đơn giản là chọn hướng ngắn hơn xung quanh vòng tròn đó. Đầu ra là tổng số lần nhấn nút tối thiểu cần thiết cho tất cả các chuyển đổi liên tiếp. Tuyên bố cuộc thi ban đầu nêu rõ`t <= 10^9`và nhiều nhất`1000`các bản nhạc được yêu cầu cho mỗi trường hợp thử nghiệm. 

Giới hạn lớn trên`t`là hạn chế chính. Một đĩa CD có thể chứa một tỷ bản nhạc, do đó việc mô phỏng các lần nhấn nút riêng lẻ có thể yêu cầu hàng trăm triệu thao tác cho một lần chuyển đổi. Với tối đa 1000 bản nhạc được yêu cầu, mô phỏng như vậy có thể đạt tới khoảng`1000 * 5 * 10^8`, hoặc về`5 * 10^11`hoạt động cấp nút trong trường hợp xấu nhất. Cần phải tính toán theo thời gian không đổi cho từng cặp rãnh được yêu cầu. Số lượng ca kiểm thử không bị giới hạn bởi một giá trị lớn trong câu lệnh, nhưng bản thân tổng số đầu vào đủ nhỏ để`O(s)`giải pháp cho mỗi trường hợp là dễ dàng thích hợp. 

Trường hợp khó phát hiện đầu tiên là khi bản nhạc được yêu cầu chính xác là bản nhạc sẽ phát một cách tự nhiên. Ví dụ:```
1
5 2
3 4
```Đầu ra đúng là:```
0
```Sau khi theo dõi`3`kết thúc, theo dõi`4`đã được xếp hàng rồi. Việc triển khai bất cẩn có thể coi vị trí hiện tại là dấu vết`3`và tính khoảng cách`1`, quên mất rằng CD đã tiến lên một cách tự nhiên trước khi nhấn bất kỳ nút nào. 

Trường hợp tinh vi thứ hai là quấn quanh phần cuối của đĩa CD. Ví dụ:```
1
5 2
5 1
```Đầu ra đúng là:```
0
```Theo dõi`1`tự nhiên theo dõi`5`, vì vậy không cần nút nào. Việc triển khai sử dụng sự khác biệt tuyệt đối thông thường giữa`5`Và`1`sẽ nhận được sai`4`. 

Trường hợp tinh tế thứ ba là yêu cầu cùng một bản nhạc hai lần liên tiếp. Ví dụ:```
1
3 3
3 1 1
```Đầu ra đúng là:```
1
```Sau khi theo dõi`1`kết thúc, theo dõi`2`đang được xếp hàng. Một lần nhấn lùi sẽ quay lại theo dõi`1`. Chỉ cần so sánh hai số bản nhạc được yêu cầu sẽ cho khoảng cách bằng 0, điều này là sai vì quá trình chuyển đổi tự nhiên đã đưa người chơi về phía trước. Trường hợp chính xác này xuất hiện dưới dạng mẫu chính thức thứ ba. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp có thể mô phỏng đầu đĩa CD theo đúng nghĩa đen. Sau khi mỗi bản nhạc được yêu cầu kết thúc, chúng tôi bắt đầu từ bản nhạc tiếp theo được xếp hàng tự nhiên và nhấn liên tục nút tiến hoặc lùi cho đến khi đạt được bản nhạc mong muốn. Chúng ta có thể thử cả hai hướng và đếm số lần nhấn, sau đó chọn số đếm nhỏ hơn. 

Cách tiếp cận này đúng vì mỗi lần nhấn nút sẽ thay đổi bản nhạc được xếp hàng đợi chính xác một vị trí xung quanh đĩa CD tròn. Tuy nhiên, nó hoàn toàn bỏ qua thực tế là đĩa CD có thể chứa tới một tỷ bản nhạc. Trong trường hợp xấu nhất, việc đến được tuyến đường được yêu cầu có thể mất khoảng`t / 2`, hoặc`5 * 10^8`, máy ép. Với khoảng 1000 bản nhạc được yêu cầu, con số đó gần bằng`5 * 10^11`hoạt động mô phỏng, vượt xa giới hạn cuộc thi một giây. Đánh giá cuộc thi chỉ ra rõ ràng rằng CD có thể có tới một tỷ bản nhạc và việc đếm các chuyển tiếp riêng lẻ là không khả thi. 

Quan sát loại bỏ mô phỏng là mọi chuyển đổi đều diễn ra trên cùng một cấu trúc vòng tròn. Giả sử bản nhạc được yêu cầu trước đó là`a`. Một lần`a`kết thúc, người chơi đã xếp hàng tại```
a + 1
```với`t + 1`được hiểu là`1`. Từ đường đua xếp hàng đó, việc tiến lên hay lùi lại chỉ là chuyển động theo một chu kỳ dài`t`. 

Hãy để bản nhạc được xếp hàng đợi`cur`và đích đến được yêu cầu là`b`. Khoảng cách trực tiếp là`abs(cur - b)`. Di chuyển theo hướng ngược lại sẽ đi qua đường bao quanh và do đó mất`t - abs(cur - b)`máy ép. Số tối ưu là số nhỏ hơn trong hai giá trị này. 

Điều này làm giảm mọi quá trình chuyển đổi từ hàng trăm triệu lần nhấn mô phỏng sang một số phép tính số học. Toàn bộ vấn đề trở thành một lần vượt qua trình tự được yêu cầu. Đánh giá chính thức của cuộc thi mô tả mức giảm tương tự, nhấn mạnh rằng bài hát được xếp hàng đợi sau khi bài hát trước kết thúc phải được sử dụng làm điểm bắt đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(s * t)`trường hợp xấu nhất |`O(1)`| Quá chậm | 
| Tối ưu |`O(s)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng bài hát`t`và trình tự được yêu cầu. Bản nhạc được yêu cầu đầu tiên không yêu cầu nhấn nút vì người chơi ban đầu được yêu cầu theo dõi bản nhạc đó. 
2. Đặt bản nhạc được yêu cầu đầu tiên làm bản nhạc trước đó. Mỗi lần chuyển đổi sau này sẽ được tính toán tương ứng với giá trị này. 
3. Đối với mỗi bản nhạc được yêu cầu tiếp theo`target`, tính toán bản nhạc sẽ được xếp hàng sau`previous`kết thúc. Sử dụng số bản nhạc dựa trên một, đây là`(previous % t) + 1`. 

Bước này tự động xử lý phần bao quanh. Nếu như`previous == t`, biểu thức tạo ra`1`, khớp chính xác với hành vi tuần hoàn của CD. 
4. Tính khoảng cách thông thường giữa đường được xếp hàng tự nhiên và`target`:```
direct = abs(current - target)
```5. Tính khoảng cách theo chiều ngược lại:```
wrap = t - direct
```Có chính xác`t`vị trí xung quanh chu kỳ hoàn chỉnh, vì vậy sau khi di chuyển`direct`bước một chiều, tuyến đường còn lại chứa`t - direct`các bước. 
6. Thêm`min(direct, wrap)`để trả lời. Đây là số lần nhấn nút tối thiểu cần thiết cho quá trình chuyển đổi này. 
7. Thay thế`previous`với`target`và tiếp tục cho đến khi mọi bản nhạc được yêu cầu đã được xử lý. 

### Tại sao nó hoạt động 

Điều bất biến là ngay trước khi xử lý một quá trình chuyển đổi,`previous`là bài hát vừa chơi xong. Do đó,`(previous % t) + 1`chính xác là bản nhạc sẽ được xếp hàng đợi mà không cần nhấn bất cứ thứ gì. Mọi chiến lược nút có thể thực hiện được từ thời điểm đó là chuyển động quanh một chu kỳ có độ dài`t`, do đó chỉ có hai hướng liên quan giữa rãnh hiện tại và mục tiêu. chiều dài của chúng là`direct`Và`t - direct`, và cái ngắn hơn là tối ưu. Vì mỗi quá trình chuyển đổi là độc lập sau khi biết được tuyến đường bắt đầu được xếp hàng đợi của nó, nên việc tính tổng các chi phí chuyển đổi tối thiểu này sẽ cho ra mức tối thiểu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    out = []

    for _ in range(n):
        t, s = map(int, input().split())
        tracks = list(map(int, input().split()))

        ans = 0
        previous = tracks[0]

        for target in tracks[1:]:
            current = previous % t + 1
            direct = abs(current - target)
            ans += min(direct, t - direct)
            previous = target

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu vào bắt đầu bằng số lượng ca kiểm thử, khớp với định dạng bài toán ban đầu. Sau đó, mỗi trường hợp sẽ cung cấp kích thước CD, số lượng bản nhạc được yêu cầu và trình tự được yêu cầu. 

Bản nhạc được yêu cầu đầu tiên được lưu trữ trong`previous`và không đóng góp gì cho câu trả lời. Điều này diễn ra trực tiếp từ điều kiện ban đầu là người chơi đã được đưa vào bản nhạc đó. 

Đối với mỗi bài hát sau này,`previous % t + 1`tính toán đường đi được xếp hàng tự nhiên. các`% t`tốt hơn là một cách rõ ràng`if previous == t`hãy kiểm tra vì nó xử lý cả các bản nhạc thông thường và phần bao quanh bằng một biểu thức. 

giá trị`direct`nhiều nhất là`t - 1`, Vì thế`t - direct`luôn là khoảng cách không âm hợp lệ xung quanh phía bên kia của vòng tròn. Số nguyên Python có độ chính xác tùy ý, vì vậy câu trả lời chính thức thứ hai của`3000000000`không yêu cầu loại số nguyên đặc biệt. Trong các ngôn ngữ có loại số nguyên có chiều rộng cố định, nên sử dụng số nguyên 64 bit cho câu trả lời tích lũy. Người đánh giá cuộc thi đưa ra nhận xét tương tự về câu trả lời có khả năng vượt quá số nguyên 32 bit. 

Thứ tự của các hoạt động quan trọng. Chúng tôi tính toán tuyến đường được xếp hàng tự nhiên trước khi so sánh nó với mục tiêu. So sánh`previous`trực tiếp với`target`sẽ bỏ lỡ phần tiến lên một bản nhạc tự động sau mỗi bài hát hoàn thành. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên chính thức là:```
1
68 6
67 57 66 67 48 15
```Câu trả lời là`73`. 

| Trước | Xếp hàng tự nhiên | Mục tiêu | Khoảng cách trực tiếp | Hướng khác | Chi phí | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 67 | 68 | 57 | 11 | 57 | 11 | 11 | 
| 57 | 58 | 66 | 8 | 60 | 8 | 19 | 
| 66 | 67 | 67 | 0 | 68 | 0 | 19 | 
| 67 | 68 | 48 | 20 | 48 | 20 | 39 | 
| 48 | 49 | 15 | 34 | 34 | 34 | 73 | 

Sau khi theo dõi`67`kết thúc, theo dõi`68`đã được xếp hàng, vì vậy việc tiếp cận`57`ngược lại`11`máy ép. Từ`58`, đạt`66`chuyển tiếp mất`8`. Sau đó`66`, theo dõi`67`diễn ra một cách tự nhiên, do đó quá trình chuyển đổi không tốn kém gì. Chi phí cho hai lần chuyển đổi cuối cùng`20`Và`34`, cho`73`. 

Dấu vết này thể hiện sự bất biến trung tâm. Mỗi hàng bắt đầu từ bản nhạc mà người chơi thực sự đã xếp hàng sau khi bản nhạc được yêu cầu trước đó kết thúc, chứ không phải từ chính bản nhạc được yêu cầu trước đó. 

### Mẫu 2 

Mẫu thứ hai chính thức là:```
1
1000000000 7
1 500000002 3 500000004 5 500000006 7
```Câu trả lời là`3000000000`. 

| Trước | Xếp hàng tự nhiên | Mục tiêu | Khoảng cách trực tiếp | Hướng khác | Chi phí | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 500000002 | 500000000 | 500000000 | 500000000 | 500000000 | 
| 500000002 | 500000003 | 3 | 500000000 | 500000000 | 500000000 | 1000000000 | 
| 3 | 4 | 500000004 | 500000000 | 500000000 | 500000000 | 1500000000 | 
| 500000004 | 500000005 | 5 | 500000000 | 500000000 | 500000000 | 2000000000 | 
| 5 | 6 | 500000006 | 500000000 | 500000000 | 500000000 | 2500000000 | 
| 500000006 | 500000007 | 7 | 500000000 | 500000000 | 500000000 | 3000000000 | 

Mỗi lần chuyển đổi chính xác là nửa tỷ lần nhấn theo một trong hai hướng. Phần quan trọng là thuật toán thực hiện sáu phép tính số học thay vì mô phỏng ba tỷ lần nhấn nút. 

Ví dụ này thực hiện cả giới hạn số lớn và nhu cầu về số học có kích thước 64 bit trong các ngôn ngữ có số nguyên có chiều rộng cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(s)`mỗi trường hợp thử nghiệm | Mỗi bản nhạc được yêu cầu sau bản nhạc đầu tiên được xử lý một lần với số học theo thời gian không đổi. | 
| Không gian |`O(s)`| Việc thực hiện lưu trữ trình tự được yêu cầu. | 

Độ dài chuỗi tối đa chỉ là 1000, trong khi bản thân đĩa CD có thể chứa một tỷ bản nhạc. Thuật toán không bao giờ phân bổ bất cứ thứ gì tỷ lệ thuận với số lượng bản nhạc và nó không bao giờ lặp qua đĩa CD. Thời gian chạy của nó chỉ phụ thuộc vào số lượng bài hát được yêu cầu, do đó, nó phù hợp thoải mái với giới hạn một giây và 256 MB do Codeforces chỉ định. 

Trình tự này cũng có thể được xử lý tăng dần nếu muốn, giảm dung lượng lưu trữ phụ xuống còn`O(1)`, nhưng việc giữ nguyên trình tự đầu vào giúp việc triển khai trở nên đơn giản và vẫn chỉ sử dụng một vài kilobyte trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    out = []

    for _ in range(n):
        t = int(next(it))
        s = int(next(it))

        previous = int(next(it))
        ans = 0

        for _ in range(s - 1):
            target = int(next(it))
            current = previous % t + 1
            direct = abs(current - target)
            ans += min(direct, t - direct)
            previous = target

        out.append(str(ans))

    return "\n".join(out)

def run(inp: str) -> str:
    return solve_data(inp)

# Provided sample 1
assert run(
    """1
68 6
67 57 66 67 48 15
"""
) == "73", "sample 1"

# Provided sample 2
assert run(
    """1
1000000000 7
1 500000002 3 500000004 5 500000006 7
"""
) == "3000000000", "sample 2"

# Provided sample 3
assert run(
    """1
3 3
3 1 1
"""
) == "1", "sample 3"

# Minimum-size CD and sequence
assert run(
    """1
1 1
1
"""
) == "0", "minimum-size case"

# All requested tracks are identical
assert run(
    """1
7 5
4 4 4 4 4
"""
) == "4", "repeated track requires backward presses"

# Boundary wraparound
assert run(
    """1
5 2
5 1
"""
) == "0", "natural wraparound"

# Exact half-circle tie
assert run(
    """1
10 2
1 7
"""
) == "5", "half-circle distance"

# Maximum-size CD with maximum sequence length, all equal
max_case = "1000000000 1000\n" + " ".join(["123456789"] * 1000) + "\n"
assert run(max_case) == "999", "maximum-size all-equal sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 1`|`0`| Kích thước CD tối thiểu và độ dài chuỗi tối thiểu | 
|`7 5 / 4 4 4 4 4`|`4`| Các yêu cầu lặp lại phải tính đến bản nhạc tiếp theo được xếp hàng tự nhiên | 
|`5 2 / 5 1`|`0`| Bao quanh từ bài hát cuối cùng đến bài hát`1`| 
|`10 2 / 1 7`|`5`| Các tuyến tiến và lùi có chi phí bằng nhau | 
|`1000000000 1000 / 1000 copies of 123456789`|`999`| Giới hạn số tối đa và xử lý theo dõi lặp lại | 

Việc kiểm tra hoàn toàn bằng nhau không được mong đợi sẽ cho kết quả bằng 0. Sau khi một bản nhạc kết thúc, người chơi đã chuyển sang bản nhạc tiếp theo. Ví dụ, sau khi theo dõi`4`kết thúc trên một đĩa CD bảy ​​bài hát, bài hát`5`đang được xếp hàng. Trở lại theo dõi`4`mất một lần nhấn lùi. Bốn quá trình chuyển đổi như vậy tạo ra`4`. 

Kiểm tra kích thước tối đa cũng kiểm tra xem việc triển khai không vô tình lặp lại trên đĩa CD một tỷ rãnh. Trình tự của nó chứa 1000 yêu cầu nên chỉ có 999 chuyển đổi được xử lý. 

## Vỏ cạnh 

Đối với trường hợp kế thừa tự nhiên, hãy xem xét:```
1
5 2
3 4
```Thuật toán bắt đầu với`previous = 3`. Nó tính toán`current = 3 % 5 + 1 = 4`, đã bằng mục tiêu. Như vậy`direct = 0`và quá trình chuyển đổi đóng góp bằng không. Đầu ra cuối cùng là`0`. Điều này ngăn ngừa lỗi phổ biến khi đo từ đường ray`3`thay vì từ bản nhạc đã được xếp hàng đợi`4`. 

Để bao bọc, hãy xem xét:```
1
5 2
5 1
```Đây`previous = 5`, Vì thế`current = 5 % 5 + 1 = 1`. Mục tiêu cũng là`1`, cho chi phí bằng không. Thứ tự vòng tròn của CD được thể hiện trực tiếp bằng biểu thức modulo, do đó không cần có trường hợp đặc biệt nào cho bản nhạc cuối cùng. 

Đối với các yêu cầu lặp lại, hãy xem xét:```
1
3 3
3 1 1
```Quá trình chuyển đổi đầu tiên đi từ bài hát trước đó`3`theo dõi xếp hàng tự nhiên`1`, vì vậy nó có giá bằng không. Đối với lần chuyển tiếp thứ hai, sau track`1`kết thúc, theo dõi`2`đang được xếp hàng. Mục tiêu là`1`, Vì thế`direct = 1`và chi phí tuyến đường ngược lại`3 - 1 = 2`. Thuật toán chọn`1`, tạo ra tổng đúng của`1`. Đây chính xác là hành vi được thể hiện bằng mẫu thứ ba chính thức. 

Đối với đĩa CD có một bản nhạc, hãy xem xét:```
1
1 1
1
```Đường đua được xếp hàng tự nhiên luôn`1`. Khoảng cách trực tiếp bằng 0 và khoảng cách khác là`1`, vì vậy mọi chuyển đổi sẽ không tốn phí. Chỉ với một bản nhạc được yêu cầu thì không có sự chuyển đổi nào cả và câu trả lời cũng là 0. 

Đối với trường hợp nửa đường tròn, xét:```
1
10 2
1 7
```Sau khi theo dõi`1`, theo dõi`2`đang được xếp hàng. Theo dõi`7`cách đó năm vị trí theo một trong hai hướng trên một chu kỳ mười đường. Thuật toán tính toán`direct = 5`Và`t - direct = 5`, sau đó chọn`5`. Hướng nút nào cũng tối ưu, vì vậy câu trả lời là`5`.
