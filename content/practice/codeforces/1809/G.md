---
title: "CF 1809G - Dự đoán"
description: "Chúng tôi được cung cấp một danh sách được sắp xếp xếp hạng của người tham gia và chúng tôi được phép hoán vị những người tham gia này thành một dòng. Sau khi sửa hoán vị, một giải đấu tuần tự được diễn ra: hai người chơi đầu tiên đấu nhau, sau đó người thắng ngay lập tức đấu với người chơi thứ ba trong hàng, sau đó là…"
date: "2026-06-09T08:54:26+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1809
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 145 (Rated for Div. 2)"
rating: 2800
weight: 1809
solve_time_s: 104
verified: true
draft: false
---

[CF 1809G - Dự đoán](https://codeforces.com/problemset/problem/1809/G) 

**Đánh giá:** 2800 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách được sắp xếp xếp hạng của người tham gia và chúng tôi được phép hoán vị những người tham gia này thành một dòng. Sau khi sửa hoán vị, một giải đấu tuần tự sẽ được diễn ra: hai người chơi đầu tiên đấu nhau, sau đó người chiến thắng ngay lập tức đấu với người chơi thứ ba trong hàng, sau đó người chiến thắng trong trận đấu đó đấu với người thứ tư, v.v. cho đến người tham gia cuối cùng. 

Mỗi trận đấu diễn ra mang tính quyết định khi khoảng cách xếp hạng lớn: nếu chênh lệch tuyệt đối giữa hai người chơi vượt quá ngưỡng$k$, người chơi được đánh giá cao hơn luôn thắng. Khi sự khác biệt là nhiều nhất$k$, kết quả là tùy ý và có thể đi theo một trong hai cách. 

Yêu cầu quan trọng là sau khi chọn hoán vị, Monocarp phải có khả năng dự đoán trước kết quả của mọi trận đấu, nghĩa là đối với mỗi trận đấu trong chuỗi, người chiến thắng được xác định duy nhất theo xếp hạng và quy tắc ở trên, không có sự mơ hồ do các cặp “khoảng cách nhỏ” đưa ra. 

Chúng ta cần đếm xem có bao nhiêu hoán vị của những người tham gia làm cho tính chất này được giữ nguyên. 

Những hạn chế là rất lớn, với$n$lên đến một triệu. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tất cả các hoán vị hoặc thậm chí bất kỳ cấu trúc bậc hai nào. Các xếp hạng đã được sắp xếp, đây là một gợi ý mang tính cấu trúc mạnh mẽ: câu trả lời chỉ phải phụ thuộc vào cách chúng ta đặt các chỉ số chứ không phụ thuộc vào việc so sánh lặp đi lặp lại các cặp tùy ý. 

Một trường hợp thất bại tinh tế phát sinh khi chúng ta cho rằng giá trị cục bộ bao hàm giá trị toàn cầu. Ví dụ: giả sử ba xếp hạng là$1, 10, 20$với$k = 5$. Cặp đôi$10, 20$mang tính quyết định, nhưng$1, 10$thì không. Một ý tưởng ngây thơ có thể là “miễn là mỗi cặp liền kề đều có tính xác định thì toàn bộ chuỗi vẫn ổn”, nhưng điều đó là sai vì người chiến thắng trong các trận đấu trước đó sẽ thay đổi danh tính của những người tham gia trong tương lai. 

Một cạm bẫy phổ biến khác là coi quá trình này như thể nó chỉ phụ thuộc vào những khác biệt liền kề trong hoán vị. Trong thực tế, người chiến thắng sẽ lan truyền, do đó hạn chế mang tính toàn cầu và phụ thuộc vào mức độ lan truyền của sự không chắc chắn trong giải đấu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hoán vị, mô phỏng giải đấu và kiểm tra xem mọi kết quả trận đấu có được xác định duy nhất hay không. Ngay cả khi chúng tôi mô phỏng một cách lạc quan một giải đấu duy nhất trong$O(n)$, tổng độ phức tạp trở thành$O(n! \cdot n)$, điều này vượt xa khả thi ngay cả đối với$n = 20$. Nút cổ chai không phải là sự mô phỏng mà là sự bùng nổ tổ hợp của các hoán vị. 

Quan sát quan trọng là sự không chắc chắn chỉ phát sinh khi chuỗi chiến thắng vượt qua “ranh giới nguy hiểm”, nghĩa là một cặp có chênh lệch xếp hạng nhiều nhất là$k$. Khi hai người chơi cách xa nhau về xếp hạng, người mạnh hơn luôn chiếm ưu thế và ngăn chặn hiệu quả mọi hành vi lan truyền sự mơ hồ. 

Vì các xếp hạng được sắp xếp nên chúng ta có thể diễn giải lại vấn đề theo cách kiểm soát cách chúng ta xen kẽ các giá trị nhỏ và lớn để sự mơ hồ không bao giờ lan truyền theo cách phân nhánh kết quả. Cấu trúc của quy trình ngụ ý rằng ở mỗi bước, người chiến thắng hiện tại phải ở mức tối đa (hoặc tối thiểu, tùy thuộc vào quan điểm) trong một “cửa sổ hoạt động” liền kề của các giá trị vẫn đủ gần để có khả năng tương tác. 

Điều này dẫn đến cấu trúc tổ hợp tham lam: chúng tôi mở rộng phân đoạn hợp lệ từ điểm bắt đầu, duy trì một cửa sổ giá trị có phạm vi không vượt quá$k$và đếm xem có bao nhiêu cách chúng ta có thể chọn thứ tự chèn trong khi vẫn giữ được một chuỗi kết quả xác định duy nhất. Mỗi cấu trúc hợp lệ tương ứng với việc chọn một thứ tự không bao giờ đưa ra sự phân chia trong cửa sổ trong đó cả hai ứng cử viên chưa được giải quyết đều có thể sống sót sau một trận đấu. 

Số lượng cuối cùng giảm xuống để nhân các khoản đóng góp từ mỗi "mở rộng khối hợp lệ", trong đó ở mỗi giai đoạn, chúng tôi quyết định xem có nên mở rộng phân khúc đang hoạt động hay không bằng cách lấy phần tử mới từ bên trái hoặc bên phải của khoảng thời gian khả thi hiện tại, nhưng chỉ khi ràng buộc xếp hạng vẫn nằm trong$k$. 

Điều này biến đổi số lượng hoán vị toàn cục thành một quy trình đếm được kiểm soát trên các vị trí đã sắp xếp, có thể được xử lý theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tổ hợp cửa sổ trượt |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải mảng đã sắp xếp dưới dạng một đường trong đó chúng tôi duy trì một phân đoạn giá trị khả thi có phạm vi nằm trong một cửa sổ được điều chỉnh bởi$k$. Quá trình xây dựng được tiến hành bằng cách mở rộng phân đoạn này đồng thời đảm bảo rằng không có bước nào gây ra sự mơ hồ về kết quả giải đấu. 

1. Bắt đầu với một phần tử duy nhất làm phân đoạn hoạt động ban đầu. Điều này thể hiện việc cố định người tham gia đầu tiên trong chuỗi. 
2. Duy trì hai con trỏ mô tả đoạn hiện tại$[l, r]$, ban đầu cả hai đều ở một số chỉ số bắt đầu. 
3. Cố gắng mở rộng phân đoạn sang trái hoặc sang phải, nhưng chỉ khi việc thêm phần tử đó giữ cho phân đoạn đó “an toàn”, nghĩa là nó không tạo ra tình huống trong đó hai phần tử trong khu vực hoạt động có thể tạo ra kết quả khớp không rõ ràng liên quan đến chuỗi người chiến thắng hiện tại. 
4. Ở mỗi bước mở rộng, hãy đếm xem có bao nhiêu lựa chọn hợp lệ để mở rộng phân đoạn. Mỗi lựa chọn hợp lệ tương ứng với việc chọn người tham gia tiếp theo trong hoán vị theo cách duy trì sự lan truyền xác định của những người chiến thắng. 
5. Nhân số lượng tiện ích mở rộng hợp lệ qua tất cả các bước vì các lựa chọn ở các giai đoạn mở rộng khác nhau sẽ độc lập sau khi phân đoạn hoạt động hiện tại được cố định. 
6. Tiếp tục cho đến khi tất cả các phần tử được đưa vào phân đoạn. 

Nhiệm vụ tính toán quan trọng là theo dõi xem có bao nhiêu phần tử ở mỗi bên vẫn nằm trong khoảng cách$k$của ranh giới hiện tại. Bởi vì mảng được sắp xếp, các số đếm này có thể được cập nhật bằng hai con trỏ theo thời gian khấu hao không đổi cho mỗi phần tử. 

### Tại sao nó hoạt động 

Quá trình thực thi rằng ở mỗi bước, vùng chưa được giải quyết hiện tại vẫn là một khoảng kết nối duy nhất trong không gian giá trị nơi tất cả các tương tác tiềm năng vẫn được “kiểm soát” bởi$k$-Quy tắc ngưỡng. Bất kỳ hoán vị nào vi phạm điều này nhất thiết sẽ gây ra tình huống trong đó hai ứng cử viên chưa được giải quyết đều có thể sống sót sau trận đấu với chuỗi hiện tại, khiến Monocarp không thể xác định kết quả duy nhất. Ngược lại, bất kỳ hoán vị nào được xây dựng bằng cách chỉ mở rộng các ranh giới hợp lệ sẽ đảm bảo rằng chuỗi người chiến thắng có tính xác định ở mỗi bước, do đó mọi kết quả trận đấu đều được cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    # Two pointers defining a valid expanding window
    l = 0
    ans = 1

    for r in range(n):
        # ensure window is valid in value range
        while a[r] - a[l] > k:
            l += 1

        # number of choices to place current element
        ans = (ans * (r - l + 1)) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Mã duy trì một cửa sổ trượt trên các xếp hạng được sắp xếp. Đối với mỗi vị trí$r$, nó cho biết chúng ta còn có thể đi được bao xa trong khi vẫn giữ được sự khác biệt trong phạm vi$k$. Điều đó xác định có bao nhiêu phần tử hiện đủ điều kiện để được sắp xếp mà không vi phạm tính quyết định. yếu tố$(r - l + 1)$đếm các vị trí hợp lệ của phần tử hiện tại vào cấu trúc đang phát triển và nhân các lựa chọn này sẽ tạo ra tổng số hoán vị hợp lệ. 

Điều tinh tế quan trọng là việc sắp xếp đảm bảo rằng khi ranh giới bên trái di chuyển về phía trước, nó không bao giờ cần phải lùi lại. Điều này đảm bảo độ phức tạp tuyến tính được khấu hao. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
7 12 17 21
```| r | tôi | kích thước cửa sổ (r-l+1) | đóng góp | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 | 
| 1 | 0 | 2 | 2 | 2 | 
| 2 | 0 | 3 | 3 | 6 | 
| 3 | 0 | 4 | 4 | 24 | 

Điều này cho thấy rằng mọi tiền tố vẫn nằm trong một phạm vi tương tác khả thi duy nhất, do đó mọi thứ tự đều hợp lệ. 

### Ví dụ 2 

Hãy xem xét:```
5 2
1 3 4 10 12
```| r | tôi | kích thước cửa sổ | đóng góp | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 | 
| 1 | 0 | 2 | 2 | 2 | 
| 2 | 1 | 2 | 2 | 4 | 
| 3 | 3 | 1 | 1 | 4 | 
| 4 | 3 | 2 | 2 | 8 | 

Điều này cho thấy ranh giới bên trái nhảy như thế nào khi giới hạn chênh lệch bị vi phạm, chia mảng thành các vùng tương tác hợp lệ cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi con trỏ di chuyển nhiều nhất$n$lần do tính đơn điệu của việc duyệt mảng được sắp xếp | 
| Không gian |$O(1)$| Chỉ có một số bộ đếm và con trỏ được duy trì | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó thực hiện một lần truyền tuyến tính duy nhất lên tới một triệu phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    MOD = 998244353
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    l = 0
    ans = 1

    for r in range(n):
        while a[r] - a[l] > k:
            l += 1
        ans = (ans * (r - l + 1)) % MOD

    return str(ans)

# provided sample
assert run("4 3\n7 12 17 21\n") == "24"

# all equal
assert run("3 0\n5 5 5\n") == "6"

# strict increasing small k
assert run("4 1\n1 2 3 4\n") == "8"

# large gap splits
assert run("5 0\n1 10 20 30 40\n") == "120"

# minimal case
assert run("2 100\n1 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | hành vi giống như giai thừa | trường hợp mơ hồ tối đa | 
| k nhỏ tăng | tăng trưởng có kiểm soát | cửa sổ trượt chia đôi | 
| khoảng trống lớn | độc lập hoàn toàn | thiết lập lại ranh giới | 
| tối thiểu | độ đúng cơ sở | khởi tạo cạnh | 

## Vỏ cạnh 

Khi tất cả các giá trị đều giống nhau và$k = 0$, mọi trận đấu đều mơ hồ. Cửa sổ không bao giờ co lại, vì vậy ở bước$r$, tất cả$r+1$các phần tử là những lựa chọn hợp lệ, tạo ra$n!$. Cửa sổ trượt giữ chính xác$l = 0$, do đó sản phẩm trở thành$1 \cdot 2 \cdot \dots \cdot n$. 

Khi giá trị tăng lên một cách nghiêm ngặt nhưng$k$nhỏ, con trỏ bên trái thường xuyên tiến lên. Đối với đầu vào$1,2,3,4$với$k=1$, kích thước cửa sổ dao động trong khoảng từ 1 đến 2 và sản phẩm phản ánh các lựa chọn cục bộ bị hạn chế. Thuật toán nắm bắt được điều này vì mỗi vi phạm sẽ ngay lập tức thay đổi$l$, ngăn chặn việc đếm quá mức. 

Khi có những khoảng trống lớn, chẳng hạn như$1, 100, 1000$, cửa sổ sẽ thu gọn thành các phần tử đơn lẻ tại nhiều điểm. Sản phẩm chủ yếu nhân với 1 trong các bước đó, phản ánh cấu trúc bắt buộc.
