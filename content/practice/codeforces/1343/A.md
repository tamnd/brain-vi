---
problem: 1343A
contest_id: 1343
problem_index: A
name: "Candies"
contest_name: "Codeforces Round 636 (Div. 3)"
rating: 900
tags: ["brute force", "math"]
answer: passed_samples
verified: true
solve_time_s: 135
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e1df0-693c-83ec-baca-c8680d507a68
---

#CF 1343A - Kẹo 

**Xếp hạng:** 900 
**Tags:** sức mạnh vũ phu, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e1df0-693c-83ec-baca-c8680d507a68 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được tặng một số giấy gói kẹo và chúng tôi biết chúng đến từ một kiểu mua hàng rất cụ thể. Vào ngày đầu tiên Vova mua một số lượng kẹo nguyên, gọi nó là$x$. Vào mỗi ngày tiếp theo, anh ta tăng gấp đôi số lần mua của ngày hôm trước, vì vậy trình tự mua hàng trông giống như$x, 2x, 4x, \dots, 2^{k-1}x$trong một số ngày không xác định$k$, Ở đâu$k > 1$. 

Nếu chúng ta cộng tất cả số kẹo đã mua trong tất cả các ngày, chúng ta sẽ nhận được:$$n = x(1 + 2 + 4 + \dots + 2^{k-1})$$Chuỗi hình học bên trong ngoặc luôn thu gọn về$2^k - 1$, do đó ràng buộc tương đương với việc tìm bất kỳ sự phân tách nào của$n$như vậy:$$n = x(2^k - 1), \quad k > 1, \quad x \ge 1$$Nhiệm vụ không phải là khôi phục bản gốc$x$Và$k$, mà chỉ đơn giản là xuất ra bất kỳ giá trị nào$x$điều đó có thể đã tạo ra cái đã cho$n$đối với một số hợp lệ$k$. 

Các ràng buộc cho phép lên đến$10^4$trường hợp thử nghiệm và$n$lên tới$10^9$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê tất cả các cặp$(x, k)$hoặc mô phỏng quy trình theo kiểu lồng nhau đơn giản cho từng trường hợp thử nghiệm. Một lực lượng vũ phu trực tiếp trên$k$vẫn còn nhỏ vì$2^k - 1$tăng trưởng theo cấp số nhân, do đó$k$nhiều nhất là khoảng 30 đối với lớn nhất$n$. Sự quan sát đó là rất quan trọng. 

Một trường hợp cạnh tinh tế là$k$phải lớn hơn 1. Nếu ai đó quên điều này, họ có thể cho phép không chính xác$k = 1$, điều đó sẽ làm cho$n = x$, nhưng điều này không hợp lệ trong vấn đề. Một vấn đề khác đang cố gắng khắc phục$x = 1$luôn luôn; điều này chỉ hoạt động khi$n$bản thân nó là số Mersenne và nói chung là không thành công. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ thử mọi cách có thể$k$, tính hệ số tổng$2^k - 1$, và kiểm tra xem$n$được chia cho nó. Nếu vậy thì$x = n / (2^k - 1)$là một câu trả lời hợp lệ Điều này hiệu quả vì công thức là chính xác và mọi cách xây dựng hợp lệ đều tương ứng với một số$k$. Vấn đề là hiệu suất vẫn bị giới hạn bởi bao nhiêu$k$giá trị chúng tôi kiểm tra. 

Quan sát quan trọng là hệ số nhân hình học$2^k - 1$phát triển cực kỳ nhanh chóng. Từ$n \le 10^9$, chúng ta chỉ cần kiểm tra tối đa khoảng 30 giá trị của$k$. Điều này biến vấn đề thành một vòng lặp nhỏ cho mỗi trường hợp thử nghiệm. Đối với mỗi$k$, chúng tôi kiểm tra tính chia hết và ngay lập tức trả về giá trị hợp lệ$x$. 

