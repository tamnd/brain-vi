---
title: "CF 106167K - Hội nghị Killjoys"
description: "Chúng tôi được cung cấp một nhóm người và danh sách các cặp không được ngồi cùng phòng. Một sự sắp xếp cuộc họp hợp lệ sẽ phân công mỗi người vào đúng một trong hai phòng, Đông hoặc Tây, sao cho mỗi cặp “không thích” được chia thành các phòng."
date: "2026-06-21T09:44:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106167
codeforces_index: "K"
codeforces_contest_name: "2021-2022 ICPC German Collegiate Programming Contest (GCPC 2021)"
rating: 0
weight: 106167
solve_time_s: 79
verified: true
draft: false
---

[CF 106167K - Hội nghị của Killjoys](https://codeforces.com/problemset/problem/106167/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một nhóm người và danh sách các cặp không được ngồi cùng phòng. Một sự sắp xếp cuộc họp hợp lệ sẽ phân công mỗi người vào đúng một trong hai phòng, Đông hoặc Tây, sao cho mỗi cặp “không thích” được chia thành các phòng. 

Mỗi năm chúng ta phải chọn lại một nhiệm vụ hợp lệ như vậy cho cùng một nhóm người và những ràng buộc tương tự. Điều đáng chú ý là chúng tôi đang cố gắng tiếp tục tạo các bài tập hợp lệ mới mà không bao giờ lặp lại bài tập đã được sử dụng trước đó và việc hoán đổi hai phòng được coi là giống hệt nhau chứ không phải là mới. Chúng tôi muốn biết vào năm nào việc lặp lại trở nên không thể tránh khỏi, nghĩa là tất cả các bài tập riêng biệt (theo mức tương đương này) đã được sử dụng hết. 

Đầu vào mô tả một biểu đồ trong đó các đỉnh là người và các cạnh là các mối quan hệ không thích. Phép gán hợp lệ chính xác là phép tô màu 2 của biểu đồ này. Đầu ra là năm đầu tiên phải xảy ra sự lặp lại, lấy modulo một số nguyên tố lẻ cho trước hoặc từ không thể nếu không tồn tại 2 màu hợp lệ. 

Các ràng buộc lên tới một triệu đỉnh và cạnh, do đó, bất kỳ giải pháp nào về cơ bản đều phải tuyến tính theo kích thước của biểu đồ, với chi phí tối đa là logarit. Bất cứ điều gì liên quan đến việc liệt kê các bài tập hoặc khám phá các chất tạo màu theo cấp số nhân đều bị loại trừ ngay lập tức. 

Một trường hợp cạnh ngây thơ nhưng nguy hiểm là khi đồ thị chứa một chu trình lẻ. Ví dụ: tam giác 1-2-3-1 khiến không thể chia thành hai phòng. Việc triển khai bất cẩn chỉ gán một cách tham lam mà không kiểm tra tính nhất quán vẫn có thể tạo ra một số con số, nhưng kết quả đầu ra chính xác là không thể. 

Một trường hợp tinh vi khác là đồ thị không có cạnh. Trong trường hợp này, mọi phép gán đều hợp lệ, nhưng việc hoán đổi Đông và Tây được coi là giống hệt nhau, do đó số lượng các phép gán riêng biệt không phải là 2^n mà thay vào đó sẽ thu gọn theo tính đối xứng toàn cục. Đây là nơi mà việc đếm ngây thơ các lựa chọn đỉnh độc lập thường bị tính quá mức. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là tạo ra mọi sự phân công hợp lệ của mọi người vào các phòng Đông và Tây và lưu trữ chúng thành một bộ, dừng lại khi một bản sao xuất hiện. Mỗi phép gán là một nhãn nhị phân của biểu đồ, nhưng nó cũng phải đáp ứng tất cả các ràng buộc về cạnh, vì vậy chúng ta cần liệt kê tất cả các cách tô màu hợp lệ. 

Ngay cả khi bỏ qua chi phí xác thực, số lượng bài tập thô là 2^n, điều này là không thể đối với n lên tới 10^6. Ngay cả khi chúng tôi giới hạn bản thân chỉ với các màu hợp lệ, số lượng vẫn tăng theo cấp số nhân với số lượng thành phần được kết nối, do đó việc liệt kê vẫn không khả thi. 

Quan sát quan trọng là tính hợp lệ hoàn toàn mang tính cục bộ đối với các thành phần được kết nối. Mỗi thành phần được kết nối của biểu đồ hoạt động độc lập: khi chúng ta sửa màu của một đỉnh trong một thành phần thì phần còn lại của thành phần đó sẽ bị ép buộc. Vì vậy, mỗi thành phần đóng góp chính xác một lựa chọn nhị phân, tương ứng với việc lật tất cả các màu trong thành phần đó. 

Nếu có c thành phần được kết nối thì có 2^c màu thô hợp lệ. Tuy nhiên, việc hoán đổi Đông và Tây trên toàn cầu không tạo ra một nhiệm vụ mới, vì vậy mọi cấu hình đều được ghép nối với phần bổ sung toàn cầu của nó. Điều này chia số đếm cho 2, để lại 2^(c−1) phép gán riêng biệt. 

Quá trình được mô tả trong bài toán liệt kê một cách hiệu quả tất cả các phép gán riêng biệt cho đến khi cạn kiệt, do đó việc lặp lại là không thể tránh khỏi ngay sau khi sử dụng tất cả các phép gán duy nhất. Điều đó có nghĩa là câu trả lời là 2^(c−1) + 1, được tính theo modulo p. 

Trước khi đếm, chúng ta cũng phải đảm bảo đồ thị là lưỡng cực. Nếu bất kỳ thành phần nào không có 2 màu, quá trình sẽ bị hỏng ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các nhiệm vụ Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Các thành phần được kết nối + Kiểm tra lưỡng cực + Nguồn | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi quy vấn đề về các thuộc tính của cấu trúc biểu đồ và số lượng lựa chọn nhị phân độc lập mà nó chứa. 

1. Xây dựng biểu đồ từ các cặp không thích cho trước. Mỗi người là một nút và mỗi cặp là một cạnh vô hướng. 
2. Chạy BFS hoặc DFS trên mỗi nút chưa được truy cập để kiểm tra tính lưỡng cực và đếm các thành phần được kết nối. Trong khi duyệt qua một thành phần, hãy gán các màu xen kẽ để phát hiện sự mâu thuẫn. Nếu chúng ta cố gắng gán cho một nút một màu khác với màu mà nó đã có thì biểu đồ đó không phải là biểu đồ hai bên và không có phép gán hợp lệ nào tồn tại. 
3. Mỗi lần chúng tôi bắt đầu BFS/DFS mới từ một nút chưa được truy cập, chúng tôi sẽ tăng số lượng thành phần được kết nối c. Điều này hiệu quả vì mỗi lần truyền tải sẽ khám phá đầy đủ một thành phần. 
4. Nếu phát hiện thấy mâu thuẫn tại bất kỳ điểm nào, chúng tôi sẽ ngay lập tức đưa ra kết quả là không thể. 
5. Khi chúng ta có c, hãy tính số lượng nhiệm vụ riêng biệt để hoán đổi tất cả các phòng trên toàn cầu. Đây là 2^(c−1) modulo p. 
6. Lần lặp lại đầu tiên xảy ra ngay sau khi hết tất cả các phép gán riêng biệt, vì vậy chúng ta xuất ra 2^(c−1) + 1 modulo p. 

Điểm tinh tế là tại sao chỉ có các thành phần quan trọng: trong một thành phần được kết nối, việc cố định một đỉnh sẽ xác định phần còn lại, do đó mỗi thành phần đóng góp chính xác một bit lật độc lập. Hoán đổi toàn cầu loại bỏ đồng thời một bậc tự do trên tất cả các thành phần. 

### Tại sao nó hoạt động 

Mọi phép gán hợp lệ đều tương ứng với việc chọn hướng nhị phân cho từng thành phần được kết nối. Hai phép gán giống hệt nhau theo tính tương đương của bài toán nếu chúng chỉ khác nhau bằng cách lật tất cả các thành phần cùng một lúc. Điều này có nghĩa là không gian của các cấu hình riêng biệt là thương số của khối boolean c chiều theo đối xứng lật toàn cục, để lại chính xác 2^(c−1) trạng thái riêng biệt. Vì mỗi năm phải sử dụng một trạng thái mới cho đến khi cạn kiệt nên lần lặp lại bắt buộc đầu tiên xảy ra ngay sau khi bộ này cạn kiệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n, m, p = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    
    for _ in range(m):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)

    color = [-1] * (n + 1)
    components = 0

    for i in range(1, n + 1):
        if color[i] != -1:
            continue
        components += 1
        stack = [i]
        color[i] = 0

        while stack:
            v = stack.pop()
            for to in g[v]:
                if color[to] == -1:
                    color[to] = color[v] ^ 1
                    stack.append(to)
                elif color[to] == color[v]:
                    print("impossible")
                    return

    if components == 0:
        print(1 % p)
        return

    ans = pow(2, components - 1, p) + 1
    print(ans % p)

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu trữ dưới dạng danh sách kề để cả việc kiểm tra truyền tải và kiểm tra hai bên đều chạy theo thời gian tuyến tính. DFS sử dụng một ngăn xếp rõ ràng để tránh các vấn đề về độ sâu đệ quy ở một triệu nút. 

