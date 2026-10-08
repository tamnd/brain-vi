---
title: "CF 104968A - Thiên Đường Pepperoni"
description: "Chiếc bánh pizza được biểu diễn dưới dạng lưới vuông có kích thước $N nhân N$, trong đó mỗi ô chứa một ký tự bảng chữ cái. Trong số các ký tự này, chỉ có hai ký hiệu quan trọng đối với chúng ta: chữ hoa 'P' và chữ thường 'p'."
date: "2026-06-28T06:47:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104968
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 2 (Beginner)"
rating: 0
weight: 104968
solve_time_s: 69
verified: true
draft: false
---

[CF 104968A - Thiên đường Pepperoni](https://codeforces.com/problemset/problem/104968/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chiếc bánh pizza được biểu diễn dưới dạng một lưới hình vuông có kích thước$N \times N$, trong đó mỗi ô chứa một ký tự bảng chữ cái. Trong số các ký tự này, chỉ có hai ký hiệu quan trọng đối với chúng ta: chữ hoa 'P' và chữ thường 'p'. Mỗi lần xuất hiện của một trong hai ký tự này đại diện cho một pepperoni được đặt trên ô đó của chiếc bánh pizza. Tất cả các chữ cái khác là đồ trang trí không liên quan. 

Nhiệm vụ là quét toàn bộ lưới và đếm tổng cộng có bao nhiêu pepperonis. Vì mỗi ô có thể đóng góp nhiều nhất một pepperoni nên câu trả lời chỉ đơn giản là số ô chứa 'P' hoặc 'p'. 

Ràng buộc$N \leq 100$có nghĩa là lưới có nhiều nhất$10^4$tế bào. Ngay cả việc quét đơn giản trên tất cả các ô cũng cực kỳ rẻ. Giải pháp kiểm tra từng ô một lần đã là giải pháp tối ưu; bất cứ điều gì phức tạp hơn quét tuyến tính sẽ không cần thiết. 

Các trường hợp chính chủ yếu liên quan đến việc giải thích đầu vào hơn là độ khó về thuật toán. Lưới được đảm bảo chính xác$N$dòng của$N$mỗi ký tự, nhưng việc phân tích cú pháp đầu vào bất cẩn vẫn có thể sai nếu người ta giả sử các mã thông báo được phân tách bằng dấu cách hoặc quên xử lý dòng mới. 

Ví dụ, nếu$N = 1$và lưới là:```
P
```câu trả lời là 1. Nếu:```
a
```câu trả lời là 0. Nếu:```
p
```câu trả lời cũng là 1. Một lỗi phổ biến là chỉ kiểm tra chữ 'P' viết hoa và quên chữ 'p' viết thường, điều này sẽ tạo ra 0 không chính xác cho pepperonis hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là lặp qua từng ô trong lưới và kiểm tra xem ký tự là 'P' hay 'p'. Điều này có tác dụng vì mỗi ô độc lập và đóng góp một giá trị cố định là 1 hoặc 0 vào tổng số. Tổng hợp những đóng góp này sẽ đưa ra câu trả lời cuối cùng. 

Một mô hình tinh thần ngây thơ hơn một chút có thể coi lưới như một tập hợp các chuỗi và liên tục tìm kiếm sự xuất hiện của “P” hoặc “p” bằng cách sử dụng tìm kiếm chuỗi con hoặc quét lặp lại. Đây vẫn là tuyến tính trong tổng kích thước đầu vào, nhưng gây ra chi phí không cần thiết trong các hoạt động chuỗi và truyền lặp lại trên cùng một dữ liệu. Trong trường hợp xấu nhất, nó vẫn có thể chạm tới tất cả$N^2$ký tự nhiều lần, điều này có thể tránh được. 

Sự đơn giản hóa chính là nhận ra rằng không có mối quan hệ không gian nào quan trọng. Không có sự nhóm, không có sự liền kề và không có sự chuyển đổi. Mỗi ký tự xác định độc lập xem có tăng câu trả lời hay không. Điều này thu gọn vấn đề thành một lần chuyển qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lặp đi lặp lại Brute Force | O(N^2) đến O(N^3) | O(1) | Quá chậm/không cần thiết | 
| Đếm một lần | O(N^2) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$N$, xác định cả số hàng và số cột trong lưới. Điều này đặt tổng số ký tự chúng tôi sẽ xử lý. 
2. Khởi tạo một biến đếm về 0. Biến này tích lũy số lượng pepperonis gặp phải trong quá trình quét. 
3. Lặp lại từng bước$N$hàng. Mỗi hàng là một chuỗi có độ dài$N$, đại diện cho một lát ngang của lưới pizza. 
4. Đối với mỗi ký tự trong hàng hiện tại, hãy kiểm tra xem nó có bằng ‘P’ hay ‘p’ hay không. Nếu nó khớp với một trong hai, hãy tăng bộ đếm lên một. Bước này hợp lý vì mỗi ô phù hợp sẽ đóng góp chính xác một pepperoni. 
5. Sau khi xử lý tất cả các hàng và tất cả ký tự, xuất ra giá trị cuối cùng của bộ đếm. 

### Tại sao nó hoạt động 

Thuật toán dựa trên ánh xạ trực tiếp một-một giữa các ký tự hợp lệ và đóng góp cho câu trả lời. Mỗi ô lưới được kiểm tra chính xác một lần và mỗi ô đóng góp độc lập 0 hoặc 1. Vì không có sự chồng chéo hoặc tương tác giữa các ô nên việc tính tổng các đóng góp cục bộ sẽ tạo ra tổng số toàn cầu. Bất biến được duy trì trong suốt quá trình quét là bộ đếm bằng số lượng ô pepperoni được nhìn thấy cho đến nay, do đó, khi kết thúc, nó bằng tổng số trong lưới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    cnt = 0

    for _ in range(n):
        row = input().strip()
        for ch in row:
            if ch == 'P' or ch == 'p':
                cnt += 1

    print(cnt)

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng hàng dưới dạng một chuỗi thô và lặp lại từng ký tự. sử dụng`strip()`đảm bảo rằng các ký tự dòng mới không cản trở quá trình xử lý, đây là nguyên nhân phổ biến gây ra lỗi từng lỗi một trong các sự cố về lưới. Điều kiện kiểm tra rõ ràng cả dạng chữ hoa và chữ thường, ngăn chặn các kết quả trùng khớp bị bỏ lỡ. 

Bộ đếm được cập nhật ngay lập tức khi tìm thấy ký tự hợp lệ, do đó không cần lưu trữ trung gian hoặc tiền xử lý lưới. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
SPWXjSSaO
```Chúng tôi hiểu đây là lưới 3 ký tự mỗi hàng nhưng chỉ một hàng được hiển thị trong định dạng mẫu. Ý tưởng chính vẫn là: chúng tôi quét tất cả các ký tự và đếm 'P' và 'p'. 

| Bước | Hàng | Nhân vật | Hành động | Quầy | 
| --- | --- | --- | --- | --- | 
| 1 | SPWXjSSaO | S | bỏ qua | 0 | 
| 2 | SPWXjSSaO | P | tăng | 1 | 
| 3 | SPWXjSSaO | W | bỏ qua | 1 | 
| ... | ... | ... | ... | 1 | 
| kết thúc | SPWXjSSaO | Ồ | bỏ qua | 1 | 

Đầu ra cuối cùng:```
1
```Điều này xác nhận rằng chỉ có một ký tự pepperoni hợp lệ duy nhất xuất hiện trong toàn bộ lưới. 

### Mẫu 2 

đầu vào:```
4
APZaqPbpcCaAXyPZ
```Một lần nữa, chúng tôi quét toàn bộ cấu trúc từng hàng. 

| Bước | Hàng | Nhân vật | Hành động | Quầy | 
| --- | --- | --- | --- | --- | 
| 1 | APZaqPbpcCaAXyPZ | A | bỏ qua | 0 | 
| 2 | APZaqPbpcCaAXyPZ | P | tăng | 1 | 
| 3 | APZaqPbpcCaAXyPZ | Z | bỏ qua | 1 | 
| 4 | APZaqPbpcCaAXyPZ | một | bỏ qua | 1 | 
| 5 | APZaqPbpcCaAXyPZ | q | bỏ qua | 1 | 
| 6 | APZaqPbpcCaAXyPZ | P | tăng | 2 | 
| ... | ... | ... | ... | 4 | 

Đầu ra cuối cùng:```
4
```Điều này chứng tỏ rằng cả hai biến thể chữ hoa và chữ thường đều đóng góp như nhau và nhiều lần xuất hiện được tích lũy một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^2) | Mỗi trong số$N^2$các ô được truy cập đúng một lần | 
| Không gian | O(1) | Chỉ có một bộ đếm duy nhất được duy trì bất kể kích thước lưới | 

Kích thước đầu vào tối đa là$10^4$các ký tự, vì vậy việc quét tuyến tính đơn lẻ là không đáng kể trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào, không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (interpreted in grid form)
assert run("1\nP\n") == "1"
assert run("1\na\n") == "0"

# custom cases
assert run("2\nPP\npp\n") == "4", "all pepperonis both cases"
assert run("3\nabc\ndef\nghi\n") == "0", "no pepperonis"
assert run("3\nPpp\nPPP\nppp\n") == "9", "mixed grid"
assert run("1\nZ\n") == "0", "single non-match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 P | 1 | trường hợp tích cực tối thiểu | 
| 1x1 một | 0 | trường hợp tiêu cực tối thiểu | 
| hỗn hợp 2x2 | 4 | xử lý phân biệt chữ hoa chữ thường | 
| tất cả không phải P/p | 0 | không có kết quả dương tính giả | 
| lưới hỗn hợp đầy đủ | 9 | tích lũy đúng đắn | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi lưới chỉ chứa các chữ cái viết thường và không có chữ 'P' viết hoa. Đối với đầu vào:```
2
pp
pp
```quá trình quét xử lý từng ký tự và số gia cho mỗi 'p', tạo ra 4. Việc triển khai có lỗi chỉ kiểm tra chữ hoa 'P' sẽ trả về 0, nhưng logic đúng sẽ xử lý cả hai trường hợp một cách đối xứng. 

Một trường hợp khác là lưới nhỏ nhất:```
1
P
```Vòng lặp chạy một lần, thấy kết quả khớp và trả về 1. Logic tương tự cũng xử lý:```
1
z
```trong đó không có sự gia tăng nào xảy ra và đầu ra vẫn bằng 0. Điều này xác nhận rằng thuật toán hoạt động nhất quán ngay cả ở ranh giới nơi lưới thu gọn thành một ô duy nhất.
