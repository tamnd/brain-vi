---
title: "CF 102465F - Paris về đêm"
description: "Chúng tôi có (N) di tích. Tượng đài (i) có tọa độ ((xi,yi)) và cấp dương (gi). Morgane chọn hai di tích riêng biệt làm điểm cuối của một đường."
date: "2026-08-09T03:38:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102465
codeforces_index: "F"
codeforces_contest_name: "2018-2019 ICPC Southwestern European Regional Programming Contest (SWERC 2018)"
rating: 0
weight: 102465
solve_time_s: 166
verified: true
draft: false
---

[CF 102465F - Paris về đêm](https://codeforces.com/problemset/problem/102465/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (N) di tích. Tượng đài (i) có tọa độ ((x_i,y_i)) và cấp dương (g_i). Morgane chọn hai di tích riêng biệt làm điểm cuối của một đường. Quả bóng được đặt giữa hai tượng đài đó và đường chia tất cả các tượng đài còn lại thành hai nửa mặt phẳng mở. 

Mỗi bức ảnh chứa hai tượng đài điểm cuối đã chọn, cộng với tất cả các tượng đài ở một trong hai phía của đường thẳng. Nếu tổng của hai bức ảnh là (A) và (B), chúng tôi muốn giảm thiểu (|A-B|) trên mọi cặp tượng đài điểm cuối có thể có. 

Đầu vào đưa ra số lượng di tích theo sau là tọa độ và cấp độ của chúng. Đầu ra là sự khác biệt nhỏ nhất có thể có giữa hai loại ảnh. 

Sự đơn giản hóa hình học quan trọng là hai tượng đài điểm cuối xuất hiện trong cả hai bức ảnh. Do đó, điểm của họ được cộng vào cả hai tổng và bị hủy khi chúng tôi lấy chênh lệch. Đối với một cặp điểm cuối cố định (i,j), giả sử cấp độ của các di tích ở một phía của đường thẳng có tổng bằng (S). Gọi tổng cấp độ của tất cả các di tích là (T). Bên kia có lớp 

[ 
T-g_i-g_j-S. 
] 

Như vậy sự khác biệt là 

\left|2S-T+g_i+g_j\right|. 
] 

Vì vậy, toàn bộ vấn đề trở thành: với mỗi đường thẳng đi qua hai tượng đài, hãy tìm tổng số điểm ở một bên càng nhanh càng tốt. 

Giới hạn tọa độ đạt đến (10^9), do đó chênh lệch tọa độ và tích chéo có thể lớn hơn nhiều so với số nguyên 32 bit. Một tích chéo có thể đạt khoảng (10^{18}), trong khi tổng của tất cả các điểm có thể đạt tới (4\cdot10^{12}). Số nguyên Python xử lý trực tiếp các giá trị này, do đó không có vấn đề tràn trong quá trình triển khai. 

Giới hạn (N\le 4000) là tín hiệu thuật toán chính. Việc liệt kê tất cả các cặp điểm cuối đã mang lại khả năng (N^2/2), do đó, việc chi tiêu (O(N)) cho mỗi cặp sẽ yêu cầu khoảng 

31.976.004.000 
] 

phân loại điểm trong trường hợp xấu nhất. Đó là vượt xa giới hạn 15 giây. Chúng ta cần xử lý tất cả các cặp được liên kết với một điểm cuối cố định cùng nhau. 

Trường hợp cạnh đầu tiên là (N=2). Ví dụ,```
2
0 0 1
1 2 999
```có câu trả lời`0`. Không có di tích nào ở hai bên chiến tuyến. Cả hai bức ảnh đều chứa cả hai điểm cuối, vì vậy cả hai đều là (1000). Giải pháp bất cẩn chỉ tính mỗi điểm cuối một lần sẽ báo cáo sai (998). 

Trường hợp cạnh thứ hai là (N=3). Ví dụ,```
3
0 0 5
1 0 10
0 1 7
```có câu trả lời`5`. Cặp tượng đài nào được chọn làm tượng đài ranh giới thì tượng đài còn lại nằm hoàn toàn ở một bên, bên kia để trống. Các điểm cuối bị hủy bỏ, để lại sự khác biệt bằng với cấp độ của tượng đài còn lại. Sự lựa chọn tốt nhất để lại tượng đài với điểm (5). 

