---
title: "CF 102700N - Đặt tên cho vấn đề này"
description: "Jolany có một năm gồm n ngày. Mỗi ngày cô ấy có thể thử cỗ máy thời gian một lần. Nếu máy thành công, cô ấy sẽ đến ngay sinh nhật tiếp theo của mình."
date: "2026-08-10T06:03:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "N"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 305
verified: true
draft: false
---

[CF 102700N - Đặt tên cho vấn đề này](https://codeforces.com/problemset/problem/102700/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Jolany có một năm bao gồm`n`ngày. Mỗi ngày cô ấy có thể thử cỗ máy thời gian một lần. Nếu máy thành công, cô ấy sẽ đến ngay sinh nhật tiếp theo của mình. Xác suất thành công được cho trước`n`giá trị, nhưng thứ tự của chúng được hoán vị ngẫu nhiên, nên mọi hoán vị đều có khả năng xảy ra như nhau. 

Đối với một đơn đặt hàng cố định`p_1, p_2, ..., p_n`, cho phép`T`là số ngày cô ấy chờ đợi. Nếu máy bị lỗi lần đầu`k`mấy ngày nay cô ấy đã đợi rồi`k`ngày. Một dạng kỳ vọng thuận tiện là 

[ 
E[T] = \sum_{k=1}^{n}\Pr(T\ge k). 
]

Vì`T >= k`, cái đầu tiên`k`mọi nỗ lực đều phải thất bại. Nếu chúng ta viết 

[ 
q_i=1-p_i, 
] 

sau đó cho một hoán vị cố định, 

\sum_{k=1}^{n}q_1q_2\cdots q_k. 
] 

Nhiệm vụ là tính trung bình số lượng này trên tất cả`n!`những hoán vị có thể có. 

Những ràng buộc chính thức cho phép`n`lên đến`5000`, với giới hạn thời gian 1 giây và bộ nhớ 512 MB. Một thuật toán thời gian giai thừa ngay lập tức là không thể thực hiện được, và thậm chí`O(n^3)`chương trình động sẽ quá lớn. MỘT`O(n^2)`thuật toán là mục tiêu tự nhiên, yêu cầu khoảng 12,5 triệu cập nhật trạng thái khi`n=5000`. Giới hạn bộ nhớ dễ dàng điều chỉnh DP một chiều. 

Có một số trường hợp ranh giới bộc lộ những lỗi phổ biến. Với```
1
0
```máy luôn bị lỗi nên câu trả lời là chính xác`1`. Công thức chỉ tính số lần thử thành công có thể trả về số 0 không chính xác. Với```
1
1
```câu trả lời là`0`, vì lần thử đầu tiên luôn thành công. Với```
2
0 1
```hai lệnh có khả năng như nhau là`[0,1]`Và`[1,0]`. Thời gian chờ đợi của họ là`1`Và`0`, vậy câu trả lời là`0.5`. Một giải pháp coi xác suất là các biến ngẫu nhiên độc lập thay vì lấy mẫu mà không thay thế sẽ sai trong trường hợp này. Cuối cùng, xác suất trùng lặp không được loại bỏ. Quá trình ngẫu nhiên là sự hoán vị của các vị trí, do đó, ngay cả những giá trị bằng nhau vẫn chiếm các vị trí khác nhau trong`n!`hoán vị có khả năng như nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi hoán vị của mảng xác suất. Đối với mỗi hoán vị, hãy quét số ngày của nó và tính thời gian chờ dự kiến ​​bằng cách sử dụng xác suất thất bại. Điều này đúng vì nó đánh giá rõ ràng mọi thứ tự ẩn có thể có với xác suất được quy định chính xác. Thật không may, có`n!`hoán vị và đánh giá người ta mất`O(n)`thời gian, cho`O(n · n!)`hoạt động. Thậm chí`n=10`đã có nghĩa là khoảng 36 triệu hoạt động cơ bản ở cấp độ ban ngày, trong khi`n=5000`làm cho phương pháp này hoàn toàn không khả thi. 

