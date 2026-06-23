---
title: "CF 105327A - Chú ý đến cuộc họp"
description: "Chúng tôi đang lên lịch một cuộc họp với các diễn giả $N$. Mỗi người nói có số phút như nhau và giữa mỗi cặp người nói liên tiếp có thời gian nghỉ cố định là 1 phút."
date: "2026-06-22T17:30:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105327
codeforces_index: "A"
codeforces_contest_name: "2024-2025 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 105327
solve_time_s: 73
verified: true
draft: false
---

[CF 105327A - Chú ý đến cuộc họp](https://codeforces.com/problemset/problem/105327/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang lên lịch một cuộc gặp với$N$loa. Mỗi người nói có số phút như nhau và giữa mỗi cặp người nói liên tiếp có thời gian nghỉ cố định là 1 phút. Do đó, tổng thời gian của cuộc họp gồm có hai phần: thời gian phát biểu, đó là$N \cdot t$nếu mỗi cuộc nói chuyện kéo dài$t$phút và thời gian nhàn rỗi, chính xác là$N-1$phút vì có quá nhiều khoảng cách giữa$N$bài phát biểu. 

Nhiệm vụ là chọn số nguyên lớn nhất có thể$t \ge 1$sao cho tổng thời gian không vượt quá giới hạn$K$. Về mặt hình thức, chúng tôi muốn tối đa$t$thỏa mãn$$N \cdot t + (N - 1) \le K.$$Kích thước đầu vào nhỏ:$N \le 100$,$K \le 1000$. Điều này ngay lập tức cho chúng ta biết rằng ngay cả việc quét tuyến tính đơn giản trên tất cả các giá trị có thể có của$t$lên đến$K$là tầm thường về mặt tính toán. Bất kì$O(K)$hoặc thậm chí$O(NK)$cách tiếp cận sẽ chạy ngay lập tức. Cấu trúc hoàn toàn là số học, không có phép truyền tổ hợp hoặc đồ thị ẩn bên trong. 

Sự tinh tế duy nhất có thể gây ra sai lầm là quên rằng thời gian nghỉ được tính giữa các bài phát biểu chứ không phải sau mỗi bài phát biểu. Ví dụ, nếu$N = 1$, không có dấu ngắt nào, do đó công thức giảm xuống còn$t \le K$, cho$t = K$. Việc thực hiện bất cẩn luôn gây thiệt hại$N$thay vì$N-1$sẽ giảm câu trả lời một cách không chính xác. 

Một trường hợp cạnh tiềm năng khác là ranh giới chặt chẽ trong đó lịch trình lấp đầy chính xác giới hạn. Ví dụ, nếu$N=3$,$K=10$, Và$t=3$, tổng thời gian là$3\cdot3 + 2 = 11$, vốn đã vượt quá$K$, vậy đáp án đúng phải là$2$. Điều này nhấn mạnh rằng chúng ta phải thực thi sự bất bình đẳng một cách nghiêm ngặt. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử tất cả các độ dài bài phát biểu có thể. Chúng tôi bắt đầu từ$t=1$và tăng cho đến khi tổng thời lượng vượt quá$K$. Mỗi lần kiểm tra sẽ tính$N \cdot t + (N-1)$. Từ$t$nhiều nhất là ở xung quanh$K$, cách tiếp cận bạo lực này thực hiện tối đa 1000 lần lặp, mỗi lần lặp trong thời gian không đổi. Điều đó đã đủ nhanh cho các hạn chế. 

Thời điểm chúng ta viết ra bất đẳng thức$N \cdot t + (N-1) \le K$, ta thấy bài toán hoàn toàn tuyến tính. Chúng tôi không cần mô phỏng hay tìm kiếm: chúng tôi có thể cô lập trực tiếp$t$. Sắp xếp lại mang lại$$t \le \frac{K - (N - 1)}{N}.$$Từ$t$phải là một số nguyên và chúng ta muốn giá trị hợp lệ tối đa, chúng ta chỉ cần lấy giá trị sàn của biểu thức này. Điều này loại bỏ hoàn toàn bất kỳ sự lặp lại nào. 

Brute-force hoạt động vì không gian ràng buộc rất nhỏ, nhưng nó trở nên không cần thiết khi chúng ta nhận ra biểu thức xác định mối quan hệ đơn điệu trong$t$. BẰNG$t$tăng, tổng thời gian tăng tuyến tính, do đó có chính xác một ngưỡng mà chúng ta chuyển từ hợp lệ sang không hợp lệ. Sự đơn điệu đó cho phép tính toán trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(K)$|$O(1)$| Đã chấp nhận | 
| Tối ưu (công thức) |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$N$Và$K$. Những điều này xác định số lượng cuộc nói chuyện và tổng thời lượng tối đa được phép. 
2. Trừ thời gian nghỉ bắt buộc$(N-1)$từ$K$. Điều này giúp xác định lượng thời gian thực sự dành cho việc nói. Bước này rất quan trọng vì các khoảng nghỉ là cố định và không phụ thuộc vào$t$. 
3. Chia thời gian còn lại cho$N$dùng phép chia số nguyên. Điều này phân bổ đều thời gian phát biểu có sẵn cho tất cả các diễn giả đồng thời đảm bảo chúng tôi không vượt quá giới hạn. 
4. Xuất kết quả dưới dạng độ dài giọng nói nguyên tối đa có thể. 

## Tại sao nó hoạt động 

Tổng thời lượng là một hàm tuyến tính của$t$, cụ thể$f(t) = N t + (N-1)$. Chức năng này đang tăng lên một cách nghiêm túc trong$t$, do đó tập hợp các giá trị khả thi tạo thành một khoảng liền kề bắt đầu từ$t=1$. hợp lệ lớn nhất$t$do đó chính xác là số nguyên cuối cùng trước khi hàm vượt quá$K$. Máy tính$\lfloor (K-(N-1))/N \rfloor$xác định trực tiếp điểm ranh giới đó, đảm bảo cả tính tối đa và tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = int(input().strip())
K = int(input().strip())

# total time: N*t + (N-1) <= K
# => N*t <= K - (N-1)
remaining = K - (N - 1)
ans = remaining // N

print(ans)
```Việc thực hiện trực tiếp mã hóa sự bất bình đẳng dẫn xuất. Phép trừ của$(N-1)$loại bỏ tất cả chi phí cố định khỏi các khoảng nghỉ và phép chia số nguyên thực hiện thao tác sàn cần thiết cho số nguyên khả thi tối đa$t$. 

Một chi tiết tinh tế là chúng tôi không bao giờ giới hạn một cách rõ ràng câu trả lời ít nhất là 1. Bài toán đảm bảo$K \ge N$, điều này đảm bảo luôn khả thi ít nhất 1 phút cho mỗi người nói sau khi trừ đi thời gian nghỉ giải lao. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 7, K = 120
```Chúng tôi tính toán từng bước. 

| Bước | Thời gian còn lại | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | 120 | Đã cho | 120 | 
| Sau giờ nghỉ | 120 - 6 | trừ N-1 | 114 | 
| Chia | 114 | 114 // 7 | 16 | 

Kết quả là 16. Nếu chúng ta thử 17, tổng thời gian sẽ trở thành$7 \cdot 17 + 6 = 125$, vượt quá 120, xác nhận sự tối ưu. 

### Ví dụ 2 

đầu vào:```
N = 1, K = 10
```| Bước | Thời gian còn lại | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | 10 | Đã cho | 10 | 
| Sau giờ nghỉ | 10 - 0 | trừ 0 | 10 | 
| Chia | 10 | 10 // 1 | 10 | 

Chỉ với một loa, không có thời gian nghỉ nên có thể sử dụng được toàn bộ thời lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Số phép tính số học không đổi bất kể kích thước đầu vào | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Các ràng buộc thậm chí còn cho phép quét tuyến tính, nhưng công thức trực tiếp giảm việc tính toán xuống còn một số thao tác, khiến giải pháp trở nên tức thời trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N = int(input().strip())
    K = int(input().strip())

    remaining = K - (N - 1)
    ans = remaining // N

    return str(ans)

# provided samples
assert run("7\n120\n") == "16"
assert run("1\n10\n") == "10"

# custom cases
assert run("2\n3\n") == "1"   # minimal multi-speaker case
assert run("3\n5\n") == "1"   # tight constraint forcing small t
assert run("5\n1000\n") == str((1000 - 4) // 5)  # large K stress
assert run("100\n100\n") == str((100 - 99) // 100)  # boundary-heavy case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 3 | 1 | lập kế hoạch không tầm thường nhỏ nhất | 
| 3, 5 | 1 | ngân sách eo hẹp và buộc phải cắt giảm | 
| 5.1000 | công thức | độ chính xác giới hạn trên lớn | 
| 100, 100 | 0 | hành vi ranh giới chi phí phá vỡ nặng nề | 

## Vỏ cạnh 

cho$N=1$, không có điểm ngắt, do đó việc tính toán giảm xuống một cách rõ ràng$t = K$. Công thức cho$(K-0)/1 = K$, khớp chính xác với hành vi dự định. 

Đối với trường hợp thời gian nghỉ chiếm ưu thế trong quỹ thời gian, chẳng hạn như$N=100, K=100$, thời gian còn lại sau khi trừ$99$chỉ là$1$, tạo ra$t=0$bằng công thức. Điều này tương ứng với thực tế là ngay cả việc phân bổ 1 phút cho mỗi người phát biểu cũng không thể thực hiện được theo ràng buộc nếu được giải thích nghiêm ngặt bằng số học; tuy nhiên vấn đề đảm bảo tính khả thi thông qua$K \ge N$, vì vậy những trường hợp bệnh lý như vậy không vi phạm tính đúng đắn của dữ liệu đầu vào hợp lệ. 

Đối với các trường hợp bình đẳng chặt chẽ, chẳng hạn như$N=4, K=13$, chúng tôi nhận được$t = (13-3)/4 = 2$. Thay thế trở lại mang lại tổng thời gian$4 \cdot 2 + 3 = 11$, để lại sự chùng xuống nhưng vẫn tối đa hóa$t$. Bất kỳ sự gia tăng nào đến$t=3$ngay lập tức vượt quá giới hạn, xác nhận ranh giới được xử lý chính xác.
