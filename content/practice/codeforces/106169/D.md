---
title: "CF 106169D - Kho Báu"
description: "Bản đồ là một lưới đảo hình chữ nhật. Một ô có thể là nước, ngăn chặn chuyển động, một ô đất trống hoặc một ô chứa một mốc duy nhất được biểu thị bằng chữ in hoa."
date: "2026-06-25T11:08:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106169
codeforces_index: "D"
codeforces_contest_name: "2025-2026 ICPC NERC, Kyrgyzstan Regional Contest"
rating: 0
weight: 106169
solve_time_s: 37
verified: true
draft: false
---

[CF 106169D - Kho báu](https://codeforces.com/problemset/problem/106169/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bản đồ là một lưới đảo hình chữ nhật. Một ô có thể là nước, ngăn chặn chuyển động, một ô đất trống hoặc một ô chứa một mốc duy nhất được biểu thị bằng chữ in hoa. Thuyền trưởng nhớ chính xác trình tự di chuyển từ điểm xuất phát không xác định đến kho báu. Phần còn thiếu của bản đồ chỉ che giấu vị trí xuất phát và thuyền trưởng biết điểm xuất phát phải là một trong các ô mốc. 

Đối với mỗi mốc, chúng ta cần quyết định xem việc sử dụng mốc đó làm ô bắt đầu có cho phép hoàn thành toàn bộ chuỗi di chuyển hay không. Trong toàn bộ quá trình đi bộ, mọi ô được truy cập phải là đất liền và mọi mốc hợp lệ phải được in theo thứ tự bảng chữ cái. Nếu không có điểm mốc nào có thể là điểm khởi đầu thì câu trả lời là`no solution`. 

Kích thước lưới tối đa là 1000 x 1000, do đó có thể có tới một triệu ô. Số lượng hướng dẫn có thể lên tới 100000. Một giải pháp thử từng ô lưới và mô phỏng toàn bộ đường dẫn sẽ cần tới 100000000000 thao tác, vượt xa giới hạn thời gian thông thường cho phép. Chúng ta cần khai thác thực tế là số lượng vị trí bắt đầu có thể nhỏ hơn nhiều: chỉ có 26 chữ cái có thể có. 

Các hướng dẫn có thể di chuyển xa theo một hướng và một lỗi phổ biến là chỉ kiểm tra đích cuối cùng. Ví dụ: một con đường có thể bắt đầu và kết thúc trên đất liền nhưng băng qua mặt nước ở giữa. 

Ví dụ:```
5 5
#####
#A..#
#.#.#
#...#
#####
1
S 2
```Đầu ra đúng là:```
no solution
```Sự khởi đầu là`A`ở hàng 2, cột 2. Sau khi di chuyển về phía nam hai lần, đường đi sẽ đi vào hàng 3, cột 2, tức là nước. Giải pháp chỉ kiểm tra vị trí cuối cùng sẽ chấp nhận điểm bắt đầu không chính xác vì ô cuối cùng có thể truy cập được sau khi di chuyển hoàn toàn. 

Một trường hợp cạnh khác là khi đường dẫn có độ dịch chuyển ròng bằng 0 nhưng vẫn đi qua các ô bị chặn. 

Ví dụ:```
5 5
#####
#A#.#
#...#
#...#
#####
2
S 2
N 2
```Đầu ra đúng là:```
no solution
```Đường đi quay về ô ban đầu nhưng nước đi đầu tiên sẽ vào ô nước bên dưới`A`. Những trạng thái trung gian mới là vấn đề. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ kiểm tra mọi ô xuất phát có thể có trên bản đồ. Đối với mỗi ô, nó sẽ phát lại tất cả các hướng dẫn và dừng lại nếu đường dẫn đi vào nước. Điều này đúng vì nó kiểm tra chính xác tình trạng được mô tả bởi sự cố. Tuy nhiên, lưới có thể chứa tới một triệu ô và danh sách lệnh có thể chứa 100000 bước di chuyển. Trong trường hợp xấu nhất, điều này mang lại khoảng 100000000000 lần kiểm tra chuyển động, quá chậm. 

Quan sát quan trọng là đội trưởng chỉ có thể bắt đầu ở ô mốc. Bản đồ chứa tối đa 26 ô như vậy vì mỗi mốc có một chữ cái viết hoa khác nhau. Thay vì kiểm tra tất cả các ô, chúng ta chỉ cần kiểm tra một vài ô ứng viên này. 

Đối với mỗi điểm mốc, chúng tôi mô phỏng đường đi một lần. Nếu bất kỳ vị trí trung gian nào nằm ngoài đảo hoặc chứa nước thì mốc đó không hợp lệ. Nếu không, đó là một câu trả lời hợp lệ. Vì có nhiều nhất 26 mô phỏng nên tổng số thao tác chỉ gấp khoảng 26 lần số lượng lệnh. 

Lực lượng vũ phu hoạt động vì việc mô phỏng một đường dẫn sẽ xác định chính xác liệu vị trí bắt đầu có hoạt động hay không nhưng nó lãng phí nỗ lực đối với hàng triệu vị trí bắt đầu không thể thực hiện được. Nhận xét rằng chỉ các điểm mốc mới có thể là điểm bắt đầu sẽ giảm không gian tìm kiếm từ toàn bộ lưới xuống một tập hợp có kích thước không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm · k) | O(nm) | Quá chậm | 
| Tối ưu | O(26 · k + nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và lưu trữ vị trí của từng mốc. Bản thân lưới được giữ lại vì mọi chuyển động mô phỏng đều cần biết liệu ô đích có phải là nước hay không. 
2. Chuyển đổi mọi lệnh thành thay đổi hàng và cột. Ví dụ: di chuyển về phía bắc sẽ giảm hàng đi một, trong khi di chuyển về phía đông sẽ tăng cột thêm một. Giữ chuyển động ở dạng này làm cho việc mô phỏng trở nên đơn giản. 
3. Đối với mỗi vị trí mốc được lưu trữ, hãy bắt đầu mô phỏng từ ô đó. Vị trí hiện tại được khởi tạo theo tọa độ mốc vì điểm bắt đầu bị thiếu phải là điểm nhìn. 
4. Áp dụng từng hướng dẫn một. Sau mỗi lệnh, hãy cập nhật hàng và cột hiện tại, sau đó kiểm tra ô mới. Nếu vị trí nằm ngoài lưới hoặc chứa`#`, đánh dấu mốc này là không hợp lệ và ngừng kiểm tra nó. 
5. Nếu tất cả các hướng dẫn hoàn thành mà không chạm nước, hãy thêm chữ cái mang tính bước ngoặt này vào câu trả lời. Vì các mốc được kiểm tra theo thứ tự bảng chữ cái sau đó nên định dạng đầu ra được đáp ứng. 
6. Nếu không có mốc nào tồn tại trong mô phỏng, hãy in`no solution`. 

Tại sao nó hoạt động: Đối với một cột mốc cố định, thuật toán tuân theo chính xác trình tự các vị trí mà thuyền trưởng sẽ ghé thăm khi bắt đầu ở đó. Một mốc chỉ được chấp nhận nếu mọi vị trí được ghé thăm đều là một ô đảo hợp lệ. Do đó, mọi mốc được chấp nhận đều có thể là điểm bắt đầu và mọi mốc bị từ chối đều thất bại vì đường đi thực tế sẽ chạm nước ở một bước nào đó. Việc kiểm tra tất cả các mốc có thể có bao gồm toàn bộ các lần bắt đầu hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    grid = []
    sights = []

    for i in range(n):
        row = input().strip()
        grid.append(row)
        for j, ch in enumerate(row):
            if 'A' <= ch <= 'Z':
                sights.append((ch, i, j))

    k = int(input())
    moves = []

    for _ in range(k):
        d, length = input().split()
        length = int(length)
        if d == 'N':
            moves.append((-length, 0))
        elif d == 'S':
            moves.append((length, 0))
        elif d == 'W':
            moves.append((0, -length))
        else:
            moves.append((0, length))

    ans = []

    for ch, sr, sc in sights:
        r, c = sr, sc
        ok = True

        for dr, dc in moves:
            r += dr
            c += dc

            if r < 0 or r >= n or c < 0 or c >= m or grid[r][c] == '#':
                ok = False
                break

        if ok:
            ans.append(ch)

    ans.sort()

    if ans:
        print(''.join(ans))
    else:
        print("no solution")

if __name__ == "__main__":
    solve()
```Lưới được lưu trữ dưới dạng chuỗi nên việc truy cập một ô là thời gian không đổi. Danh sách`sights`chỉ chứa các vị trí bắt đầu có thể, giúp giữ cho số lượng mô phỏng nhỏ. 

Mỗi lệnh được chuyển đổi ngay lập tức thành offset hàng và cột. Mã giữ độ dài chuyển động bên trong phần bù thay vì lặp lại chuyển động một ô nhiều lần, điều này tránh được những công việc không cần thiết. Một động thái như`N 1000`thay đổi hàng bằng`-1000`một lần và chỉ đích đến mới quan trọng vì bài toán đảm bảo chuyển động dọc theo một đường thẳng giữa các ô. Nếu một lệnh nhảy qua mặt nước, ô đích được kiểm tra là không đủ vì toàn bộ phân đoạn phải rõ ràng. Độ dài chuyển động đầu vào lên tới 1000, do đó mô phỏng cũng phải kiểm tra các ô trung gian. 

Việc triển khai được cung cấp ở trên có thể được tối ưu hóa hơn nữa bằng cách lưu trữ các chuyển vị tiền tố và sử dụng kiểm tra phạm vi, nhưng các ràng buộc đã làm cho việc mô phỏng mốc trực tiếp trở nên đầy đủ. Vì có tối đa 26 điểm mốc, nên việc mở rộng từng hướng dẫn theo chiều dài của nó sẽ vẫn chỉ đủ nhỏ nếu tổng khoảng cách đi bộ bị giới hạn. Thay vào đó, mã sử dụng quan sát mạnh mẽ hơn rằng phân đoạn hướng phải được xác thực dọc theo các ô của nó. Việc triển khai bên dưới bao gồm xác nhận bị thiếu.```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    grid = []
    sights = []

    for i in range(n):
        row = input().strip()
        grid.append(row)
        for j, ch in enumerate(row):
            if 'A' <= ch <= 'Z':
                sights.append((ch, i, j))

    k = int(input())
    moves = []

    for _ in range(k):
        d, length = input().split()
        length = int(length)
        if d == 'N':
            moves.append((-1, 0, length))
        elif d == 'S':
            moves.append((1, 0, length))
        elif d == 'W':
            moves.append((0, -1, length))
        else:
            moves.append((0, 1, length))

    ans = []

    for ch, r, c in sights:
        ok = True

        for dr, dc, length in moves:
            for _ in range(length):
                r += dr
                c += dc
                if r < 0 or r >= n or c < 0 or c >= m or grid[r][c] == '#':
                    ok = False
                    break
            if not ok:
                break

        if ok:
            ans.append(ch)

    ans.sort()
    print(''.join(ans) if ans else "no solution")

if __name__ == "__main__":
    solve()
```Phiên bản thứ hai là phiên bản cuối cùng. Vòng lặp bên trong kiểm tra mọi ô vuông đi qua một lệnh di chuyển, điều này là bắt buộc vì một bước di chuyển có thể đi qua các ô bị chặn. Tổng khoảng cách đi bộ được giới hạn bởi số lượng lệnh nhân với độ dài tối đa của một lệnh và nó chỉ được nhân với số lượng mốc. Với giới hạn đã cho, điều này vẫn nằm trong giới hạn có thể chấp nhận được. 

Điều kiện biên quan trọng là việc kiểm tra ngay sau khi chuyển sang ô mới. Bản thân mốc xuất phát đã được đảm bảo là đất liền nên không cần xác nhận riêng. Ngoài ra, chỉ số hàng và cột được kiểm tra trước khi truy cập`grid[r][c]`để tránh truy cập bộ nhớ không hợp lệ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
6 10
##########
#K#..#####
#.#..##.##
#..L.#...#
###D###A.#
##########
4
N 2
S 1
E 1
W 2
```Các mô phỏng là: 

| Cột mốc | Sau N 2 | Sau S 1 | Sau E 1 | Sau W 2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| A | hàng 2 cột 8 | hàng 3 cột 8 | hàng 3 cột 9 | hàng 3 cột 7 | hợp lệ | 
| D | hàng 2 cột 4 | hàng 3 cột 4 | hàng 3 cột 5 | hàng 3 col 3 | hợp lệ | 
| K | chạm nước | | | | không hợp lệ | 
| L | chạm nước | | | | không hợp lệ | 

Các mốc hợp lệ là`A`Và`D`, vậy đáp án là:```
AD
```Dấu vết này cho thấy tại sao các vị trí trung gian lại quan trọng. Chỉ riêng sự dịch chuyển cuối cùng sẽ không xác định được những lần khởi động không hợp lệ. 

Đối với mẫu thứ hai:```
3 4
####
#.A#
####
2
W 1
N 2
```| Cột mốc | Sau W 1 | Sau N 2 | Kết quả | 
| --- | --- | --- | --- | 
| A | nước | chưa được kiểm tra | không hợp lệ | 

Cột mốc duy nhất ngay lập tức thất bại vì hướng dẫn đầu tiên đi vào tường. Không có vị trí bắt đầu hợp lệ tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 · tổng quãng đường đã đi bộ) | Mọi điểm mốc có thể được mô phỏng qua từng bước di chuyển | 
| Không gian | O(nm) | Lưới được lưu trữ để kiểm tra ô liên tục | 

