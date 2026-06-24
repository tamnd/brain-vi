---
title: "CF 105216F - Giải Công bằng"
description: "Chúng ta được cấp một hàng giá trị giải thưởng, mỗi giải có một giá trị nguyên dương. John có giới hạn điểm $p$ và anh ấy chỉ được phép chọn các giải thưởng có giá trị không vượt quá $p$. Trong số tất cả các giải thưởng hợp lệ, anh ấy muốn giải thưởng có giá trị lớn nhất."
date: "2026-06-24T17:05:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105216
codeforces_index: "F"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 105216
solve_time_s: 74
verified: false
draft: false
---

[CF 105216F - Giải thưởng công bằng](https://codeforces.com/problemset/problem/105216/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng giá trị giải thưởng, mỗi giải có một giá trị nguyên dương. John có giới hạn điểm$p$, và anh ta chỉ được phép chọn giải thưởng có giá trị không vượt quá$p$. Trong số tất cả các giải thưởng hợp lệ, anh ấy muốn giải thưởng có giá trị lớn nhất. 

Nói một cách đơn giản hơn, chúng tôi quét qua danh sách các số nguyên và chỉ được phép xem xét những số nguyên tối đa ở một ngưỡng nhất định. Nhiệm vụ là tìm số lớn nhất trong số các số nguyên đủ điều kiện đó. 

Các ràng buộc rất nhỏ: cả số lượng giải thưởng$n$và tất cả các giá trị, bao gồm cả$p$, nhiều nhất là 1000. Điều này ngay lập tức cho chúng ta biết rằng ngay cả việc quét tuyến tính đơn giản trên mảng cũng đã đủ nhanh. MỘT$O(n)$hoặc thậm chí$O(n^2)$ approach would still run comfortably within limits, but anything beyond that is unnecessary complexity.

The structure of the input is also straightforward: one line gives the size and limit, and the second line gives the list of prize values.

A few edge cases matter even in such a simple problem. First, all values might be equal to $p$, meaning every item is valid and the answer is simply that value. Second, the maximum valid value might appear at the end of the list, so any early stopping strategy that assumes sorted order would fail if applied blindly. Third, values smaller than $p$may appear everywhere, and we must ensure we do not accidentally pick the first valid element rather than the maximum valid one.

 ## Phương pháp tiếp cận 

The brute-force idea is almost identical to the final solution. We scan through every prize and keep track of the best value we are allowed to take. For each element, if it is less than or equal to$p$, we compare it with our current best answer and update if it is larger.

This works because every valid candidate must be inspected at least once, and there is no ordering guarantee that lets us skip elements. The operation count is exactly $n$so sánh với ngưỡng và lên đến$n$cập nhật của một biến tối đa, vì vậy tổng thể$O(n)$. 

Không có sự tối ưu hóa có ý nghĩa nào ngoài điều này. Việc sắp xếp sẽ tốn kém$O(n \log n)$mà không cải thiện logic. Các cấu trúc dữ liệu như đống hoặc cây phân đoạn là những chi phí không cần thiết cho một truy vấn trên một mảng tĩnh. Quan sát quan trọng là vấn đề hoàn toàn là một mức tối đa bị ràng buộc đối với một danh sách tĩnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu |$O(n)$|$O(1)$| Đã chấp nhận | 
| Sắp xếp rồi lọc |$O(n \log n)$|$O(1)$hoặc$O(n)$| Được chấp nhận nhưng không cần thiết | 
| Quét tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng một lần trong khi vẫn duy trì một biến duy nhất lưu trữ giải thưởng hợp lệ nhất từng thấy cho đến nay. 

1. Khởi tạo một biến`best`với giá trị được đảm bảo nhỏ hơn bất kỳ câu trả lời hợp lệ nào, chẳng hạn như 0. Điều này hiệu quả vì tất cả các giá trị giải thưởng ít nhất là 1, vì vậy 0 không thể vô tình là câu trả lời. 
2. Lặp lại qua từng giá trị giải thưởng$v_i$trong mảng từ trái sang phải. 
3. Với mỗi giá trị, kiểm tra xem nó có thỏa mãn không$v_i \le p$. Điều kiện này đảm bảo chúng tôi chỉ xem xét các giải thưởng mà John được phép nhận. 
4. Nếu giá trị hợp lệ, hãy so sánh nó với`best`. Nếu như$v_i > best$, cập nhật`best`ĐẾN$v_i$. Bước này đảm bảo chúng tôi luôn giữ lại giá trị hợp lệ tối đa được thấy cho đến nay. 
5. Sau khi xử lý tất cả các giá trị, xuất ra`best`như câu trả lời cuối cùng. 

Lý do quy trình này hoạt động là vì mọi ứng cử viên khả thi đều được kiểm tra chính xác một lần và thuật toán duy trì tính bất biến sau khi xử lý ứng viên đầu tiên.$k$các yếu tố,`best`lưu trữ giá trị tối đa trong số tất cả các phần tử hợp lệ trong tiền tố đó. Vì câu trả lời cuối cùng chỉ phụ thuộc vào mức tối đa toàn cục trong số các giá trị hợp lệ, nên việc mở rộng bất biến này sang toàn bộ mảng sẽ đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, p = map(int, input().split())
    arr = list(map(int, input().split()))
    
    best = 0
    for v in arr:
        if v <= p and v > best:
            best = v
    
    print(best)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc kích thước của danh sách và ngưỡng. Sau đó nó phân tích tất cả các giá trị giải thưởng thành một mảng. 

Biến`best`được khởi tạo thành 0 vì tất cả các giá trị đều dương, vì vậy đây là phần tử nhận dạng an toàn để theo dõi tối đa trong điều kiện ràng buộc$v_i \le p$. 

Vòng lặp thực hiện cả hai bước kiểm tra trong một lần: đầu tiên xác minh tính khả thi đối với$p$, sau đó cập nhật tối đa. Thứ tự này ngăn chặn các cập nhật không chính xác từ các giá trị không hợp lệ. 

Cuối cùng, mức tối đa được tính toán sẽ được in trực tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 10
4 2 4 3 9
```Chúng tôi theo dõi`best`: 

| Bước | Giá trị | Hợp lệ (≤ p) | tốt nhất trước | tốt nhất sau |
 | --- | --- | --- | --- | --- | 
| 1 | 4 | vâng | 0 | 4 | 
| 2 | 2 | vâng | 4 | 4 | 
| 3 | 4 | vâng | 4 | 4 | 
| 4 | 3 | vâng | 4 | 4 | 
| 5 | 9 | vâng | 4 | 9 | 

Câu trả lời cuối cùng là 9. 

Điều này chứng tỏ rằng thuật toán không dừng sớm và tìm chính xác mức tối đa muộn hơn. 

### Mẫu 2 

đầu vào:```
1 10
10
```| Bước | Giá trị | Hợp lệ (≤ p) | tốt nhất trước | tốt nhất sau | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | vâng | 0 | 10 | 

Câu trả lời cuối cùng là 10. 

Điều này xác nhận việc xử lý đúng kích thước mảng tối thiểu khi chỉ tồn tại một ứng cử viên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| mỗi giải thưởng được xử lý đúng một lần | 
| Không gian |$O(1)$| chỉ một biến duy nhất được sử dụng bất kể kích thước đầu vào | 

Kích thước đầu vào tối đa là 1000 phần tử, do đó, quá trình quét tuyến tính nằm trong giới hạn tầm thường. Ngay cả với nhiều trường hợp thử nghiệm (không bao gồm vấn đề này), chiến lược tuyến tính giống nhau sẽ vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip() if False else None
```Vì chúng tôi không thể dựa vào việc nhập lại trong môi trường này nên dưới đây là các thử nghiệm kiểu khẳng định mang tính khái niệm nhất quán với logic giải pháp:```python
import sys, io

def run_solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    def solve():
        n, p = map(int, input().split())
        arr = list(map(int, input().split()))
        best = 0
        for v in arr:
            if v <= p and v > best:
                best = v
        output.append(str(best))
    
    solve()
    return output[0]

# provided samples
assert run_solution("5 10\n4 2 4 3 9\n") == "9"
assert run_solution("1 10\n10\n") == "10"

# custom cases
assert run_solution("3 5\n1 2 3\n") == "3"
assert run_solution("4 2\n5 6 1 2\n") == "2"
assert run_solution("5 100\n10 20 30 40 50\n") == "50"
assert run_solution("6 3\n4 4 4 4 4 4\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giá trị nhỏ hỗn hợp | tối đa ≤ p | lọc tiêu chuẩn | 
| tất cả đều không hợp lệ ngoại trừ một | lựa chọn đúng | lọc ranh giới | 
| tất cả đều có giá trị tăng | lựa chọn tối đa toàn cầu | tính chính xác của việc theo dõi tối đa | 
| tất cả đều không hợp lệ | 0 | xử lý không có ứng viên hợp lệ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các phần tử đều lớn hơn$p$. Ví dụ, nếu$p = 2$và mảng là$[5, 6, 7]$, không có giá trị nào đủ điều kiện. Thuật toán khởi tạo`best`thành 0 và không bao giờ cập nhật nó, tạo ra 0. Vì sự cố đảm bảo tồn tại ít nhất một lựa chọn hợp lệ nên trường hợp này sẽ không xuất hiện trong đầu vào chính thức, nhưng quá trình triển khai vẫn xử lý nó một cách an toàn.

 Một trường hợp cạnh khác là khi giá trị hợp lệ tối đa xuất hiện ở vị trí đầu tiên. Đối với đầu vào$p = 10$, mảng$[9, 1, 8]$, thuật toán đặt đúng`best`xuống 9 ngay lập tức và không bị ảnh hưởng bởi các giá trị nhỏ hơn sau này. 

Trường hợp cạnh cuối cùng là khi giá trị hợp lệ tối đa xuất hiện ở cuối. Đối với đầu vào$p = 10$, mảng$[1, 2, 10]$, thuật toán cập nhật dần dần và nắm bắt chính xác phần tử cuối cùng là phần tử tốt nhất. Điều này xác nhận rằng không cần giả định thứ tự nào để đảm bảo tính chính xác.
