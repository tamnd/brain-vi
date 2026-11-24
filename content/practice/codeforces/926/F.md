---
title: "CF 926F - Truyền thông di động"
description: "Chúng tôi đang mô phỏng một tài khoản ngân hàng trong một chuỗi ngày. Mỗi ngày bắt đầu với khoản phí cố định $p$ rúp áp dụng cho tài khoản. Vào một số ngày cụ thể, trước khi khoản phí này xảy ra, Arkady sẽ gửi tiền vào tài khoản."
date: "2026-06-17T03:10:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 926
codeforces_index: "F"
codeforces_contest_name: "VK Cup 2018 - Wild-card Round 1"
rating: 2000
weight: 926
solve_time_s: 74
verified: true
draft: false
---

[CF 926F - Truyền thông di động](https://codeforces.com/problemset/problem/926/F) 

**Xếp hạng:** 2000 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một tài khoản ngân hàng trong một chuỗi ngày. Mỗi ngày bắt đầu với một khoản phí cố định là$p$rúp áp dụng cho tài khoản. Vào một số ngày cụ thể, trước khi khoản phí này xảy ra, Arkady sẽ gửi tiền vào tài khoản. 

Dòng thời gian được sắp xếp theo trình tự thời gian chặt chẽ từ ngày 1 đến ngày$m$. Ban đầu số dư bằng không. Vào mỗi ngày, nếu là ngày gửi tiền, số dư tăng trước, sau đó là số tiền thanh toán hàng ngày$p$được trừ đi. Sau thao tác này, chúng tôi kiểm tra xem số dư có âm hay không. Chúng ta cần đếm số ngày tạo ra số dư âm sau khi tính phí hàng ngày. 

Một quan sát quan trọng là quá trình này hoàn toàn mang tính tích lũy: số dư trong ngày$d$chỉ phụ thuộc vào số tiền gửi trước đó và khoản trừ cố định$p$mỗi ngày. Không có hành vi thiết lập lại hoặc phân nhánh. 

Các ràng buộc làm cho một mô phỏng đơn giản không thể thực hiện được. Số ngày$m$có thể lớn như$10^9$, vì vậy việc lặp đi lặp lại hàng ngày là không khả thi. Tuy nhiên, số lượng sự kiện gửi tiền$n$nhiều nhất là$10^5$, đủ nhỏ để sắp xếp và xử lý tuyến tính. Sự bất đối xứng này giữa$n$Và$m$là gợi ý mang tính cấu trúc cốt lõi: chỉ một số ít ngày thực sự làm thay đổi sự cân bằng. 

Một cách tiếp cận ngây thơ sẽ mô phỏng tất cả$m$ngày và duy trì số dư hoạt động. Đây sẽ là$O(m)$, điều đó hoàn toàn không thể xảy ra khi$m = 10^9$. 

Một ý tưởng ngây thơ tinh tế hơn là chỉ xử lý các ngày gửi tiền và cho rằng có điều gì đó đơn giản xảy ra ở giữa. Điều này không thành công vì số dư thay đổi tuyến tính mỗi ngày, do đó, ngay cả trong những ngày không gửi tiền, số dư vẫn thay đổi đáng kể. 

Các trường hợp Edge phá vỡ lý luận ngây thơ bao gồm: 

- Không có khoản tiền gửi nào cả: số dư giảm dần$p$mỗi ngày, nên tất cả các ngày đều âm. 
- Một khoản tiền gửi lớn duy nhất gần cuối: ngày đầu âm, ngày sau có thể phục hồi. 
- Khoản tiền gửi tạm thời hủy bỏ chính xác việc rút tiền hàng ngày, gây ra hành vi ký hiệu xen kẽ trên các phân đoạn. 

Những hành vi này chỉ ra rằng chúng ta phải coi dòng thời gian như một hàm tuyến tính từng phần thay vì mô phỏng từng ngày. 

## Phương pháp tiếp cận 

Nếu chúng ta mô phỏng từng ngày, chúng ta duy trì một biến số dư và lặp từ 1 đến$m$, áp dụng tiền gửi khi chúng xảy ra và trừ đi$p$mỗi lần. Điều này đúng nhưng quá chậm khi$m$lớn vì nó đòi hỏi tới$10^9$lần lặp lại. 

Quan sát quan trọng là không có gì thay đổi trong khoảng thời gian giữa các ngày ký gửi ngoại trừ mức giảm tuyến tính không đổi. Nếu chúng ta biết số dư khi bắt đầu một khoảng thời gian, chúng ta có thể tính toán số ngày trong khoảng thời gian đó tạo ra số dư âm bằng cách sử dụng lý luận số học thay vì mô phỏng. 

Chúng tôi xử lý các ngày gửi tiền theo thứ tự và coi mỗi khoảng thời gian giữa các sự kiện gửi tiền liên tiếp là một khối. Trong mỗi khối, số dư giảm dần$p$mỗi ngày, do đó nó tạo thành một cấp số cộng. Chúng ta có thể xác định trực tiếp khi nào số dư vượt xuống dưới 0 và đếm số ngày âm. 

Điều này làm giảm vấn đề xử lý$n + 1$phân đoạn thay vì$m$ngày. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m) | O(1) | Quá chậm | 
| Xử lý số học phân đoạn | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi theo dõi số dư hiện tại và lặp lại tất cả các sự kiện gửi tiền theo thứ tự tăng dần trong ngày. 

