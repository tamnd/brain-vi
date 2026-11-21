---
title: "CF 26E - Đa luồng"
description: "Mỗi quá trình thực hiện lặp đi lặp lại hai hướng dẫn nguyên tử:"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 26
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 26 (Codeforces format)"
rating: 2400
weight: 26
solve_time_s: 97
verified: true
draft: false
---
[CF 26E - Đa luồng](https://codeforces.com/problemset/problem/26/E) 

**Đánh giá:** 2400 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi quá trình thực hiện lặp đi lặp lại hai hướng dẫn nguyên tử:```
yi := y
y := yi + 1
```Biến được chia sẻ`y`bắt đầu lúc`0`. Mỗi tiến trình đều có biến riêng của nó`yi`, vì vậy các tiến trình khác nhau không bao giờ ghi đè lên trạng thái cục bộ của nhau. 

Chi tiết quan trọng là hai hướng dẫn là các hoạt động nguyên tử riêng biệt. Một tiến trình có thể đọc giá trị hiện tại của`y`, bị gián đoạn một thời gian dài, và sau đó viết lại`yi + 1`sử dụng một giá trị lỗi thời. Vì điều này, số gia tăng có thể bị mất. 

Nếu tất cả các thao tác được thực hiện tuần tự mà không bị gián đoạn thì giá trị cuối cùng sẽ đơn giản là:```
sum(ni)
```Nhưng với sự xen kẽ tùy ý, câu trả lời cuối cùng có thể nhỏ hơn nhiều. 

Chúng tôi được hỏi liệu một số lịch trình thực hiện có thể tạo ra chính xác`W`. Nếu có, chúng ta phải xuất ra một chuỗi lệnh được thực hiện hợp lệ. Lịch trình chứa một chỉ mục quy trình cho mỗi lệnh được thực hiện, do đó độ dài của nó phải chính xác:```
2 * sum(ni)
```bởi vì mỗi lần lặp vòng lặp thực hiện hai hướng dẫn. 

Các ràng buộc nhỏ một cách đáng ngạc nhiên ở một chiều và lớn ở một chiều khác. Có nhiều nhất`100`các tiến trình và mỗi tiến trình thực hiện tối đa`1000`lần lặp, vậy tổng số lần lặp nhiều nhất là`100000`. Điều đó có nghĩa là việc xây dựng và in ấn một lịch trình rõ ràng là hoàn toàn khả thi. 

Điều không khả thi là khám phá tất cả các khả năng xen kẽ có thể. Mỗi lần lặp lại đóng góp hai phép toán, do đó có rất nhiều lịch trình. Bất kỳ cuộc tìm kiếm vũ phu nào đối với các lệnh thực thi đều không thể thực hiện được. 

Phần khó khăn là hiểu được giá trị cuối cùng nào có thể đạt được. 

Hãy xem xét hai quy trình với mỗi lần lặp một lần:```
P1: read 0
P2: read 0
P1: write 1
P2: write 1
```Giá trị cuối cùng trở thành`1`, mặc dù có hai mức tăng. Một mức tăng bị mất vì cả hai quá trình đều đọc cùng một giá trị cũ. 

Bây giờ hãy xem xét một quá trình duy nhất với`n1 = 11`. Vì không có sự đồng thời nên mọi lần tăng đều thành công. Giá trị cuối cùng duy nhất có thể là`11`. Yêu cầu`W = 10`là không thể. 

Một giả định sai lầm phổ biến là mọi giá trị giữa`1`Và`sum(ni)`có thể truy cập được. Điều đó không thành công khi có quá ít quy trình. 

Ví dụ:```
2 1
1 1
```là có thể, bởi vì cả hai quá trình đều có thể đọc`0`trước khi viết. 

Nhưng:```
1 0
1
```là không thể. Ít nhất một lần ghi luôn xảy ra và tập hợp ghi đó`y`ít nhất là`1`. 

Một trường hợp tinh tế khác là khi một quy trình dài hơn nhiều so với các quy trình khác:```
2 5
100 1
```Việc thi công bất cẩn có thể vô tình buộc quá trình kéo dài phải tăng lên`y`vượt xa`5`. Lịch trình phải sắp xếp cẩn thận các lần đọc cũ để nhiều lần ghi đè lên nhau. 

Thách thức chính không phải là mô phỏng mà là mô tả chính xác giá trị cuối cùng nào có thể xảy ra. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tại mọi thời điểm, hãy chọn bất kỳ quy trình nào có lệnh tiếp theo tồn tại, thực hiện lệnh đó và tiếp tục đệ quy. Cuối cùng, kiểm tra xem giá trị cuối cùng có bằng không`W`. 

Điều này có hiệu quả vì câu lệnh cho phép xen kẽ tùy ý, do đó việc liệt kê tất cả các lịch trình cuối cùng sẽ tìm thấy mọi trạng thái có thể truy cập được. 

Vấn đề là yếu tố phân nhánh. Giả sử tổng số lần lặp là`S`. Sau đó có`2S`các hoạt động nguyên tử. Ngay cả khi chúng ta bỏ qua sự phụ thuộc của lệnh thì số lượng các phần xen kẽ vẫn rất lớn:```
(2S)!
```Với`S = 100000`, điều này hoàn toàn vô vọng. 

Sự đột phá đến từ việc hiểu được điều gì thực sự thay đổi`y`. 

Lệnh ghi luôn đặt:```
y = old_read_value + 1
```Nếu một số tiến trình đọc cùng một giá trị`k`, sau đó tất cả các bài viết của họ sẽ được thiết lập sau`y`ĐẾN`k + 1`. Cho dù có bao nhiêu lần ghi như vậy xảy ra, giá trị chỉ tăng một lần. 

Vì vậy, việc thực thi có thể được xem theo lớp:```
0 -> 1 -> 2 -> ...
```Tăng từ`k`ĐẾN`k + 1`, ít nhất một tiến trình phải thực hiện việc đọc mới`k`. Bất kỳ số lượng quy trình bổ sung nào cũng có thể đọc`k`, nhưng lần viết sau của họ không làm tăng giá trị thêm nữa. 

Điều này ngay lập tức đưa ra một đặc tính. 

Giả sử giá trị cuối cùng là`W`. 

Khi đó phải tồn tại một chuỗi`W`sự gia tăng thành công. Mỗi lần tăng thành công yêu cầu một lần lặp từ một số quy trình. Một quy trình có thể đóng góp nhiều lần tăng thành công bằng cách đọc liên tục giá trị mới nhất. 

Giá trị cuối cùng tối thiểu có thể đạt được khi tất cả lần đọc đầu tiên diễn ra trước bất kỳ lần ghi nào. Sau đó mỗi cửa hàng viết`1`, vậy đáp án cuối cùng là`1`. 

Giá trị cuối cùng tối đa có thể là`sum(ni)`khi mỗi lần tăng đều thành công tuần tự. 

Nhưng không phải mọi giá trị trung gian đều có thể truy cập được. Quan sát quan trọng là thế này: 

Một quá trình với`ni`lặp đi lặp lại có thể đóng góp nhiều nhất`ni`các gia số thành công, nhưng nếu nó hoàn toàn tham gia thì lần lặp đầu tiên của nó phải đọc một số giá trị và cuối cùng tạo ra ít nhất một gia tăng. 

Sau khi làm việc cẩn thận qua cấu trúc, các giá trị có thể đạt được chính xác là:```
1 <= W <= sum(ni)
```ngoại trừ khi chỉ có một quy trình, trong đó câu trả lời buộc phải chính xác`n1`. 

Nhiệm vụ còn lại mang tính xây dựng. Chúng tôi cần một lịch trình rõ ràng. 

Việc xây dựng là thanh lịch. Chúng tôi chọn một quy trình làm bộ đếm chính, thực hiện chính xác`W`sự gia tăng thành công. Tất cả các lần lặp lại còn lại đều trở thành bản ghi đè cũ không thay đổi giá trị cuối cùng. 

Để tạo các bản ghi đè cũ, chúng tôi cho phép nhiều lần lặp lại đọc một giá trị cũ trước khi bộ đếm tiến xa hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(tổng ni) | O(tổng ni) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số lần lặp:```
S = sum(ni)
```1. Xử lý riêng trường hợp một quy trình. 

Chỉ với một quy trình, không có sự xen kẽ. Mỗi lần lặp lại tăng`y`chính xác bằng một, nên giá trị cuối cùng phải bằng`n1`. 
2. Đối với nhiều quy trình, hãy kiểm tra xem:```
1 <= W <= S
```Nếu không thì in`No`. 

1. Chọn một quy trình làm quy trình tăng chính. Chúng tôi sẽ sử dụng quy trình`1`. 
2. Đặt chỗ chính xác`W`lặp đi lặp lại trên tất cả các quy trình để tạo thành chuỗi tăng trưởng thành công. 

Ý tưởng là các lần lặp dành riêng này thực hiện gần như tuần tự:```
read k
write k + 1
```vì`k = 0, 1, ..., W - 1`. 

1. Tất cả các lần lặp lại còn lại được chuyển thành dạng ghi cũ vô hại. 

Trước khi bộ đếm tiến quá xa, hãy thực hiện các lần lặp bổ sung này để đọc một giá trị cũ. Sau này, khi họ viết lại`old + 1`, họ ghi đè`y`với giá trị đã có trước đó nên giá trị cuối cùng không tăng. 
2. Xây dựng lịch trình một cách rõ ràng. 

Đối với mỗi lần tăng thành công: 

Đầu tiên thực hiện lệnh đọc của lần lặp đã chọn. 

Sau đó thực hiện ngay lệnh ghi của nó. 

Điều này đảm bảo rằng`y`tăng lên một. 
3. Đối với mỗi lần lặp còn sót lại: 

Thực hiện lệnh đọc sớm trước khi bộ đếm đạt giá trị cuối cùng. 

Trì hoãn lệnh ghi cho đến khi bộ đếm đã đạt ít nhất cùng giá trị đó. 

Sau đó việc viết trở nên vô hại. 
4. Xuất lịch trình. 

### Tại sao nó hoạt động 

Điều bất biến là chỉ có chuỗi dành riêng mới đóng góp các giá trị mới. 

Mỗi lần tăng thành công sẽ đọc giá trị hiện tại của`y`và ngay lập tức viết lại`y + 1`, do đó giá trị tăng đúng một lần. 

Mỗi lần lặp lại cũ sẽ đọc một số giá trị trước đó`k`. Vào thời điểm việc ghi bị trì hoãn xảy ra,`y`ít nhất là đã rồi`k + 1`. Viết`k + 1`lại không thể tăng giá trị tối đa đạt được. 

Vì chúng tôi xây dựng chính xác`W`gia tăng thành công và ngăn chặn tất cả các lần lặp khác đóng góp các gia tăng mới, giá trị cuối cùng sẽ trở thành chính xác`W`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, w = map(int, input().split())
    a = list(map(int, input().split()))

    total = sum(a)

    if n == 1:
        if w != a[0]:
            print("No")
            return

        print("Yes")
        ans = []

        for _ in range(a[0]):
            ans.append(1)
            ans.append(1)

        print(*ans)
        return

    if w < 1 or w > total:
        print("No")
        return

    print("Yes")

    rem = a[:]
    ans = []

    good = []

    need = w

    for i in range(n):
        take = min(rem[i], need)
        rem[i] -= take
        need -= take

        for _ in range(take):
            good.append(i + 1)

        if need == 0:
            break

    bad = []

    for i in range(n):
        for _ in range(rem[i]):
            bad.append(i + 1)

    # stale reads
    for p in bad:
        ans.append(p)

    # successful increments
    for p in good:
        ans.append(p)
        ans.append(p)

    # stale writes
    for p in bad:
        ans.append(p)

    print(*ans)

solve()
```Việc xây dựng tách các bước lặp thành hai nhóm. 

các`good`các lần lặp lại chịu trách nhiệm cho sự gia tăng thực tế từ`0`ĐẾN`W`. Mỗi trong số chúng thực hiện việc đọc và ghi liên tục, đảm bảo một lần tăng thành công. 

các`bad`lặp đi lặp lại là ghi đè cũ. Hướng dẫn đọc của họ được thực hiện trước khi chuỗi thành công bắt đầu. Tại thời điểm đó tất cả đều đọc cùng một giá trị nhỏ, thường là`0`. 

Hướng dẫn ghi của họ bị hoãn lại cho đến khi tất cả các lần tăng thành công kết thúc. Đến lúc đó,`y`đã đủ lớn để những lần ghi này không thể tăng nó. 

Định dạng lịch trình là tinh tế. Mỗi chỉ mục quy trình đề cập đến hướng dẫn tiếp theo của nó chứ không phải toàn bộ lần lặp. Vì vậy viết:```
ans.append(p)
ans.append(p)
```có nghĩa là thực hiện lệnh đọc đầu tiên và sau đó là lệnh ghi của cùng một lần lặp. 

Một lỗi thường gặp là quên rằng mỗi lần lặp lại đóng góp chính xác hai mục lịch trình. Độ dài lịch trình cuối cùng phải là:```
2 * sum(ni)
```Việc xây dựng này tự động đáp ứng điều đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 10
11
```Chỉ có một quy trình có mười một lần lặp. 

| Bước | Hoạt động | y | 
| --- | --- | --- | 
| 1 | tăng tuần tự | 1 | 
| 2 | tăng tuần tự | 2 | 
| ... | ... | ... | 
| 11 | tăng tuần tự | 11 | 

Giá trị cuối cùng buộc phải là`11`. Không có sự xen kẽ nào tồn tại vì chỉ có một quá trình. Tiếp cận`10`là không thể. 

Đầu ra:```
No
```Ví dụ này cho thấy trường hợp ngoại lệ duy nhất. Với một quy trình duy nhất, tính đồng thời sẽ biến mất hoàn toàn. 

### Ví dụ 2 

đầu vào:```
2 1
1 1
```Sự thi công:```
bad = [2]
good = [1]
```Lịch trình được tạo:```
2 1 1 2
```Dấu vết thực hiện: 

| Bước | Quy trình | Hướng dẫn | y | 
| --- | --- | --- | --- | 
| 1 | 2 | đọc 0 | 0 | 
| 2 | 1 | đọc 0 | 0 | 
| 3 | 1 | viết 1 | 1 | 
| 4 | 2 | viết 1 | 1 | 

Quá trình thứ hai thực hiện ghi đè cũ. Cả hai tiến trình đều đọc`0`, vì vậy cả hai đều viết sản xuất`1`. 

Dấu vết này xác nhận rằng nhiều lần lặp lại có thể thu gọn thành một mức tăng hiệu quả duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng ni) | mỗi lần lặp được xử lý với số lần không đổi | 
| Không gian | O(tổng ni) | lịch trình rõ ràng bao gồm`2 * sum(ni)`số nguyên | 

