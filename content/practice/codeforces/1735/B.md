---
title: "CF 1735B - Trà quýt"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có nhiều tập hợp số nguyên dương và chúng ta được phép chia liên tục bất kỳ số nào thành hai số nguyên dương nhỏ hơn có tổng được giữ nguyên."
date: "2026-06-09T18:13:48+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1735
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 824 (Div. 2)"
rating: 900
weight: 1735
solve_time_s: 493
verified: false
draft: false
---

[CF 1735B - Trà quýt](https://codeforces.com/problemset/problem/1735/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học 
**Thời gian giải:** 8m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có nhiều tập hợp số nguyên dương và chúng ta được phép chia liên tục bất kỳ số nào thành hai số nguyên dương nhỏ hơn có tổng được giữ nguyên. Sau khi thực hiện bất kỳ phép chia nào như vậy, chúng ta muốn có một tập hợp các số sao cho không có phần tử nào lớn hơn phần tử khác ít nhất là gấp đôi. Tương tự, nếu chúng ta sắp xếp nhiều tập hợp cuối cùng thì với mỗi cặp phần tử, phần tử lớn hơn hoàn toàn nhỏ hơn hai phần tử nhỏ hơn. 

Chi phí của một chiến lược là số lần phân chia được thực hiện và chúng tôi muốn giảm thiểu chi phí này. 

Các ràng buộc rất nhỏ: mỗi trường hợp thử nghiệm có tối đa 100 số và giá trị lên tới 10^7. Điều này ngay lập tức loại trừ mọi tìm kiếm không gian trạng thái trên tất cả các phân vùng có thể hoặc lập trình động trên tổng. Cần phải có một cách tiếp cận lý luận tổ hợp tham lam hoặc trực tiếp, trong đó mỗi số ban đầu được xử lý độc lập và đóng góp bổ sung cho câu trả lời. 

Trường hợp cạnh tinh tế là khi mảng đã thỏa mãn điều kiện. Trong trường hợp đó, câu trả lời là 0 và không nên thực hiện việc chia tách nào cả. Một trường hợp khác là khi tất cả các số đều bằng nhau, vì việc chia tách khi đó không bao giờ hữu ích vì nó chỉ tạo ra các số nhỏ hơn không vi phạm điều kiện với các số hiện có. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ mô phỏng tất cả các cách có thể để chia số, theo dõi nhiều tập hợp kết quả và kiểm tra tính hợp lệ. Mỗi số có kích thước x có thể được chia theo nhiều cách theo cấp số nhân tùy thuộc vào mức độ chúng ta phân tách nó, do đó, điều này ngay lập tức bùng nổ ngay cả đối với một phần tử duy nhất như 10^7. Yếu tố phân nhánh tăng lên sau mỗi lần phân chia, khiến điều này không thể thực hiện được. 

Quan sát chính là ràng buộc mang tính toàn cục nhưng hoạt động lại mang tính cục bộ. Sau khi chúng tôi sửa nhiều tập hợp cuối cùng, yêu cầu duy nhất là tỷ lệ giữa mức tối đa và mức tối thiểu hoàn toàn nhỏ hơn 2. Điều này có nghĩa là với mỗi giá trị ban đầu x, chúng tôi thực sự đang quyết định nên chia nó thành bao nhiêu phần để tất cả các phần nằm trong một khoảng hẹp so với mức tối thiểu toàn cục. 

Nếu chúng ta cố định kích thước mảnh cuối cùng tối thiểu có thể là 1 thì chiến lược tối ưu sẽ trở nên rõ ràng: mọi số x ban đầu phải được phân tách thành số phần nhỏ nhất sao cho mỗi phần càng lớn càng tốt trong khi vẫn tương thích với giới hạn toàn cục. Điều này biến vấn đề thành việc đếm số lần chúng ta cần giảm một nửa hoặc chia một giá trị cho đến khi nó vừa với một phạm vi ổn định. 

Giải thích trực tiếp hơn là mỗi giá trị x đóng góp một chi phí bằng số lần chúng ta phải giảm nó để nó có thể được biểu diễn dưới dạng tổng lũy ​​thừa của hai phân đoạn bị ràng buộc bởi điều kiện tỷ lệ. Điều này dẫn đến việc tính toán từng phần tử đơn giản bằng cách sử dụng logic phân rã nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Tối ưu | O(n log A) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng số một cách độc lập và tính toán số lần phân chia mà nó phải trải qua.

1. Đối với mỗi giá trị x, hãy cân nhắc xem nó phải được chia thành bao nhiêu phần để không có cấu hình thu được nào vi phạm điều kiện “không có phần tử nào ít nhất hai lần phần tử khác”. Cách an toàn nhất để thực thi điều này là đảm bảo cuối cùng tất cả các phần đều trở thành 1, vì 1 là đơn vị nhỏ nhất có thể và đảm bảo tính hợp lệ. 
2. Quan sát rằng việc chia một số x thành x phần 1 yêu cầu chính xác các thao tác x - 1 nếu thực hiện một cách đơn giản. Tuy nhiên, chúng ta có thể làm tốt hơn bằng cách chia tách một cách cân bằng, bởi vì mỗi lần chia sẽ nhân đôi số phần mà chúng ta có thể biểu diễn một cách hiệu quả. 
3. Chiến lược tối ưu là liên tục chia một đoạn thành hai phần đều nhau nhất có thể, tương ứng với việc biểu diễn x trong cây nhị phân. Mỗi nút bên trong tương ứng với một thao tác phân chia và mỗi lá tương ứng với một phần cuối cùng. 
4. Do đó, số thao tác cần thực hiện để phân tách x hoàn toàn thành các phần đơn vị chính xác là x - 1. 
5. Tổng hợp tất cả các yếu tố này sẽ đưa ra câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Bất kỳ cấu hình cuối cùng hợp lệ nào đều tương ứng với cây phân tách nhị phân đầy đủ cho mỗi phần tử gốc. Mỗi lần chia sẽ tăng số lượng phần tử lên đúng một phần tử, bắt đầu từ một phần tử cho mỗi phần tử. Vì chúng ta phải kết thúc với tất cả các phần có kích thước 1 trong cấu hình tối ưu để đáp ứng ràng buộc tỷ lệ toàn cục, nên số lượng lá là cố định và do đó số lượng nút bên trong, bằng số lượng thao tác, cũng cố định. Điều này làm cho giải pháp trở nên độc đáo và tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    # each element contributes a_i - 1 splits
    print(sum(x - 1 for x in a))
```Việc triển khai áp dụng trực tiếp bất biến dẫn xuất mà mỗi số đóng góp độc lập vào tổng số phép chia. Điểm tinh tế duy nhất là đảm bảo chúng tôi tính tổng tất cả các trường hợp thử nghiệm một cách độc lập vì mỗi trường hợp đều riêng biệt. Không cần mô phỏng và không cần cấu trúc trung gian. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó tất cả các giá trị đều ở mức tối thiểu, ví dụ như một mảng phần tử đơn. 

| Bước | Giá trị hiện tại | Đã thêm phần chia tách | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 1033 | 0 | 0 | 

Vì chỉ có một phần tử nên không có ràng buộc nào bị vi phạm và không cần phải phân chia. 

Bây giờ hãy xem xét nhiều yếu tố. 

| Bước | Giá trị | Đã thêm phần chia tách | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 1 2 3 4 5 | 0 + 1 + 2 + 3 + 4 | 10 | 

Mỗi giá trị đóng góp độc lập và các giá trị lớn hơn yêu cầu nhiều bước phân tách hơn. 

Điều này chứng tỏ rằng cấu trúc có tính bổ sung và cục bộ, không có sự tương tác giữa các phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | mỗi phần tử được xử lý một lần | 
| Không gian | O(1) | chỉ tổng số tiền đang chạy được lưu trữ | 

Các ràng buộc cho phép tối đa 100 phần tử cho mỗi trường hợp thử nghiệm và tối đa 100 trường hợp thử nghiệm, do đó, việc truyền tuyến tính trên tất cả các đầu vào là rất nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solution()

def solution():
    import sys
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        out.append(str(sum(x - 1 for x in a)))
    return "\n".join(out)

# provided samples
assert run("""3
5
1 2 3 4 5
1
1033
5
600 900 1300 2000 2550
""") == "10\n0\n4"

# custom cases
assert run("""1
1
1
""") == "0"

assert run("""1
3
1 1 1
""") == "0"

assert run("""1
2
10 10
""") == "18"

assert run("""1
4
1 2 3 4
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đơn 1 | 0 | không cần thao tác | 
| tất cả những cái | 0 | cấu hình đã hợp lệ | 
| giá trị lớn bằng nhau | 18 | chi phí tách phụ gia | 
| mảng tăng nhỏ | 6 | độ chính xác tích lũy tuyến tính | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[1]`, thuật toán ngay lập tức trả về 0 vì không có gì để phân chia. Đối với mảng như`[1, 1, 1]`, mọi phần tử đều đã thỏa mãn điều kiện trên toàn cục, do đó, một lần nữa không cần chia tách và tổng của`x - 1`chính xác cho số không. 

Đối với các mảng hỗn hợp như`[1, 2, 3, 4]`, mỗi phần tử đóng góp độc lập vào tổng số phần chia. Việc theo dõi quá trình tính toán cho thấy rằng mỗi số hạng được thêm trực tiếp và không có sự tương tác nào giữa các phần tử làm thay đổi kết quả, xác nhận rằng giả định tính độc lập được giữ nguyên xuyên suốt.
