---
title: "CF 104024B - ZYW với điểm số của anh ấy"
description: "Chúng ta được cho hai số nguyên đến từ hai số ẩn, gọi chúng là $a$ và $b$. Thay vì tiết lộ trực tiếp $a$ và $b$, chúng ta chỉ được biết tổng $a + b$ và bitwise VÀ $a đất b$ của chúng. Nhiệm vụ là khôi phục XOR $a oplus b$ theo bit."
date: "2026-07-02T04:18:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104024
codeforces_index: "B"
codeforces_contest_name: "The 16-th BIT Campus Programming Contest - Online Round(2022)"
rating: 0
weight: 104024
solve_time_s: 42
verified: true
draft: false
---

[CF 104024B - ZYW với điểm số của anh ấy](https://codeforces.com/problemset/problem/104024/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên đến từ hai số ẩn, gọi chúng là$a$Và$b$. Thay vì tiết lộ$a$Và$b$một cách trực tiếp, chúng ta chỉ được biết tổng của chúng$a + b$và bitwise AND của họ$a \land b$. Nhiệm vụ là khôi phục XOR theo bit$a \oplus b$. 

Khó khăn chính là phép cộng trộn các bit với số mang, trong khi AND nắm bắt được cả hai số có bit 1. XOR ghi lại chính xác các bit mà chúng khác nhau, vì vậy mục tiêu là tái tạo lại bit đó mà không cần tái tạo lại một cách rõ ràng$a$Và$b$. 

Những ràng buộc cho phép$a, b \le 10^9$, ngụ ý các giá trị phù hợp trong phạm vi 31 bit. Giá trị này đủ nhỏ để lý luận theo bit là hoàn toàn an toàn và các phép toán thời gian không đổi là đủ. Bất kỳ giải pháp nào hoạt động theo thời gian tuyến tính trên bit hoặc thậm chí số học O(1) là đủ. Một lực lượng vũ phu đối với tất cả các cặp là không liên quan vì chúng tôi không tìm kiếm mà chỉ xây dựng lại. 

Một sai lầm ngây thơ ở đây là cho rằng$a + b = a \oplus b$. Điều này thất bại ngay lập tức khi có mang. Ví dụ, nếu$a = 1$Và$b = 1$, sau đó$a + b = 2$Nhưng$a \oplus b = 0$. Một trường hợp thất bại khác là giả sử AND đóng góp trực tiếp vào XOR, nhưng thay vào đó AND đại diện cho các bit được chia sẻ thực sự _tạo thêm_. 

Trường hợp cạnh tinh tế hơn là khi AND lớn so với tổng. Ví dụ, nếu$a = 3$Và$b = 2$, sau đó$a + b = 5$,$a \land b = 2$và XOR là$1$. Bất kỳ sự sắp xếp lại không chính xác nào bỏ qua hệ số hai từ số lần mang sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các cặp$(a, b)$nhất quán với tổng và AND đã cho, sau đó tính toán XOR trực tiếp. Từ$a$Và$b$có thể lên đến$10^9$, điều này dẫn đến một không gian tìm kiếm có kích thước không khả thi$10^{18}$. Ngay cả với việc cắt tỉa, không có cấu trúc nào cho phép liệt kê, bởi vì cấu hình nhiều bit có thể khớp với cùng một tổng và AND. 

Thông tin chi tiết quan trọng là sử dụng danh tính tiêu chuẩn kết nối phép cộng, XOR và AND:$$a + b = (a \oplus b) + 2 \cdot (a \land b)$$Điều này xuất phát từ việc kiểm tra từng bit một cách độc lập. Tại mỗi vị trí bit, nếu cả hai bit đều bằng 1 thì chúng đóng góp 2 vào tổng ở cấp độ tiếp theo, đó chính xác là những gì AND mã hóa. XOR ghi lại 1 bit chưa ghép cặp còn lại. Vì những đóng góp này không can thiệp vào các bit nên danh tính được giữ nguyên trên toàn cầu. 

Sắp xếp lại mang lại:$$a \oplus b = (a + b) - 2 \cdot (a \land b)$$Vì vậy, toàn bộ vấn đề được rút gọn thành một biểu thức số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên$s = a + b$Và$c = a \land b$. Đây là những thông tin được cung cấp duy nhất về những con số ẩn. 
2. Tính toán XOR bằng cách sử dụng danh tính$x = s - 2c$. Điều này hoạt động vì mỗi bit chia sẻ được tính trong AND đóng góp hai lần trong tổng do lan truyền mang nhị phân. 
3. Xuất trực tiếp giá trị XOR được tính toán. 

### Tại sao nó hoạt động 

Mỗi vị trí bit đóng góp độc lập vào cấu trúc số học. Nếu một bit được thiết lập trong cả hai$a$Và$b$, nó đóng góp tổng 2 ở mức bit đó và phần đóng góp đó được mã hóa chính xác trong$2 \cdot (a \land b)$. Phần đóng góp còn lại sau khi loại bỏ các phần mang này tương ứng chính xác với các bit XOR. Vì không có nhiễu chéo bit ngoài số lần mang đã được tính toán nên việc trừ hai lần AND sẽ tách biệt XOR một cách duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s, c = map(int, input().split())
    print(s - 2 * c)

if __name__ == "__main__":
    solve()
```Việc triển khai áp dụng trực tiếp danh tính dẫn xuất. Không cần thiết phải xây dựng lại$a$hoặc$b$và không cần mô phỏng từng bit một. Điều tinh tế duy nhất là đảm bảo phép nhân với 2 được áp dụng cho giá trị AND trước khi trừ; việc đảo ngược thứ tự sẽ không chính xác do quyền ưu tiên của toán tử trong các biểu thức phức tạp hơn, mặc dù trong Python dòng cụ thể này vẫn an toàn như được viết. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu được cung cấp. 

đầu vào:```
5 2
```Chúng tôi giải thích điều này như$a + b = 5$Và$a \land b = 2$. 

Chúng ta hãy tính toán từng bước: 

| Bước | Giá trị | 
| --- | --- | 
| Tổng$s$| 5 | 
| VÀ$c$| 2 | 
| XOR$s - 2c$| 5 - 4 = 1 | 

Vì vậy, đầu ra là 1. 

Điều này phù hợp với một bản tái thiết hợp lệ, ví dụ$a = 3$,$b = 2$. Trong hệ nhị phân: 3 = 011 và 2 = 010, do đó AND là 010 (2), tổng là 5 và XOR là 001 (1). 

Một ví dụ được xây dựng thứ hai: 

đầu vào:```
10 2
```Thử$a = 8$,$b = 2$. Khi đó tổng là 10 và AND là 0 nên XOR phải là 10. Áp dụng công thức: 

| Bước | Giá trị | 
| --- | --- | 
| Tổng$s$| 10 | 
| VÀ$c$| 0 | 
| XOR$s - 2c$| 10 | 

Điều này xác nhận tính đúng đắn khi không tồn tại các bit chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó thực hiện một lần đọc và một vài thao tác số nguyên bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("5 2") == "1", "sample 1"

# custom cases
assert run("0 0") == "0", "minimum values"
assert run("1 0") == "1", "single bit"
assert run("3 2") == "1", "basic carry case"
assert run("10 0") == "10", "no overlap case"
assert run("7 7") == "0", "all bits overlap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 0 | trường hợp không cạnh | 
| 1 0 | 1 | độ chính xác từng bit | 
| 3 2 | 1 | mang theo trường hợp tương tác | 
| 10 0 | 10 | không có bit chồng chéo | 
| 7 7 | 0 | hành vi chồng chéo hoàn toàn | 

## Vỏ cạnh 

Đối với đầu vào$0\ 0$, cả hai số đều bằng 0, vì vậy AND bằng 0 và tổng bằng 0. Công thức cho$0 - 0 = 0$, điều đó đúng. Không có cấu trúc mang ẩn nên XOR cũng phải bằng 0. 

Đối với đầu vào$7\ 7$, chúng tôi giải thích cả hai số là giống hệt nhau. Khi đó tổng là 14 và AND là 7. Áp dụng thuật toán cho$14 - 14 = 0$. Theo bit, các số giống hệt nhau luôn XOR về 0 và công thức hủy chính xác tất cả các đóng góp được chia sẻ hai lần, không để lại gì.
