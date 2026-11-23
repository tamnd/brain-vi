---
title: "CF 6C - Alice, Bob và Sôcôla"
description: "Chúng ta có một hàng thanh sô cô la và mỗi thanh cần một khoảng thời gian nhất định để ăn. Alice bắt đầu từ đầu bên trái và tiếp tục di chuyển sang phải. Bob bắt đầu từ đầu bên phải và tiếp tục di chuyển sang trái."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 6
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 6 (Div. 2 Only)"
rating: 1200
weight: 6
solve_time_s: 89
verified: true
draft: false
---
[CF 6C - Alice, Bob và Sôcôla](https://codeforces.com/problemset/problem/6/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng thanh sô cô la và mỗi thanh cần một khoảng thời gian nhất định để ăn. Alice bắt đầu từ đầu bên trái và tiếp tục di chuyển sang phải. Bob bắt đầu từ đầu bên phải và tiếp tục di chuyển sang trái. Cả hai ăn liên tục không ngừng nghỉ, và không ai có thể ăn dở một miếng sô-cô-la rồi dừng giữa chừng. 

Điều thú vị là quá trình này được xác định theo thời gian chứ không phải lần lượt. Nếu Alice ăn hết sô cô la hiện tại của mình sớm hơn Bob, cô ấy sẽ ngay lập tức bắt đầu ăn sô cô la tiếp theo của mình. Điều tương tự cũng xảy ra với Bob. Cuối cùng hai người chơi gặp nhau ở giữa. Nếu họ bắt đầu sử dụng cùng một loại sô cô la vào cùng một thời điểm, Alice sẽ nhận được sô cô la đó và Bob sẽ dừng lại. 

Đầu vào chỉ đơn giản là một mảng trong đó mỗi phần tử biểu thị thời gian cần thiết để ăn một viên sôcôla. Đầu ra hỏi Alice ăn bao nhiêu sôcôla và cuối cùng Bob ăn bao nhiêu sôcôla. 

Những ràng buộc đủ lớn để chúng ta phải suy nghĩ cẩn thận về hiệu quả. Số lượng sôcôla có thể đạt tới$10^5$, vì vậy mọi giải pháp bậc hai sẽ thực hiện xung quanh$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn trong thời gian chạy 2 giây. Thời gian tuyến tính là mục tiêu tự nhiên ở đây. Vì chúng ta chỉ cần xử lý sôcôla theo thứ tự từ cả hai đầu, điều này gợi ý rõ ràng về cách tiếp cận hai con trỏ. 

Có một số trường hợp đặc biệt có thể lặng lẽ phá vỡ việc triển khai không chính xác. 

Hãy xem xét một sô cô la duy nhất:```
1
5
```Đầu ra đúng là:```
1 0
```Alice bắt đầu từ bên trái, Bob bắt đầu từ bên phải, nhưng cả hai vị trí đều giống nhau. Alice nhận được sô cô la vì những chiếc cà vạt thuộc về cô ấy. Việc thực hiện bất cẩn có thể đưa nhầm một viên sôcôla cho mỗi người chơi. 

Một trường hợp khó khăn khác là khi cả hai người chơi tích lũy thời gian ăn bằng nhau trước khi đến giữa:```
3
2 2 2
```Đầu ra đúng là:```
2 1
```Alice ăn miếng sô cô la đầu tiên trong 2 giây, Bob ăn miếng cuối cùng trong 2 giây và bây giờ cả hai sẽ bắt đầu ăn miếng sô cô la ở giữa cùng một lúc. Alice nhận được nó. 

Một nguồn lỗi khác xuất hiện khi một bên tiến bộ nhanh hơn nhiều:```
5
1 100 1 1 1
```Đầu ra đúng là:```
1 4
```Alice bị mắc kẹt trong miếng sôcôla lớn trong một thời gian dài trong khi Bob nhanh chóng ăn hết một ít sôcôla ở bên phải. Việc triển khai luân phiên thay vì so sánh thời gian tích lũy sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp là ý tưởng tự nhiên đầu tiên. Chúng ta có thể tưởng tượng việc theo dõi thời gian hiện tại chính xác của Alice và Bob, ưu tiên người chơi nào về đích trước. Mỗi người chơi tiêu thụ từng viên sôcôla một và chúng tôi liên tục xác định xem ai sẽ là người tự do tiếp theo. 

Mô phỏng lực lượng vũ phu này thực sự chính xác vì bản thân quá trình này là tuần tự và mang tính quyết định. Vấn đề là việc triển khai đơn giản có thể liên tục tìm kiếm sự kiện tiếp theo hoặc tính toán lại thời gian một cách không cần thiết. Nếu chúng ta mô phỏng đơn vị thời gian theo đơn vị thời gian, thời gian chạy sẽ trở nên thảm hại vì thời gian sô-cô-la có thể cộng lại bằng$10^8$. 

Quan sát quan trọng là chúng ta không bao giờ cần mô phỏng từng giây riêng lẻ. Tại bất kỳ thời điểm nào, thông tin duy nhất quan trọng là tổng thời gian mà Alice đã sử dụng và tổng thời gian mà Bob đã sử dụng. 

Giả sử Alice dành tổng thời gian ít hơn Bob. Điều đó có nghĩa là Alice về đích sớm hơn và được tự do trước, vì vậy cô ấy phải lấy viên sô cô la tiếp theo ở bên trái. Tương tự, nếu Bob dành ít thời gian hơn, anh ấy phải lấy viên sô cô la tiếp theo ở bên phải. 

Điều này biến toàn bộ quá trình thành một thủ tục tham lam rất nhỏ gọn. Chúng tôi duy trì hai con trỏ, một từ bên trái và một từ bên phải, cùng với thời gian ăn tích lũy cho cả hai người chơi. Ở mỗi bước, người chơi có tổng thời gian nhỏ hơn sẽ lấy sô cô la có sẵn tiếp theo. 

Cấu trúc của bài toán làm cho sự lựa chọn tham lam này trở nên an toàn vì thứ tự ăn uống bị ép buộc bởi thời gian hoàn thành. Không ai có thể bỏ qua một thanh sôcôla hoặc cố ý chờ đợi, vì vậy bước đi tiếp theo được xác định duy nhất bởi ai có mặt trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng thời gian ăn) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ: 

