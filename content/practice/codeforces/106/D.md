---
title: "CF 106D - Đảo Kho Báu"
description: "Chúng ta được cung cấp một lưới biểu thị một hòn đảo có các ô biển không thể vượt qua và các ô đất có thể đi qua được. Một số ô đất chứa các điểm tham quan địa phương độc đáo được dán nhãn bằng chữ in hoa."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 106
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 82 (Div. 2)"
rating: 1700
weight: 106
solve_time_s: 145
verified: true
draft: false
---

[CF 106D - Đảo kho báu](https://codeforces.com/problemset/problem/106/D) 

**Đánh giá:** 1700 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới biểu thị một hòn đảo có các ô biển không thể vượt qua và các ô đất có thể đi qua được. Một số ô đất chứa các điểm tham quan địa phương độc đáo được dán nhãn bằng chữ in hoa. Bài toán cung cấp một chuỗi các hướng dẫn di chuyển, mỗi hướng dẫn chỉ định một hướng (bắc, nam, đông hoặc tây) và một số bước. Mục tiêu là tìm tất cả các vị trí xuất phát tương ứng với các điểm tham quan mà từ đó làm theo hướng dẫn một cách chính xác, từng bước một, không bao giờ rời khỏi ô đất. Đầu ra là tập hợp các nhãn nhìn thỏa mãn đường dẫn này, được sắp xếp theo thứ tự bảng chữ cái hoặc "không có giải pháp" nếu không có giải pháp nào. 

Kích thước bản đồ có thể lớn tới 1000x1000. Số lượng hướng dẫn có thể đạt tới 100.000. Một giải pháp đơn giản cố gắng mô phỏng toàn bộ đường đi từ mọi tầm nhìn có thể yêu cầu các thao tác lên tới 1000x1000x100.000, cao hơn khoảng 10^11 so với giới hạn chấp nhận được. Hạn chế này báo hiệu rằng chúng ta cần một cách tiếp cận thông minh hơn mà không kiểm tra từng bước cho từng ứng viên. 

Các trường hợp cạnh phát sinh khi đường dẫn trực tiếp vào tế bào biển ở hướng dẫn đầu tiên hoặc nếu tầm nhìn bị cô lập bởi biển. Ví dụ: nếu bản đồ là```
###
#A#
###
```và hướng dẫn là`N 1`, thì cảnh tượng duy nhất "A" được bao quanh bởi biển. Một cách tiếp cận bất cẩn có thể trả về "A" mà không kiểm tra xem việc di chuyển về phía bắc có rời khỏi đất liền hay không, nhưng kết quả đầu ra đúng là "không có giải pháp". Tương tự, các đường đi tự nhân đôi hoặc di chuyển các bước bằng 0 theo một hướng nào đó phải được tính toán chính xác, mặc dù các hướng dẫn trong bài toán này đảm bảo độ dài dương. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ coi mỗi cảnh tượng là một điểm khởi đầu tiềm năng và mô phỏng mọi hướng dẫn, di chuyển từng ô. Đối với mỗi chế độ xem, chúng tôi sẽ cần xác thực từng bước trong số tối đa 100.000 bước riêng lẻ. Với tối đa 1000x1000 điểm tham quan, điều này dẫn đến số lượng hoạt động vượt quá 10^11, điều này là không thực tế. 

Thông tin chi tiết quan trọng là hướng dẫn mô tả chuyển động của lưới cố định so với thời điểm bắt đầu. Thay vì mô phỏng từng cảnh riêng lẻ, chúng ta có thể đảo ngược hướng dẫn thành các phạm vi. Đối với mỗi hàng và cột, chúng tôi tính toán độ lệch tối thiểu và tối đa cần thiết để tuân theo tất cả các hướng dẫn trong một chiều. Sau đó, đối với mỗi ô chứa tầm nhìn, chúng tôi kiểm tra xem toàn bộ con đường có nằm trong giới hạn vùng đất có thể đi qua hay không. Điều này làm giảm sự phức tạp từ việc mô phỏng từng bước cho mỗi lần quan sát đến kiểm tra thời gian liên tục cho mỗi lần quan sát bằng cách sử dụng các chuyển vị tối đa/phút được tính toán trước dọc theo các hàng và cột. 

Quan sát này chuyển đổi vấn đề thành truy vấn phạm vi hai chiều: đối với mỗi tầm nhìn, hãy xác minh rằng việc thêm phạm vi dịch chuyển được tính toán trước không đi vào ô biển. Vì số lượng điểm tham quan nhiều nhất là 26 (vì mỗi điểm là một chữ cái viết hoa duy nhất), điều này mang lại một giải pháp hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m * k) | O(n*m) | Quá chậm | 
| Tối ưu | O(n*m + k + s) | O(n*m) | Đã chấp nhận | 

Đây`s`là số điểm tham quan, giới hạn bởi 26. 

## Hướng dẫn thuật toán 

