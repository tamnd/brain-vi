---
title: "CF 1059B - Giả mạo"
description: "Chúng ta được cung cấp một lưới nhị phân biểu thị một bản vẽ mục tiêu được tạo từ các ô mực. Mỗi ô đã được điền hoặc trống và chúng tôi muốn xác định xem liệu mẫu cuối cùng này có thể được tạo từ một lưới trống ban đầu bằng cách sử dụng một công cụ dập cụ thể hay không."
date: "2026-06-15T09:35:15+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1059
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 514 (Div. 2)"
rating: 1300
weight: 1059
solve_time_s: 219
verified: true
draft: false
---

[CF 1059B - Giả mạo](https://codeforces.com/problemset/problem/1059/B) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới nhị phân biểu thị một bản vẽ mục tiêu được tạo từ các ô mực. Mỗi ô đã được điền hoặc trống và chúng tôi muốn xác định xem liệu mẫu cuối cùng này có thể được tạo từ một lưới trống ban đầu bằng cách sử dụng một công cụ dập cụ thể hay không. 

Công cụ này không vẽ các hình dạng tùy ý. Thay vào đó, nó luôn vẽ đường viền của hình vuông 3 x 3, giữ nguyên ô trung tâm. Mọi ứng dụng đều tập trung vào một ô nào đó và nó vẽ tám ô xung quanh trong vùng lân cận 3 x 3 đó. Bản thân trung tâm không bao giờ bị ảnh hưởng. Thao tác chỉ có thể được sử dụng khi toàn bộ khối 3 x 3 nằm bên trong lưới. 

Câu hỏi đặt ra là liệu có tồn tại một chuỗi các thao tác đóng dấu tạo ra chính xác lưới đã cho, bắt đầu từ tất cả các ô trống hay không. 

Kích thước lưới có thể lên tới 1000 x 1000, nghĩa là lên tới một triệu ô. Bất kỳ giải pháp nào cố gắng mô phỏng việc dập ở mọi vị trí có thể và liên tục cập nhật lưới theo cách đơn giản đều có nguy cơ trở nên quá chậm nếu mỗi lần kiểm tra đều tốn kém. Do đó, chúng tôi cần một phương pháp trong đó mỗi ô chỉ được xử lý với số lần không đổi hoặc ít nhất mỗi dấu tiềm năng được đánh giá trong thời gian không đổi. 

Một khó khăn tinh tế xuất hiện khi tồn tại các ô mực không thể thuộc về đường viền của bất kỳ con tem hợp lệ nào. Ví dụ, một đơn bị cô lập`#`Ở giữa một biển dấu chấm không thể giải thích được, vì mỗi con tem đều vẽ nên một vòng tám người hàng xóm. Một mô hình có vấn đề khác là`#`ô không được hỗ trợ bởi bất kỳ cấu hình 3 x 3 nào có thể, nghĩa là nó không bao giờ là một phần của bất kỳ đường viền hợp lệ nào. 

Kiểu thất bại chính của lý luận ngây thơ là giả định rằng mọi`#`phải là trung tâm của một con tem nào đó hoặc mọi thứ`#`bản thân nó phải được sơn trực tiếp. Trên thực tế, chỉ có vấn đề biên giới và phạm vi bao phủ mới có thể chồng chéo theo những cách phức tạp. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng việc dập. Đối với mọi tâm có thể có của khối 3 x 3, chúng ta có thể thử áp dụng tem nếu nó không mâu thuẫn với lưới mục tiêu và đánh dấu các ô mà nó sẽ vẽ. Sau khi thử tất cả các vị trí có thể, chúng tôi so sánh lưới được vẽ kết quả với mục tiêu. 

Cách tiếp cận này đúng về mặt khái niệm, nhưng điểm yếu của nó là không thực thi được sự cần thiết. Chúng ta có thể áp dụng những con tem không bắt buộc, và thậm chí tệ hơn, chúng ta có thể bỏ lỡ sự thật rằng một số`#`các ô không bao giờ được bao phủ trừ khi chúng tôi kiểm tra cẩn thận các ràng buộc về phạm vi bao phủ. Trong trường hợp xấu nhất, chúng tôi sẽ kiểm tra mọi ô như một trung tâm tiềm năng và đối với mỗi trung tâm, chúng tôi kiểm tra tối đa 9 ô, dẫn đến khoảng 9 triệu hoạt động, nằm ở ranh giới nhưng vẫn có thể quản lý được. Tuy nhiên, tính chính xác trở nên khó khăn vì chúng ta cần đảm bảo rằng mọi`#`có thể giải thích được chứ không chỉ có thể tái tạo bằng cách dán tem tùy ý. 

Cái nhìn sâu sắc quan trọng là đảo ngược suy nghĩ. Thay vì xây dựng lưới về phía trước, chúng tôi kiểm tra xem mọi`#`ô có thể được "căn cứ" là thuộc về đường viền của ít nhất một tem hợp lệ. Một ô chỉ có thể được vẽ nếu tồn tại một số tâm xung quanh nó sao cho nó nằm trong mẫu đường viền 3 x 3. Điều đó có nghĩa là đối với mỗi`#`, chúng ta kiểm tra xem nó có thể là hàng xóm trên, dưới, trái hoặc phải của một tâm hợp lệ nào đó hay không. Nếu không có trung tâm như vậy tồn tại thì không thể cấu hình được. 

Điều này làm giảm vấn đề xác minh tính khả thi cục bộ xung quanh mỗi ô. Chúng tôi quét tất cả các trung tâm có thể, đánh dấu các ô viền mà chúng có thể tạo ra và sau đó xác minh rằng mọi`#`được bao phủ bởi ít nhất một trung tâm tem hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n m) đến O(n m · 9) | O(n m) | Quá chậm / khó lý luận | 
| Xác thực dựa trên trung tâm | O(n m) | O(n m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi tạo một lưới boolean`good`đánh dấu xem tem 3 x 3 ở giữa tại một vị trí nhất định có hợp lệ hay không. Một trung tâm hợp lệ nếu tất cả tám ô viền đều`#`. Điều này đảm bảo rằng việc dán tem vào đó sẽ không tạo ra bất kỳ dấu chấm sai nào. 
2. Chúng tôi lặp lại tất cả các trung tâm có thể`(i, j)`trong đó một khối 3 x 3 đầy đủ phù hợp. Đối với mỗi trung tâm, chúng tôi kiểm tra tám vị trí xung quanh. Nếu họ là tất cả`#`, chúng tôi đánh dấu trung tâm này là có thể sử dụng được. 
3. Chúng tôi tạo một lưới boolean khác`covered`có cùng kích thước với lưới đầu vào, ban đầu tất cả đều sai. Điều này sẽ theo dõi những ô nào có thể được giải thích bằng ít nhất một dấu hợp lệ. 
4. Đối với mọi trung tâm hợp lệ`(i, j)`, chúng tôi đánh dấu tám ô viền của nó trong`covered`như sự thật. Điều này mô phỏng thực tế rằng con tem này có thể đã đóng góp mực vào các ô đó. 
5. Sau khi xử lý tất cả các trung tâm, chúng tôi lặp lại trên toàn bộ lưới và kiểm tra mọi`#`tế bào. Nếu có`#`không được đánh dấu trong`covered`, thì không thể giải thích được bằng bất kỳ cấu hình tem hợp lệ nào, vì vậy câu trả lời là KHÔNG. 
6. Nếu tất cả`#`các ô được che phủ, chúng tôi trả về CÓ. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi giá trị cuối cùng hợp lệ`#`ô phải có thể giải thích được như một phần của ít nhất một đường viền 3 x 3 có tâm nằm bên trong lưới. Bất kỳ cấu trúc hợp lệ nào chỉ bao gồm các tem như vậy và mỗi tem chỉ đóng góp mực cho các ô viền của nó. Do đó, nếu một ô không nằm trong bất kỳ đường viền hợp lệ nào thì không có chuỗi tem nào có thể tạo ra ô đó. Ngược lại, nếu mỗi`#`nằm trong ít nhất một đường viền hợp lệ, chúng ta có thể gán tem một cách tham lam theo bất kỳ thứ tự nào vì được phép chồng chéo và không bao giờ xóa mực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    g = [input().strip() for _ in range(n)]
    
    covered = [[False] * m for _ in range(n)]
    
    def ok(i, j):
        return (g[i-1][j-1] == '#' and g[i-1][j] == '#' and g[i-1][j+1] == '#' and
                g[i][j-1] == '#'                     and g[i][j+1] == '#' and
                g[i+1][j-1] == '#' and g[i+1][j] == '#' and g[i+1][j+1] == '#')
    
    for i in range(1, n - 1):
        for j in range(1, m - 1):
            if ok(i, j):
                covered[i-1][j-1] = True
                covered[i-1][j] = True
                covered[i-1][j+1] = True
                covered[i][j-1] = True
                covered[i][j+1] = True
                covered[i+1][j-1] = True
                covered[i+1][j] = True
                covered[i+1][j+1] = True
    
    for i in range(n):
        for j in range(m):
            if g[i][j] == '#' and not covered[i][j]:
                print("NO")
                return
    
    print("YES")

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`ok`chức năng kiểm tra xem tem có được căn giữa tại`(i, j)`là hợp lệ. Chúng tôi chỉ xem xét các trung tâm hoàn toàn nằm trong lưới, vì các trung tâm biên giới không thể tạo thành mô hình 3 x 3. 

các`covered`ma trận theo dõi khả năng tiếp cận từ tất cả các trung tâm tem hợp lệ. Mỗi trung tâm hợp lệ đóng góp vào chính xác tám ô xung quanh, khớp chính xác với quy tắc dập. Lần quét cuối cùng đảm bảo rằng không có ô mực cần thiết nào không được hỗ trợ. 

Xử lý ranh giới là rất quan trọng ở đây. Các vòng lặp kết thúc`i in range(1, n-1)`Và`j in range(1, m-1)`đảm bảo chúng tôi không bao giờ truy cập vào những người hàng xóm ngoài giới hạn khi kiểm tra khối 3 x 3. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
###
#.#
###
```Chúng tôi đánh giá trung tâm duy nhất có thể tại`(1, 1)`trong lập chỉ mục dựa trên 0 (trung tâm của lưới). 

| Trung tâm | Con dấu hợp lệ? | Tế bào được bảo hiểm | 
| --- | --- | --- | 
| (1,1) | Có | tất cả các ô viền | 

Sau khi đánh dấu, tất cả`#`các ô ở viền bị che và ô ở giữa bị bỏ qua. 

Mọi`#`có hỗ trợ, vì vậy đầu ra là CÓ. 

### Mẫu 2 (mẫu không thể về mặt khái niệm) 

đầu vào:```
3 3
###
#..
###
```| Trung tâm | Con dấu hợp lệ? | Tế bào được bảo hiểm | 
| --- | --- | --- | 
| (1,1) | Không | không | 

Không có tem hợp lệ vì không phải tất cả các ô viền đều`#`. Vì vậy không có bảo hiểm nào được tạo ra. 

Ô ở dưới cùng bên trái hoặc trên cùng bên phải`#`không thể giải thích được nên kết quả là KHÔNG. 

Điều này cho thấy các đường viền một phần phá vỡ khả năng xây dựng cấu hình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được kiểm tra một số lần không đổi như một phần của tối đa một lần quét trung tâm | 
| Không gian | O(nm) | Chúng tôi lưu trữ một lưới phủ sóng có cùng kích thước với đầu vào | 

Kích thước lưới tối đa là một triệu ô và mỗi ô được xử lý với công việc liên tục. Điều này phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve_capture()

def solve_capture():
    n, m = map(int, input().split())
    g = [input().strip() for _ in range(n)]
    covered = [[False]*m for _ in range(n)]

    def ok(i, j):
        return (g[i-1][j-1] == '#' and g[i-1][j] == '#' and g[i-1][j+1] == '#' and
                g[i][j-1] == '#' and g[i][j+1] == '#' and
                g[i+1][j-1] == '#' and g[i+1][j] == '#' and g[i+1][j+1] == '#')

    for i in range(1, n-1):
        for j in range(1, m-1):
            if ok(i, j):
                covered[i-1][j-1] = covered[i-1][j] = covered[i-1][j+1] = True
                covered[i][j-1] = covered[i][j+1] = True
                covered[i+1][j-1] = covered[i+1][j] = covered[i+1][j+1] = True

    for i in range(n):
        for j in range(m):
            if g[i][j] == '#' and not covered[i][j]:
                return "NO"
    return "YES"

# provided sample
assert run("""3 3
###
#.#
###
""") == "YES"

# isolated impossible
assert run("""3 3
###
#..
###
""") == "NO"

# minimum valid-ish pattern
assert run("""3 3
###
###
###
""") == "YES"

# all dots
assert run("""3 3
...
...
...
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khối 3x3 đầy đủ | CÓ | giá trị trung tâm duy nhất | 
| biên giới bị hỏng | KHÔNG | không thể đóng dấu một phần | 
| lưới đầy đủ | CÓ | phạm vi bảo hiểm tối đa | 
| lưới trống | CÓ | không có ràng buộc bắt buộc | 

## Vỏ cạnh 

Trường hợp góc là khi lưới chứa`#`các ô gần ranh giới. Ví dụ, một`#`ở vị trí`(0, 0)`không bao giờ có thể bị che phủ bởi bất kỳ con tem nào vì không có khối 3 x 3 nào bao gồm nó làm ô viền. Thuật toán xử lý việc này một cách tự nhiên vì không có trung tâm hợp lệ nào có thể đánh dấu nó trong`covered`, vì vậy nó được phát hiện là không được che phủ. 

Một trường hợp khác là khi tất cả các ô đều`#`. Mỗi trung tâm`(i, j)`với vùng lân cận 3 x 3 đầy đủ sẽ trở nên hợp lệ và mọi ô đều được bao phủ bởi nhiều tem chồng chéo. Thuật toán vẫn chỉ yêu cầu ít nhất một vùng phủ sóng, do đó, nó xuất ra CÓ một cách chính xác mà không cần phải giải thích về cấu trúc chồng chéo.
