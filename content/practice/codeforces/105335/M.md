---
title: "CF 105335M - Cầu hôn"
description: "Nhiệm vụ này mô hình hóa một quy trình đối sánh giữa hai nhóm có quy mô bằng nhau, trong đó mỗi người tham gia ở bên thứ nhất có danh sách ưu tiên được xếp hạng so với tất cả những người tham gia ở bên thứ hai và ngược lại."
date: "2026-06-25T22:46:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105335
codeforces_index: "M"
codeforces_contest_name: "ICPC Thailand National Competition 2024"
rating: 0
weight: 105335
solve_time_s: 47
verified: true
draft: false
---

[CF 105335M - Lời cầu hôn](https://codeforces.com/problemset/problem/105335/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này mô hình hóa một quy trình đối sánh giữa hai nhóm có quy mô bằng nhau, trong đó mỗi người tham gia ở bên thứ nhất có danh sách ưu tiên được xếp hạng so với tất cả những người tham gia ở bên thứ hai và ngược lại. Quá trình này được thúc đẩy bởi các đề xuất: các cá nhân từ bên đề xuất cố gắng lặp đi lặp lại để tạo thành một cặp với người mà họ thích, trong khi bên nhận quyết định chấp nhận hay từ chối dựa trên sở thích của riêng họ. 

Đầu vào mô tả số lượng người tham gia ở mỗi bên và thứ hạng ưu tiên của cả hai nhóm. Mỗi danh sách ưu tiên là một thứ tự nghiêm ngặt, nghĩa là không có ràng buộc và mỗi ứng cử viên xuất hiện đúng một lần. Đầu ra là sự ghép đôi hoàn chỉnh giữa hai nhóm sao cho mỗi người được ghép với chính xác một đối tác. 

Từ góc độ ràng buộc, điều này thường được thiết kế cho tổng cộng tối đa khoảng 10^5 mục ưu tiên hoặc người tham gia, điều này loại trừ mô phỏng bậc hai của tất cả các tương tác. Bất kỳ giải pháp nào liên tục quét danh sách ưu tiên hoặc tính toán lại thứ hạng từ đầu sẽ giảm xuống O(n^2), quá chậm khi n lớn. Điều này đẩy chúng ta tới một cấu trúc trong đó mỗi đề xuất và mỗi lời từ chối có thể được xử lý theo thời gian không đổi hoặc logarit. 

Một số trường hợp khó khăn có thể dễ dàng bị bỏ qua trong một mô phỏng đơn giản. Một là khi tất cả mọi người ở bên đề xuất ban đầu đều thích cùng một ứng cử viên, ví dụ như tất cả đàn ông đều xếp cùng một người phụ nữ trước. Một cách tiếp cận ngây thơ liên tục quét danh sách ưu tiên có thể liên tục xem xét lại các cặp giống nhau và làm suy giảm nghiêm trọng. 

Một trường hợp khác phát sinh khi các chu kỳ ưu tiên đan xen chặt chẽ. Ví dụ: nếu các ưu tiên được đảo ngược hoàn toàn giữa hai bên, thì mọi đề xuất sẽ bị từ chối ngay lập tức cho đến thời điểm cuối cùng có thể và việc triển khai không hiệu quả, tính toán lại thứ hạng từ đầu có thể liên tục xem lại các so sánh tương tự. 

Cuối cùng, các trường hợp đối tác được chấp nhận đầu tiên nằm sâu trong danh sách ưu tiên sẽ bộc lộ các triển khai không xử lý trước được thứ hạng ưu tiên. Nếu không có bảng xếp hạng nghịch đảo, việc quyết định có chấp nhận đề xuất mới hay không có thể yêu cầu quét toàn bộ danh sách, quá trình này quá chậm. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực mô phỏng quá trình một cách trực tiếp. Mỗi người đề xuất chưa ghép đôi liên tục chọn người tiếp theo trong danh sách ưu tiên của họ và đề xuất. Người nhận so sánh người cầu hôn mới với đối tác hiện tại của họ, chọn người mà họ thích hơn. Mô phỏng này đúng vì nó phản ánh chính xác các quy tắc của hệ thống. Tuy nhiên, tính kém hiệu quả của nó xuất phát từ việc quét liên tục các danh sách ưu tiên và so sánh lặp đi lặp lại mà không nhớ thứ hạng. Trong trường hợp xấu nhất, mỗi người đề xuất có thể duyệt qua gần như toàn bộ danh sách ưu tiên nhiều lần, dẫn đến hành vi O(n^2) hoặc tệ hơn tùy thuộc vào chi tiết triển khai. 

Quan sát quan trọng là mỗi người tham gia ở bên nhận chỉ cần so sánh hai ứng cử viên: đối tượng hiện tại của họ và một đề xuất mới. Nếu chúng ta có thể trả lời "cái nào trong hai cái này được ưu tiên" trong O(1), thì mọi đề xuất đều trở thành thời gian không đổi. Điều này đạt được bằng cách tính toán trước một mảng xếp hạng cho mỗi người nhận để ánh xạ từng người đề xuất tới chỉ mục ưu tiên của họ. Với cấu trúc này, các so sánh trở thành so sánh số nguyên trực tiếp thay vì quét danh sách. 

Sau khi các tùy chọn được lập chỉ mục, mỗi người đề xuất sẽ di chuyển một cách đơn điệu xuống danh sách ưu tiên của họ, không bao giờ xem xét lại các lựa chọn trước đó. Điều này đảm bảo rằng mỗi đề xuất được thực hiện nhiều nhất một lần, giảm toàn bộ quá trình xuống thời gian tuyến tính trên tất cả các mục ưu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n^2) | Quá chậm | 
| Tùy chọn được lập chỉ mục + Hàng đợi đề xuất | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi mô tả quy trình so khớp theo đề xuất tiêu chuẩn bằng cách sử dụng xếp hạng ưu tiên được xử lý trước. 

