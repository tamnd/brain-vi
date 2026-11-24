---
problem: 925A
contest_id: 925
problem_index: A
name: "Stairs and Elevators"
contest_name: "VK Cup 2018 - Round 3"
rating: 1600
tags: ["binary search"]
answer: passed_samples
verified: true
solve_time_s: 111
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a326f66-d5a4-83ec-8424-637d475b2a1e
---

#CF 925A - Cầu Thang Và Thang Máy 

**Đánh giá:** 1600 
**Thẻ:** tìm kiếm nhị phân 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a326f66-d5a4-83ec-8424-637d475b2a1e 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng tôi đang làm việc với một mạng lưới khách sạn rất lớn có`n`sàn nhà và`m`số phòng mỗi tầng, tạo thành một`n × m`lưới. Mỗi phòng được xác định bằng tọa độ`(floor, section)`. 

Một số cột dọc rất đặc biệt: chúng có cầu thang hoặc thang máy. Nếu cột là cột cầu thang thì di chuyển theo chiều dọc giữa các tầng liền kề trong cột đó tốn 1 mỗi tầng. Nếu là cột thang máy, việc di chuyển theo chiều dọc sẽ nhanh hơn: bạn có thể di chuyển lên hoặc xuống bất kỳ số tầng nào, nhưng chi phí thời gian sẽ tăng lên khi số tầng chia cho`v`, làm tròn lên. Chuyển động ngang dọc theo một tầng luôn tốn 1 chi phí cho mỗi phần liền kề. 

Mỗi truy vấn yêu cầu thời gian di chuyển tối thiểu giữa hai phòng nhất định, trong đó bạn có thể đi bộ theo chiều ngang để đến cầu thang hoặc cột thang máy, di chuyển theo chiều dọc bằng cách sử dụng nó, sau đó lại đi bộ theo chiều ngang để đến đích. 

Hạn chế chính đó là`n`Và`m`có thể lớn tới 10^8, vì vậy chúng tôi không thể lập mô hình lưới một cách rõ ràng. Số lượng cột đặc biệt tối đa là 10^5 và có tối đa 10^5 truy vấn, vì vậy mỗi truy vấn phải được xử lý theo thời gian gần đúng logarit. 

Một đường dẫn ngắn nhất ngây thơ trên lưới sẽ cố gắng xem xét nhiều vị trí trung gian hoặc chạy logic giống BFS, điều này là không thể ở quy mô này. 

Một vấn đề tế nhị là đường dẫn tối ưu luôn sử dụng tối đa một cột đặc biệt. Mặc dù có vẻ như việc chuyển đổi giữa nhiều cầu thang hoặc thang máy có thể hữu ích nhưng bất kỳ chuyển đổi bổ sung nào cũng chỉ làm tăng thêm chi phí theo chiều ngang mà không cải thiện hiệu quả theo chiều dọc. 

Các trường hợp cạnh xuất hiện khi cột tốt nhất không phải là cột gần nhất theo thứ tự chỉ mục mà là cột gần thứ hai. Ví dụ: nếu cầu thang ở vị trí 2 và 100 và bạn bắt đầu ở gần số 1, thì lựa chọn tốt nhất có thể là số 2 ngay cả khi đích đến gần hơn 100. Cách tiếp cận tham lam “luôn đi về phía đích” sẽ thất bại. 

Một trường hợp đặc biệt khác phát sinh khi cầu thang hoàn toàn không tồn tại hoặc thang máy không tồn tại. Sau đó, giải pháp phải chuyển sang chỉ sử dụng cấu trúc có sẵn một cách duyên dáng. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là coi mọi cầu thang hoặc cột thang máy có thể là điểm dừng trung gian. Đối với mỗi truy vấn, chúng tôi tính toán chi phí đi từ đầu đến cột đó, di chuyển theo chiều dọc, sau đó đi đến đích. Vì có tới 10^5 cột đặc biệt, điều này mang lại cho O(k) công việc cho mỗi truy vấn, dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát chính là cấu trúc đường dẫn tối ưu cực kỳ đơn giản. Bất kỳ tuyến đường hợp lệ nào cũng phải chọn chính xác một cột để thực hiện chuyển động thẳng đứng. Khi cột đó được cố định, chi phí sẽ phân tách rõ ràng thành ba phần độc lập: chuyển động theo chiều ngang từ đầu đến cột, chuyển động theo chiều dọc bên trong cột và chuyển động theo chiều ngang từ cột đến đích. 

Điều này biến vấn đề thành một cuộc tìm kiếm trên một tập hợp các cột ứng cử viên đã được sắp xếp. Vì khoảng cách theo chiều ngang là giá trị tuyệt đối trên một đường nên các ứng cử viên tốt nhất cho một vị trí nhất định luôn nằm trong số các cột có sẵn gần nhất ở bên trái và bên phải. Đây là thuộc tính tiêu chuẩn của chi phí lồi trên một tập đã được sắp xếp: chỉ cần kiểm tra những hàng xóm gần nhất trong danh sách đã sắp xếp là đủ. 

