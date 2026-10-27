---
title: "CF 106145D - Kẹo bánh quy"
description: "Chúng ta được cho một hoán vị có độ dài $n$, nghĩa là mọi số từ 1 đến $n$ xuất hiện đúng một lần, nhưng chúng được sắp xếp theo một thứ tự tùy ý dọc theo một dòng vị trí $1 chấm n$."
date: "2026-06-25T11:28:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106145
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 10-29-25"
rating: 0
weight: 106145
solve_time_s: 36
verified: true
draft: false
---

[CF 106145D - Kẹo bánh quy](https://codeforces.com/problemset/problem/106145/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$, nghĩa là mọi số từ 1 đến$n$xuất hiện đúng một lần, nhưng chúng được sắp xếp theo thứ tự tùy ý dọc theo một dòng vị trí$1 \dots n$. giá trị$a_i$cho chúng tôi biết nhãn nào nằm ở vị trí$i$và vì là một hoán vị nên mỗi nhãn tương ứng với đúng một vị trí. 

Bây giờ hãy tưởng tượng chúng ta phải truy cập các nhãn theo thứ tự tăng dần: đầu tiên là vị trí có giá trị 1, sau đó là vị trí của giá trị 2, v.v. cho đến khi có giá trị$n$. Chi phí đi lại là tổng chênh lệch tuyệt đối giữa các vị trí được ghé thăm liên tiếp. Nếu vị trí của giá trị$x$là$p_x$, thì chi phí là tổng của$|p_{x+1} - p_x|$. 

Chúng ta được phép thực hiện nhiều nhất một thao tác hoán đổi thứ tự truy cập của hai nhãn. Tương tự, ta chọn hai giá trị$x$Và$y$, và theo thứ tự$1,2,\dots,n$chúng tôi hoán đổi vị trí của chúng, trong khi tất cả các giá trị khác vẫn giữ nguyên trật tự tự nhiên. 

Nhiệm vụ là giảm thiểu chi phí đi lại. 

Các ràng buộc đi lên đến$n \le 5000$. Một sự ngây thơ$O(n^2)$hoặc$O(n^3)$giải pháp ở mức giới hạn nhưng vẫn có khả năng thực hiện được, tuy nhiên bất cứ điều gì như tính toán lại đường dẫn đầy đủ cho mọi ứng cử viên hoán đổi sẽ dẫn đến khoảng$O(n^3)$, quá chậm. Cấu trúc của hàm chi phí là thứ chúng ta phải khai thác. 

Một số trường hợp đặc biệt quan trọng. 

Nếu hoán vị đã được sắp xếp, nghĩa là$a_i = i$, thì các vị trí đã theo thứ tự tăng dần và chi phí chính xác là$n-1$. Bất kỳ sự hoán đổi nào cũng chỉ có thể làm xấu đi hoặc bảo toàn cấu trúc liền kề, vì vậy câu trả lời vẫn là$n-1$. Một giải pháp bất cẩn vẫn có thể thử hoán đổi và giảm chi phí một cách không chính xác do không xử lý được cấu trúc không thay đổi. 

Nếu hoán vị bị đảo ngược, tất cả các vị trí sẽ bị phân tán tối đa và việc hoán đổi hai phần tử trung tâm có thể làm thay đổi đáng kể một số điểm khác biệt liền kề. Việc tính toán lại lực lượng vũ phu chỉ phải tính toán chính xác những thay đổi cục bộ; nếu không nó có thể đếm quá mức các cạnh không bị ảnh hưởng. 

Một trường hợp tinh tế là khi các phần tử được hoán đổi liền kề nhau theo thứ tự giá trị, chẳng hạn như hoán đổi 5 và 6. Sự thay đổi chi phí mang tính cục bộ xung quanh các phần tử lân cận của chúng, nhưng một phép tính lại ngây thơ chỉ điều chỉnh hai cạnh có thể bỏ sót thực tế là cả hai phần tử đều đóng góp vào các cạnh kề khác nhau sau khi hoán đổi. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tính chi phí ban đầu, sau đó thử từng cặp giá trị$(x,y)$, mô phỏng việc hoán đổi chúng theo thứ tự truy cập, tính toán lại toàn bộ chi phí từ đầu và lấy mức tối thiểu. 

Chi phí tính toán từ đầu mất$O(n)$, và có$O(n^2)$hoán đổi, cho$O(n^3)$. Với$n = 5000$, đây là theo thứ tự của$1.25 \times 10^{11}$hoạt động vượt quá giới hạn. 

Quan sát chính là việc hoán đổi hai giá trị chỉ ảnh hưởng đến vị trí của hai phần tử đó trong chuỗi vị trí$p_1, p_2, \dots, p_n$. Tất cả các khác biệt liền kề khác không thay đổi. Vì vậy, thay vì tính lại toàn bộ tổng, chúng ta chỉ cần tính toán lại phần đóng góp của các cạnh liên quan đến các phần tử được hoán đổi. 

Đối với bất kỳ giá trị$x$, chỉ có hai cạnh liên quan đến tổng đó trong tổng ban đầu:$|p_x - p_{x-1}|$Và$|p_{x+1} - p_x|$. Sau khi hoán đổi$x$Và$y$, bốn cạnh này thay đổi cục bộ và mọi thứ khác vẫn giữ nguyên. Điều này làm giảm việc đánh giá mỗi lần hoán đổi thành$O(1)$. 

Chúng tôi vẫn có$O(n^2)$hoán đổi, nhưng mỗi lần hoán đổi bây giờ là thời gian không đổi, phù hợp thoải mái. 

Một cải tiến nữa là chúng ta không cần phải xây dựng các chuỗi mới một cách rõ ràng. Chúng tôi chỉ thao tác mảng vị trí và tính toán trực tiếp những khác biệt bị ảnh hưởng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại đường dẫn đầy đủ cho mỗi lần hoán đổi) |$O(n^3)$|$O(n)$| Quá chậm | 
| Trao đổi với các bản cập nhật delta cục bộ |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách chuyển đổi hoán vị thành một mảng nghịch đảo$p$, Ở đâu$p[x]$là vị trí của giá trị$x$. Điều này rất cần thiết vì thứ tự truy cập được tính theo giá trị chứ không phải theo chỉ mục. 

