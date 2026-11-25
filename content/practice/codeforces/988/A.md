---
problem: 988A
contest_id: 988
problem_index: A
name: "Diverse Team"
contest_name: "Codeforces Round 486 (Div. 3)"
rating: 800
tags: ["brute force", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 64
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33a9d8-c8d4-83ec-b44a-533030529b6c
---

#CF 988A - Đội ngũ đa dạng 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a9d8-c8d4-83ec-b44a-533030529b6c 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách sinh viên, mỗi sinh viên có một giá trị xếp hạng. Nhiệm vụ là chọn một tập hợp con của chính xác`k`sao cho không có hai học sinh được chọn nào có cùng đánh giá. Kết quả đầu ra không phải là xếp hạng mà là chỉ số của các sinh viên được chọn trong danh sách ban đầu. 

Vì vậy, vấn đề giảm xuống còn việc quét qua một mảng trong khi chọn các vị trí có giá trị chưa được nhìn thấy trước đó, cho đến khi chúng tôi thu thập được`k`chỉ số hợp lệ hoặc chúng tôi hết các giá trị khác biệt. 

Những hạn chế rất nhỏ:`n ≤ 100`Và`a_i ≤ 100`. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nâng cao hoặc tối ưu hóa. Ngay cả việc quét tuyến tính bậc hai hoặc đơn giản cũng đủ. Một đường chuyền duy nhất với một bộ là quá đủ. 

Tình huống duy nhất mà câu trả lời không tồn tại là khi mảng chứa ít hơn`k`những giá trị riêng biệt. Trong trường hợp đó, dù chọn học sinh thế nào cũng không thể đạt được quy mô`k`trong khi vẫn giữ xếp hạng khác biệt. 

Trường hợp cạnh tinh tế xuất hiện khi các bản sao chiếm ưu thế trong mảng. Ví dụ: nếu tất cả các giá trị giống hệt nhau và`k > 1`, chúng ta phải bác bỏ ngay. Một trường hợp khác là khi tồn tại các bản sao nhưng đủ các giá trị khác biệt nằm rải rác sau đó trong mảng; quá trình quét tham lam phải đảm bảo chúng tôi vẫn chọn tất cả các giá trị riêng biệt và không bị mắc kẹt sớm bởi bất kỳ giả định đặt hàng nào. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là thử tất cả các tập hợp con có kích thước`k`và kiểm tra xem tất cả các giá trị được chọn có khác biệt hay không. Điều này sẽ liên quan đến việc kiểm tra sự kết hợp của`n`chọn`k`và đối với mỗi tập hợp con xác minh tính duy nhất, dẫn đến sự bùng nổ ngay cả đối với các tập hợp nhỏ`n`. Trong trường hợp xấu nhất, số lượng tập hợp con theo thứ tự là 2ⁿ và thậm chí việc giới hạn ở kích thước cố định k vẫn mang lại hành vi O(nᵏ), điều này không cần thiết cho vấn đề này. 

Cấu trúc của bài toán gợi ý một quan sát đơn giản hơn: chúng ta không cần tìm kiếm trên toàn cục. Chúng tôi chỉ cần bất kỳ`k`những giá trị riêng biệt. Vì chúng tôi được phép xuất bất kỳ tập hợp con hợp lệ nào, chúng tôi có thể quét từ trái sang phải một cách tham lam và chọn lần xuất hiện đầu tiên của mỗi xếp hạng mới. Điều này hiệu quả vì thứ tự lựa chọn không quan trọng và mọi chỉ mục đều có giá trị như nhau miễn là giá trị của nó chưa được sử dụng trước đó. 

Brute-force thất bại vì nó khám phá những kết hợp không liên quan, trong khi giải pháp tham lam lại hiệu quả vì tính duy nhất là thuộc tính cục bộ có thể được thực thi tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ · n) | O(k) | Quá chậm | 
| Quét tham lam với Set | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời dần dần trong khi theo dõi xếp hạng nào chúng tôi đã sử dụng. 

1. Khởi tạo một tập hợp trống`used`để lưu trữ các xếp hạng mà chúng tôi đã chọn và một danh sách trống`ans`để lưu trữ các chỉ số đã chọn. Tập hợp này cần thiết để kiểm tra tính duy nhất trong thời gian không đổi. 
2. Lặp lại sinh viên từ chỉ mục`1`ĐẾN`n`. Đối với mỗi học sinh, chúng tôi kiểm tra xem xếp hạng của họ đã đạt chưa`used`. Việc kiểm tra này đảm bảo chúng tôi chỉ chọn các giá trị riêng biệt. 
3. Nếu xếp hạng chưa được sử dụng, chúng tôi sẽ thêm chỉ mục của sinh viên này vào`ans`và đánh dấu xếp hạng là đã sử dụng. Chúng tôi đang cam kết một cách hiệu quả với sinh viên này với tư cách là người đại diện cho đánh giá của họ. 
4. Dừng lại sớm nếu`ans`đạt kích thước`k`. Tại thời điểm đó, chúng tôi đã có một nhóm hợp lệ và việc tiếp tục sẽ không thay đổi tính đúng đắn. 
5. Sau vòng lặp, kiểm tra xem`ans`có kích thước`k`. Nếu không, điều đó có nghĩa là chúng tôi gặp ít hơn`k`xếp hạng riêng biệt trong toàn bộ mảng, do đó việc thành lập một nhóm hợp lệ là không thể. 

### Tại sao nó hoạt động 

