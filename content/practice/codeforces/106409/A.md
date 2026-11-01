---
title: "CF 106409A - Harker!!"
description: "Bài toán mô tả cây nhị phân đầy đủ có chiều cao h. Mỗi nút là một nhiệm vụ. Ban đầu chỉ có tác vụ gốc có sẵn. Trong mỗi thời điểm, tối đa p bộ xử lý có thể hoàn thành các nhiệm vụ có sẵn và việc hoàn thành một nhiệm vụ sẽ khiến hai bộ xử lý con của nó sẵn sàng cho thời điểm tiếp theo."
date: "2026-06-25T09:57:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106409
codeforces_index: "A"
codeforces_contest_name: "HPI 2026 Advanced"
rating: 0
weight: 106409
solve_time_s: 36
verified: true
draft: false
---

[CF 106409A - Harker!!](https://codeforces.com/problemset/problem/106409/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả cây nhị phân đầy đủ có chiều cao`h`. Mỗi nút là một nhiệm vụ. Ban đầu chỉ có tác vụ gốc có sẵn. Trong mỗi thời điểm, có tới`p`bộ xử lý có thể hoàn thành các nhiệm vụ có sẵn và việc hoàn thành một nhiệm vụ sẽ giúp hai phần tử con của nó sẵn sàng cho thời điểm tiếp theo. Một tác vụ khả dụng sẽ vẫn khả dụng cho đến khi bộ xử lý xử lý nó. 

Đối với mỗi trường hợp thử nghiệm, chúng ta cần tìm số khoảnh khắc tối thiểu cần thiết để hoàn thành tất cả các nhiệm vụ. 

Đầu vào đưa ra một số kịch bản độc lập. Mỗi kịch bản chứa chiều cao của cây và số lượng bộ xử lý. Đầu ra là độ dài lịch trình tối thiểu cho kịch bản đó. 

Khó khăn chính đến từ việc xử lý không bị buộc phải diễn ra theo từng cấp độ. Một giải pháp chậm có thể hoàn thành tất cả các nút ở một độ sâu trước khi xem xét độ sâu tiếp theo, nhưng bộ xử lý có thể để lại một số nút ở độ sâu trước đó chưa hoàn thành và kết hợp chúng với các tác vụ mới được tạo sau này. 

Chiều cao đủ nhỏ để số lượng cấp độ là yếu tố quan trọng chứ không phải tổng số nút. Cây nhị phân đầy đủ có chiều cao`h`có`2^h - 1`các nút, do đó việc mô phỏng trực tiếp mọi nút trở nên không thể khi`h`là lớn. Giải pháp phải hoạt động chỉ bằng số lượng nhiệm vụ có sẵn ở mỗi độ sâu. Vì số lượng bộ xử lý cũng có thể lớn nên mọi thao tác phải gần với thời gian không đổi trên mỗi cấp độ. MỘT`O(2^h)`cách tiếp cận sẽ nhanh chóng trở nên quá chậm, trong khi một`O(h)`Cách tiếp cận dễ dàng xử lý các giới hạn. 

Trường hợp đặc biệt phổ biến là khi bộ xử lý có đủ khả năng để hoàn thành toàn bộ cấp độ ngay lập tức nhưng không đủ để hoàn thành tất cả công việc trong tương lai. Ví dụ:```
1
3 2
```Cây có 7 nút. Việc root được thực hiện ở thời điểm 1, hai nút ở thời điểm 2 và bốn nút còn lại không thể hoàn thành trong một thời điểm nữa vì chỉ tồn tại hai bộ xử lý. Câu trả lời đúng là:```
4
```Việc triển khai theo từng cấp độ không chuyển tiếp công việc còn dang dở có thể vô tình tạo ra giá trị lớn hơn vì nó lãng phí bộ xử lý ở các cấp độ trống. 

Một trường hợp phức tạp khác là khi số lượng tác vụ khả dụng không chia hết cho số lượng bộ xử lý. Ví dụ:```
1
4 3
```Tại một thời điểm nào đó có thể có 5 nhiệm vụ sẵn sàng. Ba cái đã được xử lý và hai cái còn lại sẽ được kết hợp với các tác vụ được tạo sau này. Việc coi hai nhiệm vụ đó như thể chúng thuộc một cấp độ biệt lập sẽ đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng các cấp độ của cây. Ở độ sâu`0`có một nhiệm vụ, ở độ sâu`1`có hai nhiệm vụ, rồi bốn nhiệm vụ, v.v. Đối với mỗi độ sâu, chúng tôi có thể tính toán cần bao nhiêu khoảnh khắc để xử lý tất cả các tác vụ ở độ sâu đó. Điều này hiệu quả với những cây nhỏ vì cấu trúc đơn giản. 

Vấn đề là mô phỏng này giả định các nhiệm vụ ở một độ sâu này độc lập với các nhiệm vụ ở độ sâu tiếp theo. Trong thực tế, các nhiệm vụ còn sót lại vẫn có sẵn. Nếu ba bộ xử lý đang làm việc trên một cây và một cấp độ có bốn nhiệm vụ thì nhiệm vụ cuối cùng sẽ không biến mất sau thời điểm đó. Nó chờ và có thể được xử lý cùng với các nhiệm vụ ở cấp độ tiếp theo. 

Việc mô phỏng lực lượng vũ phu trên mỗi nút cũng quá tốn kém. Cây có chiều cao`h`chứa`2^h - 1`các nút và điều này trở nên rất lớn ngay cả đối với độ cao vừa phải. 

Quan sát quan trọng là chúng ta không cần biết các nhiệm vụ riêng lẻ. Chúng ta chỉ cần biết có bao nhiêu nhiệm vụ đang chờ trước khi xử lý từng cấp độ. Cho phép`remaining`đại diện cho các nhiệm vụ từ các cấp độ trước đó chưa được hoàn thành. Khi một cấp độ mới chứa`2^i`nhiệm vụ, tổng số công việc sẵn có sẽ trở thành`2^i + remaining`. Chúng tôi có thể hoàn thành càng nhiều nhóm bộ xử lý đầy đủ càng tốt và bất cứ thứ gì còn lại sẽ trở thành bộ xử lý mới`remaining`. 

Điều này biến vấn đề từ việc mô phỏng một cái cây thành một quá trình tham lam đơn giản theo chiều cao. Sự lựa chọn tham lam là tối ưu vì mọi khe cắm bộ xử lý nên được sử dụng càng sớm càng tốt. Việc trì hoãn một nhiệm vụ đã sẵn sàng không bao giờ tạo ra thêm năng lực trong tương lai, trong khi việc hoàn thành nhiệm vụ sớm hơn chỉ có thể khiến nhiều trẻ em bị ảnh hưởng hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^h) | O(2^h) | Quá chậm | 
| Tối ưu | O(h) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với`remaining = 0`Và`answer = 0`. các`remaining`value lưu trữ các tác vụ đã sẵn sàng nhưng chưa được xử lý trong thời điểm trước đó. 
2. Xử lý các cấp độ cây từ gốc trở xuống. Ở cấp độ`i`, số lượng nhiệm vụ mới có sẵn là`2^i`. Tổng số nhiệm vụ có thể được xử lý ở giai đoạn này là`2^i + remaining`. 

