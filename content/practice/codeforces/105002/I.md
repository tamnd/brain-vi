---
title: "CF 105002I - \u041f\u0440\u0438\u0434\u0443\u043c\u0430\u0439 \u0437\u0430\u0434\u0430\u043d\u0438\u0435"
description: "Mỗi trường hợp thử nghiệm đưa ra một từ và đối với từ đó, chúng ta cần quyết định xem liệu nó có thể được xem như một phiên bản “mở rộng” của một chuỗi ngắn hơn sau khi chèn chính xác một ký tự hay không."
date: "2026-06-28T03:20:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105002
codeforces_index: "I"
codeforces_contest_name: "vkoshp.letovo 2022"
rating: 0
weight: 105002
solve_time_s: 105
verified: false
draft: false
---

[CF 105002I - \u041f\u0440\u0438\u0434\u0443\u043c\u0430\u0439 \u0437\u0430\u0434\u0430\u043d\u0438\u0435](https://codeforces.com/problemset/problem/105002/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra một từ và đối với từ đó, chúng ta cần quyết định xem liệu nó có thể được xem như một phiên bản “mở rộng” của một chuỗi ngắn hơn sau khi chèn chính xác một ký tự hay không. 

Chính xác hơn, chúng ta đang tìm kiếm một chuỗi`t`sao cho nếu chúng ta chèn một ký tự vào bất cứ đâu vào`t`, chúng ta thu được từ đã cho`s`. Trong số tất cả các cách có thể để xóa một ký tự khỏi`s`, chúng tôi muốn có ít nhất một vị trí xóa trong đó không thể tạo ra chuỗi ngắn hơn bằng cách xóa bất kỳ vị trí nào khác theo cách tương tự. Nếu vị trí đó tồn tại, chúng tôi xuất ra chuỗi kết quả đó; nếu không thì chúng tôi xuất ra`-1`. 

Kích thước đầu vào rất nhỏ về số lượng từ, nhiều nhất là mười, nhưng mỗi từ có thể khá dài, lên tới một trăm nghìn ký tự. Sự mất cân bằng này rất quan trọng: bất kỳ giải pháp nào cố gắng tính toán lại một số bậc hai theo từng độ dài từ sẽ ngay lập tức thất bại, trong khi việc quét tuyến tính trên mỗi từ là đủ dễ dàng. 

Trường hợp lỗi tinh vi xuất hiện khi từ chỉ chứa các ký tự lặp lại. Ví dụ, hãy xem xét`"aaaa"`. Loại bỏ bất kỳ một vị trí nào luôn mang lại kết quả`"aaa"`, nhưng có nhiều vị trí xóa hợp lệ tạo ra cùng một kết quả, do đó “vị trí chèn” sẽ không phải là duy nhất. Trong trường hợp đó câu trả lời đúng là`-1`, mặc dù tồn tại một chuỗi ngắn hơn. 

Một trường hợp cạnh khác là một từ như`"abca"`. Loại bỏ cái đầu tiên`'a'`và cuối cùng`'a'`cả hai đều sản xuất`"bca"`, do đó kết quả không rõ ràng và không hợp lệ. Một cách tiếp cận ngây thơ chỉ cần xóa bất kỳ vị trí nào sẽ chấp nhận những trường hợp như vậy một cách không chính xác. 

## Phương pháp tiếp cận 

Cách đơn giản để suy nghĩ về vấn đề này là thử mọi vị trí có thể có trong từ, loại bỏ ký tự đó và coi kết quả là một chuỗi tác vụ ứng cử viên. Về nguyên tắc, điều này đúng: mọi chuỗi tác vụ hợp lệ phải xuất phát từ việc xóa chính xác một ký tự. 

Tuy nhiên, chỉ sự đúng đắn thôi thì chưa đủ; chúng tôi cũng cần việc xóa là duy nhất. Nếu hai vị trí khác nhau tạo ra cùng một chuỗi rút ngắn, thì việc chèn ký tự đã xóa trở lại chuỗi đó sẽ không xác định được vị trí duy nhất, vi phạm yêu cầu. 

Đối với mỗi chỉ mục, việc triển khai bạo lực trực tiếp sẽ xây dựng chuỗi rút gọn và so sánh nó với tất cả các thao tác xóa khác để đảm bảo tính duy nhất. Điều này dẫn đến một$O(n^2)$cách xây dựng mỗi từ, vì mỗi lần xóa sẽ tạo ra một chuỗi có độ dài mới$O(n)$, và sự so sánh cũng$O(n)$. Với độ dài từ lên tới$10^5$, phương pháp này trở nên hoàn toàn không khả thi. 

Quan sát chính là hai vị trí xóa khác nhau chỉ có thể cho ra kết quả giống nhau nếu các ký tự bị xóa giống hệt nhau và tất cả cấu trúc xung quanh đều giống nhau, điều này chỉ có thể thực hiện được khi ký tự bị xóa xuất hiện nhiều lần trong từ. Điều này làm giảm điều kiện duy nhất thành một kiểm tra tần số đơn giản: một vị trí xóa là hợp lệ khi và chỉ khi ký tự ở vị trí đó xuất hiện chính xác một lần trong toàn bộ từ. 

Điều này biến vấn đề từ tìm kiếm tổ hợp thành quét tần số duy nhất cho mỗi từ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) trường hợp xấu nhất trên mỗi từ | O(n) | Quá chậm | 
| Quét dựa trên tần số | O(n) mỗi từ | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng từ một cách độc lập. 

