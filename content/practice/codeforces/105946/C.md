---
title: "CF 105946C - Trà quản gia"
description: "Mỗi trường hợp thử nghiệm mô tả một hộ gia đình trong đó một nhóm người hầu phải hoàn thành một loạt công việc. Mỗi công việc đều có một lượng công việc cần thiết và mỗi giờ mỗi người hầu đều đóng góp công việc tùy theo cách họ được giao."
date: "2026-06-22T16:00:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105946
codeforces_index: "C"
codeforces_contest_name: "2025 UP ACM Algolympics Final Round"
rating: 0
weight: 105946
solve_time_s: 101
verified: true
draft: false
---

[CF 105946C - Trà của quản gia](https://codeforces.com/problemset/problem/105946/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một hộ gia đình trong đó một nhóm người hầu phải hoàn thành một loạt công việc. Mỗi công việc đều có một lượng công việc cần thiết và mỗi giờ mỗi người hầu đều đóng góp công việc tùy theo cách họ được giao. 

Một người hầu luôn đóng góp 1 đơn vị công việc mỗi giờ cho công việc mà họ được giao. Ngoài ra, mỗi người hầu có đúng một công việc “yêu thích”. Nếu họ được giao làm công việc cụ thể đó trong một giờ nhất định, họ sẽ trở nên hiệu quả hơn và đóng góp 5 đơn vị thay vì 1 đơn vị trong giờ đó. 

Quản gia trưởng có thể phân công lại tất cả người hầu một cách độc lập mỗi giờ và nhiều người hầu có thể làm cùng một công việc. Mục tiêu là chọn các nhiệm vụ theo giờ sao cho tất cả công việc đều đạt hoặc vượt mức công việc yêu cầu trong số giờ tối thiểu. 

Phần đầu vào quan trọng là danh sách chuyên môn, cho biết từng người hầu sẽ tăng cường công việc nào và giá trị công việc cần thiết cho mỗi công việc. Đầu ra là số giờ tối thiểu cần thiết để hoàn thành tất cả công việc theo lịch trình tối ưu. 

Các ràng buộc rất nhỏ: tối đa 100 người hầu và 100 công việc, với yêu cầu công việc cũng bị giới hạn bởi 100. Điều này cho thấy rõ ràng rằng giải pháp không yêu cầu cấu trúc tối ưu hóa nặng nề hoặc không gian trạng thái lớn. Bất kỳ cách tiếp cận nào liên quan đến mô phỏng theo giờ hoặc tổng hợp từng công việc đều khả thi, miễn là logic mỗi bước đơn giản. 

Một trường hợp phức tạp xuất hiện khi một số công việc vặt không có người hầu tận tâm. Trong tình huống đó, công việc đó chỉ có thể được thực hiện bằng cách sử dụng các khoản đóng góp chung 1 đơn vị, điều này có thể buộc thời gian hoàn thành cao hơn. Một tình huống quan trọng khác là khi một công việc có nhiều người phục vụ tận tâm, khiến công việc đó nhanh hơn đáng kể so với những công việc khác, điều này có thể chiếm ưu thế trong lịch trình chung. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề là mô phỏng từng giờ. Trong mỗi giờ, chúng tôi chọn sự phân công người hầu làm việc nhà và tính xem mỗi công việc đó nhận được bao nhiêu công việc. Chúng tôi lặp lại điều này cho đến khi tất cả các công việc được hoàn thành. Điều này đúng, nhưng hệ số phân nhánh rất lớn vì mỗi người hầu có thể chọn bất kỳ công việc vặt nào trong mỗi giờ, do đó số lượng nhiệm vụ có thể được giao mỗi giờ là c^n. Ngay cả với n = 100, điều này hoàn toàn không thể thực hiện được. 

Cấu trúc của vấn đề cho phép đơn giản hóa mạnh mẽ hơn. Mỗi người hầu đóng góp mức cơ bản là 1 đơn vị mỗi giờ bất kể thế nào và chỉ nhận thêm 4 đơn vị khi được giao công việc riêng của họ. Điều này có nghĩa là “quyền lực bổ sung” duy nhất trong hệ thống đến từ việc kết nối những người hầu với công việc yêu thích của họ. 

Thay vì suy nghĩ về các bài tập theo giờ trên toàn cầu, chúng ta có thể suy luận theo từng công việc. Nếu một công việc mà tôi có người hầu chuyên trách, thì mỗi giờ chúng tôi có thể đảm bảo đóng góp mạnh mẽ 5 cho mỗi người hầu đó nếu chúng tôi phân công họ chính xác. Tất cả những người hầu còn lại vẫn có thể đóng góp mỗi người 1 đơn vị, nhưng khoản đóng góp cơ bản này là thống nhất cho các công việc và luôn có thể được phân phối lại một cách tự do. 

Điều này dẫn đến nhận xét quan trọng: trong một lịch trình tối ưu, không có lý do gì để giảm việc sử dụng các chuyên gia trong công việc riêng của họ, vì việc di chuyển họ đi sẽ làm mất 4 đơn vị năng suất mỗi giờ trong khi chỉ đạt được 1 đơn vị ở nơi khác. Do đó, mỗi công việc đều có năng lực sản xuất cố định mỗi giờ của tất cả những người hầu làm việc bình thường cộng với phần thưởng bổ sung từ các chuyên gia của nó. 

Từ quan điểm này, mỗi công việc tôi nhận được n đơn vị công việc cơ bản mỗi giờ (một đơn vị từ mỗi người hầu), cộng thêm 4 đơn vị cho mỗi người hầu có chuyên môn là i. Điều đó mang lại tỷ lệ hiệu quả mỗi giờ là: 

n + 4 · mi 

Khi tỷ lệ này được cố định, thời gian cần thiết cho mỗi công việc sẽ trở nên độc lập và câu trả lời tổng thể sẽ được xác định bởi công việc chậm nhất.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng giờ Brute Force trên tất cả các bài tập | Hàm mũ | O(1) | Quá chậm | 
| Tính tỷ lệ mỗi công việc n + 4·mi và lấy trần tối đa | O(n + c) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán 

1. Đếm xem, với mỗi công việc i, có bao nhiêu người hầu chuyên làm việc đó. Gọi giá trị này là mi. Điều này cho thấy có bao nhiêu công nhân có thể tạo ra sản lượng tăng thêm cho công việc đó. 
2. Lưu ý rằng mỗi người phục vụ đóng góp 1 đơn vị công việc mỗi giờ bất kể nhiệm vụ được giao. Điều này có nghĩa là mọi công việc đều nhận được mức cơ bản thống nhất là n đơn vị mỗi giờ một cách hiệu quả nếu chúng ta phân bổ công nhân một cách tối ưu. 
3. Thêm lợi thế về chuyên môn hóa: mỗi người hầu được giao công việc riêng của mình sẽ đóng góp thêm 4 đơn vị ngoài mức cơ bản. Vì có mi những người hầu làm công việc vặt như vậy nên số tiền đóng góp thêm cho công việc đó mỗi giờ là 4 · mi. 
4. Kết hợp đường cơ sở và tiền thưởng để tính tiến độ tối đa có thể đạt được mỗi giờ cho công việc i là n + 4 · mi. 
5. Tính thời gian cần thiết cho mỗi công việc nhà dưới dạng ceil(wi / (n + 4 · mi)). 
6. Câu trả lời cuối cùng là tối đa cho tất cả các công việc, vì tất cả đều phải được hoàn thành. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mỗi người hầu luôn sản xuất chính xác 1 đơn vị mỗi giờ và lợi thế duy nhất có thể kiểm soát được là liệu người hầu đó có được giao công việc riêng của họ hay không, nhận thêm 4 đơn vị. Vì việc phân công lại một người hầu chuyên môn khỏi công việc của họ sẽ làm giảm tổng năng suất đi 4 đơn vị, nên bất kỳ lịch trình tối ưu nào cũng sẽ không bao giờ hy sinh vĩnh viễn lợi ích chuyên môn hóa trừ khi công việc đó đã hoàn thành. Điều này đảm bảo rằng năng suất hiệu quả của mỗi công việc có thể được xử lý độc lập bằng cách sử dụng đầy đủ các chuyên gia và không có sự đánh đổi nào giữa các công việc có thể cải thiện thời gian hoàn thành tối đa ngoài việc cân bằng các tỷ lệ độc lập này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, c = map(int, input().split())
        s = list(map(int, input().split()))
        w = list(map(int, input().split()))
        
        cnt = [0] * (c + 1)
        for x in s:
            cnt[x] += 1
        
        ans = 0
        for i in range(1, c + 1):
            rate = n + 4 * cnt[i]
            need = (w[i - 1] + rate - 1) // rate
            ans = max(ans, need)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng một bảng tần số về số lượng người hầu chuyên làm từng công việc nhà. Đây là thông tin cấu trúc duy nhất cần thiết từ các ràng buộc gán. 

Đối với mỗi công việc, nó tính toán mức đóng góp hiệu quả mỗi giờ là n + 4 · mi. Sau đó, tổng thời gian được tính bằng phép chia trần số nguyên, vì không được phép bán một phần giờ. 

Câu trả lời cuối cùng là tối đa cho tất cả các công việc, vì tất cả đều phải được hoàn thành. 

Một cạm bẫy triển khai phổ biến là quên rằng các công việc được lập chỉ mục 1 trong đầu vào trong khi các mảng trong Python được lập chỉ mục 0, được xử lý bằng cách trừ đi một khi đọc wi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3, c = 3 

s = [1, 2, 3] 

w = [5, 11, 8] 

Mọi công việc đều có duy nhất một người hầu tận tâm nên mi = 1 cho tất cả i. 

| Việc vặt | mi | Tỷ lệ = n + 4mi | wi | Thời gian | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 7 | 5 | 1 | 
| 2 | 1 | 7 | 11 | 2 | 
| 3 | 1 | 7 | 8 | 2 | 

Thời gian tối đa là 2 giờ. Công việc thứ hai chiếm ưu thế vì nó đòi hỏi nhiều công sức nhất so với tỷ lệ của nó. 

Dấu vết này cho thấy cấu trúc chuyên môn hóa giống hệt nhau vẫn dẫn đến thời gian hoàn thành khác nhau hoàn toàn do wi khác nhau. 

### Ví dụ 2 

đầu vào: 

n = 3, c = 3 

s = [3, 1, 1] 

w = [7, 6, 5] 

Ở đây giá trị mi là: 

m1 = 2, m2 = 0, m3 = 1. 

| Việc vặt | mi | Tỷ lệ = n + 4mi | wi | Thời gian | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 11 | 7 | 1 | 
| 2 | 0 | 3 | 6 | 2 | 
| 3 | 1 | 7 | 5 | 1 | 

Công việc thứ hai trở thành nút thắt cổ chai vì nó không có chuyên gia và chỉ dựa vào những đóng góp cơ bản. 

Ví dụ này chứng tỏ việc thiếu chuyên gia trực tiếp làm tăng thời gian hoàn thành ngay cả khi các công việc khác kết thúc rất nhanh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + c) cho mỗi trường hợp thử nghiệm | Đếm chuyên môn và đánh giá từng công việc một lần | 
| Không gian | O(c) | Mảng tần số tính chuyên môn công việc | 

