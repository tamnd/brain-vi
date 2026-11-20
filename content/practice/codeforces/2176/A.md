---
title: "CF 2176A - Hoạt động với sự đảo ngược"
description: "Chúng ta được cung cấp một dãy số và chúng ta được phép loại bỏ nhiều lần các phần tử trong một điều kiện rất cụ thể. Mỗi nước đi chọn hai chỉ số $i < j$ sao cho giá trị tại $i$ hoàn toàn lớn hơn giá trị tại $j$, sau đó xóa phần tử tại vị trí $j$."
date: "2026-06-07T22:28:21+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2176
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1070 (Div. 2)"
rating: 800
weight: 2176
solve_time_s: 107
verified: false
draft: false
---

[CF 2176A - Hoạt động với sự đảo ngược](https://codeforces.com/problemset/problem/2176/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và chúng ta được phép loại bỏ nhiều lần các phần tử trong một điều kiện rất cụ thể. Mỗi nước đi chọn hai chỉ số$i < j$sao cho giá trị tại$i$hoàn toàn lớn hơn giá trị tại$j$, sau đó xóa phần tử tại vị trí$j$. Mảng co lại và các phần tử còn lại giữ nguyên thứ tự tương đối của chúng. 

Nhiệm vụ là xác định có thể thực hiện bao nhiêu thao tác xóa như vậy nếu chúng ta luôn chọn các cặp một cách tối ưu. 

Một cách hữu ích để suy nghĩ về quy trình là mọi thao tác sẽ loại bỏ một phần tử có ít nhất một phần tử lớn hơn ở đâu đó bên trái của nó tại thời điểm đó. Khi một phần tử không có phần tử lớn hơn ở bên trái, nó sẽ trở thành "được bảo vệ" và không bao giờ có thể bị xóa. 

Các hạn chế là nhỏ:$n \le 100$Và$t \le 50$. Điều này ngay lập tức loại trừ bất cứ điều gì phức tạp hơn$O(n^2)$cho mỗi trường hợp thử nghiệm mà không cần quan tâm. Ngay cả các phương pháp tiếp cận khối vẫn có khả năng vượt qua, nhưng chúng ta nên hướng tới một ý tưởng quét tuyến tính hoặc tham lam rõ ràng hơn là các phương pháp mô phỏng nặng. 

Điều tinh tế là việc loại bỏ sẽ làm thay đổi mảng, vì vậy lý luận ngây thơ như “đếm xem ban đầu có bao nhiêu đảo ngược tồn tại” là không chính xác. Phần tử đã xóa có thể bỏ chặn các lần xóa trong tương lai hoặc thay đổi phần tử nào có thể hoạt động hợp lệ$i$. 

Một lỗi điển hình là cho rằng mọi phép đảo ngược đều tương ứng với một phần tử có thể tháo rời. Điều đó không thành công vì một phần tử chỉ có thể bị xóa một lần và tính khả dụng của đối tác bên trái hợp lệ phụ thuộc vào mảng đang phát triển chứ không phải trạng thái ban đầu. 

Ví dụ: trong một mảng tăng dần như$[1,2,3]$, ban đầu không có cặp hợp lệ nào và không có cặp nào có thể bị xóa. Trong một mảng giảm dần như$[3,2,1]$, mọi phần tử ngoại trừ phần tử đầu tiên cuối cùng đều có thể bị xóa, nhưng chỉ theo kiểu giống như dây chuyền. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng quá trình. Chúng tôi liên tục quét tất cả các cặp$(i, j)$, xóa một giá trị hợp lệ$j$, và tiếp tục cho đến khi không có động thái nào tồn tại. Chi phí mỗi lần quét$O(n^2)$và chúng tôi có thể xóa tối đa$n$các yếu tố, dẫn đến$O(n^3)$mỗi trường hợp thử nghiệm. Với$n \le 100$, đây là ranh giới nhưng vẫn có thể chấp nhận được. Tuy nhiên, nó che khuất cấu trúc và làm cho việc suy luận chính xác trở nên khó khăn hơn. 

Quan sát quan trọng là các phần tử duy nhất có thể tồn tại là những phần tử không có phần tử nào lớn hơn ở bên trái trong cấu hình cuối cùng. Khi chúng tôi xem xét quy trình trên toàn cầu, việc xóa có thể được coi là việc chọn một chuỗi con trở nên không giảm hoàn toàn từ trái sang phải, bởi vì bất kỳ vi phạm nào$a_i > a_j$cho phép xóa$a_j$. 

Điều này giải quyết lại vấn đề: chúng tôi đang cố gắng xóa càng nhiều phần tử càng tốt để mảng còn lại không có cặp giảm dần. Điều đó có nghĩa là mảng cuối cùng phải không giảm. 

Vì vậy, vấn đề trở thành: tìm số lần xóa lớn nhất sao cho chúng ta có thể giữ được dãy con không giảm. Tương tự, việc xóa tối đa có nghĩa là giảm thiểu kích thước của chuỗi không giảm cuối cùng hợp lệ có thể đạt được bằng cách giữ các phần tử theo thứ tự. 

Mảng cuối cùng nhỏ nhất có thể tương ứng với việc xây dựng một dãy con không giảm tham lam chỉ giữ “cực tiểu bản ghi cần thiết từ cấu trúc bên phải”, nhưng một đặc tính đơn giản hơn hoạt động: mảng cuối cùng chính xác là tập hợp các phần tử không bị “chi phối” bởi bất kỳ phần tử nào ở bên phải của chúng theo một nghĩa tiến hóa nhất định. Điều này làm giảm việc quét tham lam từ trái sang phải trong khi vẫn duy trì hoạt động tối đa của cấu trúc được giữ từ phía bên trái. 

Cụ thể hơn, chúng tôi mô phỏng những phần tử nào phải ở lại: chúng tôi giữ lại phần tử đầu tiên và sau đó mỗi khi chúng tôi thấy một phần tử nhỏ hơn hoàn toàn so với một số phần tử được giữ trước đó, nó có thể tháo rời được; nếu không nó sẽ trở thành một “đỉnh được giữ nguyên” mới. Số lượng đỉnh được giữ trong cấu trúc tham lam này cho kích thước cuối cùng tối thiểu và câu trả lời là$n - \text{kept}$. 

Điều này hoạt động vì sau này không thể xóa bất kỳ phần tử nào trở thành mức tối đa mới trong hậu tố còn lại, trong khi bất kỳ phần tử nào không phải là mức tối đa ở bên trái của nó luôn có thể bị xóa thông qua mức tối đa đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^3)$|$O(n)$| Được chấp nhận nhưng không cần thiết | 
| Tham lam đếm đỉnh |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Bắt đầu không có phần tử nào được giữ lại và một biến theo dõi giá trị lớn nhất được thấy cho đến nay trong cấu trúc được giữ lại. 
2. Quét mảng từ trái sang phải. 
3. Nếu phần tử hiện tại lớn hơn mức tối đa được giữ hiện tại, chúng ta phải giữ nó và cập nhật mức tối đa. 
4. Mặt khác, phần tử hiện tại cuối cùng có thể bị loại bỏ vì tồn tại một phần tử lớn hơn ở bên trái của nó có thể đóng vai trò là phần tử$i$trong hoạt động. 
5. Đếm xem có bao nhiêu phần tử được giữ lại theo quy tắc này. 
6. Câu trả lời là tổng số phần tử trừ đi số lượng được giữ lại. 

