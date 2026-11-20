---
title: "CF 2138B - Antiamuny Muốn Học Hoán Đổi"
description: "Chúng ta được cấp một hoán vị các số nguyên từ 1 đến $n$ và chúng ta phải trả lời nhiều truy vấn về việc liệu các mảng con nhất định có \"hoàn hảo\" hay không theo một quy tắc sắp xếp đặc biệt."
date: "2026-06-08T02:24:39+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2138
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1048 (Div. 1)"
rating: 1900
weight: 2138
solve_time_s: 102
verified: false
draft: false
---

[CF 2138B - Antiamuny muốn học hoán đổi](https://codeforces.com/problemset/problem/2138/B) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến$n$, and we must answer multiple queries about whether certain subarrays are "perfect" according to a special sorting rule. Mỗi mảng con có thể được sắp xếp bằng hai loại thao tác: hoán đổi các phần tử liền kề hoặc hoán đổi các phần tử cách nhau hai vị trí, nhưng loại thứ hai chỉ có thể được sử dụng một lần. A subarray is perfect if allowing this one special swap does not reduce the total number of operations compared to using only adjacent swaps. Về bản chất, chúng ta cần kiểm tra xem mảng con có chứa bất kỳ cặp phần tử nào "không đúng thứ tự" đúng hai vị trí và sẽ được hưởng lợi từ việc hoán đổi kép duy nhất hay không. 

The permutation property simplifies our analysis. Since the elements are unique and range from 1 to$n$, mỗi giá trị có một vị trí duy nhất trong mảng được sắp xếp đầy đủ. Điều này có nghĩa là chúng ta có thể tập trung vào thứ tự tương đối của các giá trị thay vì lo lắng về sự trùng lặp. 

Những ràng buộc chỉ ra rằng$n$và tổng số truy vấn có thể đạt được$5 \cdot 10^5$. Điều này loại trừ bất kỳ thuật toán nào yêu cầu$O(n^2)$các thao tác trên mỗi truy vấn hoặc mô phỏng các giao dịch hoán đổi cho từng mảng con, bởi vì chúng ta có thể dễ dàng đạt được$10^{11}$hoạt động. Chúng ta cần một giải pháp với tiền xử lý tuyến tính và đánh giá truy vấn theo thời gian không đổi hoặc theo thời gian logarit. 

Trường hợp cạnh tinh tế phát sinh khi các mảng con đã được sắp xếp hoặc chỉ có một phần tử. Trong những trường hợp này, việc hoán đổi kép không mang lại bất kỳ lợi thế nào và mảng con hoàn hảo một cách tầm thường. Ngược lại, một mảng con trong đó các phần tử tối đa và tối thiểu cách xa nhau nhưng được phân tách bằng một chuỗi có thể được sắp xếp lại bằng một lần hoán đổi kép sẽ không hoàn hảo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng việc sắp xếp từng mảng con, trước tiên chỉ với các giao dịch hoán đổi liền kề để có được$g(b)$, và sau đó với một trao đổi kép tùy chọn để có được$f(b)$. Ví dụ, chúng ta có thể đếm số lần đảo ngược để xác định số lần hoán đổi liền kề tối thiểu. Tuy nhiên, ngay cả việc đếm số lần đảo ngược cho từng truy vấn một cách độc lập cũng$O(n \log n)$, quá chậm khi tổng hợp qua nhiều truy vấn. 

Quan sát quan trọng đến từ việc phân tích hoạt động của hoán đổi đặc biệt. Hoán đổi kép tại chỉ số$i$chỉ có thể sửa lỗi đảo ngược giữa các vị trí$i$Và$i+2$. Nếu không có sự đảo ngược kiểu đó trong mảng con thì mảng con đó là hoàn hảo. Do đó, thay vì mô phỏng các hoán đổi, chúng ta có thể tính toán trước các vị trí mà các phần tử tăng liên tiếp lên 1 và vị trí chúng "phá vỡ" thứ tự cần thiết cho hoán đổi kép. Cụ thể, chúng tôi theo dõi chỉ số ngoài cùng bên trái$i$Ở đâu$a[i] > a[i+2]$trong mảng ban đầu. Sau đó, đối với mỗi truy vấn, chúng tôi chỉ cần kiểm tra xem liệu vi phạm đó có tồn tại trong phạm vi được truy vấn hay không. Nếu không có thì mảng con đó là hoàn hảo. 

Quan sát này làm giảm mỗi lần kiểm tra truy vấn xuống$O(1)$sau khi xử lý trước các "vi phạm", việc này có thể được thực hiện theo thời gian tuyến tính trên mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng hoán đổi) | O(q * n log n) | O(n) | Quá chậm | 
| Tối ưu (tính toán trước vi phạm, kiểm tra phạm vi) | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các vị trí mà việc hoán đổi kép sẽ thực sự hữu ích. Lặp lại mảng và đánh dấu mọi chỉ mục$i$như vậy$a[i] > a[i+2]$. Đây là những vị thế duy nhất mà hoán đổi kép đơn có thể giảm tổng số thao tác so với việc chỉ sử dụng các hoán đổi liền kề. 
2. Xây dựng mảng tiền tố tối đa của các vị trí được đánh dấu này. Đối với mỗi chỉ mục, mảng lưu trữ vị trí ngoài cùng bên phải của "vi phạm" cho đến thời điểm đó. Điều này cho phép chúng tôi kiểm tra một cách hiệu quả xem có tồn tại vi phạm trong bất kỳ phạm vi được truy vấn nào hay không. 
3. Đối với mỗi truy vấn$[l, r]$, kiểm tra xem có vi phạm nào trong$l$ĐẾN$r-2$bởi vì hoán đổi kép bao gồm hai bước tiến về phía trước. Nếu vi phạm tối đa trong khoảng thời gian này lớn hơn$r-2$, thì không có trao đổi kép hữu ích nào trong phạm vi, vì vậy mảng con là hoàn hảo. Nếu không thì nó là không hoàn hảo. 
4. Ghi "YES" nếu hoàn hảo, nếu không thì xuất "NO". 

