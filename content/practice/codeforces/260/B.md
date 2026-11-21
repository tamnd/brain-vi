---
title: "CF 260B - Lời tiên tri cổ xưa"
description: "Đầu vào là một chuỗi dài chỉ bao gồm các chữ số và ký tự -. Đâu đó bên trong chuỗi này có thể có nhiều chuỗi con trông giống như ngày tháng được viết chính xác là dd-mm-yyyy. Nhiệm vụ của chúng ta là tìm ngày hợp lệ xuất hiện thường xuyên nhất dưới dạng chuỗi con như vậy."
date: "2026-06-04T17:41:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 260
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 158 (Div. 2)"
rating: 1600
weight: 260
solve_time_s: 130
verified: true
draft: false
---

[CF 260B - Lời tiên tri cổ xưa](https://codeforces.com/problemset/problem/260/B) 

**Đánh giá:** 1600 
**Tags:** vũ lực, thực hiện, dây 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào là một chuỗi dài chỉ bao gồm các chữ số và ký tự`-`. Ở đâu đó bên trong chuỗi này có thể có nhiều chuỗi con trông giống như ngày tháng được viết chính xác như`dd-mm-yyyy`. 

Nhiệm vụ của chúng ta là tìm ngày hợp lệ xuất hiện thường xuyên nhất dưới dạng chuỗi con như vậy. Một ngày hợp lệ phải đáp ứng ba điều kiện: 

Đầu tiên, định dạng phải có chính xác hai chữ số cho ngày, hai chữ số cho tháng và bốn chữ số cho năm, được phân tách bằng dấu gạch ngang. 

Thứ hai, năm phải là một trong`2013`,`2014`, hoặc`2015`. 

Thứ ba, ngày phải hợp lệ trong tháng nhất định. Vì không có năm nào là năm nhuận nên tháng 2 có 28 ngày. 

Câu trả lời được đảm bảo tồn tại và duy nhất. 

Độ dài chuỗi tối đa là`10^5`. Chuỗi con đại diện cho một ngày luôn có độ dài`10`, vậy có nhiều nhất khoảng`10^5`vị trí ứng viên để xem xét. Điều này ngay lập tức gợi ý rằng việc quét tất cả các chuỗi con có độ dài 10 có thể là khả thi. Bất kỳ giải pháp nào cố gắng so sánh mọi ứng cử viên với mọi ứng cử viên khác sẽ hướng tới hành vi bậc hai và trở nên quá chậm. 

Phần tinh tế là một chuỗi con có thể trông giống như một ngày tháng về mặt cú pháp trong khi không hợp lệ. Ví dụ:```
31-02-2013
```Điều này phù hợp với mẫu`dd-mm-yyyy`, nhưng tháng 2 không bao giờ có 31 ngày nên không tính được. 

Một lỗi dễ mắc phải khác là chấp nhận ngày không sử dụng chính xác hai chữ số cho ngày và tháng. Coi như:```
1-01-2013
```Đây không phải là trường hợp hợp lệ vì định dạng bắt buộc phải dài chính xác mười ký tự. Chỉ một:```
01-01-2013
```tính. 

Cạm bẫy thứ ba là giới hạn ngày cụ thể theo tháng. Ví dụ:```
31-04-2014
```Tháng 4 chỉ có 30 ngày. Một giải pháp chỉ kiểm tra`1 <= day <= 31`sẽ đếm sai. 

Cuối cùng, sự xuất hiện chồng chéo được cho phép. trong chuỗi```
0012-10-2012-10-2012
```các vị trí bắt đầu khác nhau có thể tạo ra chuỗi con ngày tháng hợp lệ. Chúng ta phải kiểm tra từng cửa sổ có độ dài 10 một cách độc lập. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tạo ra mọi ngày hợp lệ trong khoảng thời gian từ năm 2013 đến năm 2015 và tìm kiếm toàn bộ lời tiên tri cho từng ngày. 

Chỉ có khoảng một nghìn ngày hợp lệ trong phạm vi đó, vì vậy độ chính xác rất dễ hiểu. Đối với mỗi ngày, hãy đếm số lần biểu diễn chuỗi của nó xuất hiện trong lời tiên tri và giữ mức tối đa. 

Vấn đề là thời gian chạy. Lời tiên tri có thể chứa đựng`10^5`nhân vật. Tìm kiếm khoảng 1000 ngày một cách độc lập dẫn đến khoảng`10^8`các thao tác ký tự trong trường hợp xấu nhất, gây tốn kém không cần thiết. 

Cấu trúc của vấn đề gợi ý nên xoay ngược lại quá trình. 

Thay vì hỏi "Mỗi ngày hợp lệ xảy ra bao nhiêu lần?", chúng ta có thể hỏi "Mỗi chuỗi con có độ dài 10 đại diện cho ngày nào?" 

Mỗi lần xuất hiện hợp lệ phải chiếm chính xác mười ký tự:```
dd-mm-yyyy
```chỉ có`n - 9`các cửa sổ như vậy trong toàn bộ chuỗi. Đối với mỗi cửa sổ, chúng tôi có thể kiểm tra xem nó có tạo thành một ngày hợp lệ hay không. Nếu có, chúng tôi sẽ tăng tần suất của ngày đó. 

Điều này biến vấn đề thành một bản quét tuyến tính duy nhất của lời tiên tri. Mỗi vị trí được xử lý một lần, việc kiểm tra tính hợp lệ mất nhiều thời gian và bản đồ băm lưu trữ tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1000·n) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(k) | Đã chấp nhận | 

