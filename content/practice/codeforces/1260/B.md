---
problem: 1260B
contest_id: 1260
problem_index: B
name: "Obtain Two Zeroes"
contest_name: "Educational Codeforces Round 77 (Rated for Div. 2)"
rating: 1300
tags: ["binary search", "math"]
answer: passed_samples
verified: true
solve_time_s: 289
date: 2026-06-13
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d7f9c-3e08-83ec-a865-d8e20e6df5bb
---

# CF 1260B - Lấy hai số 0 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 4 phút 49 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d7f9c-3e08-83ec-a865-d8e20e6df5bb 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho hai chồng đơn vị, được mô tả bằng số nguyên$a$Và$b$. Trong một nước đi chúng ta được phép chọn một số nguyên dương$x$và áp dụng một trong hai phép biến đổi: hoặc chúng ta trừ$x$từ$a$Và$2x$từ$b$, hoặc chúng ta trừ$2x$từ$a$Và$x$từ$b$. Chúng ta có thể lặp lại những bước di chuyển này bao nhiêu lần, chọn các giá trị khác nhau của$x$mỗi lần. 

Câu hỏi đặt ra là liệu có thể xả hoàn toàn cả hai cọc về 0 cùng một lúc mà không bao giờ tạo ra giá trị âm hay không. 

Các ràng buộc nhỏ về số lượng ca kiểm thử, nhưng các giá trị của$a$Và$b$có thể lớn như$10^9$. Điều đó loại trừ bất kỳ mô phỏng nào cố gắng áp dụng các hoạt động từng bước, bởi vì mức độ giảm quá lớn và không gian lựa chọn cho$x$liên tục trên các số nguyên dương. Bất kỳ giải pháp đúng nào cũng phải giảm vấn đề xuống mức kiểm tra liên tục cho mỗi trường hợp thử nghiệm. 

Một vấn đề tế nhị nảy sinh từ việc tự do lựa chọn các giá trị khác nhau của$x$. Một trực giác ngây thơ có thể đề nghị trừ đi giá trị lớn hơn một cách tham lam trước hoặc luôn sử dụng$x=1$, nhưng những cách tiếp cận này không thành công vì các thao tác không phải là các bước đơn vị độc lập; mỗi thao tác kết hợp hai biến theo một tỷ lệ cố định, do đó, các quyết định tham lam cục bộ có thể phá hủy tính khả thi toàn cầu. Ví dụ, bắt đầu từ$(1,2)$, chỉ áp dụng một cách tham lam$(x,2x)$thao tác không thành công ngay lập tức mặc dù tồn tại một chuỗi hợp lệ bằng cách sử dụng cả hai loại thao tác. 

Khó khăn cốt lõi là chúng tôi không phân phối một số phép toán đơn vị cố định mà kết hợp hai phép biến đổi vectơ với tỷ lệ tùy ý. 

## Phương pháp tiếp cận 

Nếu bỏ qua tính hiệu quả, chúng ta có thể thử nghĩ đến việc áp dụng lặp đi lặp lại các phép toán và khám phá tất cả các trình tự có thể có. Mỗi thao tác làm giảm cặp$(a,b)$bởi một trong hai$x(1,2)$hoặc$x(2,1)$. Tìm kiếm brute-force sẽ phân nhánh theo các loại hoạt động và các giá trị có thể có của$x$, cố gắng tiếp cận$(0,0)$. Ngay cả khi chúng ta rời rạc hóa$x$lên đơn vị bước, không gian trạng thái tăng cực kỳ nhanh vì cả hai tọa độ có thể lớn bằng$10^9$. Điều này làm cho bất kỳ phương pháp dựa trên tìm kiếm nào đều không khả thi. 

Quan sát quan trọng là thứ tự thực hiện các thao tác không quan trọng. Chỉ có tổng đóng góp của từng loại hoạt động mới quan trọng. Giả sử chúng ta tổng hợp tất cả các hoạt động thuộc loại$(1,2)$thành tổng số tiền$p$và tất cả các hoạt động thuộc loại$(2,1)$thành tổng số tiền$q$. Sau đó, vấn đề sẽ trở thành việc giải một hệ thống tuyến tính trong đó những đóng góp tổng hợp này phải khớp chính xác$a$Và$b$. 

Điều này làm giảm vấn đề từ suy luận về chuỗi đến giải hai phương trình tuyến tính với các ràng buộc không âm. Khi chúng tôi làm điều đó, cấu trúc sẽ trở nên đủ cứng nhắc để có thể kiểm tra tính khả thi trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Giảm phương trình tuyến tính | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại các hoạt động theo tổng mức sử dụng tích lũy của từng loại hoạt động. Thay vì suy nghĩ về những bước đi riêng lẻ, chúng tôi nhóm tất cả$(1,2)$-loại giảm cùng nhau và tất cả$(2,1)$-loại giảm với nhau. 

