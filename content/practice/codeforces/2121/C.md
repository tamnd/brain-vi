---
title: "CF 2121C - Những Người Ở Bên Chúng Ta"
description: "Chúng ta được cho một lưới hình chữ nhật gồm các số nguyên. Trong một lần di chuyển, chúng ta chọn một hàng và một cột. Mỗi ô nằm trong hàng đã chọn hoặc cột đã chọn đó sẽ bị giảm đi một ô, với ô giao nhau chỉ được tính một lần."
date: "2026-06-08T03:47:17+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2121
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1032 (Div. 3)"
rating: 1200
weight: 2121
solve_time_s: 94
verified: true
draft: false
---

[CF 2121C - Những người ở bên chúng tôi](https://codeforces.com/problemset/problem/2121/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật gồm các số nguyên. Trong một lần di chuyển, chúng ta chọn một hàng và một cột. Mỗi ô nằm trong hàng đã chọn hoặc cột đã chọn đó sẽ bị giảm đi một ô, với ô giao nhau chỉ được tính một lần. 

Nhiệm vụ là chọn hàng và cột này sao cho sau khi thực hiện thao tác đơn lẻ này, giá trị lớn nhất ở bất kỳ đâu trong lưới trở nên nhỏ nhất có thể. 

Khó khăn chính là việc giảm một hàng và một cột sẽ ảnh hưởng đến vùng hình chữ thập chứ không phải mảng con đơn giản. Mỗi lựa chọn ứng cử viên của hàng và cột sẽ thay đổi nhiều ô cùng một lúc và chúng ta phải suy luận về cách phần tử tối đa hoạt động sau khi giảm có cấu trúc này. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số ô trên tất cả các trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ việc thử tất cả$n \cdot m$cặp hàng và cột cho mọi trường hợp thử nghiệm, vì điều đó sẽ dẫn đến khoảng$O(n^2 m^2)$hành vi trong trường hợp xấu nhất là quá lớn. Chúng ta cần một phương pháp gần tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi giá trị tối đa là duy nhất. Nếu mức tối đa duy nhất nằm ở một ô, chúng ta luôn có thể chọn hàng và cột của nó, giảm nó và có khả năng giảm mức tối đa tổng thể. Nhưng khi tồn tại nhiều cực đại ở các hàng và cột khác nhau, một thao tác đơn lẻ có thể không bao gồm tất cả chúng và một số cực đại vẫn không bị ảnh hưởng. Sự khác biệt này thúc đẩy toàn bộ giải pháp. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi cặp$(r, c)$, mô phỏng việc giảm tất cả các ô trong hàng$r$và cột$c$, và tính giá trị lớn nhất thu được. Mỗi chi phí mô phỏng$O(nm)$, và có$O(nm)$sự lựa chọn, dẫn đến$O(n^2 m^2)$trong trường hợp xấu nhất. Điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần phải mô phỏng đầy đủ từng lựa chọn. Chúng tôi chỉ quan tâm đến việc liệu giá trị tối đa sau thao tác có trở thành giá trị tối đa ban đầu hay nhỏ hơn nó một đơn vị hay không. Vì mỗi ô bị ảnh hưởng giảm đi đúng một, nên cách duy nhất để giảm mức tối đa toàn cục là đảm bảo rằng tất cả các lần xuất hiện của giá trị tối đa hiện tại đều được “bao phủ” bởi hàng và cột đã chọn. 

Nếu ô tối đa không có trong hàng hoặc cột đã chọn thì ô đó sẽ không thay đổi, do đó ô tối đa vẫn giữ nguyên. Nếu tất cả các ô tối đa được che phủ thì mỗi ô tối đa sẽ giảm xuống tối đa trừ một ô và câu trả lời mới sẽ trở thành giá trị đó, trừ khi giá trị lớn thứ hai vẫn không bị ảnh hưởng và bằng giá trị tối đa ban đầu trừ đi một. 

Do đó, vấn đề giảm xuống còn việc kiểm tra xem liệu chúng ta có thể chọn một hàng và cột bao gồm tất cả các vị trí chứa mức tối đa toàn cục hay không. Nếu có, câu trả lời sẽ giảm đi; mặt khác, nó vẫn không thay đổi. 

Chúng tôi tinh chỉnh điều này hơn nữa. Giả sử giá trị tối đa xuất hiện ở một số ô. Chúng tôi cố gắng chọn một hàng và cột sao cho mọi lần xuất hiện tối đa đều nằm trong hàng đó hoặc cột đó. Nếu một cặp như vậy tồn tại thì mức tối đa sẽ giảm đi một; nếu không thì nó vẫn giữ nguyên. 

Các ứng cử viên duy nhất cho$r$Và$c$vật chất đó được suy ra từ vị trí của các phần tử cực đại. Chúng ta chỉ cần kiểm tra các hàng và cột xuất hiện ở những vị trí đó, điều này giúp tìm kiếm tuyến tính theo số lượng ô tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 m^2)$|$O(1)$| Quá chậm | 
| Lọc vị trí tối đa |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết từng trường hợp thử nghiệm một cách độc lập. 

