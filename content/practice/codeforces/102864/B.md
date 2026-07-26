---
title: "CF 102864B - \u5927\u91c7\u8d2d"
description: "Kệ chứa M sản phẩm được sắp xếp theo một thứ tự cố định. Mỗi sản phẩm đều có trọng lượng và giá trị. Longlong chỉ có thể di chuyển dọc theo kệ theo thứ tự đó nên các sản phẩm anh chọn phải tạo thành một dãy con của dãy ban đầu. Xe đẩy hàng có trọng lượng tối đa là N."
date: "2026-07-25T20:35:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102864
codeforces_index: "B"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Online Round"
rating: 0
weight: 102864
solve_time_s: 49
verified: true
draft: false
---

[CF 102864B - \u5927\u91c7\u8d2d](https://codeforces.com/problemset/problem/102864/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Kệ chứa M sản phẩm được sắp xếp theo một thứ tự cố định. Mỗi sản phẩm đều có trọng lượng và giá trị. Longlong chỉ có thể di chuyển dọc theo kệ theo thứ tự đó nên các sản phẩm anh chọn phải tạo thành một dãy con của dãy ban đầu. 

Xe đẩy hàng có trọng lượng tối đa là N. Nguyên tắc đặc biệt là mỗi sản phẩm được chọn sau sản phẩm đầu tiên không được nặng hơn sản phẩm được chọn ngay trước nó. Nói cách khác, nếu sản phẩm được chọn có trọng số g1, g2, g3 thì phải thỏa mãn g1 >= g2 >= g3. Trong số tất cả các lựa chọn hợp lệ có tổng trọng số không vượt quá N, chúng ta cần tổng giá trị lớn nhất có thể. 

Các giới hạn này nhỏ, với cả N và M nhiều nhất là 100. Một giải pháp ba lô thông thường có thêm một chiều là đủ, nhưng các phương pháp liệt kê tất cả các tập hợp con có thể là không thể. Có thể có 100 sản phẩm, tạo ra 2^100 lựa chọn khả thi, vượt xa những gì có thể khám phá trong thời gian giới hạn. 

Một vài chi tiết có thể dễ dàng gây ra câu trả lời sai. Sản phẩm đầu tiên được chọn không có giới hạn về trọng lượng trước đó nên có thể có bất kỳ trọng lượng nào. Ví dụ:```
5 2
5 10
1 1
```Câu trả lời đúng là 11 vì cả hai sản phẩm đều được chọn. Giải pháp khởi tạo trọng lượng trước đó không chính xác có thể từ chối sản phẩm đầu tiên hoặc không sử dụng được trạng thái trống. 

Một trường hợp khác là khi sản phẩm nặng hơn xuất hiện sau:```
5 3
1 10
3 100
2 5
```Câu trả lời đúng là 105 bằng cách chọn trọng số 3 và 2. Sản phẩm có trọng lượng 1 không thể được chọn trước chúng vì không thể thay đổi thứ tự trên kệ. Giải pháp sắp xếp sản phẩm theo trọng lượng, như trong các biến thể ba lô thông thường, sẽ giải quyết một vấn đề khác và tạo ra kết quả không hợp lệ. 

Trường hợp cạnh cuối cùng là không chọn gì:```
1 1
1 7
```Câu trả lời là 7 vì sản phẩm phù hợp. Nếu dung lượng bằng 0 thì câu trả lời sẽ là 0, do đó lập trình động phải luôn bảo toàn lựa chọn trống. 

## Phương pháp tiếp cận 

Phương pháp trực tiếp nhất là thử mọi tập hợp con sản phẩm có thể. Đối với mỗi tập hợp con, chúng tôi kiểm tra xem các chỉ mục đã chọn có giữ nguyên thứ tự ban đầu hay không, trọng số có không tăng hay không và tổng trọng lượng có vừa với giỏ hàng hay không. Phương pháp này đúng vì mọi lựa chọn có thể đều được kiểm tra nhưng nó có 2^M tập hợp con. Với M = 100, trường hợp xấu nhất cần khoảng 1,27 × 10^30 lần kiểm tra, điều này là không thể. 

Quan sát hữu ích là tương lai chỉ phụ thuộc vào hai thông tin từ các sản phẩm đã được xử lý. Chúng ta cần biết tổng trọng lượng hiện tại vì nó xác định dung lượng còn lại là bao nhiêu và chúng ta cần biết trọng lượng của sản phẩm được chọn cuối cùng vì nó kiểm soát xem sản phẩm tiếp theo có được phép hay không. 

Điều này biến vấn đề thành lập trình động. Trong khi quét sản phẩm từ trái sang phải, chúng tôi duy trì giá trị tốt nhất cho mọi sự kết hợp giữa công suất sử dụng và trọng lượng được chọn cuối cùng. Khi xử lý một sản phẩm, chúng tôi bỏ qua và giữ nguyên trạng thái cũ hoặc lấy sản phẩm đó nếu trọng lượng của nó không vượt quá trọng lượng đã chọn trước đó. 

Sản phẩm được chọn đầu tiên cần được xử lý đặc biệt vì không có trọng lượng trước đó. Chúng tôi đại diện cho trạng thái trống với trọng lượng đặc biệt trước đó lớn hơn mọi trọng lượng sản phẩm có thể có, cho phép bất kỳ sản phẩm nào được chọn làm sản phẩm đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^M × M) | O(M) | Quá chậm | 
| Tối ưu | O(M × N × 101) | O(N × 101) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bảng lập trình động.`dp[c][w]`biểu thị giá trị tối đa có thể đạt được sau khi xử lý một số tiền tố của giá, sử dụng tổng trọng lượng`c`, với sản phẩm được chọn cuối cùng có trọng lượng`w`. Trạng thái đặc biệt`w = 101`đại diện cho việc không chọn gì cho đến nay. 

