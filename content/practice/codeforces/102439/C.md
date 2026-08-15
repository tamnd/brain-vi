---
title: "CF 102439C - Đua Gián"
description: "Chúng ta có n con gián và mỗi con gián có m chữ số được viết trên lưng. Một số chữ số đã được biết đến, trong khi mọi chữ số ? có thể được thay thế độc lập bằng bất kỳ chữ số nào từ 0 đến 9. Cho phép sử dụng các số 0 đứng đầu."
date: "2026-08-12T08:11:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102439
codeforces_index: "C"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 102439
solve_time_s: 235
verified: true
draft: false
---

[CF 102439C - Đua gián](https://codeforces.com/problemset/problem/102439/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`gián, và mỗi con gián có một`m`số có chữ số viết ở mặt sau. Một số chữ số đã được biết đến, trong khi mọi`?`có thể được thay thế độc lập bằng bất kỳ chữ số nào từ`0`ĐẾN`9`. Số 0 đứng đầu được cho phép. 

Sau khi thay hết dấu chấm hỏi, số thu được phải thỏa mãn 

[ 
x_1 < x_2 < \dots < x_n. 
] 

Nhiệm vụ không phải là xây dựng một chuỗi như vậy. Chúng ta phải xem xét mọi khôi phục hợp lệ và cộng các giá trị của tất cả`n`những con số xuất hiện trong lần phục hồi đó. Câu trả lời cuối cùng là tổng modulo này (10^9+7). Các ràng buộc chính thức là (n,m\le 50), với giới hạn thời gian là 1,5 giây. 

Vì mọi số đều có độ dài bằng nhau nên việc so sánh hai số về mặt số cũng giống như so sánh các chuỗi chữ số của chúng về mặt từ điển. Vị trí đầu tiên nơi hai số khác nhau sẽ quyết định thứ tự của chúng. 

Giới hạn 50 đủ nhỏ để lập trình động theo từng khoảng thời gian của gián, nhưng quá nhỏ đối với bất kỳ số mũ nào trong cả hai`n`hoặc`m`. Nếu tất cả`n*m`vị trí là dấu chấm hỏi, có sự phục hồi (10^{nm}). Ở kích thước tối đa, đây là (10^{2500}) khả năng. Ngay cả việc kiểm tra một lần khôi phục trong thời gian (O(nm)) cũng sẽ đưa ra so sánh chữ số khoảng (2450\cdot10^{2500}), do đó, việc sử dụng vũ lực là hoàn toàn không thể. 

Có ba trường hợp đặc biệt có xu hướng gây ra lỗi thầm lặng. 

Đầu tiên, thứ tự tăng dần chứ không phải không giảm. Vì```
2 1
?
?
```các cặp hợp lệ là 45 cặp (0<1,0<2,\ldots,8<9) và sự kết hợp của chúng. Mỗi chữ số xuất hiện đúng 9 cặp hợp lệ, vì vậy câu trả lời là (9(0+1+\dots+9)=405). điều trị`<=`hợp lệ sẽ bao gồm các cặp bằng nhau không chính xác và tạo ra 495. 

Thứ hai, các số 0 đứng đầu là các chữ số thực và không được bỏ đi. Vì```
2 2
0?
10
```số đầu tiên có thể là`00`bởi vì`09`, còn số thứ hai là`10`. Tất cả mười lần khôi phục đều hợp lệ và tổng số lần khôi phục là 

[ 
(0+1+\dots+9)+10\cdot10=145. 
] 

Một triển khai diễn giải`0?`vì một đại diện không hợp lệ sẽ từ chối tất cả chúng một cách không chính xác. 

Thứ ba, bằng nhau ở chữ số đầu không có nghĩa là hai số bằng nhau. Vì```
2 2
??
??
```hai số có thể có cùng chữ số đầu tiên và vẫn tạo thành một cặp hợp lệ nếu chữ số thứ hai của chúng tăng dần. Trường hợp có tiền tố bằng nhau phải tiếp tục đến chữ số tiếp theo thay vì bị từ chối ngay lập tức. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thay thế từng dấu chấm hỏi một cách độc lập, xây dựng tất cả các số kết quả và kiểm tra xem chuỗi có tăng nghiêm ngặt hay không. Điều này đúng vì nó kiểm tra rõ ràng mọi khả năng phục hồi có thể. Vấn đề của nó là không gian tìm kiếm. Với`q`dấu chấm hỏi nó xem xét (10^q) bài tập và trong trường hợp xấu nhất (q=nm=2500). Ngay cả trước khi xem xét chi phí tính toán số tiền được yêu cầu, điều này mang lại (10^{2500}) ứng viên, do đó, vũ lực là không thể sử dụng được. 

Quan sát hữu ích là dãy tăng dần có cấu trúc rất cụ thể khi chúng ta kiểm tra vị trí một chữ số tại một thời điểm. 

Giả sử một khoảng gián liên tiếp hiện có cùng một tiền tố. Ở chữ số tiếp theo, chữ số được chọn của họ phải không giảm. Bất cứ khi nào chữ số tăng dần giữa hai con gián liên tiếp, thì hai bên đó đã được sắp xếp theo thứ tự mãi mãi nên khoảng chia thành hai nhóm độc lập. Những con gián nhận được cùng một chữ số vẫn nằm trong cùng một nhóm và phải được sắp xếp theo các hậu tố còn lại của chúng. 

Ví dụ: nếu bốn con gián hiện có chung tiền tố và các chữ số tiếp theo của chúng là 

[ 
2,2,5,8, 
] 

sau đó trình tự chia thành các nhóm`[1,2]`,`[3,3]`, Và`[4,4]`. Nhóm đầu tiên vẫn cần các hậu tố của nó tăng lên, trong khi hai nhóm còn lại, mỗi nhóm chứa một con gián và không cần đặt hàng thêm. 

Điều này biến tập hợp các trạng thái so sánh theo cấp số nhân thành một chương trình động theo khoảng. Định nghĩa`cnt[l][r]`là số cách hợp lệ để điền hậu tố còn lại khi gián`l`bởi vì`r`hiện có tiền tố giống hệt nhau. Đồng thời xác định`sum[l][r]`dưới dạng tổng của tất cả các giá trị số của chúng theo những cách đó, chỉ xem xét hậu tố còn lại. 

Ở một chữ số, một khoảng được chia thành các khối liên tiếp. Mỗi khối nhận được một chữ số, các chữ số khối tăng dần và hậu tố của các khối khác nhau là độc lập. một khối`[k,r]`được phép nhận chữ số`d`chính xác khi mọi mẫu từ`k`bởi vì`r`chấp nhận`d`ở vị trí này. 

Chúng ta có thể liệt kê khối cuối cùng và chữ số được gán cho nó. Các khối trước đó phải sử dụng chữ số nhỏ hơn. Vì chỉ có mười chữ số có thể nên chúng tôi giữ một chiều DP nhỏ khác cho chữ số cuối cùng. Điều này mang lại (O(10mn^3)) thời gian, rất thiết thực cho`n,m <= 50`. 

Cùng một DP có thể mang số tiền cũng như số đếm. Khi hai khối độc lập được kết hợp, số lượng của chúng sẽ nhân lên. Tổng của chúng kết hợp thành 

[ 
S = S_1C_2+C_1S_2. 
] 

Chữ số hiện tại đóng góp giá trị của nó nhân với lũy thừa thích hợp của mười, một lần cho mỗi con gián trong toàn bộ khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nm\cdot10^{nm})) | (O(nm)) | Quá chậm | 
| Khoảng thời gian DP | (O(10mn^3)) | (O(n^2+mn)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi vị trí mẫu thành mặt nạ mười bit. Chút`d`được thiết lập khi chữ số`d`có thể đặt ở vị trí đó Điều này làm cho việc kiểm tra xem toàn bộ khoảng có thể nhận được một chữ số hay không bằng một thao tác bit nguyên. 
2. Xử lý vị trí chữ số từ phải sang trái. Lúc đầu không còn chữ số nào. Do đó, khoảng chứa một con gián có một hậu tố trống hợp lệ, trong khi khoảng chứa ít nhất hai con gián không có hậu tố hợp lệ vì không được phép có số lượng đầy đủ bằng nhau. 
3. Đối với vị trí chữ số hiện tại, hãy xem xét một khoảng`[l,r]`có tiền tố trước đó giống hệt nhau đối với mọi con gián bên trong nó. Các chữ số hiện tại được chọn phải không giảm. Các chữ số bằng nhau tạo thành một khối, trong khi mức tăng nghiêm ngặt sẽ phân tách hai khối độc lập. 
4. Sửa chữ số`d`của khối cuối cùng`[k,r]`. Khối này chỉ hợp pháp nếu mọi mẫu từ`k`bởi vì`r`chấp nhận`d`. Đối với mỗi vị trí và chữ số, chúng tôi tính toán trước chỉ số đầu tiên mà khoảng kết thúc tại`r`có thể bao gồm toàn bộ các mẫu chấp nhận chữ số đó. Điều này tránh việc kiểm tra từng ký tự riêng biệt bên trong quá trình chuyển đổi. 
5. Phần`[l,k-1]`, nếu không trống thì phải kết thúc bằng chữ số nhỏ hơn`d`. Đối với mọi khả năng`k`, tiền tố DP cho chúng ta biết số cách và tổng hậu tố cho tất cả các khối trước đó. khối`[k,r]`đóng góp nó đã được tính toán`cnt[k][r]`Và`sum[k][r]`. 
6. Kết hợp phần trước và khối cuối cùng. Nếu số lượng của họ là`C1`Và`C2`, tổng số là`C1*C2`. Nếu tổng của chúng là`S1`Và`S2`, tổng hậu tố kết hợp là`S1*C2 + C1*S2`, bởi vì mọi phép gán của phần đầu tiên đều có thể được ghép nối với mọi phép gán của phần thứ hai. 
7. Sau khi tính DP cho chữ số hiện tại, hãy cộng chính chữ số hiện tại vào tổng. Nếu khoảng chứa`r-l+1`gián và vị trí hiện tại có giá trị vị trí (10^{m-1-p}), gán chữ số`d`đóng góp 

[ 
d\cdot(r-l+1)\cdot10^{m-1-p} 
] 

cho mọi nhiệm vụ được tính bởi tiểu bang đó. 

1. Sau khi tất cả các vị trí đã được xử lý,`cnt[0][n-1]`đếm mọi chuỗi hoàn chỉnh hợp lệ và`sum[0][n-1]`chính xác là tổng số được yêu cầu. 

### Tại sao nó hoạt động 

Tính bất biến đó là`cnt[l][r]`Và`sum[l][r]`mô tả chính xác tất cả các phép gán hậu tố hợp lệ cho gián`l..r`với giả định rằng các tiền tố đã được xử lý của chúng bằng nhau. Ở chữ số hiện tại, mọi phép gán hợp lệ đều có một phân vùng duy nhất thành các khối liên tiếp tối đa có các chữ số hiện tại bằng nhau. Các chữ số khối đó đang tăng lên một cách nghiêm ngặt và các khối khác nhau có thể được hoàn thành một cách độc lập. Quá trình chuyển đổi liệt kê mọi khối cuối cùng có thể có và mọi chữ số có thể có cho nó, vì vậy mỗi phép gán hợp lệ được tính chính xác một lần. Ngược lại, mọi chuyển đổi đều tạo ra các chữ số hiện tại không giảm và các hậu tố hợp lệ đệ quy, do đó nó tạo ra một chuỗi tăng nghiêm ngặt. Việc thực hiện cả số đếm và tổng thông qua cùng một quá trình phân tách sẽ cho kết quả tổng được yêu cầu mà không cần liệt kê các phép gán riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n, m = map(int, input().split())
    patterns = [input().strip() for _ in range(n)]

    # allowed[pos][i] is a bit mask of digits allowed for cockroach i.
    allowed = [[0] * n for _ in range(m)]

    for i in range(n):
        s = patterns[i]
        for p, ch in enumerate(s):
            if ch == '?':
                allowed[p][i] = (1 << 10) - 1
            else:
                allowed[p][i] = 1 << (ord(ch) - ord('0'))

    # next_cnt[l][r], next_sum[l][r]:
    # valid completions for positions p+1 ... m-1,
    # assuming positions before p are equal for l..r.
    next_cnt = [[0] * n for _ in range(n)]
    next_sum = [[0] * n for _ in range(n)]

    for i in range(n):
        next_cnt[i][i] = 1

    pow10 = [1] * m
    for i in range(1, m):
        pow10[i] = pow10[i - 1] * 10 % MOD

    for p in range(m - 1, -1, -1):
        # start[d][r] is the first k such that every pattern k..r
        # accepts digit d at position p.
        start = [[0] * n for _ in range(10)]
        last_bad = [-1] * 10

        for r in range(n):
            mask = allowed[p][r]
            for d in range(10):
                if not (mask >> d) & 1:
                    last_bad[d] = r
                start[d][r] = last_bad[d] + 1

        cur_cnt = [[0] * n for _ in range(n)]
        cur_sum = [[0] * n for _ in range(n)]

        for l in range(n):
            # fcnt[r][d]:
            # ways for [l,r] where the last current-digit block uses d.
            #
            # fsum[r][d]:
            # corresponding sum, including positions p+1..m-1,
            # but not yet the current digit at p.
            fcnt = [[0] * 10 for _ in range(n)]
            fsum = [[0] * 10 for _ in range(n)]

            # Prefix sums over the possible last digit.
            pref_cnt = [[0] * 10 for _ in range(n)]
            pref_sum = [[0] * 10 for _ in range(n)]

            weight = pow10[m - 1 - p]

            for r in range(l, n):
                row_sum = 0

                for d in range(10):
                    lo = max(l, start[d][r])
                    if lo > r:
                        continue

                    ways = 0
                    total = 0

                    for k in range(lo, r + 1):
                        block_cnt = next_cnt[k][r]
                        if block_cnt == 0:
                            continue

                        block_sum = next_sum[k][r]

                        if k == l:
                            prev_cnt = 1
                            prev_sum = 0
                        elif d == 0:
                            continue
                        else:
                            prev_cnt = pref_cnt[k - 1][d - 1]
                            prev_sum = pref_sum[k - 1][d - 1]

                        ways += prev_cnt * block_cnt
                        total += prev_sum * block_cnt + prev_cnt * block_sum

                    fcnt[r][d] = ways % MOD
                    fsum[r][d] = total % MOD

                    row_sum += fsum[r][d]

                # Build prefix sums for this r.
                pc = 0
                ps = 0
                for d in range(10):
                    pc += fcnt[r][d]
                    ps += fsum[r][d]
                    pref_cnt[r][d] = pc % MOD
                    pref_sum[r][d] = ps % MOD

                total_cnt = pc % MOD

                # Add the current digit to every cockroach in [l,r].
                size = r - l + 1
                digit_contribution = 0

                for d in range(10):
                    digit_contribution += (
                        d * size * weight * fcnt[r][d]
                    )

                cur_cnt[l][r] = total_cnt
                cur_sum[l][r] = (
                    row_sum + digit_contribution
                ) % MOD

        next_cnt = cur_cnt
        next_sum = cur_sum

    print(next_sum[0][n - 1] % MOD)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ chuyển đổi mọi vị trí mẫu thành mặt nạ chữ số. MỘT`?`nhận được tất cả mười bit, trong khi một chữ số cố định nhận được chính xác một bit. DP không bao giờ phải kiểm tra lại các ký tự gốc nữa.`next_cnt`Và`next_sum`đại diện cho trạng thái sau khi vị trí hiện tại đã bị xóa. Các mục theo đường chéo được khởi tạo thành một vì một con gián luôn có chính xác một hậu tố trống. Các khoảng có độ dài ít nhất hai bắt đầu từ 0 vì các số hoàn chỉnh bằng nhau không thể đáp ứng một thứ tự nghiêm ngặt. 

Đối với mỗi vị trí,`start[d][r]`ghi lại khoảng cách còn lại của một khối kết thúc tại`r`có thể mở rộng trong khi vẫn cho phép chữ số`d`. Nếu mẫu`r`từ chối`d`, không có khối nào như vậy tồn tại. Mặt khác, ranh giới được xác định bởi mẫu từ chối gần đây nhất`d`. Đây là điều kiện hiệu lực khoảng thời gian được sử dụng bởi quá trình chuyển đổi. 

Đối với điểm xuất phát cố định`l`,`fcnt[r][d]`Và`fsum[r][d]`mô tả tất cả các phân vùng của`[l,r]`khối cuối cùng của nó nhận được chữ số`d`. Quá trình chuyển đổi chọn sự khởi đầu`k`của khối cuối cùng đó. Phần trước phải kết thúc bằng chữ số nhỏ hơn, đó là lý do tại sao`pref_cnt[k-1][d-1]`Và`pref_sum[k-1][d-1]`được sử dụng. 

các`k == l`case đại diện cho một tiền tố trống trước khối cuối cùng. Số của nó là một và tổng của nó bằng không. Trường hợp biên này là cần thiết cho các khoảng có khối đầu tiên chiếm toàn bộ khoảng. 

Quá trình chuyển đổi tổng sử dụng phép nhân số lượng vì hai khối độc lập sau khi các chữ số hiện tại của chúng khác nhau. Số tiền tương ứng được sử dụng`prev_sum * block_cnt + prev_cnt * block_sum`, điều này giải thích thực tế là mỗi bài tập ở một bên có thể được ghép nối với mọi bài tập ở bên kia. 

Cuối cùng,`digit_contribution`thêm chữ số hiện tại vào mỗi con gián trong khoảng thời gian. Phép nhân với`pow10[m-1-p]`là thứ chuyển đổi một chữ số thành giá trị vị trí số thực của nó. Số nguyên Python không bị tràn, nhưng tất cả các giá trị DP đều được giảm theo modulo (10^9+7) nên các giá trị trung gian vẫn đủ nhỏ để tính toán hiệu quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 2
??
??
```Ở vị trí cuối cùng, hai con gián duy nhất có mười lựa chọn. Đối với khoảng chứa cả hai con gián, các chữ số cuối cùng phải thỏa mãn`a < b`, cho 45 khả năng. 

| Vị trí | Khoảng thời gian | Đếm | Tổng hậu tố | 
| --- | --- | --- | --- | 
| 1 |`[0,0]`| 10 | 45 | 
| 1 |`[1,1]`| 10 | 45 | 
| 1 |`[0,1]`| 45 | 405 | 
| 0 |`[0,0]`| 100 | 4500 | 
| 0 |`[1,1]`| 100 | 4500 | 
| 0 |`[0,1]`| 4950 | 490050 | 

Ở vị trí 0, hai chữ số đầu tiên khác nhau ngay lập tức thiết lập thứ tự, trong khi các chữ số đầu tiên bằng nhau để lại hậu tố so sánh với trạng thái được tính cho vị trí 1. Tổng cuối cùng là```
490050
```phù hợp với mẫu. 

### Mẫu 2 

Đầu vào là```
2 3
4??
??2
```Ở chữ số cuối cùng, con gián đầu tiên có thể sử dụng`0`hoặc`1`khi các tiền tố vẫn bằng nhau, vì con gián thứ hai buộc phải sử dụng`2`. Điều này mang lại hai cặp hậu tố và tổng hậu tố của`5`. 

Ở vị trí tiếp theo, hai chữ số có thể khác nhau hoặc có thể bằng nhau và để vị trí cuối cùng quyết định thứ tự. 

| Vị trí | Khoảng thời gian | Đếm | Tính tổng từ hậu tố hiện tại trở đi | 
| --- | --- | --- | --- | 
| 2 |`[0,1]`| 2 | 5 | 
| 1 |`[0,1]`| 470 | 45275 | 
| 0 |`[0,1]`| 5470 | 6403775 | 

Ở vị trí 0, số đầu tiên bắt đầu bằng`4`. Nếu số thứ hai bắt đầu bằng`5`bởi vì`9`, thứ tự đã được quyết định và cả hai hậu tố còn lại đều độc lập. Nếu nó cũng bắt đầu bằng`4`, trạng thái hậu tố với 470 lần hoàn thành hợp lệ sẽ được sử dụng lại. Tổng hợp các trường hợp này sẽ cho ra câu trả lời cuối cùng`6403775`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(10mn^3)) | Đối với mỗi vị trí chữ số, khoảng bắt đầu, khoảng kết thúc, chữ số và ranh giới khối cuối cùng có thể có, một chuyển đổi được xử lý. | 
| Không gian | (O(n^2+mn)) | Hai`n x n`Các lớp DP được lưu trữ cùng với các mặt nạ đầu vào và các mảng phụ trợ nhỏ cho mỗi vị trí. | 

Với (n,m\le50), hệ số bậc ba có thể quản lý được vì thứ nguyên chữ số chỉ có 10 và các chuyển đổi khoảng thời gian chỉ chứa khoảng (O(n^3)) kết hợp cho mỗi vị trí. Thuật toán tránh mọi sự phụ thuộc vào (10^{nm}), đây là yêu cầu chính cho những ràng buộc này. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1_000_000_007

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    patterns = [input().strip() for _ in range(n)]

    allowed = [[0] * n for _ in range(m)]

    for i in range(n):
        s = patterns[i]
        for p, ch in enumerate(s):
            if ch == '?':
                allowed[p][i] = (1 << 10) - 1
            else:
                allowed[p][i] = 1 << (ord(ch) - ord('0'))

    next_cnt = [[0] * n for _ in range(n)]
    next_sum = [[0] * n for _ in range(n)]

    for i in range(n):
        next_cnt[i][i] = 1

    pow10 = [1] * m
    for i in range(1, m):
        pow10[i] = pow10[i - 1] * 10 % MOD

    for p in range(m - 1, -1, -1):
        start = [[0] * n for _ in range(10)]
        last_bad = [-1] * 10

        for r in range(n):
            mask = allowed[p][r]
            for d in range(10):
                if not ((mask >> d) & 1):
                    last_bad[d] = r
                start[d][r] = last_bad[d] + 1

        cur_cnt = [[0] * n for _ in range(n)]
        cur_sum = [[0] * n for _ in range(n)]

        weight = pow10[m - 1 - p]

        for l in range(n):
            fcnt = [[0] * 10 for _ in range(n)]
            fsum = [[0] * 10 for _ in range(n)]
            pref_cnt = [[0] * 10 for _ in range(n)]
            pref_sum = [[0] * 10 for _ in range(n)]

            for r in range(l, n):
                row_sum = 0

                for d in range(10):
                    lo = max(l, start[d][r])
                    if lo > r:
                        continue

                    ways = 0
                    total = 0

                    for k in range(lo, r + 1):
                        block_cnt = next_cnt[k][r]
                        if block_cnt == 0:
                            continue

                        block_sum = next_sum[k][r]

                        if k == l:
                            prev_cnt = 1
                            prev_sum = 0
                        elif d == 0:
                            continue
                        else:
                            prev_cnt = pref_cnt[k - 1][d - 1]
                            prev_sum = pref_sum[k - 1][d - 1]

                        ways += prev_cnt * block_cnt
                        total += prev_sum * block_cnt
                        total += prev_cnt * block_sum

                    fcnt[r][d] = ways % MOD
                    fsum[r][d] = total % MOD
                    row_sum += fsum[r][d]

                pc = 0
                ps = 0

                for d in range(10):
                    pc += fcnt[r][d]
                    ps += fsum[r][d]
                    pref_cnt[r][d] = pc % MOD
                    pref_sum[r][d] = ps % MOD

                size = r - l + 1
                digit_sum = 0

                for d in range(10):
                    digit_sum += d * size * weight * fcnt[r][d]

                cur_cnt[l][r] = pc % MOD
                cur_sum[l][r] = (row_sum + digit_sum) % MOD

        next_cnt = cur_cnt
        next_sum = cur_sum

    print(next_sum[0][n - 1] % MOD)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("""2 2
??
??
""") == "490050", "sample 1"

assert run("""2 3
4??
??2
""") == "6403775", "sample 2"

assert run("""4 1
0
?
4
8
""") == "42", "sample 3"

# Minimum-size input
assert run("""1 1
?
""") == "45", "single question mark"

# Strict inequality boundary
assert run("""2 1
?
?
""") == "405", "strictly increasing, not nondecreasing"

# Leading zeroes are valid
assert run("""2 2
0?
10
""") == "145", "leading zeroes"

# Fixed increasing sequence
assert run("""3 1
1
2
3
""") == "6", "fixed increasing sequence"

# All equal values give no valid sequence
assert run("""3 1
7
7
7
""") == "0", "equal values"

# Maximum-size input, deliberately impossible
maximum = "50 50\n" + ("0" * 50 + "\n") * 50
assert run(maximum) == "0", "maximum-size impossible input"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / ?`|`45`| Kích thước tối thiểu và DP một khoảng | 
|`2 1 / ? / ?`|`405`| Bất bình đẳng nghiêm ngặt và từ chối giá trị ngang nhau | 
|`2 2 / 0? / 10`|`145`| Số 0 đứng đầu | 
|`3 1 / 1 / 2 / 3`|`6`| Đã sửa hoàn toàn trình tự hợp lệ | 
|`3 1 / 7 / 7 / 7`|`0`| Giá trị hoàn toàn bằng nhau | 
|`50 50`với tất cả các số 0 |`0`| Đầu vào có kích thước tối đa và các khoảng không thể | 

## Vỏ cạnh 

Trường hợp bất đẳng thức chặt chẽ```
2 1
?
?
```bắt đầu với trạng thái cơ bản trong hai khoảng thời gian một phần tử. Ở vị trí chữ số duy nhất, hai phần tử phải tạo thành hai khối khác nhau, với chữ số của khối thứ nhất nhỏ hơn chữ số của khối thứ hai. Có 45 cặp chữ số như vậy. Tổng của hai chữ số trên tất cả các cặp là 405, do đó DP trả về chính xác`405`. Các chữ số bằng nhau không bao giờ bước vào quá trình chuyển đổi vì vị trí chữ số cuối cùng không còn hậu tố để phân tách chúng. 

Đối với các số 0 đứng đầu,```
2 2
0?
10
```mẫu đầu tiên đại diện cho mười giá trị`00,01,...,09`, trong khi thứ hai là`10`. Số đầu tiên luôn nhỏ hơn nên có 10 dãy hợp lệ. Tổng số của họ là 145. DP chiêu đãi`0`như một chữ số được phép thông thường và không bao giờ chuyển đổi mẫu thành số nguyên sớm, do đó số 0 đứng đầu không gây ra trường hợp đặc biệt nào. 

Đối với tiền tố bằng nhau,```
2 2
??
??
```chữ số đầu tiên có thể bằng nhau đối với cả hai con gián. Trong trường hợp đó, khoảng vẫn là một khối duy nhất và DP sử dụng trạng thái từ chữ số tiếp theo. Ở chữ số thứ hai, chỉ những cặp tăng nghiêm ngặt mới tồn tại. Đây chính xác là lý do tại sao phép truy toán có thể xử lý các tiền tố bằng nhau mà không lưu trữ trạng thái so sánh rõ ràng cho mọi cặp liền kề. 

Đối với một chuỗi không thể như```
3 1
7
7
7
```trạng thái cơ sở không chứa khoảng thời gian hợp lệ có độ dài lớn hơn một. Việc xử lý chữ số duy nhất không thể chia ba con gián thành các chữ số khác nhau vì mọi mẫu chỉ chấp nhận`7`. Kết quả đếm và tổng đều bằng 0. 

Đối với đầu vào hoàn toàn bằng 0 có kích thước tối đa, mọi khoảng chứa hai hoặc nhiều con gián vẫn không thể thực hiện được ở mọi vị trí. DP vẫn xử lý toàn bộ`50 x 50`ví dụ, nhưng tất cả các trạng thái không đơn lẻ đều bằng không. Câu trả lời là do đó`0`và mức sử dụng bộ nhớ vẫn là bậc hai trong`n`.
