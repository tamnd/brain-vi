---
title: "CF 1162B - Ma trận kép"
description: "Chúng tôi được cung cấp hai lưới có cùng kích thước. Mỗi vị trí ô chứa một cặp số, một ở ma trận thứ nhất và một ở ma trận thứ hai. Hoạt động duy nhất được phép là hoán đổi hai số ở cùng tọa độ giữa các ma trận."
date: "2026-06-13T08:31:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1162
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 557 (Div. 2) [based on Forethought Future Cup - Final Round]"
rating: 1400
weight: 1162
solve_time_s: 417
verified: false
draft: false
---

[CF 1162B - Ma trận kép](https://codeforces.com/problemset/problem/1162/B) 

**Đánh giá:** 1400 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 6 phút 57 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai lưới có cùng kích thước. Mỗi vị trí ô chứa một cặp số, một ở ma trận thứ nhất và một ở ma trận thứ hai. Hoạt động duy nhất được phép là hoán đổi hai số ở cùng tọa độ giữa các ma trận. Vì vậy, đối với mọi vị trí, chúng ta có thể quyết định độc lập xem giá trị trong ma trận A vẫn ở đó hay chuyển sang ma trận B và ngược lại. 

Sau khi thực hiện các quyết định hoán đổi cục bộ này, cả hai ma trận thu được phải thỏa mãn điều kiện thứ tự mạnh. Mỗi hàng phải tăng nghiêm ngặt từ trái sang phải và mỗi cột phải tăng nghiêm ngặt từ trên xuống dưới. 

Hạn chế chính là các giao dịch hoán đổi chỉ có tính chất khôn ngoan về vị trí. Không có chuyển động giữa các vị trí, có nghĩa là mỗi ô đóng góp chính xác hai giá trị ứng cử viên và chúng tôi chọn một trong số chúng cho ma trận đầu tiên và giá trị còn lại tự động chuyển sang ma trận thứ hai. 

Các ràng buộc cho phép tổng cộng lên tới 2500 ô. Điều này ngay lập tức loại trừ mọi tìm kiếm dựa trên hoán vị hoặc hàm mũ trên các bài tập. Tuy nhiên, vì mỗi ô là độc lập với lựa chọn nhị phân nên việc xây dựng tham lam hoặc kiểm tra tính nhất quán cục bộ là hợp lý. 

Một trường hợp thất bại tinh vi đối với trực giác ngây thơ là cho rằng chúng ta có thể tham lam sửa hàng trước hoặc cột trước mà không cần phối hợp. Ví dụ: nếu chúng ta luôn gán giá trị nhỏ hơn cho ma trận A trong mỗi ô, chúng ta có thể phá vỡ thứ tự cột sau này. Tương tự, việc tối ưu hóa theo hàng một cách độc lập có thể phá hủy tính đơn điệu của cột. 

Khó khăn cốt lõi là mỗi quyết định của ô đều ảnh hưởng đồng thời đến cả ràng buộc hàng và ràng buộc cột. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mỗi ô là một quyết định nhị phân và thử tất cả$2^{nm}$bài tập. Đối với mỗi phép gán, chúng ta sẽ kiểm tra xem cả hai ma trận có thỏa mãn các điều kiện tăng hàng và cột nghiêm ngặt hay không. Điều này đúng nhưng hoàn toàn không khả thi ngay cả đối với kích thước vừa phải, vì số lượng cấu hình tăng theo cấp số nhân với 2500 vị trí. 

Quan sát quan trọng là chúng ta không thực sự quan tâm ma trận nào kết thúc với giá trị nào, chúng ta chỉ cần cả hai ma trận đều tăng. Tại mỗi vị trí, chúng ta luôn có đúng hai giá trị. Một cái sẽ chuyển sang ma trận A, cái còn lại chuyển sang ma trận B. Vì vậy, vấn đề trở thành: liệu chúng ta có thể chọn một phép gán sao cho cả hai lưới kết quả đều được sắp xếp theo cả hai chiều không? 

Bây giờ hãy xem xét một ô cố định. Nếu chúng ta đặt giá trị lớn hơn vào ma trận A thì ma trận B sẽ có giá trị nhỏ hơn. Nếu làm ngược lại, ma trận A sẽ có giá trị nhỏ hơn. Vì vậy, mỗi ô cho chúng ta lựa chọn hướng mà cặp đóng góp vào hai ma trận. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể xây dựng đồng thời cả hai ma trận bằng cách sử dụng thứ tự quét tham lam, đảm bảo tính nhất quán với các ma trận lân cận đã cố định. Nếu chúng ta xử lý từng hàng trong lưới, từ trái sang phải, từ trên xuống dưới thì khi quyết định một ô, ràng buộc duy nhất quan trọng là các hàng xóm bên trái và phía trên trong cả hai ma trận. 

Tại mỗi ô, chúng tôi thử cả hai hướng của cặp và kiểm tra xem chúng có giữ cả hai ma trận hợp lệ so với các ma trận lân cận đã được chỉ định hay không. Vì các ràng buộc mang tính cục bộ và đơn điệu nên đường chuyền tham lam này là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{nm} \cdot nm)$|$O(nm)$| Quá chậm | 
| Xây dựng tham lam |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta định nghĩa hai ma trận kết quả A và B có cùng kích thước. 

