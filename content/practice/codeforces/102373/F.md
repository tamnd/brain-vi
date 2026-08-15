---
title: "CF 102373F - \u041e\u043d\u0438"
description: "Chúng ta có một mảng a[1..n], trong đó a[i] là số con ở vị trí i. Pennywise cũ lấy tiền tố, vị trí từ 1 đến l, trong khi Pennywise hiện đại lấy hậu tố, vị trí từ r đến n. Hai phân đoạn không được chồng lên nhau, vì vậy l < r."
date: "2026-08-14T12:41:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102373
codeforces_index: "F"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434 \u0434\u043b\u044f \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102373
solve_time_s: 123
verified: false
draft: false
---

[CF 102373F - \u041e\u043d\u0438](https://codeforces.com/problemset/problem/102373/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng`a[1..n]`, Ở đâu`a[i]`là số trẻ ở vị trí`i`. Pennywise cũ lấy tiền tố, vị trí`1`bởi vì`l`, trong khi Pennywise hiện đại có hậu tố, vị trí`r`bởi vì`n`. Hai phân đoạn không được chồng lên nhau, vì vậy`l < r`. 

Đối với một cặp được chọn`(l, r)`, hai điểm đó là`S1 = a[1] + ... + a[l]`Và`S2 = a[r] + ... + a[n]`. 

Nhiệm vụ là tìm bất kỳ cặp hợp lệ nào có thể giảm thiểu`|S1 - S2|`và in chênh lệch tối thiểu đó cùng với giá trị tương ứng`l`Và`r`. 

Ràng buộc`n <= 10^6`là tín hiệu thuật toán chính. Có thể có khoảng một triệu vị trí, vì vậy`O(n^2)`việc tìm kiếm hoàn toàn không khả thi. Thậm chí một`O(n log n)`Cách tiếp cận này là không cần thiết ở đây vì các giá trị mảng dương cho chúng ta một cấu trúc đơn điệu cho phép quét tuyến tính. Bản thân các giá trị có thể lớn bằng`10^9`, do đó tổng có thể đạt tới`10^15`. C++ cần loại số nguyên 64 bit cho các tổng này, trong khi số nguyên Python đã xử lý chúng một cách an toàn. 

Có một số trường hợp ranh giới trong đó việc triển khai có thể thất bại trong âm thầm. Với`n = 2`, cặp hợp pháp duy nhất là`l = 1, r = 2`. Đối với đầu vào```
2
8 3
```kết quả đúng là```
5 1 2
```bởi vì hai điểm số là`8`Và`3`. Việc triển khai di chuyển một con trỏ trước khi đánh giá cặp ban đầu có thể bỏ lỡ câu trả lời hợp lệ duy nhất. 

Trường hợp cạnh thứ hai xảy ra khi hai đoạn ban đầu đã có tổng bằng nhau. Vì```
4
7 1 1 7
```cặp đôi`l = 1, r = 4`cho điểm`7`Và`7`, vậy câu trả lời là```
0 1 4
```Việc triển khai bất cẩn luôn thực hiện chuyển động con trỏ trước khi kiểm tra sự khác biệt có thể bỏ lỡ mức tối ưu bằng 0. 

Ranh giới con trỏ ngược lại cũng quan trọng. Coi như```
3
1 2 3
```Bắt đầu với phần tử đầu tiên và cuối cùng sẽ cho điểm`1`Và`3`. Điểm bên trái nhỏ hơn nên con trỏ bên trái di chuyển đến vị trí`2`, cho`3`Và`3`. Kết quả đúng là```
0 2 3
```Nếu điều kiện được viết không chính xác để đẳng thức hoặc vị trí hợp lệ cuối cùng gây ra một chuyển động bổ sung, thì cặp`(2, 3)`có thể được bỏ qua. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xem xét từng cặp`(l, r)`thỏa mãn`l < r`. Đối với mỗi cặp, chúng ta có thể tính tổng tiền tố và hậu tố bằng cách sử dụng tổng tiền tố được tính toán trước, do đó mỗi ứng viên sẽ lấy`O(1)`thời gian. có`n(n-1)/2`cặp hợp lệ, đó là`O(n^2)`. Vì`n = 10^6`, đó là về`5 * 10^11`cặp, vượt xa những gì mà bất kỳ giới hạn thời gian lập trình cạnh tranh nào có thể xử lý được. 

Cấu trúc của hai tổng này cho chúng ta một cách tìm kiếm tốt hơn nhiều. Giả sử điểm bên trái hiện tại là`S1`và số điểm đúng hiện tại là`S2`. Mọi phần tử mảng đều dương. Nếu như`S1 < S2`, mở rộng tiền tố bằng cách di chuyển`l`một vị trí bên phải tăng nghiêm ngặt`S1`, trong khi vẫn giữ nguyên hậu tố bên phải. Di chuyển ranh giới bên phải trong tình huống này sẽ làm cho`S2`nhỏ hơn, nhưng chúng ta có thể suy luận trực tiếp hơn bằng cách xem hai ranh giới như một sự cạnh tranh giữa việc tăng cạnh nhỏ hơn và giảm cạnh lớn hơn. 

Quá trình hai con trỏ thuận tiện bắt đầu với cặp rộng nhất có thể,`l = 1`Và`r = n`. Nếu tổng tiền tố nhỏ hơn, hãy tiến lên`l`, bởi vì chỉ khi đó số tiền nhỏ hơn mới có thể bắt kịp. Nếu tổng hậu tố nhỏ hơn thì giảm`r`, bởi vì chỉ khi đó số tiền đó mới có thể bắt kịp. Sau mỗi trạng thái, chúng tôi kiểm tra sự khác biệt tuyệt đối và giữ lại trạng thái tốt nhất. 

Tại sao điều này là đủ? Tổng tiền tố tăng đơn điệu như`l`di chuyển sang phải và tổng hậu tố co lại một cách đơn điệu như`r`di chuyển sang phải. Cặp tối ưu phải xảy ra tại hoặc xung quanh điểm mà hai đại lượng đơn điệu này giao nhau. Quá trình hai con trỏ di chuyển trực tiếp tới giao điểm đó và đánh giá mọi trạng thái cần thiết xung quanh nó. 

Phương pháp brute-force hoạt động hiệu quả vì nó kiểm tra mọi cặp ranh giới có thể có. Nó thất bại vì có nhiều cặp như vậy theo phương trình bậc hai. Nhận xét rằng một tổng chỉ có thể di chuyển lên trên trong khi tổng kia chỉ có thể di chuyển xuống dưới cho phép chúng tôi loại bỏ toàn bộ các vùng của các cặp ứng cử viên sau mỗi lần so sánh, giảm việc tìm kiếm xuống còn`O(n)`tiểu bang. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^2)`|`O(n)`với tổng tiền tố | Quá chậm | 
| Hai con trỏ |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và khởi tạo`l = 0`Và`r = n - 1`. Đây là số 0 dựa trên nội bộ, vì vậy chúng đại diện cho các vị trí`1`Và`n`ở đầu ra cần thiết. Bộ`left_sum = a[l]`Và`right_sum = a[r]`. 

Trạng thái bắt đầu này luôn hợp lệ vì`n >= 2`, Vì thế`l < r`. 
2. Khởi tạo câu trả lời đúng nhất với chênh lệch giữa hai tổng ban đầu này. Việc lưu trữ các con trỏ tương ứng ngay lập tức là cần thiết vì cặp ban đầu có thể đã tối ưu. 
3. Trong khi`l < r`, so sánh`left_sum`Và`right_sum`. Nếu như`left_sum <= right_sum`, nâng cao`l`và thêm`a[l]`về tổng bên trái. Ngược lại thì giảm`r`và thêm`a[r]`đến số tiền phù hợp. 

Khi tổng bên trái nhỏ hơn, việc tăng tiền tố là chuyển động duy nhất làm cho cạnh đó lớn hơn. Khi tổng bên phải nhỏ hơn thì giảm`r`là chuyển động thêm một phần tử khác vào hậu tố và làm cho cạnh đó lớn hơn. 
4. Sau mỗi lần di chuyển con trỏ, hãy so sánh chênh lệch tuyệt đối mới với chênh lệch tốt nhất được tìm thấy cho đến nay. Nếu nó nhỏ hơn, hãy lưu con trỏ hiện tại. 

Trạng thái sau một chuyển động có thể là điểm đầu tiên mà tại đó hai tổng trở nên bằng nhau hoặc chéo nhau, vì vậy chỉ kiểm tra trạng thái ban đầu là không đủ. 
5. Dừng lại khi`l == r`. Tại thời điểm đó, hai phân đoạn được chọn sẽ chia sẻ một vị trí, do đó cặp này không còn giá trị. Chuyển đổi các chỉ số dựa trên số 0 được lưu trữ thành các chỉ số dựa trên một và in chênh lệch,`l + 1`, Và`r + 1`. 

### Tại sao nó hoạt động 

Tại mọi trạng thái hợp lệ,`left_sum`là tổng của tiền tố kết thúc tại`l`, Và`right_sum`là tổng của một hậu tố bắt đầu từ`r`. Bởi vì tất cả`a[i]`tích cực, chuyển động`l`bên phải tăng tổng bên trái một cách nghiêm ngặt, trong khi di chuyển`r`bên trái làm tăng tổng bên phải. 

Nếu tổng bên trái nhỏ hơn, mọi nỗ lực hữu ích nhằm giảm chênh lệch đều phải tăng vế trái cho đến khi bằng vế phải. Thuật toán thực hiện chính xác điều đó. Một cách đối xứng, nếu tổng bên phải nhỏ hơn thì nó sẽ mở rộng đoạn bên phải. Do đó, con trỏ luôn di chuyển về phía điểm mà hai tổng đơn điệu gần nhau nhất. Thuật toán kiểm tra sự khác biệt ở mọi trạng thái dọc theo đường dẫn này, bao gồm trạng thái trước khi giao nhau và trạng thái sau khi giao nhau. Do đó, mức tối thiểu trong số các trạng thái đó là mức tối thiểu toàn cầu trên tất cả các cặp ranh giới hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    l = 0
    r = n - 1

    left_sum = a[l]
    right_sum = a[r]

    best_diff = abs(left_sum - right_sum)
    best_l = l
    best_r = r

    while l < r:
        if left_sum <= right_sum:
            l += 1
            left_sum += a[l]
        else:
            r -= 1
            right_sum += a[r]

        if l >= r:
            break

        diff = abs(left_sum - right_sum)
        if diff < best_diff:
            best_diff = diff
            best_l = l
            best_r = r

    print(best_diff, best_l + 1, best_r + 1)

if __name__ == "__main__":
    solve()
```Mảng được lưu trữ vì con trỏ bên phải có thể di chuyển sang bên trái, do đó thuật toán cần truy cập ngẫu nhiên vào phần tử mới được đưa vào. Số tiền ban đầu sử dụng`a[0]`Và`a[n - 1]`, đại diện cho tiền tố và hậu tố nhỏ nhất có thể. 

