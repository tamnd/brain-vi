---
title: "CF 279E - Phân hủy đẹp"
description: "Một số đẹp là lũy thừa của hai với một trong hai dấu. Nói cách khác, mọi triệu chứng phải là một trong $$pm 2^0, pm 2^1, pm 2^2,dots$$ Chúng ta được cấp một số nguyên dương $n$, nhưng không ở dạng thập phân."
date: "2026-06-05T05:57:38+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 279
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 171 (Div. 2)"
rating: 1900
weight: 279
solve_time_s: 248
verified: true
draft: false
---

[CF 279E - Sự phân hủy đẹp mắt](https://codeforces.com/problemset/problem/279/E) 

**Xếp hạng:** 1900 
**Tags:** dp, trò chơi, tham lam, lý thuyết số 
**Thời gian giải:** 4 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một số đẹp là lũy thừa của hai với một trong hai dấu. Nói cách khác, mỗi lệnh triệu tập phải là một trong$$\pm 2^0,\ \pm 2^1,\ \pm 2^2,\dots$$Chúng ta được cho một số nguyên dương$n$, nhưng không ở dạng thập phân. Thay vào đó, biểu diễn nhị phân của nó được cung cấp dưới dạng một chuỗi có độ dài có thể đạt tới một triệu ký tự. Chúng ta phải tìm số nhỏ nhất có thể có các số đẹp có tổng bằng$n$. 

Một cách khác để xem vấn đề là chúng ta muốn viết$$n=\sum_i d_i 2^i$$trong đó mỗi hệ số$d_i$được chọn từ$\{-1,0,1\}$. Mỗi hệ số khác 0 đóng góp một số đẹp, vì vậy mục tiêu là giảm thiểu số vị trí trong đó$d_i \neq 0$. 

Hạn chế nổi bật nhất là độ dài đầu vào. Chuỗi nhị phân có thể chứa$10^6$bit, có nghĩa là giá trị của$n$bản thân nó lớn về mặt thiên văn. Chuyển đổi nó thành một số nguyên là không thể. Mọi giải pháp đều phải hoạt động trực tiếp trên biểu diễn nhị phân. 

Một thuật toán bậc hai ngay lập tức bị loại trừ. Thậm chí$O(|s| \log |s|)$là không cần thiết khi bản thân dữ liệu đầu vào đã chứa một triệu ký tự. Mục tiêu là quét tuyến tính với công việc bổ sung không đổi trên mỗi bit. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét đầu vào:```
1
```Số đã có rồi$2^0$, vậy đáp án là:```
1
```Một giải pháp giả sử mọi biểu diễn đều cần ít nhất một bước lan truyền mang có thể vô tình tạo ra giá trị lớn hơn. 

Coi như:```
111
```đó là$7$. 

Biểu diễn nhị phân ngây thơ sử dụng ba quyền hạn:$$7=2^2+2^1+2^0$$nhưng phân hủy tốt hơn là$$7=2^3-2^0$$chỉ sử dụng hai lệnh triệu tập. Bất kỳ cách tiếp cận nào chỉ đơn giản là đếm các bit đã đặt đều trả về 3 thay vì câu trả lời đúng 2. 

Một trường hợp tinh tế khác là một loạt các trường hợp:```
111111
```Trọng lượng nhị phân là sáu, nhưng$$63=64-1$$chỉ cần hai lệnh triệu tập. Giải pháp tối ưu phải có khả năng tạo ra các hệ số mang theo có chủ ý vì làm như vậy thường làm giảm tổng số hệ số khác 0. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là tìm kiếm trên tất cả các phép gán có thể có của các hệ số$-1$,$0$, Và$1$lũy thừa của hai và kiểm tra xem các kết hợp nào có tổng bằng$n$. Điều này đúng vì mọi phân tách hợp lệ đều tương ứng với phép gán như vậy. 

Thật không may, ngay cả đối với một số chỉ có$m$bit, có khoảng$3^m$khả năng. Với$m$lên tới một triệu, điều này là hoàn toàn không khả thi. 

Quan sát tiếp theo là số học nhị phân đã cho chúng ta biết các hệ số tương tác như thế nào. Giả sử chúng ta xử lý số từ bit có ý nghĩa nhỏ nhất trở lên. Ở bất kỳ vị trí nào, thông tin duy nhất quan trọng từ các bit thấp hơn là liệu phần mang có vào vị trí hiện tại hay không. 

Điều đó có nghĩa là toàn bộ bài toán có thể được biểu diễn dưới dạng một chương trình động chỉ có hai trạng thái:$$\text{carry} = 0 \quad \text{or} \quad \text{carry} = 1$$Một chút$b$, chúng ta biết giá trị$$x = b + \text{carry}.$$Chúng ta phải chọn một chữ số$d \in \{-1,0,1\}$và một đồ mang theo mới để$$x = d + 2 \cdot \text{new\_carry}.$$Mỗi lần$d$khác 0, một số đẹp sẽ được sử dụng. 

Cái nhìn sâu sắc quan trọng là tương lai chỉ phụ thuộc vào việc mang theo chứ không phụ thuộc vào bất kỳ lựa chọn nào trước đó. Điều này biến tìm kiếm theo cấp số nhân thành DP hai trạng thái được quét qua chuỗi nhị phân một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(3^m)$|$O(m)$| Quá chậm | 
| DP tối ưu với các trạng thái thực hiện |$O(m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đảo ngược chuỗi nhị phân để các bit được xử lý từ ít quan trọng nhất đến quan trọng nhất. 
2. Duy trì hai giá trị DP:$$dp[0]$$là chi phí tối thiểu sau khi xử lý hậu tố hiện tại có mang 0.$$dp[1]$$là chi phí tối thiểu sau khi xử lý hậu tố hiện tại có nhớ 1. 
3. Khởi tạo:$$dp[0]=0,\quad dp[1]=\infty$$bởi vì trước khi xử lý bất kỳ bit nào sẽ không có dữ liệu mang đến. 
4. Đối với mỗi bit$b$: 

1. Tạo một mảng mới$ndp$. 
2. Đối với mỗi trạng thái mang$c\in\{0,1\}$, tính toán$$x=b+c.$$3. Liệt kê tất cả các phân rã hợp lệ của$$x=d+2\cdot nc$$Ở đâu$d\in\{-1,0,1\}$. 
4. Thêm chi phí$1$nếu như$d\neq0$, nếu không thì thêm$0$. 
5. Thư giãn trạng thái tương ứng$ndp[nc]$. 
5. Sau khi tất cả các bit được xử lý, vẫn có thể có phần nhớ cuối cùng. 
6. Nếu số cuối là 0 thì không cần thêm chữ số nào. 
7. Nếu số cuối là 1 thì biểu thị thêm một hệ số$+1$ở lũy thừa tiếp theo của hai, đóng góp thêm một triệu hồi. 
8. Lấy chi phí cuối cùng hợp lệ tối thiểu. 

### Tại sao nó hoạt động 

Đối với mỗi vị trí được xử lý, DP lưu trữ số lượng tối thiểu các chữ số có dấu khác 0 để có thể tạo ra chính xác phần số đã được nhìn thấy trong khi để lại một giá trị mang xác định cho các bit cao hơn còn lại. 

Quá trình chuyển đổi xem xét mọi cách có thể để thỏa mãn phương trình bit hiện tại$$b+\text{carry}=d+2\cdot\text{new\_carry}.$$Không có biểu diễn nhị phân có dấu hợp lệ nào bị loại trừ và mọi biểu diễn tương ứng với một chuỗi duy nhất của các lựa chọn cục bộ đó. Vì DP luôn giữ chi phí tối thiểu cho mỗi trạng thái thực hiện nên câu trả lời cuối cùng chính xác là số lượng lệnh triệu hồi đẹp mắt tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    INF = 10**18
    dp0, dp1 = 0, INF

    for ch in reversed(s):
        b = ord(ch) - ord('0')

        ndp0 = INF
        ndp1 = INF

        # carry = 0
        if dp0 < INF:
            x = b

            if x == 0:
                ndp0 = min(ndp0, dp0)
            else:  # x == 1
                ndp0 = min(ndp0, dp0 + 1)  # d = 1
                ndp1 = min(ndp1, dp0 + 1)  # d = -1

        # carry = 1
        if dp1 < INF:
            x = b + 1

            if x == 1:
                ndp0 = min(ndp0, dp1 + 1)  # d = 1
                ndp1 = min(ndp1, dp1 + 1)  # d = -1
            else:  # x == 2
                ndp1 = min(ndp1, dp1)      # d = 0

        dp0, dp1 = ndp0, ndp1

    answer = min(dp0, dp1 + 1)
    print(answer)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo DP một cách chính xác. 

Cặp đôi`(dp0, dp1)`lưu trữ chi phí tốt nhất cho mỗi trạng thái thực hiện. Vì chỉ có hai trạng thái nên mức sử dụng bộ nhớ không đổi ngay cả khi đầu vào chứa một triệu bit. 

Logic chuyển đổi xuất phát trực tiếp từ việc giải quyết$$x=d+2\cdot nc.$$Chỉ có ba giá trị có thể có của$x$: 0, 1 và 2. Mỗi bit có một số lượng phân tách hợp lệ rất nhỏ, vì vậy mỗi bit chỉ yêu cầu công việc không đổi. 

Biểu thức cuối cùng```
min(dp0, dp1 + 1)
```xử lý việc mang theo còn sót lại. Mang số 1 nghĩa là hệ số bổ sung$+1$ở lũy thừa tiếp theo của hai, tốn thêm một triệu hồi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10
```Số lượng là$2$. 

Thứ tự xử lý là`"01"`. 

| Chút | dp0 trước | dp1 trước | dp0 sau | dp1 sau | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | INF | 0 | INF | 
| 1 | 0 | INF | 1 | 1 | 

Câu trả lời cuối cùng:$$\min(1,\;1+1)=1$$Đầu ra:```
1
```Ví dụ này cho thấy trường hợp đơn giản nhất khi bản thân số đó đã là lũy thừa của hai. 

### Ví dụ 2 

đầu vào:```
111
```Số lượng là$7$. 

Thứ tự xử lý là`"111"`. 

| Chút | dp0 trước | dp1 trước | dp0 sau | dp1 sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | INF | 1 | 1 | 
| 1 | 1 | 1 | 2 | 1 | 
| 1 | 2 | 1 | 2 | 1 | 

Câu trả lời cuối cùng:$$\min(2,\;1+1)=2$$Đầu ra:```
2
```Biểu diễn tối ưu mà DP tìm thấy là$$7 = 2^3 - 2^0.$$Dấu vết minh họa tại sao việc thực hiện một loạt những cái đó lại có lợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O( | s | 
| Không gian |$O(1)$| Chỉ có hai trạng thái DP được lưu trữ | 

Độ dài đầu vào có thể đạt tới$10^6$, do đó quét tuyến tính chính xác là những gì các ràng buộc yêu cầu. Bộ nhớ không đổi dễ dàng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    s = input().strip()

    INF = 10**18
    dp0, dp1 = 0, INF

    for ch in reversed(s):
        b = ord(ch) - ord('0')

        ndp0 = INF
        ndp1 = INF

        if dp0 < INF:
            if b == 0:
                ndp0 = min(ndp0, dp0)
            else:
                ndp0 = min(ndp0, dp0 + 1)
                ndp1 = min(ndp1, dp0 + 1)

        if dp1 < INF:
            if b == 0:
                ndp0 = min(ndp0, dp1 + 1)
                ndp1 = min(ndp1, dp1 + 1)
            else:
                ndp1 = min(ndp1, dp1)

        dp0, dp1 = ndp0, ndp1

    return str(min(dp0, dp1 + 1))

# provided sample
assert run("10\n") == "1", "sample 1"

# custom cases
assert run("1\n") == "1", "minimum input"
assert run("11\n") == "2", "3 = 2 + 1"
assert run("111\n") == "2", "7 = 8 - 1"
assert run("111111\n") == "2", "63 = 64 - 1"
assert run("1000000\n") == "1", "single power of two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Số hợp lệ nhỏ nhất | 
|`11`|`2`| Không có dây chuyền mang theo có lợi | 
|`111`|`2`| Vượt qua những cái liên tiếp | 
|`111111`|`2`| Một chuỗi dài những cái sụp đổ thành hai triệu hồi | 
|`1000000`|`1`| Sức mạnh chính xác của hai | 

## Vỏ cạnh 

Xem xét đầu vào```
1
```DP bắt đầu với`(dp0, dp1) = (0, INF)`. Xử lý bit duy nhất`1`tạo ra trạng thái`(1, 1)`. Kết thúc bằng mang 0 tốn 1, trong khi hoàn thành bằng mang 1 sẽ tốn 1 chi phí`1 + 1 = 2`. Đáp án là 1, tương ứng với phép phân tích$1 = 2^0$. 

Coi như```
111
```Biểu diễn nhị phân đơn giản sử dụng ba lệnh triệu tập. Trong quá trình xử lý DP, trạng thái nhớ-1 trở nên rẻ hơn so với việc sử dụng các hệ số dương nhiều lần. Đáp án cuối cùng là 2, tương ứng với$8 - 1$. Điều này xác nhận rằng thuật toán khai thác chính xác số mang để giảm số chữ số khác 0. 

Coi như```
111111
```DP tiếp tục truyền bá thông qua toàn bộ khối các khối đó mà không mất thêm chi phí. Cuối cùng chỉ có hệ số$-1$ở bit thấp nhất và được tạo ra cuối cùng$+1$duy trì. Câu trả lời là 2, phù hợp$$63 = 64 - 1.$$Đây chính xác là trường hợp mà việc đếm các bit được đặt sẽ bị lỗi nặng, trong khi DP trạng thái mang vẫn hoạt động theo thời gian tuyến tính.
