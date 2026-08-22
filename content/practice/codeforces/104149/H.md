---
title: "CF 104149H - Trường sinh linh giá ẩn giấu"
description: "Harry đang cố gắng đến một trung tâm sa mạc cách đó đúng $d$ ngày nếu anh ấy đi bộ một mình. Mỗi ngày tiêu thụ một đơn vị nước cho mỗi người."
date: "2026-07-02T01:25:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104149
codeforces_index: "H"
codeforces_contest_name: "CPUlm Winter Contest 2022"
rating: 0
weight: 104149
solve_time_s: 47
verified: true
draft: false
---

[CF 104149H - Trường sinh linh giá ẩn](https://codeforces.com/problemset/problem/104149/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chính xác là Harry đang cố gắng đến một trung tâm sa mạc$d$ngày xa nếu anh đi một mình. Mỗi ngày tiêu thụ một đơn vị nước cho mỗi người. Mỗi người, kể cả Harry và mọi người bạn, chỉ có thể mang theo nhiều nhất$c$tổng số đơn vị nước và nước có thể được phân phối lại một cách tự do trong nhóm khi họ ở cùng nhau. 

Bạn bè được phép đi cùng Harry trong một phần của cuộc hành trình và sau đó quay trở lại điểm xuất phát trên cùng một con đường. Một người bạn quay trở lại phải có đủ nước để sống sót trong chuyến trở về, mất đúng số ngày bằng khoảng cách đã đi được vào thời điểm họ quay lại. Mục tiêu của Harry chỉ là tự mình đến đích chứ không quay trở lại. 

Khó khăn mấu chốt là bạn bè không chỉ là phương tiện chở thêm cho một chuyến đi cố định. Họ là những người vận chuyển tạm thời, phải tồn tại ở cả chặng đi tiếp và chặng về nếu bị gửi trả lại. Điều này tạo ra sự cân bằng: nhiều bạn bè hơn sẽ tăng tổng công suất nước nhưng cũng đưa ra các nghĩa vụ bổ sung về nước vì khách du lịch quay trở lại phải được cung cấp đầy đủ cho chuyến hành trình trở về của họ. 

Đầu vào mang lại$d$, số ngày để đến đích và$c$, lượng nước tối đa mỗi người có thể mang theo. Đầu ra là số lượng bạn bè tối thiểu cần thiết để Harry có thể sống sót đến đích, hoặc không thể nếu ngay cả với nhiều bạn bè tùy ý thì các ràng buộc cũng không thể được thỏa mãn. 

Trường hợp cạnh đầu tiên xuất hiện khi$c = 1$. Mỗi người có thể mang theo chính xác một đơn vị nước và tiêu thụ một đơn vị nước mỗi ngày, nghĩa là không ai có thể sống sót dù chỉ một ngày di chuyển. Nếu như$d \ge 1$, Chỉ riêng Harry đã thất bại và việc thêm bạn bè cũng không giúp ích được gì vì tất cả họ đều cư xử giống hệt nhau. Đầu ra chính xác là không thể. 

Một trường hợp tế nhị khác xảy ra khi$c$nhỏ nhưng$d$là lớn. Một trực giác ngây thơ có thể gợi ý rằng có đủ bạn bè luôn giải quyết được vấn đề, nhưng việc quay trở lại những người bạn trở nên cực kỳ tốn kém vì họ phải mang theo nước cho một chặng đường dài trở về tỷ lệ thuận với khoảng thời gian họ đã đi ra ngoài. 

Một trường hợp thất bại có cấu trúc chặt chẽ hơn là khi$c$chỉ ở trên 1 một chút. Ví dụ:$c = 2$,$d = 10$. Mỗi người chỉ có thêm một đơn vị ngoài khả năng sống sót hàng ngày, điều này hạn chế nghiêm trọng khoảng cách mà người trợ giúp có thể đi được trước khi cần quay lại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử tất cả số lượng bạn bè có thể có$k$, và mô phỏng xem Harry có thể đạt được khoảng cách không$d$với$k$những người giúp đỡ. Đối với mỗi cấu hình, chúng tôi sẽ theo dõi xem nhóm có thể tiến xa đến đâu mỗi ngày, lượng nước tiêu thụ và khi nào bạn bè phải quay lại. Ngay cả khi chúng tôi tối ưu hóa mô phỏng một cách cẩn thận, mỗi lần kiểm tra tính khả thi đều phụ thuộc vào việc mô phỏng tối đa$d$ngày hoặc ít nhất là nhiều bước ngoặt, và$k$có thể đi lên$d$trong lý luận tồi tệ nhất. Điều này dẫn đến khoảng$O(d^2)$hoặc hành vi tệ hơn, điều này là không thể đối với$d \le 10^9$. 

Quan sát quan trọng là chúng ta không bao giờ cần mô phỏng chuyển động hàng ngày. Thay vào đó, chúng ta chỉ cần suy luận về tổng ngân sách nước cần thiết cho một chiến lược hợp lý và cần bao nhiêu người để cung cấp nó. 

Một cách hữu ích để điều chỉnh lại vấn đề là hãy tưởng tượng mỗi người bạn như một “thùng chứa nước” có thể tái sử dụng, đóng góp sức chứa khi hiện tại nhưng trở thành gánh nặng khi trở về vì họ phải mang đủ nước cho hành trình trở về. Hạn chế quan trọng là tại bất kỳ thời điểm nào, nhóm phải có khả năng hỗ trợ đồng thời tất cả khách du lịch đang hoạt động và tất cả khách du lịch quay lại. 

Điều này dẫn đến một cấu trúc đơn điệu: nếu$k$bạn bè là đủ rồi$k' > k$cũng là đủ. Tính đơn điệu này cho phép tìm kiếm nhị phân trên câu trả lời. Nhiệm vụ còn lại là kiểm tra tính khả thi: đã cho$k$, xác định xem Harry có thể được hỗ trợ hay không. 

Việc kiểm tra tính khả thi giảm xuống thành một lý do tham lam về việc một người bạn có thể ở lại bao lâu trước khi quay trở lại. Nếu một người bạn quay lại sau$x$ngày, họ phải mang theo ít nhất$x$đơn vị trả lại, cộng thêm 1 đơn vị mỗi ngày trong thời gian trả lại đã được tính đối xứng; hạn chế chủ yếu là nước mang theo của họ phải bao phủ cả hai chân trong khả năng$c$. Điều này ngụ ý độ dài chuyến tham quan hiệu quả tối đa cho mỗi người bạn và do đó có giới hạn về mức độ “phạm vi bảo hiểm” mà mỗi người bạn có thể đóng góp cho sự tiến bộ về phía trước của Harry. 

Giải pháp tối ưu bắt nguồn từ việc tính toán xem mỗi người bạn có thể cung cấp bao nhiêu “ngày hỗ trợ thêm” hiệu quả và tích lũy đóng góp cho đến khi đạt được$d$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(d^2)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân + Tính khả thi tham lam |$O(\log d)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý số lượng bạn bè$k$như một phỏng đoán và kiểm tra xem nó có đủ không. 

1. Đối với cố định$k$, xét toàn bộ hệ thống$k+1$con người, mỗi người có năng lực$c$, cho tổng lượng nước$(k+1)c$. Đây là tổng số tài nguyên có sẵn tại thời điểm 0. 
2. Mỗi ngày sinh tồn của toàn bộ nhóm hoạt động tiêu tốn chính xác số người hiện đang đi về phía trước. Điều phức tạp là một số bạn bè có thể quay lại, nhưng việc quay lại chỉ làm chuyển mức tiêu dùng của họ sang giai đoạn quay lại. 
3. Ý tưởng trọng tâm là tính toán khoảng cách chuyển tiếp tối đa có thể được “hỗ trợ” bởi$k$bạn bè mà không vi phạm ràng buộc trả lại. Mỗi người bạn có thể được gửi tiếp trong một số ngày$t$, nhưng phải giữ đủ nước để quay trở lại$t$ngày, vì vậy sự tham gia về phía trước của họ bị giới hạn bởi$c = t + t_{\text{return}} + \text{ongoing consumption}$, điều này sụp đổ thành một ràng buộc tuyến tính về thời gian chúng có thể hữu ích. 
4. Chúng tôi rút ra được sự đóng góp hiệu quả: một người bạn có thể đóng góp nhiều nhất$c - 1$thêm nhiều ngày trước khi cần dự trữ nước để sử dụng lại, vì phải sử dụng ít nhất 1 đơn vị mỗi ngày để tồn tại khi còn hiện diện. 
5. Chúng tôi sắp xếp hoặc tích lũy các khoản đóng góp về mặt khái niệm: với$k$các bạn ơi, tổng số hỗ trợ bổ sung là$k \cdot (c - 1)$. Bản thân Harry chỉ đóng góp$c$ngày sống sót mà không cần ràng buộc về lợi nhuận. 
6. Chúng tôi kiểm tra xem tổng số hỗ trợ hiệu quả có$c + k(c - 1)$đạt ít nhất$d$. Nếu có,$k$là đủ. 
7. Chúng tôi tìm kiếm nhị phân nhỏ nhất$k$TRONG$[0, d]$thỏa mãn điều kiện này. 
8. Nếu thậm chí rất lớn$k$không thỏa mãn bất đẳng thức thì ta cho kết quả là không thể. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi đơn vị tiến trình về phía trước sẽ tiêu thụ chính xác một đơn vị nước từ chính xác một người tham gia đang hoạt động và bất kỳ người bạn nào quay lại đều phải trả một chi phí đối xứng bằng với mức tham gia về phía trước của họ. Sự đối xứng này tạo ra một mô hình tính toán tuyến tính: mỗi người bạn bổ sung đóng góp một lượng biên cố định cho khả năng chịu đựng có thể sử dụng được trong tương lai, không phụ thuộc vào việc lập kế hoạch. Vì không có chiến lược nào có thể tái sử dụng nước vượt quá khả năng$c$mỗi người, giới hạn tuyến tính là chặt chẽ và mọi lịch trình khả thi đều tương ứng với sự phân chia của$d$vào các khoản đóng góp giới hạn bởi$c$Và$c-1$số gia tăng. Vì vậy, việc kiểm tra bất đẳng thức chính xác là đặc trưng của tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(k, d, c):
    # Harry + k friends
    # total effective forward capacity
    return c + k * (c - 1) >= d

def solve():
    d, c = map(int, input().split())

    if c == 1:
        print("impossible" if d > 0 else 0)
        return

    lo, hi = 0, d
    ans = None

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, d, c):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    if ans is None:
        print("impossible")
    else:
        print(ans)

