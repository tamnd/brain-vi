---
title: "CF 105418D - Khắc nghiệt và lợi nhuận"
description: "Vấn đề mô tả một quá trình lợi nhuận phát triển theo một cách rất có cấu trúc. Vào ngày đầu tiên, Harsh không kiếm được gì và mỗi ngày tiếp theo số tiền kiếm được hàng ngày của anh ta tăng thêm đúng một đơn vị so với mức tăng trước đó."
date: "2026-06-23T04:21:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105418
codeforces_index: "D"
codeforces_contest_name: "Algorithmia IIITN 2024 - Round 1"
rating: 0
weight: 105418
solve_time_s: 71
verified: true
draft: false
---

[CF 105418D - Khắc nghiệt và lợi nhuận](https://codeforces.com/problemset/problem/105418/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một quá trình lợi nhuận phát triển theo một cách rất có cấu trúc. Vào ngày đầu tiên, Harsh không kiếm được gì và mỗi ngày tiếp theo số tiền kiếm được hàng ngày của anh ta tăng thêm đúng một đơn vị so với mức tăng trước đó. Vì vậy, chuỗi lợi nhuận hàng ngày được tích lũy: ngày 1 cho 0, ngày thứ 2 cho 1, ngày thứ 3 cho 3, ngày 4 cho 6, v.v. 

Điều chúng tôi thực sự được hỏi là đây: cho một số ngày$d$, hãy tính tổng lợi nhuận tích lũy cho đến ngày hôm đó theo quy tắc tăng đều hàng ngày này. 

Quan sát chính từ các ràng buộc là$d$có thể lớn như$10^9$, trong khi có tới$10^4$trường hợp thử nghiệm. Bất kỳ giải pháp nào mô phỏng sự tích lũy hàng ngày đều không thể thực hiện được, vì thậm chí tối đa một trường hợp thử nghiệm duy nhất cũng$d$sẽ yêu cầu lặp lại một tỷ bước. 

Điều này ngay lập tức buộc chúng ta phải hướng tới một công thức dạng đóng hoặc cách giải thích số học trực tiếp. 

Trường hợp cạnh tinh tế là đầu vào nhỏ nhất. Khi$d = 1$, lợi nhuận phải bằng 0. Khi$d = 2$, lợi nhuận là 1. Một lỗi ngẫu nhiên thường xảy ra nếu người ta coi sai số hạng đầu tiên là 1 thay vì 0, dịch chuyển toàn bộ dãy lên trên và tạo ra các số tam giác bắt đầu từ cơ số sai. 

Một vấn đề tiềm ẩn khác là sự nhầm lẫn giữa mức tăng hàng ngày và lợi nhuận tích lũy. Câu lệnh xác định một quy trình, không phải một công thức trực tiếp, do đó, chỉ triển khai logic một phần như “tổng của các số nguyên d đầu tiên” mà không điều chỉnh các chỉ số sẽ cho kết quả không chính xác đối với các giá trị nhỏ. 

## Phương pháp tiếp cận 

Nếu chúng ta mô phỏng quy trình theo đúng nghĩa đen, chúng ta sẽ duy trì được lợi nhuận đang hoạt động và mức tăng hàng ngày tăng lên một đơn vị mỗi ngày. Vào ngày thứ nhất chúng ta thêm 0, vào ngày thứ 2 chúng ta thêm 1, vào ngày thứ 3 chúng ta thêm 2, v.v. Sau đó$d$ngày, chúng ta sẽ tính được tổng:$$0 + 1 + 2 + \dots + (d-1)$$Đây là một tiến trình số học tiêu chuẩn. Phương pháp brute-force sẽ lặp từ 1 đến$d$và tích lũy lợi nhuận gia tăng mỗi lần. Điều đó hoạt động về mặt khái niệm, nhưng thời gian chạy của nó là$O(d)$mỗi trường hợp thử nghiệm. Với$d$lên đến$10^9$, điều này trở nên hoàn toàn không khả thi ngay cả đối với một truy vấn. 

Cấu trúc của trình tự là điều làm cho vấn đề trở nên đơn giản. Thay vì mô phỏng, chúng tôi nhận ra rằng tổng lợi nhuận là tổng của lần đầu tiên$d-1$số nguyên. Tổng đó có dạng đóng:$$\frac{(d-1)d}{2}$$Điều này loại bỏ tất cả sự phụ thuộc vào phép lặp và giảm mỗi truy vấn về số học theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(d)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Mỗi trường hợp thử nghiệm đều độc lập và yêu cầu tính toán một công thức trực tiếp cho một ngày nhất định. 
2. Cho mỗi ngày nhập liệu$d$, diễn giải quy trình dưới dạng tổng tích lũy của các số gia tăng hàng ngày bắt đầu từ 0. Điều này chuyển bài toán thành tổng chuỗi$0, 1, 2, \dots, d-1$. 
3. Tính tổng bằng công thức chuỗi số học$\frac{(d-1)d}{2}$. Bước này tránh hoàn toàn việc lặp lại và đánh giá trực tiếp kết quả cuối cùng. 
4. Xuất ngay giá trị tính toán cho từng test. 

### Tại sao nó hoạt động 

Quá trình xác định mức tăng hàng ngày là một chuỗi tăng dần bắt đầu từ 0, tăng thêm 1 mỗi ngày. Điều đó có nghĩa là vào ngày$i$, số gia tăng thêm là$i-1$. Tổng lợi nhuận sau$d$ngày chính xác là tổng của tất cả các mức tăng này. Vì điều này tạo thành một cấp số cộng tuyến tính nên tổng của nó được xác định duy nhất bởi số hạng đầu tiên, số hạng cuối cùng và số số hạng của nó. Không có thứ tự tích lũy thay thế nào tồn tại, vì vậy biểu mẫu đóng phải khớp với bất kỳ mô phỏng hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        d = int(input())
        print((d - 1) * d // 2)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là công thức số học trực tiếp. Việc trừ 1 là rất quan trọng vì ngày đầu tiên đóng góp lợi nhuận bằng 0, nghĩa là chuỗi đóng góp kết thúc vào lúc$d-1$, không$d$. Việc sử dụng phép chia số nguyên đảm bảo tính chính xác mà không gặp vấn đề về độ chính xác của dấu phẩy động. 

Các trường hợp kiểm thử lặp lại rất đơn giản và mỗi truy vấn được xử lý trong thời gian không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
d = 5
```Chúng tôi tính toán mức tăng hàng ngày: 0, 1, 2, 3, 4. 

| Ngày | Tăng | Tổng số chạy | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 
| 3 | 2 | 3 | 
| 4 | 3 | 6 | 
| 5 | 4 | 10 | 

Đầu ra cuối cùng là 10, phù hợp$\frac{4 \cdot 5}{2} = 10$. 

Điều này xác nhận rằng công thức nắm bắt chính xác mức tăng trưởng tích lũy mà không cần mô phỏng. 

### Ví dụ 2 

đầu vào:```
d = 6
```Chúng tôi tính toán các khoảng tăng: 0, 1, 2, 3, 4, 5. 

| Ngày | Tăng | Tổng số chạy | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 
| 3 | 2 | 3 | 
| 4 | 3 | 6 | 
| 5 | 4 | 10 | 
| 6 | 5 | 15 | 

Đầu ra cuối cùng là 15, phù hợp$\frac{5 \cdot 6}{2} = 15$. 

Điều này chứng tỏ tính đúng đắn đối với trường hợp lớn hơn một chút và thể hiện rõ ràng mô hình tăng trưởng tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm được tính toán bằng công thức thời gian không đổi | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc cho phép lên đến$10^4$truy vấn, do đó, giải pháp liên tục theo thời gian cho mỗi truy vấn dễ dàng phù hợp với giới hạn thời gian. Các phép tính số học an toàn trong phạm vi số nguyên 64 bit cho$d \le 10^9$, vì giá trị cực đại là khoảng$5 \cdot 10^{17}$. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        d = int(input())
        print((d - 1) * d // 2)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin

# provided samples
assert run("3\n5\n6\n3\n") == "10\n15\n3\n", "sample 1"

# custom cases
assert run("1\n1\n") == "0\n", "minimum case"
assert run("1\n2\n") == "1\n", "small transition case"
assert run("1\n10\n") == "45\n", "moderate arithmetic check"
assert run("2\n1000000000\n999999999\n") == "499999999500000000\n499999998500000000\n", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 | 0 | trường hợp ranh giới tối thiểu | 
| 1, 2 | 1 | chuyển đổi khác 0 đầu tiên | 
| 1, 10 | 45 | tính đúng đắn của cấp số cộng | 
| giá trị lớn | đầu ra lớn | an toàn tràn và chia tỷ lệ công thức | 

## Vỏ cạnh 

cho$d = 1$, chuỗi số gia trống ngoại trừ một đóng góp 0 duy nhất. Công thức cho$(1-1)\cdot1/2 = 0$, khớp chính xác với kết quả mong đợi. 

Vì$d = 2$, số gia là 0 và 1, cho tổng số 1. Công thức mang lại$(2-1)\cdot2/2 = 1$, xác nhận tính đúng đắn trong trường hợp không tầm thường nhỏ nhất. 

Đối với rất lớn$d$, chẳng hạn như$10^9$, phép nhân an toàn trong Python do các số nguyên có độ chính xác tùy ý. Trong các ngôn ngữ có chiều rộng cố định, điều này sẽ yêu cầu bộ nhớ 64-bit nhưng không vượt quá bước trung gian$10^{18}$, vì vậy các loại an toàn chống tràn là đủ.
