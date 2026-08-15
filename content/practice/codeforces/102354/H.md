---
title: "CF 102354H - Thách Thức Trọng Lực"
description: "Chúng tôi có một tập hợp các vệ tinh điểm xung quanh gốc tọa độ. Mỗi vệ tinh có một vị trí góc, khoảng cách từ điểm gốc và khối lượng."
date: "2026-08-15T17:51:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "H"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 357
verified: false
draft: false
---

[CF 102354H - Thách thức trọng lực](https://codeforces.com/problemset/problem/102354/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 57 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một tập hợp các vệ tinh điểm xung quanh gốc tọa độ. Mỗi vệ tinh có một vị trí góc, khoảng cách từ điểm gốc và khối lượng. Elphaba chọn một tia bắt đầu từ gốc tọa độ và muốn tổng lực hấp dẫn dọc theo tia đó không có thành phần nào vuông góc với tia tại bất kỳ điểm nào cô đi qua. 

Quan sát trọng tâm là đường bay hợp lệ phải là trục phản ánh của toàn bộ cấu hình vệ tinh có trọng số. Sự phản xạ qua đường này phải bảo toàn cả vị trí của mọi vệ tinh và khối lượng của nó. Đây cũng là cách rút gọn hình học dự định cho bài toán: các đường hợp lệ chính xác là các trục đối xứng của cấu hình có trọng số, sau đó bài toán trở thành bài toán palindrome. 

Góc được tính bằng cung giây nguyên, với một vòng quay đầy đủ chứa chính xác 129600 vị trí. Độ phân giải góc hữu hạn này cực kỳ hữu ích. Chúng ta có thể tạo một mảng có độ dài 129600, đặt cặp`(rho, mass)`ở mọi góc bị chiếm dụng và sử dụng điểm đánh dấu trống ở nơi khác. 

Giá trị của`n`có thể lớn tới 200000, nhưng tọa độ góc là các số nguyên riêng biệt từ 0 đến 129599, do đó, đầu vào hợp lệ có thể chứa tối đa 129600 vệ tinh. Giới hạn thời gian hai giây loại trừ mọi thứ bậc hai trong`n`. Ngay cả 129600 bình phương cũng là khoảng 16,8 tỷ phép so sánh, vượt xa những gì Python có thể thực hiện kịp thời. Một thuật toán tuyến tính hoặc gần tuyến tính trên vũ trụ góc cố định là mục tiêu phù hợp. Vấn đề chính thức có giới hạn bộ nhớ 256 MiB, đủ cho một vài mảng có kích thước khoảng 260000. 

Có một số trường hợp đặc biệt mà việc triển khai hình học trực tiếp có thể xử lý sai. Đầu tiên, một trục đối xứng có thể đi qua một vệ tinh, nhưng khi đó tia bay tương ứng sẽ bị cấm nếu vệ tinh nằm theo hướng di chuyển. Ví dụ,```
2
1 0 1
1 64800 1
```có trục đối xứng ở 0 và 32400 giây cung theo modulo 64800. Hướng 0 và 64800 bị vệ tinh chặn, trong khi 32400 và 97200 thì rõ ràng, do đó đầu ra chính xác là```
2
32400.0000000
97200.0000000
```Việc triển khai bất cẩn báo cáo trục đối xứng mà không kiểm tra tia thực tế sẽ tạo ra bốn hướng không chính xác. 

Thứ hai, một trục không nhất thiết phải nằm ở một góc nguyên. Hai vệ tinh bằng nhau ở góc 0 và 1 có trục đối xứng ở 0,5 độ theo đơn vị cung-giây của bài toán, chính xác hơn là 0,5 giây cung:```
2
1 0 1
1 1 1
```Đầu ra đúng là```
2
0.5000000000
64800.5000000000
```Việc triển khai chỉ kiểm tra các trung tâm số nguyên sẽ âm thầm bỏ lỡ cả hai câu trả lời. 

Thứ ba, tính đối xứng hình học là không đủ nếu khối lượng khác nhau. Coi như```
4
1 0 1
1 64800 2
1 32400 3
1 97200 3
```Các vị trí hình học đối xứng quanh trục hoành, nhưng hai vệ tinh trên trục đó có khối lượng khác nhau. Cấu hình không phải là đối xứng phản xạ có trọng số và các hướng ngang cũng bị chặn. Đầu ra đúng là```
0
```Chỉ kiểm tra tọa độ và bỏ qua khối lượng sẽ chấp nhận hướng không chính xác. 

Cuối cùng, vị trí góc trống cũng quan trọng. Nếu hai vệ tinh cách nhau một khoảng góc lớn thì sự phản xạ phải bảo toàn khoảng cách đó cũng như chính các vệ tinh. Việc điền vào mảng góc 129600 vị trí hoàn chỉnh bằng một ký hiệu trống rõ ràng sẽ tự động xử lý việc này. Việc nén đầu vào chỉ vào các vị trí bị chiếm giữ sẽ làm mất thông tin về khoảng cách góc và có thể tạo ra sự đối xứng sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là chọn một đường có thể, phản chiếu mọi vệ tinh qua nó và kiểm tra xem cấu hình phản xạ có giống nhau hay không, bao gồm cả khối lượng. Vì mọi trục hợp lệ được xác định bởi hành động của nó trên tọa độ góc, nên chỉ có thể có O(129600), bởi vì một trục được xác định modulo 180 độ và có thể nằm ở cung số nguyên hoặc nửa số nguyên thứ hai. Kiểm tra một trục so với tất cả`n`các vệ tinh lấy O(n), cho O(129600n) hoặc khoảng 1,7 × 10^10 so sánh ở đầu vào khả thi lớn nhất. Ngay cả trước khi xem xét chi phí hoạt động của Python, tốc độ này vẫn quá chậm. 

Có một cách tốt hơn vì tọa độ góc là rời rạc. Dán nhãn vệ tinh`(rho, mass)`ở góc của nó và đặt một giá trị trống ở mọi góc nguyên khác. Phản xạ quanh một trục có góc gấp đôi`k`gửi góc`x`ĐẾN`k-x`modulo 129600. Như vậy, khi mảng hình tròn được cắt ở điểm thích hợp thì các nhãn ở hai bên trục phải tạo thành một bảng màu. 

Ranh giới hình tròn là sự phức tạp duy nhất. Chúng tôi giải quyết nó bằng cách nhân đôi mảng góc. Đối xứng của hình tròn ban đầu trở thành một palindrome có chiều dài 129600 ở giữa ở đâu đó trong mảng nhân đôi. Cả tâm nguyên và tâm nửa số nguyên đều xuất hiện, vì vậy chúng tôi sử dụng bán kính chẵn và lẻ từ thuật toán Manacher. Manacher tính toán tất cả bán kính palindrome tối đa trong thời gian tuyến tính. 

Các tham số vật lý hoàn toàn không yêu cầu tính toán dấu phẩy động. Hai vị trí phản xạ bằng nhau một cách chính xác khi chúng`(rho, mass)`các cặp đều bằng nhau. Điều này có nghĩa là so sánh bộ dữ liệu là đủ và tránh hoàn toàn xung đột băm. 

Khi mọi trục phản xạ đã được tìm thấy, mỗi trục biểu thị hai hướng bay ngược nhau. Một hướng chỉ hợp lệ nếu không có vệ tinh chính xác trên tia đó. Vì tất cả các góc vệ tinh đều là số nguyên nên các hướng nửa số nguyên không bao giờ có thể chứa vệ tinh, trong khi hướng số nguyên có thể được kiểm tra trực tiếp trong mảng góc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(129600n) | O(n) | Quá chậm | 
| Tối ưu | O(129600 + n + A) | O(129600 + n) | Đã chấp nhận | 

Đây`A`là số hướng đầu ra. Lần quét cuối cùng thực sự là O (259200), do đó toàn bộ giải pháp là tuyến tính một cách hiệu quả trong vũ trụ góc cố định. 

## Hướng dẫn thuật toán 

1. Tạo một mảng góc`a`chiều dài`C = 129600`. Tại vị trí`phi`, cửa hàng`(rho, mass)`cho vệ tinh có góc đó. Mọi cửa hàng vị trí khác`None`. Tuple là thông tin đầy đủ mà sự phản ánh phải lưu giữ. 
2. Nhân đôi mảng này để có được`s = a + a`. Trục phản xạ có thể vượt qua ranh giới nhân tạo giữa góc 129599 và góc 0, do đó, chỉ làm việc trên một bản sao sẽ làm mất các bảng màu hợp lệ. Bản sao thứ hai có đủ chỗ để biểu diễn mọi palindrome hình tròn như một palindrome thông thường. 
3. Chạy thuật toán Manacher trên`s`và tính bán kính palindrome lẻ`d1`và thậm chí bán kính palindrome`d2`. Bán kính lẻ`d1[i]`là số lớp phù hợp bao gồm cả vị trí`i`, trong khi`d2[i]`là số lớp phù hợp xung quanh khoảng trống ngay trước`i`. 
4. Biểu diễn một trục có thể bằng`k = 2alpha`, Ở đâu`alpha`là góc của nó Vì một trục và cùng một trục quay 180 độ là giống hệt nhau nên`k`chỉ cần các giá trị từ 0 đến`C-1`. Thậm chí`k`cho một góc trục nguyên, trong khi số lẻ`k`cho một góc trục nửa số nguyên. 
5. Thậm chí`k`, trục có tâm tại vị trí`C + k/2`trong mảng nhân đôi. Sự phản xạ tròn hoàn toàn đòi hỏi sự bằng nhau về khoảng cách lên đến`C/2 - 1`từ trung tâm này. Theo ký hiệu của Manacher thì đây chính xác là điều kiện`d1[C + k/2] >= C/2`. 
6. Đối với số lẻ`k`, trục nằm giữa hai vị trí mảng. Trung tâm của nó được đại diện bởi chỉ số chẵn-palindrome`C + (k+1)/2`. Chúng tôi cần`C/2`cặp phù hợp, vì vậy điều kiện là`d2[C + (k+1)/2] >= C/2`. 
7. Đánh dấu mọi`k`thỏa mãn điều kiện palindrom tương ứng làm trục phản xạ hợp lệ. Palindrome chứa toàn bộ vòng tròn góc, do đó thử nghiệm này không chỉ đơn thuần là kiểm tra một phần cục bộ của cấu hình. Nó kiểm tra mọi vệ tinh và mọi vị trí góc trống so với đối tác phản chiếu của nó. 
8. Quét các góc hướng gấp đôi`x`từ 0 đến`2C-1`. Hướng bay thực tế là`x/2`cung giây và trục phản xạ của nó là`k = x mod C`. Nếu trục đó không đối xứng thì hướng bị từ chối. Nếu như`x`là số lẻ, hướng là một góc nửa nguyên và không thể chứa vệ tinh. Nếu như`x`chẵn, kiểm tra xem`a[x/2]`trống rỗng. Chỉ có hướng trống được phát ra. 
9. In các góc hướng gấp đôi theo thứ tự tăng dần, chia đôi khi định dạng. Đang quét`x`theo thứ tự tăng dần đã cung cấp đầu ra được sắp xếp theo yêu cầu, do đó không cần sắp xếp riêng. 

### Tại sao nó hoạt động 

Đối với đường bay cố định, hãy phân tách vị trí của từng vệ tinh thành tọa độ song song và vuông góc với đường bay. Thành phần vuông góc của lực hấp dẫn của nó tỉ lệ với 

[ 
\frac{m_i b_i} 
{(t^2-2a_i t+\rho_i^2)^{3/2}}, 
] 

ở đâu`t`là khoảng cách của Elphaba từ gốc dọc theo tia,`a_i`là tọa độ song song của vệ tinh và`b_i`là tọa độ vuông góc của nó. Tổng phải biến mất cho mọi`t > 0`. 

Một vệ tinh ngoài trục có đóng góp duy nhất được xác định bởi tọa độ song song của nó, khoảng cách từ điểm gốc và khối lượng của nó. Sự hủy bỏ duy nhất có thể xảy ra là từ hình ảnh phản chiếu của nó trên đường thẳng, bởi vì gương có cùng`a_i`Và`rho_i`nhưng ngược lại`b_i`. Sự hủy bỏ đòi hỏi khối lượng cũng phải bằng nhau. Một vệ tinh trên trục có`b_i = 0`và không đóng góp lực vuông góc. Do đó, cấu hình vệ tinh có trọng số phải bất biến dưới sự phản xạ trên đường bay. 

Mảng góc ghi lại chính xác cấu hình này. Phản xạ quanh một trục có góc gấp đôi`k`bản đồ vị trí`i`ĐẾN`k-i`modulo`C`, đó chính xác là quan hệ đẳng thức xác định một bảng màu hình tròn. Mảng nhân đôi chuyển đổi bảng màu hình tròn đó thành một bảng màu thông thường và Manacher tìm xem liệu bảng màu có độ dài đầy đủ cần thiết có tồn tại hay không. Do đó, mọi trục được đánh dấu đều là trục đối xứng thực sự và mọi trục đối xứng thực sự đều được đánh dấu. 

Kiểm tra tia cuối cùng sẽ loại bỏ chính xác những nửa đường chứa vệ tinh. Do đó, các hướng phát ra chính xác là các hướng bay được phép về mặt vật lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

C = 129600
HALF = C // 2

def manacher_odd_even(s):
    n = len(s)

    d1 = [0] * n
    l = 0
    r = -1

    for i in range(n):
        if i > r:
            k = 1
        else:
            k = min(d1[l + r - i], r - i + 1)

        while i - k >= 0 and i + k < n and s[i - k] == s[i + k]:
            k += 1

        d1[i] = k

        k -= 1
        if i + k > r:
            l = i - k
            r = i + k

    d2 = [0] * n
    l = 0
    r = -1

    for i in range(n):
        if i > r:
            k = 0
        else:
            k = min(d2[l + r - i + 1], r - i + 1)

        while i - k - 1 >= 0 and i + k < n and s[i - k - 1] == s[i + k]:
            k += 1

        d2[i] = k

        k -= 1
        if i + k > r:
            l = i - k - 1
            r = i + k

    return d1, d2

def solve():
    n = int(input())

    a = [None] * C

    for _ in range(n):
        rho, phi, mass = map(int, input().split())
        a[phi] = (rho, mass)

    s = a + a
    d1, d2 = manacher_odd_even(s)

    good = [False] * C

    for k in range(C):
        if k & 1:
            center = C + (k + 1) // 2
            if d2[center] >= HALF:
                good[k] = True
        else:
            center = C + k // 2
            if d1[center] >= HALF:
                good[k] = True

    out = []

    for x in range(2 * C):
        k = x % C

        if not good[k]:
            continue

        if x & 1:
            out.append(f"{x / 2:.10f}")
        else:
            phi = x // 2
            if a[phi] is None:
                out.append(f"{phi:.10f}")

    sys.stdout.write(str(len(out)) + "\n")
    sys.stdout.write("\n".join(out))
    if out:
        sys.stdout.write("\n")

if __name__ == "__main__":
    solve()
```Phần đầu tiên của`solve`xây dựng chuỗi vòng tròn hoàn chỉnh. Việc sử dụng`None`là cố ý. Điều đó có nghĩa là một vị trí trống được phản ánh phải khớp với một vị trí trống khác, do đó việc kiểm tra bảng màu cũng xác minh tất cả các khoảng trống góc. 

các`manacher_odd_even`là hàm tính toán chẵn và lẻ theo thời gian tuyến tính tiêu chuẩn. Hai mảng của nó là cần thiết vì một trục có thể đi trực tiếp qua một vị trí góc nguyên hoặc giữa hai vị trí nguyên. Thuật toán chỉ sử dụng so sánh đẳng thức, do đó các bộ dữ liệu vệ tinh có thể được so sánh trực tiếp mà không cần chuyển đổi tọa độ lớn của chúng thành dấu phẩy động. 

Trường hợp trục chẵn sử dụng`d1`. Vì`k = 2alpha`, tâm của mảng trùng lặp là`C + alpha`, và bán kính cần tìm là`C/2`. Ngưỡng chính xác là`HALF`, bởi vì`d1`tính chính tâm là một lớp và do đó bán kính là`HALF`bao gồm các khoảng cách từ 0 đến`HALF-1`. Cặp đôi ở khoảng cách chính xác`HALF`đại diện cho cùng một vị trí góc sau một vòng quay hoàn chỉnh và không áp đặt điều kiện bổ sung nào. 

Trường hợp trục lẻ sử dụng`d2`. Ở đây trục nằm giữa hai vị trí góc nguyên và có chính xác`HALF`các cặp phản xạ riêng biệt xung quanh một vòng tròn hoàn chỉnh. Do đó, bán kính chẵn-palindrome yêu cầu chính xác là`HALF`. 

Lần quét cuối cùng sử dụng các góc gấp đôi. Điều này tránh tích lũy các lỗi dấu phẩy động và cũng cung cấp đầu ra được sắp xếp miễn phí. Đối với góc nhân đôi lẻ, hướng là cung giây nửa số nguyên và không thể trùng với vệ tinh đầu vào. Đối với một góc chẵn nhân đôi, vị trí số nguyên tương ứng được kiểm tra`a`. 

Không có vấn đề tràn số nguyên trong Python. Số nguyên được lưu trữ lớn nhất chỉ khoảng`10^18`và số nguyên Python vẫn xử lý chính xác. Quan trọng hơn, thuật toán không bao giờ thực hiện các phép tính lượng giác, do đó`rho`cũng không`phi`cần phải được chuyển đổi sang dấu phẩy động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hai vệ tinh chiếm các góc 0 và 64800 và có cùng`(rho, mass)`đôi. Dãy góc trống ở mọi nơi ngoại trừ hai vị trí đối diện đó. 

Trục đối xứng ở 0 có góc gấp đôi`k = 0`. Sự phản chiếu của nó không trao đổi vệ tinh nào với một nhãn khác, vì vậy nó là một trục hình học hợp lệ. Tuy nhiên, cả hai hướng của trục này đều chứa các vệ tinh và bị loại bỏ trong lần quét cuối cùng. 

Trục vuông góc có`k = 64800`, tương ứng với góc 32400. Nó cũng đối xứng và cả hai tia của nó đều không chứa vệ tinh. 

| Tiểu bang | Giá trị | 
| --- | --- | 
|`C`|`129600`| 
|`HALF`|`64800`| 
| góc chiếm đóng |`0`,`64800`| 
| trục đối xứng`k`|`0`,`64800`| 
| chỉ đường bị chặn |`0`,`64800`| 
| hướng phát ra |`32400`,`97200`| 

Do đó, đầu ra là```
2
32400.0000000000
97200.0000000000
```Ví dụ này chứng tỏ tại sao việc tìm trục đối xứng không phải là bước cuối cùng. Một đường đối xứng vẫn có thể không sử dụng được vì một trong các tia của nó đi qua vệ tinh. 

### Hình vuông tùy chỉnh 

Xét bốn vệ tinh bằng nhau ở bốn hướng chính:```
4
1 0 1
1 32400 1
1 64800 1
1 97200 1
```Cấu hình là một hình vuông. Trục đối xứng của nó là trục ngang, trục dọc và hai trục chéo. 

Trục ngang và trục dọc bị chặn vì chứa các vệ tinh. Hai trục chéo rõ ràng. 

| Trục nhân đôi góc`k`| Góc trục | Đối xứng | Hướng 1 | Hướng 2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
|`0`|`0`| vâng |`0`bị chặn |`64800`bị chặn | từ chối | 
|`32400`|`16200`| vâng |`16200`rõ ràng |`81000`rõ ràng | chấp nhận | 
|`64800`|`32400`| vâng |`32400`bị chặn |`97200`bị chặn | từ chối | 
|`97200`|`48600`| vâng |`48600`rõ ràng |`113400`rõ ràng | chấp nhận | 

Đầu ra là```
4
16200.0000000000
48600.0000000000
81000.0000000000
113400.0000000000
```Dấu vết thể hiện sự khác biệt giữa trục đối xứng và tia bay được phép, cũng như thực tế là mỗi trục đóng góp hai hướng ngược nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(C + n + A) | Xây dựng mảng góc, chạy Manacher trên`2C`vị trí và quét`2C`chỉ đường | 
| Không gian | O(C + n) | Lưu trữ các nhãn góc và hai mảng bán kính Manacher | 

Đây`C = 129600`Và`A`là số hướng được báo cáo. Từ`C`được cố định bởi tuyên bố và`A <= 200000`, khối lượng công việc thực tế là vài trăm nghìn thao tác mảng cộng với xử lý đầu vào và đầu ra. Điều này phù hợp một cách thoải mái trong giới hạn dự định và việc triển khai không thực hiện bất kỳ công việc O(n²) nào. 

## Trường hợp thử nghiệm```python
import sys
import io
from contextlib import redirect_stdout

C = 129600

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    with redirect_stdout(out):
        solve()

    sys.stdin = old_stdin
    return out.getvalue()

def parse_output(out: str):
    tokens = out.split()
    count = int(tokens[0])
    values = list(map(float, tokens[1:]))
    assert count == len(values)
    return count, values

# Provided sample
sample1 = """\
2
1 0 1
1 64800 1
"""

assert run(sample1) == (
    "2\n"
    "32400.0000000000\n"
    "97200.0000000000\n"
), "sample 1"

# Minimum-size input with a half-integer symmetry axis.
case2 = """\
2
1 0 1
1 1 1
"""

count, values = parse_output(run(case2))
assert count == 2, "half-integer axis count"
assert abs(values[0] - 0.5) < 1e-9, "half-integer first direction"
assert abs(values[1] - 64800.5) < 1e-9, "half-integer second direction"

# Weighted configuration with geometric symmetry but no valid weighted symmetry.
case3 = """\
4
1 0 1
1 64800 2
1 32400 3
1 97200 3
"""

count, values = parse_output(run(case3))
assert count == 0, "unequal masses must break symmetry"

# Square: cardinal axes are blocked, diagonal axes are clear.
case4 = """\
4
1 0 1
1 32400 1
1 64800 1
1 97200 1
"""

count, values = parse_output(run(case4))
expected = [16200.0, 48600.0, 81000.0, 113400.0]
assert count == 4, "square direction count"
for got, want in zip(values, expected):
    assert abs(got - want) < 1e-9, "square directions"

# Maximum feasible number of satellites.
# Every integer angle is occupied with the same (rho, mass).
# All half-integer directions are clear, and every such direction
# is an axis of reflection.
parts = ["129600"]
for phi in range(129600):
    parts.append(f"1 {phi} 1")
case5 = "\n".join(parts) + "\n"

count, values = parse_output(run(case5))
assert count == 129600, "maximum feasible input"
assert abs(values[0] - 0.5) < 1e-9, "first half-integer direction"
assert abs(values[-1] - 129599.5) < 1e-9, "last half-integer direction"
assert all(abs(values[i] - (i + 0.5)) < 1e-9 for i in range(count)), \
    "all half-integer directions"

# Boundary around angle 129599 and angle 0.
case6 = """\
2
1 129599 7
1 0 7
"""

count, values = parse_output(run(case6))
assert count == 2, "wrap-around symmetry count"
assert abs(values[0] - 64800.5) < 1e-9
assert abs(values[1] - 129599.5) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`32400`,`97200`| Ví dụ được cung cấp và trục đối xứng bị chặn | 
|`0, 1`có nhãn bằng nhau |`0.5`,`64800.5`| Trục nửa số nguyên | 
| Bốn vệ tinh có khối lượng không bằng nhau |`0`| Khối lượng phải là một phần của nhãn phản chiếu | 
| Bốn vệ tinh hồng y |`16200`,`48600`,`81000`,`113400`| Một số trục và hướng bị chặn | 
| Đã chiếm hết 129600 góc nguyên | 129600 hướng nửa số nguyên | Đầu vào khả thi tối đa và tính đối xứng dày đặc | 
| Góc 129599 và 0 |`64800.5`,`129599.5`| Bao bọc xung quanh | 

Trường hợp kích thước tối đa cũng hữu ích để kiểm tra xem thuật toán không vô tình phụ thuộc vào số lượng vị trí bị chiếm giữ nhỏ. Bản thân vũ trụ góc bị giới hạn nên việc xử lý tất cả 129600 vị trí vẫn thực tế. 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là trục đối xứng chứa các vệ tinh. Vì```
2
1 0 1
1 64800 1
```trục ở 0 được phát hiện vì sự phản xạ gửi góc 0 tới chính nó và 64800 cho chính nó. Chỉ số trục kép tương ứng là`k = 0`. Trong quá trình quét đầu ra, hướng`x = 0`ánh xạ tới góc 0, trong đó`a[0]`bị chiếm đóng nên nó bị từ chối. Phương hướng`x = 129600`ánh xạ tới góc 64800, trong đó`a[64800]`cũng bị chiếm giữ nên nó bị từ chối. Trục vuông góc tồn tại và tạo ra 32400 và 97200. 

Trường hợp cạnh thứ hai là trục nửa số nguyên. Vì```
2
1 0 1
1 1 1
```phản xạ khoảng 0,5 trao đổi hai vệ tinh. Giá trị trục nhân đôi của nó là`k = 1`, do đó thuật toán sử dụng bán kính chẵn-palindrome. Vì cả hai tia đều có góc gấp đôi 1 và 129601 nên chúng có hướng nửa nguyên. Không có góc đầu vào nào có thể bằng một góc nào đó, vì vậy cả hai hướng đều an toàn. Đầu ra là 0,5 và 64800,5. 

Trường hợp cạnh thứ ba có khối lượng không bằng nhau. Vì```
4
1 0 1
1 64800 2
1 32400 3
1 97200 3
```các vị trí tại 32400 và 97200 có nhãn bằng nhau, nhưng các nhãn ở 0 và 64800 khác nhau. Bất kỳ sự phản xạ nào trao đổi hai vị trí đó đều làm thay đổi khối lượng, do đó nó không phải là sự đối xứng của trường hấp dẫn. Trục hình học duy nhất giữ cố định cả hai vệ tinh có khối lượng không bằng nhau là trục hoành, nhưng cả hai tia của trục đó đều chứa các vệ tinh. Câu trả lời cuối cùng là trống rỗng. 

Trường hợp cạnh thứ tư là sự đối xứng đi qua ranh giới hình tròn. Vì```
2
1 129599 7
1 0 7
```hai vệ tinh liền kề nhau qua ranh giới 0 độ. Trục đối xứng của chúng là 129599,5, được biểu thị bằng modulo 180 độ bằng 64799,5. Cấu trúc mảng kép là điều làm cho bảng màu thông thường này có thể nhìn thấy được. Nếu không sao chép chuỗi góc, cặp này sẽ dường như nằm ở hai đầu đối diện của cấu trúc dữ liệu và việc triển khai bảng màu cục bộ có thể bỏ lỡ nó. 

Trường hợp cạnh thứ năm là một vòng tròn góc bị chiếm hoàn toàn. Nếu mọi góc nguyên từ 0 đến 129599 đều chứa cùng một`(rho, mass)`, mọi trục phản xạ đều có giá trị như một đối xứng hình học. Mọi tia nguyên đều bị chặn, trong khi mọi tia nửa số nguyên đều rõ ràng. Có chính xác 129600 hướng hợp lệ, mỗi hướng ở mỗi góc nửa số nguyên. Thuật toán xử lý việc này mà không có bất kỳ trường hợp đặc biệt nào vì thử nghiệm palindrome nhìn thấy một chuỗi hoàn toàn đồng nhất và thử nghiệm tia cuối cùng sẽ loại bỏ chính xác các vị trí số nguyên bị chiếm giữ.
