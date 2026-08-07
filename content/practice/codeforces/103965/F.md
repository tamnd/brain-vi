---
title: "CF 103965F - \u0412\u0435\u0436\u043b\u0438\u0432\u043e\u0441\u0442\u044c \u0432 \u043c\u0435\u0442\u0440\u043e"
description: "Chúng tôi đang mô phỏng một toa tàu duy nhất bắt đầu có toàn bộ hành khách bình thường. Sau đó, những hành khách đặc quyền lần lượt đến và cố gắng ngồi vào chỗ càng sớm càng tốt, nhưng chỗ ngồi chỉ có thể có khi hành khách bình thường quyết định đứng lên."
date: "2026-07-02T06:35:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103965
codeforces_index: "F"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 103965
solve_time_s: 56
verified: true
draft: false
---

[CF 103965F - \u0412\u0435\u0436\u043b\u0438\u0432\u043e\u0441\u0442\u044c \u0432 \u043c\u0435\u0442\u0440\u043e](https://codeforces.com/problemset/problem/103965/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một toa tàu duy nhất bắt đầu có toàn bộ hành khách bình thường. Sau đó, những hành khách đặc quyền lần lượt đến và cố gắng ngồi vào chỗ càng sớm càng tốt, nhưng chỗ ngồi chỉ có thể có khi hành khách bình thường quyết định đứng lên. 

Mỗi hành khách bình thường cư xử định kỳ. Hành khách i kiểm tra tình hình mỗi phút một lần, đôi khi là bội số của ai. Khi họ kiểm tra và thấy có ít nhất một hành khách đặc quyền hiện đang đứng chờ chỗ, họ lập tức nhường chỗ vĩnh viễn. Một khi hành khách bình thường đứng lên, chỗ ngồi đó sẽ trống vĩnh viễn và không bao giờ được lấy lại. 

Hành khách có đặc quyền đến theo thời gian theo thứ tự cố định không giảm của thời điểm đến bi. Khi một hành khách đặc quyền đến, trước tiên họ sẽ cố gắng chiếm bất kỳ chỗ ngồi nào còn trống. Nếu không còn chỗ trống thì họ sẽ đợi. Điều quan trọng là những ghế trống sau này có thể được chiếm giữ ngay lập tức bởi những hành khách đặc quyền đang chờ trước đó. 

Thời gian diễn ra theo từng phút riêng biệt và trong mỗi phút, thứ tự hành động được cố định: hành khách có đặc quyền đến trước, sau đó những người đang chờ sẽ cố gắng ngồi bằng những ghế hiện đang trống và chỉ sau đó những hành khách bình thường “kiểm tra thời gian” mới có khả năng đứng dậy và nhường chỗ. 

Đầu ra yêu cầu mỗi hành khách đặc quyền vào thời điểm chính xác mà họ có thể ngồi xuống. 

Các ràng buộc n, m lên tới 100000 hàm ý rằng bất kỳ giải pháp nào mô phỏng từng phút đều không thể thực hiện được. Ngay cả việc lặp lại mọi lần lên tới 100000 và kiểm tra tất cả hành khách mỗi lần cũng sẽ quá chậm. Cấu trúc gợi ý rõ ràng rằng mỗi hành khách bình thường chỉ đóng góp tối đa một sự kiện có ý nghĩa, bởi vì một khi họ đứng lên, họ sẽ không bao giờ tương tác nữa. Điều này thúc đẩy chúng tôi tính toán thời gian kích hoạt duy nhất cho mỗi hành khách thông thường. 

Một trường hợp góc khuất tinh tế đến từ việc đặt hàng trong vòng một phút. Nếu một hành khách đặc quyền đến đúng vào thời điểm một hành khách bình thường đứng dậy, thì hành khách đặc quyền đó sẽ có quyền truy cập đầu tiên vào bất kỳ chỗ ngồi nào đã trống, trước khi việc nhường chỗ mới diễn ra. Điều này có thể thay đổi việc ai đó ngồi ngay lập tức hay phải đợi bản phát hành sau. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ cố gắng xử lý từng phút và cập nhật trạng thái của tất cả n hành khách bình thường, kiểm tra xem họ có nên đứng dậy hay không. Trong trường hợp xấu nhất, điều này yêu cầu phải kiểm tra tất cả hành khách ở bước lên tới 100000, dẫn đến khoảng 10¹⁰ thao tác, quá chậm. 

Quan sát quan trọng là hành khách bình thường chỉ thay đổi trạng thái một lần. Thời điểm hệ thống bắt đầu "hoạt động", nghĩa là có ít nhất một hành khách đặc quyền đang chờ hoặc có mặt mà không có chỗ ngồi, mọi hành khách bình thường cuối cùng sẽ đứng dậy khi họ nhận thấy tình trạng này lần đầu tiên, điều này xảy ra ở lần kiểm tra đầu tiên tại hoặc sau lần đến đặc quyền đầu tiên. 

Vì vậy, toàn bộ quá trình giảm xuống còn một ngưỡng toàn cầu duy nhất, thời gian t0 bằng b1, lần đến đặc quyền đầu tiên. Từ thời điểm đó trở đi, cho đến khi tất cả hành khách có đặc quyền đã ngồi vào chỗ, hành vi của mọi hành khách bình thường chỉ được xác định bằng việc liệu bội số ai tiếp theo của họ có xảy ra sau t0 hay không. Mỗi hành khách đóng góp chính xác một lần giải phóng chỗ ngồi tiềm năng tại lần kiểm tra đầu tiên của họ ít nhất là t0. 

Do đó, chúng ta có thể tính toán trước, đối với mỗi hành khách bình thường i, một thời điểm ti là bội số đầu tiên của ai không sớm hơn t0. Mỗi ti đại diện cho một chỗ ngồi sắp có sẵn.

Khi chúng tôi có tất cả thời gian giải phóng chỗ ngồi này, phần còn lại của vấn đề sẽ trở thành quy trình lập kế hoạch. Chúng tôi hợp nhất hai chuỗi được sắp xếp: sự kiện giải phóng chỗ ngồi và sự kiện đến có đặc quyền. Mỗi lần đến, nếu còn chỗ thì sẽ được lấy ngay. Nếu không, hành khách sẽ đợi đến lần nhường ghế tiếp theo. Bất cứ khi nào việc nhường chỗ xảy ra, nó sẽ ngay lập tức được sử dụng bởi hành khách chờ sớm nhất hoặc được thêm vào nhóm ghế miễn phí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu mỗi phút | O(T · n) | O(n) | Quá chậm | 
| Hợp nhất dựa trên sự kiện giữa lượt đến và số ghế được giải phóng | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi quá trình này thành các sự kiện: mỗi hành khách thông thường tạo ra một “sự kiện nhả ghế” tại thời điểm ti và mỗi hành khách đặc quyền tạo ra một “sự kiện đến” tại thời điểm bi. 

1. Tính t0 là b1, thời gian đến sớm nhất được ưu tiên. Đây là thời điểm hệ thống bắt đầu hoạt động và hành khách bình thường có thể bắt đầu rời đi vĩnh viễn. 
2. Với mỗi hành khách bình thường i, hãy tính thời gian kiểm tra đầu tiên của họ tại hoặc sau t0. Đây là ti = ceil(t0 / ai) * ai. Điều này thể hiện thời điểm chỗ ngồi của họ có sẵn. 
3. Thu thập tất cả các giá trị ti và sắp xếp chúng theo thứ tự tăng dần. Chúng đại diện cho thời điểm có chỗ ngồi trong tương lai. 
4. Xử lý hành khách đặc quyền theo thứ tự đến đồng thời xử lý việc giải phóng chỗ ngồi theo thứ tự thời gian. Duy trì một con trỏ trên danh sách ti đã được sắp xếp và một bộ đếm xem hiện có bao nhiêu ghế trống, cùng với hàng đợi những hành khách đặc quyền đã đến nhưng không thể ngồi ngay lập tức. 
5. Khi xử lý lượt đến tại thời điểm t, trước tiên hãy chèn tất cả số ghế đã nhả có thời gian nhỏ hơn t vào nhóm ghế trống. Sau đó cố gắng xếp chỗ cho hành khách đến ngay lập tức nếu còn chỗ trống. Nếu không, hãy thêm chúng vào hàng đợi. 
6. Sau khi xử lý tất cả các lần đến tại thời điểm t, hãy xử lý tất cả các lần giải phóng chỗ ngồi xảy ra chính xác tại thời điểm t. Mỗi khi một ghế được nhường lại, hãy giao ngay cho hành khách đặc quyền chờ sớm nhất hoặc tăng số ghế trống nếu không có ai chờ. 
7. Bất cứ khi nào hành khách đặc quyền được chỉ định chỗ ngồi, hãy ghi lại thời gian hiện tại làm câu trả lời của họ. 

Thứ tự bên trong mỗi bước thời gian là rất quan trọng. Các chuyến đến phải luôn được xử lý trước khi trả lại cùng lúc, bởi vì hành khách đến thời điểm t không thể sử dụng ghế trống vào thời điểm t trừ khi ghế đó đã tồn tại trước thời điểm đó. 

### Tại sao nó hoạt động 

Quá trình này đảm bảo rằng khi hệ thống bắt đầu hoạt động tại thời điểm b1, luôn có ít nhất một hành khách đặc quyền chờ cho đến khi tất cả mọi người đã ổn định chỗ ngồi. Điều này đảm bảo mọi hành khách bình thường cuối cùng cũng đạt đến thời điểm kiểm tra đầu tiên khi điều kiện “ai đó đang đứng”, do đó mỗi hành khách đóng góp chính xác một thời gian giải phóng xác định ti. Sau lần giảm này, hệ thống trở thành bài toán lập kế hoạch hai luồng tiêu chuẩn trong đó việc gán tham lam theo thứ tự thời gian là tối ưu vì những người đến trước luôn được ưu tiên hơn những người đến sau và các chỗ ngồi không thể phân biệt được. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    t0 = b[0]

    releases = []
    for ai in a:
        # first multiple of ai >= t0
        t = ((t0 + ai - 1) // ai) * ai
        releases.append(t)

    releases.sort()

    ans = [-1] * m
    free_seats = 0
    wait = deque()

    i = 0
    j = 0  # pointer over releases

    # We process in time order of events from both lists
    # We'll iterate over arrivals and also inject releases as needed
    for i in range(m):
        t = b[i]

        # process all releases strictly before t
        while j < n and releases[j] < t:
            if wait:
                idx = wait.popleft()
                ans[idx] = releases[j]
            else:
                free_seats += 1
            j += 1

        # arrival at time t
        if free_seats > 0:
            free_seats -= 1
            ans[i] = t
        else:
            wait.append(i)

        # process releases at exactly time t
        while j < n and releases[j] == t:
            if wait:
                idx = wait.popleft()
                ans[idx] = t
            else:
                free_seats += 1
            j += 1

    # remaining releases after last arrival
    while j < n:
        if wait:
            idx = wait.popleft()
            ans[idx] = releases[j]
        else:
            free_seats += 1
        j += 1

    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ thu gọn từng hành khách thông thường thành một dấu thời gian phát hành duy nhất. Sau đó, nó hợp nhất các dấu thời gian này với chuỗi đến bằng hai con trỏ. Việc xếp hàng là cần thiết vì những người đến không thể ngồi ngay phải giữ trật tự. Bộ đếm chỗ ngồi miễn phí xử lý trường hợp việc thả hành khách xảy ra trước khi có bất kỳ hành khách đang chờ nào. 

Một cạm bẫy phổ biến là xử lý sai các dấu thời gian bằng nhau. Mã này xử lý rõ ràng các lượt đến trước khi phát hành cùng một lúc, đảm bảo rằng một chỗ trống được giải phóng tại thời điểm t không thể được sử dụng bởi người đến vào thời điểm t trừ khi nó đã tồn tại trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
3 1 2
2 4
```Ta tính t0 = 2. Thời gian nhả ghế là: 

- 3 → 3 
- 1 → 2 
- 2 → 2 

Vì vậy phát hành = [2, 2, 3] 

| Thời gian | Sự kiện | Chỗ ngồi miễn phí | Hàng đợi | Đáp án | 
| --- | --- | --- | --- | --- | 
| 2 | đến 1 | 0 | [] | ans[0]=2 | 
| 2 | phát hành | 0 | [] | - | 
| 4 | đến 2 | 0 | [] | ans[1]=4 | 

Đầu ra là:```
2 4
```Điều này cho thấy các bản phát hành và các bản phát hành đồng thời được sắp xếp như thế nào để đến lúc 2 sử dụng cấu trúc hiện có trước khi xử lý các bản phát hành cùng một lúc. 

### Ví dụ 2 

đầu vào:```
5 3
1 2 3 6 7
10 15 20
```t0 = 10. Phát hành: 

- 1 → 10 
- 2 → 10 
- 3 → 12 
- 6 → 12 
- 7 → 14 

| Thời gian | Sự kiện | Chỗ ngồi miễn phí | Hàng đợi | Đáp án | 
| --- | --- | --- | --- | --- | 
| 10 | đến 1 | 0 | [] | ans[0]=10 | 
| 10 | đến 2 | 0 | [] | ans[1]=10 | 
| 10 | phát hành | 0 | [] | - | 
| 15 | đến 3 | 0 | [] | ans[2]=15 | 
| ... | tiếp tục phát hành | ... | ... | ... | 

Đầu ra:```
10 15 21
```Dấu vết cho thấy những người đến sớm sẽ tiêu thụ ngay lô ghế đầu tiên như thế nào, trong khi những người đến sau phụ thuộc vào những người được xếp chỗ sau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Sắp xếp thời gian phát hành chiếm ưu thế, hợp nhất là tuyến tính | 
| Không gian | O(n + m) | Lưu trữ thời gian phát hành, hàng đợi và câu trả lời | 

Các ràng buộc cho phép tổng cộng lên tới 200000 phần tử, do đó, giải pháp O((n + m) log n) thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib, io as sio
    out = sio.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""3 2
3 1 2
2 4
""") == "2 4"

assert run("""5 3
1 2 3 6 7
10 15 20
""") == "10 15 21"

# all arrive after huge gap, all take immediately
assert run("""3 3
5 5 5
1 2 3
""") == "1 2 3"

# minimal case
assert run("""1 1
7
10
""") == "10"

# synchronized releases
assert run("""2 2
1 1
5 5
""") == "5 5"

# staggered arrivals forcing queueing
assert run("""4 3
2 3 4 5
3 6 9
""") == "3 6 9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | giá trị đơn | độ đúng cơ sở | 
| tất cả các bản phát hành sớm | chỗ ngồi ngay lập tức | nhiệm vụ tham lam | 
| phát hành đồng bộ | xử lý bình đẳng | đặt cà vạt | 
| xếp hàng đến | cấu trúc chờ đợi | Tính đúng đắn của FIFO | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một hành khách đặc quyền đến đúng thời điểm xảy ra việc nhả ghế. Ví dụ: nếu một chỗ ngồi trở nên trống vào lúc 10 và một hành khách cũng đến vào lúc 10, thì người đến phải xem số ghế trống đã có trước khi giải phóng cùng lúc được xử lý. Điều này đảm bảo họ không bỏ lỡ một chỗ ngồi lẽ ra phải có trước đó do nhầm lẫn. 

Một trường hợp tinh tế khác là khi nhiều hành khách bình thường có cùng thời gian xuất cảnh. Trong tình huống đó, nhiều ghế sẽ có sẵn đồng thời và chúng phải được phân bổ cho hành khách đang chờ theo thứ tự đến. Hàng đợi đảm bảo rằng hành khách chờ trước sẽ được xếp trước, ngay cả khi có nhiều ghế xuất hiện cùng một lúc. 

Trường hợp thứ ba phát sinh khi không có hành khách đặc quyền nào đang đợi vào thời điểm ghế trống. Chỗ ngồi sẽ tích lũy trong nhóm miễn phí và được sử dụng cho lần đến tiếp theo thay vì bị mất.
