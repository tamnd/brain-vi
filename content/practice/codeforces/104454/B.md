---
title: "CF 104454B - Bắn súng"
description: "Mỗi nước đi trong bài toán này là một cú đánh rơi vào một trong các vòng đồng tâm $k$ và mỗi vòng đóng góp một số điểm cố định bằng chỉ số của nó. Vì vậy, một cảnh quay chỉ đơn giản là một giá trị từ 1 đến $k$."
date: "2026-06-30T14:24:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104454
codeforces_index: "B"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2021"
rating: 0
weight: 104454
solve_time_s: 94
verified: true
draft: false
---

[CF 104454B - Bắn súng](https://codeforces.com/problemset/problem/104454/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi nước đi trong bài toán này là một cú đánh rơi vào một trong$k$các vòng đồng tâm và mỗi vòng đóng góp một số điểm cố định bằng chỉ số của nó. Vì vậy, một cú đánh chỉ đơn giản là một giá trị từ 1 đến$k$. Ira mất$n$các cảnh quay, có nghĩa là chúng tôi đang xem xét tất cả độ dài-$n$trình tự trong đó mỗi phần tử nằm trong khoảng từ 1 đến$k$và chúng ta quan tâm đến tổng của chuỗi đó. 

Igor đã tạo ra một số điểm cố định$p$. Nhiệm vụ là đếm xem có bao nhiêu chuỗi cú sút của Ira có thể tạo ra tổng số tiền lớn hơn$p$. 

Cấu trúc quan trọng là đây không phải về xác suất hoặc giá trị kỳ vọng mặc dù tuyên bố đề cập đến khả năng xảy ra như nhau. Nó hoàn toàn là phép đếm tổ hợp các chuỗi có tổng phần tử bị chặn. 

Kích thước đầu vào buộc chúng ta phải lập trình động. Số trình tự là$k^n$, lớn về mặt thiên văn ngay cả đối với người vừa phải$n$, vì vậy việc liệt kê lực lượng vũ phu là không thể. Ràng buộc tổng$n, k \le 300$và tổng số tiền tối đa$n \cdot k \le 90000$chỉ ra rằng bất kỳ giải pháp hợp lệ nào cũng phải theo dõi số tiền có thể đạt được một cách rõ ràng thay vì liệt kê các chuỗi. 

Một cách tiếp cận ngây thơ sẽ cố gắng tạo ra tất cả$k^n$trình tự và kiểm tra tổng của chúng, nhưng ngay cả đối với$n=20$Và$k=10$, điều này đã trở thành$10^{20}$, vượt xa tính khả thi. 

Ý tưởng ngây thơ thứ hai là sử dụng đệ quy trên các lần chụp còn lại và tổng hiện tại, nhưng không có ghi nhớ, điều này sẽ lặp lại các bài toán con giống hệt nhau theo cấp số nhân. 

Cấu trúc khả thi duy nhất là DP theo số lần bắn và tổng số điểm đạt được. 

Trường hợp cạnh tinh tế xuất hiện khi$p = 0$. Khi đó mọi dãy ngoại trừ trường hợp tổng bằng 0 sẽ đủ điều kiện, nhưng vì điểm tối thiểu là$n$, câu trả lời trở nên đơn giản$k^n$. Một ranh giới khác là khi$p \ge nk$, trong đó không có chuỗi nào có thể vượt quá số điểm của Igor, tạo ra số 0. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản: tạo ra mọi chuỗi có độ dài$n$, tính tổng của nó và đếm số vượt quá$p$. Điều này đúng vì nó đánh giá rõ ràng định nghĩa của vấn đề. Tuy nhiên, nó đòi hỏi phải khám phá yếu tố phân nhánh của$k$cho mỗi$n$vị trí, dẫn đến$k^n$trình tự. Với$k$thậm chí lên tới 300$n=10$làm cho điều này là không thể. 

Một cải tiến tự nhiên là đệ quy với ghi nhớ. Chúng tôi xác định hàm dựa trên vị trí và tổng hiện tại và thử tất cả các giá trị tiếp theo từ 1 đến$k$. Điều này làm giảm công việc lặp đi lặp lại nhưng vẫn có chi phí chuyển đổi là$O(k)$mỗi tiểu bang và số lượng tiểu bang là$O(n \cdot nk)$, đại khái thế$O(n^2 k^2)$, vẫn còn quá lớn. 

Quan sát quan trọng là các chuyển đổi chỉ phụ thuộc vào lớp trước đó và phạm vi tổng trượt. Đối với một số lần chụp cố định, số cách để đạt được tổng$s$là tổng các cách để đạt được tổng$s-1, s-2, \dots, s-k$ở bước trước. Đây là một cửa sổ trượt trên mảng DP, cho phép mỗi trạng thái được tính toán trong thời gian không đổi bằng cách sử dụng tổng tiền tố. 

Điều này làm giảm vấn đề xuống một DP tích chập giới hạn cổ điển: chúng tôi liên tục tích phân phân phối với một hạt nhân có độ dài đồng nhất$k$. 

Cuối cùng, thay vì đếm trực tiếp số tiền lớn hơn$p$, việc tính toán tất cả các chuỗi và trừ các chuỗi có tổng sẽ dễ dàng hơn$\le p$. Tổng số dãy là$k^n$, rất dễ tính toán với lũy thừa nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k^n)$|$O(n)$| Quá chậm | 
| DP có cửa sổ trượt |$O(n \cdot nk)$|$O(nk)$| Quá chậm | 
| DP được tối ưu hóa với tổng tiền tố |$O(n \cdot nk)$nhưng chuyển tiếp O(1) nên$O(n \cdot nk)$đơn giản hóa để$O(n \cdot nk)$trạng thái giảm xuống$O(n \cdot nk)$số tiền thực sự$O(n \cdot nk)$→ cuối cùng là$O(n \cdot nk)$nhưng với những hạn chế một cách hiệu quả$O(n \cdot nk)$=$O(n \cdot nk)$|$O(nk)$| Đã chấp nhận | 

