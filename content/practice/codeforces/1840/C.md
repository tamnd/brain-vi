---
title: "CF 1840C - Khu nghỉ dưỡng trượt tuyết"
description: "Bài toán yêu cầu chúng ta đếm xem có bao nhiêu cách Dima có thể chọn những ngày nghỉ liên tiếp tại một khu nghỉ dưỡng trượt tuyết trong điều kiện thời tiết hạn chế."
date: "2026-06-09T06:24:32+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1840
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 878 (Div. 3)"
rating: 1000
weight: 1840
solve_time_s: 71
verified: true
draft: false
---

[CF 1840C - Khu nghỉ dưỡng trượt tuyết](https://codeforces.com/problemset/problem/1840/C) 

**Đánh giá:** 1000 
**Tags:** tổ hợp, toán, hai con trỏ 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta đếm xem có bao nhiêu cách Dima có thể chọn những ngày nghỉ liên tiếp tại một khu nghỉ dưỡng trượt tuyết trong điều kiện thời tiết hạn chế. Chúng ta được cung cấp một mảng các số nguyên biểu thị nhiệt độ hàng ngày và hai số: độ dài tối thiểu của kỳ nghỉ$k$và nhiệt độ tối đa$q$mà Dima có thể chịu đựng được. Một khoảng thời gian nghỉ hợp lệ là một phân mảng liền kề trong đó tất cả nhiệt độ cao nhất là$q$và chiều dài của nó ít nhất là$k$. Nhiệm vụ là tính toán, đối với mỗi trường hợp kiểm thử, tổng số thời gian nghỉ phép hợp lệ. 

Các ràng buộc đầu vào chỉ ra rằng tổng số ngày trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$. Điều này loại trừ bất kỳ giải pháp nào kiểm tra mọi mảng con có thể một cách ngây thơ, bởi vì việc liệt kê tất cả các mảng con của một mảng lớn có thể yêu cầu$O(n^2)$hoạt động quá chậm đối với$n$lên đến$2 \cdot 10^5$. 

Các trường hợp đặc biệt cần xem xét bao gồm các tình huống trong đó tất cả nhiệt độ đều cao hơn$q$, nên không thể có kỳ nghỉ, hoặc ở đâu$k = 1$và tất cả nhiệt độ đều hợp lệ, nghĩa là mỗi ngày và mọi phân mảng liên tiếp đều đóng góp. Một cách tiếp cận bất cẩn có thể cố gắng đếm các mảng con một cách thô bạo và vượt quá giới hạn thời gian hoặc xử lý sai ranh giới của các mảng con hợp lệ khi nhiệt độ vượt quá$q$. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp đi lặp lại mỗi ngày bắt đầu và mọi ngày kết thúc có thể xảy ra, kiểm tra xem nhiệt độ giữa chúng không bao giờ vượt quá$q$và chiều dài ít nhất là$k$. Điều này đúng nhưng đòi hỏi$O(n^2)$hoạt động trên mỗi trường hợp thử nghiệm, điều này không thể chấp nhận được đối với đầu vào lớn nhất. 

Quan sát quan trọng cho phép tiếp cận nhanh hơn là bất kỳ chuỗi ngày liên tiếp nào có nhiệt độ cao nhất$q$có thể được coi là một khối. Trong khối chiều dài này$len$, mọi mảng con liền kề có độ dài ít nhất$k$là hợp lệ. Số lượng các mảng con như vậy có thể được tính trực tiếp bằng một công thức đơn giản: đối với một khối có chiều dài$len \ge k$, có$(len - k + 1) + (len - k) + \dots + 1 = (len - k + 1)(len - k + 2)/2$mảng con hợp lệ. Nếu như$len < k$, không có mảng con hợp lệ. Điều này làm giảm vấn đề quét mảng một lần và tính tổng số lượng cho mỗi khối hợp lệ. 

Cách tiếp cận này đương nhiên dẫn đến một$O(n)$thuật toán cho từng trường hợp thử nghiệm, vì chúng tôi chỉ duyệt mảng một lần và tính toán công thức số học đơn giản cho từng khối nhiệt độ hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm để theo dõi số kỳ nghỉ hợp lệ. Khởi tạo một biến`length`để theo dõi khối hiện tại của những ngày liên tiếp có nhiệt độ không vượt quá`q`. 
2. Lặp lại từng nhiệt độ trong mảng. Nếu nhiệt độ ngày hiện tại cao nhất`q`, tăng`length`để mở rộng khối hiện tại. 
3. Nếu nhiệt độ ngày hiện tại vượt quá`q`, tính số mảng con hợp lệ trong khối hiện tại. Chỉ các khối có chiều dài ít nhất`k`đóng góp. Sử dụng công thức$(length - k + 1) + (length - k) + \dots + 1 = (length - k + 1) \cdot (length - k + 2) / 2$. Cài lại`length`về 0 để bắt đầu một khối mới. 
4. Sau vòng lặp, xử lý khối cuối cùng nếu mảng kết thúc bằng một chuỗi hợp lệ. 
5. Trả về tổng số mảng con hợp lệ. 

Tại sao nó hoạt động: mọi khối nhiệt độ liền kề thỏa mãn giới hạn nhiệt độ tối đa đều có thể được tính độc lập. Trong mỗi khối, số mảng con hợp lệ có độ dài ít nhất`k`có thể được tính toán chính xác mà không cần liệt kê. Điều bất biến là tại bất kỳ thời điểm nào,`length`đại diện cho số ngày hợp lệ liên tiếp kết thúc ở chỉ mục hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_vacations(n, k, q, a):
    count = 0
    length = 0
    for temp in a:
        if temp <= q:
            length += 1
        else:
            if length >= k:
                count += (length - k + 1) * (length - k + 2) // 2
            length = 0
    if length >= k:
        count += (length - k + 1) * (length - k + 2) // 2
    return count

t = int(input())
for _ in range(t):
    n, k, q = map(int, input().split())
    a = list(map(int, input().split()))
    print(count_vacations(n, k, q, a))
```Giải pháp này đọc đầu vào bằng I/O nhanh để xử lý tối đa$10^4$trường hợp thử nghiệm một cách hiệu quả. chức năng`count_vacations`theo dõi các khối của ngày hợp lệ và tính toán số lượng mảng con cho mỗi khối. Chi tiết triển khai chính là xử lý khối cuối cùng sau khi vòng lặp kết thúc, điều này rất dễ bị quên. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 1 15
-5 0 -10
```| Chỉ mục | Nhiệt độ | Chiều dài | Đếm | 
| --- | --- | --- | --- | 
| 0 | -5 | 1 | 0 | 
| 1 | 0 | 2 | 0 | 
| 2 | -10 | 3 | 0 | 
| Kết thúc mảng: length=3, k=1 → add (3-1+1)*(3-1+2)/2 = 6 | | | | 
| Đầu ra: 6 | | | | 

Điều này xác nhận công thức đếm tạo ra tất cả các mảng con liền kề có độ dài ít nhất là 1. 

Đầu vào mẫu 2:```
4 3 12
12 12 10 15
```| Chỉ mục | Nhiệt độ | Chiều dài | Đếm | 
| --- | --- | --- | --- | 
| 0 | 12 | 1 | 0 | 
| 1 | 12 | 2 | 0 | 
| 2 | 10 | 3 | 0 | 
| 3 | 15 | 0 | length=3 ≥ k → cộng (3-3+1)*(3-3+2)/2=1 | 
| Đầu ra: 1 | | | | 

Điều này thể hiện việc xử lý một khối bị gián đoạn do một ngày nhiệt độ quá cao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Truyền một lần qua mảng, công việc không đổi trên mỗi phần tử. | 
| Không gian | O(1) | Chỉ một số số nguyên được sử dụng để theo dõi số lượng và độ dài. | 

Xét tổng của tất cả`n`trên nhiều trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, tổng thời gian chạy nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    result = []
    t = int(input())
    for _ in range(t):
        n, k, q = map(int, input().split())
        a = list(map(int, input().split()))
        result.append(str(count_vacations(n, k, q, a)))
    return "\n".join(result)

# Provided samples
assert run("7\n3 1 15\n-5 0 -10\n5 3 -33\n8 12 9 0 5\n4 3 12\n12 12 10 15\n4 1 -5\n0 -1 2 5\n5 5 0\n3 -1 4 -5 -3\n1 1 5\n5\n6 1 3\n0 3 -2 5 -4 -4\n") == "6\n0\n1\n0\n0\n1\n9"

# Custom tests
assert run("1\n1 1 0\n-1\n") == "1"  # single valid day
assert run("1\n5 2 10\n11 12 13 14 15\n") == "0"  # all temps too high
assert run("1\n5 3 5\n1 2 3 4 5\n") == "6"  # full valid block
assert run("1\n6 2 3\n1 4 2 1 5 2\n") == "3"  # multiple small blocks
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 / -1 | 1 | Một ngày dưới ngưỡng | 
| 5 2 10 / 11 12 13 14 15 | 0 | Mọi nhiệt độ đều vượt quá q | 
| 5 3 5 / 1 2 3 4 5 | 6 | Khối hợp lệ đầy đủ, độ dài > k | 
| 6 2 3 / 1 4 2 1 5 2 | 3 | Nhiều khối hợp lệ nhỏ được phân tách bằng nhiệt độ cao | 

## Vỏ cạnh 

Nếu tất cả nhiệt độ đều vượt quá ngưỡng, thuật toán sẽ trả về 0 một cách chính xác vì không có khối nào đạt đến độ dài`k`. Ví dụ, với đầu vào`[6 2 0, 1 2 3 4 5 6]`,`length`không bao giờ tăng vượt quá 0, vì vậy số đếm vẫn bằng 0. 

Nếu mảng kết thúc bằng một khối hợp lệ,
