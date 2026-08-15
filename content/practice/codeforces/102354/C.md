---
title: "CF 102354C - Chia sẻ tiền"
description: "Chúng tôi xử lý một chuỗi các sự kiện chia tiền theo thứ tự thời gian. Giá trị dương có nghĩa là nhiều tiền được thêm vào tài khoản công cộng. Giá trị âm thể hiện người đi vay yêu cầu số tiền dương tương ứng."
date: "2026-08-14T12:17:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "C"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 353
verified: false
draft: false
---

[CF 102354C - Chia sẻ tiền](https://codeforces.com/problemset/problem/102354/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi xử lý một chuỗi các sự kiện chia tiền theo thứ tự thời gian. Giá trị dương có nghĩa là nhiều tiền được thêm vào tài khoản công cộng. Giá trị âm thể hiện người đi vay yêu cầu số tiền dương tương ứng. Chúng ta phải quyết định phê duyệt những yêu cầu nào để số dư tài khoản không bao giờ bị âm. 

Trong số tất cả các quyết định hợp lệ, mục tiêu chính là phê duyệt càng nhiều yêu cầu càng tốt. Tương tự, chúng tôi muốn giảm thiểu số lượng yêu cầu bị từ chối. Khi có nhiều quyết định tối ưu thì quyết định nào cũng được chấp nhận. 

Đầu ra có một dòng cho mỗi sự kiện. Một sự kiện tích cực luôn tạo ra`resupplied`. Đối với một sự kiện tiêu cực, kết quả đầu ra cho biết liệu yêu cầu đó cuối cùng có được giữ nguyên như đã phê duyệt hay bị xóa khỏi nhóm yêu cầu đã chọn hay không. 

Có thể có tối đa (10^5) yêu cầu và (10^5) nguồn cung cấp lại, do đó toàn bộ chuỗi chứa tối đa (2\cdot10^5) sự kiện. Một thuật toán thử nhiều tập hợp con yêu cầu ngay lập tức là không thể, vì (2^{100000}) vượt xa mọi giới hạn thực tế. Ngay cả thuật toán (O(n^2)) cũng sẽ thực hiện khoảng (10^{10}) thao tác trong trường hợp xấu nhất, quá nhiều so với giới hạn thời gian một giây. Về cơ bản chúng ta cần xử lý tuyến tính hoặc (O(n\log n)). 

Bản thân các giá trị có thể có cường độ lên tới (10^9) và có thể có (2\cdot10^5) sự kiện, do đó tổng số tiền có thể đạt khoảng (2\cdot10^{14}). Số nguyên Python xử lý việc này một cách an toàn, trong khi ngôn ngữ có số nguyên có chiều rộng cố định nên sử dụng số học 64 bit. 

Một trường hợp tinh vi xảy ra khi việc chấp nhận yêu cầu hiện tại chỉ có thể thực hiện được nếu sau đó chúng tôi xóa một yêu cầu lớn hơn đã được chấp nhận. Ví dụ:```
3 1
+5
-3
-4
-2
```Sản lượng tối ưu là:```
resupplied
declined
approved
approved
```Sau khi chấp nhận`-3`, vậy chỉ còn lại 2 chiếc`-4`không thể được chấp nhận cùng với nó. Một thuật toán tham lam bất cẩn có thể đơn giản từ chối`-4`, sau đó cũng giảm`-2`bởi vì nó chỉ nhìn vào số dư hiện tại. Quyết định tốt hơn là thay thế yêu cầu kích thước 3 trước đó bằng yêu cầu kích thước 4. Tổng số yêu cầu được phê duyệt tại thời điểm đó vẫn là một và 2 đơn vị còn lại sau đó có thể phê duyệt yêu cầu cuối cùng. 

Một trường hợp đặc biệt khác là khi yêu cầu khiến số dư trở nên âm lại chính là yêu cầu được chấp nhận lớn nhất. Ví dụ:```
1 1
+3
-5
```Đầu ra đúng duy nhất là:```
resupplied
declined
```Thuật toán không được để số dư âm sau khi tạm thời chấp nhận yêu cầu. Nó sẽ ngay lập tức loại bỏ yêu cầu lớn nhất được chấp nhận, đây là yêu cầu hiện tại. 

Trường hợp thứ ba là một yêu cầu có số tiền chính xác bằng số dư khả dụng:```
2 1
+5
-5
-1
```Đầu ra đúng là:```
resupplied
approved
declined
```Một điều kiện sử dụng`balance <= 0`thay vì`balance < 0`sẽ từ chối không chính xác yêu cầu sử dụng toàn bộ số dư. Số tiền còn lại bằng 0 là hoàn toàn hợp lệ. 

Cuối cùng, nhiều nguồn cung cấp có thể xuất hiện giữa các yêu cầu:```
3 2
+2
+3
-4
-1
-1
```Hai yêu cầu đầu tiên có thể được chấp thuận, không để lại số tiền nào, trong khi yêu cầu cuối cùng phải bị từ chối. Một giải pháp xử lý từng nguồn cung cấp một cách độc lập thay vì duy trì số dư tích lũy sẽ đưa ra những quyết định sai lầm. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xem xét từng tập hợp con các yêu cầu. Đối với mỗi tập hợp con, chúng tôi mô phỏng các sự kiện theo trình tự thời gian và kiểm tra xem mọi yêu cầu đã chọn có thể được thanh toán tại vị trí của nó hay không. Trong số tất cả các tập hợp con khả thi, chúng tôi giữ một tập hợp con chứa số lượng yêu cầu lớn nhất. Điều này đúng vì mọi tập hợp yêu cầu được phê duyệt có thể đều được xem xét rõ ràng. 

Vấn đề là số lượng tập hợp con. Với (n) yêu cầu, có (2^n) lựa chọn khả thi và việc kiểm tra một lựa chọn sẽ mất (O(n+m)) thời gian. Do đó, số lượng hoạt động trong trường hợp xấu nhất là (O((n+m)2^n)), điều này đã vô vọng đối với ngay cả vài chục yêu cầu, chứ đừng nói đến (10^5). 

Quan sát quan trọng là mọi yêu cầu đều có cùng một giá trị cho mục tiêu của chúng tôi: việc phê duyệt bất kỳ yêu cầu nào sẽ đóng góp một giá trị vào câu trả lời. Điều duy nhất khác nhau giữa các yêu cầu là số tiền họ tiêu thụ. 

Giả sử tại một thời điểm nào đó, các yêu cầu đã chọn yêu cầu nhiều tiền hơn số tiền đã được cung cấp cho đến nay. Chúng tôi phải xóa ít nhất một yêu cầu đã được chấp nhận. Vì việc xóa bất kỳ một yêu cầu nào sẽ làm giảm đúng một yêu cầu được phê duyệt, nên cách khắc phục tốt nhất có thể là xóa yêu cầu giải phóng số tiền lớn nhất. Việc xóa một yêu cầu lớn hơn sẽ mang lại cho chúng tôi ít nhất số dư còn lại bằng với việc xóa bất kỳ yêu cầu nhỏ hơn nào, trong khi chi phí vẫn bằng cho một lần phê duyệt. 

Điều này tự nhiên dẫn đến một chiến lược tham lam. Tạm thời chấp nhận mọi yêu cầu. Nếu làm như vậy khiến số dư bị âm, hãy xóa yêu cầu lớn nhất trong số tất cả các yêu cầu hiện được chấp nhận. Vùng heap tối đa cho phép chúng tôi tìm thấy yêu cầu đó một cách hiệu quả. 

Phần quan trọng là yêu cầu bị xóa trước đó có thể được thay thế bằng yêu cầu sau. Đây là lý do tại sao chỉ từ chối yêu cầu đầu tiên không phù hợp là không đủ. Heap cho chúng ta khả năng sửa lại quyết định trước đó trong khi vẫn giữ được số lượng yêu cầu được chấp nhận tối đa có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((n+m)2^n)) | (O(n+m)) | Quá chậm | 
| Tham lam với max-heap | (O((n+m)\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi sự kiện hoàn chỉnh và tạo một mảng đầu ra ban đầu không chứa quyết định nào cho mỗi sự kiện. Giữ một biến`balance`, ban đầu bằng 0, thể hiện số tiền hiện có sau khi tất cả các yêu cầu được phê duyệt được xử lý cho đến nay. 
2. Với mỗi sự kiện tích cực, hãy cộng giá trị của nó vào`balance`và viết`resupplied`cho sự kiện đó. Việc tiếp tế không bao giờ cạnh tranh với một quyết định khác, vì vậy nó luôn có thể được kết hợp ngay lập tức. 
3. Với mỗi sự kiện tiêu cực, hãy để`cost`là giá trị tuyệt đối của nó. Tạm thời phê duyệt yêu cầu bằng cách trừ`cost`từ`balance`. Lưu trữ yêu cầu được chấp nhận này trong vùng heap tối đa cùng với vị trí của nó. 
4. Nếu số dư cuối cùng không âm, hãy tiếp tục chấp thuận yêu cầu. Tập hợp các yêu cầu được chấp nhận hiện tại là khả thi tại thời điểm này, vì vậy không có lý do gì để xóa bất kỳ yêu cầu nào. 
5. Nếu số dư âm, hãy xóa yêu cầu được chấp nhận có chi phí lớn nhất khỏi heap. Đánh dấu yêu cầu đó là`declined`và thêm chi phí của nó trở lại`balance`. Bởi vì tất cả các yêu cầu đều đóng góp chính xác một đơn vị cho mục tiêu nên việc xóa yêu cầu lớn nhất là cách sửa chữa tốt nhất có thể: mỗi lần xóa có thể sẽ mất một phê duyệt và yêu cầu lớn nhất sẽ trả lại nhiều tiền nhất. 
6. Tiếp tục toàn bộ chuỗi sự kiện. Cuối cùng, mọi yêu cầu còn lại trong heap đều được chấp thuận, trong khi mọi yêu cầu bị xóa khỏi heap đều bị từ chối. Mảng đầu ra đã ghi lại trạng thái cuối cùng của mọi yêu cầu, bao gồm cả các yêu cầu được chấp nhận tạm thời và sau đó bị xóa. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của chuỗi sự kiện, vùng heap chứa một tập hợp các yêu cầu được phê duyệt khả thi với số lượng yêu cầu tối đa có thể có trong số tất cả các lựa chọn khả thi cho tiền tố đó. 

Khi một yêu cầu mới phù hợp, việc thêm yêu cầu đó sẽ tăng số lượng yêu cầu được phê duyệt lên một, do đó, tập hợp mới là tối ưu cho tiền tố đó. Khi nó không phù hợp, một số yêu cầu được chấp nhận phải được loại bỏ. Mỗi lần xóa có thể sẽ làm giảm số lượng phê duyệt đi một, nhưng việc xóa yêu cầu lớn nhất sẽ mang lại số dư lớn nhất có thể. Do đó, heap chứa tập khả thi tốt nhất có thể có trong số tất cả các lựa chọn với số lượng phê duyệt tối đa. Vì các sự kiện trong tương lai chỉ phụ thuộc vào số tiền còn lại và các yêu cầu đã được chọn, việc duy trì số dư còn lại lớn nhất có thể giữa các giải pháp lớn như nhau không bao giờ có thể ảnh hưởng đến tính khả thi trong tương lai. Điều này duy trì tính bất biến trong suốt chuỗi và đưa ra số lượng yêu cầu được phê duyệt tối ưu trên toàn cầu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    total = n + m

    events = [int(input()) for _ in range(total)]

    balance = 0
    heap = []
    answer = [""] * total

    for i, x in enumerate(events):
        if x > 0:
            balance += x
            answer[i] = "resupplied"
        else:
            cost = -x

            balance -= cost
            heapq.heappush(heap, (-cost, i))
            answer[i] = "approved"

            if balance < 0:
                neg_cost, idx = heapq.heappop(heap)
                removed_cost = -neg_cost

                balance += removed_cost
                answer[idx] = "declined"

    sys.stdout.write("\n".join(answer))

if __name__ == "__main__":
    solve()
```các`balance`biến đại diện cho số tiền thực tế còn lại sau tất cả các yêu cầu hiện được phê duyệt. Các sự kiện tích cực làm tăng nó trước khi có thể đưa ra bất kỳ quyết định yêu cầu nào. 

Đối với một yêu cầu, mã đầu tiên sẽ chèn`(-cost, index)`vào vùng heap tối thiểu của Python. Việc phủ định chi phí sẽ biến khóa heap nhỏ nhất thành chi phí ban đầu lớn nhất, mang lại cho chúng ta một heap tối đa một cách hiệu quả. 

Yêu cầu ban đầu được đánh dấu`approved`bởi vì thuật toán tham lam cần coi nó là một ứng cử viên. Nếu điều này tạo ra số dư âm thì chính xác một yêu cầu sẽ bị xóa. Mục nhập heap xuất hiện cho chúng ta biết yêu cầu nào sẽ từ chối và quyết định đó sẽ khôi phục được bao nhiêu tiền. 

Chỉ mục được lưu trữ trong mỗi mục nhập heap là cần thiết vì yêu cầu bị xóa khỏi heap có thể không phải là yêu cầu hiện tại. Nếu không có chỉ mục, chúng ta có thể biết số tiền cần xóa nhưng không thể cập nhật dòng đầu ra tương ứng. 

Bài kiểm tra là`balance < 0`, không`balance <= 0`. Số dư bằng 0 là hợp lệ vì yêu cầu được phê duyệt gần đây nhất có thể tiêu tốn toàn bộ số dư còn lại của tài khoản. 

Không có vấn đề tràn số nguyên trong Python. Giá trị tích lũy lớn nhất có thể có là vào khoảng (10^{14}), được xử lý thoải mái bằng các số nguyên có độ chính xác tùy ý của Python. 

Đầu vào được đọc với`sys.stdin.readline`và tất cả đầu ra được tập hợp thành một chuỗi. Điều này tránh được chi phí thực hiện nhiều thao tác đầu ra riêng biệt cho tối đa (2\cdot10^5) dòng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
4 1
+5
-3
-2
-1
-1
```Dấu vết là: 

| Sự kiện | Số dư trước | Hành động | Đống sau hành động | Cân bằng sau | 
| --- | --- | --- | --- | --- | 
|`+5`| 0 | tiếp tế | trống | 5 | 
|`-3`| 5 | phê duyệt | {3} | 2 | 
|`-2`| 2 | phê duyệt | {3, 2} | 0 | 
|`-1`| 0 | tạm thời phê duyệt, xóa 3 | {2, 1} | 2 | 
|`-1`| 2 | phê duyệt | {2, 1, 1} | 1 | 

Ở sự kiện thứ tư, việc chấp thuận yêu cầu mới sẽ khiến số dư bị âm. Yêu cầu được chấp nhận lớn nhất là yêu cầu trước đó có kích thước 3, do đó yêu cầu đó sẽ bị xóa. Yêu cầu mới có kích thước 1 vẫn tồn tại và tài khoản còn lại 2 đơn vị. Điều này tốt hơn việc từ chối yêu cầu hiện tại vì cả hai lựa chọn sẽ loại bỏ một phê duyệt, nhưng việc thay thế yêu cầu cỡ 3 bằng yêu cầu cỡ 1 sẽ mang lại nhiều tiền hơn cho các yêu cầu trong tương lai. 

Đầu ra cuối cùng là:```
resupplied
declined
approved
approved
approved
```### Xây dựng ví dụ 2 

Hãy xem xét:```
4 2
+5
-4
-3
+2
-2
-2
```Dấu vết là: 

| Sự kiện | Số dư trước | Hành động | Đống sau hành động | Cân bằng sau | 
| --- | --- | --- | --- | --- | 
|`+5`| 0 | tiếp tế | trống | 5 | 
|`-4`| 5 | phê duyệt | {4} | 1 | 
|`-3`| 1 | tạm thời phê duyệt, xóa 4 | {3} | 2 | 
|`+2`| 2 | tiếp tế | {3} | 4 | 
|`-2`| 4 | phê duyệt | {3, 2} | 2 | 
|`-2`| 2 | phê duyệt | {3, 2, 2} | 0 | 

Yêu cầu thứ hai không thể cùng tồn tại với yêu cầu đầu tiên, vì chi phí tổng hợp của chúng là 7 trong khi chỉ có 5 được cung cấp cho đến nay. Thuật toán tham lam thay thế yêu cầu có giá 4 bằng yêu cầu có giá 3. Sau đó, nguồn cung cấp bổ sung giúp có thể chấp nhận cả hai yêu cầu còn lại. 

Đầu ra cuối cùng là:```
resupplied
declined
approved
resupplied
approved
approved
```Ví dụ này giải thích tại sao thuật toán phải được phép hoàn tác phê duyệt trước đó. Một chiến lược cam kết vĩnh viễn với mọi yêu cầu phù hợp vào thời điểm riêng của nó có thể mất đi sự chấp thuận sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+m)\log n)) | Mỗi yêu cầu được chèn vào và có thể bị xóa khỏi heap một lần. | 
| Không gian | (O(n+m)) | Mảng sự kiện và đầu ra chứa toàn bộ chuỗi, trong khi vùng heap chứa tối đa (n) yêu cầu. | 

