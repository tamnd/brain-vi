---
title: "CF 1900C - Cây nhị phân của Anji"
description: "Chúng ta được cấp một cây nhị phân có gốc. Mỗi nút chứa một ký tự: - 'L' có nghĩa là \"đi đến con bên trái\" - 'R' có nghĩa là \"đi đến con bên phải\" - 'U' có nghĩa là \"đi đến con cha\" Nếu điểm đến được yêu cầu không tồn tại, khách du lịch vẫn ở nguyên vị trí của mình."
date: "2026-06-08T21:20:58+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1900
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 911 (Div. 2)"
rating: 1300
weight: 1900
solve_time_s: 247
verified: true
draft: false
---

[CF 1900C - Cây nhị phân của Anji](https://codeforces.com/problemset/problem/1900/C) 

**Đánh giá:** 1300 
**Thẻ:** dfs và tương tự, dp, cây cối 
**Thời gian giải:** 4 phút 7 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây nhị phân có gốc. Mỗi nút chứa một ký tự: 

-`'L'`có nghĩa là "đi tới con bên trái" 
-`'R'`có nghĩa là "đi đến đúng con" 
-`'U'`có nghĩa là "đi đến cha mẹ" 

Nếu điểm đến được yêu cầu không tồn tại, du khách sẽ ở lại nơi mình đang ở. 

Lữ khách bắt đầu từ gốc rễ. Trước khi bắt đầu, chúng ta có thể thay đổi ký tự được ghi trên bất kỳ nút nào. Mỗi thay đổi tốn một thao tác. 

Mục tiêu của chúng tôi là làm cho khách du lịch chạm tới ít nhất một chiếc lá tại một thời điểm nào đó trong suốt chuyến đi trong khi chi tiêu ít sửa đổi nhất có thể. 

Điều đầu tiên cần chú ý là chúng ta không quan tâm đến toàn bộ quá trình chuyển động vô hạn. Chúng tôi chỉ quan tâm liệu cuối cùng có đạt được một số lá hay không. Vì cây là hữu hạn nên khi chúng ta chọn đường đi từ gốc đến lá, các nút duy nhất quan trọng là các nút trên đường dẫn đó. 

Tổng số nút trên tất cả các trường hợp thử nghiệm nhiều nhất là$3 \cdot 10^5$. Bất kỳ thuật toán nào thử từng lá riêng biệt và liên tục đi qua các phần lớn của cây đều có thể trở thành bậc hai. Với$3 \cdot 10^5$nút,$O(n^2)$là hoàn toàn không thể thực hiện được, trong khi$O(n)$hoặc$O(n \log n)$đủ nhanh một cách dễ dàng. 

Một điểm tinh tế là một nút có nhãn`'U'`không phải lúc nào cũng xấu. Giả sử một nút không phải là một phần của tuyến đường từ gốc đến lá đã chọn. Chúng tôi không quan tâm những gì xảy ra ở đó. Chỉ các nút trên tuyến đường mà chúng tôi muốn khách du lịch đi theo mới đóng góp vào câu trả lời. 

Một sai lầm dễ mắc phải khác là nghĩ rằng mọi nút trên tuyến đều phải hướng xuống dưới. Coi như:```
1('L')
/
2
```Nút 2 đã là một chiếc lá. Khi khách du lịch đến nút 2, mục tiêu sẽ đạt được. Chúng ta không bao giờ cần quan tâm đến việc di chuyển được thực hiện từ chính nút 2. 

Một ví dụ cụ thể:```
n = 2
s = "LU"

1 -> right child = 2
```Gốc muốn đi sang trái, nhưng chỉ có con bên phải tồn tại. Lữ khách bị mắc kẹt mãi mãi ở gốc rễ. Thay đổi gốc từ`'L'`ĐẾN`'R'`có giá 1 và ngay lập tức đến lá. Câu trả lời đúng là 1. 

Một giải pháp dựa trên mô phỏng bất cẩn cũng có thể bị mắc kẹt bởi các chu trình được tạo bởi`'U'`nhãn. Ví dụ:```
1('L')
|
2('U')
```Bắt đầu từ số 1, người du hành cứ luân phiên giữa số 1 và số 2 mãi mãi. Tuy nhiên, chỉ thay đổi nút 2 để trỏ tới một chiếc lá ở đâu đó bên dưới có thể giải quyết được vấn đề. Chúng ta cần tối ưu hóa toàn cục trên cây chứ không phải mô phỏng các nhãn hiện tại. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xem xét từng chiếc lá một cách riêng biệt. Đối với một lá được chọn, hãy nhìn vào đường dẫn từ gốc tới lá duy nhất. Mỗi nút nội bộ trên đường dẫn đó phải hướng người du hành tới nút tiếp theo trong đường dẫn. Nếu nhãn hiện tại đã khớp với hướng được yêu cầu đó thì chi phí cho nút đó là 0; nếu không thì bằng 1. Tổng các chi phí này sẽ cho ra số lượng sửa đổi cần thiết để đạt được lá cụ thể đó. Lấy mức tối thiểu trên tất cả các lá sẽ mang lại câu trả lời. 

Lý luận này thực sự đúng. Vấn đề là hiệu quả. Nếu chúng ta xây dựng lại rõ ràng đường đi từ gốc tới lá cho mỗi lá, một cây có độ lệch cao có thể chứa$O(n)$lá có đường đi dài$O(n)$, dẫn đến$O(n^2)$công việc. 

Quan sát quan trọng là sự đóng góp chi phí của một nút chỉ phụ thuộc vào nút con nào chúng ta chọn tiếp theo. 

Giả sử chúng ta hiện đang ở nút$v$. 

Nếu chúng ta quyết định rằng lá tối ưu nằm ở cây con bên trái thì nút$v$phải gửi chúng tôi trái. Chi phí tại nút này là: 

- 0 nếu nhãn của nó là`'L'`- 1 cái khác 

Tương tự, nếu chúng ta tiếp tục đi vào cây con bên phải thì chi phí là: 

- 0 nếu nhãn của nó là`'R'`- 1 cái khác 

Điều này biến vấn đề thành một cây DP. 

Cho phép`dp[v]`biểu thị số lượng sửa đổi tối thiểu cần thiết để đạt được một số lá bắt đầu từ nút`v`. 

Đối với một chiếc lá, không cần thay đổi gì nữa, vì vậy`dp[v] = 0`. 

Đối với nút nội bộ, chúng tôi thử mọi nút con hiện có. Nếu chúng tôi rẽ trái, chúng tôi phải trả chi phí sửa đổi cục bộ cộng thêm`dp[left]`. Nếu chúng tôi đi đúng, chúng tôi sẽ trả chi phí sửa đổi cục bộ tương ứng cộng thêm`dp[right]`. 

Mức tối thiểu của những lựa chọn đó trở thành`dp[v]`. 

Vì mỗi nút được xử lý một lần nên toàn bộ cây được giải theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cấu trúc cây và chuỗi nhãn. 
2. Xác định`dp(v)`là số lượng sửa đổi tối thiểu cần thiết để đạt được một số lá bắt đầu từ nút`v`. 
3. Nếu`v`là một chiếc lá, trở về`0`. 

Chúng tôi đã ở một nút thỏa mãn mục tiêu sau khi đạt được. 
4. Khởi tạo câu trả lời cho nút này là vô cùng. 
5. Nếu tồn tại con trái, hãy tính:```
cost = (label[v] != 'L') + dp(left_child)
```Điều này thể hiện việc thay đổi nhãn hiện tại nếu cần thiết và sau đó tiếp cận một lá từ cây con bên trái một cách tối ưu. 
6. Cập nhật câu trả lời của nút với giá trị đó. 
7. Nếu tồn tại một con phải, hãy tính:```
cost = (label[v] != 'R') + dp(right_child)
```8. Cập nhật câu trả lời của nút với giá trị tối thiểu thu được. 
9. Trả về giá trị đã tính. 
10. Câu trả lời cho test case là`dp(root)`. 

### Tại sao nó hoạt động 

Mỗi đường đi từ gốc đến lá được xác định duy nhất bởi chuỗi các lựa chọn con dọc theo đường đi đó. Tại mỗi nút, quyết định duy nhất là lá được chọn nằm ở cây con bên trái hay cây con bên phải. 

Nếu đường dẫn đã chọn tiếp tục đi qua nút con bên trái thì nút hiện tại phải chứa`'L'`. Bất kỳ nhãn nào khác đều yêu cầu chính xác một sửa đổi. Sau khi trả chi phí địa phương đó, vấn đề còn lại giống như việc lấy được một chiếc lá từ con trái. Lập luận tương tự cũng áp dụng cho đứa trẻ phù hợp. 

Vì mọi lộ trình từ gốc đến lá hợp lệ sẽ phân rã thành một quyết định cục bộ cộng với một bài toán con tối ưu, nên phép truy toán sẽ kiểm tra tất cả các lá có thể và tính tổng số sửa đổi tối thiểu trong số đó. Bằng quy nạp về kích thước cây con,`dp(v)`là câu trả lời tối ưu thực sự cho mọi nút, bao gồm cả nút gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 20)

