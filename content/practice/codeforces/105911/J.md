---
title: "CF 105911J - Ớt cay"
description: "Chúng tôi được cho một số điểm trên một lưới vô hạn. Mỗi điểm đại diện cho một hạt tiêu được đặt ở một tọa độ cụ thể và mỗi hạt tiêu có một loại cố định. Một loại tương tác dọc theo cột của nó, có nghĩa là nó “quan tâm” đến các loại ớt khác có cùng tọa độ x."
date: "2026-06-21T12:13:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105911
codeforces_index: "J"
codeforces_contest_name: "2025 ICPC Nanchang Invitational and Jiangxi Provincial Collegiate Programming Contest"
rating: 0
weight: 105911
solve_time_s: 55
verified: true
draft: false
---

[CF 105911J - Ớt cay](https://codeforces.com/problemset/problem/105911/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cho một số điểm trên một lưới vô hạn. Mỗi điểm đại diện cho một hạt tiêu được đặt ở một tọa độ cụ thể và mỗi hạt tiêu có một loại cố định. Một loại tương tác dọc theo cột của nó, có nghĩa là nó “quan tâm” đến các loại ớt khác có cùng tọa độ x. Loại còn lại tương tác dọc theo hàng của nó, nghĩa là nó “quan tâm” đến những quả ớt có cùng tọa độ y. 

Yêu cầu là mỗi loại hạt tiêu hiện có phải được “hỗ trợ” bởi ít nhất một loại hạt tiêu khác thuộc loại tương tác chính xác. Cụ thể, một hạt tiêu kiểu dọc (tương tác dọc theo các cột) cần ít nhất một hạt tiêu kiểu dọc khác trong cùng một cột nhưng ở một hàng khác. Một hạt tiêu loại ngang (tương tác dọc theo hàng) cần ít nhất một hạt tiêu loại ngang khác trong cùng một hàng nhưng ở một cột khác. Nếu một hạt tiêu chưa có đối tác như vậy, chúng tôi được phép thêm ớt mới vào bất kỳ đâu trên lưới, chọn cả vị trí và loại của chúng, đồng thời chúng tôi muốn giảm thiểu số lượng chúng tôi thêm vào. 

Các ràng buộc cho phép tối đa 2×10^5 quả ớt trong tất cả các trường hợp thử nghiệm và tọa độ lên tới 10^9. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng mô phỏng các tương tác theo cặp giữa tất cả các điểm hoặc tìm kiếm trực tiếp trên lưới. Bất cứ điều gì bậc hai trong k sẽ quá chậm và thậm chí các hệ số log trên mỗi cặp sẽ ở mức giới hạn nếu được thực hiện một cách ngây thơ. 

Trường hợp cạnh tinh tế xuất hiện khi một cấu trúc gần như hoàn chỉnh nhưng chưa hoàn toàn. Ví dụ: nếu có chính xác một hạt tiêu dọc trong một cột thì không thể đáp ứng yêu cầu nếu không thêm ít nhất một hạt tiêu dọc nữa vào cùng cột đó. Một cách tiếp cận ngây thơ chỉ kiểm tra số lượng toàn cầu hoặc chỉ kiểm tra xem một hàng/cột có tồn tại hay không sẽ bỏ lỡ điều này. 

Tương tự, hãy xem xét một cột có hai quả ớt thẳng đứng. Cả hai đều đã hài lòng rồi, vì mỗi người đều có một đối tác trong cùng một cột. Một chiến lược tham lam bất cẩn vẫn có thể cố gắng bổ sung thêm ớt một cách không cần thiết nếu nó không nhận ra rằng sự thỏa mãn lẫn nhau đã được đảm bảo. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là mô phỏng rõ ràng sự hài lòng đối với từng hạt tiêu. Đối với mỗi loại ớt, chúng tôi sẽ quét tất cả các loại ớt khác và kiểm tra xem đối tác hợp lệ có tồn tại trong cùng hàng hoặc cột với đúng loại hay không. Điều này trực tiếp tuân theo điều kiện nhưng tốn O(k^2) cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất, điều này hoàn toàn không khả thi khi k đạt 2×10^5. 

Quan sát quan trọng là sự hài lòng của một loại hạt tiêu không phụ thuộc vào cấu hình toàn cầu, mà chỉ phụ thuộc vào việc liệu có tồn tại ít nhất một loại hạt tiêu khác trong một lớp tương đương cụ thể hay không. Phân vùng ớt dọc theo tọa độ x và phân vùng ớt ngang theo tọa độ y. Trong mỗi nhóm như vậy, điều duy nhất quan trọng là số lượng ớt thuộc loại đó. 

Một khi chúng ta cô lập được cấu trúc này, vấn đề sẽ trở thành cục bộ. Đối với mỗi tọa độ x, chúng ta chỉ quan tâm có bao nhiêu quả ớt dọc tồn tại ở đó. Đối với mỗi tọa độ y, chúng ta chỉ quan tâm có bao nhiêu quả ớt nằm ngang tồn tại ở đó. Yêu cầu tương tác giảm xuống một điều kiện ngưỡng đơn giản: một nhóm hợp lệ nếu kích thước của nó bằng 0 hoặc ít nhất là hai. Một nhóm có kích thước một là trường hợp có vấn đề duy nhất, vì nó buộc chúng ta phải thêm chính xác một phần tử nữa để khắc phục. 

Không cần có sự tương tác giữa các nhóm vì ớt được thêm vào luôn có thể được đặt độc lập vào bất kỳ hàng hoặc cột nào được yêu cầu. Điều này loại bỏ bất kỳ khớp nối nào giữa các tọa độ khác nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^2) | O(k) | Quá chậm | 
| Đếm theo hàng/cột | O(k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách vấn đề thành hai nhiệm vụ đếm độc lập: một cho các quả ớt dọc được nhóm theo cột và một cho các quả ớt ngang được nhóm theo hàng.

1. Chúng tôi duyệt qua tất cả các loại ớt đã cho và duy trì hai bản đồ băm. Một bản đồ đếm xem có bao nhiêu quả ớt dọc xuất hiện trong mỗi cột x. Người còn lại đếm xem mỗi hàng xuất hiện bao nhiêu quả ớt ngang y. Bước này nén tất cả thông tin hình học thành thống kê tần số. 
2. Sau khi xử lý xong toàn bộ số ớt, chúng ta kiểm tra từng cột trên bản đồ dọc. Nếu một cột chứa chính xác một hạt tiêu dọc thì hạt tiêu đó hiện không có đối tác nào trong cột của nó. Để khắc phục điều này, chúng ta phải thêm ít nhất một hạt tiêu dọc bổ sung vào đâu đó trong cùng một cột. Nếu số lượng từ hai hạt tiêu trở lên thì không cần thực hiện hành động nào vì mỗi hạt tiêu trong cột đó đã có ít nhất một hạt tiêu đồng hành. 
3. Chúng tôi lặp lại lý do tương tự cho các hàng trong bản đồ ngang. Bất kỳ hàng nào chứa chính xác một hạt tiêu ngang đều đóng góp một phép cộng bắt buộc. 
4. Chúng tôi tính tổng tất cả các trường hợp đơn lẻ như vậy từ cả hai bản đồ. Tổng số này là số lượng ớt tối thiểu cần thêm vào. 

Lựa chọn thiết kế quan trọng ở đây là mỗi nhóm vấn đề được khắc phục một cách độc lập. Việc thêm hạt tiêu mới để cố định cột đơn không ảnh hưởng đến các cột khác vì nó được đặt ở tọa độ đã chọn và chỉ ảnh hưởng đến nhóm cụ thể đó. 

### Tại sao nó hoạt động 

Điều bất biến là mọi cấu hình hợp lệ phải đảm bảo rằng trong mỗi cột, các quả ớt dọc được nhóm thành các thành phần có kích thước ít nhất là hai và trong mỗi hàng, các quả ớt ngang được nhóm tương tự. Bất kỳ nhóm nào có quy mô một đều bị cô lập và không thể được thỏa mãn trong nội bộ, vì vậy nó nhất thiết buộc phải có ít nhất một sự bổ sung bên ngoài. Ngược lại, việc thêm chính xác một hạt tiêu mới vào mỗi nhóm đơn lẻ luôn là đủ, vì nó tạo ra một cặp hợp lệ bên trong nhóm đó mà không ảnh hưởng đến tính chính xác ở nơi khác. Vì các nhóm không tương tác trên các tọa độ nên việc tính tổng các hiệu chỉnh độc lập mang lại kết quả tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        k = int(input())
        
        col_v = defaultdict(int)
        row_h = defaultdict(int)

        for _ in range(k):
            x, y, w = map(int, input().split())
            if w == 0:
                col_v[x] += 1
            else:
                row_h[y] += 1

        ans = 0

        for x, c in col_v.items():
            if c == 1:
                ans += 1

        for y, c in row_h.items():
            if c == 1:
                ans += 1

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai sẽ nén lưới thành hai bản đồ tần số, một bản đồ được khóa bằng x cho các hạt tiêu dọc và một bản đồ có khóa y cho các hạt tiêu ngang. Mỗi mục bản đồ đại diện trực tiếp cho một nhóm ràng buộc. Vòng lặp cuối cùng chỉ đơn giản là đếm xem có bao nhiêu nhóm trong số đó có kích thước chính xác bằng một, tương ứng với số lượng bổ sung bắt buộc. 

Một lỗi triển khai phổ biến ở đây là cố gắng mô phỏng việc thêm ớt lặp đi lặp lại. Điều đó đòi hỏi các cấu trúc phải cập nhật liên tục và có thể vô tình dẫn đến việc cập nhật tính hai lần hoặc xếp tầng. Cách tiếp cận đếm tránh được điều đó hoàn toàn vì nó tính toán trạng thái cuối cùng trực tiếp từ phân phối ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét các quả ớt dọc ở các cột có số lượng`[3, 1, 2]`và ớt nằm ngang theo hàng`[1, 2, 1]`. 

| Bước | trạng thái col_v | trạng thái row_h | đóng góp | 
| --- | --- | --- | --- | 
| ban đầu | {a:3,b:1,c:2} | {d:1,e:2} | - | 
| quét cột | singleton tại b | - | +1 | 
| quét hàng | - | singleton tại d | +1 | 
| kết quả | - | - | 2 | 

Điều này cho thấy chỉ những nhóm biệt lập mới quan trọng chứ không phải những cụm lớn hơn. 

### Ví dụ 2 

Giả sử tất cả các cột dọc đã có ít nhất hai quả ớt:`{x1:2, x2:4}`và các hàng ngang là`{y1:3}`. 

| Bước | trạng thái col_v | trạng thái row_h | đóng góp | 
| --- | --- | --- | --- | 
| ban đầu | {x1:2,x2:4} | {y1:3} | - | 
| quét cột | không có người độc thân | - | 0 | 
| quét hàng | - | không có người độc thân | 0 | 
| kết quả | - | - | 0 | 

Điều này xác nhận rằng khi mỗi nhóm có ít nhất hai người thì không cần bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) mỗi trường hợp thử nghiệm | Mỗi hạt tiêu được xử lý một lần và mỗi bản đồ được quét một lần | 
| Không gian | O(k) | Lưu trữ số tần số trên mỗi tọa độ | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng số quả ớt, vừa vặn thoải mái trong giới hạn 2 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    from collections import defaultdict

    t = int(input())
    out = []

    for _ in range(t):
        k = int(input())
        col_v = defaultdict(int)
        row_h = defaultdict(int)

        for _ in range(k):
            x, y, w = map(int, input().split())
            if w == 0:
                col_v[x] += 1
            else:
                row_h[y] += 1

        ans = 0
        for c in col_v.values():
            if c == 1:
                ans += 1
        for c in row_h.values():
            if c == 1:
                ans += 1

        out.append(str(ans))

    return "\n".join(out)

