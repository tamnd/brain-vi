---
problem: 926C
contest_id: 926
problem_index: C
name: "Is This a Zebra?"
contest_name: "VK Cup 2018 - Wild-card Round 1"
rating: 1700
tags: ["implementation"]
answer: passed_samples
verified: true
solve_time_s: 73
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a3270b2-6b2c-83ec-a440-f77b7015f16a
---

#CF 926C - Đây có phải ngựa vằn không? 

**Đánh giá:** 1700 
**Thẻ:** triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a3270b2-6b2c-83ec-a440-f77b7015f16a 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta có một hình ảnh một chiều được biểu diễn dưới dạng một chuỗi có độ dài$n$, trong đó mỗi vị trí có màu đen hoặc trắng. Việc giải thích theo cột: mỗi cột được tô màu đồng nhất, do đó toàn bộ hình ảnh sẽ giảm xuống một mảng nhị phân. 

Nhiệm vụ là quyết định xem mảng này có thể được phân tách thành các khối liền kề trong đó mỗi khối bao gồm các giá trị giống hệt nhau và tất cả các khối có cùng độ dài hay không. Màu sắc phải xen kẽ giữa các khối liền kề. Nói cách khác, mảng phải là sự lặp lại của một mẫu có độ dài cố định$k$, trong đó mỗi đoạn có độ dài$k$không đổi và các đoạn liên tiếp sẽ đổi màu. 

Đầu ra là một kiểm tra tính khả thi đơn giản: liệu chiều rộng sọc đồng nhất như vậy có tồn tại để có thể tạo ra toàn bộ chuỗi hay không. 

Ràng buộc$n \le 100000$ngụ ý rằng mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính. MỘT$O(n^2)$nỗ lực thử mọi chiều rộng sọc có thể có và xác thực nó trực tiếp sẽ yêu cầu kiểm tra tối đa$n$ứng viên, mỗi chi phí$O(n)$, dẫn đến$10^{10}$hoạt động trong trường hợp xấu nhất. Điều đó vượt xa giới hạn có thể chấp nhận được. Thậm chí một$O(n \log n)$giải pháp là không cần thiết vì cấu trúc có thể được xác minh trong một lần duy nhất. 

Một vài trường hợp khó khăn rất dễ bị đánh giá thấp. 

Ví dụ: nếu tất cả các phần tử đều giống hệt nhau$[1,1,1,1]$, câu trả lời đúng là “CÓ” vì nó có thể được xem như một sọc duy nhất bao phủ toàn bộ mảng. Việc triển khai ngây thơ đòi hỏi ít nhất hai phân đoạn xen kẽ có thể từ chối trường hợp này một cách không chính xác. 

Nếu độ dài mảng là số lẻ và chúng tôi cố gắng buộc chiều rộng sọc bằng nhau lớn hơn một, chẳng hạn như$[0,0,0,1,1]$, một số triển khai có thể cho rằng phân đoạn hợp lệ tồn tại một cách không chính xác mà không kiểm tra xem khối cuối cùng đã hoàn tất chưa. Yêu cầu là khả năng phân chia toàn cầu chứ không phải tính nhất quán cục bộ. 

Nếu mảng xen kẽ mọi phần tử, chẳng hạn như$[0,1,0,1,0,1]$, chiều rộng của sọc phải là 1. Bất kỳ cách tiếp cận nào giả định các sọc phải dài hơn một sọc sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chiều rộng sọc có thể$k$từ 1 đến$n$. Đối với mỗi ứng viên$k$, chúng tôi chia mảng thành các đoạn có độ dài liên tiếp$k$và xác minh hai điều kiện: mỗi phân đoạn phải đồng nhất và các phân đoạn liền kề phải khác nhau. 

Điều này hiệu quả vì nó phản ánh trực tiếp định nghĩa của mẫu ngựa vằn hợp lệ. Tuy nhiên, chi phí trở nên quá cao. Đối với mỗi$k$, chúng tôi quét toàn bộ mảng, thu được$O(n)$làm việc cho mỗi ứng viên và$O(n^2)$độ phức tạp tổng thể trong trường hợp xấu nhất. Với$n = 100000$, tốc độ này quá chậm. 

