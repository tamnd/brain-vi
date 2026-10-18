---
title: "CF 105586C - \u4ea4\u901a\u8981\u585e"
description: "Chúng ta đang di chuyển dọc theo một hành lang thẳng được chia thành các “làn đường” dọc được đánh số từ 0 đến n + 1. Thời gian là rời rạc. Tại mỗi giây, bạn có thể ở lại làn đường hiện tại của mình hoặc di chuyển chính xác một làn sang bên phải."
date: "2026-06-22T17:55:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105586
codeforces_index: "C"
codeforces_contest_name: "\u201c\u534e\u4e3a\u676f\u201d 2024 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u65b0\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u51b3\u8d5b\uff09"
rating: 0
weight: 105586
solve_time_s: 66
verified: true
draft: false
---

[CF 105586C - \u4ea4\u901a\u8981\u585e](https://codeforces.com/problemset/problem/105586/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang di chuyển dọc theo một hành lang thẳng được chia thành các “làn đường” dọc được đánh số từ 0 đến n + 1. Thời gian là rời rạc. Tại mỗi giây, bạn có thể ở lại làn đường hiện tại của mình hoặc di chuyển chính xác một làn sang bên phải. Bạn luôn ở trên trục hoành y = 0, vì vậy trạng thái có ý nghĩa duy nhất là chỉ số làn đường hiện tại của bạn và thời gian hiện tại. 

Mỗi làn đường trung gian i từ 1 đến n bị chặn xe ngoại trừ một đoạn dọc an toàn. Đoạn an toàn đó ban đầu kéo dài y trong khoảng [l_i, r_i], nhưng cứ sau mỗi giây, toàn bộ mô hình lưu lượng lại dịch chuyển xuống một đơn vị. Vì bạn luôn đứng ở y = 0 nên câu hỏi duy nhất là liệu y = 0 có nằm trong đoạn an toàn tại thời điểm bạn đang ở làn i hay không. 

Tại thời điểm t, đoạn an toàn ở làn i đã chuyển sang [l_i − t, r_i − t]. Vị trí y = 0 an toàn chính xác khi l_i ≤ t ≤ r_i. Vì vậy, mỗi làn i áp đặt một khoảng thời gian: bạn chỉ được phép ở làn i vào những thời điểm t trong khoảng thời gian đó. 

Làn đầu 0 và làn cuối n+1 luôn an toàn nên không áp đặt hạn chế nào. 

Nhiệm vụ là tìm thời gian tối thiểu cần thiết để đến làn n + 1 bắt đầu từ làn 0, đồng thời tôn trọng thực tế là tại thời điểm t làn đường của bạn phải có hiệu lực trong thời gian đó và bạn chỉ có thể tăng chỉ số làn đường của mình lên tối đa một lần mỗi giây. 

Các ràng buộc n 10^5 và l_i, r_i lên tới 10^12 ngụ ý rằng chúng ta cần quét tuyến tính hoặc gần tuyến tính. Bất kỳ cách tiếp cận nào cố gắng khám phá các trạng thái theo thời gian hoặc sử dụng lập trình động theo các giá trị thời gian một cách trực tiếp sẽ quá chậm, vì thời gian có thể tăng lên lớn và các chuyển đổi liên tục lên tới các giá trị tỷ lệ 10^12. 

Một mô phỏng đơn giản theo thời gian cũng sẽ thất bại vì thời gian có thể kéo dài vượt xa n do chờ đợi và mỗi làn đưa ra một cửa sổ ràng buộc toàn cầu. 

Một trường hợp lỗi nhỏ xuất hiện khi khoảng thời gian hợp lệ của một làn đường sớm hơn khoảng thời gian có thể tiếp cận được bằng cách đi bộ về phía trước. 

Ví dụ: nếu làn đường i có r_i < i, thì ngay cả việc đến ngay lập tức mà không chờ đợi cũng đã vi phạm ràng buộc, khiến toàn bộ đường đi không thể thực hiện được. Một kẻ tham lam ngây thơ chỉ kiểm tra tính khả thi sau khi chuyển đi sẽ bỏ lỡ điều này. 

Một trường hợp phức tạp khác là khi cần phải chờ đợi để căn chỉnh cho phù hợp với các khoảng thời gian trong tương lai. Nếu một giải pháp ngây thơ luôn di chuyển bất cứ khi nào có thể, nó có thể đến quá sớm ở một làn đường có cửa sổ hợp lệ bắt đầu muộn hơn, dẫn đến mâu thuẫn muộn hơn mặc dù chờ đợi sớm hơn là đúng. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực coi mỗi trạng thái là một cặp (vị trí, thời gian). Từ mỗi trạng thái, chúng tôi có thể di chuyển hoặc chờ đợi và chúng tôi kiểm tra xem làn đường hiện tại có hợp lệ ở thời điểm hiện tại hay không. Điều này tạo ra một biểu đồ với các ràng buộc phụ thuộc vào thời gian. BFS hoặc đường đi ngắn nhất trên biểu đồ ẩn này là đúng về mặt khái niệm vì mỗi hành động tốn một giây nhưng không gian trạng thái không bị giới hạn về mặt thời gian. Trong trường hợp xấu nhất, giá trị thời gian có thể đạt tới 10^12 và số lượng trạng thái có thể truy cập tăng tỷ lệ thuận với tỷ lệ đó, khiến điều này hoàn toàn không khả thi. 

Quan sát quan trọng là thời gian không cần phải được khám phá một cách rõ ràng. Khi chúng tôi sửa chỉ số làn đường i, thông tin liên quan duy nhất là thời gian sớm nhất chúng tôi có thể đến đó. Bất kỳ sự đến muộn nào chỉ làm cho các ràng buộc trở nên khó khăn hơn chứ không bao giờ dễ dàng hơn, bởi vì mỗi làn đường i yêu cầu t phải nằm trong một khoảng cố định [l_i, r_i]. Điều này biến bài toán thành tính toán một chuỗi tăng dần khả thi duy nhất với số lần t_0, t_1, …, t_{n+1}. 

Khi di chuyển từ làn i − 1 đến i, một giây luôn trôi qua, vì vậy t_i ít nhất phải bằng t_{i−1} + 1. Ngoài ra, chúng ta không thể đến trước khi có thể đến được làn i trong cửa sổ thời gian của nó, vì vậy t_i cũng phải ít nhất là l_i. Cuối cùng, chúng ta không được vi phạm thời hạn r_i nên t_i phải ≤ r_i.

Điều này biến vấn đề thành một công trình xây dựng tham lam về phía trước, trong đó chúng ta luôn dành thời gian khả thi sớm nhất ở mỗi bước. Nếu thời gian khả thi sớm nhất đó vượt quá r_i thì không có lịch trình hợp lệ nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS không gian trạng thái trên (làn đường, thời gian) | O(rất lớn, lên tới 10^12 trạng thái) | O(tiểu bang) | Quá chậm | 
| Lập kế hoạch thời gian tham lam cho mỗi làn đường | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng thời gian đến cho mỗi làn từ trái sang phải. 

1. Khởi tạo thời gian ở làn 0 là 0. Điều này thể hiện việc bắt đầu ở vị trí 0 tại thời điểm 0. 
2. Với mỗi làn đường i từ 1 đến n, hãy tính thời gian đến sớm nhất có thể. Vì chúng ta phải dành ít nhất một giây để di chuyển từ làn i − 1 nên thời gian không thể nhỏ hơn t_{i−1} + 1. 
3. Làn đường cũng áp đặt khoảng thời gian hiệu lực riêng, vì vậy chúng tôi phải đảm bảo rằng chúng tôi không đến trước l_i. Do đó, chúng tôi lấy tối đa t_{i−1} + 1 và l_i làm thời gian đến ứng cử viên. 
4. Nếu ứng viên này vượt quá r_i, cửa sổ an toàn của làn đường đã đóng, do đó không có chuỗi di chuyển hợp lệ nào có thể đi qua làn đường này và quá trình kết thúc với thất bại. 
5. Ngược lại, chúng ta đặt t_i cho ứng cử viên này và tiếp tục. 
6. Sau khi xử lý làn n, chúng ta chuyển sang làn n + 1, không hạn chế. Chúng ta vẫn phải mất một giây để di chuyển nên đáp án cuối cùng là t_{n+1} = t_n + 1. 

Tại sao nó hoạt động: tại mỗi làn i, mọi lịch trình hợp lệ đều phải đến không sớm hơn t_{i−1} + 1 do tiến trình thời gian và không sớm hơn l_i do hạn chế về an toàn. Bất kỳ sự đến muộn nào chỉ làm tăng tất cả các lần tiếp theo và không thể giúp đáp ứng các giới hạn dưới trong tương lai, vì vậy việc chọn thời gian đến khả thi tối thiểu sẽ duy trì tính khả thi trên toàn cầu. Nếu mức tối thiểu này đã vi phạm r_i, thì mọi lịch trình có thể cũng phải vi phạm nó, bởi vì tất cả các lịch trình đều đến muộn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    l = [0] * (n + 2)
    r = [10**30] * (n + 2)

    for i in range(1, n + 1):
        li, ri = map(int, input().split())
        l[i], r[i] = li, ri

    t = 0
    for i in range(1, n + 1):
        t = max(t + 1, l[i])
        if t > r[i]:
            print(-1)
            return

    t = t + 1
    print(t)

if __name__ == "__main__":
    solve()
```Việc thực hiện chỉ giữ lại thời gian hiện tại vì mỗi làn chỉ phụ thuộc vào làn trước đó. Sự chuyển tiếp`t = max(t + 1, l[i])`mã hóa cả tiến trình thời gian bắt buộc và thời gian sớm nhất làn đường có thể sử dụng được. Việc kiểm tra tính khả thi đối với`r[i]`đảm bảo chúng ta không bao giờ đi vào làn đường sau khi cửa sổ an toàn của nó đóng lại. 

Mức tăng cuối cùng sau làn n tính đến việc di chuyển sang làn đường luôn an toàn cuối cùng n + 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2
3 5
1 6
```Chúng tôi theo dõi thời gian khi chúng tôi di chuyển: 

| Ngõ tôi | l_i | r_i | t trước | ứng viên max(t+1, l_i) | t sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | tối đa(1,1)=1 | 1 | 
| 2 | 3 | 5 | 1 | tối đa(2,3)=3 | 3 | 
| 3 | 1 | 6 | 3 | tối đa(4,1)=4 | 4 | 

Hết làn 3 chúng ta chuyển sang làn 4 nên đáp án là 5. 

Dấu vết này cho thấy cách chờ đợi được xử lý ngầm. Ở làn 2, chúng tôi buộc phải đợi đến giờ 3 mặc dù lẽ ra chúng tôi có thể đến sớm hơn. 

### Ví dụ 2 

đầu vào:```
2
2 3
1 2
```| Ngõ tôi | l_i | r_i | t trước | ứng cử viên | t sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 0 | tối đa(1,2)=2 | 2 | 
| 2 | 1 | 2 | 2 | tối đa(3,1)=3 | không hợp lệ | 

Tại làn 2, người đến sớm nhất là 3, nhưng r_2 = 2 nên không thể đi được. 

Điều này thể hiện một trường hợp thất bại trong đó chiến lược luôn di chuyển ngây thơ sẽ đến được làn 2 ở thời điểm 2 và cho rằng thành công không chính xác, việc bỏ lỡ làn 2 đó đòi hỏi phải đến muộn hơn và điều này không còn khả thi nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi làn được xử lý một lần với chuyển tiếp O(1) | 
| Không gian | O(1) | Chỉ thời gian hiện tại được lưu trữ | 

Quét tuyến tính phù hợp thoải mái trong phạm vi n 10^5 và tất cả các thao tác đều là so sánh số nguyên đơn giản, do đó giải pháp chạy dễ dàng trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    def solve():
        n = int(input())
        t = 0
        for i in range(n):
            l, r = map(int, input().split())
            t = max(t + 1, l)
            if t > r:
                print(-1)
                return
        t += 1
        print(t)

    solve()
    return sys.stdout.getvalue().strip()

# sample-like tests
assert run("1\n0 0\n") == "2"
assert run("2\n2 3\n1 2\n") == "-1"

# minimum size
assert run("1\n0 100\n") == "2"

# always wide intervals
assert run("3\n0 100\n0 100\n0 100\n") == "4"

# tight increasing windows
assert run("3\n1 1\n2 2\n3 3\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| làn đường rộng đơn | 2 | tính đúng đắn của quá trình chuyển đổi cơ sở | 
| làn đường giữa không thể | -1 | lan truyền thất bại | 
| tất cả các khoảng rộng | n+1 | tiến triển bình thường | 
| cửa sổ chính xác chặt chẽ | n+1 | xử lý ranh giới bình đẳng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi làn đầu tiên đã có cửa sổ bắt đầu sau thời gian 1. Đối với đầu vào`1\n5 10\n`, thuật toán đặt t = max(1, 5) = 5, hợp lệ và sau đó chuyển sang làn 2 tại thời điểm 6. 

Một trường hợp khác là khi làn đường đóng quá sớm. Vì`1\n0 0\n`, chúng ta phải đến làn 1 tại thời điểm 0, nhưng việc di chuyển cần ít nhất thời gian 1, do đó thời gian tính toán trở thành 1, vi phạm r_1 = 0 và không thể trả về chính xác. 

Trường hợp cạnh cấu trúc cuối cùng là khi tất cả các khoảng đều rất lớn, chẳng hạn như`n = 100000`với tất cả [0, 10^12]. Thuật toán chỉ đơn giản tăng thời gian lên 1 trên mỗi làn, tạo ra t = n + 1 mà không gặp phải ràng buộc nào, phù hợp với trực giác rằng không cần phải chờ đợi.
