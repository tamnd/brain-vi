---
title: "CF 102348H - Triển vọng Berland"
description: "Những chiếc đèn lồng được cho dưới dạng tọa độ tăng dần, vì vậy thứ tự đầu vào của chúng đã là thứ tự của chúng dọc theo đường phố. Chúng ta cần chọn dãy con lớn nhất của các tọa độ này để tạo thành một cấp số cộng."
date: "2026-08-14T12:11:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "H"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 1176
verified: false
draft: false
---

[CF 102348H - Triển vọng Berland](https://codeforces.com/problemset/problem/102348/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 19 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Những chiếc đèn lồng được cho dưới dạng tọa độ tăng dần, vì vậy thứ tự đầu vào của chúng đã là thứ tự của chúng dọc theo đường phố. Chúng ta cần chọn dãy con lớn nhất của các tọa độ này để tạo thành một cấp số cộng. Nếu chúng ta chọn tọa độ (a_1,a_2,\ldots,a_k) thì mọi khoảng cách liên tiếp phải giống nhau. Bất kỳ lựa chọn nào về một hoặc hai chiếc đèn lồng đều tự động hợp lệ, vì vậy phần thú vị sẽ bắt đầu ở độ dài thứ ba. 

Tọa độ có thể lớn bằng (10^{18}), loại trừ các phương pháp tiếp cận phụ thuộc vào phạm vi tọa độ. Tuy nhiên, chúng tôi có nhiều nhất (n=3000) đèn lồng, vì vậy thuật toán (O(n^2)) là thực tế. Có khoảng (n^2/2=4,5) triệu cặp khi (n=3000), đây là lượng công việc phù hợp cho một chương trình động bậc hai. Thuật toán (O(n^3)) sẽ thực hiện theo thứ tự (3000^3/6=4,5) tỷ lần lặp bên trong trong quá trình triển khai tự nhiên, vượt xa giới hạn hai giây. 

Việc tăng tọa độ chặt chẽ cũng mang lại cho chúng ta một thuộc tính sắp xếp hữu ích. Nếu (i<j), sai sai chung của cấp số cộng tận cùng tại (x_i,x_j) là (x_j-x_i>0). Tọa độ trước đó của nó, nếu có, phải là 

[ 
x_i-(x_j-x_i)=2x_i-x_j. 
] 

Vì (2x_i-x_j<x_i) nên tọa độ trước đó phải có chỉ số nhỏ hơn. Điều này cho phép mọi trạng thái chỉ phụ thuộc vào các trạng thái đã được tính toán. 

Có một số trường hợp đặc biệt có thể dẫn đến việc triển khai không chính xác. Với ba tọa độ liên tiếp như```
3
1 2 3
```câu trả lời là 3, vì toàn bộ mảng có các khoảng trống bằng nhau. Việc triển khai khởi tạo mọi tiến trình có độ dài 2 nhưng quên mở rộng nó khi tồn tại điểm giữa sẽ trả về 2 không chính xác. 

Trường hợp thứ hai là```
5
1 2 4 6 7
```có câu trả lời là 3. Tọa độ (1,4,7) tạo thành một cấp số cộng, mặc dù chúng không liên tiếp trong đầu vào. Giải pháp chỉ kiểm tra các đèn lồng liên tiếp sẽ bỏ lỡ điều này và trả về 2. 

Vấn đề thứ ba là một tiến trình mà tiền thân của nó vắng mặt. Ví dụ,```
3
1 2 4
```có câu trả lời 2. Đối với cặp (2,4), tọa độ bắt buộc trước đó sẽ là (0), tọa độ này không có. Cặp này vẫn là một cấp số hợp lệ có độ dài 2, do đó việc triển khai không được coi phần trước bị thiếu là một lỗi. 

Cuối cùng, tọa độ có thể cực kỳ lớn. Một biểu thức như (2x_i-x_j) có thể tạm thời rời khỏi khoảng ([0,10^{18}]), do đó, nó phải được xử lý dưới dạng số nguyên thay vì dựa vào việc lập chỉ mục mảng theo tọa độ. Số nguyên Python có độ chính xác tùy ý, điều này làm cho việc này trở nên đơn giản. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chọn hai đèn lồng đầu tiên của một tiến trình, xác định sự khác biệt của chúng và sau đó liên tục tìm kiếm tọa độ tiếp theo có cùng sự khác biệt. Có thể có (O(n^2)) cặp bắt đầu. Nếu mỗi cặp quét các tọa độ còn lại thì số lần lặp trong trường hợp xấu nhất là 

[ 
\sum_{i=0}^{n-1}\sum_{j=i+1}^{n-1}(n-j-1)=O(n^3), 
] 

với khoảng (n^3/6) hoặc 4,5 tỷ lần lặp tại (n=3000). Một bộ băm có thể làm cho mỗi tìm kiếm riêng lẻ có thời gian không đổi, nhưng nó không loại bỏ yếu tố thứ ba vì mọi cặp bắt đầu vẫn có thể tạo ra một quá trình quét dài. 

Cách tiếp cận bạo lực có hiệu quả vì khi hai tọa độ đầu tiên được cố định, toàn bộ cấp số cộng sẽ được xác định. Vấn đề là nó liên tục phát hiện ra các hậu tố giống nhau. Ví dụ: nếu một số cặp bắt đầu khác nhau cuối cùng đạt đến cùng một cặp tọa độ (x_k,x_i), thì tất cả chúng sẽ làm lại công việc cần thiết một cách độc lập để mở rộng cặp tọa độ đó. 

Quan sát quan trọng là một cặp tọa độ được chọn liên tiếp mô tả hoàn toàn trạng thái chúng ta cần cho các tiện ích mở rộng trong tương lai. Xác định (dp[i][j]), cho (i<j), là độ dài tối đa của cấp số cộng có hai tọa độ được chọn cuối cùng là (x_i,x_j). Nếu chúng ta muốn kéo dài tiến trình này về phía sau thì tọa độ trước đó của nó phải là (2x_i-x_j). Có nhiều nhất một chiếc đèn lồng như vậy vì tất cả các tọa độ đều khác nhau. 

Giả sử tọa độ đó tồn tại ở chỉ số (k<i). Sau đó, tiến trình kết thúc tại (x_i,x_j) thu được bằng cách lấy tiến trình kết thúc tại (x_k,x_i) và nối thêm (x_j). Như vậy 

[ 
dp[i][j]=dp[k][i]+1. 
] 

Nếu (2x_i-x_j) vắng mặt, cặp (x_i,x_j) vẫn có thể bắt đầu một tiến trình hợp lệ, vì vậy (dp[i][j]=2). 

Phép truy toán đưa ra các trạng thái (O(n^2)) và công việc không đổi trên mỗi trạng thái sau khi tọa độ được ánh xạ tới các chỉ mục. Danh sách Python hai chiều thông thường sẽ lãng phí một lượng lớn bộ nhớ vì hàng triệu đối tượng số nguyên và tham chiếu danh sách của Python đều đắt tiền. Vì câu trả lời nhiều nhất là 3000 nên mọi giá trị DP đều khớp với số nguyên 16 bit không dấu. Chúng ta có thể lưu trữ tất cả các trạng thái DP hình tam giác trong một`array('H')`, giảm dung lượng lưu trữ DP xuống khoảng 9 MB. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n^2)) | (O(n^2)) được đóng gói thành các giá trị 16 bit | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tọa độ theo thứ tự đã sắp xếp và xây dựng từ điển ánh xạ mọi tọa độ vào chỉ mục của nó. Từ điển cho phép chúng tôi xác định xem tiền thân được yêu cầu (2x_i-x_j) có tồn tại trong thời gian dự kiến ​​không đổi hay không. 
2. Phân bổ một mục nhập DP được đóng gói cho mỗi cặp (i<j). Chúng ta chỉ cần lưu trữ hình tam giác vì các trạng thái có (i\ge j) là vô nghĩa và mỗi giá trị tối đa là (n\le3000), do đó, số nguyên 16 bit không dấu là đủ. 
3. Xử lý chỉ mục đầu tiên (i) từ trái sang phải và với mọi (j>i), xét cặp (x_i,x_j). Mọi tiền thân của cặp này phải có chỉ số nhỏ hơn (i), do đó trạng thái cần thiết cho phép truy toán đã được tính toán. 
4. Tính tọa độ liền trước cần thiết là (p=2x_i-x_j). Nếu (p) hiện diện tại chỉ mục (k), hãy đọc trạng thái đã được tính toán (dp[k][i]) và đặt 

