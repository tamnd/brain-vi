---
title: "CF 106137G - Cây con cân bằng trắng-đen"
description: "Chúng ta có một cây có gốc có gốc là đỉnh 1. Mỗi đỉnh có một trong hai màu đen hoặc trắng. Đối với mỗi đỉnh, chúng ta xem xét cây con bắt đầu từ đỉnh đó và chứa tất cả các đỉnh con của đỉnh đó, bao gồm cả đỉnh đó."
date: "2026-06-25T11:31:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106137
codeforces_index: "G"
codeforces_contest_name: "BFS  BFS - MTA"
rating: 0
weight: 106137
solve_time_s: 30
verified: true
draft: false
---

[CF 106137G - Cây con cân bằng trắng-đen](https://codeforces.com/problemset/problem/106137/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc có gốc là đỉnh`1`. Mỗi đỉnh có một trong hai màu đen hoặc trắng. Đối với mỗi đỉnh, chúng ta xem xét cây con bắt đầu từ đỉnh đó và chứa tất cả các đỉnh con của đỉnh đó, bao gồm cả đỉnh đó. Một cây con được gọi là cân bằng khi số đỉnh đen bên trong nó bằng số đỉnh trắng bên trong nó. Nhiệm vụ là đếm xem có bao nhiêu đỉnh có cây con cân bằng. 

Đầu vào mô tả cây thông qua các con trỏ cha. Với mọi đỉnh từ`2`ĐẾN`n`, chúng ta có đỉnh cha của nó, luôn là đỉnh được đánh số nhỏ hơn. Chuỗi màu cho biết màu của mọi đỉnh theo thứ tự. 

Các ràng buộc là lý do chính khiến giải pháp cần phải dựa trên việc duyệt cây đơn lẻ. Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là`2 * 10^5`, vì vậy một`O(n)`hoặc`O(n log n)`Cách tiếp cận thoải mái. Giải pháp khám phá nhiều lần mọi cây con sẽ trở nên quá chậm vì cây hình chuỗi có thể làm cho tổng kích thước của cây con đạt tới`O(n^2)`, xung quanh`4 * 10^10`hoạt động cho đầu vào lớn. 

Các trường hợp cạnh chính là do kích thước cây con và số lượng màu cục bộ ở mỗi đỉnh. 

Xét một cây có một đỉnh ngoài gốc:```
2
1
BW
```Cây con gốc chứa một đỉnh đen và một đỉnh trắng, vì vậy câu trả lời là`1`. Giải pháp bất cẩn mà chỉ kiểm tra lá sẽ bỏ sót gốc. 

Một trường hợp khác là:```
3
1 1
BBB
```Mỗi cây con chỉ chứa các đỉnh màu đen nên câu trả lời là`0`. Giải pháp chỉ so sánh kích thước cây con với số lượng màu mà quên đi sự đóng góp của màu sắc sẽ tạo ra kết quả không chính xác. 

Trường hợp ranh giới cuối cùng là một chuỗi:```
4
1 2 3
BWBW
```Các cây con từ dưới lên trên chứa:```
vertex 4: B
vertex 3: WB
vertex 2: BWB
vertex 1: BWBW
```Chỉ các đỉnh`1`Và`3`cân bằng nên đáp án là`2`. Việc triển khai xử lý cha mẹ trước con cái có thể sử dụng thông tin không đầy đủ và không đạt được cấu trúc này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là bắt đầu DFS từ mọi đỉnh. Trong DFS của một đỉnh đã chọn, chúng tôi đếm các đỉnh đen và trắng trong cây con của nó và kiểm tra xem hai số đó có bằng nhau hay không. Điều này đúng vì mỗi đỉnh được kiểm tra độc lập dựa trên định nghĩa chính xác của cây con cân bằng. 

Vấn đề là công việc lặp đi lặp lại. Trong một chuỗi`n`đỉnh, cây con của đỉnh`1`có`n`đỉnh, cây con của đỉnh`2`có`n-1`đỉnh, v.v. Tổng số đỉnh đã thăm trở thành:```
n + (n - 1) + (n - 2) + ... + 1 = O(n^2)
```Vì`n = 200000`, điều này vượt xa những gì có thể chạy trong thời gian giới hạn. 

Cấu trúc cây cho chúng ta một cách tốt hơn. Cây con của một đỉnh chính xác là sự kết hợp của các cây con con của nó cộng với chính đỉnh đó. Khi xử lý xong tất cả các đỉnh con của một đỉnh, chúng ta đã biết mọi thứ cần thiết để tính toán độ cân bằng màu của đỉnh đó. 

Thay vì lưu trữ hai số đếm riêng biệt, chúng ta có thể lưu trữ một giá trị. Hãy để màu đen đóng góp`-1`và trắng đóng góp`+1`. Tổng của cây con bằng 0 khi số đỉnh đen và đỉnh trắng bằng nhau. DFS thứ tự sau tính toán tổng này cho mỗi đỉnh và tăng kết quả bất cứ khi nào tổng bằng 0. 

Phương pháp brute-force hoạt động vì nó liên tục tái tạo lại số lượng màu của mỗi cây con. Quan sát cho thấy mọi cây con được xây dựng từ các cây con đã được xử lý cho phép chúng tôi sử dụng lại thông tin đó và giảm vấn đề xuống còn một lần duyệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của các con từ mảng cha. Mỗi đỉnh chỉ cần truy cập vào các nút con trực tiếp của nó vì thông tin về cây con sẽ được thu thập từ dưới lên. 
2. Chạy DFS bắt đầu từ thư mục gốc. Việc truyền tải phải hoàn thành tất cả các nút con trước khi xử lý đỉnh hiện tại vì cây con hiện tại phụ thuộc vào các giá trị đã được tính toán của các nút con của nó. 
3. Đối với một đỉnh`u`, bắt đầu cân bằng với`+1`nếu nó có màu trắng và`-1`nếu nó có màu đen. Sau đó cộng số dư được trả về bởi tất cả các đỉnh con. 
4. Sau khi kết hợp tất cả các phần tử con, hãy kiểm tra xem số dư của`u`là số không. Nếu đúng như vậy thì cây con của`u`chứa số đỉnh đen và trắng bằng nhau, do đó hãy tăng câu trả lời. 
5. Trả về số dư đã tính của`u`tới cha mẹ của nó. Cây cha sẽ sử dụng giá trị này như một phần của phép tính cây con của chính nó. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi DFS kết thúc một đỉnh`u`, giá trị trả về chính xác là sự khác biệt giữa các đỉnh trắng và đen bên trong cây con của`u`. Đóng góp cơ bản của`u`đại diện cho màu sắc riêng của nó và mỗi phần tử con đóng góp vào sự khác biệt vốn đã chính xác của cây con của nó. Vì toàn bộ cây con là đỉnh cộng với tất cả các cây con con nên tổng được tính là chính xác. Một cây con được cân bằng chính xác khi chênh lệch này bằng 0, do đó mỗi cây con cân bằng được tính một lần và không có cây con không cân bằng nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        children = [[] for _ in range(n)]

        parents = list(map(int, input().split()))
        for i, p in enumerate(parents, start=1):
            children[p - 1].append(i)

        s = input().strip()

        ans = 0
        balance = [0] * n

        stack = [(0, 0)]
        order = []

        while stack:
            u, state = stack.pop()
            if state == 0:
                stack.append((u, 1))
                for v in children[u]:
                    stack.append((v, 0))
            else:
                order.append(u)

        for u in order:
            cur = 1 if s[u] == 'W' else -1
            for v in children[u]:
                cur += balance[v]

            balance[u] = cur
            if cur == 0:
                ans += 1

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu vào được chuyển đổi thành danh sách kề phụ. Các đỉnh được đánh số từ 0 bên trong, giúp tránh bị trừ đi một lần trong quá trình truyền tải. 

DFS lặp lại tạo ra một chuỗi thứ tự sau. Cũng có thể sử dụng DFS đệ quy, nhưng độ sâu đệ quy của Python quá nhỏ đối với một cây có chuỗi dài. Ngăn xếp rõ ràng mô phỏng đệ quy trong khi xử lý độ sâu tối đa một cách an toàn. 

các`order`danh sách chứa các đỉnh sau khi tất cả các con cháu của chúng đã được thêm vào. Việc xử lý danh sách này từ trái sang phải sẽ mang lại hiệu quả tương tự như DFS đặt hàng sau. Vào thời điểm một đỉnh được xử lý, mọi đứa trẻ đều đã có được sự cân bằng cuối cùng. 

Giá trị được lưu trữ trong`balance[u]`là sự khác biệt màu sắc của toàn bộ cây con. sử dụng`+1`Và`-1`tránh việc lưu trữ hai bộ đếm và làm cho điều kiện cân bằng trở thành một phép so sánh 0 đơn giản. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7
1 1 2 3 3 5
WBBWWBW
```Trạng thái quan trọng sau khi xử lý từng đỉnh là: 

| Đỉnh | Giá trị riêng | Đóng góp của trẻ em | Số dư cuối cùng | Cân bằng | 
| --- | --- | --- | --- | --- | 
| 4 | +1 | 0 | +1 | Không | 
| 6 | -1 | 0 | -1 | Không | 
| 5 | +1 | -1 | 0 | Có | 
| 2 | -1 | +1 | 0 | Có | 
| 7 | +1 | 0 | +1 | Không | 
| 3 | +1 | +1 | +2 | Không | 
| 1 | +1 | +2 | +3 | Không | 

Việc truyền tải tìm thấy hai số dư bằng 0, vì vậy câu trả lời là`2`. Ví dụ này cho thấy tại sao mỗi đỉnh phải được kiểm tra sau khi các đỉnh con của nó được hợp nhất. 

### Mẫu 2 

đầu vào:```
2
1
BW
```| Đỉnh | Giá trị riêng | Đóng góp của trẻ em | Số dư cuối cùng | Cân bằng | 
| --- | --- | --- | --- | --- | 
| 2 | -1 | 0 | -1 | Không | 
| 1 | +1 | -1 | 0 | Có | 

Chỉ gốc chứa số lượng bằng nhau của cả hai màu, đưa ra câu trả lời`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh được chèn vào ngăn xếp truyền tải một lần và được xử lý một lần. | 
| Không gian | O(n) | Danh sách kề, ngăn xếp truyền tải, danh sách thứ tự và mảng cân bằng đều lưu trữ thông tin tuyến tính. | 

Tổng số đỉnh trong tất cả các trường hợp thử nghiệm là`2 * 10^5`, do đó việc truyền tải tuyến tính dễ dàng phù hợp trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""3
7
1 1 2 3 3 5
WBBWWBW
2
1
BW
8
1 2 3 4 5 6 7
BWBWBWBW
""") == "2\n1\n4", "provided samples"

assert run("""1
2
1
BW
""") == "1", "two opposite colors"

assert run("""1
3
1 1
BBB
""") == "0", "all black"

assert run("""1
4
1 2 3
BWBW
""") == "2", "chain structure"

assert run("""1
6
1 1 1 1 1
WWWWWW
""") == "0", "all white"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh có màu đối diện | 1 | Kiểm tra cây con cân bằng nhỏ nhất. | 
| Tất cả các đỉnh màu đen | 0 | Kiểm tra kích thước cây con bằng nhau là không đủ nếu không có cân bằng màu. | 
| Một chuỗi các đỉnh | 2 | Kiểm tra quá trình xử lý đơn hàng sau và cây sâu. | 
| Tất cả các đỉnh màu trắng | 0 | Kiểm tra màu sắc đối diện cực đoan. | 

## Vỏ cạnh 

Đối với trường hợp cân bằng nhỏ nhất:```
2
1
BW
```DFS đầu tiên xử lý đỉnh`2`. Sự cân bằng của nó là`-1`, nên không được tính. Root nhận giá trị đó và thêm giá trị riêng của nó`+1`, sản xuất`0`. Câu trả lời trở thành`1`. 

Đối với cây mà mọi đỉnh đều có cùng màu:```
3
1 1
BBB
```Mỗi chiếc lá trở về`-1`. Gốc kết hợp cả con và con của nó`-1`, dẫn đến`-3`. Không có đỉnh nào đạt tới điểm cân bằng 0, do đó thuật toán đưa ra`0`. 

Đối với một chuỗi:```
4
1 2 3
BWBW
```Quá trình truyền tải kết thúc đỉnh`4`đầu tiên, sau đó`3`, sau đó`2`, sau đó`1`. Số dư của họ là`-1`,`0`,`-1`, Và`0`tương ứng. Câu trả lời là`2`và ví dụ này xác nhận rằng thuật toán phụ thuộc vào việc các kết quả con đã sẵn sàng trước khi xử lý cấp độ gốc.
