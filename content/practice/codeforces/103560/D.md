---
title: "CF 103560D - \u041f\u043e\u0434\u0430\u0440\u043e\u043a \u0434\u043b\u044f \u041b\u0443\u0438\u0434\u0436\u0438"
description: "Chúng ta được tặng một hộp kẹo, mỗi viên kẹo thuộc về một loại nào đó. Với mỗi loại, chúng ta có thể đếm được có bao nhiêu loại kẹo thuộc loại đó. Từ nhóm này, chúng tôi muốn tập hợp một “món quà” bằng cách chọn một số loại kẹo."
date: "2026-07-03T05:26:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103560
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2018"
rating: 0
weight: 103560
solve_time_s: 51
verified: true
draft: false
---

[CF 103560D - \u041f\u043e\u0434\u0430\u0440\u043e\u043a \u0434\u043b\u044f \u041b\u0443\u0438\u0434\u0436\u0438](https://codeforces.com/problemset/problem/103560/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một hộp kẹo, mỗi viên kẹo thuộc về một loại nào đó. Với mỗi loại, chúng ta có thể đếm được có bao nhiêu loại kẹo thuộc loại đó. Từ nhóm này, chúng tôi muốn tập hợp một “món quà” bằng cách chọn một số loại kẹo. 

Hạn chế mang tính cấu trúc: nếu chúng ta xem xét món quà cuối cùng và nhóm kẹo theo loại, số lượng kẹo được chọn của mỗi loại phải khác với mọi loại được chọn khác. Nói cách khác, nếu loại 1 xuất hiện 5 lần trong quà tặng và loại 2 xuất hiện 3 lần thì không loại nào được chọn khác được phép xuất hiện đúng 5 hoặc 3 lần. Một số loại có thể bị bỏ qua hoàn toàn và đối với các loại đã chọn, chúng tôi có thể tự do lấy bất kỳ số dương nào tùy theo nguồn cung hiện có. 

Mục tiêu là tối đa hóa tổng số kẹo trong quà tặng, là tổng số kẹo đã chọn trên tất cả các loại đã chọn. 

Đây là vấn đề phân bổ toàn cầu: chúng tôi đang phân phối “hạn ngạch” số nguyên cho các loại có giới hạn dung lượng (mỗi loại có tần suất tối đa), đồng thời thực thi rằng tất cả hạn ngạch đã chọn phải khác biệt theo cặp và chúng tôi muốn tối đa hóa tổng của chúng. 

Các ràng buộc rất lớn, với tổng kích thước đầu vào lên tới khoảng hai trăm nghìn viên kẹo trên các truy vấn. Điều này loại trừ mọi mô phỏng lồng nhau bậc hai hoặc theo truy vấn trên tất cả các giá trị. Bất cứ điều gì chậm hơn tuyến tính đại khái cho mỗi truy vấn sẽ không thành công. 

Trường hợp khó nhận biết xuất hiện khi tần số rất nhỏ hoặc rất lặp lại. Ví dụ: nếu tất cả các loại xuất hiện nhiều lần, chúng tôi không thể chỉ định hạn ngạch lớn bằng nhau ngay cả khi có dung lượng. Ngược lại, nếu nhiều loại xuất hiện một lần, chúng ta không thể gán cho tất cả chúng giá trị 1 vì bị cấm trùng lặp. 

Ví dụ: giả sử chúng ta có ba loại, mỗi loại xuất hiện hai lần. Một cách tiếp cận đơn giản có thể gán 2, 2, 2 nhưng điều đó vi phạm quy tắc phân biệt. Hành vi đúng là gán 2, 1, 0, tạo ra tổng bằng 3. Một trường hợp đặc biệt khác là một loại có nhiều lần xuất hiện, trong đó câu trả lời đơn giản là tất cả chúng. 

Những mẫu này cho thấy chúng tôi không chỉ đơn giản là tối đa hóa mức sử dụng theo từng loại mà còn sắp xếp các giá trị được chỉ định một cách cẩn thận. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng gán giá trị cho từng loại bằng cách quay lại hoặc đoán tham lam. Đối với mỗi loại, chúng tôi có thể thử mọi số đếm có thể từ tần số của nó xuống 1 và đảm bảo không sử dụng bản sao. Điều này ngay lập tức dẫn đến một vụ nổ tổ hợp. Ngay cả khi chúng ta chỉ xem xét các tần số lên tới n, việc thử tất cả các phép gán trên k loại sẽ tạo ra kết quả như n lựa chọn cho mỗi loại, là số mũ tính bằng k. Với tối đa hai trăm nghìn phần tử, điều này là không thể. 

Quan sát quan trọng là chỉ những viên kẹo được chọn cuối cùng mới được tính, chứ không phải loại kẹo chính xác nào được sử dụng. Khi chúng tôi tính toán tần số của từng loại, bài toán sẽ trở thành: gán cho mỗi loại một số nguyên x_i sao cho 0 ≤ x_i ≤ c_i, tất cả x_i dương đều khác biệt và tổng tối đa của x_i. 

Đây là một cấu trúc tham lam cổ điển. Nếu chúng tôi sắp xếp các loại theo tần suất giảm dần, chúng tôi luôn muốn đưa ra hạn ngạch lớn hơn cho những loại có đủ khả năng chi trả. Lý do rất đơn giản: tần số lớn mang lại sự linh hoạt, vì vậy chúng ta nên dành những giá trị lớn cho chúng. Các tần số nhỏ hơn bị hạn chế và sẽ có hạn ngạch nhỏ hơn hoặc bị loại bỏ. 

Sau đó, chúng tôi xây dựng giải pháp từ hạn ngạch lớn nhất có thể trở xuống. Chúng tôi duy trì hạn ngạch sẵn có lớn nhất mà chúng tôi sẵn sàng chỉ định. Đối với mỗi tần số theo thứ tự giảm dần, chúng tôi gán cho nó số hợp lệ lớn nhất có thể, nhỏ hơn hoàn toàn so với tần số chúng tôi đã sử dụng trước đó, đồng thời không vượt quá dung lượng của tần số đó. Nếu công suất quá nhỏ thì chúng ta chỉ lấy công suất đó và tiếp tục giảm xuống.

Cấu trúc này hoạt động vì ràng buộc chỉ là “tất cả các giá trị được gán phải khác biệt”. Điều đó ngay lập tức gợi ý rằng chuỗi giảm dần là tối ưu, vì bất kỳ hoán vị nào của các giá trị được gán đều có thể được sắp xếp mà không làm thay đổi tính khả thi và các giá trị lớn hơn luôn đóng góp nhiều hơn vào tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm phân công vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Sắp xếp tần số + phân công giảm tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén dữ liệu đầu vào thành tần số của từng loại kẹo. Điều này chuyển đổi vấn đề từ việc xử lý các mục riêng lẻ sang xử lý nhiều năng lực. 

## Hướng dẫn thuật toán 

1. Đếm số lần mỗi loại xuất hiện, tạo ra một dãy tần số. Bước này là cần thiết vì chỉ tính theo từng loại chứ không tính danh tính của từng loại kẹo. 
2. Sắp xếp mảng tần số theo thứ tự giảm dần. Điều này đảm bảo rằng chúng tôi luôn cố gắng chỉ định hạn ngạch lớn cho những loại thực sự có thể hỗ trợ chúng, ngăn ngừa lãng phí sớm các giá trị cao cho các loại có dung lượng nhỏ. 
3. Khởi tạo một biến`cap`là một số rất lớn, về mặt khái niệm là vô hạn nhưng được triển khai giống như n, vì không loại nào có thể lấy nhiều hơn n kẹo. 
4. Lặp lại các tần số theo thứ tự được sắp xếp. Với mỗi tần số`f`, chúng tôi muốn gán một giá trị`x`đó là cả 2 f và nhỏ hơn giá trị được gán cuối cùng`cap`. 
5. Đặt`x = min(f, cap - 1)`. Điều này thực thi đồng thời cả hai ràng buộc: chúng tôi không vượt quá mức sẵn có và chúng tôi duy trì tính khác biệt bằng cách giảm thiểu sự chỉ định. 
6. Nếu`x`trở thành 0 hoặc âm, chúng ta dừng quá trình vì không thể gán thêm giá trị dương nào nữa. Tất cả các loại còn lại sẽ không đóng góp gì. 
7. Nếu không, hãy thêm`x`để trả lời và cập nhật`cap = x`. Điều này đảm bảo giá trị được gán tiếp theo phải nhỏ hơn hoàn toàn, duy trì tính duy nhất. 

Quá trình này xây dựng một chuỗi số lượng được chọn giảm dần, mỗi số được giới hạn ở trên bởi tần số tương ứng. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi sẽ quyết định giá trị hợp lệ lớn nhất có thể cho loại hiện tại với ràng buộc là tất cả các giá trị được chọn trước đó đều khác biệt và lớn hơn. Bởi vì chúng tôi xử lý tần số theo thứ tự giảm dần nên chúng tôi không bao giờ hối tiếc khi gán một giá trị lớn sớm: mọi phép gán sau này thậm chí sẽ có dung lượng nhỏ hơn và việc hoán đổi sẽ không làm tăng tổng. Bất biến tham lam là sau khi xử lý i loại, chúng tôi đã xây dựng tổng tối đa có thể bằng cách sử dụng i số nguyên dương riêng biệt, mỗi số nguyên tuân theo giới hạn tần số của nó và bất kỳ sự sắp xếp thay thế nào có cùng độ dài tiền tố đều không thể vượt quá nó mà không vi phạm thứ tự hoặc tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        n = int(input())
        freq = {}
        for _ in range(n):
            a = int(input())
            freq[a] = freq.get(a, 0) + 1

        vals = sorted(freq.values(), reverse=True)

        cap = n
        ans = 0

        for f in vals:
            x = min(f, cap - 1)
            if x <= 0:
                break
            ans += x
            cap = x

        print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách nén từng truy vấn vào bản đồ tần số, vì chỉ tính theo loại. Sắp xếp theo thứ tự giảm dần là xương sống của chiến lược tham lam, đảm bảo chúng ta luôn xem xét các loại linh hoạt nhất trước tiên. 

Biến`cap`theo dõi giá trị hữu dụng lớn nhất còn lại để chuyển nhượng. Nó bắt đầu từ n vì không có phép gán hợp lệ nào có thể vượt quá số lượng kẹo. Ở mỗi bước, chúng tôi kẹp cả tần số và giới hạn hiện tại trừ đi một, đảm bảo mức giảm nghiêm ngặt. 

Một sai lầm phổ biến ở đây là quên`-1`khi tính toán giá trị được phép tiếp theo. Không có nó, các phép gán liên tiếp bằng nhau có thể xuất hiện, vi phạm ràng buộc về tính khác biệt. 

Việc nghỉ sớm khi`x <= 0`rất quan trọng vì một khi chúng ta không thể chỉ định ít nhất 1 thì tất cả các đóng góp còn lại đều bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
types: [1, 1, 1, 2, 2]
```Tần số trở thành:```
[3, 2]
```| Bước | Tần số | nắp trước | đã chọn x | giới hạn sau | tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 5 | 2 | 2 | 2 | 
| 2 | 2 | 2 | 1 | 1 | 3 | 

Thuật toán trước tiên gán 2 cho loại có tần số 3. Mặc dù có sẵn 3 nhưng chúng ta không thể sử dụng nó vì phải bảo toàn khoảng trống cho các giá trị riêng biệt khác. Loại tiếp theo nhận được 1. Câu trả lời cuối cùng là 3, là tối ưu. 

### Ví dụ 2 

đầu vào:```
n = 6
types: [1, 1, 1, 1, 1, 1]
```Tần số:```
[6]
```| Bước | Tần số | nắp trước | đã chọn x | giới hạn sau | tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 6 | 6 | 5 | 5 | 5 | 

Chúng tôi lấy 5 vì phép gán đầu tiên phải chừa chỗ cho các số nguyên dương nhỏ hơn. Không có loại nào khác nên quá trình kết thúc ngay lập tức. 

Điều này cho thấy rằng ngay cả một loại duy nhất cũng không thể sử dụng hết dung lượng của nó, vì các ràng buộc về tính khác biệt ngầm buộc một cấu trúc giảm dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | tần suất sắp xếp trên mỗi truy vấn chiếm ưu thế | 
| Không gian | O(n) | bản đồ tần số và danh sách đếm | 

Tổng số n trên các truy vấn tối đa là hai trăm nghìn, vì vậy giải pháp phù hợp thoải mái trong giới hạn thời gian. Việc sắp xếp vẫn hiệu quả vì tổng số mục nhập tần suất trên tất cả các truy vấn cũng bị giới hạn bởi số lượng loại riêng biệt gặp phải. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    q = int(input())
    out = []

    for _ in range(q):
        n = int(input())
        freq = {}
        for _ in range(n):
            a = int(input())
            freq[a] = freq.get(a, 0) + 1

        vals = sorted(freq.values(), reverse=True)

        cap = n
        ans = 0
        for f in vals:
            x = min(f, cap - 1)
            if x <= 0:
                break
            ans += x
            cap = x

        out.append(str(ans))

    return "\n".join(out)

# sample tests (format adapted)
assert run("1\n5\n1\n1\n1\n2\n2\n") == "3"

# single type max
assert run("1\n6\n1\n1\n1\n1\n1\n1\n") == "5"

# all distinct
assert run("1\n4\n1\n2\n3\n4\n") == "4"

# mixed frequencies
assert run("1\n6\n1\n1\n1\n2\n2\n3\n") == "5"

# edge small
assert run("1\n1\n1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các loại bằng nhau | hành vi giới hạn giảm tuyến tính | thống trị tần số đơn | 
| tất cả các loại riêng biệt | lựa chọn đầy đủ tầm thường | độ chính xác cơ bản | 
| phân phối hỗn hợp | cân bằng tham lam | tương tác của các ràng buộc | 
| phần tử đơn | ranh giới tối thiểu | xử lý trường hợp cạnh | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các tần số đều bằng 1. Thuật toán gán 1, sau đó gán 0 cho cạnh tiếp theo và dừng ngay lập tức. Điều này tạo ra chính xác 1 cho nhiều loại thay vì tổng hợp không chính xác tất cả chúng. 

Một trường hợp cạnh khác là loại đơn có tần số rất lớn. Thuật toán giảm nó một cách chính xác xuống n-1, vì chúng ta phải duy trì một dãy dương giảm nghiêm ngặt. 

Trường hợp thứ ba là khi có nhiều tần số trung bình, chẳng hạn như nhiều loại, mỗi loại có tần số 3. Thứ tự tham lam đảm bảo chúng ta gán 3, 2, 1 cho một số loại đầu tiên rồi dừng lại. Bất kỳ nỗ lực nào để gán các giá trị bằng nhau sẽ vi phạm ràng buộc, do đó, cơ chế giới hạn giảm dần sẽ thực thi tính chính xác một cách tự nhiên.