Quan sát hữu ích là chúng ta không thực sự quan tâm đến thứ tự chính xác của biểu thức đầu tiên.`k`xác suất. Trong thời hạn 

[ 
q_1q_2\cdots q_k, 
] 

chỉ có bộ đầu tiên`k`các yếu tố quan trọng. Dưới một hoán vị ngẫu nhiên thống nhất, những cái đầu tiên`k`các phần tử tạo thành ngẫu nhiên thống nhất`k`- tập hợp phần tử của mảng ban đầu. 

Cho phép`e_k`là tổng đối xứng cơ bản của bậc`k`về xác suất thất bại: 

[ 
e_k = 
\sum_{1\le i_1<i_2<\cdots<i_k\le n} 
q_{i_1}q_{i_2}\cdots q_{i_k}. 
] 

Có chính xác 

[ 
\binom nk 
] 

tập hợp có thể có của đầu tiên`k`các vị trí, tất cả đều có khả năng như nhau. Như vậy 

\frac{e_k}{\binom nk}. 
] 

Câu trả lời cuối cùng là do đó 

[ 
\sum_{k=1}^{n}\frac{e_k}{\binom nk}. 
] 

Các tổng đối xứng cơ bản có thể được tính bằng đa thức một chiều tiêu chuẩn DP. Tuy nhiên việc lưu trữ`e_k`trực tiếp là bất tiện về mặt số lượng vì những giá trị này có thể lớn bằng`\binom nk`. Thay vào đó, chúng tôi duy trì phiên bản chuẩn hóa trong khi xử lý từng xác suất một. 

Sau khi xử lý`m`xác suất thất bại, xác định 

[ 
dp_k = 
\frac{e_k^{(m)}}{\binom mk}, 
] 

ở đâu`e_k^{(m)}`là bằng cấp-`k`tổng đối xứng cơ bản của chúng`m`các giá trị. Mọi`dp_k`là trung bình tích của các số trong`[0,1]`, vậy nó cũng nằm trong`[0,1]`. 

Giả sử xác suất thất bại tiếp theo là`q`. Sự tái phát đối xứng cơ bản thông thường là 

e_k^{(m-1)}+q e_{k-1}^{(m-1)}. 
] 

Sau khi chia cho`\binom mk`, hai số hạng thu được các hệ số đơn giản: 

\frac{m-k}{m}dp_k^{(m-1)} 
+ 
\frac{k}{m}q,dp_{k-1}^{(m-1)}. 
] 

Điều này mang lại một`O(n^2)`thuật toán chỉ sử dụng`O(n)`ký ức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n · n!)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n²)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quy đổi mọi xác suất thành công`p_i`vào xác suất thất bại của nó`q_i = 1-p_i`. Thời gian chờ đợi dự kiến ​​được thể hiện một cách tự nhiên thông qua các lần thất bại liên tiếp, vì vậy đây là số lượng mà DP cần. 
2. Khởi tạo`dp[0] = 1`và mọi thứ khác`dp[k] = 0`. Trước khi xử lý bất kỳ xác suất nào, tích trống sẽ được`1`, trong khi không tồn tại tập hợp con có kích thước dương. 
3. Xử lý từng xác suất lỗi một. Giả sử số lượng giá trị được xử lý hiện tại là`m`. Đối với mọi`k`từ`m`xuống`1`, cập nhật 

\frac{(m-k)dp_k+kq_mdp_{k-1}}{m}. 
] 

Vòng lặp phải quay ngược lại vì`dp[k-1]`vẫn phải tham chiếu đến trạng thái từ số lượng giá trị được xử lý trước đó. Việc cập nhật chuyển tiếp sẽ ghi đè lên trạng thái đó trước khi nó được sử dụng. 
4. Rốt cuộc`n`xác suất đã được xử lý,`dp[k]`bằng 

[ 
\frac{e_k}{\binom nk}, 
] 

đó chính xác là kết quả mong đợi của xác suất thất bại trong lần đầu tiên`k`các vị trí hoán vị ngẫu nhiên đều. 
5. Tổng`dp[1]`bởi vì`dp[n]`. Theo công thức tổng đuôi cho thời gian chờ có giá trị nguyên không âm, tổng này là số ngày Jolany chờ đợi dự kiến. 