Một sai lầm dễ mắc phải khác là sử dụng sai mức độ nghiêm ngặt khi kiểm tra hai bên ranh giới. Tuyên bố đảm bảo rằng không có ba di tích nào thẳng hàng, do đó không có di tích thứ ba nào có thể nằm chính xác trên một đường ranh giới. Chúng ta có thể sử dụng thử nghiệm chéo sản phẩm một cách an toàn. Việc coi bản thân điểm cuối như một phần của một bên sẽ là sai và việc sao chép nó trong tổng bên cũng sẽ làm hỏng câu trả lời. 

Phân tích SWERC chính thức mô tả cách giảm tương tự từ giải pháp (O(N^3)) thành giải pháp (O(N^2\log N)) bằng cách sắp xếp các di tích khác xung quanh mỗi di tích cố định và cập nhật dần dần sự khác biệt. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp rất đơn giản. Liệt kê mỗi cặp (i,j) là hai tượng đài ranh giới. Sau đó kiểm tra mọi tượng đài khác (k), tính hướng của (i,j,k) bằng tích chéo và cộng (g_k) với một trong hai tổng bên. Khi tất cả các điểm đã được phân loại, hãy tính hiệu của tổng hai vế. 

Điều này đúng vì mọi lựa chọn có thể có của các di tích ranh giới đều được xem xét rõ ràng và tích chéo cho chúng ta biết chính xác phía nào của đường ranh giới mà mỗi di tích còn lại chiếm giữ. Vấn đề là chi phí. Có các cặp điểm cuối (O(N^2)) và mỗi cặp yêu cầu phân loại (O(N)), đưa ra (O(N^3)). Đối với (N=4000), trường hợp xấu nhất chứa khoảng (3,2\cdot10^{10}) phân loại điểm, vì vậy phương pháp này không khả thi. 

Quan sát loại bỏ hệ số phụ của (N) là đối với một tượng đài cố định (i), tất cả các điểm cuối thứ hai có thể có đều có thể được xem xét theo thứ tự góc. 

Đặt tạm thời tượng đài (i) tại gốc tọa độ và xét các vectơ từ (i) đến mọi tượng đài khác. Sắp xếp các vectơ đó theo góc cực. Khi di tích ranh giới thứ hai di chuyển theo chiều kim đồng hồ theo thứ tự được sắp xếp này, tập hợp các di tích nằm trong một nửa mặt phẳng mở cụ thể sẽ thay đổi liên tục. Chính xác hơn, các di tích ở một phía của đường ranh giới mới tạo thành các khoảng góc liền kề có chiều rộng nhỏ hơn (180^\circ). 

Khoảng thời gian đó có thể được duy trì bằng cách quét hai con trỏ. Điểm cuối bên trái là vectơ ranh giới hiện tại. Điểm cuối bên phải chỉ di chuyển về phía trước vì việc quay vectơ biên không thể làm cho điểm cuối của hình bán nguyệt tương ứng di chuyển về phía sau. Do đó, trong toàn bộ quá trình quét đối với một (i) cố định, mỗi con trỏ chỉ di chuyển (O(N)) lần. 

Hoạt động tốn kém duy nhất còn lại đối với một (i) cố định là sắp xếp các di tích (N-1) khác theo góc, có chi phí (O(N\log N)). Lặp lại điều này cho tất cả (N) lựa chọn có thể có của (i) sẽ cho (O(N^2\log N)). 

Phân tích chính thức đưa ra chính xác sự phức tạp này và mô tả thứ tự các di tích xung quanh mỗi di tích có giới hạn cố định, sau đó tính toán sự khác biệt về cấp độ theo cấp độ tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^3)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N^2\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các di tích và tính tổng điểm (T). Chúng tôi sẽ liên tục sử dụng (T-g_i-g_j), tổng cấp độ của tất cả các di tích ngoại trừ hai di tích ranh giới. 
2. Cố định một tượng đài (i) làm tượng đài ranh giới đầu tiên. Với mọi tượng đài khác (j), hãy tạo thành vectơ 
[ 
v_j=(x_j-x_i,\ y_j-y_i). 
] 
Lưu trữ hướng của nó, các thành phần vectơ và cấp của nó. Chúng ta sắp xếp các vectơ này theo góc cực xung quanh (i). 

Việc sắp xếp cho chúng ta một thứ tự vòng tròn trong đó tượng đài ranh giới thứ hai có thể xoay quanh (i). 
3. Nhân đôi trình tự góc về mặt khái niệm bằng cách cho phép các chỉ số lên tới (2(N-1)-1). Về mặt vật lý, chúng ta không cần phải sao chép các đối tượng vector. Một chỉ số (k) đề cập đến`vectors[k % m]`, trong đó (m=N-1). 

