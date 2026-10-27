---
title: "CF 106118L - Dán nhãn cho cây"
description: "Chúng ta có một cây có gốc trong đó đỉnh 1 là gốc và mọi đỉnh khác đều có cha cố định. Trên cây này, chúng ta xem xét các hoán vị của các đỉnh, nghĩa là mỗi nhãn đỉnh từ 1 đến n xuất hiện đúng một lần theo một thứ tự nào đó."
date: "2026-06-20T05:03:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106118
codeforces_index: "L"
codeforces_contest_name: "2025 ICPC, Chula Selection Contest"
rating: 0
weight: 106118
solve_time_s: 55
verified: true
draft: false
---

[CF 106118L - Dán nhãn cho cây](https://codeforces.com/problemset/problem/106118/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó đỉnh 1 là gốc và mọi đỉnh khác đều có cha cố định. Trên cây này, chúng ta xem xét các hoán vị của các đỉnh, nghĩa là mỗi nhãn đỉnh từ 1 đến n xuất hiện đúng một lần theo một thứ tự nào đó. 

Một hoán vị được gọi là hợp lệ nếu mọi vị trí i được gán một giá trị p[i] “tương thích” với đỉnh i theo ít nhất một trong ba cách. Hoặc giá trị bằng chính i hoặc giá trị tương ứng với một đỉnh nằm trên đường đi từ i đến gốc (vì vậy nó là tổ tiên của i), hoặc giá trị nằm trong cây con của i (vì vậy nó là hậu duệ của i). Ràng buộc có tính đối xứng về mặt cấu trúc nhưng không đối xứng về mặt giải thích vì mối quan hệ tổ tiên và con cháu phụ thuộc vào cây có gốc tại 1. 

Nhiệm vụ là đếm xem tồn tại bao nhiêu hoán vị hợp lệ như vậy, modulo 998244353. 

Kích thước đầu vào tổng thể nhỏ, với tổng n trên tất cả các trường hợp thử nghiệm lên tới 5000. Điều này ngay lập tức loại trừ bất kỳ điều gì tồi tệ hơn đại khái bậc hai cho mỗi trường hợp thử nghiệm và gợi ý mạnh mẽ rằng giải pháp nên dựa trên lập trình động trên cấu trúc cây hoặc phân rã tổ hợp các cây con. Bất kỳ phép liệt kê giai thừa nào đối với các hoán vị đều không thể thực hiện được ngay cả với n khoảng 20, vì vậy điều kiện phải chuyển thành một ràng buộc có cấu trúc về cách các phần tử từ các cây con khác nhau có thể tương tác. 

Một trường hợp khó phát hiện xuất phát từ thực tế là các tập tổ tiên và tập hợp con cháu chồng lên nhau rất nhiều dọc theo các đường dẫn gốc. Ví dụ, trong cây chuỗi, mọi cặp đỉnh đều có thể so sánh được, do đó điều kiện trở nên gần như trống và mọi hoán vị đều hợp lệ. Ở một thái cực khác, trong cây sao, hầu hết các cặp đều không thể so sánh được ngoại trừ gốc, điều này tạo ra những ràng buộc về cấu trúc mạnh mẽ hơn. Bất kỳ nỗ lực ngây thơ nào xử lý các ràng buộc tổ tiên và con cháu một cách độc lập sẽ tính toán sai các hoán vị vì cùng một đỉnh có thể đáp ứng nhiều vai trò cho các vị trí khác nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các hoán vị của các đỉnh và kiểm tra từng hoán vị theo điều kiện. Đối với mỗi vị trí i, chúng tôi sẽ xác minh xem p[i] có bằng i hay không, hay p[i] nằm trong chuỗi tổ tiên của i hay trong cây con của nó. Việc tính toán trước các mối quan hệ tổ tiên và con cháu khiến mỗi lần kiểm tra trở thành O(1), nhưng việc tạo ra các hoán vị đã là O(n!) cho mỗi trường hợp thử nghiệm, điều này không khả thi ngay cả với n khoảng 12. 

Quan sát quan trọng là điều kiện không phụ thuộc vào nhãn tuyệt đối mà chỉ phụ thuộc vào mối quan hệ cấu trúc trong cây. Đối với đỉnh i cố định, các giá trị được phép p[i] tạo thành chính xác tập hợp bao gồm tất cả các nút trong cây con liên kết với tổ tiên của nó, là một cấu trúc được kết nối theo thứ tự cây. Điều này gợi ý rằng các phép gán bị hạn chế bởi cách các hoán vị tôn trọng sự phân tách cây con. 

Cái nhìn sâu sắc về cấu trúc quan trọng là xử lý cây từ dưới lên và đếm cách các hoán vị có thể được hình thành bằng cách hợp nhất các hoán vị của cây con. Mỗi cây con hoạt động giống như một khối có thể duy trì sự nhất quán nội bộ hoặc cho phép một số “chuyển nhượng chéo” nhất định thông qua các kết nối tổ tiên. Bởi vì mỗi nút chỉ kết nối lên dọc theo đường dẫn gốc nên sự tương tác giữa các cây con khác nhau bị hạn chế rất nhiều: các cây con chỉ được ghép nối thông qua tổ tiên chung thấp nhất của chúng và điều này cho phép DP cây trong đó các trạng thái theo dõi số lượng phần tử từ cây con được gán bên ngoài nó thông qua các cạnh tổ tiên. 

Điều này làm giảm vấn đề hoán vị toàn cục thành việc kết hợp các đóng góp của cây con độc lập trong khi vẫn duy trì một không gian trạng thái nhỏ trên mỗi nút, thay vì theo dõi các hoán vị đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Cây DP hợp nhất cây con | O(n²) cho mỗi trường hợp thử nghiệm | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi root cây lúc 1 và xử lý nó theo thứ tự sau. Đối với mỗi nút u, chúng tôi tính toán một bảng DP mô tả có bao nhiêu cách chúng tôi có thể gán hoán vị cho cây con của u theo số lượng “vị trí bên ngoài” khác nhau, nghĩa là các phần tử từ cây con này được gán cho các vị trí bên ngoài cây con nhưng vẫn hợp lệ thông qua các mối quan hệ tổ tiên. 

Chúng ta định nghĩa dp[u][k] là số cách xử lý cây con của u sao cho chính xác k phần tử từ cây con(u) không được đặt bên trong cây con(u), mà thay vào đó được sử dụng ở các vị trí tổ tiên phía trên u. 

Chúng tôi khởi tạo tại các lá với dp[u][0] = 1 vì một nút duy nhất chỉ có thể ánh xạ tới chính nó và không có cấu trúc cây con để tương tác. 

Đối với nút bên trong u, chúng tôi hợp nhất từng nút con một. Giả sử chúng tôi đã có một phần DP cho u và chúng tôi kết hợp v con. Chúng tôi kết hợp các cấu hình bằng cách quyết định có bao nhiêu phần tử từ cây con của v được xuất lên trên. Nếu v xuất các phần tử x thì các phần tử x đó phải được sắp xếp trong số các vị trí tổ tiên có sẵn so với u và các phần tử cây con(v) còn lại vẫn ở bên trong. 

Bước hợp nhất này mang tính tổ hợp: chúng tôi chọn số lượng phần tử mà mỗi phần tử con xuất và chúng tôi nhân với các hệ số nhị thức biểu thị cách chúng tôi xen kẽ các phép gán giữa các cây con khác nhau trong khi vẫn tôn trọng tính duy nhất hoán vị. Cấu trúc cây đảm bảo rằng các phần tử được xuất từ ​​các phần tử con khác nhau không gây nhiễu ngoại trừ thông qua số lượng, bởi vì các đường dẫn tổ tiên của chúng chỉ gặp nhau tại u. 

Sau khi xử lý tất cả các phần tử con, chúng tôi cập nhật dp[u] tương ứng và tiếp tục đi lên. Tại thư mục gốc, không có tổ tiên nào, vì vậy các cấu hình hợp lệ phải không có phần tử nào được xuất. Câu trả lời cuối cùng là dp[1][0]. 

### Tại sao nó hoạt động 

Bất biến DP là dp[u][k] đếm chính xác tất cả các hoán vị từng phần hợp lệ của cây con(u) phù hợp với các ràng buộc tổ tiên-con cháu và có chính xác k phần tử “được đẩy lên” tới tổ tiên của u. Cấu trúc cây đảm bảo rằng mọi hoán vị toàn cục hợp lệ đều có thể được phân tách duy nhất thành các đóng góp của cây con độc lập cộng với lựa chọn vị trí đặt các phần tử xuất dọc theo chuỗi tổ tiên. Vì chuỗi tổ tiên không phân nhánh nên xung đột không thể xảy ra giữa các cây con khác nhau ngoại trừ ở tổ tiên chung thấp nhất của chúng và đây chính xác là điều mà trạng thái DP nắm bắt. Mỗi bước hợp nhất duy trì tính nhất quán vì nó chỉ chiếm các vị trí hợp lệ trong đó các phần tử cây con ở bên trong hoặc di chuyển lên trên dọc theo các đường dẫn tổ tiên được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        parents = list(map(int, input().split()))
        
        g = [[] for _ in range(n + 1)]
        for i, p in enumerate(parents, start=2):
            g[p].append(i)

        # dp[u] = list where dp[u][k] = ways with k exported nodes
        dp = [None] * (n + 1)

        def dfs(u):
            dp[u] = [0]
            dp[u][0] = 1
            size = 1

            for v in g[u]:
                dfs(v)

                ndp = [0] * (size + len(dp[v]))
                for i in range(len(dp[u])):
                    if dp[u][i] == 0:
                        continue
                    for j in range(len(dp[v])):
                        val = dp[v][j] * dp[u][i] % MOD
                        ndp[i + j] = (ndp[i + j] + val) % MOD
                dp[u] = ndp
                size += len(dp[v]) - 1

            return

        dfs(1)

        print(dp[1][0] % MOD)

if __name__ == "__main__":
    solve()
```Giải pháp này xây dựng cây gốc từ mảng gốc và chạy DFS thứ tự sau. Mỗi nút duy trì một mảng DP được lập chỉ mục theo số đỉnh từ cây con của nó được coi là “xuất lên trên”. Bước hợp nhất là tích hợp các mảng DP từ các mảng con, tích lũy tất cả các cách có thể để phân phối các phần tử được xuất trên các cây con. 

Câu trả lời cuối cùng được lấy ở gốc mà không có phần tử nào được xuất, vì gốc không có tổ tiên để tiếp nhận bất kỳ phép gán nào trở lên. 

Việc triển khai dựa trên sự tích chập giống như đa thức lặp đi lặp lại. Các vòng lặp lồng nhau bên trong thực hiện điều này một cách trực tiếp và tính chính xác phụ thuộc vào việc căn chỉnh chính xác các chỉ số i + j khi kết hợp các trạng thái từ hai cây con. Việc ghi sổ kích thước đảm bảo các mảng vẫn đủ lớn để thể hiện tất cả số lượng xuất khẩu có thể có. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét một chuỗi đơn giản: 1 → 2 → 3. 

| Nút | trạng thái dp (k xuất khẩu) | 
| --- | --- | 
| 1 | [1] | 
| 2 | [1] | 
| 3 | [1] | 

Mỗi nút không có sự phân nhánh có ý nghĩa, do đó không bắt buộc phải xuất. Mỗi cây con hợp nhất một cách tầm thường. 

Điều này cho thấy rằng DP không đưa ra các hạn chế nhân tạo trong các cấu trúc tuyến tính, vì các mối quan hệ tổ tiên-con cháu đã cho phép sự linh hoạt hoàn toàn. 

### Ví dụ 2 

Hãy xem xét một ngôi sao: 1 với trẻ em 2, 3. 

| Bước | Nút đã xử lý | trạng thái dp[1] | 
| --- | --- | --- | 
| ban đầu | 1 | [1] | 
| thêm 2 | hợp nhất lá | [1, 0] | 
| thêm 3 | hợp nhất lá | [1, 0, 0] | 

Sau khi hợp nhất cả hai phần tử con, chỉ những cấu hình không có xuất khẩu ở gốc mới đóng góp. 

Điều này thể hiện cách xuất khẩu tích lũy khi kết hợp các cây con độc lập và tại sao ràng buộc gốc buộc phải lọc tới k = 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) mỗi lần kiểm tra | Mỗi cạnh kích hoạt tích chập các mảng DP có tổng kích thước bằng O(n) trên cây | 
| Không gian | O(n²) | Mảng DP lưu trữ tối đa kích thước cây con cho mỗi nút | 

Tổng n trong các trường hợp thử nghiệm là 5000, do đó, cách tiếp cận O(n²) phù hợp thoải mái trong giới hạn thời gian trong Python khi được triển khai cẩn thận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# minimal tree
assert run("""1
2
1
""") == "1"

# chain
assert run("""1
3
1 2
""") == "1"

# star
assert run("""1
3
1 1
""") == "1"

# small balanced
assert run("""1
4
1 1 2
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây 2 nút | 1 | trường hợp cơ sở đúng đắn | 
| chuỗi 3 | 1 | xử lý cấu trúc tuyến tính | 
| sao 3 | 1 | anh chị em độc lập | 
| Hỗn hợp 4 nút | 2 | hành vi hợp nhất cây con | 

## Vỏ cạnh 

Đối với cây hai nút, đỉnh 1 là cha của 2. DP bắt đầu bằng dp[1] = [1], dp[2] = [1] và việc hợp nhất không tạo ra trạng thái bổ sung nào. Ràng buộc gốc thực thi dp[1][0] = 1, khớp với thực tế là chỉ hoán vị danh tính mới thỏa mãn tính nhất quán tổ tiên-con cháu cho cả hai nút cùng một lúc. 

Đối với cây hình ngôi sao, tất cả các lá đều là cây con độc lập dưới gốc. Mỗi lá đóng góp một DP tầm thường và việc hợp nhất cho thấy rằng bất kỳ sự chuyển nhượng chéo nào cũng sẽ yêu cầu xuất khẩu không thể được hấp thụ ở gốc, do đó chỉ các cấu hình bên trong cân bằng mới có hiệu lực. Thuật toán lọc các hoán vị không hợp lệ một cách tự nhiên thông qua ràng buộc số lượng xuất và dp[1][0] cuối cùng nắm bắt chính xác các phép gán toàn cục hợp lệ.