### Tại sao nó hoạt động 

Sau khi xử lý`m`giá trị, thì bất biến là`dp[k]`bằng tích trung bình của mọi`k`-tập hợp phần tử của chúng`m`xác suất thất bại. Khi giá trị mới`q`được thêm vào, một`k`-tập hợp phần tử hoặc loại trừ`q`, trong trường hợp đó nó đến từ cái cũ`m-1`giá trị hoặc bao gồm`q`, trong trường hợp đó nó khác`k-1`các yếu tố đến từ các giá trị cũ. có`m-k`các lựa chọn thuộc loại đầu tiên liên quan đến mức trung bình chuẩn hóa và`k`các lựa chọn được biểu thị bằng loại thứ hai, đưa ra chính xác sự lặp lại ở trên. Tại`m=n`, mọi`k`-tập hợp phần tử có khả năng chiếm phần tử đầu tiên như nhau`k`vị trí của một hoán vị ngẫu nhiên. Như vậy`dp[k]`là xác suất kỳ vọng mà tất cả đều đầu tiên`k`những nỗ lực thất bại. Tổng hợp những xác suất đó sẽ cho ra thời gian chờ đợi dự kiến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(float, input().split()))

    # dp[k] = average product of all k-element subsets
    # among the probabilities processed so far.
    dp = [0.0] * (n + 1)
    dp[0] = 1.0

    m = 0
    for prob in p:
        q = 1.0 - prob
        m += 1
        inv_m = 1.0 / m

        # Descending order is required because dp[k - 1]
        # must still contain the previous-m state.
        for k in range(m, 0, -1):
            dp[k] = ((m - k) * dp[k] + k * q * dp[k - 1]) * inv_m

    answer = sum(dp[1:])
    print(f"{answer:.12f}")

if __name__ == "__main__":
    solve()
```Mảng`dp`lưu trữ các tổng đối xứng cơ bản được chuẩn hóa thay vì tổng thô. Điều này tránh được các giá trị trung gian rất lớn. Ví dụ, nếu mọi xác suất thất bại là`1`, hệ số độ thô-2500 sẽ là`\binom{5000}{2500}`, nằm ngoài phạm vi hữu ích của các số dấu phẩy động thông thường, trong khi giá trị chuẩn hóa của nó chỉ đơn giản là`1`. 

Vòng ngoài tăng lên`m`từ`1`ĐẾN`n`. Tại thời điểm đó, vòng lặp bên trong sẽ xem xét mọi kích thước tập hợp con có thể tồn tại trong số tập hợp con đầu tiên.`m`các giá trị. Thứ tự giảm dần là chi tiết triển khai chính. Ví dụ, trong khi tính toán mới`dp[3]`,`dp[2]`vẫn phải là giá trị thu được sau khi xử lý`m-1`các phần tử. 

biểu thức```
((m - k) * dp[k] + k * q * dp[k - 1]) * inv_m
```chính xác là sự tái phát chuẩn hóa. sử dụng`inv_m`tránh thực hiện hai phép chia cho mỗi trạng thái. Tất cả các giá trị nằm trong khoảng`[0,1]`lên đến làm tròn dấu phẩy động thông thường, do đó không có tràn số nguyên và không cần duy trì hệ số dấu phẩy động lớn. 

Đầu vào bao gồm một trường hợp thử nghiệm duy nhất. Định dạng đầu vào thực tế của câu lệnh bắt đầu bằng`n`, theo sau là`n`xác suất. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
1
0.2
```xác suất thất bại là`0.8`. Chỉ có một kích thước tập hợp con để xem xét. 

|`m`|`q`|`k`|`dp[k]`sau khi cập nhật | 
| --- | --- | --- | --- | 
| 0 | | 0 |`1`| 
| 1 |`0.8`| 1 |`0.8`| 