Điều này biến thứ tự góc tròn thành một chuỗi tuyến tính, do đó, một khoảng hình bán nguyệt có thể vượt qua ranh giới (0^\circ) mà không có trường hợp đặc biệt. 
4. Duy trì con trỏ (k) ở cuối hình bán nguyệt mở hiện tại. Đối với vectơ biên hiện tại (v_j), tiến lên (k) while 
[ 
\operatorname{cross}(v_j,v_k)>0. 
] 

Tích chéo dương có nghĩa là (v_k) ngược chiều kim đồng hồ với (v_j) một góc nằm giữa (0^\circ) và (180^\circ). Vì không có ba di tích nào thẳng hàng nên tích chéo không thể bằng 0 đối với hai vectơ khác nhau từ cùng một di tích cố định. 
5. Duy trì tổng tiền tố của các điểm theo thứ tự góc. Nếu hình bán nguyệt hiện tại chứa các chỉ số từ (j+1) đến (k-1), tổng điểm của nó thu được bằng (O(1)) từ tổng tiền tố. 

Bản thân di tích ranh giới bị loại trừ vì khoảng thời gian bắt đầu hoàn toàn sau (j). Đây chính xác là số tiền phụ chúng ta cần. 
6. Đối với các di tích ranh giới (i) và (j), đặt`side`là tổng số điểm bên trong hình bán nguyệt hiện tại. Cấp độ của tất cả các di tích không biên giới là 
[ 
T-g_i-g_j. 
] 
Do đó tổng hai vế là`side`và 
[ 
T-g_i-g_j-\text{side}. 
] 
Do đó, sự khác biệt của bức ảnh là 
[ 
\left|2\cdot\text{side}-(T-g_i-g_j)\right|. 
] 
7. Cập nhật mức tối thiểu toàn cầu với chênh lệch này. Nếu câu trả lời bằng 0, hãy dừng lại ngay lập tức vì không tồn tại chênh lệch tuyệt đối nhỏ hơn. 
8. Lặp lại việc sắp xếp góc và quét hai con trỏ cho mọi di tích ranh giới đầu tiên có thể (i). Mọi cặp di tích ranh giới không có thứ tự đều gặp phải trong ít nhất một trong những lần quét này, do đó mức tối thiểu toàn cầu chứa mức tối ưu thực sự. 

### Tại sao nó hoạt động 

Đối với di tích cố định (i), các vectơ được sắp xếp biểu thị tất cả các hướng có thể có của di tích ranh giới thứ hai. Đối với bất kỳ hướng hiện tại nào (v_j), các di tích ở một bên của đường ranh giới tương ứng chính xác là những di tích có hướng nằm hoàn toàn ngược chiều kim đồng hồ từ (v_j) nhỏ hơn (180^\circ). Điều kiện sản phẩm chéo xác định chính xác khoảng thời gian này. 

Khi (j) tiến lên theo thứ tự góc, điểm cuối của khoảng (180^\circ) đó không bao giờ di chuyển lùi lại. Do đó, quá trình quét hai con trỏ sẽ truy cập mọi khoảng thời gian có thể có trong khi vẫn duy trì tổng điểm của nó. Tổng tiền tố chuyển đổi từng khoảng thành truy vấn (O(1)). 

Mỗi cặp di tích ranh giới xác định một khoảng như vậy và hai bức ảnh chính xác là hai tổng bên bổ sung cộng với hai điểm cuối giống nhau. Do đó, công thức được thuật toán sử dụng sẽ tính toán sự khác biệt chính xác cho mọi cặp có thể. Lấy giá trị tối thiểu trên tất cả các cặp sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n = int(input())
    points = [tuple(map(int, input().split())) for _ in range(n)]

    total_grade = sum(g for _, _, g in points)
    answer = total_grade

    for i in range(n):
        xi, yi, gi = points[i]

        vectors = []
        for j in range(n):
            if i == j:
                continue

            xj, yj, gj = points[j]
            dx = xj - xi
            dy = yj - yi

            vectors.append((math.atan2(dy, dx), dx, dy, gj))

        vectors.sort(key=lambda v: v[0])
        m = n - 1

        grades = [v[3] for v in vectors]

        prefix = [0] * (2 * m + 1)
        for p in range(2 * m):
            prefix[p + 1] = prefix[p] + grades[p % m]

        k = 1

        for j in range(m):
            if k < j + 1:
                k = j + 1

            _, ax, ay, gj = vectors[j]

            limit = j + m

            while k < limit:
                _, bx, by, _ = vectors[k % m]

                cross = ax * by - ay * bx

                if cross <= 0:
                    break

                k += 1

            side = prefix[k] - prefix[j + 1]
            other = total_grade - gi - gj

            diff = abs(2 * side - other)

            if diff < answer:
                answer = diff

                if answer == 0:
                    print(0)
                    return

    print(answer)

