---
title: "CF 1778A - Tổng Flip Flop"
description: "Chúng ta được cung cấp một mảng chỉ bao gồm 1 và -1. Nhiệm vụ là thực hiện đúng một thao tác: chọn hai phần tử liên tiếp và lật dấu của chúng. Sau khi thực hiện việc này, chúng tôi muốn tối đa hóa tổng của tất cả các phần tử mảng."
date: "2026-06-09T11:36:09+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1778
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 848 (Div. 2)"
rating: 800
weight: 1778
solve_time_s: 201
verified: false
draft: false
---

[CF 1778A - Tổng Flip Flop](https://codeforces.com/problemset/problem/1778/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 3m 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng chỉ bao gồm`1`cát`-1`S. Nhiệm vụ là thực hiện đúng một thao tác: chọn hai phần tử liên tiếp và lật dấu của chúng. Sau khi thực hiện việc này, chúng tôi muốn tối đa hóa tổng của tất cả các phần tử mảng. Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có mảng và đầu ra phải là tổng tối đa có thể đạt được cho mỗi trường hợp. 

Các ràng buộc chỉ ra rằng mảng có thể khá dài, lên tới 100.000 phần tử cho mỗi trường hợp thử nghiệm và tổng số phần tử trên tất cả các trường hợp thử nghiệm cũng bị giới hạn ở mức 100.000. Điều này cho chúng ta biết rằng bất kỳ thuật toán nào có độ phức tạp kém hơn tuyến tính trong`n`mỗi trường hợp thử nghiệm sẽ quá chậm. 

Các trường hợp cạnh phát sinh khi mảng có tất cả`1`s hoặc tất cả`-1`S. Ví dụ, nếu mảng là`[1,1,1,1]`, lật hai phần tử bất kỳ sẽ làm giảm tổng nên cách di chuyển tối ưu là giảm thiểu thiệt hại. Ngược lại, nếu mảng là`[-1,-1]`, lật chúng sẽ tăng tổng từ`-2`ĐẾN`2`. Một trường hợp tinh tế khác xảy ra khi mảng chứa một hỗn hợp nhưng có một cặp liên tiếp`-1`hoặc`1`s ở cuối; việc chọn đúng cặp có thể tạo ra sự cải thiện lớn nhất hoặc ngăn chặn sự suy giảm. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên mọi chỉ mục`i`từ`1`ĐẾN`n-1`, lật`a[i]`Và`a[i+1]`, tính tổng và theo dõi mức tối đa. Điều này đúng vì nó kiểm tra tất cả các khả năng, nhưng nó yêu cầu các thao tác O(n) cho mỗi trường hợp kiểm thử, điều này có thể chấp nhận được trong bài toán này nhưng không cần thiết. Cách tiếp cận đơn giản hoạt động ở đây do tổng kích thước đầu vào nhỏ nhưng có thể được đơn giản hóa bằng quan sát tham lam. 

Cái nhìn sâu sắc quan trọng là việc lật một cặp số liên tiếp sẽ thay đổi tổng như sau. Nếu cặp đó là`[x, y]`, lật cả hai cho`-x - y`. Tổng thay đổi là`(-x - y) - (x + y) = -2*(x + y)`. Để tối đa hóa tổng, chúng tôi muốn`x + y`càng nhỏ càng tốt bởi vì`-2*(x + y)`thì sẽ là mức tăng lớn nhất. Tổng nhỏ nhất có thể có của hai phần tử là`-2`, xảy ra khi cả hai đều`-1`. Ngược lại, tổng lớn nhất là`2`(cả hai`1`s), làm giảm tổng nếu bị lật. Vì vậy, chiến lược tối ưu là kiểm tra xem có tồn tại một cặp`-1, -1`; nếu vậy, việc lật cặp đó sẽ tăng tổng lên 4. Nếu không có cặp nào như vậy tồn tại, việc lật một cặp`1,1`hoặc`1,-1`giảm tổng đi 4 hoặc 0. 

Với số lượng nhỏ khả năng của các cặp (`-1,-1`,`1,1`,`1,-1`,`-1,1`), chúng ta có thể tham lam kiểm tra mảng các mẫu này và áp dụng thay đổi tương ứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Lật Mỗi Cặp | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận nhưng có thể đơn giản hóa | 
| Tham lam Dựa trên Cặp Tổng | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận và đơn giản hơn | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc độ dài của mảng`n`và mảng`a`. 
3. Tính tổng ban đầu của mảng. Đây là đường cơ sở trước khi thực hiện bất kỳ cú lật nào. 
4. Lặp lại mảng từ phần tử đầu tiên đến phần tử thứ hai đến phần tử cuối cùng. Đối với mỗi chỉ số`i`, tính tổng của`a[i] + a[i+1]`. 
5. Nếu số tiền này là`-2`(cả hai`-1`s), sau đó lật cặp này sẽ tăng tổng số tiền lên 4. Ghi lại rằng đây là mức tăng tốt nhất có thể và phá vỡ vòng lặp vì không có cặp nào khác có thể cho mức tăng lớn hơn. 
6. Nếu không có`-1,-1`cặp, kiểm tra`1,1`cặp; lật chúng sẽ giảm tổng đi 4. Nếu chỉ có`1,-1`hoặc`-1,1`cặp, lật chúng sẽ thay đổi tổng bằng 0. 
7. Thêm mức tăng hoặc giảm tốt nhất có thể đạt được vào tổng ban đầu và đưa ra kết quả. 

Tại sao nó hoạt động: Thao tác chỉ ảnh hưởng đến hai phần tử liên tiếp. Sự thay đổi tổng chỉ phụ thuộc vào hai yếu tố này. Bằng cách kiểm tra bốn cặp có thể và chọn cặp có mức tăng tối đa (hoặc giảm thiểu tối đa), chúng tôi đảm bảo tổng tối ưu. Vì mảng chỉ chứa`1`Và`-1`, đây là bốn cấu hình duy nhất nên sự lựa chọn tham lam là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = sum(a)
        best_change = -float('inf')
        for i in range(n-1):
            pair_sum = a[i] + a[i+1]
            change = -2 * pair_sum
            if change > best_change:
                best_change = change
            if best_change == 4:  # maximum possible, can't do better
                break
        print(total + best_change)

if __name__ == "__main__":
    solve()
```Giải pháp tính tổng một lần, lặp qua các cặp liên tiếp, tính toán sự thay đổi tiềm năng nếu bị đảo ngược và giữ mức cải thiện lớn nhất có thể. Nó bị đoản mạch một lần một cặp`-1,-1`được tìm thấy vì điều này mang lại mức tăng tối đa. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5
-1 1 1 -1 -1
```| Chỉ mục tôi | một [tôi] | a[i+1] | cặp_tổng | thay đổi | best_change | 
| --- | --- | --- | --- | --- | --- | 
| 0 | -1 | 1 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 2 | -4 | 0 | 
| 2 | 1 | -1 | 0 | 0 | 0 | 
| 3 | -1 | -1 | -2 | 4 | 4 | 

Tổng ban đầu = -1 + 1 + 1 -1 -1 = -1. Tổng tối đa = -1 + 4 = 3. 

Đối với đầu vào:```
1 1
```Chỉ có tổng cặp = 2, thay đổi = -4. Tổng ban đầu = 2. Tổng tối đa = 2 - 4 = -2. 

Điều này xác nhận thuật toán chọn cặp tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Lặp lại qua n-1 cặp liên tiếp | 
| Không gian | O(n) | Lưu trữ mảng a | 

Với tổng số n trên tất cả các trường hợp thử nghiệm ≤ 10^5, giải pháp này chạy tốt trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io
    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n5\n-1 1 1 -1 -1\n5\n1 1 -1 -1 -1\n2\n1 1\n4\n1 -1 -1 1\n") == "3\n3\n-2\n4"

# custom cases
assert run("1\n2\n-1 -1\n") == "2", "flip -1 -1 to increase sum"
assert run("1\n3\n1 1 1\n") == "-1", "flip 1 1 to decrease sum minimally"
assert run("1\n4\n1 -1 1 -1\n") == "0", "flip any 1 -1 pair keeps sum same"
assert run("1\n5\n-1 -1 -1 -1 -1\n") == "1", "flip first -1 -1 gives maximum increase"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n-1 -1 | 2 | mảng tối thiểu, flip tăng tổng | 
| 3\n1 1 1 | -1 | 1s liên tiếp, phải lật để giảm thiểu tổn thất | 
| 4\n1 -1 1 -1 | 0 | đảo các cặp hỗn hợp, tổng không đổi | 
| 5\n-1 -1 -1 -1 -1 | 1 | nhiều -1, chọn cặp tối ưu để có tổng tối đa | 

## Vỏ cạnh 

Đối với mảng hai phần tử`[1,1]`, thao tác duy nhất là lật hai phần tử này. Tổng ban đầu là 2, lật các thay đổi thành -2. Thuật toán tính toán`pair_sum = 2`,`change = -4`và cập nhật chính xác tổng số tiền thành -2. Vì`[ -1, -1 ]`, lật tăng tổng từ -2 lên 2. Mảng có dấu xen kẽ như`[1, -1, 1, -1]`luôn sản xuất`change = 0`, do đó tổng số tiền không thay đổi. Giải pháp xử lý chính xác tất cả các trường hợp cạnh như vậy vì nó kiểm tra từng cặp liên tiếp và chọn một cặp tối đa hóa thay đổi ròng.
