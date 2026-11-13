---
title: "CF 171B - Ngôi sao"
description: "Trong bài toán này, chúng ta được yêu cầu tính tổng số ô theo mô hình hình ngôi sao được vẽ trên lưới, cho một số lớp. Đầu vào là một số nguyên a, biểu thị số lớp đồng tâm trong ngôi sao."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "combinatorics"]
categories: ["algorithms"]
codeforces_contest: 171
codeforces_index: "B"
codeforces_contest_name: "April Fools Day Contest"
rating: 1300
weight: 171
solve_time_s: 177
verified: true
draft: false
---

[CF 171B - Ngôi sao](https://codeforces.com/problemset/problem/171/B) 

**Đánh giá:** 1300 
**Tags:** *đặc biệt, tổ hợp 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, chúng ta được yêu cầu tính tổng số ô theo mô hình hình ngôi sao được vẽ trên lưới, cho một số lớp. Đầu vào là một số nguyên duy nhất`a`, biểu thị số lớp đồng tâm trong ngôi sao. Đầu ra là một số nguyên duy nhất, là tổng số ô sẽ được tô màu nếu chúng ta vẽ tất cả`a`các lớp của ngôi sao 

Mỗi lớp của ngôi sao tăng kích thước một cách đối xứng nên số lượng ô tăng theo một công thức tổ hợp đơn giản. Lớp đầu tiên là một ô duy nhất ở trung tâm. Lớp thứ hai thêm các ô xung quanh nó tạo thành một mẫu hình chữ thập cộng với các góc và mỗi lớp tiếp theo sẽ thêm nhiều ô hơn theo cùng một cấu trúc đối xứng. 

Những ràng buộc cho chúng ta biết rằng`a`có thể lên tới 18257. Do đầu ra có thể đạt tới 2·10⁹, điều này ngụ ý rằng bất kỳ mô phỏng đơn giản nào về việc lấp đầy từng lớp lưới theo từng lớp sẽ quá chậm và tốn nhiều bộ nhớ. Chúng ta cần một công thức dạng đóng cho số lượng ô để xử lý đầu vào tối đa một cách hiệu quả. 

Một trường hợp cạnh là khi`a = 1`. Đầu ra đúng là 1, vì chỉ có ô trung tâm. Một cách tiếp cận bất cẩn giả định ít nhất hai lớp hoặc sử dụng công thức mà không xử lý lớp đầu tiên một cách rõ ràng sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng việc vẽ rõ ràng từng lớp của ngôi sao, đếm từng ô được thêm vào. Lớp đầu tiên thêm 1, lớp thứ hai thêm 12, lớp thứ ba thêm 24, v.v. Mặc dù điều này đúng về mặt khái niệm nhưng nó sẽ yêu cầu lặp lại từng lớp và từng ô trong lớp. Với`a = 18257`, tổng số tế bào tăng lên gần bằng`6*a^2 + 6*a + 1`, tức là khoảng 2·10⁹ phép tính lớn nhất`a`. Điều này quá chậm và vượt quá giới hạn bộ nhớ nếu chúng ta cố gắng lưu trữ lưới. 

Cái nhìn sâu sắc quan trọng là nhận ra mô hình về số lượng ô. Chuỗi tổng số ô là một dãy bậc hai: lớp thứ nhất là 1, lớp thứ hai là 13, lớp thứ ba là 37, v.v. Bằng cách kiểm tra sự khác biệt giữa các lớp, chúng tôi nhận thấy rằng mỗi lớp mới sẽ bổ sung thêm`12 * (layer - 1)`tế bào. Điều này dẫn đến một công thức khép kín tính tổng số ô trong`a`lớp:`1 + 6*a*(a-1)`. 

Cách tiếp cận vũ phu có hiệu quả đối với nhỏ`a`bởi vì chúng tôi có thể mô phỏng từng lớp một cách an toàn nhưng lại thất bại`a`nhờ sự tăng trưởng bậc hai. Việc quan sát rằng chuỗi là bậc hai cho phép chúng ta tính kết quả trực tiếp trong thời gian không đổi mà không cần lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a²) | O(a²) | Quá chậm cho đầu vào tối đa | 
| Công thức dạng đóng | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`từ đầu vào. Điều này đại diện cho số lượng lớp trong ngôi sao. 
2. Tính tổng số ô bằng công thức đóng`1 + 6*a*(a-1)`. Công thức này phát sinh từ việc quan sát rằng mỗi lớp`i`(Ở đâu`i ≥ 2`) thêm`12*(i-1)`các ô và tính tổng các ô này sẽ cho một biểu thức bậc hai. 
3. In tổng số đã tính. 

Lý do điều này có tác dụng là vì công thức nắm bắt chính xác mô hình cộng ô. Lớp đầu tiên đóng góp 1. Mỗi lớp tiếp theo sẽ thêm tổng cộng 12 ô so với lớp trước đó, tạo ra chuỗi hiệu số 12, 24, 36, v.v., tổng thành công thức bậc hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input())
result = 1 + 6 * a * (a - 1)
print(result)
```Giải pháp đọc một số nguyên duy nhất`a`, tính tổng số ô bằng công thức dẫn xuất và in nó. Công thức chỉ sử dụng số học số nguyên nên không có vấn đề về dấu phẩy động. Chúng tôi nhân lên`6*a*(a-1)`đầu tiên, điều này tránh tràn số nguyên vì Python tự động xử lý các số nguyên lớn. 

