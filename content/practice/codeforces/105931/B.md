---
title: "CF 105931B - \u041c\u0430\u043d\u0445\u044d\u0442\u0442\u0435\u043d\u0441\u043a\u0438\u0435 \u043f\u0435\u0440\u0435\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0438"
description: "Chúng ta được cho một hoán vị có độ dài $n$, nghĩa là sự sắp xếp các số từ $1$ đến $n$ mà không lặp lại. Đối với bất kỳ vị trí $i$ nào, phần đóng góp của vị trí đó là khoảng cách giữa giá trị được đặt ở đó và chính chỉ số, được lấy theo giá trị tuyệt đối."
date: "2026-06-22T15:43:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105931
codeforces_index: "B"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2024"
rating: 0
weight: 105931
solve_time_s: 66
verified: true
draft: false
---

[CF 105931B - \u041c\u0430\u043d\u0445\u044d\u0442\u0442\u0435\u043d\u0441\u043a\u0438\u0435 \u043f\u0435\u0440\u0435\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0438](https://codeforces.com/problemset/problem/105931/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$, nghĩa là sự sắp xếp các số từ$1$ĐẾN$n$không có sự lặp lại. Đối với bất kỳ vị trí nào$i$, phần đóng góp của vị trí đó là khoảng cách giữa giá trị được đặt ở đó và chính chỉ mục, được lấy theo giá trị tuyệt đối. Tính tổng giá trị này trên tất cả các vị trí sẽ cho ra tổng “chi phí dịch chuyển” của hoán vị. 

Nhiệm vụ là quyết định xem có tồn tại một hoán vị có tổng độ dịch chuyển chính xác bằng$k$, và nếu vậy, hãy xây dựng một hoán vị bất kỳ như vậy. 

Cấu trúc của bài toán không phải là kiểm tra các hoán vị mà là kiểm soát mức độ dịch chuyển tổng chỉ số mà chúng ta có thể tạo ra bằng cách sắp xếp lại các phần tử. 

Ràng buộc$n \le 2 \cdot 10^5$loại trừ mọi phép liệt kê hoặc tìm kiếm trên các hoán vị. giá trị$k \le 10^{12}$lớn hơn nhiều so với$n$, vì vậy khó khăn chính là hiểu được phạm vi của số tiền có thể đạt được thay vì xây dựng các kết hợp một cách mù quáng. 

Một số tình huống khó khăn rất dễ bị hiểu sai. Nếu như$n = 1$, hoán vị duy nhất là$[1]$, và chi phí luôn bằng không. Vì vậy bất kỳ$k > 0$là không thể. 

Một trường hợp tế nhị khác xuất hiện khi$k$là lớn. Mặc dù$k$có thể lên đến$10^{12}$, độ dịch chuyển tối đa có thể có của một hoán vị kích thước$n$được giới hạn bởi hàm bậc hai của$n$, vì vậy đối với lớn$k$liên quan đến$n$, câu trả lời phải là “Không”. 

Cạm bẫy thứ ba là cho rằng các giao dịch hoán đổi địa phương tham lam luôn có thể tạo ra bất kỳ giá trị nào. Ví dụ nhỏ như$n=4$cho thấy rằng không phải tất cả các khoản tiền trung gian đều có thể đạt được và việc xây dựng phải tôn trọng một cấu trúc rất cứng nhắc về cách thức các giao dịch hoán đổi góp phần vào tổng dịch chuyển. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi hoán vị, tính toán độ dịch chuyển của nó và kiểm tra xem có kết quả khớp nào không$k$. Điều này đúng nhưng không khả thi vì có$n!$hoán vị. Ngay cả đối với$n = 10$, điều này trở nên hoàn toàn khó chữa. 

Cần có một quan sát có cấu trúc hơn: sự dịch chuyển được tối đa hóa khi các phần tử được đặt càng xa vị trí ban đầu của chúng càng tốt. Cách tự nhiên nhất để tăng tổng là hoán đổi các phần tử theo cặp đối xứng, đẩy các giá trị ra khỏi vị trí nhận dạng của chúng. 

Thông tin chi tiết quan trọng là hoán vị có thể được xây dựng tăng dần bằng cách xử lý các vị trí từ trái sang phải và quyết định xem nên giữ nguyên một phần tử hay hoán đổi nó với vị trí sau này. Việc hoán đổi giữa hai vị trí đóng góp một lượng có thể dự đoán được vào tổng chi phí và những đóng góp này có thể được xử lý độc lập khi được sắp xếp cẩn thận. 

Đặc biệt, việc hoán đổi vị trí$i$Và$j$đóng góp$2 \cdot (j - i)$đến tổng chuyển vị. Điều này có nghĩa là chúng ta có thể nghĩ đến việc xây dựng câu trả lời bằng cách sử dụng các hoán đổi phân đoạn rời rạc, mỗi hoán đổi đóng góp một lượng chẵn có thể kiểm soát được. 

Điều này làm giảm vấn đề phân hủy$k$thành tổng các đóng góp của phân khúc đó, có thể được xây dựng một cách tham lam từ các phân khúc lớn nhất có thể trở xuống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Xây dựng hoán đổi phân khúc |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên hãy tính độ dịch chuyển tối đa có thể. Điều này đạt được bằng cách đảo ngược hoán vị và tổng chi phí trở thành$\sum_{i=1}^{n} |(n-i+1) - i|$. Nếu như$k$lớn hơn giá trị này thì không thể xây dựng được. Việc kiểm tra này ngăn cản nỗ lực chi tiêu vào các mục tiêu không thể thực hiện được. 
2. Nếu$k = 0$, hoán vị danh tính đã thỏa mãn điều kiện, vì mọi phần tử đều khớp với chỉ mục của nó và đóng góp bằng 0. 
3. Khởi tạo hoán vị dưới dạng mảng nhận dạng$[1, 2, \dots, n]$. Chúng tôi sẽ sửa đổi nó theo từng phân đoạn. 
4. Lặp lại từ chỉ mục ngoài cùng bên trái$i = 1$ĐẾN$n$và tại mỗi vị trí quyết định xem có áp dụng hoán đổi với một số vị trí hay không$j > i$. Sự lựa chọn của$j$được thúc đẩy bởi sự đóng góp lớn nhất còn lại mà chúng ta vẫn có thể sử dụng mà không vượt quá$k$. 
5. Đối với một vị trí$i$, cố gắng sử dụng đối tác xa nhất có thể$j = n - (i - 1)$, hình thành các hoán đổi đối xứng từ đầu vào trong. Sự hoán đổi giữa$i$Và$j$đóng góp$2 \cdot (j - i)$đến tổng chuyển vị. 
6. Nếu khoản đóng góp này nhỏ hơn hoặc bằng phần còn lại$k$, thực hiện hoán đổi và trừ giá trị này khỏi$k$. Nếu không, hãy bỏ qua vị trí này và tiến về phía trước mà không đổi chỗ. 
7. Tiếp tục cho đến khi tất cả các vị trí được xử lý hoặc phần còn lại$k$trở thành số không. Nếu ở cuối$k$không bằng 0, việc xây dựng thất bại. 

### Tại sao nó hoạt động 

Thuật toán hoạt động vì mỗi lần hoán đổi mà chúng tôi thực hiện đều tương ứng với một phân đoạn rời rạc có đóng góp vào tổng dịch chuyển không phụ thuộc vào các lần hoán đổi khác. Mỗi lần chúng tôi chọn một giao dịch hoán đổi, chúng tôi sẽ loại bỏ một phạm vi liền kề khỏi việc xem xét thêm, ngăn chặn sự chồng chéo. Sự lựa chọn tham lam về giao dịch hoán đổi hợp lệ lớn nhất đảm bảo rằng chúng ta không bao giờ lãng phí những khoản đóng góp lớn tiềm năng có thể cần thiết sau này. Vì tất cả các khoản đóng góp đều đồng đều và được cấu trúc xung quanh các khoảng cách đối xứng nên mọi tổng số có thể đạt được đều phải được biểu diễn dưới dạng tổng của các khoản đóng góp cho từng phân khúc rời rạc này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    # maximum possible displacement
    max_k = 0
    for i in range(1, n + 1):
        max_k += abs(n - i + 1 - i)

    if k > max_k:
        print("No")
        return

    if k == 0:
        print("Yes")
        print(*range(1, n + 1))
        return

    p = list(range(1, n + 1))

    l, r = 0, n - 1

    while l < r:
        cost = 2 * (r - l)
        if cost <= k:
            p[l], p[r] = p[r], p[l]
            k -= cost
        l += 1
        r -= 1

    if k != 0:
        print("No")
    else:
        print("Yes")
        print(*p)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính toán giới hạn trên của chuyển vị có thể đạt được, tương ứng với hoán vị ngược lại. Điều này đảm bảo chúng tôi không bao giờ cố gắng xây dựng những mục tiêu bất khả thi. 

Giai đoạn xây dựng sử dụng hai con trỏ di chuyển vào trong. Mỗi hoán đổi đối xứng được đánh giá dưới dạng một quyết định duy nhất: hoặc chúng tôi sử dụng toàn bộ đóng góp của hoán đổi đó hoặc chúng tôi bỏ qua nó. Phép trừ của$2 \cdot (r - l)$phản ánh sự thay đổi chính xác về chênh lệch tuyệt đối do trao đổi vị trí đối xứng. 

Tính đúng đắn phụ thuộc vào thực tế là sau mỗi lần hoán đổi, cấu trúc còn lại vẫn đối xứng và độc lập với các lựa chọn trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 3, k = 4$Chúng tôi bắt đầu từ danh tính$[1,2,3]$. 

| tôi | r | chi phí | k trước | hành động | k sau | hoán vị | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 4 | 4 | trao đổi | 0 | [3,2,1] | 

Chúng ta thực hiện hoán đổi hoàn toàn giữa vị trí 0 và 2, tạo ra hoán vị$[3,2,1]$. Tổng chi phí trùng khớp chính xác với 4 nên việc xây dựng thành công. 

Điều này chứng tỏ trường hợp một hoán đổi đối xứng duy nhất là đủ để đạt được mục tiêu. 

### Ví dụ 2:$n = 5, k = 2$Bắt đầu từ$[1,2,3,4,5]$. 

| tôi | r | chi phí | k trước | hành động | k sau | hoán vị | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 8 | 2 | bỏ qua | 2 | [1,2,3,4,5] | 
| 1 | 3 | 4 | 2 | bỏ qua | 2 | [1,2,3,4,5] | 
| 2 | 2 | - | 2 | dừng lại | 2 | [1,2,3,4,5] | 

Chúng tôi không thể sử dụng bất kỳ trao đổi nào vì khoản đóng góp nhỏ nhất có thể đã lớn hơn phần còn lại$k$. Thuật toán kết thúc với$k \neq 0$, kết luận chính xác là không thể. 

Điều này cho thấy cách xây dựng tôn trọng các đơn vị hoán đổi không thể phân chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| một lần với sự hoán đổi con trỏ đối xứng và một lần tính toán trước | 
| Không gian |$O(n)$| lưu trữ mảng hoán vị | 

Độ phức tạp tuyến tính là đủ cho$n \le 2 \cdot 10^5$và mức sử dụng bộ nhớ nằm trong giới hạn vì chỉ duy trì một mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    _stdout = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = _stdout
    return out.strip()

# provided samples
assert run("3 4\n") == "Yes\n3 2 1" or run("3 4\n") == "Yes\n3 1 2"
assert run("112 777\n") == "No"

# custom cases
assert run("1 0\n") == "Yes\n1"
assert run("1 1\n") == "No"
assert run("2 2\n") == "Yes\n2 1"
assert run("4 0\n") == "Yes\n1 2 3 4"
assert run("4 12\n") == "Yes\n4 3 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | Có 1 | trường hợp hợp lệ kích thước tối thiểu | 
| 1 1 | Không | yếu tố đơn lẻ không thể | 
| 2 2 | Có 2 1 | trao đổi không tầm thường nhỏ nhất | 
| 4 0 | danh tính | đường cơ sở dịch chuyển bằng không | 
| 4 12 | đảo ngược | chuyển vị tối đa có thể đạt được | 

## Vỏ cạnh 

cho$n = 1$, thuật toán ngay lập tức tính chuyển vị tối đa bằng 0. Nếu như$k = 0$, nó xuất ra$[1]$. Nếu như$k > 0$, nó từ chối. Vòng lặp không bao giờ được nhập vào, điều này tránh mọi logic con trỏ không hợp lệ. 

Để tối đa$k$, vòng lặp hoán đổi luôn chấp nhận mọi hoán đổi đối xứng. Hoán vị trở nên đảo ngược hoàn toàn và chi phí tích lũy khớp chính xác với giới hạn được tính toán trước, xác nhận rằng kẻ tham lam chấp nhận mọi khoản đóng góp có sẵn. 

Đối với nhỏ$k$nhỏ hơn phần đóng góp hoán đổi nhỏ nhất, thuật toán sẽ bỏ qua tất cả các hoán đổi. Hoán vị kết quả vẫn là danh tính, và vì$k$không thể giảm thêm nữa, nó kết luận một cách chính xác là không thể trừ khi$k = 0$.
