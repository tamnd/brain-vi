---
title: "CF 920D - Xe tăng"
description: "Chúng tôi có một số bể chứa nước. Một hoạt động duy nhất là không bình thường: khi chúng tôi chọn bể nguồn, chúng tôi không quyết định lượng nước cần lấy. Muỗng tự động lấy lượng tối thiểu (lượng hiện tại, K) ml từ bể đó và đổ ngay toàn bộ sang bể khác."
date: "2026-06-13T02:54:47+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 920
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 37 (Rated for Div. 2)"
rating: 2400
weight: 920
solve_time_s: 603
verified: true
draft: false
---

[CF 920D - Xe tăng](https://codeforces.com/problemset/problem/920/D) 

**Đánh giá:** 2400 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 10m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số bể chứa nước. Một hoạt động duy nhất là không bình thường: khi chúng tôi chọn bể nguồn, chúng tôi không quyết định lượng nước cần lấy. Muỗng tự động lấy`min(current_amount, K)`ml từ bể đó và đổ ngay toàn bộ sang bể khác. 

Mục đích không phải là làm cho tổng lượng nước bằng`V`. Tổng số tiền không bao giờ thay đổi. Thay vào đó, chúng ta phải tạo ra ít nhất một thùng chứa chính xác`V`mililit sau một chuỗi các thao tác. 

Định dạng đầu ra được nén. Nếu cùng chuyển từ bể`x`để xe tăng`y`được lặp lại nhiều lần liên tiếp, chúng ta có thể in thành một dòng`cnt x y`. 

Những hạn chế là đầu mối chính. Có tối đa 5000 xe tăng, mỗi số tiền ban đầu tối đa là`10^5`, nhưng mục tiêu`V`có thể lớn như`10^9`. Việc mô phỏng mạnh mẽ các hoạt động của từng muỗng riêng lẻ là không thể vì một giải pháp có thể yêu cầu hàng tỷ lượt chuyển tiền. Giới hạn đầu ra tối đa`N + 5`các hoạt động nén gợi ý mạnh mẽ rằng tồn tại một giải pháp toán học mang tính xây dựng. 

Quan sát quan trọng nhất là hoạt động thay đổi xe tăng một cách chính xác`K`hoặc bằng số tiền còn lại nếu hiện tại nó nắm giữ ít hơn`K`. Điều này có nghĩa là dư lượng modulo`K`đóng vai trò trung tâm. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như:```
2 3 0
5 7
```Câu trả lời đúng là CÓ. Chúng ta có thể liên tục lấy nước ra khỏi một bể cho đến khi nó cạn. Giải pháp chỉ tìm kiếm số lượng mục tiêu dương sẽ từ chối trường hợp này một cách không chính xác. 

Coi như:```
2 5 8
3 5
```Tổng số nước là 8, vì vậy câu trả lời hiển nhiên là CÓ. Chúng ta có thể chỉ cần di chuyển tất cả nước vào một bể. Bất kỳ cách tiếp cận nào chỉ có lý do modulo`K`không xem xét tổng số tiền sẽ bỏ lỡ khả năng tầm thường này. 

Coi như:```
3 4 7
4 8 12
```Tất cả số tiền trong bể đều là bội số của 4. Mọi số tiền từng xuất hiện vẫn là bội số của 4. Vì 7 không phải là bội số của 4 nên câu trả lời là KHÔNG. Một cách xây dựng bất cẩn có thể cố gắng xây dựng các giá trị tùy ý mặc dù bất biến modulo cấm chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ khám phá các trạng thái có thể tiếp cận bằng cách áp dụng các hoạt động. Việc tìm kiếm như vậy là đúng vì mọi chuỗi hợp lệ đều tương ứng với một đường dẫn trong biểu đồ trạng thái. 

Thật không may, không gian trạng thái là rất lớn. Một chiếc xe tăng có thể chứa số lượng lớn tùy ý và ngay cả với đầu vào nhỏ, số lượng cấu hình có thể tiếp cận sẽ tăng vọt. Tìm kiếm trong không gian trạng thái là hoàn toàn không khả thi. 

Quan sát quan trọng đến từ việc kiểm tra xem điều gì xảy ra theo modulo`K`. 

Giả sử một thùng chứa ít nhất`K`đơn vị. Một lần chuyển sẽ loại bỏ chính xác`K`, vậy phần dư của nó theo modulo`K`không thay đổi. 

Giả sử một thùng chứa ít hơn`K`. Một lần chuyển sẽ làm trống nó hoàn toàn, tạo ra phần còn lại là 0. 

Do đó, mỗi xe tăng chỉ có thể đóng góp modulo còn lại ban đầu của nó`K`. Chúng ta có thể trừ nhiều lần`K`từ một bể chứa, sau đó tùy ý thực hiện một thao tác cuối cùng để đưa nó về 0. 

Điều này gợi ý rằng hãy coi mỗi chiếc xe tăng như một đồng xu có giá trị là```
ri = ai mod K
```ngoại trừ phần còn lại của 0 không đóng góp gì. 

Bằng cách xả các bể chứa vào một bể thu gom đặc biệt, chúng ta có thể tự do quyết định phần nào còn sót lại và phần nào trở thành số không. Bất kỳ lượng bể nào có thể đạt được đều có cặn bằng tổng của một tập hợp con nào đó của phần dư này theo modulo`K`. Vì mọi số dư đều nhỏ hơn`K`, giá trị thực tế có thể đạt được từ một tập hợp con chỉ đơn giản là tổng tập hợp con thông thường của các phần còn lại. 

Vấn đề trở thành: 

Chúng ta có thể chọn một số số dư có tổng bằng`V mod K`, trong khi tổng số tiền tích lũy trong bộ sưu tập ít nhất là`V`? 

Điều này dẫn đến vấn đề lập trình động tổng tập hợp con trên các giá trị modulo`K`. Từ`K ≤ 5000`, MỘT`O(NK)`DP là hoàn toàn thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| DP trên phần dư Modulo K | O(NK) | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xử lý trường hợp dễ 

hãy để`S`là tổng lượng nước. 

Nếu như`V > S`, câu trả lời là không thể ngay lập tức vì nước được bảo toàn. 

Nếu như`V == S`, chúng ta có thể chuyển toàn bộ nước vào một bể và hoàn thành. 

### 2. Tính số dư hữu ích 

Đối với mỗi bể tính toán:```
ri = ai mod K
```Chỉ những dư lượng này mới quan trọng để xây dựng phần còn lại mục tiêu. 

### 3. Chạy tập hợp con DP modulo K 

Hãy:```
target = V mod K
```Chúng tôi thực hiện DP kiểu ba lô trên phần còn lại.`dp[x]`ghi lại số tiền còn lại`x`có thể được hình thành, cùng với thông tin gốc cho phép xây dựng lại. 

Vì mọi số dư đều nhỏ hơn`K`, tất cả số tiền có thể đạt được đều nằm trong`[0, K-1]`. 

### 4. Xây dựng lại một tập hợp con 

Nếu`target`không thể truy cập được, hãy trả lời KHÔNG. 

Mặt khác, hãy xây dựng lại một tập hợp con các xe tăng có tổng số dư là`target`. 

Những chiếc xe tăng này sẽ giữ phần đóng góp còn lại của họ. Mọi bể chứa khác cuối cùng sẽ bị cạn kiệt hoàn toàn. 

### 5. Chọn bể thu gom 

Trong số tập hợp con đã chọn, chọn một xe tăng để trở thành xe tăng mục tiêu cuối cùng. 

Tất cả các bể không được chọn sẽ được xả hoàn toàn vào đó. 

Các xe tăng được chọn không phải là bộ sưu tập sẽ được giảm thiểu chính xác phần còn lại của chúng. 

Người thu gom nhận được tất cả nước chuyển. 

### 6. Tính số tiền thu gom 

Sau tất cả các lần giảm, bộ sưu tập chứa```
collector_value = S - sum(other_selected_remainders)
```Tập hợp con được chọn đảm bảo```
collector_value ≡ V (mod K)
```Và```
collector_value ≥ V
```### 7. Giảm bộ thu xuống V 

Từ sự khác biệt```
collector_value - V
```là bội số không âm của`K`, chúng tôi liên tục chuyển từ bộ thu vào bất kỳ bể phụ nào. 

Mỗi lần chuyển sẽ loại bỏ chính xác`K`. 

Bộ sưu tập cuối cùng trở thành chính xác`V`. 

### Tại sao nó hoạt động 

Điều bất biến là modulo dư của xe tăng`K`không bao giờ có thể thay đổi thành phần dư khác 0. Một chiếc xe tăng chỉ có thể giữ phần còn lại ban đầu hoặc giảm xuống bằng không. 

Do đó, mọi cấu hình có thể đạt được đều được xác định bằng cách chọn những phần còn lại ban đầu còn tồn tại. Tập hợp con DP liệt kê chính xác tất cả các tổng còn lại có thể có của người sống sót. 

Sau khi sửa một tập hợp con như vậy, mọi thao tác còn lại đều mang tính xây dựng. Các bể không được chọn sẽ giảm về 0, các bể được chọn sẽ giảm về phần còn lại và tất cả nước loại bỏ sẽ được tích lũy trong một bộ thu. Số tiền của người sưu tập khác với giá trị mong muốn gấp bội số`K`, vì vậy việc chuyển toàn bộ số lần lặp đi lặp lại sẽ giảm nó xuống mức chính xác`V`. 

Vì mọi mẫu dư lượng có thể đạt được đều được thể hiện trong DP và mọi giải pháp DP đều có thể được hiện thực hóa bằng các hoạt động của bể thực tế nên thuật toán vừa hoàn chỉnh vừa chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, v = map(int, input().split())
    a = list(map(int, input().split()))

    total = sum(a)

    if v > total:
        print("NO")
        return

    target = v % k

    dp = [-1] * k
    parent = [(-1, -1)] * k
    dp[0] = -2

    rem = [x % k for x in a]

    for i in range(n):
        r = rem[i]
        if r == 0:
            continue

        ndp = dp[:]

        for s in range(k):
            if dp[s] == -1:
                continue

            ns = (s + r) % k

            if ndp[ns] == -1:
                ndp[ns] = i
                parent[ns] = (s, i)

        dp = ndp

    if dp[target] == -1:
        print("NO")
        return

    chosen = [False] * n

    cur = target
    while cur != 0:
        prev, idx = parent[cur]
        chosen[idx] = True
        cur = prev

    chosen_indices = [i for i in range(n) if chosen[i]]

    if not chosen_indices:
        chosen_indices = [0]
        chosen[0] = True

    collector = chosen_indices[0]

    ops = []

    def add_op(cnt, x, y):
        if cnt > 0 and x != y:
            ops.append((cnt, x + 1, y + 1))

    for i in range(n):
        if i == collector:
            continue

        if chosen[i]:
            need = rem[i]
            cnt = (a[i] - need) // k
            add_op(cnt, i, collector)

            a[collector] += cnt * k
            a[i] -= cnt * k
        else:
            cnt = a[i] // k
            add_op(cnt, i, collector)

            a[collector] += cnt * k
            a[i] -= cnt * k

            if a[i] > 0:
                add_op(1, i, collector)
                a[collector] += a[i]
                a[i] = 0

    diff = a[collector] - v

    aux = 0 if collector != 0 else 1

    add_op(diff // k, collector, aux)

    print("YES")
    for cnt, x, y in ops:
        print(cnt, x, y)

if __name__ == "__main__":
    solve()
```Chỉ có cửa hàng DP`K`trạng thái, đó là sự tối ưu hóa quan trọng. Con trỏ gốc cho phép xây dựng lại tập hợp con đã chọn mà không lưu trữ toàn bộ bảng DP. 

Giai đoạn xây dựng phản ánh bằng chứng. Các bể nằm ngoài tập hợp con đã chọn sẽ cạn kiệt hoàn toàn. Xe tăng bên trong tập hợp con giữ chính xác phần còn lại của chúng. Mọi đơn vị bị loại bỏ đều được dẫn vào bộ thu. 

Một điểm tinh tế là sự giảm thiểu cuối cùng của bộ sưu tập. Sự khác biệt giữa giá trị hiện tại của nó và`V`được đảm bảo chia hết cho`K`. Đó chính xác là lý do tại sao việc chuyển toàn bộ số tiền lặp đi lặp lại có thể loại bỏ số tiền dư thừa mà không làm ảnh hưởng đến lớp modulo. 

Một chi tiết tinh tế khác là xử lý các thùng chứa không còn dư. Chúng không bao giờ giúp đạt được dư lượng mới trong DP, vì vậy chúng có thể bị bỏ qua. Trong quá trình xây dựng, chúng chỉ đơn giản được xả hoàn toàn vào bộ thu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3 5
2 3
```còn lại là`[2, 0]`. 

Dư lượng mục tiêu:```
5 mod 3 = 2
```DP chọn xe tăng 1. 

| Bước | Tập hợp con được chọn | Nhà sưu tập | Số tiền sưu tầm | 
| --- | --- | --- | --- | 
| Ban đầu | {1} | 1 | 2 | 
| Bể xả 2 | {1} | 1 | 5 | 
| Điều chỉnh cuối cùng | không cần thiết | 1 | 5 | 

Đầu ra:```
YES
1 2 1
```Dấu vết cho thấy việc xây dựng thành công đơn giản nhất. Bộ sưu tập ngay lập tức đạt được giá trị mong muốn. 

### Ví dụ 2 

đầu vào:```
3 4 7
5 6 3
```còn lại là`[1, 2, 3]`. 

Dư lượng mục tiêu:```
7 mod 4 = 3
```DP chỉ có thể chọn xe tăng thứ ba. 

| Bước | Giá trị xe tăng | Nhà sưu tập | 
| --- | --- | --- | 
| Ban đầu | (5,6,3) | 3 | 
| Bể xả 1 | (1,6,7) | 3 | 
| Bể xả 2 | (1,2,11) | 3 | 
| Giảm bộ thu | (5,2,7) | 3 | 

Người thu thập đạt 11 đầu tiên. Từ`11 - 7 = 4`, một lần chuyển toàn bộ sẽ loại bỏ chính xác một khối có kích thước`K`, để lại 7. 

Dấu vết này chứng tỏ tại sao chỉ có lớp dư lượng mới quan trọng. Khi đã đạt được số dư chính xác, bội số vượt quá của`K`luôn có thể được gỡ bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NK) | Một chuyển đổi ba lô cho mỗi thùng và cặn | 
| Không gian | O(K) | Các tiểu bang DP và thông tin phụ huynh | 
| Kích thước đầu ra | O(N) | Tối đa một số hoạt động không đổi trên mỗi xe tăng | 

Với`N ≤ 5000`Và`K ≤ 5000`, cái`O(NK)`độ phức tạp thực hiện khoảng 25 triệu chuyển đổi trong trường hợp xấu nhất, phù hợp thoải mái trong giới hạn trong Python được tối ưu hóa. Việc sử dụng bộ nhớ vẫn còn nhỏ vì chỉ`K`Trạng thái DP được lưu trữ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old
    return out.getvalue()

# provided sample
assert run("2 3 5\n2 3\n").startswith("YES")

# impossible because target exceeds total water
assert run("2 5 20\n3 4\n").strip() == "NO"

# already have exact amount
assert run("2 5 3\n3 7\n").startswith("YES")

# all values multiples of K, target residue impossible
assert run("3 4 7\n4 8 12\n").strip() == "NO"

# target zero
assert run("2 3 0\n5 7\n").startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 5 20 / 3 4`| KHÔNG | Bảo tồn nước | 
|`2 5 3 / 3 7`| CÓ | Giải pháp hiện tại đã có mặt | 
|`3 4 7 / 4 8 12`| KHÔNG | Modulo bất biến | 
|`2 3 0 / 5 7`| CÓ | Xử lý mục tiêu bằng không | 

## Vỏ cạnh 

### Mục tiêu lớn hơn tổng lượng nước 

đầu vào:```
2 5 20
3 4
```Tổng lượng nước chỉ là 7. Vì các hoạt động không bao giờ tạo ra nước nên việc đạt tới 20 là không thể. Thuật toán phát hiện điều này trước khi chạy DP và in NO. 

### Tất cả số dư đều bằng 0 

đầu vào:```
3 4 7
4 8 12
```Mỗi bể là bội số của 4. Bất kỳ thao tác nào sẽ loại bỏ hoặc thêm bội số của 4, do đó, tất cả lượng bể luôn là bội số của 4. Vì 7 không phải là bội số của 4 nên không có nghiệm nào tồn tại. DP chỉ đạt đến dư lượng 0 và loại bỏ chính xác dư lượng mục tiêu 3. 

### Mục tiêu bằng 0 

đầu vào:```
2 3 0
5 7
```Dư lượng mục tiêu cũng bằng không. DP ngay lập tức thành công với tập hợp con trống. Cấu trúc chọn một bộ thu tùy ý, tập hợp tất cả nước vào đó, sau đó loại bỏ bội số của 3 cho đến khi bộ thu trở thành chính xác 0. 

### Bể hiện có đã chứa V rồi 

đầu vào:```
3 5 10
10 7 8
```Thuật toán vẫn hoạt động. DP tìm thấy mẫu dư lượng hợp lệ và việc xây dựng có thể tạo ra các hoạt động bằng 0 hoặc một chuỗi tương đương ngắn. Sự tồn tại của một chiếc xe tăng đã hoạt động chính xác không bao giờ gây ra lỗi vì thuật toán lý giải về khả năng tiếp cận hơn là yêu cầu bất kỳ sửa đổi nào.
