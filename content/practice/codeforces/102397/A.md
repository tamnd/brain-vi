---
title: "CF 102397A - Bashar và SHAWERMA!"
description: "Bashar muốn mua chính xác x bánh mì Shawerma. Mỗi chiếc bánh sandwich có giá chính xác là 2 JD, vì vậy số tiền cần thiết là giá của một chiếc bánh sandwich nhân với số lượng bánh sandwich. Dữ liệu đầu vào chứa một số nguyên x, biểu thị số lượng bánh mì mà Bashar muốn mua."
date: "2026-08-10T17:51:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102397
codeforces_index: "A"
codeforces_contest_name: "Asu Coding Cup 4"
rating: 0
weight: 102397
solve_time_s: 255
verified: true
draft: false
---

[CF 102397A - Bashar và SHAWERMA!](https://codeforces.com/problemset/problem/102397/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 15 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bashar muốn mua chính xác`x`bánh sandwich Shawerma. Mỗi chiếc bánh sandwich có giá chính xác là 2 JD, vì vậy số tiền cần thiết là giá của một chiếc bánh sandwich nhân với số lượng bánh sandwich. 

Đầu vào chứa một số nguyên`x`, đại diện cho số lượng bánh mì mà Bashar muốn mua. Đầu ra phải chứa tổng chi phí tính bằng JD. 

Ràng buộc`1 <= x <= 30`làm cho vấn đề trở nên cực kỳ nhỏ. Ngay cả một vòng lặp thực hiện một phép cộng cho mỗi bánh sandwich cũng sẽ yêu cầu tối đa 30 lần lặp, điều này không đáng kể trong giới hạn thời gian 1,5 giây. Quan trọng hơn, giá cố định của mỗi chiếc bánh sandwich có nghĩa là không cần phải kiểm tra từng chiếc bánh mì hoặc duy trì bất kỳ trạng thái phức tạp nào. Câu trả lời được xác định trực tiếp từ`x`. 

Không có trường hợp biên khó nào liên quan đến số 0 vì giá trị nhỏ nhất được phép là 1. Ví dụ: với đầu vào`1`, đầu ra đúng là`2`, vì Bashar mua một chiếc bánh sandwich. Một giải pháp bất cẩn mà vô tình in`x`sẽ sản xuất`1`, nhầm lẫn số lượng bánh mì với tổng giá của chúng. 

Ở ranh giới khác, đầu vào`30`yêu cầu`60`JD. Giải pháp chỉ thực hiện một phép nhân với giá sai hoặc vô tình coi giới hạn trên là 29 sẽ tạo ra kết quả không chính xác. Vì câu trả lời luôn chính xác hai lần`x`, cả hai ranh giới đều được xử lý theo cùng một công thức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ bắt đầu với tổng chi phí bằng 0 và thêm 2 JD một lần cho mỗi chiếc bánh sandwich. Điều này đúng vì mỗi lần lặp chiếm chính xác một bánh sandwich, vì vậy sau`x`số lần lặp lại số tiền tích lũy là`2 + 2 + ... + 2`, với`x`bản sao là 2. Ở giá trị đầu vào tối đa, thao tác này chỉ thực hiện 30 phép cộng, do đó nó đã đủ nhanh cho các ràng buộc đã cho. 

Cách tiếp cận bạo lực hoạt động vì đầu vào rất nhỏ nhưng không cần thiết. Quan sát quan trọng là mỗi lần lặp lại đều thêm cùng một lượng cố định, 2 JD. Cộng lặp lại cùng một giá trị`x`lần chính xác là nhân với`x`. Do đó toàn bộ vòng lặp có thể được thay thế bằng biểu thức`2 * x`. 

Không có điểm lỗi hiệu năng đáng kể nào đối với phương pháp brute-force theo ràng buộc đã nêu, vì 30 lần lặp là không đáng kể. Giải pháp tối ưu vẫn được ưu tiên hơn vì nó thể hiện trực tiếp cấu trúc toán học và giảm việc thực hiện xuống còn một phép nhân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(x), nhiều nhất là O(30) | O(1) | Được chấp nhận, nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`x`, số lượng bánh mì Shawerma mà Bashar muốn mua. Đây là giá trị duy nhất cần thiết vì mọi chiếc bánh sandwich đều có cùng mức giá. 
2. Nhân`x`qua`2`. Mỗi chiếc bánh sandwich đóng góp chính xác 2 JD, do đó nhân số lượng bánh mì với giá cố định sẽ ra tổng chi phí. 
3. In giá trị kết quả. Không cần làm tròn, chia hoặc xử lý bổ sung vì giá và số lượng bánh mì đều là số nguyên. 

### Tại sao nó hoạt động 

Tổng giá là tổng giá của mỗi chiếc bánh sandwich. Vì mỗi trong số`x`bánh mì có giá chính xác là 2 JD, số tiền đó là`2 + 2 + ... + 2`với`x`điều khoản. Theo định nghĩa của phép nhân, tổng này bằng`2 * x`. Thuật toán tính toán chính xác giá trị này nên kết quả in ra luôn là tổng chi phí cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x = int(input())
print(2 * x)
```Dòng đầu tiên nhập`sys`, Và`input`được giao cho`sys.stdin.readline`làm mẫu đầu vào nhanh tiêu chuẩn cho lập trình cạnh tranh. Đầu vào chỉ bao gồm một số nguyên, do đó không cần logic phân tích cú pháp đặc biệt. 

Giá trị được đọc vào`x`đại diện cho số lượng bánh sandwich. biểu hiện`2 * x`trực tiếp thực hiện công thức tính tổng chi phí. 

Không có vấn đề riêng lẻ nào vì phép tính không sử dụng vòng lặp hoặc chỉ mục. Cũng không có vấn đề tràn số nguyên trong Python và thậm chí với ràng buộc đã cho, kết quả lớn nhất có thể chỉ là`60`. 

Sau đó, chương trình sẽ in kết quả theo sau là một dòng mới, đây chính xác là định dạng đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

Câu lệnh được cung cấp trong lời nhắc không chứa các giá trị đầu vào và đầu ra mẫu thực tế, vì vậy hai dấu vết sau đây sử dụng đầu vào hợp lệ cụ thể. 

### Ví dụ 1 

đầu vào:```
1
```| Bước |`x`| Giá mỗi chiếc bánh sandwich | Tổng chi phí | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 1 | 2 | 0 | 
| Nhân | 1 | 2 | 2 | 
| In | 1 | 2 | 2 | 

Đầu vào đại diện cho một chiếc bánh sandwich. Nhân một chiếc bánh sandwich với giá của nó là 2 JD sẽ cho kết quả là`2`. Điều này cũng kiểm tra đầu vào nhỏ nhất được phép. 

### Ví dụ 2 

đầu vào:```
7
```| Bước |`x`| Giá mỗi chiếc bánh sandwich | Tổng chi phí | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 7 | 2 | 0 | 
| Nhân | 7 | 2 | 14 | 
| In | 7 | 2 | 14 | 

Giá bảy chiếc bánh sandwich`2 * 7 = 14`JD. Dấu vết cho thấy thuật toán không cần mô phỏng bảy lần mua hàng riêng biệt vì phép nhân thể hiện trực tiếp cùng một phép cộng lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một phép nhân số nguyên và một thao tác xuất được thực hiện. | 
| Không gian | O(1) | Chỉ có giá trị đầu vào và kết quả tính toán được lưu trữ. | 

Đầu vào được giới hạn tối đa là 30 bánh sandwich, do đó, ngay cả giải pháp bổ sung lặp lại đơn giản cũng sẽ dễ dàng phù hợp với giới hạn thời gian 1,5 giây và giới hạn bộ nhớ 256 MB. Giải pháp dựa trên phép nhân là thời gian không đổi và sử dụng bộ nhớ không đổi, làm cho nó thoải mái trong cả hai giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    input = sys.stdin.readline

    x = int(input())
    print(2 * x)

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Custom test cases based on the problem's missing sample section.
assert run("1\n") == "2\n", "minimum input"
assert run("2\n") == "4\n", "small boundary case"
assert run("15\n") == "30\n", "middle value"
assert run("30\n") == "60\n", "maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`2`| Số lượng bánh mì tối thiểu được phép | 
|`2`|`4`| Giá trị nhỏ và phép nhân cơ bản | 
|`15`|`30`| Đầu vào tầm trung | 
|`30`|`60`| Đầu vào tối đa được phép và ranh giới trên | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu, đầu vào chính xác là`1`. Thuật toán đọc`x = 1`, tính toán`2 * 1 = 2`, và bản in`2`. Một lỗi phổ biến là in trực tiếp dữ liệu đầu vào, điều này sẽ gây nhầm lẫn giữa số lượng bánh mì với số tiền cần thiết. 

Đối với đầu vào tối đa, đầu vào chính xác là`30`. Thuật toán tính toán`2 * 30 = 60`và in`60`. Vì tính toán không có ranh giới vòng lặp nên không có khả năng vô tình chỉ xử lý 29 chiếc bánh sandwich. 

giá trị`2`cũng hữu ích trong việc phát hiện sự hiểu lầm về giá cả. Với đầu vào`2`, Bashar mua hai chiếc bánh sandwich và số tiền cần thiết là`2 + 2 = 4`, vì vậy đầu ra đúng là`4`. Công thức tạo ra chính xác giá trị đó. 

Một giá trị trung bình như`15`cho`30`JD. Điều này xác nhận rằng giải pháp không dựa vào trường hợp đặc biệt nào cho cả hai điểm cuối và mối quan hệ giá cố định tương tự áp dụng trong toàn bộ phạm vi đầu vào hợp lệ.
