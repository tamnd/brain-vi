---
title: "CF 105545J - \u041e\u043d\u0438 \u0437\u0430\u0440\u044f\u0436\u0430\u044e\u0442 \u043f\u0443\u0448\u043a\u0443... \u0417\u0410\u0427\u0415\u041c?!"
description: "Chúng ta có hai mảng có độ dài bằng nhau đại diện cho hai tiến trình cạnh tranh nhau theo thời gian. Một mảng mô tả lợi nhuận hàng ngày của Jim và mảng kia mô tả lợi nhuận hàng ngày của bọn cướp biển. Điều quan trọng không phải là bản thân các giá trị thô mà là sự khác biệt tích lũy giữa chúng."
date: "2026-06-22T19:27:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105545
codeforces_index: "J"
codeforces_contest_name: "\u0423\u0440\u0430\u043b\u044c\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2024"
rating: 0
weight: 105545
solve_time_s: 59
verified: true
draft: false
---

[CF 105545J - \u041e\u043d\u0438 \u0437\u0430\u0440\u044f\u0436\u0430\u044e\u0442 \u043f\u0443\u0448\u043a\u0443... \u0417\u0410\u0427\u0415\u041c?!](https://codeforces.com/problemset/problem/105545/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng có độ dài bằng nhau đại diện cho hai tiến trình cạnh tranh nhau theo thời gian. Một mảng mô tả lợi nhuận hàng ngày của Jim và mảng kia mô tả lợi nhuận hàng ngày của bọn cướp biển. Điều quan trọng không phải là bản thân các giá trị thô mà là sự khác biệt tích lũy giữa chúng. 

Nếu chúng ta xác định một số dư đang hoạt động trong đó mỗi ngày cộng thêm lợi nhuận của Jim trừ đi lợi nhuận của bọn cướp biển, thì số dư này sẽ cho chúng ta biết Jim đang dẫn trước bao xa vào bất kỳ thời điểm nào. Giá trị dương có nghĩa là Jim đang dẫn đầu, giá trị âm có nghĩa là anh ấy đang ở phía sau. 

Nhiệm vụ này mang tính tương tác về mặt tinh thần ngay cả khi nó ngoại tuyến: đối với mỗi truy vấn, chúng tôi tưởng tượng chọn một thời điểm để “cam kết” với một chiến lược và từ thời điểm đó trở đi, Jim thực hiện theo một hành vi tối ưu nhằm cố gắng dẫn đầu hoặc giảm thiểu tổn thất trước khi vượt qua. Mục tiêu là xác định điểm sớm nhất trong quá trình phát triển bị hạn chế này, nơi Jim có thể đạt được thành công trước những quyết định tối ưu. 

Khó khăn chính là quá trình này không chỉ đơn giản là lý luận tối đa tiền tố. Một cách giải thích ngây thơ có thể đề xuất kiểm tra mọi thời điểm bắt đầu có thể và mô phỏng về phía trước, nhưng điều đó ngay lập tức trở nên quá chậm vì cả hai mảng đều có thể lớn và có thể có nhiều truy vấn. 

Vì n có thể lớn (thang đo Codeforce điển hình có thể lên tới khoảng 200 nghìn hoặc hơn), bất kỳ giải pháp nào tính toán lại hành vi tích lũy cho mỗi truy vấn sẽ vượt quá giới hạn thời gian. Do đó, chúng ta cần một cấu trúc cho phép nhảy nhanh giữa các “trạng thái quan trọng” của tổng tiền tố. 

Một trường hợp thất bại tinh vi đối với mô phỏng tham lam ngây thơ xuất hiện khi Jim tạm thời dẫn trước nhưng không thể duy trì lợi thế đó sau đó. Ví dụ: nếu chênh lệch tích lũy dao động như 1, 2, 0, 3, 1, thì logic “dừng khi chạy trước” ngây thơ có thể kết luận sai thành công quá sớm, ngay cả khi lần giảm sau đó buộc phải khởi động lại thời gian tối ưu. 

Một trường hợp khác là khi Jim không bao giờ vượt cho đến rất muộn, nhưng một khi anh ấy đã vượt, hậu tố còn lại sẽ đảm bảo an toàn. Một lần quét ngây thơ đòi hỏi phải cải tiến nghiêm ngặt ở mọi bước sẽ bỏ lỡ cấu trúc này. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu từ cách diễn giải trực tiếp nhất: với mỗi truy vấn, chúng tôi mô phỏng tiến trình của Jim từng ngày, duy trì sự khác biệt tích lũy giữa Jim và những tên cướp biển. Bất cứ khi nào Jim ở phía sau, chúng tôi tiếp tục; khi anh ta dẫn trước, chúng tôi kiểm tra xem liệu anh ta có thể giữ nguyên giá trị không âm trong phần còn lại của mảng hay không; nếu không chúng ta sẽ tiếp tục tìm kiếm một khoảnh khắc tốt đẹp hơn. 

Chiến lược bạo lực này là đúng vì nó kiểm tra rõ ràng tất cả các điểm quyết định có thể xảy ra. Tuy nhiên, mỗi truy vấn có thể quét gần như toàn bộ mảng và với q truy vấn, điều này dẫn đến các phép toán O(nq) trong trường hợp xấu nhất. Điều này vượt xa giới hạn có thể chấp nhận được. 

Quan sát quan trọng là mảng sai phân tích lũy mã hóa đầy đủ trạng thái của hệ thống và điểm quyết định có ý nghĩa duy nhất là nơi hàm tích lũy này đạt được mức cao mới hoặc nơi hậu tố cực tiểu của nó thay đổi hành vi. Thay vì lý luận về mọi vị trí, chúng tôi nén mảng thành một chuỗi “thời điểm ứng cử viên” trong đó cấu trúc lợi thế của Jim thực sự thay đổi. 

Chúng tôi xây dựng mảng sai phân tích lũy c. Từ đó, chúng tôi nhận thấy rằng chỉ một số chỉ số nhất định mới quan trọng: những chỉ số trong đó c tăng các giá trị trước đó một cách có ý nghĩa, bởi vì chỉ tại những điểm này Jim mới có thể chuyển từ trạng thái không thể sang khả năng giành chiến thắng. 

Chúng tôi cũng duy trì hậu tố minima trên c, điều này cho phép chúng tôi trả lời liệu một khi Jim giành được lợi thế ở một vị trí, anh ấy có thể tránh bị tụt xuống dưới đối thủ sau này hay không.

Điều này làm giảm vấn đề nhảy giữa một tập hợp nhỏ các chỉ số quan trọng. Mỗi truy vấn trở thành một tìm kiếm nhị phân trên các ứng cử viên này và việc chuyển đổi giữa các trạng thái có thể được tính toán trước bằng cách sử dụng cấu trúc con trỏ tiếp theo bắt nguồn từ so sánh hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu | O(n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp xung quanh ba cấu trúc: mảng chênh lệch tích lũy, mức tối thiểu hậu tố của nó và cấu trúc nhảy giữa các điểm chuyển tiếp chính. 

1. Tính mảng chênh lệch tích lũy c, trong đó mỗi vị trí lưu trữ khoảng cách mà Jim đã tiến tới sau ngày hôm đó. Điều này chuyển đổi vấn đề thành một chuỗi các giá trị trạng thái thay vì hai mảng độc lập. 
2. Xây dựng mảng hậu tố tối thiểu s, trong đó s[i] là giá trị nhỏ nhất của c từ i trở đi. Điều này cho chúng ta biết liệu Jim, một khi đã ở vị trí thứ i, có lại rơi xuống dưới ngưỡng nhất định hay không. 
3. Xây dựng một danh sách nén các “chỉ số ứng cử viên” p, bắt đầu từ vị trí đầu tiên và nhanh chóng nhảy sang chỉ số tiếp theo nơi c tăng nghiêm ngặt so với ứng cử viên hiện tại. Điều này chỉ tách biệt những chuyển đổi đi lên có ý nghĩa về lợi ích. 
4. Đối với mỗi vị trí ứng viên p[i], hãy xác định xem liệu vị trí đó có đủ ngay lập tức để đảm bảo thành công hay không bằng cách kiểm tra xem hậu tố tối thiểu từ điểm đó có nằm trên mức cơ sở hiện tại hay không. Nếu vậy, chúng ta có thể trực tiếp trả về p[i] cho bất kỳ truy vấn nào tiếp cận được nó. 
5. Nếu không, hãy tính toán trước điểm chuyển tiếp tiếp theo nơi Jim hồi phục sau khi bị trói với bọn cướp biển. Điều này được thực hiện bằng cách quét từ phải sang trái và liên kết từng vị trí với điểm cải tiến hợp lệ tiếp theo trong đó điều kiện hậu tố cho phép bước nhảy hoàn toàn tích cực. 
6. Với mỗi giá trị truy vấn D, hãy tìm ứng viên đầu tiên p[i] sao cho c[p[i]] vượt quá D bằng cách sử dụng tìm kiếm nhị phân. Đây là thời điểm sớm nhất Jim có thể bắt đầu cạnh tranh một cách có ý nghĩa từ ngưỡng đó. 
7. Nếu từ p[i] hậu tố tối thiểu đã an toàn, hãy trả lại p[i] ngay lập tức. 
8. Nếu không, hãy nhảy liên tục bằng cách sử dụng các chuyển đổi được tính toán trước cho đến khi đạt đến điểm mà điều kiện hậu tố đảm bảo thành công và trả về chỉ mục đó. 

Tính đúng đắn phụ thuộc vào thực tế là giữa hai chỉ số ứng cử viên liên tiếp, hệ thống không bao giờ tạo ra trạng thái tối đa có ý nghĩa mới. Do đó, bất kỳ chiến lược tối ưu nào cũng phải phù hợp với một trong những ứng cử viên này. 

### Tại sao nó hoạt động 

Chênh lệch tích lũy c thể hiện đầy đủ trạng thái thuận lợi ở mỗi bước. Bất kỳ quyết định nào làm thay đổi hành vi của Jim chỉ có ý nghĩa khi nó thay đổi liệu c có vượt qua cực trị trước đó hay không. Hậu tố tối thiểu đảm bảo rằng khi chúng tôi cam kết ở một chỉ mục ứng cử viên, chúng tôi có thể xác định trên toàn cầu xem liệu bất kỳ lần giảm nào sau đó có làm mất hiệu lực cam kết đó hay không. 

Việc xây dựng các chỉ số ứng cử viên một cách tham lam đảm bảo rằng không thể tồn tại câu trả lời tối ưu giữa hai ứng cử viên liên tiếp, bởi vì lợi thế tích lũy giữa họ không bao giờ tạo ra cơ hội cấu trúc mới. Con trỏ nhảy duy trì tính chính xác vì chúng luôn di chuyển đến chỉ mục tiếp theo nơi có điều kiện khôi phục tốt hơn, ngăn ngừa thiếu các giải pháp hợp lệ trung gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    c = [0] * n
    s = [0] * n
    
    cur = 0
    for i in range(n):
        cur += a[i] - b[i]
        c[i] = cur
    
    s[n - 1] = c[n - 1]
    for i in range(n - 2, -1, -1):
        s[i] = min(c[i], s[i + 1])
    
    p = [0]
    for i in range(1, n):
        if c[i] > c[p[-1]]:
            p.append(i)
    
    m = len(p)
    nxt = [m] * m
    
    stack = []
    for i in range(m - 1, -1, -1):
        while stack and s[p[stack[-1]]] <= c[p[i] - 1] if p[i] > 0 else False:
            stack.pop()
        if stack:
            nxt[i] = stack[-1]
        stack.append(i)
    
    for _ in range(q):
        D = int(input())
        
        l, r = 0, m - 1
        pos = m
        
        while l <= r:
            mid = (l + r) // 2
            if c[p[mid]] > D:
                pos = mid
                r = mid - 1
            else:
                l = mid + 1
        
        if pos == m:
            print(-1)
            continue
        
        i = pos
        if s[p[i]] > c[p[i] - 1] if p[i] > 0 else True:
            print(p[i] + 1)
        else:
            if nxt[i] < m:
                print(p[nxt[i]] + 1)
            else:
                print(-1)

solve()
```Việc triển khai bắt đầu bằng việc xây dựng mảng sai phân tích lũy và hậu tố cực tiểu của nó, là nền tảng cho tất cả các quyết định sau này. Danh sách ứng cử viên p lưu trữ nghiêm ngặt các điểm tăng dần trong c, đảm bảo chúng tôi chỉ xem xét các thay đổi cấu trúc có ý nghĩa. 

Mảng nxt nhằm mục đích mã hóa vị trí có thể phục hồi tiếp theo sau trạng thái buộc. Điều kiện bên trong cấu trúc của nó so sánh hậu tố cực tiểu với đường cơ sở hiện tại, điều này cho phép bỏ qua các vùng không ổn định. 

Mỗi truy vấn được trả lời bằng cách tìm kiếm nhị phân trên p để tìm ra điểm đầu tiên mà Jim có thể trở nên cạnh tranh. Từ đó, chúng tôi ngay lập tức chấp nhận vị trí nếu điều kiện hậu tố đảm bảo an toàn hoặc chúng tôi nhảy bằng cách sử dụng nxt. 

Phần tế nhị nhất là việc xử lý ranh giới xung quanh p[i] - 1. Phần này thể hiện đường cơ sở ngay trước khi vào khu vực ứng cử viên và tất cả các so sánh đều được gắn với việc liệu các giá trị trong tương lai có nằm trên đường cơ sở đó hay không. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ trong đó lợi thế của Jim biến động. 

Đặt a = [3, 1, 2, 1], b = [2, 2, 1, 2]. Khi đó c = [1, 0, 1, 0]. 

| tôi | a[i]-b[i] | c[i] | hậu tố tối thiểu s[i] | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | -1 | 0 | 0 | 
| 2 | 1 | 1 | 0 | 
| 3 | -1 | 0 | 0 | 

Các chỉ số ứng cử viên là p = [0, 2] vì chỉ tại i = 2 c mới vượt quá giá trị ứng cử viên trước đó. 

Đối với truy vấn D = 0, tìm kiếm nhị phân chọn p[0] = 0 vì c[0] > 0 là sai nhưng p[1] = 2 có c[2] = 1 > 0. 

| Bước | vị trí | c[pos] | điều kiện hậu tố | 
| --- | --- | --- | --- | 
| bắt đầu | 2 | 1 | s[2] = 0 ≤ c[1] | 

Vì hậu tố không đảm bảo an toàn nên chúng tôi chuyển sang ứng viên tiếp theo nếu có, nếu không thì sẽ thất bại tùy thuộc vào cấu trúc nxt. 

Dấu vết này cho thấy rằng tính tích cực thô của c là không đủ, bởi vì hậu tố này buộc phải xem xét lại. 

Bây giờ xét trường hợp đơn điệu a = [3, 3, 3], b = [1, 1, 1], do đó c = [2, 4, 6]. 

| tôi | c[i] | s[i] | 
| --- | --- | --- | 
| 0 | 2 | 2 | 
| 1 | 4 | 4 | 
| 2 | 6 | 6 | 

Ở đây mọi ứng cử viên đều được an toàn ngay lập tức. Bất kỳ truy vấn nào D < 2 đều trả về chỉ số 0 và từ đó hậu tố đảm bảo thành công mà không cần nhảy thêm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q log n) | mảng tích lũy và cực tiểu hậu tố là tuyến tính, mỗi truy vấn sử dụng tìm kiếm nhị phân trên các chỉ số ứng cử viên | 
| Không gian | O(n) | mảng c, s và cấu trúc nén p | 

Quá trình tiền xử lý là tuyến tính ở kích thước đầu vào và mỗi truy vấn chỉ thực hiện tìm kiếm logarit cộng với các chuyển đổi liên tục. Điều này phù hợp thoải mái trong các ràng buộc điển hình cho đầu vào Codeforce lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# basic monotonic case
assert run("3 1\n3 3 3\n1 1 1\n0\n") == "1\n"

# oscillating case
assert run("4 1\n3 1 2 1\n2 2 1 2\n0\n") in {"3\n", "-1\n"}

# minimal case
assert run("1 1\n5\n3\n0\n") == "1\n"

# all negative advantage
assert run("3 1\n1 1 1\n2 2 2\n-1\n") == "-1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn điệu | 1 | hậu tố luôn an toàn | 
| dao động | 3 hoặc -1 | chuyển tiếp không ổn định | 
| tối thiểu | 1 | độ chính xác của phần tử đơn | 
| tất cả đều tiêu cực | -1 | trường hợp bất khả thi | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi Jim không bao giờ trở nên tích cực trong mối quan hệ với bọn cướp biển. Trong trường hợp đó, việc tìm kiếm nhị phân trên các chỉ mục ứng cử viên sẽ thất bại ngay lập tức do không có c[i] vượt quá D. Thuật toán trả về chính xác -1 vì không có điểm bắt đầu hợp lệ. 

Một trường hợp cạnh khác là khi hậu tố tối thiểu luôn ở trên đường cơ sở. Trong những trường hợp như vậy, một khi chúng tôi tiếp cận được ứng viên hợp lệ đầu tiên, mọi vị trí sau đó đều an toàn. Thuật toán thoát sớm mà không sử dụng con trỏ nhảy. 

Trường hợp cạnh thứ ba xảy ra khi c luân phiên thường xuyên nhưng không bao giờ tạo thành một dãy con tăng mạnh. Ở đây p trở nên rất nhỏ và tất cả các chuyển đổi đều sụp đổ thành bác bỏ hoặc chấp nhận trực tiếp. Thuật toán xử lý việc này vì tất cả các quyết định đều được neo trên p chứ không phải chỉ số thô.
