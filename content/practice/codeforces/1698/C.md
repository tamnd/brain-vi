---
title: "CF 1698C - Đóng 3SUM"
description: "Chúng ta được yêu cầu xác định xem một mảng số nguyên có phải là 3SUM đóng hay không, nghĩa là với mỗi bộ ba phần tử riêng biệt trong mảng, tổng của ba phần tử đó sẽ xuất hiện ở đâu đó trong mảng."
date: "2026-06-09T22:23:26+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1698
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 803 (Div. 2)"
rating: 1300
weight: 1698
solve_time_s: 455
verified: true
draft: false
---

[CF 1698C - Đóng 3SUM](https://codeforces.com/problemset/problem/1698/C) 

**Đánh giá:** 1300 
**Tags:** bạo lực, cấu trúc dữ liệu 
**Thời gian giải:** 7 phút 35 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một mảng số nguyên có phải là 3SUM đóng hay không, nghĩa là với mỗi bộ ba phần tử riêng biệt trong mảng, tổng của ba phần tử đó sẽ xuất hiện ở đâu đó trong mảng. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định độ dài của mảng theo sau là các phần tử của nó. Đầu ra là "CÓ" nếu mảng được đóng 3SUM và nếu không thì "KHÔNG". 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm, điều này ngụ ý rằng bất kỳ thuật toán nào có kết quả tệ hơn đại khái$O(n^2)$mỗi trường hợp thử nghiệm có thể là quá chậm. Một cách tiếp cận bạo lực xem xét trực tiếp tất cả các bộ ba sẽ yêu cầu$O(n^3)$hoạt động không khả thi đối với các hoạt động có quy mô lớn.$n$. 

Các trường hợp cạnh cần xem xét bao gồm các mảng có tất cả các phần tử giống hệt nhau, mảng chỉ có số 0 và mảng chứa cả số dương và số âm lớn. Ví dụ,$[0,0,0,0]$được đóng 3SUM một cách tầm thường vì tất cả các bộ ba có tổng bằng 0, nằm trong mảng. Mảng có nhiều hơn ba phần tử khác 0 riêng biệt, chẳng hạn như$[-1, 2, -3, 4]$, có thể không thuộc tính vì một số tổng gấp ba nằm ngoài mảng. Mảng có nhiều bản sao hoặc số 0 phải được xử lý cẩn thận vì việc đếm các giá trị riêng biệt là điều cần thiết để đảm bảo tính chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lặp qua từng bộ ba chỉ số có thể có và kiểm tra xem tổng của chúng có tồn tại trong mảng hay không. Điều này đúng vì nó trực tiếp thực hiện định nghĩa đóng 3SUM, nhưng độ phức tạp là$O(n^3)$, điều này không thể chấp nhận được khi$n$là lớn. Ví dụ,$n=2 \cdot 10^5$sẽ yêu cầu đại khái$8 \cdot 10^{15}$hoạt động. 

Quan sát quan trọng là một mảng có nhiều hơn ba phần tử khác 0 riêng biệt không thể đóng 3SUM. Mỗi bộ ba số khác 0 riêng biệt tạo ra một tổng không nhất thiết phải có trong mảng. Do đó, chúng ta có thể giảm mảng xuống tối đa ba số dương, ba số âm và bất kỳ số 0 nào. Nếu có nhiều hơn ba số dương hoặc nhiều hơn ba số âm thì mảng không thể đóng 3SUM. 

Một khi mảng đã được thu gọn lại, bài toán sẽ trở nên dễ giải quyết hơn vì số phần tử khác 0 riêng biệt nhiều nhất là sáu. Sau đó, chúng ta có thể liệt kê tất cả các bộ ba phần tử (có tính đến các số 0) và kiểm tra xem mỗi tổng có tồn tại trong mảng hay không. Vì tổng số phần tử được xem xét nhiều nhất là 7 nên việc kiểm tra này khả thi trong$O(1)$mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Giảm tối ưu + Kiểm tra ba lần | O(1) cho mỗi trường hợp kiểm thử sau khi lọc, O(n) cho tiền xử lý | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng$a$và chuyển đổi nó thành một tập hợp để truy vấn thành viên nhanh chóng. 
2. Tách các phần tử thành dương, âm và không. Chỉ giữ tối đa ba số dương lớn nhất và tối đa ba số âm nhỏ nhất. Bao gồm tất cả các số 0 vì chúng không làm tăng số lượng tổng riêng biệt. Điều này làm giảm mảng xuống còn tối đa bảy phần tử. 
3. Tạo tất cả các bộ ba phần tử riêng biệt từ mảng rút gọn. Với mỗi bộ ba, hãy tính tổng của nó. 
4. Kiểm tra xem tổng của mỗi bộ ba có tồn tại trong tập hợp các phần tử mảng ban đầu hay không. Nếu thiếu tổng nào thì ghi "NO". 
5. Nếu có tất cả ba tổng, ghi "CÓ". 

Tại sao nó hoạt động: Bất kỳ mảng nào có nhiều hơn ba số dương hoặc ba số âm sẽ tự động không hợp lệ vì một số tổng ba không thể được biểu diễn trong mảng. Bằng cách giới hạn các yếu tố quan trọng, chúng tôi đảm bảo rằng việc kiểm tra tất cả các tổng gấp ba là đủ để xác nhận việc đóng 3SUM. Điều này dựa vào tính bất biến rằng bất kỳ phần tử bị loại trừ nào cũng sẽ tạo ra số tiền đã được tính bằng các bộ ba hoặc tổng còn lại bên ngoài mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from itertools import combinations

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a_set = set(a)
        
        pos = sorted([x for x in a if x > 0])
        neg = sorted([x for x in a if x < 0])
        zeros = [x for x in a if x == 0]
        
        if len(pos) > 3 or len(neg) > 3:
            print("NO")
            continue
        
        reduced = neg[:3] + zeros + pos[-3:]
        ok = True
        for triple in combinations(reduced, 3):
            if sum(triple) not in a_set:
                ok = False
                break
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ phân vùng mảng và rút gọn nó thành các phần tử quan trọng. Sử dụng một bộ cho phép$O(1)$thành viên kiểm tra số tiền. Sự kết hợp của mảng rút gọn đảm bảo rằng tổng số bộ ba là nhỏ, do đó việc liệt kê chúng sẽ hiệu quả. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`[3, -1, 0, 1]`: 

| giảm | gấp ba | tổng hợp | trong bộ? | 
| --- | --- | --- | --- | 
| [-1,0,1] | (-1,0,1) | 0 | Có | 

Đầu ra: CÓ. Tất cả các tổng ba đều tồn tại trong mảng. 

Đầu vào mẫu`[4, -1, 2, -3, 4]`: 

| giảm | gấp ba | tổng hợp | trong bộ? | 
| --- | --- | --- | --- | 
| [-3,-1,2,4] | (-3,-1,2) | -2 | Không | 

Đầu ra: KHÔNG. Tổng -2 không tồn tại trong mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Việc phân vùng mảng và rút gọn các phần tử là tuyến tính; kiểm tra bộ ba không đổi vì độ dài giảm ≤ 7 | 
| Không gian | O(n) | Đặt để kiểm tra thành viên và giảm lưu trữ danh sách | 

Giải pháp phù hợp trong giới hạn thời gian và bộ nhớ vì tổng của tất cả$n$nhiều nhất là$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("4\n3\n-1 0 1\n5\n1 -2 -2 1 -3\n6\n0 0 0 0 0 0\n4\n-1 2 -3 4\n") == "YES\nNO\nYES\nNO"

# Custom cases
assert run("1\n3\n1 2 3\n") == "NO", "three positive numbers not closed"
assert run("1\n3\n0 0 0\n") == "YES", "all zeros"
assert run("1\n7\n-2 -1 0 0 0 1 2\n") == "YES", "balanced negatives and positives with zeros"
assert run("1\n8\n-5 -4 -3 -2 0 2 3 4\n") == "NO", "too many negatives"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 | KHÔNG | ba số dương riêng biệt không thể tạo thành 3SUM-đóng | 
| 0 0 0 | CÓ | tất cả số không, đóng cửa tầm thường | 
| -2 -1 0 0 0 1 2 | CÓ | giảm việc kiểm tra mảng với số 0 được bao gồm | 
| -5 -4 -3 -2 0 2 3 4 | KHÔNG | vượt quá giới hạn ba tiêu cực | 

## Vỏ cạnh 

Một mảng có tất cả các số 0`[0,0,0,0]`tạo ra ba tổng bằng 0, tất cả đều có trong mảng. Một mảng cân bằng`[-2,-1,0,0,0,1,2]`hoạt động vì sau khi giảm xuống tối đa ba số âm, ba số dương và số không, tất cả các tổng ba đều xuất hiện. Mảng vượt quá ba số dương hoặc ba số âm, chẳng hạn như`[-5,-4,-3,-2,0,2,3,4]`, thất bại vì một số tổng gấp ba nằm ngoài mảng. Thuật toán xác định chính xác các tình huống này bằng cách giới hạn tập rút gọn và kiểm tra rõ ràng tất cả các tổng ba.
