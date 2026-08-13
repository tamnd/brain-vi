---
title: "CF 102365E - Hành động thú vị"
description: "Chúng ta có một mảng a[1..N] mô tả sự phấn khích của các cảnh theo trình tự thời gian. Chúng ta phải cắt mảng này thành chính xác K phần liền kề không trống."
date: "2026-08-12T23:54:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102365
codeforces_index: "E"
codeforces_contest_name: "UBC Programming Contest 2019 (UBCPC 2019)"
rating: 0
weight: 102365
solve_time_s: 183
verified: true
draft: false
---

[CF 102365E - Hành động thú vị](https://codeforces.com/problemset/problem/102365/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng`a[1..N]`miêu tả sự sôi động của các cảnh theo trình tự thời gian. Chúng ta phải cắt mảng này thành chính xác`K`các phần liền kề không trống. Giá trị được đóng góp bởi một phần là giá trị mảng tối đa bên trong phần đó và mục tiêu là tối đa hóa tổng của các giá trị đó`K`cực đại. Những ràng buộc chính thức là`1 <= K <= N <= 2000`Và`1 <= a[i] <= 1000`. 

Giới hạn đầu tiên,`N <= 2000`, đủ lớn để việc thử từng cặp vị trí cắt bên trong mỗi trạng thái DP là quá tốn kém. Một phân vùng tiêu chuẩn DP có`K * N`các tiểu bang và mỗi tiểu bang có thể có tới`N`vị trí cắt trước đó có thể, đưa ra`O(KN^2)`, đạt khoảng`8 * 10^9`chuyển tiếp khi`N = K = 2000`. Điều đó vượt xa những gì giới hạn một giây có thể hỗ trợ. Chúng ta cần khai thác cách đặc biệt những thay đổi tối đa của một phân khúc khi điểm cuối bên phải của nó di chuyển. 

Có một số trường hợp ranh giới có thể âm thầm phá vỡ việc triển khai bất cẩn. Nếu như`K = 1`, chỉ có một phân đoạn, vì vậy câu trả lời đơn giản là giá trị lớn nhất của toàn bộ mảng. Ví dụ,`N = 2, K = 1, a = [3, 1]`cho`3`, không`4`, vì hai cảnh thuộc cùng một màn. 

Nếu như`K = N`, mỗi đoạn phải chứa chính xác một phần tử. Ví dụ,`N = 3, K = 3, a = [2, 7, 4]`cho`13`. DP vô tình cho phép một phân đoạn trống có thể tạo ra sự chuyển đổi không hợp lệ và tính toán quá mức. 

Các giá trị bằng nhau cũng cần được xử lý cẩn thận. Vì`N = 4, K = 2, a = [5, 5, 1, 5]`, câu trả lời là`10`. Khi một giá trị mới bằng mức tối đa của phân đoạn hiện tại, các ứng cử viên tương ứng có thể được coi là một nhóm ngăn xếp đơn điệu vì mức tối đa của chúng không thay đổi. Việc sử dụng sai mức độ nghiêm ngặt trong điều kiện ngăn xếp có thể chia các cực đại bằng nhau thành các nhóm không cần thiết, mặc dù giá trị cuối cùng chỉ đúng nếu các trạng thái ứng cử viên liên quan được xử lý một cách nhất quán. 

## Phương pháp tiếp cận 

Việc xây dựng quy hoạch động trực tiếp rất đơn giản. Cho phép`dp[k][i]`là sự phấn khích tối đa có thể đạt được bằng cách phân vùng đầu tiên`i`thành phần chính xác`k`hành động không trống rỗng. Nếu hành động cuối cùng bắt đầu ngay sau vị trí`j`, thì phần đóng góp của nó là`max(a[j+1..i])`, Vì thế`dp[k][i] = max(dp[k-1][j] + max(a[j+1..i]))`trên tất cả hợp lệ`j`. 

Sự lặp lại này là đúng vì mọi phân vùng hợp lệ đều có một vị trí duy nhất ngay trước hành động cuối cùng của nó. Khi vị trí đó đã được cố định, đầu tiên`j`các phần tử tạo thành một giải pháp tối ưu với`k-1`hành động, trong khi khoảng thời gian cuối cùng đóng góp tối đa. 

Việc triển khai bạo lực có thể duy trì mức tối đa trong khi di chuyển`j`ngược lại, do đó mỗi trạng thái DP sẽ mất`O(N)`time thay vì tính toán lại mọi khoảng thời gian tối đa từ đầu. có`O(KN)`tiểu bang, đưa ra`O(KN^2)`tổng công việc. Với`N = K = 2000`, đây là về`8 * 10^9`quá trình chuyển đổi ứng viên, quá chậm. 

Quan sát quan trọng là phần tốn kém của quá trình chuyển đổi không phải là`dp[k-1][j]`. Đó là giá trị thay đổi của`max(a[j+1..i])`BẰNG`i`di chuyển sang bên phải. 

Sửa một lớp DP`k`và giả sử chúng tôi đang xử lý đúng điểm cuối`i`. Cho mọi sự khởi đầu có thể`j`, xem xét ứng viên`dp[k-1][j] + max(a[j+1..i])`. 

BẰNG`i`tăng lên một, tất cả các khoảng cực đại này không thay đổi hoặc trở thành giá trị mới`a[i]`. Cụ thể hơn, nếu một số lần bắt đầu hiện có mức tối đa phân đoạn không lớn hơn`a[i]`, tất cả những lần bắt đầu đó bây giờ đều có cùng mức tối đa mới`a[i]`. Họ có thể được hợp nhất thành một nhóm. 

Một ngăn xếp đơn điệu đương nhiên đại diện cho các nhóm này. Ngăn xếp lưu trữ các giá trị tối đa của phân đoạn giảm dần. Cùng với mỗi mức tối đa, chúng tôi lưu trữ lớn nhất`dp[k-1][j]`trong số tất cả các điểm bắt đầu thuộc về nhóm đó. Ứng cử viên tốt nhất được đại diện bởi nhóm sau đó chỉ đơn giản là`group_maximum + largest_previous_dp`. 

Khi mới`a[i]`đến, các nhóm có mức tối đa là nhiều nhất`a[i]`được bật lên và hợp nhất. Từ`a[i]`ít nhất bằng giá trị cực đại trước đó của chúng, các giá trị ứng viên của chúng không giảm sau khi hợp nhất. Điều này mang lại sự đơn giản hóa quan trọng khác: chúng ta không cần cây phân đoạn hoặc đống để duy trì mức tối đa toàn cục. Nếu mức tối ưu toàn cục cũ thuộc về một nhóm đã được loại bỏ thì nhóm được hợp nhất sẽ chứa cùng một ứng cử viên đó và có thể cải thiện nó. Nếu nó thuộc về một nhóm không được bật lên, nó sẽ không thay đổi. Do đó, chúng ta có thể duy trì câu trả lời cho trạng thái DP hiện tại bằng một đại lượng vô hướng duy nhất. 

Ngăn xếp đơn điệu và mỗi nhóm được đẩy một lần và xuất hiện nhiều nhất một lần trong một lớp DP. Do đó, một lớp DP cần`O(N)`, đưa ra một`O(KN)`thuật toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP |`O(KN^2)`|`O(N)`với DP lăn | Quá chậm | 
| Ngăn xếp đơn điệu DP |`O(KN)`|`O(N)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`prev[i]`là điểm tốt nhất cho việc phân vùng đầu tiên`i`các phần tử vào số hành động trước đó. Ban đầu, đối với hành vi không, chỉ`prev[0] = 0`là hợp lệ. Mọi trạng thái khác là không thể. 
2. Xử lý số hành vi từ`1`bởi vì`K`. Đối với lớp hiện tại, hãy tạo`cur`, Ở đâu`cur[i]`cuối cùng sẽ đại diện cho điểm số tốt nhất để phân vùng đầu tiên`i`các yếu tố vào số lượng hành động hiện tại. 
3. Đối với điểm cuối bên phải cố định`i`, hãy cân nhắc mọi sự khởi đầu có thể`j`của hành động cuối cùng. Giá trị ứng viên của nó là`prev[j] + max(a[j+1..i])`. Thay vì lưu trữ từng ứng viên riêng biệt, nhóm bắt đầu có khoảng thời gian tối đa hiện tại bằng nhau. 
4. Lưu trữ các nhóm đó trong một ngăn xếp có giá trị tối đa giảm dần từ dưới lên trên. Mỗi mục ngăn xếp chứa một giá trị tối đa`mx`và lớn nhất`prev[j]`trong số tất cả các lần xuất phát do nhóm đó đại diện. Ứng viên tốt nhất trong nhóm đó là`mx + best_prev`. 
5. Khi xử lý`a[i]`, bắt đầu với sự bắt đầu hành động cuối cùng mới có thể`j = i-1`. Khoảng của nó chỉ bao gồm`a[i]`, vậy đoạn của nó có giá trị lớn nhất là`a[i]`và giá trị cơ sở của nó là`prev[i-1]`. 
6. Bật mọi nhóm ngăn xếp có mức tối đa nhiều nhất`a[i]`. Tất cả những người bắt đầu trong các nhóm đó bây giờ có`a[i]`là khoảng thời gian mới tối đa của họ. Hợp nhất của họ`best_prev`giá trị với`prev[i-1]`, sau đó tạo một nhóm với số lượng tối đa`a[i]`. 

Điều kiện sử dụng`<=`còn hơn là`<`bởi vì cực đại bằng nhau có tác dụng như nhau đối với mọi ứng cử viên. Việc kết hợp chúng sẽ giữ cho ngăn xếp gọn gàng mà không làm mất đi bất kỳ sự tối ưu nào có thể có. 
7. Ứng viên sáng giá nhất của nhóm mới thành lập là`a[i] + best_prev`. Cập nhật tốt nhất toàn cầu cho`cur[i]`với giá trị này. Các nhóm không được xuất hiện sẽ giữ chính xác các giá trị ứng cử viên giống nhau, do đó giá trị tốt nhất toàn cầu trước đó vẫn hợp lệ. 
8. Chỉ các điểm cuối có đủ thành phần cho số lượng hành động được yêu cầu mới được xử lý. Vì`k`hành động, điểm cuối có thể đầu tiên là`i = k`, bởi vì mỗi hành động phải chứa ít nhất một phần tử. Sau khi hoàn thành lớp, thay thế`prev`qua`cur`. 
9. Sau khi xử lý xong tất cả`K`lớp,`prev[N]`là sự phấn khích tối đa có thể có cho toàn bộ mảng được chia thành chính xác`K`hành động. 

Bất biến là ngay trước khi tính toán`cur[i]`, mọi sự khởi đầu có thể`j`cho hành động cuối cùng được đại diện bởi chính xác một nhóm ngăn xếp. Trong nhóm đó, tất cả các điểm khởi đầu như vậy đều có cùng giá trị`max(a[j+1..i])`, do đó chỉ giữ lại giá trị lớn nhất`prev[j]`là đủ. Khi`a[i]`được thêm vào, chính xác là các nhóm có tối đa nhiều nhất`a[i]`thay đổi và tất cả chúng đều thay đổi đến mức tối đa như nhau`a[i]`, đó chính xác là những gì thao tác bật và hợp nhất thực hiện. Vì các nhóm không bị ảnh hưởng bảo toàn các ứng cử viên của họ và nhóm được sáp nhập bảo tồn hoặc cải thiện mọi ứng cử viên mà nó hấp thụ, nên mức tối đa toàn cầu được duy trì chính xác là`cur[i]`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    neg = -10**18

    # prev[i] = best value for first i elements using the previous
    # number of acts.
    prev = [neg] * (n + 1)
    prev[0] = 0

    for acts in range(1, k + 1):
        cur = [neg] * (n + 1)

        # Each entry is [current maximum, best prev[j]]
        # for all starts j represented by this group.
        stack = []

        best = neg

        for i in range(acts, n + 1):
            x = a[i - 1]

            best_prev = prev[i - 1]

            while stack and stack[-1][0] <= x:
                _, group_best = stack.pop()
                if group_best > best_prev:
                    best_prev = group_best

            stack.append((x, best_prev))

            candidate = x + best_prev
            if candidate > best:
                best = candidate

            cur[i] = best

        prev = cur

    print(prev[n])

if __name__ == "__main__":
    solve()
```Vòng lặp bên ngoài xây dựng một lớp DP cho mỗi số hành động có thể có.`prev`chứa các câu trả lời cho`acts - 1`hành động, trong khi`cur`được lấp đầy cho`acts`hành động. Chỉ giữ lại hai lớp là đủ vì sự lặp lại chỉ đề cập đến số hành vi trước đó. 

Bên trong một lớp,`stack`cửa hàng cặp`(maximum, best_previous_value)`. Thành phần thứ hai là tối đa`prev[j]`trên tất cả các vị trí bắt đầu hiện đang chia sẻ thành phần đầu tiên là phân khúc tối đa của chúng. Không có thông tin nào khác về những lần khởi đầu đó có thể ảnh hưởng đến quá trình chuyển đổi. 

dòng`while stack and stack[-1][0] <= x`là cốt lõi của việc tối ưu hóa. Mỗi nhóm bật lên có mức tối đa trước đó không lớn hơn`x`, vì vậy sau khi mở rộng điểm cuối bên phải đến vị trí hiện tại, mức tối đa của nó sẽ trở thành`x`. Tốt nhất của nó`prev[j]`do đó có thể được sáp nhập vào nhóm mới. 

Biến`best`lưu trữ ứng cử viên tối đa trong số tất cả các nhóm được nhìn thấy cho điểm cuối hiện tại. Nó không cần phải tính toán lại sau khi nhóm popping. Nếu mức tối ưu cũ nằm trong một nhóm chưa được xuất hiện thì nó sẽ không thay đổi. Nếu nó nằm trong một nhóm đã tách ra, ứng cử viên của nhóm đó sẽ được thay thế bằng một ứng cử viên trong nhóm đã hợp nhất có số lượng ít nhất bằng. 

Giới hạn dưới`i = acts`ngăn chặn các trạng thái không thể có ít hơn`acts`các phần tử đang được chia thành`acts`các phân đoạn không trống. Số nguyên Python không tràn và câu trả lời lý thuyết nhiều nhất là`K * 1000 <= 2,000,000`. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
2 1
3 1
```Chỉ có một hành động, vì vậy lớp DP cần tìm mức tối đa của toàn bộ tiền tố. 

|`i`|`a[i]`| Ngăn xếp sau khi cập nhật |`best`|`cur[i]`| 
| --- | --- | --- | --- | --- | 
| 1 | 3 |`(3, 0)`| 3 | 3 | 
| 2 | 1 |`(3, 0), (1, -inf)`| 3 | 3 | 

Tại`i = 2`, giá trị mới`1`không thay thế mức tối đa hiện tại`3`, vì vậy phân vùng một hành động tốt nhất vẫn còn`3`. Đầu ra là`3`. 

Đối với mẫu 2,```
2 2
3 100
```Lớp DP đầu tiên đại diện cho một hành động. Ở cuối lớp đó, các câu trả lời có tiền tố là`3`Và`100`. Lớp thứ hai sau đó chỉ xem xét`i = 2`, bởi vì hai hành động yêu cầu ít nhất hai yếu tố. 

|`i`|`a[i]`| Ngăn xếp sau khi cập nhật |`best`|`cur[i]`| 
| --- | --- | --- | --- | --- | 
| 2 | 100 |`(100, 3)`| 103 | 103 | 

Phân vùng duy nhất có thể là`[3] | [100]`, số điểm của ai là`3 + 100 = 103`. Dấu vết cũng chứng minh tại sao`prev[i-1]`là giá trị cơ bản chính xác cho hành động cuối cùng mới được bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(KN)`| Mỗi lớp DP xử lý mọi điểm cuối một lần và mỗi mục nhập ngăn xếp được đẩy và xuất ra nhiều nhất một lần. | 
| Không gian |`O(N)`| Hai mảng DP và một ngăn xếp đơn điệu, mỗi mảng chứa tối đa`N + 1`mục nhập. | 

Với`N <= 2000`Và`K <= 2000`, thuật toán thực hiện theo thứ tự bốn triệu lần lặp DP chính, cộng với số lượng tuyến tính các thao tác ngăn xếp trên mỗi lớp. Điều này phù hợp thoải mái với các yêu cầu tiệm cận dự kiến, trong khi sự chuyển đổi bậc hai so với các vị trí cắt trước đó sẽ đạt tới hàng tỷ phép tính. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(data: str) -> str:
    it = iter(data.split())
    n = int(next(it))
    k = int(next(it))
    a = [int(next(it)) for _ in range(n)]

    neg = -10**18
    prev = [neg] * (n + 1)
    prev[0] = 0

    for acts in range(1, k + 1):
        cur = [neg] * (n + 1)
        stack = []
        best = neg

        for i in range(acts, n + 1):
            x = a[i - 1]
            best_prev = prev[i - 1]

            while stack and stack[-1][0] <= x:
                _, group_best = stack.pop()
                if group_best > best_prev:
                    best_prev = group_best

            stack.append((x, best_prev))

            candidate = x + best_prev
            if candidate > best:
                best = candidate

            cur[i] = best

        prev = cur

    return str(prev[n])

def run(inp: str) -> str:
    return solution(inp).strip()

# Provided samples
assert run("2 1\n3 1\n") == "3", "sample 1"
assert run("2 2\n3 100\n") == "103", "sample 2"

# Minimum-size input
assert run("1 1\n7\n") == "7", "single element"

# K = N, every element must form its own act
assert run("5 5\n1 9 3 7 2\n") == "22", "K equals N"

# All values equal
assert run("5 3\n4 4 4 4 4\n") == "12", "all equal"

# Case where the best cut is not near an obvious local maximum
assert run("4 2\n1 10 2 9\n") == "19", "best two-act partition"

# Maximum-size case, also checks repeated equal values
n = 2000
k = 2000
inp = f"{n} {k}\n" + " ".join(["1000"] * n) + "\n"
assert run(inp) == "2000000", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7`|`7`| tối thiểu`N`Và`K`, với chính xác một đoạn không trống | 
|`5 5 / 1 9 3 7 2`|`22`| Trường hợp ranh giới`K = N`, trong đó mỗi đoạn có độ dài bằng một | 
|`5 3 / 4 4 4 4 4`|`12`| Các giá trị bằng nhau và`<=`điều kiện hợp nhất ngăn xếp | 
|`4 2 / 1 10 2 9`|`19`| Chọn cách cắt tối ưu thay vì cắt tham lam ở cực đại cục bộ | 
|`2000 2000 / 1000 ... 1000`|`2000000`| Tối đa`N`, tối đa`K`và giá trị kích thích tối đa | 

## Vỏ cạnh 

Khi nào`K = 1`, thuật toán chỉ chạy một lớp DP. Đối với đầu vào```
2 1
3 1
```vị trí đầu tiên tạo ra nhóm`(3, 0)`. Ở vị trí thứ hai,`1`nhỏ hơn`3`, nên nó tạo ra một nhóm khác, nhưng nhóm tốt nhất toàn cầu vẫn còn`3`. Câu trả lời cuối cùng là`3`, chính xác là mức tối đa của toàn bộ mảng. 

Khi`K = N`, mỗi hành động phải chứa chính xác một phần tử. Coi như```
3 3
2 7 4
```Lớp đầu tiên tạo ra các giá trị tiền tố một hành động tốt nhất`2, 7, 7`. Lớp thứ hai bắt đầu tại`i = 2`, cho`2 + 7 = 9`và cuối cùng đạt đến giá trị tiền tố chính xác cho hai hành động. Lớp thứ ba chỉ có thể kết thúc ở`i = 3`, trong đó hành động một phần tử cuối cùng đóng góp`4`, sản xuất`2 + 7 + 4 = 13`. 

Các giá trị bằng nhau thực hiện quy tắc hợp nhất ngăn xếp. Vì```
4 2
5 5 1 5
```khi thứ hai`5`đến, nhóm hiện có với tối đa`5`được bật lên vì mức tối đa của nó bằng với giá trị mới. Cả hai điểm bắt đầu hiện có cùng một phân đoạn tối đa, vì vậy việc hợp nhất chúng sẽ không làm mất thông tin. Sau đó, trận chung kết`5`tương tự hợp nhất tất cả các nhóm có mức tối đa hiện tại là nhiều nhất`5`. Điểm tối ưu là`10`. 

Một trường hợp như```
4 2
1 10 2 9
```cho thấy lý do tại sao DP phải bảo toàn nhiều vị trí bắt đầu có thể. Cắt sau phần tử đầu tiên sẽ cho`1 + 10 = 11`, trong khi cắt sau phần tử thứ ba sẽ cho`10 + 9 = 19`. Ngăn xếp đơn điệu giữ các ứng cử viên cho cả hai khả năng cho đến khi điểm cuối bên phải xác định cái nào tốt hơn, do đó thuật toán trả về`19`. 

Đầu vào có kích thước tối đa chứa`2000`các yếu tố và có thể yêu cầu`2000`hành động. Với mọi giá trị bằng`1000`, mọi hành động đều đóng góp chính xác`1000`, vậy câu trả lời là`2,000,000`. Các hoạt động ngăn xếp vẫn tuyến tính trên mỗi lớp DP ngay cả trong trường hợp lặp đi lặp lại nhiều lần này, mang lại yêu cầu`O(KN)`thời gian chạy.
