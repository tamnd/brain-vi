---
title: "CF 102440A - \u0414\u043e\u043c\u0430\u0448\u043d\u044f\u044f \u0430\u043a\u0443\u043b\u0430"
description: "Chúng ta có một chuỗi các đơn vị thực phẩm, trong đó mỗi vị trí lưu trữ một loại thực phẩm. Cá mập có thể ăn chính xác k đơn vị trong một lần ăn và tất cả các đơn vị đó phải cùng loại. Khoảng đã chọn là thành công nếu tất cả thực phẩm bên trong nó có thể được chia thành các nhóm có đúng k loại bằng nhau."
date: "2026-08-09T00:51:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102440
codeforces_index: "A"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Junior"
rating: 0
weight: 102440
solve_time_s: 588
verified: true
draft: false
---

[CF 102440A - \u0414\u043e\u043c\u0430\u0448\u043d\u044f\u044f \u0430\u043a\u0443\u043b\u0430](https://codeforces.com/problemset/problem/102440/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các đơn vị thực phẩm, trong đó mỗi vị trí lưu trữ một loại thực phẩm. Cá mập có thể ăn chính xác`k`các đơn vị trong một lần cho ăn và tất cả các đơn vị đó phải cùng loại. Khoảng thời gian đã chọn sẽ thành công nếu tất cả thực phẩm bên trong nó có thể được chia thành các nhóm chính xác`k`loại bằng nhau. 

Trong một khoảng thời gian cố định, thứ tự các phần tử của nó không quan trọng. Điều quan trọng là số lần xuất hiện của mỗi loại thực phẩm. Khoảng có giá trị chính xác khi mọi số đếm như vậy đều chia hết cho`k`. 

Nhiệm vụ là đếm tất cả các mảng con liền kề thỏa mãn điều kiện đó. Với`n`lên đến`2 * 10^5`, có thể có 

[ 
\frac{n(n+1)}2 = 20.000.100.000 
] 

các khoảng khác nhau trong trường hợp lớn nhất. Một thuật toán bậc hai sẽ phải xử lý khoảng 20 tỷ khoảng, vượt xa giới hạn thời gian của cuộc thi thông thường cho phép. Chúng ta cần một phương thức chỉ xử lý mỗi vị trí mảng trong một số lần không đổi, mang lại độ phức tạp gần như tuyến tính. 

Một số trường hợp ranh giới rất dễ bị xử lý sai. Nếu như`k = 1`, mọi khoảng thời gian đều hợp lệ vì mỗi đơn vị thực phẩm riêng lẻ có thể được ăn riêng. Ví dụ,```
1 1
1
```có câu trả lời`1`, trong khi việc triển khai chỉ tìm kiếm các nhóm có độ dài lớn hơn một có thể trả về 0 không chính xác. 

Nếu như`k > n`, không có khoảng trống nào có thể chứa`k`bản sao của bất kỳ loại nào, do đó không có khoảng thời gian nào có thể hợp lệ. Ví dụ,```
3 4
1 1 1
```có câu trả lời`0`. Một giải pháp chỉ kiểm tra xem tổng chiều dài có chia hết cho không`k`có thể vô tình đếm tiền tố trống hoặc xử lý sai các khoảng thời gian ngắn. 

Các lần xuất hiện lặp lại cùng loại cũng phải được tính theo modulo`k`, thay vì chỉ kiểm tra xem tất cả các loại có xuất hiện cùng số lần hay không. Ví dụ,```
4 2
1 1 1 1
```có câu trả lời`3 + 1 = 4`, bởi vì ba khoảng có độ dài hai và toàn bộ khoảng có độ dài bốn là hợp lệ. Một cách tiếp cận chỉ công nhận nhóm hoàn chỉnh đầu tiên của`k`các đơn vị sẽ bỏ lỡ khoảng thời gian dài hơn. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể liệt kê mọi khoảng thời gian và duy trì tần suất của từng loại thực phẩm đồng thời mở rộng điểm cuối phù hợp của nó. Đối với điểm cuối bên trái cố định, hãy thêm`a[r]`một vị trí tại một thời điểm và giữ một bộ đếm cho biết có bao nhiêu loại thực phẩm hiện có tần số không chia hết cho`k`. Khi bộ đếm đó trở thành 0, khoảng thời gian hiện tại là hợp lệ. Điều này kiểm tra chính xác mọi khoảng thời gian mà không cần quét liên tục tất cả`n`các loại. 

Vấn đề là số lượng khoảng thời gian. có`n(n+1)/2`trong số đó đạt tới`20,000,100,000`khi`n = 200000`. Mặc dù mỗi khoảng thời gian được xử lý theo thời gian không đổi trong phiên bản mạnh mẽ được tối ưu hóa này, thời gian bậc hai vẫn quá chậm. 

Quan sát quan trọng là khả năng chia hết chỉ phụ thuộc vào tần số của mỗi loại modulo`k`. Hãy xem xét các tiền tố của mảng. Đối với mỗi tiền tố, hãy xác định trạng thái của nó là vectơ 

[ 
(c_1 \bmod k,\ c_2 \bmod k,\ \ldots,\ c_n \bmod k), 
] 

ở đâu`c_x`là số lần xuất hiện của loại`x`trong tiền tố đó. 

Giả sử tiền tố kết thúc ở vị trí`l - 1`Và`r`có trạng thái hoàn toàn giống nhau. Trừ các vectơ tần số của chúng cho chúng ta biết rằng mỗi loại xảy ra bội số của`k`lần trong khoảng thời gian`[l, r]`. Do đó khoảng đó là hợp lệ. 

Điều ngược lại cũng đúng. Nếu như`[l, r]`là hợp lệ, mỗi loại đóng góp bội số của`k`, do đó hai trạng thái tiền tố bằng modulo`k`. 

Do đó, bài toán ban đầu trở thành bài toán đếm trạng thái tiền tố tiêu chuẩn: quét mảng, tính trạng thái của mọi tiền tố và đếm xem tồn tại bao nhiêu cặp trạng thái bằng nhau. Nếu một trạng thái đã xuất hiện`f`lần trước, tiền tố hiện tại tạo chính xác`f`khoảng thời gian hợp lệ mới. 

Trở ngại là trạng thái hoàn chỉnh có tới`n`tọa độ, do đó việc lưu trữ nó dưới dạng một bộ dữ liệu sẽ yêu cầu`O(n)`làm việc theo từng vị trí. Thay vào đó, chúng tôi duy trì dấu vân tay 128 bit ngẫu nhiên của trạng thái. Cho mọi loại thực phẩm`x`trọng lượng 128 bit ngẫu nhiên`w[x]`. Nếu dư lượng hiện tại của nó là`c[x]`, đóng góp của nó cho dấu vân tay là`c[x] * w[x]`. Khi một lần xuất hiện của loại`x`được thêm vào thì dư lượng của nó tăng thêm một modulo`k`, do đó tổng số dấu vân tay thay đổi một lượng đã biết. Chúng tôi có thể cập nhật nó trong thời gian liên tục. 

Dấu vân tay mang tính xác suất chứ không phải không có va chạm về mặt toán học, nhưng giá trị ngẫu nhiên 128 bit khiến cho việc va chạm vô tình khó xảy ra. Đây là ý tưởng tương tự như việc băm ngẫu nhiên một vectơ lớn, ngoại trừ ở đây vectơ thay đổi một tọa độ tại một thời điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) dự kiến ​​| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chỉ định từng loại thực phẩm`x`trọng lượng 128 bit ngẫu nhiên`w[x]`. Chúng ta sẽ biểu diễn vectơ của tất cả các dư lượng tần số bằng dấu vân tay 

