---
title: "CF 104804C - \u041c\u043e\u0440\u044f\u043a\u0438"
description: "Chúng tôi đang mô phỏng quy trình kiếm tiền rất đơn giản thông qua một chuỗi vật phẩm được gọi là thủy thủ. Tổng cộng có $n$ thủy thủ và trong mỗi vòng $k$, Igor chọn chính xác một thủy thủ từ những gì còn lại."
date: "2026-06-28T13:24:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104804
codeforces_index: "C"
codeforces_contest_name: "Central Russia Regional Contest, 2022, Qualification Contest"
rating: 0
weight: 104804
solve_time_s: 78
verified: true
draft: false
---

[CF 104804C - \u041c\u043e\u0440\u044f\u043a\u0438](https://codeforces.com/problemset/problem/104804/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng quy trình kiếm tiền rất đơn giản thông qua một chuỗi vật phẩm được gọi là thủy thủ. có$n$tổng số thủy thủ có sẵn và trong mỗi$k$vòng Igor lấy chính xác một thủy thủ từ những gì còn lại. Mỗi thủy thủ có một giá trị cố định được xác định hoàn toàn bởi vị trí của nó trong thứ tự thủy thủ toàn cầu: thủy thủ đầu tiên có giá trị$m$đồng xu, đồng thứ hai có giá trị$m+1$, cái thứ ba có giá trị$m+2$, vân vân. 

Bởi vì không có sự tương tác giữa các thủy thủ và không có sự ngẫu nhiên, điều duy nhất quan trọng là chỉ số nào trong số những chỉ số đầu tiên$n$vị trí được chọn trong lần đầu tiên$k$chọn. Vì Igor luôn đưa một thủy thủ mỗi vòng và không có hạn chế nào ngoài khả năng sẵn có nên quá trình này tương đương với việc lấy người đầu tiên.$\min(n, k)$thủy thủ theo thứ tự. 

Đầu ra là tổng giá trị của tất cả các thủy thủ được lấy trong các vòng này. 

Các ràng buộc cho phép lên đến$10^5$thủy thủ và$10^5$vòng. Một giải pháp lặp lại trên từng thủy thủ riêng lẻ đã có thể chấp nhận được ở đường biên, nhưng bất kỳ giải pháp nào liên quan đến các vòng lặp lồng nhau hoặc tính toán lại lặp đi lặp lại sẽ chỉ ổn nếu nó hoàn toàn tuyến tính về tổng thể. Bất kỳ cách tiếp cận nào cố gắng mô phỏng các lựa chọn từng bước mà không nhận ra cấu trúc số học đều có nguy cơ gây ra chi phí không cần thiết nhưng vẫn an toàn; sự đơn giản hóa thực sự đến từ việc nhận ra rằng các giá trị được chọn tạo thành một cấp số cộng liền kề. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 0$. Trong trường hợp đó, không có thủy thủ nào bị bắt và câu trả lời phải bằng 0. Một trường hợp cạnh khác là khi$k > n$, chỉ ở đâu$n$thủy thủ tồn tại, do đó quá trình này thực sự dừng lại sớm và chỉ$n$điều khoản đóng góp. Một cách triển khai ngây thơ luôn tính tổng$k$các thuật ngữ sẽ cho rằng các thủy thủ không tồn tại tồn tại một cách không chính xác. 

## Phương pháp tiếp cận 

Nếu chúng ta mô phỏng trực tiếp quá trình này, chúng ta sẽ chọn từng thủy thủ một và tích lũy giá trị của họ. Giá trị của$i$-th thủy thủ bị bắt là$m + i - 1$, và chúng tôi dừng lại sau$k$bước hoặc sau khi kiệt sức$n$thủy thủ. Phương pháp trực tiếp này thực hiện một lượng công việc không đổi cho mỗi thủy thủ được thực hiện, do đó nó chạy trong$O(\min(n, k))$, đã đủ nhanh cho các giới hạn. 

Tuy nhiên, cấu trúc không phải là tùy ý, nó là một cấp số cộng liên tiếp bắt đầu từ$m$với sự khác biệt$1$. Thay vì lặp lại, chúng ta có thể tính tổng ở dạng đóng. Nếu như$t = \min(n, k)$, thì trình tự là:$$m, m+1, m+2, \dots, m+t-1$$Đây là một chuỗi số học tiêu chuẩn có tổng có thể được tính là:$$t \cdot m + \frac{t(t-1)}{2}$$Cải tiến quan trọng là thay thế việc lặp lại bằng cách đánh giá trực tiếp công thức này, giảm việc tính toán về thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(phút(n, k)) | O(1) | Đã chấp nhận | 
| Công thức số học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$,$k$, Và$m$. Những giá trị này xác định có bao nhiêu thủy thủ tồn tại, bao nhiêu thủy thủ được bắt và giá trị bắt đầu của thủy thủ đầu tiên. 
2. Xác định số lượng thủy thủ thực sự bị bắt: đặt$t = \min(n, k)$. Điều này là cần thiết vì chúng ta không thể nhận nhiều thủy thủ hơn mức tồn tại. 
3. Tính tổng cấp số cộng bắt đầu từ$m$với chiều dài$t$. Thay vì lặp lại, hãy sử dụng danh tính mà tổng bằng$t \cdot m + \frac{t(t-1)}{2}$. 
4. Xuất ra tổng tính toán. 

### Tại sao nó hoạt động 

Ở mỗi bước$i$, giá trị gia tăng chính xác là$m + i$, với mức tăng nhất quán là 1 giữa các thủy thủ liên tiếp. Điều này đảm bảo rằng các giá trị được lấy tạo thành một cấp số cộng hoàn hảo mà không có khoảng trống hoặc sắp xếp lại. Vì chúng ta lấy chính xác số đầu tiên$t$các thủy thủ theo thứ tự, không có hiệu ứng hoán vị hoặc lựa chọn nào làm thay đổi trình tự. Do đó, công thức tổng của cấp số cộng khớp chính xác với kết quả tích lũy của bất kỳ việc thực thi hợp lệ nào của quy trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, m = map(int, input().split())
    
    t = min(n, k)
    
    # sum of m + (m+1) + ... + (m+t-1)
    # = t*m + (0 + 1 + ... + t-1)
    # = t*m + t*(t-1)//2
    ans = t * m + t * (t - 1) // 2
    
    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc các giá trị đầu vào và ngay lập tức giảm kích thước bài toán xuống$t = \min(n, k)$, đảm bảo chúng tôi chỉ xem xét các lựa chọn hợp lệ. Sau đó nó áp dụng trực tiếp công thức chuỗi số học. Công thức không có phép trừ tránh mọi phép tính dấu phẩy động và hoàn toàn dựa vào số học số nguyên, an toàn trong các số nguyên không giới hạn của Python. 

Một cạm bẫy phổ biến là quên kẹp$k$qua$n$, điều này sẽ cho rằng các thủy thủ vượt ra ngoài$n$hiện hữu. Một cách khác là thực hiện tính tổng bằng phép chia thay vì phép chia số nguyên, điều này có thể gây ra lỗi nổi trong các ngôn ngữ khác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:$n = 12$,$k = 18$,$m = 2$Vì chỉ có 12 thủy thủ tồn tại,$t = \min(12, 18) = 12$. 

| tôi | Giá trị gia tăng | 
| --- | --- | 
| 1 | 2 | 
| 2 | 3 | 
| 3 | 4 | 
| ... | ... | 
| 12 | 13 | 

Tổng =$2 + 3 + \dots + 13 = 90$Điều này khẳng định rằng mặc dù$k$lớn hơn$n$, chỉ những thủy thủ có sẵn mới đóng góp. 

### Mẫu 2 

đầu vào:$n = 5$,$k = 4$,$m = 10$Đây$t = 4$, vì chúng tôi nhận ít thủy thủ hơn số lượng sẵn có. 

| tôi | Giá trị gia tăng | 
| --- | --- | 
| 1 | 10 | 
| 2 | 11 | 
| 3 | 12 | 
| 4 | 13 | 

Tổng =$10 + 11 + 12 + 13 = 46$Điều này cho thấy tiến trình số học đơn giản trong lần đầu tiên$k$các phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ số học thời gian không đổi sau khi đọc đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Việc tính toán làm giảm toàn bộ quá trình thành một đánh giá công thức duy nhất, thỏa mãn một cách tầm thường các ràng buộc ngay cả đối với các đầu vào lớn nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("12 18 2\n") == "90", "sample 1"
assert run("5 4 10\n") == "46", "sample 2"
assert run("75 40 96\n") == "4620", "sample 3"

# custom cases
assert run("1 0 100\n") == "0", "no rounds"
assert run("1 10 1\n") == "1", "single sailor cap by n"
assert run("5 5 1\n") == "15", "small full range"
assert run("100000 100000 100000\n") == str(100000 * 100000 + 100000 * 99999 // 2), "max stress"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 100 | 0 | trường hợp cạnh không tròn | 
| 1 10 1 | 1 | k > n với phần tử đơn | 
| 5 5 1 | 15 | tiến trình chính xác đầy đủ | 
| 100000 100000 100000 | số tiền lớn | hiệu suất và an toàn tràn | 

## Vỏ cạnh 

cho$k = 0$, bộ thuật toán$t = 0$, ngay lập tức tạo ra tổng$0$bởi vì cả hai số hạng trong công thức đều biến mất. Ví dụ, đầu vào$1\ 0\ 10$dẫn đến$t=0$, kể từ đây$0 \cdot 10 + 0 = 0$. 

Khi$k > n$, bước kẹp đảm bảo chúng ta chỉ tính tổng các thủy thủ hiện có. Ví dụ,$n=3, k=10, m=5$sản lượng$t=3$, sản xuất$5+6+7=18$. Bất kỳ vòng lặp ngây thơ nào$k$sẽ cố gắng tiếp cận các thủy thủ không tồn tại và gây ra sự cố hoặc đếm quá mức. 

Khi$n = 1$, tiến trình thu gọn về một giá trị duy nhất bất kể$k$. Vì$n=1, k=100, m=42$, kết quả luôn là$42$, từ$t=1$và công thức giảm xuống còn$1 \cdot 42 + 0$.
