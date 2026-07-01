---
title: "CF 104207A - Chó và lồng"
description: "Chúng ta được cung cấp một hệ thống có cùng số lượng chó và số lồng, cả hai đều được dán nhãn từ 0 đến N − 1. Mỗi con chó độc lập chọn ngẫu nhiên một cái lồng thống nhất một cách độc lập và mỗi lồng có thể chứa nhiều nhất một con chó, nghĩa là cấu hình cuối cùng là một hoán vị ngẫu nhiên của những con chó trên các lồng."
date: "2026-07-01T23:55:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104207
codeforces_index: "A"
codeforces_contest_name: "2017 China Collegiate Programming Contest Final (CCPC-Final 2017)"
rating: 0
weight: 104207
solve_time_s: 45
verified: true
draft: false
---

[CF 104207A - Chó và lồng](https://codeforces.com/problemset/problem/104207/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống có cùng số lượng chó và số lồng, cả hai đều được dán nhãn từ 0 đến N − 1. Mỗi con chó độc lập chọn ngẫu nhiên một cái lồng thống nhất một cách độc lập và mỗi lồng có thể chứa nhiều nhất một con chó, nghĩa là cấu hình cuối cùng là một hoán vị ngẫu nhiên của những con chó trên các lồng. 

Chúng tôi không được hỏi về sự phân bổ đầy đủ các kết quả, chỉ hỏi về số lượng dự kiến ​​những con chó không bị nhốt vào chuồng có cùng chỉ số với chúng. Nói cách khác, với mỗi con chó i, chúng ta quan tâm liệu nó có tránh bị nhốt vào lồng i hay không và chúng ta muốn biết số lượng chó như vậy dự kiến. 

Dữ liệu đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cho một giá trị N và với mỗi trường hợp, chúng tôi đưa ra số lượng chó "không khớp" dự kiến. 

Các ràng buộc cho phép tối đa 10^5 trường hợp thử nghiệm và N tối đa 10^5, do đó, mọi mô phỏng cho mỗi trường hợp kiểm tra hoặc lý luận dựa trên giai thừa đều không thể thực hiện được. Một giải pháp phải giảm mỗi truy vấn xuống còn O(1) thời gian sau khi xử lý trước tối thiểu hoặc đánh giá công thức trực tiếp. 

Trường hợp cạnh tinh tế là N = 1. Với một con chó và một cái lồng, con chó luôn ở trong lồng chính xác, vì vậy câu trả lời phải chính xác là 0. Bất kỳ đạo hàm nào giả định không chính xác tính đối xứng trên nhiều phần tử nhưng quên trường hợp cơ bản này vẫn có thể tạo ra các công thức trông đúng cho N lớn hơn nhưng không thành công ở đây. 

Một cạm bẫy khái niệm khác là coi quá trình này là những lựa chọn độc lập cho mỗi con chó. Điều đó cho thấy mỗi con chó có xác suất sai là 1 − 1/N, nhưng không đảm bảo tính duy nhất của các lồng. May mắn thay, tính tuyến tính kỳ vọng cho phép sự đơn giản hóa này vẫn có hiệu lực mặc dù phép gán toàn cục là một hoán vị chứ không phải là các phép rút độc lập. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu bằng cách tưởng tượng tất cả các nhiệm vụ hợp lệ có thể có của chó vào chuồng. Mỗi phép gán là một hoán vị có kích thước N và với mỗi hoán vị, chúng ta đếm có bao nhiêu chỉ số tôi thỏa mãn hoán vị[i] ≠ i. Sau đó chúng tôi tính trung bình giá trị này trên tất cả N! hoán vị. 

Điều này đúng nhưng hoàn toàn không khả thi. Đếm N! không thể cấu hình ngay cả khi N = 10, vì 10! đã là 3,6 triệu, và với N = 20, nó trở nên lớn về mặt thiên văn. 

Quan sát quan trọng là chúng ta thực sự không cần cấu trúc chung của hoán vị. Chúng ta chỉ cần sự đóng góp dự kiến ​​của từng chỉ số riêng lẻ. Xác định biến chỉ báo Xi bằng 1 nếu chó i không ở trong lồng i và 0 nếu ngược lại. Câu trả lời là giá trị kỳ vọng của tổng tất cả Xi. 

Tính tuyến tính của kỳ vọng cho phép chúng ta tính tổng kỳ vọng của từng chỉ số riêng lẻ mà không cần xem xét sự phụ thuộc giữa các vị trí. Đối với một con chó i cố định, tính đối xứng ngụ ý rằng trong một hoán vị ngẫu nhiên đều, con chó i có khả năng xuất hiện như nhau trong bất kỳ lồng nào, do đó xác suất để nó rơi vào lồng của chính nó là chính xác là 1/N. Do đó, xác suất nó không khớp là 1 − 1/N = (N − 1)/N. 

Tổng tất cả N con chó sẽ cho ra N · (N − 1)/N = N − 1. Điều này thu gọn toàn bộ bài toán thành một công thức thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị | O(N!) | O(1) | Quá chậm | 
| Giá trị kỳ vọng trên mỗi vị trí | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Đọc N cho test case hiện tại. 
2. Xử lý trường hợp đặc biệt khi N = 1 và xuất ra ngay 0 vì con chó duy nhất phải đúng. 
3. Với N ≥ 2, hãy tính số lượng không khớp dự kiến bằng cách sử dụng công thức dẫn xuất N − 1. 
4. In kết quả ở định dạng dấu phẩy động với độ chính xác vừa đủ. 

Bước lý luận đằng sau việc tính toán là mỗi vị trí đóng góp độc lập trong kỳ vọng, mặc dù hoán vị kết hợp chúng trên toàn cầu. Việc phân rã chỉ báo đảm bảo rằng chúng ta không bao giờ dựa vào tính độc lập thực tế. 

### Tại sao nó hoạt động

Gọi Xi là dấu hiệu cho biết con chó i có ở trong lồng i hay không. Trong một hoán vị ngẫu nhiên, mỗi con chó có xác suất 1/N được cố định vào đúng vị trí của nó, do đó E[Xi] = (N − 1)/N. Tổng kỳ vọng là E[ΣXi] = ΣE[Xi] = N · (N − 1)/N. Điều này đảm bảo tính chính xác mà không cần liệt kê hoặc mô phỏng các hoán vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for i in range(1, t + 1):
        n = int(input())
        if n == 1:
            out.append(f"Case #{i}: 0.0000000000")
        else:
            ans = n - 1
            out.append(f"Case #{i}: {ans:.10f}")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai áp dụng trực tiếp biểu thức dạng đóng dẫn xuất. Nhánh duy nhất dành cho N = 1 để duy trì tính chính xác theo yêu cầu định dạng và tránh các thao tác dấu phẩy động không cần thiết trong các trường hợp tầm thường. Định dạng có 10 chữ số thập phân phù hợp với dung sai đầu ra điển hình của Codeforce cho các vấn đề về giá trị mong đợi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 1
```Chúng tôi đánh giá: 

| Bước | N | Đóng góp dự kiến ​​cho mỗi con chó | Tổng cộng | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 0/1 = 0 | 0 | 

Con chó duy nhất luôn ở trong chuồng riêng nên số lượng không khớp là 0. Điều này xác nhận cách xử lý trường hợp cơ bản. 

### Ví dụ 2 

đầu vào:```
N = 2
```Chúng tôi tính toán: 

| Bước | N | xác suất không phù hợp của mỗi con chó | Tổng dự kiến ​​| 
| --- | --- | --- | --- | 
| tính toán | 2 | 1/2 | 2 × 1/2 = 1 | 

Cả hai hoán vị đều có khả năng xảy ra như nhau: danh tính cho 0 điểm không khớp, hoán đổi cho 2 điểm không khớp, tính trung bình là 1. Điều này khớp với công thức N − 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | mỗi trường hợp kiểm thử được xử lý bằng một phép tính số học duy nhất | 
| Không gian | O(1) | chỉ một số biến và bộ đệm đầu ra được sử dụng | 

Giải pháp này dễ dàng xử lý 10^5 trường hợp thử nghiệm vì nó tránh được vòng lặp cho từng trường hợp trên N. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    input = sys.stdin.readline
    t = int(input())
    out = []
    for i in range(1, t + 1):
        n = int(input())
        if n == 1:
            out.append(f"Case #{i}: 0.0000000000")
        else:
            ans = n - 1
            out.append(f"Case #{i}: {ans:.10f}")
    return "\n".join(out)

# provided samples
assert run("2\n1\n2\n") == "Case #1: 0.0000000000\nCase #2: 1.0000000000"

# custom cases
assert run("1\n3\n") == "Case #1: 2.0000000000"
assert run("1\n5\n") == "Case #1: 4.0000000000"
assert run("1\n10\n") == "Case #1: 9.0000000000"
assert run("3\n1\n2\n4\n") == "Case #1: 0.0000000000\nCase #2: 1.0000000000\nCase #3: 3.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N=1 | 0 | trường hợp cơ sở đúng đắn | 
| N=3 | 2 | kỳ vọng không hề nhỏ | 
| N=10 | 9 | hành vi mở rộng quy mô | 
| trường hợp hỗn hợp | định dạng đúng | xử lý nhiều bài kiểm tra | 

## Vỏ cạnh 

Với N = 1, thuật toán trả về 0 một cách rõ ràng. Công thức N − 1 cũng sẽ cho 0, nhưng nhánh tránh được bất kỳ sự mơ hồ nào trong logic định dạng. 

Ví dụ đầu vào N = 1: 

| Bước | Giá trị | 
| --- | --- | 
| đọc N | 1 | 
| kiểm tra N == 1 | đúng | 
| đầu ra | 0,0000000000 | 

Điều này xác nhận rằng cấu hình nhỏ nhất có thể hoạt động nhất quán với đối số kỳ vọng. 

Đối với N lớn hơn như N = 2 hoặc N = 3, phép tính giảm trực tiếp xuống N − 1 mà không phụ thuộc vào cấu trúc hoán vị. Thuật toán không cần mô phỏng các bài tập vì kỳ vọng đã loại bỏ tất cả các tương tác giữa các con chó.
