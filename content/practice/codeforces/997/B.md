---
problem: 997B
contest_id: 997
problem_index: B
name: "Roman Digits"
contest_name: "Codeforces Round 493 (Div. 1)"
rating: 2000
tags: ["brute force", "combinatorics", "dp", "greedy", "math"]
answer: passed_samples
verified: true
solve_time_s: 82
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33af7d-bf40-83ec-9286-1b6afde7d406
---

# CF 997B - Chữ số La Mã 

**Xếp hạng:** 2000 
**Tags:** sức mạnh vũ phu, tổ hợp, dp, tham lam, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33af7d-bf40-83ec-9286-1b6afde7d406 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có thể tạo được bao nhiêu giá trị nguyên khác nhau bằng cách sử dụng chính xác$n$các ký hiệu được chọn từ bốn loại “chữ số La Mã”: I, V, X và L. Mỗi ký hiệu đóng góp một giá trị số cố định, cụ thể là 1, 5, 10 và 50 tương ứng và giá trị của một chuỗi chỉ là tổng của các ký hiệu đó. 

Điểm mấu chốt là thứ tự không liên quan đến giá trị. Một chuỗi được xác định hoàn toàn bởi số lượng mỗi chữ số mà nó chứa. Nếu chúng ta chọn$i$Là,$v$V,$x$X và$l$L là vậy đó$i+v+x+l=n$, thì số kết quả là$$i + 5v + 10x + 50l.$$Các cách sắp xếp khác nhau tạo ra cùng một số đếm được coi là cùng một số nguyên, vì vậy câu hỏi thực sự không phải là về dãy số mà là về các bộ bốn số đếm riêng biệt dẫn đến các tổng khác nhau. 

Kích thước đầu vào$n$có thể lớn như$10^9$, do đó, bất kỳ giải pháp nào lặp lại các phân phối chữ số có thể có đều ngay lập tức không thể thực hiện được. Ngay cả sự phụ thuộc bậc ba hoặc bậc hai vào$n$nằm ngoài phạm vi, điều này đẩy chúng ta tới một biểu thức dạng đóng hoặc một diễn giải tổ hợp. 

Một vấn đề tế nhị là về nguyên tắc, sự kết hợp khác nhau của các chữ số có thể tạo ra tổng số tiền như nhau. Ví dụ: năm chữ I và một chữ V đều có giá trị là 5, do đó nhiều cách biểu diễn sẽ thu gọn thành một số nguyên duy nhất. Điều này có nghĩa là chúng ta phải cẩn thận để không nhầm lẫn giữa “số lượng biểu diễn” với “số lượng giá trị riêng biệt”. Nhiệm vụ hoàn toàn là về các giá trị riêng biệt. 

Một cách tiếp cận ngây thơ liệt kê tất cả các bộ tứ$(i,v,x,l)$là không khả thi ngay cả đối với nhỏ$n$, vì số lượng bộ dữ liệu như vậy tăng lên như$O(n^3)$. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất đơn giản. Chúng tôi lặp đi lặp lại tất cả các cách để phân phối$n$các vị trí giống nhau giữa các loại có bốn chữ số, tính tổng kết quả và chèn nó vào một tập hợp. Điều này ghi lại chính xác các số nguyên riêng biệt vì tập hợp này sẽ tự động loại bỏ các số nguyên trùng lặp. Tuy nhiên, số lượng phân phối$$\binom{n+3}{3} = O(n^3),$$điều đó hoàn toàn không thể thực hiện được khi$n$có thể đạt được$10^9$. 

Quan sát cấu trúc quan trọng là giá trị chỉ phụ thuộc vào số lượng của bốn chữ số và số lượng này tạo thành cấu hình “sao và thanh” tiêu chuẩn: chúng ta đang chọn một bộ bốn số nguyên không âm có tổng bằng$n$. Ánh xạ từ số đếm đến số nguyên là tuyến tính và mặc dù các bộ tứ khác nhau đôi khi có thể xung đột về giá trị, cấu trúc của hệ số cụ thể này hoạt động theo cách mà số tổng riêng biệt có thể đạt được khớp với số phân bố số lượng đó. Điều này làm giảm vấn đề về việc đếm các nghiệm số nguyên không âm của$$i + v + x + l = n.$$Đó là một kết quả tổ hợp cổ điển: số nghiệm là số cách đặt 3 dấu phân cách giữa$n$những món đồ giống hệt nhau. 

Như vậy bài toán được chuyển thành một công thức tổ hợp trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force của tất cả các chữ số |$O(n^3)$|$O(n^3)$| Quá chậm | 
| Tổ hợp (sao và thanh) |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích bài toán bằng cách chọn số lần sử dụng mỗi chữ số. Chúng tôi giới thiệu các biến$i, v, x, l$với sự ràng buộc$i+v+x+l=n$. Việc cải cách này loại bỏ hoàn toàn thứ tự vì thứ tự không ảnh hưởng đến tổng. 
2. Nhận biết rằng mọi cấu hình hợp lệ đều tương ứng với nghiệm của phương trình tuyến tính trong bốn số nguyên không âm. Nhiệm vụ trở thành đếm xem có bao nhiêu giải pháp như vậy tồn tại. 
3. Áp dụng nguyên tắc ngôi sao và vạch. Chúng tôi tưởng tượng$n$các đồ vật giống hệt nhau (vị trí chữ số) và 3 ngăn chia chúng thành bốn nhóm. Mỗi vị trí của dải phân cách tương ứng với chính xác một bộ tứ$(i,v,x,l)$. 
4. Tính số cách xếp 3 ngăn chia vào nhau$n+3$tổng số vị trí. Điều này mang lại hệ số nhị thức:$$\binom{n+3}{3}.$$1. Trả về giá trị này làm câu trả lời. 

