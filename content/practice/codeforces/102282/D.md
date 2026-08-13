---
title: "CF 102282D - \u0420\u043e\u0431\u043e\u0442 \u0432 \u043b\u0430\u0431\u0438\u0440\u0438\u043d\u0442\u0435"
description: "Chúng tôi có một lưới hình chữ nhật có nhiều nhất (100 lần 100) ô. Một số ô là tường và không thể vào được, trong khi các ô còn lại có thể đi qua được. Ban đầu, một ô có thể đi qua chứa robot."
date: "2026-08-13T09:07:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102282
codeforces_index: "D"
codeforces_contest_name: "2011, \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u043a\u043e\u043d\u0442\u0435\u0441\u0442 \u0421\u0413\u0410\u0423 \u043d\u0430 \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b ACM ICPC"
rating: 0
weight: 102282
solve_time_s: 71
verified: true
draft: false
---

[CF 102282D - \u0420\u043e\u0431\u043e\u0442 \u0432 \u043b\u0430\u0431\u0438\u0440\u0438\u043d\u0442\u0435](https://codeforces.com/problemset/problem/102282/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật có nhiều nhất (100 \times 100) ô. Một số ô là tường và không thể vào được, trong khi các ô còn lại có thể đi qua được. Ban đầu, một ô có thể đi qua chứa robot. Một lệnh sẽ di chuyển rô-bốt chính xác một ô theo một trong bốn hướng và một chương trình chỉ hợp lệ nếu mọi di chuyển vẫn nằm trong lưới và chạm vào một ô có thể di chuyển ngang. 

Chúng ta cần đếm tất cả các chương trình không trống có độ dài tối đa là (l), trong đó (l \le 30), sao cho việc thực thi toàn bộ chương trình sẽ đưa robot trở về ô bắt đầu. Các chuỗi lệnh khác nhau là các chương trình khác nhau, ngay cả khi chúng truy cập vào cùng một ô. 

Câu trả lời là không lấy modulo gì cả. Số lượng chương trình lớn nhất có thể có độ dài tối đa là 30 là theo thứ tự (4^{30}), tức là khoảng (1,15 \cdot 10^{18}), do đó việc triển khai cần số học số nguyên có thể biểu thị các giá trị vượt quá 32 bit. Số nguyên Python xử lý việc này trực tiếp. 

Lưới chỉ có (10^4) ô và độ dài chương trình tối đa chỉ là 30. Điều này gợi ý rõ ràng rằng chúng ta nên giữ một số trạng thái cho mỗi ô và mọi độ dài có thể. Một phép tính tỷ lệ với (nml) hoặc (4nml) dễ dàng đủ nhỏ. Ngược lại, việc liệt kê mọi chuỗi lệnh yêu cầu thời gian theo cấp số nhân trong (l), điều này trở nên không thể thực hiện được khi (l=30). 

Có một số trường hợp việc triển khai có vẻ hợp lý lại có thể đưa ra câu trả lời sai. 

Hãy xem xét mê cung nhỏ nhất có thể:```
1 1 1
*
```Câu trả lời đúng là`0`. Trình tự trống sẽ đưa rô-bốt trở về ô ban đầu nhưng vấn đề loại trừ rõ ràng chương trình trống. Việc triển khai bắt đầu câu trả lời của nó với số cách bắt đầu sau khi di chuyển bằng 0 và bao gồm nó sẽ xuất ra không chính xác`1`. 

Một ví dụ thứ hai là:```
1 3 2
=*.
```Robot bắt đầu ở giữa. Nó có thể di chuyển sang phải rồi sang trái, vì vậy chương trình hợp lệ duy nhất có độ dài tối đa là 2 là`ПЛ`. chương trình`ЛП`không hợp lệ vì lệnh đầu tiên của nó ngay lập tức đi vào tường. Đầu ra đúng là`1`. Một mô phỏng chỉ kiểm tra vị trí cuối cùng mà không từ chối chương trình ngay khi một nước đi trung gian chạm vào tường, có thể tính các chương trình không hợp lệ. 

Trường hợp thứ ba thể hiện ranh giới của độ dài cho phép:```
2 2 1
*.
..
```Không có chương trình một lệnh nào quay trở lại ô bắt đầu. Mỗi nước đi sẽ làm thay đổi tính chẵn lẻ của khoảng cách Manhattan ngay từ đầu, do đó chỉ có thể quay trở lại sau một số nước đi chẵn. Đầu ra đúng là`0`. Một lỗi thường gặp là đếm trạng thái ban đầu hoặc vô tình xử lý một chuyển đổi bổ sung. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi chuỗi lệnh có độ dài từ 1 đến (l). Đối với mỗi chuỗi, hãy mô phỏng robot từ ô bắt đầu. Nếu mọi di chuyển vẫn ở trong lưới và tránh các bức tường, hãy kiểm tra xem ô cuối cùng có phải là ô bắt đầu hay không. 

Điều này đúng vì mọi chương trình có thể được tạo ra chính xác một lần và mô phỏng tuân theo chính xác các quy tắc của mê cung. Vấn đề là số lượng trình tự. Có (4^k) chương trình có độ dài (k), vậy số lượng chương trình có độ dài phù hợp là 

\frac{4^{31}-4}{3}, 
] 

