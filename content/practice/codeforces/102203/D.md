---
title: "CF 102203D - \u041f\u0440\u043e \u043a\u044b\u0440\u0442"
description: "Chúng ta có một điểm cố định (O), đại diện cho nhà máy và (n) điểm thu gom trong mặt phẳng. Chúng ta cần chọn bốn điểm thu thập và chia chúng thành hai cặp. Đoạn nối các điểm của mỗi cặp phải đi qua (O) và bốn điểm phải nằm trên một đường tròn."
date: "2026-08-18T00:42:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "D"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 361
verified: true
draft: false
---

[CF 102203D - \u041f\u0440\u043e \u043a\u044b\u0440\u0442](https://codeforces.com/problemset/problem/102203/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 1 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một điểm cố định (O), đại diện cho nhà máy và (n) điểm thu gom trong mặt phẳng. Chúng ta cần chọn bốn điểm thu thập và chia chúng thành hai cặp. Đoạn nối các điểm của mỗi cặp phải đi qua (O) và bốn điểm phải nằm trên một đường tròn. 

Vì mỗi đường đi qua (O) chứa tối đa hai điểm tập hợp nên khi chúng ta chọn được một cặp hợp lệ thì hai điểm đó phải nằm đối diện nhau của (O). Do đó, mỗi cặp hữu ích tương ứng với một dòng đi qua (O) chứa chính xác hai điểm thu thập. 

Tọa độ của (O) và tất cả các điểm thu thập là số nguyên có giá trị tuyệt đối tối đa (10^4), trong khi (n) tối đa là (2000). Một giải pháp (O(n^2)) sẽ thực hiện khoảng bốn triệu lần lặp, điều này khá thoải mái trong Python. Giải pháp (O(n^3)) hoặc (O(n^4)) không thực tế trong giới hạn một giây. Do đó, mục tiêu chính là giảm việc tìm kiếm về cơ bản chỉ còn một lần vượt qua các điểm. 

Có một số chi tiết hình học có thể khiến việc triển khai dường như đúng không thành công. Một cặp điểm trên cùng một tia từ (O) không tạo thành một đoạn chứa (O). Ví dụ,```
0 0
4
1 0
2 0
0 1
0 -2
```có hai điểm trên tia dương (x), nên hai điểm đầu tiên không thể là một cặp hợp lệ. Cặp hợp lệ duy nhất là ((0,1),(0,-2)), không đủ để tạo thành bốn điểm cần thiết, vì vậy câu trả lời là`NO`. Việc triển khai chỉ nhóm các điểm theo độ dốc vô hướng và quên mất phía nào của (O) mà chúng chiếm giữ có thể coi hai điểm đầu tiên là một cặp không chính xác. 

Một điểm trùng với (O) cũng phải được xử lý riêng. Ví dụ,```
0 0
3
0 0
1 0
-1 0
```chỉ có một cặp không suy biến nên đáp án là`NO`. Một điểm tại (O) có khoảng cách bằng 0 với (O) và nó không thể tham gia vào một trong các đoạn hai điểm bắt buộc trong cấu hình bốn điểm hữu ích. 

Các tọa độ trùng lặp là một lý do khác để không cho rằng mọi hướng chuẩn hóa đều tự động biểu thị một cặp hợp lệ. Ví dụ,```
0 0
2
1 1
1 1
```chứa hai điểm thu thập ở cùng một vị trí. Chúng không nằm đối diện với (O) nên không thể tạo thành đoạn cần tìm thông qua (O). Câu trả lời là`NO`. 

Cuối cùng, vấn đề số học chính xác. Đại lượng hình học liên quan là tích của bình phương khoảng cách và việc sử dụng các giá trị dấu phẩy động cho nó có thể gây ra các lỗi đẳng thức không cần thiết. Tất cả tọa độ đều là tích phân nên toàn bộ phép tính có thể được thực hiện bằng số nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê từng bộ bốn điểm thu thập, thử cả ba cách để chia bốn điểm thành hai cặp, kiểm tra xem các đoạn tương ứng có đi qua (O) hay không, sau đó kiểm tra xem bốn điểm có đồng tuyến hay không. Điều này đúng vì mọi câu trả lời có thể đều được kiểm tra rõ ràng. Tuy nhiên, với (n=2000), chỉ cần liệt kê các tập con bốn điểm 

[ 
\binom{2000}{4}=664,668,499,500 
] 

lặp lại trong trường hợp xấu nhất. Đó là vượt xa thời gian có sẵn. 

Quan sát hình học hữu ích là hai đoạn được chọn là các cát tuyến của cùng một đường tròn đi qua (O). Với bất kỳ cặp điểm (A,B) nào trên một đường thẳng như vậy, hãy xác định 

[ 
P(A,B)=OA\cdot OB. 
] 

Đối với hai cát tuyến bất kỳ đi qua cùng một điểm (O) của đường tròn, định lý lũy thừa điểm cho 

[ 
OA\cdot OB=OC\cdot OD. 
] 

Vì vậy, thay vì kiểm tra bốn điểm cùng một lúc, chúng ta có thể kiểm tra từng đường thẳng qua (O) chứa một cặp điểm và tính tích khoảng cách của nó. Hai dòng hợp lệ khác nhau đưa ra câu trả lời chính xác cần thiết khi tích của chúng bằng nhau. 

Không cần thiết phải sử dụng khoảng cách thực tế. Nếu 

[ 
r_A^2=OA^2,\qquad r_B^2=OB^2, 
] 

thì đẳng thức của tích khoảng cách tương đương với 

[ 
r_A^2r_B^2=r_C^2r_D^2. 
] 

Tất cả các giá trị này là số nguyên. 

Nhiệm vụ còn lại là xác định các điểm đối diện trên cùng một đường thẳng. Đối với một điểm (P), dịch nó thành (O), giảm vectơ bằng gcd tọa độ của nó và giữ nguyên dấu của nó. Ví dụ: ((6,4)) trở thành ((3,2)), trong khi tia đối diện của nó là ((-3,-2)). Một cặp tồn tại chính xác khi cả hai vectơ chuẩn hóa xuất hiện. 

Đầu vào đảm bảo rằng có nhiều nhất hai điểm trên bất kỳ đường thẳng nào đi qua (O), do đó mỗi tia có thể chứa nhiều nhất một điểm liên quan. Chúng ta có thể lưu trữ một điểm cho mỗi vectơ có hướng chuẩn hóa, sau đó tìm phủ định của nó. 

Cách tiếp cận bạo lực hoạt động vì nó kiểm tra rõ ràng mọi cấu hình bốn điểm có thể có, nhưng không thành công khi (n=2000). Quan sát thấy rằng mọi cấu hình hợp lệ được xác định bởi hai cặp tia đối diện và đường tròn làm cho tích khoảng cách của chúng bằng nhau, cho phép chúng ta giảm vấn đề xuống việc băm một giá trị nguyên trên mỗi dòng hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^4)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(n\log C)) | (O(n)) | Đã chấp nhận | 

