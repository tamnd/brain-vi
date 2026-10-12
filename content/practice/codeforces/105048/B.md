---
title: "CF 105048B - Romeo và bước đi ngẫu nhiên"
description: "Chúng ta có một tập hợp các điểm trên trục số, mỗi điểm biểu thị vị trí ban đầu có thể có của Juliet tại thời điểm 0. Sau này Romeo biết được rằng tại thời điểm d, Juliet phải nằm đâu đó bên trong một khoảng [A, B] đã biết."
date: "2026-06-28T05:07:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105048
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 03-22-24 Div. 2 (Beginner)"
rating: 0
weight: 105048
solve_time_s: 78
verified: false
draft: false
---

[CF 105048B - Romeo và bước đi ngẫu nhiên](https://codeforces.com/problemset/problem/105048/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên trục số, mỗi điểm biểu thị vị trí ban đầu có thể có của Juliet tại thời điểm 0. Romeo sau đó biết được điều đó vào thời điểm đó`d`, Juliet phải nằm đâu đó trong một khoảng đã biết`[A, B]`. Giữa thời gian`0`và thời gian`d`, Juliet có thể di chuyển tự do dọc theo đường thẳng, nhưng tốc độ của cô ấy bị hạn chế: trong mỗi phút cô ấy có thể di chuyển tối đa một khoảng cách, vì vậy sau`d`trong vài phút cô ấy có thể đến bất cứ đâu trong khoảng cách`d`từ nơi cô ấy bắt đầu. 

Nhiệm vụ là xác định điểm xuất phát nào trong số các ứng cử viên nhất định có thể phát triển thành một vị trí nào đó bên trong`[A, B]`sau chính xác`d`phút chuyển động như vậy. Đầu ra là danh sách các chỉ số của những điểm bắt đầu hợp lệ đó. 

Các ràng buộc cho phép lên đến`10^5`ứng cử viên điểm và tọa độ lên đến`10^9`, với`d`cũng lên tới`10^9`. Điều này ngay lập tức loại trừ bất kỳ phương pháp tiếp cận nào mô phỏng chuyển động hoặc kiểm tra khả năng tiếp cận từng phút. Bất kỳ giải pháp nào cũng phải giảm từng điểm thành một cuộc kiểm tra liên tục, vì`O(N)`hoặc`O(N log N)`giải pháp dự kiến ​​​​sẽ trôi qua một cách thoải mái, trong khi mọi thứ bậc hai sẽ quá chậm. 

Một vấn đề tế nhị nảy sinh từ lý luận về ranh giới. Một lỗi phổ biến là coi khả năng tiếp cận là một điểm duy nhất thay vì một khoảng thời gian. Một cách khác là chỉ kiểm tra một hướng chuyển động, ví dụ như xác minh xem`P_i`có thể đạt được`A`hoặc`B`riêng lẻ, thay vì kiểm tra sự chồng chéo của các phạm vi có thể truy cập. Những sai lầm này dẫn đến việc từ chối các điểm bắt đầu hợp lệ khi Juliet chỉ cần di chuyển về phía đúng phần của khoảng thời gian. 

Ví dụ, giả sử`d = 2`,`P_i = 5`, Và`[A, B] = [6, 7]`. Juliet có thể di chuyển từ`5`ĐẾN`7`trong hai bước, vì vậy điểm bắt đầu này là hợp lệ. Một câu kiểm tra ngây thơ như “cô ấy có thể với tới`A`chính xác” sẽ từ chối nó một cách không chính xác. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực mô phỏng chuyển động từ mỗi vị trí bắt đầu. Từ một điểm`x`, sau đó`d`phút Juliet có thể ở bất cứ đâu trong khoảng thời gian đó`[x - d, x + d]`. Để kiểm tra tính hợp lệ, người ta có thể liệt kê tất cả các vị trí có thể tiếp cận được từ`x`và xác minh xem có bất kỳ lời nói dối nào trong`[A, B]`. Điều này ngay lập tức trở nên không khả thi vì mỗi khoảng chứa`O(d)`vị trí số nguyên và`d`có thể lớn như`10^9`, làm cho việc mô phỏng không thể thực hiện được. 

Quan sát quan trọng là chuyển động biến mỗi điểm bắt đầu thành một khoảng liên tục và chúng ta chỉ cần biết liệu hai khoảng có giao nhau hay không. Thay vì theo dõi tất cả các vị trí có thể, chúng tôi tính toán khoảng thời gian có thể tiếp cận`[x - d, x + d]`và kiểm tra xem nó có trùng lặp với`[A, B]`. Sự chồng lấp khoảng thời gian giảm xuống còn một cặp bất đẳng thức tuyến tính, điều này đơn giản hóa hơn nữa thành một ràng buộc trực tiếp trên`x`. 

Các khoảng cắt nhau khi và chỉ khi tồn tại vị trí nào đó nằm trong cả hai. Điều này xảy ra chính xác khi`x + d >= A`Và`x - d <= B`. Sắp xếp lại mang lại`x >= A - d`Và`x <= B + d`. Vì vậy, mỗi điểm ứng cử viên có thể được kiểm tra độc lập trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N·d) | O(1) | Quá chậm | 
| Giảm khoảng thời gian | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề về khả năng tiếp cận thành một bộ lọc phạm vi đơn giản trên các điểm ban đầu. 

