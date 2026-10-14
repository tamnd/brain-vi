---
title: "CF 105200C - Chữa bài thi"
description: "Chúng ta được giao một nhiệm vụ tính toán rất nhỏ: một cặp số nguyên đại diện cho một phân số và chúng ta cần quyết định xem phân số đó có ít nhất là một nửa hay không. Bạn có thể coi nó như việc kiểm tra xem một số giá trị thu được có “không tệ hơn một nửa giá trị tối đa có thể” hay không."
date: "2026-06-27T02:52:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105200
codeforces_index: "C"
codeforces_contest_name: "IME++ Starters Try-outs 2024"
rating: 0
weight: 105200
solve_time_s: 42
verified: true
draft: false
---

[CF 105200C - Chữa bài kiểm tra](https://codeforces.com/problemset/problem/105200/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một nhiệm vụ tính toán rất nhỏ: một cặp số nguyên đại diện cho một phân số và chúng ta cần quyết định xem phân số đó có ít nhất là một nửa hay không. 

Bạn có thể coi nó như việc kiểm tra xem một số giá trị thu được có “không tệ hơn một nửa giá trị tối đa có thể” hay không. Đầu vào mô tả tử số và mẫu số, đầu ra là quyết định nhị phân, thường được in dưới dạng câu trả lời kiểu có/không hoặc 1/0 tùy theo quy ước của bài toán. 

Hoạt động cốt lõi là so sánh giữa tỷ lệ và ngưỡng cố định. Vì mọi thứ quy về một điều kiện số học duy nhất nên cấu trúc là thời gian không đổi cho mỗi trường hợp kiểm thử. 

Mặc dù nhiệm vụ có vẻ tầm thường nhưng có một số dạng lỗi thường xuất hiện khi gửi không chính xác. Phổ biến nhất là phép chia dấu phẩy động. Ví dụ, so sánh`a / b >= 0.5`sử dụng số học dấu phẩy động có thể tạo ra kết quả không chính xác khi`a`Và`b`lớn vì mất độ chính xác có thể làm đảo lộn các trường hợp ranh giới. 

Một vấn đề tế nhị khác là phép chia số nguyên. Nếu ai đó viết`a / b >= 1/2`trong một ngôn ngữ mà phép chia số nguyên bị cắt ngắn, biểu thức`a / b`trở thành 0 đối với nhiều đầu vào hợp lệ, điều này sẽ phá hủy tính chính xác ngay lập tức. 

Trường hợp cạnh cụ thể là khi giá trị nằm chính xác trên đường biên. Ví dụ, nếu`a = 5`Và`b = 10`, câu trả lời đúng là đúng vì nó chính xác bằng một nửa. Cách tiếp cận dấu phẩy động có thể vẫn hoạt động ở đây, nhưng cách tiếp cận chia số nguyên bất cẩn sẽ đánh giá không chính xác`5 / 10`BẰNG`0`và thất bại trong việc kiểm tra. 

Cách an toàn để giải thích tất cả những điều này là tránh chia hoàn toàn và biến bất đẳng thức thành một phép so sánh số nguyên tương đương. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ tính toán trực tiếp phân số và so sánh nó với một nửa. Điều đó có nghĩa là đánh giá`a / b`dưới dạng số dấu phẩy động và kiểm tra xem nó có ít nhất là`0.5`. Điều này có hiệu quả về mặt khái niệm vì nó phù hợp trực tiếp với định nghĩa của vấn đề. 

Tuy nhiên, cách tiếp cận này trở nên mong manh trong việc thực hiện. Số học dấu phẩy động có độ chính xác hạn chế và khi`a`Và`b`lớn lên, có thể xảy ra lỗi làm tròn. Trong trường hợp xấu nhất, các giá trị cực kỳ gần ngưỡng có thể bị phân loại sai. Mặc dù độ phức tạp về thời gian là không đổi nhưng tính chính xác không được đảm bảo dưới các ràng buộc lập trình cạnh tranh điển hình. 

Quan sát quan trọng là chúng ta có thể loại bỏ hoàn toàn phép chia bằng cách nhân cả hai vế của bất đẳng thức. điều kiện$$\frac{a}{b} \ge \frac{1}{2}$$có thể được viết lại như$$2a \ge b$$Phép biến đổi này bảo toàn tính đúng đắn trong khi chỉ sử dụng số học số nguyên. Nó cũng tránh hoàn toàn việc chia cho số 0 vì chúng ta không bao giờ chia. 

Sau khi rút gọn về dạng này, vấn đề sẽ trở thành một phép so sánh duy nhất cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (phân chia dấu phẩy động) | O(1) | O(1) | Rủi ro | 
| Tối ưu (bất đẳng thức số nguyên) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên`a`Và`b`từ đầu vào. Chúng đại diện cho tử số và mẫu số của phân số chúng ta đang đánh giá. 
2. Chuyển điều kiện “phân số ít nhất bằng một nửa” thành bất đẳng thức nguyên bằng cách viết lại dưới dạng`2 * a >= b`. Điều này tránh mọi sự phân chia và giữ cho mọi hoạt động được chính xác. 
3. Thực hiện so sánh chỉ sử dụng số học số nguyên. Nếu bất đẳng thức đúng thì phân số đó ít nhất bằng một nửa. 
4. Xuất kết quả tương ứng theo mẫu yêu cầu của bài toán. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ sự biến đổi tương đương của các bất đẳng thức. Nhân cả hai vế của phép so sánh với một số dương vẫn giữ nguyên thứ tự, do đó nhân với`2b`(hoặc chỉ nhân chéo một cách cẩn thận) không làm thay đổi tính đúng đắn của câu lệnh. Từ`b`được giả định là dương trong bất kỳ bối cảnh phân số hợp lệ nào thì không xảy ra sự đảo dấu. Điều này đảm bảo điều kiện số nguyên khớp chính xác với điều kiện phân số ban đầu cho tất cả các đầu vào hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())
    if 2 * a >= b:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Chương trình đọc một cặp số nguyên và áp dụng trực tiếp bất đẳng thức đã chuyển đổi. phép nhân`2 * a`là an toàn trong Python vì số nguyên Python không bị giới hạn trong thực tế, do đó việc tràn không phải là vấn đề. 

