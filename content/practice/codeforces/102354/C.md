---
title: "CF 102354C - Chia sẻ tiền"
description: "Chúng tôi xử lý một trình tự thời gian chứa hai loại sự kiện. Giá trị dương có nghĩa là số tiền này sẽ được thêm vào tài khoản chung. Giá trị âm thể hiện yêu cầu vay có kích thước là giá trị tuyệt đối của số đó."
date: "2026-08-16T01:42:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "C"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 163
verified: false
draft: false
---

[CF 102354C - Chia sẻ tiền](https://codeforces.com/problemset/problem/102354/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi xử lý một trình tự thời gian chứa hai loại sự kiện. Giá trị dương có nghĩa là số tiền này sẽ được thêm vào tài khoản chung. Giá trị âm thể hiện yêu cầu vay có kích thước là giá trị tuyệt đối của số đó. Tài khoản bắt đầu từ số 0 và yêu cầu được phê duyệt sẽ ngay lập tức xóa số tiền đó khỏi tài khoản. Yêu cầu chỉ có thể được chấp thuận nếu tài khoản có thể thanh toán tại thời điểm đó. 

Đầu ra có một dòng cho mỗi sự kiện. Một sự kiện tích cực luôn được in dưới dạng`resupplied`. Đối với một yêu cầu, chúng tôi in`approved`nếu chúng tôi giữ yêu cầu đó trong tập hợp được chấp nhận và`declined`nếu không thì. Mục tiêu là giảm thiểu tổng số yêu cầu bị từ chối chứ không phải tổng số tiền đã vay. 

Có thể có tối đa (10^5) yêu cầu và (10^5) nguồn cung cấp lại, do đó chuỗi sự kiện có thể chứa các phần tử (2\cdot10^5). Thuật toán bậc hai có thể thực hiện khoảng (4\cdot10^{10}) phép tính trong trường hợp xấu nhất, vượt xa giới hạn một giây. Về cơ bản, chúng ta cần phép tính tuyến tính hoặc (O(N\log N)), trong đó (N=n+m). Số tiền có thể đạt tới (10^9) và tổng của chúng có thể đạt khoảng (2\cdot10^{14}), do đó, số dư đang chạy phải sử dụng loại số nguyên có khả năng giữ các giá trị lớn hơn nhiều so với số nguyên 32 bit. Số nguyên Python đã cung cấp điều này một cách an toàn. 

Trường hợp khó khăn đầu tiên là một yêu cầu quá lớn đối với chính tài khoản. Ví dụ,```
1 1
-5
+5
```có đầu ra```
declined
resupplied
```Yêu cầu đầu tiên phải bị từ chối vì tiền đến sau không thể phê duyệt yêu cầu đó. Một giải pháp bất cẩn trước tiên tính toán tổng số lượng của tất cả các nguồn cung cấp lại rồi quyết định những yêu cầu nào được chấp nhận sẽ coi yêu cầu đầu tiên là khả thi một cách không chính xác. 

Một trường hợp tinh vi khác là khi yêu cầu hiện tại có thể bị từ chối, nhưng từ chối yêu cầu trước đó thì tốt hơn. Coi như,```
3 1
+5
-3
-2
-1
```Một đầu ra đúng là```
resupplied
approved
approved
approved
```Ở đây số dư sau hai yêu cầu đầu tiên bằng 0, vì vậy yêu cầu cuối cùng là không thể thực hiện được và thực tế phải bị từ chối. Do đó, đầu ra đúng thay vào đó là```
resupplied
approved
approved
declined
```Một ví dụ rõ ràng hơn là,```
3 1
+5
-4
-3
+10
```Tại`-3`, tài khoản chỉ còn một đơn vị. Chúng ta có thể làm cho tiền tố hiện tại trở nên khả thi bằng cách từ chối yêu cầu trước đó là 4 thay vì yêu cầu hiện tại là 3. Đầu ra có thể là```
resupplied
declined
approved
resupplied
```Một quy tắc tham lam luôn từ chối yêu cầu hiện tại bất cứ khi nào không đủ tiền sẽ làm mất đi một yêu cầu được chấp nhận không cần thiết. 

Cũng có trường hợp phải xóa một số yêu cầu tại một sự kiện. Ví dụ,```
3 1
+3
-2
-2
-2
```Sau khi chấp nhận hai yêu cầu đầu tiên, số dư sẽ âm khi yêu cầu thứ ba đến. Ở đây, việc xóa yêu cầu lớn nhất được chấp nhận là đủ, nhưng nói chung có thể cần phải xóa một số yêu cầu. Việc triển khai phải tiếp tục loại bỏ các yêu cầu đã được chấp nhận cho đến khi số dư không âm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là quyết định một cách độc lập xem mọi yêu cầu có được chấp thuận hay từ chối hay không. Với (n) yêu cầu, có thể có (2^n) tập hợp con. Đối với mỗi tập hợp con, chúng tôi có thể quét toàn bộ chuỗi sự kiện và kiểm tra xem các yêu cầu được chấp nhận của nó có khả thi ở mọi thời điểm hay không. Điều đó mang lại các hoạt động (O((n+m)2^n)). Với (n=10^5), ngay cả (2^n) cũng vô vọng, vì vậy phương pháp này chỉ hữu ích để hiểu mục tiêu tối ưu hóa. 

Quan sát chính là tính khả thi chỉ phụ thuộc vào tổng số yêu cầu được chấp nhận cho đến nay. Giả sử tổng lượng hàng tiếp tế nhận được cho đến nay là (S) và các yêu cầu được chấp nhận trong tiền tố có tổng số lượng (C). Tiền tố khả thi chính xác khi (C\le S). 

Bây giờ hãy xử lý các yêu cầu từ trái sang phải và tạm chấp nhận từng yêu cầu. Nếu việc chấp nhận yêu cầu hiện tại khiến tổng số tiền vượt quá số tiền hiện có thì phải xóa ít nhất một yêu cầu được chấp nhận từ tiền tố đã xử lý. Vì mục tiêu tính các yêu cầu bị từ chối nên chúng tôi muốn xóa chính xác một yêu cầu bất cứ khi nào một lần xóa có thể khôi phục tính khả thi. Trong số tất cả các yêu cầu được chấp nhận, việc loại bỏ yêu cầu lớn nhất sẽ giúp giảm số tiền tiêu thụ nhiều nhất trong khi vẫn phải trả giá bằng việc từ chối yêu cầu đó. 

Sự lựa chọn tham lam này cũng bảo toàn được nhiều tiền nhất cho những yêu cầu trong tương lai. Giả sử hai yêu cầu được chấp nhận có kích thước (a<b). Nếu chúng ta phải từ chối một trong số chúng, việc từ chối (b) sẽ khiến (b-a) có nhiều tiền hơn từ chối (a), trong khi cả hai lựa chọn đều làm tăng số lượng yêu cầu bị từ chối lên đúng một. Số dư còn lại lớn hơn không bao giờ có thể làm cho tính khả thi trong tương lai trở nên tồi tệ hơn. 

Điều đó tự nhiên dẫn đến một vùng heap tối đa chứa tất cả các kích thước yêu cầu hiện được chấp nhận. Khi có một yêu cầu đến, hãy tạm chấp nhận nó và chèn nó vào heap. Nếu số dư trở nên âm, hãy xóa yêu cầu được chấp nhận lớn nhất khỏi vùng nhớ heap và đánh dấu yêu cầu đó là bị từ chối. Lặp lại cho đến khi số dư không âm. Vùng heap cho phép chúng ta tìm yêu cầu được chấp nhận lớn nhất trong (O(\log n)). 

Phương pháp brute-force xem xét mọi tập hợp con được chấp nhận có thể vì nó không có cách nào để nhận ra những quyết định nào có thể thay thế cho nhau. Phương pháp tham lam khai thác thực tế là mọi yêu cầu đều có cùng chi phí về mặt mục tiêu, cụ thể là một lần bị từ chối, trong khi quy mô tiền tệ của chúng khác nhau. Bất cứ khi nào bị buộc phải từ chối, yêu cầu được chấp nhận lớn nhất sẽ là yêu cầu có giá trị nhất để loại bỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((n+m)2^n)) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O((n+m)\log n)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi sự kiện hoàn chỉnh và tạo một mảng đầu ra được khởi tạo theo loại sự kiện. Đối với một sự kiện tích cực, câu trả lời là ngay lập tức`resupplied`; đối với một yêu cầu, ban đầu hãy đánh dấu nó`approved`bởi vì thuật toán tham lam trước tiên sẽ cố gắng chấp nhận mọi yêu cầu. 
2. Duy trì`balance`, số tiền hiện còn lại trong tài khoản và số lượng yêu cầu được chấp nhận tối đa. của Python`heapq`là một đống tối thiểu, vì vậy hãy lưu trữ mỗi số tiền yêu cầu dưới dạng số âm. Lưu trữ chỉ mục sự kiện cùng với số lượng vì việc xóa vùng nhớ heap sau này có thể từ chối yêu cầu trước đó. 
3. Khi xảy ra sự kiện dương có giá trị (x), hãy thêm (x) vào`balance`. Không cần phải loại bỏ thứ gì khỏi đống vì việc cung cấp lại chỉ làm tăng số tiền sẵn có. 
4. Khi xảy ra sự kiện tiêu cực có giá trị (-x), hãy tạm chấp nhận nó. Trừ (x) từ`balance`và chèn (( -x, index)) vào heap. 
5. Nếu`balance`bây giờ là âm, hãy liên tục xóa yêu cầu lớn nhất khỏi heap. Đối với một mục nhập heap biểu thị số tiền (x), hãy thêm (x) trở lại`balance`và thay đổi đầu ra của yêu cầu đó từ`approved`ĐẾN`declined`. 

