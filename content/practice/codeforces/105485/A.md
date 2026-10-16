---
title: "CF 105485A - \u6570\u7ec4"
description: "Nhiệm vụ mô tả một mảng hoàn toàn như một cách để thúc đẩy việc lập chỉ mục từ con số 0. Nếu một mảng có độ dài $n$, các chỉ số hợp lệ của nó sẽ chạy từ $0$ đến $n - 1$. Sau đó, bài toán yêu cầu chúng ta xuất chỉ mục của phần tử cuối cùng của một mảng như vậy khi chỉ đưa ra độ dài $n$."
date: "2026-06-23T18:22:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105485
codeforces_index: "A"
codeforces_contest_name: "2024 China Unversity of Geosciences (Wuhan) Freshman Contest"
rating: 0
weight: 105485
solve_time_s: 54
verified: true
draft: false
---

[CF 105485A - \u6570\u7ec4](https://codeforces.com/problemset/problem/105485/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một mảng hoàn toàn như một cách để thúc đẩy việc lập chỉ mục từ con số 0. Nếu một mảng có độ dài$n$, các chỉ số hợp lệ của nó chạy từ$0$lên tới$n - 1$. Sau đó, bài toán yêu cầu chúng ta xuất chỉ mục của phần tử cuối cùng của mảng đó khi chỉ có độ dài$n$được đưa ra. 

Vì vậy, đầu vào là một số nguyên biểu thị kích thước của một mảng khái niệm và đầu ra là chỉ mục hợp lệ lớn nhất trong mảng đó. Nói cách khác, chúng tôi đang chuyển đổi độ dài thành vị trí cuối cùng của nó theo chỉ mục dựa trên số 0. 

Ràng buộc$1 \le n \le 100$ngụ ý rằng đầu vào cực kỳ nhỏ, do đó mọi tính toán từ thời gian không đổi đến thời gian tuyến tính sẽ dễ dàng vượt qua. Tuy nhiên, cấu trúc của bài toán không yêu cầu lặp lại các phần tử hoặc bất kỳ mô phỏng nào. Đầu ra chỉ phụ thuộc vào mối quan hệ số học trực tiếp giữa kích thước mảng và chỉ mục cuối cùng của nó. 

Không có trường hợp cạnh nào có ý nghĩa ngoài đầu vào nhỏ nhất có thể. Khi$n = 1$, mảng có một phần tử duy nhất tại chỉ mục$0$, vì vậy đầu ra đúng là$0$. Thay vào đó, bất kỳ cách tiếp cận nào giả định không chính xác các chỉ số đều bắt đầu từ một sẽ xuất ra$1$, vốn đã nằm ngoài phạm vi hợp lệ. 

## Phương pháp tiếp cận 

Một cách giải thích ngây thơ sẽ là xây dựng rõ ràng ý tưởng về một mảng và sau đó tính chỉ số cuối cùng của nó bằng cách lặp lại các vị trí. Người ta có thể tưởng tượng việc tạo ra một chuỗi từ$0$ĐẾN$n - 1$, sau đó trả về phần tử cuối cùng gặp phải. Điều này hoạt động chính xác vì nó phản ánh định nghĩa về lập chỉ mục, nhưng nó thực hiện công việc không cần thiết: việc tạo tất cả các chỉ mục yêu cầu$O(n)$hoạt động mặc dù câu trả lời cuối cùng đã được xác định chỉ bằng độ dài. Trong trường hợp xấu nhất, điều này có nghĩa là có tới 100 lần lặp, điều này vẫn không đáng kể ở đây, nhưng cấu trúc sẽ trở nên lãng phí đối với các ràng buộc lớn hơn. 

Quan sát quan trọng là việc lập chỉ mục dựa trên số 0 xác định mối quan hệ cố định: chỉ mục hợp lệ cuối cùng luôn nhỏ hơn kích thước của mảng một đơn vị. Điều này giúp loại bỏ mọi nhu cầu xây dựng hoặc duyệt qua mảng. Vấn đề được giải quyết hoàn toàn bằng cách tính một phép trừ duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng chỉ số) | O(n) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu (công thức trực tiếp) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$từ đầu vào, biểu thị độ dài của mảng. Giá trị được đảm bảo ít nhất là 1, do đó mảng luôn có ít nhất một chỉ mục hợp lệ. 
2. Tính toán$n - 1$, tương ứng với chỉ số dựa trên số 0 hợp lệ cuối cùng. 
3. Xuất trực tiếp giá trị tính toán. 