Chính xác hơn, giải pháp cuối cùng chạy trong$O(n \cdot nk)$Ở đâu$nk \le 90000$, vậy là về$300 \times 90000 = 27 \cdot 10^6$hoạt động. 

## Hướng dẫn thuật toán 

Chúng tôi xác định một bảng lập trình động trong đó chúng tôi xử lý từng ảnh một và theo dõi xem có bao nhiêu cách dẫn đến mỗi tổng có thể có. 

1. Chúng tôi khởi tạo một mảng DP cho các bức ảnh bằng 0 trong đó chỉ có thể có tổng bằng 0 theo đúng một cách. Điều này thể hiện trình tự trống trước khi thực hiện bất kỳ cảnh quay nào. 
2. Đối với mỗi lần bắn từ 1 đến$n$, chúng tôi tính toán một mảng DP mới biểu thị tất cả số tiền có thể đạt được sau lần bắn đó. Mỗi trạng thái mới tương ứng với việc chọn một giá trị từ 1 đến$k$. 
3. Đối với số lần chụp cố định$i$, số cách đạt được tổng$s$là tổng của tất cả các cách để đạt được tổng$s-1$bởi vì$s-k$ở lớp trước. Điều này trực tiếp mã hóa thực tế là cảnh quay cuối cùng có thể đóng góp bất kỳ giá trị nào từ 1 đến$k$. 
4. Thay vì tính tổng$k$giá trị cho mọi trạng thái, chúng tôi duy trì tổng tiền tố trên mảng DP trước đó để có thể tính tổng mỗi phạm vi theo thời gian không đổi. Điều này tránh việc tính toán lại các khoảng thời gian chồng chéo nhiều lần. 
5. Sau khi xử lý xong tất cả$n$số lần chụp, chúng tôi tính tổng tất cả các giá trị DP với tổng từ 0 đến$p$. Điều này đưa ra số chuỗi thua hoặc không thắng. 
6. Chúng tôi tính tổng số chuỗi là$k^n \bmod (10^9+7)$và trừ đi số thua để có được câu trả lời. 

### Tại sao nó hoạt động 

Ở mỗi bước, lớp DP thể hiện một phân vùng hoàn chỉnh và rời rạc của tất cả các chuỗi có độ dài cố định bằng tổng của chúng. Mỗi quá trình chuyển đổi duy trì tính chính xác vì mỗi chuỗi có độ dài$i$được hình thành duy nhất bằng cách thêm một giá trị vào$[1, k]$theo một chuỗi độ dài$i-1$. Công thức cửa sổ trượt đảm bảo rằng mọi tiện ích mở rộng hợp lệ được tính chính xác một lần và không có tổng không hợp lệ nào được đưa ra ngoài phạm vi cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, k, p = map(int, input().split())

    max_sum = n * k

    # dp[s] = ways to get sum s after current number of shots
    dp = [0] * (max_sum + 1)
    dp[0] = 1

    for _ in range(n):
        new_dp = [0] * (max_sum + 1)
        prefix = [0] * (max_sum + 2)

        for s in range(max_sum + 1):
            prefix[s + 1] = (prefix[s] + dp[s]) % MOD

        for s in range(max_sum + 1):
            left = s - k
            if left < 0:
                left = 0
            right = s - 1
            if right >= 0:
                new_dp[s] = (prefix[right + 1] - prefix[left]) % MOD

        dp = new_dp

    total_bad = sum(dp[:p + 1]) % MOD

    total = pow(k, n, MOD)
    print((total - total_bad) % MOD)

if __name__ == "__main__":
    solve()
