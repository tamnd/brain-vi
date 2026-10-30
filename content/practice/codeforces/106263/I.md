---
title: "CF 106263I - \u5347\u7ea7\u5b9d\u53ef\u68a6"
description: "Mỗi người trong số sáu người chơi bắt đầu với Pokémon cấp 1. Để nâng cấp Pokémon từ cấp $k-1$ lên cấp $k$, chúng ta phải chi chính xác $k-1$ tinh thể kinh nghiệm. Đối với mỗi người chơi, chúng tôi được cấp một mức mục tiêu $ai$."
date: "2026-06-19T16:38:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106263
codeforces_index: "I"
codeforces_contest_name: "2025 \u534e\u5357\u5e08\u8303\u5927\u5b66\u201c\u5353\u8d8a\u6559\u80b2\u676f\u201d\u7b97\u6cd5\u4e0e\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u65b0\u751f\u8d5b\uff09"
rating: 0
weight: 106263
solve_time_s: 42
verified: true
draft: false
---

[CF 106263I - \u5347\u7ea7\u5b9d\u53ef\u68a6](https://codeforces.com/problemset/problem/106263/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người trong số sáu người chơi bắt đầu với Pokémon cấp 1. Để nâng cấp Pokémon từ cấp độ$k-1$để lên cấp$k$, chúng ta phải chi tiêu chính xác$k-1$trải nghiệm tinh thể. 

Đối với mỗi người chơi, chúng tôi được cấp một mức mục tiêu$a_i$. Chúng ta phải xác định cần bao nhiêu tinh thể để nâng Pokémon từ cấp 1 lên cấp độ cao nhất$a_i$, và xuất ra câu trả lời cho cả sáu người chơi. 

Mức mục tiêu có thể lớn như$10^9$. Điều đó ngay lập tức loại trừ mọi cách tiếp cận mô phỏng việc tăng cấp từng bước một. Một Pokémon có thể yêu cầu gần một tỷ bước nâng cấp, vượt xa những gì có thể được xử lý trong thời gian một giây. 

Thử thách chính không phải là số lượng người chơi, vì luôn có đúng sáu người trong số họ. Thách thức là tính toán tổng chi phí tinh thể cho cấp mục tiêu rất lớn mà không lặp lại qua mọi cấp độ trung gian. 

Có một số trường hợp khó xử lý sai. 

Giả sử mức mục tiêu đã là 1:```
1 1 1 1 1 1
```Đầu ra đúng là:```
0 0 0 0 0 0
```Không cần nâng cấp. Một công thức bất cẩn có thể vô tình bao gồm một số hạng thừa và tạo ra một câu trả lời khẳng định. 

Xem xét mục tiêu cấp 2:```
2 2 2 2 2 2
```Đầu ra đúng là:```
1 1 1 1 1 1
```Chỉ cần nâng cấp từ cấp 1 lên cấp 2, tốn đúng 1 tinh thể. Những lỗi sai lầm thường xuất hiện ở đây. 

Mức độ lớn nhất cũng rất quan trọng:```
1000000000
```Câu trả lời là khoảng$5 \times 10^{17}$, không vừa với số nguyên 32 bit. Các ngôn ngữ có kiểu số nguyên có chiều rộng cố định yêu cầu số học 64 bit. Python xử lý việc này một cách tự động, nhưng vẫn cần lưu ý đến kích thước toán học. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp là điểm khởi đầu tự nhiên nhất. Nếu mức mục tiêu là$a$, chúng tôi có thể thêm chi phí cho mỗi lần nâng cấp:$$1 + 2 + 3 + \cdots + (a-1)$$Điều này có hiệu quả vì nâng cấp từ cấp 1 lên cấp 2 tốn 1, từ cấp 2 lên cấp 3 tốn 2, v.v. cho đến lần nâng cấp cuối cùng lên cấp$a$. 

Vấn đề là thời gian chạy. Vì$a=10^9$, chúng tôi sẽ thực hiện khoảng một tỷ phép cộng cho một người chơi. Mặc dù chỉ có sáu người chơi nhưng hàng tỷ thao tác vẫn quá chậm. 

Quan sát cho thấy tổng được yêu cầu là một chuỗi số học nổi tiếng:$$1+2+\cdots+n=\frac{n(n+1)}{2}$$Đây$n=a-1$, do đó tổng chi phí tinh thể trở thành$$\frac{(a-1)a}{2}$$Công thức này tính toán câu trả lời theo thời gian không đổi, bất kể mức độ lớn đến đâu. Vì chỉ có sáu đầu vào nên chúng tôi chỉ cần áp dụng công thức sáu lần và in kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a) mỗi người chơi | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi người chơi | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc sáu cấp độ mục tiêu. 
2. Đối với từng cấp độ mục tiêu$a$, tính tổng chi phí tinh thể như sau:$$\frac{a(a-1)}{2}$$Đây là tổng chi phí nâng cấp từ cấp 1 lên cấp$a$. 
3. Lưu trữ sáu giá trị được tính toán. 
4. In ra sáu câu trả lời cách nhau bằng dấu cách. 

### Tại sao nó hoạt động 

Để đạt được cấp độ$a$, Pokémon phải thực hiện mọi nâng cấp:$$1 \to 2,\quad 2 \to 3,\quad \ldots,\quad (a-1)\to a$$Chi phí của những nâng cấp này là:$$1,2,3,\ldots,a-1$$Tổng số tinh thể cần thiết chính xác là tổng của các số nguyên này. Công thức chuỗi số học phát biểu rằng$$1+2+\cdots+(a-1)=\frac{a(a-1)}{2}$$Vì thuật toán tính toán chính xác giá trị này cho mọi người chơi nên mọi kết quả đầu ra đều chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

