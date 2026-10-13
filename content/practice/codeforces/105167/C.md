---
title: "CF 105167C - Đếm hình chữ nhật"
description: "Chúng ta được cung cấp một lưới được vẽ bằng các đường ngang và dọc. Lưới được xác định đầy đủ bằng cách có $n$ đường ngang và $m$ đường thẳng đứng, cách đều nhau trong mặt phẳng."
date: "2026-06-27T10:02:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105167
codeforces_index: "C"
codeforces_contest_name: "ETH Zurich Competitive Programming Contest Spring 2024"
rating: 0
weight: 105167
solve_time_s: 56
verified: true
draft: false
---

[CF 105167C - Đếm hình chữ nhật](https://codeforces.com/problemset/problem/105167/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới được vẽ bằng các đường ngang và dọc. Lưới được xác định đầy đủ bằng cách có$n$đường ngang và$m$các đường thẳng đứng cách đều nhau trong mặt phẳng. Bất kỳ hình chữ nhật nào chúng ta tạo thành đều phải sử dụng những đường này làm ranh giới, nghĩa là mỗi cạnh của hình chữ nhật nằm chính xác trên một trong các đường lưới. 

Một hình chữ nhật hợp lệ được xác định bằng cách chọn hai đường ngang riêng biệt làm ranh giới trên và dưới và hai đường thẳng đứng riêng biệt làm ranh giới bên trái và bên phải. Mỗi lựa chọn như vậy xác định duy nhất một hình chữ nhật. Hai hình chữ nhật chỉ được coi là giống nhau nếu bốn điểm góc của chúng trùng nhau, điều này một lần nữa tương ứng chính xác với việc chọn cùng một cặp đường ngang và cùng một cặp đường thẳng đứng. 

Nhiệm vụ là đếm xem tồn tại bao nhiêu hình chữ nhật không suy biến như vậy. 

Các ràng buộc là nhỏ, với$n, m \le 100$, do đó, ngay cả các phương pháp tiếp cận bậc ba hoặc bậc hai cũng có thể thực hiện được. Tuy nhiên, cấu trúc của bài toán gợi ý rằng chúng ta nên mong đợi một số đồng nhất tính tổ hợp hơn là bất kỳ mô phỏng nào. Bất kỳ cách tiếp cận nào lặp lại rõ ràng trên tất cả các hình chữ nhật bằng cách kiểm tra hình học đều đã có hiệu quả.$O(n^2 m^2)$, ở đây vẫn ổn nhưng không cần thiết. 

Một sai lầm phổ biến là nghĩ về “ô lưới” thay vì “đường lưới”. Nếu có người giải thích$n$Và$m$là ô chứ không phải là dòng, chúng có thể sử dụng không chính xác$n \cdot m$hoặc những cách diễn đạt tương tự. Một sai lầm nhỏ khác là quên rằng hình chữ nhật cần có hai đường ngang và hai đường dọc riêng biệt, vì vậy các trường hợp$n < 2$hoặc$m < 2$phải tạo ra số không. 

Ví dụ, nếu$n = 1, m = 4$, chỉ có một đường ngang nên không thể tạo được hình chữ nhật nào cả. Câu trả lời đúng là 0. Bất kỳ cách tiếp cận nào giả định “chọn bất kỳ cặp nào” mà không kiểm tra tính khả thi sẽ âm thầm tạo ra các kết hợp không hợp lệ. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực bắt đầu bằng cách chọn rõ ràng một đường ngang trên và dưới, sau đó là đường dọc bên trái và bên phải, đồng thời đếm mọi kết hợp hợp lệ. Đây đã là một mô hình trực tiếp của bài toán: mỗi hình chữ nhật tương ứng với chính xác một phần bốn đường biên phân biệt. 

Phương pháp brute-force hoạt động bằng cách lặp lại tất cả các cặp đường ngang và tất cả các cặp đường thẳng đứng. Số cặp ngang là$\binom{n}{2}$, và số cặp dọc là$\binom{m}{2}$. Đối với mỗi sự kết hợp, chúng tôi đếm một hình chữ nhật. Điều này đúng vì hình chữ nhật được xác định duy nhất bởi các đường biên của nó. 

Sự kém hiệu quả chỉ xuất hiện nếu chúng ta nghĩ về các thuật ngữ liệt kê hình học, chẳng hạn như quét tất cả các hình chữ nhật theo từng ô hoặc xác thực hình học một cách rõ ràng. Ngay cả khi đó, kích thước vấn đề vẫn đủ nhỏ để$O(n^2 m^2)$sẽ trôi qua, vì$n, m \le 100$, cho nhiều nhất$10^8$hoạt động, đường biên nhưng vẫn được chấp nhận trong Python được tối ưu hóa. 

Quan sát quan trọng là các lựa chọn theo chiều ngang và chiều dọc là độc lập. Khi chúng tôi chọn hai đường ngang, chúng có thể ghép với bất kỳ cặp đường dọc hợp lệ nào. Điều này tách vấn đề thành hai số tổ hợp. 

Vậy tổng số hình chữ nhật là:$$\binom{n}{2} \cdot \binom{m}{2}$$Điều này loại bỏ tất cả nhu cầu lặp lại và giảm giải pháp về số học theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 m^2)$|$O(1)$| Được chấp nhận nhưng không cần thiết | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$Và$m$. Chúng đại diện cho số lượng đường lưới ngang và dọc có sẵn. 
2. Tính xem có bao nhiêu cách chọn hai đường thẳng nằm ngang phân biệt. Đây là$\frac{n(n-1)}{2}$. Lý do là hình chữ nhật cần có ranh giới trên và dưới và thứ tự không quan trọng. 
3. Tính xem có bao nhiêu cách chọn hai đường thẳng đứng phân biệt. Đây là$\frac{m(m-1)}{2}$, vì lý do tương tự. 
4. Nhân hai giá trị này. Mỗi cặp ngang có thể được kết hợp với bất kỳ cặp dọc nào và mỗi kết hợp tạo thành chính xác một hình chữ nhật. 
5. Xuất sản phẩm. 