### Tại sao nó hoạt động 

Trong một mảng không có chỉ mục, phần tử đầu tiên có chỉ số 0 và mỗi phần tử bổ sung sẽ tăng chỉ số lên một. Sau khi đặt$n$phần tử, vị trí cuối cùng đạt được chính xác là$n - 1$. Mối quan hệ này đúng với mọi giá trị hợp lệ$n$trong miền đầu vào, do đó tính toán$n - 1$luôn mang lại chỉ số cuối cùng chính xác mà không có ngoại lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    print(n - 1)

if __name__ == "__main__":
    solve()
```Giải pháp đọc một số nguyên duy nhất và áp dụng ngay mối quan hệ dẫn xuất giữa kích thước mảng và chỉ mục cuối cùng. Phép trừ được thực hiện một lần và kết quả được in ra. Không cần vòng lặp, điều kiện hoặc cấu trúc bộ nhớ bổ sung. Việc sử dụng`strip()`đảm bảo phân tích cú pháp an toàn ngay cả khi đầu vào chứa khoảng trắng ở cuối, điều này thường gặp trong môi trường lập trình cạnh tranh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```| Bước | n | Tính toán | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 5 | - | - | 
| Kết quả tính toán | 5 | 5 - 1 | 4 | 
| In | 5 | - | 4 | 

Điều này cho thấy một trường hợp tiêu chuẩn trong đó mảng có nhiều phần tử. Chỉ mục hợp lệ cuối cùng nhỏ hơn số phần tử một đơn vị, cho kết quả là 4. 

### Ví dụ 2 

đầu vào:```
1
```| Bước | n | Tính toán | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 1 | - | - | 
| Kết quả tính toán | 1 | 1 - 1 | 0 | 
| In | 1 | - | 0 | 

Điều này thể hiện trường hợp ranh giới trong đó mảng chứa chính xác một phần tử. Chỉ số hợp lệ duy nhất là 0, xác nhận tính đúng đắn của công thức ở đầu vào nhỏ nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một phép tính số học duy nhất được thực hiện sau khi đọc đầu vào | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc cho phép lên tới 100, nhưng giải pháp này hoạt động liên tục bất kể kích thước đầu vào, do đó, nó phù hợp thoải mái trong mọi giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    n = int(sys.stdin.readline().strip())
    print(n - 1)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided sample style cases
assert run("1\n") == "0", "sample-like 1"
assert run("5\n") == "4", "sample-like 5"

# custom cases
assert run("2\n") == "1", "small increment case"
assert run("100\n") == "99", "upper bound case"
assert run("10\n") == "9", "typical case"
assert run("3\n") == "2", "off-by-one check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | mảng kích thước tối thiểu | 
| 100 | 99 | ranh giới ràng buộc tối đa | 
| 2 | 1 | chuyển đổi không tầm thường nhỏ nhất | 
| 3 | 2 | sự đúng đắn từng cái một | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa về mặt cấu trúc duy nhất là khi độ dài mảng tối thiểu. Đối với đầu vào$n = 1$, quá trình tính toán được tiến hành trực tiếp: đọc 1, trừ 1 và xuất ra 0. Điều này phù hợp với thực tế là mảng một phần tử chỉ có chỉ số 0. Bất kỳ sai lệch nào so với quy tắc trừ sẽ phá vỡ trường hợp này ngay lập tức, điều này làm cho nó trở thành một neo chính xác hữu ích cho toàn bộ logic.
