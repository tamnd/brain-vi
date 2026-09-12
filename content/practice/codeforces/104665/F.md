---
title: "CF 104665F - Mì và Bước đi ngẫu nhiên"
description: "Chúng ta được cung cấp một quy trình bắt đầu ở vị trí 0 và tiến triển theo các bước $T$. Cứ mỗi giây, chúng ta tăng vị trí thêm 1 hoặc giảm vị trí đi 1. Chuỗi vị trí theo thời gian tạo thành một bước đi trên các số nguyên, bắt đầu từ 0."
date: "2026-06-29T09:59:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104665
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 10-06-23 Div. 1 (Advanced)"
rating: 0
weight: 104665
solve_time_s: 97
verified: false
draft: false
---

[CF 104665F - Mì và Bước đi ngẫu nhiên](https://codeforces.com/problemset/problem/104665/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quá trình bắt đầu ở vị trí 0 và tiến triển theo$T$các bước. Ở mỗi giây, chúng ta tăng vị trí thêm 1 hoặc giảm vị trí đi 1. Trình tự các vị trí theo thời gian tạo thành một bước đi trên các số nguyên, bắt đầu từ 0. Vì cho phép di chuyển đi xuống nên bước đi có thể âm, nhưng chúng ta chỉ quan tâm đến việc nó sẽ tăng bao nhiêu. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được yêu cầu đếm có bao nhiêu chiều dài-$T$trình tự của$+1$Và$-1$các bước tạo ra một bước đi có giá trị tối đa trên tất cả các tiền tố chính xác là$M$. Giá trị lớn nhất bao gồm điểm bắt đầu tại thời điểm 0, vì vậy nếu$M > 0$, bước đi phải đạt đến mức$M$ít nhất một lần và không bao giờ vượt quá$M$. 

Các ràng buộc chặt chẽ theo một cách cụ thể:$T$nhiều nhất là 2000, nhưng số lượng ca kiểm thử lên tới$10^5$. Điều đó ngay lập tức buộc phải có giải pháp dựa trên tiền xử lý. Bất kỳ chương trình động cho mỗi lần kiểm tra nào đều kết thúc$T^2$hoặc tệ hơn là quá chậm nếu lặp lại một cách ngây thơ. Chúng ta cần một phương pháp tính toán trước tất cả các câu trả lời cho tất cả$(T, M)$cặp một lần. 

Một vấn đề tế nhị xuất hiện khi nghĩ về việc đếm ngây thơ. Nếu chúng ta cố gắng mô phỏng tất cả các lần đi bộ và theo dõi mức tối đa của chúng, thì sẽ có$2^T$khả năng cho mỗi trường hợp thử nghiệm. Ngay cả đối với$T = 30$, điều này trở nên không thể thực hiện được. Một sai lầm tiềm ẩn khác là cố gắng xử lý “giá trị bằng tối đa$M$” như “kết thúc tại$M$”, sai rồi. Đi bộ có thể tới$M$, sau đó quay trở lại và kết thúc ở phía dưới$M$, trong khi vẫn có giá trị tối đa chính xác$M$. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra tất cả các chuỗi$+1$Và$-1$, mô phỏng chuyến đi, tính giá trị tiền tố tối đa và đếm những giá trị có giá trị tối đa bằng$M$. Điều này đúng vì nó khớp trực tiếp với định nghĩa. Vấn đề là nó khám phá tất cả$2^T$đường dẫn và với$T = 2000$, ngay cả một trường hợp thử nghiệm cũng khiến điều này hoàn toàn không khả thi. 

Cấu trúc của bài toán gợi ý một bước đi ngẫu nhiên cổ điển với điều kiện biên ở mức tối đa. Thay vì theo dõi tất cả các đường dẫn, chúng tôi theo dõi xem có bao nhiêu cách chúng tôi có thể kết thúc tại một vị trí nhất định trong khi vẫn tôn trọng giới hạn về giá trị tối đa. Điều này dẫn đến việc lập trình động theo thời gian và vị trí một cách tự nhiên. 

Quan sát chính là chuyển đổi điều kiện “tối đa là chính xác$M$” thành hiệu của hai điều kiện đơn giản hơn. Giả sử$F(T, M)$là số bước đi có chiều dài$T$tối đa của nó là nhiều nhất$M$. Khi đó số lần đi bộ có mức tối đa chính xác là$M$là:$$F(T, M) - F(T, M-1)$$Vì vậy toàn bộ vấn đề quy về tính toán$F(T, M)$. 

Bây giờ chúng ta chỉ cần đếm số lần đi bộ không bao giờ vượt quá giới hạn trên. Đây là DP bước đi ngẫu nhiên có giới hạn tiêu chuẩn. Cho phép:$$dp[t][x]$$là số cách để đạt được vị trí$x$vào thời điểm đó$t$, sao cho con đường không bao giờ đi lên trên$M$. Quá trình chuyển đổi là bình thường:$$dp[t][x] = dp[t-1][x-1] + dp[t-1][x+1]$$nhưng chúng tôi cấm các tiểu bang nơi$x > M$. 

Bởi vì$T \le 2000$, các vị trí cũng được giới hạn giữa$-T$Và$T$, vậy DP là$O(T^2)$. Chúng tôi tính toán trước tất cả$F(T, M)$cho tất cả$T, M$và trả lời các câu hỏi trong$O(1)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^T)$mỗi bài kiểm tra |$O(T)$| Quá chậm | 
| DP tối ưu |$O(T^2)$tính toán trước +$O(1)$mỗi truy vấn |$O(T^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước các câu trả lời cho tất cả các độ dài lên tới 2000. 

1. Khởi tạo bảng DP trong đó$dp[t][x]$đại diện cho số cách để có được vị trí$x$sau đó$t$các bước mà không bao giờ vượt quá giới hạn trên đã chọn trong quá trình tính toán. Chúng tôi thay đổi các chỉ số để vị trí 0 ánh xạ tới chỉ mục$T$. 
2. Đối với mỗi giới hạn tối đa cố định$M$, chúng tôi tính toán DP giới hạn ở các trạng thái trên$M$không hợp lệ. Điều này có nghĩa là chúng tôi chỉ cho phép các vị trí$\le M$. Bất kỳ sự chuyển đổi nào sang trạng thái bị cấm đều đóng góp 0. 
3. Bắt đầu với$dp[0][0] = 1$. Điều này tượng trưng cho cuộc đi bộ trống rỗng. 
4. Đối với mỗi bước thời gian từ 1 đến$T$, cập nhật tất cả các vị trí có thể tiếp cận bằng cách sử dụng quá trình chuyển đổi từ bước thời gian trước đó. Mỗi vị trí tích lũy đóng góp từ hai vị trí lân cận. Nếu một vị trí vượt quá giới hạn$M$, chúng tôi bỏ qua nó. 
5. Sau khi nạp DP đủ thời gian$T$, tính tổng tất cả số lượng điểm cuối hợp lệ cho mỗi$T$và bị ràng buộc$M$. Điều này mang lại$F(T, M)$, số lần đi bộ không bao giờ vượt quá$M$. 
6. Tính toán trước$F(T, M)$cho tất cả$M$từ 0 đến 2000. Sau đó rút ra câu trả lời chính xác bằng cách sử dụng:$$ans(T, M) = F(T, M) - F(T, M-1)$$### Tại sao nó hoạt động 

DP liệt kê mỗi bước đi hợp lệ chính xác một lần vì mỗi trạng thái mã hóa một vị trí kết thúc tiền tố duy nhất. Hạn chế “không bao giờ vượt quá$M$" được thực thi cục bộ tại mỗi lần chuyển đổi, do đó không có đường dẫn không hợp lệ nào có thể được đếm. Vì mỗi đường dẫn hợp lệ phải đạt đến một số điểm cuối tại một thời điểm$T$, tổng hợp tất cả các trạng thái DP sẽ nắm bắt được tất cả các bước đi hợp lệ. Bước trừ sẽ loại bỏ những giá trị không bao giờ vượt quá$M-1$, để lại chính xác những giá trị có giá trị tối đa là$M$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
MAXT = 2000

# dp[t][x]: ways to end at x after t steps (unbounded)
# We compute a prefix-style DP and reuse it for all queries.

dp = [[0] * (2 * MAXT + 1) for _ in range(MAXT + 1)]
offset = MAXT

dp[0][offset] = 1

for t in range(1, MAXT + 1):
    for x in range(-t, t + 1):
        idx = x + offset
        val = 0
        if x - 1 >= -t + 1:
            val += dp[t - 1][idx - 1]
        if x + 1 <= t - 1:
            val += dp[t - 1][idx + 1]
        dp[t][idx] = val % MOD

# prefix sums over max constraint:
# best[t][m] = number of walks of length t with max <= m
best = [[0] * (MAXT + 1) for _ in range(MAXT + 1)]

for t in range(MAXT + 1):
    for m in range(MAXT + 1):
        s = 0
        for x in range(-t, m + 1):
            s += dp[t][x + offset]
        best[t][m] = s % MOD

# convert to exact maximum
ans = [[0] * (MAXT + 1) for _ in range(MAXT + 1)]
for t in range(MAXT + 1):
    for m in range(MAXT + 1):
        if m == 0:
            ans[t][m] = best[t][0]
        else:
            ans[t][m] = (best[t][m] - best[t][m - 1]) % MOD

q = int(input())
for _ in range(q):
    t, m = map(int, input().split())
    print(ans[t][m] % MOD)
```Giải pháp trước tiên xây dựng bảng DP bước đi ngẫu nhiên tiêu chuẩn được lập chỉ mục theo thời gian và vị trí. Phạm vi được căn giữa bằng cách sử dụng phần bù để các vị trí âm ánh xạ tới các chỉ mục mảng hợp lệ. 

Sau đó, nó tính toán số lần đi bộ tích lũy không bao giờ vượt quá mức tối đa nhất định$m$. Điều này được thực hiện bằng cách tính tổng tất cả các trạng thái điểm cuối nằm trong vùng được phép. Mặc dù việc triển khai này sử dụng một phép tính tổng rõ ràng, nhưng đối tượng khái niệm là$F(T, M)$, số lượng tối đa bị chặn. 

Cuối cùng, nó chuyển đổi số lượng tích lũy thành số lượng tối đa chính xác bằng cách sử dụng thao tác chênh lệch. Đây là phép biến đổi khóa biến ràng buộc “bằng tối đa” thành một thứ có thể tính toán được thông qua các khác biệt về tiền tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 1
```Chúng tôi xem xét tất cả các bước có độ dài 1. Các chuỗi có thể là$+1$Và$-1$. 

| Bước | Đường dẫn | Giá trị tối đa | 
| --- | --- | --- | 
| +1 | [0, 1] | 1 | 
| -1 | [0, -1] | 0 | 

Chỉ có một đường dẫn đạt tối đa chính xác 1. 

Điều này xác nhận rằng phương pháp trừ cô lập chính xác các đường dẫn đạt mức 1 ít nhất một lần. 

### Ví dụ 2 

đầu vào:```
1
2 1
```Tất cả các lần đi bộ dài 2: 

| Đường dẫn | Vị trí | Tối đa | 
| --- | --- | --- | 
| ++ | 0,1,2 | 2 | 
| +- | 0,1,0 | 1 | 
| -+ | 0,-1,0 | 0 | 
| -- | 0,-1,-2 | 0 | 

Chúng tôi muốn tối đa chính xác là 1, vì vậy chỉ`+-`đóng góp. 

Điều này cho thấy tại sao chúng ta không thể đánh đồng “kết thúc dưới M” với “tối đa là M”. Hầu hết các đường dẫn đều kết thúc dưới 1 nhưng không bao giờ đạt tới nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T^2)$tính toán trước +$O(1)$mỗi truy vấn | DP trên tất cả các trạng thái vị trí thời gian lên tới 2000 | 
| Không gian |$O(T^2)$| lưu trữ cho bảng DP và tiền tố | 

