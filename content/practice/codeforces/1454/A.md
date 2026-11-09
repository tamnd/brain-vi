---
title: "CF 1454A - Hoán vị đặc biệt"
description: "Nhiệm vụ là xây dựng một hoán vị có độ dài (n 1) sao cho không có phần tử nào chiếm vị trí ban đầu của nó. Nói cách khác, đối với một hoán vị (p) có kích thước (n), chúng ta yêu cầu (pi ne i) đó cho mọi chỉ số (i)."
date: "2026-06-11T02:58:11+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1454
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 686 (Div. 3)"
rating: 800
weight: 1454
solve_time_s: 462
verified: false
draft: false
---

[CF 1454A - Hoán vị đặc biệt](https://codeforces.com/problemset/problem/1454/A) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng, xác suất 
**Thời gian giải:** 7 phút 42 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xây dựng một hoán vị có độ dài\(n > 1\)sao cho không có phần tử nào chiếm giữ vị trí ban đầu của nó. Nói cách khác, đối với một hoán vị\(p\)kích thước\(n\), chúng tôi yêu cầu điều đó\(p_i \ne i\)cho mọi chỉ số\(i\). Mỗi trường hợp thử nghiệm bao gồm một số nguyên duy nhất\(n\)và đầu ra phải là một chuỗi\(n\)các số nguyên tạo thành một hoán vị hợp lệ đáp ứng điều kiện đã nêu. 

Những hạn chế còn khiêm tốn:\(n\)có thể có nhiều nhất là 100 và có tới 100 trường hợp thử nghiệm. Điều này có nghĩa là mọi cách tiếp cận \(O(n^2)\) đều đủ nhanh, mặc dù có thể có giải pháp thời gian tuyến tính. Các trường hợp cạnh chính xảy ra khi\(n\)là tối thiểu, cụ thể\(n = 2\), trong đó chỉ có một khả năng có thể xảy ra: hoán đổi hai số. Đối với lớn hơn\(n\), có nhiều hoán vị hợp lệ, do đó giải pháp có thể mang tính xây dựng và không yêu cầu tìm kiếm hoặc tối ưu hóa. 

Một cách tiếp cận đơn giản chỉ ngẫu nhiên hóa các số có thể thất bại vì nó có thể vô tình đặt một số số vào vị trí ban đầu của chúng và việc xác minh nó sẽ yêu cầu kiểm tra từng hoán vị. Điều này sẽ phức tạp không cần thiết do tính đơn giản của vấn đề. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra tất cả các hoán vị có độ dài\(n\)và kiểm tra từng phần để đảm bảo không có phần tử nào ở vị trí ban đầu. Mặc dù đúng nhưng cách tiếp cận này có độ phức tạp theo giai thừa, \(O(n!)\) và trở nên không khả thi ngay cả đối với\(n = 10\). 

Quan sát quan trọng là một sự dịch chuyển theo chu kỳ đơn giản của tất cả các phần tử theo một vị trí sẽ tạo ra một hoán vị hợp lệ cho tất cả\(n > 1\). Cụ thể, việc đặt phần tử đầu tiên ở vị trí thứ hai, phần tử thứ hai ở vị trí thứ ba, ..., và phần tử cuối cùng ở vị trí đầu tiên đảm bảo rằng không có phần tử nào còn ở vị trí ban đầu. Đây là một cách tiếp cận mang tính xây dựng theo thời gian tuyến tính và đáp ứng tất cả các ràng buộc một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Brute Force (tất cả các hoán vị) | Ồ (n!) | O(n) | Quá chậm | 
| Chuyển đổi theo chu kỳ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,\(t\). 
2. Đối với mỗi test case: 
1. Đọc số nguyên\(n\), độ dài của hoán vị. 
2. Tạo danh sách các số nguyên từ 1 đến\(n\). 
3. Xây dựng hoán vị bằng cách thực hiện phép dịch chuyển tuần hoàn: di chuyển phần tử đầu tiên về cuối và dịch chuyển tất cả các phần tử còn lại sang trái một vị trí. 
4. In hoán vị kết quả dưới dạng một chuỗi được phân tách bằng dấu cách. 

Tại sao nó hoạt động: Bằng cách dịch chuyển mọi phần tử sang trái một vị trí và di chuyển phần tử đầu tiên đến cuối, mỗi số\(i\)di chuyển từ vị trí\(i\)đến vị trí \((i-1 \mod n) + 1\). Điều này đảm bảo rằng không có phần tử nào còn ở vị trí ban đầu, đáp ứng yêu cầu bài toán cho tất cả\(n > 1\). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        perm = list(range(1, n + 1))
        # cyclic shift: first element moves to the end
        perm = perm[1:] + perm[:1]
        print(' '.join(map(str, perm)))

if __name__ == "__main__":
    solve()
```Giải pháp đọc số lượng ca kiểm thử trước, sau đó lặp lại từng trường hợp kiểm thử. Nó xây dựng một phạm vi từ 1 đến\(n\)và ngay lập tức áp dụng phép dịch chuyển theo chu kỳ, đây là phương pháp đơn giản nhất để tạo ra sự biến dạng hợp lệ. Việc in được thực hiện dưới dạng chuỗi được phân tách bằng dấu cách, đảm bảo định dạng chính xác. Việc sử dụng`sys.stdin.readline`đảm bảo xử lý đầu vào nhanh chóng cho số lượng trường hợp thử nghiệm tối đa. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2
2
5
```| Bước |`perm`trước ca |`perm`sau ca | 
|------|-------------------|-----------------| 
| Trường hợp thử nghiệm 1 | [1, 2] | [2, 1] | 
| Trường hợp thử nghiệm 2 | [1, 2, 3, 4, 5] | [2, 3, 4, 5, 1] ​​| 

Điều này chứng tỏ rằng phép dịch chuyển tuần hoàn luôn tạo ra một hoán vị không có phần tử nào ở vị trí ban đầu. Vì\(n=2\), trao đổi duy nhất có thể hoạt động. Vì\(n=5\), hoán vị`[2, 3, 4, 5, 1]`đáp ứng mọi yêu cầu. 

### Đầu vào mẫu 2```
1
3
```| Bước |`perm`trước ca |`perm`sau ca | 
|------|-------------------|-----------------| 
| Trường hợp thử nghiệm 1 | [1, 2, 3] | [2, 3, 1] | 

This shows that even small odd-sized arrays work, confirming the cyclic shift method is general.

 ## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Generating and shifting the permutation takes linear time |
 | Không gian | O(n) cho mỗi trường hợp thử nghiệm | Mảng hoán vị yêu cầu không gian tuyến tính | 

Được cho\(n \le 100\)Và\(t \le 100\), giải pháp dễ dàng phù hợp với cả giới hạn thời gian 1 giây và hạn chế về bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# Provided samples
assert run("2\n2\n5\n") == "2 1\n2 3 4 5 1", "sample 1"

# Custom cases
assert run("1\n3\n") == "2 3 1", "odd n"
assert run("1\n4\n") == "2 3 4 1", "even n"
assert run("1\n100\n") == " ".join(map(str, list(range(2,101))+[1])), "max n"
assert run("3\n2\n3\n4\n") == "2 1\n2 3 1\n2 3 4 1", "multiple test cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`1\n3\n`|`2 3 1`| Hoán vị cỡ lẻ nhỏ | 
|`1\n4\n`|`2 3 4 1`| Hoán vị cỡ chẵn nhỏ | 
|`1\n100\n`|`2 ... 100 1`| Kích thước đầu vào tối đa | 
|`3\n2\n3\n4\n`|`2 1\n2 3 1\n2 3 4 1`| Xử lý nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

Tối thiểu\(n = 2\)trường hợp`[1,2]`trở thành`[2,1]`với sự dịch chuyển theo chu kỳ. lớn nhất\(n = 100\)trường hợp sử dụng nguyên tắc tương tự; phần tử đầu tiên di chuyển về cuối và tất cả các phần tử khác dịch chuyển sang trái, điều này đảm bảo không có phần tử nào còn ở vị trí ban đầu. Tất cả các đầu vào có kích thước lẻ, chẵn và tối đa đều tuân theo cùng một logic dịch chuyển tuyến tính, xác nhận tính chính xác.
