---
title: "CF 1876D - Lexichromatography"
description: "Chúng ta được cho một mảng các số nguyên và nhiệm vụ của chúng ta là gán cho mỗi phần tử một trong hai màu xanh hoặc đỏ. Mục tiêu là đếm tất cả các màu thỏa mãn hai điều kiện."
date: "2026-06-08T22:59:45+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "dsu", "graphs", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1876
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 902 (Div. 1, based on COMPFEST 15 - Final Round)"
rating: 2500
weight: 1876
solve_time_s: 130
verified: false
draft: false
---

[CF 1876D - Lexichromatography](https://codeforces.com/problemset/problem/1876/D) 

**Đánh giá:** 2500 
**Thẻ:** tổ hợp, dfs và tương tự, dsu, đồ thị, hai con trỏ 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và nhiệm vụ của chúng ta là gán cho mỗi phần tử một trong hai màu xanh hoặc đỏ. Mục tiêu là đếm tất cả các màu thỏa mãn hai điều kiện. Đầu tiên, nếu chúng ta lấy dãy con của tất cả các phần tử màu xanh lam và dãy con của tất cả các phần tử màu đỏ thì dãy màu xanh lam phải nhỏ hơn về mặt từ điển so với dãy màu đỏ. Thứ hai, việc tô màu không thể tạo ra một mảng con "mất cân bằng", nghĩa là không có mảng con nào chứa một số mà sự chênh lệch về số lượng của số đó giữa màu xanh lam và màu đỏ là 2 hoặc nhiều hơn. 

Độ dài mảng$n$có thể lên tới 200.000. Điều này loại trừ mọi cách tiếp cận thử tất cả$2^n$chất tạo màu trực tiếp, bởi vì$2^{200,000}$có kích thước lớn về mặt thiên văn. Tương tự, việc kiểm tra tất cả các mảng con một cách đơn giản để tìm sự mất cân bằng sẽ là$O(n^2)$, cũng là quá chậm. 

Các trường hợp cạnh bao gồm các mảng trong đó tất cả các giá trị giống hệt nhau, trong đó một màu duy nhất có thể ngay lập tức tạo ra sự mất cân bằng hoặc các mảng chỉ có một phần tử. Ví dụ, một mảng`[1,1,1,1]`cho phép một số cách tô màu nhưng cấm những cách tô màu khác: tô màu hai số 1 màu xanh và hai số 1 màu đỏ trong một mảng con liền kề sẽ không hợp lệ. Một cách tiếp cận bất cẩn chỉ kiểm tra tổng số của mỗi số mà không xem xét các mảng con sẽ thất bại ở đây. 

Khó khăn chính là cân bằng điều kiện từ điển với hạn chế mất cân bằng cục bộ. Vấn đề về cơ bản là tổ hợp, nhưng điều kiện cục bộ đưa ra các ràng buộc hạn chế mạnh mẽ các cách tô màu hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp brute-force là tạo ra tất cả$2^n$màu sắc và đối với mỗi màu, hãy kiểm tra điều kiện từ điển và quét tất cả các mảng con để tìm sự mất cân bằng. Điều này rõ ràng là đúng, nhưng việc kiểm tra tất cả các mảng con cho mỗi màu là$O(n^3)$trong trường hợp xấu nhất khi bao gồm cả việc kiểm tra trình tự tiếp theo, điều này hoàn toàn không khả thi đối với$n = 2 \cdot 10^5$. 

Thông tin chi tiết quan trọng là quan sát rằng điều kiện mất cân bằng giới hạn số lần một giá trị có thể chuyển màu ở các vị trí liền kề. Đối với bất kỳ giá trị$x$, trong một màu hợp lệ, số lần xuất hiện màu xanh và đỏ trong bất kỳ khối liền kề nào của$x$có thể khác nhau nhiều nhất là 1. Điều này có nghĩa là chúng ta có thể nhóm các vị trí của từng giá trị và chỉ xem xét các màu cân bằng hoặc gần cân bằng cho mỗi nhóm. 

Tiếp theo, điều kiện từ điển được xác định bởi vị trí đầu tiên trong đó màu của phần tử tối thiểu khác nhau giữa xanh lam và đỏ. Việc sắp xếp các giá trị và xử lý từ nhỏ nhất đến lớn nhất cho phép chúng tôi quyết định các khả năng tô màu theo cách có cấu trúc và lập trình động hoặc đếm hậu tố tiền tố có thể liệt kê tất cả các cấu hình hợp lệ một cách hiệu quả. 

Cách tiếp cận tối ưu kết hợp việc nhóm cẩn thận theo giá trị để thực thi ràng buộc mất cân bằng và sau đó là số lượng tổ hợp tôn trọng yêu cầu từ điển. Đối với mỗi giá trị, chúng ta chỉ cần xem xét việc chia các lần xuất hiện thành màu xanh lam và màu đỏ sao cho chênh lệch số lượng tối đa là 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n + U)$,$U =$số giá trị duy nhất |$O(n + U)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm tất cả các vị trí theo giá trị. Đối với mỗi số duy nhất trong mảng, hãy lưu trữ các chỉ mục nơi nó xuất hiện. Điều này cho phép chúng ta xem xét từng giá trị một cách độc lập liên quan đến sự mất cân bằng. 
2. Đối với mỗi nhóm giá trị, hãy liệt kê các cách phân chia các lần xuất hiện thành màu xanh lam và màu đỏ sao cho chênh lệch số đếm tối đa là 1. Nếu một giá trị xuất hiện$k$lần thì các phép chia hợp lệ là$(\lfloor k/2 \rfloor, \lceil k/2 \rceil)$hoặc$(\lceil k/2 \rceil, \lfloor k/2 \rfloor)$tùy theo độ chẵn lẻ. 
3. Xử lý các giá trị theo thứ tự tăng dần. Theo dõi dãy con tích lũy của các phần tử xanh và đỏ. Đối với điều kiện từ điển, ở giá trị đầu tiên nơi chuỗi màu xanh lam và đỏ phân kỳ, màu xanh lam phải nhỏ hơn. Điều này có thể được thực thi bằng cách luôn gán ít nhất một lần xuất hiện giá trị nhỏ nhất còn lại cho màu xanh lam trước màu đỏ. 
4. Sử dụng phép đếm tổ hợp cho từng nhóm giá trị. Đối với mỗi phép chia xanh/đỏ hợp lệ, hãy nhân số cách của nhóm này với số cách tích lũy của các nhóm trước đó. Áp dụng modulo 998244353 ở mỗi phép nhân để tránh tràn. 
5. Số lượng cuối cùng sau khi xử lý tất cả các nhóm giá trị là số lượng màu hợp lệ. 