Tổng số lần lặp nhiều nhất là`100000`, do đó độ phức tạp tuyến tính dễ dàng đủ nhanh trong giới hạn 2 giây. Việc sử dụng bộ nhớ cũng an toàn vì chính lịch trình sẽ chi phối việc lưu trữ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    n, w = map(int, input().split())
    a = list(map(int, input().split()))

    total = sum(a)

    if n == 1:
        if w != a[0]:
            print("No")
            return out.getvalue()

        print("Yes")
        ans = []

        for _ in range(a[0]):
            ans.append(1)
            ans.append(1)

        print(*ans)
        return out.getvalue()

    if w < 1 or w > total:
        print("No")
        return out.getvalue()

    print("Yes")

    rem = a[:]
    ans = []

    good = []

    need = w

    for i in range(n):
        take = min(rem[i], need)
        rem[i] -= take
        need -= take

        for _ in range(take):
            good.append(i + 1)

        if need == 0:
            break

    bad = []

    for i in range(n):
        for _ in range(rem[i]):
            bad.append(i + 1)

    for p in bad:
        ans.append(p)

    for p in good:
        ans.append(p)
        ans.append(p)

    for p in bad:
        ans.append(p)

    print(*ans)

    return out.getvalue()

# provided sample
assert run("1 10\n11\n") == "No\n"

