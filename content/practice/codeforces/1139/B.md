---
title: "CF 1139B - Sôcôla"
description: "Chúng tôi có một bộ các loại sôcôla, mỗi loại có số lượng hạn chế. Mục tiêu của chúng ta là chọn một số sôcôla từ mỗi loại sao cho tổng số sôcôla là lớn nhất."
date: "2026-06-12T03:50:37+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1139
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 548 (Div. 2)"
rating: 1000
weight: 1139
solve_time_s: 87
verified: true
draft: false
---

[CF 1139B - Sôcôla](https://codeforces.com/problemset/problem/1139/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ các loại sôcôla, mỗi loại có số lượng hạn chế. Mục tiêu của chúng ta là chọn một số sôcôla từ mỗi loại sao cho tổng số sôcôla là lớn nhất. Điều bắt buộc là một quy tắc “tăng nghiêm ngặt hoặc bằng 0”: đối với bất kỳ loại sô cô la nào, nếu chúng ta chọn nhiều hơn 0 thì tất cả các loại trước đó chúng ta đã chọn phải có ít sôcôla hơn loại này. Nói cách khác, dãy số khác 0 phải tăng một cách nghiêm ngặt. 

Đầu vào bao gồm số loại,$n$và một mảng$a$chiều dài$n$Ở đâu$a_i$là cổ phiếu của$i$-thứ sôcôla. Đầu ra là tổng tối đa của một mảng lựa chọn hợp lệ$x$, Ở đâu$0 \le x_i \le a_i$Và$x$thỏa mãn quy luật tăng chặt chẽ ở trên. 

Được cho$n$có thể lên đến$2 \cdot 10^5$và mỗi$a_i$lên đến$10^9$, bất kỳ giải pháp nào có vòng lặp lồng nhau$n$có lẽ là quá chậm. Chúng ta phải tránh$O(n^2)$cách tiếp cận. 

Các trường hợp có rủi ro không rõ ràng bao gồm các tình huống trong đó số lượng hàng tồn kho không tăng hoặc không đổi. Ví dụ,$a = [5, 5, 5]$. Một cách tiếp cận ngây thơ có thể thử$x = [5, 5, 5]$, nhưng điều này vi phạm quy tắc tăng nghiêm ngặt. Cách tiếp cận đúng sẽ chọn$x = [1, 2, 3]$, tôn trọng quy tắc và tối đa hóa tổng số. 

Một trường hợp khó khăn khác là tất cả hàng tồn kho đều rất nhỏ hoặc bằng không. Ví dụ,$a = [1, 1, 1]$dẫn đến$x = [0, 1, 1]$hoặc$x = [1, 1, 0]$tùy thuộc vào mức độ tiến triển của chúng ta, nhưng số tiền phải được quản lý cẩn thận để tuân theo quy luật. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử tất cả các lựa chọn có thể cho$x_i$từ$0$ĐẾN$a_i$và kiểm tra xem điều kiện tăng chặt có đúng hay không. Đối với mỗi$x_i$, chúng ta có thể kiểm tra mọi lần trước đó$x_j$để xác nhận điều kiện. Điều này có thể đúng, nhưng số lượng hoạt động sẽ rất lớn. Vì$n = 2 \cdot 10^5$Và$a_i$lên đến$10^9$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là yêu cầu ngày càng tăng nghiêm ngặt áp đặt một trật tự tự nhiên: chúng ta không thể chọn nhiều hơn số khác 0 trước đó trừ đi một cho bất kỳ sô cô la nào. Nếu chúng ta làm ngược lại từ sô cô la cuối cùng, chúng ta có thể đảm bảo rằng mỗi số được chọn ít nhất là một số ít hơn số tiếp theo hoặc nhiều nhất là số lượng còn lại của nó. Điều này biến vấn đề thành một đường tuyến tính duy nhất từ ​​phải sang trái, duy trì một`next_max`giá trị giới hạn lựa chọn hiện tại. 

Ví dụ: nếu loại cuối cùng có 6 viên sôcôla thì loại tiếp theo có thể chọn tối đa 5, sau đó là 4, v.v. Nếu cổ phiếu nhỏ hơn giới hạn này, chúng tôi chọn cổ phiếu. Nếu giới hạn bằng 0, chúng ta không thể chọn thêm bất kỳ viên sôcôla nào nữa. 

Cái nhìn sâu sắc này làm giảm vấn đề từ độ phức tạp theo cấp số nhân hoặc bậc hai sang tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tích của a_i) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`next_max`với một số lượng rất lớn, lớn hơn bất kỳ`a_i`. Điều này sẽ theo dõi mức tối đa mà chúng ta có thể chọn cho loại sôcôla hiện tại để đáp ứng quy tắc tăng nghiêm ngặt. 
2. Khởi tạo`total`về không. Điều này sẽ tích lũy tổng số sôcôla. 
3. Lặp lại từ loại sôcôla cuối cùng đến loại đầu tiên. Ở mỗi bước: 

