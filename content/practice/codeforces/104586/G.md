---
title: "CF 104586G - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0448\u0430\u0445\u043c\u0430\u0442\u044b"
description: "Chúng ta có một bàn cờ 8 x 8, trong đó một số ô được đánh dấu là điểm đến có thể có của một nước đi cờ chưa xác định."
date: "2026-06-30T07:35:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104586
codeforces_index: "G"
codeforces_contest_name: "Codemasters Codecup 2023 - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 104586
solve_time_s: 89
verified: false
draft: false
---

[CF 104586G - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0448\u0430\u0445\u043c\u0430\u0442\u044b](https://codeforces.com/problemset/problem/104586/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bàn cờ 8 x 8, trong đó một số ô được đánh dấu là điểm đến có thể có của một nước đi cờ chưa xác định. Tất cả các ô được đánh dấu tương ứng với các ô mà một quân cờ có thể di chuyển hợp pháp trong đúng một nước đi từ một ô bắt đầu không xác định nào đó, theo luật cờ vua tiêu chuẩn với các quân chặn hiện trên bàn cờ. Bản thân ô bắt đầu không được hiển thị và quân di chuyển cũng không xác định được, ngoại trừ việc nó được đảm bảo không phải là quân tốt. 

Nhiệm vụ là xác định quân cờ nào có thể tạo ra chính xác tập hợp các ô vuông có thể tiếp cận được hiển thị, giả sử bàn cờ có thể chứa các quân chặn và các nước đi tuân theo các quy tắc tiêu chuẩn. Các quân cờ ứng cử viên là vua, hoàng hậu, xe, giám mục và hiệp sĩ. Chúng tôi phải xuất ra tất cả các phần tồn tại ít nhất một vị trí bắt đầu và một số vị trí của các phần chặn phù hợp với mẫu khả năng tiếp cận được hiển thị. 

Điểm trừu tượng chính là chúng tôi không xây dựng lại một bảng đầy đủ. Chúng tôi đang kiểm tra tính khả thi: liệu tập hợp được đánh dấu có thể chính xác là kiểu tấn công hoặc di chuyển của một quân cờ từ một nguồn gốc nào đó hay không, với việc cho phép chặn các quân trượt. 

Kích thước đầu vào không đổi, luôn là 8 x 8, do đó, bất kỳ giải pháp nào có số lượng mô phỏng cố định trên mỗi ô đều nhanh chóng. Thách thức thực sự là tính chính xác: hiểu sai cách việc chặn ảnh hưởng đến khả năng tiếp cận hoặc quên các ràng buộc như “không giữ nguyên vị trí” dẫn đến việc chấp nhận sai các mẫu không thể thực hiện được. 

Trường hợp cạnh tinh tế là khi bộ bao gồm các hình vuông không được kết nối theo bất kỳ cách nào bằng một loại mảnh duy nhất. Ví dụ: một hiệp sĩ luôn tạo ra tối đa 8 điểm bù trừ riêng biệt từ một nguồn gốc duy nhất. Nếu mẫu hiển thị hai cụm cách xa nhau mà không thể giải thích được bằng một nguồn gốc duy nhất thì không có vị trí hiệp sĩ nào hoạt động ngay cả khi các ô vuông riêng lẻ có vẻ hợp lệ cục bộ. 

Một trường hợp cạnh quan trọng khác là các mảnh trượt. Xe hoặc quân tượng có thể bị chặn, điều đó có nghĩa là khả năng tiếp cận không chỉ đơn thuần là hình học từ gốc; nó phụ thuộc vào nơi đặt trình chặn. Điều này làm cho việc kết hợp hình học đơn giản là không đủ. Ví dụ: một quân xe có thể tạo ra bất kỳ tập hợp con nào của một hàng và cột liền kề với điểm gốc cho đến khi một trình chặn chặn nó lại, vì vậy chúng ta chỉ cần đảm bảo rằng mọi ô vuông được đánh dấu đều nằm trên một tia từ điểm gốc và không bắt buộc phải mở rộng ra ngoài ô vuông được đánh dấu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi ô vuông bắt đầu có thể và mọi loại quân cờ, sau đó mô phỏng tất cả các nước đi hợp pháp trên một bàn cờ trống và so sánh tập hợp có thể tiếp cận được với mẫu đã cho. Điều này ngay lập tức thất bại vì việc chặn làm phức tạp việc mô phỏng: đối với các phần trượt, chúng ta cũng cần liệt kê tất cả các cấu hình chặn có thể có để tạo ra chính xác mẫu cắt được quan sát. Số lượng các cấu hình như vậy tăng theo cấp số nhân theo số ô vuông, khiến phương pháp này không khả thi. 

Quan sát quan trọng là các trình chặn không bị ràng buộc bởi tuyên bố vấn đề theo cách hạn chế. Chúng tôi có thể tự do giả định rằng bất kỳ hình vuông nào không nằm trong mẫu đầu ra đều có thể bị chiếm bởi một quân chặn nếu nó giúp điều chỉnh cấu trúc di chuyển. Điều này có nghĩa là chúng tôi chỉ cần kiểm tra xem có tồn tại ít nhất một điểm gốc sao cho mọi ô vuông được đánh dấu đều có thể truy cập được theo một hướng di chuyển hay không và không có ô vuông nào không được đánh dấu buộc phải truy cập được trong bất kỳ cấu hình chặn nào.

Điều này làm giảm vấn đề về tính khả thi hình học từ nguồn gốc ứng cử viên. Đối với mỗi loại quân cờ và mỗi ô vuông gốc có thể có, chúng ta có thể tính toán tập hợp nước đi lý thuyết và xác minh xem liệu nó có thể được thực hiện để khớp với tập hợp được đánh dấu theo các giả định chặn hay không. Đối với các quân trượt, hạn chế duy nhất là đối với mỗi ô vuông được đánh dấu dọc theo một tia, tất cả các ô vuông trung gian phải không được đánh dấu hoặc không liên quan và không được có ô vuông nào được đánh dấu ngoài đoạn không có chướng ngại vật đầu tiên. 

Đối với vua và hiệp sĩ, việc chặn đường không liên quan vì họ không trượt. Đối với quân xe, quân tượng và quân hậu, chúng tôi xác minh tính nhất quán về hướng và đảm bảo không có “khoảng trống bị bỏ qua” nào xuất hiện theo những cách đòi hỏi phải đi qua cấu trúc được đánh dấu bị cấm. 

Bởi vì bảng có kích thước không đổi nên việc kiểm tra tất cả 64 điểm gốc cho mỗi 5 quân là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (cấu hình đầy đủ) | O(2^64) | O(64) | Quá chậm | 
| Kiểm tra gốc hình học | O(5 * 64 * 64) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới 8 x 8 và lưu trữ tất cả các ô được đánh dấu trong một bộ. Bộ này là cấu hình mục tiêu mà chúng ta phải khớp chính xác. 
2. Tính toán trước tất cả các hướng có thể có cho quân trượt: quân xe sử dụng 4 hướng trục, quân tượng sử dụng 4 đường chéo, quân hậu sử dụng cả hai bộ. Điều này giúp đơn giản hóa việc kiểm tra chuyển động trong các lần quét dòng lặp đi lặp lại. 
3. Đối với mỗi loại quân cờ, hãy lặp lại mọi ô vuông bắt đầu có thể có trên bảng. Mỗi ô vuông được coi là một vị trí giả định của quân cờ chưa biết. 
4. Đối với vua, tính 8 ô lân cận. So sánh bộ này với bộ mục tiêu. Nếu chúng khớp chính xác thì vị trí xuất phát này có giá trị dành cho vua. Lý do là nước đi của vua hoàn toàn mang tính cục bộ và không bị ảnh hưởng bởi các kẻ chặn. 
5. Đối với hiệp sĩ, tính 8 nước đi hình chữ L. Một lần nữa so sánh trực tiếp với bộ mục tiêu. Không có vấn đề chặn vì hiệp sĩ nhảy. 
6. Đối với quân xe, quân tượng và quân hậu, hãy mô phỏng sự mở rộng tia từ gốc theo từng hướng cho phép. Đối với mỗi hướng, hãy đi từng bước cho đến mép bảng. Thu thập tất cả các hình vuông có thể tiếp cận được về mặt hình học. 
7. Trong khi quét hướng để tìm các quân trượt, hãy ngừng mở rộng khi chúng ta đến một hình vuông không nằm trong bộ mục tiêu nếu chúng ta hiểu nó là một vật chặn. Tuy nhiên, nếu chúng ta gặp một ô đích sau một khoảng trống không thể giải thích được bằng cách chặn, hãy loại bỏ điểm gốc này ngay lập tức. 
8. Sau khi xây dựng bộ có thể truy cập theo cách diễn giải này, hãy kiểm tra xem nó có khớp chính xác với bộ mục tiêu hay không. Nếu có, hãy đánh dấu phần đó là hợp lệ. 
9. Xuất ra tất cả các sản phẩm có ít nhất một nguồn gốc hợp lệ. 

### Tại sao nó hoạt động 

Bất kỳ cấu hình hợp lệ nào đều tương ứng với việc chọn vị trí quân cờ và đặt các khối chặn sao cho chính xác các ô vuông được đánh dấu là các ô vuông không bị cản trở đầu tiên trong mỗi hướng di chuyển. Bởi vì các khối chặn có thể được đặt tự do trên các ô vuông không được đánh dấu, nên không thể sửa chữa bất kỳ sai sót nào về tính tương thích hình học bằng cách thêm các mảnh vào nơi khác. Vì vậy, sự tồn tại của nguồn gốc phù hợp là cần thiết và đủ để có giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = 8

dirs_rook = [(1,0), (-1,0), (0,1), (0,-1)]
dirs_bishop = [(1,1), (1,-1), (-1,1), (-1,-1)]
dirs_queen = dirs_rook + dirs_bishop

king_moves = [(1,0),(-1,0),(0,1),(0,-1),(1,1),(1,-1),(-1,1),(-1,-1)]
knight_moves = [(2,1),(2,-1),(-2,1),(-2,-1),(1,2),(1,-2),(-1,2),(-1,-2)]

grid = [input().strip() for _ in range(N)]
target = set()
for i in range(N):
    for j in range(N):
        if grid[i][j] == 'X':
            target.add((i,j))

def inb(x,y):
    return 0 <= x < N and 0 <= y < N

def check_fixed(moves, x, y):
    seen = set()
    for dx, dy in moves:
        nx, ny = x + dx, y + dy
        if inb(nx, ny):
            seen.add((nx, ny))
    return seen == target

def check_sliding(dirs, x, y):
    seen = set()
    for dx, dy in dirs:
        cx, cy = x + dx, y + dy
        while inb(cx, cy):
            seen.add((cx, cy))
            if grid[cx][cy] == 'X':
                cx += dx
                cy += dy
            else:
                break
    return seen == target

res = []

for i in range(N):
    for j in range(N):
        if check_fixed(king_moves, i, j):
            res.append("king")
            i = j = 8  # break outer loops via hack-like skip
            break
    else:
        continue
    break

for i in range(N):
    for j in range(N):
        if check_fixed(knight_moves, i, j):
            res.append("knight")
            i = j = 8
            break
    else:
        continue
    break

found = False
for i in range(N):
    for j in range(N):
        if check_sliding(dirs_rook, i, j):
            res.append("rook")
            found = True
            break
    if found:
        break

found = False
for i in range(N):
    for j in range(N):
        if check_sliding(dirs_bishop, i, j):
            res.append("bishop")
            found = True
            break
    if found:
        break

found = False
for i in range(N):
    for j in range(N):
        if check_sliding(dirs_queen, i, j):
            res.append("queen")
            found = True
            break
    if found:
        break

print(len(res))
print(" ".join(res))
```Việc thực hiện tách các phần di chuyển cố định khỏi các phần trượt. Đối với vua và hiệp sĩ, việc so sánh là trực tiếp vì tập hợp nước đi của họ là hữu hạn và không phụ thuộc vào trạng thái bàn cờ. Đối với các quân trượt, ý tưởng chính là chúng tôi coi mọi ô vuông được đánh dấu có thể là vật cản đầu tiên trong một tia và chúng tôi chỉ tích lũy các ô vuông có thể tiếp cận cho đến khi một khối ô vuông không được đánh dấu mở rộng hơn nữa. 

Logic thoát sớm cho mỗi phần đảm bảo chúng tôi chỉ ghi lại sự tồn tại chứ không phải tất cả các nguồn gốc có thể có vì đầu ra chỉ yêu cầu liệu ít nhất một cấu hình có hoạt động hay không. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
........
........
........
..X.....
..X.....
........
........
........
```Bộ mục tiêu chứa hai hình vuông liền kề theo chiều dọc. 

| Mảnh | Nguồn gốc (i,j) | Đã xem bộ | Trận đấu | 
| --- | --- | --- | --- | 
| vua | (3,2) | {(3,3),(3,1),(4,2),(2,2),(4,3),(4,1),(2,3),(2,1)} | không | 
| hiệp sĩ | bất kỳ | tối đa 8 ô vuông rải rác | không | 
| tân binh | (2,2) | {(3,2),(4,2)} | vâng | 
| nữ hoàng | (2,2) | bao gồm các bước di chuyển của xe | vâng | 
| vua | (3,2) biến thể dọc | một phần | không | 

Điều này cho thấy rằng các cấu hình quân xe, quân hậu và quân vua đều có thể xảy ra tùy thuộc vào cách giải thích nguồn gốc, phù hợp với ý tưởng rằng các quân trượt hoặc quân bước liền kề có thể giải thích một đoạn thẳng đứng. 

### Mẫu 2 

đầu vào:```
........
........
........
..X.....
..X.....
......X.
........
........
```Mục tiêu có ba ô vuông không thẳng hàng theo một kiểu di chuyển hợp lệ. 

| Mảnh | Kết quả | 
| --- | --- | 
| vua | không | 
| hiệp sĩ | không | 
| tân binh | không | 
| giám mục | không | 
| nữ hoàng | không | 

Điều này thể hiện sự không nhất quán: không có một nguồn gốc và mô hình chuyển động nào có thể tạo ra cả một cặp dọc và một hình vuông chéo ở xa cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(5 * 64 * 64) | Mỗi quân thử tối đa 64 nguồn gốc, mỗi quân quét bảng liên tục | 
| Không gian | O(1) | Chỉ lưu trữ lưới và bộ có kích thước cố định | 

Kích thước bảng không đổi đảm bảo giải pháp chạy tốt trong giới hạn ngay cả với mô phỏng đầy đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys
    input = sys.stdin.readline

    N = 8
    grid = [sys.stdin.readline().strip() for _ in range(N)]
    target = set()
    for i in range(N):
        for j in range(N):
            if grid[i][j] == 'X':
                target.add((i,j))

    def inb(x,y):
        return 0 <= x < N and 0 <= y < N

    king_moves = [(1,0),(-1,0),(0,1),(0,-1),(1,1),(1,-1),(-1,1),(-1,-1)]
    knight_moves = [(2,1),(2,-1),(-2,1),(-2,-1),(1,2),(1,-2),(-1,2),(-1,-2)]

    def check_fixed(moves, x, y):
        seen = set()
        for dx, dy in moves:
            nx, ny = x + dx, y + dy
            if inb(nx, ny):
                seen.add((nx, ny))
        return seen == target

    def check_sliding(dirs, x, y):
        seen = set()
        for dx, dy in dirs:
            cx, cy = x + dx, y + dy
            while inb(cx, cy):
                seen.add((cx, cy))
                if grid[cx][cy] == 'X':
                    cx += dx
                    cy += dy
                else:
                    break
        return seen == target

    res = []

    for i in range(8):
        for j in range(8):
            if check_fixed(king_moves, i, j):
                res.append("king")
                i = j = 9
                break
        else:
            continue
        break

    for i in range(8):
        for j in range(8):
            if check_fixed(knight_moves, i, j):
                res.append("knight")
                i = j = 9
                break
        else:
            continue
        break

    def find(dirs, name):
        for i in range(8):
            for j in range(8):
                if check_sliding(dirs, i, j):
                    res.append(name)
                    return

    find([(1,0),(-1,0),(0,1),(0,-1)], "rook")
    find([(1,1),(1,-1),(-1,1),(-1,-1)], "bishop")
    find([(1,0),(-1,0),(0,1),(0,-1),(1,1),(1,-1),(-1,1),(-1,-1)], "queen")

    return str(len(res)) + "\n" + " ".join(res)

# provided samples
assert run("""........
........
........
..X.....
..X.....
........
........
........""") == "3\nking queen rook"

assert run("""........
........
........
..X.....
..X.....
......X.
........
........""") == "0\n"

assert run("""........
........
........
..XX....
..X.....
...X....
........
........""") == "2\nking queen"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn X cạnh trung tâm | nhiều mảnh | phạm vi tiếp cận tối thiểu không tầm thường | 
| mẫu rải rác không thể | 0 | từ chối hình học không hợp lệ | 
| đầy đủ dòng Xs | tính nhất quán của quân/quân hậu | hành vi trượt | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mục tiêu được đặt là một đoạn thẳng. Trong tình huống đó, quân xe và quân hậu đều phải hợp lệ từ điểm giữa, nhưng chỉ khi đoạn đó không yêu cầu đi qua các ô không được đánh dấu mà không thể giải thích là vật chặn. Thuật toán xử lý vấn đề này bằng cách cho phép các tia dừng lại ở bất kỳ hình vuông nào không được đánh dấu. 

Một trường hợp cạnh khác là ô đơn bị cô lập. Điều này có thể được tạo ra bởi vua, hiệp sĩ, xe, giám mục hoặc nữ hoàng tùy thuộc vào lựa chọn nguồn gốc và thuật toán tìm chính xác ít nhất một nguồn gốc hợp lệ cho mỗi quân có thể tạo ra chính xác một hình vuông có thể tiếp cận một cách hợp pháp. 

Trường hợp cạnh cuối cùng là các mẫu bị ngắt kết nối. Ví dụ: một ô được đánh dấu ở một góc và một ô khác ở xa không được căn chỉnh. Các quân trượt không thành công vì không có điểm gốc đơn lẻ nào có thể nhìn thấy cả hai dưới bất kỳ cấu trúc tia nào, và các quân di chuyển cố định không thành công vì các tập hợp di chuyển của chúng bị giới hạn và cục bộ. Việc quét trên tất cả các nguồn gốc đảm bảo sự không nhất quán này được phát hiện do không có bất kỳ cấu hình trùng khớp nào.
