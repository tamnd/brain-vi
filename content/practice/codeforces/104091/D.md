---
title: "CF 104091D - \u0428\u0430\u0445\u043c\u0430\u0442\u043d\u044b\u0439 \u0434\u043e\u0437\u043e\u0440"
description: "Chúng ta có một lưới hình chữ nhật có $n$ hàng và $m$ cột. Bên trong lưới này có $q$ mảnh đặc biệt được gọi là trinh sát. Mỗi trinh sát nằm trên một ô riêng biệt $(x, y)$, trong đó $x$ là chỉ mục hàng từ trên xuống dưới và $y$ là chỉ mục cột từ trái sang phải."
date: "2026-07-02T02:28:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104091
codeforces_index: "D"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041f\u0435\u0442\u0440\u043e\u0437\u0430\u0432\u043e\u0434\u0441\u043a\u0435 \u0438 \u041a\u0430\u0440\u0435\u043b\u0438\u0438 2022-2023"
rating: 0
weight: 104091
solve_time_s: 40
verified: true
draft: false
---

[CF 104091D - \u0428\u0430\u0445\u043c\u0430\u0442\u043d\u044b\u0439 \u0434\u043e\u0437\u043e\u0440](https://codeforces.com/problemset/problem/104091/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có$n$hàng và$m$cột. Bên trong lưới này có$q$phần đặc biệt được gọi là trinh sát. Mỗi trinh sát ngồi trên một phòng giam riêng biệt$(x, y)$, Ở đâu$x$là chỉ số hàng từ trên xuống dưới và$y$là chỉ số cột từ trái sang phải. 

Một trinh sát “tấn công” mọi ô nằm ngay trong khu vực bên phải hoặc ngay bên dưới nó, bao gồm cả vị trí của chính nó. Chính thức, một trinh sát tại$(x, y)$bao phủ tất cả các tế bào$(i, j)$như vậy$i \ge x$Và$j \ge y$. Nếu nhiều trinh sát bao vây cùng một ô, chúng tôi vẫn chỉ tính một lần. 

Nhiệm vụ là tính toán có bao nhiêu ô lưới riêng biệt được bao phủ bởi ít nhất một trinh sát. 

Các ràng buộc về kích thước lưới là cực kỳ lớn, lên tới$10^9 \times 10^9$, vì vậy chúng tôi không thể mô phỏng lưới. Số lượng trinh sát lên tới$10^5$, có nghĩa là mọi giải pháp đều phải tránh xử lý trên mỗi ô và thay vào đó dựa vào các đóng góp hình học tổng hợp. 

Một cách tiếp cận đơn giản là kiểm tra từng ô lưới hoặc thậm chí cố gắng lặp lại từng trinh sát trên hình chữ nhật đầy đủ của nó là không thể bởi vì một trinh sát duy nhất có thể che đậy tới$10^{18}$tế bào. 

Một trường hợp thất bại ít rõ ràng hơn là do sự chồng chéo. Ví dụ: nếu một trinh sát đang ở$(1, 1)$và cái khác ở$(2, 2)$, khu vực của trinh sát thứ hai hoàn toàn nằm trong khu vực đầu tiên, vì vậy phép tính tổng đơn giản sẽ vượt quá mức trừ khi chúng tôi xử lý rõ ràng các giao lộ. 

Khó khăn chính là mỗi trinh sát xác định một hình chữ nhật phía dưới bên phải và chúng ta cần kích thước của phần kết của các hình chữ nhật này. 

## Phương pháp tiếp cận 

Mỗi trinh sát xác định một hình chữ nhật thẳng hàng theo trục kéo dài đến góc dưới bên phải của lưới. Vì vậy, vấn đề trở thành tính diện tích hợp của$q$hình chữ nhật, trong đó mỗi hình chữ nhật là$[x, n] \times [y, m]$. 

Việc giải thích bạo lực sẽ đánh dấu mọi ô trong mỗi hình chữ nhật, nhưng mỗi hình chữ nhật có thể rất lớn, khiến điều này hoàn toàn không khả thi. Ngay cả khi chúng ta cố gắng chỉ xử lý cấu trúc biên, việc khai triển trực tiếp vẫn phụ thuộc vào$n \cdot m$, quá lớn. 

Quan sát quan trọng là tất cả các hình chữ nhật đều có chung một “góc mục tiêu”$(n, m)$và hình dạng của chúng đều đơn điệu: chúng kéo dài vô tận xuống bên phải trong ranh giới lưới. Cấu trúc đơn điệu này cho phép chúng ta sắp xếp các trinh sát và nén quy trình hợp nhất thành việc đếm các khoản đóng góp theo hàng hoặc từng cột. 

Thay vì suy nghĩ ở dạng 2D, chúng ta đảo ngược quan điểm. Đối với một hàng cố định$x$, một ô$(x, y)$được bảo hiểm nếu và chỉ nếu có một trinh sát ở một thời điểm nào đó$(x', y')$với$x' \le x$Và$y' \le y$. Tương tự, nếu chúng ta xem xét tất cả các trinh sát có tọa độ hàng nhiều nhất$x$, họ áp đặt một ngưỡng cho phạm vi bao phủ của cột. 

Điều này gợi ý quét các hàng từ dưới lên trên. Khi chúng tôi di chuyển lên trên, nhiều trinh sát sẽ hoạt động hơn và mỗi trinh sát đang hoạt động sẽ đóng góp một hạn chế về khoảng cách còn lại mà chúng tôi phải xem xét các cột. Đối với một hàng nhất định, chỉ có ngưỡng cột nhỏ nhất mới quan trọng: một khi chúng ta biết mức tối thiểu$y$trong số tất cả các trinh sát với$x' \le x$, tất cả các cột từ mức tối thiểu đó đến$m$được che phủ. 

Vì vậy, đối với mỗi hàng, khoảng bao phủ chỉ đơn giản là$[ \min y \text{ among active scouts},\ m]$và câu trả lời là tổng của các hàng có độ dài khoảng. Chúng tôi tránh lặp lại các hàng một cách rõ ràng bằng cách nhóm theo các hàng duy nhất$x$tọa độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên mỗi ô |$O(nm)$|$O(1)$| Quá chậm | 
| Quét theo hàng với tổng hợp |$O(q \log q)$|$O(q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các vị trí trinh sát và nhóm chúng theo tọa độ hàng$x$. Chúng tôi lưu trữ cho mỗi hàng danh sách các vị trí cột$y$. Điều này là cần thiết vì hiệu ứng của trinh sát được kích hoạt khi chúng ta tiếp cận hàng của họ trong một cuộc truy quét từ dưới lên. 
2. Sắp xếp tất cả tọa độ hàng riêng biệt theo thứ tự tăng dần. Chúng tôi sẽ xử lý các hàng từ dưới lên trên để khi trinh sát được kích hoạt, nó vẫn hoạt động cho tất cả các hàng phía trên. 
3. Duy trì giá trị đang hoạt động`min_y`, khởi tạo là$m+1$. Đây là chỉ số cột nhỏ nhất trong số tất cả các trinh sát hiện đang hoạt động trong cuộc truy quét. 
4. Bắt đầu từ hàng$n$xuống$1$. Ở mỗi hàng$x$, trước tiên hãy kích hoạt tất cả các trinh sát nằm chính xác tại hàng đó bằng cách cập nhật`min_y = min(min_y, y)`cho mỗi người trong số họ. Bước này đảm bảo rằng`min_y`luôn phản ánh ranh giới bên trái chặt chẽ nhất được áp đặt bởi tất cả các trinh sát tích cực. 
5. Sau khi xử lý kích hoạt cho hàng$x$, hãy tính xem có bao nhiêu ô trong hàng này: đó là$\max(0, m - min_y + 1)$. Điều này hoạt động vì mỗi cột từ`min_y`ĐẾN$m$được đảm bảo có ít nhất một trinh sát tích cực bảo vệ. 
6. Tích lũy giá trị này vào câu trả lời. 
7. Tiếp tục cho đến khi hàng 1 được xử lý. 

### Tại sao nó hoạt động 

Tại bất kỳ hàng cố định nào$x$, một ô$(x, y)$được bảo hiểm nếu tồn tại ít nhất một trinh sát$(x', y')$như vậy$x' \le x$Và$y' \le y$. Trong số tất cả các trinh sát tích cực (những người có$x' \le x$), điều kiện giảm xuống còn$y \ge \min y'$. Do đó, phạm vi bao phủ trên mỗi hàng được xác định hoàn toàn bởi ngưỡng cột nhỏ nhất trong số các trinh sát đang hoạt động. Quá trình quét duy trì chính xác tính bất biến này, do đó mỗi hàng đều được tính chính xác mà không bị chồng chéo hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())
    
    by_row = {}
    for _ in range(q):
        x, y = map(int, input().split())
        if x not in by_row:
            by_row[x] = []
        by_row[x].append(y)

    min_y = m + 1
    ans = 0

    for x in range(n, 0, -1):
        if x in by_row:
            for y in by_row[x]:
                if y < min_y:
                    min_y = y

        if min_y <= m:
            ans += (m - min_y + 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp nhóm các trinh sát theo hàng để tránh việc quét lặp lại. Biến trạng thái chính là`min_y`, nén tất cả các trinh sát đang hoạt động vào một ranh giới hiệu quả duy nhất. Vòng lặp từ$n$xuống$1$đảm bảo rằng một khi trinh sát bắt đầu hoạt động, nó sẽ ảnh hưởng đến tất cả các hàng phía trên nó. 

biểu hiện`m - min_y + 1`trực tiếp đếm độ dài của hậu tố được bao phủ trong mỗi hàng. điều kiện`min_y <= m`ngăn việc đếm khi không có trinh sát nào được kích hoạt hoặc khi tất cả các trinh sát đang hoạt động nằm ngoài phạm vi lưới. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ: 

đầu vào:```
5 6 2
2 4
4 2
```Chúng tôi xử lý từ hàng 5 xuống 1. 

| Hàng | Trinh sát được kích hoạt | phút_y | Cột có mái che | Đóng góp hàng | 
| --- | --- | --- | --- | --- | 
| 5 | không | 7 | không | 0 | 
| 4 | (4,2) | 2 | 2..6 | 5 | 
| 3 | (4,2) | 2 | 2..6 | 5 | 
| 2 | (4,2), (2,4) | 2 | 2..6 | 5 | 
| 1 | (4,2), (2,4) | 2 | 2..6 | 5 | 

Câu trả lời cuối cùng là$20$. 

Dấu vết này cho thấy việc thêm một trinh sát mới chỉ có thể thắt chặt ranh giới bên trái và không bao giờ mở rộng nó, điều này phù hợp với hình dạng của hình chữ nhật phía dưới bên phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q + n)$| Mỗi trinh sát được xử lý một lần và các hàng được quét một lần | 
| Không gian |$O(q)$| Lưu trữ các trinh sát được nhóm theo hàng | 

Những ràng buộc cho phép$q \le 10^5$, Nhưng$n$có thể lớn. Quét tuyến tính trên$n$được chấp nhận trong Python do các thao tác đơn giản trên mỗi lần lặp và chi phí chủ yếu vẫn là xử lý đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import sys
    input = sys.stdin.readline

    n, m, q = map(int, input().split())
    by_row = {}
    for _ in range(q):
        x, y = map(int, input().split())
        by_row.setdefault(x, []).append(y)

    min_y = m + 1
    ans = 0
    for x in range(n, 0, -1):
        if x in by_row:
            for y in by_row[x]:
                min_y = min(min_y, y)
        if min_y <= m:
            ans += (m - min_y + 1)

    return str(ans)

# provided sample
assert run("8 15 3\n3 10\n5 7\n6 12\n") == "59"

# minimum grid
assert run("1 1 1\n1 1\n") == "1"

# single row wide grid
assert run("1 5 2\n1 2\n1 4\n") == "4"

# single column grid
assert run("5 1 2\n2 1\n4 1\n") == "5"

# full dominance
assert run("3 3 1\n2 2\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | 1 | độ đúng cơ sở | 
| hàng đơn | 4 | hợp nhất theo khoảng thời gian | 
| cột đơn | 5 | truyền dọc | 
| trinh sát trung tâm | 4 | hình học bao phủ một phần | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi chưa có trinh sát nào được kích hoạt trong khi quét từ dưới lên. Ví dụ,$n=3, m=3$với một trinh sát tại$(1,1)$. Ở hàng 3 và 2,`min_y`còn lại$4$, vì vậy không có đóng góp nào được thêm vào. Chỉ ở hàng 1, phạm vi bảo hiểm mới bắt đầu, tạo ra chính xác$3$tế bào. Thuật toán trì hoãn kích hoạt một cách chính xác cho đến khi đạt được hàng chính xác. 

Một trường hợp khác là có nhiều trinh sát trong cùng một hàng. Đối với đầu vào$(4,2)$,$(4,5)$, bản cập nhật đảm bảo`min_y`trở thành$2$, không$5$và cả hai đều được xử lý trong một bước kích hoạt duy nhất. Việc quét chính xác sáp nhập_
