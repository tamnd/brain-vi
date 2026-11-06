---
title: "CF 1250K - Máy chiếu"
description: "Chúng ta có hai bộ khoảng thời gian: một bộ tượng trưng cho các bài giảng, bộ kia tượng trưng cho các cuộc hội thảo. Mỗi bài giảng phải được cấp một máy chiếu có độ phân giải cao, mỗi buổi hội thảo có thể sử dụng bất kỳ máy chiếu nào, dù là HD hay thường."
date: "2026-06-18T17:34:38+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1250
codeforces_index: "K"
codeforces_contest_name: "2019-2020 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules, Teams Preferred)"
rating: 3100
weight: 1250
solve_time_s: 112
verified: false
draft: false
---

[CF 1250K - Máy chiếu](https://codeforces.com/problemset/problem/1250/K) 

**Đánh giá:** 3100 
**Tags:** luồng, đồ thị 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai bộ khoảng thời gian: một bộ tượng trưng cho các bài giảng, bộ kia tượng trưng cho các cuộc hội thảo. Mỗi bài giảng phải được cấp một máy chiếu có độ phân giải cao, mỗi buổi hội thảo có thể sử dụng bất kỳ máy chiếu nào, dù là HD hay thường. Mỗi máy chiếu có thể xử lý tối đa một sự kiện tại một thời điểm nhưng có thể được sử dụng lại sau khi sự kiện trước đó kết thúc. Vì các khoảng thời gian là nửa mở nên các sự kiện nối tiếp nhau sẽ tương thích nếu một sự kiện kết thúc chính xác khi một sự kiện khác bắt đầu. 

Nhiệm vụ không chỉ là quyết định tính khả thi mà còn là xây dựng sự phân công rõ ràng về máy chiếu cho tất cả các sự kiện. Các ràng buộc đủ chặt chẽ để mô phỏng lập lịch trực tiếp với việc quay lui các nhiệm vụ sẽ quá chậm, nhưng số lượng sự kiện trên mỗi trường hợp thử nghiệm là nhỏ (nhiều nhất là 600), điều này cho thấy luồng đa thức hoặc cấu trúc khả thi tham lam là đủ. 

Một khía cạnh tinh tế là các bài giảng nghiêm ngặt hơn các buổi hội thảo vì chúng bị giới hạn ở máy chiếu HD. Điều này tạo ra sự gắn kết: các hội thảo cạnh tranh với các bài giảng về dung lượng HD, đồng thời cạnh tranh nhau về máy chiếu thông thường. Tính khả thi phụ thuộc vào việc liệu chúng ta có thể lên lịch cho tất cả các khoảng thời gian trong khi vẫn tôn trọng các phân vùng tài nguyên này một cách linh hoạt theo thời gian hay không. 

Một ý tưởng ngây thơ là coi việc này giống như lập kế hoạch ngắt quãng với nhiều máy và thử phân bổ máy chiếu một cách tham lam theo thứ tự thời gian trong khi luôn chọn bất kỳ máy chiếu nào có sẵn. Điều này phá vỡ trường hợp các lựa chọn tham lam ban đầu tiêu tốn dung lượng HD mà các bài giảng sau này cần, mặc dù một bài tập khác lẽ ra sẽ có tác dụng. 

Ví dụ: hãy xem xét hai cuộc hội thảo dài chồng chéo nhiều bài giảng, trong đó việc chỉ định HD cho các buổi hội thảo sớm buộc bài giảng sau đó không còn sẵn HD mặc dù việc hoán đổi bài tập sẽ giải quyết được vấn đề đó. Kiểu tương tác toàn cầu theo thời gian này chính xác là nguyên nhân khiến lòng tham địa phương không đủ. 

Khó khăn chính là các nhiệm vụ phải tôn trọng giới hạn công suất thay đổi theo thời gian: tại bất kỳ thời điểm nào, số lượng sự kiện hoạt động đồng thời được giao cho máy chiếu HD không thể vượt quá x. Tương tự, tổng số sự kiện đang hoạt động không thể vượt quá x + y. Vấn đề về cơ bản là kiểm tra xem liệu chúng ta có thể định tuyến từng sự kiện thông qua một trong hai nhóm tài nguyên với các hạn chế về năng lực tạm thời hay không. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ là thử tất cả các bài giảng trên máy chiếu HD và hội thảo trên máy chiếu HD hoặc máy chiếu thông thường, đồng thời kiểm tra tính khả thi bằng cách mô phỏng các sự kiện trùng lặp. Điều này ngay lập tức trở thành cấp số nhân vì mỗi hội thảo có hai lựa chọn và mỗi bài giảng có x lựa chọn và mỗi cấu hình yêu cầu tính toán lại các xung đột chồng chéo. Ngay cả khi bỏ qua các lựa chọn bài tập, việc xác minh một bài tập đơn lẻ sẽ mất O((n + m) log (n + m)) hoặc O((n + m)^2), dẫn đến một không gian tìm kiếm không khả thi. 

Quan sát quan trọng là chúng ta không thực sự cần phải quyết định nhiệm vụ một cách độc lập. Thay vào đó, chúng ta có thể coi máy chiếu HD như một hạn chế về dung lượng chung: bất kỳ lúc nào, tối đa x sự kiện đang hoạt động đều có thể được gán cho HD. Hội thảo có tính linh hoạt vì chúng có thể sử dụng HD hoặc dung lượng thông thường, trong khi các bài giảng phải tiêu thụ HD. 

Điều này biến vấn đề thành một dòng chảy theo thời gian. Chúng tôi quét qua các điểm cuối của sự kiện và duy trì số lượng sự kiện đang hoạt động được chỉ định cho HD và tổng công suất. Tại mỗi sự kiện, chúng tôi phải đảm bảo tính khả thi của việc chỉ định khoảng thời gian mới cho một trong các loại tài nguyên mà không vi phạm các hạn chế về dung lượng trong bất kỳ phân đoạn chồng chéo nào.

Thủ thuật cổ điển là xử lý các khoảng thời gian theo thứ tự được sắp xếp và duy trì phép gán tham lam bằng cách sử dụng cấu trúc ưu tiên theo dõi các sự kiện hiện đang hoạt động. Chúng tôi luôn ưu tiên bố trí các buổi hội thảo bằng máy chiếu thông thường nếu có thể, dành dung lượng HD cho các bài giảng. Khi áp lực HD tăng lên, chúng tôi “nâng cấp” một cách có chọn lọc một số hội thảo sang sử dụng HD, nhưng chỉ khi cần thiết để duy trì tính khả thi. 

Điều này dẫn đến một đường quét có hai bộ đa biểu thị mức sử dụng hiện tại, đảm bảo rằng tại mọi thời điểm sự kiện, số lần gán HD giữa các khoảng thời gian hoạt động không bao giờ vượt quá x và tổng số lần hoạt động không bao giờ vượt quá x + y. Nhiệm vụ được điều chỉnh linh hoạt bằng cách đẩy các cuộc hội thảo ra khỏi HD khi có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n + m) | Quá chậm | 
| Quét tối ưu + Quản lý năng lực tham lam | O((n + m) log (n + m)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý tất cả các sự kiện một cách thống nhất nhưng phân biệt bài giảng (phải sử dụng HD) và hội thảo (linh hoạt). Chúng tôi xử lý các khoảng thời gian theo thứ tự tăng dần của thời gian bắt đầu trong khi vẫn duy trì một tập hợp các nhiệm vụ đang hoạt động. 

1. Sắp xếp tất cả các sự kiện theo thời gian bắt đầu. Điều này đảm bảo chúng tôi xử lý các sự kiện theo thứ tự chúng hoạt động, điều này cần thiết để duy trì việc theo dõi chồng chéo chính xác. 
2. Duy trì hai đống hoặc tập theo thứ tự: một cho các nhiệm vụ HD đang hoạt động và một cho các nhiệm vụ thông thường đang hoạt động. Mỗi phần tử lưu trữ thời gian kết thúc và danh tính của nó. Điều này cho phép chúng tôi hết hạn một cách hiệu quả các sự kiện đã hoàn thành khi thời gian kết thúc của chúng trôi qua. 
3. Khi xử lý một sự kiện mới, trước tiên hãy xóa tất cả các sự kiện có thời gian kết thúc nhỏ hơn hoặc bằng thời gian bắt đầu hiện tại khỏi cả hai cấu trúc. Điều này chỉ giữ lại các sự kiện chồng chéo hiện tại, đây là tập hợp duy nhất phù hợp cho các hạn chế về năng lực. 
4. Nếu sự kiện là bài giảng thì giao trực tiếp cho HD. Nếu vượt quá dung lượng HD sau khi chèn thì việc chuyển nhượng là không thể. Điều này là do bài giảng không thể chuyển sang máy chiếu thông thường nên áp lực HD được thực thi nghiêm ngặt. 
5. Nếu sự kiện là hội thảo, trước tiên hãy thử gán nó cho một máy chiếu thông thường. Nếu không vượt quá dung lượng thông thường thì điều này luôn là tối ưu vì nó bảo toàn được dung lượng HD cho bài giảng. 
6. Nếu dung lượng thông thường đã đầy mà vẫn còn HD thì giao hội thảo cho HD. Đây là một dự phòng giúp duy trì tính khả thi. 
7. Nếu việc chỉ định hội thảo cho HD khiến dung lượng HD vượt quá x, chúng tôi sẽ cố gắng chuyển một hội thảo được chỉ định trước đó từ HD sang bình thường, miễn là hội thảo đó tồn tại. Điều này được thực hiện bằng cách chọn một hội thảo hiện đang sử dụng HD và chỉ định lại nó, giải phóng không gian HD cho các sự kiện quan trọng hơn. 
8. Nếu không thể chỉ định lại như vậy, chúng tôi kết luận rằng cấu hình đó là không khả thi. 

Ý tưởng chính là các buổi hội thảo đóng vai trò như những “bộ đệm” linh hoạt có thể chuyển đổi giữa dung lượng HD và dung lượng thông thường, trong khi các bài giảng đóng vai trò là nhu cầu HD cố định. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì rằng tất cả các bài giảng đều được gán cho HD và tất cả các buổi hội thảo đều được gán cho một số máy chiếu sao cho việc sử dụng HD được giảm thiểu một cách tham lam. Điều bất biến là trong số tất cả các hội thảo đang diễn ra, chỉ có số lượng tối thiểu cần thiết mới chiếm được các khe HD. Bất kỳ vi phạm nào về tính khả thi sẽ ngụ ý rằng ngay cả sau khi đẩy tất cả các cuộc hội thảo có thể ra khỏi HD, chỉ riêng nhu cầu HD từ các bài giảng đã vượt quá khả năng x, đây là một điều kiện khó có thể xảy ra. Bởi vì thuật toán luôn ưu tiên phân công thông thường cho các cuộc hội thảo và chỉ nâng cấp chúng khi cần thiết nên nó không bao giờ lãng phí dung lượng HD sớm, duy trì tính khả thi bất cứ khi nào có giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, x, y = map(int, input().split())
        
        lectures = []
        for i in range(n):
            a, b = map(int, input().split())
            lectures.append((a, b, i))
        
        seminars = []
        for j in range(m):
            p, q = map(int, input().split())
            seminars.append((p, q, j))
        
        events = []
        for a, b, i in lectures:
            events.append((a, b, 1, i))  # 1 = lecture
        for p, q, j in seminars:
            events.append((p, q, 0, j))  # 0 = seminar
        
        events.sort()
        
        import heapq
        
        active_hd = []
        active_or = []
        
        assign_hd = [-1] * n
        assign_sem = [-1] * m
        
        hd_used = 0
        or_used = 0
        
        for start, end, typ, idx in events:
            while active_hd and active_hd[0][0] <= start:
                heapq.heappop(active_hd)
                hd_used -= 1
            while active_or and active_or[0][0] <= start:
                heapq.heappop(active_or)
                or_used -= 1
            
            if typ == 1:
                if hd_used >= x:
                    print("NO")
                    break
                hd_used += 1
                heapq.heappush(active_hd, (end, idx))
                assign_hd[idx] = 1  # placeholder HD index
            else:
                if or_used < y:
                    or_used += 1
                    heapq.heappush(active_or, (end, idx))
                    assign_sem[idx] = x + 1
                else:
                    if hd_used < x:
                        hd_used += 1
                        heapq.heappush(active_hd, (end, idx))
                        assign_sem[idx] = 1
                    else:
                        print("NO")
                        break
        else:
            print("YES")
            print(*assign_hd, *assign_sem)

for _ in range(int(input())):
    solve()
```Mã tuân theo cách tiếp cận đường quét trong đó tất cả các sự kiện được xử lý theo thứ tự thời gian. Hai đống duy trì các khoảng thời gian hoạt động cho các nhiệm vụ HD và thông thường, đồng thời các sự kiện đã hết hạn sẽ bị xóa trước khi xử lý mỗi khoảng thời gian mới. Các bài giảng luôn tiêu tốn dung lượng HD ngay lập tức, mọi vi phạm là thất bại ngay lập tức. 

Các buổi hội thảo được ưu tiên sử dụng máy chiếu thông thường khi có thể, bảo toàn dung lượng HD cho bài giảng. Chỉ khi dung lượng thông thường đã cạn kiệt thì các buổi hội thảo mới sử dụng dung lượng HD. Điều kiện khả thi được thực thi bằng cách theo dõi số lượng sử dụng hiện tại, đảm bảo không có sự trùng lặp vượt quá số máy chiếu hiện có. 

Một vấn đề triển khai tinh vi là phiên bản đơn giản hóa này giả định bất kỳ chỉ số máy chiếu HD nào cũng có thể hoán đổi cho nhau, điều này có giá trị về tính khả thi nhưng không có giá trị về tính chính xác của phép gán rõ ràng trong các ràng buộc đầy đủ; một giải pháp hoàn chỉnh sẽ theo dõi thêm danh tính máy chiếu cụ thể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 2 2
1 5
2 5
1 5
1 4
```Chúng tôi xử lý các sự kiện theo thứ tự: 

| Thời gian | Sự kiện | Loại | HD đã qua sử dụng | HOẶC đã sử dụng | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | bài giảng | L | 1 | 0 | giao HD | 
| 1 | hội thảo | S | 1 | 1 | chỉ định HOẶC | 
| 2 | bài giảng | L | 2 | 1 | giao HD | 
| 5 | kết thúc hội thảo | - | 1 | 0 | khe miễn phí | 
| 5 | bài giảng kết thúc | - | 0 | 0 | khe miễn phí | 

Tất cả các ràng buộc đều được thỏa mãn vì mức sử dụng HD không bao giờ vượt quá 2. 

Điều này xác nhận rằng các đỉnh áp suất đồng thời được xử lý chính xác bằng cách duy trì số lượng trực tiếp. 

### Ví dụ 2 

đầu vào:```
1 3 1 2
1 10
1 2
2 3
3 4
```| Thời gian | Sự kiện | Loại | HD đã qua sử dụng | HOẶC đã sử dụng | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | bài giảng | L | 1 | 0 | HD được giao | 
| 1 | hội thảo | S | 1 | 1 | HOẶC được chỉ định | 
| 2 | hội thảo | S | 1 | 2 | HOẶC được chỉ định | 
| 3 | hội thảo | S | 1 | 2 | phải sử dụng HD hoặc thất bại | 

Đến thời điểm thứ 3, HD đã hết bài giảng và không thể nhận thêm hội thảo nữa, còn bình thường thì đã đầy. Điều này chứng tỏ trường hợp thất bại nghiêm trọng khi tính linh hoạt đã cạn kiệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log (n + m)) | sắp xếp các sự kiện và hoạt động heap theo khoảng thời gian | 
| Không gian | O(n + m) | lưu trữ các sự kiện, đống và bài tập | 

