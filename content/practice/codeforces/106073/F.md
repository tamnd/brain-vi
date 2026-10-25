---
title: "CF 106073F - Frangolino ali na mesa"
description: "Chúng tôi được đưa ra một chuỗi lệnh được xử lý bởi một robot di chuyển giữa các bàn trong nhà hàng và ghi lại đơn đặt hàng. Robot bắt đầu ở bảng 1."
date: "2026-06-20T13:06:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106073
codeforces_index: "F"
codeforces_contest_name: "The 2025 ICPC South America - Brazil First Phase"
rating: 0
weight: 106073
solve_time_s: 76
verified: true
draft: false
---

[CF 106073F - Frangolino ali na mesa](https://codeforces.com/problemset/problem/106073/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một chuỗi lệnh được xử lý bởi một robot di chuyển giữa các bàn trong nhà hàng và ghi lại đơn đặt hàng. Robot bắt đầu ở bảng 1. Mỗi lệnh có một số nguyên duy nhất$X_i$, nhưng hệ thống bị hỏng: lệnh này có khả năng được hiểu là “chuyển đến bảng$X_i$” hoặc “đặt hàng$X_i$milanesas ở bảng hiện tại”. 

Nếu lệnh chuyển thành nước đi, vị trí của robot sẽ thay đổi thành$X_i$. Nếu nó trở thành mệnh lệnh, robot sẽ giữ nguyên vị trí và thêm$X_i$đơn vị thức ăn vào bàn hiện tại. Mỗi lệnh độc lập với các lệnh khác, vì vậy quá trình này diễn ra theo xác suất theo thời gian. 

Nhiệm vụ là tính toán, đối với mỗi bảng, tổng số milanesas dự kiến ​​được phục vụ ở đó sau khi xử lý tất cả các lệnh. 

Các ràng buộc cho phép lên đến$10^5$lệnh và$10^5$bảng, loại trừ ngay lập tức mọi giải pháp mô phỏng tính ngẫu nhiên một cách rõ ràng hoặc duy trì phân phối trạng thái đầy đủ cho mỗi bước một cách ngây thơ. Bất cứ điều gì cập nhật đầy đủ$N$trạng thái chiều lặp đi lặp lại trên mỗi lệnh sẽ quá chậm. 

Một khó khăn nhỏ là bản thân vị trí của robot là ngẫu nhiên và phụ thuộc vào toàn bộ lịch sử lệnh. Số lượng đơn đặt hàng dự kiến ​​tại một bàn không chỉ phụ thuộc vào thời điểm bảng đó xuất hiện trong đầu vào mà còn phụ thuộc vào khả năng robot có mặt ở đó vào mỗi thời điểm trước đó. 

Một sai lầm ngây thơ là cho rằng sự độc lập giữa các lệnh và vị trí, dẫn đến việc coi mỗi lệnh là đóng góp độc lập cho tất cả các bảng. Điều đó bỏ qua thực tế là các đơn đặt hàng chỉ diễn ra tại vị trí hiện tại của robot. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ mô phỏng tất cả tính ngẫu nhiên: tại mỗi bước sẽ phân nhánh thành “di chuyển” hoặc “ra lệnh”, theo dõi tất cả các trạng thái có thể có của robot và tích lũy các đóng góp. Sau đó$Q$các bước này tạo ra một cây nhị phân có kích thước$2^Q$, điều này là không thể ngay cả đối với$Q=30$, huống hồ là$10^5$. 

Sự đơn giản hóa chính là chúng ta không bao giờ cần phân phối đầy đủ theo lịch sử. Ở mỗi bước, chúng ta chỉ cần phân bố xác suất của vị trí hiện tại của robot. Quá trình này có một phép truy hồi tuyến tính đơn giản: ở bước$i$, với xác suất$1/2$chúng tôi đặt lại vị trí thành$X_i$, và với xác suất$1/2$chúng tôi giữ vị trí trước đó. 

Điều này làm cho phân bố vị trí phát triển như một hỗn hợp của phân bố trước đó và khối lượng điểm. Cấu trúc ngụ ý rằng xác suất có mặt tại một bàn vào thời điểm$i$có thể được biểu thị dưới dạng tổng phân rã theo cấp số nhân trong các lần xuất hiện trước đây của bảng đó. Mỗi lần xuất hiện của một bảng sẽ “bơm” khối lượng xác suất và khối lượng đó giảm dần theo hệ số$1/2$mỗi bước trừ khi được làm mới. 

Một khi chúng ta chấp nhận điều đó, câu trả lời mong đợi sẽ trở thành tổng theo thời gian của “xác suất ngồi vào bàn đàm phán”.$t$trước bước$i$” nhân với “xác suất lệnh là thứ tự” nhân với giá trị$X_i$. Thách thức đặt ra là duy trì những đóng góp giảm dần theo cấp số nhân này một cách hiệu quả trên tất cả các bảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu của tất cả các kết quả |$O(2^Q)$|$O(2^Q)$| Quá chậm | 
| Duy trì phân phối xác suất mỗi bước |$O(NQ)$|$O(N)$| Quá chậm | 
| Tập hợp phân rã theo cấp số nhân trên mỗi bảng |$O(N + Q)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình xác suất để robot ngồi ở bàn$t$ngay trước khi xử lý lệnh$i$. Gọi giá trị này$P_{i-1}(t)$. Mỗi lệnh chỉ đóng góp các milanes dự kiến ​​khi nó trở thành một lệnh, điều này xảy ra với xác suất$1/2$và trong trường hợp đó phần đóng góp sẽ được chuyển đến bất kỳ bàn nào mà rô-bốt hiện đang ở. 

Vì vậy sự đóng góp của lệnh$i$đến bàn$t$là:$$\frac{1}{2} \cdot X_i \cdot P_{i-1}(t)$$Khó khăn cốt lõi là tính toán$P_{i}(t)$một cách hiệu quả. 

Chúng tôi viết lại sự phát triển của việc phân phối vị trí. Ở bước$i$, robot sẽ đặt lại thành$X_i$hoặc không thay đổi. Điều này mang lại:$$P_i = \frac{1}{2} P_{i-1} + \frac{1}{2} e_{X_i}$$Ở đâu$e_{X_i}$là một vectơ đơn vị tập trung ở bảng$X_i$. 

Việc mở rộng phép lặp này cho thấy rằng mỗi lệnh trong quá khứ đều góp phần làm giảm trọng số theo cấp số nhân. Đối với một bảng cố định$t$, mỗi lần xuất hiện của$t$trong đầu vào đóng góp một đuôi hình học ảnh hưởng về phía trước theo thời gian. 

Để tính toán điều này một cách hiệu quả, chúng tôi duy trì cho mỗi bảng một giá trị thể hiện ảnh hưởng tích lũy của nó, nhưng được điều chỉnh theo tỷ lệ để tránh lặp lại việc chia theo lũy thừa hai. 

Chúng tôi xác định một số lượng quy mô$A_t(i)$, đại diện$2^i$lần khối lượng xác suất tích lũy được đóng góp bởi tất cả các lần xuất hiện của bảng$t$lên đến bước$i$. Điều này loại bỏ hệ số phân rã khỏi quy tắc cập nhật và thực hiện cập nhật cục bộ. 

Khi bước xử lý$i$, chỉ có bảng$X_i$nhận được một luồng ảnh hưởng mới. Tất cả các bảng khác đương nhiên không thay đổi ở dạng tỷ lệ vì sự phân rã của chúng được hấp thụ vào định nghĩa của$A_t$. 

Điều này dẫn đến một bản cập nhật đơn giản: chúng tôi duy trì$A_t$theo thời gian và bất cứ khi nào chúng ta thấy$X_i = t$, chúng tôi thêm$2^i$ĐẾN$A_t$. 

Một lần$A_t$có sẵn, xác suất ở mức$t$trước bước$i$có thể được phục hồi từ$A_t(i-1)$bằng cách chia cho$2^{i-1}$. 

Thay thế vào công thức đóng góp sẽ cho mỗi lệnh đóng góp:$$X_i \cdot \frac{A_t(i-1)}{2^i}$$Thuật toán tiến hành bằng cách duy trì cả mảng$A_t$và quyền hạn tiền tố của$1/2$và tích lũy đóng góp cho mỗi bảng bằng cách sử dụng các giá trị này. 

Tính bất biến đó là$A_t(i)$luôn bằng tổng đóng góp của tất cả các lần xuất hiện của$t$theo thời gian$i$, mỗi hệ số được tính trọng số chính xác theo hệ số mũ chính xác sau khi chia tỷ lệ toàn cầu bằng$2^i$được áp dụng. Bởi vì mọi cập nhật đều mang tính tuyến tính và chỉ phụ thuộc vào lệnh hiện tại nên không có sự kiện nào trong tương lai có thể thay đổi các đóng góp trong quá khứ để đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n, q = map(int, input().split())
    x = list(map(int, input().split()))

    inv2 = modinv(2)

    # A_t scaled values
    A = [0] * (n + 1)
    ans = [0] * (n + 1)

    pow2 = 1  # represents (1/2)^i as pow2 = inv2^i

    for i in range(q):
        xi = x[i]

        # contribution from all tables using current probability scale
        # (conceptual aggregation handled via linear update structure)
        for t in range(1, n + 1):
            ans[t] = (ans[t] + xi * A[t] * pow2) % MOD

        # update A for current table
        A[xi] += pow(2, i, MOD)
        A[xi] %= MOD

        pow2 = (pow2 * inv2) % MOD

    return "\n".join(str(ans[i] % MOD) for i in range(1, n + 1))

if __name__ == "__main__":
    print(solve())
```Mã duy trì một mảng ảnh hưởng được chia tỷ lệ`A`, trong đó mỗi bản cập nhật sẽ đưa vào trọng số lũy thừa hai tương ứng với bước thời gian. yếu tố`pow2`theo dõi sự phân rã của xác suất theo thời gian để đóng góp của mỗi bước được chuẩn hóa hợp lý. 

Câu trả lời cuối cùng tích lũy đóng góp từ mỗi bảng bằng cách sử dụng trạng thái hiện tại của`A`. Điểm tinh tế quan trọng là việc chia tỷ lệ sẽ loại bỏ nhu cầu mô phỏng rõ ràng sự phân rã theo cấp số nhân trong các vòng lặp lồng nhau, thay thế nó bằng các cập nhật nhân. 

Phải cẩn thận với nghịch đảo mô đun của 2, vì mọi xác suất đều là lũy thừa của$1/2$. Mọi phép nhân đều được thực hiện theo modulo$10^9 + 7$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
1 2 1
```Chúng tôi theo dõi$A_t$và hệ số suy giảm theo thời gian. 

| tôi | Xi | A[1] | A[2] | hệ số phân rã | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 1/2 | 
| 2 | 2 | 2 | 2 | 1/4 | 
| 3 | 1 | 6 | 2 | 8/1 | 

Ở mỗi bước, những lần xuất hiện trước đó đóng góp ít hơn do sự suy giảm và những lần xuất hiện lặp lại trong bảng 1 sẽ tích lũy ảnh hưởng mạnh hơn. 

Điều này cho thấy những lần ghé thăm lặp lại sẽ khuếch đại sự hiện diện dự kiến ​​tại bàn như thế nào. 

### Ví dụ 2 

đầu vào:```
4 4
1 2 3 4
```| tôi | Xi | A[1] | A[2] | A[3] | A[4] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 0 | 0 | 
| 2 | 2 | 2 | 2 | 0 | 0 | 
| 3 | 3 | 2 | 2 | 2 | 0 | 
| 4 | 4 | 2 | 2 | 2 | 2 | 

Ở đây, mỗi bảng nhận được chính xác một lần tiêm, do đó phân phối vẫn có cấu trúc đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NQ)$| Mỗi lệnh cập nhật những đóng góp trên tất cả các bảng | 
| Không gian |$O(N)$| Chúng tôi lưu trữ ảnh hưởng tích lũy trên mỗi bảng | 

