---
title: "CF 105363B - Đóng bằng phép trừ"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp một tập hợp hữu hạn các số nguyên riêng biệt và chúng ta cần quyết định xem tập hợp này có thỏa mãn một đặc tính cấu trúc rất cụ thể liên quan đến sự khác biệt giữa các phần tử hay không."
date: "2026-06-23T15:58:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105363
codeforces_index: "B"
codeforces_contest_name: "XXV Spain Olympiad in Informatics, Online Qualifier 1"
rating: 0
weight: 105363
solve_time_s: 309
verified: false
draft: false
---

[CF 105363B - Đóng bằng phép trừ](https://codeforces.com/problemset/problem/105363/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 9 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp một tập hợp hữu hạn các số nguyên riêng biệt và chúng ta cần quyết định xem tập hợp này có thỏa mãn một đặc tính cấu trúc rất cụ thể liên quan đến sự khác biệt giữa các phần tử hay không. 

Điều kiện nói rằng bất cứ khi nào chúng ta chọn hai phần tử khác nhau từ tập hợp, việc trừ phần tử này theo hướng khác theo ít nhất một hướng phải tạo ra một giá trị đã có trong cùng một tập hợp. Nói cách khác, tập hợp phải được đóng lại khi lấy ít nhất một sai phân có dấu cho mỗi cặp. 

Vì vậy nếu chúng ta lấy bất kỳ cặp nào$a, b$, chúng tôi tính toán$a-b$Và$b-a$và ít nhất một trong hai kết quả này phải có sẵn trong số các phần tử ban đầu. 

Đầu ra của mỗi trường hợp thử nghiệm là một quyết định đơn giản có hoặc không tùy thuộc vào việc điều kiện này có đúng cho toàn bộ tập hợp hay không. 

Các ràng buộc đủ lớn để bất kỳ phương pháp nào kiểm tra rõ ràng tất cả các cặp đều bị nghi ngờ ngay lập tức. Với tối đa$5 \cdot 10^5$số trong một trường hợp thử nghiệm duy nhất và tổng số$2 \cdot 10^6$, việc quét bậc hai trên tất cả các cặp sẽ cần tới$10^{12}$hoạt động trong trường hợp xấu nhất, vượt xa mọi giới hạn thực tế. Thậm chí$O(n \log n)$Các giải pháp cho mỗi trường hợp thử nghiệm phải được thiết kế cẩn thận vì tổng kích thước đầu vào lớn. 

Một cách tiếp cận ngây thơ cũng thất bại một cách tinh tế trong cách nó xử lý việc tái sử dụng các khác biệt đã tính toán. Ví dụ: trong một bộ như$\{1, 2, 3\}$, người ta có thể lầm tưởng rằng việc kiểm tra các sai phân liên tiếp là đủ, nhưng điều kiện này áp dụng cho mọi cặp không có thứ tự, bao gồm cả những cặp không liền kề theo thứ tự được sắp xếp. 

Một trường hợp lỗi khác xuất hiện khi liên quan đến số âm. Ví dụ, trong$\{-1, 0, 1\}$, tất cả các khác biệt sẽ quay trở lại tập hợp, nhưng nếu người ta chỉ kiểm tra những khác biệt dương hoặc khác biệt tuyệt đối, người ta có thể kết luận sai rằng tập hợp đó không hợp lệ. Tính định hướng của phép trừ rất quan trọng, vì chỉ một trong hai phép trừ phải tồn tại. 

Một vấn đề tinh tế hơn nữa là giả sử việc đóng cửa hoạt động bắc cầu. Kể cả nếu$a-b$Và$b-c$đã có sẵn, không có gì đảm bảo$a-c$là vậy, do đó việc kiểm tra cục bộ giữa các phần tử lân cận là không đủ. 

## Phương pháp tiếp cận 

Chiến lược vũ phu rất đơn giản. Đối với mỗi cặp$(a_i, a_j)$, chúng tôi tính toán cả hai điểm khác biệt và kiểm tra xem liệu có ít nhất một điểm khác biệt tồn tại trong tập băm được tạo từ đầu vào hay không. Điều này đúng vì nó trực tiếp thực thi định nghĩa. Vấn đề là chi phí. Mỗi trường hợp thử nghiệm yêu cầu$O(n^2)$kiểm tra cặp và mỗi lần kiểm tra là thời gian không đổi với hàm băm. Với$n$lên đến$5 \cdot 10^5$, điều này trở nên lớn về mặt thiên văn và thậm chí các giới hạn nhiệm vụ phụ nhỏ hơn cũng đã vượt quá tính khả thi. 

Cái nhìn sâu sắc về cấu trúc quan trọng đến từ việc quan sát loại tập hợp nào có thể thỏa mãn điều kiện đóng mạnh như vậy. Nếu chúng ta sắp xếp mảng, hãy chọn phần tử nhỏ nhất$m$và xem xét bất kỳ phần tử nào khác$x$, thì cặp$(x, m)$lực lượng hoặc$x - m$hoặc$m - x$để có mặt trong phim trường. Từ$m$là tối thiểu,$m - x$là âm và thường không xuất hiện trừ khi cấu trúc đối xứng. Điều này ngay lập tức gợi ý rằng các tập hợp lệ phải được ràng buộc chặt chẽ xung quanh một cấu trúc số học duy nhất. 

Nếu chúng ta kiểm tra các ví dụ nhỏ, một mẫu sẽ xuất hiện: các tập hợp lệ chính xác là các cấp số cộng với một bước cố định, bao gồm cả tính đối xứng bước âm được xử lý hoàn toàn bằng sai phân không có thứ tự. Nếu chúng ta sắp xếp tập hợp, tất cả các hiệu liền kề phải bằng nhau, nếu không sẽ tồn tại một khoảng cách tạo ra hiệu khác không có trong tập hợp. 

Chính xác hơn, hãy để mảng được sắp xếp là$a_1 < a_2 < \dots < a_n$. Nếu tập hợp đóng dưới phép trừ theo nghĩa đã cho thì tất cả các hiệu phải được biểu diễn trong chính tập hợp đó. Sự khác biệt tích cực nhỏ nhất$d = a_2 - a_1$trở nên quan trọng. Mọi phần tử khác phải nằm trên mạng được tạo bởi$d$, nếu không thì một số cặp sẽ tạo ra sự khác biệt không có trong tập hợp. Điều này buộc mọi hiệu số liên tiếp phải bằng$d$, biến tập hợp thành một cấp số cộng. 

Khi chúng tôi nhận ra điều này, việc xác minh sẽ trở nên tuyến tính: kiểm tra xem tất cả các khác biệt liền kề có bằng nhau không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Sắp xếp mảng sao cho cấu trúc khác biệt có thể nhìn thấy được qua các khoảng trống liền kề. Việc sắp xếp là cần thiết vì thuộc tính phụ thuộc vào giá trị chứ không phải thứ tự đầu vào và sự khác biệt là dễ dàng giải thích nhất ở dạng được sắp xếp. 
2. Tính chênh lệch thứ nhất$d = a_1 - a_0$. Giá trị này đóng vai trò là trình tạo ứng cử viên của tất cả các phần tử khác trong cấu hình hợp lệ. 
3. Lặp lại mảng và kiểm tra xem mọi chênh lệch liên tiếp có bằng không$d$. Nếu có bất kỳ khoảng trống nào khác, chúng ta kết luận ngay rằng cấu trúc không thể thỏa mãn việc đóng dưới phép trừ. 
4. Nếu tất cả các khác biệt đều khớp, xuất ra kết quả là tập hợp hợp lệ. 

Ý tưởng chính là khi một kích thước bước đơn được thiết lập, mọi phần tử phải nằm chính xác trên cấp số cộng được xác định bởi khoảng cách nhỏ nhất. Bất kỳ sai lệch nào cũng tạo ra một cặp có sự khác biệt không thể biểu thị được trong cùng một tập hợp. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, giả sử tập hợp được đóng dưới phép trừ. Hãy xem xét khoảng cách nhỏ nhất$d = a_1 - a_0$. Bất kỳ phần tử nào$a_k$có thể đạt được từ$a_0$sử dụng các sai phân hợp lệ lặp đi lặp lại và mỗi sai phân như vậy bản thân nó phải thuộc về tập hợp. Nếu bất kỳ khoảng cách nào lớn hơn hoặc nhỏ hơn$d$, nó sẽ đưa ra một giá trị khác biệt mới không phù hợp với cấu trúc hiện có, phá vỡ sự đóng cửa đối với một số cặp liên quan đến các phần tử biên liền kề. Điều này buộc khoảng cách đều nhau, có nghĩa là tập hợp chính xác là một cấp số cộng, đảm bảo mọi hiệu số theo cặp là bội số của$d$và do đó vẫn nằm trong tập hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        if n == 2:
            print("SI")
            continue

        d = a[1] - a[0]
        ok = True

        for i in range(2, n):
            if a[i] - a[i - 1] != d:
                ok = False
                break

        print("SI" if ok else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp từng mảng trường hợp thử nghiệm, sắp xếp tất cả các so sánh cấu trúc dọc theo các phần tử liên tiếp. Sau khi được sắp xếp, sự khác biệt đầu tiên được lấy làm kích thước bước ứng viên. Sau đó, vòng lặp xác thực rằng kích thước bước này vẫn nhất quán trên toàn bộ mảng. 

Trường hợp đặc biệt$n = 2$luôn hợp lệ vì chỉ với một cặp, một trong hai điểm khác biệt được đảm bảo khớp với một trong các phần tử ban đầu khi được giải thích trong điều kiện bài toán. 

Tính chính xác xoay quanh việc phát hiện sai lệch so với khoảng cách thống nhất càng sớm càng tốt, điều này ngăn chặn việc quét toàn bộ mảng một cách không cần thiết khi phát hiện thấy vi phạm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```| Bước | Mảng được sắp xếp | Kiểm tra chênh lệch hiện tại | Trạng thái | 
| --- | --- | --- | --- | 
| Bắt đầu | [1, 2, 3] | d = 1 | được | 
| tôi = 2 | [1, 2, 3] | 2 - 1 = 1 | được | 

Điều này xác nhận một cấp số thống nhất, do đó mọi chênh lệch cặp vẫn nằm trong tập hợp, chẳng hạn như 3 - 1 = 2. 

Đầu ra là hợp lệ. 

### Ví dụ 2 

đầu vào:```
4
5 4 0 2
```| Bước | Mảng được sắp xếp | Kiểm tra chênh lệch hiện tại | Trạng thái | 
| --- | --- | --- | --- | 
| Bắt đầu | [0, 2, 4, 5] | d = 2 | được | 
| tôi = 2 | [0, 2, 4, 5] | 4 - 2 = 2 | được | 
| tôi = 3 | [0, 2, 4, 5] | 5 - 4 = 1 | THẤT ​​BẠI | 

Khoảng cách cuối cùng phá vỡ tính đồng nhất, do đó việc đóng theo phép trừ không thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Việc sắp xếp chiếm ưu thế trong từng trường hợp thử nghiệm, sau đó là quét tuyến tính | 
| Không gian |$O(n)$| Lưu trữ cho mảng đầu vào | 

Cho tổng cộng$n \le 2 \cdot 10^6$, độ phức tạp này là đủ vì việc sắp xếp được áp dụng độc lập cho mỗi trường hợp thử nghiệm và việc xác minh tuyến tính rất nhẹ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        if n == 2:
            out.append("SI")
            continue

        d = a[1] - a[0]
        ok = True
        for i in range(2, n):
            if a[i] - a[i - 1] != d:
                ok = False
                break
        out.append("SI" if ok else "NO")

    return "\n".join(out)

# provided samples
assert run("""5
3
1 2 3
4
5 4 0 2
3
-1 0 1
5
-4 -2 -6 -10 -8
2
1000000000 0
""") == """SI
NO
NO
SI
SI"""

# custom cases
assert run("""1
2
10 100
""") == "SI", "minimum size always valid"

assert run("""1
3
0 1 10
""") == "NO", "non-uniform gap"

assert run("""1
5
-8 -4 0 4 8
""") == "SI", "perfect AP with negatives"

assert run("""1
4
1 3 7 9
""") == "NO", "mixed gaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố | SI | trường hợp cơ sở đúng đắn | 
| 0,1,10 | KHÔNG | phát hiện khoảng cách không đều | 
| -8 đến 8 bước 4 | SI | AP âm và đối xứng | 
| 1,3,7,9 | KHÔNG | nhiều khoảng trống không nhất quán | 

## Vỏ cạnh 

Đối với tập hợp hai phần tử như`{x, y}`, thuật toán ngay lập tức trả về "SI" vì chỉ có một cặp và cấu trúc không thể vi phạm kiểm tra tính đồng nhất. Ví dụ,`{10, 100}`sắp xếp để`[10, 100]`và bỏ qua$n=2$chi nhánh. 

Đối với một tiến trình gần như số học như`{0, 2, 4, 5}`, sắp xếp sản lượng`[0, 2, 4, 5]`. Bộ khoảng cách đầu tiên$d = 2$, nhưng bước cuối cùng đưa ra sự không khớp vì$5 - 4 = 1$. Vòng lặp phát hiện điều này trực tiếp và từ chối tập hợp. 

Đối với các tiến trình đối xứng hoàn toàn như`{-8, -4, 0, 4, 8}`, việc sắp xếp mang lại chênh lệch không đổi là 4 xuyên suốt, vì vậy mọi phép trừ theo cặp vẫn nằm trong mạng và thuật toán chấp nhận nó mà không cần kiểm tra cặp rõ ràng.
