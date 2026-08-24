---
title: "CF 102163K - Masaoud YÊU PIZZA"
description: "Chúng ta có một mảng A biểu thị những lát bánh pizza trên đĩa của học sinh đứng thành một hàng cố định. Masaoud phải chọn một phân đoạn liền kề của mảng này, nghĩa là anh ta chọn một số điểm cuối bên trái l và điểm cuối bên phải r, và đánh cắp mọi lát cắt trong A[l..r]."
date: "2026-08-23T14:22:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "K"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 1644
verified: true
draft: false
---

[CF 102163K - Masaoud YÊU PIZZAS](https://codeforces.com/problemset/problem/102163/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 27p 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng`A`thể hiện những lát bánh pizza trên đĩa của học sinh đứng thành một hàng cố định. Masaoud phải chọn một đoạn liền kề của mảng này, nghĩa là anh ta chọn một số điểm cuối bên trái`l`và điểm cuối bên phải`r`, và đánh cắp mọi lát trong`A[l..r]`. Chúng ta cần đếm xem có bao nhiêu phân đoạn liền kề khác nhau có tổng tổng nhỏ hơn`X`. 

Từ "nghiêm túc" quan trọng. Một phân đoạn có tổng chính xác`X`không hợp lệ. Vì mọi`A[i]`là dương, việc mở rộng một đoạn chỉ có thể làm tăng tổng của nó. Tính đơn điệu đó là đặc tính làm cho lời giải thời gian tuyến tính có thể thực hiện được. 

Với`N`lớn như`10^5`, việc kiểm tra từng cặp điểm cuối đã quá tốn kém rồi. có`N(N+1)/2`các phân đoạn liền kề, đó là về`5 * 10^9`khi`N = 10^5`. Một giải pháp kiểm tra từng phân đoạn riêng lẻ không thể phù hợp với giới hạn thời gian 1 giây. Chúng ta cần một thuật toán gần`O(N)`mỗi trường hợp thử nghiệm. Các giá trị của`A[i]`Và`X`có thể đạt được`10^9`, và câu trả lời có thể xoay quanh`5 * 10^9`, do đó việc triển khai cũng cần một loại số nguyên có khả năng lưu trữ các giá trị lớn hơn số nguyên 32 bit. Số nguyên Python xử lý việc này một cách tự nhiên. 

Một sai lầm phổ biến về ranh giới là xử lý một số tiền bằng`X`hợp lệ. Ví dụ, với`N = 1`,`X = 4`, Và`A = [4]`, đoạn duy nhất có tổng`4`, vậy câu trả lời là`0`, không`1`. Điều kiện là`sum < X`, không`sum <= X`. 

Một sai lầm khác là quên rằng một phần tử đơn lẻ cũng có thể khiến cửa sổ không hợp lệ. Vì`N = 2`,`X = 3`, Và`A = [5, 1]`, không`[5]`cũng không`[5, 1]`là hợp lệ, trong khi`[1]`là hợp lệ, vì vậy câu trả lời là`1`. Việc triển khai cửa sổ trượt phải liên tục di chuyển điểm cuối bên trái của nó cho đến khi tổng hiện tại hợp lệ trở lại. 

Vấn đề thứ ba là giả định rằng câu trả lời phù hợp với số nguyên 32 bit. Với`N = 100000`,`X = 100001`, và mọi`A[i] = 1`, mọi phân đoạn liền kề không trống đều hợp lệ. Câu trả lời là`100000 * 100001 / 2 = 5,000,050,000`, lớn hơn`2^32`chỉ ở dưới nó một chút nhưng đã vượt xa phạm vi 32-bit đã ký. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là liệt kê mọi cặp điểm cuối có thể. Đối với mỗi vị trí xuất phát`l`, chúng ta có thể mở rộng`r`từ`l`bởi vì`N - 1`, duy trì tổng hiện tại và tăng câu trả lời bất cứ khi nào tổng đó nhỏ hơn`X`. Điều này đúng vì mọi phân đoạn liền kề không trống có chính xác một cặp điểm cuối, vì vậy mỗi phân đoạn hợp lệ sẽ được tính một lần. 

Ngay cả khi chúng ta duy trì tổng hiện hành thay vì tính toán lại từ đầu, vẫn có`N(N+1)/2`cặp điểm cuối. Vì`N = 10^5`, đó là`5,000,050,000`kiểm tra phân đoạn trong trường hợp xấu nhất. Điều này vượt xa giới hạn 1 giây cho phép. 

Phương pháp brute-force hoạt động hiệu quả vì nó kiểm tra rõ ràng mọi phân khúc ứng viên. Thất bại vì có quá nhiều ứng viên Quan sát chính là tất cả các giá trị mảng đều dương. Giả sử cửa sổ hiện tại có tổng nhỏ hơn`X`. Nếu chúng ta mở rộng điểm cuối bên phải của nó thì tổng chỉ có thể tăng lên. Ngược lại, nếu cửa sổ trở nên quá lớn, việc di chuyển điểm cuối bên trái của nó sang bên phải chỉ có thể làm giảm tổng. 

Điều đó có nghĩa là chúng ta có thể duy trì một cửa sổ trượt hợp lệ cho mọi điểm cuối bên phải. Đối với điểm cuối bên phải cố định`r`, cho phép`l`là điểm cuối bên trái nhỏ nhất sao cho`A[l..r]`có tổng nhỏ hơn`X`. Vì tất cả các giá trị đều dương nên mọi đoạn đều kết thúc tại`r`và bắt đầu từ bất cứ đâu`l`bởi vì`r`cũng hợp lệ. Có chính xác`r - l + 1`những phân khúc như vậy. 

Chúng ta có thể tìm thấy giá trị nhỏ nhất này`l`bằng cách di chuyển con trỏ trái về phía trước bất cứ khi nào tổng hiện tại ít nhất`X`. Mỗi phần tử vào cửa sổ một lần và rời khỏi cửa sổ nhiều nhất một lần, do đó tổng số chuyển động của con trỏ là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Cửa Sổ Trượt | O(N) | O(N) cho mảng đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng cả hai con trỏ ở đầu mảng, vì vậy`left = 0`, và giữ`current_sum = 0`Và`answer = 0`. 
2. Di chuyển`right`từ`0`ĐẾN`N - 1`. Thêm vào`A[right]`ĐẾN`current_sum`, vì điểm cuối bên phải mới có nghĩa là phần tử này hiện thuộc về cửa sổ hiện tại. 
3. Trong khi`current_sum >= X`, di chuyển`left`chuyển tiếp và trừ`A[left]`từ`current_sum`trước khi tăng`left`. Vòng lặp là cần thiết vì một lần loại bỏ có thể không đủ để làm cho tổng nhỏ hơn`X`. 
4. Khi vòng lặp kết thúc, cửa sổ hiện tại`A[left..right]`có tổng nhỏ hơn`X`. Vì tất cả các phần tử đều dương nên mọi đoạn đều kết thúc tại`right`có điểm cuối bên trái nằm giữa`left`Và`right`cũng có tổng nhỏ hơn`X`. 
5. Thêm`right - left + 1`ĐẾN`answer`. Điều này đếm chính xác những phân đoạn hợp lệ kết thúc ở vị trí hiện tại. 
6. Lặp lại cho đến khi mọi điểm cuối bên phải có thể được xử lý, sau đó xuất ra`answer`. 

### Tại sao nó hoạt động 

Sau mỗi lần lặp,`left`là chỉ mục nhỏ nhất mà cửa sổ hiện tại`A[left..right]`có tổng nhỏ hơn`X`. Bất kỳ đoạn nào kết thúc tại`right`và bắt đầu trước`left`chứa cửa sổ hợp lệ hiện tại cộng với ít nhất một phần tử dương bổ sung, do đó tổng của nó ít nhất là`X`và nó không thể hợp lệ. Mỗi đoạn bắt đầu từ`left`hoặc muộn hơn là phân đoạn con của cửa sổ hợp lệ và do đó có tổng dương thậm chí còn nhỏ hơn hoặc bằng nhau, vì vậy nó hợp lệ. Như vậy chính xác`right - left + 1`phân đoạn hợp lệ kết thúc tại`right`. 

Bởi vì`right`chỉ di chuyển về phía trước và`left`cũng chỉ di chuyển về phía trước, không có phần tử nào được thêm vào hoặc xóa khỏi cửa sổ trượt nhiều lần. Do đó, thuật toán xử lý toàn bộ mảng theo thời gian tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))

        left = 0
        current_sum = 0
        answer = 0

        for right in range(n):
            current_sum += a[right]

            while current_sum >= x:
                current_sum -= a[left]
                left += 1

            answer += right - left + 1

        print(answer)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc một lần cho mỗi trường hợp kiểm thử và mảng được lưu trữ sao cho con trỏ bên trái có thể trừ các phần tử khi cửa sổ trở nên quá lớn. Vòng lặp chính tương ứng trực tiếp với bước con trỏ bên phải của thuật toán. 