Việc so sánh sử dụng`<=`cho phía bên trái. Nếu tổng bằng nhau thì về mặt kỹ thuật, một trong hai con trỏ có thể được di chuyển, nhưng việc chọn con trỏ bên trái là đủ vì trạng thái hiện tại đã được ghi lại là chênh lệch tốt nhất có thể bằng 0. 

Sau khi di chuyển con trỏ, đầu tiên mã sẽ kiểm tra`l >= r`. Điều này ngăn việc đánh giá một cặp không hợp lệ trong đó tiền tố và hậu tố trùng nhau. Cặp ban đầu được đánh giá riêng trước khi vào vòng lặp, vì vậy cặp hợp pháp duy nhất cho`n = 2`được xử lý chính xác. 

Các số nguyên chính xác tùy ý của Python lưu trữ tổng một cách an toàn lên đến`10^15`. Thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi chuyển động của con trỏ và mỗi chuyển động của con trỏ nhiều nhất là`n`các vị trí. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho```
5
5 1 1 1 1
```tiền tố ban đầu chứa`5`, trong khi hậu tố chứa phần cuối cùng`1`. Vì tổng bên trái lớn hơn nên con trỏ bên phải sẽ di chuyển sang trái và dần dần phóng to hậu tố cho đến khi tìm thấy sự khác biệt tốt nhất. 

