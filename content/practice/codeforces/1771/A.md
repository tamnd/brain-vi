---
title: "CF 1771A - Hossam và tổ hợp"
description: "Chúng ta được cung cấp một mảng cho từng trường hợp thử nghiệm và chúng ta được yêu cầu chỉ tập trung vào các cặp vị trí trong mảng đó. Từ mỗi cặp chỉ số riêng biệt được sắp xếp, chúng ta có thể tính toán sự khác biệt tuyệt đối giữa hai giá trị."
date: "2026-06-09T12:21:44+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1771
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 837 (Div. 2)"
rating: 900
weight: 1771
solve_time_s: 67
verified: true
draft: false
---

[CF 1771A - Hossam và Tổ hợp](https://codeforces.com/problemset/problem/1771/A) 

**Xếp hạng:** 900 
**Tags:** tổ hợp, toán, sắp xếp 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng cho từng trường hợp thử nghiệm và chúng ta được yêu cầu chỉ tập trung vào các cặp vị trí trong mảng đó. Từ mỗi cặp chỉ số riêng biệt được sắp xếp, chúng ta có thể tính toán sự khác biệt tuyệt đối giữa hai giá trị. Trong số tất cả những khác biệt này, có một giá trị tối đa duy nhất được xác định bởi các số nhỏ nhất và lớn nhất có trong mảng. 

Nhiệm vụ không phải là tìm ra sự khác biệt tối đa đó, một việc đơn giản mà là đếm xem có bao nhiêu cặp chỉ số có thứ tự thực sự đạt được nó. Vì các cặp được sắp xếp theo thứ tự nên việc chọn chỉ số i rồi j được coi là khác với việc chọn j rồi i, ngay cả khi chúng sử dụng hai giá trị giống nhau. 

Quan sát chính về các ràng buộc là tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là 100000. Điều này ngay lập tức loại trừ mọi giải pháp bậc hai so sánh tất cả các cặp. Một vòng lặp kép đơn giản cho mỗi trường hợp thử nghiệm sẽ giảm xuống còn khoảng 10¹⁰ thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Giải pháp phải giảm từng trường hợp thử nghiệm về thời gian tuyến tính. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các phần tử đều bằng nhau. Trong trường hợp đó, chênh lệch tối đa bằng 0 và mọi cặp chỉ số phân biệt có thứ tự đều thỏa mãn |a[i] − a[j]| = 0. Bất kỳ cách tiếp cận nào giả định chênh lệch tối đa đến từ các giá trị riêng biệt sẽ bị phá vỡ ở đây nếu nó không xử lý rõ ràng các điểm cuối bằng nhau. 

Một trường hợp cạnh khác xuất hiện khi giá trị tối đa và tối thiểu xuất hiện nhiều lần. Ví dụ: nếu mức tối thiểu xảy ra k lần và mức tối đa xảy ra m lần thì câu trả lời không phải là 1 hoặc 2 mà phụ thuộc vào tất cả các tổ hợp lần xuất hiện. Một lỗi thường gặp là chỉ tính một lần xuất hiện của mỗi điểm cuối thay vì tất cả các lần trùng lặp. 

## Phương pháp tiếp cận 

Một giải pháp brute-force kiểm tra mọi cặp có thứ tự (i, j), tính toán |a[i] − a[j]|, theo dõi sự khác biệt tối đa toàn cầu và đếm xem có bao nhiêu cặp đạt được nó. Điều này đúng nhưng yêu cầu hai vòng lặp lồng nhau, dẫn đến công việc O(n²) cho mỗi trường hợp thử nghiệm. Với n lên tới 100000, điều này là không thể. 

Thông tin chi tiết về cấu trúc quan trọng là luôn đạt được sự khác biệt tuyệt đối tối đa trong một mảng giữa giá trị tối thiểu toàn cục và giá trị tối đa toàn cục. Không có cặp nào khác có thể vượt quá khoảng cách này. Vì vậy, chúng ta không bao giờ cần so sánh các phần tử tùy ý; chúng ta chỉ cần biết mức tối thiểu và tối đa xuất hiện bao nhiêu lần. 

Khi chúng tôi biết giá trị tối thiểu xuất hiện cnt_min lần và giá trị tối đa xuất hiện cnt_max lần, mọi cặp thứ tự hợp lệ phải chọn một điểm cuối là điểm tối thiểu và điểm cuối kia là điểm tối đa. Vì thứ tự quan trọng nên cả hai hướng đều đóng góp, tạo ra các cặp cnt_min × cnt_max từ min đến max và cnt_max × cnt_min từ max đến min, với tổng số là 2 × cnt_min × cnt_max. 

Nếu giá trị tối thiểu và tối đa có cùng giá trị thì mọi phần tử đều giống hệt nhau và tất cả các cặp chỉ số riêng biệt được sắp xếp đều hợp lệ. Số đó là n × (n − 1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, quét mảng một lần để xác định giá trị nhỏ nhất và giá trị lớn nhất. Điều này xác định hai ứng cử viên duy nhất có thể tạo ra sự khác biệt tối đa. 
2. Đếm xem giá trị tối thiểu xuất hiện bao nhiêu lần và giá trị tối đa xuất hiện bao nhiêu lần. Những tần số này xác định có bao nhiêu lựa chọn để hình thành các cặp cực trị. 
3. Nếu giá trị tối thiểu và tối đa bằng nhau, hãy tính số cặp chỉ số phân biệt theo thứ tự trực tiếp là n × (n − 1), vì mọi cặp đều tạo ra chênh lệch bằng 0. 
4. Ngược lại, tính kết quả là 2 × cnt_min × cnt_max. Điều này giải thích cho cả hai hướng đặt hàng giữa các phần tử tối thiểu và tối đa. 
5. Xuất kết quả cho test case. 

### Tại sao nó hoạt động

Bất kỳ cặp nào có chênh lệch tuyệt đối tối đa đều phải liên quan đến cả hai cực trị tổng thể, vì bất kỳ giá trị trung gian nào cũng làm giảm khoảng cách có thể có. Cấu trúc mảng thu gọn vấn đề thành việc đếm điểm cuối thay vì kiểm tra tất cả các cặp. Bởi vì mọi lần xuất hiện của mức tối thiểu có thể kết hợp với mọi lần xuất hiện của mức tối đa theo cả hai hướng, nên việc đếm trở nên thuần túy tổ hợp và độc lập với các vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        mn = min(a)
        mx = max(a)
        
        if mn == mx:
            print(n * (n - 1))
            continue
        
        cnt_mn = 0
        cnt_mx = 0
        
        for x in a:
            if x == mn:
                cnt_mn += 1
            if x == mx:
                cnt_mx += 1
        
        print(2 * cnt_mn * cnt_mx)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc tất cả các trường hợp thử nghiệm và lặp qua từng mảng một cách độc lập. Nó tính toán mức tối thiểu và tối đa bằng cách sử dụng một lượt duy nhất được cung cấp bởi các hàm dựng sẵn của Python, tuyến tính theo thời gian. 

Trường hợp đặc biệt trong đó tất cả các giá trị bằng nhau được xử lý rõ ràng, vì nếu không thì cnt_min và cnt_max sẽ giống nhau và công thức chung sẽ giảm xuống 0 một cách không chính xác. 

Vòng đếm cẩn thận tăng cả hai bộ đếm một cách độc lập, vì cùng một phần tử chỉ có thể là tối thiểu và tối đa trong trường hợp thống nhất, đã được tách ra. 

Cuối cùng, công thức 2 × cnt_min × cnt_max được in trực tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
6 2 3 8 1
```| Bước | mn | mx | cnt_mn | cnt_mx | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Quét mảng | 1 | 8 | 1 | 1 | - | 
| Đếm | 1 | 1 | 1 | 1 | - | 
| Tính toán | - | - | - | - | 2 | 

Điều này chứng tỏ rằng chỉ tồn tại một cực tiểu và một cực đại, do đó chỉ có thể có hai hướng có thứ tự. 

### Ví dụ 2 

đầu vào:```
6
7 2 8 3 2 10
```| Bước | mn | mx | cnt_mn | cnt_mx | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Quét mảng | 2 | 10 | 2 | 1 | - | 
| Đếm | 2 | 1 | 2 | 1 | - | 
| Tính toán | - | - | - | - | 4 | 

Ở đây, mỗi lần xuất hiện có 2 cặp tối thiểu với tối đa là 10 ở cả hai hướng, cho ra 2 × 2 × 1 = 4 cặp có thứ tự. 

Những dấu vết này xác nhận rằng các bản sao đã được tính toán đầy đủ, đó là điểm mấu chốt của vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | quét một lần để tìm giá trị tối thiểu, tối đa và số lượng | 
| Không gian | O(1) thêm | chỉ các bộ đếm và biến được sử dụng | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm tối đa là 100000, do đó, việc quét tuyến tính trên mỗi trường hợp thử nghiệm dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            mn = min(a)
            mx = max(a)
            if mn == mx:
                print(n * (n - 1))
                continue
            cnt_mn = sum(1 for x in a if x == mn)
            cnt_mx = sum(1 for x in a if x == mx)
            print(2 * cnt_mn * cnt_mx)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided samples
assert run("""2
5
6 2 3 8 1
6
7 2 8 3 2 10
""") == "2\n4"

# all equal values
assert run("""1
4
5 5 5 5
""") == "12"

# min and max appear multiple times
assert run("""1
5
1 1 10 10 10
""") == "12"

# two elements only
assert run("""1
2
1 100
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | n(n−1) | trường hợp cạnh thống nhất | 
| lặp lại tối thiểu/tối đa | 2·cntmin·cntmax | xử lý trùng lặp | 
| mảng kích thước 2 | 2 | cấu trúc tối thiểu | 

## Vỏ cạnh 

Đối với đầu vào có tất cả các giá trị giống hệt nhau, chẳng hạn như`4 5 5 5 5`, thuật toán đặt mn và mx có cùng giá trị và tính trực tiếp n × (n − 1). Vì mọi cặp chỉ số riêng biệt được sắp xếp đều hợp lệ, nên giá trị này khớp với số lượng dự kiến ​​mà không cần dựa vào logic tần số nếu không sẽ bị thu gọn. 

Đối với trường hợp như`1 1 10 10 10`, quá trình quét tạo ra mn = 1 với số đếm 2 và mx = 10 với số đếm 3. Kết quả trở thành 2 × 2 × 3 = 12. Mỗi phần tử trong số hai phần tử tối thiểu kết hợp với mỗi phần tử trong số ba phần tử tối đa theo cả hai hướng và cơ chế đếm nắm bắt chính xác sự đối xứng này.
