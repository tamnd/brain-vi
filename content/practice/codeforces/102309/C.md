---
title: "CF 102309C - Thái Từ Khôn và Orz Pandas"
description: "Chúng ta cần chọn vận tốc ban đầu cho một quả bóng rổ được ném từ (P=(x0,y0)) sao cho nó đạt (Q=(x1,y1)) tại một thời điểm nào đó (t), trong khi tốc độ ban đầu không vượt quá (v{max}). Bài toán chính thức sử dụng phương trình vật lý [ B(t)=P+v0t+frac12gt^2, ] trong đó (g=(0,-9.80665))."
date: "2026-08-13T06:42:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102309
codeforces_index: "C"
codeforces_contest_name: "The 2019 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102309
solve_time_s: 663
verified: true
draft: false
---

[CF 102309C - Thái Từ Khôn và Orz Pandas](https://codeforces.com/problemset/problem/102309/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chọn vận tốc ban đầu cho một quả bóng rổ được ném từ (P=(x_0,y_0)) sao cho nó đạt đến (Q=(x_1,y_1)) tại một thời điểm nào đó (t), trong khi tốc độ ban đầu không vượt quá (v_{\max}). 

Bài toán chính thức sử dụng phương trình vật lý 

[ 
B(t)=P+v_0t+\frac12gt^2, 
] 

trong đó (g=(0,-9.80665)). Câu lệnh được sao chép trong lời nhắc bỏ qua hệ số (1/2), nhưng câu lệnh Codeforces chính thức có chứa nó và kết quả đầu ra mẫu nhất quán với phiên bản đó. 

Đối với một trường hợp thử nghiệm, bốn tọa độ mô tả điểm bắt đầu và điểm mục tiêu tính bằng mét, trong khi (v_{\max}) là độ lớn lớn nhất được phép của vận tốc ban đầu tính bằng mét trên giây. Chúng ta phải xuất ra (v_x), (v_y) và thời gian đánh (t). Có thể có nhiều câu trả lời hợp lệ tùy ý nên chúng ta chỉ cần xây dựng một câu trả lời. 

Các tọa độ được giới hạn bởi (200) về giá trị tuyệt đối, do đó độ dịch chuyển theo một trong hai hướng tối đa là (400). Không có mảng hoặc biểu đồ lớn nào bị ẩn trong đầu vào và mỗi trường hợp thử nghiệm có thể được giải bằng cách sử dụng một số phép tính số học không đổi. Thách thức thực sự là tìm ra cách xây dựng đại số thay vì tối ưu hóa một phép tính lớn tiệm cận. Nhiều trường hợp thử nghiệm tiếp tục cho đến EOF, vì vậy giải pháp nên sử dụng công việc không đổi trên mỗi dòng. 

Có hai trường hợp đặc biệt cần được xử lý rõ ràng. Ví dụ: nếu điểm bắt đầu và điểm đích trùng nhau```
0 0 0 0 0
```thì (v_0=(0,0)) và (t=0) là câu trả lời hợp lệ. Một công thức chia cho khoảng cách giữa các điểm sẽ chia cho 0. Việc bài toán đảm bảo có nghiệm cũng có nghĩa là (t=0) phải được chấp nhận trong trường hợp này, vì với (v_{\max}=0) không có nghiệm nào theo thời gian dương quay về cùng một điểm. 

Trường hợp đặc biệt thứ hai là một cú ném thẳng đứng xuống, chẳng hạn```
0 10 0 0 0
```Một câu trả lời hợp lệ là```
0 0 1.428...
```bởi vì trọng lực một mình làm quả bóng đi xuống (10) mét. Việc thực hiện bất cẩn có thể đòi hỏi vận tốc ban đầu dương theo phương thẳng đứng hoặc chia cho chuyển vị ngang, cả hai điều này đều không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận số trực tiếp sẽ coi thời gian bay (t) là ẩn số. Đối với mọi ứng cử viên (t), vận tốc cần thiết được xác định duy nhất: 

[ 
v_x=\frac{dx}{t}, 
\qquad 
v_y=\frac{dy}{t}+\frac{g_0t}{2}, 
] 

trong đó (dx=x_1-x_0), (dy=y_1-y_0) và (g_0=9.80665). Chúng ta có thể quét các giá trị có thể có của (t), tính tốc độ tương ứng và tìm giá trị không vượt quá (v_{\max}). Phương pháp này đúng về mặt khái niệm vì mỗi thời điểm dương sẽ xác định chính xác một vận tốc đạt tới mục tiêu. 

Vấn đề là độ chính xác. Nếu chúng tôi quét khoảng từ (0) đến (100) với bước (10^{-8}), thì chúng tôi đã cần đánh giá (10^{10}) cho một trường hợp thử nghiệm. Một bước thô hơn nhiều có thể bỏ lỡ khoảng thời gian hợp lệ hẹp hoặc tạo ra quỹ đạo có sai số vị trí vượt quá (10^{-6}). Quét số đang giải một bài toán mà đại số cho phép chúng ta giải một cách chính xác. 

Quan sát quan trọng là sau khi biểu thị vận tốc theo (t), độ lớn bình phương của nó có dạng rất đơn giản. hãy để 

[ 
r=\sqrt{dx^2+dy^2} 
] 

là khoảng cách giữa hai điểm và gọi 

[ 
k=\frac{9.80665}{2}=4.903325. 
] 

Sau đó 

[ 
v_x=\frac{dx}{t}, 
\qquad 
v_y=\frac{dy}{t}+kt. 
] 

Bình phương và cộng sẽ cho 

\frac{r^2}{t^2}+2kdy+k^2t^2. 
] 

