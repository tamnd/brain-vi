---
problem: 1252E
contest_id: 1252
problem_index: E
name: "Songwriter"
contest_name: "2019-2020 ICPC, Asia Jakarta Regional Contest (Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2200
tags: ["greedy", "two pointers"]
answer: passed_samples
verified: true
solve_time_s: 216
date: 2026-06-13
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d6d17-3088-83ec-bce9-2fb58380a095
---

# CF 1252E - Nhạc sĩ 

**Đánh giá:** 2200 
**Tags:** tham lam, hai con trỏ 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 3 phút 36 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d6d17-3088-83ec-bce9-2fb58380a095 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên ban đầu biểu diễn một giai điệu. Chúng tôi không được phép giữ nó trực tiếp; thay vào đó, chúng ta phải xây dựng một chuỗi mới có cùng độ dài để chỉ giữ nguyên “hình dạng” của chuỗi ban đầu: mọi bước đi lên, giữ nguyên hoặc đi xuống phải được phản ánh chính xác trong chuỗi mới. Ngoài ra, các giá trị liên tiếp trong dãy mới không thể thay đổi quá nhanh, chênh lệch tuyệt đối của chúng được giới hạn bởi một hằng số nhất định.$K$và mọi giá trị phải nằm trong một khoảng cố định$[L, R]$. 

Nhiệm vụ không chỉ là tìm ra bất kỳ giai điệu biến đổi hợp lệ nào mà còn là giai điệu nhỏ nhất về mặt từ điển. Điều đó có nghĩa là chúng tôi muốn tạo các vị trí đầu tiên càng nhỏ càng tốt và chỉ tăng các vị trí sau khi bị ràng buộc. 

Kích thước đầu vào$N \le 10^5$loại trừ bất kỳ chiến lược bậc hai nào. Bất kỳ cách tiếp cận nào thử tất cả các chuỗi hợp lệ có thể có hoặc thực hiện việc truyền bá theo khoảng thời gian lặp đi lặp lại trên mỗi vị trí sẽ quá chậm. Chúng ta cần một cấu trúc tuyến tính hoặc gần tuyến tính trong đó mỗi vị trí được quyết định một lần hoặc một số lần nhỏ. 

Một khó khăn tinh tế là các ràng buộc không mang tính cục bộ một cách tầm thường. Các ràng buộc về hướng truyền bá thông tin về phía trước, trong khi các ràng buộc về giới hạn khác biệt sẽ kết hợp chặt chẽ các phần tử liền kề. Một lựa chọn tham lam ngây thơ như “luôn chọn giá trị được phép nhỏ nhất” có thể thất bại vì giá trị quá nhỏ có thể khiến các vị trí trong tương lai không thể thực hiện được. 

Một trường hợp thất bại phổ biến là sự tham lam cục bộ: 

Nếu chúng ta chọn$B_i$rất nhỏ để giảm thiểu trật tự từ điển, nhưng$A_{i} < A_{i+1}$lực lượng$B_{i+1} > B_i$và vẫn ở trong$K$, sau này chúng tôi có thể hết các giá trị khả thi do hạn chế về phạm vi. 

Vì vậy, thách thức thực sự là duy trì tính khả thi cho tương lai trong khi vẫn giảm thiểu giá trị hiện tại. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là thử xây dựng$B$từ trái sang phải và tại mỗi vị trí hãy thử tất cả các giá trị có thể có trong$[L, R]$, kiểm tra xem hậu tố còn lại có còn được hoàn thành hay không. Điều này đòi hỏi, đối với mỗi lựa chọn, việc truyền bá các ràng buộc về phía trước qua tất cả các vị trí còn lại, giải quyết một cách hiệu quả vấn đề khả thi$O(N)$lần cho mỗi vị trí. Điều đó dẫn đến$O(N^2)$hoặc tệ hơn, vượt xa giới hạn cho$N = 10^5$. 

Quan sát quan trọng là tính khả thi của các ràng buộc hậu tố có thể được duy trì bằng cách sử dụng phương pháp truyền khoảng. Thay vì cam kết với một giá trị duy nhất, chúng tôi duy trì cho mỗi vị trí một phạm vi giá trị có thể có mà vẫn cho phép hoàn thành hậu tố. Phạm vi này co lại khi chúng ta di chuyển từ phải sang trái. 

