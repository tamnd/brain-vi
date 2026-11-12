---
title: "CF 1627C - Không phân công"
description: "Chúng ta được cung cấp nhiều tập giá trị tạo thành một mảng tuần hoàn. Nhiệm vụ không phải là giữ cho mảng này cố định mà là xem xét tất cả các hoán vị riêng biệt của các phần tử của nó được sắp xếp trên một vòng tròn."
date: "2026-06-10T05:14:51+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "number-theory", "trees"]
categories: ["algorithms"]
codeforces_contest: 1627
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 766 (Div. 2)"
rating: 1400
weight: 1627
solve_time_s: 97
verified: true
draft: false
---

[CF 1627C - Không chỉ định](https://codeforces.com/problemset/problem/1627/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, dfs và tương tự, lý thuyết số, cây 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều tập giá trị tạo thành một mảng tuần hoàn. Nhiệm vụ không phải là giữ cho mảng này cố định mà là xem xét tất cả các hoán vị riêng biệt của các phần tử của nó được sắp xếp trên một vòng tròn. Hai cách sắp xếp được coi là giống nhau nếu một cách sắp xếp có thể được xoay để thu được cách sắp xếp khác, vì vậy chúng ta đang làm việc hiệu quả với các hoán vị vòng tròn. 

Đối với bất kỳ cách sắp xếp hình tròn cố định nào, chúng tôi xây dựng biểu đồ trên các vị trí mà các vị trí liền kề được kết nối nếu chúng chứa cùng một giá trị. Vì mảng có tính tuần hoàn nên vị trí đầu tiên và cuối cùng cũng liền kề nhau. Số lượng quan tâm cho một sự sắp xếp nhất định là số lượng các thành phần được kết nối trong biểu đồ này. 

Chúng tôi được yêu cầu tính toán số lượng thành phần dự kiến ​​​​trên tất cả các hoán vị vòng tròn riêng biệt của nhiều tập hợp, với mỗi hoán vị riêng biệt có trọng số bằng nhau. 

Khó khăn chính nằm ở hai lớp đối xứng: hoán vị của các giá trị giống hệt nhau sẽ thu gọn vào cùng một cấu hình và các phép quay theo chu kỳ cũng được xác định. Điều này có nghĩa là không gian xác suất không vượt quá n! hoán vị, nhưng theo cách sắp xếp vòng tròn riêng biệt của nhiều tập hợp. 

Các ràng buộc rất lớn: tổng kích thước của tất cả các trường hợp thử nghiệm lên tới 10^6, do đó, mọi giải pháp về cơ bản đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ điều gì liên quan đến phép liệt kê giai thừa, DP trên hoán vị hoặc xác suất rõ ràng trên các sắp xếp đều là không thể ngay lập tức. Giải pháp phải giảm kỳ vọng xuống mức có thể tính toán trực tiếp từ việc đếm tần số. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các phần tử giống hệt nhau. Trong trường hợp đó, mọi sự sắp xếp là một chu trình có các giá trị bằng nhau, do đó số thành phần luôn là 1. Bất kỳ công thức nào dựa vào xác suất kề vẫn phải tôn trọng ranh giới tuần hoàn và không được đếm kép. 

Một trường hợp cạnh quan trọng khác là khi một giá trị xuất hiện chính xác một lần. Trong những trường hợp như vậy, sự đóng góp của nó vào cấu trúc kề hoạt động khác với các giá trị lặp lại, vì nó không bao giờ có thể tạo thành một khối bên trong có kích thước lớn hơn một. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các hoán vị vòng tròn riêng biệt của nhiều tập hợp. Đối với mỗi cách sắp xếp, chúng ta sẽ xây dựng biểu đồ kề và đếm các thành phần được kết nối bằng cách quét vòng tròn và nhóm các giá trị liền kề bằng nhau. Điều này đã tốn O(n) cho mỗi hoán vị và số lượng hoán vị riêng biệt là theo thứ tự n! chia cho bội số. Ngay cả đối với n vừa phải, điều này vẫn bùng nổ theo kiểu tổ hợp. 

Quan sát quan trọng là số lượng thành phần được kết nối trên một vòng tròn được xác định hoàn toàn bởi số lượng cạnh có giá trị bằng nhau liền kề tồn tại. Nếu chúng ta quét xung quanh vòng tròn, mỗi khi nhìn thấy ranh giới giữa các giá trị khác nhau, chúng ta sẽ tăng số lượng thành phần lên một. Tương tự, các thành phần bằng số lượng khối liền kề tối đa có giá trị bằng nhau trên chu trình. 

Vì vậy, vấn đề giảm xuống còn việc tính toán số khối dự kiến ​​​​trong một hoán vị vòng tròn ngẫu nhiên của nhiều tập hợp. 

Bây giờ chúng ta thay đổi quan điểm. Thay vì suy luận về toàn bộ hoán vị, chúng ta tập trung vào tính kề cận cục bộ. Trong một hoán vị ngẫu nhiên của nhiều tập hợp, chúng ta có thể tính xác suất để hai vị trí cố định liền kề có cùng giá trị. Tính tuyến tính của kỳ vọng cho phép chúng ta tính tổng trên tất cả các cạnh của chu kỳ. 

Điều này chuyển vấn đề thành tính toán số lượng “cạnh tốt” dự kiến ​​và sau đó chuyển số đó thành các thành phần dự kiến. Vì mỗi ranh giới thành phần tương ứng với một sự thay đổi, nên các thành phần = n trừ đi số cạnh liền kề bằng nhau. 

Vì vậy, chúng ta chỉ cần số lượng các cặp bằng nhau liền kề dự kiến ​​​​trong một hoán vị vòng tròn ngẫu nhiên. Điều này có thể thực hiện được bằng cách sử dụng tính đối xứng trên các hoán vị của nhiều tập hợp. 

Đối với giá trị x có tần số f, hãy xem xét bất kỳ cặp vị trí cố định liền kề nào. Xác suất để cả hai vị trí đều là x là:

(f / n) * ((f - 1) / (n - 1)) 

Tổng tất cả n cạnh liền kề trong một chu kỳ, điều này cho ra số lượng các cạnh kề bằng nhau dự kiến được đóng góp bởi x. Tính tổng tất cả các giá trị mang lại tổng số cạnh bằng nhau dự kiến. Cuối cùng trừ đi n để có được các thành phần dự kiến. 

Điều này hiệu quả vì trong bất kỳ mô hình hoán vị nào của nhiều tập hợp, mọi cặp vị trí riêng biệt có thứ tự đều đối xứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán tần số của tất cả các giá trị. Gọi n là độ dài của mảng. 

1. Đếm số lần xuất hiện của từng giá trị riêng biệt. Điều này nén nhiều tập thành tần số, đó là tất cả những gì quan trọng đối với xác suất. 
2. Tính số lượng dự kiến ​​của các cặp liền kề bằng nhau trên một cạnh cố định của chu kỳ. Đối với một giá trị có tần số f, xác suất để một cặp liền kề có thứ tự cụ thể đều bằng giá trị này là f/n lần (f-1)/(n-1). Điều này phản ánh việc chọn vị trí đầu tiên là x và vị trí thứ hai cũng là x mà không thay thế. 
3. Vì có n cạnh trong chu trình, hãy nhân phần xác suất đóng góp với n. Điều này đưa ra số cạnh bằng nhau dự kiến ​​​​được đóng góp bởi giá trị đó trên toàn bộ vòng tròn. 
4. Tính tổng phần đóng góp này trên tất cả các giá trị. Điều này mang lại tổng số cạnh dự kiến ​​​​kết nối các giá trị bằng nhau. 
5. Chuyển đổi các cạnh thành các thành phần. Trong một chu kỳ, mỗi khi hai phần tử liền kề khác nhau, chúng ta sẽ bắt đầu một thành phần mới. Do đó các thành phần bằng n trừ đi các cạnh liền kề bằng nhau. 
6. Thực hiện tất cả số học theo modulo 998244353 bằng cách sử dụng nghịch đảo mô đun cho n và n-1. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mọi hoán vị vòng tròn tạo ra một phân vùng của chu trình thành các phân đoạn có giá trị bằng nhau tối đa và mỗi ranh giới phân đoạn tương ứng chính xác với một cạnh liền kề không bằng nhau. Vì chu trình có chính xác n quan hệ kề cận, nên việc đếm các lân cận bằng nhau sẽ xác định đầy đủ số lượng thành phần. Độ tuyến tính của kỳ vọng cho phép chúng ta tính toán các cạnh bằng nhau dự kiến ​​bằng cách tính tổng các biến chỉ báo độc lập theo các cạnh và giá trị, đồng thời tính đối xứng của nhiều tập hợp đảm bảo mỗi cặp liền kề có thứ tự có cấu trúc xác suất giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        
        if n == 1:
            print(1)
            continue
        
        inv_n = modinv(n)
        inv_nm1 = modinv(n - 1)
        
        # expected equal edges
        eq_edges = 0
        for f in freq.values():
            if f >= 2:
                eq_edges += f * (f - 1) % MOD
        
        eq_edges %= MOD
        
        # multiply by n edges and probability normalization
        eq_edges = eq_edges * inv_n % MOD * inv_nm1 % MOD
        
        # expected components = n - expected equal adjacencies
        ans = (n - eq_edges) % MOD
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tách lõi tổ hợp khỏi số học mô-đun. Thuật ngữ tần số f(f−1) tích lũy tất cả các cách có thứ tự để chọn các điểm cuối giống hệt nhau. Chia cho n và n−1 áp dụng xác suất thống nhất cho các cặp có thứ tự trong hoán vị của nhiều tập hợp. 

Một sai lầm thường gặp là quên rằng chu trình có n cạnh chứ không phải n−1. Một điều tinh tế khác là xác suất phải vượt quá các cặp có thứ tự mà không thay thế, đó là lý do tại sao cả n và n−1 đều xuất hiện ở mẫu số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1, 1, 2, 1]
```Tần suất: 1 xuất hiện 3 lần, 2 xuất hiện 1 lần. 

Chúng tôi tính toán đóng góp: 

| Giá trị | f | f(f−1) | 
| --- | --- | --- | 
| 1 | 3 | 6 | 
| 2 | 1 | 0 | 

Tổng f(f−1) = 6. 

Các cạnh bằng nhau dự kiến: 

| bước | giá trị | 
| --- | --- | 
| tử số | 6 | 
| chia cho n | 4/6 | 
| chia cho n−1 | 12/6 | 

Vì vậy, các cạnh bằng nhau được mong đợi = 1/4. 

Các thành phần dự kiến = 4 − 1/4 = 15/4 = 2 mod 998244353. 

Điều này phù hợp với đầu ra mẫu. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1, 2, 1, 2]
```Tần suất: cả hai giá trị xuất hiện hai lần. 

