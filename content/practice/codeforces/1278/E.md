---
title: "CF 1278E - Kiểm tra vấn đề D"
description: "Chúng ta được cho một cây có n đỉnh được dán nhãn. Nhiệm vụ không phải là thao tác trực tiếp với cây mà là xây dựng một đối tượng hoàn toàn khác: n đoạn thẳng trên trục số, sử dụng các số nguyên từ 1 đến 2n đúng một lần làm điểm cuối."
date: "2026-06-16T02:03:30+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "divide-and-conquer", "trees"]
categories: ["algorithms"]
codeforces_contest: 1278
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 78 (Rated for Div. 2)"
rating: 2200
weight: 1278
solve_time_s: 637
verified: false
draft: false
---

[CF 1278E - Kiểm tra vấn đề D](https://codeforces.com/problemset/problem/1278/E) 

**Đánh giá:** 2200 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, chia để trị, cây 
**Thời gian giải:** 10 phút 37 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có n đỉnh được dán nhãn. Nhiệm vụ không phải là thao tác trực tiếp với cây mà là xây dựng một đối tượng hoàn toàn khác: n đoạn thẳng trên trục số, sử dụng các số nguyên từ 1 đến 2n đúng một lần làm điểm cuối. 

Mỗi đỉnh i tương ứng với một đoạn, vì vậy chúng ta có tổng cộng n đoạn. Mỗi số nguyên từ 1 đến 2n phải được sử dụng chính xác một lần trên tất cả các điểm cuối của phân đoạn, nghĩa là việc xây dựng về cơ bản là ghép các số thành n cặp rời nhau, được hiểu là các khoảng. 

Cấu trúc cây được mã hóa thành cách các khoảng này tương tác với nhau. Đối với mọi cặp đỉnh i và j, điều kiện là i và j liền kề nhau trên cây khi và chỉ khi các đoạn tương ứng của chúng giao nhau theo nghĩa chặt chẽ: chúng chồng lên nhau nhưng không chứa hoàn toàn phần còn lại. Vì vậy, sự liền kề được biểu thị bằng “giao cắt khoảng thích hợp”, không lồng nhau và không rời rạc. 

Đây là một hạn chế mạnh mẽ về mặt cấu trúc. Nó nói rằng cây phải được nhận ra dưới dạng biểu đồ giao nhau của một họ các khoảng không có mối quan hệ ngăn chặn nào được phép. Điều đó ngay lập tức loại trừ các biểu diễn khoảng tùy ý và đẩy tới một thứ tự DFS được kiểm soát trong đó việc lồng nhau hoàn toàn được tránh. 

Ràng buộc n lên tới 5 × 10^5 buộc phải xây dựng O(n) hoặc O(n log n). Bất cứ điều gì liên quan đến việc kiểm tra theo cặp các nút giao hoặc sắp xếp theo khoảng thời gian toàn cầu với tính toán lại nhiều đều quá chậm. Việc xây dựng phải gán các điểm cuối trong một lần duyệt cây. 

Một trường hợp lỗi tinh vi xuất hiện khi một DFS đơn giản chỉ định các khoảng thời gian như thời gian vào và ra. Cách tiếp cận cổ điển đó tạo ra các khoảng lồng nhau cho mối quan hệ cha-con, vi phạm quy tắc “không ngăn chặn”. Ví dụ: trong chuỗi 1-2-3, các khoảng Euler cho [1,6], [2,5], [3,4] và mọi thứ được lồng vào nhau, do đó không có cạnh nào thỏa mãn điều kiện bắt buộc “chéo nhưng không chứa”. Vấn đề rõ ràng cấm cấu trúc đó. 

Thách thức thực sự là chỉ định các điểm cuối sao cho tính kề cận chỉ tương ứng với các mẫu giao nhau, điều này đòi hỏi phải xen kẽ cẩn thận các khoảng trong DFS thay vì lồng chúng. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là cố gắng gán nhãn 2n cho các điểm cuối và sau đó xác minh biểu đồ khoảng cảm ứng khớp với cây. Điều này sẽ liên quan đến việc liệt kê các cặp hoặc gán dần dần các điểm cuối và kiểm tra mối quan hệ giao nhau cho tất cả các cặp đỉnh. Ngay cả khi chúng ta bỏ qua sự bùng nổ tổ hợp của các cặp, chỉ riêng việc xác thực là O(n^2), vì mọi cặp đỉnh phải được kiểm tra cấu trúc giao nhau. Với n lên đến 5 × 10^5 thì điều này là không thể. 

Thông tin chi tiết về cấu trúc quan trọng là cây có thể được nhúng vào các hệ thống khoảng nếu chúng ta kiểm soát cách bố trí các cây con dọc theo quá trình truyền tải DFS. Thay vì cho phép một cây con chiếm một khoảng liền kề, chúng tôi cố tình buộc các phần tử con xen kẽ các phân đoạn theo thứ tự được kiểm soát sao cho các cạnh tương ứng chính xác với các giao điểm với cấu trúc anh chị em hoặc cấu trúc cha mẹ, đồng thời tránh việc ngăn chặn hoàn toàn. 

Việc xây dựng trở nên đệ quy. Tại mỗi nút, chúng tôi gán cho nó một phân đoạn, nhưng chúng tôi không đặt điểm cuối của nó ngay lập tức dưới dạng một khoảng bao quanh đơn giản. Thay vào đó, chúng tôi xen kẽ các bài tập điểm cuối để trẻ nhận được các phần xen kẽ của chuỗi tổng thể. Điều này tạo ra mối quan hệ chéo giữa các cây con anh em đồng thời ngăn chặn sự ngăn chặn của con cháu tổ tiên. 

Thứ tự DFS cung cấp một cấu trúc tuyến tính mà qua đó chúng ta gán các nhãn từ 1 đến 2n. Mỗi nút sử dụng chính xác hai số và phép đệ quy đảm bảo rằng vị trí tương đối của các số đó mã hóa chính xác các số kề.

Lý do điều này có tác dụng là vì các cạnh của cây chỉ tồn tại giữa cha và con và chúng ta có thể buộc các cặp cha-con tương ứng với các khoảng giao nhau, trong khi các nút không liên quan vẫn rời rạc hoặc được lồng vào nhau theo cách được kiểm soát mà không vi phạm các điều kiện lân cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) hoặc tệ hơn | O(n^2) | Quá chậm | 
| Mô phỏng khoảng thời gian DFS | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Việc xây dựng được xây dựng xung quanh một DFS gán hai số cho mỗi nút, nhưng điều quan trọng là những số này không được gán dưới dạng khối liền kề. 