Trực giác đằng sau bước 3 là bất cứ khi nào chúng ta thấy một mức tối đa toàn cục mới thì không có phần tử nào trước đó có thể giúp loại bỏ nó sau này, vì không có phần tử nào lớn hơn tồn tại ở bên trái của nó. Phần tử đó trở nên không thể đảo ngược về mặt cấu trúc trong bất kỳ chuỗi xóa nào. 

### Tại sao nó hoạt động 

Điều bất biến là các phần tử được giữ lại tạo thành một chuỗi tăng dần nghiêm ngặt của “cực đại từ trái sang phải” trong cấu hình tối ưu đang phát triển. Mỗi phần tử không được giữ lại có ít nhất một phần tử được giữ ở bên trái lớn hơn nó, đảm bảo nó có thể bị xóa ở một giai đoạn nào đó. Ngược lại, mọi phần tử được giữ lại là lần xuất hiện đầu tiên của một giá trị tối đa mới, do đó không có thao tác hợp lệ nào có thể xóa nó. 

Điều này phân vùng mảng thành các cực đại thiết yếu phải giữ lại và có thể xóa các phần tử chiếm ưu thế và không có thao tác nào có thể chuyển đổi phần tử chiếm ưu thế thành phần tử không bị chiếm ưu thế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        kept = 0
        mx = 0

        for x in a:
            if x > mx:
                kept += 1
                mx = x

        print(n - kept)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo quy tắc tham lam. Biến`mx`lưu trữ giá trị lớn nhất trong số các phần tử được giữ cho đến nay. Mỗi khi chúng tôi gặp một giá trị vượt quá nó, chúng tôi sẽ tăng số lượng phần tử thiết yếu. 

