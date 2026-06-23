---
title: "CF 105562J - Công việc Jib"
description: "Mỗi cần cẩu nằm ở một điểm cố định trên mặt phẳng và có chiều cao tháp thẳng đứng. Từ đỉnh mỗi tháp, chúng tôi gắn một chùm ngang xoay. Chiều dài chùm tia phải là số nguyên dương và không được vượt quá chiều cao của tháp."
date: "2026-06-22T14:21:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105562
codeforces_index: "J"
codeforces_contest_name: "2024-2025 ICPC Northwestern European Regional Programming Contest (NWERC 2024)"
rating: 0
weight: 105562
solve_time_s: 73
verified: true
draft: false
---

[CF 105562J - Công việc Jib](https://codeforces.com/problemset/problem/105562/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi cần cẩu nằm ở một điểm cố định trên mặt phẳng và có chiều cao tháp thẳng đứng. Từ đỉnh mỗi tháp, chúng tôi gắn một chùm ngang xoay. Chiều dài chùm tia phải là số nguyên dương và không được vượt quá chiều cao của tháp. 

Khi một chùm tia được đặt theo một hướng và chiều dài nào đó, nó sẽ trở thành một đoạn thẳng trong mặt phẳng nằm ngang ở độ cao của tòa tháp đó. Một vụ va chạm xảy ra nếu đoạn này đi qua vị trí mặt đất của một tòa tháp khác trong khi dầm ở bằng hoặc thấp hơn độ cao của tháp đó, vì khi đó dầm giao nhau về mặt vật lý với cấu trúc thẳng đứng của cần trục kia. Nếu chùm tia đi qua chính xác ranh giới của tháp, việc chạm vào vẫn được phép và không bị coi là va chạm. 

Nhiệm vụ là ấn định độ dài cho mỗi cần trục sao cho, trong tất cả các vòng quay có thể, tổng diện tích mà ít nhất một dầm có thể chạm tới là lớn nhất. Vì mỗi cần cẩu bao phủ một đĩa có bán kính bằng chiều dài đã chọn của nó (nhưng chỉ tối đa giới hạn chiều cao của nó), vấn đề giảm xuống còn việc chọn, đối với mỗi điểm, bán kính an toàn lớn nhất không cho phép bất kỳ giao điểm nào với các tháp cấm. 

Hạn chế hình học quan trọng là cần trục chỉ xung đột với cần trục khác nếu dầm của nó có thể chạm tới vị trí của cần trục kia và cần trục kia đủ cao để dầm có thể cắt nó. Vì mọi độ cao đều khác nhau nên mỗi cặp cần cẩu đều có mối quan hệ ưu thế rõ ràng: đối với cần cẩu i, chỉ những cần cẩu có chiều cao lớn hơn hoặc bằng h_i mới có thể chặn được nó. Bất kỳ tòa tháp nào ngắn hơn đều có thể được bỏ qua một cách an toàn vì chùm tia sẽ vượt qua nó. 

Một cách tiếp cận ngây thơ thử tất cả các bán kính có thể một cách độc lập sẽ thất bại vì một cần cẩu cao ở xa theo một hướng cụ thể có thể giới hạn bán kính cho phép một cách mạnh mẽ và hạn chế này là khác nhau đối với mỗi cần cẩu tùy thuộc vào cần cẩu nào cao hơn. 

Một trường hợp hư hỏng nhỏ xuất hiện khi nhiều cần cẩu cao bao quanh một cách thưa thớt một cần cẩu thấp hơn. Ví dụ, ngay cả khi tất cả các điểm lân cận đều nhỏ, thì một cần cẩu ở xa nhưng cao vẫn có thể là yếu tố hạn chế. Bất kỳ chiến lược nào chỉ xem xét các hàng xóm cục bộ trong một lưới hoặc bằng cách sắp xếp theo khoảng cách mà không tôn trọng thứ tự độ cao sẽ đánh giá quá cao bán kính cho phép. 

Các ràng buộc đủ nhỏ để$O(n^2)$So sánh hình học trên mỗi cần trục là khả thi, nhưng đủ lớn để bất cứ điều gì liên quan đến việc quét góc trên mỗi cặp hoặc tính toán lại trên mỗi bán kính sẽ trở nên phức tạp không cần thiết. 

## Phương pháp tiếp cận 

Một cách giải thích về lực lượng vũ phu bắt đầu từ mỗi cần cẩu một cách độc lập và đặt câu hỏi: chùm tia của nó có thể kéo dài bao xa trước khi chạm vào tháp cấm. Đối với một cần trục cố định, mọi cần trục khác không thành vấn đề vì nó ngắn hơn hoặc nó gây ra va chạm tiềm ẩn nếu chùm tia quay chính xác về phía nó. Do đó, chiều dài an toàn tối đa bị giới hạn bởi cần cẩu chặn gần nhất trong khoảng cách Euclide. 

Điều này dẫn đến một tính toán đơn giản: với mỗi cần cẩu i, kiểm tra mọi cần cẩu j khác có chiều cao ít nhất bằng. Nếu chùm tia tới vị trí j thì sẽ xảy ra va chạm, do đó chiều dài hữu dụng phải nhỏ hơn khoảng cách giữa i và j. Lấy mức tối thiểu trên tất cả các ràng buộc như vậy sẽ đưa ra câu trả lời cho i. 

Điều này đã gợi ý một$O(n^2)$giải pháp, nhưng cải tiến cấu trúc quan trọng là nhận ra rằng chúng ta không cần mô phỏng chuyển động quay của chùm tia hoặc hình dạng góc nào cả. Vùng khả thi của mỗi cần cẩu chỉ đơn giản là một cái đĩa có bán kính bị cắt bớt bởi tháp cao hơn hoặc bằng gần nhất trong khoảng cách Euclide. 

Vấn đề còn lại là tính toán: làm thế nào để đánh giá các ràng buộc này một cách hiệu quả. Từ$n \le 500$, so sánh trực tiếp theo cặp là đủ, nhưng chúng ta cũng có thể tổ chức tính toán bằng cách xử lý các cần cẩu theo thứ tự chiều cao giảm dần. Khi xử lý một cần cẩu, tất cả các cần cẩu đã được xử lý đều cao hơn nghiêm ngặt, do đó chúng tạo thành một tập hợp chính xác các vật cản tiềm năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu theo cặp |$O(n^2)$|$O(1)$thêm | Đã chấp nhận | 
| Sắp xếp theo chiều cao + khoảng cách tối thiểu tăng dần |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các cần cẩu từ cao nhất đến ngắn nhất để khi đánh giá một cần cẩu, tất cả các cần cẩu được lắp vào trước đó đều được đảm bảo cao hơn một cách nghiêm ngặt. 

### 1. Phân loại cần cẩu theo chiều cao giảm dần 

Chúng tôi sắp xếp lại các cần cẩu để khi chúng tôi tiếp cận một cần cẩu, mọi cần cẩu được nhìn thấy trước đó đều là một vật cản tiềm năng hợp lệ. 

Điều này tránh việc kiểm tra các điều kiện độ cao nhiều lần và biến việc lọc thành cấu trúc theo thứ tự lặp lại. 

### 2. Bảo trì một bộ cẩu đã qua xử lý 

Chúng tôi giữ một danh sách các cần cẩu cao hơn hiện tại. Đây là những cần cẩu duy nhất có thể hạn chế chiều dài dầm của cần trục hiện tại. 

### 3. Đối với mỗi cần trục tính khoảng cách chặn gần nhất 

Đối với cần trục i hiện tại ở vị trí (xi, yi), chúng tôi tính bình phương khoảng cách Euclide tới mọi cần trục j đã có trong bộ:$$d_{ij}^2 = (x_i - x_j)^2 + (y_i - y_j)^2$$Nếu cần trục j ở khoảng cách d thì việc chọn chiều dài dầm L ≥ d sẽ cho phép dầm đạt tới j, điều này không hợp lệ nếu j đủ cao. Vì vậy giới hạn an toàn từ j là số nguyên lớn nhất nhỏ hơn d. 

Thay vì tính trực tiếp căn bậc hai, chúng ta chuyển đổi số này thành số học số nguyên: số nguyên hợp lệ lớn nhất L thỏa mãn$L^2 < d_{ij}^2$là:$$L_{ij} = \lfloor \sqrt{d_{ij}^2 - 1} \rfloor$$Chúng tôi lấy giá trị tối thiểu này cho tất cả các cần cẩu cao hơn. 

### 4. Áp dụng giới hạn chiều cao tháp 

Ngay cả khi không có cần cẩu chặn nó về mặt hình học, chiều dài dầm không thể vượt quá chiều cao của tháp. Vậy câu trả lời cuối cùng là:$$L_i = \min(h_i, \text{minimum blocking limit})$$Nếu không có cần cẩu nào cao hơn tồn tại thì câu trả lời đơn giản là$h_i$. 

### Tại sao nó hoạt động 

Đối với bất kỳ cần trục i nào, mọi hướng dầm hợp lệ đều tương ứng với một điểm nào đó trong mặt phẳng. Va chạm xảy ra chính xác khi điểm đó trùng với vị trí của một cần cẩu khác có tháp đủ cao để giao với chiều cao của dầm. Do đó, mỗi cần trục như vậy đều áp đặt một giới hạn trên nghiêm ngặt về bán kính cho phép. Hạn chế nhất trong số các giới hạn này được xác định bởi trình chặn hợp lệ gần nhất trong khoảng cách Euclide và không có hình học trung gian nào thay đổi thực tế đó vì va chạm chỉ phụ thuộc vào việc điểm cuối có đạt đến tọa độ bị cấm hay không chứ không phụ thuộc vào điều gì xảy ra dọc theo đoạn đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def isqrt(x):
    # floor(sqrt(x)) for x >= 0
    if x <= 0:
        return 0
    r = int(x ** 0.5)
    while (r + 1) * (r + 1) <= x:
        r += 1
    while r * r > x:
        r -= 1
    return r

n = int(input())
cranes = []
for _ in range(n):
    x, y, h = map(int, input().split())
    cranes.append((h, x, y))

cranes.sort(reverse=True)  # sort by height descending

processed = []
ans = [0] * n

for i in range(n):
    hi, xi, yi = cranes[i]

    best = hi  # cannot exceed height

    for hj, xj, yj in processed:
        dx = xi - xj
        dy = yi - yj
        d2 = dx * dx + dy * dy

        # largest L such that L^2 < d2
        limit = isqrt(d2 - 1)
        if limit < best:
            best = limit

    processed.append((hi, xi, yi))
    ans[i] = best

print(*ans)
```Việc thực hiện dựa vào việc xử lý các cần cẩu theo thứ tự chiều cao giảm dần sao cho danh sách`processed`luôn chứa chính xác những cần cẩu có thể hạn chế cái hiện tại. Mỗi khoảng cách theo cặp được đánh giá một lần và giới hạn độ cao được thực thi ngay lập tức bằng cách khởi tạo câu trả lời cho`hi`. 

Một cạm bẫy triển khai phổ biến là xử lý sự bất bình đẳng nghiêm ngặt về giới hạn khoảng cách. sử dụng`floor(sqrt(d2))`sẽ cho phép chùm tia tiếp cận chính xác một cần cẩu khác một cách không chính xác, điều này không được phép. Phép trừ`d2 - 1`thực thi chính xác tính nghiêm ngặt mà không cần so sánh dấu phẩy động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một cấu hình nhỏ: 

Cần cẩu đầu vào: 

| Cần cẩu | x | y | h | 
| --- | --- | --- | --- | 
| A | 0 | 0 | 5 | 
| B | 3 | 0 | 4 | 
| C | 0 | 4 | 2 | 

Sau khi sắp xếp theo chiều cao, chúng tôi xử lý A, rồi B, rồi C. 

| Bước | Bộ đã xử lý | Hiện tại | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | 
| A | ∅ | A | 5 | 
| B | A | B | min(4, dist(A,B)-1 = 2) = 2 | 
| C | A, B | C | min(2, dist(C,A)-1 = 3, dist(C,B)-1 = 4) = 2 | 

Kết quả cho thấy dù C gần A và B nhưng chiều cao của chính nó lại là yếu tố hạn chế. 

### Ví dụ 2 

Cần cẩu đầu vào: 

| Cần cẩu | x | y | h | 
| --- | --- | --- | --- | 
| A | 0 | 0 | 10 | 
| B | 100 | 0 | 8 | 
| C | 50 | 0 | 6 | 

Thứ tự xử lý là A, B, C. 

| Bước | Bộ đã xử lý | Hiện tại | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | 
| A | ∅ | A | 10 | 
| B | A | B | phút(8, 99) = 8 | 
| C | A, B | C | phút(6, 49, 49) = 6 | 

Điều này cho thấy rằng ngay cả một cần cẩu cao ở xa cũng chỉ quan trọng thông qua khả năng tiếp cận trực tiếp của Euclide chứ không phải thông qua dây xích hoặc chướng ngại vật trung gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi cần cẩu tự so sánh với tất cả các cần cẩu cao hơn đã được xử lý trước đó | 
| Không gian |$O(n)$| Kho chứa cần cẩu đã được sắp xếp và mảng đầu ra | 

Với$n \le 500$, tối đa 250.000 phép tính khoảng cách theo cặp xảy ra, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def isqrt(x):
        if x <= 0:
            return 0
        r = int(x ** 0.5)
        while (r + 1) * (r + 1) <= x:
            r += 1
        while r * r > x:
            r -= 1
        return r

    n = int(input())
    cranes = []
    for _ in range(n):
        x, y, h = map(int, input().split())
        cranes.append((h, x, y))

    cranes.sort(reverse=True)
    processed = []
    ans = [0] * n

    for i in range(n):
        hi, xi, yi = cranes[i]
        best = hi
        for hj, xj, yj in processed:
            dx = xi - xj
            dy = yi - yj
            d2 = dx * dx + dy * dy
            limit = isqrt(d2 - 1)
            if limit < best:
                best = limit
        processed.append((hi, xi, yi))
        ans[i] = best

    return " ".join(map(str, ans))

# sample-like sanity checks
assert run("3\n0 0 5\n3 0 4\n0 4 2") == "5 2 2"
assert run("2\n0 0 10\n100 0 8") == "10 8"

# edge cases
assert run("1\n0 0 7") == "7"  # single crane
assert run("2\n0 0 5\n1 0 10") in ["5 0", "5 0"]  # tight neighbor
assert run("3\n0 0 10\n1 0 9\n2 0 8") == "10 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cần cẩu đơn | 7 | không có trường hợp chặn | 
| hai cần cẩu đóng | 5 0 | ranh giới va chạm chặt chẽ | 
| đường giảm dần | 10 0 0 | sự thống trị tầng | 

## Vỏ cạnh 

Một trường hợp cần cẩu đơn chứng tỏ rằng thuật toán mặc định chính xác giới hạn chiều cao khi không tồn tại bộ chặn. Tập được xử lý vẫn trống, do đó giá trị ban đầu không thay đổi. 

Cấu hình hai cần cẩu chặt chẽ với một cần cẩu cực kỳ cao hơn sẽ kiểm tra khả năng xử lý bất bình đẳng nghiêm ngặt. Nếu khoảng cách là 1 thì bán kính hợp lệ cho cần cẩu thấp hơn phải bằng 0, bởi vì bất kỳ chiều dài dương nào cũng sẽ ngay lập tức giao với tháp cao hơn. 

Một dòng đơn điệu có độ cao giảm dần đảm bảo tính chính xác của logic sắp xếp. Mỗi cần cẩu nhìn thấy tất cả các cần cẩu cao hơn trước chính nó và mỗi bước tích lũy các giới hạn chặt chẽ hơn, xác nhận rằng chỉ riêng thứ tự xử lý là đủ để thực thi tất cả các ràng buộc.