Yêu cầu đã xóa không nhất thiết phải là yêu cầu hiện tại. Đây là bước tham lam trọng tâm: một lần từ chối có thể lấy lại số tiền lớn nhất có thể, để lại số dư lớn nhất có thể cho các yêu cầu trong tương lai. 
6. Tiếp tục cho đến khi tất cả các sự kiện đã được xử lý. Mọi yêu cầu được chấp nhận vẫn còn trong heap và mọi yêu cầu được xóa khỏi heap đều được đánh dấu`declined`. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào, vùng heap chứa chính xác các yêu cầu mà thuật toán hiện chấp nhận. Điều bất biến là tổng chi phí của họ không bao giờ vượt quá tổng lượng cung cấp lại nhận được trong tiền tố đó, do đó số dư tài khoản luôn không âm. 

Khi một yêu cầu mới gây ra thiếu hụt, mọi giải pháp khả thi đều phải từ chối ít nhất một yêu cầu từ tiền tố được chấp nhận. Thuật toán từ chối yêu cầu được chấp nhận lớn nhất. Điều này sử dụng số lượng từ chối mới tối thiểu có thể, bởi vì một từ chối là đủ bất cứ khi nào yêu cầu lớn nhất khôi phục tính khả thi. Nếu phải xóa một số yêu cầu, mỗi lần xóa sẽ chọn yêu cầu lớn nhất còn lại, tối đa hóa số tiền thu hồi được cho mỗi lần từ chối. 

