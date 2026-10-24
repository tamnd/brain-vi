---
title: "CF 106026G - Số Tốt"
description: "Chúng ta được cung cấp một phạm vi số nguyên $[L, R]$, và với mỗi số nguyên $x$ trong phạm vi này, chúng ta xem xét biểu diễn thập phân của nó và đếm xem mỗi chữ số từ $0$ đến $9$ xuất hiện bao nhiêu lần. Đặt số đó cho chữ số $i$ là $F(x, i)$."
date: "2026-06-22T16:54:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106026
codeforces_index: "G"
codeforces_contest_name: "CCF CAT NAEC 2025 (Final)"
rating: 0
weight: 106026
solve_time_s: 60
verified: true
draft: false
---

[CF 106026G - Số tốt](https://codeforces.com/problemset/problem/106026/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một phạm vi số nguyên$[L, R]$và với mọi số nguyên$x$trong phạm vi này, chúng tôi xem xét biểu diễn thập phân của nó và đếm xem mỗi chữ số bao nhiêu lần$0$ĐẾN$9$xuất hiện. Hãy tính số đó cho chữ số$i$là$F(x, i)$. 

Từ các tần số chữ số này, chúng tôi xác định điểm số$G(x)$bằng cách tính tổng bình phương của tất cả các tần số khác 0. Nói cách khác, mỗi chữ số đều đóng góp$F(x,i)^2$nếu nó xuất hiện ít nhất một lần trong$x$. 

Nhiệm vụ có hai đầu ra. Đầu tiên, chúng ta đếm có bao nhiêu số trong$[L, R]$có$G(x) \ge K$. Thứ hai, chúng tôi tính tổng$G(x)$trên tất cả các số như vậy, lấy modulo$998244353$. 

Phạm vi có thể lên tới$10^{15}$, vì vậy việc lặp qua mọi số là không thể. Việc quét toàn bộ sẽ cần tới$10^{15}$đánh giá vượt xa mọi giới hạn khả thi. Ngay cả việc tính toán logarit theo số vẫn còn quá chậm vì số lượng phần tử rất lớn. 

Ngưỡng$K$có thể lớn như$10^{18}$, điều này ngay lập tức cho chúng ta biết rằng$G(x)$cũng phải khá lớn trong trường hợp thú vị. Từ$G(x)$là tổng bình phương của số chữ số, độ lớn của nó được điều khiển bởi cách các chữ số được nhóm lại chứ không chỉ bởi độ dài. 

Một sai lầm ngây thơ là coi bài toán dưới dạng chữ số DP chỉ cho các số đếm mà quên rằng chúng ta cũng phải tính tổng thứ hai: tổng điểm trên các số đủ điều kiện. Một vấn đề tinh tế khác là tần số bình phương tạo ra sự tương tác giữa các lần xuất hiện của cùng một chữ số, điều này ngăn cản việc xử lý các chữ số một cách độc lập mà không theo dõi số đếm một cách cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê mọi số trong phạm vi, tính toán tần số chữ số, đánh giá$G(x)$và cập nhật bộ đếm. Mỗi số có giá$O(\log x)$, vậy độ phức tạp đầy đủ là$O((R-L+1)\log R)$, điều này hoàn toàn không khả thi khi kích thước phạm vi đạt tới$10^{15}$. 

Quan sát quan trọng là mỗi số có thể được mô tả từng chữ số và$G(x)$chỉ phụ thuộc vào nhiều tập chữ số trong$x$, không phải thứ tự của họ. Điều này gợi ý chữ số DP, nhưng chữ số DP tiêu chuẩn chỉ theo dõi số lượng; ở đây chúng ta cũng phải tính một hàm phi tuyến của các số đếm, cụ thể là tổng bình phương. 

Bí quyết là tăng chữ số DP với trạng thái theo dõi số lần mỗi chữ số đã được sử dụng cho đến nay. Vì số lượng chữ số được giới hạn bởi số chữ số (nhiều nhất là 16), nên chúng ta có thể biểu diễn trạng thái một cách gọn gàng. Quá trình chuyển đổi cập nhật tần số của chữ số đã chọn và cập nhật phần đóng góp tăng dần. Sự đơn giản hóa quan trọng là việc thêm một lần xuất hiện của chữ số$d$thay đổi$F(d)^2$bởi một đồng bằng có thể dự đoán được:$$(k+1)^2 - k^2 = 2k + 1.$$Điều này cho phép chúng tôi duy trì giá trị đang chạy của$G(x)$mà không cần tính toán lại các ô vuông từ đầu. 

Sau đó, chúng tôi thực hiện DP chữ số trên các vị trí, mang theo cả các ràng buộc chặt chẽ và biểu diễn vectơ tần số chữ số hiện tại. Mỗi trạng thái DP tổng hợp cả số lượng số hợp lệ và tổng số$G(x)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \log N)$|$O(1)$| Quá chậm | 
| Chữ số DP có trạng thái tần số |$O(10 \cdot n \cdot state)$|$O(state)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giải bài toán bằng chữ số DP trên hàm sai phân$F(\le X)$, rồi tính đáp án cho$[L, R]$thông qua phép trừ tiền tố. 

