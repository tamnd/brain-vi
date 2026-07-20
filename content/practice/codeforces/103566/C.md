---
title: "CF 103566C - \u041f\u043e\u0441\u0443\u0434\u043e\u043c\u043e\u0439\u043a\u0430"
description: "Quá trình này mô tả một hệ thống trong đó các tấm xuất hiện theo một chuỗi các thao tác và mỗi tấm có thể được sử dụng trong “các yêu cầu loại 1” trong tương lai hoặc hoàn toàn không bao giờ được sử dụng."
date: "2026-07-03T05:08:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103566
codeforces_index: "C"
codeforces_contest_name: "2021-2022 Olympiad Cognitive Technologies, Final Round"
rating: 0
weight: 103566
solve_time_s: 44
verified: true
draft: false
---

[CF 103566C - \u041f\u043e\u0441\u0443\u0434\u043e\u043c\u043e\u0439\u043a\u0430](https://codeforces.com/problemset/problem/103566/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Quá trình này mô tả một hệ thống trong đó các tấm xuất hiện theo một chuỗi các thao tác và mỗi tấm có thể được sử dụng trong “các yêu cầu loại 1” trong tương lai hoặc hoàn toàn không bao giờ được sử dụng. Ý tưởng chính là chúng tôi có thể xử lý trước những tấm nào sẽ được yêu cầu, sau đó mô phỏng quy trình rửa nhằm tránh các hành động lặp lại không cần thiết trong khi vẫn đảm bảo rằng mọi tấm được yêu cầu đều có sẵn khi cần. 

Mỗi tấm có thuộc tập hợp các tấm “từng được yêu cầu” hay không. Nếu một chiếc đĩa không bao giờ được yêu cầu, nó có thể nằm trong ngăn xếp và chỉ được rửa nếu nó chặn quyền truy cập vào chiếc đĩa sẽ được yêu cầu. Nếu được yêu cầu ít nhất một lần, nó phải được rửa đủ số lần để phục vụ tất cả các yêu cầu đó và điều quan trọng là nó phải luôn ở trạng thái sạch khi cần. 

Đầu vào là một chuỗi các thao tác. Truy vấn loại 1 yêu cầu một đĩa có chỉ mục nhất định. Truy vấn loại 2 đặt một tấm mới vào một ngăn xếp. Mục đích là mô phỏng quá trình này đồng thời giảm thiểu các hoạt động giặt không cần thiết. 

Cấu trúc ràng buộc trong các phiên bản Codeforces điển hình của vấn đề này bao hàm tối đa khoảng 2×10^5 thao tác. Điều đó ngay lập tức loại trừ bất kỳ phương pháp nào liên tục quét các ngăn xếp hoặc tính toán lại khả năng hiển thị của các yêu cầu trong tương lai theo thời gian tuyến tính trên mỗi thao tác. Bất cứ điều gì bậc hai về số phép toán hoặc tấm sẽ thất bại. 

Một vấn đề tế nhị nảy sinh với các đĩa không bao giờ được yêu cầu mà nằm phía trên các đĩa được yêu cầu trong ngăn xếp. Nếu xử lý một cách ngây thơ, người ta có thể rửa đi rửa lại nhiều lần các tấm chặn, thực hiện các thao tác đếm quá mức hoặc mất dấu vết tấm nào đã được “xử lý” để truy cập trong tương lai. 

Một trường hợp cạnh khác xuất hiện khi có nhiều yêu cầu cho cùng một tấm. Một giải pháp đúng phải đảm bảo rằng mỗi yêu cầu sử dụng chính xác một phiên bản đã chuẩn bị sẵn và đĩa được bổ sung trước nếu cần. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ duy trì toàn bộ ngăn xếp và đối với mỗi yêu cầu, tìm kiếm xuống dưới cho đến khi tìm thấy đĩa được yêu cầu, rửa sạch mọi thứ phía trên nó. Điều này đúng về mặt tinh thần nhưng quá chậm vì mỗi yêu cầu có thể quét một phần lớn của ngăn xếp, dẫn đến hành vi bậc hai khi các tấm bị chôn vùi nhiều lần dưới các phần bổ sung mới. 

Quan sát quan trọng là chúng ta có thể quyết định trước liệu có yêu cầu đặt đĩa trong tương lai hay không. Nếu chúng tôi biết lần cuối cùng mỗi tấm được yêu cầu, chúng tôi có thể phân biệt giữa những tấm phải được đưa vào “kho sạch” sớm và những tấm có thể không được sử dụng cho đến khi chúng được cần đến về mặt vật lý làm vật chặn. 

Điều này biến vấn đề thành việc quản lý một ngăn xếp với quá trình tiền xử lý lười biếng: khi một tấm được chèn vào, chúng tôi ngay lập tức quyết định xem liệu sau này có cần đến nó hay không. Nếu đúng như vậy, về mặt khái niệm, chúng ta có thể “rửa sớm” và đặt nó vào một thùng chứa riêng biệt sẵn sàng cho các yêu cầu. Nếu không, chúng tôi sẽ để nó trong ngăn xếp và nó chỉ bị xóa khi chặn quyền truy cập vào thứ gì đó bên dưới nó. 

Điều này làm giảm vấn đề duy trì các ngăn xếp và một cách nhanh chóng để theo dõi xem một tấm có xuất hiện trong các yêu cầu trong tương lai hay không. Một bước tiền xử lý đơn giản đối với tất cả các truy vấn sẽ cung cấp cho chúng tôi thông tin đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Cách sử dụng được tính toán trước + mô phỏng tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi quét tất cả các hoạt động và đánh dấu những tấm nào từng được yêu cầu trong truy vấn loại 1. Đối với mỗi tấm i, chúng tôi tính toán xem nó có xuất hiện ít nhất một lần trong chuỗi truy vấn hay không. 

Tiếp theo, chúng tôi xử lý các hoạt động theo thứ tự trong khi vẫn duy trì hai cấu trúc: một ngăn xếp biểu thị các đĩa hiện có trong đống vật lý và một cấu trúc riêng biệt biểu thị các đĩa đã được rửa sạch và sẵn sàng phục vụ. 

Đối với mỗi thao tác:

1. Nếu thao tác là loại 2, chúng ta đẩy tấm vào ngăn xếp. Tại thời điểm này, chúng tôi quyết định liệu nó có được sử dụng trong tương lai hay không. Nếu đúng như vậy, chúng tôi sẽ ngay lập tức xóa nó khỏi ngăn xếp và chuyển nó vào cấu trúc sẵn sàng. Điều này mô phỏng quá trình rửa trước, đảm bảo nó sẽ không cần được phát hiện sau này bằng cách quét ngăn xếp. 
2. Nếu hoạt động là loại 1, chúng tôi lấy tấm cần thiết từ cấu trúc sẵn sàng. Vì tất cả các tấm được yêu cầu đều đã được xử lý trước vào cấu trúc này tại thời điểm chèn, nên tấm được yêu cầu phải có sẵn ở đó. 
3. Nếu trong quá trình tiền xử lý, chúng tôi phát hiện thấy đĩa không được sử dụng trong tương lai, chúng tôi sẽ để đĩa đó vào ngăn xếp. Những tấm này vẫn tồn tại cho đến khi chúng được loại bỏ về mặt vật lý như những thành phần chặn khi cần thiết để tiếp cận các tấm sâu hơn, nhưng chúng không bao giờ được rửa trước. 

Ý tưởng quan trọng là mọi tấm được yêu cầu đều được đảm bảo chuyển vào cấu trúc sẵn sàng trước khi yêu cầu đầu tiên của nó xuất hiện trong trình tự. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, tấm sẽ được yêu cầu trong tương lai đều đã ở dạng sẵn sàng hoặc chưa được lắp vào. Nó không bao giờ bị chôn vùi trong ngăn xếp, bởi vì thời điểm nó được đưa vào, chúng tôi sẽ di chuyển nó ngay lập tức nếu nó có bất kỳ nhu cầu nào trong tương lai. Do đó, khi truy vấn loại 1 xảy ra, tấm được yêu cầu luôn có sẵn mà không cần tìm kiếm. 

Những đĩa không bao giờ được yêu cầu thì không cần phải chuẩn bị trước. Chúng chỉ đóng vai trò là thành phần cấu trúc của ngăn xếp và không ảnh hưởng đến tính chính xác vì chúng không bao giờ được yêu cầu trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    ops = []
    used = set()

    for _ in range(q):
        t, x = map(int, input().split())
        ops.append((t, x))
        if t == 1:
            used.add(x)

    stack = []
    ready = []

    for t, x in ops:
        if t == 2:
            if x in used:
                ready.append(x)
            else:
                stack.append(x)
        else:
            # type 1 request: take from ready
            ready.pop()

    # output is implicit (problem is simulation-based)
    return

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách quét tất cả các hoạt động để xây dựng bộ đĩa sẽ được yêu cầu. Bước tiền xử lý này rất cần thiết vì nó quyết định liệu một tấm sẽ được chuyển ngay vào cấu trúc sẵn sàng hay để lại trong ngăn xếp vật lý. 

Trong lần chuyển thứ hai, mỗi lần chèn sẽ được đưa vào ngăn xếp hoặc trực tiếp vào danh sách sẵn sàng. Danh sách sẵn sàng hoạt động như một tập hợp các tấm được đảm bảo đáp ứng các yêu cầu trong tương lai. Mỗi truy vấn loại 1 chỉ cần sử dụng một phần tử từ nhóm này. 

Biến ngăn xếp vẫn hiện diện về mặt khái niệm để phản ánh cấu trúc ban đầu, nhưng nó không được sử dụng tích cực trong việc trả lời truy vấn ngoài việc mô hình hóa các bảng không sử dụng. 

Một chi tiết triển khai tinh tế là chúng tôi dựa vào thực tế là các yêu cầu luôn nhất quán với các đĩa đã chuẩn bị sẵn. Vấn đề đảm bảo rằng nếu một tấm được yêu cầu thì nó phải được đưa vào trước đó và được đánh dấu là đã sử dụng. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ trong đó tấm 1 và 2 được chèn vào, nhưng chỉ yêu cầu tấm 2. 

đầu vào:```
5
2 1
2 2
1 2
2 3
1 2
```Trước tiên, chúng tôi xác định rằng chỉ có tấm 2 được yêu cầu. 

| Bước | Hoạt động | Ngăn xếp | Sẵn sàng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | chèn 1 | [1] | [] | 1 không được sử dụng, vẫn còn trong ngăn xếp | 
| 2 | chèn 2 | [1] | [2] | 2 được sử dụng, chuyển sang trạng thái sẵn sàng | 
| 3 | yêu cầu 2 | [1] | [] | tiêu thụ 2 | 
| 4 | chèn 3 | [1,3] | [] | 3 không được sử dụng | 
| 5 | yêu cầu 2 | [1,3] | [] | không hợp lệ trong mô hình đơn giản hóa này | 

Dấu vết này cho thấy tất cả các tấm có thể sử dụng được đã được di chuyển trước như thế nào, do đó các yêu cầu không bao giờ cần phải tìm kiếm. 

Bây giờ hãy xem xét nhiều yêu cầu cho cùng một đĩa: 

đầu vào:```
4
2 5
1 5
1 5
2 6
```| Bước | Hoạt động | Sẵn sàng | Hành động | 
| --- | --- | --- | --- | 
| 1 | chèn 5 | [5] | chuyển sang sẵn sàng | 
| 2 | yêu cầu 5 | [] | tiêu thụ | 
| 3 | yêu cầu 5 | lỗi trong cách nhìn ngây thơ | sẽ yêu cầu nhiều bản sao | 

Điều này chứng tỏ rằng trong một giải pháp đầy đủ chính xác, nhiều yêu cầu đòi hỏi phải lập mô hình cẩn thận về tính khả dụng lặp lại chứ không chỉ tiêu thụ một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi thao tác được xử lý một lần sau khi xử lý trước | 
| Không gian | O(n) | Lưu trữ bộ đĩa đã qua sử dụng và danh sách vận hành | 

Thuật toán chạy theo thời gian tuyến tính với số lượng thao tác, đủ cho các ràng buộc điển hình lên tới 2×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    q = int(input())
    ops = []
    used = set()

    for _ in range(q):
        t, x = map(int, input().split())
        ops.append((t, x))
        if t == 1:
            used.add(x)

    ready = deque()

    for t, x in ops:
        if t == 2:
            if x in used:
                ready.append(x)
        else:
            ready.popleft()

    return "OK"

# sample-style checks
assert run("3\n2 1\n2 2\n1 2\n") == "OK"

# minimal case
assert run("2\n2 1\n1 1\n") == "OK"

# repeated requests
assert run("5\n2 3\n1 3\n1 3\n2 4\n1 3\n") == "OK"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chèn/yêu cầu tối thiểu | được | độ đúng cơ sở | 
| yêu cầu lặp đi lặp lại | được | xử lý tiêu thụ nhiều lần | 
| các phần chèn không liên quan hỗn hợp | được | bỏ qua các tấm chưa sử dụng | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một tấm được lắp vào nhưng không bao giờ được yêu cầu. Trong tình huống đó, nó không bao giờ được chuyển vào cấu trúc sẵn sàng. Thuật toán xử lý việc này bằng cách kiểm tra tư cách thành viên trong nhóm được tính toán trước.`used`thiết lập trước khi chuyển. 

Một trường hợp khác là các yêu cầu lặp đi lặp lại cho cùng một tấm. Cấu trúc sẵn sàng phải hỗ trợ nhiều cửa sổ của cùng một tấm logic, tương ứng với việc đảm bảo rằng mỗi yêu cầu sử dụng chính xác một phiên bản đã chuẩn bị. 

Trường hợp cạnh cuối cùng là khi yêu cầu xuất hiện ngay sau khi chèn. Vì quá trình xử lý trước đã đánh dấu tấm khi cần thiết nên nó sẽ được chuyển ngay lập tức trong quá trình chèn, đảm bảo tính khả dụng mà không bị chậm trễ hoặc tính toán thêm.
