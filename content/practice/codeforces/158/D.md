---
title: "CF 158D - Tượng Băng"
description: "Chúng ta có n tác phẩm điêu khắc bằng băng được đặt đều trên một vòng tròn. Mỗi tác phẩm điêu khắc có một giá trị, có thể tích cực hoặc tiêu cực. Chúng tôi có thể loại bỏ một số tác phẩm điêu khắc, nhưng những tác phẩm điêu khắc còn lại vẫn phải tạo thành một đa giác đều."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "brute-force", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 158
codeforces_index: "D"
codeforces_contest_name: "VK Cup 2012 Qualification Round 1"
rating: 1300
weight: 158
solve_time_s: 98
verified: true
draft: false
---

[CF 158D - Tác phẩm điêu khắc trên băng](https://codeforces.com/problemset/problem/158/D) 

**Đánh giá:** 1300 
**Tags:** *đặc biệt, vũ phu, lý thuyết số 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`tác phẩm điêu khắc băng được đặt đều trên một vòng tròn. Mỗi tác phẩm điêu khắc có một giá trị, có thể tích cực hoặc tiêu cực. Chúng tôi có thể loại bỏ một số tác phẩm điêu khắc, nhưng những tác phẩm điêu khắc còn lại vẫn phải tạo thành một đa giác đều. 

Vì các tác phẩm điêu khắc không thể di chuyển nên các đỉnh còn lại phải cách đều nhau dọc theo đường tròn ban đầu. Điều đó có nghĩa là nếu chúng ta giữ`k`thì tác phẩm điêu khắc`k`phải chia`n`, và các tác phẩm điêu khắc được giữ lại có được bằng cách nhảy liên tục theo một kích thước bước cố định. 

Ví dụ, nếu`n = 12`và chúng tôi giữ 4 tác phẩm điêu khắc, thì khoảng cách giữa các tác phẩm điêu khắc được giữ liên tiếp phải là`12 / 4 = 3`. Bắt đầu từ bất kỳ vị trí nào và cộng liên tục 3 modulo 12 sẽ tạo ra một đa giác hợp lệ. 

Nhiệm vụ là tối đa hóa tổng giá trị tác phẩm điêu khắc được lưu giữ, đồng thời giữ lại ít nhất 3 tác phẩm điêu khắc. 

Các ràng buộc đủ nhỏ để cho phép thử nhiều ước của`n`, nhưng quá lớn đối với bất kỳ số bậc hai nào trong`n`. Từ`n ≤ 20000`, MỘT`O(n^2)`giải pháp sẽ thực hiện khoảng 400 triệu thao tác trong trường hợp xấu nhất, quá chậm trong Python. Một giải pháp xung quanh`O(n * d(n))`, Ở đâu`d(n)`là số ước số, dễ dàng phù hợp. 

Có một số sai lầm dễ dàng trong vấn đề này. 

Một lỗi phổ biến là chỉ kiểm tra các phân đoạn liền kề. Các tác phẩm điêu khắc còn lại không bắt buộc phải nằm liền kề trên vòng tròn. 

Coi như:```
8
1 2 -3 4 -5 5 2 3
```Câu trả lời tốt nhất có được bằng cách giữ mỗi tác phẩm điêu khắc thứ hai:```
2 + 4 + 5 + 3 = 14
```Cách tiếp cận mảng con liền kề sẽ hoàn toàn bỏ lỡ cấu trúc này. 

Một sai lầm nữa là quên rằng đa giác còn lại phải có ít nhất 3 đỉnh. 

Ví dụ:```
6
100 -1 100 -1 100 -1
```Chỉ giữ lại ba`100`s là hợp lệ và cho`300`. Giữ hai đỉnh sẽ cho`200`, nhưng 2-giác không được phép. 

Một lỗi tinh vi hơn xuất hiện khi xử lý bước tròn. Giả định:```
5
1 2 3 4 5
```Vì 5 là số nguyên tố nên đa giác hợp lệ duy nhất sử dụng tất cả các tác phẩm điêu khắc. Mọi nỗ lực sử dụng kích thước bước 2 hoặc 3 vẫn truy cập vào mọi đỉnh vì các bước đó nguyên tố cùng nhau với 5. Câu trả lời đúng là`15`. 

Việc triển khai bất cẩn đôi khi tính các chu kỳ như vậy nhiều lần hoặc giả định không chính xác rằng mỗi bước sẽ tạo ra một đa giác nhỏ hơn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng ta có thể thử từng tập hợp con của tác phẩm điêu khắc và kiểm tra xem các vị trí đã chọn có tạo thành đa giác đều hay không. Một tập hợp con hợp lệ nếu khoảng cách giữa các đỉnh được chọn liên tiếp xung quanh đường tròn bằng nhau. 

Điều này có hiệu quả về mặt khái niệm vì định nghĩa của một đa giác đều rất cứng nhắc. Thật không may, có`2^n`tập hợp con, điều này trở nên không thể ngay cả đối với`n = 30`, huống hồ là`20000`. 

Một lực lượng vũ phu có cấu trúc hơn quan sát thấy rằng mọi đa giác hợp lệ đều tương ứng với việc liên tục thực hiện các bước nhảy có kích thước cố định xung quanh vòng tròn. 

Giả sử kích thước bước nhảy là`d`. Bắt đầu từ vị trí`s`, chúng tôi ghé thăm:```
s, s+d, s+2d, ...
```modulo`n`. 

Điều này tạo thành một chu kỳ. Số đỉnh riêng biệt được thăm bằng:```
n / gcd(n, d)
```Để đa giác hợp lệ, chúng ta cần ít nhất 3 đỉnh. 

Bây giờ không gian tìm kiếm trở nên có thể quản lý được. Đối với mỗi kích thước bước có thể, chúng ta có thể mô phỏng mọi vị trí bắt đầu và tính tổng dọc theo chu kỳ của nó. 

Quan sát quan trọng là các chu trình được tạo ra bởi cùng một kích thước bước sẽ phân chia vòng tròn thành các nhóm rời rạc. Nếu như`g = gcd(n, d)`, thì có chính xác`g`chu kỳ khác nhau, mỗi chu kỳ có độ dài`n / g`. 

Thay vì suy nghĩ theo các đa giác tùy ý, chúng ta chỉ cần xét các ước của`n`. 

Cho phép`k`là số đỉnh được giữ Sau đó`k`phải chia`n`. Kích thước bước trở thành:```
step = n / k
```Với mỗi ước số`k ≥ 3`, chúng ta tính tổng của tất cả các cấp số cộng theo modulo`n`với kích thước bước đó và giữ mức tối đa. 

Số ước của một số lên đến`20000`nhỏ nên tổng công gần như tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n) | O(n) | Quá chậm | 
| Đếm chu kỳ theo ước số | O(n × d(n)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng giá trị điêu khắc. 
2. Khởi tạo câu trả lời dưới dạng tổng của tất cả các tác phẩm điêu khắc. 

Điều này đã tương ứng với việc giữ toàn bộ đa giác, điều này luôn được cho phép. 

1. Liệt kê tất cả các ước số`k`của`n`như vậy`k ≥ 3`. 

Đây,`k`đại diện cho số đỉnh trong đa giác còn lại. 

1. Đối với mỗi hợp lệ`k`, tính:```
step = n / k
```Nếu chúng ta liên tục di chuyển qua`step`, chúng tôi ghé thăm chính xác`k`đỉnh trước khi quay lại điểm xuất phát. 

1. Đối với mọi vị trí xuất phát từ`0`ĐẾN`step - 1`, tính tổng của chu trình được hình thành bằng cách thêm liên tục`step`. 

Mỗi vị trí bắt đầu tạo ra một cấu hình đa giác riêng biệt cho ước số này. 

1. Cập nhật mức tối đa toàn cầu bằng tổng chu kỳ. 
2. In giá trị tốt nhất tìm được. 

### Tại sao nó hoạt động 

Một đa giác còn lại hợp lệ phải bao gồm các đỉnh cách đều nhau trên đường tròn ban đầu. Nếu đa giác có`k`đỉnh thì khoảng cách giữa các đỉnh được giữ liên tiếp phải chính xác`n / k`. 

Bắt đầu từ bất kỳ đỉnh nào và liên tục thêm modulo khoảng cách này`n`tạo ra chính xác các đỉnh của đa giác đó. 

Ngược lại, mọi chu trình được tạo theo cách này sẽ tạo thành một đa giác đều hợp lệ vì tất cả các khoảng trống đều bằng nhau. 

Thuật toán liệt kê mọi ước số có thể`k`và mọi vị trí bắt đầu có thể có cho khoảng cách đó, vì vậy mọi đa giác hợp lệ đều được kiểm tra chính xác một lần. Vì chúng tôi tính toán tổng chính xác cho từng ứng viên và giữ mức tối đa nên câu trả lời cuối cùng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    ans = sum(a)

    for k in range(3, n + 1):
        if n % k != 0:
            continue

        step = n // k

        for start in range(step):
            cur = 0
            pos = start

            for _ in range(k):
                cur += a[pos]
                pos += step

            ans = max(ans, cur)

    print(ans)

solve()
```Lời giải trực tiếp tuân theo cấu trúc toán học của đa giác hợp lệ. 

Vòng lặp bên ngoài lặp lại trên tất cả các kích thước đa giác có thể`k`. Chỉ có các ước của`n`là hợp lệ vì các đỉnh cách đều nhau phải bao quanh đường tròn một cách hoàn hảo. 

Đối với một cố định`k`, kích thước bước nhảy là`step = n // k`. Bắt đầu từ bất kỳ chỉ mục nào và liên tục thêm`step`truy cập chính xác các đỉnh của một đa giác ứng cử viên. 

Phạm vi`0 .. step-1`là đủ cho các vị trí bắt đầu vì các lần khởi động lớn hơn sẽ lặp lại các chu kỳ tương tự. Đây là một sự tối ưu hóa tinh tế giúp tránh công việc trùng lặp. 

Bên trong vòng lặp trong cùng, chúng tôi thu thập chính xác`k`các phần tử. Từ`k * step = n`, chúng ta quay trở lại vị trí ban đầu sau khi chính xác`k`nhảy. 

Một chi tiết khác là số học modulo không cần thiết ở đây. Bởi vì`pos`tăng lên bởi`step`chính xác`k`lần, chỉ mục được truy cập lớn nhất là:```
start + (k - 1) * step < n
```Vì vậy, mọi quyền truy cập đều nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
1 2 -3 4 -5 5 2 3
```Ước số hợp lệ`k ≥ 3`là`4`Và`8`. 

Vì`k = 4`:```
step = 8 / 4 = 2
```| Bắt đầu | Giá trị đã truy cập | Tổng hợp | 
| --- | --- | --- | 
| 0 | 1, -3, -5, 2 | -5 | 
| 1 | 2, 4, 5, 3 | 14 | 

Vì`k = 8`: 

| Bắt đầu | Giá trị đã truy cập | Tổng hợp | 
| --- | --- | --- | 
| 0 | tất cả các giá trị | 9 | 

Tối đa là`14`. 

Dấu vết này cho thấy tại sao các lựa chọn không liền kề lại quan trọng. Đa giác tốt nhất bỏ qua mọi tác phẩm điêu khắc khác. 

### Ví dụ 2 

đầu vào:```
6
100 -1 100 -1 100 -1
```Các ước số hợp lệ là`3`Và`6`. 

Vì`k = 3`:```
step = 2
```| Bắt đầu | Giá trị đã truy cập | Tổng hợp | 
| --- | --- | --- | 
| 0 | 100, 100, 100 | 300 | 
| 1 | -1, -1, -1 | -3 | 

Vì`k = 6`: 

| Bắt đầu | Giá trị đã truy cập | Tổng hợp | 
| --- | --- | --- | 
| 0 | tất cả các giá trị | 297 | 

Câu trả lời là`300`. 

Ví dụ này chứng minh tại sao việc kiểm tra các đa giác hợp lệ nhỏ hơn là cần thiết ngay cả khi việc giữ lại tất cả các tác phẩm điêu khắc đã mang lại một số tiền lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n × d(n)) | Chúng tôi xử lý mọi ước số và duyệt qua nhiều nhất`n`các phần tử cho mỗi | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Số ước của các số nguyên lên tới`20000`rất nhỏ nên thời gian chạy thực tế gần như tuyến tính. Giải pháp vừa vặn thoải mái trong giới hạn 3 giây trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        ans = sum(a)

        for k in range(3, n + 1):
            if n % k != 0:
                continue

            step = n // k

            for start in range(step):
                cur = 0
                pos = start

                for _ in range(k):
                    cur += a[pos]
                    pos += step

                ans = max(ans, cur)

        return str(ans)

    return solve()

# provided sample
assert run("8\n1 2 -3 4 -5 5 2 3\n") == "14", "sample 1"

# minimum size
assert run("3\n1 2 3\n") == "6", "minimum n"

# all negative
assert run("4\n-1 -2 -3 -4\n") == "-10", "must keep at least 3"

# alternating large values
assert run("6\n100 -1 100 -1 100 -1\n") == "300", "every second element"

# prime n
assert run("5\n1 2 3 4 5\n") == "15", "only full polygon possible"

# all equal
assert run("12\n7 7 7 7 7 7 7 7 7 7 7 7\n") == "84", "uniform values"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 2 3`|`6`| Kích thước tối thiểu cho phép | 
|`4 / -1 -2 -3 -4`|`-10`| Phải giữ ít nhất 3 tác phẩm điêu khắc | 
|`6 / 100 -1 100 -1 100 -1`|`300`| Giải pháp tốt nhất bỏ qua các yếu tố | 
|`5 / 1 2 3 4 5`|`15`| Xuất sắc`n`chỉ cho phép chu kỳ đầy đủ | 
|`12 / all 7s`|`84`| Mảng thống nhất và xử lý số chia | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các giá trị đều âm:```
4
-1 -2 -3 -4
```Một giải pháp bất cẩn có thể cố gắng chỉ giữ lại một tác phẩm điêu khắc có giá trị`-1`, nhưng đa giác có ít hơn 3 đỉnh là không hợp lệ. 

Thuật toán chỉ kiểm tra các ước số`k ≥ 3`. 

Vì`k = 4`:```
sum = -10
```Không có đa giác hợp lệ nào khác tồn tại, vì vậy câu trả lời là`-10`. 

Bây giờ hãy xem xét một số lượng lớn các tác phẩm điêu khắc:```
5
1 2 3 4 5
```Ước duy nhất của 5 là 1 và 5. Vì chúng ta cần ít nhất 3 đỉnh nên chỉ`k = 5`được xem xét. 

Thuật toán tính toán:```
1 + 2 + 3 + 4 + 5 = 15
```Điều này xử lý chính xác thực tế là mọi số nguyên tố có kích thước bước khác 0 đều đi qua toàn bộ vòng tròn. 

Cuối cùng, hãy xem xét các chu kỳ lặp đi lặp lại:```
8
1 2 -3 4 -5 5 2 3
```Vì`k = 4`, kích thước bước là 2. 

Vị trí bắt đầu lớn hơn 1 sẽ lặp lại chu kỳ hiện có:```
0 -> 2 -> 4 -> 6
1 -> 3 -> 5 -> 7
```Thuật toán chỉ lặp lại bắt đầu trong`[0, step-1]`, do đó mọi đa giác riêng biệt được xử lý chính xác một lần.
