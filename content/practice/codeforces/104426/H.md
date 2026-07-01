---
title: "CF 104426H - Sinh tố Abo Abdo"
description: "Chúng ta có hai dãy có độ dài $n$. Chuỗi đầu tiên đại diện cho số sinh tố mà Naseem thực sự đã mua và chuỗi thứ hai đại diện cho thứ mà mỗi người bạn lý tưởng nhất muốn nhận."
date: "2026-06-30T19:06:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104426
codeforces_index: "H"
codeforces_contest_name: "Syrian Private Universities Collegiate Programming Contest 2023"
rating: 0
weight: 104426
solve_time_s: 82
verified: false
draft: false
---

[CF 104426H - Sinh tố Abo Abdo](https://codeforces.com/problemset/problem/104426/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi có độ dài$n$. Chuỗi đầu tiên đại diện cho số sinh tố mà Naseem thực sự đã mua và chuỗi thứ hai đại diện cho thứ mà mỗi người bạn lý tưởng nhất muốn nhận. Không có giới hạn nào về việc người bạn nào nhận được loại sinh tố nào, vì vậy chúng ta có thể tự do hoán đổi việc phân công số sinh tố đã mua giữa những người bạn. Mục tiêu là tối đa hóa số lượng bạn bè nhận được loại sinh tố phù hợp chính xác với sở thích của họ. 

Một cách khác để thấy điều này là chúng ta có hai tập hợp kích thước$n$: một đến từ việc mua hàng và một đến từ sở thích. Chúng tôi muốn khớp các phần tử từ phần tử thứ nhất với phần tử thứ hai, tối đa hóa số lượng cặp bằng nhau. 

Những hạn chế$n, m \le 10^5$ngụ ý rằng bất kỳ giải pháp nào tệ hơn thời gian tuyến tính hoặc tuyến tính sẽ không hiệu quả. Một phương pháp so sánh bậc hai giữa tất cả các cặp sẽ liên quan đến$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn. Điều này thúc đẩy chúng ta hướng tới lý luận dựa trên tần số thay vì kết hợp theo cặp. 

Một cạm bẫy tinh vi xuất hiện khi các bản sao tồn tại nhiều ở một bên nhưng không có ở bên kia. Ví dụ: nếu tất cả sinh tố đã mua là loại 1 nhưng bạn bè muốn có nhiều loại khác nhau, một nhiệm vụ tham lam ngây thơ cố gắng khớp theo thứ tự mà không nhóm theo loại có thể dễ dàng đếm thiếu hoặc đếm quá mức tùy thuộc vào thứ tự truyền tải. Một vấn đề khác là giả sử việc căn chỉnh chỉ mục là quan trọng, trong khi thực tế chúng ta được phép hoán vị tùy ý. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo là thử giao sinh tố cho bạn bè bằng mọi cách có thể và đếm xem chúng ta có thể nhận được bao nhiêu quả phù hợp. Điều này tương ứng với việc tạo ra tất cả các hoán vị của phép gán giữa hai mảng. Đối với mỗi hoán vị, chúng tôi tính toán có bao nhiêu vị trí phù hợp. Điều này đúng vì nó khám phá toàn bộ không gian giải pháp, nhưng nó phát triển theo giai thừa, đại khái là$n!$, điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 20$. 

Một cải tiến mạnh mẽ có cấu trúc hơn là sửa thứ tự bạn bè và đối với mỗi người bạn, hãy thử chỉ định bất kỳ sinh tố nào chưa sử dụng, tối đa hóa các kết quả phù hợp một cách đệ quy. Đây vẫn là cấp số nhân vì mỗi bước phân nhánh thành$n$sự lựa chọn. 

Quan sát quan trọng là việc nhận dạng các phần tử chỉ quan trọng thông qua tần số. Đối với một loại sinh tố nhất định$x$, giả sử nó xuất hiện$c_a[x]$lần trong danh sách đã mua và$c_b[x]$lần trong sở thích. Cho dù chúng ta hoán vị như thế nào, chúng ta có thể khớp nhiều nhất$\min(c_a[x], c_b[x])$những người bạn thuộc loại$x$, bởi vì mỗi trận đấu tiêu tốn một sinh tố và một sở thích thuộc loại đó. Ngược lại, giới hạn này có thể đạt được bằng cách ghép đôi một cách tham lam trong mỗi loại. 

Điều này làm giảm vấn đề từ vấn đề gán toàn cục sang tính độc lập trên mỗi giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Khớp tần số |$O(n)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách đếm số lần mỗi loại sinh tố xuất hiện trong cả hai mảng và tính tổng số lần trùng lặp tối thiểu. 

1. Xây dựng bản đồ tần suất cho mảng sinh tố đã mua$a$. Điều này ghi lại số lượng mặt hàng chúng tôi có sẵn cho mỗi loại. 
2. Xây dựng bản đồ tần số cho mảng ưu tiên$b$. Điều này ghi lại số lượng bạn bè muốn có mỗi loại. 
3. Đối với mỗi loại xuất hiện trong một trong hai bản đồ, hãy tính số lượng kết quả trùng khớp ở mức tối thiểu trong hai tần số của nó. 
4. Tích lũy các giá trị tối thiểu này vào câu trả lời cuối cùng và xuất ra kết quả. 

Mỗi bước được thúc đẩy bởi ý tưởng rằng các kết quả khớp độc lập giữa các loại. Sau khi sinh tố được chỉ định, nó không thể được sử dụng lại, do đó việc tính toán theo loại sẽ mô hình chính xác mức tiêu thụ tài nguyên. 

### Tại sao nó hoạt động 

Với mỗi giá trị$x$, mọi phép gán hợp lệ chỉ có thể ghép nhiều nhất một loại sinh tố$x$với một người bạn muốn gõ chữ$x$. Do đó, số lượng cặp hợp lệ cho loại đó bị giới hạn trên bởi cả nguồn cung$c_a[x]$và nhu cầu$c_b[x]$. Lấy mức tối thiểu thỏa mãn cả hai ràng buộc cùng một lúc. Tính tổng tất cả các loại là hợp lệ vì các loại khác nhau không bao giờ cạnh tranh cho cùng một mặt hàng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    freq_a = {}
    freq_b = {}

    for x in a:
        freq_a[x] = freq_a.get(x, 0) + 1

    for x in b:
        freq_b[x] = freq_b.get(x, 0) + 1

    ans = 0
    for x in freq_a:
        if x in freq_b:
            ans += min(freq_a[x], freq_b[x])

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp tách việc đếm thành hai từ điển. Một lựa chọn triển khai tinh tế là chỉ lặp lại các khóa của`freq_a`và kiểm tra sự tồn tại trong`freq_b`, giúp tránh lặp lại toàn bộ phạm vi các loại sinh tố có thể có cho đến$m$, có thể bao gồm các giá trị không được sử dụng. 

Một chi tiết quan trọng khác là sử dụng`get`để cập nhật tần suất nhằm tránh các lỗi chính và giữ cho mã sạch và tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 1 2
2 2 1
```Chúng tôi theo dõi tần số: 

| Loại | tần số_a | tần số_b | phút | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 
| 2 | 1 | 2 | 1 | 

Đáp án = 2 

Điều này chứng tỏ rằng ngay cả khi phân phối bị lệch, việc so khớp hoàn toàn bị giới hạn tần số chứ không phụ thuộc vào thứ tự. 

### Ví dụ 2 

đầu vào:```
5 3
1 1 1 2 3
1 2 2 2 2
```| Loại | tần số_a | tần số_b | phút | 
| --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 
| 2 | 1 | 4 | 1 | 
| 3 | 1 | 0 | 0 | 

Đáp án = 2 

Điều này cho thấy rằng nhu cầu hoặc cung vượt mức trong một danh mục không ảnh hưởng đến những danh mục khác, củng cố tính độc lập giữa các loại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi mảng được quét một lần và việc lặp từ điển là tuyến tính với các giá trị riêng biệt | 
| Không gian |$O(m)$| Bản đồ tần số Số lượng cửa hàng cho mỗi loại sinh tố riêng biệt | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì$n \le 10^5$và các hoạt động từ điển được mong đợi$O(1)$trung bình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    freq_a = {}
    freq_b = {}

    for x in a:
        freq_a[x] = freq_a.get(x, 0) + 1
    for x in b:
        freq_b[x] = freq_b.get(x, 0) + 1

    ans = 0
    for x in freq_a:
        if x in freq_b:
            ans += min(freq_a[x], freq_b[x])

    return str(ans).strip()

# provided sample
assert run("3 2\n1 1 2\n2 2 1\n") == "2"

# all equal
assert run("4 1\n1 1 1 1\n1 1 1 1\n") == "4"

# no overlap
assert run("3 3\n1 1 1\n2 2 2\n") == "0"

# partial overlap
assert run("5 5\n1 2 2 3 3\n2 2 3 4 4\n") == "3"

# single element
assert run("1 10\n5\n5\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 4 | độ bão hòa phù hợp đầy đủ | 
| không chồng chéo | 0 | bộ rời rạc | 
| chồng chéo một phần | 3 | cân bằng tần số hỗn hợp | 
| phần tử đơn | 1 | trường hợp ranh giới tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một loại chỉ xuất hiện ở một bên. Ví dụ: 

đầu vào:```
3 3
1 1 1
2 2 2
```Thuật toán xây dựng`freq_a = {1: 3}`Và`freq_b = {2: 3}`. Vì không có sự giao nhau của các phím nên vòng lặp không đóng góp gì và câu trả lời là 0, phù hợp với thực tế vì không có sở thích nào của bạn bè có thể được thỏa mãn. 

Một trường hợp khác là mất cân bằng nặng: 

đầu vào:```
5 5
1 1 1 1 1
1 1 2 2 2
```Tần số cho: 

loại 1 đóng góp tối thiểu (5, 2) = 2, loại 2 đóng góp ngầm định min(0, 3) = 0, vì vậy câu trả lời là 2. Thuật toán tránh gán quá mức các sinh tố loại 1 bổ sung cho tùy chọn loại 2, vì nó không bao giờ trộn lẫn các loại trên các phím. 

Những trường hợp này xác nhận rằng phân rã dựa trên tần số duy trì tính chính xác ngay cả khi có độ lệch cực lớn.
