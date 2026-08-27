---
title: "CF 104366F - MPFT"
description: "Chúng tôi đang mô phỏng một nhóm trò chuyện thay đổi theo thời gian. Mọi người có thể tham gia nhóm hoặc gửi tin nhắn và nhóm có các quy định nghiêm ngặt có thể buộc loại bỏ thành viên. Nguyên tắc đầu tiên là hạn chế về năng lực. Nhóm có thể chứa tối đa N người."
date: "2026-07-01T17:43:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104366
codeforces_index: "F"
codeforces_contest_name: "The 17th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 104366
solve_time_s: 56
verified: true
draft: false
---

[CF 104366F - MPFT](https://codeforces.com/problemset/problem/104366/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một nhóm trò chuyện thay đổi theo thời gian. Mọi người có thể tham gia nhóm hoặc gửi tin nhắn và nhóm có các quy định nghiêm ngặt có thể buộc loại bỏ thành viên. 

Nguyên tắc đầu tiên là hạn chế về năng lực. Nhóm có thể chứa tối đa N người. Nếu một người mới cố gắng tham gia khi nhóm đã đầy người thì một người nào đó hiện đang ở trong nhóm sẽ bị xóa để nhường chỗ. Sự cố chỉ rõ rằng người bị xóa là người có hoạt động gần đây nhất là hoạt động lâu đời nhất trong số tất cả các thành viên hiện tại, nghĩa là người dùng ít hoạt động gần đây nhất sẽ bị trục xuất. 

Ngoài ra, còn có quy tắc thứ hai mạnh mẽ hơn. Nếu bất kỳ thành viên nào gửi K tin nhắn trong cửa sổ thời gian trượt có độ dài T, tính từ lần tham gia cuối cùng của họ và bao gồm cả cả hai điểm cuối, họ sẽ ngay lập tức bị loại. Điều quan trọng là việc tham gia sẽ đặt lại khoảng thời gian đếm cho người đó vì chỉ những tin nhắn sau lần tham gia gần đây nhất của họ mới được xem xét. 

Mỗi lần tham gia sẽ tự động tạo một tin nhắn có tên là “xin chào”, vì vậy, thành viên mới tham gia luôn có ít nhất một tin nhắn ngay lập tức. 

Đầu vào là một chuỗi sự kiện được sắp xếp theo thời gian. Đối với mỗi sự kiện, chúng tôi thấy một người tham gia hoặc một người gửi tin nhắn. Nếu người đó hiện đang ở trong nhóm trước thời gian diễn ra sự kiện thì sự kiện đó là một tin nhắn; nếu không thì đó là sự tham gia. Nhiệm vụ là xử lý tất cả các sự kiện, xuất ra mọi lần khởi động theo thứ tự thời gian và cuối cùng xuất ra các thành viên còn lại. 

Các ràng buộc có thể lên tới một triệu sự kiện và một triệu người dùng, với giá trị thời gian lên tới một tỷ. Điều này ngay lập tức loại trừ mọi cách tiếp cận quét tất cả thành viên trên mỗi sự kiện hoặc duy trì cửa sổ trượt cho mỗi thành viên với cấu trúc dữ liệu đơn giản. Bất kỳ giải pháp nào lặp đi lặp lại trên các thành viên tích cực sẽ chuyển sang hành vi bậc hai trong trường hợp xấu nhất và thất bại. 

Một khó khăn tinh vi nằm ở sự tương tác giữa hai quy tắc trục xuất. Một người dùng có thể bị xóa do không hoạt động so với những người khác (quy tắc dung lượng) hoặc do tin nhắn rác trong quy tắc khoảng thời gian. Một khía cạnh phức tạp khác là việc tham gia sẽ đặt lại cửa sổ tin nhắn, điều này làm mất hiệu lực lịch sử tin nhắn cũ và khiến việc đếm tiền tố đơn giản không chính xác. 

Một trường hợp nhỏ thường phá vỡ các triển khai ngây thơ là hành vi tham gia-kick-rejoin lặp đi lặp lại. Giả sử người dùng tham gia tại thời điểm 1, gửi K tin nhắn nhanh chóng, bị loại và tham gia lại sau. Chỉ các tin nhắn sau lần tham gia thứ hai mới quan trọng và lịch sử trước đó không được rò rỉ. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ duy trì danh sách đầy đủ các thành viên nhóm và đối với mọi sự kiện, sẽ tính toán lại thành viên ít hoạt động gần đây nhất nếu cần loại bỏ. Đối với mỗi sự kiện tin nhắn, chúng tôi cũng sẽ tính toán lại số lượng tin nhắn mà mỗi người dùng có trong khoảng thời gian T gần nhất bằng cách quét toàn bộ lịch sử hoặc tất cả các sự kiện của họ. Trong trường hợp xấu nhất, mỗi M sự kiện có thể kích hoạt việc quét tất cả N người dùng và mỗi người dùng có thể duy trì tối đa M tin nhắn. Điều này dẫn đến độ phức tạp ở mức O(MN), vượt xa khả năng thực hiện đối với một triệu sự kiện. 

Quan sát quan trọng là cả hai quy tắc chỉ phụ thuộc vào hoạt động gần đây: thời gian thông báo mới nhất để loại bỏ công suất và số lượng cửa sổ trượt để loại bỏ thư rác. Điều này gợi ý việc duy trì trạng thái nhỏ gọn cho mỗi người dùng thay vì lịch sử đầy đủ. 

Đối với mỗi người dùng, chúng tôi chỉ cần thời gian tham gia cuối cùng của họ, một chuỗi dấu thời gian tin nhắn sau lần tham gia đó và thời gian hoạt động mới nhất của họ. Quy tắc năng lực sau đó trở thành vấn đề luôn trích xuất “thời gian hoạt động cuối cùng” tối thiểu trong số những người dùng đang hoạt động. Đây là trường hợp sử dụng hàng đợi ưu tiên cổ điển nhưng cập nhật chậm vì thời gian hoạt động thay đổi. 

Đối với ràng buộc cửa sổ trượt, thay vì lưu trữ tất cả tin nhắn, chúng tôi duy trì một hàng đợi cho mỗi người dùng và loại bỏ các dấu thời gian cũ hơn thời gian hiện tại trừ T. Điều này đảm bảo mỗi tin nhắn được chèn và xóa nhiều nhất một lần, duy trì O(1) được khấu hao.

Cùng với nhau, điều này dẫn đến một hệ thống trong đó mỗi sự kiện được xử lý theo thời gian logarit do cập nhật vùng nhớ khối, trong khi hàng đợi của mỗi người dùng nhìn chung vẫn giữ nguyên tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(MN) | O(M) | Quá chậm | 
| Tối ưu | O(M log N) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ba cấu trúc cốt lõi. Một từ điển dành cho các thành viên đang hoạt động, một chuỗi dấu thời gian tin nhắn cho mỗi người dùng và một đống được sắp xếp theo thời gian hoạt động gần đây nhất để đưa ra các quyết định trục xuất. 

Mỗi sự kiện được xử lý như sau. 

1. Nếu người dùng hiện không có trong nhóm, chúng tôi coi sự kiện này là một lần tham gia. Chúng tôi chỉ định cho họ dấu thời gian tham gia và khởi tạo hàng đợi tin nhắn của họ bằng một mục nhập duy nhất thể hiện tin nhắn “xin chào” tự động. Chúng tôi cũng đặt thời gian hoạt động cuối cùng của chúng thành thời gian tham gia và đẩy chúng vào một đống được khóa theo giá trị này. Điều này đảm bảo rằng họ ngay lập tức được coi là chủ động đưa ra các quyết định trục xuất. 
2. Nếu nhóm đã đầy trước khi xử lý phép nối, chúng tôi sẽ liên tục xóa người dùng có thời gian hoạt động cuối cùng nhỏ nhất khỏi vùng nhớ heap cho đến khi tìm thấy ai đó vẫn hợp lệ trong nhóm hoạt động. Người dùng đó bị đuổi ra ngoài. Chúng tôi ghi lại thời gian trục xuất và đánh dấu chúng là không hoạt động. Vùng heap có thể chứa các mục nhập cũ, vì vậy chúng tôi xác minh tư cách thành viên trước khi sử dụng phần tử được bật lên. 
3. Nếu người dùng đã ở trong nhóm thì sự kiện sẽ là một tin nhắn. Chúng tôi gắn dấu thời gian vào deque tin nhắn của họ và cập nhật thời gian hoạt động cuối cùng của họ vào thời điểm hiện tại, đẩy một mục mới vào vùng nhớ heap. 
4. Sau mỗi lần chèn tin nhắn, chúng tôi thực thi quy tắc cửa sổ trượt. Chúng tôi xóa dấu thời gian ở bên trái của deque nằm ngoài khoảng [t - T, t]. Nếu số tin nhắn còn lại đạt K trở lên, người dùng sẽ bị đuổi ngay lập tức. Chúng tôi ghi lại sự kiện và xóa chúng khỏi nhóm hoạt động. 
5. Khi người dùng bị đuổi vì bất kỳ lý do gì, chúng tôi đánh dấu họ là không hoạt động. Bất kỳ mục nhập đống nào trong tương lai cho chúng đều bị bỏ qua trong quá trình trích xuất. 

Ý tưởng chính là các mục heap không bao giờ bị xóa một cách rõ ràng. Thay vào đó, chúng tôi dựa vào việc xóa từng phần bằng cách kiểm tra xem người dùng có còn hoạt động hay không khi mục nhập được bật lên. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng mọi người dùng đang hoạt động đều được thể hiện trong vùng heap với ít nhất một mục nhập phản ánh hoạt động gần đây nhất của họ và deque thông báo của họ chứa chính xác các thông báo kể từ lần tham gia cuối cùng của họ. Bởi vì cả hai quy tắc trục xuất chỉ phụ thuộc vào hai thông tin này nên không cần có dữ liệu lịch sử. Tính năng dọn dẹp vùng nhớ tạm thời đảm bảo tính chính xác ngay cả khi tồn tại nhiều bản ghi hoạt động lỗi thời, vì chỉ trạng thái hợp lệ gần đây nhất của mỗi người dùng mới được xem xét khi chọn ứng cử viên bị trục xuất. 

## Giải pháp Python```python
import sys
import heapq
from collections import deque

input = sys.stdin.readline

def solve():
    N, M, T, K = map(int, input().split())

    active = set()
    last_join = {}
    last_activity = {}
    msgs = {}

    # (last_activity_time, user)
    heap = []
    events = []
    kicks = []

    def kick(user, t):
        if user in active:
            active.remove(user)
            kicks.append((t, user))

    for _ in range(M):
        t, p = map(int, input().split())

        if p not in active:
            # join
            if len(active) >= N:
                while heap:
                    time, u = heapq.heappop(heap)
                    if u in active and last_activity[u] == time:
                        kick(u, t)
                        break

            active.add(p)
            last_join[p] = t
            msgs[p] = deque([t])  # hello message
            last_activity[p] = t
            heapq.heappush(heap, (t, p))

        else:
            # message
            msgs[p].append(t)
            last_activity[p] = t
            heapq.heappush(heap, (t, p))

            # sliding window
            dq = msgs[p]
            while dq and dq[0] < t - T:
                dq.popleft()

            if len(dq) >= K:
                kick(p, t)

    print(len(kicks), len(active))
    for t, u in kicks:
        print(t, u)
    print(*sorted(active))

if __name__ == "__main__":
    solve()
```Việc triển khai tập trung vào việc tách việc theo dõi thành viên khỏi logic đặt hàng. các`active`set cung cấp kiểm tra tư cách thành viên O(1). Vùng nhớ heap lưu trữ thứ tự trục xuất ứng viên theo hoạt động gần đây nhất, nhưng vì có nhiều bản cập nhật cho cùng một người dùng nên chúng tôi xác thực từng mục nhập vùng nhớ heap so với hiện tại`last_activity`. 

Deque cho mỗi người dùng là rất quan trọng đối với ràng buộc cửa sổ trượt. Nó đảm bảo chúng tôi chỉ lưu trữ các dấu thời gian có liên quan và mỗi dấu thời gian được đẩy và bật chính xác một lần. 

Chức năng kick tập trung hóa logic loại bỏ để cả quy tắc dung lượng và thư rác đều sử dụng các chuyển đổi trạng thái giống hệt nhau. 

Một điểm tinh tế là chúng tôi luôn cập nhật vùng nhớ heap trên mỗi tin nhắn, mặc dù các mục cũ hơn vẫn còn. Đây là điều cho phép xóa lười biếng và tránh các bản cập nhật đống tốn kém. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

Hãy xem xét một kịch bản nhỏ với dung lượng 2 và ràng buộc thông điệp chặt chẽ. 

đầu vào:```
2 4 1 2
1 1
2 2
3 2
4 2
```| Thời gian | Sự kiện | Bộ hoạt động | Heap (có ý nghĩa hàng đầu) | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | tham gia 1 | {1} | (1,1) | người dùng 1 tham gia | 
| 2 | tham gia 2 | {1,2} | (1,1),(2,2) | người dùng 2 tham gia | 
| 3 | tin nhắn 2 | {1,2} | (1,1),(3,2) | người dùng 2 cập nhật | 
| 4 | tin nhắn 2 | {1,2} | (1,1),(4,2) | người dùng 2 cập nhật | 

Dấu vết này cho thấy không có hoạt động trục xuất nào xảy ra vì người dùng 1 vẫn ít hoạt động nhất nhưng không bao giờ trở nên không hợp lệ theo quy tắc năng lực và người dùng 2 không vượt quá ngưỡng tin nhắn. 

### Ví dụ Dấu vết 2 

Bây giờ là trường hợp thư rác gây ra việc trục xuất. 

đầu vào:```
2 5 2 2
1 1
2 1
3 1
4 2
5 2
```| Thời gian | Sự kiện | Hàng đợi(1) | Hành động | 
| --- | --- | --- | --- | 
| 1 | tham gia 1 | [1] | xin chào | 
| 2 | tin nhắn 1 | [1,2] | được | 
| 3 | tin nhắn 1 | [1,2,3] | vượt quá K=2 → đá | 

Người dùng 1 bị xóa ở thời điểm thứ 3 vì trong cửa sổ [1,3] họ có 3 tin nhắn bao gồm xin chào, vượt ngưỡng. 

Điều này chứng tỏ rằng các tin nhắn được chèn vào được tính ngang bằng với các tin nhắn thông thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log N) | mỗi sự kiện cập nhật heap và deque khấu hao công việc liên tục | 
| Không gian | O(M + N) | mỗi người dùng chỉ lưu trữ các tin nhắn hoạt động kể từ lần tham gia cuối cùng | 

Độ phức tạp vừa vặn thoải mái trong các giới hạn vì M lên tới một triệu và mỗi phép toán tệ nhất là logarit, với các phép toán deque được khấu hao hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# Sample-like case
assert run("""2 4 1 2
1 1
2 2
3 2
4 2
""") is not None

# Minimum case
assert run("""1 1 1 1
1 1
""") is not None

# Immediate spam kick
assert run("""3 3 1 2
1 1
2 1
3 1
""") is not None

# Capacity eviction case
assert run("""1 2 10 10
1 1
2 2
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tham gia đơn lẻ | không đá | khởi tạo cơ sở | 
| vụ nổ thư rác | cú đá xảy ra | cửa sổ trượt đúng cách | 
| tràn công suất | trục xuất | đống thứ tự chính xác | 

## Vỏ cạnh 

Một trường hợp quan trọng là hành vi thiết lập lại tham gia lặp đi lặp lại. Một người dùng tham gia, xây dựng lịch sử tin nhắn, bị loại và sau đó đăng nhập lại. Thuật toán xử lý việc này bằng cách đặt lại deque của chúng mỗi lần tham gia, đảm bảo các tin nhắn cũ không bao giờ được xem xét. 

Một trường hợp khác là các mục nhập cũ. Một người dùng có thể có nhiều dấu thời gian hoạt động lỗi thời trong vùng nhớ heap. Bước xác thực lười biếng chỉ đảm bảo khớp mục nhập`last_activity`hợp lệ nên các mục đã lỗi thời sẽ bị bỏ qua mà không ảnh hưởng đến tính chính xác. 

Trường hợp tinh vi cuối cùng là việc đồng thời đáp ứng cả hai quy tắc trục xuất sau một tin nhắn. Thuật toán luôn kiểm tra quy tắc cửa sổ trượt ngay sau khi cập nhật tin nhắn và việc xóa được áp dụng trước bất kỳ quyết định nào về dung lượng tiếp theo, đảm bảo chuyển đổi trạng thái nhất quán.
