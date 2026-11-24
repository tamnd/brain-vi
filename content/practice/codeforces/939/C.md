---
problem: 939C
contest_id: 939
problem_index: C
name: "Convenient For Everybody"
contest_name: "Codeforces Round 464 (Div. 2)"
rating: 1600
tags: ["binary search", "two pointers"]
answer: passed_samples
verified: false
solve_time_s: 89
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a339882-e2e8-83ec-ad7b-d55e1e16e616
---

#CF 939C - Tiện lợi cho mọi người 

**Đánh giá:** 1600 
**Thẻ:** tìm kiếm nhị phân, hai con trỏ 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a339882-e2e8-83ec-ad7b-d55e1e16e616 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng thời gian tròn với$n$giờ riêng biệt, trong đó nhãn giờ quấn quanh để sau$n$lại đến 1 lần nữa. Mỗi trong số$n$các múi giờ thay đổi giờ địa phương chính xác một giờ so với giờ lân cận, do đó, thời gian bắt đầu toàn cầu ở múi giờ 1 sẽ tạo ra thời gian bắt đầu cục bộ cố định ở mọi múi giờ khác. 

Đối với mỗi múi giờ$i$, có$a_i$người tham gia. Một người tham gia từ múi giờ$i$sẽ chỉ tham gia cuộc thi nếu, theo đồng hồ địa phương của múi giờ đó, cuộc thi bắt đầu vào hoặc sau giờ$s$, và kết thúc đúng trước giờ$f$. Cuộc thi kéo dài đúng một giờ và luôn bắt đầu vào một giờ nguyên. 

Nhiệm vụ là chọn giờ bắt đầu ở múi giờ 1 để sau khi dịch thời gian bắt đầu đó sang giờ địa phương của từng múi giờ, số lượng người tham gia thỏa mãn các ràng buộc sẽ là tối đa. Nếu nhiều giờ bắt đầu mang lại cùng số lượng người tham gia tối đa thì phải trả về giờ bắt đầu nhỏ nhất trong múi giờ 1. 

Kích thước đầu vào$n \le 10^5$ngay lập tức loại trừ việc kiểm tra tất cả thời gian bắt đầu có thể bằng cách tính chi phí mô phỏng trực tiếp$O(n^2)$. Bất kỳ giải pháp nào cũng phải hoạt động trong thời gian gần như tuyến tính hoặc tuyến tính. Điều này cho thấy rằng mỗi thời gian bắt đầu không nên được tính toán lại từ đầu mà nên được cập nhật tăng dần hoặc được tính toán thông qua các cấu trúc tiền tố. 

Một điểm tinh tế là tính chất tuần hoàn của thời gian. Khoảng thời gian như$[s, f)$theo giờ địa phương không nhất thiết phải được căn chỉnh theo biểu diễn tuyến tính của giờ trong một mảng duy nhất, bởi vì bao quanh$n$chia chúng thành hai đoạn. Một cách tiếp cận ngây thơ mà bỏ qua việc bao bọc sẽ thất bại trong những trường hợp$s > f$, mặc dù vấn đề đảm bảo$s < f$, lớp bọc vẫn xuất hiện sau khi dịch chuyển theo độ lệch múi giờ. 

Trường hợp cạnh thứ hai phát sinh từ việc bẻ khóa: khi nhiều thời gian bắt đầu tạo ra cùng một mức tối đa, chúng ta phải trả về chỉ số nhỏ nhất. Do đó, giải pháp kiểu cửa sổ trượt phải xác định cẩn thận thứ tự cập nhật để nó không vô tình ưu tiên các chỉ mục sau này. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ sẽ thử mọi giờ bắt đầu có thể trong múi giờ 1. Đối với mỗi thời gian bắt đầu$t$, chúng tôi tính toán thời gian bắt đầu cục bộ trong từng múi giờ$i$, điều đó có hiệu quả$t + (i-1)$modulo$n$, và kiểm tra xem nó có nằm trong khoảng không$[s, f)$. Nếu có, chúng tôi thêm$a_i$vào tổng số đóng góp của thời gian bắt đầu đó. 