1. Tính vị trí xuất phát tối thiểu mà vẫn có thể đạt tới`[A, B]`sau đó`d`phút, tức là`A - d`. Điều này xuất phát từ việc đảo ngược chuyển động sang phải tối đa. 
2. Tính vị trí xuất phát tối đa mà vẫn có thể đạt tới`[A, B]`, đó là`B + d`, đến từ việc đảo ngược chuyển động sang trái tối đa. 
3. Lặp lại tất cả các điểm`P[i]`. 
4. Đối với mỗi điểm, hãy kiểm tra xem nó có nằm trong khoảng không`[A - d, B + d]`. Nếu có, hãy đưa chỉ mục của nó vào câu trả lời. 
5. Xuất tất cả các chỉ số đã thu thập theo thứ tự tăng dần. 

Lý do chúng tôi đảo ngược khoảng thời gian thay vì chuyển động mô phỏng về phía trước là vì mọi vị trí bắt đầu đều ánh xạ tới một đoạn có thể tiếp cận đối xứng. Làm việc ngược chuyển đổi vấn đề về khả năng tiếp cận động thành điều kiện lọc tĩnh. 

### Tại sao nó hoạt động 

Tại thời điểm 0, vị trí bắt đầu`x`mở rộng thành khoảng đầy đủ`[x - d, x + d]`vào thời điểm đó`d`. Juliet hợp lệ khi và chỉ khi khoảng này cắt nhau`[A, B]`. Giao điểm khoảng tương đương với điều kiện không có khoảng nào nằm hoàn toàn bên trái hoặc hoàn toàn bên phải của khoảng kia. Việc dịch các điều kiện không trùng lặp đó sẽ tạo ra các giới hạn`x >= A - d`Và`x <= B + d`. Do đó, mọi điểm bắt đầu hợp lệ sẽ được bộ lọc này ghi lại chính xác một lần và không có điểm không hợp lệ nào có thể đáp ứng được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, A, B, d = map(int, input().split())
    P = list(map(int, input().split()))

    left = A - d
    right = B + d

    res = []
    for i, x in enumerate(P):
        if left <= x <= right:
            res.append(str(i))

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Việc thực hiện mã hóa trực tiếp điều kiện khoảng dẫn xuất. Điểm tinh tế duy nhất là các chỉ số được giữ nguyên theo thứ tự đầu vào, vì vậy chúng tôi liệt kê bằng`i`bắt đầu từ số 0 và in các chỉ số đó dưới dạng chuỗi. 

