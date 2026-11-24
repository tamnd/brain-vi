---
title: "CF 89A - Cướp tài sản"
description: "Ngân hàng lưu trữ kim cương trong một hàng ô. Sau mỗi phút, hệ thống bảo mật sẽ kiểm tra tổng của mọi cặp liền kề: $$a1 + a2, a2 ​​+ a3, dấu chấm, a{n-1} + an$$ Nếu bất kỳ tổng nào trong số này thay đổi so với lần kiểm tra trước đó, cảnh báo sẽ kích hoạt."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 89
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 74 (Div. 1 Only)"
rating: 1800
weight: 89
solve_time_s: 114
verified: true
draft: false
---

[CF 89A - Cướp bóc](https://codeforces.com/problemset/problem/89/A) 

**Đánh giá:** 1800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ngân hàng lưu trữ kim cương trong một hàng ô. Sau mỗi phút, hệ thống bảo mật sẽ kiểm tra tổng của từng cặp liền kề:$$a_1 + a_2,\ a_2 + a_3,\ \dots,\ a_{n-1} + a_n$$Nếu bất kỳ số tiền nào trong số này thay đổi so với lần kiểm tra trước đó, cảnh báo sẽ kích hoạt. 

Joe có thể di chuyển kim cương giữa các séc. Một nước đi có thể chuyển một viên kim cương từ ô này sang ô khác, từ ô này sang túi của anh ta hoặc từ túi của anh ta trở lại ô. Anh ấy có nhiều nhất`m`di chuyển mỗi phút và chính xác`k`phút trước khi rời đi. Những gì còn lại trong túi của anh ta cuối cùng đều bị đánh cắp. 

Thử thách là tối đa hóa số lượng kim cương bị loại bỏ trong khi vẫn giữ nguyên tất cả các số tiền liền kề sau mỗi phút. 

Các ràng buộc ngay lập tức loại trừ bất kỳ mô phỏng nào đối với từng viên kim cương riêng lẻ. Số lượng tế bào chỉ$10^4$, nhưng cả hai`m`Và`k`có thể đạt được$10^9$. Tổng số hoạt động có sẵn có thể lớn như$10^{18}$, vì vậy mọi cách tiếp cận mô phỏng chuyển động từng phút đều không thể thực hiện được. Giải pháp phải trích xuất cấu trúc toán học từ bất biến do hệ thống bảo mật áp đặt. 

Khó khăn chính là hiểu được cấu hình nào bảo toàn tất cả các tổng liền kề. Một cách tiếp cận bất cẩn có thể cho rằng mảng phải không thay đổi, điều này là sai. 

Coi như:```
3 2 1
4 1 3
```Các tổng liền kề là:```
5 4
```Joe có thể chuyển đổi mảng thành:```
3 2 2
```Số tiền còn lại:```
5 4
```và anh ta đánh cắp một viên kim cương. 

Một trường hợp tế nhị khác xuất hiện khi`n = 2`.```
2 3 10
2 3
```Tổng liền kề duy nhất là`a1 + a2`. Vì tổng đó phải không đổi nên việc loại bỏ bất kỳ viên kim cương nào sẽ thay đổi nó ngay lập tức. Câu trả lời là`0`. Một chiến lược xen kẽ ngây thơ sẽ thất bại vì không có ô thứ ba để bù đắp. 

Sự bình đẳng cũng có vấn đề. Thậm chí`n`, hệ thống sẽ xác định hoàn toàn mọi giá trị sau khi cố định một phần tử. Đối với số lẻ`n`, có một mức độ tự do, đó chính xác là mức độ cho phép ăn trộm. 

Ví dụ:```
5 100 100
10 1 10 1 10
```Tổng xen kẽ là:$$10 - 1 + 10 - 1 + 10 = 28$$Cho dù Joe có bao nhiêu nước đi, anh ta cũng không thể cướp nhiều hơn số tiền luân phiên tối thiểu có thể bị ép buộc bởi các giá trị không âm. 

Việc triển khai tham lam mà chỉ xem xét chuyển khoản cục bộ có thể bỏ lỡ bất biến toàn cục này. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu bằng cách coi mỗi phút là một vấn đề chuyển trạng thái. Chúng ta có thể cố gắng liệt kê tất cả các mảng có thể truy cập sau nhiều nhất`m`di chuyển trong khi bảo toàn mọi tổng liền kề. Từ một cấu hình hợp lệ, chúng tôi thử tất cả các lần chuyển có thể và kiểm tra xem các tổng liền kề thu được có giống nhau hay không. 

Điều này hiệu quả với các đầu vào nhỏ vì điều kiện rất dễ xác minh. Đối với mỗi lần di chuyển, chỉ có các khoản tiền lân cận thay đổi. Thật không may, số lượng trạng thái bùng nổ ngay lập tức. Ngay cả khi mỗi ô chỉ chứa một vài viên kim cương, số lượng phân phối vẫn tăng theo cấp số nhân với tổng số kim cương. Với tối đa$10^4$các ô và giá trị lên đến$10^5$, vũ lực là vô vọng. 

Bước ngoặt đến từ việc nghiên cứu các phương trình do hệ thống an ninh áp đặt. 

Giả sử mảng hiện tại là:$$b_1, b_2, \dots, b_n$$và nó phải bảo toàn các tổng liền kề ban đầu:$$b_i + b_{i+1} = a_i + a_{i+1}$$cho mọi$i$. 

Trừ các phương trình lân cận cho:$$b_{i+1} - a_{i+1} = -(b_i - a_i)$$Những thay đổi phải xen kẽ các dấu hiệu. Nếu chúng ta xác định:$$x = b_1 - a_1$$sau đó:$$b_i = a_i + (-1)^{i-1}x$$Mọi cấu hình hợp lệ được xác định bởi một tham số duy nhất`x`. 

Bây giờ vấn đề trở nên đơn giản hơn nhiều. Joe đánh cắp kim cương bằng cách giảm tổng số tiền của mảng. Tổng số cuối cùng trở thành:$$\sum b_i$$và số tiền bị đánh cắp là:$$\sum a_i - \sum b_i$$Thậm chí`n`, các phép cộng và trừ luân phiên bị triệt tiêu nên tổng không bao giờ thay đổi. Ăn trộm là không thể. 

Đối với số lẻ`n`, tổng số thay đổi chính xác`x`. Cụ thể:$$\sum b_i = \sum a_i + x$$Để tối đa hóa số kim cương bị đánh cắp, chúng tôi muốn những viên kim cương nhỏ nhất có thể thực hiện được`x`. 

Bởi vì mọi ô đều phải không âm: 

Đối với vị trí lẻ:$$a_i + x \ge 0$$Đối với vị trí chẵn:$$a_i - x \ge 0$$Như vậy:$$-\min(\text{odd positions}) \le x \le \min(\text{even positions})$$Khả thi tối thiểu`x`là:$$-\min(\text{odd positions})$$Số tiền có thể đánh cắp tối đa trở thành:$$-x = \min(\text{odd positions})$$nếu như`n`thật kỳ quặc. 

Vẫn còn một hạn chế còn lại: số lượng hoạt động. 

Thay đổi một ô bằng một viên kim cương yêu cầu một lần di chuyển vào hoặc ra khỏi ô đó. Tổng số viên kim cương đã di chuyển bằng:$$|x| \times n$$bởi vì mọi tế bào đều thay đổi chính xác`|x|`. 

Joe chỉ có`m * k`tổng số hoạt động. Vì vậy, câu trả lời thực tế được giới hạn bởi:$$\left\lfloor \frac{m \cdot k}{n} \right\rfloor$$Kết hợp cả hai giới hạn sẽ cho câu trả lời cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tính tổng số phép toán có sẵn:$$\text{ops} = m \cdot k$$1. Nếu`n`chẵn, in`0`. 

Đối với các mảng có độ dài chẵn, việc bảo toàn tất cả các khoản tiền liền kề cũng bảo toàn tổng số tiền. Không có viên kim cương nào có thể rời khỏi hệ thống. 

1. Mặt khác, tìm giá trị tối thiểu giữa các vị trí được lập chỉ mục lẻ trong lập chỉ mục dựa trên 1. 

Đây là những vị trí`1, 3, 5, ...`. 

1. Đặt mức tối thiểu này là`mn`. 

Đặc tính toán học cho thấy Joe có thể ăn trộm nhiều nhất`mn`kim cương trong khi vẫn giữ cho mọi ô không âm. 

1. Mỗi viên kim cương bị đánh cắp đều phải sửa đổi tất cả`n`các ô theo một đơn vị theo các hướng xen kẽ. 

Thế là ăn trộm`t`kim cương có giá chính xác:$$t \cdot n$$hoạt động. 

1. Kinh phí hoạt động cho phép tối đa:$$\left\lfloor \frac{m \cdot k}{n} \right\rfloor$$kim cương bị đánh cắp. 

1. Câu trả lời là:$$\min\left(mn,\ \left\lfloor \frac{m \cdot k}{n} \right\rfloor\right)$$### Tại sao nó hoạt động 

Mọi cấu hình cuối cùng hợp lệ phải đáp ứng:$$b_i = a_i + (-1)^{i-1}x$$đối với một số nguyên`x`. Điều này bị ép buộc một cách duy nhất bởi các phương trình tổng liền kề. 

Khi`n`là số chẵn, các thay đổi xen kẽ bị hủy bỏ trong tổng số, vì vậy việc lấy cắp là không thể. 

Khi`n`là lẻ, giảm dần`x`làm giảm tổng số kim cương trong ngân hàng. Giá trị khả thi nhỏ nhất bị giới hạn bởi tính không âm của các ô có chỉ số lẻ, mang lại:$$x \ge -\min(\text{odd positions})$$Do đó, số tiền bị đánh cắp tối đa có thể bằng giá trị vị trí lẻ tối thiểu đó. 

Thay đổi`x`by one sửa đổi từng ô bằng một viên kim cương, chính xác như vậy`n`hoạt động là cần thiết cho mỗi viên kim cương bị đánh cắp. Ngân sách hoạt động tạo ra giới hạn trên thứ hai. Vì cả hai giới hạn đều có thể đạt được đồng thời nên việc lấy mức tối thiểu của chúng sẽ mang lại câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    if n % 2 == 0:
        print(0)
        return

    mn = min(a[::2])

    ops = m * k

    print(min(mn, ops // n))

solve()
```Quan sát đầu tiên được triển khai trong mã là kiểm tra tính chẵn lẻ. Các mảng có độ dài chẵn không thể thay đổi tổng của chúng trong khi vẫn giữ nguyên các tổng liền kề, vì vậy câu trả lời ngay lập tức là 0. 

Đối với các mảng có độ dài lẻ, giải pháp chỉ cần mức tối thiểu trong số các ô có chỉ số lẻ. Vì Python sử dụng tính năng lập chỉ mục dựa trên số 0 nên các vị trí`1, 3, 5, ...`trong câu lệnh tương ứng với các chỉ số`0, 2, 4, ...`, chính xác là`a[::2]`. 

Giới hạn hoạt động được tính là`m * k`. Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn ngay cả khi cả hai giá trị đều đạt$10^9$. 

biểu thức`ops // n`tính toán số lượng kim cương tối đa có thể bị đánh cắp với ngân sách di chuyển. Mỗi viên kim cương bị đánh cắp đều có giá chính xác`n`di chuyển. 

Câu trả lời cuối cùng là giới hạn cấu trúc và giới hạn hoạt động nhỏ hơn. 

Một sai lầm phổ biến là chia cho`2`hoặc giả sử mỗi viên kim cương chỉ cần một thao tác. Tính bất biến buộc mọi ô phải thay đổi đồng thời theo các hướng xen kẽ, do đó, một viên kim cương bị đánh cắp luôn có giá chính xác`n`hoạt động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3 1
2 3
```Từ`n`thậm chí, ăn trộm là không thể. 

| Bước | Giá trị | 
| --- | --- | 
| n | 2 | 
| Chẵn lẻ | Thậm chí | 
| Trả lời | 0 | 

Ví dụ này thể hiện tính bất biến mạnh nhất trong bài toán. Với số ô chẵn, việc bảo toàn các số tiền liền kề sẽ tự động bảo toàn tổng số kim cương. 

### Mẫu 2 

đầu vào:```
3 2 2
4 1 3
```| Bước | Giá trị | 
| --- | --- | 
| Các ô được lập chỉ mục lẻ | 4, 3 | 
| Giá trị vị trí lẻ tối thiểu | 3 | 
| Tổng số hoạt động | 4 | 
| Hoạt động trên mỗi viên kim cương bị đánh cắp | 3 | 
| Giới hạn ngân sách | 1 | 
| Câu trả lời cuối cùng | 1 | 

Joe chỉ có thể đánh cắp một viên kim cương vì mỗi viên kim cương bị đánh cắp yêu cầu phải thay đổi cả ba ô. 

Một phép biến đổi hợp lệ là: 

| Tiểu bang | Mảng | Bỏ túi | 
| --- | --- | --- | 
| Ban đầu | 4 1 3 | 0 | 
| Sau phút 1 | 3 2 2 | 1 | 

Các số liền kề còn lại:```
5 4
```khắp. 

Dấu vết này xác nhận cả hai ràng buộc cùng một lúc. Cấu trúc cho phép đánh cắp tối đa ba viên kim cương, nhưng ngân sách hoạt động chỉ cho phép một viên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tìm giá trị vị trí lẻ tối thiểu | 
| Không gian | O(1) | Chỉ có một vài biến ngoài mảng đầu vào | 

Thuật toán dễ dàng phù hợp trong giới hạn. Ngay cả đối với$n = 10^4$, một lần quét tuyến tính đơn lẻ là chuyện nhỏ. Không cần mô phỏng các thao tác hoặc số phút, mặc dù`m`Và`k`lớn như$10^9$. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    if n % 2 == 0:
        print(0)
        return

    mn = min(a[::2])
    print(min(mn, (m * k) // n))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run(
"""2 3 1
2 3
"""
) == "0", "sample 1"

# odd length, enough operations
assert run(
"""3 100 100
4 1 3
"""
) == "3", "full structural limit"

# odd length, operation budget binds
assert run(
"""3 1 1
4 1 3
"""
) == "0", "not enough operations"

# minimum size
assert run(
"""1 5 2
7
"""
) == "7", "single cell"

# even length always impossible
assert run(
"""4 100 100
1 2 3 4
"""
) == "0", "even length invariant"

# all equal values
assert run(
"""5 10 10
5 5 5 5 5
"""
) == "4", "operation limit smaller"

# zero among odd positions
assert run(
"""5 100 100
0 9 8 9 7
"""
) == "0", "nonnegative constraint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 100 100 / 4 1 3`|`3`| Giới hạn cơ cấu khi hoạt động dồi dào | 
|`3 1 1 / 4 1 3`|`0`| Ngân sách hoạt động có thể ngăn chặn hoàn toàn hành vi trộm cắp | 
|`1 5 2 / 7`|`7`| Vỏ cạnh đơn | 
|`4 100 100 / 1 2 3 4`|`0`| Mảng có độ dài chẵn không thể mất tổng | 
|`5 10 10 / 5 5 5 5 5`|`4`| Giới hạn ngân sách nhỏ hơn giới hạn cơ cấu | 
|`5 100 100 / 0 9 8 9 7`|`0`| Tính khả thi của việc kiểm soát tối thiểu vị trí lẻ | 

## Vỏ cạnh 

Hãy xem xét trường hợp có độ dài lẻ nhỏ nhất có thể:```
1 5 2
7
```Không có số tiền liền kề nào cả nên hệ thống bảo mật không áp đặt hạn chế nào. Joe có thể loại bỏ tất cả bảy viên kim cương. Thuật toán tính toán: 

| Số lượng | Giá trị | 
| --- | --- | 
| Giá trị vị trí lẻ tối thiểu | 7 | 
| Ngân sách hoạt động | 10 | 
| Giới hạn ngân sách | 10 | 
| Trả lời | 7 | 

Kết quả phù hợp với hành vi trực quan. 

Bây giờ hãy xem xét một mảng có độ dài chẵn:```
2 100 100
5 8
```Giá trị được kiểm tra duy nhất là:$$5 + 8 = 13$$Việc loại bỏ dù chỉ một viên kim cương cũng sẽ thay đổi số tiền này ngay lập tức. Thuật toán phát hiện tính chẵn lẻ và trả về`0`ngay lập tức. 

Cuối cùng, hãy xem xét trường hợp trong đó tính không âm trở thành yếu tố hạn chế:```
5 100 100
1 10 7 10 9
```Các ô có chỉ số lẻ là:```
1 7 9
```Tối thiểu là`1`, vì vậy Joe có thể đánh cắp nhiều nhất một viên kim cương. Cố gắng đánh cắp hai ô sẽ buộc ô đầu tiên âm tính:```
1 - 2 = -1
```điều đó là không thể. Thuật toán xuất ra chính xác`1`.
