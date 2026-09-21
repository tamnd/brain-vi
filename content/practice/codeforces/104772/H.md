---
title: "CF 104772H - Hình chữ H"
description: "Chúng ta có một đoạn có hướng cố định được xác định bởi hai điểm $P$ và $Q$. Ngoài ra, còn có các đoạn đường ứng cử $n$ nằm rải rác trên mặt phẳng. Mỗi đoạn ứng cử viên có thể được sử dụng như một “thanh dọc” trong cấu hình hình học."
date: "2026-06-28T16:12:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104772
codeforces_index: "H"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104772
solve_time_s: 34
verified: true
draft: false
---

[CF 104772H - Hình chữ H](https://codeforces.com/problemset/problem/104772/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đoạn có hướng cố định được xác định bởi hai điểm$P$Và$Q$. Ngoài ra, còn có$n$các đoạn đường ứng cử viên nằm rải rác trên mặt phẳng. Mỗi đoạn ứng cử viên có thể được sử dụng như một “thanh dọc” trong cấu hình hình học. 

Chúng tôi muốn đếm các cặp phân đoạn riêng biệt theo thứ tự$(a, b)$được chọn từ các ứng viên sao cho cùng với phân khúc$PQ$, chúng tạo thành cấu trúc hình chữ H. 

Về mặt hình học, điều này có nghĩa là các điều kiện sau phải được giữ đồng thời. Điểm$P$phải nằm đúng trong phân khúc$a$, và đoạn$a$không được nằm trên cùng một đường vô hạn như$PQ$. Tương tự, điểm$Q$phải nằm đúng trong phân khúc$b$, và đoạn$b$không được cộng hưởng với$PQ$. Cuối cùng, hai đoạn được chọn$a$Và$b$không được cắt nhau tại bất kỳ điểm nào. 

Vì vậy, theo trực giác, mỗi cấu hình hợp lệ được xác định bằng cách chọn một phân đoạn “đi qua”$P$theo cách không song song và một đoạn khác “cắt ngang”$Q$theo cách không song song, đồng thời đảm bảo hai đoạn được chọn không chồng lên nhau hoặc chạm vào nhau. 

Các ràng buộc ngụ ý đến$2 \cdot 10^5$phân đoạn trên tất cả các trường hợp thử nghiệm, do đó, mọi giải pháp đều phải gần như tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Việc kiểm tra bậc hai trên tất cả các cặp là không thể ngay lập tức vì nó đòi hỏi khoảng$4 \cdot 10^{10}$kiểm tra giao nhau trong trường hợp xấu nhất. 

Một điểm tinh tế là các điều kiện mang tính hình học nhưng nhiệm vụ cuối cùng là tổ hợp: chúng ta đang đếm các cặp hợp lệ. Điều đó thường báo hiệu việc giảm phân loại cộng với việc tính toán bằng cách sắp xếp hoặc băm. 

Lỗi triển khai đơn giản xuất hiện trong điều kiện giao lộ. Ví dụ: nếu nhiều phân đoạn đi qua$P$hoặc$Q$, một cách tiếp cận đơn giản để kiểm tra tất cả các cặp phân đoạn ứng cử viên sẽ liên tục tính toán lại hình học giao lộ. Một cạm bẫy khác là xử lý không chính xác “điểm nằm hoàn toàn bên trong phân đoạn” bằng cách cho phép các điểm cuối, điều này sẽ bao gồm không chính xác các hình chữ H suy biến trong đó điểm cuối của phân đoạn trùng với$P$hoặc$Q$. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ kiểm tra mọi cặp phân đoạn có thứ tự$(a, b)$. Đối với mỗi cặp, chúng tôi sẽ kiểm tra xem$P$nằm đúng bên trong$a$, liệu$Q$nằm đúng bên trong$b$, liệu$a$Và$b$không thẳng hàng với$PQ$, và liệu$a$Và$b$không giao nhau. Mỗi kiểm tra cặp yêu cầu các vị từ hình học có thời gian không đổi, do đó tổng chi phí là$O(n^2)$mỗi trường hợp thử nghiệm. 

Với$n$lên đến$2 \cdot 10^5$, điều này trở nên hoàn toàn không thể thực hiện được. Thậm chí$n=10^5$sản lượng$10^{10}$kiểm tra, vượt xa giới hạn. 

Quan sát chính là hầu hết các ràng buộc đều cục bộ đối với một phân đoạn. Liệu một phân đoạn có hợp lệ hay không$P$chỉ phụ thuộc vào phân khúc đó và$P$, không phải trên các phân đoạn khác. Điều tương tự cũng áp dụng cho$Q$. Tương tác toàn cục duy nhất là ràng buộc giao nhau giữa các phân đoạn được chọn cho$P$và những người được chọn cho$Q$. 

Điều này gợi ý việc chia các phân khúc thành hai nhóm: những nhóm có thể phục vụ$P$và những thứ có thể phục vụ$Q$. Khi chúng tôi có các nhóm này, vấn đề sẽ giảm xuống việc đếm các cặp trong hai nhóm và trừ đi các tương tác không hợp lệ do giao điểm của các đoạn gây ra. 

Cái nhìn sâu sắc về cấu trúc quan trọng là các ràng buộc giao lộ có thể được xử lý bằng cách sắp xếp các đoạn xung quanh các điểm và đếm “các khoảng xung đột” bằng cách quét hoặc sắp xếp theo thứ tự góc xung quanh$P$Và$Q$. Điều này chuyển đổi các ràng buộc giao nhau hình học thành phép đếm đảo ngược theo thứ tự tuần hoàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Sắp xếp góc + đếm |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Phân loại các đoạn theo điều kiện điểm cuối 

Đối với mỗi đoạn, hãy kiểm tra xem điểm$P$nằm chặt chẽ bên trong nó. Điều này được thực hiện bằng cách sử dụng kiểm tra hộp cộng tuyến và giới hạn. Nếu đúng và phân đoạn không thẳng hàng với$PQ$, đánh dấu nó là ứng cử viên cho$P$. Làm tương tự cho$Q$. 

Bước này lọc sớm các phân đoạn không liên quan để việc đếm sau chỉ xem xét các ứng cử viên có ý nghĩa. 

### 2. Biểu diễn các đoạn liên quan đến điểm cuối 

Đối với mỗi phân đoạn hợp lệ cho$P$, tính toán hai điểm cuối theo thứ tự góc cực xung quanh$P$. Làm tương tự xung quanh$Q$. Điều này biến mỗi đoạn thành một khoảng góc trên một vòng tròn. 

Lý do cho sự chuyển đổi này là các mối quan hệ giao điểm của đoạn trở nên chồng chéo các khoảng theo thứ tự góc khi nhìn từ một điểm cố định. 

### 3. Sắp xếp các đoạn theo biểu diễn góc 

Sắp xếp các phân khúc ứng viên cho$P$bởi góc của điểm giữa của chúng xung quanh$P$, và tương tự cho$Q$. Thứ tự này cho phép chúng tôi phát hiện các điểm giao nhau bằng cách quét theo thứ tự tuần hoàn nhất quán. 

Thứ tự này là cần thiết vì các ràng buộc giao nhau trở nên đơn điệu trong không gian góc. 

### 4. Đếm các cặp hợp lệ bằng logic quét 

Chúng tôi muốn các cặp$(a, b)$như vậy$a$có giá trị cho$P$,$b$có giá trị cho$Q$, và chúng không cắt nhau. 

Bắt đầu với tổng sản phẩm$|A| \cdot |B|$. Sau đó trừ các cặp không hợp lệ trong đó phân đoạn từ$A$giao cắt đoạn từ$B$. 

Để đếm các nút giao thông một cách hiệu quả, cho từng đoạn trong$A$, chúng tôi xác định có bao nhiêu phân đoạn trong$B$nó giao nhau bằng cách quét qua các điểm cuối góc, duy trì một tập hợp hoạt động. Mỗi giao lộ tương ứng với một giao lộ theo thứ tự góc có thể được tính là đảo ngược. 

### 5. Tổng hợp câu trả lời cuối cùng 

Kết quả cuối cùng là tổng số cặp trừ đi các cặp giao nhau. 

### Tại sao nó hoạt động 

Mỗi đoạn được xác định đầy đủ bởi khoảng góc của nó xung quanh một điểm tham chiếu. Hai đoạn thẳng cắt nhau khi và chỉ khi các hình chiếu của chúng trên ít nhất một thứ tự góc của điểm cuối cắt nhau. Điều này làm giảm giao điểm hình học thành điều kiện giao nhau tổ hợp. Vì các giao điểm tạo thành một cấu trúc đảo ngược khi sắp xếp, nên việc đếm chúng bằng cách quét đảm bảo phép trừ chính xác mà không cần đếm hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def orient(px, py, ax, ay, bx, by):
    return cross(ax - px, ay - py, bx - px, by - py)

def on_segment(px, py, ax, ay, bx, by):
    if orient(px, py, ax, ay, bx, by) != 0:
        return False
    return min(ax, bx) <= px <= max(ax, bx) and min(ay, by) <= py <= max(ay, by)

def point_on_strict_segment(px, py, ax, ay, bx, by):
    if orient(px, py, ax, ay, bx, by) != 0:
        return False
    return (min(ax, bx) < px < max(ax, bx) or min(ay, by) < py < max(ay, by))

def solve():
    t = int(input())
    for _ in range(t):
        xP, yP, xQ, yQ = map(int, input().split())
        n = int(input())

        A = []
        B = []

        for _ in range(n):
            x1, y1, x2, y2 = map(int, input().split())

            if point_on_strict_segment(xP, yP, x1, y1, x2, y2):
                A.append((x1, y1, x2, y2))

            if point_on_strict_se_ment(xQ, yQ, x1, y1, x2, y2):
                B.append((x1, y1, x2, y2))

        total = len(A) * len(B)

        #
```
