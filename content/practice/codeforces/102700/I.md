---
title: "CF 102700I - Khả năng chụp ảnh đáng kinh ngạc"
description: "Chúng ta có một dãy gồm (n) tòa nhà, trong đó tòa nhà (i) có chiều cao (hi). Paula có thể bắt đầu ở bất kỳ tòa nhà nào và liên tục di chuyển đến tòa nhà cao hơn tòa nhà hiện tại của cô ấy và có thể nhìn thấy được từ đó."
date: "2026-08-16T17:54:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "I"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 153
verified: true
draft: false
---

[CF 102700I - Khả năng chụp ảnh đáng kinh ngạc](https://codeforces.com/problemset/problem/102700/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy gồm (n) tòa nhà, trong đó tòa nhà (i) có chiều cao (h_i). Paula có thể bắt đầu ở bất kỳ tòa nhà nào và liên tục di chuyển đến tòa nhà cao hơn tòa nhà hiện tại của cô ấy và có thể nhìn thấy được từ đó. Nếu cô ấy di chuyển từ (i) đến (j), khoảng cách được thêm vào bước đi của cô ấy là (|i-j|). 

Tòa nhà (j) hiển thị từ (i) khi mọi tòa nhà nằm giữa chúng đều có chiều cao tối đa (h_j). Một tòa nhà có cùng chiều cao với (j) không chặn tầm nhìn vì chỉ có tòa nhà cao hơn mới chặn nó. 

Đối với mỗi tòa nhà bắt đầu (i), chúng ta cần tổng khoảng cách tối đa trên bất kỳ chuỗi di chuyển hợp lệ nào. Vì mọi chuyển động đều đi đến một tòa nhà cao hơn, nên một tuyến đường hợp lệ không bao giờ có thể quay trở lại độ cao trước đó, do đó đồ thị chuyển động có tính chất không theo chu kỳ. 

Ràng buộc quan trọng là (n\le 10^5). Thuật toán (O(n^2)) đã thực hiện kiểm tra cặp (5\cdot10^9) trong trường hợp xấu nhất, vượt xa giới hạn một giây có thể xử lý. Chúng ta cần một giải pháp (O(n\log n)) hoặc tốt hơn. Độ cao có thể lớn tới (10^9), nhưng chúng chỉ tham gia so sánh nên độ lớn của chúng không ảnh hưởng đến cấu trúc dữ liệu. 

Có một số trường hợp ranh giới trong đó việc triển khai có thể âm thầm gặp trục trặc. Ví dụ, với một tòa nhà,`1 / 15`có câu trả lời`0`, bởi vì không có nơi nào để di chuyển. Việc triển khai giả định mọi tòa nhà đều có ứng cử viên bên trái hoặc bên phải có thể truy cập vào một chỉ mục không hợp lệ. 

Chiều cao bằng nhau cần được chăm sóc nhiều hơn. Vì`3 / 1 3 3`, câu trả lời là`2 0 0`. Tòa nhà đầu tiên có thể nhìn thấy cả hai tòa nhà có chiều cao (3), bao gồm cả tòa nhà xa hơn, bởi vì tòa nhà có chiều cao gần hơn-(3) không hoàn toàn cao hơn tòa nhà xa hơn và do đó không chặn nó. Cách tiếp cận chỉ dựa trên tòa nhà cao hơn gần nhất sẽ chỉ xem xét sai cách đầu tiên (3). 

Hai hướng cũng phải được xử lý độc lập. Vì`3 / 5 4 3`, câu trả lời đúng là`0 1 2`. Tòa nhà (2) có thể di chuyển sang trái đến tòa nhà (1), trong khoảng cách (2), trong khi không có di chuyển hợp lệ sang bên phải. Việc triển khai chỉ tìm kiếm ở bên phải sẽ bỏ lỡ tất cả các tuyến đường này. 

## Phương pháp tiếp cận 

Giải pháp lập trình động trực tiếp sẽ xem xét mọi tòa nhà tiếp theo có thể. Đối với tòa nhà cố định (i), hãy quét sang bên phải trong khi vẫn duy trì chiều cao tối đa gặp phải và thực hiện tương tự ở bên trái. Bất cứ khi nào nhìn thấy tòa nhà cao hơn (j), nó sẽ chuyển tiếp 

[ 
dp[i] = \max(dp[i], |i-j|+dp[j]). 
] 

Các tòa nhà có thể được xử lý theo chiều cao giảm dần, do đó (dp[j]) đã được biết bất cứ khi nào (j) cao hơn (i). Lực lượng vũ phu này là chính xác bởi vì nó xem xét rõ ràng mọi bước đi đầu tiên hợp pháp. 

Vấn đề là số lượng ứng viên. Hãy cân nhắc việc tăng chiều cao một cách nghiêm túc. Từ mọi tòa nhà, mọi tòa nhà bên phải đều có thể nhìn thấy được. Thuật toán phải kiểm tra 

[ 
\frac{n(n-1)}2 
] 

cặp ứng cử viên, là (4.999.950.000) cặp khi (n=100000). Việc kiểm tra mức độ hiển thị trực tiếp cho từng cặp có thể còn tệ hơn, đạt tới (O(n^3)) nếu các tòa nhà giữa cặp được quét mỗi lần. 

Quan sát hữu ích là chúng ta có thể đảo ngược quá trình chuyển đổi. Sửa một tòa nhà cao hơn (j) và hỏi những tòa nhà thấp hơn có thể nhìn thấy nó. 

Gọi (L_j) là tòa nhà gần nhất cao hơn (j) ở bên trái. Khi đó mọi tòa nhà (i) thỏa mãn 

[ 
L_j < tôi < j 
] 

và (h_i<h_j) có thể thấy (j). Không thể có tòa nhà nào cao hơn (h_j) giữa (i) và (j), vì (L_j) là tòa nhà gần nhất như vậy. Ngược lại, nếu (i\le L_j), tòa nhà (L_j) chặn tầm nhìn của (j). 

Lập luận tương tự được áp dụng ở bên phải. Nếu (R_j) là tòa nhà cao hơn gần nhất ở bên phải thì mọi tòa nhà thấp hơn (i) có 

[ 
j<i<R_j 
] 

có thể nhìn thấy (j). 

Điều này chuyển đổi một tòa nhà (j) thành hai bản cập nhật phạm vi. Đối với tòa nhà thấp hơn (i<j), quá trình chuyển đổi qua (j) có giá trị 

[ 
dp[j]+j-i=(dp[j]+j)-i. 
] 

Biểu thức có dạng hằng số trừ (i), vì vậy chúng ta có thể lưu hằng số (dp[j]+j) trên một khoảng. Đối với tòa nhà thấp hơn (i>j), quá trình chuyển đổi là 

[ 
dp[j]+i-j=(dp[j]-j)+i, 
] 

vì vậy chúng tôi lưu trữ (dp[j]-j) vào một khoảng thời gian khác. 

Chỉ cần cập nhật phạm vi tối đa theo sau là truy vấn điểm là đủ. Chúng tôi xử lý các tòa nhà bằng cách giảm chiều cao. Tất cả các tòa nhà có cùng chiều cao đều được truy vấn trước khi bất kỳ tòa nhà nào cập nhật cấu trúc dữ liệu. Chi tiết này xử lý các độ cao bằng nhau một cách chính xác, vì Paula không được phép di chuyển giữa các tòa nhà có chiều cao bằng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán (L_i), chỉ số gần nhất ở bên trái có chiều cao lớn hơn (h_i). Một ngăn xếp đơn điệu giảm dần sẽ tìm thấy tất cả các vị trí này trong (O(n)). Mặc dù đỉnh của ngăn xếp có chiều cao tối đa là (h_i), hãy loại bỏ nó vì nó không thể là tòa nhà lớn hơn gần nhất đối với (i). 
2. Tính toán (R_i), chỉ mục gần nhất ở bên phải có chiều cao lớn hơn (h_i), sử dụng cùng một ý tưởng ngăn xếp đơn điệu trong khi quét từ phải sang trái. Nếu không có tòa nhà như vậy tồn tại, hãy sử dụng (n) làm lính canh. 
3. Sắp xếp chỉ số công trình theo chiều cao giảm dần. Các tòa nhà có chiều cao bằng nhau được giữ trong cùng một nhóm. Chúng ta cần nhóm này vì tòa nhà có chiều cao (H) chỉ có thể di chuyển đến độ cao lớn hơn (H), không bao giờ đến tòa nhà khác có chiều cao (H). 
4. Duy trì một cấu trúc range-chmax cho các bước di chuyển từ bên phải và một cấu trúc khác cho các bước di chuyển từ bên trái. Cấu trúc đầu tiên lưu trữ các giá trị (dp[j]+j) và cấu trúc thứ hai lưu trữ các giá trị (dp[j]-j). Truy vấn điểm tại (i) đưa ra hằng số được lưu trữ tốt nhất áp dụng cho (i). 
5. Đối với mọi tòa nhà (i) trong nhóm chiều cao hiện tại, hãy truy vấn cả hai cấu trúc trước khi áp dụng bất kỳ cập nhật nào từ nhóm này. Nếu cấu trúc bên trái trả về (A), tuyến đường tương ứng có giá trị (A-i). Nếu cấu trúc bên phải trả về (B), tuyến đường tương ứng có giá trị (B+i). Đặt (dp[i]) thành giá trị tối đa của các giá trị này và bằng 0. 
6. Sau khi tất cả các tòa nhà trong nhóm chiều cao hiện tại có giá trị (dp), hãy cập nhật cấu trúc cho từng tòa nhà (j). Khoảng bên trái là ([L_j+1,j)), vì đó là các vị trí bên trái có thể nhìn thấy (j). Lưu trữ (dp[j]+j) ở đó. Khoảng bên phải là ([j+1,R_j)) và chúng tôi lưu trữ (dp[j]-j) ở đó. 
7. Sau khi tất cả các nhóm chiều cao đã được xử lý, hãy in mảng (dp) kết quả. Mọi chuyển đổi được sử dụng để tính toán giá trị đều đến từ một tòa nhà cao hơn nghiêm ngặt mà câu trả lời đã được hoàn thiện. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ sự chuyển đổi pháp lý nào từ tòa nhà thấp hơn (i) sang tòa nhà cao hơn (j) với (i<j). Quá trình chuyển đổi có hiệu lực chính xác khi không có tòa nhà nào cao hơn (h_j) nằm giữa chúng. Theo định nghĩa, (L_j) là tòa nhà gần nhất ở bên trái, vì vậy điều kiện này tương đương với (L_j<i<j). Do đó, bản cập nhật do (j) tạo ra bao gồm chính xác các tòa nhà hợp lệ thấp hơn ở bên trái của nó. Phía bên phải là đối xứng. 

Đối với mỗi lần chuyển đổi như vậy, cấu trúc dữ liệu lưu trữ (dp[j]+j), do đó, một truy vấn tại (i) sẽ tái tạo lại (dp[j]+j-i), chính xác là khoảng cách (j-i) cộng với tuyến đường tốt nhất bắt đầu tại (j). Vì tất cả các cập nhật từ các nhóm có chiều cao cao hơn đã được áp dụng nên mọi bước đi đầu tiên có thể được biểu thị khi (dp[i]) được tính toán. Các tòa nhà có chiều cao bằng nhau không được phép di chuyển và việc trì hoãn tất cả các bản cập nhật từ một nhóm có chiều cao bằng nhau sẽ ngăn chúng ảnh hưởng lẫn nhau. Do đó, mức tối đa được tạo ra cho mỗi tòa nhà chính xác là khoảng cách đi bộ hợp pháp tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

NEG = -10**30

class RangeChmaxPointQuery:
    def __init__(self, n):
        size = 1
        while size < n:
            size <<= 1
        self.size = size
        self.tree = [NEG] * (2 * size)

    def update(self, left, right, value):
        if left >= right:
            return

        size = self.size
        tree = self.tree

        left += size
        right += size

        while left < right:
            if left & 1:
                if value > tree[left]:
                    tree[left] = value
                left += 1

            if right & 1:
                right -= 1
                if value > tree[right]:
                    tree[right] = value

            left >>= 1
            right >>= 1

    def query(self, pos):
        tree = self.tree
        pos += self.size
        result = NEG

        while pos:
            if tree[pos] > result:
                result = tree[pos]
            pos >>= 1

        return result

def solve():
    n = int(input())
    h = list(map(int, input().split()))

    left_greater = [-1] * n
    right_greater = [n] * n

    stack = []

    for i in range(n):
        while stack and h[stack[-1]] <= h[i]:
            stack.pop()

        if stack:
            left_greater[i] = stack[-1]

        stack.append(i)

    stack = []

    for i in range(n - 1, -1, -1):
        while stack and h[stack[-1]] <= h[i]:
            stack.pop()

        if stack:
            right_greater[i] = stack[-1]

        stack.append(i)

    order = sorted(range(n), key=h.__getitem__, reverse=True)

    left_tree = RangeChmaxPointQuery(n)
    right_tree = RangeChmaxPointQuery(n)

    dp = [0] * n

    p = 0
    while p < n:
        q = p + 1
        height = h[order[p]]

        while q < n and h[order[q]] == height:
            q += 1

        for k in range(p, q):
            i = order[k]

            best = 0

            a = left_tree.query(i)
            if a != NEG:
                candidate = a - i
                if candidate > best:
                    best = candidate

            b = right_tree.query(i)
            if b != NEG:
                candidate = b + i
                if candidate > best:
                    best = candidate

            dp[i] = best

        for k in range(p, q):
            j = order[k]

            left_tree.update(
                left_greater[j] + 1,
                j,
                dp[j] + j
            )

            right_tree.update(
                j + 1,
                right_greater[j],
                dp[j] - j
            )

        p = q

    print(*dp)

if __name__ == "__main__":
    solve()
```Thẻ ngăn xếp đơn điệu đầu tiên tính toán tòa nhà cao hơn gần nhất ở bên trái. các`<=`so sánh là có chủ ý. Một tòa nhà có chiều cao bằng nhau không được tính là cao hơn một cách nghiêm ngặt, vì vậy nó phải được xóa khỏi ngăn xếp trước khi trình chặn hợp lệ gần nhất được chọn. 

Lần thứ hai thực hiện phép tính đối xứng cho ranh giới bên phải. sử dụng`n`vì ranh giới bên phải bị thiếu khiến cho việc cập nhật phạm vi sau này trở nên tự nhiên`[j+1,n)`không có xử lý đặc biệt. 

các`RangeChmaxPointQuery`cấu trúc sử dụng cây phân đoạn lặp. Nó không cần lan truyền lười biếng theo nghĩa thông thường vì các bản cập nhật chỉ là các bài tập có phạm vi tối đa và các truy vấn chỉ là điểm. Một phạm vi được phân tách thành các nút cây chính tắc (O(\log n)) và một truy vấn điểm chiếm mức tối đa trên các nút (O(\log n)) trên đường dẫn gốc của nó. 

Hai cây tượng trưng cho hai dấu hiệu của khoảng cách. Đối với đích (j) ở bên phải của (i), biểu thức là`dp[j] + j - i`, vì vậy giá trị được lưu trữ là`dp[j] + j`. Đối với điểm đến (j) ở bên trái, đó là`dp[j] - j + i`, vì vậy giá trị được lưu trữ là`dp[j] - j`. 

Thứ tự truy vấn và cập nhật trong một nhóm có chiều cao bằng nhau là điều cần thiết. Nếu tòa nhà có chiều cao-(H) cập nhật cấu trúc trước khi tòa nhà có chiều cao-(H) khác được xử lý thì tòa nhà thứ hai có thể sử dụng sai tòa nhà đầu tiên làm điểm đến. Việc xử lý toàn bộ nhóm trước tiên sẽ ngăn chặn điều đó. 

Số nguyên Python có độ chính xác tùy ý, do đó khoảng cách đi bộ tích lũy lớn có thể không bị tràn. Trong ngôn ngữ có chiều rộng cố định, nên sử dụng loại số nguyên 64 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào```
4
3 1 2 4
```ranh giới lớn hơn gần nhất là 

[ 
L=[-1,0,0,0] 
] 

và 

[ 
R=[3,2,3,4]. 
] 

Bảng sau đây cho thấy trạng thái quan trọng khi xử lý các nhóm chiều cao. các`left query`đại diện cho một (dp[j]+j được lưu trữ), trong khi`right query`đại diện cho một (dp[j]-j được lưu trữ). 

| Nhóm chiều cao | Tòa nhà | Truy vấn bên trái | Truy vấn đúng | (dp) | 
| --- | --- | --- | --- | --- | 
| 4 | 3 | không | không | 0 | 
| 3 | 0 | không | 3 | 3 | 
| 2 | 2 | 3 | 3 | 5 | 
| 1 | 1 | 7 | 3 | 6 | 

Sau khi xử lý tòa nhà (3) có chiều cao (4), bản cập nhật bên trái của nó sẽ lưu trữ (dp[3]+3=3) trên các vị trí (0,1,2). Do đó tòa nhà (0) đã có thể đạt được khoảng cách (3). 

Khi tòa nhà (0) được xử lý, câu trả lời của nó trở thành (3). Bản cập nhật bên phải của nó lưu trữ (dp[0]-0=3) trên các vị trí (1,2). Tại tòa nhà (2), kết quả này cho (3+2=5), tương ứng với tuyến đường (2\to0\to3). 

Cuối cùng, tòa nhà (2) đóng góp (dp[2]+2=7) vào khoảng bên trái của nó, do đó tòa nhà (1) thu được (7-1=6). Kết quả đầu ra là`3 6 5 0`. 

### Mẫu 2 

Đối với đầu vào```
5
3 3 1 5 5
```ranh giới lớn hơn gần nhất là 

[ 
L=[-1,-1,1,-1,-1] 
] 

và 

[ 
R=[3,3,3,5,5]. 
] 

Hai tòa nhà có chiều cao (5) phải được xử lý cùng nhau. Cập nhật của họ chỉ xảy ra sau khi cả hai đã nhận được (dp=0). 

| Nhóm chiều cao | Tòa nhà | Truy vấn bên trái | Truy vấn đúng | (dp) | 
| --- | --- | --- | --- | --- | 
| 5 | 3 | không | không | 0 | 
| 5 | 4 | không | không | 0 | 
| 3 | 0 | 4 | không | 4 | 
| 3 | 1 | 4 | không | 3 | 
| 1 | 2 | 4 | 4 | 6 | 

Tòa nhà (4) có chiều cao (5) cập nhật mọi tòa nhà thấp hơn ở bên trái với giá trị (dp[4]+4=4). Đây là lý do tại sao tòa nhà (1), chẳng hạn, có thể nhìn thấy trực tiếp tòa nhà có chiều cao-(5) xa hơn và đạt được khoảng cách (3), mặc dù tòa nhà có chiều cao-(5) khác nằm giữa chúng. 

Sau khi các tòa nhà (0) và (1) được xử lý, các cập nhật bên phải của chúng tạo ra tuyến đường tốt nhất cho tòa nhà (2) bằng (6). Một tuyến đường tối ưu là (2\to0\to4), với khoảng cách (2) và (4). 

Đầu ra cuối cùng là`4 3 6 0 0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Việc sắp xếp mất (O(n\log n)), trong khi mỗi tòa nhà thực hiện một số lượng không đổi các thao tác trên cây phân đoạn (O(\log n)). | 
| Không gian | (O(n)) | Mảng chiều cao, mảng ranh giới, mảng DP, ngăn xếp và cây hai đoạn đều sử dụng không gian tuyến tính. | 

Với (n\le10^5), giải pháp chỉ thực hiện số lượng thao tác logarit trên mỗi tòa nhà sau bước sắp xếp. Điều này nằm trong giới hạn tiệm cận dự kiến, trong khi mức sử dụng bộ nhớ tuyến tính thấp hơn nhiều so với giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

NEG = -10**30

class RangeChmaxPointQuery:
    def __init__(self, n):
        size = 1
        while size < n:
            size <<= 1
        self.size = size
        self.tree = [NEG] * (2 * size)

    def update(self, left, right, value):
        if left >= right:
            return

        left += self.size
        right += self.size

        while left < right:
            if left & 1:
                self.tree[left] = max(self.tree[left], value)
                left += 1

            if right & 1:
                right -= 1
                self.tree[right] = max(self.tree[right], value)

            left >>= 1
            right >>= 1

    def query(self, pos):
        pos += self.size
        result = NEG

        while pos:
            result = max(result, self.tree[pos])
            pos >>= 1

        return result

def solve_data(data: str) -> str:
    tokens = list(map(int, data.split()))
    n = tokens[0]
    h = tokens[1:n + 1]

    left_greater = [-1] * n
    right_greater = [n] * n

    stack = []

    for i in range(n):
        while stack and h[stack[-1]] <= h[i]:
            stack.pop()

        if stack:
            left_greater[i] = stack[-1]

        stack.append(i)

    stack = []

    for i in range(n - 1, -1, -1):
        while stack and h[stack[-1]] <= h[i]:
            stack.pop()

        if stack:
            right_greater[i] = stack[-1]

        stack.append(i)

    order = sorted(range(n), key=h.__getitem__, reverse=True)

    left_tree = RangeChmaxPointQuery(n)
    right_tree = RangeChmaxPointQuery(n)
    dp = [0] * n

    p = 0
    while p < n:
        q = p + 1

        while q < n and h[order[q]] == h[order[p]]:
            q += 1

        for k in range(p, q):
            i = order[k]
            best = 0

            a = left_tree.query(i)
            if a != NEG:
                best = max(best, a - i)

            b = right_tree.query(i)
            if b != NEG:
                best = max(best, b + i)

            dp[i] = best

        for k in range(p, q):
            j = order[k]

            left_tree.update(
                left_greater[j] + 1,
                j,
                dp[j] + j
            )

            right_tree.update(
                j + 1,
                right_greater[j],
                dp[j] - j
            )

        p = q

    return " ".join(map(str, dp))

def run(inp: str) -> str:
    return solve_data(inp).strip()

# Provided samples
assert run("4\n3 1 2 4\n") == "3 6 5 0", "sample 1"
assert run("1\n15\n") == "0", "sample 2"
assert run("5\n3 3 1 5 5\n") == "4 3 6 0 0", "sample 3"

# Minimum-size input
assert run("1\n7\n") == "0", "single building"

# Maximum-size input with all heights equal
n = 100000
inp = f"{n}\n" + " ".join(["42"] * n) + "\n"
expected = " ".join(["0"] * n)
assert run(inp) == expected, "maximum size and all equal"

# Boundary condition: every useful move is to the left
assert run("5\n5 4 3 2 1\n") == "0 1 2 3 4", "decreasing heights"

# Equal-height visibility and off-by-one boundary
assert run("3\n1 3 3\n") == "2 0 0", "equal-height farther building"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`0`| Kích thước tối thiểu và không có chuyển tiếp hợp lệ | 
|`100000 / 42 42 ... 42`|`0 0 ... 0`| Kích thước tối đa và phân mẻ có chiều cao bằng nhau | 
|`5 / 5 4 3 2 1`|`0 1 2 3 4`| Chuyển tiếp bên trái và ranh giới tại tòa nhà đầu tiên | 
|`3 / 1 3 3`|`2 0 0`| Độ cao bằng nhau vẫn hiển thị và mức tối đa bằng nhau xa hơn không bị chặn nhầm |

 ## Vỏ cạnh 

Đối với một tòa nhà, đầu vào`1 / 7`cho`0`. Cả hai mảng lớn gần nhất chỉ chứa các ranh giới, vì vậy cả cây phân đoạn đều không nhận được bản cập nhật có thể tiếp cận tòa nhà. Giá trị DP của nó vẫn bằng 0, chính xác theo yêu cầu. 

Để có chiều cao bằng nhau, hãy xem xét`3 / 1 3 3`. Cả hai tòa nhà có chiều cao (3) đều được xử lý cùng nhau và nhận được`dp=0`. Sau khi nhóm đó kết thúc, tòa nhà xa hơn ở chỉ số (2) cập nhật tòa nhà thấp hơn ở chỉ số (0), lưu trữ (0+2=2). Tòa nhà (0) do đó nhận được câu trả lời (2). Các tòa nhà có chiều cao bằng nhau không bao giờ cập nhật lẫn nhau vì việc cập nhật của chúng bị trì hoãn cho đến khi toàn bộ nhóm của chúng được đánh giá. 

Đối với một chuỗi có các bước di chuyển hữu ích đều ở bên trái,`5 / 5 4 3 2 1`, tòa nhà lớn hơn gần nhất cho mọi vị trí ngoại trừ tòa nhà đầu tiên nằm ở bên trái. Việc xử lý tòa nhà cao nhất trước tiên sẽ đưa ra câu trả lời là 0, sau đó mỗi tòa nhà thấp hơn sẽ nhận được khoảng cách đến tòa nhà cao nhất cộng với phần tiếp theo đã được tính toán của nó. Các câu trả lời trở thành`0 1 2 3 4`. 

Các ranh giới khoảng cách cũng quan trọng khi không có tòa nhà nào lớn hơn ở một bên. Đối với tòa nhà không có vật chắn lớn hơn ở bên trái,`left_greater`là`-1`, do đó quá trình cập nhật của nó bắt đầu ở vị trí 0. Khi không có trình chặn lớn hơn ở bên phải,`right_greater`là`n`, do đó bản cập nhật của nó kéo dài đến vị trí cuối cùng. Việc sử dụng các điểm cuối toàn diện ở đây sẽ cho phép bản thân tòa nhà đích nhận được bản cập nhật của chính nó một cách không chính xác, đó là lý do tại sao việc triển khai luôn sử dụng các khoảng thời gian nửa mở`[left, right)`.
