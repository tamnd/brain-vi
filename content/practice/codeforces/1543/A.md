---
title: "CF 1543A - Cược Sôi Động"
description: "Chúng ta có hai số nguyên không âm và chúng ta có thể di chuyển chúng theo từng bước: mọi thao tác đều thay đổi cả hai giá trị với cùng một lượng, tăng cả hai một hoặc giảm cả hai một (miễn là chúng ta không xuống dưới 0)."
date: "2026-06-16T15:25:08+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1543
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 730 (Div. 2)"
rating: 900
weight: 1543
solve_time_s: 486
verified: true
draft: false
---

[CF 1543A - Cược thú vị](https://codeforces.com/problemset/problem/1543/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học, lý thuyết số 
**Thời gian giải:** 8m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai số nguyên không âm và chúng ta có thể di chuyển chúng theo từng bước: mọi thao tác đều thay đổi cả hai giá trị với cùng một lượng, tăng cả hai một hoặc giảm cả hai một (miễn là chúng ta không xuống dưới 0). 

Tỉ số mà chúng ta quan tâm chính là ước số chung lớn nhất của hai số sau một chuỗi nước đi bất kỳ. Chúng tôi muốn tối đa hóa điểm số này và trong số tất cả các cách để đạt được GCD tối đa có thể, chúng tôi muốn số lần di chuyển cần thiết là nhỏ nhất. 

Một quan sát quan trọng là mọi nước đi đều bảo toàn sự khác biệt giữa hai con số. Nếu chúng ta định nghĩa$d = a - b$, thì cả hai thao tác đều giữ nguyên$a - b$không thay đổi. Điều này có nghĩa là toàn bộ quá trình bị hạn chế ở các điểm có dạng$(x, x - d)$hoặc$(x + d, x)$tùy theo đơn đặt hàng. 

Các ràng buộc đi lên đến$10^{18}$, vì vậy bất kỳ giải pháp nào thử tất cả các trạng thái có thể truy cập đều không thể thực hiện được. Thậm chí quét một loạt các giá trị xung quanh$a$Và$b$là không khả thi vì các giá trị có thể tiếp cận trải dài trên một đường vô hạn theo cả hai hướng. 

Trường hợp cạnh tinh tế xuất hiện khi$a = b$. Trong trường hợp đó, việc tăng cả hai số sẽ bảo toàn sự bằng nhau và giữ cho GCD bằng chính giá trị đó và nó có thể tăng lên mà không bị giới hạn. Điều này dẫn đến sự phấn khích vô hạn, cần phải được phát hiện. 

Một trường hợp góc khác phát sinh khi một giá trị bằng 0. Từ$gcd(x, 0) = x$, việc giảm cả hai giá trị có thể là không thể, nhưng việc tăng cả hai giá trị cho phép đạt được các giá trị bằng nhau lớn tùy ý miễn là chúng ta không tự chặn mình bằng những ràng buộc tiêu cực. Tuy nhiên, lý luận tối ưu vẫn phụ thuộc vào sai phân bất biến. 

Một sai lầm ngây thơ là cho rằng chúng ta có thể điều chỉnh các giá trị một cách độc lập theo hướng có nhiều mối quan hệ. Chúng ta không thể thay đổi sự khác biệt, vì vậy bất kỳ cặp mục tiêu nào cũng phải thỏa mãn mối quan hệ tuyến tính cố định. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng tất cả các trạng thái có thể tiếp cận được từ$(a, b)$bởi BFS hoặc DFS, cập nhật cả hai giá trị ±1 cùng nhau. Điều này nhanh chóng trở thành vô hạn vì đồ thị trạng thái là một đường vô hạn theo cả hai hướng. Ngay cả khi chúng ta hạn chế các giới hạn theo phương pháp heuristic, GCD tối ưu có thể xảy ra rất xa, vì GCD phụ thuộc vào khả năng chia hết chứ không phải độ gần. 

Cái nhìn sâu sắc về cấu trúc quan trọng là sự khác biệt$d = |a - b|$không bao giờ thay đổi. Bất kỳ cặp nào có thể truy cập đều có thể được viết là:$$(a', b') = (x, x - d)$$đối với một số nguyên$x$(giả sử đặt hàng). 

Chúng tôi muốn chọn$x$như vậy:$$\gcd(x, x - d)$$được tối đa hóa. 

Sử dụng danh tính:$$\gcd(x, x - d) = \gcd(x, d)$$vấn đề giảm xuống việc lựa chọn$x$trên một dòng sao cho nó chia sẻ ước số lớn nhất có thể với một số cố định$d$, đồng thời giảm thiểu khoảng cách từ điểm xuất phát. 

GCD tối đa có thể chỉ đơn giản là$d$chính nó khi có thể, nhưng chúng ta phải đảm bảo tính khả thi với ràng buộc là chúng ta chỉ có thể đạt được các giá trị phù hợp với các bước nguyên từ$a$Và$b$. 

Kết quả cơ cấu quan trọng là GCD có thể đạt được tốt nhất là$g = |a - b|$liệu chúng ta có thể căn chỉnh một số thành một cấu trúc bội thông qua dịch chuyển hay không; nếu không thì khi$a = b$, câu trả lời là vô hạn. 

Điều này làm giảm nhiệm vụ thành một phép tính lý thuyết số đơn giản dựa trên sự căn chỉnh sai phân và khoảng cách đến cấu hình hợp lệ gần nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Vô hạn (trạng thái bùng nổ) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu$a = b$, lập tức trở lại hưng phấn vô hạn. 

Lý do là cả hai số luôn bằng nhau sau bất kỳ số lượng hoạt động nào và các hoạt động tăng dần lặp đi lặp lại làm cho GCD tăng lên không giới hạn. 
2. Tính toán$d = |a - b|$. 

Sự khác biệt này là bất biến trong tất cả các phép toán được phép, do đó mọi trạng thái có thể truy cập đều nằm trên một dòng số học cố định. 
3. Nếu$d = 0$, chúng ta đã xử lý nó rồi, vì vậy hãy tiếp tục giả sử$d > 0$. 
4. Tính GCD tốt nhất có thể đạt được là$g = d$. 

Điều này xuất phát từ danh tính$\gcd(x, x - d) = \gcd(x, d)$, nghĩa là GCD luôn là ước số của$d$, và chúng ta có thể căn chỉnh$x$để tối đa hóa số chia này, đạt được$d$chính nó. 
5. Xác định số lần di chuyển tối thiểu cần thiết để đạt được cấu hình trong đó một trong các số chia hết cho$d$. 

Vì mỗi lần di chuyển sẽ dịch chuyển cả hai số đi 1, nên chúng ta đang dịch chuyển cặp dọc theo dòng số nguyên một cách hiệu quả. Chúng tôi chọn nhiều căn chỉnh gần nhất để giảm thiểu khoảng cách. 
6. Tính xem bao xa$a$là từ bội số gần nhất của$d$và sử dụng sự thay đổi đó làm số lần di chuyển. 

### Tại sao nó hoạt động 

Điều bất biến là sự khác biệt$a - b$không bao giờ thay đổi. Điều này hạn chế tất cả các trạng thái có thể tiếp cận được trong một mạng một chiều. Trên mạng này, GCD đơn giản hóa thành hàm một biến và một hằng số cố định$d$. Vì mọi GCD có thể đạt được đều là ước số của$d$, mức tối đa đạt được bằng cách căn chỉnh trạng thái sao cho một tọa độ là bội số của$d$. Số lần di chuyển tối thiểu theo sau vì mỗi thao tác sẽ dịch chuyển toàn bộ điểm mạng chính xác một đơn vị, do đó việc đạt được căn chỉnh hợp lệ gần nhất là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())

        if a == b:
            print(0, 0)
            continue

        d = abs(a - b)

        # shift to nearest multiple alignment
        r = a % d
        move = min(r, d - r)

        print(d, move)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ kiểm tra trường hợp tương đương, đây là kịch bản duy nhất dẫn đến tăng trưởng không giới hạn. Sau đó, mọi thứ phụ thuộc vào sự khác biệt bất biến$d$. Phần còn lại của$a$modulo$d$cho biết chúng ta còn bao xa nữa mới sắp xếp được$a$đến bội số của$d$và việc dịch chuyển cả hai số sẽ đảm bảo tính khả thi trong khi di chuyển dọc theo đường có thể tiếp cận. 

