---
title: "CF 102348H - Triển vọng Berland"
description: "Chúng ta có n đèn lồng được đặt ở tọa độ nguyên tăng dần x[0], x[1], ..., x[n-1]. Chúng tôi có thể chọn bất kỳ tập hợp con nào trong số chúng để bật."
date: "2026-08-16T16:07:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "H"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 354
verified: true
draft: false
---

[CF 102348H - Triển vọng Berland](https://codeforces.com/problemset/problem/102348/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 54 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`đèn lồng được đặt ở tọa độ nguyên tăng dần`x[0], x[1], ..., x[n-1]`. Chúng tôi có thể chọn bất kỳ tập hợp con nào trong số chúng để bật. Nếu ít nhất ba chiếc đèn lồng được chọn, tọa độ của chúng phải tạo thành một cấp số cộng, nghĩa là mỗi cặp được chọn liên tiếp đều có khoảng cách như nhau. Không có, một hoặc hai chiếc đèn lồng được chọn, không có hạn chế. 

Nhiệm vụ là tìm số lượng đèn lồng lớn nhất có thể có tọa độ tạo thành một cấp số cộng như vậy. 

Số lượng đèn lồng nhiều nhất là`3000`. Một thuật toán bậc hai thực hiện khoảng`9,000,000`chuyển đổi trạng thái, điều này hợp lý cho việc triển khai được biên dịch và cũng có thể được thực hiện trong Python với bộ lưu trữ nhỏ gọn và tra cứu tọa độ theo thời gian không đổi. Một thuật toán bậc ba sẽ thực hiện theo thứ tự`27,000,000,000`chuyển tiếp cơ bản trong trường hợp xấu nhất, vượt xa giới hạn hai giây. Tọa độ có thể lớn bằng`10^18`, do đó, giải pháp không được dựa vào các mảng tọa độ nhỏ mà các số nguyên Python xử lý trực tiếp số học. 

Có một số trường hợp việc thực hiện bất cẩn có thể thất bại. Ví dụ, với chính xác ba đèn lồng, đầu vào```
3
1 2 4
```có câu trả lời`2`, bởi vì`1, 2, 4`không phải là một cấp số cộng và hai đèn lồng bất kỳ vẫn được phép. Một giải pháp giả sử ba chiếc đèn lồng phải luôn tạo thành một tiến trình sẽ in sai`3`. 

Quá trình tiến triển không nhất thiết phải sử dụng đèn lồng liên tiếp từ đầu vào. Vì```
5
1 2 4 6 7
```câu trả lời là`3`, sử dụng tọa độ`1, 4, 7`. Một phương pháp chỉ kiểm tra tọa độ đầu vào liền kề sẽ thấy các khoảng trống`1, 2, 2, 1`và có thể kết luận không chính xác rằng không có sự tiến triển hữu ích nào. 

Người tiền nhiệm được yêu cầu cũng có thể nằm ngoài phạm vi tọa độ. Vì```
3
0 10 20
```câu trả lời là`3`, trong khi đối với```
3
0 10 21
```câu trả lời là`2`. Khi kiểm tra xem một cặp có thể được mở rộng hay không, tiền thân được tính toán`2*x[i] - x[j]`có thể âm hoặc có thể vượt quá`10^18`. Những giá trị như vậy đơn giản là không có trong bản đồ tọa độ, vì vậy việc tra cứu phải xử lý sự vắng mặt thay vì giả sử tọa độ là hợp lệ. 

Ranh giới tọa độ trên cũng vô hại. Vì```
3
0 500000000000000000 1000000000000000000
```câu trả lời là`3`. Số học sử dụng các giá trị xung quanh`10^18`, nhưng không cần lưu trữ theo chỉ mục tọa độ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chọn hai đèn lồng đầu tiên của tiến trình mong muốn. Một khi tọa độ của chúng là`a`Và`b`, sự khác biệt chung được cố định là`d = b - a`, vì vậy mọi tọa độ sau này đều bị buộc phải:`b + d`, sau đó`b + 2d`, vân vân. Nếu chúng ta tìm kiếm toàn bộ mảng cho mọi tọa độ bắt buộc tiếp theo, chúng ta có thể thử tất cả`O(n^2)`bắt đầu cặp và chi tiêu`O(n)`công việc mở rộng mỗi cái. Điều này mang lại`O(n^3)`thời gian, đại khái`3000^3 = 27 billion`kiểm tra trong trường hợp xấu nhất. 

Nút thắt khối tương tự xuất hiện nếu chúng ta xây dựng một chương trình động nhưng tìm kiếm phần trước bằng cách quét tất cả các đèn lồng trước đó. Đối với mỗi cặp`(i, j)`, chúng ta sẽ cần tìm một chỉ mục`k`thỏa mãn`x[k] = 2*x[i] - x[j]`. 

Cấu trúc của phương trình mang lại sự tối ưu hóa chính. Tọa độ trước đó được xác định duy nhất bởi hai tọa độ hiện tại. Vì tất cả các tọa độ đều khác nhau nên chúng ta có thể xây dựng một từ điển từ tọa độ đến chỉ mục của nó và tìm`k`trong dự kiến`O(1)`thời gian. 

Định nghĩa`dp[i][j]`vì`i < j`là độ dài của cấp số cộng dài nhất có hai đèn lồng cuối cùng là`i`Và`j`. Nếu tiến trình có đèn lồng trước đó`k`, sau đó`x[i] - x[k] = x[j] - x[i]`. 

Sắp xếp lại mang lại`x[k] = 2*x[i] - x[j]`. 

Bởi vì`x[k] < x[i]`, bất kỳ thứ gì như vậy`k`tự động sớm hơn`i`. Như vậy, khi tính toán`dp[i][j]`, trạng thái cần thiết`dp[k][i]`đã được tính toán rồi. 

Nếu cặp tiền thân không tồn tại thì cặp`(i, j)`chính nó là một cấp số cộng của độ dài`2`, vậy trạng thái của nó là`2`. Nếu tiền thân tồn tại, chúng tôi sẽ mở rộng tiến trình tốt nhất kết thúc tại`(k, i)`bằng một chiếc đèn lồng. 

Điều này biến vấn đề thành`O(n^2)`lập trình động. Vấn đề thực tế duy nhất dành riêng cho Python là bộ nhớ. Ma trận Python đầy đủ gồm các số nguyên thông thường lớn một cách không cần thiết, do đó việc triển khai sẽ lưu trữ các giá trị DP trong một mảng ngắn không dấu nhỏ gọn. Vì câu trả lời không bao giờ có thể vượt quá`3000`, mọi trạng thái đều phù hợp thoải mái trong 16 bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^3)`|`O(n)`| Quá chậm | 
| DP tối ưu |`O(n^2)`|`O(n^2)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ và tạo từ điển`pos`ánh xạ mọi tọa độ vào chỉ mục của nó. Các tọa độ là duy nhất nên mỗi lần tra cứu đều có chính xác một chỉ mục có thể. 
2. Phân bổ`dp[i][j]`cho mỗi cặp`i < j`. Ban đầu, mỗi cặp biểu diễn một cấp số cộng có độ dài`2`, vậy giá trị của nó là`2`. 
3. Xử lý chỉ số ở giữa có thể`i`từ trái sang phải. Đối với mọi`j > i`, tính tọa độ phải đứng trước`x[i]`nếu như`(x[i], x[j])`là hai số hạng cuối cùng của một cấp số cộng. tọa độ đó là`target = 2*x[i] - x[j]`. 
4. Tra cứu`target`TRONG`pos`. Nếu nó vắng mặt thì không có cách nào để mở rộng cặp`(i, j)`, Vì thế`dp[i][j]`còn lại`2`. 
5. Nếu`target`tồn tại ở chỉ mục`k`, sau đó`k < i`bởi vì`x[j] > x[i]`ngụ ý`2*x[i] - x[j] < x[i]`. Quá trình tiến triển kết thúc ở`k, i`đã có sự khác biệt chung chính xác, vì vậy hãy đặt`dp[i][j] = dp[k][i] + 1`. 
6. Giữ giá trị DP tối đa được nhìn thấy. Giá trị đó là số lượng đèn lồng tối đa có thể được bật. 

