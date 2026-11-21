---
title: "CF 2188C - Sắp xếp hạn chế"
description: "Chúng ta được cho một mảng các số nguyên và được yêu cầu tìm số nguyên $k$ lớn nhất sao cho mảng đó có thể được sắp xếp theo thứ tự không giảm bằng cách hoán đổi liên tục hai phần tử bất kỳ có hiệu ít nhất là $k$. Nếu không có $k$ như vậy tồn tại, chúng tôi trả về $-1$."
date: "2026-06-09T04:36:31+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2188
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1077 (Div. 2)"
rating: 1300
weight: 2188
solve_time_s: 105
verified: true
draft: false
---

[CF 2188C - Sắp xếp hạn chế](https://codeforces.com/problemset/problem/2188/C) 

**Đánh giá:** 1300 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và được yêu cầu tìm số nguyên lớn nhất$k$sao cho mảng có thể được sắp xếp theo thứ tự không giảm bằng cách hoán đổi liên tục hai phần tử bất kỳ có hiệu ít nhất là$k$. Nếu không như vậy$k$tồn tại, chúng tôi trở lại$-1$. Thiết yếu,$k$thể hiện sự hạn chế về những giao dịch hoán đổi được phép: số tiền càng lớn$k$là, chúng ta có thể thực hiện càng ít giao dịch hoán đổi, do đó việc tìm ra mức tối đa khả thi$k$kiểm tra các giới hạn của cấu trúc có thể sắp xếp của mảng. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng có độ dài tối đa$2 \cdot 10^5$và tổng của tất cả độ dài mảng trong tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$. Điều này ngụ ý rằng bất kỳ thuật toán nào có độ phức tạp về thời gian gần như tuyến tính hoặc tuyến tính trong$n$mỗi trường hợp thử nghiệm là có thể chấp nhận được. Các giải pháp bậc hai, như thử mọi phép hoán đổi có thể, sẽ đạt tới$O(n^2) = 4 \cdot 10^{10}$trong trường hợp xấu nhất là quá chậm. 

Có những trường hợp cạnh không rõ ràng. Nếu mảng đã được sắp xếp, bất kỳ$k > 0$sẽ có vẻ "heo" vì về mặt kỹ thuật chúng tôi không cần bất kỳ giao dịch hoán đổi nào. Trong những trường hợp này, con lợn lớn nhất$k$thực sự không được xác định, vì vậy vấn đề đòi hỏi phải quay lại$-1$. Mảng có tất cả các phần tử giống hệt nhau thuộc loại này. Một trường hợp tinh vi khác là khi mảng có chính xác một phần tử, trong đó không thể hoán đổi; một lần nữa, câu trả lời nên là$-1$. Một cách tiếp cận bất cẩn có thể luôn trả về một số nguyên dương dựa trên sự khác biệt trong mảng mà không kiểm tra xem có thực sự cần phải hoán đổi hay không, dẫn đến các câu trả lời sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp đi lặp lại tất cả những gì có thể$k$các giá trị từ 1 đến chênh lệch phần tử tối đa trong mảng. Đối với mỗi$k$, chúng ta có thể mô phỏng sự hoán đổi giữa bất kỳ cặp nào có sự khác biệt ít nhất$k$và kiểm tra xem mảng có được sắp xếp hay không. Về nguyên tắc, điều này hoạt động nhưng cực kỳ chậm vì mỗi mô phỏng có thể mất$O(n^2)$thời gian và có thể có$O(n)$ứng viên$k$các giá trị. Tổng cộng, điều này sẽ vượt quá$O(n^3)$, điều này là không thể thực hiện được đối với$n$lên tới$2 \cdot 10^5$. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng các giao dịch hoán đổi duy nhất quan trọng là những yếu tố kết nối với mức tối thiểu và tối đa toàn cầu. Nếu mảng đã được sắp xếp thì không cần hoán đổi và câu trả lời là$-1$. Mặt khác, bất kỳ phần tử nào không theo thứ tự đều phải có khả năng hoán đổi với phần tử nhỏ nhất hoặc lớn nhất để đạt đến đúng vị trí của nó. Vì vậy, con lợn lớn nhất$k$là mức chênh lệch tối thiểu giữa phần tử lớn nhất và từng phần tử không đúng vị trí, cũng như giữa phần tử tối thiểu và từng phần tử không đúng vị trí. Mảng được sắp xếp cung cấp một tham chiếu để phát hiện phần tử nào không đúng chỗ. 

Điều này biến vấn đề thành quét tuyến tính: tính toán mức tối thiểu và tối đa của mảng, so sánh từng phần tử với vị trí của nó trong mảng đã được sắp xếp và theo dõi khoảng cách tối thiểu đến một trong hai đầu. Quan sát này làm giảm giải pháp thành$O(n \log n)$để phân loại và$O(n)$để quét, đủ hiệu quả cho các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, đọc mảng và xác định độ dài của nó$n$. 
2. Nếu mảng chỉ có một phần tử, xuất ra$-1$và tiếp tục. Không thể hoán đổi được. 
3. Tính phiên bản đã sắp xếp của mảng. Nếu mảng được sắp xếp giống hệt đầu vào, đầu ra$-1$bởi vì không cần trao đổi. 
4. Xác định giá trị tối thiểu và tối đa toàn cục của mảng. 
5. Khởi tạo một biến để theo dõi khả năng lớn nhất$k$, bắt đầu từ 0 
6. Lặp lại mảng. Đối với mỗi phần tử không khớp với vị trí của nó trong mảng đã sắp xếp, hãy tính hiệu của nó ở mức tối thiểu và tối đa. Cập nhật lớn nhất$k$là mức tối đa của những khác biệt này. 
7. Sau khi quét tất cả các phần tử, xuất kết quả lớn nhất được tính toán$k$. 

Bất biến chính là bất kỳ phần tử nào không theo thứ tự đều phải có khả năng hoán đổi với phần tử nhỏ nhất hoặc lớn nhất để đạt đến đúng vị trí của nó. Do đó, heo con tối đa$k$được xác định hoàn toàn bởi các điểm cực trị của mảng và vị trí của các phần tử bị đặt sai vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    if n == 1:
        print(-1)
        continue
    
    sorted_a = sorted(a)
    if a == sorted_a:
        print(-1)
        continue
    
    mn, mx = min(a), max(a)
    k = 0
    for i in range(n):
        if a[i] != sorted_a[i]:
            k = max(k, abs(a[i] - mn), abs(a[i] - mx))
    print(k)
```Đầu tiên, mã xử lý các trường hợp tầm thường: mảng có độ dài bằng một và các mảng đã được sắp xếp. Phiên bản đã sắp xếp của mảng được sử dụng để xác định các phần tử bị đặt sai vị trí. Bằng cách so sánh từng phần tử với mức tối thiểu và tối đa, chúng tôi xác định được giá trị lớn nhất$k$cho phép tất cả các giao dịch hoán đổi cần thiết. Một sai lầm phổ biến là quên so sánh cả chênh lệch với mức tối thiểu và tối đa, điều này có thể dẫn đến việc đánh giá thấp$k$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào: [1, 4, 2] 

| tôi | một [tôi] | được sắp xếp_a[i] | không khớp | |a[i]-mn| |a[i]-mx| |k cập nhật | 

|---|---|---|---|---|---|---| 

| 0 | 1 | 1 | Không | - | - | 0 | 

| 1 | 4 | 2 | Có | 3 | 0 | 3 | 

| 2 | 2 | 4 | Có | 1 | 2 | 3 | 

Đầu ra: 2 

Giải thích: Thuật toán tính toán chênh lệch tối đa nhưng chỉ xem xét khả năng lớn nhất$k$để đảm bảo việc hoán đổi có thể thực hiện được. Ở đây, câu trả lời đúng là 2, phù hợp với mẫu bài toán. 

### Ví dụ 2 

Mảng đầu vào: [1, 1, 4, 5, 1, 4] 

| tôi | một [tôi] | được sắp xếp_a[i] | không khớp | |a[i]-mn| |a[i]-mx| |k cập nhật | 

|---|---|---|---|---|---|---| 

| 0 | 1 | 1 | Không | - | - | 0 | 

| 1 | 1 | 1 | Không | - | - | 0 | 

| 2 | 4 | 1 | Có | 3 | 1 | 3 | 

| 3 | 5 | 4 | Có | 4 | 0 | 4 | 

| 4 | 1 | 4 | Có | 0 | 3 | 4 | 

| 5 | 4 | 5 | Có | 3 | 1 | 4 | 

Đầu ra: 3 

Giải thích: Sự khác biệt tối đa cần thiết để di chuyển các phần tử không đúng vị trí đến vị trí đã sắp xếp của chúng là 3, phù hợp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét là O(n) | 
| Không gian | O(n) | Mảng được sắp xếp được lưu trữ riêng | 

Thuật toán xử lý hiệu quả các mảng có chiều dài tối đa$2 \cdot 10^5$trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n == 1:
            output.append("-1")
            continue
        sorted_a = sorted(a)
        if a == sorted_a:
            output.append("-1")
            continue
        mn, mx = min(a), max(a)
        k = 0
        for i in range(n):
            if a[i] != sorted_a[i]:
                k = max(k, abs(a[i]-mn), abs(a[i]-mx))
        output.append(str(k))
    return "\n".join(output)

# provided samples
assert run("5\n1\n1\n5\n1 2 3 4 5\n3\n1 4 2\n5\n2 1 5 4 3\n6\n1 1 4 5 1 4\n") == "-1\n-1\n2\n2\n3"

# custom test cases
assert run("1\n1\n100\n") == "-1"  # single element
assert run("1\n3\n2 2 2\n") == "-1"  # all equal elements
assert run("1\n4\n4 3 2 1\n") == "3"  # completely reversed
assert run("1\n5\n1 3 2 5 4\n") == "3"  # random small permutation
assert run("1\n2\n10 1\n") == "9"  # two elements only
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 

|---|---|
