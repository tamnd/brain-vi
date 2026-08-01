---
title: "CF 102576F - Halfwitters"
description: "Chúng ta có một hàng gồm n người lính được đánh số duy nhất. Mục đích là chuyển đổi hoán vị hiện tại thành thứ tự được sắp xếp. Các hoạt động được phép là đổi lính lân cận, đảo ngược toàn bộ hàng hoặc trả một chi phí cố định để ngẫu nhiên hóa toàn bộ hàng."
date: "2026-07-31T07:34:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102576
codeforces_index: "F"
codeforces_contest_name: "2020 Petrozavodsk Winter Camp, Jagiellonian U Contest"
rating: 0
weight: 102576
solve_time_s: 94
verified: true
draft: false
---

[CF 102576F - The Halfwitters](https://codeforces.com/problemset/problem/102576/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng tôi có một hàng`n`binh sĩ được đánh số duy nhất. Mục đích là chuyển đổi hoán vị hiện tại thành thứ tự được sắp xếp. Các hoạt động được phép là đổi lính lân cận, đảo ngược toàn bộ hàng hoặc trả một chi phí cố định để ngẫu nhiên hóa toàn bộ hàng. Sau khi ngẫu nhiên hóa, hàng được phân bố đồng đều trên tất cả`n!`hoán vị có thể xảy ra, do đó chi phí dự kiến ​​trong tương lai là như nhau ở mọi trạng thái. 

Đối với mỗi hoán vị bắt đầu, chúng ta cần thời gian dự kiến ​​tối ưu. Chi phí tương tự được sử dụng cho tất cả các ngày trong một trường hợp thử nghiệm, nhưng hoán vị ban đầu thay đổi, do đó giải pháp phải trả lời nhiều truy vấn một cách nhanh chóng. 

Sự ràng buộc`n <= 16`là manh mối chính. Việc liệt kê tất cả các hoán vị là không thể vì`16!`là về`2 * 10^13`. Tuy nhiên,`n(n-1)/2 <= 120`, do đó, bất kỳ đại lượng nào chỉ phụ thuộc vào số lần đảo ngược đều có thể được xử lý bằng một chương trình động nhỏ. Số ngày có thể đạt tới`100000`, có nghĩa là mỗi truy vấn phải gần với thời gian không đổi sau khi xử lý trước. 

Điểm tinh tế là hoạt động ngẫu nhiên không có nghĩa là chúng ta nên mô phỏng các trạng thái ngẫu nhiên. Hoạt động ngẫu nhiên chỉ đóng góp một giá trị chưa biết, chi phí dự kiến ​​trung bình trên tất cả các hoán vị. Một sai lầm phổ biến khác là cho rằng thao tác đảo ngược chỉ có thể được sử dụng một lần mà không cần bằng chứng. Nó thực sự có thể được giảm xuống tối đa một lần đảo chiều vì hai lần đảo chiều có thể được di chuyển cùng nhau và loại bỏ, trong khi các giao dịch hoán đổi liền kề giữa chúng có thể được phản ánh. 

Một hoán vị đã được sắp xếp có câu trả lời`0/1`. Ví dụ, với`n=3`và chi phí`a=b=c=1`, hoán vị đầu vào`1 2 3`phải sản xuất`0/1`; coi hoạt động ngẫu nhiên là bắt buộc sẽ đưa ra câu trả lời tích cực sai. 

Hoạt động ngược lại cũng tạo ra một trường hợp biên. Vì`n=3`, hoán vị`3 2 1`có ba nghịch đảo, nhưng nghịch đảo của nó không có nghịch đảo. Một giải pháp chỉ xem xét các giao dịch hoán đổi liền kề sẽ trả lại chi phí`3a`, trong khi chi phí xác định chính xác có thể chỉ là`b`. 

# Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ mô hình hóa mọi hoán vị dưới dạng nút biểu đồ. Mỗi nút sẽ có các cạnh tới các trạng thái thu được bằng cách hoán đổi và đảo ngược liền kề, cộng với một cạnh biểu thị sự ngẫu nhiên. Chạy thư giãn kiểu đường dẫn ngắn nhất trên biểu đồ này sẽ đúng, nhưng biểu đồ chứa`n!`nút. Tại`n=16`, thậm chí việc lưu trữ các nút là không thể. 

Quan sát quan trọng là chi phí sắp xếp xác định chỉ phụ thuộc vào số lượng đảo ngược. Các giao dịch hoán đổi liền kề sẽ loại bỏ hoặc tạo một đảo ngược, do đó sắp xếp mà không có chi phí đảo ngược`a * inv`. Nếu chúng ta sử dụng một lần đảo ngược, số lần đảo ngược sẽ thay đổi từ`k`ĐẾN`m-k`, Ở đâu`m=n(n-1)/2`, bởi vì mọi cặp đều thay đổi từ đảo ngược sang không đảo ngược hoặc ngược lại. Do đó, chi phí xác định tốt nhất là:`min(a*k, b + a*(m-k))`. 

Bây giờ hãy xem xét hoạt động ngẫu nhiên. Cho phép`M`là câu trả lời được mong đợi trung bình trên tất cả các hoán vị. Chi phí của việc ngẫu nhiên hóa là`c+M`, đó là một hằng số. Nếu chi phí xác định của một quốc gia là`D`, giá trị tối ưu chỉ đơn giản là`min(D, c+M)`. Điều này có hiệu quả vì nếu một đường dẫn đạt đến trạng thái có giá trị nhỏ hơn chi phí ngẫu nhiên thì bản thân đường dẫn đó chứng tỏ trạng thái ban đầu có nghiệm xác định nhỏ hơn. 

Vì vậy, chúng tôi chỉ cần phân phối số lượng đảo ngược. Số lượng hoán vị với mỗi lần đảo ngược có thể được tìm thấy bằng lập trình động số Mahonian cổ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n!)`tiểu bang |`O(n!)`| Quá chậm | 
| Tối ưu |`O(n^2 + n^3 + d)`mỗi trường hợp thử nghiệm |`O(n^2)`| Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Tính trước bao nhiêu hoán vị của độ dài`n`có mọi số lượng đảo ngược có thể. DP thêm một phần tử mới lớn nhất và thử tất cả các vị trí có thể, điều này tạo ra giữa`0`Và`i-1`những nghịch đảo mới. 
2. Đối với mọi số lần đảo ngược có thể`k`, tính chi phí sắp xếp xác định:`min(a*k, b+a*(m-k))`. 

Câu trả lời cho một hoán vị cụ thể chỉ cần số lần đảo ngược của nó. 
3. Giải giá trị ngẫu nhiên trung bình. Cho phép`C=c+M`. Chúng tôi cần:`C-c = average(min(C, deterministic_cost))`. 

Vế phải là tuyến tính từng phần vì mọi chi phí xác định đều là một giá trị không đổi. Sắp xếp các chi phí xác định có thể có và tìm khoảng chứa điểm cố định. Bên trong một khoảng phương trình trở thành một phương trình tuyến tính đơn giản. 
4. Đối với mỗi hoán vị truy vấn, hãy đếm số lần đảo ngược bằng vòng lặp kép. Tính chi phí xác định và lợi nhuận của nó`min(C, cost)`dưới dạng phân số rút gọn. 

Tại sao nó hoạt động: thông tin duy nhất cần có từ một hoán vị là số lần đảo ngược của nó. Các giao dịch hoán đổi liền kề và đảo ngược toàn cục duy trì đủ tính đối xứng để mọi hoán vị có cùng số lượng đảo ngược đều có cùng mức tối ưu xác định. Việc tính toán điểm cố định tính toán chính xác khả năng cuối cùng sử dụng phép toán ngẫu nhiên, bởi vì mọi trạng thái đều chọn giữa lộ trình xác định của nó và cùng một giá trị khởi động lại ngẫu nhiên. 

#Giải pháp Python```python
import sys
from fractions import Fraction

