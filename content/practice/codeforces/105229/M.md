---
title: "CF 105229M - \u4e0d\u5171\u6234\u5929"
description: "Chúng ta được cho một dòng hoa súng $n$ được dán nhãn từ trái sang phải. Chúng ta cần thiết kế hai hệ thống kết nối được định hướng độc lập trên các vị trí này. Hệ thống đầu tiên thuộc về một con ếch."
date: "2026-06-24T16:12:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105229
codeforces_index: "M"
codeforces_contest_name: "The 2024 Shanghai Collegiate Programming Contest"
rating: 0
weight: 105229
solve_time_s: 63
verified: true
draft: false
---

[CF 105229M - \u4e0d\u5171\u6234\u5929](https://codeforces.com/problemset/problem/105229/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng$n$hoa súng được dán nhãn từ trái sang phải. Chúng ta cần thiết kế hai hệ thống kết nối được định hướng độc lập trên các vị trí này. 

Hệ thống đầu tiên thuộc về một con ếch. Nó bao gồm các bước nhảy có hướng dẫn từ một số vị trí$a_i$ĐẾN$b_i$, luôn luôn với$a_i < b_i$và mọi vị trí được sử dụng nhiều nhất một lần khi bắt đầu và nhiều nhất một lần khi kết thúc. Do hạn chế này, các bước di chuyển của ếch tạo thành một tập hợp các cạnh có hướng rời rạc trên đường thẳng. 

Hệ thống thứ hai thuộc về một con chim. Nó được định nghĩa theo cách tương tự bằng cách sử dụng các cặp$x_i \to y_i$, cũng như tất cả các điểm bắt đầu đều khác biệt và tất cả các điểm kết thúc đều khác biệt. 

Khi hai hệ thống này được cố định, chuyển động được cho phép bằng cách đi theo các cạnh có hướng liên tục, do đó cả ếch và chim đều có thể di chuyển dọc theo các đường đi trong đồ thị có hướng tương ứng của chúng. 

Một cặp vị trí$(i, j)$trở nên nguy hiểm nếu con ếch có thể với tới$j$từ$i$sử dụng hệ thống nhảy của nó và đồng thời con chim cũng có thể đạt được$j$từ$i$sử dụng hệ thống bay của nó. Nếu chỉ cần một cặp như vậy tồn tại thì cuối cùng con chim sẽ có thể bắt được con ếch. 

Nhiệm vụ là xây dựng cả hai hệ thống theo cách không tồn tại cặp khả năng tiếp cận chồng chéo như vậy, đồng thời tối đa hóa tổng số cạnh trên cả hai hệ thống. 

Hạn chế chính là mỗi hệ thống có cấu trúc rất hạn chế: bởi vì tất cả các nguồn đều khác biệt và tất cả các mục tiêu đều khác biệt, mỗi hệ thống là một phần khớp được hướng từ trái sang phải. Tuy nhiên, khả năng tiếp cận có tính bắc cầu, do đó, ngay cả khi các cạnh trông đơn giản cục bộ, việc liên kết có thể tạo ra khả năng tiếp cận tầm xa và có khả năng tạo ra sự chồng chéo bị cấm. 

Với$n \le 10^4$, chúng ta có thể đủ khả năng$O(n)$hoặc$O(n \log n)$sự thi công. Bất cứ điều gì liên quan đến lý luận cặp tổng thể trên tất cả$O(n^2)$cặp là không thể. 

Một trường hợp thất bại tinh tế xuất hiện khi một hệ thống tạo thành một chuỗi. Ví dụ: nếu chúng ta xây dựng các cạnh ếch như$1 \to 2, 2 \to 3, 3 \to 4$, thì con ếch có thể đến mọi nút sau từ mọi nút trước đó. Trong tình huống đó, con chim phải tránh hoàn toàn mọi cặp có thể tiếp cận được, điều này khiến cấu trúc của nó gần như không còn gì. Điều này cho thấy các chuỗi dài rất nguy hiểm vì chúng làm bùng nổ khả năng tiếp cận thông qua tính bắc cầu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các cách có thể để gán các cạnh có hướng cho ếch và chim theo các ràng buộc về tính duy nhất, tính toán khả năng tiếp cận trong cả hai biểu đồ và kiểm tra xem có cặp nào có thể tiếp cận được trong cả hai biểu đồ hay không. Điều này nhanh chóng trở nên không khả thi vì ngay cả việc xây dựng khả năng tiếp cận cho một biểu đồ cũng$O(n^2)$trong trường hợp xấu nhất và số lượng cấu hình là tổ hợp. 

Cái nhìn sâu sắc về cấu trúc là khả năng tiếp cận chỉ trở nên lớn khi chuỗi hình thành. Nếu một đồ thị không chứa đường đi có hướng có độ dài lớn hơn một thì mối quan hệ về khả năng tiếp cận của nó chính xác là tập cạnh của nó. Điều này xảy ra khi mọi nút xuất hiện ở nhiều nhất một cạnh với tư cách là nguồn và nhiều nhất là một cạnh là đích, và quan trọng là không có đỉnh nào được sử dụng làm cầu nối trung gian giữa hai cạnh. Nói cách khác, chúng tôi giữ mỗi hệ thống như một sự phù hợp, do đó việc đóng bắc cầu không mở rộng bất cứ điều gì. 

Khi cả hai hệ thống đều khớp nhau, điều kiện sẽ đơn giản hóa đáng kể: một cặp có thể truy cập được khi và chỉ khi nó trực tiếp là một cạnh. Ràng buộc “không có cặp nào có thể truy cập được trong cả hai hệ thống” trở thành “không có cạnh định hướng nào được chia sẻ giữa hai hệ thống”. 

Điều này làm giảm nhiệm vụ đóng gói càng nhiều cạnh có hướng rời rạc càng tốt thành hai phần khớp trên đường thẳng. Vì mỗi kết quả khớp có thể sử dụng mỗi đỉnh nhiều nhất một lần làm điểm bắt đầu và nhiều nhất một lần làm điểm kết thúc, nên mỗi kết quả khớp có thể chứa nhiều nhất$\lfloor n/2 \rfloor$các cạnh và chúng ta có thể xây dựng cả hai đồng thời theo mô hình đan xen sao cho chúng sử dụng hầu hết các cặp liền kề mà không xung đột. 

Một công trình sạch sẽ đạt được tổng thể$n-1$các cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Sức ép mạnh mẽ đối với đồ thị + khả năng tiếp cận | số mũ /$O(n^3)$|$O(n^2)$| Quá chậm | 
| Xây dựng kết hợp xen kẽ |$O(n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hai kết quả phù hợp theo hướng trên đường dẫn$1, 2, \dots, n$, được sắp xếp so le một cách cẩn thận để không có cặp định hướng nào được sử dụng lại. 

1. Xây dựng các cạnh ếch bằng cách ghép các nút liên tiếp bắt đầu từ 1:$1 \to 2, 3 \to 4, 5 \to 6,$và vân vân. 

Điều này đảm bảo biểu đồ ếch là một tập hợp các cạnh độc lập không có đường dẫn nào dài hơn một bước, do đó khả năng tiếp cận giống hệt với tập hợp cạnh. 
2. Tạo các cạnh chim bằng cách dịch chuyển từng cặp một:$2 \to 3, 4 \to 5, 6 \to 7,$và vân vân. 

Điều này cũng tạo thành một sự so khớp, một lần nữa ngăn chặn bất kỳ khả năng tiếp cận nhiều bước nào. 
3. Dừng mỗi hệ thống khi hết cặp hợp lệ. Nếu như$n$là số lẻ, nút cuối cùng vẫn không được sử dụng trong một hoặc cả hai hệ thống, điều này là không thể tránh khỏi do ràng buộc khớp. 
4. In tất cả các cạnh con ếch trước, sau đó đến tất cả các cạnh con chim. 

Cấu trúc đảm bảo rằng mỗi cạnh là duy nhất trên các hệ thống vì các cạnh ếch luôn kết nối các cặp chẵn lẻ, trong khi các cạnh chim kết nối các cặp chẵn lẻ dịch chuyển một. Sự bù đắp cấu trúc này ngăn ngừa sự chồng chéo. 

Tại sao nó hoạt động được là do khả năng tiếp cận bị sụp đổ. Vì cả hai đồ thị đều khớp nhau nên không có đỉnh nào đóng vai trò là điểm nối nối hai cạnh, do đó, mọi cặp có thể tiếp cận đều chính xác là một cạnh. Do đó, cách duy nhất điều kiện cấm có thể xảy ra là nếu cả hai hệ thống đều chứa cùng một cặp định hướng. Cấu trúc xen kẽ ngăn chặn hoàn toàn điều này trong khi tối đa hóa số cạnh mà mỗi hệ thống có thể nắm giữ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    frog = []
    bird = []
    
    # frog: (1,2), (3,4), ...
    for i in range(1, n, 2):
        if i + 1 <= n:
            frog.append((i, i + 1))
    
    # bird: (2,3), (4,5), ...
    for i in range(2, n, 2):
        if i + 1 <= n:
            bird.append((i, i + 1))
    
    m = len(frog) + len(bird)
    print(m)
    
    for a, b in frog:
        print(a, b)
    
    for a, b in bird:
        print(a, b)

if __name__ == "__main__":
    solve()
```Các cạnh ếch được tạo ra bằng cách duyệt qua các chỉ số lẻ, đảm bảo mọi cạnh đều bắt đầu ở vị trí lẻ và kết thúc ở vị trí chẵn tiếp theo. Các cạnh chim được tạo ra một cách đối xứng nhưng bị dịch chuyển, bắt đầu từ các chỉ số chẵn. Điều này đảm bảo rằng không có cặp định hướng nào xuất hiện trong cả hai danh sách. 

Một cạm bẫy phổ biến là cố gắng tối đa hóa các cạnh mà không kiểm soát cấu trúc, điều này vô tình tạo ra các chuỗi như$1 \to 2 \to 3$và gây ra sự đóng cửa chuyển tiếp lớn. Ở đây, chúng tôi tránh điều đó một cách rõ ràng bằng cách đảm bảo mỗi nút tham gia vào tối đa một cạnh trên mỗi biểu đồ, loại bỏ tất cả khả năng tiếp cận nhiều bước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để$n = 6$. 

Các cạnh của ếch là:$1 \to 2, 3 \to 4, 5 \to 6$Các cạnh của chim là:$2 \to 3, 4 \to 5$| Bước | Cạnh ếch | Cạnh chim | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | (1,2) | - | 1 | 
| 2 | (1,2),(3,4) | - | 2 | 
| 3 | (1,2),(3,4),(5,6) | - | 3 | 
| 4 | giống nhau | (2,3) | 4 | 
| 5 | giống nhau | (2,3),(4,5) | 5 | 

Điều này cho thấy mẫu xen kẽ lấp đầy hầu hết các cặp liền kề mà không chồng chéo. 

Bất biến quan sát được là các cạnh ếch luôn bắt đầu từ các chỉ số lẻ, trong khi các cạnh chim luôn bắt đầu từ các chỉ số chẵn, do đó không có cạnh nào trùng khớp. 

### Ví dụ 2 

hãy để$n = 5$. 

Cạnh ếch:$1 \to 2, 3 \to 4$Cạnh chim:$2 \to 3, 4 \to 5$| Bước | Ếch | Chim | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | (1,2) | - | 1 | 
| 2 | (1,2),(3,4) | - | 2 | 
| 3 | giống nhau | (2,3) | 3 | 
| 4 | giống nhau | (2,3),(4,5) | 4 | 

Một lần nữa, không có sự chồng chéo nào xảy ra và tất cả các cạnh đều trực tiếp, do đó khả năng tiếp cận không vượt ra ngoài các cạnh đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục được xử lý một lần để tạo tối đa một cạnh trên mỗi biểu đồ | 
| Không gian |$O(1)$thêm | Chỉ lưu trữ các cạnh đầu ra | 

Việc xây dựng là tuyến tính và dễ dàng phù hợp với các ràng buộc$n \le 10^4$, với mức sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal
assert run("2") == "1\n1 2", "n=2 simplest"

# provided-like small even
assert run("6") == "5\n1 2\n3 4\n5 6\n2 3\n4 5"

# odd case
assert run("5") == "4\n1 2\n3 4\n2 3\n4 5"

# single chain boundary
assert run("3") == "2\n1 2\n2 3"

# larger structure sanity
assert run("8").splitlines()[0] == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | chỉ một cạnh | ranh giới tối thiểu | 
| 6 | mô hình xen kẽ | tính đúng đắn của việc xây dựng | 
| 5 | xử lý độ dài lẻ | hành vi nút còn sót lại | 
| 3 | trường hợp chồng chéo không tầm thường nhỏ nhất | tránh sự hình thành chuỗi | 

## Vỏ cạnh 

cho$n = 2$, công trình tạo ra một cạnh ếch duy nhất$1 \to 2$và không có cạnh chim. Không có khả năng trùng lặp vì chỉ tồn tại một cặp. 

Vì$n = 3$, ếch được$1 \to 2$, chim được$2 \to 3$. Không có khả năng tiếp cận bắc cầu ngoài các cạnh này, do đó không có sự chồng chéo ẩn nào xuất hiện. 

Đối với số lẻ lớn hơn$n$, nút cuối cùng không được sử dụng trong một hoặc cả hai hệ thống. Điều này là không thể tránh khỏi do ràng buộc so khớp và thuật toán sẽ cô lập nó một cách tự nhiên mà không ảnh hưởng đến tính chính xác.
