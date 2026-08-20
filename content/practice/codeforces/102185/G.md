---
title: "CF 102185G - \u0413\u0438\u0440\u043b\u044f\u043d\u0434\u0430"
description: "Vòng hoa được điều khiển bởi hai thông số. Sau khi được bật ở thời điểm nguyên (S), nó vẫn sáng trong (A) phút, sau đó tối trong (A) phút và lặp lại chu kỳ này mãi mãi. Trước thời gian (S), trời đã tối vì vòng hoa chưa được bật lên."
date: "2026-08-20T00:39:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102185
codeforces_index: "G"
codeforces_contest_name: "Southern Russia Open Championship \u2013 ContestSFedU 2019, Team Final."
rating: 0
weight: 102185
solve_time_s: 306
verified: true
draft: false
---

[CF 102185G - \u0413\u0438\u0440\u043b\u044f\u043d\u0434\u0430](https://codeforces.com/problemset/problem/102185/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vòng hoa được điều khiển bởi hai thông số. Sau khi được bật ở thời điểm nguyên (S), nó vẫn sáng trong (A) phút, sau đó tối trong (A) phút và lặp lại chu kỳ này mãi mãi. Trước thời gian (S), trời đã tối vì vòng hoa chưa được bật lên. 

Ngày nghỉ chiếm khoảng ([0,T]). Trong khoảng thời gian này ít nhất một nửa thời gian phải được thắp sáng. Đồng thời, ông nội ở nhà trong một số khoảng thời gian rời rạc ([L_i,R_i]) và chúng tôi muốn giảm thiểu tổng chiều dài giao điểm của chúng với các phần được chiếu sáng của vòng hoa. 

Câu trả lời bao gồm sự chồng chéo tối thiểu không thể tránh khỏi, thời gian được chọn (A) và thời gian chuyển đổi (S). Trong số các lần trùng lặp bằng nhau, chúng tôi thích thời gian chuyển đổi muộn hơn (A) và trong số các lần trùng lặp bằng nhau (A), chúng tôi thích thời gian chuyển đổi muộn hơn. 

Tất cả các điểm cuối đều là số nguyên, vì vậy chúng ta có thể coi mỗi phút ([t,t+1)) là một vị trí riêng biệt. Khi đó, khoảng ông ([L,R]) chiếm chính xác các vị trí (L,L+1,\ldots,R-1). 

Giới hạn (T\le 5000) là manh mối chính. Giải pháp (O(T^2)) là thực tế, trong khi phương pháp quét tất cả (T) phút cho mỗi cặp ((A,S)) là quá đắt. Số lượng khoảng thời gian lớn nhất là (T/2), do đó thành phần (O(NT)) cũng được chấp nhận như một phần của giải pháp (O(T^2)). 

Có một số trường hợp ranh giới có thể dễ dàng phá vỡ việc triển khai có vẻ đúng. Đầu tiên là vòng hoa không chạy định kỳ trước khi được bật. Ví dụ,```
4 0
```có câu trả lời```
0 1 1
```bởi vì với (A=1) và (S=1), vòng hoa được thắp sáng trong ([1,2)) và ([3,4)), đúng một nửa ngày lễ. Việc coi mẫu là tuần hoàn trước (S) sẽ coi (S=1) tương đương với (S=-1) một cách không chính xác. 

Trường hợp thứ hai là thời gian chuyển đổi trước kỳ nghỉ lễ. Vì```
8 2
1 3
5 7
```câu trả lời là```
0 2 -1
```Với (A=2) và (S=-1), vòng hoa được thắp sáng trong ([0,1)), ([3,5)) và ([7,8)), cho chính xác bốn phút thắp sáng trong khi tránh cả hai khoảng thời gian ông ngoại. Một nghiệm chỉ xét (S\ge0) sẽ thiếu phương án tối ưu. 

Trường hợp cạnh thứ ba xảy ra khi thời gian chuyển mạch chính xác ở điểm bắt đầu của khoảng thời gian ông nội. Vì```
4 1
1 2
```câu trả lời là```
0 2 2
```Vòng hoa được thắp sáng trên ([2,4)), tạo ra hai phút sáng và không có sự chồng chéo. Sai lầm phổ biến ở đây là coi điểm cuối (2) thuộc khoảng ([1,2]). Đây là những khoảng thời gian nên giao điểm của chúng có độ dài bằng không. 

Cuối cùng, (A) không cần phải xem xét ngoài (T). Nếu (A>T), nhiều nhất một đoạn sáng có thể giao nhau với các ngày nghỉ. Phân đoạn như vậy có thể là tiền tố, hậu tố hoặc toàn bộ phần nghỉ và cùng một phân đoạn sáng có thể được sao chép bằng (A=T). Vì (A) nhỏ hơn thắng hòa nên chỉ cần xem xét (A\le T) là đủ. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp là khái niệm đơn giản. Chúng ta có thể thử mọi (A) từ (1) đến (T), mọi thời gian chuyển đổi số nguyên có liên quan (S), mô phỏng vòng hoa từng phút, đếm thời lượng thắp sáng của nó và đếm riêng xem bao nhiêu phút thắp sáng thuộc về khoảng thời gian ông nội. Mọi ứng viên đều được kiểm tra chính xác nên phương pháp này là chính xác. 

Đối với (A) cố định, thời gian chuyển đổi âm chỉ cần đại diện từ ([-2A,-1]), vì việc dịch chuyển (S) theo (2A) không làm thay đổi mô hình tuần hoàn sau khi vòng hoa đã được bật. Đối với (S) không âm, phương án khả thi không thể có (S>\lfloor T/2\rfloor), vì vẫn còn nhiều nhất (T-S) phút. Điều này khiến ứng viên (O(2A+T)) bắt đầu cho một (A). Nếu mọi thí sinh quét hết (T) phút thì trường hợp xấu nhất là khoảng 

[ 
T\left(\sum_{A=1}^{T} (2A+T/2)\right) 
] 

các hoạt động, tức là khoảng (1.9\cdot10^{11}) hoạt động tại (T=5000). Đó không phải là gần giới hạn một giây. 

Quan sát quan trọng là đối với một (A) cố định, mẫu tuần hoàn vô hạn chỉ phụ thuộc vào (S\bmod 2A). Nếu chúng ta biết, với mỗi modulo dư (2A), có bao nhiêu phút nghỉ và bao nhiêu phút ông ngoại có số dư đó, thì việc di chuyển (S) đi một đơn vị chỉ loại bỏ một lớp cặn khỏi nửa chu kỳ sáng và thêm một lớp cặn khác. Do đó, một giá trị tương quan hoàn chỉnh có thể được cập nhật trong (O(1)). 

Có một điều phức tạp. Đối với (S\ge0), công thức tuần hoàn sẽ chiếu sai số lần trước (S). Chúng tôi xử lý vấn đề này bằng cách duy trì lượng lit nằm trong tiền tố ([0,S)). Khi (S) tăng thêm một, tập hợp các lớp dư lượng lit sẽ thay đổi một, do đó phần đóng góp tiền tố đó cũng có thể được cập nhật trong (O(1)) bằng cách sử dụng tổng dư lượng tích lũy cho tiền tố đã được xử lý. 

Có một sự đơn giản hóa bổ sung cho (2A>T). Khi đó, khoảng thời gian (2A) dài hơn toàn bộ phần nghỉ, do đó phần nghỉ chỉ có thể chứa một đoạn sáng. Bài toán cho (A) đó trở thành một bài toán giao khoảng đơn giản, có thể được đánh giá trực tiếp từ tổng tiền tố của số người ở trong phòng. 

Cách tiếp cận kết quả là (O(T^2)), chỉ với bộ nhớ (O(T)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(T^3)) | (O(T)) | Quá chậm | 
| Tối ưu | (O(T^2)) | (O(T)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng`g[t]`với (0\le t<T), trong đó`g[t]=1`chính xác khi ông nội có mặt ở nhà trong phút ([t,t+1)). Cũng xây dựng tổng tiền tố của nó. Điều này cho phép chúng ta tìm được phần chồng lên nhau của bất kỳ khoảng thông thường nào trong (O(1)). 
2. Xét mọi (A) từ (1) đến (T). Các giá trị trên (T) là không cần thiết vì bất kỳ mẫu khả thi nào được tạo bởi (A) như vậy đều có thể được sao chép bằng (A=T). 
3. Nếu (2A>T), khoảng thời gian dài hơn ngày nghỉ. Phần sáng bên trong ([0,T]) là một khoảng đơn. Với mỗi thời gian chuyển đổi khả thi (S), giao ([S,S+A)) với ([0,T]), tính độ dài của nó và tính toán phần chồng chéo của nó với tổng tiền tố. Đối với âm (S), điểm cuối bên trái được cắt bớt về 0 vì vòng hoa đã được bật trước kỳ nghỉ lễ. 
4. Với (2A\le T), đặt (P=2A). Xây dựng`cnt[r]`, số phút nghỉ lễ có thời gian bằng (r\pmod P), và`home[r]`, số phút của ông nội có cùng số dư. Phần sáng của mẫu tuần hoàn bắt đầu từ phần dư (S\bmod P) bao gồm chính xác (A) phần dư liên tiếp. 
5. Xây dựng chu kỳ chiếu sáng và chồng lấp định kỳ cho (S=0). Đây là tổng của phần dư (0,\ldots,A-1). Khi (S) thay đổi từ (S) thành (S+1), dư lượng (S\bmod P) rời khỏi nửa sáng và dư lượng ((S+A)\bmod P) đi vào nửa đó. Do đó, cả hai tổng số đều thay đổi chỉ với hai lần truy cập mảng. 
6. Liệt kê thời gian chuyển đổi âm bằng cách sử dụng các đại diện (S=-P+1,\ldots,-1). Sự khởi đầu tiêu cực có nghĩa là vòng hoa đã bắt đầu, vì vậy việc tính toán định kỳ là hành vi thực tế vào những ngày lễ. Đại diện (-P) có cùng mẫu ngày lễ với (0) nhưng sớm hơn nên có thể bỏ qua. 
7. Thời gian chuyển mạch không âm của quá trình (S=0,\ldots,\lfloor T/2\rfloor). Tổng số định kỳ vẫn mô tả một mẫu giả định kéo dài trước (S), vì vậy hãy trừ đi phần của mẫu đó nằm trong ([0,S)). Duy trì sự đóng góp tiền tố này dần dần. Khi chuyển từ (S) sang (S+1), tiền tố cũ mất phần dư (S\bmod P), nhận phần dư ((S+A)\bmod P) và phút mới được thêm vào (S) không sáng trong mẫu mới vì thời gian tương đối của nó bằng 0. 
8. Đối với mỗi ứng viên, yêu cầu`2 * lit >= T`. Nếu khả thi, hãy so sánh sự chồng chéo của ông nội với câu trả lời hiện tại. Việc so sánh đầu tiên giảm thiểu sự chồng chéo, sau đó là (A), sau đó tối đa hóa (S). 
9. In bộ ba tốt nhất. 

Bất biến đằng sau phần tuần hoàn là`period_lit`Và`period_home`luôn bằng tổng số tiền thu được bằng cách áp dụng mô hình tuần hoàn vô hạn với pha chuyển mạch hiện tại cho toàn bộ khoảng thời gian nghỉ lễ. Các biến tiền tố luôn có cùng số tiền được giới hạn trong vài phút trước thời gian chuyển đổi thực tế. Sự khác biệt của chúng chính là hoạt động của vòng hoa thật, nó tối trước khi được bật lên. Vì mọi giai đoạn liên quan đều được liệt kê và mọi thời gian chuyển đổi không âm liên quan đều được xử lý nên đảm bảo tìm được ứng viên khả thi nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(data=None):
    if data is None:
        T, N = map(int, input().split())
        intervals = [tuple(map(int, input().split())) for _ in range(N)]
    else:
        it = iter(map(int, data.split()))
        T = next(it)
        N = next(it)
        intervals = [(next(it), next(it)) for _ in range(N)]

    # g[t] = 1 iff the grandfather is home during minute [t, t+1).
    diff_g = [0] * (T + 1)
    for l, r in intervals:
        diff_g[l] += 1
        diff_g[r] -= 1

    g = [0] * T
    cur = 0
    for t in range(T):
        cur += diff_g[t]
        g[t] = 1 if cur else 0

    # Prefix sum of grandfather occupancy.
    pref_g = [0] * (T + 1)
    for t in range(T):
        pref_g[t + 1] = pref_g[t] + g[t]

    half_floor = T // 2
    half_ceil = (T + 1) // 2

    best_cost = T + 1
    best_a = 0
    best_s = 0

    def consider(cost, a, s):
        nonlocal best_cost, best_a, best_s
        if cost < best_cost:
            best_cost = cost
            best_a = a
            best_s = s
        elif cost == best_cost and a == best_a and s > best_s:
            best_s = s

    for A in range(1, T + 1):
        # If the period is longer than the holiday, there can be
        # only one lit segment inside [0, T].
        if 2 * A > T:
            lo = half_ceil - A

            for s in range(lo, half_floor + 1):
                left = max(0, s)
                right = min(T, s + A)

                lit = right - left
                if 2 * lit < T:
                    continue

                cost = pref_g[right] - pref_g[left]
                consider(cost, A, s)

            continue

        P = 2 * A

        # home[r] = number of grandfather minutes t with t % P == r.
        #
        # Each grandfather interval contributes q to every residue,
        # plus one to a cyclic range of length rem.
        home_diff = [0] * (P + 1)
        base = 0

        for l, r in intervals:
            length = r - l
            q, rem = divmod(length, P)
            base += q

            if rem:
                start = l % P
                end = start + rem

                if end <= P:
                    home_diff[start] += 1
                    home_diff[end] -= 1
                else:
                    home_diff[start] += 1
                    home_diff[P] -= 1
                    home_diff[0] += 1
                    home_diff[end - P] -= 1

        home = [0] * P
        cur = 0
        for r in range(P):
            cur += home_diff[r]
            home[r] = cur + base

        # cnt[r] = number of holiday minutes with t % P == r.
        q, rem = divmod(T, P)
        cnt = [q + (1 if r < rem else 0) for r in range(P)]

        # Phase S = 0, whose lit residues are [0, A).
        period_lit = sum(cnt[:A])
        period_home = sum(home[:A])

        # Negative starts. For s < 0 the periodic pattern is real,
        # because the garland has already been switched on.
        cur_lit = period_lit
        cur_home = period_home

        for r in range(1, P):
            cur_lit += cnt[(r + A - 1) % P] - cnt[r - 1]
            cur_home += home[(r + A - 1) % P] - home[r - 1]

            s = -P + r

            if 2 * cur_lit >= T:
                consider(cur_home, A, s)

        # Nonnegative starts.
        cur_lit = period_lit
        cur_home = period_home

        # pref_cnt[r] and pref_home[r] contain the already processed
        # prefix [0, s), grouped by residue modulo P.
        pref_cnt = [0] * P
        pref_home = [0] * P

        # s = 0: nothing has to be removed from the periodic pattern.
        if 2 * cur_lit >= T:
            consider(cur_home, A, 0)

        for s in range(half_floor):
            r1 = s % P
            r2 = (s + A) % P

            # Shift the infinite periodic phase by one.
            cur_lit += cnt[r2] - cnt[r1]
            cur_home += home[r2] - home[r1]

            # Shift the part lying before the actual switch time.
            removed_lit = pref_cnt[r2] - pref_cnt[r1]
            removed_home = pref_home[r2] - pref_home[r1]

            pref_cnt[r1] += 1
            pref_home[r1] += g[s]

            actual_lit = cur_lit - removed_lit
            actual_home = cur_home - removed_home

            ns = s + 1

            if 2 * actual_lit >= T:
                consider(actual_home, A, ns)

    return f"{best_cost} {best_a} {best_s}"

if __name__ == "__main__":
    sys.stdout.write(solve() + "\n")
```Phần đầu tiên xây dựng tỷ lệ chiếm chỗ của ông nội ở cấp độ phút và tổng tiền tố của nó. Bởi vì tất cả các điểm cuối của khoảng đều là số nguyên nên cách biểu diễn này là chính xác chứ không phải là xấp xỉ thời gian liên tục. 

chi nhánh`2 * A > T`sử dụng thực tế là khoảng thời gian (2A) dài hơn toàn bộ ngày nghỉ. Không thể có đoạn sáng thứ hai trong ngày lễ, vì vậy vòng hoa được thể hiện bằng một khoảng duy nhất. Phạm vi ứng cử viên bắt đầu tại`half_ceil - A`, đây là thời gian chuyển đổi âm sớm nhất mà giao điểm được chiếu sáng có thể đạt đến một nửa thời gian nghỉ lễ. 

Vì`2 * A <= T`, mã xây dựng số dư theo modulo`P = 2 * A`. Mảng dư lượng ông nội được xây dựng với các cập nhật phạm vi mảng khác nhau. Một khoảng độ dài`q * P + rem`đóng góp`q`cho mọi dư lượng và một đơn vị bổ sung để`rem`dư lượng tuần hoàn liên tiếp. Điều này tránh việc quét tất cả (T) phút cho mọi giá trị của (A). 

Hai biến`period_lit`Và`period_home`mô tả mô hình vô hạn giả định. Vòng bắt đầu âm có thể sử dụng chúng trực tiếp vì vòng hoa đã được bật trước thời gian 0. 

Vòng khởi đầu tích cực tinh tế hơn.`pref_cnt`Và`pref_home`mô tả phần của mẫu tuần hoàn nằm trước công tắc thực tế. Cập nhật xảy ra trước khi chèn phút`s`vào các mảng này. Thứ tự đó quan trọng vì phút`s`không phải trước thời điểm chuyển đổi mới`s+1`, nhưng nó cũng không được chiếu sáng bởi pha mới tại thời điểm tương đối bằng 0. 

Việc kiểm tra tính khả thi sử dụng`2 * lit >= T`thay vì chia dấu phẩy động. Điều này xử lý chính xác các giá trị lẻ của (T). Ví dụ: nếu (T=5), cần ít nhất ba phút sáng. 

Mã phá vỡ ràng buộc dựa vào việc lặp lại (A) theo thứ tự tăng dần. Với chi phí bằng nhau, (A) lớn hơn không bao giờ thay thế được giải pháp đã được lưu trữ. Đối với cùng một (A), một ứng viên chỉ thay thế ứng viên hiện tại khi thời gian chuyển đổi của nó muộn hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
10 2
1 4
7 10
```Hãy xem xét (A=1). Chu kỳ của nó là (2), vì vậy cứ mỗi phút lại sáng theo kiểu tuần hoàn. 

| Bắt đầu (S) | Thắp sáng định kỳ | Đã xóa tiền tố lit | Thực tế sáng | Ông nội chồng chéo | Khả thi | 
| --- | --- | --- | --- | --- | --- | 
| -1 | 5 | 0 | 5 | 4 | Có | 
| 0 | 5 | 0 | 5 | 2 | Có | 
| 1 | 5 | 0 | 5 | 4 | Có | 
| 2 | 5 | 1 | 4 | 2 | Không | 

Ứng cử viên tốt nhất cho (A=1) là (S=0), có sự trùng lặp (2). Lớn hơn (A) không thể cải thiện câu trả lời, vì vậy kết quả cuối cùng là```
2 1 0
```Chi tiết quan trọng ở đây là sự khác biệt giữa (S=-1) và (S=1). Các giai đoạn định kỳ của chúng có liên quan với nhau, nhưng vòng hoa thực sự tối trước thời điểm chuyển đổi thực sự của nó. Phép trừ tiền tố nắm bắt được sự khác biệt đó. 

### Mẫu 2 

Đầu vào là```
8 2
1 3
5 7
```Với (A=2), chu kỳ là (4). Pha âm (S=-1) sáng phút (0,3,4,7), chính xác là bốn phút. 

| Bắt đầu (S) | Thắp sáng định kỳ | Đã xóa tiền tố lit | Thực tế sáng | Ông nội chồng chéo | Khả thi | 
| --- | --- | --- | --- | --- | --- | 
| -1 | 4 | 0 | 4 | 0 | Có | 
| 0 | 4 | 0 | 4 | 2 | Có | 
| 1 | 4 | 0 | 4 | 4 | Có | 
| 2 | 4 | 1 | 3 | 2 | Không | 

Sự khởi đầu tiêu cực sẽ tránh hoàn toàn cả hai khoảng thời gian của ông nội trong khi vẫn thắp sáng đúng một nửa thời gian nghỉ lễ. Do đó câu trả lời là```
0 2 -1
```Ví dụ này thực hiện phần thuật toán phải bảo toàn thời gian chuyển mạch âm thực sự thay vì thay thế chúng bằng các đại diện pha không âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T^2)) | Đối với mỗi (A), việc xây dựng phần dư và tất cả các cập nhật ứng cử viên lấy (O(N+T)) và (N\le T/2). | 
| Không gian | (O(T)) | Tất cả các mảng dư có độ dài tối đa (2A\le T) trong nhánh tuần hoàn. | 

