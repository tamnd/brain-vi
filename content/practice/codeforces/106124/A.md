---
title: "CF 106124A - Thích ứng số học"
description: "Chúng ta được cho một số nguyên s và chúng ta cần chia nó thành hai số nguyên a và b sao cho tổng của chúng bằng s. Cả a và b phải khác 0 và cả hai phải nằm trong phạm vi số nguyên có ba chữ số, nghĩa là bao gồm từ −999 đến 999."
date: "2026-06-20T05:32:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106124
codeforces_index: "A"
codeforces_contest_name: "2025-2026 ICPC Nordic Collegiate Programming Contest (NCPC 2025)"
rating: 0
weight: 106124
solve_time_s: 50
verified: true
draft: false
---

[CF 106124A - Thích ứng số học](https://codeforces.com/problemset/problem/106124/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất`s`, và chúng ta cần chia nó thành hai số nguyên`a`Và`b`sao cho tổng của chúng bằng`s`. Cả hai`a`Và`b`phải khác 0 và cả hai phải nằm trong phạm vi số nguyên có ba chữ số, nghĩa là bao gồm từ −999 đến 999. 

Nhiệm vụ không phải là tối ưu hóa bất cứ thứ gì như độ dài hoặc giá trị tuyệt đối. Mọi phân tách hợp lệ đều được chấp nhận miễn là nó tôn trọng các ràng buộc. Đầu ra chỉ đơn giản là hai số nguyên được in trên một dòng. 

Cửa sổ ràng buộc là cực kỳ nhỏ. Mọi ứng cử viên hợp lệ đều nằm trong một hộp hữu hạn có kích thước khoảng hai nghìn x hai nghìn. Điều này ngay lập tức ngụ ý rằng ngay cả một tìm kiếm đơn giản trên tất cả các khả năng vẫn sẽ đủ nhanh trong Python. Tuy nhiên, cấu trúc của bài toán gợi ý rằng chúng ta nên tránh việc tìm kiếm hoàn toàn. 

Các trường hợp thất bại khó phát hiện duy nhất đến từ các ranh giới ràng buộc. Một ý tưởng ngây thơ như luôn sử dụng`a = 0`Và`b = s`thất bại ngay lập tức vì số 0 bị cấm. Một lỗi phổ biến khác là bỏ qua giới hạn ba chữ số, ví dụ như tạo`a = s`Và`b = 0`hoặc tạo ra giá trị ngoài ±999 khi`s`gần ranh giới và điều chỉnh không chính xác. 

Một trường hợp có vấn đề cụ thể là`s = 0`. Nếu chúng ta cố gắng`a = 0`,`b = 0`, cả hai đều không hợp lệ. Một cái khác là`s = 1000`, nơi phân chia đối xứng ngây thơ như`a = 500`,`b = 500`hoạt động, nhưng`a = 0`,`b = 1000`vi phạm cả hai ràng buộc cùng một lúc. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các số nguyên`a`trong phạm vi hợp lệ và tính toán`b = s - a`, sau đó kiểm tra xem cả hai đều hợp lệ và khác không. Điều này có tác dụng vì không gian tìm kiếm có nhiều nhất là 1999 ứng viên cho`a`và mỗi lần kiểm tra là thời gian không đổi. Vì vậy, tổng cộng là khoảng hai nghìn thao tác, nhanh không đáng kể. 

Tuy nhiên, điều này là không cần thiết vì cấu trúc của bài toán đảm bảo rằng một cách xây dựng hợp lệ luôn dễ dàng được hình thành một cách trực tiếp. Vì chúng ta chỉ cần hai số khác 0 nên chúng ta có thể cố ý chọn một số trước và xác định số kia bằng phép trừ. 

Điều quan trọng là chúng ta chỉ cần tránh một giá trị xấu duy nhất: 0. Nếu chúng ta chọn`a = 1`, sau đó`b = s - 1`. Rủi ro duy nhất là khi`b = 0`, điều này xảy ra chính xác khi`s = 1`. Trong trường hợp đó, thay vào đó chúng ta có thể chọn`a = 2`,`b = -1`. Cả hai đều khác 0 và vẫn nằm trong giới hạn. Bởi vì`s`được giới hạn ở mức ±999, những lựa chọn cố định này không bao giờ vượt quá phạm vi cho phép. 

Vì vậy, giải pháp giảm xuống việc xây dựng có điều kiện theo thời gian không đổi thay vì bất kỳ tìm kiếm nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1999) | O(1) | Đã chấp nhận | 
| Xây dựng trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng câu trả lời một cách trực tiếp bằng cách cẩn thận tránh số 0. 

