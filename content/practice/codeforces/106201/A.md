---
title: "CF 106201A - \u041d\u0435\u0441\u0442\u0430\u043d\u0434\u0430\u0440\u0442\u043d\u044b\u0439 \u043f\u043e\u0434\u0445\u043e\u0434"
description: "Chúng ta được cung cấp một lưới $n nhân m$ và chúng ta được phép chọn một ô bắt đầu duy nhất trên ranh giới của lưới này. Từ ô được chọn đó, một quá trình sẽ bắt đầu lan sang tất cả bốn ô lân cận mỗi giây, giống hệt như việc mở rộng theo chiều rộng đầu tiên trên lưới."
date: "2026-06-20T22:27:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106201
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2025"
rating: 0
weight: 106201
solve_time_s: 41
verified: true
draft: false
---

[CF 106201A - \u041d\u0435\u0441\u0442\u0430\u043d\u0434\u0430\u0440\u0442\u043d\u044b\u0439 \u043f\u043e\u0434\u0445\u043e\u0434](https://codeforces.com/problemset/problem/106201/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times m$lưới và chúng ta được phép chọn một ô bắt đầu duy nhất trên ranh giới của lưới này. Từ ô được chọn đó, một quá trình sẽ bắt đầu lan sang tất cả bốn ô lân cận mỗi giây, giống hệt như việc mở rộng theo chiều rộng đầu tiên trên lưới. Thời gian cần thiết để đến bất kỳ ô nào chính xác bằng khoảng cách lưới kiểu Manhattan của nó từ ô bắt đầu đã chọn, bởi vì chỉ được phép di chuyển theo bốn hướng với đơn giá. 

Nhiệm vụ là chọn một ô biên sao cho khoảng cách tối đa từ nó đến bất kỳ ô nào trong lưới là nhỏ nhất. Nói cách khác, chúng tôi đang chọn một nguồn ở đường viền để giảm thiểu khoảng cách của ô xa nhất khi di chuyển theo 4 hướng. 

Đầu ra chỉ đơn giản là tọa độ của bất kỳ ô ranh giới tối ưu nào. 

Những ràng buộc cho phép$n, m \le 10^{18}$, điều này ngay lập tức loại trừ mọi mô phỏng hoặc truyền tải lưới. Bất kỳ giải pháp nào cũng phải là thời gian không đổi, vì ngay cả thời gian tuyến tính trong cả hai chiều đều không thể xảy ra. 

Một cách tiếp cận đơn giản là chọn một ô ranh giới ứng cử viên và tính toán khoảng cách tối đa của nó tới tất cả các ô, vốn đã yêu cầu quét lưới. Điều đó là không khả thi, và thậm chí tệ hơn, làm điều đó cho tất cả các ô ranh giới sẽ nhân chi phí lên gấp nhiều lần.$O(n + m)$, điều đó vẫn là không thể. 

Một trường hợp khó phát hiện khi$n = 1$hoặc$m = 1$. Trong các lưới suy biến như vậy, mọi ô đều nằm trên ranh giới và cấu trúc khoảng cách thu gọn thành một đường đơn giản. Một trường hợp cạnh khác là khi lưới đối xứng, chẳng hạn như$n = m$, trong đó tồn tại nhiều câu trả lời tối ưu và bất kỳ câu trả lời nào trong số đó đều hợp lệ. 

## Phương pháp tiếp cận 

Quan sát quan trọng là quá trình trải phổ hoạt động giống như bài toán khoảng cách nhiều nguồn trong đó chúng ta chọn chính xác một nguồn trên đường biên. Ô xa nhất so với điểm bắt đầu ranh giới luôn là ô “sâu nhất” bên trong lưới so với cạnh bắt đầu đó. 

Nếu chúng ta cố định ô bắt đầu ở hàng trên cùng, khoảng cách trong trường hợp xấu nhất sẽ bị hàng dưới cùng chiếm ưu thế. Tương tự, bắt đầu từ hàng dưới cùng sẽ khiến hàng trên trở thành nút cổ chai. Sự đối xứng tương tự giữ theo chiều ngang. 

Vì vậy, chiến lược tối ưu là chọn một ô biên sao cho tối thiểu khoảng cách theo chiều dọc và chiều ngang của nó đến các cạnh xa nhất. Theo trực giác, chúng tôi muốn bắt đầu ở vị trí trung tâm nhất có thể dọc theo đường biên nhưng bị hạn chế ở phần chu vi. 

Điều này làm giảm vấn đề giảm thiểu khoảng cách đến phía xa nhất. Nếu bắt đầu ở hàng trên cùng hoặc dưới cùng thì khoảng cách giới hạn là chiều cao dọc trừ đi 1. Nếu bắt đầu ở cột bên trái hoặc cột bên phải thì khoảng cách giới hạn là chiều rộng ngang trừ đi 1. Để giảm thiểu trường hợp xấu nhất, chúng ta so sánh$n$Và$m$và chọn bên có phạm vi tiếp cận tối đa nhỏ hơn. 

Nếu như$n \le m$, tốt hơn nên chọn một ô ở cột giữa của ranh giới hàng, vì độ sâu dọc nhỏ hơn hoặc bằng. Nếu như$m < n$, chúng ta chọn một ô ở hàng giữa của ranh giới cột. 

Cụ thể hơn, điểm tối ưu luôn tập trung vào trục của chiều dài hơn trong khi vẫn nằm trên ranh giới của chiều ngắn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các lần bắt đầu ranh giới + BFS) |$O(nm(n+m))$|$O(nm)$| Quá chậm | 
| Tối ưu (lý luận hình học) |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi suy luận về ô ranh giới nào giảm thiểu khoảng cách tối đa đến bất kỳ ô nào trong lưới. 