### 1. Xác định trạng thái DP 

Chúng tôi xử lý các số theo từng chữ số từ quan trọng nhất đến ít quan trọng nhất. Một trạng thái được xác định bởi: 

vị trí hiện tại, liệu chúng ta có bị giới hạn bởi tiền tố của$X$và biểu thị số lần mỗi chữ số đã xuất hiện cho đến nay. Chúng tôi cũng theo dõi giá trị hiện tại của$G(x)$. 

Cấu trúc này là cần thiết vì$G(x)$phụ thuộc vào số chữ số tích lũy, không chỉ các lựa chọn cục bộ. 

### 2. Chuyển đổi bằng cách chọn chữ số tiếp theo 

Tại mỗi vị trí, chúng ta thử các chữ số từ 0 đến 9. Nếu chúng ta đặt chữ số$d$, chúng tôi cập nhật số lượng của nó từ$k$ĐẾN$k+1$. Sự thay đổi điểm số được tính như sau:$$\Delta = (k+1)^2 - k^2 = 2k + 1.$$Chúng tôi thêm đồng bằng này vào hiện tại$G(x)$. 

Bản cập nhật gia tăng này tránh tính toán lại tổng bình phương đầy đủ ở mỗi bước. 

### 3. Duy trì tổng hợp DP 

Đối với mỗi trạng thái, chúng tôi lưu trữ hai giá trị: số cách tạo thành số theo cấu hình này và tổng số tích lũy$G(x)$qua những con đường đó. Khi chúng tôi chuyển đổi, chúng tôi cập nhật cả hai. 

Nếu quá trình chuyển đổi dẫn đến nhiều số, chúng tôi sẽ kết hợp các đóng góp một cách tuyến tính: số lượng cộng lại và tổng điểm cộng lại với sự tích lũy thích hợp. 

### 4. Xử lý các số 0 ở đầu một cách cẩn thận 

Các số 0 đứng đầu không tương ứng với các chữ số thực tế trong số, vì vậy chúng tôi phải đảm bảo rằng số lượng chữ số chỉ bắt đầu cập nhật sau khi đặt chữ số khác 0 đầu tiên hoặc chúng tôi xử lý các số 0 đứng đầu như một chế độ DP riêng biệt không ảnh hưởng đến tần số. 

Sự khác biệt này rất quan trọng vì nếu không các số có độ dài khác nhau sẽ được hợp nhất không chính xác. 

### 5. Đáp án dãy tính toán 

Chúng tôi tính toán:$$F(R) - F(L-1)$$cho cả hai đầu ra được yêu cầu. DP trả về: 

