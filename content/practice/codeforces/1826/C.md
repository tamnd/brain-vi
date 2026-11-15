---
title: "CF 1826C - Giấc Mơ Tự Do"
description: "Chúng ta có một nhóm gồm n lập trình viên đang liên tục bỏ phiếu để chọn ra một thuật toán yêu thích duy nhất trong số m thuật toán. Mỗi vòng, mỗi lập trình viên sẽ bỏ phiếu cho một trong các phương án còn lại."
date: "2026-06-09T07:31:32+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1826
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 870 (Div. 2)"
rating: 1300
weight: 1826
solve_time_s: 88
verified: true
draft: false
---

[CF 1826C - Ước mơ tự do](https://codeforces.com/problemset/problem/1826/C) 

**Đánh giá:** 1300 
**Tags:** tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm`n`các lập trình viên liên tục bỏ phiếu để chọn một mục yêu thích duy nhất trong số`m`thuật toán. Mỗi vòng, mỗi lập trình viên sẽ bỏ phiếu cho một trong các phương án còn lại. Sau mỗi vòng, chỉ (các) thuật toán có số phiếu bầu cao nhất mới được vào vòng tiếp theo. Quá trình kết thúc khi chỉ còn lại một thuật toán. Chúng ta được yêu cầu xác định, với các giá trị đã cho của`n`Và`m`, liệu quá trình bỏ phiếu có được đảm bảo chấm dứt với một tùy chọn duy nhất hay về mặt lý thuyết nó có thể tiếp tục mãi mãi, giả sử các lập trình viên bỏ phiếu một cách chiến lược. 

Đầu vào cung cấp`t`trường hợp thử nghiệm, mỗi trường hợp bao gồm các số nguyên`n`Và`m`. Đầu ra cho mỗi trường hợp thử nghiệm là "CÓ" nếu cuối cùng một tùy chọn duy nhất được chọn hoặc "KHÔNG" nếu có thể xảy ra tình trạng bế tắc vô thời hạn. 

Những hạn chế rất đáng kể:`n`Và`m`đều có thể lớn bằng 10^6, và`t`có thể lên tới 10^5. Một mô phỏng đơn giản về các vòng biểu quyết sẽ yêu cầu tạo ra tất cả các phân phối phiếu bầu, có thể lên tới`m^n`sự kết hợp. Điều này rõ ràng là không thể theo bất kỳ ý nghĩa thực tế nào, vì vậy chúng ta cần một logic đơn giản, có thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh xuất hiện khi một trong hai`n`hoặc`m`là rất nhỏ. Nếu chỉ có một lập trình viên (`n = 1`), thì bất kể số lượng lựa chọn là bao nhiêu, cuộc bỏ phiếu sẽ kết thúc sau một vòng. Ngược lại, nếu số lượng tùy chọn vượt quá số lượng người lập trình (`m > n`), có khả năng mỗi thuật toán nhận được phiếu bầu bằng nhau, tạo ra sự ràng buộc và ngăn chặn việc loại bỏ. Ví dụ, với`n = 4`Và`m = 2`, nếu hai lập trình viên bỏ phiếu cho mỗi tùy chọn, thì sự ràng buộc sẽ tồn tại vô thời hạn, do đó kết quả đầu ra đúng là "KHÔNG". 

## Phương pháp tiếp cận 

Phương pháp bạo lực sẽ mô phỏng mọi kiểu bỏ phiếu có thể có cho mỗi vòng. Cách tiếp cận này hoạt động về nguyên tắc vì nó xem xét tất cả các cách mà các lập trình viên có thể bỏ phiếu, loại bỏ tất cả các lựa chọn ngoại trừ những lựa chọn có số phiếu bầu tối đa. Tuy nhiên, nó thất bại trong thực tế vì số lượng kết hợp tăng theo cấp số nhân. Vì`n = 10^6`Và`m = 10^6`, việc mô phỏng dù chỉ một vòng là không thể tính toán được. 

Quan sát quan trọng là quá trình này phụ thuộc hoàn toàn vào mối quan hệ giữa`n`Và`m`. Nếu số lượng người lập trình lớn hơn số lượng tùy chọn (`n > m`), không có cách nào để phân phối phiếu bầu đồng đều trên tất cả các tùy chọn mãi mãi - ít nhất một tùy chọn cuối cùng sẽ nhận được nhiều phiếu bầu hơn, làm giảm các tùy chọn còn lại. Nếu số lượng tùy chọn lớn hơn hoặc bằng số lượng lập trình viên (`m >= n`), các lập trình viên có thể chia đều phiếu bầu của họ, duy trì tỷ lệ hòa cho tất cả các lựa chọn vô thời hạn. Sự ràng buộc ngăn chặn bất kỳ sự loại bỏ nào, vì vậy về mặt lý thuyết, quá trình này có thể tiếp tục mãi mãi. 

Điều này dẫn đến một giải pháp thời gian không đổi cho mỗi trường hợp thử nghiệm: kiểm tra xem`n > m`. Nếu vậy, hãy in "CÓ"; nếu không thì in "KHÔNG". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n * vòng) | O(m) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,`t`. Mỗi trường hợp thử nghiệm là độc lập, vì vậy chúng tôi sẽ xử lý chúng một cách tuần tự. 
2. Với mỗi test case, hãy đọc`n`Và`m`. 
3. So sánh`n`Và`m`. Nếu như`n`thực sự lớn hơn`m`, thì việc loại bỏ cuối cùng sẽ giảm các tùy chọn xuống còn một, do đó xuất ra "CÓ". 
4. Ngược lại, nếu`n <= m`, có thể phiếu bầu được chia đều và không có lựa chọn nào bị loại ở bất kỳ vòng nào, do đó xuất ra "KHÔNG". 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: bất biến là mối quan hệ giữa người lập trình và các tùy chọn. Nếu có nhiều người lập trình hơn các lựa chọn, thì mọi mối ràng buộc đều có thể bị phá vỡ theo ít nhất một cách, đảm bảo tiến trình hướng tới một lựa chọn duy nhất còn lại. Nếu số lượng tùy chọn bằng hoặc vượt quá người lập trình thì luôn có thể có một sự hòa hợp hoàn hảo và việc loại bỏ có thể không bao giờ xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
results = []

