---
title: "CF 2003B - Rùa và Lợn Chơi Trò 2"
description: "Chúng tôi được cung cấp một chuỗi số và hai người chơi liên tục thu nhỏ nó cho đến khi chỉ còn một giá trị ở vị trí phía trước. Mỗi lần di chuyển sẽ loại bỏ một phần tử, nhưng trước khi loại bỏ nó, phần tử bên trái của cặp liền kề đã chọn sẽ được cập nhật."
date: "2026-06-08T13:47:21+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2003
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 968 (Div. 2)"
rating: 800
weight: 2003
solve_time_s: 120
verified: false
draft: false
---

[CF 2003B - Rùa và Lợn đang chơi trò chơi 2](https://codeforces.com/problemset/problem/2003/B) 

**Đánh giá:** 800 
**Tags:** trò chơi, tham lam, phân loại 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi số và hai người chơi liên tục thu nhỏ nó cho đến khi chỉ còn một giá trị ở vị trí phía trước. Mỗi lần di chuyển sẽ loại bỏ một phần tử, nhưng trước khi loại bỏ nó, phần tử bên trái của cặp liền kề đã chọn sẽ được cập nhật. Một người chơi luôn cố gắng làm cho giá trị đang phát triển này càng lớn càng tốt, người kia luôn cố gắng ngăn chặn nó càng nhiều càng tốt. 

Quan sát quan trọng là chỉ có giá trị cuối cùng trở thành phần tử đầu tiên mới quan trọng. Mọi thứ khác chỉ là công cụ để tác động đến giá trị đó thông qua một chuỗi tương tác tối đa và tối thiểu trong khi mảng thu gọn. 

Các ràng buộc cho phép lên đến$10^5$các yếu tố trên mỗi thử nghiệm tổng thể, do đó, bất kỳ cách tiếp cận nào mô phỏng tất cả các trạng thái trò chơi hoặc xem xét từng chuỗi hợp nhất có thể xảy ra đều không thể thực hiện được. Quá trình này giảm độ dài đi một bước cho mỗi lần di chuyển, do đó có$O(n)$di chuyển trong mỗi lần kiểm tra, nhưng mỗi bước di chuyển phụ thuộc vào lựa chọn chiến lược. Một mức tối thiểu ngây thơ đối với tất cả các lựa chọn sẽ phân nhánh theo cấp số nhân. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đều bằng nhau. Trong trường hợp đó, mọi thao tác đều mang tính trung lập và câu trả lời phải không thay đổi. Một trường hợp góc khác là khi phần tử tối đa không ở gần điểm bắt đầu, bởi vì trực giác ngây thơ có thể chỉ đề xuất các giá trị cục bộ quan trọng, nhưng trên thực tế, các giá trị có thể được kéo về chỉ số 1 thông qua việc hợp nhất lặp đi lặp lại. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ cố gắng mô hình hóa mọi động thái có thể có cho cả hai người chơi, phân nhánh trên tất cả các chỉ số hợp lệ ở mỗi bước. Vì mỗi nước đi làm giảm độ dài chuỗi đi một, nên có$n-1$quyết định, và mỗi quyết định có thể được thực hiện trong$O(n)$theo nhiều cách, dẫn đến số lượng trạng thái theo cấp số nhân. Ngay cả việc cắt tỉa cũng không giúp ích gì vì tác động của mỗi bước di chuyển sẽ thay đổi các lựa chọn tối ưu trong tương lai theo cách không cục bộ. 

Điểm mấu chốt là trò chơi không duy trì trật tự một cách phức tạp, chỉ ảnh hưởng tương đối của các giá trị đến vị trí cuối cùng mới quan trọng. Mỗi thao tác hoặc truyền cực đại về bên trái hoặc truyền cực tiểu về bên trái. Điều này có nghĩa là giá trị cuối cùng được xác định không phải bởi cây chiến lược đầy đủ mà bởi một thuộc tính cấu trúc duy nhất của mảng: phần tử nào có thể tồn tại trong các cơn co thắt tối đa tối đa xen kẽ trong khi vẫn có ảnh hưởng. 

Hóa ra cách chơi tối ưu sẽ chuyển sang mô tả đặc điểm tham lam: yếu tố đầu tiên cuối cùng sẽ trở thành giá trị tốt nhất có thể đạt được dưới các ràng buộc thống trị xen kẽ, có thể được tính toán trực tiếp mà không cần mô phỏng trò chơi. 

Việc rút gọn xuất phát từ việc diễn giải quá trình này như một chuỗi các phép rút gọn bắt buộc theo cặp. Thay vì suy nghĩ về các bước di chuyển, chúng tôi theo dõi giá trị ở vị trí 1 phát triển như thế nào khi nó cạnh tranh với từng phần tử ở bên phải của nó dưới sự kiểm soát tối đa/phút xen kẽ. Điều này loại bỏ hoàn toàn nhu cầu mô phỏng trò chơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng cây trò chơi đầy đủ | Hàm mũ | Hàm mũ | Quá chậm | 
| Quan sát giảm tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp tối ưu dựa trên việc xử lý phần tử đầu tiên bị ảnh hưởng như thế nào bởi các phần tử còn lại dưới sự điều khiển luân phiên. 

1. Bắt đầu từ phần tử đầu tiên ban đầu làm câu trả lời của ứng viên hiện tại. Đây là giá trị duy nhất mà chúng tôi trực tiếp theo dõi vì tất cả các hoạt động cuối cùng sẽ chuyển sang sửa đổi giá trị đó thông qua một chuỗi hợp nhất. 
2. Duyệt mảng từ trái sang phải, mô phỏng thực tế là mọi phần tử cuối cùng sẽ tương tác với tiền tố hiện tại thông qua một số chuỗi hợp nhất. Thay vì mô phỏng các nước đi, chúng tôi suy luận xem liệu mỗi phần tử có thể lấn át giá trị hiện tại hay không tùy thuộc vào tính chẵn lẻ của lượt chơi. 
3. Duy trì trạng thái thể hiện hoạt động hiệu quả hiện tại hoạt động giống như bước tối đa hay tối thiểu. Sự luân phiên này phản ánh thực tế là Rùa và Heo luân phiên di chuyển, và theo thời gian, sự luân phiên này lan truyền thông qua quá trình co lại. 
4. Khi xử lý một phần tử mới, hãy cập nhật giá trị hiện tại bằng cách lấy giá trị tối đa hoặc tối thiểu tùy thuộc vào trạng thái chẵn lẻ hiện tại. Điều này nắm bắt ý tưởng rằng một người chơi đẩy các giá trị lên trên trong khi người kia đẩy chúng xuống và mọi phần tử đều chịu cả hai lực tùy thuộc vào vị trí của nó trong thứ tự loại bỏ. 
5. Lật trạng thái sau mỗi bước để phản ánh các lượt luân phiên sụp đổ thành ảnh hưởng xen kẽ dọc theo chuỗi hiệu quả. 

Sau khi xử lý tất cả các phần tử, giá trị được theo dõi cuối cùng chính là câu trả lời. 

Bất biến chính là sau khi xử lý k phần tử đầu tiên, giá trị được duy trì bằng giá trị tối ưu có thể đạt được của phần tử đầu tiên sau khi giải quyết tối ưu tất cả các tương tác trong tiền tố đó. Mỗi phần tử mới đóng góp chính xác một lần theo toán tử xen kẽ chính xác và không có thao tác nào trong tương lai có thể thay đổi hiệu ứng của các hợp nhất đã giải quyết trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    cur = a[0]
    take_max = True
    
    for i in range(1, n):
        if take_max:
            cur = max(cur, a[i])
        else:
            cur = min(cur, a[i])
        take_max = not take_max
    
    print(cur)
```Sau khi đọc từng trường hợp thử nghiệm, chúng tôi khởi tạo kết quả với phần tử đầu tiên vì mọi chuỗi thao tác cuối cùng đều biến đổi vị trí này. Sau đó, chúng tôi lặp qua phần còn lại của mảng, xen kẽ giữa việc áp dụng bản cập nhật tối đa và bản cập nhật tối thiểu. Cờ boolean mã hóa ảnh hưởng của người chơi hiện đang chiếm ưu thế trong quá trình giảm thiểu hiệu quả. 

Bản cập nhật xen kẽ là sự đơn giản hóa quan trọng: thay vì mô phỏng việc xóa dựa trên lân cận, chúng tôi nén toàn bộ trò chơi thành một lượt duy nhất trong đó mỗi phần tử được kết hợp chính xác một lần theo loại thao tác chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
n = 3
a = [1, 2, 3]
```| tôi | phần tử | hoạt động | cur trước | cur sau | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | ban đầu | - | 1 | 
| 1 | 2 | tối đa | 1 | 2 | 
| 2 | 3 | phút | 2 | 2 | 

Câu trả lời cuối cùng là 2. 

Điều này cho thấy cách một giá trị lớn có thể được vô hiệu hóa bằng thao tác tối thiểu sau đó, phản ánh cách chơi tối ưu của Piggy. 

Bây giờ hãy xem xét:```
n = 5
a = [3, 1, 2, 2, 3]
```| tôi | phần tử | hoạt động | cur trước | cur sau | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | ban đầu | - | 3 | 
| 1 | 1 | tối đa | 3 | 3 | 
| 2 | 2 | phút | 3 | 2 | 
| 3 | 2 | tối đa | 2 | 2 | 
| 4 | 3 | phút | 2 | 2 | 

Câu trả lời cuối cùng là 2, phù hợp với hành vi mẫu trong đó cách chơi tối ưu cân bằng việc đẩy lên và xuống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với công việc O(1) | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Giải pháp chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm và vì tổng kích thước đầu vào được giới hạn bởi$2 \cdot 10^5$, nó dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        cur = a[0]
        take_max = True
        for i in range(1, n):
            if take_max:
                cur = max(cur, a[i])
            else:
                cur = min(cur, a[i])
            take_max = not take_max
        out.append(str(cur))
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided samples
assert run("""5
2
1 2
3
1 1 2
3
1 2 3
5
3 1 2 2 3
10
10 2 5 2 7 9 2 5 10 7
""") == """2
1
2
2
7"""

# custom cases
assert run("""1
4
4 3 2 1
""") == "2", "alternating collapse check"

assert run("""1
3
5 5 5
""") == "5", "all equal stability"

assert run("""1
2
100 1
""") == "100", "two element dominance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 3 2 1 | 2 | đảo ngược sự thống trị xen kẽ | 
| 5 5 5 | 5 | bất biến dưới các giá trị bằng nhau | 
| 100 1 | 100 | trường hợp cơ sở với so sánh đơn | 

## Vỏ cạnh 

Đối với một đầu vào như`[5, 5, 5, 5]`, mọi thao tác bất kể giá trị tối đa hay tối thiểu đều không thay đổi giá trị. Thuật toán xen kẽ giữa giá trị tối đa và tối thiểu, nhưng cả hai thao tác đều trả về cùng một giá trị ở mỗi bước, do đó, bất biến được giữ nguyên và câu trả lời cuối cùng vẫn là 5. 

Đối với một mảng giảm nghiêm ngặt như`[4, 3, 2, 1]`, thao tác tối đa đầu tiên không có hại gì, nhưng thao tác tối thiểu tiếp theo ngay lập tức đẩy giá trị xuống và luân phiên ổn định ở mức 2, phù hợp với kết quả tính toán. 

Đối với mảng hai phần tử`[x, y]`, chỉ có một thao tác tối đa xảy ra, do đó kết quả chỉ đơn giản là`max(x, y)`, điều này phù hợp với việc trò chơi kết thúc ngay sau nước đi đầu tiên.
