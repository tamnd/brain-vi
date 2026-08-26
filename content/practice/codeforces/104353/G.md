---
title: "CF 104353G - Trò chơi dây II"
description: "Hai người chơi Alice và Bob bắt đầu với hai chuỗi có độ dài bằng nhau. Các chuỗi chỉ chứa các chữ cái tiếng Anh viết thường. Họ liên tục thực hiện một trò chơi trong đúng số vòng $P$."
date: "2026-07-01T18:12:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104353
codeforces_index: "G"
codeforces_contest_name: "2023 Xiangtan University Programming Contest"
rating: 0
weight: 104353
solve_time_s: 47
verified: true
draft: false
---

[CF 104353G - Trò chơi dây II](https://codeforces.com/problemset/problem/104353/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi Alice và Bob bắt đầu với hai chuỗi có độ dài bằng nhau. Các chuỗi chỉ chứa các chữ cái tiếng Anh viết thường. Họ liên tục thực hiện một trò chơi cho chính xác$P$vòng. Trong mỗi vòng, Alice buộc phải sửa đổi một ký tự trong chuỗi của Bob và Bob buộc phải sửa đổi một ký tự trong chuỗi của Alice. Sửa đổi có nghĩa là chọn một vị trí và thay thế ký tự ở đó bằng bất kỳ chữ cái viết thường nào khác mà họ chọn. 

Rốt cuộc$P$vòng, mỗi người chơi chỉ đánh giá chuỗi của riêng mình. Điểm của người chơi được xác định là tần số của ký tự phổ biến nhất trong chuỗi cuối cùng của họ. Người chiến thắng là người chơi có số điểm cao hơn và hòa sẽ tạo ra kết quả hòa. Cả hai người chơi đều chơi tối ưu với đầy đủ kiến ​​thức về các dây ban đầu. 

Khó khăn chính là mỗi thao tác đều mang tính đối nghịch giữa các chuỗi nhưng hợp tác trong chiến lược riêng của người chơi: Alice chỉ cải thiện điểm số cuối cùng của Alice bằng cách làm hỏng chuỗi của Bob chứ không phải của chính cô ấy và ngược lại. Việc tối ưu hóa hoàn toàn là cách định hình sự phân bố tần suất của các chữ cái theo một số lần chỉnh sửa cố định. 

Những hạn chế là rất lớn về mặt$P$, lên đến$10^9$, trong khi$N$chỉ tối đa 500. Điều này ngay lập tức loại trừ mọi mô phỏng qua các vòng. Giải pháp chỉ được phụ thuộc vào cấu trúc của phân bố tần số ban đầu và mức độ “thay đổi hiệu quả” có thể đạt được trên mỗi lần di chuyển. 

Trường hợp cạnh tinh tế xuất hiện khi$P = 0$. Không thể thay đổi được, vì vậy câu trả lời hoàn toàn được xác định bởi tần số ký tự tối đa ban đầu trong mỗi chuỗi. Một trường hợp góc khác là khi các dây đã đồng nhất. Trong những trường hợp như vậy, việc thay đổi một nhân vật có thể làm giảm hoặc tăng sự tập trung tùy theo chiến lược, nhưng vì cả hai người chơi đều hành động đối xứng nên hiệu ứng thực sự chỉ phụ thuộc vào số lượng thay đổi được thực hiện và cách chúng được phân bổ trên các chữ cái. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ mô phỏng trò chơi. Mỗi vòng, Alice sẽ cố gắng chọn một vị trí trong chuỗi của Bob để giảm thiểu tần số tối đa cuối cùng của Bob, trong khi Bob cũng làm như vậy với Alice. Tuy nhiên, sau mỗi lần sửa đổi, lựa chọn tối ưu phụ thuộc vào sự phân bố đầy đủ các chữ cái hiện tại và việc tính toán lại các nước đi tốt nhất sẽ cần đến$O(N)$mỗi lần di chuyển. Với$P$lên đến$10^9$, điều này hoàn toàn không thể thực hiện được. Ngay cả khi chúng tôi giới hạn mô phỏng ở mức$P \le 500$, không gian trạng thái vẫn phức tạp vì mỗi lần di chuyển sẽ thay đổi chữ cái mục tiêu tốt nhất một cách linh hoạt. 

Quan sát quan trọng là điều duy nhất quan trọng đối với điểm số cuối cùng của chuỗi là kích thước của lớp tần số lớn nhất của nó. Mỗi thao tác thay đổi một ký tự, do đó, nó sẽ giảm một số tần số và tăng một ký tự khác. Từ cấp độ cao, mỗi người chơi đều muốn tối đa hóa số lượng ký tự có thể chuyển đổi thành một chữ cái chi phối duy nhất. 

Giả sử một chuỗi có mảng tần số$f_1, f_2, \dots, f_{26}$. Nếu chúng ta muốn tối đa hóa chữ cái thường xuyên nhất sau$x$thay đổi, chiến lược tốt nhất luôn là chọn chữ cái tốt nhất hiện tại làm mục tiêu và chuyển đổi tất cả các chữ cái khác thành chữ cái đó. Mỗi thao tác có thể tăng tần số tốt nhất lên tối đa 1, nhưng chỉ cho đến khi hết tất cả các ký tự khác. 

Như vậy, sau$x$hoạt động, tần số tối đa có thể trở thành:$$\min(N, \max(f_i) + x)$$bởi vì mỗi lần di chuyển có thể tăng số lượng chữ cái trội lên tối đa một và chúng ta không thể vượt quá độ dài chuỗi. 

Bây giờ sự tương tác giữa Alice và Bob trở nên đối xứng và tách rời: mỗi người chơi sử dụng$P$di chuyển trên chuỗi của đối thủ, nhưng chỉ có chuỗi cuối cùng của họ mới quan trọng. Vì vậy, điểm của Alice phụ thuộc vào chuỗi ban đầu của Alice và khả năng rút gọn chuỗi đó của Bob và ngược lại. 

Tuy nhiên, vì Bob chỉ thay đổi chuỗi của Alice nên Bob đang cố gắng giảm thiểu tần số tối đa của Alice một cách hiệu quả. Cách tốt nhất để giảm mức tần suất là nhắm mục tiêu vào chữ cái thường xuyên nhất trước tiên. Mỗi thao tác chỉ giảm mức tối đa hiện tại đi 1 nếu nó chạm vào chữ cái đó; nếu không nó sẽ bị lãng phí đối với các chữ cái không chiếm ưu thế. Vì Bob luôn có thể chọn một cách tối ưu nên anh ta sẽ luôn tấn công chữ cái chiếm đa số hiện tại trong chuỗi của Alice. Đối xứng với Alice. 

Vì vậy kết quả tối ưu của mỗi người chơi chỉ phụ thuộc vào việc đối phương có đủ thao tác để “phá vỡ” sự tập trung đa số hay không. 

Cho phép$mx_1$là tần số tối đa trong$S_1$,$mx_2$vì$S_2$. Sau đó$P$di chuyển từ Bob tới Alice, điểm cuối cùng tốt nhất có thể có của Alice sẽ trở thành:$$\max(1, mx_1 - P)$$bởi vì mỗi nước đi có thể giảm tần số chiếm ưu thế tối đa một lần cho đến khi nó đạt đến 1 (không thể xuống dưới 1 vì vẫn còn ít nhất một lần xuất hiện). 

Tương tự:$$\max(1, mx_2 - P)$$Do đó trò chơi chuyển sang so sánh hai giá trị này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force |$O(P \cdot N)$|$O(N)$| Quá chậm | 
| Mô hình giảm tần số |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng ký tự trong cả hai chuỗi. 

Điều này cung cấp bản tóm tắt đầy đủ về cấu trúc của từng chuỗi, đủ vì các thao tác chỉ ảnh hưởng đến số lượng chứ không ảnh hưởng đến vị trí. 
2. Tính toán$mx_1$Và$mx_2$, tần số tối đa trong chuỗi ban đầu của Alice và Bob. 

Điểm chỉ phụ thuộc vào ký tự xuất hiện thường xuyên nhất, vì vậy các tần số khác có thể bị bỏ qua. 
3. Tác động của Mô hình Bob lên Alice là làm giảm tần số tối đa của Alice tới$P$, sản xuất$a = \max(1, mx_1 - P)$. 

Mỗi thao tác có thể loại bỏ một lần xuất hiện của ký tự chi phối hiện tại cho đến khi nó không còn chiếm ưu thế nữa. 
4. Tính toán tương tự$b = \max(1, mx_2 - P)$về tác động của Alice đối với Bob. 
5. So sánh$a$Và$b$. Nếu như$a > b$, Alice thắng. Nếu như$a < b$, Bob thắng. Ngược lại kết quả là hòa. 

### Tại sao nó hoạt động 

Điều bất biến chính là việc nhận dạng chữ cái thường xuyên nhất trong chuỗi không cần phải được theo dõi rõ ràng trong khi chơi. Chỉ có số lượng của nó là quan trọng và mọi bước đi tối ưu luôn nhắm vào tầng lớp đa số hiện tại. Điều này đảm bảo rằng không có chiến lược nào có thể rút ra nhiều hơn một đơn vị giảm điểm cho mỗi nước đi từ điểm của đối thủ và không có chiến lược nào có thể tăng nhiều hơn một điểm cho mỗi nước đi. Kết quả là quá trình này được ghi lại hoàn toàn bằng cách điều chỉnh tuyến tính tần số tối đa ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, p = map(int, input().split())
        s1 = input().strip()
        s2 = input().strip()

        f1 = [0] * 26
        f2 = [0] * 26

        for c in s1:
            f1[ord(c) - 97] += 1
        for c in s2:
            f2[ord(c) - 97] += 1

        mx1 = max(f1)
        mx2 = max(f2)

        a = max(1, mx1 - p)
        b = max(1, mx2 - p)

        if a > b:
            print("Alice")
        elif a < b:
            print("Bob")
        else:
            print("Draw")

if __name__ == "__main__":
    solve()
```Việc triển khai nén từng chuỗi thành một mảng tần số có độ dài 26. Điều này tránh mọi nhu cầu theo dõi vị trí hoặc mô phỏng hoạt động. 

Chi tiết tinh tế duy nhất là kẹp bằng`max(1, mx - p)`. Kể cả nếu$P$đủ lớn để vượt quá tần số tối đa hiện tại, điểm số không thể giảm xuống 0 vì vẫn còn ít nhất một lần xuất hiện của một chữ cái nào đó. Việc triển khai thực thi giới hạn dưới này một cách trực tiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5 2
aaaaa
bbbbb
```Alice bắt đầu với tần số tối đa 5, Bob cũng 5. 

| Bước | mx1 | mx2 | Điểm Alice | Điểm Bob | 
| --- | --- | --- | --- | --- | 
| ban đầu | 5 | 5 | 5 | 5 | 
| sau khi giảm | 5 | 5 | 3 | 3 | 

Cả hai đều giảm đi 2 thao tác, vì vậy cả hai đều kết thúc ở 3. 

Điều này xác nhận tính đối xứng dẫn đến kết quả hòa. 

Đầu ra: Vẽ 

### Ví dụ 2 

đầu vào:```
1
6 1
aaaaaa
abbbbb
```| Bước | mx1 | mx2 | Điểm Alice | Điểm Bob | 
| --- | --- | --- | --- | --- | 
| ban đầu | 6 | 5 | 6 | 5 | 
| sau khi giảm | 6 | 5 | 5 | 4 | 

Alice vẫn có nồng độ cuối cùng cao hơn. 

Đầu ra: Alice 

Điều này chứng tỏ rằng ngay cả một sự khác biệt nhỏ về tần số tối đa ban đầu vẫn tồn tại sau khi giảm bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot N)$| Mỗi bài kiểm tra đếm tần số trên hai chuỗi có độ dài$N$| 
| Không gian |$O(1)$| Chỉ có 26 bộ đếm được sử dụng cho mỗi trường hợp thử nghiệm | 

