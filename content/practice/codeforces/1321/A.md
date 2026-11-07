---
problem: 1321A
contest_id: 1321
problem_index: A
name: "Contest for Robots"
contest_name: "Codeforces Round 625 (Div. 2, based on Technocup 2020 Final Round)"
rating: 900
tags: ["greedy"]
answer: passed_samples
verified: true
solve_time_s: 275
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2dea0c-3f38-83ec-99b2-4ad1063464b8
---

#CF 1321A - Cuộc thi Robot 

**Xếp hạng:** 900 
**Thẻ:** tham lam 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 4 phút 35 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2dea0c-3f38-83ec-99b2-4ad1063464b8 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta có hai chuỗi nhị phân có độ dài cố định$n$, mô tả cách hai robot hoạt động trong một nhóm nhiệm vụ. Đối với mỗi nhiệm vụ$i$, robot đầu tiên có thể giải được hoặc không, và điều tương tự đối với robot thứ hai. Polycarp gán điểm số nguyên dương$p_i \ge 1$cho từng nhiệm vụ và tổng điểm của rô-bốt sẽ trở thành tổng điểm của các nhiệm vụ mà nó giải quyết được. 

Sự tự do là trong việc lựa chọn các giá trị$p_i$, nhưng có một hạn chế: chúng tôi muốn rô-bốt đầu tiên có tổng điểm lớn hơn rô-bốt thứ hai. Trong số tất cả các phép gán hợp lệ như vậy, chúng ta muốn cực tiểu hóa giá trị được gán lớn nhất. 

Sự tương tác giữa các nhiệm vụ hoàn toàn độc lập ngoại trừ thông qua ràng buộc tối đa toàn cầu này. Mỗi nhiệm vụ đều đóng góp như nhau cho cả hai robot, chỉ cho một trong số chúng hoặc không cho nhiệm vụ nào. Cấu trúc này gợi ý rằng điều quan trọng không phải là giá trị tuyệt đối của$p_i$, mà là cách chúng khuếch đại sự khác biệt giữa hai robot. 

Những hạn chế$n \le 100$ngụ ý rằng ngay cả lý luận bậc hai hoặc bậc ba đối với các nhiệm vụ cũng có thể thực hiện được, nhưng giải pháp cuối cùng sẽ trở thành tuyến tính sau khi xác định được tập hợp chính xác. Khó khăn chính là mỗi$p_i$không cố định trong một tập nhỏ, nó có thể thay đổi độc lập, do đó việc tìm kiếm đơn giản trên các bài tập là không thể. 

Một trường hợp phức tạp xảy ra khi cả hai robot đều hoạt động giống hệt nhau trong mọi nhiệm vụ. Trong tình huống đó, bất kỳ bài tập nào cũng tạo ra điểm số giống nhau nên việc vượt trội hoàn toàn là điều không thể. Một trường hợp thất bại khác xảy ra khi robot thứ nhất không bao giờ giải được nhiệm vụ mà robot thứ hai giải được, từ đó robot thứ nhất không bao giờ có thể giành được lợi thế dù có ghi điểm hay không. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các phép gán giá trị có thể$p_i$lên đến một số ràng buộc$K$và kiểm tra xem có tồn tại nhiệm vụ làm cho điểm của rô-bốt đầu tiên lớn hơn hay không. Ngay cả đối với một cố định$K$, mỗi$p_i$có$K$sự lựa chọn, vì vậy đây là$K^n$, điều này vượt xa khả thi ngay cả đối với những người nhỏ bé$n$. 

Quan sát quan trọng là sự khác biệt về điểm số là tuyến tính trong các giá trị được chọn. Mỗi nhiệm vụ đóng góp tích cực, tiêu cực hoặc hoàn toàn không tạo nên sự khác biệt giữa các robot. Vì chúng tôi kiểm soát từng hệ số$p_i$độc lập bên trong$[1, K]$, chiến lược tối ưu là đẩy những đóng góp tích cực lên càng cao càng tốt và những đóng góp tiêu cực càng thấp càng tốt. 

