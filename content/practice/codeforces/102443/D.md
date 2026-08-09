---
title: "CF 102443D - Đoán đường đi"
description: "Chúng tôi có một lưới (mtimes n). Một đường đi đơn điệu ẩn bắt đầu tại ((1,1)), kết thúc tại ((m,n)) và chỉ sử dụng các bước di chuyển xuống và sang phải. Mỗi ô của con đường ẩn đó đều chứa một máy dò. Chúng tôi có thể gửi một đường dẫn đơn điệu của riêng mình dưới dạng truy vấn."
date: "2026-08-08T12:51:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "D"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 313
verified: true
draft: false
---

[CF 102443D - Đoán đường](https://codeforces.com/problemset/problem/102443/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Tuyên bố vấn đề 

Chúng ta có một lưới (m\times n). Một đường đi đơn điệu ẩn bắt đầu tại ((1,1)), kết thúc tại ((m,n)) và chỉ sử dụng các bước di chuyển xuống và sang phải. Mỗi ô của con đường ẩn đó đều chứa một máy dò. 

Chúng tôi có thể gửi một đường dẫn đơn điệu của riêng mình dưới dạng truy vấn. Trình tương tác trả về mọi ô dò tìm mà đường dẫn truy vấn của chúng tôi cũng truy cập. Do đó, một truy vấn sẽ cho chúng ta giao điểm giữa đường dẫn đã chọn và đường dẫn ẩn. 

Chúng tôi có tối đa 10 truy vấn. Sau khi thu thập đủ thông tin, chúng ta phải xuất ra đường dẫn ẩn chính xác dưới dạng một chuỗi`D`Và`R`di chuyển. Giới hạn chính thức là (1\le m,n\le1000), (m+n>2), với giới hạn thời gian một giây và bộ nhớ 512 MB. 

## Đầu vào 

Dòng đầu tiên chứa (m) và (n). Sau đó, chương trình giao tiếp với người tương tác. Đối với mỗi truy vấn, nó in`?`theo sau là một chuỗi đường dẫn hợp lệ. Đầu tiên, trình tương tác trả về số lượng ô phát hiện được báo cáo, sau đó là tọa độ của chúng, được sắp xếp theo hàng rồi theo cột. 

## Đầu ra 

Khi đường dẫn ẩn đã được xác định, hãy in`!`theo sau là trình tự của nó`D`Và`R`di chuyển. Mỗi truy vấn và câu trả lời cuối cùng phải được theo sau bởi một dòng mới và đầu ra tương tác phải được xóa. của Python`print`đã xả nước khi sử dụng với`flush=True`. 

## Hiểu vấn đề 

Cách hữu ích để nghĩ về trình tương tác là nó cung cấp cho chúng ta các điểm giao nhau chính xác chứ không chỉ đơn thuần là câu trả lời có hoặc không. Nếu truy vấn của chúng ta đi qua một ô của đường dẫn ẩn, ô đó sẽ xuất hiện trong phản hồi. 

Ban đầu chỉ có ((1,1)) và ((m,n)) được biết. Giả sử hai ô đã biết liên tiếp trên đường dẫn ẩn là (A=(r_1,c_1)) và (B=(r_2,c_2)). Chúng ta biết đường đi ẩn giữa chúng nằm trong hình chữ nhật được xác định bởi hai điểm này, nhưng chúng ta không biết các ngã rẽ của nó. 

Điều quan trọng là phân chia mọi phần chưa biết như vậy ở hàng giữa của nó. hãy để 

[ 
r_{\text{mid}}=\left\lfloor\frac{r_1+r_2}{2}\right\rfloor. 
] 

Chúng ta tạo một truy vấn đầu tiên đi xuống hàng (r_{\text{mid}}), sau đó đi ngang qua toàn bộ hình chữ nhật và cuối cùng đi xuống (B). Bất kỳ đường dẫn ẩn đơn điệu nào từ (A) đến (B) đều phải truy cập vào hàng (r_{\text{mid}}) và ô của nó trên hàng đó nằm ở đâu đó giữa các cột (c_1) và (c_2). Phần ngang của chúng tôi bao phủ mọi ô trong số đó, vì vậy hai đường dẫn phải giao nhau ở đó. 

Do đó, mọi phần chưa biết sẽ được chia thành các phần nhỏ hơn có hiệu số hàng tối đa bằng một nửa hiệu số hàng trước đó. Bởi vì (m\le1000), mười nửa là đủ. Đây là cấu trúc tìm kiếm nhị phân đằng sau giải pháp. Một mô tả độc lập ngắn gọn về cùng một ý tưởng được đưa ra trong bản viết giải pháp cuộc thi, trong đó mô tả truy vấn dưới dạng lộ trình gồm năm hình và nhận thấy rằng mỗi truy vấn sẽ giảm một nửa phạm vi còn lại. 

Có một sự tinh tế. Biết điểm giao nhau ở hàng giữa không nhất thiết chỉ cần kết nối các điểm mới bằng các bước di chuyển tùy ý là đủ. Bản thân truy vấn cuối cùng cho chúng ta biết cách giải quyết các khoảng trống một hàng còn lại. Nếu hai điểm được báo cáo liên tiếp khác nhau đúng một hàng thì phần truy vấn tương ứng của chúng tôi chứa chính xác một lần di chuyển xuống. Nếu truy vấn của chúng tôi bắt đầu phần đó bằng`D`, thay vào đó, đường dẫn ẩn phải bắt đầu bằng các bước di chuyển đúng cần thiết và đi xuống ở cuối, trừ khi hai đường dẫn đó trùng nhau. Nếu truy vấn bắt đầu bằng`R`, sự sắp xếp ngược lại là bắt buộc. Đây là lý do tại sao đường dẫn truy vấn phải được giữ lại thay vì chỉ lưu trữ tọa độ được trả về. 

Ví dụ, hãy xem xét một phần (2\times2) từ ((1,1)) đến ((2,2)). Nếu chúng ta truy vấn`DR`, thì con đường ẩn`DR`báo cáo ô trung gian ((2,1)). Nếu ô đó vắng mặt thì đường dẫn ẩn phải là`RD`. Sự khác biệt chính xác này cũng được nhấn mạnh trong một cuộc thảo luận giải pháp độc lập về vấn đề này. 

Việc thực hiện bất cẩn có thể thất bại khi (m=1). Không có hàng để tìm kiếm nhị phân trong trường hợp đó và đường dẫn duy nhất có thể là hoàn toàn nằm ngang. Ví dụ, với đầu vào```
1 4
```con đường cuối cùng đúng là`RRR`. Cố gắng thực hiện thủ tục điểm giữa mà không có trường hợp đặc biệt này có thể tạo ra một truy vấn trống hoặc không hợp lệ. 

Một trường hợp cạnh khác là (n=1). Con đường duy nhất có thể là hoàn toàn thẳng đứng. Vì```
4 1
```câu trả lời là`DDD`. Cấu trúc giả định mọi phần đều chứa một bước di chuyển đúng có thể tạo ra một truy vấn không hợp lệ một cách không chính xác. 

Trường hợp cạnh tinh tế hơn xảy ra khi hai điểm đã biết liên tiếp nằm cạnh nhau theo đường chéo. Ví dụ: giữa ((1,3)) và ((2,4)), đường dẫn ẩn là`DR`hoặc`RD`. Chỉ biết hai điểm cuối là không đủ. Bản thân phân đoạn truy vấn phải được kiểm tra vì phản hồi giao nhau của nó phân biệt hai khả năng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét mọi con đường đơn điệu có thể. có 

[ 
\binom{m+n-2}{m-1} 
] 

những đường đi như vậy, vì chúng ta chọn (m-1) trong số (m+n-2) nước đi nào là nước đi xuống. Một trình giải quyết mạnh mẽ có thể giữ mọi đường dẫn ứng viên và loại bỏ các ứng cử viên không nhất quán với từng câu trả lời truy vấn. Kiểm tra chi phí của một ứng cử viên (O(m+n)), do đó công việc trong trường hợp xấu nhất là 

[ 
\Theta\left((m+n)\binom{m+n-2}{m-1}\right). 
] 

Đối với (m=n=1000), đây là khoảng (1998\binom{1998}{999}), theo thứ tự (10^{603}) kiểm tra ô đường dẫn cơ bản. Giới hạn một giây khiến điều này hoàn toàn không thể thực hiện được. 

Lực lượng vũ phu hoạt động về mặt khái niệm vì mọi phản hồi đều cung cấp đủ thông tin để từ chối các đường dẫn không chứa các ô được báo cáo. Vấn đề là số lượng đường dẫn có thể có là theo cấp số nhân, trong khi người tương tác chỉ cho chúng ta mười cơ hội để đặt câu hỏi. 

Quan sát hữu ích là chúng ta không cần phải phân biệt mọi con đường hoàn chỉnh cùng một lúc. Chúng ta có thể duy trì các ô được xác nhận liên tiếp và phân chia đồng thời mọi phần không chắc chắn. Đối với mỗi phần, truy vấn gồm năm hình sẽ đi xuống hàng giữa của nó, di chuyển qua phần đó rồi đi xuống điểm cuối của nó. Mọi đường đi ẩn đều phải đi qua đoạn giữa nằm ngang đó. Do đó, một truy vấn sẽ cung cấp một điểm xác nhận mới cho mọi phần đủ lớn cùng một lúc. 

Khoảng cách hàng của mỗi phần kết quả tối đa bằng một nửa khoảng cách hàng cũ, có thể làm tròn. Vì khoảng cách hàng tối đa chỉ là (999) nên mười truy vấn là đủ. Các phần một hàng cuối cùng được giải quyết trực tiếp từ hình dạng của truy vấn cuối cùng và các giao điểm được báo cáo của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((m+n)\binom{m+n-2}{m-1})) | Hàm mũ | Quá chậm | 
| Tối ưu | (O(10(m+n))) | (O(m+n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (m) và (n). Nếu (m=1), đường dẫn buộc phải`R`lặp lại (n-1) lần. Nếu (n=1) thì buộc phải`D`lặp lại (m-1) lần. Những trường hợp này không cần truy vấn. 
2. Mặt khác, khởi tạo các điểm được xác nhận chỉ với ((1,1)) và ((m,n)). Họ được đảm bảo thuộc về con đường ẩn. 
3. Với mỗi cặp điểm được xác nhận liên tiếp (A=(r_1,c_1)) và (B=(r_2,c_2)), hãy tính (r_{\text{mid}}=\lfloor(r_1+r_2)/2\rfloor). Xây dựng phân đoạn truy vấn dưới dạng`D`lặp lại (r_{\text{mid}}-r_1) lần, tiếp theo là`R`lặp lại (c_2-c_1) lần, tiếp theo là`D`lặp lại (r_2-r_{\text{mid}}) lần. Việc ghép các phân đoạn này sẽ tạo ra một đường dẫn hợp lệ hoàn chỉnh từ ((1,1)) đến ((m,n)). 
4. Gửi đường dẫn này dưới dạng truy vấn và đọc tất cả tọa độ máy dò được báo cáo. Thay thế chuỗi điểm được xác nhận hiện tại bằng các tọa độ được báo cáo này. Chúng đã được sắp xếp theo thứ tự đường dẫn vì cả hai đường dẫn đều đơn điệu và trình tương tác báo cáo tọa độ theo hàng rồi đến cột. 
5. Kiểm tra sự khác biệt hàng giữa mỗi cặp điểm được báo cáo liên tiếp. Nếu mỗi sự khác biệt như vậy nhiều nhất là một thì hãy ngừng truy vấn. Nếu không, hãy lặp lại việc xây dựng với các điểm mới được xác nhận. 
6. Để biết tại sao số lượng truy vấn bị giới hạn, hãy xem xét một phần cũ có độ lệch hàng (d). Truy vấn chứa toàn bộ hàng giữa của phần đó và đường dẫn ẩn phải đáp ứng nó. Điểm được báo cáo mới trên hàng đó chia phần thành các phần có chênh lệch hàng nhiều nhất (\lceil d/2\rceil). Do đó, sau mười truy vấn, chênh lệch ban đầu tối đa (999) đã trở thành nhiều nhất là một. 
7. Giữ chuỗi truy vấn thực tế từ vòng cuối cùng. Xây dựng bản đồ tọa độ theo vị trí cho truy vấn đó để chúng tôi biết chính xác phần nào của truy vấn nằm giữa mỗi cặp ô được báo cáo liên tiếp. 
8. Nếu hai ô được báo cáo liên tiếp nằm trên cùng một hàng, đường dẫn ẩn giữa chúng buộc phải bao gồm toàn bộ`R`di chuyển. 
9. Nếu chúng khác nhau một hàng, hãy kiểm tra phần tương ứng của truy vấn cuối cùng. Nếu phần truy vấn đó bắt đầu bằng`D`, đường dẫn ẩn phải sử dụng thứ tự ngược lại,`R`di chuyển theo sau bởi`D`. Nếu phần truy vấn bắt đầu bằng`R`, đường dẫn ẩn phải là`D`theo sau là yêu cầu`R`di chuyển. Chỉ có một lần di chuyển xuống trong phần như vậy, vì vậy điều này xác định chính xác đường dẫn ẩn. 
10. Nối tất cả các phần được xây dựng lại và in đường dẫn kết quả với`!`. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi truy vấn, mỗi tọa độ được báo cáo là một ô chính xác của đường dẫn ẩn và các tọa độ được báo cáo liên tiếp sẽ phân chia các phần vẫn chưa xác định của đường dẫn ẩn. Đối với mỗi phần như vậy, truy vấn sẽ đi qua hàng giữa của nó một cách rõ ràng, do đó đường dẫn ẩn phải giao nhau với truy vấn ở đó. Do đó, mỗi phần mới có chiều cao tối đa bằng một nửa hàng trước đó. Khi chiều cao bằng 0 hoặc 1, đường dẫn còn lại bị bắt buộc hoặc được xác định duy nhất bằng cách xem đường dẫn ẩn có phù hợp với bước đi đầu tiên của truy vấn cuối cùng hay không. Do đó việc xây dựng lại không thể chọn một lượt sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    m, n = map(int, input().split())

    if m == 1:
        print("! " + "R" * (n - 1), flush=True)
        return

    if n == 1:
        print("! " + "D" * (m - 1), flush=True)
        return

    points = [(1, 1), (m, n)]
    last_query = None

    for _ in range(10):
        parts = []

        for (r1, c1), (r2, c2) in zip(points, points[1:]):
            mid = (r1 + r2) // 2

            parts.append("D" * (mid - r1))
            parts.append("R" * (c2 - c1))
            parts.append("D" * (r2 - mid))

        query = "".join(parts)
        last_query = query

        print("? " + query, flush=True)

        t = int(input())
        points = [
            tuple(map(int, input().split()))
            for _ in range(t)
        ]

        if all(
            points[i + 1][0] - points[i][0] <= 1
            for i in range(len(points) - 1)
        ):
            break

    # Map every cell of the final query to its position in the query.
    qpos = {}
    r, c = 1, 1
    qpos[(r, c)] = 0

    for i, move in enumerate(last_query, 1):
        if move == "D":
            r += 1
        else:
            c += 1
        qpos[(r, c)] = i

    answer = []

    for a, b in zip(points, points[1:]):
        r1, c1 = a
        r2, c2 = b

        if r1 == r2:
            answer.append("R" * (c2 - c1))
            continue

        # Their row difference is at most one.
        ia = qpos[a]
        ib = qpos[b]

        query_segment = last_query[ia:ib]

        if query_segment[0] == "D":
            # Query uses D followed by R's.
            # The hidden path must use R's followed by D.
            answer.append("R" * (len(query_segment) - 1))
            answer.append("D")
        else:
            # Query uses R's followed by D.
            # The hidden path must use D followed by R's.
            answer.append("D")
            answer.append("R" * (len(query_segment) - 1))

    print("! " + "".join(answer), flush=True)

if __name__ == "__main__":
    main()
```Hai trường hợp đặc biệt đầu tiên tránh được sự tương tác không cần thiết khi lưới chỉ có một hàng hoặc một cột. Trong cả hai trường hợp đều có chính xác một con đường có thể. 

Vòng lặp chính lưu trữ các ô dò được xác nhận trong`points`. Truy vấn được xây dựng độc lập cho mỗi cặp liên tiếp, sau đó tất cả các phần được nối với nhau. Mỗi phần bắt đầu tại một điểm được xác nhận và kết thúc ở điểm tiếp theo, vì vậy toàn bộ truy vấn là một đường dẫn hợp lệ từ góc trên bên trái đến góc dưới cùng bên phải. 

Điểm giữa sử dụng phép chia sàn số nguyên. Điều này là có chủ ý vì nó đảm bảo rằng cả hai khoảng cách hàng kết quả đều không lớn hơn (\lceil d/2\rceil). Với (d\le999), mười lần lặp là đủ. 

Phản hồi từ trình tương tác đã được sắp xếp nên không cần sắp xếp thêm. Điều này diễn ra trực tiếp từ giao thức tương tác, đảm bảo tăng hàng và tăng thứ tự cột trong một hàng. 

trận chung kết`qpos`bản đồ là phần tinh tế của việc thực hiện. Chúng tôi không thể xây dựng lại phần một hàng chỉ từ các điểm cuối của nó. Chúng ta cần biết liệu truy vấn cuối cùng có đi qua phần đó hay không`D...R`hoặc`R...D`. Vì mỗi ô của truy vấn có tọa độ duy nhất,`qpos`cho phép chúng tôi trích xuất phân đoạn truy vấn chính xác giữa hai ô được báo cáo. 

Không có vấn đề tràn số nguyên trong Python. Đường dẫn được tạo lớn nhất chỉ có (m+n-2\le1998) di chuyển, vì vậy tất cả các chuỗi và bộ sưu tập tọa độ đều rất nhỏ. 

Sự tương tác phải được xóa sau mỗi truy vấn và sau câu trả lời cuối cùng. sử dụng`print(..., flush=True)`xử lý việc này trực tiếp, theo yêu cầu của tuyên bố. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu chính thức sử dụng lưới (3\times4) và đường dẫn ẩn là`RDRDR`. Tương tác chính thức sử dụng các truy vấn hợp lệ khác nhau, nhưng chúng tôi có thể theo dõi chiến lược điểm giữa từ bài xã luận này. Câu lệnh xác nhận rằng đường dẫn ẩn trong mẫu là`RDRDR`. 

Các điểm được xác nhận ban đầu là ((1,1)) và ((3,4)). Hàng trung điểm là (2), vì vậy truy vấn đầu tiên là`DRRRD`. 

| Bước | Điểm được xác nhận | Truy vấn | Điểm báo cáo | 
| --- | --- | --- | --- | 
| 0 | ((1,1),(3,4)) |`DRRRD`| | 
| 1 | ((1,1),(2,2),(2,3),(3,4)) |`DRRRD`| ((1,1),(2,2),(2,3),(3,4)) | 

Đối với phần đầu tiên, từ ((1,1)) đến ((2,2)), phân đoạn truy vấn là`DR`. Vì đây là các điểm được báo cáo liên tiếp nên đường dẫn ẩn không thể chứa ô truy vấn bên trong ((2,1)). Vì vậy đường dẫn ẩn phải sử dụng thứ tự ngược lại,`RD`. 

Đối với phần giữa, cả hai điểm đều nằm trên cùng một hàng, do đó đường dẫn chỉ đơn giản là`R`. 

Đối với phần cuối cùng, phân đoạn truy vấn lại được`RD`, vì vậy đường dẫn ẩn phải là`DR`. 

Nối ba mảnh cho`RD`+`R`+`DR`, chính xác là`RDRDR`. 

This example demonstrates why storing the last query is necessary. The reported coordinates alone do not distinguish`DR`từ`RD`, nhưng vị trí của họ trong truy vấn thì có. 

### Mẫu 2 

Xét một lưới (5\times5) có đường đi ẩn là`DDDDRRRR`. 

Truy vấn đầu tiên sử dụng hàng giữa (3):```
DDRRRRDD
```Đường dẫn ẩn cắt truy vấn này ở hàng giữa và tại các điểm cuối, cho đủ điểm để chia phần năm hàng ban đầu thành các phần nhỏ hơn. 

| Bước | Khoảng cách hàng tối đa | Hình dạng truy vấn | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 4 |`DDRRRRDD`| Hàng giữa được phát hiện | 
| 1 | 2 | Truy vấn năm hình cho mỗi phần | Tất cả các khoảng trống còn lại có khoảng cách hàng tối đa là 1 | 

Ở lần xây dựng lại cuối cùng, mọi khoảng trống cùng hàng đều buộc phải nằm ngang. Khoảng cách một hàng được giải quyết bằng cách so sánh phần tương ứng của truy vấn với sự vắng mặt hoặc hiện diện của giao lộ bên trong. Đường dẫn kết quả là`DDDDRRRR`. 

Dấu vết này thể hiện sự giảm nhị phân của phạm vi hàng. Số lượng hàng không cần phải được xử lý riêng lẻ. Tất cả các phần không chắc chắn hiện tại được tinh chỉnh đồng thời bằng một truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((m+n)\log m)) | Tối đa 10 truy vấn được tạo, mỗi truy vấn chứa (m+n-2) di chuyển và truy vấn cuối cùng được quét một lần | 
| Không gian | (O(m+n)) | Tối đa (m+n-1) ô được báo cáo và (m+n-2) di chuyển truy vấn được lưu trữ | 

Vì (m\le1000), (\lceil\log_2(m-1)\rceil\le10). Mỗi truy vấn có độ dài tối đa là 1998 nên tổng lượng dữ liệu truy vấn được tạo ra chỉ khoảng hai mươi nghìn ký tự. Các phản ứng tọa độ có cùng thứ tự. Điều này dễ dàng phù hợp với giới hạn bộ nhớ 512 MB và đủ nhỏ cho giới hạn một giây. 

## Trường hợp thử nghiệm 

Bởi vì đây là một vấn đề tương tác nên mẫu chính thức không thể được chuyển sang một mẫu thông thường`run()`hoạt động như đầu vào thông thường. Khai thác sau đây coi đường dẫn ẩn là một dòng đầu vào bổ sung và mô phỏng bộ tương tác. Trình mô phỏng áp dụng chính xác cấu trúc truy vấn tương tự và trả về các ô giao nhau mà người đánh giá thực sự sẽ trả về.```python
import sys
import io

def hidden_cells(m, n, path):
    r, c = 1, 1
    cells = [(r, c)]

    for ch in path:
        if ch == "D":
            r += 1
        else:
            c += 1
        cells.append((r, c))

    assert (r, c) == (m, n)
    return cells

def solve_offline(m, n, hidden):
    if m == 1:
        return "R" * (n - 1)

    if n == 1:
        return "D" * (m - 1)

    hidden_set = set(hidden)
    points = [(1, 1), (m, n)]
    last_query = None

    for _ in range(10):
        parts = []

        for (r1, c1), (r2, c2) in zip(points, points[1:]):
            mid = (r1 + r2) // 2
            parts.append("D" * (mid - r1))
            parts.append("R" * (c2 - c1))
            parts.append("D" * (r2 - mid))

        query = "".join(parts)
        last_query = query

        r, c = 1, 1
        response = [(r, c)] if (r, c) in hidden_set else []

        for ch in query:
            if ch == "D":
                r += 1
            else:
                c += 1
            if (r, c) in hidden_set:
                response.append((r, c))

        points = response

        if all(
            points[i + 1][0] - points[i][0] <= 1
            for i in range(len(points) - 1)
        ):
            break
    else:
        raise AssertionError("More than 10 queries required")

    qpos = {}
    r, c = 1, 1
    qpos[(r, c)] = 0

    for i, ch in enumerate(last_query, 1):
        if ch == "D":
            r += 1
        else:
            c += 1
        qpos[(r, c)] = i

    answer = []

    for a, b in zip(points, points[1:]):
        r1, c1 = a
        r2, c2 = b

        if r1 == r2:
            answer.append("R" * (c2 - c1))
            continue

        ia = qpos[a]
        ib = qpos[b]
        segment = last_query[ia:ib]

        if segment[0] == "D":
            answer.append("R" * (len(segment) - 1))
            answer.append("D")
        else:
            answer.append("D")
            answer.append("R" * (len(segment) - 1))

    result = "".join(answer)
    assert result == hidden
    return result

def run(inp: str) -> str:
    data = inp.split()
    m = int(data[0])
    n = int(data[1])
    hidden = data[2]

    return solve_offline(
        m,
        n,
        hidden_cells(m, n, hidden)
    )

# Provided sample, represented in simulator form.
assert run("3 4 RDRDR") == "RDRDR", "sample 1"

# Minimum-size grid.
assert run("1 2 R") == "R", "minimum-size horizontal"

# Single-column boundary case.
assert run("5 1 DDDD") == "DDDD", "minimum-width vertical"

# All moves of one direction.
assert run("1 8 RRRRRRR") == "RRRRRRR", "all right moves"

# Maximum-size grid, with all downs first and then all rights.
max_path = "D" * 999 + "R" * 999
assert run(f"1000 1000 {max_path}") == max_path, "maximum-size case"

# Alternating path, designed to exercise many turns.
zigzag = "RDRD" * 499 + "RD"
assert len(zigzag) == 1998
assert run(f"1000 1000 {zigzag}") == zigzag, "many turns"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 4 RDRDR`|`RDRDR`| Cung cấp mẫu và tái thiết theo đường chéo | 
|`1 2 R`|`R`| Lưới ngang kích thước tối thiểu | 
|`5 1 DDDD`|`DDDD`| Ranh giới một cột | 
|`1 8 RRRRRRR`|`RRRRRRR`| Con đường bắt buộc phải toàn quyền | 
|`1000 1000 D...DR...R`| Con đường ẩn tương tự | Kích thước tối đa và giới hạn mười truy vấn | 
|`1000 1000 RDRD...`| Con đường ẩn tương tự | Nhiều lượt và tái thiết điểm giữa lặp đi lặp lại | 

## Vỏ cạnh 

Khi (m=1), không có sự bất định nào cả. Để có đầu vào chính xác```
1 4
```robot phải di chuyển sang phải ba lần, vì vậy câu trả lời là`RRR`. Việc triển khai xử lý việc này trước khi vào vòng lặp tương tác. 

Khi (n=1), đường đi duy nhất có thể là đường thẳng đứng. Vì```
4 1
```câu trả lời là`DDD`. Một lần nữa, không cần truy vấn vì chỉ có một đường dẫn đơn điệu hợp lệ. 

Trường hợp hai ô chéo là sự mơ hồ quan trọng. Giả sử hai điểm được xác nhận liên tiếp là ((1,1)) và ((2,2)). Hai con đường có thể là`DR`Và`RD`. Nếu truy vấn sử dụng`DR`, sau đó`DR`như báo cáo đường dẫn ẩn ((2,1)). Nếu ô đó vắng mặt thì đường dẫn ẩn phải là`RD`. Mã xây dựng lại cuối cùng nắm bắt chính xác logic này bằng cách kiểm tra bước đi đầu tiên của phân đoạn truy vấn tương ứng. 

Cuối cùng, kích thước tối đa là (1000), tạo ra chênh lệch hàng ban đầu tối đa là (999). Trình tự chênh lệch hàng trong trường hợp xấu nhất là 

[ 
999,\ 500,\ 250,\ 125,\ 63,\ 32,\ 16,\ 8,\ 4,\ 2,\ 1. 
] 

Như vậy mười truy vấn là đủ ngay cả trong trường hợp xấu nhất. Việc xây dựng không bao giờ cần phải dựa vào việc bộ tương tác từ chối truy vấn thứ mười một, đây là một chi tiết phòng thủ hữu ích cho các vấn đề tương tác. Giao thức chính thức tuyên bố hơn mười truy vấn đều có câu trả lời sai ngay lập tức. 

Nếu bạn muốn, tôi cũng có thể biến phong cách này thành phong cách biên tập Codeforces điển hình hơn với văn xuôi ngắn hơn và bằng chứng chính xác trang trọng hơn.