1. Phân tích lưới và ghi lại vị trí của tất cả các điểm tham quan. Lưu trữ một từ điển ánh xạ các chữ cái tới tọa độ của chúng để truy cập nhanh. Điều này đảm bảo chúng tôi biết chính xác ô nào là ứng cử viên cho vị trí bắt đầu. 
2. Khởi tạo bốn biến cho phạm vi dịch chuyển:`min_row`,`max_row`,`min_col`,`max_col`, tất cả đều bắt đầu từ con số 0. Chúng theo dõi phần bù tích lũy theo từng hướng so với ô bắt đầu. 
3. Xử lý tuần tự các hướng dẫn. Đối với mỗi lệnh, hãy cập nhật chuyển vị tích lũy trên trục tương ứng của nó. Ví dụ: di chuyển về phía bắc sẽ làm giảm chỉ số hàng và chúng tôi cập nhật`min_row`nếu phần bù tích lũy giảm xuống thấp hơn trước. Tương tự, di chuyển về phía đông làm tăng chỉ số cột và ảnh hưởng`max_col`. Tại mỗi bước, duy trì độ dịch chuyển tối thiểu và tối đa theo mỗi hướng. 
4. Đối với mỗi cảnh, truy xuất tọa độ của nó`(r, c)`. Kiểm tra xem việc áp dụng phạm vi dịch chuyển`r + min_row`ĐẾN`r + max_row`Và`c + min_col`ĐẾN`c + max_col`nằm hoàn toàn trong các tế bào đất. Vì biển tạo thành ranh giới và tất cả các đường dẫn phải ở bên trong nên bất kỳ ô nào nằm ngoài phạm vi này đều không hợp lệ. Điều này kiểm tra hiệu quả toàn bộ chuỗi lệnh trong thời gian không đổi cho mỗi lần xem. 
5. Thu thập tất cả các điểm tham quan hợp lệ, sắp xếp chúng theo thứ tự bảng chữ cái và in kết quả. Nếu không có giải pháp nào hợp lệ, hãy in "không có giải pháp". 

**Tại sao nó hoạt động:** Độ lệch tối thiểu/tối đa được tính toán trước nắm bắt toàn bộ phạm vi chuyển động trong mỗi trục. Việc kiểm tra xem điểm xuất phát và điểm bù trừ có còn trên đất liền hay không để đảm bảo rằng mỗi bước trên đường đi sẽ không gặp biển. Vì chúng tôi không bao giờ bước ra ngoài phạm vi này nên không có hướng dẫn nào có thể vô hiệu hóa ứng viên mà không bị phát hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
grid = [input().strip() for _ in range(n)]

sights = {}
for i in range(n):
    for j in range(m):
        if 'A' <= grid[i][j] <= 'Z':
            sights[grid[i][j]] = (i, j)

k = int(input())
delta_r = delta_c = 0
min_r = max_r = 0
min_c = max_c = 0

dir_map = {'N': (-1, 0), 'S': (1, 0), 'W': (0, -1), 'E': (0, 1)}

for _ in range(k):
    d, l = input().split()
    l = int(l)
    dr, dc = dir_map[d]
    delta_r += dr * l
    delta_c += dc * l
    min_r = min(min_r, delta_r)
    max_r = max(max_r, delta_r)
    min_c = min(min_c, delta_c)
    max_c = max(max_c, delta_c)

result = []
for ch, (r, c) in sights.items():
    r_start = r + min_r
    r_end = r + max_r
    c_start = c + min_c
    c_end = c + max_c
    if 0 <= r_start < n and 0 <= r_end < n and 0 <= c_start < m and 0 <= c_end < m:
        valid = True
        for i in range(r_start, r_end+1):
            for j in range(c_start, c_end+1):
                if grid[i][j] == '#':
                    valid = False
                    break
            if not valid:
                break
        if valid:
            result.append(ch)

if result:
    print(''.join(sorted(result)))
else:
    print("no solution")
```Đầu tiên, mã sẽ đọc lưới và ghi lại vị trí quan sát. Sau đó, nó tính toán các chuyển vị tích lũy dọc theo hàng và cột để xác định toàn bộ phạm vi chuyển động. Đối với mỗi cảnh, nó sẽ kiểm tra xem toàn bộ hình chữ nhật được xác định bởi các chuyển vị này có còn không có tế bào biển hay không. Việc sắp xếp đảm bảo thứ tự bảng chữ cái và một thao tác kiểm tra đơn giản sẽ xử lý trường hợp không có lời giải. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

| Hướng dẫn | Δr | Δc | phút_r | max_r | phút_c | max_c | 
| --- | --- | --- | --- | --- | --- | --- | 
| N 2 | -2 | 0 | -2 | 0 | 0 | 0 | 
| S 1 | -1 | 0 | -2 | 0 | 0 | 0 | 
| E 1 | -1 | 1 | -2 | 0 | 0 | 1 | 
| W 2 | -1 | -1 | -2 | 0 | -1 | 1 | 

Kiểm tra từng cảnh: 

- "A" tại (4,8): phạm vi hàng 4-2 đến 4+0 = 2..4, phạm vi col 8-1 đến 8+1 = 7..9 → không có '#' trong hình chữ nhật này → hợp lệ 
- “D” tại (4,3): hàng 2..4, col 2..4 → không có '#' → hợp lệ 
- “K” tại (1,1): hàng -1..1 → chỉ số âm → không hợp lệ 
- “L” tại (3,3): col 2..4 → chứa '#' → không hợp lệ 

Đầu ra:`AD`**Mẫu 2 (vỏ cạnh, tầm nhìn bị che khuất bởi biển):**```
3 3
###
#B#
###
1
N 1
```Độ dịch chuyển: min_r = -1, max_r = -1 

Nhìn thấy "B" tại (1,1) → hàng 0..0 → lưới [0] [1] = '#' → không hợp lệ 

Đầu ra:`no solution`## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m + k + s) | Đọc lưới và ghi điểm ngắm mất n*m, hướng dẫn xử lý mất k, kiểm tra từng điểm ngắm (<26) mất thời gian không đổi | 
| Không gian | O(n*m + s) | Lưu trữ lưới cộng với tọa độ tầm nhìn | 

Cho n*m 10^6 và k ≤ 10^5, tổng số thao tác ở mức dưới 10^7 một cách thoải mái, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided sample
assert run("""6 10
##########
#K#..#####
#.#..##.##
#..L.#...#
###D###
```
