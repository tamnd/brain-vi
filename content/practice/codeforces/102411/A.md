---
title: "CF 102411A - Chuyển động chính xác"
description: "Ta có một hộp có chiều dài n chứa hai thanh nằm trên hai thanh ray song song. Thanh ngắn có chiều dài a, thanh dài có chiều dài b, với a < b. Thanh dài có một nút chặn ở mỗi đầu và thanh ngắn phải nằm hoàn toàn giữa hai nút đó."
date: "2026-08-11T07:22:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102411
codeforces_index: "A"
codeforces_contest_name: "ICPC 2019-2020 North-Western Russia Regional Contest"
rating: 0
weight: 102411
solve_time_s: 332
verified: true
draft: false
---

[CF 102411A - Chuyển động chính xác](https://codeforces.com/problemset/problem/102411/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hộp có chiều dài`n`chứa hai thanh nằm trên đường ray song song. Thanh ngắn có chiều dài`a`, thanh dài có chiều dài`b`, với`a < b`. Thanh dài có một nút chặn ở mỗi đầu và thanh ngắn phải nằm hoàn toàn giữa hai nút đó. 

Ban đầu, cả hai thanh đều bắt đầu ngang bằng với cạnh trái của hộp. Mục tiêu là di chuyển cả hai thanh cho đến khi chúng ngang bằng với bên phải, sử dụng càng ít bước di chuyển càng tốt. Trong một lần di chuyển, chính xác một thanh có thể được thay đổi vị trí, trong khi thanh kia vẫn cố định. Hạn chế duy nhất là sau mỗi lần di chuyển thanh ngắn vẫn phải nằm giữa hai nút chặn của thanh dài. 

Đầu vào chứa`a`,`b`, Và`n`, Ở đâu`1 ≤ a < b ≤ n ≤ 10^7`. Giới hạn trên của`10^7`cho chúng ta biết rằng một thuật toán thực hiện một lượng công việc không đổi là lý tưởng, trong khi ngay cả một thuật toán tỷ lệ thuận với`n^2`là hoàn toàn không khả thi. Một mô phỏng tuyến tính có thể đạt tới khoảng mười triệu lần lặp trong trường hợp xấu nhất, có khả năng nằm dưới giới hạn hai giây trong Python. Công thức trực tiếp được ưa chuộng hơn vì nó giảm việc tính toán xuống còn một vài phép tính số học. Giới hạn chính thức là hai giây và giới hạn bộ nhớ là 512 MB. 

Trường hợp cạnh đầu tiên là khi thanh dài đã lấp đầy toàn bộ hộp. Ví dụ, với đầu vào`1 3 3`, thanh dài có chiều dài`3`và cái hộp cũng có chiều dài`3`. Nó không thể di chuyển chút nào, trong khi thanh ngắn có thể di chuyển trực tiếp từ nút bên trái sang nút bên phải, vì vậy câu trả lời là`1`. Một giải pháp bất cẩn luôn cho rằng cả hai thanh phải di chuyển sẽ quay trở lại ít nhất`2`. 

Trường hợp cạnh thứ hai xảy ra khi`b - a = 1`. Ví dụ,`2 3 5`chỉ có một đơn vị không gian trống bên trong thanh dài xung quanh thanh ngắn. Thanh dài phải di chuyển`5 - 3 = 2`đơn vị và mỗi cặp nước đi chỉ có thể tiến thêm một đơn vị, vì vậy câu trả lời là`2 * 2 + 1 = 5`. Một công thức sử dụng phép chia số nguyên thay vì trần sẽ chỉ tính sai`3`di chuyển. 

Trường hợp cạnh thứ ba là khi chuyển động cần thiết của thanh dài không chia hết cho`b - a`. Ví dụ,`2 5 8`yêu cầu thanh dài để di chuyển`3`đơn vị, trong khi thanh ngắn có thể tạo ra khoảng cách tối đa`3`đơn vị. Ở đây, một cặp bước di chuyển đầy đủ sẽ đẩy thanh dài lên`3`, cho`3`tổng cộng sẽ di chuyển. Với`2 4 9`, tuy nhiên, thanh dài phải di chuyển`5`đơn vị và mỗi cặp có thể nâng cao nó tối đa`2`, vậy cần có ba cặp và câu trả lời là`7`. Việc vận hành trần là cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực có thể mô hình hóa vị trí của cả hai thanh một cách rõ ràng. Cho phép`x`là điểm cuối bên trái của thanh dài và`y`điểm cuối bên trái của thanh ngắn. Một nhà nước pháp lý đáp ứng`0 ≤ x ≤ n - b`Và`x ≤ y ≤ x + b - a`. 

Bắt đầu từ`(0, 0)`, chúng ta có thể thực hiện tìm kiếm đường đi ngắn nhất trên tất cả các trạng thái nguyên có thể có. có`O(n^2)`cặp có thể`(x, y)`và từ một trạng thái, chúng ta có thể thử nhiều vị trí mới có thể có cho thanh đã chọn. Điều đó từ bỏ`O(n^3)`làm việc một cách trực tiếp nhất. Với`n = 10^7`, đây là theo thứ tự của`10^21`hoạt động, vì vậy nó không khả thi từ xa. 

Lực lượng vũ phu hoạt động vì nó thể hiện rõ ràng mọi vị trí tương đối hợp pháp của các thanh. Quan sát quan trọng là chúng ta không thực sự cần phải xem xét tất cả các quan điểm đó. Cho phép`d = b - a`. 

Giả sử thanh dài bắt đầu ở vị trí`x`. Vì thanh ngắn phải nằm giữa hai nút chặn nên điểm cuối bên trái của nó tối đa có thể`x + d`. Như vậy, khi thanh dài cố định thì thanh ngắn chỉ được dịch chuyển sang phải nhiều nhất`d`. 
Bây giờ đảo ngược vai trò. Thanh ngắn là ở`y`, thanh dài có thể di chuyển sang phải cho đến khi điểm cuối bên trái của nó chạm tới`y`. Nếu lần đầu tiên chúng ta đặt thanh ngắn tại`x + d`, thanh dài sau đó có thể di chuyển từ`x`ĐẾN`x + d`. Vì vậy, một bước di chuyển của thanh ngắn theo sau là một bước di chuyển của thanh dài có thể đẩy thanh dài lên một cách chính xác.`d`. 

Việc dừng di chuyển trước vị trí tối đa có thể của nó không bao giờ có lợi. Di chuyển xa hơn về bên phải không thể làm cho trạng thái trong tương lai trở nên tồi tệ hơn, bởi vì mọi vị trí pháp lý sau này cũng xa hơn về bên phải. Điều này cho phép chúng ta luân phiên các thanh một cách tham lam, với mỗi bước di chuyển ngoại trừ bước di chuyển của thanh ngắn cuối cùng đều đóng góp chính xác.`d`đơn vị tiến độ cho thanh dài. Hướng dẫn chính thức đưa ra quan sát và công thức kết quả tương tự. 

Thanh dài phải di chuyển từ vị trí`0`để định vị`n - b`, vậy số cặp nước đi ngắn/dài cần có là`ceil((n - b) / (b - a))`. 

Mỗi cặp sử dụng hai nước đi và sau khi thanh dài đạt đến vị trí cuối cùng, nước đi cuối cùng sẽ đặt thanh ngắn vào cạnh phải của hộp. Do đó câu trả lời là`2 * ceil((n - b) / (b - a)) + 1`. 

Chúng ta có thể tính mức trần cho các số nguyên dương với`ceil(x / d) = (x + d - 1) // d`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n²) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`a`,`b`, Và`n`. Đại lượng duy nhất quan trọng đối với số lượng chuyển động là khoảng cách mà thanh dài phải di chuyển,`n - b`và độ dịch chuyển tương đối tối đa có sẵn giữa các thanh,`b - a`. 
2. Tính toán`distance = n - b`. Đây là tổng khoảng cách mà điểm cuối bên trái của thanh dài phải di chuyển trước khi thanh dài chạm tới cạnh bên phải của hộp. 
3. Tính toán`gap = b - a`. Đây là khoảng cách lớn nhất mà thanh ngắn có thể được đặt trước thanh dài trong khi vẫn giữ nguyên giữa hai nút chặn của thanh dài. 
4. Tính toán`pairs = (distance + gap - 1) // gap`. Mỗi cặp bao gồm việc di chuyển thanh ngắn sang phải càng xa càng tốt và sau đó di chuyển thanh dài sang phải càng xa càng tốt. Một cặp như vậy đẩy thanh dài lên`gap`, ngoại trừ cặp cuối cùng, có thể tăng nó ít hơn vì thanh dài đã chạm đến ranh giới. 
5. Nhân`pairs`bởi vì mỗi cặp có một bước di chuyển thanh ngắn và một bước di chuyển thanh dài. 
6. Thêm một bước cuối cùng cho thanh ngắn. Sau khi thanh dài đã đạt đến vị trí mục tiêu, thanh ngắn có thể được di chuyển trực tiếp đến vị trí mục tiêu trong khi vẫn ở giữa các nút chặn cuối cùng. 