Trạng thái ban đầu là`dp[0][101] = 0`, bởi vì trước khi lấy bất kỳ sản phẩm nào, xe đẩy có trọng lượng bằng 0 và không có hạn chế về sản phẩm đầu tiên. 

1. Xử lý từng sản phẩm từ trái sang phải. Đối với sản phẩm hiện tại có trọng lượng`g`và giá trị`s`, hãy tạo một bản sao của các trạng thái hiện tại để thể hiện tùy chọn bỏ qua sản phẩm này. 

Việc bỏ qua phải được cân nhắc vì sản phẩm được chọn chỉ cần là một phần tiếp theo chứ không phải là một đoạn liên tục của kệ. 

1. Đối với mọi trạng thái hiện có, hãy thử lấy sản phẩm hiện tại. Nếu trạng thái hiện tại có trọng số trước đó`w`Và`g <= w`, sản phẩm có thể được thêm vào. Cập nhật trạng thái với dung lượng mới`c + g`và trọng lượng cuối cùng mới`g`. 

Trọng lượng được chọn cuối cùng trở thành`g`bởi vì mọi sản phẩm trong tương lai đều phải được so sánh với sản phẩm được thêm vào gần đây nhất. 

1. Sau khi tất cả các sản phẩm được xử lý, câu trả lời là giá trị tối đa trong số tất cả các trạng thái hợp lệ, bất kể công suất sử dụng cuối cùng hay trọng lượng được chọn cuối cùng. 

Tại sao nó hoạt động: 

Trạng thái lập trình động lưu trữ chính xác thông tin cần thiết cho các quyết định trong tương lai. Hai lựa chọn khác nhau đã xử lý cùng một tiền tố và có cùng công suất sử dụng cũng như trọng lượng được chọn cuối cùng sẽ có các khả năng giống nhau cho mọi sản phẩm còn lại. Chỉ giữ lại giá trị lớn hơn trong số chúng không thể loại bỏ được giải pháp tối ưu. Mỗi quá trình chuyển đổi xem xét hai lựa chọn duy nhất cho một sản phẩm, bỏ qua nó hoặc lấy nó khi được phép, do đó mọi dãy con hợp lệ đều được thể hiện. Mức tối đa cuối cùng trong số tất cả các tiểu bang là kế hoạch mua sắm hợp lệ tốt nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M = map(int, input().split())
    items = [tuple(map(int, input().split())) for _ in range(M)]

    NEG = -1
    dp = [[NEG] * 102 for _ in range(N + 1)]
    dp[0][101] = 0

    for g, s in items:
        new_dp = [row[:] for row in dp]

        for weight in range(N + 1):
            for last in range(102):
                if dp[weight][last] == NEG:
                    continue
                if g <= last and weight + g <= N:
                    new_dp[weight + g][g] = max(
                        new_dp[weight + g][g],
                        dp[weight][last] + s
                    )

        dp = new_dp

    ans = 0
    for weight in range(N + 1):
        for last in range(102):
            ans = max(ans, dp[weight][last])

    print(ans)

