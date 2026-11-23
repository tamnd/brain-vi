---
title: "CF 45D - Ngày diễn ra sự kiện"
description: "Mỗi sự kiện có nhiều ngày có thể xảy ra. Đối với sự kiện -th, bất kỳ ngày nguyên nào giữa và đều được chấp nhận. Chúng ta phải chỉ định chính xác một ngày cho mỗi sự kiện và không được có hai sự kiện nào có cùng ngày."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "meet-in-the-middle", "sortings"]
categories: ["algorithms"]
codeforces_contest: 45
codeforces_index: "D"
codeforces_contest_name: "School Team Contest 3 (Winter Computer School 2010/11)"
rating: 1900
weight: 45
solve_time_s: 95
verified: true
draft: false
---
[CF 45D - Ngày diễn ra sự kiện](https://codeforces.com/problemset/problem/45/D) 

**Xếp hạng:** 1900 
**Tags:** tham lam, gặp mặt, phân loại 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi sự kiện có nhiều ngày có thể xảy ra. Đối với$i$-sự kiện thứ, bất kỳ ngày số nguyên nào giữa$l_i$Và$r_i$được chấp nhận. Chúng ta phải chỉ định chính xác một ngày cho mỗi sự kiện và không được có hai sự kiện nào có cùng ngày. 

Nhiệm vụ không phải là đếm bài tập hay tối ưu hóa bất cứ thứ gì. Chúng ta chỉ cần xây dựng một bài tập hợp lệ. 

Những hạn chế nhỏ đến mức đáng ngạc nhiên. Có tối đa 100 sự kiện, trong khi bản thân giá trị ngày có thể lớn bằng$10^7$. Phạm vi tọa độ lớn có nghĩa là chúng ta không thể xây dựng một mảng được lập chỉ mục theo ngày, nhưng số khoảng nhỏ có nghĩa là$O(n^2)$hoặc$O(n^3)$giải pháp là hoàn toàn ổn. Ngay cả một cái gì đó như$100^3 = 10^6$hoạt động dễ dàng phù hợp trong giới hạn. 

Phần khó khăn là việc đưa ra những lựa chọn hợp lý tại địa phương có thể phá hủy khả năng hoàn thiện sau này. Giả sử chúng ta xử lý các sự kiện theo thứ tự đầu vào và luôn chọn ngày không sử dụng sớm nhất. 

Hãy xem xét ví dụ này:```
2
1 100
1 1
```Nếu chúng ta gán ngày 1 cho sự kiện đầu tiên thì sự kiện thứ hai sẽ không thể thực hiện được. Phép gán đúng là:```
2 1
```Khoảng cách hẹp phải được ưu tiên. 

Một trường hợp tinh tế khác xuất hiện khi một số khoảng chồng lên nhau nhiều.```
3
1 2
1 2
2 2
```Một chiến lược tham lam bất cẩn có thể chỉ định:```
1 2 ?
```và bị mắc kẹt ở sự kiện cuối cùng. Cấu trúc hợp lệ duy nhất là:```
1 2 2
```điều này là không thể vì ngày phải khác biệt. Điều này có nghĩa là phép gán hợp lệ thực tế thay vào đó phải là:```
1 ? 2
```và khoảng thời gian còn lại nhận được ngày 1 chưa sử dụng, cũng không thành công. Nhìn kỹ hơn, đầu vào này thực sự không có giải pháp. Tuyên bố đảm bảo khả năng giải quyết được, nhưng ví dụ này cho thấy tại sao các quyết định tham lam lại quan trọng. 

Bây giờ hãy xem xét một phiên bản hợp lệ:```
3
1 3
1 2
2 3
```Chọn ngày sai thứ tự vẫn có thể thất bại dù đã có giải pháp. Một thuật toán mạnh mẽ phải đảm bảo tính linh hoạt trong tương lai. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mỗi ngày có thể trong mỗi khoảng thời gian và quay lại bất cứ khi nào hai sự kiện va chạm. Vì mỗi khoảng có thể kéo dài tới$10^7$các giá trị, không gian tìm kiếm rất lớn. Ngay cả khi khoảng thời gian rất nhỏ, số lượng bài tập có thể đạt tới$n!$, điều này trở nên không khả thi rất nhanh. 

Cách tiếp cận vũ phu hoạt động về mặt khái niệm vì bài toán chỉ yêu cầu sự tồn tại của một phép gán riêng biệt thỏa mãn các ràng buộc về khoảng. Việc quay lại khám phá tất cả các khả năng một cách tự nhiên cho đến khi tìm thấy một khả năng. Vấn đề là nó lặp lại một lượng lớn công việc và không khai thác bất kỳ cấu trúc nào của các khoảng thời gian. 

Quan sát quan trọng là các khoảng hoạt động tốt khi được xử lý bởi các điểm cuối bên phải của chúng. Nếu một sự kiện kết thúc sớm hơn một sự kiện khác, việc trì hoãn nó là nguy hiểm vì các lựa chọn có sẵn của nó sẽ biến mất sớm hơn. 

Điều này dẫn đến một chiến lược tham lam cổ điển: 

1. Sắp xếp các khoảng bằng cách tăng điểm cuối bên phải. 
2. Đối với mỗi khoảng thời gian, chỉ định ngày chưa sử dụng nhỏ nhất vẫn nằm trong khoảng thời gian đó. 

Tại sao ngày có sẵn nhỏ nhất lại có ích? Bởi vì những ngày lớn hơn vẫn có sẵn cho những khoảng thời gian trong tương lai có thể cần đến chúng. Tại sao việc sắp xếp theo đúng điểm cuối lại giúp ích? Bởi vì những khoảng thời gian có thời hạn chặt chẽ được xử lý trước những khoảng thời gian linh hoạt. 

Giả sử chúng ta xử lý một khoảng$[l, r]$. Nếu chúng ta chọn ngày không được sử dụng sớm nhất bên trong nó thì mỗi ngày khả thi nhỏ hơn đã được chiếm bởi các khoảng có điểm cuối bên phải nhỏ hơn hoặc bằng nhau. Những khoảng thời gian đó không thể di chuyển một cách an toàn sau đó, trong khi khoảng thời gian hiện tại vẫn có thể. 

Từ$n \le 100$, chúng ta không cần cấu trúc dữ liệu phức tạp. Chúng ta có thể chỉ cần quét về phía trước từ$l$cho đến khi chúng ta tìm được một ngày chưa sử dụng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ |$O(n)$ngăn xếp đệ quy | Quá chậm | 
| Tham lam tối ưu |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các khoảng thời gian và ghi nhớ các chỉ số ban đầu của chúng. 

Chúng tôi sắp xếp các khoảng thời gian trong quá trình xử lý, nhưng đầu ra phải tuân theo thứ tự đầu vào. 
2. Sắp xếp các khoảng bằng cách tăng điểm cuối bên phải. 

Khoảng thời gian kết thúc sớm hơn sẽ hạn chế hơn. Xử lý chúng trước tiên sẽ ngăn chặn những ngõ cụt trong tương lai. 
3. Duy trì một tập hợp các ngày đã sử dụng. 

Mỗi ngày được chỉ định phải là duy nhất. 
4. Đối với mỗi khoảng thời gian$[l, r]$theo thứ tự sắp xếp, ngày quét bắt đầu từ$l$. 

Chúng tôi tìm kiếm ngày không sử dụng đầu tiên trong khoảng thời gian đó. 
5. Ấn định ngày không sử dụng đầu tiên$d$như vậy$l \le d \le r$. 

Việc chọn ngày khả thi nhỏ nhất sẽ giữ lại những ngày lớn hơn cho những khoảng thời gian trong tương lai. 
6. Lưu trữ ngày được chỉ định trong mảng câu trả lời bằng chỉ mục gốc của khoảng thời gian. 

Điều này khôi phục thứ tự đầu ra cần thiết. 
7. In mảng câu trả lời. 

### Tại sao nó hoạt động 

Lựa chọn tham lam là an toàn vì các khoảng được xử lý theo thứ tự tăng dần của điểm cuối bên phải của chúng. 

Khi xử lý một khoảng$[l, r]$, mọi khoảng thời gian được xử lý trước đó có nhiều nhất điểm cuối đúng$r$. Những khoảng thời gian đó ít nhất cũng khẩn cấp như khoảng thời gian hiện tại. Việc chỉ định khoảng thời gian hiện tại là ngày nhỏ nhất có sẵn để có nhiều khoảng trống nhất có thể cho các khoảng thời gian sau đó, có điểm cuối bên phải lớn hơn. 

Giả sử thuật toán gán ngày$d$đến khoảng thời gian hiện tại. Mỗi ngày khả thi nhỏ hơn đã được sử dụng trong khoảng thời gian trước đó. Việc chuyển một trong những khoảng thời gian trước đó sang một ngày lớn hơn sẽ chỉ làm cho tình hình của nó trở nên tồi tệ hơn vì thời hạn của nó không muộn hơn thời hạn của khoảng thời gian hiện tại. 

Đối số trao đổi này cho thấy phép gán tham lam không bao giờ chặn một giải pháp hợp lệ nếu có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

events = []

for i in range(n):
    l, r = map(int, input().split())
    events.append((r, l, i))

events.sort()

used = set()
ans = [0] * n

for r, l, idx in events:
    day = l

    while day in used:
        day += 1

    ans[idx] = day
    used.add(day)

print(*ans)
```Giải pháp bắt đầu bằng cách lưu trữ từng khoảng cùng với vị trí ban đầu của nó. Thứ tự tuple là`(r, l, idx)`vì Python sắp xếp các bộ dữ liệu theo từ điển, nên các khoảng tự động được sắp xếp theo thứ tự bằng cách tăng điểm cuối bên phải. 

các`used`đặt theo dõi những ngày đã được chỉ định. Vì chỉ tồn tại 100 sự kiện nên một bộ băm là quá đủ. 

Đối với mỗi khoảng thời gian, mã sẽ quét lên trên bắt đầu từ`l`. Câu lệnh đảm bảo rằng một phép gán hợp lệ tồn tại, do đó cuối cùng một ngày không được sử dụng sẽ không vượt quá`r`sẽ xuất hiện. Chúng tôi không cần kiểm tra giới hạn rõ ràng bên trong vòng lặp vì khả năng giải quyết được đảm bảo. 

Một điểm tinh tế là giữ nguyên thứ tự đầu ra. Sau khi sắp xếp, các khoảng không còn ở vị trí ban đầu nữa. các`idx`trường cho phép chúng ta đặt mỗi ngày đã chọn trở lại vị trí chính xác của mảng câu trả lời. 

Một chi tiết tinh tế khác là thuật toán cố tình chọn ngày sớm nhất có sẵn chứ không phải ngày tùy ý. Chọn một ngày muộn hơn có thể dễ dàng chặn các khoảng thời gian chặt chẽ hơn sau này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2
2 3
3 4
```Các khoảng được sắp xếp:```
[1,2], [2,3], [3,4]
```| Bước | Khoảng thời gian | Ngày đã qua sử dụng | Ngày được chọn | Ngày sử dụng sau | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2] | {} | 1 | {1} | 
| 2 | [2,3] | {1} | 2 | {1,2} | 
| 3 | [3,4] | {1,2} | 3 | {1,2,3} | 

