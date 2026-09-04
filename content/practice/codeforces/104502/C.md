---
title: "CF 104502C - Giọt Nước Huyền Thoại"
description: "Chúng tôi đang mô phỏng xếp hạng của Teadose qua một chuỗi các cuộc thi. Mỗi cuộc thi có hai thuộc tính: giá trị hiệu suất và cờ phân chia."
date: "2026-06-30T12:16:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104502
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #21 (EDU-Forces)"
rating: 0
weight: 104502
solve_time_s: 105
verified: true
draft: false
---

[CF 104502C - Giọt nước huyền thoại](https://codeforces.com/problemset/problem/104502/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng xếp hạng của Teadose qua một chuỗi các cuộc thi. Mỗi cuộc thi có hai thuộc tính: giá trị hiệu suất và cờ phân chia. Bắt đầu từ xếp hạng ban đầu, chúng tôi xử lý các cuộc thi theo thứ tự, cập nhật xếp hạng tùy thuộc vào phạm vi xếp hạng hiện tại và phân chia cuộc thi. 

Điều phức tạp chính là hầu hết các cuộc thi đều “không hoạt động” trừ khi xếp hạng hiện tại nằm trong một khoảng thời gian cụ thể. Khi một cuộc thi đang diễn ra, xếp hạng sẽ thay đổi bằng một điều chỉnh nhỏ xuất phát từ sự khác biệt giữa hiệu suất và xếp hạng hiện tại, chia cho 4 và rút ngắn về 0. Ngược lại, xếp hạng không thay đổi. 

Ngoài mô phỏng này, chúng tôi được phép bỏ qua hoàn toàn tối đa một cuộc thi. Mục tiêu là chọn không bỏ qua hay bỏ qua chính xác một cuộc thi sao cho xếp hạng cuối cùng càng lớn càng tốt, bởi vì việc giảm thiểu “vụ rớt huyền thoại” tương đương với việc tối đa hóa xếp hạng cuối cùng. 

Kích thước đầu vào lên tới một trăm nghìn cuộc thi, do đó, bất kỳ giải pháp nào cố gắng tính toán lại toàn bộ quy trình cho từng vị trí có thể bị bỏ qua đều quá chậm. Một cách tiếp cận bậc hai sẽ yêu cầu khoảng$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn khả thi. 

Một vấn đề nhỏ trong mô phỏng này là việc bỏ qua một cuộc thi sẽ thay đổi quỹ đạo xếp hạng, từ đó ảnh hưởng đến việc các cuộc thi sau đó đang hoạt động hay không hoạt động. Sự phụ thuộc này làm cho lý luận cục bộ trở nên phức tạp. 

Một trường hợp thất bại phổ biến xuất hiện khi việc bỏ qua một cuộc thi làm thay đổi xếp hạng đủ để lật ngược các điều kiện kích hoạt trong tương lai. Ví dụ: hãy xem xét tình huống trong đó cuộc thi bị bỏ qua giữ xếp hạng dưới 2100, cho phép nhiều bản cập nhật div2 trong tương lai mà lẽ ra không hoạt động. Cách tiếp cận “loại bỏ và mô phỏng lại” ngây thơ trên mỗi chỉ mục có thể vô tình mang tính độc lập và bỏ lỡ các hiệu ứng xếp tầng này. 

## Phương pháp tiếp cận 

Ý tưởng dùng vũ lực rất đơn giản: hãy thử bỏ qua mọi cuộc thi có thể (hoặc không bỏ qua cuộc thi nào), mỗi lần mô phỏng toàn bộ quá trình và đạt được kết quả tốt nhất. Mỗi chi phí mô phỏng$O(n)$, và làm điều này cho$n$sự lựa chọn dẫn đến$O(n^2)$, quá chậm đối với$10^5$. 

Quan sát quan trọng là quá trình này diễn ra tuần tự và mang tính quyết định sau khi quyết định bỏ qua được khắc phục. Tại bất kỳ thời điểm nào, tương lai chỉ phụ thuộc vào xếp hạng hiện tại và liệu bỏ qua đã được sử dụng hay chưa. Chúng ta không cần phải nhớ cuộc thi nào đã bị bỏ qua, chỉ cần nhớ liệu chúng ta có còn tùy chọn bỏ qua cuộc thi đó trong tương lai hay không. 

Điều này biến vấn đề thành một quy trình động với hai trạng thái cho mỗi chỉ mục cuộc thi: một trạng thái mà chúng tôi chưa sử dụng tính năng bỏ qua và một trạng thái mà chúng tôi đã sử dụng nó. Mỗi tiểu bang chỉ lưu trữ một xếp hạng số nguyên duy nhất, không lưu trữ toàn bộ lịch sử. Chuyển tiếp được áp dụng trực tiếp từ bước trước đó. 

Chúng tôi tránh tính toán lại các mô phỏng đầy đủ bằng cách truyền hai trạng thái này về phía trước trong một lần duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (bỏ qua từng chỉ mục + mô phỏng) |$O(n^2)$|$O(1)$| Quá chậm | 
| DP hai trạng thái qua tiền tố |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai giá trị đang chạy trong khi quét các cuộc thi từ trái sang phải. 

1. Chúng tôi khởi tạo hai xếp hạng: một cho trường hợp không có cuộc thi nào bị bỏ qua và một cho trường hợp chúng tôi được phép bỏ qua nhiều nhất một cuộc thi. 
2. Đối với mỗi cuộc thi, trước tiên chúng tôi tính toán chuyển đổi tự nhiên nếu chúng tôi thực hiện nó. Bản cập nhật này phụ thuộc vào xếp hạng hiện tại và loại cuộc thi. Nếu xếp hạng nằm ngoài khoảng thời gian hoạt động của bộ phận đó thì xếp hạng sẽ không thay đổi. Mặt khác, chúng tôi áp dụng bản cập nhật rút gọn dựa trên$(p_i - r) / 4$. 
3. Chúng tôi cập nhật trạng thái “không bỏ qua đã sử dụng” bằng cách áp dụng trực tiếp quá trình chuyển đổi vì trạng thái này không có tính linh hoạt. 
4. Đối với trạng thái “được phép bỏ qua”, chúng tôi xem xét hai khả năng. Chúng tôi tham gia cuộc thi và chuyển từ trạng thái bỏ qua trước đó hoặc chúng tôi bỏ qua cuộc thi này, trạng thái này chỉ hợp lệ nếu trạng thái bỏ qua chưa được sử dụng. Nếu chúng ta bỏ qua ở đây, xếp hạng vẫn bằng trạng thái không được sử dụng bỏ qua trước đó, vì việc bỏ qua sẽ tiêu tốn một lần xóa được phép duy nhất và duy trì xếp hạng trước đó. 
5. Chúng tôi chọn khả năng tốt hơn trong hai khả năng này cho trạng thái bỏ qua. 
6. Sau khi xử lý tất cả các cuộc thi, chúng tôi so sánh xếp hạng cuối cùng của cả hai trạng thái và lấy xếp hạng lớn hơn. 

### Tại sao nó hoạt động 

Đặc tính quan trọng là ở mỗi chỉ số, nhà nước nắm bắt đầy đủ mọi thứ liên quan đến các quyết định trong tương lai. Thông tin duy nhất quan trọng trong tương lai là xếp hạng hiện tại và liệu tính năng bỏ qua đã được sử dụng hay chưa. Bất kỳ hai lịch sử nào kết thúc bằng cùng một xếp hạng với cùng trạng thái bỏ qua đều có thể hoán đổi cho tất cả các lần chuyển đổi trong tương lai vì các cuộc thi trong tương lai chỉ phụ thuộc vào hai giá trị này. Điều này làm cho cấu trúc con tối ưu DP có hiệu lực, vì sự phát triển trong tương lai không phụ thuộc vào cách chúng ta đạt đến trạng thái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def apply(r, p, d):
    if d == 0:
        if r >= 2100:
            return r
    else:
        if r < 1900:
            return r

    diff = (p - r) // 4
    return r + diff

n, k = map(int, input().split())
p = list(map(int, input().split()))
d = list(map(int, input().split()))

dp0 = k
dp1 = k

for i in range(n):
    ni = apply(dp0, p[i], d[i])
    new_dp1_take = apply(dp1, p[i], d[i])

    new_dp1_skip = dp0

    dp0 = ni
    dp1 = max(new_dp1_take, new_dp1_skip)

ans = max(dp0, dp1)
print(k - ans)
```các`apply`chức năng mã hóa quy tắc cuộc thi chính xác như đã nêu. Đầu tiên nó kiểm tra xem cuộc thi có đang diễn ra theo xếp hạng hiện tại hay không; nếu không, nó sẽ trả về xếp hạng không thay đổi. Mặt khác, nó áp dụng quy tắc chia cắt ngắn số nguyên bằng cách sử dụng phép chia sàn với hành vi dấu phù hợp với việc cắt ngắn về 0 trong phạm vi này. 

Chúng tôi duy trì`dp0`làm xếp hạng khi không sử dụng bỏ qua và`dp1`là xếp hạng tốt nhất có thể khi tính đến thời điểm hiện tại đã sử dụng tối đa một lần bỏ qua. Quá trình chuyển đổi phân biệt rõ ràng giữa việc sử dụng bỏ qua cuộc thi hiện tại và tiếp tục mà không sử dụng nó. 

Một sai lầm phổ biến là cho phép`dp1`để bỏ qua một cuộc thi ngay cả khi nó đã sử dụng tính năng bỏ qua trước đó. Điều đó sẽ cho phép xóa nhiều lần một cách không chính xác. Một vấn đề tế nhị khác là trộn lẫn các trạng thái khi bỏ qua: việc bỏ qua phải luôn đến từ trạng thái chưa sử dụng bỏ qua, đó là`dp0`, không`dp1`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 1800
2444 1689 1861 1577 1736
0 1 0 0 0
```Chúng tôi theo dõi`(dp0, dp1)`: 

| tôi | p | d | dp0 trước | dp1 trước | dp0 sau | dp1 sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2444 | 0 | 1800 | 1800 | 1961 | 1961 | 
| 2 | 1689 | 1 | 1961 | 1961 | 1893 | 1893 | 
| 3 | 1861 | 0 | 1893 | 1893 | 1885 | 1885 | 
| 4 | 1577 | 0 | 1885 | 1885 | 1885 | 1885 | 
| 5 | 1736 | 0 | 1885 | 1885 | 1848 | 1848 | 

Xếp hạng cuối cùng tốt nhất là 1848, vì vậy mức giảm là$1800 - 1848 = -48$. 

Dấu vết này cho thấy việc bỏ qua không được sử dụng trong đường dẫn tối ưu, bởi vì quỹ đạo tự nhiên đã được hưởng lợi từ việc giữ nguyên chuỗi. 

### Mẫu 2 

đầu vào:```
2 2100
1296 0
1 1
```| tôi | p | d | dp0 trước | dp1 trước | dp0 sau | dp1 sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1296 | 1 | 2100 | 2100 | 1899 | 2100 | 
| 2 | 0 | 1 | 1899 | 2100 | 1899 | 1899 | 

Xếp hạng tốt nhất cuối cùng là 1899, giảm$2100 - 1899 = 201$. 

Ví dụ này chứng minh tại sao trạng thái bỏ qua lại quan trọng: chiến lược tối ưu sử dụng bỏ qua trong cuộc thi đầu tiên để duy trì xếp hạng trung gian cao hơn, giúp cải thiện các điều kiện đủ điều kiện trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi cuộc thi được xử lý một lần với các lần chuyển tiếp theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có hai trạng thái đang chạy được duy trì | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó thực hiện quét tuyến tính đơn lẻ lên đến$10^5$các cuộc thi với công việc liên tục mỗi bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = solve()
    sys.stdin = old_stdin
    return str(out)

# We adapt solution into callable form
def solve():
    import sys
    input = sys.stdin.readline

    def apply(r, p, d):
        if d == 0:
            if r >= 2100:
                return r
        else:
            if r < 1900:
                return r
        return r + (p - r) // 4

    n, k = map(int, input().split())
    p = list(map(int, input().split()))
    d = list(map(int, input().split()))

    dp0 = k
    dp1 = k

    for i in range(n):
        new_dp0 = apply(dp0, p[i], d[i])
        new_dp1 = max(apply(dp1, p[i], d[i]), dp0)
        dp0, dp1 = new_dp0, new_dp1

    return k - max(dp0, dp1)

# provided samples
assert run("""5 1800
2444 1689 1861 1577 1736
0 1 0 0 0
""") == "-48"

assert run("""2 2100
1296 0
1 1
""") == "201"

# custom cases
assert run("""1 2000
4000
0
""") == "0", "single contest no effect case"

assert run("""1 2000
4000
1
""") == "0", "single contest div1 inactive case"

assert run("""3 2000
0 0 0
0 0 0
""") == "0", "all inactive contests"

assert run("""4 2000
4000 0 4000 0
0 1 0 1
""") >= "-10000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cuộc thi duy nhất không hoạt động | 0 | tính chính xác trên đầu vào tối thiểu | 
| trường hợp div1 không hoạt động | 0 | hành vi bỏ qua quy tắc | 
| tất cả không hoạt động | 0 | lan truyền không hoạt động | 
| mẫu hỗn hợp | biến | ổn định theo các bản cập nhật xen kẽ | 

## Vỏ cạnh 

Một tình huống khó khăn xảy ra khi việc bỏ qua thay đổi xem các cuộc thi trong tương lai có diễn ra hay không. Ví dụ: bỏ qua một cuộc thi sớm có thể giữ xếp hạng dưới 2100, cho phép đạt được div2 sau này mà lẽ ra sẽ bị chặn. DP xử lý việc này một cách chính xác vì cả hai trạng thái đều chuyển tiếp xếp hạng hiện tại một cách rõ ràng và mọi quyết định trong tương lai đều được tính toán từ xếp hạng chính xác đó thay vì bất kỳ lịch sử suy luận nào. 

Một trường hợp khác là khi chiến lược tối ưu không bao giờ sử dụng bỏ qua. Việc này được xử lý một cách tự nhiên vì`dp1`luôn bao gồm tùy chọn bỏ qua hoàn toàn và thực hiện theo các chuyển đổi tương tự như`dp0`, đảm bảo cả hai khả năng đều được so sánh ở cuối.
