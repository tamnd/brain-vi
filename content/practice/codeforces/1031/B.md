---
title: "CF 1031B - Sự tò mò không có giới hạn"
description: "Chúng ta có hai mảng có độ dài $n-1$. Mỗi vị trí $i$ mô tả mối quan hệ giữa hai giá trị liên tiếp không xác định $ti$ và $t{i+1}$, trong đó mỗi $ti$ là một số nguyên trong phạm vi $[0, 3]$, nghĩa là chúng ta có thể coi mọi giá trị là số 2 bit."
date: "2026-06-16T20:44:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1031
codeforces_index: "B"
codeforces_contest_name: "Technocup 2019 - Elimination Round 2"
rating: 1500
weight: 1031
solve_time_s: 1041
verified: false
draft: false
---

[CF 1031B - Sự tò mò không có giới hạn](https://codeforces.com/problemset/problem/1031/B) 

**Đánh giá:** 1500 
**Thẻ:** - 
**Thời gian giải:** 17m 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng có độ dài$n-1$. Mỗi vị trí$i$mô tả mối quan hệ giữa hai giá trị liên tiếp chưa biết$t_i$Và$t_{i+1}$, mỗi nơi$t_i$là một số nguyên trong khoảng$[0, 3]$, nghĩa là chúng ta có thể coi mọi giá trị là số 2 bit. 

Đối với mỗi cặp liền kề, chúng ta được biết OR theo bit của cặp và AND theo bit của cặp. Nhiệm vụ là quyết định xem có tồn tại một mảng hay không$t_1, t_2, \ldots, t_n$đồng thời thỏa mãn tất cả các ràng buộc này và nếu nó tồn tại, hãy xây dựng bất kỳ ràng buộc hợp lệ nào. 

Cấu trúc chính là mỗi ràng buộc chỉ liên quan đến hai biến lân cận, do đó bài toán hoạt động giống như một chuỗi các điều kiện tương thích cục bộ. Mỗi$t_i$bị ràng buộc chặt chẽ bởi các hàng xóm của nó, nhưng chúng ta vẫn có quyền tự do vì nhiều mẫu bit có thể tạo ra cùng một OR và AND. 

Những hạn chế$a_i, b_i \in [0,3]$ngụ ý rằng chúng tôi đang làm việc chỉ với hai bit cho mỗi vị trí. Điều này quan trọng vì nó giới hạn không gian tìm kiếm cục bộ ở bốn khả năng cho mỗi biến, giúp quản lý quá trình chuyển đổi theo thời gian tuyến tính. 

Từ$n$có thể lên đến$10^5$, mọi giải pháp thử tất cả các khả năng cho từng vị trí một cách độc lập hoặc thực hiện quay lui trên toàn bộ nhiệm vụ sẽ không thành công. Một cách xây dựng hàm mũ ngây thơ trên$4^n$trạng thái là hoàn toàn không khả thi, và thậm chí$O(n \cdot 4^2)$chuyển đổi thô bạo không có cấu trúc sẽ ổn nhưng phải được tổ chức cẩn thận để tránh tính toán lại ẩn. 

Một trường hợp phức tạp xuất hiện khi các ràng buộc có giá trị cục bộ nhưng không nhất quán trên toàn cầu. Ví dụ: một cặp ràng buộc có thể buộc các yêu cầu trái ngược nhau trên cùng một phần tử ở giữa. Một nhiệm vụ tham lam chỉ kiểm tra tính nhất quán ngay lập tức mà không đảm bảo tính khả thi cho bước tiếp theo có thể dễ dàng bị mắc kẹt sau này mặc dù các lựa chọn trước đó có vẻ hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ xử lý từng vị trí$t_i$như một sự lựa chọn trong số bốn giá trị và cố gắng gán chúng một cách tuần tự trong khi kiểm tra các ràng buộc. Ở mỗi bước, chúng ta sẽ thử tất cả các giá trị có thể có cho$t_{i+1}$thỏa mãn cả OR và AND với dòng điện$t_i$. Điều này dẫn đến một quá trình phân nhánh trong đó mỗi trạng thái có thể chuyển sang một vài trạng thái hợp lệ tiếp theo. 

Lực lượng mạnh mẽ này là chính xác bởi vì mọi giải pháp hợp lệ phải tương ứng với một đường đi qua các chuyển đổi cục bộ này. Tuy nhiên, số lượng phép gán từng phần tăng theo cấp số nhân trong trường hợp xấu nhất vì vẫn có thể có nhiều lựa chọn ở mỗi bước, dẫn đến gần như$4 \cdot 4^{n-1}$khả năng trong kịch bản phân nhánh tồi tệ nhất. 

Quan sát quan trọng là vì mỗi vị trí chỉ phụ thuộc vào vị trí trước đó nên chúng ta không cần phải khám phá tất cả các đường dẫn. Chúng ta chỉ cần biết giá trị nào của$t_i$khả thi ở vị trí$i$, và sau đó tuyên truyền tính khả thi về phía trước. Điều này làm giảm vấn đề đối với lập trình động trên chuỗi có không gian trạng thái có kích thước 4 cho mỗi vị trí. 

Chúng tôi duy trì những giá trị nào của$t_i$là có thể và với mỗi giá trị có thể, chúng tôi lưu trữ giá trị trước đó để xây dựng lại chuỗi. Vì các quá trình chuyển đổi chỉ phụ thuộc vào các ràng buộc liền kề nên mỗi bước có thể được tính toán theo thời gian không đổi trên mỗi trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(4^n)$|$O(n)$| Quá chậm | 
| DP tối ưu qua các trạng thái |$O(4n)$|$O(4n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích từng$t_i$dưới dạng mặt nạ 2 bit. Đối với bất kỳ cặp liền kề nào$(x, y)$, các ràng buộc trở thành: 

x \,|\, y = a_i,\quad x \,&\, y = b_i 

Hai phương trình này xác định đầy đủ mối quan hệ giữa các bit của$x$Và$y$. 

### bước 

1. Đối với từng vị trí$i$, xét mọi giá trị của$t_i$TRONG$\{0,1,2,3\}$. Chúng đại diện cho tất cả các trạng thái 2 bit có thể có và chúng tôi sẽ truyền bá các chuyển đổi hợp lệ trên toàn chuỗi. 
2. Cho mỗi cặp$(x, y)$, tính toán trước xem nó có thỏa mãn cặp ràng buộc hay không$(a_i, b_i)$. Điều này được thực hiện bằng cách kiểm tra trực tiếp cả hai phương trình bằng cách sử dụng các phép toán bitwise. Bước này có thời gian không đổi cho mỗi cặp vì kích thước miền là cố định. 
3. Xây dựng đồ thị chuyển tiếp giữa các trạng thái tại các vị trí liên tiếp. Đối với mỗi cặp hợp lệ$(x, y)$, đánh dấu rằng$y$có thể theo dõi$x$. Điều này mã hóa tất cả các quy tắc nhất quán cục bộ. 
4. Chạy quy trình lập trình động theo lớp từ trái sang phải. Ở vị trí 1, tất cả các giá trị ban đầu đều được phép. Đối với mỗi vị trí tiếp theo$i$, chúng tôi chỉ giữ giá trị$y$có thể truy cập được từ ít nhất một hợp lệ$x$ở lớp trước. 
5. Lưu trữ một con trỏ cha cho mỗi trạng thái có thể truy cập$y$ở vị trí$i$, ghi lại trạng thái nào$x$dẫn đến nó. Điều này cho phép xây dựng lại trình tự cuối cùng. 
6. Sau khi xử lý tất cả các vị trí, hãy kiểm tra xem có thể truy cập được trạng thái nào tại vị trí không$n$. Nếu không thể truy cập được thì hệ thống các ràng buộc không nhất quán và không tồn tại trình tự nào. 
7. Mặt khác, chọn bất kỳ trạng thái cuối cùng hợp lệ nào và xây dựng lại chuỗi bằng cách đi ngược lại các con trỏ cha. 

### Tại sao nó hoạt động 

DP duy trì bất biến sau khi xử lý vị trí$i$, tập hợp các trạng thái biểu diễn chính xác các giá trị đó của$t_i$có thể được mở rộng thành tiền tố hợp lệ đáp ứng mọi ràng buộc lên đến$i-1$. Bởi vì mọi quá trình chuyển đổi đều mã hóa cả hai ràng buộc OR và AND cho một cạnh duy nhất, nên không có phép gán một phần không hợp lệ nào còn tồn tại. Ngược lại, mọi giải pháp hợp lệ toàn cầu đều phải tồn tại sau mỗi bước lọc vì mỗi tiền tố của nó đều hợp lệ cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok(x, y, a, b):
    return (x | y) == a and (x & y) == b

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

dp = [[False] * 4 for _ in range(n)]
parent = [[-1] * 4 for _ in range(n)]

for v in range(4):
    dp[0][v] = True

for i in range(1, n):
    for cur in range(4):
        for prev in range(4):
            if dp[i-1][prev] and ok(prev, cur, a[i-1], b[i-1]):
                dp[i][cur] = True
                parent[i][cur] = prev
                break

end = -1
for v in range(4):
    if dp[n-1][v]:
        end = v
        break

if end == -1:
    print("NO")
    sys.exit()

res = [0] * n
res[n-1] = end

for i in range(n-1, 0, -1):
    res[i-1] = parent[i][res[i]]

print("YES")
print(*res)
```Kiểm tra chuyển đổi trực tiếp thực thi cả hai ràng buộc theo bit trên mỗi cạnh. Bảng DP ghi lại tính khả thi của từng giá trị tại mỗi vị trí, trong khi bảng cha lưu trữ một giá trị tiền thân hợp lệ duy nhất để cho phép xây dựng lại. Quá trình lùi sẽ xây dựng lại một phép gán nhất quán trong thời gian tuyến tính. 

Một điểm tinh tế là chúng ta phá vỡ sau khi tìm thấy tiền thân hợp lệ đầu tiên. Điều này an toàn vì chúng ta chỉ cần sự tồn tại chứ không cần mọi giải pháp. DP đảm bảo rằng bất kỳ phần trước nào được ghi lại sẽ dẫn đến phần tiếp theo hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [3, 3, 2]
b = [1, 2, 0]
```Chúng tôi theo dõi các trạng thái có thể truy cập. 

| tôi | trạng thái có thể truy cập | 
| --- | --- | 
| 1 | {0,1,2,3} | 
| 2 | {1,2,3} | 
| 3 | {0,2,3} | 
| 4 | {0,1,2,3} (ví dụ kết thúc ở bất kỳ trạng thái hợp lệ nào) | 

Một con đường tái thiết mang lại:$t = [1, 3, 2, 0]$. 

Điều này cho thấy ở mỗi bước vẫn có thể có nhiều trạng thái nhưng những chuyển đổi không khả thi sẽ bị loại bỏ dần dần. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [0, 3]
b = [0, 3]
```Ở ràng buộc đầu tiên, chỉ cho phép các cặp có giá trị giống nhau, buộc$t_1 = t_2$TRONG$\{0,1,2,3\}$. Ràng buộc thứ hai đòi hỏi$t_2 = t_3$nhưng cũng buộc cả OR và AND đều bằng 3, điều này chỉ xảy ra đối với$t_2 = t_3 = 3$. Điều này là nhất quán, nhưng nếu chúng ta sửa đổi một chút:```
a = [1, 2]
b = [0, 3]
```Sau đó, không có cặp nào thỏa mãn đồng thời cả hai ràng buộc và DP trống ở vị trí 2, mang lại NO. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vị trí kiểm tra tối đa 16 lần chuyển trạng thái | 
| Không gian |$O(n)$| DP và con trỏ cha mẹ qua$n \times 4$bàn | 

Không gian trạng thái không đổi đảm bảo tỷ lệ tuyến tính ngay cả đối với$n = 10^5$, trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    def ok(x, y, a, b):
        return (x | y) == a and (x & y) == b

    dp = [[False] * 4 for _ in range(n)]
    parent = [[-1] * 4 for _ in range(n)]

    for v in range(4):
        dp[0][v] = True

    for i in range(1, n):
        for cur in range(4):
            for prev in range(4):
                if dp[i-1][prev] and ok(prev, cur, a[i-1], b[i-1]):
                    dp[i][cur] = True
                    parent[i][cur] = prev
                    break

    end = -1
    for v in range(4):
        if dp[n-1][v]:
            end = v
            break

    if end == -1:
        return "NO"

    res = [0] * n
    res[n-1] = end

    for i in range(n-1, 0, -1):
        res[i-1] = parent[i][res[i]]

    return "YES\n" + " ".join(map(str, res))

# provided sample
assert run("""4
3 3 2
1 2 0
""").startswith("YES")

# minimum size
assert run("""2
3
1
""") in ["YES\n1 3", "YES\n3 1"]

# impossible case
assert run("""3
1 2
0 3
""") == "NO"

# all equal trivial
assert run("""3
0 0
0 0
""").startswith("YES")

# larger simple chain
assert run("""5
3 3 3 3
3 3 3 3
""").startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 3/1 | CÓ hoặc đảo ngược | xây dựng tối thiểu | 
| 3, không nhất quán | KHÔNG | phát hiện mâu thuẫn sớm | 
| tất cả số không | CÓ | chuỗi nhất quán tầm thường | 
| cả ba | CÓ | chuỗi ràng buộc tối đa | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi các ràng buộc cố định hoàn toàn các giá trị liền kề. Ví dụ,$a_i = b_i = 3$lực lượng$t_i = t_{i+1} = 3$. DP xử lý việc này bằng cách chỉ cho phép trạng thái 3 tồn tại ở vị trí đó và tất cả các trạng thái khác sẽ tự nhiên biến mất. 

Một trường hợp khác là khi các ràng buộc cho phép thực hiện nhiều lần chuyển đổi, chẳng hạn như$a_i = 3, b_i = 0$, cho phép bất kỳ cặp giá trị riêng biệt nào bao gồm tất cả các bit trong OR nhưng không trùng lặp trong AND. DP bảo toàn chính xác nhiều trạng thái và không cam kết sớm với một giá trị duy nhất, điều này tránh được ngõ cụt sau này. 

Trường hợp thất bại của phương pháp tham lam xuất hiện khi các lựa chọn ban đầu vẫn cho phép so khớp cục bộ nhưng loại bỏ khả năng tương thích trong tương lai. DP tránh điều này bằng cách không bao giờ cố định một giá trị vĩnh viễn cho đến bước xây dựng lại cuối cùng, bảo toàn tất cả các phần tiếp theo hợp lệ trên toàn cầu.
