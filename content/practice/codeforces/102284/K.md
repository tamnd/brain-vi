---
title: "CF 102284K - \u041f\u0440\u0438\u0448\u0451\u043b \u0414\u0435\u043c\u0438\u0434 \u0438 \u0432\u0441\u0451 \u043f\u0440\u043e\u0432\u0435\u0440\u0438\u043b"
description: "Chúng tôi có một hàng (N) gói. Mỗi gói thuộc một trong bốn nhóm, được biểu thị bằng các chữ số 6, 7, 8 và 9. Đọc các nhóm từ trái sang phải sẽ cho ra số có giá trị xác định vẻ đẹp của hàng đợi."
date: "2026-08-13T08:58:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102284
codeforces_index: "K"
codeforces_contest_name: "\u041b\u041a\u0428 2019, \u0418\u044e\u043b\u044c, \u041c\u0438\u043a\u0441 \u0441\u0442\u0430\u0440\u0448\u0435\u0439 \u0438 \u043c\u043b\u0430\u0434\u0448\u0435\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434"
rating: 0
weight: 102284
solve_time_s: 201
verified: true
draft: false
---

[CF 102284K - \u041f\u0440\u0438\u0448\u0451\u043b \u0414\u0435\u043c\u0438\u0434 \u0438\u0432\u0441\u0451 \u043f\u0440\u043e\u0432\u0435\u0440\u0438\u043b](https://codeforces.com/problemset/problem/102284/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng (N) gói. Mỗi gói thuộc một trong bốn nhóm, được biểu thị bằng các chữ số 6, 7, 8 và 9. Đọc các nhóm từ trái sang phải sẽ cho ra số có giá trị xác định vẻ đẹp của hàng đợi. Vì tất cả các hàng đợi đều có cùng độ dài nên việc tối đa hóa con số này hoàn toàn giống với việc tối đa hóa chuỗi theo từ điển. 

Hoạt động duy nhất được phép là hoán đổi hai gói liền kề. Việc hoán đổi nhóm (i) và (j) tốn (c_{i,j}) giây và tổng chi phí không được vượt quá (K). Nhiệm vụ là xuất ra chuỗi từ điển lớn nhất có thể tiếp cận được trong ngân sách đó. 

Các ràng buộc là (N\le 10^5) và (K\le 10^9). Câu lệnh Codeforces ban đầu đưa ra giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. Một thuật toán bậc hai sẽ thực hiện khoảng (5\cdot10^9) lần lặp cơ bản trong trường hợp xấu nhất, vượt xa giới hạn dự định. Thực tế cấu trúc hữu ích là chỉ có bốn chữ số có thể, do đó, một thuật toán thực hiện công liên tục trên mỗi gói là thực tế. 

Có một số trường hợp dễ dàng phá vỡ việc thực hiện bất cẩn. 

Hãy xem xét ngân sách bằng 0 với các giao dịch hoán đổi chi phí bằng 0:```
3 0
678
0 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0
```Câu trả lời đúng là`876`. Một chương trình diễn giải (K=0) là "không được phép hoán đổi" sẽ trả về sai`678`. Một giao dịch hoán đổi có thể có chi phí bằng 0, vì vậy điều kiện là chi phí tối đa (K), không phải số lượng giao dịch hoán đổi nhiều nhất (K). 

Bẫy thứ hai là việc di chuyển một gói đến nhiều vị trí sẽ tốn tổng của tất cả các giao dịch hoán đổi liền kề trên đường đi của nó. Ví dụ:```
3 1
678
0 1 1 1
1 0 1 1
1 1 0 1
1 1 1 0
```Di chuyển 8 lên phía trước tốn (1+1=2), vì vậy điều đó là không thể. Di chuyển 7 vị trí sang trái tốn 1, đưa ra câu trả lời`768`. Việc triển khai bất cẩn chỉ kiểm tra chi phí hoán đổi 8 với người tiền nhiệm ngay trước nó có thể chọn sai`867`hoặc`876`. 

Chi phí cá nhân lớn tạo ra một trường hợp ranh giới khác:```
3 1000000000
678
0 1000000000 1000000000 1000000000
1000000000 0 1000000000 1000000000
1000000000 1000000000 0 1000000000
1000000000 1000000000 1000000000 0
```Di chuyển 7 lên phía trước tốn chính xác (10^9), vì vậy điều này được cho phép. Di chuyển 8 chi phí (2\cdot10^9) nên không được. Câu trả lời là`768`. Trường hợp đẳng thức phải sử dụng`cost <= K`và chi phí trung gian có thể vượt quá phạm vi số nguyên 32 bit mặc dù bản thân (K) tối đa là (10^9). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xây dựng câu trả lời từ trái sang phải. Tại mọi vị trí, hãy kiểm tra hàng đợi còn lại và xác định gói hàng nào có thể được chuyển lên phía trước trong phạm vi ngân sách còn lại. Trong số các gói đó, hãy chọn chữ số lớn nhất. Trong khi quét hàng đợi còn lại, chúng tôi có thể duy trì chi phí di chuyển từng gói hàng gặp phải lên phía trước, do đó một vị trí có thể được xử lý theo thời gian tuyến tính. 

Sự lựa chọn tham lam này là đúng vì chữ số đầu tiên chiếm ưu thế hơn giá trị của toàn bộ số. Nếu số 9 có thể được đặt ở vị trí hiện tại thì việc chọn bất cứ thứ gì nhỏ hơn sẽ không bao giờ có thể được sửa chữa bằng các vị trí sau này. Nếu chữ số đã chọn có thể được chuyển lên phía trước trong phạm vi ngân sách còn lại, chúng tôi có thể thực hiện chính xác các giao dịch hoán đổi đó và giữ nguyên phần còn lại của hàng đợi, vì vậy lựa chọn luôn khả thi. 

Vấn đề với việc triển khai này là việc quét lặp đi lặp lại. Có thể có (N) ứng viên cho vị trí đầu ra đầu tiên, (N-1) ứng viên cho vị trí đầu ra thứ hai, v.v. Tổng số vị trí đã ghé thăm có thể đạt tới 

[ 
N+(N-1)+\cdots+1=\frac{N(N+1)}2, 
] 

tức là khoảng (5\cdot10^9) khi (N=10^5). 

Quan sát quan trọng là chỉ có bốn chữ số có thể. Đối với mỗi chữ số (d), chúng ta không cần xem xét mọi lần xuất hiện của (d). Lần xuất hiện rẻ nhất để chuyển lên phía trước luôn là lần xuất hiện còn lại đầu tiên của (d), bởi vì mọi lần xuất hiện khác đều có tất cả các gói trước đó giống nhau cộng với chi phí hoán đổi không âm bổ sung. 

Vì vậy tại mỗi thời điểm chúng ta chỉ cần bốn ứng viên. Đối với mỗi chữ số, hãy duy trì vị trí của lần xuất hiện đầu tiên còn lại và chi phí chính xác cần thiết để đưa lần xuất hiện đó lên phía trước. 

Sau khi chọn gói loại (d), việc loại bỏ gói đó sẽ ảnh hưởng đến bốn chi phí này một cách rất có cấu trúc. Đối với một chữ số khác (x), nếu gói đã chọn nằm trước (x) đầu tiên còn lại, việc loại bỏ nó sẽ giảm chi phí cho (x) xuống (c_{d,x}). Nếu nó nằm sau (x) đầu tiên thì chi phí đó không thay đổi. 

Cập nhật duy nhất ít ngay lập tức hơn liên quan đến chữ số đã chọn (d). Lần xuất hiện đầu tiên của nó biến mất, vì vậy chúng ta phải tìm (d) còn lại tiếp theo. Chúng tôi giữ một danh sách liên kết đôi của các vị trí ban đầu hiện còn lại. Bắt đầu ngay sau vị trí đã loại bỏ, chúng tôi đi bộ cho đến vị trí (d) tiếp theo, cộng thêm chi phí băng qua. Đối với một chữ số cố định, các bước đi này trải qua các khoảng thời gian rời rạc giữa các lần xuất hiện liên tiếp của chữ số đó, do đó, mọi vị trí ban đầu được truy cập nhiều nhất một lần cho chữ số đó. Vì chỉ có bốn chữ số nên tất cả các bước đi như vậy cùng nhau mất (O(4N)=O(N)) thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét tham lam ngây thơ | (O(N^2)) | (O(N)) | Quá chậm | 
| Bảo trì bốn ứng viên tối ưu | (O(N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi ký tự từ`6`bởi vì`9`thành chỉ mục từ 0 đến 3. Điều này làm cho ma trận chi phí có thể định địa chỉ trực tiếp và cho phép chúng ta lặp qua bốn nhóm có thể theo thứ tự giảm dần. 
2. Với mỗi chữ số, hãy tìm lần xuất hiện đầu tiên còn lại của nó. Lúc đầu, đây chỉ đơn giản là lần xuất hiện đầu tiên của nó trong chuỗi gốc. Bên cạnh đó hãy tính`cost[d]`, chi phí để di chuyển sự kiện này lên đầu hàng đợi hiện tại. 

Đối với lần xuất hiện đầu tiên của (d), mỗi gói trước nó phải được gạch chéo đúng một lần. Do đó chi phí của nó là tổng của (c_{x,d}) trên tất cả các gói còn lại trước nó. 
3. Xây dựng danh sách liên kết đôi trên các vị trí ban đầu.`prev[i]`Và`next[i]`cho chúng tôi biết gói trước và gói tiếp theo vẫn còn trong hàng đợi. Việc xóa gói khỏi danh sách này đồng nghĩa với việc chuyển gói đó vào tiền tố câu trả lời đã được tạo sẵn. 
4. Tại mỗi vị trí đầu ra, kiểm tra các chữ số 9, 8, 7 và 6 theo thứ tự đó. Chọn chữ số đầu tiên tồn tại lần xuất hiện đầu tiên còn lại và chữ số đó`cost[d]`tối đa là ngân sách còn lại. 

Đây là quyết định tham lam. Chữ số lớn nhất có thể chấp nhận được phải được chọn vì vị trí hiện tại có ý nghĩa lớn hơn mọi vị trí sau này cộng lại. 
5. Thêm chữ số đã chọn vào câu trả lời và trừ đi chi phí di chuyển của nó khỏi (K). Lần xuất hiện được chọn là lần xuất hiện đầu tiên còn lại của chữ số đó, do đó, việc di chuyển nó lên phía trước sẽ tốn chính xác`cost[d]`. 
6. Xóa vị trí đã chọn khỏi danh sách liên kết. Điều này thay đổi thứ tự tương đối của các gói còn lại giống hệt như các giao dịch hoán đổi liền kề. 
7. Cập nhật giá của từng chữ số khác (x). Nếu lần xuất hiện đầu tiên còn lại của nó nằm sau vị trí bị loại bỏ thì gói bị loại bỏ đó là một trong những gói phải được chuyển qua, do đó giá của nó giảm đi (c_{d,x}). Nếu lần xuất hiện đầu tiên của nó nằm trước vị trí bị loại bỏ, thì gói hàng bị loại bỏ không nằm trong đường đi ngang qua của nó, do đó chi phí của nó không thay đổi. 
8. Tìm lần xuất hiện tiếp theo còn lại của chữ số đã chọn. Bắt đầu từ gói còn lại tiếp theo sau vị trí đã xóa, duyệt qua danh sách được liên kết cho đến khi tìm thấy gói khác có cùng chữ số. Thêm chi phí vượt qua mỗi gói đã truy cập vào chi phí cũ. 

Lần xuất hiện đầu tiên cũ có một số gói trước đó. Những gói đó vẫn còn trước lần xuất hiện đầu tiên mới. Các gói mới được truy cập giữa hai lần xuất hiện cũng phải được gạch chéo, do đó, việc cộng chính xác chi phí của chúng sẽ mang lại gói mới.`cost[d]`. 
9. Lặp lại cho đến khi tất cả (N) gói đã được đặt vào câu trả lời. 

### Tại sao nó hoạt động 

Điều bất biến là trước mỗi lần lặp,`first[d]`là lần xuất hiện đầu tiên còn lại của chữ số (d) và`cost[d]`chính xác là chi phí để di chuyển sự kiện đó lên đầu hàng đợi còn lại. 

Bởi vì tất cả chi phí hoán đổi đều không âm, nên không có sự xuất hiện sau nào của cùng một chữ số có thể rẻ hơn để chuyển lên phía trước so với số đầu tiên. Như vậy`cost[d]`thể hiện chi phí tối thiểu có thể có để đặt chữ số (d) ở vị trí hiện tại. 

Nếu chữ số lớn nhất với`cost[d] <= K`là (d), có một hàng đợi hợp lệ bắt đầu bằng (d), thu được bằng cách di chuyển lần xuất hiện đầu tiên của nó lên phía trước. Bất kỳ hàng đợi nào bắt đầu bằng một chữ số nhỏ hơn sẽ ngay lập tức tệ hơn về mặt từ điển. Do đó, mọi lựa chọn tham lam đều đồng ý với chữ số đầu tiên của câu trả lời tối ưu. 

Các cập nhật chi phí duy trì sự bất biến. Việc xóa một gói trước khi xuất hiện lần đầu tiên một chữ số khác sẽ loại bỏ chính xác một lần hoán đổi bắt buộc khỏi chi phí di chuyển của chữ số đó. Việc xóa lần xuất hiện đầu tiên của chữ số đã chọn sẽ thay đổi mục tiêu của nó sang lần xuất hiện tiếp theo và quá trình quét danh sách liên kết sẽ bổ sung chính xác chi phí của các gói còn lại mới được vượt qua. Do đó, bất biến vẫn đúng sau mỗi lần lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()
    a = [ord(ch) - ord('6') for ch in s]

    c = [list(map(int, input().split())) for _ in range(4)]

    first = [-1] * 4
    cost = [0] * 4

    # Find the first occurrence of every digit and its initial movement cost.
    for i, x in enumerate(a):
        for d in range(4):
            if first[d] == -1 and d != x:
                cost[d] += c[x][d]
        if first[x] == -1:
            first[x] = i

    # Doubly linked list of currently remaining positions.
    prev = [i - 1 for i in range(n)]
    nxt = [i + 1 for i in range(n)]
    nxt[n - 1] = -1

    answer = []

    for _ in range(n):
        chosen = -1

        # Lexicographically largest affordable digit.
        for d in range(3, -1, -1):
            if first[d] != -1 and cost[d] <= k:
                chosen = d
                break

        # The current first remaining package is always affordable:
        # choosing it costs zero.
        if chosen == -1:
            # This branch is unreachable.
            break

        p = first[chosen]
        spent = cost[chosen]
        k -= spent
        answer.append(chr(ord('6') + chosen))

        # Save the next remaining position before unlinking p.
        q = nxt[p]
        left = prev[p]

        if left != -1:
            nxt[left] = q
        if q != -1:
            prev[q] = left

        # Removing p reduces the cost for every first occurrence after p.
        for d in range(4):
            if d != chosen and first[d] != -1 and p < first[d]:
                cost[d] -= c[chosen][d]

        # Find the next remaining occurrence of the chosen digit.
        new_cost = spent
        cur = q

        while cur != -1 and a[cur] != chosen:
            new_cost += c[a[cur]][chosen]
            cur = nxt[cur]

        first[chosen] = cur
        if cur == -1:
            cost[chosen] = 0
        else:
            cost[chosen] = new_cost

    print(''.join(answer))

if __name__ == "__main__":
    solve()
```ban đầu`first`Và`cost`quá trình xây dựng xử lý hàng đợi ban đầu một lần. Khi vị trí`i`chứa chữ số`x`, nó đóng góp vào chi phí của mỗi chữ số chưa xuất hiện lần đầu tiên, ngoại trừ`x`chính nó. Ngoại lệ quan trọng vì lần xuất hiện đầu tiên của`x`là mục tiêu nên không bị vượt qua. 

Danh sách liên kết đại diện cho hàng đợi sau khi các gói đã chọn trước đó được chuyển vào câu trả lời. Chúng tôi sử dụng các chỉ mục gốc thay vì sửa đổi chuỗi về mặt vật lý, điều này tránh việc dịch chuyển các phần tử (O(N)) sau mỗi lần chọn. 

Vòng lựa chọn chỉ kiểm tra bốn chữ số. Vì nó kiểm tra từ 9 xuống 6 nên ứng cử viên khả thi đầu tiên chính xác là chữ số hiện tại tốt nhất về mặt từ điển. 

Bản cập nhật cho ba chữ số còn lại sử dụng phép so sánh`p < first[d]`. Sự so sánh này trái ngược với vị trí ban đầu, nhưng đó cũng là thứ tự trong danh sách liên kết của tất cả các gói còn lại. Việc xóa các gói không bao giờ thay đổi thứ tự tương đối của các gói còn lại. 

Đối với chữ số được chọn,`spent`là chi phí cũ của lần xuất hiện đầu tiên. Sau khi loại bỏ sự xuất hiện đó, quá trình quét sẽ bắt đầu ở gói còn lại tiếp theo. Mỗi gói gặp phải trước lần xuất hiện tiếp theo của cùng một chữ số phải được vượt qua bởi lần xuất hiện đầu tiên mới. Danh sách liên kết tự động bỏ qua các gói đã được chọn. 

Tất cả chi phí được lưu trữ dưới dạng số nguyên Python. Một chuyển động có thể vượt qua (10^5) gói, mỗi gói có giá lên tới (10^9), do đó tổng trung gian có thể đạt tới (10^{14}), lớn hơn nhiều so với số nguyên 32 bit. 

Điều kiện là`cost[d] <= k`, không`cost[d] < k`, vì được phép chi tiêu toàn bộ ngân sách còn lại. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, hàng đợi ban đầu là`999789`và ngân sách là 2. Ba số 9 đầu tiên đã ở phía trước nên việc lựa chọn chúng không tốn kém gì. 8 số đầu tiên sẽ yêu cầu vượt qua 9, 9, 9 và 7, có giá (3+3+3+1=10), vì vậy ban đầu không thể chọn nó. Sau khi cố định ba số 9 đầu tiên, 8 chỉ cần vượt qua 7, tức là có giá 1. 

| Bước | Những ứng cử viên đầu tiên còn lại | Chi phí thí sinh tháng 6, 7, 8, 9 | Ngân sách | Được chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 7, 8, 9 | không có, 3, 10, 0 | 2 | 9 | 
| 2 | 7, 8, 9 | không có, 3, 10, 0 | 2 | 9 | 
| 3 | 7, 8, 9 | không có, 3, 10, 0 | 2 | 9 | 
| 4 | 7, 8, 9 | không có sẵn, 1, 1, 1 | 2 | 8 | 
| 5 | 7, 9 | không có sẵn, 0, 0 | 1 | 9 | 
| 6 | 7 | không có sẵn, 0, không có sẵn | 1 | 7 | 

Bước thứ tư chứng minh tại sao chi phí phải được duy trì cho hàng đợi còn lại hiện tại thay vì chỉ tính toán từ vị trí ban đầu. Sau ba lựa chọn đầu tiên, 8 chỉ còn lại một gói trước nó, đó là 7, vì vậy giá của nó chính xác là 1. Câu trả lời là`999897`. 

Đối với ví dụ thứ hai, hãy xem xét```
3 1
678
0 1 1 1
1 0 1 1
1 1 0 1
1 1 1 0
```Ban đầu, 6 giá 0, 7 giá 1 và 8 giá 2. Chữ số lớn nhất có thể chấp nhận được là 7. Sau khi chuyển nó lên phía trước, hàng đợi còn lại là`68`và ngân sách còn lại là 0. 

| Bước | Đơn hàng còn lại | Chi phí 6 | Chi phí 7 | Chi phí 8 | Ngân sách | Được chọn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 678 | 0 | 1 | 2 | 1 | 7 | 
| 2 | 68 | 0 | không có sẵn | 1 | 0 | 6 | 
| 3 | 8 | không có sẵn | không có sẵn | 0 | 0 | 8 | 

Bước thứ hai thể hiện việc cập nhật chi phí. Việc loại bỏ 7 sẽ giảm chi phí đưa 8 về phía trước một cách chính xác (c_{7,8}=1). Vì ngân sách còn lại bằng 0 nên 8 không thể di chuyển trước 6, nên kết quả cuối cùng là`768`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Mỗi lần lặp sẽ kiểm tra bốn chữ số và đối với mỗi chữ số, quá trình quét danh sách liên kết sẽ vượt qua từng vị trí ban đầu nhiều nhất một lần. | 
| Không gian | (O(N)) | Biểu diễn chuỗi, mảng kiểu và hai mảng danh sách liên kết đều sử dụng bộ nhớ tuyến tính. | 

Đối với (N=10^5), thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi gói ngoài việc kiểm tra bốn chữ số giới hạn. Tổng chi phí di chuyển có thể lớn hơn nhiều so với (K), nhưng số nguyên Python xử lý chúng một cách chính xác. Độ phức tạp tuyến tính phù hợp thoải mái với giới hạn 2 giây và 512 MB đã nêu. 

## Trường hợp thử nghiệm```python
import io
import sys

def solve_data(inp: str) -> str:
    it = iter(inp.split())

    n = int(next(it))
    k = int(next(it))
    s = next(it)

    c = [[int(next(it)) for _ in range(4)] for _ in range(4)]

    a = [ord(ch) - ord('6') for ch in s]

    first = [-1] * 4
    cost = [0] * 4

    for i, x in enumerate(a):
        for d in range(4):
            if first[d] == -1 and d != x:
                cost[d] += c[x][d]
        if first[x] == -1:
            first[x] = i

    prev = [i - 1 for i in range(n)]
    nxt = [i + 1 for i in range(n)]
    nxt[n - 1] = -1

    ans = []

    for _ in range(n):
        chosen = -1

        for d in range(3, -1, -1):
            if first[d] != -1 and cost[d] <= k:
                chosen = d
                break

        p = first[chosen]
        spent = cost[chosen]
        k -= spent
        ans.append(chr(ord('6') + chosen))

        q = nxt[p]
        left = prev[p]

        if left != -1:
            nxt[left] = q
        if q != -1:
            prev[q] = left

        for d in range(4):
            if d != chosen and first[d] != -1 and p < first[d]:
                cost[d] -= c[chosen][d]

        new_cost = spent
        cur = q

        while cur != -1 and a[cur] != chosen:
            new_cost += c[a[cur]][chosen]
            cur = nxt[cur]

        first[chosen] = cur
        cost[chosen] = 0 if cur == -1 else new_cost

    return ''.join(ans)

def run(inp: str) -> str:
    return solve_data(inp).strip()

# Provided sample
assert run(
    """\
6 2
999789
1 1 1 1
1 1 1 1
1 1 1 3
1 1 3 1
"""
) == "999897", "sample 1"

# Minimum-size input
assert run(
    """\
1 0
6
0 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0
"""
) == "6", "single package"

# Zero budget, but all swaps are free
assert run(
    """\
3 0
678
0 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0
"""
) == "876", "free swaps must still be allowed"

# Exact budget boundary and multi-swap cost
assert run(
    """\
3 1
678
0 1 1 1
1 0 1 1
1 1 0 1
1 1 1 0
"""
) == "768", "8 needs two swaps, 7 needs exactly one"

# Costs near the maximum and exact K boundary
assert run(
    """\
3 1000000000
678
0 1000000000 1000000000 1000000000
1000000000 0 1000000000 1000000000
1000000000 1000000000 0 1000000000
1000000000 1000000000 1000000000 0
"""
) == "768", "large integer costs"

# Maximum-size input, all equal values
n = 100000
s = "6" * n
matrix = "\n".join(["0 0 0 0"] * 4)
large_input = f"{n} 0\n{s}\n{matrix}\n"

assert run(large_input) == s, "maximum N and all equal values"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`, xếp hàng`6`|`6`| Kích thước tối thiểu và không có sự thay thế có sẵn. | 
|`3 0`, xếp hàng`678`, tất cả chi phí bằng không |`876`| Hoán đổi chi phí bằng 0 có hiệu lực ngay cả khi ngân sách bằng 0. | 
|`3 1`, xếp hàng`678`, chi phí hoán đổi đơn vị |`768`| Một gói có thể yêu cầu nhiều giao dịch hoán đổi liền kề và được phép bình đẳng với (K). | 
|`3 10^9`, xếp hàng`678`, chi phí ngoài đường chéo (10^9) |`768`| Tổng số nguyên lớn và ranh giới ngân sách chính xác. | 
| (N=100000), hàng đợi chỉ`6`| Chuỗi 100000 ký tự tương tự | Kích thước đầu vào tối đa và các giá trị bằng nhau lặp lại. | 

## Vỏ cạnh 

Trường hợp ngân sách bằng 0 với giao dịch hoán đổi miễn phí được xử lý bằng cách so sánh`cost[d] <= k`. Vì```
3 0
678
0 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0
```mọi ứng cử viên đều có chi phí di chuyển bằng 0. Thuật toán đầu tiên chọn 9, rồi 8, rồi 7, tạo ra`876`. Không cần xử lý đặc biệt đối với (K=0). 

Trường hợp hoán đổi nhiều lần```
3 1
678
0 1 1 1
1 0 1 1
1 1 0 1
1 1 1 0
```bắt đầu với giá 0 cho 6, 1 cho 7 và 2 cho 8. Thuật toán chọn 7 vì đây là chữ số lớn nhất có thể chấp nhận được. Việc loại bỏ 7 làm cho giá của 8 giảm từ 2 xuống 1, vì 7 là một trong những gói mà 8 sẽ vượt qua. Ngân sách còn lại bằng 0 nên 6 được chọn trước 8. Kết quả đầu ra là`768`. 

Trường hợp chi phí lớn```
3 1000000000
678
0 1000000000 1000000000 1000000000
1000000000 0 1000000000 1000000000
1000000000 1000000000 0 1000000000
1000000000 1000000000 1000000000 0
```có chi phí 0 cho 6, (10^9) cho 7 và (2\cdot10^9) cho 8. Thuật toán chấp nhận 7 chính xác tại ranh giới ngân sách và từ chối 8. Sau khi chi tiêu (10^9), ngân sách còn lại bằng 0, cho`768`. Các số nguyên có độ chính xác tùy ý của Python duy trì phép so sánh (2\cdot10^9) mà không bị tràn. 

Các giá trị bằng nhau không yêu cầu hoán đổi nhân tạo. Nếu hàng đợi chỉ có 6 số thì 6 số còn lại đầu tiên luôn có giá bằng 0 vì nó đã ở phía trước hàng đợi còn lại. Thuật toán chỉ đơn giản là chọn 6 lần tiếp theo. Ngay cả khi (c_{6,6}) lớn thì không có lý do gì để hoán đổi các gói bằng nhau, do đó chi phí đường chéo không bao giờ tính vào chi phí di chuyển của lần xuất hiện đầu tiên. 

Trường hợp kích thước tối đa có (N=10^5), do đó, việc sửa đổi vật lý hàng đợi sau mỗi lần lựa chọn sẽ rất nguy hiểm vì các phép dịch lặp lại có thể trở thành bậc hai. Biểu diễn danh sách liên kết sẽ loại bỏ một vị trí đã chọn trong thời gian không đổi và chỉ thay đổi dữ liệu chi phí bị ảnh hưởng bởi việc loại bỏ đó. Toàn bộ công việc quét vẫn tuyến tính vì mỗi vị trí được duyệt qua nhiều nhất một lần cho mỗi chữ số trong số bốn chữ số có thể.