[ 
H = \sum_x c_x w_x \pmod {2^{128}}, 
] 

ở đâu`c_x`là tần số hiện tại của loại`x`modulo`k`. 

Chúng ta không cần lưu trữ toàn bộ vector. Dấu vân tay đủ để so sánh các trạng thái tiền tố với xác suất áp đảo. 
2. Khởi tạo mọi phần dư về 0, do đó tiền tố trống có dấu vân tay`0`. Đặt dấu vân tay này vào từ điển tần số có số đếm`1`. 

Sự xuất hiện đầu tiên đại diện cho vị trí tiền tố`0`. Nếu tiền tố sau có cùng trạng thái thì khoảng cách giữa hai tiền tố này bắt đầu tại vị trí`1`. 
3. Quét mảng từ trái sang phải. Đối với loại thực phẩm hiện tại`x`, tăng modulo dư lượng của nó`k`. 

Nếu dư lượng cũ của nó nhỏ hơn`k - 1`, số dư tăng thêm một nên hãy thêm`w[x]`tới dấu vân tay. Nếu dư lượng cũ là`k - 1`, phần dư mới trở thành 0, do đó phần đóng góp thay đổi từ`(k - 1)w[x]`về không. Trong trường hợp đó hãy trừ`(k - 1)w[x]`. 
4. Sau khi cập nhật dấu vân tay, hãy tra cứu nó trong từ điển. Nếu nó đã xuất hiện rồi`f`lần, thêm`f`để trả lời. 

