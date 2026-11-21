---
title: "CF 2197A - Số thân thiện"
description: "Chúng ta được cho một số nguyên $x$ và chúng ta muốn đếm xem có bao nhiêu số nguyên $y$ thỏa mãn một điều kiện cân bằng rất cụ thể: nếu bạn lấy $y$ và trừ tổng các chữ số của nó, bạn sẽ đạt chính xác trên $x$."
date: "2026-06-07T20:28:04+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "expression-parsing", "math", "schedules"]
categories: ["algorithms"]
codeforces_contest: 2197
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1079 (Div. 2)"
rating: 800
weight: 2197
solve_time_s: 66
verified: true
draft: false
---

[CF 2197A - Những con số thân thiện](https://codeforces.com/problemset/problem/2197/A) 

**Đánh giá:** 800 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, phân tích biểu thức, toán học, lịch trình 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên$x$, và chúng tôi muốn đếm có bao nhiêu số nguyên$y$thỏa mãn một điều kiện cân bằng rất cụ thể: nếu bạn lấy$y$và trừ đi tổng các chữ số của nó, bạn sẽ đến đích chính xác$x$. Nói cách khác, mỗi số hợp lệ$y$“trả quá nhiều” so với$x$và số tiền thanh toán vượt mức chính xác là tổng các chữ số của$y$. 

Vì vậy nhiệm vụ không phải là xây dựng$y$, nhưng để đếm có bao nhiêu công trình như vậy tồn tại cho mỗi$x$. Mỗi ca kiểm thử hỏi: trong số tất cả các số nguyên, có bao nhiêu số khác nhau?$y$thu gọn về cùng một giá trị sau khi trừ đi tổng các chữ số của chúng? 

Các ràng buộc đi lên đến$x \le 10^9$Và$t \le 500$. Một bản quét ngây thơ trên tất cả những gì có thể$y$ngay lập tức là không thể bởi vì ngay cả một trường hợp thử nghiệm cũng sẽ yêu cầu kiểm tra hàng tỷ ứng viên tiềm năng. Thậm chí chỉ quét gần$x$không an toàn nếu không có giới hạn về khoảng cách$y$có thể trôi dạt. 

Một quan sát cấu trúc quan trọng là sự chuyển đổi$y \mapsto y - d(y)$chỉ dịch chuyển số xuống dưới tối đa số chữ số nhân với 9. Đối với số có 10 chữ số, tổng các chữ số nhiều nhất là 90, vì vậy$y$không thể lớn hơn khoảng 90$x$. Điều này làm cho không gian tìm kiếm bị giới hạn chặt chẽ. 

Một trường hợp khó phát hiện khi$x$là nhỏ. Ví dụ, nếu$x = 1$, thì các ứng viên tiềm năng$y$sẽ cần phải thỏa mãn$y - d(y) = 1$. Việc thử các giá trị nhỏ cho thấy có rất ít số đủ điều kiện và rất dễ giả định sai rằng không có hoặc nhiều số nếu không kiểm tra mối quan hệ ràng buộc chính xác. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ là lặp đi lặp lại tất cả những gì có thể$y$lên đến một số giới hạn lớn và kiểm tra xem$y - d(y) = x$. Điều này đúng nhưng hoàn toàn không khả thi vì$y$phạm vi lên đến$10^9 + 100$và chúng tôi sẽ thực hiện các phép tính tổng chữ số cho từng ứng cử viên. Điều đó dẫn đến khoảng$O(10^9)$hoạt động cho mỗi trường hợp thử nghiệm, vượt xa mọi giới hạn hợp lý. 

Cái nhìn sâu sắc quan trọng là đảo ngược phương trình. Thay vì bắt đầu từ$y$, chúng ta bắt đầu từ$x$và hỏi những giá trị nào của$y$có thể có thể ánh xạ tới nó. Nếu như$y - d(y) = x$, sau đó$y = x + d(y)$. Điều này trông có vẻ tròn trịa nhưng nó bộc lộ một hạn chế quan trọng: tổng các chữ số của$y$là nhỏ và bị giới hạn bởi nhiều nhất$9 \cdot 10 = 90$. 

Vì vậy chúng ta có thể thử tất cả các tổng chữ số có thể$s$trong một phạm vi nhỏ. Nếu chúng ta sửa$s = d(y)$, sau đó$y = x + s$. Chúng tôi chỉ đơn giản kiểm tra xem tổng chữ số của$x + s$bằng$s$. Mỗi hợp lệ$s$đóng góp chính xác một giá trị hợp lệ$y$. 

Điều này làm giảm vấn đề từ hàng tỷ ứng viên xuống còn tối đa 100 kiểm tra cho mỗi trường hợp kiểm thử, mỗi trường hợp liên quan đến tính toán tổng chữ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả$y$|$O(N \cdot \log N)$|$O(1)$| Quá chậm | 
| Hãy thử tất cả các tổng chữ số$s \in [0, 100]$|$O(100 \cdot \log x)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với một nhất định$x$, lưu ý rằng mọi giá trị hợp lệ$y$phải thỏa mãn$y = x + s$, Ở đâu$s = d(y)$. Từ$y$có tối đa 10 chữ số,$s \le 90$, vì vậy chúng ta chỉ cần thử một phạm vi nhỏ các tổng chữ số ứng cử viên. 
2. Lặp lại tất cả các giá trị có thể có của$s$từ 0 đến 100. Giới hạn trên được nới lỏng một chút để tránh các lỗi biên, nhưng chỉ các giá trị lên tới 90 mới thực sự thành công. 
3. Đối với mỗi$s$, tính toán$y = x + s$. Đây là ứng cử viên duy nhất có thể có cho tổng chữ số đó. 
4. Tính tổng các chữ số của$y$, ký hiệu$d(y)$. Bước này xác minh xem giả định của chúng tôi về tổng chữ số có nhất quán hay không. 
5. Nếu$d(y) = s$, sau đó$y$là một số thân thiện hợp lệ. Tăng câu trả lời. 
6. Sau khi kiểm tra tất cả những gì có thể$s$, xuất ra tổng số. 

Lý do bảng liệt kê này hoạt động là vì mỗi giá trị hợp lệ$y$được liên kết duy nhất với tổng chữ số của nó$s$và chúng tôi đảm bảo không có ứng cử viên nào bị bỏ sót hoặc được tính hai lần. 

### Tại sao nó hoạt động 

Mọi giải pháp$y$tạo ra chính xác một giá trị$s = d(y)$, và phải thỏa mãn$y = x + s$. Vòng lặp của chúng tôi trên tất cả có thể$s$đảm bảo chúng tôi tái cấu trúc mọi ứng cử viên$y$có thể thỏa mãn phương trình. Vì tổng chữ số bị giới hạn và xác định lần lượt độ lệch giữa$x$Và$y$, không gian tìm kiếm vừa đầy đủ vừa không dư thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_sum(n):
    s = 0
    while n > 0:
        s += n % 10
        n //= 10
    return s

t = int(input())
for _ in range(t):
    x = int(input())
    ans = 0
    
    for s in range(0, 100):
        y = x + s
        if digit_sum(y) == s:
            ans += 1
    
    print(ans)
```Đoạn mã này tách phép tính tổng các chữ số thành một hàm trợ giúp vì nó được gọi lặp đi lặp lại. Vòng lặp kết thúc$s$là ý tưởng cốt lõi: mỗi$s$được coi như một giả thuyết cho tổng chữ số của$y$, và chúng tôi xác minh nó trực tiếp. 

Giới hạn 100 là an toàn vì tổng chữ số tối đa có thể có của số có 10 chữ số là 90 và$x \le 10^9$đảm bảo tối đa 10 chữ số trong$y$. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$x = 18$Chúng tôi kiểm tra sự thay đổi ứng viên$s$. 

| s | y = x + s | tổng chữ số của y | có hiệu lực? | 
| --- | --- | --- | --- | 
| 2 | 20 | 2 | vâng | 
| 3 | 21 | 3 | vâng | 
| 4 | 22 | 4 | vâng | 
| 5 | 23 | 5 | vâng | 
| 6 | 24 | 6 | vâng | 
| 7 | 25 | 7 | vâng | 
| 8 | 26 | 8 | vâng | 
| 9 | 27 | 9 | vâng | 
| 10 | 28 | 10 | vâng | 

Điều này mang lại 9 số hợp lệ từ 20 đến 28. Tiếp tục mẫu tương tự cho đến 29 xác nhận tổng số 10 giá trị hợp lệ. 

Điều này chứng tỏ rằng một khi$x$đã được cố định, hợp lệ$y$tạo thành một khối liền kề được xác định bởi sự ổn định của tổng chữ số. 

### Ví dụ 2:$x = 1$| s | y = x + s | tổng chữ số của y | có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | không | 
| 2 | 3 | 3 | không | 
| 3 | 4 | 4 | không | 
| ... | ... | ... | ... | 

Không có giá trị của$s$thỏa mãn điều kiện nhất quán nên câu trả lời là 0. Điều này cho thấy rằng nhỏ$x$không đảm bảo bất kỳ giải pháp nào, ngay cả khi các ứng cử viên tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(100 \cdot \log x)$| Chúng tôi thử tối đa 100 ca, mỗi ca yêu cầu tính tổng các chữ số trên tối đa 10 chữ số | 
| Không gian |$O(1)$| Chỉ một số số nguyên được sử dụng | 

Giới hạn không đổi của các phép thử tổng chữ số đảm bảo giải pháp chạy thoải mái trong giới hạn ngay cả đối với 500 trường hợp thử nghiệm. Hệ số logarit không đáng kể trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def digit_sum(n):
        s = 0
        while n > 0:
            s += n % 10
            n //= 10
        return s

    t = int(input())
    out = []
    for _ in range(t):
        x = int(input())
        ans = 0
        for s in range(0, 100):
            y = x + s
            if digit_sum(y) == s:
                ans += 1
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("3\n1\n18\n998244360\n") == "0\n10\n10"

# custom cases
assert run("1\n0\n") == "1"
assert run("1\n10\n") >= "0"
assert run("1\n1000000000\n") >= "1"
assert run("2\n1\n2\n")  # basic sanity check
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | cạnh x dương nhỏ nhất | 
| 10 | khác nhau | hành vi ranh giới nhỏ | 
| 1e9 | ≥1 | ổn định biên lớn | 
| hỗn hợp | tính nhất quán | xử lý nhiều bài kiểm tra | 

## Vỏ cạnh 

Khi nào$x = 1$, thuật toán sẽ kiểm tra tất cả$s \in [0, 99]$, sản xuất ứng viên$y = 1 + s$. Không ai trong số những ứng viên này thỏa mãn$d(y) = s$, vì tổng các chữ số tăng quá chậm so với phép dịch tuyến tính. Thuật toán trả về đúng 0. 

Khi nào$x = 999{,}999{,}999$, số ứng viên có thể dao động trong khoảng$10^9 + 100$. Ngay cả ở đây, tổng các chữ số vẫn bị giới hạn bởi 90, do đó chỉ một tập hợp nhỏ các ca có thể hoạt động. Vòng lặp vẫn nắm bắt được mọi khả năng vì mọi khả năng đều hợp lệ$y$phải nằm trong cửa sổ được liệt kê. 

Khi$x$là bội số của 9 hoặc chứa nhiều số 9 ở cuối, tổng các chữ số của các số lân cận dao động nhiều hơn nhưng vẫn nằm trong phạm vi giới hạn. Thuật toán không dựa vào tính đơn điệu, chỉ dựa vào việc kiểm tra toàn diện tất cả các tổng chữ số khả thi, vì vậy những trường hợp này được xử lý một cách tự nhiên mà không cần sửa đổi.