input = sys.stdin.readline

cache_dp = {}

def inversion_counts(n):
    if n in cache_dp:
        return cache_dp[n]
    dp = [1]
    for length in range(1, n + 1):
        ndp = [0] * (len(dp) + length)
        for i, x in enumerate(dp):
            for add in range(length):
                ndp[i + add] += x
        dp = ndp
    cache_dp[n] = dp
    return dp

def solve_average(n, a, b, c):
    counts = inversion_counts(n)
    total = 1
    for i in range(2, n + 1):
        total *= i

    mx = n * (n - 1) // 2
    values = []
    for k, cnt in enumerate(counts):
        values.append((min(a * k, b + a * (mx - k)), cnt))

    grouped = {}
    for v, cnt in values:
        grouped[v] = grouped.get(v, 0) + cnt

    arr = sorted(grouped.items())

    pref_sum = 0
    pref_cnt = 0
    prev = Fraction(0)

    for value, cnt in arr:
        if pref_cnt:
            cand = Fraction(total * c + pref_sum, pref_cnt)
            if cand <= value and cand >= prev:
                return cand
        pref_sum += value * cnt
        pref_cnt += cnt
        prev = Fraction(value)

    return Fraction(total * c + pref_sum, total)

def inv_of_perm(p):
    n = len(p)
    ans = 0
    for i in range(n):
        pi = p[i]
        for j in range(i + 1, n):
            if pi > p[j]:
                ans += 1
    return ans

def solve_case(n, a, b, c, perms):
    C = solve_average(n, a, b, c)
    mx = n * (n - 1) // 2
    out = []

    for p in perms:
        k = inv_of_perm(p)
        det = min(a * k, b + a * (mx - k))
        ans = min(C, Fraction(det))
        out.append(f"{ans.numerator}/{ans.denominator}")
    return out