Quan sát quan trọng là mảng được xác định hoàn toàn bằng mã hóa độ dài chạy của nó. Khi chúng tôi nén nó thành các khối liên tiếp có giá trị giống hệt nhau, vấn đề sẽ giảm xuống còn việc kiểm tra xem tất cả độ dài chạy có bằng nhau hay không. 

Nếu mảng hợp lệ thì mỗi lần chạy các giá trị giống nhau liên tiếp phải căn chỉnh hoàn hảo với các ranh giới sọc. Điều đó có nghĩa là không có đường chạy nào có thể được chia thành các sọc và không đường sọc nào có thể hợp nhất nhiều đường chạy có độ dài khác nhau. Do đó, tất cả các lần chạy phải có cùng độ dài, ngoại trừ trường hợp tổng cộng chỉ có một lần chạy. 

Điều này biến vấn đề thành một lần quét đơn giản trong đó chúng tôi tính toán thời lượng chạy và xác minh tính đồng nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duyệt mảng từ trái sang phải trong khi theo dõi các giá trị bằng nhau liên tiếp. Mỗi lần giá trị thay đổi, hãy ghi lại độ dài của đoạn vừa kết thúc. Bước này nén đầu vào thành các chiều dài chạy, thu thập tất cả thông tin cấu trúc liên quan đến sự hình thành sọc. 
2. Lưu trữ độ dài lần chạy đầu tiên làm giá trị tham chiếu$k$. Điều này đại diện cho chiều rộng sọc ứng cử viên. 
3. Đối với mỗi lần chạy tiếp theo, hãy so sánh độ dài của nó với$k$. Nếu bất kỳ lần chạy nào khác nhau, cấu trúc không thể được phân chia thành các sọc có chiều rộng bằng nhau, do đó câu trả lời sẽ không hợp lệ ngay lập tức. 
4. Nếu quá trình truyền tải kết thúc mà không có mâu thuẫn, hãy chấp nhận cấu hình. 

Ý tưởng chính là các mẫu ngựa vằn hợp lệ thực thi sự căn chỉnh chặt chẽ giữa ranh giới đường chạy và ranh giới sọc. Nếu hai lần chạy liên tiếp có độ dài khác nhau thì bất kỳ chiều rộng sọc nào cũng sẽ chia tách một lần chạy hoặc hợp nhất các lần chạy không khớp, cả hai đều vi phạm định nghĩa. 

### Tại sao nó hoạt động 

Mọi giải pháp hợp lệ đều phân vùng mảng thành các khối có độ dài bằng nhau trong đó các giá trị xen kẽ nhau. Mỗi khối là đồng nhất, vì vậy mỗi lần chạy các giá trị giống hệt nhau phải được chứa đầy đủ trong một khối duy nhất. Vì tất cả các khối có kích thước bằng nhau nên mỗi lần chạy cũng phải có độ dài giống nhau. Điều này buộc phải có sự tương ứng một-một giữa các đường chạy và sọc, làm cho độ dài đường chạy bằng nhau cả cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
a = list(map(int, input().split()))

runs = []

cur = a[0]
length = 1

for i in range(1, n):
    if a[i] == cur:
        length += 1
    else:
        runs.append(length)
        cur = a[i]
        length = 1

runs.append(length)

ok = True
for i in range(1, len(runs)):
    if runs[i] != runs[0]:
        ok = False
        break

print("YES" if ok else "NO")
```Việc triển khai trực tiếp xây dựng độ dài chạy trong một lần chạy. Vòng lặp duy trì một phân đoạn hiện tại và đếm khoảng thời gian nó tiếp tục cho đến khi có thay đổi. Mỗi lần chạy hoàn thành sẽ được thêm vào danh sách. 

Bước xác thực được cố ý tối thiểu: một khi chúng tôi đã chạy hết thời gian, chỉ có sự bình đẳng mới quan trọng. Lần chạy đầu tiên xác định chiều rộng của sọc và mọi lần chạy khác phải khớp với chiều rộng đó. 

Một điểm tinh tế là chúng ta không cần phải kiểm tra rõ ràng sự luân phiên của các giá trị, bởi vì việc xây dựng chạy đã đảm bảo sự luân phiên theo định nghĩa của một điểm thay đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9
0 0 0 1 1 1 0 0 0
```Chạy thi công: 