Mỗi tiền tố trước có cùng trạng thái tạo thành một khoảng hợp lệ kết thúc ở vị trí hiện tại. Chúng tôi đếm tất cả những khoảng thời gian đó cùng một lúc. 
5. Tăng tần suất của dấu vân tay hiện tại trong từ điển. Tiếp tục cho đến khi mọi vị trí đã được xử lý. 
6. Trả về câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Đối với mỗi tiền tố, dư lượng duy trì cho một loại thực phẩm chính xác là số lần xuất hiện của nó theo modulo`k`. Hãy xem xét hai tiền tố, một tiền tố kết thúc trước một khoảng và một tiền tố kết thúc ở điểm cuối bên phải của nó. Trạng thái của chúng hoàn toàn bằng nhau khi mọi loại thực phẩm đều có cùng dư lượng ở cả hai tiền tố. Trừ hai số tiền tố có nghĩa là mọi loại thực phẩm đều có bội số của`k`lần trong khoảng đó. Khoảng thời gian như vậy có thể được chia thành các nhóm chính xác`k`đơn vị thức ăn bằng nhau nên cá mập có thể ăn hoàn toàn. 

Ngược lại, nếu một khoảng có thể ăn hết thì mỗi loại xảy ra một bội số của`k`lần, do đó việc loại bỏ khoảng thời gian đó khỏi tiền tố sau sẽ không làm thay đổi bất kỳ phần dư nào. Hai trạng thái tiền tố bằng nhau. Do đó, các khoảng hợp lệ tương ứng chính xác với các trạng thái tiền tố bằng nhau. 

Từ điển đếm mỗi cặp trạng thái bằng nhau một lần, vì vậy mọi khoảng hợp lệ đều được tính và không có khoảng không hợp lệ nào được tính. Giá trị gần đúng duy nhất là dấu vân tay ngẫu nhiên: về mặt lý thuyết, hai trạng thái khác nhau có thể nhận cùng một giá trị 128 bit, nhưng xác suất là không đáng kể. 

## Giải pháp Python```python
import sys
import random

input = sys.stdin.readline

MASK = (1 << 128) - 1

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    # A fresh random weight for every food type.
    rng = random.Random()
    weight = [0] + [rng.getrandbits(128) for _ in range(n)]

    # residue[x] = current frequency of type x modulo k.
    residue = [0] * (n + 1)

    # Frequency of every prefix fingerprint.
    seen = {0: 1}

    state = 0
    answer = 0

    for x in a:
        r = residue[x]

        if r == k - 1:
            # (k - 1) * w[x] -> 0
            state = (state - (k - 1) * weight[x]) & MASK
            residue[x] = 0
        else:
            state = (state + weight[x]) & MASK
            residue[x] = r + 1

        answer += seen.get(state, 0)
        seen[state] = seen.get(state, 0) + 1

    print(answer)

if __name__ == "__main__":
    solve()
