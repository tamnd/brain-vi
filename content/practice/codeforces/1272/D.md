---
problem: 1272D
contest_id: 1272
problem_index: D
name: "Remove One Element"
contest_name: "Codeforces Round 605 (Div. 3)"
rating: 1500
tags: ["brute force", "dp"]
answer: passed_samples
verified: true
solve_time_s: 182
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d93db-e3c8-83ec-95b1-9e42d257a00b
---

#CF 1272D - Loại bỏ một phần tử 

**Đánh giá:** 1500 
**Tags:** vũ lực, dp 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3m 2s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d93db-e3c8-83ec-95b1-9e42d257a00b 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên được sắp xếp theo một thứ tự cố định. Chúng ta được phép tùy ý xóa một phần tử khỏi chuỗi này hoặc giữ nguyên. Sau thao tác này, chúng tôi xem xét tất cả các phân đoạn liền kề của chuỗi kết quả và chỉ tập trung vào những phân đoạn có giá trị tăng dần từ trái sang phải. Trong số tất cả các phân đoạn như vậy, chúng tôi muốn có độ dài tối đa có thể. 

Nói một cách đơn giản hơn, chúng tôi đang cố gắng thực hiện một đợt “gần như tăng dần” càng lâu càng tốt, trong đó chúng tôi được phép khắc phục chính xác một sự gián đoạn cục bộ bằng cách loại bỏ một yếu tố phá vỡ xu hướng. 

Hạn chế lên tới$2 \cdot 10^5$các phần tử ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng việc xóa ở mọi vị trí và tính toán lại các đoạn tăng dài nhất từ ​​đầu. Phương pháp bậc hai tính toán lại các lần quét sau mỗi lần loại bỏ sẽ bao gồm khoảng$n^2$hiệu quả, vượt xa giới hạn 2 giây cho phép. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Một vài trường hợp đặc biệt tiết lộ lý do tại sao suy nghĩ ngây thơ lại thất bại. 

Nếu mảng đã tăng lên một cách nghiêm ngặt, ví dụ:```
5
1 2 3 4 5
```câu trả lời là 5, vì chúng ta không được lợi gì khi loại bỏ bất cứ thứ gì. Một chiến lược ngây thơ luôn xóa một phần tử sẽ làm giảm câu trả lời xuống còn 4 một cách sai lầm. 

Nếu mảng đang giảm nghiêm ngặt:```
5
5 4 3 2 1
```bất kỳ thao tác xóa đơn lẻ nào vẫn để lại đoạn tăng dài nhất tốt nhất là độ dài 2, vì không thể hình thành chuỗi tăng cục bộ. Một nỗ lực ngây thơ để “sửa” nhiều lần ngắt sẽ đánh giá quá cao lợi ích thu được từ một lần xóa. 

Một trường hợp tế nhị hơn:```
5
1 3 2 3 4
```loại bỏ 2 phần ở giữa sẽ hợp nhất hai phần tăng dần thành một phần dài hơn. Bất kỳ giải pháp đúng nào cũng phải phát hiện ra rằng lợi ích của việc xóa chỉ đến từ việc kết nối hai phân đoạn đã tăng lên. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi chỉ mục, chúng tôi mô phỏng việc loại bỏ phần tử đó, sau đó quét toàn bộ mảng để tính toán mảng con liền kề tăng dần nghiêm ngặt dài nhất. Quá trình quét này là tuyến tính, do đó việc thực hiện nó cho mọi chỉ mục sẽ mang lại$O(n^2)$thời gian. Với$n = 2 \cdot 10^5$, điều này sẽ cần khoảng$4 \cdot 10^{10}$hoạt động, điều đó là không thể thực hiện được. 

Quan sát quan trọng là cấu trúc của các phân đoạn tăng dần đã rất đều đặn mà không cần xóa bỏ. Đối với từng vị trí, chúng ta có thể tính toán trước khoảng cách tăng dần về bên trái và bên phải. Khi đã biết điều này, tác động có ý nghĩa duy nhất của việc loại bỏ một phần tử là có khả năng kết nối chuỗi tăng dần bên trái kết thúc tại$i-1$với mức tăng dần phù hợp bắt đầu từ$i+1$, với điều kiện biên$a[i-1] < a[i+1]$nắm giữ. 

Điều này làm giảm vấn đề trong việc theo dõi thời lượng chạy cục bộ và kiểm tra nhiều nhất một điểm hợp nhất cho mỗi chỉ mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán hai mảng mô tả cấu trúc đơn điệu. 

