---
title: "CF 104941A - Toán cổ"
description: "Chúng ta được cho bán kính của một hình tròn và chúng ta được yêu cầu dựng một hình vuông có diện tích bằng chính xác hình tròn đó. Nhiệm vụ không phải là xây dựng hình học theo nghĩa cổ điển mà là tính toán số trực tiếp: chúng ta phải đưa ra độ dài cạnh của hình vuông đó."
date: "2026-06-28T07:15:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104941
codeforces_index: "A"
codeforces_contest_name: "SLPC 2024 Open Division"
rating: 0
weight: 104941
solve_time_s: 64
verified: true
draft: false
---

[CF 104941A - Toán cổ](https://codeforces.com/problemset/problem/104941/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho bán kính của một hình tròn và chúng ta được yêu cầu dựng một hình vuông có diện tích bằng chính xác hình tròn đó. Nhiệm vụ không phải là xây dựng hình học theo nghĩa cổ điển mà là tính toán số trực tiếp: chúng ta phải đưa ra độ dài cạnh của hình vuông đó. 

Diện tích hình tròn có bán kính$r$là$\pi r^2$. Nếu hình vuông có độ dài cạnh$s$, diện tích của nó là$s^2$. Đánh đồng những điều này mang lại$s^2 = \pi r^2$, vậy số lượng chúng ta cần là$s = r\sqrt{\pi}$. 

Kích thước đầu vào cực kỳ nhỏ, với$r \le 1000$. Điều này có nghĩa là hiệu suất hoàn toàn không phải là vấn đề đáng lo ngại và bất kỳ phép tính số học nào trong thời gian không đổi là đủ. Yêu cầu thực sự duy nhất là độ chính xác về mặt số học, vì câu trả lời liên quan đến căn bậc hai và phép nhân với$\pi$và thẩm phán cho phép một lỗi nhỏ tương đối hoặc tuyệt đối lên đến$10^{-4}$. 

Không có trường hợp biên cấu trúc nào xét về hành vi thuật toán, nhưng có một cạm bẫy thực tế: sử dụng không đủ độ chính xác của dấu phẩy động hoặc giá trị không chính xác của$\pi$. Một cách triển khai đơn giản sử dụng hằng số được làm tròn quá mức cho$\pi$vẫn có thể vượt qua, nhưng nó có nguy cơ thất bại nếu phép tính gần đúng quá thô, đặc biệt đối với các giá trị lớn hơn của$r$như 1000 trong đó sai số tuyệt đối tăng lên. 

Một vấn đề tế nhị khác là lỗi chia số nguyên trong các ngôn ngữ mà$r$có thể vô tình được coi là số học số nguyên trong toàn bộ biểu thức như`r * sqrt(pi)`nếu viết sai. Tuy nhiên, trong Python điều này đương nhiên là an toàn. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng tái tạo lại hình học một cách rõ ràng, có thể bằng cách mô phỏng diện tích hình tròn hoặc tìm kiếm một cạnh hình vuông có diện tích phù hợp$\pi r^2$. Cách tiếp cận như vậy sẽ phức tạp một cách không cần thiết và sẽ đưa ra các phương pháp lặp số hoặc tìm nghiệm. Ví dụ: người ta có thể tìm kiếm nhị phân cho$s$như vậy$s^2 \approx \pi r^2$. Điều này sẽ hiệu quả, nhưng nó quá mức cần thiết: mỗi đánh giá có thời gian không đổi và tốc độ hội tụ nhanh, nhưng nó làm tăng thêm độ phức tạp mà không mang lại bất kỳ lợi ích nào. 

Quan sát quan trọng là mối quan hệ giữa hình tròn và hình vuông có tính đại số và ngay lập tức sụp đổ thành một dạng khép kín. Khi chúng ta đánh đồng diện tích, không cần quá trình xấp xỉ nào ngoài việc đánh giá căn bậc hai. Điều này làm giảm vấn đề từ tìm kiếm bằng số thành đánh giá biểu thức đơn lẻ. 

Ý tưởng vũ phu hoạt động vì chức năng$f(s) = s^2 - \pi r^2$là đơn điệu trong$s$, do đó việc tìm nghiệm sẽ hội tụ. Tuy nhiên, nó thất bại theo nghĩa là không cần thiết và dễ bị lỗi hơn so với tính toán trực tiếp. Dạng đóng loại bỏ hoàn toàn việc lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (root tìm kiếm nhị phân) |$O(\log \frac{1}{\epsilon})$|$O(1)$| Được chấp nhận nhưng quá phức tạp | 
| Tối ưu (dạng đóng) |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bán kính số nguyên$r$. Đây là đầu vào duy nhất và xác định đầy đủ khu vực mục tiêu của vòng tròn. 
2. Tính độ dài cạnh bằng đẳng thức dẫn xuất$s = r \cdot \sqrt{\pi}$. Bước này chuyển trực tiếp điều kiện hình học sang dạng số học, tránh mọi vòng lặp gần đúng hoặc tinh chỉnh lặp lại. 
3. In giá trị kết quả với độ chính xác vừa đủ. Vì sai số cho phép là$10^{-4}$, định dạng dấu phẩy động tiêu chuẩn có khoảng 10 chữ số thập phân là quá đủ để đảm bảo được chấp nhận. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ sự tương đương trực tiếp của các diện tích. Cả hai hình dạng đều được mô tả đầy đủ bởi một tham số duy nhất và diện tích của chúng là các hàm xác định của tham số đó. Cài đặt$\pi r^2 = s^2$không để lại sự mơ hồ trong$s$, và nghiệm dương là nghiệm hình học duy nhất có ý nghĩa. Bởi vì chúng tôi không bao giờ ước chừng thông qua quá trình sàng lọc lặp đi lặp lại, nên không có sự tích tụ lỗi nào ngoài lỗi biểu diễn dấu phẩy động tiêu chuẩn, lỗi này vẫn nằm trong phạm vi dung sai cho phạm vi ràng buộc nhất định. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

r = int(input().strip())
ans = r * math.sqrt(math.pi)
print(ans)
```Giải pháp này hoàn toàn dựa vào số học dấu phẩy động tích hợp của Python và`math.sqrt`, đủ chính xác cho dung sai yêu cầu. Hằng số$\pi$cũng được cung cấp bởi`math.pi`, đảm bảo độ chính xác cao so với các phép tính gần đúng được xác định thủ công. 

Thứ tự nhân rất đơn giản và an toàn vì cả hai toán hạng đều tương thích với dấu phẩy động. Không có vấn đề về phân chia số nguyên hoặc mối lo ngại về tràn số do ràng buộc nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```Chúng tôi tính toán$s = 1 \cdot \sqrt{\pi}$. 

| Bước | r | sqrt(pi) | s | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 1 | - | - | 
| Tính toán | 1 | 1.7724538509 | 1.7724538509 | 

Kết quả đầu ra khớp với độ dài cạnh hình vuông dự kiến ​​cho một hình tròn đơn vị, xác nhận rằng công thức mô tả trực tiếp hình học. 

### Ví dụ 2 

đầu vào:```
42
```Chúng tôi tính toán$s = 42 \cdot \sqrt{\pi}$. 

| Bước | r | sqrt(pi) | s | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 42 | - | - | 
| Tính toán | 42 | 1.7724538509 | 74.4430617380 | 

Điều này thể hiện tỷ lệ tuyến tính với bán kính: tăng gấp đôi$r$nhân đôi chiều dài cạnh hình vuông thu được, phù hợp với mối quan hệ đại số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số phép tính số học không đổi được thực hiện bất kể kích thước đầu vào | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Với hạn chế$r \le 1000$, quá trình tính toán diễn ra nhanh chóng và việc đánh giá dấu phẩy động chi phối tất cả các cân nhắc về thời gian chạy, điều này không đáng kể trong Python. 

## Trường hợp thử nghiệm```python
import sys, io, math

def solve():
    import sys, math
    r = int(sys.stdin.readline())
    print(r * math.sqrt(math.pi))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from io import StringIO
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert abs(float(run("1")) - 1.7724538509) < 1e-6
assert abs(float(run("42")) - 74.4430617380) < 1e-6

# custom cases
assert abs(float(run("0")) - 0.0) < 1e-6
assert abs(float(run("1000")) - 1000 * math.sqrt(math.pi)) < 1e-6
assert abs(float(run("2")) - 2 * math.sqrt(math.pi)) < 1e-6
assert abs(float(run("10")) - 10 * math.sqrt(math.pi)) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | trường hợp biên bán kính bằng 0 | 
| 1000 | 1000·√π | chia tỷ lệ hạn chế tối đa | 
| 2 | 2·√π | tỷ lệ nhỏ không tầm thường | 
| 10 | 10·√π | độ ổn định chính xác trung gian | 

## Vỏ cạnh 

Đối với bán kính nhỏ nhất, chẳng hạn như$r = 1$, việc tính toán giảm xuống còn$s = \sqrt{\pi}$. Thuật toán chỉ cần đánh giá biểu thức và in nó mà không cần xử lý đặc biệt. Độ chính xác của dấu phẩy động là quá đủ để giữ sai số trong phạm vi dung sai. 

Đối với bán kính lớn nhất$r = 1000$, kết quả là khoảng$1772.45$. Ngay cả ở đây, phép nhân với$\sqrt{\pi}$không gây ra sự mất ổn định, vì cả hai toán hạng đều nằm trong phạm vi dấu phẩy động an toàn. Thuật toán thực hiện một phép nhân đơn và căn bậc hai, do đó không có phép làm tròn trung gian nào được tích lũy qua các bước.
