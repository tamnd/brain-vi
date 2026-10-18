---
title: "CF 105579C - Mê cung Thermotaur"
description: "Mê cung có thể được xem dưới dạng lưới $N lần N$ trong đó mỗi ô có nhiệt độ nguyên duy nhất. Từ bất kỳ ô nào, minotaur được phép di chuyển đến một trong bốn ô liền kề nếu nó chọn."
date: "2026-06-22T06:14:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105579
codeforces_index: "C"
codeforces_contest_name: "Udmurtia High School Programming Contest (Qualification for VKOSHP 2012)"
rating: 0
weight: 105579
solve_time_s: 49
verified: true
draft: false
---

[CF 105579C - Mê cung Thermotaur](https://codeforces.com/problemset/problem/105579/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mê cung có thể được xem như một$N \times N$lưới trong đó mỗi ô có nhiệt độ nguyên duy nhất. Từ bất kỳ ô nào, minotaur được phép di chuyển đến một trong bốn ô liền kề nếu nó chọn. Quy tắc chuyển động của nó mang tính quyết định: nó luôn nhìn vào bốn ô lân cận và di chuyển đến ô có nhiệt độ nhỏ nhất, nhưng chỉ khi hàng xóm đó lạnh hơn ô hiện tại. Nếu không có ô liền kề nào lạnh hơn, nó sẽ dừng vĩnh viễn ở vị trí hiện tại. 

Mỗi truy vấn đưa ra một ô bắt đầu và nhiệm vụ là xác định nhiệt độ của ô nơi cuối cùng con minotaur dừng lại sau khi áp dụng quy tắc này nhiều lần. 

Ý nghĩa chính của các ràng buộc là$N \le 500$, như vậy lưới có tới 250.000 ô, trong khi số lượng truy vấn có thể lên tới 20.000. Một mô phỏng đơn giản cho mỗi truy vấn có thể đi qua nhiều ô nhiều lần và trong trường hợp xấu nhất, một đường dẫn có thể đi qua gần như toàn bộ lưới. Điều đó làm cho mọi cách tiếp cận truyền tải theo truy vấn đều có khả năng quá chậm nếu được lặp lại một cách độc lập. 

Một đặc tính tinh tế nhưng quan trọng là tất cả nhiệt độ đều khác nhau. Điều này loại bỏ sự mơ hồ trong việc lựa chọn bước đi tiếp theo và đảm bảo rằng mọi bước đi đều làm giảm nhiệt độ một cách nghiêm ngặt. 

Một lỗi đơn giản xuất hiện khi mô phỏng từng truy vấn một cách độc lập mà không ghi nhớ. Ví dụ: trong một lưới có hình xoắn ốc giảm dần, một truy vấn có thể đi qua$O(N^2)$tế bào. Việc lặp lại điều đó cho 20.000 truy vấn sẽ dẫn đến một kết quả không khả thi$O(N^2 Q)$trường hợp xấu nhất. 

Một trường hợp thất bại khác phát sinh nếu người ta cho rằng cực tiểu cục bộ là hiếm hoặc thử các lối tắt tham lam mà không có sự lan truyền đầy đủ. Hãy xem xét một ô không cực tiểu cục bộ mà dẫn vào một chuỗi dài giảm dần. Quy tắc tham lam “dừng sớm nếu được bao quanh bởi các giá trị cao hơn trong bán kính 1” sẽ chấm dứt không chính xác. 

Mô hình đúng là mọi ô đều di chuyển một cách xác định dọc theo một đường giảm dần cho đến khi đạt mức tối thiểu cục bộ theo nghĩa đồ thị lân cận của nó. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý từng truy vấn một cách độc lập. Bắt đầu từ ô nhất định, chúng tôi liên tục kiểm tra bốn ô lân cận của nó, chọn ô có nhiệt độ nhỏ nhất nếu nó thấp hơn ô hiện tại và di chuyển đến đó. Vì mỗi bước di chuyển sẽ làm giảm nhiệt độ một cách nghiêm ngặt và tất cả các giá trị đều khác nhau nên độ dài đường đi được giới hạn bởi$N^2$trong trường hợp xấu nhất. 

Cách tiếp cận này đúng vì nó mô phỏng trực tiếp các quy tắc. Tuy nhiên, nó tính toán lại các đường đi xuống giống nhau nhiều lần. Nhiều điểm bắt đầu cuối cùng sẽ chảy vào cùng một ô chứa, nghĩa là một phần lớn của lưới sẽ được truyền đi nhiều lần. 

Quan sát quan trọng là mỗi ô đều có chính xác một “bước tiếp theo” xác định nếu nó không phải là mức tối thiểu cục bộ. Điều này tạo ra một biểu đồ chức năng trên các ô trong đó mỗi nút trỏ đến nút lân cận tốt nhất của nó. Bởi vì mọi cạnh đều đi từ nhiệt độ cao hơn đến nhiệt độ thấp hơn nên các chu kỳ là không thể và mọi thành phần được kết nối tạo thành một cây có gốc ở mức tối thiểu cục bộ. Do đó, mỗi truy vấn sẽ hỏi: “nút này thuộc về gốc nào?” 

Điều này biến vấn đề thành việc tìm các thành phần được kết nối trong một nhóm được định hướng được xác định bởi các con trỏ gốc cục bộ. Khi chúng tôi tính toán đích cuối cùng (gốc) cho mỗi ô, mỗi truy vấn sẽ trở thành$O(1)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu cho mỗi truy vấn |$O(N^2 Q)$trường hợp xấu nhất |$O(1)$thêm | Quá chậm | 
| Tính toán trước phần chìm (DFS / DP / lan truyền kiểu Union) |$O(N^2)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán trước vị trí cuối cùng của mỗi ô bằng cách coi chuyển động là một cạnh được định hướng tới một ô lân cận lạnh hơn. 

1. Đối với mỗi ô, hãy xác định ô tiếp theo bằng cách quét bốn vị trí liền kề và chọn vị trí có nhiệt độ nhỏ nhất trong số các vị trí nhỏ hơn ô hiện tại. Nếu không có hàng xóm nào như vậy tồn tại, hãy đánh dấu nó là bồn chứa. Bước này xây dựng “con trỏ tiếp theo” xác định cho mỗi ô. 
2. Tạo cấu trúc ghi nhớ trong đó mỗi ô lưu trữ phần chìm cuối cùng của nó (mức tối thiểu cục bộ mà cuối cùng nó đạt đến). Ban đầu, tất cả các mục không được tính toán. 
3. Đối với mỗi ô trong lưới, hãy chạy độ phân giải kiểu DFS: nếu phần chìm cuối cùng của nó đã được biết, hãy trả lại ngay lập tức. Ngược lại, hãy đi theo con trỏ tiếp theo một cách đệ quy cho đến khi đạt đến điểm chìm, sau đó truyền kết quả đó trở lại dọc theo đường đệ quy. Điều này nén từng chuỗi gốc thành các con trỏ trực tiếp tới phần chìm một cách hiệu quả. 
4. Sau khi tiền xử lý, hãy trả lời từng truy vấn bằng cách trả về giá trị đích được lưu trữ cho ô được truy vấn. 

Lý do chúng tôi sử dụng tính năng ghi nhớ một cách rõ ràng thay vì truyền tải đơn giản là vì có nhiều đường dẫn chồng chéo lên nhau. Khi đã biết đích đến của một ô, mọi đường dẫn đi vào ô đó sẽ kế thừa cùng một kết quả. 

### Tại sao nó hoạt động 

Mỗi ô có một đường đi giảm dần vì mỗi bước di chuyển đều làm giảm nhiệt độ và tất cả các giá trị đều khác nhau. Điều này đảm bảo rằng đệ quy kết thúc ở mức tối thiểu cục bộ duy nhất. Việc ghi nhớ đảm bảo rằng phần chìm của mỗi ô được tính toán chính xác một lần. Vì “con trỏ tiếp theo” có tính xác định nên cấu trúc tạo thành một rừng gồm các cây được định hướng có gốc ở cực tiểu cục bộ và thuật toán tính toán gốc của mỗi nút trong rừng đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

N = int(input())
grid = [list(map(int, input().split())) for _ in range(N)]

# directions
dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

# next pointer: where each cell would move
nxt = [[None] * N for _ in range(N)]
for i in range(N):
    for j in range(N):
        best = (grid[i][j], i, j)
        for di, dj in dirs:
            ni, nj = i + di, j + dj
            if 0 <= ni < N and 0 <= nj < N:
                if grid[ni][nj] < best[0]:
                    best = (grid[ni][nj], ni, nj)
        if best[1] == i and best[2] == j:
            nxt[i][j] = (i, j)
        else:
            nxt[i][j] = (best[1], best[2])

# memoized sink
sink = [[None] * N for _ in range(N)]

def find(i, j):
    if sink[i][j] is not None:
        return sink[i][j]
    ni, nj = nxt[i][j]
    if (ni, nj) == (i, j):
        sink[i][j] = (i, j)
        return (i, j)
    sink[i][j] = find(ni, nj)
    return sink[i][j]

for i in range(N):
    for j in range(N):
        find(i, j)

Q = int(input())
out = []
for _ in range(Q):
    x, y = map(int, input().split())
    x -= 1
    y -= 1
    i, j = sink[x][y]
    out.append(str(grid[i][j]))

print("\n".join(out))
```Lưới được đọc trực tiếp vào bộ nhớ và đối với mỗi ô, chúng tôi tính toán cạnh ra tốt nhất của nó bằng cách quét bốn ô lân cận. Điều này thiết lập đồ thị gốc xác định. 

các`find`hàm thực hiện đệ quy được ghi nhớ. Một ô sẽ giải quyết ngay lập tức nếu đã được tính toán hoặc giải quyết đệ quy ô tiếp theo của nó và lưu trữ kết quả. Điều này đảm bảo rằng mỗi ô được truy cập một lần trong quá trình tiền xử lý. 

Các truy vấn chỉ cần lập chỉ mục vào bảng chìm được tính toán trước và trả về nhiệt độ được lưu trữ. Phép trừ một trong phân tích cú pháp đầu vào sẽ căn chỉnh việc lập chỉ mục dựa trên 1 của vấn đề với các mảng dựa trên 0 của Python. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

Hãy xem xét một lưới 2 × 2 đơn giản:$$\begin{matrix}
4 & 3 \\
2 & 1
\end{matrix}$$Truy vấn: (1,1), (1,2), (2,1), (2,2) 

| Bắt đầu | Bước tiếp theo | Đường dẫn | Chìm | 
| --- | --- | --- | --- | 
| (1,1)=4 | (1,2)=3 | 4→3→1 | (2,2) | 
| (1,2)=3 | (2,2)=1 | 3→1 | (2,2) | 
| (2,1)=2 | (2,2)=1 | 2→1 | (2,2) | 
| (2,2)=1 | dừng lại | 1 | (2,2) | 

Điều này cho thấy rằng tất cả các đường dẫn đều thu gọn thành một mức tối thiểu cục bộ duy nhất và quá trình ghi nhớ sẽ sử dụng lại kết quả cho mọi nút trung gian. 

### Ví dụ Dấu vết 2 

Lưới bị lệch:$$\begin{matrix}
5 & 4 & 3 \\
6 & 7 & 2 \\
9 & 8 & 1
\end{matrix}$$| Bắt đầu | Bước tiếp theo | Đường dẫn | Chìm | 
| --- | --- | --- | --- | 
| 6 (2,1) | 5 (1,1) | 6→5→4→3→2→1 | (3,3) | 
| 7 (2,2) | 2 (1,3) | 7→2→1 | (3,3) | 
| 9 (3,1) | 6 (2,1) | 9→6→...→1 | (3,3) | 

Mọi nút cuối cùng đều chuyển sang mức tối thiểu phía dưới bên phải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| mỗi ô được xử lý một lần trong quá trình ghi nhớ DFS | 
| Không gian |$O(N^2)$| lưu trữ cho lưới, con trỏ tiếp theo và bảng chìm | 

Quá trình tiền xử lý chiếm ưu thế trong giải pháp. Từ$N^2 \le 250{,}000$, điều này phù hợp thoải mái trong giới hạn thời gian và mỗi truy vấn trong số tối đa 20.000 truy vấn được trả lời trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N = int(input())
    grid = [list(map(int, input().split())) for _ in range(N)]
    dirs = [(1,0),(-1,0),(0,1),(0,-1)]
    nxt = [[None]*N for _ in range(N)]
    for i in range(N):
        for j in range(N):
            best = (grid[i][j], i, j)
            for di,dj in dirs:
                ni,nj = i+di,j+dj
                if 0<=ni<N and 0<=nj<N and grid[ni][nj] < best[0]:
                    best = (grid[ni][nj], ni, nj)
            if best[1]==i and best[2]==j:
                nxt[i][j]=(i,j)
            else:
                nxt[i][j]=(best[1],best[2])

    sink = [[None]*N for _ in range(N)]
    sys.setrecursionlimit(10**7)

    def find(i,j):
        if sink[i][j] is not None:
            return sink[i][j]
        ni,nj = nxt[i][j]
        if (ni,nj)==(i,j):
            sink[i][j]=(i,j)
            return (i,j)
        sink[i][j]=find(ni,nj)
        return sink[i][j]

    for i in range(N):
        for j in range(N):
            find(i,j)

    Q = int(input())
    out=[]
    for _ in range(Q):
        x,y = map(int,input().split())
        x-=1;y-=1
        i,j = sink[x][y]
        out.append(str(grid[i][j]))
    return "\n".join(out)

# provided sample (format assumed consistent)
# assert run(...) == "..."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 ô đơn | giá trị đó | hành vi chìm tầm thường | 
| lưới đơn điệu tăng dần ra ngoài | dưới cùng bên phải | sập dây chuyền | 
| bàn cờ giảm dần | sáp nhập bồn rửa nhất quán | hành vi lưu vực chung | 
| 500×500 ngẫu nhiên | đầu ra xác định ổn định | hiệu suất và độ chính xác dưới mức tối đa | 

## Vỏ cạnh 

Lưới 1×1 là trường hợp đơn giản nhất. Thuật toán đánh dấu ô duy nhất là ô chứa chính nó vì không có ô lân cận nào tồn tại. Một truy vấn ngay lập tức trả về nhiệt độ của ô đó mà không cần đệ quy. 

Một lưới giảm dần dọc theo một đường dẫn sẽ kiểm tra các chuỗi dài. Thuật toán vẫn phân giải từng ô một lần trong quá trình ghi nhớ DFS, do đó, mặc dù độ dài đường dẫn lớn nhưng không xảy ra việc tính toán lại. 

Trường hợp nhiều ô chuyển vào cùng một mức tối thiểu cục bộ cho thấy tầm quan trọng của bộ nhớ đệm. Nếu không có tính năng ghi nhớ, mỗi truy vấn sẽ lặp đi lặp lại cùng một đường dẫn, nhưng ở đây tất cả các nút trung gian đều sử dụng lại phần chìm được tính toán, do đó các truy vấn lặp lại vẫn duy trì thời gian không đổi.