levels = list(map(int, input().split()))

ans = [a * (a - 1) // 2 for a in levels]

print(*ans)
```Chương trình đọc sáu cấp độ mục tiêu vào một danh sách. 

Đối với mỗi cấp độ`a`, nó đánh giá`a * (a - 1) // 2`, đó là công thức chuỗi số học được rút ra trước đó. Phép chia số nguyên được sử dụng vì kết quả luôn là số nguyên. 

Các câu trả lời được thu thập thành một danh sách và được in trên một dòng, cách nhau bằng dấu cách, phù hợp với định dạng đầu ra được yêu cầu. 

Một lỗi thường gặp là tính toán`a * (a + 1) // 2`. Đó sẽ là tổng từ 1 đến`a`, nhưng chi phí nâng cấp cuối cùng chỉ`a - 1`, vậy công thức đúng là`a * (a - 1) // 2`. 

Số nguyên Python tự động mở rộng đến kích thước tùy ý, do đó, ngay cả câu trả lời lớn nhất có thể cho$a=10^9$được xử lý một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 8 3 4 6 2
```| Cấp mục tiêu a | Công thức | Kết quả | 
| --- | --- | --- | 
| 10 | 10×9/2 | 45 | 
| 8 | 8×7/2 | 28 | 
| 3 | 3×2/2 | 3 | 
| 4 | 4×3/2 | 6 | 
| 6 | 6×5/2 | 15 | 
| 2 | 2×1/2 | 1 | 

Đầu ra:```
45 28 3 6 15 1
```Ví dụ này cho thấy rằng mỗi câu trả lời chỉ đơn giản là số tam giác tương ứng với$a-1$. 

### Ví dụ 2 

đầu vào:```
1 2 3 4 5 6
```| Cấp mục tiêu a | Công thức | Kết quả | 
| --- | --- | --- | 
| 1 | 1×0/2 | 0 | 
| 2 | 2×1/2 | 1 | 
| 3 | 3×2/2 | 3 | 
| 4 | 4×3/2 | 6 | 
| 5 | 5×4/2 | 10 | 
| 6 | 6×5/2 | 15 | 

Đầu ra:```
0 1 3 6 10 15
```Dấu vết này nêu bật trường hợp ranh giới trong đó cấp 1 không yêu cầu tinh thể nào cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chính xác sáu đánh giá công thức | 
| Không gian | O(1) | Chỉ có một vài biến và sáu kết quả đầu ra được lưu trữ | 

Kích thước đầu vào được cố định ở sáu số nguyên, do đó thời gian chạy thực tế là không đổi. Công thức số học tránh mọi sự phụ thuộc vào giá trị mức mục tiêu, cho phép mức lên tới$10^9$để được xử lý ngay lập tức. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    levels = list(map(int, input().split()))
    ans = [a * (a - 1) // 2 for a in levels]
    return " ".join(map(str, ans))

# provided sample
assert run("10 8 3 4 6 2\n") == "45 28 3 6 15 1", "sample"

# minimum levels
assert run("1 1 1 1 1 1\n") == "0 0 0 0 0 0", "all level 1"

# all equal values
assert run("5 5 5 5 5 5\n") == "10 10 10 10 10 10", "all equal"

# off-by-one boundary
assert run("2 2 2 2 2 2\n") == "1 1 1 1 1 1", "level 2"

# maximum values
assert run("1000000000 1000000000 1000000000 1000000000 1000000000 1000000000\n") == \
       "499999999500000000 499999999500000000 499999999500000000 499999999500000000 499999999500000000 499999999500000000", \
       "maximum level"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1 1`|`0 0 0 0 0 0`| Không cần nâng cấp | 
|`5 5 5 5 5 5`|`10 10 10 10 10 10`| Giá trị lặp lại | 
|`2 2 2 2 2 2`|`1 1 1 1 1 1`| Độ chính xác từng cái một | 
|`1000000000 ...`|`499999999500000000 ...`| Xử lý số lượng lớn | 

## Vỏ cạnh 

### Cấp độ mục tiêu đã là 1 

đầu vào:```
1 1 1 1 1 1
```Đối với mỗi giá trị:$$\frac{1\cdot(1-1)}{2}=0$$Các đầu ra thuật toán:```
0 0 0 0 0 0
```Điều này đúng vì Pokémon bắt đầu ở cấp độ 1 và không cần nâng cấp. 

### Cấp độ không tầm thường đầu tiên 

đầu vào:```
2 2 2 2 2 2
```Đối với mỗi giá trị:$$\frac{2\cdot1}{2}=1$$Các đầu ra thuật toán:```
1 1 1 1 1 1
```Chỉ có một lần nâng cấp, từ cấp 1 lên cấp 2, tốn đúng một viên pha lê. 

### Mức tối đa 

đầu vào:```
1000000000 1000000000 1000000000 1000000000 1000000000 1000000000
```Đối với mỗi giá trị:$$\frac{1000000000\cdot999999999}{2}
=
499999999500000000$$Thuật toán tính toán điều này trực tiếp mà không cần lặp lại. Mặc dù câu trả lời rất lớn nhưng Python vẫn lưu trữ nó một cách an toàn và in ra giá trị chính xác.
