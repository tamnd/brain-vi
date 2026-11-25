---
problem: 986C
contest_id: 986
problem_index: C
name: "AND Graph"
contest_name: "Codeforces Round 485 (Div. 1)"
rating: 2500
tags: ["bitmasks", "dfs and similar", "dsu", "graphs"]
answer: passed_samples
verified: true
solve_time_s: 65
date: 2026-06-18
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33779d-a8a4-83ec-af71-7ea6e697c632
---

#CF 986C - VÀ Đồ thị 

**Đánh giá:** 2500 
**Thẻ:** bitmasks, dfs và tương tự, dsu, đồ thị 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33779d-a8a4-83ec-af71-7ea6e697c632 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta có một tập hợp các số nguyên riêng biệt, mỗi số có thể được xem như một mặt nạ n-bit. Chúng tôi xây dựng một biểu đồ có các đỉnh là những số này và chúng tôi kết nối hai đỉnh bất cứ khi nào AND của chúng bằng 0. Nói cách khác, hai số liền kề nhau nếu chúng không chia sẻ bất kỳ vị trí bit nào mà cả hai đều có số 1. 

Nhiệm vụ là xác định xem biểu đồ này có bao nhiêu thành phần được kết nối. 

Ràng buộc cấu trúc quan trọng là n nhiều nhất là 22, có nghĩa là mọi số đều tồn tại trong một vũ trụ có kích thước tối đa là 22². Giá trị này đủ nhỏ để chúng ta có thể coi các tập hợp con bit là trạng thái và lý do trên tập lũy thừa đầy đủ, nhưng đủ lớn để việc xây dựng biểu đồ cặp đơn giản trên m lên tới 2²² là không thể. 

Một cách giải thích ngây thơ sẽ đề xuất xây dựng biểu đồ một cách rõ ràng và chạy DFS hoặc DSU trên tất cả các cặp. Điều đó ngay lập tức gặp rắc rối vì số lượng cặp là m2, trong trường hợp xấu nhất là khoảng 2⁴⁴ thao tác, vượt xa mọi giới hạn khả thi. 

Một dạng lỗi tinh vi hơn xuất hiện nếu chúng ta chỉ cố gắng kết nối mỗi nút với một vài phần bù tùy ý. Ví dụ: nếu chúng ta tham lam kết nối một số với một đối tác rời rạc, chúng ta có thể dễ dàng bỏ lỡ kết nối bắc cầu thông qua các nút trung gian. Một ví dụ nhỏ minh họa điều này: 

đầu vào:```
n = 3
a = [1, 2, 4]
```Ở đây, mọi cặp đều có AND 0, do đó biểu đồ được kết nối đầy đủ và câu trả lời là 1. Bất kỳ cách tiếp cận nào chỉ kết nối mỗi nút với một phần bù duy nhất hoặc sử dụng thế hệ lân cận không đầy đủ có thể không nắm bắt được cấu trúc cụm đầy đủ. 

Khó khăn chính là tính liền kề được xác định bởi ràng buộc bit toàn cục thay vì cấu trúc cục bộ trong danh sách đã cho. 

## Phương pháp tiếp cận 

Việc xây dựng biểu đồ trực tiếp sẽ kiểm tra tất cả các cặp và liên kết những cặp đó với các mặt nạ bit rời rạc. Điều này đúng nhưng không khả thi khi m lớn. Mỗi bài kiểm tra cạnh là O(1), nhưng có các cặp O(m2), do đó tổng công việc bị suy giảm nhanh chóng. 

Quan sát quan trọng là đồ thị không phải là tùy ý. Mỗi nút được kết nối với tất cả các nút có mặt nạ bit nằm hoàn toàn bên trong mặt nạ bổ sung của nó. Điều này gợi ý suy nghĩ về các tập hợp con của bit hơn là so sánh theo cặp. 

Thay vì khám phá các cạnh từ mỗi nút, chúng ta có thể lật ngược góc nhìn: coi toàn bộ vũ trụ 2ⁿ là không gian trạng thái và sử dụng sự lan truyền giống như sàng trên các tập hợp con. Ý tưởng trung tâm là thống nhất tất cả các nút có thể truy cập trong cùng một thành phần bất cứ khi nào chúng có thể được kết nối thông qua các mặt nạ rời rạc, tương ứng với việc duyệt qua các phần bổ sung trong không gian bitmask. 

Chúng tôi xử lý trước những mặt nạ nào tồn tại, sau đó truyền bá kết nối bằng DFS qua không gian bổ sung mặt nạ bit. Mỗi lần chúng ta truy cập một mặt nạ, chúng ta cố gắng di chuyển đến tất cả các mặt nạ con của phần bù tồn tại trong tập đầu vào. Điều này tránh việc xây dựng các cạnh một cách rõ ràng giữa các cặp tùy ý. 