1. Quét toàn bộ lưới để tìm giá trị lớn nhất. 

Điều này thiết lập mức mục tiêu mà chúng tôi đang cố gắng loại bỏ hoặc giảm bớt. 
2. Thu thập tất cả các tọa độ nơi giá trị tối đa này xuất hiện. 

Đây là những ô duy nhất quan trọng để quyết định liệu thao tác có thể rút gọn câu trả lời hay không. 
3. Cố gắng xác định xem có tồn tại một hàng không$r$sao cho tất cả các ô tối đa đều nằm trong hàng$r$hoặc nằm trong một cột chung. 

Chính xác hơn, đối với mỗi hàng ứng cử viên chứa ít nhất một giá trị tối đa, chúng tôi kiểm tra xem tất cả các giá trị cực đại còn lại chưa được khám phá có chung một cột hay không. 
4. Để kiểm tra một hàng ứng viên$r$, nhìn vào tất cả các vị trí tối đa không liên tiếp$r$. Nếu chúng tồn tại, tất cả chúng phải có cùng một cột$c$. Nếu điều này đúng, chúng tôi đã tìm thấy một cặp hợp lệ$(r, c)$. 
5. Nếu không có cấu hình như vậy thì không thể bao phủ tất cả các ô tối đa bằng một hàng và cột duy nhất, do đó ít nhất một mức tối đa vẫn không bị ảnh hưởng. 
6. Nếu tồn tại một cặp hợp lệ, câu trả lời cuối cùng sẽ có giá trị lớn nhất là trừ một. Nếu không, nó vẫn là mức tối đa ban đầu. 

### Tại sao nó hoạt động 

Thao tác này chỉ làm giảm các ô nằm trong một hàng hoặc cột đã chọn. Mọi ô tối đa đều phải bị ảnh hưởng nếu chúng ta muốn giảm mức tối đa toàn cầu. Một ô tối đa chỉ bị ảnh hưởng nếu nó nằm trong hàng hoặc cột đã chọn. Vì vậy, mọi vị trí lớn nhất phải thuộc về hợp của một hàng và một cột. 

Nếu hai ô tối đa nằm ở các hàng khác nhau và các cột khác nhau cùng một lúc thì không có cặp hàng-cột nào có thể bao gồm cả hai trừ khi một trong các hàng được chọn làm hàng chính và tất cả các cực đại còn lại chia sẻ một cột. Đây chính xác là điều kiện được thử nghiệm trong thuật toán. Cấu trúc bao phủ theo hàng cộng với cột thể hiện đầy đủ tính khả thi nên việc kiểm tra điều kiện này vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        grid = []
        mx = 0

        for _ in range(n):
            row = list(map(int, input().split()))
            grid.append(row)
            mx = max(mx, max(row))

        positions = []
        for i in range(n):
            for j in range(m):
                if grid[i][j] == mx:
                    positions.append((i, j))

        rows = set(i for i, _ in positions)
        cols = set(j for _, j in positions)

        ok = False

        # try fixing a row
        for r in rows:
            col_candidate = -1
            valid = True
            for i, j in positions:
                if i == r:
                    continue
                if col_candidate == -1:
                    col_candidate = j
                elif col_candidate != j:
                    valid = False
                    break
            if valid:
                ok = True
                break

        # try fixing a column if not already ok
        if not ok:
            for c in cols:
                row_candidate = -1
                valid = True
                for i, j in positions:
                    if j == c:
                        continue
                    if row_candidate == -1:
                        row_candidate = i
                    elif row_candidate != i:
                        valid = False
                        break
                if valid:
                    ok = True
                    break

        print(mx - 1 if ok else mx)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên trích xuất giá trị tối đa và tất cả các lần xuất hiện của nó. Sau đó, nó sẽ kiểm tra xem những lần xuất hiện đó có thể được bao phủ bởi một hàng cộng với một cột theo một trong hai hướng hay không. Kiểm tra hai giai đoạn tương ứng chính xác với việc chọn chỉ số nào trong hai chỉ số được chọn đóng vai trò là hàng hoặc cột chiếm ưu thế. 

Một cạm bẫy triển khai phổ biến là giả định rằng việc chọn một hàng và cột riêng lẻ chứa nhiều cực đại là đủ. Ràng buộc chặt chẽ hơn: tất cả các giá trị cực đại bên ngoài hàng đã chọn phải căn chỉnh thành một cột duy nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ: 

đầu vào:```
1
3 3
2 1 2
1 2 1
2 1 2
```Tối đa là 2 và xảy ra ở bốn góc. Chúng tôi kiểm tra các hàng ứng cử viên. 

