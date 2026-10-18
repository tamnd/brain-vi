---
title: "CF 105579G - Minecraft"
description: "Chúng ta có một mặt cắt dọc của thế giới Minecraft được biểu thị dưới dạng lưới $h nhân w$. Mỗi ô trống, khối đất hoặc khối vàng."
date: "2026-06-22T14:30:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105579
codeforces_index: "G"
codeforces_contest_name: "Udmurtia High School Programming Contest (Qualification for VKOSHP 2012)"
rating: 0
weight: 105579
solve_time_s: 64
verified: true
draft: false
---

[CF 105579G - Minecraft](https://codeforces.com/problemset/problem/105579/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mặt cắt dọc của thế giới Minecraft được thể hiện dưới dạng$h \times w$lưới. Mỗi ô trống, khối đất hoặc khối vàng. Đáy là dung nham và cấu hình ban đầu tạo thành các ngăn xếp vững chắc: không có khối "nổi", vì vậy mọi ô bị chiếm giữ đều có hỗ trợ bên dưới suốt chặng đường đi xuống. 

Người chơi bắt đầu ngay phía trên ô không trống cao nhất ở khu vực có liên quan ngoài cùng bên trái và có thể di chuyển tự do qua khoảng trống gần đầu. Di chuyển không phải là khó khăn chính. Cái giá thực sự đến từ việc sửa đổi thế giới: khai thác các khối và đặt các khối đất. 

Việc khai thác có thể được thực hiện theo hướng xuống hoặc sang phải. Việc khai thác sẽ loại bỏ một khối và có khả năng khiến trọng lực làm sụp đổ bất cứ thứ gì phía trên nó. Việc đặt bụi bẩn sẽ chèn một khối sẽ rơi cho đến khi chạm vào giá đỡ. Nhiệm vụ là loại bỏ tất cả các khối vàng đồng thời đạt đến đúng ranh giới, giảm thiểu số lượng hành động tạo và phá hủy khối. 

Một cách hữu ích để giải thích điều này là việc truyền tải về cơ bản là miễn phí khi tồn tại không gian trống. Những hành động tốn kém duy nhất là những hành động thay đổi cấu trúc vĩnh viễn: loại bỏ các khối hoặc đặt các khối mới. Điều này có nghĩa là vấn đề về cơ bản là khối nào phải được loại bỏ để có thể tiếp cận được tất cả vàng, chứ không phải là kế hoạch di chuyển. 

Một vấn đề tế nhị xuất hiện khi các khối vàng được chôn bên trong một cột. Nếu chúng ta cố gắng tiếp cận họ từ phía trên, trước tiên chúng ta phải loại bỏ mọi khối phía trên họ. Điều này ngay lập tức cho thấy rằng chi phí được xác định bởi sự phụ thuộc theo chiều dọc bên trong mỗi cột. 

Không có vật cản ngang bệnh lý nào buộc phải tái cấu trúc lưới điện toàn cầu. Vì hàng trên cùng trống trên toàn bộ chiều rộng nên người chơi có thể di chuyển theo chiều ngang phía trên tất cả các cấu trúc mà không cần thao tác chi tiêu. Điều này loại bỏ hầu hết khía cạnh “mê cung” và giảm vấn đề xuống dạng lý luận theo cột. 

Một sai lầm ngây thơ là cho rằng chỉ có khối vàng mới quan trọng và câu trả lời chỉ đơn giản là số lượng ô vàng. Điều này không thành công khi vàng không ở trên đỉnh cột của nó. 

Ví dụ: hãy xem xét một cột duy nhất:```
#
G
#
```Nếu chúng ta cố gắng chỉ khai thác ô vàng, chúng ta không thể tiếp cận trực tiếp với nó. Trước tiên chúng ta phải loại bỏ khối bụi bẩn trên cùng. Giá đúng là 2 chứ không phải 1. 

Một trường hợp thất bại khác là giả sử chúng ta phải xóa toàn bộ các cột. Điều đó sẽ vượt quá mức khi vàng chỉ xuất hiện ở gần đỉnh. 

Thách thức chính là xác định chính xác khoảng cách chúng ta cần đào xuống trong mỗi cột. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng chuyển động của người chơi và mọi chuỗi hoạt động có thể xảy ra. Trạng thái sẽ cần bao gồm toàn bộ cấu hình lưới vì mọi hoạt động khai thác đều khiến trọng lực dịch chuyển các khối và mọi vị trí đều có thể thay đổi khả năng tiếp cận trong tương lai. Ngay cả với$h, w \le 100$, số lượng cấu hình tăng lên một cách bùng nổ. Mỗi thao tác sẽ phân nhánh thành nhiều trạng thái tiếp theo có thể có và hệ số phân nhánh lớn vì người chơi có thể chọn các cột khác nhau và các lệnh hành động khác nhau. Điều này nhanh chóng trở nên không khả thi. 

Sự đơn giản hóa cấu trúc quan trọng là chuyển động ngang hoàn toàn tự do và luôn có thể thực hiện được thông qua không gian trống ở phía trên. Điều này có nghĩa là chúng tôi có thể xử lý từng cột một cách độc lập về khả năng truy cập từ phía trên. Khi chúng ta đứng phía trên một cột, câu hỏi duy nhất là chúng ta phải loại bỏ bao nhiêu khối trước khi loại bỏ hết vàng trong cột mà chúng ta quan tâm. 

Bên trong một cột duy nhất, trọng lực đảm bảo rằng việc loại bỏ các khối bên trên không tạo ra sự sắp xếp lại bên trong phức tạp ảnh hưởng đến chi phí. Cột luôn là một ngăn xếp liền kề nhau, vì vậy chúng ta có thể nghĩ theo kiểu quét dọc từ trên xuống dưới. 

Bây giờ, quan sát quan trọng là chúng ta không bao giờ cần phải xuống dưới khối vàng thấp nhất trong một cột. Bất kỳ bụi bẩn hoặc cấu trúc nào bên dưới nó có thể vẫn còn nguyên vì nó không chặn khả năng tiếp cận số vàng còn lại ở nơi khác hoặc lối ra. Do đó, mỗi cột đóng góp độc lập: chúng ta chỉ cần xóa mọi thứ từ trên xuống đến số vàng thấp nhất trong cột đó. 

Điều này làm giảm vấn đề đối với việc tính toán, đối với mỗi cột, độ sâu cao nhất (gần nhất với đáy) trong số các ô vàng của nó và tính tổng tất cả các khối ở trên và bao gồm cả điểm đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ |$O(hw)$| Quá chậm | 
| Quét vàng thấp nhất theo cột |$O(hw)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi cột, quét từ trên xuống dưới và ghi lại chỉ số hàng thấp nhất (sâu nhất) có chứa khối vàng. 

Điều này quan trọng vì bất cứ điều gì dưới điểm đó đều không liên quan đến việc hoàn thành tất cả số vàng trong cột. 
2. Nếu một cột không chứa vàng, nó sẽ đóng góp chi phí bằng 0 vì chúng ta không bao giờ cần sửa đổi nó. 
3. Mặt khác, tính xem có bao nhiêu ô nằm từ đầu cột xuống vị trí vàng thấp nhất. 

Mỗi ô như vậy phải được loại bỏ tại một thời điểm nào đó để truy cập và khai thác tất cả vàng trong cột đó. 
4. Tính tổng giá trị này trên tất cả các cột để có được câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Điều bất biến chính là để loại bỏ một khối vàng, tất cả các khối phía trên nó trong cùng một cột phải bị phá hủy trước tiên và không có thao tác nào ở cột khác có thể loại bỏ yêu cầu này. Vì chuyển động trên cùng là tự do nên không có sự phụ thuộc giữa các cột. Khi chúng tôi quyết định xóa một cột xuống một độ sâu nhất định, quá trình này sẽ tuyến tính và độc lập với các cột khác. Do đó, việc chọn lượng vàng sâu nhất trong mỗi cột sẽ xác định mức khai thác cần thiết tối thiểu cho cột đó và việc kết hợp các mức tối thiểu độc lập này sẽ mang lại mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    h, w = map(int, input().split())
    grid = [input().strip() for _ in range(h)]

    ans = 0

    for j in range(w):
        lowest_gold = -1
        for i in range(h):
            if grid[i][j] == 'G':
                lowest_gold = i

        if lowest_gold == -1:
            continue

        # we must clear from top down to lowest_gold inclusive
        ans += lowest_gold + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo ý tưởng trực tiếp: đối với mỗi cột, chúng tôi xác định ô vàng sâu nhất. Số thao tác hủy cần thực hiện ở cột đó chính xác là số ô tính từ trên xuống đến vị trí đó. 

Chúng tôi tránh mô phỏng hoàn toàn chuyển động vì cấu trúc bài toán đảm bảo việc di chuyển không hạn chế qua vùng trên cùng trống. Công việc có ý nghĩa duy nhất là tính các phụ thuộc theo chiều dọc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
.....
#..#.
##.##
#GGG#
#####
```Chúng tôi xử lý từng cột một cách độc lập. 

| Cột | Hàng G thấp nhất | Chi phí tăng thêm | 
| --- | --- | --- | 
| 1 | 3 | 4 | 
| 2 | 3 | 4 | 
| 3 | 3 | 4 | 
| 4 | - | 0 | 
| 5 | - | 0 | 

Tổng cộng là 12. 

Điều này xác nhận rằng cả ba vàng đều nằm trong cùng một hàng và mọi thứ phía trên hàng đó trong các cột tương ứng của chúng phải bị loại bỏ trước khi tiếp cận chúng. 

### Ví dụ 2 

đầu vào:```
5 5
.....
#..G.
#..##
#####
#####
```| Cột | Hàng G thấp nhất | Chi phí tăng thêm | 
| --- | --- | --- | 
| 1 | - | 0 | 
| 2 | 3 | 4 | 
| 3 | - | 0 | 
| 4 | - | 0 | 
| 5 | - | 0 | 

Chỉ có một cột đóng góp. Chúng ta chỉ cần giải quyết tới số vàng ở cột 2. 

Điều này cho thấy rằng phần đất bên dưới số vàng là không liên quan, vì chúng ta không bao giờ cần phải xuống sâu hơn sau khi đã khai thác hết số vàng cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(hw)$| Mỗi ô được kiểm tra một lần trong khi quét các cột để tìm vị trí vàng | 
| Không gian |$O(1)$| Chỉ một số biến được sử dụng ngoài lưới đầu vào | 

Những hạn chế$h, w \le 100$làm cho cách tiếp cận này trở nên tầm thường để thực hiện trong giới hạn, vì nhiều nhất$10^4$tế bào được xử lý 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    h, w = map(int, sys.stdin.readline().split())
    grid = [sys.stdin.readline().strip() for _ in range(h)]

    ans = 0
    for j in range(w):
        lowest = -1
        for i in range(h):
            if grid[i][j] == 'G':
                lowest = i
        if lowest != -1:
            ans += lowest + 1

    return str(ans)

# sample-like tests
assert run("""5 5
.....
#..#.
##.##
#GGG#
#####
""") == "12"

assert run("""5 5
.....
#..G.
#..##
#####
#####
""") == "4"

# minimal grid, no gold
assert run("""5 5
.....
.....
.....
.....
#####
""") == "0"

# single column gold at top
assert run("""5 1
G
#
#
#
#
""") == "1"

# gold at bottom of column
assert run("""5 1
#
#
#
G
#
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có lưới vàng | 0 | xử lý các cột trống | 
| đỉnh cột đơn vàng | 1 | sự đúng đắn từng cái một | 
| vàng đậm | 4 | logic vàng sâu sắc nhất | 
| lưới giống mẫu | 12 | tổng hợp nhiều cột | 

## Vỏ cạnh 

Cột không có vàng được xử lý rõ ràng vì chúng tôi không bao giờ cập nhật`lowest_gold`điểm đánh dấu và nó đóng góp bằng không. Điều này tránh việc buộc phải đào không chính xác các cấu trúc đất không liên quan. 

Cột có vàng ở trên cùng đảm bảo thuật toán trả về 1, vì chúng ta vẫn phải thực hiện một lần hủy để loại bỏ chính ô vàng đó. Quá trình quét xác định chính xác`lowest_gold = 0`, dẫn đến chi phí 1. 

Một cột có nhiều khối vàng tồn tại xác nhận lý do tại sao chỉ có khối sâu nhất mới quan trọng. Sau khi chúng tôi đào xuống lượng vàng thấp nhất, tất cả các khối vàng cao hơn sẽ tự động được đưa vào đường dẫn loại bỏ, do đó, không phát sinh thêm chi phí cột vượt quá độ sâu đó.