Giá trị lớn nhất của (T) chỉ là (5000), vì vậy (O(T^2)) có nghĩa là khoảng (25) triệu phép toán tỷ lệ cho các phần chiếm ưu thế của thuật toán. Giải pháp không bao giờ quét tất cả (T) phút cho mỗi thời gian chuyển đổi riêng lẻ, đây là điểm khác biệt giữa phương pháp được chấp nhận và phương pháp cưỡng bức. 

## Trường hợp thử nghiệm```python
import io
import sys

def solve(data=None):
    if data is None:
        T, N = map(int, input().split())
        intervals = [tuple(map(int, input().split())) for _ in range(N)]
    else:
        it = iter(map(int, data.split()))
        T = next(it)
        N = next(it)
        intervals = [(next(it), next(it)) for _ in range(N)]

    diff_g = [0] * (T + 1)
    for l, r in intervals:
        diff_g[l] += 1
        diff_g[r] -= 1

    g = [0] * T
    cur = 0
    for t in range(T):
        cur += diff_g[t]
        g[t] = 1 if cur else 0

    pref_g = [0] * (T + 1)
    for t in range(T):
        pref_g[t + 1] = pref_g[t] + g[t]

    half_floor = T // 2
    half_ceil = (T + 1) // 2

    best_cost = T + 1
    best_a = 0
    best_s = 0

    def consider(cost, a, s):
        nonlocal best_cost, best_a, best_s
        if cost < best_cost:
            best_cost = cost
            best_a = a
            best_s = s
        elif cost == best_cost and a == best_a and s > best_s:
            best_s = s

    for A in range(1, T + 1):
        if 2 * A > T:
            lo = half_ceil - A

            for s in range(lo, half_floor + 1):
                left = max(0, s)
                right = min(T, s + A)
                lit = right - left

                if 2 * lit < T:
                    continue

                cost = pref_g[right] - pref_g[left]
                consider(cost, A, s)

            continue

        P = 2 * A

        home_diff = [0] * (P + 1)
        base = 0

        for l, r in intervals:
            length = r - l
            q, rem = divmod(length, P)
            base += q

            if rem:
                start = l % P
                end = start + rem

                if end <= P:
                    home_diff[start] += 1
                    home_diff[end] -= 1
                else:
                    home_diff[start] += 1
                    home_diff[P] -= 1
                    home_diff[0] += 1
                    home_diff[end - P] -= 1

        home = [0] * P
        cur = 0
        for r in range(P):
            cur += home_diff[r]
            home[r] = cur + base

        q, rem = divmod(T, P)
        cnt = [q + (1 if r < rem else 0) for r in range(P)]

        period_lit = sum(cnt[:A])
        period_home = sum(home[:A])

        cur_lit = period_lit
        cur_home = period_home

        for r in range(1, P):
            cur_lit += cnt[(r + A - 1) % P] - cnt[r - 1]
            cur_home += home[(r + A - 1) % P] - home[r - 1]

            s = -P + r
            if 2 * cur_lit >= T:
                consider(cur_home, A, s)

        cur_lit = period_lit
        cur_home = period_home

        pref_cnt = [0] * P
        pref_home = [0] * P

        if 2 * cur_lit >= T:
            consider(cur_home, A, 0)

        for s in range(half_floor):
            r1 = s % P
            r2 = (s + A) % P

            cur_lit += cnt[r2] - cnt[r1]
            cur_home += home[r2] - home[r1]

            removed_lit = pref_cnt[r2] - pref_cnt[r1]
            removed_home = pref_home[r2] - pref_home[r1]

            pref_cnt[r1] += 1
            pref_home[r1] += g[s]

            actual_lit = cur_lit - removed_lit
            actual_home = cur_home - removed_home

            ns = s + 1

            if 2 * actual_lit >= T:
                consider(actual_home, A, ns)

    return f"{best_cost} {best_a} {best_s}"

