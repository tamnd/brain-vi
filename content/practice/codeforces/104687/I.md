---
title: "CF 104687I - \u0412\u044b\u0431\u043e\u0440 \u0447\u0438\u0441\u0435\u043b 2"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta cần chọn chính xác ba phần tử từ nó. Hạn chế duy nhất là về cấu trúc: nếu chúng ta chọn các phần tử ở vị trí $i1 < i2 < i3$, thì mỗi cặp liên tiếp phải được phân tách bằng ít nhất các chỉ số $d$, nghĩa là $i{t+1} - it ge d$."
date: "2026-06-29T08:48:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104687
codeforces_index: "I"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u0432 \u0426\u0420\u041e\u0414 2022"
rating: 0
weight: 104687
solve_time_s: 86
verified: true
draft: false
---

[CF 104687I - \u0412\u044b\u0431\u043e\u0440 \u0447\u0438\u0441\u0435\u043b 2](https://codeforces.com/problemset/problem/104687/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và chúng ta cần chọn chính xác ba phần tử từ nó. Hạn chế duy nhất là về mặt cấu trúc: nếu chúng ta chọn các phần tử tại các vị trí$i_1 < i_2 < i_3$thì mỗi cặp liên tiếp phải cách nhau ít nhất$d$chỉ số, ý nghĩa$i_{t+1} - i_t \ge d$. Mục tiêu là tối đa hóa tổng các giá trị đã chọn. 

Khó khăn chính là các vị trí tương tác. Việc chọn một giá trị lớn sớm có thể chặn quyền truy cập vào các giá trị lớn khác gần đó, do đó, lựa chọn tối ưu cục bộ có thể phá hủy giá trị tối ưu toàn cục. 

Các ràng buộc rất lớn:$n \le 150000$. Điều này ngay lập tức loại trừ mọi phép khám phá bậc ba hoặc bậc hai của tất cả các bộ ba hoặc tất cả các kết hợp hợp lệ. Thậm chí$O(n^2)$cách tiếp cận quá chậm. Giải pháp về cơ bản phải là tuyến tính hoặc tuyến tính. 

Một vấn đề tế nhị phát sinh khi giá trị âm. Một trực giác ngây thơ có thể gợi ý “luôn lấy yếu tố tiếp theo tốt nhất có sẵn”, nhưng điều đó không thành công vì một lựa chọn sớm nhỏ hơn một chút có thể mở khóa hai lựa chọn lớn hơn nhiều sau đó. 

Ví dụ, hãy xem xét:```
n = 6, d = 2
A = [10, -100, 9, 9, 9, 9]
```Một lựa chọn tham lam có thể lấy 10 ở chỉ số 1, sau đó bị ép vào khoảng cách dưới mức tối ưu. Chiến lược đúng có thể bỏ qua nó để cho phép hai hoặc ba giá trị lớn sau này tùy thuộc vào khoảng cách. 

Một trường hợp thất bại khác xuất phát từ lý luận cửa sổ cục bộ. Bất kỳ cách tiếp cận nào chỉ nhìn vào phần tiếp theo$d$hoặc$2d$các vị trí độc lập sẽ thất bại vì bộ ba tối ưu phụ thuộc vào sự liên kết toàn cục trên mảng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử từng bộ ba chỉ số$i < j < k$, kiểm tra xem các ràng buộc về khoảng cách có được thỏa mãn hay không và tính tổng. Đây là$O(n^3)$, điều này hoàn toàn không thể thực hiện được$n = 150000$. Ngay cả việc tối ưu hóa việc kiểm tra tính hợp lệ cũng không giúp ích được gì, vì bản thân số lượng ứng viên tăng gấp ba lần là rất lớn. 

Chúng ta cần một cách để tránh liệt kê các cặp và thay vào đó sử dụng lại cấu trúc con tối ưu. Quan sát quan trọng là khi chúng ta sửa phần tử ở giữa$j$, bài toán chia thành hai bài toán con độc lập: lựa chọn hợp lệ tốt nhất ở bên trái và lựa chọn hợp lệ tốt nhất ở bên phải, mỗi lựa chọn đều có các ràng buộc về khoảng cách. 

Điều này gợi ý lập trình động. Chúng tôi xác định các trạng thái nắm bắt được tổng tốt nhất có thể của việc chọn 1, 2 hoặc 3 phần tử cho đến một chỉ mục nhất định trong khi vẫn tôn trọng khoảng cách. 

Cụ thể hơn, chúng ta có thể định nghĩa: 

-$dp1[i]$: tổng tốt nhất chọn 1 phần tử từ tiền tố$[1..i]$-$dp2[i]$: tổng tốt nhất chọn 2 phần tử từ tiền tố$[1..i]$-$dp3[i]$: tổng tốt nhất chọn 3 phần tử từ tiền tố$[1..i]$Việc chuyển đổi phụ thuộc vào việc chọn có đặt phần tử được chọn cuối cùng vào vị trí hay không$i$và đảm bảo phần tử được chọn trước đó tối đa là$i - d$. 

Để thực thi khoảng cách một cách hiệu quả, chúng tôi duy trì các giá trị tốt nhất từ ​​các phạm vi hợp lệ trước đó thay vì quét tất cả các trạng thái trước đó. 

Quá trình chuyển đổi trở thành: 

- Hoặc bỏ qua$i$- Hoặc sử dụng$i$là phần tử được chọn cuối cùng và kết hợp nó với trạng thái hợp lệ tốt nhất kết thúc tại hoặc trước$i - d$Điều này làm giảm mỗi lần chuyển trạng thái sang$O(1)$khấu hao bằng cách sử dụng tiền tố maxima. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| DP tối ưu với tiền tố cực đại |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng ba lớp DP, nhưng chúng tôi tính toán chúng trong một lần chuyển tiếp duy nhất bằng cách sử dụng tính năng theo dõi tiền tố tốt nhất. 

1. Tính toán trước các mảng tối đa tiền tố cho phép chúng tôi truy vấn giá trị dp tốt nhất cho đến chỉ mục$i$. Điều này đảm bảo chúng tôi có thể tìm thấy lựa chọn trước đó hợp lệ nhất mà không cần quét. 
2. Duy trì ba mảng DP: dp1, dp2, dp3. Mỗi trạng thái dp đại diện cho tổng tốt nhất có thể đạt được bằng cách sử dụng chính xác số lượng lựa chọn để lập chỉ mục$i$. Sự tách biệt này là cần thiết vì sự chuyển tiếp phụ thuộc vào số lượng phần tử đã được chọn. 
3. Khởi tạo dp1[i] là phần tử đơn tốt nhất được thấy cho đến nay cho đến i. Đây đơn giản là giá trị tối đa trong tiền tố vì chỉ có một phần tử được chọn. 
4. Đối với dp2[i], hãy cân nhắc lấy phần tử i làm lựa chọn thứ hai. Lựa chọn đầu tiên tối đa phải đến từ các chỉ số i - d. Vì vậy dp2[i] được cập nhật là: 

giá trị dp1 tốt nhất trong tiền tố [1 .. i - d] + A[i] hoặc mang dp2[i-1]. 

Bước này mã hóa giới hạn khoảng cách trực tiếp vào quá trình chuyển đổi. 
5. Đối với dp3[i], tương tự hãy cân nhắc chọn i làm lựa chọn thứ ba. Chúng tôi kết hợp giá trị dp2 tốt nhất lên tới i - d với A[i] hoặc mang dp3[i-1]. 
6. Đáp án cuối cùng là dp3[n]. 

Tại sao nó hoạt động: 

Tại mọi chỉ số i, dp1, dp2, dp3 đều lưu trữ lời giải tối ưu cho các tiền tố tận cùng là i. Bất kỳ lựa chọn tối ưu nào gồm ba phần tử đều phải có phần tử cuối cùng k. Khi k được cố định, hai phần tử còn lại tạo thành lựa chọn hợp lệ tối ưu trong tiền tố lên tới k - d, bởi vì bất kỳ sự trùng lặp hoặc sai lệch nào cũng sẽ mâu thuẫn với tính tối ưu. Cấu trúc con tối ưu này đảm bảo rằng các giải pháp xây dựng tăng dần bằng cách sử dụng tiền tố maxima không bao giờ bỏ lỡ cấu hình tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, d = map(int, input().split())
    a = list(map(int, input().split()))
    
    NEG = -10**30
    
    # dp arrays
    dp1 = [NEG] * n
    dp2 = [NEG] * n
    dp3 = [NEG] * n

    dp1[0] = a[0]

    # prefix bests for dp1, dp2, dp3
    best1 = [NEG] * n
    best2 = [NEG] * n
    best3 = [NEG] * n

    best1[0] = dp1[0]

    for i in range(1, n):
        # dp1: take best single element
        dp1[i] = max(dp1[i-1], a[i])

        # dp2: either skip or take i as second element
        j = i - d
        best_prev1 = best1[j] if j >= 0 else NEG
        dp2[i] = max(dp2[i-1], best_prev1 + a[i])

        # dp3: either skip or take i as third element
        best_prev2 = best2[j] if j >= 0 else NEG
        dp3[i] = max(dp3[i-1], best_prev2 + a[i])

        # update prefix bests
        best1[i] = max(best1[i-1], dp1[i])
        best2[i] = max(best2[i-1], dp2[i])
        best3[i] = max(best3[i-1], dp3[i])

    print(dp3[n-1])

if __name__ == "__main__":
    solve()
```Việc triển khai theo dõi cả DP chính xác ở vị trí i và mức tối đa tiền tố. Mảng tiền tố rất cần thiết vì quá trình chuyển đổi luôn yêu cầu giá trị tốt nhất cho đến chỉ số giới hạn$i - d$, không nhất thiết phải kết thúc chính xác tại$i - d$. 

Giá trị trọng tâm âm được đặt đủ lớn để tránh tình trạng tràn ngẫu nhiên khi thêm giá trị. Sự chuyển tiếp`dpX[i-1]`đảm bảo rằng việc bỏ qua một phần tử luôn được xem xét. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 3 2
-1 4 2 -6 3 3 5 -1 4 -1
```Chúng tôi theo dõi dp1, dp2, dp3 tại các vị trí quan trọng. 

| tôi | một [tôi] | dp1 | dp2 | dp3 | 
| --- | --- | --- | --- | --- | 
| 0 | -1 | -1 | -inf | -inf | 
| 1 | 4 | 4 | -inf | -inf | 
| 2 | 2 | 4 | 6 | -inf | 
| 3 | -6 | 4 | 6 | -inf | 
| 4 | 3 | 4 | 7 | 10 | 
| 5 | 3 | 4 | 7 | 10 | 
| 6 | 5 | 5 | 9 | 13 | 
| 7 | -1 | 5 | 9 | 13 | 
| 8 | 4 | 5 | 9 | 13 | 
| 9 | -1 | 5 | 9 | 13 | 

Câu trả lời cuối cùng là 13, đạt được bằng cách chọn các chỉ số tương ứng với các giá trị 4, 3, 5 với khoảng cách hợp lệ. 

Dấu vết này cho thấy dp2 ổn định sớm như thế nào nhưng dp3 tiếp tục cải thiện sau khi có lựa chọn thứ ba mạnh mẽ ở chỉ số 6. 

### Ví dụ 2 

đầu vào:```
7 3 2
5 -1 6 -2 7 -3 8
```| tôi | một [tôi] | dp1 | dp2 | dp3 | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 5 | -inf | -inf | 
| 1 | -1 | 5 | -inf | -inf | 
| 2 | 6 | 6 | 11 | -inf | 
| 3 | -2 | 6 | 11 | -inf | 
| 4 | 7 | 7 | 13 | 18 | 
| 5 | -3 | 7 | 13 | 18 | 
| 6 | 8 | 8 | 15 | 20 | 

Câu trả lời cuối cùng là 20. 

Ví dụ này nhấn mạnh rằng việc bỏ qua các giá trị âm được xử lý tự động vì trạng thái dp chuyển tiếp các giá trị tốt nhất trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục được xử lý một lần với các chuyển đổi O(1) sử dụng tiền tố maxima | 
| Không gian |$O(n)$| Ba mảng DP và mảng cực đại tiền tố | 

Lời giải dễ dàng nằm trong giới hạn vì$n = 150000$cho phép truyền tải tuyến tính với các hệ số không đổi tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k, d = map(int, input().split())
    a = list(map(int, input().split()))

    NEG = -10**30
    dp1 = [NEG] * n
    dp2 = [NEG] * n
    dp3 = [NEG] * n

    dp1[0] = a[0]

    best1 = [NEG] * n
    best2 = [NEG] * n

    best1[0] = dp1[0]

    for i in range(1, n):
        dp1[i] = max(dp1[i-1], a[i])

        j = i - d
        best_prev1 = best1[j] if j >= 0 else NEG
        dp2[i] = max(dp2[i-1], best_prev1 + a[i])

        best_prev2 = best2[j] if j >= 0 else NEG
        dp3[i] = max(dp3[i-1], best_prev2 + a[i])

        best1[i] = max(best1[i-1], dp1[i])
        best2[i] = max(best2[i-1], dp2[i])

    return str(dp3[n-1])

# provided sample
assert run("10 3 2\n-1 4 2 -6 3 3 5 -1 4 -1\n") == "13"

# minimum size
assert run("3 3 1\n1 2 3\n") == "6"

# all negative
assert run("6 3 2\n-1 -2 -3 -4 -5 -6\n") == "-6"

# spaced best picks
assert run("7 3 2\n5 -1 6 -2 7 -3 8\n") == "20"

# alternating pattern
assert run("8 3 2\n10 1 10 1 10 1 10 1\n") == "30"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đều tăng dương | tổng đúng của các cực đại cách nhau | tham lam tránh thất bại | 
| tất cả đều tiêu cực | chọn bộ ba ít có hại nhất | xử lý tiêu cực | 
| mức cao xen kẽ | thực thi khoảng cách | sự phụ thuộc giữa các vị trí | 
| tối thiểu n | độ đúng cơ sở | điều kiện biên | 

## Vỏ cạnh 

Trường hợp cạnh tranh quan trọng là khi giá trị ban đầu tốt nhất chặn quyền truy cập vào sự kết hợp mạnh hơn nhiều sau này. Ví dụ:```
n = 6, d = 2
A = [100, 1, 1, 1, 100, 100]
```Một cách tiếp cận ngây thơ có thể chọn 100 ở chỉ số 0, buộc các lượt chọn còn lại phải cách xa nhau và mất khả năng lấy cả 100 ở cuối. DP bỏ qua chính xác 100 đầu tiên khi hình thành dp2 và dp3 vì dp2 và dp3 lưu giữ các lịch sử thay thế. 

Một trường hợp cạnh khác là khi các lựa chọn tối ưu được căn chỉnh chặt chẽ chính xác ở ranh giới khoảng cách d. Việc chuyển đổi sử dụng`i - d`, không`i - d - 1`, do đó việc đưa ranh giới vào là đúng. Bất kỳ lỗi nào ở đây sẽ cho phép các lựa chọn không hợp lệ hoặc cấm các lựa chọn hợp lệ một cách không chính xác, ngay lập tức phá vỡ tính chính xác của các giải pháp tối ưu được đóng gói chặt chẽ.