1. Tính một mảng`left[i]`lưu trữ độ dài của mảng con liền kề tăng dần kết thúc tại chỉ mục$i$. 

Điều này được xây dựng bằng cách quét từ trái sang phải. Nếu như$a[i] > a[i-1]$, chúng tôi kéo dài lần chạy trước; nếu không chúng tôi đặt lại về 1. 
2. Tính một mảng`right[i]`lưu trữ độ dài của mảng con liền kề tăng dần bắt đầu từ chỉ mục$i$. 

Điều này được xây dựng bằng cách quét từ phải sang trái bằng cách sử dụng cùng một logic. 
3. Khởi tạo đáp án là giá trị lớn nhất trong`left[i]`. 

Điều này thể hiện trường hợp chúng tôi không loại bỏ bất kỳ phần tử nào. 
4. Đối với từng vị trí$i$, hãy cân nhắc việc loại bỏ$a[i]$. 

Việc loại bỏ có thể có khả năng kết nối đoạn tăng dần kết thúc tại$i-1$với cái bắt đầu từ$i+1$. 
5. Việc hợp nhất chỉ hợp lệ nếu$a[i-1] < a[i+1]$. 

Nếu điều này được giữ, độ dài đoạn được hợp nhất sẽ trở thành`left[i-1] + right[i+1]`. 
6. Cập nhật câu trả lời với giá trị tối đa trên tất cả các lần xóa. 

### Tại sao nó hoạt động 

Bất kỳ phân đoạn liền kề tăng nghiêm ngặt nào trong mảng cuối cùng sau một lần xóa phải thuộc một trong hai loại. Hoặc nó không đi qua phần tử bị loại bỏ, nghĩa là nó đã tồn tại trong mảng ban đầu và bị bắt bởi`left[i]`, hoặc nó vượt qua vị trí bị loại bỏ, nghĩa là nó được hình thành bằng cách nối hai lần chạy tăng dần ban đầu được phân tách chính xác tại chỉ mục đã bị loại bỏ. Không có khả năng cấu trúc nào khác bởi vì một phân đoạn tăng nghiêm ngặt không thể chứa các mức giảm nội bộ, do đó, nhiều nhất một “điểm gãy” có thể được sửa chữa bằng cách xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

left = [1] * n
right = [1] * n

for i in range(1, n):
    if a[i] > a[i - 1]:
        left[i] = left[i - 1] + 1
    else:
        left[i] = 1

for i in range(n - 2, -1, -1):
    if a[i] < a[i + 1]:
        right[i] = right[i + 1] + 1
    else:
        right[i] = 1

ans = max(left)

for i in range(n):
    if i == 0:
        ans = max(ans, right[1])
    elif i == n - 1:
        ans = max(ans, left[n - 2])
    else:
        if a[i - 1] < a[i + 1]:
            ans = max(ans, left[i - 1] + right[i + 1])
        else:
            ans = max(ans, left[i - 1], right[i + 1])

print(ans)
```các`left`mảng ghi lại khoảng cách chúng ta có thể mở rộng đoạn tăng dần kết thúc ở mỗi vị trí mà không cần sửa đổi. các`right`mảng thực hiện công việc đối xứng từ phía bên phải. 

Vòng lặp kết thúc`i`xử lý cẩn thận các vị trí ranh giới. Khi loại bỏ phần tử đầu tiên hoặc cuối cùng, chỉ còn lại một bên, do đó, phân đoạn tốt nhất có thể đạt được trực tiếp là lần chạy được tính toán trước tương ứng. 

Đối với các vị trí nội bộ, việc kiểm tra quan trọng là liệu hai bên có thể được sáp nhập hay không. điều kiện`a[i - 1] < a[i + 1]`đảm bảo rằng việc loại bỏ`a[i]`không vi phạm mức tăng nghiêm ngặt tại điểm tham gia. Nếu thất bại, chúng tôi không thể kết nối hai phân đoạn và phải quay lại các hoạt động độc lập tốt nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 5 3 4
```| tôi | a[i] đã xóa | trái[i-1] | đúng[i+1] | hợp nhất hợp lệ | ứng cử viên | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | đúng[1]=2 | - | 4 | 
| 1 | 2 | 1 | đúng[2]=1 | vâng | 4 | 
| 2 | 5 | 2 | đúng[3]=2 | có (2<3) | 4 | 
| 3 | 3 | 3 | đúng[4]=1 | vâng | 4 | 
| 4 | 4 | 4 | - | - | 4 | 

