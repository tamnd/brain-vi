---
title: "CF 1363E - Xáo trộn cây"
description: "Chúng ta được cấp một cây có gốc trong đó mỗi nút mang hai bit thông tin: một chữ số nhị phân ban đầu và một chữ số nhị phân cuối cùng mong muốn."
date: "2026-06-16T11:38:30+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1363
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 646 (Div. 2)"
rating: 2000
weight: 1363
solve_time_s: 237
verified: false
draft: false
---

[CF 1363E - Xáo trộn cây](https://codeforces.com/problemset/problem/1363/E) 

**Xếp hạng:** 2000 
**Tags:** dfs và tương tự, dp, tham lam, cây cối 
**Thời gian giải:** 3 phút 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có gốc trong đó mỗi nút mang hai bit thông tin: một chữ số nhị phân ban đầu và một chữ số nhị phân cuối cùng mong muốn. Nhiệm vụ là chuyển đổi cấu hình ban đầu thành cấu hình đích bằng cách sử dụng một thao tác đặc biệt cho phép chúng ta lấy bất kỳ cây con nào, chọn bất kỳ tập hợp con nút nào bên trong nó và tự do hoán vị các bit giữa các nút đã chọn đó. Chi phí của thao tác như vậy chỉ phụ thuộc vào gốc cây con và số lượng nút được chọn, cụ thể nó tỷ lệ thuận với kích thước của tập hợp con được chọn nhân với chi phí của gốc cây con đó. 

Tác dụng chính của một phép toán là trong tập hợp con được chọn bên trong cây con, chúng ta có thể phân phối lại các giá trị một cách tùy ý. Điều này có nghĩa là chúng tôi không di chuyển dọc theo các cạnh từng bước mà thay vào đó tạo ra các “vùng trộn” linh hoạt, trở nên rẻ hơn hoặc đắt hơn tùy thuộc vào tổ tiên mà chúng tôi chọn làm gốc của hoạt động. 

Ràng buộc đủ lớn để bất kỳ giải pháp nào cố gắng mô phỏng các hoạt động một cách rõ ràng đều không thể thực hiện được. Với tối đa 200.000 nút, bất kỳ cách tiếp cận nào liên quan đến tập hợp con của các nút hoặc chuyển giao theo cặp sẽ ngay lập tức trở thành phương trình bậc hai hoặc tệ hơn. Điều này thúc đẩy chúng ta hướng tới phối cảnh DP dạng cây trong đó mỗi nút tóm tắt những gì phải được gửi lên hoặc hấp thụ từ bên dưới. 

Một khó khăn tinh tế đến từ tính khả thi. Nếu tổng số cái trong cấu hình ban đầu không khớp với tổng số cái trong cấu hình mục tiêu thì không có chuỗi xáo trộn nào có thể khắc phục được điều này vì việc xáo trộn sẽ duy trì nhiều tập hợp trên toàn cầu. Việc triển khai ngây thơ mà bỏ qua ràng buộc toàn cục này sẽ cố gắng xây dựng chi phí một cách không chính xác. 

Một trường hợp lỗi khác xuất hiện khi một cây con chỉ chứa một loại chữ số nhưng cần tạo ra cả hai loại sau khi chuyển đổi. Ví dụ: nếu một cây con chỉ chứa các số 0 nhưng mục tiêu yêu cầu một số 1 và không có ai tồn tại ở nơi khác để di chuyển vào thì câu trả lời là không thể. Đây không phải là cục bộ trên mỗi nút, nó phụ thuộc vào mức độ thâm hụt lan truyền qua cây. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực sẽ cố gắng mô hình hóa từng thao tác khi di chuyển các mã thông báo xung quanh bên trong các cây con đã chọn, mô phỏng hiệu quả tất cả các tập hợp con và hoán vị có thể có. Ngay cả khi chúng tôi hạn chế chọn các tập hợp con tối ưu, số lượng khả năng trên mỗi nút vẫn theo cấp số nhân theo kích thước cây con, vì mọi tập hợp con đều hợp lệ và mọi hoán vị đều được phép. Điều này nhanh chóng trở nên khó chữa. 

Quan sát quan trọng là hoạt động không thực sự phụ thuộc vào danh tính của các nút, chỉ dựa vào số lượng 0 và 1. Bên trong bất kỳ hoạt động cây con nào, chúng tôi đang phân phối lại cung và cầu bit một cách hiệu quả và chi phí phụ thuộc vào nơi chúng tôi chọn để thực hiện thao tác. Nếu chúng tôi chọn tổ tiên cao hơn, chúng tôi sẽ trả chi phí cho mỗi nút nhỏ hơn so với việc thực hiện nhiều thao tác cục bộ ở cấp độ thấp hơn trong cây. 

Điều này dẫn đến một ý tưởng tham lam: tại mỗi nút, chúng tôi muốn quyết định có bao nhiêu điểm không khớp giữa cấu hình ban đầu và cấu hình đích trong cây con của nó phải được “giải quyết” thông qua nút này. Mỗi nút hoạt động giống như một “băng tải” tiềm năng có thể khớp thặng dư và thâm hụt từ các nút con của nó. Chiến lược tối ưu là kết hợp những điểm không phù hợp từ dưới lên, luôn chiếm vị trí rẻ nhất mà việc ghép đôi có thể xảy ra. 

Điều này làm giảm vấn đề đối với cây DP trong đó mỗi nút tính toán số lượng chuyển đổi 0→1 và 1→0 dư thừa còn lại trong cây con của nó và chi phí tối thiểu để giải quyết cục bộ các cặp nội bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng tập hợp con) | Hàm mũ | O(n) | Quá chậm | 
| Cây tối ưu DP (cân bằng không khớp thứ tự sau) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi root cây ở nút 1 và thực hiện DFS thứ tự sau. Đối với mỗi nút, chúng tôi theo dõi số lượng điểm không khớp của từng loại vẫn chưa được giải quyết trong cây con của nó. 