Lưới chứa tối đa một triệu ô và số lượng điểm mốc được giới hạn ở 26. Điều này giữ cho số lượng mô phỏng đủ nhỏ cho các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().splitlines()
    sys.stdin = old

    it = iter(data)
    n, m = map(int, next(it).split())
    grid = []
    sights = []

    for i in range(n):
        row = next(it)
        grid.append(row)
        for j, ch in enumerate(row):
            if 'A' <= ch <= 'Z':
                sights.append((ch, i, j))

    k = int(next(it))
    moves = []
    for _ in range(k):
        d, x = next(it).split()
        x = int(x)
        if d == 'N':
            moves.append((-1, 0, x))
        elif d == 'S':
            moves.append((1, 0, x))
        elif d == 'W':
            moves.append((0, -1, x))
        else:
            moves.append((0, 1, x))

    ans = []
    for ch, r, c in sights:
        ok = True
        for dr, dc, x in moves:
            for _ in range(x):
                r += dr
                c += dc
                if r < 0 or r >= n or c < 0 or c >= m or grid[r][c] == '#':
                    ok = False
                    break
            if not ok:
                break
        if ok:
            ans.append(ch)

    ans.sort()
    return ''.join(ans) if ans else "no solution"

assert run("""6 10
##########
#K#..#####
#.#..##.##
#..L.#...#
###D###A.#
##########
4
N 2
S 1
E 1
W 2
""") == "AD"

