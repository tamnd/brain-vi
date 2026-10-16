---
title: "CF 105481B - \u6bd4\u5206\u5e7b\u672f"
description: "Chúng ta được phát một bảng điểm viết theo dạng A-B, trong đó A là điểm của Alex và B là điểm của đối thủ. Cả hai giá trị đều là các chữ số đơn từ 0 đến 9. Hệ thống áp dụng thao tác “ảo giác điểm số” để hoán đổi hai giá trị một cách đơn giản."
date: "2026-06-23T01:58:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105481
codeforces_index: "B"
codeforces_contest_name: "2024 CCPC Liaoning Provincial Contest"
rating: 0
weight: 105481
solve_time_s: 44
verified: true
draft: false
---

[CF 105481B - \u6bd4\u5206\u5e7b\u672f](https://codeforces.com/problemset/problem/105481/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bảng điểm viết dưới dạng`A-B`, Ở đâu`A`đại diện cho điểm số của Alex và`B`đại diện cho điểm số của đối thủ. Cả hai giá trị đều là các chữ số đơn từ 0 đến 9. 

Hệ thống áp dụng thao tác “ảo giác điểm số” chỉ đơn giản là hoán đổi hai giá trị. Sau khi biến đổi, điểm của Alex trở thành điểm của đối thủ và điểm của đối thủ trở thành điểm của Alex. Đầu ra phải được in lại theo cùng một cách`A-B`định dạng, nhưng với các giá trị được hoán đổi. 

Các ràng buộc là cực kỳ nhỏ vì mỗi giá trị là một chữ số được mã hóa trực tiếp trong chuỗi đầu vào. Điều này có nghĩa là không cần phân tích số nguyên ngoài việc đọc ký tự hoặc tách chuỗi. Bất kỳ giải pháp nào chạy trong thời gian không đổi cho mỗi đầu vào đều đủ tầm thường. 

Sự tinh tế chính hoàn toàn là cú pháp: trích xuất chính xác hai số và giữ nguyên định dạng. Việc triển khai bất cẩn có thể vô tình coi toàn bộ chuỗi là một số hoặc phân chia không chính xác khi các biến thể định dạng xuất hiện. 

Trường hợp cạnh bê tông là`0-0`. Đầu ra đúng vẫn là`0-0`. Một cái khác là`9-0`, mà phải trở thành`0-9`. Bất kỳ phân tích cú pháp nào loại bỏ các số 0 đứng đầu hoặc giả sử số học có nhiều chữ số vẫn cần phải giữ nguyên các chữ số chính xác. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ coi A và B là các số, mô phỏng việc hoán đổi bằng cách chuyển đổi chúng thành số nguyên và xây dựng lại chuỗi. Điều này hoạt động chính xác vì việc hoán đổi không phụ thuộc vào cấu trúc số, nhưng đó là chi phí không cần thiết đối với đầu vào có kích thước cố định nhỏ như vậy. 

Sự đơn giản hóa thực sự đến từ việc nhận thấy rằng định dạng đầu vào rất cứng nhắc: chính xác một ký tự, sau đó là dấu gạch nối, rồi một ký tự. Không có sự mơ hồ, không có phân tích cú pháp nhiều chữ số và không xử lý khoảng trắng. Việc “tính toán” chỉ là vấn đề sắp xếp lại ký tự. 

Do đó, giải pháp giảm bớt việc đọc chuỗi và hoán đổi các ký tự xung quanh dấu gạch nối. Không có số học nào được yêu cầu cả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (phân tích số nguyên, hoán đổi, định dạng) | O(1) | O(1) | Đã chấp nhận | 
| Trao đổi chuỗi trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào`s`, có dạng`A-B`. Cấu trúc đảm bảo chính xác ba ký tự có ý nghĩa cộng với một dòng mới. 
2. Xác định ký tự đầu tiên`s[0]`là điểm ban đầu của Alex và nhân vật cuối cùng`s[2]`như điểm của đối thủ. Ký tự ở giữa luôn là dấu phân cách`-`. 
3. Xây dựng kết quả bằng cách đặt`s[2]`, sau đó`-`, sau đó`s[0]`. Điều này trực tiếp thực hiện “ảo ảnh” mà không cần chuyển đổi loại hoặc sửa đổi giá trị. 
4. Xuất chuỗi đã xây dựng. 

### Tại sao nó hoạt động 

Bài toán xác định phép biến đổi là một phép hoán đổi thuần túy của hai giá trị vô hướng độc lập được nhúng vào các vị trí cố định. Vì định dạng là bất biến và mỗi điểm chiếm một vị trí ký tự duy nhất nên việc hoán đổi các ký tự ở vị trí 0 và 2 sẽ duy trì tất cả ý nghĩa ngữ nghĩa. Không có sự phụ thuộc giữa các chữ số và không có hiệu ứng mang hoặc số học, do đó hoán vị ký tự khớp chính xác với phép biến đổi được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
print(f"{s[2]}-{s[0]}")
```Việc thực hiện hoàn toàn dựa vào việc lập chỉ mục vị trí. các`.strip()`đảm bảo rằng các ký tự dòng mới không ảnh hưởng đến việc lập chỉ mục. Vì định dạng được đảm bảo là`digit-digits`, lập chỉ mục`s[0]`Và`s[2]`luôn được an toàn. 

Một lỗi phổ biến là cố gắng chia cho`-`và quên xây dựng lại định dạng một cách chính xác. Trong khi`split('-')`cũng hoạt động, việc lập chỉ mục trực tiếp đơn giản hơn và tránh được việc phân bổ chuỗi trung gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`2-3`| Bước | A (bản gốc) | B (bản gốc) | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | Đọc chuỗi | "2-3" | 
| 2 | 2 | 3 | Trích xuất vị trí | s[0]=2, s[2]=3 | 
| 3 | 2 | 3 | Hoán đổi | "3-2" | 

Đầu ra:`3-2`Điều này xác nhận hoạt động hoàn toàn là vị trí và không phụ thuộc vào việc giải thích số. 

### Ví dụ 2 

đầu vào:`9-0`| Bước | A (bản gốc) | B (bản gốc) | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 0 | Đọc chuỗi | "9-0" | 
| 2 | 9 | 0 | Trích xuất vị trí | s[0]=9, s[2]=0 | 
| 3 | 9 | 0 | Hoán đổi | "0-9" | 

Đầu ra:`0-9`Điều này nhấn mạnh rằng các số 0 đứng đầu được giữ nguyên tự động vì chúng tôi không bao giờ chuyển đổi thành số nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ truy cập và định dạng ký tự theo thời gian cố định | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ ngoài chuỗi đầu vào | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó thực hiện một số thao tác cố định bất kể đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    s = _sys.stdin.readline().strip()
    return f"{s[2]}-{s[0]}"

# provided samples
assert run("2-3\n") == "3-2", "sample 1"

# custom cases
assert run("0-0\n") == "0-0", "all equal"
assert run("9-0\n") == "0-9", "boundary swap"
assert run("1-9\n") == "9-1", "max digits"
assert run("5-2\n") == "2-5", "general case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0-0 | 0-0 | giá trị giống hệt nhau | 
| 9-0 | 0-9 | chữ số ranh giới | 
| 1-9 | 9-1 | trao đổi phạm vi tối đa | 
| 5-2 | 2-5 | tính đúng đắn chung | 

## Vỏ cạnh 

đầu vào`0-0`chứng tỏ rằng việc hoán đổi các giá trị giống hệt nhau phải bảo toàn cùng một chuỗi. Thuật toán đọc`s[0]=0`Và`s[2]=0`, sau đó xây dựng`0-0`, phù hợp với kỳ vọng. 

Vì`9-0`, thuật toán trích xuất`9`Và`0`và đầu ra`0-9`. Không cần xử lý đặc biệt vì định dạng đầu ra hoàn toàn là vị trí, không phải số. 

Vì mỗi đầu vào hợp lệ chính xác là ba ký tự có ý nghĩa nên không có trường hợp cạnh cấu trúc nào ngoài nhận dạng chữ số mà thuật toán xử lý vốn có thông qua lập chỉ mục.
