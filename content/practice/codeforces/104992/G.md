---
title: "CF 104992G - \u041c\u0435\u0434\u0432\u0435\u0434\u044c \u0438 \u043f\u0440\u0430\u0432\u0438\u043b\u044c\u043d\u043e\u0435 \u043f\u0438\u0442\u0430\u043d\u0438\u0435"
description: "Chúng ta được cung cấp một bộ sưu tập các mặt hàng thực phẩm, mỗi mặt hàng được mô tả bằng thời gian nó biến mất khỏi nhà kho và lượng calo mà nó cung cấp. Thời gian di chuyển theo từng giờ riêng biệt bắt đầu từ thời điểm con gấu đến."
date: "2026-06-28T04:28:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104992
codeforces_index: "G"
codeforces_contest_name: "qual VKOSHP Junior 24"
rating: 0
weight: 104992
solve_time_s: 73
verified: false
draft: false
---

[CF 104992G - \u041c\u0435\u0434\u0432\u0435\u0434\u044c \u0438 \u043f\u0440\u0430\u0432\u0438\u043b\u044c\u043d\u043e\u0435 \u043f\u0438\u0442\u0430\u043d\u0438\u0435](https://codeforces.com/problemset/problem/104992/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập các mặt hàng thực phẩm, mỗi mặt hàng được mô tả bằng thời gian nó biến mất khỏi nhà kho và lượng calo mà nó cung cấp. Thời gian di chuyển theo từng giờ riêng biệt bắt đầu từ thời điểm con gấu đến. Trong mỗi giờ, con gấu có thể ăn tối đa một món đồ và khi thời hạn của món đồ đó trôi qua thì món đồ đó sẽ biến mất vĩnh viễn. 

Mỗi mục hoạt động giống như một công việc phải được thực hiện trong khoảng thời gian một giờ trước thời hạn của nó. Mục tiêu là chọn và sắp xếp một tập hợp con các công việc này sao cho không có hai công việc nào chiếm cùng một giờ và không có công việc nào được chọn được lên lịch sau thời hạn, đồng thời tối đa hóa tổng lượng calo thu được. 

Đầu vào cung cấp cho mỗi mục một thời hạn tính bằng giờ và một giá trị. Thời hạn có nghĩa là chỉ số giờ cuối cùng mà mặt hàng đó có thể được tiêu thụ nếu chúng ta lập chỉ mục số giờ từ 0. Đầu ra là tổng giá trị tối đa có thể có trong số tất cả các mục có thể được lên lịch khả thi theo các ràng buộc này. 

Các ràng buộc rất lớn, lên tới 200.000 mặt hàng. Bất kỳ cách tiếp cận nào cố gắng mô phỏng mỗi giờ hoặc liên tục tìm kiếm mục tốt nhất trong mỗi khoảng thời gian sẽ không thành công vì nó sẽ suy giảm hành vi bậc hai. Các giải pháp khả thi duy nhất phải chạy ở khoảng O(n log n). 

Một trường hợp thất bại tinh tế do lý luận ngây thơ xuất hiện khi chọn các mặt hàng một cách tham lam theo giá trị mà không tôn trọng thời hạn. Ví dụ: nếu một mặt hàng có giá trị cao có thời hạn rất sớm và một số mặt hàng nhỏ hơn có thời hạn muộn hơn, việc chọn sai có thể chặn lịch trình cho phép tổng giá trị cao hơn. Một trường hợp thất bại khác phát sinh khi sắp xếp theo thời hạn và luôn lấy những món có sẵn mà không theo dõi những lựa chọn nào trước đó nên được thay thế bằng những lựa chọn tốt hơn. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là xem xét mọi tập hợp con có thể có của các mục và đối với mỗi tập hợp con, hãy cố gắng gán chúng vào các khoảng thời gian trước thời hạn của chúng. Ngay cả khi chúng ta sửa một tập hợp con, việc lập kế hoạch cho nó đòi hỏi phải kiểm tra xem liệu nó có thể được sắp xếp theo thứ tự thời gian tăng dần mà không vi phạm thời hạn hay không. Điều này dẫn đến các tập hợp con theo cấp số nhân và thậm chí việc xác minh từng tập hợp con cũng mất thời gian tuyến tính, tạo ra vụ nổ quy mô 2^n không thể thực hiện được. 

Một nỗ lực có cấu trúc hơn là xử lý thời gian theo từng giờ và tại mỗi giờ hãy chọn mục tốt nhất có sẵn. Điều này không thành công vì “có sẵn” thay đổi linh hoạt theo thời hạn và việc duy trì mức tốt nhất toàn cầu mà không xem lại các quyết định trong quá khứ sẽ dẫn đến những lựa chọn sai lầm. Ý tưởng còn thiếu là chúng ta thực sự không quan tâm đến thời gian chính xác mỗi món được ăn, mà chỉ quan tâm đến việc liệu chúng ta có thể đáp ứng đủ một bộ kích thước k trong thời hạn hay không và k món nào là tốt nhất. 

Nhận xét quan trọng là đây là một bài toán tối đa hóa lịch trình có thời hạn cổ điển. Nếu chúng ta sắp xếp các mục theo thời hạn tăng dần thì khi chúng ta đã xem xét tất cả các mục có giá trị thời hạn d nào đó, chúng ta được phép chọn nhiều nhất d mục trong số đó. Trong số tất cả các cách chọn món d, lựa chọn tối ưu luôn là món d có lượng calo cao nhất được thấy cho đến nay. Nếu vượt quá số lượng cho phép, chúng ta nên loại bỏ vật phẩm có giá trị thấp nhất vì nó đóng góp ít nhất vào mục tiêu trong khi chiếm một vị trí khan hiếm. 

Điều này trực tiếp dẫn đến việc duy trì một tập hợp các mục đã chọn bằng cách sử dụng cấu trúc có thể loại bỏ giá trị nhỏ nhất một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tham lam với đống | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sắp xếp tất cả các mục theo thời hạn của chúng theo thứ tự không giảm. Sau đó, chúng tôi quét từng cái một, duy trì cấu trúc các giá trị calo đã chọn.

1. Sắp xếp các mục theo thời hạn tăng dần. Điều này đảm bảo rằng khi chúng tôi xử lý một mục, chúng tôi đã biết ràng buộc chặt chẽ nhất áp dụng cho tiền tố hiện tại của mục. 
2. Duy trì lượng calo tối thiểu đã chọn. Đống này đại diện cho tập hợp các mục được chọn hiện tại mà chúng tôi dự định lên lịch. 
3. Đối với mỗi mục theo thứ tự sắp xếp, hãy điền giá trị calo của nó vào đống. Điều này tương ứng với việc tạm thời đưa mục này vào lịch trình của chúng tôi. 
4. Sau khi chèn, hãy kiểm tra xem kích thước heap có vượt quá thời hạn của mục hiện tại hay không. Nếu có, hãy loại bỏ phần calo nhỏ nhất khỏi đống. Bước này bắt buộc rằng trong số tất cả các mục có thời hạn d, chúng ta chỉ giữ lại các mục d tốt nhất, bởi vì bất kỳ lịch trình khả thi nào cũng có thể sử dụng hầu hết các mục d trong các khe thời gian d. 
5. Sau khi xử lý tất cả các mục, tính tổng đống. Tổng số này là tổng lượng calo tối đa có thể đạt được. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố nào của các mục được sắp xếp theo thời hạn, giả sử thời hạn hiện tại là d. Bất kỳ lịch trình hợp lệ nào cũng có thể bao gồm nhiều nhất d mục từ tiền tố này vì chỉ có d khe thời gian tồn tại trước hoặc tại ranh giới thời hạn đó. Thuật toán luôn duy trì chính xác tập hợp kích thước tốt nhất có thể ở mức tối đa d bằng cách loại bỏ mục calo nhỏ nhất bất cứ khi nào vượt quá giới hạn. Bất biến này đảm bảo rằng sau khi xử lý từng tiền tố, vùng heap chứa lựa chọn tối ưu cho tiền tố đó và do đó cho cả tập hợp đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n = int(input())
    items = []
    for _ in range(n):
        t, k = map(int, input().split())
        items.append((t, k))

    items.sort()

    heap = []
    total = 0

    for t, k in items:
        heapq.heappush(heap, k)
        total += k

        if len(heap) > t:
            removed = heapq.heappop(heap)
            total -= removed

    print(total)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo chiến lược tham lam dựa trên đống. Việc sắp xếp theo thời hạn là rất quan trọng vì nó biến tính khả thi về thời gian thành một ràng buộc về tiền tố. Heap luôn lưu trữ các mục hiện được chọn. Việc giữ tổng đang chạy sẽ tránh việc tính toán lại tổng ở cuối. 

Chi tiết thực hiện tinh tế duy nhất là điều kiện`len(heap) > t`. Điều này sử dụng thực tế là sau khi xử lý tất cả các mục có thời hạn lên tới t, chúng tôi buộc phải chọn không quá t mục. Việc loại bỏ heap luôn nhắm tới giá trị nhỏ nhất, vì đó là lựa chọn ít gây tổn hại nhất để duy trì tính khả thi. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào nhỏ với các mục (thời hạn, giá trị): (1, 5), (1, 2), (2, 6), (2, 3). 

Sau khi sắp xếp, thứ tự không thay đổi. Chúng tôi theo dõi đống: 

| Bước | Mục | Đống sau khi chèn | Hành động | Tổng hợp | 
| --- | --- | --- | --- | --- | 
| 1 | (1,5) | [5] | được | 5 | 
| 2 | (1,2) | [2,5] | vượt quá 1, loại bỏ 2 | 5 | 
| 3 | (2,6) | [5,6] | được | 11 | 
| 4 | (2,3) | [3,5,6] | vượt quá 2, loại bỏ 3 | 11 | 

Lựa chọn cuối cùng tương ứng với việc lấy các mục có giá trị 5 và 6, là tối ưu vì tối đa hai mục có thể được lên lịch trong thời gian 2 và đây là những giá trị cao nhất tương thích với thời hạn. 

Bây giờ hãy xem xét (thời hạn, giá trị): (1,10), (2,1), (2,1), (2,1). 

| Bước | Mục | Đống sau khi chèn | Hành động | Tổng hợp | 
| --- | --- | --- | --- | --- | 
| 1 | (1,10) | [10] | được | 10 | 
| 2 | (2,1) | [1,10] | được | 11 | 
| 3 | (2,1) | [1,1,10] | xóa 1 | 10 | 
| 4 | (2,1) | [1,1,10] | xóa 1 | 9 | 

Điều này cho thấy rằng mặc dù có nhiều mục tồn tại nhưng chỉ có thể lấy được hai mục một cách hiệu quả và thuật toán đảm bảo giữ lại tập hợp con có giá trị cao nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế với O(n log n), mỗi thao tác heap là log n trên n mục | 
| Không gian | O(n) | Lưu trữ heap ở hầu hết tất cả các mục trong trường hợp xấu nhất | 

Các ràng buộc lên tới 200.000 mục phù hợp thoải mái trong độ phức tạp này vì các hệ số logarit vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io

    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like small case
assert run("3\n1 5\n1 2\n2 6\n") == "11"

# all same deadlines
assert run("4\n2 1\n2 2\n2 3\n2 4\n") == "7"

# strictly increasing deadlines
assert run("3\n1 10\n2 20\n3 30\n") == "60"

# tight constraint forcing drops
assert run("5\n1 100\n2 1\n2 1\n2 1\n2 1\n") == "101"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thời hạn hỗn hợp nhỏ | 11 | tính đúng đắn của việc thay thế đống | 
| tất cả các thời hạn giống nhau | 7 | chọn k đồ tốt nhất | 
| tăng thời hạn | 60 | không cần xóa | 
| một mặt hàng chiếm ưu thế | 101 | tham lam giữ giá trị cao | 

## Vỏ cạnh 

Trường hợp quan trọng là khi một mặt hàng có giá trị rất cao có thời hạn rất sớm. Đối với đầu vào (1.100), (2,1), (2,1), (2,1), (2,1), trước tiên thuật toán lấy 100, sau đó tạm thời chấp nhận các mục nhỏ hơn, nhưng liên tục loại bỏ chúng vì heap phải tôn trọng giới hạn kích thước. Đống cuối cùng giữ lại 100 và thêm một vật phẩm tốt nhất, tạo ra 101, phù hợp với lịch trình tối ưu trong đó vật phẩm tốt nhất chiếm giữ trong giờ đầu tiên và một vật phẩm nhỏ sẽ lấp đầy một vị trí khác. 

Một trường hợp đặc biệt khác là khi tất cả các thời hạn đều giống hệt nhau. Trong tình huống này, thuật toán giảm một cách hiệu quả xuống việc chọn các giá trị d cao nhất trong số tất cả các mục, trong đó d là thời hạn chung đó. Heap thực thi điều này một cách tự nhiên mà không cần xử lý rõ ràng.