1. Hãy để$p$thể hiện tổng số tiền đóng góp của các hoạt động thuộc loại$(1,2)$, Và$q$thể hiện tổng số tiền đóng góp của các hoạt động thuộc loại$(2,1)$. Điều này hợp lệ vì việc mở rộng quy mô hoạt động bằng cách chia tách$x$thành các phần nhỏ hơn không làm thay đổi hiệu ứng ròng. 
2. Thể hiện mức giảm cuối cùng trên$a$Và$b$về mặt$p$Và$q$. Tổng mức giảm trong$a$trở thành$p + 2q$, trong khi tổng mức giảm trong$b$trở thành$2p + q$. Để đạt đến số 0, chúng tôi yêu cầu hệ thống:$$p + 2q = a, \quad 2p + q = b$$3. Giải hệ này để tìm hiểu điều kiện khả thi. Loại bỏ các biến mang lại$3p = 2b - a$Và$3q = 2a - b$. Điều này ngay lập tức ngụ ý rằng cả hai biểu thức phải không âm và chia hết cho 3 để tồn tại các nghiệm số nguyên. 
4. Một điều kiện cần đơn giản hơn xuất hiện bằng cách cộng các phương trình:$3(p+q) = a + b$, Vì thế$a + b$phải chia hết cho 3. Điều kiện này đã hạn chế hầu hết các trường hợp bất khả thi. 
5. Các ràng buộc không âm chuyển thành tính khả thi về hình học: không tọa độ nào có thể quá nhỏ so với tọa độ kia. Cụ thể, người ta không bao giờ được yêu cầu giảm nhiều hơn từ một biến so với mức có thể được cung cấp bằng cách kết hợp cả hai loại hoạt động. 
6. Kết hợp các điều kiện này dẫn đến một kiểm tra rút gọn: tổng phải chia hết cho 3 và không được chia hết cho 3.$a$cũng không$b$có thể vượt quá hai lần khác. 

### Tại sao nó hoạt động 

Mỗi hoạt động làm giảm tổng$a + b$chính xác$3x$, do đó tổng số là bất biến modulo 3 trên tất cả các phép toán. Điều này làm cho việc chia hết cho 3 trở thành một yêu cầu khó khăn. Khi điều kiện đó được thỏa mãn, toàn bộ quá trình sẽ chuyển sang phân phối tổng mức giảm này giữa hai vectơ cố định$(1,2)$Và$(2,1)$. Các vectơ này tạo thành cơ sở hai chiều, do đó, mọi điểm cuối có thể truy cập đều tương ứng chính xác với sự kết hợp không âm của chúng. Các bất đẳng thức đảm bảo rằng sự kết hợp này không yêu cầu các hệ số âm, tương ứng với các phép toán đảo ngược, một điều không được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())

        if (a + b) % 3 != 0:
            print("NO")
            continue

        if max(a, b) > 2 * min(a, b):
            print("NO")
            continue

        print("YES")

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện các điều kiện khả thi xuất phát. Kiểm tra đầu tiên thực thi bất biến modulo-3 xuất phát từ thực tế là mọi thao tác đều loại bỏ chính xác$3x$tổng số đơn vị. Kiểm tra thứ hai đảm bảo rằng không có biến nào quá lớn so với biến kia, điều này sẽ khiến cho việc cân bằng mức giảm bằng cách sử dụng các tỷ lệ hoạt động được phép. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi logic trên hai trường hợp đại diện. 

### Ví dụ 1: (6, 9) 

| Bước | một | b | a+b mod 3 | điều kiện tối đa | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 6 | 9 | 0 | 9 12 | CÓ | 

Tổng chia hết cho 3 và không có giá trị nào vượt quá hệ số cho phép là 2. Điều này tương ứng với việc phân tách hợp lệ thành hai loại phép toán, vì vậy câu trả lời là CÓ. 

### Ví dụ 2: (1, 1) 

| Bước | một | b | a+b mod 3 | điều kiện tối đa | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 1 | 2 | 1 2 | KHÔNG | 

Mặc dù các giá trị nhỏ và cân bằng nhưng tổng tổng không chia hết cho 3. Vì mọi thao tác đều loại bỏ bội số của 3 khỏi tổng nên việc đạt đến 0 là không thể. 

Ví dụ thứ hai cho thấy chỉ sự cân bằng thôi thì chưa đủ; ràng buộc mô-đun là yếu tố hạn chế thực sự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm chỉ yêu cầu một số lần kiểm tra số học không đổi | 
| Không gian |$O(1)$| Không có bộ nhớ bổ sung nào ngoài các biến đầu vào được sử dụng | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì$t \le 100$và mỗi trường hợp được xử lý độc lập trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        if (a + b) % 3 != 0:
            out.append("NO")
        elif max(a, b) > 2 * min(a, b):
            out.append("NO")
        else:
            out.append("YES")
    return "\n".join(out) + "\n"

# provided samples
assert run("3\n6 9\n1 1\n1 2\n") == "YES\nNO\nYES\n"

# custom cases
assert run("1\n0 0\n") == "YES\n", "already zero"
assert run("1\n2 2\n") == "NO\n", "sum not divisible by 3"
assert run("1\n2 4\n") == "YES\n", "valid balanced pair"
assert run("1\n1 1000000000\n") == "NO\n", "extreme imbalance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | CÓ | trường hợp cơ bản tầm thường | 
| 2 2 | KHÔNG | không thể mô-đun | 
| 2 4 | CÓ | trường hợp tỷ lệ xây dựng hợp lệ | 
| 1 1000000000 | KHÔNG | hạn chế mất cân bằng cực độ | 

## Vỏ cạnh 

Một cạm bẫy thường gặp là chỉ dựa vào điều kiện tỷ lệ mà không kiểm tra ràng buộc mô-đun. Ví dụ,$(2,4)$thỏa mãn trực giác cân bằng nhưng không thành công nếu điều kiện tổng bị bỏ qua trong các biến thể tương tự; ở đây nó thực sự hoạt động vì toàn bộ hệ thống đều nhất quán. Cách xử lý đúng đắn là luôn thực thi cả hai ràng buộc cùng nhau, vì một hạn chế nắm bắt được vấn đề bảo tồn toàn cầu và hạn chế kia nắm bắt được tính khả thi theo định hướng của việc cắt giảm.
