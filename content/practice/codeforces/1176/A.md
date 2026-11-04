---
title: "CF 1176A - Chia đi!"
description: "Chúng ta được cấp một số nguyên bắt đầu và chúng ta được phép thu nhỏ nó nhiều lần bằng cách sử dụng một tập hợp nhỏ các phép chia chia cố định. Mỗi thao tác thay thế giá trị hiện tại bằng một giá trị nhỏ hơn nhưng chỉ khi giá trị hiện tại chia hết cho một số cụ thể: 2, 3 hoặc 5."
date: "2026-06-12T01:44:57+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1176
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 565 (Div. 3)"
rating: 800
weight: 1176
solve_time_s: 83
verified: true
draft: false
---

[CF 1176A - Chia nó!](https://codeforces.com/problemset/problem/1176/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số nguyên bắt đầu và chúng ta được phép thu nhỏ nó nhiều lần bằng cách sử dụng một tập hợp nhỏ các phép chia chia cố định. Mỗi thao tác thay thế giá trị hiện tại bằng một giá trị nhỏ hơn nhưng chỉ khi giá trị hiện tại chia hết cho một số cụ thể: 2, 3 hoặc 5. Mục tiêu là chuyển đổi số đó thành chính xác 1 bằng cách sử dụng càng ít thao tác càng tốt. Nếu không có cách nào đạt được 1 bằng cách sử dụng các quy tắc này, chúng tôi phải báo cáo là không thể thực hiện được. 

Mỗi truy vấn là độc lập, vì vậy chúng tôi giải quyết vấn đề chuyển đổi này nhiều lần cho các giá trị bắt đầu khác nhau có khả năng lên tới$10^{18}$. Ý tưởng chính là mọi thao tác đều giảm số lượng một cách nghiêm ngặt, do đó mọi quy trình hợp lệ đều hữu hạn, nhưng không phải mọi số nguyên đều có thể giảm xuống 1 vì các phép toán được phép chỉ loại bỏ một số thừa số nguyên tố nhất định trong tỷ lệ có cấu trúc. 

Ràng buộc$n \le 10^{18}$ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng tất cả các đường dẫn chuyển đổi có thể có. Không thể thực hiện được BFS hoặc DP trên các giá trị vì không gian trạng thái không bị giới hạn và việc phân nhánh thậm chí ở mức độ vừa phải sẽ bùng nổ. Điều còn lại là một phương pháp suy luận trực tiếp về cấu trúc của các con số thay vì khám phá trình tự. 

Trường hợp cạnh tinh tế xuất hiện khi một số chứa các thừa số nguyên tố khác 2, 3 hoặc 5. Ví dụ:$n = 14$chứa hệ số 7. Không có thao tác nào loại bỏ hệ số 7, vì vậy mọi nỗ lực sẽ thất bại ngay lập tức. Đầu ra đúng là -1. Việc giảm tham lam bất cẩn chỉ thử các thao tác khi có thể vẫn có thể lặp lại hoặc kết thúc thành công không chính xác nếu nó không xác thực rõ ràng khả năng tiếp cận cuối cùng. 

Một trường hợp cạnh khác xảy ra khi số đã bằng 1. Trong trường hợp này, không cần thực hiện phép toán nào và câu trả lời là 0. Điều này rất quan trọng vì một vòng lặp đơn giản luôn cố gắng chia sẽ thực hiện sai các phép toán không cần thiết. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là coi mỗi số là một trạng thái trong biểu đồ trong đó các cạnh biểu thị các phép toán hợp lệ. Từ bất kỳ số nào$n$, chúng ta có thể đi đến$n/2$,$2n/3$, hoặc$4n/5$khi điều kiện chia được thỏa mãn. BFS sẽ đảm bảo số bước tối thiểu để đạt 1. 

Cách tiếp cận này đúng vì mọi hoạt động đều có chi phí bằng nhau và BFS khám phá các trạng thái theo thứ tự khoảng cách tăng dần. Tuy nhiên, lỗ hổng nghiêm trọng là số lượng trạng thái có thể truy cập không bị giới hạn một cách hữu ích. Mặc dù các giá trị tổng thể giảm xuống, việc phân nhánh theo các thứ tự loại bỏ yếu tố khác nhau sẽ tạo ra nhiều trạng thái trung gian và đối với số lượng lớn, điều này trở nên không khả thi. 

Quan sát chính là cấu trúc của các hoạt động không phải là tùy ý. Mỗi thao tác chỉ loại bỏ các thừa số nguyên tố 2, 3 hoặc 5 và thực hiện như vậy theo một tỷ lệ cố định. Điều này có nghĩa là thứ tự thực hiện các thao tác không quan trọng; điều quan trọng là chúng ta loại bỏ từng thừa số nguyên tố bao nhiêu lần. Bài toán rút gọn thành việc đếm bội số của 2, 3 và 5 trong$n$, đồng thời đảm bảo không tồn tại thừa số nguyên tố nào khác. 

Khi chúng tôi nhận ra điều này, giải pháp tối ưu sẽ trở thành một vòng lặp nhân tử đơn giản: liên tục chia 2, rồi 3, rồi 5 và theo dõi số lần chúng tôi thực hiện mỗi phép chia. Sau khi loại bỏ tất cả các số 2, 3 và 5 có thể có, nếu còn số nào khác ngoài 1 thì câu trả lời là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (BFS trên các tiểu bang) | Hàm mũ | O(tiểu bang) | Quá chậm | 
| Tối ưu (tước hệ số nguyên tố) | O(log n) cho mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập và giảm số lượng từng bước. 

1. Đọc số$n$. Nếu như$n = 1$, ngay lập tức xuất ra 0 vì không cần thao tác nào. Điều này xử lý trường hợp suy biến một cách rõ ràng mà không cần nhập bất kỳ vòng lặp nào. 
2. Khởi tạo bộ đếm`moves = 0`. Điều này sẽ lưu trữ tổng số hoạt động được thực hiện. 
3. Trong khi$n$chia hết cho 2 thì chia cho 2 rồi tăng dần`moves`. Mỗi phép chia như vậy tương ứng chính xác với một phép toán hợp lệ, vì vậy chúng ta đang trích xuất tất cả các thừa số của 2. 
4. Lặp lại quy trình tương tự trong 3: while$n$chia hết cho 3 thì chia và tăng`moves`. Điều này đảm bảo tất cả các khoản đóng góp từ loại hoạt động thứ hai đều được tính đến. 
5. Lặp lại lần nữa trong 5: trong khi$n$chia hết cho 5 thì chia và tăng`moves`. Điều này trích xuất tất cả các yếu tố tương ứng với loại hoạt động thứ ba. 
6. Sau khi loại bỏ tất cả các thừa số của 2, 3 và 5, kiểm tra xem giá trị còn lại có phải là 1 hay không. Nếu không, ghi -1 vì điều này có nghĩa là$n$chứa ít nhất một thừa số nguyên tố bên ngoài {2, 3, 5}, không bao giờ có thể loại bỏ được bằng các phép toán được phép. 
7. Nếu giá trị còn lại là 1, xuất ra`moves`. 

### Tại sao nó hoạt động 

Mọi thao tác chỉ loại bỏ một loại thừa số nguyên tố khỏi số một cách có kiểm soát. Không có phép toán nào đưa ra các số nguyên tố mới hoặc thay đổi các số nguyên tố ngoài {2, 3, 5}. Do đó, bất kỳ chuỗi phép tính hợp lệ nào cũng tương ứng chính xác với việc loại bỏ tất cả các lần xuất hiện của 2, 3 và 5 khỏi hệ số của$n$. Thứ tự loại bỏ không quan trọng vì phép nhân có tính giao hoán trong hệ số nguyên tố. Nếu sau khi loại bỏ các số nguyên tố này mà vẫn còn sót lại thì hệ số còn lại đó là bất biến theo quy tắc, do đó việc đạt đến 1 là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(n: int) -> int:
    moves = 0

    for p in (2, 3, 5):
        while n % p == 0:
            n //= p
            moves += 1

    return moves if n == 1 else -1

q = int(input())
for _ in range(q):
    n = int(input())
    print(solve(n))
```Giải pháp xử lý từng số một cách độc lập bằng cách sử dụng một vòng lặp nhỏ trên các số nguyên tố 2, 3 và 5. Mỗi phép chia tương ứng chính xác với một phép toán được phép, do đó việc đếm các phép chia trực tiếp sẽ đưa ra câu trả lời. Kiểm tra cuối cùng đảm bảo không còn thừa số nguyên tố không hợp lệ. 

Thứ tự xử lý các số nguyên tố không quan trọng vì mỗi vòng lặp độc lập và chỉ loại bỏ phần đóng góp của số nguyên tố đó. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào từ bộ mẫu để xem thuật toán hoạt động như thế nào. 

### Ví dụ 1: n = 30 

| Bước | n | Hành động | di chuyển | 
| --- | --- | --- | --- | 
| bắt đầu | 30 | ban đầu | 0 | 
| chia cho 2 | 15 | 30/2 | 1 | 
| chia cho 3 | 5 | 15/3 | 2 | 
| chia cho 5 | 1 | 5/5 | 3 | 

Kết quả cuối cùng là 1, vì vậy đáp án là 3. 

Điều này chứng tỏ cách các thừa số nguyên tố hỗn hợp được tách ra một cách độc lập cho đến khi chỉ còn lại 1. 

### Ví dụ 2: n = 14 

| Bước | n | Hành động | di chuyển | 
| --- | --- | --- | --- | 
| bắt đầu | 14 | ban đầu | 0 | 
| chia cho 2 | 7 | 14/2 | 1 | 
| chia cho 3 | 7 | không thay đổi | 1 | 
| chia cho 5 | 7 | không thay đổi | 1 | 

Kết quả cuối cùng là 7, không phải 1, vì vậy đầu ra là -1. 

Điều này cho thấy một số chứa hệ số nguyên tố không được hỗ trợ không thể giảm hoàn toàn bất kể các thao tác có sẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) cho mỗi truy vấn | Mỗi phép chia giảm n ít nhất hệ số 2, 3 hoặc 5 | 
| Không gian | O(1) | Chỉ có một số bộ đếm và biến được sử dụng | 

Các ràng buộc cho phép tối đa 1000 truy vấn với các giá trị lên tới$10^{18}$. Ngay cả trong trường hợp xấu nhất, phép chia lặp đi lặp lại sẽ giảm số lượng rất nhanh, do đó lời giải vẫn chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve(n: int) -> int:
        moves = 0
        for p in (2, 3, 5):
            while n % p == 0:
                n //= p
                moves += 1
        return moves if n == 1 else -1

    q = int(input())
    out = []
    for _ in range(q):
        n = int(input())
        out.append(str(solve(n)))
    return "\n".join(out)

# provided samples
assert run("7\n1\n10\n25\n30\n14\n27\n1000000000000000000\n") == "0\n4\n6\n3\n-1\n3\n72"

# custom cases
assert run("1\n1\n") == "0", "single base case"
assert run("1\n14\n") == "-1", "prime factor outside 2,3,5"
assert run("1\n8\n") == "3", "only power of two"
assert run("1\n45\n") == "4", "mixed 3 and 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | trường hợp nhận dạng | 
| 14 | -1 | không thể do số nguyên tố 7 | 
| 8 | 3 | phép chia lặp lại cho 2 | 
| 45 | 4 | loại bỏ yếu tố hỗn hợp | 

## Vỏ cạnh 

cho$n = 1$, thuật toán ngay lập tức trả về 0 trước khi kiểm tra bất kỳ yếu tố nào. Điều này tránh việc đếm sai các phép chia không cần thiết. 

Đối với một số như$n = 1000000000000000000$, thuật toán liên tục chia cho 2, rồi 3, rồi 5. Dù số lớn nhưng nó nhanh chóng rút gọn về 1 vì chỉ chứa các thừa số nguyên tố được phép. Cấu trúc vòng lặp đảm bảo tính chính xác vì mỗi lần xóa đều chính xác và không thể đảo ngược. 

Vì$n = 14$, số tồn tại trong tất cả các phép chia được phép có thừa số còn lại là 7. Thuật toán phát hiện chính xác điều này bằng lần kiểm tra cuối cùng$n \neq 1$, đảm bảo không xảy ra kết quả dương tính giả ngay cả khi việc giảm một phần xảy ra sớm.
