---
title: "CF 102330B - \u041f\u043e\u0435\u0437\u0434\u043a\u0430 \u043d\u0430 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0443"
description: "Có n toa tàu. Ở toa i, ghế a[i] vẫn còn trống sau khi đội hóa học đã mua vé. Đội tin học có đúng k người tham gia và muốn mua vé để mọi toa xe họ đi đều đầy ắp."
date: "2026-08-13T03:56:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102330
codeforces_index: "B"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2019.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 102330
solve_time_s: 54
verified: true
draft: false
---

[CF 102330B - \u041f\u043e\u0435\u0437\u0434\u043a\u0430 \u043d\u0430 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0443](https://codeforces.com/problemset/problem/102330/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có`n`xe lửa. Trong ô tô`i`,`a[i]`Chỗ ngồi vẫn còn trống sau khi đội hóa học đã mua vé. Đội ngũ tin học đã chính xác`k`người tham gia và muốn mua vé để mọi toa xe họ đi đều đầy ắp. Vì không ai ngoại trừ những người tham gia môn hóa học được phép đi chung một chiếc xe như vậy nên mọi chỗ ngồi trống còn lại trên chiếc xe đã chọn phải có một người tham gia tin học ngồi. 

Do đó, nếu đội sử dụng một bộ ô tô thì tổng số ghế trống trên các ô tô đó phải chính xác`k`. Những chiếc xe được chọn phải tạo thành phân khúc liền kề nhỏ nhất có thể xét về số xe đầu tiên và số xe cuối cùng. Một chiếc ô tô với`a[i] = 0`có thể xảy ra giữa hai ô tô liên quan vì nó đã đầy chỗ và không cần vé mới. Chúng tôi chỉ phải xuất ra các điểm cuối của phân khúc tối ưu. 

Ví dụ, với`a = [1, 2, 3, 4]`Và`k = 5`, xe 2 và 3 chứa đúng 5 chỗ ngồi trống nên đáp án là`2 3`. Với`a = [1, 0, 2]`Và`k = 3`, cả đoạn từ xe 1 đến xe 3 có đúng 3 ghế trống, xe giữa đã kín chỗ rồi. 

Số lượng ô tô nhiều nhất là`10^5`. Một thuật toán bậc hai sẽ xem xét khoảng`n(n+1)/2`, gần với`5 * 10^9`khoảng cách ở kích thước tối đa. Điều đó vượt xa những gì giới hạn lập trình cạnh tranh một giây có thể xử lý. Các giá trị của`a[i]`Và`k`có thể đạt được`10^9`và tổng của chúng cũng có thể đạt tới`10^9`, do đó, về mặt khái niệm, việc triển khai nên sử dụng số học số nguyên để xử lý các tổng có kích thước này một cách an toàn. Số nguyên Python không có vấn đề tràn ở đây. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn không thành công. Đầu tiên, câu trả lời có thể bao gồm một chiếc ô tô. Đối với đầu vào```
1 5
5
```đầu ra đúng là`1 1`. Việc triển khai chỉ kiểm tra các đoạn có độ dài ít nhất là hai sẽ bỏ lỡ nó. 

Thứ hai, những chiếc xe đầy đủ có thể xảy ra bên trong câu trả lời. Vì```
3 3
1 0 2
```đầu ra đúng là`1 3`. Số 0 ở giữa không đóng góp gì nhưng điểm cuối vẫn phải được báo cáo là`1`Và`3`. 

Thứ ba, cần phải có một số tiền chính xác. Vì```
2 5
2 2
```đầu ra đúng là`-1`. Toàn bộ chuyến tàu chỉ có bốn chỗ ngồi miễn phí nên việc mua năm vé không thể lấp đầy mọi toa đã chọn. 

Cuối cùng, những chiếc ô tô có giá trị bằng 0 có thể tạo ra một số phân đoạn với cùng số lượng ô tô khác 0 nhưng khoảng cách điểm cuối khác nhau. Vì```
5 3
0 1 0 2 0
```đầu ra đúng là`2 4`. Phân đoạn chứa ba chỗ trống, trong khi việc mở rộng nó để bao gồm điểm cuối có giá trị bằng 0 sẽ khiến khoảng thời gian dài hơn và do đó tệ hơn. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể liệt kê mọi khoảng thời gian có thể`[l, r]`và tính tổng của nó. Sử dụng tổng tiền tố, mỗi tổng khoảng có thể thu được trong thời gian không đổi, do đó thuật toán mất`O(n^2)`thời gian và`O(n)`ký ức. Nó đúng vì mọi lựa chọn có thể có của xe đầu tiên và xe cuối cùng đều được kiểm tra rõ ràng, và trong số những xe có tổng bằng`k`, chúng ta có thể giữ cái ngắn nhất. 

Vấn đề là số lượng khoảng thời gian. Vì`n = 100000`, có`n(n + 1) / 2 = 5,000,050,000`khoảng thời gian khác nhau. Mặc dù mỗi lần tra cứu tổng là thời gian không đổi, nhưng vài tỷ lần lặp lại là quá chậm. 

Quan sát quan trọng là mọi`a[i]`là không âm. Điều này làm cho tổng khoảng có tính chất đơn điệu. Nếu chúng ta kéo dài một khoảng sang bên phải thì tổng của nó chỉ có thể giữ nguyên hoặc tăng lên. Nếu chúng ta di chuyển điểm cuối bên trái của nó sang bên phải thì tổng của nó chỉ có thể giữ nguyên hoặc giảm đi. 

Đó chính xác là cấu trúc cần thiết cho cửa sổ trượt hai con trỏ. Chúng tôi duy trì một cửa sổ`[l, r]`và tổng của nó. Chúng tôi mở rộng`r`cho đến khi tổng đạt hoặc vượt qua`k`. Nếu tổng đó chính xác`k`, cửa sổ hiện tại là một ứng cử viên. Sau đó, chúng tôi xóa các phần tử ở bên trái trong khi có thể, vì việc xóa giá trị dương sẽ làm cho cửa sổ ngắn hơn và có thể hiển thị câu trả lời tốt hơn. Những chiếc xe có giá trị bằng 0 cần được chăm sóc đặc biệt vì việc loại bỏ chúng không làm thay đổi tổng và có thể rút ngắn câu trả lời ngay cả khi tổng vẫn bằng`k`. 

Lực lượng vũ phu hoạt động vì mỗi khoảng thời gian trả lời độc lập câu hỏi "phân đoạn này có chứa chính xác không`k`ghế trống?" Nhận xét rằng các giá trị không âm làm cho tổng trở nên đơn điệu cho phép chúng ta sử dụng lại thông tin từ một khoảng trong khi chuyển sang khoảng tiếp theo, giảm công việc về thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^2)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`k`, và mảng`a`. Chúng ta cần tìm một khoảng có tổng chính xác`k`, với độ dài tối thiểu 
2. Khởi tạo con trỏ trái`l = 0`, tổng hiện tại`s = 0`và các biến lưu trữ khoảng thời gian tốt nhất được tìm thấy cho đến nay. 
3. Di chuyển con trỏ sang phải`r`từ trái sang phải. Thêm vào`a[r]`ĐẾN`s`, vì cửa sổ hiện tại bao gồm chiếc xe này. 
4. Trong khi`s > k`, di dời`a[l]`từ`s`và tăng dần`l`. Vì tất cả các giá trị đều không âm nên cách duy nhất để giảm tổng quá mức là di chuyển ranh giới bên trái về phía trước. 
5. Sau khi giảm cửa sổ, nếu`s == k`, khoảng thời gian hiện tại là hợp lệ. So sánh độ dài của nó với khoảng thời gian tốt nhất được tìm thấy cho đến nay và giữ nguyên nếu nó ngắn hơn. 
6. Khi nào`s == k`, việc loại bỏ nhiều lần các số 0 ở đầu rất hữu ích vì chúng không làm thay đổi tổng. Trong quá trình triển khai, điều này được xử lý một cách tự nhiên bằng logic chuyển động biên trái tương tự trước khi so sánh độ dài ứng cử viên hoặc bằng cách thu nhỏ các số 0 một cách rõ ràng sau khi tìm thấy tổng chính xác. Điều này quan trọng bởi vì`[0, 1, 2]`Và`[1, 2]`có cùng tổng, nhưng số sau là khoảng tốt hơn. 
7. Sau khi xử lý mọi điểm cuối bên phải, nếu không có khoảng có tổng`k`đã được tìm thấy, in`-1`. Nếu không, hãy in điểm cuối của khoảng thời gian tốt nhất bằng cách sử dụng đánh số ô tô dựa trên một. 

