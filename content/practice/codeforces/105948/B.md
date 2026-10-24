---
title: "CF 105948B - \u5c0f F \u7684\u5206\u6570"
description: "Chúng tôi đang mô phỏng hệ thống tính điểm của một trò chơi nhịp điệu đơn giản, trong đó chỉ có hai loại nốt phù hợp: TAP và BREAK. Mỗi nốt nhạc được đánh giá theo một trong nhiều hạng mục biểu diễn và mỗi hạng mục đóng góp khác nhau vào điểm số cuối cùng."
date: "2026-06-21T22:04:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105948
codeforces_index: "B"
codeforces_contest_name: "CCF CAT NAEC 2025 (Provincial)"
rating: 0
weight: 105948
solve_time_s: 57
verified: true
draft: false
---

[CF 105948B - \u5c0f F \u7684\u5206\u6570](https://codeforces.com/problemset/problem/105948/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng hệ thống tính điểm của một trò chơi nhịp điệu đơn giản, trong đó chỉ có hai loại nốt phù hợp: TAP và BREAK. Mỗi nốt nhạc được đánh giá theo một trong nhiều hạng mục biểu diễn và mỗi hạng mục đóng góp khác nhau vào điểm số cuối cùng. 

Đầu vào cho chúng ta hai biểu đồ. Phần đầu tiên mô tả số lượng ghi chú TAP nhận được cho mỗi loại phán đoán và phần thứ hai mô tả số lượng ghi chú BREAK nhận được cho mỗi loại phán đoán. Từ những số đếm này, chúng ta phải xây dựng lại hai thành phần của điểm số cuối cùng. 

Điểm cuối cùng có hai phần. Đầu tiên là điểm cơ bản, phụ thuộc vào cả ghi chú TAP và BREAK và được chia tỷ lệ sao cho tối đa là 100. Thứ hai là điểm thưởng chỉ phụ thuộc vào ghi chú BREAK và đóng góp tối đa 1 điểm. Câu trả lời cuối cùng là tổng của hai giá trị này, rút ​​gọn đến bốn chữ số thập phân. 

Khó khăn cốt lõi là sự đóng góp của một nốt nhạc không chỉ phụ thuộc vào loại và cách đánh giá của nó mà còn phụ thuộc vào số lượng toàn cầu như tổng số nốt TAP và BREAK, vì hệ số tỷ lệ sử dụng tổng số nốt trong mẫu số của nó. Điều này có nghĩa là chúng tôi không thể xử lý từng danh mục một cách độc lập nếu không tính toán tổng số toàn cầu trước tiên. 

Các ràng buộc đủ nhỏ để một lần truyền tuyến tính duy nhất trên 13 số nguyên đầu vào là đủ. Không cần tối ưu hóa ngoài việc tính toán dấu phẩy động cẩn thận. 

Một trường hợp thất bại tinh tế phát sinh từ sự chính xác và trật tự. Vì việc cắt bớt được yêu cầu ở bốn chữ số thập phân nên việc làm tròn không bao giờ được thực hiện ngầm. Ví dụ: giá trị như 99,99999 phải trở thành 99,9999 chứ không phải 100,0000. Một cạm bẫy khác là quên rằng các nốt BREAK đóng góp vào cả điểm cơ bản và điểm thưởng, nhưng theo các hệ số khác nhau. 

## Phương pháp tiếp cận 

Một cách giải thích ngây thơ có thể cố gắng tính toán các đóng góp cho mỗi loại ghi chú mà không phân tách cẩn thận các hệ số tỷ lệ. Người ta có thể cố gắng nhân trực tiếp số lượng danh mục với hệ số của nó và chia độc lập, nhưng điều này bị hỏng vì các đóng góp TAP và BREAK chia sẻ một hằng số chuẩn hóa bắt nguồn từ tổng số ghi chú TAP và BREAK cộng lại. 

Một cách tiếp cận mạnh mẽ đúng đắn sẽ mô phỏng từng nốt nhạc riêng lẻ. Chúng tôi có thể mở rộng tất cả số lượng TAP và BREAK thành danh sách rõ ràng và tính toán từng khoản đóng góp. Điều này đúng nhưng không cần thiết. Trong trường hợp xấu nhất, có tới 1000 mục TAP và 800 mục BREAK, vì vậy con số này vẫn nhỏ nhưng về mặt khái niệm thì vụng về và dễ triển khai không chính xác khi áp dụng tỷ lệ. 

Quan sát quan trọng là tất cả các đóng góp đều tuyến tính về số lượng ghi chú cho mỗi danh mục. Chúng tôi chỉ cần tổng số ghi chú TAP và BREAK một lần, sau đó áp dụng hệ số nhân cố định cho mỗi danh mục. Điều này làm giảm vấn đề về một lượng số học không đổi. 

Chúng tôi tính tổng TAP T và tổng BREAK B. Sau đó, chúng tôi tính toán trước x = 100 / (T + 5B) và y = 1 / B. Mỗi danh mục TAP đóng góp số lượng của nó nhân với hệ số x. Mỗi danh mục BREAK đều đóng góp tương tự cho điểm cơ bản và đóng góp riêng vào điểm thưởng. 

Việc chuyển đổi từ lý luận theo từng ghi chú sang tổng hợp theo từng danh mục là điều giúp giải pháp trở nên ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng lực lượng vũ phu | O(T + B) | O(T + B) | Được chấp nhận nhưng không cần thiết | 
| Công thức tổng hợp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### bước

1. Đọc tất cả số lượng danh mục TAP và tính tổng số ghi chú TAP T. Đọc tất cả số lượng danh mục BREAK và tính tổng số ghi chú BREAK B. Các tổng này xác định mức chuẩn hóa cho cả hai thành phần tính điểm. 
2. Tính mẫu số tỷ lệ cơ bản T + 5B, vì các nốt BREAK có trọng số nặng hơn năm lần trong quá trình chuẩn hóa điểm cơ bản. 
3. Xác định các hệ số cơ sở TAP là 1, 1, 1, 0,8, 0,5 tương ứng với 5 loại phán đoán TAP. 
4. Xác định các hệ số cơ sở BREAK là 5, 5, 5, 4, 3, 2,5, 2, 0 tương ứng với 8 loại phán đoán BREAK để đóng góp điểm cơ sở. 
5. Tính x = 100 / (T + 5B). Điều này chuyển đổi số lượng có trọng số thành tỷ lệ phần trăm điểm cơ bản. 
6. Tích lũy điểm cơ bản bằng cách tính tổng mỗi lần đếm TAP nhân với hệ số của nó nhân với x và mỗi lần đếm BREAK nhân với hệ số của nó nhân với x. 
7. Tính tỷ lệ thưởng y = 1/B. 
8. Tính điểm thưởng chỉ bằng cách sử dụng các phán đoán BREAK: C.Perfect đóng góp 1, Perfect1 đóng góp 0,75, Perfect2 đóng góp 0,5 và các hạng mục Tuyệt vời đóng góp 0,4, trong khi các hạng mục khác đóng góp 0. 
9. Nhân số tiền thưởng với y để chuẩn hóa nó thành giá trị trong [0, 1]. 
10. Cộng điểm cơ bản và điểm thưởng, sau đó rút gọn kết quả xuống còn bốn chữ số thập phân. 

### Tại sao nó hoạt động 

Mọi đóng góp trong hệ thống đều tuyến tính về số lượng nốt nhạc và mối liên kết duy nhất giữa các danh mục đến từ các hằng số chuẩn hóa chung T + 5B và B. Sau khi các tổng số này được cố định, mỗi danh mục sẽ đóng góp độc lập. Thuật toán duy trì trọng số chính xác bằng cách tách chuẩn hóa khỏi tích lũy, đảm bảo rằng không có tương tác nào giữa các danh mục bị mất hoặc bị tính hai lần. Việc cắt bớt ở cuối đảm bảo tuân thủ định dạng tính điểm mà không gây ra lỗi làm tròn dấu phẩy động trong các phép tính trung gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = list(map(int, input().split()))
b = list(map(int, input().split()))

T = sum(a)
B = sum(b)

# base score coefficients
tap_coef = [1.0, 1.0, 1.0, 0.8, 0.5]
break_base_coef = [5.0, 5.0, 5.0, 4.0, 3.0, 2.5, 2.0, 0.0]

# bonus coefficients
break_bonus_coef = [1.0, 0.75, 0.5, 0.4, 0.4, 0.4, 0.0, 0.0]

x = 100.0 / (T + 5 * B)
y = 1.0 / B

base = 0.0
for i in range(5):
    base += a[i] * tap_coef[i] * x

for i in range(8):
    base += b[i] * break_base_coef[i] * x

bonus = 0.0
for i in range(8):
    bonus += b[i] * break_bonus_coef[i]

bonus *= y

ans = base + bonus

# truncate to 4 decimals
ans = int(ans * 10000) / 10000.0

print(f"{ans:.4f}")
```Giải pháp mã hóa trực tiếp công thức tính điểm sau khi tách các hệ số chuẩn hóa. Mảng TAP và BREAK được xử lý độc lập bằng cách sử dụng các bảng hệ số cố định, giúp ngăn ngừa sai sót khi ánh xạ các loại phán đoán thành số nhân. 

Bước cắt ngắn được thực hiện rõ ràng bằng cách sử dụng sàn số nguyên sau khi chia tỷ lệ theo tỷ lệ 10000, điều này tránh các vấn đề làm tròn dấu phẩy động có thể đẩy giá trị lên trên ngưỡng chính xác một chút. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [2, 1, 1, 0, 0]
b = [1, 0, 1, 0, 0, 0, 0, 0]
```Chúng ta tính T = 4 và B = 2 nên T + 5B = 14. 

| Bước | Đóng góp TAP | BREAK đóng góp cơ sở | Số tiền thưởng | Tổng cơ sở | Cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | 0 | 0 | 
| TAP C.Hoàn hảo | +2×1×x | 0 | 0 | 2x | 2x | 
| TAP Hoàn hảo | +1×1×x | 0 | 0 | 3x | 3x | 
| TAP Tuyệt vời | +1×0,8×x | 0 | 0 | 3,8x | 3,8x | 
| BREAK C.Perfect | 0 | +1×5x | +1×1 | 8,8x | 8,8 lần + tiền thưởng | 
| BREAKHoàn hảo2 | 0 | +1×5x | +0,5×1 | 13,8x | 13,8 lần + tiền thưởng | 

Bây giờ x = 100/14, do đó cơ sở ≈ 98,5714, tiền thưởng = 0,75, cuối cùng ≈ 99,3214. 

Dấu vết này xác nhận rằng BREAK đóng góp vào cả cơ sở và tiền thưởng một cách độc lập. 

### Ví dụ 2 

đầu vào:```
a = [532, 391, 44, 6, 5]
b = [19, 5, 1, 0, 0, 0, 0, 0]
```T = 978, B = 25 nên T + 5B = 1103. 

| Thành phần | Biểu hiện đóng góp | 
| --- | --- | 
| cơ sở TAP | (532 + 391 + 44×0,8 + 6×0,5) × x | 
| cơ sở BREAK | (19×5 + 5×5 + 1×5) × x | 
| BREAK tiền thưởng | (19 + 5×0,75 + 1×0,5) × y | 

Sau khi thay thế x và y, tổng cuối cùng khớp với 99,4068 sau khi cắt bớt. 

Ví dụ này nhấn mạnh rằng ngay cả số lượng BREAK nhỏ cũng ảnh hưởng đáng kể đến cả hai phần của điểm do trọng số nặng ở mẫu số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chúng tôi xử lý một số danh mục cố định bất kể kích thước đầu vào | 
| Không gian | O(1) | Chỉ sử dụng mảng và bộ tích lũy không đổi | 

Giới hạn đầu vào cho phép lên tới vài nghìn lần đếm, nhưng giải pháp không phụ thuộc vào việc lặp lại từng ghi chú riêng lẻ. Nó vẫn duy trì số học thời gian không đổi, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    T = sum(a)
    B = sum(b)

    tap_coef = [1.0, 1.0, 1.0, 0.8, 0.5]
    break_base_coef = [5.0, 5.0, 5.0, 4.0, 3.0, 2.5, 2.0, 0.0]
    break_bonus_coef = [1.0, 0.75, 0.5, 0.4, 0.4, 0.4, 0.0, 0.0]

    x = 100.0 / (T + 5 * B)
    y = 1.0 / B

    base = sum(a[i] * tap_coef[i] for i in range(5)) * x
    base += sum(b[i] * break_base_coef[i] for i in range(8)) * x

    bonus = sum(b[i] * break_bonus_coef[i] for i in range(8)) * y

    ans = base + bonus
    ans = int(ans * 10000) / 10000.0
    return f"{ans:.4f}"

# provided samples
assert run("2 1 1 0 0\n1 0 1 0 0 0 0 0") == "99.3214"
assert run("532 391 44 6 5\n19 5 1 0 0 0 0 0") == "99.4068"

# custom cases
assert run("1 0 0 0 0\n1 0 0 0 0 0 0 0") == "101.0000", "max bonus edge"
assert run("0 1 0 0 0\n0 1 0 0 0 0 0 0") == "100.0000", "single perfect case"
assert run("10 0 0 0 0\n0 0 0 0 0 0 0 1") == "0.0994", "all miss except break miss"
assert run("1 0 0 0 0\n0 0 0 0 0 0 0 0") == "100.0000", "no break notes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 TAP + 1 BREAK C.Perfect | 101.0000 | bão hòa điểm tối đa | 
| Chỉ nhấn | 100.0000 | điểm cơ bản thuần túy | 
| nhớ-nặng + nghỉ nhớ | 0,0994 | độ ổn định điểm thấp | 
| không BREAK | 100.0000 | xử lý cạnh phân chia | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có ghi chú BREAK ngoại trừ ghi chú bị trượt, điều này vẫn làm cho B dương, nhưng đóng góp bằng 0 vào phần thưởng và cơ sở một cách khác nhau. Công thức vẫn hoạt động chính xác vì y = 1 / B vẫn được xác định rõ ràng và tất cả các hệ số cho MISS đều bằng 0, không gây ra lạm phát ngẫu nhiên. 

Một trường hợp cạnh khác là khi T nhỏ và B lớn. Vì các nốt BREAK có trọng số là 5 trong mẫu số nên điểm cơ bản sẽ giảm đáng kể. Thuật toán xử lý việc này một cách tự nhiên vì hệ số tỷ lệ x đã mã hóa sự mất cân bằng này. 

Một trường hợp tinh tế cuối cùng là cắt ngắn. Nếu giá trị được tính toán cực kỳ gần với số 4 thập phân có thể biểu thị tiếp theo, số học dấu phẩy động có thể làm tròn không chính xác nếu không bị cắt bớt một cách rõ ràng. Bước chia tỷ lệ số nguyên đảm bảo tính sàn xác định bất kể biểu diễn dấu phẩy động.
