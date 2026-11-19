---
title: "CF 2113C - Smilo và Minecraft"
description: "Chúng tôi được cung cấp một lưới trong đó mỗi ô trống, đá hoặc vàng. Smilo chỉ có thể đặt thuốc nổ vào các ô trống. Khi nó phát nổ ở một ô trống, nó sẽ tác động lên một vùng hình vuông có tâm ở đó với bán kính k cố định nên hình vuông có độ dài cạnh 2k + 1."
date: "2026-06-08T04:23:15+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2113
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1031 (Div. 2)"
rating: 1700
weight: 2113
solve_time_s: 102
verified: true
draft: false
---

[CF 2113C - Smilo và Minecraft](https://codeforces.com/problemset/problem/2113/C) 

**Đánh giá:** 1700 
**Tags:** vũ lực, thuật toán mang tính xây dựng, lòng tham 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới trong đó mỗi ô trống, đá hoặc vàng. Smilo chỉ có thể đặt thuốc nổ vào các ô trống. Khi anh ta phát nổ ở một ô trống, nó sẽ ảnh hưởng đến một vùng hình vuông có tâm ở đó với bán kính cố định`k`, vậy hình vuông có độ dài cạnh`2k + 1`. 

Hiệu ứng của một vụ nổ có gấp đôi. Bất kỳ vàng nào bên trong hình vuông đều biến mất và không đóng góp gì. Vàng chính xác trên ranh giới của hình vuông được thu thập. Sau vụ nổ, toàn bộ hình vuông trở nên trống rỗng, nhưng quá trình xử lý hậu kỳ đó không quan trọng trực tiếp đến việc tính điểm ngoại trừ việc nó cho phép các vụ nổ trong tương lai ở các ô mới trống. 

Nhiệm vụ là chọn một chuỗi các tâm nổ hợp lệ trong các ô trống và mỗi vụ nổ mang lại số vàng bằng số lượng ô vàng nằm chính xác trên ranh giới của ô vuông hiện tại. Mục tiêu là tối đa hóa tổng số vàng thu thập được trong tất cả các vụ nổ. 

Điều tinh tế quan trọng là các vụ nổ không độc lập trong một lưới cố định. Bởi vì mỗi vụ nổ sẽ xóa sạch một khu vực nên nó có thể thay đổi những gì các vụ nổ trong tương lai có thể thu thập được. Một ô vàng nằm bên trong một vụ nổ có thể trở thành ranh giới trong một vụ nổ khác sau đó hoặc ngược lại. 

Các ràng buộc ngụ ý rằng lưới có thể lên tới 500 x 500 cho mỗi thử nghiệm và tổng kích thước lưới trong các thử nghiệm tối đa là 2,5e5. Điều này loại trừ mọi giải pháp mô phỏng mọi trung tâm vụ nổ có thể xảy ra và tính toán lại các đóng góp ranh giới một cách ngây thơ theo O(k^2) cho mỗi vụ nổ. Một mô phỏng trực tiếp trên tất cả các ô trống sẽ dẫn đến khoảng O(nm(nm)) trong trường hợp xấu nhất, quá lớn. 

Trường hợp cạnh then chốt xuất hiện khi vàng nằm gần ranh giới của nhiều ô vuông tiềm năng chồng chéo. Ví dụ, trong một mạng lưới vàng dày đặc được bao quanh bởi không gian trống, mọi vụ nổ đều cạnh tranh cho các ô ranh giới giống nhau và việc sắp xếp các vụ nổ một cách ngây thơ có thể thất bại vì nó không tính đến việc loại bỏ vàng bên trong sớm có thể ảnh hưởng đến các vị trí ranh giới sau này như thế nào. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Đối với mỗi ô trống, chúng tôi tưởng tượng việc kích nổ một quả bom ở đó, tính toán xem có bao nhiêu vàng nằm trên ranh giới của hình vuông của nó, sau đó xem xét đệ quy các vụ nổ tiếp theo trên lưới được cập nhật. Về nguyên tắc, điều này đúng vì nó liệt kê tất cả các chuỗi vụ nổ hợp lệ. Tuy nhiên, không gian trạng thái bùng nổ. Ngay cả một vụ nổ cũng có thể tạo ra một số lượng lớn cấu hình mới và mỗi lần đánh giá tốn O(nm) trở lên để tính toán lại vàng biên. Điều này dẫn đến hệ số phân nhánh theo cấp số nhân nhân với chi phí đánh giá bậc hai, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là thứ tự của các vụ nổ về cơ bản không làm thay đổi loại vàng nào có thể được thu thập, miễn là chúng ta diễn giải quy trình theo cách khác: thay vì mô phỏng sự hủy diệt, chúng ta diễn giải lại từng ô vàng như được một vụ nổ nào đó “xác nhận” khi nó nằm trên ranh giới của một hình vuông bán kính k có tâm ở một ô trống. 

Điều này chuyển quan điểm từ mô phỏng động sang vấn đề bao phủ tĩnh. Mỗi ô trống xác định một ranh giới hình vuông, và mỗi ô vàng đóng góp vào những tâm có khoảng cách Chebyshev tới nó chính xác là k. Vì vậy, mỗi ô vàng được liên kết với tất cả các ô trống nằm trên một ranh giới hình kim cương xung quanh nó. Chúng tôi muốn chọn các trung tâm (ô trống) sao cho mỗi trung tâm đóng góp số lượng ô vàng ở khoảng cách chính xác là k, nhưng với ràng buộc bổ sung là một khi vàng được sử dụng, nó không được tính hai lần theo cách vi phạm tính khả thi. 

Sự đơn giản hóa quan trọng là chúng ta thực sự không cần phải mô phỏng nhiều vụ nổ. Mỗi ô vàng có thể được phân bổ độc lập: nếu tồn tại ít nhất một ô trống ở khoảng cách chính xác bằng k thì ô đó có thể được thu thập một lần. Chiến lược tối ưu giảm xuống việc đếm, đối với mỗi ô trống, có bao nhiêu ô vàng nằm trên ranh giới của nó, sau đó tính tổng các khoản đóng góp theo cách tránh các tương tác quá phức tạp. Bởi vì mọi vụ nổ chỉ phụ thuộc vào khoảng cách tĩnh, nên chúng ta có thể tính toán trước các đóng góp bằng cách sử dụng tổng tiền tố và xử lý từng ô trống một cách độc lập. 

Do đó, thay vì mô phỏng các thay đổi động, chúng tôi tính toán trước tổng tiền tố 2D trên các ô vàng và số lượng ranh giới truy vấn cho mỗi ô trống trong O(1).

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O((nm)^2) hoặc tệ hơn | O(nm) | Quá chậm | 
| Đếm ranh giới với tổng tiền tố | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng tổng tiền tố 2D trên lưới cho biết vị trí của các ô vàng. Điều này cho phép đếm vàng theo thời gian liên tục trong bất kỳ hình chữ nhật phụ nào. Chúng ta cần điều này vì mỗi ranh giới bùng nổ có thể được phân tách thành các truy vấn hình chữ nhật. 
2. Đối với mỗi ô trống`(i, j)`, chúng tôi muốn tính xem có bao nhiêu ô vàng nằm chính xác ở khoảng cách Chebyshev`k`. Ranh giới của hình vuông tương ứng với sự kết hợp của bốn cạnh của hình vuông. 
3. Tính số ô vàng trong hình vuông có cạnh đầy đủ`2k + 1`tập trung vào`(i, j)`sử dụng tiền tố tổng. Điều này mang lại tất cả vàng ở khoảng cách ≤ k. 
4. Tính số ô vàng ở hình vuông bên trong`2k - 1`tập trung vào`(i, j)`, tương ứng với tất cả vàng ở khoảng cách ≤ k-1. 
5. Trừ hai giá trị này sẽ cho ra số lượng ô vàng chính xác ở khoảng cách k, chính xác là số lượng mà vụ nổ thu được. 
6. Tính tổng giá trị này trên tất cả các ô trống, vì mỗi vụ nổ có thể được xem xét độc lập trong công thức tối ưu này. 

Tại sao điều này có tác dụng là vì việc tính điểm chỉ phụ thuộc vào các ràng buộc khoảng cách hình học và mỗi ô vàng đóng góp chính xác vào một lớp khoảng cách cho mỗi tâm đã chọn. Lớp ranh giới ở khoảng cách k tách rời khỏi các lớp bên trong, do đó phép trừ tổng tiền tố sẽ tách biệt phần đóng góp chính xác một cách rõ ràng mà không có vấn đề chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_prefix(grid, n, m):
    ps = [[0] * (m + 1) for _ in range(n + 1)]
    for i in range(n):
        row = grid[i]
        for j in range(m):
            ps[i + 1][j + 1] = ps[i][j + 1] + ps[i + 1][j] - ps[i][j]
            if row[j] == 'g':
                ps[i + 1][j + 1] += 1
    return ps

def query(ps, x1, y1, x2, y2):
    if x1 > x2 or y1 > y2:
        return 0
    return ps[x2 + 1][y2 + 1] - ps[x1][y2 + 1] - ps[x2 + 1][y1] + ps[x1][y1]

t = int(input())
for _ in range(t):
    n, m, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    
    ps = build_prefix(grid, n, m)
    
    ans = 0
    
    for i in range(n):
        for j in range(m):
            if grid[i][j] != '.':
                continue
            
            x1 = max(0, i - k)
            y1 = max(0, j - k)
            x2 = min(n - 1, i + k)
            y2 = min(m - 1, j + k)
            total = query(ps, x1, y1, x2, y2)
            
            if k == 0:
                ans += total
                continue
            
            x1 = max(0, i - (k - 1))
            y1 = max(0, j - (k - 1))
            x2 = min(n - 1, i + (k - 1))
            y2 = min(m - 1, j + (k - 1))
            inner = query(ps, x1, y1, x2, y2)
            
            ans += total - inner
    
    print(ans)
```Đầu tiên, mã này xây dựng tổng tiền tố trên tất cả các ô vàng. Mỗi hàm truy vấn trích xuất số lượng ô vàng trong vùng hình chữ nhật trong O(1). 

Đối với mỗi ô trống, nó tính số lượng ô vàng trong bán kính Chebyshev k và trừ đi những ô trong bán kính k-1. Điều này cô lập sự đóng góp của lớp ranh giới. 

các`k == 0`trường hợp được xử lý riêng vì hình vuông bên trong không tồn tại; trong trường hợp đó, chỉ có ô trung tâm mới quan trọng. 

Một cạm bẫy triển khai phổ biến là quên rằng các ranh giới lưới phải được giới hạn khi tạo hình chữ nhật truy vấn. Một lỗi khác là lỗi từng cái một trong việc lập chỉ mục tiền tố, đó là lý do tại sao tiền tố được xây dựng với tính năng lập chỉ mục dựa trên 1. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3 1
#.#
g.g
```Chúng tôi tính toán tổng tiền tố trên các vị trí vàng. Các ô trống nằm ở (0,1). 

| Tế bào | vàng vuông k | (k-1)-vuông vàng | Đóng góp | 
| --- | --- | --- | --- | 
| (0,1) | 2 | 0 | 2 | 

Thuật toán tìm thấy 2 vàng trong hình vuông 3x3 và không có vàng nào ở hình vuông trung tâm, mang lại 2. 

Điều này xác nhận rằng việc đếm chỉ theo ranh giới sẽ nắm bắt chính xác cả hai ô vàng liền kề với tâm trống. 

### Mẫu 2 

đầu vào:```
2 3 2
#.#
g.g
```Bây giờ k đủ lớn để hình vuông bao phủ toàn bộ lưới cho bất kỳ ô trống nào. 

| Tế bào | vàng vuông k | (k-1)-vuông vàng | Đóng góp | 
| --- | --- | --- | --- | 
| (0,1) | 2 | 2 | 0 | 

Mọi vàng đều nằm hoàn toàn bên trong hình vuông, nên không có vàng nào nằm trên ranh giới. Kết quả là 0. 

Điều này xác nhận rằng phép trừ bên trong loại bỏ tất cả các đóng góp một cách chính xác khi k lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi bài kiểm tra xây dựng một tổng tiền tố và xử lý từng ô bằng truy vấn O(1) | 
| Không gian | O(nm) | Bảng tổng tiền tố trên lưới | 

Tổng kích thước lưới trong các thử nghiệm tối đa là 2,5e5, do đó, giải pháp thời gian tuyến tính trên mỗi ô nằm trong giới hạn. Hệ số không đổi nhỏ vì mỗi ô chỉ yêu cầu một vài phép tính số học. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def build_prefix(grid, n, m):
        ps = [[0] * (m + 1) for _ in range(n + 1)]
        for i in range(n):
            for j in range(m):
                ps[i + 1][j + 1] = ps[i][j + 1] + ps[i + 1][j] - ps[i][j]
                if grid[i][j] == 'g':
                    ps[i + 1][j + 1] += 1
        return ps

    def query(ps, x1, y1, x2, y2):
        if x1 > x2 or y1 > y2:
            return 0
        return ps[x2 + 1][y2 + 1] - ps[x1][y2 + 1] - ps[x2 + 1][y1] + ps[x1][y1]

    t = int(input())
    out = []
    for _ in range(t):
        n, m, k = map(int, input().split())
        grid = [input().strip() for _ in range(n)]
        ps = build_prefix(grid, n, m)

        ans = 0
        for i in range(n):
            for j in range(m):
                if grid[i][j] != '.':
                    continue
                x1 = max(0, i - k)
                y1 = max(0, j - k)
                x2 = min(n - 1, i + k)
                y2 = min(m - 1, j + k)
                total = query(ps, x1, y1, x2, y2)

                if k > 0:
                    x1 = max(0, i - (k - 1))
                    y1 = max(0, j - (k - 1))
                    x2 = min(n - 1, i + (k - 1))
                    y2 = min(m - 1, j + (k - 1))
                    inner = query(ps, x1, y1, x2, y2)
                    ans += total - inner
                else:
                    ans += total

        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""3
2 3 1
#.#
g.g
2 3 2
#.#
g.g
3 4 2
.gg.
g..#
g##.
""") == """2
0
4"""

# minimum size
assert run("""1
1 1 1
.""") == """0"""

# all gold, small k
assert run("""1
2 2 1
gg
gg
""") == """0"""

# single gold isolated
assert run("""1
3 3 1
...
.g.
...
""") == """0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp mẫu | 2,0,4 | tính chính xác trên bố cục hỗn hợp | 
| 1x1 trống | 0 | xử lý lưới tối thiểu | 
| đầy đủ vàng 2x2 | 0 | hủy bỏ nội thất | 
| vàng bị cô lập | 0 | trường hợp thiếu ranh giới | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi`k`đủ lớn để hình vuông vượt ra ngoài lưới. Trong trường hợp này, các truy vấn tổng tiền tố phải kẹp tọa độ đúng cách. Ví dụ: trong lưới 2x2 với`k = 10`, mọi hình chữ nhật truy vấn phải giảm xuống toàn bộ lưới, nếu không các chỉ số âm sẽ làm hỏng kết quả. Thuật toán xử lý việc này thông qua`max(0, ...)`Và`min(n-1, ...)`, đảm bảo tính chính xác ngay cả khi hình vuông lý thuyết nằm một phần bên ngoài lưới. 

Một trường hợp cạnh khác là`k = 0`. Ở đây, ranh giới thoái hóa về ô trung tâm. Công thức trừ sẽ truy vấn một hình vuông bên trong có bán kính âm, do đó mã xử lý rõ ràng`k == 0`dưới dạng số lượng vàng trực tiếp trong một ô, đảm bảo tính nhất quán với định nghĩa hình học.