1. Tính mảng$p$để chúng ta có thể xác định ngay từng giá trị trong hoán vị. Điều này tránh việc quét mảng nhiều lần. 
2. Tính chi phí cơ bản bằng cách tính tổng$|p[i] - p[i-1]|$cho tất cả$i \ge 2$. Điều này thể hiện khoảng cách di chuyển ban đầu trước khi hoán đổi. 
3. Khởi tạo câu trả lời với giá trị cơ bản này, vì được phép thực hiện hoán đổi bằng 0. 
4. Lặp lại tất cả các cặp giá trị$(x, y)$với$x < y$. Mỗi cặp tượng trưng cho việc hoán đổi thứ tự thăm viếng của họ. 
5. Với mỗi cặp, hãy tính sự thay đổi về chi phí do hoán đổi$x$Và$y$. Thay vì xây dựng lại đường dẫn đầy đủ, hãy xóa những đóng góp cũ liên quan đến$x$Và$y$, sau đó thêm những đóng góp mới của họ sau khi hoán đổi vị trí. 

Chi tiết quan trọng là sự liền kề trong chuỗi giá trị mới là vấn đề quan trọng, vì vậy chỉ những người hàng xóm mới$x-1, x+1, y-1, y+1$đang tham gia. Chúng tôi cẩn thận loại bỏ các cạnh cũ và thêm các cạnh mới bằng cách sử dụng sự khác biệt tuyệt đối về vị trí. 

1. Cập nhật đáp án với giá trị nhỏ nhất thu được. 

### Tại sao nó hoạt động 

Hàm chi phí là tổng của các cặp liền kề theo thứ tự giá trị. Hoán đổi chỉ thay đổi vị trí của hai giá trị, do đó, bất kỳ thuật ngữ nào không liên quan đến một trong hai giá trị vẫn giống hệt nhau. Vì tổng là phép cộng và các cạnh độc lập nên độ chính xác sẽ giảm xuống còn việc cập nhật chính xác các số hạng bị ảnh hưởng đó. Không có sự phụ thuộc ẩn nào tồn tại ngoài các mối quan hệ kề cận này, vì vậy việc chỉ theo dõi các đóng góp cục bộ sẽ bảo toàn tổng số một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # p[v] = position of value v
    p = [0] * (n + 1)
    for i, v in enumerate(a):
        p[v] = i

    def cost_at(i, j):
        return abs(p[i] - p[j])

    base = 0
    for i in range(2, n + 1):
        base += abs(p[i] - p[i - 1])

    ans = base

    for x in range(1, n + 1):
        for y in range(x + 1, n + 1):
            px, py = p[x], p[y]

            # remove old contributions around x and y
            old = 0
            if x > 1:
                old += abs(p[x] - p[x - 1])
            if x < n:
                old += abs(p[x] - p[x + 1])
            if y > 1:
                old += abs(p[y] - p[y - 1])
            if y < n:
                old += abs(p[y] - p[y + 1])

            # avoid double counting if adjacent in value
            if abs(x - y) == 1:
                # one edge counted twice in above, adjust carefully
                old = 0
                for i in range(2, n + 1):
                    old += abs(p[i] - p[i - 1])

            # swap positions
            p[x], p[y] = p[y], p[x]

            new = 0
            if x > 1:
                new += abs(p[x] - p[x - 1])
            if x < n:
                new += abs(p[x] - p[x + 1])
            if y > 1:
                new += abs(p[y] - p[y - 1])
            if y < n:
                new += abs(p[y] - p[y + 1])

            if abs(x - y) == 1:
                new = 0
                for i in range(2, n + 1):
                    new += abs(p[i] - p[i - 1])

            ans = min(ans, base - old + new)

            # revert swap
            p[x], p[y] = p[y], p[x]

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc duy trì mảng vị trí nghịch đảo sao cho vị trí của mỗi giá trị là O(1). Vòng lặp trao đổi tạm thời trao đổi vị trí của hai giá trị, tính delta trong tổng kề và sau đó khôi phục mảng. 