đã ở mức khoảng (1,5 \cdot 10^{18}). Vì việc mô phỏng một chương trình thực hiện các thao tác (O(k)) nên tổng công việc thực sự là (\Theta(l4^l)), khoảng (4,5 \cdot 10^{19}) kiểm tra chuyển động cơ bản tại (l=30). Bản thân việc tối ưu hóa mô phỏng không thể làm cho phương pháp này trở nên khả thi. 

Lý do Brute Force chứa quá nhiều công việc lặp đi lặp lại là vì nhiều chương trình khác nhau đến cùng một ô sau cùng một số lệnh. Khi hai tiền tố đã đến cùng một ô sau khi di chuyển chính xác (k), khả năng trong tương lai của chúng là giống hệt nhau. Thông tin duy nhất cần thiết để tiếp tục chương trình là ô hiện tại và số lệnh đã được sử dụng. Tiền tố chính xác đã đưa robot đến đó không còn quan trọng nữa. 

Quan sát đó dẫn trực tiếp đến lập trình động. Cho phép`dp[r][c]`biểu thị số chương trình hợp lệ có độ dài hiện tại kết thúc tại ô ((r,c)). Ban đầu, trước bất kỳ lệnh nào, chỉ có một cách duy nhất để đến ô bắt đầu, vì vậy`dp[start] = 1`. Để xây dựng chương trình dài hơn một lệnh, mọi trạng thái hợp lệ có thể di chuyển đến từng trạng thái trong số tối đa bốn trạng thái lân cận có thể đi qua của nó. Chúng tôi thêm số lượng của nó vào trạng thái tiếp theo tương ứng. 

Sau khi tính toán trạng thái cho các lệnh chính xác (k), giá trị ở ô bắt đầu chính xác là số chương trình hợp lệ có độ dài (k) quay về nhà. Tổng giá trị này cho (k=1,\dots,l) sẽ cho câu trả lời được yêu cầu. Trạng thái ban đầu không được thêm vào câu trả lời nên chương trình trống sẽ tự động bị loại trừ. 