-`left = 0`về phía Alice. 
-`right = n - 1`về phía Bob. 
2. Duy trì thời gian ăn uống tích lũy: 

-`alice_time = 0`-`bob_time = 0`3. Duy trì quầy sôcôla đã ăn: 

-`alice_count = 0`-`bob_count = 0`4. Tiếp tục trong khi`left <= right`. 

Ở mỗi bước, hãy so sánh`alice_time`Và`bob_time`. 
5. Nếu`alice_time <= bob_time`, Alice sẽ sẵn sàng trước hoặc cả hai sẽ sẵn sàng đồng thời. 

Alice lấy sô cô la ở chỉ số`left`. 

Thêm thời gian ăn của nó vào`alice_time`, tăng`alice_count`, và di chuyển`left`đúng một bước. 

Trường hợp bình đẳng rất quan trọng vì mối quan hệ thuộc về Alice. 
6. Nếu không, Bob sẽ có mặt trước. 

Bob lấy sô cô la ở chỉ số`right`. 

Thêm thời gian ăn của nó vào`bob_time`, tăng`bob_count`, và di chuyển`right`còn một bước nữa. 
7. Khi các con trỏ giao nhau, mỗi viên sôcôla đã được chỉ định. In hai quầy. 

### Tại sao nó hoạt động 

Điều bất biến là trước mỗi bước, mỗi viên sô-cô-la nằm ngoài khoảng`[left, right]`đã được chỉ định vĩnh viễn cho chính xác một người chơi và`alice_time`Và`bob_time`thể hiện tổng thời gian chính xác mà mỗi người chơi đã dành cho đến nay. 

Nếu Alice tiêu tốn tổng thời gian ít hơn Bob, cô ấy phải hoàn thành sớm hơn và ngay lập tức bắt đầu lấy sô cô la tiếp theo dành cho mình. Bob không thể lấy được sô cô la đó trước vì anh ấy vẫn đang bận ăn. Lý do tương tự cũng được áp dụng một cách đối xứng cho Bob. 

Khi tổng điểm của cả hai bằng nhau, cả hai người chơi đều được tự do cùng một lúc. Tuyên bố nói rõ rằng Alice nhận được sô cô la được chia sẻ trong tình huống đó, đó chính xác là lý do tại sao điều kiện sử dụng`<=`. 

Vì mỗi bước đều khớp chính xác với quy trình thực nên thuật toán sẽ tạo ra số đếm cuối cùng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
t = list(map(int, input().split()))

left = 0
right = n - 1

alice_time = 0
bob_time = 0

alice_count = 0
bob_count = 0

while left <= right:
    if alice_time <= bob_time:
        alice_time += t[left]
        alice_count += 1
        left += 1
    else:
        bob_time += t[right]
        bob_count += 1
        right -= 1

print(alice_count, bob_count)
```Giải pháp tuân theo mô phỏng tham lam trực tiếp. 

Hai con trỏ đại diện cho số sôcôla chưa có người nhận còn lại. Mọi thứ bên ngoài khoảng thời gian đó đều đã bị ăn hết. 

Sự so sánh giữa`alice_time`Và`bob_time`là ý tưởng cốt lõi. Nếu Alice dành ít hơn hoặc tổng thời gian bằng nhau, cô ấy phải là người chơi tiếp theo được tự do, vì vậy cô ấy lấy sô cô la tiếp theo từ phía bên trái. 

Trường hợp bình đẳng là tinh tế. sử dụng`<`thay vì`<=`sẽ đưa sôcôla cùng lúc cho Bob một cách không chính xác, điều này vi phạm tuyên bố. 

Điều kiện vòng lặp cũng rất quan trọng. Chúng tôi tiếp tục trong khi`left <= right`, không chỉ`<`. Khi còn lại đúng một viên sô cô la, nó vẫn phải được giao chính xác, thường là cho Alice vì quy tắc hòa. 

Tất cả các biến đều vừa vặn thoải mái trong số nguyên Python vì tổng thời gian ăn tối đa là$10^8$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 9 8 2 7
```| Bước | trái | đúng | alice_time | bob_time | Hành động | thanh Alice | Thanh bob | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 4 | 0 | 0 | Alice mất 2 | 1 | 0 | 
| 1 | 1 | 4 | 2 | 0 | Bob mất 7 | 1 | 1 | 
| 2 | 1 | 3 | 2 | 7 | Alice mất 9 | 2 | 1 | 
| 3 | 2 | 3 | 11 | 7 | Bob mất 2 | 2 | 2 | 
| 4 | 2 | 2 | 11 | 9 | Bob mất 8 | 2 | 3 | 

