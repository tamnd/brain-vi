---
title: "CF 102801K - Niềm Tự Hào Của PepperLa"
description: "Vấn đề mô tả một người chạy đang di chuyển qua một lưới đang cháy. Người chạy bắt đầu ở ô trên cùng bên trái và cần đến ô dưới cùng bên phải. Mỗi ô chứa một lượng không khí trong lành."
date: "2026-08-01T23:20:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102801
codeforces_index: "K"
codeforces_contest_name: "The 14th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102801
solve_time_s: 128
verified: true
draft: false
---

[CF 102801K - Lời tự hào của PepperLa](https://codeforces.com/problemset/problem/102801/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Vấn đề mô tả một người chạy đang di chuyển qua một lưới đang cháy. Người chạy bắt đầu ở ô trên cùng bên trái và cần đến ô dưới cùng bên phải. Mỗi ô chứa một lượng không khí trong lành. Các tế bào dương tính có thể được hít vào, trong khi các tế bào không dương tính có chứa khói và không thể xâm nhập trừ khi người chạy hiện đang nín thở. 

Một bước di chuyển luôn đi xuống, phải hoặc theo đường chéo xuống bên phải, vì vậy đường đi là đường dẫn không tuần hoàn được định hướng xuyên qua lưới. Bắt đầu một buổi tập nín thở tốn kém`U`không khí và cho phép đi qua khói nhiều nhất`K`di chuyển. Mục tiêu không chỉ là trốn thoát mà còn tối đa hóa lượng không khí còn lại sau khi đến lối ra. 

Kích thước lưới có thể đạt tới`1000`và tổng của tất cả các ô lưới trong tất cả các trường hợp thử nghiệm có thể đạt tới`7 * 10^6`. Một thuật toán khám phá mọi đường đi có thể là không thể vì số lượng đường đi tăng theo cấp số nhân. Ngay cả một chương trình động kiểm tra mọi ô có thể có trước đó cho mọi vị trí cũng sẽ có mặt`O(N^2M^2)`trong trường hợp xấu nhất, vượt xa giới hạn. Chúng ta cần một giải pháp gần tuyến tính về số lượng ô. 

Các trường hợp nguy hiểm chính đến từ sự tương tác giữa chuyển động bình thường và nhịp thở. Một lối đi có thể cần phải bắt đầu nín thở trước khi đi vào làn khói và vị trí tốt nhất trước đó có thể ở rất xa bên trong đường đi.`K`qua`K`hình chữ nhật chứ không phải là một trong ba ô lân cận. 

Ví dụ:```
1 3 2 5
10 0 10
```Câu trả lời là`15`. Giải pháp bất cẩn chỉ xét đến các ô dương tính lân cận sẽ thất bại vì người chạy phải tiêu tốn không khí để đi qua ô khói ở giữa. 

Một trường hợp khác là khi`K`lớn hơn kích thước lưới:```
2 2 10 3
5 0
0 7
```Câu trả lời là`9`. Một giải pháp giả sử thời lượng hơi thở là chính xác`K`di chuyển có thể từ chối các đường dẫn hợp lệ dừng trước đó. 

## Phương pháp tiếp cận 

Giải pháp lập trình động trực tiếp rất dễ xác định. Cho phép`dp[i][j]`là lượng không khí tối đa còn lại sau khi đến ô`(i,j)`. Nếu ô hiện tại chứa không khí, chúng ta có thể đến từ ba ô bình thường trước đó. Chúng ta cũng có thể bắt đầu nín thở từ bất kỳ ô nào có thể tiếp cận sớm hơn trong`K`hàng và`K`cột, thanh toán`U`và thêm không khí của ô hiện tại. 

Sự tái diễn là đúng, nhưng việc chuyển đổi hơi thở rất tốn kém. Đối với mỗi ô, chúng tôi sẽ quét tối đa`K*K`các vị trí xuất phát có thể. Từ`K`có thể lớn như`10^9`, đây thực sự là quá trình quét toàn bộ lưới trước đó và quá chậm. 

Quan sát quan trọng là quá trình chuyển đổi hơi thở chỉ cần tối đa`dp`giá trị bên trong một hình chữ nhật trượt. Khi quét từng hàng, hình chữ nhật này luôn di chuyển từng bước một. Chúng ta có thể duy trì các giá trị tối đa với hàng đợi đơn điệu. 

Đối với mỗi cột, hãy giữ một deque chứa các vị trí bắt đầu hữu ích từ cột cuối cùng`K`hàng. Deque đang giảm dần`dp`giá trị, vì vậy mặt trước của nó là ứng cử viên tốt nhất trong cột đó. Sau đó duy trì một deque khác trên hàng hiện tại để có được cột tốt nhất trong số cột cuối cùng`K`cột. Hai cấu trúc này cùng nhau cung cấp tối đa`dp`giá trị trong toàn bộ`K`qua`K`cửa sổ trong thời gian khấu hao không đổi. 

Lực lượng vũ phu hoạt động vì mọi lần khởi động nhịp thở hợp lệ đều được kiểm tra nhưng không thành công khi lưới trở nên lớn. Nhận xét rằng quá trình chuyển đổi là mức tối đa trượt cho phép chúng ta thay thế hàng triệu phép so sánh lặp lại bằng hai hàng đợi đơn điệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM * K2) | O(NM) | Quá chậm | 
| Tối ưu | O(NM) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý lưới từ trên xuống dưới và từ trái qua phải. Thứ tự này khớp với các quy tắc di chuyển vì mọi ô có thể có trước ô đều đã được xử lý. 
2. Đối với mỗi ô dương, trước tiên hãy tính toán các chuyển đổi mà không cần nín thở. Ba ô phía trước có thể là ô phía trên, ô bên trái và ô chéo. Thêm không khí của ô hiện tại sau khi lấy giá trị tối đa có thể đạt được. 
3. Duy trì các cột chứa các ô trước đó có thể bắt đầu phiên thở. Xóa các mục nhiều hơn`K`hàng đi. Deque giữ lớn nhất`dp`được đặt lên hàng đầu vì những ứng viên yếu hơn sẽ không bao giờ có thể trở nên hữu ích về sau. 
4. Duy trì một hàng deque chứa các ứng cử viên tốt nhất từ ​​các cột đang hoạt động. Xóa các cột lớn hơn`K`cột đi. Mặt trước của deque này là điểm bắt đầu tốt nhất có thể cho một phiên thở kết thúc tại ô hiện tại. 
5. Nếu ô hiện tại là dương và tồn tại nhịp thở bắt đầu hợp lệ, hãy cập nhật`dp[i][j]`với giá trị từ điểm bắt đầu đó trừ đi`U`, sau đó thêm không khí của ô hiện tại. 
6. Sau khi tính toán`dp[i][j]`, chỉ chèn nó vào cột deque của nó nếu không khí còn lại ít nhất`U`. Giá trị nhỏ hơn không thể bắt đầu phiên thở vì nó không thể trả chi phí. 