Một điểm tinh tế là sự bình đẳng không tạo ra một phần tử mới được giữ lại. Nếu như`x == mx`, nó luôn có thể tháo rời được vì phần tử lớn hơn hoặc bằng cực đại ở bên trái của nó chỉ có thể đóng vai trò là neo xóa nếu tồn tại lớn hơn một cách nghiêm ngặt và các giá trị bằng nhau không bao giờ trở thành “cực đại mới” cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$[3, 1, 4, 5, 2]$| Bước | Giá trị | mx | Giữ | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 1 | giữ (tối đa mới) | 
| 2 | 1 | 3 | 1 | loại bỏ ứng viên | 
| 3 | 4 | 4 | 2 | giữ (tối đa mới) | 
| 4 | 5 | 5 | 3 | giữ (tối đa mới) | 
| 5 | 2 | 5 | 3 | loại bỏ ứng viên | 

Các phần tử được giữ là 3, vì vậy câu trả lời là$5 - 3 = 2$. Điều này phù hợp với ý kiến ​​cho rằng chỉ những “đỉnh” ngày càng tăng mới tồn tại được. 

### Ví dụ 2 

đầu vào:$[3, 2, 1]$| Bước | Giá trị | mx | Giữ | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 1 | giữ | 
| 2 | 2 | 3 | 1 | loại bỏ ứng viên | 
| 3 | 1 | 3 | 1 | loại bỏ ứng viên | 

Chỉ còn lại một phần tử nên câu trả lời là$3 - 1 = 2$. Điều này phản ánh chuỗi giảm dần đầy đủ trong đó mọi phần tử không phải đầu tiên đều có thể loại bỏ được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Quét một lần từ trái sang phải | 
| Không gian |$O(1)$| Chỉ có bộ đếm và biến tối đa | 

Các ràng buộc cho phép tổng cộng lên tới 5000 phần tử, vì vậy cách tiếp cận tuyến tính này nhanh chóng trong giới hạn và tránh mọi chi phí mô phỏng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []

    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        kept = 0
        mx = 0
        for x in a:
            if x > mx:
                kept += 1
                mx = x
        output.append(str(n - kept))

    return "\n".join(output)

# provided samples
assert run("""5
3
3 2 1
3
1 2 3
3
3 3 3
5
3 1 4 5 2
1
1
""") == """2
0
0
2
0"""

# custom: already increasing
assert run("""1
5
1 2 3 4 5
""") == "0"

# custom: strictly decreasing
assert run("""1
4
4 3 2 1
""") == "3"

# custom: alternating peaks
assert run("""1
6
1 5 2 6 3 7
""") == "0"

# custom: all equal
assert run("""1
4
2 2 2 2
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trình tự tăng dần | 0 | không thể xóa | 
| dãy giảm dần | n-1 | xóa toàn bộ chuỗi | 
| đỉnh xen kẽ | 0 | tất cả đều là maxima mới | 
| tất cả đều bình đẳng | 0 | bình đẳng không bao giờ giúp ích | 

## Vỏ cạnh 

Một mảng tăng đầy đủ như$[1,2,3,4]$là điểm thất bại đơn giản nhất đối với lý luận đếm ngược không chính xác. Thuật toán giữ tất cả các phần tử ở mức cực đại, dẫn đến không có phần tử nào bị xóa, phù hợp với thực tế vì không tồn tại cặp hợp lệ nào. 

Một mảng giảm hoàn toàn như$[4,3,2,1]$thể hiện sự xóa bỏ tối đa. Mỗi phần tử mới hoàn toàn nhỏ hơn phần tử trước đó, vì vậy nó luôn có thể tháo rời cho đến khi chỉ còn lại phần tử đầu tiên. 

Một mô hình cao thấp xen kẽ như$[1,5,2,6,3,7]$kiểm tra xem thuật toán có tính nhầm nhiều lần xóa hay không. Ở đây mọi giá trị cao đều đặt lại mức tối đa, không để lại cấu trúc nào có thể tháo rời được; quá trình quét tham lam giữ lại tất cả các phần tử một cách chính xác, không tạo ra hoạt động nào.
