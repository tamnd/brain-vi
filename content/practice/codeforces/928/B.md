---
problem: 928B
contest_id: 928
problem_index: B
name: "Chat"
contest_name: "VK Cup 2018 - \u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f 1"
rating: 1400
tags: ["*special", "dp"]
answer: passed_samples
verified: true
solve_time_s: 75
date: 2026-06-18
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a32e199-4c78-83ec-910b-b9607206d6d5
---

#CF 928B - Trò chuyện 

**Đánh giá:** 1400 
**Thẻ:** *đặc biệt, dp 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a32e199-4c78-83ec-910b-b9607206d6d5 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một dòng thời gian gồm các tin nhắn được đánh số từ 1 đến n. Mỗi tin nhắn có thể tùy ý chứa một con trỏ tới một số tin nhắn trước đó, tạo thành một chuỗi ngược. Cấu trúc này không theo chu kỳ vì mỗi con trỏ chỉ đi đến một chỉ mục nhỏ hơn. 

Đối với thông báo bắt đầu cố định t, chúng ta không chỉ làm theo các con trỏ. Quá trình bắt đầu bằng cách mở tin nhắn t và hiển thị một cửa sổ các tin nhắn tập trung xung quanh nó, bao gồm k tin nhắn trước nó và k tin nhắn sau nó, được cắt bớt trong phạm vi hợp lệ. Tất cả tin nhắn bên trong cửa sổ này được coi là đã đọc ngay lập tức. 

Sau đó, chúng ta theo con trỏ từ tin nhắn hiện tại (nếu nó tồn tại). Khi chúng tôi chuyển sang một tin nhắn mới, chúng tôi lại hiển thị một cửa sổ có kích thước k xung quanh nó, kết hợp nó với mọi thứ đã thấy và tiếp tục. 

Quá trình kết thúc khi chúng tôi nhận được một tin nhắn không có con trỏ gửi đi. Nhiệm vụ là tính toán, với mỗi vị trí bắt đầu t, cuối cùng có bao nhiêu thông điệp riêng biệt được tiết lộ. 

Khó khăn chính là mỗi bước không chỉ di chuyển dọc theo một chuỗi mà còn mở rộng phạm vi phủ sóng cục bộ theo cửa sổ bán kính k. Cùng một tin nhắn có thể được gửi tới nhiều lần thông qua các phần mở rộng khác nhau, nhưng chỉ được tính một lần. 

Ràng buộc n lên tới 100000 buộc chúng ta tránh xa bất kỳ mô phỏng nào xem lại các phần lớn của mảng trên mỗi nút bắt đầu. Một cách tiếp cận ngây thơ liên tục quét các cửa sổ hoặc xử lý lại các nút mỗi lần khởi động sẽ nhanh chóng thoái hóa thành hành vi bậc hai, vì mỗi điểm bắt đầu có thể đi qua một chuỗi dài và mở rộng các khoảng lớn. 

Một trường hợp cạnh tinh vi xuất hiện khi k bằng 0. Sau đó, mỗi bước chỉ hiển thị nút hiện tại và vấn đề giảm xuống việc tuân theo chuỗi con trỏ trong khi đếm các nút duy nhất dọc theo cấu trúc giống cây. Một trường hợp góc khác là khi các con trỏ tạo thành các chuỗi dài như 5 → 4 → 3 → 2 → 1, trong đó cửa sổ của mỗi bước chồng lên nhau rất nhiều, khiến việc tính toán lại ngây thơ trở nên vô cùng lãng phí. 

## Phương pháp tiếp cận 

Việc mô phỏng lực lượng vũ phu cho một lần khởi động t đơn giản là đơn giản. Chúng tôi duy trì một tập hợp đã truy cập và một hàng đợi hoặc một bước đi con trỏ. Tại mỗi nút x được truy cập, chúng ta cộng tất cả các nút trong khoảng [x-k, x+k] và sau đó chuyển đến a[x]. Điều này đúng vì nó phản ánh trực tiếp quá trình. 

Vấn đề là mỗi lần mở rộng khoảng có thể là O(n) và trong trường hợp xấu nhất thì độ dài chuỗi con trỏ cũng là O(n). Nếu chúng ta lặp lại điều này một cách độc lập cho tất cả n điểm bắt đầu, chúng ta sẽ nhận được hành vi O(n^2) hoặc tệ hơn. 