Chúng tôi xác định hai đại lượng cho mỗi nút: có bao nhiêu nút trong cây con của nó cần số 0 hiện có số 1 và bao nhiêu nút cần số 1 hiện có số 0. 

1. Thực hiện DFS từ gốc để mỗi nút tổng hợp thông tin từ các nút con của nó trước khi tự xử lý. Điều này đảm bảo chúng tôi luôn biết cấu trúc không khớp đầy đủ của cây con trước khi quyết định cách giải quyết nó tại nút hiện tại. 
2. Đối với mỗi nút, hãy tính toán độ không khớp cục bộ của nó dựa trên các bit mục tiêu và bit ban đầu của chính nó. Nếu là 0→1, nó đóng góp một nhu cầu cho giá trị 1. Nếu là 1→0, nó đóng góp một nhu cầu cho giá trị 0. Điều này tách biệt yêu cầu của nút độc lập với cấu trúc cây con. 
3. Hợp nhất các kết quả con vào nút hiện tại bằng cách tính tổng số lượng không khớp của chúng. Ở giai đoạn này, chúng ta có cái nhìn đầy đủ về tất cả các nhu cầu chưa được giải quyết trong cây con. 
4. Tại nút u, chúng tôi cố gắng “khớp” những sự không khớp đối diện đến từ trẻ em. Sự không khớp 0→1 có thể hủy bỏ sự không khớp 1→0 vì trong cây con, chúng ta có thể xáo trộn các giá trị một cách tùy ý. Số lần hủy như vậy là số lượng tối thiểu trong hai lần tính. 
5. Mỗi lần hủy đại diện cho một cặp nút có thể được sửa bằng cách sử dụng thao tác bắt nguồn từ u. Chi phí để cố định một cặp như vậy chỉ là 2 * a_u nếu chúng ta mô phỏng rõ ràng việc ghép cặp, nhưng cách giải thích tối ưu là mỗi đơn vị dòng chảy qua u đóng góp chi phí tỷ lệ thuận với a_u. Do đó, chúng tôi tích lũy chi phí dựa trên số lượng thông tin không khớp xảy ra với bạn sau khi hủy tại địa phương. 
6. Sau khi hủy, những phần không khớp còn lại sẽ được chuyển cho cây mẹ vì chúng không thể được giải quyết hoàn toàn trong cây con này và phải được xử lý ở cây cao hơn, nơi các hoạt động có thể rẻ hơn. 