```các`residue`mảng lưu trữ chính xác thông tin thay đổi khi một đơn vị thực phẩm mới được chế biến. Đối với một loại có dư lượng là`r`, thêm một đơn vị sẽ thay đổi nó thành`r + 1`trừ khi nó đạt tới`k`, trong trường hợp đó nó sẽ quay về 0. 

Dấu vân tay được duy trì nhất quán với cặn đó. Đối với quá trình chuyển đổi thông thường, sự đóng góp thay đổi theo`+weight[x]`. Về quá trình chuyển đổi toàn diện, đóng góp cũ`(k - 1) * weight[x]`biến mất hoàn toàn nên chúng tôi trừ đi giá trị đó. 

các`& MASK`hoạt động giữ modulo dấu vân tay`2^128`. Số nguyên Python có thể tăng lớn tùy ý, do đó nếu không có mặt nạ này, trạng thái sẽ tích lũy các giá trị lớn không cần thiết. Mặt nạ cũng cung cấp cho chúng ta dấu vân tay 128 bit cố định. 

Từ điển bắt đầu bằng`{0: 1}`bởi vì tiền tố trống là trạng thái tiền tố thực. Quên nó sẽ mất mọi khoảng thời gian hợp lệ bắt đầu từ vị trí`1`. Câu trả lời được cập nhật trước khi chèn trạng thái hiện tại, do đó chỉ các tiền tố trước đó mới được sử dụng để tạo thành các khoảng kết thúc tại vị trí hiện tại. 

Số nguyên Python xử lý câu trả lời cuối cùng một cách an toàn. Số lượng khoảng thời gian tối đa là khoảng`2 * 10^10`, nằm trong phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
6 2
1 2 1 2 1 2
```Mỗi loại phải xảy ra một số lần chẵn. Chúng tôi theo dõi dư lượng modulo hai. Để dễ đọc, bảng hiển thị vectơ dư lượng chính xác thay vì dấu vân tay ngẫu nhiên của nó. 

| Vị trí | Đã thêm loại | Trạng thái sau vị trí | Số trạng thái bằng nhau trước đó | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | không |`(0, 0)`| 1 | 0 | 
| 1 | 1 |`(1, 0)`| 0 | 0 | 
| 2 | 2 |`(1, 1)`| 0 | 0 | 
| 3 | 1 |`(0, 1)`| 0 | 0 | 
| 4 | 2 |`(0, 0)`| 1 | 1 | 
| 5 | 1 |`(1, 0)`| 1 | 2 | 
| 6 | 2 |`(1, 1)`| 1 | 3 | 

Ở vị trí thứ 4, trạng thái trở về`(0, 0)`, Vì thế`[1, 4]`là hợp lệ. Ở vị trí thứ năm và thứ sáu, các bang`(1, 0)`Và`(1, 1)`lặp lại các phiên bản trước đó của họ, tạo ra hai khoảng thời gian hợp lệ hơn. Câu trả lời cuối cùng là`3`. 

Dấu vết thể hiện tính bất biến trung tâm: trạng thái dư lượng tiền tố lặp lại chính xác là một khoảng hợp lệ. 

### Mẫu 2 

Đầu vào là```
6 6
1 1 1 1 1 1
```Chỉ có một loại thực phẩm và số lượng của nó phải chia hết cho sáu. 

| Vị trí | Đã thêm loại | Trạng thái sau vị trí | Số trạng thái bằng nhau trước đó | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | không |`(0)`| 1 | 0 | 
| 1 | 1 |`(1)`| 0 | 0 | 
| 2 | 1 |`(2)`| 0 | 0 | 
| 3 | 1 |`(3)`| 0 | 0 | 
| 4 | 1 |`(4)`| 0 | 0 | 
| 5 | 1 |`(5)`| 0 | 0 | 
| 6 | 1 |`(0)`| 1 | 1 | 

Chỉ tiền tố cuối cùng trở về số 0. Nó khớp với tiền tố trống, vì vậy toàn bộ mảng là khoảng hợp lệ duy nhất. 

Ví dụ này thực hiện trường hợp bao quanh`k - 1 -> 0`. Xử lý quá trình chuyển đổi đó một cách đơn giản`state += weight[x]`sẽ không chính xác vì sự đóng góp của loại đó phải biến mất hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) dự kiến ​​| Mỗi phần tử mảng gây ra các cập nhật băm và dư lượng O(1), theo sau là các hoạt động từ điển O(1) dự kiến. | 
| Không gian | O(n) | Mảng dư lượng, trọng số ngẫu nhiên và nhiều nhất là`n + 1`trạng thái tiền tố được lưu trữ. | 

Với`n <= 200000`, thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi đơn vị thực phẩm và lưu trữ một số nguyên tuyến tính. Điều này nằm trong phạm vi dự định của bài toán, trong khi phương pháp bậc hai sẽ phải xử lý khoảng 20 tỷ khoảng. 

## Trường hợp thử nghiệm```python
import sys
import io
import random

