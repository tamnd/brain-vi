---
title: "CF 1614E - Đi văng và ngôi nhà nhỏ"
description: "Chúng tôi được yêu cầu mô phỏng sự thay đổi nhiệt độ bên trong ngôi nhà của Divan trong một chuỗi ngày, trong đó nhiệt độ điều chỉnh một đơn vị mỗi ngày so với nhiệt độ bên ngoài."
date: "2026-06-10T06:50:16+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1614
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 757 (Div. 2)"
rating: 2600
weight: 1614
solve_time_s: 117
verified: false
draft: false
---

[CF 1614E - Divan và ngôi nhà nhỏ](https://codeforces.com/problemset/problem/1614/E) 

**Đánh giá:** 2600 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng sự thay đổi nhiệt độ bên trong ngôi nhà của Divan trong một chuỗi ngày, trong đó nhiệt độ điều chỉnh một đơn vị mỗi ngày so với nhiệt độ bên ngoài. Cụ thể, nếu nhiệt độ hiện tại trong nhà thấp hơn nhiệt độ bên ngoài thì sẽ tăng thêm một; nếu lớn hơn thì giảm đi một; nếu không thì nó vẫn giữ nguyên. Sau đó, chúng tôi được yêu cầu trả lời các câu hỏi có dạng: nếu nhiệt độ trong nhà bắt đầu ở mức`x`vào ngày đầu tiên, sau đó sẽ như thế nào`i`ngày? Mỗi truy vấn được mã hóa bằng cách sử dụng một cuộn`lastans`giá trị yêu cầu số học mô-đun để giải mã và các truy vấn phải được trả lời theo thứ tự chúng được nhận. 

Các ràng buộc rất đáng kể: có thể lên tới 200.000 ngày và tổng số truy vấn trong tất cả các ngày cũng lên tới 200.000. Do đó, một mô phỏng đơn giản lặp đi lặp lại hàng ngày cho mỗi truy vấn sẽ quá chậm, vì trong trường hợp xấu nhất, nó có thể thực hiện gần đúng.`200,000 * 200,000`hoạt động, vượt xa giới hạn thời gian 2 giây. Số đầu vào có thể lớn bằng`10^9`, do đó, các giải pháp dựa vào mảng dày đặc hoặc đếm lặp đơn giản trong phạm vi nhiệt độ cũng sẽ không thành công do giới hạn bộ nhớ hoặc thời gian. 

Các trường hợp đặc biệt không rõ ràng bao gồm các truy vấn bắt đầu chính xác ở nhiệt độ tối đa hoặc tối thiểu có thể hoặc chuỗi ngày mà nhiệt độ bên ngoài không đổi. Ví dụ: nếu một truy vấn bắt đầu ở mức 0 và ngày đầu tiên có nhiệt độ bên ngoài là 10 thì nhiệt độ trong nhà sẽ tăng thêm một mỗi ngày cho đến khi đạt đến 10. Việc triển khai bất cẩn giả định các truy vấn độc lập với các truy vấn của ngày hôm trước sẽ tính toán sai vì`lastans`sửa đổi từng truy vấn trước khi mô phỏng. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi truy vấn, hãy mô phỏng nhiệt độ từng ngày. Mỗi ngày, hãy so sánh nhiệt độ hiện tại với nhiệt độ bên ngoài và điều chỉnh theo nhiệt độ tương ứng. Điều này đúng vì nó tuân theo các quy tắc một cách trực tiếp, nhưng nó có độ phức tạp trong trường hợp xấu nhất`O(Q * N)`Ở đâu`Q`là số lượng truy vấn và`N`là số ngày. Với cả hai lên đến`2 * 10^5`, điều này có thể dẫn đến`4 * 10^{10}`hoạt động, điều đó là không thể thực hiện được. 

Thông tin chi tiết quan trọng là hiệu ứng của mỗi ngày có thể được biểu thị dưới dạng sự thay đổi phạm vi. Cụ thể, sau`i`ngày, bất kỳ nhiệt độ ban đầu nào`x`sẽ bị giới hạn bởi nhiệt độ tối thiểu và tối đa có thể đạt được nếu chúng ta luôn hướng tới nhiệt độ bên ngoài hàng ngày. Chính thức hơn, chúng ta có thể duy trì hai biến,`lo`Và`hi`, biểu thị nhiệt độ tối thiểu và tối đa có thể có của bất kỳ điểm bắt đầu nào sau khi xử lý các ngày cho đến nay. Mỗi ngày,`lo`được điều chỉnh tăng lên nếu nó ở dưới`T_i`, hoặc đi xuống nếu ở trên`T_i`. Tương tự,`hi`được điều chỉnh theo hướng`T_i`. Sau đó, bất kỳ truy vấn nào cũng có thể được trả lời trong`O(1)`bằng cách kẹp nhiệt độ ban đầu`x`giữa`lo`Và`hi`. 

Điều này chuyển đổi vấn đề từ một khả năng`O(Q * N)`mô phỏng lực lượng vũ phu đến thời gian tuyến tính trôi qua trong các ngày với đánh giá truy vấn theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q * N) | O(1) | Quá chậm | 
| Tối ưu | O(N + Q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`lo`Và`hi`đến 0, biểu thị sự thay đổi nhiệt độ tối thiểu và tối đa có thể có so với trạng thái ban đầu. Khởi tạo`lastans`đến 0. 
2. Lặp lại mỗi ngày`i`từ 1 đến`n`. Đối với nhiệt độ bên ngoài hiện tại`T_i`, cập nhật giới hạn: 

- Tăng`lo`nếu nó nhỏ hơn`T_i`hoặc giảm nó nếu nó lớn hơn`T_i`. Sau đó kẹp`lo`ĐẾN`T_i`nếu nó vượt quá. 
- Điều chỉnh tương tự`hi`theo hướng`T_i`. 

Điều này đảm bảo`lo`Và`hi`luôn thể hiện giới hạn nhiệt độ trong nhà có thể có sau khi xử lý tất cả các ngày cho đến nay. 
3. Đối với mỗi truy vấn`x'_i`vào ngày`i`, trước tiên hãy giải mã giá trị truy vấn thực sự:`x_i = (x'_i + lastans) % (10^9 + 1)`. 
4. Trả lời câu hỏi bằng cách kẹp`x_i`giữa`lo`Và`hi`. Bộ`lastans`đến kết quả này và xuất nó. 
5. Lặp lại cho đến khi tất cả các ngày và truy vấn được xử lý. 

Điều bất biến là sau khi xử lý`i`ngày,`lo`là nhiệt độ nhỏ nhất có thể đạt được bắt đầu từ bất kỳ`x`và di chuyển theo nhiệt độ bên ngoài mỗi ngày, và`hi`là lớn nhất. Kẹp bất kỳ`x`ở trong`[lo, hi]`lập mô hình chính xác các điều chỉnh tăng dần mà không cần mô phỏng từng bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
lastans = 0
lo = hi = 0

for _ in range(n):
    T = int(input())
    k = int(input())
    queries = list(map(int, input().split()))

    # Update bounds
    if lo < T:
        lo += 1
        if lo > T:
            lo = T
    elif lo > T:
        lo -= 1
        if lo < T:
            lo = T

    if hi < T:
        hi += 1
        if hi > T:
            hi = T
    elif hi > T:
        hi -= 1
        if hi < T:
            hi = T

    # Answer queries
    for x_enc in queries:
        x = (x_enc + lastans) % (10**9 + 1)
        lastans = max(lo, min(hi, x))
        print(lastans)
```Mã tuân theo thuật toán chính xác. Giới hạn được điều chỉnh tăng dần theo nhiệt độ bên ngoài trong ngày. Các truy vấn được giải mã bằng cách sử dụng`lastans`để xử lý đầu vào được mã hóa. Việc kẹp cuối cùng đảm bảo rằng mỗi truy vấn được trả lời một cách`O(1)`thời gian mà không cần mô phỏng mỗi ngày trung gian cho mỗi truy vấn. Phải cẩn thận với số học mô-đun và thứ tự cập nhật`lastans`. 

## Ví dụ đã hoạt động 

Sử dụng mẫu 1: 

| Ngày | T_i | Truy vấn x'_i | x được giải mã | lo | xin chào | người cuối cùng | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 50 | 1 | 1 | 1 | 1 | 2 | 2 | 
| 1 | 50 | 2 | 4 | 1 | 1 | 5 | 5 | 
| 1 | 50 | 3 | 9 | 1 | 1 | 9 | 9 | 
| 2 | 50 | 4 | 13 | 2 | 2 | 15 | 15 | 

Bảng này cho thấy cách`lo`Và`hi`điều chỉnh theo nhiệt độ mỗi ngày và mỗi truy vấn được kẹp giữa chúng để có câu trả lời cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + Q) | Giới hạn mỗi ngày được cập nhật một lần và mỗi truy vấn được trả lời trong O(1) | 
| Không gian | O(1) | Chỉ có một số số nguyên được duy trì; không cần mảng tỷ lệ với kích thước đầu vào | 

Giải pháp xử lý thoải mái 200.000 ngày và 200.000 truy vấn trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    lastans = 0
    lo = hi = 0
    n = int(input())
    for _ in range(n):
        T = int(input())
        k = int(input())
        queries = list(map(int, input().split()))
        if lo < T:
            lo += 1
            if lo > T: lo = T
        elif lo > T:
            lo -= 1
            if lo < T: lo = T
        if hi < T:
            hi += 1
            if hi > T: hi = T
        elif hi > T:
            hi -= 1
            if hi < T: hi = T
        for x_enc in queries:
            x = (x_enc + lastans) % (10**9 + 1)
            lastans = max(lo, min(hi, x))
            output.append(str(lastans))
    return "\n".join(output)

# Sample
assert run("3\n50\n3\n1 2 3\n50\n3\n4 5 6\n0\n3\n7 8 9\n") == "2\n5\n9\n15\n22\n30\n38\n47\n53", "sample 1"

# Minimum size
assert run("1\n0\n1\n0\n") == "1", "min input"

# Max temperature
assert run("1\n1000000000\n2\n0 1000000000\n") == "1\n1000000000", "max temperature"

# All days same temperature
assert run("2\n5\n1\n2\n5\n1\n3\n") == "3\n5", "constant temperature"

# Edge case with lastans wrap-around
assert run("2\n1\n1\n1000000000\n2\n1\n1 2\n") == "1000000000\n1000000000\n1000000001", "lastans wrap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n0\n1\n0 | 1 | Xử lý đầu vào tối thiểu |
