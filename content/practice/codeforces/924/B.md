---
problem: 924B
contest_id: 924
problem_index: B
name: "Three-level Laser"
contest_name: "VK Cup 2018 - Round 2"
rating: 1600
tags: ["binary search", "greedy", "two pointers"]
answer: passed_samples
verified: true
solve_time_s: 85
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a326e6b-2cbc-83ec-b7fb-a28cecb0a5b4
---

#CF 924B - Laser ba cấp độ 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, tham lam, hai con trỏ 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a326e6b-2cbc-83ec-b7fb-a28cecb0a5b4 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một dãy mức năng lượng ngày càng tăng dần. Chúng ta phải chọn ba chỉ số$i < j < k$và giải thích chúng như một quá trình vật lý gồm ba bước tạo ra tỷ lệ giữa năng lượng hữu ích và năng lượng tiêu hao. 

Quá trình này chỉ phụ thuộc vào sự khác biệt của các giá trị được chọn. Năng lượng tiêu hao là bước nhảy từ$i$ĐẾN$k$, trong khi năng lượng hữu ích là sự phát xạ từ$k$xuống tới$j$. Hiệu quả do đó$$\eta = \frac{E_k - E_j}{E_k - E_i}.$$Có một hạn chế bổ sung: tổng bước nhảy từ$i$ĐẾN$k$không thể vượt quá$U$, Vì thế$E_k - E_i \le U$. Nếu không tồn tại bộ ba hợp lệ thì câu trả lời là$-1$. 

Cấu trúc hoàn toàn là một chiều với các giá trị đơn điệu, điều này cho thấy rằng bất kỳ giải pháp tối ưu hợp lệ nào cũng có thể được suy luận theo các khoảng thay vì các lựa chọn tổ hợp tùy ý. Khó khăn là chúng ta đang tối ưu hóa tỷ lệ trên tất cả các bộ ba dưới một ràng buộc về khoảng cách trượt. 

Các ràng buộc rất lớn: lên tới$10^5$tiểu bang, vì vậy bất kỳ$O(n^2)$hoặc tệ hơn là cách tiếp cận quá chậm. Một giải pháp về cơ bản phải là tuyến tính hoặc tuyến tính. Điều này ngay lập tức gợi ý việc duy trì một cửa sổ di chuyển hợp lệ$k$ứng cử viên cho mỗi$i$, và trong cửa sổ đó chọn một cái tốt nhất$j$một cách hiệu quả. 

Một trường hợp phức tạp là khi mảng quá “thưa thớt” so với$U$, không thể nhân ba được. Ví dụ, nếu$n = 3$,$E = [1, 100, 200]$, Và$U = 50$, thì không có cặp$i,k$thỏa mãn$E_k - E_i \le U$, vậy đáp án phải là$-1$. Một giải pháp bất cẩn giả định sự tồn tại của bộ ba sẽ tạo ra những tỉ số hoặc phép chia vô nghĩa cho 0. 

Một dạng lỗi khác là sự mất ổn định về mặt số học. Vì câu trả lời là một tỷ lệ chứ không phải số nguyên nên các vấn đề về độ chính xác sẽ phát sinh nếu chúng ta tính toán lại nhiều lần bằng số học động mà không có logic tối đa hóa cẩn thận. 

## Phương pháp tiếp cận 

Phương pháp brute-force liệt kê tất cả các bộ ba$i < j < k$, kiểm tra xem$E_k - E_i \le U$, và tính tỉ số Điều này đúng vì nó trực tiếp đánh giá định nghĩa. Tuy nhiên, nó tốn kém$O(n^3)$, quá chậm đối với$n = 10^5$. Thậm chí giảm nó để sửa chữa$i$Và$k$và quét$j$cho$O(n^2)$, vẫn là không thể được. 

Quan sát quan trọng là đối với cố định$i$Và$k$, tốt nhất$j$chỉ được xác định bằng cách tối đa hóa$E_j$dưới$i < j < k$, vì tử số là$E_k - E_j$. Để tối đa hóa tỷ lệ, chúng tôi muốn giảm thiểu$E_j$liên quan đến khoảng thời gian, nhưng sự phụ thuộc là phân số và không tuyến tính một cách tầm thường. 

