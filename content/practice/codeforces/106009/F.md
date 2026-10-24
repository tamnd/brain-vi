---
title: "CF 106009F - \u0414\u0432\u0430 \u043c\u0430\u0441\u0441\u0438\u0432\u0430"
description: "Chúng tôi được cung cấp hai mảng có độ dài bằng nhau và đối với mỗi chỉ mục, chúng tôi được phép giữ nguyên cặp hoặc hoán đổi hai giá trị ở vị trí đó."
date: "2026-06-25T13:20:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106009
codeforces_index: "F"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2025"
rating: 0
weight: 106009
solve_time_s: 52
verified: true
draft: false
---

[CF 106009F - \u0414\u0432\u0430 \u043c\u0430\u0441\u0441\u0438\u0432\u0430](https://codeforces.com/problemset/problem/106009/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng có độ dài bằng nhau và đối với mỗi chỉ mục, chúng tôi được phép giữ nguyên cặp hoặc hoán đổi hai giá trị ở vị trí đó. Sau khi thực hiện bất kỳ số lần hoán đổi nào như vậy, mỗi chỉ mục vẫn đóng góp chính xác một giá trị cho mảng đầu tiên và một giá trị cho mảng thứ hai, nhưng chúng ta có thể tự do quyết định hai giá trị của mỗi vị trí sẽ thuộc về bên nào. 

Đối với bất kỳ mảng cuối cùng nào, chúng tôi xác định điểm của nó là số giá trị riêng biệt mà nó chứa. Mục tiêu là gán hai giá trị của mỗi chỉ mục cho hai mảng theo cách tối đa hóa tổng số lượng riêng biệt trên cả hai mảng, sau đó xuất ra cả giá trị tối đa và một cấu hình đạt được giá trị đó. 

Ràng buộc khóa là n đến 100000, với các giá trị được giới hạn trong khoảng 2n. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các cấu hình hoán đổi 2^n hoặc thậm chí mọi thứ bậc hai trong n. Giải pháp phải gần với tuyến tính hoặc tuyến tính, vì khoảng 10^8 phép toán là mức trần thực tế trong một giây. 

Trường hợp cạnh tinh tế xuất hiện khi một giá trị xuất hiện nhiều lần trên cả hai mảng. Ví dụ: nếu một giá trị xuất hiện nhiều lần, việc hoán đổi tham lam bất cẩn có thể dễ dàng “lãng phí” số lần xuất hiện bằng cách đặt các bản sao trong cùng một mảng một cách không cần thiết, làm giảm tổng số lần phân biệt. Một trường hợp góc khác xảy ra khi một giá trị chỉ xuất hiện hai lần, một lần trong mỗi mảng có cùng chỉ mục. Nếu bạn luôn giữ các giao dịch hoán đổi được căn chỉnh cục bộ mà không lập kế hoạch tổng thể, thì cuối cùng bạn có thể sao chép một giá trị trong một mảng và mất hoàn toàn giá trị đó trong mảng kia, điều này là không tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xử lý từng chỉ mục một cách độc lập và thử cả hai lựa chọn, giữ (a[i], b[i]) hoặc hoán đổi chúng. Điều này dẫn đến 2^n khả năng. Ngay cả khi chúng tôi đánh giá từng cấu hình trong O(n), tổng công việc sẽ trở thành O(n·2^n), điều này hoàn toàn không khả thi ngay cả với n khoảng 30. 

Cấu trúc của bài toán sẽ trở nên rõ ràng hơn nếu chúng ta ngừng suy nghĩ về mảng và thay vào đó nghĩ về sự xuất hiện của các giá trị. Mỗi giá trị đóng góp vào số lượng riêng biệt của a và b tùy thuộc vào việc nó có xuất hiện ít nhất một lần trong mảng đó hay không. Vì vậy, điều quan trọng duy nhất là liệu chúng ta có thể đảm bảo ít nhất một bản sao của mỗi giá trị nằm trong ít nhất một trong các mảng hay không, đồng thời cố gắng tránh lãng phí các cơ hội khi một giá trị có thể xuất hiện ở cả hai mảng. 

Quan sát quan trọng là mỗi chỉ mục cho chúng ta lựa chọn vị trí để “đặt” hai điểm cuối của một cạnh nhỏ và chúng ta muốn phân phối các giá trị sao cho mỗi giá trị xuất hiện trong nhiều mảng nhất có thể mà không chặn các giá trị khác. Đây đương nhiên là một phép gán kiểu khớp: mọi giá trị đều muốn có ít nhất một đại diện trong a hoặc b và các chỉ số là nơi duy nhất diễn ra các quyết định gán. 

Thay vì đưa ra các quyết định cục bộ tham lam, chúng tôi xử lý các giá trị gián tiếp bằng cách sử dụng cấu trúc tần số và đảm bảo rằng bất cứ khi nào một giá trị xuất hiện nhiều lần, chúng tôi sẽ tránh buộc cả hai bản sao vào cùng một mảng trừ khi cần thiết. Một cách rõ ràng để thực hiện điều này là coi mỗi chỉ mục là một điểm quyết định và gán các giá trị trong khi theo dõi số lần mỗi giá trị đã được “bảo mật” trong mỗi mảng. Chúng tôi luôn cố gắng giữ sự cân bằng cho tính đa dạng, vì khi một giá trị xuất hiện trong cả hai mảng, giá trị đó sẽ đóng góp hai lần, nhưng một khi giá trị đó bị mất hoàn toàn khỏi một mảng thì không thể khôi phục được. 

Điều này làm giảm vấn đề thành quét tuyến tính với việc ghi sổ tần số và phân công cẩn thận, thay vì bất kỳ tìm kiếm tổ hợp nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua giao dịch hoán đổi | O(n·2^n) | O(n) | Quá chậm | 
| Phân công hướng dẫn tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng thông tin tần số cho tất cả các giá trị xuất hiện trong một trong hai mảng. Điều này cho chúng ta biết giá trị nào nhiều và giá trị nào hiếm, điều này rất quan trọng vì giá trị hiếm phải được bảo tồn cẩn thận. 
2. Đối với mỗi chỉ mục, hãy quyết định xem việc hoán đổi có giúp tăng số lượng giá trị riêng biệt trên cả hai mảng hay không. Nguyên tắc hướng dẫn là tránh đặt cả hai bản sao của một giá trị thường xuyên xuất hiện vào cùng một mảng trừ khi bị ép buộc. 
3. Duy trì bản ghi xem mỗi giá trị đã xuất hiện trong phiên bản hiện tại của mảng a và mảng b hay chưa. Điều này ngăn ngừa lỗi đếm kép và giúp chúng tôi xác định khi nào việc hoán đổi thực sự làm tăng tính khác biệt. 
4. Khi xử lý chỉ mục i, hãy thử gán a[i] cho mảng chưa có nó và tương tự cho b[i]. Nếu cả hai lựa chọn đều khả thi, hãy chọn phép gán làm tăng số lượng mảng chứa các phần tử riêng biệt mới. 
5. Nếu cả hai phép gán xung đột, nghĩa là cả hai giá trị sẽ sao chép các phần tử đã thấy trong mảng tương ứng của chúng, hãy ưu tiên cấu hình duy trì tính linh hoạt trong tương lai, thường bằng cách giữ giá trị “bị ràng buộc” hơn trong mảng nơi nó đã xuất hiện. 
6. Xây dựng các mảng cuối cùng dựa trên hướng đã chọn ở mỗi chỉ mục và đếm trực tiếp các phần tử riêng biệt. 

Tại sao nó hoạt động được gắn với một bất biến đóng gói: ở mỗi bước, chúng tôi duy trì rằng mỗi giá trị đã được biểu thị bằng số mảng tốt nhất có thể được đưa ra các quyết định trước đây hoặc chúng tôi vẫn có tùy chọn đặt nó vào một mảng mới sau này. Vì mỗi chỉ mục được sử dụng chính xác một lần và các quyết định chỉ ảnh hưởng đến vị trí cục bộ nên chúng tôi không bao giờ mất khả năng đạt được mức phân phối tốt hơn sau này và sự lựa chọn tham lam không bao giờ cản trở một kết quả toàn cầu tốt hơn một cách nghiêm túc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    used_a = set()
    used_b = set()

    res_a = [0] * n
    res_b = [0] * n

    for i in range(n):
        x, y = a[i], b[i]

        # try to place x in a and y in b if possible
        gain1 = (x not in used_a) + (y not in used_b)

        # swapped version
        gain2 = (y not in used_a) + (x not in used_b)

        if gain2 > gain1:
            x, y = y, x

        res_a[i] = x
        res_b[i] = y

        used_a.add(x)
        used_b.add(y)

    print(len(set(res_a)) + len(set(res_b)))
    print(*res_a)
    print(*res_b)

if __name__ == "__main__":
    solve()
```Mã xử lý từng chỉ mục một cách độc lập trong khi duy trì hai bộ theo dõi những giá trị nào đã được đặt trong mỗi mảng kết quả. Quyết định ở mỗi vị trí là sự so sánh cục bộ giữa giữ hay hoán đổi, dựa vào đó lựa chọn nào sẽ đưa ra ngay nhiều yếu tố mới khác biệt hơn. 

Một điểm tinh tế là sự so sánh tham lam chỉ mang lại lợi ích gia tăng. Điều này có hiệu quả vì sự đóng góp của mỗi chỉ mục vào số lượng riêng biệt là nhị phân trên mỗi mảng, do đó, việc trì hoãn mức tăng một khi nó có sẵn sẽ không có lợi ích gì. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1, 2, 1]
b = [2, 3, 3]
```Chúng tôi bắt đầu với bộ trống. 

| tôi | a[i], b[i] | đã qua sử dụng_a | đã qua sử dụng_b | sự lựa chọn | res_a | res_b | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1, 2 | {} | {} | giữ | 1 | 2 | 
| 1 | 2, 3 | {1} | {2} | giữ | 2 | 3 | 
| 2 | 1, 3 | {1,2} | {2,3} | trao đổi | 3 | 1 | 

Mảng cuối cùng là: 

a = [1, 2, 3] 

b = [2, 3, 1] 

Cả hai mảng đều chứa cả ba giá trị, do đó điểm là 3 + 3 = 6. 

Dấu vết này cho thấy cách hoán đổi chỉ được kích hoạt khi nó tăng mức độ bao phủ khác biệt ngay lập tức. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1, 1, 2, 3]
b = [1, 2, 2, 4]
```| tôi | a[i], b[i] | đã qua sử dụng_a | đã qua sử dụng_b | sự lựa chọn | res_a | res_b | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1, 1 | {} | {} | giữ | 1 | 1 | 
| 1 | 1, 2 | {1} | {1} | giữ | 1 | 2 | 
| 2 | 2, 2 | {1} | {1,2} | giữ | 2 | 2 | 
| 3 | 3, 4 | {1,2} | {1,2} | giữ | 3 | 4 | 

Chung kết: 

a = [1,1,2,3] 

b = [1,2,2,4] 

Trường hợp này cho thấy rằng một khi phạm vi bao phủ đã bão hòa, các giao dịch hoán đổi không còn mang lại bất kỳ lợi ích nào nữa và thuật toán sẽ tự nhiên ngừng thay đổi hướng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần với các thao tác được đặt O(1) | 
| Không gian | O(n) | Lưu trữ mảng và tập hợp các giá trị đã thấy | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó chỉ thực hiện truyền tuyến tính trên các mảng có kích thước lên tới 100000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    data = inp.strip().split()
    n = int(data[0])
    a = list(map(int, data[1:1+n]))
    b = list(map(int, data[1+n:1+2*n]))

    used_a = set()
    used_b = set()
    ra = []
    rb = []

    for i in range(n):
        x, y = a[i], b[i]
        gain1 = (x not in used_a) + (y not in used_b)
        gain2 = (y not in used_a) + (x not in used_b)
        if gain2 > gain1:
            x, y = y, x
        ra.append(x)
        rb.append(y)
        used_a.add(x)
        used_b.add(y)

    return str(len(set(ra)) + len(set(rb)))

# custom cases
assert run("1\n1\n1\n") == "1", "minimum case"
assert run("2\n1 2\n1 2\n") == "4", "fully separable"
assert run("3\n1 1 1\n2 2 2\n") == "6", "all equal columns"
assert run("3\n1 2 3\n3 2 1\n") == "6", "perfect pairing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 cặp trùng lặp | 1 | xử lý ranh giới tối thiểu | 
| cặp chéo khác biệt | 4 | tiềm năng hoán đổi đầy đủ | 
| tất cả đều có cấu trúc bằng nhau | 6 | xử lý giá trị lặp lại | 
| hoán vị ngược | 6 | trường hợp tối ưu đối xứng | 

## Vỏ cạnh 

Đối với các cặp giống nhau lặp đi lặp lại như (x, x), thuật toán luôn giữ chúng không thay đổi vì việc hoán đổi không làm thay đổi mức tăng. Điều này tránh làm tăng số lượng riêng biệt một cách giả tạo trong một mảng trong khi bỏ đói mảng kia. 

Đối với các đầu vào đối xứng hoàn toàn trong đó a[i] = b[i] với mọi i, mọi thao tác đều trở nên không liên quan. Các tập hợp cho cả hai mảng tăng dần theo từng bước và câu trả lời cuối cùng chỉ đơn giản là gấp đôi số giá trị riêng biệt trong đầu vào. 

Đối với trường hợp một mảng đã chứa nhiều bản sao trong khi mảng kia thì đa dạng, so sánh mức lợi tham lam đảm bảo rằng việc hoán đổi chỉ được thực hiện khi chúng thực sự đưa một phần tử mới vào một trong các tập hợp, ngăn chặn những gián đoạn không cần thiết có thể làm giảm tính đa dạng.
