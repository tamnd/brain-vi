---
title: "CF 102307E - Hình ảnh cực nét"
description: "Chúng ta có (n) vật thể phát sáng, mỗi vật thể được mô tả bằng khoảng cách của nó với Trái đất và vị trí góc của nó quanh Trái đất. Đài quan sát có thể chọn bất kỳ khoảng hướng tâm nào có độ dài (d), được viết là ([x,x+d]) và bất kỳ khoảng góc nào của độ dài (alpha), được viết là ([omega,omega+alpha])."
date: "2026-08-13T23:41:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "E"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 80
verified: true
draft: false
---

[CF 102307E - Hình ảnh cực chất](https://codeforces.com/problemset/problem/102307/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có (n) vật thể phát sáng, mỗi vật thể được mô tả bằng khoảng cách của nó với Trái đất và vị trí góc của nó quanh Trái đất. Đài quan sát có thể chọn bất kỳ khoảng hướng tâm nào có chiều dài (d), được viết là ([x,x+d]) và bất kỳ khoảng góc nào của chiều dài (\alpha), được viết là ([\omega,\omega+\alpha]). Tọa độ góc là hình tròn, do đó một khoảng có thể đi qua (360^\circ) và tiếp tục từ (0^\circ). 

Nhiệm vụ là chọn cả hai khoảng sao cho số lượng vật thể có khoảng cách và góc đồng thời bên trong chúng càng lớn càng tốt. Câu trả lời là số lượng thi thể bị bắt tối đa. 

Dữ liệu đầu vào chứa tối đa (10^5) phần thân, khoảng cách là số nguyên lên tới (10^5) và tất cả các góc và (\alpha) có tối đa hai chữ số sau dấu thập phân. Giới hạn (10^5) loại trừ việc kiểm tra mọi cặp nội dung, vì thuật toán (O(n^2)) sẽ thực hiện khoảng (10^{10}) thao tác trong trường hợp xấu nhất. Một giải pháp xung quanh (O(n\log n)) là phù hợp với giới hạn hai giây. 

Hai chữ số thập phân trên các góc cho chúng ta một giới hạn hữu ích khác. Sau khi nhân mọi góc với (100), mọi góc liên quan đều là số nguyên từ (0) đến (35999). Điều này cho phép chúng ta sử dụng cây phân đoạn chỉ trên (36000) vị trí góc có thể có thay vì xây dựng một cấu trúc có kích thước phụ thuộc vào (n). 

Trường hợp ranh giới đầu tiên là (\alpha=0). Ví dụ,```
1 10 0.00
5 20.00
```có câu trả lời (1), bởi vì một khoảng góc có độ dài bằng 0 vẫn có thể chứa một vật chính xác ở góc đã chọn của nó. Việc triển khai coi độ dài bằng 0 là khoảng trống sẽ trả về không chính xác (0). 

Trường hợp ranh giới thứ hai là vượt qua khoảng góc (360^\circ). Ví dụ,```
2 10 20.00
5 350.00
5 10.00
```có câu trả lời (2). Việc chọn khoảng góc ([350^\circ,10^\circ]) sẽ chụp cả hai vật thể. Việc triển khai khoảng thời gian tuyến tính, không tròn sẽ bỏ lỡ một trong số chúng. 

Trường hợp ranh giới thứ ba là tính bao hàm. Ví dụ,```
2 10 10.00
1 0.00
11 10.00
```có câu trả lời (2), vì cả hai điểm cuối khoảng cách và cả hai điểm cuối góc đều được bao gồm. Một so sánh nghiêm ngặt chẳng hạn như (r > x) hoặc (\theta < \omega+\alpha) sẽ làm mất đi một vật thể nằm chính xác trên một điểm cuối. 

Cuối cùng, tất cả các vị trí trùng lặp đều phải được tính. Ví dụ,```
3 5 0.00
10 25.00
10 25.00
10 25.00
```có câu trả lời (3). Các phần thân khác nhau mặc dù tọa độ của chúng giống hệt nhau, do đó cấu trúc dữ liệu phải thực hiện một lần cập nhật cho mỗi phần thân đầu vào. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi khoảng bán kính có thể có và mọi khoảng góc có thể có. Chúng ta có thể giảm phần đầu tiên một chút bằng cách quan sát rằng một khoảng bán kính tối ưu có thể được di chuyển cho đến khi điểm cuối bên phải của nó đạt tới khoảng cách của vật thể nào đó, do đó chỉ có (n) cửa sổ bán kính có liên quan. Tuy nhiên, đối với mỗi cửa sổ như vậy, việc kiểm tra tất cả các điểm bắt đầu góc có thể có hoặc tất cả các cặp điểm cuối góc vẫn phải thực hiện công việc (O(n)). Trong trường hợp xấu nhất, điều này trở thành (O(n^2)), khoảng (10^{10}) hoạt động ở cấp độ cơ thể cho (n=10^5), vượt xa giới hạn thời gian. 

Lực lượng vũ phu là chính xác bởi vì mỗi tập hợp được bắt được xác định bởi một khoảng hướng tâm và một khoảng góc, và việc thử tất cả các lựa chọn có liên quan cuối cùng sẽ kiểm tra một cặp tối ưu. Vấn đề là bài toán góc được tính lại từ đầu sau mỗi lần thay đổi hướng tâm. 

Quan sát quan trọng là khoảng thời gian xuyên tâm có thể bị quét. Sắp xếp các phần tử theo khoảng cách và duy trì chính xác các phần tử có khoảng cách thuộc về cửa sổ hiện tại ([rd,r]), trong đó (r) là khoảng cách của phần thân ngoài cùng bên phải hiện tại. Khi chúng ta chuyển sang cơ thể tiếp theo, một số cơ thể đi vào cửa sổ và một số rời khỏi đó. Mỗi phần thân được chèn một lần và được gỡ bỏ một lần. 

Chúng ta vẫn cần trả lời một câu hỏi góc động: trong số các vật thể hiện đang hoạt động, số lớn nhất chứa trong một khoảng độ dài góc (\alpha) là bao nhiêu? 

Bởi vì mỗi góc đầu vào đều có hai chữ số thập phân, tỷ lệ góc bằng (100). Chỉ có (36000) vị trí cấp một trăm có thể. Thay vì lưu trữ số lượng cơ thể ở mỗi khoảng thời gian có thể, hãy bắt đầu trực tiếp, hãy nghĩ xem một cơ thể đóng góp những gì. 

Giả sử một vật có góc (\theta). Một khoảng góc bắt đầu tại (s) ghi lại chính xác thời điểm 

[ 
s \leq \theta \leq s+\alpha 
] 

trên vòng tròn. Sắp xếp lại mang lại 

[ 
\theta-\alpha \leq s \leq \theta. 
] 

Do đó, từ góc độ các khoảng thời gian bắt đầu có thể xảy ra, một phần tử đóng góp (+1) vào phạm vi vòng tròn các lần bắt đầu từ (\theta-\alpha) đến (\theta). Do đó, việc thêm phần thân là phép cộng phạm vi vòng tròn và loại bỏ nó là phép trừ phạm vi vòng tròn. 

Cây phân đoạn có thể hỗ trợ phép cộng phạm vi và giá trị tối đa trên toàn bộ mảng trong (O(\log 36000)). Gốc của nó luôn lưu trữ khoảng góc tốt nhất cho cửa sổ xuyên tâm hiện đang hoạt động. Kết hợp điều này với việc quét khoảng cách sẽ cho ra thuật toán (O(n\log n+n\log 36000)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n+n\log 36000)) | (O(n+36000)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Nhân mọi góc và (\alpha) với (100), chuyển đổi dữ liệu đầu vào thành số nguyên chính xác. Điều này tránh so sánh dấu phẩy động ở các ranh giới như (10,00) so với (10,01). 
2. Sắp xếp tất cả các vật thể theo khoảng cách của chúng. Chúng tôi sẽ xử lý chúng theo thứ tự khoảng cách tăng dần và sử dụng khoảng cách của vật thể hiện tại làm điểm cuối bên phải của cửa sổ xuyên tâm đang hoạt động. 
3. Xây dựng cây phân đoạn với (36000) lá. (Các) lá thể hiện việc chọn khoảng góc để bắt đầu ở (các) góc được chia tỷ lệ. Ban đầu mọi lá đều chứa số 0 vì không có vật thể nào hoạt động. 
4. Khi phần thân có góc chia tỷ lệ (\theta) hoạt động, hãy thêm (1) vào tất cả các lần bắt đầu có thể có trong khoảng vòng tròn ([\theta-\alpha,\theta]). Mỗi lần bắt đầu như vậy sẽ chụp được phần thân này, do đó, việc tăng chính xác các giá trị đó sẽ cập nhật số lần chụp cho mỗi khoảng góc bị ảnh hưởng. 
5. Nếu khoảng thời gian cập nhật vượt qua 0, hãy chia nó thành hai phạm vi thông thường, một phạm vi kết thúc bằng (35999) và một phạm vi bắt đầu bằng 0. Nếu nó không vượt qua 0 thì chỉ cần cập nhật phạm vi thông thường là đủ. 
6. Sau khi chèn phần thân hiện tại, hãy loại bỏ mọi phần thân có khoảng cách nhỏ hơn (rd), trong đó (r) là khoảng cách của phần thân hiện tại. Sự bất đẳng thức nghiêm ngặt là có chủ ý vì khoảng bán kính bị đóng, do đó vật thể tại chính xác (rd) phải duy trì hoạt động. 
7. Đọc giá trị lớn nhất được lưu trữ ở gốc của cây phân đoạn và cập nhật câu trả lời chung. Gốc biểu thị khoảng góc tốt nhất trong số tất cả các điểm bắt đầu cho cửa sổ hướng tâm hiện tại. 
8. Tiếp tục đi qua tất cả các cơ thể. Mọi khoảng cách hướng tâm tối ưu có thể được dịch chuyển cho đến khi điểm cuối bên phải của nó đạt tới khoảng cách của một trong các vật thể được chụp của nó, do đó một trong các vị trí quét này thể hiện sự lựa chọn hướng tâm tối ưu. 

### Tại sao nó hoạt động 

Tại mỗi vị trí quét, tập hoạt động chính xác là tập hợp các vật thể có khoảng cách nằm trong khoảng đóng ([rd,r]). Đối với tập hợp bán kính cố định này, giá trị được lưu trữ tại (các) lá cây phân đoạn chính xác là số lượng vật thể hoạt động có các góc nằm trong khoảng tròn ([s,s+\alpha]). Điều này đúng bởi vì mỗi cơ thể hoạt động đều đóng góp một bản cập nhật phạm vi chính xác cho những lần bắt đầu nắm bắt nó. Do đó, gốc cây phân đoạn là số lượng phần tử hoạt động tối đa được bắt bởi bất kỳ khoảng góc nào. 

Hãy xem xét một cặp khoảng thời gian hướng tâm và góc tối ưu. Nếu khoảng hướng tâm tối ưu chứa ít nhất một vật thể, hãy di chuyển nó sang bên phải cho đến khi điểm cuối bên phải của nó đạt đến khoảng cách vật thể được chụp xa nhất. Không có vật thể bị bắt nào bị mất trong quá trình di chuyển này, do đó, có một giải pháp tốt không kém mà điểm cuối bên phải là một trong những khoảng cách được xử lý. Tại vị trí quét đó, cây phân đoạn xem xét mọi điểm bắt đầu góc có thể có và do đó tìm thấy khoảng góc thu được chính xác số lượng tối đa có thể có cho lựa chọn bán kính đó. Do đó, thuật toán kiểm tra một giải pháp ít nhất là tốt bằng mức tối ưu toàn cục, trong khi không có giá trị tính toán nào có thể vượt quá số được ghi lại bởi một số khoảng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

ANGLE_COUNT = 36000
INF = 10**30

def parse_scaled(s):
    if '.' in s:
        whole, frac = s.split('.')
        frac = (frac + '00')[:2]
    else:
        whole, frac = s, '00'
    return int(whole) * 100 + int(frac)

class SegmentTree:
    def __init__(self, n):
        size = 1
        while size < n:
            size <<= 1
        self.size = size
        self.mx = [0] * (2 * size)
        self.lazy = [0] * (2 * size)

    def _apply(self, node, value):
        self.mx[node] += value
        self.lazy[node] += value

    def _push(self, node):
        value = self.lazy[node]
        if value:
            self._apply(node * 2, value)
            self._apply(node * 2 + 1, value)
            self.lazy[node] = 0

    def _add(self, node, left, right, ql, qr, value):
        if ql <= left and right <= qr:
            self._apply(node, value)
            return

        self._push(node)
        mid = (left + right) // 2

        if ql <= mid:
            self._add(node * 2, left, mid, ql, qr, value)
        if qr > mid:
            self._add(node * 2 + 1, mid + 1, right, ql, qr, value)

        self.mx[node] = max(self.mx[node * 2], self.mx[node * 2 + 1])

    def add(self, left, right, value):
        if left > right:
            return
        self._add(1, 0, self.size - 1, left, right, value)

    def maximum(self):
        return self.mx[1]

def solve():
    n, d, alpha_text = input().split()
    n = int(n)
    d = int(d)
    alpha = parse_scaled(alpha_text)

    points = []

    for _ in range(n):
        r_text, angle_text = input().split()
        r = int(r_text)
        angle = parse_scaled(angle_text)
        points.append((r, angle))

    points.sort()

    seg = SegmentTree(ANGLE_COUNT)

    def update_angle(theta, delta):
        if alpha == 0:
            seg.add(theta, theta, delta)
            return

        left = theta - alpha

        while left < 0:
            left += ANGLE_COUNT

        if left <= theta:
            seg.add(left, theta, delta)
        else:
            seg.add(left, ANGLE_COUNT - 1, delta)
            seg.add(0, theta, delta)

    left = 0
    answer = 0

    for right in range(n):
        r, theta = points[right]

        update_angle(theta, 1)

        while points[left][0] < r - d:
            old_theta = points[left][1]
            update_angle(old_theta, -1)
            left += 1

        answer = max(answer, seg.maximum())

    print(answer)

if __name__ == "__main__":
    solve()
```Chi tiết phân tích cú pháp đầu tiên là có chủ ý. Không nên sử dụng các giá trị dấu phẩy động của Python để so sánh góc vì các giá trị thập phân như`0.01`thường không được biểu diễn chính xác trong dấu phẩy động nhị phân. Vì đầu vào có nhiều nhất hai chữ số thập phân nên nhân với (100) sẽ cho kết quả là số nguyên chính xác. 

các`points`mảng được sắp xếp theo khoảng cách, điều này mang lại cho quá trình quét cấu trúc một chiều của nó. Con trỏ`left`luôn đánh dấu phần thân đầu tiên vẫn còn bên trong cửa sổ hướng tâm hiện tại. 

chức năng`update_angle`thực hiện sự đóng góp của một cơ thể để có thể bắt đầu góc cạnh. Đối với một cơ thể ở góc`theta`, số lần bắt đầu hợp lệ nằm trong khoảng từ`theta - alpha`bởi vì`theta`. Khi phạm vi này bao quanh số 0, nó được biểu thị bằng hai bản cập nhật cây phân đoạn. 

Trường hợp đặc biệt`alpha == 0`xứng đáng có chi nhánh riêng của mình. Sự khởi đầu hợp lệ là chính xác`theta`, do đó việc cập nhật phải ảnh hưởng đến một lá thay vì vô tình tạo ra một khoảng thời gian bao bọc lớn. 

Điều kiện loại bỏ là`points[left][0] < r - d`. Chính xác là một cơ thể`r-d`thuộc về khoảng xuyên tâm khép kín và không được loại bỏ. Giải thích toàn diện tương tự đã được tích hợp vào các bản cập nhật phạm vi góc vì cả hai điểm cuối đều được cập nhật. 

Cây phân đoạn lưu trữ một giá trị lười biếng cho mỗi nút. Bản cập nhật phạm vi thay đổi mức tối đa của toàn bộ nút đó với cùng một lượng, do đó, nó có thể được áp dụng mà không giảm dần đến từng lá. Do đó, gốc sẽ cho khoảng góc tốt nhất sau mỗi lần chèn và xóa. 

Không có vấn đề tràn số nguyên trong Python. Trong ngôn ngữ có chiều rộng cố định, số lượng tối đa chỉ là (10^5), do đó số nguyên có dấu 32 bit sẽ đủ cho số lượng được lưu trữ. 

## Ví dụ đã hoạt động 

Nguồn câu lệnh liệt kê mẫu sau và đầu ra của nó là (3).```
7 80 60.00
220 20.00
360 45.00
180 45.00
200 150.00
200 75.00
180 315.00
360 225.00
```Sau khi chia tỷ lệ các góc, (\alpha=6000). Cửa sổ khoảng cách có chiều rộng (80). Trạng thái quét có thể được tóm tắt như sau. 

| Khoảng cách hiện tại | Khoảng cách hoạt động | Số góc tốt nhất hiện nay | Câu trả lời toàn cầu | 
| --- | --- | --- | --- | 
| 180 | 180, 180 | 2 | 2 | 
| 200 | 180, 180, 200, 200 | 2 | 2 | 
| 220 | 180, 180, 200, 200, 220 | 3 | 3 | 
| 360 | 360 | 1 | 3 | 

Ở khoảng cách (220), khoảng hướng tâm hoạt động là ([140,220]), do đó, nó chứa hai vật thể ở khoảng cách (180), cả hai vật thể ở khoảng cách (200) và vật thể ở (220). Cây phân đoạn tìm thấy một khoảng góc có chiều rộng (60^\circ) chứa ba trong số chúng, đưa ra câu trả lời cuối cùng (3). 

Đối với ví dụ thứ hai, hãy xem xét một ranh giới xuyên tâm và một góc bao bọc cùng một lúc.```
4 10 20.00
5 350.00
15 10.00
15 20.00
16 5.00
```Quá trình quét hoạt động như sau. 

| Khoảng cách hiện tại | Cơ thể hoạt động theo góc | Khởi đầu góc cạnh tốt nhất | Số lượng tốt nhất | 
| --- | --- | --- | --- | 
| 5 | 350 | 350 | 1 | 
| 15 | 350, 10, 20 | 350 | 2 | 
| 16 | 10, 20, 5 | 350 | 3 | 

Ở khoảng cách (15), khoảng cách xuyên tâm là ([5,15]), do đó vật thể ở khoảng cách (5) vẫn hoạt động vì ranh giới đã bao gồm. Khoảng góc bắt đầu từ (350^\circ) bao gồm (350^\circ), (10^\circ) và các vị trí bên (10^\circ) lên đến (10^\circ), nhưng không bao gồm (20^\circ), vì vậy số đếm tốt nhất là (2). Ở khoảng cách (16), vật ở khoảng cách (5) bị loại bỏ vì (5 < 16-10), để lại ba vật ở khoảng cách (15,15,16). Khoảng góc tròn giống nhau có thể nắm bắt được cả ba vì (350^\circ), (5^\circ) và (10^\circ) vừa với khoảng bao bọc (20^\circ). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n+n\log 36000)) | Chi phí sắp xếp (O(n\log n)) và mọi nội dung được chèn và xóa một lần bằng cách sử dụng các cập nhật phạm vi (O(\log 36000)). | 
| Không gian | (O(n+36000)) | Các phần được sắp xếp yêu cầu không gian (O(n)) và cây phân đoạn yêu cầu không gian (O(36000)). | 

Vì (36000) được cố định bởi độ chính xác góc hai thập phân, nên phần cây phân đoạn có hiệu quả là (O(n)) với hằng số logarit nhỏ. Hoạt động chủ yếu là sắp xếp (10^5) phần thân, dễ dàng phù hợp với các ràng buộc đã nêu. 

## Trường hợp thử nghiệm 

Đầu ra mẫu được hiển thị của câu lệnh ban đầu là (3).```python
import sys
import io

ANGLE_COUNT = 36000

def parse_scaled(s):
    if '.' in s:
        whole, frac = s.split('.')
        frac = (frac + '00')[:2]
    else:
        whole, frac = s, '00'
    return int(whole) * 100 + int(frac)

class SegmentTree:
    def __init__(self, n):
        size = 1
        while size < n:
            size <<= 1
        self.size = size
        self.mx = [0] * (2 * size)
        self.lazy = [0] * (2 * size)

    def _apply(self, node, value):
        self.mx[node] += value
        self.lazy[node] += value

    def _push(self, node):
        value = self.lazy[node]
        if value:
            self._apply(node * 2, value)
            self._apply(node * 2 + 1, value)
            self.lazy[node] = 0

    def _add(self, node, left, right, ql, qr, value):
        if ql <= left and right <= qr:
            self._apply(node, value)
            return

        self._push(node)
        mid = (left + right) // 2

        if ql <= mid:
            self._add(node * 2, left, mid, ql, qr, value)
        if qr > mid:
            self._add(node * 2 + 1, mid + 1, right, ql, qr, value)

        self.mx[node] = max(self.mx[node * 2], self.mx[node * 2 + 1])

    def add(self, left, right, value):
        if left <= right:
            self._add(1, 0, self.size - 1, left, right, value)

    def maximum(self):
        return self.mx[1]

def solve():
    input = sys.stdin.readline

    n, d, alpha_text = input().split()
    n = int(n)
    d = int(d)
    alpha = parse_scaled(alpha_text)

    points = []
    for _ in range(n):
        r_text, angle_text = input().split()
        points.append((int(r_text), parse_scaled(angle_text)))

    points.sort()

    seg = SegmentTree(ANGLE_COUNT)

    def update(theta, delta):
        if alpha == 0:
            seg.add(theta, theta, delta)
            return

        left = theta - alpha
        if left < 0:
            left += ANGLE_COUNT

        if left <= theta:
            seg.add(left, theta, delta)
        else:
            seg.add(left, ANGLE_COUNT - 1, delta)
            seg.add(0, theta, delta)

    left = 0
    answer = 0

    for right in range(n):
        r, theta = points[right]
        update(theta, 1)

        while points[left][0] < r - d:
            update(points[left][1], -1)
            left += 1

        answer = max(answer, seg.maximum())

    return str(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return solve().strip()
    finally:
        sys.stdin = old_stdin

sample1 = """\
7 80 60.00
220 20.00
360 45.00
180 45.00
200 150.00
200 75.00
180 315.00
360 225.00
"""

assert run(sample1) == "3", "sample 1"

assert run("""\
1 1 0.00
1 0.00
""") == "1", "minimum-size case"

assert run("""\
3 5 0.00
10 25.00
10 25.00
10 25.00
""") == "3", "zero angular width and duplicates"

assert run("""\
2 10 20.00
5 350.00
5 10.00
""") == "2", "angular interval wraps around 360 degrees"

assert run("""\
2 10 10.00
1 0.00
11 10.00
""") == "2", "both radial and angular boundaries are inclusive"

# Maximum-size stress case. All bodies are at the same position,
# so the answer must be n.
n = 100000
large = [f"{n} 100000 359.99\n"]
large.extend(f"1 0.00\n" for _ in range(n))
assert run("".join(large)) == str(n), "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0.00 / 1 0.00`| 1 | Kích thước đầu vào tối thiểu và chiều rộng góc bằng 0 | 
| Ba cơ thể giống hệt nhau`alpha = 0`| 3 | Cơ thể trùng lặp và khớp góc chính xác | 
| Thi thể tại`350.00`Và`10.00`với`alpha = 20.00`| 2 | Khoảng góc tròn | 
| Khoảng cách`1`Và`11`với`d = 10`| 2 | Bao gồm các ranh giới hướng tâm và góc | 
| 100000 cơ thể giống hệt nhau`alpha = 359.99`| 100000 | Kích thước đầu vào tối đa và số lượng cây phân đoạn lớn | 

## Vỏ cạnh 

Đối với chiều rộng góc bằng 0, hãy xem xét```
1 1 0.00
1 0.00
```Chiều rộng góc tỷ lệ bằng không. Khi cơ thể duy nhất được chèn vào,`update`thay đổi chính xác lá đại diện (0^\circ). Giá trị tối đa của cây phân đoạn trở thành (1), do đó thuật toán trả về (1). Không có khoảng thời gian có độ dài dương nào được tạo ra một cách ngẫu nhiên. 

Đối với gói hình tròn, hãy xem xét```
2 10 20.00
5 350.00
5 10.00
```Đối với nội dung đầu tiên, khoảng bắt đầu hợp lệ là từ (330^\circ) đến (350^\circ). Đối với phần thứ hai, chúng nằm trong khoảng từ (350^\circ) đến (10^\circ), vượt qua số 0. Bản cập nhật được chia thành các phạm vi ([350^\circ,359.99^\circ]) và ([0^\circ,10^\circ]). Do đó, điểm bắt đầu ở (350^\circ) nhận được cả hai khoản đóng góp, cho ra tối đa là (2). 

Đối với ranh giới khoảng cách bao gồm, hãy xem xét```
2 10 10.00
1 0.00
11 10.00
```Khi khoảng cách bên phải hiện tại là (11), cửa sổ xuyên tâm là ([1,11]). Điều kiện loại bỏ kiểm tra xem khoảng cách có nhỏ hơn (1) hay không, do đó vật thể ở khoảng cách (1) vẫn hoạt động. Cả hai phần thân cũng có thể nằm trên các điểm cuối của khoảng góc, do đó cây phân đoạn sẽ đếm cả hai và trả về (2). 

Đối với nội dung trùng lặp, hãy xem xét```
3 5 0.00
10 25.00
10 25.00
10 25.00
```Mỗi dòng đầu vào gây ra một bản cập nhật phạm vi riêng biệt, mặc dù cả ba bản cập nhật đều nhắm đến cùng một lá. Giá trị lá trở thành (3), biểu thị ba vật thể phát sáng riêng biệt tại tọa độ đó. Kết quả là (3). 

Logic tương tự xử lý (\ alpha=359,99). Một vật thể đóng góp vào mọi sự khởi đầu có thể ngoại trừ khoảng góc bổ sung nhỏ có chiều rộng (0,01^\circ). Vì cây phân đoạn thể hiện rõ ràng tất cả (36000) phần bắt đầu ở cấp độ một trăm, nên không có sự mơ hồ gần đúng hoặc dấu phẩy động ở giá trị cực trị này.