Các ràng buộc cho phép lên tới 2000 trường hợp thử nghiệm và$N \le 500$, vậy nhiều nhất là khoảng$10^6$hoạt động của nhân vật, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like tests
assert run("""1
5 0
aabbb
bbbbb
""") == "Bob"

assert run("""1
5 0
aaaaa
aaaab
""") == "Alice"

# edge: zero operations
assert run("""1
4 0
abcd
wxyz
""") == "Draw"

# edge: large P
assert run("""1
3 100
abc
def
""") == "Draw"

# edge: identical strings
assert run("""1
6 2
aabbbb
aabbbb
""") == "Draw"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoạt động bằng không | Vẽ | chỉ so sánh cơ bản | 
| P lớn | Vẽ | kẹp tại 1 công trình | 
| chuỗi giống hệt nhau | Vẽ | xử lý đối xứng | 
| tần số lệch | Alice/Bob | sự nhạy cảm thống trị | 

## Vỏ cạnh 

Khi nào$P = 0$, thuật toán so sánh trực tiếp tần số tối đa ban đầu. Ví dụ, với`abcd`vs`aabc`, cả hai cực đại lần lượt là 1 và 2 nên Bob thắng ngay. Không áp dụng mức giảm nào và công thức duy trì chính xác trạng thái ban đầu. 

Khi$P$là cực kỳ lớn, nói$10^9$, cả hai chuỗi đều được giảm xuống điểm tối thiểu là 1 bất kể cấu trúc. Ví dụ,`aaaaa`vs`bbbbb`trở thành 1 vs 1, tạo ra một trận hòa. Việc kẹp đảm bảo không xuất hiện tần số âm hoặc bằng 0. 

Khi các chuỗi giống hệt nhau, cả hai$mx_1$Và$mx_2$bằng nhau và sau khi giảm như nhau thì cả hai vẫn bằng nhau. Ngay cả khi một chuỗi có nhiều chữ cái đa dạng hơn, tần số tối đa vẫn chiếm ưu thế và đảm bảo so sánh nhất quán bất kể phân bố ở nơi nào khác.