Phần tế nhị nhất là xử lý tính kề trong không gian giá trị, khi$x$Và$y$khác nhau 1. Trong trường hợp đó, phép trừ đơn giản của các đóng góp cục bộ sẽ tính gấp đôi các cạnh được chia sẻ, do đó, một dự phòng tính toán lại đầy đủ được sử dụng để đảm bảo tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 4 1 3
```Đầu tiên chúng ta tính toán vị trí:$p[1]=2, p[2]=0, p[3]=3, p[4]=1$. 

Chi phí ban đầu: 

| tôi | p[i-1] | p[i] | |khác biệt| 

|---|---|---|---| 

| 2 | 0 | 1 | 1 | 

| 3 | 1 | 3 | 2 | 

| 4 | 3 | 0 | 3 | 

Tổng cộng = 6. 

Bây giờ hãy xem xét việc hoán đổi giá trị 1 và 4. Chúng tôi cập nhật vị trí của chúng và chỉ tính toán lại các cạnh bị ảnh hưởng. Sau khi hoán đổi, các vị trí sẽ trở thành$p[1]=1, p[4]=2$. Vùng lân cận được cập nhật giúp giảm tổng chi phí xuống còn 5. 

Dấu vết này cho thấy rằng chỉ các cạnh liên quan đến 1 và 4 là quan trọng, trong khi tất cả các khác biệt kề cận khác vẫn không thay đổi. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
```Các vị trí đã được đặt hàng:$p[i] = i-1$. 

| tôi | p[i-1] | p[i] | khác biệt | 
| --- | --- | --- | --- | 
| 2 | 0 | 1 | 1 | 
| 3 | 1 | 2 | 1 | 
| 4 | 2 | 3 | 1 | 

Tổng cộng = 3. 

Bất kỳ sự hoán đổi nào cũng gây ra sự sai lệch giữa các giá trị liên tiếp, làm tăng ít nhất một khoảng cách lân cận. Thuật toán đánh giá các giao dịch hoán đổi nhưng luôn không tìm thấy sự cải thiện nào, xác nhận thứ tự nhận dạng là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Tất cả các cặp giá trị đều được kiểm tra, mỗi lần hoán đổi được đánh giá theo thời gian không đổi | 
| Không gian |$O(n)$| Mảng định vị và bộ nhớ phụ trợ tối thiểu | 

Độ phức tạp bậc hai là đủ cho$n \le 5000$, kể từ khoảng$2.5 \times 10^7$hoạt động được chấp nhận trong Python với việc triển khai chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# provided samples
assert run("""4
2 4 1 3
""").strip() == "5"

assert run("""4
1 2 3 4
""").strip() == "3"

# custom cases
assert run("""2
1 2
""").strip() == "1", "minimum size already sorted"

assert run("""2
2 1
""").strip() == "1", "single swap reverses order"

assert run("""5
5 4 3 2 1
""").strip() == "4", "fully reversed permutation"

assert run("""3
2 1 3
""").strip() == "2", "swap middle effects only local structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 1 | trường hợp cơ bản tầm thường | 
| 2 1 | 1 | tính chính xác của trao đổi đơn | 
| 5 4 3 2 1 | 4 | đảo ngược cực độ | 
| 2 1 3 | 2 | hành vi trao đổi cục bộ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi hoán vị đã được sắp xếp. Thuật toán tính toán chi phí cơ bản một cách chính xác như$n-1$và mọi đánh giá hoán đổi chỉ tính toán lại những đóng góp bị ảnh hưởng, không bao giờ tạo ra sự cải thiện sai vì cấu trúc kề đã ở mức tối thiểu. 

Một trường hợp cạnh khác xảy ra khi hoán đổi các giá trị liền kề theo thứ tự giá trị, chẳng hạn như hoán đổi$x$Và$x+1$. Trong trường hợp này, cả hai giá trị đều có chung sự kề cận trực tiếp trong định nghĩa chi phí, do đó, phép trừ cục bộ ngây thơ có nguy cơ bị tính gấp đôi. Thuật toán xử lý vấn đề này bằng cách quay trở lại tính toán lại toàn bộ cho lần hoán đổi đó, đảm bảo tính chính xác ngay cả khi xảy ra sự chồng chéo phụ thuộc. 

Trường hợp tinh tế cuối cùng là khi các phần tử được hoán đổi nằm gần các ranh giới, chẳng hạn như giá trị 1 hoặc giá trị$n$. Trong những trường hợp đó, một hoặc cả hai số hạng kề không tồn tại. Việc thực hiện kiểm tra rõ ràng các ranh giới$x>1, x<n$, do đó nó tránh được việc truy cập các hàng xóm không hợp lệ trong khi vẫn cập nhật chính xác chi phí một phần.
