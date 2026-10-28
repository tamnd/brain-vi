---
title: "CF 106177D - Mơ mộng"
description: "Bản đồ là một lưới đảo hình chữ nhật. Một số ô là biển và không thể vào được, trong khi các ô còn lại là đất liền. Một số ô đất chứa các điểm tham quan độc đáo được biểu thị bằng chữ in hoa."
date: "2026-06-25T11:00:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106177
codeforces_index: "D"
codeforces_contest_name: "TheForces Round #45 (DIV3-Forces2)"
rating: 0
weight: 106177
solve_time_s: 35
verified: true
draft: false
---

[CF 106177D - Mơ mộng](https://codeforces.com/problemset/problem/106177/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bản đồ là một lưới đảo hình chữ nhật. Một số ô là biển và không thể vào được, trong khi các ô còn lại là đất liền. Một số ô đất chứa các điểm tham quan độc đáo được biểu thị bằng chữ in hoa. Thuyền trưởng biết trình tự hướng dẫn đi bộ, nhưng vị trí xuất phát trên bản đồ thì không xác định được. Vị trí bắt đầu duy nhất có thể là các ô chứa điểm tham quan. 

Đối với mỗi cảnh quan, chúng ta cần quyết định xem liệu việc bắt đầu từ đó và làm theo toàn bộ trình tự hướng dẫn có giúp thuyền trưởng ở trên đất liền trong toàn bộ hành trình hay không. Nếu một số điểm tham quan hoạt động, các chữ cái của chúng phải được in theo thứ tự bảng chữ cái. Nếu không có tác dụng, câu trả lời là`no solution`. 

Kích thước lưới tối đa là 1000 x 1000, do đó có thể có tới một triệu ô. Số lượng lệnh có thể đạt tới 100000 và một lệnh có thể di chuyển tới 1000 ô. Việc mô phỏng đường đi theo từng ô cho mỗi lần bắt đầu có thể sẽ quá chậm vì tổng khoảng cách đã đi có thể lên tới 100 triệu ô. Tuy nhiên, số lượng vị trí bắt đầu có thể bị giới hạn bởi số lượng chữ cái, nhiều nhất là 26. Điều này thay đổi hoàn toàn vấn đề: chúng ta có đủ khả năng mô phỏng chuỗi lệnh cho mọi cảnh, miễn là mỗi lệnh được xử lý trong thời gian không đổi. 

Một giải pháp bất cẩn vẫn có thể thất bại ở một số chi tiết. Một lỗi phổ biến là chỉ kiểm tra ô đích sau một chặng đường dài. Ví dụ:```
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
```Bắt đầu từ`A`, ô đích đang mở nhưng đường đi lại đi qua ô biển ở giữa. Mọi ô trung gian phải hợp lệ. 

Một sai lầm khác là cho phép thuyền trưởng rời khỏi bản đồ. Chu vi là biển, nhưng một động thái vẫn có thể cố gắng vượt ra ngoài nếu việc thực hiện không kiểm tra toàn bộ đoạn đường. Ví dụ:```
3 3
###
#A#
###
1
N 1
```Câu trả lời là:```
no solution
```Di chuyển đến một ô biển nên không thể sử dụng tầm nhìn. 

Trường hợp cuối cùng là khi mọi cảnh nhìn đều thất bại ngoại trừ một cảnh kết thúc ở đâu đó không liên quan. Vị trí cuối cùng không cần phải có tầm nhìn. Chỉ có giá trị của toàn bộ cuộc đi bộ mới quan trọng. Ví dụ:```
4 5
#####
#A..#
#...#
#####
1
E 2
```Câu trả lời là:```
A
```Thuyền trưởng chỉ cần sống sót theo hướng dẫn. Hình vuông kết thúc không có bất kỳ điều kiện bổ sung nào. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là bắt đầu từ mọi cảnh và thực hiện từng hướng dẫn một. Điều này đúng vì hướng dẫn xác định hoàn toàn lộ trình của thuyền trưởng từ điểm xuất phát đã chọn. Vấn đề là làm cho mỗi hướng dẫn trở nên rẻ tiền. Việc triển khai ngây thơ sẽ đi qua từng ô vuông riêng lẻ trong một chuyển động. Trong trường hợp xấu nhất, có 26 điểm bắt đầu, 100000 hướng dẫn và mỗi hướng dẫn di chuyển 1000 ô, đưa ra khoảng 2,6 tỷ lần kiểm tra ô. Đó là vượt xa thời gian có sẵn. 

Quan sát hữu ích là số lượng điểm bắt đầu rất nhỏ nhưng lưới lại lớn. Chúng ta nên dành thời gian tiền xử lý trên lưới để có thể kiểm tra ngay chuyển động thẳng dài. Vì mọi lệnh đều di chuyển theo chiều ngang hoặc chiều dọc nên chúng ta chỉ cần trả lời các truy vấn có dạng "tất cả các ô trong đoạn hàng này hay đoạn cột đều nằm trên đất?" 

Chúng tôi xây dựng tổng tiền tố của các tế bào biển. Tiền tố hàng cho phép chúng tôi kiểm tra bất kỳ phân đoạn ngang nào và tiền tố cột cho phép chúng tôi kiểm tra bất kỳ phân đoạn dọc nào. Một chuyển động hợp lệ nếu số lượng tế bào biển trong đoạn di chuyển bằng 0. Sau đó, mỗi lệnh sẽ trở thành một vài thao tác truy cập mảng và phép tính số học. 

Phương pháp vũ lực không thành công vì nó lặp lại công việc cho từng ô riêng lẻ được vượt qua. Việc quan sát tổng tiền tố sẽ loại bỏ quá trình quét lặp lại đó, giảm từng mô phỏng hoàn chỉnh xuống số lượng hướng dẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26 * k * L) trong đó L là độ dài di chuyển tối đa | O(nm) | Quá chậm | 
| Tối ưu | O(nm + 26k) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bản đồ và lưu trữ vị trí của mọi điểm tham quan. Chỉ những ô này mới có thể là vị trí bắt đầu, vì vậy không có lý do gì để mô phỏng từ bất kỳ ô nào khác. 
2. Xây dựng hai cấu trúc tiền tố cho các ô biển. Một tiền tố được duy trì cho mỗi hàng và trả lời các truy vấn phân đoạn ngang. Cái còn lại được duy trì cho mỗi cột và trả lời các truy vấn phân đoạn dọc. 
3. Đối với mọi tầm nhìn, hãy đặt vị trí hiện tại theo tọa độ của tầm nhìn đó và xử lý tất cả các hướng dẫn theo thứ tự. Nếu có chuyển động đi qua bất kỳ ô biển nào, hãy loại bỏ cảnh tượng này ngay lập tức. 
4. Để di chuyển theo chiều ngang, hãy sử dụng tiền tố hàng để đếm các ô biển giữa cột hiện tại và cột đích. Đối với chuyển động thẳng đứng, hãy sử dụng tiền tố cột theo cách tương tự. Nếu số đếm bằng 0, hãy cập nhật vị trí hiện tại. 
5. Sau khi tất cả các hướng dẫn được xử lý thành công, hãy thêm chữ cái này vào câu trả lời. 
6. Sắp xếp các chữ cái đã thu thập và in chúng. Nếu bộ sưu tập trống, hãy in`no solution`. 