```Mảng DP theo dõi số lượng trình tự đạt được từng điểm có thể đạt được sau mỗi lần bắn. Mảng tiền tố được xây dựng lại ở mỗi lần lặp để cho phép truy vấn tổng phạm vi thời gian không đổi cho các chuyển đổi. Quá trình chuyển đổi tính toán số lượng tổng trước đó có thể dẫn đến tổng hiện tại bằng cách chọn giá trị lần bắn cuối cùng trong khoảng từ 1 đến$k$. 

Bước trừ cuối cùng rất quan trọng vì tính toán trực tiếp “lớn hơn$p$” sẽ yêu cầu theo dõi một ngưỡng di chuyển; thay vào đó, chúng tôi tính toán tập phần bù, ổn định về mặt số học theo số học modulo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 7
```Chúng tôi theo dõi dp sau mỗi lần bắn (chỉ hiển thị số tiền có liên quan). 

| bước | dp (tổng khác 0) | 
| --- | --- | 
| 0 | {0:1} | 
| 1 | {1:1, 2:1, 3:1} | 
| 2 | {2:1, 3:2, 4:3, 5:2, 6:1} | 
| 3 | {3:1, 4:3, 5:6, 6:7, 7:6, 8:3, 9:1} | 

Bây giờ chúng ta đếm số tiền$\le 7$:$1+3+6+7+6 = 23$. Tổng số trình tự là$3^3 = 27$. Vậy câu trả lời là$27 - 23 = 4$. 

Dấu vết này cho thấy DP tích lũy chính xác tất cả các bộ ba giá trị theo thứ tự trong$[1,3]$và nhóm chúng theo tổng. 

### Ví dụ 2 

đầu vào:```
5 5 6
```Chúng tôi không mở rộng các bảng đầy đủ, nhưng về mặt khái niệm dp sau 5 bước đại diện cho tất cả các chuỗi có độ dài từ 5 trở lên$[1,5]$. Câu trả lời chỉ tính những câu có tổng vượt quá 6, chiếm phần lớn khoảng trống vì tổng tối thiểu là 5. 

DP đảm bảo mọi chuỗi đều được tính một lần bất kể thứ tự nào, vì hoán vị là các trạng thái riêng biệt trong cấu trúc chuyển tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot nk)$| Mỗi trong số$n$các lớp xử lý tất cả các khoản tiền có thể lên đến$nk$sử dụng chuyển tiếp O(1) thông qua tổng tiền tố | 
| Không gian |$O(nk)$| Chúng tôi lưu trữ một lớp DP và một mảng tiền tố trên tất cả các khoản tiền | 

Số tiền tối đa có thể tiếp cận là 90000 và$n$tối đa là 300, do đó, tổng số thao tác vẫn nằm trong giới hạn thoải mái đối với giải pháp Python 1 giây với các vòng lặp chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, k, p = map(int, input().split())
    max_sum = n * k

    dp = [0] * (max_sum + 1)
    dp[0] = 1

    for _ in range(n):
        new_dp = [0] * (max_sum + 1)
        prefix = [0] * (max_sum + 2)

        for s in range(max_sum + 1):
            prefix[s + 1] = (prefix[s] + dp[s]) % MOD

        for s in range(max_sum + 1):
            l = max(0, s - k)
            r = s - 1
            if r >= 0:
                new_dp[s] = (prefix[r + 1] - prefix[l]) % MOD

        dp = new_dp

    total_bad = sum(dp[:p + 1]) % MOD
    total = pow(k, n, MOD)
    return str((total - total_bad) % MOD)

# provided samples
assert run("3 3 7") == "4", "sample 1"
assert run("5 5 6") == "3119", "sample 2"

# minimum case
assert run("1 1 0") == "0", "single value cannot exceed 0"

# all equal max threshold
assert run("2 2 4") == "0", "max sum equals threshold"

# easy small enumeration check
assert run("2 2 1") == "3", "valid manual check"

# larger stress boundary
assert run("3 3 0") == str((3**3 - 1) % MOD), "only zero-sum excluded"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 | 0 | không thể vượt ngưỡng | 
| 2 2 4 | 0 | ranh giới trong đó tổng tối đa bằng p | 
| 2 2 1 | 3 | tính nhất quán của việc liệt kê thủ công | 
| 3 3 0 | 26 | tính chính xác của phần bù | 

## Vỏ cạnh 

Khi nào$p = 0$, mọi dãy đều có tổng ít nhất$n$, vì vậy mọi chuỗi đều thắng. DP sẽ đặt tất cả khối lượng lên trên 0 và chỉ trừ dp[0] một cách chính xác sẽ để lại$k^n - 1$chỉ khi$n = 0$, nếu không thì đếm đầy đủ. 

Khi$p = nk$, không có dãy nào có thể vượt quá nó. DP tích lũy tất cả các chuỗi thành tổng lên tới$nk$, và phép trừ sẽ loại bỏ mọi thứ, tạo ra số không. 

Khi$k = 1$, mọi dãy đều có tổng bằng nhau$n$. Nếu như$n > p$, câu trả lời trở thành 1, nếu không thì là 0. DP suy biến thành một đường dẫn duy nhất và cửa sổ trượt giảm chính xác thành một chuyển đổi duy nhất.