[ 
dp[i][j]=dp[k][i]+1. 
] 

Nhận dạng số học đằng sau bước này chính xác là điều kiện khoảng cách bằng nhau. Nếu khoảng cách từ (x_k) đến (x_i) bằng khoảng cách từ (x_i) đến (x_j), thì (x_k=2x_i-x_j). 

1. Nếu không có (p), đặt (dp[i][j]=2). Bất kỳ cặp đèn lồng nào cũng có giá trị bất kể khoảng cách của chúng, vì vậy mỗi cặp đều có độ dài tăng dần ít nhất là hai. 
2. Cập nhật câu trả lời chung với giá trị DP lớn nhất được thấy. Vì (n\ge3), câu trả lời luôn có ít nhất là 2 và nếu tồn tại một cấp số hợp lệ từ ba cấp trở lên thì phép lặp sẽ ghi lại toàn bộ chiều dài của nó. 

Tại sao nó hoạt động: bất biến là sau khi xử lý một cặp (i,j),`dp[i,j]`chính xác là cấp số cộng dài nhất có hai tọa độ cuối cùng là (x_i,x_j). Nếu tồn tại cấp số trước bắt buộc (2x_i-x_j), mọi cấp số nhân kết thúc tại (i,j) phải sử dụng cấp số trước duy nhất đó, do đó, việc mở rộng cấp số tiến trình tốt nhất kết thúc tại (k,i) sẽ mang lại giá trị tối ưu. Nếu cấp trước không tồn tại thì không có cấp số nào có độ dài ít nhất ba có thể kết thúc tại (i,j), để lại chính cặp đó có độ dài tối ưu 2. Vì mọi cấp số cộng có thể có một số cặp cuối cùng, nên việc lấy giá trị lớn nhất trên tất cả các cặp sẽ cho giá trị tối ưu toàn cục. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n = int(input())
    x = list(map(int, input().split()))

    pos = {value: i for i, value in enumerate(x)}

    # For row i, store dp[i][i+1], dp[i][i+2], ..., dp[i][n-1].
    # Number of stored pairs is n * (n - 1) // 2.
    total = n * (n - 1) // 2
    dp = array('H', [0]) * total

    # base[i] is the first position belonging to row i.
    base = [0] * n
    for i in range(1, n):
        base[i] = base[i - 1] + (n - i)

    def index(i, j):
        return base[i] + (j - i - 1)

    ans = 2

    for i in range(n - 1):
        xi = x[i]

        for j in range(i + 1, n):
            previous = 2 * xi - x[j]
            k = pos.get(previous)

            if k is not None:
                length = dp[index(k, i)] + 1
            else:
                length = 2

            dp[index(i, j)] = length

            if length > ans:
                ans = length

    print(ans)