các`color`mảng đồng thời theo dõi lượt truy cập và tô màu hai bên. Một giá trị của`-1`có nghĩa là chưa được thăm viếng, trong khi`0`Và`1`đại diện cho hai phòng. Bất kỳ xung đột nào mà hàng xóm đã có cùng màu đều báo hiệu một chu kỳ kỳ lạ. 

Việc đếm thành phần được thực hiện bằng cách bắt đầu một quá trình truyền tải mới bất cứ khi nào chúng ta tìm thấy một nút chưa được ghé thăm. Điều này trực tiếp mang lại số lượng lựa chọn nhị phân độc lập trong công thức cuối cùng. 

Cuối cùng, phép lũy thừa mô-đun được sử dụng cho 2^(c−1) theo mô-đun p và chúng tôi thêm 1 để tính cho lần lặp lại bắt buộc đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đồ thị đầu vào: 4 nút, cạnh (1-2) và (3-4). 

Chúng tôi theo dõi các thành phần và màu sắc: 

| Bước | Nút | Hành động | Linh kiện | Màu sắc | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | thành phần mới | 1 | 1:0 | 
| BFS | 2 | màu đối lập | 1 | 1:0, 2:1 | 
| Tiếp theo | 3 | thành phần mới | 2 | 3:0 | 
| BFS | 4 | màu đối lập | 2 | 3:0, 4:1 | 