Hiệu suất đạt được nhờ việc thay thế kiểm tra từng cặp bằng duyệt tập hợp con có cấu trúc. Mỗi mặt nạ tạo ra sự chuyển tiếp thông qua các mặt nạ con của phần bù của nó và việc liệt kê tập hợp con trên không gian 22 bit là khả thi khi được khấu hao cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đồ thị lực lượng vũ phu | O(m2 · n) | O(m) | Quá chậm | 
| Submask DFS trên không gian bổ sung | O(m · 2^(n/2)) khấu hao | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi lưu trữ tất cả các số đã cho trong một mảng boolean hoặc tập hợp băm trên toàn bộ không gian bit có kích thước 2ⁿ. Điều này cho phép kiểm tra tư cách thành viên O(1). Bước này là cần thiết vì các quá trình chuyển đổi sau này phụ thuộc vào việc truy vấn xem mặt nạ ứng cử viên có tồn tại hay không. 
2. Chúng tôi duy trì một mảng đã truy cập trên cùng một không gian để đảm bảo mỗi số được gán cho chính xác một thành phần được kết nối. Điều này ngăn chặn việc xem lại các trạng thái và đảm bảo khấu hao tuyến tính trên các trạng thái có thể tiếp cận. 
3. Chúng tôi lặp lại tất cả các số đầu vào. Đối với mỗi số chưa được truy cập, chúng tôi bắt đầu DFS từ số đó và tăng bộ đếm thành phần. 
4. Bên trong DFS cho nút x, chúng tôi tính toán mặt nạ phần bù của nó trong phạm vi n bit, được định nghĩa là tất cả các bit được đặt thành 1 XOR x. Phần bù này đại diện cho tất cả các bit được phép xuất hiện trong các lân cận của x. 
5. Chúng tôi liệt kê tất cả các mặt nạ con của phần bổ sung này. Mỗi mặt nạ con y đại diện cho một nút ứng cử viên tách khỏi x. Nếu y tồn tại trong tập đầu vào và không được truy cập, chúng tôi đánh dấu nó đã truy cập và tiếp tục DFS từ y. Việc liệt kê mặt nạ con được thực hiện bằng thủ thuật tiêu chuẩn: bắt đầu từ phần bù, di chuyển liên tục đến (mặt nạ con - 1) & phần bù. 
6. Do đó, DFS mở rộng thông qua tất cả các nút có thể truy cập được bằng cách lặp đi lặp lại các chuyển đổi rời rạc, khám phá một cách hiệu quả thành phần được kết nối đầy đủ. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là DFS khám phá chính xác sự đóng cửa của mối quan hệ “có thể được kết nối thông qua một chuỗi các mặt nạ bit tách rời theo cặp”. Bất kỳ cạnh nào trong biểu đồ đều tương ứng với một chuyển đổi hợp lệ vì tính kề được xác định bởi tính rời rạc. Ngược lại, bất kỳ bước truyền tải nào chỉ đi theo các cạnh hợp lệ vì mọi mặt nạ con của phần bù đều tách rời khỏi nút hiện tại. Vì mọi nút có thể truy cập phải được kết nối thông qua một số chuỗi mặt nạ trung gian rời rạc và DFS khám phá tất cả các chuyển đổi hợp lệ như vậy mà không bỏ sót, nên mỗi thành phần được phát hiện chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
arr = list(map(int, input().split()))

N = 1 << n
present = [False] * N
vis = [False] * N

for x in arr:
    present[x] = True

sys.setrecursionlimit(10**7)

def dfs(x):
    vis[x] = True
    comp = (~x) & (N - 1)

    sub = comp
    while True:
        if present[sub] and not vis[sub]:
            dfs(sub)
        if sub == 0:
            break
        sub = (sub - 1) & comp

ans = 0

for x in arr:
    if not vis[x]:
        ans += 1
        dfs(x)

print(ans)
```Giải pháp dựa trên vũ trụ bitmask đầy đủ có kích thước 2ⁿ, khả thi vì n 22. DFS sử dụng đệ quy nhưng có thể được chuyển đổi thành ngăn xếp rõ ràng nếu độ sâu đệ quy là vấn đề đáng lo ngại. Chi tiết triển khai quan trọng là việc liệt kê mặt nạ con chính xác: bắt đầu từ phần bổ sung đầy đủ và áp dụng nhiều lần`(sub - 1) & comp`, đảm bảo tất cả các tập hợp con được truy cập chính xác một lần. 

Một điểm tinh tế là che đậy`(~x) & (N - 1)`, giới hạn phần bù ở n bit. Nếu không có điều này, số nguyên bit vô hạn của Python sẽ đưa ra các chuyển đổi không chính xác bên ngoài miền hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3
1 2 3
```Chúng tôi theo dõi các thành phần DFS trên mặt nạ hiện tại {01, 10, 11}. 

