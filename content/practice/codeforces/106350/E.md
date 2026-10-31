---
title: "CF 106350E - Hàm đối xứng của Zeyad"
description: "Bài toán yêu cầu chúng ta xem xét đường cong được xác định bởi hàm nghịch đảo, trong đó đầu vào cho một khoảng tọa độ x nguyên. Với mọi số nguyên khác 0 bên trong khoảng đó, chúng ta vẽ đường tiếp tuyến của đường cong tại tọa độ x đó."
date: "2026-06-25T08:06:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106350
codeforces_index: "E"
codeforces_contest_name: "Zaglol Contest - FCDS level 1 contest 2026"
rating: 0
weight: 106350
solve_time_s: 29
verified: true
draft: false
---

[CF 106350E - Hàm đối xứng của Zeyad](https://codeforces.com/problemset/problem/106350/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xem xét đường cong được xác định bởi hàm nghịch đảo, trong đó đầu vào cho một khoảng tọa độ x nguyên. Với mọi số nguyên khác 0 bên trong khoảng đó, chúng ta vẽ đường tiếp tuyến của đường cong tại tọa độ x đó. Đường tiếp tuyến đó cắt hai trục tọa độ và tạo thành một hình tam giác. Nhiệm vụ là cộng diện tích của tất cả các hình tam giác này. 

Giá trị đầu vào có thể lớn bằng$10^{18}$về giá trị tuyệt đối. Điều này ngay lập tức loại trừ việc lặp qua mọi số nguyên trong khoảng, vì độ dài khoảng có thể vào khoảng$2 \times 10^{18}$, vượt xa những gì bất kỳ chương trình nào có thể xử lý. Chúng ta cần quan sát thời gian theo logarit hoặc thời gian không đổi. Vì hàm số và công thức tiếp tuyến đơn giản về mặt đại số nên giải pháp mong muốn đến từ việc đơn giản hóa biểu thức hình học thay vì tìm kiếm hoặc mô phỏng. 

Một sai lầm phổ biến là quên rằng số 0 là không được phép. Ví dụ: đối với đầu vào:```
0 0
```câu trả lời là:```
0
```Không có tọa độ x hợp lệ vì hàm nghịch đảo không được xác định ở mức 0. Một chương trình chỉ đếm kích thước của khoảng sẽ xuất ra không chính xác 2. 

Một trường hợp cạnh khác xuất hiện khi khoảng vượt qua 0. Đối với đầu vào:```
-2 2
```tọa độ hợp lệ là$-2,-1,1,2$, vậy đáp án là:```
8
```Việc thực hiện bất cẩn có thể đếm năm vị trí và bao gồm số 0, tạo ra kết quả không chính xác. 

Trường hợp ranh giới cuối cùng là một giá trị âm duy nhất:```
-7 -7
```Câu trả lời là:```
2
```Diện tích không trở nên âm khi tiếp tuyến nằm ở phía bên kia của trục. Diện tích tam giác sử dụng độ dài tuyệt đối. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là truy cập mọi số nguyên x trong khoảng, bỏ qua số 0, tính đường tiếp tuyến, tính diện tích tam giác của nó và thêm nó vào câu trả lời. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, nếu khoảng chứa$10^{18}$các giá trị, vòng lặp sẽ yêu cầu khoảng$10^{18}$lặp đi lặp lại, điều này là không thể trong thời gian giới hạn. 

Quan sát quan trọng là diện tích tam giác giống nhau đối với mọi tọa độ x hợp lệ. Đặt tọa độ đã chọn là$a$. Đạo hàm của$1/x$là$-1/x^2$, do đó đường tiếp tuyến tại$(a,1/a)$là$$y-\frac1a=-\frac1{a^2}(x-a)$$điều đó đơn giản hóa thành$$y=-\frac{x}{a^2}+\frac2a$$Chặn x được tìm thấy bằng cách cài đặt$y=0$:$$0=-\frac{x}{a^2}+\frac2a$$Vì thế:$$x=2a$$Đánh chặn y là:$$y=\frac2a$$Diện tích tam giác tạo bởi trục và đường thẳng là:$$\frac12 \times |2a| \times \left|\frac2a\right| = 2$$Mọi số nguyên khác 0 đóng góp chính xác bằng 2 vào câu trả lời. Hình học biến mất và toàn bộ vấn đề trở thành việc đếm xem có bao nhiêu số nguyên khác 0 trong khoảng. 

Phương pháp brute-force hoạt động vì mỗi điểm có thể được xử lý độc lập nhưng không thành công vì có quá nhiều điểm. Việc đơn giản hóa đại số cho phép chúng ta thay thế hàng tỷ phép tính hình học bằng một lần đếm khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r-l+1) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai điểm cuối của khoảng. 
2. Đếm xem có bao nhiêu số nguyên trong khoảng. Tổng số số nguyên là$r-l+1$. Nếu khoảng chứa số 0, hãy xóa nó khỏi số đếm vì hàm không được xác định ở đó. 
3. Nhân số số nguyên hợp lệ với 2. Mỗi số nguyên hợp lệ tạo ra một hình tam giác có diện tích đúng bằng 2. 

