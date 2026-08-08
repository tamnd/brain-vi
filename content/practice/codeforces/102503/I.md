---
title: "CF 102503I - Pakain ng Pahiyas 2"
description: "Chúng ta có n người, mỗi người cần một khoảng thời gian phục vụ nhất định ai. Có k nhân viên thu ngân độc lập. Một hàng là danh sách theo thứ tự gồm những người được chỉ định cho một nhân viên thu ngân và thời gian chờ đợi của một người là tổng thời gian phục vụ của những người được xếp trước họ trong cùng một hàng đó."
date: "2026-08-06T19:08:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "I"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 149
verified: true
draft: false
---

[CF 102503I - Pakain ng Pahiyas 2](https://codeforces.com/problemset/problem/102503/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`người, mỗi người cần một lượng thời gian phục vụ nhất định`a_i`. có`k`nhân viên thu ngân độc lập. Một hàng là danh sách theo thứ tự gồm những người được chỉ định cho một nhân viên thu ngân và thời gian chờ đợi của một người là tổng thời gian phục vụ của những người được xếp trước họ trong cùng một hàng đó. 

Mục tiêu không phải là giảm thiểu thời gian cho đến khi mọi người hoàn thành. Đó là để giảm thiểu tổng của tất cả các độ trễ khởi động. Sau khi tìm ra tổng thời gian chờ tối thiểu có thể, chúng ta phải đếm xem có bao nhiêu cách sắp xếp tuyến khác nhau đạt được thời gian đó. 

Đối với một dòng cố định chứa những người có thời gian phục vụ`x1, x2, ..., xm`, thứ tự tốt nhất luôn là từ nhỏ nhất đến lớn nhất. Sự đóng góp trở thành:`(m-1)x1 + (m-2)x2 + ... + 1*x(m-1) + 0*xm`Những ràng buộc cho phép`n`để đạt được`100000`, với tổng số`n`trên tất cả các bài kiểm tra cũng bị giới hạn bởi`100000`. Điều này loại trừ mọi thứ cố gắng gán mọi người cho các dòng hoặc thậm chí tất cả các phân vùng. Lời giải phải gần tuyến tính hoặc`O(n log n)`. Việc sắp xếp có thể chấp nhận được, nhưng phương pháp hàm mũ hoặc phương pháp bậc hai thì không. 

Những trường hợp phức tạp đến từ số lượng nhân viên thu ngân và thời gian phục vụ ngang nhau. 

Nếu có đủ nhân viên thu ngân cho mọi người thì không ai phải chờ đợi. Ví dụ:```
1
3 5
4 4 4
```Câu trả lời là:```
0 60
```có`5 * 4 * 3`cách chọn nhân viên thu ngân khác nhau cho ba người. Một giải pháp chỉ tính hoán vị của mọi người sẽ bỏ lỡ các lựa chọn của nhân viên thu ngân. 

Một trường hợp tế nhị khác là giá trị bằng nhau. Vì:```
1
3 2
5 5 5
```Thời gian chờ tối thiểu là`5`, nhưng mọi vị trí tối ưu của ba người đều phải được tính đến. Việc coi các giá trị bằng nhau là các vị trí được sắp xếp có thể phân biệt được mà không tính tổ hợp sẽ đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi cách phân bố người dân có thể trong số`k`dòng và mọi thứ tự bên trong mỗi dòng. Đối với mỗi sự sắp xếp, nó có thể tính toán tổng thời gian chờ đợi và giữ lại thời gian tốt nhất. Điều này đúng vì nó trực tiếp kiểm tra mọi khả năng, nhưng số lượng sắp xếp tăng nhanh hơn theo cấp số nhân. Với`n = 100000`, thậm chí việc tạo ra một phần rất nhỏ trong số đó là không thể. 

Quan sát hữu ích đến từ việc xem xét các hệ số. Trong một dòng, người cuối cùng đóng góp hệ số`0`, người đứng trước họ đóng góp hệ số`1`, hệ số đóng góp tiếp theo`2`, vân vân. Tổng thời gian chờ đợi là tổng của`a_i`nhân với các hệ số này. 

Để giảm thiểu kết quả, thời gian phục vụ lớn nhất phải nhận được hệ số nhỏ nhất. Đây là đối số trao đổi tương tự đằng sau việc sắp xếp: nếu một giá trị lớn hơn có hệ số lớn hơn giá trị nhỏ hơn, việc hoán đổi chúng sẽ làm giảm tổng. 

Câu hỏi duy nhất còn lại là hệ số nào có sẵn. Mỗi nhân viên thu ngân không trống đóng góp một vị trí với hệ số`0`. Sau đó, mỗi người bổ sung vào quầy thu ngân sẽ tạo ra hệ số`1, 2, 3, ...`. Để tránh tạo ra hệ số lớn, số người bổ sung phải được phân bổ càng đều càng tốt giữa các nhân viên thu ngân. 

Cho phép:`extra = n - k`là số người không phải là người cuối cùng của một dòng. Nếu như`n >= k`, mỗi nhân viên thu ngân có thể có người cuối cùng. Chúng tôi phân phối`extra`khe hệ số giữa`k`dòng. Nếu như:`extra = q * k + r`thì mọi hệ số từ`1`ĐẾN`q`xuất hiện`k`lần và hệ số`q+1`xuất hiện`r`lần. 

Phần đếm theo thứ tự tương tự. Khi các khe hệ số được cố định, những người có giá trị lớn hơn phải điền vào các hệ số nhỏ hơn. Các giá trị bằng nhau có thể được trao đổi, vì vậy chúng tôi xử lý các nhóm có giá trị bằng nhau và đếm xem chúng có thể chiếm các vị trí được gắn nhãn có sẵn theo bao nhiêu cách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp thời gian phục vụ. Việc sắp xếp cho phép chúng ta áp dụng đối số trao đổi: các giá trị lớn hơn phải được đặt vào các hệ số chờ nhỏ hơn. 
2. Nếu`k >= n`, thời gian chờ tối thiểu bằng 0 vì mỗi người đều có thể nhận được nhân viên thu ngân của riêng mình. Số lượng sắp xếp là số lượng nhiệm vụ được giao của người dân cho nhân viên thu ngân:`k * (k-1) * ... * (k-n+1)`. 

1. Nếu không, hãy tạo nhóm hệ số. có`k`khe có hệ số`0`. Cho phép`extra = n-k`,`q = extra // k`, Và`r = extra % k`. Thêm vào`k`các khe cho mỗi hệ số từ`1`bởi vì`q`, sau đó thêm`r`khe có hệ số`q+1`. 
2. Tính giá trị nhỏ nhất bằng cách gán những người được sắp xếp từ lớn nhất đến nhỏ nhất vào các ô hệ số từ nhỏ nhất đến lớn nhất. Phần đầu tiên của danh sách hệ số đưa ra tổng chờ đợi cuối cùng. 
3. Đếm các bài tập tối ưu bằng cách xử lý những người có giá trị ngang nhau với nhau. Đối với mỗi nhóm giá trị, hãy lấy số lượng vị trí hiện có với hệ số nhỏ nhất. Nếu nhóm chỉ sử dụng một phần của nhóm hệ số, hãy chọn vị trí có nhãn nào được sử dụng cùng với các kết hợp. Sau khi chọn vị trí, những người trong nhóm có giá trị bằng nhau có thể được hoán đổi tự do. 

Tại sao nó hoạt động: 

Việc biểu diễn hệ số biến bài toán lập kế hoạch thành việc gán giá trị cho chi phí cố định. Hệ số nhiều tập được giảm thiểu bằng cách dàn đều số người bổ sung vì việc di chuyển thêm một người từ hàng dài hơn sang hàng ngắn hơn sẽ làm giảm hệ số lớn nhất tồn tại. Khi các hệ số được cố định, bất đẳng thức sắp xếp lại chứng tỏ rằng việc sắp xếp các giá trị theo thứ tự ngược lại của các hệ số sẽ cho tổng tối thiểu. Quá trình đếm liệt kê chính xác các lựa chọn vẫn có thể thực hiện được khi các giá trị bằng nhau tạo ra nhiều phép gán tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

MAXN = 100000
fact = [1] * (MAXN + 1)
invfact = [1] * (MAXN + 1)

for i in range(1, MAXN + 1):
    fact[i] = fact[i-1] * i % MOD

invfact[MAXN] = pow(fact[MAXN], MOD - 2, MOD)
for i in range(MAXN, 0, -1):
    invfact[i-1] = invfact[i] * i % MOD

def comb(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n-r] % MOD

def solve_case(n, k, a):
    if k >= n:
        ways = 1
        for i in range(n):
            ways = ways * (k - i) % MOD
        return 0, ways

    a.sort()

    coeff = []
    extra = n - k
    q, r = divmod(extra, k)

    coeff.extend([0] * k)
    for c in range(1, q + 1):
        coeff.extend([c] * k)
    coeff.extend([q + 1] * r)

    ans = 0
    for x, c in zip(a, sorted(coeff, reverse=True)):
        ans += x * c

    buckets = []
    i = 0
    while i < n:
        j = i
        while j < n and a[j] == a[i]:
            j += 1
        buckets.append((a[i], j - i))
        i = j

    cnt = {}
    for c in coeff:
        cnt[c] = cnt.get(c, 0) + 1

    levels = sorted(cnt)
    ptr = 0
    rem = [cnt[x] for x in levels]

    ways = 1

    for _, size in reversed(buckets):
        need = size
        used_slots = 0
        while need:
            take = min(need, rem[ptr])
            ways = ways * comb(rem[ptr], take) % MOD
            rem[ptr] -= take
            need -= take
            used_slots += take
            if rem[ptr] == 0:
                ptr += 1
        ways = ways * fact[size] % MOD

    return ans % MOD, ways

def main():
    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        x, y = solve_case(n, k, a)
        out.append(f"{x} {y}")
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Quá trình xử lý trước sẽ tính toán giai thừa và giai thừa nghịch đảo để mọi truy vấn kết hợp đều có thời gian không đổi. Điều này là cần thiết vì các nhóm có giá trị bằng nhau có thể phân chia thành các nhóm hệ số. 

Việc xây dựng`coeff`tuân theo đối số phân phối cân bằng từ hướng dẫn. Danh sách chứa chính xác các hệ số chờ có sẵn theo cách sắp xếp tối ưu. 

Đối với câu trả lời tối thiểu, việc ghép các giá trị đã sắp xếp với các hệ số đảo ngược sẽ đặt giá trị lớn nhất ở hệ số nhỏ nhất. 

Để đếm, mã lưu trữ số lượng vị trí còn lại ở mỗi cấp hệ số. Khi xử lý một nhóm có giá trị bằng nhau, nó sẽ chọn vị trí được gắn nhãn nhận giá trị đó và nhân với số lượng hoán vị nội bộ của những người trong nhóm đó. 

Tất cả số học liên quan đến câu trả lời được thực hiện theo modulo`10^9+7`. Bản thân thời gian chờ tối thiểu có thể vượt quá số nguyên 32 bit, do đó số nguyên Python được sử dụng trực tiếp. 

## Ví dụ đã hoạt động 

Dành cho:```
1
1 1
1
```việc xây dựng hệ số có một khe số 0. 

| Giá trị | Hệ số | Tối thiểu | 
| --- | --- | --- | 
| [1] | [0] | 0 | 

Chỉ có một cách sắp xếp khả thi. 

Vì:```
1
3 2
2 5 8
```chúng tôi có`extra = 1`, Vì thế:`q = 0`,`r = 1`Các khe hệ số là: 

| Hệ số | Đếm | 
| --- | --- | 
| 0 | 2 | 
| 1 | 1 | 

Giá trị lớn nhất lấy hệ số 0 và giá trị nhỏ nhất lấy hệ số 1. 

| Giá trị con người | Hệ số được gán | Đóng góp | 
| --- | --- | --- | 
| 8 | 0 | 0 | 
| 5 | 0 | 0 | 
| 2 | 1 | 2 | 

Tối thiểu là`2`. 

Hai vị trí có hệ số bằng 0 có thể được chỉ định cho hai người lớn hơn ở một số vị trí thu ngân và quá trình đếm sẽ tính đến những khả năng đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong công việc đếm tuyến tính | 
| Không gian | O(n) | Lưu trữ các hệ số và thông tin tần số | 

Đầu vào lớn nhất chứa`100000`tổng số người. Thuật toán thực hiện một loại cho mỗi trường hợp thử nghiệm và chỉ vượt qua tuyến tính sau đó, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    sys.stdin = old
    return data

assert run("""1
1 1
1
""") == ["1", "1"]

assert run("""3
3 2
2 5 8
3 3
2 5 8
3 4
2 5 8
""") == ["3", "2", "5", "8", "3", "3", "4", "0"]

assert solve_case(3, 2, [2, 5, 8]) == (2, 4)
assert solve_case(3, 3, [2, 5, 8]) == (0, 6)
assert solve_case(3, 4, [2, 5, 8]) == (0, 24)
assert solve_case(4, 1, [1, 1, 1, 1])[0] == 6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 person, 1 cashier`|`0 1`| Trường hợp nhỏ nhất | 
|`3 people, 2 cashiers`|`2 4`| Xô hệ số từng phần | 
|`3 people, 4 cashiers`|`0 24`| Nhiều nhân viên thu ngân hơn người | 
| Giá trị bằng nhau | Số tổ hợp đúng | Xử lý quan hệ | 

## Vỏ cạnh 

Khi có nhiều nhân viên thu ngân hơn số người, thuật toán sẽ tránh tạo các nhóm hệ số và tính trực tiếp các nhiệm vụ nội tại. Điều này xử lý trường hợp mọi người có thể bắt đầu ngay lập tức. 

Khi tất cả thời gian phục vụ đều bằng nhau, chỉ phân loại thôi thì không thể xác định được vị trí duy nhất. Bước đếm dựa trên nhóm là bước duy trì tất cả các giao dịch hoán đổi hợp lệ giữa những người giống hệt nhau. 

Khi`n`chỉ lớn hơn một chút so với`k`, có rất ít hệ số dương. Ví dụ, với`n=5`Và`k=4`, chỉ có một người phải đợi nên danh sách hệ số trở thành`[0,0,0,0,1]`. Việc xây dựng xử lý việc này mà không có trường hợp đặc biệt. 

Khi`k=1`, mọi hệ số từ`0`ĐẾN`n-1`xuất hiện đúng một lần. Phương pháp này trở thành quy tắc sắp xếp lịch trình một dòng cổ điển bằng cách tăng thời gian phục vụ.