if __name__ == "__main__":
    solve()
```Từ điển tọa độ được xây dựng trước tiên, do đó việc tra cứu tiền thân không yêu cầu tìm kiếm nhị phân cho mỗi cặp. Vì tọa độ khác nhau nên`pos.get(previous)`trả về chỉ mục tiền nhiệm duy nhất hoặc`None`. 

Bố cục DP hình tam giác đáng được chú ý. Hàng ngang`i`chỉ chứa các cặp`(i, i+1)`bởi vì`(i, n-1)`. Vị trí bắt đầu của nó là 

[ 
\text{base[i]=\sum_{r=0}^{i-1}(n-r-1). 
] 

Sự tái phát sau đó lập bản đồ`(i,j)`ĐẾN`base[i] + (j-i-1)`. Việc trừ một là cần thiết vì cặp được lưu đầu tiên trong hàng`i`là`(i,i+1)`. 

Sự tái phát đọc`dp[index(k, i)]`, không`dp[index(i, k)]`, bởi vì tiến trình được sắp xếp theo tọa độ và (k<i). Việc tính toán tiền thân đảm bảo thứ tự này tự động bất cứ khi nào tiền thân tồn tại. 

Giá trị DP được lưu trữ trong mảng 16 bit không dấu. Độ dài lũy tiến tối đa có thể là 3000, thoải mái dưới 65535. Điều này tránh được việc sử dụng bộ nhớ của hàng triệu số nguyên Python. Số học số nguyên của Python cũng xử lý tọa độ gần (10^{18}) mà không bị tràn. 

Không có trường hợp đặc biệt nào cho ba chiếc đèn lồng. Sự lặp lại cặp tự nhiên thay đổi trạng thái độ dài-2 thành độ dài 3 khi tồn tại tiền thân được yêu cầu, do đó mẫu`1 2 3`tạo ra 3 mà không có logic riêng biệt. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, tọa độ là`1 2 3`. Tiến trình ba đèn lồng duy nhất có điểm khác biệt chung 1. 

| tôi | j | cặp | tọa độ trước đó | chỉ số tiền thân | dp[i][j] | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1, 2 | 0 | vắng mặt | 2 | 2 | 
| 0 | 2 | 1, 3 | -1 | vắng mặt | 2 | 2 | 
| 1 | 2 | 2, 3 | 1 | 0 | 3 | 3 | 

Đối với cặp cuối cùng`(2,3)`, số tiền trước cần thiết là`1`, hiện diện ở chỉ số 0. Trạng thái đã được tính toán cho`(1,2)`có độ dài 2, do đó việc thêm 3 sẽ có độ dài 3. Do đó, câu trả lời là 3. 

Đối với Mẫu 2, tọa độ là`1 2 4 6 7`. Tiến triển tốt nhất là`1,4,7`, có điểm chung 3. 

| tôi | j | cặp | tọa độ trước đó | chỉ số tiền thân | dp[i][j] | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1, 2 | 0 | vắng mặt | 2 | 2 | 
| 0 | 2 | 1, 4 | -2 | vắng mặt | 2 | 2 | 
| 0 | 3 | 1, 6 | -4 | vắng mặt | 2 | 2 | 
| 0 | 4 | 1, 7 | -5 | vắng mặt | 2 | 2 | 
| 1 | 2 | 2, 4 | 0 | vắng mặt | 2 | 2 | 
| 1 | 3 | 2, 6 | -2 | vắng mặt | 2 | 2 | 
| 1 | 4 | 2, 7 | -3 | vắng mặt | 2 | 2 | 
| 2 | 3 | 4, 6 | 2 | chỉ số 1 | 3 | 3 | 
| 2 | 4 | 4, 7 | 1 | chỉ số 0 | 3 | 3 | 
| 3 | 4 | 6, 7 | 5 | vắng mặt | 2 | 3 | 

Nhà nước`(2,4)`tương ứng với tọa độ`4,7`. Tiền thân của nó là tọa độ`1`, vì vậy nó mở rộng trạng thái`(0,2)`, đại diện`1,4`, có độ dài 3. Điều này chứng tỏ tại sao DP phải xem xét các chỉ số đầu vào không liên tiếp thay vì chỉ các đèn lồng lân cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2)) | Mỗi cặp (i<j) được xử lý một lần, với công việc từ điển theo thời gian dự kiến ​​không đổi. | 
| Không gian | (O(n^2)) | Có (n-1)/2) trạng thái DP, được lưu trữ dưới dạng số nguyên 16 bit, cộng với từ điển tọa độ và mảng chỉ mục. | 

Với (n=3000), chỉ có khoảng 4,5 triệu trạng thái DP. Bản trình bày được đóng gói sử dụng khoảng 9 MB cho chính DP, trong khi các cấu trúc dữ liệu Python còn lại vẫn nằm trong giới hạn bộ nhớ 512 MB. Vòng lặp bậc hai thực hiện khoảng 4,5 triệu lần lặp, phù hợp với giới hạn đã cho. 

## Trường hợp thử nghiệm```python
import sys
import io
from array import array

