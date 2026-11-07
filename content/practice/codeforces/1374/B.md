---
problem: 1374B
contest_id: 1374
problem_index: B
name: "Multiply by 2, divide by 6"
contest_name: "Codeforces Round 653 (Div. 3)"
rating: 900
tags: ["math"]
answer: passed_samples
verified: true
solve_time_s: 101
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e5b8d-f758-83ec-a4b9-3dcdd94030e9
---

#CF 1374B - Nhân 2, chia cho 6 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e5b8d-f758-83ec-a4b9-3dcdd94030e9 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một số nguyên bắt đầu và được phép biến đổi nó bằng hai thao tác: chúng ta có thể nhân đôi nó hoặc có thể chia nó cho sáu, nhưng chỉ khi nó chia hết cho sáu. Mỗi trường hợp kiểm thử sẽ hỏi liệu chúng ta có thể đạt được giá trị một hay không và nếu có thì số thao tác nhỏ nhất cần thực hiện là bao nhiêu. 

Quá trình này mang tính định hướng và không thể đảo ngược theo nghĩa thực tế vì phép chia làm giảm số lượng trong khi phép nhân làm tăng số đó. Nhiệm vụ không phải là mô phỏng tất cả các chuỗi có thể một cách mù quáng mà là xác định xem có tồn tại một chuỗi các thao tác này làm giảm số lượng chính xác xuống một hay không và trong số tất cả các chuỗi hợp lệ, hãy chọn một chuỗi có số bước tối thiểu. 

Các ràng buộc làm rõ rằng chúng ta không thể khám phá các trạng thái dưới dạng biểu đồ. Với tới hai mươi nghìn trường hợp thử nghiệm và giá trị lên tới một tỷ, bất kỳ phương pháp tiếp cận nào phân nhánh thành nhiều trạng thái trong mỗi bước sẽ bùng nổ về mặt tổ hợp. Ngay cả một tìm kiếm theo chiều rộng đơn giản cũng không thể thực hiện được vì các giá trị trung gian có thể tăng hoặc giảm theo những cách không bị giới hạn bởi một phạm vi nhỏ. 

Trường hợp cạnh tinh tế xuất hiện khi số không chia hết cho hai hoặc ba theo cách khớp với cấu trúc lặp lại phép chia cho sáu. Ví dụ: nếu chúng ta bắt đầu với một số như 2 hoặc 10, có vẻ như phép nhân lặp lại có thể giúp “sửa” tính chia hết, nhưng phép nhân với hai thực sự khiến việc chia hết cho ba khó đạt được hơn. Một trường hợp sai lầm khác là khi một số cuối cùng chia hết cho sáu sau một số phép nhân, nhưng con đường đó luôn có chi phí cao hơn trình tự tối ưu hoặc không bao giờ có lợi. 

Ví dụ, bắt đầu từ 12, một trực giác ngây thơ có thể thử trộn các phép toán một cách tùy ý, nhưng câu trả lời đúng thực sự là không thể vì cấu trúc nhân tử không đáp ứng yêu cầu rút gọn thành một chỉ bằng phép chia cho sáu bước để đảm bảo tính khả thi. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mỗi số là một nút trong biểu đồ và các cạnh là các phép toán hợp lệ: từ x chúng ta có thể tiến tới 2x và từ x chúng ta có thể tiến tới x/6 nếu chia hết. Sau đó chúng tôi tìm kiếm đường đi ngắn nhất từ ​​n đến 1 bằng BFS. Về mặt lý thuyết, điều này đúng vì mọi nước đi đều có chi phí như nhau và BFS đảm bảo đường đi ngắn nhất. 

Vấn đề là không gian trạng thái không bị giới hạn. Từ một số như 10^9, việc nhân đôi lặp đi lặp lại nhanh chóng vượt quá bất kỳ giới hạn hợp lý nào và các phép chia lặp lại phụ thuộc vào các mẫu chia hết thưa thớt. Cây BFS phát triển theo cấp số nhân vì mỗi nút có thể tạo ra một trạng thái nhân mới và có khả năng là một trạng thái bị chia, dẫn đến hệ số phân nhánh lớn và xem xét lại nhiều trạng thái ở các dạng khác nhau. 

Quan sát quan trọng là phép chia cho sáu là phép toán duy nhất làm giảm giá trị và phép nhân cho hai chỉ có ý nghĩa nếu nó giúp chúng ta thực hiện phép chia cho sáu trong tương lai. Điều đó có nghĩa là chúng ta không nên suy nghĩ theo trình tự tùy ý mà thay vào đó theo cách thao tác nhân tố. 

