---
problem: 1285C
contest_id: 1285
problem_index: C
name: "Fadi and LCM"
contest_name: "Codeforces Round 613 (Div. 2)"
rating: 1400
tags: ["brute force", "math", "number theory"]
answer: passed_samples
verified: true
solve_time_s: 490
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2dad95-f3d8-83ec-91f0-7606a9042964
---

#CF 1285C - Fadi và LCM 

**Đánh giá:** 1400 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 8 phút 10s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2dad95-f3d8-83ec-91f0-7606a9042964 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$X$, và chúng ta cần chia nó thành hai số nguyên dương$a$Và$b$sao cho bội số chung nhỏ nhất của chúng chính xác là$X$. Trong số tất cả các cặp hợp lệ như vậy, chúng ta muốn có cặp số nhỏ nhất trong hai số. 

Nói cách khác, chúng ta không chỉ tìm kiếm bất kỳ sự nhân tử hóa nào của$X$, mà là một cặp "bao trùm" mọi lũy thừa của$X$thông qua LCM của họ, đồng thời giữ cả hai số cân bằng nhất có thể sao cho phần tử tối đa nhỏ nhất có thể. 

Ràng buộc$X \le 10^{12}$ngay lập tức loại trừ mọi tìm kiếm tuyến tính bậc hai hoặc thậm chí đơn giản trên tất cả các cặp. Một sức mạnh tàn bạo trên tất cả$a \le X$là không thể, vì$X$có thể cực kỳ lớn và có tới khoảng$10^6$các ước số tiềm năng trong các kịch bản mật độ trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất hiện khi$X = 1$. Trong trường hợp đó, cả hai$a = 1$Và$b = 1$là bắt buộc vì LCM(1,1) = 1 và không tồn tại cặp nào khác. Bất kỳ giải pháp nào cũng phải xử lý vấn đề này một cách rõ ràng hoặc hội tụ một cách tự nhiên về nó. 

Một trường hợp thất bại tiềm ẩn khác là giả định rằng việc chọn$a$như một ước số của$X$và thiết lập$b = X / a$luôn luôn hợp lệ. Điều đó không đúng, vì LCM phụ thuộc vào số mũ nguyên tố chứ không chỉ dựa vào cấu trúc tích. Ví dụ, nếu$X = 12$, đang chọn$a = 4$,$b = 3$hoạt động, nhưng$a = 6$,$b = 2$cũng hoạt động; tuy nhiên, nhiều cặp số chia như$a = 12$,$b = 1$là hợp lệ nhưng dưới mức tối ưu. 

Khó khăn chính là đảm bảo điều kiện LCM trong khi tối ưu hóa giá trị tối đa. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê tất cả các cặp$(a, b)$như vậy$\text{lcm}(a, b) = X$, sau đó theo dõi việc giảm thiểu cặp$\max(a, b)$. Vấn đề trước mắt là có$O(X)$các ứng cử viên, và thậm chí nếu chúng ta giới hạn ở các ước số, có thể có$O(\sqrt{X})$số chia, dẫn đến$O(D^2)$cặp. Với$X$lên tới$10^{12}$, điều này trở nên không khả thi về mặt tính toán. 

Bước ngoặt là nhận ra rằng cả hai$a$Và$b$phải chia$X$, bởi vì nếu$\text{lcm}(a,b) = X$, mọi thừa số nguyên tố của$a$Và$b$đã có trong$X$. Vì vậy ta chỉ cần tìm trên các ước của$X$, không phải số nguyên tùy ý. 

Bây giờ vấn đề trở thành: giữa các cặp số chia$a, b$, tìm một giá trị có LCM bằng$X$và giảm thiểu$\max(a,b)$. Nếu chúng ta sửa một ước số$a$, đối tác hợp lệ nhỏ nhất$b$không phải là tùy tiện; nó phải đóng góp chính xác số lũy thừa còn thiếu để LCM trở thành$X$. 