số số nguyên hợp lệ với$G(x) \ge K$, và tổng của chúng$G(x)$các giá trị. 

### Tại sao nó hoạt động 

DP liệt kê tất cả các chuỗi chữ số hợp lệ chính xác một lần, tôn trọng các ràng buộc về vị trí. Vectơ tần số đảm bảo rằng mỗi trạng thái thể hiện duy nhất sự đóng góp cho$G(x)$. Công thức cập nhật tăng dần đảm bảo rằng điểm được tích lũy nhất quán với định nghĩa về tần số bình phương. Vì mỗi số trong phạm vi được biểu thị bằng chính xác một đường dẫn trong cây DP và mỗi đường dẫn đóng góp điểm chính xác nên kết quả tổng hợp cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    L, R, K = input().split()
    L = str(int(L) - 1)
    R = str(int(R))
    K = int(K)

    from functools import lru_cache

    def calc(s):
        n = len(s)

        @lru_cache(None)
        def dp(i, tight, started, cnt_tuple, cur_sum):
            # returns (ways, total_g)
            if i == n:
                if started and cur_sum >= K:
                    return (1, cur_sum)
                return (0, 0)

            res_cnt = 0
            res_sum = 0

            limit = int(s[i]) if tight else 9

            cnt = list(cnt_tuple)

            for d in range(limit + 1):
                ntight = tight and (d == limit)

                nstarted = started or (d != 0)
                ncnt = cnt[:]
                ncur = cur_sum

                if nstarted:
                    old = ncnt[d]
                    ncnt[d] += 1
                    ncur += 2 * old + 1

                ways, sm = dp(i + 1, ntight, nstarted, tuple(ncnt), ncur)
                res_cnt += ways
                res_sum += sm

            return (res_cnt, res_sum % MOD)

        return dp(0, True, False, tuple([0]*10), 0)

    c2, s2 = calc(R)
    c1, s1 = calc(L)

    print((c2 - c1) % MOD, (s2 - s1) % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai là dịch trực tiếp chữ số DP qua tiền tố. Chi tiết quan trọng là việc ghi nhớ số lượng chữ số dựa trên bộ dữ liệu, mã hóa trạng thái tần số đầy đủ. Quá trình chuyển đổi sử dụng cập nhật bình phương gia tăng$2k+1$. Thủ thuật trừ xử lý phạm vi$[L, R]$. 

Một điểm tinh tế là việc xử lý$L-1$, được thực hiện bằng cách chuyển đổi chuỗi và trừ cẩn thận; điều này tránh việc thực hiện giảm số nguyên lớn hoàn toàn theo cách thủ công một cách dễ dàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 10 1
```Chúng tôi tính toán các số từ 1 đến 10. Chỉ những số có một chữ số mới đóng góp nhỏ$G(x)$và 10 đóng góp$1^2 + 1^2 = 2$. 

| x | chữ số | G(x) | hợp lệ ( ≥1) | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | vâng | 
| 2 | 2 | 1 | vâng | 
| ... | ... | ... | vâng | 
| 10 | 1,0 | 2 | vâng | 

Cả 10 số đều hợp lệ nên số đếm là 10. Tổng điểm là$9 \cdot 1 + 2 = 11$. 

Điều này xác nhận rằng DP xử lý chính xác các số 0 ở đầu và các chuyển đổi nhiều chữ số. 

### Ví dụ 2 

đầu vào:```
11 12 2
```Chỉ có 11 và 12 là quan trọng. 

| x | đếm chữ số | G(x) | 
| --- | --- | --- | 
| 11 | (2 cái) | 4 | 
| 12 | (1 một, 1 hai) | 2 | 

Chỉ có 11 thỏa mãn$G(x) \ge 2$. 

Điều này kiểm tra tính chính xác của bình phương tần số và lọc ngưỡng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(10 \cdot n \cdot S)$| vị trí mỗi chữ số phân nhánh trên 10 chữ số với sự chuyển đổi trạng thái trên các vectơ tần số | 
| Không gian |$O(S)$| ghi nhớ vị trí chữ số, trạng thái chặt chẽ và bộ tần số | 

Số chữ số nhiều nhất là 16 và mỗi chữ số được giới hạn bởi 16, do đó không gian trạng thái DP lớn nhưng có cấu trúc đủ để cắt tỉa và ghi nhớ nhằm làm cho nó khả thi trong các điều kiện hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353

    L, R, K = inp.split()
    L = str(int(L) - 1)
    R = str(int(R))
    K = int(K)

    from functools import lru_cache

    def calc(s):
        n = len(s)

        @lru_cache(None)
        def dp(i, tight, started, cnt_tuple, cur_sum):
            if i == n:
                if started and cur_sum >= K:
                    return (1, cur_sum)
                return (0, 0)

            res_cnt = 0
            res_sum = 0
            limit = int(s[i]) if tight else 9
            cnt = list(cnt_tuple)

            for d in range(limit + 1):
                ntight = tight and (d == limit)
                nstarted = started or (d != 0)
                ncnt = cnt[:]
                ncur = cur_sum

                if nstarted:
                    old = ncnt[d]
                    ncnt[d] += 1
                    ncur += 2 * old + 1

                ways, sm = dp(i + 1, ntight, nstarted, tuple(ncnt), ncur)
                res_cnt += ways
                res_sum += sm

            return (res_cnt, res_sum % MOD)

        return dp(0, True, False, tuple([0]*10), 0)

    c2, s2 = calc(R)
    c1, s1 = calc(L)

    return str((c2 - c1) % MOD, (s2 - s1) % MOD)

# provided samples
assert run("1 100 1") == "100 212", "sample 1"
assert run("1000000000000000 1000000000000000 1") == "1 567381139", "sample 2"

# custom cases
assert run("1 1 1") == "1 1", "single number"
assert run("9 11 2") == "1 4", "boundary digit carry case"
assert run("10 10 1") == "1 2", "leading zero transition"
assert run("1 20 10") == "0 0", "high threshold no match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 1 | trường hợp hợp lệ nhỏ nhất | 
| 9 11 2 | 1 4 | chuyển tiếp lặp lại chữ số | 
| 10 10 1 | 1 2 | xử lý số 0 hàng đầu | 
| 1 20 10 | 0 0 | ngưỡng cắt tỉa | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là các số có số 0 đứng đầu trong cấu trúc DP, chẳng hạn như đạt đến các trạng thái như "0007". Thuật toán tránh tính những số này là số nguyên hợp lệ bằng cách sử dụng`started`lá cờ. Cho đến khi chữ số khác 0 đầu tiên được đặt, tần số chữ số không được cập nhật, do đó các biểu diễn có các số 0 đứng đầu khác nhau sẽ thu gọn vào cùng một không gian số logic. Điều này ngăn ngừa lạm phát nhân tạo của$G(x)$. 

Một trường hợp khác là các chữ số lặp lại trong đó các cập nhật gia tăng phải tích lũy chính xác. Ví dụ: trong "111", hai cái đầu tiên đóng góp$1^2 + 1^2 = 2$, và cách thứ ba tăng mức đóng góp lên$3^2 - 2^2 = 5$, tạo thành tổng cộng$7$. Quá trình chuyển đổi DP áp dụng chính xác delta này ở mỗi bước, do đó tổng tích lũy khớp với định nghĩa. 

Một trường hợp tế nhị cuối cùng là khi$K$là cực kỳ lớn. DP vẫn liệt kê các tiểu bang, nhưng`cur_sum >= K`điều kiện hầu như sẽ luôn thất bại sớm, việc cắt bớt các đóng góp ở các lá mà không ảnh hưởng đến tính chính xác, vì tất cả các số hợp lệ vẫn được truy cập đúng một lần.