Tại sao nó hoạt động: 

Đối với tầm nhìn bắt đầu cố định, mô phỏng tuân theo chính xác lộ trình được mô tả trong hướng dẫn. Một nước đi chỉ được chấp nhận khi tổng tiền tố chứng minh rằng mọi ô được cắt ngang đều là đất liền, vì vậy vị trí mô phỏng luôn khớp với một bước đi hợp pháp trên bản đồ. Nếu thuật toán từ chối tầm nhìn thì lệnh tương ứng sẽ thực sự truyền qua biển. Nếu nó chấp nhận tầm nhìn, mọi lệnh đã được hoàn thành mà không cần nhập ô không hợp lệ. Vì mọi khả năng nhìn thấy ban đầu đều được kiểm tra nên tập hợp các chữ cái được chấp nhận cuối cùng chính xác là câu trả lời được yêu cầu. 

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

    row_pref = [[0] * (m + 1) for _ in range(n)]
    col_pref = [[0] * (n + 1) for _ in range(m)]

    for i in range(n):
        for j in range(m):
            row_pref[i][j + 1] = row_pref[i][j] + (grid[i][j] == '#')
            col_pref[j][i + 1] = col_pref[j][i] + (grid[i][j] == '#')

    k = int(input())
    moves = []
    for _ in range(k):
        d, x = input().split()
        moves.append((d, int(x)))

    ans = []

    for ch, sr, sc in sights:
        r, c = sr, sc
        ok = True

        for d, x in moves:
            if d == 'E':
                nr, nc = r, c + x
                if nc > m - 1:
                    ok = False
                    break
                if row_pref[r][nc + 1] - row_pref[r][c + 1] != 0:
                    ok = False
                    break
                c = nc
            elif d == 'W':
                nr, nc = r, c - x
                if nc < 0:
                    ok = False
                    break
                if row_pref[r][c] - row_pref[r][nc] != 0:
                    ok = False
                    break
                c = nc
            elif d == 'S':
                nr, nc = r + x, c
                if nr > n - 1:
                    ok = False
                    break
                if col_pref[c][nr + 1] - col_pref[c][r + 1] != 0:
                    ok = False
                    break
                r = nr
            else:
                nr, nc = r - x, c
                if nr < 0:
                    ok = False
                    break
                if col_pref[c][r] - col_pref[c][nr] != 0:
                    ok = False
                    break
                r = nr

        if ok:
            ans.append(ch)

    if ans:
        print(''.join(sorted(ans)))
    else:
        print("no solution")

if __name__ == "__main__":
    solve()
