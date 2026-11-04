---
title: "CF 1175A - Từ anh hùng đến số không"
description: "Chúng ta được cho một số $n$ và một tham số $k$. Bắt đầu từ $n$, chúng ta muốn đạt đến số 0 bằng cách sử dụng hai phép toán được phép: trừ một hoặc nếu giá trị hiện tại chia hết cho $k$, hãy thay thế nó bằng thương số sau khi chia cho $k$."
date: "2026-06-15T17:25:40+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1175
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 66 (Rated for Div. 2)"
rating: 900
weight: 1175
solve_time_s: 222
verified: true
draft: false
---

[CF 1175A - Từ Anh hùng đến Không](https://codeforces.com/problemset/problem/1175/A) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 3 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$n$và một tham số$k$. Bắt đầu từ$n$, chúng ta muốn đạt tới số 0 bằng cách sử dụng hai phép toán được phép: trừ một hoặc nếu giá trị hiện tại chia hết cho$k$, thay thế bằng thương sau khi chia cho$k$. Mỗi thao tác tốn một bước và mục tiêu là giảm thiểu tổng số bước. 

Đây thực chất là một quá trình liên tục thu nhỏ một số dần dần hoặc bằng những bước nhảy lớn khi khả năng chia hết cho phép. Khó khăn là việc chia đôi khi chỉ thực hiện được nên chúng ta không thể thoải mái “chia một cách tham lam” mà không tính đến các bước cần thiết để làm cho số đó chia hết. 

Những hạn chế là vô cùng lớn, với$n$Và$k$lên đến$10^{18}$. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào giảm dần từng cái một. Ngay cả việc quét tuyến tính về mặt$n$là không thể, vì$10^{18}$hoạt động vượt xa thời hạn. Bất kỳ giải pháp hợp lệ nào cũng phải giảm số theo các pha logarit hoặc gần logarit, thường bằng cách bỏ qua các phép trừ liên tiếp kéo dài. 

Một cách tiếp cận ngây thơ sẽ mô phỏng quá trình: nếu$n$không chia hết cho$k$, trừ đi một lần cho đến khi nó chia hết rồi chia. Điều này đúng nhưng có thể xuống cấp trầm trọng khi$n$lớn và$k$là nhỏ. Ví dụ, nếu$n = 10^{18}$Và$k = 2$, một mô phỏng đơn giản sẽ liên tục trừ một để đạt đến số chẵn, sau đó chia, lặp lại mô hình này nhiều lần. Số bước tỉ lệ thuận với$n$, điều đó là không thể thực hiện được. 

Một trường hợp cạnh tinh tế khác phát sinh khi$k = 1$. Phép chia cho một không bao giờ thay đổi giá trị, vì vậy phép toán có ý nghĩa duy nhất là trừ một. Trong trường hợp đó, câu trả lời chỉ đơn giản là$n$. Bất kỳ triển khai nào không đề phòng trường hợp này có thể lặp lại mãi mãi hoặc liên tục áp dụng phép chia vô ích. 

## Phương pháp tiếp cận 

Chiến lược bạo lực là luôn áp dụng từng bước di chuyển tốt nhất hiện có. Tại mỗi trạng thái, nếu chia hết cho$k$, chia; nếu không thì trừ đi một. Điều này là hợp lý cục bộ vì phép chia thu nhỏ số lượng một cách nhanh chóng và phép trừ chuẩn bị cho các phép chia trong tương lai. 

Điều này hoạt động chính xác nhưng không hiệu quả vì giai đoạn trừ chiếm ưu thế trong thời gian chạy. Trong trường hợp xấu nhất, trước mỗi lần phân chia, chúng ta có thể cần tới$k-1$phép trừ và điều này có thể lặp lại nhiều lần trên toàn bộ phạm vi cho đến 0. Từ$n$tùy thuộc vào$10^{18}$, điều này dẫn đến số lượng hoạt động không giới hạn. 

Quan sát quan trọng là chúng ta không bao giờ cần phải mô phỏng từng phép trừ một. Thay vào đó, chúng ta có thể nhảy trực tiếp đến bội số gần nhất của$k$. Nếu số hiện tại là$x$, chúng tôi tính toán$x \bmod k$. Giá trị đó cho chúng ta biết chính xác cần bao nhiêu phép trừ để đạt được trạng thái chia hết. Chúng tôi áp dụng chúng trong một bước tổng hợp. Sau đó, nếu số lượng ít nhất vẫn còn$k$, chúng ta thực hiện phép chia và tiếp tục. 

Điều này biến quá trình thành một chuỗi các bước nhảy lớn theo sau là một số lượng nhỏ các phần chia, giảm số lần lặp lại xuống còn khoảng$O(\log_k n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi liên tục giảm số lượng bằng cách kết hợp các phép trừ theo đợt và các phép chia không thường xuyên. 

1. Nếu$k = 1$, động thái hữu ích duy nhất là trừ đi một, vì vậy câu trả lời là$n$. Chúng tôi quay lại ngay lập tức. Điều này tránh các vòng lặp vô hạn do phép chia không thay đổi gì. 
2. Trong khi$n \ge k$, đầu tiên chúng ta tính toán$r = n \bmod k$. Điều này cho chúng ta biết chúng ta còn cách bội số nhỏ gần nhất của bao xa$k$. 
3. Chúng tôi trừ$r$trong một bước, giảm$n$đến bội số của$k$, và thêm$r$để trả lời. Điều này thay thế nhiều phép toán đơn vị bằng một bước nhảy số học duy nhất. 
4. Sau đó chúng ta chia$n$qua$k$, tăng câu trả lời lên một. Đây là bước chia hợp lệ duy nhất và mang lại sự giảm lớn về độ lớn. 
5. Chúng tôi lặp lại quá trình này cho đến khi$n < k$, nghĩa là không thể chia được nữa. 
6. Cuối cùng, chúng tôi trừ đi phần còn lại$n$để đạt đến số 0, thêm$n$để trả lời. 

Lý do điều này có tác dụng là vì ở mọi giai đoạn, chúng tôi đang chuyển đổi chiến lược cục bộ tối ưu thành một phân tách chi phí chính xác: bất kỳ chuỗi hợp lệ nào cuối cùng cũng phải loại bỏ cùng một phần dư trước khi việc phân chia có thể xảy ra, do đó, việc thanh toán hàng loạt không làm thay đổi tính tối ưu. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi lần lặp, chúng ta đã tính toán chính xác số phép trừ cần thiết để đưa trạng thái hiện tại về điểm chia hợp lệ gần nhất. Vì phép chia luôn được thực hiện ngay khi có thể và mỗi phép trừ được tính chính xác một lần nên không có thứ tự phép tính thay thế nào có thể làm giảm tổng chi phí. Bất kỳ đường dẫn tối ưu nào cũng phải “xóa” phần còn lại trước khi chia, do đó, việc nhóm các thao tác đó sẽ đảm bảo tính chính xác đồng thời loại bỏ sự thiếu hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n, k = map(int, input().split())
        
        if k == 1:
            out.append(str(n))
            continue
        
        steps = 0
        
        while n >= k:
            r = n % k
            steps += r
            n -= r
            n //= k
            steps += 1
        
        steps += n
        out.append(str(steps))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện ý tưởng tạo khối. Hoạt động mô đun tính toán số lượng giảm cần thiết trước khi chia. Sau khi áp dụng chúng, chúng tôi chia một lần và tiếp tục. Vòng lặp kết thúc khi số đó nhỏ hơn$k$, tại thời điểm đó chỉ còn lại phép trừ. 

Trường hợp đặc biệt$k = 1$được xử lý rõ ràng để tránh vòng lặp không cần thiết và để phản ánh chính xác rằng chỉ có thể giảm dần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 59, k = 3
```| n | n % k | phép trừ | phép chia | tổng số bước | 
| --- | --- | --- | --- | --- | 
| 59 | 2 | 2 | 1 | 3 | 
| 19 | 1 | 1 | 1 | 5 | 
| 6 | 0 | 0 | 1 | 6 | 
| 2 | - | 2 (cuối cùng) | 0 | 8 | 

Quá trình này cho thấy sự nén lặp đi lặp lại: mỗi phép chia được bắt đầu bằng các phép trừ đủ chính xác để căn chỉnh số. Giai đoạn cuối cùng là phép trừ thuần túy khi giá trị giảm xuống dưới$k$. 

### Ví dụ 2 

đầu vào:```
n = 10^18, k = 10
```| n | n % k | phép trừ | phép chia | tổng số bước | 
| --- | --- | --- | --- | --- | 
| 10^18 | 0 | 0 | 1 | 1 | 
| 10^17 | 0 | 0 | 1 | 2 | 
| ... | ... | ... | ... | 19 | 

Trường hợp này cho thấy sự phân chia rõ ràng lặp đi lặp lại vì số này đã được căn chỉnh với lũy thừa của 10. Quá trình này trở thành một chuỗi logarit thuần túy. 

Dấu vết xác nhận rằng thuật toán hoạt động tối ưu khi khả năng chia hết xảy ra thường xuyên và vẫn hiệu quả khi không xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log_k n)$| Mỗi phép chia làm giảm độ lớn theo hệ số$k$và mỗi lần lặp lại thực hiện công việc liên tục | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được duy trì | 

Thời gian chạy dễ dàng đủ nhanh để$t \le 100$và giá trị lên đến$10^{18}$, vì ngay cả trong trường hợp xấu nhất số lần lặp trên mỗi ca kiểm thử vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    t = int(input())
    res = []
    
    for _ in range(t):
        n, k = map(int, input().split())
        if k == 1:
            res.append(str(n))
            continue
        
        steps = 0
        while n >= k:
            r = n % k
            steps += r
            n -= r
            n //= k
            steps += 1
        steps += n
        res.append(str(steps))
    
    return "\n".join(res)

# provided samples
assert run("2\n59 3\n1000000000000000000 10\n") == "8\n19"

# custom cases
assert run("1\n1 2\n") == "1", "minimum case"
assert run("1\n10 10\n") == "2", "single division then subtraction"
assert run("1\n100 2\n") == "9", "repeated halving structure"
assert run("1\n5 1\n") == "5", "k=1 edge case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2/1 | 1 | trường hợp khác 0 nhỏ nhất | 
| 10 10 | 2 | chia hết chính xác rồi kết thúc | 
| 100 2 | 9 | hoạt động hỗn hợp lặp đi lặp lại | 
| 5 1 | 5 | hành vi phân chia thoái hóa | 

## Vỏ cạnh 

Khi nào$k = 1$, phép chia luôn có thể thực hiện được nhưng vô ích. Thuật toán trả về một cách rõ ràng$n$, phù hợp với thực tế là chỉ có phép trừ mới làm giảm số đó. Đối với đầu vào$n = 5, k = 1$, chúng tôi trực tiếp xuất ra 5. 

Khi nào$n < k$, không có phép chia nào xảy ra cả. Ví dụ,$n = 7, k = 10$, vòng lặp bị bỏ qua và câu trả lời chỉ đơn giản là 7, vì chúng ta chỉ trừ xuống 0. 

Khi$n$chính xác là chia hết cho$k$, chẳng hạn như$n = 100, k = 10$, số dư bằng 0 nên ta chia ngay mà không tốn thêm chi phí. Thuật toán thực hiện chính xác một bước trên mỗi phép chia cho đến khi kết thúc.