def solve():
    input = sys.stdin.readline

    n = int(input())
    x = list(map(int, input().split()))

    pos = {value: i for i, value in enumerate(x)}

    total = n * (n - 1) // 2
    dp = array('H', [0]) * total

    base = [0] * n
    for i in range(1, n):
        base[i] = base[i - 1] + (n - i)

    def index(i, j):
        return base[i] + (j - i - 1)

    ans = 2

    for i in range(n - 1):
        xi = x[i]
        for j in range(i + 1, n):
            previous = 2 * xi - x[j]
            k = pos.get(previous)

            if k is None:
                length = 2
            else:
                length = dp[index(k, i)] + 1

            dp[index(i, j)] = length
            ans = max(ans, length)

    print(ans)

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

# Provided samples
assert run("3\n1 2 3\n") == "3\n", "sample 1"
assert run("5\n1 2 4 6 7\n") == "3\n", "sample 2"
assert run("10\n5 10 15 20 35 60 80 85 110 120\n") == "5\n", "sample 3"

# Minimum-size input
assert run("3\n1 2 4\n") == "2\n", "no three-term progression"

# Boundary coordinates near 10^18
assert run("5\n0 250000000000000000 500000000000000000 750000000000000000 1000000000000000000\n") == "5\n", "large coordinates"