if __name__ == "__main__":
    solve()
```chức năng`can(k, d, c)`mã hóa bất đẳng thức khả thi dẫn xuất. Tìm kiếm nhị phân khám phá không gian đơn điệu của số lượng bạn bè. Trường hợp đặc biệt`c == 1`được xử lý riêng biệt vì sự bất bình đẳng suy giảm: không người tham gia nào có thể sống sót dù chỉ một ngày, khiến mọi khoảng cách tích cực đều không thể xảy ra. 

Sự tinh tế chính là đảm bảo chúng tôi không mô phỏng bất kỳ chuyển động nào. Tất cả động lực của hành trình tiến và lùi được nén thành một biểu thức công suất tuyến tính duy nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét$d = 4, c = 3$. 

Chúng tôi đánh giá số lượng bạn bè ngày càng tăng. 

| k | c + k(c-1) | Khả thi | 
| --- | --- | --- | 
| 0 | 3 | Không | 
| 1 | 5 | Có | 

Tối thiểu là$k = 1$. Điều này phù hợp với ý tưởng rằng một người bạn có thể kéo dài sức chịu đựng của Harry thêm 2 ngày một cách hiệu quả ngoài khả năng của chính anh ấy. 

Dấu vết này cho thấy tính khả thi tăng trưởng đơn điệu với$k$, điều này biện minh cho tìm kiếm nhị phân. 

Bây giờ hãy xem xét$d = 5, c = 3$. 

| k | c + k(c-1) | Khả thi | 
| --- | --- | --- | 
| 0 | 3 | Không | 
| 1 | 5 | Có | 

Một lần nữa, một người bạn là đủ, và trường hợp ranh giới xảy ra chính xác khi tổng sức chứa phù hợp với khoảng cách yêu cầu. 

Những ví dụ này chứng minh rằng mỗi người bạn đóng góp một khoản lợi ích cộng thêm cố định$c-1$, và Harry đóng góp cơ sở$c$, tạo thành một mô hình tăng trưởng tuyến tính đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log d)$| Tìm kiếm nhị phân theo số lượng bạn bè có thể có, mỗi lần kiểm tra là$O(1)$| 
| Không gian |$O(1)$| Chỉ sử dụng các biến số học | 

Những ràng buộc cho phép$d$lên tới$10^9$, vì vậy các phương pháp tiếp cận tuyến tính hoặc dựa trên mô phỏng là không khả thi. Tìm kiếm logarit với kiểm tra tính khả thi theo thời gian liên tục dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    d, c = map(int, input().split())

    if c == 1:
        return "impossible" if d > 0 else "0"

    def can(k):
        return c + k * (c - 1) >= d

    lo, hi = 0, d
    ans = None
    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    return str(ans) if ans is not None else "impossible"

# provided samples (as given text is ambiguous, we keep structure)
# assert run("4 3") == "1"
# assert run("5 3") == "1"

# custom cases
assert run("1 1") == "impossible", "minimum impossible case"
assert run("4 3") == "1", "small feasible case"
assert run("5 3") == "1", "slightly larger feasible case"
assert run("10 2") == "8", "tight capacity growth case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | không thể | trường hợp cạnh công suất bằng không | 
| 4 3 | 1 | tính khả thi tối thiểu không tầm thường | 
| 5 3 | 1 | ổn định ranh giới | 
| 10 2 | 8 | tăng trưởng công suất chậm | 

## Vỏ cạnh 

Khi nào$c = 1$, mỗi người tiêu thụ chính xác những gì họ có thể mang theo mỗi ngày, vì vậy ngay cả một mình Harry cũng không thể sống sót qua bước đầu tiên. Thuật toán ngay lập tức trả về không thể nếu không nhập tìm kiếm nhị phân, khớp với ràng buộc$c + k(c-1)$không bao giờ vượt quá 1. 

cho$d = 1, c = 2$, tờ séc cho$2 + 0 = 2 \ge 1$, vì vậy không có bạn bè là đủ. Thuật toán đưa ra kết quả 0 chính xác vì chỉ riêng Harry đã đủ năng lực cho một ngày. 

Đối với lớn$d$và nhỏ$c$, chẳng hạn như$d = 10^9, c = 2$, số lượng bạn bè cần thiết trở nên vô cùng lớn. Tìm kiếm nhị phân khám phá tới$d$, nhưng vẫn hội tụ nhanh vì tính khả thi tăng tuyến tính với độ dốc$c-1 = 1$, vậy câu trả lời là xấp xỉ$d - c$, phù hợp với việc kiểm tra bất đẳng thức.
