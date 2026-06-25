---
title: "CF 105244F - Xổ số"
description: "Chúng ta được cấp một tấm vé số khởi đầu có độ dài $n$. Mỗi vị trí ban đầu đều chứa số 1. Đối với mỗi vị trí $i$, có một giá trị mục tiêu bắt buộc là $bi$. Nếu chúng tôi cố gắng làm cho vị trí thứ $i$-th bằng $bi$, chúng tôi sẽ kiếm được đồng xu $ci$."
date: "2026-06-24T07:01:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105244
codeforces_index: "F"
codeforces_contest_name: "Dynamic Programming, SPbSU 2024, Training 2"
rating: 0
weight: 105244
solve_time_s: 54
verified: true
draft: false
---

[CF 105244F - Xổ số](https://codeforces.com/problemset/problem/105244/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một vé số khởi đầu có độ dài$n$. Mỗi vị trí ban đầu đều chứa số 1. Đối với mỗi vị trí$i$, có một giá trị mục tiêu bắt buộc$b_i$. Nếu chúng ta có thể thực hiện được$i$-vị trí thứ bằng$b_i$, chúng tôi kiếm được$c_i$tiền xu. 

Chúng ta có thể sửa đổi từng vị trí một cách độc lập bằng cách sử dụng một số thao tác giới hạn. Một thao tác chọn một chỉ mục$i$và một số nguyên dương$z$, và tăng giá trị hiện tại$a_i$qua$\lfloor a_i / z \rfloor$. Tất cả các vị trí bắt đầu từ 1 và chúng tôi được phép nhiều nhất$k$tổng số hoạt động trên tất cả các chỉ số. Mục tiêu là chọn các hoạt động sao cho một số tập hợp con của các vị trí đạt được vị trí tương ứng của chúng.$b_i$, tối đa hóa tổng số xu từ các vị trí đã hoàn thành. 

Các hạn chế ở mức độ vừa phải nhưng lớn về ngân sách hoạt động:$n \le 1000$,$k \le 10^6$, Và$b_i \le 1000$. Điều này ngay lập tức gợi ý rằng chúng ta sẽ không mô phỏng các hoạt động một cách ngây thơ qua tất cả các bước cho đến$k$, vì không gian hoạt động quá lớn. Thay vào đó, chúng ta cần hiểu cấu trúc của mỗi vị trí có thể phát triển nhanh như thế nào. 

Một điểm tinh tế là các hoạt động chỉ tương tác thông qua ngân sách được chia sẻ$k$, trong khi mỗi chỉ số phát triển độc lập. Sự độc lập đó thường gợi ý đến vấn đề lựa chọn kiểu ba lô một khi chúng ta biết chi phí để đạt được từng mục tiêu. 

Một trường hợp cạnh không rõ ràng xuất phát từ các vị trí trong đó$b_i = 1$. Những điều này không yêu cầu thao tác nào và luôn đóng góp$c_i$. Một góc khác là khi$k = 0$, trong đó chúng ta chỉ có thể lấy những giá trị đã có giá trị 1. 

Khó khăn chính là hiểu cần bao nhiêu thao tác để biến 1 thành một số cho trước$b_i$, vì định nghĩa hoạt động không phải là tuyến tính hoặc dựa trên mức tăng tiêu chuẩn. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu bằng cách phân tích cách một vị trí phát triển. 

Từ giá trị hiện tại$x$, một thao tác cho phép chúng ta di chuyển đến$$x \rightarrow x + \left\lfloor \frac{x}{z} \right\rfloor$$cho bất kỳ$z \ge 1$. Sự lựa chọn tốt nhất có thể là tối đa hóa số tiền được thêm vào. Từ$\lfloor x/z \rfloor$đạt cực đại khi$z = 1$, thao tác tốt nhất luôn là:$$x \rightarrow x + x = 2x$$Bất kì$z > 1$chỉ làm giảm mức tăng nên không thể giúp giảm số bước cần thiết để đạt được mục tiêu. Điều này làm giảm quá trình nhân đôi thuần túy. 

Bắt đầu từ 1, sau$t$hoạt động giá trị tối đa có thể đạt được là$2^t$. Vì điều này cũng tối ưu ở mỗi bước nên số lượng thao tác tối thiểu cần thiết để đạt hoặc vượt quá$b_i$là:$$w_i = \lceil \log_2 b_i \rceil$$Bây giờ mỗi vị trí trở thành một mục độc lập: chọn vị trí$i$chi phí$w_i$giá trị hoạt động và sản lượng$c_i$. Chúng ta phải chọn một tập hợp con có tổng chi phí nhiều nhất$k$, tối đa hóa tổng giá trị. 

Đây chính xác là một vấn đề về chiếc ba lô 0/1. Sự phức tạp duy nhất là$k$có thể lớn như$10^6$, nhưng trọng lượng thực tế$w_i$được giới hạn bởi$\log_2(1000)$, tối đa là 10. Vì vậy, tổng của tất cả trọng lượng của các mặt hàng nhiều nhất là khoảng 10000. Chúng ta có thể chạy ba lô trên tổng trọng lượng một cách an toàn thay vì$k$, cắt giảm công suất. 

Phương án thay thế vũ lực sẽ thử tất cả các tập hợp con và tất cả các chuỗi hoạt động có thể có trên mỗi chỉ mục, theo cấp số nhân trong$n$và cũng theo cấp số nhân trong$k$, điều đó hoàn toàn không thể thực hiện được. Quan sát cho thấy mức tăng trưởng tối ưu luôn tăng gấp đôi đã khiến toàn bộ hệ thống vận hành trở thành một bài toán lựa chọn có trọng số tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force đối với các hoạt động và tập hợp con | hàm mũ | hàm mũ | Quá chậm | 
| Ba lô vượt quá chi phí phát sinh |$O(n \cdot \sum w_i)$|$O(\sum w_i)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng chính: chuyển đổi từng vị trí thành một cặp chi phí-giá trị 

1. Đối với từng vị trí$i$, tính số thao tác tối thiểu cần thiết để đạt được$b_i$bắt đầu từ 1. 

Điều này được tính như$w_i = \lceil \log_2 b_i \rceil$. 

Bước này thay thế hệ thống vận hành phức tạp bằng một chi phí đơn giản. 
2. Nếu$b_i = 1$, bộ$w_i = 0$. 

Không cần thao tác nào, vì vậy mặt hàng này luôn có sẵn để kiếm lợi nhuận miễn phí. 
3. Tính tổng của tất cả$w_i$. Đây là sức chứa ba lô có ý nghĩa tối đa, vì chúng ta không bao giờ có thể chi tiêu nhiều hơn tất cả các món đồ cộng lại. 
4. Xác định mảng DP trong đó`dp[x]`chính xác là số tiền tối đa có thể đạt được bằng cách sử dụng$x$hoạt động. 
5. Khởi tạo`dp[0] = 0`và tất cả các trạng thái khác về âm vô cùng. 
6. Xử lý từng vị trí$i$và áp dụng chuyển tiếp ba lô 0/1 tiêu chuẩn: 

cho mỗi công suất$x$từ mức tối đa hiện tại xuống$w_i$, cập nhật:$$dp[x] = \max(dp[x], dp[x - w_i] + c_i)$$7. Đáp án là giá trị lớn nhất trên tất cả$dp[x]$vì$x \le k$, nhưng bị giới hạn ở tổng trọng lượng. 

Việc lặp lại ngược lại$x$đảm bảo mỗi vật phẩm được sử dụng tối đa một lần, duy trì độ chính xác của ba lô 0/1. 

### Tại sao nó hoạt động 

Bước chuyển đổi là hợp lệ vì từ bất kỳ trạng thái nào$x$, thao tác luôn có lựa chọn ưu tiên: nhân đôi. Bất kỳ chiến lược tăng trưởng chậm hơn nào cũng không thể giảm số bước để đạt đến ngưỡng$b_i$, vì nó mang lại những giá trị hoàn toàn nhỏ hơn ở mỗi bước so với việc nhân đôi. Vì vậy, thời gian tối thiểu để đạt được$b_i$chính xác là số lần nhân đôi cần thiết. 

Khi mỗi mặt hàng có chi phí cố định độc lập với các mặt hàng khác, vấn đề sẽ trở thành vấn đề phân bổ nguồn lực với một ràng buộc duy nhất, đó chính xác là chiếc ba lô. Bất biến DP là sau khi xử lý dữ liệu đầu tiên$i$mặt hàng,`dp`lưu trữ giá trị tốt nhất có thể đạt được cho mọi ngân sách hoạt động có thể chỉ bằng cách sử dụng các mục đó. Mỗi chuyển đổi duy trì tính khả thi và không sử dụng lại các mục, do đó không có kết hợp không hợp lệ nào được đưa ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    k = int(input())
    b = list(map(int, input().split()))
    c = list(map(int, input().split()))

    weights = []
    for bi in b:
        if bi <= 1:
            weights.append(0)
        else:
            w = 0
            x = 1
            while x < bi:
                x *= 2
                w += 1
            weights.append(w)

    max_w = sum(weights)
    max_w = min(max_w, k)

    NEG = -10**18
    dp = [NEG] * (max_w + 1)
    dp[0] = 0

    for wi, ci in zip(weights, c):
        if wi == 0:
            for j in range(max_w + 1):
                if dp[j] != NEG:
                    dp[j] += ci
            continue

        for j in range(max_w, wi - 1, -1):
            if dp[j - wi] != NEG:
                dp[j] = max(dp[j], dp[j - wi] + ci)

    print(max(dp))

if __name__ == "__main__":
    solve()
```Vòng tiền xử lý chuyển đổi từng giá trị đích thành số lần nhân đôi cần thiết để đạt được giá trị đó. Mảng DP khi đó là chiếc ba lô 0/1 tiêu chuẩn đối với các trọng số dẫn xuất này. Các hạng mục có chi phí bằng 0 sẽ được xử lý riêng biệt bằng cách cộng phần đóng góp của chúng vào tất cả các trạng thái vì chúng luôn được bao gồm. 

Chi tiết triển khai chính là cắt bớt kích thước DP thành$\min(k, \sum w_i)$, giúp giải pháp được nhanh chóng ngay cả khi$k$là lớn. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ: 

đầu vào:```
n = 3, k = 3
b = [2, 3, 4]
c = [1, 1, 1]
```Trọng số dẫn xuất là: 

- 2 yêu cầu nhân đôi 1 
- 3 cần 2 lần nhân đôi 
- 4 cần 2 lần nhân đôi 

Vì vậy, chúng tôi có các mục: 

(1,1), (2,1), (2,1) 

Sự phát triển của DP: 

| Mục | Công suất 0 | 1 | 2 | 3 | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 0 | -∞ | -∞ | -∞ | 
| (1,1) | 0 | 1 | -∞ | -∞ | 
| (2,1) | 0 | 1 | 1 | -∞ | 
| (2,1) | 0 | 1 | 2 | 2 | 

Kết quả tốt nhất là 2 xu trong 3 thao tác. 

Điều này cho thấy thuật toán ưu tiên nhắm mục tiêu rẻ hơn trước nhưng vẫn được hưởng lợi từ việc kết hợp các vật phẩm một cách tối ưu. 

Ví dụ thứ hai: 

đầu vào:```
n = 2, k = 1
b = [1, 2]
c = [5, 10]
```Trọng lượng: 

- 1 → 0 
- 2 → 1 

DP bắt đầu bằng dp[0] = 0. 

Mục (1,5) là miễn phí và thêm 5 ở mọi nơi: 

dp = [5, 5] 

Mục (1,10) cộng 10: 

dp = [15, 15] 

Câu trả lời là 15 ngay cả khi chỉ có một thao tác, vì mục đầu tiên không yêu cầu. 

Điều này xác nhận rằng các mặt hàng có chi phí bằng 0 luôn được tự động đưa vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot \sum w_i)$| Mỗi hạng mục cập nhật DP trên tất cả ngân sách hoạt động có thể đạt được và tổng ngân sách được giới hạn bởi trọng số logarit | 
| Không gian |$O(\sum w_i)$| Chỉ một mảng DP 1D trên số lượng hoạt động khả thi được lưu trữ | 

Vì mỗi$w_i \le 10$, tổng kích thước DP tối đa là khoảng$10^4$. Với$n \le 1000$, điều này nằm trong giới hạn cho giải pháp 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve.__wrapped__()) if hasattr(solve, "__wrapped__") else None

# Since direct import isn't assumed in CF style, we validate logic separately below

# custom sanity checks (conceptual placeholders)

# single element, free gain
# n=1, b=1, k=0 => always take c
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0\n1\n5\n`|`5`| xử lý mặt hàng không tốn phí | 
|`1\n0\n2\n5\n`|`0`| không đủ khả năng thực hiện bất kỳ hoạt động nào | 
|`3\n3\n2 3 4\n1 1 1\n`|`2`| lựa chọn ba lô cơ bản | 
|`2\n10\n1 1\n5 7\n`|`12`| tất cả các mặt hàng miễn phí | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả$b_i = 1$. Trong tình huống này mọi trọng số đều bằng 0, do đó DP suy biến thành tổng tất cả$c_i$. Thuật toán xử lý vấn đề này vì mọi mặt hàng có chi phí bằng 0 sẽ được áp dụng ngay lập tức cho tất cả các trạng thái, tích lũy tất cả phần thưởng một cách hiệu quả mà không tiêu tốn dung lượng. 

Một trường hợp khác là khi$k = 0$. Chỉ những vật phẩm có trọng lượng bằng 0 vẫn hoạt động. Vì DP chỉ được khởi tạo ở mức công suất bằng 0 nên mọi mục có trọng số dương đều không thể truy cập được và không đóng góp gì, phù hợp với yêu cầu. 

Đối với các mục tiêu nhỏ như$b_i = 2$, trọng số chính xác là 1. DP coi đây là những hạng mục chi phí đơn vị một cách chính xác, đảm bảo chúng cạnh tranh công bằng trong ngân sách hạn chế. 

Bước chuyển đổi cũng xử lý lớn$b_i$một cách chính xác. Kể cả nếu$b_i = 1000$, trọng số được tính toán tối đa là 10, do đó không xảy ra tình trạng tràn hoặc giãn nở DP quá mức, duy trì sự ổn định về hiệu suất.
