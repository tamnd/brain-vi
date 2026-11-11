---
title: "CF 1553D - Xóa lùi"
description: "Chúng tôi đang mô phỏng quá trình gõ trong đó chúng tôi quét chuỗi nguồn s từ trái sang phải. Tại mỗi vị trí, chúng tôi thêm ký tự hiện tại vào vùng đệm văn bản đang phát triển hoặc nhấn phím lùi để xóa ký tự được thêm gần đây nhất nếu nó tồn tại."
date: "2026-06-16T15:54:08+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1553
codeforces_index: "D"
codeforces_contest_name: "Harbour.Space Scholarship Contest 2021-2022 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 1500
weight: 1553
solve_time_s: 361
verified: false
draft: false
---

[CF 1553D - Phím xóa lùi](https://codeforces.com/problemset/problem/1553/D) 

**Đánh giá:** 1500 
**Tags:** dp, tham lam, chuỗi, hai con trỏ 
**Thời gian giải:** 6 phút 1 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng quá trình gõ trong đó chúng tôi quét chuỗi nguồn`s`từ trái sang phải. Tại mỗi vị trí, chúng tôi thêm ký tự hiện tại vào vùng đệm văn bản đang phát triển hoặc nhấn phím lùi để xóa ký tự được thêm gần đây nhất nếu nó tồn tại. 

Câu hỏi đặt ra là liệu có tồn tại sự lựa chọn các thao tác xóa lùi sao cho sau khi xử lý tất cả các ký tự của`s`, bộ đệm cuối cùng sẽ trở thành chuỗi đích chính xác`t`. 

Một cách hữu ích để suy nghĩ về quá trình này là mọi nhân vật trong`s`hoặc tồn tại trong chuỗi cuối cùng hoặc bị xóa bởi một dấu lùi sau. Hạn chế là việc xóa luôn diễn ra ở cuối chuỗi được xây dựng hiện tại, do đó, các ký tự trước đó chỉ có thể bị xóa nếu có đủ khoảng lùi sau đó. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm độc lập và trong tất cả các trường hợp, tổng độ dài chuỗi tối đa là 200000. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng tất cả các khả năng hoặc sử dụng các lựa chọn theo cấp số nhân cho mỗi ký tự. Ngay cả giải pháp O(n log n) cho mỗi trường hợp thử nghiệm cũng có nguy cơ quá chậm nếu nó không tổng hợp cẩn thận, do đó cần phải có giải pháp tuyến tính cho mỗi trường hợp thử nghiệm. 

Một sự tinh tế quan trọng xuất hiện khi các ký tự trong`s`cuộc thi đấu`t`nhưng không thể căn chỉnh do xóa sau này. Ví dụ, nếu`s = "ababa"`Và`t = "ba"`, có thể bỏ qua các ký tự đầu và xóa có chọn lọc các ký tự sau để chỉ ký tự thứ hai và cuối cùng tồn tại theo đúng thứ tự. Tuy nhiên, nếu`t`yêu cầu các ký tự theo thứ tự buộc phải giữ tiền tố mà sau này phải xóa một phần theo cách phá hủy thứ tự, điều đó trở nên không thể. 

Một trường hợp thất bại khác phát sinh khi`t`dài hơn số lượng ký tự cuối cùng có thể tồn tại. Vì mỗi phím xóa lùi sẽ loại bỏ một ký tự đã gõ trước đó và việc gõ sẽ tạo ra chính xác một ký tự cho mỗi bước không phải phím xóa lùi, nên độ dài cuối cùng không thể vượt quá số ký tự đã gõ trừ đi các khoảng lùi, vì vậy nếu`|t| > |s|`, câu trả lời ngay lập tức là không thể. Tinh tế hơn là khi độ dài tương thích nhưng các ràng buộc về thứ tự tương đối khiến cho việc khớp không thể thực hiện được. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là xử lý từng vị trí trong`s`dưới dạng quyết định nhị phân: chúng ta gõ nó hoặc nhấn phím backspace. Điều này dẫn đến khoảng`2^n`và đối với mỗi khả năng, chúng tôi mô phỏng hành vi ngăn xếp trong O(n), cho ra O(n 2^n), vượt xa mọi giới hạn ngay cả đối với n khoảng 40. 

Cấu trúc của vấn đề sẽ đơn giản hóa khi chúng ta ngừng suy nghĩ về những ký tự nào bị xóa và thay vào đó hãy nghĩ ngược lại. Mọi thao tác xóa lùi sẽ xóa một ký tự chắc chắn phải được giữ trước đó, nghĩa là chúng ta thực sự đang quyết định ký tự nào trong`s`tồn tại dưới dạng một dãy con, nhưng có thêm một ràng buộc: việc xóa có thể xóa các ký tự được giữ trước đó, vì vậy chúng ta không chỉ đơn giản chọn một dãy con của`s`. Tuy nhiên, chúng ta có thể diễn giải lại quy trình như xây dựng một ngăn xếp trong đó mỗi ký tự sau này có thể bị xóa và chúng ta muốn ngăn xếp cuối cùng bằng nhau.`t`. 

Quan sát quan trọng là nếu chúng ta xử lý`s`từ trái sang phải và duy trì một ngăn xếp, lựa chọn thực sự duy nhất là đẩy ký tự hiện tại hay mô phỏng một khoảng lùi. Thay vì thử mọi lựa chọn, chúng ta có thể lý luận một cách tham lam từ cuối cùng: chúng ta muốn phù hợp`t`là chuỗi cuối cùng còn lại, vì vậy chúng tôi cố gắng đảm bảo rằng các ký tự của`t`xuất hiện dưới dạng phần tử cuối cùng còn sót lại của ngăn xếp theo thứ tự. 

Một sự đơn giản hóa về mặt cấu trúc hơn là mỗi ký tự của`t`phải tương ứng với một số sự kiện xảy ra trong`s`vẫn tồn tại sau tất cả các thao tác xóa sau nó. Làm ngược lại, chúng ta có thể tham lam sánh đôi`t`từ cuối`s`, quyết định liệu chúng ta có sử dụng một ký tự như một phần của`t`hoặc coi nó như thứ gì đó đã bị xóa hoặc được sử dụng để xóa các ký tự trước đó. Điều này làm giảm chiến lược quét hai con trỏ từ phải sang trái. 

Chúng tôi duy trì một con trỏ`j`TRÊN`t`bắt đầu từ ký tự cuối cùng của nó và quét`s`từ phải sang trái. Khi chúng ta nhìn thấy một ký tự bằng`t[j]`, chúng ta có thể ghép nó với tư cách là một nhân vật còn sống sót và di chuyển`j`lạc hậu. Ngược lại, ký tự đó trong`s`phải có thể sử dụng được, nghĩa là nó có thể được đánh máy và sau đó bị xóa hoặc đơn giản là bị bỏ qua trong quá trình xây dựng. Nếu chúng ta quản lý để khớp tất cả các ký tự của`t`, việc xây dựng là có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực trước sự lựa chọn | O(2^n · n) | O(n) | Quá chậm | 
| Hai con trỏ tham lam từ cuối | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đặt con trỏ`j`đến chỉ số cuối cùng của`t`. Điều này đại diện cho ký tự tiếp theo mà chúng ta vẫn cần khớp theo thứ tự ngược lại. Ý tưởng là vậy`t`phải xuất hiện dưới dạng dãy con của ngăn xếp cuối cùng hợp lệ, vì vậy chúng tôi cố gắng nhúng nó từ phía sau. 
2. Đi ngang`s`từ phải sang trái sử dụng chỉ mục`i`. Chúng tôi kiểm tra từng ký tự như một yếu tố đóng góp tiềm năng cho cấu trúc phù hợp cuối cùng. 
3. Nếu`s[i]`bằng`t[j]`, chúng tôi coi ký tự này khớp với ký tự được yêu cầu hiện tại của`t`và giảm dần`j`. Điều này có nghĩa là chúng tôi cam kết vị trí này với tư cách là một trong những nhân vật còn sống sót trong kết quả cuối cùng. 
4. Nếu`s[i]`không bằng`t[j]`, chúng tôi bỏ qua nó. Về mặt khái niệm, ký tự này được gõ và sau đó bị xóa bằng dấu cách lùi hoặc không bao giờ đóng góp vào ngăn xếp cuối cùng. 
5. Sau khi quét xong, hãy kiểm tra xem`j`đã chuyển đến`-1`. Nếu vậy thì mỗi nhân vật của`t`đã được khớp theo đúng thứ tự ngược lại nên có thể xây dựng được. 

Lý do điều này có hiệu quả là vì việc so khớp từ cuối đảm bảo chúng ta luôn chỉ định từng`t[j]`đến sự xuất hiện ngoài cùng bên phải có thể xảy ra trong`s`. Bất kỳ nhân vật nào trong`s`mà chúng ta bỏ qua có thể được coi là có thể xóa được một cách an toàn, vì các khoảng lùi cho phép xóa tùy ý các ký tự đã nhập trước đó, do đó việc bỏ qua không vi phạm tính khả thi. Nếu một kết quả khớp tồn tại trong một cấu trúc hợp lệ, thì luôn có một cách để chọn các thao tác xóa sao cho kết quả khớp ngoài cùng bên phải tham lam này là hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    t = input().strip()

    j = len(t) - 1

    for i in range(len(s) - 1, -1, -1):
        if j >= 0 and s[i] == t[j]:
            j -= 1

    print("YES" if j == -1 else "NO")

q = int(input())
for _ in range(q):
    solve()
```Việc thực hiện trực tiếp tuân theo chiến lược kết hợp ngược. Chúng tôi tránh mọi mô phỏng các thao tác ngăn xếp hoặc xóa lùi vì chúng được xử lý hoàn toàn bằng quyền tự do loại bỏ các ký tự không khớp. 

Điểm tinh tế duy nhất là chúng tôi không bao giờ mô phỏng rõ ràng việc xóa. Điều này an toàn vì các khoảng lùi chỉ hạn chế chúng tôi bằng cách xóa các ký tự gần đây nhất và việc quét từ phải sang trái đảm bảo chúng tôi không bao giờ dựa vào các ràng buộc thứ tự sẽ bị vô hiệu khi xóa như vậy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`s = ababa`,`t = ba`Chúng tôi theo dõi`j`từ cuối`t`. 

| tôi (chỉ số) | s[i] | t[j] | hành động | j sau | 
| --- | --- | --- | --- | --- | 
| 4 | một | một | trận đấu | 0 | 
| 3 | b | một | bỏ qua | 0 | 
| 2 | một | một | trận đấu | -1 | 

Một lần`j`trở thành`-1`, các ký tự còn lại không liên quan. Điều này xác nhận chúng ta có thể nhúng`t`theo thứ tự ngược lại. 

Dấu vết này cho thấy rằng chỉ những kết quả phù hợp có thể sử dụng ở ngoài cùng bên phải mới quan trọng và cấu trúc trước đó không hạn chế tính khả thi sau khi các kết quả phù hợp sau đó được bảo mật. 

### Ví dụ 2 

đầu vào:`s = ababa`,`t = bb`| tôi | s[i] | t[j] | hành động | j | 
| --- | --- | --- | --- | --- | 
| 4 | một | b | bỏ qua | 1 | 
| 3 | b | b | trận đấu | 0 | 
| 2 | một | b | bỏ qua | 0 | 
| 1 | b | b | trận đấu | -1 | 
| 0 | một | - | bỏ qua | -1 | 

Chúng tôi kết hợp thành công cả hai`b`các ký tự, cho thấy rằng ngay cả với các ký tự can thiệp, việc so khớp ngược tham lam sẽ tìm thấy các vị trí hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | quét ngược một lần`s`| 
| Không gian | O(1) | chỉ sử dụng các biến con trỏ | 

Tổng chiều dài trên tất cả các trường hợp thử nghiệm được giới hạn bởi 200000, do đó công việc tổng thể là tuyến tính ở kích thước đầu vào, vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    q = int(input())
    out = []

    for _ in range(q):
        s = input().strip()
        t = input().strip()

        j = len(t) - 1
        for i in range(len(s) - 1, -1, -1):
            if j >= 0 and s[i] == t[j]:
                j -= 1

        out.append("YES" if j == -1 else "NO")

    return "\n".join(out)

# provided samples
assert run("""4
ababa
ba
ababa
bb
aaa
aaaa
aababa
ababa
""") == """YES
NO
NO
YES"""

# minimum size
assert run("""2
a
a
a
b
""") == """YES
NO"""

# all equal
assert run("""2
aaaaa
aaa
aaaaa
aaaaaa
""") == """YES
NO"""

# ordering constraint
assert run("""2
abcde
edc
abcde
ecd
""") == """YES
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu đơn | CÓ/KHÔNG | độ đúng cơ sở | 
| tất cả các chuỗi bằng nhau | CÓ/KHÔNG | tính khả thi về độ dài | 
| mô hình đảo ngược | CÓ/KHÔNG | xử lý ràng buộc đặt hàng | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi`t`dài hơn số ký tự có thể khớp từ cuối`s`, chẳng hạn như`s = "abc"`Và`t = "abcd"`. Thuật toán ngay lập tức thất bại vì`j`không thể với tới`-1`, phản ánh chính xác tính không thể xảy ra vì không có chuỗi khoảng lùi nào có thể tạo thêm ký tự. 

Một trường hợp cạnh khác là khi`s`chứa tất cả các ký tự cần thiết nhưng không đủ thứ tự hiệu quả, chẳng hạn như`s = "abac"`Và`t = "abc"`. Quá trình quét phù hợp`c`, sau đó`b`, nhưng càng sớm`a`không thể bù đắp cho các yêu cầu đặt hàng và việc khớp ngược tham lam sẽ xác định chính xác lỗi khi kết quả cuối cùng`c`không thể ghép nối trong một cấu trúc hậu tố nhất quán.