Câu trả lời cuối cùng là`dp[1] = 0.8`. Máy bị lỗi có xác suất`0.8`, và trong trường hợp đó Jolany đợi một ngày, vì vậy điều này phù hợp với cách giải thích trực tiếp của mẫu. 

Đối với mẫu 2,```
2
0.5 1.0
```xác suất thất bại là`0.5`Và`0`. 

|`m`|`q`|`k`|`dp[k]`sau khi cập nhật | 
| --- | --- | --- | --- | 
| 0 | | 0 |`1`| 
| 1 |`0.5`| 1 |`0.5`| 
| 2 |`0`| 2 |`0`| 
| 2 |`0`| 1 |`0.25`| 

Cuối cùng,`dp[1]=0.25`Và`dp[2]=0`, cho`0.25`. Điều này phù hợp với hai đơn đặt hàng có thể:`[0.5,1]`đưa ra sự chờ đợi dự kiến`0.5`, trong khi`[1,0.5]`cho`0`, vậy trung bình của họ là`0.25`. 

Đối với mẫu 3,```
3
0.3 0.1 0.1
```xác suất thất bại là`0.7`,`0.9`, Và`0.9`. 

|`m`|`q`|`dp[1]`|`dp[2]`|`dp[3]`| 
| --- | --- | --- | --- | --- | 
| 1 |`0.7`|`0.700000`|`0`|`0`| 
| 2 |`0.9`|`0.800000`|`0.630000`|`0`| 
| 3 |`0.9`|`0.833333`|`0.690000`|`0.567000`| 

Tổng số tiền là 

[ 
0,8333333333+0,69+0,567=2,0903333333, 
] 

cung cấp đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n²)`| Đối với`m`- xác suất thứ, chính xác`m`Trạng thái DP được cập nhật, đưa ra`1+2+...+n`hoạt động. | 
| Không gian |`O(n)`| Chỉ có một chiều`dp`mảng và xác suất đầu vào được lưu trữ. | 

Vì`n=5000`, DP thực hiện khoảng 12,5 triệu cập nhật trạng thái. Biểu diễn chuẩn hóa giữ cho mọi trạng thái bị giới hạn, vì vậy số học dấu phẩy động thông thường của Python là đủ cho yêu cầu`10^-6`khả năng chịu lỗi. các`O(n)`mức sử dụng bộ nhớ rất nhỏ so với giới hạn 512 MB. Ràng buộc đã nêu là`n <= 5000`. 

## Trường hợp thử nghiệm 

Khai thác kiểm tra sau đây sử dụng logic DP giống như giải pháp đã gửi và so sánh các câu trả lời dấu phẩy động với dung sai thay vì yêu cầu biểu diễn thập phân giống hệt nhau.```python
import sys
import io
import math

input = sys.stdin.readline