Mạnh mẽ hơn, trong số tất cả các lựa chọn khả thi có cùng số lượng yêu cầu được chấp nhận trong tiền tố được xử lý, việc giữ số dư còn lại lớn nhất có thể ít nhất luôn tốt cho tương lai. Từ chối yêu cầu lớn nhất đạt được chính xác điều đó. Do đó, trạng thái tham lam không bao giờ có ít khả năng đưa ra các yêu cầu trong tương lai hơn một giải pháp khác có cùng số lần bị từ chối. Bằng cách quy nạp theo trình tự sự kiện, số lượng yêu cầu bị từ chối cuối cùng là tối thiểu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    total = n + m

    events = [int(input()) for _ in range(total)]

    answer = []
    heap = []
    balance = 0

    for i, x in enumerate(events):
        if x > 0:
            balance += x
            answer.append("resupplied")
            continue

        amount = -x
        balance -= amount
        heapq.heappush(heap, (-amount, i))
        answer.append("approved")

        while balance < 0:
            neg_amount, idx = heapq.heappop(heap)
            amount_removed = -neg_amount
            balance += amount_removed
            answer[idx] = "declined"

    sys.stdout.write("\n".join(answer))

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ trước để mọi yêu cầu đều có chỉ mục sự kiện ổn định. Chỉ mục này là cần thiết vì yêu cầu được chấp nhận trước đó có thể trở thành yêu cầu mà chúng tôi quyết định từ chối sau này. 

