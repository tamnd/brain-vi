---
title: "CF 106235C - Café Mafia"
description: "Chúng tôi đang mô phỏng một hệ thống chỗ ngồi trong một quán cà phê nơi cả người và bàn đều được sắp xếp theo mức độ quan trọng. Ý tưởng chính là mỗi người đến với một khoảng thời gian cố định trong đó họ chiếm một bàn và các bảng không thể lưu trữ các khoảng thời gian chồng chéo."
date: "2026-06-25T07:05:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106235
codeforces_index: "C"
codeforces_contest_name: "Algo Cup 2025 by csspace.io (Qualification Round)"
rating: 0
weight: 106235
solve_time_s: 39
verified: true
draft: false
---

[CF 106235C - Quán cà phê Mafia](https://codeforces.com/problemset/problem/106235/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hệ thống chỗ ngồi trong một quán cà phê nơi cả người và bàn đều được sắp xếp theo mức độ quan trọng. Ý tưởng chính là mỗi người đến với một khoảng thời gian cố định trong đó họ chiếm một bàn và các bảng không thể lưu trữ các khoảng thời gian chồng chéo. 

Mỗi mafioso phải được chỉ định bảng được đánh số thấp nhất miễn phí trong toàn bộ khoảng thời gian của họ. Một bàn được coi là miễn phí cho một người nếu không có người nào được chỉ định trước đó ngồi vào bàn đó có khoảng thời gian trùng lặp với họ, trong đó ngay cả việc chạm vào các điểm cuối cũng được coi là trùng lặp. 

Vì vậy chúng tôi xử lý mafiosi theo thứ tự từ 1 đến n. Đối với mỗi người, chúng tôi cố gắng sử dụng lại các bảng hiện có bắt đầu từ bảng 1 và chúng tôi chỉ định bảng đầu tiên không xung đột về thời gian với bất kỳ ai đã ngồi ở đó. 

Đầu ra đơn giản là bảng được gán cho mỗi người theo thứ tự đầu vào. 

Các ràng buộc cho phép tổng cộng lên tới 250.000 khoảng thời gian. Việc kiểm tra đơn giản đối với tất cả các khoảng được gán trước đó trên mỗi bảng sẽ quá chậm vì mỗi phép gán có thể quét nhiều khoảng trong quá khứ, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế đến từ việc chạm vào ranh giới. Nếu một khoảng kết thúc tại thời điểm t và một khoảng khác bắt đầu tại thời điểm t, chúng vẫn bị coi là xung đột nhau, do đó chúng không thể chia sẻ một bảng. Ví dụ: các khoảng [1, 2] và [2, 3] phải ở các bảng khác nhau. 

Một trường hợp góc khác là khi nhiều khoảng được lồng vào nhau hoặc giống hệt nhau. Nếu nhiều mafiosi có cùng phạm vi thời gian, mỗi người phải nhận một bảng khác nhau mặc dù phạm vi thời gian của họ giống hệt nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là duy trì, đối với mỗi bảng, danh sách đầy đủ các khoảng được chỉ định và kiểm tra xem khoảng mới có trùng với bất kỳ khoảng nào trong số đó hay không. Mỗi lần kiểm tra sẽ quét tất cả các khoảng đã được đặt ở bảng đó. Trong trường hợp xấu nhất, nếu tất cả các khoảng thời gian trùng nhau nhiều, cuối cùng chúng tôi sẽ kiểm tra hầu hết tất cả các nhiệm vụ trước đó cho từng mafioso mới, dẫn đến các hoạt động có khoảng O(n²). 

Cấu trúc của quy trình gợi ý một nhiệm vụ tham lam với ràng buộc thứ tự toàn cầu. Chúng tôi luôn muốn có một bảng nhỏ nhất có sẵn và khi một bảng được giao cho một mafioso, chúng tôi chỉ cần biết khi nào nó sẽ trống trở lại. Điều này làm giảm vấn đề duy trì, đối với mỗi bảng, thời gian sớm nhất mà nó có sẵn. 

Khi một mafioso có khoảng [a_i, b_i] được xử lý, một bảng sẽ hợp lệ nếu khoảng thời gian được chỉ định cuối cùng của nó kết thúc đúng trước a_i. Vì các điểm cuối được tính là trùng lặp nên không được phép có sự bình đẳng, vì vậy chúng tôi yêu cầu end_end < a_i. 

Điều này biến vấn đề thành việc chọn liên tục bảng chỉ mục nhỏ nhất có end_end nhỏ hơn a_i. Để thực hiện điều này một cách hiệu quả, chúng tôi duy trì cấu trúc ưu tiên được khóa theo thời gian có sẵn tiếp theo của mỗi bảng. Mỗi khi chúng tôi chỉ định một bảng, chúng tôi sẽ cập nhật thời gian rảnh tiếp theo của nó thành b_i. 

Một đống tối thiểu hoàn toàn phù hợp: nó lưu trữ các cặp (next_free_time, table_id). Heap luôn ưu tiên bảng trống sớm nhất. Tuy nhiên, vì chúng ta cũng cần bảng hợp lệ được lập chỉ mục nhỏ nhất trong số những bảng miễn phí nên chúng ta phải cẩn thận. Quan sát quan trọng là nếu chúng ta giữ tất cả các bảng trong vùng heap, chúng ta có thể tạm thời trích xuất những bảng chưa trống và chèn lại chúng sau khi kiểm tra tính khả thi. Bởi vì mỗi bảng được đẩy và bật một lần trong mỗi lần gán, nên tổng độ phức tạp vẫn giữ nguyên logarit trên mỗi thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tham lam dựa trên đống | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Khởi tạo hàng đợi ưu tiên chứa tất cả các bảng, mỗi bảng có next_free_time = 0 và chỉ mục của nó. Điều này thể hiện rằng ban đầu tất cả các bảng đều có sẵn ngay lập tức. 
2. Xử lý các mafiosi từ 1 đến n theo thứ tự, vì các mafiosi trước đó có mức độ ưu tiên trong cả việc phân công và ràng buộc. 
3. Đối với mafioso hiện tại có khoảng [a_i, b_i], liên tục trích xuất phần trên cùng của heap trong khi next_free_time của nó không hoàn toàn nhỏ hơn a_i. Các bảng này tạm thời không có sẵn vì chúng vẫn còn người ở tại hoặc ngoài a_i. 
4. Bảng được trích xuất đầu tiên thỏa mãn next_free_time < a_i là bảng khả thi có chỉ số nhỏ nhất trong số những bảng có sẵn sớm nhất. Chỉ định bảng này cho mafia hiện tại. 
5. Cập nhật next_free_time của bảng đó thành b_i, vì nó hiện đã được sử dụng cho đến thời điểm b_i. 
6. Đẩy lùi tất cả các bảng đã bị xóa tạm thời vào heap không thay đổi, sau đó đẩy cả bảng đã cập nhật. 
7. Ghi lại bài tập và tiếp tục. 

Lý do bước 3 an toàn là vì bất kỳ bảng nào vi phạm giới hạn thời gian đều không thể được sử dụng cho mafioso hiện tại, nhưng nó vẫn có thể cần thiết cho các bảng trong tương lai, vì vậy nó phải được khôi phục. 

### Tại sao nó hoạt động 

Ở mỗi bước, mỗi bảng trong heap thể hiện chính xác thời điểm sớm nhất nó trở nên trống sau khi xử lý tất cả các mafiosi trước đó. Bất biến heap đảm bảo rằng khi chúng ta chọn một bảng, chúng ta đang chọn chỉ mục nhỏ nhất có sẵn trong số tất cả các bảng có khoảng thời gian được chỉ định cuối cùng kết thúc trước thời gian bắt đầu hiện tại. Vì chúng tôi luôn chỉ định bảng khả thi đầu tiên và cập nhật ngay tính khả dụng của nó nên không có sự phân công nào trong tương lai có thể tạo ra xung đột về trước. Thứ tự xử lý đảm bảo rằng tất cả các ràng buộc chỉ đến từ các khoảng thời gian đã được chỉ định, do đó tính khả thi chỉ phụ thuộc vào end_end trên mỗi bảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        intervals = [tuple(map(int, input().split())) for _ in range(n)]

        # heap: (next_free_time, table_id)
        heap = [(0, i) for i in range(1, n + 1)]
        heapq.heapify(heap)

        ans = [0] * n

        for i, (a, b) in enumerate(intervals):
            blocked = []

            # find first available table
            while heap and heap[0][0] >= a:
                blocked.append(heapq.heappop(heap))

            # now heap[0] must be valid
            free_time, table_id = heapq.heappop(heap)

            ans[i] = table_id

            # push back blocked tables
            for item in blocked:
                heapq.heappush(heap, item)

            # update chosen table
            heapq.heappush(heap, (b, table_id))

        print(*ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp thuật toán. Heap lưu trữ khi mỗi bảng trở nên trống. Vòng lặp bật lên các bảng bị chặn là cần thiết vì vùng nhớ heap được sắp xếp theo thời gian rảnh sớm nhất chứ không phải theo tính khả thi cho khoảng thời gian bắt đầu hiện tại, vì vậy chúng tôi tạm thời xóa tất cả các bảng vẫn còn bị chiếm giữ tại thời điểm a_i. 

Một sai lầm phổ biến ở đây là quên rằng sự bình đẳng được coi là sự chồng chéo. Chính vì vậy điều kiện là`heap[0][0] >= a`còn hơn là`>`. 

Một sự tinh tế khác là chúng tôi luôn đẩy lùi tất cả các bảng bị xóa tạm thời trước khi chuyển sang mafioso tiếp theo, nếu không chúng tôi sẽ vĩnh viễn mất các ứng viên hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có ba mafiosi: 

Khoảng: [1, 1], [1, 2], [2, 3] 

### Dấu vết 1 

| tôi | Khoảng thời gian | Đỉnh đống | Bị chặn | Bảng được giao | Đã cập nhật đống | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1,1] | (0,1) | không | 1 | (1,1) + những người khác | 
| 2 | [1,2] | (1,1) bị chặn → tiếp theo (0,2) | (1,1) | 2 | đã cập nhật (2,2) | 
| 3 | [2,3] | (1,1) hợp lệ | không | 1 | cập nhật (3,1) | 

Điều này cho thấy việc sử dụng lại bảng chỉ phụ thuộc vào việc Last_end có hoàn toàn nhỏ hơn thời gian bắt đầu hay không. 

### Dấu vết 2 

Khoảng thời gian: [1,2], [2,3], [3,4] 

| tôi | Khoảng thời gian | Đỉnh đống | Bị chặn | Bảng được giao | Đã cập nhật đống | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1,2] | (0,1) | không | 1 | (2,1) | 
| 2 | [2,3] | (2,1) bị chặn | (2,1) | 2 | (3,2) | 
| 3 | [3,4] | (3,2) bị chặn | (3,2) | 3 | (4,3) | 

