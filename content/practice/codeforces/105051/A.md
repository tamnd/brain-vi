---
title: "CF 105051A - \u0412\u044b\u0431\u043e\u0440\u044b \u0432 \u0420\u0418\u041b\u0418"
description: "Ba ứng cử viên đang nhận được phiếu bầu trong một cuộc bầu cử. Ứng cử viên đầu tiên hiện có phiếu bầu, người thứ hai có b và người thứ ba có c. Nhiều phiếu bầu hơn có thể vẫn đến, nhưng chỉ phiếu bầu bổ sung cho ứng cử viên đầu tiên mới quan trọng."
date: "2026-06-28T00:35:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105051
codeforces_index: "A"
codeforces_contest_name: "2023-2024 \u0424\u0438\u043d\u0430\u043b \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u00ab\u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430\u00bb"
rating: 0
weight: 105051
solve_time_s: 47
verified: true
draft: false
---

[CF 105051A - \u0412\u044b\u0431\u043e\u0440\u044b \u0432 \u0420\u0418\u041b\u0418](https://codeforces.com/problemset/problem/105051/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ba ứng cử viên đang nhận được phiếu bầu trong một cuộc bầu cử. Ứng cử viên đầu tiên hiện có`a`phiếu bầu, người thứ hai có`b`, và thứ ba có`c`. Nhiều phiếu bầu hơn có thể vẫn đến, nhưng chỉ phiếu bầu bổ sung cho ứng cử viên đầu tiên mới quan trọng. 

Nhiệm vụ là xác định số phiếu bầu bổ sung nhỏ nhất phải được trao cho ứng cử viên đầu tiên sao cho sau khi cộng số phiếu này, ứng cử viên thứ nhất nắm giữ ít nhất một nửa tổng số phiếu bầu. “Ít nhất một nửa” có nghĩa là nếu chúng ta biểu thị số phiếu bầu cuối cùng được tính là`A`,`B`, Và`C`, sau đó`A`phải thỏa mãn`A ≥ (A + B + C) / 2`, hoặc tương đương`2A ≥ A + B + C`. 

Kích thước đầu vào lên tới`10^9`, loại trừ mọi mô phỏng thêm từng phiếu bầu một. Bất kỳ giải pháp nào cũng phải có thời gian không đổi, vì ngay cả việc lặp tuyến tính qua các phiếu bầu bổ sung có thể sẽ là quá chậm. 

Một điểm tế nhị là cách giải thích “50% phiếu bầu”. Vì phiếu bầu là số nguyên nên chúng tôi không xử lý việc làm tròn. Điều kiện hoàn toàn là số học: ứng cử viên đầu tiên phải có ít nhất một nửa tổng số tiền, không quá nhiều và không được làm tròn. 

Một sai lầm ngây thơ là nghĩ rằng chúng ta cần “mô phỏng cho đến khi chiếm đa số”, tăng số phiếu bầu từng bước một. Điều đó sẽ thất bại ngay lập tức dưới những ràng buộc lớn, và ngay cả những trường hợp nhỏ cũng sẽ chậm một cách không cần thiết. 

Một cách giải thích sai phổ biến khác là so sánh với mức tối đa của các ứng cử viên khác thay vì tổng số. Ví dụ, đảm bảo`a+x ≥ max(b, c)`là không đủ vì yêu cầu là về tổng số kết hợp chứ không phải sự thống trị theo cặp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tăng số lượng phiếu bầu bổ sung`x`bắt đầu từ số 0 và kiểm tra mỗi lần xem ứng cử viên đầu tiên có đạt được ít nhất một nửa tổng số phiếu bầu hay không. Mỗi lần kiểm tra yêu cầu tính lại tổng và xác minh bất đẳng thức, do đó nó có giá trị O(1), nhưng trong trường hợp xấu nhất`x`có thể lớn lên khoảng`b + c`, làm cho độ phức tạp tổng thể O(b + c). Với giá trị lên tới`10^9`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là điều kiện có thể được viết lại bằng đại số. Sau khi thêm`x`phiếu, ứng cử viên đầu tiên có`a + x`, và tổng số là`a + b + c + x`. Yêu cầu trở thành:`2(a + x) ≥ a + b + c + x`Việc mở rộng và đơn giản hóa sẽ loại bỏ sự phụ thuộc vào tổng số đang phát triển và giảm vấn đề về sự bất bình đẳng trực tiếp trong`x`. Điều này biến nhiệm vụ thành tính toán một biểu thức thay vì tìm kiếm. 

Cấu trúc của bài toán là tuyến tính vì việc thêm một phiếu bầu sẽ làm tăng cả tử số và mẫu số theo cách có thể dự đoán được. Tính đơn điệu này đảm bảo rằng một khi điều kiện trở thành đúng thì nó vẫn đúng, đó là lý do tại sao việc giải bất đẳng thức trực tiếp sẽ cho kết quả cực tiểu chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(b + c) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn số phiếu bầu bổ sung nhỏ nhất`x`sao cho ứng cử viên đầu tiên có ít nhất một nửa tổng số phiếu bầu sau khi thêm họ. 

1. Tính tổng số phiếu bầu hiện tại cho tất cả các ứng cử viên. Tổng số trước khi thêm phiếu bầu mới là`S = a + b + c`. 
2. Giả sử chúng ta thêm`x`phiếu bầu cho ứng cử viên đầu tiên, do đó số phiếu mới của nó trở thành`a + x`, và tổng số trở thành`S + x`. 
3. Chuyển điều kiện “ít nhất một nửa” thành bất đẳng thức:`2(a + x) ≥ S + x`. Điều này loại bỏ sự phân chia và tránh suy luận dấu phẩy động. 
4. Mở rộng bất đẳng thức để cô lập`x`. Điều này mang lại`2a + 2x ≥ a + b + c + x`. 
5. Đơn giản hóa bằng cách thu thập các thuật ngữ:`a + x ≥ b + c`. 
6. Giải quyết`x`, mang lại`x ≥ (b + c - a)`. 
7. Vì`x`không thể âm, lấy`x = max(0, b + c - a)`. 

Tại sao nó hoạt động 

Sự bất bình đẳng thể hiện chính xác điểm mà phần chia sẻ của ứng cử viên đầu tiên vượt qua hoặc đạt một nửa tổng số. Phép biến đổi loại bỏ sự phụ thuộc động giữa tử số và mẫu số và giảm mọi thứ thành một ràng buộc tuyến tính duy nhất. Vì phía bên trái phát triển nhanh hơn phía bên phải nên`x`tăng, số nguyên nhỏ nhất thỏa mãn bất đẳng thức chính xác là ngưỡng được tính ở trên và mọi giá trị nhỏ hơn đều không đáp ứng điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input().strip())
b = int(input().strip())
c = int(input().strip())

