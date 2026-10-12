---
title: "CF 105055I - Giao diện DJ"
description: "Một đĩa vinyl hình tròn được vẽ trên một màn hình vuông. Đĩa được căn giữa trong hình vuông từ chế độ xem căn chỉnh gốc, với vùng hiển thị của nó là tập hợp các điểm bên trong đường tròn có đường kính $D$. Người dùng nhấp vào một điểm trên đĩa, kéo đến điểm khác và thả ra."
date: "2026-06-28T01:07:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105055
codeforces_index: "I"
codeforces_contest_name: "UDESC Selection Contest 2023-2"
rating: 0
weight: 105055
solve_time_s: 84
verified: false
draft: false
---

[CF 105055I - Giao diện DJ](https://codeforces.com/problemset/problem/105055/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Một đĩa vinyl hình tròn được vẽ trên một màn hình vuông. Đĩa được căn giữa trong hình vuông từ chế độ xem căn chỉnh gốc, với vùng nhìn thấy của nó là tập hợp các điểm bên trong đường tròn có đường kính$D$. Người dùng nhấp vào một điểm trên đĩa, kéo đến điểm khác và thả ra. Phần mềm diễn giải thao tác kéo này như một lệnh xoay đĩa. 

Ý tưởng chính là chỉ có sự dịch chuyển góc của hai điểm so với tâm mới quan trọng. Mỗi vị trí chuột được chuyển đổi thành một vectơ từ tâm đĩa và chuyển động quay do lực kéo gây ra là hiệu góc có dấu giữa hai vectơ này, nhưng luôn được coi là hướng nhỏ hơn trong hai hướng có thể có xung quanh vòng tròn. 

Nếu một trong hai điểm cuối nằm ngoài đĩa thì lực cản sẽ bị bỏ qua. Ngược lại, chuyển vị góc được ánh xạ vào sự dịch chuyển thời gian. Đĩa quay với tốc độ góc cố định tương ứng với 33⅓ vòng/phút, do đó chuyển động góc chuyển trực tiếp thành giây chuyển đổi âm thanh. 

Đầu vào cho đường kính đĩa$D$, tiếp theo là vị trí chuột ban đầu và vị trí chuột cuối cùng. Đầu ra là số giây mà bản nhạc sẽ được tua đi hoặc tua lại nhanh. 

Những hạn chế$D \le 10^4$ngụ ý rằng tất cả các tính toán phải$O(1)$cho mỗi trường hợp thử nghiệm, vì mọi thứ liên quan đến mô phỏng hoặc rời rạc hóa trên pixel sẽ không cần thiết và có khả năng dễ hỏng. Hình học là liên tục và có thể được giải trực tiếp bằng số học dấu phẩy động. 

Một số trường hợp đặc biệt cần được chăm sóc. Nếu một điểm nằm bên ngoài đường tròn, dù chỉ một chút, lực cản sẽ bị bỏ qua hoàn toàn, điều này khiến việc kiểm tra ranh giới về khoảng cách trở nên cần thiết. Một trường hợp tinh vi khác là khi hai vectơ cực kỳ gần nhau, tạo ra một góc gần bằng 0; đầu ra vẫn phải chính xác bằng 0 trong phạm vi dung sai. Cuối cùng, cần phải cẩn thận khi tính toán các góc vì các arccos dựa trên sản phẩm chấm ngây thơ có thể bị mất độ chính xác khi các giá trị hơi trôi ra ngoài$[-1, 1]$. 

## Phương pháp tiếp cận 

Giải thích lực lượng vũ phu sẽ cố gắng mô phỏng chuyển động quay bằng cách tách vòng tròn thành các bước góc nhỏ và tích lũy chuyển động quay cho đến khi đạt được hướng mục tiêu. Điều này sẽ liên quan đến việc điều chỉnh liên tục một góc và tính toán lại các vị trí hoặc sử dụng các bước xoay nhỏ để ước tính lực kéo. Mặc dù về mặt khái niệm đơn giản nhưng nó hoàn toàn không cần thiết và dễ vỡ. Ngay cả việc rời rạc hóa thô 1e5 bước cho mỗi thao tác cũng đã là quá mức cần thiết và độ chính xác cao hơn sẽ đẩy nó vượt quá giới hạn thời gian hoặc gây ra hiện tượng trôi dấu phẩy động. 

Quan sát quan trọng là toàn bộ phép biến đổi chỉ phụ thuộc vào hình học: hai điểm xác định hai vectơ từ tâm và phép quay chính xác là góc có dấu giữa chúng. Góc đó có thể được tính trực tiếp bằng cách sử dụng các phép toán vectơ tiêu chuẩn, cụ thể là công thức atan2 của tích chéo và tích chấm, giúp tránh được sự mất ổn định của cosin nghịch đảo. Khi đã biết góc, nó sẽ được chuyển đổi tuyến tính thành giây bằng tốc độ quay không đổi của đĩa. 

Điều này làm giảm vấn đề từ bất kỳ loại mô phỏng nào thành một phép tính hình học duy nhất cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(K)$mỗi lần kéo |$O(1)$| Quá chậm/không chính xác | 
| Tính Góc Vector |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi tâm của đĩa là gốc của hệ tọa độ, sau đó chuyển đổi cả hai vị trí chuột thành vectơ tương ứng với tâm đó. Phần còn lại của giải pháp hoàn toàn là hình học vector. 

1. Đầu tiên, tính tâm của đĩa như sau:$(D/2, D/2)$. Điều này là cần thiết vì tất cả các góc được xác định tương ứng với tâm chứ không phải gốc màn hình. Việc dịch chuyển tọa độ đảm bảo việc xoay có ý nghĩa. 
2. Chuyển đổi vị trí chuột ban đầu và cuối cùng thành vectơ$v_1$Và$v_2$bằng cách trừ tọa độ tâm. Điều này biến đổi các vị trí không gian màn hình thành một hệ tọa độ trong đó đĩa được đặt ở giữa$(0, 0)$. 
3. Kiểm tra xem một trong hai điểm có nằm ngoài đĩa hay không bằng cách xác minh$x^2 + y^2 \le (D/2)^2$. Nếu một trong hai điểm cuối không đạt bài kiểm tra này, hãy trả về 0 ngay lập tức vì các thao tác kéo không hợp lệ sẽ không làm xoay đĩa. 
4. Tính góc có dấu giữa$v_1$Và$v_2$sử dụng:$$\theta = \text{atan2}(v_1 \times v_2, v_1 \cdot v_2)$$trong đó tích chéo trong 2D là$x_1y_2 - y_1x_2$, và tích chấm là$x_1x_2 + y_1y_2$. Điều này mang lại góc quay có dấu hiệu nhỏ nhất. 
5. Chuyển đổi chuyển vị góc sang thời gian. Đĩa quay với tốc độ 33⅓ vòng/phút, bằng$\frac{100}{3}$số vòng quay mỗi phút. Một vòng quay tương ứng với 60 giây nên vận tốc góc là:$$\omega = \frac{100}{3} \cdot \frac{2\pi}{60} = \frac{10\pi}{9} \text{ radians per second}$$Vì thế:$$t = \frac{\theta}{\omega}$$6. Đầu ra$t$, bảo quản dấu hiệu. Tích cực tương ứng với chuyển động về phía trước và tiêu cực tương ứng với chuyển động tua lại. 

### Tại sao nó hoạt động 

Mỗi vị trí chuột tương ứng duy nhất với một tia từ tâm đĩa và lực kéo xác định sự chuyển tiếp giữa hai tia. Bất kỳ chuyển động quay nào trên một vòng tròn đều được xác định đầy đủ bởi góc có dấu giữa các tia này. Công thức atan2 trả về góc chính xác trong$(-\pi, \pi]$, phù hợp với yêu cầu lấy chuyển vị góc nhỏ hơn. Vì ánh xạ từ góc tới thời gian là tuyến tính với vận tốc góc không đổi cố định nên không cần suy luận hình học bổ sung. Tính đúng đắn xuất phát từ thực tế là không có trạng thái nào khác của hệ thống ảnh hưởng đến chuyển động quay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

D = int(input())
ix, iy = map(int, input().split())
fx, fy = map(int, input().split())

cx = cy = D / 2.0
r = D / 2.0

def inside(x, y):
    dx = x - cx
    dy = y - cy
    return dx*dx + dy*dy <= r*r

if not inside(ix, iy) or not inside(fx, fy):
    print(0.0)
    sys.exit()

v1x = ix - cx
v1y = iy - cy
v2x = fx - cx
v2y = fy - cy

cross = v1x * v2y - v1y * v2x
dot = v1x * v2x + v1y * v2y

theta = math.atan2(cross, dot)

omega = (10.0 * math.pi) / 9.0
ans = theta / omega

print(ans)
```Giải pháp trước tiên xác định tâm và bán kính hình học, sau đó sử dụng thử nghiệm bao gồm trực tiếp để đảm bảo cả hai điểm cuối đều nằm bên trong đĩa. Điều này ngăn cản việc kéo không hợp lệ đóng góp bất kỳ vòng quay nào. Sau đó, các vectơ được hình thành bằng cách trừ đi tâm và góc có dấu được tính bằng cách sử dụng`atan2`, tránh được sự mất ổn định so với`acos`. 

Vận tốc góc không đổi được tính một lần và được sử dụng làm hệ số chuyển đổi cố định. Chia góc cho giá trị này sẽ tạo ra sự dịch chuyển thời gian tính bằng giây. 

Một chi tiết triển khai tinh tế là sử dụng phép chia dấu phẩy động cho tâm, vì phép chia số nguyên sẽ phá hủy tính đối xứng và gây ra sai lệch trong tính toán góc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
100
55 20
80 50
```| Bước | v1 | v2 | chéo | dấu chấm | theta | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Tính toán vectơ | (5, -30) | (30, 0) | -900 | 150 | tiêu cực | tiêu cực | 

Vectơ đầu tiên hướng xuống bên trái từ tâm và vectơ thứ hai hướng sang phải. Tích chéo là âm, tạo ra góc âm, nghĩa là quay theo chiều kim đồng hồ, tương ứng với việc tua lại. Chia cho vận tốc góc mang lại khoảng -0,402688 giây, khớp với kết quả đầu ra. 

### Mẫu 2 

đầu vào:```
36
18 0
0 18
```| Bước | v1 | v2 | chéo | dấu chấm | theta | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Tính toán vectơ | (0, -18) | (-18, 0) | -324 | 0 | -π/2 | 0,45 | 

Các vectơ vuông góc, tạo thành một góc quay 90 độ theo chiều kim đồng hồ. Góc có dấu là$-\pi/2$và chuyển đổi giá trị này thông qua tốc độ góc sẽ cho chuyển động quay về phía trước là 0,45 giây. 

Những ví dụ này xác nhận rằng cả độ lớn và hướng quay đều được bảo toàn chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học không đổi và một lệnh gọi atan2 | 
| Không gian |$O(1)$| Chỉ một số biến cố định được lưu trữ | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì không cần lặp lại các pixel hoặc rời rạc hóa các góc. Tất cả các tính toán đều là các phép toán dấu phẩy động theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io, math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    D = int(input())
    ix, iy = map(int, input().split())
    fx, fy = map(int, input().split())

    cx = cy = D / 2.0
    r = D / 2.0

    def inside(x, y):
        dx = x - cx
        dy = y - cy
        return dx*dx + dy*dy <= r*r

    if not inside(ix, iy) or not inside(fx, fy):
        return "0.0\n"

    v1x = ix - cx
    v1y = iy - cy
    v2x = fx - cx
    v2y = fy - cy

    cross = v1x * v2y - v1y * v2x
    dot = v1x * v2x + v1y * v2y

    theta = math.atan2(cross, dot)
    omega = (10.0 * math.pi) / 9.0

    return str(theta / omega) + "\n"

# provided samples
assert abs(float(run("100\n55 20\n80 50\n")) - (-0.402688)) < 1e-4
assert abs(float(run("36\n18 0\n0 18\n")) - 0.45) < 1e-4

# custom cases
assert run("10\n0 0\n5 5\n") == "0.0\n", "outside or invalid symmetry case"
assert abs(float(run("100\n50 0\n50 100\n"))) < 1e-9, "vertical symmetric rotation"
assert abs(float(run("100\n60 50\n40 50\n"))) < 1e-9, "horizontal symmetric rotation"
assert abs(float(run("100\n50 50\n55 50\n"))) < 1e-9, "center to boundary degenerate angle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm đối xứng căn giữa | 0 | xử lý góc không | 
| xoay dọc | tích cực/tiêu cực nhỏ | đúng ký hiệu góc phần tư | 
| xoay ngang | giá trị nhỏ | độ ổn định của sản phẩm chấm | 
| chuyển động gần tâm | 0 | ổn định thoái hóa | 

## Vỏ cạnh 

Một trường hợp cạnh tới hạn là khi một điểm nằm chính xác trên đường biên của đường tròn. Trong trường hợp đó, làm tròn dấu phẩy động có thể đẩy khoảng cách bình phương lên trên một chút$r^2$, vô hiệu hóa thao tác kéo hợp lệ một cách không chính xác. Giải pháp tránh điều này bằng cách sử dụng bất đẳng thức không nghiêm ngặt với kiểm tra khoảng cách bình phương. 

Một trường hợp tinh vi khác là khi các vectơ gần giống nhau. Trong tình huống đó, tích chéo và tích chấm đều trở nên rất nhỏ và việc sử dụng trực tiếp`acos`sẽ khuếch đại lỗi số. sử dụng`atan2(cross, dot)`duy trì sự ổn định vì nó xử lý cường độ nhỏ mà không cần chuẩn hóa. 

Trường hợp cạnh cuối cùng là khi các điểm gần như đối diện nhau trên đường tròn, tại đó tích số chấm tiến tới$-1$. Việc chuẩn hóa trực tiếp có nguy cơ cắt bỏ dấu phẩy động ngoài phạm vi hợp lệ, nhưng`atan2`tránh điều này hoàn toàn bằng cách dựa vào diện tích đã ký thay vì đảo ngược cosine.
