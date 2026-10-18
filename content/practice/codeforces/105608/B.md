---
title: "CF 105608B - \u0414\u0432\u0435 \u043c\u0438\u0448\u0435\u043d\u0438"
description: "Chúng ta được giao hai mục tiêu hình tròn trên một mặt phẳng. Mỗi mục tiêu được xác định bởi tọa độ trung tâm và bán kính của nó. Từ hai vòng tròn này, chúng ta được yêu cầu xác định xem có thể đạt được điểm nào trong số bốn “điểm” từ 0 đến 3, trong đó mỗi điểm tương ứng với mối quan hệ hình học giữa hai…"
date: "2026-06-22T05:50:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105608
codeforces_index: "B"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421, \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440 2024-2025"
rating: 0
weight: 105608
solve_time_s: 50
verified: true
draft: false
---

[CF 105608B - \u0414\u0432\u0435 \u043c\u0438\u0448\u0435\u043d\u0438](https://codeforces.com/problemset/problem/105608/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao hai mục tiêu hình tròn trên một mặt phẳng. Mỗi mục tiêu được xác định bởi tọa độ trung tâm và bán kính của nó. Từ hai vòng tròn này, chúng ta được yêu cầu xác định xem có thể đạt được điểm nào trong số bốn “điểm” từ 0 đến 3, trong đó mỗi điểm tương ứng với mối quan hệ hình học giữa hai vòng tròn. 

Vòng tròn đầu tiên có thể đóng góp điểm 1 nếu nó không nằm hoàn toàn bên trong vòng tròn thứ hai. Đối xứng, vòng tròn thứ hai có thể đóng góp điểm 2 nếu nó không nằm hoàn toàn bên trong vòng tròn thứ nhất. Có thể đạt điểm 3 nếu hai vòng tròn giao nhau hoặc chạm vào nhau. Theo định nghĩa, điểm 0 luôn có thể xảy ra, vì vậy nó luôn được bao gồm trong tập hợp câu trả lời. 

Đại lượng quan trọng là khoảng cách giữa tâm của các vòng tròn. Câu lệnh định nghĩa nó là khoảng cách Euclide, nhưng mã được cung cấp sử dụng khoảng cách bình phương dưới căn bậc hai, tính toán hiệu quả khoảng cách thực tế ở dạng dấu phẩy động. 

Mặc dù vấn đề có vẻ liên quan đến nhiều điều kiện nhưng kết quả đầu ra chỉ là tập hợp con của tập hợp {0, 1, 2, 3}, được in theo thứ tự tăng dần. 

Các ràng buộc đủ nhỏ để mỗi trường hợp thử nghiệm độc lập và có thể được giải quyết trong thời gian không đổi. Việc tính toán chỉ bao gồm một vài phép tính số học, vì vậy giải pháp mong muốn phải chạy ở O(1) cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh chính xuất phát từ các điều kiện biên hình học. Các đường tròn tiếp tuyến chính xác có thể dễ dàng đảo ngược các điều kiện tùy thuộc vào việc bất đẳng thức là chặt chẽ hay không chặt chẽ. Một trường hợp tinh tế khác xảy ra khi một đường tròn hoàn toàn nằm bên trong đường tròn kia nhưng ranh giới của chúng vẫn chạm vào đúng một điểm. Trong cấu hình như vậy, các điều kiện ngăn chặn và điều kiện giao nhau tương tác theo cách có thể gây nhầm lẫn cho việc triển khai không chính xác nếu sử dụng bất đẳng thức nghiêm ngặt không chính xác. 

Ví dụ: nếu vòng tròn 1 hoàn toàn nằm trong vòng tròn 2 thì không nên đưa điểm 1 vào. Một phép kiểm tra ngây thơ như chỉ so sánh bán kính sẽ thất bại khi các tâm được bù lại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực ở đây không thực sự mang tính tổ hợp vì không có cấu trúc lớn để tìm kiếm. Thay vào đó, ý tưởng ngây thơ là diễn giải từng điều kiện về mặt hình học theo nghĩa đen: để ngăn chặn, chúng ta có thể thử kiểm tra xem mọi điểm của một đường tròn có nằm bên trong một đường tròn khác hay không, và đối với giao điểm, chúng ta có thể cố gắng suy luận về các vùng chồng lấp bằng cách sử dụng các công thức hình học rút ra từ các nguyên tắc đầu tiên. 

Cách tiếp cận như vậy sẽ nhanh chóng trở nên phức tạp và dễ xảy ra lỗi một cách không cần thiết. Việc kiểm tra ngăn chặn từng điểm một là không thể trong hình học liên tục, và thậm chí việc rời rạc hóa các điểm cũng sẽ vô nghĩa và không hiệu quả. Việc giải thích bạo lực không thành công vì hình học cung cấp các điều kiện khoảng cách dạng đóng thay thế mọi nhu cầu lấy mẫu hoặc liệt kê. 

Cái nhìn sâu sắc quan trọng là tất cả các điều kiện bắt buộc giảm xuống còn các so sánh chỉ liên quan đến khoảng cách giữa tâm và bán kính. Khi chúng tôi tính toán khoảng cách đó, mỗi điểm trong số ba điểm không tầm thường sẽ tương ứng với một bất đẳng thức đơn giản. 

Việc ngăn chặn vòng tròn 1 trong vòng tròn 2 phụ thuộc vào việc điểm xa nhất của vòng tròn 1 có còn nằm trong vòng tròn 2 hay không, điều này giúp kiểm tra xem khoảng cách giữa các tâm cộng với bán kính của vòng tròn 1 có vượt quá bán kính của vòng tròn 2 hay không. Logic tương tự được áp dụng đối xứng cho điều kiện ngăn chặn ngược. 

Giao điểm phụ thuộc vào việc tổng bán kính ít nhất có bằng khoảng cách giữa các tâm hay không. Điều này nắm bắt cả giao điểm và tiếp tuyến thích hợp. 

Do đó, toàn bộ vấn đề tập trung vào việc đánh giá ba bất đẳng thức độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lý luận/lấy mẫu hình học thô bạo | O(vô hạn / không thực tế) | O(1) | Không áp dụng | 
| Kiểm tra công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi tính toán khoảng cách Euclide giữa tâm của hai đường tròn và sau đó đánh giá ba điều kiện hình học độc lập. 

1. Đọc tọa độ và bán kính của cả hai đường tròn. Điều này xác định cấu hình hình học đầy đủ. 
2. Tính khoảng cách giữa các tâm bằng công thức Euclide. Giá trị duy nhất này xác định đầy đủ tất cả các mối quan hệ không gian giữa các vòng tròn. 
3. Bắt đầu bộ câu trả lời bằng 0, vì nó luôn được bao gồm bất kể hình học. 
4. Kiểm tra xem vòng tròn 1 có nằm hoàn toàn trong vòng tròn 2 hay không bằng cách kiểm tra xem khoảng cách cộng với bán kính của vòng tròn 1 có lớn hơn bán kính của vòng tròn 2 hay không. Nếu đúng, hãy thêm 1 vào câu trả lời. 
5. Kiểm tra điều kiện đối xứng để biết vòng tròn 2 không chứa đầy đủ trong vòng tròn 1. Nếu đúng, hãy thêm 2 vào câu trả lời. 
6. Kiểm tra xem các vòng tròn có giao nhau hay chạm nhau hay không bằng cách xác minh rằng khoảng cách giữa các tâm tối đa bằng tổng bán kính. Nếu đúng, hãy bao gồm 3 trong câu trả lời. 
7. Xuất ra tất cả các giá trị thu thập được theo thứ tự tăng dần. 

Lý do đằng sau mỗi điều kiện hoàn toàn mang tính hình học. Để ngăn chặn, chúng tôi đo điểm xa nhất có thể của một vòng tròn so với tâm kia. Đối với giao lộ, chúng tôi so sánh khoảng cách giữa các tâm và khoảng cách các vòng tròn mở rộng ra bên ngoài. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là mọi mối quan hệ giữa hai đường tròn đều được xác định đầy đủ bởi ba đại lượng: hai bán kính và khoảng cách giữa các tâm. Mọi điều kiện trong phát biểu bài toán có thể được viết lại dưới dạng bất đẳng thức trực tiếp bao gồm các giá trị này. Vì những bất bình đẳng này vừa cần thiết vừa đủ nên việc đánh giá chúng một cách độc lập không thể bỏ sót bất kỳ điểm hợp lệ nào hoặc bao gồm bất kỳ điểm không hợp lệ nào. Tính độc lập của các điều kiện đảm bảo không có sự phụ thuộc tiềm ẩn giữa các lần kiểm tra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x1, y1, r1 = map(int, input().split())
x2, y2, r2 = map(int, input().split())

dx = x1 - x2
dy = y1 - y2
d = (dx * dx + dy * dy) ** 0.5

ans = [0]

if r1 + d > r2:
    ans.append(1)

if r2 + d > r1:
    ans.append(2)

if d <= r1 + r2:
    ans.append(3)

print(*ans)
```Việc thực hiện tuân theo các dẫn xuất hình học trực tiếp. Việc tính toán khoảng cách được thực hiện bằng cách sử dụng sai phân bình phương và căn bậc hai, phù hợp với số liệu Euclide dự kiến. Mỗi điều kiện được kiểm tra độc lập và các kết quả được tự động thêm vào theo thứ tự tăng dần vì chúng tôi bắt đầu từ 0 và đánh giá các điều kiện theo thứ tự nhãn tăng dần. 

Một chi tiết triển khai tinh tế là việc sử dụng số học dấu phẩy động cho khoảng cách. Mặc dù so sánh khoảng cách bình phương có thể tránh được các vấn đề về độ chính xác của dấu phẩy động, nhưng giải pháp dự kiến ​​của bài toán sử dụng dạng căn bậc hai, do đó, điều này vẫn đúng trong các ràng buộc thông thường. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 1
3 0 1
```Ở đây các trung tâm cách nhau 3 đơn vị. 

| Bước | d | r1 + d > r2 | r2 + d > r1 | d ≤ r1 + r2 | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 3 | - | - | - | [0] | 
| Kiểm tra 1 | 3 | 1 + 3 > 1 → có | - | - | [0, 1] | 
| Kiểm tra 2 | 3 | - | 1 + 3 > 1 → có | - | [0, 1, 2] | 
| Kiểm tra 3 | 3 | - | - | 3 2 → không | [0, 1, 2] | 

Điều này cho thấy một cấu hình rời rạc trong đó không vòng tròn nào chứa vòng tròn kia nhưng chúng quá xa nhau để có thể giao nhau. 

### Ví dụ 2 

đầu vào:```
0 0 5
1 0 1
```Ở đây vòng tròn nhỏ hơn nằm bên trong vòng tròn lớn hơn. 

| Bước | d | r1 + d > r2 | r2 + d > r1 | d ≤ r1 + r2 | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 1 | - | - | - | [0] | 
| Kiểm tra 1 | 1 | 5 + 1 > 1 → có | - | - | [0, 1] | 
| Kiểm tra 2 | 1 | - | 1 + 1 > 5 → không | - | [0, 1] | 
| Kiểm tra 3 | 1 | - | - | 1 6 → có | [0, 1, 3] | 

Điều này xác nhận rằng sự ngăn chặn là không đối xứng và giao điểm vẫn giữ nguyên ngay cả khi một vòng tròn nằm hoàn toàn bên trong một vòng tròn khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số lượng phép tính và so sánh số học không đổi được thực hiện | 
| Không gian | O(1) | Chỉ sử dụng danh sách câu trả lời có kích thước cố định | 

Việc tính toán có thời gian không đổi cho mỗi trường hợp thử nghiệm, dễ dàng đáp ứng các ràng buộc lập trình cạnh tranh điển hình ngay cả đối với đầu vào lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import sqrt

    x1, y1, r1 = map(int, input().split())
    x2, y2, r2 = map(int, input().split())

    dx = x1 - x2
    dy = y1 - y2
    d = (dx * dx + dy * dy) ** 0.5

    ans = [0]

    if r1 + d > r2:
        ans.append(1)

    if r2 + d > r1:
        ans.append(2)

    if d <= r1 + r2:
        ans.append(3)

    return " ".join(map(str, ans))

# provided samples (conceptual placeholders since original statement omits them)
assert run("0 0 1\n3 0 1") == "0 1 2", "disjoint circles"

# touching circles
assert run("0 0 1\n2 0 1") == "0 1 2 3", "tangent case"

# full containment
assert run("0 0 5\n1 0 1") == "0 1 3", "inner circle"

# identical circles
assert run("0 0 2\n0 0 2") == "0 3", "coincident circles"

# far apart
assert run("0 0 1\n10 0 1") == "0 1 2", "no intersection"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đường tròn tiếp tuyến | 0 1 2 3 | giao lộ ranh giới đúng đắn | 
| trường hợp ngăn chặn | 0 1 3 | logic bao hàm bất đối xứng | 
| vòng tròn giống hệt nhau | 0 3 | trường hợp cạnh chồng lên nhau đầy đủ | 
| vòng tròn xa xôi | 0 1 2 | tách không giao nhau | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là tiếp tuyến, trong đó các vòng tròn chạm nhau tại đúng một điểm. Ví dụ: các vòng tròn có khoảng cách tâm bằng r1 + r2 vẫn được tính là giao nhau. Điều kiện d ≤ r1 + r2 bao gồm chính xác trường hợp này và thuật toán cộng điểm 3. 

Một trường hợp tinh vi khác là sự trùng lặp hoàn toàn trong đó cả hai đường tròn đều có tâm và bán kính giống hệt nhau. Trong tình huống đó, mỗi vòng tròn chỉ được chứa hoàn toàn trong vòng tròn kia theo nghĩa suy biến, nhưng bất đẳng thức r + d > r không theo cả hai hướng, do đó điểm 1 và 2 bị loại trừ. Đồng thời, giao điểm vẫn giữ nguyên vì khoảng cách bằng 0, nhỏ hơn hoặc bằng tổng bán kính. 

Trường hợp thứ ba là ngăn chặn chặt chẽ bằng sự tách biệt, trong đó một vòng tròn nằm hoàn toàn bên trong một vòng tròn khác nhưng không chạm vào ranh giới. Ví dụ: một vòng tròn lớn có tâm ở (0,0) với bán kính 10 và một vòng tròn nhỏ ở (1,0) với bán kính 1 tạo ra d = 1. Điều kiện r1 + d > r2 phát hiện chính xác sự bất đối xứng, đảm bảo chỉ đưa vào điểm thích hợp trong khi vẫn đánh dấu giao điểm là đúng.
