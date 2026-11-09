---
title: "CF 1503B - Sơn 3 màu"
description: "Alice và Bob đang lấp đầy một lưới $n lần n$. Ở mỗi lượt, Alice lần đầu tiên thông báo một màu từ ${1,2,3}$. Sau đó, Bob phải chọn một ô trống và đặt một mã thông báo có màu khác. Bob sẽ thua ngay lập tức nếu hai ô liền kề có cùng màu."
date: "2026-06-10T20:54:11+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "games", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1503
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 712 (Div. 1)"
rating: 1700
weight: 1503
solve_time_s: 389
verified: false
draft: false
---

[CF 1503B - 3-Tô màu](https://codeforces.com/problemset/problem/1503/B) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, trò chơi, tương tác 
**Thời gian giải:** 6 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Alice và Bob đang điền vào một$n \times n$lưới. Ở mỗi lượt, Alice lần đầu tiên thông báo một màu từ$\{1,2,3\}$. Sau đó, Bob phải chọn một ô trống và đặt một mã thông báo có màu khác. 

Bob sẽ thua ngay lập tức nếu hai ô liền kề có cùng màu. Mục tiêu của anh ấy là lấp đầy toàn bộ bảng đồng thời tránh những xung đột như vậy. 

Trong phiên bản tương tác ban đầu, Alice chọn màu sắc một cách thích ứng. Trong phiên bản hack được sử dụng trên Codeforces, chúng ta được cung cấp trước tất cả các lựa chọn màu sắc trong tương lai của Alice. Chúng ta chỉ cần xuất ra bất kỳ chuỗi nước đi hợp lệ nào của Bob. 

Kích thước bảng tối đa là$100 \times 100$, vậy có nhiều nhất$10^4$rẽ. Bất kỳ giải pháp nào thực hiện một lượng công việc không đổi trên mỗi lần di chuyển đều đủ nhanh. Thách thức không phải là sự phức tạp về mặt tính toán, mà là việc xây dựng các bước di chuyển luôn hợp pháp bất kể lựa chọn của Alice như thế nào. 

Điểm tinh tế đầu tiên là Bob không thể tô màu các ô một cách tham lam bằng một màu cố định. Coi như$n=2$và Alice liên tục chọn màu$1$. Nếu Bob luôn phản hồi bằng màu sắc$2$, các ô liền kề cuối cùng sẽ nhận được cùng màu và xuất hiện xung đột. 

Một sai lầm dễ mắc nữa là phân vùng bảng một cách chính xác nhưng lại quên rằng màu mà Bob chọn phải khác với màu hiện tại của Alice. Giả sử một ô thuộc nhóm được cho là sẽ nhận màu$1$. Nếu Alice cũng thông báo$1$, Bob không thể sử dụng màu đó và phải tạm thời sử dụng màu$3$thay vì. 

Cạm bẫy thứ ba xuất hiện ở gần cuối trò chơi. Một phân vùng có thể trống trước phân vùng kia. Giải pháp luôn cố gắng phát từ phân vùng ưa thích sẽ không thành công khi phân vùng đó hết ô. Chiến lược phải xử lý rõ ràng tình huống chỉ còn lại một phân vùng. 

## Phương pháp tiếp cận 

Tư duy vũ phu là xử lý mọi hành động một cách độc lập. Cho màu của Alice, tìm kiếm tất cả các ô trống và tất cả các màu được phép, tìm kiếm một nước đi giữ cho bảng không bị xung đột. Một động thái như vậy luôn tồn tại nhưng việc chứng minh và thực hiện nó một cách rõ ràng là rất khó. Ngay cả khi chúng tôi quét toàn bộ bảng mỗi lượt, độ phức tạp vẫn xấp xỉ$O(n^4)$: có$n^2$lượt và mỗi lượt có thể kiểm tra$O(n^2)$tế bào. Với$n=100$, nó đạt tới khoảng$10^8$hoạt động. 

Quan sát chính xuất phát từ cấu trúc của biểu đồ lưới. Màu bàn cờ sẽ chia các ô thành hai bộ độc lập. Mỗi cạnh kết nối một ô đen và một ô trắng. 

Giả sử chúng ta dự trữ màu$1$cho tất cả các ô màu đen và màu$2$cho tất cả các tế bào màu trắng. Nếu chúng ta luôn làm theo kế hoạch đó thì các ô liền kề sẽ tự động có màu khác nhau vì các ô lân cận thuộc các phân vùng đối diện. 

Alice chỉ ngăn một màu tại một thời điểm. Nếu cô ấy nói$1$, chúng ta vẫn có thể đặt màu$2$. Nếu cô ấy nói$2$, chúng ta vẫn có thể đặt màu$1$. Tình huống khó khăn duy nhất xảy ra khi phân vùng có sẵn màu dự trữ đã hết. 

Giải pháp là duy trì hai danh sách: 

- Các ô của một bàn cờ chẵn lẻ, gắn liền với màu sắc$1$. 
- Các ô tương đương khác, liên quan đến màu sắc$2$. 

Miễn là cả hai danh sách đều không trống, lựa chọn của Alice sẽ xác định phân vùng nào chúng ta sử dụng. Nếu Alice nói$1$, chúng tôi lấy một ô từ màu-$2$phân vùng và sơn nó$2$. Nếu Alice nói$2$, chúng tôi lấy một ô từ màu-$1$phân vùng và sơn nó$1$. Nếu Alice nói$3$, phân vùng đều hoạt động. 

Cuối cùng một phân vùng trở nên trống rỗng. Tại thời điểm đó tất cả các ô còn lại thuộc về phân vùng khác. Màu sắc dành riêng của họ đôi khi có thể bị Alice cấm. Khi điều đó xảy ra, chúng tôi sử dụng màu sắc$3$thay vì. Vì tất cả các ô còn lại nằm trong cùng một phân vùng bàn cờ nên không có ô nào liền kề nhau, do đó giới thiệu màu sắc$3$không thể tạo ra xung đột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^4)$|$O(n^2)$| Quá chậm/không cần thiết | 
| Tối ưu |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia tất cả các ô bàn cờ thành hai nhóm theo tính chẵn lẻ của bàn cờ. 