Đây`k`là số ngày hợp lệ riêng biệt gặp phải, nhiều nhất là số ngày hợp lệ trong khoảng thời gian từ 2013 đến 2015. 

## Hướng dẫn thuật toán 

1. Đọc chuỗi lời tiên tri. 
2. Chuẩn bị một mảng chứa số ngày trong mỗi tháng:```
[31,28,31,30,31,30,31,31,30,31,30,31]
```Vì các năm 2013 đến 2015 không phải là năm nhuận nên tháng 2 luôn có 28 ngày. 
3. Lặp lại mọi vị trí bắt đầu`i`sao cho tồn tại một chuỗi con có độ dài 10. 
4. Trích xuất chuỗi con`s[i:i+10]`. 
5. Kiểm tra xem vị trí 2 và 5 có chứa dấu gạch nối hay không. 

Nếu không, chuỗi con không thể khớp với định dạng được yêu cầu. 
6. Phân tích: 

- ngày từ nhân vật`[0:2]`- tháng từ ký tự`[3:5]`- năm từ ký tự`[6:10]`Nếu phân tích cú pháp không thành công, hãy loại bỏ chuỗi con. 
7. Xác minh ngày tháng: 

- Năm là giữa 2013 và 2015 
- tháng nằm trong khoảng từ 1 đến 12 
- ngày ít nhất là 1 
- ngày không vượt quá số ngày trong tháng đó 
8. Nếu ngày hợp lệ, hãy tăng số đếm của nó trong bản đồ băm bằng cách sử dụng chuỗi con ban đầu làm khóa. 
9. Theo dõi ngày có tần suất cao nhất được thấy cho đến nay. 
10. Sau khi xử lý tất cả các cửa sổ, xuất ra ngày có số lượng tối đa. 

### Tại sao nó hoạt động 

Mỗi lần xuất hiện hợp lệ trong lời tiên tri chiếm chính xác mười ký tự liên tiếp và phải được biểu thị bằng một cửa sổ có độ dài 10 nào đó. Thuật toán kiểm tra mọi cửa sổ như vậy chính xác một lần. 

Một chuỗi con chỉ đóng góp vào bản đồ tần số nếu nó vượt qua tất cả các bước kiểm tra tính hợp lệ, do đó mỗi lần xuất hiện được đếm đều tương ứng với một ngày chính xác. Ngược lại, mọi ngày xuất hiện chính xác sẽ xuất hiện dưới dạng một trong các cửa sổ được kiểm tra và sẽ được tính. 