if __name__ == "__main__":
    solve()
```Vòng ngoài chọn tượng đài ranh giới đầu tiên. Đối với điểm cố định đó, cấu trúc bên trong tạo ra một vectơ cho mọi tượng đài khác. Vectơ lưu trữ cả khóa sắp xếp góc và tọa độ nguyên chính xác của nó, bởi vì phép tính tích chéo sau này phải sử dụng các thành phần số nguyên ban đầu.`atan2`chỉ được sử dụng để có được thứ tự vòng tròn. Bài kiểm tra tư cách thành viên thực tế (180^\circ) sử dụng tích số nguyên chính xác, do đó số học dấu phẩy động không được sử dụng để quyết định liệu một di tích có thuộc về một bên hay không. Điều này đặc biệt hữu ích với tọa độ lớn tới (10^9). 

Các vectơ được sắp xếp theo góc của chúng trong ([-\pi,\pi]). Mảng tiền tố có độ dài (2m+1), thể hiện hai bản sao của chuỗi cấp độ. Chúng tôi không tự sao chép các đối tượng vectơ, điều này giúp duy trì bộ nhớ trên mỗi trục nhỏ. 

Con trỏ`k`không bao giờ được thiết lập lại khi`j`tăng lên. Đây là thuộc tính hai con trỏ chính. Phần cuối của hình bán nguyệt mở di chuyển đều đều xung quanh hình tròn, do đó`k`chỉ có thể tăng trong một lần quét hoàn chỉnh. Do đó, tổng số thử nghiệm sản phẩm chéo cho một trục cố định là (O(N)), thay vì (O(N^2)). 

điều kiện`cross <= 0`dừng hình bán nguyệt. Tích chéo bằng 0 có nghĩa là hai vectơ thẳng hàng, điều này không thể xảy ra đối với hai di tích khác biệt vì không có ba di tích nào nằm trên một đường thẳng. sử dụng`<= 0`tuy nhiên đây là quy ước ranh giới an toàn và cũng loại trừ bản sao trùng lặp của vectơ hiện tại sau một vòng quay hoàn chỉnh. 

biểu thức```
side = prefix[k] - prefix[j + 1]
```cố tình bắt đầu sau`j`. Di tích ranh giới hiện tại không được tính là một phần của tổng hai bên. Hai điểm cuối được chèn vào cả hai điểm ảnh một cách riêng biệt và hủy bỏ khi tính chênh lệch của chúng. 

Số nguyên Python tự động mở rộng đến độ chính xác tùy ý, do đó tích chéo gần (10^{18}) và tổng điểm gần (4\cdot10^{12}) không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

Mẫu chính thức là:```
8
0 0 10
1 1 2
2 1 3
3 2 7
2 3 8
5 2 5
1 5 12
4 5 14
```Tổng điểm là (61). Coi di tích ((2,1)), có cấp (3), là di tích có ranh giới cố định đầu tiên. Các di tích khác được sắp xếp ngược chiều kim đồng hồ theo hướng của chúng kể từ thời điểm này. 

| Vị trí góc | Tượng đài | Lớp | 
| --- | --- | --- | 
| 1 | ((0,0)) | 10 | 
| 2 | ((5,2)) | 5 | 
| 3 | ((3,2)) | 7 | 
| 4 | ((4,5)) | 14 | 
| 5 | ((2,3)) | 8 | 
| 6 | ((1,5)) | 12 | 
| 7 | ((1,1)) | 2 | 

Trong quá trình quét, trạng thái quan trọng là: 

| Tượng đài ranh giới | Lớp | Tổng phụ | Tổng bên kia | Sự khác biệt | 
| --- | --- | --- | --- | --- | 
| ((0,0)) | 10 | 46 | 2 | 44 | 
| ((5,2)) | 5 | 43 | 10 | 33 | 
| ((3,2)) | 7 | 46 | 5 | 41 | 
| ((4,5)) | 14 | 32 | 12 | 20 | 
| ((2,3)) | 8 | 24 | 26 | 2 | 
| ((1,5)) | 12 | 12 | 34 | 22 | 
| ((1,1)) | 2 | 10 | 46 | 36 | 

Khi ((2,3)) là tượng đài ranh giới thứ hai, đường thẳng đứng. Các di tích bên trái có cấp độ (10+2+12=24), trong khi các di tích bên phải có cấp độ (7+5+14=26). Cả hai bức ảnh đều chứa điểm cuối (3+8=11), vì vậy điểm cuối cùng của chúng là (35) và (37). Sự khác biệt của chúng là (2), phù hợp với đầu ra mẫu. Điều này chứng tỏ sự hủy bỏ trung tâm của các cấp độ ranh giới và tính chính xác của khoảng thời gian hình bán nguyệt. 

Đối với ví dụ thứ hai, hãy xem xét đầu vào có kích thước tối thiểu:```
2
0 0 1
1 2 999
```Chỉ có một cặp di tích ranh giới có thể có. 

| Cặp ranh giới | Tổng phụ | Mặt khác | Tổng điểm cuối | Điểm ảnh | Sự khác biệt | 
| --- | --- | --- | --- | --- | --- | 
| 1, 2 | 0 | 0 | 1000 | 1000, 1000 | 0 | 

Hai bức ảnh giống hệt nhau vì cả hai đều chứa cả hai tượng đài. Việc quét hai con trỏ không có tượng đài thứ ba để chèn vào hai bên, vì vậy tổng của cả hai bên vẫn bằng 0. Điều này xác nhận rằng các điểm cuối phải được tính trong cả hai bức ảnh thay vì được gán cho một bên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^2\log N)) | Đối với mỗi tượng đài cố định, sắp xếp (N-1) hướng và sau đó thực hiện quét hai con trỏ (O(N)). | 
| Không gian | (O(N)) | Một danh sách góc và một mảng tổng tiền tố được duy trì cho tượng đài cố định hiện tại. | 

Đối với (N=4000), thuật toán thực hiện (N) loại góc của khoảng (N) mỗi phần tử, theo sau là chỉ cập nhật hình học (O(N)) cho mỗi loại. Đây là cách tiếp cận dự định (O(N^2\log N)) được mô tả trong phân tích SWERC chính thức. Việc sử dụng bộ nhớ vẫn tuyến tính vì dữ liệu góc cho một trục có thể bị loại bỏ trước khi xử lý trục tiếp theo. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng cùng một thuật toán trong một cuộc gọi có thể`solve_data`để có thể kiểm tra từng trường hợp bằng các xác nhận Python.```python
import io
import math