|`l`|`r`|`left_sum`|`right_sum`|`diff`| Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | 1 | 4 |`4 1 5`| 
| 1 | 4 | 5 | 2 | 3 |`3 1 4`| 
| 1 | 3 | 5 | 3 | 2 |`2 1 3`| 
| 1 | 2 | 5 | 4 | 1 |`1 1 2`| 

Chuyển động tiếp theo sẽ làm`l = r`, do đó thuật toán dừng lại. Trạng thái hợp lệ tốt nhất là`l = 1, r = 2`, với sự khác biệt`1`, phù hợp với mẫu 

### Mẫu 2 

cho```
4
1 2 3 4
```phía bên trái bắt đầu nhỏ hơn nên con trỏ bên trái tiến lên. Nó đạt đến sự bình đẳng ở vị trí thứ hai. 

|`l`|`r`|`left_sum`|`right_sum`|`diff`| Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 4 | 3 |`3 1 4`| 
| 2 | 4 | 3 | 4 | 1 |`1 2 4`| 
| 3 | 4 | 6 | 4 | 2 |`1 2 4`| 

Trạng thái tốt nhất là`(2, 4)`, điểm số ở đâu`3`Và`4`. Trạng thái sau tệ hơn nên câu trả lời được lưu trữ vẫn còn`1 2 4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi con trỏ chỉ di chuyển về phía con trỏ kia nên có nhiều nhất`n - 1`các phong trào. | 
| Không gian |`O(n)`| Mảng đầu vào được lưu trữ để một trong hai con trỏ có thể truy cập phần tử tiếp theo. | 

Với`n`lên đến`10^6`, quét tuyến tính là thích hợp. Thuật toán chỉ thực hiện một vài thao tác số nguyên cho mỗi phần tử mảng, trong khi giải pháp thay thế brute-force sẽ yêu cầu khoảng`5 * 10^11`cặp ứng cử viên ở kích thước tối đa. Mảng được lưu trữ là tuyến tính trong`n`và các tổng yêu cầu giá trị lên tới`10^15`, mà Python xử lý nguyên bản. 

## Trường hợp thử nghiệm 

Các thử nghiệm tùy chỉnh bên dưới sử dụng kết quả đầu ra chính xác như mong đợi trong đó cặp tối ưu được xác định duy nhất hoặc trạng thái ban đầu của thuật toán rõ ràng là tối ưu. Trường hợp kích thước tối đa sử dụng một triệu giá trị bằng nhau, do đó cặp ban đầu đã tối ưu và thử nghiệm cũng thực hiện thang đo đầu vào được yêu cầu.```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        input = sys.stdin.readline

        n = int(input())
        a = list(map(int, input().split()))

        l = 0
        r = n - 1

        left_sum = a[l]
        right_sum = a[r]

        best_diff = abs(left_sum - right_sum)
        best_l = l
        best_r = r

        while l < r:
            if left_sum <= right_sum:
                l += 1
                left_sum += a[l]
            else:
                r -= 1
                right_sum += a[r]

            if l >= r:
                break

            diff = abs(left_sum - right_sum)
            if diff < best_diff:
                best_diff = diff
                best_l = l
                best_r = r

        print(best_diff, best_l + 1, best_r + 1)
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert solution("""5
5 1 1 1 1
""") == "1 1 2", "sample 1"

