---
title: "CF 1180A - Alex và hình thoi"
description: "Chúng ta có một hình dạng phát triển trên một lưới bắt đầu từ một ô duy nhất. Ở giai đoạn đầu tiên có chính xác một ô. Ở mỗi giai đoạn tiếp theo, hình sẽ mở rộng bằng cách gắn mọi ô lưới có chung cạnh với bất kỳ ô nào đã có trong hình."
date: "2026-06-12T01:32:52+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1180
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 569 (Div. 2)"
rating: 800
weight: 1180
solve_time_s: 59
verified: true
draft: false
---

[CF 1180A - Alex và hình thoi](https://codeforces.com/problemset/problem/1180/A) 

**Đánh giá:** 800 
**Tags:** dp, triển khai, toán học 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hình dạng phát triển trên một lưới bắt đầu từ một ô duy nhất. Ở giai đoạn đầu tiên có chính xác một ô. Ở mỗi giai đoạn tiếp theo, hình sẽ mở rộng bằng cách gắn mọi ô lưới có chung cạnh với bất kỳ ô nào đã có trong hình. Nói cách khác, mỗi bước sẽ thêm “đường viền dày một ô” đầy đủ xung quanh hình hiện tại theo bốn hướng chính. 

Điều này tạo ra một mẫu giống như kim cương có tâm ở ô ban đầu. Nhiệm vụ là xác định có bao nhiêu ô lưới đơn vị sau khi hình được mở rộng chính xác.$n$lần theo quy luật này. 

Đầu vào là một số nguyên duy nhất$n$, Ở đâu$1 \le n \le 100$và đầu ra là tổng số ô bị chiếm trong hình dạng cuối cùng. 

Hạn chế là cực kỳ nhỏ. Với$n \le 100$, bất kỳ phương pháp nào lên đến khoảng$10^8$các thao tác đơn giản có thể diễn ra dễ dàng, nhưng trong thực tế, vấn đề này có cấu trúc dạng đóng khiến cho việc mô phỏng không cần thiết. 

Một mô phỏng đơn giản sẽ duy trì rõ ràng tập hợp các ô và mở rộng từng lớp một. Điều này hoạt động về mặt khái niệm nhưng trở nên không hiệu quả khi hình dạng phát triển theo phương trình bậc hai. Tuy nhiên, ngay cả trường hợp xấu nhất ở đây cũng rất nhỏ, do đó mô phỏng sẽ không hết thời gian nhưng không cần thiết phải hiểu cấu trúc. 

Không có trường hợp khó khăn nào về tính hợp lệ của đầu vào. Điều kiện biên duy nhất là$n = 1$, trong đó câu trả lời phải chính xác là 1 ô. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Bắt đầu với một ô duy nhất ở gốc. Đối với mỗi bước, hãy quét tất cả các ô hiện tại và thêm bốn ô lân cận nếu chúng chưa có. Nếu chúng tôi duy trì một bộ, chúng tôi đảm bảo không có bản sao. Sau đó$n-1$mở rộng, chúng tôi đếm tổng số ô. 

Điều này đúng vì nó tuân theo định nghĩa: mỗi bước bao gồm tất cả các ô có chung ít nhất một cạnh với hình dạng hiện tại. 

Vấn đề là cách tiếp cận này ẩn cấu trúc. Mỗi lớp thêm một vòng đối xứng xung quanh hình dạng trước đó. Nếu chúng ta nhìn vào hình dạng sau một vài bước, chúng ta quan sát thấy: 

Tại$n = 1$, chúng ta có 1 ô. 

Tại$n = 2$, chúng ta nhận được một đường chéo gồm 5 ô. 

Tại$n = 3$, hình dạng mở rộng đến 13 ô. 

Tại$n = 4$, nó trở thành 25 ô. 

Số gia là 4, 8, 12, v.v. Mỗi lớp mới thêm$4(n-1)$tế bào. Điều này xảy ra vì ranh giới của hình tạo thành một viên kim cương có chu vi tăng tuyến tính với$n$. 

Vì vậy, thay vì mô phỏng hình học, chúng tôi rút gọn bài toán thành tổng một cấp số cộng:$$1 + 4(1 + 2 + \cdots + (n-1))$$Tổng bên trong là$\frac{(n-1)n}{2}$, cho:$$1 + 2n(n-1)$$Biểu mẫu đóng này loại bỏ tất cả mô phỏng và giảm nhiệm vụ về số học theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n^2)$| Được chấp nhận nhưng không cần thiết | 
| Công thức dạng đóng |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$. Điều này thể hiện số lớp mở rộng mà chúng tôi áp dụng bắt đầu từ một ô. 
2. Nếu$n = 1$, trả về 1 ngay lập tức vì không có sự mở rộng nào xảy ra và hình dạng vẫn là một ô đơn vị. 
3. Tính số ô được thêm vào sau khi mở rộng như sau$2n(n-1)$. Điều này xuất phát từ việc tổng hợp kích thước của các lớp ranh giới liên tiếp. 
4. Thêm 1 vào tài khoản cho ô trung tâm ban đầu. 
5. Xuất kết quả cuối cùng. 