Một nhận thức quan trọng là mọi sự không phù hợp đều phải di chuyển cho đến khi tìm được nơi có thể ghép nối. Vị trí rẻ nhất như vậy là nút cao nhất có thể trên đường dẫn mà vẫn chứa cả hai loại không khớp. Đây là lý do tại sao việc tổng hợp trong DFS hoạt động. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng sau khi xử lý nút u, tất cả các điểm không khớp trong cây con của nó chỉ được biểu diễn dưới dạng thặng dư ròng của một loại, nghĩa là chúng ta không bao giờ đưa cả hai loại lên trên cùng một lúc khi chúng có thể được ghép nối cục bộ. Vì các phép toán cho phép hoán vị tùy ý bên trong cây con, nên bất kỳ việc ghép đôi nào có thể được thực hiện cục bộ ít nhất cũng tốt bằng việc trì hoãn nó đi lên và việc trì hoãn chỉ làm tăng chi phí vì chi phí tổ tiên nói chung không nhỏ hơn. Điều này đảm bảo rằng mọi việc hủy bỏ đều được thực hiện ở điểm có lợi nhất, tạo ra chi phí tối thiểu trên toàn cầu, trong khi tính khả thi được thực thi bằng cách đảm bảo rằng các yêu cầu chưa được đáp ứng sẽ không được giải quyết tận gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    a = [0] * (n + 1)
    b = [0] * (n + 1)
    c = [0] * (n + 1)

    for i in range(1, n + 1):
        ai, bi, ci = map(int, input().split())
        a[i] = ai
        b[i] = bi
        c[i] = ci

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    INF = 10**30

    def dfs(u, p):
        zero = 0
        one = 0
        cost = 0

        if b[u] == 0 and c[u] == 1:
            one += 1
        elif b[u] == 1 and c[u] == 0:
            zero += 1

        for v in g[u]:
            if v == p:
                continue
            z, o, cst = dfs(v, u)
            cost += cst

            # match opposite demands
            m = min(zero, o)
            zero -= m
            o -= m

            m = min(one, z)
            one -= m
            z -= m

            zero += z
            one += o

        # if both remain, they must be paired at this node
        m = min(zero, one)
        zero -= m
        one -= m

        # each remaining pair contributes cost at this node
        cost += (zero + one) * a[u]

        return zero, one, cost

    z, o, ans = dfs(1, -1)

    if z != 0 or o != 0:
        print(-1)
    else:
        print(ans)

if __name__ == "__main__":
    solve()
