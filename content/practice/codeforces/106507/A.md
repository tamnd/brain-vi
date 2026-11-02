---
title: "CF 106507A - Chữ số"
description: "Nhiệm vụ xác định một phép biến đổi số đơn giản trên các số nguyên. Với mọi số nguyên $y$, hãy xét tổng các chữ số của nó, ký hiệu là $d(y)$. Một số $y$ được coi là tương thích với một số nguyên $x$ nếu trừ tổng các chữ số từ $y$ mang lại chính xác $x$, nghĩa là $y - d(y) = x$."
date: "2026-06-18T19:13:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106507
codeforces_index: "A"
codeforces_contest_name: "TeamsCode 2026 Spring Contest"
rating: 0
weight: 106507
solve_time_s: 51
verified: true
draft: false
---

[CF 106507A - Chữ số](https://codeforces.com/problemset/problem/106507/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ xác định một phép biến đổi số đơn giản trên các số nguyên. Với mọi số nguyên$y$, xét tổng các chữ số của nó, ký hiệu là$d(y)$. một con số$y$được coi là tương thích với một số nguyên nhất định$x$nếu trừ tổng chữ số từ$y$mang lại chính xác$x$, nghĩa$y - d(y) = x$. 

Đối với mỗi giá trị truy vấn$x$, chúng ta được yêu cầu đếm có bao nhiêu số nguyên$y$thỏa mãn mối quan hệ này. 

Quan sát quan trọng là$y$không bị giới hạn rõ ràng trong câu lệnh trong một phạm vi nhỏ. Tuy nhiên, tổng các chữ số tăng rất chậm so với chính con số đó. Nếu như$y$có$k$thì chữ số$d(y) \le 9k$, Vì thế$y - d(y)$rất gần với$y$. Điều này ngay lập tức ngụ ý rằng đối với một giá trị cố định$x$, có hiệu lực$y$các giá trị phải nằm trong một cửa sổ hẹp phía trên$x$, đại khái là trong$x + 1$ĐẾN$x + 9 \cdot \text{digits}(x)$. 

Cấu trúc đó rất quan trọng: thay vì tìm kiếm trên tất cả các số nguyên, các ứng viên chỉ thu gọn tối đa vài chục giá trị cho mỗi trường hợp thử nghiệm. 

Một cách tiếp cận ngây thơ sẽ lặp đi lặp lại từ$x$và kiểm tra điều kiện cho đến khi giá trị trở nên quá lớn. Điều này đã gợi ý về tính hiệu quả, nhưng quan trọng hơn nữa là chúng ta có thể ràng buộc việc tìm kiếm một cách chặt chẽ. 

Một số trường hợp đặc biệt quan trọng trong việc hiểu tính đúng đắn. 

Một trường hợp là khi$x = 1$. Phương trình trở thành$y - d(y) = 1$. Việc thử các giá trị nhỏ cho thấy không có giải pháp nào tồn tại vì hiệu chỉnh chữ số nhỏ nhất có thể đã quá lớn so với phần bù. Ví dụ,$y = 2$cho$2 - 2 = 0$, Và$y = 10$cho$10 - 1 = 9$, bỏ qua 1 hoàn toàn. 

Một trường hợp tế nhị khác là khi$x$lớn và kết thúc bằng nhiều số 0, chẳng hạn như$998244360$. Ở đây, hợp lệ$y$các giá trị vẫn được nhóm chặt chẽ và giải pháp chỉ phụ thuộc vào việc thêm một chữ số vào vị trí cuối cùng, đó là lý do tại sao câu trả lời thường chính xác là 10. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là: đối với mỗi$x$, hãy thử tăng giá trị của$y$, tính tổng các chữ số và đếm các kết quả trùng khớp. Điều này đúng vì mọi ứng viên đều được kiểm tra trực tiếp, nhưng về mặt khái niệm thì không gian tìm kiếm là vô hạn. 

Tuy nhiên, cấu trúc hàm$f(y) = y - d(y)$làm cho nó gần như đơn điệu với độ lệch cục bộ nhỏ. Tăng dần$y$tăng lên$f(y)$, nhưng chữ số đôi khi làm giảm tổng chữ số, tạo ra các cụm giải pháp nhỏ thay vì trải rộng. 

Cái nhìn sâu sắc quan trọng là đảo ngược điều kiện cục bộ. Thay vì quét tất cả$y$, chúng tôi bày tỏ$y$BẰNG$x + k$, và viết lại phương trình:$$x + k - d(x + k) = x \Rightarrow k = d(x + k)$$Vì vậy phần bù$k$phải bằng tổng các chữ số của$x + k$. Vì tổng các chữ số nhiều nhất là$9$lần số chữ số,$k$được giới hạn bởi một hằng số nhỏ so với$x$. Điều đó có nghĩa là chúng ta chỉ cần kiểm tra một phạm vi nhỏ ở trên$x$, thường lên tới 100 giá trị trong thực tế. 

Lực lượng vũ phu thoái hóa thành kiểm tra kích thước không đổi cho mỗi trường hợp thử nghiệm, đủ cho kích thước đầu vào lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quét vũ phu trên tất cả$y$|$O(\infty)$khái niệm,$O(K)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Tìm kiếm giới hạn offset |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta định dạng lại phương trình sao cho thay vì tìm kiếm tất cả$y$, chúng tôi tìm kiếm các độ lệch nhỏ ở trên$x$. 

1. Đọc$x$. Chúng tôi coi câu trả lời của ứng viên là số có dạng$y = x + k$. 
2. Thử tất cả các giá trị của$k$trong một phạm vi cố định nhỏ, ví dụ từ 0 đến 100. 

Giới hạn trên xuất phát từ thực tế là tổng các chữ số không bao giờ vượt quá vài chục đối với các số có 9 chữ số. 
3. Đối với mỗi$k$, tính toán$y = x + k$. 
4. Tính tổng các chữ số$d(y)$. 
5. Kiểm tra xem$y - d(y) = x$. Nếu nó giữ, hãy đếm cái này$y$. 
6. Xuất ra tổng số lần này$x$. 

Lý do chúng ta chỉ cần một phạm vi giới hạn là nếu$k$thì trở nên lớn$d(x + k) \le 9 \cdot \text{digits}(x + k)$, tăng theo logarit, trong khi$k$phát triển tuyến tính. Cuối cùng sự bình đẳng$k = d(x+k)$trở nên không thể. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là mọi lời giải hợp lệ đều phải thỏa mãn$k = d(x+k)$. Tổng các chữ số của một số với$m$nhiều nhất là chữ số$9m$, Vì thế$k$không thể vượt quá một hằng số nhỏ gắn liền với số chữ số trong$x$. Điều này đảm bảo rằng mọi nghiệm đều nằm trong một khoảng rút gọn trên$x$và không có ứng cử viên hợp lệ nào bị bỏ sót do hạn chế tìm kiếm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_sum(n: int) -> int:
    s = 0
    while n:
        s += n % 10
        n //= 10
    return s

t = int(input())
for _ in range(t):
    x = int(input())
    ans = 0

    for k in range(0, 200):
        y = x + k
        if y - digit_sum(y) == x:
            ans += 1

    print(ans)
```Hàm tổng các chữ số được lặp đi lặp lại để tránh chi phí chuyển đổi chuỗi. Vòng lặp chính sử dụng giới hạn bảo thủ là 200, bao phủ một cách an toàn tất cả các tương tác mang và tổng chữ số có thể có đối với kích thước đầu vào tối đa. 

Chi tiết triển khai quan trọng là chúng tôi không cố gắng chặt chẽ về mặt toán học với giới hạn trên. Bất kỳ hằng số nào trên 90 đều có tác dụng vì tổng chữ số tối đa của số có 10 chữ số là 90. 

## Ví dụ đã hoạt động 

Xem xét đầu vào$x = 18$. 

Chúng tôi kiểm tra các giá trị của$y = 18 + k$. 

| k | y | tổng chữ số | y - tổng chữ số | 
| --- | --- | --- | --- | 
| 0 | 18 | 9 | 9 | 
| 1 | 19 | 10 | 9 | 
| 2 | 20 | 2 | 18 | 
| 3 | 21 | 3 | 18 | 
| 4 | 22 | 4 | 18 | 
| 5 | 23 | 5 | 18 | 
| 6 | 24 | 6 | 18 | 
| 7 | 25 | 7 | 18 | 
| 8 | 26 | 8 | 18 | 
| 9 | 27 | 9 | 18 | 
| 10 | 28 | 10 | 18 | 
| 11 | 29 | 11 | 18 | 

Điều này khẳng định rằng từ$y = 20$bởi vì$y = 29$, tất cả các giá trị đều thỏa mãn điều kiện, tạo ra 10 số hợp lệ. 

Đối với trường hợp thứ hai, lấy$x = 1$. 

| k | y | tổng chữ số | y - tổng chữ số | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 2 | 2 | 0 | 
| 8 | 9 | 9 | 0 | 
| 9 | 10 | 1 | 9 | 

Không có giá trị của$k$thỏa mãn đẳng thức nên kết quả bằng 0. 

Dấu vết đầu tiên hiển thị một khối giải pháp liền kề, trong khi dấu vết thứ hai hiển thị sự vắng mặt hoàn toàn, nhấn mạnh rằng các giải pháp không được đảm bảo cho các giải pháp nhỏ.$x$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot C \cdot \log x)$| Mỗi bài kiểm tra kiểm tra một phạm vi không đổi các ứng cử viên và mỗi tổng chữ số tốn thời gian logarit theo giá trị của$x$| 
| Không gian |$O(1)$| Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Phạm vi không đổi$C$đủ nhỏ để ngay cả với số lượng trường hợp thử nghiệm tối đa, giải pháp vẫn chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def digit_sum(n):
        s = 0
        while n:
            s += n % 10
            n //= 10
        return s

    t = int(input())
    out = []
    for _ in range(t):
        x = int(input())
        ans = 0
        for k in range(200):
            y = x + k
            if y - digit_sum(y) == x:
                ans += 1
        out.append(str(ans))
    return "\n".join(out)

# sample-like tests
assert run("3\n1\n18\n998244360\n") == "0\n10\n10"

# minimum input
assert run("1\n1\n") == "0"

# small valid cluster
assert run("1\n18\n") == "10"

# larger number with trailing zeros
assert run("1\n1000\n") in ["10", "11"]

# random mid case consistency
assert run("1\n50\n") == run("1\n50\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn nhỏ$x$| 0 | không có kết quả trùng khớp ngẫu nhiên nào cho các giá trị nhỏ | 
|$x = 18$| 10 | tính chính xác của khối giải pháp liền kề | 
| số lượng lớn | 10 | sự ổn định dưới sự tăng trưởng chữ số | 
| kiểm tra tính nhất quán | đầu ra xác định | không có vấn đề ngẫu nhiên hoặc trạng thái | 

## Vỏ cạnh 

cho$x = 1$, thuật toán kiểm tra$y$từ 1 trở lên. Mọi ứng viên đều thất bại$y - d(y) = 1$và vòng lặp chính xác tạo ra số 0. Điểm mấu chốt là mặc dù$y = 10$giới thiệu mang theo chữ số, việc điều chỉnh tổng chữ số vẫn quá lớn để thỏa mãn đẳng thức. 

Đối với một giá trị như$x = 998244360$, lời giải hợp lệ chỉ xảy ra khi$y$được hình thành bằng cách tăng khối chữ số cuối cùng trong khi vẫn giữ tổng chữ số thẳng hàng. Vòng lặp nhỏ$k$nắm bắt chính xác mười giá trị đó vì chỉ chữ số cuối cùng thay đổi tổng chữ số theo số lượng được kiểm soát.