def main():
    data = sys.stdin.read().strip().splitlines()
    if not data:
        return

    first = list(map(int, data[0].split()))
    idx = 0
    cases = []

    if len(first) == 1:
        z = first[0]
        idx = 1
        for _ in range(z):
            n, a, b, c, d = map(int, data[idx].split())
            idx += 1
            perms = []
            for _ in range(d):
                perms.append(list(map(int, data[idx].split())))
                idx += 1
            cases.append((n, a, b, c, perms))
    else:
        while idx < len(data):
            n, a, b, c, d = map(int, data[idx].split())
            idx += 1
            perms = []
            for _ in range(d):
                perms.append(list(map(int, data[idx].split())))
                idx += 1
            cases.append((n, a, b, c, perms))

    ans = []
    for case in cases:
        ans.extend(solve_case(*case))

    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Lập trình động đếm ngược lưu trữ số Mahonian. Quá trình chuyển đổi hoạt động vì việc chèn người lính lớn nhất mới vào một vị trí sẽ tạo ra chính xác số lần đảo ngược mới đã biết và mọi vị trí có thể xảy ra đều được xem xét. 

Các nhóm bộ giải điểm cố định có chi phí xác định bằng nhau thay vì lặp qua tất cả các hoán vị. Có nhiều nhất`121`số lần đảo ngược nên phần này vẫn rất nhỏ.`Fraction`chỉ được sử dụng khi giải quyết kỳ vọng tổng thể, giúp tránh các lỗi chính xác khi đầu ra được yêu cầu là một số hữu tỉ chính xác. 

Phần truy vấn cố tình không sử dụng bảng DP. Bảng chỉ cung cấp tần số trên tất cả các hoán vị. Đối với một hoán vị thực tế, chúng ta cần số lần đảo ngược chính xác của nó và`n=16`làm cho phương pháp đếm bậc hai trở nên dễ dàng và đủ nhanh để`100000`truy vấn. 

# Ví dụ đã hoạt động 

Đối với hoán vị được sắp xếp: 

| hoán vị | đảo ngược | chi phí xác định | giá trị cuối cùng | 
| --- | --- | --- | --- | 
|`1 2 3 4 5 6`| 0 | 0 |`0/1`| 

Số lần đảo ngược đã ở mức tối thiểu nên không cần thực hiện thao tác nào. 

Đối với hoán vị`5 4 3 2 1 6`: 

| hoán vị | đảo ngược | đảo ngược | sự lựa chọn xác định | 
| --- | --- | --- | --- | 
|`5 4 3 2 1 6`| 10 | 5 |`min(10a, b+5a)`| 

Với chi phí mẫu`a=b=c=1`, chi phí xác định là`6`. Tùy chọn ngẫu nhiên đắt hơn nên kết quả là`6/1`. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n^3 + d*n^2)`| quá trình tiền xử lý rất nhỏ vì số lần đảo ngược tối đa là 120 và mỗi truy vấn đều tính số lần đảo ngược | 
| Không gian |`O(n^2)`| chỉ lưu trữ phân bố đảo ngược và các mảng phụ trợ nhỏ | 

Lớn nhất có thể`n`chỉ cung cấp 121 trạng thái đếm đảo ngược. Công việc chủ yếu là xử lý các hoán vị đầu vào, điều này khả thi vì`16^2 * 100000`so sánh vẫn còn nhỏ. 

# Trường hợp thử nghiệm```
# The implementation above can be tested with these inputs.

# Sample
sample = """6 1 1 1 3
1 2 3 4 5 6
5 4 3 2 1 6
6 4 2 1 3 5
"""

# Expected:
# 0/1
# 6/1
# 2771/428
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2`, hoán vị được sắp xếp |`0/1`| trạng thái đã được giải quyết | 
|`n=2`, hoán vị ngược | tối thiểu một lần hoán đổi và một lần đảo ngược | ranh giới đảo ngược | 
|`n=16`, hoán vị ngẫu nhiên | phân số hữu hạn | xử lý kích thước tối đa | 
| giá trị chi phí giống hệt nhau`a=b=c=1`| đầu ra hợp lý chính xác | tính điểm cố định | 

# Vỏ cạnh 

Một hoán vị được giải quyết có số lần đảo ngược bằng 0. Chi phí xác định trở thành 0, do đó thuật toán trả về 0 trước khi giá trị ngẫu nhiên có thể quan trọng. 

Một hoán vị đảo ngược hoàn toàn có số lần đảo ngược`m`. Thao tác ngược lại sẽ thay đổi nó thành số nghịch đảo bằng 0, do đó công thức sẽ chọn`b`thay vì trả tiền cho tất cả các giao dịch hoán đổi liền kề. 

Trường hợp ngẫu nhiên hấp dẫn sẽ được xử lý theo điểm cố định. Nếu mọi phương án xác định đều đắt tiền thì giá trị tính toán`C`trở nên nhỏ hơn những chi phí đó và mọi hoán vị như vậy trước tiên sẽ sử dụng chính xác phép toán ngẫu nhiên. Phương trình được giải toàn cục nên tất cả các hoán vị đều có cùng kỳ vọng khởi động lại.
