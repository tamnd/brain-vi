---
title: "CF 102646D - Lựa chọn đội"
description: "Bài toán mô phỏng quá trình lựa chọn đội bóng rổ. Có n người chơi đứng theo một thứ tự cố định và người chơi i có giá trị kỹ năng là a[i]. Chúng ta phải chọn chính xác k người chơi mà vẫn giữ nguyên thứ tự ban đầu của họ."
date: "2026-07-30T23:07:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102646
codeforces_index: "D"
codeforces_contest_name: "Testing Round #XVII"
rating: 0
weight: 102646
solve_time_s: 258
verified: true
draft: false
---

[CF 102646D - Lựa chọn nhóm](https://codeforces.com/problemset/problem/102646/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô phỏng quá trình lựa chọn đội bóng rổ. có`n`người chơi đứng theo một thứ tự cố định và người chơi`i`có giá trị kỹ năng`a[i]`. Chúng ta phải lựa chọn chính xác`k`người chơi trong khi vẫn giữ nguyên trật tự ban đầu của họ. Người chơi được chọn đầu tiên nhận được số nhân`b[1]`, người thứ hai nhận được`b[2]`, vân vân. Mục tiêu là tối đa hóa tổng số tiền đóng góp, tức là tổng kỹ năng của người chơi được chọn nhân với hệ số được chỉ định của họ. 

Dữ liệu đầu vào cung cấp cho người chơi các kỹ năng theo thứ tự đội hình và`k`số nhân mô tả giá trị của từng vị trí được chọn trong nhóm. Đầu ra là tổng giá trị tối đa có thể có sau khi chọn chuỗi người chơi tốt nhất. 

Hạn chế chính là`n <= 1000`. Giá trị này đủ nhỏ để lập trình động bậc hai là thực tế. Một giải pháp xung quanh`O(n^2)`thật thoải mái, nhưng hãy thử mọi cách có thể`k`người chơi là không thể vì số lượng các chuỗi con tăng lên theo kiểu tổ hợp. Các giá trị của`a[i]`Và`b[i]`có thể đạt được`100000`, vì vậy câu trả lời có thể xoay quanh`10^13`, nghĩa là việc triển khai phải sử dụng số nguyên 64 bit. Số nguyên Python đã xử lý phạm vi này. 

Phần tinh tế nhất là những cầu thủ giỏi nhất không nhất thiết phải có giá trị kỹ năng lớn nhất. Một người chơi có giá trị kỹ năng lớn có thể có giá trị hơn khi được chỉ định cho hệ số nhân lớn hơn sau này, trong khi người chơi nhỏ hơn có thể cần được giữ ở vị trí sớm hơn. Ràng buộc thứ tự ngăn cản việc sắp xếp người chơi một cách đơn giản. 

Ví dụ:```
Input:
3 2
10 1 10
1 100

Output:
1010
```Một giải pháp bất cẩn là sắp xếp người chơi theo kỹ năng có thể chọn hai người chơi có kỹ năng`10`, điều này xảy ra ở đây nhưng lý do vẫn chưa đầy đủ. Phần quan trọng là chỉ định hệ số nhân thứ hai cho người chơi được chọn sau người chơi được chọn đầu tiên. Các lựa chọn hợp lệ là các chuỗi con, không phải các nhóm tùy ý. 

Một trường hợp đặc biệt khác xuất hiện khi số nhân khác nhau rất nhiều:```
Input:
3 2
100 50 1
1 100

Output:
5100
```Việc chọn hai người chơi đầu tiên mang lại`100 * 1 + 50 * 100 = 5100`. Một cách tiếp cận tham lam kết hợp kỹ năng lớn nhất với hệ số nhân lớn nhất và bỏ qua các vị trí có thể cố gắng sử dụng người chơi thứ nhất và thứ ba, mang lại cho`100 * 1 + 1 * 100 = 200`, điều đó còn tệ hơn. 

Trường hợp nhỏ nhất có thể cũng cần chú ý:```
Input:
1 1
7
9

Output:
63
```Chỉ có một lựa chọn có thể. Bất kỳ quá trình khởi tạo DP nào giả định có ít nhất một trình phát bị bỏ qua đều có thể thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là liệt kê mọi nhóm có thể`k`người chơi trong khi vẫn giữ nguyên thứ tự ban đầu của họ. Đối với mỗi chuỗi con được chọn, chúng tôi tính toán mức đóng góp bằng cách nhân kỹ năng được chọn đầu tiên với`b[1]`, thứ hai bằng`b[2]`, vân vân. Điều này đúng vì nó kiểm tra mọi đội hợp lệ. 

Vấn đề là số lượng đội có thể. Từ`n`người chơi, số cách chọn`k`là`C(n, k)`. Khi`n = 1000`, điều này có thể lớn về mặt thiên văn. Ngay cả khi việc đánh giá một đội chỉ mất`k`hoạt động thì tổng khối lượng công việc vẫn vượt xa giới hạn. 

Điều quan trọng cần lưu ý là thông tin duy nhất quan trọng khi quét người chơi từ trái sang phải là số lượng người chơi đã được chọn. Nếu chúng tôi đã xử lý một số tiền tố của dòng sản phẩm và chọn`j`người chơi, danh tính chính xác của những người chơi được chọn đó không còn quan trọng nữa ngoại trừ thông qua tổng giá trị tốt nhất đạt được. Cấu trúc bài toán con chồng chéo này dẫn đến lập trình động một cách tự nhiên. 

Cách tiếp cận bạo lực có hiệu quả vì mọi quyết định có thể đều được xem xét, nhưng nó lặp lại nhiều lần các lựa chọn từng phần giống hệt nhau. Quan sát rằng hai đội có cùng tiền tố được xử lý và cùng số lượng người chơi được chọn chỉ cần điểm cao hơn cho phép chúng tôi nén tất cả những khả năng đó vào một trạng thái nhỏ. 

Chúng tôi xác định`dp[j]`là số điểm tối đa có thể sau khi xử lý một số tiền tố của người chơi và chọn chính xác`j`người chơi. Khi xem xét một người chơi mới, chúng ta hoặc bỏ qua họ hoặc sử dụng họ làm đối tượng.`(j+1)`-người chơi được chọn thứ Việc cập nhật ngược các trạng thái cho phép chúng ta chỉ lưu trữ một mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n,k) * k) | O(k) | Quá chậm | 
| Tối ưu | O(nk) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`dp[0] = 0`và mọi trạng thái khác là không thể. Nhà nước`dp[j]`đại diện cho số điểm tốt nhất sau khi chọn chính xác`j`người chơi từ những người chơi được xử lý cho đến nay. 
2. Xử lý từng người chơi từ trái sang phải. Dành cho người chơi hiện tại có giá trị kỹ năng`x`, hãy thử chỉ định họ làm người chơi được chọn tiếp theo. 
3. Cập nhật số lượng lựa chọn từ`k-1`xuống tới`0`. Nếu như`j`người chơi đã được chọn, việc chọn người chơi hiện tại sẽ mang lại một giá trị mới là`dp[j] + x * b[j+1]`. 

