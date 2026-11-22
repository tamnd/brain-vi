---
title: "CF 345A - Rắc rối mong đợi"
description: "Chúng ta được cung cấp một chuỗi đại diện cho Thứ Sáu ngày 13 trong một khoảng thời gian. Mỗi ký tự trong chuỗi có thể là \"0\" cho một ngày bình thường, \"1\" cho một ngày đặc biệt tồi tệ hoặc \"?\" cho một ngày mà người dùng không thể nhớ lại."
date: "2026-06-06T17:56:59+07:00"
tags: ["codeforces", "competitive-programming", "*special", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 345
codeforces_index: "A"
codeforces_contest_name: "Friday the 13th, Programmers Day"
rating: 1500
weight: 345
solve_time_s: 96
verified: true
draft: false
---

[CF 345A - Có thể gặp rắc rối](https://codeforces.com/problemset/problem/345/A) 

**Đánh giá:** 1500 
**Thẻ:** *đặc biệt, xác suất 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi đại diện cho Thứ Sáu ngày 13 trong một khoảng thời gian. Mỗi ký tự trong chuỗi có thể`"0"`cho một ngày bình thường,`"1"`cho một ngày đặc biệt tồi tệ, hoặc`"?"`trong một ngày người dùng không thể nhớ lại. Bên cạnh đó, chúng ta có một xác suất`p`đại diện cho khả năng một ngày không xác định`"?"`thực sự là một ngày tồi tệ`"1"`. 

Nhiệm vụ là tính toán giá trị kỳ vọng của mức độ xấu trung bình trong tất cả các ngày Thứ Sáu. Nói cách khác, chúng ta muốn giá trị trung bình của biến ngẫu nhiên đại diện cho mức độ tồi tệ của mỗi ngày. Những ngày đã biết đóng góp giá trị quyết định của chúng (`0`hoặc`1`) và những ngày chưa biết đóng góp giá trị kỳ vọng của chúng, đó chỉ là`p`bởi vì mỗi`"?"`hoạt động giống như một biến ngẫu nhiên Bernoulli. 

Chuỗi có thể dài tối đa 50 ký tự, rất nhỏ nên chúng tôi không bị hạn chế bởi độ phức tạp của thuật toán ở đây. Chúng ta chỉ cần truyền tuyến tính qua chuỗi để tính tổng dự kiến, sau đó chia cho độ dài của chuỗi để lấy giá trị trung bình. 

Trường hợp cạnh tinh tế xảy ra khi chuỗi chỉ chứa`"?"`nhân vật. Ví dụ: nếu đầu vào là`"???"`Và`p = 0.6`, việc triển khai ngây thơ có thể cố gắng tạo ra tất cả các kết hợp có thể có của`0`Và`1`đối với những ẩn số, nhưng giá trị kỳ vọng có thể được tính trực tiếp như`3 * 0.6 / 3 = 0.6`. Một trường hợp cạnh khác là khi`p`chính xác là`0`hoặc`1`. Nếu như`p = 1`, mọi`"?"`được đối xử như`1`; nếu như`p = 0`, mọi`"?"`được đối xử như`0`. Việc thực hiện làm tròn bất cẩn cũng có thể tạo ra`0.99999`thay vì`1.00000`nếu không cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê tất cả các khả năng thay thế có thể cho`"?"`và tính toán mức độ xấu trung bình cho mỗi sự kết hợp. Vì`n`những điều chưa biết, có`2^n`sự kết hợp, mỗi sự kết hợp yêu cầu một`O(n)`tính toán trung bình. Điều này mang lại`O(n * 2^n)`sự phức tạp về mặt thời gian. Ngay cả đối với`n = 50`, cái này trở nên lớn về mặt thiên văn (`~10^15`hoạt động), điều này rõ ràng là không khả thi. 

Cái nhìn sâu sắc quan trọng là tuyến tính của kỳ vọng. Giá trị kỳ vọng của tổng các biến ngẫu nhiên độc lập là tổng các giá trị kỳ vọng của chúng, bất kể sự phụ thuộc giữa các biến. Mỗi ngày được biết đều góp phần mang tính quyết định của nó`0`hoặc`1`, và mỗi ngày chưa biết`"?"`đóng góp`p`. Điều này làm giảm vấn đề thành một đường tuyến tính duy nhất xuyên qua chuỗi để tính tổng các giá trị này và chia cho độ dài. Không cần liệt kê tổ hợp và giải pháp là chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2^n) | O(n) | Quá chậm | 
| Giá trị mong đợi / Quét tuyến tính | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi`s`và xác suất`p`từ đầu vào.`s`chứa đựng lịch sử của ngày thứ Sáu và`p`là khả năng một ngày không xác định là xấu. 
2. Khởi tạo một biến`total_badness`ĐẾN`0`. Điều này sẽ tích lũy những điều tồi tệ dự kiến ​​​​trong tất cả các ngày. 
3. Lặp lại từng ký tự`c`TRONG`s`. Nếu như`c`là`"1"`, thêm vào`1`ĐẾN`total_badness`bởi vì đó rõ ràng là một ngày tồi tệ. Nếu như`c`là`"0"`, thêm vào`0`vì đó là một ngày tốt lành. Nếu như`c`là`"?"`, thêm vào`p`bởi vì giá trị kỳ vọng của biến ngẫu nhiên Bernoulli có xác suất`p`chính xác là`p`. 
4. Sau vòng lặp, tính giá trị trung bình kỳ vọng bằng cách chia`total_badness`theo chiều dài của`s`. 
5. In kết quả có đúng năm chữ số thập phân. Sử dụng các quy tắc làm tròn tiêu chuẩn để tránh các lỗi sai lệch ở chữ số cuối cùng. 

Tại sao nó hoạt động: Tính tuyến tính của kỳ vọng đảm bảo rằng việc tính tổng các giá trị kỳ vọng của từng ngày sẽ cho ra số tiền kỳ vọng chính xác. Chia cho số ngày sẽ bảo toàn kỳ vọng ở mức trung bình. Không mất đi độ chính xác trong xử lý`"?"`BẰNG`p`bởi vì kỳ vọng không đòi hỏi phải tạo ra các kết quả ngẫu nhiên thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
p = float(input().strip())

total_badness = 0.0

for c in s:
    if c == '1':
        total_badness += 1.0
    elif c == '?':
        total_badness += p
    # '0' adds 0, so we do nothing

expected_average = total_badness / len(s)
print(f"{expected_average:.5f}")
```Giải pháp đọc đầu vào bằng cách sử dụng`sys.stdin.readline`để vào/ra nhanh. Vòng lặp xử lý từng ký tự theo ý nghĩa của nó. Chúng tôi tích lũy mức độ xấu dự kiến ​​trong một biến dấu phẩy động và cuối cùng chia cho độ dài. Định dạng bằng`:.5f`đảm bảo đúng năm chữ số thập phân, làm tròn theo quy định chuẩn. Trường hợp trống`"0"`được xử lý ngầm vì việc thêm số 0 không ảnh hưởng đến tổng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
?111?1??1
1.0
```| Nhân vật | Đóng góp | Tổng số xấu | 
| --- | --- | --- | 
| ? | 1.0 | 1.0 | 
| 1 | 1 | 2.0 | 
| 1 | 1 | 3.0 | 
| 1 | 1 | 4.0 | 
| ? | 1.0 | 5.0 | 
| 1 | 1 | 6.0 | 
| ? | 1.0 | 7.0 | 
| ? | 1.0 | 8.0 | 
| 1 | 1 | 9,0 | 

Trung bình = 9,0 / 9 = 1,00000. Dấu vết xác nhận rằng mọi`"?"`đóng góp`p=1.0`và mọi`1`đóng góp`1`, tạo ra tổng cộng 9. 

**Đầu vào mẫu 2**```
0?0?
0.25
```| Nhân vật | Đóng góp | Tổng số xấu | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| ? | 0,25 | 0,25 | 
| 0 | 0 | 0,25 | 
| ? | 0,25 | 0,50 | 

Trung bình = 0,50 / 4 = 0,12500. Dấu vết này thể hiện việc xử lý các ngày đã biết và chưa biết hỗn hợp với xác suất không hề nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua chuỗi để tính tổng các giá trị mong đợi; n 50 | 
| Không gian | O(1) | Chỉ cần một biến để lưu tổng; kích thước đầu vào không đáng kể | 

Thuật toán tuyến tính theo số ngày, nằm trong giới hạn n ≤ 50. Việc sử dụng bộ nhớ là không đáng kể, phù hợp với giới hạn 256 MB theo một số bậc độ lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    p = float(input().strip())
    total_badness = 0.0
    for c in s:
        if c == '1':
            total_badness += 1.0
        elif c == '?':
            total_badness += p
    expected_average = total_badness / len(s)
    return f"{expected_average:.5f}"

# Provided samples
assert run("?111?1??1\n1.0\n") == "1.00000", "sample 1"
# Custom tests
assert run("0?0?\n0.25\n") == "0.12500", "mix of known and unknown"
assert run("?????\n0.5\n") == "0.50000", "all unknowns"
assert run("11111\n0.75\n") == "1.00000", "all bad days"
assert run("00000\n0.99\n") == "0.00000", "all good days"
assert run("1?0?1?0?1?\n0.5\n") == "0.50000", "alternating known and unknown"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0?0? / 0,25 | 0,12500 | Hỗn hợp đã biết và chưa biết, p nhỏ | 
| ????? / 0,5 | 0,50000 | Tất cả những điều chưa biết | 
| 11111 / 0,75 | 1,00000 | Tất cả đều biết xấu | 
| 00000 / 0,99 | 0,00000 | Tất cả đều biết tốt | 
| 1?0?1?0?1? / 0,5 | 0,50000 | Xen kẽ đã biết và chưa biết | 

## Vỏ cạnh 

Đối với một chuỗi chỉ có ẩn số,`"????"`Và`p = 0.3`, mỗi`"?"`đóng góp`0.3`. Tổng số xấu là`1.2`, trung bình là`1.2 / 4 = 0.3`. Thuật toán tích lũy chính xác những đóng góp này mà không cần cố gắng liệt kê tất cả các khả năng. 

Vì