Do đó, bản đồ tần số lưu trữ số lần xuất hiện chính xác của mỗi ngày hợp lệ. Vì ngày tận thế được xác định là ngày hợp lệ duy nhất với tần suất tối đa nên việc chọn khóa có số lượng lớn nhất sẽ tạo ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    days_in_month = [31, 28, 31, 30, 31, 30,
                     31, 31, 30, 31, 30, 31]

    freq = {}
    best_date = ""
    best_count = 0

    for i in range(len(s) - 9):
        cur = s[i:i + 10]

        if cur[2] != '-' or cur[5] != '-':
            continue

        try:
            day = int(cur[0:2])
            month = int(cur[3:5])
            year = int(cur[6:10])
        except ValueError:
            continue

        if not (2013 <= year <= 2015):
            continue

        if not (1 <= month <= 12):
            continue

        if not (1 <= day <= days_in_month[month - 1]):
            continue

        freq[cur] = freq.get(cur, 0) + 1

        if freq[cur] > best_count:
            best_count = freq[cur]
            best_date = cur

    print(best_date)

if __name__ == "__main__":
    solve()
```Quá trình quét sẽ kiểm tra mọi cửa sổ có độ dài 10 có thể có vì mọi ngày xuất hiện hợp lệ đều phải chiếm chính xác 10 ký tự. 

Việc kiểm tra dấu gạch nối được thực hiện trước khi phân tích số nguyên. Điều này nhanh chóng loại bỏ hầu hết các cửa sổ không hợp lệ và tránh những chuyển đổi không cần thiết. 

Việc kiểm tra tính hợp lệ của tháng diễn ra trước khi lập chỉ mục`days_in_month[month - 1]`. Việc đảo ngược thứ tự này sẽ có nguy cơ truy cập vào chỉ mục không hợp lệ khi tháng nằm ngoài phạm vi`1..12`. 

Chuỗi con ban đầu được sử dụng làm khóa từ điển. Vì ngày luôn được lưu trữ theo kiểu chuẩn`dd-mm-yyyy`định dạng, ngày giống hệt nhau luôn tạo ra các khóa giống nhau. 

Tốc độ chạy tối đa được cập nhật trong quá trình quét, tránh lần thứ hai vượt qua bản đồ tần số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
777-444---21-12-2013-12-2013-12-2013---444-777
```Các cửa sổ hợp lệ có liên quan: 

| Cửa sổ | Có hiệu lực? | Đếm Sau Cập Nhật | 
| --- | --- | --- | 
| 21-12-2013 | Có | 1 | 
| 12-13-2013 | Có | 1 | 
| 20-12-1312 | Không | - | 
| 12-13-2013 | Có | 2 | 
| 12-13-2013 | Có | 3 | 

Tần số cuối cùng: 

| Ngày | Tần số | 
| --- | --- | 
| 21-12-2013 | 1 | 
| 12-13-2013 | 3 | 

Đầu ra:```
13-12-2013
```Ví dụ này cho thấy tại sao mọi cửa sổ có độ dài 10 phải được kiểm tra. Ngày trúng thưởng xuất hiện nhiều lần do các vị trí chồng chéo bên trong chuỗi lớn hơn. 

### Ví dụ 2 

đầu vào:```
01-01-201331-02-201301-01-2013
```Dấu vết: 

| Cửa sổ | Có hiệu lực? | Tần số | 
| --- | --- | --- | 
| 01-01-2013 | Có | 1 | 
| 31-02-2013 | Không | - | 
| 01-01-2013 | Có | 2 | 

Đầu ra:```
01-01-2013
```Ví dụ này cho thấy tầm quan trọng của việc xác nhận giới hạn ngày cụ thể theo tháng. Chuỗi con ở giữa có hình dạng đúng nhưng không phải là ngày dương lịch thực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cửa sổ có độ dài 10 được xử lý một lần | 
| Không gian | O(k) | Bản đồ tần số lưu trữ ngày hợp lệ riêng biệt | 