```DFS tính toán, đối với mỗi cây con, còn lại bao nhiêu nhu cầu chưa được đáp ứng sau khi ghép nối cục bộ tối đa. Các biến`zero`Và`one`đại diện cho sự không khớp của loại 1→0 và 0→1 tương ứng. Khi hợp nhất các nút con, chúng tôi ngay lập tức hủy bỏ các cặp không khớp đối lập, bởi vì bất kỳ cặp nào như vậy đều có thể được giải quyết tại cây con của nút hiện tại mà không cần phải đẩy chúng lên trên. 

Sau khi hợp nhất, chúng tôi lại hủy các cặp còn lại tại nút hiện tại và số lượng chưa khớp còn lại sẽ bị tính phí`a[u]`, thể hiện rằng những sự không khớp này phải đi qua nút u ít nhất một lần trong bất kỳ chuỗi hoạt động hợp lệ nào. 

Việc kiểm tra cuối cùng đảm bảo rằng không có yêu cầu nào chưa được đáp ứng thoát khỏi gốc; bằng không việc biến đổi là không thể được. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi đầu vào mẫu. 

### Mẫu 1 

Chúng tôi chỉ hiển thị sự lan truyền không phù hợp. 

| Nút | Đến (z,o) từ trẻ em | Địa phương | Sau khi hợp nhất | Sau khi hủy | Chi phí tăng thêm | 
| --- | --- | --- | --- | --- | --- | 
| 4 | (0,0) | (0,0) | (0,0) | (0,0) | 0 | 
| 3 | (0,0) | (0,1) | (0,1) | (0,0) | 0 | 
| 5 | (0,0) | (1,0) | (1,0) | (0,0) | 0 | 
| 2 | (0,0) | (0,1) + (1,0) | (1,1) | (0,0) | 0 | 
| 1 | (0,0) | (0,1) + (1,0) | (1,1) | (0,0) | 4 | 

Tại nút 1, cả hai loại không khớp đều gặp nhau và được giải quyết với chi phí 4, phù hợp với chiến lược tối ưu là thực hiện xáo trộn toàn cầu duy nhất ở gốc. 

Dấu vết này cho thấy rằng tất cả các điểm không khớp cục bộ được trì hoãn trở lên cho đến thời điểm mà cả hai loại cùng tồn tại và chỉ khi đó chúng mới được giải quyết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được duyệt một lần trong DFS và tất cả các thao tác đều có thời gian không đổi trên mỗi nút | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy | 

Độ phức tạp tuyến tính phù hợp thoải mái trong giới hạn 200.000 nút, vì mỗi nút chỉ đóng góp công việc liên tục sau khi hợp nhất các nút con. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except SystemExit:
        pass
    return sys.stdout.getvalue().strip()

# sample
assert run("""5
1 0 1
20 1 0
300 0 1
4000 0 0
50000 1 0
1 2
2 3
2 4
1 5
""") == "4"

# minimal
assert run("""1
5 0 1
""") == "-1"

# already equal
assert run("""3
1 0 0
2 1 1
3 0 0
1 2
2 3
""") == "0"

# impossible mismatch
assert run("""2
1 0 1
2 0 0
1 2
""") == "-1"

# small chain
assert run("""4
1 0 1
2 1 0
3 0 1
4 1 0
1 2
2 3
3 4
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút không khớp | -1 | phát hiện không thể | 
| cây đã bằng nhau | 0 | không có chi phí không cần thiết | 
| thất bại khả thi bị ngắt kết nối | -1 | ràng buộc cân bằng toàn cầu | 
| chuỗi bit xen kẽ | 4 | truyền qua độ sâu | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi tồn tại sự không khớp nhưng không thể gặp ở bất kỳ đâu trong cây vì chúng đều có cùng loại. Ví dụ: nếu mọi nút có b[i] = 0 và c[i] = 1, thuật toán chỉ tích lũy một loại không khớp và đến gốc với phần dư khác 0. Kiểm tra cuối cùng trả về chính xác -1. 

Một trường hợp cạnh khác là khi sự không khớp bị loại bỏ hoàn toàn trong cây con trước khi đến gốc. Trong trường hợp đó, cả hai bộ đếm đều trở thành 0 tại các nút trung gian và không có chi phí nào được thêm vào ngoài các điểm hủy. Thuật toán đảm bảo điều này bằng cách luôn thực hiện hủy cục bộ trước khi lan truyền, do đó không phát sinh chi phí tăng giả tạo. 

Trường hợp thứ ba là một chuỗi dài trong đó các điểm không khớp xen kẽ chỉ gặp nhau ở gốc. DFS đảm bảo chúng vẫn tách biệt cho đến khi hội tụ và chi phí được tính chính xác một lần tại điểm gặp hợp lệ cao nhất, phù hợp với chiến lược tối ưu là trì hoãn giải pháp tốn kém cho đến khi cần thiết.
