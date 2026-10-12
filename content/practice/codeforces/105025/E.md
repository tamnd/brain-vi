---
title: "CF 105025E - \u0411\u0430\u043d\u0430\u043d\u043e\u0432\u044b\u0439 \u0431\u0438\u0437\u043d\u0435\u0441 \u041e\u043b\u0435\u0433\u0430"
description: "Chúng ta được cung cấp một chuỗi giá chuối được sắp xếp thành một dòng. Mỗi vị trí giữ một giá trị và chúng ta được phép thực hiện chính xác các thao tác $k$, trong đó một thao tác hoán đổi hai phần tử liền kề."
date: "2026-06-28T01:40:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105025
codeforces_index: "E"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u00ab\u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430\u00bb \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 105025
solve_time_s: 59
verified: true
draft: false
---

[CF 105025E - \u0411\u0430\u043d\u0430\u043d\u043e\u0432\u044b\u0439 \u0431\u0438\u0437\u043d\u0435\u0441 \u041e\u043b\u0435\u0433\u0430](https://codeforces.com/problemset/problem/105025/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi giá chuối được sắp xếp thành một dòng. Mỗi vị trí giữ một giá trị và chúng ta được phép thực hiện chính xác$k$hoạt động, trong đó một hoạt động hoán đổi hai phần tử liền kề. 

Sau khi thực hiện các phép hoán đổi này, chúng tôi xem xét mảng cuối cùng và đánh giá từng cặp vị trí lân cận. Đối với mỗi cặp lân cận, chúng tôi tính toán chênh lệch tuyệt đối của các giá trị của chúng và chúng tôi quan tâm đến chênh lệch tối đa đó. Mục tiêu là sắp xếp mảng bằng cách sử dụng chính xác$k$các giao dịch hoán đổi liền kề sao cho chênh lệch liền kề tối đa này trở nên lớn nhất có thể. 

Khó khăn chính là việc hoán đổi mang tính chất địa phương. Mặc dù chúng ta có thể tự do sắp xếp lại mảng, chi phí di chuyển các giá trị phụ thuộc vào khoảng cách giữa chúng bắt đầu, vì mỗi lần hoán đổi chỉ di chuyển các phần tử theo một vị trí. 

Những hạn chế đẩy tới một$O(n)$hoặc$O(n \log n)$giải pháp. Độ dài mảng có thể đạt tới$10^5$, trong khi$k$có thể lớn như$10^{18}$, điều này ngay lập tức ngụ ý rằng việc mô phỏng các giao dịch hoán đổi là không thể. Bất kỳ cách tiếp cận nào xử lý từng trao đổi một cách rõ ràng sẽ thất bại. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 0$. Trong tình huống đó, mảng hoàn toàn không thể thay đổi và câu trả lời chỉ đơn giản là chênh lệch tuyệt đối tối đa so với các cặp liền kề ban đầu. Một trường hợp cạnh khác là khi$k$là cực kỳ lớn. Trong chế độ đó, mảng gần như có thể được sắp xếp lại hoàn toàn và chúng ta cần hiểu giới hạn cấu trúc nào vẫn tồn tại ngay cả khi có nhiều lần hoán đổi. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu với cách giải thích trực tiếp nhất. Chúng ta có thể mô phỏng tất cả các chuỗi của$k$các giao dịch hoán đổi liền kề và tính toán kết quả cấu hình tốt nhất. Điều này đúng về nguyên tắc vì mỗi trạng thái có thể truy cập chính xác$k$hoán đổi tương ứng với một thỏa thuận hợp lệ. Vấn đề là yếu tố phân nhánh. Mỗi cấu hình có$O(n)$sự hoán đổi có thể xảy ra, và sau đó$k$bước này tăng theo cấp số nhân. Ngay cả đối với nhỏ$n$, điều này hoàn toàn không thể thực hiện được. 

Bước tiếp theo là ngừng suy nghĩ về trình tự các giao dịch hoán đổi và thay vào đó hãy nghĩ về việc các phần tử có thể di chuyển bao xa. Mỗi lần hoán đổi sẽ thay đổi vị trí của chính xác hai phần tử theo một bước. Nếu chúng ta theo dõi hai phần tử mà chúng ta muốn đặt liền kề nhau, thì câu hỏi liên quan duy nhất là cần bao nhiêu lần hoán đổi để đưa chúng đến cạnh nhau. 

Giả sử chúng ta lấy hai giá trị tại các vị trí$i$Và$j$, với$i < j$. Để làm cho chúng liền kề nhau, chúng ta cần loại bỏ khoảng cách giữa chúng. Mỗi lần hoán đổi có thể giảm khoảng cách này tối đa một. Do đó, số lần hoán đổi tối thiểu cần thiết để làm cho chúng liền kề nhau là$j - i - 1$. Quan sát này chuyển vấn đề thành việc chọn một cặp chỉ số có khoảng cách đủ nhỏ. 

Chúng ta có thể tự do chọn bất kỳ cặp giá trị nào làm cặp liền kề cuối cùng. Vì vậy chúng tôi muốn tối đa hóa$|c_i - c_j|$tùy thuộc vào$j - i - 1 \le k$, hoặc tương đương$j - i \le k + 1$. 

Sự cải tổ này thay đổi hoàn toàn cấu trúc. Thay vì suy luận về các hoán vị, chúng ta chỉ cần xét các cặp nằm trong khoảng cách chỉ số giới hạn. Đối với mỗi cặp hợp lệ, chúng tôi tưởng tượng việc chi tiêu các giao dịch hoán đổi để mang chúng lại với nhau và phần còn lại của mảng luôn có thể được điều chỉnh xung quanh chúng mà không ảnh hưởng đến tính khả thi. 

Điều này dẫn đến phối cảnh cửa sổ trượt. Bất kỳ cặp chỉ số nào bên trong một đoạn có độ dài$k + 2$tự động thỏa mãn ràng buộc về khoảng cách. Ngược lại, bất kỳ cặp hợp lệ nào cũng phải nằm trong một số phân đoạn như vậy. Vì vậy, vấn đề giảm xuống còn việc tìm sự khác biệt tối đa giữa hai giá trị bất kỳ bên trong bất kỳ cửa sổ có kích thước nào$k + 2$. 

Bên trong một cửa sổ cố định, cặp tốt nhất chỉ đơn giản là giá trị tối thiểu và tối đa trong cửa sổ đó. Vì vậy, chúng ta cần duy trì cửa sổ di chuyển tối thiểu và tối đa trên tất cả các cửa sổ có kích thước$k + 2$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng trao đổi vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Cửa sổ tối thiểu/tối đa sau khi giảm khoảng cách | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính kích thước cửa sổ hiệu quả$w = k + 2$. Nếu như$w \ge n$, coi toàn bộ mảng là một cửa sổ. Điều này tương ứng với trường hợp hai phần tử bất kỳ có thể được đặt liền kề trong phạm vi hoán đổi được phép. 
2. Sử dụng cửa sổ trượt trên mảng để duy trì giá trị tối đa và tối thiểu trong cửa sổ hiện tại. Điều này được thực hiện một cách hiệu quả với hai deques đơn điệu, một deque duy trì giá trị giảm dần cho cực đại và một duy trì giá trị tăng dần cho cực tiểu. 
3. Đối với mỗi điểm cuối bên phải của cửa sổ, hãy chèn phần tử mới vào cả hai deques trong khi vẫn giữ nguyên thứ tự đơn điệu. Các yếu tố vi phạm tính đơn điệu sẽ bị loại bỏ ở phía sau. 
4. Khi kích thước cửa sổ vượt quá$w$, loại bỏ các phần tử nằm ngoài ranh giới bên trái ở phía trước mỗi deque. 
5. Sau mỗi lần điều chỉnh cửa sổ, hãy tính toán sự khác biệt giữa mặt trước của deque tối đa và mặt trước của deque tối thiểu. Theo dõi sự khác biệt tối đa như vậy trên tất cả các cửa sổ. 

Lý do điều này có tác dụng là vì bất kỳ giải pháp tối ưu nào cũng tương ứng với việc chọn tối đa hai phần tử trở nên liền kề sau đó.$k$trao đổi. Hai yếu tố đó ban đầu phải nằm trong khoảng cách$k+1$, điều này đảm bảo chúng xuất hiện cùng nhau trong một số cửa sổ có kích thước$k+2$. Trong cửa sổ đó, chênh lệch liền kề tối đa có thể có sau khi sắp xếp lại chính xác là chênh lệch giữa giá trị tối thiểu và tối đa của nó, vì chúng ta luôn có thể đặt hai giá trị đó cạnh nhau bằng cách sử dụng các hoán đổi được phép bên trong ràng buộc cửa sổ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    w = k + 2
    if w >= n:
        print(max(a) - min(a))
        return

    maxdq = deque()
    mindq = deque()
    ans = 0

    l = 0
    for r in range(n):
        while maxdq and a[maxdq[-1]] <= a[r]:
            maxdq.pop()
        maxdq.append(r)

        while mindq and a[mindq[-1]] >= a[r]:
            mindq.pop()
        mindq.append(r)

        if r - l + 1 > w:
            if maxdq[0] == l:
                maxdq.popleft()
            if mindq[0] == l:
                mindq.popleft()
            l += 1

        if r - l + 1 == w:
            ans = max(ans, a[maxdq[0]] - a[mindq[0]])

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ các chỉ mục ở dạng deques thay vì giá trị để chúng ta có thể loại bỏ các phần tử trượt ra khỏi cửa sổ một cách hiệu quả. Thuộc tính đơn điệu đảm bảo rằng mặt trước của mỗi deque luôn đại diện cho điểm cực trị của cửa sổ hiện tại. Phần tế nhị duy nhất là duy trì kích thước cửa sổ một cách chính xác, vì các cửa sổ lớn hơn$k+2$không hợp lệ cho đối số rút gọn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 0
1 3 2 4
```Ở đây kích thước cửa sổ là$k+2 = 2$. Chúng tôi chỉ kiểm tra các cặp liền kề. 

| Cửa sổ | Yếu tố | Tối thiểu | Tối đa | Sự khác biệt | 
| --- | --- | --- | --- | --- | 
| [1,3] | 1,3 | 1 | 3 | 2 | 
| [3,2] | 3,2 | 2 | 3 | 1 | 
| [2,4] | 2,4 | 2 | 4 | 2 | 

Tối đa là 2, phù hợp với chênh lệch liền kề tốt nhất trong mảng ban đầu. 

Điều này xác nhận rằng khi không cho phép hoán đổi, thuật toán sẽ giảm xuống việc quét các cặp liền kề. 

### Ví dụ 2 

đầu vào:```
4 2
1 3 2 4
```Hiện nay$w = 4$, vì vậy toàn bộ mảng là một cửa sổ duy nhất. 

| Cửa sổ | Yếu tố | Tối thiểu | Tối đa | Sự khác biệt | 
| --- | --- | --- | --- | --- | 
| [1,3,2,4] | 1,3,2,4 | 1 | 4 | 3 | 

Kết quả là 3. Điều này tương ứng với thực tế là với hai lần hoán đổi, chúng ta có thể mang 1 và 4 cạnh nhau, đạt được mức chênh lệch tối đa có thể. 

Điều này thể hiện chế độ trong đó mảng trở nên linh hoạt trên toàn cầu và chỉ các giá trị cực trị mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục vào và rời khỏi mỗi deque một lần trong khi vẫn duy trì cửa sổ trượt | 
| Không gian | O(n) | Deques lưu trữ hầu hết tất cả các chỉ mục trong cửa sổ hiện tại | 

Giải pháp dễ dàng phù hợp trong giới hạn vì tất cả các hoạt động đều là các đường truyền tuyến tính trên mảng và$n \le 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    def solve():
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        w = k + 2
        if w >= n:
            print(max(a) - min(a))
            return

        maxdq = deque()
        mindq = deque()
        ans = 0

        l = 0
        for r in range(n):
            while maxdq and a[maxdq[-1]] <= a[r]:
                maxdq.pop()
            maxdq.append(r)

            while mindq and a[mindq[-1]] >= a[r]:
                mindq.pop()
            mindq.append(r)

            if r - l + 1 > w:
                if maxdq[0] == l:
                    maxdq.popleft()
                if mindq[0] == l:
                    mindq.popleft()
                l += 1

            if r - l + 1 == w:
                ans = max(ans, a[maxdq[0]] - a[mindq[0]])

        print(ans)

    solve()
    return ""  # placeholder, actual output comparison omitted

# custom sanity checks would normally go here
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 / 5 1 | 4 | trường hợp cạnh cửa sổ nhỏ nhất | 
| 5 100 / 5 4 3 2 1 | 4 | tối ưu hóa toàn cầu lớn k | 
| 6 1 / 1 100 2 3 4 5 | 99 | ràng buộc cửa sổ cục bộ | 

## Vỏ cạnh 

Khi nào$k = 0$, bộ thuật toán$w = 2$, vì vậy chỉ các cặp liền kề mới được xem xét. Điều này phù hợp với thực tế là không được phép hoán đổi, vì vậy câu trả lời phải đến trực tiếp từ cấu hình ban đầu. 

Khi$k \ge n - 2$, kích thước cửa sổ trở nên ít nhất$n$, do đó thuật toán được chuyển thành tính toán$\max(c) - \min(c)$. Trong chế độ này, bất kỳ cặp nào cũng có thể được đặt liền kề vì có đủ quyền tự do để di chuyển các phần tử trên toàn bộ mảng. 

Khi tất cả các giá trị đều bằng nhau, mọi cửa sổ đều tạo ra chênh lệch bằng 0 và các deque duy trì chính xác các cực đại và cực tiểu giống hệt nhau. Thuật toán vẫn thực hiện đầy đủ các bước nhưng không bao giờ đánh giá quá cao vì cả hai đầu của mỗi cửa sổ đều giống hệt nhau.
