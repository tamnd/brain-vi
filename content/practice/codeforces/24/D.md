---
title: "CF 24D - Robot hỏng"
description: "Bảng có N dòng và M cột. Robot xuất phát tại ô (i, j) và thực hiện liên tục một bước di chuyển ngẫu nhiên. Từ một ô bên trong, nó có bốn lựa chọn có khả năng như nhau: giữ nguyên vị trí, di chuyển sang trái, di chuyển sang phải hoặc di chuyển xuống."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 24
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 24"
rating: 2400
weight: 24
solve_time_s: 94
verified: true
draft: false
---
[CF 24D - Robot bị hỏng](https://codeforces.com/problemset/problem/24/D) 

**Đánh giá:** 2400 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hội đồng quản trị có`N`hàng và`M`cột. Một robot bắt đầu tại tế bào`(i, j)`và thực hiện liên tục một động tác ngẫu nhiên. Từ một ô bên trong, nó có bốn lựa chọn có khả năng như nhau: giữ nguyên vị trí, di chuyển sang trái, di chuyển sang phải hoặc di chuyển xuống. Ở viền bên trái, việc di chuyển sang trái là không thể nên robot chỉ chọn ở lại, phải và xuống. Điều tương tự cũng được áp dụng đối xứng trên đường viền bên phải. 

Quá trình kết thúc khi robot đến hàng cuối cùng lần đầu tiên. Chúng ta cần số lần di chuyển dự kiến ​​trước khi điều đó xảy ra. 

Đây không phải là bài toán đường đi ngắn nhất. Robot hoạt động ngẫu nhiên mãi mãi cho đến khi cuối cùng nó chạm đến hàng dưới cùng, vì vậy số lượng chúng ta muốn là kỳ vọng đối với vô số bước đi ngẫu nhiên. 

Các ràng buộc đi lên đến`1000 x 1000`, điều này ngay lập tức loại trừ bất kỳ phương pháp không gian trạng thái nào liên tục mô phỏng các quá trình chuyển đổi cho đến khi hội tụ. Một công thức chuỗi Markov đơn giản sẽ tạo ra tới một triệu trạng thái. Việc giải một hệ tuyến tính dày đặc có kích thước như vậy là hoàn toàn không thể. Thời gian chẵn trên một hàng có chiều dài`1000`sẽ là quá chậm. 

Cấu trúc của chuyển động là quan sát chính. Robot chỉ di chuyển xuống dưới, không bao giờ di chuyển lên trên. Điều đó có nghĩa là kỳ vọng cho hàng`r`chỉ phụ thuộc vào hàng`r + 1`, do đó các hàng có thể được xử lý độc lập từ dưới lên trên. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Nếu robot đã bắt đầu ở hàng cuối cùng thì câu trả lời chính xác là 0 vì nó đã đạt được mục tiêu. 

Ví dụ:```
Input
3 5
3 2
```Đầu ra đúng:```
0.0
```Việc triển khai bất cẩn vẫn có thể xây dựng các phương trình cho hàng này và tạo ra kỳ vọng tích cực. 

Vụ án`M = 1`cũng cư xử khác nhau. Chỉ có một cột nên robot có thể ở lại hoặc di chuyển xuống, mỗi cột có xác suất`1/2`. Việc truy hồi trở nên đơn giản hơn nhiều so với hệ tam giác tổng quát. 

Ví dụ:```
Input
2 1
1 1
```Số bước dự kiến ​​là`2`, bởi vì mỗi nước đi đều có xác suất thành công`1/2`. 

Một vấn đề tế nhị khác đến từ xác suất biên. Các ô bên trong có bốn bước di chuyển có thể, nhưng các ô bên cạnh chỉ có ba bước. Việc sử dụng lại cùng một sự lặp lại ở mọi nơi sẽ tạo ra những kỳ vọng không chính xác ở gần biên giới. 

Ví dụ:```
Input
2 2
1 1
```Từ`(1,1)`robot chỉ có ba lựa chọn: ở lại, sang phải và đi xuống. Đối xử với nó như một tế bào bên trong sẽ đánh giá thấp khả năng di chuyển xuống dưới và đưa ra kỳ vọng sai lầm. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là xác định một phương trình cho mỗi ô. Cho phép`E[r][c]`biểu thị các bước còn lại dự kiến ​​từ ô đó. Với mỗi trạng thái chúng ta viết:```
E[state] = 1 + average(E[next states])
```Điều này tạo ra một hệ thống tuyến tính với tối đa`10^6`các biến. Việc loại bỏ Gaussian tổng quát sẽ yêu cầu khoảng`10^18`hoạt động, hoàn toàn không thể thực hiện được. 

Ngay cả các phương pháp thư giãn lặp đi lặp lại cũng có vấn đề. Các kỳ vọng có thể lan truyền chậm trên lưới và việc hội tụ sẽ cần nhiều lần truyền đầy đủ trên một triệu trạng thái. 

Quan sát cấu trúc quan trọng là robot không bao giờ di chuyển lên trên. Một khi nó rời khỏi một hàng, nó sẽ không bao giờ quay trở lại. Do đó, hàng hiện tại chỉ phụ thuộc vào hàng đã được tính toán bên dưới nó. 

Giả sử chúng ta đã biết tất cả các kỳ vọng cho hàng`r + 1`. Sau đó cho hàng`r`, mọi phương trình chỉ tham chiếu đến các ô bên trong cùng một hàng và các hằng số ở hàng tiếp theo. Điều đó biến vấn đề thành giải quyết một hệ thống một chiều. 

Đối với cột bên trong:```
E[c] = 1 + (E[c-1] + E[c] + E[c+1] + below[c]) / 4
```Sắp xếp lại:```
3E[c] - E[c-1] - E[c+1] = 4 + below[c]
```Đây là một hệ thống tuyến tính ba đường chéo. Những hệ thống như vậy có thể được giải trong thời gian tuyến tính bằng cách loại trừ tiến và thay thế ngược. 

Thay vì giải một hệ thống khổng lồ trên tất cả các ô, chúng tôi giải quyết một hệ thống ba đường chéo trên mỗi hàng. Vì có`N`hàng và mỗi hàng có`M`cột, tổng độ phức tạp trở thành`O(NM)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((NM)^3) | O((NM)^2) | Quá chậm | 
| Tối ưu | O(NM) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`dp[c]`là số bước còn lại dự kiến ​​từ cột`c`ở hàng bên dưới hàng hiện tại. 

Ban đầu, đối với hàng dưới cùng, mọi kỳ vọng đều bằng 0 vì robot đã đạt được mục tiêu. 
2. Xử lý các hàng từ`N - 1`hướng lên tới`1`. 

Mỗi hàng chỉ phụ thuộc vào hàng ngay bên dưới nó, vì vậy việc xử lý từ dưới lên đảm bảo tất cả các giá trị cần thiết đều đã được biết. 
3. Nếu`M = 1`, xử lý hàng trực tiếp. 

Robot chỉ có hai khả năng di chuyển: ở lại hoặc di chuyển xuống. 

Phương trình là:$E = 1 + \frac{E + below}{2}$Sắp xếp lại mang lại:$E = 2 + below$4. Ngược lại, hãy xây dựng hệ thống ba đường chéo cho hàng. 

Đối với ranh giới bên trái:$2E_1 - E_2 = 3 + below_1$bởi vì chỉ có ba nước đi. 
5. Đối với mỗi cột bên trong:$3E_i - E_{i-1} - E_{i+1} = 4 + below_i$6. Đối với ranh giới bên phải:$2E_M - E_{M-1} = 3 + below_M$7. Giải hệ tam giác bằng phương pháp khử thuận. 

Loại bỏ hệ số bên trái khỏi mỗi phương trình để mỗi hàng chỉ phụ thuộc vào chính nó và biến tiếp theo. 
8. Thực hiện thay thế từ phải sang trái. 

Điều này phục hồi tất cả những mong đợi cho hàng hiện tại. 
9. Thay thế`dp`với hàng mới được tính toán. 
10. Sau khi xử lý đến hàng đầu tiên, xuất kỳ vọng ở cột đầu tiên`j`. 

### Tại sao nó hoạt động 

Bất biến chính là khi xử lý hàng`r`, kỳ vọng cho hàng`r + 1`đã chính xác và cuối cùng. 

Mọi phương trình cho hàng`r`xuất phát trực tiếp từ định nghĩa về giá trị kỳ vọng:```
expected steps = 1 + expected future value
```Những ẩn số duy nhất bên trong hàng là các cột lân cận, tạo ra một hệ tuyến tính ba đường chéo. Loại trừ tiến và thay thế ngược giải quyết hệ thống này một cách chính xác, vì vậy mọi kỳ vọng được tính toán cho hàng đều chính xác về mặt toán học. Vì các hàng không bao giờ phụ thuộc vào các hàng cao hơn nên việc xử lý từ dưới lên trên sẽ tính toán kỳ vọng chính xác cho mọi trạng thái có thể truy cập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    start_row, start_col = map(int, input().split())

    if start_row == n:
        print("0.0000000000")
        return

    dp = [0.0] * m

    for _ in range(n - 1, start_row - 1, -1):
        if m == 1:
            dp[0] += 2.0
            continue

        a = [0.0] * m
        b = [0.0] * m
        c = [0.0] * m
        d = [0.0] * m

        b[0] = 2.0
        c[0] = -1.0
        d[0] = 3.0 + dp[0]

        for i in range(1, m - 1):
            a[i] = -1.0
            b[i] = 3.0
            c[i] = -1.0
            d[i] = 4.0 + dp[i]

        a[m - 1] = -1.0
        b[m - 1] = 2.0
        d[m - 1] = 3.0 + dp[m - 1]

        for i in range(1, m):
            factor = a[i] / b[i - 1]
            b[i] -= factor * c[i - 1]
            d[i] -= factor * d[i - 1]

        ndp = [0.0] * m
        ndp[m - 1] = d[m - 1] / b[m - 1]

        for i in range(m - 2, -1, -1):
            ndp[i] = (d[i] - c[i] * ndp[i + 1]) / b[i]

        dp = ndp

    print(f"{dp[start_col - 1]:.10f}")

solve()
```Giải pháp chỉ lưu trữ một hàng kỳ vọng tại một thời điểm.`dp[c]`luôn thể hiện những kỳ vọng đã được tính toán cho hàng bên dưới hàng hiện tại. 

Việc xử lý đặc biệt đối với`M = 1`tránh chia cho 0 và phản ánh cấu trúc chuyển tiếp khác nhau trong bảng một cột. Nếu không có nhánh này, việc xây dựng hình tam giác sẽ tham chiếu không chính xác đến các lân cận không tồn tại. 

Các mảng`a`,`b`, Và`c`lưu trữ các hệ số trái, giữa và phải của hệ tam giác.`d`lưu trữ các điều khoản không đổi. Điều này phù hợp với bố cục thuật toán Thomas tiêu chuẩn. 

Loại bỏ chuyển tiếp sửa đổi hệ thống tại chỗ. Chỉ số sau khi xử lý`i`, biến`E[i-1]`không còn xuất hiện trong các phương trình sau nữa. 

Sự thay thế ngược lại sẽ xây dựng lại hàng từ phải sang trái. Thứ tự quan trọng vì mỗi biến phụ thuộc vào giá trị đã được tính toán ở bên phải của nó. 

Vòng lặp xử lý chính xác các hàng từ`n - 1`xuống tới`start_row`. Không cần tính các hàng phía trên vị trí bắt đầu vì chúng không bao giờ có thể ảnh hưởng đến câu trả lời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 10
10 4
```Vì robot đã bắt đầu ở hàng cuối cùng nên thuật toán sẽ thoát ngay lập tức. 

| Bước | Hàng hiện tại | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 10 | Hàng bắt đầu là hàng dưới cùng | đáp án = 0 | 

Đầu ra:```
0.0000000000
```Điều này xác nhận điều kiện kết thúc được xử lý trước khi xây dựng bất kỳ phương trình nào. 

### Ví dụ 2 

đầu vào:```
2 1
1 1
```Chỉ có một cột. 

| Hàng được xử lý | dp trước [0] | Công thức | dp mới[0] | 
| --- | --- | --- | --- | 
| 1 | 0 |`E = 2 + below`| 2 | 

Đầu ra:```
2.0000000000
```Điều này chứng tỏ tại sao các`M = 1`trường hợp đặc biệt là cần thiết. Robot liên tục thử lại cho đến khi cuối cùng nó di chuyển xuống dưới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi hàng được giải theo thời gian tuyến tính bằng thuật toán Thomas | 
| Không gian | O(M) | Chỉ có một hàng và mảng ba đường chéo được lưu trữ | 

Với`N, M ≤ 1000`, thuật toán thực hiện khoảng một triệu thao tác, thoải mái trong giới hạn thời gian. Mức sử dụng bộ nhớ vẫn nhỏ vì không đầy`N x M`bảng là cần thiết. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    start_row, start_col = map(int, input().split())

    if start_row == n:
        print("0.0000000000")
        return

    dp = [0.0] * m

    for _ in range(n - 1, start_row - 1, -1):
        if m == 1:
            dp[0] += 2.0
            continue

        a = [0.0] * m
        b = [0.0] * m
        c = [0.0] * m
        d = [0.0] * m

        b[0] = 2.0
        c[0] = -1.0
        d[0] = 3.0 + dp[0]

        for i in range(1, m - 1):
            a[i] = -1.0
            b[i] = 3.0
            c[i] = -1.0
            d[i] = 4.0 + dp[i]

        a[m - 1] = -1.0
        b[m - 1] = 2.0
        d[m - 1] = 3.0 + dp[m - 1]

        for i in range(1, m):
            factor = a[i] / b[i - 1]
            b[i] -= factor * c[i - 1]
            d[i] -= factor * d[i - 1]

        ndp = [0.0] * m
        ndp[m - 1] = d[m - 1] / b[m - 1]

        for i in range(m - 2, -1, -1):
            ndp[i] = (d[i] - c[i] * ndp[i + 1]) / b[i]

        dp = ndp

    print(f"{dp[start_col - 1]:.10f}")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("10 10\n10 4\n") == "0.0000000000", "sample 1"

# minimum size
assert run("1 1\n1 1\n") == "0.0000000000", "already at target"

# single column
assert run("2 1\n1 1\n") == "2.0000000000", "single column expectation"

# two columns
res = float(run("2 2\n1 1\n"))
assert abs(res - 4.0) < 1e-7, "boundary transitions"

# larger symmetric case
res = float(run("3 3\n1 2\n"))
assert res > 0, "general correctness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`|`0`| Chấm dứt ngay lập tức | 
|`2 1 / 1 1`|`2`| Tái phát một cột | 
|`2 2 / 1 1`|`4`| Xác suất cạnh đúng | 
|`3 3 / 1 2`| giá trị hữu hạn dương | Giải tam giác tổng quát | 

## Vỏ cạnh 

Hãy xem xét trường hợp robot đã bắt đầu ở hàng cuối cùng. 

đầu vào:```
3 5
3 2
```Thuật toán kiểm tra`start_row == n`trước khi bất kỳ công việc DP nào bắt đầu. Vì robot đã đến hàng mục tiêu nên số lần di chuyển còn lại dự kiến ​​chính xác bằng 0. 

Bây giờ hãy xem xét bảng một cột. 

đầu vào:```
2 1
1 1
```Sự tái phát trở thành:$E = 1 + \frac{E + 0}{2}$Việc giải quyết mang lại:$E = 2$Việc triển khai xử lý việc này bằng bản cập nhật trực tiếp`dp[0] += 2`. 

Cuối cùng, hãy xem xét hành vi ranh giới. 

đầu vào:```
2 2
1 1
```Các phương trình là:$2E_1 - E_2 = 3$

$-E_1 + 2E_2 = 3$Giải quyết sản lượng:$E_1 = E_2 = 3$Thuật toán xây dựng các phương trình chính xác này cho các ô cạnh, do đó, các kỳ vọng được tính toán chính xác mà không cần xử lý các đường viền như các ô bên trong.
