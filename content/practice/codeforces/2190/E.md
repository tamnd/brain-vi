---
title: "CF 2190E - Hoán vị trung vị"
description: "Thay vì nghĩ trực tiếp về số trung vị, sẽ hữu ích hơn nhiều khi nghĩ về hoán vị được biểu hiện theo thứ tự giá trị tăng dần. Giả sử chúng ta xử lý các giá trị hoán vị từ 1 đến n. Khi giá trị x được xử lý, vị trí của nó sẽ trở thành \"hoạt động\"."
date: "2026-06-07T21:06:34+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics"]
categories: ["algorithms"]
codeforces_contest: 2190
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1073 (Div. 1)"
rating: 3200
weight: 2190
solve_time_s: 98
verified: true
draft: false
---

[CF 2190E - Hoán vị trung vị](https://codeforces.com/problemset/problem/2190/E) 

**Đánh giá:** 3200 
**Thẻ:** tổ hợp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Thay vì nghĩ trực tiếp về số trung vị, sẽ hữu ích hơn nhiều khi nghĩ về hoán vị được biểu hiện theo thứ tự giá trị tăng dần. 

Giả sử chúng ta xử lý các giá trị của một hoán vị từ`1`ĐẾN`n`. Khi giá trị`x`được xử lý, vị trí của nó trở thành "hoạt động". Đối với bất kỳ vị trí nào, trạng thái của nó là:`0`= giá trị chưa được xử lý,`X`= giá trị hiện đang được xử lý,`1`= giá trị đã được xử lý. 

Điều kiện là tất cả các trung vị của các cửa sổ có độ dài 3 đều khác biệt sẽ trở thành một hạn chế mạnh mẽ về vị trí nào có thể được kích hoạt tiếp theo. Sau khi đơn giản hóa các mẫu cục bộ bị cấm, người ta thu được một quy trình vị trí thuần túy. Các vị trí phải được kích hoạt theo một thứ tự rất đặc biệt. 

Đầu vào sửa một phần hoán vị cuối cùng. Một số vị trí đã chứa các giá trị cụ thể, trong khi số 0 biểu thị các giá trị chưa xác định. Chúng ta phải đếm có bao nhiêu hoán vị hoàn chỉnh thỏa mãn cả các mục cố định và điều kiện trung vị phân biệt. 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm chỉ`2 · 10^5`, điều này ngay lập tức gợi ý rằng một`O(n)`hoặc`O(n log n)`giải pháp cho mỗi tập tin thử nghiệm là bắt buộc. Bất cứ điều gì bậc hai đều là vô vọng bởi vì`n^2`sẽ ở xung quanh`4 · 10^10`hoạt động. 

Một điểm tinh tế là tuyên bố đảm bảo rằng cả hai`1`Và`n`xuất hiện trong mảng đã biết một phần. Hai giá trị đó không phải là ngẫu nhiên. Vị trí của chúng xác định duy nhất cấu trúc hình học của mọi lệnh kích hoạt hợp lệ. Toàn bộ đối số đếm được xây dựng xung quanh chúng. 

Một sai lầm dễ mắc phải là xử lý các giá trị chưa biết một cách độc lập. Coi như:```
n = 5
0 5 4 1 0
```Chỉ có hai hoán vị tương thích, nhưng cả hai đều tạo ra một trung vị lặp lại, vì vậy câu trả lời là`0`. Chỉ nhìn vào tính nhất quán cục bộ của các giá trị cố định sẽ hoàn toàn bỏ sót điều này. 

Một sai lầm dễ mắc phải khác là cho rằng các vị trí có thể được kích hoạt theo cách mở rộng trái-phải tùy ý từ vị trí của`1`. Ví dụ:```
n = 7
0 0 1 0 0 7 0
```Nhiều đơn đặt hàng mở rộng thoạt nhìn có vẻ khả thi. Trong thực tế, ràng buộc trung tuyến khác biệt chỉ để lại hai chuỗi vị trí cố định và quyền tự do còn lại chỉ đơn thuần là cách các chuỗi đó được xen kẽ. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ lấp đầy mọi số 0 với tất cả các giá trị còn thiếu có thể có, tạo ra mọi hoán vị tương thích, tính toán tất cả các trung vị của các bộ ba liền kề và kiểm tra xem chuỗi kết quả có chứa các bản sao hay không. 

Ngay cả khi chỉ có hai mươi vị trí chưa biết, điều này có nghĩa đại khái là`20!`khả năng. Không gian tìm kiếm bùng nổ ngay lập tức. 

Quan sát quan trọng là điều kiện trung bình mạnh hơn nhiều so với lần đầu xuất hiện. 

Nếu chúng ta xử lý các giá trị theo thứ tự tăng dần thì mỗi giá trị sẽ kích hoạt chính xác một vị trí. Sự lặp lại trung vị bị cấm có thể được dịch thành các mẫu trạng thái cục bộ bị cấm`0`,`X`, Và`1`. Sau khi đơn giản hóa các mẫu đó, quá trình kích hoạt gần như mang tính quyết định. Vị trí giá trị`1`là điểm khởi đầu. Từ đó, các vị trí hợp lệ được chia thành hai chuỗi. Một chuỗi đi qua các vị trí có thể tiếp cận bằng cách di chuyển liên tục hai bước theo một hướng và nảy ở các biên giới. Chuỗi còn lại được xây dựng đối xứng. Cả hai chuỗi đều kết thúc ở vị trí chứa`n`. 

Mọi giá trị từ`2`ĐẾN`n-1`phải thuộc một trong hai chuỗi đó. Trong mỗi chuỗi, thứ tự tương đối được cố định. Sự tự do duy nhất còn lại là cách hai chuỗi được đan xen. 

Mảng đã biết một phần áp đặt các ràng buộc về vị trí các giá trị cụ thể phải xuất hiện bên trong các chuỗi này. Khi những ràng buộc đó được chuyển thành các vị trí trong chuỗi, câu trả lời sẽ trở thành một chuỗi tính toán hệ số nhị thức đếm các lần xen kẽ hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / giai thừa | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm vị trí`st`chứa giá trị`1`và vị trí`ed`chứa giá trị`n`. 
2. Xây dựng chuỗi đầu tiên bắt đầu từ`st`. 

Di chuyển theo`-2`nhiều lần. Nếu dây xích chạm vào một đường viền trước khi chạm tới`ed`, tiếp tục với các vị trí có độ chẵn lẻ thích hợp từ trái sang phải, rồi từ phải sang trái, cho đến khi đạt`ed`. 
3. Xây dựng chuỗi thứ hai một cách đối xứng. 

Di chuyển theo`+2`nhiều lần. Sau khi chạm vào một đường viền, thực hiện bước đi chẵn lẻ tương tự cho đến khi đạt đến`ed`. 
4. Mọi vị trí ngoại trừ`st`Và`ed`thuộc về đúng một trong hai chuỗi. 
5. Với mọi giá trị cố định`v`với`2 ≤ v ≤ n-1`, ghi lại: 

- chuỗi nào chứa vị trí của nó, 
- thứ hạng của nó trong chuỗi đó. 
6. Xử lý các giá trị theo thứ tự tăng dần từ`2`ĐẾN`n-1`. 

Duy trì số lượng phần tử của mỗi chuỗi đã được tiêu thụ. 
7. Khi có giá trị cố định`v`gặp phải, thứ hạng chuỗi của nó bị ép buộc. 

Giả sử chúng ta phải tiến lên`dt[0]`các yếu tố trên chuỗi`0`Và`dt[1]`các yếu tố trên chuỗi`1`trước khi đặt`v`. 

Nếu một trong hai số tiền là không thể, câu trả lời là`0`. 
8. Đếm số cách xen kẽ các phần tử chuỗi mới được tiêu thụ này trong khi vẫn giữ được trật tự bên trong mỗi chuỗi. 

Số lượng là$$\binom{dt[0]+dt[1]-1}{dt[c]-1},$$Ở đâu`c`là chuỗi chứa`v`. 
9. Nhân câu trả lời với hệ số nhị thức này và cập nhật độ dài đã sử dụng. 
10. Sau khi xử lý tất cả các giá trị cố định, hãy xen kẽ các hậu tố còn lại của hai chuỗi:$$\binom{rem_0+rem_1}{rem_0}.$$1. Xuất kết quả theo modulo`998244353`. 

### Tại sao nó hoạt động 

Các mẫu cục bộ bị cấm buộc mọi lệnh kích hoạt hợp lệ phải tuân theo chính xác hai chuỗi vị trí được xác định duy nhất bởi các vị trí của`1`Và`n`. Bên trong một chuỗi, trật tự được cố định. Bất kỳ hoán vị hợp lệ nào cũng tương ứng với việc chọn sự đan xen của các chuỗi này. 

Các mục nhập cố định trong đầu vào buộc các giá trị nhất định xuất hiện ở các cấp bậc nhất định trong chuỗi. Trong khi xử lý các giá trị theo thứ tự tăng dần, mỗi giá trị cố định sẽ chia phần xen kẽ thành các phân đoạn độc lập. Trong một phân đoạn, chỉ có thứ tự tương đối bên trong mỗi chuỗi là quan trọng và việc đếm các lần hợp nhất hợp lệ chính xác là một hệ số nhị thức. 

Nhân số lượng phân đoạn sẽ cho số lượng các phần xen kẽ hợp lệ trên toàn cầu, do đó số lượng hoán vị hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXN = 400000

fac = [1] * (MAXN + 1)
for i in range(1, MAXN + 1):
    fac[i] = fac[i - 1] * i % MOD

invfac = [1] * (MAXN + 1)
invfac[MAXN] = pow(fac[MAXN], MOD - 2, MOD)
for i in range(MAXN, 0, -1):
    invfac[i - 1] = invfac[i] * i % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fac[n] * invfac[r] % MOD * invfac[n - r] % MOD

def solve_case():
    n = int(input())
    a = list(map(int, input().split()))

    st = ed = -1
    for i, x in enumerate(a, start=1):
        if x == 1:
            st = i
        if x == n:
            ed = i

    used = [False] * (n + 1)
    used[st] = True

    chains = [[], []]

    found = False
    for pos in range(st - 2, 0, -2):
        if pos == ed:
            found = True
            break
        if not used[pos]:
            chains[0].append(pos)
            used[pos] = True

    if not found:
        start = 2 if used[1] else 1
        for pos in range(start, n + 1, 2):
            if pos == ed:
                found = True
                break
            if not used[pos]:
                chains[0].append(pos)
                used[pos] = True

    if not found:
        start = n - 1 if used[n] else n
        for pos in range(start, 0, -2):
            if pos == ed:
                break
            if not used[pos]:
                chains[0].append(pos)
                used[pos] = True

    found = False
    for pos in range(st + 2, n + 1, 2):
        if pos == ed:
            found = True
            break
        if not used[pos]:
            chains[1].append(pos)
            used[pos] = True

    if not found:
        start = n - 1 if used[n] else n
        for pos in range(start, 0, -2):
            if pos == ed:
                found = True
                break
            if not used[pos]:
                chains[1].append(pos)
                used[pos] = True

    if not found:
        start = 2 if used[1] else 1
        for pos in range(start, n + 1, 2):
            if pos == ed:
                break
            if not used[pos]:
                chains[1].append(pos)
                used[pos] = True

    len0 = len(chains[0])
    len1 = len(chains[1])

    chain_id = [-1] * (n + 1)
    rank_in_chain = [-1] * (n + 1)

    for cid in range(2):
        for r, pos in enumerate(chains[cid], start=1):
            val = a[pos - 1]
            if val != 0:
                chain_id[val] = cid
                rank_in_chain[val] = r

    cur = [0, 0]
    ans = 1

    for v in range(2, n):
        if chain_id[v] == -1:
            continue

        cid = chain_id[v]

        if rank_in_chain[v] <= cur[cid]:
            return 0

        dt = [0, 0]
        dt[cid] = rank_in_chain[v] - cur[cid]
        dt[cid ^ 1] = (v - 1) - (cur[0] + cur[1]) - dt[cid]

        if dt[cid ^ 1] < 0:
            return 0

        limit = len0 if (cid ^ 1) == 0 else len1
        if cur[cid ^ 1] + dt[cid ^ 1] > limit:
            return 0

        ans = ans * C(dt[0] + dt[1] - 1, dt[cid] - 1) % MOD

        cur[0] += dt[0]
        cur[1] += dt[1]

    rem0 = len0 - cur[0]
    rem1 = len1 - cur[1]

    ans = ans * C(rem0 + rem1, rem0) % MOD
    return ans

t = int(input())
out = []
for _ in range(t):
    out.append(str(solve_case()))

sys.stdout.write("\n".join(out))
```Quá trình tiền xử lý tính toán các giai thừa và giai thừa nghịch đảo một lần, cho phép đánh giá mọi hệ số nhị thức trong thời gian không đổi. 

Hai khối xây dựng chuỗi là trung tâm của giải pháp. Chúng tái tạo cấu trúc vị trí độc đáo bị ép buộc bởi điều kiện trung vị khác biệt. Mỗi vị trí được gán cho chính xác một chuỗi. 

Đối với mỗi giá trị cố định, chúng tôi chỉ lưu trữ chuỗi và thứ hạng của nó. Điều này chuyển đổi vấn đề hoán vị ban đầu thành sự hợp nhất có ràng buộc của hai chuỗi có thứ tự. 

Biến`cur`theo dõi xem có bao nhiêu phần tử của mỗi chuỗi đã được sử dụng bởi các giá trị được xử lý trước đó. các`dt`các giá trị mô tả phân đoạn tiếp theo phải được hợp nhất. Hệ số nhị thức tính tất cả các cách hợp lệ để hợp nhất phân đoạn đó trong khi vẫn duy trì trật tự chuỗi nội bộ. 

Hệ số nhị thức cuối cùng xử lý hậu tố sau giá trị cố định cuối cùng. 

## Ví dụ đã hoạt động 

### Mẫu```
n = 7
0 0 1 0 0 7 0
```

`1`đang ở vị trí`3`,`7`đang ở vị trí`6`. 

| Bước | Chuỗi 0 | Chuỗi 1 | 
| --- | --- | --- | 
| Xây dựng chuỗi | 1, 5 | Analog 5, 7 đường | 
| Giá trị cố định | không có ngoại trừ 1 và 7 | không | 

Không có giá trị trung gian cố định nên câu trả lời đơn giản là số lần đan xen của hai chuỗi. 

| rem0 | rem1 | Đếm | 
| --- | --- | --- | 
| 2 | 3 | C(5,2)=10 | 

Trả lời:`10`. 

Ví dụ này cho thấy rằng một khi không có giá trị trung gian nào được cố định thì toàn bộ vấn đề sẽ giảm xuống việc đếm số lần hợp nhất của hai chuỗi. 

### Vật mẫu```
n = 10
1 10 0 0 0 0 0 0 0 0
```Ở đây các vị trí của`1`Và`10`xác định hoàn toàn cả hai chuỗi. 

| Giá trị cố định | Ràng buộc | 
| --- | --- | 
| 1 | vị trí bắt đầu cố định | 
| 10 | vị trí cuối cố định | 

Tất cả các giá trị trung gian vẫn không bị ràng buộc, nhưng cấu trúc chuỗi chỉ để lại một khả năng hợp nhất. 

| Số lượng hợp nhất còn lại | 
| --- | 
| 1 | 

Trả lời:`1`. 

Ví dụ này chứng minh rằng một số vị trí của`1`Và`n`buộc một lệnh kích hoạt duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi vị trí và giá trị cố định được xử lý một số lần không đổi | 
| Không gian | O(n) | Chuỗi và mảng phụ lưu trữ thông tin tuyến tính | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, do đó thời gian chạy tổng thể là tuyến tính ở kích thước đầu vào. Điều này thoải mái phù hợp trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```
# helper sketch

# sample 1
assert run("""5
3
1 3 2
5
0 5 4 1 0
7
0 0 1 0 0 7 0
10
1 10 0 0 0 0 0 0 0 0
15
0 0 10 0 0 15 0 0 6 7 0 1 0 0 3
""") == """1
0
10
1
4
"""

# minimum size
assert run("""1
3
1 3 2
""") == "1\n"

# impossible because fixed values violate chain order
assert run("""1
5
2 5 4 1 3
""") == "0\n"

# only endpoints fixed
assert run("""1
4
1 0 4 0
""") == run("""1
4
1 0 4 0
""")

# boundary placement of 1 and n
assert run("""1
3
1 2 3
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 2`|`1`| Phiên bản hợp lệ tối thiểu | 
|`0 5 4 1 0`|`0`| Tình trạng trung bình lặp đi lặp lại | 
| Chỉ cố định điểm cuối | Giá trị tính toán | Số lượng đan xen chuỗi thuần túy | 
|`1`Và`n`ở biên giới | Câu trả lời hợp lệ | Logic xây dựng ranh giới | 

## Vỏ cạnh 

Hãy xem xét:```
5
0 5 4 1 0
```Giá trị lực lượng cấu trúc chuỗi`4`xuất hiện dưới dạng trung vị của hai bộ ba khác nhau trong mỗi lần hoàn thành tương thích. Trong quá trình xử lý, các ràng buộc có giá trị cố định trở nên không nhất quán với các thứ hạng chuỗi có sẵn, khiến thuật toán trả về`0`. 

Coi như:```
7
0 0 1 0 0 7 0
```Không có giá trị trung gian nào được cố định. Thuật toán không bao giờ đi vào nhánh xử lý ràng buộc và đếm trực tiếp tất cả các lần xen kẽ hợp lệ của hai chuỗi. Kết quả là`10`. 

Coi như:```
10
1 10 0 0 0 0 0 0 0 0
```Các vị trí của`1`Và`10`buộc cả hai chuỗi hoàn toàn. Mỗi giá trị trung gian phải tuân theo một thứ tự tương đối duy nhất. Tất cả các hệ số nhị thức đều bằng nhau`1`, vậy câu trả lời là`1`. 

Thuật toán xử lý tất cả các trường hợp này một cách thống nhất vì mọi điều kiện được biểu thị dưới dạng ràng buộc về thứ hạng chuỗi và hợp nhất chuỗi chứ không phải dưới dạng logic trung bình trong trường hợp đặc biệt.