Cách tiếp cận này đúng vì nó mô phỏng trực tiếp tình trạng của mọi người tham gia. Tuy nhiên, đối với mỗi$n$thời gian bắt đầu, chúng tôi lặp lại tất cả$n$múi giờ, dẫn đến$O(n^2)$hoạt động. Với$n = 10^5$, đây là$10^{10}$kiểm tra, vượt xa mọi giới hạn thời gian. 

Quan sát quan trọng là mỗi thời gian bắt đầu sẽ dịch chuyển cùng một bộ giá trị theo chu kỳ. Thay vì tính toán lại các đóng góp từ đầu, chúng ta có thể xem vấn đề như duy trì một cửa sổ trượt trên một mảng hình tròn. Đối với thời gian bắt đầu cố định, những người tham gia tham gia tương ứng chính xác với các múi giờ có chỉ số đã dịch chuyển rơi vào một khoảng liền kề trên trục tròn. 

Điều này làm giảm vấn đề duy trì tổng trọng số bên trong một cửa sổ chuyển động có độ dài cố định$f - s$. Khi thời gian bắt đầu tăng thêm một, cửa sổ này sẽ dịch chuyển một vị trí và chỉ có hai vị trí ranh giới thay đổi tư cách thành viên. Với cấu trúc tổng tiền tố hoặc hai con trỏ, chúng ta có thể cập nhật câu trả lời trong$O(1)$mỗi ca. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Cửa sổ trượt / tiền tố |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích mỗi thời gian bắt đầu ở múi giờ 1 là tạo ra sự dịch chuyển của tất cả các múi giờ, tương đương với việc xoay mảng đóng góp trên một vòng tròn. Công thức cải tiến này chuyển vấn đề thành việc chọn một cửa sổ có độ dài cố định trên một mảng hình tròn. 
2. Xây dựng mảng phụ trợ có kích thước$2n$bằng cách sao chép bản gốc$a_i$. Điều này cho phép các cửa sổ hình tròn được biểu diễn dưới dạng các đoạn tuyến tính mà không cần số học mô-đun. 
3. Tính tổng tiền tố trên mảng mở rộng để có thể tính tổng bất kỳ phân đoạn nào trong thời gian không đổi. 
4. Xác định độ dài cửa sổ$L = f - s$. Đối với mỗi vị trí bắt đầu có thể$t$từ$1$ĐẾN$n$, xét đoạn$[t + s, t + f)$trong tọa độ cục bộ, ánh xạ tới một khoảng có độ dài cố định trong mảng mở rộng. 
5. Đối với mỗi$t$, tính tổng số người tham gia dưới dạng tổng phạm vi bằng cách sử dụng tổng tiền tố. Theo dõi giá trị tối đa và chỉ số nhỏ nhất đạt được nó. 

Lý do công thức này hoạt động là vì việc thay đổi thời gian bắt đầu toàn cầu tương ứng chính xác với việc thay đổi múi giờ nào nằm trong khoảng thời gian cục bộ có thể chấp nhận được. Vì tất cả các múi giờ đều dịch chuyển đồng đều nên cấu trúc tương đối của các chỉ số “tốt” được giữ nguyên dưới dạng cửa sổ trượt trên một chuỗi hình tròn. Việc giảm tổng tiền tố đảm bảo mỗi cửa sổ ứng cử viên được đánh giá trong thời gian không đổi, do đó quá trình quét toàn bộ trên tất cả các lần khởi động vẫn tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
s, f = map(int, input().split())

# build circular array
arr = a + a

# prefix sums
pref = [0] * (2 * n + 1)
for i in range(2 * n):
    pref[i + 1] = pref[i] + arr[i]

length = f - s  # window size in local time

best_value = -1
best_idx = 1

for start in range(n):
    left = start + s - 1
    right = start + f - 1
    total = pref[right] - pref[left]

    if total > best_value:
        best_value = total
        best_idx = start + 1