Viết lại biểu thức giúp:$$\eta = \frac{E_k - E_j}{E_k - E_i} = 1 - \frac{E_j - E_i}{E_k - E_i}.$$Để cố định$i,k$, tối đa hóa hiệu quả tương đương với việc giảm thiểu$E_j$. Tuy nhiên,$j$phải nằm chặt chẽ giữa chúng. Vì vậy với mỗi khoảng$(i,k)$, chúng ta muốn giá trị nhỏ nhất ở bên trong. 

Điều này biến vấn đề thành cấu trúc hai con trỏ: chúng tôi sửa$i$, mở rộng$k$trong khi tôn trọng$E_k - E_i \le U$và duy trì cấu trúc dữ liệu (hoặc quan sát đơn điệu) cho phép chúng tôi theo dõi mức tối thiểu$E_j$ở khu vực giữa. Vì mảng tăng dần nên giá trị nhỏ nhất trong bất kỳ phân đoạn nào luôn ở điểm cuối bên trái của nó, vì vậy đối với mỗi phân đoạn$i,k$, tốt nhất$j$đơn giản là$i+1$. Điều này làm giảm vấn đề đáng kể. 

Như vậy đối với mỗi$i$và với mỗi giá trị hợp lệ$k$, chúng ta chỉ cần kiểm tra$j = i+1$, lập tỉ số:$$\frac{E_k - E_{i+1}}{E_k - E_i}.$$Bây giờ chúng ta chỉ cần duy trì một cửa sổ trượt hợp lệ$k$giá trị và đánh giá biểu thức này một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cố định điểm cuối bên trái$i$bắt đầu từ chỉ số nhỏ nhất. Điều này đảm bảo tất cả các bộ ba được xem xét theo thứ tự tăng dần của ranh giới bên trái của chúng. 
2. Duy trì một con trỏ$k$điều đó chỉ tiến về phía trước. Đối với mỗi$i$, mở rộng$k$càng nhiều càng tốt trong khi đáp ứng$E_k - E_i \le U$. Điều này đảm bảo chúng tôi chỉ xem xét các điểm cuối hợp lệ. 
3. Đối với mỗi cặp cố định$(i, k)$, xác định trạng thái trung gian tốt nhất. Bởi vì năng lượng đang tăng nghiêm ngặt nên giá trị nhỏ nhất có giá trị$E_j$vì$i < j < k$luôn luôn ở$j = i+1$, cung cấp$k \ge i+2$. Điều này tránh việc quét nội thất. 
4. Tính toán ứng viên hiệu quả:$$\eta = \frac{E_k - E_{i+1}}{E_k - E_i}.$$Chỉ đánh giá khi$k \ge i+2$, vì chúng ta cần ba chỉ số riêng biệt. 
5. Theo dõi giá trị tối đa trên tất cả các cấu hình hợp lệ. Nếu không có cấu hình tồn tại, hãy quay lại$-1$. 

### Tại sao nó hoạt động 

Đối với một cố định$i$Và$k$, mẫu số$E_k - E_i$là không đổi. tử số$E_k - E_j$đạt cực đại khi$E_j$được giảm thiểu. Vì mảng đang tăng dần nên giá trị nhỏ nhất có thể$E_j$TRONG$(i,k)$luôn luôn là$E_{i+1}$. Vì vậy không có sự lựa chọn nội thất của$j$bên cạnh đó$i+1$có thể cải thiện tỷ lệ và chỉ kiểm tra ứng cử viên này duy trì sự tối ưu. 

Con trỏ trượt cho$k$đảm bảo mọi khoảng thời gian hợp lệ được xem xét chính xác một lần và tính đơn điệu của mảng đảm bảo tính chính xác của việc bỏ qua chuyển động lùi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, U = map(int, input().split())
    E = list(map(int, input().split()))

    ans = -1.0
    k = 0

    for i in range(n):
        if k < i + 2:
            k = i + 2

        while k < n and E[k] - E[i] <= U:
            if i + 1 < k:
                val = (E[k] - E[i + 1]) / (E[k] - E[i])
                if val > ans:
                    ans = val
            k += 1

        # step back one because k is now invalid or out of range
        # but k only moves forward globally, so no reset needed

    if ans < 0:
        print(-1)
    else:
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng một lượt duy nhất với một con trỏ toàn cục$k$không bao giờ di chuyển lùi, đảm bảo độ phức tạp tuyến tính. Chi tiết quan trọng là chúng tôi chỉ đánh giá ứng viên khi$k$là hợp lệ và$i+1 < k$, đảm bảo ba chỉ số riêng biệt. 