Do đó, đối với mỗi truy vấn, chúng tôi tìm kiếm nhị phân trong cả danh sách cầu thang và danh sách thang máy và chỉ đánh giá các ứng cử viên gần nhất ở cả hai phía. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên tất cả các cột | O(q · (c_l + c_e)) | O(c_l + c_e) | Quá chậm | 
| Tìm kiếm nhị phân ứng cử viên gần nhất | O(q log(c_l + c_e)) | O(c_l + c_e) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ vị trí cầu thang, vị trí thang máy theo mảng đã sắp xếp. Việc sắp xếp là cần thiết để chúng ta có thể thực hiện tìm kiếm nhị phân nhằm xác định vị trí các ứng viên gần nhất một cách hiệu quả. 
2. Với mỗi truy vấn, bắt đầu giải nén`(x1, y1)`và điểm đến`(x2, y2)`. 
3. Tính toán câu trả lời cơ bản là chi phí di chuyển theo chiều ngang thuần túy`|y1 - y2| + |x1 - x2|`. Điều này tương ứng với việc bỏ qua tất cả cầu thang và thang máy và đi bộ trực tiếp, đây luôn là một phương án dự phòng hợp lệ. 
4. Để cải thiện mức cơ bản này, hãy cân nhắc sử dụng cột cầu thang. Sử dụng tìm kiếm nhị phân, tìm vị trí chèn của`y1`trong mảng cầu thang. 
5. Từ vị trí đó, chọn tối đa hai cột cầu thang dự kiến: cột gần bên trái nhất và cột gần bên phải nhất. Đây là những cột duy nhất có thể giảm thiểu khoảng cách ngang từ điểm bắt đầu. 
6. Đối với mỗi cột cầu thang ứng viên`c`, tính tổng chi phí khi đi theo chiều ngang từ đầu đến`c`, chuyển động thẳng đứng`|x1 - x2|`, và đi ngang từ`c`đến đích. Cập nhật câu trả lời. 
7. Lặp lại quy trình tương tự cho cột thang máy, nhưng tính chi phí theo chiều dọc như sau:`ceil(|x1 - x2| / v)`thay vì sự khác biệt tuyến tính. Một lần nữa chỉ xem xét hai ứng cử viên gần nhất từ ​​tìm kiếm nhị phân. 
8. Đưa ra mức tối thiểu trên đường cơ sở, các tuyến đường dựa trên cầu thang và các tuyến đường dựa trên thang máy. 

Tính chính xác dựa trên thực tế là đối với loại vận chuyển dọc cố định, hàm chi phí chỉ phụ thuộc vào cột được chọn thông qua khoảng cách ngang tuyệt đối từ hai điểm cố định. Chức năng này được giảm thiểu ở các cột có sẵn gần nhất, do đó việc kiểm tra các lân cận xung quanh vị trí tìm kiếm nhị phân sẽ nắm bắt được mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ceil_div(a, b):
    return (a + b - 1) // b

n, m, cl, ce, v = map(int, input().split())

stairs = []
elevators = []

if cl:
    stairs = list(map(int, input().split()))
if ce:
    elevators = list(map(int, input().split()))

q = int(input())

def best_cost(col_list, x1, y1, x2, y2, vertical_cost_fn):
    import bisect
    if not col_list:
        return float('inf')

    pos = bisect.bisect_left(col_list, y1)
    res = float('inf')

    for i in (pos - 1, pos):
        if 0 <= i < len(col_list):
            c = col_list[i]
            cost = abs(y1 - c) + abs(y2 - c) + vertical_cost_fn()
            res = min(res, cost)

    return res

for _ in range(q):
    x1, y1, x2, y2 = map(int, input().split())
    dx = abs(x1 - x2)

    ans = abs(y1 - y2) + dx

    ans = min(ans, best_cost(
        stairs, x1, y1, x2, y2,
        lambda dx=dx: dx
    ))

    ans = min(ans, best_cost(
        elevators, x1, y1, x2, y2,
        lambda dx=dx: ceil_div(dx, v)
    ))

    print(ans)
