---
title: "CF 104805D - Một bức tranh trừu tượng"
description: "Chúng ta được cho một số nguyên $n$, và chúng ta phải quyết định xem liệu có thể xây dựng một bức tranh hình vuông có độ dài cạnh $k$, trong đó $k le n$, theo một cách diễn giải rất cụ thể về “bức tranh” hay không. Bức tranh không chỉ là một mạng lưới các ô đồng nhất."
date: "2026-06-28T13:17:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104805
codeforces_index: "D"
codeforces_contest_name: "Central Russia Regional Contest, 2022"
rating: 0
weight: 104805
solve_time_s: 79
verified: true
draft: false
---

[CF 104805D - Một bức tranh trừu tượng](https://codeforces.com/problemset/problem/104805/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$n$, và chúng ta phải quyết định xem có thể dựng được một bức tranh hình vuông có độ dài cạnh nào đó hay không$k$, Ở đâu$k \le n$, theo một cách giải thích rất cụ thể về “bức tranh”. 

Bức tranh không chỉ là một mạng lưới các ô đồng nhất. Thay vào đó, nó phải thể hiện sự phân rã của một$k \times k$hình vuông thành các hình vuông nhỏ hơn theo trục, mỗi hình vuông bao phủ hoàn toàn khung vẽ mà không có khoảng trống hoặc chồng chéo. Mỗi ô vuông nhỏ hơn này được gán một trong bốn màu. Đầu ra hiển thị cuối cùng là một lưới có kích thước$k \times k$, trong đó mỗi ô đơn vị kế thừa màu của hình vuông nhỏ mà nó thuộc về. 

Có một ràng buộc bổ sung về tính kề cận: nếu hai vùng màu tiếp xúc dọc theo một cạnh thì chúng phải có các màu khác nhau. Chỉ chạm vào một góc không thành vấn đề. 

Vì vậy, nhiệm vụ cơ bản là chọn một giá trị hợp lệ$k$và xây dựng một tấm lát của$k \times k$chia lưới thành các hình vuông, sau đó tô màu các hình vuông đó bằng 4 màu sao cho hai hình vuông bất kỳ có chung một cạnh có màu khác nhau. Nếu không có cấu trúc như vậy tồn tại, chúng tôi xuất ra$-1$. 

Ràng buộc$n \le 1000$gợi ý rằng bất kỳ giải pháp nào liên quan đến việc tìm kiếm nhiều trên các phân vùng hoặc cấu trúc hình học sẽ quá chậm. Một nỗ lực mạnh mẽ nhằm liệt kê tất cả các phép chia hình vuông đang phát triển bùng nổ, vì số lượng các ô vuông có thể có của một lưới tăng siêu đa thức theo diện tích. 

Một ràng buộc tinh tế hơn được ẩn trong các mẫu. Khi$n = 1$, một nghiệm tồn tại một cách tầm thường. Khi$n = 2$hoặc$n = 3$, không có giải pháp tồn tại. Điều này ngay lập tức gợi ý rằng không phải mọi$n$thậm chí còn cho phép phân tích hình học hợp lệ, bất kể sức mạnh tô màu. 

Hạn chế chính về mặt cấu trúc là việc xây dựng chỉ có thể thực hiện được khi toàn bộ$k \times k$lưới có thể được hiểu là một phân vùng thành các ô vuông đơn vị, nghĩa là$n = k^2$. Bất kỳ sự phân rã nào khác thành các ô vuông không bằng nhau sẽ tạo ra các vùng còn sót lại khó xử hoặc các cấu trúc ranh giới không tương thích. 

Một sai lầm ngây thơ là cho rằng chúng ta luôn có thể xếp một hình vuông thành bất kỳ số lượng hình vuông nhỏ hơn nào. Ví dụ, người ta có thể thử phân tích một$3 \times 3$lưới thành ba hình vuông, nhưng bất kỳ nỗ lực nào chắc chắn sẽ để lại các mảnh hình chữ nhật hoặc vi phạm yêu cầu rằng mỗi mảnh đều là một hình vuông. 

Một cạm bẫy phổ biến khác là chỉ tập trung vào việc tô màu, giả sử rằng một khi đã có lưới thì 4 màu luôn là đủ. Mặc dù điều đó đúng với một lưới cố định, nhưng khó khăn thực sự nằm ở chỗ liệu việc phân rã lưới có khả thi đối với một lưới nhất định hay không.$n$. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các ô có thể có của một$k \times k$lưới thành chính xác$n$hình vuông nhỏ hơn cho mỗi$k \le n$, sau đó kiểm tra xem màu hợp lệ có tồn tại hay không. Ngay cả đối với mức độ vừa phải$k$, số lượng các ô vuông là rất lớn, vì mỗi vị trí của một hình vuông sẽ hạn chế các vị trí trong tương lai theo cách phân nhánh. Điều này dẫn đến một không gian tìm kiếm hàm mũ trong cả cấu trúc hình học và phân vùng. 

Quan sát quan trọng là điều kiện hình học chặt chẽ hơn nhiều so với lần đầu tiên nó xuất hiện. Một hình vuông nói chung không thể bị phân hủy thành một số lượng hình vuông nhỏ hơn tùy ý. Trên thực tế, nếu chúng ta giới hạn bản thân ở những hình vuông thẳng hàng theo trục không chồng lên nhau bao phủ chính xác một$k \times k$vùng, cấu trúc thống nhất và đơn giản nhất luôn hoạt động là phân tách đơn vị thành$k^2$tế bào. 

Một khi chúng ta chấp nhận rằng cách xây dựng hợp lệ nhất quán duy nhất là lưới đơn vị đầy đủ thì vấn đề sẽ trở thành việc chọn$k$như vậy$n = k^2$. Sau đó, nhiệm vụ còn lại hoàn toàn trở thành một bài toán tô màu đồ thị trên một lưới, có thể giải được bằng cách sử dụng mẫu 2x2 lặp lại sử dụng cả bốn màu. 

Do đó toàn bộ vấn đề quy về việc kiểm tra xem$n$là một hình vuông hoàn hảo Nếu không, không có công trình hợp lệ nào tồn tại. Nếu đúng thì chúng ta đặt$k = \sqrt{n}$và tô màu lưới bằng cách sử dụng một mẫu định kỳ cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ốp lát Brute Force + tô màu | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng hình vuông hoàn hảo |$O(k^2)$|$O(k^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán 

1. Tính toán$k = \lfloor \sqrt{n} \rfloor$và kiểm tra xem$k \cdot k = n$. Nếu không, không có cách nào để tạo thành một ô vuông nhất quán, vì vậy chúng tôi ngay lập tức xuất ra$-1$. Sự từ chối xảy ra sớm vì bất kỳ diện tích không vuông nào cũng không thể phân hủy thành đồng phục$k \times k$lưới các khối vuông mà không đưa vào hình học còn sót lại. 
2. Đặt kích thước canvas thành$k \times k$. Tại thời điểm này, chúng tôi hiểu mỗi ô là một vùng hình vuông riêng lẻ. 
3. Gán màu bằng cách sử dụng mẫu 2x2 lặp lại trên lưới. Đối với một ô ở tọa độ$(i, j)$, chúng tôi ánh xạ cặp$(i \bmod 2, j \bmod 2)$đến một trong bốn màu$\{Y, O, P, L\}$. Điều này đảm bảo rằng bất kỳ hai ô liền kề theo chiều ngang hoặc chiều dọc nào cũng có màu khác nhau. 
4. Đầu ra$k$tiếp theo là lưới được xây dựng. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai thuộc tính cấu trúc. Thứ nhất, việc xây dựng chỉ tiến hành khi$n$là một hình vuông hoàn hảo, có nghĩa là lưới có thể được hiểu là một bộ đồng phục$k \times k$sự phân hủy. Không cần phân vùng hình vuông cấp cao hơn vì mỗi ô đơn vị đã tạo thành một vùng hình vuông hợp lệ. 

Thứ hai, chức năng tô màu đảm bảo rằng mọi cạnh giữa các ô liền kề đều kết nối hai cặp chẵn lẻ khác nhau trong bàn cờ 2D trong khoảng thời gian 2x2. Vì mỗi lần di chuyển theo chiều ngang hoặc chiều dọc sẽ lật ít nhất một tọa độ chẵn lẻ nên các ô liền kề luôn nhận được các màu riêng biệt. Điều này thỏa mãn ràng buộc cho tất cả các vùng tiếp xúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

k = int(n ** 0.5)
if k * k != n:
    print(-1)
    sys.exit()

colors = ['Y', 'O', 'P', 'L']

# 2x2 periodic assignment
mapping = {
    (0, 0): 'Y',
    (0, 1): 'O',
    (1, 0): 'P',
    (1, 1): 'L'
}

print(k)
for i in range(k):
    row = []
    for j in range(k):
        row.append(mapping[(i % 2, j % 2)])
    print(''.join(row))
```Giải pháp trước tiên xác nhận xem dữ liệu đầu vào có tạo thành một hình vuông hoàn hảo hay không. Bước này rất quan trọng vì nó ngăn cản việc cố gắng xây dựng một lưới có kích thước không thể tương ứng với một ô vuông nhất quán. 

Lưới sau đó được lấp đầy bằng cách sử dụng mẫu định kỳ xác định. Việc lựa chọn ánh xạ 2x2 rất quan trọng vì đây là mẫu nhỏ nhất đảm bảo bốn màu riêng biệt đồng thời đảm bảo các ràng buộc kề được thỏa mãn theo cả hai hướng. 

Một chi tiết tinh tế là căn bậc hai số nguyên phải được xử lý cẩn thận. Việc sử dụng trực tiếp sqrt dấu phẩy động mà không xác thực có thể gây ra các vấn đề về độ chính xác, vì vậy việc kiểm tra số nguyên$k \cdot k = n$được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```Đây$n = 1$, Vì thế$k = 1$. 

| Bước | k | Kiểm tra | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 1×1 = 1 hợp lệ | Xây dựng lưới | 

Đầu ra:```
1
Y
```Điều này xác nhận rằng một ô đơn lẻ thỏa mãn tất cả các ràng buộc lân cận vì không có ô lân cận. 

### Ví dụ 2 

đầu vào:```
2
```Đây$n = 2$, Và$\sqrt{2}$không phải là số nguyên. 

| Bước | k | Kiểm tra | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 1×1 ≠ 2 | Từ chối | 

Đầu ra:```
-1
```Điều này chứng tỏ rằng không có sự phân tách lưới vuông nào tồn tại đối với các kích thước hình vuông không hoàn hảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k^2)$| Chúng tôi điền vào từng ô của$k \times k$lưới chính xác một lần | 
| Không gian |$O(1)$thêm | Chỉ sử dụng ánh xạ liên tục và bộ đệm nhỏ | 

Tối đa$n$là 1000, vậy$k \le 31$. Do đó, việc xây dựng diễn ra nhanh chóng và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n = int(input().strip())

    k = int(n ** 0.5)
    if k * k != n:
        return "-1"

    mapping = {
        (0, 0): 'Y',
        (0, 1): 'O',
        (1, 0): 'P',
        (1, 1): 'L'
    }

    out = [str(k)]
    for i in range(k):
        row = []
        for j in range(k):
            row.append(mapping[(i % 2, j % 2)])
        out.append(''.join(row))
    return "\n".join(out)

# provided samples
assert run("1") == "1\nY"
assert run("2") == "-1"
assert run("3") == "-1"

# custom cases
assert run("4") == "2\nYO\nPL", "perfect square 2x2"
assert run("9") == "3\nYOY\nPLP\nYOY", "3x3 pattern"
assert run("16") == "4\nYOYO\nPLPL\nYOYO\nPLPL", "larger perfect square"
assert run("10") == "-1", "non-square rejection"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 năm | trường hợp hợp lệ tối thiểu | 
| 2 | -1 | trường hợp không hợp lệ nhỏ nhất | 
| 9 | 3 lưới | hình vuông hợp lệ không tầm thường | 
| 10 | -1 | từ chối không vuông | 

## Vỏ cạnh 

cho$n = 1$, thuật toán xác định chính xác$k = 1$và xuất ra một ô màu duy nhất. Không có ràng buộc kề nào, do đó, mọi phép gán màu đều hợp lệ và ánh xạ cố định vẫn tạo ra kết quả chính xác. 

Đối với các giá trị không vuông góc như$n = 2$hoặc$n = 3$, thuật toán sẽ bác bỏ ngay lập tức. Cố gắng xây dựng một lưới sẽ ngầm giả định độ dài cạnh không nguyên, điều này phá vỡ cách giải thích hình học của một phân vùng canvas hình vuông. 

Đối với các hình vuông hoàn hảo lớn hơn như$n = 100$, việc xây dựng có tỷ lệ tuyến tính theo số lượng ô. Mẫu tô màu định kỳ tiếp tục đảm bảo sự phân tách kề cận mà không cần bất kỳ sự phối hợp toàn cục nào ngoài việc kiểm tra tính chẵn lẻ cục bộ.