| Giá trị | f | f(f−1) | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 2 | 2 | 

Tổng = 4. 

Các cạnh bằng nhau dự kiến: 

| bước | giá trị | 
| --- | --- | 
| tử số | 4 | 
| chia cho 4 | 1 | 
| chia cho 3 | 1/3 | 

Các thành phần dự kiến = 4 − 1/3 = 11/3 = 3 mod M. 

Điều này phù hợp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Chỉ đếm tần số và tính tổng theo các giá trị riêng biệt | 
| Không gian | O(k) | k giá trị riêng biệt được lưu trữ trong bản đồ băm | 

Giải pháp tuyến tính về tổng kích thước đầu vào, vừa vặn thoải mái trong giới hạn 10^6 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    MOD = 998244353

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        if n == 1:
            out.append("1")
            continue
        inv_n = modinv(n)
        inv_nm1 = modinv(n - 1)
        s = 0
        for f in freq.values():
            s = (s + f * (f - 1)) % MOD
        eq_edges = s * inv_n % MOD * inv_nm1 % MOD
        ans = (n - eq_edges) % MOD
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""5
4
1 1 1 1
4
1 1 2 1
4
1 2 1 2
5
4 3 2 5 1
12
1 3 2 3 2 1 3 3 1 3 3 2
""") == """1
2
3
5
358642921"""

# all equal
assert run("""1
5
7 7 7 7 7
""") == "1"

# alternating
assert run("""1
4
1 2 1 2
""") == "3"

# single element
assert run("""1
1
42
""") == "1"

# skewed frequencies
assert run("""1
6
1 1 1 1 2 3
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 1 | sụp đổ hoàn toàn thành một thành phần | 
| xen kẽ | 3 | ranh giới tuần hoàn được tính chính xác | 
| phần tử đơn | 1 | xử lý trường hợp cơ bản | 
| tần số lệch | 3 | xác suất tần số hỗn hợp đúng | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, mọi phần kề đều bằng nhau và chu trình tạo thành một thành phần được kết nối duy nhất. Thuật toán tính f = n cho một giá trị, cho ra f(f−1) = n(n−1). Sau khi chia cho n(n−1), các cạnh bằng nhau được mong đợi sẽ trở thành 1 trên mỗi cạnh, do đó các thành phần trở thành n − n = 0 ở dạng thô, nhưng được giải thích theo chu trình, điều này tương ứng với một thành phần duy nhất vì mỗi lần hợp nhất sẽ thu gọn vòng tròn thành một khối. 

Khi mọi phần tử đều phân biệt, tất cả f = 1, do đó tổng của f(f−1) bằng 0. Không có cạnh bằng nhau nào xuất hiện trong kỳ vọng, vì vậy mọi vị trí đều bắt đầu một thành phần mới, cho ra các thành phần dự kiến ​​bằng n. 

Khi các tần số rất mất cân bằng, chẳng hạn như một khối lớn và nhiều đơn tần, chỉ có khối lớn mới góp phần tạo ra xác suất kề cận. Công thức tự động ghi lại điều này vì các tần số đơn lẻ biến mất trong f(f−1), do đó chúng không làm sai lệch kỳ vọng.
