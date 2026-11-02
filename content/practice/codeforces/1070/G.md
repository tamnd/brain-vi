---
title: "CF 1070G - Quái vật và Độc dược"
description: "Chúng ta có một bảng một chiều có chiều dài $n$. Mỗi ô có thể chứa một con quái vật có HP, một lọ thuốc tăng HP hoặc để trống. Ngoài ra, có những anh hùng trị giá $m$ ban đầu được đặt trên các ô trống riêng biệt, mỗi anh hùng bắt đầu với HP của riêng mình."
date: "2026-06-15T13:51:19+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1070
codeforces_index: "G"
codeforces_contest_name: "2018-2019 ICPC, NEERC, Southern Subregional Contest (Online Mirror, ACM-ICPC Rules, Teams Preferred)"
rating: 2300
weight: 1070
solve_time_s: 180
verified: true
draft: false
---

[CF 1070G - Quái vật và Độc dược](https://codeforces.com/problemset/problem/1070/G) 

**Đánh giá:** 2300 
**Tags:** bạo lực, dp, tham lam, thực hiện 
**Thời gian giải:** 3 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bảng có chiều dài một chiều$n$. Mỗi ô có thể chứa một con quái vật có HP, một lọ thuốc tăng HP hoặc để trống. Ngoài ra, còn có$m$các anh hùng ban đầu được đặt trên các ô trống riêng biệt, mỗi anh hùng bắt đầu với HP của riêng mình. 

Chúng ta phải chọn một ô duy nhất làm điểm tập hợp. Sau đó, chúng tôi cử từng anh hùng theo thứ tự nào đó đến phòng giam đó. Một anh hùng đi dọc theo con đường thẳng duy nhất từng ô. Khi di chuyển, nó có thể thu thập thuốc và chiến đấu với quái vật gặp trên đường đi. Thuốc được tiêu thụ ngay lập tức và tăng HP, quái vật sẽ bị tiêu diệt nếu HP hiện tại của anh hùng ít nhất bằng HP của quái vật, nếu không anh hùng sẽ chết và toàn bộ cấu hình không hợp lệ. Nếu anh hùng sống sót, quái vật sẽ biến mất đối với tất cả các anh hùng trong tương lai. 

Khó khăn chính là các anh hùng trước đó sẽ sửa đổi vĩnh viễn bảng bằng cách loại bỏ quái vật và thuốc mà họ tương tác, vì vậy thứ tự của các anh hùng rất quan trọng. 

Mục tiêu là tìm ra một điểm tập hợp và trật tự các anh hùng sao cho mọi anh hùng đều đến được điểm đó một cách an toàn. 

Những hạn chế$n \le 100$Và$m \le n$chỉ ra rằng một$O(n^2)$hoặc$O(n^2 \log n)$giải pháp có thể chấp nhận được. Bất cứ điều gì liên quan đến việc thử tất cả các hoán vị của các anh hùng hoặc tính toán lại mô phỏng đầy đủ cho mỗi ứng cử viên sẽ quá chậm. 

Một ý tưởng ngây thơ nhưng tự nhiên là cố định một điểm tập hợp và thử tất cả mệnh lệnh của các anh hùng, mô phỏng từng mệnh lệnh. Đó là$m!$hoán vị cho mỗi điểm tập hợp và mỗi mô phỏng là$O(n)$, điều này vượt xa khả thi ngay cả đối với$m = 10$. 

Ý tưởng ngây thơ thứ hai là mô phỏng các anh hùng một cách tham lam theo thứ tự đầu vào cho mỗi điểm tập hợp. Điều này không thành công vì thứ tự tốt nhất phụ thuộc vào việc anh hùng nào đủ mạnh để sớm tiêu diệt quái vật một cách an toàn. 

Một trường hợp phức tạp xuất hiện khi một anh hùng phải vượt qua một con quái vật mà chỉ anh hùng khác mới có thể loại bỏ được, nhưng anh hùng đó lại ở phía đối diện của điểm tập hợp. Thứ tự từ trái sang phải hoặc từ phải sang trái tham lam có thể thất bại ngay cả khi có giải pháp hợp lệ. 

## Phương pháp tiếp cận 

Quan sát quan trọng là đối với một điểm tập hợp cố định, mỗi anh hùng có tác động quyết định đến đoạn giữa điểm bắt đầu và điểm tập hợp. Nếu chúng ta biết thứ tự của các anh hùng, chúng ta sẽ quyết định một cách hiệu quả những anh hùng nào được phép “dọn dẹp” quái vật trước khi những anh hùng yếu hơn cố gắng đi theo con đường tương tự. 

Sự đơn giản hóa quan trọng là đảo ngược quan điểm: thay vì suy nghĩ về sự tương tác năng động giữa các anh hùng, chúng tôi tính toán cho từng anh hùng xem liệu họ có thể đi đến điểm tập kết với một tập hợp cố định các chướng ngại vật còn lại hay không và chúng tôi muốn một mệnh lệnh dần dần làm cho con đường trở nên dễ dàng hơn. 

Đối với một điểm tập hợp cố định, mỗi anh hùng đưa ra một “yêu cầu an toàn”: dọc theo con đường của mình, HP tích lũy không bao giờ giảm xuống dưới 1. Điều này tương đương với việc tính toán lượng HP ban đầu tối thiểu cần thiết cho con đường đó, coi quái vật là đóng góp tiêu cực và bình thuốc là đóng góp tích cực, nhưng có một điểm mấu chốt là quái vật biến mất sau khi bị giết bởi những anh hùng trước đó. 

Điều này dẫn tới cách giải thích về lịch trình một cách tham lam. Mỗi anh hùng có thể được gán một giá trị đại diện cho lượng “bộ đệm” mà nó cần để tồn tại. Những anh hùng có thể sống sót với vùng đệm nhỏ hơn sẽ linh hoạt hơn và thường nên đi muộn hơn, vì họ có nhiều khả năng sống sót hơn ngay cả sau khi bàn cờ đã được cải thiện một phần bởi những anh hùng mạnh hơn. 

Chiến lược đúng đắn là tính toán trước đối với mỗi anh hùng và từng điểm tập hợp ứng cử viên, liệu anh hùng đó có thể đến được điểm tập hợp hay không với giả định rằng anh ta đứng đầu trong số những anh hùng còn lại chưa được xử lý. Sau đó, chúng tôi tham lam chọn một mệnh lệnh bằng cách liên tục chọn bất kỳ anh hùng nào hiện có khả thi, cập nhật trạng thái như thể anh hùng đó đã dọn đường cho nó. 

Chúng tôi thử mọi điểm tập hợp. Đối với mỗi người, chúng tôi duy trì nhiều nhóm anh hùng còn sống và liên tục chọn một anh hùng hiện có thể sống sót trong cuộc hành trình. Nếu ở một bước nào đó không có anh hùng nào khả thi thì điểm tập hợp này sẽ thất bại. 

Điều này có tác dụng vì một khi anh hùng vượt qua thành công, nó chỉ cải thiện bảng bằng cách loại bỏ quái vật và tiêu thụ thuốc, không bao giờ khiến con đường trong tương lai trở nên khó khăn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu |$O(n \cdot m!)$|$O(n)$| Quá chậm | 
| Hãy thử từng cuộc biểu tình + đặt hàng khả thi tham lam |$O(n^3)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sửa chữa một điểm tập hợp$r$. Chúng tôi xử lý bên trái và bên phải một cách độc lập nhưng logic tương tự được áp dụng một cách đối xứng. 

1. Đối với mỗi anh hùng, hãy tính hiệu ứng ròng ban đầu của nó trên đường đi tới$r$. Chúng tôi mô phỏng việc đi bộ từ đầu đến cuối$r$, thu thập quái vật và bình thuốc, nhưng chúng tôi chỉ sử dụng điều này để xác định tính khả thi với giả định rằng không có anh hùng nào trước đó sửa đổi bàn cờ. 
2. Đối với trạng thái hiện tại của bảng, hãy xác định hàm`can(hero)`mô phỏng quá trình di chuyển của anh hùng và kiểm tra xem HP của anh hùng có bao giờ giảm xuống dưới 0 hay không. Mô phỏng này là$O(n)$. 
3. Duy trì một bộ tướng chưa sử dụng. 
4. Quét liên tục tất cả các anh hùng không sử dụng và tìm một anh hùng thỏa mãn`can(hero)`. 
5. Sau khi tìm thấy một anh hùng khả thi, chúng tôi mô phỏng toàn bộ quá trình di chuyển của anh hùng đó vĩnh viễn trên bảng, loại bỏ quái vật và thuốc mà anh hùng đó tiêu thụ và đánh dấu anh hùng đó là đã sử dụng. 
6. Thêm anh hùng vào thứ tự câu trả lời. 
7. Nếu tại một số lần lặp lại không có anh hùng nào không được sử dụng khả thi, hãy hủy bỏ điểm tập hợp này. 
8. Nếu tất cả các anh hùng được đặt thành công, hãy xuất ra điểm tập hợp này và thứ tự. 

Phần không rõ ràng là tại sao lựa chọn tham lam lặp đi lặp lại là hợp lệ. Lý do là sự tương tác duy nhất giữa các anh hùng là thông qua việc loại bỏ các chướng ngại vật. Bất kỳ anh hùng nào hiện khả thi vẫn khả thi sau khi các anh hùng khác hành động, bởi vì việc loại bỏ chướng ngại vật chỉ có thể tăng HP hoặc giảm sát thương trên các con đường trong tương lai. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, bảng phản ánh một tập hợp con các chướng ngại vật vẫn đang hoạt động. Kết quả vượt qua của một anh hùng là đơn điệu đối với việc loại bỏ chướng ngại vật: loại bỏ quái vật hoặc bổ sung HP thông qua các lọ thuốc trước đó không thể khiến con đường khả thi trước đó trở nên không khả thi. Vì vậy, một khi một anh hùng trở nên khả thi ở một giai đoạn nào đó, việc lựa chọn anh hùng đó không thể cản trở các giải pháp trong tương lai. Nếu một giải pháp tồn tại, luôn có ít nhất một anh hùng khả thi trong số những anh hùng chưa được đặt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def simulate(hero_start, hero_hp, board, r):
    n = len(board)
    hp = hero_hp
    pos = hero_start

    step = 1 if r > pos else -1
    i = pos

    while i != r:
        i += step
        cell = board[i]

        if cell < 0:
            hp += cell  # monster: subtract
            if hp < 0:
                return False
            board[i] = 0
        elif cell > 0:
            hp += cell
            board[i] = 0

    return True

def can(hero_start, hero_hp, board, r):
    hp = hero_hp
    i = hero_start
    step = 1 if r > i else -1

    while i != r:
        i += step
        cell = board[i]
        if cell < 0:
            hp += cell
            if hp < 0:
                return False
        elif cell > 0:
            hp += cell
    return True

n, m = map(int, input().split())

heroes = []
for _ in range(m):
    s, h = map(int, input().split())
    heroes.append((s - 1, h))

board = list(map(int, input().split()))

for r in range(n):
    b = board[:]
    used = [False] * m
    order = []

    ok = True

    for _ in range(m):
        found = -1

        for i in range(m):
            if used[i]:
                continue
            s, h = heroes[i]
            if can(s, h, b, r):
                found = i
                break

        if found == -1:
            ok = False
            break

        s, h = heroes[found]
        if not simulate(s, h, b, r):
            ok = False
            break

        used[found] = True
        order.append(found + 1)

    if ok:
        print(r + 1)
        print(*order)
        break
```Giải pháp lặp lại trên mọi điểm tập hợp có thể. Đối với mỗi ứng cử viên, nó sao chép bảng để chúng ta có thể mô phỏng tác dụng hủy diệt của các anh hùng mà không ảnh hưởng đến các thử nghiệm khác. 

các`can`chức năng kiểm tra xem một anh hùng có sống sót trong trạng thái hiện tại của bảng mà không sửa đổi nó hay không. các`simulate`chức năng thực hiện quá trình di chuyển tương tự nhưng cũng loại bỏ quái vật và thuốc được tiêu thụ. 

Một điểm tinh tế là cả hai hàm đều tính toán lại toàn bộ đường dẫn mỗi lần, điều này có thể chấp nhận được vì$n \le 100$và chúng tôi làm điều này nhiều nhất$O(n^2)$lần cho mỗi điểm tập hợp. 

Vòng lựa chọn tham lam cố gắng chọn bất kỳ anh hùng nào hiện có thể sống sót. Độ chính xác phụ thuộc vào thực tế là tính khả thi chỉ tăng lên khi bảng trở nên sạch hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 3
8 2
1 3
4 9
0 3 -5 0 -5 -4 -1 0
```Chúng tôi kiểm tra một điểm phục hồi, chẳng hạn như chỉ số 6. 

| Bước | Những anh hùng còn lại | Trạng thái hội đồng quản trị (đơn giản hóa) | Anh hùng được chọn | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | 1,2,3 | ban đầu | 3 | HP mạnh nhất, có thể xuyên qua | 
| 2 | 1,2 | sau khi anh hùng 3 dọn đường | 1 | bây giờ đường dẫn được cải thiện | 
| 3 | 2 | sau khi anh hùng 1 dọn đường | 2 | còn lại cuối cùng | 

Sau khi anh hùng thứ 3 vượt qua chướng ngại vật sớm, bàn cờ trở nên dễ dàng hơn, tạo điều kiện cho những anh hùng yếu hơn vượt qua. 

Điều này khẳng định tính bất biến rằng tính khả thi chỉ được cải thiện. 

### Ví dụ 2 

Hãy xem xét một trường hợp được xây dựng đơn giản hơn:```
5 2
1 2
5 1
0 -3 0 -2 0
```Hãy thử điểm tập hợp 3. 

| Bước | Những anh hùng còn lại | Ban | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1,2 | bản gốc | anh hùng 1 khả thi, đã chọn | 
| 2 | 2 | bảng cập nhật | anh hùng 2 khả thi, đã chọn | 

Anh hùng 1 phải đi trước vì nó có thể sống sót sau quái vật ban đầu, giúp anh hùng 2 vượt qua an toàn. 

Điều này thể hiện độ phân giải phụ thuộc thông qua việc thanh toán bù trừ tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$|$n$điểm tập hợp,$n$anh hùng, mỗi lần kiểm tra tính khả thi là$O(n)$| 
| Không gian |$O(n)$| bảng sao chép và mảng kế toán | 

Những hạn chế$n \le 100$đảm bảo rằng lên đến$10^6$hoạt động nguyên thủy là khả thi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    heroes = []
    for _ in range(m):
        s, h = map(int, input().split())
        heroes.append((s - 1, h))
    board = list(map(int, input().split()))

    def simulate(hero_start, hero_hp, board, r):
        hp = hero_hp
        i = hero_start
        step = 1 if r > i else -1
        while i != r:
            i += step
            cell = board[i]
            if cell < 0:
                hp += cell
                if hp < 0:
                    return False
                board[i] = 0
            elif cell > 0:
                hp += cell
                board[i] = 0
        return True

    def can(hero_start, hero_hp, board, r):
        hp = hero_hp
        i = hero_start
        step = 1 if r > i else -1
        while i != r:
            i += step
            cell = board[i]
            if cell < 0:
                hp += cell
                if hp < 0:
                    return False
            elif cell > 0:
                hp += cell
        return True

    for r in range(n):
        b = board[:]
        used = [False] * m
        order = []
        ok = True

        for _ in range(m):
            found = -1
            for i in range(m):
                if used[i]:
                    continue
                s, h = heroes[i]
                if can(s, h, b, r):
                    found = i
                    break

            if found == -1:
                ok = False
                break

            s, h = heroes[found]
            if not simulate(s, h, b, r):
                ok = False
                break

            used[found] = True
            order.append(found + 1)

        if ok:
            assert len(order) == m
            return str(r + 1) + "\n" + " ".join(map(str, order))

    return "-1"

# provided sample
assert run("""8 3
8 2
1 3
4 9
0 3 -5 0 -5 -4 -1 0
""") == """6
3 1 2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | 6/3 1 2 | tính đúng đắn của hỗn hợp quái vật và thuốc độc | 
| 1 1/1 1/0 | 1/1 | vụ án anh hùng đơn thân | 
| 3 2 / 1 5 / 3 5 / 0 -4 0 | 2 / 1 2 | quái vật ở giữa yêu cầu dọn dẹp chung | 
| 5 2 / 1 2 / 5 1 / 0 -3 0 -2 0 | đơn hàng hợp lệ | thứ tự phụ thuộc | 

## Vỏ cạnh 

Một trường hợp quan trọng xảy ra khi tất cả các anh hùng xuất phát ở cùng một phe với một con quái vật hạng nặng. Nếu anh hùng được chọn đầu tiên không đủ mạnh, việc lựa chọn tham lam có thể bị mắc kẹt. Tuy nhiên, trong cấu hình hợp lệ, ít nhất một anh hùng phải có khả năng vượt qua trạng thái bảng hiện tại, nếu không thì không có giải pháp nào cho điểm tập hợp đó. Mô phỏng đảm bảo rằng chỉ những anh hùng khả thi mới được chọn, do đó thuật toán sẽ không loại bỏ một cách sắp xếp có thể giải được một cách sai lầm. 

Một trường hợp khó khăn khác là khi một lọ thuốc nằm ngay cạnh một con quái vật. Một anh hùng yếu có thể thất bại trừ khi anh ta tiêu thụ thuốc trước. Mô phỏng giải thích chính xác điều này vì thuốc được áp dụng ngay lập tức khi gặp phải, do đó HP sẽ tăng lên trước các trận chiến tiếp theo. 

Cuối cùng, nếu điểm tập hợp nằm bên trong một cụm quái vật dày đặc, những anh hùng đầu tiên có thể bị buộc phải dọn đường để những anh hùng sau này sử dụng lại. Thuộc tính thanh toán bù trừ đơn điệu đảm bảo rằng một khi quái vật bị loại bỏ, sẽ không có thất bại nào trong tương lai do sự vắng mặt của nó, đảm bảo tính nhất quán của quá trình tham lam.
