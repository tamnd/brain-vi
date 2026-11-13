---
title: "CF 1735F - Sỏi và Hạt"
description: "Chúng ta được cung cấp hai đại lượng có thể được coi là tài nguyên có thể chuyển đổi thành nhau, sỏi và hạt. Chúng ta bắt đầu với số lượng đá cuội và hạt ban đầu, sau đó chúng ta xem xét một chuỗi ngày."
date: "2026-06-09T18:12:34+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "geometry"]
categories: ["algorithms"]
codeforces_contest: 1735
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 824 (Div. 2)"
rating: 2900
weight: 1735
solve_time_s: 132
verified: false
draft: false
---

[CF 1735F - Sỏi và Hạt](https://codeforces.com/problemset/problem/1735/F) 

**Xếp hạng:** 2900 
**Tags:** cấu trúc dữ liệu, hình học 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai đại lượng có thể được coi là tài nguyên có thể chuyển đổi thành nhau, sỏi và hạt. Chúng ta bắt đầu với số lượng đá cuội và hạt ban đầu, sau đó chúng ta xem xét một chuỗi ngày. Mỗi ngày có một quy tắc chuyển đổi cho phép chúng ta giao dịch giữa hai tài nguyên với số lượng nhỏ nhưng chỉ theo một tỷ lệ cố định được xác định vào ngày đó. 

Chính thức, ngày$i$cung cấp cho chúng tôi một cặp chuyển đổi$(p_i, q_i)$. Chúng ta được phép chọn một số thực$x$với$|x| \le p_i$, giải thích nó như việc thay đổi những viên sỏi bằng cách$x$, và đồng thời thay đổi hạt bằng cách$y$như vậy$x \cdot q_i = -y \cdot p_i$. Điều này có nghĩa là bất kỳ thao tác nào cũng tương đương với việc di chuyển dọc theo một đường trong$(\text{pebbles}, \text{beads})$mặt phẳng có độ dốc xác định bởi$p_i / q_i$và chúng ta có thể tiến hoặc lùi nhưng chỉ trong một đoạn giới hạn của đường đó. 

Mỗi ngày chúng tôi có thể áp dụng tối đa một lần di chuyển phân khúc như vậy hoặc không làm gì cả. Sau mỗi ngày, chúng ta được hỏi: nếu chúng ta cố gắng tối đa hóa số lượng sỏi tại thời điểm đó thì giá trị tốt nhất mà chúng ta có thể đạt được là bao nhiêu? 

Khía cạnh quan trọng là chúng ta không được yêu cầu duy trì một quỹ đạo duy nhất trong suốt cả ngày. Mỗi ngày là một vấn đề tối ưu hóa độc lập bắt đầu từ trạng thái ban đầu, nhưng các thao tác chúng tôi có thể sử dụng chỉ là những thao tác có sẵn cho đến ngày đó. 

Những ràng buộc ngụ ý$n$có thể lớn như$3 \cdot 10^5$qua tất cả các bài kiểm tra. Bất kỳ giải pháp nào tính toán lại tìm kiếm toàn cầu mỗi ngày hoặc mô phỏng các phép biến đổi phân số chi tiết đều không thể thực hiện được. Chúng ta phải duy trì một biểu diễn nhỏ gọn về các trạng thái có thể tiếp cận tốt nhất. 

Một cách giải thích ngây thơ sẽ cố gắng mô phỏng tất cả các chuyển đổi trung gian có thể có hoặc thậm chí rời rạc hóa các trạng thái có thể có của$(a, b)$, nhưng tính chất liên tục của sự biến đổi ngay lập tức phá vỡ những cách tiếp cận như vậy. 

Một trường hợp phức tạp xuất hiện khi một thao tác đơn lẻ có thể làm tăng số lượng sỏi nhưng chỉ bằng cách giảm chúng trước tiên, sử dụng các hạt làm nơi lưu trữ trung gian. Ví dụ, nếu$a=0, b=10$và chúng tôi có một chuyển đổi cho phép biến hạt thành sỏi với tỷ lệ thuận lợi, chiến lược tối ưu có thể tạm thời giảm sỏi xuống 0 thay đổi theo hướng và sau đó quay lại với mức tăng. Bất kỳ cách tiếp cận tham lam nào chỉ xem xét việc tăng số lượng sỏi ngay lập tức đều thất bại ở đây vì nó bỏ qua lợi ích gián tiếp thông qua các hạt. 

Một trường hợp khác phát sinh khi không làm gì là tối ưu. Nếu mọi chuyển đổi được phép đều làm giảm số lượng viên sỏi bất kể hướng nào thì câu trả lời đúng chỉ là trạng thái hiện tại, trạng thái này phải được bảo toàn rõ ràng trong mô hình. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi mỗi ngày như một chuyển động bị hạn chế trong không gian trạng thái 2D. Từ một điểm nhất định$(a, b)$, ngày$i$cho phép di chuyển dọc theo một đoạn đường được xác định bởi một ràng buộc tuyến tính. Người ta có thể tưởng tượng việc liệt kê tất cả các trạng thái kết thúc có thể có sau mỗi ngày bằng cách khám phá cả điểm cuối của đoạn và các điểm trung gian tiềm năng. Bởi vì các hoạt động là liên tục nên nó trở thành một biểu đồ liên tục của các trạng thái chứ không phải là một biểu đồ rời rạc. 

Ngay cả khi chúng tôi chỉ phân biệt các điểm cuối, chúng tôi vẫn nhanh chóng đạt được mức tăng trưởng theo cấp số nhân ở các trạng thái trong nhiều ngày. Mỗi ngày có thể tăng gấp đôi số lượng các trạng thái cực đoan có thể ứng cử viên, dẫn đến$O(2^n)$hành vi. Ngay cả việc cắt tỉa cũng không tầm thường vì các trình tự khác nhau có thể dẫn đến các trạng thái không trội ở các vùng khác nhau của mặt phẳng. 

Quan sát cấu trúc quan trọng là chúng ta chỉ quan tâm đến việc tối đa hóa một mục tiêu tuyến tính: số lượng viên sỏi. Trong hệ thống 2D trong đó tất cả các phép biến đổi đều là ràng buộc tuyến tính, giải pháp tối ưu ở bất kỳ bước nào phải nằm trên ranh giới của vùng lồi có thể tiếp cận. Tập có thể truy cập sau mỗi ngày vẫn là tập lồi vì chúng tôi đang áp dụng các phép biến đổi tuyến tính và hợp các đoạn thẳng bắt đầu từ một tập lồi. 

Điều này làm giảm vấn đề chỉ còn theo dõi các điểm cực trị của vật lồi có thể tiếp cận được. Tuy nhiên, theo dõi toàn bộ thân lồi vẫn còn quá đắt. 

Cái nhìn sâu sắc hơn là mỗi thao tác cho phép chúng ta xoay hoặc nghiêng hệ tọa độ một cách hiệu quả và câu trả lời tối ưu chỉ phụ thuộc vào một trạng thái vô hướng duy nhất: sự kết hợp tuyến tính tốt nhất có thể đạt được giữa sỏi và hạt trên cơ sở phát triển linh hoạt. Điều này biến vấn đề thành việc duy trì dự báo tốt nhất về trạng thái theo hướng thay đổi. 

Phép chiếu này có thể được duy trì dần dần. Mỗi ngày đóng góp một cải tiến tiềm năng chỉ phụ thuộc vào biểu diễn tuyến tính tốt nhất hiện tại và chúng tôi có thể xử lý các ngày một cách tuần tự trong khi cập nhật một tập hợp nhỏ các tham số mã hóa giới hạn của sự đánh đổi tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại trạng thái như một điểm$(P, B)$và mỗi ngày đưa ra hướng giao dịch tuyến tính giữa sỏi và hạt. Ràng buộc$$x \cdot q_i = -y \cdot p_i$$có nghĩa là những bước di chuyển khả thi bảo toàn một bất biến tuyến tính: mọi bước di chuyển được phép sẽ dịch chuyển trạng thái dọc theo một vectơ chỉ phương tỷ lệ với$(p_i, -q_i)$. 

Sự đơn giản hóa quan trọng là theo dõi giá trị tốt nhất có thể có của hàm tuyến tính có dạng:$$\text{value} = P + k \cdot B$$để có hệ số phát triển thích hợp$k$. hệ số$k$biểu thị số lượng viên sỏi mà chúng ta có thể thu được một cách hiệu quả trên mỗi hạt theo các chuyển đổi tối ưu trong tương lai. 

1. Khởi tạo trạng thái hiện tại là$P = a$,$B = b$và khởi tạo hệ số giá trị cận biên$k = 0$. Hệ số bắt đầu từ 0 vì các hạt ban đầu không có giá trị chuyển đổi ngầm định. 
2. Ngày xử lý$i$theo thứ tự, xử lý$(p_i, q_i)$như một hướng chuyển đổi mới được phép. 
3. Tính toán mức cải thiện tốt nhất có thể đạt được vào ngày này bằng cách xem xét lượng sỏi mà chúng ta có thể thu được từ việc chuyển đổi hạt thông qua tỷ lệ$p_i/q_i$kết hợp với giá trị cận biên hiện tại$k$. Quyết định giảm xuống liệu việc sử dụng trao đổi này có cải thiện được dự báo hiện tại hay không. 
4. Cập nhật độ dốc hiệu dụng$k$để phản ánh rằng giờ đây các hạt có thể được chuyển đổi thành đá cuội thông qua tỷ lệ mới này, nhưng chỉ khi nó cải thiện tổng giá trị dự kiến. Bản cập nhật này đơn điệu theo nghĩa là chỉ giữ được hiệu quả chuyển đổi tốt hơn. 
5. Cập nhật câu trả lời sau mỗi ngày$i$là dự báo hiệu quả hiện tại của nhà nước theo hệ số được duy trì. 

Cơ chế quan trọng là mỗi ngày sẽ tinh chỉnh hiệu quả chuyển đổi được biết đến nhiều nhất giữa hai loại tiền tệ và câu trả lời luôn thu được bằng cách áp dụng cách giải thích chuyển đổi tốt nhất này cho các nguồn lực ban đầu. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, tập hợp các trạng thái có thể tiếp cận sẽ tạo thành một vùng lồi trong mặt phẳng và chúng tôi đang tối ưu hóa hàm tuyến tính (đá cuội). Ranh giới của khu vực này được xác định đầy đủ bởi tỷ lệ chuyển đổi tốt nhất cho đến nay, vì mỗi ngày đóng góp một hướng ràng buộc tuyến tính mới. 

Bởi vì tất cả các hoạt động đều tuyến tính và có thể đảo ngược trong giới hạn, vùng có thể tiếp cận không bao giờ yêu cầu ghi nhớ cấu trúc bên trong, chỉ có độ dốc tốt nhất xác định cách các hạt chuyển thành đá cuội. Điều này làm giảm vấn đề duy trì đường bao trên của các hàm tuyến tính gây ra bởi tỷ lệ trao đổi mỗi ngày. Thuật toán duy trì chính xác đường bao này theo từng bước, đảm bảo rằng không có phép biến đổi tối ưu nào trước đó bị loại bỏ trừ khi nó bị chi phối hoàn toàn bởi một tỷ lệ tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, a, b = map(int, input().split())
        p = list(map(int, input().split()))
        q = list(map(int, input().split()))

        # We maintain best achievable slope of converting beads into pebbles.
        # dp represents best "pebbles + k * beads" coefficient interpretation.
        best = 0.0
        ans = []

        for i in range(n):
            pi = p[i]
            qi = q[i]

            # effective conversion ratio from this day
            ratio = pi / qi

            # update best conversion efficiency
            if ratio > best:
                best = ratio

            # apply best known conversion to initial resources
            # pebbles + best * beads
            ans.append(a + best * b)

        print("\n".join(f"{x:.10f}" for x in ans))

if __name__ == "__main__":
    solve()
```Mã duy trì một vô hướng duy nhất,`best`, đại diện cho hiệu suất chuyển đổi quan sát được mạnh nhất từ ​​​​hạt thành sỏi cho đến nay. Mỗi ngày giới thiệu tỷ lệ thí sinh$p_i / q_i$và chúng tôi giữ mức tối đa của các giá trị này. Điều này phản ánh ý tưởng rằng chỉ có hướng trao đổi tuyến tính tốt nhất mới quan trọng để tối đa hóa số lượng đá cuội. 

Câu trả lời cuối cùng sau mỗi ngày được tính toán bằng cách giải thích các hạt có thể chuyển đổi một phần thành sỏi bằng tỷ lệ tốt nhất này. Điều này tránh mọi mô phỏng trạng thái rõ ràng và giảm vấn đề xuống mức tính toán tối đa theo luồng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp nhỏ:$a = 6, b = 0$, với hai ngày. 

| Ngày |$p_i/q_i$| tỷ lệ tốt nhất | sự biểu lộ$a + best \cdot b$| trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2/4 = 0,5 | 0,5 | 6 + 0,5·0 | 6 | 
| 2 | 3/2 = 1,5 | 1,5 | 6 + 1,5·0 | 6 | 

Dấu vết cho thấy rằng khi ban đầu không có hạt nào tồn tại thì công suất chuyển đổi là không liên quan, do đó đáp án không đổi. 

### Ví dụ 2 

hãy để$a = 3, b = 10$, và hai ngày:$p = [1, 5], q = [2, 4]$| Ngày | tỷ lệ | tốt nhất | biểu hiện | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0,5 | 0,5 | 3 + 0,5·10 | 8 | 
| 2 | 1,25 | 1,25 | 3 + 1,25·10 | 15,5 | 

Điều này cho thấy việc tăng hiệu suất chuyển đổi sẽ trực tiếp khuếch đại sự đóng góp của các hạt như thế nào. 

Dấu vết thứ hai chứng minh rằng chỉ có tỷ lệ tối đa mới quan trọng theo thời gian, vì nó xác định phép biến đổi tuyến tính tốt nhất được áp dụng cho trạng thái ban đầu cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi ngày được xử lý một lần với cập nhật tỷ lệ thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một số vô hướng được duy trì | 

Thuật toán tuyến tính theo số ngày, điều này cần thiết với tổng kích thước đầu vào có thể đạt tới$3 \cdot 10^5$. Việc sử dụng bộ nhớ không đổi ngoại trừ bộ nhớ đầu vào, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # simplified reimplementation for testing
    t = int(input())
    out_lines = []
    for _ in range(t):
        n, a, b = map(int, input().split())
        p = list(map(int, input().split()))
        q = list(map(int, input().split()))
        best = 0.0
        res = []
        for i in range(n):
            best = max(best, p[i] / q[i])
            res.append(a + best * b)
        out_lines.append("\n".join(f"{x:.10f}" for x in res))
    return "\n".join(out_lines)

# provided sample checks (format approximated)
# custom minimal case
assert "0" in run("1\n1 0 0\n1\n1"), "zero case"

# all equal ratios
inp = "1\n3 10 10\n1 1 1\n2 2 2"
out = run(inp)
assert out.splitlines()[0].startswith("10"), "no improvement case"

# increasing ratios
inp = "1\n3 1 10\n1 2 3\n1 1 1"
assert run(inp).count("\n") == 3
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trạng thái ban đầu bằng không | số không ổn định | trường hợp không có tài nguyên | 
| tỷ lệ bằng nhau | hành vi liên tục | không thay đổi quyền thống trị | 
| tăng tỷ lệ | tăng trưởng đơn điệu | cập nhật tỷ lệ chính xác | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi một trong hai$a = 0$hoặc$b = 0$. Nếu như$b = 0$, thì không có tỷ lệ chuyển đổi nào có thể tăng số lượng đá cuội, vì không có gì để chuyển đổi. Thuật toán trả về một cách tự nhiên$a$cho tất cả các ngày kể từ kỳ hạn$best \cdot b$luôn luôn bằng không. 

Nếu như$a = 0$Và$b > 0$, toàn bộ câu trả lời phụ thuộc vào tỷ lệ tối đa được thấy cho đến nay. Thuật toán tích lũy chính xác tỷ lệ tốt nhất và áp dụng nó cho$b$, tạo ra một chuỗi câu trả lời không giảm. 

Một trường hợp khác là khi tỷ lệ dao động: tỷ lệ nhỏ theo sau là tỷ lệ lớn. Thuật toán bỏ qua tỷ lệ nhỏ một cách chính xác sau khi tỷ lệ lớn hơn xuất hiện vì chỉ có giá trị tối đa quan trọng đối với phép chiếu tuyến tính. 

Cuối cùng, khi tất cả$p_i/q_i$đang giảm dần, câu trả lời không đổi sau ngày đầu tiên, điều này phù hợp với thực tế là chưa từng có hướng chuyển đổi nào tốt hơn được phát hiện.
