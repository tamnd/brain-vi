---
title: "CF 105586A - \u6f14\u594f\u6625\u65e5\u5f71"
description: "Chúng tôi được cung cấp một danh sách ngắn các bài hát được biểu diễn dưới dạng chuỗi. Mỗi chuỗi là một mục chương trình duy nhất trong lịch trình buổi hòa nhạc ban đầu. Nhiệm vụ mô phỏng một quy tắc đơn giản được áp dụng khi đọc lịch trình này từ trên xuống dưới."
date: "2026-06-22T14:44:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105586
codeforces_index: "A"
codeforces_contest_name: "\u201c\u534e\u4e3a\u676f\u201d 2024 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u65b0\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u51b3\u8d5b\uff09"
rating: 0
weight: 105586
solve_time_s: 49
verified: true
draft: false
---

[CF 105586A - \u6f14\u594f\u6625\u65e5\u5f71](https://codeforces.com/problemset/problem/105586/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách ngắn các bài hát được biểu diễn dưới dạng chuỗi. Mỗi chuỗi là một mục chương trình duy nhất trong lịch trình buổi hòa nhạc ban đầu. 

Nhiệm vụ mô phỏng một quy tắc đơn giản được áp dụng khi đọc lịch trình này từ trên xuống dưới. Mỗi khi người biểu diễn nhìn thấy chuỗi chính xác`"Tomori"`, cô ấy liền chèn thêm một bài hát`"Haruhikage"`ngay sau nó trong đầu ra cuối cùng. Tất cả các chuỗi khác được sao chép không thay đổi và giữ nguyên thứ tự ban đầu. 

Vì vậy, đầu ra không chỉ là bản sao của danh sách đầu vào. Đó là một danh sách được chuyển đổi trong đó một số mục mở rộng thành hai mục liên tiếp tùy thuộc vào giá trị của chúng. 

Kích thước đầu vào rất nhỏ, tối đa 100 chuỗi và mỗi chuỗi có độ dài tối đa 20. Điều này loại bỏ mọi lo ngại về tối ưu hóa hiệu suất ngoài việc xử lý tuyến tính đơn giản. Chỉ cần vượt qua đầu vào một lần là đủ. 

Điều tinh tế duy nhất là việc kết hợp phải chính xác và phân biệt chữ hoa chữ thường. Các chuỗi như`"TOMORI"`hoặc`"tomori"`không kích hoạt chèn. Một cách tiếp cận ngây thơ cố gắng so sánh không phân biệt chữ hoa chữ thường hoặc khớp chuỗi con sẽ âm thầm tạo ra kết quả đầu ra không chính xác. 

Trường hợp góc thứ hai là khi`"Tomori"`xuất hiện liên tiếp. Trong tình huống đó, mỗi lần xuất hiện sẽ kích hoạt thao tác chèn một cách độc lập, do đó nhiều lần`"Haruhikage"`dòng có thể xuất hiện theo thứ tự. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề là xây dựng danh sách cuối cùng một cách rõ ràng. Chúng tôi đọc tất cả các chuỗi thành một mảng, sau đó lặp qua mảng đó và nối từng chuỗi vào danh sách kết quả. Bất cứ khi nào chúng ta gặp phải`"Tomori"`, chúng tôi cũng nối thêm`"Haruhikage"`ngay sau đó. 

Cách tiếp cận này đã tối ưu về cấu trúc. Cách giải thích bạo lực sẽ là liên tục xây dựng lại hoặc chèn vào danh sách ngày càng tăng bằng cách sử dụng các thao tác dịch chuyển các phần tử, chẳng hạn như chèn vào giữa một mảng. Điều đó dẫn đến chi phí không cần thiết: mỗi lần chèn vào cấu trúc dựa trên mảng có thể tốn O(n) và trong trường hợp xấu nhất là mọi phần tử đều bị`"Tomori"`, chúng tôi sẽ thực hiện các phép chèn O(n), dẫn đến hành vi O(n^2). 

Quan sát quan trọng là chúng ta không bao giờ cần sửa đổi cấu trúc ở giữa. Chúng tôi chỉ cần phát ra đầu ra theo kiểu phát trực tuyến. Điều này biến vấn đề thành một lần quét tuyến tính duy nhất trong đó mỗi phần tử đầu vào đóng góp một hoặc hai phần tử đầu ra. 

Cấu trúc của bài toán đảm bảo tính độc lập giữa các phần tử. Mỗi dòng có thể được xử lý mà không cần biết về các giá trị trong tương lai hoặc quá khứ, điều này giúp loại bỏ mọi nhu cầu về cấu trúc dữ liệu phức tạp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (chèn vào mảng/danh sách nhiều lần) | O(n^2) | O(n) | Không cần thiết nhưng có tác dụng với n nhỏ | 
| Tối ưu (chắp thêm/đầu ra một lần) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, cho biết có bao nhiêu chuỗi theo sau. 
2. Đối với mỗi điều tiếp theo`n`dòng, đọc chuỗi`s`. 
3. Xuất ngay`s`. 
4. Nếu`s`chính xác bằng`"Tomori"`, xuất thêm một dòng`"Haruhikage"`ngay sau nó. 

Lý do đằng sau kết quả đầu ra ngay lập tức là việc chuyển đổi chỉ phụ thuộc vào phần tử hiện tại. Không có sự phụ thuộc vào các yếu tố trong tương lai, vì vậy việc trì hoãn xử lý không mang lại lợi ích gì. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến đơn giản: sau khi xử lý chuỗi đầu vào thứ i, đầu ra chứa chính xác phiên bản được chuyển đổi của chuỗi i đầu tiên theo quy tắc và không có gì khác. Vì mỗi chuỗi được xử lý độc lập và thứ tự được giữ nguyên nên việc ghép tất cả các phép biến đổi cục bộ sẽ tạo ra kết quả chung chính xác. Không cần sắp xếp lại hoặc lưu vào bộ đệm vì phép chuyển đổi không bao giờ thay đổi thứ tự tương đối, chỉ chèn một chuỗi cố định sau các kết quả khớp cụ thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

for _ in range(n):
    s = input().strip()
    print(s)
    if s == "Tomori":
        print("Haruhikage")
```Việc thực hiện theo thuật toán trực tiếp. Đầu vào được xử lý từng dòng bằng cách sử dụng I/O nhanh. Mỗi chuỗi được loại bỏ các ký tự dòng mới ở cuối để đảm bảo so sánh chính xác. Kiểm tra có điều kiện là kiểm tra đẳng thức trực tiếp, thực thi phân biệt chữ hoa chữ thường. 

Chi tiết triển khai chính là in ngay lập tức thay vì lưu trữ kết quả. Mặc dù việc lưu trữ trong danh sách cũng có tác dụng nhưng điều đó là không cần thiết do có những ràng buộc nhỏ và tính đơn giản của đầu ra phát trực tuyến. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
Tomori
Neko
Tomori
```Chúng tôi xử lý từng chuỗi một cách tuần tự. 

| Bước | Chuỗi đầu vào | Đầu ra cho đến nay | 
| --- | --- | --- | 
| 1 | Tomori | Tomori Haruhikage | 
| 2 | Neko | Tomori Haruhikage Neko | 
| 3 | Tomori | Tomori Haruhikage Neko Tomori Haruhikage | 

Chuỗi thứ nhất và thứ ba kích hoạt quy tắc chèn, vì vậy`"Haruhikage"`xuất hiện hai lần trong đầu ra. 

Điều này chứng tỏ rằng mỗi lần xảy ra đều được xử lý độc lập, không có bất kỳ trạng thái chia sẻ nào giữa các bước. 

### Ví dụ 2 

đầu vào:```
4
TOMORI
Tomori
Tomo
Haruhikage
```| Bước | Chuỗi đầu vào | Đầu ra cho đến nay | 
| --- | --- | --- | 
| 1 | TOMORI | TOMORI | 
| 2 | Tomori | TOMORI Tomori Haruhikage | 
| 3 | Tomo | TOMORI Tomori Haruhikage Tomo | 
| 4 | Haruhikage | TOMORI Tomori Haruhikage Tomo Haruhikage | 

Chỉ chính xác`"Tomori"`phù hợp với việc chèn trình kích hoạt. Điều này xác nhận rằng các kết quả khớp chuỗi con và các biến thể chữ hoa chữ thường được bỏ qua một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chuỗi được đọc một lần và được xử lý bằng các phép toán so sánh và xuất O(1) | 
| Không gian | O(1) phụ trợ | Không yêu cầu cấu trúc dữ liệu bổ sung ngoài chuỗi đầu vào hiện tại | 

Quét tuyến tính dễ dàng đủ cho n lên tới 100 và thậm chí có thể mở rộng thoải mái vượt xa các ràng buộc của vấn đề. Việc sử dụng bộ nhớ không đổi ngoài việc đệm đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        n = int(sys.stdin.readline().strip())
        for _ in range(n):
            s = sys.stdin.readline().strip()
            print(s)
            if s == "Tomori":
                print("Haruhikage")
    return out.getvalue().strip()

# provided sample-style tests
assert run("3\nTomori\nNeko\nTomori\n") == "Tomori\nHaruhikage\nNeko\nTomori\nHaruhikage"

# single non-trigger case
assert run("2\nNeko\nRikki\n") == "Neko\nRikki"

# case sensitivity check
assert run("3\nTOMORI\nTomori\ntomori\n") == "TOMORI\nTomori\nHaruhikage\ntomori"

# consecutive triggers
assert run("2\nTomori\nTomori\n") == "Tomori\nHaruhikage\nTomori\nHaruhikage"

# minimum case
assert run("1\nTomori\n") == "Tomori\nHaruhikage"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 dòng trộn | được mở rộng bằng phần chèn | tính đúng đắn của phép biến đổi cơ bản | 
| không Tomori | đầu ra không đổi | hành vi truyền qua cơ sở | 
| các biến thể trường hợp | chỉ kích hoạt kết hợp chính xác | phân biệt chữ hoa chữ thường | 
| Tomori liên tiếp | xử lý chèn lặp đi lặp lại | sự độc lập của các sự kiện | 
| phần tử đơn | trường hợp ranh giới tối thiểu | độ đúng tại n=1 | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có chuỗi nào phù hợp cả. Ví dụ: nếu đầu vào là:```
2
Neko
Rikki
```thuật toán chỉ in mỗi dòng một lần và không thực hiện thao tác chèn thêm nào. Vì điều kiện không bao giờ được thỏa mãn nên không có đầu ra bổ sung nào xuất hiện và đầu ra giống hệt với chuỗi đầu vào. 

Một trường hợp khác là liên tiếp`"Tomori"`mục:```
2
Tomori
Tomori
```Thuật toán xử lý đầu tiên`"Tomori"`và in ngay lập tức`"Haruhikage"`, sau đó xử lý lần thứ hai một cách độc lập và thực hiện lại tương tự. Không có sự tương tác giữa hai bước nên kết quả chứa đúng hai dòng được chèn. 

Trường hợp thứ ba là các chuỗi trông giống với trình kích hoạt nhưng khác về trường hợp:```
1
TOMORI
```Vì phép so sánh là chính xác nên điều kiện không thành công và không có đầu ra bổ sung nào được tạo ra. Điều này xác nhận rằng việc kiểm tra tính bằng nhau là nghiêm ngặt và không dựa vào việc khớp mẫu hoặc chuẩn hóa.