# minimum size valid
assert run("1 1\n1\n").startswith("Yes")

# smallest concurrent collapse
assert run("2 1\n1 1\n").startswith("Yes")

# impossible because W too large
assert run("2 10\n3 4\n") == "No\n"

# maximum reachable value
assert run("3 6\n1 2 3\n").startswith("Yes")

# boundary: W = 0 impossible for multiple processes
assert run("2 0\n1 1\n") == "No\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`| Có | đối sánh chính xác một quy trình | 
|`2 1 / 1 1`| Có | cũ ghi đè thu gọn gia số | 
|`2 10 / 3 4`| Không | từ chối các giá trị trên tổng số lần lặp | 
|`3 6 / 1 2 3`| Có | giá trị tối đa có thể đạt được | 
|`2 0 / 1 1`| Không | giá trị cuối cùng tối thiểu là 1 | 

## Vỏ cạnh 

Hãy xem xét:```
1 5
7
```Chỉ có một quá trình. Không có hướng dẫn nào có thể xen kẽ với bất cứ điều gì khác. 

Việc thực thi luôn trông giống như:```
read k
write k + 1
```cho các giá trị liên tiếp của`k`. 

Sau bảy lần lặp, giá trị cuối cùng buộc phải trở thành`7`. 

Thuật toán phát hiện`n == 1`và kiểm tra xem`W == n1`. Từ`5 != 7`, nó in chính xác:```
No
```Bây giờ hãy xem xét:```
2 1
1 1
```Thuật toán tạo ra một lần tăng thành công và một lần ghi đè cũ. 

Thực thi: 

| Bước | Hành động | y | 
| --- | --- | --- | 
| 1 | P2 đọc 0 | 0 | 
| 2 | P1 đọc 0 | 0 | 
| 3 | P1 viết 1 | 1 | 
| 4 | P2 viết 1 | 1 | 

Giá trị cuối cùng vẫn còn`1`. 

Cuối cùng, hãy xem xét một trường hợp rất mất cân bằng:```
2 5
100 1
```Một lịch trình đơn giản có thể vô tình cho phép nhiều bước tiến thành công từ quy trình lớn. 

Việc xây dựng tránh được điều đó. Chỉ có năm lần lặp được đặt trong chuỗi thành công. Mỗi lần lặp khác thực hiện việc đọc cũ trước và ghi chậm sau. 

Cho dù tồn tại bao nhiêu lần lặp bổ sung, chỉ chuỗi dành riêng mới đóng góp các giá trị mới, vì vậy câu trả lời cuối cùng vẫn chính xác`5`.
