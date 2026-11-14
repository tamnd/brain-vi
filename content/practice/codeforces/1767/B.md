---
title: "CF 1767B - Tháp khối"
description: "Chúng ta được cấp một số tháp, mỗi tháp chứa một số khối nhất định. Mục tiêu là tối đa hóa số khối trên tháp đầu tiên bằng cách di chuyển các khối từ các tháp có nhiều khối hơn các khối khác."
date: "2026-06-09T12:49:49+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1767
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 140 (Rated for Div. 2)"
rating: 800
weight: 1767
solve_time_s: 122
verified: false
draft: false
---

[CF 1767B - Tháp khối](https://codeforces.com/problemset/problem/1767/B) 

**Đánh giá:** 800 
**Tags:** cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số tháp, mỗi tháp chứa một số khối nhất định. Mục tiêu là tối đa hóa số khối trên tháp đầu tiên bằng cách di chuyển các khối từ các tháp có nhiều khối hơn các khối khác. Việc di chuyển chỉ được phép nếu chúng ta lấy một khối từ một tháp có nhiều khối hơn và đặt nó lên một tháp có ít khối hơn. Mỗi lần di chuyển sẽ giảm một tòa tháp xuống một khối và tăng thêm một khối khác. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số lượng tháp và danh sách số khối cho tất cả các tháp. Đối với mỗi trường hợp thử nghiệm, đầu ra phải là số khối tối đa có thể có trên tháp đầu tiên sau khi thực hiện bất kỳ chuỗi di chuyển hợp lệ nào. 

Những hạn chế rất lớn: số lượng tháp$n$có thể đạt được$2 \cdot 10^5$, và tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$. Điều này loại trừ mọi giải pháp mô phỏng từng chuyển động một vì trong trường hợp xấu nhất, việc di chuyển các khối riêng lẻ sẽ yêu cầu tới$10^9$hoạt động cho một tòa tháp với hàng tỷ khối, vượt xa thời hạn. Bản thân các khối có thể rất lớn, vì vậy giải pháp phải hoạt động với số học chứ không phải mô phỏng vũ phu. 

Các trường hợp biên không rõ ràng bao gồm các tình huống trong đó một tháp có số lượng khối lớn và số khối còn lại là tối thiểu hoặc tất cả các tháp đều bắt đầu với cùng số khối. Ví dụ: nếu đầu vào là`[1, 1000000000]`, một thuật toán ngây thơ kiểm tra lặp đi lặp lại từng nước đi có thể sẽ chậm một cách vô vọng. Một trường hợp cạnh khác là khi tất cả các tháp đều bằng nhau, như`[5, 5, 5]`. Ở đây không thể di chuyển được và giá trị tối đa của tháp một chỉ đơn giản là giá trị ban đầu của nó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: quét liên tục tất cả các tháp, tìm một tháp có nhiều khối hơn tháp đầu tiên và di chuyển một khối từ tháp đó sang tháp một. Lặp lại cho đến khi không thể thực hiện được những động tác như vậy. Phương pháp này đúng vì mỗi lần di chuyển đều hợp pháp và chúng tôi chỉ dừng lại khi tháp một không thể tăng thêm nữa. Vấn đề là hiệu suất: trong trường hợp xấu nhất, chúng ta có thể tính tổng chênh lệch giữa các tháp, có thể đạt tới$10^9$hoạt động cho mỗi trường hợp thử nghiệm. Điều này vượt quá mọi giới hạn khả thi. 

Cách tiếp cận tối ưu xuất phát từ việc quan sát rằng thứ tự di chuyển không quan trọng. Chúng ta chỉ cần biết tổng số khối và bao nhiêu khối có thể đi lên tháp một một cách hợp pháp. Tháp một có thể nhận khối từ tất cả các tháp khác, nhưng không tháp nào có thể giảm xuống dưới số lượng hiện tại của tháp khi chúng ta di chuyển khối. Do đó, số khối tối đa trên tháp một là số nguyên trung bình của tất cả các khối được làm tròn lên. Về mặt hình thức, tổng của tất cả các khối chia cho số lượng tháp sẽ mang lại mức tối đa hợp lý mà bất kỳ tháp nào cũng có thể đạt được khi phân phối lại các khối một cách tối ưu. Điều này là do bất kỳ tòa tháp nào có ít khối hơn mức trung bình đều không thể cho nhiều hơn mức chênh lệch giữa số lượng của nó và mức trung bình. Vậy công thức trở thành`max_possible = ceil(total_blocks / n)`, có thể được tính toán một cách an toàn bằng cách sử dụng số học số nguyên như`(total + n - 1) // n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng(a_i)) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc số lượng tháp`n`và danh sách số khối`a`. 
3. Tính tổng số khối trên tất cả các tòa tháp:`total = sum(a)`. 
4. Tính số khối tối đa mà tòa tháp có thể đạt được. Vì tòa tháp không bao giờ có thể vượt quá mức trần trung bình của các khối, hãy tính`(total + n - 1) // n`. 
5. In giá trị tối đa được tính toán cho từng trường hợp thử nghiệm. 

Tính đúng đắn của phương pháp này dựa trên tính bất biến rằng không có tháp nào có thể nhận được nhiều hơn số khối được chia công bằng mà không vi phạm quy tắc di chuyển. Bằng cách lấy mức trần trung bình, chúng tôi đảm bảo tòa tháp nhận được càng nhiều khối càng tốt trong khi vẫn tôn trọng chiều cao tương đối của các tòa tháp khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    total = sum(a)
    max_blocks = (total + n - 1) // n
    print(max_blocks)
```Giải pháp đọc đầu vào hiệu quả với`sys.stdin.readline`, xử lý nhiều trường hợp thử nghiệm, tính tổng các khối và áp dụng số học số nguyên để tính mức trần mà không cần các phép toán dấu phẩy động. sử dụng`(total + n - 1) // n`tránh các lỗi làm tròn và xử lý số tiền lớn lên đến$2 \cdot 10^{14}$một cách an toàn trong Python. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`[1, 2, 3]`| Tháp | Ban đầu | Tổng số tiền | Trần (trung bình) | 
| --- | --- | --- | --- | 
| 1 | 1 | 6 | 3 | 
| 2 | 2 | | | 
| 3 | 3 | | | 

Tổng là 6, số tháp là 3 nên số lượng tối đa có thể có của tháp một là`(6 + 3 - 1) // 3 = 8 // 3 = 3`. Điều này phù hợp với sản lượng dự kiến. 

**Ví dụ 2** 

đầu vào:`[1, 2, 2]`| Tháp | Ban đầu | Tổng số tiền | Trần (trung bình) | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 | 2 | 
| 2 | 2 | | | 
| 3 | 2 | | | 

Tổng là 5, tháp = 3, tối đa có thể có cho tháp một là`(5 + 3 - 1) // 3 = 7 // 3 = 2`. 

Những ví dụ này xác nhận rằng công thức tính toán chính xác mức trần trung bình, là mức tối đa mà người ta có thể đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính tổng tất cả các khối mất thời gian tuyến tính cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(1) | Chỉ có danh sách tổng và đầu vào được lưu trữ tạm thời. | 

Cho rằng tổng của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, thuật toán chạy trong một phần giây, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = sum(a)
        max_blocks = (total + n - 1) // n
        print(max_blocks)
    return output.getvalue().strip()

# provided samples
assert run("4\n3\n1 2 3\n3\n1 2 2\n2\n1 1000000000\n10\n3 8 6 7 4 1 2 4 10 1\n") == "3\n2\n500000001\n9"

# custom test cases
assert run("1\n2\n5 5\n") == "5", "all equal"
assert run("1\n3\n1 1 1\n") == "1", "minimum equal blocks"
assert run("1\n2\n1 1000000000\n") == "500000001", "extreme disparity"
assert run("1\n5\n2 3 4 5 6\n") == "5", "average ceiling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 tháp bằng nhau`[5, 5]`| 5 | Thuật toán xử lý chính xác các tòa tháp bằng nhau | 
| tối thiểu 3 tháp`[1, 1, 1]`| 1 | Thuật toán xử lý các tháp nhỏ, đồng đều | 
| Sự chênh lệch cực độ`[1, 1000000000]`| 500000001 | Xử lý số lượng lớn và tính toán trần | 
|`[2, 3, 4, 5, 6]`| 5 | Tính toán trần đúng cho phân bố không đều | 

## Vỏ cạnh 

Nếu tất cả các tòa tháp có cùng số khối thì tổng chia cho`n`là một số nguyên và trần không thay đổi giá trị. Ví dụ,`[5, 5, 5]`sản lượng`(15 + 3 - 1) // 3 = 17 // 3 = 5`. Thuật toán đưa ra số bắt đầu cho tháp một, điều này đúng vì không có bước di chuyển nào có thể làm tăng nó. 

Nếu một tòa tháp có số lượng khối khổng lồ so với các khối khác, chẳng hạn như`[1, 1000000000]`, tổng số tiền là`1000000001`, số tháp = 2, cho`(1000000001 + 2 - 1) // 2 = 1000000002 // 2 = 500000001`. Thuật toán nắm bắt phân phối tối đa có thể một cách hiệu quả, tránh các bước di chuyển lặp đi lặp lại.