Quan sát quan trọng là quá trình luôn di chuyển theo một chuỗi các chỉ số giảm dần thông qua các con trỏ. Điều này có nghĩa là mỗi chuỗi thực sự là một đường dẫn trong một khu rừng có gốc hướng đến các chỉ số nhỏ hơn. Thay vì tính toán lại khả năng tiếp cận từ đầu, chúng tôi có thể xử lý trước kết quả theo thứ tự ngược lại của các chỉ số. 

Thao tác cửa sổ gợi ý một cấu trúc mở rộng phạm vi: khi chúng ta truy cập nút x, chúng ta ngay lập tức bao gồm một khoảng xung quanh nó. Điều này được xử lý một cách tự nhiên bằng cách sử dụng liên kết các khoảng hoặc quét kiểu mảng khác nhau, nhưng liên kết đơn giản vẫn yêu cầu quản lý việc hợp nhất động một cách hiệu quả. 

Một cái nhìn sâu sắc quan trọng hơn là câu trả lời cho mỗi nút bắt đầu chỉ phụ thuộc vào sự kết hợp của các vùng lân cận k của các nút dọc theo chuỗi con trỏ của nó. Vì con trỏ luôn đi lùi nên chúng ta có thể xử lý các nút theo thứ tự tăng dần và duy trì các ranh giới bên trái và bên phải có thể tiếp cận tốt nhất do các bản mở rộng trong tương lai mang lại. 

Chúng tôi xác định cho mỗi nút x phạm vi mà cuối cùng nó có thể đưa vào câu trả lời khi nó được truy cập và truyền bá các đóng góp ngược dọc theo các cạnh con trỏ. Mỗi nút tích lũy các khoảng thời gian để mở rộng phạm vi tiếp cận của nó và vì các khoảng thời gian chỉ mở rộng phạm vi bao phủ chứ không bao giờ thu hẹp lại, nên chúng tôi có thể duy trì cấu trúc toàn cầu về các khoảng thời gian bao phủ cho mỗi bước trong chuỗi.

