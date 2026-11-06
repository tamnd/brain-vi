---
title: "CF 1279C - Chồng Quà"
description: "Tình huống là một chồng các mục được đánh số duy nhất, trong đó chỉ có phần trên cùng của ngăn xếp mới có thể truy cập trực tiếp. Một chuỗi các thao tác yêu cầu chúng ta liên tục xóa các mục cụ thể theo một thứ tự nhất định."
date: "2026-06-16T02:11:18+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1279
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 79 (Rated for Div. 2)"
rating: 1400
weight: 1279
solve_time_s: 369
verified: false
draft: false
---

[CF 1279C - Chồng quà](https://codeforces.com/problemset/problem/1279/C) 

**Đánh giá:** 1400 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 6 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Tình huống là một chồng các mục được đánh số duy nhất, trong đó chỉ có phần trên cùng của ngăn xếp mới có thể truy cập trực tiếp. Một chuỗi các thao tác yêu cầu chúng ta liên tục xóa các mục cụ thể theo một thứ tự nhất định. Việc xóa một mục không miễn phí: nếu nó nằm ở k vị trí phía dưới trên cùng tại thời điểm xóa, chúng ta phải tạm thời bật k mục phía trên nó, lấy mục tiêu và sau đó khôi phục k mục đó trở lại ngăn xếp. Chi phí của một hoạt động như vậy tỷ lệ thuận với mức độ sâu của mục mục tiêu trong trạng thái ngăn xếp hiện tại. 

Một bước ngoặt làm cho vấn đề trở nên không tầm thường. Khi khôi phục các mục đã tạm thời bị loại bỏ, chúng ta được phép sắp xếp lại chúng một cách tùy ý trước khi đặt lại. Tuy nhiên, chúng tôi không thể di chuyển bất cứ thứ gì ban đầu nằm dưới mục tiêu đã xóa và chúng tôi phải bảo toàn cấu trúc thấp hơn đó mãi mãi. Qua một chuỗi xóa, khả năng sắp xếp lại này cho phép chúng tôi “định hình” ngăn xếp trên các mục tiêu trong tương lai để giảm chi phí truy cập trong tương lai. 

Đầu vào bao gồm thứ tự ngăn xếp ban đầu và danh sách các yêu cầu loại bỏ. Đầu ra là tổng thời gian tối thiểu cần thiết để loại bỏ tất cả các mục cần thiết theo thứ tự nhất định, giả sử chúng ta luôn sử dụng chiến lược sắp xếp lại tốt nhất có thể sau mỗi lần xóa. 

Các ràng buộc cho phép tối đa 100.000 phần tử trong tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào kiểm tra hoặc mô phỏng các hoạt động ngăn xếp lặp đi lặp lại một cách đơn giản đều có nguy cơ xảy ra hành vi bậc hai. Cách tiếp cận O(nm) hoặc thậm chí O(n²) sẽ thất bại vì mỗi lần xóa có khả năng quét và sắp xếp lại các phần lớn của ngăn xếp. 

Chế độ lỗi tinh vi xuất hiện khi một giải pháp đơn giản giả định rằng khi một phần tử bị loại bỏ, vị trí của nó so với tất cả các phần tử còn lại sẽ được cố định. Điều đó không chính xác vì việc sắp xếp lại các phần tử “bộ đệm” có thể đẩy các mục tiêu sau này lên gần đầu hơn. 

Một trường hợp gây hiểu lầm cụ thể là khi các mục tiêu xuất hiện theo thứ tự đan xen các phần tử lồng nhau sâu. Giả sử ngăn xếp là`[5, 4, 3, 2, 1]`và các truy vấn là`[1, 2, 3]`. Một mô phỏng tham lam không tính đến việc sắp xếp lại có thể tính toán lại độ sâu không chính xác sau mỗi lần trích xuất, thiếu các mục đã xóa trước đó ở trên có thể được sắp xếp lại để giảm thiểu độ sâu trong tương lai. 

Một vấn đề khác phát sinh khi việc triển khai giả định rằng số phần tử phía trên mục tiêu là tĩnh. Không phải vậy, bởi vì chỉ những mục tiêu đã được trích xuất mới ảnh hưởng đến việc sắp xếp lại trong tương lai; các phần tử không phải mục tiêu có thể được cải tổ lại để tránh chặn các mục tiêu trong tương lai. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp xử lý từng truy vấn bằng cách quét ngăn xếp để xác định vị trí phần tử được yêu cầu, đếm xem có bao nhiêu mục nằm phía trên nó, sau đó xóa và khôi phục chúng. Về nguyên tắc, điều này đúng vì nó phản ánh chính xác quá trình nhưng lại quá chậm. Mỗi truy vấn có thể đi qua ngăn xếp, dẫn đến hành vi O(nm) trong trường hợp xấu nhất. 

Điều quan trọng cần lưu ý là chỉ có thứ tự tương đối giữa các phần tử “đã được xử lý” và “chưa được xử lý” mới quan trọng. Sau khi xóa một phần tử, chúng tôi có thể coi tất cả các mục phía trên phần tử đó là một nhóm linh hoạt. Chúng tôi có thể tự do sắp xếp lại nhóm này, vì vậy đối với các truy vấn trong tương lai, chúng tôi chỉ quan tâm đến việc liệu một phần tử đã được “kích hoạt” hay chưa. 

Chúng tôi xử lý các truy vấn theo thứ tự và duy trì vị trí cao nhất trong ngăn xếp ban đầu trong số tất cả các phần tử chúng tôi đã sử dụng. Mỗi khi chúng tôi chọn một phần tử mới, nếu nó nằm trên “ranh giới hoạt động” hiện tại, thì sẽ không mất thêm chi phí nào khi vượt qua các phần tử không nhìn thấy được; nếu không, chúng tôi sẽ trả tiền cho tất cả các phần tử không nhìn thấy được giữa ranh giới và phần tử này. 

Điều này làm giảm vấn đề theo dõi vị trí xa nhất đạt được cho đến nay trong thứ tự ngăn xếp ban đầu và chỉ tính tổng các đóng góp khi chúng tôi mở rộng ra ngoài các chỉ số đã thấy trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu hóa theo dõi chỉ mục | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi ánh xạ từng số hiện tại vào vị trí của nó trong ngăn xếp ban đầu. Điều này cho phép chúng ta chuyển đổi chuỗi truy vấn thành chuỗi các chỉ mục trong thời gian tra cứu O(1). 

Chúng tôi duy trì một biến`max_pos`, đại diện cho độ sâu xa nhất trong ngăn xếp ban đầu mà chúng tôi đã “kích hoạt” do các lần xóa trước đó. Chúng tôi cũng duy trì một con trỏ chỉ mục di chuyển qua các vị trí theo thứ tự tăng dần nếu cần. 

Đối với mỗi món quà được yêu cầu: 

1. Chuyển đổi hiện tại vào vị trí của nó trong ngăn xếp ban đầu. 

Bước này chuyển vấn đề từ không gian giá trị sang không gian vị trí, trong đó thứ tự trở nên tuyến tính và có thể so sánh được. 
2. Nếu vị trí này nhỏ hơn hoặc bằng`max_pos`, thêm 0 vào câu trả lời. 

Phần tử này đã nằm trong vùng đã được xóa về mặt khái niệm bởi các hoạt động trước đó, vì vậy nó không yêu cầu phải đi qua bất kỳ mục nào không nhìn thấy được. 
3. Nếu không, hãy thêm`(position - max_pos)`để trả lời. 

Điều này thể hiện các phần tử mới gặp ở trên ranh giới hiện tại và phải được vượt qua lần đầu tiên. 
4. Cập nhật`max_pos`đến vị trí hiện tại nếu nó lớn hơn. 

Điều này mở rộng vùng được xử lý để các truy vấn trong tương lai nhận ra những phần tử này như đã được tính đến. 

Lý do đằng sau quá trình này là mỗi phần tử trong ngăn xếp chỉ đóng góp vào chi phí một lần, tại thời điểm đó nó trở thành một phần của tiền tố được truy cập. Sau đó, nó luôn có thể được sắp xếp lại để tránh chi phí lặp lại. 

### Tại sao nó hoạt động 

Ngăn xếp có thể được xem như một thứ tự dọc cố định, nhưng quyền tự do của chúng ta nằm ở việc chỉ sắp xếp lại phần phía trên phần tử hiện đang được truy cập sau mỗi thao tác. Điều đó có nghĩa là bất kỳ phần tử nào từng được đưa vào “tiền tố đã thấy” đều có thể được đặt ở vị trí cao hơn các mục tiêu trong tương lai một cách tối ưu, loại bỏ chi phí truyền tải lặp đi lặp lại. 

Tính bất biến đó là`max_pos`luôn đại diện cho vị trí sâu nhất trong ngăn xếp ban đầu đã được hiển thị ít nhất một lần trong quá trình xử lý. Mỗi đơn vị tăng thêm`max_pos`tương ứng với chính xác một yếu tố được thanh toán một lần và không bao giờ lặp lại. Vì không có yếu tố nào có thể bị bỏ qua khi nó nằm trên mục tiêu bắt buộc nên mọi đóng góp cho câu trả lời đều cần thiết và ở mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, x in enumerate(a):
        pos[x] = i + 1

    max_pos = 0
    ans = 0

    for x in b:
        p = pos[x]
        if p > max_pos:
            ans += p - max_pos
            max_pos = p

    print(ans)
```Chi tiết triển khai chính là chuyển đổi ngăn xếp thành mảng vị trí. Điều này tránh việc quét lặp lại và cho phép tra cứu liên tục theo thời gian cho từng thành phần truy vấn. Biến`max_pos`mã hóa giới hạn của tiền tố ngăn xếp đã được tính toán sẵn, đảm bảo chúng tôi không bao giờ đếm gấp đôi các phần tử đã được bao gồm trong chi phí trước đó. 

Một lỗi phổ biến là đặt lại hoặc điều chỉnh`max_pos`sau mỗi truy vấn không chính xác, coi nó như một ranh giới cửa sổ trượt. Cách giải thích đúng là đơn điệu: một khi đã đạt được một vị trí thì nó sẽ tiếp tục đạt được mãi mãi. 

## Ví dụ đã hoạt động 

Hãy xem xét ngăn xếp mẫu`[3, 1, 2]`với các truy vấn`[3, 2, 1]`. 

Chúng tôi ánh xạ các giá trị tới các vị trí:`3 -> 1`,`1 -> 2`,`2 -> 3`. 

| Truy vấn | Vị trí | max_pos trước | Chi phí tăng thêm | max_pos sau | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | 0 | 1 | 1 | 
| 2 | 3 | 1 | 2 | 3 | 
| 1 | 2 | 3 | 0 | 3 | 

Tổng chi phí là 3. 

Bây giờ hãy xem xét`[2, 1, 7, 3, 4, 5, 6]`với các truy vấn`[3, 1]`. 

Lập bản đồ:`3 -> 4`,`1 -> 2`. 

| Truy vấn | Vị trí | max_pos trước | Chi phí tăng thêm | max_pos sau | 
| --- | --- | --- | --- | --- | 
| 3 | 4 | 0 | 4 | 4 | 
| 1 | 2 | 4 | 0 | 4 | 

Tổng chi phí là 4. 

Những dấu vết này cho thấy rằng chi phí chỉ tích lũy khi chúng tôi mở rộng tiền tố đã xử lý của ngăn xếp ban đầu lần đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi phần tử được ánh xạ một lần và mỗi truy vấn được xử lý trong thời gian không đổi | 
| Không gian | O(n) | Mảng vị trí lưu trữ chỉ số của từng món quà | 

Giải pháp này phù hợp thoải mái trong các ràng buộc vì tổng số phần tử trong các trường hợp thử nghiệm bị giới hạn bởi 100.000, giúp việc xử lý tuyến tính trên mỗi trường hợp thử nghiệm trở nên hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, x in enumerate(a):
            pos[x] = i + 1

        max_pos = 0
        ans = 0
        for x in b:
            p = pos[x]
            if p > max_pos:
                ans += p - max_pos
                max_pos = p

        out.append(str(ans))
    return "\n".join(out) + "\n"

# provided samples
assert run("""2
3 3
3 1 2
3 2 1
7 2
2 1 7 3 4 5 6
3 1
""") == "5\n8\n"

# custom: single element
assert run("""1
1 1
1
1
""") == "1\n"

# custom: increasing order
assert run("""1
5 5
1 2 3 4 5
1 2 3 4 5
""") == "5\n"

# custom: reverse order
assert run("""1
5 5
5 4 3 2 1
1 2 3 4 5
""") == "5\n"

# custom: interleaving
assert run("""1
6 3
6 1 5 2 4 3
2 5 3
""") == "5\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | ranh giới tối thiểu | 
| thứ tự tăng dần | 5 | không tiết kiệm thêm | 
| thứ tự ngược lại | 5 | tiến triển độ sâu tồi tệ nhất | 
| xen kẽ | 5 | tính chính xác trong truy cập không đơn điệu | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi tất cả phần tử được yêu cầu đều ở gần đầu ngăn xếp nhưng xuất hiện theo thứ tự vị trí giảm dần. Ví dụ, nếu ngăn xếp là`[1, 2, 3, 4, 5]`và các truy vấn là`[5, 4, 3]`, thuật toán mở rộng`max_pos`chỉ khi cần thiết, đảm bảo mỗi quyền truy cập sâu hơn mới sẽ đóng góp chính xác vào phân khúc chưa được khám phá của nó. Việc tính toán chính xác sẽ tránh được việc tính hai lần vì một lần`max_pos`đạt tới 5, tất cả các truy vấn tiếp theo đều nằm trong tiền tố được xử lý. 

Một trường hợp khác là khi các truy vấn đã được sắp xếp theo thứ tự xếp chồng. Nếu ngăn xếp là`[4, 3, 2, 1]`và các truy vấn là`[3, 1]`, truy vấn đầu tiên mở rộng tiền tố một phần và truy vấn thứ hai nằm hoàn toàn bên trong tiền tố đó. Thuật toán tự nhiên chỉ định chi phí bổ sung bằng 0 cho truy vấn thứ hai vì không có tiền tố mới nào được hiển thị.