Các ràng buộc cho phép tối đa 100 người hầu và 100 công việc, vì vậy phương pháp quét tuyến tính này nhanh chóng và dễ dàng sử dụng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample-like cases
assert run("""1
3 3
1 2 3
5 11 8
""") == "2"

assert run("""1
3 3
3 1 1
7 6 5
""") == "2"

# minimum case
assert run("""1
1 1
1
1
""") == "1"

# all servants specialize same task
assert run("""1
4 2
1 1 1 1
10 1
""") == "1"

# no specialization advantage case
assert run("""1
3 3
1 2 3
3 3 3
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nhiệm vụ duy nhất của người hầu | 1 | Độ đúng cơ sở | 
| Tất cả các chuyên gia tập trung | 1 | Chia tỷ lệ cao | 
| Công việc nhỏ thống nhất | 1 | Phân phối cân bằng | 
| Trường hợp kiểu mẫu | 2 | Hành vi tắc nghẽn đa tác vụ | 

## Vỏ cạnh 

Trường hợp góc cạnh xuất hiện khi một công việc vặt không có người hầu tận tâm. Trong trường hợp đó mi = 0, do đó tốc độ của nó trở thành n. Thuật toán xử lý vấn đề này một cách tự nhiên bằng cách chỉ đưa ra đóng góp cơ bản, điều này phản ánh chính xác rằng không có phần thưởng nào tồn tại để tăng tốc nó. 

Một trường hợp khác là khi tất cả người hầu đều chuyên về một công việc giống nhau. Sau đó, một công việc được đánh giá rất cao trong khi những công việc khác chỉ nhận được sự hỗ trợ cơ bản. Công thức vẫn được áp dụng rõ ràng và mức công việc tối đa sẽ xác định chính xác liệu công việc chuyên biệt hay những công việc khác chiếm ưu thế về thời gian hoàn thành. 

Cuối cùng, khi tất cả wi đều rất nhỏ, thậm chí một giờ cũng đủ trong hầu hết các trường hợp. Việc chia trần bằng số nguyên đảm bảo rằng mọi yêu cầu khác 0 vẫn được làm tròn chính xác, tránh đánh giá thấp thời gian hoàn thành.