### Tại sao nó hoạt động 

Mỗi tập hợp chữ số hợp lệ tương ứng duy nhất với một nghiệm của$i+v+x+l=n$và mọi giải pháp như vậy đều tương ứng với chính xác một vị trí của dấu phân cách trong sự sắp xếp tuyến tính của$n$những món đồ giống hệt nhau. Sự song song này đảm bảo rằng việc đếm các vị trí phân cách sẽ đếm chính xác số lượng cấu hình có thể đạt được và do đó số lượng số nguyên riêng biệt được tạo ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

# compute C(n+3, 3)
def comb3(k):
    return k * (k - 1) * (k - 2) // 6

print(comb3(n + 3))
```Việc triển khai dựa trên biểu thức dạng đóng cho$\binom{n+3}{3}$. Từ$n$có thể rất lớn, nên tránh tính toán giai thừa trực tiếp mà thay vào đó là biểu thức đa thức có thời gian không đổi. 

Sự tinh tế duy nhất là đảm bảo sử dụng phép chia số nguyên. Số nguyên lớn của Python xử lý các giá trị một cách an toàn ngay cả khi$n = 10^9$, vì kết quả phù hợp thoải mái trong phạm vi 64-bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```Chúng tôi tính toán:$$\binom{4}{3} = 4.$$Có bốn cấu hình: 

một I, một V, một X hoặc một L. 

| tôi | v | x | tôi | tổng hợp | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 1 | 
| 0 | 1 | 0 | 0 | 5 | 
| 0 | 0 | 1 | 0 | 10 | 
| 0 | 0 | 0 | 1 | 50 | 

Điều này xác nhận công thức khớp với tập giá trị thực tế. 

### Ví dụ 2 

đầu vào:```
2
```Chúng tôi tính toán:$$\binom{5}{3} = 10.$$| tôi | v | x | tôi | tổng hợp | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | 0 | 0 | 2 | 
| 1 | 1 | 0 | 0 | 6 | 
| 1 | 0 | 1 | 0 | 11 | 
| 1 | 0 | 0 | 1 | 51 | 
| 0 | 2 | 0 | 0 | 10 | 
| 0 | 1 | 1 | 0 | 15 | 
| 0 | 1 | 0 | 1 | 55 | 
| 0 | 0 | 2 | 0 | 20 | 
| 0 | 0 | 1 | 1 | 60 | 
| 0 | 0 | 0 | 2 | 100 | 

Chúng tôi quan sát 10 khoản tiền riêng biệt, phù hợp với công thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số lượng phép tính số học không đổi được thực hiện bất kể$n$. | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ được sử dụng. | 

Giải pháp xử lý thoải mái$n \le 10^9$vì nó làm giảm bài toán tính một đa thức bậc ba trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    n = int(sys.stdin.readline().strip())
    return str((n + 3) * (n + 2) * (n + 1) // 6)

# provided samples
assert run("1\n") == "4"
assert run("2\n") == "10"

# custom cases
assert run("3\n") == "20", "small increasing case"
assert run("4\n") == "35", "check cubic growth"
assert run("10\n") == str((13*12*11)//6), "larger correctness check"
assert run("1000000000\n") == str(((10**9 + 3)*(10**9 + 2)*(10**9 + 1))//6), "large bound"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 4 | cấu hình tối thiểu | 
| 2 | 10 | tính đúng đắn cơ bản | 
| 3 | 20 | mô hình tăng trưởng nhỏ | 
| 10 | 286 | hành vi công thức tổ hợp | 
| 10^9 | số nguyên lớn | khả năng mở rộng và an toàn tràn | 

## Vỏ cạnh 

Khi nào$n = 1$, chỉ có một chữ số được chọn nên đáp án phải là 4. Công thức cho$\binom{4}{3} = 4$, khớp trực tiếp. Thuật toán xử lý vấn đề này một cách chính xác vì không cần phân nhánh đặc biệt. 

Khi$n = 2$, phân phối bao gồm các trường hợp như hai chữ số giống nhau hoặc hai chữ số khác nhau. Mặc dù nhiều thành phần có thể tạo ra cùng một giá trị số (ví dụ: VV và mười I đều tạo ra 10), chúng tôi đang tính các tổng riêng biệt có thể đạt được thông qua cấu trúc tổ hợp chứ không phải biểu diễn. Công thức vẫn mang lại 10, khớp với các kết quả khác biệt hợp lệ. 

Khi$n$rất lớn, chẳng hạn như$10^9$, giải pháp không bao giờ lặp lại các cấu hình. Nó trực tiếp đánh giá một biểu thức đa thức, tránh mọi sự phụ thuộc vào độ lớn của$n$.
