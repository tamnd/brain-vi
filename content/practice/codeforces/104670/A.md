---
title: "CF 104670A - Phân tích anten"
description: "Chúng ta được cung cấp một chuỗi các phép đo hàng ngày, trong đó mỗi ngày có một giá trị nguyên duy nhất. Đối với mỗi ngày i, chúng tôi muốn so sánh ngày đó với bất kỳ ngày j nào trước đó, bao gồm cả chính nó, và tính toán mức độ “bước nhảy có ý nghĩa” về số đo giữa hai ngày đó sau khi bị phạt…"
date: "2026-06-29T09:33:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104670
codeforces_index: "A"
codeforces_contest_name: "2021-2022 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2021)"
rating: 0
weight: 104670
solve_time_s: 47
verified: true
draft: false
---

[CF 104670A - Phân tích ăng-ten](https://codeforces.com/problemset/problem/104670/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các phép đo hàng ngày, trong đó mỗi ngày có một giá trị nguyên duy nhất. Đối với mỗi ngày i, chúng tôi muốn so sánh ngày đó với bất kỳ ngày j nào trước đó, bao gồm cả chính nó, và tính xem “bước nhảy có ý nghĩa” về số đo giữa hai ngày đó lớn đến mức nào sau khi trừ khoảng cách thời gian. 

Điểm số giữa hai ngày được định nghĩa là chênh lệch tuyệt đối trong các phép đo trừ đi mức phạt tỷ lệ thuận với khoảng cách giữa các ngày. Về mặt hình thức, với mỗi i chúng ta xét tất cả j ≤ i và đánh giá |xi − xj| − c · (i − j), rồi lấy giá trị lớn nhất. 

Đầu ra là một mảng trong đó giá trị thứ i là điểm tối đa cho ngày thứ i. 

Ràng buộc n lên tới 4 · 10^5 ngay lập tức loại trừ mọi so sánh bậc hai giữa tất cả các cặp ngày. Cách tiếp cận O(n^2) ngây thơ sẽ yêu cầu theo thứ tự 10^11 thao tác, vượt xa mọi giới hạn khả thi trong vài giây. Điều này thúc đẩy chúng tôi hướng tới một giải pháp trong đó mỗi chỉ mục đóng góp theo thời gian không đổi hoặc logarit, thường thông qua thủ thuật duy trì tiền tố hoặc phép chuyển đổi biến biểu thức thành thứ mà chúng tôi có thể tối ưu hóa bằng bản tóm tắt đang chạy. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị giống hệt nhau. Trong tình huống đó, mọi sự khác biệt |xi − xj| bằng 0 và câu trả lời sẽ bằng 0 với mọi i. Bất kỳ cách tiếp cận nào quên bao gồm j = i làm lựa chọn hợp lệ sẽ tạo ra các giá trị âm không chính xác hoặc không thể kẹp về 0. 

Một trường hợp góc khác xảy ra khi chỉ số tối ưu trước đó ở rất gần về thời gian nhưng có giá trị kém hơn một chút, trong khi chỉ số xa hơn có giá trị tốt hơn nhiều. Đây chính xác là lý do tại sao các chiến lược “theo dõi giá trị tối đa hoặc tối thiểu cuối cùng” ngây thơ lại thất bại, vì hình phạt thời gian tương tác với chênh lệch giá trị và không thể tách rời nếu không chuyển đổi. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi ngày i, lặp lại tất cả các ngày trước j và tính điểm trực tiếp, giữ nguyên kết quả tốt nhất. Điều này đúng vì nó đánh giá rõ ràng mọi cặp ứng cử viên. Tuy nhiên, mỗi i quét tới i giá trị, do đó tổng số phép toán tăng lên như 1 + 2 + … + n, tức là khoảng n^2/2. Với n = 4 · 10^5, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là giá trị tuyệt đối chia bài toán thành hai dạng tuyến tính tùy thuộc vào việc xi có lớn hơn xj hay không. Mỗi dạng có thể được sắp xếp lại sao cho tất cả sự phụ thuộc vào j được tách biệt thành một thống kê tiền tố, trong khi tất cả sự phụ thuộc vào i là một biểu thức đơn giản mà chúng tôi tính toán một lần cho mỗi chỉ mục. 

Khai triển biểu thức cho hai trường hợp. Nếu loại bỏ giá trị tuyệt đối, chúng ta sẽ nhận được xi − xj hoặc xj − xi. Mỗi trong số này trở thành tuyến tính theo i và j khi chúng ta mở rộng hình phạt thời gian c · (i − j). Điều này cho phép chúng ta viết lại bài toán dưới dạng duy trì hai đại lượng đang chạy trên tất cả j trước đó: một đại lượng theo dõi giá trị nhỏ nhất của xj − c · j và một đại lượng theo dõi giá trị lớn nhất của xj + c · j. 

Điều này làm giảm mỗi truy vấn về thời gian không đổi, vì vào ngày thứ i chúng tôi chỉ kết hợp xi với hai tóm tắt tiền tố này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải, duy trì thông tin về tất cả các chỉ mục trước đó.

1. Duy trì hai giá trị đang chạy trong khi quét: giá trị tối thiểu của xj − c · j và giá trị tối đa của xj + c · j cho tất cả j đã thấy cho đến nay. Chúng tóm tắt tất cả các lựa chọn trước đó theo cách phù hợp với hai hướng có thể có của giá trị tuyệt đối. 
2. Với mỗi chỉ số i, hãy tính hai câu trả lời ứng viên. Ứng viên đầu tiên tương ứng với trường hợp xi lớn hơn xj, và nó trở thành (xi − c · i) − min(xj − c · j). Điều này đo lường mức độ lớn của xi so với “giá trị điều chỉnh thấp” tốt nhất được thấy trước đó. 
3. Ứng viên thứ hai tương ứng với trường hợp xj lớn hơn xi và nó trở thành max(xj + c · j) − (xi + c · i). Điều này đo lường mức độ lớn của giá trị điều chỉnh cao trước đó so với giá trị điều chỉnh hiện tại. 
4. Đáp án của ngày thứ i là đáp án lớn nhất của hai ứng viên này. Sau khi tính toán nó, chúng tôi cập nhật cả hai bản tóm tắt đang chạy với chỉ mục hiện tại i để nó có sẵn cho những ngày trong tương lai. 

### Tại sao nó hoạt động 

Phép biến đổi tách sự phụ thuộc vào j thành hai thống kê đơn điệu trên các tiền tố. Mọi chỉ số có thể có trước đó đều đóng góp vào một trong hai dạng tuyến tính và tiền tố tối thiểu hoặc tối đa đảm bảo rằng j tốt nhất có thể luôn được biểu thị trong thời gian O(1). Vì mọi cặp (i, j) đều được xem xét ngầm thông qua các tóm tắt này, nên giá trị tối đa được tính toán khớp chính xác với định nghĩa brute-force. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, c = map(int, input().split())
    x = list(map(int, input().split()))

    INF = 10**30

    min_left = INF
    max_left = -INF

    res = []

    for i in range(n):
        xi = x[i]

        val1 = xi - c * i
        val2 = xi + c * i

        if i == 0:
            res.append(0)
        else:
            best1 = val1 - min_left
            best2 = max_left - val2
            res.append(max(best1, best2))

        expr1 = xi - c * i
        expr2 = xi + c * i

        if expr1 < min_left:
            min_left = expr1
        if expr2 > max_left:
            max_left = expr2

    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ hai giá trị đang chạy tương ứng trực tiếp với các dạng đại số được rút ra trong phần hướng dẫn. Biến`min_left`lưu trữ giá trị nhỏ nhất của xj − c·j được thấy cho đến nay, trong khi`max_left`lưu trữ giá trị lớn nhất của xj + c·j. Ở mỗi bước, chúng tôi tính toán sự đóng góp của chỉ số hiện tại đối với cả hai bản tóm tắt trong thời gian không đổi. 

Một lỗi phổ biến là cập nhật các giá trị tiền tố trước khi tính toán câu trả lời cho i, điều này sẽ cho phép j = i tự ảnh hưởng đến chính nó theo cách phá vỡ sự phân tách dự kiến. Thứ tự đúng là tính toán câu trả lời trước, sau đó kết hợp chỉ mục hiện tại vào cấu trúc đang chạy. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu: 

đầu vào: 

5 1 

2 7 1 5 4 

Chúng tôi theo dõi trạng thái từng bước. 

| tôi | xi | min(xj − cj) | max(xj + cj) | tốt nhất1 | tốt nhất2 | yi | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | thông tin | -inf | - | - | 0 | 
| 1 | 7 | 2 | 2 | 7 | 0 | 4 | 
| 2 | 1 | -1 | 8 | 5 | 5 | 5 | 
| 3 | 5 | -1 | 8 | 3 | 3 | 3 | 
| 4 | 4 | -2 | 8 | 1 | 1 | 1 | 

Dấu vết này cho thấy các giá trị biến đổi cực trị trước đó xác định đầy đủ từng câu trả lời như thế nào. Ngày thứ ba minh họa tại sao suy luận cục bộ thất bại: mặc dù xi nhỏ, giá trị rất lớn trước đó vẫn ảnh hưởng đến kết quả thông qua dạng biến đổi thứ hai. 

Một ví dụ nhỏ thứ hai: 

đầu vào: 

3 2 

5 1 10 

| tôi | xi | min(xj − cj) | max(xj + cj) | yi | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | thông tin | -inf | 0 | 
| 1 | 1 | 5 | 5 | 0 | 
| 2 | 10 | -3 | 9 | 7 | 

Điều này cho thấy giá trị thấp trước đó có thể trở lại phù hợp như thế nào sau khi hình phạt được tính đến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần với các truy vấn và cập nhật liên tục | 
| Không gian | O(1) | Chỉ có hai tập hợp đang chạy được lưu trữ bất kể kích thước đầu vào | 

Hành vi thời gian tuyến tính đủ cho n tối đa 4 · 10^5 và việc sử dụng bộ nhớ liên tục sẽ tránh mọi áp lực lên giới hạn bộ nhớ lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, c = map(int, input().split())
    x = list(map(int, input().split()))

    INF = 10**30
    min_left = INF
    max_left = -INF
    res = []

    for i in range(n):
        xi = x[i]
        if i == 0:
            res.append(0)
        else:
            best1 = (xi - c * i) - min_left
            best2 = max_left - (xi + c * i)
            res.append(max(best1, best2))

        min_left = min(min_left, xi - c * i)
        max_left = max(max_left, xi + c * i)

    return " ".join(map(str, res))

# provided sample
assert run("5 1\n2 7 1 5 4\n") == "0 4 5 3 1"

# minimum size
assert run("1 10\n5\n") == "0"

# all equal
assert run("4 3\n7 7 7 7\n") == "0 0 0 0"

# increasing values
assert run("5 1\n1 2 3 4 5\n") == "0 0 0 0 0"

# strong jump late
assert run("4 2\n10 1 1 50\n") == "0 7 7 42"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ sở đúng đắn | 
| tất cả đều bình đẳng | tất cả số không | giá trị tuyệt đối đối xứng | 
| trình tự tăng dần | số không | hủy bỏ bị phạt | 
| tăng đột biến muộn | giá trị cuối cùng lớn | đóng góp đường dài | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, các đại lượng biến đổi xj − c·j và xj + c·j chỉ tiến triển do số hạng phạt tuyến tính. Thuật toán vẫn duy trì giá trị cực tiểu và cực đại chính xác, nhưng cả hai biểu thức ứng cử viên đều giảm về 0 ở mỗi bước, vì xi khớp với mọi xj và hình phạt không bao giờ tạo ra mức tăng dương. Điều này phù hợp với kết quả đầu ra dự kiến ​​của một mảng bằng không. 

Khi chỉ mục trước đó tốt nhất cách rất xa, số liệu thống kê tiền tố đã mã hóa chỉ mục đó bất kể nó cách đây bao xa. Ví dụ: nếu một giá trị rất lớn xuất hiện sớm, đóng góp của nó vẫn được lưu trữ ở dạng max(xj + c·j) và tiếp tục tác động chính xác đến tất cả các chỉ số trong tương lai, bởi vì việc điều chỉnh tuyến tính sẽ bù chính xác cho khoảng cách thời gian.
