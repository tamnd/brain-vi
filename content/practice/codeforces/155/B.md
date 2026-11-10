---
title: "CF 155B - Kết hợp"
description: "Chúng tôi có một bộ sưu tập thẻ. Mỗi lá bài mang lại hai thứ khi chơi. Số đầu tiên cộng thêm điểm và số thứ hai bổ sung thêm cơ hội để chơi nhiều bài hơn. Trò chơi bắt đầu với đúng một nước đi có sẵn."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 155
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 109 (Div. 2)"
rating: 1100
weight: 155
solve_time_s: 95
verified: true
draft: false
---

[CF 155B - Kết hợp](https://codeforces.com/problemset/problem/155/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 35 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập thẻ. Mỗi lá bài mang lại hai thứ khi chơi. Số đầu tiên cộng thêm điểm và số thứ hai bổ sung thêm cơ hội để chơi nhiều bài hơn. 

Trò chơi bắt đầu với đúng một nước đi có sẵn. Đánh bài tốn một nước đi, sau đó cộng ngay`b[i]`những động thái mới. Một thẻ chỉ có thể được sử dụng một lần. Trò chơi kết thúc khi không còn nước đi nào hoặc khi mọi lá bài đã được sử dụng. 

Nhiệm vụ là chọn cả tập hợp con các quân bài và thứ tự chơi sao cho tổng số điểm kiếm được càng lớn càng tốt. 

Các ràng buộc đủ nhỏ để việc sắp xếp có thể dễ dàng thực hiện được. Với`n ≤ 1000`, thậm chí là một`O(n^2)`giải pháp sẽ trôi qua một cách thoải mái. Không thể tìm kiếm theo cấp số nhân trên tất cả các tập hợp con hoặc tất cả các hoán vị vì`2^1000`Và`1000!`có kích thước lớn về mặt thiên văn. Cấu trúc của trò chơi phải được khai thác trực tiếp. 

Điều khó khăn là một lá bài có điểm thấp vẫn có thể cực kỳ có giá trị nếu nó tạo ra nhiều nước đi bổ sung. Một chiến lược tham lam luôn giành được điểm cao nhất trước tiên có thể thất bại nặng nề. 

Hãy xem xét ví dụ này:```
3
100 0
1 2
50 0
```Nếu chúng ta tham lam chọn điểm lớn nhất trước, chúng ta sẽ chơi`(100,0)`và trò chơi kết thúc với tổng số`100`. 

Trình tự tối ưu là khác nhau. Lần đầu tiên chúng tôi chơi`(1,2)`, làm tăng số nước đi có sẵn, sau đó chơi cả hai quân bài còn lại. Tổng số trở thành`1 + 100 + 50 = 151`. 

Một sai lầm dễ mắc phải khác là cho rằng mọi lá bài có điểm dương đều phải được lấy. Đôi khi trò chơi kết thúc trước khi chúng ta có thể tiếp cận được họ.```
2
10 0
9 0
```Chúng ta chỉ có một nước đi ban đầu và không có quân bài nào tạo thêm nước đi. Chúng ta có thể chơi chính xác một lá bài, vì vậy câu trả lời là`10`, không`19`. 

Ngoài ra còn có một vấn đề nhỏ về thứ tự giữa các quân bài tạo ra các nước đi bổ sung. Giả sử chúng ta có:```
3
5 1
4 1
100 0
```Chơi hai quân bài tạo ra trước tiên cho phép chúng ta cuối cùng lấy được cả ba quân bài`109`. Nếu chúng ta chơi`(100,0)`ngay lập tức, trò chơi kết thúc ngay lập tức. 

Thử thách cốt lõi là cân bằng điểm số trước mắt với khả năng tiếp tục chơi trong tương lai. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi thứ tự chơi bài có thể và mô phỏng trò chơi. Điều này đúng vì kết quả trận đấu phụ thuộc hoàn toàn vào trình tự đã chọn. Thật không may, số lượng hoán vị là`n!`. Ngay cả đối với`n = 15`, cái này đã quá lớn rồi. 

Chúng ta cần hiểu điều gì thực sự quan trọng theo một trình tự tốt. 

Giả sử một thẻ có`b[i] > 0`. Chơi nó không bao giờ làm giảm tổng số thẻ có thể chơi được trong tương lai. Chúng ta bỏ ra một nước đi và thu lại được ít nhất một nước đi. Những lá bài như vậy rất có giá trị vì chúng giúp trò chơi tồn tại. 

Bây giờ giả sử một thẻ có`b[i] = 0`. Chơi nó sẽ giảm nghiêm ngặt số lần di chuyển còn lại đi một. Những thẻ này tiêu thụ nguồn tài nguyên hạn chế. 

Quan sát này gợi ý chia các thẻ thành hai nhóm. 

Thẻ có`b[i] > 0`là thẻ cơ sở hạ tầng. Chúng giúp chúng ta tiếp tục chơi nhiều bài hơn sau này. Thẻ có`b[i] = 0`là những thẻ đầu cuối. Chúng thường nên bị trì hoãn cho đến khi chúng tôi đã mở rộng các bước di chuyển có sẵn của mình nhiều nhất có thể. 

Trong số các thẻ cơ sở hạ tầng, chúng tôi muốn ưu tiên những thẻ tạo ra nhiều cơ hội hơn trong tương lai. Nếu một lá bài cung cấp thêm nhiều nước đi, việc trì hoãn nó có nguy cơ dẫn đến hết nước đi trước khi sử dụng. 

Điều này dẫn đến chiến lược tham lam: 

Sắp xếp thẻ theo thứ tự giảm dần`b[i]`. Sau đó xử lý chúng theo thứ tự đó trong khi chúng ta vẫn còn những nước đi sẵn có. 

Tại sao điều này làm việc? 

Bất cứ khi nào chúng ta phải lựa chọn giữa hai thẻ chưa sử dụng, thẻ có giá trị lớn hơn`b[i]`bảo tồn tính linh hoạt hơn trong tương lai. Thực hiện sớm hơn chỉ có thể hữu ích vì nó để lại cho chúng ta ít nhất nhiều bước đi trong tương lai như bất kỳ lựa chọn nào khác. 

Sau khi xử lý xong tất cả các thẻ mở rộng có lợi nhuận, chúng tôi sử dụng các nước đi còn lại trên các thẻ đầu cuối có điểm cao nhất. 

Trên thực tế, một quy tắc sắp xếp duy nhất đã nắm bắt được hành vi này một cách rõ ràng. Sắp xếp giảm dần`b[i]`tự động đặt tất cả các card mở rộng trước các card đầu cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n!)`|`O(n)`| Quá chậm | 
| Tham lam tối ưu + Sắp xếp |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các thẻ và lưu trữ chúng theo cặp`(a, b)`. 
2. Sắp xếp các thẻ theo thứ tự giảm dần`b`. 

Những lá bài tạo ra nhiều nước đi trong tương lai nên được sử dụng sớm hơn vì chúng giúp trò chơi tồn tại lâu hơn. 
3. Khởi tạo số bước di chuyển có sẵn như`1`. 

Điều này phù hợp với quy tắc trò chơi là ban đầu chúng ta có thể chơi chính xác một lá bài. 
4. Lặp lại các thẻ đã được sắp xếp. 
5. Trước khi chơi bài, hãy kiểm tra xem chúng ta có còn ít nhất một nước đi hay không. 

Nếu bộ đếm nước đi đã bằng 0, trò chơi phải dừng ngay lập tức. 
6. Chơi bài hiện tại. 

Thêm vào`a[i]`để trả lời. 
7. Cập nhật bộ đếm nước đi. 

Chơi bài tiêu tốn một nước đi và thêm`b[i]`động thái mới, vì vậy:```
moves = moves - 1 + b[i]
```8. Tiếp tục cho đến khi tất cả các thẻ được xử lý hoặc bộ đếm nước đi trở về 0. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý một số tiền tố của thứ tự đã sắp xếp, không có thứ tự nào khác của các quân bài đó có thể khiến chúng ta có nhiều nước đi còn lại hơn. 

Giả sử hai thẻ liền kề nhau và`b`giá trị thỏa mãn`b1 < b2`. Nếu chúng ta hoán đổi chúng sao cho lớn hơn`b`quân bài đến trước, số nước đi còn lại sau khi chơi cả hai quân bài sẽ tăng lên sớm hơn và không bao giờ trở nên tệ hơn về sau. Đối số trao đổi này cho thấy rằng bất kỳ thứ tự tối ưu nào cũng có thể được chuyển đổi thành một thứ tự được sắp xếp giảm dần`b`mà không làm giảm số điểm đạt được. 

Khi thứ tự được cố định theo cách này, việc lấy mọi quân bài có thể chơi được luôn là tối ưu vì điểm số không âm. Không bao giờ có lý do để bỏ qua một quân bài nếu chúng ta vẫn còn một nước đi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    cards = []

    for _ in range(n):
        a, b = map(int, input().split())
        cards.append((b, a))

    cards.sort(reverse=True)

    moves = 1
    ans = 0

    for b, a in cards:
        if moves == 0:
            break

        ans += a
        moves = moves - 1 + b

    print(ans)

solve()
```Việc triển khai lưu trữ thẻ dưới dạng`(b, a)`thay vì`(a, b)`bởi vì Python sắp xếp các bộ dữ liệu theo từ điển. Điều này làm cho việc sắp xếp giảm dần tự động ưu tiên lớn hơn`b`. 