assert solution("""4
1 2 3 4
""") == "1 2 4", "sample 2"

assert solution("""2
8 3
""") == "5 1 2", "minimum-size input"

assert solution("""4
7 1 1 7
""") == "0 1 4", "equal initial sums"

assert solution("""3
1 2 3
""") == "0 2 3", "off-by-one boundary"

n = 1_000_000
maximum_input = str(n) + "\n" + ("1 " * (n - 1)) + "1\n"
assert solution(maximum_input) == "0 1 1000000", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 8 3`|`5 1 2`| Mảng pháp lý nhỏ nhất và cặp duy nhất có thể có | 
|`4 / 7 1 1 7`|`0 1 4`| Trạng thái ban đầu có thể đã tối ưu | 
|`3 / 1 2 3`|`0 2 3`| Điểm tối ưu có thể xuất hiện ngay sau khi di chuyển ranh giới bên trái | 
|`1000000 / all ones`|`0 1 1000000`| Kích thước đầu vào tối đa, xử lý tuyến tính và xử lý đầu vào lớn | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
2
8 3
```thuật toán bắt đầu với`l = 0`Và`r = 1`, tính tổng`8`Và`3`và sự khác biệt`5`. Điều kiện vòng lặp là đúng, nhưng tổng bên trái lớn hơn sẽ gây ra`r`di chuyển từ`1`ĐẾN`0`. Từ`l >= r`, trạng thái mới bị từ chối và cặp được lưu trữ trước đó vẫn còn`(1, 2)`. Đầu ra là`5 1 2`. Đây là lý do tại sao việc kiểm tra ranh giới phải diễn ra trước khi đánh giá trạng thái được di chuyển. 

