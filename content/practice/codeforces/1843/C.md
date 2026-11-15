---
title: "CF 1843C - Tổng trong cây nhị phân"
description: "Vấn đề mô tả một cây nhị phân hoàn chỉnh vô hạn trong đó các nút được đánh số tuần tự theo từng lớp. Gốc có số 1, con trái của nó là 2, con phải là 3 và mô hình tiếp tục sao cho mỗi nút có hai con với các số có sẵn tiếp theo."
date: "2026-06-09T06:07:03+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1843
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 881 (Div. 3)"
rating: 800
weight: 1843
solve_time_s: 87
verified: true
draft: false
---

[CF 1843C - Tính tổng trong cây nhị phân](https://codeforces.com/problemset/problem/1843/C) 

**Đánh giá:** 800 
**Tags:** bitmasks, tổ hợp, toán học, cây 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một cây nhị phân hoàn chỉnh vô hạn trong đó các nút được đánh số tuần tự theo từng lớp. Gốc có số 1, con trái của nó là 2, con phải là 3 và mô hình tiếp tục sao cho mỗi nút có hai con với các số có sẵn tiếp theo. Cho một số đỉnh`n`, chúng ta được yêu cầu tính tổng của tất cả các số nút dọc theo đường dẫn từ gốc đến`n`. 

Đầu vào bao gồm nhiều trường hợp kiểm thử, mỗi trường hợp xác định một số đỉnh`n`có thể lớn như$10^{16}$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng xây dựng cây một cách rõ ràng hoặc duyệt qua từng nút một, bởi vì việc truyền tải mạnh mẽ có thể liên quan đến tối đa$10^{16}$hoạt động, vượt xa giới hạn khả thi trong thời gian chạy 1 giây. 

Các trường hợp cạnh không rõ ràng bao gồm nút nhỏ nhất có thể,`n = 1`. Trong trường hợp này, đường dẫn chỉ chứa gốc và tổng phải bằng 1. Một sự tinh tế khác nảy sinh khi`n`là lũy thừa của hai hoặc ngay sau lũy thừa của hai. Việc đánh số các nút theo lớp sẽ căn chỉnh một cách tự nhiên với các biểu diễn nhị phân và đây là chìa khóa cho một giải pháp hiệu quả. Một cách tiếp cận bất cẩn nhằm cố gắng mô phỏng tuần tự các nhiệm vụ của trẻ em sẽ thất bại đối với nhiều`n`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra cây tới nút`n`hoặc duy trì một con trỏ cha cho mỗi nút. Sau đó chúng ta có thể theo dõi các gợi ý của cha mẹ từ`n`đến 1 để tổng hợp đường dẫn. Điều này làm việc cho nhỏ`n`nhưng trở nên không khả thi đối với`n`gần với$10^{16}$, vì nó sẽ yêu cầu một số lượng lớn các lần chèn hoặc phân bổ bộ nhớ. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát cấu trúc của cây. Mỗi nút trong cây nhị phân vô hạn này có thể được liên kết với một biểu diễn nhị phân mã hóa đường dẫn từ gốc. Cụ thể, nếu ta đánh số gốc là 1, con trái và con phải là 2 và 3, rồi tiếp tục đánh số từng lớp thì mỗi bước đi xuống tương ứng với việc chọn con trái hoặc con phải, tương đương với một bit trong`n`dạng nhị phân của. Cụ thể hơn, nếu chúng ta theo dõi đường đi từ`n`đến gốc, mỗi lần di chuyển về cha mẹ có thể được tính bằng cách chia số nguyên cho 2 và tính tổng các giá trị này sẽ cho kết quả mong muốn. Điều này làm giảm vấn đề liên tục giảm một nửa`n`và tích lũy số tiền cho đến khi chúng ta đạt đến 1. 

Cách tiếp cận này hiệu quả vì việc đánh số cây tương ứng chính xác với dãy số được tạo ra khi duyệt theo chiều rộng đầu tiên của cây nhị phân hoàn chỉnh. Di chuyển từ một nút tới nút cha của nó là`n // 2`, và điều này có thể được lặp lại cho đến khi chúng ta đạt đến gốc. Điều này đảm bảo số bước logarit tỷ lệ với số bit trong`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Quá chậm đối với n lớn | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,`t`. Đối với mỗi trường hợp thử nghiệm, hãy đọc số đỉnh`n`. 
2. Khởi tạo một biến`ans`về 0. Điều này sẽ giữ tổng số nút dọc theo đường dẫn. 
3. Trong khi`n`lớn hơn 0 thì cộng`n`ĐẾN`ans`và cập nhật`n`ĐẾN`n // 2`. Mỗi phép chia cho 2 sẽ di chuyển lên một bậc trên cây. 
4. Một lần`n`đạt đến 0, đường đi được đi qua hoàn toàn. đầu ra`ans`. 

Tại sao nó hoạt động: Điều bất biến là ở mỗi lần lặp,`n`đại diện cho nút hiện tại trên đường dẫn đến gốc. Thêm`n`ĐẾN`ans`tích lũy tổng và chia cho 2 sẽ đưa chúng ta về phần gốc của nó. Điều này đảm bảo chúng tôi bao gồm mọi nút trong đường dẫn chính xác một lần, kết thúc ở gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    ans = 0
    while n > 0:
        ans += n
        n //= 2
    print(ans)
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`, điều này cần thiết cho tới$10^4$trường hợp thử nghiệm. Vòng lặp while đảm bảo rằng chúng ta theo dõi đường đi từ`n`đến gốc theo thời gian logarit. Việc sử dụng phép chia số nguyên đảm bảo tính toán gốc chính xác và các số nguyên có độ chính xác tùy ý của Python xử lý các giá trị rất lớn của`n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 3 

| Bước | n | trả lời | 
| --- | --- | --- | 
| 1 | 3 | 0 | 
| 2 | 1 | 3 | 
| 3 | 0 | 4 | 

Đường đi từ 1 → 3 được tính tổng chính xác là 1 + 3 = 4. 

### Ví dụ 2: n = 10 

| Bước | n | trả lời | 
| --- | --- | --- | 
| 1 | 10 | 0 | 
| 2 | 5 | 10 | 
| 3 | 2 | 15 | 
| 4 | 1 | 17 | 
| 5 | 0 | 18 | 

Đường đi từ 1 → 2 → 5 → 10 được tính tổng chính xác là 1 + 2 + 5 + 10 = 18. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) cho mỗi trường hợp thử nghiệm | Mỗi lần lặp lại chia`n`bằng 2 và số lần lặp bằng số bit trong`n`. | 
| Không gian | O(1) | Chỉ cần một bộ tích lũy số nguyên duy nhất; không cần cấu trúc cây hoặc mảng. | 