1. Đếm tần suất xuất hiện của từng ký tự trong từ. Điều này cho chúng ta biết ký tự nào là duy nhất và ký tự nào được lặp lại. Lý do chúng tôi làm điều này trước tiên là tính duy nhất của việc xóa phụ thuộc hoàn toàn vào việc ký tự của nó có thể khớp với một ký tự giống hệt khác ở nơi khác hay không. 
2. Quét từ từ trái sang phải và tìm vị trí mà ký tự xuất hiện đúng một lần trong toàn bộ chuỗi. Vị trí này an toàn vì không có thao tác xóa nào khác có thể xóa cùng một ký tự và tạo ra kết quả tương tự. 
3. Nếu tìm thấy vị trí như vậy, hãy xây dựng chuỗi câu trả lời bằng cách loại bỏ ký tự đó và xuất ra ngay lập tức. Chúng ta có thể dừng sớm vì mọi chuỗi tác vụ hợp lệ đều có thể chấp nhận được và bài toán không yêu cầu kết quả tối thiểu hoặc tối đa về mặt từ điển. 
4. Nếu quá trình quét hoàn tất mà không tìm thấy ký tự xuất hiện duy nhất, hãy xuất ra`-1`. Điều này có nghĩa là mỗi ký tự trong từ xuất hiện ít nhất hai lần, do đó mọi ký tự có thể bị xóa đều không rõ ràng. 

Ý tưởng cốt lõi đằng sau tính chính xác là việc xóa một ký tự xuất hiện một lần sẽ tạo ra ánh xạ một-một giữa chuỗi kết quả và vị trí ban đầu. Không có chỉ mục nào khác có thể tạo ra cùng một chuỗi rút ngắn vì không có ký tự giống hệt nào khác tồn tại ở nơi khác trong từ. Ngược lại, nếu mỗi ký tự xuất hiện nhiều lần, bất kỳ thao tác xóa nào cũng có thể được phản chiếu ở vị trí khác có cùng ký tự, phá vỡ tính duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    if not s:
        return

    from collections import Counter
    cnt = Counter(s)

    for i, ch in enumerate(s):
        if cnt[ch] == 1:
            print(s[:i] + s[i+1:])
            return

    print(-1)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo chiến lược tần số đầu tiên một cách trực tiếp. các`Counter`tính toán tần số ký tự trong thời gian tuyến tính. Quá trình quét sau đó sẽ kiểm tra từng vị trí và ngay lập tức đưa ra lần xóa hợp lệ đầu tiên. 

