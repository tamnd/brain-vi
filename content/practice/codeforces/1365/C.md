---
problem: 1365C
contest_id: 1365
problem_index: C
name: "Rotation Matching"
contest_name: "Codeforces Round 648 (Div. 2)"
rating: 1400
tags: ["constructive algorithms", "data structures", "greedy", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 128
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e4979-4b48-83ec-8352-cdd9514f063c
---

#CF 1365C - Khớp xoay 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam, triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e4979-4b48-83ec-8352-cdd9514f063c 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho hai hoán vị của cùng một bộ số từ 1 đến n. Hãy coi chúng như hai cách sắp xếp hình tròn khác nhau của cùng một ô có nhãn. Chúng ta được phép xoay từng vòng tròn một cách độc lập bao nhiêu lần và sau khi làm như vậy, chúng ta căn chỉnh chúng ở một vị trí cố định. Một vị trí sẽ góp phần đưa ra câu trả lời nếu cùng một giá trị xuất hiện trong cả hai mảng tại vị trí đó. 

Vì vậy, câu hỏi thực sự là: sau khi chọn hai phép dịch chuyển tuần hoàn, chúng ta có thể tạo bao nhiêu chỉ số để thỏa mãn a[i] == b[i]? 

Vì cả hai mảng đều chứa các giá trị giống hệt nhau nên mọi giá trị x đều có một vị trí cố định trong a và một vị trí cố định trong b. Việc xoay một mảng chỉ đơn giản là dịch chuyển tất cả các vị trí này một cách đồng nhất, do đó thứ tự tương đối bên trong mỗi hoán vị không thay đổi, chỉ có phần bù chung thay đổi. 

Ràng buộc n lên tới 2 × 10^5 buộc mọi phép so sánh bậc hai trên tất cả các phép dịch đều không thể thực hiện được. Việc thử tất cả n ca cho một mảng so với n ca cho mảng kia đã cho kết quả O(n^2), quá chậm. Ngay cả các cách tiếp cận O(n log n) hoặc O(n sqrt n) cũng được chấp nhận, nhưng chúng ta nên mong đợi một phép biến đổi tuyến tính hoặc gần tuyến tính dựa trên các vị trí. 

Một trường hợp cạnh tinh tế xuất hiện khi các hoán vị giống hệt nhau cho đến khi xoay, trong trường hợp đó chúng ta có thể căn chỉnh tất cả các vị trí và nhận được n kết quả khớp. Ở một thái cực khác, có những trường hợp mỗi căn chỉnh chỉ mang lại một kết quả khớp, ví dụ: 

a = [1,2,3,4], b = [2,3,1,4]. Không có vòng quay nào có thể căn chỉnh đồng thời nhiều hơn hai vị trí, vì cấu trúc tuần hoàn xung đột. 

Một chi tiết ẩn khác là việc xoay cả hai mảng tương đương với việc cố định một mảng và xoay mảng kia theo hướng ngược lại. Sự đối xứng này rất cần thiết vì nó làm giảm bài toán từ hai bậc tự do xuống còn một. 

## Phương pháp tiếp cận 

Một ý tưởng vũ phu rất đơn giản. Chúng tôi sửa một phép quay của a, sau đó thử mọi phép quay của b, tính xem có bao nhiêu chỉ số khớp và lấy giá trị tối đa. Mỗi phép so sánh tốn O(n) và có các cặp xoay O(n^2), dẫn đến thời gian O(n^3) nếu thực hiện một cách ngây thơ hoặc O(n^2) nếu chúng ta tính toán trước các phép so sánh một cách cẩn thận. Dù thế nào đi nữa, nó quá lớn đối với n = 2 × 10^5. 

Quan sát quan trọng là giá trị chứ không phải vị trí sẽ điều khiển sự căn chỉnh. Mỗi giá trị x xuất hiện ở một vị trí nào đó pos_a[x] trong a và pos_b[x] trong b. Nếu chúng ta xoay b sao cho vị trí của x của b trùng với vị trí của x của a thì x đóng góp một kết quả khớp. Đối với một vòng quay cố định, số lượng kết quả khớp chính xác là số lượng giá trị mà sự dịch chuyển yêu cầu của nó phù hợp. 

Vì vậy, mọi giá trị đều tạo ra một “độ lệch xoay bắt buộc” giữa a và b. Nếu chúng ta tính toán độ lệch này cho mỗi x thì căn chỉnh tốt nhất đơn giản là độ lệch thường xuyên nhất. Điều này làm giảm vấn đề đếm tần số trên n hiệu được tính toán. 

Điều này chuyển vấn đề từ việc thử tất cả các sắp xếp sang nhóm các giá trị theo mức độ xoay mà chúng yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) hoặc O(n^2) | O(1)-O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính vị trí của từng giá trị trong hoán vị thứ nhất. Với mọi giá trị x, lưu trữ ở nơi nó xuất hiện trong a. 
2. Tính vị trí từng giá trị ở hoán vị thứ hai tương tự. Điều này giúp chúng tôi tra cứu nhanh chóng vị trí mà mỗi giá trị cần đến. 
3. Với mỗi giá trị x, hãy tính độ dịch chuyển cần thiết để căn chỉnh b[x] với a[x]. Nếu pos_a[x] là i và pos_b[x] là j, thì việc dịch b theo (i - j) modulo n sẽ căn chỉnh x một cách chính xác. Điều này chuyển vấn đề thành việc xác định giá trị dịch chuyển nào là phổ biến nhất. 
4. Đếm xem mỗi ca có bao nhiêu giá trị. Chúng tôi duy trì một mảng tần số hoặc sơ đồ băm được lập chỉ mục theo giá trị dịch chuyển modulo n. 
5. Câu trả lời là tần suất tối đa trong số tất cả các ca.

