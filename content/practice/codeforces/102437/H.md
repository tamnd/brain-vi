---
title: "CF 102437H - \u0421\u044d\u043c \u0438 \u0445\u0440\u0430\u043d\u0438\u043b\u0438\u0449\u0435"
description: "Chúng ta có một mảng các giá trị dương a[1..n]. Hai người chơi xử lý nó từ trái sang phải. Trong mỗi lượt, người chơi hiện tại có thể loại bỏ bất kỳ số phần tử nào vẫn chưa được sử dụng ở phía trước, sau đó lấy phần tử tiếp theo."
date: "2026-08-12T08:06:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "H"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 169
verified: true
draft: false
---

[CF 102437H - \u0421\u044d\u043c \u0438 \u0445\u0440\u0430\u043d\u0438\u043b\u0438\u0449\u0435](https://codeforces.com/problemset/problem/102437/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt các giá trị tích cực`a[1..n]`. Hai người chơi xử lý nó từ trái sang phải. Trong mỗi lượt, người chơi hiện tại có thể loại bỏ bất kỳ số phần tử nào vẫn chưa được sử dụng ở phía trước, sau đó lấy phần tử tiếp theo. Vì vậy, sau khi người chơi vào vị trí`j`, mọi vị trí cho đến`j`đã biến mất và người chơi tiếp theo bắt đầu từ`j + 1`. 

Người chơi có thể bỏ qua các phần tử vì họ được phép hủy một số tiền tố trước khi lấy một phần tử. Mục tiêu không phải là tối đa hóa tổng các phần tử được lấy riêng lẻ mà là tối đa hóa sự khác biệt giữa tổng của người chơi và tổng của đối thủ. Sam di chuyển trước nên chúng ta cần giá trị tối ưu của`Sam's sum - Catcher's sum`. 

Mảng có thể chứa tới`200000`các phần tử, trong khi mọi giá trị có thể lớn bằng`10^9`. Tuyên bố chính thức đưa ra giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. MỘT`O(n^2)`thuật toán sẽ thực hiện khoảng`n(n+1)/2`, hoặc đại khái`20,000,100,000`, quá trình chuyển đổi ứng viên ở kích thước tối đa, vượt xa những gì phù hợp trong thời hạn. Chúng tôi cần một`O(n)`hoặc gần`O(n log n)`giải pháp. Bản thân câu trả lời có thể lớn như`10^9`và số nguyên Python xử lý nó một cách an toàn mà không cần bất kỳ cách xử lý tràn đặc biệt nào. 

Có một số trường hợp nhỏ bộc lộ những lỗi phổ biến. Vì`n = 1`Và`a = [7]`, câu trả lời là`7`, vì Sam chỉ lấy phần tử duy nhất. Vì`n = 2`Và`a = [1, 100]`, câu trả lời là`100`: Sam có thể loại bỏ`1`và ngay lập tức lấy`100`, không để lại gì cho người chơi thứ hai. Một giải pháp giả định mọi yếu tố phải được thực hiện sẽ nhận được sai`-99`. Ngược lại, đối với`n = 2`Và`a = [5, 1]`, Sam nên lấy`5`, sau đó đối thủ nhận được`1`, cho`4`. Một quy tắc tham lam luôn nhảy đến giá trị còn lại lớn nhất có tác dụng ở đây, nhưng đó không phải là lý do giải pháp đúng. 

Những giá trị bình đẳng cũng đáng được quan tâm. Vì`[1, 1, 1, 1]`, câu trả lời là`1`, không`0`hoặc`2`. Sam có thể lấy một cái`1`, và đối thủ có thể lấy cái khác, nhưng người chơi cũng có thể bỏ qua các phần tử. Sự lặp lại chính xác phải tính đến mọi vị trí tiếp theo có thể xảy ra thay vì giả định rằng người chơi chỉ đơn giản luân phiên qua các phần tử liền kề. 

## Phương pháp tiếp cận 

Công thức lập trình động trực tiếp đã đủ để mô tả chính xác trò chơi. Cho phép`dp[i]`là chênh lệch điểm số tối ưu cho người chơi đến lượt khi mảng còn lại bắt đầu ở vị trí`i`. Giả sử người chơi chọn vị trí`j`, Ở đâu`j >= i`. Họ nhận được`a[j]`và toàn bộ trò chơi còn lại bắt đầu lúc`j + 1`cùng đối phương di chuyển. Từ quan điểm của người chơi hiện tại, trò chơi trong tương lai đó có giá trị`dp[j + 1]`với đối thủ, do đó nó góp phần`-dp[j + 1]`vào điểm số của người chơi hiện tại. Lựa chọn`j`do đó mang lại`a[j] - dp[j + 1]`. 

Việc đưa ra lựa chọn tốt nhất có thể sẽ mang lại`dp[i] = max(a[j] - dp[j + 1])`cho tất cả`j >= i`,

với`dp[n + 1] = 0`. 

Sự lặp lại này là đúng vì mọi nước đi hợp pháp đều được đặc trưng hoàn toàn bởi vị trí mà người chơi hiện tại lựa chọn. Khi vị trí đó đã được cố định, chỉ còn lại đúng một trò chơi, đó là hậu tố sau vị trí đó. 

Việc triển khai bạo lực đánh giá mức tối đa một cách độc lập cho mọi`i`. Vì`i = 1`nó kiểm tra`n`sự lựa chọn, cho`i = 2`nó kiểm tra`n - 1`, vân vân. Tổng số lần kiểm tra là`n + (n - 1) + ... + 1 = n(n+1)/2`. Tại`n = 200000`, đó là`20,000,100,000`kiểm tra, do đó phiên bản bậc hai không thể vượt qua. 

Quan sát quan trọng là biểu thức được cực đại hóa có dạng đặc biệt. Khi chúng tôi tính toán`dp[i]`, mọi ứng cử viên đều`a[j] - dp[j + 1]`. Giá trị thuộc vị trí`j`không phụ thuộc vào`i`. Khi di chuyển từ phải sang trái, chúng ta chỉ thêm một ứng viên mới, ứng viên tương ứng với vị trí hiện tại. Chúng tôi có thể duy trì số lượng ứng cử viên tối đa được thấy cho đến nay thay vì quét lại toàn bộ hậu tố. Điều này biến phép truy hồi bậc hai thành một bước lùi duy nhất. Sự tối ưu hóa tương tự được mô tả trong bài xã luận chính thức: các giá trị ứng viên được giữ cố định như`i`giảm, do đó mức chạy tối đa là đủ. 

Không cần cây phân đoạn hoặc cấu trúc dữ liệu phức tạp khác. Phạm vi được truy vấn luôn là hậu tố và các hậu tố đó tăng thêm chính xác một phần tử trong quá trình truyền ngược. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt giá trị cho hậu tố trống thành`dp[n + 1] = 0`. Nếu không còn hàm tạo nào thì sẽ không có sự khác biệt về điểm số. 
2. Xử lý các vị trí từ`n`xuống`1`. Tại vị trí`i`, ứng cử viên cho vị trí chính xác này là`a[i] - dp[i + 1]`. Phép trừ xuất hiện vì sau khi lấy`a[i]`, đối thủ trở thành người chơi có chênh lệch điểm tối ưu là`dp[i + 1]`. 
3. Duy trì một biến`best`chứa giá trị lớn nhất của`a[j] - dp[j + 1]`trong số tất cả các vị trí`j`đã được xử lý rồi. Khi xử lý`i`, tính toán`a[i] - dp[i + 1]`và cập nhật`best`với ứng viên này. 
4. Đặt`dp[i] = best`. Điều này có tác dụng vì các lựa chọn hợp pháp từ tiểu bang`i`chính xác là những vị trí`i, i + 1, ..., n`, Và`best`chính xác là ứng cử viên tối đa cho hậu tố đó. 
5. Vị trí sau khi xử lý`1`, đầu ra`dp[1]`, bởi vì trạng thái đó là trò chơi gốc và Sam là người chơi thực hiện nước đi đầu tiên. 

Điều bất biến là sau khi xử lý vị trí`i`,`best`bằng mức tối đa của`a[j] - dp[j + 1]`trên mọi`j >= i`. Do đó`dp[i]`chính xác là kết quả tối ưu của trò chơi bắt đầu lúc`i`. Mỗi nước đi đầu tiên hợp pháp được đại diện bởi một ứng cử viên và sự lặp lại tính đến sự tiếp tục tối ưu sau nước đi đó. Vì sự lặp lại được đánh giá từ phải sang trái nên mọi yêu cầu`dp[j + 1]`đã được biết trước khi ứng cử viên của nó được xem xét. Do đó, mức tối đa đang chạy không thể bỏ qua một động thái hợp pháp hoặc đưa ra một động thái bất hợp pháp. 

Có một sự đơn giản hóa bổ sung trong việc thực hiện. Chúng ta thực sự không cần phải lưu trữ toàn bộ`dp`mảng. Trong khi xử lý`i`, chỉ một`dp[i + 1]`là cần thiết và mức tối đa đang chạy đã thể hiện`dp[i]`. Chúng ta có thể giữ một biến cho giá trị DP trước đó và một biến cho giá trị DP tối đa hiện tại. Điều này làm giảm bộ nhớ phụ xuống còn`O(1)`ngoài mảng đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(n, a):
    dp_next = 0
    best = -10**30

    for i in range(n - 1, -1, -1):
        candidate = a[i] - dp_next
        if candidate > best:
            best = candidate
        dp_next = best

    return dp_next

def main():
    n = int(input())
    a = list(map(int, input().split()))
    print(solve(n, a))

if __name__ == "__main__":
    main()
```Mảng được đọc một lần, sau đó vòng lặp bắt đầu ở phần tử cuối cùng của nó.`dp_next`đại diện cho`dp[i + 1]`, do đó nó được khởi tạo về 0 trước khi xử lý vị trí cuối cùng. Ứng viên`a[i] - dp_next`chính xác là giá trị thu được bằng cách chọn vị trí`i`. 

Sau khi tính toán ứng viên,`best`được cập nhật trước khi gán nó cho`dp_next`. Thứ tự này quan trọng. cái mới`dp[i]`bao gồm khả năng lấy phần tử hiện tại, vì vậy ứng viên hiện tại phải là một phần của`best`trước`dp_next`trở thành giá trị cho lần lặp tiếp theo. 

Việc khởi tạo`-10**30`là an toàn dưới mọi ứng cử viên có thể. Thực ra, bởi vì tất cả`a[i]`là dương và chênh lệch điểm được giới hạn bởi tổng mảng, số nguyên Python thông thường đã là quá đủ. Vòng lặp thực hiện chính xác`n`các lần lặp và mỗi lần lặp sử dụng số học và so sánh theo thời gian không đổi. 

Giải pháp sử dụng`O(n)`bộ nhớ cho mảng đầu vào và chỉ`O(1)`bộ nhớ bổ sung cho trạng thái lập trình động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`a = [1, 2, 3]`, chúng ta xử lý mảng từ phải sang trái. 

| Chức vụ`i`|`a[i]`|`dp_next`| Ứng viên`a[i] - dp_next`|`best`|`dp[i]`| 
| --- | --- | --- | --- | --- | --- | 
| 3 | 3 | 0 | 3 | 3 | 3 | 
| 2 | 2 | 3 | -1 | 3 | 3 | 
| 1 | 1 | 3 | -2 | 3 | 3 | 

Người chơi đầu tiên cuối cùng có thể nhận được sự khác biệt`3`. Bước đi tối ưu đầu tiên là loại bỏ`1`Và`2`và lấy`3`, kết thúc trò chơi ngay lập tức. Mức tối đa đang chạy sẽ duy trì chính xác tùy chọn này khi các vị trí trước đó được xử lý. 

### Mẫu 2 

cho`a = [3, 2, 1]`, tính toán ngược tương tự cho: 

| Chức vụ`i`|`a[i]`|`dp_next`| Ứng viên`a[i] - dp_next`|`best`|`dp[i]`| 
| --- | --- | --- | --- | --- | --- | 
| 3 | 1 | 0 | 1 | 1 | 1 | 
| 2 | 2 | 1 | 1 | 1 | 1 | 
| 1 | 3 | 1 | 2 | 2 | 2 | 

Ở vị trí đầu tiên, chiếm`3`lá`[2, 1]`, có giá trị tối ưu cho đối thủ là`1`, vậy là Sam nhận được`3 - 1 = 2`. Sự thay thế của việc bỏ qua`2`hoặc`1`không tốt hơn. Kết quả là`2`, phù hợp với mẫu 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử mảng tạo ra chính xác một ứng cử viên và một bản cập nhật tối đa. | 
| Không gian | O(n) | Mảng đầu vào chiếm bộ nhớ O(n); bản thân DP sử dụng bộ nhớ bổ sung O(1). | 

Với`n <= 200000`, đường tuyến tính chỉ thực hiện vài trăm nghìn phép tính số học và phép so sánh. Mảng đầu vào chứa tối đa`200000`số nguyên, nằm trong giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(n, a):
    dp_next = 0
    best = -10**30

    for i in range(n - 1, -1, -1):
        candidate = a[i] - dp_next
        if candidate > best:
            best = candidate
        dp_next = best

    return dp_next

def run(inp: str) -> str:
    data = inp.split()
    n = int(data[0])
    a = list(map(int, data[1:]))
    return str(solve(n, a)) + "\n"

# Provided samples
assert run("3\n1 2 3\n") == "3\n", "sample 1"
assert run("3\n3 2 1\n") == "2\n", "sample 2"

# Minimum-size input
assert run("1\n7\n") == "7\n", "single constructor"

# Skipping the first element is optimal
assert run("2\n1 100\n") == "100\n", "must allow skipping"

# Taking the first element is optimal
assert run("2\n5 1\n") == "4\n", "must account for opponent's response"

# All values equal
assert run("4\n1 1 1 1\n") == "1\n", "equal values"

# Maximum-size input
n = 200000
inp = str(n) + "\n" + ("1 " * (n - 1)) + "1\n"
assert run(inp) == "1\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`7`| Kích thước tối thiểu và ranh giới hậu tố trống | 
|`2 / 1 100`|`100`| Bỏ qua một số hàm tạo ban đầu có thể là tối ưu | 
|`2 / 5 1`|`4`| Sự tiếp tục tối ưu của đối thủ phải bị trừ đi | 
|`4 / 1 1 1 1`|`1`| Giá trị bằng nhau và cập nhật tối đa lặp đi lặp lại | 
|`200000 / all ones`|`1`| Kích thước đầu vào tối đa và hiệu suất tuyến tính | 

## Vỏ cạnh 

Đối với một hàm tạo duy nhất, đầu vào```
1
7
```bắt đầu bằng`dp[2] = 0`. Ứng cử viên duy nhất là`7 - 0 = 7`, Vì thế`best`Và`dp[1]`cả hai đều trở thành`7`. Đầu ra của thuật toán`7`, xử lý chính xác ranh giới hậu tố mà không yêu cầu trường hợp đặc biệt. 

Đối với đầu vào```
2
1 100
```vị trí cuối cùng mang lại`dp[2] = 100`. Tại vị trí`1`, lấy`1`sẽ sản xuất`1 - 100 = -99`, nhưng ứng cử viên cho vị trí`2`đã được lưu trữ trong`best`BẰNG`100`. Như vậy`dp[1] = 100`. Điều này nắm bắt quy tắc quan trọng là Sam có thể phá hủy hàm tạo đầu tiên và lấy hàm tạo thứ hai, sau đó mảng trống và đối thủ không được di chuyển. 

Đối với đầu vào```
2
5 1
```vị trí cuối cùng mang lại`dp[2] = 1`. Tại vị trí`1`, lấy`5`đưa cho ứng viên`5 - 1 = 4`, trong khi lấy phần tử thứ hai sẽ cho`1`. Tối đa là`4`, nên Sam lấy`5`và đối thủ lấy`1`. Điều này chứng tỏ tại sao việc chỉ lấy giá trị lớn nhất được thấy cho đến nay không phải là đối số DP thực tế. Giá trị của một phần tử phụ thuộc vào cách chơi tối ưu còn lại sau phần tử đó. 

Đối với đầu vào hoàn toàn bằng nhau```
4
1 1 1 1
```các trạng thái lạc hậu là`dp[4] = 1`,`dp[3] = 1`,`dp[2] = 1`, Và`dp[1] = 1`. Ở mọi vị trí, ứng viên mới đều`1 - 1 = 0`, ngoại trừ vị trí cuối cùng nơi nó`1`. Do đó, mức chạy tối đa vẫn còn`1`. Điều này xác nhận rằng thuật toán xử lý các ứng cử viên bằng nhau lặp lại mà không dựa vào các bất đẳng thức nghiêm ngặt hoặc các giá trị khác biệt. 

Hộp có kích thước tối đa bao gồm`200000`các phần tử đều bằng nhau`1`. Mỗi ứng viên được tính một lần nên vòng lặp vẫn chỉ thực hiện`200000`lần lặp lại. Câu trả lời vẫn còn`1`, trong khi công thức bậc hai sẽ cố gắng khoảng`20`tỷ lượt kiểm tra ứng viên. Đây là trường hợp phân biệt rõ ràng nhất giải pháp được chấp nhận với việc triển khai lập trình động đơn giản.
