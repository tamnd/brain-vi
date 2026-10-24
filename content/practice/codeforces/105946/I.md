---
title: "CF 105946I - Thủ môn vs Người ghi đè"
description: "Chúng ta được cung cấp một chuỗi các thao tác ghi được áp dụng cho một mảng thưa có kích thước $n$, ban đầu trống. Mỗi thao tác ghi một giá trị $x$ vào vị trí $i$. Điều khó khăn là hai hệ thống khác nhau diễn giải việc ghi đè khác nhau."
date: "2026-06-21T22:08:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105946
codeforces_index: "I"
codeforces_contest_name: "2025 UP ACM Algolympics Final Round"
rating: 0
weight: 105946
solve_time_s: 60
verified: true
draft: false
---

[CF 105946I - Người giữ bóng và Người ghi đè](https://codeforces.com/problemset/problem/105946/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thao tác ghi được áp dụng cho một mảng có kích thước thưa thớt$n$, ban đầu trống rỗng. Mỗi thao tác ghi một giá trị$x$vào vị trí$i$. Điều khó khăn là hai hệ thống khác nhau diễn giải việc ghi đè khác nhau. Một hệ thống từ chối ghi đè lên vị trí đã được lấp đầy, trong khi hệ thống kia luôn thay thế giá trị cũ. 

Chúng ta được phép xóa một số thao tác trước khi thực hiện. Sau khi xóa, cả hai hệ thống đều xử lý các lệnh còn lại nhưng theo quy tắc ghi đè của riêng chúng. Chúng tôi muốn các mảng cuối cùng thu được khớp chính xác: mọi vị trí được lấp đầy phải chứa cùng một giá trị trong cả hai hệ thống và cả hai phải đồng ý về vị trí nào được lấp đầy. 

Nhiệm vụ là loại bỏ số lượng thao tác tối thiểu sao cho, mặc dù ngữ nghĩa ghi đè khác nhau, cả hai cách diễn giải đều tạo ra trạng thái cuối cùng giống hệt nhau. 

Những ràng buộc làm cho khó khăn chính trở nên rõ ràng. Số lượng hoạt động lên tới$2 \cdot 10^5$, do đó, bất kỳ cách tiếp cận nào so sánh các hoạt động theo cặp hoặc mô phỏng các quyết định trên các tập hợp con đều quá chậm. Kích thước mảng$n$có thể lớn như$10^9$, ngụ ý rằng chúng tôi không thể lưu trữ trạng thái của mỗi vị trí; chỉ những vị trí thực sự xuất hiện trong hoạt động mới quan trọng. 

Một cách tiếp cận đơn giản sẽ thử các tập hợp con của lệnh hoặc mô phỏng cả hai hệ thống trong khi phân nhánh các lựa chọn nên giữ lại thao tác nào. Điều đó ngay lập tức trở thành cấp số nhân. Ngay cả việc tái cấu trúc theo vị trí tham lam mà không sắp xếp cẩn thận cũng thất bại vì các giá trị khác nhau cạnh tranh trên cùng một chỉ mục và sau đó ghi đè sẽ tương tác trên cả hai cách diễn giải. 

Trường hợp cạnh tinh tế xuất hiện khi cùng một vị trí nhận được nhiều giá trị khác nhau: 

đầu vào:```
1 3
1 2
1 3
1 2
```Nếu chúng ta cố gắng giữ lại tất cả các lệnh, hai hệ thống sẽ phân kỳ vì một hệ thống có thể từ chối ghi đè trong khi hệ thống kia chấp nhận chúng, tạo ra các giá trị cuối cùng khác nhau tùy thuộc vào thao tác nào "được thực hiện". Câu trả lời đúng yêu cầu loại bỏ các lệnh một cách có chọn lọc để cả hai cách diễn giải đều “đồng ý” một cách hiệu quả về việc cách viết nào là lệnh có ý nghĩa cuối cùng cho mỗi vị trí. 

## Phương pháp tiếp cận 

Khó khăn chính là mỗi chỉ mục hoạt động độc lập nhưng chỉ sau khi chúng tôi hiểu ý nghĩa của tính nhất quán trên toàn cầu trên cả hai hệ thống. 

Đầu tiên chúng ta hãy xem xét sức mạnh vũ phu. Chúng tôi có thể thử loại bỏ các tập hợp con của hoạt động và mô phỏng chính xác cả hai hệ thống. Đối với mỗi tập hợp con, chúng tôi áp dụng các lệnh theo thứ tự, theo dõi hai mảng với các quy tắc ghi đè khác nhau. Sau khi xử lý, chúng tôi so sánh kết quả. Điều này hoạt động vì nó tuân theo định nghĩa trực tiếp, nhưng nó chậm một cách vô vọng. có$2^q$tập hợp con, và thậm chí đánh giá chi phí của một tập hợp con$O(q)$, dẫn đến điều không thể thực hiện được$O(q \cdot 2^q)$. 

Chúng ta cần hiểu đặc tính nào thực sự phá vỡ tính nhất quán. Tập trung vào một chỉ mục duy nhất$i$. Giả sử chúng ta xem xét tất cả các hoạt động ảnh hưởng đến$i$, theo thứ tự. Với mỗi giá trị$x$, cuối cùng chỉ một lần xuất hiện có thể “quan trọng” ở trạng thái nhất quán cuối cùng, bởi vì mọi lần ghi xung đột trước đó đều phải bị loại bỏ hoặc bỏ qua. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì nghĩ đến việc xóa các thao tác để buộc phải thỏa thuận, hãy nghĩ đến những thao tác nào phải được giữ lại để cả hai hệ thống thống nhất về "lần ghi cuối cùng có hiệu lực" cuối cùng cho mỗi chỉ mục. Sự bất đồng chỉ nảy sinh khi nhiều giá trị riêng biệt cạnh tranh ở cùng một chỉ mục. Trong mỗi chỉ mục, cách duy nhất để đảm bảo sự đồng thuận là chọn một chuỗi các lần ghi trở nên nhất quán theo cả ngữ nghĩa ghi đè, điều này làm giảm việc giữ một tập hợp các lần ghi không xung đột được lựa chọn cẩn thận. 

Nếu chúng ta nhóm các phép toán theo chỉ mục thì mỗi nhóm sẽ trở thành một chuỗi các giá trị. Chúng tôi muốn giữ một dãy con sao cho khi áp dụng, cả hai hệ thống đều tạo ra cùng một giá trị cuối cùng. Điều đó có nghĩa là giá trị được lưu giữ cuối cùng ở mỗi chỉ mục phải giống nhau bất kể hành vi ghi đè, vì vậy, chúng tôi đang chọn một “bản ghi đại diện cuối cùng” nhất quán cho mỗi chỉ mục và đảm bảo tất cả các hoạt động được lưu giữ đều tương thích với nó. 

Cấu trúc tối ưu sẽ trở nên rõ ràng hơn nếu chúng ta xử lý các phép toán theo thứ tự ngược lại. Khi quét lùi, chúng tôi quyết định xem liệu lần ghi này có phải là lần đầu tiên chúng tôi nhìn thấy một giá trị ở chỉ mục đó hay không. Nếu đúng như vậy, chúng tôi có thể giữ nó làm người đại diện cuối cùng cho vị trí đó. Bất kỳ thao tác ghi xung đột nào trước đó có thể ghi đè lên nó đều phải được loại bỏ. 

Điều này biến thành logic “lựa chọn lần xuất hiện cuối cùng” cổ điển: đối với mỗi chỉ mục, chỉ lần xuất hiện cuối cùng của mỗi giá trị mới có thể phù hợp và trong số đó, chúng tôi chọn phép gán hiệu quả mới nhất để xác định trạng thái cuối cùng. Mọi hoạt động xung đột khác phải được xóa. 

Do đó, vấn đề giảm xuống còn việc theo dõi, trên mỗi chỉ mục, những giá trị mà chúng tôi đã cam kết giữ làm “ứng cử viên cuối cùng” và tính tất cả các hoạt động khác là xóa trừ khi chúng thiết lập nhiệm vụ nhất quán đầu tiên từ phía sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force |$O(q \cdot 2^q)$|$O(q)$| Quá chậm | 
| Quét ngược với tính năng theo dõi được nhìn thấy lần cuối |$O(q)$|$O(q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các hoạt động được nhóm theo chỉ mục nhưng được triển khai trong một lần chuyển từ cuối để chúng tôi có thể xác định lần ghi có ý nghĩa cuối cùng cho mỗi cặp vị trí-giá trị. 

1. Thực hiện các thao tác từ lệnh cuối cùng đến lệnh đầu tiên. Điều này đảm bảo rằng khi chúng ta gặp một cặp$(i, x)$, chúng ta đang thấy lần cuối cùng nhiệm vụ chính xác này có thể quan trọng. 
2. Duy trì một cuốn từ điển`seen[i]`lưu trữ liệu chỉ mục$i$đã chấp nhận một giá trị cuối cùng. Nếu không, chúng tôi cũng theo dõi giá trị nào được chỉ định. 
3. Duy trì một bộ`used[i]`hoặc trên toàn cầu một bản đồ đánh dấu xem một cặp cụ thể$(i, x)$đã được xem xét rồi. Điều này ngăn cản việc đếm nhiều lần xuất hiện của cùng một giá trị tại cùng một chỉ mục. 
4. Đối với mỗi thao tác$(i, x)$, nếu chỉ số$i$chưa được gán giá trị cuối cùng, chúng tôi chấp nhận thao tác này như là thao tác ghi xác định cho chỉ mục và nhãn hiệu đó$i$như được phân công. 
5. Nếu chỉ số$i$đã được chỉ định nhưng giá trị hiện tại khác với giá trị cuối cùng đã chọn, thao tác này không thể cùng tồn tại trong một giải pháp nhất quán và phải được tính là bị bỏ qua. 
6. Đếm tất cả các hoạt động không được chấp nhận là góp phần tạo nên cấu hình nhất quán cuối cùng; số này là câu trả lời. 

Ý tưởng chính là lần đầu tiên chúng ta gặp một chỉ mục trong khi quét ngược, chúng ta đang chọn giá trị cuối cùng của nó một cách hiệu quả. Mọi thao tác xung đột sau này (sớm hơn theo thứ tự ban đầu) đều không liên quan và phải được loại bỏ. 

### Tại sao nó hoạt động 

Khi chúng tôi sửa giá trị cuối cùng của một chỉ mục bằng cách gặp phải lần xuất hiện ngược đầu tiên của nó, mọi giá trị khác tại chỉ mục đó sẽ ghi đè lên nó theo một cách hiểu hoặc bị bỏ qua theo cách hiểu khác. Sự không phù hợp này chính xác là nguyên nhân gây ra sự phân kỳ. Bằng cách cam kết giá trị gặp đầu tiên theo thứ tự ngược lại, chúng tôi đảm bảo rằng tất cả các hoạt động được chấp nhận còn lại đều nhất quán với một phép gán cuối cùng duy nhất cho mỗi chỉ mục, loại bỏ mọi khả năng có sự bất đồng giữa ngữ nghĩa ghi đè. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    ops = [tuple(map(int, input().split())) for _ in range(q)]
    
    assigned = {}
    used = set()
    removed = 0

    for i in range(q - 1, -1, -1):
        idx, val = ops[i]
        if idx not in assigned:
            assigned[idx] = val
        else:
            if assigned[idx] != val:
                removed += 1

    print(removed)

if __name__ == "__main__":
    solve()
```Mã đọc tất cả các hoạt động và xử lý chúng theo thứ tự ngược lại. Từ điển`assigned`ghi lại giá trị được chọn cuối cùng cho mỗi chỉ mục. Lần đầu tiên chúng ta thấy một chỉ mục đảo ngược là giá trị được cố định cho chỉ mục đó. Bất kỳ lần gặp nào sau này với một giá trị khác ở cùng một chỉ mục đều được tính là một thao tác phải được bỏ qua. 

Biến`used`là không cần thiết trong giải pháp đơn giản hóa cuối cùng vì khi một chỉ mục được chỉ định, chúng tôi chỉ quan tâm đến tính nhất quán của giá trị của nó chứ không phải sự xuất hiện nhiều lần của các giá trị giống hệt nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 6
1 2
2 3
5 2
5 1
2 4
4 6
```Chúng tôi xử lý ngược. 

| Bước | Hoạt động | Trạng thái được chỉ định | Quyết định | Đã xóa | 
| --- | --- | --- | --- | --- | 
| 1 | (4,6) | 4→6 | giao | 0 | 
| 2 | (2,4) | 2→4 | giao | 0 | 
| 3 | (5,1) | 5→1 | giao | 0 | 
| 4 | (5,2) | 5→1 | xung đột | 1 | 
| 5 | (2,3) | 2→4 | xung đột | 2 | 
| 6 | (1,2) | 1→2 | giao | 2 | 

Câu trả lời cuối cùng là 2. 

Dấu vết này cho thấy cách mỗi chỉ mục khóa vào giá trị cuối cùng của nó trong lần đầu tiên nó được nhìn thấy từ cuối. Bất kỳ thao tác viết xung đột nào trước đó đều bị loại bỏ vì chúng không thể cùng tồn tại với nhiệm vụ cuối cùng đã chọn. 

### Ví dụ 2 

đầu vào:```
3 3
1 2
2 3
3 2
```| Bước | Hoạt động | Trạng thái được chỉ định | Quyết định | Đã xóa | 
| --- | --- | --- | --- | --- | 
| 1 | (3,2) | 3→2 | giao | 0 | 
| 2 | (2,3) | 2→3 | giao | 0 | 
| 3 | (1,2) | 1→2 | giao | 0 | 

Tất cả các giá trị đều khác biệt trên mỗi chỉ mục, do đó không xuất hiện xung đột. Cả hai hệ thống đều đồng ý một cách tầm thường sau khi giữ tất cả các hoạt động. 

Điều này cho thấy trường hợp đặc biệt trong đó mỗi chỉ mục xuất hiện một lần và không cần xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q)$| Mỗi thao tác được xử lý một lần theo thứ tự ngược lại với các thao tác từ điển O(1) | 
| Không gian |$O(n')$| Chỉ các chỉ mục xuất hiện trong các thao tác mới được lưu trữ | 

Giải pháp chạy thoải mái trong giới hạn vì$q \le 2 \cdot 10^5$và tất cả các hoạt động đều là tra cứu và gán từ điển theo thời gian liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    ops = [tuple(map(int, input().split())) for _ in range(q)]
    
    assigned = {}
    removed = 0

    for i in range(q - 1, -1, -1):
        idx, val = ops[i]
        if idx not in assigned:
            assigned[idx] = val
        elif assigned[idx] != val:
            removed += 1

    return str(removed)

# provided samples
assert run("5 6\n1 2\n2 3\n5 2\n5 1\n2 4\n4 6\n") == "2", "sample 1"
assert run("3 3\n1 2\n2 3\n3 2\n") == "0", "sample 2"

# minimum size
assert run("1 1\n1 5\n") == "0"

# all same index conflicts
assert run("1 4\n1 1\n1 2\n1 3\n1 4\n") == "3"

# already consistent distinct indices
assert run("5 3\n1 1\n2 2\n3 3\n") == "0"

# alternating values same index
assert run("2 5\n1 1\n1 2\n1 1\n1 2\n1 3\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoạt động đơn lẻ | 0 | tính đúng đắn của trường hợp cơ sở | 
| xung đột lặp đi lặp lại | 3 | xử lý nhiều lần ghi đè tại một chỉ mục | 
| chỉ số riêng biệt | 0 | không xóa không cần thiết | 
| mô hình xen kẽ | 3 | đúng đắn trong những xung đột đan xen | 

## Vỏ cạnh 

Trường hợp cạnh tinh vi xảy ra khi tất cả các thao tác nhắm mục tiêu vào một chỉ mục duy nhất nhưng có các giá trị thay thế. Trong trường hợp như vậy, chỉ giá trị cuối cùng gặp phải từ phía trước mới tồn tại. Ví dụ:```
1 4
1 1
1 2
1 3
1 4
```Xử lý ngược, trước tiên chúng tôi gán 4 làm giá trị cuối cùng. Mọi giá trị khác nhau trước đó đều trở nên không hợp lệ, do đó, ba lần xóa sẽ được tính. Thuật toán xử lý việc này một cách chính xác vì lần gặp ngược đầu tiên sẽ khắc phục trạng thái cuối cùng ngay lập tức. 

Một trường hợp khác là khi mỗi chỉ mục xuất hiện một lần. Vì không có xung đột nên mọi thao tác đều được chấp nhận. Quá trình quét ngược chỉ định mỗi chỉ mục chính xác một lần và không bao giờ gây ra sự không khớp, mang lại kết quả không bị xóa.