Độ dài chuỗi tối đa là`10^5`, đại khái thế`10^5`cửa sổ được kiểm tra. Mỗi cửa sổ chỉ yêu cầu phân tích cú pháp và xác thực theo thời gian liên tục. Giải pháp phù hợp thoải mái trong thời gian giới hạn và sử dụng rất ít bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    def solve():
        input = sys.stdin.readline
        s = input().strip()

        days = [31, 28, 31, 30, 31, 30,
                31, 31, 30, 31, 30, 31]

        freq = {}
        best_date = ""
        best_count = 0

        for i in range(len(s) - 9):
            cur = s[i:i + 10]

            if cur[2] != '-' or cur[5] != '-':
                continue

            try:
                d = int(cur[:2])
                m = int(cur[3:5])
                y = int(cur[6:])
            except ValueError:
                continue

            if not (2013 <= y <= 2015):
                continue
            if not (1 <= m <= 12):
                continue
            if not (1 <= d <= days[m - 1]):
                continue

            freq[cur] = freq.get(cur, 0) + 1

            if freq[cur] > best_count:
                best_count = freq[cur]
                best_date = cur

        print(best_date)

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue().strip()

# provided sample
assert run(
    "777-444---21-12-2013-12-2013-12-2013---444-777\n"
) == "13-12-2013", "sample"

# single valid date
assert run("01-01-2013\n") == "01-01-2013", "minimum valid case"

# invalid date between two valid occurrences
assert run(
    "01-01-201331-02-201301-01-2013\n"
) == "01-01-2013", "reject invalid February date"

# month boundary
assert run(
    "30-04-201431-04-201430-04-2014\n"
) == "30-04-2014", "April has only 30 days"

# year boundary
assert run(
    "31-12-201231-12-201331-12-2013\n"
) == "31-12-2013", "reject year outside range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`01-01-2013`|`01-01-2013`| Đầu vào hợp lệ có ý nghĩa nhỏ nhất | 
|`01-01-201331-02-201301-01-2013`|`01-01-2013`| Ngày tháng 2 không hợp lệ bị bỏ qua | 
|`30-04-201431-04-201430-04-2014`|`30-04-2014`| Giới hạn ngày cụ thể theo tháng | 
|`31-12-201231-12-201331-12-2013`|`31-12-2013`| Xác thực phạm vi năm | 

## Vỏ cạnh 

Hãy xem xét:```
31-02-2013
```Chuỗi con có đúng`dd-mm-yyyy`kết cấu. Thuật toán phân tích ngày`31`, tháng`2`, và năm`2013`. Tháng có giá trị, nhưng tháng hai chỉ cho phép`28`ngày. điều kiện```
day <= days_in_month[month - 1]
```không thành công nên chuỗi con bị loại bỏ và không bao giờ được tính. 

Coi như:```
31-04-2014
```Tháng Tư có`30`ngày. Thuật toán tính toán`days_in_month[3] = 30`, phát hiện ra rằng`31 > 30`, và từ chối ngày tháng. Điều này ngăn việc đếm ngày theo lịch đúng về mặt cú pháp nhưng không thể thực hiện được. 

Coi như:```
1-01-2013
```Chuỗi này chỉ dài chín ký tự. Vì thuật toán chỉ kiểm tra các cửa sổ có độ dài 10 nên nó không bao giờ coi đây là ngày ứng cử. Định dạng bắt buộc yêu cầu chính xác hai chữ số cho cả ngày và tháng. 

Coi như:```
13-12-2013-12-2013
```Chuỗi con`13-12-2013`xuất hiện ở đầu và một lần xuất hiện khác bắt đầu sau do chồng chéo. Bởi vì thuật toán kiểm tra mọi vị trí bắt đầu một cách độc lập nên cả hai lần xuất hiện đều được tính chính xác. Các trận đấu chồng chéo không yêu cầu xử lý đặc biệt.