### Tại sao nó hoạt động 

Điều bất biến là sau`dp[i][j]`được tính toán, nó chính xác là cấp số cộng dài nhất kết thúc bằng đèn lồng`i`Và`j`. Bất kỳ tiến trình nào kết thúc ở đó phải có tọa độ trước đó bằng`2*x[i] - x[j]`, do đó có nhiều nhất một khả năng có thể xảy ra trước nó. Nếu có tọa độ đó thì mọi tiến trình hợp lệ sẽ kết thúc tại`(i, j)`thu được bằng cách mở rộng một tiến trình hợp lệ kết thúc tại`(k, i)`. Nếu nó vắng mặt, không có tiến trình nào có độ dài ít nhất ba có thể kết thúc tại`(i, j)`, để lại độ dài cơ sở`2`. Từ`k < i`, trạng thái cần thiết đã được tính toán. Do đó, việc lấy giá trị tối đa trên tất cả các cặp sẽ xem xét mọi cấp số cộng có thể có. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n = int(input())
    x = list(map(int, input().split()))

    pos = {value: i for i, value in enumerate(x)}

    # Every pair initially represents a progression of length 2.
    # n <= 3000, so every answer fits into an unsigned short.
    dp = array('H', [2]) * (n * n)

    ans = 2

    for i in range(n):
        xi = x[i]
        row = i * n

        for j in range(i + 1, n):
            target = 2 * xi - x[j]
            k = pos.get(target)

            if k is not None:
                length = dp[k * n + i] + 1
                dp[row + j] = length
                if length > ans:
                    ans = length

    print(ans)