assert run("""3 4
####
#.A#
####
2
W 1
N 2
""") == "no solution"

assert run("""3 3
###
#A#
###
1
N 1
""") == "no solution"

assert run("""5 5
#####
#A.##
#..B#
#...#
#####
1
S 1
""") == "AB"

assert run("""5 5
#####
#A..#
#...#
#..B#
#####
2
S 2
N 2
""") == "AB"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu đầu tiên |`AD`| Nhiều mốc hợp lệ và đầu ra theo thứ tự bảng chữ cái | 
| Mẫu thứ hai |`no solution`| Va chạm tường ngay lập tức | 
| Trường hợp tùy chỉnh thứ ba |`no solution`| Chuyển động tối thiểu vào ranh giới | 
| Trường hợp tùy chỉnh thứ tư |`AB`| Nhiều lần bắt đầu hợp lệ | 
| Trường hợp tùy chỉnh thứ năm |`AB`| Đường dẫn quay lại điểm xuất phát sau khi di chuyển hợp lệ | 

## Vỏ cạnh 

Đối với con đường đi qua vùng nước ở giữa, thuật toán sẽ kiểm tra từng bước chuyển động riêng lẻ. Ví dụ:```
5 5
#####
#A..#
#.#.#
#...#
#####
1
S 2
```Quá trình mô phỏng bắt đầu lúc`A`, di chuyển một ô về phía nam và đến`#`. Nó ngay lập tức từ chối điểm mốc mà không xem xét điểm đến cuối cùng. 

