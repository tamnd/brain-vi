---
title: "CF 1768A - Độ lồi lớn nhất"
description: "Bài toán yêu cầu chúng ta tìm số nguyên lớn nhất x nhỏ hơn một số nguyên k cho trước sao cho tổng của x! và (x-1)! chia hết cho k. Nói một cách dễ hiểu, chúng ta muốn một số ngay dưới k trong đó việc cộng giai thừa của nó với giai thừa của số trước đó sẽ là bội số của k."
date: "2026-06-09T12:42:24+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1768
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 842 (Div. 2)"
rating: 800
weight: 1768
solve_time_s: 105
verified: true
draft: false
---

[CF 1768A - Độ lồi lớn nhất](https://codeforces.com/problemset/problem/1768/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta tìm số nguyên lớn nhất`x`nhỏ hơn một số nguyên nhất định`k`sao cho tổng của`x!`Và`(x-1)!`chia hết cho`k`. Nói một cách dễ hiểu, chúng tôi muốn một số ngay bên dưới`k`trong đó việc cộng giai thừa của nó với giai thừa của số trước đó sẽ là bội số của`k`. Mỗi trường hợp thử nghiệm là một số nguyên duy nhất`k`, và chúng ta cần xuất ra kết quả tương ứng`x`hoặc`-1`nếu không có số đó tồn tại. 

Nhìn vào những hạn chế,`k`có thể lớn tới hàng tỷ và chúng tôi có thể có tới mười nghìn trường hợp thử nghiệm. Điều này có nghĩa là bất kỳ giải pháp nào tính toán rõ ràng giai thừa cho các số tiến gần đến`k`là hoàn toàn không khả thi. Các giai thừa phát triển cực kỳ nhanh chóng và`100!`đã vượt quá`10^157`, vượt xa các phép toán số nguyên tiêu chuẩn trong thực tế. Do đó, một cách tiếp cận cố gắng tính toán giai thừa một cách trực tiếp cho các trường hợp lớn`k`sẽ không hoạt động. 

Một trường hợp cạnh tinh tế phát sinh với các giá trị rất nhỏ của`k`. Ví dụ, nếu`k = 3`, lớn nhất`x`là`2`bởi vì`2! + 1! = 2 + 1 = 3`, chia hết cho 3. Nếu chúng ta cố gắng một cách ngây thơ`x = k - 1`không cần kiểm tra, nó có thể hoạt động, nhưng chúng ta phải hiểu tại sao mô hình này lại đúng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ lặp đi lặp lại`x`từ`k-1`xuống tới`1`, tính toán`x! + (x-1)!`và kiểm tra khả năng chia hết cho`k`. Điều này đúng về mặt toán học nhưng lại thất bại về mặt tính toán. Tính giai thừa đến`10^9`là không thể. Ngay cả khi sử dụng số học mô-đun, chuỗi giai thừa nhanh chóng vượt quá bất kỳ kích thước số nguyên khả thi nào, do đó phương pháp này quá chậm và tốn nhiều bộ nhớ. 

Quan sát quan trọng là tổng`x! + (x-1)!`có thể được viết lại như`(x-1)! * (x + 1)`. Chúng tôi muốn biểu thức này chia hết cho`k`. Nếu như`x >= k`,`(x-1)!`đã chứa`k`như một yếu tố khi`k > 2`, nhưng chúng tôi bị giới hạn ở`x < k`. Vậy lớn nhất`x`cái đó hoạt động được`k-1`. Điều này là do`(k-1)! + (k-2)!`đơn giản hóa để`(k-2)! * k`, luôn chia hết cho`k`vì`k > 2`. Vì`k = 2`, ứng cử viên duy nhất là`x = 1`. 

Chúng ta có thể tóm tắt: với bất kỳ`k >= 3`, câu trả lời là`k-1`. Vì`k = 2`, câu trả lời là`1`. Không cần phải tính toán giai thừa một cách rõ ràng, điều này làm giảm bài toán thành số học đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) mỗi trường hợp thử nghiệm | O(1) | Quá chậm đối với k lớn | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,`t`. 
2. Với mỗi test, hãy đọc số nguyên`k`. 
3. Nếu`k`bằng 2, đầu ra 1. Đây là trường hợp duy nhất có giá trị lớn nhất`x`không phải`k-1`. 
4. Ngược lại, xuất ra`k-1`. Điều này hoạt động vì`(k-1)! + (k-2)! = (k-2)! * k`, chia hết cho`k`. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: thuộc tính quan trọng là hệ số nhân tố. Với mọi số nguyên`k > 2`,`(k-2)!`nhân với`k`đảm bảo khả năng chia cắt. Bằng cách luôn luôn lấy`x = k-1`, chúng tôi đảm bảo rằng chúng tôi đang tối đa hóa`x`trong khi giữ nó ít hơn`k`. Trường hợp đặc biệt`k = 2`xử lý giới hạn nhỏ nhất nơi mẫu giai thừa vẫn giữ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    k = int(input())
    if k == 2:
        print(1)
    else:
        print(k - 1)
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó sẽ kiểm tra trường hợp đặc biệt`k = 2`vì công thức tổng quát`k-1`sẽ sản xuất`1`, trùng khớp ngẫu nhiên, nhưng chúng tôi xử lý nó một cách rõ ràng cho rõ ràng. Đối với tất cả những người khác`k`, nó in`k-1`, bằng cách sử dụng sự đơn giản hóa toán học. Nhập liệu nhanh với`sys.stdin.readline`đảm bảo giải pháp chạy hiệu quả cho tối đa 10.000 trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