Để tiếp tế, mã chỉ tăng`balance`và hồ sơ`resupplied`. Không có lý do gì để sửa đổi vùng nhớ heap vì các yêu cầu đã được chấp nhận hiện tại vẫn được chấp nhận khi có nhiều tiền hơn. 

Đối với một yêu cầu, mã đầu tiên sẽ xử lý nó như được chấp nhận. Điều này tạm thời trừ đi số tiền của nó từ`balance`và chèn nó vào heap. Nếu số dư cuối cùng là âm thì tiền tố hiện tại không thể khả thi với tất cả các phê duyệt dự kiến. 

Các cửa hàng heap`(-amount, index)`. Vì vùng heap của Python trả về giá trị nhỏ nhất nên mục nhập âm nhất tương ứng với số lượng yêu cầu lớn nhất. Ví dụ: các yêu cầu có kích thước 3, 8 và 5 được lưu trữ dưới dạng`-3`,`-8`, Và`-5`, Vì thế`-8`được loại bỏ đầu tiên. 

Khi yêu cầu bị xóa, mã sẽ cộng số tiền của nó vào số dư và thay đổi`answer[idx]`ĐẾN`declined`. Đây là lý do tại sao đầu ra không thể được quyết định một cách đơn giản tại thời điểm có yêu cầu. Yêu cầu được phê duyệt ban đầu có thể trở thành yêu cầu bị từ chối sau này. 

các`while`điều kiện là cần thiết chứ không phải là một`if`. Một yêu cầu lớn có thể tạo ra mức thâm hụt lớn hơn mọi yêu cầu được chấp nhận khác, do đó một số yêu cầu được chấp nhận có thể phải bị loại bỏ. Heap luôn chứa ít nhất yêu cầu hiện tại trong khi số dư âm, do đó vòng lặp không thể hết phần tử trước khi khôi phục tính khả thi. 