Chúng tôi kết thúc với 2 thành phần được kết nối. Số lượng bài tập riêng biệt là 2^(2−1) = 2, do đó lần lặp lại đầu tiên xảy ra vào năm thứ 3. 

Điều này phù hợp với trực giác rằng mỗi thành phần cặp cạnh có thể được đảo ngược, nhưng việc hoán đổi toàn cục sẽ xác định các cấu hình đối xứng. 

### Ví dụ 2 

Đồ thị đầu vào: 5 nút, các cạnh (1-2) và (3-4), nút 5 bị cô lập. 

| Bước | Nút | Hành động | Linh kiện | Màu sắc | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | thành phần mới | 1 | 1:0 | 
| BFS | 2 | màu đối lập | 1 | 1:0, 2:1 | 
| Tiếp theo | 3 | thành phần mới | 2 | 3:0 | 
| BFS | 4 | màu đối lập | 2 | 3:0, 4:1 | 
| Tiếp theo | 5 | thành phần mới | 3 | 5:0 | 

Bây giờ chúng ta có 3 thành phần, vì vậy các phép gán riêng biệt là 2^(3−1) = 4 và sự lặp lại bắt đầu từ 5. 

Dấu vết cho thấy các đỉnh bị cô lập hoạt động giống hệt như các thành phần có cấu trúc cưỡng bức, góp phần độc lập vào việc đếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và cạnh được xử lý một lần trong DFS/BFS | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng tô màu | 

Độ phức tạp tuyến tính là cần thiết vì cả n và m đều có thể đạt tới một triệu. Thuật toán chỉ thực hiện công không đổi trên mỗi cạnh và mỗi đỉnh, do đó nó phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    # assume solution is defined above in same file
    # re-implement minimal call
    def solve():
        n, m, p = map(int, sys.stdin.readline().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(m):
            a, b = map(int, sys.stdin.readline().split())
            g[a].append(b)
            g[b].append(a)

        color = [-1] * (n + 1)
        comp = 0

        for i in range(1, n + 1):
            if color[i] != -1:
                continue
            comp += 1
            stack = [i]
            color[i] = 0
            while stack:
                v = stack.pop()
                for to in g[v]:
                    if color[to] == -1:
                        color[to] = color[v] ^ 1
                        stack.append(to)
                    elif color[to] == color[v]:
                        print("impossible")
                        return
        if comp == 0:
            print(1 % p)
            return
        print((pow(2, comp - 1, p) + 1) % p)

    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("4 2 11\n1 2\n3 4\n") == "3"
assert run("5 2 3\n1 2\n3 4\n") == "2"

# custom cases
assert run("3 3 11\n1 2\n2 3\n3 1\n") == "impossible"
assert run("1 0 13\n") == "2"
assert run("2 0 13\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 chu kỳ | không thể | phát hiện chu kỳ lẻ | 
| nút đơn | 2 | trường hợp cơ sở c=1 | 
| hai nút bị cô lập | 3 | nhiều thành phần không có cạnh | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một biểu đồ hoàn toàn bị ngắt kết nối. Trong trường hợp đó, mỗi nút là thành phần riêng của nó, nên số thành phần bằng n. Thuật toán vẫn tính mỗi đỉnh bị cô lập là một thành phần riêng biệt trong DFS và công thức cuối cùng giảm chính xác xuống 2^(n−1) + 1. 

Một trường hợp cạnh khác là đồ thị có một đỉnh. Quá trình truyền tải bắt đầu và kết thúc ngay lập tức với một thành phần. Công thức trở thành 2^(0) + 1 = 2, nghĩa là ban đầu chúng ta có thể chỉ định Đông hoặc Tây và năm thứ hai phải lặp lại. 

Trường hợp quan trọng cuối cùng là một chu trình lẻ. Trong DFS, cuối cùng chúng tôi cố gắng gán cho nút một màu xung đột với nhiệm vụ trước đó. Thuật toán ngay lập tức dừng lại và không thể đưa ra kết quả nếu không cố gắng đếm các thành phần, vì không tồn tại không gian gán hợp lệ nào cả.
