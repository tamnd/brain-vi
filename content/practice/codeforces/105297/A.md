---
title: "CF 105297A - Nauryz"
description: "Có một thiết bị nghe nhạc duy nhất phát các bài hát lần lượt. Mỗi khách đến vào một thời điểm cụ thể, chọn một bài hát có thời lượng cố định và bài hát đó thường được thêm vào cuối hàng đợi phát lại."
date: "2026-06-23T06:29:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105297
codeforces_index: "A"
codeforces_contest_name: "2024 USP Try-outs"
rating: 0
weight: 105297
solve_time_s: 61
verified: true
draft: false
---

[CF 105297A - Nauryz](https://codeforces.com/problemset/problem/105297/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có một thiết bị nghe nhạc duy nhất phát các bài hát lần lượt. Mỗi khách đến vào một thời điểm cụ thể, chọn một bài hát có thời lượng cố định và bài hát đó thường được thêm vào cuối hàng đợi phát lại. 

Thời gian trôi về phía trước và thiết bị liên tục phát bất kỳ nội dung nào hiện đang hoạt động. Khi một bài hát kết thúc, bài hát tiếp theo trong hàng đợi sẽ bắt đầu ngay lập tức, không có độ trễ. 

Điều thú vị là một số khách có thể nhấn một nút đặc biệt vào thời điểm họ chọn bài hát của mình. Nếu họ làm vậy, bài hát của họ sẽ không hoạt động giống như một bài hát xếp hàng thông thường. Thay vào đó, nó ngay lập tức thay thế bất kỳ nội dung nào hiện đang phát tại thời điểm chính xác đó và quá trình phát lại sẽ chuyển ngay sang bài hát của họ. Bài hát bị gián đoạn sẽ bị loại bỏ và sẽ không bao giờ tiếp tục. Điều quan trọng là mọi thứ vẫn đang chờ trong hàng đợi vẫn không thay đổi. 

Một vị khách chỉ trở nên không hài lòng nếu bài hát của họ đang được phát tích cực và một vị khách khác sử dụng nút ngắt này vào thời điểm nghiêm ngặt trước khi bài hát của họ kết thúc. Nếu sự gián đoạn xảy ra chính xác vào thời điểm bài hát kết thúc thì điều đó không được tính là sự gián đoạn. 

Nhiệm vụ là xác định xem bài hát của khách nào bị gián đoạn ít nhất một lần. 

Kích thước đầu vào có thể lên tới 100.000 sự kiện, điều này ngay lập tức loại trừ mọi mô phỏng khởi động lại từ đầu cho mỗi sự kiện hoặc quét hàng đợi nhiều lần. Bất kỳ giải pháp nào tệ hơn tuyến tính hoặc gần tuyến tính trong thực tế sẽ gặp khó khăn trong thời hạn. Cấu trúc cũng gợi ý rằng chúng ta đang xử lý một dòng thời gian phát triển duy nhất, do đó cần phải mô phỏng hoặc xử lý sự kiện hiệu quả. 

Một trường hợp phức tạp phát sinh khi nhiều khách tương tác trong khi không có bài hát nào được phát. Trong trường hợp đó, dù khách có dùng nút ngắt thì cũng không có gì để ngắt nên không ai thấy khó chịu. Một trường hợp phức tạp khác là khi ngắt xảy ra chính xác ở ranh giới bài hát. Vì bài hát đã kết thúc nên không được coi là gián đoạn. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ duy trì một hàng bài hát rõ ràng và mô phỏng thời gian từng giây hoặc từng sự kiện, kiểm tra mọi thời điểm xem bài hát có kết thúc hay không hoặc liệu khách có làm gián đoạn hay không. Điều này hoạt động về mặt khái niệm vì hệ thống có tính xác định: các bài hát phát theo thứ tự và các ngắt ngay lập tức thay thế bài hát hiện tại. Tuy nhiên, mô phỏng ở mức độ chi tiết cao là quá chậm vì thời lượng và thời gian của bài hát lên tới 10^7, nghĩa là mô phỏng có thể yêu cầu tới 10^12 thao tác trong trường hợp xấu nhất. 

Quan sát quan trọng là không có gì xảy ra giữa các sự kiện ngoại trừ sự tiến triển liên tục của thời gian và sự hoàn thành mang tính quyết định của các bài hát. Chúng ta chỉ cần phản ứng vào những thời điểm riêng biệt: khi có khách đến và khi bài hát kết thúc. Điều này cho phép chúng tôi duy trì một con trỏ duy nhất tới bài hát hiện tại và hàng đợi các bài hát đang chờ, đồng thời tăng thời gian theo bước nhảy thay vì từng bước. 

Thông tin chi tiết thứ hai là các ngắt chỉ ảnh hưởng đến bài hát hiện đang phát chứ không bao giờ ảnh hưởng đến hàng đợi. Điều này có nghĩa là chúng tôi không bao giờ cần sửa đổi các bài hát đã lên lịch trong tương lai ngoại trừ việc thêm những bài hát mới. Do đó, hệ thống có thể được mô hình hóa thành một dòng thời gian duy nhất với trạng thái hiện tại, hàng đợi FIFO và các thay thế bắt buộc không thường xuyên. 

Bằng cách mô phỏng tiến trình thời gian giữa các sự kiện liên tiếp, chúng tôi có thể đảm bảo rằng bài hát hiện tại luôn chính xác tại mỗi thời điểm đến và chúng tôi chỉ thực hiện công việc liên tục cho mỗi sự kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng từng bước ngây thơ | O(tổng thời gian) | O(n) | Quá chậm | 
| Mô phỏng hàng đợi theo sự kiện | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ba phần trạng thái: bài hát hiện tại (nếu có), thời gian kết thúc theo lịch trình và hàng đợi các bài hát đang chờ được thêm vào mà không bị gián đoạn. Chúng tôi cũng duy trì một con trỏ thời gian hiện tại để theo dõi khoảng cách chúng tôi đã mô phỏng.

1. Sắp xếp hoặc giả sử các sự kiện đã theo thứ tự thời gian tăng dần. Vì bài toán đảm bảo thời gian phân biệt nên chúng ta có thể xử lý theo thứ tự đầu vào. 
2. Trước khi xử lý một sự kiện tại thời điểm đó`t`, tiến hành mô phỏng từ thời điểm hiện tại lên tới`t`. Trong khoảng thời gian này, hãy liên tục kết thúc bài hát hiện tại nếu nó kết thúc trước hoặc vào lúc`t`, sau đó bắt đầu ngay bài hát tiếp theo trong hàng đợi. Điều này đảm bảo rằng tại thời điểm`t`, trạng thái bài hát hiện tại là chính xác. 
3. Nếu không có bài hát hiện tại vào thời điểm đó`t`và hàng đợi không trống, hãy bắt đầu bài hát được xếp hàng tiếp theo ngay lập tức. Thời gian kết thúc của nó được tính là`t + duration`. 
4. Xử lý sự kiện theo thời gian`t`. Nếu khách không sử dụng nút (`c = 0`), hãy thêm bài hát của họ vào hàng đợi. Cuối cùng nó sẽ phát sau khi bài hát trước đó kết thúc. 
5. Nếu khách sử dụng nút (`c = 1`), kiểm tra xem bài hát hiện có đang phát hay không và liệu nó có kết thúc hoàn toàn sau đó không.`t`. Nếu vậy, hãy đánh dấu chủ nhân của bài hát hiện tại là buồn vì nó bị gián đoạn. 
6. Thay thế bài hát hiện tại bằng bài hát mới ngay lập tức, đặt thời gian kết thúc của bài hát đó thành`t + duration`. Bài hát này không vào hàng đợi. 
7. Tiếp tục sự kiện tiếp theo. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, trạng thái hệ thống hoàn toàn được xác định bởi bài hát hiện tại và hàng đợi các bài hát đang chờ xử lý. Giữa các sự kiện, hệ thống phát triển một cách xác định mà không cần đầu vào bên ngoài, vì vậy chúng ta có thể chuyển nhanh thời gian đến ranh giới sự kiện tiếp theo một cách an toàn mà không làm mất thông tin. Mọi gián đoạn đều được xử lý chính xác một lần tại thời điểm nó xảy ra và không có sự kiện nào trong tương lai có thể ảnh hưởng trở lại quá trình phát lại trong quá khứ. Điều này đảm bảo rằng mọi bài hát bị gián đoạn đều được tính chính xác khi nó được thay thế trong khi vẫn đang phát. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n = int(input())
    events = []
    for i in range(n):
        t, m, c = map(int, input().split())
        events.append((t, m, c, i + 1))

    queue = deque()
    sad = set()

    cur_id = 0
    cur_end = 0
    cur_dur = 0

    # we also need to know current owner id; store it separately
    cur_owner = None

    time = 0

    def start_next(t):
        nonlocal cur_owner, cur_end, cur_dur
        if queue:
            oid, dur = queue.popleft()
            cur_owner = oid
            cur_dur = dur
            cur_end = t + dur
        else:
            cur_owner = None
            cur_end = t

    for t, m, c, idx in events:
        # advance time to t
        while cur_owner is not None and cur_end <= t:
            time = cur_end
            start_next(time)

        time = t

        # if no current song, start from queue
        if cur_owner is None:
            start_next(t)

        if c == 0:
            queue.append((idx, m))
        else:
            # interrupt if currently playing and not already finished at t
            if cur_owner is not None and cur_end > t:
                sad.add(cur_owner)

            cur_owner = idx
            cur_dur = m
            cur_end = t + m

    print(len(sad))
    if sad:
        print(*sad)
    else:
        print()

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một hàng các bài hát đang chờ xử lý và một bài hát đang hoạt động với thời gian kết thúc theo lịch trình. Logic trợ giúp`start_next`đảm bảo rằng bất cứ khi nào bài hát hiện tại kết thúc, bài hát tiếp theo trong hàng đợi sẽ bắt đầu ngay vào đúng thời điểm. 

Sự tinh tế quan trọng là điều kiện`cur_end > t`khi quyết định có đánh dấu một bài hát là bị gián đoạn hay không. Nếu một bài hát kết thúc đúng lúc`t`, được phép làm cho sạch sẽ nên không được tính là buồn. 

Một chi tiết quan trọng khác là các bài hát bị gián đoạn sẽ không bao giờ được chèn lại ở bất kỳ đâu. Chúng biến mất hoàn toàn, trong khi các bài hát được xếp hàng đợi vẫn không bị ảnh hưởng. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản khi một bài hát đang được phát và một vị khách khác làm gián đoạn bài hát đó. 

### Ví dụ 1 

đầu vào:```
3
1 5 0
2 4 1
10 3 0
```| Thời gian | Sự kiện | Chủ sở hữu hiện tại | Thời gian kết thúc | Xếp hàng | Bộ buồn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cộng (1,5) | 1 | 6 | [] | {} | 
| 2 | ngắt (2,4) | 2 thay thế 1 | 6 (kết thúc mới ở số 6? thực tế là 2+4=6) | [] | {1} | 
| 10 | cộng (3,3) | 2 xong sớm hơn, 3 xếp hàng rồi chơi | 13 | [] | {1} | 

Bài hát đầu tiên bị gián đoạn ở nhịp thứ 2 nên chủ nhân của nó trở nên buồn bã. Bài hát thứ hai kết thúc bình thường và bài hát thứ ba không bao giờ bị gián đoạn. 

### Ví dụ 2 

đầu vào:```
4
1 3 0
2 2 0
5 4 1
7 1 0
```| Thời gian | Sự kiện | Chủ sở hữu hiện tại | Thời gian kết thúc | Xếp hàng | Bộ buồn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | thêm A | A | 4 | [] | {} | 
| 2 | thêm B | A | 4 | [B] | {} | 
| 5 | ngắt C | C thay thế B? thực ra A đã chơi xong, B đang chơi | 7 | [] | {B} | 
| 7 | thêm D | C | 9 | [D] | {B} | 

Điều này chứng tỏ rằng sự gián đoạn phụ thuộc vào bài hát đang hoạt động hiện tại tại thời điểm chính xác chứ không phụ thuộc vào vị trí hàng đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bài hát được bắt đầu và kết thúc nhiều nhất một lần và mỗi sự kiện được xử lý một lần | 
| Không gian | O(n) | Xếp hàng lưu trữ tối đa n bài hát đang chờ xử lý và tập hợp buồn lưu trữ những bài bị gián đoạn | 

Thuật toán xử lý mỗi khách chính xác một lần và tăng thời gian nhảy giữa các sự kiện hoặc hoàn thành bài hát, giữ cho tổng công việc tuyến tính theo số lượng đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n = int(input())
    events = []
    for i in range(n):
        t, m, c = map(int, input().split())
        events.append((t, m, c, i + 1))

    queue = deque()
    sad = set()

    cur_owner = None
    cur_end = 0

    def start_next(t):
        nonlocal cur_owner, cur_end
        if queue:
            oid, dur = queue.popleft()
            cur_owner = oid
            cur_end = t + dur
        else:
            cur_owner = None
            cur_end = t

    time = 0

    for t, m, c, idx in events:
        while cur_owner is not None and cur_end <= t:
            time = cur_end
            start_next(time)

        time = t

        if cur_owner is None:
            start_next(t)

        if c == 0:
            queue.append((idx, m))
        else:
            if cur_owner is not None and cur_end > t:
                sad.add(cur_owner)
            cur_owner = idx
            cur_end = t + m

    out = [str(len(sad))]
    if sad:
        out.append(" ".join(map(str, sad)))
    else:
        out.append("")
    return "\n".join(out)

# provided sample style tests
assert run("3\n1 5 0\n2 4 1\n10 3 0\n").split()[0] == "1"
assert run("1\n1 1 0\n") == "0\n"

# boundary cases
assert run("2\n1 10 0\n2 1 1\n").split()[0] == "1"
assert run("3\n1 2 1\n2 2 1\n3 2 1\n")  # no crash
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| single non-interrupt | 0 | không có khách buồn khi không xảy ra gián đoạn | 
| immediate interrupt | 1 + index | phát hiện chính xác sự gián đoạn khi bắt đầu | 
| chained interrupts | nhiều | bền bỉ khi thay thế nhiều lần | 
| all ci=1 | all applicable | căng thẳng của việc ghi đè lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi sự gián đoạn xảy ra chính xác vào thời điểm bài hát kết thúc. Trong hoàn cảnh đó, điều kiện`cur_end > t`đảm bảo bài hát không bị coi là bị gián đoạn. Ví dụ: nếu một bài hát kết thúc ở thời điểm thứ 5 và một vị khách khác sử dụng nút ở thời điểm thứ 5 thì bài hát đầu tiên đã kết thúc nên không có nỗi buồn nào được ghi lại. 

Một trường hợp khác xảy ra khi có nhiều khách đến trong khi không có bài hát nào được phát và hàng đợi trống. Trong trường hợp này, mỗi bài hát không bị gián đoạn sẽ bắt đầu ngay lập tức và các phần ngắt chỉ cần thay thế bài hát hiện tại mà không ảnh hưởng đến bất kỳ trạng thái ẩn nào. Thuật toán xử lý việc này vì`cur_owner`trở thành`None`và chúng tôi chỉ chuyển đổi khi có bài hát hoạt động hợp lệ. 

Trường hợp thứ ba là sự gián đoạn nhanh chóng liên tiếp. Vì mỗi ngắt sẽ thay thế trực tiếp bài hát hiện tại và chúng tôi chỉ theo dõi một bài hát đang hoạt động nên các bản cập nhật lặp lại sẽ không tích lũy lỗi. Mỗi lần thay thế được xử lý độc lập tại dấu thời gian chính xác của nó và bài hát đang hoạt động trước đó được ghi lại là bài buồn đúng một lần trước khi bị loại bỏ.