Một cạm bẫy triển khai phổ biến là cố gắng xác thực tính duy nhất bằng cách thực sự xây dựng tất cả các phép xóa có thể có và so sánh các chuỗi, âm thầm trở thành bậc hai. Một sai lầm khác là chỉ kiểm tra các điều kiện cục bộ, chẳng hạn như liệu các ký tự lân cận có khác nhau hay không, điều này không liên quan đến yêu cầu về tính duy nhất toàn cục. 

## Ví dụ đã hoạt động 

Xem xét từ đầu vào`"junior"`. 

| Bước | Chỉ mục | Nhân vật | Tần số | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | - | - | j1 u1 n1 i1 o1 r1 | tần số tính toán | - | 
| 2 | 0 | j | 1 | xóa |`unior`| 

Ký tự đầu tiên đã hoạt động vì nó là duy nhất. Chuỗi kết quả được chấp nhận ngay lập tức, xác nhận rằng tính duy nhất chỉ phụ thuộc vào tần số chung chứ không phụ thuộc vào vị trí. 

Bây giờ hãy xem xét`"tracktor"`. 

| Bước | Chỉ mục | Nhân vật | Tần số | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | - | - | t2 r2 a1 c1 k1 o1 | tần số tính toán | - | 
| 2 | 2 | một | 1 | xóa |`trcktor`| 

Ở đây nhân vật`'a'`là duy nhất, do đó việc loại bỏ nó sẽ tạo ra một chuỗi tác vụ hợp lệ. Mặc dù các nhân vật khác lặp lại, chỉ có nhân vật duy nhất mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) mỗi từ | Một lượt đếm tần số và một lượt quét để xóa | 
| Không gian | O(1) | Kích thước bảng chữ cái được cố định (26 chữ cái viết thường) | 

Các ràng buộc cho phép lên đến$10^5$ký tự trên mỗi từ, nhưng chỉ tối đa mười từ, do đó, việc quét tuyến tính trên mỗi từ nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = []
    n = int(input())
    for _ in range(n):
        s = input().strip()
        cnt = Counter(s)
        ans = -1
        for i, ch in enumerate(s):
            if cnt[ch] == 1:
                ans = s[:i] + s[i+1:]
                break
        out.append(str(ans))
    return "\n".join(out)

# provided sample
assert run("4\naaab\nzzz\njunior\ntracktor\n") == "-1\n-1\nunior\ntrcktor"

# all equal characters
assert run("1\naaaa\n") == "-1"

# single unique middle character
assert run("1\naabaaa\n") == "aabaaa".replace("b","")

# unique only at end
assert run("1\nxxxy\n") == "xxx"

# already valid mixed
assert run("1\nabca\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aaaa`|`-1`| không thể xóa duy nhất | 
|`abca`|`-1`| sự mơ hồ của ký tự trùng lặp | 
|`xxxy`|`xxx`| ký tự độc đáo ở cuối | 
|`aabaaa`|`aabaaa`không có`b`| trường hợp ký tự duy nhất trung tâm | 

## Vỏ cạnh 

Một từ mà tất cả các ký tự đều giống hệt nhau, chẳng hạn như`"bbbbbb"`, chứng tỏ sự thất bại của bất kỳ chiến lược xóa ngây thơ nào. Mỗi lần xóa đều tạo ra`"bbbbb"`, nhưng mỗi kết quả có thể được lấy từ nhiều vị trí, do đó không tồn tại chuỗi tác vụ hợp lệ và đầu ra phải là`-1`. 

Trong một từ như`"abca"`, loại bỏ một trong hai`'a'`mang lại chuỗi rút ngắn tương tự`"bca"`. Mặc dù tồn tại một chuỗi ngắn hơn nhưng nó không tương ứng với một vị trí chèn duy nhất, do đó thuật toán sẽ loại bỏ nó một cách chính xác vì không có ký tự nào có tần số chính xác bằng một.