### Tại sao nó hoạt động 

Tại mỗi thời điểm, cửa sổ chứa một dãy ô tô liền kề nhau và`s`chính xác là tổng số ghế trống của họ. Bởi vì tất cả`a[i]`không âm, việc mở rộng điểm cuối bên phải không bao giờ giảm`s`, trong khi việc nâng điểm cuối bên trái không bao giờ làm tăng nó. Vì vậy, khi một cửa sổ có tổng lớn hơn`k`, việc giữ điểm cuối bên trái hiện tại không thể tạo ra điểm cuối bên phải lớn hơn hợp lệ, do đó điểm cuối bên trái có thể tiến lên một cách an toàn. Bất cứ khi nào cửa sổ có tổng chính xác`k`, đó là một giải pháp hợp lệ và việc loại bỏ các số 0 đứng đầu sẽ mang lại khoảng thời gian ngắn nhất với cùng điểm cuối bên phải đó. Vì mọi điểm cuối bên phải đều được xử lý và mọi con trỏ chỉ di chuyển về phía trước nên thuật toán sẽ xem xét khoảng thời gian hợp lệ ngắn nhất có thể được liên kết với mọi vị trí và do đó tìm ra khoảng thời gian ngắn nhất trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    left = 0
    current_sum = 0

    best_left = -1
    best_right = -1
    best_len = n + 1

    for right in range(n):
        current_sum += a[right]

        while left <= right and current_sum > k:
            current_sum -= a[left]
            left += 1

        if current_sum == k:
            while left <= right and a[left] == 0:
                left += 1

            length = right - left + 1

            if length < best_len:
                best_len = length
                best_left = left
                best_right = right

    if best_left == -1:
        print(-1)
    else:
        print(best_left + 1, best_right + 1)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc một lần và được lưu trữ trong`a`, sau đó`left`Và`right`đại diện cho cửa sổ trượt hiện tại.`current_sum`được cập nhật bất cứ khi nào một trong hai ranh giới di chuyển, do đó không cần mảng tổng tiền tố. 