| tôi | giá trị | chạy hiện tại | hành động | chạy | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | bắt đầu | [] | 
| 1 | 0 | 2 | mở rộng | [] | 
| 2 | 0 | 3 | mở rộng | [] | 
| 3 | 1 | 1 | đóng chạy, bắt đầu mới | [3] | 
| 4 | 1 | 2 | mở rộng | [3] | 
| 5 | 1 | 3 | mở rộng | [3] | 
| 6 | 0 | 1 | đóng chạy | [3, 3] | 
| 7 | 0 | 2 | mở rộng | [3, 3] | 
| 8 | 0 | 3 | mở rộng | [3, 3] | 

Lần chạy cuối cùng:$[3,3,3]$Tất cả thời lượng chạy đều khớp nhau, vì vậy câu trả lời là CÓ. 

Điều này xác nhận tính bất biến rằng chiều rộng sọc nhất quán sẽ tạo ra độ dài đường chạy giống hệt nhau. 

### Ví dụ 2 

đầu vào:```
7
0 0 0 1 1 1 1
```Chạy thi công: 

| tôi | giá trị | chạy hiện tại | hành động | chạy | 
| --- | --- | --- | --- | --- | 
| 0-2 | 0 | 3 | chạy | [] | 
| 3-5 | 1 | 3 | đóng + bắt đầu | [3] | 
| 6 | 1 | 4 | mở rộng | [3] | 

Lần chạy cuối cùng:$[3,4]$Độ dài các đường chạy khác nhau nên không có chiều rộng sọc đồng nhất. 

Điều này cho thấy một phân đoạn không nhất quán sẽ phá vỡ tính khả thi toàn cầu như thế nào mặc dù sự thay thế cục bộ là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được truy cập một lần để xây dựng độ dài chạy | 
| Không gian |$O(n)$| Trong trường hợp xấu nhất các giá trị xen kẽ tạo ra$n$chạy | 

Quét tuyến tính phù hợp thoải mái trong các ràng buộc đối với$n \le 100000$. Ngay cả đầu vào xen kẽ trong trường hợp xấu nhất cũng chỉ tạo ra 100000 lần chạy, vẫn có thể quản lý được cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    a = list(map(int, input().split()))

    runs = []
    cur = a[0]
    length = 1

    for i in range(1, n):
        if a[i] == cur:
            length += 1
        else:
            runs.append(length)
            cur = a[i]
            length = 1

    runs.append(length)

    ok = all(x == runs[0] for x in runs)
    return "YES" if ok else "NO"

# provided sample
assert run("9\n0 0 0 1 1 1 0 0 0\n") == "YES"

# all equal
assert run("4\n1 1 1 1\n") == "YES"

# alternating
assert run("6\n0 1 0 1 0 1\n") == "YES"

# invalid unequal runs
assert run("7\n0 0 0 1 1 1 1\n") == "NO"

# single element
assert run("1\n0\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | CÓ | trường hợp chạy một lần | 
| xen kẽ | CÓ | chiều rộng sọc = 1 | 
| chạy không đều | KHÔNG | phát hiện sự không phù hợp | 
| n = 1 | CÓ | trường hợp ranh giới tối thiểu | 

## Vỏ cạnh 

Một mảng hằng số duy nhất như$[1,1,1,1]$tạo ra chính xác một lần chạy. Các bản ghi thuật toán chạy =$[4]$, và vì không có mâu thuẫn nào nên kết quả trả về CÓ. Điều này phù hợp với cách giải thích rằng một sọc đơn là hợp lệ. 

Một mảng xen kẽ như$[0,1,0,1]$tạo ra các cuộc chạy$[1,1,1,1]$. Sự bình đẳng được duy trì trên tất cả các lần chạy, do đó độ rộng sọc 1 được suy ra chính xác, xác nhận rằng phương pháp này không giả định kích thước sọc tối thiểu. 

Một trường hợp như$[0,0,0,1,1]$tạo ra các cuộc chạy$[3,2]$. Sự không khớp được phát hiện ngay lập tức ở lần chạy thứ hai, chứng tỏ rằng việc căn chỉnh toàn cục không thành công ngay cả khi có sự thay thế cục bộ.