t = int(input())

for _ in range(t):
    n = int(input())
    s = input().strip()

    left = [0] * (n + 1)
    right = [0] * (n + 1)

    for i in range(1, n + 1):
        l, r = map(int, input().split())
        left[i] = l
        right[i] = r

    dp = [0] * (n + 1)

    def dfs(v):
        if left[v] == 0 and right[v] == 0:
            dp[v] = 0
            return 0

        res = 10 ** 9

        if left[v]:
            res = min(
                res,
                (0 if s[v - 1] == 'L' else 1) + dfs(left[v])
            )

        if right[v]:
            res = min(
                res,
                (0 if s[v - 1] == 'R' else 1) + dfs(right[v])
            )

        dp[v] = res
        return res

    print(dfs(1))
```DFS tính toán giá trị DP của mỗi cây con chính xác một lần. Một lá ngay lập tức trả về 0 vì việc tiếp cận lá đó đã thỏa mãn mục tiêu. 

Đối với một nút bên trong, phép truy toán phản ánh trực tiếp lý do từ bằng chứng. Nếu chúng ta chọn cây con bên trái, nút hiện tại phải hướng hành khách sang trái. biểu hiện```
(0 if s[v - 1] == 'L' else 1)
```chính xác là chi phí sửa đổi cần thiết tại nút đó. 

Các nhãn được lưu trữ trong một chuỗi không có chỉ mục trong khi các nút được đánh số từ 1, đó là lý do tại sao mã truy cập`s[v - 1]`. 

Một nút có thể chỉ có một nút con. Việc triển khai chỉ đánh giá các phần tử con hiện có, điều này rất quan trọng vì việc tiến tới một phần tử con không tồn tại không bao giờ hữu ích khi cố gắng tiếp cận một chiếc lá. 

Độ sâu đệ quy có thể đạt tới$n$trong cây suy biến, do đó giới hạn đệ quy được tăng lên trước khi xử lý các ca kiểm thử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
LRU
2 3
0 0
0 0
```Cây:```
    1(L)
   /   \
  2     3
```| Nút | Chi phí còn lại | Chi phí phù hợp | dp | 
| --- | --- | --- | --- | 
| 2 | - | - | 0 | 
| 3 | - | - | 0 | 
| 1 | 0 + dp(2) = 0 | 1 + dp(3) = 1 | 0 | 

