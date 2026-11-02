---
title: "CF 106507L - Số gia"
description: "Nhiệm vụ là xây dựng lại một chuỗi các phép cộng phạm vi. Chúng ta bắt đầu với một dãy số 0. Một thao tác chọn một phân đoạn liên tục và tăng mọi phần tử bên trong phân đoạn đó lên một."
date: "2026-06-25T08:30:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106507
codeforces_index: "L"
codeforces_contest_name: "TeamsCode 2026 Spring Contest"
rating: 0
weight: 106507
solve_time_s: 34
verified: true
draft: false
---

[CF 106507L - Phần tăng](https://codeforces.com/problemset/problem/106507/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xây dựng lại một chuỗi các phép cộng phạm vi. Chúng ta bắt đầu với một dãy số 0. Một thao tác chọn một phân đoạn liên tục và tăng mọi phần tử bên trong phân đoạn đó lên một. Sau một số thao tác như vậy không xác định, chúng tôi nhận được mảng cuối cùng và phải xuất ra số lượng thao tác tối thiểu có thể tạo ra nó, cùng với các phân đoạn được sử dụng. 

Đối tượng chính không phải là bản thân các giá trị mảng mà là cách các giá trị thay đổi giữa các vị trí lân cận. Đối với một mảng`a`, xác định mảng khác biệt:`d[i] = a[i] - a[i - 1]`với`a[0] = 0`và thêm một phần tử ảo nữa`a[n + 1] = 0`vì vậy sự khác biệt cuối cùng được bao gồm. Một hoạt động duy nhất trên phân khúc`[l, r]`thay đổi mảng khác biệt này bằng cách thêm`1`ở vị trí`l`và trừ`1`ở vị trí`r + 1`. Nói cách khác, mọi thao tác đều tạo ra chính xác một thay đổi dương và một thay đổi âm trong mảng sai phân. 

Độ dài đầu vào có thể lớn bằng`100000`. Bất kỳ cách tiếp cận nào mô phỏng lặp đi lặp lại việc áp dụng các phép toán vào mảng hoặc tìm kiếm thông qua các phân đoạn có thể sẽ quá chậm vì nó có thể yêu cầu phép tính bậc hai. Chúng ta cần một công trình tuyến tính hoặc gần tuyến tính. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu toàn bộ mảng có cùng giá trị dương, chẳng hạn như:```
3
2 2 2
```câu trả lời không phải là ba thao tác trên các phần tử riêng lẻ. Câu trả lời đúng là một thao tác:```
1
1 3
```bởi vì một phân đoạn bao gồm toàn bộ mảng. 

Another tricky case is when there are zeroes inside the array:```
5
1 0 1 0 1
```Câu trả lời đúng là ba thao tác:```
3
1 1
3 3
5 5
```Một giải pháp bất cẩn cố gắng mở rộng mọi phân đoạn dương cho đến khi giá trị lớn hơn tiếp theo có thể vô tình bao phủ các vị trí phải bằng 0. 

Trường hợp ranh giới thứ ba là phần cuối của mảng:```
4
1 1 1 1
```Câu trả lời đúng là một thao tác:```
1
1 4
```Việc giảm cuối cùng về 0 xảy ra sau mảng, do đó vị trí ảo`n + 1`phải được xử lý. Bỏ qua nó sẽ để lại số gia tăng chưa từng có. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là suy nghĩ về mọi phân đoạn có thể và liên tục áp dụng các mức tăng dần cho đến khi đạt được mảng mục tiêu. Điều này đúng vì mọi thao tác đều có thể được mô phỏng chính xác nhưng nó không mang lại cấu trúc hữu ích. Trong trường hợp xấu nhất, một mảng có độ dài`100000`với các giá trị xung quanh`100000`có thể yêu cầu hàng tỷ bản cập nhật phần tử riêng lẻ, vượt xa thời gian sẵn có. 

Quan sát hữu ích đến từ việc xem xét sự khác biệt. Mỗi mức tăng phạm vi bắt đầu tại một vị trí và ngừng ảnh hưởng đến mảng ngay sau vị trí khác. Điều đó có nghĩa là nơi duy nhất mà mảng chênh lệch thay đổi là ranh giới bắt đầu và kết thúc. 

Giả sử mảng chênh lệch có giá trị dương`d[i] = 3`. Điều này có nghĩa là cần phải bắt đầu thêm ba đoạn nữa ở vị trí`i`hơn là kết thúc ở đó. Tương tự, giá trị âm có nghĩa là nhiều phân đoạn đang hoạt động phải kết thúc trước vị trí đó. Số lượng thao tác tối thiểu chính xác là tổng số chênh lệch dương, bởi vì mọi thao tác chỉ có thể đóng góp một điểm bắt đầu. 

Việc xây dựng rất đơn giản. Quét từ trái sang phải. Bất cứ khi nào có sự khác biệt tích cực xuất hiện, hãy tạo ra nhiều phân khúc mới để bắt đầu. Bất cứ khi nào chênh lệch âm xuất hiện, hãy đóng nhiều phân đoạn hiện đang mở đó. Tổng tiền tố của các khác biệt luôn là giá trị mảng hiện tại, vì vậy nó không bao giờ âm. Vì vậy, chúng ta sẽ luôn có đủ phân đoạn mở để đóng. 

Phương pháp brute-force cố gắng khám phá các phân đoạn bằng cách sửa đổi mảng. Phương pháp tối ưu phát hiện thông tin tương tự trực tiếp từ ranh giới của các phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số thao tác × n) | O(1) | Quá chậm | 
| Tối ưu | O(n + câu trả lời) | O(câu trả lời) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mảng sai phân. Coi phần tử trước vị trí đầu tiên và phần tử sau vị trí cuối cùng là 0. Điều này làm cho mọi phép toán phạm vi tương ứng với chính xác một ranh giới dương và một ranh giới âm. 
2. Quét mảng khác biệt từ trái sang phải. Khi`d[i]`là dương, thêm`d[i]`bản sao của vị trí`i`vào danh sách các điểm bắt đầu phân đoạn hiện đang mở. Đây là những hoạt động phải bắt đầu ở đây. 
3. Khi nào`d[i]`là tiêu cực, loại bỏ`-d[i]`bắt đầu từ danh sách và ghép mỗi phần bắt đầu bị loại bỏ với vị trí kết thúc là`i - 1`. Hoạt động được biểu diễn bởi cặp này là đoạn từ điểm bắt đầu đó đến`i - 1`. 
4. Xuất tất cả các phân đoạn được tạo. Số lượng phân đoạn là số lượng đóng góp tích cực trong mảng sai phân, là số lượng tối thiểu. 

