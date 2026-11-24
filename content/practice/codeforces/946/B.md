---
problem: 946B
contest_id: 946
problem_index: B
name: "Weird Subtraction Process"
contest_name: "Educational Codeforces Round 39 (Rated for Div. 2)"
rating: 1100
tags: ["math", "number theory"]
answer: passed_samples
verified: true
solve_time_s: 63
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a327b46-911c-83ec-af62-8431b2a8c956
---

#CF 946B - Quá trình trừ kỳ lạ 

**Đánh giá:** 1100 
**Tags:** toán, lý thuyết số 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a327b46-911c-83ec-af62-8431b2a8c956 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho hai số nguyên dương biểu thị kích thước của hai cọc. Một quy trình xác định liên tục sửa đổi hai giá trị này bằng cách sử dụng một quy tắc rất cụ thể: nếu một giá trị ít nhất gấp đôi giá trị kia, chúng ta trừ hai lần giá trị nhỏ hơn khỏi giá trị lớn hơn. Quá trình chỉ dừng khi không có giá trị nào bằng ít nhất hai lần giá trị kia hoặc khi một trong hai giá trị đó bằng 0. 

Nhiệm vụ không phải là mô phỏng một số bước cố định mà là xác định cặp ổn định cuối cùng sau khi áp dụng tất cả các mức giảm có thể. 

Các ràng buộc cho phép các giá trị lên tới$10^{18}$, điều này ngay lập tức loại trừ mô phỏng từng bước. Một bước trừ đơn lẻ có thể rẻ, nhưng trong trường hợp xấu nhất, số lần lặp tỷ lệ thuận với độ lớn của các số. Ví dụ, bắt đầu từ$(10^{18}, 1)$, một mô phỏng đơn giản sẽ yêu cầu khoảng$5 \cdot 10^{17}$hoạt động vượt xa mọi giới hạn thời gian khả thi. 

Khó khăn chính là quá trình trừ không đối xứng; nó chỉ kích hoạt khi một giá trị ít nhất gấp đôi giá trị kia. Điều kiện này tạo ra những khoảng thời gian dài trong đó một bên thu hẹp nhanh chóng theo từng phần, nhưng cũng có trường hợp các vai trò thay thế nhau. Một mô phỏng bất cẩn thường thất bại vì nó cho rằng chỉ có một bên chiếm ưu thế hoặc quên rằng sau khi giảm một biến, điều kiện có thể ngay lập tức bị đảo ngược và cho phép thực hiện phép tính ngược lại. 

Trường hợp cạnh điển hình là khi các giá trị xen kẽ giữa các mức giảm kích hoạt: 

đầu vào:```
12 5
```Đầu ra đúng:```
0 1
```Một cách tiếp cận ngây thơ có thể chỉ trừ một lần cho mỗi lần kiểm tra và bỏ lỡ các lần giảm chuỗi lặp đi lặp lại xảy ra ngay sau khi thay đổi trạng thái. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu tuân theo các quy tắc: liên tục kiểm tra xem$a \ge 2b$hoặc$b \ge 2a$và thực hiện phép trừ tương ứng. Điều này đúng vì mỗi thao tác đều phản ánh chính xác báo cáo vấn đề. Tuy nhiên, mỗi bước đều giảm thiểu ít nhất một biến và trong trường hợp xấu nhất chỉ giảm một lượng nhỏ so với kích thước của nó. Nếu một số lớn hơn nhiều so với số kia, hãy nói$a = 10^{18}$Và$b = 1$, thì mỗi bước hợp lệ sẽ giảm$a$bằng 2, dẫn đến khoảng$5 \cdot 10^{17}$lần lặp lại. 

Quan sát quan trọng là phép trừ lặp đi lặp lại có cùng dạng hoạt động giống như thuật toán Euclid với một điểm thay đổi: thay vì trừ một bản sao của giá trị nhỏ hơn, chúng ta trừ nó theo từng đoạn có kích thước nhỏ hơn.$2b$. Điều này có nghĩa là chúng ta có thể thay thế nhiều bước lặp lại bằng một bước chia số nguyên duy nhất. Nếu như$a \ge 2b$, chúng tôi muốn áp dụng thao tác này nhiều lần nhất có thể trong một lần:$$a \leftarrow a \bmod (2b)$$bởi vì mỗi thao tác giảm$a$chính xác$2b$, và chúng ta tiếp tục cho đến khi điều kiện không còn đúng nữa. 

Ý tưởng tương tự được áp dụng một cách đối xứng cho$b$. Điều này biến quá trình này thành một phép rút gọn logarit tương tự như thuật toán Euclide, vì ở mỗi giai đoạn có ít nhất một biến co lại đáng kể. 