| Bước | Hàng đã chọn | Vị trí tối đa còn lại | Cột riêng biệt | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | (2,0), (2,2), (1,1), (2,0), (2,2) | 0,2,1 | Không | 

Không có hàng nào cô lập các cực đại còn lại thành một cột, do đó việc bao phủ theo hàng không thành công. Việc kiểm tra dựa trên cột cũng thất bại tương tự vì giá trị cực đại còn lại trải rộng trên nhiều hàng và cột. Do đó đầu ra vẫn ở mức 2. 

Dấu vết này cho thấy rằng mặc dù cực đại là phổ biến, nhưng độ rộng hình học của chúng ngăn không cho một đường chéo nào che phủ chúng. 

Bây giờ hãy xem xét: 

đầu vào:```
1
2 3
5 1 5
1 1 1
```Tối đa là 5 tại các vị trí (0,0) và (0,2). 

Nếu chúng ta chọn hàng 0 thì tất cả các mức tối đa đều đã có trong hàng đó, do đó thao tác sẽ tự động bao gồm chúng. 

| Bước | Hàng | Vị trí tối đa còn lại | Cột bên ngoài hàng | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | không | không | Có | 

Vì vậy, câu trả lời trở thành 4. 

Điều này xác nhận rằng khi tất cả các cực đại đã nằm trong một hàng, thao tác sẽ giảm mức tối đa một cách đáng kể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Chúng tôi quét lưới một lần và sau đó chỉ xử lý các vị trí tối đa | 
| Không gian |$O(k)$| Chúng tôi lưu trữ vị trí của các phần tử tối đa | 

Tổng số ô trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó chỉ cần quét tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Công việc bổ sung ở các vị trí tối đa là không đáng kể khi so sánh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    t = int(input())
    out_lines = []

    for _ in range(t):
        n, m = map(int, input().split())
        grid = []
        mx = 0
        for _ in range(n):
            row = list(map(int, input().split()))
            grid.append(row)
            mx = max(mx, max(row))

        positions = [(i, j)
                     for i in range(n)
                     for j in range(m)
                     if grid[i][j] == mx]

        rows = set(i for i, _ in positions)
        cols = set(j for _, j in positions)

        ok = False

        for r in rows:
            col_candidate = -1
            valid = True
            for i, j in positions:
                if i == r:
                    continue
                if col_candidate == -1:
                    col_candidate = j
                elif col_candidate != j:
                    valid = False
                    break
            if valid:
                ok = True
                break

        if not ok:
            for c in cols:
                row_candidate = -1
                valid = True
                for i, j in positions:
                    if j == c:
                        continue
                    if row_candidate == -1:
                        row_candidate = i
                    elif row_candidate != i:
                        valid = False
                        break
                if valid:
                    ok = True
                    break

        out_lines.append(str(mx - 1 if ok else mx))

    return "\n".join(out_lines)

# provided sample (partial check placeholder)
assert run("""1
1 1
1
""") == "0"

# custom cases
assert run("""1
2 2
1 2
3 4
""") == "3", "no max coverage change"

assert run("""1
1 3
5 5 5
""") == "4", "all maxima in one row"

assert run("""1
3 3
2 1 2
1 2 1
2 1 2
""") == "2", "spread maxima cannot be covered"

assert run("""1
2 3
5 1 5
1 1 1
""") == "4", "row covers all maxima"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | 0 | trường hợp tối thiểu | 
| tất cả các hàng bằng nhau | 4 | phủ sóng toàn hàng | 
| cực đại rải rác | 2 | bảo hiểm không thể | 
| cấu trúc hàng + cột | 4 | thành công mang tính xây dựng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mọi ô đều đã ở mức tối đa. Trong trường hợp đó, chọn bất kỳ hàng và cột nào vẫn để lại nhiều ô không bị ảnh hưởng nhưng tất cả đều được giảm đồng đều nên tối đa giảm đúng một ô. Thuật toán xử lý việc này vì tất cả các vị trí tối đa đều nằm trong mỗi hàng, do đó, bất kỳ kiểm tra dựa trên hàng nào cũng sẽ thành công ngay lập tức. 

Một trường hợp cạnh khác là khi vị trí tối đa tạo thành hình chữ L. Ví dụ: cực đại tại (0,0), (0,1), (1,0). Không có hàng hoặc cột nào bao gồm tất cả chúng cùng một lúc. Thuật toán phát hiện điều này vì sau khi sửa bất kỳ hàng hoặc cột nào, các vị trí còn lại vẫn phân chia thành nhiều hàng và cột khiến việc kiểm tra không thành công. 

Trường hợp tinh tế cuối cùng là khi có chính xác một ô tối đa. Bất kỳ hàng và cột nào được chọn bao gồm nó sẽ thành công ngay lập tức và câu trả lời đúng sẽ trở thành điểm trừ tối đa một.