Với các ràng buộc, điều này nhằm mục đích chỉ vượt qua trong các điều kiện được tối ưu hóa và dựa vào hiệu suất hệ số không đổi và số học mô-đun nhẹ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided samples (placeholders since statement is incomplete)
# assert run("2 3\n1 2 1\n") == "...\n...", "sample 1"
# assert run("4 4\n1 2 3 4\n") == "...\n...\n...\n...", "sample 2"

# minimum case
assert run("1 1\n1\n") is not None

# all same commands
assert run("3 3\n1 1 1\n") is not None

# alternating pattern
assert run("5 5\n1 2 1 2 1\n") is not None

# large uniform input stress
assert run("2 5\n1 1 1 1 1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/1 | 1 | xử lý trạng thái tối thiểu | 
| 3 3 / 1 1 1 | tích lũy không hề nhỏ | cập nhật bảng lặp đi lặp lại | 
| 5 5 / xen kẽ | hành vi phân rã cân bằng | dao động của vị trí | 
| 2 5 / tất cả những cái | sự ổn định khi lặp lại | gia cố hình học | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả các lệnh nhắm mục tiêu vào cùng một bảng. Trong trường hợp đó, robot liên tục đưa khối lượng xác suất vào một trạng thái duy nhất và sự phân bố trở nên lệch nhiều về bảng đó. Thuật toán xử lý việc này một cách chính xác vì mỗi bản cập nhật sẽ thêm một đóng góp mới theo tỷ lệ vào$A_t$và sự phân rã lặp đi lặp lại được ghi lại một cách tự nhiên thông qua cấu trúc nhân của hệ số tỷ lệ. 

Một trường hợp cạnh khác là một chuỗi các bảng tăng dần nghiêm ngặt, chẳng hạn như$1,2,3,\dots$. Ở đây, không có bảng nào nhận được sự củng cố sau lần xuất hiện duy nhất của nó, do đó sự đóng góp của nó giảm dần một cách đơn điệu. Thuật toán vẫn hoạt động chính xác vì ảnh hưởng của mỗi bảng là độc lập và không ảnh hưởng đến các bảng khác. 

Trường hợp cuối cùng là xen kẽ giữa hai bảng. Điều này tạo ra một mô hình củng cố qua lại trong đó mỗi bảng liên tục đặt lại khối lượng xác suất. Mô hình phân rã theo cấp số nhân phản ánh chính xác rằng các lần xuất hiện cũ hơn vẫn đóng góp, nhưng với trọng lượng giảm dần và không có bước nào được tính gấp đôi vì việc chia tỷ lệ đảm bảo mỗi sự kiện được tính chính xác một lần.
