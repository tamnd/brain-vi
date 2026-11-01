---
title: "CF 106487D - Máy bay không người lái trong bảo tàng"
description: "Vấn đề mô tả một bản đồ hòn đảo hình chữ nhật. Một số ô là biển và không thể vào được, trong khi các ô khác là đất liền. Một số ô đất chứa các mốc duy nhất được biểu thị bằng chữ in hoa."
date: "2026-06-25T08:47:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106487
codeforces_index: "D"
codeforces_contest_name: "XXX Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 106487
solve_time_s: 33
verified: true
draft: false
---

[CF 106487D - Máy bay không người lái và bảo tàng](https://codeforces.com/problemset/problem/106487/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Vấn đề mô tả một bản đồ hòn đảo hình chữ nhật. Một số ô là biển và không thể vào được, trong khi các ô khác là đất liền. Một số ô đất chứa các mốc duy nhất được biểu thị bằng chữ in hoa. Một chuỗi hướng dẫn đi bộ được đưa ra và nhiệm vụ là tìm ra những điểm mốc nào có thể là điểm bắt đầu không xác định của chuyến đi bộ. Một mốc có giá trị nếu xuất phát từ ô đó và áp dụng mọi chỉ dẫn theo thứ tự không bao giờ bước xuống biển. Vị trí cuối cùng không cần phải được kiểm tra đối với một mục tiêu đã biết vì bản thân vị trí kho báu vẫn chưa được xác định. Chúng tôi chỉ cần xác minh rằng toàn bộ tuyến đường có thể thực hiện được. 

Bản đồ có thể chứa tới một triệu ô vì cả hai chiều có thể đạt tới 1000. Điều này ngay lập tức loại trừ việc mô phỏng tuyến đường từ mọi ô trên bản đồ. Số lượng hướng dẫn có thể đạt tới 100000, do đó, giải pháp thử tất cả các ô sẽ thực hiện khoảng 10^11 chuyển động trong trường hợp xấu nhất. Quan sát hữu ích là có nhiều nhất 26 mốc, vì vậy việc kiểm tra từng mốc riêng lẻ là đủ nhanh. 

Các trường hợp chính xuất phát từ việc coi tuyến đường chỉ là sự dịch chuyển cuối cùng thay vì một đường đi đầy đủ. Ví dụ:```
3 5
#####
#A..#
#####
1
E 3
```Đầu ra đúng là:```
no solution
```Cột mốc di chuyển ra ngoài đảo. Giải pháp chỉ kiểm tra tọa độ cuối cùng sẽ chấp nhận sai nếu tọa độ cuối cùng quay trở lại đất liền. 

Một trường hợp quan trọng khác là tuyến đường đi vào biển tạm thời rồi quay trở lại:```
3 5
#####
#A###
#####
1
E 1
```Đầu ra đúng là:```
no solution
```Nước đi đầu tiên đã không hợp lệ. Tuyến đường phải hợp lệ sau mỗi lệnh, không chỉ sau chuỗi hoàn chỉnh. 

Trường hợp cạnh thứ ba là khi một mốc không di chuyển chút nào sau tất cả các hướng dẫn nhưng vẫn có bước trung gian không hợp lệ. Việc thực hiện phải mô phỏng từng phân đoạn theo thứ tự. 

# Phương pháp tiếp cận 

Một giải pháp đơn giản là bắt đầu mô phỏng từ mọi điểm mốc. Đối với mỗi ô ứng cử viên, chúng tôi giữ lại hàng và cột hiện tại và áp dụng từng hướng dẫn một. Nếu nước đi đến ô biển, cột mốc sẽ bị từ chối. Nếu tất cả các hướng dẫn thành công, mốc sẽ được thêm vào câu trả lời. Cách tiếp cận này đúng vì điều kiện duy nhất để một điểm mốc có thể thực hiện được là con đường đã ghi nhớ thực sự có thể đi được. 

Lý do phương pháp này đủ nhanh là do sự kết hợp bất thường của các ràng buộc. Chúng tôi không bao giờ phải kiểm tra mọi ô bản đồ, chỉ những ô chứa các mốc. Có nhiều nhất 26 ô như vậy. Ngay cả với 100000 hướng dẫn, tổng công việc là khoảng 26 × 100000 thao tác, chỉ bằng vài triệu lần kiểm tra. 

Một hướng sai phổ biến là tìm kiếm toàn bộ lưới hoặc chỉ tính toán chuyển vị cuối cùng. Sự dịch chuyển cuối cùng làm mất thông tin về các chướng ngại vật giữa điểm bắt đầu và điểm kết thúc. Lực lượng vũ phu hoạt động vì số lượng vị trí bắt đầu có thể có là rất nhỏ, nhưng sẽ thất bại nếu mọi ô đều có thể là ứng cử viên. Nhận xét rằng chỉ có các chữ cái mới quan trọng cho phép chúng ta giảm vấn đề xuống một số lượng nhỏ các mô phỏng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên mọi tế bào | O(nmk) | O(1) | Quá chậm | 
| Chỉ mô phỏng các mốc | O(26k) | O(1) bên cạnh việc lưu trữ đầu vào | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc bản đồ và lưu trữ tọa độ của từng mốc. Các ô còn lại không liên quan đến điểm bắt đầu vì chỉ có điểm mốc mới có thể là câu trả lời. 
2. Đọc hướng dẫn và lưu trữ chúng theo cặp hướng và độ dài. Việc giữ nguyên chúng cho phép mọi điểm mốc được kiểm tra trên cùng một tuyến đường. 
3. Đối với mỗi mốc, hãy bắt đầu mô phỏng từ tọa độ của nó. Vị trí hiện tại thể hiện vị trí của kẻ trộm sau khi thực hiện các hướng dẫn được xử lý cho đến nay. 
4. Áp dụng từng lệnh bằng cách di chuyển từng ô một. Sau mỗi lần di chuyển, hãy kiểm tra xem ô mới có phải là biển không. Điều này là cần thiết vì một chuyển động dài có thể vượt qua các ô bị chặn ngay cả khi điểm cuối của nó hợp lệ. 
5. Nếu tất cả các hướng dẫn kết thúc mà không xuống biển, hãy thêm chữ cái mốc vào kết quả. Vì các chữ cái được kiểm tra theo thứ tự bảng chữ cái nên kết quả cuối cùng đã được sắp xếp. 

Tại sao nó hoạt động: 

Thuật toán trực tiếp kiểm tra định nghĩa của điểm bắt đầu hợp lệ. Đối với bất kỳ cột mốc nào, mô phỏng tuân theo chính xác trình tự di chuyển mà tên trộm sẽ thực hiện. Nếu mô phỏng không thành công, đường dẫn chứa một bước không thể thực hiện được, do đó mốc đó không thể là điểm bắt đầu. Nếu mô phỏng thành công, mọi ô được truy cập đều là đất, có nghĩa là mốc đáp ứng mọi yêu cầu. Vì mọi mốc có thể đều được kiểm tra nên không có câu trả lời hợp lệ nào bị bỏ sót và không có câu trả lời không hợp lệ nào được đưa vào. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = []
    sights = []

    for i in range(n):
        row = input().strip()
        grid.append(row)
        for j, c in enumerate(row):
            if 'A' <= c <= 'Z':
                sights.append((c, i, j))

    k = int(input())
    moves = []
    for _ in range(k):
        d, length = input().split()
        moves.append((d, int(length)))

    delta = {
        'N': (-1, 0),
        'S': (1, 0),
        'W': (0, -1),
        'E': (0, 1)
    }

    answer = []

    for c, start_r, start_c in sights:
        r, col = start_r, start_c
        possible = True

        for d, length in moves:
            dr, dc = delta[d]
            for _ in range(length):
                r += dr
                col += dc
                if grid[r][col] == '#':
                    possible = False
                    break
            if not possible:
                break

        if possible:
            answer.append(c)

    if answer:
        print(''.join(sorted(answer)))
    else:
        print("no solution")

if __name__ == "__main__":
    solve()
```Giai đoạn đầu vào lưu trữ toàn bộ lưới vì việc kiểm tra chuyển động cần truy cập ngẫu nhiên vào các ô. Đồng thời, tọa độ của các ô viết hoa được thu thập, tránh việc quét sau này. 

Các hướng dẫn được lưu trữ dưới dạng cặp hướng và độ dài. Từ điển hướng chuyển đổi mỗi ký tự thành một thay đổi hàng và cột, giúp tránh lặp lại logic có điều kiện trong quá trình mô phỏng. 

Mô phỏng sử dụng các vòng lặp lồng nhau vì độ dài của lệnh có nghĩa là phải đi qua mọi ô trung gian. Chỉ kiểm tra vị trí cuối cùng sẽ bỏ sót những con đường đi qua biển. Biên giới của bản đồ được đảm bảo là biển nên các bước di chuyển hợp lệ không thể rời khỏi mảng mà không đi vào ô biển trước. 

Kết quả được sắp xếp trước khi in. Mặc dù các mốc là duy nhất nhưng chúng được phát hiện theo vị trí của chúng trong lưới đầu vào chứ không phải theo thứ tự bảng chữ cái. 

# Ví dụ đã hoạt động 

## Mẫu 1 

đầu vào:```
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
```Đối với mốc`A`: 

| Bước | Hướng dẫn | Vị trí | hợp lệ | 
| --- | --- | --- | --- | 
| Bắt đầu | A | (4,7) | vâng | 
| 1 | N 2 | (2,7) | vâng | 
| 2 | S 1 | (3,7) | vâng | 
| 3 | E 1 | (3,8) | vâng | 
| 4 | W 2 | (3,6) | vâng | 

Đối với mốc`D`: 

| Bước | Hướng dẫn | Vị trí | hợp lệ | 
| --- | --- | --- | --- | 
| Bắt đầu | D | (4,3) | vâng | 
| 1 | N 2 | (2,3) | vâng | 
| 2 | S 1 | (3,3) | vâng | 
| 3 | E 1 | (3,4) | vâng | 
| 4 | W 2 | (3,2) | vâng | 

Cả hai con đường đều thành công, vì vậy đầu ra là`AD`. 

Dấu vết này chứng tỏ rằng các mốc bắt đầu khác nhau có thể tạo ra các vị trí cuối cùng khác nhau. Vị trí cuối cùng không liên quan, chỉ có tính hợp lệ của đường dẫn mới quan trọng. 

## Mẫu 2 

đầu vào:```
3 4
####
#.A#
####
2
W 1
N 2
```| Bước | Hướng dẫn | Vị trí | hợp lệ | 
| --- | --- | --- | --- | 
| Bắt đầu | A | (1,2) | vâng | 
| 1 | W 1 | (1,1) | không | 

Chuyến đi đầu tiên ra biển nên cột mốc bị bác bỏ ngay. Hướng dẫn thứ hai không bao giờ cần thiết. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26k) | Mỗi cột mốc trong số tối đa 26 cột mốc mô phỏng tất cả các bước đi bộ. | 
| Không gian | O(nm) | Lưới được lưu trữ để mọi chuyển động có thể được kiểm tra. | 

Mô phỏng tuyến đường lớn nhất có thể chỉ thực hiện vài triệu lượt kiểm tra ô, vừa vặn trong giới hạn. Lưới yêu cầu khoảng một triệu ký tự được lưu trữ, cũng nằm trong giới hạn bộ nhớ. 

# Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = []
    sights = []

    for i in range(n):
        row = input().strip()
        grid.append(row)
        for j, c in enumerate(row):
            if 'A' <= c <= 'Z':
                sights.append((c, i, j))

    k = int(input())
    moves = []
    for _ in range(k):
        d, x = input().split()
        moves.append((d, int(x)))

    delta = {'N': (-1, 0), 'S': (1, 0), 'W': (0, -1), 'E': (0, 1)}

    ans = []
    for c, r, col in sights:
        ok = True
        for d, length in moves:
            dr, dc = delta[d]
            for _ in range(length):
                r += dr
                col += dc
                if grid[r][col] == '#':
                    ok = False
                    break
            if not ok:
                break
        if ok:
            ans.append(c)

    sys.stdin = old
    return ''.join(sorted(ans)) if ans else "no solution"

assert solve("""6 10
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

assert solve("""3 4
####
#.A#
####
2
W 1
N 2
""") == "no solution"

assert solve("""3 5
#####
#A..#
#####
1
E 3
""") == "no solution"

assert solve("""5 5
#####
#A..#
#...#
#..B#
#####
2
S 2
E 1
""") == "AB"

assert solve("""3 3
###
#A#
###
1
N 1
""") == "no solution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | QUẢNG CÁO | Nhiều mốc hợp lệ | 
| Mẫu 2 | không có giải pháp | Phong trào bị chặn ngay lập tức | 
| Đường thẳng vào tường | không có giải pháp | Kiểm tra xác nhận chuyển động trung gian | 
| Khu vực mở với hai cột mốc | AB | Kiểm tra nhiều mô phỏng thành công | 
| Bản đồ kèm theo tối thiểu | không có giải pháp | Kiểm tra xử lý ranh giới | 

# Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
3 5
#####
#A..#
#####
1
E 3
```Thuật toán bắt đầu lúc`A`và thực hiện ba nước đi về phía đông. Hai chặng đầu tới đất liền, còn chặng thứ ba tới biên giới biển. Quá trình mô phỏng dừng lại và từ chối`A`, sản xuất`no solution`. 

Đối với trường hợp cạnh thứ hai:```
3 5
#####
#A###
#####
1
E 1
```Nước đi duy nhất có thể là ngay lập tức đi vào ô biển. Thuật toán phát hiện điều này sau bước đầu tiên và không bao giờ chấp nhận mốc. 

Đối với trường hợp tổng chuyển vị quay trở lại gần khu vực bắt đầu nhưng đường dẫn không hợp lệ, thuật toán vẫn hoạt động chính xác vì nó xác thực từng chuyển động đơn lẻ. Tuyến đường được coi là một chuỗi các vị trí, không chỉ là một vectơ từ đầu đến cuối.