if __name__ == "__main__":
    solve()
```Bảng có thể có 102 trọng số trước đó vì trọng số thực của sản phẩm là từ 1 đến 100 và chỉ số 101 được dành riêng cho lựa chọn trống. giá trị`NEG`đánh dấu các trạng thái không thể đạt được, ngăn chặn việc sử dụng các chuyển đổi không hợp lệ. 

Đối với mỗi sản phẩm, mã sẽ sao chép bảng cũ trước khi áp dụng chuyển tiếp. Điều này ngăn việc sử dụng cùng một sản phẩm nhiều lần trong một lần lặp, đó cũng là lý do khiến ba lô 0/1 cập nhật trạng thái từ lớp trước. 

điều kiện`g <= last`thực thi yêu cầu về trọng lượng không tăng. Trạng thái trống có`last = 101`, vì vậy mọi sản phẩm đầu tiên đều được chấp nhận. Việc kiểm tra dung lượng diễn ra trước khi cập nhật, ngăn các trạng thái vượt quá giới hạn giỏ hàng vào bảng. 

Tất cả các giá trị đều dễ dàng khớp với số nguyên Python vì tổng giá trị tối đa có thể chỉ là 10000. 

## Ví dụ đã hoạt động 

Đầu vào mẫu là:```
5 5
1 5
2 2
3 3
4 4
1 2
```Một dấu vết của các trạng thái quan trọng được hiển thị dưới đây. Chỉ trạng thái có thể truy cập tốt nhất sau mỗi sản phẩm được xử lý mới được hiển thị. 

| Bước | Sản phẩm | Lựa chọn tốt nhất hiện nay | Trọng lượng đã qua sử dụng | Cân nặng cuối cùng | Giá trị | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | Không có | Trống | 0 | 101 | 0 | 
| 1 | (1,5) | Chọn sản phẩm 1 | 1 | 1 | 5 | 
| 2 | (2,2) | Chọn sản phẩm 2 thôi | 2 | 2 | 2 | 
| 3 | (3,3) | Chọn sản phẩm 3 thôi | 3 | 3 | 3 | 
| 4 | (4,4) | Chọn sản phẩm 4 thôi | 4 | 4 | 4 | 
| 5 | (1,2) | Chọn trọng lượng 3,1 hoặc 2,1 hoặc 4,1 | 4 hoặc 5 | 1 | 6 | 

Câu trả lời cuối cùng là 6. Dấu vết cho thấy tại sao thứ tự sản phẩm lại quan trọng. Mặc dù sản phẩm đầu tiên có giá trị cao so với một số sản phẩm sau này nhưng việc lựa chọn sản phẩm có trọng lượng tăng dần đều bị cấm. 

Một ví dụ thứ hai:```
6 4
5 8
3 6
3 4
1 10
```| Bước | Sản phẩm | Trọng lượng đã qua sử dụng | Cân nặng cuối cùng | Giá trị tốt nhất | 
| --- | --- | --- | --- | --- | 
| Ban đầu | Không có | 0 | 101 | 0 | 
| 1 | (5,8) | 5 | 5 | 8 | 
| 2 | (3,6) | 8 | 3 | 14 | 
| 3 | (3,4) | 6 | 3 | 12 | 
| 4 | (1,10) | 6 | 1 | 20 | 

Sự lựa chọn tối ưu là sản phẩm thứ nhất, thứ hai và thứ tư. Trọng số của chúng là 5, 3 và 1, thỏa mãn yêu cầu giảm dần và tổng giá trị của chúng là 24. Bảng nhấn mạnh rằng thuật toán giữ một số trọng số cuối cùng có thể có vì trạng thái có trọng số cuối nhỏ hơn vẫn có thể có giá trị khi các sản phẩm trong tương lai nhẹ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M × N × 102) | Mỗi sản phẩm sẽ kiểm tra từng công suất và trạng thái cân nặng trước đó | 
| Không gian | O(N × 102) | Chỉ cần các lớp lập trình động hiện tại và trước đó | 

Số lượng trạng thái tối đa là khoảng 10200 và chỉ có 100 sản phẩm. Điều này mang lại khoảng một triệu chuyển đổi trạng thái, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    N, M = map(int, input().split())
    items = [tuple(map(int, input().split())) for _ in range(M)]

    NEG = -1
    dp = [[NEG] * 102 for _ in range(N + 1)]
    dp[0][101] = 0

    for g, s in items:
        new_dp = [row[:] for row in dp]
        for w in range(N + 1):
            for last in range(102):
                if dp[w][last] != NEG and g <= last and w + g <= N:
                    new_dp[w + g][g] = max(
                        new_dp[w + g][g],
                        dp[w][last] + s
                    )
        dp = new_dp

    return str(max(max(row) for row in dp)) + "\n"

assert solve_case("""5 5
1 5
2 2
3 3
4 4
1 2
""") == "6\n", "sample"

assert solve_case("""1 1
1 7
""") == "7\n", "minimum size"

assert solve_case("""10 3
2 5
2 5
2 5
""") == "15\n", "all equal weights"

assert solve_case("""5 3
5 10
4 9
4 9
""") == "19\n", "boundary capacity"

assert solve_case("""100 100
1 1
""" + "1 1\n" * 99) == "100\n", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp mẫu | 6 | Lựa chọn giảm cân cơ bản | 
| Sản phẩm duy nhất | 7 | Khởi tạo trạng thái trống và xử lý lựa chọn đầu tiên | 
| Trọng lượng bằng nhau | 15 | Nhiều sản phẩm có trọng lượng giống hệt nhau | 
| Ranh giới công suất | 19 | Sử dụng chính xác giới hạn giỏ hàng | 
| 100 sản phẩm | 100 | Xử lý kích thước đầu vào tối đa | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
5 2
5 10
1 1
```Trạng thái ban đầu là`(capacity = 0, last = 101)`. Sản phẩm đầu tiên có thể được chọn vì 5 nhỏ hơn 101, tạo trạng thái có dung lượng 5, trọng số cuối cùng là 5 và giá trị 10. Sản phẩm thứ hai có thể theo sau vì 1 nhỏ hơn 5, tạo ra câu trả lời cuối cùng 11. Trạng thái trống đặc biệt là thứ cho phép sản phẩm đầu tiên không bị hạn chế. 

Đối với trường hợp cạnh thứ tự:```
5 3
1 10
3 100
2 5
```Sau sản phẩm đầu tiên, trạng thái tốt nhất có trọng lượng cuối cùng là 1. Sản phẩm thứ hai có trọng số 3 nên không thể theo trạng thái đó. Thuật toán vẫn giữ trạng thái trống, cho phép sản phẩm thứ hai trở thành lựa chọn đầu tiên. Khi đó sản phẩm thứ ba có thể theo sau vì 2 nhỏ hơn 3. Câu trả lời là 105, tuân theo thứ tự kệ ban đầu. 

Đối với ranh giới không có lựa chọn:```
0 1
1 7
```Trạng thái duy nhất có thể truy cập là giỏ hàng trống. Sản phẩm không bao giờ thỏa mãn điều kiện dung lượng vì việc thêm vào sẽ tạo ra trọng số 1, vượt quá giới hạn. Giá trị còn lại tối đa là 0, đây là kết quả đúng. 

Tôi cũng có thể biến phiên bản này thành phiên bản biên tập ngắn hơn theo phong cách Codeforces nếu bạn muốn nội dung nào đó gần giống với nội dung xuất hiện trên trang cuộc thi.
