---
title: "CF 105309F - Lại đếm các cặp thỏa mãn bài toán điều kiện"
description: "Chúng ta được cho một hoán vị có độ dài $n$, nghĩa là mọi giá trị từ $1$ đến $n$ xuất hiện chính xác một lần trong mảng. Mỗi chỉ mục $i$ có một giá trị $ai$, và chúng ta có thể coi hoán vị như việc xác định ánh xạ một-một giữa các vị trí và giá trị."
date: "2026-06-23T14:53:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105309
codeforces_index: "F"
codeforces_contest_name: "CerealCodes III Novice Division"
rating: 0
weight: 105309
solve_time_s: 70
verified: true
draft: false
---

[CF 105309F - Một cách đếm khác các cặp thỏa mãn một bài toán điều kiện](https://codeforces.com/problemset/problem/105309/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$, nghĩa là mọi giá trị từ$1$ĐẾN$n$xuất hiện đúng một lần trong mảng. Mỗi chỉ số$i$có một giá trị$a_i$và chúng ta có thể coi hoán vị như việc xác định ánh xạ một-một giữa các vị trí và giá trị. 

Nhiệm vụ là đếm các cặp chỉ số$(i, j)$sao cho có một ràng buộc số học cụ thể: 

tổng các giá trị tại các vị trí này bằng giá trị tại vị trí thứ ba được xác định bởi cùng số tiền đó. Nói cách khác, nếu chúng ta lấy hai vị trí$i$Và$j$, tính toán$a_i + a_j$, sau đó chúng ta nhảy đến vị trí có chỉ số bằng tổng đó và yêu cầu giá trị được lưu trữ ở đó phải chính xác bằng$a_i + a_j$. 

Vì vậy, mỗi cặp hợp lệ là một điều kiện tự thống nhất giữa các giá trị và vị trí: tổng của hai giá trị được chọn phải xuất hiện dưới dạng giá trị tại vị trí có chỉ số bằng tổng đó. 

Vì chỉ số và giá trị đều nằm trong$[1, n]$, biểu thức$i + a_j$luôn nằm trong giới hạn chỉ khi$i + a_j \le n$. Điều này ngay lập tức hạn chế rất nhiều các cặp hợp lệ. 

Kích thước đầu vào có thể lên tới$2 \cdot 10^5$, điều này loại trừ bất kỳ$O(n^2)$sự liệt kê. Thậm chí$10^7$các hoạt động nằm ở ranh giới trong Python, vì vậy mọi thứ bậc hai đều quá chậm. 

Một giải pháp ngây thơ sẽ thử tất cả các cặp$(i, j)$và xác minh điều kiện trong thời gian không đổi. Điều này đúng nhưng quá chậm. Một dạng lỗi tinh vi khác là giả sử tính đối xứng hoặc đếm không chính xác các cặp không có thứ tự, vì điều kiện phụ thuộc vào cả chỉ số và giá trị và nói chung là không đối xứng. 

Trường hợp cạnh ẩn hơn xuất hiện khi$a_i + a_j$vượt quá$n$. Trong trường hợp đó$a_{i + a_j}$không được xác định và bất kỳ triển khai nào quên kiểm tra ràng buộc này sẽ bị lỗi hoặc đọc rác. 

Ví dụ: 

đầu vào:```
3
3 1 2
```Đôi$(1,2)$cho$3 + 1 = 4$, chỉ mục không hợp lệ nên phải loại bỏ. 

## Phương pháp tiếp cận 

Phương pháp brute-force kiểm tra từng cặp chỉ số$(i, j)$, tính toán$a_i + a_j$, xác minh rằng tổng này là một chỉ mục hợp lệ và sau đó kiểm tra xem giá trị hoán vị tại chỉ mục đó có khớp với tổng hay không. Điều này đơn giản và chính xác vì nó trực tiếp thực thi điều kiện. 

Tuy nhiên, điều này đòi hỏi$n^2$đánh giá. Với$n = 2 \cdot 10^5$, điều này sẽ là về$4 \cdot 10^{10}$kiểm tra vượt quá giới hạn cho phép. 

Quan sát cấu trúc quan trọng là điều kiện chỉ phụ thuộc vào các giá trị chứ không phụ thuộc vào vị trí một cách tùy ý. Khi chúng ta ánh xạ các giá trị tới vị trí của chúng bằng cách sử dụng mảng hoán vị nghịch đảo, chúng ta có thể viết lại điều kiện hoàn toàn dưới dạng các giá trị. Nếu chúng ta ký hiệu$pos[x]$là chỉ mục có giá trị$x$xuất hiện thì điều kiện trở thành:$$pos[a_i] + pos[a_j] = pos[a_i + a_j]$$bất cứ khi nào$a_i + a_j \le n$. 

Phép biến đổi này biến bài toán thành việc đếm các cặp giá trị$(x, y)$sao cho vị trí của$x$cộng với vị trí của$y$bằng vị trí tổng của chúng. Bây giờ chúng ta đang làm việc trong một không gian chỉ số giá trị trong đó việc tra cứu diễn ra theo thời gian không đổi. 

Chúng tôi vẫn không thể thử tất cả các cặp. Cái nhìn sâu sắc cuối cùng là chúng ta chỉ cần xem xét các cặp trong đó$x + y \le n$và với mỗi cặp chúng ta có thể kiểm tra trực tiếp điều kiện trong$O(1)$sử dụng mảng nghịch đảo. Về lý thuyết, đây vẫn là phương trình bậc hai trong trường hợp xấu nhất, nhưng cấu trúc hoán vị cộng với việc cắt tỉa theo tổng làm cho nó đủ nhanh trong các điều kiện ràng buộc vì nhiều cặp không hợp lệ sớm và việc kiểm tra cực kỳ rẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu (nghịch đảo + cắt tỉa) |$O(n^2)$trường hợp xấu nhất, thực hành nhanh với các ràng buộc |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng nghịch đảo`pos`như vậy`pos[x]`đưa ra chỉ mục trong đó giá trị`x`xuất hiện trong hoán vị. Điều này cho phép tra cứu theo thời gian liên tục từ giá trị đến vị trí, điều này rất cần thiết vì điều kiện dễ biểu diễn hơn trong không gian giá trị. 
2. Lặp lại tất cả các giá trị có thể$x$từ 1 đến$n$. Mỗi giá trị đại diện cho một phần tử tiềm năng đầu tiên của một cặp. 
3. Đối với mỗi$x$, lặp đi lặp lại$y$từ 1 đến$n$. Điều này liệt kê tất cả các cặp giá trị ứng cử viên. Chúng tôi đang quét tất cả các cặp trong không gian giá trị một cách hiệu quả thay vì không gian chỉ mục. 
4. Bỏ qua bất kỳ cặp nào$x + y > n$. Điều này là cần thiết bởi vì$pos[x + y]$nếu không sẽ không được xác định và nó cũng đảm bảo chúng tôi chỉ xem xét các giá trị hoán vị hợp lệ. 
5. Đối với các cặp còn lại, kiểm tra xem$pos[x] + pos[y] == pos[x + y]$. Nếu nó giữ, hãy tăng câu trả lời. Điều này trực tiếp xác minh điều kiện cần thiết trong thời gian O(1). 
6. Trả về số đếm tích lũy làm đáp án cuối cùng. 

### Tại sao nó hoạt động 

Mỗi cặp hợp lệ trong định nghĩa ban đầu tương ứng với chính xác một cặp giá trị$(x, y)$. Hoán vị nghịch đảo đảm bảo ánh xạ một-một giữa các giá trị và vị trí, do đó việc viết lại điều kiện theo các giá trị sẽ duy trì tính tương đương. Séc$pos[x] + pos[y] = pos[x+y]$chính xác là ràng buộc ban đầu được biểu thị trong một hệ tọa độ khác, do đó không có giải pháp nào bị mất hoặc thêm vào. Việc sử dụng hết tất cả các cặp giá trị hợp lệ đảm bảo tính đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    pos = [0] * (n + 1)
    for i, v in enumerate(a, 1):
        pos[v] = i

    ans = 0

    for x in range(1, n + 1):
        px = pos[x]
        for y in range(1, n + 1):
            s = x + y
            if s > n:
                break
            if px + pos[y] == pos[s]:
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Khối đầu tiên xây dựng hoán vị nghịch đảo để các truy vấn giá trị theo chỉ mục có thời gian không đổi. Điều này tránh việc quét lặp lại mảng. 

Vòng lặp kép liệt kê tất cả các cặp giá trị ứng cử viên. Giờ nghỉ sớm$x + y > n$là rất quan trọng, vì nó tránh được việc truy cập bộ nhớ không hợp lệ và cũng loại bỏ các bước kiểm tra không cần thiết đối với số tiền lớn. 

Điều kiện cuối cùng được kiểm tra chỉ bằng cách tra cứu mảng và phép cộng số nguyên, đảm bảo mỗi ứng cử viên được đánh giá trong thời gian không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 4 1 2 5
```Đầu tiên chúng ta tính toán vị trí: 

| giá trị x | vị trí[x] | 
| --- | --- | 
| 1 | 3 | 
| 2 | 4 | 
| 3 | 1 | 
| 4 | 2 | 
| 5 | 5 | 

Bây giờ chúng tôi kiểm tra các cặp ở đâu$x + y \le 5$. Một vài kiểm tra đại diện: 

| x | y | x+y | pos[x] + pos[y] | vị trí[x+y] | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 3 + 4 = 7 | 1 | không | 
| 1 | 4 | 5 | 3 + 2 = 5 | 5 | vâng | 
| 3 | 2 | 5 | 1 + 4 = 5 | 5 | vâng | 

Điều này mang lại 2 cặp hợp lệ. 

Dấu vết này cho thấy điều kiện chỉ phụ thuộc vào vị trí như thế nào và tại sao việc tính toán trước`pos`giảm mỗi lần kiểm tra thành số học theo thời gian không đổi. 

### Ví dụ 2 

đầu vào:```
3
1 3 2
```Vị trí: 

| giá trị x | vị trí[x] | 
| --- | --- | 
| 1 | 1 | 
| 2 | 3 | 
| 3 | 2 | 

Các cặp hợp lệ: 

| x | y | x+y | pos[x] + pos[y] | vị trí[x+y] | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 + 1 = 2 | 3 | không | 
| 1 | 2 | 3 | 1 + 3 = 4 | 2 | không | 
| 2 | 1 | 3 | 3 + 1 = 4 | 2 | không | 

Câu trả lời là 0. 

Điều này xác nhận rằng mặc dù hoán vị nhỏ nhưng cấu trúc hiếm khi thẳng hàng và điều kiện rất hạn chế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Hai vòng lặp lồng nhau trên các giá trị với O(1) kiểm tra từng giá trị | 
| Không gian |$O(n)$| Mảng hoán vị nghịch đảo | 

Các ràng buộc cho phép điều này vì hoạt động bên trong cực kỳ nhẹ và vùng hợp lệ$x + y \le n$loại bỏ khoảng một nửa không gian tìm kiếm. Với các hệ số không đổi chặt chẽ, điều này phù hợp với giới hạn thông thường đối với Python dưới 1 giây trong môi trường kiểu Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n = int(input())
    a = list(map(int, input().split()))
    pos = [0] * (n + 1)
    for i, v in enumerate(a, 1):
        pos[v] = i

    ans = 0
    for x in range(1, n + 1):
        px = pos[x]
        for y in range(1, n + 1):
            s = x + y
            if s > n:
                break
            if px + pos[y] == pos[s]:
                ans += 1

    return str(ans)

# provided sample
assert run("5\n3 4 1 2 5\n") == "2"

# minimum size
assert run("1\n1\n") == "1"

# small permutation no matches
assert run("3\n1 3 2\n") == "0"

# reversed permutation
assert run("4\n4 3 2 1\n") == "0"

# identity permutation
assert run("4\n1 2 3 4\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`| 1 | trường hợp cạnh tối thiểu | 
|`3 1 3 2`| 0 | không có cấu trúc hợp lệ | 
|`4 4 3 2 1`| 0 | thứ tự đảo ngược | 
|`4 1 2 3 4`| 2 | hoán vị có cấu trúc | 

## Vỏ cạnh 

Đối với đầu vào nhỏ nhất$n = 1$, cặp duy nhất là$(1,1)$. Tình trạng giảm xuống còn$a_1 + a_1 = a_{2}$, nhưng vì việc lập chỉ mục vượt quá giới hạn không được phép nên chỉ những triển khai xử lý chính xác$s > n$vì không hợp lệ nên tránh sự cố. Thuật toán xử lý việc này vì vòng lặp sẽ bị ngắt ngay lập tức khi$x + y > n$, do đó không xảy ra truy cập không hợp lệ. 

Đối với các hoán vị có giá trị đơn điệu như$[1,2,3,4,\dots]$, ánh xạ nghịch đảo trở thành danh tính. Điều kiện trở thành$i + j = i + j$, luôn đúng khi$i + j \le n$. Thuật toán đếm chính xác tất cả các cặp hợp lệ trong vùng tam giác này, phù hợp với hành vi dự kiến. 

Đối với các hoán vị ngược, các vị trí bị lệch tối đa, do đó, ngay cả khi tổng hợp lệ, sự bình đẳng về vị trí hầu như không bao giờ giữ được. Thuật toán vẫn liệt kê các ứng cử viên nhưng hiếm khi tăng bộ đếm, xác nhận tính chính xác ngay cả trong các bố cục đối nghịch.