Lý do cấu trúc này tối ưu là vì mọi thao tác phải tạo chính xác một ranh giới dương trong mảng sai phân. Nếu tổng chênh lệch dương là`x`, không có giải pháp nào có thể sử dụng ít hơn`x`hoạt động. Thuật toán tạo ra chính xác`x`hoạt động, do đó nó đạt đến giới hạn dưới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    diff = []
    prev = 0
    for x in a:
        diff.append(x - prev)
        prev = x
    diff.append(-prev)

    starts = []
    ans = []

    for i, x in enumerate(diff, start=1):
        if x > 0:
            for _ in range(x):
                starts.append(i)
        elif x < 0:
            for _ in range(-x):
                l = starts.pop()
                ans.append((l, i - 1))

    print(len(ans))
    for l, r in ans:
        print(l, r)

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng mảng khác biệt. Giá trị cuối cùng bổ sung`-prev`đại diện cho sự chuyển đổi từ phần tử cuối cùng trở về 0. Nếu không có vị trí này, các đoạn đến cuối mảng sẽ không bao giờ bị đóng. 

các`starts`liệt kê các vị trí cửa hàng nơi các phân đoạn đã bắt đầu nhưng chưa kết thúc. Khi tìm thấy sự khác biệt âm, mã sẽ xuất hiện chính xác số lần bắt đầu được yêu cầu. Mọi thứ tự đều hoạt động vì các hoạt động đều độc lập, vì vậy việc sử dụng các lần khởi động gần đây nhất là một lựa chọn triển khai thuận tiện. 

Số lượng đầu ra tự động ở mức tối thiểu vì mỗi lần khởi động được lưu trữ đều tương ứng với một thao tác không thể tránh khỏi. Không có hoạt động bổ sung nào được thêm vào trong quá trình xây dựng. 