x = b + c - a
if x < 0:
    x = 0

print(x)
```Việc thực hiện áp dụng trực tiếp công thức dẫn xuất. Mỗi đầu vào được đọc độc lập và không cần xử lý thêm. 

Sự tinh tế duy nhất là đảm bảo kết quả không tiêu cực. Nếu ứng cử viên đầu tiên đã có đủ phiếu bầu để thỏa mãn điều kiện, giá trị được tính toán sẽ trở thành âm và chúng tôi giữ nó ở mức 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
15
9
```Chúng tôi tính toán`x = b + c - a = 15 + 9 - 7 = 17`. 

| Bước | a + x | tổng (a+b+c+x) | kiểm tra tình trạng | 
| --- | --- | --- | --- | 
| 0 | 7 | 31 | 14 < 31 | 
| 17 | 24 | 48 | 48 ≥ 48 | 

Điều này cho thấy 17 là điểm đầu tiên mà ứng cử viên đạt đúng một nửa tổng số phiếu bầu. 

### Ví dụ 2 

đầu vào:```
491
257
145
```Tính toán`x = 257 + 145 - 491 = -89`, Vì thế`x = 0`. 

| Bước | a + x | tổng cộng | kiểm tra tình trạng | 
| --- | --- | --- | --- | 
| 0 | 491 | 893 | 982 ≥ 893 | 

Ứng cử viên đầu tiên đã vượt quá một nửa tổng số phiếu bầu nên không cần bầu thêm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học trên ba số nguyên | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép các giá trị lên đến`10^9`, nhưng vì giải pháp hoàn toàn là số học nên không có nguy cơ tràn trong Python và không có mối lo ngại về hiệu năng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a = int(input().strip())
    b = int(input().strip())
    c = int(input().strip())

    x = b + c - a
    if x < 0:
        x = 0
    return str(x)

# provided samples
assert run("7\n15\n9\n") == "17"
assert run("491\n257\n145\n") == "0"

# custom cases
assert run("0\n1\n1\n") == "2"
assert run("10\n0\n0\n") == "0"
assert run("5\n100\n0\n") == "95"
assert run("100\n50\n49\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 1 1 | 2 | trường hợp tối thiểu trong đó lần đầu tiên bắt đầu từ 0 | 
| 10 0 0 | 0 | đã có đa số tuyệt đối | 
| 5 100 0 | 95 | mất cân đối lớn cần nhiều phiếu bầu | 
| 100 50 49 | 0 | ranh giới nơi đa số đã nắm giữ | 

## Vỏ cạnh 

Khi ứng cử viên đầu tiên đã có đủ phiếu bầu, biểu thức dẫn xuất`b + c - a`trở nên tiêu cực. Đối với đầu vào như`a = 100, b = 50, c = 49`, công thức cho`-1`, nhưng câu trả lời đúng là`0`. Bước kẹp đảm bảo độ chính xác. 

Đối với đầu vào ở đâu`a = 0`, chẳng hạn như`0, 1, 1`, công thức cho`2`, và xác minh từng bước cho thấy với hai phiếu được thêm vào, ứng cử viên đầu tiên đạt đúng một nửa tổng số phiếu bầu. 

Giá trị lớn gần`10^9`hoạt động giống hệt nhau vì giải pháp chỉ phụ thuộc vào tổng và hiệu, không phụ thuộc vào hành vi mô phỏng lặp hoặc chia tỷ lệ.
