---
problem: 961B
contest_id: 961
problem_index: B
name: "Lecture Sleep"
contest_name: "Educational Codeforces Round 41 (Rated for Div. 2)"
rating: 1200
tags: ["data structures", "dp", "implementation", "two pointers"]
answer: passed_samples
verified: true
solve_time_s: 74
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a3280b1-1f98-83ec-b0ee-5785d1836f05
---

#CF 961B - Giảng Giấc Ngủ 

**Đánh giá:** 1200 
**Tags:** cấu trúc dữ liệu, dp, triển khai, hai con trỏ 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a3280b1-1f98-83ec-b0ee-5785d1836f05 

--- 

## Giải pháp 

## Hiểu vấn đề 

Bài giảng là một chuỗi nhiều phút, trong mỗi phút giảng viên đưa ra một số định lý nhất định. Nếu Mishka thức vào phút đó thì những định lý đó sẽ được ghi lại; nếu anh ta ngủ, họ sẽ bị lạc. 

Điều khó khăn là Mishka có kiểu ngủ của riêng mình, được biểu thị bằng một mảng nhị phân. Giá trị 1 có nghĩa là anh ấy tỉnh táo một cách tự nhiên và ghi lại mọi thứ trong phút đó, trong khi giá trị 0 có nghĩa là anh ấy đang ngủ và không ghi lại gì. Bạn được phép can thiệp đúng một lần bằng cách chọn một khối có chiều dài liền kề nhau`k`phút và buộc Mishka phải thức suốt cả buổi tập, bất kể trạng thái tự nhiên của anh ấy như thế nào. Trong khối đó, anh ta ghi lại tất cả các định lý như thể anh ta đang tỉnh táo. 

Mục tiêu là chọn vị trí bắt đầu tốt nhất có thể cho phân đoạn bắt buộc phải thức này sao cho tổng số định lý được ghi lại là tối đa. 

Đầu ra là một số nguyên duy nhất: tổng số định lý tối đa mà Mishka có thể ghi lại sau khi áp dụng cửa sổ bắt buộc thức đúng một lần. 

Ràng buộc`n ≤ 100000`loại trừ mọi giải pháp thử mọi cửa sổ có thể và tính lại tổng từ đầu. Một cách tiếp cận ngây thơ sẽ là O(nk), nó trở nên quá chậm trong trường hợp xấu nhất là khoảng 10^10 thao tác. Điều này đẩy chúng ta tới một phương pháp tuyến tính hoặc gần tuyến tính. 

Một vấn đề nhỏ xuất hiện khi phân đoạn tốt nhất trùng lặp nhiều với những phút đã thức. Việc triển khai ngây thơ chỉ tính tổng phân đoạn bắt buộc hoặc chỉ tính tổng các phần thức một cách riêng biệt có thể tính gấp đôi hoặc bỏ lỡ các đóng góp nếu nó không tách biệt cẩn thận đường cơ sở và mức tăng thêm. 

Ví dụ, nếu tất cả`t[i] = 1`, câu trả lời đơn giản là tổng của tất cả`a[i]`và mọi lựa chọn cửa sổ sẽ không thay đổi bất cứ điều gì. Nếu một giải pháp ngây thơ thêm nhầm phần đóng góp của cửa sổ mà không trừ đi sự trùng lặp, nó sẽ bị tính quá mức. 

Một trường hợp khác là khi tất cả`t[i] = 0`. Khi đó câu trả lời chỉ đơn giản là mảng con có tổng độ dài tối đa`k`và bất kỳ lỗi nào trong việc xử lý cửa sổ sẽ trực tiếp hiển thị dưới dạng hành vi trượt không chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force thử mọi vị trí bắt đầu có thể có của phân đoạn thức tỉnh cưỡng bức. Đối với mỗi vị trí`i`, nó tính toán lại tổng phần đóng góp: tổng của tất cả các định lý mà Mishka ghi lại một cách tự nhiên cộng với các định lý bổ sung thu được khi đánh thức anh ta dậy trong`[i, i+k-1]`. 

Đóng góp tự nhiên là cố định và có thể được tính một lần. Khó khăn nằm ở việc tính toán hiệu quả độ lợi cho mỗi cửa sổ. Trong một phiên bản đơn giản, với mỗi cửa sổ chúng tôi quét`k`các phần tử, dẫn đến độ phức tạp O(nk). Với`n = 10^5`, điều này trở nên quá chậm. 