các`while current_sum >= x`điều kiện sử dụng`>=`, còn hơn là`>`, bởi vì một tổng chính xác bằng`X`không hợp lệ. Sau vòng lặp, bất biến là`current_sum < x`. 

biểu thức`right - left + 1`đếm các vị trí bắt đầu có thể có của một phân đoạn hợp lệ kết thúc tại`right`. Cả hai điểm cuối đều được bao gồm, vì vậy`+1`là cần thiết. Ví dụ, nếu`left == right`, có chính xác một đoạn, phần tử duy nhất tại`right`. 

Kiểu số nguyên của Python tránh tràn khi câu trả lời đạt tới hàng tỷ. Trong các ngôn ngữ có loại số nguyên có chiều rộng cố định, câu trả lời phải được lưu trữ ở dạng số nguyên 64 bit. 

## Ví dụ đã hoạt động 

Đối với trường hợp kiểm thử mẫu đầu tiên, có thể có một học sinh và một phân đoạn. 

| đúng | giá trị gia tăng | tổng hiện tại trước khi thu hẹp | còn lại sau khi thu nhỏ | phân đoạn hợp lệ kết thúc ở bên phải | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | 0 | 1 | 1 | 

tổng`3`đúng là ít hơn`X = 4`, do đó đoạn đơn`[3]`là hợp lệ. Câu trả lời là`1`. 