Trả lời =`dp(1) = 0`. 

Gốc đã trỏ về lá 2 nên không cần sửa đổi. 

### Ví dụ 2 

đầu vào:```
2
LU
0 2
0 0
```Cây:```
1(L)
 \
  2
```| Nút | Chi phí còn lại | Chi phí phù hợp | dp | 
| --- | --- | --- | --- | 
| 2 | - | - | 0 | 
| 1 | không hợp lệ | 1 + dp(2) = 1 | 1 | 

Trả lời =`1`. 

Gốc chỉ có một con đúng. Vì nhãn của nó là`'L'`, một sửa đổi là cần thiết. 

Ví dụ này chứng minh tại sao các phần tử con không tồn tại phải bị bỏ qua thay vì được coi là các chuyển đổi hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được xử lý một lần | 
| Không gian | O(n) | Bộ lưu trữ cây cộng với ngăn xếp đệ quy và mảng DP | 

Tổng số lượng nút nhiều nhất là$3 \cdot 10^5$. Việc truyền tải tuyến tính trên mỗi trường hợp thử nghiệm chỉ thực hiện một lượng công việc không đổi trên mỗi nút, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.setrecursionlimit(1 << 20)

    data = io.StringIO(inp)
    input = data.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        left = [0] * (n + 1)
        right = [0] * (n + 1)

        for i in range(1, n + 1):
            l, r = map(int, input().split())
            left[i] = l
            right[i] = r

        def dfs(v):
            if left[v] == 0 and right[v] == 0:
                return 0

            res = 10 ** 9

            if left[v]:
                res = min(
                    res,
                    (0 if s[v - 1] == 'L' else 1) + dfs(left[v])
                )

            if right[v]:
                res = min(
                    res,
                    (0 if s[v - 1] == 'R' else 1) + dfs(right[v])
                )

            return res

        out.append(str(dfs(1)))

    return "\n".join(out)