Điều này làm giảm toàn bộ vấn đề về việc đếm xem có bao nhiêu nhiệm vụ giúp đỡ robot đầu tiên và bao nhiêu nhiệm vụ giúp đỡ robot thứ hai, sau đó cân bằng sự đóng góp của chúng dưới các nhiệm vụ cực đoan của$p_i$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua bài tập | Hàm mũ | O(n) | Quá chậm | 
| Tham lam tách giá trị | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi phân loại từng nhiệm vụ bằng cách so sánh hành vi của robot. Chúng ta hãy xác định ba nhóm: nhiệm vụ được giải quyết bởi cả hai robot, nhiệm vụ chỉ được giải quyết bởi robot thứ nhất và nhiệm vụ chỉ được giải quyết bởi robot thứ hai. 

Sau đó, chúng tôi suy luận về cách gán các giá trị để tối đa hóa sự khác biệt về điểm số có lợi cho robot đầu tiên trong khi vẫn tôn trọng giá trị tối đa$K$. 

1. Đếm xem robot thứ nhất có bao nhiêu nhiệm vụ được giải quyết nhưng robot thứ hai không giải quyết được. Gọi đây$A$. Những nhiệm vụ này tạo ra sự khác biệt tích cực, vì vậy chúng tôi muốn chúng càng lớn càng tốt, nghĩa là chúng tôi gán giá trị cho chúng$K$. 
2. Đếm xem robot thứ hai có bao nhiêu nhiệm vụ được giải quyết nhưng robot thứ nhất không giải quyết được. Gọi đây$B$. Những nhiệm vụ này tạo ra sự khác biệt tiêu cực, vì vậy chúng tôi muốn chúng càng nhỏ càng tốt, nghĩa là chúng tôi gán giá trị cho chúng$1$. 
3. Các nhiệm vụ được cả hai robot giải quyết hoặc không có nhiệm vụ nào đóng góp như nhau cho cả hai robot, do đó, sự đóng góp của chúng sẽ bị loại bỏ do sự khác biệt bất kể được phân công. 
4. Dưới sự phân công tối ưu, sự khác biệt về điểm số sẽ trở thành$A \cdot K - B$. 
5. Chúng ta cần sự khác biệt này hoàn toàn dương, vì vậy chúng ta yêu cầu$A \cdot K > B$. 
6. Nếu$A = 0$, thì sự khác biệt luôn là$-B$, không bao giờ có thể trở thành dương trừ khi$B = 0$, nhưng ngay cả khi đó cả hai robot vẫn bằng nhau, nên sự bất bình đẳng nghiêm ngặt là không thể xảy ra. 
7. Mặt khác, giá trị tối thiểu hợp lệ$K$là số nguyên nhỏ nhất thỏa mãn$K > \frac{B}{A}$, đó là$\left\lfloor \frac{B}{A} \right\rfloor + 1$. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi nhiệm vụ đều đóng góp độc lập và tuyến tính vào chênh lệch điểm số và mỗi hệ số$p_i$chỉ được giới hạn bên dưới và bên trên. Vì những đóng góp tích cực và tiêu cực có thể được tách biệt và được đẩy một cách độc lập đến các cực trị, nên bất kỳ giải pháp tối ưu nào cũng phải bão hòa các giới hạn này: tối đa hóa trọng số đối với các nhiệm vụ có sai biệt dương và giảm thiểu trọng số đối với các nhiệm vụ có sai biệt âm. Điều này đảm bảo rằng không có phép gán hỗn hợp nào có thể tốt hơn phép gán cực trị được xây dựng, do đó việc kiểm tra bất đẳng thức trên$A \cdot K - B$mô tả đầy đủ tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
r = list(map(int, input().split()))
b = list(map(int, input().split()))

A = 0
B = 0

for i in range(n):
    if r[i] == 1 and b[i] == 0:
        A += 1
    elif r[i] == 0 and b[i] == 1:
        B += 1

if A == 0:
    print(-1)