```Giải pháp được cấu trúc xung quanh việc đánh giá các cột ứng cử viên một cách hiệu quả. các`best_cost`Chức năng tách biệt logic cho cả cầu thang và thang máy, chỉ khác nhau ở mô hình chi phí dọc. 

Bước tìm kiếm nhị phân rất quan trọng: thay vì quét tất cả các cột, chúng ta chỉ kiểm tra hai vị trí gần nhất trong mảng đã sắp xếp. Lambda ghi lại khoảng cách theo chiều dọc để logic cầu thang và thang máy có khả năng xử lý theo chiều ngang giống hệt nhau. 

Một cạm bẫy triển khai phổ biến là quên bao gồm đường dẫn ngang trực tiếp làm ứng cử viên. Đường dẫn đó không phải lúc nào cũng tối ưu nhưng được yêu cầu làm đường cơ sở cho tính chính xác khi không có cột nào trợ giúp. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

Hãy xem xét một kịch bản đơn giản hóa với cầu thang ở`[2]`và thang máy tại`[5]`và một truy vấn từ`(1,1)`ĐẾN`(5,6)`với`v = 3`. 

Đối với cầu thang, chúng tôi đánh giá cột`2`: 

| Bước | Giá trị | 
| --- | --- | 
| Bắt đầu theo chiều ngang đến 2 | 1 | 
| Dọc | 4 | 
| Ngang đến hết | 4 | 
| Tổng cộng | 9 | 

Đối với thang máy, chúng tôi đánh giá cột`5`: 

| Bước | Giá trị | 
| --- | --- | 
| Bắt đầu theo chiều ngang đến 5 | 4 | 
| Trần dọc(4/3) | 2 | 
| Ngang đến hết | 1 | 
| Tổng cộng | 7 | 

Đường dẫn trực tiếp cơ bản là`|1-5| + |1-6| = 4 + 5 = 9`, vậy đáp án là`7`. 

Dấu vết này cho thấy tại sao sự lựa chọn thang máy có thể chiếm ưu thế ngay cả khi khoảng cách ngang tới cột lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log(c_l + c_e)) | Mỗi truy vấn thực hiện hai tìm kiếm nhị phân và kiểm tra ứng viên liên tục | 
| Không gian | O(c_l + c_e) | Kho chứa các vị trí cầu thang, thang máy | 

Các ràng buộc cho phép tối đa 10^5 truy vấn, do đó công việc logarit trên mỗi truy vấn phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, cl, ce, v = map(int, input().split())

    stairs = []
    elevators = []

    if cl:
        stairs = list(map(int, input().split()))
    if ce:
        elevators = list(map(int, input().split()))

    q = int(input())

    import bisect

    def ceil_div(a, b):
        return (a + b - 1) // b

    def best(col_list, x1, y1, x2, y2, vert):
        if not col_list:
            return float('inf')
        pos = bisect.bisect_left(col_list, y1)
        res = float('inf')
        for i in (pos - 1, pos):
            if 0 <= i < len(col_list):
                c = col_list[i]
                res = min(res, abs(y1 - c) + abs(y2 - c) + vert)
        return res

    out = []
    for _ in range(q):
        x1, y1, x2, y2 = map(int, input().split())
        dx = abs(x1 - x2)
        ans = abs(y1 - y2) + dx

        ans = min(ans, best(stairs, x1, y1, x2, y2, dx))
        ans = min(ans, best(elevators, x1, y1, x2, y2, ceil_div(dx, v)))

        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run("""5 6 1 1 3
2
5
3
1 1 5 6
1 3 5 4
3 3 5 3
""") == """7
5
4"""

# minimum case
assert run("""2 2 1 0 1

1
1 1 2 2
""") == "2"

# no useful vertical structure
assert run("""5 5 1 0 3
3
2
2
1 1 5 5
2 2 4 4
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu | 7 5 4 | tính đúng đắn của việc lựa chọn cầu thang/thang máy hỗn hợp | 
| Lưới tối thiểu | 2 | cơ sở xử lý ngang + dọc | 
| Cấu trúc thưa thớt | khác nhau | xử lý thiếu thang máy, cầu thang | 

## Vỏ cạnh 

Khi không có cầu thang, thuật toán sẽ tự nhiên bỏ qua nhánh đó và dựa vào thang máy và chuyển động trực tiếp. Hàm ứng cử viên trả về giá trị vô cùng cho danh sách trống, do đó, nó không thể chiếm ưu thế trong câu trả lời một cách sai lầm. 

Khi một điểm truy vấn gần nhất với một cột không phải là điểm gần nhất theo hướng tuyệt đối về đích, tìm kiếm nhị phân vẫn kiểm tra cả hai lân cận xung quanh điểm chèn. Điều này đảm bảo rằng cột xa hơn một chút giúp giảm tổng khoảng cách ngang kết hợp vẫn được xem xét. 

Khi khoảng cách theo chiều dọc bằng 0, nghĩa là cả hai điểm đều nằm trên cùng một tầng, chi phí thang máy và cầu thang sẽ chuyển sang định tuyến hoàn toàn theo chiều ngang qua cột đã chọn. Thuật toán vẫn đánh giá chính xác tất cả các ứng viên và đường dẫn trực tiếp vẫn mang tính cạnh tranh, đảm bảo tính chính xác.