Điều phức tạp là sau mỗi lần giảm số lượng lớn, chúng ta phải đánh giá lại cả hai điều kiện vì điều khiển có thể chuyển đổi giữa$a$Và$b$nhiều lần trước khi chấm dứt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(a,b)) | O(1) | Quá chậm | 
| Tối ưu | O(log max(a,b)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi liên tục áp dụng mức giảm hợp lệ lớn nhất có thể cho đến khi không bên nào có thể kích hoạt hoạt động. 

1. Kiểm tra xem cả hai giá trị có dương hay không. Nếu một trong hai trở thành 0, quá trình sẽ kết thúc ngay lập tức vì không có quy tắc nào có thể áp dụng được. 
2. Nếu$a \ge 2b$, thay thế$a$với$a \bmod (2b)$. Điều này nén tất cả các phép trừ lặp lại thành một bước duy nhất vì mỗi thao tác sẽ loại bỏ chính xác$2b$. 
3. Sau khi cập nhật$a$, ngay lập tức kiểm tra lại các điều kiện, vì giảm$a$có thể cho phép$b \ge 2a$để trở thành sự thật. 
4. Nếu$b \ge 2a$, thay thế$b$với$b \bmod (2a)$, với lý do tương tự như trên. 
5. Lặp lại cho đến khi không còn điều kiện nào nữa. 

Lý do thứ tự này hoạt động là vì mỗi phép toán modulo duy trì hiệu ứng của các phép trừ hợp lệ lặp đi lặp lại mà không bỏ qua bất kỳ trạng thái trung gian nào có thể thay đổi bên nào chiếm ưu thế. 

### Tại sao nó hoạt động 

Quá trình luôn duy trì tính bất biến là giá trị lớn hơn giảm đi một cách nghiêm ngặt trong khi giá trị nhỏ hơn không thay đổi trong suốt giai đoạn đó. Mỗi lần giảm số lượng lớn mô phỏng một chuỗi tối đa các bước di chuyển hợp pháp giống hệt nhau. Vì mỗi bước modulo giảm ít nhất một số xuống ít hơn hai lần số kia nên cuối cùng hệ thống đạt đến trạng thái mà không điều kiện nào có thể giữ được, khớp với điều kiện kết thúc của quy trình ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())
    
    while True:
        if a == 0 or b == 0:
            break
        
        if a >= 2 * b:
            a %= 2 * b
            continue
        
        if b >= 2 * a:
            b %= 2 * a
            continue
        
        break
    
    print(a, b)

if __name__ == "__main__":
    solve()
```Giải pháp mã hóa trực tiếp quy trình dưới dạng vòng lặp với hai quy tắc giảm số lượng lớn đối xứng. Lựa chọn triển khai chính là sử dụng modulo thay vì phép trừ lặp lại, điều này đảm bảo mỗi giai đoạn chạy trong thời gian không đổi. các`continue`các câu lệnh đảm bảo rằng sau mỗi lần giảm, thuật toán ngay lập tức đánh giá lại cả hai hướng, vì mối quan hệ thống trị có thể thay đổi sau một lần cập nhật. 

Phải cẩn thận khi đặt hàng: kiểm tra`a`điều kiện đầu tiên và sau đó là`b`điều kiện phù hợp với cấu trúc quy trình và tránh bỏ qua quá trình chuyển đổi mới được kích hoạt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
12 5
```| Bước | một | b | Hành động | 
| --- | --- | --- | --- | 
| 1 | 12 | 5 | a >= 2b → a = 12 % 10 = 2 | 
| 2 | 2 | 5 | b >= 2a → b = 5 % 4 = 1 | 
| 3 | 2 | 1 | a >= 2b → a = 2 % 2 = 0 | 
| 4 | 0 | 1 | dừng lại | 

Đầu ra:```
0 1
```Dấu vết này cho thấy quá trình luân phiên giữa việc giảm$a$Và$b$và cách các phép toán modulo nắm bắt nhiều phép trừ theo chuỗi trong một bước duy nhất. 

### Ví dụ 2 

đầu vào:```
31 12
```| Bước | một | b | Hành động | 
| --- | --- | --- | --- | 
| 1 | 31 | 12 | a >= 2b → a = 31 % 24 = 7 | 
| 2 | 7 | 12 | b >= 2a → b = 12 % 14 = 12 | 
| 3 | 7 | 12 | không có điều kiện giữ → dừng | 

Đầu ra:```
7 12
```Điều này chứng tỏ rằng quá trình có thể kết thúc mà không có một giá trị nào trở thành 0, khi không bên nào chiếm ưu thế với hệ số hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log max(a, b)) | Mỗi bước modulo giảm đáng kể một giá trị, tương tự như thuật toán Euclid | 
| Không gian | O(1) | Chỉ có hai số nguyên được lưu trữ | 

Hành vi logarit đảm bảo giải pháp dễ dàng phù hợp trong giới hạn ngay cả đối với các giá trị lên tới$10^{18}$, vì số lần giảm tỷ lệ thuận với số lần độ lớn có thể giảm đi một nửa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("12 5\n") == "0 1"

# minimum case
assert run("1 1\n") == "1 1"

# one side dominant
assert run("1000000000000000000 1\n") in ["0 1", "1 0"]

# equal powers of two pattern
assert run("8 2\n") == "0 2"

# alternating reduction
assert run("31 12\n") == "7 12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 1 | chấm dứt ngay lập tức | 
| 10^18 1 | (0,1) hoặc (1,0) | sự thống trị cực độ | 
| 8 2 | 0 2 | giảm một bên lặp đi lặp lại | 
| 31 12 | 7 12 | giảm xen kẽ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi các giá trị đã ở cấu hình ổn định trong đó không có giá trị nào ít nhất gấp đôi giá trị kia. Ví dụ: đầu vào:```
7 12
```Thuật toán kiểm tra đầu tiên$a \ge 2b$, điều đó là sai. Sau đó nó kiểm tra$b \ge 2a$, điều này cũng sai. Vòng lặp kết thúc ngay lập tức và xuất ra$7 12$, khớp với hành vi đúng vì không thể áp dụng quy tắc nào. 

Một trường hợp cạnh khác xảy ra khi việc giảm lặp đi lặp lại làm giảm giá trị chính xác về 0. Ví dụ:```
2 1
```Đây,$a \ge 2b$giữ, vì vậy$a = 2 \bmod 2 = 0$. Quá trình sau đó dừng ngay lập tức vì một giá trị bằng 0, tạo ra kết quả:```
0 1
```Điều này xác nhận rằng việc rút gọn dựa trên modulo xử lý chính xác khả năng phân chia chính xác mà không yêu cầu cách viết đặc biệt.