Ở đây (C) là tọa độ dịch tuyệt đối tối đa. Vì (C\le 2\cdot10^4), tính toán gcd thực sự là thời gian không đổi đối với các ràng buộc này. 

## Hướng dẫn thuật toán 

1. Dịch mọi điểm thu thập bằng cách trừ tọa độ của (O). Một điểm có tọa độ dịch chuyển ((0,0)) bị bỏ qua vì nó không thể tạo thành một cặp không suy biến với một điểm khác trên (O). 
2. Chuẩn hóa hướng của mọi điểm còn lại. Nếu vectơ dịch của nó là ((x,y)), chia cả hai tọa độ cho (\gcd(|x|,|y|)). Giữ dấu không thay đổi, để các tia đối diện tạo ra các phím đối diện. 
3. Lưu trữ cho mỗi vectơ có hướng chuẩn hóa chỉ số điểm và khoảng cách bình phương của nó 
[ 
r^2=x^2+y^2. 
] 
Điều kiện có nhiều nhất hai điểm trên mỗi đường đảm bảo rằng một tia không thể chứa hai điểm tập hợp khác nhau. 
4. Với mọi hướng (v=(x,y)), hãy tìm hướng ngược lại (-v). Nếu nó không tồn tại thì những điểm đó không thể tạo thành đoạn thẳng đi qua (O). 
5. Chỉ xử lý mỗi dòng vô hướng một lần. Một cách đơn giản là chỉ xử lý hướng thỏa mãn (v< -v) theo thứ tự bộ từ điển. Cho hai điểm (A,B) trên đường thẳng đó, hãy tính 
[ 
giá trị=OA^2\cdot OB^2. 
] 
6. Lưu trữ cặp đầu tiên được tìm thấy cho mỗi`value`. Nếu một dòng khác tạo ra cùng một giá trị, hãy xuất hai điểm cuối được lưu trữ theo sau là hai điểm cuối của dòng hiện tại. Hai đoạn thẳng của chúng đều chứa (O) và điều kiện tích bằng nhau đảm bảo rằng cả bốn điểm đều nằm trên một đường tròn. 
7. Nếu không có sản phẩm nào được lặp lại, hãy xuất ra`NO`. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ câu trả lời hợp lệ nào với các cặp (A,B) và (C,D). Vì cả hai đoạn thẳng đều đi qua (O) nên (A,B) nằm trên các tia đối diện của một đường thẳng và (C,D) nằm trên các tia đối diện của đường thẳng kia. Vì bốn điểm nằm trên một đường tròn nên lũy thừa của (O) đối với đường tròn đó là: 

