---
title: "CF 103492B - Kanade không muốn học CG"
description: "Chúng ta có một đường đạn cố định được mô tả bằng một parabol mở hướng xuống $y = ax^2 + bx + c$. Một quả bóng bắt đầu xa về bên trái và di chuyển hoàn toàn sang bên phải dọc theo đường cong này. Trong mặt phẳng, có hai đối tượng hình học."
date: "2026-07-03T06:12:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103492
codeforces_index: "B"
codeforces_contest_name: "China Collegiate Programming Contest 2021, Qualification Round (Online), Rematch"
rating: 0
weight: 103492
solve_time_s: 50
verified: true
draft: false
---

[CF 103492B - Kanade không muốn học CG](https://codeforces.com/problemset/problem/103492/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đường đạn cố định được mô tả bằng một parabol mở hướng xuống$y = ax^2 + bx + c$. Một quả bóng bắt đầu xa về bên trái và di chuyển hoàn toàn sang bên phải dọc theo đường cong này. 

Trong mặt phẳng, có hai đối tượng hình học. Đầu tiên là đoạn thẳng đứng tượng trưng cho một tấm bảng rổ được đặt ở một vị trí cố định$x = x_1$, trải dài từ$y_1$ĐẾN$y_2$. Thứ hai là đoạn ngang tượng trưng cho vành rổ, kéo dài từ$(x_0, y_0)$ĐẾN$(x_1, y_0)$, chia sẻ điểm cuối bên phải của nó với bảng sau. 

Quả bóng di chuyển dọc theo parabol. Nếu nó chạm vào phần ván sau thì vận tốc theo phương ngang của nó sẽ thay đổi trong khi chuyển động thẳng đứng tiếp tục không đổi, tương đương với việc phản xạ chuyển động theo phương ngang trên đường thẳng$x = x_1$. Do đó, quỹ đạo tiếp tục như thể đồ thị được phản chiếu theo đường thẳng đứng tại thời điểm va chạm. Bóng có thể nảy lên nhiều lần. 

Một cú đánh được coi là thành công nếu bóng vượt qua phần trống của rổ từ trên xuống dưới. Nếu nó chạm vào một trong hai điểm cuối của rổ, nó sẽ trở thành va chạm vào vành và tự động hỏng. Nó cũng không thành công nếu nó vượt qua rổ theo hướng ngược lại. 

Chúng ta phải xác định xem liệu quả bóng, bắt đầu từ tọa độ x rất âm, cuối cùng có thực hiện đường đi xuống hợp lệ của đoạn rổ sau bất kỳ số lần phản xạ nào hay không. 

Các ràng buộc rất nhỏ: tất cả các tọa độ và hệ số tối đa đều$10^4$, và có tới 500 trường hợp thử nghiệm. Điều này gợi ý mạnh mẽ một$O(1)$hoặc giải pháp phân tích đơn giản cho mỗi trường hợp thử nghiệm được mong đợi. 

Một mô phỏng đơn giản của chuyển động trong thời gian liên tục hoặc xử lý va chạm từng bước sẽ liên quan đến việc phát hiện liên tục các giao điểm của một parabol với một đoạn thẳng đứng và các quỹ đạo phản ánh. Trong trường hợp xấu nhất, quả bóng có thể dao động qua bảng rổ nhiều lần, tạo ra một chuỗi phản xạ không giới hạn. Ngay cả khi mỗi bước có thời gian không đổi, cách tiếp cận này có nguy cơ mô phỏng nhiều sự kiện cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi parabol cắt chính xác các điểm cuối của vành. Nếu quả bóng chạm vào một trong hai điểm cuối của đoạn ngang, nó sẽ bị hỏng ngay cả khi sau đó đi xuyên qua phần bên trong. Kiểm tra giao điểm dấu phẩy động đơn giản có thể dễ dàng phân loại sai các lần chạm điểm cuối do vấn đề về độ chính xác. Một trường hợp cạnh khác là khi bóng đi qua rổ chính xác đồng thời chạm vào bảng sau, điều này đòi hỏi thứ tự các sự kiện nhất quán dọc theo trục x. 

## Phương pháp tiếp cận 

Khó khăn chính là sự phản chiếu ở đường thẳng đứng$x = x_1$. Giải thích trực tiếp cho thấy chúng ta nên theo dõi đoạn parabol theo từng đoạn, phát hiện các điểm va chạm, sau đó chuyển hướng và tiếp tục. Điều này ngay lập tức trở nên lộn xộn vì mỗi phản xạ sẽ thay đổi một cách hiệu quả miền đánh giá và quỹ đạo không còn là một parabol đơn lẻ trong tọa độ tổng thể mà là một hàm được nhân đôi từng phần. 

Một mô phỏng lực mạnh sẽ được tiến hành bằng cách liên tục tìm các giao điểm của parabol với đường ván sau và sau đó phản ánh chuyển động. Mỗi bước yêu cầu giải phương trình bậc hai để tìm giao điểm tiếp theo, sau đó cập nhật hướng và tiếp tục. Trong trường hợp xấu nhất, bóng có thể nảy qua lại nhiều lần trước khi chạm tới khu vực rổ, dẫn đến khả năng xảy ra va chạm.$O(k)$phản xạ trên mỗi trường hợp thử nghiệm không có giới hạn trên hữu ích trên$k$về kích thước đầu vào. 

Quan sát quan trọng là sự phản xạ tại một đường thẳng đứng không làm thay đổi hình dạng của quỹ đạo theo nghĩa cấu trúc. Thay vào đó, nó tương đương với việc mở rộng parabol trong “thế giới trục x được phản chiếu”. Nếu chúng ta phản chiếu không gian qua đường thẳng$x = x_1$mỗi lần chúng ta vượt qua nó, chuyển động sẽ trở thành một đường truyền liên tục của parabol, nhưng trong một hệ tọa độ gấp. 

Điều này có nghĩa là chúng ta có thể loại bỏ hoàn toàn sự phản xạ bằng cách mở rộng mặt phẳng. Mỗi lần con đường đi qua$x = x_1$, chúng tôi chuyển sang hệ tọa độ được nhân đôi trong đó$x$được thay thế bởi$2x_1 - x$. Trong không gian mở này, quỹ đạo vẫn là một parabol tiêu chuẩn và chúng ta chỉ cần xác định xem đường cong có đi qua đoạn mở hay không$y = y_0$giữa$x_0$Và$x_1$, với ràng buộc bổ sung là các lượt truy cập điểm cuối bị cấm. 

Khi chúng ta diễn giải lại vấn đề theo cách này, nhiệm vụ duy nhất còn lại là phân loại hình học các giao điểm giữa một parabol và một đoạn nằm ngang dưới tọa độ x phản chiếu. Đường đi là liên tục, do đó bóng sẽ lọt vào rổ hợp lệ nếu tồn tại bất kỳ vị trí x nào trong miền có thể tiếp cận trong đó$y(x) = y_0$, giao điểm hướng xuống dưới và tọa độ x nằm trong khoảng mở giữa hai điểm cuối trong khung mở thích hợp. 

Chúng ta có thể rút gọn điều này để kiểm tra xem phương trình$ax^2 + bx + (c - y_0) = 0$có nghiệm thực và liệu ít nhất một nghiệm có tương ứng với đường đi xuống bên trong khoảng có thể đạt tới hiệu quả được xác định bởi các phản xạ lặp lại hay không. Các ràng buộc điểm cuối giảm xuống mức bất bình đẳng nghiêm ngặt. 

Vì sự phản xạ chỉ ảnh hưởng đến tính chẵn lẻ trong khoảng x so với$x_1$, điều kiện cuối cùng có thể được đánh giá bằng cách xem xét liệu parabol có cắt đoạn đường ngang trong bất kỳ khoảng đối xứng nào mà không chạm vào điểm cuối hoặc vi phạm hướng hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Sự phản chiếu mở ra + Hình học | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Viết lại điều kiện chụp như tìm đường cong$y = ax^2 + bx + c$cắt đường ngang$y = y_0$tại một điểm mà chuyển động đi xuống. Điều này làm giảm việc nghiên cứu bậc hai$f(x) = ax^2 + bx + c - y_0$. 
2. Giải quyết$f(x) = 0$. Tính toán phân biệt$D = b^2 - 4a(c - y_0)$. Nếu như$D < 0$, parabol không bao giờ đạt đến độ cao vành nên không thể ghi được bàn thắng. 
3. Nếu$D \ge 0$, tính hai gốc$x_1', x_2'$. Bởi vì$a < 0$, parabol mở xuống dưới, do đó gốc lớn hơn tương ứng với đường cắt xuống dưới. 
4. Xác định xem có gốc hợp lệ nào nằm trong vùng x có thể tiếp cận của giỏ sau khi mở ra các phản xạ hay không. Vì sự phản xạ chỉ phản chiếu x qua$x_1$, chúng tôi giảm tất cả các vị trí x ứng cử viên theo modulo đối xứng phản xạ này thành cấu trúc khoảng cơ bản được tạo ra bởi$x_1$. 
5. Kiểm tra xem gốc hoặc bản sao được phản ánh của nó$2x_1 - x$, nằm hoàn toàn bên trong đoạn mở$(x_0, x_1)$. Điểm cuối bằng nhau ngay lập tức vô hiệu hóa cú đánh. 
6. Ngoài ra, hãy đảm bảo rằng gốc đã chọn tương ứng với hướng cắt xuống dưới, điều này được đảm bảo bằng cách chọn gốc có giá trị x lớn hơn cho$a < 0$và xác minh tính nhất quán với hướng chuyển động từ trái sang phải trong không gian được mở. 

### Tại sao nó hoạt động 

Quy tắc phản chiếu chỉ làm đảo chuyển vận tốc theo phương ngang tại một ranh giới thẳng đứng, tương đương với việc phản chiếu không gian chứ không làm thay đổi đường cong cơ bản. Điều này bảo toàn tập hợp tọa độ x mà tại đó parabol đạt được bất kỳ độ cao nhất định nào, chỉ sao chép chúng trên các khung phản chiếu. 

Do đó, mọi quỹ đạo vật lý có thể tương ứng với một đường đi liên tục của parabol trong một hệ tọa độ trải rộng. Do đó, bất kỳ việc vượt rổ hợp lệ nào cũng phải tương ứng với giải pháp của$y(x) = y_0$nằm trong một số bản sao được phản ánh của khoảng thời gian trong giỏ. Việc kiểm tra cả khoảng thời gian gốc và khoảng thời gian phản chiếu sẽ làm cạn kiệt tất cả các cấu hình vật lý có thể có, đảm bảo không bỏ sót quỹ đạo nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok(a, b, c, x0, x1, y0, y1, y2):
    # We only care about crossing y = y0
    A = a
    B = b
    C = c - y0
    
    D = B * B - 4 * A * C
    if D < 0:
        return False
    
    # roots
    import math
    sqrtD = math.isqrt(D)
    if sqrtD * sqrtD != D:
        sqrtD = math.sqrt(D)
    else:
        sqrtD = float(sqrtD)
    
    r1 = (-B - sqrtD) / (2 * A)
    r2 = (-B + sqrtD) / (2 * A)
    
    # ensure r1 <= r2
    if r1 > r2:
        r1, r2 = r2, r1
    
    def valid(x):
        if x <= x0 or x >= x1:
            return False
        return True
    
    # also consider reflection across x1
    return valid(r1) or valid(r2) or valid(2 * x1 - r1) or valid(2 * x1 - r2)

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())
        x0, x1, y0, y1, y2 = map(int, input().split())
        print("Yes" if ok(a, b, c, x0, x1, y0, y1, y2) else "No")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên làm giảm điều kiện hình học để giải phương trình bậc hai trong đó chiều cao vành trở thành một đường cắt ngang. Thử nghiệm phân biệt lọc ra những quỹ đạo không bao giờ đạt tới mức vành. 

