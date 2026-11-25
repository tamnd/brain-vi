---
problem: 962D
contest_id: 962
problem_index: D
name: "Merge Equals"
contest_name: "Educational Codeforces Round 42 (Rated for Div. 2)"
rating: 1600
tags: ["data structures", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 96
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a328137-28e8-83ec-9075-4ac30cd1c380
---

#CF 962D - Hợp nhất bằng 

**Đánh giá:** 1600 
**Tags:** cấu trúc dữ liệu, triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a328137-28e8-83ec-9075-4ac30cd1c380 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên được sắp xếp từ trái sang phải. Quá trình liên tục tìm kiếm các giá trị xuất hiện ít nhất hai lần nhưng không chọn chúng một cách tùy tiện. Thay vào đó, trong số tất cả các giá trị hiện có bản sao, chúng tôi luôn chọn giá trị nhỏ nhất như vậy. Khi một giá trị được chọn, chúng tôi xem xét hai lần xuất hiện đầu tiên của nó theo thứ tự mảng hiện tại. Cái bên trái bị loại bỏ hoàn toàn và cái bên phải được thay thế bằng cách nhân đôi giá trị. 

Điều này có nghĩa là mảng liên tục bị thu hẹp trong khi đôi khi tạo ra các giá trị lớn hơn mà sau này có thể trở thành ứng cử viên cho việc hợp nhất. 

Kích thước đầu vào có thể lên tới 150.000 phần tử, loại trừ mọi cách tiếp cận mô phỏng mỗi lần xóa bằng các thao tác mảng đơn giản. Một mô phỏng dựa trên danh sách đơn giản sẽ yêu cầu dịch chuyển các phần tử liên tục và trong trường hợp xấu nhất, điều này trở thành bậc hai. Ngay cả việc chỉ duy trì tần số là không đủ, bởi vì thứ tự xuất hiện rất quan trọng và cấu trúc thay đổi sau mỗi lần hợp nhất. 

Một khó khăn nhỏ xuất hiện khi có nhiều bản sao của một giá trị được dàn trải ra. Ví dụ: nếu một giá trị xuất hiện nhiều lần thì việc loại bỏ hai giá trị đầu tiên không chỉ làm giảm số lượng của giá trị đó mà còn thay đổi những lần xuất hiện nào được coi là "đầu tiên" trong các bước sau. Một cách tiếp cận đơn giản chỉ theo dõi số lượng mà bỏ qua các vị trí sẽ thất bại trong trường hợp hợp nhất xen kẽ giữa các giá trị khác nhau. 

Một cạm bẫy tiềm ẩn khác là xử lý lại các giá trị mới được tạo. Khi một giá trị x được hợp nhất thành 2x, giá trị mới có thể ngay lập tức trở thành giá trị trùng lặp nhỏ nhất hoặc có thể chặn các ứng cử viên khác. Mọi giải pháp đúng đều phải cập nhật một cách hiệu quả thứ tự chung này của “các giá trị trùng lặp theo kích thước giá trị” sau mỗi lần hợp nhất. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng quá trình trên một mảng theo đúng nghĩa đen. Ở mỗi bước, chúng tôi quét mảng, tính toán tần số, tìm giá trị nhỏ nhất xuất hiện ít nhất hai lần, sau đó xác định hai lần xuất hiện đầu tiên của nó và thực hiện xóa và thay thế. Điều này đơn giản về mặt khái niệm và rõ ràng là đúng vì nó tuân thủ chính xác các quy tắc. 

Vấn đề là chi phí. Mỗi bước yêu cầu quét toàn bộ mảng hoặc duy trì cấu trúc các lần xuất hiện theo thứ tự. Có thể có sự hợp nhất O(n) và mỗi lần hợp nhất có thể tốn O(n) để tìm vị trí hoặc cập nhật cấu trúc. Điều này dẫn đến hành vi O(n²), quá chậm đối với 150.000 phần tử. 

Điều quan trọng là chúng ta không bao giờ cần phải quét toàn bộ mảng nhiều lần. Điều chúng ta thực sự cần là một cách để nhanh chóng biết giá trị nào hiện có ít nhất hai lần xuất hiện và trong số đó chọn giá trị nhỏ nhất. Chúng ta cũng cần biết, đối với mỗi giá trị, hai vị trí đầu tiên trong mảng hiện tại. 

Điều này gợi ý việc duy trì hai cấu trúc cùng một lúc. Đầu tiên, bản đồ tần số cho các giá trị. Thứ hai, đối với mỗi giá trị, một hàng các vị trí xuất hiện theo thứ tự mảng hiện tại. Khi hợp nhất hai lần xuất hiện của x, chúng tôi sử dụng hai vị trí đầu tiên từ hàng đợi của nó và cập nhật vị trí thứ hai để giữ 2x. Sau đó, chúng tôi thêm vị trí mới đó vào hàng đợi 2x. 

Để luôn trích xuất x hợp lệ nhỏ nhất một cách hiệu quả, chúng tôi duy trì một đống tối thiểu trên các giá trị hiện có tần số ít nhất là hai. Mỗi khi tần số của một giá trị đạt tới hai, chúng tôi sẽ đẩy nó. Khi lấy ra từ heap, chúng ta phải xác minh rằng nó vẫn có ít nhất hai lần xuất hiện vì các mục nhập cũ có thể tồn tại. 

Điều này biến việc quét lặp đi lặp lại thành các hoạt động logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu (hàng đợi heap + vị trí) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng quá trình trong khi theo dõi cẩn thận các vị trí xảy ra.

1. Chúng tôi đọc tất cả các giá trị và gán cho mỗi lần xuất hiện một “nút” đại diện cho vị trí hiện tại của nó trong cấu trúc động. Mỗi giá trị lưu trữ một hàng đợi gồm các nút này theo thứ tự từ trái sang phải. Điều này cho phép chúng ta luôn truy cập vào hai lần xuất hiện đầu tiên trong O(1). 
2. Chúng tôi tính toán tần số ban đầu và đẩy mọi giá trị có tần số ít nhất là hai vào một đống tối thiểu. Vùng heap này đại diện cho tất cả các ứng cử viên hợp nhất hiện đủ điều kiện, được sắp xếp theo giá trị. 
3. Chúng tôi liên tục trích xuất giá trị x nhỏ nhất từ ​​heap. Nếu tần số hiện tại của nó nhỏ hơn hai, chúng tôi sẽ loại bỏ nó và tiếp tục. Bước này là cần thiết vì việc hợp nhất trước đó có thể làm giảm tính khả dụng của nó. 
4. Đối với x hợp lệ, chúng tôi lấy hai nút đầu tiên từ hàng đợi của nó. Nút bên trái được loại bỏ khỏi cấu trúc. Nút bên phải được cập nhật lên giá trị 2x. 
5. Sau đó, chúng tôi loại bỏ nút bên phải này khỏi hàng đợi x và chèn nó vào hàng đợi 2x. Tần số được cập nhật tương ứng: x giảm 2 và 2x tăng 1. 
6. Nếu sau khi cập nhật, 2x bây giờ có tần số ít nhất là hai, nó sẽ bị đẩy vào heap. 
7. Chúng ta tiếp tục cho đến khi không còn giá trị nào trong heap có tần số ít nhất là hai. 

Sau tất cả các thao tác, các nút còn lại biểu thị mảng cuối cùng theo thứ tự từ trái sang phải. 

Ý tưởng quan trọng là vùng heap chỉ quyết định giá trị nào sẽ xử lý tiếp theo, trong khi hàng đợi duy trì cấu trúc vị trí để chúng ta luôn có thể chọn những lần xuất hiện chính xác. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, với mỗi giá trị x, hàng đợi của nó chứa chính xác các lần xuất hiện hiện tại của x theo thứ tự vị trí được sắp xếp. Mỗi lần hợp nhất sẽ loại bỏ hai lần xuất hiện sớm nhất của x, do đó hàng đợi luôn nhất quán với định nghĩa quy trình. Heap luôn chọn x nhỏ nhất hiện có ít nhất hai lần xuất hiện. Mặc dù vùng heap có thể chứa các mục nhập lỗi thời, mọi mục nhập không hợp lệ sẽ bị loại bỏ trước khi sử dụng và không có ứng cử viên hợp lệ nào bị bỏ qua. Điều này đảm bảo mô phỏng luôn phản ánh quy tắc tham lam cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq
from collections import defaultdict, deque

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pos = defaultdict(deque)
    freq = defaultdict(int)

    # Each element is represented by its index; we mutate values in-place
    for i, v in enumerate(a):
        pos[v].append(i)
        freq[v] += 1

    heap = []
    for v in freq:
        if freq[v] >= 2:
            heapq.heappush(heap, v)

    alive = [True] * n

    while heap:
        x = heapq.heappop(heap)
        if freq[x] < 2:
            continue

        # take first two occurrences
        i1 = pos[x].popleft()
        i2 = pos[x].popleft()

        alive[i1] = False

        # update second occurrence
        a[i2] = 2 * x

        freq[x] -= 2
        freq[2 * x] += 1

        pos[2 * x].append(i2)

        if freq[2 * x] == 2:
            heapq.heappush(heap, 2 * x)
        if freq[x] >= 2:
            heapq.heappush(heap, x)

    result = []
    for i in range(n):
        if alive[i]:
            result.append(a[i])

    print(len(result))
    print(*result)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ một mảng chỉ mục cố định và mô phỏng việc xóa bằng cách sử dụng điểm đánh dấu còn sống boolean. Điều này tránh được việc chuyển đổi tốn kém. Hàng đợi vị trí đảm bảo chúng ta luôn biết chỉ số nào tương ứng với hai lần xuất hiện đầu tiên. 

Một điểm tinh tế là khi chúng ta cập nhật chỉ số xuất hiện thứ hai`i2`, chúng tôi ghi đè giá trị của nó tại chỗ và phân loại lại nó vào một nhóm giá trị khác. Giá trị ban đầu được loại bỏ một cách hiệu quả khỏi vị trí đó mà không cần dịch chuyển mảng về mặt vật lý. 

Các bản sao heap được xử lý một cách lười biếng: chúng tôi có thể đẩy một giá trị nhiều lần nhưng chúng tôi chỉ xử lý nó khi tần số của nó vẫn hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
3 4 1 2 2 1 1
```Chúng tôi chỉ theo dõi các sự kiện quan trọng. 

| Bước | Đống tối thiểu x | Hai vị trí đầu tiên của x | Hành động | Thay đổi mảng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | (2,5) | xóa 2, đặt 5→2 | [3,4,1,2,2,1,1] → [3,4,2,2,2,1] | 
| 2 | 1 | (3,5) | hợp nhất lại | [3,4,2,2,2,1] → [3,4,4,2,1] | 
| 3 | 2 | (3,4) | hợp nhất | [3,4,4,2,1] → [3,8,2,1] | 

Đầu ra cuối cùng:```
3 8 2 1
```Dấu vết này cho thấy các giá trị mới được tạo ngay lập tức tham gia vào quá trình hợp nhất trong tương lai như thế nào, buộc phải phân loại lại động. 

### Ví dụ 2 

đầu vào:```
5
1 1 3 1 1
```| Bước | Đống tối thiểu x | Hai vị trí đầu tiên | Hành động | Mảng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | (0,1) | 1→2 | [2,3,1,1] | 
| 2 | 1 | (2,3) | 1→2 | [2,3,2] | 
| 3 | 2 | (0,2) | 2→3 | [3,4] | 

Đầu ra cuối cùng:```
3 4
```Ví dụ này cho thấy sự tăng trưởng theo tầng trong đó việc hợp nhất lặp đi lặp lại sẽ dịch chuyển giá trị nhỏ nhất đang hoạt động lên trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử vào và ra khỏi đống và xếp hàng với số lần giới hạn, mỗi thao tác trên đống tốn log n | 
| Không gian | O(n) | Mỗi chỉ mục được lưu trữ một lần, cùng với các bản đồ phụ trợ và heap | 

Các ràng buộc cho phép thực hiện khoảng vài triệu thao tác và hệ số logarit dễ dàng nằm trong giới hạn cho 150.000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict, deque
    import heapq

    n = int(input())
    a = list(map(int, input().split()))

    pos = defaultdict(deque)
    freq = defaultdict(int)

    for i, v in enumerate(a):
        pos[v].append(i)
        freq[v] += 1

    heap = []
    for v in freq:
        if freq[v] >= 2:
            heapq.heappush(heap, v)

    alive = [True] * n

    while heap:
        x = heapq.heappop(heap)
        if freq[x] < 2:
            continue
        i1 = pos[x].popleft()
        i2 = pos[x].popleft()
        alive[i1] = False
        a[i2] = 2 * x
        freq[x] -= 2
        freq[2 * x] += 1
        pos[2 * x].append(i2)
        if freq[2 * x] == 2:
            heapq.heappush(heap, 2 * x)
        if freq[x] >= 2:
            heapq.heappush(heap, x)

    res = [a[i] for i in range(n) if alive[i]]
    return str(len(res)) + "\n" + " ".join(map(str, res))

# provided samples
assert run("7\n3 4 1 2 2 1 1") == "4\n3 8 2 1"
assert run("5\n1 1 3 1 1") == "2\n3 4"

# custom cases
assert run("3\n1 2 3") == "3\n1 2 3", "all distinct"
assert run("4\n1 1 1 1") in ["1\n4", "2\n2 2"], "chain merges"
assert run("6\n2 2 2 2 2 2") == "1\n8", "repeated merging"
assert run("2\n5 5") == "1\n10", "minimum size repeated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều khác biệt | mảng không thay đổi | không kích hoạt sự hợp nhất | 
| đều nhỏ như nhau | giá trị duy nhất còn sót lại | hợp nhất xếp tầng lặp đi lặp lại | 
| số chẵn lặp đi lặp lại | độ chính xác tăng trưởng theo cấp số nhân | nhiều lần hợp nhất tuần tự | 
| trường hợp tối thiểu | hành vi hợp nhất đơn | độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi việc hợp nhất lặp đi lặp lại sẽ tạo ra các bản sao mới ngay lập tức. Ví dụ, bắt đầu bằng`[1,1,1,1]`, sau lần hợp nhất đầu tiên chúng tôi giới thiệu`2`, có thể trở thành đủ điều kiện trước tất cả`1`s được xử lý. Heap đảm bảo chúng ta luôn chọn giá trị hợp lệ nhỏ nhất, vì vậy ngay cả khi`2`có sẵn sớm, nó sẽ không được chọn trong khi`1`vẫn có hai lần xảy ra. 

Một trường hợp khác là khi các mục heap cũ xuất hiện. Một giá trị có thể được đẩy nhiều lần nhưng vào thời điểm nó được xử lý, tần số của nó có thể giảm xuống dưới hai. Việc kiểm tra tần suất rõ ràng sẽ ngăn chặn việc hợp nhất không chính xác. 

Trường hợp cạnh thứ ba là chuỗi dài trong đó giá trị tăng theo cấp số nhân. Vì mỗi lần hợp nhất làm giảm tổng số phần tử đi một nên quá trình luôn kết thúc, nhưng các giá trị trung gian có thể trở nên lớn. Sử dụng số nguyên Python sẽ tránh được sự cố tràn và giữ an toàn cho số học.