Số nguyên Python xử lý số dư có thể có khoảng (2\cdot10^{14}) mà không bị tràn. Vùng heap chứa tối đa (n) yêu cầu và mỗi yêu cầu được chèn một lần và bị xóa nhiều nhất một lần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4 1
+5
-3
-2
-1
-1
```Nhà nước phát triển như sau. 

| Sự kiện | Hành động | Số dư | Số lượng đống | Thay đổi đầu ra | 
| --- | --- | --- | --- | --- | 
|`+5`| Tiếp tế | 5 |`{}`|`resupplied`| 
|`-3`| Chấp nhận | 2 |`{3}`|`approved`| 
|`-2`| Chấp nhận | 0 |`{3, 2}`|`approved`| 
|`-1`| Chấp nhận | -1 |`{3, 2, 1}`| Xóa 3, vì vậy yêu cầu 2 trở thành`declined`| 
|`-1`| Chấp nhận | 0 |`{2, 1, 1}`|`approved`| 

Yêu cầu thứ ba trong bảng là yêu cầu ban đầu`-1`tại chỉ số sự kiện 3. Ban đầu nó được phê duyệt, nhưng vùng heap xác định yêu cầu trước đó có kích thước 3 là yêu cầu tốt nhất để từ chối. Như vậy sẽ đủ tiền cho cả yêu cầu cỡ 2 và cỡ 1. 

Đầu ra cuối cùng là```
resupplied
declined
approved
approved
approved
```Dấu vết cho thấy tại sao vùng heap phải lưu trữ các chỉ mục sự kiện. Yêu cầu bị từ chối không nhất thiết là yêu cầu hiện đang được xử lý. 

### Xây dựng ví dụ 2 

Hãy xem xét```
3 2
+5
-4
-3
+2
-2
```Dấu vết là: 

| Sự kiện | Hành động | Số dư | Số lượng đống | Thay đổi đầu ra | 
| --- | --- | --- | --- | --- | 
|`+5`| Tiếp tế | 5 |`{}`|`resupplied`| 
|`-4`| Chấp nhận | 1 |`{4}`|`approved`| 
|`-3`| Chấp nhận | -2 |`{4, 3}`| Xóa 4, vì vậy yêu cầu 2 trở thành`declined`| 
|`+2`| Tiếp tế | 3 |`{3}`|`resupplied`| 
|`-2`| Chấp nhận | 1 |`{3, 2}`|`approved`| 

Thuật toán từ chối yêu cầu trước đó có kích thước 4 và giữ yêu cầu có kích thước 3. Cả hai lựa chọn sẽ liên quan đến một lần từ chối tại thời điểm thâm hụt, nhưng việc giữ yêu cầu nhỏ hơn sẽ mang lại nhiều tiền hơn. Việc tiếp tế sau đó cũng làm cho yêu cầu cuối cùng trở nên khả thi. 

Đầu ra là```
resupplied
declined
approved
resupplied
approved
```Ví dụ này thực hiện đối số trao đổi trung tâm: khi cần chính xác một lần từ chối, việc loại bỏ yêu cầu được chấp nhận lớn nhất luôn là lựa chọn mạnh mẽ nhất cho chuỗi còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+m)\log n)) | Mỗi yêu cầu vào vùng nhớ heap một lần và có thể rời khỏi vùng đó một lần, với chi phí (O(\log n)) cho mỗi thao tác vùng nhớ heap. | 
| Không gian | (O(n+m)) | Mảng sự kiện, mảng đầu ra và vùng heap cùng nhau sử dụng bộ nhớ tuyến tính. | 

Với tối đa (2\cdot10^5) sự kiện, vùng heap chỉ thực hiện một số lần chèn và xóa tuyến tính, mỗi lần logarit theo số lượng yêu cầu. Điều này nằm trong mức độ phức tạp dự định cho giới hạn một giây và mức sử dụng bộ nhớ là tuyến tính. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây sử dụng cùng một thuật toán như giải pháp đã gửi nhưng gói nó trong một hàm để có thể kiểm tra một số đầu vào hoàn chỉnh bằng các xác nhận.```python
import heapq
import io
import sys

