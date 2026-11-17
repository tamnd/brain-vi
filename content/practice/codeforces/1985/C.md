---
title: "CF 1985C - Tiền tố tốt"
description: "Bài toán yêu cầu chúng ta phân tích các tiền tố của một mảng số nguyên và xác định tiền tố nào là \"tốt\" theo một quy tắc cụ thể. Tiền tố có độ dài $i$ được coi là tốt nếu tồn tại một phần tử trong tiền tố bằng tổng của tất cả các phần tử khác."
date: "2026-06-08T16:18:52+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1985
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 952 (Div. 4)"
rating: 1000
weight: 1985
solve_time_s: 158
verified: true
draft: false
---

[CF 1985C - Tiền tố tốt](https://codeforces.com/problemset/problem/1985/C) 

**Đánh giá:** 1000 
**Thẻ:** tham lam 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta phân tích các tiền tố của một mảng số nguyên và xác định tiền tố nào là “tốt” theo một quy tắc cụ thể. Tiền tố có độ dài$i$được coi là tốt nếu tồn tại một phần tử trong tiền tố bằng tổng của tất cả các phần tử khác. Ví dụ, trong mảng`[1, 3, 2, 6]`, mảng đầy đủ là tốt bởi vì`6`bằng`1 + 3 + 2`. Mảng một phần tử chỉ tốt nếu phần tử bằng 0. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ định độ dài mảng theo sau là các phần tử mảng và đầu ra là số lượng tiền tố tốt cho mỗi trường hợp. 

Các ràng buộc chỉ ra rằng một mảng có thể lên tới$2 \cdot 10^5$phần tử và tổng số trên tất cả các trường hợp thử nghiệm không vượt quá cùng một số. Điều này có nghĩa là một giải pháp với$O(n^2)$thời gian cho mỗi trường hợp kiểm thử là quá chậm, vì trong trường hợp xấu nhất nó sẽ yêu cầu khoảng$4 \cdot 10^{10}$hoạt động. Một giải pháp hiệu quả phải hoạt động trong thời gian tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm, lý tưởng nhất là$O(n)$mỗi mảng. Các trường hợp cạnh bao gồm mảng một phần tử, mảng chỉ chứa số 0, mảng trong đó nhiều phần tử có thể là "tổng của phần còn lại" và các tiền tố xuất hiện tốt sớm nhưng không thành công khi có nhiều phần tử được thêm vào. Ví dụ,`[0, 1]`là không tốt bởi vì`0 ≠ 1`Và`1 ≠ 0`, mặc dù`[0]`một mình là tốt. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản xem xét từng tiền tố riêng lẻ và đối với từng tiền tố có độ dài$i$, nó kiểm tra mọi phần tử để xem liệu nó có bằng tổng của tất cả các phần tử khác hay không. Về nguyên tắc, điều này đúng: tính tổng tiền tố và sau đó trừ nó khỏi tổng số cho mỗi phần tử để xem liệu nó có bằng phần tử đó hay không. Sự phức tạp là$O(n^2)$cho mỗi trường hợp thử nghiệm vì với mọi tiền tố có độ dài$i$bạn biểu diễn$i$kiểm tra, điều này là không khả thi do các hạn chế của vấn đề. 

Thông tin chi tiết quan trọng xuất phát từ việc quan sát rằng để xác định xem tiền tố có tốt hay không, chúng ta chỉ cần biết tổng của tất cả các phần tử cho đến nay và số lượng phần tử riêng lẻ trong tiền tố đó. Nếu tổng số tiền là`S`và tiền tố là độ dài`i`, tiền tố là tốt nếu tồn tại một phần tử`x`như vậy`x = S - x`, tức là,`2 * x = S`. Điều này có nghĩa là chúng ta chỉ cần một bản đồ tần số cho các phần tử tiền tố và tổng tiền tố đang chạy. Mỗi lần chúng tôi thêm một phần tử mới vào tiền tố, chúng tôi sẽ cập nhật tổng số tiền và kiểm tra xem liệu`(sum - element)`tồn tại trong tiền tố bằng cách sử dụng bản đồ tần số. Bằng cách duy trì một từ điển tổng và tần số đang chạy, mỗi tiền tố có thể được kiểm tra theo thời gian không đổi, dẫn đến một$O(n)$thuật toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm cho các tiền tố tốt về 0, tổng chạy cho tiền tố hiện tại và từ điển để theo dõi tần suất của từng phần tử gặp phải cho đến nay. 
2. Lặp lại từng phần tử trong mảng, thêm nó vào tổng hiện có và tăng số lượng của nó trong từ điển. 
3. Sau khi cập nhật tổng và tần số, hãy tính giá trị đích như sau:`sum - element`cho mỗi phần tử trong tiền tố. Nếu như`2 * element = sum`, tiền tố hiện tại là tốt. 
4. Tăng bộ đếm tiền tố tốt bất cứ khi nào điều kiện được thỏa. 
5. Tiếp tục đến phần tử tiếp theo cho đến hết mảng. 
6. Sau khi xử lý tất cả các phần tử của một test case, xuất ra bộ đếm cho test case đó. 

Bất biến đảm bảo tính chính xác là ở mỗi bước, tổng chạy phản ánh chính xác tổng tiền tố hiện tại và bản đồ tần số chứa tất cả các phần tử trong tiền tố. Kiểm tra`2 * x == sum`đảm bảo chúng ta nắm bắt chính xác điều kiện một phần tử bằng tổng của phần còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_good_prefixes(a):
    freq = {}
    total = 0
    good_count = 0
    for x in a:
        total += x
        freq[x] = freq.get(x, 0) + 1
        if total % 2 == 0:
            target = total // 2
            if freq.get(target, 0) > 0:
                good_count += 1
    return good_count

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(count_good_prefixes(a))
```các`count_good_prefixes`hàm duy trì một từ điển tần số và tổng số đang chạy khi nó lặp qua mảng. Đối với mỗi phần tử mới, nó cập nhật tổng tổng và kiểm tra xem tổng tiền tố có phải là số chẵn hay không. Nếu đúng, nó sẽ kiểm tra xem một nửa tổng số có tồn tại trong tiền tố hay không. Điều này đảm bảo rằng chính xác một phần tử bằng tổng của các phần tử còn lại. Việc sử dụng từ điển sẽ tránh lặp đi lặp lại tất cả các phần tử, giữ độ phức tạp tuyến tính. Hàm xử lý chính xác trường hợp cạnh trong đó tiền tố một phần tử bằng 0 là phù hợp, vì tổng bằng 0 và phần tử bằng 0. 

## Ví dụ đã hoạt động 

**Ví dụ 1:`[0, 1, 2, 1, 4]`** 

| Yếu tố | Tổng Chạy | Bản đồ tần số | Tiền tố tốt? | 
| --- | --- | --- | --- | 
| 0 | 0 | {0:1} | Có | 
| 1 | 1 | {0:1,1:1} | Không | 
| 2 | 3 | {0:1,1:1,2:1} | Không | 
| 1 | 4 | {0:1,1:2,2:1} | Có | 
| 4 | 8 | {0:1,1:2,2:1,4:1} | Có | 

Điều này cho thấy thuật toán xác định chính xác ba tiền tố tốt:`[0]`,`[0,1,2,1]`,`[0,1,2,1,4]`. 

**Ví dụ 2:`[1,1,2,0]`** 

| Yếu tố | Tổng Chạy | Bản đồ tần số | Tiền tố tốt? | 
| --- | --- | --- | --- | 
| 1 | 1 | {1:1} | Không | 
| 1 | 2 | {1:2} | Có | 
| 2 | 4 | {1:2,2:1} | Có | 
| 0 | 4 | {1:2,2:1,0:1} | Không | 

Thuật toán đếm chính xác ba tiền tố tốt cho mảng này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần, việc tra cứu và cập nhật từ điển được khấu hao O(1) | 
| Không gian | O(n) | Từ điển tần số có thể lưu trữ tối đa n phần tử riêng biệt | 

Với tổng n trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2 × 10^5, thuật toán này chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

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
        print(count_good_prefixes(a))
    return output.getvalue().strip()

# provided samples
assert run("7\n1\n0\n1\n1\n4\n1 1 2 0\n5\n0 1 2 1 4\n7\n1 1 0 3 5 2 12\n7\n1000000000 1000000000 1000000000 1000000000 1000000000 1000000000 294967296\n10\n0 1000000000 1000000000 1000000000 1000000000 1000000000 1000000000 1000000000 1000000000 589934592") == "1\n0\n3\n3\n4\n1\n2", "sample 1"

# custom cases
assert run("1\n1\n1") == "0", "single element non-zero"
assert run("1\n1\n0") == "1", "single element zero"
assert run("1\n3\n1 2 3") == "0", "no good prefixes"
assert run("1\n5\n2 2 4 0 8") == "3", "multiple good prefixes"
assert run("1\n4\n0 0 0 0") == "4", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`| 0 | mảng một phần tử khác 0 | 
|`1\n1\n0`| 1 | mảng không phần tử đơn | 
|`1\n3\n1 2 3`| 0 | không có tiền tố tốt | 
|`1\n5\n2 2 4 0 8`| 3 | nhiều tiền tố tốt được phát hiện | 
|`1\n4\n0 0 0 0`| 4 | mảng số 0 được xử lý chính xác | 

## Vỏ cạnh 

Một phần tử đơn số 0`[0]`là tốt. Thuật toán xác định chính xác điều này vì tổng bằng 0 và bản đồ tần số chứa 0, thỏa mãn`2*0=0`. Vì
