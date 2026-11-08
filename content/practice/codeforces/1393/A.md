---
title: "CF 1393A - Tô màu cầu vồng, Fluttershy và cờ vua"
description: "Chúng ta có một lưới vuông có kích thước $n nhân n$. Mục tiêu là bao phủ hoàn toàn mọi ô bằng hai màu theo mô hình bàn cờ, nghĩa là các ô liền kề phải luôn có màu đối lập nhau."
date: "2026-06-11T09:54:12+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1393
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 662 (Div. 2)"
rating: 800
weight: 1393
solve_time_s: 81
verified: true
draft: false
---

[CF 1393A - Tô màu cầu vồng, Fluttershy và cờ vua](https://codeforces.com/problemset/problem/1393/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới vuông có kích thước$n \times n$. Mục tiêu là bao phủ hoàn toàn mọi ô bằng hai màu theo mô hình bàn cờ, nghĩa là các ô liền kề phải luôn có màu đối lập nhau. 

Quá trình xây dựng bị hạn chế: ban đầu, chỉ có đường viền bên ngoài của lưới được coi là "có sẵn" và từ đó, các ô được thêm một hoặc nhiều ô mỗi lượt. Mỗi ô mới được đặt phải chạm vào ít nhất một ô hiện có trước đó dọc theo một cạnh. Trong một lượt, chỉ một người chơi hành động, nhưng cô ấy có thể đặt nhiều ô có màu của riêng mình miễn là thỏa mãn quy tắc sắp xếp. 

Câu hỏi hoàn toàn là về việc lập kế hoạch: cần bao nhiêu lượt, giả sử sự hợp tác tối ưu giữa hai người chơi, để xây dựng đầy đủ bất kỳ màu cờ hợp lệ nào của lưới. 

Ràng buộc$n \le 10^9$ngay lập tức loại trừ mọi mô phỏng lưới. Bất kỳ giải pháp nào cố gắng mô hình hóa bảng hoặc vị trí xử lý theo từng ô sẽ yêu cầu$O(n^2)$công việc hoặc trí nhớ, điều đó là không thể. Thậm chí$O(n)$mỗi trường hợp kiểm thử sẽ quá chậm trong trường hợp xấu nhất nếu có nhiều truy vấn được đưa ra. 

Khó khăn chính là câu trả lời không phải về hình học của lưới theo nghĩa phức tạp, mà là về cách mô hình cờ vua có thể được “xếp lớp” dưới ràng buộc kề cận. 

Một sai lầm ngây thơ xuất phát từ việc cho rằng vì có hai màu nên câu trả lời luôn là 2. Điều này thất bại ngay lập tức đối với$n = 4$. Trong trường hợp đó, đầu ra mẫu là 3, cho thấy một số hạn chế về cấu trúc ngăn cản việc hoàn thành mọi thứ trong hai bước di chuyển xen kẽ. 

Một cạm bẫy tinh vi khác là giả định tính đối xứng giữa số chẵn và số lẻ.$n$. Ví dụ,$n = 3$cần 2 lượt, trong khi$n = 4$cần 3 lượt, nên chỉ tính chẵn lẻ thôi không phải là toàn bộ câu chuyện. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ mô phỏng quá trình xây dựng. Chúng tôi sẽ duy trì một lưới, liên tục đặt các tập hợp ô hợp lệ mỗi lượt và thực thi ràng buộc kề cận một cách rõ ràng. Ngay cả khi chúng ta tham lam cố gắng lấp đầy các phần mở rộng hợp lệ tối đa mỗi lượt, chúng ta vẫn cần theo dõi các ràng buộc về kết nối và màu sắc trong một lượt.$n \times n$kết cấu. Điều này dẫn đến ít nhất$O(n^2)$trạng thái hoặc quét lặp đi lặp lại, điều này là không khả thi khi$n$đạt tới$10^9$. 

Quan sát quan trọng là vấn đề không phụ thuộc vào các vị trí riêng lẻ mà phụ thuộc vào số lượng “lớp” luân phiên cưỡng bức tồn tại trong cấu trúc lưới. 

Trong cách tô màu bàn cờ, lưới có thể được coi là các ô chẵn lẻ xen kẽ. Khi chúng ta bắt đầu từ ranh giới, mỗi lần di chuyển có thể “đẩy vào” một lớp màu nhất quán một cách hiệu quả, nhưng cấu trúc đường chéo buộc một số ô chỉ có thể truy cập được sau nhiều đợt mở rộng. 

Sự đơn giản hóa quan trọng là câu trả lời chỉ phụ thuộc vào việc liệu$n$là chẵn hay lẻ, và chính xác hơn là tồn tại bao nhiêu “vòng” phụ thuộc hoàn chỉnh khi mở rộng từ ranh giới vào trong. 

Đối với số lẻ$n$, ô trung tâm hoạt động như một điểm gặp gỡ tự nhiên của việc mở rộng, cho phép phân lớp hiệu quả hơn một chút. Thậm chí$n$, lưới phân chia đối xứng hơn, cần thêm một lượt nữa để điều hòa cấu trúc chẵn lẻ ở giữa. 

Điều này dẫn đến một hình thức đóng đơn giản:$$\text{answer} = \left\lfloor \frac{n}{2} \right\rfloor + 1$$Điều này phù hợp với cả hai trường hợp mẫu: 

cho$n = 3$, chúng tôi nhận được$1 + 1 = 2$. 

Vì$n = 4$, chúng tôi nhận được$2 + 1 = 3$. 

Lý do sâu xa hơn là mỗi cặp hàng (hoặc cột) đóng góp một “lớp” xây dựng và cần có một lượt bổ sung để hoàn thiện ràng buộc luân phiên cờ vua đối với sự không khớp chẵn lẻ còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n^2)$| Quá chậm | 
| Công thức$\lfloor n/2 \rfloor + 1$|$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng giá trị của$n$. Mỗi trường hợp thử nghiệm đều độc lập nên không có trạng thái nào được chia sẻ giữa chúng. 
2. Tính toán$n // 2$. Giá trị này biểu thị số lượng “lớp ghép nối” đầy đủ của lưới tồn tại khi mở rộng từ ranh giới vào trong. 
3. Thêm 1 vào kết quả. Điều này giải thích cho bước hợp nhất cuối cùng trong đó giới hạn màu cờ vua buộc phải thực hiện thêm một lượt ngoài việc ghép lớp đơn giản. 
4. Xuất ngay giá trị tính toán cho từng test. 