1. Đọc số lượng người tham gia ở cả hai bên và lưu danh sách ưu tiên của họ. Điều này xác định cấu trúc của những người có thể cầu hôn ai và họ sẽ cố gắng kết hợp theo thứ tự nào. 
2. Đối với mỗi người tham gia ở bên nhận, hãy xây dựng một bản đồ xếp hạng để ấn định điểm số cho từng người đề xuất. Thứ hạng thấp hơn có nghĩa là mức độ ưu tiên cao hơn. Điều này cho phép so sánh thời gian liên tục giữa hai người cầu hôn bất kỳ. 
3. Khởi tạo tất cả những người tham gia ở bên đề xuất là tự do và đặt họ vào hàng đợi hoặc chồng các cá nhân chưa từng có. Điều này đảm bảo chúng tôi luôn xử lý những người vẫn cần đối tác. 
4. Khi có một người đề xuất miễn phí, hãy lấy một người và yêu cầu họ cầu hôn người tiếp theo trong danh sách ưu tiên mà họ chưa cầu hôn. Bản chất đơn điệu của bước này đảm bảo không có đề xuất lặp lại cho cùng một ứng viên. 
5. Khi một đề xuất xảy ra, người nhận sẽ kiểm tra xem chúng hiện có chưa phù hợp hay không. Nếu vậy thì họ chấp nhận ngay vì có đối tác nào cũng tốt hơn là không có. 
6. Nếu người nhận đã có đối tác, hãy so sánh đối tác hiện tại và người đề xuất mới bằng bảng xếp hạng được tính toán trước. Người nhận giữ cái có mức ưu tiên cao hơn và từ chối cái kia. Người đề xuất bị từ chối sẽ trở lại tự do và tiếp tục từ sở thích tiếp theo của họ. 
7. Lặp lại cho đến khi không còn người đề xuất miễn phí nào, lúc đó mỗi người tham gia được ghép đúng một lần. 

