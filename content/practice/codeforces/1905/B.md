---
title: "CF 1905B - ​​Zelda của người mới bắt đầu"
description: "Chúng tôi được tặng một cái cây và một hoạt động đặc biệt. Trong một thao tác, chúng ta chọn hai đỉnh bất kỳ và xem xét đường đi duy nhất giữa chúng. Mỗi đỉnh trên đường đi đó được hợp nhất thành một đỉnh mới."
date: "2026-06-09T01:18:47+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1905
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 915 (Div. 2)"
rating: 1100
weight: 1905
solve_time_s: 110
verified: true
draft: false
---

[CF 1905B - Zelda của người mới bắt đầu](https://codeforces.com/problemset/problem/1905/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, cây cối 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây và một hoạt động đặc biệt. 

Trong một thao tác, chúng ta chọn hai đỉnh bất kỳ và xem xét đường đi duy nhất giữa chúng. Mỗi đỉnh trên đường đi đó được hợp nhất thành một đỉnh mới. Bất kỳ cạnh nào ban đầu kết nối một số đỉnh bên ngoài với đường đi đều được chuyển hướng đến đỉnh mới này. 

Mục tiêu là thực hiện lặp lại việc nén đường dẫn như vậy cho đến khi toàn bộ cây trở thành một đỉnh duy nhất. Chúng ta phải tìm số lượng hoạt động tối thiểu. 

Vì đồ thị là một cái cây nên có chính xác$n-1$các cạnh và một đường đi duy nhất giữa mỗi cặp đỉnh. Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$, vì vậy chúng ta cần một cái gì đó gần với thời gian tuyến tính. Bất kỳ giải pháp nào liên tục sửa đổi cây và tìm kiếm đường đi tối ưu sẽ nhanh chóng trở nên quá tốn kém. MỘT$O(n^2)$cách tiếp cận đã quá chậm trong trường hợp xấu nhất. 

Phần khó khăn là hiểu được việc nén đường dẫn thực sự tác động như thế nào đến cấu trúc của cây. 

Hãy xem xét một chuỗi đơn giản:```
1 - 2 - 3 - 4 - 5
```Chọn đỉnh 1 và 5 sẽ nén toàn bộ cây thành một đỉnh nên đáp án là 1. 

đầu vào:```
1
5
1 2
2 3
3 4
4 5
```Đầu ra:```
1
```Cách tiếp cận bất cẩn chỉ dựa trên số đỉnh sẽ bỏ sót rằng một thao tác có thể loại bỏ nhiều đỉnh cùng một lúc. 

Bây giờ hãy xem xét một ngôi sao:```
    2
    |
3 - 1 - 4
    |
    5
```Có bốn lá. Việc nén một đường dẫn giữa hai lá sẽ loại bỏ hai lá đó và tâm, nhưng các lá khác vẫn được gắn vào. Câu trả lời không phải là 1. 

đầu vào:```
1
5
1 2
1 3
1 4
1 5
```Đầu ra:```
2
```Ví dụ này cho thấy số lượng lá quan trọng hơn nhiều so với tổng số đỉnh. 

Một sai lầm dễ mắc phải khác là giả sử mọi thao tác đều giảm số lượng lá xuống đúng hai. Một đường đi có thể đi qua nhiều đỉnh bên trong, nhưng chỉ các điểm cuối của nó mới có thể là các lá. Hiểu chính xác làm thế nào những chiếc lá biến mất là quan sát quan trọng. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là nghĩ về cái cây thay đổi sau mỗi thao tác. Chúng ta có thể thử mọi cặp đỉnh có thể, mô phỏng quá trình nén, giải đệ quy cây còn lại và lấy mức tối thiểu. 

Về nguyên tắc, điều này đúng vì nó khám phá tất cả các chuỗi hoạt động có thể xảy ra. Thật không may, ngay cả một hoạt động đã có$O(n^2)$sự lựa chọn, và số lượng cây kết quả tăng lên một cách bùng nổ. Cách tiếp cận như vậy là hoàn toàn không khả thi ngay cả đối với vài chục đỉnh. 

Để tìm một mẫu hữu ích, hãy quên đi các đỉnh riêng lẻ và tập trung vào những chiếc lá. 

Mỗi cây có nhiều hơn một đỉnh thì có ít nhất hai lá. Giả sử chúng ta nén một đường dẫn có điểm cuối là các lá. Tất cả những chiếc lá nằm trên con đường đó đều biến mất. Vì một đường dẫn đơn giản chỉ có thể có các lá ở điểm cuối của nó nên thao tác này sẽ loại bỏ chính xác hai lá khỏi cây. 

Điều gì xảy ra với cấu trúc còn lại? Đường đi nén sẽ trở thành một đỉnh duy nhất. Nếu đường đi nối với phần còn lại của cây thì đỉnh mới đó có thể trở thành một chiếc lá. Một cách hiệu quả, một thao tác có thể kết hợp hai lá hiện có thành một "đại diện". 

Điều này bắt đầu trông giống như những chiếc lá được ghép nối nhiều lần. 

Cho phép$L$là số lượng lá. 

Một thao tác có thể giảm số lượng lá nhiều nhất là hai. Để loại bỏ tất cả các lá và kết thúc bằng một đỉnh duy nhất, chúng ta cần ít nhất$$\left\lceil \frac{L}{2} \right\rceil$$hoạt động. 

Điều đáng ngạc nhiên là giới hạn dưới này luôn có thể đạt được. 

Ở bất kỳ giai đoạn nào, hãy chọn hai lá và nén đường đi giữa chúng. Điều này loại bỏ hai lá đó khỏi sự xem xét. Việc lặp lại quá trình này sẽ tiếp tục ghép các lá cho đến khi không còn lá nào. Nếu như$L$chẵn, chúng ta cần$L/2$hoạt động. Nếu như$L$thật kỳ lạ, sau khi ghép nối$L-1$lá, cấu trúc tương đương một lá vẫn còn và yêu cầu một thao tác cuối cùng. Số đếm trở thành$(L+1)/2$. 

Như vậy câu trả lời chỉ phụ thuộc vào số lượng lá:$$\boxed{\left\lceil \frac{L}{2} \right\rceil}$$Tất cả những gì chúng ta cần là đếm các đỉnh bậc 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây. 
2. Compute the degree of every vertex by processing all edges.
 3. Count how many vertices have degree equal to 1. Let this value be$L$. 
4. Đầu ra$(L + 1) // 2$. 

biểu hiện$(L + 1) // 2$là dạng số nguyên của$\lceil L/2 \rceil$. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là mọi thao tác có thể loại bỏ tối đa hai lá, đó là điểm cuối của đường đã chọn. Vì có$L$rời đi ban đầu, bất kỳ giải pháp nào cũng cần ít nhất$\lceil L/2 \rceil$hoạt động. 

Ngược lại, việc chọn liên tục các đường đi giữa các cặp lá luôn loại bỏ hai lá cùng một lúc. Quá trình này đạt được chính xác$\lceil L/2 \rceil$hoạt động. Vì chúng ta có cả giới hạn dưới và cấu trúc phù hợp nên số lượng thao tác tối thiểu chính xác là$\lceil L/2 \rceil$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        n = int(input())

        deg = [0] * (n + 1)

        for _ in range(n - 1):
            u, v = map(int, input().split())
            deg[u] += 1
            deg[v] += 1

        leaves = 0
        for v in range(1, n + 1):
            if deg[v] == 1:
                leaves += 1

        answers.append(str((leaves + 1) // 2))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo kết quả toán học trực tiếp. 

Mảng độ lưu trữ số lượng cạnh chạm vào mỗi đỉnh. Trong một cây, một chiếc lá chính xác là một đỉnh có bậc bằng 1. Sau khi đọc tất cả các cạnh, chúng ta đếm các đỉnh như vậy. 

Câu trả lời cuối cùng được tính bằng cách sử dụng`(leaves + 1) // 2`, thực hiện phép chia trần cho 2 đối với các số nguyên không âm. 

Một lỗi phổ biến là cố gắng tự mô phỏng các hoạt động. Không có điều đó là cần thiết. Khi đã biết số lá, câu trả lời sẽ được xác định ngay lập tức. 

Một điểm tinh tế khác là kích thước cây tối thiểu. Khi`n = 2`, cả hai đỉnh đều là lá, vì vậy`leaves = 2`và công thức trả về đúng 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2
1 3
3 4
```Bằng cấp: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 1 | 

Số lá: 

| Lá | 
| --- | 
| 2 | 

Trả lời: 

| L | (L + 1) // 2 | 
| --- | --- | 
| 2 | 1 | 

Đầu ra:```
1
```Cây đã có một đường dẫn duy nhất từ ​​​​2 đến 4. Việc nén đường dẫn đó sẽ hợp nhất toàn bộ cây trong một thao tác. 

### Ví dụ 2 

đầu vào:```
9
3 1
3 5
3 2
5 6
6 7
7 8
7 9
6 4
```Bằng cấp: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 3 | 
| 4 | 1 | 
| 5 | 2 | 
| 6 | 3 | 
| 7 | 3 | 
| 8 | 1 | 
| 9 | 1 | 

Số lá: 

| Lá | 
| --- | 
| 5 | 

Trả lời: 

| L | (L + 1) // 2 | 
| --- | --- | 
| 5 | 3 | 

Đầu ra:```
3
```Ví dụ này minh họa trường hợp lá lẻ. Năm lá cần ba thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được xử lý một lần, sau đó tất cả các đỉnh được quét một lần | 
| Không gian | O(n) | Mảng độ lưu trữ một giá trị trên mỗi đỉnh | 

Tổng của tất cả$n$giá trị trên các trường hợp thử nghiệm là nhiều nhất$10^5$. Một giải pháp tuyến tính chỉ thực hiện tổng cộng vài trăm nghìn thao tác, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        deg = [0] * (n + 1)

        for _ in range(n - 1):
            u, v = map(int, input().split())
            deg[u] += 1
            deg[v] += 1

        leaves = sum(1 for x in deg[1:] if x == 1)
        ans.append(str((leaves + 1) // 2))

    return "\n".join(ans)

# provided sample
assert run(
"""4
4
1 2
1 3
3 4
9
3 1
3 5
3 2
5 6
6 7
7 8
7 9
6 4
7
1 2
1 3
2 4
4 5
3 6
2 7
6
1 2
1 3
1 4
4 5
2 6
"""
) == "1\n3\n2\n2"

# minimum tree
assert run(
"""1
2
1 2
"""
) == "1"

# path graph
assert run(
"""1
5
1 2
2 3
3 4
4 5
"""
) == "1"

# star with four leaves
assert run(
"""1
5
1 2
1 3
1 4
1 5
"""
) == "2"

# star with five leaves
assert run(
"""1
6
1 2
1 3
1 4
1 5
1 6
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây hai nút | 1 | Đầu vào hợp lệ nhỏ nhất | 
| Con đường đơn giản | 1 | Toàn bộ cây có thể được nén cùng một lúc | 
| Ngôi sao bốn lá | 2 | Số lá chẵn | 
| Ngôi sao năm lá | 3 | Số lá lẻ | 
| Mẫu chính thức | Kết quả đầu ra mẫu | Tính đúng đắn chung | 

## Vỏ cạnh 

Xét cây nhỏ nhất:```
1
2
1 2
```Cả hai đỉnh đều có bậc 1 nên$L = 2$. Thuật toán trả về$(2+1)//2 = 1$. Việc nén đường đi giữa hai đỉnh thực sự tạo ra một đỉnh duy nhất trong một thao tác. 

Hãy xem xét một con đường:```
1
5
1 2
2 3
3 4
4 5
```Chỉ có đỉnh 1 và 5 là lá. Thuật toán tìm$L = 2$và trả về 1. Việc chọn đường đi từ đầu này đến đầu kia bao trùm mọi đỉnh, vì vậy một thao tác là tối ưu. 

Hãy xem xét một ngôi sao có năm lá:```
1
6
1 2
1 3
1 4
1 5
1 6
```Các lá là 2, 3, 4, 5 và 6, vậy$L = 5$. Thuật toán trả về$(5+1)//2 = 3$. Một công thức sai lầm như`L // 2`sẽ trả về 2, điều này là không thể vì mỗi thao tác sẽ loại bỏ tối đa hai lá. Bộ phận trần xử lý số lá lẻ một cách chính xác.
