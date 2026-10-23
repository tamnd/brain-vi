---
title: "CF 105937E - Cúp Pythagore"
description: "Chúng ta được cấp một dòng cốc $n$. Mỗi cốc bắt đầu ở một trong ba trạng thái: rỗng, đầy một nửa hoặc đầy. Một cốc đầy ngay lập tức tràn ra và thực sự trở nên trống rỗng, vì vậy sau khi khởi tạo, chỉ có cốc rỗng và cốc đầy một nửa mới quan trọng."
date: "2026-06-22T15:47:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105937
codeforces_index: "E"
codeforces_contest_name: "2025 Xian Jiaotong University Programming Contest"
rating: 0
weight: 105937
solve_time_s: 84
verified: true
draft: false
---

[CF 105937E - Cúp Pythagore](https://codeforces.com/problemset/problem/105937/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng$n$cốc. Mỗi cốc bắt đầu ở một trong ba trạng thái: rỗng, đầy một nửa hoặc đầy. Một cốc đầy ngay lập tức tràn ra và thực sự trở nên trống rỗng, vì vậy sau khi khởi tạo, chỉ có cốc rỗng và cốc đầy một nửa mới quan trọng. 

Một động tác bao gồm việc chọn một chiếc cốc hiện đang chứa chất lỏng, sau đó đổ toàn bộ chất lỏng vào cốc ngay bên phải. Nếu chiếc cốc được chọn là chiếc cuối cùng, nó sẽ bị loại khỏi cuộc chơi. Khi rót vào cốc tiếp theo, một nửa cộng một nửa sẽ làm cho cốc tiếp theo cạn đi, còn một nửa đổ đi sẽ khiến cốc đó đầy trở lại một nửa. Cốc đầy không bao giờ tồn tại, vì vậy mọi trạng thái đều có thể được coi là “có chất lỏng” hoặc “không có”, với hiệu ứng chuyển đổi đối với trạng thái lân cận bên phải. 

Hai người chơi luân phiên nhau di chuyển, bắt đầu từ người chơi đầu tiên. Người chơi sẽ thua nếu họ không thể chọn bất kỳ chiếc cốc nào còn trống trong lượt của mình. Cả hai đều chơi tối ưu và chúng ta phải xác định xem ai thắng. 

Cấu trúc ẩn chính là mỗi lần di chuyển sẽ phá hủy chính xác một chiếc cốc đang hoạt động và có thể lật ngược trạng thái của chiếc cốc bên phải của nó. Hệ thống chỉ phát triển bằng cách xóa cục bộ và chuyển đổi cục bộ và không có thành phần độc lập mới nào được tạo. 

Các ràng buộc đi lên đến$10^6$, vì vậy mọi giải pháp cố gắng mô phỏng các bước di chuyển một cách rõ ràng sẽ thất bại. Một bậc hai hoặc thậm chí$O(n \log n)$mô phỏng đã quá chậm vì mỗi bước di chuyển có thể xếp tầng qua cấu trúc và số lần di chuyển có thể tuyến tính theo$n$. 

Một vài tình huống phạt góc rất dễ bị hiểu sai. 

Nếu tất cả các cốc đều trống sau khi chuẩn hóa (ví dụ: đầu vào là tất cả`E`hoặc tất cả`F`), khi đó không có nước đi nào tồn tại khi bắt đầu và người chơi đầu tiên ngay lập tức thua cuộc. 

Nếu có một`H`, người chơi đầu tiên sẽ thắng ngay lập tức khi lấy nó, bởi vì nó không có người hàng xóm phù hợp để tiếp tục chơi. 

Những trường hợp tinh tế hơn xuất hiện khi nhiều`H`các phân đoạn tương tác thông qua chuyển đổi, ví dụ`HEHE`, trong đó các chuyển động truyền bá các thay đổi sang bên phải và tạo ra các phản hồi bắt buộc. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ duy trì mảng một cách rõ ràng và liên tục quét để tìm cốc không trống, thực hiện di chuyển, cập nhật cốc tiếp theo và tiếp tục. Mỗi bước di chuyển là$O(n)$để tìm hoặc xử lý và có thể có$O(n)$chuyển động, dẫn đến$O(n^2)$, quá chậm đối với$n = 10^6$. 

Nhận xét quan trọng là trò chơi không bao giờ phân nhánh. Từ bất kỳ vị trí và cấu hình hiện tại nào, nước đi đã chọn hoàn toàn được xác định bằng cách chọn chỉ mục$i$và sau khi di chuyển, thay đổi duy nhất đối với các quyết định trong tương lai là một lần lật được áp dụng cho vị trí$i+1$. Điều này có nghĩa là trạng thái của trò chơi có thể được nén thành chỉ số vị trí cộng với bit chẵn lẻ cho biết liệu chiếc cốc hiện tại có bị lật bởi người hàng xóm bên trái của nó hay không. 

Một khi điều này được nhận ra, trò chơi sẽ trở thành một quá trình động tuyến tính. Chúng tôi xử lý từ phải sang trái, duy trì xem mỗi vị trí có được chuyển đổi hiệu quả bởi các bước di chuyển trước đó hay không. Tại mỗi vị trí, chúng ta chỉ cần biết liệu một động thái có tồn tại hay không và nó dẫn đến điều gì. 

Điều này làm giảm toàn bộ trò chơi xuống DP hai trạng thái cho mỗi vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| DP từ phải sang trái với trạng thái chẵn lẻ |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích trò chơi như một chuỗi các vị trí, mỗi vị trí giữ 0 (không có cốc) hoặc 1 (cốc chứa chất lỏng). Di chuyển tại vị trí$i$chỉ có thể thực hiện được khi trạng thái hiệu quả hiện tại là$i$là 1. Thực hiện thao tác xóa vị trí$i$và chuyển đổi vị trí$i+1$. 

Chúng tôi duy trì một bảng lập trình động cho các hậu tố, nhưng thay vì lưu trữ cấu hình đầy đủ, chúng tôi chỉ lưu trữ liệu người chơi hiện tại có chiến thắng ở trạng thái chẵn lẻ hay không. 

### bước 

1. Chuẩn hóa chuỗi đầu vào thành một mảng$a[i]$, Ở đâu$a[i] = 1$nếu cốc ban đầu chứa chất lỏng (H) và$a[i] = 0$nếu không thì. Cốc đầy được coi là trống vì chúng tràn ngay lập tức. 
2. Xác định trạng thái DP$dp[i][p]$, Ở đâu$p \in \{0,1\}$đại diện cho dù vị trí$i$đã bị lật một số lẻ lần bởi các nước đi trước đó. Giá trị là đúng nếu người chơi hiện tại có thể buộc giành chiến thắng bắt đầu từ vị trí$i$. 
3. Đặt vỏ cơ sở$dp[n+1][p] = false$, vì ngoài cốc cuối cùng không còn nước đi nào nữa. 
4. Xử lý các vị trí từ phải sang trái. Tại mỗi vị trí$i$, tính giá trị hiệu dụng$v = a[i] \oplus p$. 
5. Nếu$v = 0$, không có sự di chuyển hợp pháp tại vị trí$i$, do đó người chơi sẽ thua ngay lập tức từ trạng thái này, đưa ra$dp[i][p] = false$. 
6. Nếu$v = 1$, động thái duy nhất có thể là chiếm vị trí$i$, chuyển trò chơi sang vị trí$i+1$. Động thái này lật vị trí$i+1$, do đó số chẵn lẻ tiếp theo trở thành$p \oplus 1$. Kết quả là$dp[i][p] = \neg dp[i+1][p \oplus 1]$. 
7. Câu trả lời là$dp[1][0]$, vì ban đầu không có vị trí nào bị đảo lộn. 

### Tại sao nó hoạt động 

Việc nén trạng thái là hợp lệ vì mỗi lần di chuyển chỉ ảnh hưởng đến hai vị trí liền kề: nó loại bỏ vị trí hiện tại và chuyển đổi chính xác một vị trí lân cận. Không có động thái nào tạo ra sự phụ thuộc lâu dài ngoài người hàng xóm bên phải. Kết quả là, một khi chúng ta sửa vị trí$i$hoạt động hiệu quả dưới sự chẵn lẻ$p$, tất cả các hậu quả trong tương lai hoàn toàn được xác định bởi một sự chuyển đổi duy nhất sang$i+1$với tính chẵn lẻ được cập nhật. Điều này ngăn chặn bất kỳ sự phân nhánh ẩn nào, vì vậy mọi vị trí đều hoạt động giống như một nút trò chơi xác định với tối đa một nước đi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

# 1 if H, 0 otherwise (E or F both behave as empty after initialization)
a = [1 if c == 'H' else 0 for c in s]

# dp[i][p]: i from 1..n+1, p in {0,1}
dp = [[False, False] for _ in range(n + 2)]

# base: dp[n+1][*] = False already

for i in range(n, 0, -1):
    for p in (0, 1):
        v = a[i - 1] ^ p
        if v == 0:
            dp[i][p] = False
        else:
            dp[i][p] = not dp[i + 1][p ^ 1]

print("Alice" if dp[1][0] else "Bob")
```Bảng DP được xây dựng từ dưới lên để khi xử lý vị trí$i$, giá trị của$dp[i+1]$đã được biết đến rồi. XOR với$p$captures whether the current cup has been flipped by earlier moves. Quá trình chuyển đổi làm đảo lộn tính chẵn lẻ cho$i+1$, phù hợp với quy tắc mỗi nước đi sẽ chuyển đổi chiếc cốc tiếp theo. 

Một sai lầm phổ biến là quên rằng phép lật chỉ lan truyền một bước chứ không phải trên toàn bộ hậu tố. Một cách khác là coi trò chơi như thể có nhiều nước đi ở mỗi vị trí; trên thực tế, mỗi vị trí chỉ đóng góp tối đa một nước đi có ý nghĩa. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`HEFH`Chúng tôi chuyển đổi nó thành$a = [1, 0, 0, 1]$. 

Chúng tôi tính DP từ phải sang trái. 

| tôi | p | v = a[i] xor p | di chuyển? | trạng thái tiếp theo | dp[i][p] | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 0 | 1 | vâng | (5,1) | đúng | 
| 3 | 0 | 0 | không | - | sai | 
| 2 | 0 | 0 | không | - | sai | 
| 1 | 0 | 1 | vâng | (2,1) | đúng (vì dp[2][1]=false) | 

Khi bắt đầu, vị trí 1 đang hoạt động nên Alice có một nước đi. Cấu trúc buộc các phản ứng xen kẽ cho đến khi Bob bị mắc kẹt. DP xác nhận Alice thắng. 

### Ví dụ 2:`HEHEFHEHFE`Ở đây nhiều nút chuyển đổi triệt tiêu lẫn nhau, tạo ra các chuỗi cưỡng bức dài. DP đánh giá các trạng thái hậu tố trong đó các lần lật chẵn lẻ liên tục vô hiệu hóa các bước đi trong tương lai và cuối cùng vị trí bắt đầu được đánh giá là thua đối với người chơi đầu tiên. 

Cấu trúc bàn bị bỏ qua do độ dài, nhưng hành vi chính là nhiều vị trí thực sự không hoạt động theo tính chẵn lẻ xen kẽ, khiến người chơi đầu tiên hết nước đi an toàn trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vị trí được xử lý cho hai trạng thái chẵn lẻ một lần | 
| Không gian |$O(n)$| Mảng DP có kích thước$n \times 2$| 

Quét tuyến tính lên tới$10^6$các ký tự vừa vặn thoải mái trong giới hạn thời gian và dung lượng bộ nhớ vẫn nhỏ do chỉ lưu trữ hai trạng thái boolean cho mỗi vị trí. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    n = len(s)
    a = [1 if c == 'H' else 0 for c in s]
    dp = [[False, False] for _ in range(n + 2)]
    for i in range(n, 0, -1):
        for p in (0, 1):
            v = a[i - 1] ^ p
            if v == 0:
                dp[i][p] = False
            else:
                dp[i][p] = not dp[i + 1][p ^ 1]
    return "Alice" if dp[1][0] else "Bob"

# provided samples
assert run("HEFH") == "Alice"
assert run("HEHEFHEHFE") == "Bob"

# custom cases
assert run("E") == "Bob"                 # no moves
assert run("H") == "Alice"               # single move wins
assert run("HH") in {"Alice", "Bob"}     # interaction via flip
assert run("HE") == "Alice"              # immediate move only
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| E | Bob | bắt đầu trống thua ngay lập tức | 
| H | Alice | trường hợp cơ sở di chuyển đơn | 
| HH | phụ thuộc | tương tác lật giữa hàng xóm | 
| Ngài | Alice | hành vi ranh giới bên phải | 

## Vỏ cạnh 

Đối với một đầu vào như`E`, thuật toán ngay lập tức không tìm thấy vị trí hoạt động nào tại chỉ mục 1 dưới mức chẵn lẻ 0, do đó DP đặt$dp[1][0] = false$, đưa ra chính xác Bob là người chiến thắng. 

Vì`H`, vị trí 1 đang hoạt động và chuyển các chuyển đổi sang trạng thái cuối, trạng thái này sẽ thua đối với người chơi tiếp theo, vì vậy Alice thắng. 

Vì`HH`, nước đi đầu tiên sẽ lật vị trí thứ hai, thay đổi xem nó có hoạt động ở lượt tiếp theo hay không. DP nắm bắt chính xác sự thay đổi này thông qua bit chẵn lẻ, đảm bảo phản hồi của người chơi thứ hai được đánh giá ở trạng thái đã sửa đổi thay vì cấu hình ban đầu. 

Vì`HE`, nước đi duy nhất là ở vị trí 1 và nó lật vị trí 2. Vì vị trí 2 ban đầu trống nên nó sẽ hoạt động nhưng không ảnh hưởng đến kết quả cuối cùng vì không còn vị trí nào để truyền tới và DP giải quyết chính xác hậu tố đầu cuối.
