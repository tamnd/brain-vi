---
title: "CF 102392H - Hoán vị cây"
description: "Cây ban đầu có gốc ở đỉnh (1) và mỗi đỉnh (i1) có cha mẹ (pi<i) và trọng số cạnh (wi). Multiset chứa tất cả các giá trị gốc này và tất cả các trọng số cạnh này có các phần tử (2n-2), nhưng vai trò của chúng bị mất do mảng bị xáo trộn."
date: "2026-08-10T19:46:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102392
codeforces_index: "H"
codeforces_contest_name: "2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)"
rating: 0
weight: 102392
solve_time_s: 162
verified: true
draft: false
---

[CF 102392H - Hoán vị cây](https://codeforces.com/problemset/problem/102392/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cây ban đầu có gốc ở đỉnh (1) và mỗi đỉnh (i>1) có gốc (p_i<i) và trọng số cạnh (w_i). Multiset chứa tất cả các giá trị gốc này và tất cả các trọng số cạnh này có các phần tử (2n-2), nhưng vai trò của chúng bị mất do mảng bị xáo trộn. 

Chúng ta không cần phải xây dựng lại một cây cụ thể. Đối với mọi độ dài đường đi có thể có (k) từ đỉnh (1) đến đỉnh (n), chúng ta cần tổng trọng số lớn nhất có thể có trên đường đi đó. Nếu không có cây hợp lệ nào có chính xác (k) cạnh trên đường đi đó thì câu trả lời cho (k) là (-1). 

Khó khăn chính là mọi số trong mảng được xáo trộn đều có thể trở thành giá trị gốc hoặc trọng số cạnh. Một số được sử dụng làm số gốc có hạn chế về cấu trúc, trong khi một số được sử dụng làm trọng số thì không có hạn chế đó. Giải pháp đến từ việc tách biệt hai vai trò này. 

Với (n\le 10^5), mảng được xáo trộn có gần như (2\cdot10^5) phần tử. Giới hạn thời gian một giây loại trừ mọi thứ bậc hai và chắc chắn loại trừ việc liệt kê các cây hoặc hoán vị có thể có. Về cơ bản, chúng ta cần xử lý mọi giá trị đầu vào chỉ một số lần không đổi hoặc logarit. Một giải pháp (O(n\log n)) là đủ và bài xã luận chính thức của cuộc thi sử dụng cùng một giới hạn tiệm cận để thực hiện nó. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai tham lam có vẻ hợp lý không thành công. 

Coi như```
3
2 2 2 2
```Sau khi sắp xếp, giá trị đầu tiên là (2), nhưng cha của đỉnh (2) phải nhỏ hơn (2). Không có sự sắp xếp nào thậm chí có thể tạo thành một cây hợp lệ, vì vậy kết quả đầu ra đúng là```
-1 -1
```Một giải pháp bất cẩn có thể đặt một (2) làm trọng lượng và cố gắng sử dụng (2) khác làm (p_2), âm thầm vi phạm (p_2<2). 

Giá trị trùng lặp cũng quan trọng. Vì```
4
1 1 1 1 1 1
```chỉ có một giá trị phân biệt, (1). Đường đi duy nhất có thể có một cạnh và trọng số của nó là (1). Đầu ra đúng là```
1 -1 -1
```Một giải pháp coi các giá trị bằng nhau là các đỉnh đường dẫn có thể khác nhau có thể xác nhận sai các đường dẫn dài hơn. 

Một trường hợp ít rõ ràng hơn là khi một giá trị (i) bị ép vào đường dẫn mặc dù các giá trị nhỏ hơn được lặp lại. Ví dụ,```
6
1 1 1 4 4 4 4 4 5 5
```Sau khi sắp xếp, giá trị thứ tư là (4), do đó đỉnh (4) được buộc vào đường dẫn. Các giá trị đường dẫn bắt buộc là (1) và (4), trong khi (5) là giá trị đường dẫn tùy chọn. Độ dài hợp lệ là (2) và (3), cho```
-1 10 13 -1 -1
```Một chiến lược ngây thơ chỉ đơn giản lấy các giá trị riêng biệt nhỏ nhất sẽ bỏ lỡ thực tế rằng (4) là bắt buộc. 

Cuối cùng, (k=n-1) là trường hợp biên chính xác. Vì```
4
1 2 3 3 3 3
```đường đi phải chứa mọi đỉnh nên độ dài duy nhất có thể là (3). Sau khi bảo lưu các giá trị gốc (1,2,3), ba giá trị lớn nhất còn lại đều là (3), cho```
-1 -1 9
```Sự khác biệt giữa (n-1), số cạnh trong đường đi từ gốc đến (n) Hamiltonian và (n), số đỉnh trên đường đi đó, là một nguồn dễ dàng gây ra lỗi từng cái một. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ gán cho mỗi phần tử trong số (2n-2) mảng một vị trí trong chuỗi các giá trị và trọng số gốc, sau đó kiểm tra xem cây kết quả có hợp lệ hay không và ghi lại độ dài đường đi và tổng trọng số của nó. Ngay cả khi bỏ qua các giá trị trùng lặp, điều này có nghĩa là phải xem xét các sắp xếp ((2n-2)!). Tại (n=10^5), số lượng cách sắp xếp lớn đến mức ngay cả việc kiểm tra một cách sắp xếp trong thời gian không đổi cũng sẽ vô nghĩa. Kiểm tra từng sự sắp xếp trong (O(n)) sẽ cho ra (O(n(2n-2)!)), do đó, lực lượng vũ phu chỉ hữu ích cho các trường hợp nhỏ được sử dụng để khám phá các mẫu. 

Lực lượng vũ phu hoạt động vì nó khám phá rõ ràng mọi sự phân công vai trò có thể. Nó thất bại vì hầu như tất cả những nhiệm vụ đó đều không liên quan. Quan sát hữu ích là các giá trị gốc có đặc tính được sắp xếp rất cứng nhắc. 

Sắp xếp mảng được xáo trộn thành 

[ 
a_1\le a_2\le\cdots\le a_{2n-2}. 
] 

Đối với một cây hợp lệ, các phần tử (i) đầu tiên không thể chứa giá trị lớn hơn (i), với mọi (i\le n-1). Tương đương, 

[ 
a_i\le tôi. 
] 

Để biết lý do tại sao, các đỉnh (2,3,\ldots,i+1) cần (i) giá trị cha và mỗi giá trị cha trong số đó nhiều nhất là (i). Nếu (a_i>i), có nhiều nhất ít hơn (i) giá trị khả dụng (i), do đó (i) đỉnh đầu tiên không thể nhận được cha mẹ hợp pháp. Do đó, một vi phạm duy nhất có nghĩa là không có cây hợp lệ nào tồn tại. Đây là mức giảm lớn đầu tiên. 

Có một hậu quả thứ hai mạnh mẽ hơn. Giả sử 

[ 
a_i=i. 
] 

Khi đó đỉnh (i) phải nằm trên đường đi từ (1) đến (n). Có nhiều nhất (i-1) giá trị nhỏ hơn (i) và các đỉnh (2,\ldots,i) đã yêu cầu chính xác (i-1) giá trị gốc hợp pháp. Các giá trị đó được tiêu thụ hoàn toàn bởi các đỉnh thấp hơn, do đó mọi đỉnh lớn hơn (i) đều có ít nhất một đỉnh cha (i). Đường đi từ (n) quay lại (1) không thể nhảy từ giá trị lớn hơn (i) đến giá trị nhỏ hơn (i) mà không đi qua (i). Do đó (i) bị buộc phải đi vào con đường. 

Gọi (c) là số chỉ số (i\in[1,n-1]) thỏa mãn (a_i=i). Các giá trị (c) này phải xuất hiện trên mọi đường đi có thể từ (1) đến (n), do đó không thể tồn tại đường dẫn nào có ít hơn (c) cạnh. 

Bây giờ hãy xem xét tất cả các giá trị riêng biệt xuất hiện trong mảng. Một đường dẫn không thể chứa cùng một đỉnh hai lần, vì vậy các giá trị gốc của nó là khác nhau. Vì đường đi có (k) cạnh nên nó sử dụng chính xác (k) giá trị gốc, cụ thể là các đỉnh đường đi ngoại trừ (n). Do đó (k) không thể vượt quá số lượng giá trị riêng biệt trong mảng. 

Phần quan trọng là mọi (k) giữa hai giới hạn này thực sự có thể đạt được. Bắt đầu với tất cả các giá trị bắt buộc. Bất cứ khi nào chúng ta cần thêm một cạnh đường dẫn, hãy thêm giá trị khác biệt nhỏ nhất chưa được chọn. Các giá trị đã chọn sau đó được sắp xếp và đặt giữa (1) và (n) để tạo thành đường dẫn. 

Tại sao việc chọn giá trị sẵn có nhỏ nhất lại có tác dụng? Điều kiện được sắp xếp (a_i\le i) đảm bảo đủ các giá trị cha nhỏ để hỗ trợ tất cả các đỉnh bên ngoài đường dẫn. Cụ thể hơn, sau khi sửa đường dẫn, hãy xóa một lần xuất hiện của mỗi giá trị gốc của đường dẫn. Sắp xếp các giá trị cha mẹ ứng viên còn lại và các đỉnh vẫn cần cha mẹ. Ghép nối chúng theo thứ tự tăng dần. Nếu một số ứng cử viên cha không nhỏ hơn đỉnh được chỉ định của nó, thì việc đếm tối đa có bao nhiêu đỉnh đường đi và đỉnh chưa được gán thì giá trị đó sẽ mâu thuẫn với điều kiện được sắp xếp ban đầu (a_i\le i). Đây là lập luận mang tính xây dựng được sử dụng trong bài xã luận chính thức. 

Khi một đường dẫn cụ thể khả thi, trọng số tối ưu của nó sẽ đơn giản hơn nhiều. Chính xác (k) phần tử mảng được sử dụng làm giá trị gốc của đường dẫn. Mọi phần tử còn lại khác có thể là trọng số và trọng số không có giới hạn giới hạn trên. Do đó, trọng số đường dẫn tối đa có được bằng cách lấy (k) phần tử lớn nhất trong số mọi thứ còn lại sau khi loại bỏ một lần xuất hiện của mỗi giá trị đường dẫn đã chọn.

Điều này mang lại toàn bộ thuật toán. Chúng tôi sắp xếp mảng để phát hiện các giá trị không thể và bắt buộc. Sau đó chúng tôi thêm các giá trị đường dẫn tùy chọn từ nhỏ nhất đến lớn nhất. Sau mỗi lần cộng, chúng ta cần tổng (k) giá trị lớn nhất trong nhiều tập hợp còn lại. Cây Fenwick trên các giá trị (1,\ldots,n-1) duy trì cả số lần xuất hiện còn lại và tổng của chúng, cho phép truy vấn này trong (O(\log n)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n(2n-2)!)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị (2n-2) và sắp xếp chúng. Việc sắp xếp sẽ hiển thị điều kiện tiền tố (a_i\le i), đây là phép kiểm tra đơn giản nhất có thể để xem liệu có cây hợp lệ nào tồn tại hay không. 
2. Kiểm tra mọi vị trí đã sắp xếp (i) từ (1) đến (n-1). Nếu (a_i>i), xuất ra (-1) cho mỗi độ dài đường đi và điểm dừng. Không thể có bất kỳ cây hợp lệ nào cả, vì vậy mọi câu trả lời đều không thể. 
3. Xây dựng bảng tần số cho tất cả các giá trị và khởi tạo cây Fenwick chứa mọi lần xuất hiện của mọi giá trị mảng. Cây Fenwick lưu trữ cả số lần xuất hiện và tổng, vì sau này chúng ta cần xóa các giá trị gốc đã chọn và truy vấn các giá trị lớn còn lại. 
4. Quét lại mảng đã sắp xếp. Bất cứ khi nào (a_i=i), đánh dấu giá trị (i) là bắt buộc, loại bỏ một lần xuất hiện của (i) khỏi cây Fenwick và tăng độ dài đường dẫn hiện tại (k). Việc loại bỏ chính xác một lần xuất hiện là đúng vì một bản sao được coi là giá trị gốc của cạnh đường đi vào đỉnh (i+1). 
5. Sau khi tất cả các giá trị bắt buộc đã được xử lý, hãy tính kết quả cho (k) hiện tại. Multiset còn lại chứa mọi giá trị không được sử dụng dưới dạng đường dẫn gốc bắt buộc, vì vậy trọng số đường dẫn tốt nhất có thể là tổng của (k) phần tử lớn nhất của nó. 
6. Xử lý các giá trị (x=1,2,\ldots,n-1) theo thứ tự tăng dần. Nếu (x) xuất hiện và chưa bị ép buộc, hãy xóa một lần xuất hiện của (x), đánh dấu nó là đỉnh đường dẫn mới và tăng dần (k). Thứ tự tăng dần chính xác là sự lựa chọn tham lam để đảm bảo các giá trị còn lại vẫn có thể đóng vai trò là cha mẹ hợp lệ. 
7. Sau mỗi phép cộng như vậy, hãy truy vấn tổng của (k) giá trị lớn nhất còn lại và lưu nó dưới dạng câu trả lời cho độ dài đường dẫn này. Không có độ dài đường dẫn nào khác có thể xảy ra, bởi vì mọi độ dài khả thi đều nằm giữa số lượng bắt buộc và số lượng các giá trị riêng biệt. 
8. Để lại mọi câu trả lời ngoài khoảng này là (-1). Mảng câu trả lời đã được khởi tạo theo cách này nên không cần phải xây dựng riêng biệt. 

Điều bất biến là sau khi xử lý độ dài đường dẫn (k), cây Fenwick chứa chính xác nhiều tập giá trị vẫn có thể được sử dụng cho các trọng số và cha mẹ ngoài đường dẫn sau khi bảo lưu (k) các giá trị đường dẫn gốc đã chọn. Bản thân đường dẫn đã chọn có thể thực hiện được nhờ điều kiện tiền tố được sắp xếp và cấu trúc có giá trị tham lam nhỏ nhất. Vì trọng số không có ràng buộc về cấu trúc nên việc chọn giá trị lớn nhất còn lại (k) là tối ưu. Do đó, mọi câu trả lời được lưu trữ đều có thể đạt được và tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.cnt = [0] * (n + 1)
        self.s = [0] * (n + 1)

    def add(self, x, dc, ds):
        n = self.n
        cnt = self.cnt
        sm = self.s
        while x <= n:
            cnt[x] += dc
            sm[x] += ds
            x += x & -x

    def sum_smallest(self, k):
        if k <= 0:
            return 0

        idx = 0
        cnt = 0
        sm = 0

        bit = 1 << (self.n.bit_length() - 1)
        while bit:
            nxt = idx + bit
            if nxt <= self.n and cnt + self.cnt[nxt] <= k:
                idx = nxt
                cnt += self.cnt[nxt]
                sm += self.s[nxt]
            bit >>= 1

        if cnt < k:
            value = idx + 1
            sm += (k - cnt) * value

        return sm

    def sum_largest(self, k, total_count, total_sum):
        return total_sum - self.sum_smallest(total_count - k)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    m = 2 * n - 2

    a.sort()

    for i in range(1, n):
        if a[i - 1] > i:
            return " ".join(["-1"] * (n - 1))

    freq = [0] * n
    forced = [False] * n

    bit = Fenwick(n - 1)

    total_sum = 0
    for x in a:
        freq[x] += 1
        total_sum += x
        bit.add(x, 1, x)

    total_count = m

    k = 0

    for i in range(1, n):
        if a[i - 1] == i:
            forced[i] = True
            freq[i] -= 1
            bit.add(i, -1, -i)
            total_count -= 1
            total_sum -= i
            k += 1

    ans = [-1] * (n - 1)

    if k > 0:
        ans[k - 1] = bit.sum_largest(k, total_count, total_sum)

    for x in range(1, n):
        if freq[x] > 0 and not forced[x]:
            freq[x] -= 1
            bit.add(x, -1, -x)
            total_count -= 1
            total_sum -= x
            k += 1

            ans[k - 1] = bit.sum_largest(k, total_count, total_sum)

    return " ".join(map(str, ans))

if __name__ == "__main__":
    sys.stdout.write(solve())
```Mảng đã sắp xếp được sử dụng đầu tiên vì tất cả thông tin cấu trúc về giá trị gốc đều được tiền tố của nó nắm bắt. Điều kiện chỉ được kiểm tra thông qua các vị trí (1,\ldots,n-1), vì có chính xác (n-1) giá trị cha và đỉnh thứ (n)-th không thể là cha. 

các`freq`mảng ghi lại các giá trị riêng biệt vẫn có sẵn dưới dạng các đỉnh đường dẫn có thể. các`forced`mảng ngăn không cho giá trị bắt buộc được chọn lần thứ hai khi lần quét tham lam sau đó đạt đến cùng một giá trị. Điều này quan trọng khi một giá trị xảy ra nhiều lần. 

Cây Fenwick lưu trữ hai đại lượng ở mỗi tiền tố, số lần xuất hiện còn lại và tổng của chúng.`sum_smallest(t)`tìm tổng của (t) phần tử nhỏ nhất còn lại bằng cách nâng nhị phân qua cây Fenwick. Nếu tiền tố mong muốn kết thúc giữa một giá trị có nhiều lần xuất hiện bằng nhau thì các bản sao còn lại sẽ được xử lý cùng nhau ở cuối. 

Tổng các giá trị lớn nhất (k) còn lại được tính bằng 

[ 
\text{tổng số}-\text{tổng các giá trị nhỏ nhất }(m-k)\text{}. 
] 

Điều này tránh cần một cấu trúc thống kê thứ tự tối đa riêng biệt. 

Số nguyên Python có độ chính xác tùy ý, do đó tổng đường dẫn không cần xử lý tràn đặc biệt. Tổng lớn nhất có thể chỉ là (O(n^2)), nhưng việc triển khai vẫn đúng ngay cả khi không dựa vào độ rộng số nguyên cố định. 

Việc chuyển đổi chỉ số cũng có chủ ý. Các mảng toán học được lập chỉ mục một, trong khi danh sách Python không được lập chỉ mục. Bài kiểm tra`a[i - 1] > i`tương ứng chính xác với điều kiện toán học (a_i>i) và`ans[k - 1]`lưu trữ câu trả lời cho một đường dẫn chứa (k) cạnh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3
1 1 2 2
```Sau khi sắp xếp thì mảng đã có ([1,1,2,2]). Điều kiện tiền tố hợp lệ vì (a_1=1\le1) và (a_2=1\le2). 

Đẳng thức duy nhất (a_i=i) cho (i\le2) là (a_1=1), do đó độ dài đường đi khả thi tối thiểu là (1). 

| Bước | Giá trị đường dẫn đã chọn | Còn lại nhiều bộ | (k) | Tổng tốt nhất | 
| --- | --- | --- | --- | --- | 
| Ban đầu | ({1}) | ({1,2,2}) | 1 | 2 | 
| Thêm giá trị 2 | ({1,2}) | ({1,2}) | 2 | 3 | 

Đối với (k=1), hãy dành một (1) làm giá trị gốc của đỉnh (n=3). Giá trị lớn nhất còn lại là (2), nên đáp án là (2). 

Đối với (k=2), hãy dành một (1) và một (2) làm giá trị gốc của đường dẫn. Hai giá trị lớn nhất còn lại là (2) và (1) cho (3). 

Kết quả đầu ra là```
2 3
```Dấu vết chứng minh rằng một giá trị có thể được sử dụng làm giá trị gốc hoặc làm trọng số và lựa chọn tối ưu là đặt trước các giá trị đường dẫn gốc được yêu cầu nhỏ nhất trong khi vẫn giữ các giá trị lớn nhất có thể có cho trọng số đường dẫn. 

### Mẫu 2 

Đầu vào là```
3
2 2 2 2
```Mảng được sắp xếp không thay đổi. 

| Chức vụ (i) | (a_i) | Giới hạn bắt buộc (a_i\le i) | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 2 | (2\le1) | Sai | 

Thuật toán dừng ngay lập tức. 

| Bước | (k) câu trả lời | Lý do | 
| --- | --- | --- | 
| Kiểm tra tính hợp lệ | ([-1,-1]) | Không có cây hợp lệ tồn tại | 

Đầu ra là```
-1 -1
```Điều này thực hiện sự thất bại sớm nhất có thể của điều kiện tiền tố. Không có tính toán đường dẫn nào sẽ xảy ra sau lần kiểm tra này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Việc sắp xếp mất (O(n\log n)) và có (O(n)) cập nhật và truy vấn Fenwick, mỗi lần lấy (O(\log n)). | 
| Không gian | (O(n)) | Mảng được sắp xếp, mảng tần số, mảng trả lời và cây Fenwick đều sử dụng bộ nhớ (O(n)). | 

Đầu vào chứa các số (2n-2=O(n)), do đó thuật toán xử lý một lượng dữ liệu tuyến tính ngoài việc sắp xếp và các hoạt động Fenwick. Với (n\le10^5), giá trị này nằm trong phạm vi dự định (O(n\log n)) và giới hạn bộ nhớ 256 MB. Giải pháp cuộc thi chính thức cũng sử dụng cấu trúc dữ liệu phụ trợ (O(n\log n)) thời gian và (O(n)). 

## Trường hợp thử nghiệm```python
import sys
import io

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.cnt = [0] * (n + 1)
        self.s = [0] * (n + 1)

    def add(self, x, dc, ds):
        while x <= self.n:
            self.cnt[x] += dc
            self.s[x] += ds
            x += x & -x

    def sum_smallest(self, k):
        if k <= 0:
            return 0

        idx = 0
        cnt = 0
        sm = 0
        bit = 1 << (self.n.bit_length() - 1)

        while bit:
            nxt = idx + bit
            if nxt <= self.n and cnt + self.cnt[nxt] <= k:
                idx = nxt
                cnt += self.cnt[nxt]
                sm += self.s[nxt]
            bit >>= 1

        if cnt < k:
            sm += (k - cnt) * (idx + 1)

        return sm

def solution(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)
    n = next(it)
    a = [next(it) for _ in range(2 * n - 2)]

    a.sort()

    for i in range(1, n):
        if a[i - 1] > i:
            return " ".join(["-1"] * (n - 1))

    freq = [0] * n
    forced = [False] * n
    bit = Fenwick(n - 1)

    total_count = len(a)
    total_sum = sum(a)

    for x in a:
        freq[x] += 1
        bit.add(x, 1, x)

    k = 0

    for i in range(1, n):
        if a[i - 1] == i:
            forced[i] = True
            freq[i] -= 1
            bit.add(i, -1, -i)
            total_count -= 1
            total_sum -= i
            k += 1

    ans = [-1] * (n - 1)

    if k:
        small = bit.sum_smallest(total_count - k)
        ans[k - 1] = total_sum - small

    for x in range(1, n):
        if freq[x] and not forced[x]:
            freq[x] -= 1
            bit.add(x, -1, -x)
            total_count -= 1
            total_sum -= x
            k += 1

            small = bit.sum_smallest(total_count - k)
            ans[k - 1] = total_sum - small

    return " ".join(map(str, ans))

def run(inp: str) -> str:
    return solution(inp)

# Provided samples
assert run(
    """3
1 1 2 2
"""
) == "2 3", "sample 1"

assert run(
    """3
2 2 2 2
"""
) == "-1 -1", "sample 2"

assert run(
    """6
1 4 5 4 4 4 3 4 4 2
"""
) == "-1 -1 -1 17 20", "sample 3"

# Minimum-size input
assert run(
    """2
1 1
"""
) == "1", "minimum n"

# All values equal
assert run(
    """4
1 1 1 1 1 1
"""
) == "1 -1 -1", "all equal values"

# Only the maximum path length is possible
assert run(
    """4
1 2 3 3 3 3
"""
) == "-1 -1 9", "maximum path length"

# Forced value 4 is not the second distinct value
assert run(
    """6
1 1 1 4 4 4 4 4 5 5
"""
) == "-1 10 13 -1 -1", "forced non-prefix value"

# Maximum-size input
n = 100000
large_input = str(n) + "\n" + " ".join(["1"] * (2 * n - 2)) + "\n"
large_expected = "1 " + " ".join(["-1"] * (n - 2))
assert run(large_input) == large_expected, "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1`|`1`| Tối thiểu (n), cạnh đơn | 
|`4 / six 1s`|`1 -1 -1`| Giá trị trùng lặp và hạn chế đường dẫn riêng biệt | 
|`4 / 1 2 3 3 3 3`|`-1 -1 9`| Ranh giới (k=n-1) | 
|`6 / 1 1 1 4 4 4 4 4 5 5`|`-1 10 13 -1 -1`| Giá trị đường dẫn bắt buộc không phải là một phần của tiền tố riêng biệt nhỏ nhất | 
|`100000 / 199998\) ones`|`1`theo sau là (99998) bản sao của`-1`| Kích thước đầu vào và hành vi bộ nhớ tối đa | 

## Vỏ cạnh 

### Không có cây hợp lệ 

cho```
3
2 2 2 2
```mảng được sắp xếp bắt đầu bằng (a_1=2). Vì cha của đỉnh (2) phải là (1), nên không có sự xuất hiện nào của (2) có thể đảm nhận vai trò đó. Quá trình quét hợp lệ sẽ phát hiện (a_1>1) ngay lập tức và trả về`-1 -1`. Cây Fenwick không bao giờ được truy vấn, do đó một trường hợp không hợp lệ không thể vô tình tạo ra tổng đường dẫn hợp lý. 

### Giá trị lặp lại 

cho```
4
1 1 1 1 1 1
```giá trị khác biệt duy nhất là (1). Vị trí được sắp xếp đầu tiên thỏa mãn (a_1=1), do đó một (1) trở thành giá trị gốc bắt buộc cho cạnh đường dẫn duy nhất có thể. Bảy bản sao còn lại có sẵn cho các trọng số và các đỉnh cha khác, nhưng không có giá trị đỉnh riêng biệt thứ hai nào có thể được đặt trên đường đi. Thuật toán ghi lại câu trả lời một cạnh là (1) và để lại các vị trí còn lại ở (-1). 

### Giá trị đường dẫn bắt buộc phải rời khỏi đầu 

cho```
6
1 1 1 4 4 4 4 4 5 5
```tiền tố được sắp xếp thỏa mãn 

[ 
a_1=1,\qquad a_2=1,\qquad a_3=1,\qquad a_4=4. 
] 

Do đó (1) và (4) bị buộc phải đi vào mọi con đường. Độ dài đường dẫn ban đầu là (2). Loại bỏ một (1) và một (4) để lại hai (5) và bốn (4) trong số các giá trị lớn nhất, do đó hai giá trị lớn nhất còn lại có tổng bằng (10). Điều này đưa ra câu trả lời cho (k=2). 

Giá trị riêng biệt không được sử dụng tiếp theo là (5). Loại bỏ một (5) sẽ tạo ra độ dài đường dẫn (3) và ba giá trị lớn nhất còn lại là (5,4,4), có tổng là (13). Không còn giá trị phân biệt nào nữa, vì vậy (k=4) và (k=5) vẫn không thể thực hiện được. Đầu ra cuối cùng là`-1 10 13 -1 -1`. 

### Độ dài đường dẫn tối đa 

cho```
4
1 2 3 3 3 3
```các đẳng thức (a_1=1), (a_2=2) và (a_3=3) buộc tất cả ba giá trị gốc (1,2,3) vào đường dẫn. Do đó (k=3=n-1) là độ dài khả thi duy nhất. Sau khi loại bỏ một lần xuất hiện của mỗi (1,2,3), ba bản sao của (3) vẫn còn trong số các giá trị có thể được sử dụng làm trọng số đường dẫn, vì vậy câu trả lời là (9). Thuật toán lưu trữ nó tại`ans[2]`, tương ứng với (k=3), tránh lỗi phổ biến khi coi (n) đỉnh của đường đi là (n) cạnh. 

Bài xã luận ở trên tuân theo đặc tính chính thức của các tiền tố được sắp xếp hợp lệ, các đỉnh đường dẫn bắt buộc và sự lựa chọn tham lam của các giá trị đường dẫn bổ sung, trong khi sử dụng cây Fenwick cho tổng thống kê thứ tự.
