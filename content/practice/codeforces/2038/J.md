---
title: "CF 2038J - Chờ đợi..."
description: "We process a timeline of events at a bus stop. There are two kinds of events. Sự kiện P x có nghĩa là x hành khách bình thường đến và bắt đầu chờ đợi. Sự kiện A B x có nghĩa là một chiếc xe buýt đến với x chỗ ngồi miễn phí. Khi xe buýt đến, hành khách bình thường luôn lên trước Monocarp."
date: "2026-06-08T10:07:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2038
codeforces_index: "J"
codeforces_contest_name: "2024-2025 ICPC, NERC, Southern and Volga Russian Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 800
weight: 2038
solve_time_s: 108
verified: true
draft: false
---

[CF 2038J - Đang chờ...](https://codeforces.com/problemset/problem/2038/J) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi xử lý dòng thời gian của các sự kiện tại trạm xe buýt. 

Có hai loại sự kiện. MỘT`P x`sự kiện có nghĩa là`x`hành khách bình thường đến và bắt đầu chờ đợi. MỘT`B x`sự kiện có nghĩa là một chiếc xe buýt đến với`x`chỗ ngồi miễn phí. 

Khi xe buýt đến, hành khách bình thường luôn lên trước Monocarp. Nếu có nhiều hành khách chờ hơn số ghế thì chỉ có càng nhiều hành khách lên máy bay càng tốt và những người còn lại ở lại xếp hàng. Nếu tất cả hành khách đang chờ đều phù hợp, tất cả đều rời khỏi điểm dừng. 

Monocarp đặc biệt vì anh ta có thể chọn có lên một chiếc xe buýt vẫn còn chỗ trống sau khi tất cả hành khách bình thường đã lên hay không. Anh ta có thể bỏ qua chuyến xe buýt như vậy và tiếp tục đợi chuyến sau. 

Đối với mỗi sự kiện xe buýt, chúng tôi phải xác định xem liệu Monocarp có tồn tại một số chuỗi lựa chọn cho phép anh ta lên chiếc xe buýt cụ thể đó hay không. 

Số lượng sự kiện nhiều nhất là 1000, rất ít. Thậm chí một$O(n^2)$giải pháp sẽ thực hiện tối đa khoảng một triệu hoạt động. Điều đó có nghĩa là chúng tôi không cần cấu trúc dữ liệu phức tạp hoặc tối ưu hóa nâng cao. Thách thức ở đây là hiểu được thông tin nào thực sự quan trọng. 

Phần tế nhị là các quyết định của Monocarp chỉ ảnh hưởng đến một điều: liệu anh ta có còn đợi chuyến xe buýt tương lai đến hay không. Hành khách bình thường cư xử dứt khoát. Hàng đợi của họ tiến triển theo cùng một cách bất kể lựa chọn của Monocarp. 

Một lỗi phổ biến là mô phỏng Monocarp một cách tham lam, chẳng hạn như luôn lên chuyến xe buýt đầu tiên có sẵn. Coi như:```
P 2
B 3
B 1
```Các câu trả lời đúng là:```
YES
YES
```Monocarp có thể lên chuyến xe buýt đầu tiên vì vẫn còn một chỗ sau khi hai hành khách bước vào. Anh ta cũng có thể chọn bỏ qua và lên chuyến xe buýt thứ hai. Một mô phỏng đưa anh ta vĩnh viễn vào chiếc xe buýt đầu tiên sẽ in sai`NO`cho chuyến xe buýt thứ hai. 

Một sai lầm dễ mắc phải khác là quên rằng những hành khách không vừa sức vẫn phải chờ đợi:```
P 10
B 3
B 8
```Các câu trả lời đúng là:```
NO
YES
```Sau chuyến xe buýt đầu tiên, bảy hành khách vẫn đang đợi. Khi chiếc xe buýt thứ hai đến, cả bảy người lên xe, để lại một chỗ trống cho Monocarp. 

Trường hợp cạnh thứ ba xảy ra khi không có hành khách bình thường nào đang chờ đợi:```
B 1
```Câu trả lời là:```
YES
```Xe buýt có chỗ ngồi miễn phí ngay nên Monocarp có thể lên xe. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ là xem xét mọi xe buýt một cách độc lập. Đối với một xe buýt cụ thể, chúng tôi có thể phát lại tất cả các sự kiện trước đó, theo dõi số lượng hành khách đang chờ và kiểm tra xem xe buýt đó có ghế dự phòng sau khi phục vụ họ hay không. Điều này đúng vì hành khách bình thường là yếu tố duy nhất quyết định số chỗ còn trống. Nếu xe buýt có ít nhất một ghế dự phòng, Monocarp có thể chỉ cần chọn không lên bất kỳ chiếc xe buýt nào sớm hơn và thay vào đó bắt chiếc xe này. 

Với$m$xe buýt và$n$tổng số sự kiện, phát lại lịch sử cho mỗi chi phí xe buýt$O(nm)$, trở thành$O(n^2)$trong trường hợp xấu nhất. Với$n \le 1000$, thế này đã đủ nhanh rồi. 

Quan sát quan trọng là chúng ta thậm chí không cần tính toán lại lịch sử. Hàng đợi hành khách bình thường phát triển độc lập với Monocarp. Chúng tôi có thể xử lý các sự kiện một lần, duy trì số lượng hành khách đang chờ hiện tại. 

Giả sử một chiếc xe buýt đến với`b`chỗ ngồi và có`wait`hành khách hiện đang ở điểm dừng. 

Nếu như`b > wait`, thì sau khi tất cả hành khách bình thường lên máy bay, vẫn còn ít nhất một chỗ ngồi. Monocarp có thể bắt chuyến xe buýt này bằng cách tiếp tục đợi cho đến bây giờ. Câu trả lời là`YES`. 

Nếu như`b <= wait`, mọi chỗ ngồi đều được sử dụng bởi hành khách bình thường. Monocarp không có cơ hội lên chiếc xe buýt này nên câu trả lời là`NO`. 

Sau khi trả lời, hàng đợi thông thường phải được cập nhật. Xe buýt loại bỏ`min(wait, b)`hành khách rời đi`max(0, wait - b)`. 

Mô phỏng một lần này cho kết quả ngay lập tức đối với mỗi xe buýt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`wait = 0`, số lượng hành khách bình thường hiện đang chờ đợi. 
2. Xử lý các sự kiện theo trình tự thời gian. 
3. Nếu sự kiện đó`P x`, thêm những hành khách này vào hàng đợi bằng cách cài đặt`wait += x`. 
4. Nếu sự kiện đó`B x`, so sánh dung lượng xe buýt với kích thước hàng đợi hiện tại. 
5. Nếu`x > wait`, thì vẫn còn một số ghế sau khi tất cả hành khách bình thường đã lên máy bay. đầu ra`YES`. 

Monocarp luôn có thể chọn bỏ qua mọi cơ hội trước đó và vẫn có mặt trên chiếc xe buýt này. 
6. Nếu không thì xuất ra`NO`. 

Mọi chỗ ngồi đều dành cho hành khách bình thường nên Monocarp không bao giờ có cơ hội vào. 
7. Cập nhật hàng đợi hành khách sau khi xe buýt rời đi bằng cách cài đặt`wait = max(0, wait - x)`. 

Điều này loại bỏ chính xác những hành khách đã lên máy bay. 

### Tại sao nó hoạt động 

Bất biến quan trọng là`wait`luôn bằng số lượng hành khách thông thường hiện đang chờ ở điểm dừng. 

Monocarp không bao giờ ảnh hưởng đến số lượng này vì hành khách bình thường lên máy bay trước anh ta và các quyết định của anh ta không thay đổi số lượng hành khách bình thường đến hoặc rời đi. 

Đối với xe buýt có`b`chỗ ngồi, hành khách bình thường chiếm chính xác`min(wait, b)`chỗ ngồi. Một chỗ ngồi miễn phí tồn tại cho Monocarp chính xác khi$$b - \min(wait, b) > 0$$tương đương với`b > wait`. 

Nếu như`b > wait`, Monocarp có thể lên xe buýt này. Nếu như`b <= wait`, tất cả chỗ ngồi đã được sử dụng hết trước khi đến lượt anh ta. Vì thuật toán kiểm tra chính xác điều kiện này cho mọi xe buýt trong khi vẫn duy trì số lượng hành khách chính xác nên mọi câu trả lời đều đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    wait = 0
    ans = []

    for _ in range(n):
        typ, val = input().split()
        val = int(val)

        if typ == 'P':
            wait += val
        else:
            if val > wait:
                ans.append("YES")
            else:
                ans.append("NO")

            wait = max(0, wait - val)

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Biến`wait`lưu trữ chính xác số lượng hành khách bình thường hiện đang chờ đợi. 

Đối với các sự kiện hành khách, chúng tôi chỉ cần thêm vào số lượng này. 

Đối với các sự kiện xe buýt, trước tiên chúng tôi trả lời truy vấn bằng kích thước hàng đợi hiện tại. Thứ tự quan trọng. Quyết định phụ thuộc vào số lượng hành khách đang đợi trước khi có ai lên xe buýt đến. 

Sau khi đưa ra câu trả lời, chúng tôi cập nhật hàng đợi. Nếu xe buýt có đủ chỗ ngồi, tất cả hành khách đang chờ sẽ rời đi và`wait`trở thành số không. Nếu không thì chính xác`val`hành khách lên tàu và`wait`giảm dần`val`. 

Tất cả số học dễ dàng phù hợp với số nguyên Python. Mặc dù một sự kiện đơn lẻ có thể cộng lại thành$10^6$hành khách, tổng số vẫn thấp hơn nhiều so với giới hạn của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10
P 2
P 5
B 8
P 14
B 5
B 9
B 3
P 2
B 1
B 2
```| Sự kiện | đợi trước | Hành động | Đầu ra | đợi sau | 
| --- | --- | --- | --- | --- | 
| P 2 | 0 | thêm hành khách | - | 2 | 
| P 5 | 2 | thêm hành khách | - | 7 | 
| B 8 | 7 | 8 > 7 | CÓ | 0 | 
| P 14 | 0 | thêm hành khách | - | 14 | 
| B 5 | 14 | 5 14 | KHÔNG | 9 | 
| B 9 | 9 | 9 9 9 | KHÔNG | 0 | 
| B 3 | 0 | 3 > 0 | CÓ | 0 | 
| P 2 | 0 | thêm hành khách | - | 2 | 
| B 1 | 2 | 1 2 | KHÔNG | 1 | 
| B 2 | 1 | 2 > 1 | CÓ | 0 | 

Đầu ra:```
YES
NO
NO
YES
NO
YES
```Dấu vết này cho thấy chỉ có hàng đợi hành khách hiện tại mới quan trọng. Những lựa chọn trước đây của Monocarp không bao giờ ảnh hưởng đến số chỗ còn trống trong tương lai. 

### Ví dụ 2 

đầu vào:```
5
P 10
B 3
B 8
P 1
B 1
```| Sự kiện | đợi trước | Hành động | Đầu ra | đợi sau | 
| --- | --- | --- | --- | --- | 
| P 10 | 0 | thêm hành khách | - | 10 | 
| B 3 | 10 | 3 10 | KHÔNG | 7 | 
| B 8 | 7 | 8 > 7 | CÓ | 0 | 
| P 1 | 0 | thêm hành khách | - | 1 | 
| B 1 | 1 | 1 1 | KHÔNG | 0 | 

Đầu ra:```
NO
YES
NO
```Ví dụ này minh họa trường hợp xe buýt chỉ loại bỏ một phần hàng đợi. Những hành khách còn lại tiếp tục chờ đợi và ảnh hưởng đến các chuyến xe sau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi sự kiện được xử lý chính xác một lần | 
| Không gian | O(1) | Chỉ một số biến được duy trì, ngoại trừ lưu trữ đầu ra | 

Với tối đa 1000 sự kiện, giải pháp tuyến tính dễ dàng nằm trong giới hạn thời gian. Việc sử dụng bộ nhớ là không đổi và không đáng kể so với 512 MB có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    wait = 0
    ans = []

    for _ in range(n):
        typ, val = input().split()
        val = int(val)

        if typ == "P":
            wait += val
        else:
            ans.append("YES" if val > wait else "NO")
            wait = max(0, wait - val)

    return "\n".join(ans)

# provided sample
assert run(
"""10
P 2
P 5
B 8
P 14
B 5
B 9
B 3
P 2
B 1
B 2
"""
) == """YES
NO
NO
YES
NO
YES""", "sample 1"

# minimum input
assert run(
"""1
B 1
"""
) == "YES", "single bus"

# exact equality
assert run(
"""2
P 5
B 5
"""
) == "NO", "no seat remains for Monocarp"

# passengers remain after first bus
assert run(
"""3
P 10
B 3
B 8
"""
) == """NO
YES""", "carry-over queue"

# consecutive buses with empty stop
assert run(
"""3
B 1
B 2
B 3
"""
) == """YES
YES
YES""", "all buses available"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`B 1`|`YES`| Đầu vào hợp lệ nhỏ nhất | 
|`P 5, B 5`|`NO`| Trường hợp bình đẳng, không có ghế dự phòng | 
|`P 10, B 3, B 8`|`NO YES`| Hành khách còn lại vẫn kiên trì | 
|`B 1, B 2, B 3`|`YES YES YES`| Hàng đợi trống trên nhiều xe buýt | 

## Vỏ cạnh 

Xét trường hợp đẳng thức:```
2
P 5
B 5
```Trước khi xe buýt tới,`wait = 5`. Xe buýt có đúng năm chỗ ngồi. Tất cả năm ghế đều dành cho hành khách bình thường, không còn chỗ nào. Thuật toán kiểm tra`5 > 5`, sai và kết quả đầu ra:```
NO
```Điều này tránh được lỗi phổ biến khi sử dụng`>=`thay vì`>`. 

Hãy xem xét một hàng đợi được phục vụ một phần:```
3
P 10
B 3
B 8
```Ban đầu`wait = 10`. Sau bus đầu tiên, thuật toán xuất ra`NO`và cập nhật`wait`ĐẾN`7`. Đối với xe buýt thứ hai,`8 > 7`, vì vậy nó xuất ra`YES`. Những hành khách còn lại trên chuyến xe buýt đầu tiên được bảo toàn chính xác. 

Xem xét nhiều xe buýt có sẵn:```
3
P 2
B 3
B 1
```Ở chuyến xe buýt đầu tiên,`3 > 2`, vậy câu trả lời là`YES`. Sau khi xe buýt khởi hành, hàng đợi trở nên trống rỗng. Ở chuyến xe buýt thứ hai,`1 > 0`, vậy đáp án cũng là`YES`. 

Điều này xác nhận rằng chúng tôi không mô phỏng một quyết định cố định duy nhất của Monocarp. Mỗi xe buýt được đánh giá độc lập theo câu hỏi "Monocarp có thể chọn đi xe buýt này không?", Đó chính xác là những gì vấn đề yêu cầu.
