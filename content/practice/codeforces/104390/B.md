---
title: "CF 104390B - Nhà thám hiểm"
description: "Chúng tôi được tặng một cái cây có phòng $N$. Bên cạnh cấu trúc cây, chúng tôi cũng nhận được một chuỗi dài có độ dài $2N-1$, xuất phát từ quy trình khám phá cũ hơn, hoạt động giống như truyền tải theo chiều sâu nhưng ghi lại các lượt truy cập khác nhau: mỗi khi một phòng được vào, nhãn của nó là…"
date: "2026-07-01T02:44:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104390
codeforces_index: "B"
codeforces_contest_name: "The Unofficial Mirror Contest of 19th Thailand Olympiad in Informatics Day 1"
rating: 0
weight: 104390
solve_time_s: 72
verified: true
draft: false
---

[CF 104390B - Explorer](https://codeforces.com/problemset/problem/104390/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$N$phòng. Bên cạnh cấu trúc cây, chúng ta còn nhận được một chuỗi dài có độ dài$2N-1$, xuất phát từ một quy trình khám phá cũ hơn, hoạt động giống như quá trình truyền tải theo chiều sâu nhưng ghi lại số lượt truy cập theo cách khác: mỗi khi một phòng được vào, nhãn của phòng đó sẽ được thêm vào danh sách ngay cả khi phòng đó đã được nhìn thấy. 

Từ trình tự này, chúng ta biết được hai điều. Đầu tiên, nó mã hóa một số đường duyệt gốc của cây bắt đầu và kết thúc tại cùng một phòng, đây là giá trị đầu tiên trong chuỗi. Thứ hai, nó không phải là thứ tự DFS một cách trực tiếp, bởi vì các lần truy cập lại được ghi lại một cách rõ ràng, do đó, cùng một đỉnh xuất hiện nhiều lần theo cách có cấu trúc. 

Nhiệm vụ của chúng tôi không phải là xây dựng lại cây một cách duy nhất mà là đếm xem có bao nhiêu chuỗi hợp lệ có thể được tạo ra bởi quy trình khám phá hiện đại bắt đầu từ cùng một gốc, trong đó mỗi phòng chỉ được ghi lại một lần trong lần truy cập đầu tiên và quá trình truyền tải hoạt động giống như một DFS tiêu chuẩn với sự lựa chọn tùy ý giữa những người hàng xóm chưa được ghé thăm. 

Vì vậy, đầu vào cung cấp cho chúng ta một “bước đi lịch sử” với các lần lặp lại và chúng ta phải đếm xem có bao nhiêu lệnh khám phá DFS riêng biệt phù hợp với nó. 

Ràng buộc$N \le 5 \cdot 10^5$loại trừ mọi lý luận theo cấp số nhân đối với các hoán vị hoặc quay lui đối với các lựa chọn của trẻ em. Thậm chí$O(N \log N)$hoặc cây tuyến tính DP có thể chấp nhận được, nhưng bất cứ điều gì phân nhánh trên mỗi nút đều không thể thực hiện được. 

Khó khăn chính là trình tự trộn lẫn cấu trúc theo cả thứ tự cây và thứ tự truyền tải. Các mục lặp lại mã hóa việc quay lui một cách ngầm định, nhưng chúng cũng bảo toàn ranh giới cây con một cách tinh vi. 

Một trường hợp lỗi phổ biến xuất hiện khi nhiều nút con đối xứng trong chuỗi truyền tải. Ví dụ: nếu gốc có hai cây con giống hệt nhau, một DFS đơn giản sẽ xem xét cả hai thứ tự, nhưng trình tự có thể hạn chế thứ tự tương đối của chúng. 

Một trường hợp tinh tế khác là khi một cây con xuất hiện theo kiểu xen kẽ do quay lui. Nếu chúng ta hiểu sai ranh giới, chúng ta có thể coi các phân đoạn chồng chéo là các cây con độc lập không chính xác. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp bằng vũ lực là thử tất cả các lệnh DFS có thể phù hợp với cấu trúc cây. Đối với mỗi nút, chúng tôi có thể hoán vị thứ tự của các nút con của nó và mô phỏng DFS, kiểm tra xem “chuỗi truyền tải cũ” được tạo ra có khớp với chuỗi đã cho hay không sau khi nén các lượt truy cập đầu tiên. Điều này ngay lập tức bùng nổ vì mỗi nút độ$d$đóng góp$d!$hoán vị, và trong cây hình ngôi sao, điều này trở thành giai thừa trong$N$. 

Quan sát quan trọng là trình tự này không phải là tùy ý: nó chính xác là một phép duyệt Euler giống như cây có gốc trong đó mỗi cạnh được duyệt hai lần, nhưng nhãn đỉnh được ghi lại trên mỗi mục. Nếu chúng tôi loại bỏ các bản sao và chỉ xem xét các lần xuất hiện đầu tiên, chúng tôi sẽ khôi phục thứ tự DFS hợp lệ. Vấn đề trở thành việc đếm xem có bao nhiêu cây có thứ tự gốc (mặt phẳng nhúng) tạo ra các ràng buộc DFS cơ bản giống nhau được hàm ý theo trình tự. 

Cấu trúc quan trọng là trình tự có thể được phân tách bằng cách sử dụng cách diễn giải giống như ngăn xếp. Mỗi khi chúng ta nhìn thấy một nút mới, nó sẽ được đẩy và khi quay lại, chúng ta sẽ bật lên. Các ranh giới trình tự tương ứng với các khoảng cây con. Điều này có nghĩa là các thứ tự DFS hợp lệ tương ứng chính xác với các cách sắp xếp các nút con sao cho các nút con của mỗi nút tương ứng với các phân đoạn liền kề trong chuỗi. 

Một khi điều này được nhận ra, việc đếm sẽ giảm xuống còn tính toán, đối với mỗi nút, có bao nhiêu cách sắp xếp các nút con của nó trong khi vẫn tôn trọng các kích thước cây con cố định xuất phát từ chuỗi. Mỗi nút đóng góp một hệ số đa thức cho các nút con của nó, nhưng bản thân các nút con đã được cố định về danh tính bằng cách phân tích chuỗi thành cấu trúc cây. 

Vì vậy, vấn đề trở thành: xây dựng lại cấu trúc cây gốc được ngụ ý bởi chuỗi, sau đó tính toán, cho mỗi nút, số cách sắp xếp danh sách kề của nó phù hợp với các khoảng cây con. Câu trả lời cuối cùng là tích trên các nút giai thừa của số lượng con, được điều chỉnh bằng các ràng buộc cấu trúc không thể phân biệt được nếu cần thiết. 

Trong công thức này, chúng ta tránh khám phá các hoán vị một cách rõ ràng mà thay vào đó tính toán các lựa chọn tổ hợp cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị DFS Brute Force |$O(N!)$|$O(N)$| Quá chậm | 
| Xây dựng lại ngăn xếp + đếm cục bộ |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích trình tự và xây dựng lại cấu trúc cây bằng cách sử dụng ngăn xếp. Mỗi lần chúng tôi thấy một nút không bằng đỉnh ngăn xếp, chúng tôi coi nút đó như đang nhập một nút con mới và khi gặp một giá trị bằng nút tổ tiên trước đó, chúng tôi mô phỏng việc quay lui bằng cách bật lên cho đến khi tìm thấy nó. Điều này tạo lại cây có gốc một cách độc đáo. 
2. Trong khi xây dựng lại, hãy duy trì danh sách kề cho mỗi nút. Mỗi cạnh được phát hiện chính xác một lần khi một nút mới được gắn vào nút cha của nó trong ngăn xếp. 
3. Sau khi cây được xây dựng lại, hãy root nó ở phần tử đầu tiên của chuỗi. 
4. Với mỗi nút, hãy tính số nút con mà nó có. Số lượng lệnh DFS hợp lệ tại nút đó là số hoán vị của các nút con của nó, vì DFS có thể chọn bất kỳ nút con nào chưa được thăm viếng tiếp theo. 
5. Nhân tất cả các đóng góp giai thừa này trên tất cả các nút theo modulo$10^9+7$. 
6. Tính toán trước các giai thừa lên tới$N$để hỗ trợ sự kết hợp nhanh chóng của những đóng góp này. 

Một điểm tinh tế là khi cây đã cố định, các lựa chọn ở các nút khác nhau là độc lập. Thứ tự duyệt bên trong mỗi cây con không ảnh hưởng đến các lựa chọn thứ tự anh em ở nơi khác, vì vậy phép nhân là hợp lệ. 

### Tại sao nó hoạt động 

Cây được xây dựng lại đảm bảo rằng trình tự đã cho tương ứng với bước đi DFS hợp lệ trong đó mỗi cây con tạo thành một đoạn liền kề trong ngăn xếp truyền tải. Các nút con của mỗi nút chính xác là các nhánh được phát hiện lần đầu tiên từ nút đó trong quá trình tái thiết. Bất kỳ cuộc khám phá hiện đại hợp lệ nào cũng chỉ khác nhau ở thứ tự mà những đứa trẻ này được đến thăm. Vì các cây con rời rạc và được khám phá đầy đủ trước khi quay trở lại nên các hoán vị tại một nút không ảnh hưởng đến các nút khác, khiến tổng số trở thành tích của các hoán vị cục bộ độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    seq = list(map(int, input().split()))
    
    if n == 1:
        print(1)
        return

    adj = [[] for _ in range(n + 1)]
    parent = [0] * (n + 1)

    stack = [seq[0]]
    visited = set([seq[0]])

    for v in seq[1:]:
        if v not in visited:
            parent[v] = stack[-1]
            adj[stack[-1]].append(v)
            stack.append(v)
            visited.add(v)
        else:
            while stack and stack[-1] != v:
                stack.pop()

    # factorial
    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    ans = 1
    for i in range(1, n + 1):
        ans = ans * fact[len(adj[i])] % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc xây dựng lại sử dụng ngăn xếp để mô phỏng việc nhập các nút mới và quay lại nút tổ tiên khi một nút lặp lại xuất hiện. Nút gốc của mỗi nút mới được phát hiện luôn là đỉnh hiện tại của ngăn xếp, khớp với cấu trúc khám phá DFS được trình tự ngụ ý. 

Khi danh sách kề được xây dựng, mỗi nút sẽ đóng góp một số hạng giai thừa dựa trên số lượng nút con mà nó có. Điều này phản ánh rằng DFS có thể tự do hoán vị thứ tự mà nó khám phá các cây con. 

Mảng giai thừa được tính toán trước một lần để tránh chi phí nhân lặp lại trong quá trình tổng hợp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1 2 1 3 1 4 1
```Chúng tôi xây dựng lại cây: 

| Bước | Hiện tại | Ngăn xếp | Hành động | Quan hệ cha mẹ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | bắt đầu | - | 
| 2 | 2 | [1,2] | đứa con mới của 1 | 1 → 2 | 
| 3 | 1 | [1] | quay lại | - | 
| 4 | 3 | [1,3] | đứa con mới của 1 | 1 → 3 | 
| 5 | 1 | [1] | quay lại | - | 
| 6 | 4 | [1,4] | đứa con mới của 1 | 1 → 4 | 
| 7 | 1 | [1] | kết thúc | - | 

Cây: nút 1 có con {2,3,4}. 

Chúng tôi tính toán các đóng góp giai thừa: 

Nút 1: 3 con →$3! = 6$Nút 2,3,4: 0 con → mỗi nút 1 

Câu trả lời cuối cùng: 6. 

Điều này phù hợp với thực tế là DFS có thể chọn bất kỳ thứ tự nào trong ba cây con. 

### Mẫu 2 

đầu vào:```
5
1 2 4 2 5 2 1 3 1
```Tái thiết: 

| Bước | Hiện tại | Ngăn xếp | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | [1] | bắt đầu | 
| 2 | 2 | [1,2] | con 1 tuổi | 
| 3 | 4 | [1,2,4] | con 2 tuổi | 
| 4 | 2 | [1,2] | quay lại | 
| 5 | 5 | [1,2,5] | con 2 tuổi | 
| 6 | 2 | [1,2] | quay lại | 
| 7 | 1 | [1] | quay lại | 
| 8 | 3 | [1,3] | con 1 tuổi | 
| 9 | 1 | [1] | kết thúc | 

Cấu trúc cây: 

Nút 1 có con {2,3} 

Nút 2 có con {4,5} 

Giai thừa: 

Nút 1: 2 con → 2 

Nút 2: 2 con → 2 

Khác: 1 

Trả lời:$2 \times 2 = 4$. 

Điều này thể hiện tính độc lập của thứ tự cây con tại các nút khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| tái cấu trúc ngăn xếp đơn cộng với tích lũy giai thừa tuyến tính | 
| Không gian |$O(N)$| danh sách kề, ngăn xếp và mảng giai thừa | 

Thuật toán vẫn tuyến tính, phù hợp thoải mái trong$5 \cdot 10^5$giới hạn. Việc sử dụng bộ nhớ cũng tuyến tính do lưu trữ lân cận. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    seq = list(map(int, input().split()))

    if n == 1:
        return "1\n"

    adj = [[] for _ in range(n + 1)]
    stack = [seq[0]]
    visited = set([seq[0]])

    for v in seq[1:]:
        if v not in visited:
            adj[stack[-1]].append(v)
            stack.append(v)
            visited.add(v)
        else:
            while stack and stack[-1] != v:
                stack.pop()

    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    ans = 1
    for i in range(1, n + 1):
        ans = ans * fact[len(adj[i])] % MOD

    return str(ans) + "\n"

# provided samples
assert run("""4
1 2 1 3 1 4 1
""") == "6\n"

assert run("""5
1 2 4 2 5 2 1 3 1
""") == "4\n"

# minimum case
assert run("""1
1
""") == "1\n"

# star shaped
assert run("""3
1 2 1 3 1
""") == "2\n"

# chain
assert run("""3
1 2 3 2 1
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | cây tối thiểu | 
| ngôi sao | 2 | hoán vị tại gốc | 
| chuỗi | 1 | không có lựa chọn phân nhánh | 

## Vỏ cạnh 

Cây nút đơn kiểm tra trường hợp cơ sở trong đó không có lựa chọn truyền tải nào tồn tại và câu trả lời phải chính xác là một. Thuật toán xử lý việc này vì danh sách kề trống và tất cả các giai thừa đều bằng 1. 

Một chuỗi đường thẳng đảm bảo rằng mỗi nút có nhiều nhất một nút con, vì vậy mọi số hạng giai thừa đều bằng 1. Điều này xác minh rằng thuật toán không đưa ra sự tự do tổ hợp một cách không chính xác khi không tồn tại. 

Cây hình ngôi sao kiểm tra xem các nút con của gốc có được tính chính xác như các hoán vị độc lập hay không. Vì tất cả các nút được gắn trực tiếp vào nút gốc, nên việc xây dựng lại ngăn xếp tạo ra chính xác một nút cha và giai thừa của bậc của nó sẽ đưa ra số đếm chính xác.