### Tại sao nó hoạt động 

Sau bước đầu tiên, hình dạng luôn là một viên kim cương đối xứng hoàn hảo được căn chỉnh theo trục lưới. Mỗi lớp mới đóng góp một đường viền hoàn chỉnh có chiều dài tăng tuyến tính theo chỉ số lớp. Vì mỗi lớp được hình thành đầy đủ và không chồng lên các ô đã thêm trước đó nên tổng số ô chính xác là tổng của các đóng góp ranh giới độc lập. Điều này đảm bảo rằng chuỗi số học mô hình chính xác sự tăng trưởng mà không bỏ sót hoặc đếm kép bất kỳ ô nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

if n == 1:
    print(1)
else:
    print(1 + 2 * n * (n - 1))
```Giải pháp dựa hoàn toàn vào biểu thức dạng đóng. Trường hợp đặc biệt$n = 1$được xử lý rõ ràng, mặc dù công thức đã đánh giá chính xác cho$n = 1$cũng vậy. 

biểu thức$2n(n-1)$được tính toán bằng cách sử dụng số nguyên Python, xử lý các giá trị vượt quá giới hạn một cách an toàn. Không tồn tại mối lo ngại tràn trung gian. 

Cấu trúc của mã phản ánh trực tiếp đạo hàm toán học mà không cần mô phỏng hoặc lưu trữ phụ trợ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 1$| Bước | n | Công thức | Kết quả | 
| --- | --- | --- | --- | 
| Đầu vào | 1 | - | - | 
| Trường hợp cơ sở | 1 | trở lại 1 | 1 | 

Điều này xác nhận rằng cấu hình cơ sở chứa chính xác một ô. 

### Ví dụ 2:$n = 4$| Bước | Tính toán | Giá trị | 
| --- | --- | --- | 
| n(n-1) | 4 × 3 | 12 | 
| 2n(n-1) | 2 × 12 | 24 | 
| +1 | 24 + 1 | 25 | 

Điều này phù hợp với mô hình tăng trưởng trực quan trong đó mỗi lớp thêm lần lượt 4, 8 và 12 ô, tổng cộng có 24 ô được thêm vào cộng với phần giữa. 

Dấu vết xác nhận rằng công thức tích lũy chính xác cấu trúc lớp được ngụ ý bởi cấu trúc hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số lượng phép tính số học không đổi được thực hiện bất kể$n$. | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ, chỉ sử dụng các biến số nguyên. | 

Các ràng buộc cho phép lên đến$n = 100$, nhưng giải pháp hoàn toàn không phụ thuộc vào kích thước ràng buộc. Kể cả nếu$n$lớn hơn đáng kể thì việc tính toán theo thời gian không đổi sẽ vẫn có giá trị. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return main()

def main():
    n = int(sys.stdin.readline().strip())
    if n == 1:
        return str(1)
    return str(1 + 2 * n * (n - 1))

# provided sample
assert run("1\n") == "1"

# small growth checks
assert run("2\n") == "5", "n=2 cross shape"
assert run("3\n") == "13", "next layer expansion"
assert run("4\n") == "25", "arithmetic growth"

# boundary case
assert run("100\n") == str(1 + 2 * 100 * 99), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp cơ sở đúng đắn | 
| 2 | 5 | lớp mở rộng đầu tiên | 
| 3 | 13 | tích lũy lớp thứ hai | 
| 4 | 25 | tính nhất quán của mô hình tăng trưởng | 
| 100 | 19801 | ổn định giới hạn trên | 

## Vỏ cạnh 

### Trường hợp$n = 1$Đầu vào là:```
1
```Thuật toán trả về ngay 1 mà không cần áp dụng công thức. Điều này ngăn chặn số học không cần thiết và phù hợp với định nghĩa trong đó không có sự mở rộng nào xảy ra. 

Dấu vết: 

| Bước | Hành động | Giá trị | 
| --- | --- | --- | 
| Đầu vào | đọc n | 1 | 
| Kiểm tra | n == 1 | đúng | 
| Đầu ra | trở lại | 1 | 

Kết quả phù hợp với thực tế là cấu hình ban đầu là một ô duy nhất. 

### Trường hợp$n = 100$đầu vào:```
100
```Dấu vết: 

| Bước | Tính toán | Giá trị | 
| --- | --- | --- | 
| n(n-1) | 100 × 99 | 9900 | 
| 2n(n-1) | 19800 | 19800 | 
| +1 | cuối cùng | 19801 | 

Ngay cả ở kích thước đầu vào tối đa, quá trình tính toán vẫn ổn định và không cần lặp lại. Công thức xử lý sự tăng trưởng đầy đủ một cách ngầm định, xác nhận tính đúng đắn trên quy mô lớn.
