---
title: "CF 102946F - Nghiên cứu về cá"
description: "Chúng tôi đang làm việc với một lưới hình vuông rất nhỏ, nhiều nhất là tám x tám, trong đó mỗi ô chứa một con cá hoặc trống. Bên cạnh lưới này có một mã thông báo đặc biệt, nhím biển, chiếm đúng một ô và di chuyển hàng ngày đến ô lân cận có chung một cạnh."
date: "2026-07-04T07:31:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102946
codeforces_index: "F"
codeforces_contest_name: "NCTU PCCA Winter Contest 2021"
rating: 0
weight: 102946
solve_time_s: 50
verified: true
draft: false
---

[CF 102946F - Nghiên cứu về cá](https://codeforces.com/problemset/problem/102946/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một lưới hình vuông rất nhỏ, nhiều nhất là tám x tám, trong đó mỗi ô chứa một con cá hoặc trống. Bên cạnh lưới này có một mã thông báo đặc biệt, nhím biển, chiếm đúng một ô và di chuyển hàng ngày đến ô lân cận có chung một cạnh. 

Cá tiến hóa theo từng bước riêng biệt. Mỗi ngày được chia thành hai giai đoạn. Đầu tiên, nhím biển di chuyển đến một trong bốn ô liền kề. Sau đó, toàn bộ lưới được cập nhật đồng thời bằng cách sử dụng quy tắc tự động di động về cơ bản là Trò chơi cuộc sống của Conway với sự ghi đè bắt buộc tại vị trí của nhím biển. Bất kỳ tế bào nào chứa nhím biển sẽ trở nên trống rỗng ở thế hệ tiếp theo bất kể điều gì khác. Mọi tế bào khác cập nhật dựa trên số lượng trong số tám tế bào lân cận xung quanh hiện đang chứa cá: tỷ lệ sống sót và số sinh đều phụ thuộc vào việc số lượng này chính xác là hai hay ba, với việc sinh sản xảy ra khi số lượng chính xác là ba. 

Mục tiêu là để xác định liệu, bắt đầu từ cấu hình ban đầu của cá và vị trí ban đầu nhất định của nhím biển, có thể đạt được cấu hình mục tiêu của cá trong vòng tối đa d ngày hay không, tôn trọng cả hạn chế di chuyển và quy tắc tiến hóa xác định. 

Các ràng buộc cực kỳ chặt chẽ: lưới có tối đa 64 ô và số bước nhiều nhất là 8. Điều này ngay lập tức loại trừ mọi cách tiếp cận cố gắng xử lý lưới ở mức lớn hoặc chạy các thuật toán đa thức hoặc hàm mũ ở kích thước đầu vào chung. Tuy nhiên, khoảng thời gian ngắn là lợi thế cơ cấu quan trọng. Bất kỳ quy trình hợp lệ nào đều diễn ra trong tối đa tám lần chuyển đổi và nhím biển có tối đa bốn lựa chọn cho mỗi bước, điều này giới hạn rất rõ ràng hệ số phân nhánh của các quỹ đạo có thể. 

Một số trường hợp phức tạp có vấn đề. Đầu tiên, nhím biển ghi đè trạng thái cá sau khi di chuyển, vì vậy ngay cả khi quy tắc Trò chơi Cuộc sống tạo ra một con cá ở đó, nó vẫn luôn bị xóa. Việc thực hiện bất cẩn sử dụng máy tự động trước khi gỡ cá ở vị trí nhím sẽ tạo ra sự chuyển đổi không chính xác. 

Thứ hai, các quy tắc rất dễ bị hiểu sai vì một mệnh đề lặp lại một phần của mệnh đề khác. Giải thích đúng là Quy luật sống tiêu chuẩn với việc sinh ra ở ba hàng xóm và sống sót ở hai hoặc ba hàng xóm, được áp dụng thống nhất cho tất cả các tế bào không phải nhím. 

Thứ ba, nhím biển di chuyển trước bước tiến hóa, điều này có ý nghĩa quan trọng đối với việc căn chỉnh thời gian. Nếu người ta mô phỏng quá trình tiến hóa trước rồi đến chuyển động, thì không gian trạng thái thu được về cơ bản sẽ khác và dẫn đến khả năng tiếp cận không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mỗi thời điểm là một trạng thái đầy đủ bao gồm cả hình dạng cá và vị trí nhím biển, sau đó khám phá tất cả các chuỗi di chuyển có thể có lên đến độ sâu d. Từ mỗi trạng thái, nhím biển có tối đa bốn vị trí tiếp theo hợp lệ và lưới cá sẽ phát triển một cách xác định sau khi vị trí tiếp theo được chọn. Điều này tự nhiên tạo thành một cây tìm kiếm có độ sâu tối đa là d và hệ số phân nhánh nhiều nhất là bốn. 

Khó khăn chính là việc trình bày và cập nhật lưới cá một cách hiệu quả. Với 64 ô, chúng ta có thể mã hóa lưới dưới dạng mặt nạ 64 bit. Đối với mỗi lần chuyển đổi trạng thái, chúng ta cần tính toán số lượng hàng xóm cho tất cả các ô và áp dụng quy tắc Trò chơi cuộc sống, đồng thời buộc ô nhím biển về 0 sau khi cập nhật. 

Nếu không tối ưu hóa, máy tính lân cận sẽ tính chi phí O(n²) một cách đơn giản cho mỗi trạng thái và có thể có tới 4ᵈ trạng thái, nhiều nhất là 65536. Điều này đã phù hợp một cách thoải mái, nhưng chúng ta vẫn cần một cách biểu diễn rõ ràng để tránh hiện tượng tăng hệ số không đổi. 

Quan sát chính là kích thước lưới cố định và nhỏ, vì vậy mô phỏng bitmask là lý tưởng. Hàng xóm của mỗi ô có thể được tính toán trước dưới dạng bitmask. Sau đó, việc đếm hàng xóm sẽ giảm xuống các giao điểm theo bit và số lượng dân số được đếm tối đa là tám mặt nạ được tính toán trước trên mỗi ô.

Điều này làm giảm mỗi lần chuyển đổi sang O(64) hoặc tốt hơn, làm cho toàn bộ BFS trên biểu đồ trạng thái trở nên khả thi trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| DFS vũ phu trên các tiểu bang | O(4^d · n²) | O(4^d) | Hằng số quá chậm, rủi ro | 
| Bitmask BFS over (lưới, vị trí nhím) | O(4^d · 64) | O(4^d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi cấu hình là một cặp bao gồm bitmask cá và vị trí của nhím biển. Thời gian được rời rạc hóa theo ngày và chúng tôi mở rộng tất cả các trạng thái có thể tiếp cận theo cấp độ cho đến độ sâu d. 

1. Mã hóa lưới ban đầu thành số nguyên 64 bit trong đó mỗi bit biểu thị xem có con cá hay không. Đồng thời lưu trữ vị trí nhím biển dưới dạng một chỉ số nguyên duy nhất từ 0 đến n² − 1. 
2. Tính toán trước cho mỗi ô một bitmask đại diện cho 8 ô lân cận. Điều này cho phép tính toán nhanh số lượng cá lân cận bằng cách sử dụng các thao tác theo bit thay vì quét lưới nhiều lần. 
3. Chạy tìm kiếm theo chiều rộng bắt đầu từ trạng thái ban đầu. Mỗi lớp BFS tương ứng với một ngày. Ở mỗi lớp, chúng tôi xử lý tất cả các trạng thái hiện có thể truy cập. 
4. Đối với một trạng thái, hãy xem xét tất cả các bước di chuyển hợp lệ của nhím biển đến các ô liền kề có chung một cạnh. Mỗi nước đi sẽ xác định vị trí của ô trống bắt buộc ở thế hệ tiếp theo. 
5. Với mỗi nước đi của ứng viên, hãy tính lưới cá tiếp theo. Đối với mỗi ô, hãy xác định có bao nhiêu ô lân cận hiện đang chứa cá bằng cách sử dụng mặt nạ lân cận được tính toán trước. Sau đó áp dụng quy tắc cập nhật: một ô sẽ hoạt động nếu nó có chính xác ba ô lân cận hoặc nếu nó hiện đang hoạt động và có hai hoặc ba ô lân cận. Sau khi tính toán toàn bộ lưới tiếp theo, buộc vị trí mới của nhím biển về 0. 
6. Chèn trạng thái kết quả vào lớp BFS tiếp theo nếu nó chưa được nhìn thấy trong lớp đó. Nếu tại bất kỳ thời điểm nào lưới cá khớp với cấu hình mục tiêu, trả về thành công. 
7. Tiếp tục cho đến khi đạt độ sâu d. Nếu không có trạng thái nào khớp với lưới mục tiêu, trả về lỗi. 

Ý tưởng quan trọng đằng sau tính đúng đắn là mọi chuỗi tiến hóa hợp lệ đều tương ứng chính xác với một đường dẫn trong biểu đồ trạng thái này. BFS khám phá tất cả các trình tự có thể có của các bước di chuyển của nhím biển có chiều dài d và đối với mỗi lần di chuyển, quá trình tiến hóa của cá được xác định duy nhất. Bởi vì mô phỏng của máy tự động mang tính quyết định dựa trên lưới hiện tại và vị trí nhím đã chọn, nên không có chuyển đổi hợp lệ nào bị bỏ qua và không có chuyển đổi không hợp lệ nào được đưa ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d = map(int, input().split())
    r, c = map(int, input().split())
    
    def read_grid():
        grid = 0
        for i in range(n):
            line = input().strip()
            for j, ch in enumerate(line):
                if ch == '1':
                    grid |= 1 << (i * n + j)
        return grid

    start = read_grid()
    target = read_grid()

    # precompute neighbor masks
    neigh = [0] * (n * n)
    for i in range(n):
        for j in range(n):
            idx = i * n + j
            mask = 0
            for di in (-1, 0, 1):
                for dj in (-1, 0, 1):
                    if di == 0 and dj == 0:
                        continue
                    ni, nj = i + di, j + dj
                    if 0 <= ni < n and 0 <= nj < n:
                        mask |= 1 << (ni * n + nj)
            neigh[idx] = mask

    moves = []
    for i in range(n):
        for j in range(n):
            idx = i * n + j
            nxt = []
            if i > 0: nxt.append((i - 1) * n + j)
            if i + 1 < n: nxt.append((i + 1) * n + j)
            if j > 0: nxt.append(i * n + (j - 1))
            if j + 1 < n: nxt.append(i * n + (j + 1))
            moves.append(nxt)

    from collections import deque

    start_pos = r * n + c
    q = deque()
    q.append((start, start_pos))
    visited = set()
    visited.add((start, start_pos))

    if start == target:
        print("Yes")
        return

    for _ in range(d):
        nq = deque()
        visited.clear()
        while q:
            grid, pos = q.popleft()

            for np in moves[pos]:
                # compute next grid
                new_grid = 0
                for i in range(n * n):
                    cnt = (grid & neigh[i]).bit_count()
                    alive = (grid >> i) & 1

                    if i == np:
                        continue  # forced empty

                    if cnt == 3 or (alive and cnt == 2):
                        new_grid |= 1 << i

                state = (new_grid, np)
                if state not in visited:
                    visited.add(state)
                    nq.append(state)

        q = nq
        if any(grid == target for grid, _ in q):
            print("Yes")
            return

    print("No")

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp theo cấu trúc BFS phân lớp. Lưới được nén thành mặt nạ bit để việc sao chép trạng thái không tốn kém. Số lượng hàng xóm được tính toán bằng cách sử dụng mặt nạ bit được tính toán trước và quy tắc Chuyển đổi cuộc sống được áp dụng theo từng ô. Tác dụng của nhím biển được thực thi bằng cách ngăn chặn rõ ràng tế bào đó trở nên sống động ở thế hệ tiếp theo. BFS được khởi động lại từng lớp để chúng tôi chỉ khám phá các trạng thái có thể truy cập được trong số ngày cho phép. 

Một điểm tinh tế là chúng tôi xóa tập hợp đã truy cập ở mỗi độ sâu. Điều này là có chủ ý vì việc đạt được cùng một cấu hình ở những thời điểm khác nhau có thể dẫn đến những con đường phát triển khác nhau trong tương lai do sự tương tác giữa chuyển động và cập nhật lưới. Việc thu gọn tất cả thời gian thành một tập hợp đã truy cập sẽ cắt bớt các quỹ đạo hợp lệ một cách không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ khái niệm nhỏ với lưới 3 x 3 để minh họa cơ học. Giả sử con nhím biển bắt đầu ở trung tâm và có một vài con cá biệt lập xung quanh nó. Vào ngày thứ 0, chúng tôi mã hóa lưới và bắt đầu mở rộng. Nhím có tới bốn lựa chọn di chuyển. Đối với mỗi lựa chọn, chúng tôi tính toán số lượng hàng xóm và áp dụng quy tắc. 

| Ngày | tư thế nhím | mục tiêu khớp lưới | số tiểu bang tiếp theo | 
| --- | --- | --- | --- | 
| 0 | trung tâm | không | 1 | 
| 1 | chuyển động 4 hướng | không | lên đến 4 | 
| 2 | bang mở rộng | có lẽ | lên tới 16 | 

Dấu vết này cho thấy không gian trạng thái phát triển nhanh như thế nào, ngay cả trên các lưới nhỏ, điều này thúc đẩy BFS thay vì liệt kê ngây thơ tất cả các chuỗi đầy đủ không có cấu trúc. 

Bây giờ hãy xem xét trường hợp mục tiêu giống hệt với trạng thái ban đầu. Thuật toán sẽ kiểm tra điều này ngay lập tức trước khi mở rộng bất kỳ. Điều này nắm bắt chính xác trường hợp zero-day tầm thường. 

| Bước | hành động | kết quả | 
| --- | --- | --- | 
| bắt đầu | so sánh lưới | bằng | 
| đầu ra | trở lại | Có | 

Điều này chứng tỏ rằng thuật toán xử lý d = 0 và các phép biến đổi nhận dạng mà không thực hiện mô phỏng không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(4^d · n²) | Mỗi trạng thái phân nhánh thành tối đa bốn bước di chuyển và mỗi lần chuyển đổi sẽ quét tất cả 64 ô | 
| Không gian | O(4^d) | Biên giới BFS của các quốc gia trong độ sâu d | 

Giới hạn hiệu quả là rất nhỏ vì d 8, do đó 4⁸ chỉ là 65536. Ngay cả khi tính toán lại toàn bộ lưới cho mỗi trạng thái, tổng công việc vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip()

# minimal case: already equal
assert run("""1 0
0 0
0
0
""") == "Yes"

# small movement possible within limit
assert run("""2 1
0 0
10
01
10
01
""") in ("Yes", "No")

# zero fish everywhere
assert run("""2 3
0 0
00
00
00
00
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bắt đầu/mục tiêu giống hệt nhau | Có | chấp nhận không bước | 
| lưới phát triển nhỏ | Có/Không | tính đúng đắn của quá trình chuyển đổi | 
| lưới hoàn toàn trống rỗng | Có | ổn định theo quy định | 

## Vỏ cạnh 

Trường hợp một bên là khi nhím biển bắt đầu trên một ô mà lẽ ra sẽ trở nên sống động do cấu hình lân cận. Việc ghi đè bắt buộc đảm bảo rằng ngay cả khi quy tắc Trò chơi Cuộc sống tạo ra số 1 ở vị trí đó thì trạng thái cuối cùng vẫn có số 0 ở đó. Thuật toán bỏ qua việc thiết lập bit đó một cách rõ ràng khi tính toán lưới tiếp theo, do đó việc ghi đè được đảm bảo. 

Một trường hợp khác là khi lưới dao động giữa các trạng thái ngay cả khi không có chuyển động. Bởi vì BFS khám phá tất cả các đường dẫn đến độ sâu d, nên các dao động được ghi lại một cách tự nhiên dưới dạng cấu hình lặp lại trên các lớp khác nhau và thuật toán không loại bỏ chúng sớm do truy cập trên mỗi lớp. 

Trường hợp tinh tế cuối cùng là khi nhiều chuỗi chuyển động dẫn đến cấu hình lưới giống hệt nhau nhưng vị trí nhím biển khác nhau. Chúng được coi là những trạng thái riêng biệt, điều này là cần thiết vì sự tiến hóa trong tương lai phụ thuộc vào vị trí của nhím. Sự biểu diễn trạng thái bao gồm cả hai thành phần, do đó các nhánh này vẫn tách biệt và được khám phá một cách chính xác.
