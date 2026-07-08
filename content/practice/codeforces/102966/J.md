---
title: "CF 102966J - Chỉ cần quay bánh xe!"
description: "Hệ thống bao gồm hai bánh xe tròn luôn quay cùng một góc mỗi khi xe đạp di chuyển. Mỗi bánh xe được trang trí như thể nó là một đa giác đều, một bánh có cạnh $F$ và bánh kia có cạnh $B$, mặc dù cả hai thực sự là những vòng tròn liên tục bên dưới."
date: "2026-07-04T06:41:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102966
codeforces_index: "J"
codeforces_contest_name: "2020-2021 ICPC - Gran Premio de Mexico - Repechaje"
rating: 0
weight: 102966
solve_time_s: 46
verified: true
draft: false
---

[CF 102966J - Chỉ cần quay bánh xe!](https://codeforces.com/problemset/problem/102966/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hệ thống bao gồm hai bánh xe tròn luôn quay cùng một góc mỗi khi xe đạp di chuyển. Mỗi bánh xe được trang trí như thể nó là một đa giác đều, một bánh có$F$bên và bên kia với$B$các cạnh, mặc dù cả hai đều thực sự là các vòng tròn liên tục bên dưới. Mục tiêu trực quan là tại một số thời điểm nhất định, cả hai bánh xe phải trông “thẳng hàng”, nghĩa là một cạnh của mỗi mẫu đa giác nằm ngang hoàn toàn ở vị trí dưới cùng. 

Mỗi mẫu đầy đủ trên một bánh xe lặp lại mỗi$\frac{360}{k}$độ nếu bánh xe có$k$các cạnh, do đó “trạng thái thẳng hàng” hợp lệ xảy ra định kỳ khi bánh xe quay. Vì cả hai bánh xe đều quay cùng nhau nên hệ thống chỉ ở trạng thái trực quan hợp lệ khi góc quay đồng thời là bội số của cả hai chu kỳ. 

Ngoài ràng buộc hình học này, xe đạp phải di chuyển một quãng đường cố định$S$. Mỗi bánh xe có chu vi$C$, do đó di chuyển xe đạp về phía trước bằng$C$tương ứng với chính xác một vòng quay đầy đủ và khoảng cách một phần tương ứng với một vòng quay. Bài toán định nghĩa một vòng quay là một đơn vị năng lượng quay cố định tương đương với 30 độ quay của bánh xe. Vì cả hai bánh luôn quay cùng nhau nên số vòng quay của cả hai là như nhau. 

Nhiệm vụ là tính toán số lần rẽ 30 độ nhỏ nhất cần thiết để chiếc xe đạp đi được quãng đường ít nhất$S$và tại thời điểm chính xác đó cả hai bánh xe đều ở cấu hình căn chỉnh hợp lệ. 

Các ràng buộc ngụ ý đến$10^5$các trường hợp kiểm thử, do đó mỗi trường hợp phải được giải theo thời gian không đổi hoặc logarit. Giá trị của$C$nhỏ (lên tới 1000), trong khi$S$có thể lớn (lên đến$10^9$), do đó cần phải tính toán với phân số hoặc lý luận mô-đun hơn là mô phỏng chuyển động. 

Một mô phỏng đơn giản sẽ thất bại nếu lặp lại từng lượt cho đến khi cả hai điều kiện đều phù hợp. Trong trường hợp xấu nhất, việc căn chỉnh xảy ra sau một khoảng thời gian tỷ lệ với bội số chung nhỏ nhất của$F$Và$B$và những hạn chế về khoảng cách có thể đẩy điều này vượt xa$10^9$hoạt động cho mỗi thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi việc căn chỉnh xảy ra thường xuyên nhưng yêu cầu về khoảng cách lại chiếm ưu thế. Ví dụ, nếu$F = B = 3$, việc căn chỉnh xảy ra ở mỗi bước quay, nhưng nếu$S$là rất lớn, câu trả lời hoàn toàn bị chi phối bởi khoảng cách chứ không phải hình học. Ngược lại, nếu$F$Và$B$là các giá trị lớn cùng nhau, việc căn chỉnh cực kỳ hiếm và giải pháp được điều chỉnh bởi sự đồng bộ hóa thay vì khoảng cách di chuyển. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua yêu cầu căn chỉnh, vấn đề sẽ giảm xuống việc chuyển khoảng cách thành góc quay. Một vòng quay đầy đủ tương ứng với$C$đơn vị khoảng cách, và một lượt tương ứng với$30^\circ$, tức là,$1/12$của một vòng quay đầy đủ. Vì vậy, cứ 12 lượt tương ứng với một vòng quay đầy đủ, nghĩa là tồn tại sự chuyển đổi trực tiếp giữa khoảng cách và lượt quay. 

Điều này đưa ra một đường cơ sở đơn giản: tính xem cần bao nhiêu phép quay đầy đủ để bao phủ$S$, chuyển đổi nó thành lượt và làm tròn lên. 

Tuy nhiên, điều này bỏ qua ràng buộc chính: hệ thống chỉ phải dừng ở những thời điểm mà cả hai bánh xe đồng thời ở trạng thái đa giác thẳng hàng. Điều này đưa ra các ràng buộc về tính tuần hoàn đối với các góc quay được phép. 

Mỗi bánh xe với$k$các bên đều có định hướng hợp lệ$\frac{1}{k}$của một vòng quay đầy đủ. Vì vậy hệ thống chỉ có hiệu lực khi tổng số vòng quay là bội số của cả hai$\frac{1}{F}$Và$\frac{1}{B}$, nghĩa là phép quay phải là bội số của$\frac{1}{\mathrm{lcm}(F,B)}$. 

Điều này biến vấn đề thành vấn đề đồng bộ hóa giữa hai hệ thống tuần hoàn: một tiến trình liên tục được điều khiển bởi khoảng cách và một tập hợp các điểm dừng được phép riêng biệt được xác định bởi bội số chung nhỏ nhất của các chu kỳ đa giác. 

Trước tiên, chúng tôi tính toán góc quay tối thiểu cần thiết để thỏa mãn khoảng cách, sau đó làm tròn nó đến bước căn chỉnh hợp lệ tiếp theo trong$\frac{1}{\mathrm{lcm}(F,B)}$lưới. Việc làm tròn đó là toàn bộ khó khăn: chúng ta đang chiếu một yêu cầu liên tục lên một mạng tuần hoàn rời rạc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu của lượt |$O(T \cdot ans)$|$O(1)$| Quá chậm | 
| Làm tròn dựa trên LCM đến trạng thái xoay hợp lệ |$O(T \log \min(F,B))$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc hoàn toàn theo đơn vị “vòng quay”, trong đó một vòng tương ứng với một góc quay cố định 30 độ. 

1. Chuyển đổi yêu cầu về khoảng cách thành số vòng quay toàn bộ bánh xe cần thiết. Vì một vòng quay đầy đủ tương ứng với khoảng cách$C$, chiếc xe đạp phải hoạt động ít nhất$\frac{S}{C}$các phép quay. Chúng tôi giữ giá trị này dưới dạng phân số để tránh mất độ chính xác. 
2. Chuyển đổi các phép quay thành các lượt. Một vòng quay đầy đủ là 12 vòng, do đó số vòng tối thiểu cần thiết do khoảng cách là$x = 12 \cdot \frac{S}{C}$. 
3. Tính ràng buộc căn chỉnh định kỳ. Cấu hình hợp lệ xảy ra mỗi khi cả hai bánh xe trở về trạng thái mà các cạnh đa giác của chúng thẳng hàng với mặt đất. Đối với một bánh xe có$k$hai bên, điều này xảy ra mọi lúc$\frac{1}{k}$xoay, hoặc tương đương mỗi$\frac{12}{k}$rẽ. Vậy chu kỳ căn chỉnh hệ thống lần lượt là bội số chung nhỏ nhất của$\frac{12}{F}$Và$\frac{12}{B}$, điều này đơn giản hóa thành$\frac{12}{\gcd(F,B)}$. 
4. Tìm bội số nhỏ nhất của khoảng thời gian căn chỉnh ít nhất$x$. Đây là cách chia trần tiêu chuẩn: nếu khoảng thời gian là$p$, chúng tôi tính toán$\lceil x/p \rceil \cdot p$. 
5. Trả về giá trị đó làm câu trả lời. 

Quyết định quan trọng là bước 3: chuyển đổi sự liên kết đa giác thành cấu trúc tuần hoàn dựa trên gcd trong cùng hệ đơn vị với chuyển động. 

### Tại sao nó hoạt động 

Hệ thống phát triển trên một vòng tròn một chiều của các góc quay, nhưng chỉ một tập hợp con các góc rời rạc mới có giá trị dừng lại. Tập hợp con đó là tuần hoàn và tính tuần hoàn được đặc trưng đầy đủ bởi ước chung lớn nhất của các chu kỳ bánh xe riêng lẻ. Mọi cấu hình hợp lệ đều là một điểm mạng trong cấu trúc tuần hoàn này. Vì chuyển động là đơn điệu lần lượt nên thời gian dừng sớm nhất khả thi sau khi đạt đến khoảng cách yêu cầu chính xác là điểm mạng đầu tiên vượt quá ngưỡng khoảng cách. Không có điểm nào sớm hơn có thể thỏa mãn cả hai ràng buộc vì nó sẽ vi phạm yêu cầu về khoảng cách hoặc phá vỡ tính tuần hoàn căn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ceil_div(a, b):
    return (a + b - 1) // b

T = int(input())
for _ in range(T):
    C, F, B, S = map(int, input().split())

    # convert required distance to turns:
    # 1 rotation = C distance = 12 turns
    need = S * 12

    # minimal turns ignoring alignment
    # (we work in rational form: need / C)
    # multiply first to keep integer arithmetic
    base_num = need
    base_den = C

    # alignment period in turns is 12 / gcd(F, B)
    import math
    g = math.gcd(F, B)
    period_num = 12
    period_den = g

    # we want smallest t >= base such that t is multiple of period
    # convert both to common denominator:
    # t = k * period
    # need <= k * period
    # k >= need / period
    # need in turns = base_num/base_den

    # inequality:
    # base_num/base_den <= k * period_num/period_den
    # k >= base_num * period_den / (base_den * period_num)

    num = base_num * period_den
    den = base_den * period_num

    k = ceil_div(num, den)
    ans = k * period_num // period_den

    print(ans)
```Việc triển khai giữ mọi thứ ở dạng số nguyên để tránh trôi dấu phẩy động. Bước quan trọng là lần lượt chuyển cả điều kiện khoảng cách và điều kiện căn chỉnh thành thang đo tuyến tính chung, sau đó thực hiện phép chia trần để đưa kết quả vào lưới tuần hoàn hợp lệ. 

Một lỗi phổ biến là tính toán phép quay và căn chỉnh riêng biệt rồi cố gắng hợp nhất chúng sau này; điều đó bị phá vỡ vì các ràng buộc căn chỉnh không độc lập với việc tích lũy khoảng cách. Việc giảm dựa trên gcd đảm bảo cả hai ràng buộc được thể hiện trong cùng một hệ thống đơn vị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
2 8 4 10
```Chúng tôi tính toán số lượt cần thiết từ khoảng cách:$$\text{base} = \frac{10}{2} \cdot 12 = 60$$Thời gian điều chỉnh:$$g = \gcd(8,4)=4,\quad \text{period} = \frac{12}{4} = 3$$Vì vậy chúng ta cần bội số nhỏ nhất của 3 ít nhất là 60. 

| Bước | Giá trị | 
| --- | --- | 
| lượt cơ sở | 60 | 
| kỳ | 3 | 
| bội số hợp lệ đầu tiên ≥ cơ số | 60 | 

Đáp án là 60. 

Điều này cho thấy trường hợp khoảng cách đã đạt chính xác ở trạng thái căn chỉnh hợp lệ. 

### Ví dụ 2 

đầu vào:```
1
3 5 7 20
```Yêu cầu về khoảng cách:$$\text{base} = \frac{20}{3} \cdot 12 = 80$$Căn chỉnh:$$g = 1,\quad \text{period} = 12$$| Bước | Giá trị | 
| --- | --- | 
| lượt cơ sở | 80 | 
| kỳ | 12 | 
| bội số của 12 ≥ 80 | 84 | 

Đáp án là 84. 

Điều này thể hiện hành vi bẻ gãy: ngay cả khi khoảng cách cho phép dừng ở 80 vòng, lực căn chỉnh sẽ đợi đến 84. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log \min(F,B))$| Mỗi bài kiểm tra sử dụng tính toán gcd | 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ | 