Bất biến quan trọng là`used`luôn chứa chính xác tập hợp các xếp hạng được biểu diễn trong`ans`. Mỗi khi chúng tôi thêm một học sinh, chúng tôi sẽ đưa ra một xếp hạng chưa từng thấy trước đây, đảm bảo rằng tất cả các chỉ số đã chọn đều tương ứng với các giá trị riêng biệt. Vì chúng tôi xử lý mọi phần tử, nếu ít hơn`k`tồn tại các xếp hạng khác nhau, thuật toán sẽ làm cạn kiệt mảng mà không đạt được kích thước`k`, báo hiệu chính xác sự không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

used = set()
ans = []

for i in range(n):
    if a[i] not in used:
        used.add(a[i])
        ans.append(i + 1)
        if len(ans) == k:
            break

if len(ans) < k:
    print("NO")
else:
    print("YES")
    print(*ans)
```Giải pháp sử dụng một bộ để theo dõi xếp hạng đã xem và một danh sách để lưu trữ các chỉ số đã chọn. Vòng lặp dừng sớm khi chúng tôi thu thập đủ các phần tử riêng biệt, điều này tránh được những công việc không cần thiết nhưng không cần thiết để đảm bảo tính chính xác do các ràng buộc nhỏ. 

Việc lập chỉ mục được xử lý cẩn thận bằng cách xuất ra`i + 1`vì vấn đề sử dụng lập chỉ mục dựa trên 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
15 13 15 15 12
```| tôi | một [tôi] | được sử dụng trước đây | hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 15 | {} | lấy | [1] | 
| 2 | 13 | {15} | lấy | [1,2] | 
| 3 | 15 | {15,13} | bỏ qua | [1,2] | 
| 4 | 15 | {15,13} | bỏ qua | [1,2] | 
| 5 | 12 | {15,13} | lấy | [1,2,5] | 

Thuật toán thu thập chính xác 3 giá trị riêng biệt, xác nhận rằng quá trình quét tham lam đã nắm bắt thành công lần xuất hiện đầu tiên của mỗi xếp hạng. 

### Ví dụ 2 

đầu vào:```
4 3
1 1 1 2
```| tôi | một [tôi] | được sử dụng trước đây | hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {} | lấy | [1] | 
| 2 | 1 | {1} | bỏ qua | [1] | 
| 3 | 1 | {1} | bỏ qua | [1] | 
| 4 | 2 | {1} | lấy | [1,4] | 

Sau khi quét tất cả các phần tử ta chỉ có 2 giá trị phân biệt nên kết quả không thể`k = 3`. 

Điều này chứng tỏ rằng thuật toán phân biệt chính xác giữa “chưa tìm thấy đủ” và “tổng thể là không thể”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi học sinh được xử lý một lần, với trung bình các phép toán được đặt O(1) | 
| Không gian | O(n) | Lưu trữ tập hợp các xếp hạng riêng biệt và các chỉ số được chọn | 

Được cho`n ≤ 100`, điều này diễn ra trong thời gian không đổi trong thực tế. Ngay cả với những hạn chế lớn hơn nhiều, cách tiếp cận này vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys as _sys
    output = []
    
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    used = set()
    ans = []
    
    for i in range(n):
        if a[i] not in used:
            used.add(a[i])
            ans.append(i + 1)
            if len(ans) == k:
                break
    
    if len(ans) < k:
        return "NO"
    return "YES\n" + " ".join(map(str, ans))

# provided sample
assert run("5 3\n15 13 15 15 12") == "YES\n1 2 5"

# all equal values
assert run("4 2\n7 7 7 7") == "NO"

# exact k distinct
assert run("3 3\n1 2 3") == "YES\n1 2 3"

# more than k distinct
assert run("5 2\n5 4 3 2 1") == "YES\n1 2"

# duplicates scattered
assert run("6 3\n1 2 1 3 2 4") == "YES\n1 2 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | KHÔNG | phát hiện không thể | 
| chính xác k khác biệt | CÓ tất cả các chỉ số | lựa chọn hợp lệ tối thiểu | 
| giảm dần độc đáo | k chỉ số đầu tiên | dừng sớm đúng đắn | 
| trùng lặp rải rác | lựa chọn tham lam hợp lệ | sự ổn định khi lặp lại | 

## Vỏ cạnh 

Khi tất cả học sinh có cùng xếp hạng, thuật toán chỉ thêm một chỉ mục trước khi sử dụng hết mảng. Đối với đầu vào`n = 4, k = 2, a = [7,7,7,7]`,`used`trở thành`{7}`Và`ans = [1]`. Sau khi vòng lặp kết thúc, việc kiểm tra kích thước không thành công và thuật toán xuất ra chính xác`"NO"`. 

Khi mảng chứa chính xác`k`các giá trị riêng biệt, chẳng hạn như`1 2 3`với`k = 3`, mỗi lần lặp sẽ thêm một chỉ mục mới. Thuật toán đạt kích thước`k`chính xác ở bước cuối cùng và dừng sớm, xác nhận việc chấm dứt chính xác. 

Khi các bản sao được trộn lẫn với các giá trị riêng biệt, như`1 2 1 3 2 4`, bộ này đảm bảo rằng chỉ những lần xuất hiện đầu tiên mới đóng góp. Mặc dù các bản sao xuất hiện sớm hơn nhưng chúng không cản trở việc thu thập đủ các giá trị khác biệt và kết quả cuối cùng vẫn hợp lệ.
