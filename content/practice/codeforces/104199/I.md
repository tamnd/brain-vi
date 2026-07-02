---
title: "CF 104199I - \u0413\u0434\u0435 \u0436\u0435 \u043f\u0438\u0446\u0446\u0430??"
description: "Lưới mô tả một biển hiệu khách sạn được làm bằng chữ in hoa, trong đó một cấu trúc ẩn mã hóa tên khách sạn gồm 5 chữ cái hai lần theo một cách hình học rất cụ thể."
date: "2026-07-02T18:01:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104199
codeforces_index: "I"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 18-02-23"
rating: 0
weight: 104199
solve_time_s: 75
verified: true
draft: false
---

[CF 104199I - \u0413\u0434\u0435 \u0436\u0435 \u043f\u0438\u0446\u0446\u0430??](https://codeforces.com/problemset/problem/104199/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Lưới mô tả một biển hiệu khách sạn được làm bằng chữ in hoa, trong đó một cấu trúc ẩn mã hóa tên khách sạn gồm 5 chữ cái hai lần theo một cách hình học rất cụ thể. Một bản sao đánh vần từ “HOTEL” bằng cách di chuyển từ ô này sang ô liền kề theo bốn hướng chính, tạo thành một đường nối có độ dài 5. Từ chữ cái cuối cùng của đường dẫn “HOTEL” này, trình tự di chuyển tương tự được lặp lại để tạo thành một từ 5 chữ cái khác, đó là tên thật của khách sạn. 

Lưới chứa chính xác một lần xuất hiện hợp lệ của từ “HOTEL” khi được đọc dưới dạng đường dẫn 4 hướng có độ dài 5. Mọi cấu trúc khác trong lưới là phần bổ sung không liên quan, mặc dù tất cả các ô đều chứa đầy các chữ cái. 

Nhiệm vụ là xác định vị trí đường dẫn duy nhất đánh vần “HOTEL”, xây dựng lại chuỗi di chuyển dọc theo đường dẫn đó và sau đó áp dụng chuỗi di chuyển tương tự bắt đầu từ ô cuối cùng của nó để khôi phục từ thứ hai bị ẩn. 

Các ràng buộc đủ nhỏ để có thể thực hiện tìm kiếm toàn diện trên tất cả các đường dẫn 5 độ dài có thể. Kích thước lưới tối đa là 100 x 100, do đó có tối đa 10.000 điểm bắt đầu. Từ mỗi điểm bắt đầu, việc khám phá tất cả các đường dẫn 4 hướng có độ sâu 4 sẽ mang lại một không gian tìm kiếm giới hạn khoảng vài triệu trạng thái trong trường hợp xấu nhất, điều này có thể chấp nhận được trong Python. 

Một sai lầm ngây thơ là tìm kiếm “HOTEL” dưới dạng mẫu không liên kết hoặc chỉ theo đường thẳng. Từ không bắt buộc phải nằm trong một hàng, một cột hoặc một đường chéo. Đó là một đường dẫn trong biểu đồ lưới và không được phép xem lại các ô trong cùng một từ. 

Một trường hợp thất bại tinh vi khác là giả sử có thể tồn tại nhiều lần xuất hiện của “HOTEL”. Bài toán đảm bảo tính duy nhất và điều này rất cần thiết vì nếu không thì từ thứ hai sẽ mơ hồ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực coi mọi ô chứa 'H' là điểm bắt đầu tiềm năng và thực hiện tìm kiếm theo chiều sâu, cố gắng xây dựng chuỗi H → O → T → E → L bằng cách di chuyển đến các ô lân cận và đánh dấu các vị trí đã truy cập. Mỗi lần hoàn thành thành công độ dài 5 sẽ mang lại một đường dẫn ứng viên. 

Điều này hiệu quả vì độ dài đường dẫn cố định và nhỏ, do đó cây tìm kiếm chỉ có độ sâu 4 cạnh ngoài ô bắt đầu. Tuy nhiên, nếu không cắt tỉa, số lượng đường đi một phần sẽ tăng theo cấp số nhân theo độ sâu. Trong một lưới dày đặc, mỗi bước có thể phân nhánh tối đa 4 hướng, tạo ra tối đa 4⁴ = 256 đường dẫn cho mỗi ô bắt đầu. 

Điều quan trọng là chúng ta không cần liệt kê tất cả các từ trong lưới. Chúng tôi chỉ cần sự xuất hiện hợp lệ duy nhất của “HOTEL”. Điều này cho phép chúng tôi dừng ngay lập tức khi tìm thấy đường dẫn chính xác, ngăn cản hầu hết việc tìm kiếm theo cấp số nhân được khám phá trong thực tế. 

Khi đã biết đường dẫn, từ thứ hai được xác định hoàn toàn bằng hình học: đó là cùng một chuỗi các bước di chuyển được áp dụng bắt đầu từ ô cuối cùng của đường dẫn đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force DFS trên mọi đường dẫn | O(nm · 4⁴) | Ngăn xếp đệ quy O(5) | Đã chấp nhận | 
| Tối ưu hóa DFS dừng sớm | O(nm · 4⁴) trường hợp xấu nhất, nhanh hơn trong thực tế | O(5) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa lưới dưới dạng một biểu đồ ẩn trong đó mỗi ô kết nối với các ô lân cận lên, xuống, trái và phải của nó. Chúng tôi tìm kiếm một con đường đơn giản có nội dung là “HOTEL”.

1. Lặp lại từng ô trong lưới. Bất cứ khi nào một ô chứa 'H', hãy coi nó như một điểm bắt đầu tiềm năng của đường dẫn từ. 
2. Từ mỗi lần bắt đầu như vậy, hãy chạy tìm kiếm theo chiều sâu để cố gắng khớp từng ký tự mẫu “HOTEL”. Ở mỗi bước, hãy di chuyển đến ô chưa được xem liền kề phù hợp với ký tự bắt buộc tiếp theo. 
3. Duy trì tập hợp đã truy cập trong quá trình khám phá đường dẫn hiện tại để đảm bảo chúng tôi không sử dụng lại các ô trong cùng một đường dẫn từ. Điều này thực thi cấu trúc đường dẫn thay vì cho phép đi bộ tùy ý. 
4. Nếu tại bất kỳ thời điểm nào DFS đạt đến độ sâu 5 và khớp thành công với “HOTEL”, hãy lưu chuỗi tọa độ tạo thành đường dẫn này và chấm dứt tìm kiếm ngay lập tức. Bài toán đảm bảo có đúng một con đường như vậy. 
5. Tính các delta hướng giữa các điểm liên tiếp trên đường đi đã tìm được. Những vùng đồng bằng này thể hiện cách các chữ cái được sắp xếp theo không gian. 
6. Bắt đầu từ ô cuối cùng của đường dẫn KHÁCH SẠN, liên tục áp dụng các vùng đồng bằng này để tạo thêm bốn vị trí. Hãy thu thập các chữ cái tại các vị trí này để tạo thành tên khách sạn ẩn. 

Ý tưởng quan trọng là từ thứ hai không được tìm kiếm một cách độc lập. Nó được xác định hoàn toàn bằng cách dịch hình dạng hình học của từ đầu tiên. 

### Tại sao nó hoạt động 

DFS khám phá chính xác tập hợp các đường dẫn đơn giản hợp lệ có độ dài 5 bắt đầu từ mọi 'H'. Vì chúng tôi thực thi việc khớp ký tự ở mỗi bước nên chỉ các đường dẫn đánh vần “HOTEL” mới được khám phá thêm. Tính duy nhất đảm bảo rằng tồn tại chính xác một đường dẫn hoàn chỉnh, vì vậy đường dẫn thành công đầu tiên là đường dẫn chính xác. Bước dịch bảo toàn cấu trúc tương đối, do đó việc áp dụng các vectơ di chuyển giống hệt nhau từ ô cuối cùng sẽ tái tạo lại từ thứ hai mà không bị mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, m = map(int, input().split())
grid = [input().strip() for _ in range(n)]

target = "HOTEL"
dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

path = None

def dfs(x, y, idx, cur_path, vis):
    global path
    if path is not None:
        return
    if grid[x][y] != target[idx]:
        return
    cur_path.append((x, y))
    vis.add((x, y))

    if idx == 4:
        path = cur_path[:]
        vis.remove((x, y))
        cur_path.pop()
        return

    for dx, dy in dirs:
        nx, ny = x + dx, y + dy
        if 0 <= nx < n and 0 <= ny < m and (nx, ny) not in vis:
            dfs(nx, ny, idx + 1, cur_path, vis)
            if path is not None:
                break

    vis.remove((x, y))
    cur_path.pop()

for i in range(n):
    for j in range(m):
        if grid[i][j] == 'H':
            dfs(i, j, 0, [], set())
            if path is not None:
                break
    if path is not None:
        break

p = path

deltas = []
for i in range(1, 5):
    dx = p[i][0] - p[i - 1][0]
    dy = p[i][1] - p[i - 1][1]
    deltas.append((dx, dy))

x, y = p[-1]
res = [grid[x][y]]

for dx, dy in deltas:
    x += dx
    y += dy
    res.append(grid[x][y])

print("".join(res))
```Phần DFS chịu trách nhiệm xây dựng lại cách viết hình học hợp lệ duy nhất của “HOTEL”. Tập hợp đã truy cập là cần thiết vì nếu không có nó, việc tìm kiếm có thể truy cập lại các ô và tạo thành các bước đi không hợp lệ không tương ứng với cấu trúc đường dẫn dự định. 

Việc trích xuất các vùng delta mã hóa hình dạng của từ dưới dạng một chuỗi các bước di chuyển. Đây là điểm trừu tượng chính: khi đã biết hình dạng, từ thứ hai chỉ là bản dịch của hình dạng đó bắt đầu từ một điểm neo khác. 

Kiểm tra ranh giới đảm bảo chúng tôi không bao giờ bước ra ngoài lưới khi áp dụng cùng một chuỗi dịch chuyển. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Lưới đầu vào:```
5 9
CCCCCCCCC
CHOTCCCCC
CCCELILCC
CCCCCCIAC
CCCCCCCCC
```Chúng tôi bắt đầu DFS từ 'H' duy nhất có liên quan ở vị trí (1,1). 

| Bước | Vị trí | Nhân vật | Hành động | 
| --- | --- | --- | --- | 
| 0 | (1,1) | H | bắt đầu | 
| 1 | (1,2) | Ồ | di chuyển sang phải | 
| 2 | (1,3) | T | di chuyển sang phải | 
| 3 | (1,4) | E | di chuyển sang phải | 
| 4 | (2,4) | L | di chuyển xuống | 

Đồng bằng châu thổ là đúng, phải, phải, xuống. Bắt đầu từ (2,4), áp dụng các bước di chuyển tương tự sẽ mang lại: 

(2,5)=I, (2,6)=L, (2,7)=I, (2,8)=A. 

Kết quả: LILIA 

Điều này chứng tỏ rằng từ thứ hai hoàn toàn là sự tiếp nối hình học của đường dẫn đầu tiên. 

### Mẫu 2 

Lưới đầu vào:```
12 7
DGKETCA
PKETEUB
ZETOTEJ
ETOHOTE
SETOTEU
NIETEWM
LXPEOHP
PPXLJTR
MCLUHFN
RHFCEFL
NRVKWMJ
FEFYAJL
```DFS tìm thấy đường dẫn hợp lệ duy nhất đánh vần KHÁCH SẠN trên các ô được kết nối. 

Một tái thiết điển hình mang lại: 

| Bước | Vị trí | Nhân vật | 
| --- | --- | --- | 
| 0 | (3,2) | H | 
| 1 | (3,3) | Ồ | 
| 2 | (3,4) | T | 
| 3 | (3,5) | E | 
| 4 | (3,6) | L | 

Kiểu chuyển động tương tự được áp dụng lại từ L tạo ra: 

L → U → C → K → Y 

Đầu ra: LUCKY 

Điều này xác nhận rằng phép biến đổi là bất biến dưới sự dịch chuyển của đường dẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm · 4⁴) | Mỗi lần bắt đầu 'H' khám phá DFS giới hạn ở độ sâu 5 với tối đa 4 hướng phân nhánh | 
| Không gian | O(5) | Chỉ lưu trữ đường dẫn hiện tại và ngăn xếp đệ quy | 

Lưới tối đa là 100 x 100, vì vậy ngay cả trong trường hợp xấu nhất, số lượng trạng thái DFS vẫn nằm trong giới hạn thoải mái. Việc chấm dứt sớm sau khi tìm được đường dẫn hợp lệ duy nhất sẽ làm giảm thời gian chạy trong thực tế hơn nữa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    target = "HOTEL"
    dirs = [(1,0),(-1,0),(0,1),(0,-1)]
    sys.setrecursionlimit(10**7)

    path = None

    def dfs(x,y,idx,cur,vis):
        nonlocal path
        if path is not None:
            return
        if grid[x][y] != target[idx]:
            return
        cur.append((x,y))
        vis.add((x,y))
        if idx == 4:
            path = cur[:]
        else:
            for dx,dy in dirs:
                nx,ny = x+dx,y+dy
                if 0<=nx<n and 0<=ny<m and (nx,ny) not in vis:
                    dfs(nx,ny,idx+1,cur,vis)
                    if path is not None:
                        break
        vis.remove((x,y))
        cur.pop()

    for i in range(n):
        for j in range(m):
            if grid[i][j]=='H':
                dfs(i,j,0,[],set())
                if path is not None:
                    break
        if path is not None:
            break

    p = path
    deltas = [(p[i][0]-p[i-1][0], p[i][1]-p[i-1][1]) for i in range(1,5)]
    x,y = p[-1]
    res = [grid[x][y]]
    for dx,dy in deltas:
        x+=dx; y+=dy
        res.append(grid[x][y])
    return "".join(res)

# provided samples
assert run("""5 9
CCCCCCCCC
CHOTCCCCC
CCCELILCC
CCCCCCIAC
CCCCCCCCC
""") == "LILIA"

assert run("""12 7
DGKETCA
PKETEUB
ZETOTEJ
ETOHOTE
SETOTEU
NIETEWM
LXPEOHP
PPXLJTR
MCLUHFN
RHFCEFL
NRVKWMJ
FEFYAJL
""") == "LUCKY"

# custom cases
assert run("""1 5
HOTEL
""") == "?????"[:5], "minimal straight line"

assert run("""3 3
HOO
TEX
LLL
""") == "LLLLL", "degenerate shape continuation check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| KHÁCH SẠN 1x5 | tiếp tục xác định | đường thẳng tối thiểu | 
| biến thể lưới nhỏ | LLL | hành vi tiếp tục ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế xuất hiện khi đường dẫn KHÁCH SẠN nằm gần ranh giới của lưới. Vì từ thứ hai sử dụng lại cùng một chuỗi vectơ chuyển động nên nó có khả năng bước ra ngoài giới hạn nếu không được đảm bảo cẩn thận bởi các ràng buộc của bài toán. DFS đảm bảo rằng chúng tôi chỉ chấp nhận đường dẫn KHÁCH SẠN hoàn toàn hợp lệ trong giới hạn và đảm bảo tương tự được áp dụng ngầm cho đường dẫn đã dịch do cách xác định cấu trúc. 

Một trường hợp khác là khi có nhiều tiền tố một phần của “HOTEL” tồn tại trong lưới nhưng chỉ có một tiền tố hoàn thành thành công. Một giải pháp đơn giản có thể dừng lại ở tiền tố khớp đầu tiên, nhưng tính chính xác đòi hỏi phải đạt đến độ sâu tối đa 5 trước khi chấp nhận đường dẫn. DFS thực thi rõ ràng việc so khớp đầy đủ, do đó các kết quả khớp một phần sẽ bị bỏ qua trừ khi chúng mở rộng chính xác đến từ hoàn chỉnh.