def solve_io(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    m = next(it)
    events = [next(it) for _ in range(n + m)]

    answer = []
    heap = []
    balance = 0

    for i, x in enumerate(events):
        if x > 0:
            balance += x
            answer.append("resupplied")
        else:
            amount = -x
            balance -= amount
            heapq.heappush(heap, (-amount, i))
            answer.append("approved")

            while balance < 0:
                neg_amount, idx = heapq.heappop(heap)
                balance += -neg_amount
                answer[idx] = "declined"

    return "\n".join(answer)

# Provided sample
assert solve_io(
    """4 1
+5
-3
-2
-1
-1
"""
) == """resupplied
declined
approved
approved
approved""", "sample 1"

# Minimum-sized input, request arrives before any money.
assert solve_io(
    """1 1
-5
+5
"""
) == """declined
resupplied""", "initial empty account"

# A previous larger request must be rejected instead of the current request.
assert solve_io(
    """3 1
+5
-4
-3
-1
"""
) == """resupplied
declined
approved
approved""", "reject largest accepted request"

# All request and resupply amounts are equal, with exact balance at every request.
assert solve_io(
    """3 3
+1
-1
+1
-1
+1
-1
"""
) == """resupplied
approved
resupplied
approved
resupplied
approved""", "all equal values"

# Maximum-size structure: 100000 resupplies followed by 100000 requests.
# Every request has exactly one unit available.
max_n = 100000
max_m = 100000
max_input = (
    f"{max_n} {max_m}\n"
    + "+1\n" * max_m
    + "-1\n" * max_n
)
max_output = (
    "resupplied\n" * max_m
    + "approved\n" * max_n
).rstrip()

assert solve_io(max_input) == max_output, "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`, theo sau là`-5`,`+5`|`declined`,`resupplied`| Số dư ban đầu bằng 0 và tiền trong tương lai không thể đáp ứng yêu cầu trong quá khứ. | 
|`+5, -4, -3, -1`|`resupplied`,`declined`,`approved`,`approved`| Yêu cầu lớn hơn trước đó phải được loại bỏ thay vì từ chối một cách mù quáng yêu cầu hiện tại. | 
| luân phiên`+1, -1`sự kiện | Mỗi yêu cầu đều`approved`| Exact zero-balance boundaries and equal request sizes. | 
| 100000`+1`sự kiện theo sau bởi 100000`-1`sự kiện | Mỗi yêu cầu đều`approved`| Số lượng sự kiện tối đa, đầu ra lớn và mức sử dụng heap tuyến tính. | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là yêu cầu trước khi cung cấp lại lần đầu tiên. Vì```
1 1
-5
+5
```heap nhận được yêu cầu size-5, tạo ra số dư (-5). Heap ngay lập tức loại bỏ yêu cầu đó, khôi phục số dư về 0 và đánh dấu yêu cầu đó bị từ chối. Càng về sau`+5`chỉ làm tăng số dư sau đó. Kết quả là`declined`,`resupplied`, tôn trọng tính chất thời gian của tài khoản. 

Trường hợp cạnh thứ hai là sự thiếu hụt trong đó việc từ chối yêu cầu hiện tại là không tối ưu. Vì```
3 1
+5
-4
-3
-1
```sau đó`-4`số dư là 1. Chấp nhận`-3`cho ta số dư là (-2). Vùng heap chứa các yêu cầu có kích thước 4 và 3, vì vậy yêu cầu kích thước 4 sẽ bị loại bỏ. Số dư trở thành 2, khiến yêu cầu cỡ 3 được chấp nhận. trận chung kết`-1`sau đó thành công, để lại số dư 1. Đầu ra là`resupplied`,`declined`,`approved`,`approved`. 

Trường hợp cạnh thứ ba là một ranh giới chính xác trong đó số dư bằng 0. Với```
3 1
+5
-2
-3
-1
```yêu cầu đầu tiên để lại số dư 3, yêu cầu thứ hai để lại số dư 0 và yêu cầu thứ ba sẽ khiến số dư âm. Vùng heap chứa các kích thước 2, 3 và 1 sau khi được chấp nhận dự kiến, vì vậy yêu cầu lớn nhất, kích thước 3, sẽ bị loại bỏ. Số dư trở về 2 và yêu cầu cỡ 1 hiện tại vẫn được phê duyệt. Đầu ra là`resupplied`,`approved`,`declined`,`approved`. Điều kiện phải là`balance < 0`, không`balance <= 0`, bởi vì việc có chính xác số tiền bằng 0 vẫn khả thi. 

Trường hợp cạnh thứ tư là một yêu cầu tạo ra thâm hụt cần phải loại bỏ nhiều hơn một lần. Ví dụ,```
3 1
+3
-2
-2
-2
```sau yêu cầu đầu tiên, số dư là 1. Sau yêu cầu thứ hai, số dư là (-1), do đó vùng nhớ heap loại bỏ một yêu cầu cỡ 2 và khôi phục số dư về 1. Yêu cầu hiện tại vẫn được phê duyệt. Yêu cầu cỡ 2 tiếp theo lại tạo ra sự thiếu hụt, do đó, yêu cầu cỡ 2 khác sẽ bị xóa. Do đó, thuật toán xử lý nhiều lần xóa tại một sự kiện thay vì cho rằng một lần từ chối luôn là đủ. 

Trường hợp cạnh cuối cùng là kích thước đầu vào tối đa. Với (10^5) nguồn cung cấp của`+1`theo sau là (10^5) yêu cầu của`-1`, mọi yêu cầu đều khả thi ngay lập tức. Vùng heap tăng lên thành (10^5) phần tử, nhưng mọi thao tác vẫn giữ nguyên (O(\log n)) và chuỗi hoàn chỉnh vẫn chỉ yêu cầu (O((n+m)\log n)) thời gian và (O(n+m)) bộ nhớ.