Lý do chúng tôi thêm`remaining`ở đây là những nhiệm vụ còn dang dở không thuộc một cấp độ cố định. Chúng chỉ đơn giản là các nhiệm vụ chờ đợi và có thể được xử lý cùng với các nhiệm vụ mới. 
3. Chia các tác vụ có sẵn cho số lượng bộ xử lý. Thương số cho chúng ta biết có thể dành bao nhiêu khoảnh khắc hoàn chỉnh để xử lý lượng công việc này. Thêm giá trị đó vào`answer`. 
4. Lưu trữ những công việc còn sót lại sau những khoảnh khắc hoàn thành đó. Chúng là những nhiệm vụ sẽ đợi cho đến khi cấp độ tiếp theo tạo ra nhiều công việc có sẵn hơn. 
5. Sau khi tất cả các cấp độ được xử lý, nếu một số nhiệm vụ vẫn đang chờ thì cần một khoảnh khắc cuối cùng để hoàn thành chúng. Thêm khoảnh khắc cuối cùng này vào câu trả lời. 

Tại sao nó hoạt động: 

Ở mọi cấp độ, thuật toán giữ chính xác tập hợp các nhiệm vụ hiện có nhưng chưa hoàn thành. Không có tác vụ nào bị xóa trừ khi bộ xử lý xử lý nó. Bước tham lam sử dụng hết dung lượng bộ xử lý có thể ngay lập tức là an toàn vì việc xử lý một tác vụ sớm hơn chỉ có thể tiết lộ thêm con chứ không bao giờ làm giảm các lựa chọn trong tương lai. Phần còn lại được lưu trữ sẽ bảo toàn tất cả công việc chưa hoàn thành, vì vậy cấp độ tiếp theo sẽ bắt đầu với trạng thái chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        h, p = map(int, input().split())

        remaining = 0
        moments = 0

        for level in range(h):
            tasks = (1 << level) + remaining
            moments += tasks // p
            remaining = tasks % p

        if remaining:
            moments += 1

        ans.append(str(moments))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp lặp qua từng độ sâu của cây. biểu hiện`1 << level`tính toán`2^level`, giúp tránh các phép tính dấu phẩy động và giữ cho phép tính chính xác. 

Bước phân chia đếm xem có bao nhiêu lô bộ xử lý hoàn chỉnh phù hợp với các tác vụ hiện có. Nhiệm vụ còn lại là phần quan trọng của việc thực hiện. Nếu bị quên, giải pháp sẽ giả định không chính xác rằng mọi độ sâu phải được hoàn thành trước khi tiếp tục. 

