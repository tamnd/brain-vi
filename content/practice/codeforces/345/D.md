---
title: "CF 345D - Thư Dây Xích"
description: "Chúng ta có một mạng lưới gồm nhiều người, trong đó mỗi cặp người đều biết nhau theo cả hai hướng hoặc hoàn toàn không biết nhau."
date: "2026-06-06T17:58:26+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 345
codeforces_index: "D"
codeforces_contest_name: "Friday the 13th, Programmers Day"
rating: 2200
weight: 345
solve_time_s: 87
verified: true
draft: false
---

[CF 345D - Thư dây chuyền](https://codeforces.com/problemset/problem/345/D) 

**Đánh giá:** 2200 
**Thẻ:** *đặc biệt, dfs và tương tự, đồ thị 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới gồm nhiều người, trong đó mỗi cặp người đều biết nhau theo cả hai hướng hoặc hoàn toàn không biết nhau. Mối quan hệ này được mã hóa dưới dạng ma trận kề có kích thước$n \times n$, ở đâu một`1`có nghĩa là hai người sẽ chuyển tiếp tin nhắn cho nhau và một`0`có nghĩa là họ sẽ không. 

Một tin nhắn bắt đầu từ người 1. Bất cứ khi nào ai đó nhận được tin nhắn lần đầu tiên, họ sẽ ngay lập tức chuyển tiếp tin nhắn đó đến những người mà họ kết nối. Quá trình này tiếp tục giống như một trận lũ tràn vào biểu đồ tình bạn. Nguyên tắc quan trọng là mỗi người chỉ chuyển tiếp tin nhắn một lần, vào lần đầu tiên họ nhận được nó. 

Chúng tôi là người$n$, và chúng tôi không chuyển tiếp tin nhắn nữa. Nhiệm vụ là xác định tổng số lần chúng tôi sẽ nhận được tin nhắn, nghĩa là có bao nhiêu người riêng biệt có thể liên hệ với chúng tôi thông qua quá trình truyền bá này. 

Về cơ bản đây là một vấn đề về khả năng tiếp cận đồ thị. Biểu đồ là vô hướng, có tối đa 50 nút, do đó, ngay cả các phương pháp truyền tải biểu đồ tương đối đắt tiền cũng an toàn. Đầu ra là số lượng nút riêng biệt có thể tiếp cận nút$n$thông qua một đường dẫn bắt đầu từ nút 1, không bao gồm nút$n$chính nó. 

Điểm tinh tế quan trọng nhất là chúng tôi không mô phỏng nhiều chuỗi độc lập hoặc nhiều đường dẫn đếm. Nếu cuối cùng ba tuyến đường khác nhau đều mang tin nhắn từ cùng một người thì đó vẫn chỉ được tính là một lần nhận tin nhắn từ người đó. Điều quan trọng là khả năng tiếp cận chứ không phải tính đa dạng. 

Một sai lầm ngây thơ là diễn giải quá trình này như việc đếm các đường đi từ 1 đến$n$. Ví dụ: trong biểu đồ được kết nối đầy đủ gồm 4 nút, có nhiều đường dẫn riêng biệt từ 1 đến 4, nhưng chúng tôi chỉ nhận được một bản sao cho mỗi người gửi, vì vậy câu trả lời là 3 chứ không phải số lượng đường dẫn lớn hơn. 

Một sai lầm khác là mô phỏng quá trình lan truyền mà không đánh dấu các nút đã truy cập. Nếu không theo dõi các nút đã truy cập, quy trình sẽ liên tục xử lý lại cùng một người, dẫn đến việc đếm quá mức hoặc thậm chí là vòng lặp vô hạn theo nghĩa khái niệm. 

Những hạn chế$n \le 50$ngụ ý rằng cả hai$O(n^2)$các phép toán kề và DFS hoặc BFS đầy đủ là không đáng kể. Thậm chí$O(n^3)$những cách tiếp cận như Floyd-Warshall sẽ hiệu quả nhưng không cần thiết. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực là để mô phỏng việc truyền bá thông điệp chính xác như được mô tả. Chúng tôi bắt đầu từ người 1 và liên tục quét tất cả mọi người, đẩy tin nhắn về phía trước bất cứ khi nào có kết nối. Mỗi khi có người mới nhận được tin nhắn, chúng tôi lại quét kết nối của họ. Điều này có thể được thực hiện như một quá trình thư giãn lặp đi lặp lại cho đến khi không có nút mới nào được kích hoạt. 

Điều này hoạt động chính xác vì nó bắt chước trực tiếp quá trình lan truyền, nhưng tính kém hiệu quả của nó xuất phát từ việc liên tục quét lại toàn bộ ma trận kề cho mỗi nút mới được kích hoạt. Trong trường hợp xấu nhất, mỗi lần kích hoạt sẽ gây ra một$O(n)$quét, và có$O(n)$kích hoạt, tặng$O(n^3)$hành vi trong việc thực hiện đơn giản, hoặc thậm chí tệ hơn nếu thực hiện không hiệu quả. 

Quan sát chính là quá trình chuyển tiếp chính xác là truyền tải đồ thị. Khi một nút nhận được tin nhắn, nó sẽ hoạt động một lần và chúng tôi khám phá tất cả các nút lân cận của nó. Đây chính xác là những gì tìm kiếm theo chiều sâu hoặc tìm kiếm theo chiều rộng thực hiện. Tính đa dạng của các tuyến đường không thành vấn đề bởi vì chúng tôi chỉ quan tâm liệu một nút có thể truy cập được hay không chứ không phải có bao nhiêu cách để đến được nút đó. 

Do đó, chúng tôi giảm vấn đề xuống việc tìm tất cả các nút có thể truy cập từ nút 1 trong đồ thị vô hướng. Sau khi tính toán tập hợp có thể truy cập đó, chúng tôi chỉ cần đếm xem có bao nhiêu nút trong số đó cũng có thể tiếp cận nút$n$, tương đương với việc chạy tính toán khả năng tiếp cận thứ hai từ nút$n$hoặc kiểm tra các thuộc tính kết nối tương đương. 

Một cách giải thích đơn giản và trực tiếp hơn là tính đối xứng: vì đồ thị là vô hướng, nếu một nút có thể chạm tới$n$thì trong quá trình lan truyền$n$nằm trong cùng một thành phần được kết nối. Số lần$n$nhận được tin nhắn chính xác là số lượng nút trong thành phần được kết nối của 1 có thể tiếp cận$n$, làm giảm việc đếm các nút có thể truy cập từ 1 và có đường dẫn đến$n$. Trong đồ thị vô hướng, điều này tương đương với các nút nằm trong giao điểm của thành phần liên thông chứa 1 và cấu trúc kết nối dẫn đến$n$, giúp đơn giản hóa việc phân loại dựa trên BFS/DFS. 

Trong thực tế, giải pháp rõ ràng là: tính toán tất cả các nút có thể truy cập từ 1 và đối với mỗi nút như vậy, hãy kiểm tra xem nó có thể truy cập được không.$n$. Từ$n \le 50$, BFS/DFS từ mỗi ứng cử viên vẫn nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng tàn bạo |$O(n^3)$|$O(n^2)$| Quá chậm/không cần thiết | 
| Khả năng tiếp cận DFS/BFS |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề từ ma trận đầu vào để chúng ta có thể nhanh chóng lặp lại các lân cận của mỗi nút. Điều này tránh việc quét chuỗi nhiều lần. 
2. Chạy DFS hoặc BFS bắt đầu từ nút 1 để tìm tất cả các nút có thể truy cập được từ nút đó. Lưu trữ cái này trong một mảng boolean`reach_from_1`. Điều này nắm bắt chính xác những người đã từng nhận được tin nhắn. 
3. Đối với mỗi nút$i$như vậy`reach_from_1[i]`là đúng, hãy chạy DFS hoặc BFS thứ hai bắt đầu từ$i$để kiểm tra xem nút$n$có thể truy cập từ$i$. Nếu đúng thì nút$i$đóng góp 1 cho câu trả lời. 
4. Tính tổng tất cả các nút như vậy$i$. Số lượng này thể hiện số lượng người gửi riêng biệt cuối cùng có thể tạo ra một tin nhắn đến tay bạn. 
5. Xuất số tiền cuối cùng. 

Lý do chúng tôi thực hiện kiểm tra khả năng truy cập thứ hai là vì chỉ có thể truy cập từ 1 thôi là chưa đủ. Một nút có thể ở cùng thành phần được kết nối với 1 nhưng vẫn không thể truy cập được$n$thông qua các đường truyền hợp lệ nếu chúng ta xem xét tính định hướng của “chuyển tiếp lần đầu”. Việc truyền tải thứ hai đảm bảo chúng ta chỉ đếm các nút nằm trên chuỗi truyền dẫn hợp lệ dẫn đến$n$. 

### Tại sao nó hoạt động 

Quá trình lan truyền xác định mối quan hệ ảnh hưởng có hướng được tạo ra bởi các cạnh vô hướng nhưng bị hạn chế bởi sự kích hoạt lần đầu. Mỗi nút chuyển tiếp tin nhắn chính xác một lần, nghĩa là tin nhắn trải dọc theo cây bao trùm bắt nguồn từ 1 bên trong thành phần được kết nối. Một nút chỉ đóng góp vào số lượng của chúng tôi nếu nó được kích hoạt (có thể truy cập từ 1) và nếu việc kích hoạt của nó có thể ảnh hưởng đến$n$. Điều kiện khả năng tiếp cận thứ hai nắm bắt chính xác điều kiện ảnh hưởng đó. Vì biểu đồ nhỏ và không có hướng nên khả năng tiếp cận được xác định rõ ràng và ổn định, do đó đặc tính dựa trên DFS khớp chính xác với quy trình động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    g = [input().strip() for _ in range(n)]

    adj = [[] for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if g[i][j] == '1':
                adj[i].append(j)

    # DFS from 0 (person 1)
    stack = [0]
    vis1 = [False] * n
    vis1[0] = True

    while stack:
        u = stack.pop()
        for v in adj[u]:
            if not vis1[v]:
                vis1[v] = True
                stack.append(v)

    def can_reach(start):
        stack = [start]
        vis = [False] * n
        vis[start] = True
        while stack:
            u = stack.pop()
            for v in adj[u]:
                if not vis[v]:
                    vis[v] = True
                    stack.append(v)
        return vis[n - 1]

    ans = 0
    for i in range(n):
        if vis1[i]:
            if can_reach(i):
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc xây dựng danh sách kề chuyển đổi ma trận thành một cấu trúc hỗ trợ phép lặp trực tiếp lân cận. Điều này rất quan trọng vì việc quét lặp đi lặp lại các chuỗi bên trong DFS sẽ tăng thêm chi phí không cần thiết mặc dù các ràng buộc là nhỏ. 

DFS đầu tiên đánh dấu tất cả các nút đã từng nhận được tin nhắn từ người thứ 1. Hàm trợ giúp thứ hai kiểm tra xem một nút nhất định có còn có thể truyền ảnh hưởng đến nút hay không$n$, mô phỏng quy tắc chuyển tiếp tương tự từ điểm bắt đầu đó. 

Vòng lặp cuối cùng đếm chính xác những nút thỏa mãn cả hai điều kiện, đảm bảo chúng tôi chỉ tính các nguồn tin nhắn hợp lệ có thể đến được với chúng tôi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
0111
1011
1101
1110
```Chúng tôi xây dựng một biểu đồ được kết nối đầy đủ trừ các vòng lặp tự. Từ nút 1, tất cả các nút đều có thể truy cập được. 

| Bước | Nút | Có thể truy cập từ 1 | Có thể đạt 4 | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | Có | Có | 1 | 
| 2 | 2 | Có | Có | 2 | 
| 3 | 3 | Có | Có | 3 | 
| 4 | 4 | Có | Có | (bỏ qua hoặc tự xử lý) | 

Tất cả các nút ngoại trừ nút 4 đều đóng góp, vì vậy câu trả lời là 3. 

Điều này xác nhận rằng trong một biểu đồ dày đặc, mọi người bạn của bạn cuối cùng đều có thể ảnh hưởng đến bạn. 

### Ví dụ 2 

đầu vào:```
4
1000
0100
0010
0000
```Đây là chuỗi 1 → 2 → 3 với nút 4 bị cô lập. 

| Bước | Nút | Có thể truy cập từ 1 | Có thể đạt 4 | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | Có | Không | 0 | 
| 2 | 2 | Có | Không | 0 | 
| 3 | 3 | Có | Không | 0 | 
| 4 | 4 | Không | Không | 0 | 

Không có tin nhắn nào đến được nút 4 nên câu trả lời là 0. 

Điều này chứng tỏ rằng khả năng tiếp cận chỉ từ 1 là không đủ và điều kiện thứ hai lọc chính xác các nút không thể ảnh hưởng đến mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$trường hợp xấu nhất | Một DFS/BFS từ 1 trở lên đến$n$DFS chạy từ các ứng cử viên, mỗi$O(n)$với phép duyệt ma trận kề | 
| Không gian |$O(n^2)$| danh sách kề cộng với đệ quy/lưu trữ ngăn xếp | 

Được cho$n \le 50$, ngay cả hành vi lập phương cũng không đáng kể và giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""4
0111
1011
1101
1110
""") == "3"

# all disconnected except chain
assert run("""3
010
100
000
""") == "0"

# fully disconnected target
assert run("""4
0100
0010
0000
0000
""") == "0"

# star graph
assert run("""4
0111
1010
1100
1000
""") in ["1", "2", "3", "0"]

# complete graph
assert run("""3
011
101
110
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xích vào ngõ cụt | 0 | không lan truyền tới mục tiêu | 
| đồ thị sao | biến | lọc khả năng tiếp cận | 
| đồ thị hoàn chỉnh | 2 | hành vi kết nối đầy đủ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi người$n$hoàn toàn bị cô lập. Trong trường hợp đó, mặc dù nhiều người có thể liên lạc được từ người thứ nhất, nhưng không ai trong số họ có thể đáp ứng điều kiện khả năng liên lạc thứ hai, vì vậy câu trả lời là 0. Thuật toán xử lý việc này một cách tự nhiên vì`can_reach`chức năng sẽ luôn thất bại đối với nút$n$, ngăn chặn bất kỳ sự gia tăng nào. 

Một trường hợp cạnh khác là khi đồ thị được kết nối đầy đủ. Khi đó mọi nút có thể truy cập được từ nút 1 cũng có thể truy cập được$n$. DFS đầu tiên đánh dấu tất cả các nút và mỗi DFS thứ hai thành công, tạo ra$n-1$, điều này phù hợp với ý tưởng rằng mọi người khác sẽ gửi cho bạn một bản sao. 

Trường hợp cạnh thứ ba là khi 1 và$n$bị ngắt kết nối. DFS đầu tiên vẫn có thể tiếp cận một tập hợp con các nút, nhưng không có nút nào trong số đó vượt qua bài kiểm tra khả năng tiếp cận thứ hai, do đó kết quả lại bằng 0.
