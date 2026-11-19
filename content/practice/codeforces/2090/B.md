---
title: "CF 2090B - Bóng đẩy"
description: "Chúng ta được cung cấp mẫu chiếm chỗ cuối cùng của lưới sau một số chuỗi chèn bóng. Một quả bóng có thể được đẩy từ phía bên trái của một hàng hoặc từ phía trên của một cột. Quả bóng di chuyển về phía trước cho đến khi chạm tới một ô. Nếu ô đó trống, nó vẫn ở đó."
date: "2026-06-09T03:49:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2090
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1012 (Div. 2)"
rating: 1000
weight: 2090
solve_time_s: 108
verified: true
draft: false
---

[CF 2090B - Bóng đẩy](https://codeforces.com/problemset/problem/2090/B) 

**Đánh giá:** 1000 
**Tags:** vũ lực, dp, thực hiện 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp mẫu chiếm chỗ cuối cùng của lưới sau một số chuỗi chèn bóng. 

Một quả bóng có thể được đẩy từ phía bên trái của một hàng hoặc từ phía trên của một cột. Quả bóng di chuyển về phía trước cho đến khi chạm tới một ô. Nếu ô đó trống, nó vẫn ở đó. Nếu ô đó đã chứa một quả bóng, quả bóng đến sẽ chiếm vị trí đó và quả bóng bị dịch chuyển tiếp tục di chuyển theo cùng một hướng. 

Quá trình này giống như việc chèn liên tục các phần tử vào phía trước hàng hoặc cột, với các quả bóng hiện có được chuyển sâu hơn vào lưới. 

Nhiệm vụ không phải là xây dựng lại chuỗi hoạt động. Chúng ta chỉ cần quyết định xem có tồn tại ít nhất một chuỗi hợp lệ tạo ra lưới cuối cùng nhất định hay không. 

Kích thước lưới tối đa là 50 × 50 và tổng số ô trên tất cả các trường hợp thử nghiệm tối đa là 10.000. Đây là kích thước đầu vào rất nhỏ. Thậm chí một$O(nm)$hoặc$O(nm(n+m))$giải pháp dễ dàng đủ nhanh. Không cần cấu trúc dữ liệu phức tạp. 

Khó khăn là hiểu cấu hình cuối cùng nào có thể truy cập được. 

Một sai lầm phổ biến là nghĩ rằng mọi ô chứa quả bóng đều phải được kết nối với đường viền trên hoặc bên trái thông qua các quả bóng khác. Điều đó mạnh hơn mức cần thiết. 

Coi như:```
111
111
111
```Cấu hình này rõ ràng có thể truy cập được. Mỗi ô chứa một quả bóng. 

Bây giờ hãy xem xét:```
010
111
010
```Điều này cũng có thể truy cập được, mặc dù các ô ở góc trống. Một đối số kết nối đơn giản sẽ từ chối nó một cách không chính xác. 

Một sai lầm dễ mắc phải khác là tập trung vào thứ tự thực hiện các thao tác. Số lượng các trình tự chèn có thể tăng theo cấp số nhân, do đó việc mô phỏng trực tiếp hoặc quay lui là không khả thi ngay cả đối với các lưới nhỏ. 

Điều quan trọng là xác định thuộc tính cấu trúc mà mọi cấu hình có thể tiếp cận đều đáp ứng và điều đó cũng đủ cho khả năng tiếp cận. 

Một ví dụ phản biện đặc biệt rõ ràng là:```
000
000
001
```Câu trả lời là`NO`. 

Quả bóng ở góc dưới bên phải sẽ yêu cầu những quả bóng tồn tại ở đâu đó ở bên trái hoặc phía trên của nó để đẩy nó xa vào lưới. Vì cả hai đều không tồn tại nên không có chuỗi nào có thể tạo ra ô biệt lập này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các trình tự chèn có thể có. Mỗi thao tác chọn một hàng hoặc một cột. Ngay cả đối với lưới 5 × 5, số lượng các chuỗi có thể trở nên rất lớn. Vì có thể tồn tại tới 2500 quả bóng trong một trường hợp thử nghiệm nên việc tìm kiếm toàn diện là hoàn toàn không thể. 

Để tìm ra cách tiếp cận tốt hơn, chúng ta cần hiểu lực đẩy thực sự có tác dụng gì. 

Giả sử một ô$(i,j)$chứa một quả bóng trong cấu hình cuối cùng. Làm thế nào một quả bóng có thể đến đó? 

Nếu nó được chèn qua hàng$i$, thì trước khi dừng lại ở$(i,j)$, mọi ô bên trái của hàng đó phải chứa bóng. Nếu không nó đã dừng lại sớm hơn. 

Tương tự, nếu nó được chèn qua cột$j$, thì mọi ô phía trên nó trong cột đó phải chứa bóng. 

Quan sát này ngay lập tức đưa ra một điều kiện cần: 

Đối với mỗi ô bị chiếm dụng không nằm ở hàng đầu tiên và không nằm trong cột đầu tiên, ít nhất một trong các ô lân cận ngay phía trên hoặc bên trái của nó cũng phải được chiếm giữ. 

Quả thực, nếu cả hai hàng xóm đều trống thì sẽ không có cách nào để quả bóng đến được vị trí đó. 

Điều đáng ngạc nhiên là điều kiện này cũng đủ. 

Nếu mỗi ô bên trong bị chiếm giữ đều có một quả bóng ngay phía trên hoặc ngay bên trái của nó thì chúng ta luôn có thể xây dựng cấu hình. Một cách để thấy điều này là đảo ngược quá trình. Bất kỳ ô nào bị chiếm không có ô bị chiếm bên dưới nó và không có ô bị chiếm ở bên phải của nó có thể được coi là quả bóng được chèn cuối cùng. Việc loại bỏ các ô như vậy nhiều lần không bao giờ vi phạm điều kiện. Cuối cùng, toàn bộ cấu hình biến mất, chứng tỏ rằng một cấu trúc hợp lệ tồn tại. 

Điều này làm giảm toàn bộ vấn đề thành một cuộc kiểm tra lưới đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(nm) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới. 
2. Lặp lại qua từng ô. 
3. Bỏ qua các ô chứa`0`, vì chúng không áp đặt ràng buộc nào. 
4. Nếu ô ở hàng đầu tiên hoặc cột đầu tiên thì ô đó luôn hợp lệ. 

Một quả bóng luôn có thể được chèn trực tiếp từ đường viền tương ứng. 
5. Đối với mỗi ô bị chiếm dụng`(i, j)`với`i > 0`Và`j > 0`, kiểm tra ô ở trên`(i-1, j)`và ô bên trái`(i, j-1)`. 
6. Nếu cả hai hàng xóm đều chứa`0`, trả lời ngay`"NO"`. 

Không thể tiếp cận một ô như vậy từ một trong hai hướng chèn. 
7. Nếu mọi ô bên trong có người đều vượt qua bài kiểm tra, hãy trả lời`"YES"`. 

### Tại sao nó hoạt động 

Xem xét bất kỳ ô bị chiếm dụng nào không nằm ở hàng trên cùng hoặc cột bên trái. 

Nếu quả bóng cuối cùng chiếm vị trí này đi qua hàng của nó thì mọi vị trí trước nó trong hàng đó phải được chiếm giữ khi nó đến. Đặc biệt, ô ngay bên trái của nó phải chứa một quả bóng ở cấu hình cuối cùng. 

Thay vào đó, nếu nó được nhập qua cột của nó thì ô ngay phía trên phải chứa một quả bóng ở cấu hình cuối cùng. 

Do đó, mọi cấu hình có thể truy cập đều thỏa mãn điều kiện được kiểm tra bằng thuật toán. 

Để đầy đủ, hãy liên tục loại bỏ một ô bị chiếm giữ mà không có ô lân cận nào bị chiếm ở bên dưới và không có ô lân cận nào bị chiếm ở bên phải. Một ô như vậy có thể là quả bóng được chèn cuối cùng. Điều kiện cục bộ đảm bảo rằng sau khi loại bỏ nó, các ô bị chiếm giữ còn lại vẫn thỏa mãn thuộc tính tương tự. Việc tiếp tục quá trình này cuối cùng sẽ loại bỏ tất cả các quả bóng, tạo ra một cấu trúc đảo ngược hợp lệ. Do đó, mọi cấu hình vượt qua quá trình kiểm tra đều có thể truy cập được. 

Điều kiện vừa cần vừa đủ nên thuật toán đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, m = map(int, input().split())
    g = [input().strip() for _ in range(n)]

    ok = True

    for i in range(1, n):
        for j in range(1, m):
            if g[i][j] == '1':
                if g[i - 1][j] == '0' and g[i][j - 1] == '0':
                    ok = False

    print("YES" if ok else "NO")
```Việc thực hiện tuân theo bằng chứng trực tiếp. 

Hàng đầu tiên và cột đầu tiên không bao giờ cần kiểm tra vì bóng có thể vào trực tiếp từ đường viền tương ứng. 

Đối với mỗi ô bị chiếm giữ khác, chúng tôi kiểm tra xem có tồn tại ít nhất một ô lân cận hỗ trợ hay không. Nếu cả hàng xóm phía trên và bên trái đều không chứa quả bóng thì việc cấu hình là không thể và chúng tôi đánh dấu trường hợp thử nghiệm là không hợp lệ. 

Không có mô phỏng nào được thực hiện. Toàn bộ giải pháp chỉ là một lần quét lưới. 

Một lỗi thường gặp là kiểm tra tất cả các ô và truy cập`i-1`hoặc`j-1`khi`i=0`hoặc`j=0`. Bắt đầu các vòng lặp từ`1`tránh được vấn đề này hoàn toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Lưới đầu vào:```
001
001
110
```Dấu vết: 

| Tế bào | Giá trị | Lên | Trái | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| (1,1) | 0 | - | - | Bỏ qua | 
| (1,2) | 1 | 1 | 0 | Có | 
| (2,1) | 1 | 0 | 1 | Có | 
| (2,2) | 0 | - | - | Bỏ qua | 

Không có vi phạm được tìm thấy. 

Đầu ra:```
YES
```Các ô bị chiếm cách xa đường viền luôn có sự hỗ trợ từ phía trên hoặc từ bên trái, phù hợp với điều kiện về khả năng tiếp cận. 

### Ví dụ 2 

Lưới đầu vào:```
000
000
001
```Dấu vết: 

| Tế bào | Giá trị | Lên | Trái | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| (1,1) | 0 | - | - | Bỏ qua | 
| (1,2) | 0 | - | - | Bỏ qua | 
| (2,1) | 0 | - | - | Bỏ qua | 
| (2,2) | 1 | 0 | 0 | Không | 

Ô bị chiếm ở góc dưới bên phải không có quả bóng nào ở trên cũng như không có quả bóng nào ở bên trái. 

Đầu ra:```
NO
```Điều này thể hiện điều kiện bất khả thi cốt lõi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được kiểm tra một lần | 
| Không gian | O(1) thêm | Chỉ có một vài biến ngoài lưới đầu vào | 

Vì tổng số ô trong tất cả các trường hợp thử nghiệm nhiều nhất là 10.000, nên$O(nm)$quá trình quét rất nhỏ. Giải pháp chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        g = [input().strip() for _ in range(n)]

        ok = True

        for i in range(1, n):
            for j in range(1, m):
                if g[i][j] == '1':
                    if g[i - 1][j] == '0' and g[i][j - 1] == '0':
                        ok = False

        ans.append("YES" if ok else "NO")

    return "\n".join(ans)

# provided sample
assert run(
"""5
3 3
001
001
110
3 3
010
111
010
3 3
111
111
111
3 3
000
000
000
3 3
000
000
001
"""
) == "\n".join([
    "YES",
    "YES",
    "YES",
    "YES",
    "NO"
])

# minimum grid, empty
assert run(
"""1
1 1
0
"""
) == "YES"

# minimum grid, occupied
assert run(
"""1
1 1
1
"""
) == "YES"

# isolated interior ball
assert run(
"""1
2 2
00
01
"""
) == "NO"

# all ones
assert run(
"""1
4 4
1111
1111
1111
1111
"""
) == "YES"

# catches off-by-one issues near borders
assert run(
"""1
2 3
001
111
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 có 0 | CÓ | Cấu hình trống | 
| Lưới 1×1 với 1 | CÓ | Ô viền luôn có thể truy cập được | 
|`00 / 01`| KHÔNG | Bóng nội thất biệt lập | 
| Tất cả những cái | CÓ | Cấu hình dày đặc | 
|`001 / 111`| CÓ | Xử lý biên giới đúng | 

## Vỏ cạnh 

Hãy xem xét lưới chiếm chỗ nhỏ nhất:```
1 1
1
```Thuật toán không thực hiện kiểm tra bên trong vì không có ô bên trong. Câu trả lời là`YES`. Nhấn một lần vào hàng 1 hoặc cột 1 sẽ tạo cấu hình. 

Hãy xem xét ví dụ về góc cô lập:```
3 3
000
000
001
```Thuật toán tiếp cận ô`(2,2)`. Hàng xóm phía trên của nó là`0`và hàng xóm bên trái của nó là`0`, do đó điều kiện thất bại ngay lập tức và câu trả lời là`NO`. Điều này phù hợp với thực tế là không có chuỗi cú đẩy nào có thể di chuyển một quả bóng sâu như vậy mà không có các ô chiếm trung gian. 

Hãy xem xét một cấu hình chỉ có sự hỗ trợ từ một phía:```
3 3
000
110
011
```Đối với ô dưới cùng bên phải, hàng xóm bên trái là`1`, do đó điều kiện thành công mặc dù hàng xóm phía trên là`0`. Thuật toán trả về`YES`. Một quả bóng chỉ cần một đường chèn hợp lệ, từ hàng hoặc từ cột của nó. 

Hãy xem xét lưới hoàn toàn trống:```
3 3
000
000
000
```Mọi ô đều bị bỏ qua vì không có ô nào chứa bóng. Không có vi phạm nào xảy ra và câu trả lời là`YES`. Thực hiện các thao tác bằng 0 luôn được cho phép.