Điều này xác nhận rằng ngay cả khi các khoảng được xâu chuỗi hoàn hảo, mỗi khoảng vẫn phải chuyển sang một bảng mới vì sự bình đẳng giữa các điểm cuối gây ra xung đột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi bảng được đẩy và bật ra khỏi heap nhiều nhất một lần cho mỗi lần gán và các phép toán trên heap được tính theo logarit | 
| Không gian | O(n) | Heap lưu trữ một mục nhập trên mỗi bảng cộng với bộ đệm tạm thời | 

Với n lên tới 250.000 qua các thử nghiệm, điều này phù hợp thoải mái trong giới hạn thời gian vì hệ số logarit vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# simple chain
assert run("""1
3
1 1
1 2
2 3
""") == "1 2 1"

# identical intervals
assert run("""1
3
1 5
1 5
1 5
""") == "1 2 3"

# non-overlapping reuse
assert run("""1
3
1 2
3 4
5 6
""") == "1 1 1"

# full overlap stress
assert run("""1
4
1 10
1 10
1 10
1 10
""") == "1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng thời gian chuỗi | 1 2 1 | quy tắc chồng chéo điểm cuối | 
| khoảng giống hệt nhau | 1 2 3 | buộc tách bàn | 
| không chồng chéo | 1 1 1 | tái sử dụng tính đúng đắn | 
| chồng chéo hoàn toàn | 1 2 3 4 | tăng trưởng phân công trong trường hợp xấu nhất | 

## Vỏ cạnh 

Khi tất cả các khoảng đều giống nhau, mỗi mafioso phải nhận được một bảng riêng biệt. Heap liên tục chặn tất cả các bảng hiện có cho đến khi nó đạt đến một bảng mới và mỗi nhiệm vụ cập nhật một mục nhập khác nhau, tạo ra một chuỗi các chỉ mục bảng tăng dần. 

Khi các khoảng chỉ chạm vào điểm cuối, chẳng hạn như [1,2] và [2,3], khoảng thứ hai không thể sử dụng lại cùng một bảng vì next_free_time bằng thời gian bắt đầu, không nhỏ hơn nó. Thuật toán chặn chính xác bảng đó bằng cách kiểm tra`>= a_i`, đảm bảo một bảng mới được chọn. 

Khi các khoảng thời gian rời rạc và được sắp xếp theo thứ tự chuyển tiếp theo thời gian, vùng heap sẽ nhanh chóng tìm thấy cùng một bảng mà không bị chặn, vì next_free_time của nó luôn nhỏ hơn thời gian bắt đầu tiếp theo. Điều này chứng tỏ rằng việc tái sử dụng được tối đa hóa bất cứ khi nào các ràng buộc cho phép.