Quan sát quan trọng là đóng góp cơ bản không phụ thuộc vào vị trí đặt cửa sổ bắt buộc. Những thay đổi chỉ là những gì xảy ra bên trong cửa sổ đã chọn. Bên trong cửa sổ, bất cứ lúc nào`t[i] = 0`trở nên hữu ích mới, góp phần`a[i]`, trong khi số phút với`t[i] = 1`không đóng góp gì thêm vì chúng đã được tính vào đường cơ sở. 

Điều này biến vấn đề thành cực đại hóa tổng cửa sổ trượt trên một mảng dẫn xuất trong đó mỗi vị trí đóng góp`a[i]`chỉ khi Mishka ngủ ở đó, nếu không thì là 0. Sau khi phép biến đổi này được thực hiện, nhiệm vụ sẽ giảm xuống việc tìm tổng tối đa của bất kỳ mảng con nào có độ dài`k`, có thể được thực hiện bằng cửa sổ trượt trong O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách vấn đề thành những gì Mishka đã nhận được và những gì chúng tôi có thể mở khóa thêm. 

1. Tính tổng cơ sở của các định lý từ tất cả các phút khi Mishka đã thức, nghĩa là tất cả các chỉ số`i`Ở đâu`t[i] = 1`. Giá trị này không bao giờ thay đổi, bất kể lựa chọn cửa sổ của chúng ta. 
2. Xây dựng ý tưởng về "mảng khuếch đại" về mặt khái niệm, trong đó`gain[i] = a[i]`nếu như`t[i] = 0`, nếu không thì`gain[i] = 0`. Điều này chỉ đại diện cho những định lý bổ sung mà chúng ta có thể mở khóa bằng cách buộc chúng ta phải tỉnh táo vào từng phút.`i`. 
3. Xem xét bất kỳ cửa sổ có độ dài nào`k`. Nếu chúng ta buộc Mishka thức dậy ở đó, lợi ích bổ sung chính xác là tổng của`gain[i]`bên trong cửa sổ đó. Điều này hiệu quả vì những phút anh ấy đã thức không tạo thêm giá trị. 
4. Tính tổng số đầu tiên`k`các phần tử của mảng khuếch đại. Đây là mức tăng cửa sổ ban đầu. 
5. Trượt cửa sổ từ trái sang phải. Ở mỗi bước, hãy trừ phần tử rời khỏi cửa sổ và thêm phần tử mới vào đó. Điều này cập nhật mức tăng O(1) mỗi ca. 
6. Theo dõi tổng cửa sổ tối đa được thấy trong quá trình trượt này. 
7. Cộng mức tăng tối đa này vào tổng cơ sở và đưa ra kết quả. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là mọi định lý được tính chính xác một lần: theo đường cơ sở (số phút đã thức) hoặc theo mức tăng từ cửa sổ bắt buộc. Việc chuyển đổi mức tăng chỉ tách biệt những đóng góp bị thiếu trong đường cơ sở. Mọi lựa chọn hợp lệ của phân đoạn bắt buộc tương ứng chính xác với một tổng mảng con liền kề trong mảng khuếch đại, do đó, việc tối đa hóa một phân đoạn sẽ tối đa hóa phân khúc kia. Không cần điều chỉnh chồng chéo ngoài việc chuyển đổi, điều này đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))
t = list(map(int, input().split()))

base = 0
gain = [0] * n

for i in range(n):
    if t[i] == 1:
        base += a[i]
    else:
        gain[i] = a[i]

window = sum(gain[:k])
best = window

for i in range(k, n):
    window += gain[i] - gain[i - k]
    if window > best:
        best = window

print(base + best)
```Đầu tiên, mã sẽ tính toán mức đóng góp luôn đạt được từ những phút đã thức. Sau đó, nó xây dựng mảng khuếch đại tiềm ẩn bằng cách chỉ đặt các giá trị ở nơi Mishka đang ngủ. Tổng cửa sổ ban đầu tương ứng với việc buộc kỹ thuật trên phân đoạn đầu tiên. Vòng trượt cập nhật cửa sổ một cách hiệu quả bằng cách loại bỏ phần tử ngoài cùng bên trái và thêm phần tử tiếp theo, duy trì cập nhật O(1) mỗi ca. Câu trả lời cuối cùng sẽ bổ sung mức tăng thêm tốt nhất có thể đạt được vào đường cơ sở. 

Một cạm bẫy triển khai phổ biến là quên rằng số phút thức bên trong cửa sổ bắt buộc không được tính gấp đôi. Điều này có thể tránh được hoàn toàn bằng cách tách đường cơ sở và mức tăng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6, k = 3
a = [1, 3, 5, 2, 5, 4]
t = [1, 1, 0, 1, 0, 0]
```Đóng góp cơ bản đến từ các chỉ số 0 và 1. 