### Tại sao nó hoạt động 

Mỗi hình chữ nhật được xác định duy nhất bằng cách chọn hai đường ngang và hai đường dọc. Không có sự chồng chéo giữa các lựa chọn khác nhau và không có lựa chọn nào tạo ra nhiều hơn một hình chữ nhật. Điều này tạo ra sự song ánh giữa tập hợp các hình chữ nhật hợp lệ và tích Descartes của hai tập hợp kết hợp. Do ánh xạ một-một này, việc đếm các hình chữ nhật giảm chính xác xuống việc nhân các lựa chọn tổ hợp độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

# choose any 2 horizontal lines
h = n * (n - 1) // 2

# choose any 2 vertical lines
v = m * (m - 1) // 2

print(h * v)
```Giải pháp dựa hoàn toàn vào nhận dạng tổ hợp hơn là lặp lại. Lỗi triển khai phổ biến nhất là lập chỉ mục riêng lẻ khi mọi người cố gắng mô phỏng các ô lưới thay vì làm việc với số lượng dòng. Một vấn đề khác là quên phép chia số nguyên, nhưng ở đây chúng tôi sử dụng rõ ràng`//`để đảm bảo kết quả là số nguyên. 

Bước nhân là an toàn trong Python do các số nguyên có độ chính xác tùy ý, do đó không tồn tại mối lo ngại về tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
```Chúng tôi tính toán kết hợp từng bước. 

| Bước | Đường ngang | Đường dọc | Cặp ngang | Cặp dọc | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 4 | - | - | - | 
| Tính cặp | 3 | 4 | 3 | 6 | - | 
| Cuối cùng | 3 | 4 | 3 | 6 | 18 | 

Đây,$\binom{3}{2} = 3$Và$\binom{4}{2} = 6$, cho 18 hình chữ nhật. 

Điều này xác nhận rằng ngay cả một lưới nhỏ cũng tạo ra nhiều hình chữ nhật do sự kết hợp các ranh giới tổ hợp. 

### Ví dụ 2 

đầu vào:```
2 5
```| Bước | Đường ngang | Đường dọc | Cặp ngang | Cặp dọc | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 5 | - | - | - | 
| Tính cặp | 2 | 5 | 1 | 10 | - | 
| Cuối cùng | 2 | 5 | 1 | 10 | 10 | 

Điều này cho thấy trường hợp suy biến trong đó chỉ tồn tại một cặp ngang. Kết quả hoàn toàn được xác định bởi các lựa chọn theo chiều dọc. 

Dấu vết chứng minh rằng công thức xử lý các cấu trúc cạnh nhỏ một cách tự nhiên mà không cần lớp vỏ đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số phép tính số học không đổi | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Việc tính toán bao gồm một vài phép nhân và chia số nguyên, không phụ thuộc vào kích thước đầu vào. Được cho$n, m \le 100$, giải pháp này thấp hơn nhiều so với mọi ràng buộc về giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline
    n, m = map(int, input().split())
    h = n * (n - 1) // 2
    v = m * (m - 1) // 2
    return str(h * v)

# provided sample
assert run("3 4") == "18"

# minimum case: no rectangle possible
assert run("1 1") == "0"

# only vertical flexibility
assert run("2 5") == "10"

# only horizontal flexibility
assert run("5 2") == "10"

# larger balanced case
assert run("10 10") == str((10*9//2)**2)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | không có cặp hợp lệ nào tồn tại | 
| 2 5 | 10 | hộp đựng cặp ngang đơn | 
| 5 2 | 10 | trường hợp cạnh đối xứng | 
| 10 10 | 2025 | độ đúng tổ hợp lớn hơn | 

## Vỏ cạnh 

Khi nào$n = 1$hoặc$m = 1$, công thức chính xác mang lại kết quả bằng 0 vì một trong các số hạng nhị thức trở thành 0. Đối với đầu vào`1 4`, việc tính toán trở thành$\binom{1}{2} = 0$, do đó kết quả là 0. Thuật toán xử lý việc này một cách tự nhiên mà không cần phân nhánh. 

Vì`2 2`, cả hai chiều đều có chính xác một cặp đường thẳng có thể có. Tính toán mang lại kết quả$1 \cdot 1 = 1$, tương ứng với hình chữ nhật đơn được tạo thành bởi ranh giới bên ngoài. 

Đối với đầu vào lớn hơn nhưng vẫn nhỏ như`3 3`, thuật toán tạo ra$3 \cdot 3 = 9$, khớp với bảng liệt kê đầy đủ của tất cả các hình chữ nhật có thể có trong lưới giao điểm 2D.
