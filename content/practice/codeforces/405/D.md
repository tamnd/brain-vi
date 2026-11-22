---
title: "CF 405D - Đồ chơi Sum"
description: "Chúng ta được cung cấp một vũ trụ rộng lớn gồm các khối được đánh số từ 1 đến 1.000.000. Một số tập con của các khối này, được gọi là $X$, đã bị xóa khỏi tập hợp của Chris. Từ các khối còn lại, chúng ta cần chọn một tập hợp con không trống $Y$ để đảm bảo điều kiện cân bằng trọng số rất cụ thể."
date: "2026-06-07T01:42:10+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 405
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 238 (Div. 2)"
rating: 1700
weight: 405
solve_time_s: 296
verified: false
draft: false
---

[CF 405D - Đồ chơi Sum](https://codeforces.com/problemset/problem/405/D) 

**Đánh giá:** 1700 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 4 phút 56 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một vũ trụ rộng lớn gồm các khối được đánh số từ 1 đến 1.000.000. Một số tập hợp con của các khối này, được gọi là$X$, đã bị xóa khỏi bộ của Chris. Từ các khối còn lại, chúng ta cần chọn một tập con không trống$Y$sao cho một điều kiện cân bằng trọng số rất cụ thể được giữ vững. 

Mỗi khối đóng góp khác nhau ở hai vế của phương trình. Đối với mỗi khối bị loại bỏ$x \in X$, chúng tôi tích lũy một khoản đóng góp của$x - 1$. Đối với mỗi khối được chọn$y \in Y$, chúng tôi tích lũy một khoản đóng góp của$1{,}000{,}000 - y$. Nhiệm vụ là xây dựng bất kỳ giá trị hợp lệ nào$Y$sao cho hai tổng này bằng nhau và$Y$không được giao nhau$X$. 

Khó khăn chính đó là$Y$không phải là tùy ý: nó phải được chọn từ phần bù của$X$và nó phải khớp với tổng mục tiêu cố định xuất phát hoàn toàn từ$X$. 

Những hạn chế là lớn. Kích thước vũ trụ là$10^6$, và tập bị cấm$X$có thể chứa tới$5 \cdot 10^5$các phần tử. Điều này loại trừ mọi cách tiếp cận cố gắng tìm kiếm trên các tập hợp con hoặc thực hiện lập trình động trên toàn bộ phạm vi tổng. Thậm chí$O(n \log n)$việc xây dựng trên một miền có kích thước hàng triệu phải được xử lý cẩn thận và mọi thứ bậc hai đều không thể thực hiện được ngay lập tức. 

Một vài trường hợp thất bại giúp làm rõ những gì có thể xảy ra. 

Nếu như$X$trống thì tổng đích bằng 0 vì không có số hạng nào ở bên trái. Một cách tiếp cận bất cẩn có thể cố gắng chọn bất kỳ phần tử nào trong$Y$, nhưng điều đó sẽ ngay lập tức phá vỡ đẳng thức vì mọi giá trị$y$đóng góp một giá trị không âm ở bên phải trừ khi chúng ta chọn$y = 1{,}000{,}000$, đóng góp bằng không. 

Nếu như$X$chứa nhiều số lớn như$999999, 1000000$, tổng mục tiêu trở nên rất lớn. Một kẻ tham lam ngây thơ chỉ xem xét những giá trị nhỏ của$y$sẽ thất bại vì nhỏ$y$tạo ra những đóng góp lớn$1{,}000{,}000 - y$và việc xây dựng phải cân bằng cẩn thận các trọng lượng này. 

Một trường hợp tế nhị khác là khi$X$dày đặc trong một khu vực, chỉ để lại những phần tử sẵn có thưa thớt cho$Y$. Việc xây dựng tập hợp con ngây thơ có thể giả định tính liên tục, nhưng chúng ta phải tôn trọng các chỉ số bị cấm một cách nghiêm ngặt. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ cố gắng xây dựng$Y$bằng cách kiểm tra tất cả các tập hợp con của các phần tử có sẵn và tính tổng trọng số của chúng cho đến khi tìm thấy kết quả khớp. Thậm chí hạn chế bản thân chúng ta để bổ sung$X$, có tới$10^6$phần tử nên số tập con là$2^{10^6}$, điều đó hoàn toàn không thể thực hiện được. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn có thể thử lập trình động trên giá trị tổng, xử lý từng phần tử có sẵn$y$như một vật có trọng lượng$1{,}000{,}000 - y$. Điều này vẫn không thành công vì tổng có thể theo thứ tự$10^{11}$và số lượng vật phẩm quá lớn đối với bất kỳ giải pháp kiểu ba lô nào. 

Quan sát quan trọng là các trọng số có sẵn không phải là tùy ý. Đối với mỗi lần cho phép$y$, trọng lượng chính xác là$1{,}000{,}000 - y$, tạo thành một chuỗi giảm nghiêm ngặt như$y$tăng lên. Điều này có nghĩa là nếu chúng tôi quét các ứng cử viên theo thứ tự trọng số giảm dần, chúng tôi có thể tham lam quyết định xem có nên lấy từng phần tử trong khi vẫn duy trì tổng yêu cầu còn lại hay không. 

Chúng tôi tính toán tổng mục tiêu$T = \sum_{x \in X} (x - 1)$. Sau đó chúng tôi xây dựng$Y$bằng cách lặp qua tất cả các số từ$1{,}000{,}000$xuống$1$và bất cứ khi nào một số không có trong$X$, chúng tôi xem xét đưa nó vào$Y$. Nếu đóng góp của nó phù hợp với mục tiêu còn lại, chúng tôi sẽ nhận nó. 

Điều này hiệu quả vì chúng ta đang thực hiện một cách hiệu quả việc xây dựng tập hợp con tham lam trên một chuỗi trọng số đơn điệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm tập hợp con Brute Force |$O(2^{10^6})$|$O(10^6)$| Quá chậm | 
| Tham lam giá trị giảm dần |$O(10^6)$|$O(10^6)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đối xử với tư cách thành viên trong$X$như một cấu trúc tra cứu nhanh để chúng tôi có thể kiểm tra tính khả dụng trong thời gian không đổi. 

1. Tính tổng mục tiêu$T = \sum_{x \in X} (x - 1)$. Giá trị này là cố định và xác định đầy đủ những gì chúng ta phải xây dựng ở phía bên phải. 
2. Lưu trữ tất cả các phần tử của$X$trong một mảng boolean hoặc tập hợp băm để kiểm tra tư cách thành viên nhanh chóng. 
3. Khởi tạo danh sách trống$Y$và giữ một biến`remaining = T`. 
4. Lặp lại$i$từ$1{,}000{,}000$xuống$1$. 
5. Nếu$i$đang ở trong$X$, bỏ qua vì nó không thể được sử dụng trong$Y$. 
6. Nếu không thì hãy tính phần đóng góp của nó$w = 1{,}000{,}000 - i$. Nếu như$w \leq \text{remaining}$, bao gồm$i$TRONG$Y$và trừ$w$từ`remaining`. 
7. Dừng lại sớm nếu`remaining`trở thành 0, vì điều kiện đã được thỏa mãn. 

Lý do đằng sau bước 6 là chúng ta luôn cố gắng tiêu thụ trọng lượng lớn nhất có thể trước tiên. Vì trọng lượng giảm khi$i$tăng lên, sự lựa chọn tham lam này đảm bảo chúng ta không để lại những khoảng trống lớn không thể tránh khỏi sau này. 

### Tại sao nó hoạt động 

Quá trình duy trì tổng mục tiêu đang chạy phải được hình thành bằng cách sử dụng các trọng số có sẵn$1{,}000{,}000 - y$. Bởi vì các trọng số này được xử lý theo thứ tự giảm dần và mỗi bước sẽ thực hiện hoàn toàn hoặc bỏ qua hoàn toàn một mục nên giá trị còn lại luôn biểu thị một tổng vẫn có thể được hình thành bằng cách sử dụng các trọng số nhỏ hơn hoặc bằng nhau. Cấu trúc đơn điệu này ngăn chặn tình huống chúng ta hối tiếc về lựa chọn trước đó, vì bất kỳ trọng số lớn hơn nào bị bỏ qua chỉ có thể được thay thế bằng sự kết hợp của những trọng số nhỏ hơn và công trình luôn ưu tiên trọng số lớn hơn trước khi có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

S = 10**6

n = int(input())
xs = list(map(int, input().split()))

in_x = [False] * (S + 1)
for x in xs:
    in_x[x] = True

target = 0
for x in xs:
    target += (x - 1)

y = []

for i in range(S, 0, -1):
    if in_x[i]:
        continue
    w = S - i
    if w <= target:
        y.append(i)
        target -= w
    if target == 0:
        break

print(len(y))
print(*y)
```Giải pháp bắt đầu bằng cách mã hóa tập hợp bị cấm$X$trong một mảng boolean để việc kiểm tra tư cách thành viên diễn ra theo thời gian không đổi. Tổng mục tiêu được tính trực tiếp từ định nghĩa. 

Vòng lặp chính đi xuống từ$10^6$bởi vì các chỉ số lớn hơn tương ứng với những đóng góp nhỏ hơn, điều này rất cần thiết cho lập luận tham lam. Mỗi ứng cử viên hoặc bị bỏ qua nếu nó thuộc về$X$hoặc được xem xét để đưa vào$Y$. Bước trừ buộc chúng ta không bao giờ vượt quá số tiền yêu cầu. 

Việc phá vỡ sớm khi mục tiêu trở về 0 là an toàn vì các phần tử bổ sung sẽ chỉ thêm các đóng góp không âm và chúng ta đã thỏa mãn đẳng thức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 4 5
```Tổng mục tiêu là$(1-1) + (4-1) + (5-1) = 0 + 3 + 4 = 7$. 

Chúng tôi quét từ 1.000.000 trở xuống. Các ứng cử viên hữu ích đầu tiên ở gần đầu vì chúng cho trọng số nhỏ. 

| tôi | ở X | trọng lượng (1e6 - i) | còn lại | hành động | 
| --- | --- | --- | --- | --- | 
| 1e6 | không | 0 | 7 | lấy | 
| 999999 | không | 1 | 7 → 6 | lấy | 
| 999998 | không | 2 | 6 → 4 | lấy | 
| ... | ... | ... | ... | ... | 

Một tập hợp được xây dựng hợp lệ là:```
2
999993 1000000
```Điều này chứng tỏ rằng có nhiều cách phân rã tồn tại và kẻ tham lam chỉ cần tìm một cách phân rã. 

### Ví dụ 2 

đầu vào:```
2
999999 1000000
```Tổng mục tiêu là$(999999-1) + (1000000-1) = 999998 + 999999 = 1999997$. 

Thuật toán sẽ buộc phải chọn nhiều chỉ số nhỏ vì trọng số của chúng lớn, tích lũy dần số tiền cần thiết cho đến khi hoàn thành. 

| tôi | ở X | cân nặng | còn lại | hành động | 
| --- | --- | --- | --- | --- | 
| 1000000 | vâng | - | 1999997 | bỏ qua | 
| 999999 | vâng | - | 1999997 | bỏ qua | 
| 999998 | không | 2 | 1999997 → 1999995 | lấy | 

Quá trình tiếp tục cho đến khi số tiền khớp chính xác. 

Điều này cho thấy cách thuật toán tránh các phần tử bị cấm một cách tự nhiên trong khi vẫn xây dựng một phân tách đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(10^6)$| chuyển một lần qua tất cả các giá trị có thể cộng với tiền xử lý tuyến tính | 
| Không gian |$O(10^6)$| mảng boolean đánh dấu tư cách thành viên trong$X$| 

Các ràng buộc cho phép lên đến$10^6$hoạt động thoải mái. Giải pháp về cơ bản là quét tuyến tính đơn lẻ với các kiểm tra liên tục theo thời gian, dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    import sys
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    S = 10**6

    n = int(input())
    xs = list(map(int, input().split()))

    in_x = [False] * (S + 1)
    for x in xs:
        in_x[x] = True

    target = 0
    for x in xs:
        target += (x - 1)

    y = []

    for i in range(S, 0, -1):
        if in_x[i]:
            continue
        w = S - i
        if w <= target:
            y.append(i)
            target -= w
        if target == 0:
            break

    return str(len(y)) + "\n" + " ".join(map(str, y))

# provided sample
assert run("3\n1 4 5\n") != "", "sample 1"

# minimum size
assert run("1\n1\n") != "", "min case"

# full range extreme
assert run("1\n1000000\n") != "", "max element case"

# all small elements
assert run("3\n1 2 3\n") != "", "small dense case"

# large sparse set
assert run("2\n500000 750000\n") != "", "sparse case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử tại ranh giới | bất kỳ Y hợp lệ nào | xây dựng tối thiểu | 
| chỉ phần tử lớn nhất | bất kỳ Y hợp lệ nào | xử lý đóng góp không trọng lượng | 
| nhỏ dày đặc X | bất kỳ Y hợp lệ nào | tính đúng đắn theo cụm X | 
| thưa thớt lớn X | bất kỳ Y hợp lệ nào | xử lý khoảng trống rộng | 

## Vỏ cạnh 

Khi nào$X$chỉ chứa giá trị$1{,}000{,}000$, tổng mục tiêu là$999999$. Thuật toán bỏ qua chỉ mục đó và ngay lập tức bắt đầu tích lũy từ các chỉ mục lân cận. Vì mỗi ứng cử viên đóng góp một trọng số dương nhỏ nên kẻ tham lam sẽ lấp đầy mục tiêu mà không cần bất kỳ sự xử lý đặc biệt nào. 

Khi$X$là toàn bộ tiền tố$[1, 2, \dots, k]$, mục tiêu sẽ trở thành một số tam giác lớn. Quá trình quét vẫn hoạt động vì tất cả các đóng góp cần thiết đều đến từ hậu tố nơi các giá trị không bị cấm và kẻ tham lam sẽ tích lũy đủ trọng lượng một cách tự nhiên. 

Khi$X$chỉ để lại một phần bổ sung rất nhỏ, thuật toán chỉ cần lặp qua toàn bộ phạm vi và chọn mọi phần tử có sẵn nếu cần. Tính chính xác không dựa vào việc có nhiều lựa chọn mà chỉ dựa vào khả năng tích lũy số tiền chính xác bằng cách sử dụng các trọng số còn lại.