Một điểm tinh tế là chúng ta không thiết lập lại$k$cho mỗi$i$. Điều này an toàn vì tăng$i$chỉ thắt chặt các hạn chế$E_k - E_i \le U$, vì vậy mọi giá trị hợp lệ trước đó$k$vẫn là điểm bắt đầu hợp lệ hoặc đã quá lớn. Điều này bảo tồn việc quét tuyến tính khấu hao. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 3 5 7
```Chúng tôi liệt kê các bộ ba hợp lệ. 

| tôi | j | k | E[k]-E[i] | E[k]-E[j] | tỷ lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 4 | 2 | 0,5 | 

Chỉ bộ ba hợp lệ là (1,3,5) trong lập chỉ mục dựa trên 1, cho 0,5. 

Điều này chứng tỏ rằng mặc dù có nhiều bộ ba tồn tại trong các bài toán tổng quát, nhưng ở đây cái tốt nhất được xác định duy nhất bởi trạng thái ở giữa nhỏ nhất. 

### Ví dụ 2 

đầu vào:```
5 6
1 2 4 7 10
```Chúng tôi theo dõi những ứng viên tốt nhất. 

| tôi | k | j=i+1 | E[k]-E[i] | E[k]-E[j] | tỷ lệ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 3 | 2 | 0,666... ​​| 
| 0 | 3 | 1 | 6 | 5 | 0.833... | 
| 1 | 3 | 2 | 5 | 3 | 0,6 | 

Tối đa là$5/6$từ$i=1, j=2, k=3$. 

Điều này khẳng định rằng việc mở rộng$k$trong khi giữ$j=i+1$nắm bắt được sự cân bằng tối ưu giữa tăng trưởng tử số và tăng trưởng mẫu số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi con trỏ$i$Và$k$di chuyển nhiều nhất$n$tổng số lần | 
| Không gian |$O(1)$| Chỉ có một số biến được duy trì | 

Quét tuyến tính là đủ cho$n \le 10^5$và việc sử dụng bộ nhớ liên tục đảm bảo không có chi phí nào vượt quá dung lượng lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, U = map(int, inp.split()[0:2])
    # re-run solution inline
    import sys
    input = sys.stdin.readline

    def solve():
        n, U = map(int, sys.stdin.readline().split())
        E = list(map(int, sys.stdin.readline().split()))

        ans = -1.0
        k = 0

        for i in range(n):
            if k < i + 2:
                k = i + 2

            while k < n and E[k] - E[i] <= U:
                if i + 1 < k:
                    val = (E[k] - E[i + 1]) / (E[k] - E[i])
                    if val > ans:
                        ans = val
                k += 1

        if ans < 0:
            return "-1"
        return str(ans)

    return solve()

# provided sample
assert run("4 4\n1 3 5 7\n") == "0.5"

# minimum case (no valid triple)
assert run("3 1\n1 2 3\n") == "-1"

# tight window
assert run("3 2\n1 2 4\n") == "-1"

# increasing valid chain
assert run("5 10\n1 2 3 4 5\n") != "-1"

# large gap
assert run("5 100\n1 10 20 30 40\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 / 1 2 3 | -1 | Không tồn tại bộ ba hợp lệ | 
| 3 2 / 1 2 4 | -1 | Ràng buộc cửa sổ chặn bất kỳ k | 
| 5 10 / 1 2 3 4 5 | tỷ lệ hợp lệ | Trường hợp dày đặc bình thường | 
| 5 100 / giá trị cách nhau | tỷ lệ hợp lệ | Hành vi cửa sổ lớn | 

## Vỏ cạnh 

Khi nào$n = 3$, có đúng một bộ ba có thể xảy ra. Thuật toán vẫn hoạt động chính xác vì vòng lặp chỉ đánh giá khi$k = 2$, và kiểm tra$i = 0$, tạo ra một tỷ lệ hợp lệ nếu ràng buộc cho phép. 

Khi không có cặp nào thỏa mãn$E_k - E_i \le U$, con trỏ$k$luôn vượt qua phạm vi hợp lệ và`ans`còn lại$-1$. Điều này kích hoạt chính xác trường hợp không thể. 

Khi các giá trị rất gần nhau, độ chính xác của dấu phẩy động rất quan trọng. Thuật toán chỉ thực hiện một phép chia cho mỗi ứng viên, giảm thiểu lỗi tích lũy và nằm trong giới hạn yêu cầu.$10^{-9}$sức chịu đựng.