MASK = (1 << 128) - 1

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    rng = random.Random()
    weight = [0] + [rng.getrandbits(128) for _ in range(n)]
    residue = [0] * (n + 1)

    seen = {0: 1}
    state = 0
    answer = 0

    for x in a:
        r = residue[x]

        if r == k - 1:
            state = (state - (k - 1) * weight[x]) & MASK
            residue[x] = 0
        else:
            state = (state + weight[x]) & MASK
            residue[x] = r + 1

        answer += seen.get(state, 0)
        seen[state] = seen.get(state, 0) + 1

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("6 2\n1 2 1 2 1 2\n") == "3", "sample 1"
assert run("6 6\n1 1 1 1 1 1\n") == "1", "sample 2"
assert run("9 3\n1 2 3 1 2 3 1 2 3\n") == "1", "sample 3"

# Minimum-size input
assert run("1 1\n1\n") == "1", "minimum size"

# All intervals are valid when k = 1:
# n * (n + 1) / 2 = 6 * 7 / 2 = 21
assert run("6 1\n1 2 1 2 3 3\n") == "21", "k = 1"

# k > n, so no non-empty interval can contain k equal units
assert run("3 4\n1 1 1\n") == "0", "k greater than n"

# Catches the wraparound and prefix-boundary cases.
# Valid intervals are [1,2], [2,3], [3,4], [1,4].
assert run("4 2\n1 1 1 1\n") == "4", "multiple complete groups"

# Maximum-size all-equal case.
# Only the whole array has 200000 occurrences, divisible by k.
n = 200000
assert run(
    f"{n} {n}\n" + " ".join(["1"] * n) + "\n"
) == "1", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| Đầu vào tối thiểu và`k = 1`ranh giới | 
|`6 1 / 1 2 1 2 3 3`|`21`| Mỗi khoảng có giá trị khi`k = 1`| 
|`3 4 / 1 1 1`|`0`| Không có khoảng thời gian nào có thể chứa đủ thức ăn khi`k > n`| 
|`4 2 / 1 1 1 1`|`4`| Nhiều nhóm hoàn chỉnh và gói modulo | 
|`200000 200000 / 1 ... 1`|`1`| Tối đa`n`, tối đa`k`và xử lý trạng thái câu trả lời lớn | 

## Vỏ cạnh 

Khi nào`k = 1`, mọi tần số đều tự động bằng 0 modulo một. Trạng thái tiền tố không bao giờ thay đổi, vì vậy tất cả`n + 1`tiền tố có cùng trạng thái. Do đó, từ điển sẽ đếm từng cặp tiền tố, đưa ra`n(n+1)/2`. Vì`n = 6`, đây là`21`, chính xác như các bài kiểm tra tương ứng. 

Khi`k > n`, không có khoảng trống nào có thể chứa`k`bản sao của bất kỳ loại nào. Trong đầu vào`3 4 / 1 1 1`, dư lượng tiến triển từ`0`ĐẾN`1`, sau đó`2`, sau đó`3`, và không bao giờ trở về 0. Trạng thái ban đầu không bao giờ được lặp lại nên câu trả lời vẫn là`0`. 

Đối với các nhóm hoàn chỉnh lặp đi lặp lại, hãy xem xét`4 2 / 1 1 1 1`. Các tiểu bang là`0, 1, 0, 1, 0`. Trạng thái 0 xảy ra tại các vị trí`0`,`2`, Và`4`, tạo ra ba khoảng thời gian, trong khi trạng thái một xảy ra tại các vị trí`1`Và`3`, sản xuất thêm một cái nữa Câu trả lời là`3 + 1 = 4`. 

Quá trình chuyển đổi bao quanh xứng đáng được chú ý đặc biệt. Nếu dư lượng hiện tại là`k - 1`, việc thêm một lần xuất hiện sẽ làm cho nó bằng 0, không phải`k`. Vì`k = 2`, một loại thay đổi từ dư lượng`1`để lại dư lượng`0`. Mã trừ`(k - 1) * weight[x]`, chính xác là đóng góp cũ, vì vậy loại đó không đóng góp gì cho trạng thái mới. 

Cuối cùng, một khoảng hợp lệ có thể bắt đầu ở vị trí mảng đầu tiên. Đó là lý do tại sao tiền tố trống được chèn vào`seen`trước khi xử lý bất kỳ phần tử nào. Nếu không có trạng thái ban đầu đó, một đầu vào như`6 6 / 1 1 1 1 1 1`sẽ không đếm được toàn bộ mảng, mặc dù tần số của nó chia hết cho`k`.
