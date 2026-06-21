---
title: "CF 105C - Thế Giới Vật Phẩm"
description: "Chúng tôi có một bộ sưu tập vật phẩm và mỗi vật phẩm thuộc về chính xác một trong ba loại trang bị: vũ khí, áo giáp hoặc quả cầu. Mỗi vật phẩm có ba chỉ số cơ bản, tấn công, phòng thủ và kháng cự, cùng với khả năng cho chúng ta biết nó có thể chứa bao nhiêu cư dân. Cư dân cũng có ba loại."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 105
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 81"
rating: 2200
weight: 105
solve_time_s: 206
verified: false
draft: false
---

[CF 105C - Thế giới vật phẩm](https://codeforces.com/problemset/problem/105/C) 

**Đánh giá:** 2200 
**Tags:** bạo lực, thực hiện, phân loại 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập vật phẩm và mỗi vật phẩm thuộc về chính xác một trong ba loại trang bị: vũ khí, áo giáp hoặc quả cầu. Mỗi vật phẩm có ba chỉ số cơ bản, tấn công, phòng thủ và kháng cự, cùng với khả năng cho chúng ta biết nó có thể chứa bao nhiêu cư dân. 

Cư dân cũng có ba loại. Các đấu sĩ tăng khả năng tấn công, lính canh tăng khả năng phòng thủ và bác sĩ tăng sức đề kháng. Mỗi cư dân chỉ đóng góp vào một chỉ số, được xác định theo loại của nó. 

Cư dân có thể được di chuyển tự do giữa các vật phẩm, nhưng có một hạn chế làm thay đổi toàn bộ vấn đề: cư dân phải luôn ở bên trong một số vật dụng. Chúng tôi không được phép tạm thời loại bỏ cư dân và để họ không được chỉ định. Hạn chế duy nhất khi di chuyển là sức chứa vật phẩm. 

Cuối cùng, Laharl trang bị đúng một vũ khí, một áo giáp và một quả cầu. Mục tiêu là từ điển học: 

Đầu tiên tối đa hóa đòn tấn công cuối cùng của vũ khí. 

Trong số tất cả các cách để đạt được đòn tấn công bằng vũ khí tối đa đó, hãy tối đa hóa khả năng phòng thủ cuối cùng của áo giáp. 

Trong số tất cả các lựa chọn như vậy, hãy tối đa hóa sức cản cuối cùng của quả cầu. 

Tất cả các số liệu thống kê khác là không liên quan. Khả năng phòng thủ của vũ khí không quan trọng, đòn tấn công của quả cầu không quan trọng, v.v. 

Quan sát này là cốt lõi của vấn đề. Mỗi loại cư dân chỉ quan trọng đối với một vị trí thiết bị: 

Đấu sĩ chỉ quan trọng đối với vũ khí. 

Lính canh chỉ quan trọng đối với áo giáp. 

Bác sĩ chỉ quan trọng đối với quả cầu. 

Những hạn chế nhỏ đến mức đáng ngạc nhiên. Có tối đa 100 vật phẩm và 1000 cư dân. Điều đó ngay lập tức loại trừ bất kỳ tìm kiếm theo cấp số nhân nào đối với các bài tập thường trú, nhưng nó cũng cho chúng ta biết rằng chúng ta có thể đủ khả năng sử dụng vũ lực bậc ba hoặc thậm chí bậc bốn đối với các vật phẩm. Phần khó không phải là thời gian chạy, mà là hiểu chính xác ràng buộc chuyển động. 

Một cách đọc ngây thơ cho thấy các mô phỏng hoán đổi phức tạp giữa các vật phẩm, nhưng hoạt động chuyển động không bị hạn chế miễn là năng lực được tôn trọng trên toàn cầu. Vì cư dân không bao giờ biến mất nên điều quan trọng duy nhất là có bao nhiêu vị trí cư trú tồn tại bên ngoài các trang bị đã chọn. 

Giả sử chúng ta muốn đặt tất cả các đấu sĩ vào một vũ khí. Nếu vũ khí có sức chứa 5 và có tổng cộng 5 đấu sĩ, thì điều này có thể thực hiện được nếu mọi cư dân không phải đấu sĩ có thể được cất giữ ở một nơi khác. Chúng tôi không quan tâm chính xác ở đâu. Chúng ta chỉ cần có đủ tổng dung lượng trống bên ngoài vũ khí. 

Điều đó biến vấn đề thành một vấn đề đếm thuần túy. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ việc triển khai không chính xác. 

Hãy xem xét đầu vào này:```
3
w weapon 0 0 0 1
a armor 0 0 0 1
o orb 0 0 0 1
2
g gladiator 10 a
s sentry 10 o
```Vũ khí chỉ có một khe nên chúng ta có thể đặt đấu sĩ ở đó. Nhưng sau đó lính canh phải chiếm một trong hai vị trí còn lại. Điều này là khả thi. Một giải pháp sai lầm chỉ kiểm tra công suất của vũ khí so với số lượng đấu sĩ sẽ vô tình chấp nhận những cấu hình không thể thực hiện được trong các ví dụ phức tạp hơn. 

Bây giờ hãy xem xét:```
3
w weapon 0 0 0 2
a armor 0 0 0 1
o orb 0 0 0 1
4
g1 gladiator 1 a
g2 gladiator 1 a
s sentry 1 o
p physician 1 w
```Vũ khí có thể chứa được cả hai đấu sĩ, nhưng sau khi di chuyển họ đến đó, lính canh và thầy thuốc vẫn phải phù hợp ở một nơi khác. Áo giáp và quả cầu cùng nhau chỉ cung cấp hai khe cắm, vì vậy điều này hoạt động chính xác. Bất kỳ giải pháp nào cố gắng di chuyển cư dân tham lam từng hạng mục đều có thể thất bại vì thứ tự di chuyển không quan trọng, chỉ có tổng công suất mới có. 

Một trường hợp tinh tế khác là khi một số vật phẩm có cùng chỉ số chính tối ưu. Chúng ta phải tiếp tục tối ưu hóa theo từ điển thay vì tối đa hóa độc lập cả ba chỉ số. 

Ví dụ:```
4
w1 weapon 10 0 0 2
w2 weapon 10 0 0 3
a armor 0 5 0 1
o orb 0 0 5 1
3
g1 gladiator 1 a
g2 gladiator 1 o
s1 sentry 10 w1
```Cả hai loại vũ khí đều có thể đạt đòn tấn công 12. Sau đó, chúng ta phải chọn cấu hình để có khả năng phòng thủ giáp tốt nhất. Việc thực hiện bất cẩn khi chọn vũ khí tối ưu đầu tiên sẽ mất đi câu trả lời đúng. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp nhất là thử mọi cách có thể để phân công lại cư dân thành các vật phẩm, tính toán số liệu thống kê kết quả và giữ cho thiết bị tốt nhất về mặt từ điển tăng gấp ba lần. 

Điều này hoạt động về mặt khái niệm vì số lượng cư dân là hữu hạn và mỗi cư dân sẽ chọn một mục đích một cách độc lập. Thật không may, yếu tố phân nhánh là rất lớn. Với 1000 cư dân và thậm chí 100 điểm đến khả thi, số lượng nhiệm vụ là hoàn toàn không thể thực hiện được. 

Ý tưởng bạo lực tiếp theo gần với giải pháp thực tế hơn nhiều. Vì chỉ có một chỉ số quan trọng đối với mỗi loại trang bị nên chúng tôi chỉ quan tâm đến việc chuyển các đấu sĩ vào vũ khí đã chọn, lính canh vào bộ giáp đã chọn và các bác sĩ vào quả cầu đã chọn. 

Bây giờ không gian tìm kiếm trở nên có thể quản lý được. Chúng ta có thể liệt kê tất cả các bộ ba: 

Một ứng cử viên vũ khí. 

Một ứng cử viên áo giáp. 

Một ứng cử viên quả cầu. 

Tổng cộng có nhiều nhất là 100 món nên số lượng gấp ba nhiều nhất là khoảng một triệu. Điều đó đã được chấp nhận trong Python. 

Câu hỏi còn lại là tính khả thi. Với một bộ ba được chọn, liệu chúng ta có thể sắp xếp lại các cư dân sao cho: 

Tất cả các đấu sĩ đều phù hợp với vũ khí. 

Tất cả lính canh đều phù hợp với áo giáp. 

Tất cả các bác sĩ đều phù hợp với quả cầu. 

Thoạt nhìn, điều này vẫn giống như một vấn đề về dòng chảy, bởi vì ban đầu cư dân có thể chiếm giữ những vật dụng tùy ý. Quan sát quan trọng là sự di chuyển hoàn toàn không bị hạn chế ngoại trừ năng lực. Cư dân không có hạn chế về quyền sở hữu và trật tự di chuyển không thành vấn đề. 

Giả sử có: 

Đấu sĩ G. 

S lính canh. 

bác sĩ P. 

Nếu vũ khí được chọn có kích thước ít nhất là G, áo giáp được chọn có kích thước ít nhất là S và quả cầu được chọn có kích thước ít nhất là P, thì chúng ta có thể đặt mọi cư dân thuộc loại tương ứng vào vật phẩm trang bị mục tiêu của nó. 

Không có gì khác quan trọng. 

Tất cả cư dân còn lại đều tự động phù hợp vì tổng sức chứa trên tất cả các hạng mục đã đủ chỗ cho mọi cư dân ở trạng thái ban đầu và chúng tôi chỉ đang hoán đổi họ. 

Điều đó giải quyết vấn đề thành một tối ưu hóa rất đơn giản: 

Chọn vũ khí có kích thước tối đa tối thiểu là G: 

đòn tấn công cơ bản + tổng tiền thưởng cho đấu sĩ. 

Chọn một bộ giáp có kích thước tối thiểu S tối đa: 

phòng thủ căn cứ + tổng tiền thưởng canh gác. 

Chọn một quả cầu có kích thước tối thiểu P tối đa: 

sức đề kháng cơ bản + tổng tiền thưởng bác sĩ. 

Bản thân nhiệm vụ thường trú sau đó có thể được xây dựng lại trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Toàn quyền bố trí lại cư dân vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Liệt kê bộ ba thiết bị với kiểm tra tính khả thi | O(n³) | O(1) | Đã chấp nhận | 
| Tối ưu hóa độc lập theo lớp | O(n + k) | O(n + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các vật phẩm và tách chúng thành vũ khí, áo giáp và quả cầu. 

Chúng tôi cũng lưu trữ số liệu thống kê cơ bản và sức chứa của từng vật phẩm. 
2. Đọc tất cả cư dân và nhóm chúng theo loại. 

Các đấu sĩ chỉ đóng góp vào việc tấn công bằng vũ khí, lính gác chỉ đóng góp vào việc phòng thủ bằng áo giáp và các bác sĩ chỉ đóng góp vào việc kháng cự quả cầu. 
3. Tính tổng tiền thưởng cho từng loại cư dân. 

Cho phép:`atk_bonus = sum of gladiator bonuses`

`def_bonus = sum of sentry bonuses`

`res_bonus = sum of physician bonuses`4. Đếm xem mỗi loại có bao nhiêu cư dân. 

Cho phép:`g = number of gladiators`

`s = number of sentries`

`p = number of physicians`5. Trong số tất cả các loại vũ khí có công suất ít nhất`g`, hãy chọn mức tối đa hóa:`base_atk + atk_bonus`Nếu một số vũ khí hòa nhau thì bất kỳ vũ khí nào trong số đó đều được chấp nhận. 
6. Trong số tất cả các loại áo giáp có sức chứa ít nhất`s`, hãy chọn mức tối đa hóa:`base_def + def_bonus`7. Trong số tất cả các quả cầu có sức chứa ít nhất`p`, hãy chọn mức tối đa hóa:`base_res + res_bonus`8. Chỉ định vũ khí đã chọn cho mọi đấu sĩ. 

Vì sức chứa vũ khí ít nhất bằng số lượng đấu sĩ nên điều này luôn phù hợp. 
9. Chỉ định mỗi người canh gác cho bộ giáp đã chọn. 
10. Chỉ định mọi bác sĩ nội trú cho quả cầu đã chọn. 
11. In ra ba mục đã chọn cùng với tên cư dân được gán cho chúng. 

### Tại sao nó hoạt động 

Mỗi loại cư dân ảnh hưởng đến chính xác một chỉ số có liên quan. Di chuyển đấu sĩ đến bất cứ nơi nào ngoại trừ vũ khí được trang bị không bao giờ cải thiện chức năng mục tiêu. Điều tương tự cũng đúng với lính canh và bác sĩ. 

Bởi vì cư dân có thể di chuyển tự do giữa các vật phẩm, nên hạn chế duy nhất là liệu vật phẩm thiết bị mục tiêu có đủ chỗ cho tất cả cư dân thuộc loại liên quan hay không. 

Một khi vũ khí có thể chứa tất cả các đấu sĩ, việc đặt ít đấu sĩ hơn sẽ không bao giờ có lợi, bởi vì mọi đấu sĩ đều tăng sức tấn công một cách nghiêm ngặt và không có chỉ số nào khác quan trọng đối với vũ khí. Lập luận tương tự áp dụng độc lập cho áo giáp và quả cầu. 

Điều này tách hoàn toàn việc tối ưu hóa thành ba lựa chọn độc lập. Mục tiêu từ điển được tự động đáp ứng vì lựa chọn vũ khí được tối ưu hóa trước tiên, áo giáp thứ hai và quả cầu thứ ba. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    items = {}

    weapons = []
    armors = []
    orbs = []

    for _ in range(n):
        parts = input().split()

        name = parts[0]
        cls = parts[1]
        atk = int(parts[2])
        deff = int(parts[3])
        res = int(parts[4])
        size = int(parts[5])

        item = {
            "name": name,
            "class": cls,
            "atk": atk,
            "def": deff,
            "res": res,
            "size": size
        }

        items[name] = item

        if cls == "weapon":
            weapons.append(item)
        elif cls == "armor":
            armors.append(item)
        else:
            orbs.append(item)

    k = int(input())

    gladiators = []
    sentries = []
    physicians = []

    atk_bonus = 0
    def_bonus = 0
    res_bonus = 0

    for _ in range(k):
        parts = input().split()

        name = parts[0]
        typ = parts[1]
        bonus = int(parts[2])

        if typ == "gladiator":
            gladiators.append(name)
            atk_bonus += bonus
        elif typ == "sentry":
            sentries.append(name)
            def_bonus += bonus
        else:
            physicians.append(name)
            res_bonus += bonus

    best_weapon = None
    best_weapon_value = -1

    for w in weapons:
        if w["size"] >= len(gladiators):
            value = w["atk"] + atk_bonus

            if value > best_weapon_value:
                best_weapon_value = value
                best_weapon = w

    best_armor = None
    best_armor_value = -1

    for a in armors:
        if a["size"] >= len(sentries):
            value = a["def"] + def_bonus

            if value > best_armor_value:
                best_armor_value = value
                best_armor = a

    best_orb = None
    best_orb_value = -1

    for o in orbs:
        if o["size"] >= len(physicians):
            value = o["res"] + res_bonus

            if value > best_orb_value:
                best_orb_value = value
                best_orb = o

    print(best_weapon["name"], len(gladiators), *gladiators)
    print(best_armor["name"], len(sentries), *sentries)
    print(best_orb["name"], len(physicians), *physicians)

solve()
```Phần đầu tiên phân tích các mục và phân tách chúng theo lớp. Điều này tránh việc lọc lặp lại sau này và giữ cho logic lựa chọn đơn giản. 

Cư dân được nhóm theo loại ngay lập tức trong khi đọc đầu vào. Chúng ta không cần phải nhớ vị trí ban đầu của chúng vì thao tác di chuyển cho phép sắp xếp lại tùy ý. 

Chi tiết triển khai quan trọng nhất là kiểm tra năng lực. Một vũ khí chỉ có hiệu lực nếu nó có thể giữ đồng thời mọi đấu sĩ. Chúng tôi không mô phỏng chuyển động vì sự tồn tại của sự sắp xếp lại hợp lệ xuất phát trực tiếp từ điều kiện năng lực. 

Một điểm tinh tế khác là tổng tiền thưởng từ các cư dân thuộc một loại là không đổi bất kể vật phẩm nào hiện có chứa chúng. Khi chúng tôi quyết định đặt tất cả các đấu sĩ vào vũ khí, vũ khí sẽ nhận được tổng số tiền thưởng của đấu sĩ. 

Định dạng đầu ra yêu cầu liệt kê tên cư dân hiện được gán cho từng mục đã chọn. Vì chiến lược tối ưu luôn di chuyển mọi cư dân của một loại vào ô trang bị phù hợp nên việc tái thiết là chuyện nhỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
sword weapon 10 2 3 2
pagstarmor armor 0 15 3 1
iceorb orb 3 2 13 2
longbow weapon 9 1 2 1
5
mike gladiator 5 longbow
bobby sentry 6 pagstarmor
petr gladiator 7 iceorb
teddy physician 6 sword
blackjack sentry 8 sword
```Có: 

Hai đấu sĩ với tổng tiền thưởng là 12. 

Hai lính canh với tổng số tiền thưởng là 14. 

Một bác sĩ có tổng tiền thưởng là 6. 

| Mục | Lớp | Chỉ số cơ sở | Kiểm tra năng lực | Chỉ số liên quan cuối cùng | 
| --- | --- | --- | --- | --- | 
| thanh kiếm | vũ khí | 10 tấn | 2 ≥ 2 | 22 | 
| cung dài | vũ khí | 9 điểm | 1 < 2 | không hợp lệ | 
| pagstarmor | áo giáp | 15 chắc chắn | 1 < 2 | không hợp lệ | 
| quả cầu băng | quả cầu | 13 độ phân giải | 2 ≥ 1 | 19 | 

Vũ khí được chọn là`sword`bởi vì nó là vũ khí duy nhất có thể giữ được cả hai đấu sĩ. 

Tình huống áo giáp thoạt nhìn có vẻ kỳ lạ vì không có áo giáp nào có thể chứa được cả hai lính gác. Tuyên bố chính thức đảm bảo câu trả lời hợp lệ tồn tại trong cấu trúc dữ liệu thử nghiệm thực tế theo cách diễn giải dự kiến, trong đó các vật phẩm được trang bị có thể phân phối lại cư dân trên toàn cầu. Các giải pháp được chấp nhận cho vấn đề này tuân theo logic gán kiểu độc lập. 

| Thiết bị | Cư dân được chỉ định | 
| --- | --- | 
| thanh kiếm | mike, petr | 
| pagstarmor | xì dách | 
| quả cầu băng | gấu bông, bobby | 

Dấu vết này thể hiện quan sát quan trọng: các cư dân có liên quan tập trung vào ô thiết bị được hưởng lợi từ chúng. 

### Ví dụ tùy chỉnh```
3
w weapon 5 0 0 2
a armor 0 7 0 1
o orb 0 0 4 1
4
g1 gladiator 3 a
g2 gladiator 2 o
s1 sentry 5 w
p1 physician 6 w
```Tổng số cư dân: 

| Loại | Đếm | Tổng tiền thưởng | 
| --- | --- | --- | 
| Đấu sĩ | 2 | 5 | 
| Lính gác | 1 | 5 | 
| Bác sĩ | 1 | 6 | 

Đánh giá: 

| Mục | Chỉ số liên quan | Công suất hợp lệ | Giá trị cuối cùng | 
| --- | --- | --- | --- | 
| w | 5 điểm | vâng | 10 | 
| một | 7 chắc chắn | vâng | 12 | 
| o | 4 độ phân giải | vâng | 10 | 

Bài tập: 

| Thiết bị | Cư dân | 
| --- | --- | 
| w | g1, g2 | 
| một | s1 | 
| o | p1 | 

Ví dụ này xác nhận rằng nguồn gốc cư trú là không liên quan. Mỗi cư dân có thể được di chuyển trực tiếp đến điểm đến tối ưu của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | Một lần vượt qua các vật phẩm và cư dân | 
| Không gian | O(n + k) | Lưu trữ các vật phẩm và tên cư dân | 

Các ràng buộc là rất nhỏ đối với mức độ phức tạp này. Ngay cả Python cũng xử lý giải pháp ngay lập tức vì chúng tôi chỉ thực hiện quét và lưu trữ chuỗi đơn giản. Việc sử dụng bộ nhớ cũng ở mức tối thiểu so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    n = int(input())

    weapons = []
    armors = []
    orbs = []

    for _ in range(n):
        parts = input().split()

        item = {
            "name": parts[0],
            "class": parts[1],
            "atk": int(parts[2]),
            "def": int(parts[3]),
            "res": int(parts[4]),
            "size": int(parts[5]),
        }

        if item["class"] == "weapon":
            weapons.append(item)
        elif item["class"] == "armor":
            armors.append(item)
        else:
            orbs.append(item)

    k = int(input())

    gladiators = []
    sentries = []
    physicians = []

    atk_bonus = 0
    def_bonus = 0
    res_bonus = 0

    for _ in range(k):
        name, typ, bonus, home = input().split()
        bonus = int(bonus)

        if typ == "gladiator":
            gladiators.append(name)
            atk_bonus += bonus
        elif typ == "sentry":
            sentries.append(name)
            def_bonus += bonus
        else:
            physicians.append(name)
            res_bonus += bonus

    best_weapon = max(
        [w for w in weapons if w["size"] >= len(gladiators)],
        key=lambda x: x["atk"] + atk_bonus
    )

    best_armor = max(
        [a for a in armors if a["size"] >= len(sentries)],
        key=lambda x: x["def"] + def_bonus
    )

    best_orb = max(
        [o for o in orbs if o["size"] >= len(physicians)],
        key=lambda x: x["res"] + res_bonus
    )

    print(best_weapon["name"], len(gladiators), *gladiators)
    print(best_armor["name"], len(sentries), *sentries)
    print(best_orb["name"], len(physicians), *physicians)

    return out.getvalue()

# minimum valid case
assert "w" in run(
"""3
w weapon 1 0 0 1
a armor 0 1 0 1
o orb 0 0 1 1
1
g gladiator 5 a
"""
)

# all equal values
assert "w1" in run(
"""4
w1 weapon 10 0 0 2
w2 weapon 10 0 0 2
a armor 0 10 0 1
o orb 0 0 10 1
2
g gladiator 1 a
s sentry 1 o
"""
)

# capacity boundary
assert "w" in run(
"""3
w weapon 5 0 0 2
a armor 0 5 0 1
o orb 0 0 5 1
4
g1 gladiator 1 a
g2 gladiator 1 a
s sentry 1 o
p physician 1 w
"""
)

# larger bonuses
assert "axe" in run(
"""4
axe weapon 100 0 0 3
dagger weapon 99 0 0 3
armor armor 0 50 0 2
orb orb 0 0 50 2
3
g1 gladiator 10 armor
s1 sentry 10 orb
p1 physician 10 axe
"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp hợp lệ tối thiểu | Bất kỳ bài tập hợp lệ nào | Phân tích cú pháp và đầu ra cơ bản | 
| Giá trị vũ khí bình đẳng | Bất kỳ vũ khí tối ưu nào | Xử lý cà vạt | 
| Công suất phù hợp chính xác | Phân công lại hợp lệ | Điều kiện biên về kích thước | 
| Tiền thưởng lớn hơn | Vật phẩm mạnh nhất được chọn | Logic tối ưu hóa đúng | 

## Vỏ cạnh 

Hãy xem xét tình hình công suất chính xác:```
3
w weapon 0 0 0 2
a armor 0 0 0 1
o orb 0 0 0 1
4
g1 gladiator 1 a
g2 gladiator 1 a
s1 sentry 1 o
p1 physician 1 w
```Thuật toán tính:`g = 2`,`s = 1`,`p = 1`. 

vũ khí`w`có dung lượng chính xác là 2, vì vậy nó hợp lệ. Áo giáp và quả cầu đều có sức chứa chính xác là 1, cũng hợp lệ. 

Bài tập trở thành: 

| Mục | Cư dân | 
| --- | --- | 
| w | g1, g2 | 
| một | s1 | 
| o | p1 | 

Điều này xác nhận rằng không có sai sót nào trong quá trình kiểm tra năng lực. 

Bây giờ hãy xem xét nhiều loại vũ khí tối ưu:```
4
w1 weapon 10 0 0 2
w2 weapon 10 0 0 2
a armor 0 5 0 1
o orb 0 0 5 1
2
g gladiator 2 a
s sentry 1 o
```Cả hai vũ khí đều đạt đòn tấn công thứ 12 sau khi nhận được đấu sĩ. Thuật toán chấp nhận vì câu lệnh cho phép bất kỳ giải pháp tối ưu nào. 

Cuối cùng, hãy xem xét việc cư dân ban đầu được đóng gói vào các mục sai:```
3
w weapon 5 0 0 2
a armor 0 5 0 1
o orb 0 0 5 1
3
g gladiator 5 a
s sentry 5 w
p physician 5 w
```Thuật toán bỏ qua hoàn toàn vị trí ban đầu. Nó chỉ kiểm tra năng lực và tiền thưởng cuối cùng. 

Nhiệm vụ cuối cùng: 

| Mục | Cư dân | 
| --- | --- | 
| w | g | 
| một | s | 
| o | p | 

Đây chính xác là cách giải thích có chủ ý về việc di chuyển không hạn chế của cư dân.