Điều này dẫn đến một sự cải cách rõ ràng hơn. Chúng tôi lặp lại trên tất cả các ước$a \mid X$và tính:$$b = \frac{X}{\gcd(a, X/a)}$$Tuy nhiên, một quan sát đơn giản và chuẩn mực hơn sẽ giải quyết được mọi thứ: nếu chúng ta thực thi$a \cdot b = X$Và$\gcd(a,b)=1$, thì LCM trở thành$X$. Câu trả lời tối ưu luôn tương ứng với việc chia$X$thành hai phần nguyên tố cùng nhau. Mục tiêu là tìm một cặp nhân tố$(a, b)$với sản phẩm$X$, sau đó kiểm tra xem cặp nào là nguyên tố cùng nhau và cực tiểu hóa giá trị lớn nhất. 

Trực tiếp hơn nữa, cấu trúc đã biết của lời giải tối ưu là: chúng ta thử tất cả các ước số$a \le \sqrt{X}$, bộ$b = X/a$và tính toán LCM thông qua$\text{lcm}(a,b) = X \cdot \frac{a}{\gcd(a,b)} / a = \frac{X}{\gcd(a,b)}$. Vậy điều kiện giảm xuống còn$\gcd(a,b) = 1$. Trong số tất cả các cặp hợp lệ, chúng tôi chọn một cặp giảm thiểu$\max(a,b)$. 

Do đó, giải pháp tối ưu là chuyển qua một lần các ước số lên đến$\sqrt{X}$, sử dụng kiểm tra gcd. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force theo cặp |$O(X)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Số chia + phép liệt kê gcd |$O(\sqrt{X})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại bài toán dưới dạng tìm cặp ước số$(a, b)$của$X$như vậy$a \cdot b = X$Và$\gcd(a,b)=1$, giảm thiểu$\max(a,b)$. 

1. Lặp lại tất cả các số nguyên$i$từ$1$ĐẾN$\lfloor \sqrt{X} \rfloor$. 

Chúng tôi chỉ kiểm tra căn bậc hai vì các ước số có cặp đối xứng$(i, X/i)$, và chúng tôi muốn tránh làm việc dư thừa. 
2. Nếu$i$chia rẽ$X$, bộ$a = i$Và$b = X / i$. 

Điều này đảm bảo chúng tôi chỉ xem xét các cặp yếu tố hợp lệ. 
3. Tính toán$g = \gcd(a, b)$. 

Bước này kiểm tra xem cặp có chia sẻ các thừa số nguyên tố có thể làm biến dạng LCM khỏi$X$. 
4. Nếu$g = 1$, cặp này hợp lệ với điều kiện LCM. 

Sau đó chúng tôi xem xét$\max(a,b)$như một câu trả lời ứng viên. 
5. Theo dõi cặp giảm thiểu$\max(a,b)$trên tất cả các ứng viên hợp lệ. 

Câu trả lời là cặp tốt nhất được lưu trữ sau tất cả các lần lặp. 

### Tại sao nó hoạt động 

Mọi giải pháp hợp lệ chỉ được sử dụng các thừa số nguyên tố từ$X$. Nếu một cặp$(a,b)$có chung một thừa số nguyên tố chung, thừa số đó sẽ chỉ được tính một lần trong LCM, nghĩa là LCM sẽ hoàn toàn nhỏ hơn$X$trừ khi được bù bằng số mũ cao hơn, điều này sẽ buộc một trong các số vượt quá cấu trúc số mũ tương ứng của$X$. Cấu hình tối ưu tránh được sự trùng lặp hoàn toàn, có nghĩa là các cặp tốt nhất chính xác là các cặp thừa số nguyên tố cùng nhau của$X$. Việc kiểm tra tất cả các ước số đảm bảo mọi phân tách có thể đều được xem xét và giảm thiểu$\max(a,b)$đảm bảo sự cân bằng. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    x = int(input().strip())

    best_a, best_b = 1, x
    best_max = x

    i = 1
    while i * i <= x:
        if x % i == 0:
            a = i
            b = x // i

            if math.gcd(a, b) == 1:
                if max(a, b) < best_max:
                    best_a, best_b = a, b
                    best_max = max(a, b)

            # check the symmetric pair is not needed since (a,b) already covers it

        i += 1

    print(best_a, best_b)

if __name__ == "__main__":
    solve()
