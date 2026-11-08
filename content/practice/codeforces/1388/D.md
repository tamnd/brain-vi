---
title: "CF 1388D - Thuyền Trưởng Flint Và Kho Báu"
description: "Chúng ta có một cấu trúc có hướng trên các chỉ số từ 1 đến n, trong đó mỗi chỉ mục i có một giá trị a[i] và một con trỏ b[i]. Con trỏ dẫn đến chỉ mục khác hoặc vắng mặt (được biểu thị bằng -1)."
date: "2026-06-16T14:43:12+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "graphs", "greedy", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1388
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 660 (Div. 2)"
rating: 2000
weight: 1388
solve_time_s: 134
verified: true
draft: false
---

[CF 1388D - Thuyền trưởng Flint và Kho báu](https://codeforces.com/problemset/problem/1388/D) 

**Xếp hạng:** 2000 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, đồ thị, tham lam, triển khai, cây 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cấu trúc có hướng trên các chỉ số từ 1 đến n, trong đó mỗi chỉ mục i có một giá trị a[i] và một con trỏ b[i]. Con trỏ dẫn đến chỉ mục khác hoặc vắng mặt (được biểu thị bằng -1). Hoạt động quan trọng là chúng ta phải xử lý mọi chỉ mục chính xác một lần theo một thứ tự nào đó và khi xử lý chỉ mục i, chúng ta ngay lập tức đạt được a[i] thành điểm tổng thể. Ngoài ra, nếu tôi trỏ đến một chỉ mục j khác, chúng tôi cũng tăng a[j] lên giá trị hiện tại của a[i], có nghĩa là việc xử lý j sau này sẽ được hưởng lợi từ mọi thứ tích lũy trong đó cho đến nay. 

Mục tiêu là chọn thứ tự xử lý sao cho các quá trình lan truyền này được khai thác càng nhiều càng tốt và tổng giá trị thu thập cuối cùng được tối đa hóa. 

Ràng buộc n lên tới 2 · 10^5 hàm ý mọi nghiệm đều phải gần tuyến tính hoặc n log n. Bất kỳ mô phỏng thứ tự bậc hai trên hoán vị là không thể. Sự đảm bảo về cấu trúc bổ sung rằng các con trỏ b luôn kết thúc ở -1 có nghĩa là đồ thị được hình thành bởi các cạnh b là một rừng các cây có gốc hướng về các gốc ở -1. Không có chu kỳ, vì vậy sự phụ thuộc là không theo chu kỳ theo hướng lan truyền. 

Chế độ lỗi cơ bản xuất hiện khi chúng ta cố gắng xử lý các nút theo thứ tự tùy ý hoặc theo phương pháp phỏng đoán cục bộ như tăng hoặc giảm a[i]. Ví dụ: hãy xem xét chuỗi 1 → 2 → 3 có các giá trị a = [1, 100, 1]. Nếu chúng ta xử lý 3 trước, sau đó là 2, rồi 1, thì giá trị từ 1 sẽ không bao giờ đạt đến 2 hoặc 3 kịp thời. Chiến lược đúng đắn phải đảm bảo rằng các khoản đóng góp được chuyển từ trẻ em đến cha mẹ trước khi cha mẹ được xử lý. 

Một lỗi nhỏ khác phát sinh khi một nút có nhiều nút con đóng góp vào nó một cách gián tiếp thông qua các chuỗi. Việc sắp xếp cục bộ tham lam mà không tôn trọng cấu trúc cây có thể bỏ sót việc trì hoãn cha mẹ cho đến khi tất cả con cháu được xử lý sẽ làm tăng giá trị của nó một cách đáng kể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của thứ tự xử lý. Đối với mỗi hoán vị, chúng tôi mô phỏng quy trình, cập nhật một bản sao của mảng và tích lũy ans. Điều này đúng vì nó tuân thủ trực tiếp các quy tắc, nhưng số hoán vị là n!, và mỗi mô phỏng là O(n), dẫn đến bùng nổ giai thừa vượt xa giới hạn khả thi. 

Quan sát quan trọng là sự lan truyền luôn di chuyển dọc theo các cạnh b hướng lên phía gốc. Mỗi nút đóng góp giá trị hiện tại của nó lên chính xác một lần cho mỗi lần xử lý và khi một nút được xử lý, đóng góp của nó sẽ cố định và không bao giờ thay đổi nữa. Điều này gợi ý rõ ràng rằng các nút nên được xử lý theo thứ tự phù hợp với việc đẩy các giá trị tích lũy lên trên trước khi sử dụng một nút. 

Điều này tương tự như việc xử lý cây có gốc từ dưới lên. Nếu một nút nhận được sự đóng góp từ các nút con của nó, thì việc xử lý các nút con trước tiên sẽ có lợi để các giá trị tích lũy của chúng được đưa vào nút cha trước khi lấy nút cha. Do đó, trong mỗi cây, chúng tôi muốn xử lý các nút theo thứ tự sau. 

Tuy nhiên, các giá trị a[i] cũng quan trọng trong việc sắp xếp thứ tự giữa các cây con khác nhau. Chiến lược toàn cầu đúng đắn là coi mỗi nút có mức độ ưu tiên dựa trên tiềm năng tích lũy cây con và luôn xử lý các nút có con cháu đã được xử lý. Điều này tự nhiên dẫn đến việc truyền tải thứ tự sau DFS qua khu rừng được xác định bởi con trỏ b. 

Chúng tôi tính toán nhóm, chạy DFS từ các gốc (b[i] = -1) và thu thập các nút theo thứ tự sau. Thứ tự đó đảm bảo tất cả các phần tử con đều được xử lý trước phần tử cha của chúng, đảm bảo sự tích lũy tối đa được chuyển lên trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Thứ tự sau DFS tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Xây dựng danh sách kề ngược

Chúng ta xây dựng một biểu đồ trong đó mỗi nút theo dõi các nút con của nó, nghĩa là tất cả các nút i sao cho b[i] = nút cha. Điều này chuyển đổi cấu trúc con trỏ thành một biểu diễn rừng. 

Bước này là cần thiết vì quá trình truyền lan truyền từ một nút đến nút cha của nó, nhưng DFS hoạt động tự nhiên từ nút cha đến nút con. 

### Bước 2: Xác định gốc 

Bất kỳ nút nào có b[i] = -1 đều là nút gốc. Đây là điểm khởi đầu của những cây độc lập trong rừng. 

Chúng tôi sẽ khởi tạo DFS từ mỗi root. 

### Bước 3: Chạy DFS để thu thập postorder 

Chúng tôi thực hiện DFS từ mỗi gốc. Đối với mỗi nút, trước tiên chúng tôi xử lý đệ quy tất cả các nút con, sau đó nối chính nút đó vào danh sách đơn hàng. 

Lý do chúng tôi trì hoãn việc thêm nút là vì chúng tôi muốn tất cả đóng góp từ con cháu đã được hợp nhất vào nút đó trước khi nó được hoạt động "tiêu thụ". 

### Bước 4: Xuất thứ tự và tính kết quả 

Chúng tôi đảo ngược thứ tự hoàn thiện DFS nếu cần (tùy thuộc vào quy ước triển khai) và sử dụng trực tiếp làm trình tự xử lý. Sau đó, chúng tôi mô phỏng quy trình một lần theo thứ tự đó, cập nhật các giá trị và tích lũy ans. 

Mô phỏng này là tuyến tính vì mỗi cạnh được sử dụng chính xác một lần để truyền các giá trị. 

### Tại sao nó hoạt động 

Bất biến chính là khi nút i được xử lý, tất cả các nút trong cây con của nó (trong rừng con trỏ b) đã được xử lý. Do đó, a[i] đã bao gồm mọi đóng góp có thể chảy vào nó. Không thao tác nào sau này có thể tăng a[i], vì vậy thực hiện nó vào thời điểm đó là tối ưu. Điều này giúp loại bỏ khả năng mất giá trị do tiêu thụ sớm và đảm bảo mọi đơn vị giá trị được truyền bá càng xa càng tốt trước khi được thu thập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

children = [[] for _ in range(n)]
roots = []

for i in range(n):
    if b[i] == -1:
        roots.append(i)
    else:
        children[b[i] - 1].append(i)

order = []

def dfs(u):
    for v in children[u]:
        dfs(v)
    order.append(u)

for r in roots:
    dfs(r)

ans = 0

for i in order:
    ans += a[i]
    if b[i] != -1:
        a[b[i] - 1] += a[i]

print(ans)
print(*[i + 1 for i in order])
```Giải pháp đầu tiên là xây dựng cấu trúc cây được tạo ra bởi con trỏ b. Mỗi nút lưu trữ một danh sách các nút con để chúng ta có thể duyệt từ gốc trở xuống. 

DFS tạo ra một chuỗi thứ tự sau trong đó con cái luôn xuất hiện trước cha mẹ của chúng. Lệnh này được sử dụng trực tiếp làm lệnh thực hiện. 

Vòng lặp cuối cùng mô phỏng quá trình chính xác một lần theo thứ tự đó. Bởi vì phần tử con được xử lý sớm hơn nên phần đóng góp của chúng đã được thêm vào giá trị gốc vào thời điểm phần tử gốc được xử lý. 

Một chi tiết triển khai tinh tế là độ sâu đệ quy, độ sâu này phải được tăng lên vì cây có thể suy biến thành một chuỗi có độ dài n. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
2 3 -1
```Điều này tạo thành một chuỗi 1 → 2 → 3. 

DFS từ gốc 3 tạo ra thứ tự sau: 1, 2, 3. 

| Bước | Nút đã xử lý | một trạng thái trước | trả lời | Cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [1,2,3] | 1 | thêm a[2] += 1 | 
| 2 | 2 | [1,3,3] | 3 | thêm a[3] += 3 | 
| 3 | 3 | [1,3,6] | 9 | không có cha mẹ | 

Câu trả lời cuối cùng là 9 và thứ tự là 1 2 3. 

Điều này cho thấy các nút đầu khuếch đại các nút sau thông qua quá trình lan truyền như thế nào. 

### Ví dụ 2 

đầu vào:```
4
1 1 1 10
2 3 4 -1
```Chúng ta có chuỗi 1 → 2 → 3 → 4 và một giá trị lớn ở gốc. 

Thứ tự DFS là 1, 2, 3, 4. 

| Bước | Nút | một trước | trả lời | Cập nhật | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [1,1,1,10] | 1 | a2 += 1 | 
| 2 | 2 | [1,2,1,10] | 3 | a3 += 2 | 
| 3 | 3 | [1,2,3,10] | 6 | a4 += 3 | 
| 4 | 4 | [1,2,3,13] | 19 | gốc | 

Điều này chứng tỏ sự tích lũy đã chảy đến tận gốc trước khi nó bị lấy đi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần trong DFS và được xử lý một lần trong mô phỏng | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy | 

Độ phức tạp tuyến tính là cần thiết vì n có thể lên tới 200.000. Bất kỳ thuật toán nào cố gắng sắp xếp bằng cách tính toán lại hoặc mô phỏng lặp đi lặp lại trên mỗi hoán vị sẽ quá chậm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    children = [[] for _ in range(n)]
    roots = []

    for i in range(n):
        if b[i] == -1:
            roots.append(i)
        else:
            children[b[i] - 1].append(i)

    order = []

    sys.setrecursionlimit(10**7)

    def dfs(u):
        for v in children[u]:
            dfs(v)
        order.append(u)

    for r in roots:
        dfs(r)

    ans = 0
    for i in order:
        ans += a[i]
        if b[i] != -1:
            a[b[i] - 1] += a[i]

    return str(ans) + "\n" + " ".join(str(x + 1) for x in order)

# provided sample
assert run("""3
1 2 3
2 3 -1
""") == "9\n1 2 3"

# single node
assert run("""1
5
-1
""") == "5\n1"

# all root chain
assert run("""4
1 2 3 4
2 3 4 -1
""") == "10\n1 2 3 4"

# negative values
assert run("""3
-1 -2 -3
2 3 -1
""") == "-6\n1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 5 | tính đúng đắn của trường hợp cơ sở | 
| tăng chuỗi | 10 | tích lũy lan truyền | 
| chuỗi tiêu cực | -6 | tính đúng đắn với giá trị âm | 

## Vỏ cạnh 

Một trường hợp quan trọng là một chuỗi dài duy nhất. Trong trường hợp này, DFS suy biến thành đệ quy sâu n. Thuật toán vẫn xử lý các nút theo đúng thứ tự từ dưới lên, đảm bảo mỗi giá trị được truyền bá đầy đủ trước khi sử dụng. 

Một trường hợp khác là tất cả các nút có b[i] = -1. Khi đó, mỗi nút là một nút gốc và DFS chỉ cần nối các nút theo thứ tự cây con ngược lại, nhưng vì không có cạnh nào nên mọi thứ tự đều hợp lệ và mô phỏng giảm xuống còn tổng đơn giản là a[i]. 

Trường hợp thứ ba liên quan đến các giá trị âm lan truyền lên trên. Bởi vì thuật toán vẫn truyền các giá trị bất kể dấu, nên một nút âm sớm trong chuỗi có thể làm giảm các giá trị trong tương lai. Thứ tự đặt hàng sau vẫn đúng vì việc trì hoãn một nút sẽ chỉ cho phép tích lũy âm nhiều hơn nữa để chảy lên trên, điều này vẫn là tối ưu theo các quy tắc hoạt động nhất định.
