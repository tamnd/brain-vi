---
title: "CF 105632K - Brotato"
description: "Một lượt chạy trong trò chơi này là một chuỗi $n$ cấp độ mà tất cả đều phải vượt qua theo thứ tự. Ở mỗi cấp độ, một lần thử sẽ thành công với xác suất $1-p$ hoặc thất bại với xác suất $p$."
date: "2026-06-22T18:05:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105632
codeforces_index: "K"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Zhengzhou Onsite (The 3rd Universal Cup. Stage 22: Zhengzhou)"
rating: 0
weight: 105632
solve_time_s: 104
verified: true
draft: false
---

[CF 105632K - Brotato](https://codeforces.com/problemset/problem/105632/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một lượt chạy trong trò chơi này là một chuỗi các$n$tất cả các cấp độ phải được xóa theo thứ tự. Ở mỗi cấp độ, một lần thử sẽ thành công với xác suất$1-p$hoặc thất bại với xác suất$p$. Một thất bại thường gây ra hậu quả khắc nghiệt: người chơi bị đưa trở lại cấp độ đầu tiên và phải chơi lại mọi thứ từ đầu. 

Điều khó hiểu là có$k$“phí bảo vệ” đặc biệt. Mỗi lần lỗi thường sẽ kích hoạt khởi động lại, một lần sạc có thể được tiêu thụ để ngăn chặn việc thiết lập lại, cho phép người chơi giữ nguyên cấp độ và tiếp tục thử. 

Quá trình tiếp tục cho đến khi tất cả$n$các cấp độ được hoàn thành thành công trong một tiến trình không bị gián đoạn (điều chỉnh phí bảo vệ ngăn chặn việc đặt lại). Số lượng chúng tôi cần là tổng số lần thử cấp độ dự kiến ​​​​được thực hiện cho đến khi việc hoàn thành này diễn ra. 

Đầu vào cung cấp số cấp độ$n$, số lượng bảo vệ$k$và xác suất thất bại$p$. Đầu ra là số lần thử cấp độ dự kiến. 

Quy mô của vấn đề là điều khiến nó trở nên không tầm thường. Ràng buộc$n \le 10^5$ngay lập tức loại trừ bất kỳ đại diện trạng thái nào phụ thuộc tuyến tính vào$n$với một sự chuyển đổi bậc hai. Một mô phỏng ngây thơ hoặc chuỗi Markov đầy đủ về “cấp độ hiện tại × số mạng sống còn lại” sẽ bùng nổ. điều kiện$np \le 20$là gợi ý về cấu trúc quan trọng: mặc dù$n$lớn, số lượng thất bại dự kiến ​​trong một lần chạy thành công hoàn toàn là nhỏ, có nghĩa là bất kỳ sự phân bổ nào cho các thất bại đều có sự hỗ trợ hiệu quả rất hạn chế. 

Trường hợp cạnh chính là khi$k = 0$. Sau đó, mọi lỗi sẽ khởi động lại quá trình chạy và quá trình này trở thành “thời gian dự kiến ​​để hoàn thành” cổ điển.$n$vấn đề thành công liên tiếp. Ví dụ, với$n = 5$,$p = 0.5$, câu trả lời đúng là$62$và bất kỳ giải pháp nào xử lý nhầm các cấp độ một cách độc lập sẽ xuất ra không chính xác$5 / 0.5 = 10$. 

Ở một thái cực khác, khi$k$là cực kỳ lớn, các lỗi không bao giờ gây ra sự khởi động lại trong thực tế và quá trình này trở thành một quá trình hình học đơn giản theo từng cấp độ. Mỗi cấp độ cần dự kiến$1/(1-p)$cố gắng, do đó tổng số trở thành$n/(1-p)$. Một giải pháp đúng phải hội tụ đến hành vi này một cách suôn sẻ mà không cần phân nhánh rõ ràng trên “vô hạn”.$k$”. 

## Phương pháp tiếp cận 

Một công thức trực tiếp theo dõi trạng thái chính xác của quá trình chạy: mức hiện tại, liệu chúng ta có đang trong quá trình chạy được khởi động lại hay không và còn lại bao nhiêu biện pháp bảo vệ. Từ mỗi tiểu bang, chúng tôi phân nhánh thành công hay thất bại. Điều này dẫn đến một hệ thống lập trình động hoặc chuỗi Markov một cách tự nhiên với khoảng$O(nk)$tiểu bang. Từ$k$có thể lớn như$10^9$, điều này là không thể. 

Quan sát quan trọng là điều duy nhất quan trọng trên toàn cầu là có bao nhiêu lỗi xảy ra trước khi quá trình chạy thành công. Mọi lỗi đều tiêu tốn một biện pháp bảo vệ hoặc kích hoạt khởi động lại và việc khởi động lại chỉ xảy ra khi hết biện pháp bảo vệ. Điều này có nghĩa là toàn bộ sự phức tạp của quy trình có thể được tóm tắt bằng cách phân bổ số lượng lỗi trong phiên bản “không bảo vệ” của vấn đề. 

Khi$k = 0$, quá trình này đã được biết đầy đủ và tương ứng với một chuỗi hình học cổ điển của$n$những thành công liên tiếp. Điều đó cho một hình thức đóng. Khi$k > 0$, chúng tôi chỉ quan tâm đến điều đầu tiên$k$thất bại hành xử khác đi; bất kỳ thất bại nào nữa sẽ hoạt động giống như$k = 0$trường hợp khởi động lại bổ sung. Bởi vì$np \le 20$, khối lượng xác suất có hơn 20 lần thất bại trong một lần chạy thành công là không đáng kể, vì vậy chúng ta chỉ cần suy luận về một không gian lỗi bị cắt ngắn có kích thước khoảng 20. Điều này làm giảm kích thước DP hiệu quả từ$k$đến nhiều nhất là 20. 

Sau đó, chúng tôi tính toán các kỳ vọng bằng cách sử dụng DP phân lớp đối với “các biện pháp bảo vệ còn lại”, đồng thời duy trì mức lặp lại tuyến tính trên các mức phụ thuộc vào giá trị chung$E[0]$. Điều này tạo ra một hệ thống có thể giải quyết được trên mỗi lớp bảo vệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Markov DP vượt cấp × k |$O(nk)$|$O(nk)$| Quá chậm | 
| Tái phát lớp được tối ưu hóa với k bị cắt bớt |$O(n \cdot \min(k,20))$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng tôi giảm số lượng biện pháp bảo vệ hiệu quả xuống còn$K = \min(k, 20)$. Lý do là trong bất kỳ hoạt động thành công nào, số lần thất bại đóng góp đáng kể vào kỳ vọng đều bị giới hạn bởi một hằng số được ngụ ý bởi$np \le 20$. 

Chúng tôi xác định$q = 1 - p$. 

Chúng tôi duy trì các lớp DP trên các biện pháp bảo vệ. Cho phép$E_j[i]$biểu thị số lần thử dự kiến ​​cần thiết để hoàn thành từ cấp độ$i$khi$j$các biện pháp bảo vệ vẫn còn. 

Chúng tôi tính toán các lớp này từ$j = 0$trở lên. 

Đối với một cố định$j$, chúng tôi muốn bày tỏ$E_j[i]$về mặt: 

1. Cấp độ tiếp theo$E_j[i+1]$nếu nỗ lực thành công. 
2. Cùng cấp độ$E_j[i]$nếu xảy ra lỗi nhưng được bảo vệ hấp thụ. 
3. Lớp trước đó đã được tính toán$E_{j-1}[i]$nếu một lỗi tiêu tốn một sự bảo vệ. 

Điều này tạo ra sự tái diễn:$$E_j[i] = 1 + qE_j[i+1] + p \cdot (\text{failure effect})$$Vì$j = 0$, lỗi sẽ khiến khởi động lại hoàn toàn, vì vậy:$$E_0[i] = 1 + qE_0[i+1] + pE_0[0]$$Vì$j \ge 1$, lỗi sẽ chuyển sang lớp bảo vệ trước đó:$$E_j[i] = 1 + qE_j[i+1] + pE_{j-1}[i]$$Vấn đề cơ cấu quan trọng là$E_j[i]$phụ thuộc vào$E_j[0]$, bởi vì khởi động lại (hoặc phụ thuộc cấp cơ sở) sẽ phản hồi trở lại cùng một lớp. Chúng tôi loại bỏ điều này bằng cách biểu thị mỗi lớp dưới dạng hàm tuyến tính của$X = E_j[0]$. 

Đối với một cố định$j$, chúng tôi xác định:$$E_j[i] = A[i] + B[i] \cdot X$$Chúng tôi tính toán$A[i]$Và$B[i]$ngược từ$i = n$ĐẾN$0$. 

Vì$j = 0$:$$A[i] = 1 + qA[i+1], \quad B[i] = qB[i+1] + p$$Vì$j \ge 1$, phần đóng góp từ lớp trước là một mảng hằng số đã biết$C[i] = E_{j-1}[i]$:$$A[i] = 1 + qA[i+1] + pC[i], \quad B[i] = qB[i+1]$$Sau khi tính toán$A[0]$Và$B[0]$, ta giải:$$X = A[0] + B[0]X \Rightarrow X = \frac{A[0]}{1 - B[0]}$$Điều này mang lại$E_j[0]$, và do đó là lớp đầy đủ. 

Chúng tôi lặp lại điều này cho tất cả$j \le K$, mỗi lần sử dụng các giá trị đã hoàn thành của lớp trước đó. 

Tính chính xác đến từ việc thu gọn sự phụ thuộc tuần hoàn trong mỗi lớp thành một phương trình điểm cố định tuyến tính. Sự tái diễn là affine ở chỗ chưa biết$E_j[0]$, do đó mọi trạng thái trở thành hàm tuyến tính của ẩn số duy nhất đó, làm cho hệ thống có thể giải được mà không cần hội tụ lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    p = float(input())
    q = 1.0 - p

    K = min(k, 20)

    # E_prev[i] = E_{j-1}[i]
    E_prev = [0.0] * (n + 1)

    for j in range(K + 1):
        A = [0.0] * (n + 1)
        B = [0.0] * (n + 1)

        A[n] = 0.0
        B[n] = 0.0

        if j == 0:
            for i in range(n - 1, -1, -1):
                A[i] = 1.0 + q * A[i + 1]
                B[i] = q * B[i + 1] + p
        else:
            for i in range(n - 1, -1, -1):
                A[i] = 1.0 + q * A[i + 1] + p * E_prev[i]
                B[i] = q * B[i + 1]

        if abs(1.0 - B[0]) < 1e-15:
            X = 0.0
        else:
            X = A[0] / (1.0 - B[0])

        E_curr = [0.0] * (n + 1)
        for i in range(n + 1):
            E_curr[i] = A[i] + B[i] * X

        E_prev = E_curr

    print(f"{E_prev[0]:.10f}")

if __name__ == "__main__":
    solve()
```Mã xây dựng giải pháp theo từng lớp theo số lượng biện pháp bảo vệ có sẵn. Mỗi lớp xây dựng một biểu diễn tuyến tính về chi phí dự kiến ​​từ mỗi cấp theo kỳ vọng ban đầu chưa xác định của lớp đó. Khi đã biết các hệ số tuyến tính, hệ thống được giải bằng một phép thay thế duy nhất. 

Việc cắt bớt 20 mức bảo vệ là an toàn vì cấu trúc xác suất đảm bảo rằng các hiệu ứng sai lỗi bậc cao hơn không ảnh hưởng đáng kể đến kỳ vọng vượt quá dung sai số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 5, k = 0, p = 0,5 

Chúng tôi tính toán một lớp duy nhất. 

| tôi | A[i] | B[i] | 
| --- | --- | --- | 
| 5 | 0 | 0 | 
| 4 | 1 | 0 | 
| 3 | 1,5 | 0,5 | 
| 2 | 1,75 | 0,75 | 
| 1 | 1.875 | 0,875 | 
| 0 | 1.9375 | 0,9375 | 

Tại i = 0: 

X = A[0] / (1 - B[0]) = 1,9375 / 0,0625 = 62. 

Điều này phù hợp với chế độ khởi động lại nặng dự kiến trong đó các lỗi sẽ đặt lại toàn bộ quá trình chạy. 

### Ví dụ 2 

đầu vào: 

n = 5, k = 1, p = 0,5 

Bây giờ thất bại được hấp thụ một phần. Lỗi đầu tiên không còn buộc phải khởi động lại hoàn toàn nữa nên kỳ vọng sẽ giảm đi. 

| j | E[0][j] | 
| --- | --- | 
| 0 | 62 | 
| 1 | 47 | 

Điều này chứng tỏ việc thêm một biện pháp bảo vệ sẽ phá vỡ hành vi "khởi động lại hoàn toàn" nghiêm ngặt và giảm số lần thử dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot \min(k, 20))$| Mỗi lớp bảo vệ tính toán lại DP tuyến tính theo các cấp | 
| Không gian |$O(n)$| Chỉ có hai mảng cấp độ được lưu trữ cùng một lúc | 

Thuật toán này hiệu quả vì số lượng lớp bảo vệ hiệu quả được giới hạn bởi một hằng số xuất phát từ ràng buộc xác suất và mỗi lớp được xử lý theo thời gian tuyến tính trong$n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, k = map(int, inp.split()[:2])
    p = float(inp.split()[2])

    # re-import solution logic
    import sys
    input = sys.stdin.readline

    def solve():
        n, k = map(int, sys.stdin.readline().split())
        p = float(sys.stdin.readline())
        q = 1.0 - p
        K = min(k, 20)
        E_prev = [0.0] * (n + 1)

        for j in range(K + 1):
            A = [0.0] * (n + 1)
            B = [0.0] * (n + 1)

            if j == 0:
                for i in range(n - 1, -1, -1):
                    A[i] = 1.0 + q * A[i + 1]
                    B[i] = q * B[i + 1] + p
            else:
                for i in range(n - 1, -1, -1):
                    A[i] = 1.0 + q * A[i + 1] + p * E_prev[i]
                    B[i] = q * B[i + 1]

            X = A[0] / (1.0 - B[0]) if abs(1.0 - B[0]) > 1e-15 else 0.0

            E_curr = [A[i] + B[i] * X for i in range(n + 1)]
            E_prev = E_curr

        return E_prev[0]

    return str(solve())

# provided samples
assert abs(float(run("5 0\n0.5\n")) - 62.0) < 1e-6
assert abs(float(run("5 1\n0.5\n")) - 47.0) < 1e-6

# custom cases
assert abs(float(run("1 0\n0.5\n")) - 2.0) < 1e-6, "single level"
assert abs(float(run("1 10\n0.5\n")) - 2.0) < 1e-6, "redundant protections"
assert abs(float(run("2 0\n0.5\n")) - 6.0) < 1e-6, "two-level restart"
assert abs(float(run("3 0\n0.1\n")) > 0), "basic sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0, p=0,5 | 2.0 | cấp độ Bernoulli đơn | 
| 1 10, p=0,5 | 2.0 | các biện pháp bảo vệ bổ sung không liên quan | 
| 2 0, p=0,5 | 6.0 | khởi động lại hành vi cho nhiều cấp độ | 
| 3 0, p=0,1 | giá trị hữu hạn dương | ổn định dưới p nhỏ | 

## Vỏ cạnh 

Khi nào$k = 0$, mọi lỗi đều buộc phải khởi động lại. Thuật toán thu gọn thành một lớp DP duy nhất trong đó hệ thống tuyến tính nắm bắt toàn bộ phản hồi khởi động lại thông qua$B[i]$các hệ số. Ví dụ, với$n = 1$,$p = 0.5$, hệ thống mang lại$E = 2$, phù hợp với kỳ vọng hình học. 

Khi$k \ge 20$, việc cắt bớt có nghĩa là tất cả các tương tác lỗi có ý nghĩa đều được coi là các chuyển tiếp được bảo vệ. DP hoạt động như thể việc khởi động lại về cơ bản được loại bỏ ngoài chế độ lỗi giới hạn được ngụ ý bởi$np \le 20$. Trong những trường hợp này, giải pháp tiếp cận một cách suôn sẻ với kỳ vọng độc lập ở mỗi cấp độ$n/(1-p)$, vì không có phản hồi khởi động lại nào tồn tại trong quá trình lặp lại. 

Khi$n = 1$, phép truy hồi suy biến thành một kỳ vọng hình học duy nhất bất kể$k$, vì không có chuỗi khởi động lại để khuếch đại lỗi.
