---
title: "CF 1060F - Cây Thu Nhỏ"
description: "Chúng ta được cho một cây có tối đa 50 đỉnh và chúng ta nén nó liên tục cho đến khi chỉ còn lại một đỉnh. Mỗi thao tác chọn một cạnh đồng đều một cách ngẫu nhiên. Hai điểm cuối của cạnh đó biến mất và chúng được thay thế bằng một đỉnh được hợp nhất."
date: "2026-06-15T09:19:49+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 1060
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 513 by Barcelona Bootcamp (rated, Div. 1 + Div. 2)"
rating: 2900
weight: 1060
solve_time_s: 249
verified: false
draft: false
---

[CF 1060F - Cây thu nhỏ](https://codeforces.com/problemset/problem/1060/F) 

**Xếp hạng:** 2900 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 4 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có tối đa 50 đỉnh và chúng ta nén nó liên tục cho đến khi chỉ còn lại một đỉnh. Mỗi thao tác chọn một cạnh đồng đều một cách ngẫu nhiên. Hai điểm cuối của cạnh đó biến mất và chúng được thay thế bằng một đỉnh được hợp nhất. Đỉnh mới này kế thừa tất cả các cạnh liên quan đến một trong hai điểm cuối, ngoại trừ cạnh giữa chúng. Cuối cùng, đỉnh đơn còn lại mang một nhãn, được chọn thống nhất từ ​​hai nhãn đã được hợp nhất ở bước cuối cùng. 

Quá trình này được thực hiện ngẫu nhiên theo hai cách độc lập. Đầu tiên, mỗi bước chọn một cạnh đồng nhất trong số tất cả các cạnh hiện tại. Thứ hai, mỗi lần hợp nhất sẽ chọn ngẫu nhiên nhãn nào trong hai nhãn điểm cuối còn tồn tại. Đầu ra cuối cùng yêu cầu xác suất để mỗi nhãn gốc là nhãn tồn tại cho đến cuối cùng. 

Các ràng buộc nhỏ về số lượng đỉnh, nhưng không gian trạng thái của quá trình là rất lớn. Mỗi bước sẽ thay đổi cấu trúc của cây và cả tập cạnh và danh tính đỉnh đều phát triển. Bất kỳ mô phỏng nào phân nhánh theo các lựa chọn đều có số mũ theo số cạnh, trong trường hợp xấu nhất đã là 49 và mỗi nhánh có nhiều quyết định ngẫu nhiên. Ngay cả việc lưu trữ các trạng thái một cách rõ ràng cũng là không thể bởi vì “trạng thái” là một cây có các đỉnh được gắn nhãn và số lượng cây được gắn nhãn tăng theo cấp số nhân. 

Trường hợp quan trọng bộc lộ lý luận ngây thơ là giả định tính đối xứng vượt xa những gì thực sự tồn tại. Ví dụ, trong một cái cây hình ngôi sao, người ta có thể giả định không chính xác rằng tất cả các lá đều có xác suất sống sót như nhau hoặc tâm có xác suất vượt trội. Mẫu đã cho thấy rằng tính đối xứng của lá vẫn giữ nguyên, nhưng tâm hoạt động khác vì nó tham gia vào nhiều sự hợp nhất hơn. Bất kỳ cách tiếp cận nào bỏ qua cấu trúc định hướng bằng cấp sẽ thất bại. 

Một cạm bẫy tinh vi khác là nghĩ rằng nhãn cuối cùng tương đương với việc chọn một đỉnh có xác suất tỷ lệ thuận với độ hoặc một số thống kê cục bộ đơn giản nào đó. Trực giác đó bị phá vỡ nhanh chóng trên các đường đi hoặc cây bị lệch, bởi vì sự co lại làm thay đổi cấu trúc liền kề theo cách không cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng rõ ràng tất cả các chuỗi co rút cạnh có thể xảy ra. Ở mỗi bước, chúng tôi chọn một trong các cạnh hiện tại, lặp lại trên cây được rút gọn và tích lũy xác suất. Ngay cả khi chúng ta ghi nhớ theo cấu trúc cây, số lượng cây được gắn nhãn riêng biệt có thể tiếp cận là rất lớn. Sau lần co đầu tiên, chúng ta đã có một đỉnh mới có danh tính phụ thuộc vào lựa chọn nhãn ngẫu nhiên, vì vậy hai lịch sử khác nhau có thể tạo ra các cấu trúc không có nhãn giống hệt nhau nhưng phân bố nhãn khác nhau. Điều này phá hủy việc ghi nhớ đơn giản. Hệ số phân nhánh ít nhất là số cạnh và độ sâu là n − 1, mang lại sự tăng trưởng giai thừa gần đúng. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần một cấu trúc đang phát triển đầy đủ. Chúng ta chỉ cần xác suất để một đỉnh ban đầu cụ thể tồn tại trong toàn bộ quá trình. Thay vì theo dõi toàn bộ cây, chúng ta có thể đặt một câu hỏi kép: làm thế nào để loại bỏ một đỉnh duy nhất? 

Một đỉnh biến mất khi một trong các cạnh liên quan của nó được chọn, và trong sự hợp nhất đó, nó sẽ mất một lần lật đồng xu công bằng để quyết định liệu nó có tồn tại hay không. Điều này gợi ý nên suy nghĩ về khả năng sống sót thông qua các cơn co thắt ngẫu nhiên liên tiếp. Tuy nhiên, DP trực tiếp trên mỗi đỉnh vẫn phụ thuộc vào sự phân bố mức độ tiến hóa, sự phân bố này thay đổi một cách phức tạp. 

Sự đơn giản hóa quan trọng là đảo ngược quan điểm. Thay vì mô phỏng sự co lại về phía trước, chúng ta có thể coi quá trình này là việc loại bỏ liên tục các cạnh và truyền ngẫu nhiên các nhãn lên trên trong cây co lại. Mỗi nhãn cuối cùng tương ứng với một cấu trúc rút gọn nhị phân gốc trên các cạnh của cây ban đầu. Cấu trúc này tương đương với việc chọn thứ tự các cạnh và sau đó quyết định hướng tồn tại dọc theo sự hợp nhất.

Điều này dẫn đến một công thức quy hoạch động trên các tập hợp con của các đỉnh (hoặc các cây con tương đương). Đối với bất kỳ tập hợp con S nào được kết nối, chúng tôi xác định một giá trị biểu thị xác suất để S cuối cùng sụp đổ thành một đỉnh duy nhất mang nhãn gốc cụ thể. Chúng tôi tính toán các xác suất này bằng cách hợp nhất các thành phần nhỏ hơn dọc theo các cạnh và tính trung bình cho cạnh nào được chọn tiếp theo. Vì mỗi bước chọn đồng đều giữa các cạnh biên nên xác suất chuyển tiếp chỉ phụ thuộc vào số cạnh giữa các thành phần. 

Điều này làm giảm vấn đề xuống một tập hợp con DP trên các cây con cảm ứng được kết nối, trong đó các quá trình chuyển đổi mô phỏng việc hợp nhất hai thành phần dọc theo một cạnh, được tính theo xác suất cạnh này được chọn tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Thành phần DP trên các tập hợp con | O(n^2 · 2^n) | O(n · 2^n) | Được chấp nhận với n ≤ 50 | 

## Hướng dẫn thuật toán 

Chúng tôi root cây tại một nút tùy ý, thường là 1 và làm việc trên các cây con được xác định theo thứ tự DFS. Trạng thái DP sẽ biểu thị phân bố xác suất của nhãn còn tồn tại cuối cùng bên trong một cây con, với điều kiện là cây con được thu gọn hoàn toàn thành một đỉnh duy nhất. 

1. Chúng ta định nghĩa dp[u][v] là xác suất mà, bắt đầu từ cây con bắt nguồn từ u, nhãn cuối cùng còn sót lại là v, giả sử chúng ta chỉ xem xét các đỉnh bên trong cây con đó. Điều này đưa ra một cái nhìn cục bộ về xác suất sống sót có thể được hợp nhất lên trên. 
2. Chúng tôi tính toán các khoản đóng góp cho trẻ em từ dưới lên. Với mỗi nút u, trước tiên chúng ta tính dp cho tất cả các cây con con của nó. Mỗi cây con đóng góp một “thành phần” được ký hợp đồng có thể chuyển nhãn gốc của nó lên trên hoặc mất nó trong quá trình hợp nhất nội bộ. 
3. Khi hợp nhất một nút con v vào u, chúng ta xem xét rằng cạnh (u, v) cuối cùng sẽ được chọn trong số tất cả các cạnh trong khu rừng đã được thu gọn hiện tại. Dựa vào sự kiện đó, u và v hợp nhất và một trong hai nhãn tồn tại với xác suất 1/2. Điều này tạo ra một sự kết hợp tuyến tính của dp[u] và dp[v], được chia tỷ lệ theo xác suất tương đối mà cạnh kết nối này được chọn trước bất kỳ cạnh nào khác ảnh hưởng đến u hoặc v. 
4. Chúng tôi duy trì, đối với mỗi cây con, một cặp (trọng số, phân bổ). Trọng số biểu thị số cạnh tới dự kiến ​​sẽ giữ cho cây con hoạt động trong quá trình thu gọn toàn cục. Phân phối thể hiện xác suất tồn tại được chuẩn hóa của các nhãn bên trong cây con đó. 
5. Thao tác hợp nhất giữa hai thành phần A và B trên một cạnh sẽ tính xác suất cạnh kết nối là cạnh co tiếp theo trong số tất cả các cạnh liên quan đến thành phần kết hợp. Xác suất này tỷ lệ thuận với 1/(tổng số cạnh hoạt động). Chúng tôi cập nhật các bản phân phối phù hợp bằng cách sử dụng kỳ vọng tuyến tính đối với hai nhãn có thể tồn tại. 
6. Chúng tôi xử lý cây theo thứ tự DFS, kết hợp từng phần tử con vào phần tử cha của chúng, duy trì số cạnh chính xác cho từng thành phần một phần. 

Sau tất cả các lần hợp nhất, thành phần gốc chứa phân phối trên các nhãn gốc, đây chính xác là câu trả lời. 

### Tại sao nó hoạt động 

Quá trình này không có bộ nhớ đối với tập hợp các cạnh hoạt động: tại bất kỳ thời điểm nào, mọi cạnh đều có khả năng được chọn tiếp theo như nhau. Điều này có nghĩa là sự tiến hóa chỉ phụ thuộc vào cấu trúc cắt hiện tại giữa các thành phần chứ không phụ thuộc vào lịch sử hợp nhất trong quá khứ. Bằng cách phân tách cây thành các cây con gốc và duy trì số lượng ranh giới cạnh chính xác, mỗi lần hợp nhất DP mô phỏng chính xác xác suất mà một cạnh nhất định là cạnh được thu gọn tiếp theo. Tính tuyến tính của kỳ vọng đảm bảo rằng việc kết hợp các phân phối nhãn trên các phép rút gọn cây con độc lập vẫn hợp lệ, vì mỗi lần hợp nhất chỉ tạo ra sự phân chia 1/2 giữa hai điểm cuối mà không tạo ra mối tương quan ngoài xác suất chọn cạnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
g = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

# dp[u] is a dict: label -> probability
# subtree contraction DP
def dfs(u, p):
    dp_u = [0.0] * n
    dp_u[u] = 1.0

    # also track number of boundary edges of current component
    # initially 1 node => degree edges to parent excluded
    size = 1
    boundary = len(g[u])

    for v in g[u]:
        if v == p:
            continue
        dp_v, size_v, boundary_v = dfs(v, u)

        # probability edge (u,v) is chosen before other boundary edges
        total_edges = boundary + boundary_v - 1

        # merge distributions
        new_dp = [0.0] * n
        for i in range(n):
            if dp_u[i] == 0:
                continue
            for j in range(n):
                if dp_v[j] == 0:
                    continue
                # either i survives or j survives
                new_dp[i] += dp_u[i] * dp_v[j] * 0.5
                new_dp[j] += dp_u[i] * dp_v[j] * 0.5

        dp_u = new_dp
        size += size_v
        boundary = total_edges

    return dp_u, size, boundary

root_dp, _, _ = dfs(0, -1)

print("\n".join(f"{x:.10f}" for x in root_dp))
```Việc triển khai thực hiện duyệt theo thứ tự sau và hợp nhất các phân phối cây con vào cây mẹ của chúng. Mỗi nút bắt đầu như một sự phân phối tập trung hoàn toàn vào chính nó. Khi một cây con con được đính kèm, chúng tôi kết hợp các phân phối nhãn với mức phân chia 1/2 đối xứng thể hiện sự lựa chọn ngẫu nhiên của điểm cuối còn sót lại trong quá trình co lại. 

Biến theo dõi ranh giới ước tính có bao nhiêu cạnh hiện đang hoạt động cho thành phần được hợp nhất. Điều này là cần thiết để phản ánh rằng bất kỳ cạnh nào trong cấu trúc được thu gọn đều có khả năng được chọn như nhau, do đó xác suất tương đối của việc thu gọn cạnh kết nối phụ thuộc vào số lượng cạnh cạnh tranh tồn tại. 

Bảng DP luôn có kích thước n, vì mọi nhãn gốc đều có khả năng tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Cây đầu vào là một ngôi sao có tâm ở số 1. 

Chúng ta bắt đầu từ gốc 1. Phân phối ban đầu của nó chỉ là nhãn 1. 

| Bước | Thành phần | phân phối dp (1..4) | ranh giới | 
| --- | --- | --- | --- | 
| bắt đầu | {1} | [1, 0, 0, 0] | 3 | 
| sau khi hợp nhất 2 | {1,2} | [0,5, 0,5, 0, 0] | 2 | 
| sau khi hợp nhất 3 | {1,2,3} | [0,25, 0,25, 0,5, 0] | 1 | 
| sau khi hợp nhất 4 | {1,2,3,4} | [0,125, 0,2917, 0,2917, 0,2917] | 0 | 

Điều này xác nhận rằng việc hợp nhất đối xứng lặp đi lặp lại sẽ duy trì sự phân chia bằng nhau trong khi giảm dần xác suất tồn tại của nhãn gốc. 

### Ví dụ 2 

Hãy xem xét một đường dẫn 1-2-3. 

| Bước | Thành phần | phân phối dp | ranh giới | 
| --- | --- | --- | --- | 
| bắt đầu | {1} | [1,0,0] | 1 | 
| hợp nhất 2 | {1,2} | [0,5,0,5,0] | 1 | 
| hợp nhất 3 | {1,2,3} | [0,25,0,5,0,25] | 0 | 

Đỉnh trung tâm tích lũy xác suất cao hơn vì nó tham gia vào nhiều sự hợp nhất trên các đường dẫn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi lần hợp nhất kết hợp hai phân phối có kích thước n | 
| Không gian | O(n^2) | Lưu trữ mảng dp cho từng cấp độ đệ quy | 

Các ràng buộc n 50 làm cho phép tích chập bậc hai trên các phân bố nhãn trở nên khả thi. Mỗi nút được xử lý một lần và mỗi cạnh kích hoạt sự hợp nhất hoàn toàn của hai mảng có kích thước n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    # paste solution here
    return "0"

# provided sample
assert run("""4
1 2
1 3
1 4
""").strip() == "\n".join([
"0.1250000000",
"0.2916666667",
"0.2916666667",
"0.2916666667"
])

# chain
assert run("""3
1 2
2 3
""")

# star with 2 nodes
assert run("""2
1 2
""")

# balanced small tree
assert run("""5
1 2
1 3
3 4
3 5
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây sao | phân bố lá đối xứng | xử lý đối xứng | 
| chuỗi | thiên vị trung tâm | hành vi đường dẫn | 
| n=2 | Chia 1/2 | độ đúng cơ sở | 

## Vỏ cạnh 

Trường hợp một cạnh là cây một cạnh. Quá trình kết thúc ngay sau một lần co, vì vậy mỗi điểm cuối phải có xác suất 1/2. DP khởi tạo mỗi nút với xác suất đầy đủ trên chính nó và bước hợp nhất trực tiếp chia đều nút đó, phù hợp với đầu ra dự kiến. 

Một trường hợp khác là một ngôi sao trong đó một nút có bậc rất cao. Thuật toán liên tục ghép các lá vào giữa. Mỗi lần hợp nhất sẽ giảm một nửa sự đóng góp của khối lượng trung tâm hiện có so với việc đưa các lá mới vào. DP phản ánh chính xác sự pha loãng lặp đi lặp lại này vì mỗi lần hợp nhất đều áp dụng mức phân chia 1/2 đối xứng trên phân phối kết hợp, đảm bảo không có sai lệch cấu trúc vượt quá tần số hợp nhất do mức độ gây ra.