[ 
OA\cdot OB=OC\cdot OD. 
] 

Bình phương cả hai vế cho kết quả chính xác về đẳng thức được kiểm tra bằng thuật toán. 

Ngược lại, giả sử hai đường thẳng phân biệt đi qua (O) có các cặp tia đối nhau (A,B) và (C,D) có tích bằng nhau. Đi vòng tròn qua (A,B,C). Công suất của nó tại (O) được xác định bởi cát tuyến đầu tiên và bằng (OA\cdot OB). Đường thẳng thứ hai cắt đường tròn này tại (C) và tại một điểm khác có khoảng cách với (O) phải tạo ra cùng một tích. Vì (OC\cdot OD) bằng giá trị đó nên (D) chính xác là giao điểm thứ hai. Như vậy cả bốn điểm đều nằm trên cùng một đường tròn. 

Điều bất biến là mỗi dòng hợp lệ được xử lý được biểu thị bằng chính xác một số nguyên, độ lớn lũy thừa bình phương của cặp của nó đối với (O). Các số nguyên bằng nhau tương ứng chính xác với hai cát tuyến có thể thuộc cùng một đường tròn. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    ox, oy = map(int, input().split())
    n = int(input())

    rays = {}

    for idx in range(1, n + 1):
        x, y = map(int, input().split())
        x -= ox
        y -= oy

        if x == 0 and y == 0:
            continue

        g = math.gcd(abs(x), abs(y))
        x //= g
        y //= g

        rays[(x, y)] = (idx, x * x + y * y)

    seen_value = {}

    for direction, data in rays.items():
        x, y = direction
        opposite = (-x, -y)

        if opposite not in rays:
            continue

        # Process each undirected line only once.
        if direction > opposite:
            continue

        idx1, r1 = data
        idx2, r2 = rays[opposite]

        value = r1 * r2

        if value in seen_value:
            a, b = seen_value[value]
            print("YES")
            print(a, b, idx1, idx2)
            return

        seen_value[value] = (idx1, idx2)

    print("NO")

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã dịch mọi điểm để nhà máy trở thành điểm gốc. Điều này làm cho cả việc kiểm tra hướng và tính toán khoảng cách đều độc lập với vị trí ban đầu của (O). 