else:
    K = (B // A) + 1
    print(K)
```Giải pháp tách các nhiệm vụ thành hai loại duy nhất có ảnh hưởng đến sự khác biệt. Vòng lặp là tuyến tính và xây dựng số đếm trực tiếp. Công thức cuối cùng xuất phát trực tiếp từ việc giải bất đẳng thức$A \cdot K > B$dưới các ràng buộc số nguyên. 

Một lỗi phổ biến là cố gắng gán giá trị cho cả bốn mẫu có thể một cách độc lập; chỉ có các mẫu bất đối xứng mới quan trọng để tối ưu hóa. Một sự tinh tế khác là xử lý$A = 0$, trong đó phép chia sẽ không hợp lệ và câu trả lời luôn là không thể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1 1 0 0
0 1 1 1 1
```Ở đây phân loại là: 

| tôi | r | b | gõ | hiệu ứng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | A | + | 
| 2 | 1 | 1 | trung lập | 0 | 
| 3 | 1 | 1 | trung lập | 0 | 
| 4 | 0 | 1 | B | - | 
| 5 | 0 | 1 | B | - | 

Vì thế$A = 1$,$B = 2$. Chúng tôi cần$1 \cdot K > 2$, Vì thế$K = 3$. 

Điều này chứng tỏ một nhiệm vụ tích cực mạnh mẽ có thể bù đắp cho nhiều nhiệm vụ tiêu cực như thế nào, nhưng chỉ bằng cách tăng giá trị tối đa được phép. 

### Ví dụ 2 

đầu vào:```
3
1 0 1
1 0 1
```Cả hai robot đều hoạt động giống hệt nhau trong mọi nhiệm vụ, do đó không có$A$hoặc$B$đóng góp. chúng tôi nhận được$A = 0$,$B = 0$. Sự khác biệt luôn bằng 0 nên sự thống trị chặt chẽ là không thể, tạo ra$-1$. 

Điều này cho thấy rằng mặc dù điểm số có thể được ấn định tùy ý nhưng hành vi giống hệt nhau sẽ loại bỏ mọi đòn bẩy có thể kiểm soát được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Hạng mục nhiệm vụ đếm một lượt | 
| Không gian | O(1) | Chỉ có một vài quầy được sử dụng | 

Thuật toán dễ dàng phù hợp trong giới hạn vì$n \le 100$và thậm chí những khái quát hóa lớn sẽ vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    n = int(input())
    r = list(map(int, input().split()))
    b = list(map(int, input().split()))

    A = 0
    B = 0

    for i in range(n):
        if r[i] == 1 and b[i] == 0:
            A += 1
        elif r[i] == 0 and b[i] == 1:
            B += 1

    if A == 0:
        print(-1)
    else:
        print(B // A + 1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided sample
assert run("5\n1 1 1 0 0\n0 1 1 1 1\n") == "3"

# both identical
assert run("3\n1 0 1\n1 0 1\n") == "-1"

# first dominates completely
assert run("3\n1 1 1\n0 0 0\n") == "1"

# second dominates completely
assert run("3\n0 0 0\n1 1 1\n") == "-1"

# mixed case
assert run("4\n1 0 1 0\n0 1 0 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng giống hệt nhau | -1 | bất bình đẳng nghiêm ngặt không thể | 
| thống trị hoàn toàn | 1 | trường hợp tích cực đơn giản nhất | 
| sự thống trị ngược | -1 | lỗi đối xứng | 
| mô hình xen kẽ | 1 | cấu trúc cạnh cân bằng | 

## Vỏ cạnh 

Khi cả hai robot giải quyết chính xác cùng một nhóm vấn đề, mọi nhiệm vụ đều thuộc loại trung tính. Thuật toán tạo ra$A = 0$, ngay lập tức kích hoạt tình trạng không thể. Mặc dù điểm số có thể được ấn định một cách tùy ý, nhưng mọi phép gán đều mang lại tổng số giống nhau, do đó không có cấu hình nào có thể thỏa mãn tính ưu việt tuyệt đối. 

Khi robot thứ hai thống trị mọi nhiệm vụ, mọi đóng góp đều mang tính tiêu cực hoặc trung tính. Tính toán$B > 0$với$A = 0$một lần nữa buộc phải thất bại. Điều này cho thấy điều kiện không chỉ về tổng số mà còn về sự tồn tại của ít nhất một đòn bẩy mà robot đầu tiên có thể giành được lợi thế tương đối.