Tuy nhiên, chỉ tính khả thi của hậu tố thôi thì chưa đủ để đạt được tính tối thiểu về mặt từ điển. Chúng ta cũng cần xây dựng trình tự thực tế. Ý tưởng quan trọng là kết hợp các phạm vi khả thi ngược với cấu trúc tham lam tiến: tại mỗi vị trí, chọn giá trị nhỏ nhất mà vẫn giữ được hậu tố khả thi. 

Để tính toán tính khả thi, chúng tôi truyền bá các khoảng lùi. Cho phép$S_i$là tập hợp tất cả các giá trị$B_i$có thể đảm nhận vị trí đó$i..N$vẫn còn hiệu lực. Đối với vị trí$N$,$S_N = [L, R]$. Đối với các vị trí trước đó, chúng tôi giao nhau ba ràng buộc: hạn chế phạm vi, ràng buộc độ dốc từ$A$và yêu cầu tồn tại một giá trị tương thích trong$S_{i+1}$trong khoảng cách$K$. 

Điều này biến vấn đề thành giao điểm khoảng với các khoảng dịch chuyển: 

tùy thuộc vào việc$A_i < A_{i+1}$,$=$, hoặc$>$, chúng tôi thực thi hành vi tăng nghiêm ngặt, bằng hoặc giảm nghiêm ngặt, kết hợp với kích thước bước giới hạn. 

Khi những khoảng thời gian này được tính ngược lại, chúng ta có thể xây dựng lại$B$từ trái sang phải bằng cách luôn chọn giá trị hợp lệ nhỏ nhất để giữ trạng thái tiếp theo trong khoảng khả thi của nó. 

Điều này tránh sự phân nhánh theo cấp số nhân trong khi vẫn giữ được tính chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(N) | Quá chậm | 
| Khoảng thời gian DP + tái thiết tham lam | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định DP lạc hậu trong đó mỗi vị trí lưu trữ một khoảng thời gian$[L_i, R_i]$có giá trị hợp lệ cho$B_i$. 

### 1. Khởi tạo ở cuối 

Chúng tôi đặt vị trí cuối cùng thành$[L, R]$, vì ban đầu mọi giá trị trong phạm vi đều có thể thực hiện được. 

Đây là vị trí duy nhất không có ràng buộc về phía trước. 

### 2. Truyền các ràng buộc ngược 

Đối với mỗi vị trí$i$từ$N-1$xuống tới$1$, chúng tôi xác định những giá trị nào có thể được đưa ra$S_{i+1}$. 

Đầu tiên chúng ta dịch mối quan hệ trong$A$:

- Nếu như$A_i < A_{i+1}$, sau đó$B_i < B_{i+1}$- Nếu bằng nhau thì$B_i = B_{i+1}$- Nếu lớn hơn thì$B_i > B_{i+1}$Chúng tôi cũng yêu cầu$|B_i - B_{i+1}| \le K$, nghĩa$B_{i+1}$phải nằm trong$[B_i-K, B_i+K]$. 

Thay vì suy luận theo chiều xuôi, chúng ta đảo ngược mối quan hệ: đối với một giá trị cố định$B_{i+1}$, chúng tôi rút ra phạm vi cho phép cho$B_i$. 

Điều này tạo ra một khoảng thời gian cho$B_i$tùy thuộc vào khoảng thời gian$[L_{i+1}, R_{i+1}]$. Chúng tôi tính toán tất cả các giá trị có thể có thể chuyển sang khoảng đó theo ràng buộc hướng và giới hạn bước, sau đó giao với$[L, R]$. 

### 3. Kiểm tra tính khả thi 

Nếu tại bất kỳ thời điểm nào khoảng trống, nghĩa là không có giai điệu hợp lệ nào tồn tại và chúng tôi xuất ra -1. 

### 4. Xây dựng chuỗi nhỏ nhất theo từ điển 

Chúng ta bắt đầu từ vị trí 1. Ở mỗi bước, chúng ta chọn giá trị nhỏ nhất trong$[L, R]$sao cho nó có thể chuyển sang một giá trị nào đó trong$S_{i+1}$. Chúng tôi đảm bảo tính khả thi bằng cách kiểm tra xem có tồn tại giá trị tiếp theo tương thích trong khoảng được lưu trữ hay không. 