Không cần phải tìm kiếm tất cả các giải pháp hoặc tối ưu hóa thêm. Bất kỳ cặp hợp lệ nào đều được chấp nhận, vì vậy cặp thành công đầu tiên$k$là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả$x, k$|$O(n \log n)$|$O(1)$| Quá chậm | 
| Hãy thử tất cả$k$lên tới 30 |$O(t \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$. Mục đích là tìm một đại diện$n = x(2^k - 1)$. 
2. Lặp lại các giá trị có thể có của$k$, bắt đầu từ$k = 2$bởi vì vấn đề rõ ràng cấm$k = 1$. 
3. Đối với mỗi$k$, tính số nhân$m = 2^k - 1$. 
4. Nếu$m > n$, ngừng kiểm tra lớn hơn$k$, vì tất cả các số nhân trong tương lai chỉ tăng lên. 
5. Nếu$n \mod m = 0$, tính toán$x = n / m$và xuất nó ngay lập tức. 

Lý do việc dừng tham lam này có hiệu quả là khi hệ số hình học vượt quá$n$, nó không thể chia được nữa. 

### Tại sao nó hoạt động 

Mọi cấu hình hợp lệ đều tương ứng với một hệ số của biểu mẫu$n = x(2^k - 1)$. Thuật toán liệt kê một cách có hệ thống tất cả các giá trị có thể có của yếu tố thứ hai$2^k - 1$điều đó có thể phù hợp trong$n$. Vì các giá trị này tăng dần theo$k$, chúng tôi được đảm bảo sẽ gặp đúng nếu nó tồn tại. Sau khi tìm thấy, phép chia sẽ lấy lại giá trị tương ứng duy nhất$x$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        
        k = 2
        power = 3  # 2^2 - 1
        
        while power <= n:
            if n % power == 0:
                print(n // power)
                break
            k += 1
            power = (1 << k) - 1

solve()
```Việc thực hiện phản ánh trực tiếp đạo hàm. Biến`power`bài hát$2^k - 1$, bắt đầu từ$k = 2$, tương ứng với$3$. Vòng lặp tăng lên$k$cho đến khi số nhân vượt quá$n$, đảm bảo chúng tôi chỉ kiểm tra những ứng viên khả thi. 

Một lỗi phổ biến là tính toán lại lũy thừa từ đầu bằng cách sử dụng lũy ​​thừa bên trong vòng lặp, việc này không cần thiết nhưng vẫn đủ nhanh. Một vấn đề tế nhị khác bắt đầu từ$k = 1$, sẽ giới thiệu trường hợp không hợp lệ$2^1 - 1 = 1$và cho phép sai$x = n$. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào từ mẫu. 

### Ví dụ 1:$n = 21$| k |$2^k - 1$| có thể chia được? | hành động | 
| --- | --- | --- | --- | 
| 2 | 3 | vâng | đầu ra$21 / 3 = 7$| 

Thuật toán dừng ngay lập tức vì$21$chia hết cho$3$. Điều này tương ứng với$k = 2, x = 7$, tái tạo lại$7 + 14 = 21$. 

### Ví dụ 2:$n = 28$| k |$2^k - 1$| có thể chia được? | hành động | 
| --- | --- | --- | --- | 
| 2 | 3 | không | tiếp tục | 
| 3 | 7 | vâng | đầu ra$28 / 7 = 4$| 

Điều này tạo ra$k = 3, x = 4$, tương ứng với$4 + 8 + 16 = 28$. 

Những dấu vết này cho thấy thuật toán không yêu cầu tìm kiếm tất cả các phân tách mà chỉ tìm một ước số hình học hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log n)$| mỗi bài kiểm tra kiểm tra tối đa ~30 giá trị của$k$| 
| Không gian |$O(1)$| chỉ một số số nguyên được lưu trữ | 

Sự tăng trưởng theo cấp số nhân của$2^k - 1$đảm bảo vòng lặp cực kỳ nhỏ ngay cả trong trường hợp đầu vào xấu nhất, giữ cho giải pháp hoạt động tốt trong giới hạn thời gian$10^4$trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())

        k = 2
        power = 3
        while power <= n:
            if n % power == 0:
                out.append(str(n // power))
                break
            k += 1
            power = (1 << k) - 1

    return "\n".join(out)

# provided samples
assert run("7\n3\n6\n7\n21\n28\n999999999\n999999984\n") == "1\n2\n1\n7\n4\n333333333\n333333328"

# minimum edge
assert run("1\n3\n") == "1"

# Mersenne-like case
assert run("1\n7\n") == "1"

# non-trivial multiple test
assert run("3\n6\n21\n28\n") == "2\n7\n4"

# large value sanity
assert run("1\n999999999\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n3 | 1 | hợp lệ nhỏ nhất$k=2$trường hợp | 
| 1\n7 | 1 | hoàn hảo$2^k-1$cấu trúc | 
| 3\n6\n21\n28 | 2\n7\n4 | đường phân chia hỗn hợp | 
| 1\n999999999 | hợp lệ x | căng thẳng trên vòng ràng buộc | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$n$bản thân nó có dạng$2^k - 1$. Ví dụ,$n = 7$. Thuật toán bắt đầu với$k = 2$, kiểm tra$3$, chuyển đến$7$, và tìm số chia ngay lập tức, tạo ra$x = 1$. Điều này tương ứng với một chuỗi hình học đầy đủ bắt đầu từ 1. 

Một trường hợp khác là khi$n$không chia hết cho số nhân nhỏ nhưng sau đó sẽ chia hết$k$, chẳng hạn như$n = 28$. Vòng lặp bỏ qua$k = 2$từ$28 \bmod 3 \neq 0$, sau đó thành công tại$k = 3$với số nhân$7$, mang lại$x = 4$. Thuật toán không chuyển giao sớm nên không bỏ lỡ các phân tách hợp lệ sau này. 

Cuối cùng, đối với kích thước lớn$n$giống$999999999$, vòng lặp vẫn kết thúc nhanh chóng vì$2^k - 1$vượt quá$n$trong khoảng 30 lần lặp, đảm bảo thời gian chạy có giới hạn bất kể phân phối đầu vào.
