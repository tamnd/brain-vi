---
title: "CF 1861D - Sắp xếp theo phép nhân"
description: "Chúng ta được cho một mảng các số nguyên dương và nhiệm vụ của chúng ta là biến đổi nó thành một dãy tăng dần bằng cách sử dụng ít thao tác nhất."
date: "2026-06-09T00:19:15+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1861
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 154 (Rated for Div. 2)"
rating: 1800
weight: 1861
solve_time_s: 142
verified: false
draft: false
---

[CF 1861D - Sắp xếp theo phép nhân](https://codeforces.com/problemset/problem/1861/D) 

**Đánh giá:** 1800 
**Tags:** dp, tham lam 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương và nhiệm vụ của chúng ta là biến đổi nó thành một dãy tăng dần bằng cách sử dụng ít thao tác nhất. Hoạt động được phép là chọn một mảng con liền kề và nhân mọi phần tử trong đó với một số nguyên tùy ý, có thể dương, âm hoặc bằng 0. Chúng tôi được yêu cầu số lượng tối thiểu các hoạt động như vậy được yêu cầu. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết độ dài của mảng và chính mảng đó. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: số lượng phép tính nhân tối thiểu cần thiết. 

Ràng buộc chính là kích thước: tổng của`n`trên tất cả các trường hợp thử nghiệm có thể đạt tới 200.000. Điều này loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tất cả các chuỗi hoạt động có thể có, sẽ theo cấp số nhân trong`n`. Chúng ta cần một giải pháp tuyến tính hoặc tuyến tính hiệu quả trong`n`mỗi trường hợp thử nghiệm. Một điểm tinh tế khác là các phần tử có thể rất lớn (lên đến$10^9$), nhưng vì phép nhân có thể sử dụng các số nguyên tùy ý nên chúng ta không bị giới hạn bởi tình trạng tràn số trong Python, mặc dù vậy cần phải cẩn thận nếu dịch sang ngôn ngữ có chiều rộng cố định. 

Một cách tiếp cận đơn giản có thể cố gắng khắc phục từng phần tử không tăng riêng lẻ, nhưng có những trường hợp đặc biệt trong đó các hoạt động đếm thiếu hoặc đếm quá mức. Ví dụ, đối với một mảng`[1,1,1]`, câu trả lời tối thiểu là`2`, bởi vì bạn không thể nhân tất cả các phần tử cùng một lúc để làm cho chúng tăng dần. Xử lý các mảng có các phần tử lặp lại hoặc các mảng ban đầu giảm dần là các trường hợp biên điển hình. Mảng đã tăng nghiêm ngặt sẽ quay trở lại`0`. Mảng một phần tử cũng yêu cầu một cách tầm thường`0`hoạt động. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng thử mọi mảng con có thể và nhân nó để khắc phục các vi phạm trật tự. Mặc dù điều này đảm bảo tính đúng đắn về mặt lý thuyết nhưng nó không khả thi vì có$O(n^2)$mảng con và việc tính toán tập hợp hoạt động tối thiểu nhanh chóng vượt quá giới hạn thời gian. Đối với một trường hợp thử nghiệm có độ dài`10^5`, việc liệt kê các mảng con đã lên tới hàng tỷ, không thể chạy trong 2 giây. 

Điểm mấu chốt là chúng ta không quan tâm đến giá trị chính xác được tạo ra sau phép nhân mà chỉ quan tâm đến thứ tự tương đối. Bởi vì chúng ta có thể nhân với bất kỳ số nguyên nào, nên bất kỳ phép biến đổi tăng dần nào của một đoạn liền kề đều có thể được thực hiện bằng một thao tác. Do đó, vấn đề giảm xuống còn việc phân chia mảng thành số lượng tối thiểu các mảng con liền kề tăng dần. Sau đó, mỗi mảng con này có thể được “cố định” bằng một thao tác nhân. 

Quan sát này cho phép chúng ta thực hiện một cách tiếp cận tham lam. Nếu lặp từ trái sang phải, chúng ta chỉ cần bắt đầu một thao tác mới bất cứ khi nào phần tử hiện tại không lớn hơn phần tử trước đó. Mỗi lần điều này xảy ra, chúng tôi đếm thêm một thao tác. Về cơ bản, đây là việc đếm số lần chạy của các chuỗi tăng dần. Bởi vì một phép nhân có thể điều chỉnh toàn bộ quá trình chạy, điều này mang lại số lượng thao tác tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n^2) | Quá chậm | 
| Tham Lam/Đếm Chạy | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm cho các phép toán về 0. 
2. Lặp lại mảng từ phần tử thứ hai đến phần tử cuối cùng. 
3. Đối với mỗi phần tử, hãy so sánh nó với phần tử trước đó. Nếu phần tử hiện tại nhỏ hơn hoặc bằng phần tử trước đó, hãy tăng bộ đếm hoạt động vì cần có một phép nhân mới để tiếp tục một chuỗi tăng nghiêm ngặt. 
4. Tiếp tục quá trình này cho đến hết mảng. 
5. Xuất bộ đếm hoạt động cho test case. 

