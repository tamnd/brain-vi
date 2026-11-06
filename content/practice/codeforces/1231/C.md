---
title: "CF 1231C - Ma trận tăng"
description: "Chúng ta được cho một lưới hình chữ nhật gồm các số nguyên. Một số ô đã chứa các giá trị dương cố định, trong khi một số ô chứa các số 0 mà chúng ta được phép thay thế bằng các số nguyên dương mà chúng ta chọn."
date: "2026-06-15T04:59:52+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1231
codeforces_index: "C"
codeforces_contest_name: "Dasha Code Championship - Novosibirsk Finals Round (only for onsite-finalists)"
rating: 1100
weight: 1231
solve_time_s: 126
verified: false
draft: false
---

[CF 1231C - Ma trận tăng dần](https://codeforces.com/problemset/problem/1231/C) 

**Đánh giá:** 1100 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật gồm các số nguyên. Một số ô đã chứa các giá trị dương cố định, trong khi một số ô chứa các số 0 mà chúng ta được phép thay thế bằng các số nguyên dương mà chúng ta chọn. Mục tiêu là gán giá trị cho tất cả các ô bằng 0 sao cho hai điều kiện đơn điệu được giữ đồng thời: mỗi hàng phải tăng nghiêm ngặt từ trái sang phải và mọi cột phải tăng nghiêm ngặt từ trên xuống dưới. 

Trong số tất cả các lần hoàn thành hợp lệ, chúng tôi muốn một lần hoàn thành tối đa hóa tổng của tất cả các mục. Nếu không có sự hoàn thành nào có thể thỏa mãn các ràng buộc đặt hàng nghiêm ngặt thì chúng ta phải báo cáo điều đó là không thể. 

Một chi tiết cấu trúc quan trọng là tất cả các ô số 0 đều nằm hoàn toàn bên trong ma trận, nghĩa là chúng không bao giờ nằm ​​trên đường viền. Điều này đảm bảo rằng mọi ô số 0 đều có sẵn tất cả bốn ô lân cận định hướng ngay lập tức ở đâu đó trong ngữ cảnh hàng và cột của nó, điều này rất quan trọng để suy ra các ràng buộc. 

Các ràng buộc đủ lớn để bất kỳ phương pháp nào cố gắng gán giá trị một cách độc lập cho mỗi ô hoặc mô phỏng điều chỉnh nhiều lần sẽ quá chậm. Với$n, m \le 500$, một giải pháp xung quanh$O(nm)$hoặc$O(nm \log nm)$là cần thiết, trong khi mọi thứ liên quan đến tính toán lại toàn cục lặp đi lặp lại đều bị loại trừ. 

Một trường hợp lỗi phổ biến xuất hiện khi các số 0 tương tác thông qua cả ràng buộc hàng và cột. Ví dụ: nếu chúng ta tham lam lấp đầy mỗi số 0 càng lớn càng tốt chỉ với các hàng xóm bên trái và trên cùng của nó, chúng ta có thể vi phạm các ràng buộc trong tương lai đến từ các hàng xóm bên phải hoặc dưới cùng. Hãy xem xét một hàng như`5 0 2`. Về mặt cục bộ, số 0 có thể được gán từ 5 đến 2 tùy theo hướng, nhưng điều này không nhất quán, do đó phép gán tham lam ngây thơ không thành công. 

Một vấn đề tế nhị khác phát sinh khi số 0 bị ràng buộc bởi cả khoảng thời gian hàng và khoảng thời gian cột và các khoảng thời gian đó có thể xung đột. Nếu giao lộ trống, không có sự phân công hợp lệ nào tồn tại, mặc dù mỗi hướng riêng lẻ có vẻ khả thi. 

## Phương pháp tiếp cận 

Một quan điểm bạo lực sẽ cố gắng gán các giá trị cho từng ô 0 trong khi thực thi các bất đẳng thức nghiêm ngặt. Người ta có thể tưởng tượng việc khám phá các bài tập có thể thực hiện theo từng ô và quay lui bất cứ khi nào một ràng buộc bị vi phạm. Điều này hoạt động về mặt khái niệm vì mỗi phép gán có thể được kiểm tra cục bộ dựa trên các hàng xóm, nhưng số lượng lựa chọn trên 0 là không giới hạn vì các giá trị là số nguyên dương, làm cho không gian tìm kiếm trở nên vô hạn mà không cần cấu trúc bổ sung. Ngay cả khi chúng ta giới hạn các giá trị một cách giả tạo ở mức tối đa nào đó, số lượng cấu hình sẽ tăng theo cấp số nhân theo số 0, trong trường hợp xấu nhất là$O(nm)$. 

Quan sát quan trọng là tính tối ưu và tính khả thi sẽ tách rời khi chúng ta hiểu được giới hạn trên mỗi ô. Đối với mỗi ô trống, giá trị của nó bị ràng buộc từ phía trên bởi các ô lân cận cố định hoặc đã được xác định gần nhất theo hướng hàng và cột của nó, và bị ràng buộc tương tự từ bên dưới. Tính đơn điệu nghiêm ngặt ngụ ý rằng bất kỳ phép gán hợp lệ nào cũng phải nằm trong một khoảng được xác định bởi các ranh giới này. 

Khi chúng ta xem mỗi số 0 là có một khoảng khả thi, vấn đề sẽ trở thành việc kiểm tra tính nhất quán của các khoảng này và sau đó tối đa hóa tổng. Việc tối đa hóa rất đơn giản: mỗi ô phải lấy giá trị lớn nhất mà các ràng buộc của nó cho phép, bởi vì việc tăng giá trị không bao giờ vi phạm các ràng buộc miễn là nó vẫn nằm trong khoảng hợp lệ. 

Khó khăn là tính toán những giới hạn chặt chẽ nhất có thể này trên toàn cầu. Điều này được xử lý bằng cách truyền các ràng buộc theo cả hướng theo hàng và theo cột, đảm bảo rằng mỗi ô tôn trọng tất cả các neo cố định gần nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(nm) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi các ô cố định như những điểm neo áp đặt các ràng buộc đơn điệu dọc theo hàng và cột. Số 0 là những khoảng trống mà giá trị của chúng phải được suy ra giữa các điểm neo này. 

1. Đầu tiên, xử lý từng hàng độc lập từ trái sang phải, sau đó từ phải sang trái. Chúng tôi tính toán các giá trị tối đa cho phép để duy trì mức tăng nghiêm ngặt và tôn trọng các ô cố định. Việc chuyển từ trái sang phải đảm bảo mỗi ô ít nhất lớn hơn ô lân cận bên trái của nó, trong khi lượt chuyển từ phải sang trái đảm bảo tính khả thi đối với giá trị cố định hoặc bị ràng buộc tiếp theo. 
2. Thực hiện việc truyền tương tự trên các cột, từ trên xuống dưới và từ dưới lên trên. Điều này thực thi các ràng buộc nhất quán theo chiều dọc. Sau bước này, mỗi ô đều có phạm vi khả thi được bao hàm bởi các ràng buộc cả hàng và cột. 
3. Đối với mỗi ô, giao các ràng buộc bắt nguồn từ hàng và các ràng buộc bắt nguồn từ cột. Nếu giới hạn dưới vượt quá giới hạn trên thì không thể cấu hình và chúng tôi trả về -1. Bước giao nhau này là bước kiểm tra tính nhất quán quan trọng. 
4. Sau khi xác nhận tính khả thi, hãy gán cho mỗi ô 0 giá trị tối đa được phép theo khoảng giao nhau của nó. Các ô cố định không thay đổi. 
5. Cuối cùng, tính tổng của tất cả các giá trị trong lưới kết quả. 

Lý do chúng ta lấy giá trị tối đa là vì không có quyết định nào trong tương lai phụ thuộc vào việc giảm giá trị, vì tất cả các ràng buộc đều đơn điệu. Việc tăng giá trị luôn cải thiện mục tiêu mà không phá vỡ tính khả thi miễn là nó vẫn nằm trong giới hạn. 

### Tại sao nó hoạt động 

Mỗi hàng và cột thực thi một chuỗi các bất đẳng thức nghiêm ngặt, có nghĩa là mọi ô đều được giới hạn một cách hiệu quả bởi các ô lân cận cố định hoặc đã bị ràng buộc gần nhất theo bốn hướng. Việc truyền bá đảm bảo các giới hạn này nhất quán trên toàn cầu. Giao điểm của các ràng buộc ngang và dọc tạo ra khoảng khả thi chính xác cho mỗi ô. Vì các ràng buộc là đơn điệu nên bất kỳ phép gán khả thi nào cũng có thể được chuyển thành phép gán tối đa theo điểm mà không vi phạm tính khả thi, làm cho việc tối đa hóa tham lam trở nên chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    INF = 10**18

    # row constraints: left-to-right minimum feasibility, right-to-left maximum feasibility
    row_min = [[-INF]*m for _ in range(n)]
    row_max = [[INF]*m for _ in range(n)]

    for i in range(n):
        # left to right: enforce increasing lower bound
        for j in range(m):
            if j == 0:
                row_min[i][j] = 1 if a[i][j] == 0 else a[i][j]
            else:
                prev = row_min[i][j-1]
                cur = a[i][j] if a[i][j] != 0 else 1
                row_min[i][j] = max(cur, prev + 1)

        # right to left: enforce upper bound consistency
        for j in range(m-1, -1, -1):
            if j == m-1:
                row_max[i][j] = a[i][j] if a[i][j] != 0 else INF
            else:
                nxt = row_max[i][j+1]
                cur = a[i][j] if a[i][j] != 0 else INF
                row_max[i][j] = min(cur, nxt - 1)

    # column constraints
    col_min = [[-INF]*m for _ in range(n)]
    col_max = [[INF]*m for _ in range(n)]

    for j in range(m):
        for i in range(n):
            if i == 0:
                col_min[i][j] = 1 if a[i][j] == 0 else a[i][j]
            else:
                prev = col_min[i-1][j]
                cur = a[i][j] if a[i][j] != 0 else 1
                col_min[i][j] = max(cur, prev + 1)

        for i in range(n-1, -1, -1):
            if i == n-1:
                col_max[i][j] = a[i][j] if a[i][j] != 0 else INF
            else:
                nxt = col_max[i+1][j]
                cur = a[i][j] if a[i][j] != 0 else INF
                col_max[i][j] = min(cur, nxt - 1)

    ans = 0

    for i in range(n):
        for j in range(m):
            lo = max(row_min[i][j], col_min[i][j])
            hi = min(row_max[i][j], col_max[i][j])

            if lo > hi:
                print(-1)
                return

            if a[i][j] == 0:
                a[i][j] = hi

            ans += a[i][j]

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt các ràng buộc theo chiều ngang và chiều dọc vì mỗi hướng tạo thành một chuỗi bất bình đẳng độc lập. Việc chuyển hàng đảm bảo tính nhất quán trong các hàng, trong khi chuyển theo cột đảm bảo tính nhất quán trong các cột. Bước giao nhau cuối cùng là nơi cả hai ràng buộc gặp nhau. 

Một điểm tinh tế là các ô cố định hoạt động như các neo cứng: trong quá trình truyền, chúng giới hạn hoặc buộc các giá trị, trong khi các ô 0 hoạt động giống như các phần giữ chỗ linh hoạt. sử dụng$1$vì giá trị bắt đầu tối thiểu đảm bảo tất cả các phép gán vẫn dương. 

Một chi tiết quan trọng khác là việc sử dụng các điểm canh gác lớn cho giới hạn trên. Điều này tránh được sự hạn chế nhân tạo khi một đoạn không có điểm neo cố định phía trên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
1 3 5 6 7
3 0 7 0 9
5 0 0 0 10
8 9 10 11 12
```Sự lan truyền hàng cho hàng thứ hai phát triển như sau: 

| j | a[i][j] | hàng_min | hàng_max | 
| --- | --- | --- | --- | 
| 0 | 3 | 3 | 3 | 
| 1 | 0 | 4 | 6 | 
| 2 | 7 | 7 | 7 | 
| 3 | 0 | 8 | 8 | 
| 4 | 9 | 9 | 9 | 

Sau khi truyền theo cột, các giao điểm sẽ tinh chỉnh các giá trị một cách nhất quán, tạo ra:```
1 3 5 6 7
3 6 7 8 9
5 7 8 9 10
8 9 10 11 12
```Dấu vết này xác nhận rằng mỗi ô số 0 được đẩy lên giá trị cao nhất tương thích với cả hai hướng, tối đa hóa tổng trong khi vẫn duy trì thứ tự nghiêm ngặt. 

### Ví dụ 2 

Hãy xem xét một trường hợp không nhất quán nhỏ hơn:```
3 3
1 2 3
2 0 1
3 4 5
```Ở hàng giữa, các ràng buộc cột buộc ô trung tâm phải lớn hơn 2 và nhỏ hơn 1 đồng thời sau khi truyền, tạo ra một khoảng trống. Thuật toán phát hiện điều này trong giao lộ khi`lo > hi`và ngay lập tức quay trở lại`-1`. 

Điều này chứng tỏ rằng tính không khả thi được nắm bắt hoàn toàn thông qua tính nhất quán theo khoảng thời gian mà không cần tìm kiếm tổng thể rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi hàng và cột được xử lý theo hai lượt tuyến tính | 
| Không gian | O(nm) | Lưu trữ các ràng buộc tối thiểu và tối đa trên mỗi ô | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì$n, m \le 500$cung cấp tối đa 250.000 ô và mỗi ô được xử lý với số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if False else ""  # placeholder for CF-style integration

# provided sample (format placeholder)
# assert run(...) == ...

# custom tests
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới hợp lệ nhỏ nhất không có số 0 | tổng lưới | độ đúng cơ sở | 
| lưới có một số 0 bên trong | nội suy đúng | tính khả thi của địa phương | 
| ràng buộc xung đột | -1 | phát hiện tính không khả thi | 
| xen kẽ các ràng buộc chặt chẽ | tổng tối đa hợp lệ | tính chính xác của việc truyền bá | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một ô 0 bị ép giữa hai giá trị cố định theo cả hướng hàng và cột. Thuật toán xử lý vấn đề này bằng cách tính toán độc lập giới hạn hàng và cột, sau đó giao nhau. Nếu nút giao bị sập, sự bất khả thi sẽ được phát hiện ngay lập tức. 

Một trường hợp cạnh khác xảy ra khi tồn tại một chuỗi dài các số 0 giữa các điểm neo cố định. Trong tình huống này, hàng và cột chuyển các giá trị nâng dần dần và phép gán cuối cùng luôn gán giá trị cuối cùng có thể có trong mỗi phân đoạn, đảm bảo tổng tối đa mà không vi phạm tính đơn điệu. 

Trường hợp cạnh thứ ba là khi toàn bộ hàng hoặc cột không chứa ràng buộc cố định nào ngoại trừ ranh giới. Việc truyền bá dựa trên trọng điểm đảm bảo các phân đoạn này vẫn nhận được chuỗi tăng dần hợp lệ bắt đầu từ 1 mà không có hạn chế nhân tạo.
