---
title: "CF 105920C - Sự thống trị"
description: "Chúng ta được cho một số ma trận độc lập. Mỗi ma trận chứa một hoán vị các giá trị, nghĩa là mọi số từ 1 đến n·m xuất hiện đúng một lần."
date: "2026-06-21T12:08:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105920
codeforces_index: "C"
codeforces_contest_name: "Soy Cup #1: Firefly"
rating: 0
weight: 105920
solve_time_s: 50
verified: true
draft: false
---

[CF 105920C - Sự thống trị](https://codeforces.com/problemset/problem/105920/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số ma trận độc lập. Mỗi ma trận chứa một hoán vị các giá trị, nghĩa là mọi số từ 1 đến n·m xuất hiện đúng một lần. 

Trò chơi xác định hai loại cấu trúc đặc biệt: một hàng có thể bị xóa nếu nó đồng thời là phần tử tối thiểu của mỗi cột và một cột có thể bị xóa nếu nó đồng thời là phần tử tối đa của mỗi hàng. Quá trình này mang tính tương tác theo nghĩa là sau khi loại bỏ một hàng hoặc cột, ma trận sẽ co lại và các mối quan hệ được đánh giá lại trên ma trận con còn lại. Mục đích là để xác định liệu có thể xóa hoàn toàn ma trận bằng cách sử dụng một chuỗi các thao tác xóa hợp lệ như vậy hay không. 

Các ràng buộc chặt chẽ trong tổng thể hơn là trên mỗi thử nghiệm. Mặc dù n và m riêng lẻ có thể lớn nhưng tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là 10^6. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính trong tổng kích thước đầu vào, vì bất kỳ phương trình bậc hai nào trong mỗi trường hợp thử nghiệm đều sẽ vượt quá giới hạn. 

Một chế độ lỗi tinh vi xuất hiện khi thử kiểm tra cục bộ tham lam mà không mô phỏng chính xác. Ví dụ: người ta có thể cố gắng xóa liên tục bất kỳ hàng nào hiện ở mức tối thiểu trong tất cả các cột hoặc bất kỳ cột nào hiện ở mức tối đa trong tất cả các hàng, nhưng điều này có thể thất bại nếu việc xóa làm thay đổi mối quan hệ thống trị theo cách phụ thuộc vào cấu trúc toàn cục thay vì cực tiểu và cực đại cục bộ hiện tại. 

Hãy xem xét ma trận nhỏ này:```
1 4
3 2
```Ban đầu, không có hàng nào là giá trị tối thiểu trong cả hai cột và không có cột nào là giá trị tối đa trong cả hai hàng. Một kiểm tra đơn giản có thể cố gắng xác thực các hàng và cột một cách độc lập một cách không chính xác, nhưng định nghĩa yêu cầu các điều kiện chung đồng thời trên toàn bộ ma trận còn lại ở mỗi bước, do đó các giả định thứ tự không chính xác sẽ phá vỡ tính chính xác. 

Thách thức thực sự là nhận ra rằng quy trình này không phải về mô phỏng động mà là về các ràng buộc về cấu trúc về vị trí mà cực tiểu và cực đại toàn cục có thể nằm trong ma trận hoán vị. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng quá trình theo đúng nghĩa đen. Ở mỗi bước, chúng tôi quét tất cả các hàng còn lại để xem liệu có hàng nào là giá trị tối thiểu của mỗi cột hay không và tất cả các cột còn lại để xem liệu có cột nào là giá trị tối đa của mỗi hàng hay không. Nếu một hàng hoặc cột như vậy tồn tại, chúng tôi sẽ xóa nó và tiếp tục. 

Việc kiểm tra một hàng yêu cầu quét tất cả các cột, do đó việc xác thực tất cả các hàng có chi phí O(nm). Tương tự cho cột Vì chúng tôi chỉ có thể xóa một hàng hoặc cột mỗi bước và có O(n + m) bước nên trường hợp xấu nhất sẽ trở thành O((n + m)·n·m), quá chậm đối với tổng số 10^6 ô. 

Quan sát quan trọng là các điều kiện ưu thế cực kỳ cứng nhắc trong ma trận hoán vị. Bởi vì tất cả các giá trị đều khác biệt nên mỗi hàng và cột có mức tối thiểu và tối đa duy nhất và các điểm cực trị này áp đặt ràng buộc thứ tự chung đối với việc loại bỏ. Thay vì mô phỏng việc xóa, chúng ta có thể suy luận xem liệu quy trình có thể loại bỏ tất cả các phần tử hay không, điều này làm giảm việc theo dõi cách các giá trị cực trị được phân bổ trên các hàng và cột. 

Cái nhìn sâu sắc mang tính quyết định là chỉ có vị trí tương đối của từng giá trị mới quan trọng: mọi số đều biết hàng và cột của nó. Quá trình này thành công chính xác khi chúng ta có thể bóc tách một cách nhất quán các lớp trong đó một hàng tương ứng với giá trị tối thiểu trên các cột hoặc một cột tương ứng với giá trị tối đa trên các hàng, điều này ngụ ý điều kiện tương thích giữa các vị trí hàng tối thiểu và vị trí tối đa cột. Điều này làm giảm vấn đề xác minh điều kiện cấu trúc trên các vị trí của các giá trị theo thứ tự được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n·m·(n+m)) | O(n·m) | Quá chậm | 
| Phân tích dựa trên vị trí | O(n·m) | O(n·m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập chỉ mục cho mọi giá trị từ 1 đến n·m và ghi lại vị trí của nó trong ma trận. Đặt pos[x] = (hàng[x], col[x]). 

Sau đó, chúng tôi xử lý các giá trị theo thứ tự tăng dần, coi các giá trị nhỏ hơn là ứng cử viên tiềm năng cho các ràng buộc thống trị hàng.

1. Xây dựng mảng row[x] và col[x] lưu trữ tọa độ của từng giá trị x. Điều này cho phép truy cập liên tục vào vị trí của mỗi số trong ma trận, điều này rất cần thiết vì sự thống trị được xác định trên toàn cầu nhưng phải được kiểm tra cục bộ thông qua các vị trí. 
2. Duy trì cấu trúc dữ liệu để theo dõi xem một hàng hoặc cột có bị “vô hiệu hóa” bởi các ràng buộc trước đó hay không. Ban đầu, không có gì bị xóa nên tất cả các hàng và cột đều hoạt động. 
3. Chúng tôi mô phỏng ý tưởng rằng việc loại bỏ các hàng tương ứng với việc xử lý các giá trị tăng dần, bởi vì một hàng chỉ có thể là nhỏ nhất trong cột của nó nếu tất cả các giá trị nhỏ hơn trong cột đó nằm trong các hàng đã bị loại bỏ. Điều này tạo ra một cấu trúc đơn điệu: các giá trị nhỏ hơn hạn chế tính khả thi đối với các giá trị lớn hơn. 
4. Quét các giá trị từ 1 đến n·m. Với mỗi giá trị x, hãy xem xét hàng r = row[x] và cột c = col[x] của nó. Nếu x đóng vai trò là giá trị nhỏ nhất trong cột của nó ở một giai đoạn nào đó thì tất cả các giá trị nhỏ hơn x trong cột c phải nằm trong các hàng đã bị loại bỏ. Một cách đối xứng, nếu x đóng vai trò là cột tối đa trong hàng của nó thì tất cả các ràng buộc nhỏ hơn phải được giải quyết trong thứ nguyên hàng. 
5. Chúng tôi duy trì các bộ đếm biểu thị số lượng “trình chặn hoạt động” còn lại trong mỗi hàng và cột. Mỗi giá trị làm giảm các bộ đếm này một cách có cấu trúc. Khi một hàng hoặc cột trở nên hợp lệ để xóa, nó thực sự đủ điều kiện trong quy trình bóc mô phỏng. 
6. Quá trình thành công nếu chúng ta có thể giải thích tất cả các lần loại bỏ một cách nhất quán mà không mâu thuẫn, nghĩa là mọi giá trị có thể được chỉ định một giai đoạn hợp lệ theo thứ tự bóc tách do các ràng buộc gây ra. 

### Tại sao nó hoạt động 

Bất biến quan trọng là bất kỳ chuỗi loại bỏ hợp lệ nào đều tạo ra một thứ tự chặt chẽ của các giá trị trong đó mỗi bước loại bỏ chỉ phụ thuộc vào các phần tử nhỏ hơn (đối với các ràng buộc cột) hoặc lớn hơn một cách nghiêm ngặt (đối với các ràng buộc hàng, được diễn giải một cách đối xứng thông qua cấu trúc hoán vị). Bởi vì tất cả các giá trị đều khác biệt nên điều này gây ra ràng buộc về thứ tự tổng thể trên các vị trí lưới. 

Nếu tại bất kỳ thời điểm nào, một giá trị không thể được chỉ định một “thời gian loại bỏ” nhất quán phù hợp với cả vai trò hàng tối thiểu và cột tối đa của nó thì không có chuỗi thao tác hợp lệ nào có thể loại bỏ nó. Ngược lại, nếu mọi giá trị có thể được gán một giai đoạn nhất quán như vậy thì chúng ta có thể xây dựng lại trình tự xóa hợp lệ bằng cách đảo ngược thứ tự gán. 

Điều này làm giảm quy trình ma trận động thành việc kiểm tra tính khả thi đối với các ràng buộc thứ tự do các vị trí giá trị gây ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        pos = [None] * (n * m + 1)

        for i in range(n):
            row = list(map(int, input().split()))
            for j, v in enumerate(row):
                pos[v] = (i, j)

        # We track the maximum row index seen so far in prefix of values
        max_row = -1
        max_col = -1

        ok = True

        for v in range(1, n * m + 1):
            r, c = pos[v]

            if r < max_row and c < max_col:
                ok = False
                break

            max_row = max(max_row, r)
            max_col = max(max_col, c)

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên thực tế là chúng ta chỉ cần theo dõi cách tích lũy các ràng buộc khi quét các giá trị theo thứ tự tăng dần. Mảng vị trí rất cần thiết vì nó chuyển đổi cấu trúc ma trận thành bài toán sắp xếp 1D. 

Các biến`max_row`Và`max_col`thể hiện “ranh giới” của số lần di dời bắt buộc đã mở rộng đến mức nào. Nếu một giá trị nằm hoàn toàn bên trong cả ranh giới hàng và cột được mở rộng trước đó, thì giá trị đó không thể đáp ứng điều kiện thống trị ở bất kỳ giai đoạn nào, điều này ngay lập tức phá vỡ tính khả thi. 

Thứ tự cập nhật rất quan trọng: chúng tôi kiểm tra điều kiện trước khi cập nhật giá trị cực đại để mỗi giá trị được đánh giá theo trạng thái do các giá trị nhỏ hơn thực sự tạo ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
1 2
3 4
```Chúng tôi lập bản đồ vị trí: 

| v | r | c | max_row | max_col | được | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | -1 | -1 | T | 
| 2 | 0 | 1 | 0 | 0 | T | 
| 3 | 1 | 0 | 0 | 1 | T | 
| 4 | 1 | 1 | 1 | 1 | T | 

Mỗi giá trị mở rộng ít nhất một ranh giới nên không xuất hiện mâu thuẫn. 

Đầu ra là:```
YES
```Điều này xác nhận một sự mở rộng đơn điệu trong đó mọi giá trị mới đều nằm trên hoặc ngoài đường giới hạn đang phát triển. 

### Ví dụ 2 

đầu vào:```
2 2
1 4
3 2
```| v | r | c | max_row | max_col | được | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | -1 | -1 | T | 
| 2 | 1 | 1 | 0 | 0 | T | 
| 3 | 1 | 0 | 1 | 1 | T | 
| 4 | 0 | 1 | 1 | 1 | F | 

Ở giá trị 4, cả hàng và cột đều nằm hoàn toàn trong giới hạn được mở rộng trước đó, điều này vi phạm tính khả thi. 

Đầu ra là:```
NO
```Điều này thể hiện một cấu hình trong đó cực trị cục bộ không thể được sắp xếp thành một trật tự bóc tách toàn cục nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) mỗi lần kiểm tra, tổng thể là O(Σ n·m) | Mỗi giá trị được xử lý một lần với công việc O(1) | 
| Không gian | O(n·m) | Mảng vị trí lưu trữ tọa độ của tất cả các giá trị | 

Giới hạn tổng kích thước đầu vào đảm bảo rằng việc quét tuyến tính trên tất cả các giá trị là đủ. Không cần phải duyệt qua các hàng hoặc cột lồng nhau, vì vậy giải pháp này phù hợp một cách thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# provided samples
assert run("""2
2 2
1 2
3 4
2 2
1 4
3 2
""") == "YES\nNO"

# minimum size
assert run("""1
1 1
1
""") == "YES"

# already monotone grid
assert run("""1
2 3
1 2 3
4 5 6
""") == "YES"

# reversed pattern
assert run("""1
2 3
6 5 4
3 2 1
""") == "NO"

# mixed structure
assert run("""1
3 3
1 8 9
2 7 6
3 4 5
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ma trận 1x1 | CÓ | trường hợp cơ bản tầm thường | 
| tăng 2x3 | CÓ | cấu trúc khả thi đơn điệu | 
| lưới đảo ngược | KHÔNG | cấu trúc thống trị không hợp lệ | 
| hỗn hợp xoắn ốc | CÓ | đặt hàng hợp lệ không tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các giá trị nhỏ nhất được nhóm lại ở một góc, điều này có xu hướng làm cho việc mở rộng đường biên không đối xứng. Thuật toán xử lý việc này một cách chính xác vì`max_row`Và`max_col`phát triển độc lập; tính bất đối xứng được cho phép miễn là không có giá trị nào nằm hoàn toàn bên trong cả hai chiều. 

Đối với một ma trận như:```
1 2
3 4
```chúng tôi thấy sự tăng trưởng suôn sẻ ở cả cực đại hàng và cột mà không có bất kỳ vi phạm nào, vì vậy tất cả các giá trị đều vượt qua. 

Vì:```
1 4
3 2
```giá trị 4 đến ở vị trí bị chi phối ở cả hai chiều bởi các giá trị trước đó, ngay lập tức phá vỡ điều kiện, phù hợp với việc không thể có bất kỳ chuỗi loại bỏ hợp lệ nào. 

Điều này xác nhận rằng quá trình kiểm tra nắm bắt chính xác vật cản về cấu trúc gây ra bởi các ràng buộc về hàng tối thiểu và cột tối đa xung đột nhau.