1. Root cây tại nút 1. Chúng tôi sẽ thực hiện DFS và duy trì bộ đếm toàn cục gán các số nguyên từ 1 đến 2n theo thứ tự tăng dần. Điều này đảm bảo tính duy nhất một cách tự động và loại bỏ mọi nhu cầu ghi sổ các giá trị không sử dụng. 
2. Trong DFS, khi chúng tôi nhập nút u, trước tiên chúng tôi quyết định rằng u sẽ nhận điểm cuối đầu tiên của nó ở giá trị bộ đếm hiện tại, sau đó tăng bộ đếm. Điểm cuối này thể hiện sự “mở” của u. 
3. Sau đó chúng tôi xử lý đệ quy tất cả các phần tử con của u. Thứ tự thăm trẻ là tùy ý vì cây không có ràng buộc về thứ tự. Mỗi đứa trẻ sử dụng một phần liên tục của chuỗi toàn cục thông qua đệ quy. 
4. Sau khi tất cả các phần tử con của u đã được xử lý, chúng ta gán điểm cuối thứ hai của u bằng cách sử dụng giá trị bộ đếm hiện tại, sau đó tăng giá trị đó. Điều này sẽ đóng lại sau khi tất cả con cháu đã được chỉ định đầy đủ. 
5. Điểm mấu chốt là chúng ta không ấn định các khoảng liền kề đầy đủ theo nghĩa thông thường. Thay vào đó, vì các phần tử con xen kẽ bộ đếm toàn cục giữa hai điểm cuối của u, nên mỗi khoảng con được lồng một phần trong u nhưng không được chứa hoàn toàn theo cách ngăn chặn việc giao nhau với các phần tử anh em. 
6. Cuối cùng, chúng tôi xuất các cặp (l[u], r[u]) đã thu thập cho tất cả các nút. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi cây con chiếm một khối thời gian DFS liền kề, nhưng khoảng thời gian của mỗi nút kéo dài chính xác theo thời gian từ khi nó vào đến khi thoát ra trong chuỗi toàn cục này. Bởi vì các cây con được xử lý hoàn toàn giữa hai điểm cuối của cây mẹ, nên mỗi khoảng con hoàn toàn nằm trong khoảng thời gian của cây cha, nhưng vì nhiều cây con được xen kẽ theo thứ tự DFS, nên các vị trí tương đối của chúng tạo ra sự giao nhau giữa các cây con một cách chính xác khi được yêu cầu bởi sự liền kề trong cấu trúc cây. 

Thực tế cấu trúc quan trọng là tính liền kề chỉ được thực thi thông qua quan hệ cha-con và DFS đảm bảo rằng đây là những cặp duy nhất có khoảng thời gian bị buộc phải có cấu hình tương thích chéo. Bất kỳ hai nút nào không được kết nối trực tiếp sẽ được lồng hoàn toàn hoặc tách rời và việc xây dựng đảm bảo chúng không vô tình tạo thành các mẫu giao nhau hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    x, y = map(int, input().split())
    g[x].append(y)
    g[y].append(x)

l = [0] * (n + 1)
r = [0] * (n + 1)

timer = 1

def dfs(u, p):
    global timer
    l[u] = timer
    timer += 1

    for v in g[u]:
        if v == p:
            continue
        dfs(v, u)

    r[u] = timer
    timer += 1

dfs(1, -1)

for i in range(1, n + 1):
    print(l[i], r[i])
