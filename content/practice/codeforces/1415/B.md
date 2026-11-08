---
title: "CF 1415B - Phố Sơn Lại"
description: "Chúng tôi có một dãy nhà, mỗi dãy nhà được sơn một màu nào đó. Trong một ngày, Tom chọn một đoạn liền kề có đúng k căn nhà. Bên trong phân khúc đó, anh ta có thể sơn lại bất kỳ tập hợp con nào của những ngôi nhà đó và mỗi ngôi nhà có thể được sơn lại bất kỳ màu nào một cách độc lập."
date: "2026-06-11T07:11:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1415
codeforces_index: "B"
codeforces_contest_name: "Technocup 2021 - Elimination Round 2"
rating: 1100
weight: 1415
solve_time_s: 113
verified: true
draft: false
---

[CF 1415B - Phố sơn lại](https://codeforces.com/problemset/problem/1415/B) 

**Đánh giá:** 1100 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dãy nhà, mỗi dãy nhà được sơn một màu nào đó. Trong một ngày, Tom chọn đúng một đoạn liền kề`k`những ngôi nhà. Bên trong phân khúc đó, anh ta có thể sơn lại bất kỳ tập hợp con nào của những ngôi nhà đó và mỗi ngôi nhà có thể được sơn lại bất kỳ màu nào một cách độc lập. 

Mục tiêu là làm cho mọi ngôi nhà đều sơn cùng một màu trong khi sử dụng ít ngày nhất có thể. 

Một cách hữu ích để hình dung về hoạt động này là một ngày cho phép chúng ta "sửa" tối đa`k`các vị trí liên tiếp. Vì những ngôi nhà trong phân khúc đã chọn có thể được sơn lại theo cách chúng ta muốn, điều quan trọng duy nhất là ngôi nhà nào đã có màu mục tiêu cuối cùng và ngôi nhà nào thì không. 

Số lượng màu nhiều nhất là 100, trong khi tổng số nhà trong tất cả các trường hợp thử nghiệm nhiều nhất là`10^5`. Điều này ngay lập tức gợi ý rằng việc thử mọi màu cuối cùng có thể là khả thi. Mặt khác, bất kỳ thứ gì liên tục quét các phần lớn của mảng cho mọi vị trí sẽ trở nên quá đắt. MỘT`O(n * 100)`giải pháp dễ dàng đủ nhanh, trong khi`O(n^2)`thì không. 

Có một số tình huống tế nhị có thể gây ra suy luận sai. 

Coi như:```
n = 5, k = 2
1 1 1 1 1
```Câu trả lời là`0`vì đường phố vốn đã đẹp rồi. Một giải pháp bất cẩn luôn thực hiện ít nhất một thao tác cho màu đã chọn sẽ trả về`1`. 

Coi như:```
n = 5, k = 5
1 2 3 4 5
```Câu trả lời là`1`. Một đoạn dài 5 bao phủ toàn bộ con phố nên mọi ngôi nhà đều có thể sơn lại trong ngày. Một giải pháp đếm từng ngôi nhà không khớp sẽ trả về sai`5`. 

Coi như:```
n = 7, k = 3
1 2 1 2 1 2 1
```Nếu màu mục tiêu là`1`, các điểm không khớp nằm ở các vị trí 2, 4 và 6. Một thao tác bắt đầu từ vị trí 2 bao gồm các vị trí từ 2 đến 4, sửa hai điểm không khớp cùng một lúc. Một thao tác khác bắt đầu từ vị trí 6 sẽ sửa lỗi cuối cùng. Câu trả lời là`2`, không`3`. Việc đếm các phần không khớp thay vì các phân đoạn sẽ đánh giá quá cao kết quả. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ nhất là chọn màu mục tiêu và mô phỏng tất cả các chiến lược sơn lại có thể có. Chúng tôi có thể lập mô hình trạng thái đường phố sau mỗi hoạt động và tìm kiếm số ngày tối thiểu. Điều này đúng vì nó khám phá tất cả các khả năng, nhưng số lượng các phân đoạn có thể có và các lựa chọn sơn lại lại tăng lên một cách bùng nổ. Ngay cả đối với các giá trị vừa phải của`n`, việc tìm kiếm như vậy trở nên hoàn toàn không thực tế. 

Quan sát quan trọng là màu sắc thực tế bên trong thao tác sơn lại không quan trọng. Một khi chúng tôi quyết định rằng màu đường phố cuối cùng sẽ là`x`, mọi vị trí đều đã chính xác hoặc cần phải trở thành`x`. 

Giả sử chúng ta quét đường từ trái sang phải trong khi nhắm mục tiêu màu`x`. 

Khi chúng ta bắt gặp một ngôi nhà đã có màu sắc`x`, chúng tôi không làm gì cả. 

Khi chúng ta gặp một ngôi nhà có màu sắc khác, vị trí đó cuối cùng phải được che đi bằng một thao tác sơn lại nào đó. Sự lựa chọn tốt nhất là bắt đầu một hoạt động ở đây. Vì một thao tác ảnh hưởng đến một đoạn có độ dài`k`, chúng ta có thể bỏ qua ngay phần tiếp theo`k`các vị trí vì tất cả chúng đều có thể được cố định trong cùng một ngày. 

Bước tham lam này là tối ưu. Bất kỳ giải pháp nào cũng phải che đậy vị trí không khớp đó bằng cách nào đó và bắt đầu phân đoạn sơn lại ở vị trí không khớp chưa được phát hiện đầu tiên sẽ che phủ càng nhiều vị trí trong tương lai càng tốt. 

Vì màu sắc bị giới hạn trong phạm vi`1..100`, chúng ta có thể thử mọi màu mục tiêu có thể và lấy được câu trả lời tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(100n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo câu trả lời với giá trị rất lớn. 
2. Đối với mọi màu mục tiêu có thể`x`từ 1 đến 100, tính xem cần bao nhiêu ngày để toàn bộ đường phố phải nhuộm màu`x`. 
3. Quét các ngôi nhà từ trái sang phải bằng chỉ mục`i`. 
4. Nếu nhà`i`đã có màu rồi`x`, di chuyển đến vị trí tiếp theo vì không cần sơn lại ở đó. 
5. Nếu nhà`i`có màu khác thì việc sơn lại một lần là điều khó tránh khỏi. Tăng số ngày lên một. 
6. Sau khi sử dụng thao tác sơn lại đó, hãy bỏ qua thao tác tiếp theo.`k`vị trí bằng cách thiết lập`i += k`. 

Hoạt động có thể bao gồm một đoạn có chiều dài`k`bắt đầu từ điểm không khớp này, do đó mọi vị trí bên trong phân khúc đó có thể được cố định trong cùng một ngày. 
7. Tiếp tục quét cho đến khi đến cuối đường. 
8. Cập nhật câu trả lời chung với số ngày tối thiểu trong số tất cả các màu mục tiêu. 
9. Đưa ra câu trả lời tối thiểu. 

### Tại sao nó hoạt động 

Sửa màu mục tiêu`x`. 

Trong quá trình quét từ trái sang phải, giả sử vị trí`i`là điểm không khớp đầu tiên chưa được khắc phục bởi bất kỳ hoạt động sơn lại nào trước đó. Bất kỳ giải pháp hợp lệ nào cũng phải sử dụng một số vị trí hoạt động bao phủ`i`, nếu không ngôi nhà đó sẽ không bao giờ có màu sắc`x`. 

Bắt đầu hoạt động tại vị trí`i`không bao giờ tệ hơn việc bắt đầu sớm hơn, vì các vị trí trước đó đã được xử lý. Bắt đầu lúc`i`bao gồm vị trí`i`và mở rộng phạm vi bao phủ càng xa về bên phải càng tốt, có khả năng khắc phục những điểm không khớp bổ sung trong tương lai. 

Lựa chọn tham lam luôn bao trùm phạm vi hữu ích tối đa bắt đầu từ sự không phù hợp được phát hiện đầu tiên. Việc lặp lại quá trình này sẽ tạo ra số lượng thao tác tối thiểu cho màu mục tiêu đã chọn. Vì màu cuối cùng tối ưu phải là một trong các màu nằm trong phạm vi cho phép, nên việc kiểm tra tất cả các màu và lấy giá trị tối thiểu sẽ mang lại giá trị tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, k = map(int, input().split())
        c = list(map(int, input().split()))

        answer = float('inf')

        for target in range(1, 101):
            days = 0
            i = 0

            while i < n:
                if c[i] == target:
                    i += 1
                else:
                    days += 1
                    i += k

            answer = min(answer, days)

        print(answer)

solve()
```Vòng lặp bên ngoài thử mọi màu cuối cùng có thể. Vì màu sắc bị giới hạn tối đa 100 giá trị riêng biệt nên giá này rất rẻ. 

Đối với màu mục tiêu cố định, biến`i`quét đường phố. Khi một vị trí đã phù hợp với mục tiêu, chúng tôi chỉ cần tiến lên một vị trí. 

Khi tìm thấy sự không phù hợp, chúng tôi ngay lập tức dành một ngày và tiếp tục`k`các vị trí. Mô hình này đặt một đoạn sơn lại có chiều dài`k`bắt đầu từ sự không phù hợp hiện tại. Phân khúc này sẽ sửa chữa mọi ngôi nhà trong phạm vi cần thay đổi của nó, vì vậy việc xem lại những vị trí đó sẽ là thừa. 

Một sai lầm dễ mắc phải là nhảy qua`k - 1`thay vì`k`. Vị trí không khớp hiện tại cũng là một phần của đoạn được sơn nên toàn bộ đoạn đó chứa chính xác`k`các vị trí. 

Một lỗi phổ biến khác là chỉ thử các màu đã xuất hiện trong mảng. Cách đó thực sự có tác dụng với vấn đề này, nhưng lặp lại qua tất cả các màu`1..100`đơn giản hơn và vẫn chạy thoải mái trong giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 10, k = 2
1 1 2 2 1 1 2 2 2 1
```Hãy thử màu mục tiêu`2`. 

| Bước | Vị trí | Màu sắc | Hành động | Ngày | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | không khớp, sơn | 1 | 
| 2 | 3 | 2 | đã đúng rồi | 1 | 
| 3 | 4 | 2 | đã đúng rồi | 1 | 
| 4 | 5 | 1 | không khớp, sơn | 2 | 
| 5 | 7 | 2 | đã đúng rồi | 2 | 
| 6 | 8 | 2 | đã đúng rồi | 2 | 
| 7 | 9 | 2 | đã đúng rồi | 2 | 
| 8 | 10 | 1 | không khớp, sơn | 3 | 

Kết quả cho màu mục tiêu`2`là 3 ngày. Không có màu mục tiêu nào khác tốt hơn, vì vậy câu trả lời cuối cùng là`3`. 

Dấu vết này cho thấy một thao tác sơn lại có thể loại bỏ một số điểm không khớp trong tương lai bằng cách bỏ qua toàn bộ khối chiều dài`k`. 

### Ví dụ 2 

đầu vào:```
n = 10, k = 3
1 3 3 3 3 1 2 1 3 3
```Hãy thử màu mục tiêu`3`. 

| Bước | Vị trí | Màu sắc | Hành động | Ngày | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | không khớp, sơn | 1 | 
| 2 | 4 | 3 | đã đúng rồi | 1 | 
| 3 | 5 | 3 | đã đúng rồi | 1 | 
| 4 | 6 | 1 | không khớp, sơn | 2 | 
| 5 | 9 | 3 | đã đúng rồi | 2 | 
| 6 | 10 | 3 | đã đúng rồi | 2 | 

Câu trả lời cho màu sắc`3`là 2 ngày, là tối ưu. 

Ví dụ này chứng minh tại sao nhảy chính xác`k`các vị trí là chính xác. Hoạt động ở vị trí 6 bao gồm các vị trí từ 6 đến 8, sửa chữa tất cả các ngôi nhà không thuộc 3 còn lại trong khu vực đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100n) | Quét mảng một lần để tìm từng màu có thể | 
| Không gian | O(1) | Chỉ có một vài quầy được lưu trữ | 

Vì tổng của tất cả`n`giá trị nhiều nhất là`10^5`, tổng công việc là khoảng`100 × 10^5 = 10^7`các thao tác đơn giản, dễ dàng phù hợp với giới hạn thời gian trong Python. Việc sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        c = list(map(int, input().split()))

        ans = 10**9

        for target in range(1, 101):
            days = 0
            i = 0

            while i < n:
                if c[i] == target:
                    i += 1
                else:
                    days += 1
                    i += k

            ans = min(ans, days)

        out.append(str(ans))

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""3
10 2
1 1 2 2 1 1 2 2 2 1
7 1
1 2 3 4 5 6 7
10 3
1 3 3 3 3 1 2 1 3 3
"""
) == "3\n6\n2\n", "sample"

# minimum size
assert run(
"""1
1 1
5
"""
) == "0\n", "single house"

# already beautiful
assert run(
"""1
5 3
7 7 7 7 7
"""
) == "0\n", "all equal"

# k = n
assert run(
"""1
5 5
1 2 3 4 5
"""
) == "1\n", "whole array in one operation"

# off-by-one coverage check
assert run(
"""1
7 3
1 2 1 2 1 2 1
"""
) == "2\n", "segment coverage"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 5`|`0`| Ví dụ kích thước tối thiểu | 
|`7 7 7 7 7`|`0`| Đường đã đẹp | 
|`1 2 3 4 5`với`k=5`|`1`| Toàn bộ đường phố được bao phủ trong một ngày | 
|`1 2 1 2 1 2 1`với`k=3`|`2`| Bỏ qua phân đoạn chính xác và xử lý từng đoạn một | 

## Vỏ cạnh 

Hãy xem xét:```
1
5 5
1 2 3 4 5
```Đối với màu mục tiêu`1`, điểm không khớp đầu tiên xuất hiện ở vị trí 2. Một thao tác sơn lại bao gồm các vị trí từ 2 đến 5, do đó quá trình quét ngay lập tức thoát ra. Thuật toán trả về`1`. Điều này phù hợp với thực tế là một đoạn có độ dài`n`bao trùm toàn bộ đường phố. 

Coi như:```
1
5 3
4 4 4 4 4
```Mọi vị trí đều đã khớp với màu mục tiêu`4`. Quá trình quét không bao giờ đi vào nhánh sơn lại, vì vậy số ngày vẫn còn`0`. Thuật toán nhận ra chính xác rằng không cần phải làm việc gì. 

Coi như:```
1
7 3
1 2 1 2 1 2 1
```Sử dụng màu mục tiêu`1`, điểm không khớp đầu tiên là ở vị trí 2. Một thao tác bao gồm các vị trí từ 2 đến 4. Sự không khớp tiếp theo chưa được phát hiện là vị trí 6, cần thêm một thao tác nữa. Tổng số ngày bằng nhau`2`. Điều này xác nhận rằng thuật toán nhóm nhiều điểm không khớp thành một lần sơn lại bất cứ khi nào chúng nằm trong cùng một chiều dài-`k`phân đoạn. 

Coi như:```
1
7 1
1 2 3 4 5 6 7
```Với`k = 1`, mọi hoạt động đều ảnh hưởng đến đúng một ngôi nhà. Đối với bất kỳ màu mục tiêu nào được chọn, sáu ngôi nhà khác nhau, do đó cần có sáu thao tác sơn lại. Thuật toán tìm từng điểm không khớp và tiến lên một vị trí, tạo ra câu trả lời đúng`6`.