def solve_data(data: str) -> str:
    it = iter(data.strip().split())
    n = int(next(it))

    points = []
    for _ in range(n):
        x = int(next(it))
        y = int(next(it))
        g = int(next(it))
        points.append((x, y, g))

    total_grade = sum(g for _, _, g in points)
    answer = total_grade

    for i in range(n):
        xi, yi, gi = points[i]

        vectors = []
        for j in range(n):
            if i == j:
                continue

            xj, yj, gj = points[j]
            dx = xj - xi
            dy = yj - yi
            vectors.append((math.atan2(dy, dx), dx, dy, gj))

        vectors.sort(key=lambda v: v[0])

        m = n - 1
        grades = [v[3] for v in vectors]

        prefix = [0] * (2 * m + 1)
        for p in range(2 * m):
            prefix[p + 1] = prefix[p] + grades[p % m]

        k = 1

        for j in range(m):
            if k < j + 1:
                k = j + 1

            _, ax, ay, gj = vectors[j]
            limit = j + m

            while k < limit:
                _, bx, by, _ = vectors[k % m]
                if ax * by - ay * bx <= 0:
                    break
                k += 1

            side = prefix[k] - prefix[j + 1]
            other = total_grade - gi - gj
            diff = abs(2 * side - other)

            if diff < answer:
                answer = diff

            if answer == 0:
                return "0\n"

    return str(answer) + "\n"

# Provided sample.
sample1 = """\
8
0 0 10
1 1 2
2 1 3
3 2 7
2 3 8
5 2 5
1 5 12
4 5 14
"""
assert solve_data(sample1) == "2\n", "provided sample"

# Minimum-size input. Both monuments are boundary monuments,
# so both photographs contain exactly the same two monuments.
sample2 = """\
2
0 0 1
1 2 999
"""
assert solve_data(sample2) == "0\n", "two monuments must give zero"