### Tại sao nó hoạt động 

Quá trình xây dựng có hiệu quả giảm thiểu việc bong tróc lưới vào trong theo các lớp đối xứng. Mỗi cặp cạnh đối diện đóng góp một bước mở rộng đầy đủ. Tuy nhiên, ràng buộc về tính chẵn lẻ của màu cờ vua ngăn cản việc ghép đôi hoàn hảo trong mọi trường hợp, buộc phải thực hiện thêm một bước nữa để giải quyết cấu hình cuối cùng. Vì mọi công trình hợp lệ đều phải tôn trọng sự lan truyền lân cận từ ranh giới, nên không có chiến lược nào có thể giảm số lớp bên dưới$\lfloor n/2 \rfloor$và sự đối chiếu chẵn lẻ cuối cùng sẽ thực thi thuật ngữ không đổi bổ sung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

T = int(input())
for _ in range(T):
    n = int(input())
    print(n // 2 + 1)
```Giải pháp trực tiếp triển khai biểu thức dạng đóng dẫn xuất. Điều tinh tế duy nhất là đảm bảo sử dụng phép chia số nguyên vì công thức phụ thuộc vào hành vi của sàn. Mỗi trường hợp thử nghiệm được xử lý độc lập trong thời gian không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
```Chúng tôi tính toán: 

| Bước | Giá trị | 
| --- | --- | 
| n | 3 | 
| n // 2 | 1 | 
| trả lời | 2 | 

Lưới có một ô trung tâm cho phép cả hai màu lan truyền hiệu quả từ các hướng ngược nhau, nhưng vẫn yêu cầu hai lượt riêng biệt do các ràng buộc xen kẽ gặp nhau ở giữa. 

### Ví dụ 2 

đầu vào:```
n = 4
```| Bước | Giá trị | 
| --- | --- | 
| n | 4 | 
| n // 2 | 2 | 
| trả lời | 3 | 

Ở đây, lưới chia thành hai nửa đối xứng không có ô phân giải trung tâm. Điều này buộc phải thực hiện thêm một lượt so với việc xếp lớp đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Một phép tính số học cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$| Không sử dụng công trình phụ trợ | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với$n \le 10^9$, vì vậy mọi công thức về thời gian không đổi cho mỗi lần kiểm tra là đủ. Giải pháp hoạt động thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        out.append(str(n // 2 + 1))
    return "\n".join(out)

# provided samples
assert solve("2\n3\n4\n") == "2\n3"

# minimum case
assert solve("1\n1\n") == "1"

# small odd/even mix
assert solve("3\n2\n3\n5\n") == "2\n2\n3"

# large value sanity
assert solve("1\n1000000000\n") == str(1000000000 // 2 + 1)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | lưới nhỏ nhất | 
| 2,3,5 | 2,2,3 | hành vi ngang bằng | 
| 1e9 | 500000001 | độ đúng giới hạn trên | 

## Vỏ cạnh 

Khi nào$n = 1$, lưới là một ô duy nhất. Công thức cho$1 // 2 + 1 = 1$, phù hợp với thực tế là không cần phân lớp ngoài vị trí ban đầu. 

Đối với rất lớn$n$, chẳng hạn như$10^9$, việc tính toán vẫn giữ nguyên thời gian không đổi. Không có rủi ro tràn trong Python và công thức chỉ phụ thuộc vào phép chia số nguyên, do đó kết quả vẫn chính xác mà không cần mô phỏng.