def run(inp: str) -> str:
    return solve(inp)

assert run("""10 2
1 4
7 10
""") == "2 1 0", "sample 1"

assert run("""8 2
1 3
5 7
""") == "0 2 -1", "sample 2"

assert run("""6 1
0 4
""") == "1 3 3", "sample 3"

assert run("""5 1
0 5
""") == "3 1 0", "sample 4"

assert run("""4 0
""") == "0 1 1", "sample 5"

assert run("""1 0
""") == "0 1 0", "minimum-size input"

assert run("""4 1
1 2
""") == "0 2 2", "boundary endpoint"

assert run("""6 2
0 2
4 6
""") == "2 1 1", "equal intervals"

assert run("""5000 0
""") == "0 1 1", "maximum T"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`0 1 0`| Ranh giới thời gian chuyển đổi và kỳ nghỉ nhỏ nhất có thể | 
|`4 1 / 1 2`|`0 2 2`| Điểm cuối khoảng thời gian chính xác và chuyển đổi sau khi ông nội rời đi | 
|`6 2 / 0 2 / 4 6`|`2 1 1`| Khoảng thời gian ông nội có độ dài bằng nhau và hòa theo lần xuất phát muộn nhất | 
|`5000 0`|`0 1 1`| Thời gian nghỉ lễ tối đa và thời gian bắt đầu muộn | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu```
1 0
```phút duy nhất phải được thắp sáng ít nhất một nửa thời gian nghỉ một phút, nghĩa là trọn một phút. Với (A=1), chuyển đổi ở đèn (S=0) ([0,1)), không có sự chồng chéo nào. Chuyển đổi ở mức (S=1) sẽ không sáng gì trong kỳ nghỉ, vì vậy câu trả lời là```
0 1 0
```Thuật toán đến nhánh (2A>T) ngay lập tức, kiểm tra (S=0) và loại bỏ (S=1) vì thời lượng sáng của nó bằng 0. 

