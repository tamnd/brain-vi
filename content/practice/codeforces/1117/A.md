---
title: "CF 1117A - Phân khúc tốt nhất"
description: "Chúng ta được cung cấp một mảng các số nguyên và có thể chọn bất kỳ mảng con liền kề nào. Đối với mỗi mảng con được chọn, chúng ta có thể tính giá trị trung bình số học của nó, bằng tổng các phần tử của nó chia cho độ dài của nó. Nhiệm vụ là tìm giá trị trung bình lớn nhất có thể có trong số tất cả các mảng con."
date: "2026-06-12T04:42:16+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1117
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 60 (Rated for Div. 2)"
rating: 1100
weight: 1117
solve_time_s: 330
verified: false
draft: false
---

[CF 1117A - Phân đoạn hay nhất](https://codeforces.com/problemset/problem/1117/A) 

**Đánh giá:** 1100 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 5 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và có thể chọn bất kỳ mảng con liền kề nào. Đối với mỗi mảng con được chọn, chúng ta có thể tính giá trị trung bình số học của nó, bằng tổng các phần tử của nó chia cho độ dài của nó. 

Nhiệm vụ là tìm giá trị trung bình lớn nhất có thể có trong số tất cả các mảng con. Nếu một số mảng con đạt được cùng giá trị trung bình tối đa đó, chúng ta phải xuất ra độ dài của mảng con dài nhất đó. 

Độ dài mảng tối đa là$10^5$, điều này ngay lập tức loại trừ mọi cách tiếp cận xem xét tất cả$O(n^2)$mảng con. Ngay cả việc tính toán giá trị trung bình của mỗi mảng con với tổng tiền tố vẫn cần khoảng$5 \cdot 10^9$mảng con trong trường hợp xấu nhất, vượt xa những gì có thể được xử lý trong vòng một giây. 

Bản thân các giá trị có thể lớn bằng$10^9$, do đó số tiền có thể trở nên lớn. Python xử lý việc này một cách tự động, nhưng trong các ngôn ngữ có số nguyên có chiều rộng cố định, chúng ta sẽ cần số học 64 bit. 

Phần khó khăn là hiểu mức trung bình tối đa có thể là bao nhiêu. 

Hãy xem xét mảng:```
5
3 7 2 5 1
```Phần tử lớn nhất là 7. Không có mảng con nào có thể có giá trị trung bình lớn hơn 7 vì mọi phần tử đóng góp vào giá trị trung bình nhiều nhất là 7. Mảng con một phần tử`[7]`đạt được mức trung bình là 7, do đó mức trung bình tối đa có thể đạt được chính xác là 7. 

Một trường hợp tinh vi khác là khi giá trị lớn nhất xuất hiện liên tiếp:```
5
4 9 9 9 2
```Mảng con`[9,9,9]`có trung bình là 9. Bất kỳ mảng con nào ngắn hơn chỉ bao gồm 9 số cũng có trung bình là 9, nhưng bài toán yêu cầu dãy con dài nhất. Câu trả lời đúng là 3. 

Một giải pháp bất cẩn có thể chỉ đếm số lần giá trị tối đa xuất hiện trong toàn bộ mảng. Điều đó không thành công trên:```
5
9 1 9 1 9
```Giá trị tối đa xuất hiện ba lần, nhưng không có đoạn liền kề nào có độ dài 3 bao gồm toàn bộ 9 giây. Đoạn hợp lệ dài nhất có độ dài 1. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê mọi mảng con, tính giá trị trung bình của nó, theo dõi giá trị trung bình lớn nhất được tìm thấy và trong số đó chọn độ dài dài nhất. 

Với tổng tiền tố, mỗi giá trị trung bình có thể được tính theo thời gian không đổi, nhưng vẫn có$O(n^2)$mảng con. Vì$n=10^5$, điều này có nghĩa là có khoảng năm tỷ ứng viên, điều này hoàn toàn không khả thi. 

Quan sát quan trọng xuất phát từ một tính chất đơn giản của giá trị trung bình. 

Cho phép$M$là giá trị lớn nhất trong mảng. 

Không có mảng con nào có thể có giá trị trung bình lớn hơn$M$, bởi vì mọi phần tử bên trong mảng con đó nhiều nhất là$M$. Giá trị trung bình không bao giờ có thể vượt quá giá trị lớn nhất được tính trung bình. 

Đồng thời, bất kỳ mảng con nào bao gồm toàn bộ các phần tử bằng$M$có mức trung bình chính xác$M$. 

Điều này ngay lập tức cho chúng ta biết rằng mức trung bình tối đa có thể đạt được luôn là$M$. 

Bây giờ vấn đề trở nên đơn giản hơn nhiều. Những mảng con nào có giá trị trung bình chính xác$M$? 

Giả sử một mảng con chứa ít nhất một phần tử nhỏ hơn$M$. Vì tất cả các phần tử nhiều nhất$M$, việc đưa ra ngay cả một giá trị nhỏ hơn cũng buộc giá trị trung bình ở dưới$M$. 

Vì vậy, một mảng con đạt được mức trung bình$M$khi và chỉ khi mọi phần tử trong đó bằng$M$. 

Câu trả lời đơn giản là độ dài của khối phần tử tối đa liên tiếp dài nhất. 

Trước tiên, chúng tôi tìm giá trị lớn nhất trong mảng, sau đó quét mảng và tính toán lần chạy dài nhất bao gồm toàn bộ giá trị đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$hoặc$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Tính giá trị lớn nhất$M$trong mảng. 
3. Quét mảng từ trái sang phải trong khi vẫn duy trì độ dài của chuỗi giá trị liên tiếp hiện tại bằng$M$. 
4. Khi phần tử hiện tại bằng$M$, tăng thời lượng chạy hiện tại lên một. 
5. Khi phần tử hiện tại nhỏ hơn$M$, kết thúc lần chạy hiện tại và đặt lại độ dài của nó về 0. 
6. Duy trì thời lượng chạy lớn nhất được thấy trong quá trình quét. 
7. Xuất ra chiều dài chạy lớn nhất. 

### Tại sao nó hoạt động 

Phần tử lớn nhất trong mảng là$M$. Vì mỗi phần tử mảng nhiều nhất là$M$, không có mảng con nào có thể có giá trị trung bình lớn hơn$M$. 

Một mảng con có giá trị trung bình chính xác$M$chỉ khi mọi phần tử bên trong nó bằng$M$. Nếu thậm chí một phần tử nhỏ hơn$M$, giá trị trung bình trở nên nhỏ hơn$M$. 

Do đó, mọi mảng con tối ưu chính xác là một khối liền kề của các phần tử tối đa. Trong số tất cả các khối như vậy, bài toán yêu cầu khối dài nhất. Quá trình quét tính toán chính xác số lượng đó nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    mx = max(a)

    best = 0
    cur = 0

    for x in a:
        if x == mx:
            cur += 1
            best = max(best, cur)
        else:
            cur = 0

    print(best)

if __name__ == "__main__":
    solve()
```Bước đầu tiên là tìm phần tử lớn nhất. Điều này xác định mức trung bình cao nhất có thể mà bất kỳ mảng con nào có thể đạt được. 

Quá trình quét tiếp theo giữ hai biến.`cur`lưu trữ độ dài của khối phần tử tối đa liên tiếp hiện tại, trong khi`best`lưu trữ khối lớn nhất như vậy gặp phải cho đến nay. 

Bất cứ khi nào nhìn thấy phần tử tối đa, khối hiện tại sẽ mở rộng thêm một phần tử. Bất cứ khi nào một phần tử nhỏ hơn xuất hiện, khối phải kết thúc vì bất kỳ đoạn nào đi qua vị trí đó sẽ chứa giá trị dưới mức tối đa và không thể đạt được mức trung bình tối ưu nữa. 

Thứ tự cập nhật có vấn đề một chút. Đầu tiên chúng tôi tăng`cur`, sau đó cập nhật`best`, đảm bảo các lượt chạy kết thúc ở vị trí hiện tại được tính chính xác. 

Không cần xử lý đặc biệt đối với các giá trị lớn vì chúng tôi không bao giờ tính tổng hoặc trung bình. Chúng tôi chỉ so sánh các phần tử với giá trị tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
6 1 6 6 0
```Giá trị tối đa là 6. 

| Vị trí | Giá trị | Chạy hiện tại | Tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 6 | 1 | 1 | 
| 2 | 1 | 0 | 1 | 
| 3 | 6 | 1 | 1 | 
| 4 | 6 | 2 | 2 | 
| 5 | 0 | 0 | 2 | 

Đầu ra:```
2
```Dấu vết hiển thị hai khối phần tử tối đa riêng biệt. Khối dài nhất là đoạn`[6, 6]`, có độ dài bằng 2. 

### Ví dụ 2 

đầu vào:```
7
5 5 5 1 5 5 2
```Giá trị tối đa là 5. 

| Vị trí | Giá trị | Chạy hiện tại | Tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 5 | 1 | 1 | 
| 2 | 5 | 2 | 2 | 
| 3 | 5 | 3 | 3 | 
| 4 | 1 | 0 | 3 | 
| 5 | 5 | 1 | 3 | 
| 6 | 5 | 2 | 3 | 
| 7 | 2 | 0 | 3 | 

Đầu ra:```
3
```Ví dụ này chứng tỏ rằng không thể hợp nhất các nhóm giá trị lớn nhất riêng biệt. Bất kỳ giá trị can thiệp nào nhỏ hơn sẽ làm giảm mức trung bình xuống dưới mức tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lượt để tìm mức tối đa và một lượt để tìm lượt chạy dài nhất | 
| Không gian |$O(1)$| Chỉ có một vài biến được sử dụng | 

Với$n \le 10^5$, quét tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ mảng đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    mx = max(a)

    best = 0
    cur = 0

    for x in a:
        if x == mx:
            cur += 1
            best = max(best, cur)
        else:
            cur = 0

    return str(best)

# provided sample
assert run("5\n6 1 6 6 0\n") == "2", "sample 1"

# minimum size
assert run("1\n7\n") == "1", "single element"

# all equal values
assert run("4\n5 5 5 5\n") == "4", "whole array"

# separated maximum values
assert run("5\n9 1 9 1 9\n") == "1", "non-contiguous maxima"

# maximum block at the end
assert run("6\n1 2 8 8 8 8\n") == "4", "suffix block"

# maximum block at the beginning
assert run("6\n10 10 10 3 2 1\n") == "3", "prefix block"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`1`| Kích thước mảng tối thiểu | 
|`4 / 5 5 5 5`|`4`| Toàn bộ mảng là tối ưu | 
|`5 / 9 1 9 1 9`|`1`| Giá trị tối đa được phân tách bằng giá trị nhỏ hơn | 
|`6 / 1 2 8 8 8 8`|`4`| Khối dài nhất xuất hiện ở cuối | 
|`6 / 10 10 10 3 2 1`|`3`| Khối dài nhất xuất hiện ở đầu | 

## Vỏ cạnh 

Hãy xem xét:```
1
42
```Giá trị tối đa là 42. Quá trình quét gặp một phần tử tối đa, do đó`cur = 1`Và`best = 1`. Câu trả lời là 1, đây là mảng con duy nhất có thể. 

Coi như:```
5
9 1 9 1 9
```Giá trị tối đa là 9. Quá trình quét liên tục bắt đầu và kết thúc: 

| Giá trị | cur | tốt nhất | 
| --- | --- | --- | 
| 9 | 1 | 1 | 
| 1 | 0 | 1 | 
| 9 | 1 | 1 | 
| 1 | 0 | 1 | 
| 9 | 1 | 1 | 

Câu trả lời là 1. Đếm tổng số lần xuất hiện của 9 sẽ cho kết quả không chính xác là 3, nhưng những lần xuất hiện đó không liền kề nhau. 

Coi như:```
6
7 7 7 7 7 7
```Mọi phần tử đều bằng mức tối đa. Độ dài chạy tăng từ 1 lên 6 mà không bị gián đoạn. Câu trả lời là 6, tương ứng với toàn bộ mảng. Vì mỗi mảng con có trung bình là 7 nên mảng tối ưu dài nhất là toàn bộ mảng. 

Coi như:```
6
1 2 8 8 8 8
```Khối phần tử tối đa dài nhất xảy ra ở cuối. Quá trình quét chính xác sẽ tiếp tục kéo dài quá trình chạy cho đến khi mảng kết thúc, tạo ra 4. Điều này xác nhận rằng không có lỗi sai sót nào khi phân đoạn tối ưu đạt đến vị trí cuối cùng.