## Ví dụ đã hoạt động 

**Mẫu 1:** Đầu vào`2`| Bước | một | Tính công thức | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 2 | | | 
| Tính toán các ô | 2 | 1 + 6_2_(2-1) = 1 + 12 | 13 | 
| Đầu ra in | | | 13 | 

Điều này xác nhận rằng công thức chiếm chính xác hai lớp đầu tiên. 

**Mẫu 2:** Đầu vào`3`| Bước | một | Tính công thức | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 3 | | | 
| Tính toán các ô | 3 | 1 + 6_3_(3-1) = 1 + 36 | 37 | 
| Đầu ra in | | | 37 | 

Dấu vết cho thấy công thức nắm bắt chính xác mô hình tăng trưởng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Đánh giá công thức đơn không có vòng lặp | 
| Không gian | O(1) | Chỉ lưu trữ đầu vào và kết quả | 

Thuật toán thực hiện trong không gian và thời gian không đổi bất kể`a`. Thậm chí ở mức tối đa`a = 18257`, việc tính toán không quan trọng đối với các CPU hiện đại và phù hợp thoải mái với kiểu số nguyên của Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a = int(sys.stdin.readline())
    return str(1 + 6 * a * (a - 1))

# provided sample
assert run("2\n") == "13", "sample 1"
# custom cases
assert run("1\n") == "1", "minimum input"
assert run("3\n") == "37", "three layers"
assert run("18257\n") == str(1 + 6*18257*18256), "maximum input"
assert run("10\n") == "541", "ten layers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Trường hợp cạnh đầu vào tối thiểu | 
| 2 | 13 | Cung cấp mẫu, số lượng lớp nhỏ | 
| 3 | 37 | Tổng hợp chính xác nhiều lớp | 
| 18257 | 1+6_18257_18256 | Xử lý đầu vào tối đa | 
| 10 | 541 | Đầu vào tầm trung điển hình | 

## Vỏ cạnh 

cho`a = 1`, công thức đánh giá thành`1 + 6*1*(1-1) = 1`. Thuật toán xuất ra chính xác 1 mà không cần xử lý đặc biệt. Đối với đầu vào lớn như`a = 18257`, công thức tính`1 + 6*18257*18256`trực tiếp, tránh mọi sự lặp lại hoặc mô phỏng tốn nhiều bộ nhớ. Tất cả các sản phẩm trung gian đều nằm trong phạm vi số nguyên của Python. Điều này đảm bảo kết quả chính xác cho toàn bộ phạm vi đầu vào.