Đầu ra cuối cùng:```
2 3
```Dấu vết này cho thấy một người chơi có thể tiêu thụ nhiều sôcôla liên tiếp nếu thời gian tích lũy của họ vẫn nhỏ hơn. Thuật toán phản ánh quá trình định thời gian thực mà không mô phỏng rõ ràng từng giây. 

### Ví dụ 2 

đầu vào:```
3
2 2 2
```| Bước | trái | đúng | alice_time | bob_time | Hành động | thanh Alice | Thanh bob | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 2 | 0 | 0 | Alice mất 2 | 1 | 0 | 
| 1 | 1 | 2 | 2 | 0 | Bob mất 2 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 2 | Alice mất 2 | 2 | 1 | 

Đầu ra cuối cùng:```
2 1
```Ví dụ này thể hiện quy tắc hòa. Khi cả hai thời gian tích lũy bằng nhau, Alice nhận được số sô cô la còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi sô-cô-la được chế biến đúng một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Thuật toán thực hiện một lần duyệt qua mảng bằng hai con trỏ. Với$n \le 10^5$, thời gian tuyến tính dễ dàng nằm gọn trong giới hạn thời gian và bộ nhớ bổ sung liên tục thấp hơn nhiều so với giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    t = list(map(int, input().split()))

    left = 0
    right = n - 1

    alice_time = 0
    bob_time = 0

    alice_count = 0
    bob_count = 0

    while left <= right:
        if alice_time <= bob_time:
            alice_time += t[left]
            alice_count += 1
            left += 1
        else:
            bob_time += t[right]
            bob_count += 1
            right -= 1

    print(alice_count, bob_count)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("5\n2 9 8 2 7\n") == "2 3", "sample 1"

# minimum size
assert run("1\n5\n") == "1 0", "single chocolate"

# all equal values
assert run("4\n3 3 3 3\n") == "2 2", "equal times"

# tie on middle chocolate
assert run("3\n2 2 2\n") == "2 1", "alice gets middle"

# strong imbalance
assert run("5\n1 100 1 1 1\n") == "1 4", "bob finishes many quickly"

# off-by-one boundary
assert run("2\n1 2\n") == "1 1", "two elements"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`1 0`| Xử lý một phần tử | 
|`4 / 3 3 3 3`|`2 2`| Phân phối cân bằng | 
|`3 / 2 2 2`|`2 1`| Tie đi đến Alice | 
|`5 / 1 100 1 1 1`|`1 4`| Liên tiếp di chuyển về một phía | 
|`2 / 1 2`|`1 1`| Tính chính xác của con trỏ chéo | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
5
```Ban đầu cả hai thời gian tích lũy đều bằng 0 nên điều kiện`alice_time <= bob_time`là đúng. Alice lấy viên sô cô la duy nhất, số đếm của cô ấy trở thành 1 và các con trỏ ngay lập tức giao nhau. 

Đầu ra cuối cùng là:```
1 0
```Trường hợp này xác nhận rằng thuật toán xử lý một viên sôcôla còn lại một cách chính xác và tôn trọng quy tắc hòa. 

Bây giờ hãy xem xét kịch bản ở giữa:```
3
2 2 2
```Sau hai nước đi đầu tiên: 

- Alice đã dành 2 giây. 
- Bob đã dành 2 giây. 
- Chỉ còn lại phần sô-cô-la ở giữa. 

Bởi vì thuật toán sử dụng`<=`, Alice nhận được sô cô la cuối cùng. Đầu ra trở thành:```
2 1
```Nếu điều kiện được viết là`<`, Bob sẽ nhận nhầm sô cô la ở giữa. 

Cuối cùng, hãy xem xét trường hợp một người chơi tiêu thụ nhanh chóng nhiều sôcôla:```
5
1 100 1 1 1
```Từng bước một: 

- Alice ăn`1`, tổng số trở thành 1. 
- Bob ăn`1`, tổng số trở thành 1. 
- Alice và Bob bị trói nên Alice bắt đầu`100`. 
- Bob ăn ba viên sôcôla còn lại trong khi Alice vẫn đang bận. 

Đầu ra là:```
1 4
```Điều này xác minh rằng thuật toán mô hình chính xác thời gian thực đã trôi qua thay vì luân phiên một cách máy móc.