Lý do điều này có tác dụng là vì câu trả lời chỉ được xác định bởi số điểm hợp lệ chứ không phải bởi vị trí thực tế của chúng. Tọa độ âm và dương đóng góp như nhau vì diện tích tam giác sử dụng độ dài tuyệt đối. 

Tại sao nó hoạt động: 

Với mọi số nguyên khác 0$x$, đường tiếp tuyến tạo ra điểm giao nhau$2x$Và$2/x$. Tích tuyệt đối của chúng luôn bằng 4 nên diện tích tam giác luôn bằng$4/2=2$. Vì các hình tam giác là độc lập nên tổng cuối cùng chỉ đơn giản là 2 nhân với số giá trị x được phép. Loại bỏ số 0 là cách xử lý đặc biệt duy nhất cần thiết vì$1/x$không tồn tại ở đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    l, r = map(int, input().split())

    count = r - l + 1
    if l <= 0 <= r:
        count -= 1

    print(count * 2)

if __name__ == "__main__":
    solve()
```Chương trình đầu tiên tính toán kích thước của khoảng đóng. Điều này chỉ sử dụng số học trên các điểm cuối, vì vậy ngay cả các điểm cuối gần$10^{18}$được xử lý ngay lập tức bằng số nguyên Python. 

Kiểm tra bằng 0 được viết là`l <= 0 <= r`, mô tả trực tiếp liệu số 0 có thuộc khoảng hay không. Nếu có, số lượng sẽ giảm đi một. 

Phép nhân cuối cùng với 2 áp dụng sự đóng góp không đổi của mọi tam giác tiếp tuyến hợp lệ. Không có vòng lặp, phép toán dấu phẩy động hoặc phép tính hình học nên không có vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
1 3
```Việc thực hiện là: 

| tôi | r | Số nguyên trong phạm vi | Không loại bỏ | Số hợp lệ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 0 | 3 | 6 | 

Ba tọa độ hợp lệ là 1, 2 và 3. Mỗi tọa độ đóng góp diện tích 2 nên tổng số là 6. 

Hãy xem xét đầu vào:```
-2 2
```Việc thực hiện là: 

| tôi | r | Số nguyên trong phạm vi | Không loại bỏ | Số hợp lệ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| -2 | 2 | 5 | 1 | 4 | 8 | 

Dấu vết này thể hiện trường hợp đặc biệt duy nhất trong giải pháp. Số 0 được bao gồm trong kích thước khoảng nhưng phải được loại bỏ trước khi tính toán phần đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Giải pháp chỉ thực hiện một số phép tính số học. | 
| Không gian | O(1) | Không cần cấu trúc dữ liệu. | 

Độ dài khoảng thời gian tối đa có thể không ảnh hưởng đến thời gian chạy vì thuật toán không bao giờ lặp lại trong khoảng thời gian đó. Giải pháp phù hợp thoải mái với các giới hạn ngay cả đối với các giá trị lớn nhất có thể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    l, r = map(int, input().split())

    count = r - l + 1
    if l <= 0 <= r:
        count -= 1

    return str(count * 2)

# minimum-size style case
assert solve("0 0\n") == "0"

# single valid point
assert solve("5 5\n") == "2"

# interval crossing zero
assert solve("-2 2\n") == "8"

# large boundary case
assert solve("-1000000000000000000 1000000000000000000\n") == "4000000000000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`0`| Điểm 0 không xác định không đóng góp gì. | 
|`5 5`|`2`| Một tiếp tuyến hợp lệ đóng góp chính xác một giá trị diện tích. | 
|`-2 2`|`8`| Loại bỏ 0 và giá trị âm đối xứng. | 
|`-1000000000000000000 1000000000000000000`|`4000000000000000000`| Xử lý các điểm cuối có kích thước tối đa. | 

## Vỏ cạnh 

Đối với đầu vào:```
0 0
```thuật toán bắt đầu với kích thước khoảng 1. Vì số 0 nằm trong khoảng nên nó trừ đi một và nhận được số đếm hợp lệ bằng 0. Phép nhân cuối cùng cho 0. 

Đối với đầu vào:```
-2 2
```số đếm ban đầu là 5 vì các giá trị là$-2,-1,0,1,2$. Điều kiện phát hiện số 0 là đúng nên số đếm trở thành 4. Câu trả lời trở thành$4 \times 2=8$, khớp với bốn tam giác tiếp tuyến hợp lệ. 

Đối với đầu vào:```
-7 -7
```khoảng chỉ chứa một giá trị. Điều kiện 0 là sai nên số đếm vẫn là 1. Câu trả lời là 2, cho thấy tọa độ âm được xử lý giống như tọa độ dương vì diện tích sử dụng khoảng cách tuyệt đối.
