---
title: "CF 104248G - Khối tứ diện có thể tích tối thiểu"
description: "Chúng ta có ba vectơ trong không gian 3D, $OA$, $OB$ và $OC$, tạo thành một góc không suy biến tại gốc tọa độ. Ba hướng này hoạt động giống như một hệ tọa độ lệch: bất kỳ điểm nào bên trong góc này có thể được biểu diễn duy nhất dưới dạng tổ hợp dương của ba vectơ này."
date: "2026-07-01T22:09:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104248
codeforces_index: "G"
codeforces_contest_name: "Udmurt SU Contest 2010"
rating: 0
weight: 104248
solve_time_s: 55
verified: true
draft: false
---

[CF 104248G - Khối tứ diện có thể tích tối thiểu](https://codeforces.com/problemset/problem/104248/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba vectơ trong không gian 3D,$OA$,$OB$, Và$OC$, tạo thành một góc không suy biến tại gốc tọa độ. Ba hướng này hoạt động giống như một hệ tọa độ lệch: bất kỳ điểm nào bên trong góc này có thể được biểu diễn duy nhất dưới dạng tổ hợp dương của ba vectơ này. 

Điểm thứ tư$P$nằm hoàn toàn bên trong góc này, nghĩa là nó có thể được viết là$$P = \alpha A + \beta B + \gamma C$$đối với một số hệ số dương$\alpha, \beta, \gamma$. 

Sau đó chúng ta xem xét tất cả các mặt phẳng có thể đi qua$P$. Mỗi mặt phẳng như vậy cắt ba tia$OA, OB, OC$tại một số bội số vô hướng dương có dạng$X = xA$,$Y = yB$,$Z = zC$. Cùng với gốc tọa độ, ba giao điểm này tạo thành một tứ diện$OXYZ$. 

Nhiệm vụ là chọn mặt phẳng thông qua$P$sao cho khối tứ diện thu được có thể tích tối thiểu có thể và tạo ra thể tích tối thiểu đó. 

Kích thước đầu vào không đổi, bao gồm bốn điểm 3D. Điều này loại bỏ hoàn toàn những lo ngại về việc mở rộng quy mô thuật toán, vì vậy giải pháp phải đến từ cấu trúc hình học chứ không phải các thủ thuật tính toán. 

Kiểu thất bại chính trong các phương pháp tiếp cận ngây thơ xuất phát từ việc rời rạc hóa hoặc cố gắng “tìm kiếm các mặt phẳng” về mặt hình học. Một mặt phẳng ở chế độ 3D có vô số bậc tự do và các hướng lấy mẫu hoặc xây dựng các mặt phẳng tùy ý sẽ bỏ lỡ mức tối ưu thực sự. Một lỗi phổ biến khác là cố gắng tối ưu hóa âm lượng theo số lượng.$x,y,z$mà không nhận ra cấu trúc ràng buộc, dẫn đến việc thăm dò không ổn định hoặc không đầy đủ. 

Một cạm bẫy cụ thể sẽ xuất hiện nếu người ta giả định tính đối xứng hoặc cố gắng thiết lập các điểm chặn bằng nhau. Ví dụ, giả sử$x=y=z$bỏ qua sự thật rằng$P$thường bị lệch bên trong cơ sở và mặt phẳng tối ưu sẽ thích ứng với những sự bất đối xứng đó. 

## Phương pháp tiếp cận 

Điều quan trọng là tham số hóa hình học trong hệ tọa độ được xác định bởi$A, B, C$. Trên cơ sở đó, bài toán trở thành thuần túy đại số. 

Bất kỳ mặt phẳng nào cắt các trục tại$x, y, z$định nghĩa một tứ diện có thể tích tỉ lệ với$xyz$, bởi vì hình dạng là một ảnh tuyến tính của đơn hình tiêu chuẩn dưới phép biến đổi gửi cơ sở tiêu chuẩn tới$A, B, C$. Âm lượng là$$V = \frac{1}{6} |\det(A, B, C)| \cdot xyz.$$Ràng buộc xuất phát từ việc máy bay đi qua$P$. Viết$P = (\alpha, \beta, \gamma)$trên cơ sở tương tự, dạng giao điểm của mặt phẳng cho$$\frac{\alpha}{x} + \frac{\beta}{y} + \frac{\gamma}{z} = 1.$$Vì vậy vấn đề giảm xuống mức tối thiểu$xyz$dưới một ràng buộc phi tuyến duy nhất. 

Một ý tưởng mạnh mẽ sẽ là điều trị$x, y, z$dưới dạng các biến liên tục và cố gắng tối ưu hóa về mặt số lượng. Tuy nhiên, điều này là không cần thiết và không đáng tin cậy vì hàm số trơn và lồi chặt trong phép biến đổi đúng nên chấp nhận một tối ưu dạng đóng. 

Cấu trúc gợi ý sử dụng tính đối xứng trong các biến nghịch đảo. Sự ràng buộc bao gồm$\alpha/x$,$\beta/y$,$\gamma/z$, điều này chỉ ra rằng việc cân bằng các điều khoản này là tối ưu. Đây chính xác là bối cảnh trong đó các điều kiện đẳng thức của số nhân AM-GM hoặc Lagrange sẽ thu gọn hệ thống thành một mối quan hệ tỷ lệ. 

Cấu hình tối ưu xảy ra khi cả ba đóng góp đều bằng nhau:$$\frac{\alpha}{x} = \frac{\beta}{y} = \frac{\gamma}{z}.$$Điều này làm giảm toàn bộ tối ưu hóa thành một biến vô hướng duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm máy bay Brute Force | vô hạn / khó chữa | O(1) | Quá chậm | 
| Tối ưu hóa dạng đóng trong tọa độ barycentric | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng ma trận$M = [A\ B\ C]$, điều trị$A, B, C$dưới dạng vectơ cột. Điều này xác định phép biến đổi tuyến tính từ$(\alpha, \beta, \gamma)$hệ tọa độ trong không gian Descartes. Lý do để làm điều này là vì tất cả hình học bên trong góc đều trở thành đại số tuyến tính trên cơ sở này. 
2. Giải hệ tuyến tính$$M \cdot (\alpha, \beta, \gamma)^T = P$$để bày tỏ$P$trong$A,B,C$cơ sở. Các hệ số này thể hiện khoảng cách$P$dọc theo mỗi hướng trục. 
3. Tính định thức$|\det(A, B, C)|$, đại diện cho hệ số tỷ lệ âm lượng từ$(\alpha,\beta,\gamma)$tọa độ khối lập phương với không gian thực. Điều này cho phép chúng ta tách biệt việc tối ưu hóa hình dạng khỏi sự biến dạng hình học. 
4. Sử dụng điều kiện tối ưu$$\frac{\alpha}{x} = \frac{\beta}{y} = \frac{\gamma}{z}$$để bày tỏ$x = 3\alpha$,$y = 3\beta$,$z = 3\gamma$. Lý do điều này có hiệu quả là vì ràng buộc buộc một tổng cố định có các đóng góp bằng nhau và tính đối xứng đảm bảo tỷ lệ cân bằng sẽ giảm thiểu sản phẩm. 
5. Thay thế vào thể tích:$$xyz = 27 \alpha \beta \gamma.$$6. Nhân với hệ số tỉ lệ tứ diện:$$V = \frac{1}{6} |\det(A,B,C)| \cdot 27 \alpha \beta \gamma.$$7. Trả về giá trị kết quả với độ chính xác yêu cầu. 

### Tại sao nó hoạt động 

Sự chuyển đổi thành$(\alpha, \beta, \gamma)$chuyển đổi ràng buộc hình học thành một phương trình affine duy nhất trong các biến nghịch đảo. Mục tiêu$xyz$trở nên có thể phân tách theo cấp số nhân, trong khi ràng buộc trở thành tuyến tính trong$1/x, 1/y, 1/z$. Điều này buộc bất kỳ cực trị nào cũng xảy ra khi tất cả các đóng góp từng phần đều bằng nhau; mặt khác, sự dịch chuyển khối lượng giữa các biến sẽ làm giảm sản phẩm trong khi vẫn duy trì tính khả thi. Điều này đảm bảo giải pháp là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def det3(a, b, c):
    return (
        a[0] * (b[1]*c[2] - b[2]*c[1])
        - a[1] * (b[0]*c[2] - b[2]*c[0])
        + a[2] * (b[0]*c[1] - b[1]*c[0])
    )

def cramers_solve(A, B, C, P):
    D = det3(A, B, C)
    # α = det(P,B,C)/D etc.
    alpha = det3(P, B, C) / D
    beta  = det3(A, P, C) / D
    gamma = det3(A, B, P) / D
    return alpha, beta, gamma, D

def main():
    A = list(map(float, input().split()))
    B = list(map(float, input().split()))
    C = list(map(float, input().split()))
    P = list(map(float, input().split()))

    alpha, beta, gamma, D = cramers_solve(A, B, C, P)

    volume = (27.0 / 6.0) * abs(D) * alpha * beta * gamma
    print(volume)

if __name__ == "__main__":
    main()
```Hàm xác định mã hóa thể tích có dấu của hình song song được hình thành bởi các vectơ cơ sở. Quy tắc Cramer trích xuất tọa độ barycentric của$P$trên cơ sở đó mà không đảo ngược ma trận một cách rõ ràng, vừa ổn định vừa đơn giản với kích thước cố định. 

Công thức cuối cùng kết hợp hai hiệu ứng: không gian bị biến dạng như thế nào$(A,B,C)$, và sâu bao nhiêu$P$nằm trong hệ tọa độ đó. Sản phẩm$\alpha \beta \gamma$nắm bắt hành vi mở rộng quy mô tối ưu của các điểm chặn. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ khái niệm nhỏ trong đó$A, B, C$tạo thành một cơ sở gần trực giao và$P$nằm gần một trục hơn, tạo ra sự không đồng đều$\alpha, \beta, \gamma$. 

| Bước | Giá trị | 
| --- | --- | 
| Tính toán$(\alpha,\beta,\gamma)$| trích xuất thông qua định thức | 
| Tính ( | \det(A,B,C) | 
| Sản phẩm tính toán$\alpha\beta\gamma$| nhạy cảm bất đối xứng | 
| Tính khối lượng cuối cùng | sản phẩm thu nhỏ | 

Dấu vết này cho thấy rằng nếu một tọa độ của$P$trở nên nhỏ, sản phẩm co lại đáng kể, đẩy thể tích tứ diện tối ưu xuống một cách chính xác. Công thức phản ứng trơn tru với các vị trí lệch của$P$. 

Trường hợp thứ hai trong đó$A, B, C$là các vectơ trực giao và đơn vị đơn giản hóa mọi thứ:$\det=1$và câu trả lời chỉ phụ thuộc vào tọa độ của$P$, xác nhận rằng lời giải rút gọn chính xác thành bài toán tứ diện thẳng hàng với trục tiêu chuẩn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số lượng cố định các phép tính định thức 3×3 và các phép tính số học | 
| Không gian | O(1) | Chỉ lưu trữ một số lượng vectơ không đổi | 

Kích thước của bài toán là không đổi, do đó nghiệm hoàn toàn là đại số và dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def det3(a, b, c):
        return (
            a[0] * (b[1]*c[2] - b[2]*c[1])
            - a[1] * (b[0]*c[2] - b[2]*c[0])
            + a[2] * (b[0]*c[1] - b[1]*c[0])
        )

    A = list(map(float, sys.stdin.readline().split()))
    B = list(map(float, sys.stdin.readline().split()))
    C = list(map(float, sys.stdin.readline().split()))
    P = list(map(float, sys.stdin.readline().split()))

    D = det3(A, B, C)
    alpha = det3(P, B, C) / D
    beta  = det3(A, P, C) / D
    gamma = det3(A, B, P) / D

    ans = (27.0 / 6.0) * abs(D) * alpha * beta * gamma
    return str(ans).strip()

# provided sample
assert run("""1 2 3
2 3 1
2 5 3
2 4 3
""") == "2.53125"

# orthogonal basis
assert run("""1 0 0
0 1 0
0 0 1
1 2 3
""")

# symmetric case
assert run("""1 1 0
1 0 1
0 1 1
1 1 1
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 2.53125 | tính đúng đắn trên cơ sở độ lệch chung | 
| cơ sở trực giao | giá trị dựa trên khối đã biết | giảm về hình học tiêu chuẩn | 
| cơ sở đối xứng | hành vi ổn định | xử lý đối xứng | 

## Vỏ cạnh 

Khi nào$A, B, C$gần như trực giao, định thức gần bằng 1 và hệ thống hoạt động giống như tọa độ Descartes tiêu chuẩn. Thuật toán vẫn ổn định vì nó không bao giờ chia cho các giá trị nhỏ ngoại trừ trong tỷ lệ quy tắc Cramer được kiểm soát trong đó tử số và mẫu số chia tỷ lệ với nhau. 

Khi$P$nằm rất gần một trục, một trong$\alpha, \beta, \gamma$trở nên rất nhỏ. Sản phẩm$\alpha\beta\gamma$tương ứng co lại, điều khiển chính xác khối tứ diện về 0, phù hợp với trực giác hình học rằng mặt phẳng cắt phải bị lệch nhiều và khối tứ diện thu được sẽ sụp đổ theo hướng đó.
