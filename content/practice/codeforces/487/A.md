---
title: "CF 487A - Chiến đấu với quái vật"
description: "Chúng ta được cung cấp số liệu thống kê chiến đấu ban đầu của hai nhân vật, Master Yang và một con quái vật. Mỗi nhân vật đều có điểm nhấn, đòn tấn công và phòng thủ. Trận chiến diễn ra trong vài giây riêng biệt."
date: "2026-06-07T17:31:28+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 487
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 278 (Div. 1)"
rating: 1800
weight: 487
solve_time_s: 126
verified: true
draft: false
---

[CF 487A - Chiến đấu với quái vật](https://codeforces.com/problemset/problem/487/A) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, vũ lực, thực hiện 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp số liệu thống kê chiến đấu ban đầu của hai nhân vật, Master Yang và một con quái vật. Mỗi nhân vật đều có điểm nhấn, đòn tấn công và phòng thủ. 

Trận chiến diễn ra trong vài giây riêng biệt. Trong mỗi giây, Yang gây sát thương$$\max(0,\ ATK_Y - DEF_M)$$gây sát thương cho quái vật, trong khi quái vật đó đồng thời gây sát thương$$\max(0,\ ATK_M - DEF_Y)$$gây thiệt hại cho Dương. 

Yang chỉ thắng nếu HP của quái vật trở nên không dương trong khi Yang vẫn có HP hoàn toàn dương tại thời điểm đó. 

Trước khi trận chiến bắt đầu, Yang có thể tăng bất kỳ thuộc tính nào trong ba thuộc tính của mình bằng cách trả bitcoin. HP, ATK và DEF đều có chi phí riêng cho mỗi đơn vị. Chúng ta phải tìm ra tổng số tiền tối thiểu cần thiết để Yang có thể đảm bảo chiến thắng. 

Những hạn chế là rất nhỏ. Mỗi giá trị đầu vào tối đa là 100. Vì các thuộc tính đã mua cũng có thể nằm trong một phạm vi tương đối nhỏ nên việc tìm kiếm mạnh mẽ về các nâng cấp tấn công và phòng thủ có thể có là khả thi. Thử thách không phải là sự phức tạp về mặt tính toán mà là mô hình hóa trận chiến một cách chính xác và xử lý tất cả các trường hợp góc cạnh. 

Một điểm tế nhị là Yang phải thực sự có khả năng gây sát thương cho quái vật. Nếu như$$ATK_Y \le DEF_M$$thì quái vật không bao giờ mất HP, bất kể Yang mua bao nhiêu máu. 

Coi như:```
1 1 1
1 5 5
1 1 1
```Đòn tấn công của Yang là 1 và khả năng phòng thủ của quái vật là 5. Chỉ mua HP thôi thì không bao giờ có thể giúp được. Bất kỳ giải pháp nào chỉ kiểm tra khả năng sống sót mà không đảm bảo sát thương tích cực sẽ nghĩ sai rằng HP đủ đảm bảo chiến thắng. 

Một sai lầm dễ mắc phải khác là quên rằng Yang phải còn sống sau lượt giết chóc. Giả sử Yang giết quái vật trong 5 lượt và cũng nhận sát thương chí mạng trong cùng 5 lượt đó. Vì sát thương xảy ra đồng thời nên cuối cùng Yang cần có HP dương. 

Ví dụ:```
5 10 0
50 10 0
1 1 1
```Yang gây 10 sát thương mỗi lượt và cần đúng 5 lượt. Quái vật này cũng gây 10 sát thương mỗi lượt, khiến Yang mất hết 50 HP sau 5 lượt. Đây không phải là một chiến thắng vì HP của Yang không hoàn toàn tích cực. 

Yêu cầu đúng là$$HP_Y > (\text{turns needed}) \times (\text{damage received per turn})$$không lớn hơn hoặc bằng. 

Trường hợp góc thứ ba xảy ra khi khả năng phòng thủ của Yang đã đủ cao để quái vật không gây sát thương. Sau đó, bất kỳ HP dương nào cũng đủ khi Yang cuối cùng có thể giết được quái vật. 

Ví dụ:```
1 2 100
100 2 1
5 5 5
```Con quái vật không thể làm tổn thương Yang chút nào. Chúng ta chỉ cần tấn công đủ để đánh bại quái vật. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là thử mọi lượng HP, đòn tấn công và phòng thủ có thể mà Yang có thể mua, mô phỏng trận chiến và giữ cấu hình chiến thắng rẻ nhất. 

Điều này đúng vì mọi sự kết hợp nâng cấp có thể đều được kiểm tra. Vấn đề là quyết định không gian tìm kiếm phải lớn đến mức nào. Nếu chúng ta lặp lại HP, ATK và DEF một cách mù quáng trong một phạm vi lớn, số lượng trạng thái sẽ tăng theo khối. 

Nhận xét quan trọng là HP hoạt động khác với tấn công và phòng thủ. 

Giả sử chúng ta quyết định giá trị tấn công và phòng thủ cuối cùng. Khi đó kết quả trận chiến đã được định đoạt hoàn toàn ngoại trừ HP. Trong số tất cả các giá trị HP, có một mức HP tối thiểu duy nhất cần thiết để tồn tại. Mua nhiều HP hơn mức đó không bao giờ có lợi vì nó chỉ làm tăng chi phí. 

Điều này có nghĩa là chúng ta không cần liệt kê HP gì cả. 

Thay vào đó, chúng tôi liệt kê các giá trị tấn công và phòng thủ cuối cùng. Với mỗi cặp ta tính: 

1. Sát thương Dương gây ra mỗi lượt. 
2. Sát thương Yang nhận mỗi lượt. 
3. Số lượt cần thiết để tiêu diệt quái vật. 
4. HP tối thiểu cần thiết để sống sót trong lượt đó. 

Sau khi biết được lượng HP yêu cầu, chi phí của cấu hình đó sẽ được tính trực tiếp. 

Vì tất cả các giá trị ban đầu tối đa là 100 nên việc kiểm tra các giá trị tấn công và phòng thủ lên tới vài trăm dễ dàng bao quát mọi giải pháp tối ưu. Các giải pháp được chấp nhận theo truyền thống lặp lại cả hai chiều lên tới 200. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về HP, ATK, DEF | O(K³) | O(1) | Lớn không cần thiết | 
| Đếm ATK và DEF, tính HP trực tiếp | O(K2) | O(1) | Đã chấp nhận | 

Đây$K$là giới hạn trên được chọn để khám phá tấn công và phòng thủ, thường là khoảng 200. 

## Hướng dẫn thuật toán 

1. Đọc số liệu thống kê của Yang và quái vật, cùng với ba chi phí nâng cấp. 
2. Liệt kê mọi giá trị tấn công cuối cùng có thể có từ cuộc tấn công hiện tại cho đến giới hạn trên an toàn, chẳng hạn như 200. 
3. Liệt kê mọi giá trị phòng thủ cuối cùng có thể có từ phòng thủ hiện tại cho đến cùng giới hạn trên. 
4. Tính toán sát thương của Yang mỗi lượt.$$dmg_Y = ATK - DEF_M$$Nếu giá trị này không dương, hãy bỏ qua cấu hình vì quái vật không bao giờ có thể bị giết. 
5. Tính số lượt cần thiết để đánh bại quái vật.$$turns = \left\lceil \frac{HP_M}{dmg_Y} \right\rceil$$6. Tính toán sát thương của quái vật mỗi lượt.$$dmg_M = \max(0,\ ATK_M - DEF)$$7. Xác định lượng HP tối thiểu mà Yang phải có. 

Yang nhận sát thương chính xác`turns`các vòng đấu và phải sống sót sau đó.$$HP_{required} = turns \times dmg_M + 1$$Nếu Yang đã có ít nhất lượng HP này thì không cần mua HP. Nếu không hãy mua sự khác biệt. 
8. Tính tổng chi phí của tất cả các lần nâng cấp:$$(\text{extra HP}) \cdot h + (\text{extra ATK}) \cdot a + (\text{extra DEF}) \cdot d$$9. Giữ chi phí tối thiểu trong số tất cả các cấu hình hợp lệ. 
10. Xuất ra chi phí tối thiểu tìm được. 

### Tại sao nó hoạt động 

Đối với bất kỳ giá trị tấn công và phòng thủ cuối cùng nào được chọn, sẽ có giá trị HP nhỏ nhất cho phép chiến thắng. Bất kỳ HP lớn hơn nào cũng chỉ làm tăng chi phí và không bao giờ cải thiện kết quả. Do đó, mọi giải pháp tối ưu được biểu diễn chính xác bằng một trạng thái trong tìm kiếm của chúng ta. 

Thuật toán kiểm tra mọi đòn tấn công và phòng thủ cuối cùng có thể là một phần của câu trả lời tối ưu. Đối với mỗi cặp, nó tính toán lượng HP tối thiểu cần thiết để tồn tại cho đến khi quái vật chết. Chi phí thu được là chi phí rẻ nhất có thể có cho cặp tấn công-phòng thủ đó. 

Vì mọi cấu hình tối ưu khả thi đều xuất hiện ở đâu đó trong bảng liệt kê và chúng tôi lấy chi phí tối thiểu cho tất cả chúng nên câu trả lời cuối cùng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    hy, ay, dy = map(int, input().split())
    hm, am, dm = map(int, input().split())
    h_cost, a_cost, d_cost = map(int, input().split())

    INF = 10**18
    ans = INF

    for atk in range(ay, 201):
        damage_to_monster = atk - dm
        if damage_to_monster <= 0:
            continue

        turns = (hm + damage_to_monster - 1) // damage_to_monster

        for defense in range(dy, 201):
            damage_to_yang = max(0, am - defense)

            required_hp = turns * damage_to_yang + 1
            extra_hp = max(0, required_hp - hy)

            cost = (
                extra_hp * h_cost
                + (atk - ay) * a_cost
                + (defense - dy) * d_cost
            )

            ans = min(ans, cost)

    print(ans)

solve()
```Các vòng bên ngoài liệt kê tất cả các giá trị tấn công và phòng thủ cuối cùng của ứng cử viên. Đối với mỗi giá trị tấn công, chúng tôi sẽ loại bỏ ngay các trường hợp Yang không thể gây sát thương lên quái vật. 

Bộ phận trần```
(hm + damage_to_monster - 1) // damage_to_monster
```tính toán chính xác số vòng chiến đấu cần thiết để tiêu diệt quái vật. 

Điều kiện sinh tồn là phần tế nhị nhất. Yang phải có HP hoàn toàn dương sau khi gây sát thương đồng thời cuối cùng. Nếu anh ta nhận được`turns * damage_to_yang`tổng thiệt hại thì HP chiến thắng nhỏ nhất sẽ lớn hơn số đó một HP. sử dụng`>=`ở đây sẽ tạo ra câu trả lời sai trong trường hợp Yang đạt chính xác HP bằng 0. 

Giới hạn trên được chọn là 200 là đủ. Tất cả số liệu thống kê ban đầu tối đa là 100. Tăng tấn công hoặc phòng thủ trên 200 không bao giờ có ích vì đòn tấn công đã vượt quá khả năng phòng thủ của quái vật ít nhất là 100 và khả năng phòng thủ đã vô hiệu hóa mọi đòn tấn công có thể có của quái vật. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 2 1
1 100 1
1 100 100
```Chiến lược rẻ nhất là chỉ mua HP. 

| atk | chắc chắn | dmg_Y | lượt | dmg_M | bắt buộc_hp | thêm_hp | chi phí | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | 99 | 100 | 99 | 99 | 
| 3 | 1 | 2 | 1 | 99 | 100 | 99 | 199 | 
| 2 | 2 | 1 | 1 | 98 | 99 | 98 | 9899 | 

Chi phí tối thiểu là 99. 

Dấu vết này cho thấy tại sao việc nâng cấp tấn công và phòng thủ lại không hấp dẫn khi giá của chúng cực kỳ cao. Chỉ mua sức khỏe thôi đã tạo nên một cấu hình chiến thắng. 

### Mẫu 2 

Hãy xem xét:```
100 100 100
1 1 1
1 1 1
```Yang đã mạnh hơn rất nhiều. 

| atk | chắc chắn | dmg_Y | lượt | dmg_M | bắt buộc_hp | thêm_hp | chi phí | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 100 | 100 | 99 | 1 | 0 | 1 | 0 | 0 | 

Câu trả lời là 0. 

Ví dụ này chứng tỏ rằng thuật toán xử lý một cách tự nhiên các tình huống không cần mua hàng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(200²) | Liệt kê tất cả các cặp tấn công-phòng thủ | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Việc tìm kiếm kiểm tra tối đa 40.000 cấu hình. Mỗi cấu hình chỉ yêu cầu số học thời gian không đổi. Điều này thấp hơn nhiều so với giới hạn và chạy thoải mái trong vòng một giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    hy, ay, dy = map(int, input().split())
    hm, am, dm = map(int, input().split())
    h_cost, a_cost, d_cost = map(int, input().split())

    ans = 10**18

    for atk in range(ay, 201):
        dmg_y = atk - dm
        if dmg_y <= 0:
            continue

        turns = (hm + dmg_y - 1) // dmg_y

        for defense in range(dy, 201):
            dmg_m = max(0, am - defense)

            req_hp = turns * dmg_m + 1
            extra_hp = max(0, req_hp - hy)

            cost = (
                extra_hp * h_cost
                + (atk - ay) * a_cost
                + (defense - dy) * d_cost
            )

            ans = min(ans, cost)

    return str(ans)

# provided sample
assert run(
"""1 2 1
1 100 1
1 100 100
"""
) == "99"

# already winning
assert run(
"""100 100 100
1 1 1
1 1 1
"""
) == "0"

# need one attack upgrade to deal damage
assert run(
"""1 1 1
1 1 1
10 1 10
"""
) == "1"

# all values equal
assert run(
"""10 10 10
10 10 10
1 1 1
"""
) == "1"

# defense can completely negate damage
assert run(
"""1 2 1
100 100 1
10 10 1
"""
) == "99"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yang đã mạnh hơn quái vật | 0 | Không cần nâng cấp | 
| Tấn công bằng phòng thủ quái vật | 1 | Cần có sát thương tích cực | 
| Tất cả các giá trị bằng nhau | 1 | Xử lý đúng thiệt hại đồng thời | 
| Nâng cấp quốc phòng giá rẻ | 99 | Giảm sát thương nhận vào có thể là tối ưu | 

## Vỏ cạnh 

### Yang không thể gây sát thương lên quái vật 

đầu vào:```
1 1 1
10 5 5
1 1 1
```Với đòn tấn công 1 và phòng thủ quái vật 5, Yang không gây sát thương. Thuật toán bỏ qua mọi cấu hình mà đòn tấn công cuối cùng vẫn thỏa mãn`atk <= 5`. Trạng thái hợp lệ đầu tiên là`atk = 6`, gây một sát thương mỗi lượt. Điều này ngăn chặn việc coi các trận chiến vô tận là chiến thắng một cách không chính xác. 

### Yang đạt HP chính xác bằng 0 

đầu vào:```
50 10 0
50 10 0
1 1 1
```Yang gây 10 sát thương và giết quái trong 5 lượt. Anh ta cũng nhận 10 sát thương mỗi lượt. 

Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| lượt | 5 | 
| dmg_M | 10 | 
| bắt buộc_hp | 51 | 

Từ`5 * 10 = 50`, có chính xác 50 HP là không đủ. phần bổ sung`+1`thực thi yêu cầu Yang phải còn sống sau vòng cuối cùng. 

### Quái vật không gây sát thương 

đầu vào:```
1 2 100
100 2 1
5 5 5
```Đối với phòng thủ 100: 

| Biến | Giá trị | 
| --- | --- | 
| dmg_M | 0 | 
| bắt buộc_hp | 1 | 
| thêm_hp | 0 | 

Thuật toán nhận dạng chính xác rằng không cần nâng cấp HP. Chỉ có khả năng cuối cùng tiêu diệt được quái vật mới quan trọng.