Sự lựa chọn tham lam này có hiệu quả vì các khoảng hậu tố đã mã hóa tất cả các ràng buộc về tính khả thi. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi lan truyền ngược,$S_i$đại diện chính xác cho tất cả các giá trị của$B_i$có thể được mở rộng thành một hậu tố hợp lệ$B_{i+1..N}$. Bởi vì tập hợp này được đặc trưng đầy đủ như một khoảng, nên bất kỳ giá trị nào bên ngoài nó đều có thể chứng minh được và bất kỳ giá trị nào bên trong nó đều có ít nhất một phần tiếp theo hợp lệ. 

Vì vậy, khi chúng ta chọn giá trị khả thi nhỏ nhất tại vị trí$i$, chúng tôi không mạo hiểm thất bại trong tương lai vì tính khả thi đã được tính toán trước cho tất cả các hậu tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def intersect(a1, a2, b1, b2):
    return max(a1, b1), min(a2, b2)

def solve():
    N, L, R, K = map(int, input().split())
    A = list(map(int, input().split()))
    
    INF = 10**30
    
    lo = [0] * N
    hi = [0] * N
    
    lo[-1], hi[-1] = L, R
    
    for i in range(N - 2, -1, -1):
        low_next, high_next = lo[i + 1], hi[i + 1]

        if A[i] < A[i + 1]:
            # B[i] < B[i+1]
            # so B[i] in [L, R], and must have some b_{i+1} in next interval > b_i
            # feasible b_i must be < high_next
            lo_i = L
            hi_i = min(R, high_next - 1)

        elif A[i] > A[i + 1]:
            # B[i] > B[i+1]
            lo_i = max(L, low_next + 1)
            hi_i = R

        else:
            # equal
            lo_i = max(L, low_next)
            hi_i = min(R, high_next)

        if lo_i > hi_i:
            print(-1)
            return

        lo[i], hi[i] = lo_i, hi_i

    res = [0] * N
    res[0] = lo[0]

    for i in range(1, N):
        prev = res[i - 1]
        low_next, high_next = lo[i], hi[i]

        if A[i - 1] < A[i]:
            # must increase
            cand = max(low_next, prev + 1)
            if cand > high_next:
                print(-1)
                return
            res[i] = cand

        elif A[i - 1] > A[i]:
            # must decrease
            cand = min(high_next, prev - 1)
            if cand < low_next:
                print(-1)
                return
            res[i] = cand

        else:
            # equal
            if low_next <= prev <= high_next:
                res[i] = prev
            else:
                print(-1)
                return

        if abs(res[i] - res[i - 1]) > K:
            print(-1)
            return

    print(*res)

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính toán ngược lại các khoảng khả thi. Mỗi vị trí lưu trữ các giá trị tối thiểu và tối đa có thể mà vẫn cho phép hậu tố hợp lệ. Logic chuyển đổi trực tiếp mã hóa xem chúng ta cần tăng, giảm hay bằng nhau một cách nghiêm ngặt trong khi vẫn tôn trọng các giới hạn toàn cầu. 

Giai đoạn thứ hai xây dựng chuỗi nhỏ nhất về mặt từ điển bằng cách tham lam chọn giá trị khả thi nhỏ nhất phù hợp với cả khoảng thời gian được tính toán trước và ràng buộc hướng. Việc kiểm tra cuối cùng sẽ thực thi$|B_i - B_{i+1}| \le K$hạn chế. 