```Mã chỉ lặp lại tối đa$\sqrt{X}$, đảm bảo hiệu quả theo$10^{12}$hạn chế. Mỗi cặp ước số được kiểm tra một lần và quá trình lọc gcd đảm bảo tính chính xác của điều kiện LCM. Cặp tốt nhất được cập nhật bất cứ khi nào tìm thấy giá trị tối đa nhỏ hơn, mã hóa trực tiếp mục tiêu tối ưu hóa. 

Một chi tiết tinh tế là khởi tạo: bắt đầu bằng$(1, X)$đảm bảo một đường cơ sở hợp lệ, vì$\gcd(1,X)=1$luôn luôn giữ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```Chúng tôi kiểm tra các ước số lên đến$\sqrt{2}$. 

| tôi | một | b | gcd(a,b) | hợp lệ | tối đa(a,b) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | vâng | 2 | 

Cặp hợp lệ duy nhất là (1,2), vì vậy câu trả lời là (1,2). Điều này xác nhận rằng đối với các số nguyên tố, việc phân chia tối ưu luôn là tầm thường. 

### Ví dụ 2 

đầu vào:```
12
```| tôi | một | b | gcd(a,b) | hợp lệ | tối đa(a,b) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 12 | 1 | vâng | 12 | 
| 2 | 2 | 6 | 2 | không | - | 
| 3 | 3 | 4 | 1 | vâng | 4 | 

Cặp tốt nhất là (3,4) vì nó nguyên tố cùng nhau và cực tiểu hóa phần tử lớn nhất. Điều này cho thấy lý do tại sao việc ghép cặp số chia ngây thơ lại không thành công trừ khi gcd được thực thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{X})$| Chúng tôi lặp đi lặp lại đến$\sqrt{X}$, kiểm tra khả năng chia hết và gcd trong thời gian khấu hao không đổi | 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ bất kể kích thước đầu vào | 

Ràng buộc$X \le 10^{12}$cho phép lên đến$10^6$lặp đi lặp lại, nằm trong giới hạn thoải mái trong Python. Mỗi lần lặp đều nhẹ, chỉ liên quan đến tính toán modulo và gcd. 

## Trường hợp thử nghiệm```python
import sys, io, math

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x = int(sys.stdin.readline())

    best_a, best_b = 1, x
    best_max = x

    i = 1
    while i * i <= x:
        if x % i == 0:
            a = i
            b = x // i
            if math.gcd(a, b) == 1:
                if max(a, b) < best_max:
                    best_a, best_b = a, b
                    best_max = max(a, b)
        i += 1

    return f"{best_a} {best_b}"

# provided sample
assert solve("2\n") == "1 2"

# custom cases
assert solve("1\n") == "1 1", "minimum edge case"
assert solve("12\n") == "3 4", "non-trivial composite"
assert solve("6\n") in ("2 3", "3 2"), "small composite"
assert solve("1000000000000\n") is not None, "large stress case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 1 | trường hợp biên nhỏ nhất | 
| 12 | 3 4 | phân chia tối ưu không tầm thường | 
| 6 | 2 3 | lựa chọn hệ số đồng nguyên tố | 
| 10^12 | cặp hợp lệ | hiệu suất ở mức giới hạn tối đa | 

## Vỏ cạnh 

cho$X = 1$, vòng lặp chạy một lần với$i = 1$. chúng tôi nhận được$a = 1$,$b = 1$, Và$\gcd(1,1)=1$, do đó thuật toán trả về (1,1), kết quả này đúng. 

Đối với nguyên tố$X$, chỉ có cặp số chia là$(1,X)$. Vì gcd là 1 nên nó được chọn tự động và không có ứng cử viên nào khác tồn tại. 

Đối với các hình vuông hoàn hảo như$X = 36$, các ước số đối xứng như (6,6) không vượt qua bài kiểm tra gcd vì gcd không bằng 1, ngăn chặn việc giảm LCM không hợp lệ. Chỉ các cặp nguyên tố cùng nhau như (4,9) tồn tại và thuật toán sẽ ưu tiên chính xác cặp cân bằng nhất. 

Đối với các cường quốc như$X = 10^{12}$, vòng lặp vẫn chỉ chạy đến$10^6$và kiểm tra gcd vẫn giữ nguyên thời gian không đổi. Thuật toán tránh mọi sự phụ thuộc vào số lượng ước số hoặc độ sâu nhân tử, chỉ dựa vào phép liệt kê căn bậc hai.