if __name__ == "__main__":
    solve()
```Từ điển`pos`là cấu trúc tra cứu theo thời gian không đổi từ thuật toán. biểu thức`2 * xi - x[j]`là tọa độ chính xác của tiền thân bị ép buộc bởi điều kiện cấp số cộng. 

DP được làm phẳng thành một chiều`array('H')`. Trạng thái tương ứng với`(i, j)`được lưu trữ tại`i * n + j`. Một mảng phẳng tránh được chi phí lớn cho mỗi đối tượng mà danh sách Python gồm hàng triệu số nguyên Python sẽ đưa ra. 

Mọi tiểu bang đều bắt đầu lúc`2`, bởi vì bất kỳ cặp tọa độ riêng biệt nào cũng là tập hợp được chọn hợp lệ. Khi tồn tại trạng thái tiền nhiệm, trạng thái sẽ bị ghi đè bằng trạng thái trước đó cộng với một. 

Thứ tự vòng lặp quan trọng. Đối với nhà nước`(i, j)`, tiền thân là`k < i`, Vì thế`dp[k][i]`thuộc về hàng trước đó và đã được tính toán. Mã không cần phải tìm kiếm`k`bằng tay bởi vì`pos`trực tiếp cung cấp nó. 

Không có vấn đề tràn số nguyên trong Python. Tọa độ trung gian lớn nhất chỉ khoảng`2 * 10^18`, số nguyên Python thể hiện chính xác. 

Câu trả lời được khởi tạo thành`2`. Từ`n >= 3`, một cặp hợp lệ luôn tồn tại và một cấp số nhân có độ dài`2`luôn được cho phép. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
3
1 2 3
```các chuyển đổi DP có liên quan là: 

|`i`|`j`|`target`|`k`|`dp[i][j]`| 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | vắng mặt | 2 | 
| 0 | 2 | -1 | vắng mặt | 2 | 
| 1 | 2 | 1 | 0 | 3 | 

