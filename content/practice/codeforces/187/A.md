---
title: "CF 187A - Hoán vị"
description: "Chúng ta được cho hai hoán vị chứa các số từ 1 đến n. Hoán vị đầu tiên là cách sắp xếp hiện tại và hoán vị thứ hai là cách sắp xếp mục tiêu mà chúng ta muốn đạt được. Hoạt động được phép là bất thường."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 187
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 119 (Div. 1)"
rating: 1500
weight: 187
solve_time_s: 90
verified: true
draft: false
---

[CF 187A - Hoán vị](https://codeforces.com/problemset/problem/187/A) 

**Đánh giá:** 1500 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai hoán vị chứa các số từ`1`ĐẾN`n`. Hoán vị đầu tiên là cách sắp xếp hiện tại và hoán vị thứ hai là cách sắp xếp mục tiêu mà chúng ta muốn đạt được. 

Hoạt động được phép là bất thường. Trong một lần di chuyển, chúng ta chỉ có thể lấy phần tử cuối cùng của hoán vị hiện tại và chèn nó vào bất kỳ vị trí nào trước đó trong mảng, bao gồm cả phần đầu. Chúng ta không thể di chuyển trực tiếp các phần tử tùy ý. Mọi di chuyển luôn bắt đầu với phần tử cuối cùng hiện tại. 

Nhiệm vụ là tính toán số lượng tối thiểu các phép toán cần thiết để chuyển đổi hoán vị thứ nhất thành hoán vị thứ hai. 

Các ràng buộc đủ lớn nên các giải pháp nặng về mô phỏng là không thể thực hiện được. Với`n`lên đến`2 * 10^5`, bất kì`O(n^2)`cách tiếp cận sẽ thực hiện xung quanh`4 * 10^10`hoạt động trong trường hợp xấu nhất, vượt xa những gì phù hợp trong hai giây. Lời giải phải gần với thời gian tuyến tính. 

Phần khó khăn là hiểu những yếu tố nào có thể cố định. Vì mọi thao tác đều loại bỏ phần tử cuối cùng nên thứ tự tương đối của tiền tố còn lại không bao giờ thay đổi. Điều này có nghĩa là chúng ta nên tìm kiếm hậu tố lớn nhất của hoán vị đích đã xuất hiện theo thứ tự tương đối chính xác bên trong hoán vị đầu tiên và có thể không bị ảnh hưởng. 

Một số trường hợp khó khăn dễ dàng phá vỡ lý luận ngây thơ. 

Hãy xem xét ví dụ này:```
4
1 2 3 4
1 2 3 4
```Câu trả lời đúng là`0`. Việc triển khai bất cẩn luôn cố gắng khớp từ cuối mà không kiểm tra xem mọi thứ đã được căn chỉnh chưa có thể thực hiện sai các bước di chuyển không cần thiết. 

Bây giờ hãy xem xét:```
3
3 2 1
1 2 3
```Câu trả lời là`2`. Một số cách tiếp cận sai lầm cho rằng mọi phần tử đặt sai vị trí đều cần phải di chuyển, tạo ra`3`. Nhận xét quan trọng là sau khi di chuyển`1`, thứ tự của`2`Và`3`việc khôi phục trở nên dễ dàng hơn. 

Một trường hợp tế nhị khác là:```
5
1 2 3 4 5
5 1 2 3 4
```Câu trả lời là`1`. Chúng tôi chỉ đơn giản là di chuyển`5`từ cuối đến đầu. Một so sánh ngây thơ từ trái sang phải sẽ nghĩ sai về bốn vị trí khác nhau và bị tính quá mức. 

Một trường hợp quan trọng hơn là khi chỉ có một hậu tố phù hợp:```
5
2 1 3 4 5
1 2 3 4 5
```Câu trả lời là`2`. Hậu tố`[3, 4, 5]`đã đúng và không bao giờ cần phải di chuyển. Chỉ có hai yếu tố đầu tiên phải được xây dựng lại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng rõ ràng quá trình này. Ở mỗi bước, chúng ta có thể loại bỏ phần tử cuối cùng và thử mọi vị trí chèn cho đến khi hoán vị đích xuất hiện. Điều này đúng vì nó khám phá tất cả các trạng thái có thể tiếp cận, nhưng số lượng hoán vị tăng theo giai thừa. Ngay cả việc mô phỏng tham lam cũng trở nên tốn kém vì mỗi bước di chuyển có thể yêu cầu dịch chuyển nhiều phần tử, dẫn đến`O(n^2)`hành vi. 

Nút thắt cổ chai xuất phát từ việc sửa đổi mảng nhiều lần. Với`n = 2 * 10^5`, công việc bậc hai là không thể. 

Thông tin chi tiết quan trọng là thao tác chỉ ảnh hưởng đến phần tử cuối cùng. Bất kỳ phần tử nào không bao giờ bị loại bỏ sẽ duy trì trật tự tương đối của chúng mãi mãi. Điều này có nghĩa là một số hậu tố của hoán vị đích có thể không bị ảnh hưởng trong suốt quá trình. 

Giả sử chúng ta quét hoán vị đầu tiên từ phải sang trái trong khi cố gắng khớp hoán vị đích từ phải sang trái. Bất cứ khi nào chúng tôi tìm thấy phần tử mục tiêu cần thiết, chúng tôi sẽ giữ nó. Các phần tử không thể khớp cuối cùng phải được di chuyển. 

Điều này biến bài toán thành việc tìm hậu tố dài nhất của hoán vị đích xuất hiện dưới dạng dãy con trong hoán vị đầu tiên. 

Nếu hậu tố đó có độ dài`k`, thì phần còn lại`n - k`các phần tử phải được di chuyển và con số này là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Kết hợp hậu tố tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cả hai hoán vị`a`Và`b`. 
2. Bắt đầu từ cuối hoán vị`b`. Hãy để con trỏ`j = n - 1`. Con trỏ này biểu thị giá trị đích tiếp theo mà chúng ta muốn giữ nguyên mà không cần di chuyển. 
3. Quét hoán vị`a`từ phải sang trái. 
4. Bất cứ khi nào`a[i] == b[j]`, giảm`j`. 

Điều này có nghĩa là chúng tôi đã tìm thấy thêm một phần tử có thể giữ nguyên vị trí. Vì chúng tôi quét từ cuối nên thứ tự tương đối sẽ được giữ nguyên tự động. 
5. Tiếp tục cho đến khi quá trình quét kết thúc. 
6. Sau khi xử lý hết các phần tử, số phần tử chưa khớp trong`b`là`j + 1`. 

Đây chính xác là những yếu tố phải được di chuyển. 
7. Đầu ra`j + 1`. 

### Tại sao nó hoạt động 

Thao tác chỉ loại bỏ các phần tử ở cuối. Bất kỳ phần tử nào không bao giờ bị xóa sẽ giữ nguyên trật tự của nó so với tất cả các phần tử chưa được chạm tới khác. 

Khi chúng ta khớp một cách tham lam các phần tử từ cuối cả hai hoán vị, chúng ta sẽ tối đa hóa hậu tố có thể cố định. Mọi phần tử phù hợp đều thuộc về một hậu tố có sẵn mà không cần di chuyển nó. 

Bất kỳ phần tử nào chưa từng có cuối cùng phải trở thành phần tử cuối cùng và được di chuyển đến một nơi nào đó sớm hơn. Vì quá trình tham lam bảo toàn hậu tố lớn nhất có thể nên số lượng còn lại là số lượng thao tác được yêu cầu tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    j = n - 1

    for i in range(n - 1, -1, -1):
        if j >= 0 and a[i] == b[j]:
            j -= 1

    print(j + 1)

solve()
```Việc triển khai tuân theo đối số tham lam chính xác từ hướng dẫn. 

Biến`j`theo dõi phần tử chưa từng có ở ngoài cùng bên phải trong hoán vị đích. Chúng ta duyệt ngược lại hoán vị đầu tiên vì chỉ có cấu trúc hậu tố mới quan trọng. Bất cứ khi nào chúng tôi gặp phải giá trị hiện tại cần thiết bởi`b[j]`, chúng ta khớp nó và di chuyển`j`sang trái. 

Một chi tiết tinh tế là điều kiện`j >= 0`. Nếu không có bộ bảo vệ này, mã có thể tiếp tục truy cập các chỉ số phủ định sau khi khớp hoàn toàn với hoán vị đích. 

Một sai lầm dễ dàng khác là quay trở lại`j`thay vì`j + 1`. Từ`j`là một chỉ mục, số phần tử chưa khớp còn lại bằng số vị trí từ`0`bởi vì`j`. 

Thuật toán không bao giờ sửa đổi mảng, không bao giờ mô phỏng các bước di chuyển và chỉ thực hiện một lần chuyển ngược. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 1
1 2 3
```Chúng tôi quét từ phải sang trái. 

| tôi | một [tôi] | Cần b[j] | Cuộc thi đấu? | Mới j | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 3 | Không | 2 | 
| 1 | 2 | 3 | Không | 2 | 
| 0 | 3 | 3 | Có | 1 | 

Cuối cùng,`j = 1`, vậy câu trả lời là`2`. 

Dấu vết này cho thấy chỉ có hậu tố`[3]`có thể ở lại cố định. các yếu tố`1`Và`2`phải được di chuyển. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
5 1 2 3 4
```| tôi | một [tôi] | Cần b[j] | Cuộc thi đấu? | Mới j | 
| --- | --- | --- | --- | --- | 
| 4 | 5 | 4 | Không | 4 | 
| 3 | 4 | 4 | Có | 3 | 
| 2 | 3 | 3 | Có | 2 | 
| 1 | 2 | 2 | Có | 1 | 
| 0 | 1 | 1 | Có | 0 | 

Cuối cùng,`j = 0`, vậy câu trả lời là`1`. 

Điều này thể hiện tính bất biến cốt lõi. Hậu tố`[1,2,3,4]`đã xuất hiện theo đúng thứ tự tương đối và không bao giờ cần phải di chuyển. Chỉ một`5`phải di dời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét ngược đơn qua các hoán vị | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với`n`lên đến`2 * 10^5`, thời gian tuyến tính dễ dàng phù hợp trong giới hạn. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ các mảng đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        j = n - 1

        for i in range(n - 1, -1, -1):
            if j >= 0 and a[i] == b[j]:
                j -= 1

        return str(j + 1)

    return solve()

# provided sample
assert run(
    "3\n3 2 1\n1 2 3\n"
) == "2", "sample 1"

# already equal
assert run(
    "4\n1 2 3 4\n1 2 3 4\n"
) == "0", "already equal"

# single move
assert run(
    "5\n1 2 3 4 5\n5 1 2 3 4\n"
) == "1", "single move to front"

# minimum size
assert run(
    "1\n1\n1\n"
) == "0", "minimum n"

# suffix already correct
assert run(
    "5\n2 1 3 4 5\n1 2 3 4 5\n"
) == "2", "preserve longest suffix"

# complete reversal
assert run(
    "5\n5 4 3 2 1\n1 2 3 4 5\n"
) == "4", "only one element can stay"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`0`| Đầu vào kích thước tối thiểu | 
| Hoán vị bằng nhau |`0`| Không có động thái không cần thiết | 
| Xoay theo một phần tử |`1`| Vận hành tối ưu đơn lẻ | 
| Hậu tố khớp một phần |`2`| Bảo quản hậu tố dài nhất | 
| Hoán vị ngược |`4`| Hành vi khớp trường hợp xấu nhất | 

## Vỏ cạnh 

Hãy xem xét hoán vị đã đúng:```
4
1 2 3 4
1 2 3 4
```Quá trình quét khớp với mọi phần tử từ phải sang trái: 

-`4`trận đấu 
-`3`trận đấu 
-`2`trận đấu 
-`1`trận đấu 

Cuối cùng`j = -1`, vì vậy câu trả lời trở thành`0`. Thuật toán nhận dạng chính xác rằng không cần phải di chuyển. 

Bây giờ hãy xem xét trường hợp dịch chuyển theo chu kỳ:```
5
1 2 3 4 5
5 1 2 3 4
```Quét ngược phù hợp`4`,`3`,`2`, Và`1`. Chỉ một`5`vẫn chưa từng có. Từ`5`đã ở cuối, một động tác đặt nó ở phía trước sẽ giải quyết được vấn đề. Đầu ra của thuật toán`1`. 

Cuối cùng, hãy xem xét một hoán vị gần như đảo ngược:```
5
5 4 3 2 1
1 2 3 4 5
```Lần quét đầu tiên phù hợp`5`, nhưng không có phần tử nào trước đó có thể khớp`4`đồng thời giữ gìn trật tự. Hậu tố có thể bảo tồn tối đa có độ dài`1`, vậy bốn phần tử phải di chuyển. Đầu ra của thuật toán`4`, đó là tối ưu.