### Tại sao nó hoạt động 

Điều bất biến là ngay trước mỗi lần di chuyển thanh dài, thanh ngắn có thể được đặt tối đa`b - a`đơn vị phía trước thanh dài. Do đó, một bước di chuyển thanh dài có thể tiến tới điểm cuối bên trái của nó nhiều nhất`b - a`. Chiến lược tham lam đạt chính xác mức tối đa đó bất cứ khi nào cần tiến bộ hơn, vì vậy không có chiến lược nào có thể di chuyển thanh dài nhanh hơn về số lần di chuyển. 

Thanh dài cần đi lại`n - b`đơn vị, vì vậy ít nhất`ceil((n - b) / (b - a))`di chuyển thanh dài là cần thiết. Trước mỗi lần di chuyển thanh dài như vậy, ngoại trừ cấu hình ban đầu, cần phải di chuyển thanh ngắn để tạo khoảng cách cần thiết. Khi thanh dài đạt đến mục tiêu, chỉ còn lại đúng một bước di chuyển của thanh ngắn. Trình tự được xây dựng đạt được giới hạn dưới này, mang lại`2 * ceil((n - b) / (b - a)) + 1`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, n = map(int, input().split())

    distance = n - b
    gap = b - a

    pairs = (distance + gap - 1) // gap
    answer = 2 * pairs + 1

    print(answer)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên đọc ba chiều trực tiếp từ đầu vào tiêu chuẩn. Chỉ có một ca kiểm thử trong bài toán này, do đó không cần vòng lặp ca kiểm thử.`distance = n - b`là mức độ mà điểm cuối bên trái của thanh dài phải di chuyển. Nếu như`n == b`, cái này trở thành số 0, điều này dẫn đến một cách chính xác các cặp số 0 và câu trả lời là`1`.`gap = b - a`luôn dương vì mệnh đề đảm bảo`a < b`. Điều này có nghĩa là phép chia trần là an toàn và không bao giờ chia cho 0. 