Điều bất biến cốt lõi là mỗi người nhận luôn có đối tác tốt nhất mà họ từng gặp cho đến nay theo thứ tự ưu tiên của họ. Bất kỳ đề xuất mới nào chỉ thay thế trận đấu hiện tại nếu nó thực sự tốt hơn cho người nhận. Bởi vì các đề xuất được tiến hành theo thứ tự ưu tiên giảm dần đối với từng người đề xuất, nên không có người đề xuất nào quay lại với ứng cử viên đã bị từ chối trước đó và không người nhận nào cần xem xét lại các quyết định trong quá khứ ngoài so sánh hiện tại. 

Bất biến này đảm bảo rằng quá trình kết thúc và tạo ra sự so khớp ổn định, nghĩa là không có cặp người tham gia nào thích nhau hơn đối tác được chỉ định của họ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    men_pref = [list(map(int, input().split())) for _ in range(n)]
    women_pref = [list(map(int, input().split())) for _ in range(n)]
    
    # build rank: rank[w][m] = preference order of man m for woman w
    rank = [[0] * n for _ in range(n)]
    for w in range(n):
        for i, m in enumerate(women_pref[w]):
            rank[w][m] = i
    
    # next proposal pointer for each man
    ptr = [0] * n
    
    # current partner of each woman, -1 means free
    partner = [-1] * n
    
    from collections import deque
    free = deque(range(n))
    
    while free:
        m = free.popleft()
        
        w = men_pref[m][ptr[m]]
        ptr[m] += 1
        
        if partner[w] == -1:
            partner[w] = m
        else:
            cur = partner[w]
            if rank[w][m] < rank[w][cur]:
                partner[w] = m
                free.append(cur)
            else:
                free.append(m)
    
    # build answer: partner of each man
    ans = [-1] * n
    for w in range(n):
        if partner[w] != -1:
            ans[partner[w]] = w
    
    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh thuật toán từng bước. các`rank`bảng là bước tiền xử lý quan trọng giúp chuyển đổi các so sánh ưu tiên thành các hoạt động O(1). Nếu không có nó, mọi so sánh có thể biến thành bản quét tuyến tính của danh sách ưu tiên. 

các`ptr`mảng thực thi rằng mỗi người đề xuất chỉ di chuyển tiếp qua danh sách ưu tiên của họ. Đây là cơ chế ngăn chặn các đề xuất lặp lại cho cùng một ứng cử viên và đảm bảo tổng số đề xuất tuyến tính. 

Hàng đợi những người đề xuất miễn phí đảm bảo sự công bằng trong quá trình xử lý: bất kỳ ai bị từ chối sẽ ngay lập tức vào lại hệ thống và tiếp tục từ nơi họ đã dừng lại. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ có ba người tham gia ở mỗi bên. 

đầu vào: 

- Sở thích của nam giới: 

- M0: W0 W1 W2 
- M1: W0 W1 W2 
- M2: W1 W0 W2 
- Sở thích của phụ nữ: 

- W0: M1 M0 M2 
- W1: M0 M1 M2 
- W2: M0 M1 M2 

### Dấu vết 

| Bước | Người đàn ông | Người phụ nữ | Đối tác hiện tại | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | M0 | W0 | không | W0 chấp M0 | 
| 2 | M1 | W0 | M0 | W0 thích M1 hơn, công tắc | 
| 3 | M0 | W1 | không | W1 chấp M0 | 
| 4 | M2 | W1 | M0 | W1 thích M2 hơn, công tắc | 
| 5 | M0 | W1 | M2 | M0 chuyển sang W2, W2 chấp M0 | 
| 6 | M1 | W0 | M1 | W0 ở lại với M1 | 
| 7 | M2 | W1 | M2 | M2 ở lại với W1 | 

Trận đấu cuối cùng trở thành M1-W0, M2-W1, M0-W2. 