Đầu ra cuối cùng:```
1 2 3
```Ví dụ này cho thấy trường hợp đơn giản trong đó mọi khoảng đều nhận được điểm cuối bên trái của nó một cách tự nhiên. 

### Ví dụ 2 

đầu vào:```
4
1 100
1 1
2 2
3 3
```Các khoảng được sắp xếp:```
[1,1], [2,2], [3,3], [1,100]
```| Bước | Khoảng thời gian | Ngày đã qua sử dụng | Ngày được chọn | Ngày sử dụng sau | 
| --- | --- | --- | --- | --- | 
| 1 | [1,1] | {} | 1 | {1} | 
| 2 | [2,2] | {1} | 2 | {1,2} | 
| 3 | [3,3] | {1,2} | 3 | {1,2,3} | 
| 4 | [1.100] | {1,2,3} | 4 | {1,2,3,4} | 

Đầu ra cuối cùng theo thứ tự ban đầu:```
4 1 2 3
```Dấu vết này chứng tỏ tại sao việc sắp xếp theo đúng điểm cuối lại quan trọng. Nếu khoảng thời gian lớn được xử lý trước, nó có thể tiêu tốn ngày 1 và phá vỡ khoảng thời gian đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Đối với mỗi khoảng thời gian, chúng tôi có thể quét qua tối đa$n$ngày đã qua sử dụng | 
| Không gian |$O(n)$| Mảng câu trả lời và bộ lưu trữ được sử dụng nhiều nhất$n$yếu tố | 