biểu thức`(distance + gap - 1) // gap`thực hiện phép chia trần mà không cần số học dấu phẩy động. Dấu phẩy động ở đây là không cần thiết và có thể gây ra các vấn đề về độ chính xác có thể tránh được, trong khi số học số nguyên là chính xác cho toàn bộ phạm vi đầu vào. 

Cuối cùng,`2 * pairs + 1`đếm hai bước di chuyển trong mỗi cặp ngắn/dài và một bước di chuyển thanh ngắn cuối cùng. Số nguyên Python có độ chính xác tùy ý, mặc dù câu trả lời ở đây dễ dàng nằm trong phạm vi số nguyên 64 bit thông thường. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là`1 3 6`. Mẫu chính thức đưa ra câu trả lời`5`. 

Ở đây thanh dài phải di chuyển`6 - 3 = 3`đơn vị, trong khi khoảng cách tối đa giữa hai thanh là`3 - 1 = 2`. 

| Bước |`a`|`b`|`n`|`distance`|`gap`|`pairs`|`answer`| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 1 | 3 | 6 | 3 | 2 | 2 | 5 | 
| Phân chia trần | 1 | 3 | 6 | 3 | 2 | 2 | 5 | 
| Công thức cuối cùng | 1 | 3 | 6 | 3 | 2 | 2 | 5 | 

Cặp đầu tiên di chuyển thanh ngắn về phía trước hai đơn vị và sau đó di chuyển thanh dài về phía trước hai đơn vị. Cặp thứ hai hoàn thành một đơn vị chuyển động thanh dài còn lại. Việc di chuyển thanh ngắn cuối cùng đặt nó ở phía bên phải. Vì vậy trình tự yêu cầu`2 * 2 + 1 = 5`di chuyển. 

### Mẫu 2 

Đầu vào là`2 4 9`. Mẫu chính thức đưa ra câu trả lời`7`. 

Thanh dài phải di chuyển`9 - 4 = 5`đơn vị, trong khi khoảng cách tối đa là`4 - 2 = 2`. 

| Bước |`a`|`b`|`n`|`distance`|`gap`|`pairs`|`answer`| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 2 | 4 | 9 | 5 | 2 | 3 | 7 | 
| Phân chia trần | 2 | 4 | 9 | 5 | 2 | 3 | 7 | 
| Công thức cuối cùng | 2 | 4 | 9 | 5 | 2 | 3 | 7 | 