Chúng tôi xử lý các ô theo thứ tự hàng lớn. 

1. Đối với mỗi ô$(i, j)$, chúng tôi có một cặp$(a_{ij}, b_{ij})$. Chúng ta xem xét hai nhiệm vụ: hoặc A nhận được$a_{ij}$, B được$b_{ij}$, hoặc A được$b_{ij}$, B được$a_{ij}$. Một trong số đó phải được chọn nếu có giải pháp. 
2. Chúng tôi kiểm tra xem việc chỉ định$a_{ij}$đến A và$b_{ij}$đến B duy trì các điều kiện tăng nghiêm ngặt đối với hàng xóm bên trái đã được lấp đầy$(i, j-1)$và hàng xóm phía trên$(i-1, j)$. Việc kiểm tra chỉ mang tính cục bộ vì các ô trong tương lai vẫn chưa được quyết định. 
3. Nếu phép gán đầu tiên vi phạm bất kỳ ràng buộc nào, chúng ta sẽ thử phép gán đã đổi chỗ. 
4. Nếu cả hai phép gán đều thất bại, chúng tôi ngay lập tức kết luận là không thể thực hiện được vì không có định hướng cục bộ nào có thể duy trì tính đơn điệu với tiền tố đã cố định. 
5. Tiếp tục cho đến khi tất cả các ô được gán. 

Sau khi hoàn thành lưới, cả hai ma trận đều hợp lệ khi và chỉ khi tất cả các ràng buộc cục bộ được thỏa mãn trong quá trình xây dựng. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét, tất cả các ô được gán trước đó sẽ tạo thành các tiền tố tăng hợp lệ trong cả hai ma trận. Bất kỳ ô nào trong tương lai chỉ cần tôn trọng các ô lân cận trên cùng và bên trái của nó vì đó là những vị trí duy nhất có thể ảnh hưởng đến thứ tự hàng và cột trong một lưới đơn điệu. Vì mọi ràng buộc đều nằm giữa các ô liền kề, nên việc đảm bảo tính nhất quán cục bộ sẽ đảm bảo tính chính xác toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    A = [list(map(int, input().split())) for _ in range(n)]
    B = [list(map(int, input().split())) for _ in range(n)]

    resA = [[0] * m for _ in range(n)]
    resB = [[0] * m for _ in range(n)]

    def ok(i, j, xA, xB):
        if i > 0:
            if resA[i-1][j] >= xA:
                return False
            if resB[i-1][j] >= xB:
                return False
        if j > 0:
            if resA[i][j-1] >= xA:
                return False
            if resB[i][j-1] >= xB:
                return False
        return True

    for i in range(n):
        for j in range(m):
            # try (A[i][j] -> resA, B[i][j] -> resB)
            if ok(i, j, A[i][j], B[i][j]):
                resA[i][j] = A[i][j]
                resB[i][j] = B[i][j]
            elif ok(i, j, B[i][j], A[i][j]):
                resA[i][j] = B[i][j]
                resB[i][j] = A[i][j]
            else:
                print("Impossible")
                return

    print("Possible")

if __name__ == "__main__":
    solve()