Tế bào với$(i+j)\bmod 2=0$đi vào nhóm đầu tiên. Tế bào với$(i+j)\bmod 2=1$đi vào nhóm thứ hai. 
2. Màu dự trữ$1$cho nhóm đầu tiên và màu sắc$2$cho nhóm thứ hai. 

Việc gán này đảm bảo rằng các ô lân cận sẽ nhận được các màu khác nhau nếu việc đặt trước được thực hiện chính xác. 
3. Đối với mỗi màu được Alice thông báo, trước tiên hãy kiểm tra xem cả hai nhóm có còn chứa các ô chưa sử dụng hay không. 
4. Nếu cả hai nhóm đều không trống và Alice chọn màu$1$, lấy bất kỳ ô nào chưa sử dụng từ nhóm thứ hai và tô màu cho nó$2$. 

Màu sắc$2$là hợp pháp vì nó khác với màu của Alice. 
5. Nếu cả hai nhóm đều không trống và Alice chọn màu$2$, lấy bất kỳ ô nào chưa sử dụng từ nhóm đầu tiên và tô màu cho nó$1$. 
6. Nếu cả hai nhóm đều không trống và Alice chọn màu$3$, chọn một trong hai nhóm. Thông thường, sử dụng nhóm đầu tiên và tô màu cho nó$1$. 
7. Nếu nhóm đầu tiên trống, tất cả các ô còn lại thuộc nhóm thứ hai. 

Màu dành riêng của họ là$2$. 

Nếu Alice không chọn$2$, sơn ô còn lại bằng$2$. 

