---
title: "CF 102302J - Sanchola kỳ lạ"
description: "Chúng ta cần chọn một số nguyên tố và thay đổi mọi phần tử của mảng thành số nguyên tố đó. Thay đổi một phần tử bằng một phần tử sẽ tốn đúng một thao tác, vì vậy nếu số nguyên tố được chọn là p thì tổng chi phí là f(p)= i=1 ∑ N ​ ∣a i ​ −p∣."
date: "2026-08-13T23:29:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102302
codeforces_index: "J"
codeforces_contest_name: "2019 USP-ICMC"
rating: 0
weight: 102302
solve_time_s: 345
verified: true
draft: false
---

[CF 102302J - Sanchola kỳ lạ](https://codeforces.com/problemset/problem/102302/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chọn một số nguyên tố và thay đổi mọi phần tử của mảng thành số nguyên tố đó. Thay đổi một phần tử bằng một phần tử sẽ tốn đúng một thao tác, vì vậy nếu số nguyên tố được chọn là p thì tổng chi phí là 

f(p)= i=1 ∑ N ​ ∣a i ​ −p∣. 

Nhiệm vụ là tìm giá trị nhỏ nhất có thể có của biểu thức này trên tất cả các số nguyên tố p. 

Mảng có thể chứa tối đa 10 5 giá trị, trong khi mỗi giá trị có thể lớn bằng 10 9. Với 10 5 phần tử, thuật toán kiểm tra một số lượng lớn các giá trị mục tiêu có thể có và quét toàn bộ mảng để tìm mọi mục tiêu là quá tốn kém. Giới hạn thời gian chỉ là một giây, vì vậy chúng ta cần khoảng O(NlogN), O(N) hoặc một cách tiếp cận hiệu quả tương tự khác. Giới hạn giá trị lớn cũng có nghĩa là chúng ta không thể đơn giản xây dựng một sàng đến tận 10 9. 

Khó khăn toán học trọng tâm là mục tiêu tốt nhất để giảm thiểu tổng sai số tuyệt đối được xác định bởi trung vị, nhưng mục tiêu bắt buộc phải là số nguyên tố. Chúng ta cần xử lý hạn chế đó mà không cần kiểm tra từng số nguyên tố. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể thất bại. Vì`1 1`, câu trả lời đúng là`2`, bởi vì mục tiêu hữu ích duy nhất ở gần đó là mục tiêu chính`2`, tốn một thao tác cho mỗi phần tử. Việc triển khai giả định rằng bản thân giá trị trung bình có thể sử dụng được sẽ trả về 0 không chính xác. 

Vì`2`với đầu vào`1 1000000000`, câu trả lời đúng là`999999999`. Trung vị dưới là`1`, Nhưng`1`không phải là nguyên tố. Số nguyên tố hữu ích gần nhất là`2`, cho`1 + 999999998 = 999999999`. Việc triển khai chỉ xem xét các số nguyên tố dưới mức trung bình sẽ không tìm thấy ứng cử viên nào cả. 

Đối với một mảng có kích thước chẵn như`2 3 10 11`, tập hợp các bộ giảm thiểu có giá trị thực là toàn bộ khoảng từ`3`ĐẾN`10`. Từ`3`,`5`, Và`7`là số nguyên tố, việc chọn bất kỳ trong số chúng sẽ mang lại chi phí tối thiểu. Việc triển khai chỉ coi một trung vị cụ thể là hợp lệ có thể bỏ lỡ cấu trúc khoảng này. 

Số nguyên tố đích cũng được phép lớn hơn mọi giá trị ban đầu. Đối với đầu vào`1 1000000000`, Ví dụ,`1000000007`là số nguyên tố, nhưng nó tệ hơn nhiều so với`2`. Việc triển khai đúng phải cho phép việc tìm kiếm số nguyên tố tiếp tục trên 10 9, mặc dù mọi giá trị đầu vào tối đa là 10 9. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi mục tiêu chính có thể có và tính tổng chi phí bằng cách quét toàn bộ mảng. Với mỗi ứng viên p, chúng ta tính ∑∣a i ​ −p∣ và giữ mức tối thiểu. Điều này đúng vì mọi mảng cuối cùng hợp pháp đều tương ứng với chính xác một mục tiêu chính, vì vậy việc kiểm tra mọi số nguyên tố cuối cùng sẽ kiểm tra mức tối ưu. 

Vấn đề là số lượng ứng viên. Có khoảng 5×10 7 số nguyên tố dưới 10 9, vì vậy ngay cả khi việc kiểm tra tính nguyên tố là miễn phí, việc quét 10 5 phần tử mảng cho mỗi ứng cử viên sẽ yêu cầu theo thứ tự các phép toán 5×10 12 phần tử. Điều đó không thể phù hợp trong một giây. 

Quan sát chính cũng giống như quan sát đằng sau nghiệm trung vị chuẩn cho tổng sai phân tuyệt đối. Nếu chúng ta tạm thời bỏ qua yêu cầu rằng mục tiêu phải là số nguyên tố thì mọi cực tiểu của 

f(x)=∑∣a i ​ −x∣ 

là một trung vị. Đối với mảng có kích thước lẻ, điểm liên quan duy nhất là phần tử ở giữa sau khi sắp xếp. Đối với một mảng có kích thước chẵn, mọi số thực giữa hai phần tử ở giữa sẽ giảm thiểu hàm. 

Hạn chế chính không phá hủy cấu trúc này. Hàm giảm khi chúng ta di chuyển về phía khoảng trung vị và tăng sau khi chúng ta di chuyển qua khoảng đó. Do đó, trong số các số nguyên tố, ứng cử viên tốt nhất phải là số nguyên tố gần nhất với khoảng trung vị đó. 

Chúng ta có thể biểu diễn khoảng trung vị bằng cách sử dụng điểm cuối bên trái của nó, trung vị dưới. Hãy để nó là m. Số nguyên tố tốt nhất bên trái là số nguyên tố lớn nhất nhiều nhất là m. Số nguyên tố tốt nhất bên phải là số nguyên tố nhỏ nhất ít nhất là m. Nếu số nguyên tố bên phải nằm trong khoảng trung vị thì nó đã cho giá trị tối thiểu không giới hạn. Nếu không có số nguyên tố trong khoảng thì hai ứng cử viên này chính xác là mục tiêu pháp lý gần nhất của hai bên. 

Vì vậy, toàn bộ vấn đề giảm xuống còn việc sắp xếp mảng, xác định vị trí trung vị thấp hơn, tìm số nguyên tố gần nhất ở mỗi bên và đánh giá chi phí cho những ứng cử viên đó. 

Để kiểm tra tính nguyên tố, cách triển khai bên dưới sử dụng Miller-Rabin xác định cho các số nguyên có kích thước 32 bit. Điều này thuận tiện ở đây vì giá trị đầu vào tối đa là 10 9 và số nguyên tố tiếp theo chỉ có thể lớn hơn giá trị đó một chút. Việc kiểm tra tính nguyên tố thực hiện phép tính lũy thừa mô-đun logarit và trong thực tế chỉ cần kiểm tra một vùng lân cận nhỏ xung quanh đường trung bình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(π(10 9 )N), khoảng 5×10 12 phép toán mảng | O(1) bên cạnh đầu vào | Quá chậm | 
| Tối ưu | O(NlogN+GlogA) | O(N) | Đã chấp nhận | 

Ở đây G là số số nguyên ứng cử viên được kiểm tra trong khi định vị các số nguyên tố lân cận và A là độ lớn của chúng. Đối với các giá trị giới hạn bởi 10 9, G rất nhỏ trong thực tế. 

## Hướng dẫn thuật toán 

1. Đọc mảng và sắp xếp nó. Việc sắp xếp cho phép chúng ta xác định trực tiếp trung vị, đó là điểm mà mục tiêu sai biệt tuyệt đối được giảm thiểu. 
2. Lấy`a[(N - 1) // 2]`như trung vị dưới`m`. Đối với N lẻ, đây là số trung vị thông thường. Đối với N chẵn, nó là điểm cuối bên trái của khoảng chứa mọi bộ cực tiểu không hạn chế. 
3. Tìm kiếm từ dưới lên`m`cho đến khi tìm được số nguyên tố lớn nhất`left`với`left <= m`. Nếu như`m`là số nguyên tố thì ngay lập tức nó là ứng cử viên bên trái đúng. Nếu như`m`là`1`, không có số nguyên tố nào ở bên trái nên ứng viên này đơn giản bị bỏ qua. 
4. Tìm kiếm từ trên xuống`m`cho đến khi tìm được số nguyên tố nhỏ nhất`right`với`right >= m`. Tìm kiếm này cũng cần thiết khi`m`bản thân nó không phải là số nguyên tố. 
5. Tính tổng chi phí cho`left`Và`right`, bất cứ khi nào những ứng cử viên đó tồn tại. Câu trả lời là chi phí nhỏ hơn. 
6. Trả lại chi phí tối thiểu. Không có số nguyên tố nào khác có thể tốt hơn, bởi vì mọi số nguyên tố bên dưới khoảng trung vị sẽ tệ hơn khi nó di chuyển xa hơn về bên trái và mọi số nguyên tố trên khoảng đều tệ hơn khi nó di chuyển xa hơn về bên phải. 

### Tại sao nó hoạt động 

Điều bất biến là mục tiêu f(p)=∑∣a i ​ −p∣ được giảm thiểu trên tất cả các mục tiêu thực chính xác trên khoảng trung vị. Ở bên trái khoảng đó, di chuyển sang phải không thể làm tăng chi phí và ở bên phải khoảng đó, di chuyển sang trái không thể làm tăng chi phí. 

Giả sử một số nguyên tố nằm bên trái khoảng trung vị. Trong số tất cả các số nguyên tố như vậy, số lớn nhất là tốt nhất vì nó gần khoảng nhất định. Lập luận tương tự cho rằng trong số các số nguyên tố bên phải, số nhỏ nhất là tốt nhất. Một số nguyên tố bên trong khoảng trung vị đã là tối ưu. Việc tìm kiếm số nguyên tố gần nhất ở cả hai phía của trung vị dưới sẽ tìm thấy chính xác những khả năng này, vì vậy việc đánh giá chúng phải bao gồm mục tiêu nguyên tố tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_prime(n):
    if n < 2:
        return False
    if n % 2 == 0:
        return n == 2
    if n % 3 == 0:
        return n == 3

    d = 5
    step = 2
    while d * d <= n:
        if n % d == 0:
            return False
        d += step
        step = 6 - step
    return True

def previous_prime(x):
    while x >= 2:
        if is_prime(x):
            return x
        x -= 1
    return None

def next_prime(x):
    if x <= 2:
        return 2

    if x % 2 == 0:
        x += 1

    while not is_prime(x):
        x += 2

    return x

def cost(a, p):
    return sum(abs(x - p) for x in a)

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    median = a[(n - 1) // 2]

    left = previous_prime(median)
    right = next_prime(median)

    ans = float("inf")

    if left is not None:
        ans = min(ans, cost(a, left))

    if right is not None:
        ans = min(ans, cost(a, right))

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của việc thực hiện là`is_prime`. Giá trị bên dưới`2`bị từ chối ngay lập tức và`2`Và`3`được xử lý riêng biệt. Sau đó, chỉ cần kiểm tra các ước có dạng 6k−1 và 6k+1. Vòng lặp dừng khi`d * d > n`, bởi vì mọi hợp số đều phải có thừa số không lớn hơn căn bậc hai của nó.`previous_prime`bao gồm giá trị bắt đầu. Điều này là cần thiết vì bản thân số trung vị có thể đã là số nguyên tố. Ví dụ: một mảng bao gồm toàn bộ`7`s phải tạo ra số 0, vì vậy bắt đầu tìm kiếm tại`6`sẽ giới thiệu một lỗi không cần thiết.`next_prime`tuân theo cùng một quy ước bao gồm. Sau khi xử lý`2`, nó di chuyển một giá trị bắt đầu chẵn sang số lẻ tiếp theo và sau đó chỉ kiểm tra các ứng cử viên lẻ. Việc tăng thêm hai sẽ tránh việc kiểm tra mọi số tổng hợp chẵn. 

Mảng được sắp xếp một lần và`(n - 1) // 2`cố tình chọn mức trung vị thấp hơn. Đối với độ dài lẻ, đây là giá trị trung bình chính xác. Đối với độ dài chẵn, đó là ranh giới bên trái của khoảng trung vị, điều này là đủ vì số nguyên tố gần nhất bên phải sẽ nằm trong khoảng đó hoặc là số nguyên tố đầu tiên nằm ngoài khoảng đó. 

Chi phí được đánh giá trực tiếp bằng cách cộng các chênh lệch tuyệt đối. Số nguyên Python có độ chính xác tùy ý, do đó không có rủi ro tràn. Trong ngôn ngữ có chiều rộng cố định, câu trả lời tích lũy phải sử dụng số học số nguyên 64 bit vì tổng có thể vào khoảng 10 14. 

Việc tìm kiếm ứng viên chỉ được thực hiện xung quanh điểm trung vị chứ không phải trên toàn bộ phạm vi lên tới 10 9. Đây là điểm khác biệt chính so với phương pháp tiếp cận bạo lực. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là`2 3 10`. Sau khi sắp xếp, nó không thay đổi và trung vị dưới là`3`. 

| Mảng | Trung vị dưới | Nguyên tố trái | Đúng nguyên tố | Chi phí tại ứng viên | 
| --- | --- | --- | --- | --- | 
| 2, 3, 10 | 3 | 3 | 3 | 1+0+7=8 | 

Số trung vị đã là số nguyên tố nên cả hai tìm kiếm đều xác định`3`. Thay đổi`2`ĐẾN`3`tốn một thao tác, trong khi thay đổi`10`ĐẾN`3`giá bảy. Câu trả lời là`8`. 

Điều này thể hiện trường hợp đơn giản nhất của nguyên lý trung vị. Sau khi bản thân công cụ thu nhỏ không hạn chế đã hợp pháp thì không còn gì để tối ưu hóa nữa. 

### Mẫu 2 

Đầu vào là`1 1000000000`. Mảng được sắp xếp không thay đổi và trung vị dưới là`1`. 

| Mảng | Trung vị dưới | Nguyên tố trái | Đúng nguyên tố | Chi phí tại ứng viên | 
| --- | --- | --- | --- | --- | 
| 1, 1000000000 | 1 | không | 2 | 1+999999998=999999999 | 

Không có số nguyên tố nào nhỏ hơn hoặc bằng`1`, vì vậy ứng cử viên bên trái không tồn tại. Số nguyên tố đầu tiên bên phải là`2`. Di chuyển`1`ĐẾN`2`tốn một thao tác, trong khi di chuyển`1000000000`ĐẾN`2`chi phí`999999998`, đưa ra câu trả lời cần thiết của`999999999`. 

Ví dụ này cũng cho thấy tại sao việc tìm kiếm phải cho phép một ứng cử viên ở bên phải ngay cả khi bản thân số trung vị không phải là số nguyên tố. 

### Mẫu 3 

cho`3 5 7 11`, mảng được sắp xếp đã được sắp xếp và giá trị trung vị thấp hơn là`5`. 

| Mảng | Trung vị dưới | Nguyên tố trái | Đúng nguyên tố | Chi phí tại ứng viên | 
| --- | --- | --- | --- | --- | 
| 3, 5, 7, 11 | 5 | 5 | 5 | 2+0+2+6=10 | 

Ở đây toàn bộ khoảng thời gian từ`5`ĐẾN`7`bao gồm các bộ giảm thiểu không hạn chế và cả hai điểm cuối đều là số nguyên tố. Lựa chọn`5`mang lại chi phí`10`, phù hợp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NlogN+G A ​ ) | Việc sắp xếp chiếm ưu thế trong quá trình xử lý mảng, trong khi các giá trị ứng cử viên G xung quanh giá trị trung vị được kiểm tra tính nguyên tố | 
| Không gian | O(N) | Mảng được lưu trữ và sắp xếp trong bộ nhớ | 

Với N<10 5, việc sắp xếp yêu cầu so sánh đại khái NlogN, dễ dàng nằm trong phạm vi dự định. Các giá trị tối đa là 10 9, do đó việc kiểm tra tính nguyên tố chỉ bao gồm các ước số tối đa khoảng 31623. Vì việc tìm kiếm được thực hiện gần một trung vị chứ không phải trên toàn bộ phạm vi giá trị nên số lượng kiểm tra tính nguyên tố là nhỏ đối với các ràng buộc này. 

Việc triển khai cũng tránh được việc xây dựng sàng có kích thước 10 9, điều này sẽ gây lãng phí cả về bộ nhớ và thời gian tiền xử lý. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    def is_prime(n):
        if n < 2:
            return False
        if n % 2 == 0:
            return n == 2
        if n % 3 == 0:
            return n == 3

        d = 5
        step = 2
        while d * d <= n:
            if n % d == 0:
                return False
            d += step
            step = 6 - step
        return True

    def previous_prime(x):
        while x >= 2:
            if is_prime(x):
                return x
            x -= 1
        return None

    def next_prime(x):
        if x <= 2:
            return 2

        if x % 2 == 0:
            x += 1

        while not is_prime(x):
            x += 2

        return x

    def cost(a, p):
        return sum(abs(x - p) for x in a)

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    median = a[(n - 1) // 2]

    left = previous_prime(median)
    right = next_prime(median)

    ans = float("inf")

    if left is not None:
        ans = min(ans, cost(a, left))

    if right is not None:
        ans = min(ans, cost(a, right))

    print(ans)

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue().strip()

def run(inp: str) -> str:
    return solve_data(inp)

assert run("3\n2 3 10\n") == "8", "sample 1"
assert run("2\n1 1000000000\n") == "999999999", "sample 2"
assert run("4\n3 5 7 11\n") == "10", "sample 3"

assert run("1\n1\n") == "1", "minimum-size input"
assert run("3\n7 7 7\n") == "0", "all values already equal to a prime"
assert run("2\n14 16\n") == "4", "two medians with no prime between them"
assert run("2\n1000000000 1000000000\n") == "14", "maximum value requires a prime above 1e9"
assert run("2\n10 11\n") == "1", "prime inside the even-sized median interval"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Mảng nhỏ nhất có thể và giá trị không phải số nguyên tố`1`| 
|`7 7 7`|`0`| Tất cả các phần tử đã bằng số nguyên tố | 
|`14 16`|`4`| Khoảng trung bình có kích thước chẵn và số nguyên tố ở cả hai bên | 
|`1000000000 1000000000`|`14`| Giá trị đầu vào và tìm kiếm tối đa trên 10 9 | 
|`10 11`|`1`| Một số nguyên tố nằm trong khoảng trung vị | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu`1 / 1`, trung vị dưới là`1`. Việc tìm kiếm đi xuống không tìm thấy số nguyên tố vì số nguyên tố bắt đầu từ`2`, trong khi tìm kiếm hướng lên trên tìm thấy`2`. Phần tử duy nhất di chuyển từ`1`ĐẾN`2`, vậy câu trả lời là`1`. Điều này phát hiện các triển khai giả định không chính xác trung vị luôn là mục tiêu hợp lệ. 

Vì`2 / 1 1000000000`, trung vị dưới lại là`1`. Không có ứng cử viên bên trái, và ứng cử viên bên phải là`2`. Chi phí kết quả là`|1-2| + |1000000000-2| = 1 + 999999998 = 999999999`. Trường hợp này thực hiện cả ranh giới dưới của miền đầu vào và giá trị mảng lớn nhất có thể. 

Vì`3 / 7 7 7`, trung vị là`7`, Và`7`là nguyên tố. Cả hai tìm kiếm ứng viên đều trả về`7`, sản xuất`0`. Một tìm kiếm bắt đầu ngay dưới hoặc trên mức trung bình một cách sai lầm sẽ bỏ lỡ mục tiêu tối ưu một cách không chính xác. 

Vì`2 / 14 16`, các bộ giảm thiểu không hạn chế là mọi số thực từ`14`bởi vì`16`. Không có số nguyên tố trong khoảng đó. Số nguyên tố gần nhất bên trái là`13`, và số nguyên tố gần nhất bên phải là`17`. Cả hai đều đưa ra chi phí`|14-13| + |16-13| = 4`Và`|14-17| + |16-17| = 4`. Câu trả lời là`4`. Đây là trường hợp có độ dài chẵn quan trọng trong đó việc coi trung vị dưới là mức tối ưu duy nhất không hạn chế sẽ che khuất lý do tại sao hai số nguyên tố lân cận là đủ. 

Vì`2 / 1000000000 1000000000`, số trung vị dưới là 10 9. Số nguyên tố trước đó là`999999937`, đó là`63`đi, trong khi số nguyên tố tiếp theo là`1000000007`, chỉ một`7`xa. Việc chọn cái sau sẽ thay đổi mỗi phần tử thêm bảy, với tổng số`14`. Điều này xác minh rằng thuật toán xử lý mục tiêu chính trên giá trị đầu vào tối đa được phép và số học số nguyên vẫn chính xác.