Thứ tự ngược lại là cần thiết vì người chơi hiện tại chỉ có thể được chọn một lần. Cập nhật từ nhỏ`j`to lớn`j`sẽ cho phép cùng một người chơi đóng góp nhiều lần trong một lần lặp. 
4. Sau khi tất cả người chơi đã được xử lý, câu trả lời là`dp[k]`, vì trạng thái này thể hiện việc chọn chính xác số lượng người chơi cần thiết. 

Bất biến là sau khi xử lý bất kỳ tiền tố nào của dòng,`dp[j]`chứa giá trị tốt nhất có thể trong số tất cả các cách để chọn chính xác`j`người chơi từ tiền tố đó. Việc bỏ qua trình phát hiện tại sẽ giữ tất cả các trạng thái cũ hợp lệ. Việc chọn trình phát hiện tại sẽ mở rộng mọi trạng thái hợp lệ với`j`người chơi được chọn vào trạng thái hợp lệ với`j+1`những cầu thủ được chọn. Vì đây là hai quyết định duy nhất có thể xảy ra nên mọi đội hợp lệ đều được xem xét và điểm cao nhất sẽ được giữ lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    neg = -10**30
    dp = [neg] * (k + 1)
    dp[0] = 0

    for x in a:
        for j in range(k - 1, -1, -1):
            if dp[j] != neg:
                dp[j + 1] = max(dp[j + 1], dp[j] + x * b[j])

    print(dp[k])

if __name__ == "__main__":
    solve()
```Mảng`dp`chỉ lưu trữ các giá trị tốt nhất hiện tại cho mỗi số lượng người chơi được chọn có thể. Các trạng thái không thể bắt đầu bằng một số rất nhỏ nên chúng không bao giờ trở thành mức tối đa hợp lệ. 

Khi xử lý một trình phát, vòng lặp bắt đầu lúc`k - 1`và di chuyển xuống dưới. Hệ số để chọn người chơi hiện tại sau khi đã chọn`j`người chơi là`b[j]`bởi vì Python sử dụng lập chỉ mục dựa trên số 0, vì vậy`b[0]`là hệ số nhân cho người chơi được chọn đầu tiên. 

Không cần xử lý riêng cho vụ việc`n = k`. DP đương nhiên buộc câu trả lời phải được sử dụng bởi mọi người chơi vì trạng thái cuối cùng phải chứa chính xác`k`các lựa chọn. 

Câu trả lời có thể vượt quá giới hạn số nguyên 32 bit vì mức đóng góp tối đa có thể nằm trong khoảng`1000 * 100000 * 100000`, đó là`10^13`. Số nguyên Python tránh các vấn đề tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
Input:
3 2
1 2 3
2 1
```Các trạng thái quan trọng là: 