Nếu không thì sơn nó bằng$3$. 
8. Nếu nhóm thứ hai trống, tất cả các ô còn lại thuộc nhóm thứ nhất. 

Màu dành riêng của họ là$1$. 

Nếu Alice không chọn$1$, sơn ô còn lại bằng$1$. 

Nếu không thì sơn nó bằng$3$. 

### Tại sao nó hoạt động 

Điều bất biến là mọi ô từ phân vùng bàn cờ đầu tiên đều nhận được một trong hai màu$1$hoặc, khi bị ép buộc, màu sắc$3$. Mọi ô từ phân vùng thứ hai đều nhận được màu$2$hoặc, khi bị ép buộc, màu sắc$3$. 

Mặc dù cả hai phân vùng đều có sẵn nhưng chúng tôi không bao giờ sử dụng màu$3$. Mỗi phân vùng chỉ nhận được màu dành riêng nên các ô liền kề luôn có màu khác nhau. 

Khi một phân vùng trống, tất cả các ô còn lại sẽ thuộc về một phân vùng bàn cờ duy nhất. Không có hai ô nào trong cùng một phân vùng liền kề nhau. Thay thế màu dành riêng bằng màu$3$khi cần thiết không thể tạo xung đột vì không có ô lân cận trong phân vùng đó. 

Mỗi nước đi sử dụng một màu khác với lựa chọn của Alice và mỗi ô được sử dụng đúng một lần. Do đó bảng được lấp đầy hoàn toàn mà không bao giờ tạo ra các ô liền kề có màu bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    alice = list(map(int, input().split()))

    p1 = []
    p2 = []

    for i in range(n):
        for j in range(n):
            if (i + j) % 2 == 0:
                p1.append((i + 1, j + 1))
            else:
                p2.append((i + 1, j + 1))

    ans = []

    for a in alice:
        if p1 and p2:
            if a == 1:
                x, y = p2.pop()
                ans.append((2, x, y))
            elif a == 2:
                x, y = p1.pop()
                ans.append((1, x, y))
            else:
                x, y = p1.pop()
                ans.append((1, x, y))

        elif p1:
            x, y = p1.pop()

            if a == 1:
                ans.append((3, x, y))
            else:
                ans.append((1, x, y))

        else:
            x, y = p2.pop()

            if a == 2:
                ans.append((3, x, y))
            else:
                ans.append((2, x, y))

    sys.stdout.write(
        "\n".join(f"{c} {i} {j}" for c, i, j in ans)
    )

solve()
```Hai mảng`p1`Và`p2`lưu trữ các ô chưa sử dụng của hai phân vùng bàn cờ. Xóa một ô bằng`pop()`là$O(1)$, vì vậy mỗi lần di chuyển đều cần phải thực hiện liên tục. 

Nhánh đầu tiên xử lý giai đoạn chính trong đó cả hai phân vùng vẫn chứa các ô. Sự lựa chọn của Alice xác định màu dành riêng nào còn lại. Chúng tôi sử dụng ngay một ô từ phân vùng tương ứng. 

Nhánh thứ hai và thứ ba xử lý phần cuối của trò chơi. Chỉ còn lại một phân vùng, vì vậy chúng tôi sử dụng màu dành riêng cho nó bất cứ khi nào có thể. Nếu Alice chặn màu đó thì chúng ta chuyển sang màu`3`. 

Việc triển khai lưu trữ tọa độ bằng cách sử dụng chỉ mục dựa trên một vì kết quả đầu ra của vấn đề mong đợi các hàng và cột được đánh số từ`1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
Alice = [1, 2, 1, 3]
```Phân vùng ban đầu: 

| Phân vùng | Tế bào | 
| --- | --- | 
| p1 | (1,1), (2,2) | 
| p2 | (1,2), (2,1) | 

| Xoay | Alice | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | lấy từ p2, sử dụng | |
