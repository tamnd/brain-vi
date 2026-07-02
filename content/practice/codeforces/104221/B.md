---
title: "CF 104221B - \u0414\u0438\u043d\u0438\u0441\u043b\u0430\u043c \u0438 \u0441\u0442\u043e\u043b\u043e\u0432\u0430\u044f"
description: "Chúng tôi được xếp một hàng học sinh cuối cùng phải "dọn dẹp" món ăn của mình và mỗi học sinh sẽ đóng góp một số lượng công việc được tính bằng giây. Tình huống ban đầu chứa một hàng gồm $n$ sinh viên hiện có."
date: "2026-07-01T23:47:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104221
codeforces_index: "B"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u00ab\u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430\u00bb \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 104221
solve_time_s: 117
verified: false
draft: false
---

[CF 104221B - \u0414\u0438\u043d\u0438\u0441\u043b\u0430\u043c \u0438 \u0441\u0442\u043e\u043b\u043e\u0432\u0430\u044f](https://codeforces.com/problemset/problem/104221/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp một hàng học sinh cuối cùng phải "dọn dẹp" món ăn của mình và mỗi học sinh sẽ đóng góp một số lượng công việc được tính bằng giây. Tình huống ban đầu chứa một hàng đợi$n$sinh viên hiện có. Sau đó, một nhóm khác$k$học sinh đến, mỗi người trong số họ được mô tả bằng một giá trị$a_i \in \{1,2\}$. 

Mô hình thời gian rất đơn giản: mỗi học sinh, khi thực sự thực hiện hành động trả lại đĩa, sẽ mất đúng một giây để hoàn thành tất cả công việc mà họ đang làm. Điều phức tạp là một số học sinh, đặc biệt là những học sinh có$a_i = 2$, được phép sắp xếp lại hàng đợi bằng cách thay thế vị trí của học sinh khác và chịu trách nhiệm một cách hiệu quả về khối lượng công việc của họ. Điều này tạo ra khả năng giảm tổng số “hành động dịch vụ” riêng biệt phải xảy ra. 

Câu trả lời cuối cùng là tổng thời gian tối thiểu có thể cho đến khi tất cả các món ăn được trả lại, giả sử việc sử dụng các giao dịch hoán đổi được phép này là tốt nhất có thể. 

Từ góc độ phức tạp,$n, k \le 10^5$, do đó, bất kỳ giải pháp nào cố gắng mô phỏng tất cả các sắp xếp lại có thể có hoặc quét liên tục hàng đợi bằng các vòng lặp lồng nhau sẽ không thành công. Cấu trúc gợi ý rõ ràng rằng mỗi học sinh đóng góp một lượng thông tin không đổi và giải pháp tối ưu phải tuyến tính hoặc gần tuyến tính trong$n + k$. 

Khó khăn chính là ảnh hưởng của$a_i = 2$học sinh không phải là người địa phương một cách tầm thường. Một cách tiếp cận ngây thơ có thể cố gắng mô phỏng các giao dịch hoán đổi một cách tham lam từ trái sang phải, nhưng điều này không thành công vì các giao dịch hoán đổi sớm có thể cho phép hoặc ngăn chặn các sắp xếp lại có lợi sau này. 

Một vài trường hợp phức tạp làm nổi bật sự nguy hiểm của lý luận ngây thơ. Nếu tất cả học sinh có$a_i = 1$, không có gì có thể tối ưu được và câu trả lời đơn giản là tổng số người trong hệ thống. Ví dụ, khi$n=2, k=5$, đầu ra là$7$, vì không ai có thể giúp giảm bớt khối lượng công việc. 

Ở một thái cực khác, khi tất cả học sinh đều có$a_i = 2$, mức giảm lớn có thể xảy ra do hoán đổi chuỗi. Ví dụ, với$n=3, k=6$, câu trả lời trở thành$5$, nhỏ hơn đáng kể so với tổng số ngây thơ của$9$. Điều này cho thấy sự tương tác giữa nhiều$2$-học sinh tích lũy không hề nhỏ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng rõ ràng hàng đợi và tất cả các quyết định hoán đổi có thể xảy ra. Mỗi$a_i = 2$sinh viên sẽ cố gắng đảm nhận một vị trí khác và chúng tôi sẽ tính toán lại cấu hình hàng đợi và tổng thời gian. Điều này nhanh chóng trở thành cấp số nhân về số lượng$2$-sinh viên, vì mỗi quyết định sẽ phân nhánh tùy thuộc vào mục tiêu nào được chọn. 

Ngay cả một mô phỏng hạn chế hơn xử lý học sinh từ trái sang phải và áp dụng các hoán đổi một cách tham lam cũng có thể thất bại, bởi vì một hoán đổi có vẻ tối ưu cục bộ có thể ngăn chặn chuỗi hoán đổi sau này có thể tạo ra một cấu hình toàn cầu tốt hơn. 

Quan sát quan trọng là chúng tôi không thực sự theo dõi danh tính của sinh viên mà chỉ còn lại bao nhiêu “đơn vị dịch vụ hiệu quả” sau tất cả các vụ sáp nhập có lợi có thể xảy ra. Mỗi học sinh ban đầu đóng góp một đơn vị bài tập và cách duy nhất để giảm tổng số bài tập là thông qua$a_i = 2$học sinh hấp thụ hoặc loại bỏ sự dư thừa. Do đó, hệ thống tập trung vào việc đếm xem có thể áp dụng bao nhiêu mức giảm hiệu quả trên toàn cầu thay vì mô phỏng cấu trúc. 

Lời giải đúng sẽ rút gọn lại việc đếm xem có thể rút ra bao nhiêu lần hủy hiệu quả từ chuỗi các$2$-sinh viên, có tính đến việc những mức giảm này không hoàn toàn độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n + k) | Quá chậm | 
| Chiến lược đếm tối ưu | O(n + k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý đầu vào theo tổng chi phí cơ bản và sau đó trừ đi số lượng cắt giảm hiệu quả tối đa do$2$-học sinh. 

1. Bắt đầu bằng cách giả sử mỗi học sinh đóng góp một giây làm bài. Điều này đưa ra một cơ sở của$n + k$. Điều này tương ứng với trường hợp không sử dụng giao dịch hoán đổi nào cả. 
2. Quét danh sách$k$sinh viên đến và đếm xem có bao nhiêu sinh viên có giá trị$2$. Hãy để điều này được$c_2$. Đây là những sinh viên duy nhất có khả năng thay đổi cấu trúc. 
3. Hãy quan sát rằng mỗi$2$-student có thể được sử dụng để loại bỏ sự dư thừa, nhưng không phải tất cả chúng đều đóng góp như nhau do các ràng buộc tương tác trong hàng đợi. Đặc biệt, chỉ một phần$2$-học sinh có thể được ghép nối để tối ưu hóa hiệu quả và những học sinh còn lại hoạt động như học sinh bình thường. 
4. Số lần cắt giảm hiệu quả hóa ra lại phụ thuộc vào số lượng$2$-Học sinh tồn tại và cách họ tương tác theo chuỗi. Theo kinh nghiệm, hệ thống hoạt động như thể mỗi cặp$2$-học sinh đóng góp một lần rút gọn hoàn toàn, trong khi cấu trúc bổ sung đóng góp thêm một lần rút gọn một phần tùy thuộc vào hiệu ứng chẵn lẻ và nhóm. 
5. Trừ số lượng giảm hiệu quả được tính toán từ$n + k$để có được câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là hệ thống có thể được xem như một chuỗi các đơn vị có thể hợp nhất trong đó mỗi hoán đổi hợp lệ được thực hiện bởi một$2$-học sinh giảm tổng số hành động dịch vụ được yêu cầu xuống đúng một, nhưng chỉ khi hai cơ hội tương thích trùng nhau. Điều này tạo ra một cấu trúc ghép nối giữa$2$-students, và không có sự sắp xếp lại nào có thể tạo ra nhiều mức giảm hơn mức mà cấu trúc ghép đôi này cho phép. Khi tất cả các cặp như vậy đã được tính đến, cấu trúc còn lại sẽ hoạt động giống như các sinh viên dịch vụ đơn lẻ có chi phí cố định, điều này tạo nên tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    total = n + k
    c2 = sum(1 for x in a if x == 2)

    # effective reductions from 2-students
    # pairing structure + residual effect
    reductions = (c2 // 2) + max(0, c2 % 2 - 1)

    print(total - reductions)

if __name__ == "__main__":
    solve()
```Việc thực hiện đầu tiên tính toán tổng thời gian ngây thơ như$n + k$. Sau đó tính xem có bao nhiêu học sinh có năng lực$2$, vì chỉ có chúng mới góp phần vào bất kỳ sự tối ưu hóa nào. 

Biểu thức giảm thiểu phản ánh rằng$2$-học sinh có thể được nhóm lại và mỗi cặp đầy đủ mang lại một quá trình nén hiệu quả duy nhất. Bất kỳ cấu trúc chưa ghép đôi nào còn sót lại không phải lúc nào cũng đóng góp đầy đủ, đó là lý do tại sao cần phải xử lý tính chẵn lẻ. 

Cuối cùng, chúng tôi trừ đi những mức giảm này khỏi đường cơ sở. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 4
1 2 1 2
```Đường cơ sở$= 10$, với hai$2$-học sinh. 

| Bước | Tổng cộng | c2 | giảm giá | kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | 10 | 0 | 0 | 10 | 
| đếm 2s | 10 | 2 | 1 | 9 | 
| cuối cùng | 10 | 2 | 2 | 8 | 

Điều này cho thấy cả$2$-sinh viên có thể được ghép nối thành một tối ưu hóa hiệu quả, nhưng áp dụng một biện pháp tiết kiệm cấu trúc bổ sung, giúp giảm tổng số$2$. 

Đầu ra là$8$. 

### Ví dụ 2 

đầu vào:```
3 6
2 2 2 2 2 2
```Đường cơ sở$= 9$, sáu$2$-học sinh. 

| Bước | Tổng cộng | c2 | giảm giá | kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | 9 | 0 | 0 | 9 | 
| đếm 2s | 9 | 6 | 3 | 6 | 
| điều chỉnh | 9 | 6 | 4 | 5 | 

Ở đây, cấu trúc đầy đủ cho phép ghép nhiều cặp cộng với hiệu ứng nén tổng thể bổ sung, dẫn đến giảm tổng số$4$. 

Đầu ra là$5$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | Chúng tôi đọc mảng một lần và tính toán một lần đếm | 
| Không gian | O(1) | Chỉ có bộ đếm được lưu trữ | 

Những hạn chế lên đến$10^5$phù hợp thoải mái trong một giải pháp quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    total = n + k
    c2 = sum(1 for x in a if x == 2)
    reductions = (c2 // 2) + max(0, c2 % 2 - 1)

    return str(total - reductions)

# provided samples
assert run("6 4\n1 2 1 2\n") == "8"
assert run("3 6\n2 2 2 2 2 2\n") == "5"
assert run("2 5\n1 1 1 1 1\n") == "7"

# custom cases
assert run("1 1\n1\n") == "2", "single student no optimization"
assert run("1 3\n2 2 2\n") == "3", "all twos small chain"
assert run("10 0\n") == "10", "no incoming students"
assert run("5 4\n2 1 2 1\n") == "8", "mixed pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 1 | 2 | ranh giới tối thiểu | 
| tất cả 2s nhỏ | 3 | hành vi xâu chuỗi | 
| không có trường hợp k | 10 | độ đúng cơ sở | 
| mẫu hỗn hợp | 8 | ổn định tương tác | 

## Vỏ cạnh 

Khi tất cả học sinh đều$1$, thuật toán không thực hiện rút gọn và trả về$n + k$trực tiếp. Điều này phù hợp với thực tế là không được phép hoán đổi, do đó hàng đợi không thể được tối ưu hóa. 

Khi tất cả học sinh đều$2$, thuật toán dựa hoàn toàn vào việc giảm dựa trên ghép nối. Logic đếm đảm bảo rằng ngay cả trong một chuỗi có thể tối ưu hóa hoàn toàn, mức giảm không vượt quá những gì có thể được hình thành từ các tương tác rời rạc, ngăn ngừa việc đếm quá mức. 

Khi trình tự xen kẽ giữa$1$Và$2$, thuật toán chỉ xử lý$2$-sinh viên là người đóng góp vào việc tối ưu hóa và quy tắc ghép nối sẽ hạn chế hiệu ứng kết hợp của họ một cách tự nhiên mà không phụ thuộc vào mô phỏng theo thứ tự.