# Three monuments. Whichever pair is chosen, the remaining
# monument is alone on one side. The best remaining grade is 5.
sample3 = """\
3
0 0 5
1 0 10
0 1 7
"""
assert solve_data(sample3) == "5\n", "three-point case"

# All grades are equal. A balanced split is not even necessary:
# the endpoint grades cancel, and some boundary pair gives
# equal side contributions.
sample4 = """\
4
0 0 7
3 0 7
0 4 7
3 4 7
"""
assert solve_data(sample4) == "0\n", "all grades equal"

# Coordinates and grades at their maximum allowed magnitude.
# With three monuments, the answer is the smallest grade, 1.
sample5 = """\
3
0 0 1000000000
1000000000 0 999999999
0 1000000000 1
"""
assert solve_data(sample5) == "1\n", "large coordinates and grades"

# Maximum-size case. Points (i, i^2) contain no three collinear
# points, all grades are equal, and the answer is immediately zero.
n = 4000
lines = [str(n)]
for i in range(n):
    lines.append(f"{i} {i * i} 1")

large_case = "\n".join(lines) + "\n"
assert solve_data(large_case) == "0\n", "maximum N with valid geometry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 8 tượng chính thức |`2`| Quét hình học hoàn chỉnh và cặp tối ưu | 
| Hai di tích |`0`| Cả hai điểm cuối đều xuất hiện trong cả hai bức ảnh | 
| Ba tượng đài không thẳng hàng bậc 5, 10, 7 |`5`| Hủy nửa mặt phẳng trống và điểm cuối | 
| Bốn di tích đồng cấp |`0`| Không trả lời và chấm dứt sớm | 
| Tọa độ và điểm gần (10^9) |`1`| Ranh giới số học và tọa độ số nguyên lớn | 
| 4000 điểm ((i,i^2)), toàn cấp 1 |`0`| Tối đa (N), hình học không ba cộng tuyến hợp lệ và hiệu suất | 

## Vỏ cạnh 

Đối với hai di tích```
2
0 0 1
1 2 999
```cặp ranh giới duy nhất chứa mọi tượng đài. Tổng các bên đều bằng 0, trong khi cả hai bức ảnh đều chứa điểm cuối (1+999). Điểm ảnh là (1000) và (1000), vì vậy câu trả lời là`0`. Thuật toán xử lý việc này vì`other = total_grade - gi - gj`bằng 0 và khoảng tiền tố trống. 

Đối với ba di tích```
3
0 0 5
1 0 10
0 1 7
```cố định bất kỳ tượng đài nào làm ranh giới đầu tiên. Hai vectơ còn lại cách nhau một góc hoàn toàn giữa (0^\circ) và (180^\circ), do đó, một cặp ranh giới có thể đặt tượng đài thứ ba ở một bên và để trống phía bên kia. Đối với mỗi cặp, sự khác biệt chính xác là cấp độ của tượng đài còn lại. Chọn cặp rời lớp (5) sẽ có đáp án`5`. 

Đối với điểm bằng nhau,```
4
0 0 7
3 0 7
0 4 7
3 4 7
```thuật toán có thể tìm thấy một cặp có tổng hai cạnh bằng nhau, tạo ra hiệu bằng 0. Càng sớm càng`answer == 0`, việc triển khai sẽ trả về ngay lập tức vì không có sự khác biệt tuyệt đối nào có thể nhỏ hơn. 

Đối với giá trị tọa độ tối đa,```
3
0 0 1000000000
1000000000 0 999999999
0 1000000000 1
```cả ba điểm tạo thành một tam giác hợp lệ. Vì chỉ có ba di tích nên câu trả lời là cấp độ nhỏ nhất,`1`. Các tích chéo liên quan đến các giá trị xung quanh (10^{18}) mà Python xử lý chính xác. 

Đối với bài kiểm tra kích thước tối đa, điểm là ((i,i^2)) cho (0\le i<4000). Không có ba điểm nào trong số này thẳng hàng vì một đường thẳng có thể cắt đường cong bậc hai (y=x^2) ở nhiều nhất hai điểm. Mỗi điểm là (1), do đó tồn tại sự khác biệt bằng 0 và thuật toán có thể kết thúc ngay khi tìm thấy một điểm. Thử nghiệm này xác nhận cả các giả định hình học và tỷ lệ dự định (O(N^2\log N)).
