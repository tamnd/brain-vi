---
problem: 1353C
contest_id: 1353
problem_index: C
name: "Board Moves"
contest_name: "Codeforces Round 642 (Div. 3)"
rating: 1000
tags: ["math"]
answer: passed_samples
verified: true
solve_time_s: 264
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e2ede-ec38-83ec-be81-dbb97ec798c5
---

#CF 1353C - Di chuyển ván cờ 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 4 phút 24s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e2ede-ec38-83ec-be81-dbb97ec798c5 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới nơi mỗi ô bắt đầu bằng chính xác một mã thông báo. Trong một lần di chuyển, chúng tôi chọn một mã thông báo và di chuyển nó đến bất kỳ ô nào trong số tám ô lân cận, bao gồm cả các đường chéo. Các mã thông báo có thể xếp chồng lên nhau và không có hạn chế về số lượng mã thông báo nằm trong cùng một ô. Mục tiêu là đưa tất cả các mã thông báo vào một ô duy nhất đồng thời giảm thiểu tổng số lần di chuyển. 

Vì ban đầu mỗi ô chứa một mã thông báo nên chúng ta có thể coi nhiệm vụ này là chọn một ô “mục tiêu” cuối cùng và di chuyển mọi mã thông báo khác vào đó. Mỗi mã thông báo đóng góp một khoản chi phí bằng khoảng cách đường đi ngắn nhất tới mục tiêu đã chọn theo chuyển động 8 hướng. 

Cấu trúc ẩn chính là chuyển động 8 hướng tương ứng với khoảng cách Chebyshev. Đối với một tế bào$(i, j)$và mục tiêu$(x, y)$, khoảng cách là:$$\max(|i-x|, |j-y|)$$Vì vậy, vấn đề trở thành: chọn một ô trung tâm giảm thiểu tổng khoảng cách Chebyshev từ tất cả các ô lưới. 

Ràng buộc$n \le 5 \cdot 10^5$với tổng các bài kiểm tra bị giới hạn có nghĩa là chúng ta không thể mô phỏng các hoạt động của lưới hoặc lặp lại trên tất cả$n^2$tế bào. Mọi giải pháp đều phải giảm lưới thành công thức tính tổng 1D trong$O(1)$hoặc$O(n)$mỗi bài kiểm tra. 

Một trường hợp cạnh tinh tế là$n = 1$. Lưới đã có tất cả mã thông báo trong một ô nên câu trả lời là 0. Bất kỳ công thức nào cũng phải xử lý việc này một cách tự nhiên mà không cần sử dụng vỏ đặc biệt. 

Một hành vi cạnh tiềm ẩn khác xuất hiện khi suy luận về tính đối xứng. Bởi vì$n$thật kỳ lạ, có một ô trung tâm duy nhất. Giải pháp đúng nào cũng phải ngầm sử dụng thực tế này; mặt khác, khoảng cách trung bình giữa nhiều trung tâm sẽ dẫn đến việc tính toán kép hoặc suy luận phân số sai. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi ô mục tiêu có thể$(x, y)$, tính khoảng cách từ mỗi ô đến ô đó và tính tổng tất cả các đóng góp. Mỗi chi phí đánh giá$O(n^2)$, và có$O(n^2)$mục tiêu, dẫn đến$O(n^4)$, điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 200$. 

Chúng ta có thể giảm đáng kể điều này bằng cách tách tọa độ. Khoảng cách Chebyshev$\max(|i-x|, |j-y|)$khó xử lý trong 2D, nhưng tính đối xứng giúp ích: thay vì tính tổng trực tiếp tất cả các ô, chúng tôi quan sát thấy rằng mỗi lớp xung quanh tâm đều đóng góp đồng đều. 

Hãy coi bảng như những “vòng” hình vuông đồng tâm xung quanh tâm. Tất cả các ô ở khoảng cách$k$tạo thành đường viền của một hình vuông cạnh$2k+1$. Mỗi mã thông báo trong vòng đó phải di chuyển ít nhất$k$các bước để đến trung tâm, và chính xác$k$là có thể đạt được. Vì vậy, mỗi lớp đóng góp:$$(\text{number of cells in ring}) \cdot k$$Đối với lưới có kích thước$n$, tâm ở khoảng cách$0$, và có các lớp từ$1$ĐẾN$\frac{n-1}{2}$. các$k$-lớp thứ đóng góp vào chu vi của hình vuông ở lớp đó. Việc đếm chu vi đơn giản hóa thành$8k$, do đó tổng chi phí trở thành:$$\sum_{k=1}^{(n-1)/2} 8k^2$$Điều này biến bài toán thành một tổng bình phương dạng đóng, loại bỏ mọi sự phụ thuộc vào$n^2$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^4)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng tất cả các mã thông báo di chuyển độc lập về phía ô trung tâm đã chọn, do đó tổng chi phí là tổng khoảng cách đường đi ngắn nhất từ mỗi ô đến trung tâm. Điều này cho phép chúng ta xử lý từng ô riêng biệt thay vì mô phỏng các bước di chuyển. 
2. Xác định chuyển động theo 8 hướng làm cho khoảng cách bằng khoảng cách Chebyshev. Điều này thay thế mô phỏng đường dẫn bằng cách diễn giải hình học đơn giản dựa trên các lớp hình vuông. 
3. Lưu ý rằng mục tiêu tối ưu là ô trung tâm duy nhất$(\frac{n}{2}, \frac{n}{2})$, vì tính đối xứng đảm bảo bất kỳ độ lệch nào cũng làm tăng tổng khoảng cách cho ít nhất một vùng đối xứng. 
4. Phân chia lưới thành các vòng vuông đồng tâm xung quanh tâm. Vòng ngoài cùng có bán kính$\frac{n-1}{2}$, và mỗi vòng$k$chứa tất cả các ô có khoảng cách tọa độ tối đa từ tâm chính xác$k$. 
5. Tính xem có bao nhiêu ô nằm chính xác trên vòng$k$. Hình vuông cạnh$2k+1$trừ bình phương cạnh$2k-1$mang lại:$$(2k+1)^2 - (2k-1)^2 = 8k$$Vậy mỗi vòng chứa$8k$tế bào. 

