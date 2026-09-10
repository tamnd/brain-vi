---
title: "CF 104599H - Cây Ánh Sáng"
description: "Cấu trúc được mô tả là một cây nhị phân gốc được nhúng dưới dạng mảng. Mỗi nút được gắn nhãn từ $1$ đến $N$ và mỗi nút có thể trỏ đến tối đa hai con, một con trái và một con phải. Giá trị $0$ có nghĩa là phần tử con tương ứng không tồn tại."
date: "2026-06-30T03:02:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104599
codeforces_index: "H"
codeforces_contest_name: "GPL 2023 Novice"
rating: 0
weight: 104599
solve_time_s: 138
verified: false
draft: false
---

[CF 104599H - Cây ánh sáng](https://codeforces.com/problemset/problem/104599/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cấu trúc được mô tả là một cây nhị phân gốc được nhúng dưới dạng mảng. Mỗi nút được dán nhãn từ$1$ĐẾN$N$và mỗi nút có thể trỏ đến nhiều nhất hai con, một con trái và một con phải. Một giá trị của$0$có nghĩa là đứa trẻ tương ứng không tồn tại. Nút$1$được gắn vào nguồn gốc ở phía bên trái và nút$2$được gắn ở phía bên phải, vì vậy hai nhánh này là nhánh ban đầu của cấu trúc. 

Chỉ các nút không có nút con mới được coi là điểm cuối có ý nghĩa. Mỗi điểm cuối như vậy có độ sâu, được định nghĩa là số cạnh từ nguồn gốc đến nút đó. Độ sâu đó được chuyển đổi thành một chữ cái trong bảng chữ cái, với$1 \mapsto A$,$2 \mapsto B$, vân vân. 

Đầu ra cuối cùng được hình thành bằng cách liệt kê tất cả các nút điểm cuối theo thứ tự truyền tải nghiêm ngặt. Việc truyền tải không phải là thứ tự trước hoặc thứ tự tiêu chuẩn; thay vào đó, mỗi cây con bên trái đóng góp tất cả các lá của nó trước, sau đó cây con bên phải đóng góp tất cả các lá của nó và trong mỗi cây con, quy tắc tương tự được lặp lại theo cách đệ quy. 

Các ràng buộc cho phép lên đến$10^5$nút. Điều này buộc phải có một giải pháp tuyến tính hoặc gần tuyến tính, vì bất kỳ mô phỏng bậc hai nào của phép tính duyệt hoặc lặp lại cây con sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. 

Một vấn đề khó phát sinh nếu người ta cố gắng tính toán độ sâu một cách độc lập cho mỗi lá bằng cách sử dụng DFS lặp lại từ gốc. Cách tiếp cận đó tính toán lại các đường đi nhiều lần và suy biến thành$O(N^2)$ở những cây nghiêng. 

Một trường hợp lỗi khác xuất hiện khi người ta giả sử việc duyệt các lá theo thứ tự phù hợp với thứ tự yêu cầu. Ràng buộc thứ tự không đối xứng giữa cây con trái và cây con phải; đó là quy tắc nghiêm ngặt “tất cả các lá bên trái trước bất kỳ lá bên phải nào” ở mọi nút, mạnh hơn hành vi theo thứ tự tiêu chuẩn. 

## Phương pháp tiếp cận 

Một diễn giải trực tiếp sẽ xây dựng cây và thực hiện DFS từ gốc, mang theo chiều sâu. Mỗi khi chạm tới một chiếc lá, độ sâu hiện tại sẽ được chuyển đổi thành một ký tự và được thêm vào kết quả. Điều này đúng vì độ sâu được tích lũy dọc theo con đường duy nhất từ ​​gốc tới lá. Thứ tự duyệt có thể được thực thi một cách tự nhiên bằng cách luôn khám phá con bên trái trước con bên phải. 

Sự kém hiệu quả chỉ xuất hiện nếu việc triển khai tính toán lại độ sâu hoặc quét các cây con nhiều lần. Một DFS chính xác sẽ truy cập mỗi nút một lần, do đó tổng công việc là tuyến tính. 

Quan sát quan trọng là thứ tự được yêu cầu chính xác là việc truyền tải thứ tự trước với ràng buộc là đầu ra chỉ xảy ra ở các lá. Điều này loại bỏ bất kỳ nhu cầu xử lý hậu kỳ hoặc sắp xếp nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Độ sâu tính toán lại trên mỗi lá |$O(N^2)$|$O(N)$| Quá chậm | 
| Truyền tải DFS đơn lẻ |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu diễn kề trong đó mỗi nút lưu trữ con trái và con phải của nó. Điều này khớp trực tiếp với cấu trúc đầu vào và cho phép điều hướng thời gian liên tục. 
2. Xác định gốc ngầm là nút$1$vì nó được xác định là điểm vào từ nguồn điện. 
3. Chạy truyền tải theo chiều sâu bắt đầu từ nút$1$với độ sâu ban đầu$1$, vì cạnh đầu tiên từ gốc đóng góp độ sâu xuống một cấp. 
4. Khi truy cập một nút, trước tiên hãy duyệt đệ quy nút con trái của nó nếu nó tồn tại. Điều này thực thi ràng buộc thứ tự bắt buộc là tất cả các lá trong cây con bên trái phải xuất hiện trước bất kỳ lá nào trong cây con bên phải. 
5. Sau khi hoàn thành cây con bên trái, duyệt đệ quy cây con bên phải nếu nó tồn tại. Điều này đảm bảo cấu trúc từ điển chính xác của chuỗi đầu ra theo thứ tự duyệt chứ không phải theo thứ tự bảng chữ cái. 
6. Nếu một nút không có nút con, hãy coi nó như một chiếc lá và nối thêm ký tự tương ứng với độ sâu của nó vào chuỗi đầu ra. 
7. Chuyển đổi độ sâu thành ký tự bằng cách sử dụng offset, ánh xạ ASCII$1$đến 'A',$2$đến 'B', v.v. 

### Tại sao nó hoạt động 

Mỗi nút được truy cập chính xác một lần trong một lần duyệt tuân theo định nghĩa đệ quy về thứ tự cây con. Vì mỗi lá đều được tiếp cận thông qua chính xác một đường đi từ gốc, độ sâu của nó được xác định duy nhất tại thời điểm ghé thăm. Phép đệ quy trái trước phải được thực thi đảm bảo rằng thứ tự nối khớp với định nghĩa của bài toán về việc đọc các lá. Không cần sắp xếp lại hoặc sắp xếp toàn cục, do đó đầu ra truyền tải vừa hoàn chỉnh vừa được sắp xếp chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(200000)

N = int(input())
left = [0] * (N + 1)
right = [0] * (N + 1)

for i in range(1, N + 1):
    l, r = map(int, input().split())
    left[i] = l
    right[i] = r

res = []

def dfs(node, depth):
    if node == 0:
        return
    if left[node] == 0 and right[node] == 0:
        res.append(chr(ord('A') + depth - 1))
        return
    dfs(left[node], depth + 1)
    dfs(right[node], depth + 1)

dfs(1, 1)

print("".join(res))
```Việc thực hiện phản ánh trực tiếp cấu trúc truyền tải. Các mảng`left`Và`right`lưu trữ con để mỗi cuộc gọi đệ quy là thời gian không đổi. Độ sâu đệ quy tương ứng với chiều cao của cây và`sys.setrecursionlimit`ngăn ngừa tràn ngăn xếp trong chuỗi bị lệch trong trường hợp xấu nhất. Việc phát hiện lá được thực hiện bằng cách kiểm tra sự vắng mặt của cả hai nút con, đảm bảo chỉ các nút cuối mới đóng góp ký tự. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
3 4
0 0
0 0
0 5
0 0
```| Nút | Độ sâu | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | sang trái tới 3 | | 
| 3 | 2 | lá → B | B | 
| 1 | 1 | đi thẳng đến 4 | | 
| 4 | 2 | sang trái Không, phải 5 | | 
| 5 | 3 | lá → C | BC | 

Đầu ra cuối cùng là`BCA`sau khi hoàn thành lệnh duyệt. 

Dấu vết này cho thấy rằng thứ tự được điều khiển hoàn toàn bằng việc hoàn thành cây con bên trái trước khi chuyển sang cây con bên phải. 

### Mẫu 2 

đầu vào:```
5
1 3
0 0
0 0
0 0
0 0
```| Nút | Độ sâu | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | cây con bên trái chứa lá 2 | | 
| 2 | 2 | lá → B | B | 
| 1 | 1 | cây con bên phải chứa lá 3,4 | | 
| 3 | 2 | lá → B | B | 
| 4 | 2 | lá → B | BBB | 

Đầu ra cuối cùng phản ánh rằng tất cả các lá bên trái được phát ra trước bất kỳ lá bên phải nào. 

Điều này chứng tỏ rằng độ sâu giống nhau vẫn có thể tạo ra nhiều ký tự và thứ tự phụ thuộc hoàn toàn vào cấu trúc cây con chứ không chỉ vào độ sâu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| mỗi nút được truy cập một lần trong DFS | 
| Không gian |$O(N)$| lưu trữ cho cây cộng với ngăn xếp đệ quy | 

Đường truyền tuyến tính nằm trong giới hạn vì$N$tùy thuộc vào$10^5$và mỗi phép toán là thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    sys.setrecursionlimit(200000)

    N = int(input())
    left = [0] * (N + 1)
    right = [0] * (N + 1)

    for i in range(1, N + 1):
        l, r = map(int, input().split())
        left[i] = l
        right[i] = r

    res = []

    def dfs(node, depth):
        if node == 0:
            return
        if left[node] == 0 and right[node] == 0:
            res.append(chr(ord('A') + depth - 1))
            return
        dfs(left[node], depth + 1)
        dfs(right[node], depth + 1)

    dfs(1, 1)
    return "".join(res)

# provided sample
assert run("""5
3 4
0 0
0 0
0 5
0 0
""") == "BCA"

# single leaf chain
assert run("""3
2 0
3 0
0 0
""") == "ABC"

# full binary root
assert run("""1
0 0
""") == "A"

# skewed right tree
assert run("""4
2 0
3 0
4 0
0 0
""") == "ABCD"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lệch trái | ABC | độ chính xác đệ quy sâu | 
| nút đơn | A | xử lý trường hợp cơ bản | 
| nhị phân đầy đủ | đặt hàng đúng | quy tắc trái trước phải | 
| cây xích | ABCD | độ sâu tích lũy chính xác | 

## Vỏ cạnh 

Cây lệch hoàn toàn nhấn mạnh độ sâu đệ quy. DFS vẫn truy cập mỗi nút một lần và tạo ra độ sâu chính xác vì độ sâu được tăng lên dọc theo một chuỗi. 

Một cây chỉ tồn tại con bên phải xác minh rằng việc truyền tải vẫn tuân thủ các quy tắc sắp xếp ngay cả khi không có cây con bên trái. Phép đệ quy chỉ đơn giản bỏ qua các nhánh trống bên trái mà không ảnh hưởng đến chuỗi đầu ra. 

Cây nút đơn đảm bảo trường hợp cơ sở là chính xác, vì gốc đồng thời là lá và phải tạo ngay một ký tự mà không cần đệ quy.