| tôi | một [tôi] | t[i] | căn cứ | đạt được[i] | cửa sổ (3) | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 0 | | 
| 1 | 3 | 1 | 4 | 0 | | 
| 2 | 5 | 0 | 4 | 5 | | 
| 3 | 2 | 1 | 4 | 0 | | 
| 4 | 5 | 0 | 4 | 5 | | 
| 5 | 4 | 0 | 4 | 4 | | 

Cửa sổ trượt qua mức tăng: 

- [2..4] cho 5 + 0 + 5 = 10 
- [3..5] cho 0 + 5 + 4 = 9 
- mức tăng tốt nhất = 10 

Đáp án cuối cùng = 4 + 10 = 14 

Dấu vết này cho thấy phân đoạn bắt buộc chỉ đóng góp các giá trị bị thiếu chứ không phải tổng giá trị. 

### Ví dụ 2 

đầu vào:```
n = 5, k = 2
a = [4, 1, 7, 3, 2]
t = [0, 0, 1, 0, 0]
```Đường cơ sở chỉ là chỉ số 2. 

Mảng tăng là: 

[4, 1, 0, 3, 2] 

Tổng cửa sổ: 

- [0..1] = 5 
- [1..2] = 1 
- [2..3] = 3 
- [3..4] = 5 

Mức tăng tốt nhất = 5 

Đáp án cuối cùng = 7 + 5 = 12 

Ví dụ này nêu bật cách cửa sổ tối ưu có thể nằm hoàn toàn ở những vùng mà Mishka đang ngủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một đường chuyền cơ bản cộng với một đường chuyền cửa sổ trượt | 
| Không gian | O(1) | Mảng Gain có thể tránh được hoặc được lưu trữ ngầm | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì nó xử lý mảng theo thời gian tuyến tính và chỉ sử dụng bộ nhớ bổ sung không đổi ngoài bộ nhớ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    t = list(map(int, input().split()))

    base = 0
    gain = [0] * n

    for i in range(n):
        if t[i] == 1:
            base += a[i]
        else:
            gain[i] = a[i]

    window = sum(gain[:k])
    best = window

    for i in range(k, n):
        window += gain[i] - gain[i - k]
        best = max(best, window)

    return str(base + best)

# provided sample
assert run("6 3\n1 3 5 2 5 4\n1 1 0 1 0 0\n") == "14"

# all awake
assert run("3 2\n5 6 7\n1 1 1\n") == "18"

# all asleep
assert run("5 2\n1 2 3 4 5\n0 0 0 0 0\n") == "9"

# k = n
assert run("4 4\n1 2 3 4\n0 1 0 1\n") == "10"

# alternating pattern
assert run("6 2\n1 2 3 4 5 6\n0 1 0 1 0 1\n") == "16"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều tỉnh táo | 18 | không thể tăng thêm | 
| ngủ hết rồi | 9 | tính chính xác của cửa sổ trượt thuần túy | 
| k = n | 10 | trường hợp ghi đè đầy đủ | 
| xen kẽ | 16 | đường cơ sở hỗn hợp và đạt được sự tương tác | 

## Vỏ cạnh 

Khi tất cả`t[i] = 1`, mảng khuếch đại trở thành hoàn toàn bằng không. Cửa sổ trượt không bao giờ cải thiện đường cơ sở, do đó thuật toán trả về chính xác toàn bộ`a`. Bất kỳ cách triển khai không chính xác nào mà vẫn cộng tổng cửa sổ sẽ làm tăng câu trả lời. 

Khi tất cả`t[i] = 0`, đường cơ sở bằng 0 và câu trả lời rút gọn thành việc tìm mảng con có tổng độ dài tối đa`k`. Thuật toán xử lý việc này trực tiếp thông qua cửa sổ khuếch đại. Sai lầm ở đây thường xuất phát từ việc tính lại tổng số tiền đầy đủ trên mỗi cửa sổ một cách không hiệu quả hoặc quản lý sai chỉ số. 

Khi`k = n`, cửa sổ sẽ bao phủ toàn bộ mảng đúng một lần. Lợi ích trở thành tổng của tất cả`a[i]`Ở đâu`t[i] = 0`và câu trả lời cuối cùng sẽ trở thành tổng của tất cả các phần tử, như mong đợi. Điều này kiểm tra việc xử lý chính xác các ranh giới toàn phạm vi.