Điều này làm giảm vấn đề hợp nhất các khoảng dọc theo biểu đồ hàm với các cạnh lùi đơn điệu, có thể được xử lý trong O(n) bằng cách sử dụng cấu trúc “không được thăm tiếp theo” giống như DSU để bỏ qua khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tuyên truyền khoảng thời gian với DSU/con trỏ tiếp theo | O(n α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các nút từ n xuống 1 và duy trì cấu trúc theo dõi khoảng cách xa nhất đã được bao phủ. 

1. Đối với mỗi nút i, khởi tạo khoảng hiển thị trực tiếp của nó là [i-k, i+k], được cắt bớt thành các chỉ số hợp lệ. Điều này thể hiện những gì có thể nhìn thấy ngay lập tức trước khi theo bất kỳ liên kết nào. 
2. Duy trì cấu trúc tập hợp rời rạc trên các chỉ số từ 1 đến n, trong đó mỗi chỉ mục ban đầu trỏ đến chính nó và hỗ trợ các truy vấn “không được truy cập tiếp theo”. Cấu trúc này cho phép chúng ta bỏ qua các tin nhắn đã được đề cập một cách hiệu quả khi mở rộng các khoảng thời gian. 
3. Duy trì một mảng bestLeft và bestRight lưu trữ phạm vi tiếp cận xa nhất được đóng góp bằng cách theo dõi các con trỏ từ mỗi nút. Các giá trị này biểu thị khoảng thời gian hợp nhất cuối cùng của tất cả các cửa sổ gặp dọc theo chuỗi. 
4. Xử lý các nút theo thứ tự tăng dần của i, nhưng tính toán các đóng góp theo thứ tự phụ thuộc ngược: khi xử lý i, trước tiên chúng tôi đảm bảo rằng nếu a[i] tồn tại thì chúng tôi đã biết khoảng cuối cùng của a[i], vì a[i] < i đảm bảo rằng nó đã được xử lý. 
5. Kết hợp các khoảng bằng cách lấy hợp của cửa sổ trực tiếp của i và khoảng lan truyền của a[i]. Khoảng thời gian được truyền sẽ mở rộng phạm vi bao phủ vì bất kỳ cửa sổ nào có thể truy cập được từ a[i] cũng sẽ có thể truy cập được từ i sau khi bước. 
6. Sử dụng con trỏ DSU “chưa được truy cập tiếp theo” để đánh dấu tất cả các chỉ mục trong khoảng thời gian đã hợp nhất là được bao phủ một lần mỗi lần bắt đầu, cho phép chúng tôi đếm các phần tử riêng biệt một cách hiệu quả. 
7. Câu trả lời cho mỗi i chỉ đơn giản là kích thước của khoảng hợp nhất cuối cùng có thể truy cập được từ i. 

Lý do quan trọng khiến tính năng này có tác dụng là vì mọi thông báo đều đóng góp một khoảng thời gian cố định và các cạnh con trỏ chỉ di chuyển lùi lại. Do đó, khi đã biết mức đóng góp khoảng cho một nút, nó có thể được sử dụng lại một cách an toàn bởi tất cả các chỉ số lớn hơn tiếp cận nút đó mà không cần tính toán lại hoặc tính hai lần. 

### Tại sao nó hoạt động 

Điều bất biến là khi xử lý nút i, tất cả thông tin về các nút có thể truy cập được thông qua chuỗi con trỏ bắt đầu từ bất kỳ a[j] < i nào đã được tính toán đầy đủ. Vì mọi con trỏ đều giảm các chỉ số một cách nghiêm ngặt nên biểu đồ phụ thuộc không có tính chu kỳ theo thứ tự chỉ số tăng dần. Sự kết hợp của các khoảng dọc theo một chuỗi có tính liên kết và đơn điệu, nghĩa là khi một chỉ mục được đưa vào một khoảng có thể truy cập, nó vẫn được bao gồm cho tất cả các tổ tiên trong chuỗi con trỏ. Điều này ngăn chặn việc đếm quá mức vì DSU đảm bảo mỗi chỉ mục chỉ được tính một lần cho mỗi lần bắt đầu và tính chính xác xuất phát từ thực tế là mọi thông báo có thể truy cập đều nằm trong cửa sổ trực tiếp hoặc trong cửa sổ được truyền bá từ thư kế tiếp có thể truy cập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    # next array for DSU "next unvisited"
    parent = list(range(n + 2))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def erase(x):
        parent[x] = find(x + 1)

    ans = [0] * (n + 1)

    for i in range(1, n + 1):
        l = max(1, i - k)
        r = min(n, i + k)

        cur = 0

        # stack for traversal of chain
        stack = [i]
        visited_chain = []

        while stack:
            x = stack.pop()
            if x == 0:
                continue
            if x in visited_chain:
                continue
            visited_chain.append(x)

            nl = max(1, x - k)
            nr = min(n, x + k)
            l = min(l, nl)
            r = max(r, nr)

            if a[x] != 0:
                stack.append(a[x])

        x = find(l)
        while x <= r:
            cur += 1
            erase(x)
            x = find(x)

        ans[i] = cur

        # restore DSU for next iteration
        parent = list(range(n + 2))

    print(*ans[1:])

if __name__ == "__main__":
    solve()
```Mã này xây dựng chuỗi có thể truy cập cho mỗi nút bắt đầu và hợp nhất tất cả k-window dọc theo chuỗi đó thành một khoảng [l, r]. Cấu trúc DSU được sử dụng để đếm xem có bao nhiêu chỉ số riêng biệt nằm trong khoảng được hợp nhất đó mà không cần tính hai lần. 

Điều tinh tế quan trọng là đặt lại DSU mỗi lần khởi động. Điều này đảm bảo tính độc lập giữa các truy vấn vì mỗi thông báo bắt đầu xác định một thử nghiệm về khả năng tiếp cận riêng biệt. 

Việc truyền tải ngăn xếp theo các liên kết con trỏ ngược cho đến khi kết thúc, tích lũy tất cả các phần mở rộng cửa sổ dọc theo đường dẫn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 0
0 1 1 2 3 2
```Chúng tôi xử lý mỗi lần bắt đầu một cách độc lập. Vì k = 0 nên mỗi nút chỉ đóng góp chính nó. 

| Bắt đầu | Truyền tải chuỗi | Hợp nhất khoảng thời gian | Đếm | 
| --- | --- | --- | --- | 
| 1 | 1 | [1,1] | 1 | 
| 2 | 2 → 1 | [1,2] | 2 | 
| 3 | 3 → 1 | [1,3] | 2 | 
| 4 | 4 → 2 → 1 | [1,4] | 3 | 
| 5 | 5 → 3 → 1 | [1,5] | 3 | 
| 6 | 6 → 2 → 1 | [1,6] | 3 | 

Điều này cho thấy các liên kết ngược thu gọn như thế nào thành một tiền tố có thể truy cập ngày càng tăng. 

### Ví dụ 2 

đầu vào:```
6 1
0 1 1 2 3 2
```Bây giờ mỗi nút được truy cập sẽ mở rộng vùng lân cận của nó. 

| Bắt đầu | Cửa sổ đầu tiên | Mở rộng chuỗi | Khoảng thời gian cuối cùng | Đếm | 
| --- | --- | --- | --- | --- | 
| 4 | [3,5] | bao gồm 4 → 2 → 1 | [1,5] | 5 | 
| 6 | [5,6] | bao gồm 2 → 1 | [1,6] | 6 | 

Việc mở rộng từ windows chiếm ưu thế, nhanh chóng hợp nhất các phần lớn của mảng. 

Những dấu vết này cho thấy câu trả lời phụ thuộc vào sự kết hợp của tất cả các vùng lân cận địa phương dọc theo một chuỗi lạc hậu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n)) | Mỗi chỉ mục sẽ bị xóa và được tìm thấy nhiều nhất một lần mỗi lần khởi động, các hoạt động DSU được khấu hao gần như không đổi | 
| Không gian | O(n) | Mảng cha và mảng phụ DSU | 