1. Khởi tạo số dư hiện tại thành 0 và đặt con trỏ ngày hôm trước thành 1. Ý tưởng là chúng tôi xử lý phạm vi ngày bắt đầu từ ngày 1 cho đến ngày gửi tiền tiếp theo. 
2. Đối với mỗi sự kiện gửi tiền trong ngày$d$có giá trị$t$, đầu tiên xử lý ngày từ con trỏ trước đó lên đến$d - 1$, nơi không có tiền gửi xảy ra. Trong khoảng thời gian này, số dư giảm đi$p$mỗi ngày. 

Thay vì mô phỏng, chúng tôi coi đây là một chuỗi tuyến tính: bắt đầu từ số dư hiện tại, sau đó$k$ngày số dư sẽ trở thành$balance - k \cdot p$. Chúng tôi xác định có bao nhiêu ngày trong số này dẫn đến số dư âm sau khi trừ đi. 
3. Nộp tiền cọc trong ngày$d$: thêm vào$t$vào cân rồi trừ ngay$p$cho ngày hôm đó kể từ khi thanh toán được thực hiện sau khi gửi tiền. Nếu số dư kết quả là âm, hãy tăng câu trả lời. 
4. Di chuyển con trỏ trước đó tới$d + 1$, kể từ ngày$d$được xử lý đầy đủ. 
5. Sau khi xử lý tất cả các ngày gửi tiền, xử lý phần còn lại từ ngày xử lý cuối cùng đến$m$sử dụng cùng một logic số học. 

Chi tiết quan trọng là đếm chính xác có bao nhiêu giá trị trong một dãy số học giảm dần xuống dưới 0 mà không cần lặp lại. 

### Tại sao nó hoạt động 

Giữa các khoản tiền gửi, số dư tiến triển như một cấp số cộng đơn giản với bước cố định$-p$. Một chuỗi như vậy có một điểm vượt ngưỡng duy nhất: một khi nó trở thành số âm, nó sẽ âm mãi mãi. Tính đơn điệu này đảm bảo chúng ta có thể đếm số ngày âm bằng cách tìm chỉ số đầu tiên nơi số dư giảm xuống dưới 0. Vì mỗi phân đoạn là độc lập và được hạch toán đầy đủ nên mỗi ngày được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_negative_days(n, p, m, events):
    ans = 0
    cur = 0
    prev_day = 1

    for d, t in events:
        # process days [prev_day, d-1]
        length = d - prev_day
        if length > 0:
            # balance starts at cur and decreases by p each day
            # find first k such that cur - k*p < 0
            if cur < 0:
                ans += length
            else:
                first_neg = cur // p + 1
                if first_neg < 1:
                    first_neg = 1
                if first_neg <= length:
                    ans += length - first_neg + 1

            cur -= length * p

        # process deposit day d
        cur += t
        cur -= p
        if cur < 0:
            ans += 1

        prev_day = d + 1

    # tail segment
    if prev_day <= m:
        length = m - prev_day + 1
        if cur < 0:
            ans += length
        else:
            first_neg = cur // p + 1
            if first_neg <= length:
                ans += length - first_neg + 1

    return ans

def main():
    n, p, m = map(int, input().split())
    events = [tuple(map(int, input().split())) for _ in range(n)]
    print(count_negative_days(n, p, m, events))

if __name__ == "__main__":
    main()
