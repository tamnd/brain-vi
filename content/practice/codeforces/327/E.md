---
title: "CF 327E - Đi bộ theo trục"
description: "Chúng ta có n độ dài đoạn dương. Tổng của chúng là d, cũng là điểm đích trên trục số. Một tuyến đường chỉ đơn giản là một thứ tự có độ dài này. Trong khi đi theo một lộ trình, chúng tôi giữ tổng tiền tố đang chạy."
date: "2026-06-06T08:58:49+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "constructive-algorithms", "dp", "meet-in-the-middle"]
categories: ["algorithms"]
codeforces_contest: 327
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 191 (Div. 2)"
rating: 2300
weight: 327
solve_time_s: 145
verified: true
draft: false
---

[CF 327E - Di chuyển theo trục](https://codeforces.com/problemset/problem/327/E) 

**Đánh giá:** 2300 
**Tags:** bitmasks, tổ hợp, thuật toán xây dựng, dp, gặp nhau ở giữa 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`độ dài đoạn dương. Tổng số tiền của họ là`d`, cũng là điểm đích trên trục số. 

Một tuyến đường chỉ đơn giản là một thứ tự có độ dài này. Trong khi đi theo một lộ trình, chúng tôi giữ tổng tiền tố đang chạy. Sau khi lấy đoạn đầu tiên, chúng ta dừng lại một lần, sau đoạn thứ hai, chúng ta dừng lại, v.v. Điểm dừng cuối cùng luôn ở vị trí`d`. 

Một số vị trí không may mắn. Một tuyến đường được coi là tốt nếu không có điểm dừng nào trùng với vị trí không may mắn. 

Nhiệm vụ là đếm xem có bao nhiêu hoán vị của độ dài cho trước là tốt, modulo`10^9 + 7`. 

Quan sát quan trọng nhất là mọi điểm dừng đều là tổng của một tiền tố nào đó của hoán vị. Vì tất cả các độ dài đều dương nên tổng tiền tố tăng dần. Sự đơn điệu này là điều làm cho vấn đề trở nên dễ giải quyết. 

Ràng buộc`n ≤ 24`ngay lập tức loại trừ bất cứ điều gì liên quan đến tất cả các hoán vị. Thậm chí`20!`có kích thước lớn về mặt thiên văn. Một giải pháp phải hoạt động với các tập hợp con thay vì hoán vị. 

Số vị trí không may mắn nhiều nhất là hai. Giới hạn nhỏ bé đó là manh mối quan trọng thứ hai. Nó đề xuất loại trừ bao hàm đối với các vị trí không may mắn, chỉ để lại một số bài toán con đếm. 

Có một số trường hợp góc dễ bỏ sót. 

Nếu một vị trí không may mắn không thể được biểu diễn dưới dạng tổng tập hợp con thì không có tuyến đường nào có thể dừng ở đó. Ví dụ:```
n = 3
a = [2, 4, 8]
unlucky = 5
```Không có tập hợp con nào để`5`, do đó mọi hoán vị đều hợp lệ. 

Nếu một vị trí không may mắn bằng tổng số tiền`d`, mọi tuyến đường đều không hợp lệ vì điểm dừng cuối cùng luôn ở`d`. Ví dụ:```
n = 2
a = [3, 7]
unlucky = 10
```Câu trả lời là`0`. 

Khi có hai vị trí không may mắn, một lộ trình có thể gặp phải cả hai. Loại trừ bao gồm phải thêm các tuyến đường này trở lại một lần. Ví dụ:```
a = [1, 2, 3]
unlucky = {1, 3}
```Hoán vị`[1,2,3]`đánh vào cả hai vị trí không may mắn, vì vậy việc trừ từng tập hợp xấu một cách độc lập sẽ loại bỏ nó hai lần. 

## Phương pháp tiếp cận 

Giải pháp brute-force tạo ra tất cả`n!`hoán vị, tính toán tất cả các tổng tiền tố và kiểm tra xem có tiền tố nào không may mắn hay không. 

Điều này đúng vì nó tuân theo định nghĩa của một tuyến đường. Thật không may, nó gần như trở nên vô dụng ngay lập tức. Ngay cả đối với`n = 15`, số lượng hoán vị vượt quá`10^12`. 

Để có được điều gì đó khả thi, chúng ta cần đếm các hoán vị một cách gián tiếp. 

Hãy xem xét một vị trí không may mắn duy nhất`x`. 

Một tuyến đường đạt tới`x`chính xác khi các phần tử được đặt trước điểm dừng đó tạo thành một tập hợp con có tổng bằng`x`. 

Giả sử một tập hợp con`S`có tổng`x`và kích thước`s`. Mọi hoán vị trong đó tất cả các phần tử của`S`xuất hiện trước tất cả các phần tử bên ngoài`S`sẽ đạt được`x`. Các phần tử bên trong`S`có thể được sắp xếp tùy ý, các phần tử còn lại cũng có thể được sắp xếp tùy ý. 

Do đó tập hợp con đó đóng góp```
s! · (n - s)!
```hoán vị xấu. 

Vì vậy, vấn đề trở thành:```
For every subset whose sum is x,
add size! · remaining_size!
```Đây là bài toán đếm tổng tập hợp con, không phải bài toán hoán vị. 

Từ`n ≤ 24`, gặp nhau ở giữa là công cụ tự nhiên. Chia mảng thành hai nửa kích thước nhiều nhất`12`giảm`2^24`tập hợp con về khoảng`2 · 2^12 = 8192`sự liệt kê. 

Đối với hai vị trí kém may mắn`x < y`, loại trừ bao gồm yêu cầu đếm các tuyến đường đạt cả hai vị trí. 

Một tuyến đường như vậy có chứa:```
S          -> prefix sum x
S ∪ U      -> prefix sum y
```Ở đâu```
sum(S) = x
sum(U) = y - x
S and U are disjoint
```Nếu như```
|S| = a
|U| = b
```thì các hoán vị tương ứng là```
a! · b! · (n - a - b)!
```bởi vì tất cả các phần tử của`S`phải đến trước thì tất cả các phần tử của`U`, sau đó là mọi thứ khác. 

Bây giờ mỗi phần tử có ba trạng thái có thể:```
belongs to S
belongs to U
belongs to neither
```Điều này dẫn tới một cuộc gặp gỡ ba lần ở giữa`3^(n/2)`tiểu bang. 

Vì`n = 24`, mỗi nửa có nhiều nhất`3^12 = 531441`trạng thái hoàn toàn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · n!) | O(n) | Quá chậm | 
| Tối ưu | O(2^(n/2) + 3^(n/2)) | O(3^(n/2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Đếm các tuyến đường chạm vào một vị trí không may mắn 

1. Chia mảng thành hai nửa. 
2. Liệt kê mọi tập hợp con của nửa bên phải và nhóm chúng theo`(sum, size)`. 
3. Liệt kê mọi tập hợp con của nửa bên trái. 
4. Đối với vị trí mục tiêu không may mắn`x`, giả sử tập hợp con bên trái đóng góp`(sumL, sizeL)`. Chúng ta cần các tập con đúng có tổng`x - sumL`. 
5. Mỗi cặp phù hợp tạo thành một tập hợp con hoàn chỉnh có tổng kích thước là`sizeL + sizeR`. 
6. Thêm```
count · (sizeL + sizeR)! · (n - sizeL - sizeR)!
```để trả lời. 

Điều này mang lại số lượng hoán vị đạt được`x`. 

### Đếm những đường đi trúng cả 2 vị trí không may 

Giả sử`x < y`và để```
d = y - x
```1. Chia mảng thành hai nửa. 
2. Liệt kê tất cả các nhiệm vụ thứ ba của nửa bên phải. 
3. Đối với từng hồ sơ phân công:```
(sumS, sumU, sizeS, sizeU)
```và tần số của nó. 

1. Liệt kê tất cả các nhiệm vụ thứ ba của nửa bên trái. 
2. Đối với phép gán bên trái, phép gán bên phải phải đáp ứng:```
sumS_right = x - sumS_left
sumU_right = d - sumU_left
```1. Mỗi cặp khớp nhau xác định một cặp rời rạc`(S,U)`. 
2. Nếu kích thước kết hợp là:```
a = sizeS_left + sizeS_right
b = sizeU_left + sizeU_right
```thêm vào```
a! · b! · (n - a - b)!
```lần số cặp tương ứng. 

### Bao gồm-loại trừ 

Nếu không có vị trí kém may mắn:```
answer = n!
```Nếu có một vị trí không may mắn:```
answer = n! - bad(x)
```Nếu có hai vị trí không may mắn:```
answer = n! - bad(x) - bad(y) + bad_both(x,y)
```Tất cả số học được thực hiện modulo`10^9 + 7`. 

### Tại sao nó hoạt động 

Đối với một vị trí không may mắn, mỗi tuyến đường đến vị trí đó sẽ xác định chính xác một tập hợp con các phần tử xuất hiện trước điểm dừng. Ngược lại, mọi tập con có tổng yêu cầu sẽ tạo ra chính xác`s!(n-s)!`những tuyến đường như vậy. Điều này thiết lập sự phân biệt giữa các tuyến xấu và cấu hình tập hợp con được tính. 

Đối với hai vị trí không may mắn, độ dương ở mọi độ dài đảm bảo rằng vị trí kém may mắn nhỏ hơn phải đạt được trước tiên. Do đó, mọi tuyến đường chạm vào cả hai vị trí sẽ phân hủy duy nhất thành các tập hợp rời rạc`S`Và`U`, Ở đâu`S`đạt đến vị trí đầu tiên và`S ∪ U`đạt đến thứ hai. Công thức đếm tuân theo trực tiếp từ thứ tự khối bắt buộc của các tập hợp này bên trong hoán vị. 

Vì loại trừ bao gồm là chính xác nên mọi tuyến đường đều được tính với bội số chính xác. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

MOD = 1000000007

def subset_bad(arr, target, fact):
    n = len(arr)
    m = n // 2
    left = arr[:m]
    right = arr[m:]

    right_map = defaultdict(lambda: [0] * (len(right) + 1))

    rlen = len(right)
    for mask in range(1 << rlen):
        s = 0
        cnt = 0
        for i in range(rlen):
            if mask >> i & 1:
                s += right[i]
                cnt += 1
        right_map[s][cnt] += 1

    ans = 0

    llen = len(left)
    for mask in range(1 << llen):
        s = 0
        cnt = 0
        for i in range(llen):
            if mask >> i & 1:
                s += left[i]
                cnt += 1

        need = target - s
        if need not in right_map:
            continue

        vec = right_map[need]

        for rsz, ways in enumerate(vec):
            if ways == 0:
                continue

            sz = cnt + rsz
            ans = (ans + ways * fact[sz] * fact[n - sz]) % MOD

    return ans

def both_bad(arr, x, y, fact):
    if x > y:
        x, y = y, x

    diff = y - x
    n = len(arr)

    m = n // 2
    left = arr[:m]
    right = arr[m:]

    right_map = defaultdict(lambda: defaultdict(int))

    def gen_half(values):
        res = []

        def dfs(idx, sum_s, sum_u, cnt_s, cnt_u):
            if idx == len(values):
                res.append((sum_s, sum_u, cnt_s, cnt_u))
                return

            v = values[idx]

            dfs(idx + 1, sum_s, sum_u, cnt_s, cnt_u)
            dfs(idx + 1, sum_s + v, sum_u, cnt_s + 1, cnt_u)
            dfs(idx + 1, sum_s, sum_u + v, cnt_s, cnt_u + 1)

        dfs(0, 0, 0, 0, 0)
        return res

    right_states = gen_half(right)

    for ss, su, cs, cu in right_states:
        right_map[(ss, su)][(cs, cu)] += 1

    ans = 0

    left_states = gen_half(left)

    for ss, su, cs, cu in left_states:
        need = (x - ss, diff - su)

        if need not in right_map:
            continue

        bucket = right_map[need]

        for (rs, ru), ways in bucket.items():
            a = cs + rs
            b = cu + ru
            c = n - a - b

            ans += ways * fact[a] * fact[b] * fact[c]
            ans %= MOD

    return ans

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    k = int(input())

    unlucky = []
    if k:
        unlucky = list(map(int, input().split()))

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    total = fact[n]

    if k == 0:
        print(total)
        return

    if k == 1:
        ans = (total - subset_bad(a, unlucky[0], fact)) % MOD
        print(ans)
        return

    x, y = unlucky

    bad_x = subset_bad(a, x, fact)
    bad_y = subset_bad(a, y, fact)
    bad_both = both_bad(a, x, y, fact)

    ans = (total - bad_x - bad_y + bad_both) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Người trợ giúp đầu tiên tính toán số lượng hoán vị chạm vào một vị trí không may mắn. Nó thực hiện đếm tổng tập hợp con gặp nhau ở giữa tiêu chuẩn, nhưng thay vì chỉ đếm các tập hợp con, nó tính trọng số của chúng theo`size!(n-size)!`. 

Người trợ giúp thứ hai tính toán thuật ngữ giao nhau để loại trừ. Mỗi phần tử được gán cho một trong ba nhóm:`S`,`U`hoặc không. Cuộc gặp gỡ ở giữa kết hợp các trạng thái ternary có tổng hoàn thành các mục tiêu cần thiết. 

Một điểm tinh tế là trọng số chỉ phụ thuộc vào kích thước nhóm cuối cùng chứ không phụ thuộc vào các phần tử cụ thể. Điều đó cho phép tất cả các trạng thái giống hệt nhau`(sumS, sumU, sizeS, sizeU)`thông tin cần tổng hợp. 

Một chi tiết khác là xử lý các vị trí kém may mắn theo thứ tự sắp xếp. Vì tất cả độ dài đoạn đều dương nên việc đạt được cả hai vị trí có nghĩa là đạt được