1. So sánh$n$Và$m$. Điều này quyết định xem khoảng cách theo chiều dọc hay chiều ngang chiếm ưu thế trong khoảng cách trong trường hợp xấu nhất. 
2. Nếu$n \le m$, chúng tôi ưu tiên giảm thiểu các điểm cực trị theo chiều ngang. Chúng tôi đặt ô bắt đầu ở ranh giới trên cùng hoặc dưới cùng và chọn một cột càng gần tâm càng tốt. Cột tốt nhất là$j = \lceil m/2 \rceil$, bởi vì nó giảm thiểu khoảng cách tối đa đến các cạnh trái và phải. 
3. Trong trường hợp$n \le m$, chúng ta có thể chọn một trong hai hàng$i = 1$hoặc$i = n$, vì cả hai đều đối xứng. Chúng tôi chọn$i = 1$cho sự cụ thể. 
4. Nếu$m < n$, thay vào đó chúng tôi ưu tiên giảm thiểu các điểm cực đoan theo chiều dọc. Chúng ta đặt ô bắt đầu ở ranh giới bên trái hoặc bên phải và chọn một hàng càng gần tâm càng tốt. Hàng tốt nhất là$i = \lceil n/2 \rceil$. 
5. Trong trường hợp này, chúng tôi sửa$j = 1$, vì ranh giới trái hoặc phải là đối xứng. 
6. Xuất tọa độ đã chọn. 

### Tại sao nó hoạt động 

Khoảng cách lưới đến ô xa nhất chỉ phụ thuộc vào khoảng cách từ điểm bắt đầu đến các ranh giới đối diện. Bất kỳ khoảng cách bên trong nào cách xa tâm của chiều dài đều làm tăng khoảng cách tối đa đến một bên mà không cải thiện đủ bên kia để bù đắp. Vì điểm khởi đầu phải nằm trên ranh giới nên sự cân bằng tối ưu đạt được bằng cách tập trung vào chiều mà chúng ta không “neo” vào. Điều này đảm bảo rằng khoảng cách tối đa đến góc xa nhất được giảm thiểu trong số tất cả các lựa chọn ranh giới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

