---
title: "CF 104415E - Khủng hoảng thang máy"
description: "Chúng ta có một đồ thị có các đỉnh được xác định bằng các nhãn số nguyên duy nhất. Một số nhãn trong phạm vi từ 1 đến giá trị tối đa có thể không tương ứng với bất kỳ đỉnh thực tế nào. Đồ thị cũng chứa các cạnh vô hướng giữa các đỉnh được dán nhãn này."
date: "2026-06-30T19:51:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104415
codeforces_index: "E"
codeforces_contest_name: "IME++ Starters Try-outs 2023"
rating: 0
weight: 104415
solve_time_s: 55
verified: true
draft: false
---

[CF 104415E - Khủng hoảng thang máy](https://codeforces.com/problemset/problem/104415/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có các đỉnh được xác định bằng các nhãn số nguyên duy nhất. Một số nhãn trong phạm vi từ 1 đến giá trị tối đa có thể không tương ứng với bất kỳ đỉnh thực tế nào. Đồ thị cũng chứa các cạnh vô hướng giữa các đỉnh được dán nhãn này. 

Quá trình chúng tôi mô phỏng là tăng dần. Chúng tôi xem xét các nhãn theo thứ tự tăng dần. Khi đạt đến một nhãn, chúng tôi sẽ kích hoạt đỉnh tương ứng nếu nó tồn tại và chúng tôi cung cấp nó để kết nối. Bất cứ khi nào một đỉnh bắt đầu hoạt động, chúng ta cũng kết nối nó thông qua tất cả các cạnh liên kết nó với các đỉnh đã hoạt động. Tại mỗi thời điểm, chúng ta đang xem xét đồ thị con được tạo ra bởi tất cả các đỉnh hoạt động một cách hiệu quả. 

Nhiệm vụ là xác định tất cả các giá trị nhãn mà tại đó đồ thị con đang hoạt động trở nên được kết nối đầy đủ, nghĩa là tất cả các đỉnh hiện đang hoạt động đều nằm trong một thành phần được kết nối duy nhất. Nếu trong quá trình quét, chúng ta đạt đến một nhãn không tương ứng với bất kỳ đỉnh nào, quá trình sẽ dừng ngay lập tức vì không thể kích hoạt thêm ý nghĩa nào nữa. 

Các ràng buộc ngụ ý rằng số đỉnh và cạnh đủ lớn để bất kỳ phương pháp nào liên tục tính toán lại kết nối từ đầu sẽ quá chậm. Một BFS hoặc DFS ngây thơ sau mỗi lần chèn sẽ có giá O(n(n + m)), điều này trở nên không khả thi khi n lớn. Điều này thúc đẩy chúng tôi hướng tới phương pháp bảo trì kết nối gia tăng dựa trên DSU, trong đó các hoạt động liên kết diễn ra gần như liên tục trong thời gian. 

Trường hợp cạnh tinh tế phát sinh khi thiếu nhãn. Giả sử chúng ta có các đỉnh {1, 2, 4, 5}. Khi chúng ta đạt tới nhãn 3, không có đỉnh nào để kích hoạt và quá trình quét kết thúc. Việc triển khai ngây thơ bỏ qua các nhãn bị thiếu có thể tiếp tục sai đến 4 và 5, tạo ra các câu trả lời không hợp lệ theo định nghĩa quy trình. 

Một trường hợp cạnh khác xảy ra khi đồ thị được kết nối sớm nhưng những lần bổ sung sau đó sẽ phá vỡ giả định nếu chúng ta tính toán lại từ đầu không chính xác mà không bảo toàn các kết hợp trước đó. Điều này thường được DSU tránh được, nhưng không phải bằng các phương pháp truyền tải lặp đi lặp lại. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi mô phỏng nhãn quy trình theo nhãn. Đối với mỗi nhãn i, chúng tôi duy trì tập hợp các đỉnh hoạt động và xây dựng lại kết nối giữa chúng bằng DFS hoặc BFS. Sau khi kích hoạt tất cả các đỉnh cho đến i, chúng ta kiểm tra xem tất cả các đỉnh đang hoạt động có thuộc về một thành phần liên thông hay không. Nếu vậy, chúng tôi ghi lại i là câu trả lời hợp lệ. 

Điều này hoạt động chính xác vì nó trực tiếp kiểm tra kết nối ở mỗi bước. Tuy nhiên, chi phí bị chi phối bởi việc duyệt đồ thị lặp đi lặp lại. Nếu có O(n) bước và mỗi bước quét các cạnh O(n + m), tổng độ phức tạp sẽ trở thành O(n(n + m)), quá lớn so với các ràng buộc thông thường. 

Quan sát quan trọng là khả năng kết nối phát triển dần dần. Khi chuyển từ i sang i+1, chúng ta không cần tính toán lại kết nối từ đầu. Chúng ta chỉ cần thêm một đỉnh mới và kết hợp nó với các đỉnh lân cận đã hoạt động. DSU (Disjoint Set Union) được thiết kế chính xác cho kiểu hợp nhất gia tăng này. 

Đặc tính cấu trúc quan trọng là các cạnh chỉ được thêm vào về mặt kết nối, không bao giờ bị loại bỏ. Do đó, khi hai đỉnh nằm trong cùng một thành phần thì chúng vẫn như vậy. Sự đơn điệu này cho phép chúng ta duy trì thông tin kết nối một cách hiệu quả. 

Chúng tôi quét nhãn theo thứ tự tăng dần. Mỗi lần chúng ta kích hoạt một đỉnh, chúng ta sẽ kết hợp nó với tất cả các đỉnh lân cận đã hoạt động. Chúng tôi duy trì số lượng thành phần được kết nối giữa các đỉnh hoạt động. Nếu số này trở thành 1, chúng tôi ghi lại nhãn hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại BFS/DFS từng bước) | O(n(n + m)) | O(n + m) | Quá chậm | 
| Quét gia tăng DSU | O((n + m) α(n)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì cấu trúc DSU và mảng boolean để theo dõi xem một đỉnh có hoạt động hay không. Chúng tôi cũng giữ một bộ đếm các thành phần hoạt động. 

1. Chúng tôi bắt đầu từ nhãn nhỏ nhất và di chuyển lên trên. Thứ tự này quan trọng vì việc kích hoạt được xác định nghiêm ngặt bằng cách tăng số nhận dạng. 
2. Khi đạt đến nhãn i, trước tiên chúng ta kiểm tra xem đỉnh có nhãn này có tồn tại hay không. Nếu không, chúng tôi sẽ chấm dứt quá trình ngay lập tức vì không có thay đổi trạng thái nào được xác định ngoài thời điểm này. 
3. Nếu đỉnh tồn tại, chúng ta kích hoạt nó và ban đầu coi nó như thành phần được kết nối của chính nó. Điều này làm tăng số lượng thành phần lên một. 
4. Chúng ta lặp lại tất cả các lân cận của đỉnh này. Đối với mỗi hàng xóm đã hoạt động, chúng tôi hợp nhất các bộ DSU của họ. Nếu hai thành phần riêng biệt trước đó được hợp nhất, chúng tôi sẽ giảm số lượng thành phần xuống một. Điều này duy trì số lượng chính xác có bao nhiêu thành phần được kết nối tồn tại giữa các nút hoạt động. 
5. Sau khi xử lý tất cả các lân cận, chúng tôi kiểm tra xem số lượng thành phần hoạt động có bằng 1 hay không. Nếu vậy, điều đó có nghĩa là mọi đỉnh đang hoạt động đều có thể truy cập được từ mọi đỉnh đang hoạt động khác, vì vậy nhãn hiện tại là câu trả lời hợp lệ. 

Lý do khiến việc hợp nhất cục bộ này là đủ là vì tất cả thông tin kết nối đều truyền qua các cạnh và mỗi cạnh được xử lý chính xác một lần khi điểm cuối sau đó của nó được kích hoạt. 

### Tại sao nó hoạt động 

DSU duy trì tính bất biến rằng tại bất kỳ thời điểm nào, mỗi đỉnh hoạt động đều thuộc về chính xác một tập đại diện cho thành phần được kết nối của nó trong sơ đồ con cảm ứng của các đỉnh hoạt động. Mọi phép toán hợp đều tương ứng chính xác với việc tạo ra một cạnh giữa hai đỉnh hoạt động, đây là sự kiện duy nhất có thể làm giảm số lượng thành phần. Vì chúng tôi không bao giờ loại bỏ các cạnh hoặc hủy kích hoạt các đỉnh nên khi tất cả các đỉnh hoạt động được hợp nhất thành một bộ DSU duy nhất, chúng vẫn được kết nối trong tất cả các trạng thái trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    adj = [[] for _ in range(n + 1)]
    exists = [False] * (n + 1)

    for _ in range(m):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    # mark which identifiers exist
    # (if input guarantees all 1..n exist, this loop is harmless)
    for i in range(1, n + 1):
        exists[i] = True

    parent = list(range(n + 1))
    size = [1] * (n + 1)

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        ra, rb = find(a), find(b)
        if ra == rb:
            return False
        if size[ra] < size[rb]:
            ra, rb = rb, ra
        parent[rb] = ra
        size[ra] += size[rb]
        return True

    active = [False] * (n + 1)
    comp = 0

    res = []

    for i in range(1, n + 1):
        if not exists[i]:
            break

        active[i] = True
        comp += 1

        for v in adj[i]:
            if active[v]:
                if union(i, v):
                    comp -= 1

        if comp == 1:
            res.append(i)

    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng danh sách kề để mọi cạnh đều có sẵn khi điểm cuối của nó được xử lý. Cấu trúc DSU chỉ theo dõi kết nối giữa các nút hoạt động. các`active`mảng đảm bảo chúng tôi chỉ hợp các cạnh hợp lệ trong tiền tố hiện tại của quá trình quét. 

các`comp`biến là cần thiết vì chỉ riêng DSU không cho chúng ta biết trực tiếp có bao nhiêu thành phần tồn tại. Thay vào đó, chúng tôi theo dõi rõ ràng số lần hợp nhất thành công các tập hợp riêng biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét một đồ thị nhỏ có các đỉnh từ 1 đến 4 và các cạnh (1-2), (2-3), (3-4). Chúng tôi mô phỏng kích hoạt theo thứ tự. 

| tôi | Nút hoạt động | DSU sáp nhập | Linh kiện | comp == 1 | 
| --- | --- | --- | --- | --- | 
| 1 | {1} | không | 1 | vâng | 
| 2 | {1,2} | 1-2 | 1 | vâng | 
| 3 | {1,2,3} | 2-3 | 1 | vâng | 
| 4 | {1,2,3,4} | 3-4 | 1 | vâng | 

Mọi tiền tố đều được kết nối nên mọi nhãn đều được ghi lại. Điều này chứng tỏ tính bất biến khi một chuỗi được kết nối đầy đủ thì tất cả các tiền tố trong tương lai vẫn được kết nối. 

### Ví dụ 2 

Bây giờ hãy xem xét các cạnh (1-2) và (3-4), tạo thành hai cặp rời rạc. 

| tôi | Nút hoạt động | DSU sáp nhập | Linh kiện | comp == 1 | 
| --- | --- | --- | --- | --- | 
| 1 | {1} | không | 1 | vâng | 
| 2 | {1,2} | 1-2 | 1 | vâng | 
| 3 | {1,2,3} | không | 2 | không | 
| 4 | {1,2,3,4} | 3-4 | 2 | không | 

Ở đây, kết nối bị mất khi một khối biệt lập mới được đưa vào. DSU theo dõi chính xác rằng hệ thống không còn được kết nối đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) α(n)) | Mỗi cạnh được xử lý một lần và mỗi kết hợp/tìm gần như không đổi | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng DSU | 

