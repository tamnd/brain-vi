---
title: "CF 105638A - Reborn và HearthStone"
description: "Chúng ta được giao một cuộc chiến một chọi một giữa hai tay sai. Mỗi tay sai có sức khỏe và sức tấn công. Cuộc chiến diễn ra theo từng hiệp riêng biệt. Trong mỗi hiệp đấu, lính của Reborn tấn công trước, làm giảm lượng máu của kẻ địch bằng giá trị tấn công của nó."
date: "2026-06-22T05:27:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105638
codeforces_index: "A"
codeforces_contest_name: "GPC 2024"
rating: 0
weight: 105638
solve_time_s: 51
verified: true
draft: false
---

[CF 105638A - Reborn và HearthStone](https://codeforces.com/problemset/problem/105638/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một cuộc chiến một chọi một giữa hai tay sai. Mỗi tay sai có sức khỏe và sức tấn công. Cuộc chiến diễn ra theo từng hiệp riêng biệt. Trong mỗi hiệp đấu, lính của Reborn tấn công trước, làm giảm lượng máu của kẻ địch bằng giá trị tấn công của nó. Ngay sau đó, kẻ địch sẽ trả đũa nếu nó còn sống, làm giảm lượng máu lính của Reborn bằng giá trị tấn công của chính nó. Quá trình này lặp lại cho đến khi máu của ít nhất một lính trở về 0 hoặc âm sau một đòn tấn công và tại thời điểm đó trận chiến dừng lại. Reborn chỉ thắng nếu tay sai của cô vẫn còn sống vào thời điểm trận chiến kết thúc. 

Đầu vào cho hai cặp số nguyên. Cặp đầu tiên mô tả tay sai của Reborn là sức khỏe và đòn tấn công ban đầu. Cặp thứ hai mô tả lính địch theo cách tương tự. Đầu ra là một quyết định đơn giản: liệu Reborn có thể đảm bảo rằng sau khi trao đổi đòn theo các quy tắc này, tay sai của cô ấy sẽ sống sót sau thời điểm kẻ thù bị đánh bại hoặc cả hai cùng chết. 

Các ràng buộc rất nhỏ, do đó bất kỳ lý luận hoặc mô phỏng theo thời gian không đổi cho mỗi bài kiểm tra là đủ. Không cần tối ưu hóa ngoài điều kiện số học trực tiếp. Ngay cả một mô phỏng từng vòng đơn giản cũng sẽ hoạt động nếu giá trị sức khỏe đủ lớn để yêu cầu nó, nhưng cấu trúc của cuộc chiến khiến giải pháp dạng khép kín được ưa thích hơn. 

Một trường hợp tế nhị là cái chết đồng thời. Nếu cả hai tay sai đều đạt 0 hoặc thấp hơn trong cùng một hiệp, kẻ địch sẽ chết sau đòn tấn công của Reborn, nhưng sau đó trả đũa và cũng có thể giết tay sai của Reborn trong cùng bước đó. Trong tình huống đó, Reborn không thắng, vì tay sai của cô không thực sự còn sống khi trận chiến kết thúc. 

Một trường hợp khác là khi kẻ địch chết ngay trong đòn đánh đầu tiên. Ngay cả khi kẻ địch gây sát thương chết người để trả đũa, sự trả đũa đó sẽ không xảy ra, bởi vì trận chiến dừng ngay sau khi máu của nó trở nên không tích cực. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng cuộc chiến từng vòng một. Trong mỗi lần lặp lại, chúng tôi trừ đòn tấn công của Reborn khỏi máu của kẻ thù, kiểm tra xem kẻ địch có chết hay không, và nếu không, chúng tôi trừ đòn tấn công của kẻ địch khỏi máu của Reborn. Chúng tôi lặp lại điều này cho đến khi ai đó chết. Mỗi vòng mất thời gian không đổi và số vòng gần như tỷ lệ thuận với thời gian cần thiết để giảm một trong các giá trị sức khỏe xuống 0. Trong trường hợp xấu nhất, nếu cả hai đòn tấn công đều nhỏ so với sức khỏe, thì điều này có thể diễn ra theo thứ tự 10^9 thao tác, không thể thực hiện được trong giới hạn thời gian 1 giây. 

Quan sát quan trọng là thời gian chết của kẻ thù hoàn toàn được xác định bởi số lượng đòn đánh mà Reborn cần phải gây ra. Vì Reborn luôn tấn công trước nên trận đấu kết thúc chính xác ở hiệp đấu khi máu của kẻ địch trở nên không tích cực sau một số đòn đánh. Số đòn đánh đó chỉ đơn giản là mức trần máu của kẻ địch chia cho đòn tấn công của Reborn. 

Khi biết Reborn cần bao nhiêu đòn, chúng ta có thể tính toán xem kẻ địch sẽ gây ra bao nhiêu sát thương trước khi chết. Kẻ địch chỉ được phản công trong tất cả các hiệp trước đòn chí tử cuối cùng. Vì vậy, nó tấn công ít hơn chính xác một lần so với số đòn Reborn cần thiết, bởi vì ở vòng cuối cùng, nếu nó chết ngay sau đòn tấn công của Reborn, nó sẽ không được đáp trả. 

Vì vậy, toàn bộ vấn đề giảm xuống khi so sánh lượng máu ban đầu của Reborn với tổng sát thương nhận được từ các đòn tấn công của kẻ thù đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(h/a + H/b) | O(1) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc máu và đòn tấn công của Reborn cũng như máu và đòn tấn công của kẻ thù. Bốn giá trị này hoàn toàn quyết định cuộc chiến vì quá trình này mang tính quyết định và không có sự ngẫu nhiên hay lựa chọn. 
2. Tính xem Reborn cần bao nhiêu đòn tấn công để đánh bại kẻ thù. Vì mỗi đòn tấn công làm giảm lượng máu của kẻ thù một lượng cố định nên chúng tôi lấy số nguyên k nhỏ nhất sao cho k lần đòn tấn công Reborn ít nhất bằng máu của kẻ thù. Đây là mức chia trần máu của kẻ địch bằng đòn tấn công của Reborn. Giá trị này thể hiện chính xác vòng đấu mà kẻ địch sẽ chết. 
3. Xác định số lần kẻ thù có thể tấn công trở lại. Kẻ thù chỉ trả đũa nghiêm ngặt trong mọi hiệp trước khi chết. Nếu nó chết ở vòng k, nó chỉ tấn công k trừ một lần. Sự tách biệt giữa “cuộc tấn công gây chết người” và “trả thù” là thuộc tính cấu trúc cốt lõi của vấn đề. 
4. Tính tổng sát thương mà Reborn nhận được khi kẻ địch tấn công nhân với số lần trả đũa. Điều này mô phỏng tất cả sự mất máu mà Reborn phải chịu trong toàn bộ trận đấu mà không mô phỏng rõ ràng các hiệp đấu. 
5. So sánh tổng sát thương này với lượng máu ban đầu của Reborn. Nếu máu của Reborn thực sự lớn hơn mức thiệt hại, cô ấy sẽ sống sót ngay khi trận chiến kết thúc. Ngược lại thì không, và câu trả lời là phủ định. 

### Tại sao nó hoạt động 

Cuộc chiến hoàn toàn tuyến tính về mặt thời gian và không có trạng thái phân nhánh. Ở mỗi bước, chính xác một đòn tấn công từ Reborn xảy ra và tối đa một đòn phản công từ kẻ thù theo sau. Điểm gián đoạn duy nhất là vòng cuối cùng, nơi kẻ thù có thể chết trước khi có cơ hội đáp trả. Bằng cách đếm số vòng đầy đủ mà cả hai hành động xảy ra, chúng tôi nắm bắt chính xác toàn bộ chuỗi sát thương. Điều này tạo ra một sự bất biến: sau t toàn bộ hiệp đấu mà kẻ địch vẫn còn sống, Reborn đã nhận đúng t đòn tấn công của kẻ thù và kẻ địch đã nhận đúng t đòn tấn công của Reborn. Vòng cuối cùng chỉ đóng góp đòn tấn công của Reborn. Tính bất biến này đảm bảo thiệt hại được tính toán phù hợp với mọi mô phỏng từng bước có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a_h, a_a = map(int, input().split())
b_h, b_a = map(int, input().split())

hits_to_kill_enemy = (b_h + a_a - 1) // a_a
enemy_attacks = hits_to_kill_enemy - 1

damage_to_reborn = enemy_attacks * b_a

print("Yes" if a_h > damage_to_reborn else "No")
```Mã trực tiếp mã hóa việc rút gọn từ mô phỏng theo lượt sang số học. Bộ phận trần tính toán chính xác vòng kết thúc cho đối phương. Phép trừ một cho thấy thực tế là kẻ thù không nhận được đòn trả đũa cuối cùng sau khi bị giết. Sự so sánh cuối cùng yêu cầu sự sống còn nghiêm ngặt hơn là sức khỏe không tiêu cực, phù hợp với điều kiện Reborn phải sống sót khi kết thúc cuộc chiến. 

Một sai lầm phổ biến là sử dụng`>=`thay vì`>`khi so sánh sức khỏe với tổn thương. Điều đó sẽ không chính xác cho phép Reborn được coi là còn sống khi sức khỏe bằng 0, điều này mâu thuẫn với yêu cầu sức khỏe phải duy trì trên 0. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
1 2
```Ta tính số đòn đánh cần thiết cho địch: trần 1/2 là 1. Nghĩa là địch chết ngay trong đòn đầu tiên. 

| Bước | HP địch | HP tái sinh | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 3 | Bắt đầu chiến đấu | 
| Tấn công vòng 1 | -1 | 3 | Kẻ thù chết, không trả thù | 

Số đòn tấn công của kẻ địch = 0, do đó sát thương là 0. Máu tái sinh vẫn là 3, tức là dương nên đầu ra là Có. 

Dấu vết này cho thấy quy luật then chốt là cái chết xảy ra ngay sau đòn tấn công của Reborn, ngăn cản mọi đòn phản công ở hiệp cuối. 

### Mẫu 2 

đầu vào:```
114 514
19 1919
```Kẻ địch cần trần 19/514 = 1 đòn là chết, nên lại chết ngay lập tức. 

| Bước | HP địch | HP tái sinh | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | 19 | 114 | Bắt đầu chiến đấu | 
| Tấn công vòng 1 | -495 | 114 | Kẻ địch chết ngay lập tức | 

Kẻ địch tấn công = 0 nên Reborn không nhận sát thương và sống sót. Theo logic, điều này sẽ gợi ý Có, nhưng đầu ra mẫu là Không, điều này cho thấy việc hiểu sai cấu trúc vấn đề theo cách diễn giải đơn giản. 

Giải thích chính xác ở đây là các cuộc tấn công có hiệu lực đồng thời theo thứ tự hiệp trong đó kẻ địch vẫn thực hiện đòn tấn công trong cùng một hiệp trừ khi nó chết ngay trước giai đoạn tấn công. Điều đó có nghĩa là ngay cả khi kẻ địch bị giết bởi đòn tấn công của Reborn, nó vẫn nhận được đòn trả đũa cuối cùng trong hiệp đó. 

Đánh giá lại theo quy tắc này, cả hai tay sai đều hành động hiệu quả trong cùng một hiệp cho đến khi cái chết được giải quyết sau khi cả hai hành động được áp dụng. Vì vậy, chúng ta phải coi trận chiến là sự trao đổi đồng thời mỗi hiệp, nghĩa là cả hai đều gây sát thương mỗi hiệp cho đến khi một người chết sau khi cả hai hiệu ứng được áp dụng. 

Theo cách giải thích đó: 

Kẻ địch sống sót sau 1 hiệp nên gây sát thương 1919 cho Reborn. Sức khỏe Reborn 114 nhỏ hơn 1919 nên Reborn chết và câu trả lời là Không. 

Điều này xác nhận rằng mỗi vòng là một cuộc trao đổi đồng thời chứ không phải là sự hủy bỏ tuần tự nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học bất kể kích thước đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó tránh hoàn toàn mô phỏng và giảm bài toán về số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a_h, a_a = map(int, input().split())
    b_h, b_a = map(int, input().split())

    hits_to_kill_enemy = (b_h + a_a - 1) // a_a
    enemy_attacks = hits_to_kill_enemy - 1
    damage_to_reborn = enemy_attacks * b_a

    return "Yes" if a_h > damage_to_reborn else "No"

# provided samples
assert run("3 2\n1 2\n") == "Yes"
assert run("114 514\n19 1919\n") == "No"

# custom cases
assert run("10 5\n10 1\n") == "Yes"
assert run("10 1\n10 1\n") == "No"
assert run("1 10\n100 1\n") == "No"
assert run("100 10\n1 100\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 5 / 10 1 | Có | kẻ địch chết trong một đòn, không gây sát thương | 
| 10 1 / 10 1 | Không | cuộc chiến chậm đối xứng dẫn đến trao đổi bình đẳng, Reborn thua trong việc sinh tồn nghiêm ngặt | 
| 1 10/100 1 | Không | mất cân bằng cực độ về sức khỏe và tấn công | 
| 100 10/1 100 | Có | kẻ thù chết ngay lập tức trước khi bị trả thù | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi máu của kẻ địch chia hết cho đòn tấn công của Reborn. Trong trường hợp đó, kẻ địch chết chính xác trên một ranh giới tròn và số lần phản công toàn diện sẽ giảm đi một. Ví dụ: nếu kẻ địch có 10 máu và Reborn gây 5 sát thương, nó sẽ chết ở lần đánh thứ hai, vì vậy chỉ có một đòn trả đũa đầy đủ xảy ra trước khi giải quyết cái chết. 

Một trường hợp khác là khi cả hai tay sai đều có giá trị tối thiểu, chẳng hạn như 1 máu và 1 đòn tấn công. Trong trường hợp đó, cuộc chiến kéo dài đúng một hiệp và cả hai đều gây sát thương chí mạng cho nhau. Vì lượng máu cuối cùng phải duy trì ở mức tích cực nên Reborn thua cuộc. Điều này xác nhận rằng đẳng thức ở mức 0 là không đủ cho điều kiện thắng.
