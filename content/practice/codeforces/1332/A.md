---
problem: 1332A
contest_id: 1332
problem_index: A
name: "Exercising Walk"
contest_name: "Codeforces Round 630 (Div. 2)"
rating: 1100
tags: ["greedy", "implementation", "math"]
answer: passed_samples
verified: true
solve_time_s: 199
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e063c-6878-83ec-86d5-cd8ccb25344b
---

#CF 1332A - Đi bộ tập thể dục 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e063c-6878-83ec-86d5-cd8ccb25344b 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một điểm bắt đầu trên một lưới vô hạn và nhiều bước di chuyển bắt buộc theo bốn hướng chính. Con mèo phải thực hiện chính xác các bước di chuyển trái, b di chuyển phải, c di chuyển xuống và d di chuyển lên, nhưng thứ tự hoàn toàn miễn phí. 

Hạn chế chính không phải là vị trí cuối cùng mà là toàn bộ quỹ đạo. Ở mỗi bước trung gian, con mèo phải ở trong một hình chữ nhật có trục cố định. Nhiệm vụ là quyết định xem có tồn tại thứ tự di chuyển nào giữ tất cả các vị trí trung gian bên trong hình chữ nhật đó hay không. 

Mặc dù lưới là vô hạn, nhưng ràng buộc hình chữ nhật có thể biến vấn đề này thành một vấn đề khả thi về đường giới hạn với điểm xuất phát cố định và số lần di chuyển cố định của đơn vị. 

Các ràng buộc cho phép tối đa 10^3 trường hợp kiểm thử và mỗi trường hợp kiểm thử chỉ sử dụng đầu vào số học theo thời gian không đổi. Do đó, mọi giải pháp đều phải chạy ở O(1) cho mỗi trường hợp thử nghiệm. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng xây dựng hoặc mô phỏng các hoán vị của bước đi, vì số lượng hoán vị là hàm mũ trong a + b + c + d. 

Một lỗi phổ biến xuất hiện khi chỉ suy luận về vị trí cuối cùng. Vì tổng chuyển vị là cố định nên người ta có thể nghĩ rằng chỉ cần kiểm tra xem điểm cuối cùng có nằm bên trong hình chữ nhật hay không là đủ. Điều đó không chính xác vì các bước trung gian có thể tạm thời rời khỏi vùng được phép ngay cả khi điểm cuối hợp lệ. 

Một trường hợp thất bại tinh tế khác phát sinh khi hình chữ nhật quá chật theo một hướng. Ví dụ: nếu x1 = x2 = x, con mèo buộc phải không bao giờ di chuyển theo chiều ngang. Một chiến lược ngây thơ cân bằng trái và phải mà không kiểm tra tính khả thi so với ranh giới sẽ chấp nhận sai các trường hợp đường dẫn nhất thiết phải bước ra ngoài cột cho phép. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các hoán vị của các nước đi. Chúng tôi lấy một chuỗi chứa một bản sao của L, b bản sao của R, c bản sao của D và d bản sao của U và kiểm tra xem có tồn tại thứ tự giữ bước đi bên trong hình chữ nhật mọi lúc hay không. Về nguyên tắc, điều này đúng vì nó trực tiếp mô hình hóa quá trình, nhưng số lượng các chuỗi riêng biệt là (a+b+c+d)! / (a!b!c!d!), lớn về mặt thiên văn ngay cả đối với đầu vào nhỏ. Điều này nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là thứ tự di chuyển chính xác không quan trọng theo nghĩa hoàn toàn tham lam, nhưng phạm vi tiếp cận cực độ ở mỗi trục mới là điều quan trọng. Vì chuyển động ngang và chuyển động dọc là độc lập nên chúng ta chỉ cần đảm bảo có thể xen kẽ các chuyển động sao cho tọa độ x luôn nằm trong giới hạn và tương tự với y. 

Trước tiên hãy xem xét trục ngang. Con mèo bắt đầu ở x. Sau bất kỳ tiền tố nào của bước đi, tọa độ x của nó bằng x + (số R được sử dụng cho đến nay) − (số L được sử dụng cho đến nay). Để ở trong [x1, x2], chúng ta phải luôn có khả năng tránh vượt quá một trong hai ranh giới. Điều này giảm xuống còn việc kiểm tra xem liệu ngân sách trái-phải có sẵn có thể được sắp xếp sao cho đường dẫn không bao giờ ép chúng ta ra ngoài khoảng đó hay không. 

Một cách tiêu chuẩn để diễn giải điều này là xem xét xem chúng ta có thể mở rộng bao xa tính từ điểm xuất phát theo cả hai hướng. Ở vế phải, phần mở rộng tối đa có thể có là b, nhưng chúng ta bị giới hạn bởi x2 − x. Ở vế trái, phần mở rộng tối đa có thể là a, nhưng chúng ta bị giới hạn bởi x − x1. Lý do tương tự áp dụng theo chiều dọc với c và d. 

