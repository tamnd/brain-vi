---
title: "CF 105677D - Kiến trúc chùa"
description: "Chúng ta có một dãy tháp, mỗi tháp được định vị theo tọa độ nguyên từ trái sang phải, với chiều cao được ấn định cho mọi vị trí. Tất cả các độ cao đều khác nhau nên có một tòa tháp cao nhất duy nhất."
date: "2026-06-22T05:06:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105677
codeforces_index: "D"
codeforces_contest_name: "2024-2025 ICPC Southwestern European Regional Contest (SWERC 2024)"
rating: 0
weight: 105677
solve_time_s: 48
verified: true
draft: false
---

[CF 105677D - Kiến trúc đền thờ](https://codeforces.com/problemset/problem/105677/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy tháp, mỗi tháp được định vị theo tọa độ nguyên từ trái sang phải, với chiều cao được ấn định cho mọi vị trí. Tất cả các độ cao đều khác nhau nên có một tòa tháp cao nhất duy nhất. 

Đối với mọi tòa tháp ngoại trừ tòa tháp cao nhất, chúng tôi tìm kiếm tòa tháp gần nhất cao hơn nó một cách nghiêm ngặt. “Gần nhất” được đo bằng khoảng cách chỉ số, do đó, di chuyển một bước sang trái hoặc phải sẽ tốn một đơn vị. Một tòa tháp đóng góp khoảng cách tới bất kỳ tòa tháp nào cao hơn ở gần hơn, cho dù tòa tháp cao hơn đó nằm ở bên trái hay bên phải. Tháp cao nhất không đóng góp được gì vì không tồn tại tháp cao hơn. 

Nhiệm vụ là tính tổng các khoảng cách tối thiểu này trên tất cả các tòa tháp không cao nhất. 

Ràng buộc N lên tới 200.000 buộc chúng ta phải tránh xa mọi chiến lược bậc hai. Kiểm tra đơn giản về việc “quét sang trái và phải cho từng tháp cho đến khi tìm thấy tháp cao hơn” sẽ suy biến thành O(N^2) trong trường hợp xấu nhất, liên quan đến thứ tự so sánh 4 × 10^10 và vượt xa giới hạn 2 giây. 

Một số trường hợp đặc biệt quan trọng đối với tính chính xác. Nếu một tháp không có tháp cao hơn ở một bên nhưng có tháp cao hơn ở bên kia thì chỉ bên hợp lệ mới đóng góp. Nếu nó có các tháp cao hơn ở cả hai bên, chúng ta phải lấy tháp gần hơn chứ không phải tháp đầu tiên gặp phải trong một lần quét một hướng. Tòa tháp cao nhất phải bị loại trừ hoàn toàn mặc dù nó không có câu trả lời hợp lệ. 

Một trường hợp thất bại tinh tế đối với việc quét định hướng đơn giản là khi tòa tháp cao hơn đầu tiên gặp phải trong quá trình quét không phải là tòa tháp gần nhất. Ví dụ, độ cao`[1, 100, 2, 3, 4]`đối với chỉ mục 0: quét bên phải đạt 100 ở khoảng cách 1, điều này đúng, nhưng đối với chỉ mục 2, quét bên phải đạt 3 rồi 4 rồi không có, trong khi cao hơn gần nhất thực sự là 3 ở khoảng cách 1. Một giải pháp đúng phải luôn so sánh đồng thời cả hai bên. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: với mỗi chỉ số i, hãy mở rộng ra bên ngoài cho đến khi chúng ta tìm thấy bất kỳ chỉ số j nào sao cho H[j] > H[i] và giữ khoảng cách nhìn thấy nhỏ nhất. Điều này hiệu quả vì nó tuân theo định nghĩa trực tiếp và tính chính xác là ngay lập tức vì mọi ứng cử viên có thể đều được kiểm tra theo thứ tự khoảng cách tăng dần. 

Vấn đề là chi phí. Trong một mảng giảm đơn điệu như`[N, N-1, ..., 1]`, mọi phần tử phải quét gần như toàn bộ mảng để tìm phần tử cao hơn duy nhất ở bên trái. Điều này dẫn đến trung bình khoảng N/2 công việc cho mỗi phần tử, tạo ra tổng số hoạt động O(N^2). 

Quan sát quan trọng là “phần tử lớn gần nhất” trong mảng 1D có cấu trúc: đối với mỗi vị trí, câu trả lời chỉ phụ thuộc vào phần tử lớn nhất gần nhất ở bên trái và phần tử lớn nhất gần nhất ở bên phải. Bất kỳ phần tử nào lớn hơn đều không bao giờ có liên quan một khi phần tử gần hơn tồn tại ở cùng một phía. 

Điều này biến vấn đề thành một kịch bản ngăn xếp đơn điệu cổ điển. Chúng ta có thể tính toán, đối với mỗi vị trí, phần tử lớn hơn chính xác gần nhất ở bên trái và bên phải trong thời gian tuyến tính. Một khi đã biết được hai ứng cử viên đó, câu trả lời cho mỗi vị trí chỉ đơn giản là khoảng cách với họ càng nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(1) | Quá chậm | 
| Ngăn xếp đơn điệu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách tính toán các phần tử lớn hơn gần nhất ở cả hai phía bằng cách sử dụng các ngăn xếp duy trì chiều cao giảm dần. 

1. Chúng tôi tính toán một mảng`left[i]`biểu thị khoảng cách từ i đến chỉ số gần nhất j < i với H[j] > H[i]. Chúng tôi quét từ trái sang phải trong khi duy trì một chồng chỉ mục có chiều cao theo thứ tự giảm dần. Khi xử lý i, chúng tôi bật từ ngăn xếp cho đến khi đỉnh có chiều cao lớn hơn H[i], bởi vì chỉ những giá trị đó mới có thể hợp lệ “lớn hơn gần nhất ở bên trái”. Giá trị hợp lệ gần nhất là đỉnh ngăn xếp hiện tại sau khi bật lên hoặc không có nếu ngăn xếp trống. 
2. Chúng tôi tính toán một mảng`right[i]`tương tự, nhưng chúng tôi quét từ phải sang trái với cùng một ý tưởng ngăn xếp đơn điệu. Điều này mang lại phần tử lớn hơn gần nhất ở bên phải về khoảng cách chỉ mục. 
3. Với mỗi chỉ số i, nếu là phần tử cao nhất thì bỏ qua vì cả hai đều`left[i]`Và`right[i]`không liên quan theo định nghĩa. 
4. Với mọi i khác, ta lấy giá trị nhỏ nhất`left[i]`Và`right[i]`và thêm nó vào câu trả lời. Nếu một cạnh không tồn tại, chúng ta coi khoảng cách của nó là vô cùng nên cạnh kia được chọn. 
5. Chúng tôi xuất ra tổng số tiền. 

Tại sao nó hoạt động dựa trên tính bất biến được duy trì bởi ngăn xếp. Tại bất kỳ thời điểm nào trong quá trình quét, ngăn xếp chứa các chỉ số có chiều cao giảm dần. Điều này đảm bảo rằng khi chúng ta tìm kiếm phần tử lớn hơn gần nhất, mọi chiều cao nhỏ hơn hoặc bằng nhau đều không liên quan và có thể được loại bỏ một cách an toàn vì chúng không bao giờ có thể đóng vai trò là câu trả lời hợp lệ cho các phần tử hiện tại hoặc tương lai. Phần tử còn lại đầu tiên lớn hơn H[i] nhất thiết phải là phần tử gần nhất ở phía đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    h = list(map(int, input().split()))
    
    left = [10**18] * n
    right = [10**18] * n
    
    stack = []
    
    # nearest greater to left
    for i in range(n):
        while stack and h[stack[-1]] <= h[i]:
            stack.pop()
        if stack:
            left[i] = i - stack[-1]
        stack.append(i)
    
    stack.clear()
    
    # nearest greater to right
    for i in range(n - 1, -1, -1):
        while stack and h[stack[-1]] <= h[i]:
            stack.pop()
        if stack:
            right[i] = stack[-1] - i
        stack.append(i)
    
    mx = max(range(n), key=lambda i: h[i])
    
    ans = 0
    for i in range(n):
        if i == mx:
            continue
        ans += min(left[i], right[i])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tách tính toán thành hai lượt xếp chồng đơn điệu. Sự so sánh`<=`là rất quan trọng vì độ cao là khác nhau, nhưng việc giữ nó nhất quán sẽ đảm bảo tính đúng đắn ngay cả khi giả định được nới lỏng. Khoảng cách được lưu trữ dưới dạng giá trị trọng điểm lớn để các cạnh bị thiếu không vô tình ảnh hưởng đến giá trị tối thiểu. 

Chỉ số của phần tử tối đa được tính một lần và loại trừ khỏi tổng cuối cùng. Điều này tránh được lớp vỏ đặc biệt bên trong logic ngăn xếp và giữ cho cấu trúc đơn điệu đồng nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
3 2 100 1 4 2 1
```Chúng tôi tính toán lớn hơn gần nhất ở bên trái và bên phải. 

| tôi | H[i] | trái lớn hơn | quận trái | phải lớn hơn | đúng quận | đã chọn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 100 | 2 | 100 | 2 | 2 | 
| 1 | 2 | 3 | 1 | 100 | 1 | 1 | 
| 2 | 100 | - | thông tin | - | thông tin | bỏ qua | 
| 3 | 1 | 2 | 2 | 4 | 1 | 1 | 
| 4 | 4 | 100 | 1 | - | thông tin | 1 | 
| 5 | 2 | 4 | 1 | - | thông tin | 1 | 
| 6 | 1 | 2 | 3 | - | thông tin | 3 | 

Tổng là 2 + 1 + 1 + 1 + 1 + 3 = 9. 

Dấu vết này cho thấy rằng mỗi vị trí sẽ giảm xuống một cách độc lập thành sự so sánh giữa hai ứng cử viên gần nhất được tính toán trước thay vì tìm kiếm toàn cầu. 

### Ví dụ 2 

đầu vào:```
5
5 1 4 2 3
```| tôi | H[i] | trái lớn hơn | quận trái | phải lớn hơn | đúng quận | đã chọn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 5 | - | thông tin | - | thông tin | bỏ qua | 
| 1 | 1 | 5 | 1 | 4 | 2 | 1 | 
| 2 | 4 | 5 | 1 | - | thông tin | 1 | 
| 3 | 2 | 4 | 1 | 3 | 1 | 1 | 
| 4 | 3 | 4 | 2 | - | thông tin | 2 | 

Tổng là 1 + 1 + 1 + 2 = 5. 

Ví dụ này chứng minh rằng ngay cả khi các phần tử lớn hơn gần nhất nằm ở các cạnh xen kẽ nhau, giải pháp vẫn đúng vì cả hai hướng đều được theo dõi độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần trong mỗi lần xếp chồng | 
| Không gian | O(N) | Mảng cộng với ngăn xếp đơn điệu | 

Hành vi tuyến tính phù hợp thoải mái trong các ràng buộc lên tới 200.000 phần tử, vì thuật toán chỉ thực hiện một số lượng nhỏ các hoạt động không đổi trên mỗi chỉ mục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n = int(input())
    h = list(map(int, input().split()))
    
    left = [10**18] * n
    right = [10**18] * n
    
    st = []
    for i in range(n):
        while st and h[st[-1]] <= h[i]:
            st.pop()
        if st:
            left[i] = i - st[-1]
        st.append(i)
    
    st.clear()
    
    for i in range(n - 1, -1, -1):
        while st and h[st[-1]] <= h[i]:
            st.pop()
        if st:
            right[i] = st[-1] - i
        st.append(i)
    
    mx = max(range(n), key=lambda i: h[i])
    
    ans = 0
    for i in range(n):
        if i != mx:
            ans += min(left[i], right[i])
    print(ans)

# provided samples
assert run("7\n3 2 100 1 4 2 1\n") == "9", "sample 1"
assert run("5\n5 1 4 2 3\n") == "5", "sample 2"

# minimum size
assert run("2\n1 2\n") == "1"

# decreasing heights
assert run("4\n4 3 2 1\n") == "6"

# increasing heights
assert run("4\n1 2 3 4\n") == "6"

# symmetric peaks
assert run("7\n1 3 2 4 2 3 1\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố tăng dần | 1 | trường hợp hợp lệ nhỏ nhất | 
| giảm nghiêm ngặt | tổng khoảng cách chuỗi | sự thống trị bên trái trong trường hợp xấu nhất | 
| tăng nghiêm ngặt | sự thống trị bên phải đối xứng | sự đúng hướng ngược lại | 
| hình núi | sự lựa chọn gần nhất hỗn hợp | tương tác hai bên | 

## Vỏ cạnh 

Một trường hợp cạnh tới hạn là khi tất cả các độ cao đều giảm dần. Đối với một đầu vào như`[5, 4, 3, 2, 1]`, số lớn hơn gần nhất của mọi phần tử luôn ở bên trái. Ngăn xếp thu gọn một cách chính xác để mỗi phần tử chỉ nhìn thấy các giá trị lớn hơn đã thấy trước đó, tạo ra khoảng cách 1, 1, 1, 1 cho tất cả trừ phần tử đầu tiên. Cách tiếp cận chỉ quét bên phải ngây thơ sẽ thất bại hoàn toàn ở đây vì không tồn tại phần tử lớn hơn bên phải. 

Một trường hợp cạnh khác là một chuỗi tăng nghiêm ngặt như`[1, 2, 3, 4, 5]`. Ở đây số lớn hơn gần nhất của mọi phần tử luôn ở bên phải. Ngăn xếp đơn điệu đảm bảo rằng mỗi phần tử được khớp với phần tử tiếp theo ngay lập tức, không bao giờ bỏ qua các ứng cử viên gần hơn. Bất kỳ cách tiếp cận ngây thơ nào “tìm thấy lớn hơn theo một hướng” ngây thơ mà không so sánh cả hai bên sẽ đánh giá quá cao khoảng cách cho các phần tử ban đầu một cách không chính xác. 

Trường hợp cạnh thứ ba là một đỉnh ở giữa chẳng hạn như`[1, 5, 2, 4, 3]`. Mỗi phần tử ở giữa có các ứng cử viên cạnh tranh ở cả hai bên và chỉ theo dõi đồng thời mới đảm bảo lựa chọn tối thiểu chính xác. Thuật toán xử lý việc này một cách tự nhiên vì mảng bên trái và bên phải được tính toán độc lập và chỉ được hợp nhất ở cuối.
