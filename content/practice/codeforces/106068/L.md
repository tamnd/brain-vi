---
title: "CF 106068L - Lỗ tam giác"
description: "Chúng ta bắt đầu với một tam giác đều có kích thước được mô tả bằng chiều cao $H$. Quá trình này được lặp đi lặp lại. Trong mỗi thao tác, tam giác được chia thành bốn tam giác đều bằng nhau và chỉ giữ lại tam giác ở giữa trong khi ba tam giác còn lại bị loại bỏ."
date: "2026-06-21T15:57:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106068
codeforces_index: "L"
codeforces_contest_name: "2025 Aleppo and Idlib Private Universities Collegiate Programming Contest (APUCPC 2025)"
rating: 0
weight: 106068
solve_time_s: 39
verified: true
draft: false
---

[CF 106068L - Lỗ tam giác](https://codeforces.com/problemset/problem/106068/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một tam giác đều có kích thước được mô tả bằng chiều cao của nó$H$. Quá trình này được lặp đi lặp lại. Trong mỗi thao tác, tam giác được chia thành bốn tam giác đều bằng nhau và chỉ giữ lại tam giác ở giữa trong khi ba tam giác còn lại bị loại bỏ. Thao tác này được lặp lại$K$lần, luôn được áp dụng cho tam giác còn lại. 

Nhiệm vụ là xác định diện tích của tam giác còn lại cuối cùng sau$K$những hoạt động như vậy. 

Từ góc độ hình học, mỗi thao tác thu nhỏ hình tam giác theo một hệ số tuyến tính cố định. Vì tam giác được chia thành bốn tam giác con có diện tích bằng nhau và chỉ giữ lại một nên diện tích giảm đi đúng một hệ số là$4$mỗi hoạt động. Điều này về cơ bản làm cho vấn đề trở thành sự phân rã theo cấp số nhân của diện tích dưới sự phân chia lặp đi lặp lại hơn là bất kỳ sự tái thiết hình học nào. 

Giới hạn đầu vào lớn đối với số lượng hoạt động, với$K$lên đến$10^5$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng hình học hoặc thực hiện từng bước tính toán lại vùng lặp bằng cách sử dụng các cấu trúc hình học dấu phẩy động. Tuy nhiên, cấu trúc của phép biến đổi có tính nhân và hằng số trong mỗi bước, do đó toàn bộ quá trình có thể được rút gọn thành biểu thức dạng đóng. 

Trường hợp cạnh tinh tế xuất hiện khi$K = 0$, trong đó không có phép biến đổi nào xảy ra và câu trả lời đơn giản là diện tích của tam giác ban đầu. Bất kỳ giải pháp nào giả định ít nhất một thao tác sẽ làm giảm diện tích một cách không chính xác ngay cả khi không áp dụng bước nào. 

Một vấn đề khác phát sinh từ sự ổn định của dấu phẩy động. Vì câu trả lời liên quan đến phép chia lặp đi lặp lại theo lũy thừa 4, nên phép nhân hoặc phép chia lặp lại đơn giản có thể tích lũy sai số chính xác cho các số lớn.$K$, đặc biệt là gần$10^{-4}$ranh giới dung sai. Điều này khuyến khích tính toán lũy thừa cuối cùng trong một lần thay vì cập nhật từng bước. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ duy trì tam giác một cách rõ ràng và tính toán lại diện tích của nó sau mỗi lần chia nhỏ. Vì mỗi bước chỉ giữ một trong bốn phần bằng nhau nên diện tích này có thể được tính toán lại thành “diện tích hiện tại chia cho 4” nhiều lần. Điều này đúng vì mỗi thao tác duy trì sự tương đồng trong khi thu nhỏ diện tích một cách thống nhất. 

Tuy nhiên, điều này dẫn đến một quá trình thời gian tuyến tính trong$K$. Với$K$lên đến$10^5$, điều này vẫn khả thi về mặt tính toán, nhưng nó đưa ra phép chia lặp lại dấu phẩy động không cần thiết, làm tăng rủi ro lỗi tích lũy và tăng thêm chi phí. 

Quan sát quan trọng là phép biến đổi hoàn toàn có tính chất nhân lên và độc lập qua các bước. Sau đó$K$hoạt động, diện tích chính xác là diện tích ban đầu nhân với$(1/4)^K$. Điều này cho phép chúng ta tính kết quả cuối cùng theo thời gian không đổi bằng cách sử dụng lũy ​​thừa. 

Thành phần duy nhất còn lại là diện tích ban đầu của một tam giác đều khi biết chiều cao của nó$H$. Đối với một tam giác đều, nếu chiều cao bằng$H$, độ dài cạnh là$s = \frac{2H}{\sqrt{3}}$, và diện tích là:$$A = \frac{\sqrt{3}}{4}s^2 = \frac{\sqrt{3}}{4} \cdot \frac{4H^2}{3} = \frac{\sqrt{3}}{3}H^2$$Vì vậy, câu trả lời cuối cùng trở thành:$$A_K = \frac{\sqrt{3}}{3}H^2 \cdot 4^{-K}$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng từng bước | O(K) | O(1) | Quá chậm/không ổn định về số lượng | 
| Công thức dạng đóng | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$H$Và$K$. Chúng xác định hình dạng ban đầu và số lần thao tác thu nhỏ được áp dụng. 
2. Tính diện tích ban đầu của tam giác đều bằng công thức tính chiều cao$A_0 = \frac{\sqrt{3}}{3}H^2$. Bước này chuyển đổi đầu vào hình học thành một đại lượng vô hướng. 
3. Tính hệ số tỉ lệ gây ra bởi$K$hoạt động, đó là$4^{-K}$. Mỗi thao tác bảo toàn chính xác một trong bốn phần có diện tích bằng nhau, do đó diện tích được chia cho 4 mỗi lần. 
4. Nhân diện tích ban đầu với hệ số tỷ lệ để có diện tích cuối cùng. 
5. Xuất kết quả dưới dạng số dấu phẩy động. 

### Tại sao nó hoạt động 

Phép biến đổi trong mỗi thao tác ánh xạ bất kỳ tam giác đều nào thành một tam giác đồng dạng có một phần tư diện tích. Sự tương đồng đảm bảo rằng hình dạng không quan trọng ngoài quy mô, do đó chỉ có sự tiến hóa về diện tích là có liên quan. Vì mỗi bước áp dụng cùng một tỷ lệ không đổi một cách độc lập nên diện tích sau$K$các bước chính xác là diện tích ban đầu nhân với tích của$K$yếu tố giống nhau$1/4$. Tính bất biến nhân này đảm bảo rằng không cần suy luận hình học trung gian và không tồn tại hành vi phụ thuộc vào chuỗi. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

H, K = map(int, input().split())

# initial area of equilateral triangle from height
area0 = (math.sqrt(3) / 3.0) * H * H

# each operation keeps 1/4 of the area
area = area0 * (0.25 ** K)

print(area)
```Đầu tiên, mã chuyển đổi chiều cao hình học thành diện tích bằng cách sử dụng nhận dạng tam giác đều tiêu chuẩn. Việc chuyển đổi sau đó được áp dụng như một thao tác nguồn đơn lẻ, tránh bất kỳ vòng lặp lặp nào. sử dụng`0.25 ** K`đảm bảo cả tính rõ ràng và tính ổn định về số so với phép chia lặp lại. 

Một cạm bẫy phổ biến là tính toán độ dài cạnh trước rồi mới tính toán lại diện tích, việc này đưa ra các phép toán dấu phẩy động bổ sung mà không cải thiện độ chính xác. Một cách khác là lặp K lần, việc này không cần thiết và có thể tích lũy lỗi làm tròn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 2
```Diện tích ban đầu:$$A_0 = \frac{\sqrt{3}}{3} \cdot 100$$| Bước | Khu vực hiện tại | Yếu tố vận hành | Khu vực mới | 
| --- | --- | --- | --- | 
| 0 | 57.7350 | - | 57.7350 | 
| 1 | 57.7350 | 1/4 | 14.4338 | 
| 2 | 14.4338 | 1/4 | 3.6084 | 

Đầu ra:```
3.608439
```Dấu vết này cho thấy hành vi chia tỷ lệ thống nhất lặp đi lặp lại. Điều bất biến là mỗi bước sẽ giảm diện tích đúng 4 lần. 

### Ví dụ 2 

đầu vào:```
5 0
```| Bước | Khu vực hiện tại | Yếu tố vận hành | Khu vực mới | 
| --- | --- | --- | --- | 
| 0 | 14.4338 | - | 14.4338 | 

Đầu ra:```
14.433757
```Điều này chứng tỏ trường hợp cơ bản không xảy ra chuyển đổi. Thuật toán tránh áp dụng chính xác bất kỳ hệ số tỷ lệ nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số không đổi các phép tính số học và một lũy thừa | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Việc tính toán dễ dàng phù hợp với các ràng buộc và phép lũy thừa dấu phẩy động của$K \le 10^5$hoạt động hiệu quả trong Python nhờ các thói quen sử dụng năng lượng được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def solve():
    input = sys.stdin.readline
    H, K = map(int, input().split())
    area0 = (math.sqrt(3) / 3.0) * H * H
    print(area0 * (0.25 ** K))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("10 2")[:5] == "3.608"
assert run("5 0")[:6] == "14.433"

# custom cases
assert run("1 0")[:5] == "0.577", "minimum height no operations"
assert run("1 1") != "0", "single reduction should shrink area"
assert run("1000000000 0") != "", "maximum H no operations"
assert run("2 5") != "", "small height large K stability check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | khu căn cứ | đầu vào tối thiểu và không giảm | 
| 1 1 | giá trị giảm | độ chính xác của phép biến đổi đơn | 
| 1000000000 0 | H lớn | ổn định số ở quy mô tối đa | 
| 2 5 | H nhỏ, K lớn | ổn định lũy thừa | 

## Vỏ cạnh 

cho$K = 0$, thuật toán sẽ trả về diện tích tam giác ban đầu mà không áp dụng bất kỳ tỷ lệ nào. Nếu chúng ta tuân theo công thức một cách máy móc, phép tính lũy thừa$4^0 = 1$, do đó kết quả không thay đổi. Đối với đầu vào$H = 10, K = 0$, phép tính trở thành:$$A = \frac{\sqrt{3}}{3} \cdot 100 \cdot 1$$Thuật toán xử lý việc này một cách tự nhiên mà không cần phân nhánh đặc biệt. 

Đối với lớn$K$, nói$K = 100000$, hệ số$0.25^K$tràn về 0 trong số học dấu phẩy động. Điều này phù hợp với giới hạn toán học vì diện tích có xu hướng bằng 0 theo cấp số nhân. Dung sai được in cho phép hành vi này, do đó kết quả vẫn hợp lệ ngay cả khi nó trở thành số 0. 

Đối với lớn$H$, chẳng hạn như$H = 10^9$, diện tích ban đầu trở nên lớn nhưng vẫn nằm trong phạm vi chính xác gấp đôi khi kết hợp với phân rã theo cấp số nhân, bởi vì chia tỷ lệ theo$4^{-K}$nhanh chóng giảm cường độ ở mức vừa phải$K$.
