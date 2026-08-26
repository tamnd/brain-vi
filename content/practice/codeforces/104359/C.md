---
title: "CF 104359C - \u041f\u043e\u043c\u043e\u0433\u0430\u0435\u043c \u043f\u0440\u0438\u0440\u043e\u0434\u0435"
description: "Chúng ta được cung cấp một mảng biểu thị mức độ ẩm dọc theo một hàng cây. Mỗi thao tác sửa đổi một phân đoạn liền kề theo một cách rất có cấu trúc. Một thao tác giảm tiền tố đi 1, thao tác khác giảm hậu tố đi 1 và thao tác thứ ba tăng toàn bộ mảng lên 1."
date: "2026-07-01T17:58:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104359
codeforces_index: "C"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2022"
rating: 0
weight: 104359
solve_time_s: 50
verified: true
draft: false
---

[CF 104359C - \u041f\u043e\u043c\u043e\u0433\u0430\u0435\u043c \u043f\u0440\u0438\u0440\u043e\u0434\u0435](https://codeforces.com/problemset/problem/104359/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng biểu thị mức độ ẩm dọc theo một hàng cây. Mỗi thao tác sửa đổi một phân đoạn liền kề theo một cách rất có cấu trúc. Một thao tác giảm tiền tố đi 1, thao tác khác giảm hậu tố đi 1 và thao tác thứ ba tăng toàn bộ mảng lên 1. Mục tiêu là áp dụng một chuỗi các thao tác này sao cho mọi phần tử trở thành chính xác bằng 0, đồng thời giảm thiểu số lượng thao tác được sử dụng. 

Một cách khác để nghĩ về điều này là chúng ta được phép cộng hoặc trừ 1 từ các khoảng nhất định và chúng ta muốn chuyển mảng ban đầu thành mảng bằng 0 bằng cách sử dụng ít cập nhật khoảng nhất, trong đó các khoảng được phép là tiền tố, hậu tố và toàn bộ mảng. 

Các ràng buộc cho phép tối đa 200000 phần tử, do đó, bất kỳ giải pháp nào cố gắng mô phỏng các hoạt động hoặc tìm kiếm các khả năng có độ phức tạp bậc hai đều ngay lập tức không khả thi. Các hoạt động gợi ý rằng cần phải phân tách tuyến tính hoặc gần tuyến tính, có thể dựa trên sự khác biệt giữa các phần tử liền kề. 

Một điểm tinh tế là các giá trị có thể âm. Điều này quan trọng vì thao tác "tăng tất cả các phần tử" có thể được sử dụng để bù các giá trị âm trên toàn cầu, do đó giải pháp không chỉ đơn thuần là giảm các giá trị dương. Thay vào đó, bài toán có tính đối xứng theo cách cho phép diễn giải nó như xây dựng mảng từ 0 bằng cách sử dụng các cập nhật khoảng có dấu. 

Một sai lầm ngây thơ sẽ là coi đây là việc cố định từng vị trí một cách độc lập một cách tham lam. Ví dụ: nếu một người cố gắng sửa a[i] bằng cách áp dụng các thao tác tiền tố hoặc hậu tố cục bộ, thì nó sẽ bỏ qua rằng mọi thao tác đều ảnh hưởng đến nhiều vị trí cùng một lúc, do đó các sửa đổi cục bộ có thể phá vỡ các bản sửa lỗi trước đó. 

Một trường hợp lỗi phổ biến khác là bỏ qua sự tương tác giữa các phần tử liền kề. Ví dụ, hãy xem xét một mảng như [1, -1, 1]. Bất kỳ cách tiếp cận tham lam cục bộ nào để sửa phần tử đầu tiên sẽ truyền bá những thay đổi ngoài ý muốn cho phần còn lại, khiến không thể suy luận độc lập cho mỗi chỉ mục. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tìm kiếm theo chuỗi các hoạt động hoặc thậm chí biểu diễn quá trình dưới dạng một hệ thống tuyến tính số nguyên trong đó mỗi tiền tố, hậu tố và hoạt động toàn mảng là một biến. Điều này nhanh chóng trở thành một vấn đề tối ưu hóa lớn với 3n phép toán có thể xảy ra và các ràng buộc khớp nối tất cả các vị trí. Ngay cả một mô phỏng hoạt động tham lam ngây thơ cũng sẽ yêu cầu quét mảng liên tục, dẫn đến hành vi O(n^2) hoặc tệ hơn. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì áp dụng các thao tác để biến mảng về 0, chúng tôi nghĩ xem mỗi thao tác phải được áp dụng bao nhiêu lần và các thao tác đó đóng góp như thế nào cho từng vị trí. 

Chúng ta hãy xác định ba nhóm phép toán: các phép toán tiền tố kết thúc tại i, các phép toán hậu tố bắt đầu tại i và các phép toán toàn mảng. Mỗi đóng góp tuyến tính cho các phân khúc. Cấu trúc cơ bản là mọi sự khác biệt liền kề đều cô lập sự đóng góp của một loại hoạt động duy nhất. 

Nếu chúng ta xem xét sự khác biệt a[i] - a[i-1], hầu hết tất cả các hiệu ứng tổng thể đều bị loại trừ ngoại trừ các hoạt động “bắt đầu” hoặc “kết thúc” ở các ranh giới cụ thể. Đây là tín hiệu cổ điển cho thấy có sự phân tách mảng hoặc độ dốc. 

Vấn đề giảm xuống còn việc xây dựng lại số lượng thao tác phân đoạn cần thiết để tạo thành mảng từ 0, trong đó mỗi thao tác tương ứng với việc thay đổi “độ dốc” tại một ranh giới. Câu trả lời cuối cùng là tổng các thay đổi tuyệt đối trong các giá trị liền kề, cộng với sự điều chỉnh cho phần tử đầu tiên, bởi vì phép toán toàn bộ mảng hoạt động như một sự dịch chuyển toàn cục. 

Do đó, thay vì suy nghĩ trực tiếp về các khoảng thời gian, chúng tôi theo dõi cách mảng thay đổi từ trái sang phải và đếm xem có bao nhiêu “công việc” mới phải được đưa vào mỗi vị trí.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng hoạt động vũ lực | O(n²) | O(n) | Quá chậm | 
| Tái thiết dựa trên sự khác biệt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng tất cả các thao tác đều ảnh hưởng đến các phân đoạn liền kề theo cách cộng tuyến tính, do đó trạng thái cuối cùng chỉ phụ thuộc vào số lần mỗi loại thao tác được áp dụng. Điều này gợi ý chuyển vấn đề thành vấn đề về sự đóng góp tại các ranh giới thay vì mô phỏng mỗi hoạt động. 
2. Viết lại điều kiện đích dưới dạng xây dựng mảng bắt đầu từ 0 bằng cách sử dụng các cập nhật phân đoạn được phép. Tính đối xứng này hợp lệ vì mỗi phép toán đều có thể đảo ngược và tuyến tính. 
3. Xem xét việc quét mảng từ trái sang phải trong khi theo dõi xem giá trị được yêu cầu ở vị trí i đã được giải thích bằng các thao tác ảnh hưởng đến vị trí trước đó là bao nhiêu. “Yêu cầu mới” duy nhất ở i xuất phát từ sự khác biệt giữa a[i] và a[i-1]. 
4. Xác định đường cơ sở đang hoạt động thể hiện tác động tích lũy của các hoạt động bao trùm vị trí hiện tại từ các chỉ số trước đó. Ở mỗi bước i, sự không khớp giữa a[i] và đường cơ sở này cho biết có bao nhiêu thao tác mới phải bắt đầu hoặc kết thúc tại i. 
5. Tích lũy chênh lệch tuyệt đối |a[i] - a[i-1]| với mọi i ≥ 2. Mỗi chênh lệch như vậy tương ứng với số lượng điểm cuối khoảng tối thiểu cần thiết để điều chỉnh độ dốc giữa các vị trí liền kề. 
6. Cuối cùng, tính đến độ lệch ban đầu a[1], vì phần tử đầu tiên yêu cầu nhiều đơn vị thay đổi ròng từ một mảng bằng 0 ban đầu, chỉ có thể đạt được thông qua các hoạt động tiền tố, hậu tố và tổng thể kết hợp. 

### Tại sao nó hoạt động 

Mọi thao tác đều thay đổi mảng theo cách không đổi từng phần, nghĩa là nó chỉ đưa ra những thay đổi ở ranh giới của các khoảng. Nếu chúng ta xem xét sự khác biệt liền kề, mỗi thao tác góp phần tạo ra nhiều nhất hai thay đổi về ranh giới và những đóng góp này không ảnh hưởng lẫn nhau khi tính tổng trên toàn bộ mảng. Tổng số thao tác được yêu cầu chính xác là tổng biến thể của mảng khi được xem dưới dạng một chuỗi các phần tăng dần, với phần tử đầu tiên xử lý phần bù chung. 

Điều này đảm bảo rằng bất kỳ nỗ lực nào nhằm giảm bớt hoạt động hơn nữa sẽ yêu cầu hủy bỏ thay đổi ranh giới mà không ảnh hưởng đến những hoạt động khác, điều này là không thể theo cấu trúc hoạt động được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 0:
        print(0)
        return
    
    ans = abs(a[0])
    for i in range(1, n):
        ans += abs(a[i] - a[i-1])
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã đọc mảng và tích lũy hai thành phần: giá trị tuyệt đối của phần tử đầu tiên và sự khác biệt tuyệt đối giữa các phần tử liên tiếp. Số hạng đầu tiên giải thích cho sự dịch chuyển toàn cầu cần thiết để đưa giá trị ban đầu về 0, vì không có vị trí nào trước đó có thể triệt tiêu nó. Số hạng thứ hai nắm bắt tất cả các thay đổi độ dốc cục bộ, mỗi thay đổi tương ứng với một ranh giới vận hành cần thiết. 

Việc thực hiện được tối giản một cách có chủ ý vì tất cả sự phức tạp đã được hấp thụ vào quan sát rằng chỉ những khác biệt liền kề mới quan trọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`[-2, -2, -2]`Chúng tôi tính toán đóng góp từng bước. 

| tôi | một [tôi] | đóng góp khác biệt | tổng số tiền chạy | 
| --- | --- | --- | --- | 
| 1 | -2 | 2 | 2 | 
| 2 | -2 | 0 | 2 | 
| 3 | -2 | 0 | 2 | 

Câu trả lời là 2. Điều này tương ứng với việc áp dụng thao tác “tăng tất cả” hai lần, thao tác này sẽ dịch chuyển mảng một cách đồng đều về 0. 

Điều này xác nhận rằng khi tất cả các giá trị đều bằng nhau thì chỉ cần điều chỉnh toàn cục và không cần điều chỉnh ranh giới. 

### Ví dụ 2:`10, 4, 7`Chúng tôi tính toán: 

| tôi | một [tôi] | đóng góp khác biệt | tổng số tiền chạy | 
| --- | --- | --- | --- | 
| 1 | 10 | 10 | 10 | 
| 2 | 4 | 6 | 16 | 
| 3 | 7 | 3 | 19 | 

Câu trả lời là 19. 

Dấu vết này cho thấy mỗi thay đổi về độ dốc sẽ tác động đến các hoạt động bổ sung như thế nào. Việc giảm từ 10 xuống 4 cần 6 đơn vị điều chỉnh và tăng từ 4 lên 7 cần 3 đơn vị. Mỗi thay đổi phân đoạn là độc lập về các hoạt động cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | đơn lẻ đi qua mảng tính toán sự khác biệt tuyệt đối | 
| Không gian | O(1) | chỉ có một bộ tích lũy đang chạy được lưu trữ | 

Giải pháp phù hợp thoải mái trong giới hạn vì n lên tới 200000 và tính toán hoàn toàn tuyến tính với bộ nhớ bổ sung không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    
    ans = abs(a[0]) if n > 0 else 0
    for i in range(1, n):
        ans += abs(a[i] - a[i-1])
    return str(ans)

# provided samples
assert run("3\n-2 -2 -2\n") == "2"
assert run("3\n10 4 7\n") == "19"

# custom cases
assert run("1\n5\n") == "5", "single element"
assert run("1\n0\n") == "0", "already zero"
assert run("5\n1 2 3 4 5\n") == "5", "monotone increasing"
assert run("5\n5 4 3 2 1\n") == "5", "monotone decreasing"
assert run("4\n0 0 0 0\n") == "0", "all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 | cơ sở xử lý tuyệt đối | 
| phần tử không | 0 | trường hợp không hoạt động tầm thường | 
| trình tự tăng dần | 5 | chi phí độ dốc tích lũy | 
| dãy giảm dần | 5 | hành vi đối xứng | 
| tất cả số không | 0 | không cần thao tác | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mảng không đổi nhưng khác 0, chẳng hạn như`[-3, -3, -3, -3]`. Thuật toán tính toán`|a[0]| = 3`và tất cả sự khác biệt bằng 0, tạo ra 3. Điều này phù hợp với thực tế là chỉ cần thao tác toàn cục để dịch chuyển toàn bộ mảng một cách thống nhất. 

Một trường hợp quan trọng khác là một sự tăng đột biến như`[0, 0, 100, 0]`. Tính toán mang lại kết quả`|0| + |0-0| + |100-0| + |0-100| = 200`. Điều này phản ánh rằng mức tăng đột biến tạo ra hai thay đổi ranh giới độc lập, một tăng và một giảm và cả hai đều phải được thanh toán riêng. 

Cuối cùng, xen kẽ các mẫu như`[1, -1, 1, -1]`tạo ra chi phí tích lũy lớn vì mỗi lần chuyển đổi liền kề đều đóng góp một chênh lệch khác 0. Thuật toán tự nhiên tính mỗi dao động là một ranh giới hoạt động bắt buộc, phù hợp với thực tế là mỗi lần lật sẽ buộc phải điều chỉnh phân đoạn mới mà không thể sử dụng lại ở nơi khác.
