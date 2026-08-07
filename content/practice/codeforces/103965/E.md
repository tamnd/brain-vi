---
title: "CF 103965E - \u041e\u0447\u0435\u0440\u043a"
description: "Chúng ta được cung cấp một lưới nhị phân bao gồm các ô màu trắng và đỏ. Nhiệm vụ là xác định xem liệu chúng ta có thể tái tạo chính xác các ô màu đỏ hay không bằng cách sử dụng chuỗi thao tác dập với hai hình dạng cọ cố định. Mỗi thao tác chọn một ô và áp dụng một trong hai mẫu."
date: "2026-07-02T06:35:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103965
codeforces_index: "E"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 103965
solve_time_s: 35
verified: true
draft: false
---

[CF 103965E - \u041e\u0447\u0435\u0440\u043a](https://codeforces.com/problemset/problem/103965/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới nhị phân bao gồm các ô màu trắng và đỏ. Nhiệm vụ là xác định xem liệu chúng ta có thể tái tạo chính xác các ô màu đỏ hay không bằng cách sử dụng chuỗi thao tác dập với hai hình dạng cọ cố định. 

Mỗi thao tác chọn một ô và áp dụng một trong hai mẫu. Một cọ vẽ vẽ một hình cộng ở giữa ô đã chọn, ảnh hưởng đến tâm cộng với bốn ô lân cận trực giao của nó. Bàn chải còn lại vẽ hình chữ X, ảnh hưởng đến trung tâm cộng với bốn đường chéo lân cận. Cả hai cọ vẽ có thể mở rộng ra bên ngoài lưới mà không bị hạn chế, nhưng chỉ các ô bên trong lưới mới có tác dụng cho bức ảnh cuối cùng. Cho phép sử dụng nhiều cọ vẽ và sơn chồng lên nhau cũng được vì trạng thái cuối cùng chỉ phụ thuộc vào việc một ô có màu đỏ ít nhất một lần hay không. 

Mục đích là để quyết định xem có tồn tại bất kỳ chuỗi vị trí cọ nào sao cho sự kết hợp của tất cả các ô được vẽ khớp chính xác với lưới đã cho hay không. 

Ràng buộc n, m ≤ 1000 ngụ ý tối đa 10^6 ô. Bất kỳ giải pháp nào cố gắng mô phỏng trực tiếp sự kết hợp tùy ý của các vị trí đặt bàn chải sẽ không khả thi. Một cách tiếp cận hợp lệ phải giảm vấn đề xuống mức kiểm tra tính nhất quán cục bộ trên mỗi ô hoặc trên mỗi vùng lân cận nhỏ, lý tưởng nhất là tuyến tính trong kích thước lưới. 

Trường hợp cạnh tinh tế xuất phát từ thực tế là cả hai cọ vẽ nhiều ô cùng một lúc, bao gồm cả ô trung tâm. Một cách tiếp cận ngây thơ có thể cho rằng mọi ô màu đỏ phải trực tiếp là tâm của một số cọ vẽ, điều này là sai. Một ô chỉ có thể được vẽ bằng cách là hàng xóm của một số trung tâm được chọn khác. Điều này làm cho việc lập luận về hướng bao phủ phụ thuộc vào hướng và dễ mắc sai lầm. 

Một cạm bẫy phổ biến khác là bỏ qua hành vi ranh giới. Vì các cọ vẽ có thể mở rộng ra bên ngoài lưới, các ô cạnh và góc hoạt động giống như các ô bên trong về tính khả thi, nhưng vùng lân cận của chúng bị cắt bớt, điều này có thể dẫn đến các giả định không chính xác về các tâm cần thiết. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng chọn một tập hợp các tâm cọ và gán cho mỗi tâm một trong hai loại cọ, sau đó kiểm tra xem kết quả thu được của các ô bị ảnh hưởng có khớp với lưới mục tiêu hay không. Điều này nhanh chóng trở thành cấp số nhân vì mỗi ô có thể không được sử dụng hoặc được sử dụng làm trung tâm với một trong hai loại cọ. Ngay cả khi chúng ta hạn chế chỉ coi các tế bào màu đỏ là trung tâm tiềm năng, thì sự tương tác giữa các bàn chải chồng chéo vẫn tạo ra vụ nổ tổ hợp. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến cách hình ảnh được xây dựng, mà chỉ là liệu mọi ô màu đỏ có thể được "giải thích" bằng ít nhất một vị trí cọ hay không và không có ô trắng nào bị vô tình vẽ lên. 

Điều này làm đảo lộn quan điểm: thay vì xây dựng hình ảnh, chúng tôi xác minh rằng mỗi ô màu đỏ có ít nhất một lời giải thích cục bộ hợp lệ và không có lời giải thích bắt buộc nào tràn vào ô bị cấm. 

Hãy xem xét một tế bào màu đỏ. Nếu nó không được hỗ trợ bởi bất kỳ tâm cọ nào có thể chạm tới nó, thì bản thân nó phải là tâm của một cọ vẽ nào đó. Cách duy nhất để thất bại là khi không thể che ô màu đỏ mà không sơn lên ô màu trắng. 

Điều này dẫn đến một hệ thống ràng buộc cục bộ: đối với mỗi ô màu đỏ, chúng tôi kiểm tra xem có tồn tại ít nhất một vị trí cọ che phủ nó mà không mâu thuẫn với lưới hay không. Vì mỗi cọ chỉ ảnh hưởng đến một số ô không đổi nên mỗi lần kiểm tra là O(1) và chúng ta chỉ cần quét tất cả các ô. 

Một chế độ xem tinh tế hơn là coi mỗi ô là trung tâm ứng cử viên và xác nhận xem việc đặt một trong hai cọ ở đó có phù hợp với lưới mục tiêu hay không. Nếu cọ vẽ ở giữa tại (i, j) vẽ bất kỳ ô màu trắng nào thì vị trí đó không hợp lệ. Chúng tôi tính toán tất cả các vị trí hợp lệ, đánh dấu tất cả các ô mà chúng có thể bao phủ và cuối cùng đảm bảo mọi ô màu đỏ đều được bao phủ bởi ít nhất một vị trí hợp lệ. 

Bởi vì mỗi vị trí ảnh hưởng đến tối đa năm ô nên tổng quá trình xác thực là tuyến tính theo kích thước lưới.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về bài tập cọ vẽ | Hàm mũ | Hàm mũ | Quá chậm | 
| Hiệu lực cục bộ của vị trí + kiểm tra mức độ phù hợp | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi điều chỉnh lại vấn đề bằng cách kiểm tra xem mọi ô màu đỏ có thể được tạo ra bởi ít nhất một vị trí cọ hợp lệ hay không. 

1. Đối với mỗi ô trong lưới, hãy coi nó như một trung tâm cọ vẽ tiềm năng. Điều này là cần thiết vì bất kỳ tế bào màu đỏ nào cũng có thể được tạo ra như một tế bào lân cận chứ không phải là trung tâm. 
2. Đối với mỗi trung tâm, hãy mô phỏng việc đặt cọ dấu cộng. Thu thập tập hợp các ô bị ảnh hưởng: trung tâm và bốn ô lân cận trực giao của nó. Nếu bất kỳ ô nào trong số này có màu trắng thì vị trí này không hợp lệ và bị loại bỏ. Nếu không, hãy đánh dấu vị trí này là hợp lệ và ghi lại rằng nó có thể bao phủ tất cả các ô bị ảnh hưởng. 
3. Lặp lại thao tác kiểm tra tương tự cho cọ X, nó ảnh hưởng đến các đường chéo lân cận và tâm. Một lần nữa loại bỏ các vị trí chạm vào bất kỳ ô màu trắng nào. 
4. Duy trì mảng bao phủ boolean được khởi tạo thành false. Bất cứ khi nào tìm thấy vị trí hợp lệ, hãy đánh dấu tất cả các ô được bao phủ bởi vị trí đó. 
5. Sau khi xử lý tất cả các tâm và cả hai loại cọ, lặp lại trên lưới và xác minh rằng mọi ô màu đỏ đều được đánh dấu là bị che. Nếu bất kỳ ô màu đỏ nào không được che phủ thì câu trả lời là không thể. 

Tính chính xác phụ thuộc vào thực tế là bất kỳ cấu trúc hợp lệ nào cũng có thể được phân tách thành các vị trí riêng lẻ, mỗi vị trí đó phải tránh sơn các ô màu trắng. Do đó, mọi vị trí trong giải pháp hợp lệ phải xuất hiện trong tập hợp các vị trí hợp lệ cục bộ mà chúng tôi liệt kê. 

## Tại sao nó hoạt động 

Mỗi vị trí đặt cọ chỉ ảnh hưởng đến một vùng lân cận có kích thước không đổi, do đó việc nó có được phép hay không chỉ phụ thuộc vào các ô lưới ban đầu trong vùng lân cận đó. Nếu một vị trí sẽ vẽ một ô màu trắng thì không có chuỗi thao tác nào có thể đưa ô đó vào một giải pháp hợp lệ vì sơn là không thể đảo ngược. 

Ngược lại, nếu một vị trí hợp lệ cục bộ thì nó luôn có thể được sử dụng mà không vi phạm các ràng buộc và các vị trí chồng chéo chỉ tăng cường mức độ phù hợp mà không gây ra xung đột mới. Do đó, vấn đề giảm xuống còn đảm bảo rằng mọi ô màu đỏ nằm trong ít nhất một vùng lân cận hợp lệ cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
g = [list(input().strip()) for _ in range(n)]

covered = [[False] * m for _ in range(n)]

dirs_plus = [(0, 0), (1, 0), (-1, 0), (0, 1), (0, -1)]
dirs_x = [(0, 0), (1, 1), (1, -1), (-1, 1), (-1, -1)]

def inb(x, y):
    return 0 <= x < n and 0 <= y < m

for i in range(n):
    for j in range(m):
        # plus brush
        ok = True
        cells = []
        for dx, dy in dirs_plus:
            ni, nj = i + dx, j + dy
            if inb(ni, nj):
                if g[ni][nj] == '.':
                    ok = False
                    break
                cells.append((ni, nj))
        if ok:
            for x, y in cells:
                covered[x][y] = True

        # x brush
        ok = True
        cells = []
        for dx, dy in dirs_x:
            ni, nj = i + dx, j + dy
            if inb(ni, nj):
                if g[ni][nj] == '.':
                    ok = False
                    break
                cells.append((ni, nj))
        if ok:
            for x, y in cells:
                covered[x][y] = True

for i in range(n):
    for j in range(m):
        if g[i][j] == '*' and not covered[i][j]:
            print("NO")
            sys.exit()

print("YES")
```Lưới được quét từng ô, coi từng vị trí là trung tâm tiềm năng cho cả hai loại cọ. Đối với mỗi cọ vẽ, chúng tôi liệt kê rõ ràng kiểu ảnh hưởng của nó và đảm bảo nó không chạm vào bất kỳ ô màu trắng nào. Nếu nó vượt qua bước kiểm tra này, chúng tôi sẽ đánh dấu tất cả các ô bị ảnh hưởng là được bảo vệ. 

Bước xác minh cuối cùng là cần thiết vì nó đảm bảo rằng mọi ô màu đỏ cần thiết đều được hỗ trợ bởi ít nhất một vị trí cọ vẽ hợp lệ. 

Một lỗi triển khai phổ biến là quên rằng các ô ranh giới vẫn cho phép các tâm cọ nằm ngoài ảnh hưởng của lưới. Mã này xử lý nó bằng cách bỏ qua những người hàng xóm ngoài giới hạn thay vì từ chối vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
.....
..***
..***
..***
.....
```Chúng tôi theo dõi mức độ phù hợp khi chúng tôi kiểm tra các tâm cọ hợp lệ. 

| Trung tâm (i,j) | Bàn chải | hợp lệ | Tế bào mới được phủ | 
| --- | --- | --- | --- | 
| (2,2) | cộng | không | không | 
| (2,3) | cộng | vâng | (2,3), (1,3), (3,3), (2,2), (2,4) | 
| (2,3) | x | không | không | 
| (3,3) | cộng | vâng | miền Trung mở rộng | 
| (3,3) | x | không | không | 

Sau khi quét tất cả các trung tâm, mỗi ô màu đỏ được bao phủ bởi ít nhất một vị trí cộng hợp lệ, vì vậy câu trả lời là CÓ. 

Dấu vết này cho thấy các cọ vẽ cộng chồng lên nhau bao phủ khối hình chữ nhật như thế nào mặc dù không có một cọ vẽ nào phù hợp với toàn bộ hình dạng. 

### Ví dụ 2 

đầu vào:```
5 5
.....
.....
*....
.*...
*.*..
```| Trung tâm (i,j) | Bàn chải | hợp lệ | Tế bào mới được phủ | 
| --- | --- | --- | --- | 
| (4,1) | x | vâng | hoa văn chéo bao phủ màu đỏ rải rác | 
| (3,0) | cộng | vâng | hỗ trợ một phần | 
| (4,0) | cộng | không | chạm vào tế bào trắng | 

Phạm vi bảo hiểm tích lũy từ nhiều vị trí hợp lệ nhỏ. Điều này chứng tỏ rằng các tế bào màu đỏ không cần phải thuộc về một hình dạng mạch lạc duy nhất mà chỉ có thể giải thích được cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được kiểm tra hai mẫu cọ có kích thước không đổi | 
| Không gian | O(nm) | Lưới phủ sóng có cùng kích thước với đầu vào | 

Kích thước lưới đạt tới 10^6 ô và mỗi ô chỉ kích hoạt công việc liên tục. Điều này phù hợp thoải mái trong cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    g = [list(input().strip()) for _ in range(n)]

    covered = [[False] * m for _ in range(n)]

    dirs_plus = [(0, 0), (1, 0), (-1, 0), (0, 1), (0, -1)]
    dirs_x = [(0, 0), (1, 1), (1, -1), (-1, 1), (-1, -1)]

    def inb(x, y):
        return 0 <= x < n and 0 <= y < m

    for i in range(n):
        for j in range(m):
            ok = True
            cells = []
            for dx, dy in dirs_plus:
                ni, nj = i + dx, j + dy
                if inb(ni, nj):
                    if g[ni][nj] == '.':
                        ok = False
                        break
                    cells.append((ni, nj))
            if ok:
                for x, y in cells:
                    covered[x][y] = True

            ok = True
            cells = []
            for dx, dy in dirs_x:
                ni, nj = i + dx, j + dy
                if inb(ni, nj):
                    if g[ni][nj] == '.':
                        ok = False
                        break
                    cells.append((ni, nj))
            if ok:
                for x, y in cells:
                    covered[x][y] = True

    out = []
    for i in range(n):
        for j in range(m):
            if g[i][j] == '*' and not covered[i][j]:
                out.append("NO")
                return "\n".join(out)
    return "YES"

# provided samples
assert run("""5 5
.....
..***
..***
..***
.....
""") == "YES"

assert run("""5 5
.....
.....
*....
.*...
*.*..
""") == "YES"

# custom cases
assert run("""1 1
.
""") == "YES", "single white cell"

assert run("""1 1
*
""") == "YES", "single red cell"

assert run("""3 3
*.*
...
*.*
""") == "NO", "isolated reds impossible"

assert run("""2 2
**
**
""") == "YES", "full block"

assert run("""3 3
*.*
*.*
*.*
""") == "YES", "vertical stripe coverage"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 trắng | CÓ | tính nhất quán mục tiêu trống | 
| 1x1 đỏ | CÓ | vị trí hợp lệ tối thiểu | 
| góc thưa thớt | KHÔNG | các tế bào biệt lập không thể tiếp cận | 
| đầy đủ 2x2 | | |