Cách tiếp cận bạo lực hoạt động vì nó thể hiện rõ ràng mọi chương trình, nhưng không thành công vì nhiều chương trình có thể chia sẻ cùng một trạng thái trung gian theo cấp số nhân. Quan sát rằng chỉ ô hiện tại và độ dài hiện tại ảnh hưởng đến tất cả các bước di chuyển trong tương lai cho phép chúng tôi hợp nhất tất cả các tiền tố đó thành một trạng thái DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(l4^l)) | (O(l)) cho một đường dẫn mô phỏng | Quá chậm | 
| Tối ưu | (O(lnm)) | (O(nm)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và xác định vị trí ô bắt đầu duy nhất. Đối xử`.`Và`*`như các ô có thể di chuyển được, trong khi`=`là một bức tường. Ô bắt đầu có thể di chuyển được vì robot được đặt ở đó. 
2. Tạo mảng hai chiều`dp`chứa số 0 và đặt ô bắt đầu thành`1`. Điều này thể hiện tiền tố trống duy nhất hiện đang để robot ở vị trí ban đầu. Chúng tôi không thêm trạng thái này vào câu trả lời vì chương trình trống bị cấm. 
3. Lặp lại quá trình chuyển đổi cho mọi độ dài từ`1`bởi vì`l`. Đối với mỗi ô có thể duyệt, hãy lấy số cách hiện tại của nó và phân phối số đó cho từng ô lân cận hợp lệ. Một hàng xóm hợp lệ chính xác khi tọa độ của nó nằm trong lưới và ký tự lưới tương ứng không`=`. 
4. Thay thế`dp`với mảng mới được xây dựng. Sau quá trình chuyển đổi này,`dp[r][c]`có nghĩa là số lượng chương trình hợp lệ có độ dài chính xác hiện tại kết thúc tại ((r,c)). Việc giữ các mảng riêng biệt trong các độ dài liên tiếp sẽ ngăn quá trình chuyển đổi vô tình sử dụng trạng thái được tạo trước đó trong cùng một lần lặp. 
5. Thêm`dp[start_row][start_col]`để trả lời. Đây chính xác là những chương trình hợp lệ có độ dài hiện tại là số lần lặp và vị trí cuối cùng của nó là vị trí ban đầu. 
6. Sau khi xử lý tất cả các độ dài lên đến`l`, in câu trả lời tích lũy. Mọi chương trình hợp lệ không trống có chính xác một độ dài từ 1 đến`l`, vì vậy nó được tính đúng một lần. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý chính xác (k) chuyển tiếp,`dp[r][c]`bằng số lượng chuỗi lệnh hợp lệ có độ dài chính xác (k) để di chuyển robot từ ô bắt đầu đến ((r,c)). Bất biến đúng với (k=0), vì có chính xác một chuỗi trống ở ô bắt đầu và không có chuỗi nào ở ô khác. Trong quá trình chuyển đổi, mọi chuỗi có độ dài-(k+1) hợp lệ đều có tiền tố độ dài-(k) duy nhất kết thúc tại một số ô lân cận, theo sau là một bước di chuyển hợp lệ. DP bổ sung chính xác những khả năng này và từ chối mọi hành động di chuyển vào tường hoặc bên ngoài lưới. Do đó, bất biến giữ cho mọi chiều dài. Đặc biệt,`dp[start]`sau (k) di chuyển đếm chính xác các chương trình hợp lệ có độ dài (k) quay về nhà và tính tổng nó trên tất cả các độ dài dương sẽ đưa ra câu trả lời chính xác được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, l = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    sr = sc = -1
    for r in range(n):
        for c in range(m):
            if grid[r][c] == '*':
                sr, sc = r, c

    dp = [[0] * m for _ in range(n)]
    dp[sr][sc] = 1

    answer = 0

    directions = ((-1, 0), (1, 0), (0, -1), (0, 1))

    for _ in range(l):
        ndp = [[0] * m for _ in range(n)]

        for r in range(n):
            for c in range(m):
                ways = dp[r][c]
                if ways == 0 or grid[r][c] == '=':
                    continue

                for dr, dc in directions:
                    nr = r + dr
                    nc = c + dc

                    if 0 <= nr < n and 0 <= nc < m:
                        if grid[nr][nc] != '=':
                            ndp[nr][nc] += ways

        dp = ndp
        answer += dp[sr][sc]

    print(answer)

if __name__ == "__main__":
    solve()
```Lưới đầu tiên được lưu trữ dưới dạng chuỗi, vì vậy việc kiểm tra xem một ô có phải là một bức tường hay không chỉ đơn giản là`grid[r][c] == '='`. Trong khi đọc lưới, duy nhất`*`ô được ghi lại là`(sr, sc)`. 

ban đầu`dp`mảng tương ứng với các lệnh 0. Chỉ có vị trí bắt đầu có một cách để đạt được. Cách biểu diễn này thuận tiện vì sau đó có thể áp dụng cùng một quá trình chuyển đổi cho mọi độ dài lệnh. 

Đối với mỗi lần lặp,`ndp`bắt đầu trống rỗng. Mọi trạng thái khác 0 trong`dp`đại diện cho một số tập hợp các tiền tố hợp lệ. Đối với mỗi hướng trong số bốn hướng, mã sẽ kiểm tra tọa độ mới trước khi lập chỉ mục cho lưới. Thứ tự này quan trọng vì Python sẽ gây ra lỗi lập chỉ mục cho các tọa độ bên ngoài mảng và các chỉ số âm sẽ đề cập đến các ô ở phía đối diện của danh sách. 

điều kiện`grid[nr][nc] != '='`chấp nhận cả hai`.`Và`*`. Robot có thể di chuyển qua ô bắt đầu giống như bất kỳ ô có thể di chuyển nào khác, điều này rất cần thiết để đếm đường quay trở lại. 

Mảng cũ chỉ được thay thế sau khi tất cả các chuyển đổi đã được tạo. Nếu chúng tôi cập nhật`dp`tại chỗ, trạng thái đạt được sớm trong vòng lặp có thể được sử dụng lại ngay lập tức, cho phép thực hiện nhiều lệnh trong một bước DP và tạo ra số đếm không chính xác. 

Câu trả lời chỉ được cập nhật sau khi chuyển đổi hoàn toàn. Do đó, giá trị gia tăng cho lần lặp đầu tiên biểu thị các chương trình có độ dài chính xác là 1 chứ không phải chương trình trống. Các số nguyên có độ chính xác tùy ý của Python cũng loại bỏ mọi lo ngại về tràn. Mặc dù số lượng chuỗi lệnh theo lý thuyết đạt khoảng (10^{18}), Python có thể biểu thị chính xác số lượng kết quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mê cung là```
=====
=.*.=
=.===
```Ô bắt đầu chỉ có một ô lân cận có thể duyệt được, ô ngay bên phải nó. Từ ô đó, bước di chuyển hữu ích duy nhất là quay lại điểm bắt đầu. 

Sự phát triển DP là: 

| Chiều dài |`dp[start]`| Câu trả lời tích lũy | Phong trào liên quan | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | Tiền tố trống khi bắt đầu | 
| 1 | 0 | 0 | Robot di chuyển sang phải | 
| 2 | 1 | 1 | Robot di chuyển sang trái trở lại điểm xuất phát | 
| 3 | 0 | 1 | Robot lại di chuyển sang phải | 

Câu trả lời cuối cùng là`1`. Chương trình hợp lệ duy nhất là`ПЛ`. 

Ví dụ này thực hiện cả việc xử lý tường và loại trừ chương trình trống. ban đầu`1`trong DP là cần thiết cho quá trình chuyển đổi nhưng nó không bao giờ đóng góp trực tiếp vào câu trả lời. 

### Mẫu 2 

Mê cung thứ hai là```
..=..
..=..
=.*.=
=...=
```Robot có ba khả năng trả về hai lệnh, cụ thể là ba chương trình dài hai lệnh được liệt kê trong câu lệnh. Ngoài ra còn có 19 chương trình trả về hợp lệ có độ dài bốn. Không thể quay lại với số lượng lệnh lẻ vì mỗi lần di chuyển sẽ thay đổi màu bàn cờ của ô hiện tại. 

Các giá trị DP chính là: 

| Chiều dài |`dp[start]`| Câu trả lời tích lũy | 
| --- | --- | --- | 
| 0 | 1 | 0 | 
| 1 | 0 | 0 | 
| 2 | 3 | 3 | 
| 3 | 0 | 3 | 
| 4 | 19 | 22 | 

Câu trả lời cuối cùng là`22`. 

Các giá trị 0 ở độ dài 1 và 3 thể hiện cấu trúc lưỡng cực của lưới. Một nước đi luôn làm thay đổi tính chẵn lẻ của`row + column`, do đó, việc quay lại ô ban đầu cần có số lần di chuyển chẵn. DP không cần quy tắc đặc biệt cho thuộc tính này, nó tự nhiên tạo ra số 0 cho các độ dài không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(l \cdot n \cdot m)) | Mỗi lần lặp lại sẽ quét từng ô và kiểm tra tối đa bốn ô lân cận | 
| Không gian | (O(nm)) | Chỉ mảng DP hiện tại và tiếp theo được lưu trữ | 

Với (n,m \le 100) và (l \le 30), có nhiều nhất (30 \cdot 100 \cdot 100 = 300{,}000) trạng thái ô cần xử lý, với bốn lần kiểm tra lân cận cho mỗi trạng thái. Khối lượng công việc thu được nằm thoải mái trong giới hạn 1 giây đã nêu trong Python và hai mảng số nguyên (100 \times 100) dễ dàng nằm trong phạm vi 128 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

input = sys.stdin.readline

def solve():
    n, m, l = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    sr = sc = -1
    for r in range(n):
        for c in range(m):
            if grid[r][c] == '*':
                sr, sc = r, c

    dp = [[0] * m for _ in range(n)]
    dp[sr][sc] = 1

    answer = 0
    directions = ((-1, 0), (1, 0), (0, -1), (0, 1))

    for _ in range(l):
        ndp = [[0] * m for _ in range(n)]

        for r in range(n):
            for c in range(m):
                ways = dp[r][c]
                if ways == 0 or grid[r][c] == '=':
                    continue

                for dr, dc in directions:
                    nr = r + dr
                    nc = c + dc

                    if 0 <= nr < n and 0 <= nc < m:
                        if grid[nr][nc] != '=':
                            ndp[nr][nc] += ways

        dp = ndp
        answer += dp[sr][sc]

    return str(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

sample1 = """\
3 5 3
=====
=.*.=
=.===
"""
assert run(sample1) == "1", "sample 1"

sample2 = """\
4 5 4
..=..
..=..
=.*.=
=...=
"""
assert run(sample2) == "22", "sample 2"

minimum_case = """\
1 1 1
*
"""
assert run(minimum_case) == "0", "empty program must not be counted"

boundary_case = """\
1 2 4
*.
"""
assert run(boundary_case) == "2", "only LR and LRLR are possible"

small_open_case = """\
2 2 4
*.
..
"""
assert run(small_open_case) == "10", "2x2 grid has 2 returns of length 2 and 8 of length 4"

maximum_case = "100 100 30\n" + (
    "=" * 100 + "\n"
) * 50 + (
    "=" * 49 + "*" + "=" * 50 + "\n"
) + (
    "=" * 100 + "\n"
) * 49
assert run(maximum_case) == "0", "isolated start in maximum-size grid"

off_by_one_case = """\
2 2 1
*.
..
"""
assert run(off_by_one_case) == "0", "one move cannot return to the start"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`chỉ với`*`|`0`| Kích thước tối thiểu và loại trừ chương trình trống | 
|`1 2 4`với`*.`|`2`| Chuyển động và tích lũy ranh giới trong nhiều khoảng thời gian | 
|`2 2 4`với tất cả các ô có thể duyệt được |`10`| Nhiều đường dẫn đạt đến cùng trạng thái và trả về nhiều lần | 
|`100 100 30`với sự cô lập`*`|`0`| Kích thước tối đa, chiều dài tối đa và khả năng xử lý tường | 
|`2 2 1`với một người hàng xóm cởi mở |`0`| Xử lý từng cái một trong thời lượng chương trình tối đa được phép | 

## Vỏ cạnh 

### Chương trình trống 

cho```
1 1 1
*
```trạng thái DP ban đầu là`dp[0][0] = 1`, nhưng câu trả lời bắt đầu từ con số 0. Lần lặp duy nhất thử tất cả bốn hướng và mọi hướng đều rời khỏi lưới, vì vậy DP tiếp theo đều bằng 0. Câu trả lời cuối cùng là`0`. Trạng thái ban đầu được sử dụng làm nguồn cho các chuyển đổi, bản thân nó không bao giờ là một câu trả lời hợp lệ. 

### Nước đi trung gian không hợp lệ 

cho```
1 3 2
=*.
```quá trình chuyển đổi đầu tiên chỉ có thể di chuyển từ điểm bắt đầu ở cột 2 sang cột 3. Hàng xóm bên trái là một bức tường và hai hướng còn lại nằm ngoài lưới. Sau bước đầu tiên, số điểm bắt đầu được tính bằng 0. Trong lần chuyển tiếp thứ hai, robot có thể di chuyển từ cột 3 trở lại điểm xuất phát, mang lại cho`dp[start] = 1`. Như vậy câu trả lời là`1`. 

Trình tự không hợp lệ`ЛП`không bao giờ vào DP vì quá trình chuyển đổi đầu tiên của nó nhắm vào bức tường. Do đó, DP sẽ từ chối các chương trình không hợp lệ tại thời điểm chúng trở nên không hợp lệ. 

### Trả về độ dài lẻ 

cho```
2 2 1
*.
..
```lần chuyển đổi đầu tiên sẽ gửi robot đến một trong hai người hàng xóm của nó, nhưng cả hai đều không phải là bước khởi đầu. Như vậy`dp[start] = 0`và câu trả lời là`0`. 

Hiện tượng tương tự cũng xảy ra với mọi độ dài lẻ trong biểu đồ lưới. Mỗi bước di chuyển sẽ chuyển đổi tính chẵn lẻ của`r + c`, do đó số lần di chuyển lẻ không thể quay về ô ban đầu. Thuật toán không cần phải phát hiện điều này một cách riêng biệt vì số lần chuyển đổi đã mã hóa nó. 

### Ô ranh giới 

cho```
1 2 4
*.
```robot có đúng một nước đi hợp lệ từ ô xuất phát sang bên phải. Nó phải quay lại bên trái trong lệnh thứ hai. Các chương trình quay lại duy nhất có thể có độ dài tối đa bốn là`ПЛ`Và`ПЛПЛ`, vậy câu trả lời là`2`. 

Kiểm tra ranh giới`0 <= nr < n`Và`0 <= nc < m`là những gì ngăn cản việc tính các bước di chuyển ngoài các cạnh trái, phải, trên và dưới. 

### Nhiều đường dẫn hợp nhất thành một ô 

cho```
2 2 4
*.
..
```có hai cách để quay về sau hai nước đi, một đi sang phải và quay lại và một đi xuống và quay lại. Sau bốn lần di chuyển sẽ có tám chương trình quay trở lại, tổng cộng là`10`. 

Đây chính xác là tình huống mà lập trình động giúp tiết kiệm công việc. Một số tiền tố khác nhau có thể đến cùng một ô và số đếm của chúng được hợp nhất thành một số nguyên trong`dp[r][c]`. Khi lệnh tiếp theo được chọn, tất cả các tiền tố đó đều có các tùy chọn tương lai giống hệt nhau, do đó, việc giữ chúng dưới dạng một số đếm sẽ không làm mất thông tin.
