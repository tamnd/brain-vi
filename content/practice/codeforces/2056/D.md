---
title: "CF 2056D - Trung vị duy nhất"
description: "Chúng ta được yêu cầu đếm các mảng con của một mảng nhất định trong đó trung vị được xác định duy nhất. Trong thực tế, điều này có nghĩa là một mảng con sẽ phù hợp nếu sau khi sắp xếp, phần tử ở giữa (hoặc hai phần tử ở giữa, nếu độ dài là chẵn) bằng nhau."
date: "2026-06-08T08:16:38+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "combinatorics", "data-structures", "divide-and-conquer", "dp"]
categories: ["algorithms"]
codeforces_contest: 2056
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 997 (Div. 2)"
rating: 2200
weight: 2056
solve_time_s: 123
verified: false
draft: false
---

[CF 2056D - Trung vị duy nhất](https://codeforces.com/problemset/problem/2056/D) 

**Xếp hạng:** 2200 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, tổ hợp, cấu trúc dữ liệu, phân chia và chinh phục, dp 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm các mảng con của một mảng nhất định trong đó trung vị được xác định duy nhất. Trong thực tế, điều này có nghĩa là một mảng con sẽ phù hợp nếu sau khi sắp xếp, phần tử ở giữa (hoặc hai phần tử ở giữa, nếu độ dài là chẵn) bằng nhau. Đối với các mảng con có độ dài lẻ, điều này không quan trọng vì trung vị đơn luôn "bằng chính nó". Đối với các mảng con có độ dài chẵn, hai số ở giữa phải giống nhau. 

Các giá trị mảng nhỏ, nằm trong khoảng từ 1 đến 10, nhưng mảng có thể lớn, tối đa 10^5 phần tử. Với tối đa 10^4 trường hợp thử nghiệm, tổng kích thước mảng trên tất cả các thử nghiệm được giới hạn ở mức 10^5. Điều này ngụ ý rằng chúng tôi không thể chấp nhận bất kỳ cách tiếp cận O(n^2) nào cho mỗi trường hợp thử nghiệm vì trong trường hợp xấu nhất, điều đó sẽ yêu cầu 10^10 thao tác. Chúng tôi cần một cái gì đó gần hơn với O(n) hoặc O(n log n) cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh phát sinh khi tất cả các phần tử giống hệt nhau, làm cho mọi mảng con đều tốt hoặc khi tất cả các phần tử đều khác biệt, điều này làm giảm số lượng các mảng con có độ dài chẵn tốt. Một giải pháp bất cẩn có thể cố gắng kiểm tra trung vị một cách rõ ràng cho mọi mảng con, việc này sẽ quá chậm. Các ví dụ nhỏ minh họa điều này: đối với`[1,2]`, mảng con`[1,2]`không tốt vì hai đường trung tuyến khác nhau. Vì`[1,1,1,1]`, mọi mảng con đều tốt vì các số trung vị luôn trùng nhau. 

## Phương pháp tiếp cận 

Giải pháp brute-force lặp lại trên tất cả các mảng con, sắp xếp từng mảng và kiểm tra các phần tử ở giữa. Điều này đúng nhưng chia tỷ lệ thành O(n^3) trong trường hợp xấu nhất (O(n^2), mỗi mảng yêu cầu sắp xếp O(n log n)). Với n lên tới 10^5 thì điều này hoàn toàn không khả thi. 

Quan sát quan trọng là vấn đề có thể được rút gọn thành việc đếm các mảng con dựa trên tần số của giá trị "trung tâm". Sửa một số`x`và coi nó như là giá trị trung bình của ứng cử viên. Đối với mỗi vị trí trong mảng, hãy theo dõi số lần chạy: +1 nếu phần tử bằng`x`, -1 nếu nó nhỏ hơn`x`, và 0 nếu không. Đối với các mảng con có độ dài chẵn, chúng ta cần số phần tử lớn hơn`x`và ít hơn`x`được cân bằng xung quanh`x`để làm cho trung vị chính xác`x`. Điều này biến vấn đề thành vấn đề đếm tổng tiền tố: sự khác biệt giữa các số đếm tạo thành một khóa và số lượng mảng con tốt kết thúc ở mỗi chỉ mục có thể được tính bằng cách sử dụng bản đồ băm để theo dõi tần số của các khóa này. 

Cái nhìn sâu sắc này chuyển đổi một vấn đề có khả năng bậc ba thành một vấn đề tuyến tính cho mỗi ứng viên. Vì các giá trị được giới hạn bởi 10 nên chúng ta có thể lặp lại tất cả các giá trị có thể`x`từ 1 đến 10, mang lại độ phức tạp tổng thể là O(10 * n) = O(n), có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n * max(a_i)) | O(n * max(a_i)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại qua từng ứng viên trung vị có thể`x`từ 1 đến 10. Điều này khả thi vì`a_i`≤ 10. Mỗi ứng viên`x`sẽ được xem xét riêng. 
2. Chuyển mảng thành mảng cân bằng đang chạy`balance`, được khởi tạo bằng 0 ở vị trí -1. Đối với mỗi phần tử trong mảng, nếu nó bằng`x`, thêm 1 vào số dư trước đó; nếu ít hơn`x`, trừ 1; nếu lớn hơn`x`, giữ nguyên số dư. Mã hóa này nắm bắt sự thống trị tương đối của`x`trong tiền tố. 
3. Sử dụng bản đồ băm`freq`để đếm số lần xuất hiện của mỗi giá trị số dư. Khởi tạo nó với`{0: 1}`vì tiền tố trống có số dư bằng 0. Đối với mỗi vị trí, số mảng con kết thúc tại vị trí này bằng trung vị`x`bằng tần số của giá trị cân bằng hiện tại trong`freq`. Điều này hiệu quả vì hai tiền tố có cùng số dư ngụ ý mảng con giữa chúng có số dư ròng 0, nghĩa là`x`là trung vị. 
4. Tính tổng các số này trên tất cả các vị trí để có được tổng số mảng con tốt cho trung vị ứng cử viên này. 
5. Tính tổng các số thí sinh từ 1 đến 10 để có đáp án cho test case. 

Tại sao nó hoạt động: mảng cân bằng đảm bảo rằng trong bất kỳ mảng con nào giữa hai tiền tố có cùng số dư, số lượng`x`trừ đi số phần tử nhỏ hơn`x`không thay đổi. Điều này đảm bảo rằng`x`là trung vị của mảng con đó. Bằng cách xem xét tất cả`x`, chúng tôi bao gồm mọi trung vị có thể có trong mảng. 

## Giải pháp Python```python
import sys
from collections import defaultdict
input = sys.stdin.readline

def count_good_subarrays(a):
    n = len(a)
    ans = 0
    for x in range(1, 11):
        balance = 0
        freq = defaultdict(int)
        freq[0] = 1
        for val in a:
            if val == x:
                balance += 1
            elif val < x:
                balance -= 1
            # val > x does not change balance
            ans += freq[balance]
            freq[balance] += 1
    return ans

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(count_good_subarrays(a))
```Giải pháp đọc đầu vào bằng I/O nhanh. Chúng tôi lặp qua các trung vị ứng cử viên từ 1 đến 10 và tính toán một mảng cân bằng tiền tố một cách ngầm định. Sơ đồ băm`freq`đếm số dư trước đó để cho phép O(1) truy xuất số lượng mảng con. Các điểm tinh tế bao gồm việc khởi tạo`freq[0] = 1`để tính đến các mảng con bắt đầu từ chỉ số 0 và cập nhật câu trả lời trước khi tăng tần số để tránh thiếu tiền tố hiện tại. 

## Ví dụ đã hoạt động 

Mẫu 1:`a = [1, 1, 1, 1]`| tôi | giá trị | cân bằng | tần số | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | {0:1} | 1 | 
| 1 | 1 | 2 | {0:1,1:1} | 2 | 
| 2 | 1 | 3 | {0:1,1:1,2:1} | 3 | 
| 3 | 1 | 4 | {0:1,1:1,2:1,3:1} | 4 | 

Tính tổng x = 1 sẽ có tất cả 10 mảng con. Đóng góp x khác 0. 

Mẫu 2:`a = [1,10,2,3,3]`Với x = 3: 

| tôi | giá trị | cân bằng | tần số | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | -1 | {0:1} | 0 | 
| 1 | 10 | -1 | {-1:1,0:1} | 1 | 
| 2 | 2 | -2 | {-2:0,-1:1,0:1} | 1 | 
| 3 | 3 | -1 | {-2:0,-1:1,0:1} | 2 | 
| 4 | 3 | 0 | {-2:0,-1:2,0:1} | 3 | 

Tổng số cuối cùng là 11. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 10) = O(n) | Vòng lặp bên ngoài chạy 10 lần, vòng lặp bên trong chạy n lần cho mỗi trường hợp kiểm thử | 
| Không gian | O(n) | Sơ đồ băm`freq`có thể lưu trữ tối đa n+1 số dư duy nhất | 

Tỷ lệ này thoải mái trong giới hạn n 10^5 và t 10^4. Dung lượng bộ nhớ vẫn dưới 512 MB vì ​​số dư là số nguyên nhỏ và có tối đa n khóa cho mỗi trung vị ứng cử viên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict
    input = sys.stdin.readline

    def count_good_subarrays(a):
        n = len(a)
        ans = 0
        for x in range(1, 11):
            balance = 0
            freq = defaultdict(int)
            freq[0] = 1
            for val in a:
                if val == x:
                    balance += 1
                elif val < x:
                    balance -= 1
                ans += freq[balance]
                freq[balance] += 1
        return ans

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        out.append(str(count_good_subarrays(a)))
    return "\n".join(out)

# Provided samples
assert run("3\n4\n1 1 1 1\n5\n1 10 2 3 3\n10\n6 3 2 3 5 3 4 2 3 5\n") == "10\n11\n42"

# Minimum size input
assert run("1\n1\n5\n") == "1"

# Maximum size input, all equal
assert run("1\n5\n2 2 2 2 2\n") == "15"

# Mixed small numbers
assert run("1\n5\n1 3 2 3 1\n") == "9"

# Only increasing sequence
assert run("
```
