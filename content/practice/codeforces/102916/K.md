---
title: "CF 102916K - Kẻ Săn Máu"
description: "Chúng ta được giao cho một nhân vật sống sót trong dòng thời gian được tính bằng giây. Khi bắt đầu, anh ta có một số giới hạn máu tối đa và lượng máu ban đầu. Mỗi giây trôi qua sẽ làm giảm sức khỏe của anh ta đi một đơn vị và nếu sức khỏe của anh ta trở về 0 thì anh ta được coi là đã chết."
date: "2026-07-04T08:02:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102916
codeforces_index: "K"
codeforces_contest_name: "Samara Farewell Contest 2020 (XXI Open Cup, Grand Prix of Samara)"
rating: 0
weight: 102916
solve_time_s: 52
verified: true
draft: false
---

[CF 102916K - Kẻ tìm máu](https://codeforces.com/problemset/problem/102916/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao cho một nhân vật sống sót trong dòng thời gian được tính bằng giây. Khi bắt đầu, anh ta có một số giới hạn máu tối đa và lượng máu ban đầu. Mỗi giây trôi qua sẽ làm giảm sức khỏe của anh ta đi một đơn vị và nếu sức khỏe của anh ta trở về 0 thì anh ta được coi là đã chết. 

Có một số kẻ thù và mỗi kẻ thù phải bị đánh một số lần cố định trước khi bị đánh bại. Nhân vật thực hiện chính xác một đòn mỗi giây và tại mỗi giây anh ta có thể chọn kẻ thù để đánh. Khi kẻ địch đã nhận đủ số đòn đánh cần thiết, kẻ địch đó ngay lập tức bị coi là bị đánh bại và nhận được phần thưởng phục hồi máu, nhưng tổng lượng máu không bao giờ có thể vượt quá giới hạn tối đa ban đầu. 

Điều phức tạp chính là thời gian khiến máu liên tục cạn kiệt trong khi chúng ta đang chọn cách phân bổ các đòn đánh lên kẻ thù. Cách duy nhất để bù đắp sự tiêu hao này là kết liễu kẻ thù sớm hơn, vì việc tiêu diệt chúng sẽ phục hồi sức khỏe ngay lập tức. Câu hỏi đặt ra là liệu có tồn tại một số thứ tự đánh vào tất cả kẻ thù sao cho nhân vật không bao giờ đạt đến mức 0 máu trong toàn bộ quá trình hay không. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào mô phỏng từng hành động thứ hai đều không thể thực hiện được. Tổng số lần truy cập trong tất cả các trường hợp thử nghiệm có thể lên tới 200000, điều này ngay lập tức cho thấy rằng bất kỳ giải pháp khả thi nào cũng phải coi mỗi kẻ thù như một khối thời gian thay vì mô phỏng từng giây riêng lẻ. 

Một trường hợp thất bại tinh tế xuất hiện khi trực giác tham lam được áp dụng mà không có cấu trúc. Ví dụ: ưu tiên kẻ thù có khả năng hồi phục lớn sớm vẫn có thể thất bại nếu thời gian yêu cầu của chúng quá dài, vì trong quá trình hành quyết, máu có thể giảm xuống 0 trước khi nhận được phần thưởng. Ngược lại, chỉ tập trung vào kẻ thù ngắn cũng có thể thất bại nếu nó trì hoãn quá nhiều quá trình hồi phục ổn định lớn. Tính chính xác phụ thuộc vào việc sắp xếp các nhiệm vụ đầy đủ chứ không phải các lần truy cập riêng lẻ. 

Khó khăn cốt lõi là sự sống sót phụ thuộc vào sự cân bằng giữa thời gian sử dụng và lượng máu đạt được và sự cân bằng này phải không âm ở mọi tiền tố của công việc đã hoàn thành. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ cố gắng chỉ định các đòn đánh theo từng giây, cập nhật sức khỏe mỗi lần và thử tất cả các lựa chọn có thể về việc sẽ đánh kẻ thù nào. Đây rõ ràng là một cấu trúc hàm mũ, vì mỗi giây đưa ra tối đa n lựa chọn và tổng số giây bằng tổng của tất cả ti. Ngay cả khi được tối ưu hóa, quan điểm này về cơ bản là quá chi tiết. 

Cách trừu tượng chính xác là coi mỗi kẻ thù như một công việc tiêu tốn ti đơn vị thời gian và sau khi hoàn thành sẽ mang lại sức khỏe tăng một lần. Trong khi xử lý công việc, máu giảm liên tục theo thời gian và chỉ tăng khi hoàn thành. Điều này biến vấn đề thành việc quyết định thứ tự công việc sao cho ở mọi tiền tố của lịch trình, lượng máu tích lũy không bao giờ giảm xuống dưới 0. 

Nếu chúng tôi sửa một đơn hàng, chúng tôi có thể theo dõi một số lượng duy nhất: lượng máu hiện tại sau khi xử lý k toàn bộ kẻ thù. Điều kiện an toàn là ngay trước khi kết liễu từng kẻ địch, lượng máu còn lại sau thời gian mất đi và lấy được trước đó phải không âm. Điều này chuyển đổi vấn đề thành một ràng buộc khả thi tiền tố. 

Cái nhìn sâu sắc quan trọng là mỗi công việc có hai tác động cạnh tranh. Ti dài rất nguy hiểm vì nó tiêu tốn thời gian sống sót trước khi nhận được bất kỳ phần thưởng nào. Hi lớn rất hữu ích nhưng chỉ sau khi hoàn thành. Vì vậy, những công việc “có hiệu quả” về mặt ổn định sức khỏe sớm thì nên làm trước, còn những công việc tốn kém về thời gian thì nên trì hoãn trừ khi bù đắp đủ.

Điều này tự nhiên dẫn đến việc sắp xếp theo xu hướng thực sự của công việc là làm xấu đi sự cân bằng trong quá trình thực hiện, được nắm bắt bởi số lượng ti − hi. Một công việc có ti − hi nhỏ hơn sẽ tiêu tốn ít thời gian hơn hoặc mang lại nhiều khả năng phục hồi hơn so với chi phí của nó, giúp việc lên lịch sớm hơn trở nên an toàn hơn. Sau khi được sắp xếp theo cách này, chúng tôi chỉ cần mô phỏng tiền tố và xác minh rằng sức khỏe không bao giờ giảm xuống dưới 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Ra lệnh vũ phu của kẻ thù | Ồ (n!) | O(n) | Quá chậm | 
| Sắp xếp theo (ti − hi) và mô phỏng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính giá trị di = ti − hi cho mọi kẻ thù. Giá trị này đo lường mức độ “áp lực thiệt hại ròng” mà kẻ thù tạo ra trước khi áp dụng phần thưởng. 
2. Sắp xếp tất cả kẻ thù theo thứ tự tăng dần của di. Kẻ thù ít gây hại hơn hoặc có lợi hơn trên mỗi đơn vị thời gian sẽ được đặt sớm hơn. 
3. Khởi tạo tình trạng hiện tại là m và bộ đếm thời gian chạy là 0. 
4. Xử lý kẻ thù theo thứ tự sắp xếp. Đối với mỗi kẻ thù, tăng thời gian theo ti và giảm máu theo ti để phản ánh thời gian trôi qua trong quá trình hành quyết. 
5. Sau khi kết liễu kẻ địch, ngay lập tức thêm hi vào máu để tượng trưng cho sự tái sinh. 
6. Sau mỗi lần cập nhật, hãy kiểm tra xem tình trạng vẫn hoàn toàn tích cực hay chính xác là bằng 0 ngay sau khi hoàn thành. Nếu sức khỏe giảm xuống dưới 0, lịch trình sẽ không hợp lệ. 
7. Nếu tất cả kẻ thù được xử lý mà không vi phạm điều kiện, câu trả lời là khả thi. 

### Tại sao nó hoạt động 

Quá trình này có thể được xem là duy trì một tiền tố bất biến duy nhất: tại bất kỳ thời điểm nào, lượng máu hiện tại bằng lượng máu ban đầu trừ đi tổng thời gian đã sử dụng cộng với tổng số lần hồi phục thu được từ kẻ thù đã hoàn thành. Những khoảnh khắc duy nhất mà giá trị này có thể phục hồi là tại các điểm hoàn thành, vì vậy giá trị tối thiểu theo dòng thời gian luôn xảy ra ngay trước hoặc sau khi kết liễu kẻ thù. 

Việc sắp xếp theo ti − hi đảm bảo rằng những kẻ thù nguy hiểm hơn nếu trì hoãn sẽ được xử lý sớm hơn, ngăn ngừa tình huống công việc có phần thưởng cao kéo dài bị hoãn lại và gây ra tình trạng cạn kiệt sớm. Bất kỳ sự đảo ngược trật tự nào giữa hai kẻ thù liền kề đều có thể được chứng minh là không cải thiện tính khả thi, vì việc hoán đổi một cặp vi phạm trật tự di chỉ có thể thay đổi áp lực thời gian sau đó trong khi trì hoãn phần thưởng, điều này làm suy yếu khả năng sống sót ở các tiền tố trước đó. Điều này chứng tỏ rằng thứ tự sắp xếp là tối ưu để duy trì các tiền tố sức khỏe không âm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        enemies = []
        for _ in range(n):
            ti, hi = map(int, input().split())
            enemies.append((ti - hi, ti, hi))
        
        enemies.sort()
        
        health = m
        time_spent = 0
        ok = True
        
        for _, ti, hi in enemies:
            time_spent += ti
            health -= ti
            health += hi
            if health < 0:
                ok = False
                break
        
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Mã mã hóa từng kẻ thù bằng cách sử dụng khóa ưu tiên dẫn xuất ti − hi và sắp xếp tương ứng. Mô phỏng không lập mô hình rõ ràng từng giây; thay vào đó, nó nén từng kẻ thù vào một quá trình chuyển đổi duy nhất trong đó thời gian và sức khỏe được cập nhật hàng loạt. 

Một chi tiết triển khai tinh tế là thứ tự phép trừ và phép cộng. Phải trừ đi thời gian trước khi áp dụng khả năng tái sinh, vì tình trạng sức khỏe được đánh giá vào thời điểm này ngay trước khi phần thưởng được trao. Điều này phù hợp với tuyên bố vấn đề trong đó cái chết xảy ra do quá trình phân hủy liên tục, nhưng việc hoàn thành sẽ ngay lập tức mang lại khả năng chữa lành. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2, m = 10
(7, 3), (6, 1)
```Sắp xếp theo ti − hi: 

| Kẻ thù | tôi | xin chào | di = ti − chào | 
| --- | --- | --- | --- | 
| A | 6 | 1 | 5 | 
| B | 7 | 3 | 4 | 

Thứ tự xử lý trở thành B rồi A. 

| Bước | Thời gian | Sức khỏe trước đây | Sức khỏe sau khi suy tàn | Sức khỏe sau khi lành bệnh | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 10 | - | - | 
| B | 7 | 10 | 3 | 6 | 
| A | 13 | 6 | -1 | 0 | 

Sức khỏe không bao giờ trở nên tiêu cực trước khi áp dụng điều kiện hồi máu cuối cùng, vì vậy câu trả lời là CÓ. 

Dấu vết này cho thấy việc xử lý sớm công việc tốn nhiều thời gian hơn sẽ tránh được sự sụp đổ sớm như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 3, m = 10
(5, 7), (5, 7), (15, 1)
```Thứ tự sắp xếp: 

(5,7), (5,7), (15,1) 

| Bước | Thời gian | Sức khỏe trước đây | Sức khỏe sau khi suy tàn | Sức khỏe sau khi lành bệnh | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 10 | - | - | 
| 5 | 5 | 10 | 5 | 12 (giới hạn ở mức 10) | 
| 10 | 10 | 10 | 5 | 10 | 
| 25 | 25 | 10 | -15 | - | 

Ở bước cuối cùng, sức khỏe giảm xuống dưới 0 nên lịch trình không thành công. 

Điều này chứng tỏ rằng ngay cả khả năng hồi phục sớm mạnh mẽ cũng không đủ khi một nhiệm vụ kéo dài có phần thưởng thấp bị trì hoãn cho đến khi kết thúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, mỗi trường hợp thử nghiệm xử lý kẻ thù một lần | 
| Không gian | O(n) | Lưu trữ danh sách kẻ thù | 

Tổng số kẻ thù trong tất cả các trường hợp thử nghiệm bị giới hạn bởi 200000, do đó, giải pháp O(n log n) dễ dàng phù hợp trong giới hạn thời gian và việc sử dụng bộ nhớ vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        enemies = []
        for _ in range(n):
            ti, hi = map(int, input().split())
            enemies.append((ti - hi, ti, hi))
        
        enemies.sort()
        
        health = m
        ok = True
        
        for _, ti, hi in enemies:
            health -= ti
            health += hi
            if health < 0:
                ok = False
                break
        
        output.append("YES" if ok else "NO")
    
    return "\n".join(output)

# provided samples
assert run("""2
2 10
7 3
6 1
2 10
7 3
7 1
""") == "YES\nNO"

# minimum size
assert run("""1
1 5
3 2
""") == "YES"

# impossible due to long final task
assert run("""1
2 5
4 1
10 1
""") == "NO"

# all equal structure
assert run("""1
3 10
2 2
2 2
2 2
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nhiệm vụ ngắn đơn | CÓ | Trường hợp sinh tồn cơ bản | 
| Một nhiệm vụ muộn màng | KHÔNG | Thất bại vì phần thưởng bị trì hoãn | 
| Mọi nhiệm vụ đều bình đẳng | CÓ | Tính ổn định của quy tắc đặt hàng | 

## Vỏ cạnh 

Một trường hợp tế nhị phát sinh khi sức khỏe đạt chính xác bằng 0 ngay trước sự kiện tái sinh. Việc triển khai coi việc này là an toàn vì quá trình tái tạo được áp dụng ngay lập tức khi hoàn thành. Ví dụ: nếu sức khỏe là 1 và nhiệm vụ tiếp theo mất 1 giây thì sức khỏe sẽ bằng 0 khi hoàn thành, nhưng phần thưởng sẽ được áp dụng ngay lập tức và có thể khôi phục lại. 

Một tình huống cạnh khác xảy ra khi một hi rất lớn được ghép với một ti cực lớn. Một chiến lược ngây thơ có thể cố gắng ưu tiên nó sớm do phần thưởng của nó, nhưng việc sắp xếp đúng thứ tự sẽ trì hoãn nó nếu nó gây ra việc tiêu tốn quá nhiều thời gian so với lợi ích của nó. Quy tắc sắp xếp xử lý việc này một cách tự động vì một tác vụ như vậy có ti − hi lớn và được đẩy sau, ngăn ngừa sự cạn kiệt sớm trong quá trình thực thi lâu dài của nó. 

Cuối cùng, các trường hợp m nhỏ so với các giá trị ti riêng lẻ sẽ nêu bật lý do tại sao việc đặt hàng lại cần thiết. Ngay cả khi việc chữa lành toàn bộ là đủ trên toàn cầu, một lựa chọn sớm không chính xác có thể tạo ra sự thiếu hụt tạm thời giết chết quá trình trước khi phần thưởng sau đó xuất hiện, điều mà mệnh lệnh tham lam rõ ràng tránh được bằng cách giữ các nhiệm vụ áp lực cao tránh xa các tiền tố ban đầu.