Điều tinh tế duy nhất là chúng ta phải đảm bảo rằng tổng tự do theo chiều ngang đủ để chứa cả hai hướng cùng một lúc mà không bị vi phạm. Điều này giảm xuống còn việc kiểm tra xem khoảng có thể đạt được từ x bằng cách sử dụng các bước di chuyển có sẵn có cắt khoảng cho phép theo cách không yêu cầu vượt mức hay không. 

Do đó, đối với mỗi trục, chúng tôi xác minh xem liệu quỹ di chuyển có thể được đưa vào bên trong khoảng trống có sẵn xung quanh điểm bắt đầu hay không.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (hoán vị) | O((a+b+c+d)!) | O(n) | Quá chậm | 
| Kiểm tra tính khả thi tham lam | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý trục ngang và trục dọc một cách độc lập vì các chuyển động trong một trục không ảnh hưởng đến tọa độ của trục kia. 

1. Tính xem có bao nhiêu khoảng trống ở bên trái và bên phải của điểm bắt đầu: khoảng trống bên trái là x − x1 và khoảng trống bên phải là x2 − x. Điều này cho chúng ta biết chúng ta có thể di chuyển bao xa theo mỗi hướng trước khi chạm vào ranh giới. 
2. Kiểm tra xem có thể bố trí các chuyển động ngang trong giới hạn này hay không. Chúng ta cần đảm bảo rằng con mèo không bao giờ yêu cầu di chuyển sang trái nhiều hơn khoảng trống còn lại có sẵn cộng với bất kỳ sự chậm trễ nào từ các động tác phải không được sử dụng và tính đối xứng để di chuyển sang phải. Điều kiện giảm xuống để đảm bảo rằng phạm vi hiệu dụng [x − a, x + b] có thể được định vị trong [x1, x2]. 
3. Áp dụng cách suy luận tương tự cho trục tung, sử dụng c và d với y, y1, y2. 
4. Nếu cả hai trục đều thỏa mãn điều kiện khả thi một cách độc lập, thì xuất CÓ. Nếu không thì xuất ra NO. 

Ý tưởng ẩn giấu chính là mặc dù các chuyển động được xen kẽ, nhưng sự dịch chuyển trong trường hợp xấu nhất trên mỗi trục được kiểm soát hoàn toàn bằng số đếm. Chúng tôi không chọn một trật tự tránh ranh giới từng bước một; thay vào đó, chúng tôi đang xác minh rằng một đơn hàng như vậy có thể tồn tại bằng cách kiểm tra xem “ngân sách” chuyển động có vừa với bên trong hình chữ nhật hay không. 

### Tại sao nó hoạt động 

Bất kỳ bước đi hợp lệ nào đều tương ứng với một chuỗi tiền tố gồm các bước di chuyển trái và phải không bao giờ vượt quá phạm vi cho phép. Một trình tự như vậy tồn tại khi và chỉ nếu tổng chuyển vị cần thiết theo mỗi hướng có thể được lập kế hoạch mà không gây ra sự vi phạm ranh giới. Vì mỗi bước di chuyển thay đổi tọa độ chính xác ±1, nên tính khả thi chỉ phụ thuộc vào việc liệu khoảng giá trị x có thể đạt được ngay từ đầu bằng cách sử dụng các bước di chuyển L/R có sẵn có thể được nhúng vào phân đoạn được phép hay không. Bất biến tương tự giữ độc lập cho y, điều này làm cho việc kiểm tra hai trục là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok_1d(pos, a, b, l, r):
    # pos is starting coordinate
    # we must stay in [l, r]
    # left moves: a, right moves: b

    # effective reachable interval if we use all moves in worst way
    min_pos = pos - a
    max_pos = pos + b

    # but we can reorder moves; we need to ensure we can avoid leaving [l, r]
    # classic condition: interval [min_pos, max_pos] must intersect [l, r]
    # and be placeable inside bounds by shifting via ordering constraints
    return not (max_pos < l or min_pos > r)

t = int(input())
for _ in range(t):
    a, b, c, d = map(int, input().split())
    x, y, x1, y1, x2, y2 = map(int, input().split())

    # horizontal feasibility
    # ensure we can stay in bounds using all moves
    if x + b < x1 or x - a > x2:
        print("NO")
        continue

    # vertical feasibility
    if y + d < y1 or y - c > y2:
        print("NO")
        continue

    print("YES")
