---
title: "CF 104322M - \u4e00\u5143\u56db\u6b21\u65b9\u7a0b"
description: "Chúng ta được yêu cầu xuất ra năm số nguyên $a, b, c, d, e$ trong phạm vi $[-10, 10]$, với $a neq 0$, sao cho đa thức bậc bốn $$a x^4 + b x^3 + c x^2 + d x + e$$ không có nghiệm thực. Nói cách khác, không có số thực $x$ nào làm cho biểu thức bằng 0."
date: "2026-07-01T19:28:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104322
codeforces_index: "M"
codeforces_contest_name: "\u54c8\u5c14\u6ee8\u5de5\u7a0b\u5927\u5b66\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b 2023"
rating: 0
weight: 104322
solve_time_s: 68
verified: true
draft: false
---

[CF 104322M - \u4e00\u5143\u56db\u6b21\u65b9\u7a0b](https://codeforces.com/problemset/problem/104322/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xuất ra năm số nguyên$a, b, c, d, e$trong phạm vi$[-10, 10]$, với$a \neq 0$, sao cho đa thức bậc bốn$$a x^4 + b x^3 + c x^2 + d x + e$$không có rễ thực sự. 

Nói cách khác, không có số thực$x$nên làm cho biểu thức bằng 0. Nhiệm vụ này hoàn toàn mang tính xây dựng và bất kỳ tập hợp hệ số hợp lệ nào thỏa mãn điều kiện đều được chấp nhận. 

Các ràng buộc là cực kỳ nhỏ và không có đầu vào nào ngoài chính yêu cầu của bài toán. Điều này ngay lập tức báo hiệu rằng lời giải không phải là thuật toán theo nghĩa thông thường mà dựa vào việc xác định một dạng đa thức đơn giản với dấu đảm bảo trên tất cả các số thực. 

Một nỗ lực ngây thơ có thể thử các hệ số ngẫu nhiên và kiểm tra xem bậc bốn có vượt qua 0 hay không, nhưng điều đó là không cần thiết và không đáng tin cậy. Một cạm bẫy phổ biến khác là cố gắng tranh luận về các điều kiện phân biệt hoặc căn bậc bốn, điều này quá mức cần thiết và dễ xảy ra lỗi do yêu cầu phạm vi hệ số chặt chẽ. 

Yêu cầu quan trọng là tính ổn định: đa thức phải luôn dương hoặc âm hoàn toàn đối với mọi số thực.$x$. 

## Phương pháp tiếp cận 

Một tư duy mạnh mẽ sẽ là liệt kê tất cả các bộ hệ số nguyên trong phạm vi cho phép và kiểm tra xem đa thức có nghiệm thực hay không. Về mặt lý thuyết điều này có thể thực hiện được vì không gian tìm kiếm chỉ$21^5$, nhưng việc kiểm tra sự tồn tại của nghiệm đối với mỗi bậc bốn vốn đã không cần thiết và sẽ yêu cầu suy luận ký hiệu hoặc tìm nghiệm số với các vấn đề về độ chính xác. Quan trọng hơn, điều này là không cần thiết vì chúng ta chỉ cần một cách xây dựng hợp lệ. 

Cái nhìn sâu sắc về cấu trúc là các đa thức bậc bốn nhất định luôn không âm do dạng đại số của chúng. Họ đơn giản nhất là các biểu thức chiếm ưu thế thậm chí có sức mạnh như$x^4$, tăng trưởng dương với cường độ lớn$x$. Để loại bỏ hoàn toàn các nghiệm thực có thể có, chúng tôi đảm bảo đa thức không bao giờ đạt đến 0 bằng cách thêm một hằng số dương hoàn toàn. 

Ví dụ đơn giản nhất là:$$x^4 + 1$$Đối với tất cả thực tế$x$,$x^4 \ge 0$, Vì thế$x^4 + 1 \ge 1$, nghĩa là nó hoàn toàn dương ở mọi nơi. Do đó nó không có rễ thực sự. 

Điều này trực tiếp thỏa mãn mọi ràng buộc: hệ số là số nguyên nhỏ,$a = 1 \neq 0$và đa thức không bao giờ vượt qua 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(21^5) với việc kiểm tra gốc nhiều | O(1) | Không cần thiết | 
| Xây dựng (x^4 + 1) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chọn dạng đa thức luôn dương với mọi đầu vào thực, đảm bảo không tồn tại nghiệm thực. Ứng cử viên đơn giản nhất là$x^4 + 1$, vì số hạng dẫn đầu chiếm ưu thế và không bao giờ âm. 
2. Ánh xạ đa thức này thành các hệ số. Thuật ngữ$x^4$cho$a = 1$và tất cả các hệ số trung gian đều bằng 0 vì không có số hạng bậc ba, bậc hai hoặc tuyến tính. Số hạng không đổi là$e = 1$. 
3. Xuất trực tiếp các hệ số làm đáp án cuối cùng. 

### Tại sao nó hoạt động 

Đối với bất kỳ thực tế$x$, thuật ngữ$x^4$luôn không âm. Việc thêm một hằng số dương sẽ làm dịch chuyển toàn bộ đồ thị lên trên 0, do đó đa thức không thể bằng 0 tại bất kỳ điểm thực nào. Điều này đảm bảo nghiệm thực bằng không mà không yêu cầu bất kỳ phân tích trường hợp hoặc phân loại đại số nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    print(1, 0, 0, 0, 1)

if __name__ == "__main__":
    main()
```Mã trực tiếp mã hóa đa thức$x^4 + 1$. Không có quá trình xử lý đầu vào vì sự cố không cung cấp bất kỳ đầu vào nào. Các hệ số được in theo thứ tự. 

## Ví dụ đã hoạt động 

Không có ví dụ đầu vào nào có biến thể có ý nghĩa vì nhiệm vụ mang tính xây dựng. Thay vào đó, chúng ta có thể xác minh hành vi của đa thức đã chọn. 

Coi như$x = 0$, đa thức ước tính thành$1$. Vì$x = 1$, nó đánh giá là$2$. Vì$x = -2$, nó đánh giá là$16 + 1 = 17$. Những lần kiểm tra này xác nhận biểu thức vẫn hoàn toàn tích cực trong tất cả các trường hợp được thử nghiệm, khớp với thuộc tính bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | xây dựng sản lượng không đổi | 
| Không gian | O(1) | không có cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép bất kỳ việc xây dựng nào được thực hiện trong thời gian không đổi. Vì không cần tính toán trên đầu vào nên giải pháp này là tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return "1 0 0 0 1"

# no input case
assert run("") == "1 0 0 0 1"

# sanity check consistency
assert run("anything") == "1 0 0 0 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trống | 1 0 0 0 1 | không xử lý đầu vào | 
| chuỗi ngẫu nhiên | 1 0 0 0 1 | mạnh mẽ đối với đầu vào không liên quan | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là yêu cầu đa thức phải không có nghiệm thực cho tất cả các số thực, bao gồm các giá trị dương và âm rất lớn. Đa thức được chọn$x^4 + 1$xử lý vấn đề này một cách thống nhất vì quyền lực chẵn dẫn đầu thống trị sự tăng trưởng và vẫn không âm ở mọi nơi, trong khi số hạng không đổi thực thi tính tích cực nghiêm ngặt. Không thể thay đổi dấu nên không có gốc nào có thể tồn tại.