Hai cặp nâng thanh dài lên bốn đơn vị. Cặp thứ ba xử lý một đơn vị còn lại và việc di chuyển thanh ngắn cuối cùng sẽ hoàn tất cấu hình. Trần nhà là thứ thay đổi`5 / 2`từ hai cặp đến ba. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Giá trị tối đa của`n`là`10^7`, nhưng thuật toán không bao giờ lặp lại tới`n`. Nó chỉ thực hiện phép trừ, cộng, nhân và chia số nguyên, vì vậy nó thoải mái trong giới hạn thời gian hai giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(inp: str) -> str:
    data = list(map(int, inp.split()))
    a, b, n = data

    distance = n - b
    gap = b - a
    pairs = (distance + gap - 1) // gap

    return str(2 * pairs + 1)

# provided samples
assert solve("1 3 6\n") == "5\n"[:-1], "sample 1"
assert solve("2 4 9\n") == "7", "sample 2"

# minimum-size input
assert solve("1 2 2\n") == "1", "long bar already fills the box"

# all possible movement is by one unit
assert solve("1 2 5\n") == "7", "gap is one"

# exact divisibility
assert solve("2 5 8\n") == "3", "distance is exactly one gap"

# non-divisible boundary case
assert solve("2 4 8\n") == "5", "distance is not divisible by gap"

# maximum-size input
assert solve("1 10000000 10000000\n") == "1", "maximum n with b = n"
```Các trường hợp tùy chỉnh bao gồm cấu hình không cần chuyển động thanh dài, khoảng cách nhỏ nhất có thể, khả năng chia hết chính xác, chuyển động không chia hết và giá trị đầu vào lớn nhất được phép. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 2`|`1`| Thanh dài đã lấp đầy hộp | 
|`1 2 5`|`7`| Khoảng cách nhỏ nhất có thể, di chuyển lặp đi lặp lại | 
|`2 5 8`|`3`| Chia hết chính xác | 
|`2 4 8`|`5`| Ranh giới phân chia trần | 
|`1 10000000 10000000`|`1`| Kích thước đầu vào tối đa và không yêu cầu chuyển động thanh dài | 

## Vỏ cạnh 

Khi nào`n = b`, thanh dài đã kéo dài hết hộp hoàn chỉnh. Đối với đầu vào`1 3 3`, chúng tôi có`distance = 3 - 3 = 0`Và`gap = 3 - 1 = 2`. Việc phân chia trần mang lại`pairs = 0`, vậy câu trả lời là`2 * 0 + 1 = 1`. Việc di chuyển cần thiết duy nhất là thanh ngắn di chuyển từ bên trái sang bên phải. 

Khi`b - a = 1`, thanh ngắn có chiều dài gần bằng thanh dài nên nó chỉ có thể tạo ra khoảng cách một đơn vị. Đối với đầu vào`1 2 5`, thanh dài phải di chuyển`3`đơn vị. Vì mỗi bước di chuyển của thanh dài chỉ có thể tiến lên một đơn vị nên cần có ba cặp, tiếp theo là bước di chuyển của thanh ngắn cuối cùng. Công thức cho`2 * ceil(3 / 1) + 1 = 7`. 

Khi khoảng cách chia hết cho khoảng trống thì không cần phải có cặp hoàn chỉnh một phần. Đối với đầu vào`2 5 8`, thanh dài di chuyển`3`đơn vị và khoảng cách có sẵn cũng là`3`. Do đó, một lần di chuyển thanh ngắn và một lần di chuyển thanh dài sẽ đặt thanh dài chính xác vào mục tiêu của nó, tiếp theo là một lần di chuyển thanh ngắn cuối cùng. Câu trả lời là`3`. 

Khi khoảng cách không chia hết cho khoảng trống, bước di chuyển thanh dài cuối cùng chỉ sử dụng một phần khoảng cách có sẵn. Đối với đầu vào`2 4 8`, thanh dài phải di chuyển`4`đơn vị trong khi mỗi cặp chỉ có thể cung cấp`2`đơn vị. Cần có hai cặp, cho`2 * 2 + 1 = 5`. Việc thay thế trần bằng phép chia số nguyên thông thường sẽ có tác dụng đối với trường hợp chia hết chính xác này, nhưng nó sẽ thất bại đối với các đầu vào như`2 4 9`, Ở đâu`5 / 2`yêu cầu ba cặp chứ không phải hai. 

Nếu bạn muốn, tôi cũng có thể biến bài xã luận này thành một bài xã luận ngắn hơn theo phong cách Codeforces để giữ lại bằng chứng nhưng loại bỏ phần thảo luận chi tiết và có chủ ý.