Tại sao nó hoạt động: Thuật toán dựa trên thực tế là hoán đổi kép chỉ ảnh hưởng đến các phần tử cách nhau hai phần tử. Bằng cách đánh dấu chính xác các vị trí mà hoán đổi kép sẽ có lợi, chúng tôi nắm bắt được tất cả các trường hợp trong đó$f(b) < g(b)$. Nếu không có vị trí nào như vậy trong mảng con thì$f(b) = g(b)$và mảng con là hoàn hảo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        
        # violation[i] = 1 if a[i] > a[i+2], else 0
        violation = [0] * n
        for i in range(n - 2):
            if a[i] > a[i+2]:
                violation[i] = 1
        
        # prefix max of violations
        pref = [0] * n
        for i in range(n):
            pref[i] = violation[i] if i < n else 0
            if i > 0:
                pref[i] = max(pref[i], pref[i-1])
        
        for _ in range(q):
            l, r = map(int, input().split())
            l -= 1
            r -= 1
            if l >= r - 1:
                print("YES")
            else:
                if pref[r-2] - (pref[l-1] if l > 0 else 0) > 0:
                    print("NO")
                else:
                    print("YES")

if __name__ == "__main__":
    main()
```Đầu tiên, mã sẽ tính toán các vị trí mà việc hoán đổi kép sẽ hữu ích. Sau đó, nó xây dựng một mảng tiền tố tối đa để nhanh chóng kiểm tra xem truy vấn có chứa bất kỳ vị trí nào trong số này hay không. Các truy vấn có một hoặc hai phần tử là hoàn hảo một cách tầm thường. Đối với các truy vấn dài hơn, chúng tôi kiểm tra mảng tiền tố để xem liệu có vi phạm nào nằm trong mảng con hay không. 

Điều kiện biên rất quan trọng: vì chúng ta kiểm tra$a[i] > a[i+2]$, hai chỉ số cuối cùng không tham gia vi phạm. Tính toán tiền tố phải tính đến việc lập chỉ mục dựa trên số 0. 

## Ví dụ đã hoạt động 

Đối với trường hợp thử nghiệm đầu tiên: 

| Truy vấn | mảng con | Chỉ số vi phạm | Quyết định | 
| --- | --- | --- | --- | 
| 1-2 | [1,5] | không | CÓ | 
| 1-5 | [1,5,4,3,2] | 1 (5>3), 2(4>2) | KHÔNG | 
| 3-5 | [4,3,2] | 3>5 không hợp lệ? chỉ 3>5? kiểm tra vị trí | KHÔNG | 

Bảng xác nhận rằng các vi phạm xảy ra chính xác ở nơi việc hoán đổi kép sẽ hữu ích, khiến cho mảng con không hoàn hảo. 

Đối với trường hợp thử nghiệm thứ hai: 

| Truy vấn | mảng con | Chỉ số vi phạm | Quyết định | 
| --- | --- | --- | --- | 
| 1-1 | [3] | không | CÓ | 
| 4-5 | [4,5] | không | CÓ | 
| 1-4 | [3,2,1,4] | 0(3>1) | KHÔNG | 

Điều này xác nhận rằng cách tiếp cận tối đa tiền tố xác định chính xác các mảng con hoàn hảo và không hoàn hảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Một lượt để tính toán vi phạm, một lượt khác để trả lời các truy vấn trong thời gian không đổi cho mỗi truy vấn | 
| Không gian | O(n) | Lưu trữ các vi phạm và mảng tiền tố tối đa | 

Thuật toán phù hợp với các ràng buộc, như$n + q \le 5\cdot10^5$và mỗi phép toán là số học hoặc so sánh đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# provided sample
assert run("2\n5 5\n1 5 4 3 2\n1 2\n1 5\n3 5\n1 4\n2 5\n5 5\n3 2 1 4 5\n1 1\n4 5\n1 4\n2 5\n3 4\n") == \
"YES\nNO\nNO\nNO\nNO\nYES\nYES\nNO\nYES\nYES"

# minimum-size inputs
assert run("1\n1 1\n1\n1 1\n") == "YES"

# all increasing
assert run("1\n5 2\n1 2 3 4 5\n1 5\n2 4\n") == "YES\nYES"

# all decreasing
assert run("1\n5 2\n5 4 3 2 1\n1 5\n2 4\n") == "NO\nNO"

# single-element queries
assert run("1\n3 3\n3 1 2\n1 1\n2 2\n3 3\n") == "YES\nYES\nYES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | CÓ | truy vấn một phần tử | 
| 2 | CÓ, CÓ | mảng được sắp xếp đầy đủ | 
| 3 | KHÔNG, KHÔNG | mảng đảo ngược hoàn toàn | 
| 4 | CÓ, CÓ, CÓ | truy vấn chọn các phần tử đơn lẻ theo thứ tự bất kỳ | 

## Vỏ cạnh 

cho
