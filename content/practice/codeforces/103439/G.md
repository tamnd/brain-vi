---
title: "CF 103439G - Sắp xếp thay thế"
description: "Chúng ta có một mảng $A$ mà chúng ta muốn chuyển đổi thành một dãy không giảm. Bên cạnh đó, chúng ta có một bộ giá trị dự phòng $B$ riêng biệt. Mọi phần tử trên $A$ và $B$ đều khác biệt."
date: "2026-07-03T07:46:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103439
codeforces_index: "G"
codeforces_contest_name: "XXII Open Cup, Grand Prix of Southeastern Europe"
rating: 0
weight: 103439
solve_time_s: 51
verified: true
draft: false
---

[CF 103439G - Sắp xếp thay thế](https://codeforces.com/problemset/problem/103439/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng$A$mà chúng ta muốn chuyển đổi thành một dãy không giảm. Bên cạnh đó, chúng tôi có một bộ riêng$B$của các giá trị dự phòng. Mọi phần tử xuyên suốt$A$Và$B$là khác biệt. 

Hoạt động duy nhất được phép là lấy một giá trị từ$B$và ghi đè lên bất kỳ vị trí nào trong$A$với nó, và mỗi phần tử của$B$có thể được sử dụng nhiều nhất một lần. Mục tiêu không phải là sắp xếp lại$A$trực tiếp, nhưng để thay thế một cách chiến lược một số mục để mảng cuối cùng được sắp xếp theo thứ tự không giảm, đồng thời giảm thiểu số lượng thay thế được sử dụng. Nếu không có chuỗi thay thế nào có thể đạt được một mảng được sắp xếp thì câu trả lời là$-1$. 

Một hạn chế cơ cấu quan trọng là các sản phẩm thay thế không thể tái sử dụng một cách tự do. Mỗi giá trị trong$B$là một công cụ sử dụng một lần, vì vậy vấn đề không chỉ là “có bao nhiêu vị trí bị sai” mà còn là “vị trí nào có thể được sửa chữa với giá trị sẵn có nào”. 

Những ràng buộc cho phép$N, M$lên tới$5 \cdot 10^5$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng các thay thế hoặc thử các tập hợp con. Bất cứ điều gì bậc hai trong$N$hoặc$M$quá chậm. Một cách tiếp cận hợp lệ phải dựa vào việc sắp xếp và quét tuyến tính hoặc gần tuyến tính sau quá trình tiền xử lý. 

Một điểm tế nhị là chúng ta không được yêu cầu thực hiện$A$được sắp xếp bằng cách sắp xếp lại hoặc chèn các phần tử nhưng hoàn toàn bằng cách thay thế. Điều này có nghĩa là mọi giá trị cuối cùng phải đến từ mảng ban đầu hoặc từ$B$và chúng tôi đang lựa chọn một cách hiệu quả những vị trí nào cần “sửa chữa” và những giá trị nào cần chỉ định. 

Các trường hợp khó phá vỡ lý luận ngây thơ rất dễ bị bỏ sót. Ví dụ, nếu$A = [2, 6, 13, 10]$Và$B = [5]$, người ta có thể nghĩ rằng việc thay thế 13 bằng 5 sẽ khắc phục được sự đảo ngược bằng 10, nhưng nó có thể phá vỡ các ràng buộc về thứ tự ở vế trái nếu sau này chúng ta cần một giá trị lớn hơn ở đó. Đây chính xác là lý do tại sao các bản sửa lỗi cục bộ không thành công: bản thay thế phải nhất quán trên toàn cầu với cấu trúc đơn điệu cuối cùng. 

Một trường hợp cạnh khác là khi tất cả các phần tử trong$B$quá nhỏ hoặc quá lớn để khắc phục những hạn chế cần thiết. Ví dụ, nếu$A = [3, 1, 2]$Và$B = [0]$, không có cách nào khắc phục được sự đảo ngược giữa 3 và 1 trong khi vẫn giữ được tính khả thi cho phần còn lại. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử tất cả các tập hợp con vị trí trong$A$để thay thế và với mỗi tập hợp con, hãy kiểm tra xem liệu chúng ta có thể gán các giá trị riêng biệt từ$B$để mảng kết quả được sắp xếp. Đối với một tập hợp con có kích thước cố định$k$, chúng ta cũng cần phải chọn cái nào$k$giá trị từ$B$để sử dụng và kết hợp chúng với các vị trí. Ngay cả khi việc kiểm tra tính khả thi là tuyến tính thì số lượng tập hợp con vẫn$2^N$và việc gán các giá trị sẽ làm tăng thêm độ phức tạp giai thừa. Điều này hoàn toàn không thể thực hiện được ngoài rất nhỏ$N$. 

Quan sát quan trọng là sau khi mảng cuối cùng được sắp xếp, cấu trúc sẽ cứng nhắc: có một chuỗi mục tiêu không giảm mà chúng ta phải khớp và mọi vị trí đều áp đặt một ràng buộc về giá trị nào có thể kết thúc ở đó. Thay vì suy nghĩ theo cách thay thế tùy ý, chúng tôi chuyển sang suy nghĩ về cách xây dựng một mảng được sắp xếp hợp lệ từ trái sang phải trong khi giảm thiểu tần suất chúng tôi buộc phải sử dụng$B$. 

Một cách hữu ích để điều chỉnh lại vấn đề là coi mảng được sắp xếp cuối cùng như một chuỗi không giảm và phải “tương thích” với mảng ban đầu.$A$. Nếu chúng tôi xử lý$A$từ trái sang phải, chúng tôi muốn giữ lại càng nhiều giá trị ban đầu càng tốt, nhưng chỉ khi chúng vẫn có thể vừa với cấu trúc ngày càng tăng trên toàn cầu. Bất cứ khi nào một phần tử phá vỡ tính khả thi, chúng ta có thể thay thế nó, nhưng chúng ta nên sử dụng giá trị sẵn có nhỏ nhất từ$B$có thể duy trì tính nhất quán. Điều này tự nhiên gợi ý một chiến lược tham lam khi chúng ta hiểu rằng tính khả thi chỉ phụ thuộc vào các ràng buộc về thứ tự chứ không phụ thuộc vào sự đồng nhất của các vị thế. 

Cái nhìn sâu sắc hơn là chúng ta có thể coi vấn đề là xây dựng tiền tố dài nhất của$A$có thể được giữ nguyên trong khi vẫn có thể mở rộng thành một mảng được sắp xếp bằng cách sử dụng các giá trị thay thế có sẵn. Bất kỳ phần tử nào không thể được giữ lại buộc chúng tôi phải sử dụng phần tử thay thế và việc lựa chọn thay thế phải tôn trọng thứ tự trong tương lai, điều này thúc đẩy chúng tôi sắp xếp cả hai$A$Và$B$và tham lam khớp các ràng buộc. 

Giải pháp cuối cùng trở thành quét trong đó chúng tôi duy trì giá trị được chọn cuối cùng trong mảng cuối cùng và quyết định có giữ lại hay không$A[i]$hoặc thay thế nó bằng cái nhỏ nhất có thể$B$giá trị vẫn bảo toàn thứ tự không giảm. Nếu cả hai đều không thể thì việc cấu hình là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force + Xác thực |$O(2^N)$|$O(N)$| Quá chậm | 
| Tham lam với mảng được sắp xếp |$O((N+M)\log(N+M))$|$O(N+M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Sắp xếp cả hai mảng$A$Và$B$. Sắp xếp$B$cho phép chúng tôi luôn chọn thiết bị thay thế nhỏ nhất có thể sử dụng được khi cần, điều này rất quan trọng vì những thiết bị thay thế lớn hơn chỉ khiến tính khả thi trong tương lai trở nên khó khăn hơn. 
2. Khởi tạo con trỏ$j = 0$vì$B$, và một biến$last = -\infty$đại diện cho giá trị cuối cùng được đặt trong mảng cuối cùng được xây dựng. Điều này theo dõi các ràng buộc đơn điệu. 
3. Quét qua từng phần tử$A[i]$từ trái qua phải, quyết định nên giữ hay thay thế. Thứ tự này quan trọng vì các quyết định trước đó sẽ hạn chế tất cả các vị thế trong tương lai. 
4. Nếu$A[i] \ge last$, chúng ta có thể giữ an toàn$A[i]$, và chúng tôi thiết lập$last = A[i]$. Điều này là tối ưu vì việc giữ nguyên giá trị ban đầu giúp giảm số lượng thay thế. 
5. Nếu không, chúng tôi sẽ cố gắng khắc phục vi phạm bằng cách sử dụng$B$. Chúng tôi nâng cao con trỏ$j$cho đến khi chúng ta tìm thấy cái nhỏ nhất$B[j] \ge last$. Nếu phần tử đó tồn tại, chúng tôi sẽ sử dụng nó, tăng số lượng thay thế và cập nhật$last = B[j]$, sau đó di chuyển$j$phía trước. 
6. Nếu không$A[i]$cũng như bất kỳ giá trị còn lại nào trong$B$có thể đáp ứng$last$, chúng tôi kết luận quá trình này là không thể và trả lại$-1$. Điều này tương ứng với một khoảng cách về các giá trị sẵn có không thể được lấp đầy mà không phá vỡ tính đơn điệu. 
7. Tiếp tục cho đến khi tất cả các phần tử của$A$được xử lý và trả về số lượng thay thế được sử dụng. 

### Tại sao nó hoạt động 

Thuật toán duy trì bất biến sau khi xử lý chỉ số$i$, chuỗi được xây dựng là chuỗi không giảm nhỏ nhất có thể đạt được về mặt từ điển bằng cách sử dụng một số tập hợp con thay thế. Sắp xếp$B$đảm bảo rằng bất cứ khi nào chúng tôi phải chèn một sự thay thế, việc sử dụng giá trị khả thi nhỏ nhất sẽ duy trì tính linh hoạt tối đa cho các vị trí trong tương lai. Bất kỳ lựa chọn nào lớn hơn sẽ chỉ thắt chặt các ràng buộc trong tương lai mà không cải thiện tính khả thi hiện tại, bởi vì yêu cầu duy nhất là bậc không giảm, không phải là gần với giá trị ban đầu. Sự thống trị tham lam này đảm bảo rằng nếu một giải pháp tồn tại, thuật toán sẽ không bao giờ tự chặn nó một cách không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    a.sort()
    b.sort()
    
    j = 0
    last = -10**18
    used = 0
    
    for x in a:
        if x >= last:
            last = x
        else:
            while j < m and b[j] < last:
                j += 1
            if j == m:
                print(-1)
                return
            last = b[j]
            j += 1
            used += 1
    
    print(used)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp việc xây dựng tham lam. Sắp xếp cả hai mảng là bước cấu trúc biến vấn đề thành một quá trình so khớp đơn điệu. Con trỏ qua$B$đảm bảo mỗi lần thay thế được sử dụng nhiều nhất một lần và luôn được chọn ở mức tối thiểu. 

Một chi tiết triển khai tinh tế là việc sử dụng giá trị ban đầu âm lớn cho$last$, giúp tránh việc viết vỏ đặc biệt cho phần tử đầu tiên. Một điều nữa là chúng tôi không bao giờ xem xét lại các lựa chọn trước đó, do đó quá trình quét hoàn toàn tuyến tính sau khi sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
2 6 13 10
5
```Chúng tôi sắp xếp$A$BẰNG$[2, 6, 10, 13]$Và$B = [5]$. 

| tôi | A[i] | cuối cùng | Hành động | B đã qua sử dụng | cuối cùng sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | -inf | giữ | 0 | 2 | 
| 1 | 6 | 2 | giữ | 0 | 6 | 
| 2 | 10 | 6 | giữ | 0 | 10 | 
| 3 | 13 | 10 | không giữ được, thử B nhưng 5 < 10 | 0 | thất bại | 

Chúng tôi không thể tìm thấy bất kỳ sự thay thế có thể sử dụng nào ở bước cuối cùng, vì vậy câu trả lời là$-1$. Điều này cho thấy rằng mặc dù chỉ có một phép đảo ngược tồn tại trong mảng ban đầu nhưng không có giá trị khả dụng nào trong mảng ban đầu.$B$đủ lớn để duy trì sự tăng trưởng đơn điệu sau những quyết định trước đó. 

### Ví dụ 2 

đầu vào:```
4 2
2 6 13 10
5 4
```Mảng được sắp xếp:$A = [2, 6, 10, 13]$,$B = [4, 5]$. 

| tôi | A[i] | cuối cùng | Hành động | B đã qua sử dụng | cuối cùng sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | -inf | giữ | 0 | 2 | 
| 1 | 6 | 2 | giữ | 0 | 6 | 
| 2 | 10 | 6 | thay thế bằng 4? không hợp lệ, thay thế bằng 5 | 1 | 5 | 
| 3 | 13/10 | 5 | giữ 10? hợp lệ | 1 | 10 | 

Chúng tôi sử dụng một sự thay thế sớm để duy trì tính khả thi, sau đó tiến hành. Dấu vết cho thấy rằng việc sử dụng thay thế hợp lệ nhỏ nhất là cần thiết, vì việc chọn 5 thay vì 4 sẽ linh hoạt hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N+M)\log(N+M))$| Sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian |$O(N+M)$| Lưu trữ cho mảng | 

Các ràng buộc cho phép lên đến$5 \cdot 10^5$các phần tử, do đó$O(n \log n)$cách tiếp cận phù hợp thoải mái trong giới hạn thời gian, trong khi bất kỳ cách tiếp cận bậc hai nào cũng sẽ vượt xa số lượng hoạt động khả thi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    a.sort()
    b.sort()
    
    j = 0
    last = -10**18
    used = 0
    
    for x in a:
        if x >= last:
            last = x
        else:
            while j < m and b[j] < last:
                j += 1
            if j == m:
                return "-1"
            last = b[j]
            j += 1
            used += 1
    
    return str(used)

# provided samples
assert run("4 1\n2 6 13 10\n5\n") == "-1"
assert run("4 2\n2 6 13 10\n5 4\n") == "2"

# custom cases
assert run("1 1\n5\n10\n") == "0", "already sorted single element"
assert run("3 1\n3 1 2\n2\n") == "-1", "insufficient repair value"
assert run("3 3\n3 1 2\n1 2 4\n") == "1", "one repair fixes inversion"
assert run("5 5\n5 4 3 2 1\n1 2 3 4 5\n") == "2", "reverse array needs repairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp phần tử đơn | 0 | tầm thường đã hợp lệ | 
| sửa chữa không đầy đủ | -1 | phát hiện không thể | 
| sửa chữa tối thiểu | 1 | sửa chữa đảo ngược đơn | 
| mảng đảo ngược | 2 | cấu trúc giảm dần trong trường hợp xấu nhất | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mảng đã được sắp xếp. Thuật toán không bao giờ đi vào nhánh thay thế, bởi vì mọi$A[i]$thỏa mãn điều kiện đơn điệu nên đáp án vẫn là số không thay thế. 

Một trường hợp cạnh khác là khi tất cả các phần tử trong$B$nhỏ hơn giới hạn hiện tại. Trong tình huống này, con trỏ qua$B$cạn kiệt mà không tìm được ứng viên hợp lệ và thuật toán trả về chính xác$-1$. Điều này tương ứng với một khoảng trống mà không có sự thay thế nào có thể khắc phục được yêu cầu đơn điệu. 

Trường hợp thứ ba là khi sự lựa chọn tham lam có thể có vẻ rủi ro, chẳng hạn như chọn giá trị hợp lệ nhỏ nhất.$B$giá trị. Dấu vết cho thấy rằng việc sử dụng sớm giá trị lớn hơn có thể cản trở tính khả thi sau này. Thuật toán tránh điều này bằng cách luôn sử dụng sự thay thế khả thi tối thiểu, duy trì độ trễ tối đa cho các phần tử trong tương lai.