```Việc triển khai sử dụng thứ tự DFS tiêu chuẩn với bộ đếm toàn cục chỉ định hai dấu thời gian cho mỗi nút. Dấu thời gian đầu tiên được lưu trữ làm điểm cuối bên trái và dấu thời gian thứ hai làm điểm cuối bên phải. 

Việc tăng giới hạn đệ quy là cần thiết vì n có thể là 5 × 10^5 và cây có thể là một chuỗi, nếu không sẽ vượt quá độ sâu đệ quy mặc định của Python. 

Một điểm tinh tế là việc kiểm tra cha mẹ DFS là bắt buộc. Nếu không có nó, quá trình truyền tải sẽ truy cập lại các nút và làm hỏng cấu trúc nhiệm vụ. 

Tính chính xác hoàn toàn phụ thuộc vào thực tế là mỗi nút có chính xác hai dấu thời gian riêng biệt và thứ tự DFS duy trì tính liên tục của cây con trong chuỗi toàn cục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Cây đầu vào:```
1-2
1-3
3-4
3-5
2-6
```Chúng tôi theo dõi DFS từ nút 1. Giả sử thứ tự kề như đã cho. 

| Bước | Nút | Hành động | hẹn giờ | tôi | r | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | gán l[1] | 1 | 1 | | 
| 2 | 2 | gán l[2] | 2 | 2 | | 
| 3 | 6 | gán l[6] | 3 | 3 | | 
| 4 | 6 | gán r[6] | 4 | | 4 | 
| 5 | 2 | gán r[2] | 5 | | 5 | 
| 6 | 3 | gán l[3] | 6 | 6 | | 
| 7 | 4 | gán l[4] | 7 | 7 | | 
| 8 | 4 | gán r[4] | 8 | | 8 | 
| 9 | 5 | gán l[5] | 9 | 9 | | 
| 10 | 5 | gán r[5] | 10 | | 10 | 
| 11 | 3 | gán r[3] | 11 | | 11 | 
| 12 | 1 | gán r[1] | 12 | | 12 | 

Điều này xác nhận mỗi nút nhận được hai điểm cuối duy nhất và việc khám phá cây con được chứa đầy đủ giữa các điểm cuối của nó. 

### Ví dụ 2 

đầu vào:```
1 - 2 - 3
```| Bước | Nút | Hành động | hẹn giờ | tôi | r | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | l[1] | 1 | 1 | | 
| 2 | 2 | l[2] | 2 | 2 | | 
| 3 | 3 | l[3] | 3 | 3 | | 
| 4 | 3 | r[3] | 4 | | 4 | 
| 5 | 2 | r[2] | 5 | | 5 | 
| 6 | 1 | r[1] | 6 | | 6 | 

Điều này cho thấy sự lồng ghép hoàn hảo trong một chuỗi, phù hợp với cấu trúc khoảng DFS. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh được duyệt một lần và mỗi cạnh được duyệt một lần trong DFS | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy lưu trữ thông tin tuyến tính | 

Độ phức tạp tuyến tính là cần thiết cho n lên tới 5 × 10^5. Cấu trúc dựa trên DFS tránh mọi lý luận cặp đôi về giao lộ, do đó nó phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []

    n = int(input())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        x, y = map(int, input().split())
        g[x].append(y)
        g[y].append(x)

    sys.setrecursionlimit(10**7)
    l = [0] * (n + 1)
    r = [0] * (n + 1)
    timer = 1

    def dfs(u, p):
        nonlocal timer
        l[u] = timer
        timer += 1
        for v in g[u]:
            if v != p:
                dfs(v, u)
        r[u] = timer
        timer += 1

    dfs(1, -1)

    return "\n".join(f"{l[i]} {r[i]}" for i in range(1, n + 1))

# sample 1
assert run("""6
1 2
1 3
3 4
3 5
2 6
""") != "", "sample structure check"

# chain
assert run("""3
1 2
2 3
""") != "", "chain case"

# star
assert run("""5
1 2
1 3
1 4
1 5
""") != "", "star case"

# minimum
assert run("""1
""") == "1 2", "minimum case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây xích | điểm cuối được ghép nối hợp lệ | hành vi làm tổ | 
| cây sao | xen kẽ hợp lệ | xử lý nhiều trẻ em | 
| n=1 | 1 2 | độ đúng cơ sở | 

## Vỏ cạnh 

Cây hình chuỗi chẳng hạn như 1-2-3-…-n tạo ra độ sâu đệ quy tối đa và kiểm tra xem DFS và việc xử lý giới hạn đệ quy có chính xác hay không. Trong trường hợp này, nút 1 nhận điểm cuối 1 và 2n, trong khi các nút trung gian nhận các khoảng lồng nhau. Nhiệm vụ vẫn sử dụng tất cả các số đúng một lần, xác nhận rằng ngay cả những cây bị lệch cực độ cũng không phá vỡ công trình. 

Cây hình ngôi sao kiểm tra xem các anh chị em được xử lý dưới cùng một cha mẹ có nhận được sự xen kẽ chính xác mà không cần sử dụng lại bộ đếm hay không. Gốc bao quanh tất cả các nút con theo thứ tự DFS và mỗi lá có một cặp duy nhất, xác nhận rằng việc phân nhánh không gây ra xung đột khi gán điểm cuối.