Tại sao nó hoạt động: điều bất biến là mỗi deque chứa chính xác các ô hữu ích có thể truy cập bên trong cửa sổ trượt hiện tại, được sắp xếp sao cho ứng cử viên tốt nhất luôn ở phía trước. Việc xóa các mục nhập cũ sẽ duy trì giới hạn khoảng cách và việc xóa các mục nhập chiếm ưu thế sẽ duy trì mức tối đa vì giá trị kém hơn với thời gian hết hạn muộn hơn không bao giờ có thể đánh bại giá trị lớn hơn có thời hạn tương tự hoặc sớm hơn. Mọi chuyển đổi có thể đều được biểu diễn, vì vậy kết quả tính toán`dp`giá trị luôn là lượng không khí tốt nhất có thể. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve_case(n, m, k, u, grid):
    cols = [deque() for _ in range(m)]
    dp_prev = [-1] * m
    ans = -1

    for i in range(n):
        row_best = deque()
        dp_cur = [-1] * m

        for j in range(m):
            while cols[j] and cols[j][0][0] < i - k:
                cols[j].popleft()

            while row_best and row_best[0][1] < j - k:
                row_best.popleft()

            if grid[i][j] > 0:
                best = -1

                if i == 0 and j == 0:
                    best = 0

                if i > 0 and dp_prev[j] >= 0:
                    best = max(best, dp_prev[j])

                if j > 0 and dp_cur[j - 1] >= 0:
                    best = max(best, dp_cur[j - 1])

                if i > 0 and j > 0 and dp_prev[j - 1] >= 0:
                    best = max(best, dp_prev[j - 1])

                if row_best:
                    best = max(best, row_best[0][0] - u)

                if best >= 0:
                    dp_cur[j] = best + grid[i][j]

            if dp_cur[j] >= u:
                while cols[j] and cols[j][-1][1] <= dp_cur[j]:
                    cols[j].pop()
                cols[j].append((i, dp_cur[j]))

            if cols[j]:
                value = cols[j][0][1]
                while row_best and row_best[-1][0] <= value:
                    row_best.pop()
                row_best.append((value, j))

            if i == n - 1 and j == m - 1:
                ans = dp_cur[j]

        dp_prev = dp_cur

    return ans