Lý do điều này có tác dụng là vì mỗi ca xác định sự liên kết chung giữa các mảng. Một giá trị chỉ đóng góp vào một sự dịch chuyển nếu sự dịch chuyển đó ánh xạ trực tiếp vị trí của nó trong b lên vị trí của nó trong a. Do đó, bất kỳ phép xoay hợp lệ nào đều tương ứng với một trong các nhóm thay đổi này và tất cả các kết quả khớp trong phép xoay đó chính xác là các giá trị rơi vào nhóm đó. 

Bất biến là đối với bất kỳ phép quay nào được chọn, các giá trị khớp chính xác là những giá trị có độ dịch chuyển tương đối giữa a và b bằng phép quay đó. Vì mỗi giá trị đóng góp độc lập vào chính xác một chuyển vị nên việc đếm tần số trên tất cả các giá trị sẽ nắm bắt được tất cả các sắp xếp có thể một cách toàn diện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

pos_a = [0] * (n + 1)
pos_b = [0] * (n + 1)

for i, x in enumerate(a):
    pos_a[x] = i

for i, x in enumerate(b):
    pos_b[x] = i

freq = [0] * n

for x in range(1, n + 1):
    shift = (pos_a[x] - pos_b[x]) % n
    freq[shift] += 1

print(max(freq))
```Phần đầu tiên xây dựng ánh xạ ngược từ giá trị đến chỉ mục, giúp tránh việc quét lặp lại các mảng. Vòng lõi tính toán vòng quay cần thiết cho mỗi giá trị. Hoạt động modulo đảm bảo rằng các dịch chuyển âm được gói chính xác vào phạm vi [0, n-1]. 

Mảng tần số có kích thước n vì có chính xác n phép quay riêng biệt có thể. Mức tối đa cuối cùng tương ứng trực tiếp với sự liên kết tốt nhất. 

Một lỗi phổ biến là đảo ngược hướng chuyển số; cả hai quy ước đều có tác dụng miễn là duy trì được tính nhất quán. Một điều nữa là quên chuẩn hóa modulo, điều này sẽ chia các phép quay tương đương thành các nhóm âm và dương. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

a = [1,2,3,4,5], b = [2,3,4,5,1] 

| x | pos_a[x] | pos_b[x] | ca | 
| --- | --- | --- | --- | 
| 1 | 0 | 4 | 1 | 
| 2 | 1 | 0 | 1 | 
| 3 | 2 | 1 | 1 | 
| 4 | 3 | 2 | 1 | 
| 5 | 4 | 3 | 1 | 

Tất cả các giá trị đều tạo ra độ dịch chuyển 1, vì vậy tần số[1] = 5 và câu trả lời là 5. 

Điều này xác nhận trường hợp một phép quay căn chỉnh hoàn hảo cả hai hoán vị. 

### Ví dụ 2 

a = [1,3,2,4], b = [2,3,1,4] 

| x | pos_a[x] | pos_b[x] | ca | 
| --- | --- | --- | --- | 
| 1 | 0 | 2 | 2 | 
| 2 | 2 | 0 | 2 | 
| 3 | 1 | 1 | 0 | 
| 4 | 3 | 3 | 0 | 

Chúng ta nhận được hai ca: 0 và 2, mỗi ca có tần số 2. Câu trả lời là 2. 

Điều này cho thấy có thể tồn tại nhiều phép quay tối ưu và thuật toán xác định chính xác phép quay tốt nhất trong số đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giá trị được xử lý một lần để tính toán độ dịch chuyển của nó | 
| Không gian | O(n) | Mảng lưu trữ vị trí và tần suất ca làm việc | 

Độ phức tạp tuyến tính vừa vặn thoải mái trong các giới hạn lên tới 2 × 10^5 và mức sử dụng bộ nhớ là tối thiểu vì chúng tôi chỉ lưu trữ các mảng phụ trợ có hệ số không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    # solution
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    pos_a = [0] * (n + 1)
    pos_b = [0] * (n + 1)

    for i, x in enumerate(a):
        pos_a[x] = i

    for i, x in enumerate(b):
        pos_b[x] = i

    freq = [0] * n
    for x in range(1, n + 1):
        shift = (pos_a[x] - pos_b[x]) % n
        freq[shift] += 1

    print(max(freq))
    return out.getvalue().strip()

# provided samples
assert run("5\n1 2 3 4 5\n2 3 4 5 1\n") == "5"

# minimum size
assert run("1\n1\n1\n") == "1"

# already aligned
assert run("3\n1 2 3\n1 2 3\n") == "3"

# reverse-like alignment
assert run("4\n1 3 2 4\n2 3 1 4\n") == "2"

# random small case
assert run("5\n2 3 4 5 1\n1 2 3 4 5\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 giống nhau | 1 | độ đúng ranh giới tối thiểu | 
| đã bằng nhau | n | trường hợp căn chỉnh đầy đủ | 
| hoán vị hỗn hợp | 2 | nhiều ca tối ưu | 
| ghép nối ngược xoay | 5 | tính đúng đắn của sự dịch chuyển theo chu kỳ | 

## Vỏ cạnh 

Khi n = 1, cả hai hoán vị chỉ chứa một phần tử. Sự thay đổi duy nhất là 0 và mảng tần số thu gọn lại thành một nhóm duy nhất, vì vậy câu trả lời gần như là 1. 

Khi các mảng giống hệt nhau, mọi giá trị đều có độ dịch chuyển 0. Tần số của độ dịch chuyển 0 trở thành n và thuật toán trả về sự căn chỉnh đầy đủ mà không cần bất kỳ cách viết hoa đặc biệt nào. 

Khi các hoán vị là các phép quay hoàn hảo của nhau, mọi giá trị đều tạo ra cùng một giá trị dịch chuyển. Bảng tần số tập trung hoàn toàn vào một nhóm, cho thấy phương pháp này nắm bắt chính xác sự tương đương theo chu kỳ toàn cầu mà không mô phỏng rõ ràng các phép quay. 

Khi không có một phép xoay nào căn chỉnh hầu hết các phần tử ngoại trừ một tập hợp con nhỏ, bảng tần số sẽ trải rộng các giá trị một cách tự nhiên qua các ca khác nhau và mức tối đa vẫn tương ứng với cách căn chỉnh tốt nhất có thể đạt được.