def solve_data(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    p = [float(next(it)) for _ in range(n)]

    dp = [0.0] * (n + 1)
    dp[0] = 1.0

    m = 0
    for prob in p:
        q = 1.0 - prob
        m += 1
        inv_m = 1.0 / m

        for k in range(m, 0, -1):
            dp[k] = ((m - k) * dp[k] + k * q * dp[k - 1]) * inv_m

    return f"{sum(dp[1:]):.12f}"

def run(inp: str) -> str:
    return solve_data(inp)

def check(inp: str, expected: float, message: str):
    actual = float(run(inp))
    assert math.isclose(actual, expected, rel_tol=1e-9, abs_tol=1e-9), (
        f"{message}: got {actual}, expected {expected}"
    )

# Provided samples
check(
    "1\n0.2\n",
    0.8,
    "sample 1",
)

check(
    "2\n0.5 1.0\n",
    0.25,
    "sample 2",
)

check(
    "3\n0.3 0.1 0.1\n",
    2.090333333333,
    "sample 3",
)

# Minimum size, machine always fails.
check(
    "1\n0\n",
    1.0,
    "minimum size, zero probability",
)

# Minimum size, machine always succeeds.
check(
    "1\n1\n",
    0.0,
    "minimum size, probability one",
)

# Different permutations produce different waiting times.
check(
    "2\n0 1\n",
    0.5,
    "zero and one probability",
)

# All equal probabilities: n=3, p=0.5.
# Every ordering is identical, so E = 0.5 + 0.25 + 0.125 = 0.875.
check(
    "3\n0.5 0.5 0.5\n",
    0.875,
    "all equal",
)

# Maximum-size input, every attempt succeeds immediately.
check(
    "5000\n" + "1 " * 5000 + "\n",
    0.0,
    "maximum n",
)

# Maximum-size input, every attempt fails.
check(
    "5000\n" + "0 " * 5000 + "\n",
    5000.0,
    "maximum n, all failures",
)

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`1.0`| Kích thước tối thiểu và thất bại được đảm bảo | 
|`1 / 1`|`0.0`| Kích thước tối thiểu và thành công được đảm bảo | 
|`2 / 0 1`|`0.5`| Hành vi phụ thuộc vào thứ tự và tính trung bình hoán vị | 
|`3 / 0.5 0.5 0.5`|`0.875`| Tất cả các giá trị bằng nhau | 
|`5000 / all 1`|`0.0`| Tối đa`n`và thành công ngay lập tức | 
|`5000 / all 0`|`5000.0`| Tối đa`n`, đảm bảo thất bại, và cuối cùng`k=n`hạn | 

## Vỏ cạnh 

Khi máy có xác suất`0`, nó luôn thất bại. Đối với đầu vào```
1
0
```thuật toán chuyển đổi`p=0`ĐẾN`q=1`. Tại`m=1,k=1`, bản cập nhật mang lại`dp[1]=1`, vậy câu trả lời là`1`. DP bao gồm xác suất sống sót trong suốt những ngày có sẵn, đây chính xác là phần mà phép tính chỉ thành công sẽ bỏ lỡ. 

Khi máy có xác suất`1`, nó luôn thành công. Vì```
1
1
```xác suất thất bại là`q=0`. Trạng thái duy nhất trở thành`dp[1]=0`, đưa ra câu trả lời`0`. Điều này cũng chứng tỏ tại sao tích của xác suất thất bại, chứ không phải tích của xác suất thành công, lại là đại lượng chính xác. 

Vì```
2
0 1
```xác suất thất bại là`1`Và`0`. Sau giá trị đầu tiên,`dp[1]=1`. Sau khi thêm giá trị thứ hai, 
[ 
dp_1=\frac{1\cdot+1\cdot 0\cdot 1}{2}=0,5, 
] 
trong khi 
[ 
dp_2=0. 
] 
Câu trả lời là`0.5`. Đây chính xác là mức trung bình của hai đơn hàng có thể,`[0,1]`Và`[1,0]`và xác nhận rằng DP đang lấy trung bình trên các tập hợp con mà không giả định sự độc lập giữa các vị trí. 

Đối với xác suất bằng nhau như```
3
0.5 0.5 0.5
```hoán vị không thay đổi bất cứ điều gì. Xác suất thất bại luôn là`0.5`, vậy thời gian chờ dự kiến là 

[ 
0,5+0,25+0,125=0,875. 
] 

DP sản xuất`dp[1]=0.5`,`dp[2]=0.25`, Và`dp[3]=0.125`, xác nhận rằng các tổng đối xứng được chuẩn hóa sẽ giảm về lũy thừa thông thường khi tất cả các giá trị đều bằng nhau. 

Cuối cùng, khi`n=5000`và mọi xác suất đều là`0`, mọi`q_i`là`1`. Mỗi sản phẩm tập hợp con cũng`1`, vì vậy mỗi trạng thái DP chuẩn hóa là`1`và câu trả lời cuối cùng là`5000`. Đây là một trường hợp căng thẳng hữu ích vì nó đạt đến độ dài năm lớn nhất cho phép trong khi vẫn giữ được câu trả lời đúng về mặt toán học ở giá trị tối đa có thể. DP được chuẩn hóa sẽ xử lý nó mà không bao giờ xây dựng các hệ số nhị thức khổng lồ mà việc triển khai tổng đối xứng cơ bản thô sẽ cần.