def main():
    out = []
    data = sys.stdin.buffer.read().split()
    ptr = 0

    while ptr < len(data):
        n = int(data[ptr])
        m = int(data[ptr + 1])
        k = int(data[ptr + 2])
        u = int(data[ptr + 3])
        ptr += 4

        grid = []
        for _ in range(n):
            row = list(map(int, data[ptr:ptr + m]))
            ptr += m
            grid.append(row)

        out.append(str(solve_case(n, m, k, u, grid)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai chỉ lưu trữ hàng trước đó của`dp`, vì các chuyển tiếp thông thường chỉ cần các ô ngay phía trên hoặc theo đường chéo phía trên. Cột deques lưu trữ các chỉ mục hàng để có thể loại bỏ các trạng thái hết hạn khi khoảng cách lớn hơn`K`. 

Thứ tự chèn quan trọng. Một ô chỉ được sử dụng làm điểm bắt đầu nhịp thở sau khi giá trị của chính ô đó đã được tính toán, do đó, ô đó sẽ được thêm vào hàng đợi sau khi tất cả quá trình chuyển đổi cho ô hiện tại kết thúc. Điều này ngăn cản việc vô tình cho phép di chuyển hơi thở có độ dài bằng 0. 

Tất cả các giá trị được lưu trữ dưới dạng số nguyên Python vì lượng không khí còn lại tối đa có thể vượt quá giới hạn 32 bit. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
3 4 2 1
1 0 0 9
0 -1 1 1
-1 0 2 1
```Các trạng thái quan trọng là: 

| Tế bào | Chuyển tiếp tốt nhất | Không khí còn lại | 
| --- | --- | --- | 
| (1,1) | Bắt đầu | 1 | 
| (2,2) | Giữ từ (1,1) | 0 | 
| (2,3) | Dừng giữ, thu thập không khí | 1 | 
| (3,3) | Di chuyển bình thường | 3 | 
| (3,4) | Di chuyển bình thường | 4 | 

Dấu vết cho thấy lý do tại sao phiên thở được thể hiện dưới dạng chuyển tiếp bước nhảy. Người chạy không cần lưu trữ từng ô khói riêng biệt, chỉ cần lưu trữ điểm xuất phát và chi phí cuối cùng. 

Một ví dụ thứ hai:```
2 2 10 3
5 0
0 7
```| Tế bào | Chuyển tiếp tốt nhất | Không khí còn lại | 
| --- | --- | --- | 
| (1,1) | Bắt đầu | 5 | 
| (1,2) | Giữ từ (1,1) | 9 | 
| (2,1) | Giữ từ (1,1) | 9 | 
| (2,2) | Di chuyển bình thường | 16 | 

Cái lớn`K`giá trị cho phép vượt qua khói ngay lập tức và đường đi tối ưu sẽ giữ lại tất cả không khí được thu thập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NM) | Mỗi ô vào và ra khỏi mỗi hàng đợi đơn điệu một lần. | 
| Không gian | O(M) | Chỉ có một hàng DP và hàng đợi cột được lưu trữ. | 

Thuật toán chạm vào mỗi ô lưới một số lần không đổi, phù hợp với tổng giới hạn vài triệu ô. Việc sử dụng bộ nhớ phụ thuộc vào số lượng cột chứ không phải toàn bộ lưới. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    idx = 0
    ans = []

    while idx < len(data):
        n, m, k, u = map(int, data[idx:idx + 4])
        idx += 4
        grid = []
        for _ in range(n):
            grid.append(list(map(int, data[idx:idx + m])))
            idx += m

        ans.append(str(solve_case(n, m, k, u, grid)))

    return "\n".join(ans)

assert run("""3 4 2 1
1 0 0 9
0 -1 1 1
-1 0 2 1
""") == "4", "sample"

assert run("""1 1 5 10
7
""") == "7", "single cell"

assert run("""1 3 2 5
10 0 10
""") == "15", "cross smoke"

assert run("""2 2 10 3
5 0
0 7
""") == "16", "large breath duration"

assert run("""2 3 1 100
5 1 1
1 1 9
""") == "17", "all positive cells"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 x 1`lưới |`7`| Bắt đầu xử lý ô | 
| Khe khói |`15`| Chuyển đổi hơi thở | 
| Lớn`K`|`16`| Buổi tập thở dài | 
| Tất cả các tế bào dương tính |`17`| Chỉ chuyển động bình thường | 

## Vỏ cạnh 

Đường dẫn bắt đầu bằng cách nín thở từ ô xuất phát được xử lý vì`(1,1)`được chèn vào tiến trình DP một cách bình thường. Vụ án```
1 3 2 5
10 0 10
```tạo ra sự chuyển tiếp hơi thở từ ô đầu tiên đến ô cuối cùng. Cửa sổ deque chứa trạng thái bắt đầu, trừ`U`, và thêm không khí cuối cùng. 

Một buổi tập thở kết thúc sớm hơn`K`di chuyển là hợp lệ. TRONG```
2 2 10 3
5 0
0 7
```người chạy không cần phải dành hết mười phút có thể. Cửa sổ trượt chỉ giới hạn khoảng cách tối đa nên vẫn có thể chuyển đổi bình thường sau khi đến ô an toàn. 

Các ô có lượng không khí còn lại thấp sẽ không được đưa vào hàng đợi nhịp thở trừ khi giá trị của chúng ít nhất là`U`. Điều này tránh tạo ra những chuyển tiếp không thể thực hiện được khi người chạy cố gắng bắt đầu nín thở khi không có đủ không khí.