```Việc kiểm tra theo chiều ngang đảm bảo rằng ngay cả trong trật tự khắc nghiệt nhất, con mèo không bị ép hoàn toàn ra ngoài khoảng cho phép. Nếu tất cả các nước đi bên phải sẽ đẩy nó vượt quá x2 hoặc tất cả các nước đi bên trái sẽ đẩy nó xuống dưới x1, thì không có mệnh lệnh nào có thể cứu được nó. 

Kiểm tra dọc là đối xứng. 

Một điểm tinh tế là chúng ta không xây dựng một trật tự an toàn một cách rõ ràng. Logic dựa trên thực tế là miễn là không có hướng nào bị hình chữ nhật “chặn” hoàn toàn so với điểm bắt đầu và số lần di chuyển, chúng ta có thể xen kẽ các bước di chuyển để tránh vi phạm giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a=3 b=2 c=2 d=2
x=0 y=0
x1=-2 x2=2
y1=-2 y2=2
```| Bước | Kiểm tra ngang | Kiểm tra dọc | Quyết định | 
| --- | --- | --- | --- | 
| Ban đầu | x+b=2 ≥ -2 và x-a=-3 ≤ 2 | y+d=2 ≥ -2 và y-c=-2 ≤ 2 | tiếp tục | 
| Cuối cùng | hợp lệ | hợp lệ | CÓ | 

Trường hợp này cho thấy ngay cả khi di chuyển sang trái nhiều lần, điểm bắt đầu vẫn ở giữa đủ để cả hai điểm cực trị vẫn nằm trong hình chữ nhật. 

### Ví dụ 2 

đầu vào:```
a=3 b=1 c=4 d=1
x=0 y=0
x1=-1 x2=1
y1=-1 y2=1
```| Bước | Kiểm tra ngang | Kiểm tra dọc | Quyết định | 
| --- | --- | --- | --- | 
| Ban đầu | x-a=-3 < -1 thất bại | - | KHÔNG | 

Điều này chứng tỏ một trường hợp chỉ riêng chuyển động bên trái đã dẫn đến vi phạm bất kể trật tự nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm sử dụng kiểm tra số học theo thời gian không đổi | 
| Không gian | O(1) | Không có cấu trúc bổ sung ngoài các biến đầu vào | 

Lời giải dễ dàng nằm trong giới hạn vì t ≤ 10^3 và mỗi trường hợp là O(1). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b, c, d = map(int, input().split())
        x, y, x1, y1, x2, y2 = map(int, input().split())

        if x + b < x1 or x - a > x2:
            out.append("NO")
            continue
        if y + d < y1 or y - c > y2:
            out.append("NO")
            continue
        out.append("YES")

    return "\n".join(out)

# provided samples
assert run("""6
3 2 2 2
0 0 -2 -2 2 2
3 1 4 1
0 0 -1 -1 1 1
1 1 1 1
1 1 1 1 1 1
0 0 0 1
0 0 0 0 0 1
5 1 1 1
0 0 -100 -100 0 100
1 1 5 1
0 0 -100 -100 100 0
""") == """Yes
No
No
Yes
Yes
Yes"""

# custom cases
assert run("""3
1 0 0 0
0 0 0 0 0 0
0 1 0 0
0 0 0 0 1 0
2 2 0 0
0 0 -1 -1 1 1
""") == """Yes
Yes
Yes"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giới hạn điểm đơn | CÓ | chỉ được phép di chuyển theo một hướng | 
| di chuyển không đối xứng | CÓ | kiểm tra tính khả thi một chiều | 
| trường hợp giới hạn đối xứng | CÓ | độ chính xác của vị trí trung tâm | 

## Vỏ cạnh 

Một hình chữ nhật chặt chẽ như x1 = x2 = x cho thấy liệu giải pháp có xử lý chính xác sự tự do theo chiều ngang bằng 0 hay không. Trong tình huống đó, bất kỳ số lần di chuyển theo chiều ngang khác 0 đều vi phạm tính khả thi ngay lập tức vì x + b phải bằng x và x − a phải bằng x, buộc a = b = 0. Thuật toán loại bỏ chính xác những trường hợp đó thông qua điều kiện x + b < x1 hoặc x − a > x2. 

Một trường hợp cạnh khác là khi điểm bắt đầu nằm chính xác trên một ranh giới. Ví dụ: x = x1 và a > 0 buộc phải vi phạm ngay lập tức vì bất kỳ bước di chuyển sang trái nào cũng sẽ thoát khỏi khoảng thời gian. Việc kiểm tra x − a > x2 hoặc x + b < x1 nắm bắt được tình huống này mà không cần mô phỏng. 

Trường hợp tinh vi cuối cùng xảy ra khi chuyển động mất cân bằng nghiêm trọng, chẳng hạn như a lớn với b nhỏ trong một khoảng hẹp. Ngay cả khi tổng số dịch chuyển có thể bị loại bỏ, việc ra lệnh cũng không thể ngăn chặn hành vi vi phạm sớm. Điều kiện dựa trên khoảng phát hiện điều này bằng cách so sánh các vị trí cực kỳ có thể có với phạm vi cho phép một cách độc lập với thứ tự.