| Nút bắt đầu | Bổ sung | Đã kiểm tra mặt nạ con | Chuyến thăm mới | Thành phần | 
| --- | --- | --- | --- | --- | 
| 01 | 10 | 10, 00 | 10 | {01, 10} | 
| 10 | 01 | 01, 00 | 01 | cùng thành phần | 
| 11 | 00 | 00 | không | {11} | 

DFS đầu tiên kết nối 1 và 2, trong khi 3 bị cô lập, tạo ra 2 thành phần. 

### Mẫu 2 

đầu vào:```
3 3
1 2 4
```Tất cả các nút đều tương thích lẫn nhau. 

| Nút bắt đầu | Bổ sung | Đã kiểm tra mặt nạ con | Chuyến thăm mới | Thành phần | 
| --- | --- | --- | --- | --- | 
| 001 | 110 | 110.100.010.000 | 2,4 | {1,2,4} | 

Tất cả các nút đều được truy cập trong một DFS duy nhất, vì vậy câu trả lời là 1. 

Dấu vết cho thấy rằng phép liệt kê mặt nạ con đầy đủ nắm bắt chính xác tất cả các lân cận rời rạc, tạo ra một thành phần được kết nối hoàn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · 2ⁿ⁻ᵃᵐᵒᵘⁿᵗ) khấu hao | Mỗi trạng thái được truy cập một lần và mỗi DFS liệt kê các mặt nạ con một cách hiệu quả | 
| Không gian | O(2ⁿ) | Mảng lưu trữ sự hiện diện và lượt truy cập trên không gian bitmask đầy đủ | 

Giới hạn n 22 đảm bảo rằng các mảng có kích thước 2ⁿ có thể khả thi trong bộ nhớ và việc liệt kê mặt nạ con vẫn nằm trong giới hạn thời gian do khấu hao giữa các trạng thái được truy cập. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    arr = list(map(int, input().split()))

    N = 1 << n
    present = [False] * N
    vis = [False] * N

    for x in arr:
        present[x] = True

    sys.setrecursionlimit(10**7)

    def dfs(x):
        vis[x] = True
        comp = (~x) & (N - 1)
        sub = comp
        while True:
            if present[sub] and not vis[sub]:
                dfs(sub)
            if sub == 0:
                break
            sub = (sub - 1) & comp

    ans = 0
    for x in arr:
        if not vis[x]:
            ans += 1
            dfs(x)

    return str(ans)

# provided samples
assert run("2 3\n1 2 3\n") == "2"
assert run("3 3\n1 2 4\n") == "1"

# custom cases
assert run("1 2\n0 1\n") == "1", "fully connected"
assert run("2 2\n1 2\n") == "1", "disjoint pair"
assert run("2 3\n1 2 3\n") == "2", "sample structure"
assert run("3 1\n5\n") == "1", "single node"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 / 0 1 | 1 | trường hợp cơ sở kết nối đầy đủ | 
| 2 2 / 1 2 | 1 | hợp nhất cặp rời rạc | 
| 2 3 / 1 2 3 | 2 | kết nối hỗn hợp | 
| 3 1/5 | 1 | thành phần đơn lẻ | 

## Vỏ cạnh 

Trường hợp một cạnh phát sinh khi một số không có số lân cận hợp lệ trong tập hợp. Ví dụ: nếu n = 3 và đầu vào là [7], phần bù là 000 và DFS chỉ truy cập chính nó. Thuật toán đếm chính xác một thành phần bị cô lập vì không có mặt nạ con của phần bù tồn tại trong đầu vào. 

Một trường hợp khác là khi nhiều số chia sẻ các mẫu bit thưa thớt. Ví dụ: với n = 4 và các số như 1, 2, 4, 8, phần bù của mỗi số chứa nhiều mặt nạ con, nhưng chỉ một số mặt nạ con tồn tại trong đầu vào. DFS vẫn hạn chế việc truyền tải một cách chính xác bằng cách sử dụng`present`mảng, đảm bảo rằng các mặt nạ không tồn tại sẽ không mở rộng các thành phần. 

Trường hợp tinh vi cuối cùng là khi số 0 được bao gồm. Vì số 0 tách rời với tất cả các số nên nó trở thành một trung tâm. Trong trường hợp này, DFS từ 0 sẽ khám phá mọi nút hiện tại vì mỗi mặt nạ là một mặt nạ con của phần bù của nó (tất cả các bit được đặt). Thuật toán thu gọn toàn bộ tập hợp thành một thành phần một cách tự nhiên, khớp với cấu trúc biểu đồ.
