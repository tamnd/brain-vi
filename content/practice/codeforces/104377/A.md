---
title: "CF 104377A - \u8ba1\u7b97\u5f02\u6216\u548c"
description: "Chúng ta được yêu cầu xem xét tất cả các mảng có thứ tự có độ dài $m$ bao gồm các số nguyên không âm có tổng cố định là $n$. Mỗi mảng như vậy đóng góp một giá trị bằng XOR theo bit của tất cả các phần tử của nó và chúng ta cần tổng các giá trị XOR này trên tất cả các mảng hợp lệ."
date: "2026-07-01T17:21:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104377
codeforces_index: "A"
codeforces_contest_name: "The 21st Sichuan University Programming Contest"
rating: 0
weight: 104377
solve_time_s: 87
verified: true
draft: false
---

[CF 104377A - \u8ba1\u7b97\u5f02\u6216\u548c](https://codeforces.com/problemset/problem/104377/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xem xét tất cả các mảng có độ dài được sắp xếp$m$gồm các số nguyên không âm có tổng cố định bằng$n$. Mỗi mảng như vậy đóng góp một giá trị bằng XOR theo bit của tất cả các phần tử của nó và chúng ta cần tổng các giá trị XOR này trên tất cả các mảng hợp lệ. 

Một cách hữu ích để hình dung đầu vào là chúng ta đang phân phối$n$các đơn vị giống hệt nhau thành$m$những hộp có dán nhãn. Mỗi phân phối xác định một mảng$a_1, a_2, \dots, a_m$. Đối với mỗi phân phối, chúng tôi tính toán giá trị thứ hai: chúng tôi lấy biểu diễn nhị phân của mọi$a_i$, XOR chúng lại với nhau rồi cộng kết quả đó vào tổng số chung. 

Những hạn chế là điều khiến điều này trở nên thú vị. tổng$n$có thể lớn như$10^{15}$, vì vậy chúng ta không thể liệt kê các thành phần hoặc thậm chí lưu trữ bất cứ thứ gì tỷ lệ với$n$. Số lượng hộp$m$nhiều nhất là 500, đủ nhỏ để lập trình động tổ hợp theo từng vị trí là hợp lý. Bất kỳ giải pháp nào tùy thuộc vào việc lặp lại tất cả các tác phẩm hoặc sử dụng DP trên$n$trực tiếp là không khả thi ngay lập tức bởi vì ngay cả sự phụ thuộc tuyến tính vào$n$sẽ là quá chậm. 

Một cách tiếp cận ngây thơ cũng nhanh chóng bị phá vỡ theo những cách tinh tế. Ví dụ, nếu$m = 2$Và$n = 10$, các cặp hợp lệ đã có số 11 và mỗi cặp phải được XOR. Chia tỷ lệ này thành$n = 10^{15}$làm cho vũ lực hoàn toàn không thể thực hiện được, nhưng quan trọng hơn, thậm chí còn cố gắng tạo ra các trạng thái tăng dần bằng cách chia tách$n$dẫn đến sự bùng nổ theo cấp số nhân. 

Một trường hợp thất bại phổ biến khác là cố gắng xử lý các bit một cách độc lập. Nếu chúng ta giả định sai từng bit của$a_i$có thể được chỉ định độc lập chỉ với ràng buộc tổng trên mỗi bit, chúng tôi bỏ qua việc truyền lan truyền giữa các bit. Ví dụ,$a_i = 1$Và$a_i = 2$hoạt động rất khác nhau ở cấp độ bit mặc dù cả hai đều đóng góp vào nhiều vị trí bit thông qua cấu trúc bổ sung. 

Vì vậy khó khăn thực sự là điều kiện$\sum a_i = n$ghép tất cả các bit thông qua mang nhị phân, trong khi XOR chỉ phụ thuộc vào tính chẵn lẻ trên mỗi bit của các giá trị cột. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực trực tiếp sẽ tạo ra tất cả$m$-các bộ số nguyên không âm có tổng bằng$n$, sau đó tính XOR của chúng. Điều này về cơ bản là liệt kê tất cả các thành phần yếu của$n$vào trong$m$các bộ phận. Số bộ dữ liệu như vậy là$\binom{n+m-1}{m-1}$, vốn đã lớn về mặt thiên văn khi$n$tùy thuộc vào$10^{15}$. Ngay cả đối với nhỏ$n$, việc lặp lại tất cả các bộ dữ liệu hợp lệ là không khả thi và mỗi bộ dữ liệu yêu cầu$O(m)$làm việc để tính toán XOR. 

Quan sát cấu trúc quan trọng là hạn chế$\sum a_i = n$là ràng buộc cộng nhị phân. Mỗi vị trí bit đóng góp độc lập vào tổng, ngoại trừ vị trí mang. Điều này gợi ý việc xử lý các số theo từng bit, duy trì việc mang từ các bit thấp hơn, giống hệt như chữ số DP trên phép cộng. 

Tuy nhiên, chúng tôi không chỉ tính các phép gán hợp lệ mà còn đang tích lũy các giá trị XOR. XOR ở vị trí bit chỉ phụ thuộc vào số lượng$a_i$có 1 trong bit đó, cụ thể là tính chẵn lẻ của nó. Điều này cho phép chúng ta tách vấn đề thành các phần đóng góp trên mỗi bit, trong khi vẫn mang tính hợp lệ toàn cục thông qua ràng buộc bổ sung. 

Vì vậy, giải pháp trở thành DP trên các vị trí nhị phân, trong đó mỗi trạng thái theo dõi số cách chúng ta có thể hình thành các phép gán một phần các bit với một số mang nhất định vào vị trí tiếp theo và đồng thời tích lũy tổng đóng góp XOR được đóng góp bởi các bit thấp hơn đã cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong$n$|$O(m)$| Quá chậm | 
| Bitwise DP có mang |$O(60 \cdot m^2 \cdot \text{carry})$|$O(m \cdot \text{carry})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý biểu diễn nhị phân của$n$từ bit ít quan trọng nhất đến bit quan trọng nhất. Tại mỗi vị trí bit, chúng ta quyết định cách thức$m$các số phân phối số bit của chúng, đồng thời tôn trọng tổng số của chúng phải khớp với$n$bao gồm cả mang theo. 

Chúng tôi xác định trạng thái DP trong đó chúng tôi theo dõi số cách chúng tôi có thể đạt được mức mang nhất định sau khi xử lý tiền tố bit và mức đóng góp XOR đã được tích lũy từ các bit được xử lý. 

### bước 

1. Chúng tôi tính toán trước các hệ số nhị thức$C(m, k)$cho tất cả$0 \le k \le m$. Điều này là cần thiết vì ở mỗi bit chúng ta chọn chính xác có bao nhiêu$m$số có số 1 ở vị trí đó. 
2. Chúng tôi khởi tạo bảng DP trong đó trạng thái ban đầu không có phần mang và đóng góp XOR bằng 0, biểu thị không có bit được xử lý. 
3. Đối với từng vị trí bit$pos$từ 0 đến khoảng 60, chúng tôi lấy trạng thái DP hiện tại và thử tất cả các lựa chọn có thể có về số lượng trạng thái$k$xuất hiện giữa$m$số tại bit này. 

Lựa chọn$k$đóng góp một yếu tố kết hợp$C(m, k)$, vì chúng tôi đang chọn cái nào$k$chỉ số có bit 1. 
4. Đặt dòng điện mang vào bit này là$c$, và để$n_{bit}$là$pos$-bit thứ của$n$. Tổng số bit ở vị trí này trên tất cả các số là$k + c$. 

Tổng này phải thỏa mãn ràng buộc cộng nhị phân:$$k + c \equiv n_{bit} \pmod{2}$$và lần mang tiếp theo là:$$c' = \frac{k + c - n_{bit}}{2}$$5. Đối với mỗi lần chuyển đổi hợp lệ, chúng tôi cập nhật hai đại lượng. Đầu tiên, số cách nhân với$C(m, k)$. Thứ hai, sự đóng góp XOR tại bit này được xác định bởi liệu$k$thật kỳ quặc, vì XOR của$k$những cái là 1 nếu$k$thật kỳ quặc. Nếu bằng 1 thì ta cộng$2^{pos}$nhân với số cách góp phần vào quá trình chuyển đổi này. 
6. Chúng tôi tích lũy các chuyển đổi này sang trạng thái DP tiếp theo được lập chỉ mục bởi$c'$. 
7. Sau khi xử lý tất cả các bit, câu trả lời là tổng đóng góp XOR trên tất cả các trạng thái DP với số mang cuối cùng bằng 0. 

### Tại sao nó hoạt động 

DP thực thi tính chính xác của ràng buộc tổng chính xác như phép cộng nhị phân có nhớ. Mỗi bộ dữ liệu hợp lệ tương ứng với chính xác một chuỗi lựa chọn và mang bit. Hệ số tổ hợp đảm bảo chúng ta đếm tất cả các phép gán 1 bit trên toàn bộ$m$các vị trí một cách chính xác. Sự tích lũy XOR là tuyến tính trên các bit, do đó sự đóng góp từ các vị trí bit khác nhau không bao giờ gây nhiễu và có thể được tính tổng một cách an toàn trong quá trình chuyển đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m = map(int, input().split())

    max_c = m

    # precompute binomial coefficients
    C = [[0] * (m + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        C[i][0] = 1
        for j in range(1, i + 1):
            C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]) % MOD

    # dp[carry] = (ways, xor_sum)
    dp = [[0, 0] for _ in range(m + 1)]
    dp[0][0] = 1

    for bit in range(61):
        ndp = [[0, 0] for _ in range(m + 1)]
        nb = (n >> bit) & 1

        for carry in range(m + 1):
            ways, xs = dp[carry]
            if ways == 0:
                continue

            for k in range(m + 1):
                comb = C[m][k]
                if comb == 0:
                    continue

                total = k + carry
                if (total & 1) != nb:
                    continue

                nc = (total - nb) // 2
                if nc < 0 or nc > m:
                    continue

                nways = ways * comb % MOD

                # xor contribution from this bit
                if k & 1:
                    add_xor = (nways * ((1 << bit) % MOD)) % MOD
                else:
                    add_xor = 0

                ndp[nc][0] = (ndp[nc][0] + nways) % MOD
                ndp[nc][1] = (ndp[nc][1] + xs * comb % MOD + add_xor) % MOD

        dp = ndp

    print(dp[0][1] % MOD)

if __name__ == "__main__":
    solve()
```Mã xây dựng một chữ số-DP trên các vị trí nhị phân của$n$. Bảng nhị thức dùng để đếm xem có bao nhiêu cách gán$k$những người trong số$m$vị trí tại mỗi bit. Các rãnh trạng thái DP thực hiện sự lan truyền sao cho các số được xây dựng luôn có tổng chính xác bằng$n$. Sự tích lũy XOR được chia thành các đóng góp kế thừa từ các bit trước đó và các đóng góp mới từ bit hiện tại được tính trọng số bởi$2^{bit}$. 

Một điểm tinh tế là sự tách biệt của`ways`Và`xor_sum`. Tổng XOR phụ thuộc vào số lượng cách đạt đến một trạng thái, do đó, mọi chuyển đổi đều cập nhật theo cả cấp số nhân và cấp số cộng. Trộn những thứ này không chính xác là nguyên nhân phổ biến của việc đếm quá mức. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ$n = 4, m = 2$. Chúng tôi liệt kê các cặp hợp lệ:$(0,4), (1,3), (2,2), (3,1), (4,0)$. 

Chúng tôi theo dõi sự đóng góp theo từng chút một. 

| Cặp | XOR | 
| --- | --- | 
| (0,4) | 4 | 
| (1,3) | 2 | 
| (2,2) | 0 | 
| (3,1) | 2 | 
| (4,0) | 4 | 

Tổng cộng là 12. 

Theo thuật ngữ DP, tại mỗi bit, chúng ta chọn số lượng trong hai số mang số 1. Ràng buộc mang đảm bảo chính xác những cặp có tổng nhị phân khớp với 4 tồn tại qua tất cả các bit, trong khi đóng góp XOR được tích lũy khi chính xác một trong hai số có số 1 ở vị trí bit. 

Ví dụ thứ hai$n = 3, m = 2$đưa ra cặp$(0,3),(1,2),(2,1),(3,0)$. Giá trị XOR là$3,3,3,3$, tổng cộng là 12. Ở đây, mỗi phép gán hợp lệ có chính xác một bit đóng góp ở mỗi mẫu tính toán XOR và DP sẽ tính chính xác các đóng góp đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(60 \cdot m^2 \cdot m)$| Đối với mỗi bit, mỗi trạng thái mang sẽ thử tất cả$k$giá trị | 
| Không gian |$O(m)$| Các cửa hàng chỉ có DP mang theo trạng thái | 

Giới hạn$m \le 500$và khoảng 60 bit làm cho điều này trở nên khả thi. Tổng số lần chuyển đổi vào khoảng vài chục triệu, phù hợp thoải mái với giới hạn thời gian trong Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return sys.stdout.getvalue() if False else solve_wrapper(inp)

def solve_wrapper(inp: str) -> str:
    import sys
    from io import StringIO
    backup_stdin = sys.stdin
    sys.stdin = StringIO(inp)

    MOD = 10**9 + 7

    def solve():
        n, m = map(int, input().split())

        C = [[0] * (m + 1) for _ in range(m + 1)]
        for i in range(m + 1):
            C[i][0] = 1
            for j in range(1, i + 1):
                C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]) % MOD

        dp = [[0, 0] for _ in range(m + 1)]
        dp[0][0] = 1

        for bit in range(20):
            ndp = [[0, 0] for _ in range(m + 1)]
            nb = (n >> bit) & 1

            for c in range(m + 1):
                w, x = dp[c]
                if not w:
                    continue
                for k in range(m + 1):
                    comb = C[m][k]
                    if not comb:
                        continue
                    total = k + c
                    if (total & 1) != nb:
                        continue
                    nc = (total - nb) // 2
                    if nc < 0 or nc > m:
                        continue
                    nw = w * comb % MOD
                    add = (nw * ((1 << bit) % MOD)) % MOD if k & 1 else 0
                    ndp[nc][0] = (ndp[nc][0] + nw) % MOD
                    ndp[nc][1] = (ndp[nc][1] + x * comb % MOD + add) % MOD

            dp = ndp

        return str(dp[0][1] % MOD)

    return solve()

# provided sample
assert run("4 2") == "12"

# custom cases
assert run("1 1") == "1", "single configuration"
assert run("2 2") == "4", "symmetric splits"
assert run("0 2") == "0", "all zeros"
assert run("3 1") == "3", "single variable"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 2 | 12 | độ chính xác của mẫu | 
| 1 1 | 1 | cấu trúc khác không tối thiểu | 
| 2 2 | 4 | tính đối xứng trong bố cục | 
| 0 2 | 0 | trường hợp cạnh tổng bằng không | 
| 3 1 | 3 | trường hợp suy biến một biến | 

## Vỏ cạnh 

cho$n = 0$, tất cả các mảng buộc phải bằng 0. DP bắt đầu với số 0 và ngay lập tức chỉ truyền một cấu hình hợp lệ. Tại mỗi thời điểm, chỉ$k = 0$hợp lệ, do đó XOR luôn bằng 0 và đầu ra cuối cùng vẫn bằng 0. 

Vì$m = 1$, có đúng một biến$a_1 = n$. DP giảm xuống việc theo dõi một đường dẫn duy nhất trong đó việc mang luôn nhất quán với$n$, và XOR bằng$n$chính nó vì chỉ có một giá trị trong XOR. 

Đối với lớn$n$với biểu diễn nhị phân thưa thớt, hầu hết các trạng thái DP đều bị cắt bớt sớm vì các ràng buộc mang sẽ loại bỏ các chuyển đổi không hợp lệ. Điều này đảm bảo thuật toán hoạt động gần hơn với$O(60 \cdot m^2)$thay vì khám phá tất cả các trạng thái mang theo lý thuyết.
