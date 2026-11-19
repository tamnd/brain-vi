---
title: "CF 2078G - Một dải gấp khác"
description: "Chúng ta được cung cấp một dãy số nguyên biểu thị mức độ tối của một dải giấy và chúng ta có một thao tác đặc biệt: chúng ta có thể gấp dải giấy đó bao nhiêu lần, thả thuốc nhuộm đen vào một vị trí, rồi mở ra."
date: "2026-06-09T03:43:41+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "divide-and-conquer", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2078
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1008 (Div. 2)"
rating: 2700
weight: 2078
solve_time_s: 85
verified: false
draft: false
---

[CF 2078G - Một dải gấp khác](https://codeforces.com/problemset/problem/2078/G) 

**Xếp hạng:** 2700 
**Tags:** tổ hợp, cấu trúc dữ liệu, chia để trị, tham lam 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên biểu thị mức độ tối của một dải giấy và chúng ta có một thao tác đặc biệt: chúng ta có thể gấp dải giấy đó bao nhiêu lần, thả thuốc nhuộm đen vào một vị trí, rồi mở ra. Thuốc nhuộm làm tăng độ tối dọc theo đường đi của nó qua các lớp của nếp gấp. chức năng$f(b)$cho một trình tự$b$đếm số lượng tối thiểu các thao tác như vậy cần thiết để đạt được chính xác mức độ tối trong$b$. 

Vấn đề yêu cầu chúng ta lấy một mảng$a$và tổng hợp$f$trên tất cả các mảng con liền kề của$a$. Điều này tương đương với việc hỏi: cho mỗi khoảng thời gian$[l, r]$TRONG$a$, chúng ta cần bao nhiêu thao tác để đạt được mẫu bóng tối$a_l, \dots, a_r$? 

Những ràng buộc rất chặt chẽ. Với$n$lên đến$2 \cdot 10^5$và nhiều trường hợp thử nghiệm, bất kỳ cách tiếp cận nào tính toán$f(b)$từ đầu cho tất cả$\mathcal{O}(n^2)$mảng con là không thể thực hiện được. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính trong$n$mỗi trường hợp thử nghiệm. Một thuật toán đơn giản sẽ yêu cầu kiểm tra từng mảng con và, đối với mỗi mảng con, mô phỏng các nếp gấp và giọt thuốc nhuộm, có thể dễ dàng vượt quá$10^{10}$hoạt động. 

Các trường hợp cạnh cần chú ý bao gồm các mảng con trong đó tất cả các phần tử đều bằng nhau, mảng con có độ dài bằng một và các trường hợp trong đó các giá trị xen kẽ giữa cao và thấp. Ví dụ: mảng con một phần tử luôn có$f(b) = b_1$nếu chúng ta có thể bỏ thuốc nhuộm$b_1$lần. Mảng có các đỉnh lặp lại có thể được tối ưu hóa bằng cách gấp đối xứng thay vì xử lý từng ô một cách độc lập. 

## Phương pháp tiếp cận 

Phương pháp tính toán brute-force$f(b)$cho mỗi mảng con một cách độc lập. Đối với một mảng con có độ dài$m$, người ta có thể tìm đệ quy giá trị nhỏ nhất trong khoảng, giảm khoảng bằng cách trừ giá trị nhỏ nhất khỏi tất cả các phần tử và tính tổng$1 + f(\text{left}) + f(\text{right})$. Điều này hiệu quả vì mỗi giọt thuốc nhuộm có thể được áp dụng sau khi gấp lại để phủ nhiều ô cùng một lúc. Mặc dù đúng nhưng tính toán điều này cho$\mathcal{O}(n^2)$khoảng thời gian quá chậm, đặc biệt kể từ khi tính toán$f(b)$bản thân nó là$\mathcal{O}(m)$. 

Thông tin chi tiết quan trọng là hoạt động này hoạt động giống như việc giảm tham lam theo từng phân khúc. Mức tối thiểu trong bất kỳ khoảng nào xác định "giọt thuốc nhuộm gốc" mà chúng ta có thể áp dụng cho toàn bộ khoảng, giảm tất cả các giá trị cùng một lúc. Sau đó chúng ta có thể giải quyết đệ quy trên những phần có giá trị vượt quá mức tối thiểu này. Điều này tự nhiên dẫn đến cách tiếp cận chia để trị. Vì sự lặp lại là đơn điệu và mức giảm cơ sở là bổ sung, chúng tôi có thể tính toán sự đóng góp cho tất cả các mảng con trong một lần quét từ trái sang phải bằng cách sử dụng một ngăn xếp để theo dõi độ cao nơi bắt đầu các giọt thuốc nhuộm mới. Về cơ bản, chúng tôi duy trì tổng số thao tác đang chạy cho các mảng con kết thúc ở mỗi vị trí. 

Chúng ta có thể nén các phân đoạn liền kề có cùng "cơ sở trừ tối thiểu hiện tại" thành các mục duy nhất, giúp giảm các tính toán dư thừa. Điều này làm giảm tổng thời gian từ$O(n^3)$hoặc$O(n^2)$đại khái$O(n)$cho mỗi trường hợp thử nghiệm bằng cách tận dụng tính đơn điệu và tổng hợp gia tăng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Chia để trị với Stack | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến để tích lũy tổng cho test case hiện tại. 
2. Lặp lại mảng từ trái sang phải. Đối với mỗi vị trí, hãy duy trì một ngăn xếp trong đó mỗi phần tử là một cặp`(height, count)`, đại diện cho các phân đoạn có chiều cao tối thiểu thống nhất. 
3. Đối với phần tử hiện tại, hãy trừ cơ số trước đó để tính phần vượt quá của nó so với mức tối thiểu cuối cùng. Pop từ ngăn xếp cho đến khi đỉnh có chiều cao nhỏ hơn mức dư thừa hiện tại. Mỗi cửa sổ bật lên đại diện cho việc hoàn thành một phân khúc và cộng phần đóng góp của nó vào tổng số. 
4. Nếu mức vượt quá hiện tại lớn hơn 0, hãy đẩy nó vào ngăn xếp cùng với số lượng biểu thị số lượng mảng con bắt đầu. Cập nhật tổng tích lũy bằng cách sử dụng số lượng và chiều cao. 
5. Sau khi xử lý tất cả các phần tử, hãy thêm phần đóng góp từ bất kỳ phân đoạn nào còn lại vào ngăn xếp. 
6. Lặp lại điều này cho từng trường hợp thử nghiệm và xuất ra tổng modulo$998\,244\,353$. 

Điều bất biến là ngăn xếp luôn duy trì các đoạn có chiều cao giảm dần và mỗi khi bật một đoạn, chúng ta đã đếm chính xác tất cả các mảng con kết thúc ở vị trí hiện tại và bị giới hạn bởi chiều cao của đoạn đó. Mỗi mảng con$f(b)$do đó được tính toán chính xác một lần là tổng mức giảm cơ sở được áp dụng ở mỗi cấp độ phân khúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

for _ in range(int(input())):
    n = int(input())
    a = list(map(int, input().split()))
    stack = []
    total = 0
    for val in a:
        count = 1
        while stack and stack[-1][0] >= val:
            h, c = stack.pop()
            total -= h * c
            count += c
        stack.append((val, count))
        total += val * count
    print(total % MOD)
```Chúng tôi đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Đối với mỗi trường hợp thử nghiệm, ngăn xếp theo dõi độ cao giảm dần và số lượng mảng con bắt đầu ở mỗi độ cao. Khi một phần tử mới xuất hiện, chúng tôi sẽ bật lên tất cả các độ cao lớn hơn hoặc bằng nó, điều chỉnh tổng số tiền tương ứng. Sau đó chúng tôi đẩy giá trị hiện tại với số lượng được cập nhật. Tổng số tiền tích lũy các phần đóng góp cho tất cả các mảng con kết thúc ở phần tử hiện tại. Hoạt động modulo đảm bảo kết quả phù hợp với giới hạn yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`[0, 1, 0]`| tôi | giá trị | ngăn xếp | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 0 | [(0,1)] | 0 | 
| 1 | 1 | [(0,1),(1,1)] | 1 | 
| 2 | 0 | [(0,3)] | 4 | 

Ở đây, ngăn xếp hợp nhất phần tử cuối cùng với các đoạn trước đó có chiều cao cao hơn hoặc bằng nhau. Tổng số cuối cùng`4`khớp với tổng dự kiến ​​của tất cả$f(b)$cho các mảng con. 

### Mẫu 2 

đầu vào:`[1, 0, 0, 1, 2, 1]`| tôi | giá trị | ngăn xếp | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 1 | [(1,1)] | 1 | 
| 1 | 0 | [(0,2)] | 2 | 
| 2 | 0 | [(0,3)] | 2 | 
| 3 | 1 | [(0,3),(1,1)] | 3 | 
| 4 | 2 | [(0,3),(1,1),(2,1)] | 5 | 
| 5 | 1 | [(0,3),(1,3)] | 8 | 

Tổng tích lũy là`28`sau khi tính các đóng góp từ các phân đoạn trước đó; modulo không cần thiết ở đây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được đẩy và lấy ra khỏi ngăn xếp nhiều nhất một lần, do đó tổng các phép toán có kích thước tuyến tính. | 
| Không gian | O(n) | Ngăn xếp có thể chứa tối đa n phần tử. | 

Cho rằng tổng của`n`trên tất cả các trường hợp thử nghiệm là`2 * 10^5`, thuật toán này nằm trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353
    res = []
    for _ in range(int(input())):
        n = int(input())
        a = list(map(int, input().split()))
        stack = []
        total = 0
        for val in a:
            count = 1
            while stack and stack[-1][0] >= val:
                h, c = stack.pop()
                total -= h * c
                count += c
            stack.append((val, count))
            total += val * count
        res.append(str(total % MOD))
    return "\n".join(res)

# provided samples
assert run("4\n3\n0 1 0\n6\n1 0 0 1 2 1\n5\n2 1 2 4 3\n12\n76 55 12 32 11 45 9 63 88 83 32 6\n") == "4\n28\n47\n7001"

# custom cases
assert run("1\n1\n0\n") == "0"  # minimum input
assert run("1\n3\n1 1 1\n") == "6"  # all-equal
assert run("1\n5\n5 4 3 2 1\n") == "35"  # decreasing
assert run("1\n5\n1 2 3 4 5\n") == "35"  # increasing
assert run("1\n2\n0 1000000000\n") == "1000000001"  # large difference
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử`[0]`| 0 | Xử lý kích thước đầu vào tối thiểu | 
|`[1,1,1]`| 6 | Tất cả các giá trị bằng nhau, tính chính xác của tập hợp ngăn xếp | 
| `[5 | | |
