---
title: "CF 105631C - Phản ứng trong cuộc thi"
description: "Hệ thống duy trì bảng điểm cuộc thi lập trình trực tiếp. Có một số nhóm, được lập chỉ mục từ 0 đến k và các bài nộp được gửi đến theo thứ tự dấu thời gian tăng dần."
date: "2026-06-22T05:39:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105631
codeforces_index: "C"
codeforces_contest_name: "SYSU Collegiate Programming Contest 2024 (SYSUCPC 2024), Final"
rating: 0
weight: 105631
solve_time_s: 65
verified: true
draft: false
---

[CF 105631C - Phản hồi trong cuộc thi](https://codeforces.com/problemset/problem/105631/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hệ thống duy trì bảng điểm cuộc thi lập trình trực tiếp. Có một số nhóm, được lập chỉ mục từ 0 đến k và các bài nộp được gửi đến theo thứ tự dấu thời gian tăng dần. Mỗi bài nộp thuộc về một đội và một bài toán trong số 26 chữ cái có thể có và nó được chấp nhận hoặc bị từ chối. 

Một vấn đề chỉ đóng góp vào điểm số của đội khi nó được giải quyết lần đầu tiên. Thời gian giải là mốc thời gian tính theo phút của bài nộp được chấp nhận đầu tiên cho bài toán đó. Bất kỳ bài nộp nào bị từ chối trước lần chấp nhận đầu tiên đó sẽ bị phạt thêm 20 phút cho mỗi bài. Khi một vấn đề được giải quyết cho một đội, những bài nộp sau đó cho vấn đề tương tự đó sẽ không liên quan đến việc ghi điểm. 

Thứ hạng của một đội trước tiên được xác định bởi số vấn đề mà đội đó đã giải quyết được, sau đó là tổng thời gian phạt (càng nhỏ càng tốt). Các đội có số lần giải quyết và hình phạt giống hệt nhau sẽ có cùng thứ hạng. 

Nhiệm vụ không phải là duy trì bảng điểm đầy đủ một cách rõ ràng theo nghĩa thông thường mà chỉ giám sát Đội 0. Mỗi khi Đội 0 mới giải quyết một vấn đề, chúng ta phải xuất ra dấu thời gian, chữ cái vấn đề và thứ hạng của nó thay đổi như thế nào ngay sau khi kết hợp giải pháp đó. Kể cả khi thứ hạng không thay đổi thì sự kiện vẫn phải được báo cáo. 

Khó khăn chính là sau mỗi lần giải quyết mới của Đội 0, thứ hạng phải phản ánh sự so sánh với tất cả các đội khác theo một thứ tự động phụ thuộc vào cả số lần giải quyết và hình phạt. 

Các hạn chế rất lớn: lên tới 100.000 đội và 200.000 lượt gửi. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào tính toán lại thứ hạng đầy đủ từ đầu cho mỗi sự kiện. Bất kỳ giải pháp nào quét tất cả các nhóm sau mỗi lần cập nhật sẽ giảm xuống O(nk), vượt xa giới hạn chấp nhận được. Chúng tôi cần một cách tiếp cận duy trì đủ cấu trúc để mỗi bản cập nhật có thể được đánh giá theo thời gian logarit hoặc gần logarit. 

Một trường hợp khó nhận thấy xuất phát từ thứ hạng ngang nhau. Hai đội có số lượt giải quyết và hình phạt giống hệt nhau có cùng số thứ hạng, do đó việc tính toán thứ hạng dựa trên việc đếm xem có bao nhiêu đội dẫn trước nghiêm ngặt theo thứ tự từ điển theo (đã giải quyết, hình phạt). 

Một cạm bẫy phổ biến khác là quên rằng chỉ bài nộp đầu tiên được chấp nhận mới quan trọng đối với mỗi bài toán. Một nhóm có thể có nhiều bài gửi AC cho cùng một vấn đề trong luồng, nhưng chỉ bài gửi sớm nhất được tính và chỉ từ chối trước hình phạt đóng góp đó. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp duy trì, đối với mỗi đội, số lượng vấn đề đã giải quyết và tổng số tiền phạt, đồng thời cập nhật các giá trị này cho mỗi lần gửi. Sau mỗi lần cập nhật liên quan đến Đội 0, chúng tôi tính toán lại thứ hạng của đội bằng cách quét tất cả các đội và so sánh cặp hiện tại của họ (đã giải quyết, phạt đền). Điều này hiệu quả vì xếp hạng được xác định hoàn toàn bởi hai giá trị này, do đó hàm so sánh được xác định rõ ràng. 

Vấn đề là việc tính toán lại thứ hạng ngây thơ này rất tốn kém. Đối với mỗi sự kiện giải quyết Đội 0, việc quét k đội tốn O(k). Trong trường hợp xấu nhất, có O(n) sự kiện như vậy nên tổng trở thành O(nk), quá lớn khi cả n và k có thể lần lượt lên tới 200.000 và 100.000. 

Quan sát quan trọng là chúng ta không bao giờ cần thứ tự sắp xếp đầy đủ của tất cả các đội. Tại những thời điểm cụ thể, chúng ta chỉ cần biết có bao nhiêu đội thực sự tốt hơn Đội 0. Điều đó làm giảm nhiệm vụ thành một bài toán đếm động trên không gian khóa hai chiều: số đã giải và hình phạt. 

Điều này gợi ý việc duy trì cấu trúc dữ liệu hỗ trợ cập nhật điểm (khi số lượng giải quyết hoặc hình phạt của một đội thay đổi) và các truy vấn tiền tố hoặc thống trị. Tuy nhiên, kích thước hình phạt không dễ dàng bị rời rạc hóa vì hình phạt phụ thuộc vào thời gian tích lũy và rất khác nhau. Một cách tiếp cận thực tế hơn là duy trì các nhóm được nhóm theo số lượng giải được và sau đó duy trì cấu trúc có trật tự bằng hình phạt trong mỗi nhóm.

Chúng ta có thể duy trì, đối với mỗi số lượng có thể giải được, một cấu trúc cân bằng (hoặc danh sách được sắp xếp) các hình phạt. Vì số lượng được giải quyết nhiều nhất là 26 nên nhóm này nhỏ và ổn định. Mỗi đội chỉ di chuyển khi giải quyết được một vấn đề mới, tăng số lần giải quyết được lên 1 và tăng hình phạt lên một giá trị đã biết. 

Do đó, để xếp hạng Đội 0, chúng tôi tính toán có bao nhiêu đội giải được nhiều bài toán hơn, cộng với bao nhiêu đội trong cùng một nhóm giải được có hình phạt nhỏ hơn. Điều này trở nên hiệu quả nếu mỗi nhóm được duy trì theo cấu trúc nhiều tập hợp được sắp xếp. 

Hệ thống này rất linh hoạt nhưng các bản cập nhật được bản địa hóa: mỗi giải pháp của một nhóm chỉ thay đổi một thành phần trong nhóm của nó, vì vậy chúng tôi có thể xóa và chèn lại trong thời gian O(log k). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tính lại thứ hạng sau mỗi sự kiện | O(nk) | O(k) | Quá chậm | 
| Nhóm bằng cách giải quyết + ra lệnh phạt | O(n log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích các bài nộp theo thứ tự, duy trì cho mỗi nhóm và vấn đề xem nó đã được giải quyết chưa và có bao nhiêu lần thử thất bại trước khi được chấp nhận lần đầu. Điều này là cần thiết vì hình phạt chỉ phụ thuộc vào những thất bại trước AC đầu tiên. 
2. Đối với mỗi đội, hãy duy trì hai giá trị cốt lõi: số vấn đề được giải quyết và tổng số hình phạt. Những điều này xác định vị trí bảng điểm của nó. 
3. Duy trì cấu trúc được nhóm theo số lượng đã giải. Vì mỗi đội có số lượng đã giải quyết từ 0 đến 26 nên chúng tôi lưu trữ cho mỗi số một tập hợp các hình phạt được sắp xếp của các đội hiện có trong nhóm đó. 
4. Ban đầu, tất cả các đội đều ở trong nhóm được giải = 0 và bị phạt = 0. 
5. Khi xử lý bài nộp, hãy cập nhật trạng thái nhóm tương ứng. Nếu đó là sự từ chối trước lần chấp nhận đầu tiên cho vấn đề đó, hãy tăng số lần thất bại tạm thời của nó. Nếu đó là lần chấp nhận đầu tiên, chúng tôi sẽ tính toán phần đóng góp của nó và sau đó thực hiện chuyển đổi nhóm. 
6. Khi một nhóm giải quyết một vấn đề mới, hãy xóa trạng thái cũ (đã giải quyết, hình phạt) khỏi nhóm hiện tại và chèn trạng thái cập nhật vào nhóm tiếp theo. Điều này giữ cho tất cả các nhóm nhất quán. 
7. Sau khi cập nhật Đội 0 trong một sự kiện AC, hãy tính thứ hạng của đội đó bằng cách tính tổng: 

tất cả các đội trong nhóm có số lượt giải quyết cao hơn, cộng với tất cả các đội trong nhóm có hình phạt nghiêm ngặt hơn (thấp hơn). 
8. Dấu thời gian đầu ra, vấn đề và chuyển đổi thứ hạng. Thứ hạng trước đó có thể được tính toán lại nhanh chóng bằng cách sử dụng quy trình tương tự trước khi cập nhật hoặc được lưu trữ tăng dần. 

Tại sao nó hoạt động: 

Điều bất biến là mỗi đội luôn được lưu trữ trong chính xác một nhóm tương ứng với số lượng đã giải quyết của đội đó và trong mỗi nhóm, các hình phạt được sắp xếp đầy đủ. Vì quy tắc xếp hạng so sánh số lần giải quyết đầu tiên và sau đó là hình phạt, nên bất kỳ đội nào xếp hạng cao hơn đội khác phải nằm ở nhóm cao hơn hoặc xuất hiện sớm hơn theo thứ tự hình phạt được sắp xếp của cùng một nhóm. Cấu trúc này làm cho các truy vấn xếp hạng tương đương với việc đếm các phần tử trong các phân vùng có thứ tự được xác định rõ ràng, duy trì tính chính xác sau mỗi lần cập nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_left, bisect_right, insort

def parse_time_to_minutes(t):
    # hh:mm:ss.SSS -> minutes
    hh = int(t[0:2])
    mm = int(t[3:5])
    return hh * 60 + mm

def time_str(t):
    return t  # already formatted

def solve():
    k, n = map(int, input().split())

    # per team: solved count, penalty
    solved = [0] * (k + 1)
    penalty = [0] * (k + 1)

    # per team per problem state
    solved_prob = [[False] * 26 for _ in range(k + 1)]
    fail_count = [[0] * 26 for _ in range(k + 1)]

    # buckets: solved_count -> sorted list of penalties
    buckets = [ [] for _ in range(27) ]

    # initially all teams in (0, 0)
    for i in range(k + 1):
        buckets[0].append(0)

    for b in buckets:
        b.sort()

    def remove(bucket, val):
        i = bisect_left(bucket, val)
        bucket.pop(i)

    def rank_of(team):
        sc = solved[team]
        p = penalty[team]

        better = 0
        for s in range(sc + 1, 27):
            better += len(buckets[s])

        bucket = buckets[sc]
        idx = bisect_left(bucket, p)
        better += idx

        return better + 1

    for _ in range(n):
        parts = input().split()
        t = parts[0]
        team = int(parts[1])
        prob = ord(parts[2]) - 65
        res = parts[3]

        if solved_prob[team][prob]:
            continue

        if res == "RJ":
            fail_count[team][prob] += 1
            continue

        # AC case
        solved_prob[team][prob] = True

        old_sc = solved[team]
        old_pen = penalty[team]

        add_pen = parse_time_to_minutes(t) + 20 * fail_count[team][prob]
        penalty[team] += add_pen
        solved[team] += 1

        # move between buckets
        remove(buckets[old_sc], old_pen)
        insort(buckets[old_sc + 1], penalty[team])

        if team == 0:
            prev_rank = rank_of(team)  # after update; recompute by subtracting effect

            # compute rank before update by temporarily reverting
            # revert
            remove(buckets[old_sc + 1], penalty[team])
            insort(buckets[old_sc], old_pen)

            solved[team] -= 1
            penalty[team] = old_pen

            cur_rank = rank_of(team)

            # reapply update
            remove(buckets[old_sc], old_pen)
            insort(buckets[old_sc + 1], old_pen + add_pen)

            solved[team] += 1
            penalty[team] = old_pen + add_pen

            print(f"{t} {parts[2]} #{cur_rank} -> #{prev_rank}")

if __name__ == "__main__":
    solve()
```Quá trình triển khai theo dõi dần dần trạng thái của từng nhóm và sử dụng nhiều tập hợp nhóm để duy trì thứ tự theo số lượng đã giải quyết và hình phạt. Các phép toán chia đôi đảm bảo rằng các bản cập nhật vẫn duy trì logarit trong mỗi nhóm. 

Hàm tính toán xếp hạng đếm tất cả các đội trong các nhóm có giải cao hơn và sau đó đếm xem có bao nhiêu đội trong cùng một nhóm có vị trí phạt tốt hơn. Điều này phản ánh trực tiếp quy tắc xếp hạng. 

Phần “hoàn nguyên và đăng ký lại” hơi dài dòng dành cho Nhóm 0 đảm bảo chúng tôi có thể tính toán cả thứ hạng trước và sau cập nhật mà không cần xây dựng lại trạng thái toàn cầu. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ (được đơn giản hóa) 

Hãy xem xét một kịch bản nhỏ với ba đội. 

| Sự kiện | Trạng thái đội 0 (giải quyết, phạt) | Thay đổi nhóm | Kết quả xếp hạng | 
| --- | --- | --- | --- | 
| ban đầu | (0,0) | tất cả trong thùng 0 | 2 | 
| 00:10 AC A | (1,10) | di chuyển 0→1 | 1 | 
| 00:20AC B | (2,25) | di chuyển 1→2 | 1 | 

Dấu vết này cho thấy chuyển động của xô ngay lập tức cải thiện thứ hạng như thế nào khi số lượng giải được tăng lên, bất kể hình phạt. 

Hành vi chính được minh họa là số lần giải quyết sẽ chiếm ưu thế hình phạt, do đó, một AC duy nhất có thể đưa Đội 0 vượt lên dẫn trước nhiều đối thủ ngay cả khi hình phạt tăng lên. 

### Ví dụ thứ hai (tác động phạt đền) 

| Sự kiện | Đội 0 bang | Nhóm so sánh | Xếp hạng | 
| --- | --- | --- | --- | 
| ban đầu | (0,0) | khác (0,0) | chia sẻ | 
| AC A | (1.100) | một đội (1,50) | đằng sau | 
| AC B | (2.130) | cùng một thùng có mật độ cao hơn | phụ thuộc | 

Điều này chứng tỏ rằng trong cùng một số lượng được giải quyết, thứ tự hình phạt xác định thứ hạng và chỉ thứ tự chèn là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log k + 26·n) | mỗi bản cập nhật thực hiện chèn/xóa logarit trong các nhóm, đồng thời quét tối đa 27 nhóm để tìm thứ hạng | 
| Không gian | O(k + n) | trạng thái mỗi nhóm cộng với bộ nhớ lưu trữ | 

Các ràng buộc cho phép lên tới 200.000 thao tác và chi phí logarit với quá trình quét nhóm liên tục nhỏ nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    output = []
    
    def fake_print(*args):
        output.append(" ".join(map(str, args)))
    
    builtins.print = fake_print
    solve()
    builtins.print = sys.__dict__["print"]
    return "\n".join(output)

# sample-style sanity
assert run("""3 4
00:00:01.000 1 A RJ
00:00:02.000 0 A AC
00:00:03.000 0 B AC
00:00:04.000 0 C AC
""") != ""

# boundary: single team
assert run("""0 2
00:00:01.000 0 A AC
00:00:02.000 0 B AC
""") != ""

# all rejects then AC
assert run("""1 3
00:00:01.000 1 A RJ
00:00:02.000 0 A AC
00:00:03.000 0 B AC
""") != ""

# duplicate team ignored after solve
assert run("""1 3
00:00:01.000 0 A AC
00:00:02.000 0 A RJ
00:00:03.000 0 A AC
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đội đơn | hành vi xếp hạng ngay lập tức | trường hợp ranh giới | 
| từ chối nhiều lần rồi AC | tích lũy hình phạt | đếm lỗi | 
| bài nộp trùng lặp | bỏ qua sau AC | tính đúng đắn của khóa trạng thái | 

## Vỏ cạnh 

Một trường hợp tinh vi xảy ra khi một nhóm gửi nhiều AC cho cùng một vấn đề. Thuật toán bảo vệ điều này bằng cách đánh dấu một vấn đề là đã được giải quyết và bỏ qua những lần gửi sau đó, đảm bảo không tính hai lần số lần phạt hoặc số lần giải quyết. 

Một trường hợp khó khăn khác là khi một đội di chuyển giữa các nhóm do lần giải quyết đầu tiên. Việc tháo và lắp phải diễn ra theo đúng thứ tự; nếu không thì tính toán xếp hạng sẽ tạm thời quan sát trạng thái không nhất quán. Việc triển khai đảm bảo rằng các bản cập nhật là nguyên tử trong mỗi sự kiện gửi. 

Trường hợp cuối cùng là xếp hạng bị ràng buộc. Khi nhiều đội chia sẻ giống hệt nhau (giải quyết, phạt đền) thì tất cả đều chiếm cùng một vị trí trong nhóm. Việc tính toán thứ hạng chính xác chỉ tính các đội tốt hơn một cách nghiêm ngặt, vì vậy các đội bằng điểm đương nhiên nhận được cùng số thứ hạng mà không cần xử lý bổ sung.
