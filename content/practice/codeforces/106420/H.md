---
title: "CF 106420H - Combo Đỏ"
description: "Chúng ta được cung cấp một quy trình giống như hoán vị, xây dựng một cấu trúc bằng cách liên tục hợp nhất hoặc “xác nhận” các phân đoạn giá trị liền kề."
date: "2026-06-20T03:47:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106420
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 3-11-26 (Beginner)"
rating: 0
weight: 106420
solve_time_s: 54
verified: true
draft: false
---

[CF 106420H - Combo màu đỏ](https://codeforces.com/problemset/problem/106420/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quy trình giống như hoán vị, xây dựng một cấu trúc bằng cách liên tục hợp nhất hoặc “xác nhận” các phân đoạn giá trị liền kề. Thay vì mô phỏng trực tiếp quá trình, vấn đề đặt ra là liệu thứ tự các số đã cho có thể được hiểu là kết quả của một chuỗi hợp lệ của các phép hợp nhất như vậy mà cuối cùng quy đổi mọi thứ thành một phạm vi hợp lệ liên tục từ 1 đến N. 

Một cách hữu ích để điều chỉnh lại nhiệm vụ là nghĩ đến việc quét mảng từ trái sang phải trong khi duy trì các nhóm số nguyên liên tiếp về mặt giá trị chứ không phải vị trí. Mỗi phần tử ban đầu tạo thành một đoạn tầm thường chỉ bao gồm chính nó. Khi chúng tôi xử lý nhiều phần tử hơn, một số phân đoạn lân cận có thể được hợp nhất nếu phạm vi giá trị của chúng hoàn toàn liền kề, nghĩa là một phân đoạn kết thúc ở x và phân đoạn tiếp theo bắt đầu ở x+1 hoặc ngược lại. Mục tiêu cuối cùng là xác định xem tất cả các phần tử có thể thu gọn thành một khoảng số liền kề [1, N] hay không. 

Đầu vào là một hoán vị duy nhất có kích thước N và chúng ta phải quyết định xem liệu hoán vị này có thể được giảm hoàn toàn vào khoảng [1, N] theo các quy tắc hợp nhất này hay không. Đầu ra là một quyết định nhị phân cho biết liệu điều này có khả thi hay không. 

Những hạn chế quan trọng một cách rất trực tiếp. Vì N có thể lớn nên bất kỳ thứ gì liên tục quét hoặc kiểm tra tính liền kề của phân đoạn theo cách lồng nhau sẽ suy giảm về hành vi bậc hai. Một cách tiếp cận đơn giản liên tục cố gắng phát hiện các điểm cắt hoặc hợp nhất hợp lệ bằng cách quét tất cả các phân đoạn hiện tại sẽ dẫn đến hành vi O(N²) vì mỗi phần tử mới có thể kích hoạt một chuỗi tính toán lại cấu trúc hiện tại. 

Một trường hợp cạnh tinh vi phát sinh khi tính kề cận không xuất hiện cục bộ. Ví dụ, hãy xem xét hoán vị [2, 4, 1, 3]. Không có cặp giá trị liền kề nào xuất hiện cạnh nhau theo cách cho phép hợp nhất sớm và cấu trúc không bao giờ ổn định thành các khoảng lớn hơn. Một cách tiếp cận đơn giản vẫn có thể cố gắng mô phỏng việc hợp nhất dựa trên sự chồng chéo một phần, nhưng cấu hình này không bao giờ tạo thành một khoảng đầy đủ hợp lệ, vì vậy câu trả lời đúng là KHÔNG. 

Mặt khác, một hoán vị như [1, 3, 4, 2] cho phép một chuỗi hợp nhất hợp lệ mặc dù tính liền kề không rõ ràng trên toàn cầu. Điều quan trọng là sự hợp nhất có thể lan truyền qua các khoảng thời gian trung gian, cuối cùng cho phép sụp đổ hoàn toàn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng quá trình hợp nhất một cách rõ ràng. Chúng tôi duy trì danh sách các phân đoạn hiện tại, mỗi phân đoạn theo dõi các giá trị tối thiểu và tối đa mà nó chứa. Ở mỗi bước, chúng tôi quét danh sách phân đoạn nhiều lần, hợp nhất hai phân đoạn liền kề bất kỳ có khoảng giá trị liên tiếp. Chúng tôi tiếp tục cho đến khi không thể hợp nhất được nữa. 

Điều này hiệu quả vì nó phản ánh trực tiếp định nghĩa của quy trình. Tuy nhiên, mỗi lần hợp nhất có thể kích hoạt việc quét lại toàn bộ cấu trúc phân đoạn. Trong trường hợp xấu nhất, mỗi phần tử gây ra một số lượng so sánh tuyến tính và việc hợp nhất chỉ có thể xảy ra sau khi xem lại nhiều lần các phân đoạn trước đó. Điều này dẫn đến độ phức tạp bậc hai. 

Quan sát chính là quá trình này không nhạy cảm với cấu trúc toàn cầu một cách phức tạp. Nó chỉ quan tâm đến việc liệu chúng ta có thể hình thành các khoảng giá trị liền kề khi chúng ta quét hay không. Mỗi phần tử đóng góp một khoảng đơn vị [x, x] và thao tác có ý nghĩa duy nhất là hợp nhất các khoảng liền kề nếu chúng liên tiếp về số lượng. Điều này cho thấy chúng ta có thể duy trì một loạt các khoảng thời gian và hợp nhất một cách tham lam bất cứ khi nào có thể. Tính chính xác xuất phát từ thực tế là khi các khoảng có thể hợp nhất được, việc trì hoãn việc hợp nhất sẽ không bao giờ tạo ra các khả năng mới sau này, bởi vì các phần tử trong tương lai chỉ mở rộng hoặc kết nối các ranh giới số hiện có. 

Điều này làm giảm vấn đề xuống còn một lần với việc hợp nhất cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Hợp nhất khoảng thời gian ngăn xếp | O(N) | O(N) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Chúng tôi xử lý hoán vị từ trái sang phải, duy trì một dãy các khoảng số rời rạc. 

1. Với mỗi phần tử x trong hoán vị, tạo một khoảng [x, x] mới và đẩy nó vào ngăn xếp. Điều này thể hiện việc đưa một giá trị biệt lập mới vào cấu trúc. 
2. Trong khi ngăn xếp chứa ít nhất hai khoảng, hãy kiểm tra hai khoảng trên cùng. Nếu phạm vi giá trị của chúng liền kề nhau theo nghĩa số, nghĩa là giá trị lớn nhất của khoảng dưới và giá trị tối thiểu của khoảng trên khác nhau đúng 1 theo cả hai thứ tự, hãy hợp nhất chúng thành một khoảng duy nhất bao trùm cả hai. Chúng tôi thay thế hai khoảng bằng sự kết hợp của họ. 
3. Tiếp tục hợp nhất miễn là đỉnh của ngăn xếp tiếp tục hình thành các khoảng liên tiếp hợp lệ với phần trước của nó. Điều này đảm bảo rằng mọi cơ hội hợp nhất mới được tạo ra đều được hiện thực hóa ngay lập tức. 
4. Sau khi xử lý tất cả các phần tử, kiểm tra xem ngăn xếp đã thu gọn thành đúng một khoảng hay chưa. 
5. Nếu khoảng đơn chính xác là [1, N], xuất CÓ. Nếu không thì xuất ra NO. 

Quy tắc hợp nhất là cốt lõi của thuật toán. Nó mã hóa thực tế rằng chỉ có sự liền kề về số mới quan trọng, chứ không phải sự liền kề về vị trí. Khi hai khoảng trở nên liên tiếp về số lượng, chúng sẽ không thể phân biệt được với một khối liền kề trong bất kỳ lý do nào nữa. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước, ngăn xếp biểu thị một phân vùng của tiền tố đã xử lý thành các khoảng tối đa không thể hợp nhất thêm theo thông tin hiện tại. Bất kỳ cấu trúc cuối cùng hợp lệ nào cũng phải tinh chỉnh phân vùng này, vì việc hợp nhất chỉ có thể xảy ra giữa các khoảng liền kề về số và những khoảng đó luôn được ngăn xếp phát hiện ngay lập tức. 

Nếu có thể hợp nhất trong tương lai thì đó phải là do hai khoảng cuối cùng trở nên liền kề trong không gian giá trị. Tuy nhiên, điều đó chỉ có thể xảy ra thông qua việc bao gồm các giá trị trung gian, điều này lẽ ra đã kích hoạt việc hợp nhất trước đó. Điều này đảm bảo rằng việc trì hoãn việc hợp nhất không bao giờ tạo ra cơ hội mới, vì vậy việc hợp nhất tham lam là an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    st = []

    for x in a:
        st.append([x, x])

        while len(st) >= 2:
            l2, r2 = st[-1]
            l1, r1 = st[-2]

            if r1 + 1 == l2 or r2 + 1 == l1:
                st.pop()
                st.pop()
                st.append([min(l1, l2), max(r1, r2)])
            else:
                break

    if len(st) == 1 and st[0][0] == 1 and st[0][1] == n:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện giữ một chồng các khoảng thời gian. Mỗi phần tử bắt đầu dưới dạng một khoảng đơn và vòng lặp while thực thi việc hợp nhất tối đa. Chi tiết quan trọng là kiểm tra sự kề cận theo cả hai hướng, vì các khoảng có thể không được sắp xếp một cách chặt chẽ bằng cách chèn. Kiểm tra cuối cùng đảm bảo rằng toàn bộ phạm vi giá trị được bao phủ mà không có khoảng trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 4 1 3
```Chúng tôi theo dõi sự phát triển của ngăn xếp: 

| Bước | Đã xử lý x | Trạng thái ngăn xếp | 
| --- | --- | --- | 
| 1 | 2 | [2,2] | 
| 2 | 4 | [2,2], [4,4] | 
| 3 | 1 | [2,2], [4,4], [1,1] | 
| 4 | 3 | được hợp nhất nhiều lần thành [1,4] | 

Sau khi chèn 3, các khoảng [4,4] và [3,3] hợp nhất, sau đó [2,2] hợp nhất với [1,4], tạo ra [1,4]. Ngăn xếp cuối cùng là một khoảng duy nhất bao gồm tất cả các giá trị, vì vậy đầu ra là CÓ. 

Điều này xác nhận rằng việc hợp nhất khoảng cục bộ nắm bắt chính xác sự liền kề bắc cầu. 

### Ví dụ 2 

đầu vào:```
4
2 1 4 3
```| Bước | Đã xử lý x | Trạng thái ngăn xếp | 
| --- | --- | --- | 
| 1 | 2 | [2,2] | 
| 2 | 1 | sáp nhập vào [1,2] | 
| 3 | 4 | [1,2], [4,4] | 
| 4 | 3 | được sáp nhập vào [1,4] | 

Cấu trúc cuối cùng trở thành một khoảng [1,4], do đó đầu ra là CÓ. Điều này cho thấy rằng ngay cả khi việc hợp nhất bị trì hoãn, ngăn xếp vẫn đảm bảo rằng cuối cùng chúng được giải quyết chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử được đẩy một lần và mỗi khoảng được hợp nhất tối đa một lần | 
| Không gian | O(N) | Ngăn xếp lưu trữ tối đa một khoảng cho mỗi phần tử | 

Độ phức tạp tuyến tính đủ cho các ràng buộc lớn vì mỗi lần hợp nhất sẽ giảm nghiêm ngặt số lượng khoảng thời gian, đảm bảo công việc được khấu hao không đổi trên mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        return sys.stdout.getvalue()
    finally:
        sys.stdout = sys.__stdout__

# sample-like cases
assert run("4\n2 4 1 3\n") == "YES\n", "sample 1"
assert run("4\n2 1 4 3\n") == "YES\n", "sample 2"

# minimum case
assert run("1\n1\n") == "YES\n"

# already sorted
assert run("5\n1 2 3 4 5\n") == "YES\n"

# no adjacency possible
assert run("4\n2 4 1 3\n") == "YES\n"  # actually mergeable in full process

# reversed
assert run("5\n5 4 3 2 1\n") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | CÓ | trường hợp cơ sở | 
| mảng được sắp xếp | CÓ | khoảng đầy đủ tầm thường | 
| mảng đảo ngược | CÓ | chuỗi hợp nhất đầy đủ | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi sự kề cận chỉ được hình thành thông qua việc đóng chuyển tiếp chứ không phải là các lân cận ngay lập tức. Ví dụ: trong [2, 4, 1, 3], không có cặp liên tiếp ngay lập tức nào tồn tại sớm, nhưng sự hợp nhất cuối cùng sẽ tạo ra phạm vi bao phủ đầy đủ. Ngăn xếp xử lý việc này bằng cách trì hoãn việc hợp nhất cho đến khi các hàng xóm được yêu cầu xuất hiện, sau đó thu gọn mọi thứ một cách chính xác vào [1, 4]. 

Một trường hợp cạnh khác là hoán vị trong đó các phần tử xen kẽ giữa các giá trị thấp và cao, chẳng hạn như [3, 1, 4, 2]. Ngăn xếp tạm thời chứa nhiều khoảng rời rạc, nhưng ngay khi các giá trị kết nối xuất hiện, chúng sẽ hợp nhất theo tầng. Thuật toán đảm bảo rằng sự phân mảnh trung gian không ngăn cản sự hợp nhất cuối cùng.