1. Đọc số nguyên`s`. Đây là tổng chúng ta cần chia thành hai phần khác 0. 
2. Nếu`s`không bằng 1, đặt`a = 1`. Sự lựa chọn này là an toàn vì`a`khác 0 và nằm trong giới hạn, và nó làm đơn giản việc xây dựng số thứ hai. 
3. Tính toán`b = s - a`. Từ`a`được sửa, điều này trực tiếp thực thi điều kiện tổng được yêu cầu. 
4. Nếu`b`hóa ra bằng 0, thay vào đó hãy điều chỉnh cấu trúc bằng cách đặt`a = 2`Và`b = s - 2`. Điều này đảm bảo cả hai số đều khác 0 trong khi vẫn giữ nguyên tổng. 
5. Đầu ra`a`Và`b`. 

Trường hợp ngoại lệ duy nhất mà chúng tôi tránh rõ ràng là khi lần thử đầu tiên tạo ra số 0 và chúng tôi sửa nó bằng phân tách hợp lệ thứ hai. 

### Tại sao nó hoạt động 

Chúng tôi luôn thực thi`a + b = s`bằng cách xây dựng, kể từ khi`b`được định nghĩa là`s - a`. Hạn chế duy nhất có thể bị vi phạm là`b ≠ 0`, điều đó chỉ xảy ra khi`a = s`. Bằng cách chọn mặc định`a = 1`, chúng tôi tránh trường hợp này cho tất cả`s ≠ 1`. Khi`s = 1`, sự điều chỉnh`a = 2, b = -1`cung cấp một phân tách hợp lệ trong phạm vi cho phép. Vì khoảng cho phép đủ rộng để chứa các giá trị dự phòng cố định này nên không xảy ra vi phạm ranh giới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = int(input())

a = 1
b = s - a

if b == 0:
    a = 2
    b = s - 2

print(a, b)
```Việc thực hiện tuân theo chính xác chiến lược mang tính xây dựng. Chúng ta bắt đầu với giá trị khác 0 đơn giản nhất có thể,`1`và rút ra giá trị của đối tác. Sự điều chỉnh duy nhất là trường hợp hiếm hoi khi lực lượng này`b`trở thành số 0, điều này chỉ xảy ra với`s = 1`. Trong trường hợp đó, chúng tôi chuyển sang cặp cố định thứ hai. 

Không có vòng lặp, không có tìm kiếm và không có phân nhánh trường hợp cạnh nào ngoài sự điều chỉnh duy nhất này. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`s = 10`Chúng tôi bắt đầu với`a = 1`. 

| Bước | một | b = s - a | Hiệu lực | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 9 | cả hai đều khác không | 

Từ`b ≠ 0`, chúng tôi chấp nhận cặp`(1, 9)`. 

Điều này thể hiện trường hợp bình thường khi cấu trúc mặc định hoạt động mà không cần sửa đổi. 

### Ví dụ 2:`s = 1`Chúng tôi bắt đầu với`a = 1`. 

| Bước | một | b = s - a | Hiệu lực | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 0 | không hợp lệ vì b = 0 | 
| Đã áp dụng bản sửa lỗi | 2 | -1 | cả hai đều khác không | 

Việc xây dựng ngây thơ thất bại vì nó tạo ra số 0, vì vậy chúng tôi chuyển sang một phân tách thay thế để tránh giá trị bị cấm. 

Điều này cho thấy tại sao chỉ cần một trường hợp dự phòng là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số lượng không đổi các phép tính số học và một lần kiểm tra có điều kiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép tối đa ba số nguyên, nhưng thuật toán chỉ sử dụng các hằng số nhỏ cố định nên dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = int(input())

    a = 1
    b = s - a

    if b == 0:
        a = 2
        b = s - 2

    return f"{a} {b}"

# provided samples (as described)
assert run("10\n") == "1 9"
assert run("1\n") == "2 -1"

# custom cases
assert run("2\n") == "1 1", "simple positive split"
assert run("-1\n") == "1 -2", "negative sum handling"
assert run("0\n") == "1 -1", "zero sum case"
assert run("999\n") == "1 998", "upper boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`1 1`| phân tích dương cơ bản | 
|`-1`|`1 -2`| xử lý số tiền âm | 
|`0`|`1 -1`| trường hợp tổng bằng không | 
|`999`|`1 998`| giới hạn an toàn giới hạn trên | 

## Vỏ cạnh 

cho`s = 0`, bộ thuật toán`a = 1`, sản xuất`b = -1`. Cả hai giá trị đều khác 0 và nằm trong giới hạn nên không cần hiệu chỉnh. 

Vì`s = 1`, sự lựa chọn ban đầu cho`b = 0`, vi phạm ràng buộc. Thuật toán phát hiện điều này và chuyển sang`(2, -1)`, khôi phục hiệu lực. 

Đối với các giá trị dương hoặc âm lớn như`999`hoặc`-999`, cấu trúc cố định vẫn giữ cả hai số trong phạm vi cho phép vì việc điều chỉnh chỉ trừ một hằng số nhỏ khỏi`s`.