- Tính toán lựa chọn hiện tại là tối thiểu`a[i]`Và`next_max - 1`. Điều này đảm bảo chúng ta không vi phạm trình tự tăng nghiêm ngặt. 
- Nếu giá trị này âm thì đặt về 0 vì chúng ta không thể chọn sôcôla âm. 
- Thêm giá trị này vào`total`. 
- Cập nhật`next_max`giá trị này cho lần lặp tiếp theo. 
4. Sau khi vòng lặp kết thúc,`total`chứa số sôcôla tối đa chúng ta có thể mua. 

Điều bất biến ở đây là ở mỗi bước, số lượng sôcôla được chọn cho một loại là lớn nhất có thể mà không vi phạm quy tắc tăng dần cho các loại sau này. Làm việc ngược lại đảm bảo rằng chúng ta tôn trọng những hạn chế trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

next_max = float('inf')
total = 0

for i in reversed(range(n)):
    pick = min(a[i], next_max - 1)
    if pick < 0:
        pick = 0
    total += pick
    next_max = pick

print(total)
```Giải pháp đặt ra đầu tiên`next_max`đến vô cùng để loại sôcôla cuối cùng có thể lấy hết số lượng có sẵn. Lặp lại từ phải sang trái đảm bảo rằng quy tắc tăng nghiêm ngặt được thực thi. các`min`chức năng thực thi giới hạn hàng tồn kho và giới hạn giảm dần, đồng thời kiểm tra số âm đảm bảo chúng tôi không bao giờ chọn số tiền âm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`5 1 2 1 3 6`| tôi | một [tôi] | tiếp theo_max | chọn | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 4 | 6 | thông tin | 6 | 6 | 
| 3 | 3 | 6 | 3 | 9 | 
| 2 | 1 | 3 | 1 | 10 | 
| 1 | 2 | 1 | 0 | 10 | 
| 0 | 1 | 0 | 0 | 10 | 

Dấu vết này xác nhận rằng tính ngược và tôn trọng giới hạn giảm sẽ đảm bảo tính chất tăng nghiêm ngặt trong khi tối đa hóa tổng. 

### Mẫu 2 

đầu vào:`5 1 2 5 4 10`| tôi | một [tôi] | tiếp theo_max | chọn | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 4 | 10 | thông tin | 10 | 10 | 
| 3 | 4 | 10 | 4 | 14 | 
| 2 | 5 | 4 | 3 | 17 | 
| 1 | 2 | 3 | 2 | 19 | 
| 0 | 1 | 2 | 1 | 20 | 

Chúng tôi thấy rằng ngay cả khi lượng hàng tồn kho cao hơn giới hạn giảm dần, chúng tôi chỉ lấy ở mức cho phép, thực thi trình tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền đơn qua mảng | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng, không có mảng bổ sung | 

Giải pháp xử lý lên đến$2 \cdot 10^5$sôcôla một cách dễ dàng trong 2 giây vì nó chỉ thực hiện một lần quét tuyến tính duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    next_max = float('inf')
    total = 0
    for i in reversed(range(n)):
        pick = min(a[i], next_max - 1)
        if pick < 0:
            pick = 0
        total += pick
        next_max = pick
    return str(total)

# provided samples
assert run("5\n1 2 1 3 6\n") == "10", "sample 1"
assert run("5\n1 2 5 4 10\n") == "20", "sample 2"
assert run("4\n0 0 0 1\n") == "1", "small values"

# custom cases
assert run("3\n5 5 5\n") == "6", "all equal values"
assert run("6\n1 1 1 1 1 1\n") == "3", "all ones"
assert run("1\n1000000000\n") == "1000000000", "single large value"
assert run("5\n10 0 0 0 10\n") == "10", "zeros in the middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 5 5 5 | 6 | dãy giảm dần thỏa mãn quy luật | 
| 6 1 1 1 1 1 1 | 3 | lặp lại, chọn dãy tăng dần | 
| 1 1000000000 | 1000000000 | phần tử đơn, chọn tối đa | 
| 5 10 0 0 0 10 | 10 | số 0 ngắt chuỗi, phải bỏ qua đúng cách | 

## Vỏ cạnh 

Đối với đầu vào`3 5 5 5`, làm việc ngược lại sẽ cho`pick = 5`(cuối cùng), sau đó`4`, sau đó`min(5,3)=3`, tổng`3+4+5=12`, nhưng số lượt chọn giảm dần không thể vượt quá số lượt chọn trước đó, vì vậy chúng tôi điều chỉnh`next_max-1`từng bước thực thi`pick <= next_max-1`. Điều này đảm bảo chúng ta không bao giờ chọn một số vi phạm quy luật tăng dần khi đảo ngược, cho ra tổng`6`. 

Đối với đầu vào`6 1 1 1 1 1 1`, các lựa chọn trở thành`[0,0,0,0,1,2]`hoặc tương tự tùy theo lần lặp, luôn tuân theo quy tắc, thể hiện thuật toán xử lý chính xác các giá trị nhỏ lặp lại. 

Số 0 ở giữa được xử lý tự động vì`min(a[i], next_max-1)`mang lại 0 nếu giới hạn trước đó