# single vertical singleton
assert run("1\n1\n10 10 0\n") == "1"

# already satisfied pair
assert run("1\n2\n1 1 0\n1 2 0\n") == "0"

# mixed independent rows and columns
assert run("1\n4\n1 1 0\n1 2 0\n2 1 1\n3 1 1\n") == "0"

# two independent singletons
assert run("1\n2\n1 1 0\n2 2 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dọc đơn | 1 | cột đơn yêu cầu sửa chữa | 
| cột ghép nối | 0 | size ≥ 2 không cần bổ sung | 
| hỗn hợp cân bằng | 0 | sự độc lập của các nhóm hàng/cột | 
| hai người độc thân | 2 | tính chất phụ gia của câu trả lời | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi nhóm tọa độ có kích thước chính xác bằng một. Ví dụ: 

đầu vào:```
1
1
5 5 0
```Thuật toán phân loại cột x=5 có một hạt tiêu dọc. Trong quá trình xử lý, nó sẽ tăng câu trả lời lên một. Không có đóng góp hàng nào tồn tại, vì vậy đầu ra cuối cùng là 1. Tính chính xác xuất phát từ thực tế là một nút không thể đáp ứng yêu cầu nếu không đưa vào ít nhất một nút bổ sung trong cột của nó. 

Một trường hợp khác là khi có nhiều nhóm đơn lẻ tồn tại trong các cột hoặc hàng khác nhau: 

đầu vào:```
1
3
1 1 0
2 2 0
3 3 1
```Ở đây mỗi nhóm độc lập. Thuật toán đếm ba cấu trúc đơn và trả về 3. Mỗi mức tăng tương ứng với việc sửa một yêu cầu bị ngắt kết nối và vì các phần bổ sung có thể được đặt độc lập nên không có sự chồng chéo hoặc tính kép. 

Trường hợp khó phát hiện thứ ba là khi một nhóm đã có hai phần tử nhưng nằm rải rác ở nhiều vị trí. Ngay cả khi tọa độ khác nhau, miễn là chúng có cùng x (đối với chiều dọc) hoặc cùng y (đối với chiều ngang) thì chúng đã thỏa mãn nhau. Thuật toán xử lý việc này một cách tự nhiên vì nó chỉ kiểm tra số lượng chứ không kiểm tra phân bổ không gian trong nhóm.