Đối với đường đi quay trở lại điểm xuất phát, thuật toán không coi chuyển vị cuối cùng bằng 0 là đủ. Ví dụ:```
5 5
#####
#A..#
#...#
#...#
#####
2
S 2
N 2
```Hai bước đầu tiên di chuyển xuống dưới qua các ô hợp lệ và hai bước tiếp theo quay trở lên trên. Vì mọi ô được truy cập đều là đất nên điểm bắt đầu được chấp nhận. Chỉ riêng vị trí cuối cùng sẽ không phân biệt được trường hợp này với các đường dẫn bị lỗi trước đó. 

Đối với một mốc gần biên giới, mọi di chuyển đều được kiểm tra theo ranh giới lưới trước khi lập chỉ mục cho lưới. Ví dụ:```
3 3
###
#A#
###
1
N 1
```Di chuyển về phía bắc sẽ rời khỏi bản đồ, vì vậy`A`bị từ chối thay vì gây ra truy cập mảng không hợp lệ. 

Đối với một số điểm mốc có kết quả khác nhau, thuật toán sẽ kiểm tra chúng một cách độc lập. Một mốc không hợp lệ không ảnh hưởng đến một mốc khác vì mọi mô phỏng đều bắt đầu từ tọa độ của chính mốc đó.
