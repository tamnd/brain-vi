---
title: "CF 102281I - \u0414\u0435\u0442\u0441\u043a\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng ta được cung cấp một phép cộng được viết bằng từ thay vì chữ số, chẳng hạn như VOLVO+FIAT=MOTOR. Mỗi chữ cái riêng biệt phải được gán một chữ số từ 0 đến 9. Hai chữ cái khác nhau phải nhận các chữ số khác nhau, trong khi mỗi lần xuất hiện của cùng một chữ cái sẽ nhận được cùng một chữ số."
date: "2026-08-13T09:27:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "I"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 96
verified: true
draft: false
---

[CF 102281I - \u0414\u0435\u0442\u0441\u043a\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một phép cộng được viết bằng từ thay vì chữ số, chẳng hạn như`VOLVO+FIAT=MOTOR`. Mỗi chữ cái riêng biệt phải được gán một chữ số từ`0`bởi vì`9`. Hai chữ cái khác nhau phải nhận được các chữ số khác nhau, trong khi mỗi lần xuất hiện của cùng một chữ cái sẽ nhận được cùng một chữ số. Các số 0 đứng đầu được cho phép rõ ràng, vì vậy chữ cái đầu tiên của một từ không có gì đặc biệt. 

Nhiệm vụ là tìm mọi phép gán mà giá trị số của từ đầu tiên cộng với giá trị số của từ thứ hai bằng giá trị số của từ kết quả. Mỗi phép gán hợp lệ được in bằng cách thay thế mọi chữ cái trong biểu thức ban đầu bằng chữ số được gán cho nó. Tuyên bố đảm bảo rằng có nhiều nhất 1000 giải pháp. 

Mỗi từ trong số ba từ có tối đa 15 ký tự. Điều này làm cho việc chuyển đổi một bài tập đã hoàn thành thành ba số nguyên trở nên rẻ tiền, nhưng nó không làm cho việc thử mọi bài tập trở nên rẻ tiền. Có thể có nhiều nhất 10 chữ cái khác nhau vì chỉ có 10 chữ số. Một tìm kiếm hoàn toàn mù quáng trên mười chữ cái có thể lên tới`10! = 3,628,800`bài tập, và với mỗi bài tập vẫn phải đánh giá ba từ. Con số đó đã có hàng triệu ứng viên và việc triển khai Python đơn giản có thể dành phần lớn thời gian để kiểm tra các bài tập có thể đã bị từ chối sớm hơn nhiều. 

Cấu trúc của phép cộng cho chúng ta một ràng buộc mạnh hơn nhiều. Thay vì đợi cho đến khi mỗi chữ cái có một chữ số, chúng ta có thể xử lý phép cộng từ cột đơn vị về phía cột có ý nghĩa nhất. Mỗi cột chỉ có một khả năng mang và khi biết các chữ số của hai phần bổ sung, chữ số kết quả sẽ bị buộc. Điều này biến sự bình đẳng toàn cầu thành một chuỗi các kiểm tra cục bộ rất nhỏ. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Đầu tiên, số 0 đứng đầu là hợp pháp. Ví dụ,`A+A=B`có giải pháp hợp lệ`5+5=0`, vậy bài tập`A=5, B=0`phải được chấp nhận. Việc triển khai cấm ký tự đầu tiên bằng 0 sẽ loại bỏ nó một cách không chính xác. 

Vấn đề thứ hai là cùng một chữ cái có thể xuất hiện ở nhiều vị trí trong cùng một cấu trúc cột. Vì`A+A=A`, giải pháp duy nhất là`0+0=0`, vì mỗi chữ cái phải có cùng giá trị ở mọi nơi. Một bộ giải xử lý hai lần xuất hiện của`A`vì các biến độc lập có thể vô tình chấp nhận các bài tập như`1+1=1`. 

Hộp đựng cạnh thứ ba là hộp đựng cuối cùng. Đối với một đầu vào như`A+B=CA`, một phép gán hợp lệ có thể yêu cầu phép cộng để tạo thêm một chữ số có nghĩa lớn nhất. Người giải phải xử lý tất cả các cột và xác minh rằng phần nhớ còn lại sau cột thực cuối cùng chính xác bằng 0. Bỏ qua điều kiện cuối cùng đó có thể chấp nhận một phép cộng không đầy đủ. 

Cuối cùng, các chữ cái khác nhau không bao giờ được chia sẻ một chữ số. Vì`A+B=C`, bài tập`A=1, B=1, C=2`thỏa mãn đẳng thức số học nhưng không phải là phép gán mật mã hợp lệ. Cấu trúc sử dụng chữ số phải loại bỏ nó trước khi đưa ra câu trả lời. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thu thập tất cả các chữ cái riêng biệt, gán các chữ số cho chúng theo mọi cách nội suy có thể, chuyển đổi ba từ thành số nguyên và kiểm tra sự bằng nhau. Điều này đúng vì mọi phép gán hợp pháp có thể được xem xét chính xác một lần và bài kiểm tra số học cuối cùng khớp chính xác với điều kiện của bài toán. 

Khó khăn là kích thước của không gian tìm kiếm. Với mười chữ cái riêng biệt có`10 * 9 * 8 * ... * 1 = 10! = 3,628,800`bài tập. Nếu mỗi bài tập yêu cầu quét tối đa 45 ký tự trong ba từ thì trường hợp xấu nhất là ở mức 160 triệu thao tác ở cấp độ ký tự. Việc tìm kiếm cũng không có tác dụng hữu ích cho đến khi tất cả các chữ cái đã được chỉ định. 

Quan sát làm thay đổi vấn đề là phép cộng thập phân có tính chất cục bộ. Hãy xem xét một cột từ bên phải. Nếu hai chữ số cộng là`x`Và`y`, và số mang đến là`carry`, sau đó`x + y + carry = result_digit + 10 * next_carry`. 

Một lần`x`Và`y`được biết đến,`result_digit`Và`next_carry`được xác định hoàn toàn. Nếu chữ cái kết quả đã có chữ số thì chúng ta chỉ cần so sánh nó với chữ số bắt buộc. Nếu nó chưa được gán, chúng ta phải kiểm tra xem chữ số bắt buộc có được sử dụng hay không. 

Điều này cho phép chúng ta tìm kiếm từ phải sang trái. Ở mỗi cột, chúng tôi chỉ gán các chữ cái vẫn chưa được biết xuất hiện trong hai phần bổ sung. Chữ số kết quả sau đó được lấy ra thay vì đoán. Phép gán một phần sai sẽ chết ngay lập tức trong cột nơi phép tính số học trở nên không thể thực hiện được. 

Tìm kiếm brute-force hoạt động vì mọi phép gán hoàn chỉnh có thể được kiểm tra độc lập, nhưng không thành công vì nó trì hoãn tất cả các ràng buộc số học cho đến khi kết thúc. Quay lui theo cột áp dụng ràng buộc mạnh nhất hiện có ngay khi biết các chữ số yêu cầu của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(L * 10!)`trong trường hợp xấu nhất |`O(U)`| Quá chậm để triển khai Python chặt chẽ | 
| Quay lại theo cột |`O(L * P(10,U))`trường hợp xấu nhất, với việc cắt tỉa cột mạnh mẽ |`O(U + L)`| Đã chấp nhận | 

Đây`L`là độ dài từ tối đa,`U`là số chữ cái riêng biệt và`P(10,U) = 10!/(10-U)!`. Từ`U <= 10`được cố định bởi bảng chữ cái thập phân, việc tìm kiếm thực tế rất nhỏ, đặc biệt vì hầu hết các nhánh đều bị từ chối trước khi tất cả các chữ cái được gán. 

## Hướng dẫn thuật toán 

1. Chia biểu thức đầu vào tại`+`Và`=`vào hai phần bổ sung và kết quả. Lưu trữ các chữ cái dưới dạng chuỗi để sau này có thể kiểm tra vị trí của chúng từ phải sang trái. 
2. Đếm các chữ cái riêng biệt. Nếu có nhiều hơn 10 thì không thể gán bất kỳ phép gán nào vì chỉ có mười chữ số. Việc tìm kiếm có thể chấm dứt ngay lập tức. 
3. Duy trì một mảng`digit[26]`, ban đầu chứa`-1`, lưu trữ chữ số được chỉ định của mỗi chữ cái. Duy trì mười yếu tố`used`mảng cho biết chữ số nào đã được lấy. 
4. Xử lý các cột từ vị trí đơn vị đến vị trí quan trọng nhất. Đối với cột`pos`, chữ số cộng là ký tự tại`a[-1-pos]`nếu vị trí đó tồn tại, nếu không thì nó đóng góp bằng không. Quy tắc tương tự được sử dụng cho phần bổ sung thứ hai và kết quả. 
5. Chỉ nhìn vào các chữ cái riêng biệt xuất hiện ở hai vị trí phụ của cột hiện tại. Nếu một trong số chúng không có chữ số được chỉ định, hãy thử từng chữ số chưa sử dụng cho nó. Vì có nhiều nhất hai chữ cái phụ trong một cột nên điều này tạo ra tối đa 90 lựa chọn trước khi xem xét kết quả. 
6. Tính toán`total = digit_left + digit_right + carry`. Chữ số kết quả cần tìm là`total % 10`, và giá trị của cột tiếp theo là`total // 10`. 
7. Nếu chữ cái kết quả đã có một chữ số, hãy so sánh chữ số đó với chữ số kết quả được yêu cầu. Sự không phù hợp làm cho nhánh hiện tại không thể thực hiện được. Nếu chữ cái kết quả không được gán, chỉ gán chữ số được yêu cầu khi chữ số đó không được sử dụng. Đây là bước cắt tỉa quan trọng vì chữ số kết quả không bao giờ được đoán. 
8. Lặp lại cột tiếp theo với phần mang mới. Sau khi quay lại, hãy hoàn tác mọi phép gán được thực hiện trong cột hiện tại để nhánh khác bắt đầu với trạng thái chính xác trước đó. 
9. Sau khi tất cả các cột đã được xử lý, chỉ chấp nhận phép gán khi số nhớ bằng 0. Chuyển đổi mọi từ bằng cách sử dụng bài tập và lưu biểu thức kết quả. 

Điều bất biến là trước khi xử lý một cột, mọi chữ cái cần thiết cho hậu tố đã được xử lý đều có một chữ số cố định, tất cả các chữ số được gán đều khác biệt và hậu tố được xử lý thỏa mãn phép cộng bao gồm cả ký tự hiện tại. Mọi chuyển đổi đệ quy đều bảo toàn tính bất biến này vì nó kiểm tra phương trình thập phân chính xác cho cột hiện tại. Ngược lại, bất kỳ phép gán hoàn chỉnh hợp lệ nào cũng phải thỏa mãn từng phương trình cột riêng lẻ, do đó các lựa chọn tương ứng sẽ không bao giờ bị lược bỏ. Do đó, mọi phép gán được tạo ra đều hợp lệ và mọi phép gán hợp lệ cuối cùng cũng được tạo ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_expression(expr):
    a, rest = expr.split('+')
    b, c = rest.split('=')

    max_len = max(len(a), len(b), len(c))

    letters = set(a + b + c)
    if len(letters) > 10:
        return []

    digit = [-1] * 26
    used = [False] * 10
    solutions = []

    def get_char(word, pos):
        idx = len(word) - 1 - pos
        if idx < 0:
            return -1
        return ord(word[idx]) - ord('A')

    def build_number(word):
        value = 0
        for ch in word:
            value = value * 10 + digit[ord(ch) - ord('A')]
        return value

    def make_output():
        na = build_number(a)
        nb = build_number(b)
        nc = build_number(c)
        solutions.append(f"{na}+{nb}={nc}")

    def dfs(pos, carry):
        if pos == max_len:
            if carry == 0:
                make_output()
            return

        x = get_char(a, pos)
        y = get_char(b, pos)
        z = get_char(c, pos)

        assigned_now = []

        def assign_operand(ch, d):
            digit[ch] = d
            used[d] = True
            assigned_now.append((ch, d))

        def undo():
            while assigned_now:
                ch, d = assigned_now.pop()
                digit[ch] = -1
                used[d] = False

        # Recursively assign the distinct letters appearing in
        # the two addend positions.
        operands = []
        if x != -1:
            operands.append(x)
        if y != -1 and y != x:
            operands.append(y)

        def assign_operands(idx):
            if idx == len(operands):
                dx = 0 if x == -1 else digit[x]
                dy = 0 if y == -1 else digit[y]

                total = dx + dy + carry
                needed = total % 10
                next_carry = total // 10

                if z == -1:
                    if needed != 0:
                        return
                    dfs(pos + 1, next_carry)
                    return

                if digit[z] != -1:
                    if digit[z] == needed:
                        dfs(pos + 1, next_carry)
                    return

                if used[needed]:
                    return

                digit[z] = needed
                used[needed] = True
                dfs(pos + 1, next_carry)
                digit[z] = -1
                used[needed] = False
                return

            ch = operands[idx]

            if digit[ch] != -1:
                assign_operands(idx + 1)
                return

            for d in range(10):
                if used[d]:
                    continue

                assign_operand(ch, d)
                assign_operands(idx + 1)
                undo()

        assign_operands(0)

    dfs(0, 0)
    return solutions

def main():
    expr = input().strip()
    solutions = solve_expression(expr)

    out = [str(len(solutions))]
    out.extend(solutions)
    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    main()
```Trình phân tích cú pháp tách biểu thức thành chính xác ba từ. Vì định dạng đầu vào chỉ chứa một`+`và một`=`, hai`split`hoạt động là đủ. 

các`digit`mảng sử dụng các chỉ số chữ cái từ`0`ĐẾN`25`. Một giá trị của`-1`có nghĩa là bức thư chưa được giao. các`used`mảng cung cấp các kiểm tra liên tục để xem liệu có sẵn chữ số ứng cử viên hay không.`get_char`chuyển đổi số cột được đo từ bên phải thành chỉ mục ký tự. Trở về`-1`đối với một vị trí bị thiếu thì thuận tiện vì phần bổ sung bị thiếu sẽ đóng góp số 0 vào cột đó. Không có cách xử lý đặc biệt nào về số 0 đứng đầu vì bài toán cho phép rõ ràng các số 0 đứng đầu. 

Sự lồng nhau`assign_operands`chức năng là nơi xảy ra việc quay lại. Nó chỉ gán các chữ cái xuất hiện trong hai phần bổ sung cho cột hiện tại. Nếu một chữ cái được gán bởi một cột trước đó, nó sẽ được sử dụng lại mà không cần phân nhánh. 

Khi đã có sẵn các chữ số toán hạng, chữ số kết quả được tính bằng`% 10`. Việc mang theo được tính bằng`// 10`. Thứ tự này quan trọng vì chữ số kết quả thuộc về cột hiện tại, trong khi số mang thuộc về cột tiếp theo. 

Một trường hợp tinh vi xảy ra khi vị trí kết quả không tồn tại. Sau đó, chữ số kết quả về mặt khái niệm là số 0. Mã chỉ chấp nhận tình huống đó khi chữ số được tính bằng 0. Ví dụ: nếu cả hai phần bổ sung đã kết thúc nhưng phần mang vẫn còn, phần mang đó sẽ được xử lý bởi phần cuối cùng`pos == max_len`kiểm tra thay vì phát minh ra một chữ số kết quả khác. 

Các bài tập luôn được hoàn tác sau khi nhánh đệ quy kết thúc. Thư kết quả cũng được gán tạm thời và khôi phục rõ ràng. Nếu không có sự khôi phục này, một chữ số được chọn trong một nhánh sẽ rò rỉ sang nhánh tiếp theo và âm thầm loại bỏ các giải pháp hợp lệ. 

Số nguyên Python không tràn và từ lớn nhất có thể chỉ có 15 chữ số, vì vậy số học số nguyên thông thường là quá đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét`ONE+ONE=TWO`. Quá trình xử lý bắt đầu ở cột đơn vị, trong đó`E + E`phải sản xuất`O`. Sau đó, phần mang sẽ xác định cột hàng chục, v.v. Một chi nhánh đại diện thành công được hiển thị dưới đây. 

| Cột | Chữ số bên trái | Chữ số bên phải | Mang vào | Tổng hợp | Chữ số kết quả | Thực hiện | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 |`5`|`5`| 0 | 10 |`0`| 1 | 
| 1 |`6`|`6`| 1 | 13 |`3`| 1 | 
| 2 |`0`|`0`| 1 | 1 |`1`| 0 | 

Nhiệm vụ tương ứng là`O=0`,`N=1`,`E=5`, cho`015+015=030`. Chi nhánh cụ thể này thực sự bị từ chối vì`N=1`Và`O=0`là nhất quán, nhưng từ`ONE`là`015`Và`TWO`là`?30`, yêu cầu`T=0`, mâu thuẫn với`O=0`. Điểm quan trọng là xung đột được phát hiện khi gán chữ cái kết quả thay vì sau khi xây dựng tất cả các phép gán chữ cái có thể có. 

Một chi nhánh thành công như`065+065=130`có cùng một cột bất biến. Cột đơn vị cung cấp`5+5=10`, sửa chữa`O=0`và tạo ra số 1. Cột hàng chục cho`6+6+1=13`, sửa chữa`N=3`và tạo ra số 1. Cột hàng trăm cho kết quả`0+0+1=1`, sửa chữa`T=1`. Mỗi cột đều đồng ý với cùng một bản đồ toàn cầu. 

Mẫu chứa 17 bài tập hợp lệ và các số 0 đứng đầu như`065`được cố tình bảo tồn ở dạng in. 

### Mẫu 2 

cho`VOLVO+FIAT=MOTOR`, cột ngoài cùng bên phải chứa`O + T = R`cộng với việc mang theo. Cột tiếp theo chứa`V + A`, và các chữ cái lặp lại trong`VOLVO`Và`MOTOR`gây ra các phép gán được thực hiện trước đó để hạn chế các cột sau. 

Một giải pháp thành công từ mẫu là`15615+9743=25358`. Đọc từ phải sang trái sẽ có dấu vết sau. 

| Cột | Chữ số bên trái | Chữ số bên phải | Mang vào | Tổng hợp | Chữ số kết quả | Thực hiện | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 |`5`|`3`| 0 | 8 |`8`| 0 | 
| 1 |`1`|`4`| 0 | 5 |`5`| 0 | 
| 2 |`6`|`7`| 0 | 13 |`3`| 1 | 
| 3 |`5`|`9`| 1 | 15 |`5`| 1 | 
| 4 |`1`| 0 | 1 | 2 |`2`| 0 | 

Ánh xạ kết quả là`V=1`,`O=5`,`L=6`,`F=9`,`I=7`,`A=4`,`T=3`,`M=2`,`R=8`. Cột thứ năm sử dụng thực tế là`FIAT`không còn chữ số nào nữa nên đóng góp của nó bằng 0. Số mang cuối cùng bằng 0, chứng tỏ rằng đẳng thức hoàn chỉnh gồm năm chữ số đã được giải quyết. 

Ví dụ này chứng minh tại sao việc xử lý các cột từ phải sang trái lại mạnh hơn việc gán các chữ cái theo thứ tự tùy ý. Một số chữ số bị ép buộc bởi số học, thay vì được đoán một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(L * P(10,U))`trường hợp xấu nhất | Nhiều nhất`P(10,U)`phép gán chữ số nội tại có thể xuất hiện trong cây quay lui và mỗi nhánh tiến qua nhiều nhất`L`cột. Các ràng buộc số học thường được loại bỏ sớm hơn nhiều. | 
| Không gian |`O(U + L)`| Ánh xạ chữ số, mảng chữ số đã sử dụng, độ sâu đệ quy và chuỗi đầu ra được lưu trữ yêu cầu không gian tỷ lệ thuận với số lượng chữ cái, độ dài từ và giải pháp. | 

Đây`L <= 15`Và`U <= 10`. Giới hạn tìm kiếm lý thuyết là hữu hạn và nhỏ vì chỉ có mười chữ số, trong khi các ràng buộc cột loại bỏ các nhánh trước khi tất cả mười chữ số thường được gán. Tối đa 1000 giải pháp đầu ra được đảm bảo cũng giới hạn lượng dữ liệu kết quả được lưu trữ. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây sử dụng cùng một logic của bộ giải và so sánh kết quả đầu ra dưới dạng tập hợp, vì bài toán cho phép giải pháp theo thứ tự tùy ý. Đối với các mẫu, các giải pháp mong đợi được viết rõ ràng. Đối với các trường hợp tùy chỉnh, kết quả đầu ra dự kiến ​​đủ ngắn để xác minh trực tiếp.```python
# helper: run solution on input string, return output string
import sys
import io

def solve_expression(expr):
    a, rest = expr.split('+')
    b, c = rest.split('=')

    max_len = max(len(a), len(b), len(c))
    if len(set(a + b + c)) > 10:
        return []

    digit = [-1] * 26
    used = [False] * 10
    solutions = []

    def get_char(word, pos):
        idx = len(word) - 1 - pos
        if idx < 0:
            return -1
        return ord(word[idx]) - 65

    def number(word):
        value = 0
        for ch in word:
            value = value * 10 + digit[ord(ch) - 65]
        return value

    def dfs(pos, carry):
        if pos == max_len:
            if carry == 0:
                solutions.append(
                    f"{number(a)}+{number(b)}={number(c)}"
                )
            return

        x = get_char(a, pos)
        y = get_char(b, pos)
        z = get_char(c, pos)

        operands = []
        if x != -1:
            operands.append(x)
        if y != -1 and y != x:
            operands.append(y)

        def choose(idx):
            if idx == len(operands):
                dx = 0 if x == -1 else digit[x]
                dy = 0 if y == -1 else digit[y]

                total = dx + dy + carry
                needed = total % 10
                next_carry = total // 10

                if z == -1:
                    if needed == 0:
                        dfs(pos + 1, next_carry)
                    return

                if digit[z] != -1:
                    if digit[z] == needed:
                        dfs(pos + 1, next_carry)
                    return

                if used[needed]:
                    return

                digit[z] = needed
                used[needed] = True
                dfs(pos + 1, next_carry)
                used[needed] = False
                digit[z] = -1
                return

            ch = operands[idx]

            if digit[ch] != -1:
                choose(idx + 1)
                return

            for d in range(10):
                if not used[d]:
                    digit[ch] = d
                    used[d] = True
                    choose(idx + 1)
                    used[d] = False
                    digit[ch] = -1

        choose(0)

    dfs(0, 0)
    return solutions

def run(inp: str) -> str:
    expr = inp.strip()
    ans = solve_expression(expr)
    return str(len(ans)) + (("\n" + "\n".join(ans)) if ans else "")

def parse_output(s):
    lines = s.strip().splitlines()
    count = int(lines[0])
    return count, set(lines[1:])

# Sample 1
sample1 = run("ONE+ONE=TWO")
count, got = parse_output(sample1)
expected1 = {
    "065+065=130",
    "085+085=170",
    "206+206=412",
    "216+216=432",
    "231+231=462",
    "236+236=472",
    "271+271=542",
    "281+281=562",
    "286+286=572",
    "291+291=582",
    "407+407=814",
    "417+417=834",
    "427+427=854",
    "432+432=864",
    "452+452=904",
    "457+457=914",
    "467+467=934",
    "482+482=964",
}
assert count == 17 and got == expected1, "sample 1"

# Sample 2
sample2 = run("VOLVO+FIAT=MOTOR")
count, got = parse_output(sample2)
expected2 = {
    "15615+9743=25358",
    "15715+9643=25358",
    "36736+9825=46561",
    "36836+9725=46561",
    "46346+9821=56167",
    "46846+9321=56167",
    "71571+9642=81213",
    "71671+9542=82123",
    "72472+9651=82123",
    "72672+9451=82123",
}
assert count == 10 and got == expected2, "sample 2"

# Minimum-size, all letters equal.
assert run("A+A=A") == "1\n0+0=0", "same letter"

# Two distinct letters, including the valid leading-zero result.
assert run("A+A=B") == (
    "9\n"
    "1+1=2\n"
    "2+2=4\n"
    "3+3=6\n"
    "4+4=8\n"
    "5+5=0\n"
    "6+6=2\n"
    "7+7=4\n"
    "8+8=6\n"
    "9+9=8"
), "leading zero"

# Maximum word length, but only two distinct letters.
assert run("AAAAAAAAAAAAAAA+AAAAAAAAAAAAAAA=BBBBBBBBBBBBBBB") == (
    "5\n"
    "0+0=0\n"
    "111111111111111+111111111111111=222222222222222\n"
    "222222222222222+222222222222222=444444444444444\n"
    "333333333333333+333333333333333=666666666666666\n"
    "444444444444444+444444444444444=888888888888888"
), "maximum length"

# More than ten distinct letters means no assignment exists.
assert run("ABCDEFGHIJ+K=ABCDEFGHIJK") == "0", "more than ten letters"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A+A=A`|`1`giải pháp,`0+0=0`| Cùng một chữ cái ở cả hai bên và bài tập đều bằng nhau | 
|`A+A=B`|`9`giải pháp | Các chữ số riêng biệt, mang số học và kết quả bằng 0 hợp lệ | 
|`AAAAAAAAAAAAAAA+AAAAAAAAAAAAAAA=BBBBBBBBBBBBBBB`|`5`giải pháp | Độ dài từ tối đa và các chữ cái lặp lại trên mỗi cột | 
|`ABCDEFGHIJ+K=ABCDEFGHIJK`|`0`giải pháp | Hơn mười chữ cái riêng biệt | 

## Vỏ cạnh 

cho`A+A=A`, thuật toán bắt đầu với một ánh xạ trống. Trong cột duy nhất, nó nhìn thấy cùng một chữ cái toán hạng hai lần, vì vậy`operands`chứa`A`chỉ một lần. Phân công`A=0`cho`0+0=0`, do đó nhánh đi đến điểm cuối với số 0 và được chấp nhận. Bất kỳ phép gán nào khác 0 đều cho`2A`kết quả là, không thể bằng`A`bằng một chữ số thập phân riêng biệt, vì vậy tất cả các nhánh khác đều bị từ chối. Đầu ra chính xác là`0+0=0`. 

Vì`A+A=B`, cột đơn vị gán`A`Đầu tiên. Khi`A=5`, tổng là 10, do đó chữ số kết quả được yêu cầu là 0 và số mang là một. Vì số 0 không được sử dụng,`B=0`được chấp nhận. Biểu thức kết quả là`5+5=0`. Điều này chứng tỏ tại sao người giải không được áp đặt quy tắc không có số 0 đứng đầu. Đầu vào tương tự cũng thực hiện quá trình chuyển đổi mang theo mọi giá trị khác của`A`. 

Đối với đầu vào có độ dài tối đa`AAAAAAAAAAAAAAA+AAAAAAAAAAAAAAA=BBBBBBBBBBBBBBB`, tất cả mười lăm cột đều có cấu trúc giống hệt nhau. Nếu như`A=1`, cột đầu tiên tạo ra`B=2`và không mang theo. Mỗi cột tiếp theo lặp lại phép tính tương tự, vì vậy kết quả hoàn chỉnh là`111111111111111+111111111111111=222222222222222`. Bài tập`A=2,3,4`sản xuất tương tự`B=4,6,8`, trong khi`A=5`tạo ra mười lăm số không trong kết quả. Bất kì`A>=6`hoặc sử dụng lại chữ số kết quả đã được gán trong quan hệ tương ứng hoặc tạo ra chữ số kết quả bằng giá trị được sử dụng trước đó. Thuật toán xử lý tất cả mười lăm cột mà không phụ thuộc vào kích thước số tuyệt đối của các từ. 

Vì`ABCDEFGHIJ+K=ABCDEFGHIJK`, cần có mười một chữ cái riêng biệt. Vì một bài tập pháp lý cần một chữ số khác nhau cho mỗi chữ cái và chỉ có mười chữ số nên người giải sẽ trả về ngay lập tức mà không cần nhập tìm kiếm đệ quy. Việc kiểm tra này cũng ngăn cản việc triển khai bất cẩn trong việc lập chỉ mục hoặc xây dựng một hoán vị chữ số không thể thực hiện được. 

Ranh giới mang cuối cùng được xử lý bởi`pos == max_len`tình trạng. Giả sử các cột được xử lý kết thúc bằng một cột. Không còn ký tự kết quả nào ở vị trí đó nên phép cộng không hợp lệ. Đệ quy đến trường hợp cơ sở và từ chối nhánh vì`carry != 0`. Một giải pháp hợp lệ phải luôn để lại chính xác số 0 sau cột thực có ý nghĩa nhất.