Biến`moves`đại diện cho số lượng thẻ bổ sung vẫn có thể được chơi. Mỗi lần lặp lại trước tiên sẽ kiểm tra xem trò chơi đã kết thúc chưa. Việc quên điều kiện này sẽ gây ra các lượt chơi thêm không chính xác sau khi bộ đếm nước đi về 0. 

Công thức cập nhật là trung tâm của mô phỏng:```
moves = moves - 1 + b
```Việc trừ phải xảy ra vì quân bài hiện tại tiêu tốn một cơ hội có thể chơi được. Một lỗi thường gặp là viết`moves += b`, cho phép chơi miễn phí một cách không chính xác. 

Giải pháp không bao giờ bỏ qua một lá bài có thể chơi được vì tất cả các điểm đều không âm. Khi có thể truy cập được thẻ, việc lấy thẻ đó không thể làm giảm tổng điểm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
1 0
2 0
```Sau khi sắp xếp giảm dần`b`: 

| Bước | Thẻ`(a,b)`| Di chuyển trước | Điểm trước | Di chuyển sau | Điểm Sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`(2,0)`| 1 | 0 | 0 | 2 | 

Trò chơi dừng ngay lập tức vì không còn nước đi nào nữa. 

Dấu vết cho thấy tại sao chỉ có thể chơi được một lá bài. Vì không có thẻ nào tạo ra thêm nước đi nên nước đi duy nhất ban đầu là toàn bộ ngân sách tài nguyên. 

### Ví dụ tùy chỉnh 

đầu vào:```
4
100 0
1 3
50 0
40 0
```Thứ tự sắp xếp:```
(1,3), (100,0), (50,0), (40,0)
```Mô phỏng: 

| Bước | Thẻ`(a,b)`| Di chuyển trước | Điểm trước | Di chuyển sau | Điểm Sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`(1,3)`| 1 | 0 | 3 | 1 | 
| 2 |`(100,0)`| 3 | 1 | 2 | 101 | 
| 3 |`(50,0)`| 2 | 101 | 1 | 151 | 
| 4 |`(40,0)`| 1 | 151 | 0 | 191 | 

Lá bài đầu tiên có số điểm rất nhỏ nhưng nó mở ra đủ lượt di chuyển để thu thập mọi lá bài khác. Điều này chứng tỏ tại sao tối đa hóa điểm số ngay lập tức là tiêu chí tham lam sai lầm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian |`O(n)`| Mảng thẻ lưu trữ tất cả các thẻ đầu vào | 

Với`n ≤ 1000`, giải pháp này dễ dàng đủ nhanh. Việc sắp xếp một nghìn phần tử là không đáng kể trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    cards = []

    for _ in range(n):
        a, b = map(int, input().split())
        cards.append((b, a))

    cards.sort(reverse=True)

    moves = 1
    ans = 0

    for b, a in cards:
        if moves == 0:
            break

        ans += a
        moves = moves - 1 + b

    return str(ans)

# provided sample
assert solve_io(
"""2
1 0
2 0
"""
) == "2"

# minimum size
assert solve_io(
"""1
7 0
"""
) == "7"

# expansion card unlocks everything
assert solve_io(
"""4
100 0
1 3
50 0
40 0
"""
) == "191"

# all cards self-sustaining
assert solve_io(
"""3
5 1
6 1
7 1
"""
) == "18"

# greedy by score would fail
assert solve_io(
"""3
100 0
1 2
50 0
"""
) == "151"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thẻ đơn với`b=0`|`7`| Kích thước đầu vào tối thiểu | 
| Một card mở rộng cộng với thiết bị đầu cuối |`191`| Các chiêu bổ sung phải được sử dụng trước | 
| Tất cả`b=1`|`18`| Tiếp tục vô tận cho đến khi hết thẻ | 
| Điểm cao nhưng không mở rộng |`151`| Tham lam theo điểm là không đúng | 

## Vỏ cạnh 

Hãy xem xét trường hợp mỗi thẻ có`b = 0`.```
3
5 0
8 0
2 0
```Sau khi sắp xếp, thứ tự sẽ trở thành`(8,0), (5,0), (2,0)`. 

Chúng tôi bắt đầu với một động thái. Đang chơi`(8,0)`giảm bộ đếm bước đi về 0 ngay lập tức, do đó câu trả lời trở thành`8`. 

Thuật toán nhận ra chính xác rằng chỉ có thể chơi được một lá bài. 

Bây giờ hãy xem xét trường hợp một lá bài có điểm thấp tạo ra nhiều nước đi phụ.```
3
100 0
1 2
50 0
```Thứ tự sắp xếp bắt đầu bằng`(1,2)`bởi vì nó`b`giá trị là lớn nhất. 

Sau khi chơi nó, số lần di chuyển sẽ trở thành`2`, cho phép thu thập cả hai thẻ còn lại. Điểm số cuối cùng trở thành`151`. 

Một chiến lược ngây thơ chỉ dựa vào điểm số sẽ dừng lại ở`100`. 

Cuối cùng, hãy xem xét các thẻ có`b = 1`.```
4
3 1
4 1
5 1
6 1
```Mỗi lá bài đã chơi sẽ bổ sung chính xác nước đi đã tiêu thụ. Bộ đếm nước đi không bao giờ giảm cho đến khi hết thẻ. 

Mô phỏng phát triển như thế này:```
1 -> 1 -> 1 -> 1 -> 1
```Tất cả các quân bài đều được chơi thành công và tổng số điểm sẽ trở thành`18`. 

Điều này xác nhận rằng công thức cập nhật xử lý các thẻ tự duy trì một cách chính xác.