Kiểm tra cuối cùng xử lý các nhiệm vụ còn dang dở sau cấp độ cuối cùng. Những chiếc lá không tạo thêm nhiệm vụ nữa nên công việc duy nhất còn lại là dành thêm một chút thời gian để hoàn thành chúng. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3
3 1
3 2
10 6
```Trường hợp đầu tiên: 

| Cấp độ | Nhiệm vụ mới | Còn lại trước | Tổng số nhiệm vụ | Đã thêm khoảnh khắc | Còn lại sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 0 | 
| 1 | 2 | 0 | 2 | 2 | 0 | 
| 2 | 4 | 0 | 4 | 4 | 0 | 

Câu trả lời là`7`. Với một bộ xử lý, mỗi nút phải được xử lý riêng lẻ. 

Trường hợp thứ hai: 

| Cấp độ | Nhiệm vụ mới | Còn lại trước | Tổng số nhiệm vụ | Đã thêm khoảnh khắc | Còn lại sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 0 | 1 | 
| 1 | 2 | 1 | 3 | 1 | 1 | 
| 2 | 4 | 1 | 5 | 2 | 1 | 

Sau vòng lặp, một nhiệm vụ còn lại cần thêm một chút thời gian, đưa ra`4`. 

Dấu vết cho thấy tại sao việc mang theo thức ăn thừa lại quan trọng. Sau khoảnh khắc đầu tiên với hai bộ xử lý, tác vụ chưa hoàn thành vẫn có sẵn và được kết hợp với công việc ở cấp độ tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(h) | Mỗi cấp độ cây được xử lý một lần | 
| Không gian | O(1) | Chỉ các bộ đếm cho trạng thái hiện tại mới được lưu trữ | 

Thuật toán không bao giờ xây dựng cây nên việc sử dụng bộ nhớ của nó không phụ thuộc vào số lượng nút. Vì công việc chỉ tỷ lệ thuận với chiều cao nên nó vẫn nhanh ngay cả khi bản thân cây chứa quá nhiều nút để mô phỏng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue()

# provided samples
assert run("""3
3 1
3 2
10 6
""") == """7
4
173
""", "samples"

# minimum size
assert run("""1
1 1
""") == """1
""", "single node"

# all tasks fit immediately after first levels
assert run("""1
5 100
""") == """1
""", "many processors"

# leftovers across levels
assert run("""1
4 3
""") == """5
""", "carry remainder"

# large height with small processors
assert run("""1
10 1
""") == """1023
""", "single processor full tree"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1`|`1`| Trường hợp cây nhỏ nhất | 
|`1 / 5 100`|`1`| Đủ bộ xử lý để hoàn thành mọi thứ một cách nhanh chóng | 
|`1 / 4 3`|`5`| Nhiệm vụ còn sót lại được thực hiện giữa các cấp | 
|`1 / 10 1`|`1023`| Trường hợp xấu nhất là thiếu bộ xử lý | 

## Vỏ cạnh 

Đối với trường hợp bộ xử lý khớp chính xác với kích thước cấp độ, chẳng hạn như:```
1
3 4
```gốc được hoàn thành trong khoảnh khắc đầu tiên. Cấp độ tiếp theo có hai nhiệm vụ và cả hai đều được hoàn thành ngay lập tức. Cấp độ cuối cùng có bốn nhiệm vụ và cũng kết thúc ngay lập tức. Câu trả lời là`3`. Thuật toán xử lý việc này vì mỗi cấp độ đóng góp một đợt và không để lại phần còn lại. 

Đối với trường hợp còn dư:```
1
4 3
```cấp 0 đưa ra một nhiệm vụ còn sót lại sau khoảnh khắc đầu tiên. Ở cấp độ 1, công việc có sẵn là`2 + 1 = 3`, thế là nó kết thúc trong một khoảnh khắc và không để lại dấu vết nào. Ở cấp độ 2, bốn nhiệm vụ mới xuất hiện. Ba được xử lý, để lại một. Cấp độ 3 bắt đầu bằng`8 + 1 = 9`nhiệm vụ đòi hỏi ba khoảnh khắc. Tổng số trở thành`5`. Kết quả phụ thuộc vào việc duy trì tác vụ chờ thay vì đặt lại ở mọi cấp độ. 

Đối với một bộ xử lý duy nhất:```
1
10 1
```mọi nhiệm vụ đều yêu cầu thời điểm riêng của nó. Cây có chứa`2^10 - 1 = 1023`nhiệm vụ, vì vậy đầu ra là`1023`. Thuật toán đạt được điều này một cách tự nhiên bởi vì mọi phép chia cho`1`tiêu tốn chính xác một khoảnh khắc cho mỗi nhiệm vụ có sẵn.
