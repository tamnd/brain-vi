---
title: "CF 106394E - Tô màu lưới"
description: "Chúng ta có một lưới vuông có kích thước $n nhân n$ và mỗi ô phải được tô bằng một trong hai màu đỏ hoặc xanh. Một màu được coi là hợp lệ nếu mỗi ô có chính xác hai ô lân cận (chung một cạnh) có cùng màu với chính nó."
date: "2026-06-25T10:10:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106394
codeforces_index: "E"
codeforces_contest_name: "RUCP x WiCS Mini-Contest"
rating: 0
weight: 106394
solve_time_s: 45
verified: true
draft: false
---

[CF 106394E - Tô màu lưới](https://codeforces.com/problemset/problem/106394/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới vuông có kích thước$n \times n$và mỗi ô phải được tô bằng một trong hai màu đỏ hoặc xanh. Một màu được coi là hợp lệ nếu mỗi ô có chính xác hai ô lân cận (chung một cạnh) có cùng màu với chính nó. 

Vì vậy, mỗi ô sẽ nhìn vào các ô lân cận lên, xuống, trái và phải (khi chúng tồn tại) và trong số các ô liền kề đó, có chính xác hai ô phải khớp với màu của nó. Các ô ở biên có ít hàng xóm hơn nên tình trạng của chúng chỉ được diễn giải bằng cách sử dụng các hàng xóm hiện có. 

Nhiệm vụ là xác định liệu một màu như vậy có tồn tại đối với mỗi$n$và nếu có, hãy xuất ra một cấu hình hợp lệ. 

Các ràng buộc cho phép lên đến$t \le 100$trường hợp thử nghiệm và$n \le 1000$, với tổng của$n^2$qua các bài kiểm tra nhiều nhất$10^6$. Điều này có nghĩa là bất kỳ giải pháp nào$O(n^2)$mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng mọi thứ khối hoặc liên quan đến mô phỏng từng ô lặp đi lặp lại trong nhiều lần lặp sẽ quá chậm. 

Một nhiệm vụ vũ phu trên tất cả$2^{n^2}$lưới rõ ràng là không thể. Ngay cả việc cố gắng gán màu một cách tham lam và quay lại cục bộ cũng sẽ bùng nổ vì tính hợp lệ của mọi ô phụ thuộc vào cấu trúc toàn cầu chứ không chỉ tính nhất quán cục bộ. 

Vỏ ngoài tinh tế xuất hiện ngay lập tức ở kích thước nhỏ. Vì$n = 1$, ô đơn không có ô lân cận, vì vậy nó không thể có chính xác hai ô lân cận cùng màu, khiến câu trả lời là không thể. Vì$n = 2$, mỗi ô có chính xác hai ô lân cận, do đó, điều kiện giảm xuống còn "cả hai ô lân cận phải khớp với màu của ô", điều này buộc toàn bộ lưới phải đơn sắc và điều đó thực sự hoạt động. Vì$n = 3$, các nỗ lực để thỏa mãn điều kiện không thành công vì các ràng buộc góc và tâm xung đột và không có mẫu tuần hoàn nào có thể thỏa mãn tất cả các độ cùng một lúc. 

Một cách tiếp cận ngây thơ cố gắng gán màu cho từng hàng trong khi thỏa mãn các ràng buộc cục bộ thường sẽ thất bại vì việc sửa một hàng sẽ dẫn đến mâu thuẫn ở các hàng liền kề. 

## Phương pháp tiếp cận 

Quan điểm bạo lực sẽ là gán màu cho từng ô và kiểm tra xem điều kiện có giữ ở cuối hay không. Điều đó có hiệu quả về mặt khái niệm vì việc xác minh rất đơn giản: đối với mỗi ô, hãy đếm các ô lân cận phù hợp và kiểm tra xem nó có bằng hai hay không. Chi phí là$O(n^2)$cho mỗi cấu hình, nhưng số lượng cấu hình là theo cấp số nhân, vì vậy điều này ngay lập tức không khả thi. 

Cố gắng cải thiện điều này một cách tham lam, người ta có thể cố gắng gán màu trong khi vẫn duy trì tính hợp lệ một phần, đảm bảo rằng mỗi ô có số lượng hàng xóm phù hợp “mục tiêu”. Điểm thất bại là các quyết định lan truyền theo cả hai hướng: việc thiết lập một ô ảnh hưởng đến các ô lân cận của nó, nhưng các lân cận đó cũng hạn chế ô ban đầu về sau, do đó các quyết định tham lam cục bộ không ổn định. 

Cái nhìn sâu sắc về cấu trúc quan trọng là ngừng suy nghĩ về các ràng buộc cục bộ và thay vào đó hãy tìm kiếm một mô hình lặp lại toàn cầu trong đó mọi ô đều có môi trường cục bộ giống hệt nhau. Nếu mọi ô bên trong nhìn thấy cùng một mẫu của các ô lân cận, thì điều kiện sẽ tự động trở nên đồng nhất và có thể xảy ra ở mọi nơi hoặc không ở đâu cả. 

Yêu cầu “chính xác là hai hàng xóm cùng màu” gợi ý một mẫu mức độ cố định và cách đơn giản nhất để đáp ứng các ràng buộc thống nhất đó trong một lưới là sử dụng một ô xếp tuần hoàn. MỘT$2 \times 2$khối lặp lại là ứng cử viên đương nhiên vì nó đảm bảo cấu trúc lân cận giống hệt nhau cho tất cả các ô bên trong. 

Nếu chúng ta thử một khối như:```
BB
BB
```mỗi ô bên trong có tất cả các ô lân cận (tối đa bốn) bằng chính nó, điều này làm cho mỗi ô bên trong có 4 ô lân cận phù hợp chứ không phải 2, do đó không thành công. 

Lần thử tiếp theo là một mẫu bàn cờ, nhưng điều đó không mang lại kết quả nào cho các hàng xóm phù hợp, điều này cũng không thành công. 

Cấu trúc đúng xuất phát từ việc trộn cấu trúc trên cả hai lớp chẵn lẻ theo cách đảm bảo mỗi ô nhìn thấy chính xác hai ô lân cận giống hệt nhau theo chiều ngang hoặc chiều dọc. Một trong những mô hình như vậy là thực thi sự bình đẳng dọc theo các đường chéo có độ dốc 1, tạo ra sự kề cận theo cặp theo cả hai hướng. Một cách rõ ràng để thể hiện điều này là:$$color(i, j) = (i + j) \bmod 2 \text{ with a controlled flip on alternating rows}$$nhưng cụ thể hơn, cách xây dựng có hiệu quả cho vấn đề này là xây dựng sự lặp lại$2 \times 2$khối có dạng:```
R R
B B
```Điều này đảm bảo rằng trong mỗi hàng, các hàng lân cận khớp với nhau theo cặp và vùng kề dọc được cấu trúc sao cho mỗi ô nhìn thấy chính xác hai hàng xóm cùng màu: một ngang và một dọc, tùy thuộc vào vị trí. 

Tuy nhiên, mô hình này chỉ hoạt động ổn định khi$n$là chẵn. Khi$n$là số lẻ, sự không khớp chẵn lẻ buộc một hàng không ghép đôi ở ranh giới, phá vỡ điều kiện mức độ đồng nhất. Đó là lý do tại sao kích thước lẻ không thành công. 

Vì vậy nghiệm đầy đủ trở thành một đối số chẵn lẻ: thậm chí$n$thừa nhận một công trình ốp lát, kỳ quặc$n$không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân công vũ lực + xác nhận | hàm mũ |$O(n^2)$| Quá chậm | 
| Tham lam nhiệm vụ địa phương |$O(n^2)$với rủi ro quay lại |$O(n^2)$| Trường hợp thất bại | 
| Xây dựng 2×2 định kỳ + kiểm tra tính chẵn lẻ |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trước tiên hãy kiểm tra xem$n$là chẵn. Nếu nó là số lẻ, hãy kết luận ngay rằng không tồn tại công trình xây dựng hợp lệ nào, bởi vì bất kỳ sự xếp lớp thống nhất nào đều phá vỡ hàng ranh giới và không thể duy trì số lượng hàng xóm được yêu cầu trên toàn cầu. 
2. Nếu$n$chẵn, hãy xây dựng lưới theo từng hàng bằng cách lặp lại$2 \times 2$mẫu. Điều này đảm bảo rằng mọi vùng lân cận cục bộ đều lặp lại giống hệt nhau trên lưới, vì vậy chúng tôi chỉ cần xác minh một ô đại diện. 
3. Với mỗi cặp hàng$i$Và$i+1$, điền vào các cột theo khối hai. Trong mỗi khối, gán các màu giống nhau theo chiều ngang và xen kẽ các cặp theo chiều dọc để mỗi cột cũng tạo thành cặp dọc nhất quán. 
4. Xuất lưới kết quả. 

### Tại sao nó hoạt động 

Việc xây dựng thực thi rằng mọi tế bào đều thuộc về một bộ đồng phục$2 \times 2$ô trong đó tất cả các mối quan hệ kề cận đều giống hệt nhau cho đến khi dịch. Trong cách xếp như vậy, mỗi ô có chính xác hai ô lân cận có cùng màu vì mỗi lớp màu tạo thành các chuỗi rời rạc có độ dài bằng hai theo cả hướng hàng và cột. Vì mọi ô đều nằm trong một môi trường cục bộ giống hệt nhau nên việc xác minh một ô sẽ ngầm xác minh tất cả các ô khác, làm cho điều kiện nhất quán trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())

        if n % 2 == 1:
            print("NO")
            continue

        print("YES")
        for i in range(n):
            row = []
            for j in range(n):
                # 2x2 repeating block:
                if (i // 2 + j // 2) % 2 == 0:
                    row.append('R')
                else:
                    row.append('B')
            print("".join(row))

if __name__ == "__main__":
    solve()
```Mã này trực tiếp thực hiện một$2 \times 2$ốp lát định kỳ. biểu hiện`i // 2`Và`j // 2`nhóm lưới thành các khối có kích thước hai và tính chẵn lẻ của tổng của chúng sẽ xen kẽ các màu trên các khối. Điều này đảm bảo tính nhất quán theo cả chiều ngang và chiều dọc, do đó mọi ô đều có cấu trúc vùng lân cận giống nhau. 

Sự từ chối sớm cho lẻ$n$xử lý trường hợp bất khả thi mà không cần cố gắng xây dựng. 

Một lỗi triển khai phổ biến là sử dụng`(i + j) % 2`, tạo ra một bàn cờ và không đáp ứng được yêu cầu về số lượng hàng xóm hoặc quên nhóm phép chia số nguyên, điều này sẽ phá hủy$2 \times 2$cấu trúc hoàn toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
```Chúng tôi xây dựng lưới: 

| tôi | j | tôi//2 | j//2 | chẵn lẻ | màu sắc | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | R | 
| 0 | 1 | 0 | 0 | 0 | R | 
| 1 | 0 | 0 | 0 | 0 | R | 
| 1 | 1 | 0 | 0 | 0 | R | 

Đầu ra:```
RR
RR
```Mỗi ô có chính xác hai ô lân cận (vì mỗi ô có 2 hoặc 3 ô lân cận tùy theo vị trí) và tất cả chúng đều khớp nhau, do đó điều kiện được giữ nguyên. 

Điều này xác nhận rằng ngay cả trường hợp chẵn hợp lệ nhỏ nhất cũng có thể hoạt động được trong quá trình xây dựng. 

### Ví dụ 2 

đầu vào:```
n = 4
```Chúng ta có cấu trúc khối lặp lại: 

| Chặn vị trí | Màu sắc | 
| --- | --- | 
| (0,0) | R | 
| (0,1) | R | 
| (1,0) | R | 
| (1,1) | R | 
| (0,2) | B | 
| (0,3) | B | 
| (1,2) | B | 
| (1,3) | B | 

Tiếp tục mẫu này sẽ lấp đầy toàn bộ lưới. 

Điều này cho thấy mỗi$2 \times 2$khu vực này được thống nhất bên trong và vùng lân cận giữa các khối xen kẽ nhau một cách rõ ràng mà không vi phạm quy tắc “hai hàng xóm giống nhau”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$mỗi bài kiểm tra | Mỗi ô được gán chính xác một lần | 
| Không gian |$O(n^2)$| Lưu trữ lưới cho đầu ra | 

Các ràng buộc cho phép lên đến$10^6$tổng số ô, do đó, việc xây dựng một lần cho mỗi ô dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples (format adapted since original omitted full samples)
# assert run("...") == "..."

# minimum odd
assert "NO" in run("1\n1\n")

# smallest even
out = run("1\n2\n")
assert "YES" in out and "RR" in out

# larger even
out = run("1\n4\n")
assert "YES" in out

# multiple tests
out = run("3\n2\n3\n4\n")
assert out.count("YES") == 2 and out.count("NO") == 1

# all even same size
out = run("2\n2\n2\n")
assert out.count("YES") == 2
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 1 | KHÔNG | trường hợp bất khả thi nhỏ nhất | 
| n = 2 | CÓ + lưới | trường hợp xây dựng cơ sở | 
| n = 4 | CÓ + lưới | độ chính xác thậm chí lớn hơn | 
| hỗn hợp | hỗn hợp | xử lý nhiều vụ việc | 

## Vỏ cạnh 

cho$n = 1$, thuật toán ngay lập tức từ chối nó vì không có ô nào có thể đáp ứng yêu cầu liên quan đến hai ô lân cận cùng màu khi không tồn tại. đầu vào`1\n1\n`kích hoạt kiểm tra sớm và đầu ra`NO`. 

Vì$n = 2$, công trình tạo ra một khối hoàn toàn đồng nhất. Vùng lân cận của mỗi ô đều phù hợp với quy tắc vì tất cả các ô lân cận đều giống hệt nhau, do đó mỗi ô đều thỏa mãn điều kiện một cách tầm thường. 

Thậm chí lớn hơn$n$, sự lặp lại$2 \times 2$ốp lát đảm bảo rằng các hiệu ứng ranh giới không phá vỡ cấu trúc, vì mọi ô ranh giới vẫn nằm bên trong một khối hoàn chỉnh có cùng kiểu kề cận như các ô bên trong.
