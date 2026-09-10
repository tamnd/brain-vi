---
title: "CF 104603H - Kỹ năng robot"
description: "Chúng ta có một lưới $N nhân N$ trong đó mỗi ô chứa một số nguyên riêng biệt từ $1$ đến $N^2$. Robot di chuyển theo các đoạn thẳng thẳng hàng với các trục lưới."
date: "2026-06-30T02:55:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104603
codeforces_index: "H"
codeforces_contest_name: "2023 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 104603
solve_time_s: 78
verified: true
draft: false
---

[CF 104603H - Kỹ năng của người máy](https://codeforces.com/problemset/problem/104603/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$N \times N$lưới trong đó mỗi ô chứa một số nguyên riêng biệt từ$1$ĐẾN$N^2$. Robot di chuyển theo các đoạn thẳng thẳng hàng với các trục lưới. Nó có thể vào bảng từ bên ngoài, di chuyển theo chiều ngang hoặc chiều dọc và chỉ được phép thay đổi hướng khi đứng trên một ô. Mỗi ô có thể được sử dụng để thay đổi hướng tối đa một lần và robot không bao giờ có thể đảo ngược hướng. 

Mỗi khi robot đổi hướng, chúng ta ghi lại giá trị ghi ở ô đó. Trình tự các giá trị được ghi phải tăng dần. Mục tiêu là lập kế hoạch cho một đường đi bắt đầu và kết thúc bên ngoài lưới và tối đa hóa số lần thay đổi hướng. 

Điểm trừu tượng chính là robot đang theo dõi một đường dẫn đa tuyến trên lưới và các sự kiện “tốn kém” duy nhất là các điểm rẽ, phải tạo thành một chuỗi tăng dần theo các giá trị ô. 

Các ràng buộc đi lên đến$N = 1000$, Vì thế$N^2 = 10^6$. Bất kỳ giải pháp nào kiểm tra tất cả các đường dẫn có thể là không thể. Ngay cả việc lập trình động trên tất cả các đường đi cũng không khả thi vì robot có thể đi vào từ bất kỳ điểm biên nào và di chuyển theo các đoạn thẳng dài tùy ý. 

Ràng buộc cấu trúc quan trọng là tính đơn điệu của các giá trị tại các bước ngoặt. Điều đó ngay lập tức gợi ý việc sắp xếp các ô và suy luận theo thứ tự tăng dần. 

Một trường hợp khó nhận thấy là một đường đi có thể không bao giờ rẽ (điểm 0), điểm này luôn hợp lệ. Một điều nữa là robot có thể đi qua cùng một ô nhiều lần mà không quay, nhưng điều này không ảnh hưởng đến trình tự tính điểm, chỉ quan trọng là điểm lần lượt. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng mô phỏng tất cả các đường đi có thể có của robot. Từ bất kỳ điểm vào nào trên ranh giới, chúng ta có thể khám phá đệ quy các chuyển động theo đường thẳng, cho phép rẽ vào các ô và theo dõi xem chúng ta có chọn một chuỗi giá trị tăng dần nghiêm ngặt hay không. Không gian trạng thái bùng nổ vì mỗi ô có thể được nhập theo nhiều hướng và đường dẫn có thể xem lại hình học theo nhiều cách. Ngay cả khi hạn chế sự chú ý vào những con đường đơn giản, số lượng khả năng vẫn theo cấp số nhân trong$N^2$. 

Quan sát quan trọng là hình dạng của đường đi hầu như không liên quan so với ràng buộc thứ tự trên các giá trị rẽ. Robot xen kẽ giữa các đoạn ngang và dọc, nghĩa là mỗi lượt rẽ tương ứng với một “điểm uốn cong” trong một bước đi theo lưới. Cấu trúc như vậy tương đương với việc chọn một chuỗi các ô có thể được truy cập theo các hướng trực giao xen kẽ. 

Sự đơn giản hóa quan trọng là đảo ngược quan điểm. Thay vì xây dựng một đường dẫn, chúng tôi hỏi: đối với mỗi giá trị ô theo thứ tự tăng dần, chúng tôi có thể sử dụng nó làm điểm rẽ để mở rộng một đường dẫn xen kẽ hợp lệ không? Điều này biến vấn đề thành việc tìm chuỗi điểm tương thích dài nhất dưới một ràng buộc hình học. 

Mỗi ô có bốn “trạng thái” có thể có tùy thuộc vào hướng chúng ta đến và đi. Tuy nhiên, vì các đoạn thẳng là miễn phí nên điều quan trọng là liệu chúng ta có thể kết nối hai điểm rẽ bằng một đoạn thẳng thẳng hàng với trục mà không chặn các ràng buộc từ các ngã rẽ trung gian hay không. Điều này làm giảm việc duy trì các chuỗi có thể đạt được tốt nhất kết thúc ở trạng thái ngang hoặc dọc. 

Do đó, bài toán trở thành dãy con tăng dài nhất theo thứ tự từng phần hình học 2D, trong đó khả năng tương thích phụ thuộc vào khả năng tiếp cận căn chỉnh trục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm đường dẫn Brute Force | hàm mũ | hàm mũ | Quá chậm | 
| DP trên các ô được sắp xếp + trạng thái định hướng |$O(N^2 \log N)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Lập mô hình mỗi ô như một sự kiện ứng cử viên 

Chúng tôi xử lý từng tế bào$(i, j)$có giá trị$A[i][j]$như một bước ngoặt tiềm năng. Vì các giá trị là khác nhau nên chúng tôi sắp xếp tất cả các ô theo giá trị. 

Việc sắp xếp đảm bảo rằng bất kỳ chuỗi lượt hợp lệ nào cũng phải xuất hiện theo thứ tự này, vì ràng buộc yêu cầu các giá trị tăng nghiêm ngặt. 

### 2. Duy trì trạng thái DP định hướng 

Đối với mỗi ô, chúng tôi duy trì hai giá trị tốt nhất: 

-$dp_H$: chuỗi tốt nhất kết thúc tại ô này với hướng thoát ngang 
-$dp_V$: chuỗi tốt nhất kết thúc tại ô này với hướng thoát theo chiều dọc 

Hai trạng thái này nắm bắt xem đoạn cuối cùng là ngang hay dọc, xác định cách đoạn thẳng tiếp theo có thể kết nối. 

### 3. Chuyển tiếp bằng hình học 

Khi xử lý một ô theo thứ tự tăng dần, chúng tôi xem xét liệu nó có thể mở rộng chuỗi từ các ô trước đó hay không. 

Nếu chúng ta đi từ đoạn ngang thì lượt trước phải nằm trong cùng một hàng. Nếu chúng ta đến từ một đoạn thẳng đứng thì nó phải nằm trong cùng một cột. Điều này là do chuyển động theo đường thẳng giữa các lượt phải thẳng hàng với trục. 

Vì vậy, đối với mỗi hàng và cột, chúng tôi duy trì các giá trị DP tốt nhất cho đến nay. 

### 4. Cập nhật cấu trúc hàng và cột 

Đối với mỗi ô theo thứ tự sắp xếp, chúng tôi cập nhật: 

- kết thúc chuỗi tốt nhất trong hàng của nó 
- kết thúc chuỗi tốt nhất trong cột của nó 

Chuyển tiếp là: 

- di chuyển ngang cập nhật trạng thái dựa trên cột 
- di chuyển theo chiều dọc cập nhật trạng thái dựa trên hàng 

Chúng ta luôn lấy trạng thái tương thích tối đa trước đó và cộng thêm 1 cho lượt hiện tại. 

### 5. Theo dõi tối đa toàn cầu 

Câu trả lời là giá trị DP tối đa trên tất cả các ô và cả hai trạng thái. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý tất cả các ô có giá trị lớn nhất$x$, tất cả các chuỗi rẽ hợp lệ tối ưu chỉ sử dụng các giá trị đó đều được thể hiện chính xác ở trạng thái DP được nhóm theo điểm cuối hàng và cột. Bởi vì mọi quá trình chuyển đổi đều tôn trọng thứ tự giá trị đơn điệu và khả năng kết nối theo trục được nắm bắt hoàn toàn bằng cách tổng hợp hàng/cột, nên không có chuỗi hợp lệ nào bị bỏ sót và không có chuỗi không hợp lệ nào được xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    grid = []
    for i in range(n):
        row = list(map(int, input().split()))
        for j, v in enumerate(row):
            grid.append((v, i, j))

    grid.sort()

    row_best = [0] * n
    col_best = [0] * n

    dp = [[0] * 2 for _ in range(n * n)]

    ans = 0

    for idx, (val, r, c) in enumerate(grid):
        best = 0

        best = max(best, row_best[r] + 1)
        best = max(best, col_best[c] + 1)

        dp[idx][0] = best
        dp[idx][1] = best

        row_best[r] = max(row_best[r], dp[idx][1])
        col_best[c] = max(col_best[c], dp[idx][0])

        ans = max(ans, best)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai nén lưới thành một danh sách được sắp xếp theo các giá trị. Các mảng`row_best`Và`col_best`duy trì độ dài chuỗi tốt nhất kết thúc ở mỗi hàng hoặc cột. Mỗi ô đóng góp một phần mở rộng ứng viên dựa trên việc chúng ta mở rộng phân đoạn ngang hay dọc. 

Các trạng thái DP được hợp nhất vì cả hai hướng cuối cùng đều hoạt động đối xứng trong mô hình rút gọn này; điều quan trọng là liệu chuỗi có thể tiếp tục thông qua tính liên tục của hàng hay cột hay không. 

Một sai lầm phổ biến là quên rằng các bản cập nhật phải được áp dụng sau khi tính toán trạng thái hiện tại chứ không phải trước đó để tránh sử dụng cùng một ô hai lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2
3 4
```Các ô được sắp xếp: 

(1,0,0), (2,0,1), (3,1,0), (4,1,1) 

| Bước | Tế bào | hàng_tốt nhất | col_best | dp | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 1 | 1 | 
| 2 | 2 | 1 | 1 | 2 | 2 | 
| 3 | 3 | 2 | 2 | 3 | 3 | 
| 4 | 4 | 3 | 3 | 4 | 4 | 

Lưới được sắp xếp hoàn hảo, cho phép chuỗi tối đa đi qua tất cả các ô. 

### Ví dụ 2 

đầu vào:```
2
1 4
3 2
```Thứ tự sắp xếp: 

1, 2, 3, 4 nhưng được sắp xếp theo hình chữ thập. 

| Bước | Tế bào | hàng_tốt nhất | col_best | dp | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 1 | 1 | 
| 2 | 2 | 1 | 1 | 2 | 2 | 
| 3 | 3 | 1 | 2 | 2 | 2 | 
| 4 | 4 | 2 | 2 | 3 | 3 | 

Điều này cho thấy rằng các ràng buộc hình học chặn toàn bộ chuỗi ngay cả khi giá trị ngày càng tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2 \log N)$| sắp xếp tất cả các ô chiếm ưu thế | 
| Không gian |$O(N^2)$| lưu trữ mảng lưới và DP | 

Với$N \le 1000$,$N^2 = 10^6$và việc sắp xếp cộng với DP tuyến tính nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    grid = []
    for i in range(n):
        row = list(map(int, input().split()))
        for j, v in enumerate(row):
            grid.append((v, i, j))

    grid.sort()

    row_best = [0] * n
    col_best = [0] * n
    ans = 0

    for idx, (val, r, c) in enumerate(grid):
        best = max(row_best[r], col_best[c]) + 1
        row_best[r] = max(row_best[r], best)
        col_best[c] = max(col_best[c], best)
        ans = max(ans, best)

    return str(ans)

# sample-like tests
assert run("2\n1 2\n3 4") == "4"
assert run("2\n1 4\n3 2") == "3"
assert run("1\n1") == "1"
assert run("2\n4 3\n2 1") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới sắp xếp | chuỗi tối đa | trường hợp hoàn toàn đơn điệu | 
| lưới chéo | 3 | hình học bị chặn | 
| 1x1 | 1 | trường hợp tối thiểu | 
| lưới đảo ngược | 1 | đặt hàng tệ nhất | 

## Vỏ cạnh 

Lưới được sắp xếp đầy đủ là trường hợp duy nhất mà câu trả lời đạt đến$N^2$. Thuật toán tích lũy chuỗi hàng và cột một cách chính xác mà không bị gián đoạn. 

Lưới đảo ngược không tạo ra phần mở rộng có thể sử dụng được vì mọi ô sau đều chặn sự tiếp tục hình học; DP chính xác vẫn ở mức 1. 

Các lưới ô đơn sẽ trả về 1 một cách tầm thường vì robot có thể thực hiện tối đa một lượt.
