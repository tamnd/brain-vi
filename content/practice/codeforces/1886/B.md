---
title: "CF 1886B - Nỗi sợ bóng tối"
description: "Monocarp bắt đầu tại điểm gốc $(0,0)$ và muốn về đích tại điểm $P$. Vùng được chiếu sáng duy nhất đến từ hai chiếc đèn lồng nằm ở các điểm $A$ và $B$. Cả hai đèn phải sử dụng cùng một công suất $w$, có nghĩa là mỗi đèn chiếu sáng một đĩa có bán kính $w$."
date: "2026-06-08T22:15:18+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1886
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 156 (Rated for Div. 2)"
rating: 1200
weight: 1886
solve_time_s: 131
verified: true
draft: false
---

[CF 1886B - Nỗi sợ bóng tối](https://codeforces.com/problemset/problem/1886/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, hình học, toán học 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Monocarp bắt đầu từ điểm gốc$(0,0)$và muốn đến nhà anh ấy vào thời điểm$P$. Các vùng được chiếu sáng duy nhất đến từ hai chiếc đèn lồng nằm ở các điểm$A$Và$B$. Cả hai đèn lồng phải sử dụng công suất như nhau$w$, có nghĩa là mỗi chiếc đèn lồng chiếu sáng một đĩa có bán kính$w$. 

Chúng ta cần giá trị nhỏ nhất của$w$sao cho tồn tại một đường đi liên tục từ điểm gốc đến ngôi nhà không bao giờ rời khỏi vùng được chiếu sáng. 

Vùng được chiếu sáng đơn giản là sự kết hợp của hai vòng tròn có bán kính bằng nhau$w$. Một đường dẫn hợp lệ tồn tại nếu cả hai điểm cuối, điểm gốc và ngôi nhà, thuộc về cùng một thành phần được chiếu sáng được kết nối. 

Tọa độ nhiều nhất là$1000$về độ lớn và có tới$10^4$trường hợp thử nghiệm. Bất kỳ giải pháp nào thực hiện công việc quan trọng cho mỗi trường hợp thử nghiệm đều sẽ lãng phí. Vì chỉ có bốn điểm liên quan,$O$,$P$,$A$, Và$B$, chúng ta sẽ mong đợi một nghiệm hình học theo thời gian không đổi. 

Một số tình huống rất dễ bị bỏ qua. 

Giả sử cả nguồn gốc và ngôi nhà đều được bao phủ bởi cùng một chiếc đèn lồng. Ví dụ:```
P=(2,0)
A=(1,0)
B=(100,100)
```Câu trả lời đơn giản là lớn hơn$AO$Và$AP$. Giải pháp luôn cố gắng sử dụng cả hai đèn lồng sẽ đánh giá quá cao kết quả. 

Một trường hợp phức tạp khác xảy ra khi nguồn gốc bị che bởi một chiếc đèn lồng và ngôi nhà bị che bởi chiếc đèn kia. Sau đó, hai vòng tròn được chiếu sáng phải giao nhau hoặc chạm nhau để Monocarp có thể chuyển từ vùng được chiếu sáng này sang vùng được chiếu sáng khác. Ví dụ:```
P=(10,0)
A=(0,0)
B=(10,0)
```Bán kính tối thiểu là$5$, không$0$. Mỗi điểm cuối đều đã ở trung tâm đèn lồng, nhưng các vòng tròn phải kết nối. 

Chạm vào được tính là đã kết nối vì ranh giới vòng tròn được chiếu sáng. Nếu khoảng cách giữa các tâm đèn lồng chính xác$2w$, các vòng tròn gặp nhau tại một điểm và đường đi vẫn tồn tại. 

## Phương pháp tiếp cận 

Ý tưởng đầu tiên tự nhiên là tìm kiếm nhị phân trên câu trả lời. 

Đối với bán kính cố định$w$, chúng ta có thể kiểm tra xem có tồn tại một đường dẫn được chiếu sáng hợp lệ hay không. Gốc thuộc đường tròn$A$nếu như$AO \le w$, và tương tự cho tất cả các mối quan hệ điểm-vòng tròn khác. Hai vòng tròn được kết nối bất cứ khi nào$AB \le 2w$. Điều này tạo ra một biểu đồ nhỏ bao gồm điểm gốc, ngôi nhà và hai đường tròn. Chúng ta có thể xác định xem nguồn gốc và ngôi nhà có nằm trong cùng một thành phần được chiếu sáng được kết nối hay không. 

Việc kiểm tra mất thời gian không đổi và tìm kiếm nhị phân trên số thực yêu cầu khoảng sáu mươi lần lặp để đạt được độ chính xác cao hơn mức cần thiết. Điều này đã trôi qua một cách thoải mái. 

Điều quan trọng là chỉ có hai chiếc đèn lồng. Bất kỳ đường dẫn hợp lệ nào cũng phải thuộc một trong số rất ít cấu hình. 

Hoặc chỉ một chiếc đèn lồng che phủ cả hai điểm cuối, hoặc điểm gốc được che phủ bởi một chiếc đèn lồng và ngôi nhà bị che phủ bởi chiếc đèn kia. Không có khả năng nào khác. 

Nếu một đèn lồng duy nhất bao phủ cả hai điểm cuối thì bán kính yêu cầu chỉ đơn giản là khoảng cách tối đa của nó đến hai điểm cuối. 

Nếu các đèn lồng khác nhau chịu trách nhiệm cho hai điểm cuối thì phải có ba điều kiện đồng thời: 

1. Chiếc đèn lồng đầu tiên bao phủ điểm cuối của nó. 
2. Đèn lồng thứ hai che điểm cuối của nó. 
3. Hai đường tròn cắt nhau. 

Bán kính yêu cầu là tối đa trong số ba yêu cầu đó. 

Vì chỉ có hai phép gán đèn lồng cho điểm cuối, nên chúng ta có thể tính toán trực tiếp tất cả các ứng cử viên và lấy mức tối thiểu. Điều này loại bỏ hoàn toàn nhu cầu tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm nhị phân + Kiểm tra tính khả thi | O(độ chính xác của nhật ký) | O(1) | Đã chấp nhận | 
| Công thức hình học trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán tất cả các khoảng cách có liên quan: 

-$AO, AP, BO, BP,$Và$AB$. 
2. Xét trường hợp đèn lồng$A$chỉ bao gồm cả nguồn gốc và ngôi nhà. 

Bán kính cần tìm là:$$\max(AO, AP)$$3. Xét trường hợp đèn lồng$B$chỉ bao gồm cả nguồn gốc và ngôi nhà. 

Bán kính cần tìm là:$$\max(BO, BP)$$4. Xét trường hợp nguồn gốc bị đèn lồng che khuất$A$và ngôi nhà bằng đèn lồng$B$. 

Nguồn gốc yêu cầu$AO \le w$, ngôi nhà yêu cầu$BP \le w$và các vòng tròn phải giao nhau, điều này đòi hỏi:$$AB \le 2w$$Kể từ đây:$$w=\max\left(AO,\ BP,\ \frac{AB}{2}\right)$$5. Xét phép gán đối xứng trong đó gốc tọa độ được bao phủ bởi đèn lồng$B$và ngôi nhà bằng đèn lồng$A$. 

Bán kính cần tìm là:$$w=\max\left(BO,\ AP,\ \frac{AB}{2}\right)$$6. Câu trả lời là bán kính nhỏ nhất trong số bốn bán kính ứng cử viên. 

### Tại sao nó hoạt động 

Vùng được chiếu sáng là sự kết hợp của chính xác hai đĩa. Bất kỳ đường đi nào từ điểm gốc tới ngôi nhà đều phải bắt đầu từ một đĩa chứa điểm gốc và kết thúc ở đĩa chứa ngôi nhà. 

Nếu cả hai điểm cuối đều nằm trong cùng một đĩa thì chỉ riêng đĩa đó sẽ cung cấp tuyến đường được chiếu sáng được kết nối. Điều này mang lại hai ứng cử viên đầu tiên. 

Nếu không, nguồn gốc và ngôi nhà phải nằm ở các đĩa khác nhau. Vì chỉ tồn tại hai đĩa nên Monocarp phải di chuyển từ đĩa này sang đĩa kia. Điều đó có thể thực hiện được chính xác khi các đĩa giao nhau hoặc chạm vào nhau. Bán kính phải đồng thời đáp ứng yêu cầu về phạm vi bao phủ cho hai điểm cuối và yêu cầu về giao lộ. Điều này mang lại hai ứng cử viên còn lại. 

Mọi cấu hình hợp lệ có thể thuộc về một trong bốn trường hợp này và mỗi ứng cử viên là bán kính nhỏ nhất làm cho trường hợp của nó khả thi. Lấy giá trị tối thiểu trong mọi trường hợp sẽ mang lại giá trị tối ưu toàn cục. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def dist(x1, y1, x2, y2):
    return math.hypot(x1 - x2, y1 - y2)

t = int(input())

for _ in range(t):
    px, py = map(int, input().split())
    ax, ay = map(int, input().split())
    bx, by = map(int, input().split())

    ao = dist(ax, ay, 0, 0)
    ap = dist(ax, ay, px, py)

    bo = dist(bx, by, 0, 0)
    bp = dist(bx, by, px, py)

    ab = dist(ax, ay, bx, by)

    ans = min(
        max(ao, ap),
        max(bo, bp),
        max(ao, bp, ab / 2.0),
        max(bo, ap, ab / 2.0),
    )

    print(f"{ans:.10f}")
```Phần đầu tiên tính toán năm khoảng cách mô tả đầy đủ hình học liên quan đến bài toán. 

Bốn biểu thức tiếp theo tương ứng chính xác với bốn mẫu kết nối có thể được thảo luận trong chứng minh. 

Điểm tinh tế là sự xuất hiện của`ab / 2.0`. Hai đường tròn có bán kính bằng nhau$w$giao nhau khi$AB \le 2w$, tương đương với$w \ge AB/2$. Quên yếu tố thứ hai này là sai lầm phổ biến nhất. 

sử dụng`math.hypot`tránh bình phương và lấy căn bậc hai theo cách thủ công, trong khi vẫn ổn định về mặt số lượng. 

In mười chữ số thập phân dễ dàng đáp ứng yêu cầu$10^{-6}$độ chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1, Test Case 1```
P = (3,3)
A = (1,0)
B = (-1,6)
```| Số lượng | Giá trị | 
| --- | --- | 
| AO | 1.000000 | 
| AP | 3.605551 | 
| BƠ | 6.082763 | 
| BP | 5.000000 | 
| AB | 6.324555 | 
| tối đa(AO, AP) | 3.605551 | 
| tối đa(BO, BP) | 6.082763 | 
| tối đa(AO, BP, AB/2) | 5.000000 | 
| tối đa(BO, AP, AB/2) | 6.082763 | 

Trả lời:$$\min(3.605551, 6.082763, 5.000000, 6.082763) = 3.605551$$Giải pháp tối ưu sử dụng đèn lồng$A$một mình. Nó đã bao gồm cả hai điểm cuối. 

### Mẫu 1, Test Case 2```
P = (3,3)
A = (-1,-1)
B = (4,3)
```| Số lượng | Giá trị | 
| --- | --- | 
| AO | 1.414214 | 
| AP | 5.656854 | 
| BƠ | 5.000000 | 
| BP | 1.000000 | 
| AB | 6.403124 | 
| tối đa(AO, AP) | 5.656854 | 
| tối đa(BO, BP) | 5.000000 | 
| tối đa(AO, BP, AB/2) | 3.201562 | 
| tối đa(BO, AP, AB/2) | 5.656854 | 

Trả lời:$$\min(5.656854, 5.000000, 3.201562, 5.656854) = 3.201562$$Ở đây nguồn gốc được bao phủ bởi đèn lồng$A$, ngôi nhà bằng đèn lồng$B$và các vòng tròn gặp nhau chính xác khi bán kính đạt tới$AB/2$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng tính toán và so sánh khoảng cách cố định | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Mỗi trường hợp thử nghiệm thực hiện một lượng tính toán hình học không đổi. Ngay cả với$10^4$trường hợp thử nghiệm, thời gian chạy không đáng kể và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys
import math

def solve():
    input = sys.stdin.readline

    def dist(x1, y1, x2, y2):
        return math.hypot(x1 - x2, y1 - y2)

    t = int(input())
    out = []

    for _ in range(t):
        px, py = map(int, input().split())
        ax, ay = map(int, input().split())
        bx, by = map(int, input().split())

        ao = dist(ax, ay, 0, 0)
        ap = dist(ax, ay, px, py)
        bo = dist(bx, by, 0, 0)
        bp = dist(bx, by, px, py)
        ab = dist(ax, ay, bx, by)

        ans = min(
            max(ao, ap),
            max(bo, bp),
            max(ao, bp, ab / 2.0),
            max(bo, ap, ab / 2.0),
        )

        out.append(f"{ans:.10f}")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# provided sample
out = run(
"""2
3 3
1 0
-1 6
3 3
-1 -1
4 3
"""
).strip().splitlines()

assert abs(float(out[0]) - 3.6055512755) < 1e-6
assert abs(float(out[1]) - 3.2015621187) < 1e-6

# single lantern A covers everything
out = float(run(
"""1
2 0
1 0
100 100
"""
).strip())
assert abs(out - 1.0) < 1e-6

# symmetric transfer through touching circles
out = float(run(
"""1
10 0
0 0
10 0
"""
).strip())
assert abs(out - 5.0) < 1e-6

# choose lantern B alone
out = float(run(
"""1
1 1
100 100
0 1
"""
).strip())
assert abs(out - 1.0 < 1e-6)

# negative coordinates
out = float(run(
"""1
-3 -4
-1 0
-4 -3
"""
).strip())
assert out > 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nhà gần đèn A, đèn B xa | 1.0 | Giải pháp đèn lồng đơn | 
| Đèn lồng ở điểm cuối của một đoạn | 5.0 | Yêu cầu giao lộ vòng tròn | 
| Lantern B bao gồm cả hai điểm cuối | 1.0 | Sự đối xứng giữa A và B | 
| Tọa độ âm | Câu trả lời hữu hạn tích cực | Tính toán khoảng cách trong tất cả các góc phần tư | 

## Vỏ cạnh 

### Chỉ một chiếc đèn lồng là đủ 

đầu vào:```
1
2 0
1 0
100 100
```Khoảng cách là:```
AO = 1
AP = 1
```đèn lồng$A$bao gồm cả hai điểm cuối với bán kính$1$. Thuật toán tính toán`max(AO, AP)=1`và trả lại nó. Bất kỳ cách tiếp cận nào buộc cả hai chiếc đèn lồng tham gia sẽ tạo ra một câu trả lời lớn hơn. 

### Các đèn lồng khác nhau che phủ hai điểm cuối 

đầu vào:```
1
10 0
0 0
10 0
```Nguồn gốc nằm ở đèn lồng$A$trung tâm của và ngôi nhà nằm ở đèn lồng$B$trung tâm của. Chỉ riêng phạm vi bảo hiểm đã gợi ý bán kính$0$, nhưng các vùng được chiếu sáng sẽ bị ngắt kết nối. 

Thuật toán cũng kiểm tra$AB/2 = 5$, sản xuất:```
max(AO, BP, AB/2) = 5
```đó là bán kính tối thiểu thực sự. 

### Vòng kết nối chỉ chạm vào 

đầu vào:```
1
4 0
0 0
4 0
```Bán kính tối ưu là$2$. 

Các đường tròn gặp nhau tại đúng một điểm vì:$$AB = 4 = 2 \times 2$$Chạm vào là đủ vì ranh giới được chiếu sáng. Công thức xử lý việc này một cách tự nhiên vì nó sử dụng điều kiện không nghiêm ngặt$AB \le 2w$. 

### Bài tập đối xứng 

đầu vào:```
1
3 3
4 3
-1 -1
```Đây là mẫu thứ hai có nhãn đèn lồng được hoán đổi. 

Thuật toán đánh giá cả hai bài tập hỗn hợp:$$\max(AO, BP, AB/2)$$Và$$\max(BO, AP, AB/2)$$vì vậy nó vẫn đúng bất kể chiếc đèn lồng nào che gốc và chiếc đèn nào che ngôi nhà.