Đối với trường hợp bắt đầu âm```
8 2
1 3
5 7
```ứng viên phù hợp là (A=2,S=-1). Vì (2A=T), nhánh tuần hoàn được sử dụng. Các đèn pha còn lại (0) và (3) modulo (4), cho bốn phút sáng ở các thời điểm (0,3,4,7). Không có khoảng nào thuộc về khoảng thời gian ông nội, vì vậy chi phí bằng không. Việc liệt kê pha phủ định sẽ trực tiếp tìm thấy ứng viên này. 

Đối với trường hợp điểm cuối```
4 1
1 2
```ứng viên (A=2,S=2) tạo ra khoảng sáng ([2,4)). Giao điểm của nó với khoảng thời gian ông ngoại ([1,2)) trống, trong khi ngày nghỉ chứa chính xác hai phút sáng. Nhánh chu kỳ lớn tính toán`left=2`,`right=4`, và thu được chi phí`pref_g[4] - pref_g[2] = 0`. 

Đối với trường hợp khoảng bằng nhau```
6 2
0 2
4 6
```nghiệm tốt nhất là (A=1,S=1). Vòng hoa được thắp sáng trong các phút (1,3,5), chính xác là ba phút sáng, thỏa mãn điều kiện nửa ngày nghỉ. Nó cắt các khoảng thời gian ông nội trong các phút (1) và (5), do đó chi phí là hai. Chuyển đổi ở (S=0) cũng mang lại chi phí hai, nhưng quy tắc hòa sẽ chọn thời điểm bắt đầu muộn hơn (S=1). 

Để có thời gian nghỉ lễ tối đa```
5000 0
```không có ông nội nên mục tiêu bằng không. Nhỏ nhất có thể (A) là (1). Với (S=1), vòng hoa được thắp sáng cách nhau mỗi phút từ (1) đến (4999), cho ra chính xác (2500) phút thắp sáng. Việc bắt đầu muộn hơn sẽ khiến ít hơn một nửa thời gian nghỉ lễ được thắp sáng. Câu trả lời kết quả là```
0 1 1
```Trường hợp cuối cùng này cũng thực hiện quy tắc ràng buộc khi nhiều cấu hình có chi phí bằng 0: thuật toán giữ nguyên (A=1), sau đó chọn thời gian chuyển đổi khả thi mới nhất cho điều đó (A).