Việc chuẩn hóa gcd biến các vectơ trên cùng một tia thành cùng một khóa từ điển. Chẳng hạn, ((10,6)) và ((5,3)) đều trở thành ((5,3)). Tia đối diện của chúng được biểu thị bằng ((-5,-3)). Chúng ta cố tình bảo toàn dấu vì chỉ một đường vô hướng là không đủ để biết đoạn giữa hai điểm có chứa (O) hay không. 

Khoảng cách bình phương được tính toán sau khi chuẩn hóa trong mã. Điều này hợp lệ vì vectơ chuẩn hóa vẫn trỏ theo cùng một hướng, nhưng có một điểm tinh tế ở đây: khoảng cách bình phương được lưu trữ phải là khoảng cách bình phương thực tế của điểm ban đầu, không phải độ dài bình phương của hướng chuẩn hóa. Do đó, mã được viết ở trên sẽ không chính xác. 

Việc triển khai đúng phải giữ lại tọa độ đã dịch ban đầu để tính toán khoảng cách trong khi chỉ sử dụng vectơ chuẩn hóa làm khóa từ điển. Đây là giải pháp hoàn chỉnh được sửa chữa.```python
import sys
import math

input = sys.stdin.readline

def solve():
    ox, oy = map(int, input().split())
    n = int(input())

    rays = {}

    for idx in range(1, n + 1):
        x, y = map(int, input().split())
        x -= ox
        y -= oy

        if x == 0 and y == 0:
            continue

        dist2 = x * x + y * y

        g = math.gcd(abs(x), abs(y))
        dx = x // g
        dy = y // g

        rays[(dx, dy)] = (idx, dist2)

    seen_value = {}

    for direction, (idx1, r1) in rays.items():
        dx, dy = direction
        opposite = (-dx, -dy)

        if opposite not in rays:
            continue

        if direction > opposite:
            continue

        idx2, r2 = rays[opposite]
        value = r1 * r2

        if value in seen_value:
            a, b = seen_value[value]
            print("YES")
            print(a, b, idx1, idx2)
            return

        seen_value[value] = (idx1, idx2)

    print("NO")

if __name__ == "__main__":
    solve()
```Phiên bản thứ hai là phiên bản để gửi. Sự khác biệt giữa hướng chuẩn hóa và vectơ ban đầu là điều cần thiết. Quá trình chuẩn hóa sẽ loại bỏ thông tin khoảng cách, do đó, việc sử dụng vectơ chuẩn hóa để tính toán (OA^2) sẽ làm cho các điểm không liên quan trên cùng một tia dường như có cùng khoảng cách. 

Số nguyên Python có độ chính xác tùy ý, do đó sản phẩm không gây tràn. Với chênh lệch tọa độ ban đầu được giới hạn bởi (2\cdot10^4), mỗi khoảng cách bình phương tối đa là (8\cdot10^8) và tích ở dưới khoảng (6,4\cdot10^{17}). 

Sự so sánh từ điển học`direction > opposite`ngăn chặn việc xử lý cùng một dòng hai lần. Nó không ảnh hưởng đến hình học, nó chỉ tránh việc lưu trữ cùng một cặp trong bản đồ sức mạnh hai lần. 

Thứ tự của bốn chỉ số cuối cùng cũng có chủ ý. Hai chỉ số đầu tiên đến từ một cặp tia đối diện, trong khi hai chỉ số cuối đến từ một cặp tia đối diện. Do đó, phân đoạn đầu tiên và phân đoạn thứ hai đều đi qua (O), chính xác theo yêu cầu của định dạng đầu ra. 

## Ví dụ đã hoạt động 

Chỉ có một mẫu được cung cấp trong câu lệnh, vì vậy dấu vết thứ hai sử dụng một cấu hình tùy chỉnh nhỏ. 

### Mẫu 1 

Nhà máy ở (O=(1,1)). Sau khi dịch bằng (O), các điểm liên quan là: 

| Chỉ mục | Vectơ được dịch | Hướng chuẩn hóa | Khoảng cách bình phương | Đối diện | Giá trị cặp