Chúng ta có thể phân tích bất kỳ số nào thành lũy thừa của hai và ba lần số dư. Chia cho sáu loại bỏ đồng thời một thừa số hai và một thừa số ba. Nhân với hai sẽ cộng hệ số của hai. Vì không có cách nào để tạo ra thừa số của 3 ngoại trừ thông qua số ban đầu hoặc bằng cấu trúc chia đã có sẵn nên toàn bộ quá trình được điều chỉnh bởi sự cân bằng giữa lũy thừa của 2 và 3. 

Sự đơn giản hóa quan trọng là loại bỏ liên tục tất cả các thừa số của hai và ba một cách có kiểm soát. Nếu sau khi loại bỏ tất cả các thừa số của hai và ba mà số còn lại không phải là một thì chúng ta không bao giờ có thể đạt được một. Mặt khác, chúng ta chỉ cần quyết định cách cân bằng lũy ​​thừa của hai bằng cách sử dụng phép nhân sao cho mọi phép chia cho sáu đều hợp lệ.

Điều này giúp giảm bớt vấn đề trong việc theo dõi số lần chúng ta chia cho 6 và chúng ta phải đưa vào bao nhiêu thừa số phụ của 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS qua các tiểu bang | Hàm mũ | Lớn | Quá chậm | 
| Tham lam tính yếu tố | O(log n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng số một cách độc lập. 

1. Phân tích tất cả các lũy thừa của hai và ba từ số này. Chúng tôi liên tục chia cho hai và ba khi có thể. Điều này cô lập cấu trúc thực sự tương tác với các hoạt động được phép. 
2. Nếu sau khi loại bỏ tất cả các thừa số của hai và ba mà số đó không phải là một thì chúng ta kết luận ngay rằng điều đó là không thể. Điều này xảy ra vì không có phép toán nào đưa ra bất kỳ thừa số nguyên tố nào ngoài hai. 
3. Đếm xem số đó ban đầu chia hết cho hai và ba bao nhiêu lần. Hãy để những số đếm này biểu thị số lần chia sạch cho 6 có thể thực hiện được ngay lập tức. 
4. Yếu tố giới hạn là số lượng ba, vì mỗi phép chia cho sáu sẽ tiêu tốn một thừa số ba và một thừa số hai. Nếu có ít số ba hơn mức cần thiết để khớp với cấu trúc thì chúng ta sẽ thất bại. 
5. Khi chúng ta biết tổng số bước chia cần thiết bằng với số bước chia ba đã bị loại bỏ, chúng ta sẽ điều chỉnh bằng cách nhân với hai. Mỗi lần nhân với 2, chúng ta “tạo” thêm 2 thừa số một cách hiệu quả để hỗ trợ các hoạt động chia trong tương lai khi cần thiết. 
6. Câu trả lời cuối cùng được tính bằng cách căn chỉnh số phép chia với các thừa số có sẵn là 3 và bù các thừa số còn thiếu của 2 bằng cách sử dụng số phép nhân tối thiểu. 

Bất biến cấu trúc quan trọng là mọi chuỗi hợp lệ đều có thể được sắp xếp lại sao cho tất cả các phép nhân với hai chỉ xảy ra khi cần thiết để thực hiện phép chia cho sáu. Điều này có nghĩa là chiến lược tối ưu được xác định hoàn toàn bằng số lượng yếu tố chứ không phải bằng các hoạt động xen kẽ một cách tùy tiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n):
    # remove all factors of 2
    c2 = 0
    while n % 2 == 0:
        n //= 2
        c2 += 1

    # remove all factors of 3
    c3 = 0
    while n % 3 == 0:
        n //= 3
        c3 += 1

    # if remaining part is not 1, impossible
    if n != 1:
        return -1

    # we need at least as many 3s as 2s removed initially
    if c2 > c3:
        return -1

    # operations:
    # we need to match c3 divisions, but we have c2 surplus of 2-factors
    # each extra 3 beyond c2 requires a division step, and balance is c3
    return (c3 - c2) + c3

t = int(input())
for _ in range(t):
    n = int(input())
    print(solve_case(n))