**Đầu vào:**`3`**k = 3** 

| k | x | (x-1)! | x! | x! + (x-1)! | Chia hết cho k? | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 2 | 1 | 2 | 3 | Có | 

Đây`x = 2`cho`2! + 1! = 3`, chia hết cho 3. Kết quả đầu ra của thuật toán`2`. 

**Đầu vào:**`6`| k | x | (x-1)! | x! | x! + (x-1)! | Chia hết cho k? | 
| --- | --- | --- | --- | --- | --- | 
| 6 | 5 | 24 | 120 | 144 | Có |`x = 5`cho`(5! + 4!) = 144`, chia hết cho`6`. Đầu ra của thuật toán`5`. 

Các dấu vết xác nhận rằng việc nhặt`x = k-1`tối đa hóa`x`đồng thời đảm bảo tính chia hết thông qua tính chất nhân tử hóa giai thừa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp kiểm thử được xử lý theo O(1), t 10^4 | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Với những hạn chế này, giải pháp sẽ thực thi tốt trong chưa đầy 1 giây và mức sử dụng bộ nhớ ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        k = int(input())
        if k == 2:
            print(1)
        else:
            print(k - 1)
    return output.getvalue().strip()

# provided samples
assert run("4\n3\n6\n8\n10\n") == "2\n5\n7\n9", "sample 1"

# custom cases
assert run("1\n2\n") == "1", "minimum k"
assert run("1\n1000000000\n") == "999999999", "maximum k"
assert run("3\n4\n5\n6\n") == "3\n4\n5", "small consecutive k"
assert run("1\n7\n") == "6", "prime k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 | Xử lý trường hợp k cạnh tối thiểu | 
| 1000000000 | 999999999 | Xử lý k rất lớn hiệu quả | 
| 4,5,6 | 3,4,5 | Tính đúng đắn cho các số nhỏ liên tiếp | 
| 7 | 6 | Tính đúng đắn của số nguyên tố k | 

## Vỏ cạnh 

cho`k = 2`, thuật toán trả về một cách rõ ràng`1`. Mặc dù`1! + 0! = 2`chia hết cho`2`, việc xử lý nó một cách riêng biệt sẽ đảm bảo mã rõ ràng và tránh hiểu sai công thức`k-1`. Vì`k = 10^9`, thuật toán không tính toán bất kỳ giai thừa nào và đưa ra trực tiếp`999999999`, tránh tràn và chạy liên tục. Với mọi số nguyên tố hoặc hợp số lớn hơn 2, việc lựa chọn`x = k-1`là hợp lệ, bởi vì việc phân tích nhân tử`(x-1)! * (x+1)`đảm bảo khả năng chia cắt. Điều này bao gồm mọi đầu vào có thể có trong các ràng buộc. 

Điều này hoàn thành bài xã luận. Nó cho thấy lý do từ việc thao tác giai thừa, nêu bật những hiểu biết quan trọng và đưa ra giải pháp chính xác, hoàn toàn hiệu quả.