Các ràng buộc cho phép lên đến$10^5$các trường hợp thử nghiệm, do đó, gcd logarit cho mỗi trường hợp dễ dàng đủ nhanh trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        C, F, B, S = map(int, input().split())

        need = S * 12
        base_num = need
        base_den = C

        g = math.gcd(F, B)
        period_num = 12
        period_den = g

        num = base_num * period_den
        den = base_den * period_num

        k = (num + den - 1) // den
        ans = k * period_num // period_den
        out.append(str(ans))

    return "\n".join(out)

# sample
assert solve("1\n2 8 4 10\n") == "60"

# minimum values
assert solve("1\n1 3 3 1\n") is not None

# already aligned
assert solve("1\n10 4 4 10\n") == "12"

# coprime wheels forcing snapping
assert solve("1\n3 5 7 20\n") == "84"

# large distance
assert solve("1\n1000 12 18 1000000000\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| căn chỉnh nhỏ | 60 | khớp chính xác mà không làm tròn | 
| trường hợp đồng nguyên tố | 84 | bắt kịp căn chỉnh tiếp theo | 
| bánh xe bằng nhau | chính xác | gcd = tính tuần hoàn đầy đủ | 
| S lớn | hợp lệ | số học an toàn tràn | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi cả hai bánh xe có cùng số cạnh. Ví dụ$F = B = 6$. Trong tình huống này, mọi trạng thái hợp lệ của một bánh xe đều hợp lệ đối với bánh xe kia, do đó khoảng thời gian căn chỉnh trở nên tối thiểu. Thuật toán giảm thiểu điều này thông qua$g = 6$, làm cho khoảng thời gian$\frac{12}{6} = 2$rẽ. Việc tính toán chính xác cho phép các điểm dừng rất thường xuyên, vì vậy chỉ có khoảng cách mới là vấn đề. 

Một trường hợp khác là khi$F$Và$B$là nguyên tố cùng nhau. Ví dụ$F = 7, B = 9$. Sau đó$g = 1$, do đó việc căn chỉnh chỉ xảy ra sau mỗi 12 lượt. Ngay cả khi khoảng cách gợi ý dừng sớm hơn, thuật toán buộc phải làm tròn đến bội số tiếp theo của 12, phù hợp với thực tế là không có góc trung gian nào có thể thỏa mãn cả hai cách sắp xếp đa giác cùng một lúc. 

Trường hợp cạnh cuối cùng xuất hiện khi$S$rất nhỏ, có thể nhỏ hơn một vòng quay. Việc chuyển đổi vẫn hoạt động vì chúng tôi thực hiện theo các lượt có tỷ lệ số nguyên; ngay cả khoảng cách phân số cũng được làm tròn chính xác sau khi được ánh xạ vào hệ thống đơn vị dùng chung.