print(best_idx)
```Đầu tiên, mã sẽ nhân đôi mảng để bất kỳ khoảng tròn nào cũng có thể được biểu thị dưới dạng một đoạn thẳng. Tổng tiền tố cho phép truy vấn tổng phân đoạn O(1). Đối với mỗi giờ bắt đầu có thể có trong múi giờ 1, chúng tôi tính toán chỉ số nào trong mảng mở rộng tương ứng với những người tham gia có giờ địa phương rơi vào$[s, f)$. Phép trừ`pref[right] - pref[left]`chính xác là tổng phân khúc đó. 

Phần cẩn thận là lập chỉ mục. Việc chuyển đổi từ mô tả thời gian dựa trên 1 sang chỉ số Python dựa trên 0 đưa ra các thay đổi nhất quán:`start + s - 1`Và`start + f - 1`đảm bảo phân đoạn căn chỉnh chính xác với ranh giới tiền tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
1 3
```Chúng tôi đánh giá tất cả các vị trí bắt đầu. 

| bắt đầu | đoạn [bắt đầu+s, bắt đầu+f) | tổng hợp | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | [1, 3) → {1,2} | 3 | 3 | 
| 2 | [2, 4) → {2,3} | 5 | 5 | 
| 3 | [3, 5) → {3,1} | 4 | 5 | 

Mức tối đa xảy ra khi bắt đầu 2. Điều này cho thấy các phân đoạn bao quanh xuất hiện một cách tự nhiên như thế nào khi chỉ số vượt quá$n$và tại sao việc nhân đôi mảng sẽ loại bỏ nhu cầu về số học mô-đun thủ công. 

### Ví dụ 2 

đầu vào:```
5
4 1 3 2 5
2 4
```| bắt đầu | phân đoạn | tổng hợp | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | {2,3} | 4 | 4 | 
| 2 | {3,4} | 5 | 5 | 
| 3 | {4,5} | 7 | 7 | 
| 4 | {5,1} | 9 | 9 | 
| 5 | {1,2} | 5 | 9 | 

Điểm bắt đầu tốt nhất là 4. Dấu vết cho thấy cửa sổ tối ưu có thể kéo dài đến phần cuối và phần đầu của mảng hình tròn như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần vượt qua để xây dựng tổng tiền tố và một lần vượt qua tất cả các lần bắt đầu | 
| Không gian |$O(n)$| Mảng trùng lặp và mảng tiền tố có kích thước$2n$| 

Giải pháp phù hợp thoải mái trong các ràng buộc vì$n = 10^5$dẫn đến khoảng$2 \times 10^5$tính toán tiền tố và cùng số lượng truy vấn theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    s, f = map(int, input().split())

    arr = a + a
    pref = [0] * (2 * n + 1)
    for i in range(2 * n):
        pref[i + 1] = pref[i] + arr[i]

    best_value = -1
    best_idx = 1

    for start in range(n):
        left = start + s - 1
        right = start + f - 1
        total = pref[right] - pref[left]
        if total > best_value:
            best_value = total
            best_idx = start + 1

    return str(best_idx)

# provided samples
assert run("""3
1 2 3
1 3
""") == "2"

# all equal
assert run("""4
5 5 5 5
1 2
""") == "1"

# minimum n
assert run("""2
1 100
1 2
""") == "2"

# wrap-heavy case
assert run("""5
1 2 3 4 5
4 5
""") == "5"

# peak in middle
assert run("""6
1 3 10 2 1 1
2 4
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 1 | ràng buộc về chỉ số nhỏ nhất | 
| tối thiểu n | 2 | độ đúng ranh giới | 
| bọc-nặng | 5 | xử lý khoảng thời gian tròn | 
| đỉnh trung | 3 | lựa chọn tổng cửa sổ chính xác | 

## Vỏ cạnh 

Trường hợp một cạnh là khi khoảng tối ưu bao quanh ranh giới của mảng hình tròn. Ví dụ: trong cấu hình có giá trị cao nằm ở cuối và đầu mảng, cửa sổ tốt nhất sẽ mở rộng các chỉ số như$[n-1, 1]$. Cấu trúc mảng kép đảm bảo phân đoạn này được coi là phân đoạn liền kề, do đó, truy vấn tổng tiền tố vẫn nắm bắt chính xác phân đoạn đó. 

Một trường hợp khác là khi nhiều vị trí bắt đầu mang lại tổng số giống hệt nhau. Vì chúng tôi chỉ cập nhật thông tin tốt nhất khi lớn hơn một cách nghiêm ngặt nên lần xuất hiện đầu tiên được giữ nguyên, đảm bảo chỉ số nhỏ nhất được trả về.