Có nhiều nhất (2\cdot10^5) sự kiện và nhiều nhất (10^5) mục nhập heap. Mỗi thao tác heap có chi phí (O(\log n)), mang lại khoảng (2\cdot10^5\log(10^5)) hoạt động ở cấp độ heap trong trường hợp xấu nhất. Điều này dễ dàng nằm trong độ phức tạp dự định đối với các ràng buộc nhất định, trong khi cách tiếp cận theo cấp số nhân vũ phu là hoàn toàn không khả thi. 

## Trường hợp thử nghiệm 

Khai thác kiểm tra sau đây hiển thị bộ giải dưới dạng một hàm để có thể kiểm tra các trường hợp bằng các xác nhận Python thông thường.```python
import sys
import io
import heapq

def solve_data(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    m = int(next(it))
    total = n + m

    events = [int(next(it)) for _ in range(total)]

    balance = 0
    heap = []
    answer = [""] * total

    for i, x in enumerate(events):
        if x > 0:
            balance += x
            answer[i] = "resupplied"
        else:
            cost = -x

            balance -= cost
            heapq.heappush(heap, (-cost, i))
            answer[i] = "approved"

            if balance < 0:
                neg_cost, idx = heapq.heappop(heap)
                balance += -neg_cost
                answer[idx] = "declined"

    return "\n".join(answer) + "\n"

def run(inp: str) -> str:
    return solve_data(inp)

assert run("""\
4 1
+5
-3
-2
-1
-1
""") == """\
resupplied
declined
approved
approved
approved
""", "sample 1"

assert run("""\
2 1
+5
-5
-1
""") == """\
resupplied
approved
declined
""", "exact balance must be accepted"

assert run("""\
3 1
+5
-3
-4
-2
""") == """\
resupplied
declined
approved
approved
""", "replace a larger earlier request"

assert run("""\
1 1
+1
-1
""") == """\
resupplied
approved
""", "minimum-size input"

assert run("""\
4 2
+5
-4
-3
+2
-2
-2
""") == """\
resupplied
declined
approved
resupplied
approved
approved
""", "replacement followed by a later resupply"

events = [1] * 100000 + [-1] * 100000
large_input = "100000 100000\n" + "\n".join(map(str, events)) + "\n"
large_output = run(large_input)
large_lines = large_output.splitlines()

assert len(large_lines) == 200000, "maximum-size case has wrong output length"
assert all(x == "resupplied" for x in large_lines[:100000]), "all supplies must be processed"
assert all(x == "approved" for x in large_lines[100000:]), "all unit requests fit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`+1, -1`| Cả hai sự kiện được chấp nhận | Hộp có kích thước tối thiểu và số dư chính xác | 
|`+5, -5, -1`| Yêu cầu đầu tiên được chấp nhận, lần thứ hai bị từ chối | Phân biệt`balance == 0`từ số dư âm không hợp lệ | 
|`+5, -3, -4, -2`|`-3`từ chối,`-4`Và`-2`được chấp nhận | Xác minh việc thay thế yêu cầu đã được chấp nhận trước đó | 
|`+5, -4, -3, +2, -2, -2`|`-4`bị từ chối, tất cả các yêu cầu sau đó được chấp nhận | Xác minh rằng các quyết định thay thế tương tác chính xác với các nguồn cung cấp trong tương lai | 
| 100000 nguồn cung cấp theo sau là 100000 yêu cầu đơn vị | Mọi sự kiện được chấp nhận | Đầu vào và hiệu suất kích thước tối đa | 

## Vỏ cạnh 

Trường hợp không rõ ràng đầu tiên là thay thế một yêu cầu trước đó. Vì```
3 1
+5
-3
-4
-2
```số dư trở thành 5 sau khi cung cấp lại. Yêu cầu của 3 được chấp nhận, còn lại 2. Yêu cầu của 4 tạm thời được chấp nhận, nâng số dư lên -2. Vùng heap chứa các yêu cầu có kích thước 3 và 4, do đó yêu cầu kích thước 4 thực sự bị loại bỏ, để lại yêu cầu kích thước 3 được phê duyệt và số dư là 2. Sau đó, yêu cầu cuối cùng có kích thước 2 được chấp nhận. Đầu ra là:```
resupplied
approved
declined
approved
```Đây chính xác là tình huống trong đó quy tắc "từ chối yêu cầu hiện tại" đơn giản có hiệu quả, nhưng công thức heap cũng xử lý tình huống ngược lại khó khăn hơn khi yêu cầu hiện tại nhỏ hơn yêu cầu trước đó. 

Thay vào đó hãy xem xét:```
3 1
+5
-3
-2
-4
```Sau hai yêu cầu đầu tiên, số dư bằng 0 và heap chứa 3 và 2. Yêu cầu 4 tạm thời được chấp nhận, tạo ra số dư là -4. Yêu cầu được chấp nhận lớn nhất chính là 4 nên nó sẽ bị xóa. Hai yêu cầu đầu tiên vẫn được chấp thuận. Đầu ra là:```
resupplied
approved
approved
declined
```Điều này xác nhận rằng yêu cầu hiện tại có thể là yêu cầu bị xóa khỏi vùng nhớ heap. 

Ranh giới chính xác bằng 0 được xử lý bởi```
2 1
+5
-5
-1
```Số dư trở thành số 0 sau khi được phê duyệt`-5`, do đó không có sự loại bỏ nào xảy ra. Chỉ có yêu cầu cuối cùng bị từ chối. Đầu ra là:```
resupplied
approved
declined
```sử dụng`<= 0`trong điều kiện sửa chữa đống sẽ loại bỏ yêu cầu cỡ 5 một cách không chính xác. 

Một trình tự cũng có thể chứa một số nguồn cung cấp trước khi mượn:```
3 2
+2
+3
-4
-1
-1
```Số dư đạt 5 trước yêu cầu đầu tiên. Sau khi phê duyệt yêu cầu của 4 thì là 1 nên yêu cầu của 1 cũng được chấp thuận và số dư trở thành 0. Yêu cầu cuối cùng không thể được chấp thuận. Đầu ra là:```
resupplied
resupplied
approved
approved
declined
```Thuật toán chỉ đơn giản là tích lũy mọi sự kiện tích cực trong`balance`, do đó không cần xử lý đặc biệt đối với những lần tiếp tế liên tiếp. 

Số lượng lớn là một trường hợp thực tế khác. Ví dụ:```
2 2
+1000000000
+1000000000
-1000000000
-1000000000
```Số dư đạt đến (2\cdot10^9) và cả hai yêu cầu đều được chấp thuận, để lại số 0. Số học số nguyên của Python xử lý trực tiếp các giá trị này và heap lưu trữ chi phí yêu cầu mà không có bất kỳ chuyển đổi hoặc cắt bớt nào. 

Điều tinh tế cuối cùng là một yêu cầu bị từ chối có thể đã xảy ra trước đó nhiều sự kiện. Dòng đầu ra của nó phải thay đổi khi vùng heap loại bỏ nó sau đó. Đó là lý do tại sao mọi mục nhập heap đều lưu trữ chỉ mục sự kiện ban đầu. Nếu không có chỉ mục đó, thuật toán có thể tìm thấy kích thước yêu cầu chính xác nhưng sẽ không thể tạo ra kết quả đầu ra cần thiết cho mỗi sự kiện.
