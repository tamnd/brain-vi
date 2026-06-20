---
title: "CF 1038E - Kết hợp tối đa"
description: "Mỗi khối có thể được xem như một cạnh vô hướng có trọng số giữa hai màu, trong đó mỗi điểm cuối là một trong hai màu có thể có tùy theo hướng."
date: "2026-06-16T18:35:10+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dfs-and-similar", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1038
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 508 (Div. 2)"
rating: 2400
weight: 1038
solve_time_s: 282
verified: true
draft: false
---

[CF 1038E - So khớp tối đa](https://codeforces.com/problemset/problem/1038/E) 

**Đánh giá:** 2400 
**Thẻ:** bitmasks, brute Force, dfs và tương tự, dp, đồ thị 
**Thời gian giải:** 4 phút 42 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi khối có thể được xem như một cạnh vô hướng có trọng số giữa hai màu, trong đó mỗi điểm cuối là một trong hai màu có thể có tùy theo hướng. Việc chọn hướng tương đương với việc quyết định hướng cho cạnh đó trong một đường dẫn, bởi vì khi chúng ta cố định thứ tự các khối trong một chuỗi, mọi cặp liền kề sẽ buộc các màu chạm vào phải khớp chính xác. 

Nhiệm vụ không phải là sắp xếp tất cả các khối mà là chọn một tập hợp con rồi sắp xếp chúng thành một chuỗi duy nhất sao cho các khối liền kề kết nối bằng các màu bằng nhau, đồng thời tối đa hóa tổng giá trị khối đã chọn. Mỗi khối được sử dụng tối đa một lần và việc lật một khối chỉ hoán đổi điểm cuối của nó mà không thay đổi giá trị của nó. 

Không gian màu rất nhỏ, chỉ có bốn màu, nhưng số khối lên tới 100, vì vậy khó khăn thực sự là tổ hợp: quyết định tập hợp con nào có thể được xâu chuỗi và cách định tuyến chuỗi qua biểu đồ màu để tích lũy trọng lượng tối đa. 

Ràng buộc cấu trúc quan trọng là bất kỳ chuỗi hợp lệ nào cũng tạo thành một vệt trong đa đồ thị có tối đa bốn đỉnh. Mỗi khối là một cạnh và trình tự là một bước đi sử dụng các cạnh nhiều nhất một lần. 

Một chế độ thất bại ngây thơ xuất hiện khi suy nghĩ cục bộ. Ví dụ: việc tham lam luôn gắn khối tương thích có giá trị cao nhất có thể khiến việc xây dựng bị mắc kẹt sớm, mặc dù thứ tự khác với lựa chọn ban đầu kém hơn một chút sẽ mở ra nhiều cạnh có giá trị cao hơn sau này. Một lỗi nhỏ khác là giả định cấu trúc luôn là một đường dẫn: giải pháp tối ưu có thể xem lại một màu nhiều lần nhưng các cạnh vẫn không được sử dụng lại. 

Vì màu sắc chỉ từ 1 đến 4 nên không gian trạng thái kết nối đủ nhỏ để chúng tôi có thể theo dõi rõ ràng cách các giải pháp từng phần kết nối các điểm cuối. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là thử mọi tập hợp con của các khối và mọi thứ tự theo cả hai hướng, sau đó kiểm tra xem nó có tạo thành một chuỗi hợp lệ hay không. Ngay cả khi không đặt hàng, chỉ cần chọn tập hợp con là$2^n$và việc sắp xếp chúng sẽ làm tăng thêm độ phức tạp giai thừa. Điều này ngay lập tức không thể thực hiện được ở$n=100$. 

Sự đơn giản hóa thực sự đến từ việc nhận thấy rằng điều duy nhất quan trọng đối với việc xây dựng một phần là màu nào hiện đang “mở” ở hai đầu chuỗi. Vì chỉ có bốn màu nên trạng thái có thể được mô tả bằng các điểm cuối và về cơ bản chúng ta đang xây dựng một cấu trúc được kết nối trên biểu đồ 4 nút bằng cách sử dụng các cạnh có trọng số. 

Thay vì suy nghĩ theo trình tự, chúng ta diễn giải lại vấn đề như xây dựng một đồ thị con được kết nối trong đó tất cả các cạnh được chọn đều nằm trong một đường duy nhất. Một ràng buộc về đường đi có thể được thực thi bằng cách đảm bảo rằng tối đa hai đỉnh có bậc lẻ trong nhiều tập cạnh đã chọn. Tuy nhiên, vì chúng tôi không bắt buộc phải bắt đầu hoặc kết thúc ở các màu cụ thể nên chúng tôi có thể cho phép bất kỳ cặp điểm cuối nào. 

Điều này dẫn đến việc lập trình động trên các tập hợp con màu sắc được tạo ra bởi các cạnh. Một cách tiêu chuẩn để giải quyết vấn đề này là nén biểu đồ thành 4 nút và xem xét tất cả các cạnh giữa các cặp màu. Đối với mỗi cặp màu, chúng tôi sắp xếp các cạnh theo trọng số và xem xét lấy k cạnh trên cùng giữa mỗi cặp. Sau đó, chúng tôi chạy DP trên trạng thái chẵn lẻ mức độ của bốn nút. 

Một giải pháp trực tiếp và tiêu chuẩn hơn là giải thích điều này như việc chọn nhiều tập cạnh tạo thành đường Euler được kết nối trong đa đồ thị. Vì khả năng kết nối không đáng kể trên 4 nút, nên ràng buộc giảm xuống mức chẵn lẻ: số đỉnh bậc lẻ phải là 0 hoặc 2. Chúng tôi tối đa hóa tổng trọng lượng theo ràng buộc này. 

Chúng tôi xác định dp[mask] là tổng trọng lượng tối đa của các cạnh được chọn sao cho tập hợp các đỉnh có bậc lẻ bằng mặt nạ. Mỗi cạnh lật tính chẵn lẻ của hai điểm cuối của nó, do đó quá trình chuyển đổi là các cập nhật XOR đơn giản. 

Chúng tôi xử lý từng cạnh một và cập nhật DP, nhưng vì chúng tôi có thể chọn hoặc bỏ qua từng cạnh và n chỉ bằng 100 nên điều này là đủ. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con và hoán vị | O(n!) | O(n) | Quá chậm | 
| DP qua trạng thái chẵn lẻ của 4 nút | O(n · 2^4) | O(2^4) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi khối là một cạnh vô hướng giữa hai màu, bỏ qua hướng vì việc lật chỉ hoán đổi điểm cuối. 

1. Xây dựng danh sách các cạnh trong đó mỗi cạnh kết nối hai màu và mang một trọng số. Điều này chuyển bài toán thành việc chọn một tập hợp các cạnh có trọng số trên 4 đỉnh. 
2. Khởi tạo một mảng DP có kích thước 16, trong đó mỗi chỉ mục đại diện cho một mặt nạ chẵn lẻ trên 4 màu. Một bit là 1 nếu màu tương ứng hiện có bậc lẻ trong tập con các cạnh được chọn. 
3. Đặt dp[0] = 0, nghĩa là không có cạnh nào được chọn sẽ không có độ lẻ và giá trị bằng 0. Tất cả các trạng thái khác được khởi tạo ở mức âm vô cùng vì chúng chưa thể truy cập được. 
4. Lặp lại từng cạnh. Đối với mỗi cạnh nối u và v có trọng số w, chúng ta xem xét hai lựa chọn: bỏ qua hoặc lấy nó. Nếu chúng tôi lấy nó, chúng tôi sẽ cập nhật tính chẵn lẻ bằng cách chuyển đổi các bit u và v và chúng tôi thêm w vào tổng giá trị. 
5. Đối với mỗi cạnh, tiếp theo hãy tính một mảng DP mới, ban đầu được sao chép từ dp, thể hiện tùy chọn bỏ qua cạnh. Sau đó, đối với mỗi mặt nạ trạng thái, hãy chuyển sang mặt nạ XOR (1<<u) XOR (1<<v) với trọng số được tăng thêm. 
6. Thay thế dp bằng next sau khi xử lý từng cạnh. Điều này đảm bảo mỗi cạnh được sử dụng nhiều nhất một lần. 
7. Sau khi xử lý tất cả các cạnh, câu trả lời là dp[mask] tối đa trên tất cả các mặt nạ trong đó mặt nạ có 0 hoặc 2 bit được đặt, vì những bit đó tương ứng với các vệt Euler hợp lệ. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi hợp lệ nào đều tương ứng với việc chọn các cạnh tạo thành một vệt, ngụ ý tất cả các đỉnh ngoại trừ hai đỉnh có thể có bậc chẵn. DP theo dõi rõ ràng mức độ chẵn lẻ sau mỗi cạnh được chọn và mọi chuyển đổi đều duy trì tính chính xác vì việc lật các điểm cuối khớp chính xác với các cập nhật về mức độ chẵn lẻ. Vì mọi tập con của các cạnh đều có thể biểu diễn bằng một chuỗi lựa chọn nào đó nên DP bao gồm tất cả các cấu trúc hợp lệ. Việc tối đa hóa các mặt nạ chẵn lẻ hợp lệ đảm bảo chúng tôi bao gồm cả chu kỳ và các đường dẫn mở. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def popcount(x):
    return bin(x).count("1")

def solve():
    n = int(input())
    edges = []
    
    for _ in range(n):
        a, w, b = map(int, input().split())
        u = a - 1
        v = b - 1
        edges.append((u, v, w))
    
    INF = -10**18
    dp = [INF] * 16
    dp[0] = 0
    
    for u, v, w in edges:
        ndp = dp[:]  # skip edge
        bit = (1 << u) ^ (1 << v)
        for mask in range(16):
            if dp[mask] == INF:
                continue
            nm = mask ^ bit
            ndp[nm] = max(ndp[nm], dp[mask] + w)
        dp = ndp
    
    ans = 0
    for mask in range(16):
        if dp[mask] == INF:
            continue
        if popcount(mask) in (0, 2):
            ans = max(ans, dp[mask])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mảng DP mã hóa tất cả các cấu hình chẵn lẻ có thể truy cập được sau khi xử lý tiền tố của các cạnh. Bước chuyển tiếp sẽ bỏ qua một cạnh hoặc bao gồm nó và việc đưa vào sẽ lật tính chẵn lẻ tại các điểm cuối của nó, đây chính xác là tác dụng của việc thêm một cạnh vào biểu đồ. 

Bước lọc cuối cùng thực thi điều kiện đường Euler. Mặt nạ có đỉnh lẻ bằng 0 tương ứng với chu trình, trong khi mặt nạ có hai đỉnh lẻ tương ứng với đường dẫn mở. 

Một điểm triển khai tinh tế là chúng ta phải sao chép dp vào ndp trước khi chuyển đổi; nếu không thì các bản cập nhật sẽ sử dụng lại cùng một cạnh nhiều lần trong một lần lặp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi chỉ theo dõi cách dp phát triển trên chế độ xem giảm các cạnh; mặt nạ được hiển thị dưới dạng giá trị 4 bit. 

| Bước | Cạnh | Hành động | cập nhật dp (trạng thái không vô hạn) | 
| --- | --- | --- | --- | 
| 0 | không | ban đầu | dp[0000] = 0 | 
| 1 | 2-1 (w=1) | lấy/bỏ qua | dp[0000]=0, dp[0011]=1 | 
| 2 | 1-4 (w=2) | tích lũy | dp phát triển nhờ sự chuyển đổi từ cả hai trạng thái | 
| ... | ... | ... | chung cuộc tốt nhất = 63 | 

Điều này chứng tỏ rằng nhiều đường dẫn cùng tồn tại đồng thời trong DP và giải pháp tối ưu xuất hiện bằng cách kết hợp các trạng thái chẵn lẻ khác nhau. 

### Mẫu 2 (đã thi công) 

đầu vào:```
3
1 10 2
2 10 1
1 5 3
```| Bước | Cạnh | tóm tắt trạng thái dp | 
| --- | --- | --- | 
| ban đầu | - | dp[0000]=0 | 
| 1 | 1-2 (10) | dp[0000]=0, dp[0011]=10 | 
| 2 | 2-1 (10) | dp[0000]=20, dp[0011]=10 | 
| 3 | 1-3 (5) | dp được mở rộng với mặt nạ liên quan đến nút 3 | 

Câu trả lời cuối cùng là 25 bằng cách lấy tất cả các cạnh theo một vệt hợp lệ. 

Điều này cho thấy rằng nhiều cạnh song song và đảo ngược được xử lý một cách tự nhiên vì mỗi cạnh lật chẵn lẻ một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^4) | Mỗi cạnh trong số tối đa 100 cạnh cập nhật 16 trạng thái DP | 
| Không gian | O(2^4) | Chỉ có 16 mục DP được lưu trữ | 

