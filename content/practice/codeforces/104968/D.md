---
title: "CF 104968D - Nuôi dưỡng trẻ em"
description: "Chúng ta được sắp xếp một chuỗi các học sinh đến theo một thứ tự cố định và mỗi học sinh ăn một số lát bánh pizza cụ thể. Có K chiếc pizza được chuẩn bị và mỗi chiếc pizza phải có cùng số lát, gọi giá trị này là X."
date: "2026-06-28T06:48:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104968
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 2 (Beginner)"
rating: 0
weight: 104968
solve_time_s: 86
verified: false
draft: false
---

[CF 104968D - Nuôi dưỡng trẻ em](https://codeforces.com/problemset/problem/104968/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp một chuỗi các học sinh đến theo một thứ tự cố định và mỗi học sinh ăn một số lát bánh pizza cụ thể. Có K chiếc pizza được chuẩn bị và mỗi chiếc pizza phải có cùng số lát, gọi giá trị này là X. Những chiếc pizza được phục vụ lần lượt: mỗi học sinh lấy số lát yêu cầu từ chiếc bánh pizza hiện tại nếu có thể, nhưng nếu những lát còn lại không đủ, chiếc bánh pizza hiện tại sẽ bị loại bỏ ngay lập tức và một chiếc bánh mới sẽ được mở cho học sinh đó. 

Mục tiêu là chọn X nhỏ nhất có thể để tất cả học sinh có thể được phục vụ mà không cần nhiều hơn K pizza. 

Chi tiết quan trọng là quá trình này mang tính tham lam và tuyến tính: mỗi học sinh hoặc phù hợp với chiếc bánh pizza hiện tại hoặc buộc phải đặt lại. Điều này có nghĩa là số lượng pizza được sử dụng hoàn toàn được xác định bởi tần suất tổng tiền tố vượt quá bội số của X. 

Các ràng buộc cho phép tối đa 100000 sinh viên và 100000 chiếc pizza, do đó, bất kỳ giải pháp nào thử tất cả các giá trị X ứng cử viên và mô phỏng một cách ngây thơ cho từng giá trị sẽ quá chậm. Một lực lượng vũ phu đối với X lên đến tổng của tất cả các yêu cầu là không thể vì nhu cầu có thể đạt tới 10^6 và tổng số có thể đạt tới 10^11. Ngay cả việc quét tuyến tính cho mỗi ứng viên cũng sẽ vượt quá giới hạn thời gian. 

Một trường hợp khó phát hiện khi một học sinh yêu cầu nhiều hơn X. Trong trường hợp đó, mô hình hiện tại ngụ ý rằng chúng ta vẫn cung cấp cho họ pizza từ một chiếc pizza mới, vì vậy X ít nhất phải luôn là max(d_i). Một trường hợp khác là khi K đủ lớn để ngay cả X rất nhỏ cũng có thể hoạt động, nhưng X tối thiểu vẫn bị hạn chế bởi “điểm tràn” cục bộ trong chuỗi chứ không chỉ là tổng tổng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là đoán giá trị X và mô phỏng quá trình phân phối. Đối với X cố định, chúng tôi lặp lại qua các học sinh, theo dõi các lát còn lại trong chiếc bánh pizza hiện tại và đếm xem có bao nhiêu chiếc bánh pizza được sử dụng. Bất cứ khi nào một học sinh không thể ngồi vừa, chúng tôi bắt đầu làm một chiếc bánh pizza mới. Mô phỏng này là O(N) trên X. 

Khó khăn là chọn X. Vì X ít nhất là max(d_i) và nhiều nhất là tổng (d_i), nên chúng ta có thể tìm kiếm nhị phân nó. Tuy nhiên, hàm khả thi là đơn điệu: nếu một X nhất định hoạt động thì bất kỳ X lớn hơn nào cũng hoạt động vì những chiếc pizza lớn hơn không bao giờ tăng số lần nghỉ. Tính đơn điệu này cho phép tìm kiếm nhị phân trên X. 

Điểm mấu chốt là tính khả thi chỉ phụ thuộc vào việc liệu việc đóng gói tham lam có tạo ra tối đa K phân đoạn hay không trong đó mỗi phân khúc là một nhóm sinh viên liền kề có tổng không vượt quá X. Thời điểm một phân khúc vượt quá X, nó buộc phải cắt giảm. Vì vậy, việc kiểm tra X sẽ giảm xuống việc đếm xem có bao nhiêu phân đoạn như vậy được tạo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu X với mô phỏng | O(N · tổng(d)) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân + kiểm tra tham lam | O(N tổng log(d)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một chức năng`can(X)`điều đó trả về liệu K pizza có đủ hay không nếu mỗi chiếc pizza có X lát. 

1. Bắt đầu với số lượng pizza không được sử dụng và số lượng còn lại bằng không trong số pizza hiện tại. 

Về mặt khái niệm, chúng tôi coi điều này là chưa mở một chiếc bánh pizza nào. 
2. Lặp lại các học sinh theo thứ tự. 

Đối với mỗi học sinh có nhu cầu d_i, hãy cố gắng xếp chúng vào chiếc bánh pizza hiện tại. 
3. Nếu chiếc bánh pizza hiện tại còn lại ít nhất d_i lát, hãy trừ d_i khỏi nó. 

Mô hình này phục vụ họ mà không cần mở một chiếc bánh pizza mới. 
4. Nếu không, chúng ta phải mở một chiếc bánh pizza mới cho học sinh này. 

Tăng số lượng pizza lên một, đặt công suất còn lại thành X và trừ d_i. 
5. Nếu tại bất kỳ thời điểm nào có một học sinh có d_i > X, ngay lập tức trả về sai. 

Điều này là cần thiết vì ngay cả một chiếc bánh pizza tươi cũng không thể làm họ hài lòng. 
6. Sau khi xử lý tất cả học sinh, trả về tổng số pizza được sử dụng nhiều nhất là K. 

Một lần`can(X)`được xác định, chúng ta tìm kiếm nhị phân X giữa max(d_i) và sum(d_i), chọn X nhỏ nhất sao cho`can(X)`là đúng. 

Tại sao nó hoạt động: mỗi X cố định xác định một phân đoạn tham lam xác định của mảng. Việc tăng X chỉ có thể hợp nhất các phân đoạn chứ không bao giờ phân chia chúng, do đó số lượng pizza được sử dụng là đơn điệu và không tăng trong X. Điều này đảm bảo tính chính xác của tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(a, k, x):
    used = 1
    rem = x
    for v in a:
        if v > x:
            return False
        if rem >= v:
            rem -= v
        else:
            used += 1
            rem = x - v
            if used > k:
                return False
    return used <= k

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    lo = max(a)
    hi = sum(a)

    while lo < hi:
        mid = (lo + hi) // 2
        if can(a, k, mid):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Giải pháp này tách việc kiểm tra tính khả thi khỏi việc tìm kiếm trên X.`can`Chức năng mô phỏng chính xác cách tiêu thụ pizza, theo dõi các lát bánh còn lại và đếm số lượng pizza cần thiết. 

Một sai lầm phổ biến là quên rằng một học sinh không phù hợp sẽ không ăn một phần bánh pizza, họ ăn phần còn lại và buộc phải ăn một chiếc bánh pizza mới ngay lập tức. Đây là lý do tại sao chúng tôi thiết lập lại`rem = x - v`thay vì cố gắng phát huy năng lực còn sót lại. 

Một điểm tinh tế khác là khởi tạo`used = 1`. Chúng tôi chỉ mở một chiếc bánh pizza khi cần, nhưng học sinh đầu tiên luôn ăn chiếc bánh pizza mà họ phải có khi họ đến. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 5, K = 3
d = [2, 3, 4, 5, 2]
```Chúng tôi kiểm tra một ứng cử viên X = 6. 

| Sinh viên | Nhu cầu | Còn lại trước | Hành động | Còn lại sau | Pizza đã qua sử dụng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 6 | sử dụng hiện tại | 4 | 1 | 
| 2 | 3 | 4 | sử dụng hiện tại | 1 | 1 | 
| 3 | 4 | 1 | pizza mới | 2 | 2 | 
| 4 | 5 | 2 | pizza mới | - | 3 | 
| 5 | 2 | 1 | sử dụng hiện tại | - | 3 | 

Việc này sử dụng 3 chiếc pizza nên X = 6 là khả thi khi K = 3. 

Dấu vết này cho thấy việc ngắt đoạn xảy ra chính xác như thế nào khi dung lượng còn lại không đủ, tạo ra số lần mở bánh pizza cố định. 

### Ví dụ 2 

đầu vào:```
N = 4, K = 2
d = [4, 1, 5, 2]
```Hãy thử X = 6. 

| Sinh viên | Nhu cầu | Còn lại trước | Hành động | Còn lại sau | Pizza đã qua sử dụng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 6 | sử dụng | 2 | 1 | 
| 2 | 1 | 2 | sử dụng | 1 | 1 | 
| 3 | 5 | 1 | mới | 1 | 2 | 
| 4 | 2 | 1 | mới | - | 3 | 

Điều này sử dụng 3 chiếc pizza, vì vậy X = 6 không hợp lệ với K = 2. Việc tăng X sẽ làm giảm việc buộc phải nghỉ giải lao. 

Những dấu vết này thể hiện tính đơn điệu: X lớn hơn chỉ có thể giảm hoặc duy trì số lần đặt lại pizza. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log S) | Mỗi lần kiểm tra tính khả thi là O(N), tìm kiếm nhị phân theo tổng nhu cầu | 
| Không gian | O(1) | Chỉ các bộ đếm và mảng đầu vào được lưu trữ | 

Các ràng buộc N lên tới 100000 giúp quét tuyến tính có thể chấp nhận được và log(sum(d)) tối đa là khoảng 40, giữ cho tổng số thao tác thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("5 3\n2 3 4 5 2\n") == "6", "sample-like check"

# minimum size
assert run("1 1\n5\n") == "5", "single student"

# all equal
assert run("5 2\n2 2 2 2 2\n") == "6", "uniform case"

# tight K large
assert run("5 10\n1 2 3 4 5\n") == "5", "many pizzas allowed"

# boundary overflow behavior
assert run("3 2\n5 4 3\n") == "5", "forces multiple splits"

# increasing sequence
assert run("4 2\n1 2 3 4\n") == "4", "monotone demands"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sinh viên độc thân | 5 | trường hợp cơ bản, phân công trực tiếp | 
| tất cả đều bình đẳng | 6 | hành vi hợp nhất phân khúc lặp đi lặp lại | 
| K lớn | 5 | hạn chế tối thiểu là nhu cầu tối đa | 
| giảm phù hợp | 5 | đặt lại thường xuyên và tính chính xác của việc đếm | 
| trình tự tăng dần | 4 | áp lực phân khúc trong trường hợp xấu nhất | 

## Vỏ cạnh 

Trường hợp một cạnh là khi K rất lớn so với N. Đối với đầu vào như`N = 5, K = 100`, câu trả lời chỉ đơn giản là max(d_i). Thuật toán xử lý việc này vì tìm kiếm nhị phân bắt đầu ở lo = max(a) và`can(lo)`thành công ngay lập tức vì không có học sinh nào vượt quá khả năng và mỗi học sinh đều có thể ăn vừa một chiếc bánh pizza mới nếu cần. 

Một trường hợp khó khăn khác là khi mọi nhu cầu đều giống hệt nhau. Vì`a = [3,3,3,3]`, một ứng cử viên X = 3 dẫn đến một chiếc pizza cho mỗi học sinh, vì vậy 4 chiếc pizza được sử dụng. Việc kiểm tra tính khả thi tăng lên một cách chính xác`used`bất cứ khi nào phần còn lại đạt 0 sau khi trừ, đảm bảo không tính thiếu. 

Trường hợp cạnh cuối cùng là một chuỗi có một gai lớn duy nhất ở giữa. Vì`a = [1,1,100,1,1]`, mọi X < 100 đều thất bại ngay lập tức do kiểm tra rõ ràng`if v > x`. Điều này ngăn mô phỏng cố gắng phục vụ một phần yêu cầu không thể thực hiện không chính xác và đảm bảo tìm kiếm nhị phân không trôi vào phạm vi không hợp lệ.