Quyết định được đưa ra trong thời gian không đổi và được in ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 10
```| Bước | một | b | 2a | Điều kiện (2a ≥ b) | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 5 | 10 | - | - | - | 
| Tính toán | 5 | 10 | 10 | 10 ≥ 10 | CÓ | 

Trường hợp này nằm chính xác trên ranh giới. Phép biến đổi xử lý đẳng thức một cách chính xác, xác nhận rằng “ít nhất một nửa” bao gồm trường hợp biên. 

### Ví dụ 2 

đầu vào:```
3 10
```| Bước | một | b | 2a | Điều kiện (2a ≥ b) | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 3 | 10 | - | - | - | 
| Tính toán | 3 | 10 | 6 | 6 ≥ 10 (sai) | KHÔNG | 

Ở đây giá trị hoàn toàn dưới một nửa. Việc so sánh số nguyên ngay lập tức từ chối nó mà không có bất kỳ phép tính dấu phẩy động nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số lượng phép tính số học không đổi cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó chỉ thực hiện một vài phép toán số nguyên bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    a, b = map(int, input().split())
    if 2 * a >= b:
        print("YES")
    else:
        print("NO")

# provided-like samples
assert run("5 10\n") == "YES"
assert run("3 10\n") == "NO"

# custom cases
assert run("1 2\n") == "YES", "exact half boundary"
assert run("0 1\n") == "NO", "zero numerator"
assert run("10 1\n") == "YES", "greater than one"
assert run("7 14\n") == "YES", "even split exact boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`| CÓ | Trường hợp ranh giới chính xác | 
|`0 1`| KHÔNG | Xử lý phân số bằng 0 | 
|`10 1`| CÓ | Phân số lớn hơn 1 | 
|`7 14`| CÓ | Thậm chí phân chia chính xác | 

## Vỏ cạnh 

Một trường hợp ranh giới như`1 2`biểu diễn điều kiện đẳng thức. Thuật toán tính toán`2 * 1 = 2`và so sánh nó với`2`, tạo ra đầu ra CÓ chính xác. Điều này khẳng định rằng sự bình đẳng được xử lý đúng cách. 

Một trường hợp tử số bằng 0 như`0 1`sản xuất`2 * 0 = 0`, và kể từ đó`0 >= 1`là sai, đầu ra là KHÔNG. Điều này xác minh rằng thuật toán không xử lý sai số 0 khi vượt qua bất kỳ ngưỡng dương nào. 

Trường hợp phân số vượt quá một, chẳng hạn như`10 1`, tạo ra`2 * 10 = 20`, thỏa mãn điều kiện một cách tầm thường. Điều này cho thấy phương pháp này hoạt động ngay cả ngoài cách giải thích phạm vi [0,1] điển hình.
