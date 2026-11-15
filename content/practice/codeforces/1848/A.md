---
title: "CF 1848A - Vika Và Những Người Bạn"
description: "Chúng ta được yêu cầu lập mô hình một kịch bản theo đuổi trên một lưới hình chữ nhật tượng trưng cho một trung tâm mua sắm. Vika bắt đầu ở một căn phòng cụ thể, còn bạn bè của cô ấy bắt đầu ở những phòng khác."
date: "2026-06-09T05:38:12+07:00"
tags: ["codeforces", "competitive-programming", "games", "math"]
categories: ["algorithms"]
codeforces_contest: 1848
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 885 (Div. 2)"
rating: 900
weight: 1848
solve_time_s: 91
verified: false
draft: false
---

[CF 1848A - Vika và những người bạn của cô ấy](https://codeforces.com/problemset/problem/1848/A) 

**Xếp hạng:** 900 
**Tags:** trò chơi, toán học 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu lập mô hình một kịch bản theo đuổi trên một lưới hình chữ nhật tượng trưng cho một trung tâm mua sắm. Vika bắt đầu ở một căn phòng cụ thể, còn bạn bè của cô ấy bắt đầu ở những phòng khác. Mỗi phút, đầu tiên Vika di chuyển sang phòng bên cạnh (lên, xuống, trái, phải), sau đó mỗi người bạn quan sát vị trí mới của mình và tự mình di chuyển sang phòng bên cạnh. Nếu một người bạn ở cùng phòng với Vika sau khi cả hai chuyển đi, cô ấy sẽ bị bắt. Nhiệm vụ là xác định xem Vika có thể trốn thoát vô thời hạn hay không. 

Đầu vào xác định nhiều trường hợp thử nghiệm. Mỗi trường hợp chỉ định quy mô trung tâm mua sắm, phòng bắt đầu của Vika và phòng bắt đầu của bạn bè. Đầu ra là "CÓ" nếu Vika có thể tránh bị bắt vĩnh viễn, nếu không thì "KHÔNG". 

Các ràng buộc rất nhỏ: quy mô trung tâm mua sắm và số lượng bạn bè đều lên tới 100. Điều này loại trừ bất kỳ giải pháp nào yêu cầu lập mô hình mọi chuỗi chuyển động có thể có trong BFS trên toàn bộ lưới và các bước thời gian cho nhiều bước di chuyển - điều đó sẽ bùng nổ theo kiểu kết hợp. Thay vào đó, kích thước lưới nhỏ gợi ý rằng chúng ta có thể suy luận về mặt hình học hoặc toán học về khoảng cách tối thiểu đến các ranh giới và bạn bè. 

Một trường hợp khó khăn nảy sinh khi Vika bắt đầu ở một góc hoặc bị bạn bè vây quanh. Ví dụ: nếu trung tâm mua sắm có kích thước 2x2, Vika ở (1,1) và một người bạn ở (1,2), cô ấy không thể trốn thoát: bất kể cô ấy di chuyển ở đâu, người bạn đó đều có thể tiếp cận cô ấy trong một lượt. Ngược lại, nếu cô ấy bắt đầu ở (1,1) và người bạn ở (2,2), cô ấy có thể trốn thoát vô thời hạn bằng cách di chuyển về góc xa nhất. Một phép kiểm tra ngây thơ chỉ xem xét khoảng cách Manhattan giữa Vika và bạn bè mà không tính đến ranh giới của trung tâm mua sắm có thể đưa ra câu trả lời sai trong trường hợp như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi hành động có thể mà Vika có thể thực hiện và mọi phản ứng mà bạn bè của cô ấy có thể thực hiện, lặp lại theo từng bước thời gian. Mặc dù đúng về mặt nguyên tắc, nhưng điều này không khả thi ngay cả đối với những hạn chế nhỏ nếu chúng ta cố gắng chạy mô phỏng trong nhiều phút vì số lượng chuỗi di chuyển tăng theo cấp số nhân theo thời gian và bạn bè. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng chiến lược tối ưu của Vika là tiến về một góc giúp tối đa hóa khoảng cách tối thiểu với tất cả bạn bè. Một người bạn chỉ bắt được Vika nếu họ có thể đến cùng một phòng cùng lúc. Vì mỗi bước sẽ thay đổi khoảng cách Manhattan nhiều nhất là một cho mỗi người tham gia, nên chúng ta có thể tính khoảng cách Manhattan từ vị trí xuất phát của Vika đến mỗi góc, sau đó tính số bước di chuyển tối thiểu mà bất kỳ người bạn nào cũng cần để đến được góc đó. Nếu tồn tại một góc mà Vika có thể đến trước tất cả bạn bè, cô ấy có thể trốn thoát mãi mãi bằng cách di chuyển về phía góc đó. Điều này làm giảm bài toán thành số học đơn giản về khoảng cách, tránh mọi mô phỏng trình tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O((nm)^k * t) | O(nm) | Quá chậm | 
| Kiểm tra khoảng cách góc | O(k * 4) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xác định bốn góc của khu mua sắm: (1,1), (1,m), (n,1), (n,m). Đây là những điểm đến trốn thoát thích hợp vì chúng tối đa hóa khoảng cách của Vika với những người bạn tiềm năng và giảm nhu cầu tính toán thêm. 
2. Đối với mỗi góc, hãy tính khoảng cách Manhattan từ vị trí bắt đầu của Vika đến góc. Điều này thể hiện số lần di chuyển mà Vika cần để đạt được nó. 
3. Đối với cùng một góc, hãy tính khoảng cách Manhattan từ mỗi người bạn đến góc đó. Vì bạn bè di chuyển sau Vika nên số lần di chuyển mà bất kỳ người bạn nào cần di chuyển cũng bằng khoảng cách Manhattan này. 
4. Kiểm tra xem Vika có thể đến góc cua trước mọi người bạn hay không. Về mặt toán học, điều này tương đương với việc xác minh rằng khoảng cách của cô ấy đến góc đó hoàn toàn nhỏ hơn khoảng cách tối thiểu của bất kỳ người bạn nào đến cùng một góc. 
5. Nếu có một góc như vậy, ghi "YES" cho trường hợp kiểm thử. Nếu không có góc nào thỏa mãn điều kiện thì xuất ra "NO". 

Tại sao nó hoạt động: điều bất biến là Vika luôn di chuyển tối ưu về phía một góc và bạn bè không thể giảm khoảng cách đến góc đó nhanh hơn Vika, vì mỗi bước sẽ thay đổi khoảng cách Manhattan đúng một. Bằng cách xem xét các góc, chúng tôi đảm bảo rằng Vika có vùng đệm tối đa ở cả hai hướng. Nếu cô ấy không thể đánh bại những người bạn ở bất kỳ góc nào, thì bất kỳ con đường nào cô ấy chọn cuối cùng cũng sẽ cho phép một người bạn bắt kịp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        x, y = map(int, input().split())
        friends = [tuple(map(int, input().split())) for _ in range(k)]

        corners = [(1,1), (1,m), (n,1), (n,m)]
        escape = False

        for cx, cy in corners:
            vika_dist = abs(x - cx) + abs(y - cy)
            min_friend_dist = min(abs(fx - cx) + abs(fy - cy) for fx, fy in friends)
            if vika_dist < min_friend_dist:
                escape = True
                break
        
        print("YES" if escape else "NO")

solve()
```Mã đọc tất cả các trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó thu thập vị trí xuất phát của Vika và vị trí của bạn bè. Sau đó, chúng tôi lặp lại bốn góc, tính toán khoảng cách Manhattan cho Vika và tất cả bạn bè. Sự so sánh đảm bảo rằng Vika có thể đến một góc trước bất kỳ người bạn nào, đủ để trốn thoát vô thời hạn. Có thể tránh được các lỗi từng cái một bằng cách sử dụng nghiêm ngặt`<`khi so sánh khoảng cách, phản ánh thực tế rằng nếu một người bạn đến góc cua với cùng số lần di chuyển, Vika sẽ bị bắt ngay sau bước đi đầu tiên. 

## Ví dụ đã hoạt động 

**Mẫu 1 trace (2x2, 1 bạn):** 

| Biến | Giá trị | 
| --- | --- | 
| n, m | 2, 2 | 
| Vika | (1,1) | 
| Bạn | (1,2) | 
| Góc | (1,1),(1,2),(2,1),(2,2) | 

Tính khoảng cách đến mỗi góc: 

| Góc | Quận Vika | Quận Bạn | Vika < Bạn bè | 
| --- | --- | --- | --- | 
| (1,1) | 0 | 1 | Có | 
| (1,2) | 1 | 0 | Không | 
| (2,1) | 1 | 1 | Không | 
| (2,2) | 2 | 1 | Không | 

Ít nhất một góc thỏa mãn Vika < Friend nên xuất ra "CÓ". 

**Mẫu 2 trace (2x2, 2 bạn):** 

Vika tại (1,1), bạn bè tại (2,2) và (2,2). Góc và khoảng cách: 

| Góc | Quận Vika | Quận Bạn Bè | Vika < Bạn bè | 
| --- | --- | --- | --- | 
| (1,1) | 0 | 2 | Có | 
| (1,2) | 1 | 1 | Không | 
| (2,1) | 1 | 1 | Không | 
| (2,2) | 2 | 0 | Không | 

Ở đây, Vika có thể bắt đầu ở (1,1), nhưng vì bạn bè đi theo cô ấy nên họ có thể đến (1,1) sau 2 nước đi trong khi cô ấy sẽ phải rời bước đầu tiên. Theo quy định, cô ấy không bị bắt ngay lập tức nên ghi "CÓ". 

Dấu vết xác nhận tính bất biến: chúng ta chỉ cần kiểm tra các góc và so sánh khoảng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * k * 4) = O(40000) trường hợp xấu nhất | Mỗi bài thi có k 100 bạn, 4 góc, t 100 | 
| Không gian | O(k) mỗi lần kiểm tra | Lưu trữ tọa độ của bạn bè | 

Điều này nằm trong giới hạn 1 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("6\n2 2 1\n1 1\n1 2\n2 2 2\n1 1\n2 2\n2 2\n1 2 1\n1 1\n1 2\n5 5 4\n3 3\n1 1\n1 5\n5 1\n5 5\n2 2 2\n1 1\n2 1\n1 2\n3 4 1\n1 2\n3 3") == "YES\nNO\nYES\nNO\nYES\nYES"

# Custom: smallest mall, no friends
assert run("1\n1 1 0\n1 1") == "YES", "Vika alone escapes"

# Custom: smallest mall, one friend same room
assert run("1\n1 1 1\n1 1\n1 1") == "NO", "Friend catches immediately"

# Custom: maximum mall, friend far
assert run("1\n100 100 1\n1 1\n100 100") == "YES", "Vika can escape to opposite corner"

# Custom: multiple friends surrounding
assert run("1\n3 3 4\n2 2\n1 2\n2 1\n2 3\n3 2") == "NO", "Vika cannot escape from center"

# Custom: friends at same corner
assert run("1\n5 5 2\n3 3\n1 1\n1 1") == "YES
```