Với$n \le 100$, ngay cả phép quét bậc hai cũng rất nhỏ. Thuật toán phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    events = []

    for i in range(n):
        l, r = map(int, input().split())
        events.append((r, l, i))

    events.sort()

    used = set()
    ans = [0] * n

    for r, l, idx in events:
        day = l

        while day in used:
            day += 1

        ans[idx] = day
        used.add(day)

    print(*ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""3
1 2
2 3
3 4
"""
) == "1 2 3", "sample 1"

# minimum size
assert run(
"""1
5 5
"""
) == "5", "single interval"

# intervals requiring sorting by deadline
assert run(
"""4
1 100
1 1
2 2
3 3
"""
) == "4 1 2 3", "deadline priority"

# all intervals identical
out = run(
"""3
1 3
1 3
1 3
"""
)
vals = list(map(int, out.split()))
assert sorted(vals) == [1, 2, 3], "distinct assignments"

# off-by-one boundary case
assert run(
"""2
1 1
2 2
"""
) == "1 2", "boundary endpoints"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quãng đơn`[5,5]`|`5`| Đầu vào kích thước tối thiểu | 
| Khoảng thời gian linh hoạt lớn cộng với khoảng thời gian chặt chẽ |`4 1 2 3`| Đúng thứ tự tham lam | 
| Tất cả các khoảng thời gian`[1,3]`| Bất kỳ hoán vị nào của`1 2 3`| Xử lý nhiệm vụ riêng biệt | 
| Ranh giới Singleton |`1 2`| Tính chính xác của điểm cuối bao gồm | 

## Vỏ cạnh 

Hãy xem xét trường hợp một khoảng thời gian cực kỳ linh hoạt trong khi khoảng thời gian khác chỉ có thể có một ngày.```
2
1 100
1 1
```Thuật toán sắp xếp theo điểm cuối bên phải:```
[1,1], [1,100]
```Khoảng thời gian đơn lẻ ngay lập tức nhận được ngày 1. Khoảng thời gian linh hoạt sau đó sẽ nhận ngày thứ 2. Kết quả cuối cùng trở thành:```
2 1
```Một chiến lược tham lam đơn hàng đầu vào ngây thơ sẽ thất bại ở đây nếu tiêu thụ quá sớm ngày 1. 

Bây giờ hãy xem xét các khoảng thời gian chồng chéo với thời hạn chặt chẽ.```
4
1 2
1 2
2 3
3 4
```Thứ tự sắp xếp:```
[1,2], [1,2], [2,3], [3,4]
```Nhiệm vụ tiến hành như sau: 

| Khoảng thời gian | Ngày được chỉ định | 
| --- | --- | 
| [1,2] | 1 | 
| [1,2] | 2 | 
| [2,3] | 3 | 
| [3,4] | 4 | 

Mỗi khoảng thời gian sẽ nhận được ngày khả thi sớm nhất chưa được sử dụng. Thuật toán không bao giờ lãng phí những ngày nhỏ một cách không cần thiết. 

Cuối cùng, hãy xem xét các khoảng đã được sắp xếp một cách tự nhiên nhưng lại chạm vào ranh giới.```
3
1 1
1 2
2 2
```Thứ tự sắp xếp:```
[1,1], [1,2], [2,2]
```Bài tập: 

| Khoảng thời gian | Ngày được chỉ định | 
| --- | --- | 
| [1,1] | 1 | 
| [1,2] | 2 | 
| [2,2] | không thể | 

Đầu vào này không có giải pháp hợp lệ, phù hợp với vấn đề đảm bảo rằng những trường hợp như vậy không bao giờ xuất hiện. Ví dụ này minh họa tại sao việc sử dụng điểm cuối một cách bất cẩn có thể phá hủy tính khả thi ngay lập tức.