1. Mỗi cái đó$8k$mã thông báo phải di chuyển ít nhất$k$bước vào trong và ranh giới này chặt chẽ vì các đường dẫn luôn có thể được chọn một cách đơn điệu về phía trung tâm. 
2. Tổng các khoản đóng góp trên tất cả các vòng từ$k = 1$ĐẾN$k = \frac{n-1}{2}$, nhân số lượng theo khoảng cách cho mỗi lớp. 
3. Đánh giá biểu mẫu đóng kết quả một cách hiệu quả trên mỗi trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Mỗi ô thuộc về chính xác một lớp khoảng cách Chebyshev tính từ trung tâm và mỗi lớp đóng góp độc lập vào tổng chi phí. Việc phân tách thành các vòng rời rạc đảm bảo không tính hai lần và khoảng cách giới hạn dưới$k$cho chiếc nhẫn$k$chặt chẽ vì chuyển động chéo cho phép giảm đồng thời ở cả hai tọa độ. Điều này đảm bảo rằng chi phí của lớp tổng hợp khớp chính xác với tổng số lần di chuyển tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        k = (n - 1) // 2
        # sum_{i=1..k} 8*i*i
        # = 8 * sum i^2
        s = k * (k + 1) * (2 * k + 1) // 6
        print(8 * s)

if __name__ == "__main__":
    solve()
```Giải pháp làm giảm vấn đề lưới thành một biểu thức số học duy nhất. Biến$k$đại diện cho bán kính Chebyshev tối đa tính từ tâm. Công thức tổng bình phương được sử dụng trực tiếp để tránh lặp lại. 

Một lỗi phổ biến là tính tổng$8k$thay vì$8k^2$. Thuật ngữ đầu tiên đếm các ô trên mỗi lớp, nhưng mỗi ô cũng phải chịu chi phí di chuyển là$k$, trong đó giới thiệu yếu tố thứ hai của$k$. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 1$| Bước | k | Tổng hợp | Trả lời | 
| --- | --- | --- | --- | 
| ban đầu | 0 | 0 | 0 | 

Chỉ có một ô tồn tại, đã ở trung tâm. Không có chuyển động nào xảy ra, xác nhận trường hợp cơ bản. 

### Ví dụ 2:$n = 5$| lớp k | tế bào$8k$| chi phí cho mỗi tế bào$k$| đóng góp | 
| --- | --- | --- | --- | 
| 1 | 8 | 1 | 8 | 
| 2 | 16 | 2 | 32 | 

Tổng cộng = 40 

Điều này xác nhận rằng các lớp bên ngoài chi phối chi phí theo phương pháp bậc hai và công thức phù hợp với bảng liệt kê rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi bài kiểm tra | Mỗi bài kiểm tra sử dụng biểu thức số học dạng đóng | 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Giải pháp dễ dàng xử lý kích thước đầu vào tối đa vì nó thực hiện số học theo thời gian không đổi cho mỗi trường hợp thử nghiệm mà không phụ thuộc vào$n^2$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        k = (n - 1) // 2
        s = k * (k + 1) * (2 * k + 1) // 6
        out.append(str(8 * s))
    return "\n".join(out)

# provided samples
assert run("3\n1\n5\n499993\n") == "0\n40\n41664916690999888"

# custom cases
assert run("1\n3\n") == "8", "smallest non-trivial grid"
assert run("1\n7\n") == str(8 * ((3 * 4 * 7) // 6)), "larger symmetry check"
assert run("1\n9\n") == str(8 * ((4 * 5 * 9) // 6)), "odd growth check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 | 8 | cấu trúc vòng không tầm thường nhỏ nhất | 
| 7 | tính toán | tính đúng đắn của tích lũy hai lớp | 
| 9 | tính toán | mô hình tăng trưởng bậc hai | 

## Vỏ cạnh 

cho$n = 1$, chúng tôi có$k = 0$, do đó tổng trống và kết quả bằng 0. Công thức tự nhiên trả về 0 mà không cần xử lý đặc biệt. 

Đối với rất lớn$n$, chẳng hạn như$n = 499993$, giá trị của$k$trở nên lớn và các tích trung gian trong tổng bình phương vượt quá giới hạn 32 bit. Việc sử dụng số nguyên Python sẽ tránh tràn và biểu thức dạng đóng đảm bảo tính toán theo thời gian không đổi. 

Đối với kích thước lẻ tối thiểu như$n = 3$, có đúng một chiếc nhẫn. Công thức giảm xuống còn$8 \cdot 1^2 = 8$, phù hợp với thực tế là tất cả 8 ô bên ngoài đều cách tâm 1 khoảng cách.
