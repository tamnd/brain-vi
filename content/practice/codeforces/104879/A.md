---
title: "CF 104879A - Cocktail cà phê"
description: "Chúng ta được cung cấp một số loại đồ ăn nhẹ, trong đó mỗi loại đồ ăn nhẹ có thể xuất hiện nhiều lần và mỗi bữa ăn nhẹ riêng lẻ đều đóng góp một lượng caffeine nhất định."
date: "2026-06-28T09:36:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104879
codeforces_index: "A"
codeforces_contest_name: "Innopolis Open 2024. Qualification Round 2"
rating: 0
weight: 104879
solve_time_s: 46
verified: true
draft: false
---

[CF 104879A - Cocktail cà phê](https://codeforces.com/problemset/problem/104879/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số loại đồ ăn nhẹ, trong đó mỗi loại đồ ăn nhẹ có thể xuất hiện nhiều lần và mỗi bữa ăn nhẹ riêng lẻ đều đóng góp một lượng caffeine nhất định. Sự đơn giản hóa chính là thay vì suy nghĩ về khối lượng thô và tỷ lệ phần trăm, chúng tôi trực tiếp coi mỗi bữa ăn nhẹ đóng góp một giá trị caffeine cố định. Vì vậy, mọi mục trong đầu vào có thể được xem dưới dạng một “gói” bổ sung một lượng caffeine nhất định. 

Nhiệm vụ là đạt được ít nhất ngưỡng caffeine cần thiết bằng cách sử dụng số lượng tối thiểu các loại đồ ăn nhẹ đã chọn. Chi tiết quan trọng là việc chọn một loại đồ ăn nhẹ một cách hiệu quả cho phép chúng ta sử dụng tất cả đồ ăn nhẹ cùng loại với mục đích tích lũy caffeine, do đó, quyết định được đưa ra ở cấp độ loại chứ không phải ở cấp độ từng món. 

Đầu ra là số lượng tối thiểu các loại đồ ăn nhẹ riêng biệt mà chúng ta cần dùng để tổng lượng caffeine từ tất cả các món ăn nhẹ đã chọn trong các loại đó ít nhất đạt đến giá trị mục tiêu hoặc −1 nếu ngay cả việc ăn tất cả đồ ăn nhẹ cũng không đủ. 

Các ràng buộc ngụ ý rằng kích thước đầu vào có thể lớn, do đó, bất kỳ giải pháp nào thử tất cả các tập hợp con của các loại hoặc tính toán lại tổng nhiều lần sẽ quá chậm. Dự kiến ​​sẽ có một giải pháp sắp xếp hoặc tuyến tính cho mỗi bài kiểm tra, do đó, khoảng O(n log n) hoặc O(n) cho mỗi bài kiểm tra là có thể chấp nhận được, trong khi hành vi hàm mũ hoặc bậc hai thì không. 

Một trường hợp thất bại khó phát hiện khi xử lý đồ ăn nhẹ một cách độc lập thay vì nhóm theo loại. Nếu chúng ta tham lam chọn từng món ăn nhẹ mà không tổng hợp theo loại, chúng ta có thể chọn nhiều món ăn nhẹ từ loại năng suất thấp trong khi tốt hơn hết là nên chọn loại có năng suất cao hơn sớm hơn. 

Ví dụ: giả sử chúng ta có loại A với đồ ăn nhẹ đóng góp tổng cộng 100 caffeine cho nhiều món và loại B với hai món ăn nhẹ đóng góp 51, tổng cộng là 102. Nếu chúng ta chọn tham lam theo các giá trị riêng lẻ, chúng ta có thể lấy cả hai loại 51 trước và trì hoãn việc chọn loại A, nhưng lý do chính xác phụ thuộc vào việc phân nhóm theo loại đóng góp chứ không phải lựa chọn theo từng món. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu bằng cách xem xét mọi tập hợp con các loại đồ ăn nhẹ có thể có. Đối với mỗi tập hợp con, chúng tôi tổng hợp tất cả lượng caffeine đóng góp từ tất cả các món ăn nhẹ thuộc các loại đó và kiểm tra xem tổng số có đạt được mục tiêu hay không. Trong số tất cả các tập hợp con hợp lệ, chúng tôi chọn tập hợp con có số lượng loại nhỏ nhất. Điều này đúng vì nó đánh giá trực tiếp mọi quyết định có thể xảy ra. Tuy nhiên, nếu có q loại, điều này đòi hỏi phải kiểm tra 2^q tập hợp con và việc tính tổng cho mỗi tập hợp con có thể mất thời gian tuyến tính theo số lượng mục. Điều này nhanh chóng trở nên không khả thi ngay cả đối với đầu vào có kích thước vừa phải. 

Quan sát quan trọng là trong mỗi loại, không có lý do gì để sử dụng một phần đóng góp của nó. Khi chúng tôi quyết định thêm một loại, chúng tôi sẽ lấy tất cả đồ ăn nhẹ của nó, vì mỗi món đều đóng góp tích cực vào tổng số và không bị phạt khi làm như vậy. Điều này giải quyết vấn đề từ các quyết định ở cấp độ mặt hàng đến trọng số ở cấp độ loại, trong đó mỗi loại có một giá trị tổng hợp duy nhất bằng tổng lượng caffeine của nó. 

Sau khi giảm trọng số ở cấp độ loại, mục tiêu sẽ trở thành chọn số lượng loại tối thiểu có tổng trọng lượng đạt ít nhất x. Đây là một cấu trúc tham lam cổ điển: chúng ta muốn tích lũy càng nhiều caffeine càng tốt bằng cách sử dụng càng ít loại càng tốt, vì vậy chúng ta luôn ưu tiên loại “có giá trị” nhất trước tiên. 

Điều này giúp giảm bớt nhiệm vụ tính toán tổng lượng caffeine cho mỗi loại, sắp xếp các loại theo tổng lượng này theo thứ tự giảm dần và sau đó tích lũy một cách tham lam cho đến khi chúng ta đạt được mục tiêu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con loại | O(2^q · n) | O(n) | Quá chậm | 
| Nhóm + sắp xếp tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả đồ ăn nhẹ và nhóm chúng theo loại, tích lũy tổng lượng caffeine đóng góp cho từng loại. Bước này chuyển đổi dữ liệu cấp mục thành một biểu diễn nhỏ gọn trong đó mỗi loại có một trọng số có ý nghĩa duy nhất. 
2. Với mỗi loại, hãy tính tổng lượng caffeine đóng góp của nó. Điều này là cần thiết vì chỉ có tổng hợp mới quan trọng đối với bất kỳ quyết định nào về việc chọn loại đó. 
3. Lưu trữ tất cả các loại tổng trong một danh sách và sắp xếp chúng theo thứ tự giảm dần. Việc sắp xếp đảm bảo chúng tôi luôn xem xét loại có tác động mạnh nhất trước tiên, điều này rất cần thiết để giảm thiểu số lượng loại được chọn. 
4. Khởi tạo tổng hiện có ở mức 0 và bộ đếm cho các loại đã chọn. 
5. Lặp lại các tổng loại đã được sắp xếp, thêm từng loại vào tổng hiện có và tăng bộ đếm loại. Sau mỗi lần thêm, hãy kiểm tra xem tổng lượng hiện tại đã đạt hay vượt quá giá trị caffeine mục tiêu hay chưa. 
6. Nếu đạt được mục tiêu, hãy in ra số loại đã sử dụng cho đến nay. Nếu vòng lặp kết thúc mà không đạt được mục tiêu, xuất ra −1. 

Lý do sắp xếp an toàn là vì mọi loại đều đóng góp độc lập và đầy đủ sau khi được chọn, do đó, việc sắp xếp lại thứ tự lựa chọn không thể cải thiện số lượng loại cần thiết ngoài việc luôn lấy phần đóng góp lớn nhất còn lại trước. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình, trạng thái liên quan duy nhất là lượng caffeine đã được tích lũy và bao nhiêu loại đã được chọn. Vì mỗi loại đóng góp một lượng dương cố định nên việc thay thế khoản đóng góp nhỏ hơn bằng khoản đóng góp lớn hơn sớm hơn không bao giờ có thể làm tăng số lượng loại cần thiết để đạt được tổng số như nhau. Điều này chứng tỏ rằng luôn tồn tại một giải pháp tối ưu là chọn các loại theo thứ tự không tăng tổng lượng caffeine. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    
    from collections import defaultdict
    tot = defaultdict(int)

    for _ in range(n):
        t, c = map(int, input().split())
        tot[t] += c

    vals = sorted(tot.values(), reverse=True)

    cur = 0
    cnt = 0

    for v in vals:
        cur += v
        cnt += 1
        if cur >= x:
            print(cnt)
            return

    print(-1)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp thuật toán. Từ điển tổng hợp caffeine theo từng loại trong một lượt duy nhất, đảm bảo độ phức tạp tuyến tính về số lượng đồ ăn nhẹ. Việc sắp xếp các giá trị tổng hợp đảm bảo chúng tôi luôn chọn loại có sẵn tốt nhất tiếp theo. 

Điểm tinh tế duy nhất là bước nhóm phải diễn ra trước khi sắp xếp. Việc sắp xếp các mục thô thay vì tổng hợp loại sẽ dẫn đến các quyết định tham lam không chính xác, vì nhiều mục từ cùng một loại có thể được phân chia theo vùng chọn một cách không cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có ba loại: 

đầu vào:```
5 150
1 50
1 30
2 60
3 40
3 20
```Ở đây loại 1 tổng cộng 80, loại 2 tổng cộng 60 và loại 3 tổng cộng 60. 

| Bước | Các loại được chọn | Tổng hiện tại | Hành động | 
| --- | --- | --- | --- | 
| 1 | [] | 0 | Bắt đầu | 
| 2 | [1] | 80 | Lấy loại lớn nhất | 
| 3 | [1, 2] | 140 | Thêm lớn nhất tiếp theo | 
| 4 | [1, 2, 3] | 200 | Đã đạt mục tiêu | 

Chúng tôi dừng lại sau khi chọn 3 loại. 

Dấu vết này cho thấy mặc dù loại 2 và 3 bằng nhau nhưng thứ tự giữa chúng không thành vấn đề. Điều quan trọng là chúng tôi luôn ưu tiên những đóng góp lớn hơn trước. 

Bây giờ hãy xem xét trường hợp không thể đạt được mục tiêu: 

đầu vào:```
3 200
1 50
2 60
3 40
```Tổng số là 50, 60, 40. Kể cả sau khi chọn hết các loại, chúng ta cũng chỉ đạt được 150 nên đáp án là −1. 

| Bước | Các loại được chọn | Tổng hiện tại | Hành động | 
| --- | --- | --- | --- | 
| 1 | [2] | 60 | Lấy lớn nhất | 
| 2 | [2,1] | 110 | Tiếp tục | 
| 3 | [2,1,3] | 150 | Xả hết | 
| 4 | - | 150 | Thất bại | 

Điều này xác nhận rằng thuật toán xử lý chính xác các mục tiêu không khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | nhóm là O(n), tổng số loại sắp xếp chiếm ưu thế | 
| Không gian | O(n) | lưu trữ tổng số mỗi loại | 

Giải pháp này phù hợp một cách thoải mái với các ràng buộc điển hình dành cho các vấn đề của Codeforces với tối đa 2e5 mục, vì việc sắp xếp chiếm ưu thế và mọi thứ khác đều tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    from collections import defaultdict
    input = sys.stdin.readline

    it = iter(inp.strip().split())
    n = int(next(it))
    x = int(next(it))

    tot = defaultdict(int)
    for _ in range(n):
        t = int(next(it))
        c = int(next(it))
        tot[t] += c

    vals = sorted(tot.values(), reverse=True)

    cur = 0
    cnt = 0

    for v in vals:
        cur += v
        cnt += 1
        if cur >= x:
            return str(cnt)

    return str(-1)

# sample-like cases
assert solve_capture("5 150 1 50 1 30 2 60 3 40 3 20") == "3"
assert solve_capture("3 200 1 50 2 60 3 40") == "-1"

# minimal case
assert solve_capture("1 10 1 10") == "1"

# already satisfied by one type
assert solve_capture("4 50 1 10 1 20 1 30 2 100") == "1"

# multiple small types needed
assert solve_capture("6 100 1 20 2 20 3 20 4 20 5 20 6 20") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu chính xác duy nhất | 1 | trường hợp ranh giới tối thiểu | 
| tổng không thể | -1 | kiểm tra tính khả thi | 
| một loại thống trị | 1 | tham lam dừng sớm đúng đắn | 
| nhiều loại nhỏ bằng nhau | 5 | ổn định trật tự và tích lũy | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi một loại đã vượt quá mục tiêu. Thuật toán xử lý việc này ngay lập tức vì sau khi sắp xếp, loại lớn nhất sẽ được xử lý trước và kích hoạt việc chấm dứt trong một bước. 

Một trường hợp khác là khi tất cả các loại đều có đóng góp rất nhỏ và câu trả lời phụ thuộc vào việc kết hợp nhiều loại trong số chúng. Vì thuật toán tích lũy theo thứ tự được sắp xếp nên nó sẽ tiếp tục thêm một cách tự nhiên cho đến khi đạt đến ngưỡng hoặc xảy ra tình trạng cạn kiệt mà không yêu cầu bất kỳ xử lý đặc biệt nào. 

Trường hợp cuối cùng là khi không có sự kết hợp nào đạt được mục tiêu. Vì vòng lặp kết thúc sau khi xử lý tất cả các giá trị loại tổng hợp và tổng không bao giờ vượt qua ngưỡng nên thuật toán trả về chính xác −1 mà không bị kết thúc sớm.
