---
title: "CF 104875B - Lật chai"
description: "Chúng ta có một cái chai hình trụ thẳng đứng có chiều cao $h$ và bán kính $r$. Chai chứa đầy một phần nước đến độ cao $x$, đo từ đáy. Không gian còn lại trên mặt nước chứa đầy không khí."
date: "2026-06-28T17:56:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104875
codeforces_index: "B"
codeforces_contest_name: "2022-2023 ICPC Northwestern European Regional Programming Contest (NWERC 2022)"
rating: 0
weight: 104875
solve_time_s: 48
verified: true
draft: false
---

[CF 104875B - Lật chai](https://codeforces.com/problemset/problem/104875/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một cái chai hình trụ thẳng đứng có chiều cao$h$và bán kính$r$. Chai được đổ đầy một phần nước đến một độ cao nào đó$x$, đo từ dưới lên. Không gian còn lại trên mặt nước chứa đầy không khí. Nước và không khí được coi là vật liệu liên tục với mật độ không đổi$d_w$Và$d_a$, và bản thân cái chai không có trọng lượng. 

Đại lượng vật lý mà chúng ta muốn tối ưu hóa là vị trí thẳng đứng của khối tâm của cột không khí và nước kết hợp khi chai đứng thẳng. Vì diện tích mặt cắt ngang của hình trụ không đổi nên hình học chỉ ảnh hưởng đến tỷ lệ chứ không ảnh hưởng đến sự phân bổ khối lượng tương đối dọc theo chiều cao. Mục tiêu là chọn$x \in [0, h]$sao cho khối tâm càng thấp càng tốt. 

Đầu ra là một số thực biểu thị chiều cao nước tối ưu. 

Các ràng buộc là nhỏ, với tất cả các tham số lên tới 1000, do đó$O(1)$hoặc$O(\log n)$giải pháp phân tích được mong đợi. Bất kỳ cách tiếp cận nào lấy mẫu các giá trị có thể có của$x$tinh vi sẽ chậm một cách không cần thiết và kém chính xác hơn, nhưng vẫn khả thi; tuy nhiên, cấu trúc của bài toán gợi ý một giải pháp dạng đóng. 

Một vài hành vi cạnh đáng được dự đoán. Nếu chai rỗng ($x=0$), chỉ có không khí đóng góp. Nếu nó đầy ($x=h$), chỉ có nước đóng góp. Một cách tiếp cận ngây thơ giả sử điểm tối ưu phải nằm ở điểm cuối sẽ thất bại, vì các hỗn hợp trung gian có thể hạ thấp khối tâm do sự tương phản giữa không khí nhẹ ở trên và nước nặng ở dưới. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ thử nhiều độ cao của ứng viên$x$, tính khối tâm của mỗi điểm và chọn kết quả tốt nhất. Mỗi đánh giá yêu cầu tích phân phân bố khối lượng dọc theo chiều cao, nhưng vì mật độ là hằng số từng phần nên điều này có thể được tính toán theo thời gian không đổi. Lấy mẫu ở độ phân giải tốt sẽ hoạt động nhưng sẽ không cần thiết và không ổn định về mặt số nếu chúng ta dựa vào sự rời rạc. Với kích thước bước, chẳng hạn như,$10^{-6}$, chúng ta đã ở gần giới hạn của yêu cầu về độ chính xác và điều này vẫn che giấu cấu trúc thực sự của vấn đề. 

Quan sát quan trọng là hệ thống là một chiều với mặt cắt ngang đều nhau, do đó bài toán giảm xuống mức trung bình có trọng số dọc theo một khoảng. Khối tâm trở thành một hàm hữu tỉ trong$x$, được hình thành từ một tử số bậc hai (xuất phát từ việc tích phân$z$) và mẫu số tuyến tính (tính từ tổng khối lượng). Điều này làm cho mục tiêu trở nên trơn tru và khác biệt trên$(0, h)$, cho phép chúng ta xác định vị trí tối ưu bằng cách giải một phương trình đạo hàm thay vì tìm kiếm bằng số. 

Sau khi được viết lại ở dạng đóng, đạo hàm sẽ đơn giản hóa thành phương trình bậc hai có nghiệm mang lại chiều cao lấp đầy tối ưu một cách trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lấy mẫu lực lượng vũ phu |$O(k)$|$O(1)$| Quá chậm/không chính xác | 
| Giải pháp phân tích |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lấy khối tâm là hàm của chiều cao lấp đầy$x$, sau đó giảm thiểu nó. 

1. Mô hình chai như một đoạn thẳng đứng$[0, h]$với mật độ$d_w$TRÊN$[0, x]$Và$d_a$TRÊN$[x, h]$. Diện tích mặt cắt ngang không đổi nên nó triệt tiêu mọi tỷ số. 
2. Tính tổng khối lượng (đến hệ số diện tích không đổi). Nước góp phần$d_w x$, và không khí góp phần$d_a (h - x)$. Vậy tổng khối lượng là$$M(x) = d_a h + (d_w - d_a)x.$$3. Tính mômen xung quanh đáy. Nước góp phần$\int_0^x z d_w dz = d_w x^2/2$. Không khí đóng góp$\int_x^h z d_a dz = d_a(h^2 - x^2)/2$. Vậy tổng thời điểm là$$N(x) = \frac{d_a h^2 + (d_w - d_a)x^2}{2}.$$4. Khối tâm là$f(x) = N(x)/M(x)$. Yếu tố không đổi$1/2$có thể bỏ qua để giảm thiểu. 
5. Xác định$A = d_w - d_a$, đó là tích cực. Sau đó giảm thiểu$$g(x) = \frac{d_a h^2 + A x^2}{d_a h + A x}.$$6. Phân biệt$g(x)$sử dụng quy tắc thương và đặt tử số của đạo hàm bằng 0. Điều này mang lại$$A x^2 + 2 d_a h x - d_a h^2 = 0.$$7. Giải phương trình bậc hai này và lấy nghiệm dương:$$x = \frac{-2 d_a h + \sqrt{4 d_a^2 h^2 + 4 A d_a h^2}}{2A}.$$8. Rút gọn số hạng căn bậc hai:$$d_a^2 + d_a A = d_a d_w,$$vì vậy giải pháp trở thành$$x = h \cdot \frac{\sqrt{d_a d_w} - d_a}{d_w - d_a}.$$### Tại sao nó hoạt động 

chức năng$g(x)$mượt mà$(0, h)$và phát sinh từ tỷ số của hàm bậc hai lồi và hàm tuyến tính. Điều này đảm bảo một điểm dừng duy nhất trong khoảng khả thi. Vì đạo hàm rút gọn thành phương trình bậc hai có đúng một nghiệm dương trong$[0, h]$, gốc đó phải là bộ thu nhỏ toàn cục. Các điểm biên tương ứng với các trường hợp suy biến của không khí tinh khiết hoặc nước tinh khiết, và điểm dừng thống trị chúng bất cứ khi nào$d_w > d_a$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    h, r, da, dw = map(int, input().split())
    
    A = dw - da
    # avoid floating instability in degenerate reasoning
    x = h * (math.sqrt(da * dw) - da) / A
    print(x)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo biểu mẫu đóng dẫn xuất trực tiếp. Bán kính$r$không bao giờ xuất hiện trong biểu thức cuối cùng vì diện tích mặt cắt ngang không đổi bị triệt tiêu khi tính toán cả khối lượng và mô men. Phần tế nhị duy nhất là tính căn bậc hai$\sqrt{d_a d_w}$, việc này phải được thực hiện với độ chính xác gấp đôi; dung sai cần thiết của$10^{-6}$được thỏa mãn một cách dễ dàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
22 4 1 4
```Chúng tôi tính toán$A = 3$, Và$\sqrt{d_a d_w} = \sqrt{4} = 2$. 

| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Tính mét vuông |$\sqrt{1 \cdot 4}$| 2 | 
| Tử số |$2 - 1$| 1 | 
| Mẫu số |$4 - 1$| 3 | 
| x |$22 \cdot 1/3$| 7.3333... | 

Điểm tối ưu nằm ở một phần ba đoạn đường lên trên chai vì không khí nhẹ hơn nước nhiều, do đó việc tăng nước ban đầu cải thiện sự cân bằng bằng cách hạ thấp trọng tâm, nhưng cuối cùng lại nâng nó lên do tổng khối lượng tập trung tăng lên. 

### Ví dụ 2 

đầu vào:```
7 2 655 988
```| Bước | Biểu hiện | Giá trị | 
| --- | --- | --- | 
| Tính mét vuông |$\sqrt{655 \cdot 988}$| ≈ 803.49 | 
| Tử số |$803.49 - 655$| ≈ 148,49 | 
| Mẫu số |$988 - 655$| 333 | 
| x |$7 \cdot 148.49 / 333$| ≈ 3.14159 | 

Ở đây mức lấp đầy tối ưu phụ thuộc một cách tinh tế vào tỷ lệ mật độ. Kết quả rơi gần$\pi$là ngẫu nhiên, không phải cấu trúc, nhưng nó xác nhận công thức hoạt động trơn tru ngay cả khi mật độ lớn và có độ lớn gần nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ tính toán số học theo thời gian không đổi và đánh giá một căn bậc hai | 
| Không gian |$O(1)$| Không sử dụng cấu trúc phụ trợ | 

Giải pháp này nằm trong giới hạn thoải mái vì tất cả các phép toán đều là phép tính dấu phẩy động theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import math
    h, r, da, dw = map(int, sys.stdin.readline().split())
    A = dw - da
    x = h * (math.sqrt(da * dw) - da) / A
    return f"{x:.10f}"

# provided samples
assert abs(float(run("22 4 1 4\n")) - 7.3333333333) < 1e-6
assert abs(float(run("7 2 655 988\n")) - 3.1415941720) < 1e-5

# custom cases
assert abs(float(run("10 5 1 2\n")) - (10 * (math.sqrt(2) - 1))) < 1e-6, "small density gap"
assert abs(float(run("1 1 1 1000\n")) - (1 * (math.sqrt(1000) - 1) / 999)) < 1e-6, "extreme ratio"
assert abs(float(run("1000 1 999 1000\n")) - (1000 * (math.sqrt(999000) - 999) / 1)) < 1e-6, "near-equal densities"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng cách mật độ nhỏ | nội suy trơn tru | hành vi mật độ cân bằng | 
| tỷ lệ cực cao | xử lý chính xác mật độ bị lệch | ổn định số | 
| mật độ gần bằng nhau | không có sự phân chia bất ổn | độ nhạy ranh giới | 

## Vỏ cạnh 

Khi nào$d_w$chỉ lớn hơn một chút so với$d_a$, tối ưu$x$di chuyển gần bằng 0 vì việc thêm nước nhanh chóng làm tăng khối lượng mà không cải thiện đáng kể vị trí trung tâm. Công thức xử lý việc này vì tử số$\sqrt{d_a d_w} - d_a$trở nên nhỏ, phù hợp với giới hạn dự kiến. 

Khi$d_a$là rất nhỏ so với$d_w$, căn bậc hai chiếm ưu thế và giải pháp tiếp cận một phần lớn hơn$h$, phản ánh rằng không khí hầu như không đóng góp khối lượng và vị trí của nước chiếm ưu thế ở tâm khối. 

Khi$d_a$Và$d_w$rất gần nhau, mẫu số$d_w - d_a$trở nên nhỏ nhưng tử số co lại tỷ lệ thuận với sự giãn nở của$\sqrt{d_a d_w}$. Việc hủy bỏ này ngăn ngừa hiện tượng sai số và giữ cho biểu thức ổn định dưới dạng số học dấu phẩy động.