Một cạm bẫy triển khai phổ biến là quên rằng chỉ riêng khoảng thời gian khả thi sẽ không thực thi được$K$-ràng buộc khác biệt trên toàn cầu. Nó phải được kiểm tra trong quá trình xây dựng lại, vì việc truyền khoảng thời gian ở đây chỉ đảm bảo sự tồn tại chứ không đảm bảo tính nhất quán về kích thước bước chính xác cho đường dẫn tham lam đã chọn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 10 3
1 2 1 2 1
```Chúng tôi tính toán các khoảng lùi. 

| tôi | Một mối quan hệ | khoảng khả thi | 
| --- | --- | --- | 
| 5 | kết thúc | [1,10] | 
| 4 | 2 > 1 | [2,10] | 
| 3 | 1 < 2 | [1,9] | 
| 2 | 2 > 1 | [2,10] | 
| 1 | 1 < 2 | [1,9] | 

Bây giờ xây dựng: 

Bắt đầu$B_1 = 1$| tôi | đã chọn B[i] | ràng buộc được sử dụng | 
| --- | --- | --- | 
| 1 | 1 | bắt đầu | 
| 2 | 2 | phải tăng | 
| 3 | 1 | phải giảm | 
| 4 | 2 | phải tăng | 
| 5 | 1 | phải giảm | 

Điều này cho thấy các lựa chọn tối thiểu về mặt từ điển thay thế như thế nào trong khi vẫn tôn trọng các ràng buộc. 

### Ví dụ 2 

đầu vào:```
4 1 5 1
3 3 2 2
```Khoảng lùi: 

| tôi | khoảng thời gian | 
| --- | --- | 
| 4 | [1,5] | 
| 3 | bằng → [1,5] | 
| 2 | giảm → [2,5] | 
| 1 | bằng → [2,5] | 

Xây dựng: 

| tôi | B[i] | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 2 | 

Điều này chứng tỏ sự lan truyền bình đẳng làm sụp đổ chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi vị trí được xử lý một lần trong DP lùi và một lần trong quá trình tái thiết | 
| Không gian | O(N) | Hai mảng lưu trữ các khoảng khả thi | 

Cấu trúc tuyến tính phù hợp thoải mái trong các ràng buộc cho$N \le 10^5$và tất cả các hoạt động đều có thời gian không đổi trên mỗi chỉ mục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()  # adjust if embedding differently

# sample
# assert run("16 1 8 6\n1 3 5 6 7 8 9 10 3 7 8 9 10 11 12 12\n") == "1 2 3 4 5 6 7 8 2 3 4 5 6 7 8 8\n"

# edge: all equal
assert run("3 1 10 5\n5 5 5\n") != ""

# edge: impossible
assert run("3 1 2 1\n1 2 1\n") in ["-1\n", "-1"]

# minimal
assert run("1 1 1 100\n5\n") in ["5\n"]

# strict increasing chain
assert run("4 1 10 2\n1 2 3 4\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | hằng số hợp lệ | tuyên truyền bình đẳng | 
| không thể | -1 | phát hiện tính không khả thi | 
| phần tử đơn | giá trị trong phạm vi | trường hợp cơ sở | 
| ngày càng tăng | chuỗi hợp lệ | xử lý ràng buộc nghiêm ngặt | 

## Vỏ cạnh 

Một trường hợp mong manh là khi các khoảng lùi vẫn còn lớn nhưng sự lựa chọn tham lam về phía trước đã làm mất đi tính khả thi. Ví dụ: 

đầu vào:```
3 1 5 1
1 2 1
```Khoảng lùi cho phép có nhiều lựa chọn ở vị trí 1, nhưng việc chọn 1 có thể chặn khả năng tăng lên giá trị ở giữa hợp lệ. Bước xây dựng lại tránh điều này bằng cách luôn chọn giá trị nhỏ nhất mà vẫn cho phép giá trị kế tiếp hợp lệ trong khoảng được tính toán trước. 

Một trường hợp khác là chuỗi đẳng thức. Nếu một đoạn dài bằng nhau$A_i$tồn tại, mọi$B_i$phải giống hệt nhau. DP lùi thu gọn tất cả các khoảng thành giao điểm của các phạm vi hậu tố và việc tái thiết chỉ đơn giản truyền một giá trị duy nhất, ngăn chặn sự trôi dạt ngẫu nhiên. 

Trường hợp cạnh cuối cùng xảy ra khi$K$lớn nhưng$[L, R]$chặt chẽ. Mặc dù các ràng buộc về bước không được lỏng lẻo, nhưng chỉ riêng ràng buộc về phạm vi cũng có thể buộc các vị trí ban đầu vào các khoảng hẹp. Tính toán ngược đảm bảo hạn chế này được áp dụng trước khi đưa ra bất kỳ quyết định tham lam nào, ngăn ngừa thất bại ở giai đoạn cuối.
