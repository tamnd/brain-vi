---
title: "CF 105859G - Câu lạc bộ Pizza"
description: "Vấn đề mô tả một sinh viên chọn các cuộc họp câu lạc bộ để tham dự. Mỗi câu lạc bộ có một giờ họp và phát một số lát pizza nhất định. Hai câu lạc bộ vào cùng một giờ không thể cùng tham dự vì các cuộc họp trùng nhau."
date: "2026-06-25T14:41:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105859
codeforces_index: "G"
codeforces_contest_name: "Mines HSPC 2025 Open Division"
rating: 0
weight: 105859
solve_time_s: 37
verified: true
draft: false
---

[CF 105859G - Club Pizza](https://codeforces.com/problemset/problem/105859/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một sinh viên chọn các cuộc họp câu lạc bộ để tham dự. Mỗi câu lạc bộ có một giờ họp và phát một số lát pizza nhất định. Hai câu lạc bộ vào cùng một giờ không thể cùng tham dự vì các cuộc họp trùng nhau. Học sinh cũng có số lát tối đa có thể ăn trước khi no. Mục tiêu là chọn một nhóm câu lạc bộ có thể tối đa hóa số lượng câu lạc bộ tham dự trong khi vẫn giữ tổng số bánh pizza trong giới hạn. 

Dữ liệu đầu vào cung cấp số lượng câu lạc bộ, sức chứa pizza và sau đó là một bản ghi cho mỗi câu lạc bộ chứa giờ họp và số lượng pizza. Kết quả đầu ra là số lượng câu lạc bộ lớn nhất có thể được chọn. 

Số lượng câu lạc bộ có thể lên tới 1000. Một giải pháp thử mọi tập hợp con các câu lạc bộ sẽ yêu cầu kiểm tra tối đa$2^{1000}$khả năng, điều đó hoàn toàn không thể xảy ra. Tuy nhiên, số giờ chỉ có 24 nên chiều thời gian rất nhỏ. Điều này gợi ý rõ ràng rằng nhà nước nên dựa trên số giờ thay vì các câu lạc bộ riêng lẻ. Một giải pháp lập trình động với khoảng$24 \times c$trạng thái khả thi khi công suất pizza tối đa$10^6$, vì số lượng hoạt động chỉ ở mức vài chục triệu. 

Có một số sai lầm dễ dàng. Một là quên quy tắc chồng chéo. Ví dụ:```
3 10
5 2
5 3
6 5
```Đầu ra đúng là:```
2
```Một cách tiếp cận tham lam là chọn chiếc bánh pizza rẻ nhất trước tiên có thể khiến cả hai câu lạc bộ vào giờ thứ 5 và coi họ là hai lựa chọn, nhưng điều đó không hợp lệ vì chỉ có thể tham dự một cuộc họp vào giờ thứ 5. 

Một vấn đề khác là xử lý không đúng cách các câu lạc bộ không có pizza. Coi như:```
2 0
1 0
2 1
```Đầu ra đúng là:```
1
```Câu lạc bộ thứ hai không thể tham gia vì học sinh không còn đủ năng lực, nhưng câu lạc bộ đầu tiên miễn phí và phải được tính. 

Trường hợp cuối cùng là khi một số câu lạc bộ chia sẻ một giờ và câu lạc bộ tốt nhất không phải là câu lạc bộ có chiếc bánh pizza nhỏ nhất. Ví dụ:```
3 5
7 5
7 1
8 4
```Đầu ra đúng là:```
2
```Chọn câu lạc bộ một lát vào giờ thứ 7 cũng cho phép tham dự giờ thứ 8. Một phương pháp xử lý các câu lạc bộ một cách độc lập có thể bỏ lỡ sự tương tác này. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử mọi bộ sưu tập gậy có thể. Đối với mỗi tập hợp con, chúng tôi kiểm tra xem liệu có hai câu lạc bộ được chọn nào có cùng giờ hoạt động hay không và liệu tổng số pizza có nằm trong giới hạn hay không. Nếu hợp lệ, chúng tôi sẽ giữ lại số lượng câu lạc bộ được chọn tối đa. Điều này đúng vì mọi câu trả lời có thể đều xuất hiện trong số các tập hợp con. Vấn đề là có$2^n$các tập hợp con, điều này trở nên không thể thực hiện được ngay cả đối với vài chục câu lạc bộ, trong khi ở đây$n$có thể là 1000 

Quan sát chính là việc hạn chế các cuộc họp chồng chéo chỉ phụ thuộc vào 24 giờ có thể. Trong mỗi giờ, chúng ta chỉ cần quyết định sẽ tham gia câu lạc bộ nào, nếu có. Thứ tự của giờ xử lý không thành vấn đề vì việc chọn câu lạc bộ vào một giờ không bao giờ ảnh hưởng đến các lựa chọn có sẵn vào giờ khác, ngoại trừ thông qua tổng số bánh pizza được tiêu thụ. 

Điều này biến vấn đề thành một chương trình động kiểu ba lô. Chúng tôi xử lý từng giờ một. Trạng thái biểu thị số lượng pizza đã được sử dụng sau khi xem xét một số tiền tố giờ và lưu trữ số lượng câu lạc bộ tối đa có thể đạt được với số tiền đó. Đối với mỗi giờ, chúng tôi bỏ qua giờ đó hoặc chọn một trong các câu lạc bộ diễn ra trong thời gian đó. 

Tổng số câu lạc bộ là 1000 nhưng chỉ có 24 nhóm câu lạc bộ. Trong mỗi giờ, chúng tôi có thể thử tất cả các câu lạc bộ vào giờ đó và tất cả số lượng pizza có thể có, đưa ra một giới hạn thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| DP tối ưu |$O(24 \times n \times c)$trong trường hợp xấu nhất |$O(c)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm tất cả các câu lạc bộ theo giờ họp. Mỗi giờ sẽ có một danh sách các câu lạc bộ có thể được lựa chọn. 
2. Tạo một mảng lập trình động trong đó`dp[x]`lưu trữ số lượng câu lạc bộ tối đa có thể tham dự bằng cách sử dụng chính xác`x`những lát bánh pizza sau khi chế biến vài giờ. Ban đầu, chỉ có thể không sử dụng bánh pizza nào, vì vậy`dp[0] = 0`. 
3. Xử lý 24 giờ một lần. Đối với giờ hiện tại, hãy tạo một mảng mới bắt đầu như trạng thái trước đó. Điều này thể hiện sự lựa chọn không tham gia câu lạc bộ nào vào giờ này. 
4. Hãy thử mọi câu lạc bộ vào giờ hiện tại. Đối với mỗi số tiền pizza trước đó`x`, nếu việc thêm pizza của câu lạc bộ này không vượt quá sức chứa, hãy cập nhật trạng thái mới với thêm một câu lạc bộ tham dự. 
5. Thay thế trạng thái cũ bằng trạng thái mới và tiếp tục sang giờ tiếp theo. 
6. Sau khi tất cả số giờ được xử lý, giá trị tối đa được lưu ở bất kỳ đâu trong mảng DP là câu trả lời, bởi vì học sinh có thể còn lại số lượng pizza còn sót lại cuối cùng. 

Lý do điều này có tác dụng là vì sau khi xử lý vài giờ, thông tin duy nhất cần thiết về quá khứ là số lượng pizza đã được tiêu thụ và số lượng câu lạc bộ tối đa thu được. Những lựa chọn chính xác đã tạo ra trạng thái đó không còn quan trọng nữa. Mỗi lịch trình hợp lệ tương ứng với một lộ trình trong suốt quá trình chuyển đổi giờ và mọi chuyển tiếp đều tuân theo quy tắc rằng chỉ có thể chọn một câu lạc bộ từ mỗi giờ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, c = map(int, input().split())

    hours = [[] for _ in range(24)]

    for _ in range(n):
        t, p = map(int, input().split())
        if p <= c:
            hours[t].append(p)

    dp = [-10**9] * (c + 1)
    dp[0] = 0

    for clubs in hours:
        ndp = dp[:]

        for pizza in clubs:
            for used in range(c - pizza + 1):
                if dp[used] != -10**9:
                    ndp[used + pizza] = max(
                        ndp[used + pizza],
                        dp[used] + 1
                    )

        dp = ndp

    print(max(dp))

if __name__ == "__main__":
    solve()
```Đầu vào được nhóm ngay lập tức theo giờ. Các câu lạc bộ có nhiều pizza hơn tổng sức chứa sẽ bị bỏ qua vì chúng không bao giờ có thể xuất hiện trong một câu trả lời hợp lệ. 

Mảng DP sử dụng giá trị âm không thể cho các trạng thái không thể truy cập được. Điều này tránh việc vô tình mở rộng một trạng thái không thể tồn tại. 

Cứ mỗi giờ, sao chép vào`ndp`là cần thiết. Nếu cập nhật được thực hiện trực tiếp tới`dp`, việc lấy một câu lạc bộ có thể ảnh hưởng đến các lựa chọn sau này trong cùng một giờ, cho phép tính nhiều câu lạc bộ trong một giờ. Mảng riêng biệt thể hiện quy tắc mỗi giờ đóng góp bằng 0 hoặc một câu lạc bộ. 

Vòng lặp kết thúc`used`dừng lại ở`c - pizza`, điều này ngăn cản việc lập chỉ mục vượt quá khả năng. Do dung lượng tối đa lớn nhưng số giờ cố định nên việc sử dụng bộ nhớ vẫn ở mức có thể quản lý được. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 6
18 3
19 2
20 1
```Sự phát triển của DP có thể được xem như sau. 

| Giờ xử lý | Lựa chọn có sẵn | Câu lạc bộ tốt nhất được tính bằng pizza đã qua sử dụng | 
| --- | --- | --- | 
| Bắt đầu | không | pizza 0 tặng 0 câu lạc bộ | 
| 18 | 3 lát | pizza 3 tặng 1 câu lạc bộ | 
| 19 | 2 lát | pizza 5 tặng 2 câu lạc bộ | 
| 20 | 1 lát | pizza 6 tặng 3 câu lạc bộ | 

Trạng thái cuối cùng sử dụng tất cả sáu lát cắt và tham dự cả ba cuộc họp. 

Đối với mẫu thứ hai:```
3 3
18 3
19 2
20 1
```| Giờ xử lý | Lựa chọn có sẵn | Câu lạc bộ tốt nhất được tính bằng pizza đã qua sử dụng | 
| --- | --- | --- | 
| Bắt đầu | không | pizza 0 tặng 0 câu lạc bộ | 
| 18 | 3 lát | pizza 3 tặng 1 câu lạc bộ | 
| 19 | 2 lát | pizza 2 tặng 1 câu lạc bộ | 
| 20 | 1 lát | pizza 3 tặng 2 câu lạc bộ | 

Học sinh không thể mang theo câu lạc bộ ba lát và hai lát cùng nhau vì sẽ vượt quá sức chứa. Sự lựa chọn tốt nhất là gậy hai lát và gậy một lát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(24 \times n \times c)$| Mỗi giờ xử lý các câu lạc bộ của mình dựa trên tất cả số lượng pizza có thể có | 
| Không gian |$O(c)$| Chỉ các mảng DP trước đó và hiện tại mới được lưu trữ | 

Số giờ ít là yếu tố then chốt. Ngay cả với số lượng câu lạc bộ tối đa, số lần chuyển đổi DP vẫn bị giới hạn ở mức giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    sys.stdin = old

    if not data:
        return ""

    it = iter(data)
    n = int(next(it))
    c = int(next(it))

    hours = [[] for _ in range(24)]

    for _ in range(n):
        t = int(next(it))
        p = int(next(it))
        if p <= c:
            hours[t].append(p)

    dp = [-10**9] * (c + 1)
    dp[0] = 0

    for clubs in hours:
        ndp = dp[:]
        for pizza in clubs:
            for used in range(c - pizza + 1):
                if dp[used] >= 0:
                    ndp[used + pizza] = max(ndp[used + pizza], dp[used] + 1)
        dp = ndp

    return str(max(dp))

assert solve("""3 6
18 3
19 2
20 1
""") == "3"

assert solve("""3 3
18 3
19 2
20 1
""") == "2"

assert solve("""1 0
5 0
""") == "1"

assert solve("""3 5
7 5
7 1
8 4
""") == "2"

assert solve("""4 10
1 4
1 4
2 6
3 0
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba câu lạc bộ vào những giờ khác nhau với đủ pizza | 3 | Lựa chọn cơ bản theo giờ | 
| Công suất quá nhỏ cho tất cả các câu lạc bộ | 2 | Xử lý giới hạn pizza | 
| Câu lạc bộ pizza không | 1 | Lựa chọn câu lạc bộ miễn phí | 
| Nhiều câu lạc bộ trong một giờ | 2 | Xử lý chồng chéo đúng cách | 
| Các lựa chọn thay thế cùng giờ | 2 | Lựa chọn kết hợp giờ tốt hơn | 

## Vỏ cạnh 

Đối với trường hợp trùng giờ:```
3 10
5 2
5 3
6 5
```Thuật toán tạo một nhóm cho giờ thứ 5 chứa hai lựa chọn có thể có. Khi xử lý giờ thứ 5, cả hai lựa chọn đều được xem xét, nhưng chỉ một lần chuyển đổi có thể tồn tại trong giờ tiếp theo vì giờ tiếp theo bắt đầu từ trạng thái không thay đổi trước đó. Trạng thái tốt nhất cuối cùng lấy một câu lạc bộ từ giờ thứ 5 và câu lạc bộ từ giờ thứ 6, tạo ra 2. 

Đối với trường hợp không có pizza:```
2 0
1 0
2 1
```Gậy có một lát sẽ bị loại bỏ trước DP vì nó không vừa. Câu lạc bộ không có lát tiếp tục quá trình chuyển đổi từ pizza 0 sang pizza 0 với một câu lạc bộ tham dự nữa, vì vậy câu trả lời trở thành 1. 

Đối với trường hợp pizza rẻ nhất thôi vẫn chưa đủ:```
3 5
7 5
7 1
8 4
```Giờ xử lý 7 tạo ra các trạng thái để lấy 5 lát hoặc 1 lát. Giờ sau chỉ có thể kết hợp với trạng thái 1 lát, tạo ra tổng cộng 5 lát và 2 gậy. Đây chính xác là quyết định mà DP muốn bảo vệ.