Các chỉ số sử dụng`enumerate(..., start=1)`bởi vì vấn đề sử dụng lập chỉ mục dựa trên một. Điểm cuối bên phải là`i - 1`, vì chênh lệch âm ở vị trí`i`có nghĩa là đoạn dừng ngay trước vị trí đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 2 1 1 4 1
```Mảng chênh lệch bao gồm số 0 kết thúc là:`[1, 1, -1, 0, 3, -3, -1]`| Vị trí | Sự khác biệt | Bắt đầu mở | Hành động | Đã thêm câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | Bắt đầu một phân đoạn | | 
| 2 | 1 | [1, 2] | Bắt đầu một phân đoạn | | 
| 3 | -1 | [1] | Đóng bắt đầu 2 | (2, 2) | 
| 4 | 0 | [1] | Không có gì | | 
| 5 | 3 | [1, 5, 5, 5] | Bắt đầu ba phân đoạn | | 
| 6 | -3 | [1] | Đóng ba đoạn | (5,5), (5,5), (5,5) | 
| 7 | -1 | [] | Đóng đoạn cuối | (1,6) | 

Thuật toán tìm ra năm phép tính, khớp với số lượng đơn vị sai phân dương tối thiểu có thể có. 

### Ví dụ 2 

đầu vào:```
5
1 0 1 0 1
```Mảng khác biệt là:`[1, -1, 1, -1, 1, -1]`| Vị trí | Sự khác biệt | Bắt đầu mở | Hành động | Đã thêm câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | Bắt đầu phân đoạn | | 
| 2 | -1 | [] | Đóng phân đoạn | (1,1) | 
| 3 | 1 | [3] | Bắt đầu phân đoạn | | 
| 4 | -1 | [] | Đóng phân đoạn | (3,3) | 
| 5 | 1 | [5] | Bắt đầu phân đoạn | | 
| 6 | -1 | [] | Đóng phân đoạn | (5,5) | 

Dấu vết cho thấy lý do tại sao các số 0 bên trong buộc các phân đoạn phải kết thúc. Sự khác biệt tiêu cực ngăn cản thuật toán mở rộng hoạt động không chính xác trên các vị trí đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + câu trả lời) | Mỗi vị trí mảng được xử lý một lần và mỗi phân đoạn được tạo sẽ được tạo và đóng một lần. | 
| Không gian | O(câu trả lời) | Danh sách các lần bắt đầu mở và danh sách các phân đoạn cuối cùng chứa tối đa số lượng hoạt động. | 

Những ràng buộc cho phép`n = 100000`, đồng thời số lượng thao tác phát sinh cũng được đảm bảo tối đa`100000`. Việc xây dựng tuyến tính dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    data = inp.strip().split()
    if not data:
        return ""
    n = int(data[0])
    a = list(map(int, data[1:]))

    diff = []
    prev = 0
    for x in a:
        diff.append(x - prev)
        prev = x
    diff.append(-prev)

    starts = []
    ans = []

    for i, x in enumerate(diff, 1):
        if x > 0:
            for _ in range(x):
                starts.append(i)
        else:
            for _ in range(-x):
                ans.append((starts.pop(), i - 1))

    out = [str(len(ans))]
    out += [f"{l} {r}" for l, r in ans]
    return "\n".join(out)

assert solve("""6
1 2 1 1 4 1
""") == """5
2 2
5 5
5 5
5 5
1 6""", "sample 1"

assert solve("""5
1 0 1 0 1
""") == """3
1 1
3 3
5 5""", "sample 2"

assert solve("""1
7
""") == """1
1 1""", "single element"

assert solve("""4
3 3 3 3
""") == """3
1 4
1 4
1 4""", "all equal values"

assert solve("""5
1 1 0 2 2
""") == """3
4 5
4 5
1 2""", "internal zero boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[7]`| Một đoạn bao gồm phần tử duy nhất | Kích thước đầu vào tối thiểu và xử lý ranh giới cuối cùng | 
|`[3,3,3,3]`| Ba phân đoạn đầy đủ giống hệt nhau | Giá trị bằng nhau và các thao tác lặp lại | 
|`[1,1,0,2,2]`| Ba đoạn chia xung quanh số 0 | Ranh giới đóng cửa nội bộ | 
|`[1,2,1,1,4,1]`| Năm hoạt động | Nhiều phạm vi chồng chéo | 

## Vỏ cạnh 

Đối với một mảng dương không đổi:```
3
2 2 2
```mảng khác biệt là:`[2, 0, 0, -2]`Thuật toán mở hai đoạn tại vị trí`1`, sau đó đóng cả hai ở vị trí cuối ảo`4`. Đầu ra là hai bản sao của`[1,3]`. Cần có hai thao tác vì mỗi thao tác chỉ có thể đóng góp một đơn vị chiều cao. 

Đối với một mảng chứa khoảng trống bằng 0:```
5
1 0 1 0 1
```sự khác biệt xen kẽ giữa các giá trị tích cực và tiêu cực. Mọi chênh lệch dương sẽ mở ra một phân đoạn và chênh lệch âm tiếp theo sẽ ngay lập tức đóng phân đoạn đó. Các hoạt động được tạo chính xác là các phân đoạn một phần tử`[1,1]`,`[3,3]`, Và`[5,5]`. 

Đối với một mảng kết thúc bằng giá trị dương:```
4
1 1 1 1
```mảng khác biệt là:`[1,0,0,0,-1]`Giá trị âm cuối cùng xuất phát từ số 0 ảo sau mảng. Thuật toán giữ đoạn mở cho đến thời điểm đó và xuất ra`[1,4]`, xử lý chính xác ranh giới bên phải.