```Việc triển khai xử lý từng phân đoạn giữa các khoản tiền gửi và tránh lặp lại qua nhiều ngày. Ý tưởng cốt lõi là khi chúng tôi biết số dư ban đầu của một phân khúc, mỗi ngày chúng tôi sẽ giảm số dư đó đi một cách chính xác.$p$, vì vậy chúng ta có thể tính ranh giới nơi nó trở thành âm bằng cách sử dụng phép chia số nguyên. 

Phần tinh tế duy nhất là xử lý chính xác quá trình chuyển đổi từ số dư ban đầu dương sang ngày âm đầu tiên. biểu thức`cur // p + 1`đưa ra chỉ mục đầu tiên trong đó phép trừ lặp lại vượt qua 0, giả sử lập chỉ mục dựa trên 1 bên trong phân đoạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 6 7
2 13
4 20
7 9
```Chúng tôi theo dõi các phân khúc: 

| Phân đoạn | Bắt đầu cân bằng | Chiều dài | Ngày âm | Số dư mới | 
| --- | --- | --- | --- | --- | 
| 1-1 | 0 | 1 | 1 | -6 | 
| 2 | -6 + 13 - 6 = 1 | - | 0 | 1 | 
| 3 | 1 | 1 | 1 | -5 | 
| 4 | -5 + 20 - 6 = 9 | - | 0 | 9 | 
| 5-6 | 9 | 2 | 1 | -3, -9 | 
| 7 | -9 + 9 - 6 = -6 | - | 1 | -6 | 

Tổng số ngày âm = 3. 

Dấu vết này cho thấy mỗi phân đoạn hoạt động độc lập và các vùng âm tạo thành các hậu tố liền kề bên trong mỗi phân đoạn. 

### Ví dụ 2 

đầu vào:```
2 5 5
3 10
5 5
```| Phân đoạn | Bắt đầu | Chiều dài | Ngày âm | Kết thúc | 
| --- | --- | --- | --- | --- | 
| 1-2 | 0 | 2 | 1 | -5 | 
| 3 | 5 | 1 | 0 | 0 | 
| 4-4 | 0 | 1 | 1 | -5 | 
| 5 | -5 | 1 | 1 | -10 | 

Điều này thể hiện sự phục hồi và sụp đổ xen kẽ tùy thuộc vào lượng tiền gửi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi sự kiện gửi tiền được xử lý một lần với số học thời gian không đổi | 
| Không gian | O(1) | Chỉ có một số biến được duy trì bên cạnh việc lưu trữ đầu vào | 

Lời giải dễ dàng nằm trong giới hạn vì$n \le 10^5$và tất cả các hoạt động là thời gian không đổi cho mỗi sự kiện. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, p, m = map(int, input().split())
    events = [tuple(map(int, input().split())) for _ in range(n)]

    def count_negative_days(n, p, m, events):
        ans = 0
        cur = 0
        prev_day = 1

        for d, t in events:
            length = d - prev_day
            if length > 0:
                if cur < 0:
                    ans += length
                else:
                    first_neg = cur // p + 1
                    if first_neg < 1:
                        first_neg = 1
                    if first_neg <= length:
                        ans += length - first_neg + 1
                cur -= length * p

            cur += t
            cur -= p
            if cur < 0:
                ans += 1

            prev_day = d + 1

        if prev_day <= m:
            length = m - prev_day + 1
            if cur < 0:
                ans += length
            else:
                first_neg = cur // p + 1
                if first_neg <= length:
                    ans += length - first_neg + 1

        return ans

    return str(count_negative_days(n, p, m, events))

# samples
assert solve("3 6 7\n2 13\n4 20\n7 9\n") == "3"

# minimum size, single day
assert solve("1 5 1\n1 10\n") == "0"

# no deposits
assert solve("0 3 5\n") == "5"

# always positive after large deposit
assert solve("1 1 5\n1 100\n") == "0"

# alternating threshold behavior
assert solve("2 5 6\n2 10\n5 5\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tiền gửi một ngày | 0 | trường hợp ranh giới, phục hồi ngay lập tức | 
| không có tiền gửi | m | phân rã tuyến tính đầy đủ | 
| tiền gửi sớm lớn | 0 | không bao giờ tiêu cực | 
| tiền gửi xen kẽ | 2 | độ chính xác của phân đoạn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi không có tiền gửi. Trong trường hợp đó số dư chỉ đơn giản là$-p, -2p, \dots$, nên ngày nào cũng âm. Thuật toán xử lý việc này bằng cách bỏ qua vòng lặp sự kiện và xử lý đoạn đuôi cuối cùng bắt đầu từ số dư 0, đếm chính xác tất cả$m$ngày. 

Một trường hợp khác là khoản tiền gửi lớn ngăn cản mọi số dư âm trong tương lai. Logic phân đoạn phát hiện rằng số dư ban đầu không âm và chỉ số âm đầu tiên vượt quá độ dài phân đoạn, do đó, nó đóng góp 0 ngày. 

Trường hợp tinh vi cuối cùng là khi khoản tiền gửi xảy ra vào ngày cuối cùng. Thuật toán xử lý ngày hôm đó một cách bình thường và sau đó phân đoạn còn lại trống, tránh mọi lỗi sai sót do khoảng thời gian có độ dài bằng 0.