Khi xử lý`(1, 2)`, tọa độ trước yêu cầu là`2*2 - 3 = 1`, đó là chiếc đèn lồng đầu tiên. Trạng thái đã được tính toán`dp[0][1] = 2`kéo dài theo chiều dài`3`. Câu trả lời là do đó`3`. 

Đối với mẫu 2,```
5
1 2 4 6 7
```dấu vết cặp đầy đủ là: 

|`i`|`j`|`target`|`k`|`dp[i][j]`| 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | vắng mặt | 2 | 
| 0 | 2 | -2 | vắng mặt | 2 | 
| 0 | 3 | -4 | vắng mặt | 2 | 
| 0 | 4 | -5 | vắng mặt | 2 | 
| 1 | 2 | 0 | vắng mặt | 2 | 
| 1 | 3 | -2 | vắng mặt | 2 | 
| 1 | 4 | -3 | vắng mặt | 2 | 
| 2 | 3 | 2 | 1 | 3 | 
| 2 | 4 | 1 | 0 | 3 | 
| 3 | 4 | 5 | vắng mặt | 2 | 

Cặp đôi`(2, 4)`tương ứng với tọa độ`4`Và`7`. Người tiền nhiệm bắt buộc của nó là`1`, được tìm thấy tại chỉ mục`0`, do đó nó hình thành sự tiến triển`1, 4, 7`chiều dài`3`. Không có cặp nào kéo dài đến chiều dài`4`, đưa ra câu trả lời cần thiết`3`. 

Những dấu vết này cũng chứng minh tại sao phần trước không cần phải liền kề với cặp hiện tại trong đầu vào. Quá trình chuyển đổi sử dụng tọa độ thay vì các chỉ số lân cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n^2)`| Mỗi cặp đặt hàng`i < j`được xử lý một lần, với thời gian dự kiến`O(1)`tra cứu từ điển. | 
| Không gian |`O(n^2)`| DP dẹt chứa`n^2`trạng thái 16 bit nhỏ gọn, cộng với từ điển tọa độ. | 

Vì`n = 3000`, có ít hơn`4.5 million`cặp với`i < j`, mặc dù ô vuông được phân bổ DP chứa`9 million`mục nhỏ gọn. Với hai byte cho mỗi mục nhập, DP chiếm khoảng 18 MB, thấp hơn giới hạn bộ nhớ 512 MB. Số lần chuyển đổi bậc hai là thang đo dự kiến ​​cho ràng buộc này, trong khi các phương án thay thế bậc ba lại quá đắt. 

## Trường hợp thử nghiệm```python
# This test harness uses the same solve logic as the submitted solution.
import sys
import io
from array import array

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    n = data[0]
    x = data[1:1 + n]

    pos = {value: i for i, value in enumerate(x)}
    dp = array('H', [2]) * (n * n)

    ans = 2

    for i in range(n):
        xi = x[i]
        row = i * n

        for j in range(i + 1, n):
            target = 2 * xi - x[j]
            k = pos.get(target)

            if k is not None:
                length = dp[k * n + i] + 1
                dp[row + j] = length
                if length > ans:
                    ans = length

    return str(ans)

# Provided samples
assert solve_data("""3
1 2 3
""") == "3", "sample 1"

assert solve_data("""5
1 2 4 6 7
""") == "3", "sample 2"

assert solve_data("""10
5 10 15 20 35 60 80 85 110 120
""") == "5", "sample 3"

# Minimum n, but the three coordinates are not an arithmetic progression.
assert solve_data("""3
1 2 4
""") == "2", "minimum size with no 3-term progression"

# A progression uses non-consecutive input positions.
assert solve_data("""7
0 1 4 7 8 10 13
""") == "3", "non-consecutive progression"

# Boundary coordinates near 0 and 10^18.
assert solve_data("""3
0 500000000000000000 1000000000000000000
""") == "3", "coordinate boundaries"

# Maximum-size valid input: all 3000 coordinates form one progression.
n = 3000
maximum_case = str(n) + "\n" + " ".join(map(str, range(n))) + "\n"
assert solve_data(maximum_case) == "3000", "maximum size"