các`while current_sum > k`vòng lặp an toàn vì tất cả các giá trị đều không âm. Khi tổng quá lớn, việc di chuyển ranh giới bên trái là thao tác duy nhất có thể làm giảm nó. Mỗi phần tử bị xóa khỏi cửa sổ tối đa một lần, vì vậy vòng lặp này chỉ đóng góp`O(n)`tổng công việc chứ không phải là`O(n)`làm việc cho mọi điểm cuối phù hợp. 

Vòng lặp thứ hai loại bỏ những ô tô có giá trị bằng 0 ở bên trái sau khi tìm thấy số tiền chính xác. Đây là một điều kiện biên tinh tế. Giả sử cửa sổ hiện tại là`[0, 1, 2]`Và`k = 3`. Tổng của nó đã đúng rồi, nhưng`[1, 2]`là một khoảng thời gian hợp lệ ngắn hơn nghiêm ngặt. Bởi vì việc loại bỏ số 0 không thay đổi`current_sum`, chúng ta có thể loại bỏ những chiếc xe như vậy một cách an toàn. 

Câu trả lời được lưu trữ nội bộ bằng cách sử dụng các chỉ mục dựa trên 0 và chỉ được chuyển đổi thành các chỉ mục dựa trên một khi in. Không có loại số nguyên đặc biệt nào được yêu cầu trong Python vì số nguyên Python tự động tăng lên khi cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
7 5
1 2 3 4 2 1 2
```Trạng thái liên quan khi di chuyển điểm cuối bên phải là: 

|`right`| Đã thêm`a[right]`|`left`|`current_sum`| Ứng viên | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | không | 
| 1 | 2 | 0 | 3 | không | 
| 2 | 3 | 0 | 6 | quá lớn | 
| 2 | LOẠI BỎ`1`| 1 | 5 |`[1, 2]`| 
| 3 | 4 | 1 | 9 | quá lớn | 
| 3 | LOẠI BỎ`2`| 2 | 7 | quá lớn | 
| 3 | LOẠI BỎ`3`| 3 | 4 | không | 
| 4 | 2 | 3 | 6 | quá lớn | 
| 4 | LOẠI BỎ`4`| 4 | 2 | không | 
| 5 | 1 | 4 | 3 | không | 
| 6 | 2 | 4 | 5 |`[4, 6]`| 

Cửa sổ hợp lệ đầu tiên là ô tô từ 2 đến 3, với`2 + 3 = 5`. Sau này, ô tô từ 5 đến 7 cũng có tổng bằng 5, nhưng khoảng thời gian đó dài hơn. Vì thế câu trả lời là`2 3`. 

Dấu vết này thể hiện tính bất biến chính của cửa sổ trượt: khi tổng trở nên quá lớn, tiến`left`cuối cùng khôi phục lại một số tiền nhiều nhất`k`và không thể bỏ qua khoảng thời gian hợp lệ ngắn hơn. 

### Mẫu 2 

Đầu vào là:```
5 3
1 0 2 10 10
```Dấu vết là: 

|`right`| Đã thêm`a[right]`|`left`|`current_sum`| Ứng viên | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | không | 
| 1 | 0 | 0 | 1 | không | 
| 2 | 2 | 0 | 3 |`[0, 2]`| 
| 3 | 10 | 0 | 13 | quá lớn | 
| 3 | LOẠI BỎ`1`| 1 | 12 | quá lớn | 
| 3 | LOẠI BỎ`0`| 2 | 12 | quá lớn | 
| 3 | LOẠI BỎ`2`| 3 | 10 | quá lớn | 
| 3 | LOẠI BỎ`10`| 4 | 0 | không | 
| 4 | 10 | 4 | 10 | quá lớn | 
| 4 | LOẠI BỎ`10`| 5 | 0 | không | 

Khoảng hợp lệ đầu tiên là ô tô từ 1 đến 3. Ô tô thứ hai có giá trị 0 đã đầy, do đó, ba ghế trống chính xác là một ghế ở ô tô 1 và hai ghế ở ô tô 3. Đầu ra là`1 3`. 

Ví dụ này chứng minh tại sao ô tô có giá trị bằng 0 phải được phép ở trong khoảng thời gian đó. Họ không tốn vé nhưng cũng không ngăn cản đội tin học chiếm giữ xe hai bên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Con trỏ bên phải di chuyển từ trái sang phải một lần và con trỏ bên trái cũng chỉ di chuyển về phía trước. | 
| Không gian |`O(n)`| Mảng của`n`số chỗ trống được lưu trữ trong bộ nhớ. | 

Với`n <= 100000`, quét tuyến tính chỉ thực hiện một số thao tác nhỏ trên mỗi ô tô. Việc sử dụng bộ nhớ cũng thoải mái trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    left = 0
    current_sum = 0

    best_left = -1
    best_right = -1
    best_len = n + 1

    for right in range(n):
        current_sum += a[right]

        while left <= right and current_sum > k:
            current_sum -= a[left]
            left += 1

        if current_sum == k:
            while left <= right and a[left] == 0:
                left += 1

            length = right - left + 1

            if length < best_len:
                best_len = length
                best_left = left
                best_right = right

    if best_left == -1:
        print(-1)
    else:
        print(best_left + 1, best_right + 1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided samples
assert run("""7 5
1 2 3 4 2 1 2
""") == "2 3", "sample 1"

assert run("""5 3
1 0 2 10 10
""") == "1 3", "sample 2"

# Minimum-size input
assert run("""1 5
5
""") == "1 1", "single car"

# Impossible case
assert run("""2 5
2 2
""") == "-1", "total free seats are insufficient"

# All equal values, answer spans the whole array
assert run("""5 3
1 1 1 1 1
""") == "1 3", "all equal values"

# Zeroes around a valid segment
assert run("""5 3
0 1 0 2 0
""") == "2 4", "zero boundaries"

# Exact boundary after shrinking from the left
assert run("""4 6
2 4 1 5
""") == "1 2", "exact prefix"

# Maximum-size style case
n = 100000
assert run(f"{n} {n}\n" + " ".join(["1"] * n) + "\n") == "1 100000", "large input"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 / 5`|`1 1`| Kích thước tối thiểu và câu trả lời cho một toa xe | 
|`2 5 / 2 2`|`-1`| Phát hiện chính xác rằng không tồn tại số tiền chính xác | 
|`5 3 / 1 1 1 1 1`|`1 3`| Các giá trị bằng nhau và chuyển động của cửa sổ trượt thông thường | 
|`5 3 / 0 1 0 2 0`|`2 4`| Ôtô không có giá trị ở cả hai ranh giới | 
|`4 6 / 2 4 1 5`|`1 2`| Xử lý tiền tố và ranh giới chính xác | 
|`100000`những cái có`k = 100000`|`1 100000`| Tối đa`n`và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

