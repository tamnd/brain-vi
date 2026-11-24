---
title: "CF 903A - Vấn đề sinh viên đói"
description: "Nhiệm vụ hỏi liệu Ivan có thể mua chính xác x miếng gà chỉ bằng cách sử dụng phần nhỏ gồm 3 miếng và phần lớn gồm 7 miếng hay không."
date: "2026-06-12T22:53:09+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 903
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 34 (Rated for Div. 2)"
rating: 900
weight: 903
solve_time_s: 254
verified: true
draft: false
---

[CF 903A - Vấn đề học sinh đói](https://codeforces.com/problemset/problem/903/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 4 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ hỏi liệu Ivan có thể mua chính xác _x_ miếng gà chỉ bằng cách sử dụng phần nhỏ gồm 3 miếng và phần lớn gồm 7 miếng hay không. Mỗi trường hợp thử nghiệm chỉ định một số khối mục tiêu và đầu ra sẽ cho biết liệu con số chính xác đó có thể thu được dưới dạng kết hợp không âm của các phần nhỏ và lớn hay không. 

Đầu vào bị hạn chế sao cho có tối đa 100 trường hợp thử nghiệm và số khối mục tiêu trong mỗi trường hợp thử nghiệm không vượt quá 100. Điều này có nghĩa là chúng ta có thể cung cấp các thuật toán có độ phức tạp theo thứ tự hàng nghìn thao tác cho mỗi trường hợp thử nghiệm. Ví dụ: một vòng lặp lồng nhau đơn giản thử tất cả các kết hợp giữa phần nhỏ và phần lớn sẽ vẫn chạy thoải mái trong thời gian giới hạn. 

Trường hợp cạnh tinh tế phát sinh khi mục tiêu nhỏ hơn kích thước của phần lớn. Ví dụ: nếu Ivan muốn 5 phần, thì lựa chọn duy nhất là một hoặc hai phần nhỏ, nhưng không có tổng bằng 5. Một cách triển khai đơn giản chỉ kiểm tra xem mục tiêu có chia hết cho 3 hay 7 sẽ trả lời sai CÓ cho một số số, vì vậy chúng ta phải xem xét sự kết hợp của các phần thay vì bội số đơn lẻ. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại qua tất cả số phần nhỏ có thể có, tính toán các phần còn lại cần được che bởi các phần lớn và kiểm tra xem phần dư đó có chia hết cho 7 hay không. Về mặt hình thức, đối với mục tiêu _x_, người ta sẽ thử _a = 0_ đến _a_max = x // 3_ và với mỗi _a_ tính toán _b = (x - 3_a) / 7*. Nếu _b_ là số nguyên không âm thì câu trả lời là CÓ. Điều này đúng vì nó kiểm tra toàn diện tất cả các kết hợp hợp lệ. Với _x_ lớn nhất là 100, có tối đa 34 lần lặp cho mỗi trường hợp thử nghiệm và với tối đa 100 trường hợp thử nghiệm, tổng số thao tác là khoảng 3400, có thể chấp nhận được. 

Cách tiếp cận nhanh hơn dựa vào quan sát tham lam: bất kỳ số nào _x >= 12_ luôn có thể được biểu diễn dưới dạng kết hợp của 3 và 7. Các số dưới 12 có thể được tính toán trước. Điều này hiệu quả vì khi chúng ta có đủ khối để kết hợp bội số của 3 với một hoặc nhiều số 7, thì mọi số tiếp theo có thể được hình thành bằng cách thêm 3 số bổ sung. Quan sát này chuyển từng truy vấn thành một kiểm tra đơn giản: _x >= 12_ hoặc _x_ nằm trong một tập hợp nhỏ các số có thể biểu thị dưới 12 được tính toán trước. Điều này biến giải pháp thành O(1) cho mỗi trường hợp kiểm thử sau khi tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(x_max * n) = O(100 * 100) | O(1) | Đã chấp nhận | 
| Tính toán trước / Tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case _n_. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc số khối mục tiêu _x_. 
3. Nếu _x_ lớn hơn hoặc bằng 12, in ngay CÓ. Điều này dựa vào tính chất là bất kỳ số nào từ 12 trở lên đều có thể phân tách thành tổng của 3 và 7. 
4. Nếu _x < 12_, hãy kiểm tra rõ ràng xem nó có thể được biểu diễn dưới dạng tổng của 3 giây và 7 giây hay không. Các số đại diện trong phạm vi này là 3, 6, 7, 9, 10. 
5. In CÓ nếu _x_ có thể biểu thị được, nếu không in ra NO. 

Tại sao nó hoạt động: Điều bất biến là các số ≥12 luôn có thể truy cập được vì việc cộng liên tục 3 vào các số đã có số 7 sẽ đảm bảo rằng tất cả các số tiếp theo đều được bao gồm. Đối với các số dưới 12, bảng liệt kê đầy đủ sẽ xác nhận giá trị nào có thể được biểu thị. Điều này đảm bảo rằng thuật toán sẽ không tuyên bố sai một con số không thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_buy_chunks(x):
    if x >= 12:
        return True
    return x in {3, 6, 7, 9, 10}

n = int(input())
for _ in range(n):
    x = int(input())
    print("YES" if can_buy_chunks(x) else "NO")
```chức năng`can_buy_chunks`gói gọn logic cốt lõi. Kiểm tra`x >= 12`nắm bắt cái nhìn sâu sắc tham lam cho số lượng lớn. Đối với các số nhỏ hơn, việc sử dụng một bộ đảm bảo kiểm tra tư cách thành viên O(1). I/O nhanh được sử dụng vì`input()`đọc từng dòng một cách hiệu quả. Các trường hợp biên, chẳng hạn như _x = 1_ hoặc _x = 2_, trả về NO một cách chính xác vì những trường hợp này không thể biểu thị được. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

| x | x >= 12 | x trong {3,6,7,9,10} | Đầu ra | 
| --- | --- | --- | --- | 
| 6 | Sai | Đúng | CÓ | 
| 5 | Sai | Sai | KHÔNG | 

Điều này xác nhận việc kiểm tra đã đặt hoạt động với số lượng nhỏ. 

**Ví dụ 2:** 

| x | x >= 12 | Đầu ra | 
| --- | --- | --- | 
| 14 | Đúng | CÓ | 
| 11 | Sai | CÓ (11 = 7 + 3 + 1? Không được phép chờ) | 

Sửa: Với x = 11, kiểm tra tập {3,6,7,9,10}. 11 không được thiết lập nên xuất ra NO. Đã xác nhận. Quy luật tham lam chỉ áp dụng cho x ≥ 12. 

Điều này thể hiện cả việc kiểm tra số lượng nhỏ và ngưỡng tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp kiểm thử được xử lý trong O(1) bằng kiểm tra tập hợp hoặc quy tắc tham lam | 
| Không gian | O(1) | Tập hợp các số nhỏ được tính toán trước là không đổi; không cần thêm bộ nhớ động | 

Với n 100, giá trị này phù hợp trong vòng 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    n = int(input())
    for _ in range(n):
        x = int(input())
        print("YES" if can_buy_chunks(x) else "NO")
    
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("2\n6\n5\n") == "YES\nNO", "sample 1"

# custom cases
assert run("5\n1\n2\n3\n4\n7\n") == "NO\nNO\nYES\nNO\nYES", "small numbers"
assert run("3\n12\n13\n14\n") == "YES\nYES\nYES", "threshold numbers >= 12"
assert run("2\n10\n11\n") == "YES\nNO", "precomputed vs non-representable below 12"
assert run("1\n100\n") == "YES", "large number"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 4 7 | KHÔNG KHÔNG CÓ KHÔNG CÓ | kiểm tra số nhỏ | 
| 12 13 14 | CÓ CÓ CÓ | ngưỡng tham lam cho x ≥ 12 | 
| 10 11 | CÓ KHÔNG | số cạnh dưới ngưỡng | 
| 100 | CÓ | phủ sóng số lượng lớn | 

## Vỏ cạnh 

Đối với mục tiêu nhỏ nhất có thể _x = 1_ hoặc _x = 2_, thuật toán trả về NO một cách chính xác. Khi _x = 3_ hoặc _x = 7_, thuật toán sẽ xác định chúng là có thể truy cập được từ các phần nhỏ và lớn tương ứng. Đối với các mục tiêu như 12 trở lên, thuật toán sẽ in CÓ ngay lập tức, dựa vào thuộc tính mà bất kỳ kết hợp nào cũng có thể được hình thành bằng cách thêm đủ bội số của 3 vào kết hợp cơ sở bao gồm 7. Điều này đảm bảo tính chính xác trên toàn bộ phạm vi đầu vào từ 1 đến 100.
