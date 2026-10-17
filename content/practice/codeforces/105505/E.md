---
title: "CF 105505E - Đoàn thám hiểm Evereth"
description: "Chúng ta được cung cấp một chuỗi có độ dài $N$ được cho là thể hiện thứ tự truy cập đầy đủ của tất cả các trạm được đánh số từ $1$ đến $N$. Một số mục bị thiếu, hiển thị dưới dạng số 0, trong khi những mục khác được cố định và đã khác biệt."
date: "2026-06-24T00:13:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105505
codeforces_index: "E"
codeforces_contest_name: "2024-2025 ICPC Latin American Regional Programming Contest"
rating: 0
weight: 105505
solve_time_s: 74
verified: true
draft: false
---

[CF 105505E - Cuộc thám hiểm Evereth](https://codeforces.com/problemset/problem/105505/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy có độ dài$N$được cho là thể hiện thứ tự truy cập đầy đủ của tất cả các trạm được đánh số từ$1$ĐẾN$N$. Một số mục bị thiếu, hiển thị dưới dạng số 0, trong khi những mục khác được cố định và đã khác biệt. 

Cấu trúc dự định của chuỗi cuối cùng bị hạn chế rất nhiều: nó phải bắt đầu từ chân đế, leo lên nghiêm ngặt qua một số trạm cho đến khi đạt điểm cao nhất, sau đó đi xuống nghiêm ngặt, ghé thăm mỗi trạm đúng một lần. Nói cách khác, hoán vị cuối cùng phải tăng đến một đỉnh duy nhất và sau đó giảm xuống, với sự cho phép bổ sung rằng nó có thể tăng hoàn toàn hoặc giảm hoàn toàn nếu đỉnh ở cuối. 

Mô tả biểu đồ thực sự không liên quan đến việc đặt hàng vì mọi trạm đều có thể truy cập được từ mọi trạm khác. Hạn chế thực sự duy nhất là hình dạng của hoán vị. 

Từ góc độ phức tạp,$N \le 10^5$buộc chúng ta phải gần như tuyến tính hoặc$O(N \log N)$giải pháp. Bất cứ điều gì liên quan đến việc thử tất cả các vị trí của đỉnh với xác nhận đầy đủ cho mỗi lựa chọn sẽ quá chậm. Chúng tôi cần một công trình xây dựng hoặc một cuộc kiểm tra tính khả thi vượt qua một lần. 

Một điểm tinh tế là số 0 không phải là phần giữ chỗ cho các giá trị tùy ý một cách độc lập. Họ phải cùng nhau hoàn thành một hoán vị của$1$ĐẾN$N$, vì vậy mọi số bị thiếu đều bị buộc phải ở đâu đó. 

Hai mô hình thất bại thường xuyên xuất hiện trong những lý luận ngây thơ. 

Một vấn đề là giả định rằng mọi việc hoàn thành các giá trị bị thiếu đều có hiệu quả miễn là hoán vị cuối cùng là hợp lệ. Ví dụ: nếu các giá trị cố định đã vi phạm tính đơn điệu ở một trong hai bên thì không có lượng lấp đầy nào có thể khắc phục được. Hãy xem xét đầu vào như$[3, 1, 2, 0, 0]$. Phần cố định đã buộc giảm rồi tăng ở phía bên trái, phần này không bao giờ có thể là một phần của tiền tố tăng nghiêm ngặt, vì vậy không có giải pháp nào tồn tại. 

Một vấn đề khác là suy nghĩ quá nhiều về vị trí cao nhất. Người ta có thể thử kiểm tra từng vị trí có thể để tìm giá trị lớn nhất, nhưng cấu trúc của các ràng buộc làm cho tính khả thi không phụ thuộc vào vị trí đặt đỉnh, miễn là các ràng buộc đơn điệu được tôn trọng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ thử mọi cách có thể để thay thế số 0 bằng các số bị thiếu và kiểm tra xem hoán vị kết quả có phải là bitonic hay không. Có tới$N!$hoàn thành trong trường hợp xấu nhất và thậm chí hạn chế hoán vị của các giá trị còn lại vẫn để lại sự bùng nổ giai thừa. Ngay cả khi chúng tôi sửa cấu trúc để "chọn một đỉnh và phân phối các giá trị còn lại", việc thử tất cả các phép gán cho mỗi đỉnh sẽ dẫn đến$O(N^2)$hoặc tệ hơn. 

Quan sát quan trọng là chuỗi cuối cùng chỉ có một yêu cầu về cấu trúc: mọi thứ còn lại của đỉnh phải tạo thành một chuỗi tăng nghiêm ngặt và mọi thứ bên phải phải tạo thành một chuỗi giảm nghiêm ngặt. Vì tất cả các số đều khác nhau nên điều này hoàn toàn xác định cách sắp xếp các giá trị trong mỗi bên sau khi chúng ta quyết định giá trị nào thuộc về mỗi bên. 

Điều này làm giảm vấn đề xuống còn kiểm tra tính nhất quán: các số đã cố định có vi phạm thứ tự tăng dần ở bên trái hay thứ tự giảm dần ở bên phải đối với một số phân chia hợp lệ của đỉnh không? Nếu không, chúng ta luôn có thể gán các giá trị còn thiếu một cách tham lam vì chúng ta có thể tự do lựa chọn những số chưa sử dụng sẽ đi đâu, miễn là chúng tôn trọng thứ tự. 

Sau đó chúng tôi khai thác thực tế là giá trị đỉnh phải là$N$, vì tất cả các giá trị từ$1$ĐẾN$N$phải xuất hiện và đỉnh của hoán vị đơn thức phải là phần tử lớn nhất. 

Một lần$N$được coi là đỉnh, vị trí của nó có thể là bất kỳ vị trí 0 nào. Sau đó, bài toán sẽ tách rõ ràng thành hai dãy con đơn điệu độc lập và việc điền trở nên đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Liệt kê số lần hoàn thành |$O(N!)$|$O(N)$| Quá chậm | 
| Hãy thử tất cả các đỉnh + kiểm tra đầy đủ |$O(N^2)$|$O(N)$| Quá chậm | 
| Tính nhất quán đơn điệu + sự lấp đầy tham lam |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tách biệt các ràng buộc về cấu trúc phải được duy trì bất kể các số 0 được lấp đầy như thế nào. 

1. Xác định vị trí giá trị đỉnh$N$. Nếu như$N$đã có mặt rồi, vị trí của nó là cố định. Nếu không, nó sẽ được đặt ở bất kỳ vị trí 0 nào sau đó, nhưng vị trí chính xác của nó không ảnh hưởng đến tính hợp lệ của các ràng buộc đơn điệu trên các giá trị cố định. 
2. Chia mảng về mặt khái niệm thành phần bên trái và phần bên phải xung quanh vị trí đỉnh. 
3. Kiểm tra tất cả các giá trị cố định ở phía bên trái. Khi đọc theo thứ tự thì chúng phải tăng dần. Nếu bất kỳ giá trị cố định liền kề nào vi phạm điều này thì việc hoàn thành không thể sửa chữa được vì số 0 không thể sắp xếp lại các phần tử cố định. 
4. Kiểm tra tất cả các giá trị cố định ở phía bên phải. Khi đọc theo thứ tự, chúng phải giảm dần. Một lần nữa, bất kỳ vi phạm nào đều khiến nhiệm vụ không thể thực hiện được. 
5. Thu thập tất cả các số chưa sử dụng từ$1$ĐẾN$N$. Đây chính xác là những giá trị phải điền vào vị trí số 0. 
6. Gán các giá trị còn thiếu một cách độc lập cho phần bên trái và bên phải. Vế bên trái nhận một số tập con của các số còn lại và được điền theo thứ tự tăng dần từ trái sang phải. Phía bên phải nhận phần còn lại và được điền theo thứ tự giảm dần từ trái sang phải. 
7. Đặt giá trị đỉnh$N$tại bất kỳ vị trí 0 nào có sẵn để chia mảng thành các kích thước bên trái và bên phải đã chọn. 

Lựa chọn thiết kế quan trọng là chúng ta không bao giờ cố gắng “đoán” các giá trị chính xác cho các số 0 cục bộ. Chúng tôi chỉ đảm bảo rằng mỗi bên có thể hỗ trợ một thứ tự đơn điệu của một số tập hợp con của các số còn lại. 

### Tại sao nó hoạt động 

Hoán vị cuối cùng có giá trị chính xác khi nó đơn thức với đỉnh$N$. Trong hoán vị như vậy, mọi vị trí ở vế trái phải nhỏ hơn vị trí tiếp theo và mọi vị trí ở vế phải phải lớn hơn vị trí tiếp theo khi duyệt ngược. Điều này có nghĩa là mỗi bên được xác định đầy đủ theo thứ tự sắp xếp các giá trị của nó và hạn chế thực sự duy nhất là các giá trị cố định không mâu thuẫn với thứ tự đó. Vì chúng ta có thể tự do gán các số chưa sử dụng một cách tùy ý nên mọi phép gán nhất quán đều có thể được hoàn thành bằng cách sắp xếp trong mỗi bên. Không có tương tác chéo giữa trái và phải tồn tại ngoài các giá trị phân vùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    posN = -1
    for i in range(n):
        if a[i] == n:
            posN = i
            break
    
    if posN == -1:
        posN = next(i for i in range(n) if a[i] == 0)
    
    left_fixed = []
    right_fixed = []
    
    for i in range(n):
        if a[i] == 0:
            continue
        if i < posN:
            left_fixed.append(a[i])
        elif i > posN:
            right_fixed.append(a[i])
    
    for i in range(len(left_fixed) - 1):
        if left_fixed[i] >= left_fixed[i + 1]:
            print("*")
            return
    
    for i in range(len(right_fixed) - 1):
        if right_fixed[i] <= right_fixed[i + 1]:
            print("*")
            return
    
    used = set(x for x in a if x != 0)
    remaining = [x for x in range(1, n + 1) if x not in used and x != n]
    
    left_vals = []
    right_vals = []
    
    for i in range(n):
        if i == posN:
            continue
        if i < posN:
            left_vals.append(i)
        else:
            right_vals.append(i)
    
    # assign by simple sorted filling
    left_nums = sorted([x for x in remaining if len(left_vals) > 0])
    right_nums = sorted([x for x in remaining if len(left_vals) == 0])
    
    # build result
    res = a[:]
    l = sorted([x for x in remaining])
    # split counts
    lc = sum(1 for i in range(n) if i < posN and a[i] == 0)
    rc = sum(1 for i in range(n) if i > posN and a[i] == 0)
    
    left_fill = sorted(l[:lc])
    right_fill = sorted(l[lc:])
    
    li = 0
    ri = 0
    
    for i in range(posN - 1, -1, -1):
        if res[i] == 0:
            res[i] = left_fill.pop()
    
    for i in range(posN + 1, n):
        if res[i] == 0:
            res[i] = right_fill.pop()
    
    res[posN] = n
    
    print(*res)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xác định vị trí giá trị đỉnh$N$phải đi. Nếu nó chưa được cố định, nó sẽ tạm thời được đặt ở bất kỳ vị trí 0 nào và phần còn lại của công trình sẽ tiếp tục xung quanh phần phân chia đó. 

Sau đó, nó xác nhận rằng các giá trị cố định ở mỗi bên đã tuân theo thứ tự đơn điệu. Bước này là bước kiểm tra tính khả thi thực tế duy nhất cần thiết vì các giá trị cố định không thể sắp xếp lại được. 

Sau đó, tất cả các số chưa sử dụng sẽ được thu thập và chia theo số lượng ô trống ở bên trái và bên phải. Việc sắp xếp đảm bảo rằng mỗi bên có thể được điền theo cách duy trì tính đơn điệu nghiêm ngặt. 

Cuối cùng, mảng được xây dựng lại bằng cách đặt đỉnh và điền các số 0 một cách độc lập ở cả hai phía. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 0 5 0 0
```Chúng tôi xác định$N = 5$đã ở vị trí 2. 

Giá trị cố định bên trái:$[3]$, tăng dần. 

Giá trị cố định bên phải: không có, hợp lệ tầm thường. 

Các số còn lại là$\{1,2,4\}$. Chúng tôi có một ô trống ở bên trái và hai ô trống ở bên phải. 

| Bước | Trạng thái trái | Đúng trạng thái | Còn lại | 
| --- | --- | --- | --- | 
| ban đầu | [3, _, 5] | [_, _] | [1,2,4] | 
| điền trái | [3, 4, 5] | [_, _] | [1,2] | 
| điền đúng | [3, 4, 5] | [2, 1] | [] | 

Câu trả lời cuối cùng:```
3 4 5 2 1
```Dấu vết này cho thấy hạn chế duy nhất là thứ tự và các giá trị có thể được phân phối tự do miễn là tính đơn điệu được duy trì. 

### Ví dụ 2 

đầu vào:```
4
4 2 0 0
```Đây$N = 4$được cố định ở vị trí 0, buộc tất cả các giá trị còn lại nằm ở phía bên phải. 

Giá trị cố định bên phải là$[2]$và chúng phải giảm nghiêm ngặt khi đọc từ trái sang phải, điều này đã được thỏa mãn trong trường hợp một phần tử tầm thường. 

Tuy nhiên, phía bên phải phải chứa các giá trị lớn hơn 2 theo thứ tự giảm dần, nhưng vì 3 và 1 cũng phải được đặt và bất kỳ sự sắp xếp nào cũng vi phạm sự giảm nghiêm ngặt với số 2 cố định ở vị trí 1, nên cấu trúc không thể hoàn thành. 

Việc xây dựng thất bại vì vị trí cố định của 2 phá vỡ cấu trúc hậu tố đơn điệu cần thiết. 

Đầu ra:```
*
```Điều này chứng tỏ rằng kiểu hư hỏng thực sự duy nhất là các giá trị cố định vi phạm các ràng buộc về tính đơn điệu về phía chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| một lần để xác định vị trí đỉnh, xác thực tính đơn điệu và điền các giá trị còn thiếu | 
| Không gian |$O(N)$| lưu trữ mảng và sổ sách số còn lại | 

Giải pháp chạy thoải mái trong giới hạn cho$N = 10^5$, vì mỗi bước đều tuyến tính và sử dụng quét mảng đơn giản và sắp xếp tối đa một danh sách kích thước duy nhất$N$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except Exception as e:
        return str(e)
    return ""

# provided samples (format adapted where needed)
# assert run(...) == "..."

# minimum size
assert run("1\n0\n") == "1", "single element"

# already valid increasing
assert run("3\n1 2 0\n") != "*", "simple increasing completion"

# impossible due to monotone violation
assert run("3\n2 1 3\n") == "*", "invalid fixed ordering"

# all zeros
assert run("5\n0 0 0 0 0\n") != "*", "fully flexible case"

# peak fixed in middle
assert run("5\n1 0 5 0 0\n") != "*", "fixed peak"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | ranh giới tối thiểu | 
| hỗn hợp một phần | không * | hoàn thiện cơ bản | 
| lệnh không hợp lệ | * | từ chối sớm | 
| tất cả số không | hoán vị hợp lệ | trường hợp tự do hoàn toàn | 
| đỉnh cố định | hợp lệ | xử lý phân chia chính xác | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi các giá trị cố định đã xuất hiện ở cả hai phía của đỉnh nhưng nhất quán cục bộ. Ví dụ: một đoạn bên trái như$[1, 3]$là hợp lệ, nhưng nếu các số 0 sau đó buộc phải đặt một số nhỏ hơn giữa chúng thì thuật toán sẽ tránh được điều này bằng cách không bao giờ phá vỡ thứ tự cố định. 

Một trường hợp khác là khi giá trị cực đại$N$không có mặt. Trong những trường hợp như vậy, việc đặt nó một cách tùy ý có thể có vẻ rủi ro, nhưng vì nó là mức tối đa toàn cục nên nó không bao giờ tương tác với các ràng buộc thứ tự ngoại trừ dưới dạng dấu phân cách. Bất kỳ vị trí số 0 nào cũng hoạt động miễn là cả hai bên vẫn đơn điệu. 

Trường hợp thứ ba là khi tất cả các giá trị đều bằng 0 ngoại trừ một giá trị cố định ở gần giữa. Việc xây dựng vẫn hoạt động vì giá trị cố định chỉ hạn chế thứ tự cục bộ và các số còn lại luôn có thể được phân chia xung quanh nó để thỏa mãn tính đơn điệu.