Với số tiền ban đầu bằng nhau,```
4
7 1 1 7
```trạng thái ban đầu có`left_sum = 7`Và`right_sum = 7`, Vì thế`best_diff`trở thành số 0 ngay lập tức. Vòng lặp sau này có thể di chuyển một con trỏ vì việc triển khai sử dụng`<=`, nhưng không có trạng thái nào sau này có thể cải thiện sự khác biệt bằng 0. Câu trả lời được lưu trữ vẫn còn`0 1 4`. 

Đối với trường hợp vượt biên```
3
1 2 3
```trạng thái ban đầu có tổng`1`Và`3`. Vế bên trái nhỏ hơn nên`l`trở thành`2`trong một chỉ mục dựa trên và`left_sum`trở thành`3`. Các tổng hiện bằng nhau, cho chênh lệch bằng 0. Chuyển động tiếp theo sẽ làm cho ranh giới chồng lên nhau nên không được đánh giá. Kết quả là`0 2 3`. 

Trường hợp có kích thước tối đa có thể được biểu thị bằng một triệu đơn vị. Cả hai số tiền ban đầu đều đã có`1`, do đó thuật toán ghi lại chênh lệch bằng 0 mà không cần duyệt toàn bộ mảng. Đầu ra là`0 1 1000000`. Trường hợp này xác nhận rằng việc triển khai xử lý số lượng lớn nhất được phép`n`và việc lưu trữ mảng đó không làm thay đổi độ phức tạp tiệm cận. 

Chi tiết triển khai cuối cùng là tất cả các giá trị mảng đều dương. Đối số hai con trỏ đơn điệu phụ thuộc vào thuộc tính này. Nếu cho phép các giá trị 0 hoặc âm, việc di chuyển một ranh giới sẽ không còn đảm bảo rằng tổng tương ứng thay đổi theo hướng có thể dự đoán được và bằng chứng cụ thể này sẽ không còn áp dụng được nữa.