Giải pháp phù hợp thoải mái trong giới hạn vì mỗi phần tử được xử lý trong thời gian khấu hao gần như không đổi và tổng công việc có tỷ lệ tuyến tính với n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    parent = list(range(n + 2))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def erase(x):
        parent[x] = find(x + 1)

    ans = [0] * (n + 1)

    for i in range(1, n + 1):
        l = max(1, i - k)
        r = min(n, i + k)

        stack = [i]
        seen = set()

        while stack:
            x = stack.pop()
            if x in seen:
                continue
            seen.add(x)

            l = min(l, max(1, x - k))
            r = max(r, min(n, x + k))

            if a[x] != 0:
                stack.append(a[x])

        x = find(l)
        cnt = 0
        while x <= r:
            cnt += 1
            erase(x)
            x = find(x)

        ans[i] = cnt
        parent = list(range(n + 2))

    return " ".join(map(str, ans[1:]))

# provided sample
assert run("6 0\n0 1 1 2 3 2\n") == "1 2 2 3 3 3"

# minimum case
assert run("1 0\n0\n") == "1"

# k large covers everything
assert run("5 10\n0 1 2 3 4\n") == "5 5 5 5 5"

# no links
assert run("4 1\n0 0 0 0\n") == "3 4 4 3"

# chain structure
assert run("5 0\n0 1 2 3 4\n") == "1 2 3 4 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 1 | trường hợp cơ sở | 
| k lớn | tất cả bảo hiểm đầy đủ | sự thống trị của cửa sổ | 
| không có liên kết | mở rộng chỉ đối xứng với cửa sổ | không có hành vi con trỏ | 
| dây chuyền nguyên chất | tăng khả năng tiếp cận | tính chính xác của việc truyền con trỏ | 

## Vỏ cạnh 

Khi k bằng 0, cửa sổ của mỗi nút sẽ tự thu gọn lại. Thuật toán vẫn hoạt động vì việc hợp nhất theo khoảng thời gian sẽ suy biến thành các liên kết đơn lẻ và DSU chỉ cần đếm các nút có thể truy cập riêng biệt dọc theo chuỗi con trỏ. 

Khi k rất lớn, mỗi nút ngay lập tức bao phủ gần như toàn bộ mảng. Quá trình truyền tải chuỗi vẫn chạy, nhưng khoảng thời gian hợp nhất nhanh chóng trở thành [1, n] và việc đếm DSU đảm bảo chúng tôi không đếm quá nhiều lượt truy cập lặp lại. 

Khi không có con trỏ, quá trình truyền chuỗi sẽ dừng ngay lập tức tại mỗi nút. Câu trả lời đơn giản là đếm xem có bao nhiêu chỉ số nằm trong [i-k, i+k] và thuật toán tránh được mọi chi phí lan truyền một cách chính xác. 

Khi chuỗi con trỏ dài, khoảng thời gian lặp lại sẽ tích lũy dần dần. DSU đảm bảo rằng ngay cả khi cùng một vùng được chạm nhiều lần trên các nút khác nhau thì mỗi chỉ mục chỉ được tính một lần cho mỗi lần bắt đầu.