Đối với trường hợp thử nghiệm mẫu thứ hai,`A = [1, 5]`Và`X = 4`. 

| đúng | giá trị gia tăng | tổng sau khi thêm | còn lại sau khi thu nhỏ | phân đoạn hợp lệ kết thúc ở bên phải | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 1 | 1 | 
| 1 | 5 | 6 | 2 | 0 | 1 | 

Khi`5`được thêm vào, tổng sẽ trở thành`6`, do đó thuật toán loại bỏ`A[0]`, để lại số tiền`5`. Tổng số tiền ít nhất vẫn là`4`, vì vậy nó loại bỏ`A[1]`cũng vậy. Hiện nay`left = 2`, đó là một vị trí vượt quá`right`. Không có phân đoạn nào trống hợp lệ kết thúc ở vị trí`1`. Câu trả lời cuối cùng vẫn còn`1`. 

Dấu vết thứ hai chứng minh tại sao bước rút gọn phải là một bước`while`vòng lặp. Việc loại bỏ một lần không phải lúc nào cũng đủ khi ít nhất một phần tử riêng lẻ đã có`X`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) cho mỗi trường hợp thử nghiệm |`right`tiến lên N lần và`left`cũng tiến bộ nhiều nhất là N lần | 
| Không gian | O(N) | Mảng được lưu trữ để có thể xóa các phần tử khỏi phía bên trái của cửa sổ | 

Thời gian chạy tuyến tính phù hợp cho`N = 10^5`và giới hạn 1 giây, giả sử tổng kích thước đầu vào nằm trong giới hạn dự định của vấn đề. Thuật toán không thực hiện phép lặp lồng nhau trên tất cả các cặp điểm cuối, đây là điểm khác biệt quan trọng so với giải pháp brute-force. Các số nguyên có độ chính xác tùy ý của Python cũng xử lý câu trả lời lớn nhất có thể một cách an toàn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())

    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))

        left = 0
        current_sum = 0
        answer = 0

        for right in range(n):
            current_sum += a[right]

            while current_sum >= x:
                current_sum -= a[left]
                left += 1

            answer += right - left + 1

        print(answer)

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
assert run("""2
1 4
3
2 4
1 5
""") == """1
1
""", "provided sample"