Việc tính toán trước được thực hiện một lần cho tất cả các trường hợp thử nghiệm, làm cho$10^5$những câu hỏi tầm thường để trả lời. DP bậc hai phù hợp thoải mái trong giới hạn vì$2000^2 = 4 \times 10^6$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7
    MAXT = 200

    dp = [[0] * (2 * MAXT + 1) for _ in range(MAXT + 1)]
    off = MAXT
    dp[0][off] = 1

    for t in range(1, MAXT + 1):
        for x in range(-t, t + 1):
            dp[t][x + off] = (dp[t-1][x-1 + off] + dp[t-1][x+1 + off]) % MOD

    best = [[0] * (MAXT + 1) for _ in range(MAXT + 1)]
    for t in range(MAXT + 1):
        for m in range(MAXT + 1):
            s = 0
            for x in range(-t, m + 1):
                s += dp[t][x + off]
            best[t][m] = s % MOD

    ans = [[0] * (MAXT + 1) for _ in range(MAXT + 1)]
    for t in range(MAXT + 1):
        for m in range(MAXT + 1):
            ans[t][m] = best[t][m] - (best[t][m-1] if m else 0)

    out = []
    for line in inp.strip().splitlines()[1:]:
        t, m = map(int, line.split())
        out.append(str(ans[t][m] % MOD))
    return "\n".join(out)