###Một chiếc ô tô 

cho```
1 5
5
```con trỏ bên phải ghé thăm chiếc ô tô duy nhất, tạo ra`current_sum = 5`. Vì điều này bằng`k`, khoảng`[0, 0]`trở thành câu trả lời tốt nhất. Sau khi chuyển đổi sang lập chỉ mục một lần, chương trình sẽ in`1 1`. 

Sai lầm phổ biến ở đây là khởi đầu câu trả lời đúng nhất như thể phải chọn ít nhất hai ô tô. Không có gì trong bài toán yêu cầu điều đó, vì vậy một ô tô là khoảng thời gian hợp lệ và thường là tối ưu. 

### Một chiếc xe đã đầy bên trong đáp án 

cho```
3 3
1 0 2
```cửa sổ phát triển như`[1]`, sau đó`[1, 0]`, sau đó`[1, 0, 2]`. Tổng của nó trở thành đúng ba. Xe giá trị 0 đã đầy rồi nhưng nó nằm giữa hai xe chở học sinh tham gia tin học nên điểm cuối là`1`Và`3`. 

Số 0 không thể đơn giản được coi là một trở ngại. Làm như vậy sẽ kết luận không chính xác rằng không có khoảng liền kề nào có tổng yêu cầu. 

### Không thể có số tiền chính xác 

