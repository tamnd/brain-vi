---
title: "CF 312B - Cung thủ"
description: "Hai cung thủ lần lượt bắn vào một mục tiêu. SmallR bắn trước và trúng với xác suất $a/b$ trên mỗi lần bắn. Zanoes trúng đích với xác suất $c/d$ trên mỗi lần bắn. Ngay khi có người tung ra cú đánh đầu tiên của trận đấu, trò chơi kết thúc và người chơi đó thắng."
date: "2026-06-06T00:51:43+07:00"
tags: ["codeforces", "competitive-programming", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 312
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 185 (Div. 2)"
rating: 1300
weight: 312
solve_time_s: 104
verified: true
draft: false
---

[CF 312B - Cung thủ](https://codeforces.com/problemset/problem/312/B) 

**Đánh giá:** 1300 
**Tags:** toán, xác suất 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai cung thủ lần lượt bắn vào một mục tiêu. SmallR bắn trước và trúng có xác suất$a/b$trên mỗi lần bắn. Zanoes trúng đích với xác suất$c/d$trên mỗi lần bắn. Ngay khi có người tung ra cú đánh đầu tiên của trận đấu, trò chơi kết thúc và người chơi đó thắng. 

Đầu vào bao gồm bốn số nguyên$a, b, c, d$, xác định hai xác suất trúng:$$p=\frac{a}{b}, \qquad q=\frac{c}{d}$$Chúng ta phải tính xác suất để SmallR cuối cùng thắng. 

Những hạn chế là rất nhỏ. Đầu vào chỉ chứa bốn số nguyên và câu trả lời là một xác suất duy nhất. Không có cấu trúc dữ liệu lớn, không có biểu đồ và không có truy vấn lặp lại. Thử thách này hoàn toàn mang tính toán học. Bất kỳ giải pháp nào dẫn xuất biểu thức dạng đóng đều chạy trong thời gian không đổi. 

Khó khăn chính là nhận ra rằng về nguyên tắc, trò chơi có thể tiếp tục mãi mãi. Một mô phỏng đơn giản không thể liệt kê tất cả các chuỗi có thể vì có vô số vòng. Thay vào đó, xác suất phải được biểu thị dưới dạng một chuỗi vô hạn hoặc được giải bằng phép truy hồi. 

Một sai lầm phổ biến là chỉ xem xét vòng đầu tiên. Ví dụ:```
1 2 1 2
```SmallR có xác suất trúng ngay lập tức$1/2$, nhưng anh ấy cũng có thể bỏ lỡ, Zanoes có thể bỏ lỡ, và sau đó SmallR có một cơ hội khác. Câu trả lời đúng là$2/3$, không$1/2$. 

Một trường hợp tinh vi khác xảy ra khi cả hai người chơi đều có xác suất trúng đích rất nhỏ:```
1 1000 1 1000
```Trận đấu có thể kéo dài nhiều hiệp nhưng tổng xác suất thắng vẫn được xác định rõ ràng. Bất kỳ cách tiếp cận nào cắt ngắn quy trình sau một số vòng cố định đều có nguy cơ làm mất khối lượng xác suất đáng chú ý. 

Nguồn lỗi thứ ba là quên rằng SmallR bắn trước. Coi như:```
1 1 1 1
```Both players never miss. SmallR bắn trước Zanoes nên có xác suất thắng$1$, không$1/2$. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê tất cả các cách mà SmallR có thể giành chiến thắng. 

He can win immediately on his first shot. Anh ta cũng có thể giành chiến thắng sau khi cả hai người chơi trượt một lần, sau đó đánh lần thứ hai. Anh ta có thể giành chiến thắng sau khi cả hai người chơi trượt hai lần, sau đó đánh ở lần thử thứ ba, v.v. 

Điều này tạo ra một tổng vô hạn:$$p + (1-p)(1-q)p + ((1-p)(1-q))^2 p + \cdots$$Mỗi thuật ngữ tương ứng với một số vòng hoàn thành thất bại cụ thể trước khi SmallR cuối cùng đạt được cú đánh đầu tiên. 

Phiên bản brute-force sẽ đánh giá nhiều thuật ngữ của loạt bài này theo đúng nghĩa đen. Nó đúng về mặt toán học vì mỗi thuật ngữ đại diện cho một kịch bản chiến thắng rời rạc. Vấn đề là chuỗi này là vô hạn. Cắt ngắn sau$k$các thuật ngữ đưa ra lỗi gần đúng và chọn một giá trị đủ lớn$k$là không cần thiết. 

Quan sát quan trọng là chuỗi này có tính chất hình học. 

Cho phép$$r=(1-p)(1-q).$$Khi đó xác suất trở thành$$p + rp + r^2p + r^3p + \cdots.$$Đây chính xác là chuỗi hình học$$p \sum_{i=0}^{\infty} r^i.$$Từ$0 \le r < 1$, tổng của nó là$$\frac{p}{1-r}.$$Thay thế trở lại:$$\boxed{ \frac{p}{1-(1-p)(1-q)} }$$Biểu thức dạng đóng này cho kết quả theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tổng hợp nhiều thuật ngữ) | O(k) | O(1) | Chỉ gần đúng | 
| Tối ưu (chuỗi hình học) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn số nguyên$a, b, c, d$. 
2. Tính xác suất trúng của SmallR:$$p=\frac{a}{b}.$$3. Tính xác suất trúng Zanoes:$$q=\frac{c}{d}.$$4. Tính xác suất để toàn bộ ván đấu thất bại, nghĩa là cả hai người chơi đều trượt:$$r=(1-p)(1-q).$$Sau một vòng như vậy, trò chơi sẽ trở lại trạng thái giống hệt như lúc đầu. 
5. Sử dụng công thức chuỗi hình học:$$\text{answer} = \frac{p}{1-r}.$$6. In kết quả với độ chính xác vừa đủ. 

### Tại sao nó hoạt động 

hãy để$W$biểu thị xác suất SmallR thắng từ đầu vòng đấu. 

SmallR có thể thắng ngay với xác suất$p$. 

Nếu SmallR trượt và Zanoes cũng trượt, điều này xảy ra với xác suất$(1-p)(1-q)$, trò chơi trở lại trạng thái ban đầu. Từ thời điểm đó trở đi xác suất để SmalleR thắng lại là$W$. 

Như vậy$$W=p+(1-p)(1-q)W.$$Giải quyết cho$W$:$$W\bigl(1-(1-p)(1-q)\bigr)=p,$$

$$W= \frac{p}{1-(1-p)(1-q)}.$$Thuật toán tính toán chính xác đại lượng này nên phải trả về xác suất trúng thưởng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, c, d = map(int, input().split())

p = a / b
q = c / d

ans = p / (1.0 - (1.0 - p) * (1.0 - q))

print(f"{ans:.12f}")
```Đầu tiên, chương trình chuyển đổi các phân số thành xác suất dấu phẩy động. Sau đó nó tính toán xác suất mà cả hai người chơi đều trượt trong một vòng đấu hoàn chỉnh. 

Công thức$$\frac{p}{1-(1-p)(1-q)}$$được áp dụng trực tiếp. Dấu phẩy động có độ chính xác kép của Python là quá đủ cho yêu cầu$10^{-6}$sự chính xác. 

In mười hai chữ số sau dấu thập phân thỏa mãn khả năng chịu lỗi một cách thoải mái. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 1 2
```Đây$$p=\frac12,\qquad q=\frac12.$$| Biến | Giá trị | 
| --- | --- | 
|$p$| 0,5 | 
|$q$| 0,5 | 
|$r=(1-p)(1-q)$| 0,25 | 
| Trả lời$=p/(1-r)$| 0,666666666667 | 

Kết quả là$2/3$. SmallR được hưởng lợi từ việc chụp trước, vì vậy câu trả lời lớn hơn$1/2$. 

### Ví dụ 2 

đầu vào:```
1 1 1 1
```| Biến | Giá trị | 
| --- | --- | 
|$p$| 1 | 
|$q$| 1 | 
|$r=(1-p)(1-q)$| 0 | 
| Trả lời$=p/(1-r)$| 1 | 

SmallR không bao giờ bỏ lỡ. Vì anh ta bắn trước Zanoes nên trận đấu kết thúc ngay lập tức với chiến thắng thuộc về SmallR. 

Ví dụ này xác nhận rằng công thức xử lý chính xác các xác suất bằng một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Thời gian chạy không phụ thuộc vào giá trị của các số đầu vào. Giải pháp thực hiện một số phép tính dấu phẩy động và dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    a, b, c, d = map(int, input().split())
    p = a / b
    q = c / d
    ans = p / (1.0 - (1.0 - p) * (1.0 - q))

    return f"{ans:.12f}"

# provided sample
assert run("1 2 1 2\n") == "0.666666666667"

# SmallR always hits
assert run("1 1 1 2\n") == "1.000000000000"

# Zanoes always hits if he gets a turn
assert run("1 2 1 1\n") == "0.500000000000"

# Equal small probabilities
out = float(run("1 1000 1 1000\n"))
expected = (1 / 1000) / (1 - (999 / 1000) * (999 / 1000))
assert abs(out - expected) < 1e-12

# Both always hit
assert run("1 1 1 1\n") == "1.000000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 1 2`|`0.666666666667`| Mẫu chính thức | 
|`1 1 1 2`|`1.000000000000`| Thắng nhỏ ngay lập tức | 
|`1 2 1 1`|`0.500000000000`| Zanoes không bao giờ bỏ lỡ khi đến lượt mình | 
|`1 1000 1 1000`| Giá trị tính toán | Trò chơi được mong đợi từ lâu, công thức hình học | 
|`1 1 1 1`|`1.000000000000`| Cả hai đều hoàn hảo, thứ tự chụp đều quan trọng | 

## Vỏ cạnh 

### SmallR luôn trúng 

đầu vào:```
1 1 1 2
```chúng tôi có$p=1$Và$q=1/2$. 

Thuật toán tính toán$$r=(1-1)\left(1-\frac12\right)=0,$$

$$\text{answer}=\frac{1}{1-0}=1.$$Những túp lều nhỏ trước Zanoes từng bị bắn, nên rất có thể$1$là đúng. 

### Cả hai người chơi đều có xác suất trúng đích rất nhỏ 

đầu vào:```
1 1000 1 1000
```Xác suất của một vòng thất bại là$$r=\frac{999}{1000}\cdot\frac{999}{1000}=0.998001.$$Trò chơi thường kéo dài nhiều hiệp, nhưng công thức tổng hợp tất cả các khả năng có thể xảy ra cùng một lúc:$$\text{answer} = \frac{0.001}{1-0.998001} \approx 0.500250125.$$Không cần cắt bớt, do đó không mất khối lượng xác suất. 

###Cả hai luôn đánh 

đầu vào:```
1 1 1 1
```Thuật toán đưa ra$$r=0, \qquad \text{answer}=1.$$Một giải pháp bất cẩn có thể giả định tính đối xứng và trả về$1/2$. Công thức kết hợp chính xác thực tế là SmallR bắn trước và thắng ngay lập tức.
