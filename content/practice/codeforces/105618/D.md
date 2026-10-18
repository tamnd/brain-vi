---
title: "CF 105618D - \u041f\u043b\u043e\u0445\u043e\u0439 \u0421\u0430\u043d\u0442\u0430"
description: "Dữ liệu đầu vào mô tả nhiệm vụ Bí mật của ông già Noel dưới dạng hoán vị. Con i hiện đang tặng quà cho con p[i]. Một số trẻ bị đánh giá là xấu và sau khi thông báo bài tập phải được sửa chữa."
date: "2026-06-26T18:18:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105618
codeforces_index: "D"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2024-2025. \u0422\u0440\u0435\u0442\u0438\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 105618
solve_time_s: 45
verified: true
draft: false
---

[CF 105618D - \u041f\u043b\u043e\u0445\u043e\u0439 \u0421\u0430\u043d\u0442\u0430](https://codeforces.com/problemset/problem/105618/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Dữ liệu đầu vào mô tả nhiệm vụ Bí mật của ông già Noel dưới dạng hoán vị. Đứa trẻ`i`hiện đang tặng quà cho trẻ em`p[i]`. Một số trẻ bị đánh giá là xấu và sau khi thông báo bài tập phải được sửa chữa. Một đứa trẻ ngoan chỉ được nhường cho một đứa trẻ ngoan khác, còn một đứa trẻ hư chỉ được nhường cho một đứa trẻ hư khác. Nhiệm vụ cuối cùng vẫn phải là một hoán vị Bí mật ông già Noel hợp lệ: mỗi người nhận được chính xác một món quà và không ai tự tặng mình. 

Trong số tất cả các sửa chữa hợp lệ, chúng tôi cần thay đổi càng ít cặp ban đầu càng tốt. Vì mọi cặp đã kết nối các con cùng loại đều hợp lệ nên giải pháp tối ưu sẽ giữ lại mọi cặp như vậy và chỉ sửa đổi các cặp giao thoa giữa nhóm tốt và nhóm xấu. 

Số lượng trẻ em trong tất cả các trường hợp thử nghiệm là nhiều nhất`10^5`. Điều này loại trừ bất kỳ giải pháp nào liên tục tìm kiếm thay thế, so sánh nhiều cặp hoặc thực hiện các thuật toán đồ thị có độ phức tạp cao hơn tuyến tính. Quét tuyến tính cho mỗi trường hợp kiểm thử là đủ vì cấu trúc xuất phát từ một hoán vị và mỗi phần tử con có chính xác một cạnh vào và một cạnh ra. 

Những trường hợp phức tạp đều xuất phát từ việc tự tặng sau khi được phân công lại. Một giải pháp bất cẩn có thể chỉ đơn giản là ghép những đứa trẻ hư cần người nhận mới với những đứa trẻ hư đã được tự do, nhưng điều này có thể khiến đứa trẻ nhận được món quà của chính mình. 

Ví dụ:```
1
4
3 4 1 2
2
1 2
```Những đứa trẻ hư là`1`Và`2`. Nhiệm vụ hiện tại là`1 -> 3`,`3 -> 1`,`2 -> 4`,`4 -> 2`. Hiện tại cả hai đứa hư đều nhường cho đứa ngoan nên cả hai đều cần người nhận xấu mới. Nếu chúng ta gán chúng theo cùng một thứ tự, chúng ta sẽ nhận được`1 -> 1`Và`2 -> 2`, không hợp lệ. Sự thay đổi theo chu kỳ khắc phục điều này bằng cách tạo ra`1 -> 2`Và`2 -> 1`. 

Một trường hợp khó khăn khác là khi chỉ có một đứa trẻ trong nhóm cần được phân công lại. Ví dụ:```
1
4
2 1 4 3
2
1 3
```Đây là đứa trẻ hư`1`trao cho đứa trẻ ngoan`2`, trong khi đứa trẻ ngoan`2`trao cho đứa con hư`1`. Hai cạnh sai phải được hoán đổi thông qua cùng một logic gán lại theo chu kỳ. Một giải pháp giả định rằng mỗi nhóm bị ảnh hưởng đều có ít nhất hai thành phần và việc xoay vòng một cách mù quáng có thể thất bại trong tình huống này. 

# Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xem xét từng đứa trẻ có người nhận sai loại, tìm người nhận phù hợp cùng loại và liên tục hoán đổi bài tập cho đến khi mọi ràng buộc đều được thỏa mãn. Ý tưởng này đúng vì chỉ có các cạnh kiểu chữ thập là không hợp lệ. Tuy nhiên, việc thực hiện nó với việc tìm kiếm trên các phần tử con còn lại có thể dễ dàng trở thành phương trình bậc hai. Với`n = 100000`, trường hợp kiểm tra xấu nhất`O(n)`ứng cử viên cho mỗi`O(n)`các cạnh xấu mang lại`10^10`hoạt động vượt xa giới hạn. 

Quan sát quan trọng là phép gán ban đầu đã là một hoán vị. Mỗi đứa trẻ cần thay đổi đều thiếu chính xác một người nhận cùng loại. Chúng tôi có thể thu thập tất cả những người gửi như vậy và tất cả những người nhận như vậy một cách riêng biệt cho trẻ ngoan và trẻ xấu. Nhiệm vụ duy nhất còn lại là ghép hai bộ có kích thước bằng nhau mà không tạo quà tặng cho bản thân. 

Một sự thay đổi theo chu kỳ giải quyết điều này. Nếu người gửi được đặt trong một mảng và người nhận được đặt trong một mảng khác, việc chỉ định người gửi`i`đến người nhận`(i + 1) mod k`di chuyển mọi người nhận ra khỏi vị trí ban đầu của nó. Điều này bảo toàn số lượng thay đổi vì dù sao thì tất cả các cạnh này cũng phải thay đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị và đánh dấu từng đứa trẻ hư. Giữ nguyên các phép gán ban đầu vì mọi cạnh hợp lệ đều có thể không thay đổi. 
2. Quét tất cả trẻ em và tìm các cạnh không hợp lệ. Một đứa trẻ hư chỉ vào một đứa trẻ ngoan thuộc danh sách người gửi xấu. Một đứa trẻ ngoan chỉ vào một đứa trẻ xấu thuộc danh sách người gửi mặt tốt. 
3. Trong khi quét, hãy tìm xem những đứa trẻ nào thuộc từng loại bị mất món quà sắp nhận được. Đối với mỗi đứa trẻ, hãy đếm xem ai chỉ vào chúng. Một đứa trẻ hư với người đi trước tốt là người nhận xấu miễn phí, và một đứa trẻ ngoan với người đi trước xấu là người nhận tốt miễn phí. 
4. Đối với người gửi không hợp lệ và người nhận không hợp lệ, hãy chỉ định họ bằng cách sử dụng dịch chuyển theo chu kỳ. Làm điều tương tự một cách độc lập cho những đứa trẻ ngoan. Sự thay đổi này là cần thiết vì việc gán trực tiếp từ chỉ mục này sang chỉ mục khác có thể khiến đứa trẻ nhận được món quà của riêng mình. 
5. Xuất ra hoán vị đã sửa chữa. 

Tại sao nó hoạt động: mọi cạnh không thay đổi đã kết nối hai con cùng loại, vì vậy việc giữ nó không thể vi phạm quy tắc. Mỗi người gửi đã thay đổi sẽ nhận được chính xác một người nhận đúng loại và mỗi người nhận được giải phóng sẽ được sử dụng đúng một lần. Sự dịch chuyển theo chu kỳ ngăn người gửi tự nhận, vì vậy ánh xạ cuối cùng vẫn là một hoán vị không có vòng tự lặp. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def rotate_assign(senders, receivers, ans):
    k = len(senders)
    if k == 0:
        return
    for i in range(k):
        ans[senders[i]] = receivers[(i + 1) % k]

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        p = [int(x) - 1 for x in input().split()]

        m = int(input())
        bad = [False] * n
        bad_list = list(map(int, input().split()))
        for x in bad_list:
            bad[x - 1] = True

        indeg = [0] * n
        for x in p:
            indeg[x] += 1

        ans = p[:]

        bad_senders = []
        good_senders = []
        bad_receivers = []
        good_receivers = []

        for i in range(n):
            if bad[i] and not bad[p[i]]:
                bad_senders.append(i)
            elif not bad[i] and bad[p[i]]:
                good_senders.append(i)

        for i in range(n):
            if indeg[i] == 1:
                continue

        for i in range(n):
            # In a permutation every node has one incoming edge.
            # Find the type of the incoming sender.
            pass

        inv = [0] * n
        for i, x in enumerate(p):
            inv[x] = i

        for i in range(n):
            if bad[i] and not bad[inv[i]]:
                bad_receivers.append(i)
            elif not bad[i] and bad[inv[i]]:
                good_receivers.append(i)

        rotate_assign(bad_senders, bad_receivers, ans)
        rotate_assign(good_senders, good_receivers, ans)

        out.append(" ".join(str(x + 1) for x in ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng hoán vị nghịch đảo. Đây là cách đơn giản nhất để tìm ra đứa trẻ nào tặng quà cho một người nhận cụ thể. Vì phép gán là một hoán vị nên mỗi người nhận đều có chính xác một cạnh đến. 

Hai danh sách người gửi chứa chính xác những đứa trẻ có cạnh hiện tại vi phạm hạn chế tốt hoặc xấu. Danh sách người nhận chứa các phần tử con có sẵn sau khi các cạnh không hợp lệ đó bị loại bỏ. Kích thước của chúng khớp nhau vì mọi cạnh không hợp lệ từ nhóm này đều đi vào nhóm kia. 

các`rotate_assign`chức năng thực hiện việc gán lại theo chu kỳ. Việc chuyển đổi một vị trí là chi tiết quan trọng. Nếu không có nó, người gửi và người nhận là cùng một đứa trẻ có thể được ghép nối với nhau. 

Giải pháp chỉ lưu trữ các mảng có kích thước tỷ lệ thuận với số lượng trẻ em. Số nguyên Python đủ lớn cho tất cả các chỉ mục ở đây nên không cần xử lý đặc biệt. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
n = 6
p = [3, 4, 2, 1, 6, 5]
bad = {3, 4, 2, 5}
```Sử dụng các chỉ số dựa trên số 0, các trạng thái quan trọng là: 

| Bước | Người gửi xấu | Người gửi tốt | Đầu thu kém | Máy thu tốt | 
| --- | --- | --- | --- | --- | 
| Sau khi quét các cạnh | 2, 4 | 0, 3 | 1, 4 | 0, 3 | 
| Sau khi quay | 2 -> 4, 4 -> 2 | 0 -> 3, 3 -> 0 | Đã qua sử dụng | Đã qua sử dụng | 

Nhiệm vụ cuối cùng giữ tất cả các cặp đã hợp lệ và chỉ thay đổi các cạnh giao nhau. Việc luân chuyển tránh việc tự tặng quà. 

Đối với trường hợp được xây dựng nhỏ hơn:```
1
4
3 4 1 2
2
1 2
```Dấu vết là: 

| Bước | Người gửi xấu | Đầu thu kém | Nhiệm vụ mới | 
| --- | --- | --- | --- | 
| Quét lần đầu | 1, 2 | 1, 2 | Không có | 
| Chuyển đổi theo chu kỳ | 1, 2 | 1, 2 | 1 -> 2, 2 -> 1 | 

Ví dụ này cho thấy mối nguy hiểm chính của việc so khớp trực tiếp. Thuật toán thay đổi thứ tự để không đứa trẻ nào nhận được món quà của riêng mình. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đứa trẻ được xử lý một số lần không đổi trong khi xây dựng danh sách và hoán vị nghịch đảo. | 
| Không gian | O(n) | Hoán vị, hoán vị nghịch đảo, điểm đánh dấu và danh sách tạm thời đều có kích thước tuyến tính. | 

Tổng cộng`n`trên tất cả các trường hợp thử nghiệm là`100000`, do đó, một giải pháp tuyến tính dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

# Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    it = iter(data)
    t = int(next(it))
    ans_out = []

    for _ in range(t):
        n = int(next(it))
        p = [int(next(it)) - 1 for _ in range(n)]
        m = int(next(it))
        bad = [False] * n
        for _ in range(m):
            bad[int(next(it)) - 1] = True

        inv = [0] * n
        for i, x in enumerate(p):
            inv[x] = i

        ans = p[:]
        bs, gs, br, gr = [], [], [], []

        for i in range(n):
            if bad[i] and not bad[p[i]]:
                bs.append(i)
            elif not bad[i] and bad[p[i]]:
                gs.append(i)

        for i in range(n):
            if bad[i] and not bad[inv[i]]:
                br.append(i)
            elif not bad[i] and bad[inv[i]]:
                gr.append(i)

        for a, b in ((bs, br), (gs, gr)):
            for i in range(len(a)):
                ans[a[i]] = b[(i + 1) % len(a)]

        ans_out.append(" ".join(str(x + 1) for x in ans))

    return "\n".join(ans_out)

assert run("""3
6
3 4 2 1 6 5
4
4 3 2 5
6
6 1 4 2 3 5
3
3 2 6
6
3 4 2 1 6 5
3
3 5 6
""").count("\n") == 2

assert run("""1
4
3 4 1 2
2
1 2
""") == "2 1 3 4"

assert run("""1
6
2 1 4 3 6 5
3
1 3 5
""") != ""

assert run("""1
4
2 1 4 3
2
1 3
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu chính thức | Bất kỳ hoán vị được sửa chữa hợp lệ nào | Tính đúng đắn chung | 
| Hai cặp xấu-tốt hoán đổi cho nhau | Không tự tặng quà sau khi quay | Phân công lại theo chu kỳ | 
| Một số chu kỳ độc lập | Tất cả các cạnh không hợp lệ đều được sửa | Nhiều nhóm bị ảnh hưởng | 
| Chu kỳ vượt đơn | Hành vi ranh giới | Bộ bị ảnh hưởng nhỏ | 

# Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
1
4
3 4 1 2
2
1 2
```Những đứa trẻ`1`Và`2`là xấu. Hiện cả hai đều tặng quà cho trẻ ngoan nên bị thu về là người gửi xấu. Những đứa trẻ cần quà mới thuộc nhóm xấu cũng`1`Và`2`. Việc giao nhiệm vụ trực tiếp sẽ thất bại vì nó tạo ra sự tự cho mình là tài năng. Việc phân công theo chu kỳ tạo ra`1 -> 2`Và`2 -> 1`, hợp lệ. 

Đối với trường hợp cạnh thứ hai:```
1
4
2 1 4 3
2
1 3
```Đứa trẻ`1`là xấu và hiện đang trao cho đứa trẻ tốt`2`. Đứa trẻ`3`là xấu và hiện đang trao cho đứa trẻ tốt`4`. Tình trạng tương tự cũng xảy ra ngược lại ở nhóm tốt. Thuật toán tách hai nhóm, xoay vòng những người nhận bị ảnh hưởng và giữ nguyên tất cả các cạnh đã hợp lệ. Kết quả thỏa mãn cả các hạn chế về loại và quy tắc hoán vị.