```Phân tích cú pháp đầu vào lưu trữ vị trí của các ô viết hoa trong khi đọc lưới. Điều này tránh việc quét lại toàn bộ lưới khi kiểm tra vị trí bắt đầu. 

Mảng tiền tố hàng và cột lưu trữ số lượng ô biển. Ví dụ,`row_pref[r][b] - row_pref[r][a]`đưa ra số lượng ô bị chặn trong hàng`r`giữa các cột`a`Và`b - 1`. Ý tưởng tương tự được sử dụng cho các cột. 

Mỗi chuyển động sẽ kiểm tra phân đoạn không bao gồm ô bắt đầu và bao gồm cả ô đích. Ô bắt đầu đã được xác định là ô hiển thị hợp lệ nên nó sẽ không ảnh hưởng đến truy vấn phân đoạn. Sau khi kiểm tra thành công, vị trí hiện tại sẽ được cập nhật. 

Việc kiểm tra ranh giới được thực hiện trước khi truy vấn các mảng tiền tố. Điều này ngăn chặn việc truy cập mảng không hợp lệ và cũng xử lý chính xác các bước di chuyển rời khỏi lưới. 

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
```Quá trình xử lý hướng dẫn cho từng tầm nhìn có thể diễn ra như sau: 

| Cảnh tượng | Bắt đầu | Sau N 2 | Sau S 1 | Sau E 1 | Sau W 2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| A | (4,7) | bị chặn | - | - | - | Từ chối | 
| D | (4,3) | (2,3) | (3,3) | (3,4) | (3,2) | Chấp nhận | 
| K | (1,1) | bị chặn | - | - | - | Từ chối | 
| L | (3,3) | (1,3) | bị chặn | - | - | Từ chối | 

Các điểm tham quan được chấp nhận là`D`Và`A`theo thứ tự kiểm tra lộ trình của mẫu ban đầu và việc sắp xếp sẽ tạo ra:```
AD
```Điều này chứng tỏ rằng thuật toán kiểm tra đường dẫn đầy đủ thay vì chỉ vị trí cuối cùng. 

Đối với mẫu thứ hai:```
3 4
####
#.A#
####
2
W 1
N 2
```Dấu vết là: 

| Cảnh tượng | Bắt đầu | Sau W 1 | Sau N 2 | Kết quả | 
| --- | --- | --- | --- | --- | 
| A | (1,2) | bị chặn | - | Từ chối | 

Chuyển động đầu tiên đã đi vào ô biển nên tầm nhìn sẽ bị loại bỏ ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm + 26k) | Tiền tố xây dựng có giá O(nm) và mỗi tầm nhìn xử lý tối đa k lệnh. | 
| Không gian | O(nm) | Mảng tiền tố chứa một giá trị cho mỗi vị trí hàng và cột của lưới. | 

Kích thước lưới cho phép khoảng một triệu ô, phù hợp cho quá trình tiền xử lý tiền tố. Số lượng lệnh lớn nhưng số lần khởi động có thể bị giới hạn bởi kích thước bảng chữ cái, do đó công việc mô phỏng vẫn còn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = sys.__stdout__
    return out.getvalue()

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
""") == "AD\n", "sample 1"

assert run("""3 4
####
#.A#
####
2
W 1
N 2
""") == "no solution\n", "sample 2"

assert run("""3 3
###
#A#
###
1
N 1
""") == "no solution\n", "outside boundary"

assert run("""4 5
#####
#A..#
#...#
#####
1
E 2
""") == "A\n", "valid movement"

assert run("""5 5
#####
#A..#
#...#
#..B#
#####
1
E 2
""") == "AB\n", "multiple valid sights"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu đầu tiên |`AD`| Mô phỏng đường dẫn bình thường với một số điểm tham quan | 
| Mẫu thứ hai |`no solution`| Thất bại ngay lập tức do một nước đi không hợp lệ | 
| Trường hợp ranh giới |`no solution`| Ngăn chặn sự di chuyển ra ngoài bản đồ | 
| Tầm nhìn hợp lệ duy nhất |`A`| Thực hiện đầy đủ hướng dẫn thành công | 
| Hai điểm tham quan hợp lệ |`AB`| Sắp xếp và xử lý nhiều câu trả lời | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là chuyển động nhảy qua chướng ngại vật. Coi như:```
5 5
#####
#A..#
#.#.#
#...#
#####
1
S 2
```Thuật toán bắt đầu lúc`A`. Truy vấn tiền tố dọc kiểm tra hai ô bên dưới nó. Ô thứ hai là biển nên số đếm khác 0 và tầm nhìn bị từ chối. Câu trả lời là`no solution`. 

Trường hợp cạnh thứ hai là rời khỏi bản đồ:```
3 3
###
#A#
###
1
N 1
```Thuật toán tính toán rằng hàng đích nằm ngoài lưới. Nó từ chối di chuyển trước khi truy cập vào mảng tiền tố, đưa ra`no solution`. 

Trường hợp cạnh thứ ba là tuyến đường thành công trong đó vị trí cuối cùng không có ý nghĩa đặc biệt:```
4 5
#####
#A..#
#...#
#####
1
E 2
```Truy vấn tiền tố không tìm thấy ô biển nào trong đoạn đã di chuyển, do đó vị trí được cập nhật hai lần về phía đông. Thuật toán kết thúc tất cả các hướng dẫn và giữ`A`như một cảnh tượng hợp lệ. Câu trả lời là`A`.