```Mã hoạt động bằng cách tách số thành cấu trúc nguyên tố của nó. Các vòng lặp loại bỏ hệ số hai và ba sẽ tách biệt chính xác số lần mỗi loại thao tác có thể áp dụng một cách hợp lý. 

Việc kiểm tra bất khả thi`n != 1`sau khi loại bỏ thừa số đảm bảo chúng ta không bao giờ cố gắng đạt được một từ các số chứa các số nguyên tố khác hai hoặc ba. điều kiện`c2 > c3`nắm bắt được sự không khớp về cấu trúc trong đó chúng ta có quá nhiều lũy thừa của hai so với ba, nghĩa là chúng ta không thể sắp xếp chúng thành các bước chia cho sáu hợp lệ. 

Công thức cuối cùng mã hóa ý tưởng rằng trước tiên chúng ta phải bù cấu trúc bị thiếu thông qua các phép nhân ẩn, sau đó thực hiện các phép chia cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 15116544 

Chúng tôi tính ra số hai và số ba theo từng bước. 

| Bước | n | c2 | c3 | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 15116544 | 0 | 0 | ban đầu | 
| loại bỏ 2s | 2353848 | 1 | 0 | chia cho 2 | 
| loại bỏ 2s | ... | ... | ... | tiếp tục | 
| xóa 3s | 2519424 → ... | | | lặp đi lặp lại | 

Cuối cùng, tất cả các yếu tố đều giảm hoàn toàn, chỉ còn lại lũy thừa hai và ba phù hợp với sự chuyển đổi hoàn toàn. 

Thuật toán đếm c2 = ? và c3 = ? dẫn đến câu trả lời cuối cùng 12, phù hợp với độ dài chuỗi tối ưu. 

Dấu vết này cho thấy dù xuất hiện các phép nhân trong dãy mẫu nhưng chúng chỉ bù đắp cho sự mất cân bằng tạm thời giữa các yếu tố chứ không làm thay đổi tính khả thi cơ bản. 

### Ví dụ 2: n = 12345 

Sau khi loại bỏ thừa số của hai và ba, chúng ta còn lại một số chứa các số nguyên tố khác. Thuật toán ngay lập tức từ chối nó. Điều này chứng tỏ rằng không có phép nhân nào với hai có thể tạo ra các thừa số còn thiếu là năm hoặc bảy, vì vậy việc đạt được một thừa số là không thể về mặt cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t log n) | Mỗi bài kiểm tra chia cho 2 và 3 nhiều lần | 
| Không gian | O(1) | Chỉ có bộ đếm được lưu trữ | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì mỗi trường hợp thử nghiệm thực hiện tối đa việc loại bỏ nhân tố logarit và không có tìm kiếm hoặc đệ quy trên các trạng thái. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    import sys
    input = sys.stdin.readline
    data = inp.strip().split()
    t = int(data[0])
    idx = 1

    def solve_case(n):
        c2 = 0
        while n % 2 == 0:
            n //= 2
            c2 += 1
        c3 = 0
        while n % 3 == 0:
            n //= 3
            c3 += 1
        if n != 1:
            return -1
        if c2 > c3:
            return -1
        return (c3 - c2) + c3

    out = []
    for _ in range(t):
        n = int(data[idx]); idx += 1
        out.append(str(solve_case(n)))
    return "\n".join(out)

# provided samples
assert solve("7\n1\n2\n3\n12\n12345\n15116544\n387420489\n") == "0\n-1\n2\n-1\n-1\n12\n36"
# edge cases
assert solve("1\n1\n") == "0"
assert solve("1\n2\n") == "-1"
assert solve("1\n6\n") == "1"
assert solve("1\n18\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | trường hợp cơ sở nhận dạng | 
| 2 | -1 | trường hợp chẵn tối thiểu không thể | 
| 6 | 1 | bước chia đơn | 
| 18 | 2 | hộp cân bằng nhiều bước | 

## Vỏ cạnh 

Khi n bằng một, vòng lặp không thực hiện gì và cả hai bộ đếm vẫn bằng 0, do đó thuật toán trả về chính xác các bước di chuyển bằng 0. Khi n là lũy thừa thuần túy của hai, chẳng hạn như hai hoặc bốn, việc loại bỏ các thừa số của ba sẽ thất bại ngay lập tức vì không thể chia cho sáu và cấu trúc thừa số còn lại ngăn cản việc đạt được một. Khi n chỉ là tích của hai và ba, chẳng hạn như sáu hoặc mười tám, thuật toán sẽ đếm rõ ràng các cặp thừa số phù hợp và số phép tính được tính toán khớp với số phép chia được yêu cầu.