# provided sample
assert run(
"""5
3
LRU
2 3
0 0
0 0
3
ULR
3 2
0 0
0 0
2
LU
0 2
0 0
4
RULR
3 0
0 0
0 4
2 0
7
LLRRRLU
5 2
3 6
0 0
7 0
4 0
0 0
0 0
"""
) == "0\n1\n1\n3\n1"

# minimum size, already correct
assert run(
"""1
2
RU
0 2
0 0
"""
) == "0"

# minimum size, one modification needed
assert run(
"""1
2
LU
0 2
0 0
"""
) == "1"

# all labels U
assert run(
"""1
3
UUU
2 3
0 0
0 0
"""
) == "1"

# skewed chain
assert run(
"""1
4
UUUU
2 0
3 0
4 0
0 0
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây hai nút đúng hướng | 0 | Trường hợp thành công nhỏ nhất | 
| Cây hai nút sai hướng | 1 | Sửa đổi yêu cầu duy nhất | 
| Tất cả nhãn`'U'`| 1 | Chọn con đường từ gốc đến lá rẻ nhất | 
| Dây chuyền dài | 3 | Đệ quy sâu và tích lũy đường dẫn | 

## Vỏ cạnh 

### Node chỉ có một con 

đầu vào:```
1
2
LU
0 2
0 0
```Gốc không có con trái và một con phải. 

Thuật toán chỉ đánh giá sự chuyển tiếp đúng:```
cost = 1 + dp(2)
```và bỏ qua hoàn toàn đứa trẻ còn lại bị mất tích. Kết quả là 1, đúng vì gốc phải được đổi thành`'R'`. 

### Nhiều lá với chi phí khác nhau 

đầu vào:```
1
3
ULU
2 3
0 0
0 0
```Rễ có thể chạm tới một trong hai lá. 

Chi phí đi bên trái:```
1 + 0 = 1
```Đi đúng chi phí:```
1 + 0 = 1
```DP lấy mức tối thiểu và trả về 1. Thuật toán không bao giờ cam kết với một lá cụ thể quá sớm, điều này rất cần thiết để đạt được sự tối ưu. 

### Chuỗi sâu 

đầu vào:```
1
4
UUUU
2 0
3 0
4 0
0 0
```Đường dẫn từ gốc tới lá duy nhất là:```
1 -> 2 -> 3 -> 4
```Mỗi nút bên trong phải trỏ sang trái, vì vậy mỗi nút 1, 2 và 3 đều yêu cầu một sửa đổi. 

DP tính toán:```
dp(4)=0
dp(3)=1
dp(2)=2
dp(1)=3
```và trả về 3. 

### Nhãn lá không liên quan 

đầu vào:```
1
2
LR
2 0
0 0
```Nút 2 đã là một chiếc lá. Nhãn của nó`'R'`không bao giờ quan trọng vì mục tiêu sẽ đạt được ngay khi đạt được nút 2. 

Thuật toán gán:```
dp(2)=0
```mà không kiểm tra nhãn, nó khớp chính xác với định nghĩa vấn đề.