if n <= m:
    i = 1
    j = (m + 1) // 2
else:
    i = (n + 1) // 2
    j = 1

print(i, j)
```Việc thực hiện trực tiếp tuân theo quan sát rằng chỉ có kích thước tương đối của$n$Và$m$vấn đề. Khi các hàng ít hơn hoặc bằng nhau, chúng ta cố định một hàng ranh giới và căn giữa cột. Khi số cột ít hơn, chúng tôi sửa một cột ranh giới và căn giữa hàng. 

biểu hiện$(m + 1) // 2$tính toán chính xác cột giữa với cách xử lý thích hợp cho cả chiều rộng lẻ và chiều rộng chẵn, đảm bảo độ lệch tối đa tối thiểu ở hai bên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 5
```Chúng tôi so sánh$n = 2$Và$m = 5$, Vì thế$n \le m$. Chúng tôi sửa chữa$i = 1$và chọn cột giữa$j = 3$. 

| Bước | n | m | Quyết định | tôi | j | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 5 | n ≤ m | 1 | - | 
| 2 | 2 | 5 | cột trung tâm | 1 | 3 | 

Đầu ra:```
1 3
```Lựa chọn này giảm thiểu các điểm cực trị theo chiều ngang, vì cột 3 có khoảng cách bằng nhau đối với cả hai bên. 

### Ví dụ 2 

đầu vào:```
4 31
```Ở đây một lần nữa$n \le m$, vì vậy chúng ta ở trên hàng ranh giới và căn giữa cột. 

| Bước | n | m | Quyết định | tôi | j | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 31 | n ≤ m | 1 | - | 
| 2 | 4 | 31 | cột trung tâm | 1 | 16 | 

Đầu ra:```
1 16
```Điều này đặt điểm bắt đầu ở điểm giữa của chiều dài, giảm thiểu khoảng cách theo chiều ngang trong trường hợp xấu nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một vài phép tính số học và so sánh | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu bổ sung | 

Giải pháp dễ dàng phù hợp trong giới hạn vì nó không phụ thuộc vào kích thước lưới và chỉ thực hiện số học theo thời gian không đổi ngay cả đối với các giá trị tối đa lên tới$10^{18}$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())

    if n <= m:
        i = 1
        j = (m + 1) // 2
    else:
        i = (n + 1) // 2
        j = 1

    return f"{i} {j}"

# provided samples
assert run("2 5") == "1 3"
assert run("4 31") == "1 16"

# edge: single row
assert run("1 7") == "1 4"

# edge: single column
assert run("6 1") == "3 1"

# square grid
assert run("5 5") == "1 3"

# large asymmetric
assert run("1000000000000000000 2") == "500000000000000000 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 7 | 1 4 | thoái hóa ranh giới một hàng | 
| 6 1 | 3 1 | thoái hóa ranh giới cột đơn | 
| 5 5 | 1 3 | xử lý lưới đối xứng | 
| 1e18 2 | độ chính xác của việc lựa chọn hàng giữa | kích thước cực lớn và xử lý số nguyên | 

## Vỏ cạnh 

Khi nào$n = 1$, lưới thực sự là một đường. Mọi ô đều nằm trên ranh giới, nhưng ô xa nhất so với điểm bắt đầu được chọn luôn là điểm cuối. Thuật toán chọn$i = 1$và căn giữa cột, giúp giảm thiểu chính xác khoảng cách tối đa đến cả hai đầu của đường. 

Khi$m = 1$, tình huống là đối xứng. Thuật toán chọn$j = 1$và căn giữa hàng, đảm bảo khoảng cách bằng nhau đến các điểm cuối trên và dưới. 

Khi$n = m$, cả hai nhánh đều là lựa chọn hợp lệ. Thuật toán chọn hàng trên cùng và cột giữa một cách nhất quán, điều này là tối ưu vì mọi lựa chọn lấy ranh giới làm trung tâm đều có cùng khoảng cách tối đa.