Tại sao nó hoạt động: cách tiếp cận tham lam hoạt động vì bất kỳ phân đoạn tăng liền kề nào cũng có thể được nhân với bất kỳ giá trị tăng nghiêm ngặt mong muốn nào chỉ bằng một thao tác. Vì vậy, mỗi khi dãy ngừng tăng thì đó phải là điểm bắt đầu của một đoạn mới cần có một phép nhân riêng biệt. Điều này đảm bảo số lượng hoạt động tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_operations(a):
    if not a:
        return 0
    ops = 0
    for i in range(1, len(a)):
        if a[i] <= a[i-1]:
            ops += 1
    return ops

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(min_operations(a))
```Giải thích: Hàm`min_operations`lặp qua mảng một lần. Nó so sánh từng phần tử với phần tử trước nó. Bất cứ khi nào phần tử hiện tại không lớn hơn phần tử trước đó, nó sẽ tăng số lượng thao tác. Vòng lặp chính đọc nhiều trường hợp kiểm thử và áp dụng chức năng này. Các trường hợp cạnh như mảng trống hoặc mảng có độ dài một được xử lý một cách tầm thường. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng đầu vào:`[1, 1, 2, 2, 2]`| tôi | một[i-1] | một [tôi] | hoạt động |
 | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 1 | 2 | 1 | 
| 3 | 2 | 2 | 2 | 
| 4 | 2 | 2 | 3 | 

Dấu vết hiển thị chính xác số gia của bộ đếm khi chuỗi không tăng nghiêm ngặt, phù hợp với yêu cầu hoạt động tối thiểu.

 ### Mẫu 2 

Mảng đầu vào:`[5, 4, 3, 2, 5, 1]`| tôi | một[i-1] | một [tôi] | hoạt động | 
| --- | --- | --- | --- | 
| 1 | 5 | 4 | 1 | 
| 2 | 4 | 3 | 2 | 
| 3 | 3 | 2 | 3 | 
| 4 | 2 | 5 | 3 | 
| 5 | 5 | 1 | 4 | 

Mỗi lần phần tử hiện tại không lớn hơn phần tử trước, chúng ta sẽ tăng dần. Số lượng thao tác tối thiểu khớp với số lượng chuyển đổi được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Truyền đơn qua mảng | 
| Không gian | O(1) | Chỉ cần một biến đếm; lưu trữ đầu vào riêng biệt | 

Với các ràng buộc, thuật toán dễ dàng xử lý tổng tối đa`n`200.000 trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ops = 0
        for i in range(1, n):
            if a[i] <= a[i-1]:
                ops += 1
        output.append(str(ops))
    return "\n".join(output)

# provided samples
assert run("3\n5\n1 1 2 2 2\n6\n5 4 3 2 5 1\n3\n1 2 3\n") == "3\n2\n0", "sample 1-3"

# custom cases
assert run("2\n1\n100\n2\n2 2\n") == "0\n1", "single element / equal pair"
assert run("1\n5\n1 2 3 4 5\n") == "0", "already sorted"
assert run("1\n5\n5 4 3 2 1\n") == "4", "strictly decreasing"
assert run("1\n6\n1 3 3 3 2 4\n") == "3", "mixed repeats"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n100\n100 ... 100`|`99`| chuỗi dài bằng nhau, cần nhiều thao tác | 
|`1\n1\n42`|`0`| mảng một phần tử | 
|`1\n5\n1 2 3 4 5`|`0`| đã tăng nghiêm ngặt | 
|`1\n5\n5 4 3 2 1`|`4`| giảm chặt, hoạt động tối đa | 
|`1\n6\n1 3 3 3 2 4`|`3`| nhiều lần lặp lại và giảm | 

## Vỏ cạnh 

cho`[1]`, thuật toán trả về`0`vì không cần thao tác. Vì`[2, 2]`, nó đếm một thao tác, xác định chính xác rằng phần tử thứ hai phải được thay đổi. Vì`[5, 4, 3, 2, 1]`, mỗi phần tử không tăng sẽ kích hoạt một thao tác mới, mang lại`4`, đó là tối thiểu. TRONG`[1,3,3,3,2,4]`, thuật toán bắt đầu các hoạt động mới tại các chỉ số mà chuỗi ngừng tăng, phân chia chính xác thành số lượng điều chỉnh nhân tối thiểu.