# Equal coordinates are not a valid input because the statement requires
# x[i] < x[i+1]. The closest meaningful test is equal consecutive gaps.
assert solve_data("""6
10 20 30 40 50 60
""") == "6", "all equal gaps"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 2 4`|`2`| Kích thước đầu vào tối thiểu và quy tắc đặc biệt là hai đèn lồng luôn hợp lệ. | 
|`7 / 0 1 4 7 8 10 13`|`3`| Một tiến trình có thể bỏ qua các vị trí đầu vào. | 
|`3 / 0 500000000000000000 1000000000000000000`|`3`| Tọa độ rất lớn và các điểm cuối của phạm vi tọa độ. | 
|`3000 / 0 1 2 ... 2999`|`3000`| Tối đa`n`, câu trả lời DP tối đa và hiệu suất. | 
|`6 / 10 20 30 40 50 60`|`6`| Mỗi tọa độ thuộc về một cấp số cộng và nhấn mạnh các chuyển đổi thành công lặp đi lặp lại. | 

Trường hợp "tất cả các giá trị bằng nhau" được yêu cầu không thể là phép thử hợp lệ trong điều kiện đầu vào của bài toán, vì tọa độ đèn lồng đang tăng nghiêm ngặt. Việc sử dụng tọa độ trùng lặp sẽ kiểm tra hành vi bên ngoài thông số kỹ thuật. Các khoảng trống bằng nhau là cách giải thích hợp lệ cho trường hợp đặc biệt đó và thử nghiệm tùy chỉnh cuối cùng sẽ kiểm tra chính xác tình huống đó. 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
3
1 2 4
```thuật toán khởi tạo mỗi cặp để`2`. Đối với cặp`(1, 2)`, số tiền trước cần thiết là`0`, cái đó vắng mặt. Các cặp khác cũng không có tiền thân hợp lệ nên số lượng tối đa vẫn còn`2`. Điều này đúng vì chọn hai chiếc đèn lồng bất kỳ bao giờ cũng đẹp. 

Đối với một tiến trình bỏ qua đèn lồng, hãy xem xét```
5
1 2 4 6 7
```Khi thuật toán đạt tọa độ`4`Và`7`, nó tính toán`2*4 - 7 = 1`. Tọa độ`1`tồn tại, vì vậy`dp[2][4] = dp[0][2] + 1 = 3`. Tọa độ được chọn là`1, 4, 7`. Tọa độ đầu vào trung gian`2`Và`6`không thành vấn đề vì bài toán yêu cầu một tập hợp con chứ không phải một đoạn liền kề của mảng. 

Đối với tọa độ biên,```
3
0 500000000000000000 1000000000000000000
```cặp bao gồm hai tọa độ cuối cùng đã yêu cầu tiền thân`0`. Từ điển tìm thấy nó ngay lập tức, cho biết độ dài`3`. Python thực hiện phép nhân và phép trừ một cách chính xác, do đó các giá trị tọa độ lớn không yêu cầu bất kỳ xử lý số học đặc biệt nào. 

Đối với tọa độ không thể có tọa độ trước, hãy xem xét```
3
0 10 21
```Đối với cặp`10, 21`, số tiền trước cần thiết là`-1`, không có trong từ điển. Đối với cặp`0, 10`, số tiền trước cần thiết là`-10`, cũng vắng mặt Mọi tiểu bang đều ở lại`2`, vậy câu trả lời là`2`. Việc tra cứu từ điển xử lý các mục tiêu nằm ngoài khoảng tọa độ cho phép một cách tự nhiên. 

Đối với tiến trình có kích thước tối đa,```
3000
0 1 2 3 ... 2999
```mỗi cặp mà sự khác biệt của nó có thể được tiếp tục đều có một cặp đứng trước hợp lệ. DP liên tục mở rộng các trạng thái hiện có, cuối cùng đạt tới chiều dài`3000`. nhỏ gọn`array('H')`sự đại diện là đủ vì không có trạng thái nào có thể vượt quá`3000`.
