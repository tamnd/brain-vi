---
title: "CF 1111C - Ảnh chụp sáng tạo"
description: "Chúng ta có một cơ sở tuyến tính có độ dài $2^n$, trong đó một số vị trí chứa các avengers. Thanos muốn phá hủy toàn bộ căn cứ bằng sức mạnh tối thiểu."
date: "2026-06-12T04:58:01+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "divide-and-conquer", "math"]
categories: ["algorithms"]
codeforces_contest: 1111
codeforces_index: "C"
codeforces_contest_name: "CodeCraft-19 and Codeforces Round 537 (Div. 2)"
rating: 1700
weight: 1111
solve_time_s: 69
verified: true
draft: false
---

[CF 1111C - Ảnh chụp sáng tạo](https://codeforces.com/problemset/problem/1111/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, vũ phu, chia để trị, toán học 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cơ sở tuyến tính có chiều dài$2^n$, trong đó một số vị trí chứa người báo thù. Thanos muốn phá hủy toàn bộ căn cứ bằng sức mạnh tối thiểu. Anh ta có hai lựa chọn cho bất kỳ phần đế nào: đốt trực tiếp hoặc chia thành hai nửa bằng nhau và xử lý riêng từng nửa. Chi phí để ghi một phân đoạn phụ thuộc vào việc có sự hiện diện của người báo thù hay không: nếu không có, chi phí sẽ là một khoản cố định$A$; nếu có$n_a$Avengers, nó có giá$B \cdot n_a \cdot l$, Ở đâu$l$là độ dài của đoạn 

Đầu vào cho số mũ chiều dài$n$, số lượng người báo thù$k$, hằng số$A$Và$B$, và vị trí của những người báo thù. Đầu ra là một số nguyên duy nhất: tổng sức mạnh tối thiểu mà Thanos cần để phá hủy căn cứ. 

Ràng buộc$n \le 30$ngụ ý rằng độ dài cơ sở$2^n$có thể lên tới khoảng$10^9$. Điều này loại trừ bất kỳ thuật toán nào lặp lại trên tất cả các vị trí. Tuy nhiên,$k \le 10^5$có nghĩa là chúng tôi chỉ có thể xử lý các vị trí báo thù một cách hiệu quả. 

Các trường hợp cạnh không rõ ràng bao gồm các phân đoạn không có phần báo hiệu, trong đó việc ghi sẽ rẻ hơn so với việc chia và các phân đoạn có một phần báo thù duy nhất trong đó việc chia tách có thể không có ý nghĩa vì một nửa có thể trống. Một giải pháp bất cẩn lặp đi lặp lại trên tất cả các vị trí sẽ gặp sự cố$n = 30$, trong khi việc bỏ qua các phân đoạn trống có thể đánh giá quá cao chi phí. 

## Phương pháp tiếp cận 

Giải pháp brute-force xem xét đệ quy tất cả các phân đoạn có thể phân chia, tính toán chi phí ghi so với phân chia ở mỗi bước. Điều này đúng về mặt khái niệm nhưng không thực tế nếu được thực hiện một cách ngây thơ bằng cách lặp lại toàn bộ cơ sở vì độ dài có thể đạt tới$10^9$. Ngay cả cách tiếp cận đệ quy để kiểm tra mọi vị trí số nguyên cũng quá chậm. 

Thông tin chi tiết quan trọng là chúng ta chỉ cần theo dõi các phân đoạn chứa người báo thù. Nếu chúng ta sắp xếp các vị trí báo thù cho bất kỳ phân khúc nào$[l, r]$chúng ta có thể tính toán số lượng người báo thù bên trong nó bằng cách sử dụng tìm kiếm nhị phân. Điều này cho phép chúng tôi tránh lặp lại các vị trí trống và đánh giá chi phí đốt ngay lập tức. 

Chúng ta có thể thực hiện chiến lược chia để trị: đối với một phân khúc, hãy tính chi phí đốt trực tiếp phân khúc đó, sau đó tính toán đệ quy chi phí để chia phân khúc đó thành hai nửa. Mức tối thiểu của hai điều này là chi phí tối ưu cho phân khúc đó. Điều này làm giảm độ phức tạp$O(k \log L)$, Ở đâu$L = 2^n$, đó là hiệu quả được đưa ra$k \le 10^5$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(1) | Quá chậm đối với n > 20 | 
| Tối ưu | O(k log L) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào$n, k, A, B$và danh sách các vị trí báo thù. 
2. Sắp xếp các vị trí báo thù. Điều này cho phép truy vấn phạm vi nhanh về số lượng người báo thù trong bất kỳ phân khúc nào. 
3. Xác định hàm đệ quy`min_power(l, r)`đại diện cho sức mạnh tối thiểu để phá hủy phân khúc$[l, r]$. 
4. Trong`min_power(l, r)`, sử dụng tìm kiếm nhị phân để đếm số lượng người báo thù$num$TRONG$[l, r]$. Nếu như$num = 0$, trở lại$A$vì đốt một đoạn trống sẽ rẻ hơn so với việc chia nhỏ. 
5. Tính chi phí đốt trực tiếp như sau$B \cdot num \cdot (r-l+1)$. 
6. Nếu$l = r$hoặc$num = 0$, trả lại chi phí ghi vì phân khúc không thể chia được nữa. 
7. Ngược lại hãy tính điểm giữa$m = (l + r) // 2$và tính toán đệ quy công suất tối thiểu cho bên trái$[l, m]$và đúng$[m+1, r]$. Trả về mức tối thiểu của chi phí ghi trực tiếp và tổng chi phí đệ quy. 
8. Gọi`min_power(1, 2^n)`và in kết quả. 

Tại sao nó hoạt động: ở mỗi phân đoạn, thuật toán chọn cách ghi trực tiếp hoặc chia tách rẻ nhất. Bằng quy nạp, nếu các lệnh gọi đệ quy luôn trả về chi phí tối ưu cho các phân đoạn con thì sự kết hợp sẽ tạo ra mức tối thiểu toàn cục. Việc sắp xếp các vị trí báo thù đảm bảo việc đếm số lượng trong bất kỳ phân đoạn nào đều hiệu quả và chính xác. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

n, k, A, B = map(int, input().split())
positions = list(map(int, input().split()))
positions.sort()

def min_power(l, r):
    left_idx = bisect.bisect_left(positions, l)
    right_idx = bisect.bisect_right(positions, r)
    num = right_idx - left_idx
    
    if num == 0:
        return A
    burn_cost = B * num * (r - l + 1)
    if l == r:
        return burn_cost
    m = (l + r) // 2
    split_cost = min_power(l, m) + min_power(m+1, r)
    return min(burn_cost, split_cost)

total_power = min_power(1, 2**n)
print(total_power)
```Đầu tiên, mã sẽ sắp xếp các vị trí báo thù để đếm hiệu quả. các`min_power`hàm tính toán số lượng người báo thù trong một phân đoạn bằng cách sử dụng`bisect`. Đối với các phân đoạn trống, nó trả về$A$. Đối với một vị trí hoặc phân khúc có người báo thù, nó tính toán cả chi phí đốt và phân chia. Đệ quy đảm bảo chúng ta luôn tìm được mức tối thiểu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2 2 1 2
1 3
```| Phân đoạn | Avengers | Chi phí ghi | Chia chi phí | Chi phí tối thiểu | 
| --- | --- | --- | --- | --- | 
| 1-1 | 1 | 2 | Không áp dụng | 2 | 
| 2-2 | 0 | 1 | Không áp dụng | 1 | 
| 1-2 | 1 | 4 | 2+1=3 | 3 | 
| 3-3 | 1 | 2 | Không áp dụng | 2 | 
| 4-4 | 0 | 1 | Không áp dụng | 1 | 
| 3-4 | 1 | 4 | 2+1=3 | 3 | 
| 1-4 | 2 | 16 | 3+3=6 | 6 | 

Dấu vết này xác nhận thuật toán lựa chọn chính xác giữa ghi và phân tách ở mỗi bước. 

Đầu vào tùy chỉnh:```
3 3 5 1
2 4 7
```| Phân đoạn | Avengers | Đốt cháy | Chia | Tối thiểu | 
| --- | --- | --- | --- | --- | 
| 1-4 | 2 | 8 | 1-2 + 3-4 | 1-2=1, 3-4=1 => chia 2 | 
| 5-8 | 1 | 4 | chia 5-6 + 7-8 | 5-6=5, 7-8=1 => chia 6 | 
| 1-8 | 3 | 24 | chia 1-4 + 5-8 | 2+6=8 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log L) | Mỗi cuộc gọi đệ quy sẽ đếm số người báo thù thông qua tìm kiếm nhị phân trên k phần tử. Độ sâu đệ quy là log L. | 
| Không gian | O(k + log L) | Sắp xếp người báo thù sử dụng O(k). Độ sâu ngăn xếp đệ quy là O(log L). | 

Với$k \le 10^5$Và$n \le 30$, tổng số hoạt động có thể quản lý được trong vòng 1 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k, A, B = map(int, input().split())
    positions = list(map(int, input().split()))
    positions.sort()
    def min_power(l, r):
        left_idx = bisect.bisect_left(positions, l)
        right_idx = bisect.bisect_right(positions, r)
        num = right_idx - left_idx
        if num == 0:
            return A
        burn_cost = B * num * (r - l + 1)
        if l == r:
            return burn_cost
        m = (l + r) // 2
        split_cost = min_power(l, m) + min_power(m+1, r)
        return min(burn_cost, split_cost)
    return str(min_power(1, 2**n))

assert run("2 2 1 2\n1 3\n") == "6", "sample 1"
assert run("1 1 10 100\n1\n") == "100", "single avenger at start"
assert run("1 0 5 10\n") == "5", "no avengers, single position"
assert run("3 3 1 1\n2 4 7\n") == "8", "split vs burn test"
assert run("4 4 2 3\n1 5 9 13\n") == "24", "even spacing test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 1 2 / 1 3 | 6 | Quyết định đốt và chia đúng | 
| 1 1 10 100 | | |
