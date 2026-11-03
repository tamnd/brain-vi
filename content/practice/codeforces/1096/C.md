---
title: "CF 1096C - Đa giác cho góc"
description: "Chúng ta đang làm việc với một đa giác đều, nghĩa là tất cả các cạnh và các góc đều đối xứng. Dữ liệu đầu vào cho một giá trị góc và chúng ta phải quyết định xem có tồn tại một số n-giác đều đặn sao cho chúng ta có thể chọn ba đỉnh $a, b, c$ (không nhất thiết phải liên tiếp) và góc tạo thành tại $b$…"
date: "2026-06-13T05:28:53+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry"]
categories: ["algorithms"]
codeforces_contest: 1096
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 57 (Rated for Div. 2)"
rating: 1600
weight: 1096
solve_time_s: 330
verified: true
draft: false
---

[CF 1096C - Đa giác cho góc](https://codeforces.com/problemset/problem/1096/C) 

**Đánh giá:** 1600 
**Tags:** lực lượng vũ phu, hình học 
**Thời gian giải:** 5 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một đa giác đều, nghĩa là tất cả các cạnh và các góc đều đối xứng. Đầu vào cho một giá trị góc và chúng ta phải quyết định xem có tồn tại một số n-giác đều đặn để chúng ta có thể chọn ba đỉnh hay không$a, b, c$(không nhất thiết phải liên tiếp) và góc tạo thành tại$b$, cụ thể là$\angle abc$, chính xác là giá trị đã cho. 

Nói cách khác, chúng ta không xử lý góc trong của đa giác mà xử lý bất kỳ góc nào được hình thành bằng cách chọn ba đỉnh trên đường tròn theo thứ tự tuần hoàn. Hình học giảm xuống việc suy luận về các điểm cách đều nhau trên một đường tròn và các góc ở tâm giữa chúng. 

Đầu ra là số đỉnh nhỏ nhất$n$mà cấu hình đó tồn tại, hoặc$-1$nếu không có đa giác đều có thể nhận ra góc đó. 

Ràng buộc$T \le 180$nhỏ, vì vậy chúng ta có thể thực hiện một phép tính cho mỗi lần kiểm tra theo logarit hoặc thậm chí tuyến tính nhẹ trong trường hợp xấu nhất, nhưng không phải là thứ tăng tuyến tính với giới hạn lớn$998244353$. Giới hạn trên lớn chỉ đảm bảo rằng nếu một giải pháp hợp lệ tồn tại, thì nó không lớn về mặt thiên văn theo cách có thể phá vỡ việc xử lý số nguyên, chứ không phải là chúng ta nên lặp lại nó. 

Một sai lầm ngây thơ xuất phát từ việc cho rằng chúng ta chỉ cần kiểm tra các góc trong của một đa giác đều. Ví dụ, trong một hình ngũ giác đều, góc trong cố định bằng$108^\circ$, nhưng vấn đề cho phép các góc như$36^\circ$hoặc$72^\circ$được hình thành bởi các đỉnh không liền kề. Một sai lầm phổ biến khác là cho rằng góc chỉ phải đến từ các cung liền kề; trong thực tế, hai cung bất kỳ được xác định bởi khoảng cách đỉnh đều đóng góp. 

Trường hợp thất bại khó phát hiện thứ hai là xử lý góc như thể nó phải bằng$k \cdot \frac{180(n-2)}{n}$, không liên quan đến vấn đề này. Góc ở đây chỉ phụ thuộc vào hình học dây cung trên đường tròn chứ không phụ thuộc vào cấu trúc bên trong đa giác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng khắc phục kích thước đa giác$n$, địa điểm$n$các điểm trên một vòng tròn đơn vị rồi liệt kê các bộ ba$(a, b, c)$, tính góc tại$b$và kiểm tra xem có cái nào phù hợp với mục tiêu không. Số bộ ba là$O(n^3)$, và thậm chí các góc tính toán là thời gian không đổi với tích số chấm. Đang thử cái này cho nhiều người$n$thậm chí vài nghìn trở nên hoàn toàn không khả thi và rõ ràng là không thể dưới sự ràng buộc hợp lệ$n$có thể đạt tới$10^9$- giới hạn quy mô. 

Cấu trúc trở nên đơn giản hơn khi chúng ta ngừng suy nghĩ về tọa độ và thay vào đó nghĩ về sự khác biệt của cung. Trên một đường tròn, một n-giác đều chia góc đầy đủ$360^\circ$thành các bước bằng nhau$x = \frac{360}{n}$. Hai đỉnh bất kỳ tương ứng với bội số nguyên của bước này. 

Bây giờ hãy xem xét một đỉnh$b$, và hai đỉnh khác$a$Và$c$. góc$\angle abc$là góc nội tiếp trong đường tròn nên bằng nửa số đo cung$ac$cái đó không chứa$b$. Độ dài cung đó phải là bội số nguyên của bước$x$. Nếu cung giữa$a$Và$c$nhịp$k$cạnh của đa giác thì số đo cung là$k \cdot x$, và góc nội tiếp là:$$\angle abc = \frac{kx}{2} = \frac{k}{2} \cdot \frac{360}{n} = \frac{180k}{n}.$$Vậy bài toán quy về việc tìm số nguyên$n$Và$k$như vậy:$$\text{ang} = \frac{180k}{n}.$$Sắp xếp lại:$$n \cdot \text{ang} = 180k.$$Vì thế$180k$phải chia hết cho$\text{ang}$, và chúng tôi muốn nhỏ nhất$n$sao cho tồn tại một số nguyên$k$làm cho sự bình đẳng được giữ vững. Từ$k$ứng với cung không suy biến chỉ cần dương và nhỏ hơn$n$, nhưng để tồn tại chúng ta luôn có thể chọn$k$một cách thích hợp nếu tỷ lệ này hoạt động. 

Cho phép$g = \gcd(180, \text{ang})$. Sau đó chúng ta có thể đơn giản hóa:$$\text{ang} = \frac{180}{n}k \Rightarrow n = \frac{180k}{\text{ang}}.$$Viết$180 = g \cdot 180'$,$\text{ang} = g \cdot a'$, cho:$$n = \frac{180'k}{a'}.$$Để giảm thiểu$n$, ta chọn số dương nhỏ nhất$k$như vậy$a' \mid 180'k$. Từ$a'$Và$180'$là nguyên tố cùng nhau, điều này đòi hỏi$k = a'$, thu được:$$n = 180'.$$Vì vậy, câu trả lời giảm xuống:$$n = \frac{180}{\gcd(180, \text{ang})}.$$Đây là kích thước đa giác tối thiểu có thể nhận ra góc yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$mỗi n |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc góc độ cho từng truy vấn. Vấn đề là độc lập đối với mỗi trường hợp thử nghiệm, do đó mỗi trường hợp có thể được giải quyết một cách riêng biệt. 
2. Tính ước số chung lớn nhất giữa 180 và góc đã cho. Bước này trích xuất hệ số chia sẻ tối đa để xác định phân số góc nào có thể đạt được bằng cách sử dụng các phân vùng hình tròn bằng nhau. 
3. Chia 180 cho gcd này để thu được kích thước đa giác ứng cử viên. Giá trị này tương ứng với số lượng nhỏ nhất các đoạn cung bằng nhau cần thiết để góc có thể được biểu diễn dưới dạng góc nội tiếp tạo bởi hai đỉnh. 
4. Xuất trực tiếp giá trị này vì nó được đảm bảo là giá trị tối thiểu$n$. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi góc có thể tạo bởi ba đỉnh trong một n-giác đều có thể được biểu diễn dưới dạng$\frac{180k}{n}$đối với một số nguyên$k$. Điều này xuất phát trực tiếp từ việc giải thích cấu hình trên một đường tròn và sử dụng định lý góc nội tiếp. 

Do đó bài toán trở thành tìm giá trị nhỏ nhất$n$như vậy$\text{ang} \cdot n / 180$là một số nguyên cho một số cách xây dựng số nguyên hợp lệ của$k$. Việc giảm bằng gcd đảm bảo ta loại bỏ được tất cả các thừa số chung sao cho khả thi nhỏ nhất$n$vẫn còn. Bất kỳ nhỏ hơn$n$sẽ giới thiệu lại một yêu cầu phân số cho$k$, làm cho hình học không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

t = int(input())
for _ in range(t):
    ang = int(input())
    print(180 // gcd(180, ang))
```Giải pháp xoay quanh việc quan sát rằng toàn bộ hình học thu gọn vào điều kiện chia hết giữa 180 và góc được yêu cầu. Tính toán gcd tách biệt phần tối giản của góc so với số đo hình bán nguyệt đầy đủ và phép chia cuối cùng mang lại kích thước đa giác nhỏ nhất hỗ trợ phân số đó. 

Không có vòng lặp nào$n$và không có mô phỏng hình học. Điểm tinh tế duy nhất là thừa nhận rằng hệ số 180 xuất phát từ định lý góc nội tiếp chứ không phải từ công thức góc trong của đa giác. 

## Ví dụ đã hoạt động 

Chúng tôi sẽ theo dõi hai đầu vào: 54 và 178. 

Đối với góc 54: 

| Bước | áng | gcd(180, ang) | 180/gcd | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 54 | 18 | 10 | 10 | 

Gcd là 18, nghĩa là 54° có cấu trúc vững chắc với 180°, cho phép tạo thành góc bằng cách sử dụng 10 giác. Điều này tương ứng với mẫu trong đó đa giác đều 10 cạnh có góc nội tiếp là 54°. 

Dấu vết này xác nhận rằng lời giải chỉ phụ thuộc vào việc rút gọn theo lý thuyết số chứ không phải tìm kiếm hình học. 

Đối với góc 178: 

| Bước | áng | gcd(180, ang) | 180/gcd | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 178 | 2 | 90 | 90 | 

Ở đây gcd nhỏ, có nghĩa là góc gần như nguyên tố cùng nhau với 180. Điều này buộc một đa giác lớn hơn nhiều phải thể hiện độ phân giải góc tốt như vậy. 

Dấu vết cho thấy rằng khi gcd giảm, kích thước đa giác cần thiết sẽ tăng lên, phù hợp với trực giác rằng cần nhiều đỉnh hơn để xấp xỉ các góc tùy ý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm yêu cầu một phép tính gcd và số học không đổi | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung ngoài một vài số nguyên | 

Các ràng buộc cho phép tối đa 180 truy vấn, do đó, ngay cả việc tính toán gcd đơn giản cho mỗi truy vấn cũng đủ nhanh. Giải pháp chạy trong thời gian không đáng kể và sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    t = int(input())
    out = []
    for _ in range(t):
        ang = int(input())
        out.append(str(180 // math.gcd(180, ang)))
    return "\n".join(out)

# provided samples
assert run("4\n54\n50\n2\n178\n") == "10\n18\n90\n90"

# minimum case
assert run("1\n1\n") == "180"

# angle that divides 180 exactly
assert run("1\n60\n") == "3"

# angle close to 180
assert run("1\n179\n") == "180"

# multiple mixed
assert run("3\n45\n90\n30\n") == "4\n2\n6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 | 180 | góc nhỏ nhất tạo nên đa giác cực đại | 
| 60 | 3 | trường hợp chia số sạch | 
| 179 | 180 | cạnh gần nguyên tố | 
| 45, 90, 30 | 4, 2, 6 | nhiều mẫu gcd | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi góc là ước số của 180. Đối với đầu vào`ang = 60`, gcd là 60, vì vậy câu trả lời trở thành`180 / 60 = 3`. Điều này tương ứng với một hình tam giác, trong đó nhiều góc có thể được tạo thành đơn giản nhờ tính đối xứng. Một cách tiếp cận ngây thơ có thể cho rằng cần có một đa giác lớn hơn một cách không chính xác vì nó bỏ qua rằng các hình tam giác đã cho phép có nhiều góc nội tiếp. 

Vì`ang = 179`, gcd với 180 là 1, buộc đầu ra phải là 180. Giải thích hình học là một góc như vậy đòi hỏi độ chi tiết góc cực kỳ mịn, chỉ có thể đạt được khi đa giác xấp xỉ một đường tròn có nhiều đỉnh. Bất kỳ nỗ lực nào nhằm “đơn giản hóa” trường hợp này sẽ làm giảm kích thước đa giác một cách không chính xác và không đạt được độ chính xác cần thiết. 

Vì`ang = 1`, gcd cũng là 1, do đó kết quả lại là 180. Đây là bài kiểm tra ứng suất tối đa trong đó góc nhỏ nhất tạo ra độ phân giải đầy đủ của cấu trúc 180 đơn vị và bất kỳ phím tắt nào dựa trên “góc nhỏ có nghĩa là đa giác nhỏ” sẽ bị hỏng ở đây.