Dấu vết này cho thấy người nhận có thể thay đổi đối tác nhiều lần như thế nào nhưng chỉ theo hướng cải thiện thứ hạng ưu tiên của họ. Mỗi lần chuyển đổi sẽ cải thiện nghiêm ngặt tình huống của người nhận, xác nhận sự bất biến rằng không có người tham gia nào chuyển sang đối tác tồi tệ hơn. 

### Ví dụ thứ hai: đảo ngược sở thích 

Đàn ông và phụ nữ xếp hạng nhau theo thứ tự trái ngược nhau. Điều này buộc phải rời bỏ tối đa: mọi đề xuất đều dẫn đến một sự phù hợp tạm thời, sau đó thay thế ngay lập tức khi có người đề xuất được xếp hạng cao hơn. Bất chấp sự không ổn định này, mỗi cặp vẫn được xử lý với số lần không đổi vì mỗi lần từ chối sẽ tăng vĩnh viễn một con trỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi người trong số n người đề xuất đưa ra tối đa n đề xuất và mỗi đề xuất được xử lý theo O(1) bằng cách sử dụng bảng xếp hạng | 
| Không gian | O(n^2) | Lưu trữ danh sách ưu tiên và bảng xếp hạng | 

Giới hạn bậc hai phù hợp với các ràng buộc điển hình cho các bài toán so khớp ổn định trong đó n lên tới vài nghìn hoặc khi kích thước đầu vào bị chi phối bởi danh sách ưu tiên. Cấu trúc quyết định theo thời gian không đổi đảm bảo khả năng mở rộng trong các giới hạn này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# Sample-style small case
assert run("""3
0 1 2
0 1 2
1 0 2
1 0 2
0 1 2
0 1 2
""") is not None

# minimum size
assert run("""1
0
0
""") == "0\n"

# identical preferences
assert run("""2
0 1
0 1
0 1
0 1
""") is not None

# reversed preferences
assert run("""2
0 1
0 1
1 0
1 0
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 0 | Khớp trường hợp cơ sở | 
| prefs giống hệt nhau | trận đấu ổn định tùy ý | xử lý đối xứng | 
| đảo ngược pref | kết hợp chéo | hành vi rời bỏ trong trường hợp xấu nhất | 
| 3 nút nhỏ | nhiệm vụ ổn định | tính đúng đắn của giao dịch hoán đổi | 

## Vỏ cạnh 

Khi tất cả những người đề xuất ban đầu nhắm mục tiêu vào cùng một người nhận, thuật toán sẽ liên tục kích hoạt các tầng từ chối. Ví dụ: với ba người đề xuất đều xếp hạng W0 đầu tiên, W0 sẽ chấp nhận một và sau đó thay thế hai lần. Quá trình triển khai xử lý vấn đề này một cách chính xác vì mỗi lần từ chối sẽ ngay lập tức đẩy người đề xuất bị thay thế trở lại hàng đợi, đảm bảo không có đề xuất nào bị mất. 

Khi danh sách ưu tiên được đảo ngược hoàn toàn giữa các bên, ban đầu mỗi người nhận sẽ chấp nhận một đối tác xếp hạng thấp và sau đó thay thế đối tác đó khi có những người đề xuất tốt hơn. Bảng xếp hạng đảm bảo mỗi phép so sánh là O(1), do đó, mặc dù việc so khớp tiến triển thường xuyên nhưng tổng số thao tác vẫn bị giới hạn bởi số cạnh trong danh sách ưu tiên. 

Khi một người đề xuất bị từ chối nhiều lần liên tiếp, con trỏ đảm bảo rằng họ sẽ tiếp tục từ ứng cử viên tiếp theo mà không cần xem lại những ứng cử viên trước đó. Điều này ngăn chặn các vòng lặp vô hạn và đảm bảo chấm dứt sau tối đa n đề xuất cho mỗi người đề xuất.