Việc lựa chọn chỉ sử dụng$a \% d$hợp lệ vì việc dịch chuyển cả hai số ảnh hưởng đến cả hai số một cách đối xứng và điều kiện căn chỉnh chỉ phụ thuộc vào độ chia hết so với chênh lệch cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 5
```Đây$d = 3$. 

| Bước | một | b | d | một % d | Di chuyển | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 8 | 5 | 3 | 2 | - | 
| Tính d | 8 | 5 | 3 | 2 | - | 
| Phần còn lại | 8 | 5 | 3 | 2 | 1 | 
| Kết quả | - | - | 3 | - | 1 | 

GCD tối ưu là 3, đạt được bằng cách dịch chuyển một lần. Điều này chứng tỏ rằng sự liên kết tốt nhất được tìm thấy bằng cách hướng tới một cấu trúc đa dạng của sự khác biệt. 

### Ví dụ 2 

đầu vào:```
3 9
```Đây$d = 6$. 

| Bước | một | b | d | một % d | Di chuyển | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 3 | 9 | 6 | 3 | - | 
| Tính d | 3 | 9 | 6 | 3 | - | 
| Phần còn lại | 3 | 9 | 6 | 3 | 3 | 
| Kết quả | - | - | 6 | - | 3 | 

Trường hợp này cho thấy rằng đôi khi nước đi tốt nhất không bằng 0, bởi vì chúng ta cần dịch chuyển để đạt được cấu hình trong đó khả năng chia hết cho hiệu trở nên tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu các phép tính số học không đổi | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Các ràng buộc cho phép lên tới 5000 trường hợp thử nghiệm với các giá trị lên tới$10^{18}$, vì vậy một$O(1)$mỗi giải pháp thử nghiệm là cần thiết. Thuật toán thỏa mãn điều này một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        if a == b:
            out.append("0 0")
        else:
            d = abs(a - b)
            r = a % d
            out.append(f"{d} {min(r, d - r)}")
    return "\n".join(out)

# provided samples
assert run("""4
8 5
1 2
4 4
3 9
""") == """3 1
1 0
0 0
6 3"""

# custom cases
assert run("1\n0 5\n") == "5 0"
assert run("1\n10 10\n") == "0 0"
assert run("1\n7 3\n") == "4 1"
assert run("1\n1000000000000000000 999999999999999999\n") == "1 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 5 | 5 0 | hành vi không có ranh giới | 
| 10 10 | 0 0 | trường hợp đúng vô hạn | 
| 7 3 | 4 1 | sự dịch chuyển phần dư không tầm thường | 
| giá trị liền kề lớn | 1 0 | hiệu suất và tỷ lệ cạnh | 

## Vỏ cạnh 

Khi nào$a = b$, thuật toán trả về ngay$0, 0$. Ví dụ, đầu vào$4, 4$tạo ra sự bình đẳng và các mức tăng lặp lại cho phép GCD tăng trưởng không giới hạn, do đó, việc thoát sớm nắm bắt chính xác điều này. 

Khi một giá trị bằng 0, chẳng hạn như$0, 5$, chênh lệch là 5 và logic còn lại vẫn được áp dụng. Việc tính toán mang lại khoảng cách dịch chuyển hợp lệ bằng 0, nghĩa là không cần chuyển động để tối đa hóa GCD. 

Khi các giá trị đã gần nhau, chẳng hạn như$7, 3$, hiệu là 4 và phần còn lại của 7 modulo 4 là 3. Thuật toán chọn chính xác độ dịch chuyển tối thiểu để đạt được cấu hình phù hợp với khả năng chia hết cho 4. 

Khi các giá trị cực kỳ lớn nhưng chênh lệch 1, thì chênh lệch là 1 và mọi cấu hình đều có GCD 1. Logic còn lại tạo ra bước di chuyển bằng 0 vì không thể cải thiện được.