Phần thay đổi duy nhất là 

[ 
\frac{r^2}{t^2}+k^2t^2. 
] 

Biểu thức này được giảm thiểu khi 

[ 
t^2=\frac{r}{k}=\frac{2r}{9.80665}. 
] 

Vì vậy, thay vì tìm kiếm thời gian khả thi, chúng tôi cố tình chọn thời gian cho tốc độ ban đầu nhỏ nhất có thể. Bài toán đảm bảo rằng tồn tại một số quỹ đạo khả thi, do đó tốc độ tối thiểu có thể cũng lớn nhất là (v_{\max}). Điều này cho chúng ta một quỹ đạo hợp lệ ngay lập tức. 

Tốc độ tối thiểu cũng có thể được viết là 

# 2kr+2kdy 

9.80665(r+dy). 
] 

Vì (r\ge |dy|), biểu thức không bao giờ âm. Chuyển vị ngang không cần xử lý riêng, trừ trường hợp suy biến (r=0). 

Đây là toàn bộ quá trình tối ưu hóa: giảm thiểu tốc độ ban đầu cần thiết trong suốt thời gian bay, sau đó sử dụng thời gian giảm thiểu đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(1/\varepsilon)) cho mỗi trường hợp thử nghiệm | (O(1)) | Quá chậm so với độ chính xác cần thiết | 
| Tối ưu | (O(1)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chuyển vị 

[ 
dx=x_1-x_0,\qquad dy=y_1-y_0 
] 

và chiều dài Euclide của nó 

[ 
r=\sqrt{dx^2+dy^2}. 
] 

Sự dịch chuyển là thông tin hình học duy nhất cần thiết sau khi điểm bắt đầu bị trừ khỏi mục tiêu. 

1. Nếu (r=0), xuất ra (v_x=v_y=t=0). 

Bóng đã bắt đầu ở mục tiêu nên không cần di chuyển. Trường hợp này phải tách ra vì công thức tổng quát chia hết cho (t). 

1. Đặt (k=9,80665/2), tương ứng với độ lớn của gia tốc trọng trường chia cho 2 trong phương trình quỹ đạo. 

Trong thời gian dương đã chọn (t), vận tốc ban đầu cần thiết là 

[ 
v_x=\frac{dx}{t}, 
\qquad 
v_y=\frac{dy}{t}+kt. 
] 

1. Chọn 

[ 
t=\sqrt{\frac{r}{k}}. 
] 

Điều này giảm thiểu tốc độ bình phương. Để hiểu tại sao, phần thay đổi của bình phương tốc độ là 

[ 
\frac{r^2}{t^2}+k^2t^2. 
] 

Số hạng đầu tiên giảm khi (t) tăng lên, trong khi số hạng thứ hai tăng. Sự cân bằng của chúng xảy ra khi 

[ 
\frac{r^2}{t^2}=k^2t^2, 
] 

mang lại (t^2=r/k). 

1. Tính toán 

[ 
v_x=\frac{dx}{t} 
] 

và 

[ 
v_y=\frac{dy}{t}+kt. 
] 

Các giá trị này được xây dựng trực tiếp từ phương trình mục tiêu, do đó thay thế chúng trở lại quỹ đạo đạt tới (Q) vào đúng thời điểm đã chọn. 

1. In ba giá trị có nhiều chữ số. 

Mặc dù tuyên bố mô tả kết quả đầu ra là ba số thập phân, nhưng bản thân mẫu lại in ra nhiều chữ số hơn và giám khảo kiểm tra tính hợp lệ của số thay vì sự bình đẳng chính xác về mặt văn bản. Việc in mười hoặc mười hai chữ số thập phân sẽ giữ cho sai số số ở mức dưới mức dung sai yêu cầu một cách thoải mái.

Tại sao nó hoạt động: với mỗi số dương (t), hai công thức vận tốc là vận tốc ban đầu duy nhất đạt đến (Q) tại thời điểm (t). Trong số tất cả các vận tốc như vậy, vận tốc được chọn (t=\sqrt{r/k}) sẽ giảm thiểu tốc độ. Vì phát biểu đảm bảo rằng ít nhất một quỹ đạo có tốc độ tối đa (v_{\max}), quỹ đạo tốc độ tối thiểu này cũng thỏa mãn giới hạn tốc độ. Trường hợp suy biến (P=Q) được xử lý trực tiếp với quỹ đạo thời gian bằng 0. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

G = 9.80665
K = G / 2.0

def solve():
    out = []

    for line in sys.stdin:
        if not line.strip():
            continue

        x0, y0, x1, y1, vmax = map(float, line.split())

        dx = x1 - x0
        dy = y1 - y0

        r = math.hypot(dx, dy)

        if r == 0.0:
            out.append("0.0000000000 0.0000000000 0.0000000000")
            continue

        t = math.sqrt(r / K)

        vx = dx / t
        vy = dy / t + K * t

        out.append(f"{vx:.12f} {vy:.12f} {t:.12f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần hằng số lưu trữ (9.80665/2) dưới dạng`K`, khớp với số hạng (\frac12gt^2) trong phương trình quỹ đạo chính thức. 

Sự dịch chuyển được tính toán trước bất cứ điều gì liên quan đến thời gian. Điều này giữ cho đạo hàm độc lập với tọa độ tuyệt đối, có thể lớn bằng (200) theo cả hai hướng.`math.hypot(dx, dy)`tính khoảng cách mà không cần viết biểu thức căn bậc hai theo cách thủ công. Với giới hạn nhất định không có vấn đề tràn, nhưng`hypot`cũng là một cách triển khai bằng số rõ ràng của khoảng cách Euclide. 

các`r == 0.0`kiểm tra ở đây an toàn vì tọa độ đầu vào là số nguyên. Do đó (r=0) xảy ra chính xác khi cả hai điểm có tọa độ giống hệt nhau, thay vì xấp xỉ dấu phẩy động nhỏ. 

Đối với mọi chuyển vị khác không,`t = sqrt(r / K)`là thời gian cực tiểu rút ra ở trên. Một lần`t`đã biết, các thành phần vận tốc suy ra trực tiếp từ hai phương trình tọa độ. Không có vòng lặp nào có độ dài phụ thuộc vào các giá trị tọa độ và không có tìm kiếm nhị phân hoặc phép lặp nào có thể tích lũy lỗi số. 

Mã in mười hai chữ số sau dấu thập phân. Điều này chính xác hơn yêu cầu định dạng danh nghĩa và cung cấp cho trình kiểm tra đủ thông tin để đánh giá quỹ đạo có sai số thấp hơn nhiều (10^{-6}). 

## Ví dụ đã hoạt động 

Mẫu được cung cấp là```
0 0 10 0 15
```Ở đây chuyển vị nằm ngang, do đó (dx=10), (dy=0) và (r=10). 

| Biến | Giá trị | 
| --- | --- | 
| (dx) | (10) | 
| (dy) | (0) | 
| (r) | (10) | 
| (k) | (4.903325) | 
| (t=\sqrt{r/k}) | xấp xỉ (1.428) | 
| (v_x=dx/t) | xấp xỉ (7,002) | 
| (v_y=dy/t+kt) | xấp xỉ (7,002) | 

Tốc độ ban đầu thu được là xấp xỉ (9,903), thấp hơn mức cho phép (15). Mẫu chính thức in ra một quỹ đạo hợp lệ khác, xấp xỉ ((6.342,7.732)) tại thời điểm (1.577), vì bài toán chấp nhận bất kỳ quỹ đạo nào thỏa mãn các ràng buộc. 

Đối với ví dụ thứ hai, hãy xem xét```
0 10 0 0 0
```Mục tiêu nằm ngay dưới điểm xuất phát. 

| Biến | Giá trị | 
| --- | --- | 
| (dx) | (0) | 
| (dy) | (-10) | 
| (r) | (10) | 
| (k) | (4.903325) | 
| (t=\sqrt{r/k}) | xấp xỉ (1.428) | 
| (v_x) | (0) | 
| (v_y) | xấp xỉ (0) | 

Chỉ có trọng lực làm quả bóng đi xuống 

[ 
\frac12gt^2=4.903325\cdot\frac{10}{4.903325}=10. 
] 

Do đó, quả bóng đến mục tiêu với vận tốc ban đầu bằng 0, khớp chính xác với ràng buộc (v_{\max}=0). Ví dụ này cũng cho thấy tại sao phương pháp này không cần một công thức đặc biệt cho chuyển động thẳng đứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) cho mỗi trường hợp thử nghiệm | Một số phép tính số học cố định và một căn bậc hai được sử dụng | 
| Không gian | (O(1)) không bao gồm lưu trữ đầu ra | Chỉ cần một số lượng biến dấu phẩy động không đổi | 

Giới hạn tọa độ làm cho mọi đại lượng số học trở nên nhỏ và mỗi trường hợp kiểm thử đều mất thời gian không đổi. Ngay cả một số lượng rất lớn các trường hợp thử nghiệm cũng được xử lý chỉ với một vài thao tác dấu phẩy động trên mỗi dòng, do đó phương pháp này dễ dàng phù hợp với giới hạn một giây và 256 MB. 

## Trường hợp thử nghiệm 

Vì vấn đề được đánh giá đặc biệt nên bộ khai thác kiểm tra không được so sánh văn bản đầu ra với một chuỗi chính xác. Thay vào đó, nó phải xác minh trực tiếp các điều kiện toán học. Điều này cũng xử lý chính xác thực tế rằng đầu ra mẫu chỉ là một trong nhiều câu trả lời hợp lệ.```python
import sys
import io
import math

G = 9.80665
K = G / 2.0

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    try:
        out = []

        for line in sys.stdin:
            if not line.strip():
                continue

            x0, y0, x1, y1, vmax = map(float, line.split())

            dx = x1 - x0
            dy = y1 - y0
            r = math.hypot(dx, dy)

            if r == 0.0:
                out.append("0.0000000000 0.0000000000 0.0000000000")
                continue

            t = math.sqrt(r / K)
            vx = dx / t
            vy = dy / t + K * t

            out.append(f"{vx:.12f} {vy:.12f} {t:.12f}")

        print("\n".join(out))

        return output.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def check_case(inp: str):
    lines = inp.strip().splitlines()
    result = solve_data(inp).strip().splitlines()

    assert len(result) == len(lines)

    for original, produced in zip(lines, result):
        x0, y0, x1, y1, vmax = map(float, original.split())
        vx, vy, t = map(float, produced.split())

        assert t >= 0.0

        speed_sq = vx * vx + vy * vy
        assert vmax * vmax - speed_sq > -1e-6

        bx = x0 + vx * t
        by = y0 + vy * t - K * t * t

        assert abs(bx - x1) <= 1e-6
        assert abs(by - y1) <= 1e-6

# Provided sample.
check_case("0 0 10 0 15")

# Same starting and target point, including vmax = 0.
check_case("0 0 0 0 0")

# Pure vertical drop with zero initial velocity.
check_case("0 10 0 0 0")

# Pure vertical upward displacement.
check_case("0 0 0 10 20")

# Large coordinate differences and a generous speed limit.
check_case("-200 -200 200 200 200")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0 0 0`|`0 0 0`hợp lệ | Điểm trùng khớp và tốc độ cho phép bằng 0 | 
|`0 10 0 0 0`| Khoảng`0 0 1.428`| Chuyển động đi xuống chỉ có trọng lực | 
|`0 0 0 10 20`| Khoảng`0 14.005 1.428`| Chuyển vị hướng lên thuần túy | 
|`-200 -200 200 200 200`| Một quỹ đạo khác 0 hợp lệ | Độ lớn tọa độ và chuyển vị chéo tối đa | 

## Vỏ cạnh 

Đối với các điểm trùng nhau, đầu vào chính xác```
0 0 0 0 0
```cho (r=0). Cấu trúc chung sẽ cố gắng sử dụng (t=\sqrt{r/k}=0) rồi chia cho (t), do đó việc triển khai trả về vectơ 0 và thời gian bằng 0 trước khi thực hiện các phép chia đó. Việc thay thế các giá trị này vào quỹ đạo sẽ cho (B(0)=P=Q) và tốc độ chính xác bằng 0. 

Đối với trường hợp rơi chỉ do trọng lực, hãy xem xét```
0 10 0 0 0
```Ở đây (r=10), vậy 

[ 
t=\sqrt{\frac{10}{4.903325}}\approx1.428. 
] 

Vận tốc theo chiều dọc trở thành 

[ 
v_y=\frac{-10}{t}+4.903325t\approx0. 
] 

Do đó, quả bóng chạm tới điểm cách nó mười mét mà không có vận tốc ban đầu. Một phương pháp giả sử (v_y) phải dương sẽ bác bỏ quỹ đạo hợp lệ này một cách không chính xác. 

Đối với mục tiêu hướng lên,```
0 0 0 10 20
```cùng thời gian là khoảng (1.428), nhưng bây giờ 

[ 
v_y=\frac{10}{t}+4.903325t\approx14.005. 
] 

Tốc độ ban đầu là khoảng (14,005), ở mức dưới (20). Dấu của (dy) xác định dấu và độ lớn của thành phần thẳng đứng một cách tự nhiên, do đó không cần có công thức chuyển động đi lên riêng biệt. 

Cuối cùng, mẫu được cung cấp```
0 0 10 0 15
```có chuyển vị ngang là (10) mét. Quỹ đạo giảm thiểu được tạo ra bởi giải pháp này sử dụng khoảng (v_x=v_y=7,002) và (t=1,428), với tốc độ xấp xỉ (9,903<15). Câu trả lời được công bố của mẫu sử dụng một quỹ đạo hợp lệ khác, đó là lý do tại sao dây thử nghiệm phải xác minh tính chất vật lý thay vì so sánh ba số được in theo từng ký tự.
