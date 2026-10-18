---
title: "CF 105575B - Vòng Codeforce điển hình"
description: "Mỗi trường hợp bài toán bao gồm ba mảng có độ dài $n$, mà chúng ta có thể coi là tham số cho mỗi bài toán. Với mỗi chỉ số $i$, có các giá trị cố định $ai$, $bi$ và $ci$."
date: "2026-06-22T12:52:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105575
codeforces_index: "B"
codeforces_contest_name: "Southeast University 6th Programming Competition Competition (Winter, Novice Group) \u4e1c\u5357\u5927\u5b66\u7b2c\u516d\u5c4a\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u51ac\u5b63\u8d5b\uff08\u65b0\u624b\u7ec4\uff09"
rating: 0
weight: 105575
solve_time_s: 47
verified: true
draft: false
---

[CF 105575B - Vòng Codeforces điển hình](https://codeforces.com/problemset/problem/105575/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp vấn đề bao gồm ba mảng có độ dài$n$, mà chúng ta có thể coi là tham số cho mỗi bài toán. Đối với mỗi chỉ số$i$, có những giá trị cố định$a_i$,$b_i$, Và$c_i$. Sau khi đọc các mảng này, đầu vào cung cấp$n$cặp bổ sung$(t, s)$, một cho mỗi chỉ mục$i$và mỗi cặp xác định chỉ số đóng góp là bao nhiêu$i$thêm vào câu trả lời cuối cùng. 

Đối với mỗi chỉ số$i$, chúng tôi tính điểm ứng viên dựa trên$t$Và$s$. Nếu như$s \le 0$, chỉ số đó không đóng góp gì cả. Ngược lại, chúng ta đánh giá một biểu thức tuyến tính giảm theo cả hai$t$Và$s$, sau đó so sánh nó với giới hạn cố định$c_i$. Sự đóng góp của chỉ số$i$là mức tối đa giữa giá trị giới hạn$c_i$và biểu thức tính toán. Tổng hợp những đóng góp này trên tất cả các chỉ số sẽ tạo ra câu trả lời cuối cùng. 

Các ràng buộc ngụ ý trong mô tả bài toán và giải pháp tham chiếu chỉ ra rằng tất cả số học phù hợp thoải mái trong các số nguyên có dấu 32 bit. Điều này loại bỏ mọi nhu cầu xử lý số nguyên lớn hoặc số học mô-đun. Thuật toán chạy theo thời gian tuyến tính$n$, vậy các giá trị của$n$ít nhất là đến$10^5$hoặc$10^6$được dự định sẽ được xử lý trong một lần duy nhất. 

Một trường hợp tế nhị phát sinh khi$s \le 0$. Trong trường hợp đó, chỉ mục bị bỏ qua hoàn toàn. Việc triển khai đơn giản vẫn có thể tính toán công thức và đưa nó vào, dẫn đến những đóng góp tiêu cực không chính xác. 

Ví dụ, giả sử$a_i = 100$,$b_i = 2$,$c_i = 10$và đầu vào cho$t = 5$,$s = 0$. Đóng góp đầu ra đúng là$0$. Việc thực hiện bất cẩn mà bỏ qua$s \le 0$người bảo vệ sẽ tính toán$100 - 2 \cdot 5 - 50 \cdot (0 - 1) = 100 - 10 + 50 = 140$, sau đó lấy sai$\max(10, 140) = 140$, điều này không hợp lệ vì sự cố đã loại bỏ các mục nhập đó một cách rõ ràng. 

Một trường hợp khác là khi biểu thức được tính toán trở nên âm hoặc cực kỳ nhỏ. Trong trường hợp đó, nắp$c_i$thống trị. Nếu việc triển khai quên thao tác tối đa và luôn sử dụng biểu thức được tính toán thì nó sẽ tính thiếu một cách có hệ thống trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Cách đơn giản để tiếp cận vấn đề là xử lý từng chỉ số một cách độc lập, tính toán trực tiếp biểu thức và tích lũy kết quả. Đối với mỗi$i$, ta đọc thông số của nó$a_i$,$b_i$,$c_i$và các giá trị truy vấn của nó$t_i$,$s_i$, sau đó đánh giá công thức chính xác như được mô tả. Vì mỗi chỉ mục là độc lập nên không cần xử lý trước hoặc cấu trúc dữ liệu. 

Phương pháp bạo lực này đã phù hợp với cấu trúc của vấn đề. Mỗi chỉ số đóng góp chính xác một lần và mỗi đóng góp được tính theo thời gian không đổi. Do đó tổng công việc tỉ lệ thuận với$n$. Không có sự tối ưu hóa nào có ý nghĩa ngoài việc thực hiện cẩn thận. 

Quan sát quan trọng là không có gì liên kết các chỉ số khác nhau. Tất cả các mảng đều được truy cập ở cùng một vị trí và mỗi phép tính đều độc lập. Điều này giúp loại bỏ mọi nhu cầu sắp xếp, cấu trúc tiền tố hoặc lập trình động. Nhiệm vụ thực sự duy nhất là đánh giá một biểu thức số học cố định cho mỗi chỉ mục và áp dụng mức tối đa có điều kiện. 

Do cấu trúc này, bất kỳ nỗ lực nào để giới thiệu máy móc phức tạp hơn sẽ là chi phí không cần thiết. Giải pháp tối ưu chỉ đơn giản là đánh giá bạo lực với việc xử lý chính xác các điều kiện biên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(n)$| Đã chấp nhận | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$, xác định có bao nhiêu đóng góp độc lập mà chúng ta phải tính toán. Mọi giá trị tiếp theo đều được căn chỉnh theo chỉ mục, vì vậy chỉ mục$i$trong tất cả các mảng đề cập đến cùng một mục logic. 
2. Đọc mảng$a$,$b$, Và$c$. Chúng đại diện cho các tham số cố định được sử dụng trong công thức tính điểm cho mỗi chỉ số. Việc lưu trữ chúng cho phép truy cập được lập chỉ mục trực tiếp trong giai đoạn tính toán cuối cùng. 
3. Đối với mỗi chỉ số$i$, đọc một cặp$(t, s)$. Đây là những đầu vào động điều chỉnh cách tính điểm cho vị trí cụ thể đó. 
4. Nếu$s \le 0$, bỏ qua chỉ mục này và không thêm gì vào câu trả lời. Điều kiện này phản ánh một ràng buộc ở cấp độ vấn đề vô hiệu hóa sự đóng góp khi tham số thứ hai không dương. 
5. Mặt khác tính giá trị ứng cử viên$$x = a_i - b_i \cdot t - 50 \cdot (s - 1).$$Biểu thức này thể hiện sự giảm tuyến tính về điểm số khi$t$Và$s$tăng. 
6. So sánh giá trị này với ngưỡng cố định$c_i$và lấy cái lớn hơn. Điều này đảm bảo mỗi chỉ số đóng góp ít nhất giới hạn cơ sở khi công thức tính toán giảm xuống quá thấp. 
7. Cộng kết quả vào tổng số đang chạy. 
8. Sau khi xử lý tất cả các chỉ số, xuất ra tổng tích lũy. 

Tính chính xác phụ thuộc vào thực tế là mỗi chỉ mục được xử lý độc lập và chính xác một lần, do đó không tồn tại tương tác hoặc hiệu ứng thứ tự. 

### Tại sao nó hoạt động 

Mỗi chỉ mục đóng góp một giá trị được xác định hoàn toàn bởi các tham số và cặp đầu vào riêng của nó. Tính toán là một hàm thuần túy của$(a_i, b_i, c_i, t_i, s_i)$và đáp án cuối cùng là tổng của các kết quả độc lập này. Nhánh có điều kiện duy nhất là$s \le 0$trường hợp, trong đó xác định rõ ràng sự đóng góp bằng không. Vì không có bước nào sửa đổi trạng thái được chia sẻ trên các chỉ mục ngoại trừ thông qua phép cộng, nên thuật toán duy trì tính chính xác bằng cách xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [0] * (n + 1)
    b = [0] * (n + 1)
    c = [0] * (n + 1)

    for i in range(1, n + 1):
        a[i] = int(input())
    for i in range(1, n + 1):
        b[i] = int(input())
    for i in range(1, n + 1):
        c[i] = int(input())

    ans = 0
    for i in range(1, n + 1):
        t, s = map(int, input().split())
        if s <= 0:
            continue
        val = a[i] - b[i] * t - 50 * (s - 1)
        if val < c[i]:
            val = c[i]
        ans += val

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc ba mảng tham số trước, đảm bảo rằng mỗi chỉ mục được khởi tạo đầy đủ trước khi xử lý truy vấn. Vòng lặp kết thúc$t, s$cặp được đồng bộ hóa với chỉ mục$i$, do đó mỗi phép tính đều sử dụng các tham số chính xác. 

Chi tiết triển khai chính là sự tiếp tục sớm khi$s \le 0$, điều này ngăn chặn số học không chính xác trên các mục nhập không hợp lệ. Một chi tiết quan trọng khác là sử dụng số học số nguyên trực tiếp mà không cần chuyển đổi dấu phẩy động, vì tất cả các giá trị vẫn nằm trong giới hạn số nguyên an toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ được xây dựng với$n = 3$. 

Mảng đầu vào:$a = [5, 10, 8]$,$b = [1, 2, 3]$,$c = [4, 3, 6]$Cặp truy vấn:$(t, s)$:$(2, 1), (3, 0), (1, 2)$Đối với mỗi chỉ số: 

| tôi | một [tôi] | b[i] | c[i] | t | s | giá trị tính toán | giá trị cuối cùng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 4 | 2 | 1 | 5 - 2 - 0 = 3 | tối đa(4, 3) = 4 | 
| 2 | 10 | 2 | 3 | 3 | 0 | bỏ qua | 0 | 
| 3 | 8 | 3 | 6 | 1 | 2 | 8 - 3 - 50 = -45 | tối đa(6, -45) = 6 | 

Câu trả lời cuối cùng là$4 + 0 + 6 = 10$. 

Dấu vết này cho thấy cách$s \le 0$điều kiện loại bỏ hoàn toàn chỉ số 2 và cách giới hạn$c_i$chiếm ưu thế khi biểu thức tuyến tính trở nên âm đối với chỉ số 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục được xử lý chính xác một lần với các phép toán số học có thời gian không đổi | 
| Không gian |$O(n)$| Lưu trữ cho mảng$a$,$b$, Và$c$| 

Thời gian chạy là tuyến tính ở kích thước đầu vào, đây là mức tối ưu vì mọi chỉ mục phải được đọc và đóng góp độc lập vào tổng cuối cùng. Việc sử dụng bộ nhớ cũng tuyến tính do lưu trữ ba mảng đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# sample-style small case
assert run("2\n1\n2\n3\n1 1\n2 0\n") == "3", "skipped second index"

# all s <= 0
assert run("1\n10\n10\n10\n5 0\n") == "0", "no contributions"

# cap dominates
assert run("1\n5\n10\n100\n1 2\n") == "100", "c_i dominates negative expression"

# linear positive case
assert run("1\n100\n1\n0\n1 1\n") == "99", "direct formula"

# mixed case
assert run("3\n5\n10\n8\n1\n2\n3\n4\n3\n6\n2 1\n3 0\n1 2\n") == "10", "combined behavior"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn s=0 | 0 | bỏ qua logic | 
| hộp đựng chỉ có nắp | c_i | hành vi max() | 
| trường hợp hỗn hợp | tổng tính toán | tương tác của các quy tắc | 

## Vỏ cạnh 

Khi nào$s \le 0$, thuật toán bỏ qua tính toán một cách rõ ràng. Đối với đầu vào$t = 10, s = 0$với bất kỳ$a_i, b_i, c_i$, vòng lặp thực thi`continue`, do đó không có phép tính nào được thực hiện và phần đóng góp vẫn bằng không. Điều này ngăn chặn việc vô tình sử dụng thuật ngữ dịch chuyển âm$50 \cdot (s - 1)$, nếu không sẽ tăng giá trị không chính xác. 

Khi biểu thức tính toán trở nên nhỏ hơn$c_i$, thuật toán thay thế nó bằng$c_i$. Ví dụ, với$a_i = 20$,$b_i = 5$,$c_i = 15$,$t = 3$,$s = 2$, biểu thức ước tính là$20 - 15 - 50 = -45$. Hoạt động tối đa buộc sự đóng góp vào$15$, đảm bảo kết quả không giảm xuống dưới mức giới hạn cơ bản. 

Những trường hợp này xác nhận rằng hai quy tắc kiểm soát, bỏ qua các mục nhập không hợp lệ và thực thi giới hạn tối thiểu, xác định đầy đủ hành vi đúng cho tất cả các đầu vào có thể có.
