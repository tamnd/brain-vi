---
title: "CF 1198E - Tranh chữ nhật 2"
description: "Chúng ta được cấp một lưới $n nhân n$ cực lớn, nhưng thay vì liệt kê các ô đen riêng lẻ, đầu vào mô tả các vùng màu đen dưới dạng hình chữ nhật thẳng hàng với tối đa 50 trục. Mọi ô bên ngoài những hình chữ nhật này ban đầu có màu trắng và bên trong chúng có màu đen."
date: "2026-06-13T14:47:37+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graph-matchings", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1198
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 576 (Div. 1)"
rating: 2500
weight: 1198
solve_time_s: 254
verified: false
draft: false
---

[CF 1198E - Tranh hình chữ nhật 2](https://codeforces.com/problemset/problem/1198/E) 

**Đánh giá:** 2500 
**Tags:** luồng, đối sánh biểu đồ, đồ thị 
**Thời gian giải:** 4 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một khoản tiền cực kỳ lớn$n \times n$lưới, nhưng thay vì liệt kê các ô đen riêng lẻ, đầu vào mô tả các vùng màu đen dưới dạng hình chữ nhật thẳng hàng với 50 trục. Mọi ô bên ngoài những hình chữ nhật này ban đầu có màu trắng và bên trong chúng có màu đen. 

Nhiệm vụ là sơn lại lưới sao cho mọi ô đều có màu trắng. Thao tác duy nhất được phép là chọn bất kỳ hình chữ nhật thẳng hàng với trục nào và sơn nó màu trắng. Chi phí của một thao tác chỉ phụ thuộc vào kích thước hình chữ nhật: đối với hình chữ nhật có chiều cao$h$và chiều rộng$w$, chi phí là$\min(h, w)$. Cho phép thực hiện nhiều thao tác và hình chữ nhật có thể chồng lên nhau. 

Mục tiêu là bao phủ tất cả các ô màu đen ban đầu bằng cách sử dụng một tập hợp các thao tác vẽ như vậy với tổng chi phí tối thiểu. Các ô màu trắng không bao giờ quan trọng vì việc sơn lại chúng một lần nữa là vô hại, vì vậy vấn đề hoàn toàn nằm ở việc che phủ sự kết hợp của các hình chữ nhật màu đen đã cho một cách hiệu quả. 

Khó khăn chính đến từ quy mô. Kích thước lưới$n$có thể lớn như$10^9$, vì vậy chúng tôi không thể làm việc trên từng ô riêng lẻ. Tất cả cấu trúc phải đến từ tối đa 50 hình chữ nhật đầu vào. 

Một cách giải thích ngây thơ sẽ cố gắng mô phỏng các quyết định vẽ trên toàn bộ lưới hoặc xem xét tất cả các hình chữ nhật có thể có. Điều đó là không thể vì số hình chữ nhật có thể có là$O(n^4)$và thậm chí việc giới hạn điểm cuối ở ranh giới hình chữ nhật màu đen vẫn sẽ quá lớn nếu không có cấu trúc bổ sung. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ tham lam ngây thơ là khi các hình chữ nhật chồng lên nhau theo cách khiến các quyết định cục bộ trở nên sai lầm. Ví dụ: hai hình chữ nhật màu đen giao nhau:```
n = 10
(1,1)-(6,6)
(4,4)-(10,10)
```Một cách tiếp cận tham lam có thể vẽ từng hình chữ nhật riêng biệt với chi phí là 5 và 6, cho kết quả là 11. Nhưng một giải pháp tối ưu có thể chia vùng liên thành các hình chữ nhật con có cấu trúc và tái sử dụng các vết cắt rẻ hơn, có khả năng giảm đáng kể chi phí bằng cách khai thác các ranh giới chung. Điều này cho thấy rằng các quyết định phải mang tính tổng quát về hình học chứ không phải trên mỗi hình chữ nhật đầu vào. 

Một trường hợp tinh tế khác là khi một hình chữ nhật lớn gần như trống rỗng ngoại trừ một dải ô màu đen mỏng. Vẽ toàn bộ khung giới hạn rất tốn kém do min(h,w), nhưng việc chia nhỏ xung quanh các vùng trống có thể giảm đáng kể chi phí. Điều này một lần nữa gợi ý rằng chúng ta cần phân rã cấu trúc hơn là bao phủ trực tiếp. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xem xét mọi hình chữ nhật có thể có trong lưới và quyết định có nên sơn nó hay không, đảm bảo tất cả các ô màu đen đều được che phủ. Điều này đúng về mặt khái niệm: chúng ta đang chọn một tập hợp các hình chữ nhật có liên kết bao phủ tất cả các ô màu đen. Tuy nhiên, ngay cả khi chúng ta hạn chế ranh giới hình chữ nhật ở tọa độ đầu vào, vẫn có$O(k^4)$hình chữ nhật ứng viên, ở đâu$k$là số ranh giới x và y duy nhất, lên tới khoảng 100. Điều đó mang lại khoảng$10^8$hình chữ nhật, và đối với mỗi hình chữ nhật, chúng ta cần phải giải thích về sự chồng chéo và phạm vi bao phủ, điều này nhanh chóng trở nên không khả thi. 

Cái nhìn sâu sắc về cấu trúc quan trọng là chỉ có ranh giới của các hình chữ nhật nhất định mới quan trọng. Bất kỳ giải pháp tối ưu nào cũng có thể được giả định là căn chỉnh tất cả các cạnh hình chữ nhật với tọa độ x và y xuất hiện trong đầu vào. Điều này làm giảm lưới vô hạn thành lưới nén tối đa$2m$tọa độ x riêng biệt và$2m$tọa độ y rõ rệt. 

Sau khi lưới được nén, chúng tôi xem xét mọi hình chữ nhật con trong hệ tọa độ rút gọn này. Đối với mỗi hình chữ nhật con, chúng tôi tính toán xem nó có chứa bất kỳ ô màu đen nào hay không bằng cách sử dụng tổng tiền tố 2D. Nếu nó không chứa ô đen thì chi phí của nó bằng 0 vì nó không cần vùng phủ sóng. Mặt khác, chúng ta có ba lựa chọn có ý nghĩa: chúng ta có thể vẽ toàn bộ hình chữ nhật con với chi phí$\min(h,w)$, hoặc chia theo chiều ngang thành hai hình chữ nhật con nhỏ hơn hoặc chia theo chiều dọc. Mỗi phép chia sẽ rút gọn bài toán thành các bài toán con độc lập. 

Điều này tự nhiên dẫn đến một công thức lập trình động theo khoảng trên các phân đoạn 2D, trong đó mỗi trạng thái biểu thị một hình chữ nhật được nén và các chuyển tiếp tương ứng với các vết cắt ngang hoặc dọc. Cấu trúc chi phí đảm bảo cấu trúc nền tối ưu vì các quyết định sơn trên các vùng rời rạc không tương tác với nhau. 

Lý do quan trọng khiến điều này hoạt động là vì hàm chi phí chỉ phụ thuộc vào hình chữ nhật chứ không phải vị trí hoặc nội dung và sự phân chia duy trì tính độc lập giữa các bài toán con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các hình chữ nhật |$O(k^4)$hoặc tệ hơn |$O(k^2)$| Quá chậm | 
| Nén tọa độ + DP |$O(k^4)$với nhỏ$k \le 100$|$O(k^4)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trích xuất tất cả tọa độ x và tọa độ y từ các góc hình chữ nhật và sắp xếp chúng, loại bỏ các tọa độ trùng lặp. Điều này xác định các trục tọa độ nén. Lý do điều này có tác dụng là vì bất kỳ đường cắt hoặc ranh giới có ý nghĩa nào trong một giải pháp tối ưu đều phải căn chỉnh với cạnh hình chữ nhật đầu vào, vì không có cấu trúc chi phí nào khuyến khích các vị trí tùy ý. 
2. Xây dựng một lưới nén trong đó mỗi ô tương ứng với một vùng nằm giữa tọa độ x và y liên tiếp. Đánh dấu ô nén nào có màu đen bằng cách sử dụng mảng chênh lệch 2D hoặc đánh dấu trực tiếp từ hình chữ nhật đầu vào. 
3. Xây dựng tổng tiền tố 2D trên lưới nén để chúng ta có thể kiểm tra theo thời gian liên tục xem có bất kỳ hình chữ nhật con nào chứa ít nhất một ô đen hay không. 
4. Xác định trạng thái DP$dp[x1][x2][y1][y2]$là chi phí tối thiểu để vẽ tất cả các ô màu đen bên trong hình chữ nhật con được xác định bởi tọa độ nén. Điều này thể hiện ý tưởng rằng chúng ta giải quyết độc lập mọi bài toán con hình học. 
5. Nếu hình chữ nhật con không chứa ô đen, hãy đặt$dp = 0$. Đây là trường hợp cơ bản vì không cần thực hiện thao tác nào. 
6. Ngược lại, hãy tính chi phí để vẽ toàn bộ hình chữ nhật con cùng một lúc, bằng$\min(\text{width}, \text{height})$. Điều này thể hiện chiến lược bao gồm mọi thứ trong một hoạt động. 
7. Hãy thử chia theo chiều ngang giữa$y1$Và$y2$, chia hình chữ nhật thành hai hình chữ nhật nhỏ hơn và kết hợp các giá trị DP của chúng. Điều này nắm bắt được khả năng tách các dải ô đen nằm ngang độc lập. 
8. Hãy thử tất cả các cách chia dọc giữa$x1$Và$x2$, tương tự kết hợp các kết quả từ các hình chữ nhật con bên trái và bên phải. Điều này nắm bắt sự phân hủy theo chiều dọc. 
9. Lấy mức tối thiểu để sơn toàn bộ hình chữ nhật và tất cả các phần có thể phân chia. Điều này đảm bảo chúng tôi xem xét cả phạm vi bao phủ toàn cầu và phân rã đệ quy. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là bất kỳ chiến lược vẽ hợp lệ nào cũng tạo ra sự phân chia mặt phẳng thành các vùng được bao phủ bởi các thao tác riêng lẻ. Nếu một thao tác đi qua một đường phân chia tiềm năng, thì đường đó có thể được chọn từ tập hợp tọa độ hữu hạn mà không mất tính tổng quát, vì các ranh giới hình chữ nhật trong đầu vào xác định tất cả các điểm tương tác có ý nghĩa. 

Điều này có nghĩa là mọi giải pháp tối ưu đều có thể được chuyển đổi thành một giải pháp trong đó việc cắt chỉ xảy ra dọc theo các đường lưới nén và mỗi hình chữ nhật con được giải quyết một cách độc lập. DP liệt kê chính xác các phân tách này, do đó nó phải đạt được chi phí tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    rects = []
    xs = set()
    ys = set()

    for _ in range(m):
        x1, y1, x2, y2 = map(int, input().split())
        rects.append((x1, y1, x2, y2))
        xs.add(x1); xs.add(x2)
        ys.add(y1); ys.add(y2)

    xs = sorted(xs)
    ys = sorted(ys)

    xid = {x:i for i, x in enumerate(xs)}
    yid = {y:i
```