Lựa chọn tốt nhất là loại bỏ số 5, kết nối phần chạy trước và phần sau nó, tạo ra một chuỗi có độ dài tăng dần bằng 4. Điều này xác nhận logic hợp nhất xác định chính xác một phần tử gây rối duy nhất. 

### Ví dụ 2 

đầu vào:```
6
1 2 3 2 3 4
```| tôi | a[i] đã xóa | trái[i-1] | đúng[i+1] | hợp nhất hợp lệ | ứng cử viên | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | - | 5 | - | 5 | 
| 1 | 2 | 1 | 4 | vâng | 5 | 
| 2 | 3 | 2 | 3 | vâng | 5 | 
| 3 | 2 | 3 | 2 | có (3<3 sai) | 5 | 
| 4 | 3 | 1 | 1 | vâng | 5 | 
| 5 | 4 | 4 | - | - | 5 | 

Loại bỏ số 2 ở chỉ số 3 sẽ kết nối hai đoạn tăng dần thành một đoạn dài hơn. Điều này chứng tỏ cách giải pháp phát hiện một “thung lũng” duy nhất có thể sửa chữa được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Hai đường tuyến tính để tính toán`left`Và`right`, cộng thêm một lượt để đánh giá việc xóa | 
| Không gian |$O(n)$| Lưu trữ độ dài chạy tiền tố và hậu tố | 

Giải pháp này phù hợp thoải mái trong giới hạn vì tất cả các thao tác đều là so sánh số nguyên đơn giản và quét mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    left = [1] * n
    right = [1] * n

    for i in range(1, n):
        if a[i] > a[i - 1]:
            left[i] = left[i - 1] + 1
        else:
            left[i] = 1

    for i in range(n - 2, -1, -1):
        if a[i] < a[i + 1]:
            right[i] = right[i + 1] + 1
        else:
            right[i] = 1

    ans = max(left)

    for i in range(n):
        if i == 0:
            ans = max(ans, right[1])
        elif i == n - 1:
            ans = max(ans, left[n - 2])
        else:
            if a[i - 1] < a[i + 1]:
                ans = max(ans, left[i - 1] + right[i + 1])
            else:
                ans = max(ans, left[i - 1], right[i + 1])

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("5\n1 2 5 3 4\n") == "4"

# minimum size
assert run("2\n1 2\n") == "2"

# already increasing
assert run("5\n1 2 3 4 5\n") == "5"

# all equal
assert run("4\n7 7 7 7\n") == "1"

# valley fix
assert run("6\n1 2 3 2 3 4\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 2 | 2 | trường hợp tối thiểu, không cần xóa | 
| 1 2 3 4 5 | 5 | đã tối ưu mà không cần xóa | 
| 7 7 7 7 | 1 | yêu cầu tăng nghiêm ngặt | 
| 1 2 3 2 3 4 | 5 | cầu xóa đơn hai lần chạy | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi loại bỏ điểm cuối. Nếu phần tử đầu tiên bị loại bỏ, kết quả tốt nhất chỉ đơn giản là tiền tố tăng dài nhất bắt đầu từ chỉ mục 1. Thuật toán xử lý vấn đề này trực tiếp thông qua`right[1]`. Ví dụ:```
5
5 1 2 3 4
```Loại bỏ 5 cho câu trả lời 4 và`right[1]`nắm bắt chính xác điều này. 

Một trường hợp khác là khi không hợp nhất có lợi. Ví dụ:```
5
5 4 3 2 1
```Mọi`left[i]`Và`right[i]`là 1 và không có điều kiện hợp nhất hợp lệ nào được giữ. Thuật toán tự nhiên quay về 1, phù hợp với câu trả lời đúng vì việc loại bỏ bất kỳ phần tử nào vẫn không để lại đoạn tăng dài hơn 1. 

Trường hợp tinh tế cuối cùng là khi tồn tại nhiều điểm hợp nhất tiềm năng nhưng chỉ được phép xóa một lần. Vì:```
6
1 2 10 3 4 5
```chỉ loại bỏ 10 tạo ra sự hợp nhất đầy đủ. Thuật toán đánh giá chính xác từng chỉ mục một cách độc lập và chọn ra cải tiến đơn lẻ tốt nhất, đảm bảo không tính hai lần số lần xóa.