cho```
2 5
2 2
```tổng số ghế trống trên toàn đoàn tàu là bốn. Vì mọi`a[i]`không âm, không có phân mảng nào có thể có tổng lớn hơn tổng, do đó không thể đạt tới 5. Con trỏ bên phải cuối cùng xử lý toàn bộ mảng mà không tạo ra`current_sum == 5`, và chương trình in`-1`. 

Việc thực hiện bất cẩn ít nhất là tìm kiếm số tiền nhỏ nhất`k`có thể chấp nhận không chính xác toàn bộ mảng. Yêu cầu là sự bình đẳng chứ không phải`sum >= k`. 

### Ranh giới có giá trị bằng 0 

cho```
5 3
0 1 0 2 0
```cửa sổ chính xác đầu tiên là`[0, 1, 0, 2]`, tổng của nó là ba. Sau đó, thuật toán sẽ loại bỏ số 0 đứng đầu mà không thay đổi tổng, để lại`[1, 0, 2]`. Câu trả lời kết quả là ô tô`2`bởi vì`4`. 

Số 0 ở cuối không được bao gồm vì con trỏ bên phải chưa tiến tới số 0 đó khi đánh giá ứng viên. Việc bao gồm ranh giới bằng 0 sẽ tạo ra khoảng thời gian dài hơn mà không cần thêm bất kỳ chỗ ngồi hữu ích nào.