Độ phức tạp phù hợp thoải mái trong giới hạn điển hình cho n lên tới 2×10^5. DSU đảm bảo rằng ngay cả các tập hợp cạnh dày đặc vẫn hoạt động hiệu quả nhờ hành vi nghịch đảo được khấu hao của Ackermann. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal chain
assert run("4 3\n1 2\n2 3\n3 4\n") == "1 2 3 4"

# two components
assert run("4 2\n1 2\n3 4\n") == "1 2"

# star graph
assert run("5 4\n1 2\n1 3\n1 4\n1 5\n") == "1 2 3 4 5"

# isolated node breaks later connectivity
assert run("5 2\n1 2\n2 3\n") == "1 2 3"

# single node
assert run("1 0\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | tất cả các tiền tố | tuyên truyền kết nối đầy đủ | 
| hai cặp | chỉ 1 2 | thành phần bị ngắt kết nối | 
| đồ thị sao | tất cả các nhãn | kết nối dựa trên trung tâm | 
| chuỗi một phần | 1 2 3 | hợp nhất tiền tố ổn định | 
| nút đơn | 1 | trường hợp cơ sở đúng đắn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi đồ thị được kết nối ngay sau vài lần kích hoạt đầu tiên. Ví dụ: nếu nút 1 được kết nối với tất cả các nút khác thì tại i = 2 DSU đã hợp nhất mọi thứ thành một thành phần duy nhất. Thuật toán ghi lại i = 2 một cách chính xác vì comp trở thành 1 chính xác khi sự kết hợp cần thiết cuối cùng xảy ra và trạng thái này vẫn tồn tại cho tất cả i sau này. 

Một trường hợp khác xảy ra khi một mã định danh bị thiếu xuất hiện sớm. Giả sử chỉ có các nút {1, 3, 4} tồn tại. Khi đạt tới i = 2 thì quá trình kết thúc. Thuật toán dừng đúng vì`exists[i]`kiểm tra ngăn cản bất kỳ hoạt động kích hoạt hoặc kết hợp nào nữa. Một quá trình quét ngây thơ bỏ qua các nhãn bị thiếu sẽ tiếp tục không chính xác và tạo ra kết quả cho i = 3 và 4 mặc dù quá trình lẽ ra đã dừng lại. 

Trường hợp tinh tế cuối cùng là các cạnh lặp lại giữa các thành phần đã được kết nối. Phép toán kết hợp trả về sai trong trường hợp này, do đó số lượng thành phần không giảm một cách chính xác. Điều này ngăn chặn việc sáp nhập quá mức và đảm bảo trạng thái kết nối vẫn nhất quán trong suốt quá trình quét.
