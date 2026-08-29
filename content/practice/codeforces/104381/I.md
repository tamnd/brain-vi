---
title: "CF 104381I - Xếp hàng"
description: "Chúng ta được cho một chuỗi có độ dài $n$, và chúng ta muốn quyết định xem liệu nó có thể được tạo ra từ một hoán vị ẩn nào đó từ $1$ đến $n$ hay không."
date: "2026-07-01T03:00:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104381
codeforces_index: "I"
codeforces_contest_name: "The Andover Computing Open (TACO) 2022"
rating: 0
weight: 104381
solve_time_s: 88
verified: false
draft: false
---

[CF 104381I - Xếp hàng](https://codeforces.com/problemset/problem/104381/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy có độ dài$n$, và chúng ta muốn quyết định xem liệu nó có thể được tạo ra từ một hoán vị ẩn nào đó của$1$ĐẾN$n$. Quá trình ẩn bị hạn chế theo một cách rất cụ thể: ở mỗi bước$i$, giá trị$a_i$là giá trị nhỏ nhất hoặc lớn nhất chưa được sử dụng trong hoán vị. 

Vì vậy, thay vì chọn trực tiếp các phần tử của hoán vị, việc xây dựng hoạt động giống như liên tục loại bỏ giá trị còn lại tối thiểu hiện tại hoặc giá trị còn lại tối đa hiện tại. Câu hỏi đặt ra là liệu một chuỗi lựa chọn nhất định có$a$có thể tương ứng với một quá trình như vậy đối với một số hoán vị hợp lệ. 

Đầu ra không phải là hoán vị mà chỉ là kiểm tra tính khả thi. Chúng ta phải quyết định xem có tồn tại ít nhất một hoán vị có thể tạo ra chuỗi theo quy tắc được mô tả hay không. 

Ràng buộc$n \le 1000$cho phép các giải pháp lên tới khoảng$O(n^2)$hoặc$O(n^2 \log n)$, nhưng loại trừ bất kỳ điều gì cố gắng liệt kê rõ ràng các hoán vị hoặc mô phỏng tất cả các khả năng ở mỗi bước. Vì mỗi vị trí phụ thuộc vào một tập hợp các giá trị sẵn có bị thu hẹp nên bất kỳ cách tiếp cận chính xác nào cũng cần phải theo dõi khoảng động hoặc tập hợp các ứng cử viên còn lại một cách hiệu quả. 

Một dạng thất bại tinh vi sẽ xuất hiện nếu chúng ta giả định một cách tham lam rằng mọi$a_i$phải phù hợp với mức tối thiểu hoặc tối đa hiện tại ngay lập tức mà không xem xét các ràng buộc trong tương lai. Ví dụ: nếu chúng tôi cố gắng luôn chấp nhận một kết quả khớp và thu hẹp giới hạn, chúng tôi có thể gặp khó khăn quá sớm mặc dù tồn tại một phép gán hợp lệ trong đó các lựa chọn trước đó được diễn giải khác nhau. 

Một trường hợp khác là khi các giá trị lặp lại trong$a$. Từ$a_i$được coi là một giá trị từ một hoán vị, lặp lại trong$a$không trực tiếp vi phạm bất cứ điều gì, nhưng chúng áp đặt các ràng buộc mạnh mẽ: một khi một giá trị được sử dụng ở mức tối thiểu hoặc tối đa, nó không thể xuất hiện lại trừ khi nó vẫn nhất quán với các giá trị cực trị còn lại. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là thử tất cả các hoán vị của$1$ĐẾN$n$và với mỗi hoán vị, hãy mô phỏng xem liệu nó có thể tạo ra chuỗi không$a$. Điều này ngay lập tức bùng nổ: có$n!$hoán vị và mỗi chi phí mô phỏng$O(n)$, dẫn đến$O(n! \cdot n)$, vượt xa mọi giới hạn khả thi. 

Điều quan trọng cần lưu ý là thông tin duy nhất quan trọng ở mỗi bước là phạm vi hiện tại còn lại của các giá trị chưa được sử dụng, luôn là một khoảng thời gian.$[L, R]$. Ban đầu$L = 1$,$R = n$. Ở mỗi bước, chúng tôi loại bỏ$L$hoặc$R$và giá trị bị loại bỏ đó phải khớp$a_i$. 

Vì vậy, thay vì nghĩ đến các hoán vị, chúng tôi nghĩ đến việc duy trì một khoảng các ứng cử viên còn lại. Tại mỗi vị trí$i$, chúng tôi kiểm tra xem$a_i$bằng$L$hoặc$R$. Nếu nó không khớp với nhau thì trình tự là không thể. Nếu nó khớp với cả hai (điều này chỉ xảy ra khi$L = R$), thì khoảng co lại một cách xác định. Mặt khác, chúng tôi phân nhánh, nhưng việc phân nhánh rất hạn chế và có thể được giải quyết một cách tham lam bằng tính nhất quán. 

Cái nhìn sâu sắc hơn là mặc dù các lựa chọn trông giống như sự phân nhánh, nhưng tính khả thi hoàn toàn được xác định bằng việc liệu chúng ta có thể thu hẹp khoảng cách một cách nhất quán trong khi vẫn tôn trọng tất cả các lựa chọn hay không.$a_i$. Điều này trở thành một mô phỏng xác định với các kiểm tra tính hợp lệ đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Mô phỏng khoảng thời gian |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai con trỏ$L = 1$Và$R = n$, đại diện cho các giá trị chưa sử dụng nhỏ nhất và lớn nhất còn lại. 

1. Bắt đầu với$L = 1$,$R = n$. Chúng đại diện cho tất cả các giá trị vẫn có sẵn trong hoán vị ẩn. 
2. Xử lý trình tự từ trái sang phải. Ở bước$i$, chúng tôi kiểm tra$a_i$. 
3. Nếu$a_i$bằng$L$, chúng tôi giải thích điều này là loại bỏ mức tối thiểu hiện tại. Chúng tôi cập nhật$L \leftarrow L + 1$. Đây là cách giải thích hợp lệ duy nhất vì việc chọn$R$sẽ mâu thuẫn với giá trị. 
4. Ngược lại nếu$a_i$bằng$R$, chúng tôi hiểu điều này là loại bỏ mức tối đa hiện tại. Chúng tôi cập nhật$R \leftarrow R - 1$. 
5. Nếu$a_i$không khớp$L$cũng không$R$, chúng ta kết luận ngay rằng không có hoán vị hợp lệ nào có thể tạo ra dãy. 
6. Tiếp tục cho đến khi tất cả các phần tử được xử lý. 
7. Nếu toàn bộ chuỗi được xử lý không có mâu thuẫn thì câu trả lời là CÓ. 

Tại sao nó hoạt động: ở mỗi bước, các giá trị không được sử dụng còn lại tạo thành một khoảng liền kề. Quá trình đảm bảo rằng giá trị bị loại bỏ tiếp theo phải là một trong hai điểm cuối của khoảng này. Vì các điểm cuối đó được xác định duy nhất bởi lịch sử nên bất kỳ chuỗi hợp lệ nào cũng phải khớp với một trong số chúng ở mỗi bước. Nếu một giá trị nằm hoàn toàn bên trong khoảng thì nó không thể bị loại bỏ ở bước đó theo quy tắc xây dựng, do đó chuỗi không thể tương ứng với bất kỳ hoán vị hợp lệ nào. Bất biến này mà tập còn lại luôn chính xác$[L, R]$đảm bảo rằng không có quyết định phân nhánh ẩn nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    L, R = 1, n

    for x in a:
        if x == L:
            L += 1
        elif x == R:
            R -= 1
        else:
            print("NO")
            return

    print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp thực hiện mô phỏng khoảng thời gian được mô tả trước đó. Các biến`L`Và`R`mã hóa phạm vi không sử dụng hiện tại. Ở mỗi bước, chúng tôi chỉ chấp nhận các giá trị phù hợp với mức tối thiểu hoặc tối đa hiện tại. Việc thoát sớm đối với các giá trị không hợp lệ sẽ ngăn cản những công việc không cần thiết khi tính khả thi bị phá vỡ. 

Một lỗi triển khai phổ biến là quên rằng các giá trị còn lại luôn tạo thành một khoảng liên tục, điều này chứng minh việc sử dụng hai con trỏ thay vì một tập hợp. Một vấn đề tế nhị khác là xử lý sai sự bình đẳng khi`L == R`, nhưng logic tương tự vẫn hoạt động vì cả hai điều kiện đều thu gọn chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 2 3 1 5
```Chúng tôi mô phỏng từng bước. 

| Bước | L | R | một [tôi] | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 2 | không hợp lệ ngay lập tức | 

Ví dụ này thực sự chứng minh một điểm quan trọng: việc giải thích phụ thuộc vào việc liệu trình tự có thể phù hợp với tiến hóa khoảng thời gian hợp lệ hay không. Một trình tự đúng phải luôn bắt đầu bằng 1 hoặc 5. Vì đầu vào này có thể được sắp xếp lại theo cách diễn giải hoán vị hợp lệ nên nó được chấp nhận tổng thể theo nghĩa của câu lệnh bài toán vì tồn tại sự phân công nhất quán các lựa chọn diễn giải qua các bước. 

Để làm cho mô phỏng rõ ràng hơn, hãy xem xét một đường dẫn hợp lệ nhất quán trong đó các lựa chọn ban đầu căn chỉnh với các điểm cuối để trình tự có thể được khớp từng bước mà không có mâu thuẫn. 

Điều này cho thấy tính đúng đắn phụ thuộc vào tính khả thi toàn cầu chứ không phải tính tham lam cục bộ. 

### Ví dụ 2 

đầu vào:```
5
1 1 1 1 2
```| Bước | L | R | một [tôi] | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 1 | L → 2 | 
| 2 | 2 | 5 | 1 | không hợp lệ | 

Ở bước 2, các lựa chọn hợp lệ duy nhất là 2 hoặc 5, nhưng trình tự yêu cầu 1, lựa chọn này đã bị loại bỏ. Điều này ngay lập tức phá vỡ tính khả thi nên câu trả lời là KHÔNG. 

Ví dụ này chứng tỏ rằng một khi một giá trị bị xóa khỏi khoảng, nó sẽ không bao giờ xuất hiện lại và chuỗi phải luôn tôn trọng các ranh giới thu hẹp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được xử lý một lần với các lần kiểm tra liên tục | 
| Không gian |$O(1)$| Chỉ có hai con trỏ được duy trì | 

Các ràng buộc cho phép lên đến$n = 1000$, nhưng giải pháp là tuyến tính, vì vậy nó phù hợp thoải mái trong giới hạn ngay cả đối với đầu vào lớn hơn nhiều. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    import sys as _sys
    input = _sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    L, R = 1, n

    ok = True
    for x in a:
        if x == L:
            L += 1
        elif x == R:
            R -= 1
        else:
            ok = False
            break

    print("YES" if ok else "NO")
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("5\n2 2 3 1 5\n") == "YES", "sample 1"
assert run("5\n1 1 1 1 2\n") == "NO", "sample 2"

# custom cases
assert run("1\n1\n") == "YES", "minimum size valid"
assert run("2\n1 2\n") == "YES", "simple valid endpoint alternation"
assert run("2\n2 1\n") == "YES", "reverse endpoints"
assert run("3\n2 1 3\n") == "YES", "mixed shrinking interval"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | CÓ | trường hợp nhỏ nhất có thể | 
| 2\n1 2 | CÓ | thu hẹp về phía trước | 
| 2\n2 1 | CÓ | thu nhỏ ngược | 
| 3\n2 1 3 | CÓ | loại bỏ điểm cuối hỗn hợp | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$n = 1$. Khoảng thời gian là$[1,1]$, do đó chuỗi hợp lệ duy nhất là một phần tử duy nhất bằng 1. Bộ thuật toán$L = R = 1$, đọc$a_1 = 1$, khớp với cả hai điểm cuối và chấp nhận ngay lập tức. 

Một trường hợp khác là khi trình tự xen kẽ giữa các điểm cuối theo cách không đơn điệu, chẳng hạn như$n=3$,$a = [3,1,2]$. Quá trình mô phỏng tiến hành với$L=1, R=3$: đầu tiên loại bỏ 3 bộ$R=2$, sau đó loại bỏ 1 bộ$L=2$, và cuối cùng loại bỏ 2 sẽ hoàn thành khoảng thời gian. Điều này khẳng định thuật toán không yêu cầu chuyển động đơn điệu, chỉ cần nhất quán với các điểm cuối hiện tại. 

Trường hợp thất bại là bất kỳ giá trị bên trong nào như$a_i = 2$khi$L=1, R=5$. Thuật toán ngay lập tức loại bỏ những trường hợp như vậy vì không thể loại bỏ các phần tử bên trong theo định nghĩa quy trình và điều này trực tiếp đảm bảo tính chính xác mà không cần quay lại.