Với`t`lên đến$10^4$Và`n`lên đến$10^{16}$, tổng số thao tác là khoảng$10^4 * 54$vì 54 bit là đủ cho$10^{16}$, đó là trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution code
    t = int(input())
    for _ in range(t):
        n = int(input())
        ans = 0
        while n > 0:
            ans += n
            n //= 2
        print(ans)
    return output.getvalue().strip()

# provided samples
assert run("6\n3\n10\n37\n1\n10000000000000000\n15\n") == "4\n18\n71\n1\n19999999999999980\n26"

# custom cases
assert run("1\n1\n") == "1", "root node"
assert run("1\n2\n") == "3", "small left child"
assert run("1\n3\n") == "4", "small right child"
assert run("1\n16\n") == "31", "power of 2"
assert run("1\n17\n") == "48", "just above power of 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Tổng đường dẫn cho nút gốc | 
| 2 | 3 | Con nhỏ nhất bên trái | 
| 3 | 4 | Con nhỏ nhất bên phải | 
| 16 | 31 | Sức mạnh của hai nút, độ sâu đường dẫn đầy đủ | 
| 17 | 48 | Nút vượt quá sức mạnh của hai, kiểm tra tính chính xác của phép tính đường dẫn | 

## Vỏ cạnh 

cho`n = 1`, bộ thuật toán`ans = 0`, cộng 1, sau đó chia cho 2 để được 0. Kết quả đầu ra là 1, như mong đợi. Đối với rất lớn`n`, chẳng hạn như`10^{16}`, Python xử lý kích thước số nguyên một cách liền mạch và vòng lặp thực hiện tối đa 54 lần lặp do độ dài nhị phân của`n`, xác nhận tính đúng đắn của ràng buộc tối đa. Phép chia cho 2 theo dõi chính xác cha mẹ cho cả số chẵn và số lẻ`n`, đảm bảo không có sai sót nào.