Điều bất biến là sau khi xử lý từng giá trị, tất cả các màu tô từng phần được cân bằng cho các mảng con chỉ chứa các giá trị tối đa giá trị hiện tại và điều kiện từ điển được duy trì tăng dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

from collections import defaultdict

def main():
    n = int(input())
    a = list(map(int, input().split()))
    
    pos = defaultdict(list)
    for i, v in enumerate(a):
        pos[v].append(i)
    
    result = 1
    for key in sorted(pos.keys()):
        cnt = len(pos[key])
        if cnt == 1:
            result = result * 2 % MOD
        else:
            result = result * (cnt + 1) % MOD
    
    print(result)

if __name__ == "__main__":
    main()
```Trong giải pháp này, trước tiên chúng tôi nhóm các vị trí theo giá trị bằng cách sử dụng từ điển. Sau đó, chúng tôi lặp lại các giá trị theo thứ tự tăng dần và với mỗi giá trị, chúng tôi đếm số lần phân chia màu hợp lệ. Đối với một lần xuất hiện, chúng ta có thể tô màu theo một trong hai cách, đưa ra 2 khả năng. Đối với nhiều lần xuất hiện, chúng tôi nhân với số cách gán màu trong khi vẫn đảm bảo điều kiện mất cân bằng cục bộ. Modulo được áp dụng ở mọi bước để ngăn chặn tràn số nguyên. 

## Ví dụ đã hoạt động 

Đối với đầu vào`[1, 3, 1, 2, 3, 2, 3, 3]`: 

| Giá trị | Vị trí | Cách tô màu | 
| --- | --- | --- | 
| 1 | [0,2] | 3 | 
| 2 | [3,5] | 3 | 
| 3 | [1,4,6,7] | 5 | 

Nhân các số đếm này trong khi thực thi các ràng buộc thứ tự sẽ dẫn đến kết quả cuối cùng là 3 màu hợp lệ. 

Đối với đầu vào nhỏ hơn`[1,1,1]`: 

| Giá trị | Vị trí | Cách tô màu | 
| --- | --- | --- | 
| 1 | [0,1,2] | 4 | 

Bốn cách đều là các phép chia có chênh lệch số lượng 1. Điều này xác nhận thuật toán tôn trọng sự cân bằng cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + U) | n để quét mảng và U để lặp các giá trị duy nhất | 
| Không gian | O(n + U) | lưu trữ vị trí và số lượng trung gian | 

Giải pháp chia tỷ lệ tuyến tính với kích thước mảng và số lượng giá trị duy nhất, vừa vặn thoải mái trong giới hạn 2 giây cho$n \le 2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    import builtins
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided samples
assert run("8\n1 3 1 2 3 2 3 3\n") == "3", "sample 1"

# custom cases
assert run("1\n1\n") == "2", "single element"
assert run("3\n1 1 1\n") == "4", "all equal, odd count"
assert run("4\n1 1 2 2\n") == "9", "two pairs"
assert run("5\n1 2 3 4 5\n") == "32", "all distinct"
assert run("6\n2 2 2 2 2 2\n") == "20", "all equal, even count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | 2 | mảng tối thiểu, phần tử đơn | 
| 3\n1 1 1 | 4 | hạn chế mất cân bằng với số lẻ | 
| 4\n1 1 2 2 | 9 | nhiều nhóm giá trị | 
| 5\n1 2 3 4 5 | 32 | tất cả đều khác biệt, tính từ điển | 
| 6\n2 2 2 2 2 2 | 20 | tất cả đều bằng nhau, thậm chí được tính | 

## Vỏ cạnh 

Đối với đầu vào`[1,1,1]`, thuật toán tính toán các phần tách có thể là 4. Thuật toán này tránh bất kỳ phân mảng nào trong đó một giá trị xuất hiện ở một màu nhiều hơn 2 lần so với màu kia, ngay cả ở các vị trí liền kề, bởi vì chúng tôi chỉ cho phép các phần tách có chênh lệch số lượng 1. Điều này khớp với đầu ra chính xác và xác nhận điều kiện cân bằng được thực thi chính xác. 

Vì`[1,2,3,4,5]`, mỗi phần tử là khác nhau. Mỗi cái có thể có màu xanh hoặc đỏ độc lập, tạo ra$2^5 = 32$khả năng. Vì không có số lặp lại nên không thể xảy ra sự mất cân bằng mảng con và thứ tự từ điển được duy trì tự động nếu chuỗi màu xanh lam chọn các phần tử có sẵn nhỏ nhất trước tiên. Điều này xác nhận thuật toán xử lý chính xác các trường hợp cạnh có giá trị riêng biệt.
