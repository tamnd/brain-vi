---
title: "CF 104842B - Bóng rổ Plus-Trừ"
description: "Trò chơi mô phỏng một trận đấu bóng rổ trong đó hai đội mỗi đội bắt đầu với năm cầu thủ đang thi đấu và năm cầu thủ dự bị. Theo thời gian, có hai điều xảy ra: các cầu thủ được hoán đổi giữa sân và băng ghế dự bị, và các sự kiện ghi bàn xảy ra."
date: "2026-06-28T11:31:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104842
codeforces_index: "B"
codeforces_contest_name: "2020-2021 ICPC, Moscow Subregional"
rating: 0
weight: 104842
solve_time_s: 57
verified: true
draft: false
---

[CF 104842B - Bóng rổ Plus-Minus](https://codeforces.com/problemset/problem/104842/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trò chơi mô phỏng một trận đấu bóng rổ trong đó hai đội mỗi đội bắt đầu với năm cầu thủ đang thi đấu và năm cầu thủ dự bị. Theo thời gian, có hai điều xảy ra: các cầu thủ được hoán đổi giữa sân và băng ghế dự bị, và các sự kiện ghi bàn xảy ra. Điều khó khăn là việc ghi điểm không chỉ thuộc về tổng điểm của một đội mà được chia cho từng cầu thủ hiện đang có mặt trên sân. 

Bất cứ khi nào một đội ghi được một rổ có giá trị x điểm, mọi cầu thủ của đội đó hiện đang chơi trên sân sẽ nhận được x vào thống kê cá nhân của họ. Đồng thời, mọi đấu thủ của đội đối phương hiện có trên sân đều thua x. Việc thay người không ảnh hưởng đến điểm tích lũy, nhưng chúng thay đổi cầu thủ nào hiện đang bị ảnh hưởng bởi các sự kiện ghi điểm trong tương lai. 

Nhiệm vụ là xử lý toàn bộ chuỗi các lần thay người và các sự kiện ghi điểm theo thứ tự và tính toán giá trị cộng trừ cuối cùng của họ đối với mọi cầu thủ đã từng xuất hiện trên sân. Đầu ra phải tuân theo thứ tự người chơi xuất hiện lần đầu trong trò chơi chứ không phải thứ tự khai báo đầu vào. 

Các ràng buộc rất nhỏ, tối đa là 1000 sự kiện. Điều này ngay lập tức loại trừ mọi thứ phức tạp hơn mô phỏng tuyến tính hoặc gần tuyến tính. Bất kỳ cách tiếp cận nào cập nhật cho mỗi sự kiện đối với năm người chơi hiện tại của mỗi đội đều dễ dàng đủ nhanh vì mỗi sự kiện chạm vào tối đa mười người chơi. 

Một chi tiết tinh tế là các cầu thủ chỉ có thể xuất hiện theo thứ tự đầu ra khi họ lần đầu tiên bước vào sân chứ không phải khi họ có tên trong danh sách ban đầu. Một chi tiết khác là việc thay thế có thể giới thiệu những cầu thủ không được đề cập trước đó trong các sự kiện, do đó hệ thống phải đăng ký tên mới một cách linh hoạt. 

Một trường hợp khác là xuất hiện nhiều lần: một người chơi có thể rời sân và vào lại sân nhiều lần, nhưng điểm của họ sẽ được tích lũy trong tất cả các khoảng thời gian họ hoạt động. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp khớp chính xác với tuyên bố vấn đề. Chúng tôi duy trì năm người chơi hiện tại của mỗi đội trong một bộ và duy trì một từ điển ánh xạ tên từng người chơi với điểm tích lũy của họ. Chúng tôi cũng theo dõi xem người chơi đã được nhìn thấy trước đó hay chưa để sửa thứ tự đầu ra. 

Đối với mỗi sự kiện tính điểm, chúng tôi lặp lại năm cầu thủ đang hoạt động của đội ghi điểm và cộng x, đồng thời lặp lại năm cầu thủ đang hoạt động của đối thủ và trừ x. Đối với các sự kiện thay thế, chúng tôi loại bỏ một người chơi khỏi nhóm đang hoạt động và chèn một người chơi khác mà không thay đổi điểm số. 

Điều này hiệu quả vì số lượng người chơi hoạt động không đổi và nhỏ, vì vậy mỗi sự kiện đều tiêu tốn công sức liên tục. Với tối đa 1000 sự kiện, tổng số hoạt động vẫn còn rất nhỏ. 

Một biến thể bạo lực sẽ cố gắng xây dựng lại dòng thời gian đầy đủ và tính toán lại những đóng góp của mỗi người chơi cho mỗi sự kiện, nhưng điều đó là không cần thiết vì chúng tôi đã duy trì trạng thái chính xác dần dần. Không cần tính toán lại hoặc khôi phục; hệ thống hoàn toàn là phụ gia chuyển tiếp. 

Nhận xét quan trọng là trạng thái duy nhất quan trọng vào bất kỳ lúc nào là năm cầu thủ hiện tại của mỗi đội. Mọi thứ khác chỉ là sổ sách kế toán tích lũy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng tính toán lại đầy đủ cho mỗi sự kiện | O(q * n) | O(n) | Quá chậm/không cần thiết | 
| Mô phỏng trực tiếp với các bộ hoạt động | O(q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích tên đội ban đầu và đội hình xuất phát, đồng thời đánh dấu tất cả những người chơi này là “đã biết” để chúng tôi có thể duy trì thứ tự sau này. Chúng tôi cũng khởi tạo điểm số của họ bằng 0. 
2. Lưu trữ các cầu thủ hiện tại trên sân của mỗi đội theo cấu trúc tập hợp hoặc giống như từ điển. Điều này cho phép cập nhật thành viên O(1) trong quá trình thay thế. 
3. Duy trì một cuốn từ điển`score[player]`để tích lũy các giá trị cộng trừ trong suốt trận đấu. 
4. Duy trì một danh sách`order`ghi lại lần đầu tiên mỗi đấu thủ xuất hiện ở bất kỳ vị trí nào trên sân. Danh sách này sẽ được sử dụng để đặt hàng đầu ra cuối cùng. 
5. Xử lý từng sự kiện theo trình tự thời gian. 
6. Nếu sự kiện này là sự kiện tính điểm cho đội T có giá trị x, hãy lặp lại năm đấu thủ hiện có trên sân cho T và thêm x vào điểm của mỗi người. Sau đó lặp lại năm người chơi của đội đối phương và trừ x cho mỗi người trong số họ. Điều này phản ánh trực tiếp cách tính điểm ảnh hưởng đến số liệu thống kê cá nhân. 
7. Nếu sự kiện là sự thay thế, hãy loại bỏ người chơi ra khỏi nhóm đang hoạt động và chèn người chơi mới vào. Nếu người chơi đến chưa từng xuất hiện trước đó, hãy khởi tạo điểm của họ về 0 và thêm họ vào danh sách thứ tự đầu ra. 
8. Sau tất cả các sự kiện, lặp lại danh sách thứ tự đã ghi và in từng người chơi cùng với đội của họ và điểm đã ký. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, sự đóng góp của một cầu thủ vào thống kê cuối cùng chỉ phụ thuộc vào việc họ có mặt trên sân trong mỗi lần ghi điểm hay không. Thuật toán duy trì tập hợp chính xác những người chơi đang hoạt động ở mỗi bước, do đó, mỗi lần cập nhật tính điểm sẽ được áp dụng chính xác cho đúng tập hợp con người chơi. Vì sự thay thế chỉ thay đổi thành viên trong tập hợp này và không bao giờ ảnh hưởng đến các sự kiện trong quá khứ, nên việc xử lý các sự kiện một cách tuần tự sẽ duy trì tính chính xác mà không cần khôi phục hoặc kiến ​​thức trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def fmt(x):
    if x > 0:
        return f"+{x}"
    return str(x)

first_team = input().strip()
first_players = [input().strip() for _ in range(5)]

second_team = input().strip()
second_players = [input().strip() for _ in range(5)]

score = {}
team_of = {}

on = {first_team: set(first_players),
      second_team: set(second_players)}

order = []
seen = set()

def register(p, team):
    if p not in seen:
        seen.add(p)
        order.append(p)
        score[p] = 0
        team_of[p] = team

for p in first_players:
    register(p, first_team)

for p in second_players:
    register(p, second_team)

q = int(input())
for _ in range(q):
    line = input().strip()

    if "scored" in line:
        parts = line.split()
        team = parts[1]
        val = int(parts[-1])

        for p in on[team]:
            score[p] += val
        other = first_team if team == second_team else second_team
        for p in on[other]:
            score[p] -= val

    else:
        parts = line.split()
        team = parts[1]
        y = parts[3]
        z = parts[5]

        on[team].remove(y)
        on[team].add(z)

        register(z, team)

for p in order:
    s = score[p]
    sign = "" if s == 0 else ("+" if s > 0 else "")
    print(f"{p} ({team_of[p]}) {fmt(s)}")
```Việc triển khai cốt lõi giữ hai nhóm hoạt động, một nhóm cho mỗi nhóm và cập nhật chúng trực tiếp khi có sự thay thế. Logic tính điểm có tính đối xứng: chúng tôi lặp lại năm cầu thủ của đội ghi điểm để cộng điểm và năm cầu thủ của đối phương để trừ. các`register`đảm bảo chúng tôi chỉ thêm người chơi một lần vào thứ tự đầu ra và khởi tạo siêu dữ liệu của họ một cách chính xác khi họ xuất hiện lần đầu trong trò chơi. 

Một điểm tinh tế là phân tích các dòng sự kiện. Thay vì dựa vào các nhánh định dạng nghiêm ngặt, chúng tôi kiểm tra từ khóa`"scored"`, vì điều đó tách biệt rõ ràng hai loại sự kiện. Một điểm tinh tế khác là người chơi có thể được giới thiệu thông qua việc thay người trước khi xuất hiện trong đội hình ban đầu, vì vậy việc đăng ký phải diễn ra cả khi bắt đầu và trong khi thay người. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi một kịch bản đơn giản hóa để xem cách tính điểm và thay người tương tác với nhau. 

### Ví dụ 1 

đầu vào:```
A
p1
p2
p3
p4
p5
B
q1
q2
q3
q4
q5
2
Team A scored 2
Team B replaced q1 with q6
```Chúng tôi theo dõi các bộ hoạt động và điểm số. 

| Bước | Sự kiện | A trên sân | B trên sân | Thay đổi điểm | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | p1..p5 | q1..q5 | tất cả 0 | 
| 1 | A điểm 2 | p1..p5 | q1..q5 | A +2 mỗi cái, B -2 mỗi cái | 
| 2 | B phụ | p1..p5 | q2..q6 | không thay đổi | 

Hiệu ứng cuối cùng chỉ có ở bước 1, chứng tỏ rằng việc thay người chỉ ảnh hưởng đến việc ghi điểm trong tương lai. 

### Ví dụ 2 

đầu vào:```
A
a1
a2
a3
a4
a5
B
b1
b2
b3
b4
b5
3
Team A scored 3
Team A replaced a1 with a6
Team A scored 1
```| Bước | Sự kiện | A trên sân | B trên sân | Thay đổi điểm | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | a1..a5 | b1..b5 | 0 | 
| 1 | A ghi 3 | a1..a5 | b1..b5 | +3 / -3 | 
| 2 | phụ | a2..a6 | b1..b5 | 0 | 
| 3 | A ghi 1 | a2..a6 | b1..b5 | +1 / -1 | 

Điều này cho thấy tại sao việc theo dõi nhóm hoạt động hiện tại là đủ: sự kiện tính điểm thứ hai sử dụng một nhóm người chơi khác với nhóm đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi sự kiện chạm tối đa 10 người chơi (mỗi đội 5 người) | 
| Không gian | O(n) | Lưu trữ điểm số, lập bản đồ nhóm và các nhóm hoạt động | 

Với tối đa 1000 sự kiện, giải pháp chỉ thực hiện vài nghìn thao tác, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    first_team = input().strip()
    first_players = [input().strip() for _ in range(5)]

    second_team = input().strip()
    second_players = [input().strip() for _ in range(5)]

    score = {}
    team_of = {}

    on = {first_team: set(first_players),
          second_team: set(second_players)}

    order = []
    seen = set()

    def register(p, team):
        if p not in seen:
            seen.add(p)
            order.append(p)
            score[p] = 0
            team_of[p] = team

    for p in first_players:
        register(p, first_team)
    for p in second_players:
        register(p, second_team)

    q = int(input())
    for _ in range(q):
        line = input().strip()
        if "scored" in line:
            parts = line.split()
            team = parts[1]
            val = int(parts[-1])
            other = first_team if team == second_team else second_team
            for p in on[team]:
                score[p] += val
            for p in on[other]:
                score[p] -= val
        else:
            parts = line.split()
            team = parts[1]
            y = parts[3]
            z = parts[5]
            on[team].remove(y)
            on[team].add(z)
            register(z, team)

    out = []
    for p in order:
        s = score[p]
        if s > 0:
            out.append(f"{p} ({team_of[p]}) +{s}")
        elif s < 0:
            out.append(f"{p} ({team_of[p]}) {s}")
        else:
            out.append(f"{p} ({team_of[p]}) 0")

    return "\n".join(out)

# custom tests

inp = """A
a1
a2
a3
a4
a5
B
b1
b2
b3
b4
b5
1
Team A scored 1
"""
assert "a1 (A) +1" in run(inp)

inp = """A
a1
a2
a3
a4
a5
B
b1
b2
b3
b4
b5
1
Team B scored 2
"""
assert "a1 (A) -2" in run(inp)

inp = """A
a1
a2
a3
a4
a5
B
b1
b2
b3
b4
b5
3
Team A scored 1
Team A replaced a1 with a6
Team A scored 1
"""
out = run(inp)
assert "a6 (A)" in out
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | người chơi được cập nhật chính xác | tuyên truyền tính điểm cơ bản | 
| điểm đối thủ | cập nhật tiêu cực hoạt động | tính đối xứng của cập nhật | 
| thay thế + nhập lại | theo dõi người chơi mới | tính chính xác của danh sách động | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi một cầu thủ được giới thiệu thông qua việc thay thế và ngay lập tức ảnh hưởng đến thứ tự. Ví dụ: nếu một người chơi mới tham gia vào giữa trận đấu và sau đó góp phần ghi bàn, họ phải xuất hiện sau tất cả những người chơi ban đầu theo thứ tự đầu ra. các`register`đảm bảo điều này bằng cách chỉ thêm vào lần xuất hiện đầu tiên. 

Một trường hợp khác là sự thay thế lặp đi lặp lại của cùng một cầu thủ. Vì chúng tôi sử dụng các bộ nên việc xóa và thêm lại là an toàn và bình thường. Điểm không được đặt lại nên người chơi quay lại tiếp tục tích lũy từ những đóng góp trước đây. 

Trường hợp cuối cùng là những cầu thủ có tên trong danh sách ban đầu nhưng không bao giờ ra sân do bị thay người ngay lập tức. Những điều này vẫn xuất hiện ở đầu ra vì ban đầu chúng có mặt trên sân, ngay cả khi chúng không bao giờ nhận được cập nhật về điểm số.
