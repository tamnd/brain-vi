---
title: "CF 961C - Bàn Cờ"
description: "Chúng ta có bốn mảnh hình vuông riêng biệt của bàn cờ, mỗi mảnh là một lưới $n nhân n$ trong đó mọi ô đều đã được tô màu đen hoặc trắng. Giá trị $n$ là số lẻ và mục tiêu là ghép bốn quân cờ này thành một bàn cờ $2n nhân 2n$ lớn hơn."
date: "2026-06-17T01:47:14+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 961
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 41 (Rated for Div. 2)"
rating: 1400
weight: 961
solve_time_s: 82
verified: true
draft: false
---

[CF 961C - Bàn cờ](https://codeforces.com/problemset/problem/961/C) 

**Đánh giá:** 1400 
**Tags:** bitmasks, vũ lực, thực hiện 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn mảnh hình vuông riêng biệt của một bàn cờ, mỗi mảnh là một$n \times n$lưới trong đó mọi ô đều đã được tô màu đen hoặc trắng. giá trị$n$thật kỳ quặc và mục tiêu là lắp ráp bốn mảnh này thành một khối lớn hơn$2n \times 2n$bàn cờ. 

Chúng ta được phép đặt bốn quân cờ theo thứ tự bất kỳ nhưng không được phép xoay, lật chúng. Sau khi đặt chúng vào một$2 \times 2$cách bố trí các khối, chúng ta có thể đổi màu các ô riêng lẻ. Chi phí là số lượng ô mà chúng ta thay đổi màu sắc. Mục đích cuối cùng là kết quả$2n \times 2n$lưới là một bàn cờ hợp lệ, nghĩa là mỗi ô có màu khác nhau so với các ô lân cận. 

Nhiệm vụ là giảm thiểu số lần tô màu lại cần thiết sau khi chọn được cách sắp xếp tốt nhất của bốn mảnh. 

Ràng buộc$n \le 100$có nghĩa là mỗi phần có tối đa 10.000 ô, vì vậy toàn bộ dữ liệu đầu vào đủ nhỏ để chúng ta có thể tính toán chi phí trực tiếp giữa các phần. Bất kỳ giải pháp nào thử tất cả các vị trí đều khả thi vì chỉ có$4!$hoán vị. 

Một điểm tinh tế là sự liền kề trong bàn cờ cuối cùng vượt qua ranh giới quân cờ. Vì vậy, hai mảnh được đặt cạnh nhau sẽ tạo ra các ràng buộc trên các cạnh tiếp xúc của chúng. Đây là nơi xử lý độc lập ngây thơ không thành công. 

Một sai lầm điển hình là cho rằng mỗi quân cờ có thể được chế tạo độc lập thành một mẫu bàn cờ và sau đó kết hợp lại một cách đơn giản. Điều đó bỏ qua rằng tính chẵn lẻ của màu sắc phải phù hợp giữa các ranh giới. Ví dụ: nếu hai quân liền kề đều cho rằng ô trên cùng bên trái có màu đen, nhưng độ lệch tương đối của chúng không đồng đều, xung đột ranh giới sẽ xảy ra và chi phí tăng bất ngờ. 

Một vấn đề tế nhị khác là vì$n$là số lẻ, tính chẵn lẻ của tọa độ bên trong mỗi phần hoạt động nhất quán, nghĩa là mỗi phần có một “căn chỉnh chẵn lẻ của trình kiểm tra” cố định khi được đặt vào lưới cuối cùng. Sự căn chỉnh này phụ thuộc vào góc phần tư nào trong bốn góc phần tư mà quân cờ được gán cho. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực bắt đầu bằng cách nhận thấy rằng mỗi quân cờ có thể được đặt vào bất kỳ góc phần tư nào trong bốn góc phần tư của trận chung kết$2n \times 2n$Cái bảng. có$4!$cách gán các phần cho các góc phần tư, để chúng ta có thể thử mọi hoán vị. 

Đối với một bài tập cố định, chúng ta phải tính xem cần lật bao nhiêu ô để bảng cuối cùng luân phiên chính xác. Bản thân bàn cờ cuối cùng chỉ có hai màu hợp lệ: ô trên cùng bên trái có màu đen hoặc màu trắng. Khi lựa chọn bắt đầu đó được cố định, mọi ô khác sẽ được xác định theo tính chẵn lẻ của tọa độ. 

Vì vậy, đối với mỗi hoán vị và đối với mỗi màu trong số hai màu bắt đầu, chúng tôi tính toán chi phí không khớp trên tất cả$4n^2$tế bào. Điều đó đã đủ cho sự đúng đắn. 

Quan sát quan trọng là chúng ta không bao giờ cần phải mô phỏng việc đổi màu tăng dần. Mỗi ô đóng góp độc lập: chúng ta chỉ cần kiểm tra xem màu hiện tại của nó có khớp với màu bàn cờ dự kiến ​​ở vị trí cuối cùng hay không. 

Điều này làm giảm vấn đề khi đánh giá một số lượng cấu hình không đổi:$4!$hoán vị lần 2 màu tổng thể, mỗi màu được kiểm tra$O(n^2)$. 

Một cái nhìn sâu sắc về cấu trúc tinh tế hơn là mỗi mảnh luôn nằm trong một góc phần tư cố định, do đó tọa độ của nó bị dịch chuyển nhưng không bị biến đổi. Điều này làm cho công thức màu mong đợi trở nên thống nhất và dễ tính toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các vị trí với mô phỏng đầy đủ |$O(4! \cdot n^2)$|$O(n^2)$| Đã chấp nhận | 
| Bảng liệt kê tối ưu với tính toán chi phí chẵn lẻ |$O(4! \cdot n^2)$|$O(n^2)$| Đã chấp nhận | 

Trong thực tế, cả hai mô tả đều thu gọn theo cùng một thuật toán, nhưng khung “tối ưu” tránh mô phỏng trạng thái không cần thiết và tập trung hoàn toàn vào so sánh chẵn lẻ. 

## Hướng dẫn thuật toán 

Chúng tôi dán nhãn cho bốn mảnh là$P_0, P_1, P_2, P_3$. Chúng tôi xem xét tất cả các cách để gán chúng cho bốn góc phần tư: trên cùng bên trái, trên cùng bên phải, dưới cùng bên trái, dưới cùng bên phải. 

1. Tạo tất cả các hoán vị của bốn phần. Mỗi hoán vị thể hiện một vị trí cố định trong các góc phần tư. Điều này là đủ vì các góc phần tư là khác nhau và không có sự suy giảm tính đối xứng khi quay. 
2. Với mỗi hoán vị, hãy xem xét hai cách tô màu bàn cờ cuối cùng có thể xảy ra. Người ta cho rằng tế bào$(0,0)$là màu trắng, người kia cho rằng nó là màu đen. Đây là những mẫu chung hợp lệ duy nhất vì mỗi bàn cờ được xác định bởi ô bắt đầu của nó. 
3. Đối với mỗi ô$(i, j)$bên trong một mảnh, tính tọa độ cuối cùng của nó trong$2n \times 2n$bảng tùy thuộc vào góc phần tư được chỉ định của nó. Đây là một sự bù đắp đơn giản: phía trên bên trái thêm$(0,0)$, phía trên bên phải thêm$(0,n)$, phía dưới bên trái thêm$(n,0)$, phía dưới bên phải thêm$(n,n)$. 
4. Xác định màu dự kiến ​​ở vị trí cuối cùng bằng cách sử dụng tính chẵn lẻ. Nếu chúng ta xác định màu bắt đầu là 0 thì màu mong đợi là$(i + j) \bmod 2$. Nếu màu bắt đầu là 1, chúng ta đảo ngược màu này. 
5. So sánh màu dự kiến ​​với màu ô đã cho và tích lũy số lượng không khớp. Số lượng này thể hiện số lượng thao tác đổi màu cần thiết cho cấu hình này. 
6. Theo dõi sự không khớp tối thiểu trên tất cả các hoán vị và cả hai lựa chọn màu bắt đầu. 

Kết quả là mức tối thiểu trên một không gian cấu hình hữu hạn trong đó mỗi chi phí cấu hình được tính theo thời gian tuyến tính trên lưới. 

### Tại sao nó hoạt động 

Mỗi bàn cờ cuối cùng hợp lệ được xác định duy nhất bằng hai lựa chọn: sắp xếp các quân cờ vào các góc phần tư và độ lệch chẵn lẻ toàn cục. Sau khi các giá trị này được cố định, màu chính xác của mỗi ô sẽ bị ép buộc theo tính chẵn lẻ và độc lập với tất cả các ô khác. Do đó, hàm chi phí phân hủy thành tổng các giá trị không khớp độc lập trên mỗi ô và việc tìm kiếm toàn diện trên không gian cấu hình nhỏ đảm bảo rằng mức tối ưu toàn cục được kiểm tra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def read_piece(n):
    g = []
    for _ in range(n):
        line = input().strip()
        g.append([int(c) for c in line])
    return g

def solve():
    n = int(input().strip())
    input()  # consume empty line after n

    pieces = []
    for _ in range(4):
        pieces.append(read_piece(n))
        input()  # empty line between pieces

    # quadrant offsets
    offsets = [(0, 0), (0, n), (n, 0), (n, n)]

    import itertools

    ans = 10**18

    for perm in itertools.permutations(range(4)):
        for start in (0, 1):
            cost = 0

            for qi in range(4):
                pi = perm[qi]
                ox, oy = offsets[qi]
                piece = pieces[pi]

                for i in range(n):
                    for j in range(n):
                        final_parity = (ox + i + oy + j) & 1
                        expected = final_parity ^ start
                        cost += (piece[i][j] != expected)

            ans = min(ans, cost)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ phân tích bốn lưới một cách cẩn thận, chú ý đến các dòng trống giữa các khối. Đây là nguồn lỗi phổ biến trong vấn đề này, vì việc bỏ qua hoặc đọc sai dấu phân cách sẽ dịch chuyển tất cả các phần tiếp theo không chính xác. 

Mỗi hoán vị gán các phần cho các góc phần tư theo một thứ tự cố định. Mảng offset mã hóa vị trí của từng góc phần tư trong bảng cuối cùng. Đối với mỗi ô, chúng tôi tính toán mức chẵn lẻ cuối cùng của nó bằng cách sử dụng phép cộng tọa độ và sau đó XOR với màu bắt đầu chung đã chọn. XOR được sử dụng vì việc lật màu bắt đầu chỉ đơn giản là đảo ngược tất cả các giá trị mong đợi. 

Các vòng lặp lồng nhau$4n^2$tế bào được chấp nhận vì$n \le 100$, vậy tổng số phép toán xấp xỉ$4 \cdot 4! \cdot 2 \cdot 10^4$, dễ dàng nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
0

0

1

0
```Chúng tôi có bốn$1 \times 1$miếng. Mỗi mảnh là một ô duy nhất. 

| Hoán vị | Bắt đầu | Chi phí vị trí | 
| --- | --- | --- | 
| (0,1,2,3) | 0 | 1 | 
| (0,1,2,3) | 1 | 3 | 
| (3,1,2,0) | 0 | 1 | 
| (3,1,2,0) | 1 | 3 | 

Sự sắp xếp tốt nhất đạt được chi phí 1. 

Điều này cho thấy rằng