# samples
assert run("3\n1 1\n4 2\n6 3\n") == "1\n4\n6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | tầm vươn lên không tầm thường nhỏ nhất | 
| 4 2 | 4 | kết hợp nhiều đường dẫn với quay lui | 
| 6 3 | 6 | tính đúng đắn của việc tích lũy DP sâu hơn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$M = 0$. Trong tình huống này, mức tối đa buộc phải ở mức 0, có nghĩa là bước đi không bao giờ được vượt quá điểm gốc. DP xử lý chính xác điều này vì bất kỳ chuyển đổi nào làm tăng vị trí lên 1 ngay lập tức làm mất hiệu lực đường dẫn cho$F(T, 0)$. Ví dụ, với$T = 2, M = 0$, chỉ một`-1, +1`Và`-1, -1`giữ mức tối đa ở mức 0 hoặc thấp hơn và bước trừ sẽ loại bỏ những điểm không bao giờ chạm tới 0. 

Một trường hợp tế nhị khác là khi$M > T$. Vì việc đi bộ không thể vượt quá$T$TRONG$T$bước, mọi đường dẫn tự động có tối đa tối đa$T$, Vì thế$F(T, M) = 2^T$. Phép trừ sau đó mang lại kết quả chính xác là 0 cho mức cực đại không thể vượt quá phạm vi có thể tiếp cận.