# Minimum-size input
assert run("""1
1 1
1
""") == """0
""", "single element equal to X is invalid"

# Strict boundary: sums equal to X must not be counted
assert run("""1
3 3
1 1 1
""") == """5
""", "only segments of length 1 and 2 are valid"

# All values are equal and every nonempty segment is valid
assert run("""1
4 10
2 2 2 2
""") == """10
""", "all 10 subarrays are valid"

# Maximum-size case, all elements equal to 1, every segment is valid
assert run("1\n100000 100001\n" + " ".join(["1"] * 100000) + "\n") == """5000050000
""", "large answer and 64-bit boundary"

# A value larger than X forces the window to become empty
assert run("""1
3 4
1 5 1
""") == """2
""", "single element larger than X"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / [1]`|`0`| Kích thước tối thiểu và bình đẳng với`X`| 
|`1 / 3 3 / [1,1,1]`|`5`| Bất bình đẳng nghiêm ngặt và xử lý từng người một | 
|`1 / 4 10 / [2,2,2,2]`|`10`| Tất cả các mảng con đều hợp lệ | 
|`1 / 100000 100001 / [1,...,1]`|`5000050000`| Kích thước tối đa và câu trả lời lớn | 
|`1 / 3 4 / [1,5,1]`|`2`| Một phần tử riêng lẻ có thể vượt quá`X`| 

## Vỏ cạnh 

Khi tổng phân đoạn chính xác`X`, nó phải được loại trừ. Coi như`N = 1`,`X = 4`, Và`A = [4]`. Thuật toán bổ sung`4`, thấy thế`current_sum >= X`, loại bỏ`A[0]`, và những tiến bộ`left`ĐẾN`1`. Cửa sổ hiện tại trống, vì vậy`right - left + 1 = 0`. Đầu ra là`0`, xử lý chính xác bất đẳng thức nghiêm ngặt. 

Khi một phần tử lớn hơn`X`, thuật toán có thể di chuyển`left`vượt quá hiện tại`right`. Vì`N = 3`,`X = 4`, Và`A = [1, 5, 1]`, sau khi xử lý`1`câu trả lời là`1`. Sau khi thêm`5`, tổng là`6`, do đó thuật toán loại bỏ`1`, rời đi`5`, sau đó loại bỏ`5`, để lại một cửa sổ trống với`left = 2`. Không có phân đoạn nào kết thúc ở chỉ mục`1`là hợp lệ. Sau khi thêm phần cuối cùng`1`, cửa sổ chỉ chứa phần tử đó nên một đoạn nữa sẽ được tính. Câu trả lời cuối cùng là`2`, tương ứng với`[1]`ở mỗi đầu. 

Câu trả lời lớn là một trường hợp khác có thể âm thầm phá vỡ việc triển khai bằng cách sử dụng số nguyên 32 bit. Với`100000`các phần tử đều bằng nhau`1`Và`X = 100001`, tổng phân đoạn tối đa có thể là`100000`, vì vậy mọi phân đoạn không trống đều hợp lệ. Thuật toán bổ sung`1 + 2 + ... + 100000`, thu được`5,000,050,000`. Python lưu trữ giá trị này mà không bị tràn và quá trình kiểm tra xác nhận rằng biểu thức đếm là chính xác ngay cả ở tỷ lệ lớn nhất. 

Cuối cùng, khi tất cả các phần tử đều dương thì tính đơn điệu của cửa sổ trượt được đảm bảo. Ví dụ, với`A = [2,2,2,2]`Và`X = 10`, mọi phân đoạn đều có tổng dưới đây`10`, Vì thế`left`không bao giờ di chuyển. Tại mỗi điểm cuối bên phải, thuật toán sẽ thêm`1`, sau đó`2`, sau đó`3`, sau đó`4`, cho`10`tổng số phân đoạn hợp lệ. Điều này thể hiện tính bất biến cốt lõi: khi một cửa sổ hợp lệ, mọi hậu tố của cửa sổ đó cũng hợp lệ vì việc loại bỏ các phần tử dương không thể làm tăng tổng của nó.