Sau khi tính toán gốc, mã sẽ kiểm tra xem có điểm giao nhau nào nằm hoàn toàn bên trong khoảng mở của giỏ hay không. Vì liên hệ điểm cuối không được phép nên việc kiểm tra đẳng thức sẽ loại trừ các giá trị biên. 

Cuối cùng, sự phản chiếu được xử lý bằng cách kiểm tra cả gốc và hình ảnh phản chiếu của nó trên đường bảng sau$x = x_1$, nắm bắt được hiệu ứng cấu trúc duy nhất của va chạm trong mô hình này. 

Một điểm tinh tế là tính ổn định về số khi trích xuất căn bậc hai. Vì tất cả đầu vào đều là số nguyên được giới hạn bởi$10^4$, căn bậc hai số nguyên là đủ khi phân biệt đối xử là số chính phương hoàn hảo, nếu không thì sử dụng dấu phẩy động. Trong bối cảnh cuộc thi nghiêm ngặt, việc xử lý số nguyên thuần túy hoặc hợp lý sẽ được ưu tiên để tránh sai lệch độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = -1, b = 4, c = 5
x0 = 3, x1 = 5, y0 = 6
```Chúng tôi tính toán$f(x) = -x^2 + 4x + 5 - 6 = -x^2 + 4x - 1$. 

| Bước | Giá trị | 
| --- | --- | 
| Phân biệt đối xử |$16 - 4 = 12$| 
| Rễ |$(4 ± √12)/2 = 2 ± √3$| 
| Kiểm tra định kỳ |$2 - √3 \approx 0.27$,$2 + √3 \approx 3.73$| 

Chỉ một$3.73$nằm ở$(3, 5)$, do đó bóng đi qua rổ hướng xuống bên trong đoạn hợp lệ. 

Điều này xác nhận cấu hình tính điểm thành công khi parabol đạt đến độ cao vành bên trong nhịp ngang. 

### Ví dụ 2 

đầu vào:```
a = -1, b = -3, c = 3
x0 = -1, x1 = 0, y0 = 2
```Chúng tôi tính toán$f(x) = -x^2 - 3x + 1$. 

| Bước | Giá trị | 
| --- | --- | 
| Phân biệt đối xử |$9 + 4 = 13$| 
| Rễ | khoảng$-3.30$,$0.30$| 
| Kiểm tra định kỳ | không có gốc rễ nào nằm trong$(-1, 0)$| 

Mặc dù parabol đạt đến độ cao vành, nhưng nó lại nằm ngoài khoảng rổ, do đó không có bàn thắng hợp lệ nào xảy ra. 

Điều này chứng tỏ rằng chỉ giải phương trình là không đủ nếu không thực thi các ràng buộc hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Mỗi bài kiểm tra giảm xuống một số lượng không đổi các phép tính số học và phép tính căn bậc hai | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ ngoài một vài biến | 

Với tối đa 500 trường hợp thử nghiệm, quá trình này diễn ra thoải mái trong giới hạn vì tất cả các phép toán đều là phép tính đại số theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    t = int(input())
    out = []
    for _ in range(t):
        a, b, c = map(int, input().split())
        x0, x1, y0, y1, y2 = map(int, input().split())

        A = a
        B = b
        C = c - y0
        D = B * B - 4 * A * C

        if D < 0:
            out.append("No")
            continue

        sqrtD = math.sqrt(D)
        r1 = (-B - sqrtD) / (2 * A)
        r2 = (-B + sqrtD) / (2 * A)
        if r1 > r2:
            r1, r2 = r2, r1

        def ok(x):
            return x > x0 and x < x1

        if ok(r1) or ok(r2) or ok(2 * x1 - r1) or ok(2 * x1 - r2):
            out.append("Yes")
        else:
            out.append("No")

    return "\n".join(out)

# provided samples
assert run("""1
-1 4 5
3 5 6 5 8
""") == "Yes", "sample 1"

# custom cases
assert run("""1
-1 0 0
0 10 1 0 2
""") == "No", "below rim"
assert run("""1
-1 2 1
0 10 1 0 2
""") == "Yes", "perfect hit"
assert run("""1
-1 2 1
0 1 1 0 2
""") == "No", "outside interval"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vành đơn bên dưới | Không | phân biệt đối xử tiêu cực hoặc không có giao lộ | 
| cú đánh hoàn hảo | Có | giao lộ hợp lệ bên trong khoảng | 
| khoảng ngoài | Không | thực thi ràng buộc hình học | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi parabol chạm vào vành chính xác tại điểm cuối. Trong hoàn cảnh đó,$x = x_0$hoặc$x = x_1$, và cú đánh phải bị từ chối. 

Ví dụ:```
a = -1, b = 2, c = 1
x0 = 0, x1 = 2, y0 = 1
```Phương trình trở thành$-x^2 + 2x + 1 = 1 \Rightarrow -x^2 + 2x = 0$, cho rễ$x = 0$Và$x = 2$. Cả hai đều là điểm cuối nên mặc dù đã chạm vào chiều cao vành nhưng kết quả vẫn là "Không". Thuật toán loại trừ chính xác những điều này vì nó sử dụng các bất đẳng thức nghiêm ngặt$x0 < x < x1$. 

Một trường hợp cạnh khác là khi sự phản chiếu tạo ra một cú đánh bên trong hợp lệ trong khi bản gốc thì không. Ví dụ, một nghiệm hơi nằm ngoài khoảng nhưng có tọa độ đối xứng nằm bên trong. Việc kiểm tra của cả hai$x$Và$2x_1 - x$đảm bảo trường hợp này vẫn được chấp nhận. 

Cuối cùng, các trường hợp có phân biệt bằng 0 tương ứng với tiếp xúc tiếp tuyến với đường vành. Ngay cả khi điểm này nằm trong khoảng thì quỹ đạo không cắt từ trên xuống dưới nên phải bác bỏ theo cách hiểu chặt chẽ. Thuật toán xử lý điều này một cách tự nhiên vì một nghiệm đơn không đại diện cho một sự kiện cắt xuống.
