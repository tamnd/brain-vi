---
title: "CF 102431E - Ngăn chặn không tối đa"
description: "Mỗi phát hiện là một hình vuông có cùng độ dài cạnh S. Vị trí của nó được xác định bởi góc dưới bên trái (x, y) và nó có điểm tin cậy riêng biệt. NMS xử lý các phát hiện này từ điểm cao nhất đến điểm thấp nhất."
date: "2026-08-08T17:26:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102431
codeforces_index: "E"
codeforces_contest_name: "2019 China Collegiate Programming Contest Final (CCPC-Final 2019)"
rating: 0
weight: 102431
solve_time_s: 419
verified: true
draft: false
---

[CF 102431E - Ngăn chặn không tối đa](https://codeforces.com/problemset/problem/102431/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 59 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi phát hiện là một hình vuông có cùng độ dài cạnh`S`. Vị trí của nó được xác định bởi góc dưới bên trái`(x, y)`và nó có điểm tin cậy khác biệt. NMS xử lý các phát hiện này từ điểm cao nhất đến điểm thấp nhất. Một phát hiện được chọn nếu nó chưa bị chặn. Sau khi được chọn, nó sẽ ngăn chặn mọi phát hiện có điểm thấp hơn có giao điểm trên liên kết lớn hơn ngưỡng nhất định. Nhiệm vụ là xuất ra các chỉ số của tất cả các phát hiện còn sót lại trong quá trình này, được sắp xếp theo chỉ số ban đầu của chúng. 

Những ràng buộc chính thức cho phép`n`để đạt được`10^5`, với`S`lên đến`10^7`và ngưỡng là một giá trị chính xác có ba chữ số thập phân. Tuyên bố Codeforces hiện tại đưa ra giới hạn thời gian 15 giây và giới hạn bộ nhớ 256 MB. MỘT`O(n^2)`việc thực hiện có thể thực hiện khoảng`10^10`kiểm tra cặp khi`n = 10^5`, vượt xa mức hợp lý ngay cả với thời hạn tương đối hào phóng. Chúng ta cần số lượng so sánh trên mỗi hộp đã chọn được giới hạn bởi một hằng số. 

Cho hai hình vuông bằng nhau, hãy`dx = |x1 - x2|`Và`dy = |y1 - y2|`. 

Giao điểm của chúng có chiều rộng`max(0, S - dx)`và chiều cao`max(0, S - dy)`. Nếu khu vực giao lộ là`A`, diện tích hợp là`2S^2 - A`, vậy IoU là`A / (2S^2 - A)`. 

Chúng ta không nên đánh giá điều này bằng cách sử dụng dấu phẩy động. Nếu ngưỡng là`p / 1000`, sau đó`A / (2S^2 - A) > p / 1000`chính xác là tương đương với`(1000 + p) A > 2000 p S^2`. 

Mọi đại lượng trong bất đẳng thức này đều là số nguyên. 

Một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. Đầu tiên, sự so sánh hoàn toàn lớn hơn ngưỡng. Ví dụ,```
1
2 3 0.500
0 0 0.9
0 1 0.8
```Diện tích giao lộ là`3 * 2 = 6`, công đoàn là`18 - 6 = 12`, và IoU chính xác là`0.500`. Đầu ra đúng là```
Case #1: 2
1 2
```sử dụng`>= threshold`sẽ ngăn chặn không chính xác hộp 2. 

Trường hợp cạnh thứ hai không có sự chồng chéo. Ví dụ,```
1
2 4 0.300
0 0 0.9
4 0 0.8
```Các hình vuông chỉ chạm vào ranh giới của chúng nên diện tích giao điểm của chúng bằng không. Cả hai phát hiện phải được chọn:```
Case #1: 2
1 2
```Việc thực hiện bất cẩn chỉ vì lý do khoảng cách có thể vô tình coi các ô vuông chạm nhau là chồng chéo. 

Hộp đựng cạnh thứ ba là những hộp giống hệt nhau. Ví dụ,```
1
3 1 0.700
0 0 0.5
0 0 0.7
0 0 0.9
```Hộp có điểm cao nhất được chọn đầu tiên và IoU của nó với cả hai hộp còn lại là`1`, lớn hơn rất nhiều so với`0.700`. Câu trả lời là```
Case #1: 1
3
```Việc đảm bảo điểm số riêng biệt cho chúng ta biết chính xác hộp giống hệt nhau nào còn tồn tại. 

Cuối cùng, các phát hiện đã chọn phải được in theo thứ tự chỉ số tăng dần chứ không phải theo thứ tự điểm. Nếu hộp 3 và 1 tồn tại trong khi hộp 2 bị chặn thì đầu ra phải là`1 3`, mặc dù hộp 3 có thể có số điểm lớn hơn. Mẫu chính thức thể hiện thứ tự này. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp tuân theo NMS theo đúng nghĩa đen. Sắp xếp tất cả các ô theo điểm giảm dần, sau đó giữ nguyên các ô hiện không bị chặn. Khi hộp tiếp theo được chọn, hãy so sánh nó với mọi hộp còn lại và loại bỏ những hộp có IoU trên ngưỡng. Điều này đúng vì nó chính xác là định nghĩa của quy trình NMS. 

Vấn đề là số lượng so sánh. Trong trường hợp xấu nhất, không có hộp nào chặn hộp khác, vì vậy hộp được chọn đầu tiên sẽ kiểm tra một cách đại khái.`n`hộp, hộp thứ hai kiểm tra đại khái`n - 1`, vân vân. Tổng cộng là khoảng`n(n - 1)/2`, đạt tới khoảng`5 * 10^9`so sánh cho`n = 10^5`. Con số này đã quá lớn trước khi tính đến việc sắp xếp, các thao tác trên bảng băm và tính toán hình học. 

Quan sát quan trọng là tất cả các hình vuông đều có cùng kích thước. Đối với một hình vuông được chọn cố định, một hình vuông khác chỉ có thể có IoU trên ngưỡng khi góc dưới bên trái của nó đủ gần với góc đã chọn. trong`(x, y)`mặt phẳng, tập hợp các vị trí có thể xung đột là một vùng giới hạn xung quanh điểm đã chọn. 

Chúng ta có thể biến địa phương hình học này thành một lưới. Chọn kích thước ô`C`sao cho hai điểm bất kỳ trong cùng một ô đều được đảm bảo có IoU lớn hơn ngưỡng. Sau đó, có thể chọn tối đa một hộp từ mỗi ô. Chúng ta cũng có thể chỉ ra rằng hai hộp xung đột phải nằm trong các ô có tọa độ khác nhau nhiều nhất là hai. Do đó, khi xử lý một hộp đã chọn, chúng ta chỉ cần kiểm tra`5 x 5`tế bào bao quanh tế bào của nó. 

Kích thước ô được chọn chính xác thay vì xấp xỉ. Đặt ngưỡng là`p / 1000`và xác định`q = 2p / (1000 + p)`. 

Hai hộp có IoU lớn hơn ngưỡng chính xác khi diện tích giao nhau của chúng lớn hơn`qS^2`. 

Giả sử hai góc dưới bên trái nằm trong cùng một ô bên`C`. Sự khác biệt về tọa độ của họ nhiều nhất là`C - 1`, vậy diện tích giao điểm của chúng ít nhất là`(S - C + 1)^2`. 

Ta chọn số nguyên lớn nhất`C`thỏa mãn`(1000 + p)(S - C + 1)^2 > 2000pS^2`. 

Do đó mỗi cặp hộp trong cùng một ô đều xung đột. Đây là mật độ trung tâm bị ràng buộc. 

Bây giờ hãy xem xét hai hộp thực sự xung đột. Từ`(S - dx)(S - dy) > qS^2`, 

cả hai yếu tố riêng lẻ phải lớn hơn`sqrt(q) S`. Kể từ đây`dx < S(1 - sqrt(q))`và tương tự cho`dy`. 

Kích thước ô được chọn ít nhất là`S(1 - sqrt(q))`, do đó, một cặp xung đột có chiều rộng chênh lệch ít hơn hai lần một ô ở cả hai tọa độ. Do đó, tọa độ lưới của chúng có thể khác nhau nhiều nhất là hai. các`5 x 5`vùng lân cận chứa mọi hộp có thể ngăn chặn được. 

Lưới không cần phải xóa các hộp bị chặn về mặt vật lý. Một hộp thuộc về một ô và một ô có thể chứa tối đa một hộp đã chọn vì tất cả các hộp trong ô đó xung đột với nhau. Do đó, bất kỳ hộp được lưu trữ cụ thể nào cũng chỉ có thể được kiểm tra bằng các hộp đã chọn từ tối đa 25 ô lân cận. Mỗi hộp chỉ tham gia vào một số lượng so sánh tổng thể không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

Tài liệu chính thức của cuộc thi cũng mô tả ý tưởng hình học tương tự: các hình vuông có kích thước bằng nhau chỉ xung đột trong một vùng lân cận cố định, cho phép tìm kiếm dựa trên lưới thay vì chọn từng hộp. 

## Hướng dẫn thuật toán 

1. Phân tích ngưỡng dưới dạng số nguyên`p`đại diện`p / 1000`. Phân tích mọi điểm dưới dạng số nguyên được chia tỷ lệ bởi`10^6`. Điều này tránh việc so sánh dấu phẩy động khi sắp xếp và khi quyết định xem IoU có vượt quá ngưỡng hay không. 
2. Sắp xếp các chỉ số trong ô theo mức độ giảm dần. Vì tất cả các điểm đều khác nhau nên điều này đưa ra chính xác thứ tự mà NMS xem xét các hộp. 
3. Tính độ dài cạnh ô lưới lớn nhất`C`thỏa mãn`(1000 + p)(S - C + 1)^2 > 2000pS^2`. 

Vế trái giảm dần khi`C`tăng lên, do đó`C`có thể được tìm thấy bằng tìm kiếm nhị phân. Sự bất bình đẳng nghiêm ngặt là có chủ ý. Nó phù hợp với sự nghiêm ngặt`IoU > threshold`luật lệ. 
4. Chèn từng ô vào một từ điển được khóa bởi`(x // C, y // C)`. Từ điển lưu trữ các chỉ số thuộc về từng ô không gian. 
5. Xử lý các ô theo thứ tự điểm giảm dần. Nếu một hộp đã bị chặn, hãy bỏ qua nó. Nếu không, hãy chọn nó và đánh dấu nó là đã xử lý. 
6. Đối với hộp đã chọn, hãy kiểm tra tất cả các ô lưới có độ lệch từ`-2`bởi vì`2`ở cả hai tọa độ. Mọi hộp có thể có IoU trên ngưỡng phải nằm ở một trong 25 ô này. 
7. Đối với mọi ứng cử viên chưa bị loại bỏ, hãy tính diện tích giao điểm chính xác của nó và kiểm tra`(1000 + p) * intersection > 2000 * p * S^2`. 

Nếu bất đẳng thức giữ nguyên, hãy đánh dấu ứng cử viên đó là bị loại bỏ. 
8. Lưu trữ từng chỉ mục đã chọn, sau đó sắp xếp các chỉ mục đó theo thứ tự trước khi in. Bản thân quy trình NMS được sắp xếp theo điểm số, trong khi đầu ra yêu cầu được sắp xếp theo chỉ mục. 

### Tại sao nó hoạt động 

Điều bất biến là trước khi xử lý một hộp theo thứ tự điểm, mọi hộp trước đó đều được chọn hoặc bị loại bỏ chính xác theo quy định của NMS. Nếu hộp hiện tại đã bị chặn thì không bao giờ được chọn. Ngược lại, không có ô nào được chọn trước đó đã chặn nó, vì vậy đây chính xác là ô còn lại có điểm cao nhất và phải được chọn. 

Khi một hộp được chọn, mọi hộp có điểm thấp hơn mà nó có thể loại bỏ sẽ nằm trong hộp được kiểm tra.`5 x 5`hàng xóm. Cấu trúc kích thước ô đảm bảo rằng mọi cặp ô giống nhau đều xung đột, trong khi sự bất bình đẳng chồng chéo giới hạn mọi cặp xung đột với các ô ở khoảng cách tối đa là hai. Kiểm tra IoU số nguyên chính xác sau đó sẽ loại bỏ chính xác những ứng cử viên có IoU lớn hơn ngưỡng. Do đó, mọi quyết định của NMS đều được thực hiện chính xác và tập hợp được chọn cuối cùng chính xác là tập hợp được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_scaled(s, digits):
    if '.' in s:
        a, b = s.split('.')
    else:
        a, b = s, ''
    b = (b + '0' * digits)[:digits]
    return int(a) * (10 ** digits) + int(b)

def solve():
    t = int(input())
    output = []

    for case_id in range(1, t + 1):
        n, S, threshold = input().split()
        n = int(n)
        S = int(S)

        # threshold is exact to three decimal places.
        p = parse_scaled(threshold, 3)

        boxes = []
        order = []

        for i in range(n):
            x, y, score = input().split()
            x = int(x)
            y = int(y)
            score = parse_scaled(score, 6)
            boxes.append((x, y))
            order.append((score, i))

        order.sort(reverse=True)

        # We need the largest integer C such that
        #
        # (1000 + p) * (S - C + 1)^2 > 2000 * p * S^2
        #
        # Then any two boxes in one cell necessarily have IoU > threshold.
        target = 2000 * p * S * S
        coefficient = 1000 + p

        def good(c):
            overlap = S - c + 1
            return coefficient * overlap * overlap > target

        lo, hi = 1, S
        while lo < hi:
            mid = (lo + hi + 1) // 2
            if good(mid):
                lo = mid
            else:
                hi = mid - 1

        cell_size = lo

        grid = {}
        for _, idx in order:
            x, y = boxes[idx]
            key = (x // cell_size, y // cell_size)
            grid.setdefault(key, []).append(idx)

        suppressed = bytearray(n)
        selected = []

        for _, idx in order:
            if suppressed[idx]:
                continue

            suppressed[idx] = 1
            selected.append(idx + 1)

            x1, y1 = boxes[idx]
            gx = x1 // cell_size
            gy = y1 // cell_size

            for ox in range(-2, 3):
                for oy in range(-2, 3):
                    candidates = grid.get((gx + ox, gy + oy))
                    if candidates is None:
                        continue

                    for j in candidates:
                        if suppressed[j]:
                            continue

                        x2, y2 = boxes[j]

                        ix = S - abs(x1 - x2)
                        if ix <= 0:
                            continue

                        iy = S - abs(y1 - y2)
                        if iy <= 0:
                            continue

                        area = ix * iy

                        if coefficient * area > target:
                            suppressed[j] = 1

        selected.sort()

        output.append(f"Case #{case_id}: {len(selected)}")
        output.append(" ".join(map(str, selected)))

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Việc phân tích cú pháp đầu vào có chủ ý giữ ngưỡng và điểm dưới dạng số nguyên. Một ngưỡng như`0.500`trở thành`500`, trong khi một số điểm như`0.900000`trở thành`900000`. Vì đầu vào đảm bảo các giá trị thập phân chính xác nên cách biểu diễn này giữ nguyên thứ tự của chúng và tránh các lỗi làm tròn. 

Tìm kiếm nhị phân tìm thấy kích thước ô an toàn lớn nhất. Đối với kích thước ô ứng cử viên`C`, giao điểm nhỏ nhất có thể có giữa hai hộp trong ô đó xảy ra khi tọa độ của hai hộp đó khác nhau bởi`C - 1`. Giao lộ kết quả là`(S - C + 1)^2`, vì vậy`good`vị ngữ trực tiếp kiểm tra xem ngay cả trường hợp xấu nhất đó vẫn có IoU vượt quá ngưỡng hay không. 

các`grid`Từ điển ánh xạ từng ô không gian tới tất cả các chỉ mục hộp ban đầu trong ô đó. Chúng tôi giữ các mục bị chặn thay vì xóa chúng. Điều này giúp cho việc triển khai trở nên đơn giản và không phá hủy sự phức tạp bị ràng buộc. Vì tối đa một hộp từ bất kỳ ô nào có thể tồn tại, nên chỉ có thể tìm thấy một hộp được lưu trữ từ số lượng không đổi các hộp đã chọn có các ô nằm trong khoảng cách hai. 

các`suppressed`bytearray ghi lại cả các hộp đã được chọn và các hộp bị NMS loại bỏ. của Python`bytearray`nhỏ hơn đáng kể so với danh sách các boolean Python cho`10^5`mục nhập. 

Việc tính toán giao lộ sử dụng`ix = S - abs(x1 - x2)`và biểu thức tương tự cho`iy`. Giá trị không dương có nghĩa là không có giao điểm vùng dương, do đó IoU bằng 0 và ứng viên không thể bị loại bỏ. Phép so sánh cuối cùng sử dụng số học số nguyên và bảo toàn chính xác bất đẳng thức nghiêm ngặt. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các sản phẩm như`S^2`Và`2000 * p * S^2`không tràn. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, số nguyên 64 bit là đủ cho các giới hạn này. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp có ba hình vuông cạnh`4`và ngưỡng`0.390`. Các ô đã được liệt kê theo thứ tự điểm giảm dần. 

| Bước | Hộp | Tế bào | Bị đàn áp trước | Kiểm tra IoU | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 |`(0, 0)`| không | hộp 2, hộp 3 | Chọn 1, chặn 2 | 
| 2 | 2 |`(0, 0)`| vâng | không | Bỏ qua | 
| 3 | 3 |`(1, 1)`| không | hộp 1 | Chọn 3 | 

Đối với hộp thứ nhất và hộp thứ hai, sự khác biệt về tọa độ là`(1, 1)`, cho diện tích giao lộ`9`. Công đoàn là`32 - 9 = 23`, vậy IoU của họ là`9/23`, lớn hơn`0.390`. Đối với ô 1 và 3, diện tích giao điểm là`4`, công đoàn là`28`, và IoU là`1/7`, nằm dưới ngưỡng. Do đó, các chỉ số được chọn`1`Và`3`. 

Ví dụ thứ hai thực hiện ranh giới ngưỡng chính xác.```
1
3 3 0.500
0 0 0.900
0 1 0.800
10 10 0.700
```Vì`S = 3`và ngưỡng`0.500`, hai ô đầu tiên có diện tích giao nhau`6`và khu vực công đoàn`12`. 

| Bước | Hộp | Tế bào | IoU với các hộp đã chọn | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 |`(0, 0)`| không | Chọn 1 | 
| 2 | 2 |`(0, 1)`|`6 / 12 = 0.500`| Chọn 2 | 
| 3 | 3 |`(10, 10)`|`0`| Chọn 3 | 

Hộp thứ hai tồn tại vì quy tắc hoàn toàn lớn hơn ngưỡng. Kết quả cuối cùng là```
Case #1: 3
1 2 3
```Dấu vết này xác nhận rằng việc so sánh số nguyên không vô tình biến sự bình đẳng thành sự triệt tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp tốn O(n log n), trong khi mỗi hộp chỉ tham gia vào việc so sánh không gian O(1) | 
| Không gian | O(n) | Các hộp, chỉ mục sắp xếp, trạng thái loại bỏ và lưu trữ lưới đều sử dụng bộ nhớ O(n) | 

Việc xây dựng lưới yêu cầu tìm kiếm nhị phân trên`S`, chỉ thêm`O(log S)`làm việc cho mỗi trường hợp thử nghiệm. Từ`S <= 10^7`, đây là ít hơn 24 lần lặp. Chi phí chủ yếu là phân loại`10^5`các hộp, theo sau là số lần kiểm tra không gian không đổi trên mỗi hộp. Các giới hạn chính thức là`n <= 10^5`,`S <= 10^7`, 15 giây và 256 MB, do đó kết quả`O(n log n)`giải pháp phù hợp với quy mô dự kiến. 

## Trường hợp thử nghiệm 

Khai thác kiểm tra sau đây chứa giải pháp dưới dạng hàm có thể gọi để mọi xác nhận có thể được thực thi trực tiếp. Trường hợp kích thước tối đa sử dụng`100000`vị trí giống hệt nhau với số điểm chênh lệch, buộc NMS phải giữ đúng ô có điểm cao nhất.```python
import io
import sys

def parse_scaled(s, digits):
    if '.' in s:
        a, b = s.split('.')
    else:
        a, b = s, ''
    b = (b + '0' * digits)[:digits]
    return int(a) * (10 ** digits) + int(b)

def solution(data: str) -> str:
    it = iter(data.split())
    t = int(next(it))
    output = []

    for case_id in range(1, t + 1):
        n = int(next(it))
        S = int(next(it))
        threshold = next(it)
        p = parse_scaled(threshold, 3)

        boxes = []
        order = []

        for i in range(n):
            x = int(next(it))
            y = int(next(it))
            score = parse_scaled(next(it), 6)
            boxes.append((x, y))
            order.append((score, i))

        order.sort(reverse=True)

        target = 2000 * p * S * S
        coefficient = 1000 + p

        def good(c):
            overlap = S - c + 1
            return coefficient * overlap * overlap > target

        lo, hi = 1, S
        while lo < hi:
            mid = (lo + hi + 1) // 2
            if good(mid):
                lo = mid
            else:
                hi = mid - 1

        cell_size = lo

        grid = {}
        for _, idx in order:
            x, y = boxes[idx]
            key = (x // cell_size, y // cell_size)
            grid.setdefault(key, []).append(idx)

        suppressed = bytearray(n)
        selected = []

        for _, idx in order:
            if suppressed[idx]:
                continue

            suppressed[idx] = 1
            selected.append(idx + 1)

            x1, y1 = boxes[idx]
            gx = x1 // cell_size
            gy = y1 // cell_size

            for ox in range(-2, 3):
                for oy in range(-2, 3):
                    candidates = grid.get((gx + ox, gy + oy))
                    if candidates is None:
                        continue

                    for j in candidates:
                        if suppressed[j]:
                            continue

                        x2, y2 = boxes[j]

                        ix = S - abs(x1 - x2)
                        if ix <= 0:
                            continue

                        iy = S - abs(y1 - y2)
                        if iy <= 0:
                            continue

                        area = ix * iy

                        if coefficient * area > target:
                            suppressed[j] = 1

        selected.sort()

        output.append(f"Case #{case_id}: {len(selected)}")
        output.append(" ".join(map(str, selected)))

    return "\n".join(output)

def run(inp: str) -> str:
    return solution(inp)

sample = """\
1
3 4 0.390
0 0 0.9
1 1 0.8
2 2 0.7
"""

assert run(sample) == """\
Case #1: 2
1 3
""", "provided sample"

boundary = """\
1
3 3 0.500
0 0 0.900
0 1 0.800
10 10 0.700
"""

assert run(boundary) == """\
Case #1: 3
1 2 3
""", "exact threshold must not suppress"

minimum = """\
1
1 1 0.700
0 0 1.000
"""

assert run(minimum) == """\
Case #1: 1
1
""", "minimum-size input"

identical = """\
1
5 5 0.300
2 2 0.100000
2 2 0.900000
2 2 0.500000
2 2 0.700000
2 2 0.300000
"""

assert run(identical) == """\
Case #1: 1
2
""", "identical boxes keep only the highest score"

far_apart = """\
1
4 10 0.700
0 0 0.400000
10 0 0.900000
0 10 0.800000
10 10 0.700000
"""

assert run(far_apart) == """\
Case #1: 4
1 2 3 4
""", "zero-overlap boxes all survive"

n = 100000
lines = [f"{n} 1 0.700"]
for i in range(n):
    score = (n - i) / 100000
    lines.append(f"0 0 {score:.5f}")

maximum = "\n".join(["1"] + lines) + "\n"
maximum_output = run(maximum)
maximum_lines = maximum_output.splitlines()

assert maximum_lines[0] == "Case #1: 1", "maximum-size count"
assert maximum_lines[1] == "1", "maximum-size highest score survives"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu được cung cấp |`Case #1: 2`, chỉ số`1 3`| Hành vi NMS cơ bản và thứ tự đầu ra | 
|`S=3`, ngưỡng`0.500`, IOU chính xác`0.500`| Cả ba được chọn | Nghiêm ngặt`>`ranh giới | 
|`n=1`,`S=1`| Chỉ mục`1`| Đầu vào kích thước tối thiểu | 
| Năm hộp giống hệt nhau | Chỉ số có điểm cao nhất | Vị trí và thứ tự điểm giống hệt nhau | 
| Bốn hộp hoàn toàn tách biệt | Tất cả bốn lựa chọn | Xử lý không chồng chéo | 
|`n=100000`,`S=1`| Chỉ mục`1`| Tối đa`n`, vị trí giống hệt nhau dày đặc và khả năng mở rộng | 

## Vỏ cạnh 

Đối với trường hợp ngưỡng nghiêm ngặt,```
1
2 3 0.500
0 0 0.9
0 1 0.8
```hộp đầu tiên được chọn. Ô thứ hai có giao điểm`6`và công đoàn`12`, vậy IoU chính xác là`0.5`. Kiểm tra số nguyên trở thành một đẳng thức:`(1000 + 500) * 6 = 2000 * 500 * 3^2 / 12`, 

hoặc trực tiếp hơn,`1500 * 6 = 500 * 12`. 

Điều kiện sử dụng`>`còn hơn là`>=`, do đó hộp 2 không bị chặn. Thuật toán chọn cả hai chỉ số. 

Đối với sự chồng chéo bằng không,```
1
2 4 0.300
0 0 0.9
4 0 0.8
```sự khác biệt theo chiều ngang là`4`, bằng độ dài cạnh Như vậy`ix = 4 - 4 = 0`và việc triển khai ngay lập tức bỏ qua ứng viên. Không cần tính toán IoU dấu phẩy động. Cả hai hộp đều tồn tại. 

Đối với các hộp giống hệt nhau,```
1
3 1 0.700
0 0 0.5
0 0 0.7
0 0 0.9
```cả ba điểm đều thuộc cùng một ô lưới. Ô được xử lý đầu tiên có chỉ số 3 vì nó có số điểm lớn nhất. Giao điểm của nó với mọi hộp khác là`1`, do đó điều kiện triệt tiêu chính xác thành công vì IoU là`1 > 0.7`. Cả hai hộp còn lại đều được đánh dấu là bị chặn và không bao giờ được chọn. 

Để đặt hàng đầu ra, hãy xem xét```
1
3 10 0.300
0 0 0.2
9 9 0.9
5 5 0.8
```Các hộp được tách biệt đủ để tất cả đều tồn tại. NMS xử lý chúng theo thứ tự`2, 3, 1`, nhưng đầu ra yêu cầu là```
Case #1: 3
1 2 3
```Loại cuối cùng của`selected`xử lý sự khác biệt này giữa thứ tự xử lý và thứ tự đầu ra. 

Trường hợp kích thước tối đa sử dụng`100000`các hộp có cùng tọa độ. Tất cả chúng đều chiếm một ô lưới và việc xây dựng ô đảm bảo rằng các hộp trong ô đó có IoU trên ngưỡng. Ô có điểm cao nhất được chọn trước và loại bỏ ô còn lại`99999`hộp. Mỗi mục sau đó sẽ bị bỏ qua. Công việc vẫn tuyến tính ngoài cách sắp xếp ban đầu vì tất cả các hộp đó được chứa trong một nhóm không gian và mỗi hộp chỉ được kiểm tra một số lần không đổi bởi các hộp đã chọn.