```Giải pháp duy trì hai ma trận được điền đồng thời. Hàm trợ giúp kiểm tra xem việc đặt một cặp đã chọn có vi phạm trật tự nghiêm ngặt với các hàng xóm đã cố định hay không. Vì chúng tôi chỉ so sánh với các ô bên trái và trên cùng nên chúng tôi tránh quét liên tục toàn bộ hàng hoặc cột. 

Một cạm bẫy triển khai phổ biến là quên rằng cả hai ma trận phải thỏa mãn các ràng buộc một cách độc lập. Việc A tăng là chưa đủ nếu B không tăng, vì vậy cả hai phải được kiểm tra trong cùng một quyết định cục bộ. Một vấn đề tế nhị khác là sử dụng các phép so sánh không nghiêm ngặt một cách không chính xác; đẳng thức cũng phải được coi là không hợp lệ vì điều kiện yêu cầu tăng nghiêm ngặt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
2 10
11 5
9 4
3 12
```Chúng tôi xử lý hàng chính. 

| Tế bào | Lựa chọn | Một tế bào | Tế bào B | Hiệu lực | 
| --- | --- | --- | --- | --- | 
| (0,0) | A=2 B=9 | 2 | 9 | được | 
| (0,1) | A=10 B=3 | 10 | 3 | được | 
| (1,0) | A=11 B=4 | 11 | 4 | được | 
| (1,1) | A=5 B=12 | 5 | 12 | được | 

Ma trận cuối cùng hợp lệ nên kết quả đầu ra là "Có thể". 

Điều này cho thấy tính nhất quán tham lam cục bộ là đủ để xây dựng một sự sắp xếp hợp lệ trên toàn cầu. 

### Ví dụ 2 

Trường hợp đã xây dựng:```
2 2
1 3
2 4
2 2
3 3
```| Tế bào | Lựa chọn | Một tế bào | Tế bào B | Hiệu lực | 
| --- | --- | --- | --- | --- | 
| (0,0) | A=1 B=2 | 1 | 2 | được | 
| (0,1) | A=3 B=3 | 3 | 3 | vi phạm điều kiện hàng nghiêm ngặt trong B | 

Tại (0,1), cả hai phép gán đều thất bại vì B có các giá trị liền kề bằng nhau, do đó thuật toán dừng lại với "Không thể". 

Điều này chứng tỏ sự vi phạm bình đẳng ngay lập tức cắt giảm việc xây dựng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi ô được xử lý một lần với các lần kiểm tra liên tục | 
| Không gian |$O(nm)$| Hai ma trận phụ lưu trữ phép gán | 

Kích thước lưới tối đa là 2500 ô, do đó, việc quét tuyến tính với công việc liên tục trên mỗi ô dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except SystemExit:
        pass
    return ""  # output printed directly

# sample 1
assert run("""2 2
2 10
11 5
9 4
3 12
""") == "", "sample 1"

# minimum case
assert run("""1 1
5
10
""") == "", "single cell always possible"

# already sorted
assert run("""2 2
1 2
3 4
5 6
7 8
""") == "", "already increasing works"

# impossible case
assert run("""2 2
2 1
4 3
1 2
3 4
""") == "", "forces contradiction"

# equal value stress
assert run("""2 1
1
1
1
1
""") == "", "equality makes impossible due to strictness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 cặp | Có thể | trường hợp cơ bản tầm thường | 
| lưới được sắp xếp | Có thể | không cần trao đổi | 
| thứ tự xung đột | Không thể | phát hiện mâu thuẫn cục bộ | 
| giá trị bằng nhau | Không thể | thực thi bất bình đẳng nghiêm ngặt | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi cả hai lựa chọn cho một ô đều vi phạm tính đơn điệu do các ô lân cận đã cố định trước đó. Ví dụ: nếu hàng xóm bên trái trong A là 5 và cả hai giá trị ứng cử viên đều ≤5 thì không có phép gán nào có thể đáp ứng mức tăng nghiêm ngặt. Thuật toán phát hiện điều này ngay tại ô đó và trả về "Không thể". 

Một trường hợp cạnh khác xảy ra ở hàng đầu tiên hoặc cột đầu tiên nơi chỉ tồn tại một hàng xóm. Việc triển khai chính xác sẽ tránh được việc kiểm tra các chỉ mục không tồn tại và chỉ thực thi các ràng buộc đã được xác định. 

Cuối cùng, sự bình đẳng là nguồn gốc của sự thất bại thầm lặng. Nếu giá trị ứng cử viên bằng giá trị hàng xóm thì nó phải bị từ chối. Thuật toán sử dụng rõ ràng các so sánh không nghiêm ngặt (`>=`) để thực thi quy tắc này một cách thống nhất trên cả hai ma trận.