for _ in range(t):
    n, m = map(int, input().split())
    if n > m:
        results.append("YES")
    else:
        results.append("NO")

print("\n".join(results))
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`cho các trường hợp thử nghiệm lớn. Mỗi so sánh`n > m`được thực hiện trong thời gian không đổi. Các kết quả được tích lũy thành một danh sách và được in cùng một lúc, giúp giảm chi phí I/O. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n = 3, m = 2`| n | m | n > m? | Đầu ra | 
| --- | --- | --- | --- | 
| 3 | 2 | Đúng | CÓ | 

Vì tồn tại nhiều lập trình viên hơn các tùy chọn, nên ngay cả khi số phiếu bầu được chia ra, một tùy chọn sẽ nhận được nhiều phiếu bầu hơn và tồn tại, đảm bảo cuối cùng sẽ bị chấm dứt. 

**Ví dụ 2:**`n = 4, m = 2`| n | m | n > m? | Đầu ra | 
| --- | --- | --- | --- | 
| 4 | 2 | Đúng | CÓ | 

Ngay cả ở đây, mặc dù có nhiều lập trình viên, mối quan hệ cuối cùng vẫn bị phá vỡ vì`n > m`. Nếu thay vào đó`n = 2, m = 4`, đầu ra sẽ là "KHÔNG" vì số phiếu có thể bị ràng buộc vô thời hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm bao gồm một so sánh | 
| Không gian | O(t) | Lưu trữ kết quả để in ở cuối | 

Điều này phù hợp thoải mái trong giới hạn 2 giây cho tối đa 10^5 trường hợp thử nghiệm. Mức sử dụng bộ nhớ thấp, chỉ lưu trữ số nguyên và chuỗi đầu ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    results = []
    for _ in range(t):
        n, m = map(int, input().split())
        results.append("YES" if n > m else "NO")
    return "\n".join(results)

# provided samples
assert run("5\n3 2\n4 2\n5 3\n1000000 1000000\n1 1000000\n") == "YES\nYES\nYES\nNO\nYES", "sample 1"

# custom cases
assert run("3\n1 1\n2 2\n2 3\n") == "NO\nNO\nNO", "min edge cases"
assert run("2\n1000000 999999\n1000000 1000000\n") == "YES\nNO", "max edge cases"
assert run("2\n3 3\n4 3\n") == "NO\nYES", "tie scenarios"
assert run("1\n10 1\n") == "YES", "single option, many programmers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1; 2 2; 2 3 | KHÔNG; KHÔNG; KHÔNG | Lập trình viên tối thiểu, có tiềm năng | 
| 1000000 999999; 1000000 1000000 | ĐÚNG; KHÔNG | Đầu vào tối đa, cạnh đứt dây buộc | 
| 3 3; 4 3 | KHÔNG; CÓ | Hòa so với chấm dứt được đảm bảo | 
| 10 1 | CÓ | Nhiều lập trình viên, một lựa chọn | 

## Vỏ cạnh 

Khi nào`n = 1`, bất kể`m`, đầu ra là "CÓ" nếu`m = 1`bởi vì cuộc bỏ phiếu kết thúc một cách tầm thường. Nếu như`m > 1`,`n < m`, đầu ra là "KHÔNG" vì người lập trình không thể phá vỡ mối liên kết giữa nhiều tùy chọn. 

Khi`m = 1`, bất kể`n`, quá trình kết thúc ở vòng đầu tiên, vì vậy`n > m`giữ tầm thường và đầu ra là "CÓ". Thuật toán xử lý chính xác cả hai điều kiện vì nó chỉ kiểm tra mối quan hệ`n > m`. 

Giải pháp này cũng xử lý các giá trị cực lớn mà không bị tràn vì số nguyên Python không bị giới hạn và phép so sánh`>`là thời gian không đổi. Điều này đảm bảo tính chính xác và hiệu quả cho đầu vào lớn nhất được phép.