# Off-by-one case: progression uses non-consecutive input positions
assert run("7\n1 2 4 7 10 13 20\n") == "4\n", "non-consecutive progression"

# Maximum-size case: all 3000 coordinates form one progression
n = 3000
maximum_case = str(n) + "\n" + " ".join(map(str, range(n))) + "\n"
assert run(maximum_case) == "3000\n", "maximum-size arithmetic progression"

# The following would be an all-equal input:
# 3
# 5 5 5
# It is intentionally not asserted because the problem requires
# x_1 < x_2 < ... < x_n. Such an input is outside the specification.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 2 4`| 2 | Đầu vào hợp lệ tối thiểu khi không tồn tại tiến trình ba kỳ | 
|`5 / 0 250000000000000000 ... 1000000000000000000`| 5 | Tọa độ gần ranh giới (10^{18}) và số học số nguyên lớn | 
|`7 / 1 2 4 7 10 13 20`| 4 | Tiến trình có thành viên không liền kề trong đầu vào | 
| 3000 tọa độ liên tiếp | 3000 | Tối đa (n), số trạng thái bậc hai và câu trả lời tối đa có thể | 
|`3 / 5 5 5`| Không áp dụng | Các tọa độ hoàn toàn bằng nhau nằm ngoài ràng buộc đầu vào tăng nghiêm ngặt | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là đầu vào nhỏ nhất có thể. Với```
3
1 2 3
```cặp đôi`(1,2)`được khởi tạo ở độ dài 2. Khi thuật toán đạt`(2,3)`, nó tính toán (2\cdot2-3=1), tìm tọa độ 1 và đọc trạng thái độ dài-2 cho`(1,2)`. Nó tạo ra (2+1=3), vì vậy câu trả lời cuối cùng là 3. Không có cách xử lý đặc biệt nào cho`n=3`được yêu cầu. 

Trường hợp cạnh thứ hai là một cặp không có tiền thân. Vì```
3
1 2 4
```cặp đôi`(2,4)`yêu cầu tọa độ (2\cdot2-4=0). Vì không có số 0 nên thuật toán gán độ dài 2 cho cặp đó. Cặp đôi`(1,2)`cũng có độ dài 2 và không có trạng thái nào đạt tới độ dài 3. Đầu ra là 2. 

Trường hợp cạnh thứ ba là một cấp số ẩn giữa các tọa độ không liên quan. Vì```
7
1 2 4 7 10 13 20
```tọa độ`1, 4, 7, 10`tạo thành một cấp số chênh lệch 3. Khi xử lý`(7,10)`, số trước là 4, nên trạng thái mở rộng`1,4,7`ĐẾN`1,4,7,10`. Câu trả lời trở thành 4. Điều này nắm bắt các triển khai chỉ kiểm tra các phần tử đầu vào liền kề. 

Trường hợp cạnh thứ tư sử dụng các giá trị tọa độ pháp lý lớn nhất:```
5
0 250000000000000000 500000000000000000 750000000000000000 1000000000000000000
```Mỗi khoảng cách là (250000000000000000), vì vậy câu trả lời là 5. Phép tính trước đó liên tục tạo ra các giá trị lớn bằng (10^{18}) và Python xử lý chúng một cách chính xác. 

Trường hợp hoàn toàn bình đẳng được yêu cầu, chẳng hạn như```
3
5 5 5
```không thể xảy ra trong một bài kiểm tra hợp lệ vì bài toán đảm bảo tọa độ tăng dần. Từ điển và DP được thiết kế dựa trên sự đảm bảo đó, do đó, dữ liệu đầu vào không đúng định dạng này được loại trừ một cách có chủ ý khỏi các xác nhận có thể thực thi được thay vì coi đó là một thử nghiệm pháp lý.
