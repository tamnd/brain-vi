---
title: "CF 1760C - Ưu điểm"
description: "Chúng tôi có một danh sách các đối thủ cạnh tranh, mỗi đối thủ có một thế mạnh về số lượng. Đối với mọi đối thủ, chúng tôi muốn tính toán lợi thế của họ trước đối thủ mạnh nhất không phải là họ. Cụ thể, nếu mảng cường độ là [s1, s2, ..."
date: "2026-06-09T14:20:24+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1760
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 835 (Div. 4)"
rating: 800
weight: 1760
solve_time_s: 166
verified: false
draft: false
---

[CF 1760C - Ưu điểm](https://codeforces.com/problemset/problem/1760/C) 

**Đánh giá:** 800 
**Tas:** cấu trúc dữ liệu, triển khai, sắp xếp 
**Thời gian giải:** 2m 46s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một danh sách các đối thủ cạnh tranh, mỗi đối thủ có một thế mạnh về số lượng. Đối với mọi đối thủ, chúng tôi muốn tính toán lợi thế của họ trước đối thủ mạnh nhất không phải là họ. Cụ thể, nếu mảng điểm mạnh là`[s_1, s_2, ..., s_n]`, cho mỗi`i`chúng ta cần tìm giá trị lớn nhất trong số tất cả`s_j`với`j ≠ i`, và trừ nó khỏi`s_i`. Kết quả có thể âm tính nếu`s_i`nhỏ hơn tất cả những người khác. 

Các ràng buộc cho phép lên đến`2 * 10^5`người tham gia trong tất cả các trường hợp thử nghiệm, với sức mạnh của mỗi cá nhân lên tới`10^9`. Điều này ngụ ý rằng bất kỳ giải pháp nào lặp lại trên tất cả những người tham gia cho từng đối thủ cạnh tranh, dẫn đến các hoạt động O(n²) cho mỗi trường hợp thử nghiệm, sẽ quá chậm. Tổng trên tất cả`n`bị giới hạn ở`2 * 10^5`cho phép một giải pháp tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. Việc sắp xếp hoặc quét các mảng với số lần không đổi là khả thi. 

Các trường hợp chính cần chú ý là các mảng trong đó nhiều người tham gia chia sẻ sức mạnh tối đa. Nếu mức tối đa xảy ra nhiều lần, việc loại bỏ một trong số chúng vẫn giữ nguyên mức tối đa. Ví dụ, trong`[5, 5, 3]`, người tham gia đầu tiên nhìn thấy tối đa 5 trong số những người khác, đưa ra`5 - 5 = 0`, không`5 - 3 = 2`. Mảng có tất cả các giá trị bằng nhau là một trường hợp cạnh khác, ví dụ:`[4, 4, 4, 4]`, trong đó lợi thế của mọi người tham gia là bằng không. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực đơn giản sẽ lặp lại cho từng người tham gia, quét tất cả những người khác và chọn mức tối đa. Điều này có hiệu quả vì nó trực tiếp triển khai câu lệnh vấn đề, nhưng số lượng thao tác là`n * (n-1)`, xấp xỉ 4 * 10^10 trong trường hợp xấu nhất nếu`n`là 2 * 10^5. Như vậy là quá chậm. 

Thông tin chi tiết quan trọng để có cách tiếp cận nhanh hơn là giá trị tối đa của tất cả những người tham gia khác là giá trị tối đa toàn cầu của mảng hoặc, nếu người tham gia hiện tại giữ giá trị tối đa, là giá trị lớn thứ hai trong mảng. Vì vậy, chúng ta chỉ cần biết điểm mạnh lớn nhất và lớn thứ hai trong mỗi trường hợp thử nghiệm. Khi đã biết hai giá trị này, việc tính toán lợi thế của mỗi người tham gia sẽ trở thành một phép trừ duy nhất. Điều này giảm công việc xuống còn một lần tính toán hai giá trị lớn nhất và một lượt khác để tính toán các câu trả lời, đạt được O(n) cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và hàng loạt điểm mạnh`s`. 
3. Khởi tạo hai biến`max1`Và`max2`để lưu trữ giá trị lớn nhất và lớn thứ hai. Lặp lại qua`s`một lần, cập nhật các biến này. Nếu một số lớn hơn`max1`, giao phó`max2 = max1`Và`max1 = number`. Nếu nó ở giữa`max1`Và`max2`, giao phó`max2 = number`. 
4. Lặp lại mảng một lần nữa. Đối với mỗi người tham gia, nếu sức mạnh của họ bằng nhau`max1`, "tối đa khác" của họ là`max2`; nếu không thì đó là`max1`. 
5. Tính toán sự khác biệt giữa sức mạnh của người tham gia và "mức tối đa khác" tương ứng của họ và xuất ra. 

Tại sao nó hoạt động: ở mỗi bước,`max1`Và`max2`lưu trữ chính xác số lớn nhất và số lớn thứ hai. Đối với bất kỳ người tham gia nào, nếu họ là người lớn nhất, việc loại bỏ họ sẽ làm lộ ra`max2`là lớn nhất tiếp theo. Nếu không, việc loại bỏ chúng không thay đổi mức tối đa. Bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = list(map(int, input().split()))
    max1 = max2 = -1
    for num in s:
        if num > max1:
            max2 = max1
            max1 = num
        elif num > max2:
            max2 = num
    res = []
    for num in s:
        other_max = max2 if num == max1 else max1
        res.append(num - other_max)
    print(' '.join(map(str, res)))
```Vòng lặp đầu tiên tính toán hai giá trị lớn nhất. Vòng lặp thứ hai tính toán lợi thế cho mỗi người tham gia trong thời gian không đổi. sử dụng`max2 if num == max1 else max1`đảm bảo rằng nếu nhiều người tham gia chia sẻ mức tối đa thì mỗi người vẫn sử dụng đúng "tối đa khác". I/O nhanh ngăn chặn TLE khi xử lý đầu vào lớn. 

## Ví dụ đã hoạt động 

Theo dõi đầu vào`[4, 7, 3, 5]`: 

| Người tham gia | s_i | tối đa1 | tối đa2 | other_max | s_i - other_max | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 4 | 7 | 5 | 7 | -3 | 
| 7 | 7 | 7 | 5 | 5 | 2 | 
| 3 | 3 | 7 | 5 | 7 | -4 | 
| 5 | 5 | 7 | 5 | 7 | -2 | 

Điều này xác nhận rằng thuật toán phân biệt chính xác giữa người tham gia lớn nhất và những người khác. 

Theo dõi đầu vào`[4, 4, 4, 4]`: 

| Người tham gia | s_i | tối đa1 | tối đa2 | other_max | s_i - other_max | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 4 | 4 | 4 | 4 | 0 | 
| 4 | 4 | 4 | 4 | 4 | 0 | 
| 4 | 4 | 4 | 4 | 4 | 0 | 
| 4 | 4 | 4 | 4 | 4 | 0 | 

Điều này cho thấy trường hợp cạnh của tất cả các điểm mạnh bằng nhau được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lượt để tìm max1 và max2, một lượt để tính toán sự khác biệt | 
| Không gian | O(n) | Mảng đầu ra lưu trữ n giá trị cho mỗi trường hợp thử nghiệm | 

Vì tổng của`n`trên tất cả các trường hợp thử nghiệm không vượt quá 2 * 10^5, giải pháp dễ dàng chạy trong giới hạn thời gian 2 giây và trong bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = list(map(int, input().split()))
        max1 = max2 = -1
        for num in s:
            if num > max1:
                max2 = max1
                max1 = num
            elif num > max2:
                max2 = num
        res = []
        for num in s:
            other_max = max2 if num == max1 else max1
            res.append(num - other_max)
        print(' '.join(map(str, res)))
    return output.getvalue().strip()

# Provided samples
assert run("5\n4\n4 7 3 5\n2\n1 2\n5\n1 2 3 4 5\n3\n4 9 4\n4\n4 4 4 4\n") == "-3 2 -4 -2\n-1 1\n-4 -3 -2 -1 1\n-5 5 -5\n0 0 0 0"

# Custom cases
assert run("1\n2\n10 1\n") == "9 -9", "minimum size input"
assert run("1\n3\n7 7 3\n") == "0 0 -4", "duplicate maximum"
assert run("1\n4\n1 2 3 4\n") == "-3 -2 -1 1", "ascending order"
assert run("1\n5\n5 5 5 5 5\n") == "0 0 0 0 0", "all equal values"
assert run("1\n6\n1 1000000000 2 999999999 3 4\n") == "-999999999 1 -999999997 1 -999999996 -999999995", "large numbers and second max"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n10 1 | 9 -9 | Mảng kích thước tối thiểu, tính toán chênh lệch | 
| 3\n7 7 3 | 0 0 -4 | Trường hợp nhiều cạnh tối đa | 
| 4\n1 2 3 4 | -3 -2 -1 1 | Trình tự tăng dần | 
| 5\n5 5 5 5 5 | 0 0 0 0 0 | Tất cả các giá trị bằng nhau | 
| 6\n1 1000000000 2 999999999 3 4 | -999999999 1 -999999997 1 -999999996 -999999995 | Số lượng rất lớn, lựa chọn tối đa thứ hai | 

## Vỏ cạnh 

Nếu giá trị lớn nhất xuất hiện nhiều lần thì mỗi người tham gia giữ giá trị đó phải sử dụng giá trị lớn thứ hai trong số tất cả các số làm "giá trị tối đa khác". Ví dụ,`[7, 7, 3]`sản xuất