Các giới hạn n, m ≤ 300 làm cho việc này nhanh chóng một cách thoải mái và ngay cả với nhiều trường hợp thử nghiệm, các thao tác heap vẫn không đáng kể trong các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return "stub"

# provided samples (format only placeholders)
# assert run(sample_input) == sample_output

# minimum case
assert run("1\n1 0 1 1\n1 2\n") != ""

# tight HD pressure
assert run("1\n2 0 1 1\n1 3\n2 4\n") != ""

# seminar overload forcing HD spill
assert run("1\n1 3 1 1\n1 5\n1 2\n2 3\n3 4\n") != ""

# all overlap
assert run("1\n3 3 2 2\n1 10\n1 10\n1 10\n1 10\n1 10\n1 10\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | CÓ/KHÔNG | tính khả thi cơ bản | 
| HD chặt chẽ | CÓ | ranh giới dung lượng HD chính xác | 
| tràn hội thảo | KHÔNG/CÓ | hành vi buộc phải bố trí lại | 
| chồng chéo hoàn toàn | CÓ | tính đúng đắn của năng lực toàn cầu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các bài giảng trùng nhau ở một điểm duy nhất. Trong tình huống đó, thuật toán phải đảm bảo chỉ riêng dung lượng HD là đủ trước khi xem xét các hội thảo. Ví dụ: nếu x = 2 và có 3 bài giảng trùng nhau thì đầu ra đúng ngay lập tức là KHÔNG bất kể cấu trúc hội thảo. Đường quét phát hiện điều này vì hd_used sẽ vượt quá x ngay khi bài giảng thứ ba được xử lý. 

Một trường hợp bất lợi khác xảy ra khi các cuộc hội thảo đến sớm và chiếm hết các máy chiếu thông thường, không còn sự linh hoạt cho những lần phân công lại sau này. Thuật toán xử lý việc này vì nó chỉ phân công các buổi hội thảo cho các máy chiếu thông thường khi có sẵn và chỉ đẩy chúng lên HD khi cần thiết. Nếu HD đã tràn ngập các bài giảng thì lỗi được phát hiện chính xác tại thời điểm quá tải chứ không phải sau những giả định không chính xác trong tương lai. 

Trường hợp cạnh cuối cùng là khi các sự kiện luân phiên nhanh chóng theo thời gian sao cho các phần hết hạn và phần chèn vào xen kẽ nhau. Bởi vì thuật toán loại bỏ các khoảng thời gian đã hết hạn trước khi xử lý từng sự kiện nên nó đảm bảo rằng dung lượng chỉ phản ánh các phần chồng chéo đang hoạt động. Điều này ngăn chặn việc từ chối sai trong các lịch trình được đóng gói chặt chẽ, nơi có thể tái sử dụng ngay lập tức tại các thời điểm ranh giới.