| Trình phát đã xử lý | Giá trị người chơi | dp[0] | dp[1] | dp[2] | 
| --- | --- | --- | --- | --- | 
| Không có | | 0 | không thể | không thể | 
| 1 | 1 | 0 | 2 | không thể | 
| 2 | 2 | 0 | 4 | 4 | 
| 3 | 3 | 0 | 6 | 7 | 

Trạng thái cuối cùng là`dp[2] = 7`, đạt được bằng cách chọn người chơi`2`Và`3`. Điều này chứng tỏ cách DP đưa ra những lựa chọn khác nhau có thể thay vì thực hiện ngay một kỹ năng lớn nhất. 

Đối với mẫu thứ ba:```
Input:
7 4
1 9 3 8 19 3 2
50 1 9 3
```Một dấu vết rút gọn: 

| Trình phát đã xử lý | Giá trị người chơi | Sự lựa chọn số 1 tốt nhất | 2 lựa chọn tốt nhất | 3 lựa chọn tốt nhất | 4 lựa chọn tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 50 | không thể | không thể | không thể | 
| 9 | 9 | 450 | 459 | không thể | không thể | 
| 8 | 8 | 450 | 850 | 522 | không thể | 
| 19 | 19 | 950 | 1400 | 621 | 693 | 
| 3 | 3 | 950 | 1400 | 1227 | 648 | 
| 3 | 3 | 950 | 1400 | 1227 | 1308 | 
| 2 | 2 | 950 | 1400 | 1227 | 638 | 

Bảng nhấn mạnh rằng các trạng thái trung gian không đại diện trực tiếp cho câu trả lời cuối cùng. Chúng đại diện cho quy mô nhóm một phần tốt nhất, sau này được mở rộng thành nhóm đầy đủ tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi trong số`n`người chơi cập nhật nhiều nhất`k`trạng thái DP. | 
| Không gian | O(k) | Chỉ số lượng người chơi được chọn hiện tại được lưu trữ. | 

Với`n <= 1000`Và`k <= n`, số lần chuyển đổi tối đa là khoảng một triệu. Điều này dễ dàng phù hợp với giới hạn cuộc thi thông thường, trong khi số lượng đội mạnh mẽ quá lớn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)
    n = int(next(it))
    k = int(next(it))
    a = [int(next(it)) for _ in range(n)]
    b = [int(next(it)) for _ in range(k)]

    neg = -10**30
    dp = [neg] * (k + 1)
    dp[0] = 0

    for x in a:
        for j in range(k - 1, -1, -1):
            if dp[j] != neg:
                dp[j + 1] = max(dp[j + 1], dp[j] + x * b[j])

    return str(dp[k])

assert run("""3 2
1 2 3
2 1
""") == "7", "sample 1"

assert run("""5 4
5 9 10 3 2
10 10 5 5
""") == "215", "sample 2"

assert run("""1 1
7
9
""") == "63", "single player"

assert run("""3 2
100 50 1
1 100
""") == "5100", "large multiplier ordering"

assert run("""4 3
5 5 5 5
2 2 2
""") == "30", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7 / 9`|`63`| Kích thước tối thiểu và lựa chọn bắt buộc | 
|`3 2 / 100 50 1 / 1 100`|`5100`| Phép gán số nhân theo thứ tự | 
|`4 3 / 5 5 5 5 / 2 2 2`|`30`| Giá trị bằng nhau và lựa chọn lặp lại | 

## Vỏ cạnh 

Trường hợp số nhân có kích thước rất khác nhau sẽ được xử lý vì DP không quyết định ngay lập tức vai trò của người chơi. Vì:```
3 2
100 50 1
1 100
```trạng thái sau khi xử lý hai người chơi đầu tiên có khả năng lấy cả hai người chơi, đưa ra`100 * 1 + 50 * 100 = 5100`. Thuật toán giữ nguyên trạng thái này thay vì thay thế nó bằng một lựa chọn tham lam chỉ dựa trên kỹ năng. 

Trường hợp chơi đơn:```
1 1
7
9
```bắt đầu bằng`dp[0] = 0`. Sự chuyển đổi duy nhất tạo ra`dp[1] = 63`, đó là câu trả lời bắt buộc. Không có quyền truy cập không hợp lệ vì vòng lặp cập nhật dừng chính xác tại`k - 1`. 

Trường hợp có giá trị bằng nhau:```
4 3
5 5 5 5
2 2 2
```có nhiều đội tối ưu. DP xử lý việc này vì nó lưu trữ điểm số tốt nhất thay vì chỉ số được chọn chính xác. Ba người chơi bất kỳ đều có tổng điểm như nhau`30`, vì vậy trạng thái cuối cùng vẫn đúng.