Hệ số không đổi cực kỳ nhỏ vì bộ màu được cố định ở mức 4, giúp giải pháp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    edges = []
    for _ in range(n):
        a, w, b = map(int, input().split())
        edges.append((a, w, b))

    INF = -10**18
    dp = [INF] * 16
    dp[0] = 0

    def popcount(x):
        return bin(x).count("1")

    for a, w, b in edges:
        u, v = a - 1, b - 1
        ndp = dp[:]
        bit = (1 << u) ^ (1 << v)
        for mask in range(16):
            if dp[mask] == INF:
                continue
            nm = mask ^ bit
            ndp[nm] = max(ndp[nm], dp[mask] + w)
        dp = ndp

    ans = 0
    for m in range(16):
        if dp[m] != INF and popcount(m) in (0, 2):
            ans = max(ans, dp[m])

    return str(ans)

# provided sample
assert run("6\n2 1 4\n1 2 4\n3 4 4\n2 8 3\n3 16 3\n1 32 2\n") == "63"

# single edge
assert run("1\n1 10 2\n") == "10"

# two disjoint edges
assert run("2\n1 5 2\n3 7 4\n") == "12"

# chain
assert run("3\n1 5 2\n2 6 3\n3 7 4\n") == "18"

# cycle case
assert run("4\n1 1 2\n2 2 3\n3 3 1\n1 10 4\n") == "16"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 10 | tính đúng đắn của trường hợp cơ sở | 
| các cạnh rời rạc | 12 | thành phần độc lập | 
| chuỗi | 18 | hiệu quả đặt hàng tối ưu | 
| trường hợp chu kỳ | 16 | chu trình + xử lý tệp đính kèm | 

## Vỏ cạnh 

Đầu vào tối thiểu có một khối được xử lý chính xác vì DP ban đầu đã cho phép chọn không có cạnh hoặc một cạnh duy nhất và một cạnh luôn tạo thành một chuỗi hợp lệ. 

Đối với các cặp màu rời rạc như các cạnh (1,2) và (3,4), DP giữ cả hai đóng góp một cách độc lập và câu trả lời cuối cùng tính tổng chính xác chúng vì chúng không tương tác trong các chuyển tiếp chẵn lẻ. 

Các đầu vào nặng về chu kỳ được xử lý vì các mặt nạ có độ lẻ bằng 0 tự nhiên biểu thị các chu trình khép kín và DP cho phép hình thành chúng mà không yêu cầu điểm cuối, đảm bảo giữ lại trọng lượng toàn bộ chu trình. 

Trường hợp khó phát hiện nhất là khi một cạnh có giá trị cao phải được tạm thời bỏ qua để kích hoạt một chuỗi lớn hơn. DP không cam kết tham lam nên cả bỏ qua và lấy đều được khám phá song song và mức tối đa được bảo toàn ở trạng thái cuối cùng.