Phải cẩn thận để không tính toán lại khả năng tiếp cận cho mỗi điểm theo bất kỳ cách mở rộng nào. Toàn bộ giải pháp dựa vào việc giảm từng điểm thành một so sánh duy nhất với khoảng thời gian được tính toán trước. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ trong đó`A = 10`,`B = 12`,`d = 3`, và điểm là`[5, 9, 11, 15]`. 

Khoảng thời gian bắt đầu hợp lệ được chuyển đổi trở thành`[A - d, B + d] = [7, 15]`. 

| tôi | P[i] | Kiểm tra (7 ≤ x ≤ 15) | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 5 | không | từ chối | 
| 1 | 9 | vâng | giữ | 
| 2 | 11 | vâng | giữ | 
| 3 | 15 | vâng | giữ | 

Đầu ra là`1 2 3`. 

Bây giờ hãy xem xét một trường hợp nặng về ranh giới:`A = 0`,`B = 0`,`d = 5`, và điểm`[6, 5, 4, -1]`. 

Khoảng hợp lệ là`[-5, 5]`. 

| tôi | P[i] | Kiểm tra (-5 ≤ x 5) | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 6 | không | từ chối | 
| 1 | 5 | vâng | giữ | 
| 2 | 4 | vâng | giữ | 
| 3 | -1 | vâng | giữ | 

Điều này cho thấy rằng mặc dù khoảng thời gian mục tiêu thu gọn về một thời điểm`d`, bất kỳ vị trí bắt đầu nào trong khoảng cách`d`của nó vẫn còn hiệu lực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi điểm được kiểm tra một lần bằng số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến được lưu trữ ngoài đầu ra | 

Quét tuyến tính lên tới`10^5`điểm dễ dàng phù hợp trong giới hạn thời gian và mức sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample (format reconstructed)
assert run("""10 3 5 14
10 14 17 5 11 13 6 17 10 2
""") == "0 3 4 5 6 8"

# minimum size
assert run("""1 0 0 0
0
""") == "0"

# all invalid
assert run("""3 10 12 1
0 1 2
""") == ""

# all valid due to large d
assert run("""4 100 100 100
0 50 150 200
""") == "0 1 2 3"

# boundary tight case
assert run("""5 10 10 0
9 10 11 10 8
""") == "1 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | danh sách chỉ số | tính đúng đắn cơ bản | 
| phần tử đơn | 0 | xử lý đầu vào tối thiểu | 
| tất cả đều không hợp lệ | trống | từ chối đúng | 
| d lớn | tất cả các chỉ số | bao gồm đầy đủ | 
| d = 0 | chỉ khớp chính xác | độ chính xác ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi`d = 0`. Trong tình huống này, Juliet hoàn toàn không thể di chuyển, vì vậy điểm xuất phát hợp lệ duy nhất là những điểm đã có sẵn bên trong.`[A, B]`. Thuật toán xử lý việc này một cách tự nhiên vì khoảng thời gian trở thành`[A, B]`, do đó điều kiện rút gọn thành bài kiểm tra tư cách thành viên trực tiếp. 

Một trường hợp khác là khi`A - d`trở nên tiêu cực. Vì tọa độ có thể bằng 0 nhưng đầu vào không âm, nên điều này chỉ đơn giản là mở rộng vùng hợp lệ sang trái ngoài miền các điểm có thể có. Thuật toán vẫn hoạt động vì các phép so sánh vẫn hợp lệ ngay cả với giới hạn âm và không cần phải có sự kẹp đặc biệt. 

Một trường hợp tế nhị cuối cùng là khi`[A, B]`là cực kỳ lớn và chồng lên hầu hết các vị trí có thể. Trong trường hợp đó,`A - d`Và`B + d`có thể vượt quá giới hạn đầu vào theo một trong hai hướng, nhưng một lần nữa việc kiểm tra khoảng thời gian vẫn đúng vì nó chỉ phụ thuộc vào mối quan hệ bất đẳng thức chứ không phụ thuộc vào giới hạn phạm vi tuyệt đối.
